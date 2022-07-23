# Map과 Set

### Array

- primitive type 뿐만 아니라 객체들도 저장 가능
- index를 기반으로 데이터를 저장한다
  → 배열은 인덱스를 기반으로 해서 필요한 데이터들을 저장할 수 있다. 그러므로 인덱스를 통해 저장하기 때문에 순서가 매우 중요하다
- 데이터의 중복이 가능하다
  → 안에 어떠한 데이터가 들어가는지는 서로 영향을 미치지 않기 때문에 데이터 중복이 가능하다

<br />

### Set

- 데이터의 집합체
- index 없다
- 순서 또한 없다
- 중복 불가능

```jsx
const set = new Set([1, 2, 3]);
console.log(set); //Set(3) { 1, 2, 3 }
```

→ 이와 같이 Set 안에 기존의 배열을 넣어서도, 빈 값()으로도 사용이 가능하다

<br />

**set에는 다양한 method들이 있다**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/680309fd-e634-47a2-828a-c39ba1481230/Untitled.png)

<br />

### set.size

```jsx
console.log(set.size); // 3
```

→ 크기 확인

<br />

### set.has()

```jsx
console.log(set.has(2)); //true
console.log(set.has(6)); //false
```

→ 해당 set에 괄호 안의 값이 있는지 여부

<br />

### 순회 - set.forEach

```jsx
set.forEach((item) => console.log(item)); // 1 2 3
```

<br />

### 순회 - values()

```jsx
for (const value of set.values()) {
  console.log(value); // 1 2 3
}
```

<br />

### set.add()

```jsx
set.add(6);
console.log(set); //Set(4) { 1, 2, 3, 6 }
```

<br />

**그렇다면 이미 있는 값을 추가하면 어떻게 될까?**

```jsx
set.add(6);
console.log(set); //Set(4) { 1, 2, 3, 6 }
set.add(6);
console.log(set); //Set(4) { 1, 2, 3, 6 }
```

→ 무시되는 것을 확인할 수 있다

**즉, 중복이 불가능하다!**

<br />

### set.delete()

```jsx
set.delete(6);
console.log(set); //Set(3) { 1, 2, 3 }
```

→ 기존에 추가되었던 6이 삭제되어 출력되는 것을 확인할 수 있다.

<br />

### set.clear()

```jsx
set.clear();
console.log(set); // Set(0) {}
```

→ set 안에 있던 모든 것이 삭제 되어 크기가 0으로 나오는 것을 확인할 수 있다

<br />

**그렇다면 오브젝트로도 사용이 가능할까?**

정답은 **가능하다!**

```jsx
const obj1 = { name: "🍎", price: 8 };
const obj2 = { name: "🍌", price: 5 };
const objs = new Set([obj1, obj2]);
//Set(2) { { name: '🍎', price: 8 }, { name: '🍌', price: 5 } }
```

→ 총 두개의 오브젝트가 들어가 있는 것을 확인할 수 있다

<br />

**여기서 퀴즈!**

오브젝트 set을 만든 후에 오브젝트 1의 값을 바꾸면 어떻게 될까?

```jsx
obj1.price = 10;
objs.add(obj1);
console.log(objs);
// Set(2) { { name: '🍎', price: 10 }, { name: '🍌', price: 5 } }
```

→ 변경된 값이 반영된다

**즉, object는 shadow copy가 된다!**

오브젝트의 각각 0x0111, 0x0112의 주소가 있다고 가정하면 그 주소에 들어가서 값을 바꾸는 것이니 값이 변경될뿐 set이 3개가 되어 새로운 오브젝트가 생성되지 않는다

<br />

**두 번째 퀴즈!**

```jsx
const obj3 = { name: "🍌", price: 5 };
objs.add(obj3);
console.log(objs);
```

위와 같이 obj2와 동일한 오브젝트를 추가하게 되면 어떻게 될까?

<br />

```jsx
Set(3) {
  { name: '🍎', price: 10 },
  { name: '🍌', price: 5 },
  { name: '🍌', price: 5 }
}
```

→ 정답은 새로운 오브젝트 obj3이 추가되어 크기는 3으로 변경이 된다

힙에 새로운 주소 0x0113이라는 주소를 가진 obj3이 생성되어 set에 추가된다
