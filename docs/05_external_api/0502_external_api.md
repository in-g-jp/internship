# 0502 外部APIを叩く

## このステップで学ぶこと

Postman を使って実際の外部 API にリクエストを送り、データを取得する方法を学びます。
業務でも外部サービスの API を利用する場面があるため、基本的な流れを体験しておきましょう。

---

## 1. Postman のインストール

[https://www.postman.com/downloads/](https://www.postman.com/downloads/) からダウンロードしてインストールしてください。
アカウント登録を求められるので、GoogleやGitHubのアカウントを用いて新規作成してください。

---

## 2. 叩いてみる API 一覧

以下の API を順番に試してください。すべて登録不要・無料で使えます。

### 郵便番号 API（郵便局）

郵便番号から住所を取得できる API です。

| 項目 | 内容 |
|------|------|
| メソッド | `GET` |
| URL | `https://zipcloud.ibsnet.co.jp/api/search` |
| パラメータ | `zipcode=1000001` |

Postman の「Params」タブに `zipcode` / `1000001` を入力して送信してください。

---

### GitHub API

GitHub ユーザーの情報を取得できる API です。

| 項目 | 内容 |
|------|------|
| メソッド | `GET` |
| URL | `https://api.github.com/users/octocat` |

`octocat` の部分を自分の GitHub ユーザー名に変えて試してみてください。

---

### OpenWeatherMap API（天気）

世界中の都市の天気情報を取得できる API です。
利用には無料のアカウント登録と API キーの発行が必要です。

1. [https://openweathermap.org/](https://openweathermap.org/) でアカウント登録
2. API Keys ページで API キーを発行（反映まで数分かかることがあります）
3. 以下の URL でリクエストを送る

| 項目 | 内容 |
|------|------|
| メソッド | `GET` |
| URL | `https://api.openweathermap.org/data/2.5/weather` |
| パラメータ | `q=Tokyo` / `appid={発行したAPIキー}` / `units=metric` |

---

## 3. ベアラートークン認証

API によっては、リクエスト時に **ベアラートークン** を付与しないとアクセスできないものがあります。
ここでは GitHub API を使ってベアラートークンの仕組みを体験します。

### ベアラートークンとは

HTTP リクエストの `Authorization` ヘッダーにトークンを付与して認証する方式です。

```
Authorization: Bearer {トークン}
```

### Personal Access Token の発行

1. [https://github.com/settings/tokens](https://github.com/settings/tokens) を開く
2. 「Generate new token」→「Generate new token (classic)」をクリック
3. 「Note」に任意の名前を入力（例: `postman-test`）
4. 「Expiration」で有効期限を選択
5. スコープで `repo` と `read:user` にチェックを入れる
6. 「Generate token」をクリックしてトークンをコピー（再表示されないので必ず保存）

### Postman でリクエストを送る

認証なしでは取得できない自分のプライベートリポジトリ一覧を取得してみます。

| 項目 | 内容 |
|------|------|
| メソッド | `GET` |
| URL | `https://api.github.com/user/repos` |

Postman の「Authorization」タブで以下を設定してください。

| 項目 | 値 |
|------|------|
| Auth Type | `Bearer Token` |
| Token | 発行したトークンを貼り付け |

送信するとリポジトリ一覧が返ってきます。
トークンなしで同じリクエストを送ると `401 Unauthorized` が返ることも確認してください。

---

## チェックリスト

- [ ] Postman をインストールできる
- [ ] 郵便番号 API で住所を取得できる
- [ ] GitHub API で自分のユーザー情報を取得できる
- [ ] OpenWeatherMap API で東京の天気を取得できる
- [ ] GitHub Personal Access Token を発行できる
- [ ] ベアラートークンを使って自分のリポジトリ一覧を取得できる
- [ ] トークンなしで `401 Unauthorized` が返ることを確認できる

---

## 口頭確認

> 内容を理解したら担当者に連絡し、通話を繋いで実施してください。
>
> **通話が繋がってから問題を開いてください。**

<details>
<summary>質問を見る</summary>

- [ ] API キーとは何か、なぜ必要なのか説明できる
- [ ] クエリパラメータとは何か説明できる
- [ ] ベアラートークンとはどのような仕組みか説明できる
- [ ] `401 Unauthorized` はどのような場合に返ってくるか説明できる
- [ ] レスポンスの JSON をみて、必要なデータがどこにあるか読み取れる

</details>
