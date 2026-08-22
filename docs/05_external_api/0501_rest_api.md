# 0501 REST APIの勉強

## REST API とは

REST API とは、Web 上でデータをやり取りするための設計ルールです。
業務では Laravel でバックエンドの API を作り、React からそれを呼び出す構成をとっています。
まずは REST API の基本的な概念を理解しましょう。

---

## 学ぶこと

### HTTPメソッド

API へのリクエストには用途に応じたメソッドを使います。

| メソッド | 用途 | 例 |
|----------|------|----|
| `GET` | データを取得する | ユーザー一覧を取得する |
| `POST` | データを新規作成する | 新しいユーザーを登録する |
| `PUT / PATCH` | データを更新する | ユーザー情報を変更する |
| `DELETE` | データを削除する | ユーザーを削除する |

### ステータスコード

レスポンスには処理結果を示すコードが含まれます。

| コード | 意味 |
|--------|------|
| `200` | OK（成功） |
| `201` | Created（作成成功） |
| `400` | Bad Request（リクエストが不正） |
| `401` | Unauthorized（認証が必要） |
| `404` | Not Found（リソースが存在しない） |
| `500` | Internal Server Error（サーバー側のエラー） |

### リクエストとレスポンス

API のやり取りは JSON 形式で行うことが多いです。

**リクエスト例（POST）**
```json
{
  "name": "山田太郎",
  "email": "yamada@example.com"
}
```

**レスポンス例**
```json
{
  "id": 1,
  "name": "山田太郎",
  "email": "yamada@example.com"
}
```

---

## 参考資料

- [REST API とは（MDN）](https://developer.mozilla.org/ja/docs/Glossary/REST)
- [HTTP ステータスコード一覧（MDN）](https://developer.mozilla.org/ja/docs/Web/HTTP/Status)

---

## チェックリスト

- [ ] GET / POST / PUT / DELETE の違いを説明できる
- [ ] ステータスコード 200・201・400・404・500 の意味を説明できる
- [ ] リクエストとレスポンスが JSON 形式でやり取りされることを理解している

---

## 口頭確認

> 内容を理解したら担当者に連絡し、通話を繋いで実施してください。
>
> **通話が繋がってから問題を開いてください。**

<details>
<summary>質問を見る</summary>

- [ ] ユーザー情報を取得したいとき、どの HTTP メソッドを使うか説明できる
- [ ] 404 と 500 の違いを説明できる
- [ ] なぜ API のやり取りに JSON を使うのか説明できる

</details>
