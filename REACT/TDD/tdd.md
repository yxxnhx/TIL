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

# 과제 - To-do 테스트 작성하기

실제로 앱을 만들 때에도 바깥에 있는 app부터 작성하듯이 테스트 코드도 app부터 작성하는 것도 좋은 방법이다.

A******\*\*******pp.test.jsx******\*\*******

```jsx
import React from 'react';

import { render } from '@testing-library/react';

import App from './App';

test('App', () => {
  const { getByText } = render(<App />);

  expect(getByText(/추가/)).not.toBeNull();
});
```

→ 앱이 실행하자마자 보이는 가장 기본적인 것만 getByText를 이용하여 보이는지 여부를 테스트한다.

왜냐하면 App에서는 실제로 무언가를 하기 보다 상태를 관리하고 값을 내려주기 때문이다.

- TODO : 통합 테스트 코드 작성
- CodeceptJS : 실제 브라우저에서 사용자 테스트 실행 가능

**Page.test.jsx**

페이지에서는 데이터를 넣어서 랜더링 되는 그 모습까지 확인을 해주어야 한다.

Page에서는 App과 형식이 비슷하지만 App처럼 단독으로 사용하는 것이 아니라 App에서 만든 상태들을 내려받아 Page를 활동하게 해주는 것이기 때문에 그 부분을 한번 작성해보자.

**즉, Page 컴포넌트를 쓰는 방법을 서술하는 것이다.**

```jsx
const { getByText } = render(
  <Page
    taskTitle="" //입력한 값
    tasks={tasks}
    onClickAddTask={handleClickAddTask}
    onChangeTitle={handleChangeTitle}
    onClickDeleteTask={handleClickDeleteTask}
  />,
);
```

**페이지에는 어떻게 값이 들어갈 것인지 작성을 해주자**

```jsx
const tasks = [
  {
    id: 1,
    title: 'Task-1',
  },
  {
    id: 2,
    title: 'Task-2',
  },
];
```

********************\*\*********************한번 확인해보자********************\*\*********************

```jsx
expect(getByText(/Task-1/)).not.toBeNull();
expect(getByText(/Task-2/)).not.toBeNull();
```

→ 정상적으로 테스트가 통과되는 것을 확인할 수 있다.

****************************************************\*\*****************************************************그렇다면 추가 버튼이 제대로 호출되는지 여부를 확인해보자****************************************************\*\*****************************************************

먼저 jest의 함수를 이용하여 앱에서 생성한 함수를 만들어주자

```jsx
const handleChangeTitle = jest.fn();
const handleClickAddTask = jest.fn();
const handleClickDeleteTask = jest.fn();
```

fireEvent를 이용하여 handleClickAddTask가 호출되는지 여부를 확인해보자

```jsx
fireEvent.click(getByText('추가'));
expect(handleClickAddTask).toBeCalled();
```

→ 정상적으로 테스트가 통과되는 것을 확인해보자

**만약 내가 새롭게 입력한 값을 받아오는 상황을 확인하고 싶다면 다음과 같이 작성하면 동작을 할까?**

```jsx
const { getByText } = render(
  <Page
    taskTitle="New Task"
    /* 생략 */
  />,
);

fireEvent.click(getByText('추가'));
expect(handleClickAddTask).toBeCalledWith('New Task');
```

→ 정답은 아니다.

왜냐하면 이 상태를 관리하는 것은 setState로 App 컴포넌트에서 전체적으로 관리하게 하였기 때문에 Page에서는 상태를 변경하거나 값을 새롭게 줄 수가 없는 것이다.

그러므로 다른 사람이 이 테스트 코드를 볼 때 Page에서는 with를 사용하지 않고 그냥 추가를 누르면 호출만 하는 구나, 그렇다면 여기에서는 상태를 관리하거나 생성을 하지 않는다는 것을 알 수 있는 것이다.

실제로 개발을 할 때는 컴포넌트를 만들면서 테스트 코드를 작성하기도 하고, 테스트 코드를 먼저 작성 후 정상적으로 테스트를 통과한 후에 컴포넌트를 만드는 작업을 하기도 한다.

순서는 상관이 없으나 확실한 것은 테스트 코드에서 먼저 정상 작동을 하는지를 확인 후에 그 모양을 그대로 컴포넌트에 가져와서 사용하는 것을 만드는 것을 선호한다.

사용법을 우선으로 하는 테스트가 좋다.

**List.test.jsx**

리스트가 보이는지 여부, 안 보인다면 할 일이 없다는 메시지가 보일 수 있게 하기 위해서 한번 테스트 코드를 작성해보자.

```jsx
<List tasks={tasks} onClickDelete={handleClickDelete} />;

expect(getByText(/Task-1/)).not.toBeNull();
expect(getByText(/Task-2/)).not.toBeNull();

const buttons = getAllByText('완료');
fireEvent.click(buttons[0]);
expect(handleClickDelete).toBeCalledWith(1);
```

→ 정상적올 완료 버튼을 누르면 handleClickDelete가 호출되는 것을 확인할 수 있다.

**그렇다면 task가 없는 경우도 있을텐데 그 경우에는 어떻게 할까?**

두 가지 경우를 따로따로 검사를 해야 한다.

그럴 때 describe - context - it을 활용하여 구분하여 테스트를 진행하면 좋다.

test는 한 가지 경우만 테스트를 하는 것이다.

```jsx
test('테스트 #1');
```

→ 위와 같이 테스트 1만 검사를 할 경우에만 사용을 한다

```jsx
describe - it => describe('List') => it('renders tasks')
```

→ 리스트를 검사를 할 거야 tasks가 랜더가 되는지 여부를

- **with tasks**
  - List renders tasks
  - List renders 'delete' button to delete task
- **without tasks**
  - List render no tasks message

```jsx
describe('List', () => {
  const handleClickDelete = jest.fn();

  context('with tasks', () => {
    const tasks = [
      {
        id: 1,
        title: 'Task-1',
      },
      {
        id: 2,
        title: 'Task-2',
      },
    ];

    it('renders tasks', () => {
      render(
        <List
          taskTitle="" //입력한 값
          tasks={tasks}
          onClickDelete={handleClickDelete}
        />,
      );
      expect(getByText(/Task-1/)).not.toBeNull();
      expect(getByText(/Task-2/)).not.toBeNull();

      const buttons = getAllByText('완료');
      fireEvent.click(buttons[0]);
      expect(handleClickDelete).toBeCalledWith(1);
    });
  });
});
```

여기에서 context를 그냥 사용하려고 하면 jest에서 지원을 하지 않아 에러가 뜨는 것을 확인할 수 있다.

```jsx
npm i -D jest-plugin-context
```

************\*\*\*\*************jest.config.js************\*\*\*\*************

```jsx
module.exports = {
  testEnvironment: 'jsdom',
  setupFilesAfterEnv: ['jest-plugin-context/setup', './jest.setup'],
};
```

→ jest-plugin-context/setup를 추가해주면 된다

그런데 지금 리스트에서는 랜더링이 되면 할일과 완료(삭제) 버튼이 랜더링이 되므로 그 부분을 나누어서 테스트 코드를 짜보자

```jsx
it('renders tasks', () => {
      render(
				<List
			    taskTitle="" //입력한 값
			    tasks={tasks}
			    onClickDelete={handleClickDelete}
			  />)

      const { getByText } = List(tasks)
      expect(getByText(/Task-1/)).not.toBeNull()
      expect(getByText(/Task-2/)).not.toBeNull()
    })

    it('renders "완료" button to delete task', () => {
      const { getAllByText } = renderList(tasks)

      const buttons = getAllByText('완료')
      fireEvent.click(buttons[0])
      expect(handleClickDelete).toBeCalledWith(1)
    })
  })
```

**\*\*\*\***이제 tasks가 없을 때의 경우를 테스트 해보자**\*\*\*\***

```jsx
context('without tasks', () => {
  it('render no tasks message', () => {
    const tasks = [];
    const { getByText } = renderList(tasks);

    expect(getByText(/할 일이 없어요!/)).not.toBeNull();
  });
});
```

계속해서 List가 랜더되는 부분이 반복되는 것을 확인할 수 있다.

이것을 한번 함수로 외부에 빼서 코드를 한번 정리를 해보자

```jsx
function renderList(tasks) {
  return render(
    <List
      taskTitle="" //입력한 값
      tasks={tasks}
      onClickDelete={handleClickDelete}
    />,
  );
}

/* 생략 */

const { getByText } = renderList(tasks);
```

### TDD cycle : Red - Green - Refactoring

TDD에는 사이클이 있다.

잘못된 코드를 작성하고 빠르게 수정하고 해당 코드를 리팩토링하는 과정을 계속해서 반복하는 사이클이다.

그런데 왜 이렇게 테스트 코드를 계층 코드로 복잡하게 만드는가에 대한 의문이 생길 수도 있다.

그럴 때는 한번 다음과 같이 jest를 실행해보자

```jsx
npx jest --watchAll --verbose
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8f4db4d8-1264-4f8e-8c97-71bc1ef47cc5/Untitled.png)

→ 이와 같이 케이스에 따라서 구분되어 계층구조로 테스트가 확인되는 것을 볼 수 있기 때문이다.

**Input.test.jsx**

```jsx
test('Input', () => {
  const { getByText } = render(<Input />);
});
```

먼저 기본 뼈대를 만든 후에 input이 어떻게 생겼는지를 확인해보자

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

→ 입력창과 추가가 있고 input에는 value와 onChange가 할당된 것을 확인할 수 있다.

```jsx

const handleChange = jest.fn()
const handleClick = jest.fn()

<Input
  value='기존 할일'
  onChange={handleChange}
  onClick={handleClick}
 />
```

**그렇다면 화면에 무엇이 출력되어야 할까?**

먼저 첫번째로 value로 들어간 기존 할 일이 출력되는 것을 확인해보자

```jsx
expect(getByDisplayValue('기존 할일')).not.toBeNull();
```

→ 정상적으로 테스트에 통과된 것을 확인할 수 있다

그렇다면 onChange와 추가 버튼이 제대로 출력되는지를 확인해보자

```jsx
fireEvent.change(getByLabelText('할 일'), {
  target: {
    value: '무언가 하기',
  },
});

expect(handleChange).toBeCalled();

fireEvent.click(getByText('추가'));

expect(handleClick).toBeCalled();
```

→ 할 일이라는 label를 가진 곳에 value를 ‘무언가 하기’로 변경해보자

나는 handleChange가 불린 것을 기대한다.

그리고 fireEvent로 ‘추가’라는 text를 가진 것을 클릭하여 handleClick이 호출되는 것을 기대한다는 뜻이다
