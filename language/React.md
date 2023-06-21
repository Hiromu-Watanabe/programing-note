# React

<img src="../images/React_image.jpg" width="100%" alt="Reactのイメージ">
Reactはユーザーインターフェイスを構築するためのライブラリー。<br>
React はフレームワークではなく、ウェブに限定されるものでもない。<br>
例）

- React Native: モバイルアプリケーション
- React 360: 仮想現実アプリケーション

React が目指すところは、開発者が UI を構築しているときに発生するバグを最小限に抑えること。<br>
これは、コンポーネント (ユーザーインターフェイスの一部を記述する自己完結型の論理的なコード) を使用して行われる。<br>
これらのコンポーネントを組み合わせて完全な UI を作成でき、React はレンダリング作業の大部分を抽象化して、UI デザインに集中できるようにする。

<React におけるキーワード>

- hooks
- コンポーネント

<br>

※ 現在は素の React よりも[Next.js](https://nextjs.org/)（React を用いたフレームワーク）を使用したプロジェクトが増えてきてる

<br>
<br>

---

## **<font color="#00ff00">React Router</font>**

React 6 系だと`react-router-dom`の`Switch`が`Routes`に変更されたためコードが少し変わった。<br>
また、5 系までは`exact`を記述しないと完全一致という設定にならなかった<br>
（全てのパスに最初に`"/"`が入るため、`exact`がないと全てルートパスに飛んでしまっていた）<br>

<br>
<br>

## **【5 系までの書き方】**

```tsx: 5系までのReactコード
import { memo, FC } from "react";
import { Route, Switch } from "react-router-dom";

import { Login } from "../components/pages/Login";

export const Router: FC = memo(() => {
  return (
    <Switch>
      <Route exact path="/">
        <Login />
      </Route>
    </Switch>
  );
});
```

<br>
<br>

## **【6 系からの書き方】**

しかし、6 系からは`exact`の記述をしなくても完全一致となったため不要となった。<br>
また、 `Route`の中に`element={紐付けたいコンポーネント}`の形で記述するようになった。

```tsx: 6系からのReactコード
import { memo, FC } from "react";
import { Route, Routes } from "react-router-dom";

import { Login } from "../components/pages/Login";

export const Router: FC = memo(() => {
  return (
    <Routes>
      <Route path="/" element={<Login />}></Route>
    </Routes>
  );
});
```

この書き方だと、`Login`コンポーネントが undefined となりエラーになった。

```tsx: 6系からのReactコード
import { memo, FC } from "react";
import { Route, Routes } from "react-router-dom";

import { Login } from "../components/pages/Login";

export const Router: FC = memo(() => {
  return (
    <Routes>
      <Route path="/" element={<Login />}></Route>
    </Routes>
  );
});
```

エラー文

```js: エラー文
Uncaught runtime errors:

ERROR
[undefined] is not a <Route> component. All component children of <Routes> must be a <Route> or <React.Fragment>
```

実行時の環境情報

```json: 環境情報
{
  "name": "react-typescript",
  "version": "1.0.0",
  "description": "React and TypeScript example starter project",
  "keywords": [
    "typescript",
    "react",
    "starter"
  ],
  "main": "src/index.tsx",
  "dependencies": {
    "@chakra-ui/react": "^2.7.0",
    "@emotion/react": "^11.11.1",
    "@emotion/styled": "^11.11.0",
    "eslint": "^8.42.0",
    "framer-motion": "^10.12.16",
    "js-yaml": "^4.1.0",
    "loader-utils": "3.2.1",
    "react": "18.2.0",
    "react-dom": "18.2.0",
    "react-router-dom": "^6.12.1",
    "react-scripts": "5.0.1"
  },
  "devDependencies": {
    "@types/react": "18.0.25",
    "@types/react-dom": "18.0.9",
    "@typescript-eslint/eslint-plugin": "^5.59.11",
    "@typescript-eslint/parser": "^5.59.11",
    "typescript": "4.4.2"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test --env=jsdom",
    "eject": "react-scripts eject"
  },
  "browserslist": [
    ">0.2%",
    "not dead",
    "not ie <= 11",
    "not op_mini all"
  ]
}

```
