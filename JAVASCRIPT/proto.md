# 프로토타입

자바스크립트를 구성하는 정말 중요한 구성요소이다.

그렇다면 Prototype이라는 단어에 대해서 알아보자

자바스크립트 뿐만 아니라 개발 혹은 협업을 할 때, 프로토타입에 대해 말한다.

예를 들면, 이 기능에 대해 빠르게 프로토타입을 만들어봐라, 디자인팀에서 만든 프로토타입을 검토해달라 등 다양한 곳에서 사용이 된다.

**그렇다면 프로토타입의 뜻은 어떻게 될까?**

### Prototype : 원형

- 어떤 것을 복사해서 비슷한 것을 만들거나 개발하기 전 오리지널 버전
- 어떤 물체나 물건을 이야기할 때 그들이 가지고 있는 공통적인 특징에 대해 나타낸다
- 개발하고 있는 첫 번째의 이른 초기 단계의 예제

즉, 프로토타입이란, 완성된 단계의 것이 아니라 그 전에 빠르게 스케치, 대략적인 형태를 나타내는 것이라고 할 수 있다.

개발팀에서는 무언가를 만들기 전에 대략적으로 이런 식으로 보일 거야라고 틀을 잡을 때 디자인에서 프로토타입을 사용한다.

개발 단계에서 배포할 것은 아닌데 배포 단계 전에 대략적으로 빠르게 만들어낸 어플리케이션을 프로토타입 어플리케이션이라고 말한다.

자바스크립트에서 말하는 프로토타입은 다양한 객체들 간에 비슷한 특징을 클래스를 만든 것처럼, 생성자 함수를 통해 템플릿으로 만든 것처럼, 객체지향 프로그래밍을 위하여 이 프로토타입을 만들고 있다.

즉, 자바스크립트란, 이 프로토타입을 베이스로 만들어진 객체지향 프로그래밍 언어이다.

객체지향 프로그래밍을 위해서 클래스를 사용하는데 자바스크립트에서는 프로토타입을 사용한다.

최신에는 클래스를 사용하는 것도 추가가 되었다.

자바, 타입스크립트, 최신 자바스크립트에서는 클래스를 사용한다.

최신 자바스크립트에서 클래스를 사용하고 있는 것도 사실 프로토타입을 감싸고 있는 문법적 슈가일 뿐이다.

### **그렇다면 한번 프로토타입에 대해서 알아보자**

```jsx
const dog = {
  name: '와우',
  emoji: '🐶',
};

console.log(Object.keys(dog)); //[ 'name', 'emoji' ]
console.log(Object.values(dog)); //[ '와우', '🐶' ]
console.log(Object.entries(dog)); //[ [ 'name', '와우' ], [ 'emoji', '🐶' ] ]
```

- Object.keys : 인자로 있는 모든 키를 배열로 반환
- Object.values : 인자로 있는 모든 값을 배열로 반환
- Object.entries : 인자로 있는 모든 것을 배열로 반환

오브젝트에는 조금 더 유용하게 사용할 수 있는 것이 있다.

**특정한 키의 여부를 확인할 수 있는 방법이다!**

```jsx
console.log('name' in dog); // true
```

→ dog라는 객체에 name이 있니?

이와 동일한 프로퍼티 매서드가 있다.

```jsx
console.log(dog.hasOwnProperty('name')); // true
```

이 둘을 비교해보면 in이라는 연산자를 이용해서 확인하는 것이 더 효율적이라는 것을 알 수 있다.

오브젝트의 각각의 프로퍼티는 프로퍼티 디스크립터라고 하는 객체로 저장되어있다.

**한번 디스크립터를 확인해보자**

### Object.getOwnPropertyDescriptors

가지고 있는 모든 키의 디스크립터를 확인할 수 있다.

```jsx
const descriptors = Object.getOwnPropertyDescriptors(dog);
console.log(descriptors);
// {
//  name: { value: '와우', writable: true, enumerable: true, configurable: true },
//  emoji: { value: '🐶', writable: true, enumerable: true, configurable: true }
// }
```

→ 각각 name이라는 키에 있는 디스크립터, emoji라는 키의 디스크립터를 각각 확인할 수 있다

- value : 값
- writable : 값 수정 여부
- enumerable : 값 열거 여부
- configurable : 속성 수정 및 삭제 여부

→ 기본적으로 설정을 따로 하지 않으면 모두 default 값으로 true가 나온다

**그렇다면 만약 특정한 키의 하나만 받아오고 싶다면 어떻게 해야 할까?**

### Object.getOwnPropertyDescriptor

특정한 키의 디스크립터를 확인할 수 있다.

```jsx
const desc = Object.getOwnPropertyDescriptor(dog, 'name');
console.log(desc);
// { value: '와우', writable: true, enumerable: true, configurable: true }
```

→ name에 대한 디스크립터를 확인할 수 있다.

**위의 디스크립터를 변경하고 싶다면 어떻게 해야할까?**

### Object.defineProperty

```jsx
Object.defineProperty(dog, 'name', {
  value: '멍멍',
  writable: false, // 수정할 수 있니? 아니
  enumerable: false, // 열거할 수 있니? 아니
  configurable: false, // 삭제할 수 있니? 아니
});
```

→ 각각의 디스크립터들을 false로 모두 변경해보자

```jsx
console.log([dog.name](http://dog.name/)); // '멍멍'
```

→ dog 객체의 name이 변경되어 멍멍으로 출력되는 것을 확인할 수 있다.

**그리고 열거를 할 수 없게 false로 변경해두었으니 한번 확인해보자**

```jsx
console.log(Object.keys(dog)); //[ 'emoji' ]
console.log(Object.values(dog)); //[ '🐶' ]
console.log(Object.entries(dog)); //[ [ 'emoji', '🐶' ] ]
```

key, value, entries를 호출해도 모두 emoji에 관한 값만 나열되어 출력되는 것을 확인할 수 있다.

그렇다면 삭제 여부도 false로 변경해두었으니 확인해보자

```jsx
delete dog.name;
console.log(dog.name); //멍멍
```

→ delete를 활용해 지워도 계속해서 dog의 name이 출력되어 나오는 것을 확인할 수 있다.

**그렇다면 하나의 디스크립터 말고 여러 개의 디스크립터를 수정하고 싶다면 어떻게 해야 할까?**

### Object.defineProperties

```jsx
const student = {};
Object.defineProperties(student, {
  firstName: {
    value: '영희',
    writable: true,
    enumerable: true,
    configurable: true,
  },
  lastName: {
    value: '김',
    writable: true,
    enumerable: true,
    configurable: true,
  },
  fullName: {
    get() {
      return `${lastName} ${firstName}`;
    },
    set(name) {
      [this.lastName, this.firstName] = name.split(' ');
    },
    configurable: true,
  },
});

console.log(student);
```

이와 같이 definedProperties를 활용하여 각각의 키를 생성하여 만들어낼 수 있다.

그렇다면 매번 이렇게 defineProperties를 활용하여 매번 속성들을 변경해야 할까?

아니다. 속성들을 동결시키고, 확장을 금지 시키는 등의 다양한 매서드들이 있다

### Object.freeze

오브젝트의 속성을 동결시킨다

```jsx
Object.freeze(dog);
dog.name = '멍멍';
console.log(dog.name); //와우
```

값을 추가해도 추가되지 않는다

```jsx
dog.age = 4;
console.log(dog.age); //undefined
```

키를 삭제도 불가능하다

```jsx
delete dog.name;
console.log(dog); //{ name: '와우', emoji: '🐶', owner: { name: 'yxxn' } }
```

이처럼 객체를 동결하면 추가, 삭제, 쓰기, 속성 재정의 모두 불가능하다

단, 얕은 카피처럼 얕은 동결이 된다.

```jsx
yxxn.name = 'ㅎㅎ';
console.log(dog.owner); //ㅎㅎ
```

이와 같이 중첩되어 있는 값을 변경하면 얕은 카피처럼 얕게 동결되어 값이 변경되는 것을 확인할 수 있다

### Obeject.assign

오브젝트를 복사해서 가져온다

```jsx
const cat = {};
Object.assign(cat, dog);
console.log(cat); //{ name: '와우', emoji: '🐶', owner: { name: 'ㅎㅎ' } }
```

→ dog에 있는 모든 프로퍼티를 복사해서 가져올 수 있다.

```jsx
const cat = { ...dog };
console.log(cat); //{ name: '와우', emoji: '🐶', owner: { name: 'ㅎㅎ' } }
```

→ 스프래드 연산자를 이용하여 dog를 하나하나 풀어서 가져오는 것과 비슷하다

### Object.seal

객체를 밀봉한다

값의 수정은 가능하나 키를 추가하거나 삭제, 속성 재정의는 불가능하다

```jsx
Object.seal(cat);
cat.name = '야옹';
console.log(cat); //{ name: '야옹', emoji: '🐶', owner: { name: 'ㅎㅎ' } }
```

name의 value가 변경된 것을 확인할 수 있다.

**그렇다면 삭제는 가능할까?**

```jsx
delete cat.emoji;
console.log(cat); //{ name: '야옹', emoji: '🐶', owner: { name: 'ㅎㅎ' } }
```

삭제는 되지 않는 것을 확인할 수 있다

**그렇다면 freeze 되었는지 seal 되었는지 확인은 어떻게 할까?**

### Object.isFrozen / Object.isSealed

```jsx
console.log(Object.isFrozen(dog));
console.log(Object.isSealed(dog));
console.log(Object.isFrozen(cat));
console.log(Object.isSealed(cat));
```

→ 값은 true / false boolean으로 나온다

### Object.preventExtensions

확장 금지 / 추가만 불가

```jsx
const tiger = {
  name: '어흥',
};

Object.preventExtensions(tiger);
console.log(Object.isExtensible(tiger)); //false
```

**이렇게 확장이 금지된 오브젝트는 값 수정이 가능하다**

```jsx
tiger.name = '야옹';
console.log(tiger); //{ name: '야옹' }
```

**값 삭제도 가능하다**

```jsx
delete tiger.name;
console.log(tiger); //{}
```

그러나 확장이 불가능하기 때문에 새로운 값을 추가는 할 수 없다

```jsx
tiger.age = 1;
console.log(tiger); //{}
```

## 프로토타입 Prototype

```jsx
const dog1 = {
  name: '뭉치',
  emoji: '🐶',
};

const dog2 = {
  name: '뭉치',
  emoji: '🐩',
};
```

이전에 이와 같이 동일한 오브젝트를 만들 때에는 생성자 함수를 이용하여 만들어낼 수 있었다.

```jsx
function Dog(name, emoji) {
  this.name = name;
  this.emoji = emoji;
}

const dog1 = new Dog('뭉치', '🐶');
const dog2 = new Dog('코코', '🐩');

console.log(dog1, dog2);
//Dog { name: '뭉치', emoji: '🐶' } Dog { name: '코코', emoji: '🐩' }
```

이렇게 생성자 함수를 이용하여 코드를 재사용 할 수 있었다

**그렇다면 여기서 생성자 안에 함수가 있다면 어떻게 될까?**

```jsx
function Dog(name, emoji) {
  this.name = name;
  this.emoji = emoji;
  this.printName = () => {
    console.log(`${this.name} ${this.emoji}`);
  };
}

const dog1 = new Dog('뭉치', '🐶');
const dog2 = new Dog('코코', '🐩');

console.log(dog1, dog2);
//Dog { name: '뭉치', emoji: '🐶', printName: [Function (anonymous)] }
//Dog { name: '코코', emoji: '🐩', printName: [Function (anonymous)] }
```

→ 계속해서 동일한 함수가 추가되어 출력되는 것을 확인할 수 있다.

### 인스턴스 레벨의 함수

```jsx
this.printName = () => {
  console.log(`${this.name} ${this.emoji}`);
};
```

만들어진 인스터스들마다 동일한 함수를 가지고 있다

그렇다면 생성자 함수를 이용해서 100개를 만든다면 100개의 함수가 계속해서 생성이 된다. 그렇게 되면 메모리 낭비가 심하게 일어나게 된다는 것이다.

### 이 점을 보완하기 위해 프로토타입 레벨의 함수를 생성해보자

```jsx
Dog.prototype.printName = function () {
  console.log(`${this.name} ${this.emoji}`);
};
const dog1 = new Dog('뭉치', '🐶');
const dog2 = new Dog('코코', '🐩');
//Dog { name: '뭉치', emoji: '🐶' } Dog { name: '코코', emoji: '🐩' }
```

→ 더이상 인스턴트 레벨의 함수들이 계속해서 출력되지 않는 것을 확인할 수 있다

```jsx
dog1.printName(); //뭉치 🐶
dog2.printName(); //코코 🐩
```

→ 접근 및 사용 가능한 것을 확인할 수 있다.

크롬 콘솔 창에 찍어보면 다음과 같이 나와있는 것을 볼 수 있다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b0a949eb-17d2-4730-9ee9-b4e526cf7aea/Untitled.png)

dog1, dog2는 각각 이름과 이모지를 가지고 있고 프로토타입으로 오브젝트를 가지고 있다.

그 프로토타입을 보면 그 안에 printName이라는 함수를 가지고 있는 것을 볼 수 있다.

즉, 동일한 프로토타입을 상속하고 있는 것을 확인해볼 수 있다.

### 오버라이딩

인스턴스 레벨에서(자식) 동일한 이름으로 함수를 재정의하면(오버라이딩하면) 프로토타입 레벨의(부모) 함수의 프로퍼티는 가려진다

즉, 셰도잉된다.

예를 들어 아래와 같이 printName 함수가 있다고 가정하자

```jsx
Dog.prototype.printName = function () {
  console.log(`${this.name} ${this.emoji}`);
};
```

하단에서 나는 dog1만의 printName을 사용하고 싶어 아래와 같이 지정하여보자

```jsx
dog1.printName = function () {
  console.log('안녕');
};

dog1.printName(); // 안녕
```

→ 오버라이딩 되어 안녕이 출력되어 나오는 것을 확인할 수 있다.

즉, 자식 레벨에서 함수를 재정의하지 않으면, 프로토타입에 있는 것을 쓰고, 재정의하면 오버라이딩 되어 프로토타입 레벨에 있는 함수 프로퍼티는 가려지고 오버라이딩 되어 재정의한 것이 출력된다.

이처럼 프로퍼티 레벨이 함수는 우리가 각각 만들어진 객체에서 공통적으로 사용할 수 있는 함수를 만들었다.

클래스에서 static 함수를 만들면 각각의 인스턴트에서는 접근이 불가능하지만 클래스 이름으로는 접근이 가능하였다.

**이와 같은 정적 레벨의 함수도 프로퍼티에서 만들 수 있다.**

### 정적 레벨

```jsx
Dog.hello = () => {
  console.log('hello');
};
```

이렇게 생성자 이름 뒤에 함수를 바로 만들면 정적 레벨의 함수를 만들어낼 수 있다.

그렇다면 한번 인스턴스에서 접근을 해보자.

```jsx
dog1.hello();
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b733dac9-5527-44b9-90e8-4ac9bbf3d539/Untitled.png)

→ 접근이 불가능하다.

그렇다면 생성자 이름으로 접근을 해보자.

```jsx
Dog.hello(); //hello
```

→ 정상적으로 hello가 출력되는 것을 확인할 수 있다.

각각의 인스턴스 별로 공통된 함수가 있다면, 이와 같이 쓸 수 있다.
