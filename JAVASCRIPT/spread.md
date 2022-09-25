## spread 연산자, 전개구문

모든 Iterable은 spread 될 수 있다.

순회가 가능한 모든 것들은 촤르르륵 펼쳐 질 수 있다.

ex) func( …iterable) ,[…iterable], { …obj}

이러한 전개구문은 EcmaScript 2018에 추가가 되었다.

<br />

만약에 아래와 같이 add라는 함수가 있고 nums와 같이 숫자 배열이 있다고 하자.

```jsx
function add(a, b, c) {
  return a + b + c;
}

const nums = [1, 2, 3];
```

<br />

**그런데 a에는 1, b에는 2, c에는 3을 각각 넣어 add라는 함수를 실행시키고 싶다면 어떻게 해야 할까?**

```jsx
console.log(add(nums[0], nums[1], nums[2])); //6
```

→ 이와 같이 a에는 nums의 0번째, b에는 1번째, c에는 2번째를 넣어주면 된다.

<br />

그러나 이렇게 쭉 풀어서 쓰면 코드가 너무 길어지고 더럽게 된다.

**이럴 때 spread 연산자를 이용하여 풀이하면 조금 더 쉽고 간편하게 이용이 가능하다!**

```jsx
console.log(add(...nums)); //6
```

→ 위와 동일하게 6이 출력되는 것을 확인할 수 있다

<br />

**이를 이용해서 rest parameters를 사용할 수도 있다.**

```jsx
function sum(first, second, ...nums) {
  console.log(nums); // [0, 1, 2, 4]
}
sum(1, 2, 0, 1, 2, 4);
```

→ 만약에 sum의 첫번째, 두번째 값은 아는데 그 이후의 값을 모를 때에는 위와 같이 …nums를 이용하여 나머지 값을 받아올 수 있다.

<br />

**배열에서 두 개의 배열을 더할 때 concat을 이용하였는데 이것을 rest parameters를 이용하여 조금 더 쉽게 사용이 가능하다!**

```jsx
const fruits1 = ["🍏", "🥝"];
const fruits2 = ["🍓", "🍌"];
let arr = fruits1.concat(fruits2);
console.log(arr); // [ '🍏', '🥝', '🍓', '🍌' ]
```

```jsx
arr = [...fruits1, ...fruits2];
console.log(arr); // [ '🍏', '🥝', '🍓', '🍌' ]
```

→ 이렇게 rest parameters를 이용하면 concat과 동일한 값이 출력되는 것을 확인할 수 있다!

rest parameters를 이용하면 배열 두 개를 합치면서 또 다른 값을 추가하는 것을 쉽게 해낼 수 있다!

```jsx
arr = [...fruits1, "🌱", ...fruits2];
console.log(arr); //[ '🍏', '🥝', '🌱', '🍓', '🍌' ]
```

→ 만약 concat을 활용하였다면 조금 더 복잡하게 식을 짜야했지만 rest parmeters는 그냥 값을 추가해주면 된다!

<br />

**Object에서도 사용이 가능하다**

```jsx
const yxxn = {
  name: "Yoonha",
  age: 20,
};
```

→ yxxn이라는 오브젝트에 직업을 더해주고 싶을 때 rest parameters를 이용해보자

<br />

```jsx
const update = {
  ...yxxn,
  job: "front-end engineer",
};
console.log(update); //{ name: 'Yoonha', age: 20, job: 'front-end engineer' }
```

→ update라는 오브젝트에 yxxn이라는 오브젝트를 불러서 촤르르르륵 풀어주고 거기에 job이라는 key값을 추가해주면 된다!

<br />

```jsx
console.log(yxxn); // { name: 'Yoonha', age: 20 }
```

→ update라는 새로운 배열을 만들었기 때문에 기존의 yxxn이라는 배열을 건들지 않은 것 또한 확인할 수 있다

<br />

**그러나 오브젝트는 shadow copy가 이루어진다는 것을 잊지 말자!**

```jsx
const yxxn = {
  name: "Yoonha",
  age: 20,
  home: {
    address: "home",
  },
};
const update = {
  ...yxxn,
  job: "front-end engineer",
};

yxxn.home.address = "school";

console.log(yxxn); //{ name: 'Yoonha', age: 20, home: { address: 'school' } }
console.log(update); //{
//	  name: 'Yoonha',
//	  age: 20,
//	  home: { address: 'school' },
//	  job: 'front-end engineer'
//	}
```
