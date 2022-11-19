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
