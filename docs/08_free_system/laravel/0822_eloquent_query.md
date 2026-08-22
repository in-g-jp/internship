# 0822 実装のチップス - Eloquent を使った絞り込み・ソート・ページネーション

## この Tips について

0827 で実装した管理者向けお問い合わせ一覧 API では、すべてのデータを取得していました。

```php
// 0827 の実装
$contacts = Contact::orderBy('created_at', 'desc')->get();
```

この Tips では、検索・ソート・ページネーションを追加する方法を学びます。

---

## 1. where() で絞り込む

### 完全一致

```php
// email が一致するレコードのみ取得
Contact::where('email', 'yamada@example.com')->get();
```

### 部分一致（LIKE 検索）

```php
// name に「山田」を含むレコードを取得
Contact::where('name', 'like', '%山田%')->get();
```

`%` はワイルドカード。`%山田%` は前後どこにあっても一致します。

### 複数条件（AND）

```php
Contact::where('name', 'like', '%山田%')
    ->where('email', 'like', '%example.com%')
    ->get();
```

---

## 2. when() で条件付きクエリを書く

リクエストパラメータがある場合だけ絞り込むとき、`if` を並べると読みにくくなります。
`when()` を使うとクエリを1つにまとめて書けます。

**if を使った書き方（避けたい）**

```php
$query = Contact::query()->orderBy('created_at', 'desc');

if ($request->has('name')) {
    $query->where('name', 'like', "%{$request->name}%");
}

if ($request->has('email')) {
    $query->where('email', 'like', "%{$request->email}%");
}

$contacts = $query->get();
```

**when() を使った書き方（推奨）**

```php
$contacts = Contact::query()
    ->when($request->name, function ($query, $name) {
        $query->where('name', 'like', "%{$name}%");
    })
    ->when($request->email, function ($query, $email) {
        $query->where('email', 'like', "%{$email}%");
    })
    ->orderBy('created_at', 'desc')
    ->get();
```

`when()` の第1引数が truthy のときだけ、第2引数のクロージャが実行されます。

---

## 3. orderBy() でソートする

```php
// 作成日時の降順（新しい順）
Contact::orderBy('created_at', 'desc')->get();

// 作成日時の昇順（古い順）
Contact::orderBy('created_at', 'asc')->get();
```

リクエストパラメータでソート方向を切り替える例：

```php
$direction = $request->input('sort', 'desc'); // デフォルトは降順

Contact::orderBy('created_at', $direction)->get();
```

---

## 4. paginate() でページネーションする

`get()` の代わりに `paginate()` を使うと、ページごとに分割して取得できます。

```php
// 1ページあたり15件で取得
$contacts = Contact::orderBy('created_at', 'desc')->paginate(15);
```

レスポンスに自動でページネーション情報が付与されます。

```json
{
    "data": [...],
    "links": {
        "first": "http://localhost/api/admin/contacts?page=1",
        "last": "http://localhost/api/admin/contacts?page=3",
        "prev": null,
        "next": "http://localhost/api/admin/contacts?page=2"
    },
    "meta": {
        "current_page": 1,
        "last_page": 3,
        "per_page": 15,
        "total": 42
    }
}
```

ページ番号は URL クエリパラメータで指定します。

```
GET /api/admin/contacts?page=2
```

---

## 5. まとめ：検索・ソート・ページネーションを組み合わせる

```php
public function index(Request $request): JsonResponse
{
    $contacts = Contact::query()
        ->when($request->name, function ($query, $name) {
            $query->where('name', 'like', "%{$name}%");
        })
        ->when($request->email, function ($query, $email) {
            $query->where('email', 'like', "%{$email}%");
        })
        ->orderBy('created_at', 'desc')
        ->paginate(15);

    return response()->json($contacts);
}
```

---

## 6. 【発展】with() で N+1 問題を防ぐ

モデルにリレーション（`hasMany` / `belongsTo` など）がある場合、
ループ内でリレーションを参照すると N+1 クエリ問題が起きます。

```php
// NG: contacts の件数分だけ SQL が発行される
$contacts = Contact::all();
foreach ($contacts as $contact) {
    $contact->user; // ← ここで毎回 SQL が実行される
}
```

`with()` を使うと関連データをまとめて取得（Eager Loading）できます。

```php
// OK: SQL は2回だけ
$contacts = Contact::with('user')->get();
```

---

## チェックリスト

- [ ] `where()` で名前・メールアドレスの部分一致検索ができる
- [ ] `when()` でパラメータがある場合だけ絞り込みが適用される
- [ ] `orderBy()` でソートできる
- [ ] `paginate()` でページネーション情報付きのレスポンスが返る

---

## 学習

### 今何をしたか

Eloquent クエリビルダを使って、一覧取得 API に検索・ソート・ページネーション機能を追加しました。

| 用語 | 説明 |
|------|------|
| クエリビルダ | SQL を PHP のメソッドチェーンで組み立てる Eloquent の機能 |
| `when()` | 第1引数が truthy のときだけクロージャを実行するメソッド |
| `paginate()` | 件数を分割して取得するメソッド。`meta` や `links` も自動付与される |
| N+1 問題 | ループ内でリレーションを参照すると N 件分の SQL が追加発行される問題 |
| Eager Loading | `with()` で関連データをまとめて取得し、N+1 問題を防ぐ手法 |

### 参考資料

- [Laravel Eloquent - クエリビルダ](https://readouble.com/laravel/12.x/ja/eloquent.html)
- [Laravel ペジネーション](https://readouble.com/laravel/12.x/ja/pagination.html)
- [Laravel クエリビルダ - when()](https://readouble.com/laravel/12.x/ja/queries.html#conditional-clauses)
