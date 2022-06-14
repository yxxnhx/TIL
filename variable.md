## 변수 Variable

어플리케이션을 실행하면 크게 총 세 가지의 일이 일어나게 된다

![Untitled](https://t1.daumcdn.net/cfile/tistory/991815435AC727BD27)

1. 입력(input)
2. 처리(process)
3. 출력(output)

사용자에게 입력을 받아서 그 입력을 처리하여 출력을 하게 된다.

출력이라하면 모니터에 나타나거나 네트워크 통신을 통하여 백앤드에게 서버를 보내는 것도 포함한다.

우리가 무언가를 처리, process, 계산하기 위해서는 데이터를 처리할 변수가 필요하다

사용자에게 입력받은 데이터를 더하거나 빼거나 합하거나 하는 등의 데이터를 저장하기 위한 공간이 필요하다.  


즉, 변수란 값을 저장하는 공간, 자료를 저장할 수 있는 이름이 주어진 기억 장소이다

-> let / const와 같은 keyword를 사용해야 함  
</br>
</br>
</br>
```jsx
let a;
```
- 키워드 - let

- 변수 이름 - a

- 문의 끝 - ;

### 이것이 변수 선언 variable decleration

이것이 컴퓨터에게 야 나 지금 a라는 변수를 사용할거야! 하고 알려주는 것이다

값을 할당하면서 변수를 쓰기 위해서는 

```jsx
let a = 0;
```
- 키워드 - lwt

- 변수 이름 - a 

- 할당 연산자 - =

- 값 - 0

- 문의 끝 - ;

내가 원하는 값을 할당하면 된다
</br>
</br>
</br>
## 변수 선언과 값의 할당 variable declatation and assignment

나중에 값을 재할당이 가능하다

```jsx
a = 1;
```
</br>
</br>
</br>
## 값의 재할당 가능 value reassigning

변수라는 것은 계속 변화하는 것이니 내가 원하는 값을 줄 수 있다.
</br>
</br>
</br>
## 변수 이름 짓기  Naming Variables

변수란, 값을 저장하는 공간이자 자료를 저장할 수 있는 이름이 주어진 기억 장소이므로

저장된 값을 잘 나타낼 수 있게 의미있는 이름이어야 한다

→ 구체적일수록 좋다!

변수를 만들 때 예약어들은 사용하면 안된다

# 데이터 타입

- number 숫자
- string 글자
- boolean true / false
- empty 텅 빈 데이터
- object 객체

### 원시 primitive

→ 단일 데이터

즉, 딱 하나의 단일한 데이터

number, BigInt, string boolean, nullm undefined symbol
</br>
</br>
</br>
## 객체 object

→ 복합 데이터

array, function, 원시 타입이 아닌 모든 데이터
</br>
</br>
</br>
# 숫자 타입 number

숫자는 정수, 음수, 실수, 2진수, 8진수, 16진수 모두 출력 가능하다

```jsx
let integer = 123; *//정수*
let negative = -123 *//음수*
let double = 1.23; *// 실수*

let binary = 0b1111011; *// 2진수*
let octal = 0o173; *//8진수*
let hex = 0x7b; *// 16진수*
```

### 단, 숫자를 사용할 때 나누기를 주의해야 한다.

예를 들면 0을 123으로 나누게 되면 0이 나오게 된다,

그런데 어떤 특정한 값을 0으로 나누게 되면 infinity로 나오고 숫자를 text로 나누게 되면 NaN이 된다.

아래의 예를 보자

```jsx
console.log(0 / 123); // 0
console.log(123 / 0); // Infinity
console.log(123 / -0); // -Infinity
console.log(123 / 'text'); // NaN (Not a Number)
```

### Infinity란 무엇인가?

점점 작은 숫자로 나눌수록 숫자의 길이가 늘어나므로 무한정으로 늘어날 수 있다.
그런데 number가 가질 수 있는 가장 큰 값은 1.8E308인데 0으로 나누면 무한정으로 숫자의 길이가 늘어나므로  infinity가 나온다.
</br>
</br>
### NaN이란 무엇인가?

Not a Number의 말로 숫자가 아니라는 뜻이다.

→ 이렇게 특정한 값을 0으로 나누거나, text로 나누어 출력될 수 있는 부분을 조심해야 한다.

참고 자료: https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference
</br>
</br>
### +) bigInt

정말 큰 숫자를 나타내는 타입으로 큰 숫자를 사용할 때에는 숫자 뒤에 n을 붙여서 사용해야 한다.
</br>
</br>
# 문자열 타입 string

문자열을 나타낼 때에는 쌍따옴표 / 따옴표를 사용하여 작성해야 한다.

혹은 백틱, 꺽쇠를 사용해서 입력이 가능하다.

### 그렇다면 문자열을 쓸 때 따옴표를 넣어서 출력하고 싶을 때에는 어떻게 해야 할까?

→ 외부에 사용되는 따움표와 반대되는 따옴표를 사용하여 입력하면 가능하다

```jsx
console.log('"안녕!"')
```
</br>
</br>
## 특수문자 출력 방법

- **\n** - 한줄 띄어쓰기

- **\t** - tab

- **\\** = \

백슬래쉬를 사용하고 싶다면 두 번 입력해야 출력이 가능하다

- **\유니코드 번호** - 유니코드 입력

→ 백슬레쉬는 일반 문자 입력모드를 탈출해서 특수문자를 사용할 수 있는 모드로 들어갈 수 있다.

참고 자료 : https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String
</br>
</br>
### 템플릿 리터럴(Template Literal)

꺽쇄를 이용하여 입력하는 것을 템플릿 리터럴이라고도 한다.

```jsx
let id = '엘리';
let greetings = "'안녕" + id + "😀\n 즐거운 하루 보내요"
console.log(greetings);
```

우리가 여러가지를 합쳐서 출력을 하고 싶을 때에는 위와 같이 따옴표와 +를 이용하여 입력을 한다.

이렇게 복잡한 방법 대신에 한번에 입력할 수 있게 도와주는 것이 템플릿 리터럴, 꺽쇄키를 이용한 입력 방식이다.

```jsx
greetings = `'안녕 ${id}
즐거운 하루 보내요 `
console.log(greetings);
```

변수를 입력할 때에는 $ 달러와 변수를 부른다는 {} 중괄호를 이용하여 불러오면 된다.

꺽쇄 안에서는 띄어쓰기나 줄바꿈을 희망할 때 백슬레쉬를 이용한 \n을 이용하지 않고 enter를 쳐서 입력하면 동일하게 반영됨을 확인할 수 있다.
</br>
</br>

# 불리언 타입 boolean

참과 거짓 두가지만 담을 수 있다

### 그렇다면 불리언을 어떨 때에 사용할 수 있을까?

일반 변수는 명사를 쓴다.

참과 거짓을 담고 있는 변수는 is를 사용한다.

ex) isFree isOn isActvated isEntrolled 등등

```jsx
let isFree = true;
console.log(isFree); //true
```

- **Falshy** - 거짓인 값

0, 텅 빈 문자열, null, undefined, NaN

```jsx
console.log(!!0); //false
console.log(!!-0); //false
console.log(!!""); //false
console.log(!!null); //false
console.log(!!undefined); //false
console.log(!!NaN); //false
```

→ 모두 false임을 확인할 수 있다.

+) !!은 값을  true / false인지를 변환시켜주는 연산자이다. 

- **Truthy** - 참인 값

number, string, object(텅 비어도 오브젝트는 true), Infinity

```jsx
console.log(!!1); //true
console.log(!!-1); //true
console.log(!!'text'); //true
console.log(!!{}); //true
console.log(!!Infinity); //true
```

→ 모두 true임을 확인할 수 있다.

참고 자료: https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Boolean
</br>
</br>
# null과 undefined 타입

```jsx
let variable; // undefined
console.log(variable); // undefined
```

위와 같이 변수를 선언하였으나 값을 주지 않고 선언하면 undefined가 나온다.

### 그렇다면 여기서 variable은 null이라고 선언하면 어떻게 될까?

```jsx
variable = null;
console.log(variable); //null
```

null이 찍히는 것을 확인할 수 있다.

**null과 undefined에 대해 쉽게 설명해보자면**

예를 들어 꽃이라는 값을 나타내기 위해 변수를 선언하여 꽃이 폈는지 안폈는지에 대한 값을 넣으려고 한다.

값을 설정했을 때에는 이 변수에 꽃이 들어있다는 것을 알 수 있다.

근데 null로 꽃이 없다고 선언을 하면 이 변수는 꽃이 있어야 하는데 null이구나 꽃이 없구나를 알 수 있다.

그러나 undefined의 경우에는 꽃이 있는지 없는지, 꽃을 담고 있는 변수인지 아닌지에 대해 아무것도 정해지지 않은 상태임을 나타낸다.

### 그렇다면 아래 예시를 보자

```jsx
let activeItem;
```

이렇게 activeItem이라고 변수 선언만 하면 아직 활성화된 아이템이 있는지 없는지 모르는 상태가 된다.

특정이 되지 않은 상태.

그런데 나중에 아래와 같이 null로 값을 명시적으로 선언을 해보자

```jsx
activeItem = null;
```

이렇게 되면 활성화된 아이템이 없는 상태가 된다.

누군가가 활성화된 아이템을 할당해주기 전까진 없는 상태가 된다는 것이다.

### 여기서 좀 더 자세히 들어가보자

```jsx
console.log(typeof null); //object
console.log(typeof undefined); //undefined
```

이렇게 null과 undefiend의 타입을 확인해보면 null은 object 타입을 가지고 있고 undefined는 동일하게 타입이 undefined가 나온다.

**null 을 지정하는 순간 메모리에 null이라는 것을 표현하기 위해 object를 만들어내 비어있다는 것을 나타낸다.**

### 즉, null은 확실하게 비어있다는 것을 나타내고 undefined는 비어있는지, 어떤지 확실하게 정해지지 않은 상태임을 나타낸다는 것이다.
</br>
</br>
</br>

# 객체 타입 object

오브젝트란 문자열과 숫자를 담을 수 있는 큰 복합 데이터

여러 가지 데이터를 묶어서 상태 뿐만 아니라 이 상태의 행동 즉 함수를 함께 묶어줄 수 있는 것을 객체라고 한다.

```jsx
{key: value;}
```

→ value에는 원시타입, 객체타입 모두 들어갈 수 있다.

</br>

원시타입은 메모리주소가 가리키는 곳에 값이 들어가있다.

(어디에 변수가 선언되어있냐에 따라서 data / stack에 들어가 있음

오브젝트는 데이터의 크기가 정해져있지 않고 동적으로 크기가 늘어났다 줄었다 하면서 크기가 다양하기 때문에 heap에 들어가있다.

### 예를 들어 apple이라는 object를 만들었다고 하자

```jsx
let apple = {
	name: 'apple',
	color: 'red',
	display: '🍎'
}
```

여기서 {} 안에 있는 key와 value들은 heap 어딘가에 저장이 된다. 메모리 셀 하나 안에 커다란 객체가 들어갈 수 없기 때문에 여러 개의 셀 안에 저장이 된다.

apple 변수는 하나의 메모리 셀 안에 들어있는데 그 메모리셀 안에는 중괄호 안에 있는 key와 value들의 시작하는 메모리 주소를 가지고 있다.

그래서 나중에 key에 접근하려고 할 때

```jsx
apple.name
```

apple이 가지고 있는 메모리 주소를 찾아가서 거기서 값을 찾게 된다.
</br>
</br>
</br>
# 값과 참조의 차이

⭐️매우 중요⭐️

**원시 타입**은 메모리 셀 안에 **값 자체**가 들어가 있다.

**객체 타입**은 실제 객체가 들어있는 **참조 값**, 즉 메모리 주소를 가지고 있다.

아래의 예를 보자

```jsx
let a = 1;
let b = a;
```

라고 선언이 되었을 때, 원시타입은 a가 1이고 b는 a라고 했으므로 a의 값 자체가 복사되어 b의 메모리 셀 안에 1을 가진다.

이것을 **copy by value**라고 한다.
</br>
</br>
**그렇다면 여기서 b를 재할당하면 어떻게 될까?**

```jsx
b = 2;
```

원래 1을 가지고 있던 메모리셀에서 2로 업데이트 되어 b는 2를 가지고 있게 한다.

하지만 a는 원래대로 1을 가지고 있다. 서로 개별적으로 독립되어있기 때문에 재선언한 값이 반영 되지 않고 있다.

### 즉, 원시타입은 값이 복사되어 전달된다.

### 하지만 이 동작이 객체에서는 달라진다

```jsx
let apple = {
    name: 'apple',
}
let orange = apple;
```

오렌지라는 변수를 만들어 apple을 할당한다.

그러면 오렌지라는 변수에는 apple의 참조값, 메모리 주소가 복사되어진다.

예를들면 apple이라는 메모리 주소가 0x1234라고 했을 때 orange는 apple이라고 선언하였으니 orange에도 동일한 메모리 주소 0x1234가 주어진다.

이것을 **copy by reference**라고 한다.

### 그렇다면 apple의 이름을 재할당하면 어떻게 될까?

```jsx
let apple = {
    name: 'apple',
}
let orange = apple;
orange.name = 'orange';
console.log(apple); //orange
console.log(orange); //orange
```

한 곳에서만 바꿔도 둘 다 업데이트가 되어있는 것을 확인할 수 있다.

### 즉, 객체타입은 참조값(메모리주소, 레퍼런스)이 복사되어 전달된다.
</br>
</br>
</br>

# 상수 변수 const

let은 재할당이 가능하지만 const는 재할당이 불가능하다.

아래의 예시를 보자

```jsx
const text = 'hello';
text = 'hi';
```

다음과 같이 값을 재할당을 하면 Assignment to constant variable 상수가 변수가 되었다고 에러가 뜨는 것을 확인할 수 있다.

즉, 한번 값을 할당하면 다른 값으로 재할당이 불가능하다.
</br>
</br>

1. **상수**

```jsx
const MAX_FRUITS = 5;
```

→ 상수로 쓰고 싶을 때에는 대문자와 언더바로 구분하여 이름을 설정해야 한다.
</br>
</br>

2. **재할당이 불가능한 상수변수 또는 변수**

```jsx
const apple = {
    name: 'apple',
	color: 'red',
	display: '🍎',
}

apple = {}; //불가능
```

한번 할당하면 재할당이 필요하지 않다면 let보다는 const를 이용하여 선언하는 것이 좋다.

### 그러나 다음의 예시를 보자

```jsx
const apple = {
  name: 'apple',
	color: 'red',
	display: '🍎',
}

apple.name = 'orange'
console.log(apple); //{ name: 'orange', color: 'red', display: '🍎' }
```

이렇게 apple의 이름을 orange로 변경하였을 때  값이 변경되어 나타나는 것을 확인할 수 있다.
</br>
</br>
</br>
### 왜 그럴까?

객체는 heap이라는 공간에 값이 저장되어있고 apple이라는 object는 값을 저장해놓은 메모리 주소가 저장되어있다.

그러므로 그 메모리 주소로 들어가서 안에 있는 데이터를 변경하기 때문에 값이 변경이 가능해지는 것이다.

즉, apple의 메모리 주소를 변경이 불가능할 뿐, 데이터를 변경하는 것은 가능하다,

→ 변경은 둘 다 가능

→ const는 재할당만 불가능하다.
</br>
</br>
</br>

# 타입 확인 법 typeof

자바스크립트는 dynamic language, 즉 동적인 언어이다.

java나 c++ 는 컴파일러(complier)를 필요로 한다.

개발자가 작성한 코드를 컴파일러를 이용하여 실행파일로 변경해야만 실행이 가능하다.

→ 이것을 정적타입이라고 한다.

자바스크립트는 인터프리터로 동적으로 런타임시 코드를 한줄씩 번역하여 실행한다.

→ 이것을 동적타입이라고 한다.

### typeof 데이터 타입을 확인할 수 있는 연산자

값을 타입을 문자열로 반환해준다

아래의 예시를 보자

```jsx
let variable;
console.log(typeof variable); // undefined

variable ='';
console.log(typeof variable); // string

variable = 123;
console.log(typeof variable); //number
```

할당된 값에 따라 타입이 결정되어 계속 변경됨을 확인할 수 있다.

### 즉, 자바스크립트에도 타입이 있다!

### dynamic, weakly typed, programming language 타입이다!