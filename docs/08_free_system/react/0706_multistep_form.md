# 0706 実装のチップス - 入力・確認・完了の多段階フォームフロー

## この Tips について

0704 で実装したお問い合わせフォームでは、送信ボタンを押すと即 API に送信していました。
この Tips では「入力 → 確認 → 完了」の3段階フローに変更する方法を学びます。

---

## 画面構成

| 画面 | パス | 説明 |
|------|------|------|
| 入力 | `/contact` | フォームに入力する |
| 確認 | `/contact/confirm` | 入力内容を確認して送信する |
| 完了 | `/contact/complete` | 送信完了メッセージを表示する |

---

## 1. ルーティングの追加

`resources/ts/main.tsx` に確認画面のルートを追加してください。

```tsx
import ConfirmPage from './pages/contact/ConfirmPage'

<Routes>
    <Route path="/contact" element={<ContactPage />} />
    <Route path="/contact/confirm" element={<ConfirmPage />} />
    <Route path="/contact/complete" element={<CompletePage />} />
</Routes>
```

---

## 2. 入力画面 — 確認画面へデータを渡す

入力画面では、API 送信の代わりに確認画面へ遷移します。
フォームデータは `navigate()` の `state` に乗せて渡します。

```tsx
// resources/ts/pages/contact/ContactPage.tsx
const navigate = useNavigate()

const onSubmit = (data: ContactFormValues) => {
    navigate('/contact/confirm', {
        state: { value: data },
    })
}
```

確認画面から「戻る」ボタンで戻ったとき、入力内容を復元するために
`useLocation().state` から取り出した値を `defaultValues` に設定します。

```tsx
const { state } = useLocation()
const result = contactSchema.safeParse(state?.value)

const { register, handleSubmit, formState: { errors } } = useForm<ContactFormValues>({
    resolver: zodResolver(contactSchema),
    defaultValues: result.success ? result.data : undefined,
})
```

---

## 3. 確認画面 — データの取り出しと送信

`resources/ts/pages/contact/ConfirmPage.tsx` を作成してください。

確認画面では `useLocation().state` からデータを取り出します。
URL を直接入力してアクセスするなど、データが存在しない場合は入力画面にリダイレクトします。

```tsx
import { useLocation, useNavigate, Navigate, Link } from 'react-router-dom'
import { useMutation } from '@tanstack/react-query'
import { contactSchema, ContactFormValues } from './contactSchema'

const ConfirmPage = () => {
    const { state } = useLocation()
    const navigate = useNavigate()

    // state にデータがなければ入力画面に戻す
    const result = contactSchema.safeParse(state?.value)
    if (!result.success) {
        return <Navigate replace to="/contact" />
    }

    const { data } = result

    const { mutate, isPending } = useMutation({
        mutationFn: async (values: ContactFormValues) => {
            const res = await fetch('/api/contacts', {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(values),
            })
            if (!res.ok) throw new Error('送信に失敗しました。')
        },
        onSuccess: () => {
            navigate('/contact/complete')
        },
    })

    return (
        <div>
            <h1>入力内容の確認</h1>
            <dl>
                <dt>氏名</dt>
                <dd>{data.name}</dd>
                <dt>会社名</dt>
                <dd>{data.company ?? '—'}</dd>
                <dt>メールアドレス</dt>
                <dd>{data.email}</dd>
                <dt>お問い合わせ内容</dt>
                <dd>{data.message}</dd>
            </dl>

            <button onClick={() => mutate(data)} disabled={isPending}>
                {isPending ? '送信中...' : '送信する'}
            </button>

            {/* 戻るボタン: state にデータを乗せて入力画面へ戻す */}
            <Link to="/contact" state={{ value: data }}>
                戻る
            </Link>
        </div>
    )
}

export default ConfirmPage
```

---

## 4. 画面間のデータの流れ

```
入力画面
  ↓  navigate('/contact/confirm', { state: { value: data } })
確認画面
  ↓  mutate(data) → API 送信成功
  ↓  navigate('/contact/complete')
完了画面
```

「戻る」ボタンは `<Link to="/contact" state={{ value: data }}>` で
入力画面に `state` を渡すことで、フォームに入力内容が復元されます。

---

## チェックリスト

- [ ] 入力画面で送信すると確認画面に遷移し、入力内容が表示される
- [ ] 確認画面の「戻る」ボタンで入力画面に戻ったとき、入力内容が残っている
- [ ] 確認画面の URL（`/contact/confirm`）に直接アクセスすると入力画面にリダイレクトされる
- [ ] 確認画面で「送信する」を押すと API にデータが送られ、完了画面に遷移する

---

## 学習

### 今何をしたか

フォーム送信を「入力 → 確認 → 完了」の3段階に分けました。
確認画面を挟むことで、ユーザーが送信前に内容を見直せるようになります。

| 用語 | 説明 |
|------|------|
| `navigate(path, { state })` | 画面遷移と同時にデータを渡す方法。URL には乗らず、画面間だけで共有される |
| `useLocation().state` | `navigate()` で渡された `state` を受け取る hooks |
| `safeParse()` | Zod でデータを検証し、失敗しても例外を投げずに結果を返すメソッド |
| `<Navigate replace>` | 条件を満たさない場合に別ページへリダイレクトするコンポーネント。`replace` を付けると履歴に残らない |

### 参考資料

- [React Router - useNavigate](https://reactrouter.com/en/main/hooks/use-navigate)
- [React Router - useLocation](https://reactrouter.com/en/main/hooks/use-location)
- [Zod - safeParse](https://zod.dev/?id=safeparse)
