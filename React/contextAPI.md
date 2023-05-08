# Context API

모든 컴포넌트들이 공통적으로 필요한 state라면 어플리케이션 전반적으로 사용되는 데이터라면 context api를 사용할 수 있다.

**그렇다면 그 예시는 무엇이 있을까?**

언어, 테마(다크모드), 로그인 정보

**그렇지만 주의해야 할 점이 있다.**

context api를 사용하는 환경에서 context api의 상태가 변경이 되면 즉, 사용자가 언어를 바꾸거나 테마를 바꾸거나 혹은 로그인 정보가 바뀌게 되면 context api를 사용하는 모든 자식 컴포넌트들은 상태가 바뀌기 때문에 모두 re-rendering하게 된다

그러므로 빈번히 업데이트 되는 상태에서는 사용하지 않는 것이 좋다.

그렇다면 어떻게 사용해야할까?

umbralla 테크닉을 사용해보자

**한번 직접 사용해보자**

컨텍스트를 만들 때에는 먼저 리액트에서 제공하는 createContext를 호출하여 설정해준 후 프로바이더를 필수로 만들어주어야 한다

```jsx
import { createContext, useState } from "react";

export const DarkModeContext = createContext(); //데이터를 담고 있다

export function DarkModeProvider({ children }) {
  const [darkMode, setDarkMode] = useState(false);
  const toggleDarkMode = () => setDarkMode((mode) => !mode);

  return (
    <DarkModeContext.Provider value={{ darkMode, toggleDarkMode }}>
      {children}
    </DarkModeContext.Provider>
  );
} //데이터를 잘 가지고 보여주는 우산
```

프로바이더도 결국엔 컴포넌트이다 즉 하위 컴포넌트들을 감싸줄 수 있는 부모 우산 컴포넌트

즉, 다시 정리해보자면 우리가 리액트에서 context를 쓰는 것은 우산을 만든다고 생각하면 쉽다. 그리고 그 우산을 씌워주고 싶은 컴포넌트 위에다 씌우면 그 우산을 쓴 컴포넌트 하위에 있는 모든 자식 요소들이 그 우산에 있는 데이터에 접근이 가능하다.

그 우산, context를 만들려면 우선 리액트에서 제공하는 createContext를 이용하여 context를 만든다.

그 후 컴포넌트를 만들어서 children, 자식 컴포넌트들을 받아오는 프로바이더 컴포넌트를 만들어서 이 자식 컴포넌트들을 리액트에서 제공하는 프로바이더로 감싸주면 된다.

그런다음 자식 컴포넌트들과 공유하고 싶은 데이터가 있다면 value에 지정해주면 된다.

우리는 다크모드인지 아닌지 자식컴포넌트에서도 토글링하기를 원하므로 다크모드인지 아닌지를 확인하는 상태변수 darkMode와 그것을 토글링할 수 있는 toggleDarkMode 함수까지 children에게 다 제공을 해주는 것이다.

여기에서 key는 darkMode, value도 darkMode이기 때문에 생략하여 darkMode, toggleDarkMode로 넘겨줄 수 있는 것이다.

그렇다면 이것들을 어떻게 사용할 수 있을까?

**자식 컴포넌트들에게 우산을 씌워주러 가보자!**

씌워주는 방법은 굉장히 간단하다!

```jsx
export default function AppTheme() {
  return (
    <DarkModeProvider>
      <Header />
      <Main />
      <Footer />
    </DarkModeProvider>
  );
}
```

씌워주고 싶은 곳에 감싸주면 끝

그렇다면 이제 실제 사용할 곳에서 useContext를 이용하여 사용해보자!

```jsx
function ProductsDetail() {
  const { darkMode, toggleDarkMode } = useContext(DarkModeContext);
  return (
    <div>
      ProductsDetail
      <p>
        DarkMode:
        {darkMode ? (
          <span style={{ backgroundColor: "black", color: "white" }}>
            DarkMode
          </span>
        ) : (
          <span>Light Mode</span>
        )}
      </p>
      <button onClick={() => toggleDarkMode()}>Toggle</button>
    </div>
  );
}
```

context에서 받아온 값들을 useContext를 활용하여 쉽게 사용할 수 있다.
