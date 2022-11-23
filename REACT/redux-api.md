# Redux HTTPie를 활용하여 통신해보자

사용자의 로그인을 하고 로그인을 기반해서 기능들을 추가해주자
현재 사용하고 있는 서버는 로그인 서버가 따로 있어 로그인을 확인할 수 있다.
간단히 HTTPpie라는 프로그램을 이용해서 먼저 테스트를 해보고 옮기는 작업을 해보자.

## **HTTPie**

[HTTPie - API testing client that flows with you](https://httpie.io/)

HTTP 클라이언트 CLI 도구이다. 간단한 데이터 조회 및 전송을 테스트해볼 수 있다.
우리가 자주 사용하는 `curl`로도 테스트할 수 있지만 사용하기 불편한 점이 있고, 또 Postman을 사용할 수도 있지만 간단하게 테스트할 때는 HTTPie로 해보는 것도 좋다.

#### HTTPie 설치

```jsx
brew install httpie
```

먼저 사용하기 전, REST API에서 사용하는 네 가지 메서드를 짚고 넘어가자.

- GET
- POST
- PUT/PATCH
- DELETE

그 중에서 우리는 로그인을 위해서 session이라는 리소스를 POST 만들 것(Create)이다.
→ CRUD로 대응하니까!

그렇다면 터미널창에 다음과 같이 입력해보자

```jsx
http POST eatgo-login-api.ahastudio.com
```

![](https://velog.velcdn.com/images/yxxnhx/post/5c36e601-7110-4db1-8169-3b81e507d813/image.png)

위와 같이 입력을 하게 되면 제대로 이메일과 패스워드를 입력해주지 않아 접근이 되지 않는 것을 볼 수 있다.
그렇다면 이제 테스트 아이디와 패스워드를 입력하여 제대로 접근해보자.

```jsx
http POST https://eatgo-login-api.ahastudio.com email=tester@example.com password=test
```

![](https://velog.velcdn.com/images/yxxnhx/post/d3382745-0a4d-4d5b-a70e-349eb26b46b5/image.png)

→ 404 Not Found가 뜨는 것을 볼 수 있다.

**왜일까?**

앞서 말했듯, 로그인을 위해서 session이라는 리소스를 활용해서 접근할 것인데 session을 넣지 않고 그냥 주소만 입력해서 일어난 에러이다.
그렇다면 다시 `https://eatgo-login-api.ahastudio.com/session`으로 입력해보자

```jsx
http POST https://eatgo-login-api.ahastudio.com/session
email=tester@example.com password=test
```

![](https://velog.velcdn.com/images/yxxnhx/post/f7de8c4a-8efd-49d3-b550-51e721dcfc0e/image.png)

→ 정상적으로 접근이 되어 acessTocken이 출력된 것을 확인할 수 있다.
즉, session이라는 리소스를 POST하는데, email과 password를 넣었을 경우 create가 성공한 201이 나오게 된다.

만약, 잘못된 이메일 혹은 패스워드로 접근할 때는 어떻게 나올까?

```jsx
http POST https://eatgo-login-api.ahastudio.com/session
email=test@example.com password=test
```

![](https://velog.velcdn.com/images/yxxnhx/post/1a7739f1-e1ea-40bd-8652-22ff7c02fe63/image.png)

→ 아무것도 나오지 않고, 요청에 대해서도 400 에러가 나는 것을 확인할 수 있다.

**여기에서 코드에 대해서 알아보자**

- 200 - 성공
- 300 - 리다이랙션
- 400 - 클라이언트가 잘못됨
- 500 - 서버가 내부에서 문제가 생겼음

그러므로 현재, 클라이언트가 입력을 잘못했으므로 400 에러가 나는 것이다.

#### 이제 그렇다면 올바르게 입력해서 나오는 토큰을 활용하여 요청을 해보자.

예를 들어서 리뷰를 작성한다고 가정해보자.
누가 리뷰를 작성했는가? 내가 했다는 것을 알려야 한다.

**그렇다면 입력해보자.**

```jsx
http POST [https://eatgo-customer-api.ahastudio.com](https://eatgo-customer-api.ahastudio.com/)/restaurants/1/reviews
```

![](https://velog.velcdn.com/images/yxxnhx/post/4227dd08-9d0d-4c2b-a9de-600a2159e8ea/image.png)

→ body가 없다는 에러가 뜨는 것을 볼 수 있다.

그렇다면 리뷰 항목에 맞게 body를 입력해주자.

```jsx
http POST [https://eatgo-customer-api.ahastudio.com/restaurants/1/reviews](https://eatgo-customer-api.ahastudio.com/restaurants/1/reviews)
score=5 description=good
```

![](https://velog.velcdn.com/images/yxxnhx/post/02173ab8-16eb-4d18-80ff-493299c01a54/image.png)

![](https://velog.velcdn.com/images/yxxnhx/post/37b4f133-67ff-4579-b1ab-7b63c7e03291/image.png)

→ 정상적으로 넘겼으나, 사용자가 없어서 에러가 나는 것을 확인할 수 있다.
이럴 경우에는 아까 받은 accessTocken을 넣어서 문제를 해결할 수 있다.

이 부분은 먼저 UI를 만든 후 다시 확인해보자.

### 1. 로그인 화면 만들기

LoginPage.jsx, LoginPage.test.jsx 생성

```jsx
import React from 'react';

export default function LoginPage() {
  return (
    <div>
      <h2>Log In</h2>
    </div>
  );
}
```

```jsx
import React from 'react';

import { MemoryRouter } from 'react-router-dom';

import { render } from '@testing-library/react';

import LoginPage from './LoginPage';

test('LoginPage', () => {
  render(
    <MemoryRouter>
      <LoginPage />
    </MemoryRouter>,
  );
});
```

이제 그렇다면 로그인 페이지에서 필요한 것이 무엇일까?

userName과 password를 입력할 창이 필요하다.

그렇다면 먼저 테스트에서 작성해보고 컴포넌트에 넣어보자.

```jsx
import React from 'react';

import { MemoryRouter } from 'react-router-dom';

import { render } from '@testing-library/react';

import LoginPage from './LoginPage';

describe('LoginPage', () => {
  it('renders Log-in title', () => {
    const { container } = render(
      <MemoryRouter>
        <LoginPage />
      </MemoryRouter>,
    );

    expect(container).toHaveTextContent('Log In');
  });
});
```

→ 페이지에서는 간단하게 로그인 페이지 타이틀이 잘 랜더링되는지 여부만 확인해보자.

**이제 사용자의 입력을 받을 form은 LoginForm으로 컴포넌트를 생성하여 분리해보자.**
LoginFormContainer.jsx, LoginFormContainer.test.jsx 생성

```jsx
import React from 'react';

import { render } from '@testing-library/react';

import LoginFormContainer from './LoginFormContainer';

describe('LoginFormContainer', () => {
  it('renders input controls', () => {
    const { getByLabelText } = render(<LoginFormContainer />);

    expect(getByLabelText('E-mail')).not.toBeNull();
    expect(getByLabelText('Password')).not.toBeNull();
  });
});
```

→ 이렇게만 하면 테스트는 당연히 깨질 것이다.

form 컨테이너에 해당 인풋창과 라벨도 없으니 생성해서 테스트를 통과시켜보자.

```jsx
import React from 'react';

export default function LoginFormContainer() {
  return (
    <>
      <div>
        <label htmlFor="login-email">E-mail</label>
        <input type="email" id="login-email" />
      </div>
      <div>
        <label htmlFor="login-password">Password</label>
        <input type="password" id="login-password" />
      </div>
    </>
  );
}
```

→ 테스트가 정상적으로 통과되는 것을 확인할 수 있다.

그렇다면 LoginPage에서도 input에 대한 테스트를 넣을 수 있을 것이다.

```jsx
it('renders inputControls', () => {
  const { getByLabelText } = render(
    <MemoryRouter>
      <LoginPage />
    </MemoryRouter>,
  );

  expect(getByLabelText('E-mail')).not.toBeNull();
  expect(getByLabelText('Password')).not.toBeNull();
});
```

![](https://velog.velcdn.com/images/yxxnhx/post/54702dac-e817-4e57-9872-0705a1ae5efc/image.png)

→ 이렇게 바로 넣게 된다면 이메일과 패스워드 라벨을 찾을 수 없다는 에러가 뜬다.

왜일까?

LoginPage에 컨테이너를 불러오지 않아 연결이 되지 않았기 때문에 뜨는 에러이다.

**그렇다면 페이지 컴포넌트로 가서 연결을 해주자.**

```jsx
import React from 'react';
import LoginFormContainer from './LoginFormContainer';

export default function LoginPage() {
  return (
    <div>
      <h2>Log In</h2>
      <LoginFormContainer />
    </div>
  );
}
```

→ 정상적으로 테스트가 통과되는 것을 확인할 수 있다.

그런데 만약, 컨테이너에 있는 내용을 E-mail에서 그냥 Email로 고치게 되면 페이지와 컨테이너 테스트가 줄줄이 깨지게 된다.
그럴 때에는 서로 연결된 페이지들이 많고, 찾을 수 없는 지경일 때에는 해당 부분에만 테스트를 넣고 페이지에서는 테스트를 굳이 진행하지 않아도 되지만, 이정도면 내가 찾아낼 수 있다 싶다면 정확도를 위해서 테스트에 넣어도 상관없다.
이제 그렇다면 input 창에 입력을 모두 완료하고 누를 수 있는 로그인 버튼을 만들어보자.

### 2. 로그인 버튼 만들기

**LoginFormContainer.test.jsx**

```jsx
it('renders "Log in" button', () => {
  const { getByText } = render(<LoginFormContainer />);

  fireEvent.click(getByText('Log in'));
});
```

→ 당연히 깨질 것이다. 로그인 버튼을 만들어주지 않았고, 정의를 해주지 않았으므로

그렇다면 이제 컴포넌트에 가서 생성해주자.

**LoginFormContainer.jsx**

```jsx
import React from 'react';

export default function LoginFormContainer() {
  return (
    <>
      <div>
        <label htmlFor="login-email">E-mail</label>
        <input type="email" id="login-email" />
      </div>
      <div>
        <label htmlFor="login-password">Password</label>
        <input type="password" id="login-password" />
      </div>
      <button type="button">Log In</button>
    </>
  );
}
```

이제 다시 테스트로 돌아가서 로그인 버튼이 하는 일을 먼저 테스트해보자.

**LoginFormContainer.test.jsx**

```jsx
import React from 'react';

import { render } from '@testing-library/react';

import { useDispatch } from 'react-redux';

import LoginFormContainer from './LoginFormContainer';

describe('LoginFormContainer', () => {
  const dispatch = jest.fn();

  beforeEach(() => {
    useDispatch.mockImplementation(() => dispatch);
  });

  it('renders input controls', () => {
    const { getByLabelText } = render(<LoginFormContainer />);

    expect(getByLabelText('E-mail')).not.toBeNull();
    expect(getByLabelText('Password')).not.toBeNull();
  });

  it('renders "Log in" button', () => {
    const { getByText } = render(<LoginFormContainer />);

    fireEvent.click(getByText('Log In'));

    expect(dispatch).toBeCalled();
  });
});
```

**여기에서 디스패치와 beforeEach를 주목해보자.**

```jsx
const dispatch = jest.fn();

beforeEach(() => {
  useDispatch.mockImplementation(() => dispatch);
});
```

→ 먼저 dispatch를 선언해주고, beforeEach로 각각의 it(테스트) 별로 useDispatch가 위에서 선언한 dispatch를 활용하여 실행이 되는데 상단에 있는 it이 먼저 실행이 될지, 하단에 있는 it이 실행이 될지는 아무도 모른다.

만약, 상단의 it에서 디스패치가 실행되고, 하단에서 디스패치가 제대로 실행되지 않았으나 `expect(dispatch).toBeCalled()`로 테스트를 할 경우, 어쨌든 한번 불렸기 때문에 테스트가 통과되는 오류를 방지하기 위하여 다음과 같은 초기화를 시켜주어야 한다.

```jsx
beforeEach(() => {
  dispatch.mockClear();

  useDispatch.mockImplementation(() => dispatch);
});
```

→ 실행이 되기 전, dispatch를 초기화시켜주는 것이다.

**그렇다면 다시 테스트를 한번 확인해보자.**

![](https://velog.velcdn.com/images/yxxnhx/post/e067f59e-4a28-48a9-92b6-de23173079c8/image.png)

→ 디스패치가 실행되지 않았다는 에러이다.

### 3. 로그인 액션 처리하기

**이제 그렇다면 로그인 버튼에 대해 컴포넌트에서 설정을 해주자**

**LoginFormContainer.jsx**

```jsx
const dispatch = useDispatch();

function handleClick() {
  //dispatch
}
```

→ 먼저 디스패치를 선언해주고, handleClick 함수를 만들어준다. 이 안에서 디스패치가 일어날 것이다.

```jsx
<button type="button" onClick={handleClick}>
  Log In
</button>
```

→ 그리고 버튼에 onClick으로 설정을 해주면 된다.

**그렇다면 이제 어떻게 디스패치를 하면 좋을까?**

리덕스에서 액션을 꺼내와서 디스패치할 수 있도록 하면 된다.

그런데 이게 그냥 상태를 가져와서 보여주고 로딩하는 것이 아니라 로그인 요청을 하는 것이기 때문에 requestLogin으로 설정하여 디스패치할 수 있도록 해보자.

**그렇다면 requestLogin이 어떻게 될까?**

안에 있는 상태를 활용할 수 있도록 해보자.

**그렇다면 그 상태를 우리가 어떻게 알까?**

바로 리덕스에서 처리를 해주면 된다!

**LoginFormContainer.jsx**

```jsx
import { requestLogin } from './actions';

export default function LoginFormContainer() {
  const dispatch = useDispatch();

  function handleClick() {
    dispatch(requestLogin())
  }
```

**actions.js**

```jsx
export function requestLogin() {
  return async (dispatch) => {
    //
  };
}
```

그렇다면 이 안에서 어떠한 일이 펼쳐져야 할까?
아까 우리가 했듯이, HTTP POST를 할 것이고 그 결과 받은 accessToken을 활용하여 디스패치를 할 것이다.

```jsx
export function requestLogin() {
  return async (dispatch) => {
    // HTTP POST
    // dispatch(setAccessToken(accessToken))
  };
}
```

그런데 POST를 할 때, email과 password가 필요했다.

**그러므로 getState를 활용하여 받아오도록 설정해보자.**
이전에 getState를 어떻게 사용했는지 한번 참고해보자.

```jsx
const { selectedRegion: region, selectedCategory: category } = getState();
```

→ 이미 state에 있는 값을 가져와서 getState에서 활용하게 했다.
여기에서도 동일하게 이미 알고 있는 state에서 email과 password를 가져와서 사용해야 한다.

**그렇다면 우리가 input에 입력할 때마다, 값이 바뀌는 처리를 해주어야 한다.**

form에 대한 관심사를 컴포넌트로 분리하여 처리해보자.
**LoginForm.jsx, LoginForm.test.jsx 생성**

```jsx
import React from 'react';

export default function LoginForm() {
  function handleClick() {
    //TODO 깨지지 않기 위해 임시로 넣어둠
  }

  return (
    <>
      <div>
        <label htmlFor="login-email">E-mail</label>
        <input type="email" id="login-email" />
      </div>
      <div>
        <label htmlFor="login-password">Password</label>
        <input type="password" id="login-password" />
      </div>
      <button type="button" onClick={handleClick}>
        Log In
      </button>
    </>
  );
}
```

```jsx
import React from 'react';

import { render, fireEvent } from '@testing-library/react';

import { useDispatch } from 'react-redux';

import LoginForm from './LoginForm';

describe('LoginForm', () => {
  it('renders input controls', () => {
    const { getByLabelText } = render(<LoginForm />);

    expect(getByLabelText('E-mail')).not.toBeNull();
    expect(getByLabelText('Password')).not.toBeNull();
  });
});
```

그렇다면 컨테이너에서 더이상 form을 직접적으로 가지고 있을 필요가 없으니 정리해보자.

**LoginFormContainer.jsx**

```jsx
import React from 'react';
import { useDispatch } from 'react-redux';
import { requestLogin } from './actions';
import LoginForm from './LoginForm';

export default function LoginFormContainer() {
  const dispatch = useDispatch();

  function handleSubmit() {
    dispatch(requestLogin());
  }

  return <LoginForm onSubmit={handleSubmit} />;
}
```

→ 버튼을 눌러서 제출하는 것이기 때문에 click을 submit으로 변경하였다.

**이제 그렇다면 테스트에서 검증하고 컴포넌트를 수정해보자.**

```jsx
it('renders "Log in" button', () => {
  const handleSubmit = jest.fn();

  const { getByText } = render(<LoginForm onSubmit={handleSubmit} />);

  fireEvent.click(getByText('Log In'));

  expect(handleSubmit).toBeCalled();
});
```

이와 같이 설정을 해주면 컨테이너와 폼 테스트 모두 다 에러가 뜰 것이다.

그렇다면 다시 LoginForm에서 설정을 해주면 이제 이 테스트는 모두 통과가 될 것이다.

**LoginForm.jsx**

```jsx
import React from 'react';

export default function LoginForm({ onSubmit }) {
  return (
    <>
      <div>
        <label htmlFor="login-email">E-mail</label>
        <input type="email" id="login-email" />
      </div>
      <div>
        <label htmlFor="login-password">Password</label>
        <input type="password" id="login-password" />
      </div>
      <button type="button" onClick={onSubmit}>
        Log In
      </button>
    </>
  );
}
```

→ 테스트가 모두 정상적으로 통과되는 것을 확인할 수 있다.

이제 페이지에서는 로그인 페이지가 잘 랜더링 되는지, 컨테이너에서는 리덕스를 활용하여 상태가 변경되는지, 폼에서는 직접 값을 받아서 관리할 수 있도록 하였다.
그런데 LoginForm에서는 가장 중요하게 확인해야 하는 것은 들어온 값이 잘 들어오고 있는지 여부이다.
이 부분을 테스트해보자.

```jsx
it('renders input controls and listens change events', () => {
  const handleChange = jest.fn();

  const { getByLabelText } = render(<LoginForm onChange={handleChange} />);

  expect(getByLabelText('E-mail')).not.toBeNull();
  expect(getByLabelText('Password')).not.toBeNull();

  fireEvent.change(getByLabelText('E-mail'), {
    target: { value: 'tester@example.com' },
  });

  expect(handleChange).toBeCalled();
});
```

![](https://velog.velcdn.com/images/yxxnhx/post/53d4adb5-0fd3-4ae6-a590-fcc070f2563b/image.png)

→ 호출이 되지 않을 것이다. 그렇다면 다시 컴포넌트로 돌아가서 handleChange에 대한 처리를 해주자.

**LoginForm.jsx**

```jsx
import React from 'react';

export default function LoginForm({ onChange, onSubmit }) {
  function handleChange(event) {
    const {
      target: { name, value },
    } = event;
    onChange({ name, value });
  }

  return (
    <>
      <div>
        <label htmlFor="login-email">E-mail</label>
        <input
          type="email"
          id="login-email"
          name="email"
          onChange={handleChange}
        />
      </div>
      <div>
        <label htmlFor="login-password">Password</label>
        <input
          type="password"
          id="login-password"
          name="password"
          onChange={handleChange}
        />
      </div>
      <button type="button" onClick={onSubmit}>
        Log In
      </button>
    </>
  );
}
```

→ 테스트가 정상적으로 통과되는 것을 확인할 수 있다.

그렇다면 테스트를 조금 더 정교하게 하고 싶다면 아래와 같이 변경을 해줄 수 있다.

```jsx
expect(handleChange).toBeCalledWith({
  name: 'email',
  value: 'tester@example.com',
});
```

그런데 지금은 email만 검증했지만 password까지 검증을 하게 되면 동일한 코드가 계속해서 반복이 될 것이다.

**한번 이것을 배열로 정리해보도록 하자.**

```jsx
it('renders input controls and listens change events', () => {
  const handleChange = jest.fn();

  const { getByLabelText } = render(<LoginForm onChange={handleChange} />);

  const controls = [
    { label: 'E-mail', name: 'email', value: 'tester@example.com' },
    { label: 'Password', name: 'password', value: 'test' },
  ];

  controls.forEach(({ label, name, value }) => {
    expect(getByLabelText(label)).not.toBeNull();

    fireEvent.change(getByLabelText(label), {
      target: { value },
    });

    expect(handleChange).toBeCalledWith({
      name,
      value,
    });
  });
});
```

여기에서 계속 `getByLabelText(label)`이 반복되므로 따로 빼서 정리도 가능하다

```jsx
controls.forEach(({ label, name, value }) => {
  const input = getByLabelText(label);

  expect(input).not.toBeNull();

  fireEvent.change(input, { target: { value } });

  expect(handleChange).toBeCalledWith({ name, value });
});
```

이렇게 코드가 깔끔하게 정리된 것을 확인할 수 있다.

**이제 그 다음으로 해야할 것이 무엇일까?**

```jsx
function handleChange(event) {
  const {
    target: { name, value },
  } = event;
  onChange({ name, value });
}
```

이렇게 입력한 값이 onChange로 넘어가 리덕스에서 관리할 수 있게 처리해주자.

**LoginFormContainer.jsx**

```jsx
export default function LoginFormContainer() {
  const dispatch = useDispatch();

  function handleSubmit() {
    dispatch(requestLogin());
  }

  function handleChange({ name, value }) {
    dispatch(changeLoginField({ name, value }));
  }

  return <LoginForm onSubmit={handleSubmit} onChange={handleChange} />;
}
```

→ 액션에서 changeLoginField로 이름과 값을 받아와서 바뀐 값들을 리덕스에서 관리할 수 있도록 설정해주는 것이다.

그렇다면 action에서 처리해주자.

**actions.js**

```jsx
export function changeLoginField({ name, value }) {
  return {
    type: 'changeLoginField',
    payload: { name, value },
  };
}
```

**LoginFormContainer.jsx**

```jsx
import React from 'react';

import { useDispatch, useSelector } from 'react-redux';

import { requestLogin, changeLoginField } from './actions';

import LoginForm from './LoginForm';

import { get } from './utils';

export default function LoginFormContainer() {
  const dispatch = useDispatch();

  const { email, password } = useSelector(get('loginFields'));

  function handleChange({ name, value }) {
    dispatch(changeLoginField({ name, value }));
  }

  function handleSubmit() {
    dispatch(requestLogin());
  }

  return (
    <LoginForm
      fields={{ email, password }}
      onChange={handleChange}
      onSubmit={handleSubmit}
    />
  );
}
```

여기에서 handleChange를 보자

```jsx
function handleChange({ name, value }) {
  dispatch(changeLoginField({ name, value }));
}
```

이렇게 받아온 name과 value가 useSelector를 활용해서 다시 form으로 들어가게 해야 한다.

```jsx
const { email, password } = useSelector((state) => {
  email: state.email;
  password: state.password;
});
```

이렇게 각각 받아와도 되지만, 한꺼번에 받아와서 사용할 때 구조분해할당으로 사용하는 것이 조금 더 깔끔하므로 아래와 같이 변경해서 사용할 수 있다.

```jsx
const { email, password } = useSelector(get('loginFields'));
```

**이제 테스트를 확인해보자.**

![](https://velog.velcdn.com/images/yxxnhx/post/6c358508-f84d-4380-af83-d5a7414d908f/image.png)

→ 이메일을 지정해주지 않아 테스트가 깨지는 것을 볼 수 있다.

useSelector를 이용하여 테스트에서 이메일과 패스워드를 설정해주자.

**LoginFormContainer.test.jsx, LoginPage.test.jsx**

```jsx
useSelector.mockImplementation((selector) =>
  selector({
    loginFields: {
      email: '',
      password: '',
    },
  }),
);
```

→ 테스트가 정상적으로 통과되는 것을 확인할 수 있다.

테스트를 조금 더 세밀하게 하기 위해서 조건에서 value를 추가해보자.

```jsx
useSelector.mockImplementation((selector) =>
  selector({
    loginFields: {
      email: 'test@test',
      password: '1234',
    },
  }),
);
```

먼저 입력한 값을 useSelector에 넣어두고

```jsx
expect(getByLabelText('E-mail').value).toBe('test@test');
expect(getByLabelText('Password').value).toBe('1234');
```

이메일과 패스워드의 값이 각각 useSelector에 넣어준 값인지를 확인하는 것이다.

![](https://velog.velcdn.com/images/yxxnhx/post/f1639bb0-132e-4228-82e4-c5a47c9a1999/image.png)

→ 테스트 결과를 확인해보면 현재 아무런 값이 들어오지 않아 깨진 것을 확인할 수 있다.
왜 그럴까? 당연하다!
LoginFormContainer에서 넘겨준 fields를 LoginForm에서 사용하고 있지 않기 때문이다.
먼저 테스트에서 검증을 하고 컨테이너에서 설정을 해주자.

```jsx
const { getByLabelText } = render(
  <LoginForm fields={{ email, password }} onChange={handleChange} />,
);
```

→ 먼저 fields로 email과 password를 받아오게 시킨 후에 입력했던 값을 선언해주자.

```jsx
const email = 'test@test';
const password = '1234';
```

이렇게 설정을 해준 후에 테스트를 각각 값이 잘 들어오고 있는지, 처음에 각각 이메일과 패스워드가 나온다 이렇게 누어서 테스트를 해도 되고, 한꺼번에 테스트를 해도 상관없다.

```jsx
const controls = [
  { label: 'E-mail', name: 'email', value: 'tester@example.com' },
  { label: 'Password', name: 'password', value: 'test' },
];
```

그렇다면 여기서 어떻게 해야 할까?

이전 값을 previous, origin 등등으로 선언해서 확인해줄 수도 있고, value를 배열로 받아서 할 수도 있다.

```jsx
const controls = [
  {
    label: 'E-mail',
    name: 'email',
    origin: email,
    value: 'tester@example.com'
  },
  {
    label: 'Password',
    name: 'password',
    origin: password,
    value: 'test'
  }
]

controls.forEach(({label, name, origin, value}) => {
  const input = getByLabelText(label);

  expect(input.value).toBe(origin)

  fireEvent.change(input, { target: { value } })

  expect(handleChange).toBeCalledWith({ name, value })
})
})
```

위와 같이 입력하고 있는 값과 함께 테스트를 할 수 있는 것이다.
그렇다면 이제 테스트를 통과하기 위해 LoginForm 컨테이너에서 설정해주자.

```jsx
import React from 'react';

export default function LoginForm({ fields, onChange, onSubmit }) {
  const { email, password } = fields;

  function handleChange(event) {
    const {
      target: { name, value },
    } = event;
    onChange({ name, value });
  }

  return (
    <>
      <div>
        <label htmlFor="login-email">E-mail</label>
        <input
          type="email"
          id="login-email"
          name="email"
          value={email}
          onChange={handleChange}
        />
      </div>
      <div>
        <label htmlFor="login-password">Password</label>
        <input
          type="password"
          id="login-password"
          name="password"
          value={password}
          onChange={handleChange}
        />
      </div>
      <button type="button" onClick={onSubmit}>
        Log In
      </button>
    </>
  );
}
```

이제 테스트를 확인해보자.

![](https://velog.velcdn.com/images/yxxnhx/post/7fbeb220-f987-4a7e-b361-6d0affc0d3b0/image.png)

→ 지금 테스트 별로 필요한 곳에서만 handleSubmit을 불러와서 그 값을 확인하지 못하여 일어나는 에러이다.
그렇다면 handleSubmit을 불러오되 중복된 값을 밖으로 빼서 정리해서 테스트를 통과시켜보자.

```jsx
import React from 'react';

import { render, fireEvent } from '@testing-library/react';

import { useDispatch } from 'react-redux';

import LoginForm from './LoginForm';

describe('LoginForm', () => {
  const handleChange = jest.fn();
  const handleSubmit = jest.fn();

  beforeEach(() => {
    handleChange.mockClear();
    handleSubmit.mockClear();
  });

  function renderLoginForm({ email, password }) {
    return render(
      <LoginForm
        fields={{ email, password }}
        onChange={handleChange}
        onSubmit={handleSubmit}
      />,
    );
  }

  it('renders input controls and listens change events', () => {
    const email = 'test@test';
    const password = '1234';

    const { getByLabelText } = renderLoginForm({ email, password });

    const controls = [
      {
        label: 'E-mail',
        name: 'email',
        origin: email,
        value: 'tester@example.com',
      },
      {
        label: 'Password',
        name: 'password',
        origin: password,
        value: 'test',
      },
    ];

    controls.forEach(({ label, name, origin, value }) => {
      const input = getByLabelText(label);

      expect(input.value).toBe(origin);

      fireEvent.change(input, { target: { value } });

      expect(handleChange).toBeCalledWith({ name, value });
    });
  });

  it('renders "Log in" button', () => {
    const { getByText } = renderLoginForm({});
    //여기에서는 email과 password 없이도 버튼이 랜더링되는지 여부만 확인하는 테스트므로 없어도 된다
    //원한다면 넣어도 상관은 없다

    fireEvent.click(getByText('Log In'));

    expect(handleSubmit).toBeCalled();
  });
});
```

→ 테스트가 정상적으로 통과되는 것을 확인할 수 있다.
이렇게 하면 이제 기본적으로 입력하는 것에 대한 처리는 끝났다.

**그렇다면 이제 해야할 것은 무엇일까?**

```jsx
export function changeLoginField({ name, value }) {
  return {
    type: 'changeLoginField',
    payload: { name, value },
  };
}

export function requestLogin() {
  return async (dispatch, getState) => {
    // HTTP POST
    // dispatch(setAccessToken(accessToken))
  };
}
```

이 두가지의 처리를 완료하면 된다.
입력하고 있는 값이 changeLoginField에 들어가고 버튼을 눌렀을 때 requestLogin으로 처리할 수 있게 해주면 된다.
이 부분을 먼저 reducer의 테스트에서 검증 후에 처리를 해보자.

**reducer.test.js**

```jsx
describe('changeLoginField', () => {
  it('changes login field', () => {
    const initialState = {
      loginFields: {
        email: '',
        password: '',
      },
    };

    const state = reducer(
      initialState,
      changeLoginField({
        name: 'email',
        value: 'test@test',
      }),
    );

    expect(state.loginFields.email).toBe('test@test');
  });
});
```

→ 이와 같이 테스트를 하면 테스트는 당연히 깨질 것이다.

reducer에 가서 먼저 테스트를 통과하기 위하여 임의로 설정해주자.

```jsx
/* 초기값 설정 */

const initialState = {
  regions: [],
  categories: [],
  restaurants: [],
  restaurant: null,
  selectedRegion: null,
  selectedCategory: null,
  loginFields: {},
};
```

```jsx
changeLoginField(state, { payload: { name, value } }) {
  return {
    ...state,
    loginFields: { email: 'test@test' },
  };
},
```

→ 테스트가 정상적으로 통과되는 것을 확인할 수 있다.

이제 그렇다면 진짜로 처리해줄 수 있도록 변경해보자.

```jsx
changeLoginField(state, { payload: { name, value } }) {
  return {
    ...state,
    loginFields: { [name]: value },
  };
},
```

그렇다면 이메일이 바뀌었을 때 비밀번호는 어떤 상태여야 할까?

테스트로 먼저 검증을 해보자.

```jsx
describe('changeLoginField', () => {
  context('when email is changed', () => {
    it('changes email field', () => {
      const initialState = {
        loginFields: {
          email: 'email',
          password: 'password',
        },
      };

      const state = reducer(
        initialState,
        changeLoginField({
          name: 'email',
          value: 'test@test',
        }),
      );

      expect(state.loginFields.email).toBe('test@test');
      expect(state.loginFields.password).toBe('password');
    });
  });
});
```

![](https://velog.velcdn.com/images/yxxnhx/post/3c205461-2d57-4d68-8998-308998791cec/image.png)

→ 기본값으로 입력한 password가 나와야 하는데 undefined가 나오는 것을 볼 수 있다.

동일하게 비밀번호를 입력할 때는 이메일이 어떻게 되어야 할까?

```jsx
context('when password is changed', () => {
    it('changes password field', () => {
      const initialState = {
        loginFields: {
          email: 'email',
          password: 'password',
        },
      };

      const state = reducer(
        initialState,
        changeLoginField({
          name: 'password',
          value: '1234',
        }),
      );

      expect(state.loginFields.email).toBe('email');
      expect(state.loginFields.password).toBe('1234');
    });
  });
});
```

![](https://velog.velcdn.com/images/yxxnhx/post/fc5041cd-2e3c-41f3-a537-111521d8fbbc/image.png)

→ 기본값으로 입력한 email이 들어가야 하는데 undefined가 나오는 것을 확인할 수 있다.

**왜일까?**

loginFields의 가존값을 넣어주지 않아서 일어나는 일이다.

```jsx
changeLoginField(state, { payload: { name, value } }) {
    return {
      ...state,
      loginFields: {
        ...state.loginFields,
        [name]: value,
      },
    };
  },
```

이와 같이 loginFields의 기본값을 넣어주니 테스트가 정상적으로 통과되는 것을 확인할 수 있다.

이제 그렇다면 action의 requestLogin를 처리하여보자.

먼저 getState로 email과 password를 가져오자.

```jsx
export function requestLogin() {
  return async (dispatch, getState) => {
    const {
      loginFields: { email, password },
    } = getState();

    // HTTP POST
    // dispatch(setAccessToken(accessToken))
  };
}
```

그다음 api로 설정을 해주자.

**services > api.js**

```jsx
export async function postLogin({ email, password }) {
  return {};
}
```

일단 무엇을 줄지는 모르겠지만 이메일과 비밀번호를 받아서 로그인 api를 호출할 것이다.

이제 actions로 돌아가 postLogin을 연결해주자.

**actions.js**

```jsx
export function requestLogin() {
  return async (dispatch, getState) => {
    const {
      loginFields: { email, password },
    } = getState();

    const accessToken = await postLogin({ email, password });

    dispatch(setAccessToken(accessToken));
  };
} // 여기에서 try - catch를 활용해서 에러처리를 필수 요소로 해야 한다!
```

이렇게 accessToken을 받아와서 postLogin를 호출할 수 있게 설정할 것이다.

그렇다면 setAccessToken을 처리해주자.

```jsx
export function setAccessToken(accessToken) {
  return {
    type: 'setAccessToken',
    payload: { accessToken },
  };
}
```

이제 reducer 테스트로 검증 후에 처리를 해주자

**reducer.test.js**

```jsx
describe('setAccessToken', () => {
  it('changes accessToken', () => {
    const initialState = { accessToken: '' };

    const state = reducer(initialState, setAccessToken('TOKEN'));

    expect(state.accessToken).toBe('TOKEN');
  });
});
```

**reducer.js**

```jsx
setAccessToken(state, { payload: { accessToken } }) {
  return {
    ...state,
    accessToken,
  };
},
```

→ 테스트가 정상적으로 통과되는 것을 확인할 수 있다.

이제 그렇다면 로그인 페이지에서 로그인 버튼을 눌렀을 때 성공 여부에 따른 처리를 해보자.

일단은 먼저 로그인을 진행하면 accessToken이 나오게 처리해보자

**LoginFormContainer.jsx**

```jsx
import React from 'react';

import { useDispatch, useSelector } from 'react-redux';

import { requestLogin, changeLoginField } from './actions';

import LoginForm from './LoginForm';

import { get } from './utils';

export default function LoginFormContainer() {
  const dispatch = useDispatch();

  const { email, password } = useSelector(get('loginFields'));
  const accessToken = useSelector(get('accessToken'));

  function handleChange({ name, value }) {
    dispatch(changeLoginField({ name, value }));
  }

  function handleSubmit() {
    dispatch(requestLogin());
  }

  return (
    <>
      <LoginForm
        fields={{ email, password }}
        onChange={handleChange}
        onSubmit={handleSubmit}
      />
      <p>{accessToken}</p>
    </>
  );
}
```

그렇다면 이제 컨테이너에서 조금 더 세밀하게 테스트를 해줄 수 있게 되었다.

```jsx
it('renders input controls', () => {
  const { getByLabelText } = render(<LoginFormContainer />);

  expect(getByLabelText('E-mail').value).toBe('test@test');

  fireEvent.change(getByLabelText('E-mail'), {
    target: { value: 'new email' },
  });

  expect(dispatch).toBeCalledWith({
    type: 'changeLoginField',
    payload: { name: 'email', value: 'new email' },
  });
});
```

이렇게 입력값이 바뀌는 것을 확인할 수 있게 되었다

만약 비밀번호까지 확인하고 싶다면 form의 테스트처럼 배열로 받아서 정리해서 한꺼번에 테스트를 할 수도 있다

그렇다면 인풋이 랜더링 되는지, change 이벤트가 잘 실행되는지를 각각 나누어 테스트를 해줄 수도 있다.

```jsx
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
```

그렇다면 form에서도 동일하게 입력이 되는지, 이벤트가 잘 실행되는지 나누어서 할 수도 있다.

```jsx
it('renders input controls', () => {
  const email = 'test@test';
  const password = '1234';

  const { getByLabelText } = renderLoginForm({ email, password });

  const controls = [
    { label: 'E-mail', value: email },
    { label: 'Password', value: password },
  ];

  controls.forEach(({ label, value }) => {
    const input = getByLabelText(label);

    expect(input.value).toBe(value);
  });
});

it('listens change events', () => {
  const { getByLabelText } = renderLoginForm({});

  const controls = [
    { label: 'E-mail', name: 'email', value: 'tester@example.com' },
    { label: 'Password', name: 'password', value: 'test' },
  ];

  controls.forEach(({ label, name, value }) => {
    const input = getByLabelText(label);

    fireEvent.change(input, { target: { value } });

    expect(handleChange).toBeCalledWith({ name, value });
  });
});
```

그렇다면 자꾸 초기값으로 {} 빈값을 넘겨주지 말고 그냥 할당할 때부터 없다면 빈 것을 주라고 설정해줄 수도 있다.

```jsx
import React from 'react';

import { render, fireEvent } from '@testing-library/react';

import { useDispatch } from 'react-redux';

import LoginForm from './LoginForm';

describe('LoginForm', () => {
  const handleChange = jest.fn();
  const handleSubmit = jest.fn();

  beforeEach(() => {
    handleChange.mockClear();
    handleSubmit.mockClear();
  });

  function renderLoginForm({ email, password } = {}) {
    return render(
      <LoginForm
        fields={{ email, password }}
        onChange={handleChange}
        onSubmit={handleSubmit}
      />,
    );
  }

  it('renders input controls', () => {
    const email = 'test@test';
    const password = '1234';

    const { getByLabelText } = renderLoginForm({ email, password });

    const controls = [
      { label: 'E-mail', value: email },
      { label: 'Password', value: password },
    ];

    controls.forEach(({ label, value }) => {
      const input = getByLabelText(label);

      expect(input.value).toBe(value);
    });
  });

  it('listens change events', () => {
    const { getByLabelText } = renderLoginForm();

    const controls = [
      { label: 'E-mail', name: 'email', value: 'tester@example.com' },
      { label: 'Password', name: 'password', value: 'test' },
    ];

    controls.forEach(({ label, name, value }) => {
      const input = getByLabelText(label);

      fireEvent.change(input, { target: { value } });

      expect(handleChange).toBeCalledWith({ name, value });
    });
  });

  it('renders "Log in" button', () => {
    const { getByText } = renderLoginForm();

    fireEvent.click(getByText('Log In'));

    expect(handleSubmit).toBeCalled();
  });
});
```

### 4. 라우팅하기

이제 그렇다면 로그인을 라우팅하고 로컬에서 확인해보자.

```jsx
import React from 'react';

import { Route, Routes, Link } from 'react-router-dom';

import HomePage from './HomePage';
import AboutPage from './AboutPage';
import NotFoundPage from './NotFoundPage';
import RestaurantsPage from './RestaurantsPage';
import RestaurantPage from './RestaurantPage';
import LoginPage from './LoginPage';

export default function App() {
  return (
    <div>
      <header>
        <h1>
          <Link to="/">여기는 헤더입니다</Link>
        </h1>
      </header>
      <Routes>
        <Route exact path="/" element={<HomePage />} />
        <Route path="/about" element={<AboutPage />} />
        <Route path="/login" element={<LoginPage />} />
        <Route path="/restaurants" element={<RestaurantsPage />} />
        <Route path="/restaurants/:id" element={<RestaurantPage />} />
        <Route path="*" element={<NotFoundPage />} />
      </Routes>
    </div>
  );
}
```

```jsx
export default function HomePage() {
  return (
    <div>
      <h2>home</h2>
      <ul>
        <li>
          <Link to='/about'>소개</Link>
        </li>
        <li>
          <Link to='/login'>Log In</Link>
        </li>
        <li>
          <Link to='/restaurants'>restaurants</Link>
        </li>
        <li>
          <Link to='/xxx'>멸망의 길</Link>
        </li>
      </ul>
    </div>
  )
}import React from 'react';

import { Route, Routes, Link } from 'react-router-dom';

import HomePage from './HomePage';
import AboutPage from './AboutPage';
import NotFoundPage from './NotFoundPage';
import RestaurantsPage from './RestaurantsPage';
import RestaurantPage from './RestaurantPage';
import LoginPage from './LoginPage';

export default function App() {
  return (
    <div>
      <header>
        <h1><Link to='/'>여기는 헤더입니다</Link></h1>
      </header>
      <Routes>
        <Route exact path='/' element={<HomePage />} />
        <Route path='/about' element={<AboutPage />} />
        <Route path='/login' element={<LoginPage />} />
        <Route path='/restaurants' element={<RestaurantsPage />} />
        <Route path='/restaurants/:id' element={<RestaurantPage />} />
        <Route path='*' element={<NotFoundPage />} />
      </Routes>
    </div>
  )
}
```

![](https://velog.velcdn.com/images/yxxnhx/post/ba7b0cdf-f65e-4ecd-bce8-838bc5faf111/image.png)

그런데 입력을 하면 다음과 같이 인풋 컨트롤이 제대로 처리되지 않아 에러가 나는 것을 확인할 수 있다.

reducer의 초기값이 제대로 정의되지 않아 일어난 에러이다

```jsx
const initialState = {
  regions: [],
  categories: [],
  restaurants: [],
  restaurant: null,
  selectedRegion: null,
  selectedCategory: null,
  loginFields: {
    email: '',
    password: '',
  },
};
```

이제 값을 입력해도 에러가 나타나지 않는다.

![](https://velog.velcdn.com/images/yxxnhx/post/01853f75-f836-47e4-94c4-a27f6f9ed63f/image.png)

그러나 로그인 버튼을 누르면 다음과 같이 에러가 난다.

api를 제대로 만들지 않아 일어나는 에러이니 처리를 해주자.

### 5. api로 fetch 연결하기

```jsx
export async function postLogin({ email, password }) {
  const url = `https://eatgo-login-api.ahastudio.com/session`;
  const response = await fetch(url, {
    method: 'POST',
    body: JSON.stringify({ email, password }),
  });
  const { accessToken } = await response.json();

  return accessToken;
}
```

method와 body를 넘겨주는 설정을 마저 해준 뒤 다시 로컬에서 확인해보자.

![](https://velog.velcdn.com/images/yxxnhx/post/dbe6995b-ad51-4d2b-b6a3-acb7c324e4c7/image.png)

틀린 값을 입력하였을 때에는 415 에러가 뜨는 것을 확인할 수 있다.

그렇다면 제대로된 값을 입력해보자

![](https://velog.velcdn.com/images/yxxnhx/post/5a5ecda6-d555-462b-af21-c4bd7185fa89/image.png)

제대로 된 값을 입력해도 여전히 415 에러가 뜨고 있다.

**그렇다면 네트워크를 한번 확인해보자**

![](https://velog.velcdn.com/images/yxxnhx/post/a3c59805-cd99-409e-be34-2c043f812dce/image.png)

→ json으로 요청하지 않아 값을 제대로 받아오지 못하여 생긴 에러이다

```jsx
export async function postLogin({ email, password }) {
  const url = `https://eatgo-login-api.ahastudio.com/session`;
  const response = await fetch(url, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({ email, password }),
  });
  const { accessToken } = await response.json();

  return accessToken;
}
```

이러한 에러를 방지하기 위해서 Axios 라이브러리를 사용할 수도 있다.

![](https://velog.velcdn.com/images/yxxnhx/post/4d46935e-37b2-4328-96db-20aa74ecaec8/image.png)

이제 정상적으로 로그인이 되고 accessToken이 출력되는 것을 확인할 수 있다.
