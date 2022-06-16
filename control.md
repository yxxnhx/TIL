# 제어문

### 제어문이 왜 중요한가?

프로그램 혹은 어플리케이션에서는 한줄한줄 작성한 순차적으로 실행이 된다.

이 코드의 흐름을 제어하는 것을 제어문이라고 한다.

이런 제어문에는 조건문이라는 것이 있다.

if switch 특정한 조건일 때만 사용하거나

for while do-while로 반복적으로 수행할 수 있게 한다.
</br>
</br>
</br>

# 조건문 Conditional Statement

## if

- if(조건) { }
- if(조건) { } else { }
- if(조건1) { } else if(조건2) { } else { }

→ 이 세 가지 형태들로 if문을 사용할 수 있다.

아래의 예시를 보자

```jsx
let fruit = 'apple';
if(fruit === 'apple') {
    console.log('사과');
} // 사과
```

→ fruit의 값과  if문의 조건이 값과 형태가 모두 동일하므로 콘솔에 사과가 찍힌 것을 확인할 수 있다.
</br>
</br>

**그렇다면 fruit의 값을 orange로 변경하면 어떻게 될까?**

```jsx
let fruit = 'orange';
if(fruit === 'apple') {
    console.log('사과');
}
```

→ 아무것도 출력이 되지 않음을 확인할 수 있다.

여기에 else 문으로 그 조건이 아닐 시에 출력할 조건을 입력하면 아래와 같이 !!이 출력됨을 확인할 수 있다.

```jsx
let fruit = 'orange';
if(fruit === 'apple') {
    console.log('사과');
} else {
    console.log('!!');
} //!!
```

### 즉, if문은 조건이 맞을 때만 해당 코드가 실행이 되고 아닐 경우 그 다음으로 넘어가게 된다.
</br>

### +) if문 안의 조건은 true나 false로 평가되는 표현식이 들어가게 된다.

그러므로 아래의 조건을 참고해보자

```jsx
if() {
    console.log('출력되면 안 됨');
}
```

→ 출력이 되면 안 되기 때문에 괄호 안의 조건이 false여야 한다.

그러므로 괄호 안에는 0 / 텅빈 문자열 / false / null / undefined가 들어가면 해당 출력문이 조건에 맞지 않아 출력이 되지 않는다.

→ 그러므로 괄호 안의 조건이 true이면 출력이 된다는 뜻이다.

괄호 안에는 숫자 / 문자열 / true가 들어가면 true이므로 조건에 맞아 ‘출력되면 안 됨'이라는 문구가 출력됨을 확인할 수 있다.

### +) if문 안의 조건에는 연산자도 사용 가능하다

```jsx
if(2 > 1) {
    console.log('출력되면 안 됨');
}
```

→ 2가 1보다 큰 것이 true이므로 조건에 일치하여 ‘출력되면 안 됨'이라는 문구가 출력된다.

## 삼항 조건 연산자 Ternary Operator

**조건식 ? 참인 경우 : 거짓인 경우**

→ 왼쪽에 설정한 조건 식이 참이 경우 첫번째 표현식이 실행이 되고 거짓인 경우 두번째 표현식이 실행이 된다.

```jsx
let fruit = 'orange';
if(fruit === 'apple') {
    console.log('사과');
} else {
    console.log('앗'); // 앗
}
```

이와 같은 경우를 삼항 연산자로 좀더 간단하게 쓸 수 있다.

```jsx
fruit === 'apple' ? console.log('사과') : console.log('앗'); // dkt
```

→ if문과 삼항 연산자와 동일하게 출력이 되는 것을 확인할 수 있다.

```jsx
let emoji = fruit === 'apple' ? '🍎' : '🍊';
console.log(emoji);
```

→ 이와 같이 출력할 값을 바로 입력을 해도 출력이 가능하다.

[제어문 연습 퀴즈](https://www.notion.so/50acebe3b20943bda7fac94268523808)
</br>
</br>

## switch

switch와 if는 비슷하면서도 다르다.

if는 if else if else if else 등을 반복적으로 조건을 여러개 만들 수 있지만

switch는 정해진 범위 안의 값에 대해 **특정한 일을 해야 하는 경우 사용할 수 있다**.

### 예를 들면 월화수목금토일을 숫자로 나타내려고 한다고 하자.

→ 0: 월요일 1: 화요일 … 6: 일요일

```jsx
let day = 6;
let dayName;
if (day === 0) {
    dayName = '월요일';
} else if(day === 1) {
    dayName = '화요일';
} else if(day === 2) {
    dayName = '수요일';
} else if(day === 3) {
    dayName = '목요일';
} else if(day === 4) {
    dayName = '금요일';
} else if(day === 5) {
    dayName = '토요일';
} else {
    dayName = '일요일';
}
console.log(dayName);
```

→ if문을 사용하면 이와 같이 코드가 길어진다.
</br>
</br>
</br>

**switch(조건) {케이스}**

```jsx
switch(day) {
    case 0:
        dayName = '월요일';
        break;
    case 1:
        dayName = '화요일';
        break;

    case 2:
        dayName = '수요일';
        break;

    case 3:
        dayName = '목요일';
        break;

    case 4:
        dayName = '금요일';
        break;

    case 5:
        dayName = '토요일';
        break;

    case 6:
        dayName = '일요일';
        break;

}
```

→ 이렇게 케이스별로 정리하여서 출력해낼 수 있다.

**단, 케이스가 끝났을 때에는 break로 다음 케이스로 넘어가지 않게 break를 걸어줘야 한다.**

break를 걸지 않으면 계속 case를 넘어가서 가장 마지막의 case가 출력이 된다.

```jsx
let condition = 'okay'; // okay, good -> 좋음 bad -> 나쁨
let text;
switch(condition) {
    case 'okay':
    case 'good':
        text = '좋음';
        break;
    case 'bad':
        text = '나쁨';
        break;
}
console.log(text);
```

→ 이와 같이 case 여러 개가 동일한 하나의 코드를 수행할 시에는 break를 거는 것은 동일한 하나의 코드 다음에 break를 걸어야 한다.
</br>
</br>
</br>

### switch에는 if의 else와 같은 일을 하는 것이 있다

**→ default**

```jsx
switch(day) {
    case 0:
        dayName = '월요일';
        break;
    case 1:
        dayName = '화요일';
        break;

    case 2:
        dayName = '수요일';
        break;

    case 3:
        dayName = '목요일';
        break;

    case 4:
        dayName = '금요일';
        break;

    case 5:
        dayName = '토요일';
        break;

    case 6:
        dayName = '일요일';
        break;
        default:
            console.log('해당하는 요일이 없음');

}
```

→ 이렇게 어떠한 케이스도 맞지 않았을 경우 출력문을 default 다음에 입력해주면 if문의 else처럼 작동할 수 있다.

</br>
</br>
</br>
# 반복문 Loop Statement

## for

**for(변수 선언문; 조건식; 증감식) { }**

해당하는 조건이 맞을 때까지 {}의 코드 블럭을 실행한다.

### <실행 순서>

1. 변수선언문
2. 조건식의 값이 참이면 { } 코드블럭을 수행한다.
3. 증감식을 수행
4. 조건식이 거짓이 될 때까지 2, 3번을 반복한다.

```jsx
for(let i = 0; i<5; i++) {
    console.log(i); // 0, 1, 2, 3, 4
}
```

→ i가 5보다 작을 때까지 계속 증감하면서 코드 블럭을 수행하므로 0, 1, 2, 3, 4가 출력된 것을 확인할 수 있다.

보통은 변수 선언문 안에서 쓰이는 변수는 i로 많이 쓴다.

다른 단어로 대체해도 상관은 없으나 구문이 길어지므로 i를 많이 쓴다.

만약 i를 두 개씩 증감하고 싶다면 i = i+2를 쓰면 원하는대로 출력이 가능하다,.

### for 문 안에 또 다른 for문을 쓸 수 있다.

```jsx
for(let i = 0; i<5; i++) {
    for(let j = 0; j<5; j++) {
        console.log(i, j);
    }
}
```

→ 이와 같이 중첩시킬 수 있다.
</br>
</br>
</br>

### for문을 쓸 때 조건이 거짓이 되어 for문이 종료될 수 있게 해야 한다.

만약 거짓이 될 조건이 주어지지 않아 계속 뱅글뱅글 돌게 되는 것을 무한 루프라고 하는데 루프가 중지되지 않고 코드가 끝나지 않아 반응을 하지 않는 나쁜 상태가 된다.

```jsx
for(;;) {
}
```

→ 이와 같이 선언문, 조건식, 증감식 모든 것이 없어서 계속 돌아버리게 만든 것이다.

### 반복문 제어: continue, break

- break

```jsx
for(let i = 0; i <20; i++) {
    if(i === 10) {
        console.log('i가 드디어 10이 되었다');
        break;
    }
    console.log(i);
}
```

→ i가 10이 되었을 때 더이상 for문을 작동시키지 않고 제어하고 싶을 때 switch와 동일하게 break를 입력하면 i가 출력되지 않고 10까지 출력하다가 종료가 되는 것을 확인할 수 있다.

- continue

```jsx
for(let i = 0; i <20; i++) {
    if(i === 10) {
        continue;
        console.log('i가 드디어 10이 되었다');
        break;
    }
    console.log(i);
}
```

→ 반복문 안에서 continue를 만나게 되면 그 아래에 있는 코드를 무시하게 되고 바로 그 다음 반복으로 넘어가게 된다.

그래서 결국 1, 2, 3, 4, 5, 6, 7, 8, 9, 10이 출력된다는 것을 확인할 수 있다.

### 그러므로 이 조건이 맞다면 코드를 넘어가 혹은 이 조건이 맞다면 코드를 종료시키는 등의 반복문을 제어할 수 있다.
</br>
</br>
</br>

## while

while(조건) { }

조건이 false가 될 때까지 {} 무한정 코드를 반복 실행한다.

while은 따로 변수를 초기화하거나 증감하는 구문이 없으므로 외부에서 지정해줘야 한다.

```jsx
let num = 5;
while(num >= 0) {
    console.log(num);
    num--;
}
```

→ num이 0보다 작을 때까지 1씩 감소시켜서 0보다 작을 경우에는 false가 되므로 while문이 종료가 된다.
</br>
</br>

```jsx
let isActive = true;
let i = 0;
while(isActive) {
    console.log('아직 살아있다');
    if(i === 10) {
        break;
    }
		i++
}
```

→ while 안에 break와 if문을 넣어서 제어가 가능하다

→ while은 조건이 맞을 때에만 실행이 된다.
</br>
</br>
</br>

## do while

do { } while (조건);

→ 일단 먼저 실행하고 나서 조건을 검사한다.

```jsx
let isActive = false;
let i = 0;
while(isActive) {
    console.log('아직 살아있다');
    if(i === 10) {
        break;
    }
    i++
}

do {
    console.log('아직도 살아있다'); // 아직도 살아있다
} while(isActive)
```

→ isActive가 false이므로 while 조건에 맞지 않아 while문이 실행되지 않은 것을 확인할 수 있는데

do while문은 일단 먼저 실행 후에 조건을 감시하기 때문에 아직도 살아있다는 것이 출력됨을 확인할 수 있다.

### 그러므로 조건에 맞을 때만 사용하고 싶다면 while, 꼭 한번은 실행해야 한다면 do while을 사용하여 쓰면 된다
</br>
</br>
</br>

## 제어문에서 자주 쓰이는 연산자
</br>

### 논리 연산자 Logical Operator

- **&&** 그리고 - and 연산자

```jsx
let num = 8;
if(num >= 0 && num < 9) {
    console.log('우왕');
}
```

→ num이 0보다 크거나 같고, 9보다 작을 때 해당 구문을 출력할 수 있게 한다.
</br>
</br>

- **||**  또는 - or 연산자

```jsx
let num = 8;
if(num >= 0 || num > 20) {
    console.log('우왕');
}
```

→ num이 0보다 크거나 같은 것은 true이지만 num이 20보다 큰 것은 false이다.

그러나 논리연산자로 or 연산자를 사용하였으므로 둘 중의 하나라도 true이면 해당 구문이 실행이 된다.

- **!** 부정 연산자 (단항 연산자에서 온 것)

```jsx
let num = 8;
if(!(num >= 0 || num > 20)) {
    console.log('우왕');
}
```

→ 조건문을 !으로 전체 부정을 하였으므로 num이 0보다 크지 않거나 num이 20보다 크지 않은 경우 구문을 실행하라고 하였으나 num은 8이므로 실행되지 않는다.

```jsx
if(num != 9) {
    console.log('잉');
}
```

→ num이 9가 아니라면 아래 구문을 실행해라
</br>
</br>

- **!!** boolean 값으로 변환(단항 연산자 응용버전)

```jsx
console.log(!'text');
console.log(!!'text');
```

→ 문자열은 true인데 부정하였으므로 false, 두번 부정하였으므로 true이다
</br>
</br>

### 그렇다면 아래의 연산자 구문을 확인해보자

```jsx
console.log(true && true); // true
console.log(true && false); // false
console.log(false && true); // false
console.log(false && false); // false

console.log(true || true); // true
console.log(true || false); // true
console.log(false || true); // true
console.log(false || false); // false
```

→ && 연산자는 둘 다 true여야 하므로 true && true 외에는 모두 false이다.

→ || 연산자는 둘 중 하나만 true여도 true이므로 마지막 false || false 외에는 모두 true이다