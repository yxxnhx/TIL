# 성능 개선하기

### 1. useCallback

props로 전달받은 함수를 보존 (deps가 하나라도 변하면 그에 맞는 새로운 함수를 리턴)

**다음의 예시를 보자**

```jsx
export default function AppMentorsReducer(props) {
  const [person, dispatch] = useReducer(personReducer, initialPerson);

  const handleUpdateName = () => {
    const prev = prompt(`누구의 이름을 바꾸고 싶은가요?`);
    const current = prompt(`이름을 무엇으로 바꾸고 싶은가요?`);
    dispatch({ type: 'updatedName', prev, current });
  };

  /* 생략 */

  return (
    <div>
      <h1>
        {person.name}는 {person.title}
      </h1>
      <p>{person.name}의 멘토는:</p>
      <ul>
        {person.mentors.map((mentor, i) => (
          <li key={i}>
            {mentor.name}({mentor.title})
          </li>
        ))}
      </ul>
      <Button text="멘토 이름 바꾸기" onClick={handleUpdateName} />
      <Button text="멘토 타이틀 바꾸기" onClick={handleUpdateTitle} />
      <Button text="멘토 추가하기" onClick={handleAdd} />
      <Button text="멘토 삭제하기" onClick={handleDelete} />
    </div>
  );
}
```

이와 같이 멘토를 삭제하고 추가하고 수정할 때마다 계속해서 Button이 리랜더링이 되고 있는 것을 확인할 수 있다.

그런데 props로 받은 함수를 비교해서 변동이 없다면 그대로 보존하고 deps가 하나라도 변하면 그에 맞게 새로운 함수를 리턴할 수 있도록 해보자.

```jsx
const handleUpdateName = useCallback(() => {
  const prev = prompt(`누구의 이름을 바꾸고 싶은가요?`);
  const current = prompt(`이름을 무엇으로 바꾸고 싶은가요?`);
  dispatch({ type: 'updatedName', prev, current });
}, []);
```

이와 같이 useCallback을 이용하여 감싸주면 더이상 버튼들이 새롭게 리턴되는 것을 막아줄 수 있다.

### 2. useMemo

props로 전달받은 함수를 실행해서, 그 결과값을 보존 (deps=의존인자가 하나라도 변하면 함수를 다시 실행해서 그 결과값을 보존)

**다음의 예시를 보자**

```jsx
const Button = memo(({ text, onClick }) => {
  console.log('Button', text, 're-rendering');
  const result = calculateSomething();
  return (
    <button
      onClick={onClick}
      style={{
        backgroundColor: 'black',
        color: 'white',
        borderRadius: '20px',
        margin: '.4rem',
      }}
    >
      {`${text} ${result}`}
    </button>
  );
});
```

```jsx
function calculateSomething() {
  for (let i = 0; i < 10000; i++) {
    console.log('😵‍💫');
  }
  return 10;
}
```

→ 현재 무거운 일을 하도록 일부러 calculateSomething을 button 컴포넌트가 호출될 때마다 설정해놓았다.

그러므로 button이 호출되고 무언가를 삭제하거나 추가하려고 할 때마다 꽤 오랜 시간이 걸리게 된다.

그러나 만약 버튼이 호출이 될 때 별 다른 일이 없다면 메모, 즉 그 결과값을 보존하게 시켜서 시간을 단축시킨다면 버튼이 호출될 때 성능이 개선될 수 있을 것이다.

```jsx
const result = useMemo(() => calculateSomething(), []);
```

방법 또한 간단하게 useMemo를 활용하여 감싸주어서 의존 인자가 바뀌기 전까지는 기존 결과값을 메모하게 하는 것이다.

### 1. React.memo

단순 UI 컴포넌트의 리렌더링 방지 (props가 들어가는 순간 리렌더링 된다)

**다음의 예시를 보자**

```jsx
export default function AppMentorsReducer(props) {
  const [person, dispatch] = useReducer(personReducer, initialPerson);

  const handleUpdateName = () => {
    const prev = prompt(`누구의 이름을 바꾸고 싶은가요?`);
    const current = prompt(`이름을 무엇으로 바꾸고 싶은가요?`);
    dispatch({ type: 'updatedName', prev, current });
  };

  /* 생략 */

  return (
    <div>
      <h1>
        {person.name}는 {person.title}
      </h1>
      <p>{person.name}의 멘토는:</p>
      <ul>
        {person.mentors.map((mentor, i) => (
          <li key={i}>
            {mentor.name}({mentor.title})
          </li>
        ))}
      </ul>
      <Button text="멘토 이름 바꾸기" onClick={handleUpdateName} />
      <Button text="멘토 타이틀 바꾸기" onClick={handleUpdateTitle} />
      <Button text="멘토 추가하기" onClick={handleAdd} />
      <Button text="멘토 삭제하기" onClick={handleDelete} />
    </div>
  );
}
```

위의 예시에서는 계속해서 버튼 컴포넌트가 랜더링이 되는 점이 있다.

우리가 보기에는 Button 컴포넌트에 props으로 넘어가는 text와 onClick이 변동이 없어보이지만 이 컴포넌트가 리랜더링 될 때마다 실제로는 새롭게 받아오는 것이다.

쉽게 설명해보자면 지금 우리는 string으로 직접 넣어서 눈에 바로 띄진 않지만

```jsx
const buttonText = '멘토 이름 바꾸기';

<Button text={buttonText} onClick={handleUpdateName} />;
```

이렇게 값이 들어가는 것과 동일하다.

이와 같이 계속해서 페이지가 새로고침될 때마다 버튼 컴포넌트가 재랜더링 되는 것을 막기 위해서 memo를 사용하여보자.

```jsx
const Button = memo(({ text, onClick }) => {
  console.log('Button', text, 're-rendering');
  const result = useMemo(() => calculateSomething(), []);
  return (
    <button
      onClick={onClick}
      style={{
        backgroundColor: 'black',
        color: 'white',
        borderRadius: '20px',
        margin: '.4rem',
      }}
    >
      {`${text} ${result}`}
    </button>
  );
});
```

→ 방법은 아주 간단하다. Button 컴포넌트를 memo로 감싸주면 되는 것이다.

### 그렇다면 React.memo로 전부 커버가 안 되는 이유는 무엇일까?

`React.memo`로 전부 커버가 안되는 이유는 props=함수 또한 객체이기 때문에 부모 컴포넌트가 리렌더링 될때마다 얘도 같이 새로운 참조값(주소값)을 가지게 된다.

따라서 `React.memo`는 "아, 새로운 객체(함수)가 props로 들어왔구나" 라고 인식해서 리렌더링이 되어버린다.

### useMemo와 useCallback의 차이는 무엇일까?

`useMemo`와 `useCallback`은 사실 거의 비슷하다.

그러나 그 둘의 차이점을 꼽자면

`useMemo`의 경우에는 함수의 결과값을 가지고 있기 때문에 컴포넌트의 인라인 스타일에 쓰이기도 한다 (물론 인라인 스타일은 가능하면 쓰지 않는게 좋다)

`useCallback`의 경우에는 함수 자체를 가지고 있다

→ 함수 재생성 방지라는 차이를 가지고 있다

리액트 공식문서에 따르면 `useMemo`와 `useCallback`은 다음과 같은 식으로 같은 동작을 수행한다고 한다.

[Hooks API Reference – React](https://ko.reactjs.org/docs/hooks-reference.html#usecallback)

<aside>
💡 `useCallback(fn, deps)`은 `useMemo(() => fn, deps)`와 같습니다.

</aside>

- useCallback

```jsx
const memoizedCallback = useCallback(() => {
  doSomething(a, b);
}, [a, b]);
```

- useMemo

```jsx
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

|               | React.memo                         | useMemo                                               | useCallback        |
| ------------- | ---------------------------------- | ----------------------------------------------------- | ------------------ |
| 종류          | HOC                                | hook                                                  | hook               |
| 클래스/함수형 | 모두 가능                          | 함수형                                                | 함수형             |
| 사용 목적     | 단순 UI 컴포넌트 re-rendering 방지 | 함수의 연산량이 많을 때 그 결과값 재사용(결과값 보존) | 함수의 재생성 방지 |
