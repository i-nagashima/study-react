# 関数コンポーネントでコンテキスト

クラスコンポーネントでコンテキストを使用したが、関数コンポーネントでもコンテキストを使用できる。

考え方は同じで「全体で共通して使える値」を用意し利用するのが「コンテキスト」。

<br />

# 関数コンポーネントでの考え方

コンテキスト用の関数コンポーネントを作成する。  
それは、機能ごとのコンテキストでもいいし、全体で１つのコンテキストでもいい。

例ではユーザ情報を格納するコンテキストを作ってみる

<br />

# 関数コンポーネントの作成

※providers のフォルダを作成して、その中に機能ごとのコンテキストを作成するとかがいいかも。

providers/UserProvider.jsx

<br />

# コンテキストの作成

`providers/UserProvider.jsx`

```jsx
import React, { createContext, useState } from 'react';

export const UserContext = createContext({});

export const UserProvider = (props) => {
  const { children } = props;

  const [userInfo, setUserInfo] = useState(null);

  return (
    <UserContext.Provider value={{ userInfo, setUserInfo }}>
      {children}
    </UserContext.Provider>
  );
};
```

コンテキストは、React の「createContext」というメソッドを使って作成する。  
初期値は空のオブジェクトでいい。  
他のコンポーネントで使用できるように export しておく。  
`書き方はこれで覚えちゃう`  
値はステートで管理をする。  
グローバルに参照できる値は Provider の value に設定する。オブジェクトで渡す。

<br />

# コンテキストを使う準備

コンテキストを使うところで、UserContext で囲ってしまう。

例えば、`App.jsx`

```jsx
import React from 'react';
import { UserProvider } from './providers/UserProvider';

const App = () => {
  return (
    <UserProvider>
      <Main />
    </UserProvider>
  );
};
export default App;
```

# コンテキストを使ってみる

```jsx
import React, { useContext } from 'react';

export const Main = (props) => {
  const { userInfo, setUserInfo } = useContext(UserContext);

  const onSetUserInfo = () => {
    setUserInfo({ hoge: true });
  };

  return (
    <div>
      <p>{userInfo.hoge}</p>
      <button onClick={onSetUserInfo}>クリック</button>
    </div>
  );
};
```

コンテキストは `useContext`で取得することができる。その際、どのコンテキストを取得すればいいか指定をする。  
今回は`UserContext`となる。
