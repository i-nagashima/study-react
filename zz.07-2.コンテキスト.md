# コンテキスト

「コンポーネントに共通の値をどう持たせるか」  
コンポーネントに固有の値は、属性を使い、this.props で取り出せばよい。が、「いくつもあるコンポーネントすべてに同じ値を渡して、それを使って処理する」というような場合、１つ１つのコンポーネントに同じ属性を用意して・・・というのは面倒。  
「全体で共通して使える値」を用意し利用するのが「コンテキスト」。

`Rみたいなものだな`

## コンテキストの作成

コンテキストは、React の「createContext」というメソッドを使って作成する。

```js
const 変数 = React.createContext(値);
```

注意するのは、「これはクラス内ではなく、`クラスの外側で実行する`」ということ。すべてのクラスで共通して使えるようにする必要があるから。  
これで、変数にコンテキストが代入される。このコンテキストには、引数に用意した値が保管されている。この値が、どのコンポーネントでも利用できるようになる。

## コンポーネントの利用

### コンポーネントでコンテキストを設定する

```js
static contextType = コンテキスト
```

この「static contextType」は、そのまま記述する。設定する値は、createContext で作成した変数。これで、コンテキストがコンポーネントに設定される。  
こうして設定されたコンテキストの値は、this.context プロパティにまとめられる。  
`this.context.xx`というようにして、必要な値を取り出す。

## コンテキストを使ってみる

```js
import React, { Component } from 'react';
import './App.css';

let data = {
  title: 'React-Context',
  message: 'this is sample message.',
};
const SampleContext = React.createContext(data);

class App extends Component {
  render() {
    return (
      <div>
        <h1 className='bg-primary text-white display-4'>React</h1>
        <div className='container'>
          <Title />
          <Message />
        </div>
      </div>
    );
  }
}

class Title extends Component {
  static contextType = SampleContext;

  constructor(props) {
    super(props);
  }

  render() {
    return (
      <div className='card p-2 my-3'>
        <h2>{this.context.title}</h2>
      </div>
    );
  }
}

class Message extends Component {
  static contextType = SampleContext;

  constructor(props) {
    super(props);
  }

  render() {
    return (
      <div className='alert alert-primary'>
        <p>{this.context.message}</p>
      </div>
    );
  }
}

export default App;
```

<br />

# Provider でコンテキストを変更する

コンテキストは「常に利用するすべてのコンポーネントで同じ値」が基本だが、ちょっとだけ値を変えたいといったことがあるかもしれない。  
こんなときに用いられるのが、「プロバイダー（Provider）」。これは一時的にコンテキストの値を変更するためのもの。  
これは、JSX でタグとして記述し利用するのが一番使いやすい（だろう）

```html
<コンテキスト.Provider value=値>
  ... コンポーネント ...
</ コンテキスト.Provider>
```

プロバイダーは、createContext で作成したコンテキストの「Provider」プロパティとして用意されるコンポーネント。value に新たなコンテキストの値を設定すると、このタグの中にだけ、コンテキストの値が変更される。これは、外側にあるコンポーネントには影響を与えない。

App

```js
let newdata = {
  title: '新しいタイトル',
  message: 'これは新しいメッセージ',
};

<div className='container'>
<SampleContext.Provider value={this.newdata}>
  <Title />
  <Message />
</div>
```

<br />
