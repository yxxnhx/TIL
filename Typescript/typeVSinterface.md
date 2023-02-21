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


![스크린샷 2023-02-20 오후 9 35 13](https://user-images.githubusercontent.com/50559373/220396008-84f17e10-f5f9-402f-b00c-5757b3321775.png)

→ 이제 PositionInterface를 사용하는 곳에서 z가 없다는 에러가 뜨는 것을 확인할 수 있다.

즉, 다른 곳에서 한번, 두번 정의를 했고 각각 정의를 한 것을 합해서 사용해야 하는 곳에서 사용하는 것이 좋다.

**그렇다면 타입은 안될까?**

![스크린샷 2023-02-20 오후 9 37 26](https://user-images.githubusercontent.com/50559373/220396026-0bb9a2f9-4d54-4eb9-8f04-167d01029047.png)

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

![스크린샷 2023-02-20 오후 9 40 20](https://user-images.githubusercontent.com/50559373/220396048-786de4a8-34a3-44ab-9d5c-ba5ff1f51902.png)

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

**그렇다면 이제 개념적인 측면에서 이 둘의 차이를 생각해보자**

**interface의 정의는 무엇일까?**

어떤 것의 규격 사항이다. 다른 사람들과 소통할 때 오브젝트와 오브젝트 간의 의사소통을 할 때 정해진 인터페이스를 토대로 서로간의 상호작용을 할 수 있도록 도와준다.

즉, 쉽게 말하면 서로간의 약속을 할 수 있는 계약서라고 할 수 있다.

**다음의 예시를 보며 다시 생각해보자**

CoffeeMaker 안에서 makeCoffee라는 함수가 들어있다고 정의해보자.

그래서 구현하는 사람들이 이 CoffeeMaker를 활용하여 동일한 규격사항을 따라 다양한 곳에서 CoffeMaker를 활용하여 에스프레소 머신, 바닐라라떼 머신, 돌체콜드브루 머신 등 다양하게 활용할 수 있다고 해보자.

**그렇다면 이 상황에서는 type이 적절할까, interface가 적절할까?**

```tsx
// Type
type CoffeeMaker = {
  coffeeBeans: number;
  makeCoffee: (shots: number) => Coffee;
};

class CoffeeMachine implements CoffeeMaker {
  coffeeBeans: number;
  makeCoffee(shots: number) {
    return {};
  }
}

//Interface
interface CoffeeMaker {
  coffeeBeans: number;
  makeCoffee: (shots: number) => Coffee;
}

class CoffeeMachine implements CoffeeMaker {
  coffeeBeans: number;
  makeCoffee(shots: number) {
    return {};
  }
}
```

→ 이와 같이 어떤 특정한 규격을 정의하는 것이라면, 이 규격을 통해서 어떤 것이 구현된다면 interface를 쓰는 것이 더 정확하다

이처럼 interface는 누군가가 구현할 사람이 있다면 정의해서 사용하는 것이 좋다.

**그렇다면 type은 무엇일까?**

어떠한 데이터를 담을 때 데이터의 모습, 데이터의 타입을 결정하는 것이다.

**다음의 예시를 보자.**

```tsx
interface Position {
  x: number;
  y: number;
}

const pos: Position = { x: 0, y: 1 };
printPosition(pos);
```

→ Position에는 x와 y라는 데이터가 담겨있다.

이런 경우에는 타입을 쓰는 것이 더 정확하다.

```tsx
type Position = {
  x: number;
  y: number;
};

const pos: Position = { x: 0, y: 1 };
printPosition(pos);
```

위와 같이 인터페이스를 통해 데이터를 담고 있다면 다른 사람들이 코드를 볼 때 이 Position 인터페이스를 통해서 다른 것을 구현하는 클래스가 있는가? 에 대한 의문을 가지게 된다.

그러므로 어떠한 규격사항으로 구현하는 것이 아니라 데이터를 담을 목적으로 만든다면 타입을 사용하는 것이 좋다.

타입스크립트 초창기에는 타입이 현재와 같이 강력한 기능이 없어 웬만한 것들은 인터페이스를 이용하여 정의하는 것을 권고하였지만 지금은 점점 타입이 강력해지면서 인터페이스와 타입을 구분지어서 사용하는 것이 좋다.

**다시 한번 정리해보자**

- interface: 규격사항
  → 올바르게 구현하는 것이 목적
- type alias: 데이터의 묘사
  → 데이터를 담을 목적

**조금 더 쉽게 설명해보자면 그냥 문장으로만 놓고 있는 그대로 한번 읽어보자.**

- 이 클래스는 인터페이스를 구현한다(O)
- 이 클래스는 이 타입을 구현한다(???)

- 이 컴포넌트에 전달할 수 있는 Props 타입으로는 이 타입이다(O)
- 이 컴포넌트에 전달할 수 있는 Props 타입으로는 이 인터페이스이다(???)

이 두 가지 예시 문장으로만 보아도 확인할 수 있다.

인터페이스는 규격사항을 정의할 때, 타입은 데이터를 담고 묘사할 때 사용하는 것이 좋다.

다만, 이것은 아직도 많은 개발자들이 이야기가 갈리고 있다. 이 부분은 회사나 팀마다 선호하는 방식이 있다.
