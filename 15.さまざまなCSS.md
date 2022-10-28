# Inline Styles

スタイルを埋め込む

```jsx
export const InlineStyle = () => {
  const hogeStyle = {
    border: 'solid 1px #392eff',
    borderRadius: '20px',
  };
  return <div style={hogeStyle}></div>;
};
```

<br />

# CSS Modules

sass を使う

### sass をインストールする

```zsh
yarn add node-sass
```

### ファイルの対応

コンポーネントに対応する CSS ファイルを作成する。  
CssModules.jsx 　＝　 CssModules.module.scss

### CssModules.module.scss

```scss
.container {
  border: solid 1px #392eff;
  border-radius: 20px;
}
```

### CssModules.jsx

```jsx
import classes from './CssModules.module.scss';
export const CssModules = () => {
  return <div className={classes.container}></div>;
};
```

<br />

# Styled JSX

Next.js に標準で入っている  
コンポーネントの中でスタイルタグを書いていく

### styled-jsx をインストールする

```zsh
yarn add styled-jsx
```

### StyledJsx.jsx

```jsx
export const StyledJsx = () => {
  return (
    <>
      <div className='container'></div>
      <style jsx='true'>{`
        .container {
          border: solid 1px #392eff;
          border-radius: 20px;
        }
      `}</style>
    </>
  );
};
```

<br />

# styled components

人気がある。  
スタイルをコンポーネントのように扱う。

※スタイルのコンポーネントか普通のコンポーネントか区別がつかない  
※Container → SContainer のように S をつけるなど工夫が必要かな

### styled-components をインストールする

```zsh
yarn add styled-components
```

### StyledComponents.jsx

```jsx
import styled from 'styled-components';
export const StyledComponents = () => {
  return (
    <Container>
      <div></div>
    </Container>
  );
};

const Container = styled.div`
  .container {
    border: solid 1px #392eff;
    border-radius: 20px;
  }
`;
```

<br />

# Emotion

@emotion/react と @emotion/styled の 2 種類がある

### @emotion/react をインストールする

```zsh
yarn add @emotion/react
```

### @emotion/styled をインストールする

```zsh
yarn add @emotion/styled
```

### Emotion.jsx

```jsx
/** @jsx jsx */   # おまじない
import { jsx, css } from '@emotion/react';
import styled from '@emotion/styled';

export const Emotion = () => {
  # @emotion/react やり方その1
  const containerStyle_1 = css`
    .container {
      border: solid 1px #392eff;
      border-radius: 20px;
    }
  `;
  # @emotion/react やり方その2
  const containerStyle_2 = css({
    border: 'solid 1px #392eff',
    borderRadius: '20px'
  });

  # @emotion/styled
  const Container = styled.div`
    border: solid 1px #392eff;
    border-radius: 20px;
  `;

  return (
    <div css={containerStyle_1}></div>
    <div css={containerStyle_2}></div>
    <Container>
      <div></div>
    </Container>
  );
}
```

<br />

# Material-UI

`結局これ`

[Material-UI](https://material-ui.com/)
