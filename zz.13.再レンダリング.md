# 再レンダリングが起きる条件

## どんなとき？

1. state が更新されたコンポーネントは再レンダリング
2. props が変更されたコンポーネントは再レンダリング
3. 再レンダリングされたコンポーネント配下の子要素は再レンダリング

<br />

# レンダリングの最適化（memo）

`props が変更された場合のみレンダリングしたい場合に使う。`

## どういう場合か?

例えば、あるコンポーネントがあり、それを「ChildArea.jsx」とする。

`ChildArea.jsx`

```jsx
export const ChildArea = (props) => {
  const { open } = props;
  return (
    <>
      {open ? (
        <div>
          <p>子コンポーネント</p>
        </div>
      ) : null}
    </>
  );
};
```

ChildArea は prop を引数として受け取り、props の中には「open」という boolean の値（変数）がある。  
レンダリング（描画）は、この「open」が true の場合は`"子コンポーネント"`と表示し、false の場合は表示をしないシンプルなもの。

そして、ChildArea は子コンポーネントで、呼び出し元の App.jsx があるものとする。

`App.jsx`

```jsx
import { ChildArea } from './ChildArea';
const App = () => {
  const [flag, setFlag] = useState(false);
  const [open, setOpen] = useState(false);

  const onClickFlag = () => setFlag(!flag);

  const onClickOpen = () => setOpen(true);

  return (
    <div>
      <p>{flag}</p>
      <button onClick={onClickFlag}>flagのほう</button>

      <button onClick={onClickOpen}>ChildAreaを表示</button>
      <ChildArea open={open} />
    </div>
  );
};
export default App;
```

ステートは 2 つあり、ChildArea に使用されているのは open のほうである。  
ボタンが２つあり、１つ目のボタンは flag を使用している。２つ目のボタンは open を使用している。
open のボタンをクリックすると ChildArea がレンダリングされる。これはわかる。  
だが、ChildArea に全く関係のない flag のボタンをクリックしても ChildArea も再レンダリングが走る。

**なぜか?**

再レンダリングが起きる条件の  
`「3. 再レンダリングされたコンポーネント配下の子要素は再レンダリング」`  
が働いているからである。

`そこで「memo」を使う`  
memo は`props が変更された場合のみレンダリングしたい場合に使う。`

ChildArea.jsx を修正する。

```jsx
import { memo } from 'react';
export const ChildArea = memo((props) => {
  ......
});
```

memo を import して、アロー関数を memo で囲む。それだけ。  
こうすることにより、props が変更されたとき、つまり、open が変更された時にだけ ChildArea に再レンダリングが走るようになる。

`子コンポーネントで prop(ステート)を渡すときは memo を使ったほうがいいだろう。`  
なぜステートなのかは、関数コンポーネントで変更がある変数は... ステートだからである。

<br />

# レンダリングの最適化（useCallback）

`親コンポーネントから子コンポーネントのpropsに関数を渡す場合に使う。`

## どういう場合に使うか？

先程の ChildArea.jsx と App.jsx の例で見てみる。

ChildArea コンポーネントに閉じるボタンを追加して、クリックされた時に、App コンポーネントの setOpen を false で呼んでみる。そうすると、open の値が false になるので ChildArea の描画が非表示にする。

`App.jsx`

```jsx
import { ChildArea } from './ChildArea';
const App = () => {
  const [flag, setFlag] = useState(false);
  const [open, setOpen] = useState(false);

  const onClickFlag = () => setFlag(!flag);

  const onClickOpen = () => setOpen(true);

  const onClickClose = () => setOpen(false);

  return (
    <div>
      <p>{flag}</p>
      <button onClick={onClickFlag}>flagのほう</button>

      <button onClick={onClickOpen}>ChildAreaを表示</button>
      <ChildArea open={open} clickClose={onClickClose} />
    </div>
  );
};
export default App;
```

onClickClose 関数を定義して、中身は setOpen を false で呼ぶ。

```jsx
const onClickClose = () => setOpen(false);
```

その関数を ChildArea の props に渡す。

```jsx
<ChildArea open={open} clickClose={onClickClose} />
```

`ChildArea.jsx`  
※ memo もついてるよ。だから、props が変更されない限りは再レンダリングされないはず。

```jsx
import { memo } from 'react';
export const ChildArea = memo((props) => {
  const { open, clickClose } = props;
  return (
    <>
      {open ? (
        <div>
          <p>子コンポーネント</p>
          <button onClick={clickClose}>閉じる</button>
        </div>
      ) : null}
    </>
  );
});
```

props に clickClose が追加されたので、それを取得する。

```jsx
onst { open, clickClose } = props;
```

閉じるボタンを追加して、onClick イベントに clickClose を紐付ける。

```jsx
<button onClick={clickClose}>閉じる</button>
```

`これは確かに動くし、間違ってはいない`  
しかし、memo のところでやった flag ボタンをクリックした際に ChildArea が再レンダリングしなかったところが、再レンダリングをしてしまうようになってしまう。

**なぜか?**

flag ボタンをクリックした際に、App コンポーネントが再レンダリングされ、  
`const onClickClose = () => setOpen(false);`
も新しい関数として認識しまうからである。※中身は一緒でも onClickClose は別のものとして認識

なので、ChildArea コンポーネントの props の clickClose に変化があったと認識され、memo が効かなくなる。まぁ memo としては正常なことだが。

`そこで「useCallback」を使う`  
useCallback は`関数の処理が変わらない場合は同じものを使う`ときに使う。

App.jsx を修正する。

```jsx
import { useCallback } from 'react';
const App = () => {
  .....

  const onClickClose = useCallback(() => setOpen(false));

  .....
};
export default App;
```

useCallback を import して、アロー関数を useCallback で囲む。memo と同じ。  
こうすることにより、関数は再生成されなくなる。

useEffect と同じように第２引数に値を設定することができる。

第２引数に、ステートの配列を指定する。これは、この useCallback が呼び出されるステートを指定するもの。

```jsx
useCallback(関数, [ステート, ステート]);
```

1 回だけ実行するには、第２引数に空の配列を渡す。

```jsx
useCallback(関数, []);
```

例えば、open の値が変わった場合に生成（再生成）する場合は、

```jsx
const onClickClose = useCallback(() => setOpen(false), [open]);
```

<br />

# 変数の memo 化

あまり使わないかもしれないが、変数を memo 化することもできる。memo とは別もの。  
例えば、複雑な計算をした結果の変数を再レンダリングするたびに計算させるのではなく、１回だけ行う場合はいいかも。

```jsx
import { useMemo } from 'react';
const App = () => {
  const tmp = useMemo(() => 1 + 2, []);
  return (
    <>
      <ChildArea />
    </>
  );
};
```
