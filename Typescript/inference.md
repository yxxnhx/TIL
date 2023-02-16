# 타입추론 Inference

타입을 명확하게 명시해야 하는 경우보다 자동으로 타입이 정해지는 경우가 있다.

타입추론에 대해서 알아보자.

```jsx
let text = "hello";
```

![스크린샷 2023-02-15 오전 1 45 24](https://user-images.githubusercontent.com/50559373/218814353-e54115d3-a1e5-401b-8c86-ef56cd02a5ec.png)


위처럼 타입을 설정해주지 않아도 자동을 타입 추론을 해서 타입이 string 문자열이라고 나타내주고 있다.

```tsx
function print(message) {
  console.log(message);
}
```

![스크린샷 2023-02-15 오전 1 46 44](https://user-images.githubusercontent.com/50559373/218814388-6ad80655-362a-4b33-881e-5418e5f88906.png)


이처럼 함수에 받을 인자의 타입을 설정해주지 않았다면 암시적으로 any가 붙지만 타입을 설정해주는 것이 좋다고 안내가 뜨는 것을 확인할 수 있다.

**여기에서 디폴트 파라미터를 설정해주면 어떻게 될까?**

```tsx
function print(message = "hello") {
  console.log(message);
}
```

![스크린샷 2023-02-15 오전 1 48 02](https://user-images.githubusercontent.com/50559373/218814478-2d97824c-e328-4276-b458-fd0f703f845b.png)


타입 추론에 의해서 string이 선언된 것을 확인할 수 있다.

**그렇다면 이 경우에는 어떨까?**

```tsx
function add(x: number, y: number) {
  return x + y;
}

const result = add(1, 2);
```

![스크린샷 2023-02-15 오전 1 49 19](https://user-images.githubusercontent.com/50559373/218814498-1b80e708-f25e-4b20-8fdd-e4b9a9549a85.png)


숫자를 받는 함수의 반환값을 result에 할당을 하니 자동으로 타입추론에 의해서 number이 선언된 것을 확인할 수 있다.

이렇게 자동으로 타입을 정해주는 타입추론, 과연 좋을까?

현재의 예시들은 모두 간단하니 쉽게 읽힐 수 있어 편할 것 같지만 실제 코드는 길고 복잡하기 때문에 한눈에 알아보기 어려워 가독성과 명시성이 떨어진다.

그러니 타입을 직접 지정해주는 것이 좋다!
