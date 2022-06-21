# 배열

## 자료 구조 Data Structure

밀접하게 관련된 상태나 행동을 묶어서 객체로 표현할 수 있었다

예를 들어 사람에게는 이름, 나이, 성별, 말한다, 먹는다 등의 형태로 묶어놓은 템플릿을 클래스나 생성자 함수를 이용하여 만들 수 있었다

그 템플릿으로 철수나 영희 같은 인스턴스 오브젝트(클래스를 통해 만든 것)들로 하나하나 만들어낼 수 있었다

이렇게 만들어둔 것을 특정한 자료 구조에 담아둘 수 있다

**배열**은 들어온 순서대로 **0부터 시작**하는 순서표, 즉 인덱스가 있기 때문에 이렇게 순서대로 데이터를 담고 처리할 수 있다

또한 배열은 메모리상에 **인덱스 번호 순서대로** 서로 붙어있다

배열을 만들 때는 다양한 타입의 데이터를 담아둘 수는 있지만 그 방법은 추천하지 않고 **동일한 타입의 데이터만 담는 것을 추천한다**

<br>
<br>

### 배열 생성 방법

배열을 만드는 방법은 다양하다. 

1. **생성자 함수를 이용한 생성**

```jsx
let array = new Array(2);
console.log(array); //[ <2 empty items> ]
```

→ 이처럼 클래스 뒤에 배열의 크기를 지정하면 2개만 들어갈 수 있는 배열을 생성할 수 있다

<br>

```jsx
array = new Array(1, 2, 3);
console.log(array); //[ 1, 2, 3 ]
```

→ 혹은 이렇게 내가 넣고 싶은 데이터를 입력하여 생성할 수 있다
<br>
<br>

1. **static 함수를 이용한 생성**

```jsx
array = Array.of(1, 2)
console.log(array); //[1, 2]
```

→ 원하는 갯수만큼 무한정 생성 가능
<br>
<br>

2. **배열 리터럴을 이용한 생성**

```jsx
const anotherArray = [1, 2, 3, 4];
console.log(anotherArray); //[ 1, 2, 3, 4 ]
```

→ 대괄호를 이용하여 생성 가능

<br>
<br>

3. **기존의 배열, 순회 가능한 데이터을 이용하여 생성**

```jsx
array = Array.from(anotherArray)
console.log(array); //[ 1, 2, 3, 4 ]
```

→ Array.from()으로 기존의 배열을 가져와서 생성도 가능

```jsx
array = Array.from('anotherArray')
console.log(array); //[
  'a', 'n', 'o', 't',
  'h', 'e', 'r', 'A',
  'r', 'r', 'a', 'y'
]
```

→ Array.from() 순회 가능한 데이터를 가져와서 생성이 가능하므로 string 문자열도 배열로 가져와서 생성 가능하다
<br>
<br>

### 배열을 정리해보자면

일반적으로 배열은 동일한 메모리 크기를 가지며, 연속적으로 이어져 있어야 한다

하지만 자바스크립트에서의 배열은 연속적으로 이어져 있지 않고, 오브젝트와 유사하다

(사실 오브젝트와 똑같다고 봐도 된다)

자바스크립트의 배열은 일반적인 배열의 동작(다른 프로그래밍 언어)을 흉내낸 특수한 객체이다

이걸 보완하기 위해서 타입이 정해져 있는 타입 배열이 있다(Typed Collection)
<br>
<br>

1. **오브젝트를 이용한 생성**

```jsx
array = Array.from({
  0: '안',
  1: '녕',
  length: 2,
})

console.log(array); //[ '안', '녕' ]
```

→ 오브젝트에는 인덱스 key가 있고 length라는 key에 몇 개의 아이템이 들어있는지민 나타내면 배열로 만들어낼 수 있다
<br>
<br>

### 배열 아이템을 참조하는 방법

```jsx
const fruits = ['🍌', '🍎', '🍇', '🍑'];
console.log(fruits[0]); //🍌
```

→ 배열의 인덱스 번호를 지정해주면 참조가 가능 / 배열은 항상 0부터 시작한다

이처럼 하나하나 출력을 직접 하면 나중에 데이터가 추가 되었을 때 문제가 생기니 for문을 이용하여 출력하는 것이 좋다!

```jsx
for(let i = 0; i < fruits.length; i++) {
  console.log(fruits[i]); //🍌 🍎 🍇 🍑
}
```

```jsx
console.log(fruits.length); //4
```

→ 배열 안에 몇 개의 아이템이 있는지 확인 가능
<br>
<br>

## ❌배열을 사용할 때 하면 안 되는 것❌

### 배열을 추가하고 삭제하는 법 - 별로 좋지 않은 방식

- **배열 추가**

```jsx
fruits[4] = '🍓';
console.log(fruits); //[ '🍌', '🍎', '🍇', '🍑', '🍓' ]
```

**위 방법이 왜 좋지 않을까?**

나는 이 배열의 가장 마지막에 데이터를 추가하고 싶었는데 이미 들어 있는 아이템에 추가를 해버리면 덮어씌워지거나 실수로 멀리 떨어진 인덱스 번호에 추가하면 비어있는 아이템이 출력이 된다.

**만약 정말로 이렇게 마지막에 넣어야 한다면**

```jsx
fruits[fruits.length] = '🍉';
console.log(fruits); //[ '🍌', '🍎', '🍇', '🍑', '🍓', '🍉' ]
```

이렇게 **배열의 길이에 추가하게 되면 제일 마지막에 추가되는 것을 확인할 수 있다.**
<br>
<br>

- **배열 삭제**

```jsx
delete fruits[1];
console.log(fruits); //[ '🍌', <1 empty item>, '🍇', '🍑', '🍓', '🍉' ]
```

**위 방법이 왜 좋지 않을까?**

아이템은 삭제하였으나 배열의 길이는 그대로여서 <1 empty item>이라고 출력이 되기 때문

완전히 삭제된 것이 아니라 그 자리는 텅텅 비어 있음
<br>
<br>

## 배열에 사용할 수 있는 함수

배열 자체를 변경하는지, 새로운 배열을 반환하는지에 대해 포인트를 두고 알아보자

- **특정한 오브젝트가 배열인지 여부 체크 -** isArray()

```jsx
const fruits = ['🍌', '🍎', '🍇', '🍑'];
console.log(Array.isArray(fruits)); //true
```

→ Array에 있는 static 함수를 이용하여 알아보는 방법 true / false 중에 출력
<br>
<br>

- **특정한 아이템의 위치 찾기 -** indexOf()

```jsx
console.log(fruits.indexOf('🍎')); //1
```

- 배열 안에 특정한 아이템이 있는지 체크 - includes()

```jsx
console.log(fruits.includes('🍎')); true
```

- 제일 뒤에 추가 - push()

```jsx
fruits.push('🍏');  
```

→ 배열 자체를 수정(업데이트) 하나를 전달해도 되고 여러 개를 전달해도 모두 가능 추가하고 난 길이를 리턴해줌

**그렇다면 길이를 한번 확인해보자**

```jsx
let length = fruits.push('🍏');
console.log(fruits);  // [ '🍌', '🍎', '🍇', '🍑', '🍏' ]
console.log(length); //5
```

- 제일 앞에 추가 - unshift()

```jsx
fruits.unshift('🥑'); 
```

→ 배열 자체를 수정(업데이트)하고 앞에 추가하고 난 길이를 리턴해줌

```jsx
length = fruits.unshift('🥑');
console.log(fruits); // [ '🥑', '🍌', '🍎', '🍇', '🍑',  '🍏' ]
console.log(length); //6
```

- 제일 뒤 제거 - pop()

```jsx
fruits.pop();
```

→ 배열 자체를 수정하고 제거된 아이템을 리턴해줌

```jsx
let lastItem = fruits.pop();  
console.log(fruits);  //  [ '🥑', '🍌', '🍎', '🍇', '🍑' ]
console.log(lastItem); // 🍏
```

- 제일 앞 제거 - shift()

```jsx
fruits.shift()
```

→ 배열 자체를 수정해줌

```jsx
lastItem = fruits.shift();  배열 자체를 수정
console.log(fruits); //  [ '🍌', '🍎', '🍇', '🍑' ]
console.log(lastItem); // 🥑
```

- 중간에 추가 또는 삭제 - splice()

```jsx
fruits.splice()
```

→ 삭제 추가 모두 가능 배열 자체를 수정

```jsx
const deleted = fruits.splice(1, 1);  
// -> 1부터 시작해서 하나를 삭제할거야

console.log(fruits); // [ '🍌', '🍇', '🍑' ]
console.log(deleted); [ '🍎' ]
```

```jsx
fruits.splice(1, 0, '🥦', '🥕')
// -> 하나도 삭제를 하지 않고 2가지를 추가할거야
console.log(fruits); //[ '🍌', '🥦', '🥕', '🍇', '🍑' ]
// 첫번째 인덱스에 브로콜리와 당근이 추가되는 것을 확인할 수 있음
```

- 잘라진 새로운 배열들을 만드는 api - slice()

```jsx
let newArr = fruits.slice(0, 2); 
```

→ 시작하는 인덱스, 잘리기 전

```jsx
console.log(newArr); [ '🍌', '🥦' ]
```

→ 0번, 1번이 잘려서 새로운 배열로 만들어진 것을 확인할 수 있음

```jsx
console.log(fruits); [ '🍌', '🥦', '🥕', '🍇', '🍑' ]
```

**slice(start?: number, end?: number): T[];**

→ 물음표는 옵션, 아무것도 지정하지 않고 빈 괄호만 있다면 0번째부터 전체를 잘라내어 새로운 배열을 만드는 것이기 때문에 전체를 slice한다

```jsx
newArr = fruits.slice();
console.log(newArr); [ '🍌', '🥦', '🥕', '🍇', '🍑' ]
```

→ fruit의 배열 전체가 출력된 것을 확인할 수 있다

```jsx
newArr = fruits.slice(1);  
```

→ 1번째부터 끝까지 slice

```jsx
console.log(newArr); [ '🥦', '🥕', '🍇', '🍑' ]
newArr = fruits.slice(-1); //제일 뒤부터 한칸 당겨진 위치가 slice된다
console.log(newArr); [ '🍑' ]
```

→ 가장 마지막에 있던 복숭아가 출력되는 것을 확인할 수 있다

- 여러 개의 배열을 붙여 주는 api - concat()

```jsx
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const arr3 = arr1.concat(arr2)
console.log(arr1); //[1, 2, 3]
console.log(arr2); //[4, 5, 6]
console.log(arr3); //[1, 2, 3, 4, 5, 6]
```

→ arr1과 arr2를 병합한 새로운 배열을 출력하는 것을 확인할 수 있다.

- 배열의 순서를 거꾸로 해주는 api - reverse()

```jsx
const arr4 = arr3.reverse();
console.log(arr4); [ 6, 5, 4, 3, 2, 1 ]
```

- 중첩된 배열을 하나의 배열로 쫙 펴기 - flat()

```jsx
let arr = [
  [1, 2, 3], 
  [4, [5, 6]]
]
console.log(arr); [ [ 1, 2, 3 ], [ 4, [ 5, 6 ] ] ]
console.log(arr.flat()); [ 1, 2, 3, 4, [ 5, 6 ] ]
```

→ 베열 안에 중첩된 배열이 있다면 하나씩 풀어서 float하게 만들어줌

단, 아무것도 지정을 하지 않으면 1단계까지만 배열을 풀어주기 때문에 배열 안에 또다른 배열이 있다면 즉, [5, 6]은 풀어주지 못함

**만약 2단계까지 배열을 풀고 싶다면 float()안에 단계를 지정해주면 된다**

```jsx
console.log(arr.flat(2));[ 1, 2, 3, 4, 5, 6 ]
```

→ float은 기존의 배열을 수정하지 않고 새로운 배열을 반환한다

- 특정한 값으로 배열을 채우기 - fill()

```jsx
arr.fill(0); // 배열 자체를 수정
console.log(arr); [ 0, 0, 0, 0, 0, 0 ]
```

→ 지정한 데이터(0)로 배열이 채워진 것을 확인할 수 있다

**fill(value: T, start?: number, end?: number): *this*;**

→ 어떤 것으로 채울 것인지, 어떤 인덱스부터 어디까지 채울 것인지 지정 가능

```jsx
arr.fill('s', 1, 3); //end index는 포함하지 않고 바로 직전까지만 적용
console.log(arr); //  [0, s, s, 0, 0, 0]
arr.fill('a', 1);  // 시작 인덱스만 명시해주면 시작 인덱스부터 끝까지
console.log(arr); // [ 0, 'a', 'a', 'a', 'a', 'a' ]
```

- 배열을 문자열로 합치기 - join()

```jsx
let text = arr.join();
console.log(text); 0,a,a,a,a,a
```

→ 자동으로 콤마를 이용한 문자열로 반환된 것을 확인할 수 있다

**만약 콤마 말고 다른 것으로 구분하여 반환하고 싶다면 아래의 예를 보자**

```jsx
text = arr.join(' | ');
console.log(text); 0 | a | a | a | a | a
```

→ 지정한 것으로 구분되어 출력된 것을 확인할 수 있음

### 이것을 호출하면 새로운 배열로 반환이 되는지 기존의 배열을 수정하는 것인지 잘 생각하고 구분하여 사용해야 한다!

**그렇다면 추가로 피자의 가격이 올라 수정되었다고 하자**

```jsx
pizza.price = 4;
console.log('store1', store1); *//store1 [ { name: '🍕', price: 4 }, { name: '🍲', price: 3 } ]*
console.log('store2', store2); *//store2 [ name: '🍕', price: 4 }, { name: '🍲', price: 3 },{ name: '🍣', price: 1 }]*
```

→ 동일하게 가격이 인상되어 반영된 것을 확인할 수 있음

### 이것이 바로 shadow copy라고 하는 것이다

배열을 만들 때, 배열이 만들어지고 아이템이 2개 들어가있다(피자와 라면)

이 배열을 만들 때 오브젝트가 새롭게 만들어지는 것이 아니라 오브젝트가 만들어진 주소를 가리키고 있다 만약 피자의 주소는 ox11이라고 하고 라면의 주소는 ox12를 가지고 있다고 하자

이 배열을 이용하여 store2를 만들었으니 새로운 배열을 만드는 것이 아닌 해당 피자와 라면의 주소를 가지고 있는 것이다

그러므로 피자의 값이 인상이 되어 값을 재할당해준다고 해서 배열에 들어있는 오브젝트들이 새롭게 만들어진 오브젝트가 아니라 그 주소를 가지고 있기 때문에 재할당된 사항이 반영되어 출력이 되는 것이다

**즉, 앝은 복사 shallow copy 객체는 메모리 주소 전달**

자바스크립트에서 복사할 때는 항상 얕은 복사가 이루어진다

그러므로 Array,from, concat, slice, spread, Object.assign 등을 이용하여 새로운 배열을 만들어낼 때에는 새로운 오브젝트가 만들어지는 것이 아니라 기존 오브젝트의 주소를 가져오므로 얖은 복사가 이루어진다

[퀴즈1](https://www.notion.so/1-a6a32a1421ab4c7d820f7182afd58eea)