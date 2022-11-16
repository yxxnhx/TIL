# Routing

React rout dom을 활용하여 주소에 따라서 다른 여러가지 페이지를 보여주는 것을 한번 해보자!

지금까지는 한 페이지 안에서 작동하는 웹 애플리케이션을 만들었다.

그런데 실제로 웹페이지를 방문해보면 주소에 따라서 다른 페이지가 나오는 것을 쉽게 볼 수 있다.

![](https://velog.velcdn.com/images/yxxnhx/post/f2898c86-e566-4c47-9a2e-bb6e766aa7f9/image.png)

현재 우리는 지역과 카테고리에 따라서 식당 목록이 보이도록 해놓은 상태이다.

그런데 만약 현재 주소인 [`http://localhost:8080/`](http://localhost:8080/)에서 [`http://localhost:8080/](http://localhost:8080/)about` 이 들어가면 서비스에 대한 기본적인 안내 페이지가 나오고, [`http://localhost:8080/](http://localhost:8080/)restaurants` 에 들어가면 레스토랑 목록이 나오게 하면 어떨까?

### 1. 모든 웹 접속을 index.html로 연결하자

실제 서비스 같은 경우, 리다이렉트를 사용하거나 하여 index.html로 몰아서 연결을 해줄 수 있다.

개발 환경에서는 `webpack`에 `devServer`를 잡아서 연결할 수 있다.

**webpack.confing.js**

```jsx
  devServer: {
    historyApiFallback: {
      index: 'index.html',
    },
  },
```

이렇게 설정을 해주면 위와 같이 아무런 연결을 하지 않고 about 혹은 restaurants 페이지를 들어가게 주소창을 입력해도 계속해서 index.html이 나오게 된다.

**결국 index.html로 오게 된다는 것은 index.jsx로 오게 된다는 것과 같다.**

실제로 요청된 것에 따라 다른 화면이 나오게 설정을 해보자.

### 2. 페이지 분리를 해보자.

**App.jsx**

먼저 현재 보이는 지역, 카테고리, 레스토랑 목록들이 RestaurantsPage에서 보일 수 있도록 분리를 해보자.

**RestaurantsPage.jsx, RestaurantsPage.test.jsx 생성**

App에 있는 내용이 그대로 RestaurantsPage로 옮겨가기 때문에 그대로 복사하자

**RestaurantsPage.jsx**

```jsx
import React, { useEffect } from 'react';

import { useDispatch } from 'react-redux';

import RegionsContainer from './RegionsContainer';
import CategoriesContainer from './CategoriesContainer';
import RestaurantsContainer from './RestaurantsContainer';

import { loadInitialData } from './actions';

export default function RestaurantsPage() {
  const dispatch = useDispatch();

  useEffect(() => {
    dispatch(loadInitialData());
  });

  return (
    <div>
      <h1>Restaurants</h1>
      <RegionsContainer />
      <CategoriesContainer />
      <RestaurantsContainer />
    </div>
  );
}
```

**RestaurantsPage.test.jsx**

```jsx
import React from 'react';

import { useSelector, useDispatch } from 'react-redux';

import { render } from '@testing-library/react';

import RestaurantsPage from './RestaurantsPage';

test('RestaurantsPage', () => {
  const dispatch = jest.fn();

  useDispatch.mockImplementation(() => dispatch);

  useSelector.mockImplementation((selector) =>
    selector({
      regions: [{ id: 1, name: '서울' }],
      categories: [{ id: 1, name: '한식' }],
      restaurants: [{ id: 1, name: '김밥천국' }],
    }),
  );

  const { queryByText } = render(<RestaurantsPage />);

  expect(dispatch).toBeCalled();

  expect(queryByText('서울')).not.toBeNull();
  expect(queryByText('한식')).not.toBeNull();
  expect(queryByText('김밥천국')).not.toBeNull();
});
```

**그렇다면 이제 App 컴포넌트를 정리해보자.**

```jsx
import React from 'react';

import RestaurantsPage from './RestaurantsPage';

export default function App() {
  return <RestaurantsPage />;
}
```

### 3. 페이지별 설정을 해보자

이제 home 일 때, restaurantsPage 일 때 각각 다른 페이지가 보이도록 설정해보자.

```jsx
console.log(window.location);
```

window.location을 하면 내가 지금 어느 주소에 있는지를 확인할 수 있다.

![](https://velog.velcdn.com/images/yxxnhx/post/ec027ac9-10be-44d0-9c90-003911e94472/image.png)

그렇다면 여기에서 주소 뒤에 `/about`을 입력해보자

![](https://velog.velcdn.com/images/yxxnhx/post/3cf6f8db-9240-44d2-ae94-62f8dff94d4e/image.png)

href에 로컬호스트 뒤에 about이 붙어서 나오는 것을 볼 수 있다.

여기에서 계속해서 localhose~ 가 나오니 그냥 path만 보고 싶다면 pathname으로 설정해보자

```jsx
console.log(window.location.pathname);
```

![](https://velog.velcdn.com/images/yxxnhx/post/7d3fee49-850a-4f8f-8862-c8a2f0b61aa7/image.png)

→ pathname만 나오는 것을 볼 수 있다.

**그렇다면 이제 이 pathname에 따라서 설정을 해보자**

```jsx
export default function App() {
  const {
    location: { pathname },
  } = window;

  if (pathname === '/') {
    return <p>home</p>;
  }
  return <RestaurantsPage />;
}
```

이렇게 pathname이 없을 때는 home이 보이게, 다른 경우에는 RestaurantsPage가 보이도록 설정을 하였다.

한번 확인해보자.

    <img src="https://velog.velcdn.com/images/yxxnhx/post/ff21697b-a64a-4abf-a421-bccaac8a72df/image.png" width="300"/>
    <img src="https://velog.velcdn.com/images/yxxnhx/post/21e39a41-b932-42f8-82f2-9b1408b948eb/image.png" width="300"/>

→ 이렇게 pathname에 따라서 달라지는 것을 볼 수 있다.

그런데 지금 이렇게 페이지를 분리하면서 다음과 같이 App 테스트 파일에서 에러가 나는 것을 볼 수 있다.

![](https://velog.velcdn.com/images/yxxnhx/post/4bf6b4eb-bdd7-403f-9e57-e79a8b54149c/image.png)

더이상은 App에서 dispatch를 부르거나 하는 일이 없기 때문에 App 테스트를 조금 더 단순화 시켜보자

```jsx
import React from 'react';

import { useSelector } from 'react-redux';

import { render } from '@testing-library/react';

import App from './App';

test('App', () => {
  useSelector.mockImplementation((selector) =>
    selector({
      regions: [],
      categories: [],
      restaurants: [],
    }),
  );

  render(<App />);
});
```

→ 그냥 앱이 잘 랜더링되는지만 확인하게 단순화시켜 테스트를 통과시켰다.

### 4. pathname에 따라서 다른 컴포넌트가 보일 수 있도록 해보자

**App.jsx**

```jsx
export default function App() {
  const {
    location: { pathname },
  } = window;

  if (pathname === '/') {
    return <p>home</p>;
  }
  return <RestaurantsPage />;
}
```

위의 코드를 조금 다른 식으로 풀어보자면 아래와 같이 설정해줄 수 있다.

```jsx
export default function App() {
  const {
    location: { pathname },
  } = window;

  const MyComponent = RestaurantsPage;

  return <MyComponent />;
}
```

이렇게 레스토랑 페이지가 보이게 설정할 수 있는 것이다.

**이제 여기에서 pathname 입력값에 따라서 다양하게 보이게 할 것이다.**

```jsx
function HomePage() {
  return <p>home</p>;
}

export default function App() {
  const {
    location: { pathname },
  } = window;

  const MyComponent = {
    '/': HomePage,
    '/restaurants': RestaurantsPage,
  }[pathname];

  return <MyComponent />;
}
```

→ 정상적으로 pathname 입력값 별로 지정한 컴포넌트가 보이는 것을 확인할 수 있다.

단, 지정하지 않은 pathname을 입력하면 다음과 같은 에러가 뜨는 것을 볼 수 있다.

![](https://velog.velcdn.com/images/yxxnhx/post/ccda7d5a-ba49-469d-a1a1-ce17cf8af735/image.png)

그렇다면 지정하지 않은 pathname을 입력하였을 때에는 무조건 homepage가 나오도록 설정을 해보자

```jsx
const MyComponent =
  {
    '/': HomePage,
    '/restaurants': RestaurantsPage,
  }[pathname] || HomePage;
```

→ 정상적으로 지정한 홈페이지가 나오는 것을 확인할 수 있다.

혹은 NotFoundPage가 나오도록 설정을 할 수도 있다.

```jsx
function HomePage() {
  return (
    <p>home</p>
  )
}

function NotFoundPage() {
  return (
    <p>404 Not Found</p>
  )
}

export default function App() {
  const { location: { pathname }} = window;

  const MyComponent = {
    '/' : HomePage,
    '/restaurants' : RestaurantsPage
  }[pathname] || NotFoundPage

```

→ 정확하게는 실제로 404가 나가지 않고 http status 코드가 404가 나가야 한다

**about 페이지도 한번 추가해보자!**

```jsx
function HomePage() {
  return <p>home</p>;
}

function AboutPage() {
  return <p>about</p>;
}

function NotFoundPage() {
  return <p>404 Not Found</p>;
}

export default function App() {
  const {
    location: { pathname },
  } = window;

  const MyComponent =
    {
      '/': HomePage,
      '/about': AboutPage,
      '/restaurants': RestaurantsPage,
    }[pathname] || NotFoundPage;

  return <MyComponent />;
}
```

**그렇다면 home 페이지에 조금 더 링크를 연결하고 무언가를 해보자.**

```jsx
function HomePage() {
  return (
    <div>
      <h1>home</h1>
      <ul>
        <li>
          <a href="/about">about</a>
        </li>
        <li>
          <a href="/restaurants">restaurants</a>
        </li>
      </ul>
    </div>
  );
}
```

**이제 그렇다면 각각의 컴포넌트로 분리하여 만들어주자**

**HomePage.jsx**

```jsx
import React from 'react';

export default function HomePage() {
  return (
    <div>
      <h1>home</h1>
      <ul>
        <li>
          <a href="/about">about</a>
        </li>
        <li>
          <a href="/restaurants">restaurants</a>
        </li>
      </ul>
    </div>
  );
}
```

**HomePage.test.jsx**

```jsx
import React from 'react';

import { render } from '@testing-library/react';

import HomePage from './HomePage';

test('HomePage', () => {
  render(<HomePage />);
});
```

→ 가장 기본적으로 홈페이지가 제대로 랜더링되는가 여부만 확인하였다

물론 제대로 하려면 안에 있는 내용을 확인하고 해주면 좋기는 하다

**그렇다면 나머지 페이지들도 컴포넌트로 분리를 해주자**

**AboutPage.jsx**

```jsx
import React from 'react';

export default function AboutPage() {
  return <p>about</p>;
}
```

**AboutPage.test.jsx**

```jsx
import React from 'react';

import { render } from '@testing-library/react';

import AboutPage from './AboutPage';

test('AboutPage', () => {
  render(<AboutPage />);
});
```

**NotFoundPage.jsx**

```jsx
import React from 'react';

export default function NotFoundPage() {
  return <p>404 Not Found</p>;
}
```

**NotFoundPage.test.jsx**

```jsx
import React from 'react';

import { render } from '@testing-library/react';

import NotFoundPage from './NotFoundPage';

test('NotFoundPage', () => {
  render(<NotFoundPage />);
});
```

**App.jsx**

```jsx
import React from 'react';

import HomePage from './HomePage';
import AboutPage from './AboutPage';
import NotFoundPage from './NotFoundPage';
import RestaurantsPage from './RestaurantsPage';

export default function App() {
  const {
    location: { pathname },
  } = window;

  const MyComponent =
    {
      '/': HomePage,
      '/about': AboutPage,
      '/restaurants': RestaurantsPage,
    }[pathname] || NotFoundPage;

  return <MyComponent />;
}
```

이렇게 입력한 주소에 따라서 다른 컴포넌트가 보이게 하는 것을 라우팅이라고 한다.

#### **Routing**

- [SPA & Routing](https://poiemaweb.com/js-spa)

라우팅이란 출발지에서 목적지까지의 경로를 결정하는 기능이다.

애플리케이션의 라우팅은 사용자가 태스크를 수행하기 위해 어떤 화면(view)에서 다른 화면으로 화면을 전환하는 내비게이션을 관리하기 위한 기능을 의미한다.

일반적으로 사용자가 요청한 URL 또는 이벤트를 해석하고 새로운 페이지로 전환하기 위한 데이터를 취득하기 위해 서버에 필요 데이터를 요청하고 화면을 전환하는 위한 일련의 행위를 말한다.

현재는 아주 간단한 구조이기 때문에 이렇게 pathname에 따라서 보이게 설정을 해줄 수 있었지만 실제로는 이것보다 조금 더 고도화된 라이브러리를 사용하여 만들어낼 수 있다.

### 5. react router 사용하여 라우팅하기

#### **React Router**

- [react router](https://reactrouter.com/web/guides/quick-start)

**설치하기**

```jsx
npm i react-router-dom
```

```jsx
import React from 'react';

import { BrowserRouter, Route, Routes } from 'react-router-dom';

import HomePage from './HomePage';
import AboutPage from './AboutPage';
import NotFoundPage from './NotFoundPage';
import RestaurantsPage from './RestaurantsPage';

export default function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route exact path="/" element={<HomePage />} />
        <Route path="/about" element={<AboutPage />} />
        <Route path="/restaurants" element={<RestaurantsPage />} />
        <Route path="*" element={<NotFoundPage />} />
      </Routes>
    </BrowserRouter>
  );
}
```

- exact : 확실하게 입력했을 때만 들어가게 지정하고 싶을 때
- - : 지정한 path 외의 에러 페이지를 보여줄 때

**한번 HomePage 컴포넌트에서 일부러 틀린 주소를 연결해보자**

```jsx
import React from 'react';

export default function HomePage() {
  return (
    <div>
      <h1>home</h1>
      <ul>
        <li>
          <a href="/about">about</a>
        </li>
        <li>
          <a href="/restaurants">restaurants</a>
        </li>
        <li>
          <a href="/xxx">멸망의 길</a>
        </li>
      </ul>
    </div>
  );
}
```

![](https://velog.velcdn.com/images/yxxnhx/post/870a8ddb-9e9c-464b-99b8-fc40e9bc86f5/image.png)

→ 정상적으로 에러 페이지가 나오는 것을 볼 수 있다.

**그런데 한번 네트워크를 확인해보자**

![](https://velog.velcdn.com/images/yxxnhx/post/52ffc91d-d657-4acc-92e6-79f631ba3875/image.png)

링크에 따라 들어간 페이지가 가장 최상단에 올라가 있는 것을 볼 수 있다.

그런데 실제 홈페이지들은 실제 주소로 옮겨지지 않는다

한번 **Link를 활용하여 설정해보자!**

```jsx
import React from 'react';

import { Link } from 'react-router-dom';

export default function HomePage() {
  return (
    <div>
      <h1>home</h1>
      <ul>
        <li>
          <Link to="/about">about</Link>
        </li>
        <li>
          <Link to="/restaurants">restaurants</Link>
        </li>
        <li>
          <Link to="/xxx">멸망의 길</Link>
        </li>
      </ul>
    </div>
  );
}
```

![](https://velog.velcdn.com/images/yxxnhx/post/325b85ff-17cf-4f4b-8d4a-77676d4588eb/image.png)

→ 더이상 들어간 path로 변경되지 않는 것을 확인할 수 있다.

기존대로 a 태그를 사용해서 페이지 라우팅을 하게 된다면,

현재는 매우 단순해서 문제시가 되지 않지만, 페이지가 많고 사용자의 환경이 느린 인터넷을 사용하고 있다면 로딩 화면이 흰 화면으로 나와 화면이 멈춰있는 듯이 보이게 된다.

그러나 Link to를 이용하여 라우팅을 하게 된다면 가만히 있다가 준비가 되면 페이지가 나오게 되기 때문에 사용자의 불편함을 없애고 좋은 ux를 채워줄 수 있게 된다.

준비를 하는 동안에도 loading… 메시지 혹은 게이지가 올라가는 등의 퍼포먼스들을 기본적으로 세팅을 해서 보여줄 수도 있다.

**그렇다면 이제 테스트를 한번 보자**

![](https://velog.velcdn.com/images/yxxnhx/post/fab8b2d2-966e-4f1c-909e-9eb5bbffb68a/image.png)

→ Router 바깥에서 Link to를 사용해서 일어나는 에러이다

**한번 테스트 파일을 수정해보자**

**HomePage.test.jsx**

```jsx
import React from 'react';

import { BrowserRouter } from 'react-router-dom';

import { render } from '@testing-library/react';

import HomePage from './HomePage';

test('HomePage', () => {
  render(
    <BrowserRouter>
      <HomePage />
    </BrowserRouter>,
  );
});
```

→ 테스트 파일에 <BrowserRouter>를 추가해주니 테스트가 정상적으로 통과되는 것을 확인할 수 있다

**하지만 테스트에서는 직접 BrowserRouter를 사용하지 않고 MemoryRouter을 이용한다.**

```jsx
import React from 'react';

import { MemoryRouter } from 'react-router-dom';

import { render } from '@testing-library/react';

import HomePage from './HomePage';

test('HomePage', () => {
  render(
    <MemoryRouter>
      <HomePage />
    </MemoryRouter>,
  );
});
```

→ MemoryRouter를 이용하면 훨씬 더 가볍게 테스트 처리를 할 수 있다

**다양한 페이지를 연결되는 것을 보고 싶다면, App에서 라우팅하지말고 index로 BrowserRouter를 옮겨보자**

**index.jsx**

```jsx
import React from 'react';
import ReactDOM from 'react-dom';

import { Provider } from 'react-redux';

import App from './App';

import store from './store';
import { BrowserRouter } from 'react-router-dom';

ReactDOM.render(
  <Provider store={store}>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </Provider>,
  document.getElementById('app'),
);
```

**App.jsx**

```jsx
import React from 'react';

import { Route, Routes } from 'react-router-dom';

import HomePage from './HomePage';
import AboutPage from './AboutPage';
import NotFoundPage from './NotFoundPage';
import RestaurantsPage from './RestaurantsPage';

export default function App() {
  return (
    <Routes>
      <Route exact path="/" element={<HomePage />} />
      <Route path="/about" element={<AboutPage />} />
      <Route path="/restaurants" element={<RestaurantsPage />} />
      <Route path="*" element={<NotFoundPage />} />
    </Routes>
  );
}
```

**App.test.jsx**

```jsx
import React from 'react';

import { MemoryRouter } from 'react-router-dom';

import { useSelector } from 'react-redux';

import { render } from '@testing-library/react';

import App from './App';

test('App', () => {
  useSelector.mockImplementation((selector) =>
    selector({
      regions: [],
      categories: [],
      restaurants: [],
    }),
  );

  render(
    <MemoryRouter>
      <App />
    </MemoryRouter>,
  );
});
```

이제는 **화면에 따라 다르게 보이는 것을 테스트해보자**

```jsx
describe('App', () => {
  beforeEach(() => {
    useSelector.mockImplementation(selector => selector({
      regions: [],
      categories: [],
      restaurants: []
    }))
  })

  context('with path /', () => {
    it('renders the home page', () =>{
      const { container } = render((
        <MemoryRouter>
          <App />
        </MemoryRouter>
      ));

      expect(container).toHaveTextContent('home');
    })
  })
}
```

→ path가 /일 때에는 홈페이지가 랜더링되는 여부를 테스트하였다

**그렇다면 더 추가적으로 다른 페이지들도 테스트해보자**

```jsx
context('with path /about', () => {
  it('renders the about page', () => {
    const { container } = render(
      <MemoryRouter>
        <App />
      </MemoryRouter>,
    );

    expect(container).toHaveTextContent('about');
  });
});
```

**HomePage.jsx**

```jsx
<li>
  <Link to="/about">소개</Link>
</li>
```

![](https://velog.velcdn.com/images/yxxnhx/post/af282049-754f-4556-8139-7335455e4885/image.png)

about 페이지 연결을 소개를 누르면 할 수 있도록 변경을 하니 이와 같이 에러가 나는 것을 확인할 수 있다.

**AboutPage.jsx**

```jsx
import React from 'react';

export default function AboutPage() {
  return (
    <div>
      <h1>이 서비스에 대해서</h1>
      <p>이 서비스는 영국에서 시작되었습니다.</p>
      <p>이 서비스를 보셨다면 주변에 있는 20명에게 추천하셔야 합니다</p>
    </div>
  );
}
```

**이렇게 링크로 연결한 페이지 여부를 확인하려면 어떻게 해야 할까?**

링크로 연결한 페이지의 내용이 있는지 여부를 확인하는 것으로 해보자

```jsx
context('with path /about', () => {
  it('renders the about page', () => {
    const { container } = render(
      <MemoryRouter>
        <App />
      </MemoryRouter>,
    );

    expect(container).toHaveTextContent('20명에게 추천');
  });
});
```

![](https://velog.velcdn.com/images/yxxnhx/post/932f65ff-071d-4ebc-a7ee-e33da8f2316a/image.png)

→ 여전히 읽어오지 못하고 에러가 나는 것을 확인할 수 있다.

**그렇다면 어떻게 해야 할까?**

MemoryRouter에 initialEntries를 활용하여 path를 설정해주자

```jsx
<MemoryRouter *initialEntries*={['/about']}>
```

→ 정상적으로 테스트에 통과되는 것을 확인할 수 있다

**⭐️ !꼭! 배열 Array로 넣어주어야 한다⭐️**

마찬가지로 home을 검사하는 것도 조금 더 명확하게 해주고 싶다면 동일하게 추가를 해주면 된다.

```jsx
context('with path /', () => {
  it('renders the home page', () => {
    const { container } = render(
      <MemoryRouter initialEntries={['/']}>
        <App />
      </MemoryRouter>,
    );

    expect(container).toHaveTextContent('home');
  });
});
```

그런데 지금 계속해서 아래의 부분이 반복되고 있다

```jsx
render(
  <MemoryRouter initialEntries={['/']}>
    <App />
  </MemoryRouter>,
);
```

**한번 외부로 빼서 정리를 해보자**

```jsx
function renderApp({ path }) {
  return render(
    <MemoryRouter initialEntries={[path]}>
      <App />
    </MemoryRouter>,
  );
}
```

path를 받아서 각각의 테스트에서 추가를 해주자

```jsx
context('with path /', () => {
  it('renders the home page', () => {
    const { container } = renderApp({ path: '/' });

    expect(container).toHaveTextContent('home');
  });
});

context('with path /about', () => {
  it('renders the about page', () => {
    const { container } = renderApp({ path: '/about' });

    expect(container).toHaveTextContent('20명에게 추천');
  });
});
```

→ 정상적으로 테스트에 통과되는 것을 확인할 수 있다

**그렇다면 다른 페이지들도 한번 해보자!**

```jsx
context('with path /restaurants', () => {
  it('renders the restaurants page', () => {
    const { container } = renderApp({ path: '/restaurants' });

    expect(container).toHaveTextContent('서울');
  });
});
```

![](https://velog.velcdn.com/images/yxxnhx/post/b68f500a-d9a0-4e10-848c-194291702824/image.png)

→ dispatch로 받아오는 RestaurantsPage이기 때문에 dispatch가 없다는 에러가 나는 것을 확인할 수 있다.

**beforeEach에 dispatch를 설정해주자**

```jsx
beforeEach(() => {
  const dispatch = jest.fn();

  useDispatch.mockImplementation(() => dispatch);

  useSelector.mockImplementation((selector) =>
    selector({
      regions: [],
      categories: [],
      restaurants: [],
    }),
  );
});
```

![](https://velog.velcdn.com/images/yxxnhx/post/c228838c-63e9-4785-9ae0-4aa0a9f8163b/image.png)

→ 그렇다면 더이상 dispatch를 못 읽는다는 에러는 사라지고 서울을 읽지 못하는 에러만 뜬다

**그렇다면 이제 beforeEach로 설정한 초기값에 서울을 넣어보자**

```jsx
beforeEach(() => {
  const dispatch = jest.fn();

  useDispatch.mockImplementation(() => dispatch);

  useSelector.mockImplementation((selector) =>
    selector({
      regions: [{ id: 1, name: '서울' }],
      categories: [],
      restaurants: [],
    }),
  );
});
```

→ 정상적으로 테스트가 통과되는 것을 확인할 수 있다

**유효하지 않은 path도 검사를 해보자**

```jsx
context('with invalid path', () => {
  it('renders the 404 not found page', () => {
    const { container } = renderApp({ path: '/xxx' });

    expect(container).toHaveTextContent('Not Found');
  });
});
```

바깥에서 테스트를 하기 위하여 분리를 하기 위해서 App에서는 바로 Routes로 들어가고 BrowserRouter는 index로 옮긴 점만 유의하면 된다.

**만약에 나머지 모두가 동일한데 공통된 헤더가 있다면 어떻게 해야 할까?**

```jsx
export default function App() {
  return (
    <div>
      <header>여기는 헤더입니다</header>
      <Routes>
        <Route exact path="/" element={<HomePage />} />
        <Route path="/about" element={<AboutPage />} />
        <Route path="/restaurants" element={<RestaurantsPage />} />
        <Route path="*" element={<NotFoundPage />} />
      </Routes>
    </div>
  );
}
```

이렇게 Routes로 감싸진 부분만 변경이 될 뿐 바깥에 있는 header는 그대로 유지되는 것을 볼 수 있다

만약 헤더의 로고를 눌렀을 때 기존의 홈페이지들처럼 홈으로 이동하게 하고 싶다면 link를 이용하여 연결해주면 된다.
