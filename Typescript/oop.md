# Class 이용하기

private static 이나 public static 모두 클래스 내부에서 변경되지 않는 상수를 선언할 때 활용됩니다.
기본적으로, 클래스는 특정 요소가 지녀야 하는 상태 (state)과 행동 (methods) 들을 한곳에 모아둔 요소(entity) 라고 생각하면 됩니다. 따라서 클래스 내부에는 해당 클래스가 지녀야 하는 요소만 담는게 합리적인 클래스 구성이겠죠.
그렇다면, 소스코드 내에서 공통적인 상수를 활용해야 하는 경우를 살펴봅시다.
클래스를 활용하지 않는 자바스크립트에 익숙하신 경우라면 아마 const BEANS_GRAM = 7; 와 같은 방식으로 상수를 선언하셨을거에요. 그리고 아래와 같이 코드 내부에서 이 값을 활용하겠죠.

```ts
let totalBeans = 40;
const BEANS_GRAM_PER_SHOT = 7;

const makeCoffee = (shots: number) => totalBeans - (BEANS_GRAM_PER_SHOT \* shots)
```

그렇다면, 클래스를 활용한다면 어떻게 이 상수를 쓸 수 있을까요?

```ts
let totalBeans = 40;
const BEANS_GRAM_PER_SHOT = 7;

class CoffeeMachine {
  constructor() {}
  makeCoffee(shots: number) {
  totalBeans - (BEANS_GRAM_PER_SHOT \* shots)
  }
}
```

이런식으로 구현이 가능하겠죠.
그런데 생각해보면, 클래스는 특정 요소가 지녀야 하는 상태 (state)과 행동 (methods) 들을 한곳에 모아둔 요소(entity) 라고 위에서 설명드렸어요.
커피머신은 콩이 총 얼만큼 들어있는지, 그리고 샷 한잔에 콩이 얼만큼 소모될지 설정할 수 있어야겠죠? 따라서 위에 선언되었던 상수들이 클래스 외부에 존재하는건 클래스를 활용하는 의미와 어긋납니다. 그런 이유로, static field를 활용해 클래스 내부에서 활용가능한 상수를 제공해주는거에요. 아래 코드처럼요.

```ts
class CoffeeMachine {
  static BEANS_GRAM_PER_SHOT = 7;
  public beans: number;

  constructor(beans: number) {
    this.beans = beans
  }

  makeCoffee(shots: number) {
   this.beans - (CoffeMachine.BEANS_GRAM_PER_SHOT \* shots)
  }
}
```

이 코드를 보면, 커피머신에는 샷을 추출할때 소모되는 콩의 그램과, 커피머신이 갖고있는 콩의 양을 갖고있죠?
그런데 아래코드를 한번 봐볼게요. 외부에서 함부러 커피머신에 있는 콩의 양이나 소모되는 콩 그램을 바꾸고 있습니다.

```ts
// app.js
import CoffeeMachine from "./CoffeeMachine";

const coffeeMachine = new CoffeeMachine(100);

// 내 맘대로 커피콩 수를 올릴거야
coffeeMachine.beans = 80000000;

// 세상에서 제일 흐리멍텅한 커피를 타주마 하하하
CoffeeMachine.BEANS_GRAM_PER_SHOT = 1;
```

위 코드를 보면, 커피머신 클래스 자체에서 활용 / 조작되어야 하는 상태들이 외부에서 마음대로 변경되고 활용되고 있습니다. 이건 개발자가 의도하지 않았던 요소들이죠.
따라서 클래스 외부에서 사용하지 못하도록 private 필드로 설정을 하게 됩니다.

```ts
class CoffeeMachine {
private static BEANS_GRAM_PER_SHOT = 7;
private beans: number;

constructor(beans: number) {
  this.beans = beans
}

  makeCoffee(shots: number) {
    this.beans - (CoffeMachine.BEANS_GRAM_PER_SHOT \* shots)
  }
}
```

위 처럼 각 필드들을 private으로 선언해두면, 해당 상수를 활용해야 하는 클래스 내에서는 마음껏 사용이 가능하지만, 클래스 외부에서는 해당 상수값을 활용할 수 없습니다.
