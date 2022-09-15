# 객체, 복합 데이터

객체는 복합 데이터이다.

객체의 종류에는 object, function이 있다.

key와 value로 이루어진 복합 데이터도 객체라고 한다.

value는 원시 데이터로 이루어질 수 있고, 또다른 객체를 가질 수도 있다.

객체는 heap이라는 메모리 공간에 있고 변수는 그 메모리 주소를 가지고 있다.

객체란 서로 연관있는 속성과 행동을 묶어주기 위해 있다.

여기서 속성은 data(프로퍼티), 행동은 함수(메서드)를 가리킨다.

객체는 순수 데이터만 가지고 있을 수 있지만 객체와 관련된 상태와 행동을 가지고 있을 수 있다.

객체 안에서 상태를 가지고 있는 것을 속성 property, 행동을 함수 method라고 부른다

### 밀접하게 관련 있는 상태과 행동을 객체로 묶어야한다!
<br>
<br>
<br>

## 객체 리터럴

### 객체를 만들 수  있는 방법

- **Object litera**l { key : value} 중괄호를 이용하여 key와 value를 작성하기
- **new Object()** Object 클래스를 이용하여 객체 생성
- **Object.create();** Object 클래스 안에 있는 create() 함수를 이용하여 객체 생성
<br>
<br>

### 객체 구성

- **key** - 문자, 숫자, 문자열, 심볼
- **value** - 원시값, 객체(함수)

```jsx
let apple = {
	name : 'apple',
	'hello-bye': '🖐',
	0: 1,
	['hello-bye1']: '🖐',
}
```

→ 문자, 따옴표 안의 문자, 숫자, 대괄호 안의 문자 모두 key 객체에서 data로 접근할 수 있는 key

실제로 key가 가지고 있는 value는 오른쪽에 있다

### 하지만 대부분은 깔끔하게 문자로 만든다

### 속성 즉, 데이터에 접근하기 위해서는 객채 이름 점 키의 이름

- apple.name;  **마침표 표기법 dot notation**
- apple['hello-bye1']  **대괄호 표기법 bracket notation**

```jsx
console.log(apple['hello-bye1']);
apple['name'];
```

### 속성 추가

```jsx
apple.emoji = '🍎';
console.log(apple.emoji); 
console.log(apple['emoji']);
```

→ 동적으로 나중에 추가 가능

### 속성 삭제

```jsx
delete apple.emoji;
console.log(apple); // { '0': 1, name: 'apple', 'hello-bye': '🖐', 'hello-bye1': '🖐' }
```
<br>
<br>

## 객체 동적으로 접근하기

```jsx
const obj = {
  name: '엘리',
  age: 20,
}
```

→ 코딩하는 시점에 정적으로 접근이 확정됨

```jsx
obj.name;
obj.age;
```

### 동적으로 속성에 접근하고 싶을 때 대괄호 표기법을 사용할 수 있다

```jsx
function getValue(obj, key) {
  // return obj.key 이렇게 접근하면 접근 불가능함
  // obj 안에 key라는 이름을 가진 key가 없기 때문
  return obj[key] //대괄호를 이용하여 전달된 key를 접근할 수 있게 해야 함
}
```

```jsx
console.log(getValue(obj, 'name'));
```

→ 대괄호 표기법은 동적으로 접근하거나 추가 혹은 삭제할 때 많이 사용

```jsx
function addKey(obj, key, value) {
  obj[key] = value;
}
```

```jsx
addKey(obj, 'job', 'enginner'); //{ name: '엘리', age: 20, job: 'enginner' }
console.log(obj);

function deleteKey(obj, key) {
  delete obj[key];
}

console.log(obj);
```

참고 자료: https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Working_with_Objects
<br>
<br>
<br>

### 객체 축약 버전

key 이름과 value 값이 동일할 경우에는 간략한 버전으로 축약해서 사용이 가능하다

```jsx
const x = 0;
const y = 0;

const coordinate1 = {x: x, y: y};
const coordinate2 = {x, y};
console.log(coordinate1); // { x: 0, y: 0 }
console.log(coordinate2); // { x: 0, y: 0 }
```

→ 모두 동일하게 값이 출력된 것을 확인할 수 있다.

```jsx
function makeObj(name, age) {
  return {
    name: name,
    age: age,
  }
}
function makeObj(name, age) {
  return {
    name,
    age:,
  }
}
```

→ 위와 같이 축약하여 사용가능하다
<br>
<br>
## 객체 안의 함수

```jsx
const apple = {
  name: 'apple',
  display: function() {
    console.log(`${this.name}: 🍎`); // 내 자신의 이름
  },
};

apple.display(); //apple: 🍎
```

→ 여기서 key - display, value - function이다

→ 함수 내부에서 key를 부를 때에는 this.key를 이용하여 호출할 수 있다

### 이처럼 객체는 서로 연관된 정보와 함수들을 묶어서 관리할 수 있다!
<br>
<br>

## 생성자 함수

```jsx
const apple = {
  name: 'apple',
  display: function() {
    console.log(`${this.name}: 🍎`); // 내 자신의 이름
  },
};
```

→ 위와 같은 형식으로 orange 객체도 만들고 싶다면 어떻게 해야 할까?

```jsx
const orange = {
  name: 'orange',
  // key - display, value - function
  display: function() {
    console.log(`${this.name}: 🍊`); // 내 자신의 이름
  },
};

apple.display(); //apple: 🍎
orange.display(); //orange: 🍊
```

→  두 가지의 객체가 출력된 것을 확인할 수 있다

그러나 동일한 것을 여러번 써야 하고 오렌지 외에도 더 많은 과일들을 추가헤야 한다면 일이 계속 반복된다

특정한 템플릿에 맞게 객체를 쉽게 만들어줄 수 있는 생성자 함수를 이용하면 정해진 틀 안에서 원하는 객체를 만들 수 있다.

```jsx
function Fruit(name, emoji) {
  this.name = name;
  this.emoji = emoji;
  this.display = () => {
    console.log(`${this.name}: ${this.emoji}`);
  };
  return this; // 생략 가능
}

const apple = new Fruit('apple', '🍎');
const orange = new Fruit('orange', '🍊');

console.log(apple); //Fruit { name: 'apple', emoji: '🍎', display: [Function (anonymous)] }
console.log(orange); //Fruit { name: 'orange', emoji: '🍊', display: [Function (anonymous)] }

console.log(apple.name); //apple
console.log(apple.emoji); //🍎
apple.display(); //apple: 🍎
```

⭐️ 생성자 함수의 앞글자는 **대문자**

→ this라는 keyword를 이용하면 객체 자기 자신을 가리킨다

⭐️ return this; 

→ 생략 가능 생략해도 생성자 함수에서는 자동으로 this를 반환하도록 자바스크립트 엔진에서 만들어둠

### 이렇게 생성자 함수를 통해 만든 객체는 일반 리터럴 객체로 만든 객체처럼 동일하게 출력 및 접근할 수 있다!
<br>
<br>

### 정리

객체를 만들 때 공통적인 구조를 가진 객체가 있다면 생성자 함수를 이용하여 손쉽게 만들 수 있었다

만들고자 하는 객체의 양식을 생성자 함수를 통하여 정의해두고 함수를 호출하듯이 필요한 데이터만 인자로 전달하면 객체를 손쉽게 만들 수 있다

이때 **생성자 함수**는 붕어빵 기계체럼 객체를 만들어 낼 수 있는 **템플릿**

**객체**는 붕어빵 기계에 데이터를 넣어서 만든 **붕어빵**

자바스크립트에서는 생성자 함수라는 기술적으로 객체를 손쉽게 만들어낸다

이렇게 만들어낼 수 있는 것이 **프로토타입 Prototype을 베이스로 한 객체지향 프로그래밍 Object-Oriented Programming**을 지원하기 때문이다.