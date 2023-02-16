# Props Drilling

Prop Drilling 은 props를 오로지 하위 컴포넌트로 전달하는 용도로만 쓰이는 컴포넌트들을 거치면서 React Component 트리의 한 부분에서 다른 부분으로 데이터를 전달하는 과정이다.

### 예시

#### < Counter app >

```jsx
import { useState } from 'react';
import ReactDOM from 'react-dom';
import Page from './Page';

function App() {
  const [state, setState] = useState({
    count: 0,
  });

  const { count } = state;

  function handleClick(clickNumber) {
    if (!clickNumber) {
      setState({
        count: count + 1,
      });
    }
    setState({
      count: count + clickNumber,
    });
  }

  return <Page count={count} onClick={handleClick} />;
}

ReactDOM.render(<App />, document.getElementById('app'));
```

```jsx
import NumberButtons from './Buttons/NumberButtons';
import Counter from './Counter/App';

function Page({ count, onClick }) {
  return (
    <div>
      <p>Counter</p>
      <Counter count={count} onClick={onClick} />
      <NumberButtons onClick={onClick} />
    </div>
  );
}

export default Page;
```

```jsx
function NumberButton({ children, onClick }) {
  return (
    <button type="button" onClick={() => onClick(children)}>
      {children}
    </button>
  );
}

export default NumberButton;
```

```jsx
import NumberButton from './NumberButton';

function NumberButtons({ onClick }) {
  const numbers = [1, 2, 3, 4, 5];

  return (
    <div>
      {numbers.map((i) => (
        <NumberButton type="button" key={i} onClick={onClick}>
          {i}
        </NumberButton>
      ))}
    </div>
  );
}

export default NumberButtons;
```

```jsx
function Counter({ count, onClick }) {
  return (
    <button type="button" onClick={() => onClick(1)}>
      Click me! ({count})
    </button>
  );
}

export default Counter;
```

위의 예시는 click me를 누를 때는 1씩, 하단의 숫자를 누를 때에는 숫자만큼 증가하는 간단한 counter 앱이다.
컴포넌트에서 해당 props를 사용하기 위해 전달하는 과정이
`index > App > Page > NumberButtons > NumberButton` 순이다

<br />

## 무엇이 문제일까?

먼저, props 전달이 3~5개 정도 컴포넌트라면 Props Drilling은 문제가 되지 않는다. 하지만 Props 전달이 10개, 15개 같이 더 많은 과정을 거치게 된다면 어떻게 될까? 코드를 읽을 때 해당 props를 추적하기가 힘들어진다.

### 해결 방법

1. 전역 상태관리 라이브러리 사용
   redux, Mobx, recoil 등을 사용하여 해당 값이 필요한 컴포넌트에서 직접 불러서 사용할 수 있다.

2. Children을 적극적으로 사용
   Children이란?
   공식문서에서는 아래와 같이 설명하고 있다.

어떤 컴포넌트들은 어떤 자식 엘리먼트가 들어올지 미리 예상할 수 없는 경우가 있습니다. 범용적인 '박스'역할을 하는 sidebar혹은 Dialog와 같은 컴포넌트에서 특히 자주 볼 수 있다.
