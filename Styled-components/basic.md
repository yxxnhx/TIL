# Styled-components

- 기존 돔을 만드는 방식인 css, scss 파일을 밖에 두고, 태그나 id, class이름으로 가져와 쓰지 않고, 동일한 컴포넌트에서 컴포넌트 이름을 쓰듯 스타일을 지정하는 것을 styled-components라고 부릅니다.css 파일을 밖에 두지 않고, 컴포넌트 내부에 넣기 때문에, css가 전역으로 중첩되지 않도록 만들어주는 장점이 있다.

### styled components 설치 방법

[https://styled-components.com/](https://styled-components.com/)

```jsx
npm install styled-components
```

- 해당 라이브러리 불러오기

```jsx
import styled from "styled-components";
```

- 익스텐션 설치하기

\***\*vscode-styled-components\*\***

→ 자동완성 도와주는 익스텐션

- **스타일 입히는 법**

어떠한 요소에 스타일을 입히고 싶다면 일반 css처럼 태그로 사용하는 것이 아니라 컴포넌트 형식으로 만들어서 사용해야 한다
스타일은 함수 바깥에 작성해야 한다 내부에 작성하면 그렇게하면 불필요하게 랜더링 될 때마다 계속 css가 걸리는 경우가 있기 때문이다

```jsx
import React from 'react';
import styled from 'styled-components';
const Style = () => {
  return (
    <div>
      <Box> //디자인을 하기 위한 컴포넌트
      </Box>
    </div>
  );

const Box = styled.div`
  width: 500px;
  height: 300px;
  border: 1px solid #000;
  margin: 3rem auto;
  text-align: center;
`
```

→ 변수를 선언하는 것처럼 const를 이용하여 Box 컴포넌트를 불러와 styled.사용하려는 태그명 후 백틱을 찍어서 일반 css 지정하는 것처럼 속성을 지정해주면 된다.

→ 요소 검사를 해보면 div 태그로 박스가 생성이 되어 백틱 내부에 지정한 스타일들이 정상적으로 적용되어있는 것을 확인할 수 있다.

### 그렇다면 box라는 컴포넌트를 또 사용하고 싶은데 이 box는 div가 아니라 ul로 사용하고 싶을 때에는 어떻게 해야 할까?

```jsx
<Box as="ul"></Box>
```

→ 방법은 간단하다 as를 이용하여 본인이 희망하는 태그를 기입해주면 된다

### 동일한 태그에서 각자 다른 스타일을 입히고 싶은 경우

```jsx
<Title>styled component</Title>
<Title>Css IN Js</Title>
<Title>Fun and Easy</Title>

const Title = styled.h2`
  font-size: 2rem;
  color: #f00;
  margin: 1rem 0;
`
```

**타이틀을 모두 동일하게 h2 태그를 이용하였지만 각각 설정을 달리하고 싶을 때에는 어떻게 해야 할까?**

```jsx
<Title fontSize="2rem" color='#fe4d4a'>styled component</Title>
<Title fontSize="1.5rem" color='#eeeb4f'>Css IN Js</Title>
<Title color='#8fbf00'>Fun and Easy</Title>

const Title = styled.h2`
  font-size: ${(props) => props.fontSize};
  color: ${(props) => props.color};
  margin: 1rem 0;
`
```

```jsx
 font-size: ${(props) => props.fontSize};
```

글자 크기를 props로 함수 내부에서 지정한 스타일을 불러온다

→ 이렇게 props를 이용하여 받아오는 것이 styled의 가장 큰 기능이다

**한 번 더 해보자**

```jsx
<Button border="1px solid #00d" color='#00d'>normal</Button>
<Button border="1px solid #e0e0e0" color='#e0e0e0'>disable</Button>
<Button border="1px solid #712" color='#712'>active</Button>

const Button = styled.button`
  width: 150px;
  height: 70px;
  margin: 5px;
  text-align: center;
  line-height: 70px;
  border: ${(props) => props.border};
  color: ${(props) => props.color};
`
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ef3efb59-2a20-4afb-9f6a-1b8e4d52e62c/Untitled.png)

### attribute 추가

⭐️ 백틱``의 내부에는 css만 들어갈 수 있다

```jsx
const Button = styled.button.attrs(props => ({
  type: 'button',
}))`
```

→ 오브젝트처럼 사용을 하여 속성을 추가할 수 있다.

그냥 type은 button이라고 설정을 해도 되나 삼항 연산자를 통해 조건에 따라 속성을 달리하려면 이렇게 props를 이용하여 속성을 받아오는 것이 좋다

**삼항 연산자를 통한 예**

```jsx
<Button border="1px solid #00d" color='#00d' active>normal</Button>
<Button border="1px solid #e0e0e0" color='#e0e0e0'>disable</Button>
<Button border="1px solid #712" color='#712'>active</Button>

const Button = styled.button.attrs(props => ({
  type: 'button',
  className: props.active? 'on': undefined,
}))`
```

→ 첫번째 버튼에만 on이 들어오는 것을 확인할 수 있다

### 애니메이션 만들기

```jsx
import styled, { keyframes } from "styled-components";
```

→ keyframes가 꼭 불러와 있어야 에러가 나지 않는다

```jsx
const colorChange = keyframes`
  0% {background-color: red;}
  100%{background-color: yellow;}
`;

const AniBtn = styled(Button)`
  //위에 적용된 컴퍼넌트가 확장되어 사용할 수 있다
  width: 400px;
  animation: ${colorChange} 1.5s alternate infinite;
`;
```

- **위에 만들어둔 컴포넌트를 불러와서 추가로 사용하고 싶을 때**

```jsx
const AniBtn = styled(Button)``;
```

→ styled(불러올 컴포넌트명)``을 이용하여 불러와서 확장시킬 수 있다

### 글로벌 스타일 만들기

1. GolbalStyle.jsx 파일 만들기

```jsx
import { createGlobalStyle } from "styled-components";

const GlobalStyle = createGlobalStyle``;

export default GlobalStyle;
```

→ 백틱 안에 reset 시킬 혹은 전체적으로 적용시킬 스타일들을 기입하여 저장한다.

1. 최상위 컴포넌트에서 임포트하여 지정한다

```jsx
import "./App.css";
import GlobalStyle from "./component/GlobalStyle";

function App() {
  return (
    <div className="App">
      <GlobalStyle />
      <Style />
    </div>
  );
}

export default App;
```

→ 가장 최상위 객체에 지정을 하여 전체적으로 글로벌 스타일이 지정된다

## **styled components 만들기**

- `const 컴포넌트명 = styled.태그명`스타일 넣기`...`문법으로 만들어집니다.
- 만들고자하는 컴포넌트의 render 함수 밖에서 만든다

### **스타일에 props 적용하기**

- styled-component를 사용하는 장점 중 하나가 변수에 의해 스타일을 바꿀 수 있다.
- 위 예시를 보면 `email`이라는 state값에 따라 `ExampleWrap`에 prop으로 내려준 active라는 값이 true or false로 바뀐다
- styled-component는 내부적으로 props을 받을 수 있고, 그 props에 따라 스타일을 변경할 수 있다

### **스타일 상속**

- `const 컴포넌트명 = styled.스타일컴포넌트명`스타일 넣기`...`문법으로 만들어진다
- 기존에 있는 스타일컴포넌트를 상속받아 재사용

### **Mixin css props**

- css props는 자주 쓰는 css 속성을 담는 변수
- `css 변수명 = css`스타일`...` 로 사용

```jsx
const flexCenter = css`
  display: flex;
  justify-content: center;
  align-items: center;
`;

const FlexBox = div`
  ${flexCenter}
`;

        Copied!
```

### **다른 파일에서 컴포넌트 import**

- 해당 파일에서 정의한 css를 export하여 다른 파일에서 import 할 수 있습니다.

```jsx
// Login.jsx
export const LoginContainer = styled.div`
  background: red;
`;

// Other.jsx
import { LoginContainer } from ".Login";

const Other = () => {
  return <LoginContainer>...</LoginContainer>;
};

        Copied!
```

### **반응형 디자인**

참고: **[styled-components-media (opens new window)](https://styled-components.com/docs/advanced#media-templates)**

```jsx
import React from "react";
import styled, { css } from "styled-components";

const sizes = {
  desktop: 1024,
  tablet: 768
};

// sizes 객체에 따라 자동으로 media 쿼리 함수를 만들어줍니다.
const media = Object.keys(sizes).reduce((acc, label) => {
  acc[label] = (...args) => css`
    @media (max-width: ${sizes[label] / 16}em) {
      ${css(...args)};
    }
  `;

  return acc;
}, {});

const Box = styled.div`
  /* props 로 넣어준 값을 직접 전달해줄 수 있습니다. */
  background: ${props => props.color || "blue"};
  padding: 1rem;
  display: flex;
  width: 1024px;
  margin: 0 auto;
  ${media.desktop`width: 768px;`}
  ${media.tablet`width: 768px;`};
`;

        Copied!
```

### **global css**

- 프로젝트 전역으로 발동하는 css를 만들 수 있다.

```jsx
import styled, { createGlobalStyle } from "styled-components";

export const GlobalStyle = createGlobalStyle`
  body{padding:0; margin:0}
`;

        Copied!
```

### **attribute 추가**

```jsx
const Input = styled.input.attrs({
  required: true
})`
  border-radius: 5px;
`;

        Copied!
```

### **css 모듈 분리**

```jsx
const addCssType = css`
  background-color: white;
  border-radius: 10px;
  padding: 20px;
`;

const Input = styled.input.attrs({
  required: true
})`
  border-radius: 5px;
  ${addCssType}
`;

        Copied!
```

### **global Theme**

- 전역으로 css를 정의할 수 있음. (색, css 등)

1. root 레벨에 공동으로 사용할 theme.js를 만든다

```jsx
// theme.js
const theme = {
  successColor: blue;
  dangerColor: red;
}
export default theme

        Copied!
```

1. ThemePrivider를 프로젝트의 root dir에 import하고 아래와 같이 정의한다

```jsx
import React, { Component } from "react";
import styled, { ThemeProvider } from "styled-components";
import theme from "./theme";

class App extends Component {
  render() {
    return (
      <ThemeProvider theme={theme}>
        <Container>...</Container>
      </ThemeProvider>);
  }
}

        Copied!
```

1. 전역에서 호출하여 사용한다.

```jsx
// Container.jsx
import React, { Component } from "react";
import styled, { ThemeProvider } from "styled-components";
import theme from "./theme";

const Container = () => {
  return <Button>test</Button>;
};

// 아래와 같이 props.theme로 불러서 전역으로 사용한다.
const Button = styled.button`
  border-radius: 30px;
  padding: 25px 15px;
  background-color: ${props => props.theme.successColor};
`;

export default Container;

        Copied!
```

**글로벌 css에 minxin 기능을 넣고 어느 컴포넌트에서나 사용하게 할 수 있다!**

### **global-styled.ts에 mixin을 정의**

```jsx
// global-styled.ts
import { createGlobalStyle } from "styled-components";

const GlobalStyle = createGlobalStyle`
  @mixin flex($direction: 'row', $align: 'center', $justify: 'center') {
    display: flex;
    flex-direction: $direction;
    align-items: $align;
    justify-content: $justify;
  }
`;
export default GlobalStyle;

        Copied!
```

### **global-styled를 웹의 가장 상단 index.js에 import**

```jsx
// src/index.js
import GlobalStyle from "assets/styles/global-styles.ts";

ReactDOM.render(
  <Provider store={store}>
    <GlobalStyle />
    <App />
  </Provider>,
  document.getElementById("root")
);

        Copied!
```

### **이후 아무 컴포넌트에서나 글로벌에서 정의한 mixin기능을 사용 가능**

```jsx
function Test() {
  return (
    <CenterContainer>
      <p>가운데 정렬 엘리먼트</p>
    </CenterContainer>);
}
const CenterContainer = styled.div`
  @include flex(row, center, center);
`;

        Copied!
```

## **styled-components 만 사용하는 변수**

**[Transient props (opens new window)](https://styled-components.com/docs/api#transient-props)**를 사용합니다.

위 api는 v5.1 이상에서 사용 가능하다.

스타일만을 위한 변수가 기본 React 노드로 전달되거나 DOM 요소로 렌더링되는 것을 방지하려면 변수 이름 앞에 `$` 기호를 붙일 수 있다
