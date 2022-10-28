# ステート

ステートは、コンポーネントで利用する「値」の保管庫。コンポーネントの中で利用する値などは、このステートを使って保管する。  
では props と何が違うのか。

### プロパティ

クラスに値を保管しておくのに使うもの。コンポーネントに限らず、クラス全般で利用されるもの。

### 属性（props）

コンポーネントの属性をまとめて保管するためのもの。これは基本的に「read only」。  
つまり、値は取り出せるだけで変更することはできない。

### ステート

コンポーネントの状態を表す値を保管するためのもの。これは、コンポーネントの「現在の状態」を扱うためのもの。  
状態を使うというのはどういうことか？  
それはつまり「ステートの値を操作することで、コンポーネントの状態を操作できる」ということ。  
コンポーネントの表示を変えたりするのに、ステートは必須のもの。

`ステートは自動更新される`

プロパティは、変更してもコンポーネントの表示は変わらない。表示に利用している変数やプロパティを変更したら、更に ReactDOM.render を呼び出して表示を更新する必要がある。  
ステートを利用すれば、そうした更新作業は必要なくなり、ただ値を変更するだけで、自動的に表示が更新される。

<br />

# ステートを用意する

このステートは、コンポーネントに「state」というプロパティとして用意される。この中に、ステートの値がまとめて保管される。  
値が必要となれば、this.state.xx というようにして値を取り出せる。  
まずは「constructor でステートの値を設定する」方法から。

### constructor で値を初期化する

```js
this.state = { ... 値を用意する ...}
```

### ステートの値を表示する

簡単な例 App.js

```js
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      msg: 'Hello Component.',
    };
  }

  render() {
    return (
      <div>
        <h1 className='bg-primary text-white display-4'>React</h1>
        <div className='container'>
          <p className='subtitle'>Show message.</p>
          <p className='alert alert-warning'>{this.state.msg}</p>
          <p className='alert alert-dark'>{this.props.msg}</p>
        </div>
      </div>
    );
  }
}

export default App;
```

ここで初期化  
もちろん必要に応じていくつでも値を用意することができる。

```js
this.state = {
  msg: 'Hello Component.',
};
```

JSX を使った表示のところで利用  
ステートの値は this.state.xx という形で取り出すことができる。属性の値も this.props.xx で取り出せる。  
「値を取り出して表示する」という部分は、どちらも似ている。

```html
<p className="alert alert-warning">{this.state.msg}</p>
<p className="alert alert-dark">{this.props.msg}</p>
```

<br />

# ステートの更新

constructor でのステートの設定は、あくまでも「ステートの初期化」の処理。ステートの更新は以下。  
引数には、オブジェクトリテラルを使ってステートに設定する値をまとめたものを指定する。

```js
this.setState( {... 値を用意 ...} )
```

### ステートは「追加」される

setState で注意しておきたいのは、「setState によって、ステートの値が完全に置き換わるわけではない」ということ。  
setState の関数では用意した値がステートに追加される。既に同じ値があればそれは更新される。  
が、「setState に用意されていない値は消されるのか」というと、消されずにそのまま。

### ステートを変更してみる

ボタンイベントでやってみる

```js
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      msg: 'Hello',
      counter: 0,
    };
    this.doAction = this.doAction.bind(this);
  }

  doAction(event) {
    this.setState({
      counter: this.state.counter + 1,
      msg: '*** count: ' + this.state.counter + ' ***',
    });
  }

  render() {
    return (
      <div>
        <h1 className='bg-primary text-white display-4'>React</h1>
        <div className='container'>
          <p className='subtitle'>Count number.</p>
          <div className='alert alert-primary text-center'>
            <p className='h5 mb-4'>{this.state.msg}</p>
            <button className='btn btn-primary' onClick={this.doAction}>
              Click
            </button>
          </div>
        </div>
      </div>
    );
  }
}

export default App;
```

<br />
