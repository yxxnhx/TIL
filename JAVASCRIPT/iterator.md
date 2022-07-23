# ITERATOR

## 이터레이션 Iteration

무언가를 반복하다, 순회하다

자바스크립트에서는 이터레이션 프로토콜(Iteration Protocol)이라고 부른다

여기서 프로토콜이란 규격, 약속, 인터페이스를 뜻한다

즉, 순회가 가능한 규격, 약속, 인터페이스라는 뜻이다.

이렇게 순회가 가능한 객체는 for of, spread를 사용할 수 있다.

for of와 spread는 순회가 가능하다

다시 정리해보자면, 이터레이션 프로토콜에 따르면 for of, spread를 사용할 수 있는데 이터레이션 프로토콜을 따르는 기본 자료 구조는 array 배열, string 문자열, map, set이 있다.

<br />

**그렇다면 이 규격을 따른다, 준수한다는 것은 무슨 의미일까?**

어떤 객체든지 순회가 가능하기 위해서는 iterable 프로토콜을 따라야 한다.

순회하고 싶은, 순회가 가능한 객체가 되기 위해서는 다음을 따라야 한다.

```jsx
{
	[Symbol.iterator]() : Iterator 프로토콜
	-> 심볼 이터러터라는 함수를 호출했을 때 이터러털 프로토콜이라는 객체를 반환
												{
													next(): 다음값
												}
}
```

→ 만약 {1, 2, 3}을 가지고 있는 object가 있다면 심볼 이터러터를 호출했을 때 next가 호출되어 1, 2, 3이 반환된다

<br />

```jsx
const array = [1, 2, 3];
for (const item of array) {
  console.log(item);
} // 1, 2, 3
```

<br />

### Array의 정의를 한번 알아보자

![Untitled](https://user-images.githubusercontent.com/50559373/192129656-f1c758a2-9d7c-4103-baab-37950a9dbe23.png)

→ 이터레이터의 규격을 따르는 것을 확인할 수 있다.

심볼 이터레이터를 호출하면 이터러블한 이터레이터를 반환한다.

즉, array는 이터레이터를 따라간다는 것을 확인할 수 있다.

<br />

### 그렇다면 오브젝트의 경우는 어떨까?

오브젝트는 기본적으로 이터레이터 규격상을 따르지 않는다.

```jsx
const obj = { 0: 1, 1: 2 };
for (const item of obj) {
  console.log(item);
}
```

<img width="560" alt="스크린샷 2022-09-25 오후 1 02 00" src="https://user-images.githubusercontent.com/50559373/192129606-a5d874ce-d0f2-4351-ada4-a14fa95da009.png">

→ 이와 같이 오류가 나는 것을 확인할 수 있다.

**obj is not iterable**

왜냐하면 오브젝트 안에는 심볼 이터레이터라는 함수의 정의가 없기 때문!

array처럼 객체 안에 이터레이션 프로토콜을 따라가는 심볼 이터레이터가 있거나, array 안에 다양한 함수들이 있는데 그중 values()라는 것이 있다.

```jsx
array.values();
```
<img width="460" alt="스크린샷 2022-09-25 오후 1 05 16" src="https://user-images.githubusercontent.com/50559373/192129610-ffb075f8-939b-40de-849f-640052f99d81.png">

→ values를 호출하면 iterableIterator가 반환되는 것을 확인할 수 있다.

<br />

### values()

```jsx
const array = [1, 2, 3];
for (const item of array.values()) {
  console.log(item);
} // 1, 2, 3
```

→ 정상적으로 값이 반환되는 것을 확인할 수 있다.

<br />

### keys()
<img width="491" alt="스크린샷 2022-09-25 오후 1 09 50" src="https://user-images.githubusercontent.com/50559373/192129686-cb4d3d02-56c6-4d15-b89e-c51d85297915.png">

```jsx
const array = [1, 2, 3];
for (const item of array.keys()) {
  console.log(item);
} // 0, 1, 2
```

→ array의 key값들이 반환되는 것을 확인할 수 있다.

<br />

### entries()
<img width="485" alt="스크린샷 2022-09-25 오후 1 09 27" src="https://user-images.githubusercontent.com/50559373/192129691-8b2cb86e-1415-4572-a13b-ade74b7b99e8.png">


```jsx
const array = [1, 2, 3];
for (const item of array.entries()) {
  console.log(item);
} // [0, 1] [1. 2] [2, 3]
```

→ 0번째 인덱스에는 1, 1번째 인덱스에는 2, 2번째 인덱스에는 3이 출력되는 것을 확인할 수 있다.

정리해보자면

### Iterable 하다는 건 순회가 가능하다는 것!

순회가 가능하기 위해서는 이터러블 프로토콜에 따르면 된다.
객체 안에서 Symbol.iterator를 호출했을 때 next, next 할 수 있는 iterator를 가지고 있으면 된다
심볼 정의를 가진 객체나 또틑 특정한 함수가 Iterator를 리턴한다는 것은 순회 가능한 객체이다라는 것을 알 수 있다.

**순회가 가능하면 무엇을 할 수 있을까?**

바로 빙글빙글 도는 연산자를 사용할 수 있다는 뜻이다!
**for of, spread와 같은!**

```jsx
const obj = { 0: 1, 1: 2 };
for (const item of obj) {
  console.log(item);
}
```

이와 같이 이터러블 프로토콜에 따르지 않는 일반 객체라면, ~~for of~~ 말고 **for in**으로 사용이 가능하다!

<br />

```jsx
const obj = { 0: 1, 1: 2 };
for (const item in obj) {
  console.log(item);
} // 0, 1
```

→ key가 출력되는 것을 확인 할 수 있다

즉, **for in은 오브젝트 안에 있는 key를 출력하는 연산자이다!**

<br />
<br />


## 조금 더 세밀하게 iterator 파헤쳐 보자!
<img width="463" alt="스크린샷 2022-09-25 오후 1 25 32" src="https://user-images.githubusercontent.com/50559373/192129700-e43b4411-b5c8-481f-b4b5-00249f809ffc.png">


iterator를 호출하면 반복자를 통해서 순회가 가능하다.

iterator의 규칙을 확인해보면 next, return, throw가 있는 것을 확인할 수 있다.

여기서 return과 throw는 ?가 달려 있는 것을 볼 수 있다.

이것은 옵션 사항이라는 것이다.

즉, iterator는 next를 꼭 가지고 있어야만 하지만 return과 throw는 옵션이다.

<br />
<br />

**고로 위와 같이 for of를 이용하여 호출도 가능하지만 수동적으로 호출하는 방법도 있다!**

```jsx
const array = [1, 2, 3];
const iterator = array.values();
for (const item of iterator) {
  console.log(item);
}

console.log(iterator.next()); // { value: 1, done: false }
```

→ value에는 첫번째 값을, done에는 이 반복, 순회가 끝났는지 여부를 나타내준다.

이 array에서는 1, 2, 3이 있기 때문에 뒤에 더 순회할 값이 남아 있어서 false가 출력된 것을 확인할 수 있다.

<img width="496" alt="스크린샷 2022-09-25 오후 1 26 18" src="https://user-images.githubusercontent.com/50559373/192129702-7bd5273a-346a-4853-8b5b-5ec3d06b75cd.png">


→ result에는 done과 value가 있다!

<br />


```jsx
console.log(iterator.next().value); // 1
```

→ 값만 확인하고 싶다면 next().value를 이용하여 확인할 수 있다.

<br />

```jsx
console.log(iterator.next().value); //1
console.log(iterator.next().value); //2
console.log(iterator.next().value); //3
```

→ 이와 같이 array를 순회하면서 순차적으로 값을 호출하는 것을 확인 할 수 있다.

<br />


**그렇다면 한번 더 호출하면 어떤 값이 나올까?**

```jsx
console.log(iterator.next().value); //undefined
```

→ 더이상 호출할 값이 없기 때문에 undefined가 나오는 것을 확인할 수 있다.

<br />


**그렇다면 done을 호출하면 어떻게 될까?**

```jsx
console.log(iterator.next().done); //true
```

→ 반복, 순회가 끝났기 때문에 true가 출력되는 것을 확인할 수 있다.

```jsx
const iterator = array.values();

console.log(iterator.next().value);
console.log(iterator.next().value);
console.log(iterator.next().value);
console.log(iterator.next().value);
console.log(iterator.next().done);
```
<br />

### 위를 while문을 이용하여 좀 더 깔끔하게 정리를 해보자

```jsx
while (true) {
  const item = iterator.next();
  if (item.done) break;
  console.log(item.value);
}
```

→ item을 iterator의 next라고 선언 후에 item이 순회를 다 끝나서 done이 true일 때에는 while문이 종료가 되고 done이 true가 아닐 경우에는 item의 value를 콘솔에 반환해줘

[이터러블 퀴즈](https://www.notion.so/b1b214f6c48541c39ef3af2c20ca5dd0)

<br />
<br />

### 이것을 조금 더 심플하게 만들어보자!

## Generator

```jsx
function* multipleGenerator() {
  for (let i = 0; i < 10; i++) {
    yield i ** 2;
  }
}

const multiple = multipleGenerator();
let next = multiple.next();
console.log(next.value, next.done); // 0 false
```

→ 위와 같이 function 뒤에 바로 \*을 붙이게 되면 generator를 사용하겠다고 선언을 한 것이다!

여기에서 특이한 것은 **yield**이다!

yield는 사용자가 다음을 호출할 때까지 기다려서 호출을 해야만 다음 값을 반환하는 것이다.

그래서 next의 value와 done을 확인해보았지만 콘솔에 0 false이 뜨는 것이다.

<br />

**그렇다면 한변 for문 다음에 콘솔에 찍어보자**

```jsx
function* multipleGenerator() {
  for (let i = 0; i < 10; i++) {
    console.log(i); // 0
    yield i ** 2;
  }
}
```

→ 0이 나오는 것을 확인할 수 있다.

원래대로라면 0부터 9까지 for문을 돌면서 값이 나와야 하지만 yield 때문에 0만 호출된 것을 확인할 수 있다. **즉, 사용자에게 제어권을 준다고도 할 수 있다!**

```jsx
next = multiple.next();
console.log(next.value, next.done); //1 false
```

→ 다시 한번 더 next를 호출해야만 그 다음 값을 호출하는 것을 확인할 수 있다.

<br />

**그렇다면 중간에 multiple을 리턴해보면 어떤 결과가 나올까?**

```jsx
multiple.return(); //undefined true
```

→ 뒤에서 아무리 next를 불러도 generator가 종료되어 값은 undefined done은 true가 출력되는 것을 확인할 수 있다.

<br />

**그렇다면 multiple에 throw로 Error를 넣어보면 어떤 결과가 나올까?**

```jsx
multiple.throw("Error!");
```

<img width="551" alt="스크린샷 2022-09-25 오후 2 16 27" src="https://user-images.githubusercontent.com/50559373/192129706-f2448370-b837-4d43-aa30-c73b95a0e112.png">


→ 이와 같이 yield에 에러가 있다고 창이 뜬다!

<br />

**이러한 error를 잡고 싶다면 try catch문을 이용하면 잡을 수 있다!**

```jsx
function* multipleGenerator() {
  try {
    for (let i = 0; i < 10; i++) {
      console.log(i);
      yield i ** 2;
    }
  } catch (error) {
    console.log(error);
  }
} // 0 0 false Error!
```
