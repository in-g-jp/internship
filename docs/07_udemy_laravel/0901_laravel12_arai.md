# 0901 Udemy Laravel 講座 - 視聴順ガイド

**講座名**: 【第四弾】Laravel入門：メカニズムを深く理解して応用が効く知識を身につける
**作成者**: 新井元気

ドキュメント: https://iloveudemy.banana-quantum.com/basic/index.html

---

## 見なくていいもの

- **1〜2, 4**（イントロ・Q&A）: スキップ
- **9〜15**（環境構築・プロジェクト立ち上げ・Claude Code 補足）: 別途見る
- **24〜33**（ビュー・Blade）: スキップ
- **40〜41, 73〜78**（商品管理の実践）: Blade 実装部分はスキップ、Controller と Model の繋がりの確認のみ
- **44〜45**（リダイレクト・セッション実装）: スキップ
- **99〜107**（画像アップロード・ローカルストレージ実装）: スキップ
- **111〜117**（スクラッチ認証実装）: スキップ
- **121〜124**（カスタムミドルウェア・Blade 画面）: スキップ
- **125〜137**（TODO アプリ）: スキップ
- **138**（ボーナス）: スキップ

---

## 視聴順

### Step 0 — Laravel の概要

**3 → 5 → 6 → 7**

ドキュメント: [course-introduction.html](https://iloveudemy.banana-quantum.com/basic/course-introduction.html) / [index.html](https://iloveudemy.banana-quantum.com/basic/index.html)

---

### Step 1 — ルーティング

**16 → 18 → 19 → 20 → 21 → 22**

- 17（ビューを返すパターン）はスキップ

ドキュメント: [routing.html](https://iloveudemy.banana-quantum.com/basic/routing.html)

> `routes/api.php` / `routes/web.php` の使い分け、ルートパラメータ、HTTP メソッド（GET/POST/PUT/DELETE）、ルート名、ルートグループ、`artisan route:list`

---

### Step 2 — コントローラー + リクエスト

**34 → 35 → 36 → 37 → 38 → 39 → 42 → 43**

- 38（リソースコントローラー）は概念だけ把握すれば十分
- 40〜41 は Blade 部分をスキップしつつ Controller → Request の流れだけ確認

ドキュメント: [controller.html](https://iloveudemy.banana-quantum.com/basic/controller.html)

> MVC 設計、コントローラー作成（Artisan）、Request オブジェクト、バリデーション、依存性注入、CSRF

---

### Step 3 — マイグレーション

**47 → 49 → 50 → 51 → 52 → 53 → 54 → 55 → 56**

ドキュメント: [migration.html](https://iloveudemy.banana-quantum.com/basic/migration.html)

> カラム名・修飾子、`migrate` / `migrate:rollback`、外部キー制約（cascade / restrict）、シーダー

---

### Step 4 — モデル・Eloquent

**58 → 59 → 60 → 61 → 62 → 63 → 64 → 65 → 66 → 68 → 69**

- 67（1対1）は概念だけ

ドキュメント: [model.html](https://iloveudemy.banana-quantum.com/basic/model.html)

> `$fillable`、Tinker、CRUD メソッド、終端メソッド、N+1 / Eager Loading（`with()`）、1対多・多対多リレーション、`attach` / `detach` / `sync`

---

### Step 5 — ここまでの実践（流れ確認）

**71 → 72**

ドキュメント: [database-practice.html](https://iloveudemy.banana-quantum.com/basic/database-practice.html)

> Migration → Model → Controller の繋がりを俯瞰する（73〜78 の Blade 実装はスキップ）

---

### Step 6 — 検索・フィルタリング

**79 → 80 → 81 → 82 → 83 → 84**

ドキュメント: [search-filter.html](https://iloveudemy.banana-quantum.com/basic/search-filter.html)

> GET リクエスト、クエリパラメータ、LIKE 検索（部分一致・前方一致等）、カテゴリフィルタ、`when()` メソッド

---

### Step 7 — ソフトデリート

**86 → 87 → 88 → 89 → 90 → 91**

ドキュメント: [soft-delete.html](https://iloveudemy.banana-quantum.com/basic/soft-delete.html)

> `SoftDeletes` トレイト、`deleted_at` カラム、`withTrashed()` / `onlyTrashed()`、`restore()` / `forceDelete()`

---

### Step 8 — ページネーション

**93 → 94 → 95 → 96 → 97**

ドキュメント: [pagination.html](https://iloveudemy.banana-quantum.com/basic/pagination.html)

> `paginate()`、表示件数の動的変更、`orderBy()` によるソート、`appends()` でのクエリパラメータ保持

---

### Step 9 — 外部ストレージ（S3）

**108**（本番では外部ストレージを使用）

ドキュメント: [image-upload.html](https://iloveudemy.banana-quantum.com/basic/image-upload.html)（S3 の章のみ）

> 複数サーバー構成でローカルストレージを使えない理由、`store('products', 's3')` のように設定変更だけで S3 に切り替えられる仕組み

---

### Step 10 — 認証・ミドルウェア（概念把握のみ）

**110 → 119 → 120**

ドキュメント: [authentication.html](https://iloveudemy.banana-quantum.com/basic/authentication.html) / [authentication-middleware.html](https://iloveudemy.banana-quantum.com/basic/authentication-middleware.html)

> 認証ライブラリの比較（Breeze / Sanctum / Passport 等）、認証と認可の違い、ミドルウェアの仕組み（`handle()` / `$next($request)`）
