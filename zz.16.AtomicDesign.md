# Atomic Design とは

画面要素を５段階に分け、組み合わせることで UI を実現。コンポーネント化された要素が画面を構成しているという考え方。React, Vue 用というわけではない。モダン JavaScript と相性が良い

<br />

# ５段階のコンポーネント

- Atoms
- Molecules
- Organisms
- Templates
- Pages

<br />

# Atoms (原子)

最も小さくそれ以上分解できない要素

- ボタン
- ツイート入力テキストボックス
- アイコン

など

`src/components/atoms/button/PrimaryButton.jsx`  
or  
`src/components/atoms/PrimaryButton.jsx`

```jsx
export const PrimaryButton = (props) => {
  const { children } = props;
  return <button>{children}</button>;
};
```

<br />

# Molecules (分子)

Atom の組み合わせで意味を持つデザインパーツ

- アイコン＋メニュー名
- プロフィール画像＋テキストボックス
- アイコンセット

など

`src/components/molecules/SearchInput.jsx`

```jsx
export const SearchInput = () => {
  return (
    <div>
      <input />
      <PrimaryButton>検索</PrimaryButton> // Atomを使う
    </div>
  );
};
```

<br />

# Organisms (有機体)

Atom や Molecule の組み合わせで構成される単体である程度の意味を持つ要素群

- ツイート入力エリア
- サイドメニュー
- １つのツイートエリア

など

`src/components/organisms/user/UserCard.jsx`  
or  
`src/components/organisms/UserCard.jsx`

```jsx
export const UserCard = () => {
  return (
    <div>
      <SearchInput />
      <SearchInput2 />
    </div>
  );
};
```

<br />

# Templates (テンプレート)

ページのレイアウトのみを表現する要素（実際のデータはもたない）

- サイドメニュー
- ツイートエリア
- トピックエリア等のレイアウト情報

など

`src/components/templates/DefaultLayout.jsx`

この例では、ヘッダとフッタがあって、その間にコンテンツがあるレイアウト  
Header と Footer は Organisms として作っていてもいいし、Molecules でもいい。

```jsx
export const DefaultLayout = (props) => {
  const { children } = props;
  return (
    <>
      <Header />
      {children}
      <Footer />
      <>
    </>
  );
}
```

この Templates をどんな感じで使うかというと、App.jsx とかで次の Pages を中にいれる感じ。

```jsx
const App = () => {
  return (
    <>
      <DefaultLayout>
        <Top />
      </DefaultLayout>
    </>
  );
};
```

<br />

# Pages (ページ)

最終的に表示される１画面  
データを流し込んで表示を完成させる

- ページ遷移ごとに表示される各画面

`src/components/pages/Top.jsx`

```jsx
export const Top = () => {
  return (
    // routerとか使ってLinkも作っていく

    <SearchInput />
    <UserCard />
  );
};
```

<br />

# Atomic Design に取り組む時のポイント

- あくまでベース  
  Atomic Design はあくまで概念だと認識し、プロジェクトやチームに合わせてカスタマイズしていく
- 初めから分けない  
  慣れないうちに無理にコンポーネントに分けようとするとしんどい。まずは書いて定期的にリファクタリング
- 要素の関心を意識  
  「何に関心があるコンポーネントなのか」を意識しながら分割したり props を定義したりする
