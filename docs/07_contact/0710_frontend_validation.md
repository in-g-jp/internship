# 0405 ログイン - フロントエンドバリデーション

## 1. Zod スキーマの定義

`resources/ts/pages/auth/loginSchema.ts` を作成してください。

```ts
import { z } from 'zod'

export const loginSchema = z.object({
    email: z
        .string()
        .min(1, 'メールアドレスは必須です。')
        .email('メールアドレスの形式が正しくありません。'),
    password: z.string().min(1, 'パスワードは必須です。'),
})

export type LoginFormValues = z.infer<typeof loginSchema>
```

---

## 2. フォームへの組み込み

```tsx
import { useForm } from 'react-hook-form'
import { zodResolver } from '@hookform/resolvers/zod'
import { loginSchema, LoginFormValues } from './loginSchema'

const {
    register,
    handleSubmit,
    formState: { errors },
} = useForm<LoginFormValues>({
    resolver: zodResolver(loginSchema),
})
```

---

## チェックリスト

- [ ] 必須項目を空のまま送信するとエラーメッセージが表示される
- [ ] メールアドレスの形式が不正なときエラーが表示される

---

## 学習

### 今何をしたか

ログインフォームのフロントエンドバリデーションを実装しました。
0205 で学んだ Zod + React Hook Form のパターンと同じです。

### 参考資料

- [React Hook Form 公式ドキュメント](https://react-hook-form.com/)
- [Zod 公式ドキュメント](https://zod.dev/)
