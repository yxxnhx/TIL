# import React from 'react' 생략하기

## React 17, 새로운 jsx 변환 도입 시작

### jsx 변환이란?

`jsx`는 브라우저에서 랜더링하기 위해 `React` 구성 요소에서 사용하는 `HTML`과 유사한 구문이다.

#### 예시

```jsx
import React from 'react';

function App() {
  return <h1>Hello, world!</h1>;
}
```

그러나 브라우저는 JSX가 유효한 JavaScript 코드가 아니기 때문에 이것을 이해하지 못한다.

여기서 Babel 과 같은 트랜스파일러가 등장한다.
Babel 플러그인은 `@babel/plugin-transform-react-jsx` JSX를 JavaScript 엔진이 구문 분석할 수 있는 표준 JavaScript 객체로 변환해준다.

<br/>

#### babel ver

```jsx
import React from 'react';

function App() {
  return React.createElement('h1', null, 'Hello, world!');
}
```

그러나 `import React from 'react';`위의 코드가 작동하도록 사용하고 있다.

트랜스파일 전에 React 에 대한 참조가 없지만 JSX가 포함된 모든 파일에서 React를 가져와야 했다.

```jsx
React.createElement('h1', null, 'Hello, world!');
```

이전 버전의 React(버전 < 17)에서 JSX 변환은 JSX 범위에 있는 React 이름에 의존했다.
즉, JSX를 사용하려면 'React'를 가져와만 한다.

이제 React 가 JSX 변환의 새 버전이 업그레이드되어 React 및 React와 유사한 라이브러리는 JSX 요소 생성을 최적화할 수 있다.

React 17 및 Babel 버전 v7.9.0부터 `@babel/plugin-transform-react-jsx`플러그인은 필요할 때 새 React 패키지에서 "특수 기능"을 자동으로 가져오므로 수동으로 포함할 필요가 없다.

새로운 React 패키지

- react/jsx-runtime
- react/jsx-dev-runtime

Babel과 같은 트랜스파일러에서만 사용하도록 되어 있다.

<br />

### 이제 아래와 같이 JSX를 작성할 수 있다.

```jsx
function App() {
  return <h1>Hello, world!</h1>;
}
```

```jsx
// Inserted by a transpiler (don't import it manually)
import {jsx as \_jsx} from 'react/jsx-runtime';

function App() {
return \_jsx('h1', { children: 'Hello, world!' });
}
```

`{ "runtime": "automatic" }` Babel에서 새로운 JSX 변환에 대한 지원을 수동으로 추가하려면
다음과 같이 `{ "runtime": "classic" }` (기본값) `@babel/preset-react`또는` @babel/plugin-transform-react-jsx)`다음으로 전달할 수 있다.

```
module.exports = {
  presets: [
    /* 생략 */
    [
      '@babel/preset-react',
      {
        runtime: 'automatic',
      },
    ],
  ],
};

```

### 새로운 JSX 변환 장점

- JSX를 작성하기 위해 더 이상 "React"를 가져올 필요가 없다
- 애플리케이션의 번들 크기가 약간 감소
