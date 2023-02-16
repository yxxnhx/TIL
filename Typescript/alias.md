# Type Alias

새로운 타입을 내가 직접 정의할 수 있다.

다음의 예시를 보며 확인해보자

```jsx
type Text = string;
```

이처럼 Text라는 type을 내가 새롭게 string만 받는 타입이라고 선언을 해줄 수 있다.

```
const name: string = 'yxxn';
const nickname: Text = 'yxxn';
```

→ 위와 같이 타입이 string이라고 하는 것과 Text라고 하는 것이 동일한 것을 확인할 수 있다

또한 오브젝트 형태도 정의할 수 있다.

```
type Student = {
  name: string;
  age: number;
};
```

```tsx
const student: Student = {
  animal:
}
```

![스크린샷 2023-02-13 오전 1 06 35](https://user-images.githubusercontent.com/50559373/218813889-cc5ea8d1-e107-4ba8-9b24-6b913117cb21.png)

이처럼 Student라는 오브젝트에 name과 age가 들어간다고 선언을 해두고 다른 것을 할당하려고 할 시에 위와 같은 에러가 뜨는 것을 확인할 수 있다.

## String Literal Types

타입을 문자열로도 지정할 수 있다!

```jsx
type Name = "name";
```

**아니 이게 무슨 말일까?**

타입을 문자열로도 지정할 수 있는데 타입이 name이라니 이게 무슨 소리일까?

아래의 예시를 한번 확인해보자.

```
type Name = "name";
let yxxnName: Name;
yxxnName = "adsf";
```


![스크린샷 2023-02-15 오전 12 02 15](https://user-images.githubusercontent.com/50559373/218813993-dc4fadcd-ef3b-4fbc-8f1d-70b8e3da08c7.png)

yxxnName에는 name이라는 타입을 할당하였기 때문에 name 외에는 그 어떠한 것도 할당을 받을 수 없다.

**그렇다면 이걸 왜 쓰는 것일까?**

문자열을 쓸 때 오타로 인해 발생하는 에러를 방지하는 효과와 오버로드를 구분할 때 사용한다.

**다음을 보면서 확인해보자.**

## Union Type

유니언 타입은 쉽게 설명하자면 `or 또는`이다.

다음 간단한 예시를 보자.

```tsx
type Direction = "left" | "right" | "up" | "down";

function move(direction: Direction) {
  console.log(direction);
}
```

방향을 나타내는 타입을 선언한 후 move 함수를 호출해보자.

![스크린샷 2023-02-15 오전 12 12 55](https://user-images.githubusercontent.com/50559373/218814070-759db6b7-511e-4f95-9759-8a3851678627.png)


→ 할당할 수 있는 가능한 모든 인자를 자동으로 추천이 뜨는 것을 확인할 수 있다.

이처럼 이거 또는 저거 혹은 그것 등으로 모든 가능한 케이스 중에 딱 하나를 담을 수 있는 타입을 담고 싶을 때 유니언 타입을 이용한다.

**그렇다면 문자열 타입만 이용할 수 있을끼?**

아니다. 이처럼 문자열로도 사용할 수도 있고 또 다른 방법이 있다.

아래의 예시를 보자.

```tsx
type MarginSize = 8 | 16 | 32;
const margin: MarginSize = 6;
```

이처럼 숫자로도 가능하다.

다시한번 정리해보자면 Union Type은 발생할 수 있는 케이스 중 하나만 할당할 수 있을 때 쓴다.

이 Union Type은 타입스크립트에서 활용도가 굉장히 높은 타입이다.

**다음 예제를 보면서 확인해보자.**

로그인 함수가 있는데 실패할 수도 성공할 수도 있다면 어떻게 해야할까?

성공했다면 서버에서 response를 반환하고 실패했다면 이유를 알려주는 함수를 만들어보자.

```tsx
type SuccessState = {
  response: {
    body: string;
  };
};

type FailState = {
  reason: string;
};

type LoginState = SuccessState | FailState;

function login(): LoginState {
  return {
    response: {
      body: "logged in",
    },
  };
}
```

이처럼 성공했거나 실패한 경우 케이스 중 하나만 보여주고 싶을 때 유니언 타입을 이용할 수 있는 것이다.
