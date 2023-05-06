# React.memo

## 메모리제이션

[React 최상위 API - React](https://ko.reactjs.org/docs/react-api.html#reactmemo)

→ 동일한 계산을 반복해야 할 때 이전에 계산한 값을 메모리에 지정함으로써 동일한 계산의 반복을 수행을 제거하여 프로그램 실행 속도를 빠르게 하는 기술이다.

### **고차 컴포넌트**

고차 컴포넌트(HOC, Higher Order Component)는 컴포넌트 로직을 재사용하기 위한 React의 고급 기술입니다. 고차 컴포넌트(HOC)는 React API의 일부가 아니며, React의 구성적 특성에서 나오는 패턴입니다.

구체적으로, **고차 컴포넌트는 컴포넌트를 가져와 새 컴포넌트를 반환하는 함수입니다.**

```jsx
const EnhancedComponent = higherOrderComponent(WrappedComponent);
```

### memo

```jsx
const MyComponent = React.memo(function MyComponent(props) {
  /* props를 사용하여 렌더링 */
});
```

### 컴포넌트들이 언제 render가 될까?

1. 최초로 컴포넌트가 마운트 되었을 때
2. state 값이 변했을 때
3. 상위 컴포넌트(부모)로부터 새로운 props를 새로 전달 받았을 때

### 그렇다면 어떻게 하면 메모리 낭비를 하지 않고 속도를 높일 수 있을까?

1. state 선언할 때 선언 위치
2. state 값을 어떤 식으로 분할해서 만들 것인지 고려하여 배치한다
3. map을 돌릴 때 key값을 index 번호로 사용하지 않는다
4. react memo라는 고차 컴포넌트를 이용하여 메모리제이션 하는 방법

### index.jsx

```jsx
import React from "react";
import UserList from "./UserList";

const Sample1 = () => {
  return (
    <div>
      <UserList />
    </div>
  );
};

export default Sample1;
```

### UserList.jsx

```jsx
import React from "react";
import { useState } from "react";
import UserDetail from "./UserDetail";

const UserList = () => {
  console.log("유저리스크 리랜더링");
  const [user, setUser] = useState([
    {
      id: 0,
      name: "철수",
      age: 30,
    },
    {
      id: 1,
      name: "영희",
      age: 20,
    },
  ]);

  const addUser = () => {
    setUser([
      ...user,
      {
        id: 2,
        name: "정희",
        age: 15,
      },
    ]);
  };

  return (
    <div>
      <button disabled={user.length >= 3} onClick={addUser}>
        유저 만들기
      </button>
      {user.map((item) => {
        return <UserDetail key={item.id} userItem={item} />;
      })}
    </div>
  );
};

export default UserList;
```

### UserDetail.jsx

```jsx
import React from "react";

const UserDetail = ({ userItem }) => {
  console.log(`userDetail ${userItem.id}`);
  return (
    <div>
      <h2>{userItem.name}</h2>
      <h3>{userItem.age}</h3>
    </div>
  );
};

export default UserDetail;
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/43ec294a-91c5-4d10-b016-a19972b817b9/Untitled.png)

### 위와 같은 상황에서 유저 만들기 버튼을 눌러보자

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8460e109-10a3-4cee-80a6-2413f1d3b46c/Untitled.png)

→ 정희만 추가하였고 철수와 영희는 변경된 것이 없으나 userList에 있는 요소들이기 때문에 다시 리랜더링이 된다

### 이렇게 불필요한 요소들이 계속 불러지게 되면 속도가 느려지게 된다

그렇다면 어떻게 이 부분을 보완할 수 있을까?

### React.memo를 이용해보자

```jsx
import React from "react";

const UserDetail = React.memo(({ userItem }) => {
  console.log(`userDetail ${userItem.id}`);
  return (
    <div>
      <h2>{userItem.name}</h2>
      <h3>{userItem.age}</h3>
    </div>
  );
});

export default UserDetail;
```

```jsx
import React from "react";

const UserDetail = ({ userItem }) => {
  console.log(`userDetail ${userItem.id}`);
  return (
    <div>
      <h2>{userItem.name}</h2>
      <h3>{userItem.age}</h3>
    </div>
  );
};

export default React.memo(UserDetail);
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bbd36120-6f54-4497-8040-195c376cfe91/Untitled.png)

→ React.memo로 감싸주니 불필요한 요소들은 리랜더링이 되지 않는다

### useMemo

### Array.reduce()

배열을 순회하면서 각 요소를 특정 함수 실행한 결과를 누적하여 하나의 결과값을 만들어준다

```jsx
arr.map(
  callback(accumulator, currentValue, currentIndex, originArray),
  initialValue,
);
```

**매개변수**

| callback                | 새로운 배열 요소를 생성하는 함수                                                                                          |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| accumulator             | callback의 이전 반환값callback 최초 호출일 때 initialValue를 제공한 경우 initialValue, 아니면 배열의 첫번째 요소값입니다. |
| currentValue            | 처리할 현재 요소                                                                                                          |
| currentIndex (Optional) | 처리할 현재 요소의 인덱스initialValue를 제공한 경우 0, 아니면 1부터 시작합니다.                                           |
| originArray (Optional)  | reduce()를 호출한 배열                                                                                                    |
| initialValue (Optional) | callback의 최초 호출에서 첫 번째 인수(accumulator)에 제공하는 값                                                          |

**반환값**배열의 각 요소를 callback함수를 실행한 결과로 만든 새로운 배열

# **❐ 예제**

배열의 모든 요소를 더한 누적값을 구해봅시다.그리고 callback함수의 Optional parameter가 없고 initialValue가 없는 경우, parameters값도 직접 확인해봅시다.

```jsx
let testArray = [0, 1, 2, 3, 4];
let result = testArray.reduce((accumulator, currentValue) => {
  console.log(accumulator, currentValue);
  return accumulator + currentValue;
});
console.log(result);
// expected output:
// 0 1
// 1 2
// 3 3
// 6 4
// 10a
```

배열의 모든 요소를 더한 누적값을 구해봅시다.

그리고 callback함수의 Optional parameter가 없고 initialValue가 있는 경우, parameters값도 직접 확인해봅시다.

```jsx
let testArray = [0, 1, 2, 3, 4];
let result = testArray.reduce((accumulator, currentValue) => {
  console.log(accumulator, currentValue);
  return accumulator + currentValue;
}, 10);
console.log(result);
// expected output:
// 10 0
// 10 1
// 11 2
// 13 3
// 16 4
// 20
```

배열의 모든 요소를 더한 누적값을 구해봅시다.그리고 callback함수의 Optional parameter가 있고 initialValue가 없는 경우, parameters값도 직접 확인해봅시다.

```jsx
let testArray = [0, 1, 2, 3, 4];
let result = testArray.reduce(
  (accumulator, currentValue, currentIndex, originArray) => {
    console.log(accumulator, currentValue, currentIndex, originArray);
    return accumulator + currentValue;
  },
);
console.log(result);
// expected output:
// 0 1 1 [0, 1, 2, 3, 4]
// 1 2 2 [0, 1, 2, 3, 4]
// 3 3 3 [0, 1, 2, 3, 4]
// 6 4 4 [0, 1, 2, 3, 4]
// 10
```

배열의 모든 요소를 더한 누적값을 구해봅시다.그리고 callback함수의 Optional parameter가 있고 initialValue가 있는 경우, parameters값도 직접 확인해봅시다.

```jsx
let testArray = [0, 1, 2, 3, 4];
let result = testArray.reduce(
  (accumulator, currentValue, currentIndex, originArray) => {
    console.log(accumulator, currentValue, currentIndex, originArray);
    return accumulator + currentValue;
  },
  10,
);
console.log(result);
// expected output:
// 10 0 0 [0, 1, 2, 3, 4]
// 10 1 1 [0, 1, 2, 3, 4]
// 11 2 2 [0, 1, 2, 3, 4]
// 13 3 3 [0, 1, 2, 3, 4]
// 16 4 4 [0, 1, 2, 3, 4]
// 20
```

숫자가 들어있는 배열에서 0보다 큰 숫자 갯수를 구해봅시다.

```jsx
const numberArray = [2, -5, -123, 59, -5480, 24, 0, -69, 349, 3];
const result = numberArray.reduce((accumulator, currentValue, currentIndex) => {
  if (currentValue > 0) {
    accumulator++;
  }
  return accumulator;
}, 0);
console.log(result);
// expected output:
// 5
```

숫자가 들어있는 배열에서 0보다 큰 숫자만 필터하여 각 요소에 2를 곱한 새로운 배열을 만들어봅시다.

이 처럼 reduce() 하나로 filter() + map() 함수를 동시에 사용한 것과 동일한 결과를 얻을 수도 있습니다.

```jsx
const numberArray = [2, -5, -123, 59, -5480, 24, 0, -69, 349, 3];
const result = numberArray.reduce((accumulator, currentValue, currentIndex) => {
  if (currentValue > 0) {
    accumulator.push(currentValue * 2);
  }
  return accumulator;
}, []);
console.log(result);
// expected output:
// [4, 118, 48, 698, 6]
```

출처
