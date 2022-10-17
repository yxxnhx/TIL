# React

이전에 자바스크립트 파일로 만들었던 것을 한번 jsx로 만들어보자

**파일명을 index.js 에서 index.jsx로 변경을 한 후 npm start를 해보면 어떻게 될까?**

![react](/img/react%20%EC%98%A4%ED%9B%84%209.40.00.png)

→ jsx 파일을 열 모듈을 찾을 수 없다는 에러가 뜨는 것을 확인할 수 있다.

원래 설정이 index.js 파일을 찾아서 열게 설정이 되어 있다.

그런데 현재는 index.js 가 jsx 파일로 변경이 되어 그 파일을 찾을 수 없고 jsx 파일을 열 수 없기 때문에 위와 같은 에러가 나는 것이다.

이 에러는 webpack 설정에서 해결해줄 수 있다

```jsx
const path = require('path');

module.exports = {
  entry: path.resolve(__dirname, 'src/index.jsx'),
  module: {
    rules: [
      {
        test: /\.jsx?$/,
        exclude: /node_modules/,
        use: 'babel-loader',
      },
    ],
  },
};
```

- \_\_dirname : 현재 디렉토리 경로

→ entry를 src 폴더에 있는 index.jsx라고 설정해주는 것이다.

```jsx
entry: 'src/index.jsx';
```

위처럼 설정해줄 수도 있지만 아래와 같이 조금 더 명확하게 해주기 위하여 path.resolve를 이용하여 경로를 확실하게 잡아주는 것이다.

```jsx
entry: path.resolve(__dirname, 'src/index.jsx'),
```

또한 path.resolve는 path라는 표준 라이브러리를 이용하기 때문에 아래와 같이 설정을 해주어야 한다.

```jsx
const path = require('path');
```

그렇게 하면 더이상 에러가 나지 않는 것을 확인할 수 있다.

![react](/img/react%20%EC%98%A4%ED%9B%84%209.41.13.png)

**그렇다면 이제 index.jsx에서 출력해보자**

```jsx
const element = (
  <div>
    <p>Hello, world!</p>
  </div>
);

console.log(element);
```

이렇게 하면 화면과 콘솔에 출력이 될까?

정답은 아니다.

어떻게 화면을 출력하라는지에 대한 선언이 전혀 되어있지 않다.

지난 js를 출력하기 위해서는 다음과 같이 선언을 하였다.

```jsx
/* @jsx createElement */

function createElement(tagName, props, ...children) {
	/* 생략 */
	function render() {
	  const element = (
		/* 생략 */
		)
	document.getElementById('app').textContent = '';
  document.getElementById('app').appendChild(element);
}

render();
```

@jsx createElement의 형식으로 출력을 할건데 createElement라는 함수를 출력해줘

근데 app이라는 id를 가진 아이에게 appendChild로 element를 추가해줄거야

그리고 마지막으로 render 함수를 호출하여서 화면과 콘솔에 출력되게 하였었다.

### 그렇다면 한번 동일하게 설정을 해보자

```jsx
/* @jsx createElement */

function createElement() {
  return 'Hello, world!';
}

const element = (
  <div>
    <p>Hello, world!</p>
  </div>
);

console.log(element);
```

![react](/img/react%20%EC%98%A4%ED%9B%84%209.49.18.png)

콘솔에 hello, world가 출력되는 것을 확인할 수 있다.

### 그렇다면 리액트에서 본격적으로 사용하려면 어떻게 해야할까?

먼저 리액트와 리액트 돔을 설치해주자

```jsx
npm install react react-dom
```

## **React**

- [React 공식 사이트](https://reactjs.org/)
- [리액트의 탄생배경과 핵심 개념](https://soldonii.tistory.com/100)

> A JavaScript library for building user interfaces

React는 UI들을 독립적이고 재사용할 수 있는 부분으로 나누고 각 부분을 분리하여 개발할 수 있는 컴포넌트로 만들 때 도움을 주는 라이브러리 입니다.

리액트는 재사용이 가능한 컴포넌트를 만들고, 이 컴포넌트들이 모여 웹사이트를 구성하게 됩니다. 이 컴포넌트들은 결국 자바스크립트 함수(또는 객체)입니다. state를 세팅하고, 이를 기반으로 화면에 어떻게 보여지기 원하는지를 작성하여 하나의 컴포넌트로 구성합니다.

이후 리액트에 내장된 Component 라이브러리의 기능을 불러온 후, 여기에 내장된 render() 메소드를 통해 ReactDOM 라이브러리에게 rendering될 컴포넌트를 전달합니다. 최종적으로 ReactDOM 라이브러리가 현재 DOM과 전달받은 컴포넌트를 비교하여 변경이 필요한 부분만 변화를 주어 화면에 보여주게 됩니다.

### **ReactDOM**

- [React 공식 사이트](https://reactjs.org/docs/react-dom.html)

`react-dom`은 React에서 DOM에 특화된 메서드를 사용할 수 있도록 API를 제공합니다. 예를들면 `React`요소들을 DOM으로 렌더링하는 `render`함수를 제공합니다.

```
// index.html

// ...
<div id="app"></div>
<script id="main.js"></script>
// ...

```

```
// JSX를 React.createElement가 컴파일 할 수 있도록, React를 직접 사용하지 않더라도 임포트 해줘야 됩니다.
import React from 'react';
import ReactDOM from 'react-dom';

function App() {
  return (
    <div>
      <p>Hello, world!</p>
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('app'),
);

```

- [엘리먼트 렌더링](https://ko.reactjs.org/docs/rendering-elements.html)

리액트 자체는 화면에 그리는 것과 큰 상관이 없다. 그러니 react dom을 함께 설치를 해주어야 한다.

리액트 돔은 브라우저에서 돔객체를 다루는 것이다.

**이제 index.jsx 파일에서 설정을 해보자**

```jsx
*/* @jsx React.createElement */*
```

→ js 파일에서는 creactElement라고만 설정을 해주었지만 원래는 React.creatElement이다.

이대로 실행을 해보면 아래와 같은 에러가 뜬다.

![react](/img/react%20%EC%98%A4%ED%9B%84%209.54.15.png)
→ 리액트가 없다는 에러창이 뜬다.

방금 리액트와 리액트 돔을 설치해주었으니 import를 하여 출력할 수 있도록 해보자

```jsx
import React from 'react';
```

![react](/img/react%20%EC%98%A4%ED%9B%84%209.55.26.png)

→ react element로 타입이 잡히고 어떤게 있는지 콘솔에 출력되는 것을 확인할 수 있다.

### 이것을 화면에 출력해보자

리액트가 만들어낸 엘리먼트들은 우리가 직접 만들어낼 수 없고 react dom을 활용하여 화면에 출력해낼 수 있다.

```jsx
/* @jsx React.createElement */

import React from 'react';
import ReactDOM from 'react-dom';

const element = (
  <div>
    <p>Hello, world!</p>
  </div>
);

ReactDOM.render(element, document.getElementById('app'));
```

![react](/img/react%20%EC%98%A4%ED%9B%84%209.58.01.png)

→ element에 넣은 hello, world가 화면에 출력되는 것을 확인할 수 있다!

이전 js에서는 화면에 출력하기 위해 처리를 해주었어야 했지만 react에서는 리액트와 리액트 돔을 import하기만 해도 이와 같이 화면에 출력되는 것을 확인할 수 있다.

```jsx
*/* @jsx React.createElement */*
```

그리고 위는 기본값이기 때문에 지워도 문제가 없다.

이전 js에서는 react를 활용하지 않고 직접 돔객체를 만들어서 화면에 출력해내었지만 리액트에서는 리액트가 다루는 가상돔을 이용하여 전체적인 관리를 해주고 최종적으로 reactDOM이 받아서 출력을 해준다

이제 element 안에 내용들을 더 추가하여 화면에 출력할 수 있다.

만약 복잡하게 버튼들을 만들어준다고 해보자

```jsx
const element = (
  <div>
    <p>Hello, world!</p>
    <p>Hi!</p>
    {[1, 2, 3].map((i) => (
      <button type="button">{i}</button>
    ))}
  </div>
);
```

![react](/img/react%20%EC%98%A4%ED%9B%84%2010.04.01.png)

**이렇게 map을 이용해서 element에 곧바로 입력해도 되지만 내용들이 점점 더 많아지면 어떻게 해야할까?**

### 버튼을 만들어주는 함수를 만들어서 호출해보자

```jsx
function renderButtons() {
  return (
    <p>
      {[1, 2, 3].map((i) => (
        <button type="button">{i}</button>
      ))}
    </p>
  );
}

const element = (
  <div>
    <p>Hello, world!</p>
    <p>Hi!</p>
    {renderButtons()}
  </div>
);
```

### 그렇다면 버튼도 분리할 수 있지 않을까?

```jsx
function renderButton(value) {
  return <button type="button">{value}</button>;
}

function renderButtons() {
  return <div>{[1, 2, 3].map((i) => renderButton(i))}</div>;
}
```

이런 식으로 조각조각 내는 것을 componant, module을 만든다고 한다.

### 리액트는 컴포넌트라는 개념을 활용한다

여기 있는 renderButton, renderButtons 함수들을 **하나의 태그처럼 사용**을 할 수 있다

다음을 한번 보자

```jsx
function Button() {
  return <button type="button">button</button>;
}

function renderButtons() {
  return (
    <div>
      {[1, 2, 3].map((i) => (
        <Button />
      ))}
    </div>
  );
}
```

![react](/img/react%20%EC%98%A4%ED%9B%84%2010.15.49.png)

→ 버튼이 3개가 생긴 것을 확인할 수 있다.

그렇다면 처음처럼 1, 2, 3 버튼이 생기게 하려면 어떻게 해야할까?

```jsx
function renderButtons() {
  return (
    <div>
      {[1, 2, 3].map((i) => (
        <Button>{i}</Button>
      ))}
    </div>
  );
}
```

먼저 Button을 태그처럼 여닫고 {i}를 받게 해보자

```jsx
function Button(value) {
  return <button type="button">{value}</button>;
}
```

![react](/img/react%20%EC%98%A4%ED%9B%84%2010.17.47.png)

→ value를 받을 수 없다는 에러가 뜨는 것을 확인할 수 있다.

### 그렇다면 어떻게 해결을 해야할까?

Button 함수에서 파라미터로 받을 수 있는 것이 여러개 있지만 원래는 props라는 객체로 들어온다

## **Components & Props**

- [Components and Props](https://reactjs.org/docs/components-and-props.html)

리액트 훅이 나오면서 특수한 케이스를 제외하곤 앞으론 함수 컴포넌트로 개발할 것을 페이스북 측에서도 권하고 있습니다.

컴포넌트도 자바스크립트 함수입니다. 그래서 입력값과 반환값이 있습니다. 여기서 입력값을 리액트에서는 props라고 부릅니다. 컴포넌트의 반환값은 화면에 어떤 엘레먼트를 보여줄지 결정합니다.

```jsx
function Button(props) {
  console.log(props);
  return <button type="button"></button>;
}
```

한번 콘솔에 찍어보자

![react](/img/react%20%EC%98%A4%ED%9B%84%2010.19.38.png)

→ children이라는 이름으로 renderButtons에서 map을 돌린 1, 2, 3이 들어온 것을 확인할 수 있다.

### 그렇다면 한번 children을 버튼에 넣어보자

```jsx
function Button(props) {
  const { children } = props;
  return <button type="button">{children}</button>;
}
```

![react](/img/react%20%EC%98%A4%ED%9B%84%2010.21.36.png)

→ 정상적으로 출력이 되는 것을 확인할 수 있다.

그리고 const {children} = props 와 같이 구조분해할당한 것을 다음과 같이 바로 받아낼 수 있다.

```jsx
function Button({ children }) {
  return <button type="button">{children}</button>;
}
```

### 그렇다면 renderButtons 함수도 변경할 수 있지 않을까?

```jsx
function Buttons() {
  return (
    <div>
      {[1, 2, 3].map((i) => (
        <Button>{i}</Button>
      ))}
    </div>
  );
}

const element = (
  <div>
    <p>Hello, world!</p>
    <p>Hi!</p>
    <Buttons />
  </div>
);
```

### 세번째로 element를 변경해보자

element를 app이라고 설정을 하였기 때문에 element가 app이다

```jsx
function App() {
  <div>
    <p>Hello, world!</p>
    <p>Hi!</p>
    <Buttons />
  </div>;
}

ReactDOM.render(<App />, document.getElementById('app'));
```

그렇다면 element도 App 컴포넌트 만들어서 위와 같이 출력해낼 수 있는 것이다.

즉, 랜더링을 해주는데 어떻게 해주느냐. App 컴포넌트 app이라는 아이디를 가진 아이에게 넣어 출력을 하라는 것이다.

### 우리가 리액트에서 해야할 것은 여러가지 컴포넌트를 만들고 조합하여 만들어내는 것이다!

추가적으로 map을 돌려 여러가지를 만들어낼 때, 각각을 구분해주어야 할 unique한 key가 필요하다

```jsx
function Buttons() {
  return (
    <div>
      {[1, 2, 3].map((i) => (
        <Button key={i}>{i}</Button>
      ))}
    </div>
  );
}
```

이렇게 만들어내는 것들이 실행하는 것처럼 보일 수는 있지만 마치 html처럼 만들어내었는데

이것을 선언적 프로그래밍이라고 한다.

이것은 실제로 무언가를 실행하는 것이 아니라 선언을 한 것이다

이게 실제로는 화면에 출력이 되고 관심사 분리에서 실제로 비지니스 로직을 처리하는 것, 화면에 출력하는 것이 각각 어우러져 흥미로운 작업들이 이루어질 것이다.
