# 클래스

## 클래스 class

클래스란 생성자 함수와 마찬가지로 객체를 생성할 수 있는 템플릿이다

클래스는 다른 객체 지향 프로그램에서도 많이 쓰이기 때문에 중요하다!

→ 현업에서는 생성자함수 보다는 클래스를 더 많이 사용한다

**생성자 함수**는 생성자 함수를 이용하여 **객체**를 만들 수 있고

**클래스**는 클래스를 이용해 객체를 만들 수 있고 클래스를 이용해 만들어진 객체를 **인스턴스 instance**라고 한다
<br>

## 클래스의 기본

**객체를 손쉽게 만들 수 있는 템플릿**

1. 생성자 함수(오래된 고전적인 방법)
2. 클래스(최신, 다른 객체지향 프로그래밍에서도 사용)✨

```jsx
function Fruit(name, emoji) {  
  this.name = name; 
  this.emoji = emoji;
  this.display = () => {
    console.log(`${this.name}: ${this.emoji}`);
  };
  return this; 
}

const apple = new Fruit('apple', '🍎');
const orange = new Fruit('orange', '🍊');
```

→ 이 생성자 함수를 class로 변경해보자!

```jsx
class Fruit {
  constructor(name, emoji) {
    this.name = name; 
    this.emoji = emoji;  
  }  
  display = () => {  여기서 function을 붙이면 오류가 난다 
     여기서 멤버함수란 클래스 내부에 함수를 정의한 것을 멤버함수라고 한다
     객체 안에서 멤버함수가 있다면 생성자 내부가 아닌 외부에 넣어 생성하기 때문에 생성자함수와 달리 this를 붙이지 않는다
    console.log(`${this.name}: ${this.emoji}`);
  };
}
```

- class를 만들 때에는 앞에 class라는 keyword를 붙여서 생성한다
- 생성자 constructor : new 키워드로 객체를 생성할 때 호출되는 함수
    
    → 각각의 class에는 생성자 함수가 있다!
    
- 생성자 함수에는 객체를 만들 때 필요한 데이터를 인자로 받아올 수 있도록 함수 형태와 비슷하게 필요한 데이터를 외부로부터 받아올 수 있도록 생성자를 이용

```jsx
constructor(name, emoji) { }
```

- 객체 안에서 멤버함수가 있다면 생성자 내부가 아닌 외부에 넣어 생성한다

```jsx
  display = () => { }
```

→ 객체 안에서 멤버함수가 있다면 생성자 내부가 아닌 외부에 넣어 생성하기 때문에 생성자함수와 달리 this를 붙이지 않는다

+ ) 멤버함수란?  클래스 내부에 함수를 정의한 것을 멤버함수라고 한다

```jsx
console.log(apple); //Fruit { name: 'apple', emoji: '🍎', display: [Function (anonymous)] }
console.log(orange); //Fruit { name: 'orange', emoji: '🍊', display: [Function (anonymous)] }

console.log(apple.name); //apple
console.log(apple.emoji); //🍎
apple.display(); //apple: 🍎
```

→ 생성자 함수로 만들어낸 값과 동일하게 출력된 것을 확인할 수 있다

```jsx
console.log(apple);
```

→ apple은 Fruit 이라는 클래스의 인스턴스이다

```jsx
console.log(orange);
```

→ orange Fruit 이라는 클래스의 인스턴스이다

### 왜냐하면 Fruit 이라는 클래스로 만들어진 객체이기 때문에 인스턴스라고 부른다!

**그렇다면 아래의 객체와 비교해보자!**

```jsx
const obj = {name: 'ellie'}
```

→ obj는 객체이고, **그 어떠한 클래스의 인스턴스도 아니다**
<br>
<br>

## 재사용을 높이는 방법

```jsx
class Fruit {
  constructor(name, emoji) {
    this.name = name; 
    this.emoji = emoji;  
  }  
  display = () => { 
    console.log(`${this.name}: ${this.emoji}`);
  };
}
```

### → class를 이용하여 만든 인스턴스 안의 프로퍼티와 함수들은 인스턴스 레벨의 프로퍼티와 메서드이다

왜냐하면 중복적으로 만들어지기 때문이다

템플릿을 만들어두고 필요한 데이터를 주입해서 동일한 것을 반복적으로 지속적으로 인스턴스들을 만들어나갈 수 있기 때문이다

**인스턴스 레벨의 프로퍼티와 메서드이기 때문에 객체.속성, 객체.함수 등으로 호출할 수 있다**

### 그런데 모든 객체마다 동일하게 참조해야 하는 속성이나 행동이 있다면 어떻게 해야 할까?

**→ 클래스 레벨의 프로터티와 메서드를 이용하면 할 수 있다!**

클래스 안에서 static()이라는 키워드를 프로퍼티 앞이나 메서드 앞에 붙여서 사용하면 가능하다

단, static()이라는 키워드를 이용하여 만든 프로퍼티나 메서드는 만들어진 **인스턴스에 포함되지 않고 클래스에 그대로 남게 된다.**

- 각각의 인스턴스에 들어가지 않는다!
- 클래스에 한번 정의하고 재사용할 수 있다

그렇기 때문에 호출할 때는 객체.속성, 객체.함수가 아닌 **클래스 이름.메서드**로 호출할 수 있다

**아래의 예를 참고해보자**
<br>
<br>

## static 정적 프로퍼티, 메서드

```jsx
class Fruit {
    constructor(name, emoji) {
      this.name = name; 
      this.emoji = emoji;  
    }  
    display = () => { 
      console.log(`${this.name}: ${this.emoji}`);
    };
  }
  console.log(Fruit.name);
  Fruit.display(); //Fruit.display is not a function
```

→ static을 붙이지 않으면 인스턴스 레벨의 프로퍼티와 메서드이기 때문에 클래스 이름으로 접근이 불가능하다

→ 함수가 아니라고 오류가 뜨는 것을 확인할 수 있다

```jsx
static MAX_FRUITS = 4;
console.log(Fruit.MAX_FRUITS); 4
```

→ 속성 앞에 메서드를 붙일 수 있다

- **인스턴스 레벨의 메서드**

```jsx
display = () => { 
  console.log(`${this.name}: ${this.emoji}`);
};
```

→ 왜냐하면 생성자 함수 내부의 name과 emoji로 이루어진 함수이기 때문에!

### 클래스별로 공통적으로 사용할 수 있고 만들어진 인스턴스 데이터에 참조할 필요가 없는 함수라면 static으로 만들 수 있다

- **클래스 레밸의 메서드**

```jsx
static makeRandomFruit() {
  return new Fruit('banana', '🍌');
}
```

→ static이 붙은 메서드, 즉 클래스 레벨의 메서드에서는 this를 참조할 수 없다

왜냐하면 **클래스 자체로는 아무것도 채워지지 않은 템플릿**이기 때문!

```jsx
const banana = Fruit.makeRandomFruit();
```

→ banana는 Fruit이라는 클래스에 있는 makeRandomFruit라는 static함수를 직접적으로 호출할거야

```jsx
console.log(banana); Fruit 
//{ display: [Function: display], name: 'banana', emoji: '🍌' }
```

→ 콘솔에 출력된 내용을 보면 인스턴스 레밸의 메서드들은 출력이 되었으나 static 클래스 레벨의 메서드는 출력이 되지 않음을 확인할 수 있다

### static의 사용예제

- Math.pow();
- Number.isFinite(1);

→ static를 이용하면 우리가 굳이 오브젝트를 만들지 않아도 비슷한 내용을 묶어서 사용할 수 있다는 장점이 있다
<br>
<br>

## 필드

### 접근제어자

만약 apple이라는 객체를 만들었다면 외부에서 접근 및 변경이 불가능하게 만들고 싶다면 어떻게 해야 할까?

→ 접근제어자를 이용해 캡슐화를 할 수 있다

다른 프로그래밍에서는 private, public, protexted라는 키워드로 접근 제어할 수 있다

자바스크립트에서는 private을 이용하고 싶다면 private는 #을 붙여 사용, public은 기본값, protexted는 사용불가

```jsx
class Fruit {
  #name;  
  #emoji; 
  #type = '과일'; 
  constructor(name, emoji) {
    this.#name = name;  내부에서 접근할 때도 #을 붙여 접근
    this.#emoji = emoji;  
  }  
  display = () => { 
    console.log(`${this.#name}: ${this.#emoji}`);
  };
}
```

```jsx

  name;  // 외부로부터 전달받은 데이터를 할당
  emoji;  // 외부로부터 전달받은 데이터를 할당
```

→ name과 emoji는 외부로부터 전달받은 데이터를 할당하는 key, constructor로 만들어진 데이터라면 생략이 가능하다

```jsx
 #type = '과일';  
  constructor(name, emoji) {
    this.#name = name;  
    this.#emoji = emoji;  
  }  
```

→ type은 만들어지자마자 과일로 초기화가 되어있다. 이것을 **필드**라고 한다

    인스턴스 만들어질 때 초기화가 되어야 한다면 constructor 밖에 위에서 선언

→ 내부에서 접근할 때도 #을 붙여 접근해야 한다

```jsx
const apple = new Fruit('apple', '🍎');

Fruit {
  name: 'apple',
  emoji: '🍎',
  type: '과일',
  display: [Function: display]
}
```

→ 따로 type을 설정하지 않아도 초기화가 되어 type은 과일이라고 출력이 된 것을 확인할 수 있다.

```jsx
  #name;  
  #emoji; 
```

→ #을 붙이는순간 내부에서는 사용이 가능하나 외부에서는 접근이 불가능하다

**이렇게 #을 붙여 제어하면 외부에서 접근했을 때 어떻게 나오는지 확인해보자**

```jsx
const apple = new Fruit('apple', '🍎');
apple.#name = 'orange'; 
```

→ private field '#name' must be declared in an enclosing class 오류가 뜨는 것을 확인할 수 있다
 #field는 외부에서 접급이 불가능함

**그렇다면 apple을 콘솔에 한번 확인해보자**

```jsx
console.log(apple);  

Fruit {
  name: undefined,
  emoji: undefined,
  display: [Function: display]
}
```

→ 필드들의 정보가 나타나지 않는 것을 확인할 수 있다 
<br>
<br>

## getter & setter

접근자 프로퍼티

다음 예시를 보며 확인을 해보자

```jsx
class Student {
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }

  fullName() {
    return `${this.firstName} ${this.lastName}`
  }
}

const student = new Student('수지', '배');
console.log(student.firstName); //속성에 접근하는 것 프로퍼티를 부를 수 있다
console.log(student.lastName);
console.log(student.fullName());  //그러나 fullName은 함수이기 때문에 함수를 호출하듯이 부르고 있다.
 행동이 아니고 이름의 상태를 만든 건데 함수 호출하듯이 부르고 있으니 조금은 이상하다
 이때 사용할 수 있는 것이 접근자 프로퍼티이다
```

```jsx
console.log(student.firstName); //수지
console.log(student.lastName); //배
```

→ 속성에 접근하는 것처럼 프로퍼티를 부를 수 있다

```jsx
console.log(student.fullName());  
```

→ 그러나 fullName은 함수이기 때문에 함수를 호출하듯이 부르고 있다.
 행동이 아니고 이름의 상태를 만든 건데 함수 호출하듯이 부르고 있으니 조금은 이상하다
 이때 사용할 수 있는 것이 **접근자 프로퍼티**이다

그런데 여기서 의문이 들 수 있다! 함수 대신에 이렇게 쓰면 되지 않을까?

```jsx
this.fullName = `${this.firstName} ${this.lastName}`;
console.log(student.firstName); // 수지 배
```

그러나 여기서 학생의 이름을 바꾼다고 해보자

```jsx
student.firstName = '안나';
```

한번 콘솔에 찍어보자

```jsx
console.log(student.firstName); //안나  
```

→ 재선언한 이름이 정상적으로 변경이 된 것을 확인할 수 있다

그렇다면 전체 풀네임을 출력해보면 어떻게 나올까?

```jsx
console.log(student.fullName); //수지 배
```

→ 변경한 이름이 반영이 되지 않고 생성자에서 한번 만들어지고 나서 업데이트가 되지 않았기 때문

그러나 우리는 매번 이름을 업데이트할 때마다 fullName도 변경이 되는 것을 원하기 때문에 이 방법은 적절하지 않다

그렇다면 어떻게 해야 할까? 

### 바로 접근자 프로퍼티를 이용하면 된다!

방법은 간단하다!

```jsx
get fullName() {
    return `${this.firstName} ${this.lastName}`
  }
console.log(student.fullName); // 안나 배
```

함수 앞에 get을 붙인 후 일반 프로퍼티에 접근하는 것처럼 호출하면 된다

접근자 프로퍼티를 쓰면 함수지만 즉 고정된 값이 아니라 호출하는 시점에 데이터를 만들어서 리턴을 하는데 속성에 가깝다

일이라기 보다는 어떠한 특정한 속성을 조합해서 보여주기 때문에 함수보다는 get이라는 keyword를 붙이면 속성에 접근하는 것처럼 할 수있다.

**get 이외에도 set을 붙여서 사용하는 방법도 있다!**

set - 할당을 할 때 바로 이 함수가 호출이 된다 할당하는 값도 여기서 받아올 수 있다

```jsx
set fullName(value) {
      console.log(value);
  }

console.log(student.fullName); // 김철수
student.fullName = '김철수';
```

→ 값을 할당하니 set이 호출되어 그 값이 출력이 된다
<br>
<br>

## 확장 extends

만약 우리가 호랑이, 강아지를 나타내는 클래스를 만든다고 가정해보자

```jsx
class Tiger {
  constructor(color) {
    this.color = color;
  }
  eat() {
    console.log('먹자!');
  }

  sleep() {
    console.log('잔다!');
  }
}

class Dog {
  constructor(color) {
    this.color = color;
  }
  eat() {
    console.log('먹자!');
  }

  sleep() {
    console.log('잔다!');
  }

  play() {
    console.log('놀자!');
  }
}
```

→ 호랑이와 강아지를 나타내는 함수들이 중복되는 것이 많은데 이것을 하나로 만들어둘 수는 없을까?

### 확장을 이용하면 가능하다!

```jsx
class Animal {
  constructor(color) {
    this.color = color;
  }
  eat() {
    console.log('먹자!');
  }

  sleep() {
    console.log('잔다!');
  }
}
```

→ 이처럼 animal이라는 확장 클래스를 만들어 기본 생성자인 color과 공통적인 eat과 sleep을 가져와서 지정해주면 된다.

이렇게 지정하면 모든 동물들은 외부로부터 색상을 받아올 수 있고 먹고 자는 행동을 할 수 있게 되었다.

```jsx
class Tiger extends Animal {}
const tiger = new Tiger('노랑이');
console.log(tiger); // Tiger { color: '노랑이' }
tiger.sleep(); //잔다!
tiger.eat(); //먹자!
```

→ tiger는 animal과 동일하기 때문에  extends 상속, 모두 가지고 온다고 지정하면 위와 같이 제대로 출력되는 것을 확인할 수 있다

즉, 상속만하고 클래스 안에 아무것도 작성하지 않아도 제대로 상속받아 나온 것을 확인할 수 있다

일반 프로퍼티처럼 접근도 가능한 것을 볼 수 있다.

이처럼 공통된 것이 있다면 무언가 개념적으로 생각해도 공통되었다면 (호랑이와 강아지는 동물이니까) 클래스를 이용하여 부모와 자식간의 상속하는 것처럼 상속할 수 있다!

**그렇다면 강아지를 한번 만들어보자!**

```jsx
class Dog extends Animal {
  constructor(color, ownerName) {
    super(color);
    this.ownerName = ownerName;
  } 
    play() {
    console.log('놀자!');
  }
}
const dog = new Dog('백구', '엘리'); 
console.log(dog); //Dog { color: '백구', ownerName: '엘리' }
dog.sleep(); //잔다!
dog.eat(); //먹자!
dog.play(); //놀자!
console.log(dog.ownerName); //엘리
```

확장 클래스에서 더 추가할 함수나 속성이 있다면 이와 같이 추가를 해줄 수 있다

```jsx
play() {
	console.log('놀자!');
}
dog.play(); //놀자!
```

→ 추가되어 제대로 출력된 것을 확인할 수 있다

**이렇게 상속을 이용하여 공통된 내용을 상속받을 수 있고 추가할 내용이 있다면 추가를 하여 출력해낼 수 있다**

### 만약에 조금 더 나아가서 강아지에게 주인의 이름을 생성자로 추가하고 싶다면 어떻게 해야 할까?

```jsx
class Dog extends Animal {
  constructor(color, ownerName) {
    super(color);
    this.ownerName = ownerName;
  } 
```

→ constructor을 이용하여 외부로부터 값을 받아올 수 있게 선언을 해주면 된다.

대신, 자식 클래스에서 constructor을 이용하여 정의하는 순간 부모 즉, animal에 필요한 것들도 받아와야 한다

animal에는  color가 선언되어 있으므로 color를 받아와야 하는데 이 color는 부모의 것이므로  super이라는 keyword를 붙여 가져와야 한다.

여기서  super는 부모를 가리킨다. 즉, 부모 생성자의 color를 가져오겠다고 선언하는 것이다

그 이후 추가할 생성자 key는 일반 클래스처럼 동일하게 선언해주면 정상적으로 출력이 가능한 것을 확인할 수 있다

**그렇다면 여기서 부모의 eat을 강아지에서는 조금 다르게 출력을 하고 싶다면 어떻게 해야 할까?**
<br>
<br>

### 오버라이딩 overriding

```jsx
eat() {
  console.log('강아지가 먹는다!');
}

dog.eat(); //강아지가 먹는다!
```

→ 그냥 동일하게 그 함수 그대로 입력을 하여 덮어씌우기를 하면 된다

이렇게 부모의 클래스 속성을 자식에서 덮어씌우는 것을 **오버라이딩 overriding**이라고 한다

**그렇다면 부모의 기능을 그대로 유지하면서 추가적으로 무언가를 하고 싶다면 어떻게 해야 할까?**

```jsx
eat() {
    super.eat();
    console.log('강아지가 먹는다!');
  }
dog.eat(); // 먹자! 강아지가 먹는다!
```

→ super라는 키워드를 이용하여 부모의 함수를 입력 후에 새롭게 추가할 내용을 입력해주면 부모의 값인 먹자가 먼저 나온 후에 추가로 입력한 강아지가 먹는다가 출력되는 것을 확인할 수 있다

→ 만약에 추가 내용이 나온 후에 부모의 함수가 출력되길 희망한다면 순서를 바꿔주면 된다

참고 자료: 

[클래스 퀴즈1](https://www.notion.so/1-8e69e008ffcd409ca48a3f10b3014dc5) 

[클래스 퀴즈2](https://www.notion.so/2-12e407d3b1b0445997fe59aeb42b5e91)