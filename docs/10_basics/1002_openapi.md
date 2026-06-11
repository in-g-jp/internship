# OpenAPI 基礎

## OpenAPIとは

フロントエンドとバックエンドの間で「このAPIはこう使う」という仕様を共有するためのドキュメント形式。YAML というフォーマットで書く。

たとえば「ユーザー一覧を取得するAPIを作りました」と口頭で伝えるだけでは、フロントエンドは「URLは？何が返ってくる？エラーのときは？」と都度確認しなければならない。OpenAPIでAPIの仕様を書いておくことで、そうした認識のズレを防ぎながら並行して開発できる。

---

## Stoplight Studio で書く

OpenAPI の YAML を手で書くのは大変なため、**Stoplight Studio** というGUIツールを使って書く。フォームに入力するだけでYAMLが自動生成されるため、YAMLの書き方を覚えていなくても作業できる。

### インストール

以下のURLを押下し、添付画像のようにGitHubページに遷移したことを確認してください。

<https://github.com/stoplightio/studio/releases>

![alt text](image.png)
タイトル横に緑文字でLatestと記載があることを確認し、stoplight-studio-mac-x64.dmg
をクリックしてください。

インストールされたアプリを開くと、ログイン画面に遷移します。

ワークスペースの新規作成を行うと、次のようなページに遷移します。
![alt text](image-1.png)

in-g.jpのメールアドレスを用いて新規登録してください。（workspace名はing-chibaのように任意で作成して問題ないです。）

再度、アプリケーションに戻り、作成したworkspace名を入力して完了です。

### 使い方

以下の動画を参考にすること。

<https://github.com/in-g-jp/manchoku-com/assets/127195391/1d81b348-62df-43c9-97f0-346817019fbe>

---

## YAMLの基本的な読み方

OpenAPIはYAMLで書く。YAMLは「インデント（字下げ）で階層を表す」フォーマット。

```yaml
openapi: 3.0.3
info:
  title: User API # infoの中にtitleがある
  version: 0.0.1 # infoの中にversionがある
```

インデントが揃っているものは同じ階層。インデントが深いものは、その上のキーに属している。上の例では `title` と `version` は `info` の中にある。

`-` で始まる行はリスト（配列）の要素を表す。

```yaml
servers:
  - url: "http://localhost/api" # 1つ目のサーバー
    description: ローカル環境
  - url: "https://dev.manchoku.com/api" # 2つ目のサーバー
    description: ステージング環境
```

---

## ファイル構成

1つのファイルにすべてを書くと膨大な量になるため、プロジェクトでは `spec/` ディレクトリ以下にファイルを分割して管理している。

```
spec/
  ├── user.yaml          # ユーザー向けAPIのエンドポイント一覧
  ├── agent.yaml         # 業者向けAPIのエンドポイント一覧
  ├── models/            # APIが返すデータの形（レスポンスの定義）
  ├── requests/          # APIに送るデータの形（リクエストの定義）
  └── enums/             # 決まった値の中から選ぶフィールドの定義
```

メインファイル（`user.yaml` など）にエンドポイントの一覧を書き、詳細なデータの形は `models/` や `requests/` に分けて書く。

---

## メインファイルの基本構造

```yaml
openapi: 3.0.3 # OpenAPIのバージョン（現在は3.0.3が主流）
info:
  title: User API
  description: ユーザーのAPI
  version: 0.0.1
servers:
  - url: "http://localhost/api"
    description: ローカル環境
  - url: "https://dev.manchoku.com/api"
    description: ステージング環境
paths:
  # ここにエンドポイントを書いていく
```

`paths` の中に個々のエンドポイントを書いていく。以降の例はすべてこの `paths` の中に入る。

---

## エンドポイントの書き方

### GETリクエスト（データの取得）

`GET /apartments` でマンション一覧を取得するエンドポイントの例。

```yaml
paths:
  /apartments: # エンドポイントのパス
    get: # HTTPメソッド（GET = データを取得する）
      summary: マンション一覧取得 # 何をするエンドポイントか
      operationId: get-apartments # ファイル内で一意なID
      tags:
        - Apartment # グループ名（ドキュメントの見た目の整理に使う）
      responses:
        "200": # HTTPステータスコード 200 = 成功
          description: OK
          content:
            application/json: # レスポンスの形式はJSON
              schema:
                type: object
                properties:
                  data:
                    type: array
                    items:
                      $ref: ./models/Apartment.yaml # 別ファイルを参照（後述）
                  meta:
                    $ref: ./others/Meta.yaml
```

### POSTリクエスト（データの作成）

`POST /register` で新規ユーザーを登録するエンドポイントの例。GETと違い、リクエストボディ（送信するデータ）の定義が必要になる。

```yaml
/register:
  post: # HTTPメソッド（POST = データを作成する）
    summary: 新規登録
    operationId: add-user
    tags:
      - Register
    requestBody: # 送信するデータの定義
      content:
        application/json:
          schema:
            $ref: ./requests/user/register/RegisterRequest.yaml
    responses:
      "200":
        description: OK
      "422":
        $ref: "#/components/responses/422" # 共通のエラーレスポンスを参照
```

### HTTPメソッドの使い分け

| メソッド | 用途         | operationIdの例                     |
| -------- | ------------ | ----------------------------------- |
| `get`    | データの取得 | `get-apartments` / `show-apartment` |
| `post`   | データの作成 | `add-viewing`                       |
| `put`    | データの更新 | `update-room`                       |
| `delete` | データの削除 | `delete-favorite`                   |

---

## パラメータの書き方

### クエリパラメータ（URLの `?` 以降の検索条件）

`GET /apartments?page=1&min_price=50000` のように、URLの末尾に `?` で追加する値のこと。

```yaml
/apartments:
  get:
    summary: マンション一覧取得
    parameters: # パラメータはリストで並べる
      - name: page # パラメータ名
        in: query # query = クエリパラメータ
        schema:
          type: integer
      - name: min_price
        in: query
        schema:
          type: integer
      - name: query # フリーワード検索など
        in: query
        schema:
          type: string
```

### パスパラメータ（URLに埋め込まれたID）

`GET /apartments/123` のように、URLの一部にIDが入るパターン。`{id}` のように `{}` で囲んで表す。

```yaml
"/apartments/{id}": # {}で囲んだ部分がパスパラメータ
  parameters:
    - name: id
      in: path # path = パスパラメータ
      required: true # パスパラメータは必ずrequired: trueにする
      schema:
        type: integer
  get:
    summary: マンション単一取得
    operationId: show-apartment
    tags:
      - Apartment
    responses:
      "200":
        description: OK
```

---

## スキーマ（データの形）の書き方

スキーマとはAPIでやり取りするデータの「形」を定義したもの。「このフィールドは文字列」「このフィールドは必須」といった情報を書く。

### 型の種類

| type      | 意味                                     | 値の例                           |
| --------- | ---------------------------------------- | -------------------------------- |
| `string`  | 文字列                                   | `"hello"` / `"test@example.com"` |
| `integer` | 整数                                     | `42` / `100`                     |
| `boolean` | 真偽値                                   | `true` / `false`                 |
| `array`   | 配列（複数の値）                         | `["PUBLIC", "PRIVATE"]`          |
| `object`  | オブジェクト（複数のフィールドの集まり） | `{"id": 1, "email": "..."}`      |

### シンプルなスキーマ

`required` に書いたフィールドは必須、書いていないフィールドは省略可能。

```yaml
# spec/requests/user/auth/LoginRequest.yaml
type: object
properties: # このオブジェクトが持つフィールドを列挙する
  email:
    type: string
    format: email # formatを指定すると「メール形式」などの条件を付けられる
  password:
    type: string
required: # 必須フィールドをリストで書く
  - email
  - password
```

上の定義は「`email`（メール形式の文字列）と `password`（文字列）を必ず送る」という意味になる。

### オブジェクトがネストしている場合

フィールドの中にさらにオブジェクトを持つ場合は、`type: object` の中に `properties` を書く。

```yaml
type: object
properties:
  agent_id:
    type: integer
  information: # informationというフィールド自体がオブジェクト
    type: object
    required:
      - floor
      - room_number
    properties:
      floor:
        type: integer
      room_number:
        type: string
```

### 配列の場合

`items` に「配列の各要素がどんな型か」を書く。

```yaml
type: object
properties:
  schedules:
    type: array
    minItems: 3 # 最低3件必要
    maxItems: 3 # 最大3件まで
    items:
      type: string # 配列の中身はstring型
```

### nullable（nullを許容する場合）

値が入っていない場合に `null` が返ってくるフィールドには `nullable: true` を付ける。

```yaml
date:
  type: string
  nullable: true # null が返ってくる可能性がある
```

---

## レスポンスのスキーマ（`models/` 以下）

APIが返すデータの形は `models/` フォルダに定義する。

```yaml
# spec/models/User.yaml
title: User
type: object
properties:
  id:
    type: integer
  email:
    type: string
    format: email
  is_active:
    type: boolean
  viewings_count: # 内見数。データがない場合はnullが返る
    type: integer
    nullable: true
required:
  - id
  - email
  - is_active
```

---

## Enum（選択肢が決まっているフィールド）

「PUBLIC か PRIVATE のどちらか」のように、値が決まっているフィールドは `enum` で定義する。

```yaml
# spec/enums/PublicStatus.yaml
title: PublicStatus
type: string
enum:
  - PUBLIC
  - PRIVATE
```

定義したEnumは `$ref` で参照する（後述）。

```yaml
properties:
  public_status:
    $ref: ../enums/PublicStatus.yaml
```

---

## $ref（別ファイルへの参照）

`$ref` を使うと、別ファイルに書いたスキーマを「参照」できる。同じ定義を何度も書かなくて済む。

```yaml
# 同じファイル内の components を参照する場合（# から始める）
$ref: '#/components/responses/422'

# 別ファイルを参照する場合（相対パスで書く）
$ref: ./models/User.yaml       # 同じフォルダ内
$ref: ../enums/UserType.yaml   # 1つ上の階層
$ref: ../../../enums/Layout.yaml  # 3つ上の階層（階層が深い場合は ../ を重ねる）
```

たとえば `User.yaml` というモデルを複数のエンドポイントが参照している場合、`$ref` で参照しておけばUser の定義を変えるだけですべてに反映される。

---

## components（共通のレスポンス定義）

複数のエンドポイントで同じエラーレスポンスを返す場合、毎回同じ定義を書くのは冗長になる。`components` にまとめておくと、`$ref` で使い回せる。

```yaml
components:
  responses:
    "422":
      description: バリデーションエラー # 送った値が不正だった場合
      content:
        application/json:
          schema:
            type: object
            properties:
              code:
                type: integer
                example: 422 # example は具体的な値の例示
              message:
                type: string
                example: 不正なリクエストです。
              errors:
                type: array
                items:
                  type: string
                  example: 選択されたメールアドレスは正しくありません。

  securitySchemes:
    BearerAuth:
      type: http
      scheme: bearer # Bearerトークン認証（ログイン後のAPIで使う）

security:
  - BearerAuth: [] # このファイルの全エンドポイントにBearerAuth認証を適用
```

エンドポイント側では以下のように参照する。

```yaml
responses:
  "422":
    $ref: "#/components/responses/422"
```

---

## OpenAPI と Laravel Resource の関係

OpenAPI の `models/` に書いたレスポンス定義と、Laravel の Resource クラスは**同じデータ構造を表している**。

- **OpenAPI の `models/`** → 「このAPIはこういうデータを返す」という仕様（ドキュメント）
- **Laravel の Resource** → 「このAPIが実際に返すデータを組み立てる」実装コード

つまり、OpenAPI で定義したフィールドと、Resource の `toArray()` で返すフィールドは一致していなければならない。

### データが返されるまでの流れ

```
ブラウザ / アプリ
      ↓  リクエスト送信
routes/api.php         ... URLとControllerを紐づける
      ↓
Controller             ... リクエストを受け取り、処理を呼び出す
      ↓
UseCase / Action       ... データベースからデータを取得する
      ↓
Resource (toArray)     ... 返すフィールドを選んで整形する
      ↓
JSON レスポンス        ... フロントエンドに届くデータ
```

OpenAPI の定義は、この最後の「JSON レスポンス」の形を仕様として書いたもの。

### 具体例：Image

**OpenAPI の定義（`spec/models/Image.yaml`）**

```yaml
title: Image
type: object
properties:
  path:
    type: string
  url:
    type: string
required:
  - path
  - url
```

**Laravel Resource（`app/Http/Resources/Image/ImageResource.php`）**

```php
public function toArray(Request $request): array
{
    $path = $this->path;

    return [
        'path' => Str::start($path, '/'),  // path フィールドを返す
        'url'  => url("/storage/$path"),   // url フィールドを返す
    ];
}
```

OpenAPI で `path` と `url` の2つを定義しているのに対し、Resource も同じ2フィールドを返している。

### 具体例：SearchCondition（少し複雑な例）

**OpenAPI の定義（`spec/models/SearchCondition.yaml`）**

```yaml
title: SearchCondition
type: object
properties:
  id:
    type: integer
  query:
    type: string
    nullable: true
  prefectures:
    type: array
    nullable: true
    items:
      $ref: ../enums/Prefecture.yaml
  min_price:
    type: integer
    nullable: true
  max_price:
    type: integer
    nullable: true
  created_at:
    type: string
required:
  - id
  - created_at
```

**Laravel Resource（`app/Http/Resources/SearchCondition/SearchConditionResource.php`）**

```php
public function toArray(Request $request): array
{
    return [
        'id'           => $this->id,
        'query'        => $this->query,        // nullable: true のフィールド
        'prefectures'  => $this->prefectures,  // 配列型のフィールド
        'min_price'    => $this->min_price,    // nullable: true のフィールド
        'max_price'    => $this->max_price,    // nullable: true のフィールド
        'created_at'   => $this->created_at,
    ];
}
```

### OpenAPI を書くときに意識すること

Resource の `toArray()` を見ながら OpenAPI を書く、または OpenAPI を先に書いてから Resource を実装する、どちらの順番でも構わない。大切なのは**両者のフィールドが一致していること**。

Resource を変えたら OpenAPI も更新する、OpenAPI を変えたら Resource も合わせる、という意識で作業するとよい。
