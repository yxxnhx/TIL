# Enum

여러가지의 관련된 상수 값들을 모아서 정의할 수 있게 도와주는 타입이다.

자바스크립트에서는 상수를 따로 모아서 정의를 하지 않기 때문에 enum에 대해서 생소할 것이다.

```tsx
const MAX_NUM = 999;
const MAX_STUDENTS_PER_CLASS = 120;
```

자바스크립트에서 상수를 정의할 때, 한번 정해지면 바뀌지 않는 특정한 고정값을 나타낼 때 전부 대문자로 이용해왔다.

```tsx
const MONDAY = 0;
const TUESDAY = 1;
const WEDNESDAY = 2;
```

이처럼 상수들이 연관되어있어도 하나로 묶어서 관리할 수 있는 기능이 없었다.

굳이 묶어서 사용하게 된다면 배열의 freeze를 활용해서 사용할 수 있었다.

```
const DAYS_ENUM = Object.freeze({
  MONDAY: 0,
  TUESDAY: 1,
  WEDNESDAY: 2,
});

const dayOfToday = DAYS_ENUM.MONDAY;
```

그렇다면 타입스크립트에서는 어떻게 사용할 수 있는지 확인해보자.

```tsx
enum Days {
  Monday,
  Tuesday,
  Wednesday,
  Thursday,
  Friday,
  Saturday,
  Sunday,
}

console.log(Days.Monday); //0
console.log(Days.Sunday); //6
```

이렇게 enum을 활용해서 상수들을 묶어서 사용이 가능하다.

자동으로 Monday부터 0으로 시작되어 0이 출력된 것을 확인할 수 있다.

만약 1부터 시작하고 싶다면 1을 할당해주면 자동으로 번호가 매겨진다.

```tsx
enum Days {
  Monday = 1,
  Tuesday,
  Wednesday,
  Thursday,
  Friday,
  Saturday,
  Sunday,
}

console.log(Days.Monday); //1
console.log(Days.Sunday); //7
```

문자열 할당 또한 가능하다!

```tsx
enum Days {
  Monday = "mon",
  Tuesday = "tue",
  Wednesday = "wed",
  Thursday = "thu",
  Friday = "fri",
  Saturday = "sat",
  Sunday = "sun",
}

console.log(Days.Monday); // mon
console.log(Days.Sunday); // sun
```

그런데 enum의 치명적인 단점이 있다.

바로 재할당이 가능하다는 것이다.

```tsx
let day: Days = Days.Saturday;
console.log(day); // sat
day = Days.Tuesday;
console.log(day); // tue
```

그래서 다양한 값을 모아서 사용해야 할 때에는 Union 타입을 이용하는 것이 좋다.

```jsx
type DaysOfWeek = "Monday" | "Tuesday" | "Wednesday";
```

enum을 사용할 수 있다면 대부분 union 타입으로도 충분히 대체해서 사용이 가능하다.

그러다 모바일 클라이언트 안드로이드나 ios의 경우 코틀린이나 스위프트를 이용하기 때문에 사용자의 데이터를 json으로 묶어서 보내는데 클라이언트 측에서 사용하는 네이티브 프로그래밍 언어에서는 union 타입을 표현할 수 있는 방법이 없다. 그래서 그럴 경우에는 어쩔 수 없이 enum 타입을 이용하지만 그 외에 대체할 수 있는 경우에는 enum 타입의 사용을 지양하는 것이 좋다.
