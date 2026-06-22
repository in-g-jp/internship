# 0602 好きなシステムを作る

## 課題

Laravel や外部 API を組み合わせて、作ってみたいシステムを自由に作成してください。
これまで学んだ知識を活かして、自分のアイデアで実装してみましょう。

---

## 進め方

### 1. 画面をざっくり作る

まず作りたいシステムの画面を React で作りましょう。
最初はダミーデータを使って画面の見た目だけ作り、バックエンドとは繋がない状態で進めます。

UI ライブラリには **Chakra UI** を使ってください（0601章のセットアップ済み）。

一覧画面・入力フォームなど最低限の画面をざっくり作り、どんな画面が必要かを掴みましょう。

- [Chakra UI 公式ドキュメント](https://chakra-ui.com/)

---

### 2. 作りたい機能を REST API を基準にして整理する

画面ができたら「何ができるシステムか」を REST API の観点で整理します。
**どんなデータを・どう操作するか** を軸に考えるのがポイントです。

例）レシピ管理システムを作りたい場合
- レシピを登録できる
- レシピ一覧を見られる
- レシピの詳細を見られる
- レシピを削除できる

---

### 3. API 一覧を作る

整理した機能をもとに、API の一覧表を作ります。
以下の形式で書き出してみてください。

| メソッド | パス | 説明 |
|----------|------|------|
| GET | `/api/recipes` | レシピ一覧を取得 |
| POST | `/api/recipes` | レシピを新規作成 |
| GET | `/api/recipes/{id}` | レシピの詳細を取得 |
| DELETE | `/api/recipes/{id}` | レシピを削除 |

---

### 4. DB スキーマを設計する

API 一覧をもとに、必要なテーブルとカラムを設計します。

#### テーブル設計の考え方

- API で扱う「もの」（レシピ・ユーザーなど）がテーブルになる
- 各テーブルには `id`・`created_at`・`updated_at` を必ず持たせる
- 複数の値を持つもの（タグなど）は別テーブルに分ける

例）レシピテーブル

| カラム名 | 型 | 説明 |
|----------|----|------|
| `id` | `bigIncrements` | 主キー |
| `title` | `string` | レシピ名 |
| `description` | `text` | 説明 |
| `cooking_time` | `integer` | 調理時間（分） |
| `created_at` | `timestamp` | 作成日時 |
| `updated_at` | `timestamp` | 更新日時 |

#### よく使うカラムの型

| 型 | 使いどころ |
|----|-----------|
| `string` | 短いテキスト（名前・タイトルなど） |
| `text` | 長いテキスト（本文・説明など） |
| `integer` | 整数（個数・時間など） |
| `boolean` | 真偽値（フラグなど） |
| `foreignId` | 他テーブルへの参照（外部キー） |
| `nullable()` | NULL を許可する |

---

### 5. バックエンドの実装

設計をもとにバックエンドを実装します。以下の順番で進めてください。

#### 5-1. マイグレーションの作成

```bash
sail artisan make:migration create_recipes_table
```

`database/migrations/xxxx_create_recipes_table.php` を編集してください。

```php
public function up(): void
{
    Schema::create('recipes', function (Blueprint $table) {
        $table->id();
        $table->string('title');
        $table->text('description')->nullable();
        $table->integer('cooking_time');
        $table->timestamps();
    });
}
```

マイグレーションを実行してください。

```bash
sail artisan migrate
```

---

#### 5-2. モデルの作成

**モデルとは？**

モデルは、データベースのテーブルと PHP のコードを紐づける役割を持つファイルです。
モデルを作成することで、SQL を直接書かずに PHP のコードでデータの取得・作成・更新・削除ができるようになります。

例えば `Recipe` モデルを作ると、以下のように書けます。

```php
// レシピを全件取得する
Recipe::all();

// レシピを1件作成する
Recipe::create(['title' => 'カレー', 'cooking_time' => 30]);

// id が 1 のレシピを取得する
Recipe::find(1);
```

モデルファイルは `app/Models/` に作成されます。

Recipe モデルと同時にマイグレーション・ファクトリ・シーダーを作成してください。

```bash
sail artisan make:model Recipe -mfs
```

**`-m`（マイグレーション）**

`database/migrations/` にマイグレーションファイルが作成されます。
テーブルのカラム定義を記述し、`sail artisan migrate` を実行することでDBにテーブルが作成されます。

**`-f`（ファクトリ）**

`database/factories/` にファクトリファイルが作成されます。
テスト用のダミーデータを簡単に生成するための仕組みです。
たとえば開発中に「レシピを100件まとめて作りたい」といった場面で活用できます。

```php
// レシピのダミーデータを10件作成する
Recipe::factory()->count(10)->create();
```

**`-s`（シーダー）**

`database/seeders/` にシーダーファイルが作成されます。
データベースに初期データを投入するための仕組みです。
`sail artisan db:seed` を実行することでシーダーに書いた内容がDBに登録されます。

```bash
# シーダーを実行してDBに初期データを投入する
sail artisan db:seed
```

---

#### 5-3. コントローラーの作成

機能ごとにコントローラーを分けて作成してください。

```bash
sail artisan make:controller Api/Recipe/IndexController
sail artisan make:controller Api/Recipe/StoreController
sail artisan make:controller Api/Recipe/ShowController
sail artisan make:controller Api/Recipe/DestroyController
```

---

#### 5-4. ルーティングの設定

`routes/api.php` に追加してください。

```php
Route::get('/recipes', IndexController::class);
Route::post('/recipes', StoreController::class);
Route::get('/recipes/{recipe}', ShowController::class);
Route::delete('/recipes/{recipe}', DestroyController::class);
```

---

#### 5-5. Postman で動作確認

実装したエンドポイントを Postman で順番に確認してください。

| 確認内容 | URL | 期待するステータス |
|---------|-----|--------------------|
| 一覧取得 | `GET /api/recipes` | `200` |
| 新規作成 | `POST /api/recipes` | `201` |
| 詳細取得 | `GET /api/recipes/1` | `200` |
| 削除 | `DELETE /api/recipes/1` | `204` |

---

## 本番サーバーへのデプロイ

作成したシステムを本番サーバーへデプロイしたい場合は、以下の Wiki を参考にしてください。

[勉強用サーバー情報 - Backlog Wiki](https://nextstep.backlog.jp/alias/wiki/1076153846)
