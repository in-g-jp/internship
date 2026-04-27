# 0303 お問い合わせ - バックエンドバリデーション

## 1. FormRequest の作成

```bash
sail artisan make:request StoreContactRequest
```

---

## 2. バリデーションルールの実装

`app/Http/Requests/StoreContactRequest.php` を編集してください。

```php
public function authorize(): bool
{
    return true;
}

public function rules(): array
{
    return [
        'name'    => ['required', 'string', 'max:255'],
        'company' => ['nullable', 'string', 'max:255'],
        'email'   => ['required', 'email', 'max:255'],
        'message' => ['required', 'string'],
    ];
}

public function messages(): array
{
    return [
        'name.required'    => '氏名は必須です。',
        'email.required'   => 'メールアドレスは必須です。',
        'email.email'      => 'メールアドレスの形式が正しくありません。',
        'message.required' => '問い合わせ内容は必須です。',
    ];
}
```

---

## 3. Model の fillable 設定

`app/Models/Contact.php` に追加してください。

```php
protected $fillable = [
    'name',
    'company',
    'email',
    'message',
];
```

---

## チェックリスト

- [ ] 必須項目を空にして送信すると 422 エラーが返る
- [ ] メールアドレスの形式が不正なとき 422 エラーが返る
- [ ] 正しいデータを送信すると 201 が返る

---

## 学習

### 今何をしたか

API に送られてきたデータをサーバー側でチェック（バリデーション）する処理を実装しました。

| 用語 | 説明 |
|------|------|
| FormRequest | バリデーションロジックを Controller から切り出して管理するクラス |
| `required` | 必須項目のルール |
| `nullable` | NULL（未入力）を許可するルール |
| `fillable` | `create()` や `fill()` で一括代入できるカラムを制限するセキュリティ設定 |
| 422 | バリデーションエラー時に返す HTTP ステータスコード |

### 参考資料

- [Laravel バリデーション](https://readouble.com/laravel/12.x/ja/validation.html)
- [Laravel Eloquent - 複数代入](https://readouble.com/laravel/12.x/ja/eloquent.html#mass-assignment)
