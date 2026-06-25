# 0705 実装のチップス - React Query の基本（useQuery / useMutation）

## React Query とは

React Query（TanStack Query）は、API へのデータ取得・更新を簡単に扱えるライブラリです。
フェッチ中のローディング状態・エラー状態・キャッシュを自動で管理してくれるため、`useEffect` と `useState` を組み合わせる手間が省けます。

---

## 1. インストール

```bash
sail npm install @tanstack/react-query
```

---

## 2. セットアップ

`resources/ts/main.tsx` に `QueryClientProvider` を追加してください。

```tsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import { QueryClient, QueryClientProvider } from '@tanstack/react-query'
import App from './App'

const queryClient = new QueryClient()

ReactDOM.createRoot(document.getElementById('root')!).render(
    <React.StrictMode>
        <QueryClientProvider client={queryClient}>
            <App />
        </QueryClientProvider>
    </React.StrictMode>
)
```

---

## 3. データ取得（useQuery）

API からデータを取得するときは `useQuery` を使います。

```tsx
import { useQuery } from '@tanstack/react-query'

type User = {
    id: number
    name: string
}

const fetchUsers = async (): Promise<User[]> => {
    const res = await fetch('/api/users')
    if (!res.ok) throw new Error('取得に失敗しました')
    return res.json()
}

const UserList = () => {
    const { data, isLoading, isError } = useQuery({
        queryKey: ['users'],
        queryFn: fetchUsers,
    })

    if (isLoading) return <p>読み込み中...</p>
    if (isError) return <p>エラーが発生しました</p>

    return (
        <ul>
            {data?.map((user) => (
                <li key={user.id}>{user.name}</li>
            ))}
        </ul>
    )
}
```

| プロパティ | 説明 |
|------------|------|
| `queryKey` | キャッシュの識別キー。同じキーは同じデータとして扱われる |
| `queryFn` | データを取得する非同期関数 |
| `isLoading` | 初回取得中は `true` |
| `isError` | エラーが発生したら `true` |
| `data` | 取得したデータ |

---

## 4. データ送信（useMutation）

POST / PUT / DELETE など、データを変更するときは `useMutation` を使います。

```tsx
import { useMutation, useQueryClient } from '@tanstack/react-query'

const createUser = async (name: string): Promise<void> => {
    const res = await fetch('/api/users', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ name }),
    })
    if (!res.ok) throw new Error('作成に失敗しました')
}

const CreateUserForm = () => {
    const queryClient = useQueryClient()

    const mutation = useMutation({
        mutationFn: createUser,
        onSuccess: () => {
            // 成功したらユーザー一覧のキャッシュを無効化して再取得する
            queryClient.invalidateQueries({ queryKey: ['users'] })
        },
    })

    return (
        <button onClick={() => mutation.mutate('新しいユーザー')}>
            {mutation.isPending ? '送信中...' : '作成する'}
        </button>
    )
}
```

| プロパティ | 説明 |
|------------|------|
| `mutationFn` | データを変更する非同期関数 |
| `onSuccess` | 成功時に実行するコールバック |
| `isPending` | 送信中は `true` |
| `invalidateQueries` | 指定したキーのキャッシュを無効化して再フェッチする |

---

## 5. エラーハンドリング（onError）

`useMutation` の `onError` コールバックを使うと、API のエラーレスポンスを受け取って
画面にエラーメッセージを表示できます。

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

## 6. Laravel のバリデーションエラー（422）を表示する

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
            const messages = Object.values(errors).flat()
            setErrorMessage(messages.join('\n'))
        } else {
            setErrorMessage('エラーが発生しました。')
        }
    },
})
```

---

## チェックリスト

- [ ] `QueryClientProvider` でアプリ全体をラップできる
- [ ] `useQuery` で API からデータを取得して表示できる
- [ ] `useMutation` でデータを送信し、成功後に一覧を更新できる
- [ ] `onError` でエラーメッセージを画面に表示できる
- [ ] Laravel の 422 エラーのフィールドごとのメッセージを取り出せる

---

## 参考資料

- [TanStack Query 公式ドキュメント](https://tanstack.com/query/latest)
- [TanStack Query - useMutation](https://tanstack.com/query/latest/docs/framework/react/reference/useMutation)
- [Axios - エラーハンドリング](https://axios-http.com/ja/docs/handling_errors)
