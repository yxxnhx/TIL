# Flux Architecture

[Flux | 사용자 인터페이스를 만들기 위한 어플리케이션 아키텍쳐](https://haruair.github.io/flux/docs/overview.html)

Flux는 페이스북이 제시한 클라이언트 웹 애플리케이션 아키텍쳐
지금 사용하고 있는 React는 커다란 애플리케이션, Flux는 그것을 활용할 수 있는 아키텍쳐이다.
아키텍쳐란, 컴퓨터 시스템의 구성이라는 뜻을 가진 단어이다.
즉, 애플리케이션의 시스템을 구성해주는 것

위의 문서를 **반드시 정독**해야 한다. 아마 여러 가지로 어려운 내용이 있을 것이다. 실제 Redux는 구현이 좀 다른 부분이 있기 때문에 각 항목에 대해 디테일하게 모두 숙지하실 필요는 없다.

다만, 단방향 데이터 Flow란 점과 Action이 Dispatcher를 통해 Store에 전달되는 흐름이 핵심이란 걸 기억해야 한다.

## **Redux - Flux Architecture의 구현체**

**Redux로 상태 관리를 하는 이유와 이로 인해 얻어지는 이점**

- 상태 관리는 사실 리액트의 관심사가 아니다.
- App은 상태가 어떻게 처리되는지 모르게 한다.
- App은 View가 어떻게 그려지는지만 **관심**을 갖게 한다.
- App은 상태 관리(store)와 View를 연결만 해준다.
- App은 상태 관리 로직이 어떻게 구현됐는지는 모른다.
- 상태 관리 로직 변경의 영향이 View로 전파되지 않는다.

### 구조와 데이터 흐름

![Untitled](https://haruair.github.io/flux/img/flux-simple-f8-diagram-1300w.png)

여태까지 우리는 React를 활용해서 View에 관련한 부분들을 처리해왔다.
그런데 그 부분들을 조금 더 나눠서 처리를 해보자는 것이다

단방향으로 데이터 처리를 하는 것이 핵심 플로우이다.
Action이라는 것을 만들어낸다. Todo로 예시를 들자면 addTask, deleteTask가 action에 해당한다.
그 action을 받아서 처리할 수 있는 Dispatcher가 받아서 처리할 수 있는 곳으로 보내준다.
그 후 Store에서 Action에 따라서 상태를 변화해주고, 상태에 대해서도 저장을 해준다.
마지막으로 View로 넘어가게 된다.

![Untitled](https://haruair.github.io/flux/img/flux-simple-f8-diagram-with-client-action-1300w.png)

View에서 또 다른 버튼을 누르면 또 새로운 Action이 생기게 된다.

예를 들어서 할일 목록 추가를 누르면 AddTask라는 함수가 실행이 되는 등의 새로운 Action이 생기면 동일하게 dispatcher가 처리할 수 있는 곳으로 보내주고 store에서 action에 따라서 상태 변화 또는 저장을 하여 view로 넘어가는 형식을 반복한다.

![Untitled](https://haruair.github.io/flux/img/flux-simple-f8-diagram-explained-1300w.png)

Action을 만들어내는 함수(함수가 아니어도 됨)가 있는데 그것은 Action creater라는 함수 매서드가 있다.
Dispatcher에서는 처리할 수 있는 곳으로 보내주고 Store에서는 그 상태를 업데이트 등 여러가지를 해준다.
이렇게 Flux Architecture를 발표하고 나서 구현할 수 있는 여러가지 구현체가 나왔다.

**그 중 하나가 Redux이다.**

**리덕스를 활용하게 되면 어떤 점이 좋을까?**
예를 들어 아래와 같은 상태를 전체적으로 관리하는 App 컴포넌트가 있다고 해보자

```jsx
import React, { useState } from 'react';

import Page from './Page';

export default function App() {
  const [state, setState] = useState({
    newId: 100,
    taskTitle: '',
    tasks: [
      {
        id: 1,
        title: '아무것도 하지 않기',
      },
      {
        id: 2,
        title: '아무것도 하지 않기',
      },
    ],
  });

  const { newId, tasks, taskTitle } = state;

  function handleClickAddTask() {
    // TODO: 할일 추가
    setState({
      ...state,
      newId: newId + 1,
      taskTitle: '',
      tasks: [...tasks, { id: newId, title: taskTitle }],
    });
  }

  function handleChangeTitle(e) {
    setState({
      ...state,
      taskTitle: e.target.value,
    });
    console.log(e.target.value);
  }

  function handleClickDeleteTask(id) {
    // TODO: Delete
    setState({
      ...state,
      tasks: tasks.filter((task) => task.id !== id),
    });
    console.log(id);
  }

  return (
    <Page
      taskTitle={taskTitle}
      tasks={tasks}
      onClickAddTask={handleClickAddTask}
      onChangeTitle={handleChangeTitle}
      onClickDeleteTask={handleClickDeleteTask}
    />
  );
}
```

```jsx
const [state, setState] = useState({
  newId: 100,
  taskTitle: '',
  tasks: [
    {
      id: 1,
      title: '아무것도 하지 않기',
    },
    {
      id: 2,
      title: '아무것도 하지 않기',
    },
  ],
});

const { newId, tasks, taskTitle } = state;

function handleClickAddTask() {
  // TODO: 할일 추가
  setState({
    ...state,
    newId: newId + 1,
    taskTitle: '',
    tasks: [...tasks, { id: newId, title: taskTitle }],
  });
}

function handleChangeTitle(e) {
  setState({
    ...state,
    taskTitle: e.target.value,
  });
  console.log(e.target.value);
}

function handleClickDeleteTask(id) {
  // TODO: Delete
  setState({
    ...state,
    tasks: tasks.filter((task) => task.id !== id),
  });
  console.log(id);
}
```

그중 App에서 return하는 것 외의 상태를 관리하는 것들은 사실은 React의 주된 관심사가 아니다.
그냥 어떤 로직들, 처리하는 것에 불과하다.
순수하게 화면에 구현하는 것으로 본다면 이 상태들은 그냥 로직들, 독립적이고 관심사가 다르다.
기능들을 만들고 테스트를 하는데 App이라는 컴포넌트가 있을 필요가 없다.

**그렇다면 어딘가로 이 상태들을 분리하여 관리할 수 있다면 어떨까?**
이 아이디어에서 먼저 출발을 해보자.
한번 분리를 해본 후에 Flux Arcitechure에 녹여내는 것을 해보자.
