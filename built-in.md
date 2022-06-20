# 유용한 객체들

## 내장 객체 Built-in Objects

우리가 자바스크립트를 작성하면 어플리케이션은 javascript runtime 환경에서 사용이 되는데 이것은 브라우저가 될 수도 있고 노드가 될 수도 있다
<br>
<br>

## 래퍼 객체 (Wrapper Object)

원시값을 필요에 따라서 관련된 빌트인 객체로 변환한다

### 호스트 객체 Host Object

→ browser APIs, Node APIs

```jsx
const number = 123; //number 원시타입
console.log(number); //123
```

→ 넘버, 숫자를 선언하여 콘솔에 찍어보면 숫자열로 출력이 되는 것을 확인할 수 있다.

그러나 number 뒤에 dot, 점을 찍어보면 다음과 같이 무언가가 나타나는 것을 확인할 수 있다
→  number 원시 타입을 감싸고 있는 Number라는 클래스인 객체로 감싸줄 수 있는 것들이 나타난다
<br>
<br>

**한번 toString을 선택해보자**

```jsx
console.log(number.toString()); // 123 문자열로 변환함
```

→ 문자열로 반환해주는 wrapper 객체를 선택하자 콘솔에 123이 문자열로 출력되는 것을 확인할 수 있다.
<br>
<br>

**그렇다면 다시 number만 다시 출력해보자**

```jsx
console.log(number);  
```

→ 다시 number의 원시타입으로 출력되는 것을 확인할 수 있다.

**그렇다면 문자열은 어떻게 될까?**

```jsx
const text = 'text'; // string 문자열 원시타입
console.log(text);
text.length // String 객체 
text.trim()
```

→ 이처럼 string 원시타입을 입력을 해서 wrapper 객체로 감싸줄 수 있다

이렇게 각각의 원시타입들이 그에 해당하는 오브젝트, wrapper object가 있다.

평소에는 원시타입 그대로 사용하다가 필요에 의해서 wrapper 객체로 감싸져서 함수로 사용하다가 다시 원시객체로 돌아와서 사용할 수 있다.

### 그렇다면 계속 wrapper 객체로 사용하면 되지 않을까?

**→ 결론부터 말하자면 별로 좋지 않은 방법이다!**

왜냐하면 오브젝트들은 값 뿐만 아니라 다양하고 많은 데이터를 가지고 있기 때문에 원시타입보다 훨씬 더 무거워 모든 원시타입들을 오브젝트, 함수로 변경하여 사용을 하게 되면 그만큼 메모리를 많이 차지한다.

값을 만들 때마다 객체를 생성하면 메모리를 많이 차지하게 되어 문제가 생기기 때문이다.

### 그러므로 필요할 때만 원시타입을 객체로 변경하여 사용하였다가 다시 원시타입으로 돌려놓는 것이 제일 좋은 방법이다
<br>
<br>
<br>

## 글로벌 객체 global object

```jsx
console.log(globalThis);
```

```jsx
<ref *1> Object [global] {
  global: [Circular *1],
  clearInterval: [Function: clearInterval],
  clearTimeout: [Function: clearTimeout],
  setInterval: [Function: setInterval],
  setTimeout: [Function: setTimeout] {
    [Symbol(nodejs.util.promisify.custom)]: [Getter]
  },
  queueMicrotask: [Function: queueMicrotask],
  performance: Performance {
    nodeTiming: PerformanceNodeTiming {
      name: 'node',
      entryType: 'node',
      startTime: 0,
      duration: 27.056040999945253,
      nodeStart: 1.059416000265628,
      v8Start: 1.5839160000905395,
      bootstrapComplete: 21.99620799999684,
      environment: 11.973583000246435,
      loopStart: -1,
      loopExit: -1,
      idleTime: 0
    },
    timeOrigin: 1655207675640.724
  },
  clearImmediate: [Function: clearImmediate],
  setImmediate: [Function: setImmediate] {
    [Symbol(nodejs.util.promisify.custom)]: [Getter]
  }
}
```

globalThis를 콘솔에 찍어보면 노드에서 사용하는 global 객체들이 쭉 나온다

globalThis 외에도 더 많은 글로벌 객체들이 있다.

```jsx
console.log(Infinity); //Infinity
console.log(NaN); //NaN
console.log(undefined); //undefined
```

→ 전역적으로 사용할 수 있으며 브라우저에서도 사용이 가능하다

브라우저에서 globalThis, this를 출력하면 window가 맨 앞에 출력되는 것을 확인할 수 있다.

그러나 노드에서 this를 출력하면 조금 다른 값이 나온다

```jsx
console.log(this); // { }
```

왜냐하면 노드에서의 this는 현재 모듈에 있는 정보를 출력하는데 현재 모듈에는 아무 정보도 없기 때문에 빈 객체가 나오는 것이다

### 자바스크립트에서 globalThis, this는 전역을 가리킨다
<br>
<br>

## Function properties

전역적으로 사용할 수 있는 유용한 함수들이 많다

- **eval** : 자바스크립트를 한줄한줄씩 표현

```jsx
eval('const num = 2; console.log(num)') //2
```

→ 문자열 형태로 자바스크립트를 입력하면 자바스크립트를 평가해서 그 값을 출력해준다

- **isFinite()** : 숫자가 유한한지 아닌지 확인 (값은 true / false로 나옴)

```jsx
console.log(isFinite(1)); //true
console.log(isFinite(Infinity)); //false
```

→ Infinity는 무한을 뜻하기 때문에 false가 출력되는 것을 확인할 수 있다

- **isNaN()** : 숫자인지 아닌지 확인

- **parseFloat()** : 문자열인데 소숫점이 들어있는 숫자를 나타내고 있다면 숫자로 반환

```jsx
console.log(parseFloat('12.43')); //12.43
```

- **parseInt()** : 문자열 안에 있는 실수를 정수로 반환해줌

```jsx
console.log(parseInt('12.43')); //12
console.log(parseInt('11')); //11
```

→ parseInt 안에 그냥 정수를 넣어도 정수로 반환해준다

### URI, Uniform Resourse Identifier URL 하위 개념

어떤 리소스를 나타낼 수 있는 단 하나의 고유한 주소나 id를 가리킨다

웹사이트를 나타내는 유일한 것이 바로 url 주소

url 주소는 아스키 문자로만 구성되어야 하는데 만약 한글이나 특수문자를 주소에 사용하게 되면 이스케이프 처리를 해야 한다

이것을 유용하게 쓸 수 있는 글로벌 객체가 **encodeURI**이다

- **encodeURI()** : 주소에 특수문자나 한글이 들어갈 경우 이스케이프 처리를 해줌

```jsx
const URL = "https://네이버.com";
const encoded = encodeURI(URL);
console.log(encoded); // https://%EB%84%A4%EC%9D%B4%EB%B2%84.com
```

만약 encode 처리한 주소를 다시 원래 형태로 돌리고 싶다면 **decode**하면 된다

- **decodeURI()** : 이스케이프 처리를 한 주소를 다시 원래 형태로 반환

```jsx
const decoded = decodeURI(encoded);
console.log(decoded); //https://네이버.com
```

→ 다시 원래의 코드로 돌아간 것을 확인할 수 있다

**encode와  decode는 프론트앤드와 백앤드에서 서로 통신을 할 때 유용하게 쓰일 수 있는 전역객체이다**

전체 URL이 아니라 **부분적인 것**만 이스케이프 처리를 하고 싶다면 **Component**를 이용해야 한다

- **encodeURIComponent()** : 부분 url만 이스케이프 처리

```jsx
const part = '네이버.com';
console.log(encodeURIComponent(part)); //%EB%84%A4%EC%9D%B4%EB%B2%84.com
```

## boolean function

boolean 원시타입을 전역으로 만들어주는 객체가 있다.

그것은 바로  Boolean이다!

```jsx
const isTrue = true; 
const isTrue = new Boolean(true);
```

- const isTrue = true;

→ 바로 할당하여 사용

- const isTrue = new Boolean(true);

→ 전역객체를 이용하여 사용해도 되지만 객체를 사용하면 더 메모리를 차지하기 때문에 첫 번째 방법(바로 할당)을 추천함

**Boolean에는 객체들이 다양한 기능을 하지는 않는다**

- **valueof()** : boolean의 value를 출력

```jsx
const isTrue = true;

console.log(isTrue.valueOf()); //true
```

+) **그렇다면 데이터 중에서  truthy, falshy가 되는 값들은 무엇이 있을까?**

✔️ **falshy** : 0, -0, “”, null, undefined, NaN

✔️ **truthy** :  1, -1, 문자열(’0’, ‘false), 배열(텅 빈 배열도 포함), 오브젝트(텅 빈 오브젝트도 포함)

→ 0과 false도 문자열로 감싸져있다면 true

- **Boolean()** : boolean  객체 생성
- **isTrue()** : 식이 되는지 여부를 확인
<br>
<br>

## 숫자 함수 number function

- Number() : 생성자를 통해 넘버 오브젝트를 만들어냄

```jsx
const num1 = 123;
const num2 = new Number(123);

console.log(num1); //123
console.log(num2); //[Number: 123]
console.log(typeof num1); //number
console.log(typeof num2); //object
```

### static properties

클래스를 이용해서 즉, 객체를 일일히 만들지 않아도 클래스만으로도 접근이 가능함

- Number.EPSILON : 0과 1 사이에 나타낼 수 있는 가장 작은 숫자

```jsx
console.log(Number.EPSILON); //2.220446049250313e-16
```

**이해가 어려우니 아래의 예를 참고하자**

```jsx
if(Number.EPSILON > 0 && Number.EPSILON < 1) {
  console.log(Number.EPSILON); //2.220446049250313e-16
}
```

→ Number.EPSILON이 0보다는 크고 1보다는 작을 때 Number.EPSILON 값을 출력하라고 if문을 설정하였는데 true기 때문에 값이 출력된 것을 확인할 수 있음

즉, Number.EPSILON은 0과 1 사이에서 나타날 수 있는 가장 작은 숫자이다
<br>
<br>

**자바스크립트에서는 아래의 식을 어떻게 계산할까?**

```jsx
const num = 0.1 + 0.2 - 0.2;
console.log(num); //0.10000000000000003
```

→ 일반 수학으로 생각하면 0.1이 나와야 하지만 우리가 10진수로 계산하지만 자바스크립트에서는 이 10진수를 각각 2진수로 변환을 해서 계산을 하고 다시 10진수로 변환하여 값을 출력하는 과정에서 정확하게 부동소숫점까지 계산하지 않기 때문에 작은 오차가 생김
<br>
<br>

### 이러한 작은 오차를 나타내는 것이 EPSILON이다

```jsx
function isEqual(original, expected) {
  return original === expected;
}

console.log(isEqual(1, 1)); //true
console.log(isEqual(0.1, 0.1)); //true
console.log(isEqual(num, 0.1)); //false 
```

→ 왜냐하면 num은 0.10000000000000003이라고 출력이 되었기 때문이다

이 0.00000000000000003 작은 미세한 차이를 똑같다고 간주하고 싶다면

```jsx
function isEqual(original, expected) {
  return original - expected < Number.EPSILON;
}
```

→ 원래 값에서 예상한 값을 뺀 값이 EPSILON보다 작다면 값을 출력해라 라고 함수를 만들어서 확인을 하면 true가 출력되는 것을 볼 수 있다

- Number.MAX_VALUE : 표현 가능한 가장 큰 양수

```jsx
console.log(Number.MAX_VALUE); //7976931348623157e+308
```

→ e+308는 10의 308승이라는 뜻
<br>
- Number.MIN_VALUE : 표현 가능한 가장 작은 양수

```jsx
console.log(Number.MIN_VALUE); *//5e-324*
```
<br>

- Number.MAX_SAFE_INTEGER : 정수에서 나타낼 수 있는 가장 최고의 값 2^53 -1

```jsx
console.log(Number.MAX_SAFE_INTEGER); //9007199254740991
```
<br>

- Number.MIN_SAFE_INREGER : 정수에서 나타낼 수 있는 가장 최소의 값 -(2^53-1)

```jsx
console.log(Number.MIN_SAFE_INTEGER); //-9007199254740991
```
<br>

- Number.NaN : 숫자가 아님을 나타내는 특별한 값

```jsx
console.log(Number.NaN); //NaN
```
<br>

- Number.NEGATIVE_INFINITY : 음의 무한대를 나타내는 특수한 값, 넘칠 시에는 반환

```jsx
console.log(Number.NEGATIVE_INFINITY); //-Infinity
```
<br>

- Number.POSITIVE_INFINITY : 양의 무한대를 나타내는 특수한 값, 넘칠 시에는 반환

```jsx
console.log(Number.POSITIVE_INFINITY); //Infinity
```
<br>

- Number.prototype : 넘버 객체에 속성을 추가할 수 있음
<br>
<br>

### static methods

클래스에서 접근이 가능한 함수

- Number.isNaN() : 숫자인지 아닌지 확인
- Number.isFinite() : 유한한지 확인
- Number.isInteger() : 정수인지 확인
- Number.isSafeInteger() : 안전한 정수인지 확인
- Number.parseFloat(string) : 소수점을 실수로 변환
- Number.parseInt : 정수 변환
<br>
<br>

### instance methods

넘버라는 객체를 만들어서 접근 가능한 함수

- Number.prototype.toExponential(fractionDigits) : 매우 크거나 작은 숫자를 표기할 때 사용, 10의 n승으로 표기

```jsx
const num3 = 102;
console.log(num3.toExponential()); //1.02e+2
```

→ e+2는 10의 2승, 즉 1.02를 10의 2승으로 곱하는 것 다시 말해 1.02를 100으로 곱하니 102가 된다
<br>

- Number.prototype.toFixed : 반올림하여 문자열로 반환

```jsx
const num4 = 1234.12;
console.log(num4.toFixed()); //1234
```
<br>

- Number.prototype.toLocaleString() : 이 숫자를 해당 언어 방식으로 표현된 문자열을 반환

```jsx
console.log(num4.toLocaleString('ar-EG')); //١٬٢٣٤٫١٢
```

→ 아랍어로 숫자가 변환된 것을 확인할 수 있음
<br>

- Number.prototype.toPrecision() : 원하는 자릿수까지 유효하도록 반올림

```jsx
console.log(num4.toPrecision(5)); //1234.1
console.log(num4.toPrecision(2)); //1.2e+3
```

→ 입력한 숫자 자릿수보다 더 적게 설정할 경우 지수 표기법으로 변환하여 출력

즉, 전체 자릿수 표기가 안 될 때는 지수 표기법을 활용하여 반환
<br>
<br>

- Number.prototype.toString() : 숫자를 문자열을 반환

```jsx
const num4 = 1234.12;
console.log(num4.toString()); //1234.12
```

- Number.prototype.valueOf() : 명시된 객체의 원시 값을 반환
<br>
<br>

### 그렇다면 이 함수들은 언제 사용할까?

```jsx
if(num1 === Number.NaN) {};
if(Number.isNaN) {};
```

→ 이 숫자가 숫자인지 아닌지 확인하여 코드를 짜야 할 때 사용 가능

```jsx
if(num1 < Number.MAX_VALUE) {};
```

→ 이 숫자가 표현 가능한 정수 안에 있는지 없는지 확인 시 
<br>
<br>

## 수학 관련 함수들 Math

Math는 static properties와 methods를 많이 가지고 있기 때문에 우리가 직접 Math 객체를 만들어 낼 일은 거의 없다

- Math.E : 오일러의 상수, 자연로그의 밑

```jsx
console.log(Math.E); //2.718281828459045
```
<br>

- Math.PI : 원주율 파이값

```jsx
console.log(Math.PI); //3.141592653589793
```
<br>

- Math.abs() : 절대값 출력

```jsx
console.log(Math.abs(-10)); //10
```
<br>

- Math.ceil() : 소수점 이하를 무조건 올림

```jsx
console.log(Math.ceil(1.4)); //2
```
<br>

- Math.floor() :  소수점 이하를 무조건 내림

```jsx
console.log(Math.floor(1.4)); //1
```
<br>

- Math.round() : 소수점 이하를 반올림

```jsx
console.log(Math.round(1.4)); //1
console.log(Math.round(1.7)); //2
```
<br>

- Math.trunc() : 소수점에서 정수만 반환

```jsx
console.log(Math.trunc(1.532)); //1
```
<br>

- Math.max() / Math.min() : 최대 최소값 찾기

```jsx
console.log(Math.max(1, 3, 5)); //5
console.log(Math.min(1, 3, 5)); //1
```
<br>

- Math.pow() : 거듭제곱

```jsx
console.log(3 ** 2); //9
console.log(Math.pow(3, 2)); //9
```

→ 연산자를 이용해 출력해도 동일한 값이 출력되는 것을 확인할 수 있음
<br>

- Math.sqrt(): 제곱근

```jsx
console.log(Math.sqrt(9)); //3
```
<br>

- Math.random() : 랜덤 숫자 출력

```jsx
console.log(Math.random()); //0.015440231833489637
```

→ 0부터 1사이에 랜덤한 숫자를 출력할 때마다 새롭게 나타냄
<br>
<br>

**만약 1부터 10까지 랜덤한 숫자를 찾고 싶다면**

```jsx
console.log(Math.floor(Math.random()*10 + 1));
```

→ round를 사용해도 상관없음
<br>
<br>

## 문자열 함수들 string function

알고리즘에서도 많이 출제가 되고 코딩을 할 때 정말 많이 사용함

```jsx
const textObj = new String('hello world');
const text = 'hello world'
console.log(textObj); //[String: 'hello world']
console.log(text); //hello world
```

```jsx
console.log(text[0]); //h
```

→ text의 0번째 인덱스를 출력해보니 h가 출력되는 것을 볼 수 있다

즉, 문자열은 문자 하나하나가 모인 집합소라는 것을 알 수 있다

<br>

- text.charAt() : 문자열의 인덱스 접근

```jsx
console.log(text.charAt(0)); //h
console.log(text.charAt(1)); //e
console.log(text.charAt(2)); //l
```

→ 함수로 접근하는 것(text.charAt(0)) 배열의 인덱스로 접근하는 것(text[0]과 똑같은 값이 출력된다

<br>

- text.length : 문자열의 길이

```jsx
console.log(text.length);//11
```
<br>

- text.indexOf() : 텍스트의 인덱스 번호를 확인하는 법

```jsx
console.log(text.indexOf('l')); //2
```

→ indexOf는 처음부터 시작해서 찾으면 그 위치를 반환한다

만약 뒤에서부터 찾고 싶다면 lastIndexOf를 이용해야 한다

<br>

- text.lastIndexOf() : 뒤에서부터 인덱스 번호 확인

```jsx
console.log(text.lastIndexOf('l')); //9
```
<br>

- text.includes() : 해당 문자열에 포함하고 있는지 여부 확인

```jsx
console.log(text.includes('tx')); //false
console.log(text.includes('H')); //false
console.log(text.includes('h')); //true
```

→ 대소문자를 구분하여 true / false로 반환한다

<br>

- text.startsWith() : 해당 문자열로 시작하는지 여부 확인

```jsx
console.log(text.startsWith('h')); //true
console.log(text.startsWith('He')); //false
```

→ 대소문자를 구분하여 true / false로 반환한다

<br>

- text.endsWith() : 해당 문자열로 끝나는지 여부 확인

```jsx
console.log(text.endsWith('!')); //false
console.log(text.endsWith('ld')); //true
```

→ 대소문자를 구분하여 true / false로 반환한다

<br>

- text.toUpperCase() : 문자열을 모두 대문자로 반환

```jsx
console.log(text.toUpperCase()); //HELLO WORLD
```
<br>

- text.toLowerCase() :  문자열을 모두 소문자로 반환

```jsx
console.log(text.toLowerCase()); //hello world
```
<br>

- text.substring(0, 2)) : 시작하는 위치 끝나는 위치(끝나는 위치 이전까지) 부분적으로 출력

```jsx
console.log(text.substring(0, 2)); //he
```
<br>

- text.slice() : 지정한 인덱스 번호까지 문자열을 삭제하여 나머지를 출력

```jsx
console.log(text.slice(2)); //llo world
console.log(text.slice(-2)); //ld
```

→ 마이너스를 하면 뒤에서부터 2번째 있는 것까지 잘라냄

<br>

- trim() : 문자열의 공백 삭제

```jsx
const space = '         space     '
console.log(space.trim()); //space
```
<br>

- split() : 입력한 문자열을 기준으로 잘라서 배열로 출력

```jsx
const longText = 'Get to the point';
console.log(longText.split(' ')); //[ 'Get', 'to', 'the', 'point' ]
```

→ 공백 잘라내기 가능

```jsx
console.log(longText.split(' ', 2)); //[ 'Get', 'to']
```

→ 만약 끊어낸 것 중에 2가지만 가지고 싶다면 이렇게도 사용 가능
<br>
<br>

## 날짜 관련 함수들 date function

```jsx
console.log(new Date()); //2022-06-14T14:52:03.805Z
```

→ 우리가 새로운 date객체를 만들어내면 현재 시간이 출력된다

```jsx
console.log(new Date('jun 5, 2022')); //2022-06-04T15:00:00.000Z
console.log(new Date('2022-12-17T03:24:00')); //2022-12-16T18:24:00.000Z
```

→ 이렇게 시간을 지정하면 지정한 해당 시간이 출력되는 것을 확인할 수 있다

<br>

- Date.now() : 현재 시간 출력

```jsx
console.log(Date.now()); //1655218461261
```

→  Date는 UTC(협정 세계 시간, 1970년도 1월 1일 UTC 자정과의 시간 차이를 밀리초 단위로 표기한다)

<br>

- Date.parse() : 특정한 문자열의 날짜를 입력하면 밀리초 단위로 출력

```jsx
console.log(Date.parse('2022-12-17T03:24:00')); //1671215040000
```
<br>

- set과 get을 이용한 날짜

```jsx
const now = new Date();
now.setFullYear(2023);
now.setMonth(10); // 인덱스 번호를 써서 1월은 0이다
console.log(now.getFullYear());
console.log(now.getDate());
console.log(now.getDay()); // 0은 일요일 1은 월요일 ... 6은 토요일
console.log(now.getTime());
```

→ 인덱스 번호를 이용하여 첫번째는 무조건 0이다

<br>

- toString() : 전체적인 시간이 반환되는 것을 확인할 수 있다

```jsx
console.log(now.toString()); 
//Wed Nov 15 2023 00:01:36 GMT+0900 (대한민국 표준시)
```
<br>

- toDateString() : 날짜만 출력

```jsx
console.log(now.toDateString()); //Wed Nov 15 2023
```
<br>

- toTimeString() : 시간만 출력

```jsx
console.log(now.toTimeString()); //00:03:12 GMT+0900 (대한민국 표준시)
```
<br>

- toISOString() :  날짜를 ISO 8601 형식으로 출력

```jsx
console.log(now.toISOString()); //2023-11-14T15:03:12.597Z
```
<br>

- toLocaleString() : 지정한 나라의 형식으로 날짜를 출력

```jsx
console.log(now.toLocaleString('en-US')); //211/15/2023, 12:04:59 AM
```

[Quiz](https://www.notion.so/Quiz-6f53ccdfa80646c98b90cec6250d4416)