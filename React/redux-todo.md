# 리덕스를 이용한 To-do TDD 실습

### 1. 초기 상태를 분리해보자.

```jsx
const initialState = {
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
}

export default function App() {

  const [state, setState] = useState(initialState);
```

→ 초기값을 App 함수 외부로 빼서 정리를 해보자.

App이라는 컴포넌트가 초기 상태를 알 필요가 있을까? 몰라도 상관이 없기 때문에 외부로 분리를 시켜서 관리를 하게 해보자.

**이 분리한 값을 따로 관리를 하면 되니까!**

### 2. 함수들을 분리해보자

**첫번째로 handleChangeTitle를 분리해보자**

```jsx
function handleChangeTitle(e) {
  setState({
    ...state,
    taskTitle: e.target.value,
  });
  console.log(e.target.value);
}
```

여기에서 다른 용어를 사용해본다면 어떨까?

```jsx
function handleChangeTitle(e) {
  setState(updateTaskTitle(state, e.target.value));
}
```

App에서는 updateTaskTitle이 어떻게 구현하는지 알 필요가 없다.

그냥 updateTaskTitle를 할 뿐이라는 것만 알고 있으면 된다.
그저 기존 상태를 알아야 값을 바꾸고 업데이트를 해줄 수 있으므로 기존 상태와 값만 알고 있으면 된다.

**그렇다면 updateTaskTitle 함수를 외부로 분리를 해보자.**

```jsx
function updateTaskTitle(state, taskTitle) {
  return {
    ...state,
    taskTitle,
  };
}
```

이렇게 initailState에서 지정한 초기값을 updateTaskTitle에서 값을 업데이트해주는 것을 처리할 수 있다.

**그다음 handleClickAddTask를 분리해보자**

```jsx
function handleClickAddTask() {
  // TODO: 할일 추가
  setState({
    ...state,
    newId: newId + 1,
    taskTitle: '',
    tasks: [...tasks, { id: newId, title: taskTitle }],
  });
}
```

```jsx
function handleClickAddTask() {
  setState(addTask(state));
}
```

→ handleClickAddTask에서는 기존 상태만 알면 된다

기존에 있던 것들에 대해서는 이미 상태 안에 있기 때문이다.

```jsx
function addTask(state) {
  return {
    ...state,
    newId: newId + 1,
    taskTitle: '',
    tasks: [...tasks, { id: newId, title: taskTitle }],
  };
}
```

→ 이렇게 기존에 handleClickAddTask에서 가지고 있던 값들을 넣으면 newId가 없어서 에러가 뜨는 것을 확인할 수 있다.

**newId는 어디에서 왔을까?**

바로 상태 안에서 꺼내왔으니 addTask 안에서 구조분해할당으로 꺼내오면 간단히 해결할 수 있다!

그리고 tasks와 taskTitle도 필요하니 함께 구조분해할당을 이용하여 빼내보자

```jsx
function addTask(state) {
  const { newId, tasks, taskTitle } = state;

  return {
    ...state,
    newId: newId + 1,
    taskTitle: '',
    tasks: [...tasks, { id: newId, title: taskTitle }],
  };
}
```

**마지막으로 handleClickDeleteTask를 분리해보자**

기존

```jsx
function handleClickDeleteTask(id) {
  // TODO: Delete
  setState({
    ...state,
    tasks: tasks.filter((task) => task.id !== id),
  });
}
```

분리

```jsx
function handleClickDeleteTask(id) {
  setState(deleteTask(state, id));
}
```

```jsx
function deleteTask(state, id) {
  const { tasks } = state;

  return {
    ...state,
    tasks: tasks.filter((task) => task.id !== id),
  };
}
```

→ 이렇게 각각 필요한 값만 남기고 외부로 분리할 수 있는 것이다.

그렇다면 App 컴포넌트 내부에서 사용하려고 만들어둔 구조분해할당을 한번 확인해보자

```jsx
const { newId, tasks, taskTitle } = state;
```

→ 더이상 newId는 컴포넌트 내부에서 필요가 없을 것이다.

왜냐하면 newId는 추가할 때만 필요한 것인데 이미 컴포넌트 바깥에서 랜더링할 때 기본 세팅으로 newId: 100을 넣어두었기 때문에 내부에서는 몰라도 된다.

**실제로 화면을 그려줄 때 쓰는 것은 아니니까!**

우리가 원하는 것은 tasks, taskTtitle뿐이다. 화면에 그려줄!

```jsx
export default function App() {
  const [state, setState] = useState(initialState);

  const { tasks, taskTitle } = state;

  function handleChangeTitle(e) {
    setState(updateTaskTitle(state, e.target.value));
  }

  function handleClickAddTask() {
    setState(addTask(state));
  }

  function handleClickDeleteTask(id) {
    setState(deleteTask(state, id));
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

이렇게 분리한 App 컴포넌트를 보면 task를 어떻게 추가하는지, 어떻게 삭제하는지, 어떻게 title을 관리하는지는 모른다.

하지만 상관없다! App 컴포넌트는 그냥 화면에 어떻게 보이는지에만 집중하기 때문이다!

**그렇다면 외부에 분리해놓은 상태들을 보자**

초기상태

```jsx
const initialState = {
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
};
```

상태 관리

```jsx
function updateTaskTitle(state, taskTitle) {
  return {
    ...state,
    taskTitle,
  };
}

function addTask(state) {
  const { newId, tasks, taskTitle } = state;

  return {
    ...state,
    newId: newId + 1,
    taskTitle: '',
    tasks: [...tasks, { id: newId, title: taskTitle }],
  };
}

function deleteTask(state, id) {
  const { tasks } = state;

  return {
    ...state,
    tasks: tasks.filter((task) => task.id !== id),
  };
}
```

이렇게 초기상태는 어떻고, 상태 관리는 어떻게 하는지는 나와있지만 화면에 어떻게 그려질지는 모른다
이렇게 상태를 관리하는 것과, 화면에 그려줄 부분이 완벽하게 분리된 것을 확인할 수 있다
기존에도 Page, List, Item 등 컴포넌트에서 상태를 어떻게 관리하는지 몰라도 화면 구현에 전혀 문제가 없었다
그런 부분처럼 한단계 더 나아가서 App 컴포넌트도 어떻게 상태를 관리하는지 몰라도 된다.
처리하는 것들을 App 컴포넌트 내부에서 통합을 해주기는 한다

즉, handleChangeTitle, handleClickAddTask, handleClickDeleteTask이 화면에 어떻게 결합이 되어야 하는지는 알고 있다는 소리이다.
어떻게 연결하는지는 알고 있지만 어떻게 굴러가는지는 모른다는 뜻이다.

**이것이 가장 중요한 관심사의 분리이다!**
그렇게 하면 각각의 모듈화를 하여 각각의 관심사 나는 List만 보는 애야, 나는 Input만 보는 애야! 하고 분리를 해놓는다면 관리 및 수정도 훨씬 쉽게 이루어질 것이다.

**관심사를 분리한 후에 앱이 잘 구현되는지 확인해보자**
잘 구현되는 것을 확인할 수 있다.
즉, 관심사를 분리를 해도 구현에 무리가 없는 것이다. 그냥 있던 것을 위치만 바꾼 것이니까!

**그런데 이렇게 분리한 상태들을 조금 더 효율적으로 독립된 형태로 쓰고 싶다면 어떻게 해야할까?**
그럴 때 쓰는 것이 바로 Redux이다!

### **Redux 설치**

```jsx
npm i redux react-redux
```

리덕스 자체는 리액트에 종속적이지 않다. 라이브러리일뿐!

redux와 react-redux를 함께 설치해야 사용이 가능하다.

### **3가지 원칙**

[3가지 원칙 - 리덕스 공식 문서](https://redux.js.org/introduction/three-principles)에서는 리덕스를 사용할 때 따라야 할 3가지 원칙을 제시한다.

- 전체 상태값을 하나의 객체에 저장한다.
- 상태값은 불변 객체다.
- 상태값은 순수 함수에 의해서만 변경되어야 한다.

### **Action**

- [Actions](https://redux.js.org/basics/actions)

상태값은 오직 액션 객체에 의해서만 변경되어야 한다.

- type: (string)
- payload(object): { taskTitle }

액션 객체에는 type, payload 속성으로 구성되는데 type은 어떤 액션인지 구별할 수 있는 문자열 값이며 payload 안에는 변경할 상태값(불변 객체)이 전달된다.

**Redux에서 상태값을 수정하는 유일한 방법은 액션 객체와 함께 dispatch 메서드를 호출하는 것!.**

### **Reducer**

- [Reducers](https://redux.js.org/basics/reducers)

리덕스에선 기존 상태를 다른 상태로 변경하는 함수를 리듀서(reducer)라고 한다.

reducer의 구조는 다음과 같다.

```jsx
function reducer(state, action) {
  // ...

  return state;
}
```

reducer는 이전 상태값과 액션 객체를 입력으로 받아서 새로운 상태값을 만드는 [순수 함수](https://en.wikipedia.org/wiki/Pure_function)이다.

순수 함수는 부수 효과(함수 외부의 상태를 변경시키는 것)를 발생시키지 않아야 한다. 순수 함수는 같은 입력값에 대해 항상 같은 값을 반환한다.

### **Store**

- [Store](https://redux.js.org/basics/store)

store는 리덕스의 상태값을 갖는 객체

액션의 발생은 store의 dispatch 메서드로 시작된다.

스토어는 액션이 발생하면 미들웨어 함수를 실행하고, reducer를 실행해서 상태값을 새로운 값으로 변경한다.

**첫 번째 원칙에서 말한 애플리케이션의 전체 상태값을 저장하는 하나의 객체가 바로 store!**

### **Provider**

- [Provider](https://react-redux.js.org/api/provider)

문서는 무언가 복잡한 설명을 하는 것 같다.

단순하게 정리하면 React로 작성된 컴포넌트들을 Provider안에 넣으면 하위 컴포넌트들이 Provider를 통해 redux store에 접근이 가능해진다.

### **react-redux hook**

- [Hooks](https://react-redux.js.org/api/hooks#usedispatch)

Hook 등장 이전에는 `mapDispatchToProps`, `mapStateToProps` 와 `connect` 라는 함수를 이용해서 상당히 많은 코드를 작성해야 했으나 Redux에서도 Hook을 제공하면서 상당히 깔끔한 코드를 작성할 수 있게 되었다.

Redux Hook 중 가장 중요한 Hook 은 `useDispatch` 와 `useSelector`이다

- [useDispatch와 useSelector](https://medium.com/@pks2974/redux-hook-%EC%82%B4%ED%8E%B4%EB%B3%B4%EA%B8%B0-3b92b4d75466)

기존에는 상태를 한 곳에서 관리했다면 이제는 Redux에서 모든 상태를 관리하게 된다

### 리덕스를 이용한 관심사 분리

설치를 마쳤다면 리덕스를 이용하여 관심사 분리를 해보자
먼저 store를 만들어보자

→ src 폴더 안에 생성

```jsx
import { createStore } from 'redux';

const store = createStore;

export default store;
```

store을 만들게 되면 맨처음 reducer를 받게 되어있다.

**그렇다면 reducer는 무엇일까?** 한 상태를 다른 상태로 바꾸어주는 것을 reducer라고 한다.

예를 들어서 App이 있다고 가정해보자.

```jsx
function updateTaskTitle(state, taskTitle) {
  return {
    ...state,
    taskTitle,
  };
}
```

여기에서 state(이전 상태)를 받아서 새로운 상태를 return해주는데 이것을 한번에 묶어놓은 것을 reducer라고 한다.
그러므로 reducer에는 상태를 바꿔주어야 하기 때문에 최초의 상태가 필요하다.

**최초의 상태, 초기값을 한번 추가해보자**

```jsx
import { createStore } from 'redux';

const reducer = () => {
  return {};
};

const store = createStore(reducer);

export default store;
```

→ 일단은 먼저 돌아가는지 확인을 위해 빈값을 넣어서 확인해보자

**index.jsx**

```jsx
import React from 'react';
import ReactDOM from 'react-dom';

import { Provider } from 'react-redux';

import App from './App';

import store from './store';

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('app'),
);
```

store가 있다면 어떠한 상태를 관리할 것이고, reducer를 통해서 상태를 바꾸는 것도 할 것이다.
리액트에서는 이 store를 가볍게 잡아줄 수 있도록 Provider를 사용하여 감싸준다.
Provider에서는 store을 전달해야 한다.
이와 같이 세팅을 마치면 index 하위에 있는 App 컴포넌트에서도 상태를 가져와서 사용을 할 수 있다.

**어떻게? useDispatch, useSelector를 활용해서!**

**App.jsx**

기존에는 상태 관리를 컴포넌트가 해왔었다.
그러나 컴포넌트가 하지 않고 useSelector를 이용해서 필요한 상태를 받아오게 변경해보자.

```jsx
const state = useSelector((state) => state);
```

state를 useSelector를 이용해서 taskTitle, tasks를 받아오게 시켰다.
그렇다면 왜 useSelector일까? 필요한 것을 골라오기 때문이다

```jsx
const { tasks, taskTitle } = useSelector((state) => ({
  taskTitle: state.taskTitle,
  tasks: state.tasks,
}));
```

→ 즉, 이렇게 기존에 있는 state에서 필요한 값을 가져오는 것이다.

```jsx
const [state, setState] = useState(initialState);
const { tasks, taskTitle } = state;
```

기존에는 이렇게 초기값을 state에 담아 state에서 구조분해할당으로 받아오던 것을 useSelector을 이용해서 필요한 값을 받아오는 것!
그렇다면 이제 useSelector로 받아온 것을 useDispatch를 이용하여 받아오도록 해보자!

```jsx
const dispatch = useDispatch();

function handleChangeTitle(e) {
  dispatch(updateTaskTitle(state, e.target.value));
}

function handleClickAddTask() {
  dispatch(addTask(state));
}

function handleClickDeleteTask(id) {
  dispatch(deleteTask(state, id));
}
```

→ 기존에는 setState를 받아와서 값을 변경해주거나 더해주거나 하였지만 이제는 dispatch를 이용하여 원하는 값을 받아 와서 액션을 하게 해주는 것!

**그렇다면 아래의 예시에서 액션은 어떤 것일까?**

```jsx
function handleChangeTitle(e) {
  dispatch(updateTaskTitle(state, e.target.value));
}
```

→ `updateTaskTitle(state, e.target.value)`이다

/ `updateTaskTitle`은 액션을 만들어주는 것

즉, updateTaskTitle, addTask, deleteTask는 action creator이다

**그렇다면 액션을 할 수 있게 액션 크리에이터로 할 수 있게 하려면 어떻게 해야할까?**

store.js의 reducer 형식을 바꾸어야 한다

```jsx
function reducer(state, action) {
  if(action is updateTaskTitle) {
    return {
      ...state,
      taskTitle
    }
  }
};
```

→ state와 action을 받아서 그 action creator에 맞게 action을 주게 하면 된다.

### Redux의 action에는 룰이 있다

- type이라는 string을 활용한다

```jsx
if(action.type === 'updateTaskTitle')
```

- payload를 이용하여 값을 사용한다.(내가 원하는대로 taskTitle 등 사용해도 상관은 없다.)
  object이기 때문에 { taskTitle, tasks } 이러한 형식으로 사용해야 한다

```jsx
if(action.type === 'updateTaskTitle') {
  return {
  ...state,
  taskTitle: action.payload.taskTitle,
}
```

→ 즉, action type은 updateTaskTitle일 때 이 상태를 돌려주고, 아닐 경우에는 기존 값을 돌려주게 하면 된다.

**그렇다면 App의 updateTaskTitle을 보자**

```jsx
function handleChangeTitle(e) {
  dispatch(updateTaskTitle(state, e.target.value));
}
```

이것이 즉,

```jsx
dispatch({
  type: 'updateTaskTitle',
  payload: {
    taskTitle: e.target.value,
  },
});
```

이렇게 받아오고 액션을 취해야 한다는 뜻이다.
결국 App 컴포넌트 외부로 분리했던 updateTaskTitle 함수에 이 내용을 넣으면 끝!

```jsx
function updateTaskTitle(taskTitle) {
  return {
    type: 'updateTaskTitle',
    payload: { taskTitle },
  };
}
```

```jsx
function handleChangeTitle(e) {
  dispatch(updateTaskTitle(e.target.value));
}
```

→ state는 reducer에서 알아서 하니까 따로 필요가 없기 때문에 삭제
taskTitle의 값은 handleChangeTitle에서 e.target.value를 넣으면 된다
즉, 이제는 기존 상태에 대해서 일 필요조차도 없어진 것이다!
기존 상태가 어떻게 됐는진 모르겠는데 일단 e.target.value를 updateTaskTitle이라는 action creator한테 넘겨서 값을 바꿀거야

만약 조금 더 코드를 줄이고 싶다면 아래와 같이 변경도 가능하다

```jsx
return (
  <Page
    onChangeTitle={(e) => (dispatch(updateTaskTitle(e.target.value))}
  />
);
```

그러나 코드가 복잡해지면 가독성이 떨어지기 때문에 추천하지는 않는다

**이제 나머지도 한번 변경을 해보자**

```jsx
function handleClickAddTask() {
  dispatch(addTask());
}

function handleClickDeleteTask(id) {
  dispatch(deleteTask(id));
}
```

→ 일단 1차적으로 나머지 action creator들도 현재 상태를 알 필요가 없기 때문에 일괄적으로 state를 지워주자

그다음 reducer에 각각의 조건을 더 추가해주자

**store.js**

```jsx
if (action.type === 'addTask') {
  const { newId, tasks, taskTitle } = state;

  return {
    ...state,
    newId: newId + 1,
    taskTitle: '',
    tasks: [...tasks, { id: newId, title: taskTitle }],
  };
}
```

```jsx
function addTask() {
  return {
    type: 'addTask',
  };
}
```

→ 새롭게 만들어낸 것이 없기 때문에 기존 addTask가 가지고 있던 값들을 그대로 reducer에 넣으면 끝

```jsx
if (action.type === 'deleteTask') {
  const { tasks } = state;

  return {
    ...state,
    tasks: tasks.filter((task) => task.id !== action.payload.id),
  };
}
```

```jsx
function deleteTask(id) {
  return {
    type: 'deleteTask',
    payload: {
      id,
    },
  };
}
```

→ id만 action.payload.id로 변경하여 reducer가 관리하는 곳의 아이디를 가져와 확인하게 하면 된다!

**이제 App 컴포넌트에서 설정하였던 기본값을 store의 reducer에게 넘겨주자!**

```jsx
const initialState = {
  newId: 100,
  taskTitle: '',
  tasks: [
    {
      id: 1,
      title: '아무것도 하지 않기 #1',
    },
    {
      id: 2,
      title: '아무것도 하지 않기 #2',
    },
  ],
};

function reducer(state = initialState, action) {
  /* 생략 */
}
```

이렇게 상태를 관리할 수 있는 reducer을 생성하여 모두 넘기고 App 컴포넌트에서는 화면에 출력되는 구현되는 부분만 남기니 훨씬 더 App 컴포넌트가 깔끔해진 것을 볼 수 있다.

**App.jsx**

```jsx
import React from 'react';

import { useSelector, useDispatch } from 'react-redux';

import Page from './Page';

function updateTaskTitle(taskTitle) {
  return {
    type: 'updateTaskTitle',
    payload: { taskTitle },
  };
}

function addTask() {
  return {
    type: 'addTask',
  };
}

function deleteTask(id) {
  return {
    type: 'deleteTask',
    payload: {
      id,
    },
  };
}

export default function App() {
  const { tasks, taskTitle } = useSelector((state) => ({
    taskTitle: state.taskTitle,
    tasks: state.tasks,
  })); // 상태 관리하는 것 중 필요한 것만 store에서 빼온다

  const dispatch = useDispatch();

  function handleChangeTitle(e) {
    dispatch(updateTaskTitle(e.target.value));
  }

  function handleClickAddTask() {
    dispatch(addTask());
  }

  function handleClickDeleteTask(id) {
    dispatch(deleteTask(id));
  }

  // 상태를 업데이트하는 것에 대해서 직접 관리하지 않고 어딘가에 있는 것으로 redux에게 넘겨주자!
  // 값은 몰라도 돼! redux가 알아서 할테니까

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

**store.js**

```jsx
import { createStore } from 'redux';

const initialState = {
  newId: 100,
  taskTitle: '',
  tasks: [
    {
      id: 1,
      title: '아무것도 하지 않기 #1',
    },
    {
      id: 2,
      title: '아무것도 하지 않기 #2',
    },
  ],
};

function reducer(state = initialState, action) {
  // 맨처음에는 아무것도 없는 값이 넘어갈 수 있게 초기값 설정
  if (action.type === 'updateTaskTitle') {
    return {
      ...state,
      taskTitle: action.payload.taskTitle,
    };
  }

  if (action.type === 'addTask') {
    const { newId, tasks, taskTitle } = state;

    return {
      ...state,
      newId: newId + 1,
      taskTitle: '',
      tasks: [...tasks, { id: newId, title: taskTitle }],
    };
  }

  if (action.type === 'deleteTask') {
    const { tasks } = state;

    return {
      ...state,
      tasks: tasks.filter((task) => task.id !== action.payload.id),
    };
  }

  return state;
}

const store = createStore(reducer);

export default store;
```

### 이제 action creator들도 다른 곳에 분리하여 하나로 만들어두자!

actions.js를 새롭게 만들어 action creator를 옮겨보자

```jsx
export function updateTaskTitle(taskTitle) {
  return {
    type: 'updateTaskTitle',
    payload: { taskTitle },
  };
}

export function addTask() {
  return {
    type: 'addTask',
  };
}

export function deleteTask(id) {
  return {
    type: 'deleteTask',
    payload: {
      id,
    },
  };
}
```

그 후 App 컴포넌트에서는 import하여 사용하면 조금 더 App이 가벼워진 것을 확인할 수 있다

```jsx
*import* { updateTaskTitle, addTask, deleteTask } *from* './actions';
```

이제 App을 보면 조금 더 명확하게 관심사와 맡은 일을 알게 되었다.

```jsx
import React from 'react';

import { useSelector, useDispatch } from 'react-redux';

import Page from './Page';
import { updateTaskTitle, addTask, deleteTask } from './actions'; // 이 앱이 총 세 가지 일을 할거야 업데이트, 추가, 삭제

export default function App() {
  const { tasks, taskTitle } = useSelector((state) => ({
    taskTitle: state.taskTitle,
    tasks: state.tasks,
  })); // 이 앱이 가져오는 형태들은 taskTitle과 tasks가 있구나

  const dispatch = useDispatch();

  function handleChangeTitle(e) {
    dispatch(updateTaskTitle(e.target.value));
  }

  function handleClickAddTask() {
    dispatch(addTask());
  }

  function handleClickDeleteTask(id) {
    dispatch(deleteTask(id));
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
const { tasks, taskTitle } = useSelector((state) => ({
  taskTitle: state.taskTitle,
  tasks: state.tasks,
}));
```

이 부분은 useSelector 안에 있는 것이 함수기 때문에 너무 복잡하다 싶으면 다음과 같이 분리해서 코드를 정리해줄 수 있다.

```jsx
function selector(state) {
  return {
    taskTitle: state.taskTitle,
    tasks: state.tasks,
  };
}

export default function App() {
  const { tasks, taskTitle } = useSelector(selector);
  /* 생략 */
}
```

**이렇게 관심사 분리 완료하면 test 파일에서 오류가 나는 것을 확인할 수 있다**

![](https://velog.velcdn.com/images/yxxnhx/post/183a5495-ddb2-4715-b3f9-5b8897deaa6b/image.png)
→ 바로 Provider를 못 읽어온다는 에러인데 test 파일에는 redux를 설정해주지 않아서 일어난 에러이다.

**그렇다면 어떻게 해결할 수 있을까?**
총 두가지 방법이 있다.

### 관심사 분리로 인한 테스트 에러 해결 방법

#### 1. index와 동일하게 Provider로 감싸주는 방법

```jsx
test('App', () => {
  const { getByText } = render(
    <Provider store={store}>
      <App />
    </Provider>,
  );

  expect(getByText(/추가/)).not.toBeNull();
  expect(getByText(/아무것도 하지 않기 #1/)).not.toBeNull();
});
```

#### 2. test에게 useSelector, useDispatch를 가짜로 방법

가짜로 만드는 것을 test에서는 mock이라고 한다.
src와 동일한 위치에 **mocks** 폴더를 만들어서 그 안에 가짜함수를 만들어주자.

**react-redux.js**

```jsx
export const useDispatch = jest.fn();
export const useSelector = jest.fn();
```

→ 이렇게 하면 전체적인 mock이 생긴 것이다.
이전에 테스트할 때 핸들러를 가짜 함수를 통해 확인하였듯이 방법은 동일하다
**그 후에 다시 App.test.jsx로 넘어가서 설정을 해주면 끝!**

```jsx
jest.mock('react-redux');
```

경로를 import 해주듯이 위와 같이 설정을 해주게 되면 jest mock이 뒤에 있는 이름을 보고 파일을 전체적으로 스캔을 해서 이 이름을 가진 파일을 찾아내 그 안에 있는 가짜 함수들을 사용하게 된다.
그렇게 되면 그 안에 있는 가짜 함수인채로 test가 작동을 하게 되므로 아까 Provider을 읽지 못한다는 에러와는 다른 에러가 나는 것을 확인할 수 있다.

![](https://velog.velcdn.com/images/yxxnhx/post/a5317ec0-a42d-4ad3-bf16-4dbdeeae4827/image.png)

→ 니가 넣은 값을 못 읽겠는데? 정의 되지 않았어 하는 에러가 나는 것을 볼 수 있다.

**왜이럴까?**

왜냐하면 실행 가능한 함수를 넣어야 하는데 가짜 함수를 그냥 넣었기 때문이다!
그렇다면 진짜로 실행이 가능할 수 있도록 mocks에 설정을 해보자.

```jsx
*export* const useSelector = jest.fn((selector) => selector({}));
```

일단 먼저 useSelector에게 selector를 받아서 실행을 하게 할건데 빈값을 넣어보자.

![](https://velog.velcdn.com/images/yxxnhx/post/6bc33362-2333-46f6-9a3d-97c617d0c7d6/image.png)

→ 이제는 정의되지 않았다는 에러가 아닌 그런거 없는데? 길이가 없어 라는 에러가 나는 것을 볼 수 있다.

**그렇다면 이제 가짜로 한번 값을 넣어줘보자!**

```jsx
export const useSelector = jest.fn((selector) =>
  selector({
    tasks: [],
  }),
);
```

![](https://velog.velcdn.com/images/yxxnhx/post/92630804-d43d-4672-a0b4-7c90b1066ee6/image.png)

→ 이렇게 되면 값을 찾을 수 없다는 에러만 뜰뿐 아까와 같은 length를 못 읽는다거나 Provider가 없다는 등의 에러가 사라지는 것을 확인할 수 있다.
원래대로라면 App으로 돌아가서 초기값을 확인하러 가겠지만 관심사를 분리하여서 더이상 App에서는 초기값을 확인할 수 없다.

**store에 넣어둔 초기값을 일단은 mocks에 넣어줘보자.**

```jsx
export const useSelector = jest.fn((selector) =>
  selector({
    tasks: [
      {
        id: 1,
        title: '아무것도 하지 않기 #1',
      },
      {
        id: 2,
        title: '아무것도 하지 않기 #2',
      },
    ],
  }),
);
```

이렇게 하면 이제 정상적으로 test도 통과하는 것을 확인할 수 있다.

**이제 그러면 App.test.jsx에서도 useSelector를 꺼내와서 사용이 가능하다!**
한번 test 파일에서 useSelector를 import 해보자

```jsx
*import* { useSelector} *from* 'react-redux'

jest.mock('react-redux');
```

이렇게 되면 가짜로 설정을 해두었으니까 mocks안에 있는 가짜 useSelctor를 꺼내와서 쓸 수 있는 것이다.

**이제 가져온 useSelector을 이용하여 test를 조작해보자**

mocks에서 임시로 넣어둔 초기값을 지우고 test 파일 내에서 useSelector를 이용하여 지정한 초기값 tasks를 가져와서 올바르게 작동하도록 해보자

```jsx
*export* const useSelector = jest.fn((selector) => selector({}));
```

```jsx
test('App', () => {
  const tasks = [
    {
      id: 1,
      title: '아무것도 하지 않기 #1',
    },
    {
      id: 2,
      title: '아무것도 하지 않기 #2',
    },
  ];

  useSelector.mockImplementation((selector) =>
    selector({
      tasks,
    }),
  );

  const { getByText } = render(<App />);

  expect(getByText(/추가/)).not.toBeNull();
  expect(getByText(/아무것도 하지 않기 #1/)).not.toBeNull();
});
```

→ 이제 정상적으로 테스트가 통과되는 것을 확인할 수 있다.
이제 이렇게 진짜 상태가 아닌 테스트를 하기 위한 가짜 상태를 가지고 테스트를 진행할 수 있으므로 기존에 계속 사용하던 초기값을 지울 수 있다.
왜냐하면 계속 앱을 들어가면 초기값이 떠서 완벽하지 않았기 때문!

**store.js**

```jsx
const initialState = {
  newId: 100,
  taskTitle: '',
  tasks: [],
};
```

이렇게 관심사별로 분리를 해두었으니 테스트를 돌리기에 조금 더 용이해진 상태가 되었다.
그렇다면 한번 관심사별 테스트를 진행해보자!

**제일 먼저 store에 있는 핵심 기능인 reducer를 분리해보자.**

```jsx
const initialState = {
  newId: 100,
  taskTitle: '',
  tasks: [],
};

export default function reducer(state = initialState, action) {
  if (action.type === 'updateTaskTitle') {
    return {
      ...state,
      taskTitle: action.payload.taskTitle,
    };
  }

  if (action.type === 'addTask') {
    const { newId, tasks, taskTitle } = state;

    return {
      ...state,
      newId: newId + 1,
      taskTitle: '',
      tasks: [...tasks, { id: newId, title: taskTitle }],
    };
  }

  if (action.type === 'deleteTask') {
    const { tasks } = state;

    return {
      ...state,
      tasks: tasks.filter((task) => task.id !== action.payload.id),
    };
  }

  return state;
}
```

```jsx
import { createStore } from 'redux';

import reducer from './reducer';

const store = createStore(reducer);

export default store;
```

### reducer test

**이제 reducer에 대한 test를 만들어보자**

#### 1. updateTaskTitle

```jsx
describe('reducer', () => {
  describe('updateTaskTitle', () => {
    const state = reducer(previousState, action);

    expect(state.taskTitle).toBe('New Title');
  });
});
```

일단 updateTaskTilte을 생각해보면 현재 상태와 action을 받아오는 것이니 일단 틀을 먼저 잡아줘보자.
그런 후에 현재값인 previousState를 빈값으로 넣고 action의 형태를 가져와서 테스트를 돌려보자

```jsx
describe('reducer', () => {
  describe('updateTaskTitle', () => {
    it('returns new state with new task title', () => {
      const previousState = {
        taskTitle: '',
      };

      const action = {
        type: 'updateTaskTitle',
        payload: {
          taskTitle: 'New Title',
        },
      };

      const state = reducer(previousState, action);

      expect(state.taskTitle).toBe('New Title');
    });
  });
});
```

action에는 type과 payload 두 가지 요소가 필요하다.
이렇게 넣어서 테스트를 돌려보니 정상적으로 테스트 통과되는 것을 확인할 수 있다.
그러나 이렇게 직접 하나하나 넣다보면 형식을 실수하거나 값을 잘못 넣어 실수로 에러가 나는 경우가 있다.
이를 방지하기 위해 actions에서 import하여 사용해보자.

```jsx
import { updateTaskTitle, addTask, deleteTask } from './actions';

describe('reducer', () => {
  describe('updateTaskTitle', () => {
    it('returns new state with new task title', () => {
      const previousState = {
        taskTitle: '',
      };

      const state = reducer(previousState, updateTaskTitle('New Title'));

      expect(state.taskTitle).toBe('New Title');
    });
  });
});
```

**그렇다면 previousState를 굳이 뺄 필요가 있을까?**

```jsx
describe('reducer', () => {
  describe('updateTaskTitle', () => {
    it('returns new state with new task title', () => {
      const state = reducer(
        {
          taskTitle: '',
        },
        updateTaskTitle('New Title'),
      );

      expect(state.taskTitle).toBe('New Title');
    });
  });
});
```

훨씬 더 코드가 간결하고 깨끗해진 것을 볼 수 있다.

#### 2. addTask

```jsx
describe('addTask', () => {
  it('returns new state with new task', () => {
    const state = reducer(
      {
        taskTitle: 'New Task',
        tasks: [],
      },
      addTask('New Title'),
    );

    expect(state.tasks).toHaveLength(1);
  });
});
```

→ addTask를 활용해서 길이가 1이 된 것을 확인하는 테스트 코드를 작성할 수 있다.

여기에서 또 하나 테스트를 할 수 있다.

```jsx
expect(state.tasks[0].title).toBe('New Task');
```

바로 지정하였던 task title이 입력한 값이 제대로 들어오는지 여부이다.

**그렇다면 이제 또 다른 조건으로 테스트를 돌려보자.**
바로 새로운 값을 입력하고 추가를 하면 task title 입력창이 비어지게 만드는 상황이다

```jsx
it('returns new state with blank task title', () => {
  const state = reducer(
    {
      taskTitle: 'New Task',
      tasks: [],
    },
    addTask(''),
  );

  expect(state.taskTitle).toBe('');
});
```

그런데 여기서 계속 테스트가 return ~ new ~로 이어지게 되다보니 테스트명을 조금 더 간결하고 명확하게 변경을 해보자

```jsx
describe('reducer', () => {
  describe('updateTaskTitle', () => {
    it('changes new task title', () => {
      const state = reducer(
        {
          taskTitle: '',
        },
        updateTaskTitle('New Title'),
      );

      expect(state.taskTitle).toBe('New Title');
    });
  });
  describe('addTask', () => {
    it('appends a new task into tasks', () => {
      const state = reducer(
        {
          taskTitle: 'New Task',
          tasks: [],
        },
        addTask('New Title'),
      );

      expect(state.tasks).toHaveLength(1);
      expect(state.tasks[0].title).toBe('New Task');
    });

    it('clears task title', () => {
      const state = reducer(
        {
          taskTitle: 'New Task',
          tasks: [],
        },
        addTask(''),
      );

      expect(state.taskTitle).toBe('');
    });
  });
});
```

이렇게 쭉 확인을 해보다보니 그렇다면 task가 없으면 어떻게 되는지에 대한 테스트의 부재를 눈치챘을 것이다.

일단 context를 활용하여 두 개의 상황을 나누어보자

```jsx
describe('addTask', () => {
  context('with task title', () => {
    it('appends a new task into tasks', () => {
      const state = reducer(
        {
          taskTitle: 'New Task',
          tasks: [],
        },
        addTask('New Title'),
      );

      expect(state.tasks).toHaveLength(1);
      expect(state.tasks[0].title).toBe('New Task');
    });

    it('clears task title', () => {
      const state = reducer(
        {
          taskTitle: 'New Task',
          tasks: [],
        },
        addTask(''),
      );

      expect(state.taskTitle).toBe('');
    });
  });
});
```

→ task title이 있을 시에 정상적으로 구현되는 상황

그리고 task title이 없는데 addTask를 호출한 상황을 테스트로 돌려보자

```jsx
    context('without task title', () => {
      it("doesn't work", () => {
        const state = reducer(
          {
            taskTitle: '',
            tasks: [],
          },
          addTask(),
        );

        expect(state.tasks).toHaveLength(0);
      });
```

아무 일도 일어나지 않는다는 것을 보여주기 위해 tasks의 길이가 0이라고 테스트를 돌렸으나 아래와 같이 에러가 나는 것을 확인할 수 있다.

![](https://velog.velcdn.com/images/yxxnhx/post/b19d833d-00ed-42c2-b625-f1fcdcd961e0/image.png)

→ 바로 현재 우리가 테스트하고 있는 것에 id가 빠져있어서 id는 undefined인 것으로 길이가 잡혔기 때문에 일어나는 에러이다.

그렇다면 어떻게 해결해야 할까?

**먼저 reducer에서 taskTitle이 없을 경우의 케이스를 if문을 활용하여 추가를 해주자**

```jsx
if (action.type === 'addTask') {
  const { newId, tasks, taskTitle } = state;

  if (!taskTitle) {
    return state;
  }
  /* 생략 */
}
```

→ taskTitle이 없을 경우에는 기본값을 내뱉어서 아무일도 일어나지 않음을 설정

**그렇다면 addTask에서 하나의 expect를 더 추가해줄 수 있지 않을까?**

```jsx
expect(state.tasks[0].id).not.toBeUndefined();
```

task의 아이디가 undefined가 아니라는 테스트를 추가하면 아래와 같은 에러가 뜨는 것을 확인할 수 있다.

![](https://velog.velcdn.com/images/yxxnhx/post/6a9cc417-848a-4a7d-9a34-e9f621e3477f/image.png)

→ 왜냐하면 기본값에 newId를 지정해주지 않았기 때문에 받은 id가 없기 때문에 일어나는 에러이다.

```jsx
const state = reducer(
  {
    newId: 100,
    taskTitle: 'New Task',
    tasks: [],
  },
  addTask('New Title'),
);
```

이와 같이 newId: 100으로 기본값을 설정해주니 정상적으로 테스트가 통과하는 것을 확인할 수 있다.

**이제 통과가 되었으니 중복을 확인해보자.**

```jsx
const state = reducer(
  {
    newId: 100,
    taskTitle: 'New Task',
    tasks: [],
  },
  addTask('New Title'),
);
```

지금 공통적으로 이 부분이 계속해서 쓰이는 것을 확인할 수 있다.

그렇다면 이 부분을 외부로 빼서 사용하면 조금 더 코드가 간결해지지 않을까?

```jsx
function reduceAddTask(taskTitle) {
  return reducer(
    {
      newId: 100,
      taskTitle,
      tasks: [],
    },
    addTask(taskTitle),
  );
}
```

```jsx
describe('addTask', () => {
  function reduceAddTask(taskTitle) {
    return reducer(
      {
        newId: 100,
        taskTitle,
        tasks: [],
      },
      addTask(taskTitle),
    );
  }

  context('with task title', () => {
    it('appends a new task into tasks', () => {
      const state = reduceAddTask('New Task');

      expect(state.tasks).toHaveLength(1);
      expect(state.tasks[0].id).not.toBeUndefined();
      expect(state.tasks[0].title).toBe('New Task');
    });

    it('clears task title', () => {
      const state = reduceAddTask('New Task');

      expect(state.taskTitle).toBe('');
    });
  });

  context('without task title', () => {
    it("doesn't work", () => {
      const state = reduceAddTask('');

      expect(state.tasks).toHaveLength(0);
    });
  });
});
```

이와 같이 외부로 함수로 빼서 아래의 조건들에 맞게 설정을 해주면 조금더 코드가 간결해진 것을 확인할 수 있다.
이런 식으로 이제 우리가 확인하고 싶은 상태들을 테스트에 넣어서 확인을 해주면 쉬워진다.
또는 어떠한 새로운 기능이 추가되면 테스트를 먼저 하고 통과하도록 만들어주면 된다.
그렇지 않으면 먼저 만들고 다시 브라우저로 가서 각각의 케이스를 눌러보고 또 안되면 어디서 안되는지 찾아내고 하는 등의 번거로움이 사라질 수 있게 되는 것이다.

#### 3. deleteTask

```jsx
describe('deleteTask', () => {
    it('remove the task from tasks', () => {
      const state = reducer(
        {
          tasks: [{ id: 1, title: '' }],
        },
        deleteTask(1),
      );

      expect(state.tasks).toHaveLength(0);
    });
```

→ id를 찾아서 해당 아이디를 지우면 tasks의 길이는 0이 된다는 테스트를 작성하면 된다.

이것 또한 id가 존재할 때, 존재하지 않을 때를 나누어서 테스트를 더 추가할 수도 있다.

```jsx
describe('deleteTask', () => {
  context('with existed task ID', () => {
    it('remove the task from tasks', () => {
      const state = reducer(
        {
          tasks: [{ id: 1, title: '' }],
        },
        deleteTask(1),
      );

      expect(state.tasks).toHaveLength(0);
    });
  });

  context('without existed task ID', () => {
    it("doesn't work", () => {
      const state = reducer(
        {
          tasks: [{ id: 1, title: '' }],
        },
        deleteTask(1000),
      );

      expect(state.tasks).toHaveLength(1);
    });
  });
});
```

이와같이 미심쩍은 케이스를 추가해서 테스트를 진행하면 된다

이렇게 작성하면 테스트가 정상적으로 통과되는 것을 확인할 수 있다

일종의 사용법처럼 updateTaskTitle은 title을 변경해주고, addTask는 taskTitle이 있을 때와 없을 때가 나뉘어서 작동되는구나 등을 간단히 얻을 수 있게 된다.

**이제는 전체적인 컴포넌트들을 조금 더 세밀하게 관심사를 중심으로 바꿔보고 리팩터링하는 시간을 가져보자!**

### 리팩터링

## **React에서 Redux 사용하기**

- [Usage with React](https://redux.js.org/basics/usage-with-react)

리액트에서는 컴포넌트를 크게 두가지로 나누어서 설명하고 있다.

**Presentational Components**

- redux를 알지 못함.
- 보이는 것(View)에만 집중함.
  → 여태까지 해온 리액트가 주로 프레젠테이션 컴포넌트에 가깝다고 볼 수 있다. 어떻게 보이는가에 더 집중을 하고 있었기 때문이다.
  todo를 예시로 생각하면 app을 제외한 나머지 컴포넌트들은 모두 상태를 관리하기 보다 화면에 구현될 모습들만 가지고 있기 때문이다.

**Container Components**

- redux를 알고 있음.
- 데이터를 가져오고 상태를 업데이트함.

**그렇다면 한번 현재 우리의 App 컴포넌트를 보자**

```jsx
export default function App() {
  /* 생략 */
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

현재 Page에 여러 개의 관심사가 모여있는 것을 알 수 있다.
입력에 대한 처리들, 목록에 대한 처리들이 한꺼번에 모여있다.

**그렇다면 만약, 이 두 개의 관심사를 또 다시 분리할 수 있다면 어떨까?**
즉, App이라는 것이 전체적인 컨테이너 컴포넌트의 역할을 하지 말고, 또 다른 컨테이너 컴포넌트들을 만들어 관심사에 따라 그들이 처리할 수 있도록 하는 것이다

#### 1. List에 관한 관심사 분리

**ListContainer.jsx 생성**

List 목록 처리 정보들을 가지고 있는 컨테이너 컴포넌트를 만들어 보자
먼저 App 컴포넌트를 그대로 가져와서 수정을 해보도록 하자.

```jsx
export default function ListContainer() {
	const { tasks } = useSelector((state) => ({
    tasks: state.tasks,
  }));

	const dispatch = useDispatch();

  function handleClickDeleteTask(id) {
    dispatch(deleteTask(id))
  }
```

→ ListContainer는 목록만 관심이 있으니 taskTitle, updateTaskTitle, addTask는 필요가 없다.

**그저 목록에 필요한 tasks를 받아오고 그 task를 삭제할 수 있는 deleteTask만 필요할 뿐!**

**이제 여기에 List 컴포넌트를 가져와보자**

```jsx
	return (
    <List
    tasks={tasks}
    onClickDelete={handleClickDeleteTask}
    />
```

이렇게 하면 ListContainer에서는 List를 가지고 와서 보여줄 건데, 보여주면서 리스트에 관련한 상태를 가지고 있는 것이다.

```jsx
import React from 'react';

import Input from './Input';
import ListContainer from './ListContainer';

export default function Page({ taskTitle, onClickAddTask, onChangeTitle }) {
  return (
    <div>
      To-do
      <Input
        value={taskTitle}
        onChange={onChangeTitle}
        onClick={onClickAddTask}
      />
      <ListContainer />
    </div>
  );
}
```

그렇게 하면 이제 Page에는 위와 같이 List 대신 ListContainer를 return해주면 된다.

그렇다면 이제 App 컴포넌트에도 List에 관련된 tasks와 deleteTask들을 삭제해주면 된다.

```jsx
import React from 'react';

import { useSelector, useDispatch } from 'react-redux';

import Page from './Page';
import { updateTaskTitle, addTask } from './actions';

function selector(state) {
  return {
    taskTitle: state.taskTitle,
  };
}

export default function App() {
  const { taskTitle } = useSelector(selector);

  const dispatch = useDispatch();

  function handleChangeTitle(e) {
    dispatch(updateTaskTitle(e.target.value));
  }

  function handleClickAddTask() {
    dispatch(addTask());
  }

  return (
    <Page
      taskTitle={taskTitle}
      onClickAddTask={handleClickAddTask}
      onChangeTitle={handleChangeTitle}
    />
  );
}
```

기존에는 상태를 어느 한곳에 넣게 해주었는데 이제는 리덕스가 상태를 관리해주기 때문에 useDispatch와 useSelector를 가지고 상태를 가져와서 사용할 수 있게 되어 관심사에 따라서 컴포넌트들을 분리하여 사용할 수 있게 되었다.

현재 보고 있는 todo는 단순하기 때문에 한 곳에서 모든 상태를 관리하게 할 수도 있지만 프로젝트 규모에 따라서 다양하고 많은 상태들을 한 곳에서 관리하게 된다면 App 컴포넌트 크기가 커지게 된다.

하지만 redux를 사용하여 관심사에 따라서 적절한 여러개의 컴포넌트로 분배하고 나눌 수 있게 되었다!

**ListContainer에 대한 테스트도 진행해보자**

```jsx
import React from 'react';

import { render } from '@testing-library/react';

import ListContainer from './ListContainer';

import { useSelector } from 'react-redux';

jest.mock('react-redux');

test('ListContainer', () => {
  const tasks = [
    {
      id: 1,
      title: '아무것도 하지 않기 #1',
    },
    {
      id: 2,
      title: '아무것도 하지 않기 #2',
    },
  ];

  useSelector.mockImplementation((selector) =>
    selector({
      tasks,
    }),
  );

  const { getByText } = render(<ListContainer />);

  expect(getByText(/아무것도 하지 않기 #1/)).not.toBeNull();
});
```

이렇게 App의 테스트를 가져와서 필요한 목록만 남겨두면 아래와 같은 에러가 나는 것을 확인할 수 있다.

![](https://velog.velcdn.com/images/yxxnhx/post/6293e529-7b84-4b53-ab02-5cb7645a015c/image.png)

→ Page test에서 tasks를 읽어오지 못하고 있다는 에러이다.

기존에는 App에서만 상태를 관리하였는데 이제는 목록에 관련한 것은 ListContainer에 있으므로 ListContainer까지 받아와서 테스트할 수 있게 했어야 했는데 따로 합쳐주는 작업을 하지 않아서 읽어오지 못하는 에러가 나는 것이다.

**일단은 임시로 useSelector를 활용하여 읽을 수 있도록 넣어주자**

```jsx
import { useSelector} from 'react-redux'

jest.mock('react-redux');

test('Page', () => {
	useSelector.mockImplementation((selector) => selector({
    tasks,
  }))

/* 생략 */

}
```

이와 같이 임시로 설정을 해주니 더이상 에러가 나지 않는 것을 확인할 수 있다.

그리고 추가적으로 ListContainer에서 관리하는 tasks와 deleteTask를 삭제해주자.

#### 2. input에 대한 관심사 분리

먼저 inputContainer의 테스트는 ListContainer과 형식이 비슷하므로 테스트 파일을 먼저 생성하자

```jsx
import React from 'react';

import { render } from '@testing-library/react';

import InputContainer from './InputContainer';

import { useSelector } from 'react-redux';

jest.mock('react-redux');

test('InputContainer', () => {
  useSelector.mockImplementation((selector) =>
    selector({
      taskTitle: 'New Title',
    }),
  );

  const { getByText } = render(<InputContainer />);

  expect(getByText(/추가/)).not.toBeNull();
  expect(getByText(/New Title/)).not.toBeNull();
});
```

→ input에서 확인할 수 있는 taskTitle와 추가 버튼에 대해서만 검사를 진행하면 끝

**InputContainer.jsx 생성**

page에서 가져와서 수정을 해보자

```jsx
import React from 'react';

import Input from './Input';

export default function InputContainer({
  taskTitle,
  onClickAddTask,
  onChangeTitle,
}) {
  return (
    <div>
      To-do
      <Input
        value={taskTitle}
        onChange={onChangeTitle}
        onClick={onClickAddTask}
      />
    </div>
  );
}
```

→ 그러나 우리는 더이상 상태를 { taskTitle, onClickAddTask, onChangeTitle } 와 같이 받아오지 않고 App에 있던 useSelector, useDispatch를 이용해서 꺼내와서 사용해보자

```jsx
import React from 'react';

import Input from './Input';

import { useSelector, useDispatch } from 'react-redux';

import { updateTaskTitle, addTask } from './actions';

export default function InputContainer() {
  const { taskTitle } = useSelector((state) => ({
    taskTitle: state.taskTitle,
  }));

  const dispatch = useDispatch();

  function handleChangeTitle(e) {
    dispatch(updateTaskTitle(e.target.value));
  }

  function handleClickAddTask() {
    dispatch(addTask());
  }

  return (
    <Input
      value={taskTitle}
      onChange={handleChangeTitle}
      onClick={handleClickAddTask}
    />
  );
}
```

이렇게 되면 App 컨테이너에서는 더이상 상태에 관련한 코드들이 필요가 없게 된다.

```jsx
import React from 'react';

import Page from './Page';

export default function App() {
  return <Page />;
}
```

이와 같이 App 컴포넌트가 굉장히 단순해진 것을 확인할 수 있다.

그런데 지금 테스트에서 value에는 받아오고 있지만 getByText에서는 받아오지 못하고 있는 것을 볼 수 있다.

![](https://velog.velcdn.com/images/yxxnhx/post/c51c00e1-ed82-4a17-80d0-894336b7f4d0/image.png)

사실 이 경우에는 value를 받아서 확인하고 대조하는 테스트를 해야 한다.

크게 문제시가 되지 않기 때문에 무시를 하거나 혹은 selector를 이용하여 더 찾아보거나 아니면 fireEvent를 활용하여 변경되는 값이 실제로 일어나는지를 확인할 수 있는 테스트 코드를 완성하면 된다.

**그렇다면 fireEvent를 활용하여 수정해보자**

여기에서 추가 버튼을 누르면 실제로 실행이 되는지를 확인하기 위해 dispatch를 활용해야 한다.

```jsx
import { useSelector, useDispatch } from 'react-redux'

jest.mock('react-redux');

test('InputContainer', () => {
  const dispatch = jest.fn()

  useDispatch.mockImplementation(() => dispatch)
```

먼저 dispatch를 불러와서 사용을 할 것이다.

```jsx
fireEvent.click(getByText(/추가/));
expect(dispatch).toBeCalled();
```

그리고 추가를 누르면 dispatch가 호출이 되는지 확인을 하는 코드를 짤 수 있다.
이렇게 수정하면 더이상 테스트 에러가 나지 않는 것을 확인할 수 있다.

```jsx
expect(dispatch).toBeCalledWith({ type: 'addTask' });
```

여기에서 더 추가적으로 type를 함께 확인하여 디테일한 테스트 코드를 짤 수도 있다.
그런데 만들어놓고 보니 input에서는 그냥 값만 입력하면 되는 것인데 추가를 누르면 그 추가 버튼이 호출되는지까지는 확인할 필요가 없기는 하다

**그렇다면 추가적으로 값을 확인할 수 있는 셀렉터들을 확인해보자**
react testing library를 검색하면 다양한 api, 쿼리들을 확인할 수 있다.
한번 getByDisplayValue를 활용하여 value값을 확인해보자

```jsx
expect(getByDisplayValue(/New Title/)).not.toBeNull();
```

이제 테스트가 정상적으로 통과되는 것을 확인할 수 있다.

이제 그럼 Page 컴포넌트를 정리해보자

```jsx
import React from 'react';

import InputContainer from './InputContainer';
import ListContainer from './ListContainer';

export default function Page() {
  return (
    <div>
      To-do
      <InputContainer />
      <ListContainer />
    </div>
  );
}
```

Page에서도 input 대신에 InputContainer를 import를 해오면 Page도 굉장히 코드가 단순해진다.

```jsx
import React from 'react';

import InputContainer from './InputContainer';
import ListContainer from './ListContainer';

export default function Page() {
  return (
    <div>
      To-do
      <InputContainer />
      <ListContainer />
    </div>
  );
}
```

이렇게 코드를 수정하면 아래와 같은 에러가 난다

![](https://velog.velcdn.com/images/yxxnhx/post/710ff435-8b05-4e3a-8bcf-dffec92f05b9/image.png)

dispatch를 읽어오지 못하는 에러이다

**Page.test.jsx**

```jsx
import React from 'react';

import { render, fireEvent } from '@testing-library/react';

import Page from './Page';

import { useSelector } from 'react-redux';

jest.mock('react-redux');

test('Page', () => {
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

  useSelector.mockImplementation((selector) =>
    selector({
      taskTitle: '',
      tasks,
    }),
  );

  const { getByText } = render(<Page />);

  expect(getByText(/Task-1/)).not.toBeNull();
  expect(getByText(/Task-2/)).not.toBeNull();
});
```

taskTitle을 빈값으로 기본값을 넘겨준 후에 더이상 확인할 필요없는 change와 add를 지워주면 에러가 사라지는 것을 확인할 수 있다.

추가 버튼이 있는지 여부를 확인하는 코드는 이미 input에서 진행하였기 때문에 더이상 필요가 없다

이렇게 Page test도 굉장히 단순하게 정리가 되는 것을 볼 수 있다.

그런데 이제 App 컴포넌트와 Page 컴포넌트를 비교해보면 둘 다 별 내용 없이 컨테이너들을 전달해주는 역할만 하고 있는 것을 확인할 수 있다.

![](https://velog.velcdn.com/images/yxxnhx/post/116d6778-4d72-4617-bf39-11cd08aeca6f/image.png)

그렇다면 Page를 만들어서 넘기지 말고 App에서 그냥 한번에 넘겨주는 구조로 변경을 해보자.

→ Page, Page.test 삭제

**App.jsx**

```jsx
import React from 'react';

import InputContainer from './InputContainer';
import ListContainer from './ListContainer';

export default function App() {
  return (
    <div>
      <h1>To-do</h1>
      <InputContainer />
      <ListContainer />
    </div>
  );
}
```

**App.test.jsx**

```
import React from 'react'

import { render } from '@testing-library/react'

import App from './App'

import { useSelector } from 'react-redux'

jest.mock('react-redux');

test('App', () => {
  const tasks = [
    {
      id: 1,
      title: '아무것도 하지 않기 #1',
    },
    {
      id: 2,
      title: '아무것도 하지 않기 #2',
    },
  ]

  useSelector.mockImplementation((selector) => selector({
    taskTitle: '',
    tasks,
  }))

  const { getByText } = render((
    <App />
  ))

  expect(getByText(/추가/)).not.toBeNull()
  expect(getByText(/아무것도 하지 않기 #1/)).not.toBeNull()
})
```

→ taskTitle만 추가하고 나머지는 모두 동일하였다.
