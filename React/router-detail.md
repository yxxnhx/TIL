# 레스토랑 상세 페이지 구현하기

이제 한번 가게의 상세 정보를 확인하는 작업을 해보자!

그렇다면 먼저 RestaurantsPage를 가서 확인해보자.

```jsx
return (
  <div>
    <h1>Restaurants</h1>
    <RegionsContainer />
    <CategoriesContainer />
    <RestaurantsContainer />
  </div>
);
```

RestaurantsPage에서는 각각의 지역과 카테고리를 관리하고 보여주고 그에 맞는 레스토랑을 보여줄 수 있도록 관리하고 있다.

그러니 RestaurantsContaienr에서 각각의 레스토랑 상세 정보를 확인할 수 있도록 링크를 걸어주면 된다.

**그렇다면 먼저 App의 테스트를 참고하여 먼저 테스트 작업부터 진행해보자**

```jsx
context('with path /restaurants', () => {
  it('renders the restaurants page', () => {
    const { container } = renderApp({ path: '/restaurants' });

    expect(container).toHaveTextContent('서울');
  });
});
```

→ restaurants 경로를 가지고 접근을 하였더니 서울이라는 텍스트를 가지고 있다는 테스트 조건이 있다.

**이와 같이 유사하게 RestaurantsPage에서 테스트를 만들어보자.**

```jsx
describe('RestaurantsPage', () => {
  beforeEach(() => {
    const dispatch = jest.fn();

    useDispatch.mockImplementation(() => dispatch);

    useSelector.mockImplementation((selector) =>
      selector({
        regions: [{ id: 1, name: '서울' }],
        categories: [{ id: 1, name: '한식' }],
        restaurants: [{ id: 1, name: '김밥천국' }],
      }),
    );
  });

  it('renders region and category select buttons', () => {
    const { queryByText } = render(<RestaurantsPage />);

    expect(dispatch).toBeCalled();

    expect(queryByText('서울')).not.toBeNull();
    expect(queryByText('한식')).not.toBeNull();
    expect(queryByText('김밥천국')).not.toBeNull();
  });
});
```

→ 이렇게 먼저 지역과 카테고리를 선택할 수 있는 버튼의 랜더링 여부를 확인할 수 있는 조건과, 계속해서 반복해서 사용하는 디스패치와 selector를 beforeEach에 넣어 사용하면 아래와 같은 에러가 나는 것을 볼 수 있다.

![](https://velog.velcdn.com/images/yxxnhx/post/42f22fd7-9bb3-4cdb-87cb-d975e860cb83/image.png)

→ dispatch가 정의되지 않았다는 에러이다.

그러니 dispatch를 선언해준 것은 beforeEach 바깥에 빼주어야 한다.

```jsx
describe('RestaurantsPage', () => {
  const dispatch = jest.fn()

  beforeEach(() => {
    useDispatch.mockImplementation(() => dispatch)

    useSelector.mockImplementation(selector => selector({
      regions: [
        { id: 1, name: '서울'}
      ],
      categories: [
        { id: 1, name: '한식'}
      ],
      restaurants: [
        { id: 1, name: '김밥천국'}
      ]
    }))
  })
```

이와 같은 형태로 수정을 해주게 되면 테스트는 정상적으로 통과되는 것을 확인할 수 있다.

그러나, 테스트 파일 안에 테스트가 여러 개일 경우, 어떤 테스트가 먼저 실행이 될 지 모르고 이전의 테스트 상태를 디스패치가 그대로 가지고 있을 수 있기 때문에 dispatch를 초기화시켜주어야 한다.

**어떻게 초기화를 시킬 수 있을까?**

```jsx
dispatch.mockClear();
```

dispatch.mockClear를 활용하면 매번 디스패치가 실행되기 전에 초기화를 시킨 후 실행하게 된다.

**그렇다면 이제 지역과 카테고리 버튼을 누르면 각각의 정보들이 나오는 테스트를 진행해보자**

```jsx
it('renders link of restaurants', () => {
    const { container } = render((
      <RestaurantsPage />
    ));

    expect(container.innerHTML).toContain('<a href="')
  })
})
```

먼저 정말 직관적으로 링크가 있는지 여부를 테스트해보자.

![](https://velog.velcdn.com/images/yxxnhx/post/034ae6fc-ea5b-468f-96e8-1d568eb10701/image.png)

위와 같이 직관적으로 a 태그가 있는지 테스트를 하면 당연히 깨진다.
왜냐? RestaruantsPage에는 아직 링크가 걸려있지 않으니까!

**그렇다면 이제 링크를 만들어 테스트를 통과시켜보자**

**RestaurantsContainer.jsx**

```jsx
<li key={restaurant.id}>
  <a href="">{restaurant.name}</a>
</li>
```

→ 이제 정상적으로 테스트가 통과되는 것을 확인할 수 있다.

이제 그럼 path로 연결시켜주기 위하여 Link로 변경을 해주자.

```jsx
<li key={restaurant.id}>
  <Link to="/restaurants/1">{restaurant.name}</Link>
</li>
```

→ 일단은 먼저 임의로 경로를 정해주자.

원래 RESTfull API를 사용할 때,

`/restaurants`를 하게 되면 collection, 즉 그에 해당하는 목록

`/restaurants/1` 을 하게 되면 element, member 즉, 각각의 정보를 보여준다.

![](https://velog.velcdn.com/images/yxxnhx/post/495a59a0-d3d3-465b-9a41-e3c9bdc3cb38/image.png)

Link로 변경을 시켜주면 위와 같이 router 바깥에서 사용하고 있다는 에러와 함께 테스트가 깨지는 것을 볼 수 있다.

**이것을 해결할 수 있는 방법은 여러가지가 있다.**

### 1. 해당 테스트 바깥쪽으로 MemoryRouter를 이용하여 감싸주기

```
const { container } = render((
  <MemoryRouter>
    <RestaurantsContainer />
  </MemoryRouter>
));
```

Page에서도 문제가 되니 Page에도 MemoryRouter를 이용하여 잡아주기

```jsx
const { container } = render(
  <MemoryRouter>
    <RestaurantsPage />
  </MemoryRouter>,
);
```

→ 이렇게 각각의 테스트 파일에 MemoryRouter를 이용하여 감싸주면 테스트가 정상적으로 통과되는 것을 볼 수 있다.

그런데 Page 테스트에서는 테스트 조건에서 계속해서 render 부분이 반복되니 따로 빼서 정리를 해주자.

```
function renderRestaurantsPage() {
  return render((
    <MemoryRouter>
      <RestaurantsPage />
    </MemoryRouter>
  ));
}

const { queryByText } = renderRestaurantsPage()
```

### 2. 컴포넌트 자체에 props를 내려 이동할 때에는 Link를 쓸 수 있도록 설정하기

위와 같이 해당하는 컴포넌트별로 줄줄이 MemoryRouter를 사용하는 방법 외에 컴포넌트 자체에 Link를 props로 내려서 사용할 수 있도록 하는 방법이 있다.

**RestaurantsPage.jsx**

```jsx
import React, { useEffect } from 'react';

import { useDispatch } from 'react-redux';

import { Link } from 'react-router-dom';

/* 생략 */

  return (
    <div>
      <h1>Restaurants</h1>
      <RegionsContainer />
      <CategoriesContainer />
      <RestaurantsContainer
        Link={Link}
      />
    </div>
  );
}
```

**RestaurantsContainer.jsx**

```jsx
import React from 'react';

import { useSelector } from 'react-redux';

import { get } from './utils';

export default function RestaurantsContainer({ Link }) {
  const restaurants = useSelector(get('restaurants'));

  return (
    <ul>
      {restaurants.map((restaurant) => (
        <li key={restaurant.id}>
          <Link to="/restaurants/1">{restaurant.name}</Link>
        </li>
      ))}
    </ul>
  );
}
```

**RestaurantsContainer.test.jsx**

```jsx
test('RestaurantsContainer', () => {
  useSelector.mockImplementation((selector) =>
    selector({
      restaurants: [{ id: 1, name: '김밥천국' }],
    }),
  );

  const Link = ({ children }) => <>{children}</>;

  const { container } = render(<RestaurantsContainer Link={Link} />);

  expect(container).toHaveTextContent('김밥천국');
});
```

→ 테스트가 정상적으로 통과되는 것을 확인할 수 있다.

### 3. onClick을 활용하여 움직이게 하기

혹은 각각 눌러서 이동하는 행위이기 때문에 함께 테스트할 수 있도록 onClick를 활용하여 움직이게 하는 방법도 있다.

```jsx
export default function RestaurantsContainer({ onClickRestaurant }) {
  const restaurants = useSelector(get('restaurants'));

  function handleClick(restaurant) {
    onClickRestaurant(restaurant);
  }

  return (
    <ul>
      {restaurants.map((restaurant) => (
        <li key={restaurant.id}>
          <a href="/restaurants/1" onClick={() => handleClick(restaurant)}>
            {restaurant.name}
          </a>
        </li>
      ))}
    </ul>
  );
}
```

그러나 a 태그를 사용하게 되면 페이지 자체가 넘어가게 되어 네트워크를 사용하였을 때 해당 경로가 최상단에 뜨게 된다.

이렇게 넘어가지 않게 하기 위하여 event 자체를 해소해보자.

```jsx
function handleClick(restaurant) {
  return (event) => {
    onClickRestaurant(restaurant);
  };
}
```

이 형태가 많이 익숙치 않고 어려울 수 있다.

쉽게 말하자면 handleClick 내부에 함수를 받고 함수를 반환하게 하는 고차함수를 만들어낸 것이다.

```jsx
*onClick*={handleClick(restaurant)}
```

handleClick을 restaurant를 받게 하였더니 그 안에서

```jsx
return (event) => {
  onClickRestaurant(restaurant);
};
```

이렇게 `onClickRestaurant`를 반환하게 하는 것이다.

이것을 풀어서 사용하게 된다면, 아래와 같다.

```jsx
onClick={(event) => {onClickRestaurant(restaurant)}}
```

이제 그렇다면 handleClick의 반환하는 함수 내부에서 event 이슈를 해결해주면 된다.

```jsx
function handleClick(restaurant) {
  return (event) => {
    event.preventDefault();
    onClickRestaurant(restaurant);
  };
}
```

→ `event.preventDefault()`를 사용하면 링크로 이동하는 것을 막아줄 수 있다.

**이제 그렇다면 RestaurantsPage에서 onClickRestaurant를 설정해주자**

```jsx
import React, { useEffect } from 'react';

import { useDispatch } from 'react-redux';

import RegionsContainer from './RegionsContainer';
import CategoriesContainer from './CategoriesContainer';
import RestaurantsContainer from './RestaurantsContainer';

import { loadInitialData } from './actions';
import { useNavigate } from 'react-router-dom';

export default function RestaurantsPage() {
  const navigate = useNavigate();

  const dispatch = useDispatch();

  useEffect(() => {
    dispatch(loadInitialData());
  });

  function handleClickRestaurant(restaurant) {
    //TODO: 페이지 이동
    const url = `/restaurants/${restaurant.id}`;
    navigate(url);
  }

  return (
    <div>
      <h1>Restaurants</h1>
      <RegionsContainer />
      <CategoriesContainer />
      <RestaurantsContainer onClickRestaurant={handleClickRestaurant} />
    </div>
  );
}
```

→ useNavigate를 활용하여 restaurant의 id에 맞는 경로로 이동할 수 있도록 해주는 것이다.

**그 다음 App 컴포넌트에도 각각의 id 별로 보일 수 있도록 설정해주자.**

```jsx
<Route *path*='/restaurants/:id' *element*={<RestaurantPage />} />
```

이제 각각의 레스토랑 페이지를 볼 수 있도록 RestaurantPage 컴포넌트를 만들자

**RestaurantPage.jsx, RestaurantPage.test.jsx 생성**

```jsx
import React from 'react';

export default function RestaurantPage() {
  return <div>레스토랑</div>;
}
```

먼저 테스트부터 작성해보자

```jsx
import React from 'react';

import { render } from '@testing-library/react';

import RestaurantPage from './RestaurantPage';

describe('RestaurantPage', () => {
  function renderRestaurantPage() {
    return render(<RestaurantPage />);
  }

  it('renders name', () => {
    const { container } = renderRestaurantPage();

    expect(container).toHaveTextContent('마법사주방');
  });
});
```

우선 간단하게 레스토랑 이름이 잘 랜더링되는지 여부를 확인해보자

![](https://velog.velcdn.com/images/yxxnhx/post/22a6826d-b107-42fe-b252-5529ada5474f/image.png)

→ 당연히 테스트는 실패할 것이다. 레스토랑의 정보를 받아오지 못하였으니까

일단은 테스트에 통과할 수 있도록 마법사주방 대신 레스토랑을 넣어주자.

정상적으로 테스트가 통과하는 것을 확인할 수 있다.

이 다음에 할 것은 이제 주소에 따라서 레스토랑 정보를 받아오는 것이다.

만약 주소가 /restaurants/1일 경우 레스토랑 1이 출력되고 2일 경우에는 2가 출력되는 등의 경로에 따른 정보이다.

그러니 한번 해당 부분을 테스트에서 먼저 설정해보자.

```jsx
describe('RestaurantPage', () => {
  function renderRestaurantPage() {
    return render(
      <MemoryRouter initialEntries={['/restaurants/1']}>
        <RestaurantPage />
      </MemoryRouter>,
    );
  }

  it('renders name', () => {
    const { container } = renderRestaurantPage();

    expect(container).toHaveTextContent('레스토랑 1');
  });
});
```

![](https://velog.velcdn.com/images/yxxnhx/post/c9302df7-08f1-4318-a6f8-11a671df6bde/image.png)

→ 테스트가 깨지는 것을 확인할 수 있다.

그렇다면 이제 RestaurantPage에서 설정을 해주자.

```jsx
import React from 'react';
import { useParams } from 'react-router-dom';

export default function RestaurantPage() {
  const params = useParams();
  const { id } = params;

  return (
    <div>
      레스토랑
      {id}
    </div>
  );
}
```

→ useParams를 활용하면 현재 파라미터를 확인할 수 있다.

로컬 환경에서 확인해보면 레스토랑 1이 출력되는 것을 확인할 수 있으나 테스트는 여전히 동일하게 레스토랑만 출력이 되어 깨진다.

그렇다면 왜 그런 것일까?

```jsx
<MemoryRouter *initialEntries*={['/restaurants/1']}>
```

이와 같은 형태, 즉 주소로 내려주는 것은 App에서는 가능하나 여기에서는 router를 타고 오는 것이 아니기 때문에 불가능하다.
주소를 쪼개서 내려주는 것은 App에서 하기 때문에 Page에서는 알 수가 없다.

그러니 따로 처리를 해보자.

```jsx
describe('RestaurantPage', () => {
  it('renders name', () => {
    const params = { id: 1 };

    const { container } = render(<RestaurantPage params={params} />);

    expect(container).toHaveTextContent('레스토랑 1');
  });
});
```

→ RestaurantPage에서 params를 받아서 처리할 수 있게 하는 것이다.

그렇다면 다시 RestaurantPage로 가보자.

```jsx
import React from 'react';
import { useParams } from 'react-router-dom';

export default function RestaurantPage({ params }) {
  const { id } = params || useParams();

  return <div>레스토랑 {id}</div>;
}
```

→ params를 받아서 사용할 건데 있을 때는 params를 쓰고 없을 때는 useParams를 사용해서 파라미터를 받아와서 id를 꺼내서 사용하겠다는 것이다.

위와 같이 설정을 해주면 테스트, 로컬 모두 정상적으로 출력되는 것을 확인할 수 있다.

그런데 우리는 실제로는 레스토랑 1이 아닌 실제 API에서 받아와서 레스토랑 이름이 나와야 한다.

**그렇다면 API에서 어떻게 얻어와야 할까?**

**api.js**

```jsx
export async function fetchRestaurant({ restaurantId }) {
  const url = `https://eatgo-customer-api.ahastudio.com/restaurants/${restaurantId}`;
  const response = await fetch(url);
  const data = await response.json();

  return data;
}
```

테스트용 api도 함께 생성해주자

**mocks > api.js**

```
export async function fetchRestaurant({ restaurantId }) {
  return {
    id: restaurantId,
  };
}
```

조금 더 세밀하고 나은 테스트를 원한다면 id 외에도 name 등 다양한 필요한 데이터를 넣어서 확인할 수도 있다.

그 다음 이제 RestaurantPage에서 로드할 때마다 loadRestaurant를 이용하여 fetchRestaurant을 할 수 있게 설정해보자.

**RestaurantPage.jsx**

```jsx
fetchRestaurant({ restaurantId: id });
```

→ restaurantId가 id가 되는 것이다.

그렇다면 이제 restaurant를 받아오는 작업을 해보자.

```jsx
const restaurant = await fetchRestaurant({ restaurantId: id });
```

이제 이것을 로딩해주는 함수를 RestaurantPage 컴포넌트 외부에 만들어보자

```jsx
async function loadRestaurant({ restaurantId }) {
  const restaurant = await fetchRestaurant({ restaurantId });

  return restaurant;
}
```

이 loadRestaurant를 useEffect로 불러와서 사용할 수 있게 해보자.

```jsx
useEffect(() => {
  loadRestaurant({ restaurantId: id });
}, []);
```

이렇게만 하면 이 받은 것을 어떻게 표현할지를 설정해주지 않아 제대로 작동이 되지 않을 것이다.

**이제 표현에 대한 여부를 처리하기 위하여 redux로 가서 처리를 해보자!**

```jsx
const dispatch = useDispatch();

useEffect(() => {
  dispatch(loadRestaurant({ restaurantId }));
}, []);
```

먼저 실제로는 위와 같이 useEffect에서 디스패치를 활용하여 액션을 사용할 수 있도록 해야 한다.

그렇다면 컴포넌트 외부에 만들었던 loadRestaurant를 action에 가져가서 설정을 마저 해주자.

**actions.js**

```jsx
export function loadRestaurant({ restaurantId }) {
  return async (dispatch) => {
    const restaurant = await fetchRestaurant({ restaurantId });
    return restaurant;
  };
}
```

이제 더이상 restaurant를 그냥 반환하는 것이 아닌 레스토랑을 가져와서 넣어주는 액션을 디스패치를 활용하여 호출할 수 있도록 설정해야 한다.

```jsx
dispatch(setRestaurant(restaurant));
```

그렇다면 setRestaurant를 추가해주자

```jsx
export function setRestaurant(restaurant) {
  return {
    type: 'setRestaurant',
    payload: { restaurant },
  };
}
```

즉, loadRestaurant를 디스패치, 실행하게 되면 fetchRestaurant로 api에서 가져오게 된다.

그 다음 setRestaurant에 넣어주게 되는 것이다.

**이제 그러면 관심사별로 컴포넌트를 분리해보자.**

먼저 RestaurantContainer 컴포넌트를 만들어보자.

**RestaurantPage.jsx**

```jsx
export default function RestaurantPage({ params }) {
  const { id } = params || useParams();

  return <RestaurantContainer restaurantId={id} />;
}
```

RestaurantContainer에 restaurantId를 id로 넘겨준다.

그렇다면 RestaurantContainer 안에서 레스토랑 정보, id를 보여줄 수 있도록 분리를 하는 것이다.

**RestaurantContainer.jsx**

```jsx
function RestaurantContainer({ restaurantId }) {
  const dispatch = useDispatch();

  useEffect(() => {
    dispatch(loadRestaurant({ restaurantId }));
  }, []);

  return <div>레스토랑 {restaurant.id}</div>;
}
```

즉, 페이지에서는 리액트 라우터라는 라이브러리와 연결되어 파라미터를 처리하고, 컨테이너에서는 리액트 라우터 라이브러리를 모르게 분리가 된 것이다.

그냥 넘겨준 restaurantId를 가져와서 쓸 뿐이다.

그렇다면 여기에서 RestaurantDetail 컴포넌트를 만들어서 상세 정보만 처리할 수 있도록 관심사를 또 분리하면 어떨까?

**RestaurantContainer.jsx**

```jsx
function RestaurantContainer({ restaurantId }) {
  const dispatch = useDispatch();

  useEffect(() => {
    dispatch(loadRestaurant({ restaurantId }));
  }, []);

  const restaurant = useSelector(get('restaurant'));

  return <RestaurantDetail restaurant={restaurant} />;
}
```

**RestaurantDetail.jsx**

```jsx
function RestaurantDetail({ restaurant }) {
  return <div>레스토랑 {restaurant.id}</div>;
}
```

즉, 디테일은 리덕스를 모른다. 상태를 어떻게 빼오는지, 액션이 어떤지 모르고 그냥 받은 restaurant을 화면에 출력해주기만 할 뿐이다.

이제 그러면 테스트를 한번 확인해보자

테스트에 세팅을 많이 안 해줘서 id가 정의되지 않아 테스트가 깨진 것을 확인할 수 있다.

**RestaurantPage.test.jsx**

```jsx
import React from 'react';

import { useSelector, useDispatch } from 'react-redux';

import { render } from '@testing-library/react';

import RestaurantPage from './RestaurantPage';

describe('RestaurantPage', () => {
  beforeEach(() => {
    const dispatch = jest.fn();

    useDispatch.mockImplementation(() => dispatch);
    useSelector.mockImplementation((state) =>
      state({
        restaurant: {
          id: 1,
          name: '마법사 주방',
        },
      }),
    );
  });

  it('renders name', () => {
    const params = { id: 1 };

    const { container } = render(<RestaurantPage params={params} />);

    expect(container).toHaveTextContent('레스토랑 1');
  });
});
```

→ beforeEach를 활용하여 useDispatch와 useSelector로 값을 받아오고 설정할 수 있게 하였다.

각각을 정의해주니 더이상 테스트가 깨지지 않고 정상적으로 통과되는 것을 확인할 수 있다.

이제 그렇다면 실제로 구동할 수 있도록 action에 새롭게 만들었던 setRestaurant을 reducer에 처리해주자.

먼저 제대로 구현이 되는지 테스트로 검증을 해보자.

**reducer.test.js**

```jsx
describe('setRestaurant', () => {
  it('changes restaurant', () => {
    const initialState = { restaurant: null };
    const restaurant = { id: 1, name: '김밥천국' };

    const state = reducer(initialState, setRestaurant(restaurant));

    expect(state.restaurant.id).toBe(1);
    expect(state.restaurant.name).toBe('김밥천국');
  });
});
```

reducer에 아무런 설정도 해주지 않아 테스트에 실패하니 이제 설정을 해주자

**reducer.js**

```jsx
const initialState = {
  regions: [],
  categories: [],
  restaurants: [],
  restaurant: null,
  selectedRegion: null,
  selectedCategory: null,
};

/* 생략 */

setRestaurant(state, { payload: { restaurant } }) {
  return {
    ...state,
    restaurant,
  };
},
```

이제 정상적으로 테스트가 통과되는 것을 확인할 수 있다.

그렇다면 이제 RestaurantPage에서 임의로 만들어두었던 컨테이너와 디테일을 하나씩 분리해보자.

**RestaurantContainer.jsx, RestaurantContainer.test.jsx 생성**

```jsx
import React, { useEffect } from 'react';

import { useDispatch, useSelector } from 'react-redux';

import { loadRestaurant } from './actions';

import { get } from './utils';

import RestaurantDetail from './RestaurantDetail';

export default function RestaurantContainer({ restaurantId }) {
  const dispatch = useDispatch();

  useEffect(() => {
    dispatch(loadRestaurant({ restaurantId }));
  }, []);

  const restaurant = useSelector(get('restaurant'));

  return <RestaurantDetail restaurant={restaurant} />;
}
```

```jsx
import React from 'react';

import { render } from '@testing-library/react';

import RestaurantContainer from './RestaurantContainer';

import { useSelector, useDispatch } from 'react-redux';

describe('RestaurantContainer', () => {
  beforeEach(() => {
    const dispatch = jest.fn();

    useDispatch.mockImplementation(() => dispatch);

    useSelector.mockImplementation((selector) =>
      selector({
        restaurant: { id: 1, name: '마법사주방' },
      }),
    );
  });

  it('renders name', () => {
    const { container } = render(<RestaurantContainer restaurantId="1" />);

    expect(container).toHaveTextContent('마법사주방');
  });
});
```

→ 테스트가 정상적으로 통과되는 것을 확인할 수 있다.

지금은 테스트를 단순한 것을 하다보니 계속해서 동일한 것이 반복되지만 실제로는 디테일한 것을 잡아주는 것이 좋다.

예를 들면 실제 페이지에 들어가는 타이틀, 혹은 컨테이너의 경우에는 실제로 들어가는 값들을 테스트하는 것이 좋다.

```jsx
useSelector.mockImplementation(selector => selector({
    restaurant: {
      id: 1,
      name: '마법사주방',
      address: '서울시 강남구'
    }
  }))
})

it('renders name and address', () => {
  const { container } = render((
    <RestaurantContainer restaurantId='1'/>
  ));

  expect(container).toHaveTextContent('마법사주방')
  expect(container).toHaveTextContent('서울시 강남구')
})
```

이렇게 조건을 더 추가해서 확인하는 것이 좋다.

**RestaurantDetail.jsx, RestaurantDetail.test.jsx**

```jsx
import React from 'react';

export default function RestaurantDetail({ restaurant }) {
  return (
    <div>
      <p>{restaurant.name}</p>
      <p>{restaurant.address}</p>
    </div>
  );
}
```

```jsx
import React from 'react';

import { render } from '@testing-library/react';

import RestaurantDetail from './RestaurantDetail';

describe('RestaurantDetail', () => {
  const restaurant = {
    id: 1,
    name: '마법사주방',
    address: '서울시 강남구',
  };

  it('renders name and address', () => {
    const { container } = render(<RestaurantDetail restaurant={restaurant} />);

    expect(container).toHaveTextContent('마법사주방');
    expect(container).toHaveTextContent('서울시 강남구');
  });
});
```

→ 관심사 분리를 통해 디테일에서는 리덕스를 전혀 몰라도 되기 때문에 useDispatch, useSelector가 전혀 없어도 된다.

**RestaurantPage.jsx**

```jsx
import React from 'react';

import { useParams } from 'react-router-dom';

import RestaurantContainer from './RestaurantContainer';

export default function RestaurantPage({ params }) {
  const { id } = params || useParams();

  return <RestaurantContainer restaurantId={id} />;
}
```

**RestaurantPage.test.jsx**

```jsx
import React from 'react';

import { useSelector, useDispatch } from 'react-redux';

import { render } from '@testing-library/react';

import RestaurantPage from './RestaurantPage';

describe('RestaurantPage', () => {
  beforeEach(() => {
    const dispatch = jest.fn();

    useDispatch.mockImplementation(() => dispatch);
    useSelector.mockImplementation((state) =>
      state({
        restaurant: {
          id: 1,
          name: '마법사 주방',
        },
      }),
    );
  });

  it('renders name', () => {
    const params = { id: 1 };

    const { container } = render(<RestaurantPage params={params} />);

    expect(container).toHaveTextContent('마법사 주방');
  });
});
```

→ 페이지에서도 api에서 불러온 값이 제대로 들어가는지 여부를 확인할 수 있는 것으로 수정하면 전체적으로 테스트가 통과하는 것을 확인할 수 있다.

이제 그렇다면 로컬 웹페이지로 가서 확인해보자.

![](https://velog.velcdn.com/images/yxxnhx/post/d4f0d52e-4985-4dbc-aab0-2ded4b5410f6/image.png)

지금 RestaurantDetail에서 name을 읽어오지 못하는 에러가 떠있다.

즉, 못 불러 온 상태에서 랜더링하는 경우가 있다는 뜻이다.

그렇다면 RestaurantContainer에서 수정을 해주자.

```jsx
const restaurant = useSelector(get('restaurant'));
```

즉, restaurant가 없는 경우에 그리려고 하면 문제가 생기기 때문에 조건을 주자.

```jsx
if (!restaurant) {
  return null;
}
```

이제 정상적으로 로컬에서도 레스토랑 목록과 디테일이 나오는 것을 확인할 수 있다.

사실 이 경우에는 로딩 중에는 아무것도 안 보이는 문제 때문에 일어난 에러이기 때문에 null을 주기보다

```jsx
if (!restaurant) {
  return <p>Loading...</p>;
}
```

이렇게 로딩중이라는 것을 드러내는 것이 좋다.

즉, 이 부분은 무슨 상태냐에 따라서 다르게 해줘도 된다는 뜻이다.

실제로는 리덕스 안에서 isLoading 이라는 파라미터를 만들어서 사용하기도 한다.

지금은 로딩이 되는 동안 아무것도 해주지 않아서 잠깐 기다렸다가 나오는 것을 확인할 수 있는데 그 부분을 따로 더 새롭게 추가를 해줘도 재미있다.

**한번 actions에 가서 설정을 해보자.**

```jsx
export function loadRestaurant({ restaurantId }) {
  return async (dispatch) => {
    dispatch(setRestaurant(null));

    const restaurant = await fetchRestaurant({ restaurantId });

    dispatch(setRestaurant(restaurant));
  };
}
```

이렇게 처음에는 아무것도 주지 않았다가 fetch를 하면 그제서야 넣어주는 형식으로 바꾸었더니 어떻게 되는지 확인해보자.

![](https://velog.velcdn.com/images/yxxnhx/post/9fcfc076-1c60-4f62-bd1b-11785cf9cd38/image.gif)

이렇게 지정한 로딩 문구가 잠시 나왔다가 모두 로딩이 되면 레스토랑 정보가 보이는 것을 확인할 수 있다.

이런 연출들은 내가 어떻게 지정하느냐에 따라서 얼마든지 할 수 있는 것이다

이제 그렇다면 레스토랑의 나머지 정보들도 화면에 보일 수 있도록 해보자.

먼저 레스토랑에 어떤 정보가 있는지 콘솔에 찍어서 확인해보자.

![](https://velog.velcdn.com/images/yxxnhx/post/f374b3d5-8f32-4d4a-b044-436743e5b47f/image.png)

→ 이 안에 이름, 주소 외에도 menuItem들이 있는 것을 확인할 수 있다.

그럼 이 menuItem들도 화면에 출력해보자.

먼저 가볍게 보고 싶을 때에는 JSON.stringify를 활용하여 확인하는 방법도 있다.

```jsx
const { name, address, menuItems } = restaurant;

return (
  <div>
    <p>{name}</p>
    <p>{address}</p>
    <p>{JSON.stringify(menuItems)}</p>
  </div>
);
```

![](https://velog.velcdn.com/images/yxxnhx/post/c0c1b051-18f4-477a-a01c-3b71903c2ca5/image.png)

→ 이렇게 json 형태로 메뉴 아이템들이 쭉 나오는 것을 확인할 수 있다.

**이제 그렇다면 map을 활용해서 보여주자.**

```jsx
export default function RestaurantDetail({ restaurant }) {
  const { name, address, menuItems } = restaurant;

  return (
    <div>
      <p>{name}</p>
      <p>{address}</p>
      {menuItems.map((menuItem) => (
        <p key={menuItem.id}>{menuItem.name}</p>
      ))}
    </div>
  );
}
```

이 메뉴들은 리스트니까 리스트로 변경해주자

```jsx
<ul>
  {menuItems.map((menuItem) => (
    <li key={menuItem.id}>{menuItem.name}</li>
  ))}
</ul>
```

**그런데 만약, 메뉴 아이템이 없다면 어떻게 해야 할까?**

그에 대한 처리를 한번 해보자.

먼저 메뉴 아이템들만 관리할 수 있도록 관심사 분리를 해보자.

```jsx
export default function RestaurantDetail({ restaurant }) {
  const { name, address, menuItems } = restaurant;

  return (
    <div>
      <h2>{name}</h2>
      <p>주소 : {address}</p>
      <h3>메뉴 : </h3>
      <MenuItems menuItems={menuItems} />
    </div>
  );
}
```

```jsx
function MenuItems({ menuItems }) {
  if (!menuItems || menuItems.length === 0) {
    return <p>메뉴가 없어요!</p>;
  }
  return (
    <ul>
      {menuItems.map((menuItem) => (
        <li key={menuItem.id}>{menuItem.name}</li>
      ))}
    </ul>
  );
}
```

![](https://velog.velcdn.com/images/yxxnhx/post/cef61447-a074-4ec4-a87b-6f44d889e944/image.png)

→ 이렇게 메뉴 아이템이 아니거나, 메뉴 아이템의 길이가 0일 경우에는 메뉴가 없다는 문구가 뜰 수 있도록 따로 설정을 해주는 것이다.

이제 그렇다면 컴포넌트를 분리해주자.

**MenuItems.jsx, MenuItems.test.jsx 생성**

```jsx
import React from 'react';

export default function MenuItems({ menuItems }) {
  if (!menuItems || menuItems.length === 0) {
    return <p>메뉴가 없어요!</p>;
  }
  return (
    <ul>
      {menuItems.map((menuItem) => (
        <li key={menuItem.id}>{menuItem.name}</li>
      ))}
    </ul>
  );
}
```

```jsx
import React from 'react';

import { render } from '@testing-library/react';

import MenuItems from './MenuItems';

describe('MenuItems', () => {
  const menuItems = [
    { id: 1, name: '김밥' },
    { id: 2, name: '비빔밥' },
    { id: 3, name: '마라샹궈' },
  ];

  it('renders menu item', () => {
    const { container } = render(<MenuItems menuItems={menuItems} />);

    expect(container).toHaveTextContent('김밥');
    expect(container).toHaveTextContent('비빔밥');
    expect(container).toHaveTextContent('마라샹궈');
  });
});
```

그렇다면 메뉴 아이템이 없을 경우도 테스트를 추가해주자.

```jsx
import React from 'react';

import { render } from '@testing-library/react';

import MenuItems from './MenuItems';

describe('MenuItems', () => {
  context('with menu items', () => {
    const menuItems = [
      { id: 1, name: '김밥' },
      { id: 2, name: '비빔밥' },
      { id: 3, name: '마라샹궈' },
    ];

    it('renders menu item', () => {
      const { container } = render(<MenuItems menuItems={menuItems} />);

      expect(container).toHaveTextContent('김밥');
      expect(container).toHaveTextContent('비빔밥');
      expect(container).toHaveTextContent('마라샹궈');
    });
  });

  context('with menu items', () => {
    it('renders no items message', () => {
      const { container } = render(<MenuItems menuItems={[]} />);

      expect(container).toHaveTextContent('메뉴가 없어요!');
    });
  });
});
```

→ 이제 모든 테스트가 통과된 것을 확인할 수 있다.

**RestaurantDetail.jsx**

```jsx
import React from 'react';

import MenuItems from './MenuItems';

export default function RestaurantDetail({ restaurant }) {
  const { name, address, menuItems } = restaurant;

  return (
    <div>
      <h2>{name}</h2>
      <p>주소 : {address}</p>
      <h3>메뉴 : </h3>
      <MenuItems menuItems={menuItems} />
    </div>
  );
}
```

이렇게 레스토랑 디테일 컴포넌트도 단순해진 것을 확인할 수 있다.

만약, 메뉴 아이템에서 아이템이 없는 경우를 조금 더 다양하게 분리하여 확인하고 싶다면 아래와 같이 할 수도 있다.

메뉴 아이템이 아예 없는 빈 배열인지, 아니면 제이슨 파일에 메뉴 아이템이라는 목록이 없어서 undefined 상태인지에 따라서 테스트를 하고 싶다면 어떻게 해야 할까?

```jsx
context('with menu items', () => {
  it('renders no items message', () => {
    [[], undefined].forEach((menuItems) => {
      const { container } = render(<MenuItems menuItems={menuItems} />);

      expect(container).toHaveTextContent('메뉴가 없어요!');
    });
  });
});
```

위와 같이 빈 배열, 그리고 undefined의 경우를 forEach로 돌려서 확인할 수 있다.

만약에 제이슨에서 null을 주는 경우도 있다고 한다면 경우 조건에 null을 추가할 수도 있다.

```jsx
[[], null, undefined].forEach((menuItems) => {
```

결국 이것은 API가 어떻게 생겼냐에 따라서 검증하는 항목들이 조금씩 달라질 것이다.
