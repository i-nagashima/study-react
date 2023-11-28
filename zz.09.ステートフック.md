# フックの基本　ステートフック

ステートフックは、その名のとおり「ステート」のためのフック。  
このステートフックは「useState」という関数として用意されている。

```js
const [変数A, 変数B] = useState(初期値);
```

この useState は、ステートを作成するためのもの。引数には、そのステートの初期値を指定する。戻り値は、２つの値が返される。[変数 A, 変数 B]という２つの変数に、戻り値がそれぞれ代入される。

変数 A: ステートの値。ここから現在のステートの値が得られる。  
変数 B: ステートの値を変更する関数。この変数に引数を付けて呼び出すことでステートの値が変更される。

※ただし、全てのフックがこのように２つの値を返すわけではない。フックの中にはステートフック以外のものもあるし、自分で作ることもできる。これは、あくまで「useState の場合」。

※useState の戻り値は配列で、分割代入で格納している。

<br />

# ステートで数字をカウント

```js
import React, { useState } from 'react';
import './App.css';

function App() {
  const [count, setCount] = useState(0);
  const clickFunc = () => {
    setCount(count + 1);
  };

  return (
    <div>
      <h1 className='bg-primary text-white display-4'>React</h1>
      <div className='container'>
        <h4 className='my-3'>Hooks sample</h4>
        <div className='alert alert-primary text-center'>
          <p className='h5 mb-3'>click: {count} times!</p>
          <div>
            <button className='btn btn-primary' onClick={clickFunc}>
              Click me
            </button>
          </div>
        </div>
      </div>
    </div>
  );
}

export default App;
```

### ステートの用意

ステートの値を取り出すときは count、変更するときは setCount が使えるようになる。

```js
const [count, setCount] = useState(0);
```

ステートを変更する関数は以下のように用意する。

```js
const clickFunc = () => {
  setCount(count + 1);
};
```

ステートの表示

```html
<p className="h5 mb-3">click: {count} times!</p>
```

クリック

ボタンをクリックしたら clickFunc が呼び出される。

```html
<button className="btn btn-primary" onClick="{clickFunc}">Click me</button>
```

クラスコンポーネントでは this.setState でステートを変更していたが、フックを利用する場合は  
`useStateで用意した関数を呼び出せばいい`

<br />

# ステートは複数作れる

この useState を使ったフックのステートは、クラスコンポーネントのステートと異なる点がある。それは、「１つの値を設定するだけ」という点。  
useState で作成できるのは、１つの値を操作する一対の変数のみ。が、この useState は、いくらでも記述できる。３つのステートがほしければ、useState を３つ用意して、それぞれの値を操作する変数を作成すればいい。  
クラスコンポーネントのステートとは異なり、値を変更する関数はそれぞれにあった名前の変数を設定すればよいから、こっちのほうがわかりやすい。

### クラスコンポーネントのステート

```js
// 用意（初期化）
this.state = {
  counter: 0,
  msg: 'Hello',
};
// 変更
this.setState({
  counter: this.state.counter + 1,
  msg: '*** count: ' + this.state.counter + ' ***',
});
```

### 関数コンポーネントでのステートフック

```js
// 用意（初期化）
const [counter, setCounter] = useState(0);
const [msg, setMsg] = useState('Hello');
// 変更
setCounter(counter + 1);
setMsg('*** count: ' + counter + ' ***');
```

<br />

# あらゆる値は「ステート」で保管する

この「ステート」の使い方は、クラスコンポーネントと関数コンポーネントで多少違う。  
クラスコンポーネントの場合、ステートを使うのは「表示に使われる値」。特に表示に使われるわけではない値は、プロパティとして保管していた。  
関数コンポーネントの場合、  
`コンポーネントで使われる値は基本的に全てステートとして保管する。`  
`表示に使われていないものでも、コンポーネント内で値を保持するものはすべてステートする。`

## 関数コンポーネントは常にリロードされる

クラスコンポーネントと関数コンポーネントの大きな違い、それは「クラスコンポーネントは状態維持されるが、関数コンポーネントはされない」という点。  
クラスはインスタンスを作成して利用されるのでページがリロードされるまでずっと保持される。  
しかし関数コンポーネントは、`ただの関数`。呼び出されたらそれで終わり。

## 変数ではなく、ステートで

関数コンポーネント内にある変数は、そのコンポーネントが更新されるたびに関数が再実行されるため、常に初期状態に戻る。`変数に値を保管しておくことはできない`。変数は、あくまで「`関数コンポーネント内の処理を実行中の間、一時的に値を保管しておくだけ`」のもの。  
したがって関数コンポーネントでは、コンポーネントの状態として常に値を保管しておきたいものは、`すべてステートとして用意しておく必要がある`。  
`これは、クラスコンポーネントと関数コンポーネントの非常に大きな違いとして覚えておく。`

**関数コンポーネントはただ実行されるだけ**

<br />
