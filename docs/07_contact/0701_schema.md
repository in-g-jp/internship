# 0301 お問い合わせ - DB スキーマ

## 1. テーブル設計

| カラム名 | 型 | NULL | 説明 |
|----------|----|------|------|
| id | BIGINT (PK) | NO | 自動採番 |
| name | VARCHAR(255) | NO | 氏名 |
| company | VARCHAR(255) | YES | 会社名 |
| email | VARCHAR(255) | NO | メールアドレス |
| message | TEXT | NO | 問い合わせ内容 |
| created_at | TIMESTAMP | NO | 問い合わせ日時 |
| updated_at | TIMESTAMP | NO | 更新日時 |

---

## 2. マイグレーションの作成

```bash
sail artisan make:migration create_contacts_table
```

`database/migrations/xxxx_create_contacts_table.php` を編集してください。

```php
public function up(): void
{
    Schema::create('contacts', function (Blueprint $table) {
        $table->id();
        $table->string('name');
        $table->string('company')->nullable();
        $table->string('email');
        $table->text('message');
        $table->timestamps();
    });
}
```

---

## 3. マイグレーションの実行

```bash
sail artisan migrate
```

---

## チェックリスト

- [ ] マイグレーションファイルが作成されている
- [ ] `sail artisan migrate` が成功している
- [ ] DB に `contacts` テーブルが作成されている

---

## 学習

### 今何をしたか

お問い合わせデータを保存するためのテーブルを設計し、マイグレーションで作成しました。

| 用語 | 説明 |
|------|------|
| マイグレーション | DB のテーブル構造をコードで定義・管理する仕組み。チームで同じ DB 構造を共有できる |
| `nullable()` | NULL を許可するカラム。会社名は任意入力のため設定する |
| `timestamps()` | `created_at` と `updated_at` を自動で追加するメソッド |

### 参考資料

- [Laravel マイグレーション](https://readouble.com/laravel/12.x/ja/migrations.html)
- [データベース設計の基本（主キー・NULL・型）](https://www.ipa.go.jp/files/000018652.pdf)
