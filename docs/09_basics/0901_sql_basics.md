# 0901 実装のチップス - SQL の基本（SELECT・WHERE・INSERT・UPDATE・DELETE）

## SQL とは

SQL（Structured Query Language）は、データベースを操作するための言語です。
Laravel の Eloquent を使うと SQL を直接書かずにデータを操作できますが、
裏側では必ず SQL が実行されています。

SQL の基本を知ることで、Eloquent のコードが何をしているか理解しやすくなります。

---

## 動作確認の方法

以下のコマンドで MySQL に接続して SQL を直接実行できます。

```bash
sail mysql
```

接続後、データベースを選択してください。

```sql
USE laravel;
```

---

## 1. SELECT — データを取得する

```sql
-- テーブルの全データを取得
SELECT * FROM contacts;

-- 特定のカラムだけ取得
SELECT name, email FROM contacts;
```

**Eloquent との対応**

```php
Contact::all();                    -- SELECT * FROM contacts
Contact::select('name', 'email')->get();  -- SELECT name, email FROM contacts
```

---

## 2. WHERE — 条件で絞り込む

```sql
-- email が一致するレコードを取得
SELECT * FROM contacts WHERE email = 'yamada@example.com';

-- name に「山田」を含むレコードを取得（部分一致）
SELECT * FROM contacts WHERE name LIKE '%山田%';

-- 複数条件（AND）
SELECT * FROM contacts WHERE name LIKE '%山田%' AND email LIKE '%example.com%';
```

**Eloquent との対応**

```php
Contact::where('email', 'yamada@example.com')->get();
Contact::where('name', 'like', '%山田%')->get();
```

---

## 3. ORDER BY / LIMIT — 並び替えと件数制限

```sql
-- 作成日時の降順（新しい順）で取得
SELECT * FROM contacts ORDER BY created_at DESC;

-- 上位5件だけ取得
SELECT * FROM contacts ORDER BY created_at DESC LIMIT 5;
```

**Eloquent との対応**

```php
Contact::orderBy('created_at', 'desc')->get();
Contact::orderBy('created_at', 'desc')->take(5)->get();
```

---

## 4. INSERT — データを追加する

```sql
INSERT INTO contacts (name, email, message, created_at, updated_at)
VALUES ('山田 太郎', 'yamada@example.com', 'お問い合わせ内容', NOW(), NOW());
```

**Eloquent との対応**

```php
Contact::create([
    'name'    => '山田 太郎',
    'email'   => 'yamada@example.com',
    'message' => 'お問い合わせ内容',
]);
```

---

## 5. UPDATE — データを更新する

```sql
-- id = 1 のレコードの name を更新
UPDATE contacts SET name = '鈴木 一郎' WHERE id = 1;
```

`WHERE` を忘れるとテーブルの全レコードが更新されるので注意してください。

**Eloquent との対応**

```php
Contact::where('id', 1)->update(['name' => '鈴木 一郎']);
```

---

## 6. DELETE — データを削除する

```sql
-- id = 1 のレコードを削除
DELETE FROM contacts WHERE id = 1;
```

`WHERE` を忘れるとテーブルの全レコードが削除されるので注意してください。

**Eloquent との対応**

```php
Contact::where('id', 1)->delete();
```

---

## 7. JOIN — テーブルを結合する

複数のテーブルを結合してデータを取得できます。
たとえば `contacts` と `users` を user_id で結合する例です。

```sql
SELECT contacts.name, contacts.email, users.name AS user_name
FROM contacts
INNER JOIN users ON contacts.user_id = users.id;
```

| 種類 | 説明 |
|------|------|
| `INNER JOIN` | 両方のテーブルに一致するレコードだけ取得する |
| `LEFT JOIN` | 左テーブルの全レコードを取得し、右テーブルは一致しない場合 NULL になる |

**Eloquent との対応**

```php
Contact::join('users', 'contacts.user_id', '=', 'users.id')
    ->select('contacts.name', 'contacts.email', 'users.name as user_name')
    ->get();
```

---

## チェックリスト

- [ ] `SELECT * FROM contacts` で一覧が取得できる
- [ ] `WHERE` で条件を指定して絞り込める
- [ ] `INSERT` でレコードを追加できる
- [ ] `UPDATE` で特定のレコードを更新できる（`WHERE` を必ず付ける）
- [ ] `DELETE` で特定のレコードを削除できる（`WHERE` を必ず付ける）

---

## 学習

### 今何をしたか

データベースを直接操作する SQL の基本を学びました。
Eloquent は SQL を PHP のメソッドで表現したものなので、対応関係を意識すると理解が深まります。

| 用語 | 説明 |
|------|------|
| SQL | データベースを操作するための言語 |
| `SELECT` | データを取得する命令 |
| `WHERE` | 条件を指定して絞り込む句 |
| `INSERT` | データを追加する命令 |
| `UPDATE` | データを更新する命令 |
| `DELETE` | データを削除する命令 |
| `ORDER BY` | 結果を並び替える句 |
| `LIMIT` | 取得件数を制限する句 |
| `JOIN` | 複数テーブルを結合して取得する句 |
| `LIKE` | 部分一致検索に使う演算子。`%` がワイルドカード |
| `NOW()` | 現在日時を返す SQL 関数 |

### 参考資料

- [SQL 入門（MySQL 公式）](https://dev.mysql.com/doc/refman/8.0/ja/tutorial.html)
- [Laravel Eloquent - クエリビルダ](https://readouble.com/laravel/12.x/ja/eloquent.html)
