# 更新時に処理を実行する

ステートは、値を変更すると自動的に表示が変更される。ステートを利用する限りにおいて、表示は常に最新の状態に保たれる。「ステートを利用する限りにおいては」。  
ということは、ステートが直接表示に使われていないところは、当たり前だがステートが変更されても更新されない。  
が、ページにアクセスしたり表示が更新されたりした際に何らかの処理を実行したりすることはある。例えばサーバにアクセスしてデータを取得し利用するようなときは、必要に応じてアクセスし最新のデータを取得しなければいけないこともある。  
このように、「更新時に必要な処理を実行する」のに副作用フックを使う。

## 副作用フック

```js
useEffect(関数);
```

useEffect という React に用意されている関数を使う。引数には、実行する処理をまとめた関数を指定する。これだけで、コンポーネントが更新された際に指定の関数を実行して必要な処理を行えるようになる。

# 副作用フックを使ってみる

```js
import React, { useState, useEffect } from 'react';
import './App.css';

function AlertMessage(props) {
  return (
    <div className='alert alert-primary h5 text-primary'>
      <h5>{props.msg}</h5>
    </div>
  );
}

function App() {
  const [val, setVal] = useState(0);
  const [msg, setMsg] = useState('set a number...');

  const doChange = (event) => {
    setVal(event.target.value);
  };

  useEffect(() => {
    let total = 0;
    for (let i = 0; i <= val; i++) {
      total += i;
    }
    setMsg('total:' + total + '.');
  });

  return (
    <div>
      <h1 className='bg-primary text-white display-4'>React</h1>
      <div className='container'>
        <h4 className='my-3'>Hooks sample</h4>
        <AlertMessage msg={msg}></AlertMessage>
        <div className='form-group'>
          <label>input:</label>
          <input
            type='number'
            className='form-control'
            onChange={doChange}
          ></input>
        </div>
      </div>
    </div>
  );
}

export default App;
```

### ２つのステート

```js
const [val, setVal] = useState(0);
const [msg, setMsg] = useState('set a number...');
```

### onChange イベント

普通だったら、ここで、「event.target.value で値を取り出して setVal し、それから val までの合計を計算して結果のメッセージを setMsg する」といったことをやっていたが、ここではやらず、useEffect でやる。

```js
const doChange = (event) => {
  setVal(event.target.value);
};
```

### useEffect

このように、useEffect で関数を設定しておくだけで、コンポーネントがマウントされたり表示されたりした際には指定の関数が実行されるようになる。  
doChange で setVal により val の値を更新すると、コンポーネントが更新され、それにより useEffect で設定された関数が`自動的`に呼び出され計算と結果表示が行われる。  
計算処理を副作用フックとして実装することで、onChange のイベントではシンプルに「ステートを更新する」だけ行えばすむようになる。

```js
useEffect(() => {
  let total = 0;
  for (let i = 0; i <= val; i++) {
    total += i;
  }
  setMsg('total:' + total + '.');
});
```

<br />

# 複数の副作用フック

副作用フックも、ステートフックと同様、いくつでも作成することができる。

だだし、`無限に副作用フックが呼び出される`ような記述ができてしまうので注意！！

そのため、「再呼び出しのフックを指定する」ことができる。

<br />

# 再呼び出しのフックを指定する

第２引数に、ステートの配列を指定する。これは、この副作用フックが呼び出されるステートフックを指定するもの。

```js
useEffect(関数, [ステート, ステート]);
```

<br />

# useEffect をコンポーネントのマウント時に 1 回だけ実行する方法

useEffetct に渡される関数を、コンポーネントのマウント時に 1 回だけ実行するには、第２引数に空の配列を渡す。

```js
useEffect(関数, []);
```
