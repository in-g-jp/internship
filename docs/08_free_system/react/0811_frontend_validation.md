# 0811 実装のチップス - フロントエンドバリデーション（Zod + React Hook Form）

## 1. ライブラリのインストール

```bash
sail npm install react-hook-form zod @hookform/resolvers zod-i18n-map i18next
```

---

## 2. エラーメッセージの日本語化

### 日本語設定ファイルの作成

`resources/ts/libs/zod/ja-zod.ts` を作成してください。

```ts
import i18next from 'i18next'
import { z } from 'zod'
import { zodI18nMap } from 'zod-i18n-map'
import translation from 'zod-i18n-map/locales/ja/zod.json'

i18next.init({
    lng: 'ja',
    resources: {
        ja: { zod: translation },
    },
})
z.setErrorMap(zodI18nMap)

export { z }
```

### カスタムメッセージの集約

よく使うエラーメッセージは定数ファイルにまとめておくと管理しやすくなります。

`resources/ts/libs/zod/messages.ts` を作成してください。

```ts
export const REQUIRED = '必須項目です。'
export const MAX_255 = '255文字以内で入力してください。'
export const INVALID_EMAIL = 'メールアドレスの形式で入力してください。'
export const INVALID_PHONE = '電話番号は半角数字10〜11桁で入力してください。'
export const INVALID_PASSWORD = 'パスワードは8文字以上で入力してください。'
export const DO_NOT_MATCH = '同じ値を入力してください。'
```

---

## 3. Zod スキーマの定義

`zod` は `ja-zod.ts` からインポートすることで日本語メッセージが自動で適用されます。

### お問い合わせフォーム

`resources/ts/pages/contact/contactSchema.ts` を作成してください。

```ts
import { z } from '@/libs/zod/ja-zod'
import { REQUIRED, INVALID_EMAIL } from '@/libs/zod/messages'

export const contactSchema = z.object({
    name: z.string().min(1, REQUIRED),
    company: z.string().optional(),
    email: z.string().min(1, REQUIRED).email(INVALID_EMAIL),
    message: z.string().min(1, REQUIRED),
})

export type ContactFormValues = z.infer<typeof contactSchema>
```

### ログインフォーム

`resources/ts/pages/auth/loginSchema.ts` を作成してください。

```ts
import { z } from '@/libs/zod/ja-zod'
import { REQUIRED, INVALID_EMAIL } from '@/libs/zod/messages'

export const loginSchema = z.object({
    email: z.string().min(1, REQUIRED).email(INVALID_EMAIL),
    password: z.string().min(1, REQUIRED),
})

export type LoginFormValues = z.infer<typeof loginSchema>
```

---

## 4. フォームへの組み込み

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

## 5. エラーメッセージの表示

各フィールドの下にエラーメッセージを表示してください。

```tsx
{errors.name && <p>{errors.name.message}</p>}
```

---

## 6. 高度なバリデーション

### refine（複数フィールド間のバリデーション）

`refine` は複数フィールドをまたいだバリデーションに使います。
代表的な例として、パスワードと確認用パスワードが一致するかのチェックがあります。

```ts
import { z } from '@/libs/zod/ja-zod'
import { DO_NOT_MATCH } from '@/libs/zod/messages'

export const passwordSchema = z
    .object({
        password: z.string().min(8),
        password_confirmation: z.string(),
    })
    .refine(
        ({ password, password_confirmation }) =>
            password === password_confirmation,
        {
            path: ['password_confirmation'],
            message: DO_NOT_MATCH,
        }
    )
```

`path` にエラーを表示したいフィールド名を指定します。

### regex（正規表現バリデーション）

電話番号やふりがなのように、入力形式を制限したい場合は `regex` を使います。

```ts
export const profileSchema = z.object({
    last_name_kana: z
        .string()
        .min(1)
        .regex(/^[ぁ-んー]+$/u, 'ひらがなで入力してください。'),
    phone_number: z
        .string()
        .regex(/^\d{10,11}$/, '電話番号は半角数字10〜11桁で入力してください。')
        .nullable(),
})
```

### setValueAs（空文字を null に変換）

テキスト入力を空にしたとき、空文字 `""` ではなく `null` としてサーバーに送りたい場合は
React Hook Form の `setValueAs` を使います。

```tsx
<input
    {...register('phone_number', {
        setValueAs: (value) =>
            value === '' || value == null ? null : value,
    })}
/>
```

これにより、未入力のフィールドは `null` として送信されます。

---

## チェックリスト

**お問い合わせフォーム**

- [ ] 必須項目を空のまま送信するとエラーメッセージが表示される
- [ ] メールアドレスの形式が不正なときエラーが表示される
- [ ] 正しく入力すると送信できる

**ログインフォーム**

- [ ] 必須項目を空のまま送信するとエラーメッセージが表示される
- [ ] メールアドレスの形式が不正なときエラーが表示される

**日本語化**

- [ ] バリデーションエラーが日本語で表示される
- [ ] カスタムメッセージを定数ファイルで管理できる

**高度なバリデーション**

- [ ] `refine` でパスワード確認バリデーションを実装できる
- [ ] `regex` でふりがなや電話番号の形式チェックができる
- [ ] `setValueAs` で空文字を `null` に変換できる

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
| `zod-i18n-map` | Zod のエラーメッセージを多言語化するライブラリ |
| `refine` | 複数フィールドをまたいだカスタムバリデーション |

---

## 参考資料

- [React Hook Form 公式ドキュメント](https://react-hook-form.com/)
- [Zod 公式ドキュメント](https://zod.dev/)
- [React Hook Form + Zod の使い方](https://react-hook-form.com/get-started#SchemaValidation)
- [zod-i18n-map（GitHub）](https://github.com/aiji42/zod-i18n-map)
- [Zod - refine（公式ドキュメント）](https://zod.dev/?id=refine)
- [React Hook Form - register（公式ドキュメント）](https://react-hook-form.com/docs/useform/register)
