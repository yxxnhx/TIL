# condition type

```tsx
type Check<T> = T extends string ? boolean : number;
```

→ Check type은 제네릭 타입인데 이 타입이 문자열이면 불리언으로, 아니라면 숫자 타입으로 받겠다고 타입 상태를 설정할 수 있다.

```tsx
type type = Check<string>; // boolean
type type1 = Check<boolean>; // number
```

이것을 이용하면 각각의 타입이 boolean과 number인 것을 확인할 수 있다

그렇다면 다음 예제를 보자.

```tsx
type TypeName<T> = T extends string
  ? "string"
  : T extends boolean
  ? "boolean"
  : T extends number
  ? "number"
  : T extends undefined
  ? "undefined"
  : T extends Function
  ? "function"
  : "object";

type T0 = TypeName<string>; // string
type T1 = TypeName<"hello">; // string
type T2 = TypeName<() => void>; // function
type T3 = TypeName<{}>; // string
```

이처럼 컨디셔널 타입은 어떤 타입이 이런 타입이라면 이 타입을 쓰라는 조건적으로 결정할 수 있는 타입이다.