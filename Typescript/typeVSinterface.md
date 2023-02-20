# Type alias와 Interface의 차이점

타입스크립트 초창기에는 타입과 인터페이스가 서로 다른 점이 매우 많았다.

인터페이스가 할 수 있는 일들이 더 많았다.

그래서 이전에는 타입보다는 인터페이스를 쓰는 것을 권고하고 있었다.

그러다보니 이전 코드들에서는 여기저기서 인터페이스를 남발하고 있는 것을 목격할 수 있다.

그러나 이렇게 마구 남발하는 것은 좋지 않다.

타입과 인터페이스는 서로 성격과 특징이 엄연히 다르기 때문에 구분지어서 목적과 성격에 알맞게 구분지어 사용하는 것이 좋다.

**그렇다면 이제 정확히 타입과 인터페이스는 어떤 성격을 가지고 있는지, 어떤 곳에서 어떤 것을 쓰면 좋은지에 대해서 알아보자.**

먼저 구현사항에 초점을 두고 각각의 차이점과 공통점을 알아보자

```tsx
type PositionType = {
  x: number;
  y: number;
};

interface PositionInterface {
  x: number;
  y: number;
}
```

→ 현재 PositionType과 PositionInterface는 동일한 데이터를 담고 있다.

### 공통점

- 오브젝트 형식으로 만들 수 있다.

```tsx
const obj1: PositionType = {
  x: 1,
  y: 1,
};

const obj2: PositionInterface = {
  x: 1,
  y: 1,
};
```

→ 이처럼 타입과 인터페이스 모두 오브젝트 형식으로 정의하고 타입을 할당할 수 있다.

- class에서 구현이 가능하다

```tsx
class Pos1 implements PositionType {
  x: number;
  y: number;
}

class Pos2 implements PositionInterface {
  x: number;
  y: number;
}
```

→ 타입과 인터페이스 모두 클래스에서 구현이 가능하다.

- extends 확장이 가능하다

```tsx
interface ZPositionInterface extends PositionInterface {
  z: number;
}

type ZPositionType = PositionType & { z: number };
```

→ interface는 extends를 통해, type은 인터섹션을 통하여 PositionType과 z를 함께 가질 수 있도록 확장할 수 있다.

### 차이점

- interface만 결합이 가능하다.

```tsx
interface PositionInterface {
  x: number;
  y: number;
}

interface PositionInterface {
  z: number;
}
```

→ 이와 같이 앞에서 선언하였다가 뒤에서 추가로 더 선언을 해주어 결합하는 것이 가능하다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b28273e8-1f94-4eee-830f-441fa4b3d855/Untitled.png)

→ 이제 PositionInterface를 사용하는 곳에서 z가 없다는 에러가 뜨는 것을 확인할 수 있다.

즉, 다른 곳에서 한번, 두번 정의를 했고 각각 정의를 한 것을 합해서 사용해야 하는 곳에서 사용하는 것이 좋다.

**그렇다면 타입은 안될까?**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1d332431-8639-4162-a038-9427eaa0ed48/Untitled.png)

→ 바로 식별자가 중복되었다는 에러가 뜨는 것을 확인할 수 있다.

**그렇다면 타입만 되는 것은 없을까?**

- computed 프로퍼티를 이용할 수 있다.

```tsx
type Person = {
  name: string;
  age: number;
};

type Name = Person["name"]; // string
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9efd1203-b16a-4b24-baac-bf0ad7042381/Untitled.png)

→ name의 타입을 Person의 name의 타입을 사용하겠다고 설정이 가능하다.

- 새로운 타입을 정의할 수 있다.

```tsx
type NumberType = number;
```

- 유니언 타입을 만들 수 있다.

```tsx
type Direction = "top" | "bottom" | "left" | "right";
```

→ 이러한 유니언 타입은 인터페이스로는 정의할 수 없다
