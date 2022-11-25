# localStage 활용하여 로그인 상태 유지하기

현재 상태는 로그인 후 새로고침을 하면 로그인 상태가 풀려서 더는 리뷰를 남길 수 없는 상태이다.

이 상태를 보완하기 위해 localStorage를 활용해보자.

**그렇다면 두 가지가 필요하다.**

- (LoginFormContainer에서 )로그인을 성공했을 시, localStorage에 accessToken을 저장하여 (App.jsx로) 가져오기
  → requestLogin 안에서 비동기로 처리를 해야 한다.

**App.jsx**

```jsx
const dispatch = useDispatch();

const accessToken = '';

if (accessToken) {
  dispatch(setAccessToken(accessToken));
}
```

→ 기본 형태는 이렇게 될 것이다.

이제 그렇다면 requestLogin 안에서 처리를 해보자.

**actions.js**

```jsx
export function requestLogin() {
  return async (dispatch, getState) => {
    const {
      loginFields: { email, password },
    } = getState();

    try {
      const accessToken = await postLogin({ email, password });

      dispatch(setAccessToken(accessToken));
    } catch (e) {
      console.error(e);
    }
  };
}
```

먼저 간단하게 try catch로 에러를 잡아보자

![](https://velog.velcdn.com/images/yxxnhx/post/64f9747a-a3c3-4a43-aa90-a5d5af8f7a74/image.png)

먼저 아무런 값을 넣지 않았을 때 400 에러가 뜨는 것을 볼 수 있다.

그 외에도 틀린 값을 입력해도 동일하게 에러가 콘솔에 찍히고 있다.

지금은 예외처리를 따로 해놓지 않았기 때문에 따로 잡지 않고 넘어간다. 대신에 내가 원하는대로 로그인에 실패하였습니다 라는 알람이 뜨도록 리덕스에 관리하게 해주어도 좋다.

예를 들면 reducer에서

```jsx
const initialState = {
	/* 생략 */
  loginFields: {
    email: '',
    password: '',
    error: '',
  },
  loginError: '',
]
```

loginFields 안에 에러를 넣어놨다가 보여주게 시키든, loginError를 따로 넣어놨다가 실패 시에 보이게 하든 등의 다양하게 설정을 하여 보여주게 하면 된다.

**일단은 다시 actions로 돌아가서 localStorage에 저장하는 작업을 먼저 이어가보자.**

```jsx
export function requestLogin() {
  return async (dispatch, getState) => {
    const {
      loginFields: { email, password },
    } = getState();

    const accessToken = await postLogin({ email, password });

    localStorage.setItem('accessToken', accessToken);

    dispatch(setAccessToken(accessToken));
  };
}
```

→ 각각 상태를 받아오고, assessToken을 api에서 받아오고 그 받아온 assessToken을 localStorage에 저장하고, assessToken를 상태에 넣어서 관리할 수 있도록 처리해주는 것이다.

이제 그렇다면 로컬로 돌아가서 제대로 저장이 되는지 확인해보자.

![](https://velog.velcdn.com/images/yxxnhx/post/c6a5eea4-5c2b-4e5b-8de1-73273d8e6433/image.png)

틀린 값을 넣으면 undefined가 들어가고,

![](https://velog.velcdn.com/images/yxxnhx/post/3fbd0d75-c73a-4f6c-9c70-f15db25d2cbc/image.png)

제대로 된 값을 넣었을 시에는 assessToken을 받아와서 제대로 localStorage에 들어가 있는 것을 확인할 수 있다.

이제 그렇다면 새로고침을 했을 때도 localStorage에서 값을 가져와서 사용할 수 있도록 처리하면 된다

**App.jsx**

```jsx
const dispatch = useDispatch();

const accessToken = localStorage.getItem('assessToken');

if (accessToken) {
  dispatch(setAccessToken(accessToken));
}
```

테스트할 때에는 localStorage를 mocking해서 사용할 수 있다.

```jsx
jest.mock('react-redux');
jest.mock('localStorage');
```

리덕스를 mocking한 것 처럼 localStorage도 동일하게 mocking해서 사용할 수도 있고

혹은 테스트용 api를 만든 것처럼 따로 처리를 해서 테스트를 할 수도 있다.

한번 테스트용으로 처리를 해보자.

**services > storage.js 생성**

```jsx
export function saveItem(key, value) {
  //TODO:
}
export function loadItem(key) {
  //TODO:
}
```

이런 식으로 구현이 될 것이다.

그렇다면 mock도 만들어주자.

**services > **mocks** > storage.js 생성**

```jsx
export const saveItem = jest.fn();

export const loadItem = jest.fn();
```

그렇다면 App 컴포넌트에서 localStorage 데이터를 가져오는 부분을 수정해보자.

**storage.js**

```jsx
export function loadItem(key) {
  return localStorage.getItem(key);
}
```

**App.jsx**

```jsx
import { loadItem } from './services/storage';

export default function App() {
  const dispatch = useDispatch();

  const accessToken = loadItem('accessToken');

  /* 생략 */
}
```

그렇다면 actions에도 데이터를 넣는 부분을 수정해보자.

**storage.js**

```jsx
export function saveItem(key, value) {
  return localStorage.setItem(key, value);
}
```

actions**.js**

```jsx
export function requestLogin() {
  return async (dispatch, getState) => {
    const {
      loginFields: { email, password },
    } = getState();

    const accessToken = await postLogin({ email, password });

    saveItem('accessToken', accessToken);

    dispatch(setAccessToken(accessToken));
  };
}
```

이제 이 부분을 테스트해보자.

**App.test.jsx**

이제 App에서 로그인 여부를 나누어 테스트해줄 수 있게 되었다.

```jsx
context('when logged out', () => {
  it('', () => {});
});

context('when logged in', () => {
  it('', () => {});
});
```

이렇게 각각 케이스를 나누어 진행해보자

```jsx
context('when logged out', () => {
  beforeEach(() => {
    loadItem.mockImplementation(() => {
      return null;
    });
  });

  it("doesn't call dispatch", () => {
    renderApp({ path: '/' });

    expect(dispatch).not.toBeCalled();
  });
});

context('when logged in', () => {
  const accessToken = 'ACCESS_TOKEN';

  beforeEach(() => {
    loadItem.mockImplementation(() => {
      return accessToken;
    });
  });

  it('call dispatch with setAccessToken action', () => {
    renderApp({ path: '/' });

    expect(dispatch).toBeCalledWith({
      type: 'setAccessToken',
      payload: { accessToken },
    });
  });
});
```

이제 그렇다면 로컬에서 확인해보자.

![](https://velog.velcdn.com/images/yxxnhx/post/210efa25-ffa6-44f3-a797-b198a8042211/image.png)

→ 이제는 로그인하고 새로고침을 해도 accessToken이 살아있는 것을 볼 수 있다.

이제 그렇다면 로그아웃을 해줄 것이 필요하다

로그아웃을 해보도록 하자.

**LoginFormContainer.jsx**

```jsx
{
  accessToken ? (
    <LogoutForm />
  ) : (
    <LoginForm
      fields={{ email, password }}
      onChange={handleChange}
      onSubmit={handleSubmit}
    />
  );
}
```

```jsx
function LogoutForm() {
  return <div>log in</div>;
}
```

→ 삼항 연산자를 활용해서 accessToken가 있을 경우에는 로그아웃 폼이 보이도록, 아닐 경우에는 로그인 폼이 보이도록 하였다

![](https://velog.velcdn.com/images/yxxnhx/post/2d0a408c-f571-4f8b-8a83-ab7ef8a63606/image.png)

로컬에서 확인해보면 로그인이 되어있는 상태이기 때문에 로그아웃 폼이 정상적으로 보이고 있다.

이제 그렇다면 버튼을 눌러서 로그아웃이 되게 처리해보자.

```jsx
function LogoutForm({ onClick }) {
  return (
    <button type="button" onClick={onClick}>
      Log out
    </button>
  );
}
```

```jsx
function handleClickLogout() {
    dispatch(setAccessToken(''))
  }

  return (
    <>
      {
        accessToken
        ? <LogoutForm
            onClick={handleClickLogout}
          />
        : (
          <LoginForm
            fields={{ email, password }}
            onChange={handleChange}
            onSubmit={handleSubmit}
          />
        )
      }
      <p>{accessToken}</p>
    </>
  )
}
```

→ 로컬에서 확인해보면 이제 로그아웃을 누르면 다시 accessToken이 없기 때문에 LoginForm이 보이게 된다.

**그렇다면 LogoutForm을 분리해보자.**

**LogoutForm.jsx, LogoutForm.test.jsx 생성**

```jsx
import React from 'react';

export default function LogoutForm({ onClick }) {
  return (
    <button type="button" onClick={onClick}>
      Log out
    </button>
  );
}
```

```jsx
import React from 'react';

import { render } from '@testing-library/react';

import LogoutForm from './LogoutForm';

describe('LogoutForm', () => {
  const handleClick = jest.fn();

  beforeEach(() => {
    handleClick.mockClear();
  });

  function renderLogoutForm() {
    return render(<LogoutForm onClick={handleClick} />);
  }

  it('renders "Log out" button', () => {
    const { container } = renderLogoutForm();

    expect(container).toHaveTextContent('Log out');
  });
});
```

→ 테스트가 정상적으로 통과되는 것을 확인할 수 있다

그렇다면 이제 클릭했을 때, 일어나는 이벤트에 대해서도 테스트해보자.

```jsx
it('renders "Log out" button and listens click event', () => {
  const { container, getByText } = renderLogoutForm();

  expect(container).toHaveTextContent('Log out');

  fireEvent.click(getByText('Log out'));

  expect(handleClick).toBeCalled();
});
```

이제 추가적으로 LoginFormContainer 테스트에서도 로그아웃, 로그인 상태에 대해 테스트해보자.

```jsx
import React from 'react';

import { render, fireEvent } from '@testing-library/react';

import { useDispatch, useSelector } from 'react-redux';

import LoginFormContainer from './LoginFormContainer';

jest.mock('react-redux');

describe('LoginFormContainer', () => {
  const dispatch = jest.fn();

  beforeEach(() => {
    dispatch.mockClear();

    useDispatch.mockImplementation(() => dispatch);

    useSelector.mockImplementation((selector) =>
      selector({
        loginFields: {
          email: 'test@test',
          password: '1234',
        },
        accessToken: given.accessToken,
      }),
    );
  });

  context('when logged out', () => {
    given('accessToken', () => '');

    it('renders input controls', () => {
      const { getByLabelText } = render(<LoginFormContainer />);

      expect(getByLabelText('E-mail').value).toBe('test@test');
      expect(getByLabelText('Password').value).toBe('1234');
    });

    it('listens change event', () => {
      const { getByLabelText } = render(<LoginFormContainer />);

      fireEvent.change(getByLabelText('E-mail'), {
        target: { value: 'new email' },
      });

      expect(dispatch).toBeCalledWith({
        type: 'changeLoginField',
        payload: { name: 'email', value: 'new email' },
      });
    });

    it('renders "Log in" button', () => {
      const { getByText } = render(<LoginFormContainer />);

      fireEvent.click(getByText('Log In'));

      expect(dispatch).toBeCalled();
    });
  });

  context('when logged in', () => {
    given('accessToken', () => 'ACCESS_TOKEN');

    it('renders "log out" button', () => {
      const { container, getByText } = render(<LoginFormContainer />);

      expect(container).toHaveTextContent('Log out');

      fireEvent.click(getByText('Log out'));

      expect(dispatch).toBeCalledWith({
        type: 'setAccessToken',
        payload: { accessToken: '' },
      });
    });
  });
});
```

→ 테스트가 정상적으로 통과되는 것을 확인할 수 있다.

그런데 뭔가 로그아웃의 액션을 setAccessToken으로 빈 accessToken을 주는 것보다 logout이라는 액션을 새로 만들어서 사용하는 것이 조금 더 적절하지 않은가?

한번 만들어보자.

**actions.js**

```jsx
export function logout() {
  return {
    type: 'logout',
  };
}
```

**reducer.test.js**

```jsx
describe('logout', () => {
  it('changes accessToken', () => {
    const initialState = { accessToken: 'ACCESS_TOKEN' };

    const state = reducer(initialState, logout());

    expect(state.accessToken).toBe('');
  });
});
```

**reducer.js**

```jsx
logout(state) {
  return {
    ...state,
    accessToken: '',
  };
},
```

→ 모든 테스트가 정상적으로 통과되는 것을 확인할 수 있다.
