# 연산자

### 리터럴 literal: 코드에서 값을 나타내는 표기법

- 123 - number literal
- ‘123’ - string literal
- true / false - boolean literal
- { } - object literal
- [ ] - array literal
- `` - template literal
- function() {} - function literal
- 123n - bigInt literal
- ob101 - binery literal

이렇게 특정한 값을 나태는 것을 리터럴이라고 한다

### 문 statement

코드에서 최소 실행 단위 한줄 한줄을 의미한다

선언문, 할당문, 조건문, 반복문 등이 있다.

### 여기서 문 중에서 값으로 평가될 수 있는 문을 표현식 expressions라고 한다
</br>
</br>
예를 들면

```jsx
1; // 숫자 리터럴 표현식
1 + 1; // 연산자 표현식
call(); // 함수 호출 표현식
```

함수를 불러서 값으로 평가될 수 있기 때문에 함수도 표현식이라고 한다.

즉, 이 표현식은 각각의 문이라고도 말할 수 있다.

문을 닫을 때는 항상 세미콜론이 있어야 한다.

그러나 변수를 선언하는 것은 표현식이 아니라 그냥 문이다.

```jsx
let b; //선언문
```

→ 선언문은 값이 없고 선언만 하였기 때문에 값으로 평가될 수 있는 것이 아니기 때문에 선언문이다.
</br>
</br>
```jsx
b = 2; //할당문, 할당 표현식인 문
```

→ 그러나 값을 할당한 경우에는 값으로 평가될 수 있기 때문에 표현식이다.

**좀 더 자세히 설명해보자면 아래의 예를 보자**

```jsx
let b; //선언문 
b = 2; //표현식, 할당문

let a = let b;
```

이렇게 a에게 let b를 선언해주려고 하면 값이 아니기 때문에 Unexpected identifier 에러가 뜬다.
</br>
</br>
</br>
## 산술 연산자 Arithmetic Operators

- **+ 더하기**

```jsx
console.log(5+2); //7
```

### + 연산자 주의점

+는 문자열을 연결할 때에도 사용을 했었는데 문자와 숫자를 더하게 되는 일이 생길 수 있으므로 주의해야 한다. 
</br>
</br>
**아래의 예를 보자**

```jsx
let text = '두개의' + '문자를';
console.log(text); // 두개의 문자를
```

```jsx
text = '1' +1
console.log(text); //11(string)
```

→ 숫자와 문자열을 더하면 문자열로 변환된다.

**그러므로 내가 지금 더하는 것이 숫자와 숫자인지, 문자와 문자인지, 숫자와 문자열인지 생각하면서 코딩을 해야한다.**
</br>
</br>
- **- 빼기**

```jsx
console.log(5-2); //3
```
</br>
</br>
- *** 곱하기**

```jsx
console.log(5*2); //10
```
</br>
</br>

- **/ 나누기**

```jsx
console.log(5/2); //2.5
```
</br>
</br>
- **% 나머지값**

```jsx
console.log(5%2); //1
```

→ 5를 2로 나눈 다음 나머지 값 
</br>
</br>
- **** 지수(거듭제곱)**

```jsx
console.log(5**2); //25
```

→ es7에 추가된 문법이다.

```jsx
console.log(Math.pow(5, 2));
```

→ 이전에는 Math를 이용하여 5에 2번 거듭제곱이 일어날 수 있게 사용하였다.
</br>
</br>
</br>
## 단항 연산자 Unary Operators

- **+(양)**
- **-(음)**

```jsx
let a = 5;
a = -a // -1 *5
console.log(a); //-5
```

a에게 -a를 반환하라고 하는 것은 앞에서 a는 5라고 선언하였기 때문에 -1 * 5가 되는 것이다.
</br>
</br>
```jsx
a = -a;
console.log(-a); //5
```
</br>
</br>
그렇다면 현재 a는 -5인데 -a는 -(-5)가 되므로 값은 5가 찍히는 것을 확인할 수 있다.

```jsx
a = +a;
console.log(a); //5
```
</br>
</br>
여기에서는 a가 5인데 +(5)이므로 값은 5가 찍히는 것을 확인할 수 있다.

```jsx
a = -a;
a = +a;
console.log(a);
```

여기에서는 -(5)에서 +(-5)를 하기 때문에 -5가 되는 것을 확인할 수 있다
</br>
</br>

- **!(부정)**

```jsx
let boolean = true;
console.log(boolean); //true
console.log(!boolean); //false
console.log(!!boolean); //true
```

→ ! 붙으면 앞의 값의 부정 !! 붙으면 그 앞의 값의 부정이 된다
</br>
</br>
</br>

### + 연산자를 이용하여 숫자가 아닌 타입들을 숫자로 변환하면 어떤 값이 나올까?

```jsx
console.log(+false); //0
console.log(+null); //0
console.log(+''); //0
console.log(+true); //1
```

→ 텅빈 형태는 0, true는 1, false는 0으로 문자로 변환되어 나타난다.
</br>
</br>

**그렇다면 문자열을 숫자로 바꾸면 어떻게 될까?**

```jsx
console.log(+'text'); //NaN
```

→ 숫자열이 아니기 때문에 not a number이 뜬다
</br>
</br>

**그렇다면 undefined는 무슨 값이 나올까?**

```jsx
console.log(+undefined); //NaN
```

→ 어떠한 형태도 값도 정해지지 않았으므로 not a number이 나온다.
</br>
</br>

### 그렇다면 숫자 앞에 부정연산자를 쓰면 어떻게 될까?

```jsx
console.log(1);
console.log(!1); //false
console.log(!!1); //true
```

1이 가지고 있는 boolean을 출력해낼 때 !!를 이용하여 출력을 했었다.
</br>
</br>
**왜 그렇게 했을까?**

boolean이 아닌 데이터를 boolean으로 출력해내고 싶다면

1이 가지고 있는 boolean 값을 ! 한번 부정하면 false가 되고 한번 더 부정을 하여 원래의 값인  true를 출력할 수 있도록 만든 것이다.
</br>
</br>
</br>

## 할당 연산자 Assignment Operators

```jsx
let a = 1;
a = a + 2;
console.log(a); //3
```

위와 같이 변수에 값을 할당하는 것 “=” 이 할당 연산자이다
</br>
</br>
### 그렇다면 이 식은 어떻게 될까?

```jsx
a += 2; // a = a+2; 의 축약버전
console.log(a); //5
```

a += 2라는 것은 a라는 변수에게 a + 2를 하여 값을 할당하라는 뜻이다.

즉, 위에서 a는 3이라고 값을 할당하였기 때문에 3 + 2, 5가 되는 것이다.

```jsx
a -= 2
console.log(a); //3

a *= 2
console.log(a); //6

a /= 2;
console.log(a); //3

a %= 2;
console.log(a); //1

a **= 2;
console.log(a); //1
```

→ **이런 식으로 산술 연산자 축약버전으로 사용이 가능하다**
</br>
</br>
</br>

## 증감 연산자 Increment Operators & Decrement Operator

증가와 감소 연산자

```jsx
let a = 0;
console.log(a);

a = a +1;
console.log(a); //1
```

위와 같이 a에 1을 더하려면 a = a+1, 축약하여 a += 1이라고 할 수 있지만 아래와 같이 증감 연산자로 나타낼 수 있다.

### ++

```jsx
a++; //a = a +1;
console.log(a); //1
```

→ 위의 산술 연산자와 동일한 값이 나온 것을 확인할 수 있다.
</br>
</br>

### - -

```jsx
a--; //a = a-1;
console.log(a); //0
```
</br>
</br>
### 주의사항

증감연산자의 위치에 따라 동작이 달라진다!

- **a++** : 필요한 연산을 하고, 그 뒤 값을 증가시킨다

```jsx
a = 0;
let b = a++;
console.log(b); //0
console.log(a); //1
```

→ b라는 변수에 a를 먼저 할당하고 1을 증가시키니 콘솔에는 0이 출력된 것을 확인할 수 있다.

이렇게 먼저 할당한 후에 1을 증가시키니 a에는 1이 찍히는 것을 확인할 수 있다.
</br>
</br>

- **++a** : 값을 먼저 증가하고 필요한 연산을 한다

```jsx
a = 0;
let b = ++a;
console.log(b);
console.log(a);
```

→ b라는 변수에 1이 증감된 a를 할당시키니 a와 b 모두 1이 반환된 것을 알 수 있다.
</br>
</br>

### 이렇게 할당하는 것 외에도 log에 값을 반환할 때에도 똑같이 반영이 된다.

```jsx
a = 0;
console.log(a++); //0
console.log(a); //1
```

→ 여기서 먼저 필요한 연산, 출력을 먼저 하기 때문에 a++는 0을 출력 후에 1을 증가시키므로 a는 1이 출력된다
</br>
</br>
</br>

## 비교 연산자 Relational  Operators

대소 관계 비교 연산자

- > 크다
- < 작다
- >= 크거나 같다
- <= 작거나 같다

```jsx
console.log(2>3); //false
console.log(2<3); //true
console.log(3<2); //false
console.log(3<=2); //false
console.log(3>=3); //true
console.log(3>=3); //true
console.log(3>=2); //true
```

→ 이런식으로 특정한 값과 값을 비교할 수 있는 것이 비교 연산자이다

</br>
</br>
</br>

## 연산자 우선순위 Priority

```jsx
let a = 2;
let b = 3;
let result = a + b * 4;
console.log(result); // 14
```

곱하기가 들어있는 것을 먼저 12가 된 다음에 a를 더해서 14가 된다.

그렇다면 이 연산은 어떻게 값이 나올까?

```jsx
result = a++ + b * 4;
console.log(result); //14
console.log(a); //3
```

→ 위와 동일하게 b * 4 = 12에 a를 더해야 하는데 먼저 a++이므로 12에 a, 즉 2를 더한 값을 result에 반환 후에 ++ 1 이 증감하므로 14가 출력이 된다.

만약 a와 b를 더한 후에 4를 곱하고 싶다면 

```jsx
(a+b)*4
```

와 같은 형태로 괄호로 감싸주면 된다
</br>
</br>
</br>

## 동등 비교 연산자 Equality Operator

- **== : 값이 같음**

```jsx
console.log(2 == 2); //true
console.log(2 == 3); //false
console.log(2 == '2'); //true
```

→ number 2와 string 2는 타입은 비록 다르지만 숫자와 문자열을 비교할 때 문자열 안에 있는 숫자가 자동으로 숫자열로 바꾸어 비교를 해주기 때문에 값은 true이다

- != : 값이 다름

```jsx
console.log(2 != 2); //false
console.log(2 != 2); //true
```

- === : 값과 타입이 둘다 같음

```jsx
console.log(2 === '2'); //false
```

→ 값은 동일하지만 type이 number과 string으로 다르기 때문에 false이다.

⭐️ **개발을 할 때에는 값과 type을 비교하는 것이 좋으므로 === 를 쓰는 것이 제일 좋다**

- !== : 값과 타입이 다름
</br>
</br>

### 그렇다면 1과 true, 0과 false는 같을까?

```jsx
console.log(true == 1); // true
console.log(true === 1); // false
console.log(false == 0); // true
console.log(false === 0); // false
```

→ 1을 boolean 타입으로 출력하였을 때 1은 true이기 때문에 값이 동일하므로 true이다.

하지만 ===으로 type을 비교하면 boolean과 number이기 때문에 false가 나온다

→ 동일하게 0을 boolean 타입으로 출력하면 0은 false이기 때문에 동일해서 true, type은 서로 다르기 때문에 false가 나온다

</br>
</br>

### 그렇다면 더 심화해서 object를 이용해서 알아보자

```jsx
const obj1 = {
    name: 'js',
}
const obj2 = {
    name: 'js',
}

console.log(obj1 == obj2); //false
console.log(obj1 === obj2); //false
```

### 똑같이 이름이 js인 객체 obj1, obj2가 같은지 확인해보면 어떤 값이 나올까?

→ **false가 나온다**

왜냐하면 오브젝트가 만들어진 메모리 주소가 obj1에 저장이 되어있을 것이고, obj2에도 동일하게 저장된 메모리 주소가 할당이 되어있을 것이므로 아무리 안의 key와 value가 동일하다고 해도 메모리 주소가 서로 다르므로 false가 나온다.

**→ 값과 타입을 비교한 연산자에서도 false가 나온다.**

이미 값이 서로 다르다고 나왔고, 타입이 object로 동일하다고 해도 둘 중에 하나라도 다르면 false가 나온다.

```jsx
console.log(obj1.name == obj2.name); //true
console.log(obj1.name === obj2.name); //true
```

→ 둘다 value로 문자열 js를 가지고 있으므로 값을 비교하는 연산자도 true, 값과 타입을 비교하는 연산자도 true가 나온다
</br>
</br>

### 그렇다면 obj 자체를 할당하면 어떻게 될까?

```jsx
let obj3 = obj2;
console.log(obj3 == obj2); //true
console.log(obj3 === obj2); //true
```

→ obj3이라는 객체에 obj2의 메모리 주소를 할당하였으므로 주소 값도 동일, 타입도 객체로 동일하므로 둘 다 true이다