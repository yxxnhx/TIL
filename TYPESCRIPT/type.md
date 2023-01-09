# Type

기본 타입들에 대해서 알아보자

이런 기본 타입들을 제대로 이해하고 언제 어떤 타입을 써야하는지를 알고 있어야 한다.

그렇다면 왜 타입이 중요할까?

특정한 프로그램을 만들고 프로그래밍을 한다는 것은 크게 세 가지로 정리할 수 있다.

### Input → Operation → Output

입력한 값을 처리해서 다시 내보내는 것

**조금 더 자세히 알아보자.**

**Input**은 키보드로 사용자가 직접 입력할 수도 있지만 파일이나 데이터 베이스에서 읽어오는 것, 센서로 감지되는 것 또는 서버나 다른 기기에서 다른 값을 읽고 받아오는 것도 해당한다.

**Operation**은 사칙연산 뿐만 아니라 작성한 코드, 함수를 실행하는 것 모두 해당한다. 즉, 코드를 작성할 때 if, for, while, switch 등을 이용해서 어떤 특정한 조건, 특정한 구간을 반복, 각각의 케이스별로 실행할 수 있게 할 수 있다.

**Output**은 화면에 보여주는 것 뿐만 아니라 프린터하거나 또는 파일이나 데이터 베이스, 서버나 다른 기기에 데이터를 전송하는 것까지 포함된다.

즉, 프로그래밍을 한다는 것은 input → operation → output이 세 가지를 계속해서 일어날 수 있도록 하는 것이다.

이때, operation하는 동안 잠시 담아두는 곳을 변수라고 한다.

변수는 텅텅 비어진 상자와 동일하다.

이 상자에 원하는 데이터를 담아둘 수 있다.
만약 이 상자에 타입이 정해지지 않았다면 어떠한 데이터든 담아둘 수 있는 것이다.
하지만 타입을 명확하게 명시되어 있다면 그에 해당하는 데이터만 담을 수 있게 될 것이다.
그래서 변수를 만들 때 타입을 정함으로써 이 변수에는 이 데이터만 담을 수 있다고 약속하는 것과 같다.
그 다음 operation을 할 때, 함수에서 input 어떤 특정한 값을 인자로 받아서 코드를 수행한 다음에 output 결과를 리턴한다.

이때 타입이 없다면 어떤 데이터를 넣어야 하는지, 함수가 수행된 다음에 어떤 데이터가 결과값으로 나오는지 알 수 가 없다. 즉, 어떠한 데이터가 나오는지 추론이 불가능하다.

그런데 타입을 정해준다면 어떠한 값을 받아서 어떤 값을 반환하는지를 코드만 보고도 추론이 가능하다.

### 그렇다면 이제 타입이 어떤 것이 있는지 알아보자

자바스크립트에는 타입이 Primitive type, Object type이 있었다.

- **Primitive type 원시 타입**: number, string, boolean, bigint, symbool, null, undefined
- **Object type**: fuction, array 등 Primitive type 외 모든 것

그렇다면 타입스크립트에서는 어떤 타입들이 있을까?

결론부터 말하자면 타입스크립트도 자바스크립트와 동일하다. 다만, 변수를 선언할 때 조금 더 엄격하게 타입을 정의하고 한번 정의된 타입에는 다른 데이터가 들어가지 않는다.

<br />

## Typescript의 타입

- number

```jsx
const num:number ='d’
```

이렇게 number으로 타입을 지정해준 후 string을 할당할 경우 에러가 뜨는 것을 확인할 수 있다.

```jsx
const num: number = -1;
```

정수, 음수, 소숫점 모두 할당이 가능하다.

- string

```jsx
const str: string = 'hello';
```

- boolean

```jsx
const bool: boolean = true;
```

true, false 값만 할당해줄 수 있다.

- undefined

값이 있는지 없는지 아무것도 결정되지 않은 상태

number과 string처럼 먼저 사용해보자.

```tsx
let name: undefined;
name = 'hi';
```

→ undefined 밖에 할당을 해주지 못하기 때문에 string을 받을 수 없다는 에러가 뜨는 것을 확인할 수 있다.
**즉, undefined만 선언된 변수는 없다**

**그렇다면 어떻게 사용해야 할까?**

```tsx
let age: number | undefined;
age = undefined;
age = 1;
```

위처럼 number이 들어올 수도, undefined가 들어올 수도 있다고 선언을 해주는 것이다.
아직 결정되지 않은 undefined일수도, 숫자일 수도 있다는 뜻인 것이다.
옵셔널 타입일 때 많이 사용한다.

- null

조금 더 명확하게 값이 비어있다고 결정되어있는 상태

```tsx
let person: null;
person = null;
person = 1;
```

null도 동일하게 null로만 선언하였을 경우, null 외에는 다른 값이 들어오지 않는다.

```tsx
let people: string | null;
people = null;
people = 'students';
```

undefined와 동일하게 string이 들어올 수도, null 빈 값이 들어올 수도 있다고 설정해주는 것이다.
즉, 데이터가 있을 수도 없을 수도 있을 때 사용하는 것이다.

**그렇다면 이 두가지를 한번 살펴보자**

```jsx
number | undefined;
string | null;
```

무언가의 데이터 또는 undefined 혹은 무언가의 데이터 또는 null
이 두가지는 언뜻 비슷해보이지만 보편적으로는 undefined를 많이 사용한다.

**추가적으로 함수에서 위의 undefined을 사용하는 한 가지 예시를 더 보자.**

```tsx
function find(): number | undefined {
  return undefined;
}
```

무언가를 찾는 find 함수에서 찾았다면 number, 못 찾았다면 undefined를 반환할 수 있도록 데이터타입을 선언해주는 것이다.

이처럼 무언가가 있고 없을 때 undefined을 쓰는 것이다!

- unknown

```tsx
let notSure: unknown = 0;
notSure = 'hi';
notSure = true;
```

unknown 알 수 없는, 이 변수에 어떤 타입이 들어올 수 있을지 잘 모르겠어

→ 초기값을 0으로 잡아주었어도 string과 boolean 타입이 모두 재할당되는 것을 확인할 수 있다.

- any

```tsx
let anything: any = 0;
anything = 'hi';
```

어떤 타입이든 다 담을 수 있다

→ any도 unknown과 마찬가지로 초기값을 0 number로 잡아주어도 string이 재할당 될 수 있다.

위에 살펴보았듯이 타입이 명확하게 정해지지 않았기 때문에 unknown과 any는 **가능하면 사용하지 않는 것이 좋다**

**그렇다면 왜 이런 타입이 있는 것일까?**

타입스크립트는 타입이 없는 자바스크립트와 연동해서 사용할 수 있기 때문이다.

타입스크립트에서 자바스크립트 라이브러리를 이용하는 경우에 자바스크립트에서 리턴하는 타입을 모를 때 unknown을 쓸 수 있다.

- void

자바스크립트에서 알게 모르게 사용했던 타입이다

```tsx
function print() {
  console.log('hello');
  return;
}
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bc5bf265-d6a7-42f2-b32a-0592fb4969c4/Untitled.png)

→ 무언가를 프린트하는 함수, 콘솔에 출력만 할뿐 아무것도 리턴하지 않는 함수에 커서를 올리면 void라고 뜨는 것을 확인할 수 있다.

자동으로 void라고 선언되어 있는 것이다. 즉, 아무것도 리턴하지 않으면 void, 텅텅 빈, 공허한 이라는 의미를 가지고 있다.

보통은 함수에서 무언가를 리턴할 때 타입을 명시하는 것이 좋다.

void인 경우에는 생략할 수 있는데 회사나 프로젝트에 따라서 규칙에 따르는 것이 좋다.

위와 같이 함수에서는 사용되나 아래와 같이 변수에는 사용되는 경우는 드물다.

```jsx
let unusable: void = undefined;
```

왜냐하면 void로 선언하였을 경우에는 undefined 어떠한 값이 들어올지 모르는 상태인 것만 할당이 가능하기 때문이다. 즉, 활용성이 떨어진다.

void 외에도 함수에서 쓰이는 타입이 또 있다.

- never

아무것도 절대 반환하지 않는 함수라니 무엇일까? 그 함수를 호출하면 앱이 죽거나 영원히 중지되어있을텐데

아래의 예시를 참고해보자.

```tsx
function throwError(message: string): never {
  throw new Error(message);
}
```

→ 어떠한 어플리케이션에서 예상치 못한, 핸들링할 수 없는 에러를 호출할 수 있는 함수이다.

에러 메세지를 string으로 받아서 never를 리턴하게 하는 것이다.
그리고 그 메세지를 서버로 로그를 넘기고 그 에러를 어플리케이션에서 에러를 던질 수 있게 하는 것이다. 그 에러를 던지면 어플리케이션은 죽게 되는 것이다.

즉, throwError라는 함수는 리턴하는 값이 절대 없기 때문에 never라고 명시해주면서 이 함수를 호출하면 리턴할 계획이 절대 없으니까 그 점을 감안해서 코딩하라는 뜻과 동일하다.

그렇다면 이와 동일하게 never를 쓸 수 있는 경우는 또 무엇이 있을까?

```tsx
function throwError(message: string): never {
  while (true) {
    //
  }
}
```

→ 바로 while문을 true로 설정하여 계속해서 while 문을 돌게 만들어주는 것이다. while문을 계속 돌면 끝나지 않기 때문에 리턴할 값이 없으므로!

위의 두 가지 경우 외에 never가 아닐 경우에 never를 쓰게 된다면 에러가 뜨는 것을 확인할 수 있다.

```jsx
let neverEnding: never;
```

또한 위와 같이 변수에 never라고 선언해서 사용하는 경우도 없다.

- object

```tsx
let obj: object;
function acceptSomeObject(obj: object) {}
```

→ object라고 타입을 선언하고 인자로 오브젝트를 받을 수 있게 설정하면 어떤 타입의 데이터도 담을 수 있다.

```tsx
acceptSomeObject({ name: 'yxxn' });
acceptSomeObject({ animal: 'cat' });
```

어떠한 오브젝트도 다 담을 수 있는 것이다. 이렇게 이름을 담을수도, animal을 담을 수도 있다. 전혀 에러가 나지 않는 것을 확인할 수 있다.

이처럼 오브젝트 타입은 원시 타입을 제외한 모든 오브젝트 타입을 할당할 수 있다.

심지어 아래와 같이 배열을 받을 수도 있다.

```jsx
let obj: object = [1, 2, 3, 4];
```

→ 이처럼 광범위한, 추상적인, 어떠한 것이든 다 담을 수 있는 이런 타입은 굉장히 좋지 않다.

오브젝트 타입 또한 가능하면 쓰지 않는 것이 좋다. 가능하면 구체적으로 오브젝트도 어떠한 타입인지 명시하여 쓰는 것이 좋다.
