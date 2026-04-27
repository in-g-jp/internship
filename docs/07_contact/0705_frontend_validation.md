# 0305 お問い合わせ - フロントエンドバリデーション

## 1. ライブラリのインストール

```bash
sail npm install react-hook-form zod @hookform/resolvers
```

---

## 2. Zod スキーマの定義

`resources/ts/pages/contact/contactSchema.ts` を作成してください。

```ts
import { z } from 'zod'

export const contactSchema = z.object({
    name: z.string().min(1, '氏名は必須です。'),
    company: z.string().optional(),
    email: z
        .string()
        .min(1, 'メールアドレスは必須です。')
        .email('メールアドレスの形式が正しくありません。'),
    message: z.string().min(1, '問い合わせ内容は必須です。'),
})

export type ContactFormValues = z.infer<typeof contactSchema>
```

---

## 3. フォームへの組み込み

```tsx
import { useForm } from 'react-hook-form'
import { zodResolver } from '@hookform/resolvers/zod'
import { contactSchema, ContactFormValues } from './contactSchema'

const {
    register,
    handleSubmit,
    formState: { errors },
} = useForm<ContactFormValues>({
    resolver: zodResolver(contactSchema),
})
```

---

## 4. エラーメッセージの表示

各フィールドの下にエラーメッセージを表示してください。

```tsx
{errors.name && <p>{errors.name.message}</p>}
```

---

## チェックリスト

- [ ] 必須項目を空のまま送信するとエラーメッセージが表示される
- [ ] メールアドレスの形式が不正なときエラーが表示される
- [ ] 正しく入力すると送信できる

---

## 学習

### 今何をしたか

フロントエンド側でもユーザーの入力値をチェック（バリデーション）する処理を実装しました。
サーバーに送信する前に不正な値を弾くことで、UX を向上させています。

| 用語 | 説明 |
|------|------|
| React Hook Form | React のフォーム状態管理ライブラリ。パフォーマンスが高く、バリデーションと連携しやすい |
| Zod | TypeScript ファーストのスキーマバリデーションライブラリ |
| `z.infer` | Zod スキーマから TypeScript の型を自動生成する機能 |
| `resolver` | React Hook Form と Zod を繋ぐアダプター |

### 参考資料

- [React Hook Form 公式ドキュメント](https://react-hook-form.com/)
- [Zod 公式ドキュメント](https://zod.dev/)
- [React Hook Form + Zod の使い方](https://react-hook-form.com/get-started#SchemaValidation)
