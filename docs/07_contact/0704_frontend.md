# 0304 お問い合わせ - フロントエンド実装

## 画面構成

| 画面 | パス | 説明 |
|------|------|------|
| お問い合わせフォーム | `/contact` | 入力フォーム |
| 送信完了 | `/contact/complete` | 送信完了メッセージ |

---

## 1. ルーティングの設定

React Router をインストールしてください。

```bash
sail npm install react-router-dom
```

`resources/ts/main.tsx` にルーティングを設定してください。

```tsx
import { BrowserRouter, Routes, Route } from 'react-router-dom'
import ContactPage from './pages/contact/ContactPage'
import CompletePage from './pages/contact/CompletePage'

<BrowserRouter>
    <Routes>
        <Route path="/contact" element={<ContactPage />} />
        <Route path="/contact/complete" element={<CompletePage />} />
    </Routes>
</BrowserRouter>
```

---

## 2. フォームの実装

`resources/ts/pages/contact/ContactPage.tsx` を作成してください。

フォームには以下のフィールドを設けてください。

| フィールド | 必須 |
|------------|------|
| 氏名 | ◎ |
| 会社名 | - |
| メールアドレス | ◎ |
| 問い合わせ内容 | ◎ |

---

## 3. API 呼び出し

```tsx
const onSubmit = async (data: ContactFormValues) => {
    const res = await fetch('/api/contacts', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(data),
    })

    if (res.ok) {
        navigate('/contact/complete')
    }
}
```

---

## 4. 送信完了画面

`resources/ts/pages/contact/CompletePage.tsx` を作成してください。
送信完了のメッセージを表示します。

---

## チェックリスト

- [ ] フォームが表示される
- [ ] 送信するとデータが API に送られる
- [ ] 送信完了画面に遷移する

---

## 学習

### 今何をしたか

お問い合わせフォームの画面を React で実装しました。

| 用語 | 説明 |
|------|------|
| React Router | React でページ遷移（ルーティング）を実現するライブラリ |
| `fetch` | ブラウザ標準の HTTP リクエスト関数 |
| SPA | Single Page Application。ページ全体をリロードせず画面を切り替えるアーキテクチャ |

### 参考資料

- [React Router 公式ドキュメント](https://reactrouter.com/en/main)
- [Fetch API（MDN）](https://developer.mozilla.org/ja/docs/Web/API/Fetch_API/Using_Fetch)
- [Chakra UI フォームコンポーネント](https://chakra-ui.com/docs/components/form-control)
