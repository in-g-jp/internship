# 0716 実装のチップス - Zod の refine・regex・setValueAs を使った高度なバリデーション

## refine とは

`refine` は複数フィールドをまたいだバリデーションに使います。
代表的な例として、パスワードと確認用パスワードが一致するかのチェックがあります。

---

## 1. パスワード確認バリデーション

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

---

## 2. 正規表現を使ったバリデーション

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

---

## 3. 空文字を null に変換する（setValueAs）

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

- [ ] `refine` でパスワード確認バリデーションを実装できる
- [ ] `regex` でふりがなや電話番号の形式チェックができる
- [ ] `setValueAs` で空文字を `null` に変換できる

---

## 参考資料

- [Zod - refine（公式ドキュメント）](https://zod.dev/?id=refine)
- [React Hook Form - register（公式ドキュメント）](https://react-hook-form.com/docs/useform/register)
