# 함수

함수는 쉽게 말하면 일의 단위이다.

또한 자주쓰는 내용들을 묶어서 함수로 만들어두면 필요할때 언제든 불러다 쓸 수 있습니다.

예를 들면 햄버거를 만들 때 빵을 놓고 상추를 놓고 고기를 놓고 다시 빵으로 덮는 행위를 해야 하는데 계속해서 동일한 일을 해야 하는 경우, 프로세스 없이 그냥 계속해서 동작을 반복하다보면 실수가 일어날 수 있다. 그 실수를 방지하고 일을 좀 더 쉽게 하기 위해 만들어낸 것이 함수이다.

```jsx
function makeBurger(type, size, num) {
  console.log("매개변수 값은?", type);
  console.log("빵두기");
  console.log("상추두기");
  if (num < 1) {
    return "버거 개수가 0개 입니다.";
  }
  if (type == "불고기") {
    console.log("고기패티두기");
  } else if (type == "새우") {
    console.log("새우패티두기");
  }

  console.log("뚜껑덥기");
  console.log(type, "버거", size, "사이즈", num, "개 준비완료");

  return "완료되었습니다";
}

function serveCoke() {
  console.log("콜라통 선택");
  console.log("얼음담기");
  console.log("콜라담기");
}

function serveFrenchFries() {
  console.log("감튀박스선택");
  console.log("감튀 담기");
}

let result = makeBurger("불고기", "M", "0");
console.log("버거 프로세스 결과", result);
serveCoke();
serveFrenchFries();

function makeSet() {
  makeBurger("새우", "M", 2);
  serveCoke();
  serveFrenchFries();
}

makeSet();
```

### **함수 생김새**

```jsx
function 함수이름(매개변수) {
  내용입력;
  return 반환;
}
```

- **매개변수**: 함수에 전달해야되는 내용이 있을때, 이 함수가 실행될때 알아야되는 내용이 있을 때 매개변수를 통해 전달한다
- **return**: 반환값, 함수 완료 후, 반환되야하는 값이 있을 때 사용한다

### 예시

```jsx
function greet(firstName, lastName) {
  console.log("hello", firstName, lastName, "Welcome to our website");
}
greet("Noona", "Kim");
```

**함수는 반드시 불러야 실행된다**

## **함수의 또다른 이름들**

- method: 메서드, 함수와 같은 뜻이다
- 익명함수 Anonymous function : 이름이 없는 함수. **function (y) {console.log(y)}**. 함수를 변수에 넣어줄때, 일시적으로 쓰이고 말 함수들에 대해선 익명함수로 만들어준다
- 람다식 함수: => 를 사용하여 함수를 정의하는 경우이다.(이건 곧 배울 예정이다.)

```jsx
let arrowFunc = (y) => {
  console.log(y);
};
```

- **콜백함수** : 다른 함수의 매개변수로 전달된 함수.

### 예시

```jsx
button.addEventListenr("click", setCount);
// 버튼에 클릭 이벤트가 발생했을때 setCount함수를 콜을 한다
function setCount() {
  count++;
}
```

콜백은 말그대로 부른다는 뜻이다.

주로 어떤 함수에 매개변수로 들어가 어떤 특정한 조건이 되었을때만 호출이 된다

(클릭이벤트나 타이머이벤트 등등)

[함수 TEST](https://www.notion.so/TEST-daac80d5099e4a2292c59e4789e098cf)

## 함수란?

특정한 일을 수행하는 코드의 집합

한군데에 잘 묶어두면 재사용 가능, 높은 가독성 그리고 유지 보수도 가능하다.

코드를 작성할 때 똑같은 일들을 계속 반복해서 쓰는 것이 아니라 반복되는 것을 함수로 만들어 필요할 때마다 만들어진 함수를 재사용하면 된다.

![Untitled](https://upload.wikimedia.org/wikipedia/commons/thumb/3/3b/Function_machine2.svg/1200px-Function_machine2.svg.png)

함수는 특정한 박스 안에서 주어진 일들을 수행한다.

어떤 일들을 수행하는지는 외부에서는 전혀 알 수 없다.

사용자에게 필요한 데이터를 입력 받아 주어진 일들을 수행하여 출력 return하게 된다.
<br>
<br>

### 함수의 형태

```jsx
function add(a, b) {
  return a + b;
}
```

function이라는 함수 정의 키워드를 쓴 후 우리가 원하는 함수 이름을 작성 후에 외부에서 전달 받을 수 있는 매개변수, 인자를 외부로부터 받아와서 특정한 일을 수행한 후 return이라는 키워드를 통해서 우리가 처리한 일을 결과값을 반환한다.

→ 이것을 함수를 정의한다고 한다.

```jsx
add(1, 2);
```

이렇게 정의한 함수를 외부에서 함수 이름을 통해서 필요한 데이터를 전달하면서 호출할 수 있다.

add에 1과 2를 콤마를 통해서 매개변수 또는 인자로 전달이 되어 주어진 일을 수행한 후 return하게 된다.

### 즉, 프로그램 상에서 중복, 반복되는 일이 있다면 함수 단위로 작은 단위의 일들을 묶는 것이 중요하다.

### 수행하는 일을 잘 나타낼 수 있는 이름으로 짓는 것 또한 중요하다.

### 전달 받은 매개변수, 인자의 이름도 의미 있게 지어야 한다!

함수도 결국 객체이기 때문에 heap이라는 메모리에 저장이되고, 메모리 셀 하나가 아닌 여러 개의 메모리셀에 저장이 된다.

**정보가 가득 차있으니까!**

### 결국, 위의 경우에는 add, 함수의 이름은 함수라는 객체가 가지고 있는 데이터의 주소를 가지고 있다.

→ 또다른 변수에 함수의 이름을 할당하게 되면 함수의 이름은 함수를 참조하고 있다.

**함수의 객체 주소를 가지고 있다!**

### 사용예제 1

```jsx
function sum(a, b) {
  console.log("function");
  return a + b;
}

const result = sum(1, 2);
console.log(result);
```

### 사용예제2

성과 이름을 각각 따로 받아 하나의 풀네임을 만들고 싶다.

```jsx
let lastName = "김";
let firstName = "지수";
let fullName = `${lastName} ${firstName}`;
console.log(fullName);
```

이렇게 한 명일 때에는 하나씩 받아서 사용이 가능하겠지만 user가 더 많아지면 일일히 하나하나 쓸 수 없으니 이것을 함수로 받아 사용할 수 있다.

```jsx
function makeFullName(lastName, firstName) {
  return `${lastName} ${firstName}`;
}

const user1 = makeFullName("김", "빡빡");
const user2 = makeFullName("김", "인성");
console.log(user1);
console.log(user2);
```

## 함수와 메모리

```jsx
function add(a, b) {
  return a + b;
}

const sum = add;
console.log(sum(1, 2)); //3
console.log(add(1, 2)); //3
```

→ 이렇게 sum에 add라는 함수를 할당하게 되면 메모리 heap 어딘가에 있는 add의 메모리 주소가 sum에게 할당이 된다

## 반환이란?

```jsx
function add(a, b) {
  return a + b;
}

const result = add(1, 2);
console.log(result); //3
```

### 여기서 return 문을 없애면 어떻게 될까?

```jsx
function add(a, b) {
  //return a + b;
  return undefined;
}

const result = add(1, 2);
console.log(result); //undefined
```

→ undefined가 나오게 된다.

즉, 함수에서 return을 명시적으로 작성하지 않으면 코드에서 자동으로 undefined 값을 반환해낸다.

### 함수에서 값을 반환해야 할 것이 아니라면 굳이 return을 작성하지 않아도 된다.

다음과 같은 예를 보자.

```jsx
function print(text) {
  console.log(text);
}

print("text");
```

그냥 단순히 무언가를 프린트만 해도 되는 함수라면 return을 생략할 수 있다.

### return을 함수 중간에 하게 되면 함수가 종료된다.

사용 예: 조건이 맞지 않는 경우 함수 도입 부분에서 함수를 일찍이 종료한다.

예를 들어서 숫자를 출력해내는 함수를 만들건데, 숫자가 0 이상일 때만 출력해내고 싶다면 이 return을 이용하면 효율적으로 만들어낼 수 있다.

```jsx
function print(num) {
  if (num < 0) {
    return;
  }
  console.log(num);
}

print(12);
print(-12);
```

→ 만약 num이 0보다 작다면 함수를 종료하고, 0보다 클 경우에만 num을 출력해내라

함수 안에서 함수를 수행하는 데에 필요한 특정한 조건이 있다면 무거운 함수를 수행하기 이전에 이 함수를 수행하기에 유효한지 아닌지 먼저 검사를 해준 다음에 함수를 수행하게 한다

여기서 return은 return undefined를 줄인 말이다

## 함수의 인자 parameters

매개변수의 기본값은 무조건 undefined이다.

```jsx
function add(a, b) {
  console.log(a); //1
  console.log(b); //2
  return a + b;
}

add(1, 2, 3);
```

만약 위와 같이 추가로 인자가 할당이 되어도 무시됨을 확인할 수 있다.

### 함수는 객체이기 때문에 arguments를 가진다

```jsx
function add(a, b) {
  console.log(a);
  console.log(b);
  console.log(arguments); // [Arguments] { '0': 1, '1': 2, '2': 3 }
  return a + b;
}

add(1, 2, 3);
```

→ 인덱스가 0부터 시작하여 첫 번째 들어온 인자 0번째 인자는 1, 1번째 인자는 2, 2번째 인자는 3이 들어온 것을 확인할 수 있다.

이 값들이 1은 a, 2는 b로 매핑이 되어있는 것을 볼 수 있다.

### 이 arguments를 응용하여 다음과 같이 출력해볼 수도 있다.

```jsx
console.log(arguments[0]); //1
console.log(arguments[1]); //2
console.log(arguments[2]); //3
```

→ 배열처럼 접근이 가능하다.

### 즉, 매개변수의 정보는 함수 내부에서 접근이 가능한 arguments 객체에 저장이 된다.

### 매개변수 기본값 default parameters 지정 가능하다

a = 1, b = 2 와 같은 형태로 지정을 할 수 있다

```jsx
function add(a = 1, b = 1) {
  console.log(a);
  console.log(b);
  console.log(arguments);
  return a + b;
}

add(2, 3);
```

→ 이와 같이 기본값을 지정하였을 경우에는 자동으로 각각 기본값이 전달이 된다.

그러나 누군가가 값을 다시 전달한다면 기본값은 무시되고 그 값으로 업데이트 된다.

다만, undefined일 때만 반영이 된다는 점을 확인해야 한다.

### Rest 매개변수 Rest Parameters

```jsx
function sum(...numbers) {
  console.log(numbers);
}

sum(1, 2, 3, 4, 5, 6, 7, 8);
```

→ 이렇게 몇 개의 인자를 받아올지 모를 때 … rest parameters를 이용하면 배열로 받아올 수 있다.

```jsx
function sum(a, b, ...numbers) {
  console.log(a); //1
  console.log(b); //2
  console.log(numbers); //3, 4, 5, 6, 7, 8
}

sum(1, 2, 3, 4, 5, 6, 7, 8);
```

→ rest parameters 앞에 인자를 붙이면 a, b에는 0번째, 1번째 값이 할당이 되고 그 외의 값들이 numbers에 할당이 되는 것을 확인할 수 있다.

## 함수 표현식

- **함수 선언문**

```jsx
function name() {}
```

- **함수 표현식**

```jsx
const name = function () {};
```

→ 이게 왜 가능할까? 함수도 객체이기 때문에 다른 변수에 값으로 할당이 가능하기 때문!

```jsx
let add = function (a, b) {
  return a + b;
};

console.log(add(1, 2));
```

→ function의 주소가 add에 할당이 된다.

→ 위와 같이 함수의 이름이 없는 것을 **무명 함수**라고 한다

위와 같은 경우에 함수의 이름을 써도 상관은 없지만 그 이름으로 된 함수가 만들어지고 나서 바로 add에 할당되기 때문에 따로 그 이름으로 된 함수는 존재하지 않는다.

```jsx
let add = function sum(a, b) {
  return a + b;
};

console.log(sum(1, 2)); //sum is not defined
```

→ sum은 정의되지 않았다는 오류창이 뜨는 것을 확인할 수 있다.

### 화살표 함수

```jsx
const name = (인자) => {실행하고자 하는 코드}
```

```jsx
add = (a, b) => {
  return a + b;
};

console.log(add(1, 2));
```

→ 위의 코드를 화살표 함수로 변환하면 이와 같다

### 만약, 함수 내부에서 따로 어떤 일을 하지 않고 바로 값을 반환하는 경우에는 괄호+return 생략 가능

```jsx
add = (a, b) => a + b;

console.log(add(1, 2)); //3
```

→ 동일하게 3이 출력되는 것을 확인할 수 있다

### 생성자 함수

```jsx
const object = new Function();
```

→ 객체 편에서 자세하게 다룸

### 함수 자체는 선언문으로 할 때에는 문이지만, 함수는 값으로 계산될 수 있는 표현식이다.

### IIFE (Immediately-Invoked Function Expressions)

**즉각호출 함수**

```jsx
function run() {
  console.log("d");
}
```

→ 위와 같이 함수를 선언만 하고 출력을 하지 않으면 값이 출력이 되지 않는다

```jsx
(function run() {
  console.log("d");
})();
```

→ 하지만 이와 같이 괄호로 묶으면 함수가 바로 실행이 된다

함수가 값으로 변한다 즉, run이라는 함수의 값으로 변하여 호출하게 된다.

→ 잘 사용하지는 않은데 화면이 로딩되면서 즉각적으로 호출해야 할 때 사용하기도 한다.

## 콜백함수

### 일급객체 first-calss object / 일급 함수 first-calss function

일반 객체처럼 모든 연산이 가능한 것을 일급 객체라고 부른다.

- 함수의 매개변수로 전달
- 함수의 반환값
- 할당 명령문
- 동일 비교 대상

→ 함수로 이런 모든 연산이 가능하다

### 고차 함수 Higher-order function

- 인자를 함수로 받거나(콜백함수)
- 함수를 반환하는 함수를 고차함수라고 한다

```jsx
const add = (a, b) => a + b;
const multiply = (a, b) => a * b;

function calculator(a, b, action) {
→ calculator이라는 함수는 인자로 a, b를 받고 action이라는 함수를 전달 받는다
  이렇게 외부로부터 함수를 전달받는 것이 callback 함수라고 한다

  let result = action(a, b);
→ 그래서 결과값은 내가 전달받은 action이라는 함수를 a와 b를 인자로 전달하게 할 것이다
  console.log(result);
→ result를 출력하자
  return result;
→ result를 반환하면서 함수를 종료한다
}

calculator(1, 2, add); //3
→ 숫자 1, 2를 전달하고 호출할 callback함수인 add를 전달한다
  add 함수를 호출한 것이 아닌 add를 전달하였으니 add의 메모리 주소를 전달한다

calculator(1, 2, multiply); //2
→ 숫자 1, 2를 전달하고 호출할 callback함수인 multiply를 전달한다
  multiply 함수를 호출한 것이 아닌 multiply를 전달하였으니 multiply의 메모리 주소를 전달한다
```

→ 이 함수는 직접적으로 계산하는 것은 아니고 더하고 싶은지, 곱하고 싶은지 특정한 action을 할 함수를 전달받을 것이다

### 위 고차함수 콜백함수를 정리하자면

- **전달된 action은 콜백함수이다.**
- **전달될 당시에 함수를 바로 호출해서 반환된 값을 전달하는 것이 아니라**
- **함수를 가리키고 있는 함수의 레퍼런스(참조값)가 전달된다.**
- **그래서 함수는 고차함수(calculator) 안에서 필요한 순간에 호출이 나중에 된다.**

```jsx
function calculator(a, b, action) {
  if (a < 0 || b < 0) {
    return;
  }
  let result = action(a, b);
  console.log(result);
  return result;
}

calculator(-1, -2, add);
calculator(-1, -2, multiply);
```

→ 이와 같이 a와 b가 0보다 작을 경우에는 함수가 종료된다는 조건을 추가할 경우에는 action 콜백함수가 실행되지 않는다.

[함수 퀴즈](https://www.notion.so/a7a3f2fde22f4701ad1e94f1107f3ac8)

## 불변성 Immutability

동의어로는 unchangeable

상태가 변경되지 않도록 불변성을 유지하도록 코딩하는 것이 좋다

mutable - 변경할 수 있는(changeable)

immutable - 변경할 수 없는

```jsx
function display(num) {
  console.log(num);
}

const value = 4;
display(value); //4
```

### 함수 내부에서 외부로부터 주어진 인자의 값을 변경하는 것은 👎

상태변경이 필요한 경우에는, 새로운 상태(오브젝트, 값)을 만들어서 반환해야 한다

왜냐하면 원시값은 값에 의한 복사가 이루어지기 때문에 큰 문제는 없지만

객체값일 경우 참조에 의한 복사(메모리 주소가 전달)가 이루어지기 때문에 함수 내부에서 인자로 받은 값이 변경이 되면 큰 문제가 일어난다

```jsx
function display(num) {
  num = 5; // ❌정말 좋지 않은 예시임❌
  console.log(num);
}

const value = 4;
display(value);
console.log(value);
```

→ 원시값은 복사가 될 때 값이 그대로 박사가 되기 때문에 display 함수에 값이 그대로 전달이 되었으므로 value를 console에 찍어보면 4가 나오는 것을 확인할 수 있음

→ display에게 4가 할당되었으므로 num에 4가 전달이 되었지만 내부에서 num은 5라고 선언하였으므로 해당 내부에서의 console은 5가 출력되는 것을 확인할 수 있다.

### 다음 나쁜 예를 보면서 조금 더 정확하게 확인을 해보자

```jsx
function displayObj(obj) {
  obj.name = "bob"; // ❌❌❌❌❌ 외부로부터 주어진 인자(오브젝트)를 내부에서 변경하지 말아야 함
  console.log(obj);
}

const ellie = { name: "Ellie" };
displayObj(ellie);
console.log(ellie);
```

→ elie라는 오브젝트를 함수 외부에서 만들어냄 함수에 전달할 때는 오브젝트가 가리키고 있는 메모리 주소, 참조값이 전달이 된다.

obj.name으로 새롭게 값을 변경하는 순간 기존 elie의 주소에 이름이 변경이 일어난다

즉, 여기서 elie라는 오브젝트의 이름과 displayObj 함수의 인자 obj는 동일한 주소를 가리키고 있으므로 함수 내부에서 오브젝트를 변경하게 되면 이것을 가리키는 모든 오브젝트가 변경이 된다

### 그렇다면 외부로부터 받은 인자를 변경해야 하는 일이 생긴다면 어떻게 해야 할까?

우선 함수 이름부터 변경한다는 의미를 가진 이름으로 선언을 해야 한다

```jsx
function changeName(obj) {
  //이름부터 변경한다는 느낌을 주도록!
  return { ...obj, name: "bob" }; // 반환할 때는 새로운 오브젝트를 만들기
}
```

→ 리터럴을 이용하여 새로운 오브젝트를 만들기
