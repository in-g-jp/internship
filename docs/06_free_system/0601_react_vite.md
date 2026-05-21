# 0601 React + Vite セットアップ

## 1. パッケージのインストール

```bash
sail npm install
sail npm install react react-dom
sail npm install --save-dev @vitejs/plugin-react @types/react @types/react-dom typescript
```

---

## 2. vite.config.ts の更新

Laravel デフォルトでは `vite.config.js` が生成されているため、TypeScript で扱えるようにリネームしてください。

```bash
mv vite.config.js vite.config.ts
```

リネームしたら、以下のように編集してください。

```ts
import { defineConfig } from 'vite'
import laravel from 'laravel-vite-plugin'
import react from '@vitejs/plugin-react'

export default defineConfig({
    plugins: [
        laravel({
            input: ['resources/ts/main.tsx'],
            refresh: true,
        }),
        react(),
    ],
})
```

---

## 3. tsconfig.json の作成

プロジェクトルートに `tsconfig.json` を作成してください。

```json
{
    "compilerOptions": {
        "target": "ES2020",
        "lib": ["ES2020", "DOM"],
        "module": "ESNext",
        "moduleResolution": "bundler",
        "jsx": "react-jsx",
        "strict": true,
        "skipLibCheck": true
    },
    "include": ["resources/ts", "vite.config.ts"]
}
```

---

## 4. エントリーポイントの作成

Laravel デフォルトの `resources/js` は使わないため削除し、TypeScript 用のディレクトリを作成します。

```bash
rm -rf resources/js
mkdir -p resources/ts
```

`resources/ts/main.tsx` を作成してください（後の手順で Chakra UI を使うため `ChakraProvider` でラップしています）。

```tsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import { ChakraProvider, defaultSystem, Button } from '@chakra-ui/react'

ReactDOM.createRoot(document.getElementById('root')!).render(
    <React.StrictMode>
        <ChakraProvider value={defaultSystem}>
            <Button colorPalette="teal">Hello React</Button>
        </ChakraProvider>
    </React.StrictMode>
)
```

---

## 5. Blade テンプレートの作成

React を表示するための HTML の土台を作ります。

`resources/views/app.blade.php` を新規作成してください。

```html
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>App</title>
    @vite('resources/ts/main.tsx')
</head>
<body>
    <div id="root"></div>
</body>
</html>
```

次に `routes/web.php` を以下のように編集してください。すべての web ルートを `app.blade.php` に流す設定です。Laravel デフォルトの `/` welcome ルートはこの記述で上書きされます。

```php
Route::get('/{any?}', function () {
    return view('app');
})->where('any', '.*');
```

---

## 6. Chakra UI のインストール

Chakra UI v3 を使用します。v2 と異なり `@emotion/styled` や `framer-motion` は不要です。

```bash
sail npm install @chakra-ui/react @emotion/react
```

---

## 7. 開発サーバーの起動

```bash
sail npm run dev
```

---

## チェックリスト

- [ ] `sail npm run dev` が起動している
- [ ] ブラウザで `http://localhost` を開くと Chakra UI のボタンで `Hello React` が表示される

---

## 学習

### 今何をしたか

Laravel プロジェクトに React + TypeScript のフロントエンド環境を導入し、Chakra UI を使って画面を作れる状態にしました。

具体的には以下の設定を行いました。

- 必要なパッケージ（React・TypeScript・Vite プラグイン・Chakra UI）をインストール
- `vite.config.js` を `vite.config.ts` にリネームし、React プラグインを追加
- `tsconfig.json` で TypeScript のコンパイル設定を定義
- `resources/ts/main.tsx` をエントリーポイントとして作成し、`ChakraProvider` でラップ
- Blade テンプレートと routing を設定してブラウザで React が表示されるようにした

| 用語 | 説明 |
|------|------|
| Vite | 高速なフロントエンドのビルドツール。Laravel と統合して使える |
| React | UI を構築するための JavaScript ライブラリ |
| TypeScript | JavaScript に型を追加した言語。バグを事前に防ぎやすくなる |
| Chakra UI | React 用の UI コンポーネントライブラリ |
| tsconfig.json | TypeScript のコンパイル設定ファイル |
| エントリーポイント | アプリケーションの起点となるファイル。ここでは `main.tsx` |
| Blade テンプレート | Laravel のテンプレートエンジン。React の表示先 HTML を定義する |

### 参考資料

- [Vite 公式ドキュメント](https://ja.vitejs.dev/)
- [React 公式ドキュメント（日本語）](https://ja.react.dev/)
- [TypeScript 入門](https://www.typescriptlang.org/ja/docs/)
- [Chakra UI 公式ドキュメント](https://chakra-ui.com/getting-started)
