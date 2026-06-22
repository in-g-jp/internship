# 0106 TypeScript

## TypeScript とは

TypeScript は JavaScript に **型** を追加した言語です。
このプロジェクトのフロントエンドは TypeScript で実装します。

---

## 静的型付けと動的型付け

プログラミング言語には「型がいつ決まるか」によって2種類あります。

| | 説明 | 代表的な言語 |
|--|------|------------|
| **動的型付け** | プログラムを実行したときに変数の型が決まる | JavaScript、Python |
| **静的型付け** | プログラムを実行する前（コンパイル時）に型が決まる | TypeScript、Java |

JavaScript は動的型付けのため、実行してみるまで型のミスに気づけません。
TypeScript は静的型付けのため、**実行前にエラーを検出**できます。

```js
// JavaScript：実行してみるまでわからない
function double(n) {
    return n * 2
}
double('hello')  // 実行すると NaN になる（気づきにくい）
```

```ts
// TypeScript：実行前にエラーになる
function double(n: number): number {
    return n * 2
}
double('hello')  // ❌ コンパイルエラー：number を渡してください
```

コンパイラが型の間違いを指摘してくれるので、バグを本番環境に持ち込みにくくなります。

参考：[静的型付け | サバイバルTypeScript](https://typescriptbook.jp/overview/static-type)

---

## なぜ型が必要なのか

JavaScript では変数にどんな値でも入れられます。

```js
let name = '太郎'
name = 123  // 文字列のつもりが数値になってしまう
```

TypeScript では型を決めると、違う種類の値を入れたときにエラーで教えてくれます。

```ts
let name: string = '太郎'
name = 123  // ❌ エラー：string 型に number は入れられません
```

バグを実行前に発見できるのが TypeScript の一番の強みです。

---

## 型注釈とは

変数名の右に `: 型名` と書いて、「この変数にはこの型の値だけ入れてよい」とコンパイラに伝えることを **型注釈** といいます。

```ts
const name: string = '太郎'
//         ^^^^^^^^ 型注釈
```

型注釈によってコンパイラが型の不整合を検出できるようになります。
型を明示しなくても型推論で自動判断されますが、明示することでコードの意図が伝わりやすくなります。

### 型注釈はコストか、投資か

TypeScript に慣れていないうちは、型を書くことを「余分な手間（コスト）」と感じることがあります。
しかし実際には、型注釈は **投資** と考える方が正確です。

| 視点 | 考え方 |
|------|--------|
| コスト | コード量が増える、書くのに時間がかかる |
| 投資 | コンパイラが型の間違いを自動で指摘してくれるようになる |

型注釈は「コンパイラへの情報提供」です。
正確な型情報を与えるほど、コンパイラはより多くのバグを実行前に検出してくれます。
これは、専属のコードレビュアーを育てるような感覚に近いです。

参考：[静的型付け | サバイバルTypeScript](https://typescriptbook.jp/overview/static-type)

---

## 型の概念

### プリミティブ型

TypeScript の基本となる型です。まずこの7種類を覚えましょう。

| 型 | 説明 | 例 |
|----|------|----|
| `string` | 文字列 | `'太郎'`、`"hello"` |
| `number` | 数値（整数・小数どちらも） | `25`、`3.14` |
| `boolean` | 真偽値 | `true`、`false` |
| `null` | 意図的に「値がない」ことを表す | `null` |
| `undefined` | まだ値が入っていない状態 | `undefined` |
| `symbol` | ユニークな識別子（あまり使わない） | `Symbol('id')` |
| `bigint` | 非常に大きな整数（あまり使わない） | `9007199254740991n` |

実務でよく使うのは `string`・`number`・`boolean`・`null`・`undefined` の5つです。

参考：[プリミティブ型 | サバイバルTypeScript](https://typescriptbook.jp/reference/values-types-variables/primitive-types)

### 基本的な型

変数名の後ろに `: 型名` と書くことで型を指定します。

```ts
let name: string = '太郎'      // 文字列
let age: number = 25           // 数値
let isStudent: boolean = true  // true / false
```

参考：[型注釈 | サバイバルTypeScript](https://typescriptbook.jp/reference/values-types-variables/type-annotation)

### 型推論

最初に代入した値から TypeScript が自動で型を判断してくれます。
型を書かなくても型の恩恵を受けられます。

```ts
let name = '太郎'  // 自動で string 型と判断される
let age = 25       // 自動で number 型と判断される

name = 123  // ❌ エラー：一度 string と判断されたので number は入れられない
```

参考：[型推論 | サバイバルTypeScript](https://typescriptbook.jp/reference/values-types-variables/type-inference)

### 関数の型

引数と戻り値にも型を指定できます。

```ts
// name は string を受け取り、string を返す関数
function greet(name: string): string {
    return `Hello, ${name}!`
}

greet(123)  // ❌ エラー：string を渡す必要があります
```

参考：[関数のパラメーター | サバイバルTypeScript](https://typescriptbook.jp/reference/functions/function-parameters)

### オブジェクト型

オブジェクトのプロパティそれぞれに型を指定します。

```ts
let user: { name: string; age: number } = {
    name: '太郎',
    age: 25,
}

// ? をつけると省略可能（なくても OK）なプロパティになる
let user2: { name: string; email?: string } = {
    name: '花子',
    // email は省略 OK
}
```

参考：[オブジェクト型 | サバイバルTypeScript](https://typescriptbook.jp/reference/values-types-variables/object)

### 配列型

```ts
let numbers: number[] = [1, 2, 3]
let names: string[] = ['太郎', '花子']

numbers.push('hello')  // ❌ エラー：number[] に string は入れられない
```

参考：[配列 | サバイバルTypeScript](https://typescriptbook.jp/reference/values-types-variables/array)

### ユニオン型

`|` で区切ることで「どれかの型」を表現できます。

```ts
// 'pending' か 'success' か 'error' のどれかしか入れられない
type Status = 'pending' | 'success' | 'error'

let currentStatus: Status = 'pending'   // ✅
currentStatus = 'success'               // ✅
currentStatus = 'loading'              // ❌ エラー
```

文字列だけでなく型も混ぜられます。

```ts
type StringOrNumber = string | number
let value: StringOrNumber = 'hello'  // ✅
value = 42                           // ✅
```

参考：[ユニオン型 | サバイバルTypeScript](https://typescriptbook.jp/reference/values-types-variables/union)

### インターフェース

オブジェクトの型をまとめて定義できます。
同じ型を複数の場所で使いまわすときに便利です。

```ts
interface User {
    name: string
    age: number
    email?: string  // ? をつけると省略可能
}

// User 型を使いまわせる
const taro: User = { name: '太郎', age: 25 }
const hanako: User = { name: '花子', age: 22, email: 'hanako@example.com' }

// 足りないプロパティがあるとエラーになる
const invalid: User = { name: '次郎' }  // ❌ age が足りない
```

参考：[インターフェース | サバイバルTypeScript](https://typescriptbook.jp/reference/object-oriented/interface)

### ジェネリクス

「型を引数のように渡せる」仕組みです。
どんな型でも使えるように汎用的な関数を作れます。

```ts
// <T> の T は「受け取った型」を表す（名前は何でも OK）
function firstItem<T>(items: T[]): T {
    return items[0]
}

firstItem<string>(['a', 'b', 'c'])  // 戻り値は string
firstItem<number>([1, 2, 3])        // 戻り値は number
```

React の `useState` もジェネリクスを使っています（後述）。

参考：[ジェネリクス | サバイバルTypeScript](https://typescriptbook.jp/reference/generics)

---

## React の概念

### コンポーネント

React では画面を **コンポーネント** という部品に分けて作ります。
ボタン・フォーム・ヘッダーなど、画面の一部をひとつのコンポーネントとして定義します。

```tsx
// コンポーネントは関数として定義し、JSX（HTML のような記法）を返す
const Button = () => {
    return <button>クリック</button>
}

// 他のコンポーネントの中で使える
const App = () => {
    return (
        <div>
            <Button />
            <Button />
        </div>
    )
}
```

### Props

コンポーネントに渡す **引数** のようなものです。
親コンポーネントから子コンポーネントへデータを渡すときに使います。

```tsx
// Props を受け取るコンポーネント
const Button = ({ label }) => {
    return <button>{label}</button>
}

// 使う側（Props を渡す）
const App = () => {
    return (
        <div>
            <Button label="送信" />
            <Button label="キャンセル" />
        </div>
    )
}
```

### State

コンポーネントが持つ **内部データ** です。
State が変わると画面が自動で再描画されます。

```tsx
import { useState } from 'react'

const Counter = () => {
    // count が State。初期値は 0
    const [count, setCount] = useState(0)

    return (
        <div>
            <p>{count}</p>
            {/* ボタンを押すと count が増え、画面が更新される */}
            <button onClick={() => setCount(count + 1)}>+1</button>
        </div>
    )
}
```

### useWatch

React Hook Form の `useWatch` を使うと、フォームの入力値をリアルタイムで監視できます。
特定のフィールドの値が変わったときに画面の表示を切り替えるなどの用途で使います。

```tsx
import { useForm } from 'react-hook-form'
import { useWatch } from 'react-hook-form'

const Form = () => {
    const { register, control } = useForm()

    // 'type' フィールドの値を監視する
    const type = useWatch({ control, name: 'type' })

    return (
        <form>
            <select {...register('type')}>
                <option value="individual">個人</option>
                <option value="company">法人</option>
            </select>

            {/* type が 'company' のときだけ会社名の入力欄を表示 */}
            {type === 'company' && (
                <input {...register('companyName')} placeholder="会社名" />
            )}
        </form>
    )
}
```

### イベントハンドラー

ボタンのクリックや入力フォームの変更など、ユーザーの操作に応じて処理を実行します。

```tsx
const Form = () => {
    const [value, setValue] = useState('')

    return (
        <div>
            {/* 文字が入力されるたびに setValue が呼ばれる */}
            <input
                value={value}
                onChange={(e) => setValue(e.target.value)}
            />
            {/* ボタンをクリックすると alert が表示される */}
            <button onClick={() => alert(value)}>送信</button>
        </div>
    )
}
```

---

## React と TypeScript の組み合わせ

### Props の型定義

コンポーネントが受け取る Props にも型を定義します。
間違った値を渡したときにすぐ気づけます。

```tsx
interface ButtonProps {
    label: string       // 必須
    onClick: () => void // 必須（引数なし・戻り値なしの関数）
    disabled?: boolean  // 省略可能
}

const Button: React.FC<ButtonProps> = ({ label, onClick, disabled = false }) => {
    return (
        <button onClick={onClick} disabled={disabled}>
            {label}
        </button>
    )
}

// 使う側
<Button label="送信" onClick={() => console.log('clicked')} />  // ✅
<Button onClick={() => {}} />  // ❌ label が足りない
<Button label={123} onClick={() => {}} />  // ❌ label は string
```

### useState の型付け

初期値から型推論されますが、`null` を許容する場合などは明示的に書きます。

```tsx
// 型推論：初期値が 0 なので number と判断される
const [count, setCount] = useState(0)

// 明示的に型を指定（初期値が空文字でも string とわかる）
const [name, setName] = useState<string>('')

// User 型または null を許容する場合
const [user, setUser] = useState<User | null>(null)
```

### イベントハンドラーの型付け

React のイベントには専用の型があります。
`event.target.value` などにもアクセスできるようになります。

```tsx
// input の文字が変わったとき
const handleChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    const value = event.target.value  // string 型として扱える
    console.log(value)
}

// フォームを送信したとき
const handleSubmit = (event: React.FormEvent<HTMLFormElement>) => {
    event.preventDefault()  // ページリロードを防ぐ
}

// ボタンをクリックしたとき
const handleClick = (event: React.MouseEvent<HTMLButtonElement>) => {
    console.log('clicked')
}
```

---

## 参考資料

- [サバイバル TypeScript](https://typescriptbook.jp/)

---

## チェックリスト

- [ ] 型を書かないと何が困るか説明できる
- [ ] 基本的な型（string・number・boolean）を使って変数を定義できる
- [ ] インターフェースで Props の型を定義してコンポーネントを作れる
- [ ] useState・イベントハンドラーに型を付けられる

---

## 実践確認

> - AI の使用は禁止です。Web 検索は OK です。
>
> **通話が繋がってから問題を開いてください。**

<details>
<summary>問題を見る</summary>

以下の値はそれぞれ何の型でしょうか？答えてください。

1. `'太郎'`
2. `42`
3. `true`
4. `null`
5. `'123'`
6. `3.14`

</details>
