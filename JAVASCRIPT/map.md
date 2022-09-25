# map

map은 키와 값으로 이루어진 자료구조이다

순서는 전혀 중요하지 않다

**각각의 키가 해당 오브젝트를 가리키고 있기만 하면 된다**

**단, map에는 유일한 key를 가지고 있어야 한다!**

즉, key만 다르다면 계속 동일한 값을 추가할 수 있다

→ 오브젝트와 유사하다

```jsx
const map = new Map([
  ["key1", "🍎"],
  ["key2", "🍌"],
]);

console.log(map); //Map(2) { 'key1' => '🍎', 'key2' => '🍌' }
```

→ 각각의 key들이 사과, 바나나를 가리키고 있는 것을 확인할 수 있다

<br />

**map도 사이즈, 존재여부, 순회, 찾기, 추가, 삭제, 전부 삭제 모두 가능하다**

<br />

### map.size

```jsx
console.log(map.size); //2
```

<br />

### map.has()

```jsx
console.log(map.has("key1")); // true
console.log(map.has("key6")); // false
```

→ set과 다르게 key값으로 확인을 해야 한다

<br />

### 순회 - map.forEach

```jsx
map.forEach((value, key) => console.log(key, value));
// key1 🍎
// key2 🍌
```

→ set과 다르게 value와 key 모두 가져올 수 있다

<br />

### map.keys()

```jsx
console.log(map.keys()); //[Map Iterator] { 'key1', 'key2' }
```

→ map에서 기본적으로 제공하는 method keys를 통해 key값만 가져올 수 있다

<br />

### map.values()

```jsx
console.log(map.values()); //[Map Iterator] { '🍎', '🍌' }
```

→ map에서 기본적으로 제공하는 method values를 통해 value값만 가져올 수 있다

<br />

### map.entires()

```jsx
console.log(map.entries());
//[Map Entries] { [ 'key1', '🍎' ], [ 'key2', '🍌' ] }
```

→ entires를 이용하면 key와 valuse를 모두 가져올 수 있다

<br />

### map.get

```jsx
console.log(map.get("key1")); //🍎
console.log(map.get("key2")); //🍌
console.log(map.get("key4")); //undefined
```

→ get을 이용하면 원하는 key값의 value를 받아올 수 있다. 없는 key를 호출하면 undefined를 반환하는 것을 확인할 수 있다

<br />

### map.set

```jsx
map.set("key3", "🥝");
console.log(map);
//Map(3) { 'key1' => '🍎', 'key2' => '🍌', 'key3' => '🥝' }
```

→ set을 이용하면 값을 추가해줄 수 있다

<br />

### map.delete

```jsx
map.delete("key3");
console.log(map); // Map(2) { 'key1' => '🍎', 'key2' => '🍌' }
```

→ key를 이용하여 해당 key값을 삭제할 수 있다.

<br />

## 즉, map은 유일하고 고유한 값이기 때문에 모든 것은 key 값을 통해 이루어진다!!

<br />

### map.clear()

```jsx
map.clear();
console.log(map); // Map(0) {}
```

→ set과 동일하게 clear을 통하여 전체 삭제도 가능하다

<br />

### 그렇다면 오브젝트와 큰 차이점은 무엇일까?

1. **구조는 정말 비슷하지만 사용할 수 있는 함수들, methods들이 다르다**

**Object**

```jsx
const key = { name: "milk", price: 10 };
const milk = { name: "milk", price: 10, description: "맛있다" };

const obj = {
  [key]: milk,
};
console.log(obj);
//{ '[object Object]': { name: 'milk', price: 10, description: '맛있다' } }
```

**Map**

```jsx
const map2 = new Map([[key, milk]]);
console.log(map2);
// Map(1) {
  { name: 'milk', price: 10 } => { name: 'milk', price: 10, description: '맛있다' }
}
```

1. **key를 활용하여 동적 접근 방법이 다르다**

**Object**

```jsx
console.log(obj[key]); //{ name: 'milk', price: 10, description: '맛있다' }
```

→ 이렇게 동적으로 key를 활용하여 오브젝트에 접근이 가능하다

<br />

**Map**

```jsx
console.log(map2[key]); //undefined
```

→ map은 이렇게 접근이 불가능하다

**map에서 key로 접근하기 위해선 get이라는 함수를 이용해야 한다**

```jsx
console.log(map2.get(key));
//{ name: 'milk', price: 10, description: '맛있다' }
```

<br />

[map과 set quiz](https://www.notion.so/map-set-quiz-b32c919917ef41328445981801b3593f)

### map과 set quiz

1. **주어진 배열에서 중복을 제거하라**

```jsx
const fruits = ["🍌", "🍎", "🍇", "🍌", "🍎", "🍑"];
function removeDuplication(array) {
  return [...new Set(array)];
}

console.log(removeDuplication(fruits));
```

1. **주어진 두 세트의 공통된 아이템만 담고 있는 세트를 만들어라**

```jsx
const set1 = new Set([1, 2, 3, 4, 5]);
const set2 = new Set([1, 2, 3]);

function findIntersection(set1, set2) {
  const array = [...set1].filter((item) => set2.has(item));
  return new Set(array);
  // const set = Array.from(set1)
}

console.log(findIntersection(set1, set2));
```

→ 두 문제 다 중복된 값은 저장이 되지 않는 set의 특징을 이용하여 문제를 풀어나가면 쉽다!
