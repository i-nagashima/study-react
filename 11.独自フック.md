# 関数コンポーネントの汎用性

フックは、平たくいえば「関数コンポーネントで、クラスのステートのような機能を実装するもの」。  
基本的に「 `関数コンポーネントを強化するもの`」といえる。  
クラスと比べ、関数は非常に構造がシンプルで、クラスならばさまざまな機能を用意できるが、関数ではそうはいかない。そこでフックという新しい機能が考案された。  
こうした「機能をコンポーネントから切り離して汎用的に使えるようにする」には「独自フック」として定義すればいい。  
関数コンポーネント内に既存のフック（ステートフックや副作用フックなど）を使って機能を実装するのではなく、機能そのものをコンポーネントとは別の独立した「オリジナルのフック」として作成する。

 <br />

# 独自フックの基本

```js
function use〇〇 () {
  const ステート
  const 関数
  return [値]
}
```

「use 〇〇」というように「use」で始まる名前にする。  
この関数では、必ず値を返す。この値は、普通の値ではなく、`[値]`というように配列の形で値をまとめておく必要がある。  
一般的には１つ目の値として「フックの値を得るための変数」、２つ目の値として「フックの値を変更するための関数」など。そうして、１つ目の戻り値の変数を利用してフックの値を取り出し、２つ目の戻り値の関数を呼び出してフックに値を設定できるようにする。※ステートフックの利用と同じ

<br />

# カウントするフックを作る

簡単なサンプルとして「数字をカウントするフック」  
フックには以下のものが必要

1. 数字を保管するステート
2. 呼び出すと 1 だけ増やす関数

```js
function useCounter() {
  // ステート
  const [num, setNum] = useState(0);
  // 関数
  const count = () => {
    setNum(num + 1);
  };
  // return [値]
  return [num, count];
}
```

## useCounter のフックを利用する

```js
import React, { useState } from 'react';

function useCounter() {
  // ステート
  const [num, setNum] = useState(0);
  // 関数
  const count = () => {
    setNum(num + 1);
  };
  // return [値]
  return [num, count];
}

function AlertMessage(props) {
  const [counter, plus] = useCounter();
  return (
    <div>
      <h4>count: {counter}</h4>
      <button onClick={plus}>count</button>
    </div>
  );
}
```
