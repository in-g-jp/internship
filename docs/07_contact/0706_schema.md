# 0401 ログイン - DB スキーマ

## テーブル: users

Laravel のデフォルトの `users` テーブルをそのまま使用します。
マイグレーションはすでに存在しているため、新たに作成する必要はありません。

| カラム名 | 型 | NULL | 説明 |
|----------|----|------|------|
| id | BIGINT (PK) | NO | 自動採番 |
| name | VARCHAR(255) | NO | ユーザー名 |
| email | VARCHAR(255) | NO | メールアドレス（ユニーク） |
| password | VARCHAR(255) | NO | ハッシュ化済みパスワード |
| created_at | TIMESTAMP | NO | 作成日時 |
| updated_at | TIMESTAMP | NO | 更新日時 |

---

## 1. 管理者アカウントの作成（Seeder）

```bash
sail artisan make:seeder AdminUserSeeder
```

`database/seeders/AdminUserSeeder.php` を編集してください。

```php
use App\Models\User;
use Illuminate\Support\Facades\Hash;

public function run(): void
{
    User::create([
        'name'     => 'Admin',
        'email'    => 'admin@example.com',
        'password' => Hash::make('password'),
    ]);
}
```

---

## 2. Seeder の実行

```bash
sail artisan db:seed --class=AdminUserSeeder
```

---

## チェックリスト

- [ ] `users` テーブルに管理者アカウントが作成されている
- [ ] パスワードがハッシュ化されて保存されている

---

## 学習

### 今何をしたか

管理者がログインするためのアカウントを Seeder で作成しました。

| 用語 | 説明 |
|------|------|
| Seeder | 初期データを DB に投入するための仕組み |
| `Hash::make()` | パスワードをハッシュ化（元に戻せない形式に変換）する Laravel の関数 |
| ハッシュ化 | パスワードを安全に保存するための技術。平文（そのままの文字列）で保存してはいけない |

### 参考資料

- [Laravel シーディング](https://readouble.com/laravel/12.x/ja/seeding.html)
- [パスワードのハッシュ化とは](https://www.ipa.go.jp/security/vuln/websecurity/password.html)
