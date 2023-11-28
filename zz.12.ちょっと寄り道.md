# React Developer Tools

ステートの変化とか見るのに役に立つ

[chrome のプラグイン](https://chrome.google.com/webstore)

<br />

# JSX での{}

JSX では変数や関数は{}の中に書く。そのため、アロー関数、無名関数の{}は変数や関数として展開されてしまうので{} → () もしくは 書かない でやる。

```jsx
Array.map((value) => {

})
↓
Array.map((value) => (

))
or
Array.map((value) =>
```

<br />

# JSX での class (css)

class はだめ。JavaScript の class と区別つかないから className とする。

<br />

# JSX でのイベント

onclick はだめ。  
onClick とする。他のイベントも同じ。

<br />

# JSX での終了タグがないタグ

\<input\>はだめ。\<input />とする。

<br />

# JSX 記法のルールを知る

return は１つの HTML エレメントだけ。  
そのため\<div\>\</div\>で囲うのたが、余計な div がはいってしまうので React.Fragment を使う

```jsx
const App = () => {
  return (
    <React.Fragment>
      <h1>こんにちは</h1>
      <p>おげんきですか</p>
    </React.Fragment>
  );
};
```

さらに React.Fragment がめんどくさいので

```jsx
<></>
```

で書いても React.Fragment と同じになる

```jsx
const App = () => {
  return (
    <>
      <h1>こんにちは</h1>
      <p>おげんきですか</p>
    </>
  );
};
```

<br />

# コンポーネントの拡張子

`jsx`

<br />

# 関数コンポーネントをかっこよく書いて見る

`ポイント`  
できるだけ、アロー関数で書くとかっこいい。

アロー関数

```js
const b = (name) => {
  return name;
};
const b = (name) => name;
const b = (name, name2) => name + name2;
```

かっこよく書いてみる

`App.jsx`

```jsx
import React, { useEffect, useState } from 'react';
import { ColorfulMessage } from './components/ColorfulMessage';

const App = () => {
  const [num, setNum] = useState(0);
  const [faceShowFlag, setFaceShowFlag] = useState(false);

  const onClickCountUp = () => {
    setNum(num + 1);
  };
  const onClickSwitchShowFlag = () => {
    setFaceShowFlag(!faceShowFlag);
  };

  useEffect(() => {
    if (num > 0) {
      if (num % 3 === 0) {
        faceShowFlag || setFaceShowFlag(true);
      } else {
        faceShowFlag && setFaceShowFlag(false);
      }
    }
  }, [num]);

  return (
    <>
      <h1 style={{ color: 'red' }}>こんにちは</h1>
      <ColorfulMessage color='blue'>お元気ですか</ColorfulMessage>
      <ColorfulMessage color='pink'>元気です</ColorfulMessage>
      <button onClick={onClickCountUp}>カウントアップ</button>
      <br />
      <button onClick={onClickSwitchShowFlag}>on/off</button>
      <p>{num}</p>
      {faceShowFlag && <p>＼(^o^)／</p>}
    </>
  );
};
export default App;
```

`components/ColorfulMessage.jsx`

```jsx
import React from 'react';

export const ColorfulMessage = (props) => {
  const { color, children } = props;
  const contentStyle = {
    color,
    fontSize: '18px',
  };

  return <p style={contentStyle}>{children}</p>;
};
```
