# 属性の利用

コンポーネントクラスでは、constructor の引数で属性の値が渡されていた。「props」という引数。  
これを利用して、属性で指定した値を表示するようにコンポーネントを書き換えてみる。

### App.js

constructor の引数「props」から、title と message という値を取り出し、プロパティに設定。  
render では、this.title と this.message を表示。

```js
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  constructor(props) {
    super(props);
    this.title = props.title;
    this.message = props.message;
  }

  render() {
    return (
      <div>
        <h1 className='bg-primary text-white display-4'>React</h1>
        <div className='container'>
          <p className='subtitle'>{this.title}</p>
          <p>{this.message}</p>
        </div>
      </div>
    );
  }
}

export default App;
```

### index.js

title と message を App コンポーネントへ渡す。

```js
<App title='App' message='This is App Component' />
```
