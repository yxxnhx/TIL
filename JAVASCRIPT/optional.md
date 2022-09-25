## 옵셔널 체이닝 연산자 Optional Chaning Operator

ES11(ECMAScript 2020)

### ?.

```jsx
let item = { price: 1 };
const price = item && item.price;
console.log(price);
```

→ 이전에는 이렇게 && 연산자를 이용하여 item 값이 있으면 뒤의 조건인 item.price를 price에 할당하라고 선언을 해왔다.

**그렇다면 옵셔널 체이닝 연산자인 .?를 이용해보자**

```jsx
let item = { price: 1 };
const price = item?.price;
console.log(price);
```

→ item이 있니? 그럼 그 item의 price를 할당해줘 없으면 할당하지마

### 좀 더 유용하게 쓰이는 경우를 한번 보자

```jsx
let obj = { name: "🐶", owner: { name: "yxxn" } };
const ownerName = obj && obj.owner && obj.owner.name;
console.log(ownerName); // yxxn
```

→ 위와 같이 오브젝트 안에 또 다른 오브젝트가 있을 경우에는 obj가 있니? 그렇다면 obj의 owner가 있니? 그렇다면 obj의 owner의 name이 있니?를 통해서 값을 할당할 수 있다.

```jsx
let obj = { name: "🐶", owner: { name: "yxxn" } };
function printName(obj) {
  const ownerName = obj && obj.owner && obj.owner.name;
  console.log(ownerName);
}

printName(); //undefind
```

```jsx
let obj = { name: "🐶", owner: { name: "yxxn" } };
function printName(obj) {
  const ownerName = obj?.owner?.name;
  console.log(ownerName);
}

printName();
```

→ 이렇게 옵셔널 체이닝을 통해서 조금 더 간편하게 만들어낼 수 있다

<br />

## Nullish Coalescing Operator

ES11(ECMAScript 2020)

### ?? null, undefined인 경우에만 설정(할당)

|| falshy한 경우에만 설정(할당)

```jsx
let num = 0;
console.log(num || "-1"); // -1
```

→ 위와 같이 num이 없을 경우에만 -1을 출력하고 싶은데 -1이 출력되는 것을 확인할 수 있다.

왜냐하면 0은 falshy한 기본 값이기 때문에 false로 간주하여 -1을 할당한 것이다.

```jsx
console.log(num ?? "-1"); // 0
```

→ 초기값 대신 기존에 선언한 0이 반환되는 것을 확인할 수 있다.
