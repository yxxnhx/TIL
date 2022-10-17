# Hook을 이용한 컴포넌트 관심사 분리

## **리액트 훅**

Hook은 함수 컴포넌트에서 React state와 생명주기 기능(lifecycle features)을 “연동(hook into)“할 수 있게 해주는 함수입니다. Hook은 class 안에서는 동작하지 않습니다. 대신 class 없이 React를 사용할 수 있게 해주는 것입니다. (하지만 이미 짜놓은 컴포넌트를 모조리 재작성하는 것은 권장하지 않습니다. 대신 새로 작성하는 컴포넌트부터는 Hook을 이용하시면 됩니다.)

```
import React, { useState } from 'react';

function Example() {
  // "count"라는 새로운 상태 값을 정의합니다.
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}

```

## **useState**

- [Using the State Hook](https://ko.reactjs.org/docs/hooks-state.html)

React는 useState 같은 내장 Hook을 몇 가지 제공합니다. 컴포넌트 간에 상태 관련 로직을 재사용하기 위해 Hook을 직접 만드는 것도 가능합니다. 일단 내장 Hook을 먼저 보겠습니다.

### useState를 호출하는 것은 무엇을 하는 걸까요?

“state 변수”를 선언할 수 있습니다. 위에 선언한 변수는 count라고 부르지만 banana처럼 아무 이름으로 지어도 됩니다. useState는 클래스 컴포넌트의 this.state가 제공하는 기능과 똑같습니다. 일반적으로 일반 변수는 함수가 끝날 때 사라지지만, state 변수는 React에 의해 사라지지 않습니다.

### useState의 인자로 무엇을 넘겨주어야 할까요?

useState() Hook의 인자로 넘겨주는 값은 state의 초기 값입니다. 함수 컴포넌트의 state는 클래스와 달리 객체일 필요는 없고, 숫자 타입과 문자 타입을 가질 수 있습니다. 위의 예시는 사용자가 버튼을 얼마나 많이 클릭했는지 알기를 원하므로 0을 해당 state의 초기 값으로 선언했습니다. (2개의 다른 변수를 저장하기를 원한다면 useState()를 두 번 호출해야 합니다.)

### useState는 무엇을 반환할까요?

state 변수, 해당 변수를 갱신할 수 있는 함수 이 두 가지 쌍을 반환합니다. 이것이 바로 const [count, setCount] = useState()라고 쓰는 이유입니다. 클래스 컴포넌트의 this.state.count와 this.setState와 유사합니다.

### 이제 hook을 이용하여 counter를 만들어보자!

```jsx
let count = 0;
function handleClick() {
  console.log('click');
  count += 1;
  console.log(count);
}

function Counter() {
  return (
    <button type="button" onClick={handleClick}>
      Click me! ({count})
    </button>
  );
}

/* 생략 */
function App() {
  return (
    <div>
      <p>Hello, world!</p>
      <p>Hi!</p>
      <Buttons />
      <Counter />
    </div>
  );
}
```

→ 콘솔에 누른만큼 카운트되는 것을 확인할 수 있다.

그러나 문제는 화면에는 출력이 되지 않는다는 것이다. 어떻게 해결해야 할까?

```jsx
let count = 0;
function handleClick() {
  console.log('click');
  count += 1;
  console.log(count);
  render();
}

function Counter() {
  return (
    <button type="button" onClick={handleClick}>
      Click me! ({count})
    </button>
  );
}

/* 생략 */

function render() {
  ReactDOM.render(<App />, document.getElementById('app'));
}
render();
```

이렇게 지난 js처럼 한다면 react를 사용할 이유가 없는 것이다.

**그렇다면 어떻게 해야할까?**

리액트에서 가장 재미있는 것 중에 하나는 위의 방법처럼 render를 한다는 이러한 상태들을 처리하고 실행을 하고 하는 처리들을 직접하지 않고 그려지기만 한다고 쓰기만 할 수 있다.

지난번에 js에서 let을 사용하지 않고 값을 올리는 방법 중 state를 활용을 하는 방법이 있었다.

```jsx
const state = {
  count: 0,
};
```

그렇다면 이 state 값을 변경하고 처리해주는 것이 있다면 어떨까?

```jsx
function handleClick() {
  setState({
    count: count + 1,
  });
}
```

이와 같이 state 값을 처리하는 setState가 있다고 하면 count가 매번 1씩 올라가게 하는 것이다

### 이처럼 유사하게 하기 위해서 react에서는 useState라는 hook이 있다.

```jsx
import React, { useState } from 'react';

const [state, setState] = useState({
  count: 0,
});
```

→ state의 초기값은 count가 0인 오브젝트를 가지고 있다. 이것들을 새롭게 바꿔줄 때는 setState를 이용하면 된다

즉, state는 handleClick 안에 들어가있다고 생각하면 된다.

```jsx
const { count } = state;
```

그러니 state를 꺼내와서 사용을 해보자

```jsx
import React, { useState } from 'react';

function Counter() {
  const [state, setState] = useState({
    count: 0,
  });

  const { count } = state;

  function handleClick() {
    setState({
      count: count + 1,
    });
  }

  return (
    <button type="button" onClick={handleClick}>
      Click me! ({count})
    </button>
  );
}
```

→ 카운터라는 컴포넌트 안에 관리되고 있는 state라는 상태가 존재하게 되고, 그 state는 setState로 변경되게 되고, 이것들은 다른 전역변수로 접근을 하거나 다른 컴포넌트에서 접근이 불가능하다.

### 그렇다면 좀 더 컴포넌트를 분리해서 가볍게 만들어보자

지금 counter 컴포넌트 안에 상태를 선언하고, 상태를 변화하고, 상태를 보여주는 부분이 한꺼번에 들어있다.

```jsx
function Counter({ count, onClick }) {
  return (
    <button type="button" onClick={onClick}>
      Click me! ({count})
    </button>
  );
}

/* 생략 */

function App() {
  const [state, setState] = useState({
    count: 0,
  });

  const { count } = state;

  function handleClick() {
    console.log('click');
    setState({
      count: count + 1,
    });
  }

  return (
    <div>
      <p>Hello, world!</p>
      <p>Hi!</p>
      <Buttons />
      <Counter count={count} onClick={handleClick} />
    </div>
  );
}
```

마치 a 태그를 쓰는 것처럼

```jsx
<a href=""></a>
```

key와 value를 Counter 컴포넌트에서 props를 받아 사용하는 것이다.

count, onClick을 props로 attribute들을 받는 것이다!

즉 우리는 컴포넌트를 두 개로 나누려고 한다.

하나는 화면에 그려주는 것,

또 하나는 상태를 가지고서 상태를 관리하는 것들이다.

만약 현재 App 컴포넌트에 너무 많은 내용이 담겨있는 것 같다면 한 번 더 분리가 가능하다

```jsx
function Page({ count, onClick }) {
  return (
    <div>
      <p>Hello, world!</p>
      <p>Hi!</p>
      <Buttons />
      <Counter count={count} onClick={onClick} />
    </div>
  );
}

function App() {
  const [state, setState] = useState({
    count: 0,
  });

  const { count } = state;

  function handleClick() {
    console.log('click');
    setState({
      count: count + 1,
    });
  }

  return <Page count={count} onClick={handleClick} />;
}
```

## **선언형 프로그래밍**

프로그래밍 패러다임 중 하나입니다.

선언형 프로그래밍은 정확하게 어떤 명령 혹은 단계들이 실행될지 묘사하지 않고, 프로그램에서 원하는 것 목표만을 기술합니다. 즉 목표만 있을 뿐 어떻게 목표를 달성하는지 기술하지 않는 것입니다.

선언형 프로그래밍의 예로는 데이터베이스 Query language(SQL), Regular expression, 설정 파일, 함수형 프로그래밍 등이 있습니다.

<br />

## **관심사의 분리**

컴퓨터 프로그램을 구별된 부분으로 분리시키는 디자인 원칙으로, 각 부문은 개개의 관심사를 해결합니다. 각 부문의 관심사를 분리하면 코드를 이해하고 보수 하기 훨씬 더 쉬워집니다. 기존의 웹 개발 방식은 관심사의 분리보단 마크업, 디자인, 로직을 분리하는 기술의 분리에 가까웠습니다. 반면 리액트는 컴포넌트별로 관심사를 분리하여 재사용성과 확장성을 높여서 개발을 더 쉽게 해줍니다.
