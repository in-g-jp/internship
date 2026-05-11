# 0601 好きなシステムを作る

## 課題

Laravel や外部 API を組み合わせて、作ってみたいシステムを自由に作成してください。
これまで学んだ知識を活かして、自分のアイデアで実装してみましょう。

---

## 進め方

### 1. 作りたい機能を REST API を基準にして整理する

まず「何ができるシステムか」を REST API の観点で整理します。
画面のイメージではなく、**どんなデータを・どう操作するか** を軸に考えるのがポイントです。

例）レシピ管理システムを作りたい場合
- レシピを登録できる
- レシピ一覧を見られる
- レシピの詳細を見られる
- レシピを削除できる

---

### 2. API 一覧を作る

整理した機能をもとに、API の一覧表を作ります。
以下の形式で書き出してみてください。

| メソッド | パス | 説明 |
|----------|------|------|
| GET | `/api/recipes` | レシピ一覧を取得 |
| POST | `/api/recipes` | レシピを新規作成 |
| GET | `/api/recipes/{id}` | レシピの詳細を取得 |
| DELETE | `/api/recipes/{id}` | レシピを削除 |

---

### 3. DB スキーマを設計する

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

### 4. 画面をざっくり作る

API 一覧をもとに、どんな画面が必要かを考えます。
UI ライブラリを活用して画面を作りましょう。

UI ライブラリには **Chakra UI** を使ってください（05章のセットアップ済み）。

まずは一覧画面・詳細画面など最低限の画面をざっくり作り、バックエンドと繋ぎながら完成度を上げていきましょう。

- [Chakra UI 公式ドキュメント](https://chakra-ui.com/)

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

```bash
sail artisan make:model Recipe
```

`app/Models/Recipe.php` に `fillable` を設定してください。

```php
protected $fillable = [
    'title',
    'description',
    'cooking_time',
];
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
