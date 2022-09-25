# Symbol

유일한 값, 유일한 키를 나타낼 때 유용하게 사용할 수 있다

```jsx
const map = new Map();
const key1 = "key";
const key2 = "key";
map.set(key1, "hello");
console.log(map.get(key2)); // hello
```

→ key1, key2 모두 동일하게 문자열로 key를 가지고 있기 때문에 set으로 key1의 값만 hello로 변경을 해도 key2까지 함께 변경된 것을 확인할 수 있다

**왜냐? 원시타입이기 때문에 동일하다고 간주하였기 때문!**

<br />

### **이러한 것을 symbol을 이용하면 다르게 나타낼 수 있다**

```jsx
const key1 = Symbol("key");
const key2 = Symbol("key");
map.set(key1, "hello");
console.log(map.get(key2)); // undefined
console.log(key1 === key2); //false
```

→ 심볼을 이용하여 생성하면 위와 같이 key2가 변경되지 않고 undefined가 반환되고 key1와 key2가 동일한지 여부 또한 false가 나오는 것을 확인할 수 있다.

**즉, symbol은 이름은 똑같아도 서로 다른 유일한 키를 만들 때 사용할 수 있다.**

### 동일한 이름으로 하나의 키를 사용하고 싶다면 Symbol.for을 이용하면 된다

symbol의 for을 이용하면 전역적으로 심벌을 관리하는 전역 심벌 레지스트리(Global Symbol Registry)라는 곳이 있다. 여기서 이 이름의 심벌을 만들고 그것을 계속 재사용할 수 있다.

```jsx
const k1 = Symbol.for("key");
const k2 = Symbol.for("key");
console.log(k1 === k2); //true
```

```jsx
console.log(Symbol.keyFor(k1)); //key
console.log(Symbol.keyFor(key1)); //undefined
```

→ Symbol.keyFor은 글로벌 레지스트리에 저장된 것만 찾을 수 있다

```jsx
const obj = { [k1]: "hello", [Symbol("key")]: 1 };
console.log(obj); //{ [Symbol(key)]: 'hello', [Symbol(key)]: 1 }
console.log(obj[k1]); // hello
console.log(obj[Symbol("key")]); //undefined
```

→ symbol로 생성한 오브젝트는 접근할 수 없는 것을 확인할 수 있다
