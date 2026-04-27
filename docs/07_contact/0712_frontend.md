# 0502 管理者 - フロントエンド実装

## 画面構成

| 画面 | パス | 説明 |
|------|------|------|
| お問い合わせ一覧 | `/admin/contacts` | 問い合わせ一覧テーブル |

---

## 1. アクセス制御（PrivateRoute）

未ログイン状態で `/admin/contacts` にアクセスした場合、`/login` にリダイレクトするコンポーネントを作成してください。

```tsx
const PrivateRoute = ({ children }: { children: React.ReactNode }) => {
    const { isAuthenticated } = useAuth()

    if (!isAuthenticated) {
        return <Navigate to="/login" replace />
    }

    return <>{children}</>
}
```

ルーティングに組み込んでください。

```tsx
<Route
    path="/admin/contacts"
    element={
        <PrivateRoute>
            <ContactsPage />
        </PrivateRoute>
    }
/>
```

---

## 2. 一覧画面の作成

`resources/ts/pages/admin/ContactsPage.tsx` を作成してください。

---

## 3. API 呼び出し

```tsx
const fetchContacts = async () => {
    const res = await fetch('/api/admin/contacts')

    if (res.status === 401) {
        navigate('/login')
        return
    }

    const json = await res.json()
    setContacts(json.data)
}
```

---

## 4. テーブル表示

以下の項目をテーブル形式で表示してください。

| 項目 | カラム |
|------|--------|
| 氏名 | name |
| 会社名 | company |
| メールアドレス | email |
| 問い合わせ内容 | message |
| 問い合わせ日時 | created_at |

---

## チェックリスト

- [ ] ログイン済みの状態でお問い合わせ一覧が表示される
- [ ] 未ログイン状態で `/admin/contacts` にアクセスすると `/login` にリダイレクトされる

---

## 学習

### 今何をしたか

管理者専用の一覧画面を実装し、未ログイン時のアクセス制御を組み込みました。
これでアプリケーションの全機能が揃いました。

| 用語 | 説明 |
|------|------|
| PrivateRoute | 認証済みユーザーのみアクセスできるルートを実装するパターン |
| `<Navigate>` | React Router のコンポーネント。条件に応じてリダイレクトする |

### 参考資料

- [React Router - 認証の実装例](https://reactrouter.com/en/main/start/tutorial)
- [Chakra UI テーブルコンポーネント](https://chakra-ui.com/docs/components/table)
