# Create React App TypeScript

React x TypeScript のプロジェクトを作成する

```zsh
# npx の場合
$ npx create-react-app <プロジェクト> --template typescript

# yarn の場合
$ yarn create react-app <プロジェクト> --template typescript
```

<br />

# 関数コンポーネントの型を指定する

## 基本

```tsx
import React, { FC } from 'react';
const Text: FC<Props> = (props) => {
  return (
    <>
      <p>ほげほげほげ</p>
    </>
  );
};
export default Text;
```

## FC

FC = Functional Component

## Props

ジェネリックス

Props は props の型を指定している

```tsx
type Props = {
  color: string;
  fontSize: string;
};
```

FC は暗黙的に children を受け取っている

```tsx
const { color, fontSize, children } = props;
```

React の 18 からは children はなくなる（らしい）  
それまでは VFC を使ってもよいと思う

<br />

# ライブラリ型定義

ライブラリに型定義がない場合は  
`@types/<component>`  
でインストールする

```zsh
$ yarn add --dev @types/react-dom
```

<br />

# React.FC と React.VFC はべつに使わなくていい説

[React.FC と React.VFC はべつに使わなくていい説](https://kray.jp/blog/dont-have-to-use-react-fc-and-react-vfc/)
