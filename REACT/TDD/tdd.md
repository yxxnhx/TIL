## Test 코드 짜는 법

### **Describe - Context - It 패턴**

- `describe` - 설명할 테스트 대상을 명시
  → 테스트 대상이 되는 클래스, 메서드 이름 명시
- `context` - 테스트 대상이 놓인 상황 설명
  → 테스트할 메서드에 입력할 파라미터 설명
- `it` - 테스트 대상의 행동을 설명
  → 테스트 대상 메서드가 무엇을 리턴하는지 설명

이 방식은 다음과 같은 장점이 있다.

- 테스트 코드를 계층 구조로 만들어 준다.
- 테스트 코드를 추가하거나 읽을 때 스코프 범위만 신경쓰면 된다.
- "빠뜨린" 테스트 코드를 찾기 쉽다.
  - 높은 테스트 커버리지가 필요한 경우 큰 도움이 된다.
- 재미있다. 중독성이 있다.
  - 이유는 설명하기 어려운데 이 방식을 나에게 소개받은 지인 대부분이 재미있으며, 중독성이 있다고 말했다.

### 그렇다면 한번 테스트 코드를 작성해보자

예를 들어 Input 컴포넌트를 test 해본다고 하자

**이 컴포넌트에 기대하는 바가 무엇일까?**

### 1. 앱이 실행되었을 때 Input 컴포넌트가 잘 랜더링되었는가

**그럼 여기서 무엇을 테스트를 해야할까?**

```jsx
import React from 'react';

export default function Input({ onClick, value, onChange }) {
  return (
    <p>
      <label htmlFor="input-task-title">할 일</label>
      <input
        type="text"
        id="input-task-title"
        placeholder="할일을 입력해주세요"
        value={value}
        onChange={onChange}
      />
      <button type="button" onClick={onClick}>
        추가
      </button>
    </p>
  );
}
```

일단 Input 컴포넌트에 있는 입력창과 추가 버튼이 잘 랜더링 되어 나타내는지를 확인하는 것이다.

```jsx
describe('<Input />', () => {
  it('Input 이 보인다', () => {
    // ...
  });
});
```

## 2. 사용자의 입력을 잘 받는가

```jsx
describe('Input', () => {
  it('Input 이 보인다', () => {
    // ...
  });
  describe('사용자가 텍스트를 입력하면', () => {
    it('handleChange 함수가 호출된다', () => {
      // ...
    });
  });
});
```

`describe`, `context`, `it` 각각이 무슨 역할을 하며, 무엇을 적어야 하는지

`given`-`when`-`then`도 마찬가지이다.

또한, 테스트 코드 작성에서 2가지를 아는 것이 중요하다.

1. 무엇을 테스트 할 것인가
2. 1번을 위해 어떻게 테스트코드를 작성할 것인가

즉, 컴포넌트를 직접 실행하지 않고도, 테스트코드만을 보고 컴포넌트의 역할과 동작을 설명할 수 있는 명세를 작성한다고 생각하면 쉽다.

Page 컴포넌트를 예를들어 Page 컴포넌트의 행동(역할)을 나열해보자.

```jsx
import React from 'react';

import Input from './Input';
import List from './List';

export default function Page({
  taskTitle,
  tasks,
  onClickAddTask,
  onChangeTitle,
  onClickDeleteTask,
}) {
  return (
    <div>
      To-do
      <Input
        value={taskTitle}
        onChange={onChangeTitle}
        onClick={onClickAddTask}
      />
      <List tasks={tasks} onClickDelete={onClickDeleteTask} />
    </div>
  );
}
```

```jsx
title을 렌더링한다
- input을 렌더링한다
- change event를 listen한다
- 등..
```

나열한것들이 이미 하위컴포넌트에서 테스트한 것일 수도 있지만 Page 컴포넌트에서도 발생하고있는 일들이므로 상관없다.

각각의 컴포넌트를 독립적으로 바라보자.

이제 나열한것들을 그대로 테스트 시나리오로 옮겨서 작성하자.
