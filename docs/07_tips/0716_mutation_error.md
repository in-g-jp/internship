# 0716 実装のチップス - useMutation のエラーハンドリング

## API エラーを画面に表示する

`useMutation` の `onError` コールバックを使うと、API のエラーレスポンスを受け取って
画面にエラーメッセージを表示できます。

---

## 1. エラーハンドリングの基本パターン

```tsx
import { useMutation } from '@tanstack/react-query'
import { useState } from 'react'

const [errorMessage, setErrorMessage] = useState<string | null>(null)

const { mutate, isPending } = useMutation({
    mutationFn: submitForm,
    onError: (error: unknown) => {
        if (error instanceof Error) {
            setErrorMessage(error.message)
        } else {
            setErrorMessage('エラーが発生しました。')
        }
    },
    onSuccess: () => {
        setErrorMessage(null)
        // 成功時の処理（画面遷移など）
    },
})

return (
    <form onSubmit={handleSubmit((data) => mutate(data))}>
        {errorMessage && <p style={{ color: 'red' }}>{errorMessage}</p>}
        <button type="submit" disabled={isPending}>
            {isPending ? '送信中...' : '送信する'}
        </button>
    </form>
)
```

---

## 2. Laravel のバリデーションエラー（422）を表示する

Laravel が 422 を返すとき、レスポンスの形式は以下のようになっています。

```json
{
    "message": "The given data was invalid.",
    "errors": {
        "email": ["メールアドレスの形式が正しくありません。"]
    }
}
```

`axios` を使っている場合は `error.response.data` でこの内容を取得できます。

```tsx
import axios from 'axios'

const { mutate } = useMutation({
    mutationFn: submitForm,
    onError: (error: unknown) => {
        if (axios.isAxiosError(error) && error.response?.status === 422) {
            const errors = error.response.data.errors as Record<string, string[]>
            // フィールドごとのエラーを取り出す例
            const messages = Object.values(errors).flat()
            setErrorMessage(messages.join('\n'))
        } else {
            setErrorMessage('エラーが発生しました。')
        }
    },
})
```

---

## 3. 成功後にキャッシュを更新する

`onSuccess` で `invalidateQueries` を呼ぶと、関連するデータを再取得できます。

```tsx
import { useMutation, useQueryClient } from '@tanstack/react-query'

const queryClient = useQueryClient()

const { mutate } = useMutation({
    mutationFn: createItem,
    onSuccess: () => {
        // 一覧データのキャッシュを無効化して再フェッチ
        queryClient.invalidateQueries({ queryKey: ['items'] })
    },
})
```

---

## チェックリスト

- [ ] `onError` でエラーメッセージを画面に表示できる
- [ ] Laravel の 422 エラーのフィールドごとのメッセージを取り出せる
- [ ] `onSuccess` でキャッシュを更新して一覧を再取得できる

---

## 参考資料

- [TanStack Query - useMutation](https://tanstack.com/query/latest/docs/framework/react/reference/useMutation)
- [Axios - エラーハンドリング](https://axios-http.com/ja/docs/handling_errors)
