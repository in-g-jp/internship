# 0402 外部APIを叩く

## このステップで学ぶこと

Postman を使って実際の外部 API にリクエストを送り、データを取得する方法を学びます。
業務でも外部サービスの API を利用する場面があるため、基本的な流れを体験しておきましょう。

---

## 1. Postman のインストール

[https://www.postman.com/downloads/](https://www.postman.com/downloads/) からダウンロードしてインストールしてください。
アカウント登録を求められますが、「Skip and go to the app」でスキップできます。

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

## チェックリスト

- [ ] Postman をインストールできる
- [ ] 郵便番号 API で住所を取得できる
- [ ] GitHub API で自分のユーザー情報を取得できる
- [ ] OpenWeatherMap API で東京の天気を取得できる

---

## 口頭確認

> 内容を理解したら担当者に連絡し、通話を繋いで実施してください。
>
> **通話が繋がってから問題を開いてください。**

<details>
<summary>質問を見る</summary>

- [ ] API キーとは何か、なぜ必要なのか説明できる
- [ ] クエリパラメータとは何か説明できる
- [ ] レスポンスの JSON をみて、必要なデータがどこにあるか読み取れる

</details>
