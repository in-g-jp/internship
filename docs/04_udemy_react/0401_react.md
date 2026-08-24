# 0401 React（Udemy）

## この章の目的

- React でシンプルな UI を自力で作れるようになる
- **コンポーネント・Props・State** の概念を理解する

---

## コース

[React - The Complete Guide（Udemy）](https://www.udemy.com/course/react-complete-guide/)

---

## 取り組む章

| セクション名                                           | 扱い                                       | 動画時間     |
| ------------------------------------------------------ | ------------------------------------------ | ------------ |
| Section 1: はじめに                                    | 必須                                       | 10 分        |
| Section 3: 【スキップ可】Reactで頻出のJavaScriptの記法 | 任意（分からない文法が出てきたら適宜確認） | 1 時間 27 分 |
| Section 4: まずはReactに触れてみよう                   | 必須                                       | 2 時間 7 分  |
| Section 5: イベントリスナと状態管理（State）           | 必須                                       | 1 時間 41 分 |
| Section 6: 制御構文とフォームの制御                    | 必須                                       | 1 時間 33 分 |
| Section 11: 【React Hooks】様々な状態管理の方法        | 必須                                       | 2 時間       |
| Section 12: 【React Hooks】useEffectとカスタムフック   | 必須（API 通信の前提知識）                 | 58 分        |

> **合計動画時間：約 8 時間 30 分**

---

## React の基礎概念

### React とは

React は**UI を作るための JavaScript ライブラリ**です。
画面を **コンポーネント** という部品に分けて作り、それを組み合わせてアプリを構築します。

| 従来の HTML + JS           | React                                  |
| -------------------------- | -------------------------------------- |
| HTML に直接要素を書く      | コンポーネントとして部品化する         |
| DOM を手動で操作する       | State が変わると画面が自動で更新される |
| 画面の一部を再利用しにくい | コンポーネントは何度でも再利用できる   |

---

### JSX

React では JavaScript の中に HTML のような記法（**JSX**）で UI を書きます。

```tsx
const Greeting = () => {
  return <h1>こんにちは！</h1>;
};
```

JSX の中で JavaScript の値を使うときは `{}` で囲みます。

```tsx
const name = "太郎";

const Greeting = () => {
  return <h1>こんにちは、{name}！</h1>;
};
```

> JSX は最終的にブラウザが理解できる JavaScript に変換されます。見た目は HTML ですが、実態は JavaScript です。

---

### コンポーネント

React の画面は **コンポーネント** という小さな部品の組み合わせで作ります。
コンポーネントは **JSX を返す関数** として定義します。

```tsx
const Button = () => {
  return <button>クリック</button>;
};

const App = () => {
  return (
    <div>
      <Button />
      <Button />
      <Button />
    </div>
  );
};
```

コンポーネント名は**必ず大文字から始める**ルールです（小文字だと HTML タグとして扱われます）。

| 理由   | 説明                                         |
| ------ | -------------------------------------------- |
| 再利用 | 同じ見た目の UI を一度定義して何度でも使える |
| 可読性 | 画面の構造が把握しやすくなる                 |
| 保守性 | 変更箇所が 1 ヶ所に集約される                |

---

### Props

**Props（プロパティ）** は、親コンポーネントから子コンポーネントへデータを渡す仕組みです。
関数の引数のようなイメージです。

```tsx
interface ButtonProps {
  label: string;
  disabled?: boolean;
}

const Button = ({ label, disabled = false }: ButtonProps) => {
  return <button disabled={disabled}>{label}</button>;
};

const App = () => {
  return (
    <div>
      <Button label="送信" />
      <Button label="キャンセル" disabled={true} />
    </div>
  );
};
```

---

### State

**State** はコンポーネントが持つ**内部データ**です。
State が変わると React が画面を自動で再描画します。

```tsx
import { useState } from "react";

const Counter = () => {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>カウント: {count}</p>
      <button onClick={() => setCount(count + 1)}>+1</button>
    </div>
  );
};
```

> Props は**外から渡されるデータ**、State は**コンポーネント自身が持つデータ**と覚えましょう。

---

### useEffect

**useEffect** は、コンポーネントのレンダリング後に**副作用（データ取得・タイマーなど）を実行する**フックです。
API からデータを取得するときに必ず使います。

```tsx
import { useState, useEffect } from "react";

const UserList = () => {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    fetch("/api/users")
      .then((res) => res.json())
      .then((data) => setUsers(data));
  }, []); // [] = マウント時に 1 度だけ実行

  return (
    <ul>
      {users.map((user) => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
};
```

---

## チェックリスト

- [ ] JSX とは何か説明できる
- [ ] コンポーネントを自分で定義して使える
- [ ] Props を使って親から子へデータを渡せる
- [ ] `useState` を使って State を更新できる
