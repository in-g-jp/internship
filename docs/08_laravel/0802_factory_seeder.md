# 0802 実装のチップス - Factory と Seeder を使ったテストデータの一括生成

## Factory・Seeder とは

| 用語 | 説明 |
|------|------|
| Factory | モデルのダミーデータを自動生成する設定クラス。`fake()` でランダムな値を作る |
| Seeder | DB にデータを投入するクラス。Factory と組み合わせて大量のテストデータを作れる |

Seeder には2種類の使い方があります。

| 種類 | 用途 | 例 |
|------|------|-----|
| Factory を使う | ランダムな値で大量のテストデータを生成する | お問い合わせダミーデータ |
| 固定値で直接作成 | 特定のアカウントなど決まった値を1件だけ投入する | 管理者アカウント |

---

## 1. Factory の作成

```bash
sail artisan make:factory ContactFactory
```

`database/factories/ContactFactory.php` を編集してください。

```php
public function definition(): array
{
    return [
        'name'    => fake()->name(),
        'company' => fake()->company(),
        'email'   => fake()->unique()->safeEmail(),
        'message' => fake()->realText(),
    ];
}
```

`company` は任意項目（nullable）なので、NULL を含めたい場合は次のように書きます。

```php
'company' => fake()->optional()->company(),
```

---

## 2. Model に HasFactory を追加

`app/Models/Contact.php` に `HasFactory` トレイトが含まれているか確認してください。

```php
use Illuminate\Database\Eloquent\Factories\HasFactory;

class Contact extends Model
{
    use HasFactory;

    protected $fillable = [
        'name',
        'company',
        'email',
        'message',
    ];
}
```

---

## 3. Seeder の作成

```bash
sail artisan make:seeder ContactSeeder
```

`database/seeders/ContactSeeder.php` を編集してください。

```php
use App\Models\Contact;

public function run(): void
{
    Contact::factory()
        ->count(20)
        ->create();
}
```

---

## 4. 固定値 Seeder の作成（AdminUserSeeder）

管理者アカウントのように「決まった値で1件だけ作りたい」場合は、Factory を使わずに直接 `create()` します。
パスワードは必ず `Hash::make()` でハッシュ化してください。平文で保存してはいけません。

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

## 5. DatabaseSeeder への登録

`database/seeders/DatabaseSeeder.php` に追加してください。
複数の Seeder をまとめて管理できます。

```php
public function run(): void
{
    $this->call([
        AdminUserSeeder::class,
        ContactSeeder::class,
    ]);
}
```

---

## 6. 実行

**すべての Seeder を実行する**

```bash
sail artisan db:seed
```

**特定の Seeder だけ実行する**

```bash
sail artisan db:seed --class=AdminUserSeeder
```

**マイグレーションをリセットしてからすべて実行し直す**

```bash
sail artisan migrate:fresh --seed
```

開発中はデータをリセットしたいことが多いため、`migrate:fresh --seed` をよく使います。

---

## チェックリスト

- [ ] `ContactFactory` が作成されている
- [ ] `Contact` モデルに `HasFactory` が含まれている
- [ ] `sail artisan db:seed` で `contacts` テーブルにデータが20件入る
- [ ] `AdminUserSeeder` で管理者アカウントが作成され、パスワードがハッシュ化されている
- [ ] `migrate:fresh --seed` で DB をリセット後にデータが再投入される

---

## 学習

### 今何をしたか

Factory と Seeder を使い、ランダムなお問い合わせデータを大量に生成できるようにしました。
管理者画面の一覧表示の動作確認など、手入力では用意しにくいデータを素早く準備できます。

| 用語 | 説明 |
|------|------|
| `fake()` | Faker ライブラリの関数。名前・メール・文章などランダムな値を生成する |
| `unique()` | 同じ値が重複しないように生成するオプション |
| `optional()` | 一定の確率で `null` を返すオプション。nullable カラムに使う |
| `count()` | 生成するレコード数を指定する |
| `Hash::make()` | パスワードをハッシュ化（元に戻せない形式に変換）する Laravel の関数 |
| ハッシュ化 | パスワードを安全に保存するための技術。平文で保存してはいけない |
| `migrate:fresh` | すべてのテーブルを削除して再マイグレーションするコマンド |

### 参考資料

- [Laravel Factory](https://readouble.com/laravel/12.x/ja/eloquent-factories.html)
- [Laravel Seeder](https://readouble.com/laravel/12.x/ja/seeding.html)
- [Faker ドキュメント](https://fakerphp.org/)
- [パスワードのハッシュ化とは](https://www.ipa.go.jp/security/vuln/websecurity/password.html)
