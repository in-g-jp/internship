# 0404 ログイン - フロントエンド実装

## 画面構成

| 画面 | パス | 説明 |
|------|------|------|
| ログイン | `/login` | メールアドレス・パスワード入力 |

---

## 1. ログイン画面の作成

`resources/ts/pages/auth/LoginPage.tsx` を作成してください。

フォームには以下のフィールドを設けてください。

| フィールド | 必須 |
|------------|------|
| メールアドレス | ◎ |
| パスワード | ◎ |

---

## 2. API 呼び出し

```tsx
const onSubmit = async (data: LoginFormValues) => {
    const res = await fetch('/api/login', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(data),
    })

    if (res.ok) {
        navigate('/admin/contacts')
    } else {
        setErrorMessage('メールアドレスまたはパスワードが正しくありません。')
    }
}
```

---

## 3. ログイン状態の管理

ログイン状態は React の Context で管理してください。

```tsx
const AuthContext = createContext<{ isAuthenticated: boolean } | null>(null)
```

ログイン成功後に `isAuthenticated` を `true` に更新し、ログアウト時に `false` に戻します。

---

## チェックリスト

- [ ] ログイン画面が表示される
- [ ] ログイン成功後に `/admin/contacts` に遷移する
- [ ] ログイン失敗時にエラーメッセージが表示される

---

## 学習

### 今何をしたか

管理者のログイン画面を React で実装し、認証状態を Context で管理できるようにしました。

| 用語 | 説明 |
|------|------|
| Context | React でグローバルな状態（ログイン情報など）を管理する仕組み |
| 認証ガード | 未ログイン状態でアクセスを防ぐ処理。次のステップ（0402）で実装する |

### 参考資料

- [React Context 公式ドキュメント](https://ja.react.dev/reference/react/createContext)
- [Chakra UI フォームコンポーネント](https://chakra-ui.com/docs/components/form-control)
