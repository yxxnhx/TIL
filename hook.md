# Hook

[useCount(custom hook)](https://www.notion.so/useCount-custom-hook-e88a58b333f446f6a12c1144534d9d0d)

### class 없이 state를 사용할 수 있는 새로운 기능

**Hook**은 **갈고리**라는 뜻을 가지고 있는데, 보통 프로그래밍에서는 원래 존재하는 어떤 기능에 마치 갈고리를 거는 것처럼 끼어들어가 수행하는 것을 의미한다.

리액트의 훅도 마찬가지로 **리액트의 state와 생명 주기 기능에 갈고리를 걸어 원하는 시점에 정해진 함수를 실행**되도록 만든 것이다.

그리고 **이때 실행되는 함수를 훅**이라고 부르기로 정한 것이다

### 그렇다면 왜 필요할까?

주로 react는 class companent를 사용해왔는데 거기서 일어났던 불편함들을 보완하기 위해 hook이 탄생했다.

- **class component**

```jsx
import React, { PropTypes } from 'react';

export default class componetName extends Component {
    render()
			return (
        <div>

        </div>
    );
};
```

✓ 더 많은 기능 제공

✓ 더 긴 코드 양

✓ 더 복잡한 코드

✓ 더딘 성능

- **function component**

```jsx
import React from "react";

const componentName = () => {
  return <div></div>;
};

export default componentName;
```

✓ 더 적은 기능 제공

✓ 짧은 코드 양

✓ 더 심플한 코드

✓ 더 빠른 성능

### 그렇다면 함수형 컴포넌트에서 더 적은 기능을 제공한다는데

### 어떤 기능을 클래스 컴포넌트에 비해서 쓰지 못하는 걸까?

**리액트에는 생명 주기(react life cycle)라는 것이 있다**

컴포넌트가 페이지에 렌더링 되기 위해 준비하는 과정부터 제거될 때까지의 기간을 말한다.

클래스형 컴포넌트는 이러한 생명주기 중 특정 시점에 대하여 원하는 구문을 실행할 수 있도록 생명주기 메서드를 지원한다

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d4fbc912-dae8-4292-9fe1-5c6b321404d7/Untitled.png)

리액트가 시작이 될 때 Mounting 생성, Updating 업데이트, Unmounting 제거하는 부분이 있다.

처음 컴포넌트 인스턴스가 생성되고 DOM에 삽입될 때, 생명주기 메서드가 순서대로 실행된다.

**1. constructor(props)**

```jsx
constructor(props) {
super(props); // this.props를 정의하게 됨
this.state = { counter: 0 }; // this.state를 통해 지역 state를 초기화
this.handleClick =this.handleClick.bind(this); // 인스턴스에 이벤트 리스너 바인딩
}
```

컴포넌트 생성자 메소드로 **컴포넌트 마운트 전에 호출**.

이 메소드는 초기 state를 정하거나 이벤트 리스너를 바인딩하기 위해 사용하는 메소드로, 이러한 작업이 필요 없다면 굳이 구현하지 않아도 된다.

React.Component를 상속한 컴포넌트의 생성자를 구현할 때에는 메소드 내부 최상단에 super(props)를 호출해주어야 한다. 그렇지 않으면 this.props가 정의되어 있지 않아 오류가 발생할 수 있다.

**2. static getDerivedStateFromProps(nextProps, prevState)**

```jsx
static getDerivedStateFromProps(props, state) {
    // 주어진 props와 state가 다른 경우 동기화를 위해 state로 설정할 객체를 반환한다.
if (props.counter !== state.counter) {
return { counter: props.counter };
    }
    // 이러한 작업이 필요없는 경우 null을 반환한다.
return null;
}
```

이 메소드는 **컴포넌트** **마운트 또는 업데이트에서 render 메소드를 호출하기 전에 호출**되며 시간의 흐름에 따라 변하는 props를 state에 동기화하기 위해 존재하는 메소드

이는 static과 함께 선언되어야 하며 메소드 내부에서는 this를 통한 인스턴스 접근이 불가

또한, props를 이용하여 생성한 객체를 반환함으로써 props를 state에 동기화한다

**3. render()**

```jsx
render() {
    // jsx를 반환 (배열, null 등 다른 요소 반환 가능)
return ( <div>render</div>)
}
```

이 메소드는 **컴포넌트 마운트 또는 업데이트에서 호출**되며, 생명주기 메소드 중 유일하게 반드시 구현되어야 하는 메소드

**4. componentDidMount()**

```jsx
componentDidMount() {
	this.timerId = setTimeout( () => console.log('Hi') , 1000 );
}
```

이 메소드는 **컴포넌트 마운트 직후에 호출**되며, 라이브러리 또는 프레임워크 함수 호출, 이벤트 등록, setTimeout, setInterval, 네트워크 요청 등의 비동기 작업을 처리하기에 적절한 메소드

이 메소드 내부에서 setState()를 호출하는 경우도 있지만, 이 경우 추가적인 렌더링이 발생하여 성능 저하로 이루어질 수 있으므로 주의해야 한다

두 번째, props 또는 state의 변경, 부모 컴포넌트의 리렌더링, this.forceUpdate의 호출 등으로 인해 컴포넌트 **업데이트**가 발생할 때에는 아래와 같은 생명주기 메소드가 순서대로 실행된다.

**1. getDerivedStateFromProps()**

첫 번째 mounting 생성 단계를 거침

**2. shouldComponentUpdate(props, state)**

```jsx
shouldComponentUpdate(props, state) {
if (this.state.count === state.count) {
return false; // render() 호출 x
    }
return true; // render() 호출 o
}
```

이 메소드는 **컴포넌트 업데이트에서 render 메소드 호출 직전에 호출**되며 해당 컴포넌트를 리렌더링 할지를 결정하는 메소드

기본 반환 값은 true로 기본적으로 render()가 호출되어 리렌더링이 이루어지지만, this.props와 props, this.state와 state의 비교에 따라 false를 반환하는 경우 render()가 호출되지 않아 리렌더링이 이루어지지 않는다.

이는 성능 최적화를 위해 사용하는 메소드로 렌더링을 방지하는 목적으로 사용하는 경우 버그로 이어질 수 있습니다.

**3. render()**

첫 번째 mounting 생성 단계와 동일

**4. getSnapshotBeforeUpdate(props, state)**

이 메소드는 **마지막으로 렌더링 된 결과가 DOM에 반영되기 전에 호출**되는 메서드로, 이를 통해 스크롤 위치 등과 같은 정보를 변경되어 DOM에 반영되기 전에 얻을 수 있다.

이 메서드가 반환하는 값은 componentDidUpdatae()의 세 번째 인자로 전달

**5. componentDidUpdate(props, state, snapshot)**

이 메소드는 컴포넌트 업데이트 직후에 호출되는 메소드로, DOM을 조작하거나 이전과 현재의 props를 비교하여 네트워크 요청을 보내는 작업 등이 이루어진다. 메소드 내부에서 setState를 호출할 수 있으나 **조건문으로 감싸지 않으면 무한 반복**이 발생할 수 있으므로 주의해야 한다.

또한, 이 메소드는 shouldComponentUpdate()가 false를 반환하면 호출✕

마지막으로 세 번째, 컴포넌트가 DOM 상에서 완전히 **제거(Unmount)**될 때는 아래와 같은 생명주기 메소드가 순서대로 실행된다.

**1. componentWillUnmount()**

```jsx
componentWillUnmount() {
    clearTimeout(this.timerId); // componentDidMount()에서 설정한 비동기 작업을 정리
}
```

이 메소드는 **컴포넌트가 언마운트되기 직전에 호출된다**.

즉, 컴포넌트가 사라지기 전에 마지막으로 작업할 수 있는 메소드

따라서, 이 메소드는 앞서 componentDidMount()에서 설정한 모든 비동기 작업에 대하여 정리를 해주어야 한다. 또한, 해당 컴포넌트는 리렌더링 되지 않으므로 setState를 호출하면 안 된다

### 이렇게 컴포넌트 시작, 업데이트, 다운시키기 바로 직전에 해줄 수 있는 것들을 함수형 컴포넌트에서는 사용을 하지 못하였다

그래서 hook이 나오기 전까지는 클래스 컴포넌트를 주로 사용해왔다

### 그렇다면 Hook으로 인한 또 다른 이점이 있을까?

- 일반 클래스 컴포넌트에서 생명 주기를 사용하는 부분

```jsx
component DidMount() {
	//컴포넌트가 마운트 되면 updateLists 함수를 호출
this.updateLists (this.props.id)
}
componentDidUpdate (prevProps) {
if (prevProps.id != this.props.id) {
	//updateLists 함수를 호출할 때
	// 사용되는 id가 달라지면 다시 호출
	this.updateLists (this.props.id)

//updateLists 함수 정의
updateLists = (id) => {
	fetchLists(id)
		.then((lists) => this.setState({
			lists
	}))
}
```

- hook을 이용한 생명주기 이용

```jsx
useEffect(() => {
  fetchLists(id).then((repos) => {
    setRepos(repos);
  });
}, [id]);
```

rerander

변수가 증가하고 감소했을 때 등의 상태 변화를 읽어오지 못함

그러니 상태가 변했다라는 인식을 할 수 있는 변수를 만들어서 적용해야 함

그냥 변수를 만드는 것이 아니라 state에 담아서 사용해야 한다

**● 훅의 규칙**

1. 훅은 무조건 최상위 레벨에서만 호출해야 한다.

2. 반복문이나 조건문 또는 중첩된 함수들 안에서 훅을 호출하면 안된다.

3. 훅은 컴포넌트가 렌더링 될 떄마다 매번 같은 순서로 호출되어야 한다.

**● 플러그인**

eslint-plugin-react-hooks

- > 의존성 배열이 잘못되면 알려줌

![https://postfiles.pstatic.net/MjAyMjA3MjlfMjUx/MDAxNjU5MDc3MTk0OTIw._zl5GE_q75OqqO-A1ZWiwWLqMmR0HQHfeyNDtytZDfog.5mPAFmiUqGdJg3mliTZJR5CcaFvymmZElnmZF8K3ZGog.PNG.kkag8997/httpswww.npmjs.compackage.png?type=w773](https://postfiles.pstatic.net/MjAyMjA3MjlfMjUx/MDAxNjU5MDc3MTk0OTIw._zl5GE_q75OqqO-A1ZWiwWLqMmR0HQHfeyNDtytZDfog.5mPAFmiUqGdJg3mliTZJR5CcaFvymmZElnmZF8K3ZGog.PNG.kkag8997/httpswww.npmjs.compackage.png?type=w773)

**● Custom Hook 만들기**

반복적으로 사용되는 로직을 훅으로 추출해서 만들어 두고 필요할때마다 재사용하기 위해 만든다.

![https://postfiles.pstatic.net/MjAyMjA3MjlfOTEg/MDAxNjU5MDc3MjA1MDYx.7y9mNwWCn6w4-GnXFko6RUAb5RuCtiM1OW3EckbCuBEg.O9Qtv4FTC9YzoG6i4R3p7GrsII_rVJ9I7nGxI_GuULsg.PNG.kkag8997/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2022-07-29_%EC%98%A4%EC%A0%84_10.16.13.png?type=w773](https://postfiles.pstatic.net/MjAyMjA3MjlfOTEg/MDAxNjU5MDc3MjA1MDYx.7y9mNwWCn6w4-GnXFko6RUAb5RuCtiM1OW3EckbCuBEg.O9Qtv4FTC9YzoG6i4R3p7GrsII_rVJ9I7nGxI_GuULsg.PNG.kkag8997/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2022-07-29_%EC%98%A4%EC%A0%84_10.16.13.png?type=w773)

**● Custom Hook 추출하기**

이름이 use로 시작하고 내부에서 다른 훅을 호출하는 하나의 자바스크립트 함수

![https://postfiles.pstatic.net/MjAyMjA3MjlfMjY4/MDAxNjU5MDc3MjE1Njcz.EhACdLsSoB2g1GE174OjGsdsgNYZ6Gtyo7wA2XB7AtMg.iksQo55cDQN2ZKIutLVeufGPPmAnHa4MBqPar0TkweIg.PNG.kkag8997/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2022-07-29_%EC%98%A4%EC%A0%84_10.19.47.png?type=w773](https://postfiles.pstatic.net/MjAyMjA3MjlfMjY4/MDAxNjU5MDc3MjE1Njcz.EhACdLsSoB2g1GE174OjGsdsgNYZ6Gtyo7wA2XB7AtMg.iksQo55cDQN2ZKIutLVeufGPPmAnHa4MBqPar0TkweIg.PNG.kkag8997/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2022-07-29_%EC%98%A4%EC%A0%84_10.19.47.png?type=w773)

**● Custom Hook 사용하기**

![https://postfiles.pstatic.net/MjAyMjA3MjlfMTMx/MDAxNjU5MDc3MjI0MzE2.B4859e3ifUQE48Tomf14WqULxyaFgn0a0kIX0YyK6r4g.fZonemItAkw2FW2BCtbJovBEz5VKt37bSpYY99aKGnkg.PNG.kkag8997/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2022-07-29_%EC%98%A4%EC%A0%84_10.20.55.png?type=w773](https://postfiles.pstatic.net/MjAyMjA3MjlfMTMx/MDAxNjU5MDc3MjI0MzE2.B4859e3ifUQE48Tomf14WqULxyaFgn0a0kIX0YyK6r4g.fZonemItAkw2FW2BCtbJovBEz5VKt37bSpYY99aKGnkg.PNG.kkag8997/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2022-07-29_%EC%98%A4%EC%A0%84_10.20.55.png?type=w773)

Custom Hook을 잘 만들어놓으면 생산성이 향상되고 개발 속도가 빨라진다.

여러개의 컴포넌트에서 하나의 Custom Hook을 사용할 때 컴포넌트 내부에 있는 모든 state와 effects는 전부 분리되어 있다.

각 Custom Hook 호출에 대해서 분리된 state를 얻게 된다.

각 Custom Hook 호출 또한 완전히 독립적이다.

**● Hook들 사이에서 데이터를 공유하는 방법**

![https://postfiles.pstatic.net/MjAyMjA3MjlfMTg3/MDAxNjU5MDc3MjQ2Njky.poE19h-8aO4xxohS4gO3VkZdjXCQ7P8A6MU_JCd3b-Eg.UwdaRVxjen6bl0KzaUKgnWfnhKmnrwPZxrT1nfn8c2kg.PNG.kkag8997/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2022-07-29_%EC%98%A4%EC%A0%84_10.25.36.png?type=w773](https://postfiles.pstatic.net/MjAyMjA3MjlfMTg3/MDAxNjU5MDc3MjQ2Njky.poE19h-8aO4xxohS4gO3VkZdjXCQ7P8A6MU_JCd3b-Eg.UwdaRVxjen6bl0KzaUKgnWfnhKmnrwPZxrT1nfn8c2kg.PNG.kkag8997/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2022-07-29_%EC%98%A4%EC%A0%84_10.25.36.png?type=w773)

![https://postfiles.pstatic.net/MjAyMjA3MjlfMTMx/MDAxNjU5MDc3MjUwNTY3.ErZtH4p7-4UUb50eFrCtwkwQIsKZDmJAfAqBfUAro4sg.oeJQkj50xDD1vgE8iTg2nO0Vm_OkjQ5yEvuAZ3IidqMg.PNG.kkag8997/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2022-07-29_%EC%98%A4%EC%A0%84_10.27.34.png?type=w773](https://postfiles.pstatic.net/MjAyMjA3MjlfMTMx/MDAxNjU5MDc3MjUwNTY3.ErZtH4p7-4UUb50eFrCtwkwQIsKZDmJAfAqBfUAro4sg.oeJQkj50xDD1vgE8iTg2nO0Vm_OkjQ5yEvuAZ3IidqMg.PNG.kkag8997/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2022-07-29_%EC%98%A4%EC%A0%84_10.27.34.png?type=w773)

훅은 컴포넌트 상단에 모아놓는 것이 좋다.
