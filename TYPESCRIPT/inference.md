# 타입추론 Inference

타입을 명확하게 명시해야 하는 경우보다 자동으로 타입이 정해지는 경우가 있다.

타입추론에 대해서 알아보자.

```jsx
let text = "hello";
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/442d5ea1-4b41-4c47-9b3d-17780acde5f0/Untitled.png)

위처럼 타입을 설정해주지 않아도 자동을 타입 추론을 해서 타입이 string 문자열이라고 나타내주고 있다.

```tsx
function print(message) {
  console.log(message);
}
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7d44495f-5a12-4258-b79e-9bbe41bc874c/Untitled.png)

이처럼 함수에 받을 인자의 타입을 설정해주지 않았다면 암시적으로 any가 붙지만 타입을 설정해주는 것이 좋다고 안내가 뜨는 것을 확인할 수 있다.

**여기에서 디폴트 파라미터를 설정해주면 어떻게 될까?**

```tsx
function print(message = "hello") {
  console.log(message);
}
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e205e669-7c97-438f-ba20-26f8c18021a0/Untitled.png)

타입 추론에 의해서 string이 선언된 것을 확인할 수 있다.

**그렇다면 이 경우에는 어떨까?**

```tsx
function add(x: number, y: number) {
  return x + y;
}

const result = add(1, 2);
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1eaa539a-7fd3-456c-a439-71ffc4014aa4/Untitled.png)

숫자를 받는 함수의 반환값을 result에 할당을 하니 자동으로 타입추론에 의해서 number이 선언된 것을 확인할 수 있다.

이렇게 자동으로 타입을 정해주는 타입추론, 과연 좋을까?

현재의 예시들은 모두 간단하니 쉽게 읽힐 수 있어 편할 것 같지만 실제 코드는 길고 복잡하기 때문에 한눈에 알아보기 어려워 가독성과 명시성이 떨어진다.

그러니 타입을 직접 지정해주는 것이 좋다!
