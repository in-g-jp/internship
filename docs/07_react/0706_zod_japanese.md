# 0706 実装のチップス - Zod エラーメッセージの日本語化（zod-i18n-map）

## なぜ日本語化が必要か

Zod のデフォルトエラーメッセージは英語です。
`zod-i18n-map` を使うと、バリデーションエラーを日本語で表示できます。

---

## 1. ライブラリのインストール

```bash
sail npm install zod-i18n-map i18next
```

---

## 2. 日本語設定ファイルの作成

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

---

## 3. カスタムメッセージの集約

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

## 4. スキーマでの使い方

`zod` は `ja-zod.ts` からインポートすることで日本語メッセージが自動で適用されます。

```ts
import { z } from '@/libs/zod/ja-zod'
import { REQUIRED, INVALID_EMAIL } from '@/libs/zod/messages'

export const contactSchema = z.object({
    name: z.string().min(1, REQUIRED),
    email: z.string().min(1, REQUIRED).email(INVALID_EMAIL),
    message: z.string().min(1, REQUIRED),
})
```

---

## チェックリスト

- [ ] `zod-i18n-map` をインストールできる
- [ ] バリデーションエラーが日本語で表示される
- [ ] カスタムメッセージを定数ファイルで管理できる

---

## 参考資料

- [zod-i18n-map（GitHub）](https://github.com/aiji42/zod-i18n-map)
