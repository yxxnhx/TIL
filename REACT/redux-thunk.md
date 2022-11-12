# Redux thunk를 활용한 비동기 통신

api를 이용해서 백앤드와 소통을 해서 비동기로 데이터를 가져와서 화면에 보여주는 처리를 해보자.

```jsx
useEffect(() => {
  dispatch(setRestaurants([]));
}, []);
```

지난번에는 위와 같이 setRestaurant를 임의로 빈값을 가져와서 App 컴포넌트가 로드될 때마다가 값을 가져오게 하였었다.

**그런데 만약, 여기에서 API 서버 혹은 백앤드 서버에서 로딩을 하면 어떨까?**

그래서 그 데이터를 화면에 보여주면 된다.

그렇다면 그것에 대한 처리를 한번 만들어보자.

일단은 가장 먼저 쉬운 것부터 만들어보자.

**App.jsx**

```jsx
useEffect(() => {
  loadRestaurants();
}, []);
```

App 컴포넌트 외부에 loadRestaurants 함수를 만들어보자.

```jsx
function loadRestaurants() {
  //TODO: dispatch
}
```

이 함수 내부에서 dispatch를 이용해서 상태를 변화시킬 것이다.

```jsx
function loadRestaurants({ dispatch }) {
  //TODO: dispatch
}
```

그렇다면 useEffect 안의 loadRestaurants는 다음과 같이 dispatch를 인자로 받아야 한다.

```jsx
useEffect(() => {
  loadRestaurants({ dispatch });
}, []);
```

그리고 loadRestaurants 내부에서 distpatch로 setRestaurants를 호출하여 상태를 변화시킬 것이라고 설정을 하면 된다.

```jsx
function loadRestaurants({ dispatch }) {
  dispatch(setRestaurants([]));
}
```

이제 이 안에서 restaurants가 확보가 되면 그 안에 넣어주면 된다.

```jsx
function loadRestaurants({ dispatch }) {
  const restaurants = [];
  dispatch(setRestaurants(restaurants));
}
```

무엇이 될지는 모르겠지만 일단 api로 불러올 것이다. 그러므로 일단 임의로 빈값을 넣어주자.

**이제 그러면 지금 우리는 두가지가 필요하다**

- 정보를 가지고 있는 API 서버
- 데이터를 가지고 오는 fetch

### 그렇다면 본격적으로 API를 이용하여 데이터를 가져와서 화면에 출력하는 작업을 해보자!

먼저 예시로 카테고리를 가져와보자.

**한번 api 서버를 들어가보자.**

[](https://eatgo-customer-api.ahastudio.com/categories)

![](https://velog.velcdn.com/images/yxxnhx/post/ce67ee9b-7ee4-4924-afaf-1989c1bce712/image.png)

현재 api의 categories를 들어가보면 위와 같이 배열로 들어가 있는 것을 볼 수 있다.

이 내용을 가져다가 화면에 보여줄 수 것이다.

**이제 그렇다면 useEffect에 loadCategories를 만들어서 넣어보자!**

```jsx
function loadCategories({ dispatch }) {
  const categories = [];

  dispatch(setCategories(categories))
}

export default function App() {
  const dispatch = useDispatch()

  useEffect(() => {
    loadRestaurants({ dispatch });
    loadCategories({ dispatch });

  }, [])
```

위와 같이 넣으면 아래와 같이 setCategories를 찾을 수 없어 일어나는 에러를 볼 수 있을 것이다.

![](https://velog.velcdn.com/images/yxxnhx/post/e8e23cae-7da5-46d8-a3b0-ae5c8c9260a6/image.png)

**그렇다면 이제 actions에서 setCategories를 만들어주자!**

**actions.js**

```jsx
export function setCategories(categories) {
  return {
    type: 'setCategories',
    payload: {
      categories,
    },
  };
}
```

type은 setCategories, payload는 categories를 반환하게 설정을 하면 더이상 에러가 나지 않는 것을 볼 수 있다.

**이제 카테고리를 관리하는 CategoriesContainer를 만들어주자.**

형식은 restaurantsContainer와 비슷하니 형식을 복사해서 수정해보도록 하자.

**CategoriesContainer.jsx**

```jsx
import React from 'react';

import Restaurants from './Restaurants';
import { useSelector } from 'react-redux';

export default function CategoriesContainer() {
  const { restaurants } = useSelector((state) => ({
    restaurants: state.restaurants,
  }));

  return <Restaurants restaurants={restaurants} />;
}
```

**CategoriesContainer.test.jsx**

```
import React from 'react'

import { render } from '@testing-library/react'

import CategoriesContainer from './CategoriesContainer'

import { useSelector} from 'react-redux'

jest.mock('react-redux');

test('CategoriesContainer', () => {
  useSelector.mockImplementation((selector) => selector({
    categories: [
      { id: 1, name: '한식' }
    ]
  }))

  const { getByText } = render((
    <CategoriesContainer />
  ));

  expect(getByText('한식')).not.toBeNull();
})
```

이전에 restaurants를 받아왔던 것에서 categories로 변경 후 임의로 테스트용 값을 넣어준다

**그다음 테스트를 통과하기 위해 CategoriesContainer를 수정해보자!**

```jsx
import React from 'react';

import { useSelector } from 'react-redux';

export default function CategoriesContainer() {
  const { categories } = useSelector((state) => ({
    categories: state.categories,
  }));

  return <p>한식</p>;
}
```

→ 테스트에 통과하기 위해 임의로 한식을 넣었다.

테스트가 정상적으로 통과되는 것을 볼 수 있다.

**그 다음 무엇을 해야할까?**

**CategoriesContainer 안에서 categories를 쓸 수 있도록 설정을 해주자.**

**CategoriesContainer.jsx**

```jsx
import React from 'react';

import { useSelector } from 'react-redux';

import Categories from './Categories';

export default function CategoriesContainer() {
  const { categories } = useSelector((state) => ({
    categories: state.categories,
  }));

  return <Categories categories={categories} />;
}
```

**Categoreis, Categories.test 생성**

Restaurants와 형식이 비슷하니 가져와서 수정해보자!

**Categoreis.jsx**

```jsx
import React from 'react';

export default function Categories({ categories }) {
  return (
    <ul>
      {categories.map((category) => (
        <li key={category.id}>{category.name}</li>
      ))}
    </ul>
  );
}
```

**Categories.test**

```jsx
import React from 'react';

import { render } from '@testing-library/react';

import Categories from './Categories';

jest.mock('react-redux');

test('Categories', () => {
  const categories = [{ id: 1, name: '한식' }];

  const { getByText } = render(<Categories categories={categories} />);

  expect(getByText('한식')).not.toBeNull();
});
```

![](https://velog.velcdn.com/images/yxxnhx/post/4cf78abb-abcd-4adc-b359-f58db17e1d4a/image.png)

이렇게 하면 App에서 프로퍼티를 못 받아와 map을 돌릴 수 없다고 에러가 따는 것을 확인할 수 있다.

**App.test.jsx**

```jsx
useSelector.mockImplementation((selector) =>
  selector({
    restaurants: [],
    restaurant: {},
    categories: [],
  }),
);
```

→ 테스트에 categories를 빈값으로 기본값을 잡아주지 않아서 일어난 에러이다.

위와 같이 빈값으로 기본값을 설정해주니 더이상 에러가 나지 않는 것을 확인할 수 있다.

이제 여기까지 카테고리를 가져올 준비는 끝났다.

**이제는 진짜로 api에서 가져와서 보여주는 작업을 해보자!**

### 첫번째로, app에서 API를 호출해올 것이다!

REST에는 CRUD가 있다.

그 중에 R은 Read로, 복수형으로 할 때는 collection, 단수형으로 할 때는 member, element를 이용한다.

지금은 카테고리 전체를 받아올 것이기 때문에 `/categories`로 받아오면 된다.

단수형으로 받아올 때에는 `/categories/13` 등으로 받아올 수 있다.

여기에서 13은 인덱스나 아이디 번호이다.

```jsx
function loadCategories({ dispatch }) {
  const categories = [];

  dispatch(setCategories(categories));
}
```

즉, 여기에서는 얻은 categories를 setCategories를 잡아줄 것이라는 뜻이 된다.

그렇다면 setCategories를 설정해보자!

**redudcer.test.js**

```jsx
describe('setCategories', () => {
  it('changes categories', () => {
    const categories = [{ id: 1, name: '한식' }];
    const initialState = {
      categories: [],
    };

    const state = reducer(initialState, setCategories(categories));

    expect(state.categories).toHaveLength(1);
  });
});
```

먼저 setCategories는 categories를 변경시켜준다는 테스트 시나리오를 짠 후,

초기값으로 categorie는 빈배열로 잡아준다.

그 다음에 state로 초기값과 setCategoreis 안에 테스트용 categories 데이터를 넣어준다.

그리고 테스트를 테스트용 데이터 한개를 넣어두었으니 categories의 길이가 1이 된 것을 확인해본다.

![](https://velog.velcdn.com/images/yxxnhx/post/debf2d29-f0c3-4dec-8529-90e9c60033b8/image.png)

위와 같이 설정을 하면 reducer가 아무것도 하지 않기 때문에 아무런 일도 일어나지 않는 것을 확인할 수 있다.

**이제 reducer.js에서 setCategories를 설정해주자!**

```jsx
if (action.type === 'setCategories') {
  return {
    ...state,
    categories: action.payload.categories,
  };
}
```

위의 코드가 너무 기니 categories를 action.payload에서 구조분해할당을 사용해서 아래와 같이 변경시킬 수 있다.

```jsx
if (action.type === 'setCategories') {
  const { categories } = action.payload;
  return {
    ...state,
    categories,
  };
}
```

이렇게 reducer까지 설정을 해준 후 테스트를 보면 정상적으로 통과된 것을 확인할 수 있다.

### 이제 마지막으로 진짜 api에서 fetch를 해보자

**App.jsx**

```jsx
function loadCategories({ dispatch }) {
  const categories = fetchCategories();

  dispatch(setCategories(categories));
}
```

api 서버를 호출하는 fetchCategories를 폴더와 파일로 분리를 해보자

**src > services > api.js**

```jsx
*import* { fetchCategories } *from* './services/api';
```

그리고 이와 같이 비동기로 처리하는 경우에는 promise를 사용해야 한다.

**async await를 이용하여 코드를 변경해보자**

```jsx
async function loadCategories({ dispatch }) {
  const categories = await fetchCategories();

  dispatch(setCategories(categories));
}
```

만약 이렇게 async await를 사용하지 않으면 프로미스 객체를 사용하기 어렵다.

```jsx
fetchCategories().then((categories) => {});
```

혹은 이렇게 then을 이용하여 프로미스를 사용해서 코드가 복잡해지므로 async await를 이용하여 만들어주자.

**이제는 fetchCategories가 없어서 에러가 나니 api.js에 가서 설정을 해주자.**

**api.js**

```jsx
export async function fetchCategories() {
  return {};
}
```

일단 먼저 테스트를 통과하기 위해 빈 fetchCategories를 설정한 후 브라우저에서 확인을 해보자.

![](https://velog.velcdn.com/images/yxxnhx/post/8252d5dc-3573-4fba-8d42-e717152d4054/image.png)

그런데 위와 같이 map을 돌릴 프로퍼티를 찾을 수 없다는 에러를 볼 수 있다.

그렇다면 뭔가 categories가 제대로 전달이 되고 있지 않다는 뜻이다.

categories를 처리하는 곳에서 문제가 생겼다는 것이니 한번 확인해보자.

reducer에서 초기값을 설정을 해주지 않아 일어난 일이다.

다음과 같이 categories의 초기값을 추가해주면 정상적으로 화면이 출력되는 것을 확인할 수 있다.

```jsx
const initialState = {
  newId: 100,
  restaurants: [],
  restaurant: initialRestaurant,
  categories: [],
};
```

### 그렇다면 이제 API를 불러와서 카테고리 목록을 보여주는 작업을 해보자

API를 불러오는 방법은 fetch를 이용하는 것이다.

```jsx
export async function fetchCategories() {
  const url = 'https://eatgo-customer-api.ahastudio.com/categories';
  const response = await fetch(url);
  console.log(response);

  return {};
}
```

이와 같이 fetch를 이용하여 url을 받아와서 이 response를 콘솔에 찍어보면 다음과 같이 특이한 형태 객체로 받아와지게 된다.

![](https://velog.velcdn.com/images/yxxnhx/post/b8a2167b-f391-4a0c-9a92-54f685fe3177/image.png)

그러나 이 데이터를 JSON 형태로 받기를 원한다.

그럴 때는 json()을 이용하여 한번 받아보자.

```jsx
const data = *await* response.json();
```

![](https://velog.velcdn.com/images/yxxnhx/post/a70f53b9-e10b-4f4a-aee0-669a7a843b47/image.png)

json()을 이용하면 api 데이터를 json으로 파싱해서 배열로 데이터를 가져오게 되는 것이다.

```jsx
export async function fetchCategories() {
  const url = 'https://eatgo-customer-api.ahastudio.com/categories';
  const response = await fetch(url);
  const data = await response.json();

  return data;
}
```

결국 이렇게 데이터를 반환해주면 아래와 같이 API에 있던 데이터들이 화면에 출력되는 것을 확인할 수 있다.

![](https://velog.velcdn.com/images/yxxnhx/post/6e836602-7947-442f-bbaa-c05c941e83cc/image.png)

### **이제 그렇다면 테스트를 마저 만들어주자!**

API를 이용하여 데이터를 받아오는 것이 테스트에는 반영이 되어있지 않기 때문이다.

![](https://velog.velcdn.com/images/yxxnhx/post/6304ae89-38f2-424a-9500-7af07e750265/image.png)

실제로 테스트에서도 fetch가 없다는 에러가 뜨는 것을 볼 수 있다.

**그렇다면 어떻게 해야할까?**

services를 마찬가지로 mocking을 해주자.

**src > services > **mocks** > api.js**

```jsx
export async function fetchCategories() {
  return [];
}

export function xxx() {
  return {};
}
```

**App.test.jsx**

```jsx
jest.mock('./services/api');
```

이렇게 가짜로 api로 데이터를 가져올 수 있게 mocks를 만들어서 설정을 해주니 테스트가 정상적으로 통과되는 것을 볼 수 있다.

**App.jsx**

```jsx
async function loadCategories({ dispatch }) {
  const categories = await fetchCategories();
  dispatch(setCategories(categories));
}

/* 생략 */

useEffect(() => {
  loadRestaurants({ dispatch });
  loadCategories({ dispatch });
}, []);
```

그런데 지금 형식을 보면 위와 같이 fetch를 해서 불러오고 다시 또 넣고 하는 복잡함이 있다.

이것을 그냥 간단하게 아래와 같은 형식으로 부를 수 있다면 얼마나 좋을까.

```jsx
useEffect(() => {
  dispatch(loadCategories());
}, []);
```

기존에는 액션을 넣어주면 reducer가 기존 상태에서 새로운 상태로 바뀌는데,

그게 아니라 그냥 바로 loadCategories라는 액션을 받았는데 dispatch가 알아서 바로 비동기 액션을 처리해주고 상태를 업데이트해주는 형식으로 변경을 해보자.

이것을 위해서 필요한 미들웨어가 있다.

우리가 지금까지 작성한 코드들은 모두 동기 로직이었다. 액션이 dispatch 될 때 마다 상태가 즉시 업데이트되는 형식이었다.

만약에 우리가 비동기적인 로직을 실행하고 싶다면 어떻게 할 수 있을까?

예를 들면 특정한 서버로부터 데이터를 요청 같은 행위들이 있다.

### **Redux Thunk middleware**

`Redux Thunk middleware`는 Action creator가 액션을 반환하는 대신에 함수를 반환한다.

그래서 특정 액션이 실행되는 것을 지연시키거나 특정한 조건이 충족될 때만 액션이 실행될 수 있도록 할 수 있다.

```jsx
const INCREMENT_COUNTER = 'INCREMENT_COUNTER';

function increment() {
  return {
    type: INCREMENT_COUNTER,
  };
}

function incrementAsync() {
  return (dispatch) => {
    setTimeout(() => {
      // Yay! Can invoke sync or async actions with `dispatch`
      dispatch(increment());
    }, 1000);
  };
}
```

두 번째 파라미터인 `getState`를 이용하여 현재 상태를 불러올 수 있다.

그리고 아무것도 `dispatch`하지 않는다면 아무일도 일어나지 않는다.

```jsx
function incrementIfOdd() {
  return (dispatch, getState) => {
    const { counter } = getState();

    if (counter % 2 === 0) {
      return;
    }

    dispatch(increment());
  };
}
```

### **Redux Thunk 설치하기**

```jsx
npm install redux-thunk
```

⭐️ 스토어를 만들 때 두 번째 인자로 미들웨어에 등록해야 함! ⭐️

```jsx
import { createStore, applyMiddleware } from 'redux';

import thunk from 'redux-thunk';

import reducer from './reducer';

const store = createStore(reducer, applyMiddleware(thunk));
```

**store.js**

```jsx
import { createStore } from 'redux';

import thunk from 'redux-thunk';

import reducer from './reducer';

const store = createStore(reducer, applyMiddleware(thunk));

export default store;
```

그 다음으로 우리가 원래는 action을 아래와 같이 오브젝트로 줬었다.

```jsx
export function setCategories(categories) {
  return {
    type: 'setCategories',
    payload: {
      categories,
    },
  };
}
```

그런데 thunk에서는 비동기 액션을 만들 수 있다.

**어떻게? 비동기 함수로!**

대신에 return을 객체가 아닌 함수로 돌려주는 것이다.

그리고 그 안에서 비동기로 처리가 된다.

```jsx
function incrementIfOdd() {
  return (dispatch, getState) => {
    const { counter } = getState();

    if (counter % 2 === 0) {
      return;
    }

    dispatch(increment());
  };
}
```

**그렇다면 한번 반영을 하여 변경해보자!**

**App.jsx**

```jsx
import { setRestaurants, setCategories, loadCategories } from './actions'

import { fetchCategories } from './services/api';

function loadRestaurants({ dispatch }) {
  const restaurants = [];

  dispatch(setRestaurants(restaurants))
}

async function loadCategories({ dispatch }) {
  const categories = await fetchCategories();
  dispatch(setCategories(categories));
}

export default function App() {
/* 생략 */

  useEffect(() => {
    dispatch(loadCategories());

  }, [])
```

**actions.js**

```jsx
export function loadCategories() {
  return () => {
    // TODO
  };
}
```

→ 이렇게 함수를 리턴하게 틀을 먼저 짜보자.

이 함수 안에서 이전에 만들었었던 아래의 비동기 작업들이 이루어져야 한다.

```jsx
const categories = await fetchCategories();
dispatch(setCategories(categories));
```

한번 작성해보자

```jsx
export function loadCategories() {
  return () => {
    const categories = await fetchCategories();
	  dispatch(setCategories(categories));
  };
}
```

→ 비동기 작업이 이루어져야 하기때문에 비동기 함수로 넘어갈 수 있게 async를 넣어주자.

그리고 dispatch를 알아야 하는데 redux thunk에서는 첫번째 인자로 dispatch를 넣어주게 되어있다.

```jsx
export function loadCategories() {
  return async (dispatch) => {
    const categories = await fetchCategories();
    dispatch(setCategories(categories));
  };
}
```

해당사항을 반영하여 위와 같은 코드를 작성하면 끝!

이제 그렇다면 App에 있던 services의 fetchCategories를 가져와서 사용할 수 있게 설정을 해주면 된다. App에서는 더이상 필요가 없기 때문에 삭제를 해주자.

**App.jsx**

```jsx
import React, { useEffect } from 'react';

import CategoriesContainer from './CategoriesContainer';
import RestaurantsContainer from './RestaurantsContainer';
import RestaurantCreateContainer from './RestaurantCreateContainer';

import { useDispatch } from 'react-redux';

import { setRestaurants, loadCategories } from './actions';

function loadRestaurants({ dispatch }) {
  const restaurants = [];

  dispatch(setRestaurants(restaurants));
}

export default function App() {
  const dispatch = useDispatch();

  useEffect(() => {
    dispatch(loadCategories());
    loadRestaurants({ dispatch });
  }, []);

  return (
    <div>
      <h1>Restaurants</h1>
      <CategoriesContainer />
      <RestaurantsContainer />
      <RestaurantCreateContainer />
    </div>
  );
}
```

지금 보면 위와 같이 굉장히 단순해졌다.

기존에는 App이라는 컴포넌트를 보면 비동기로 처리해야할 내용들이 있고, 또 화면에 보여줄 컴포넌트들을 불러오고 하는 작업들이 있었지만 전부 없어지고 loadCategories라는 액션을 dispatch가 스스로 알아서 해줄 수 있게 되었다.

**그렇다면 마찬가지로 loadRestaurants를 바꿔보자!**

**actions.js**

```jsx
export function loadRestaurants() {
  return async (dispatch) => {
    const restaurants = await fetchCategories();
    dispatch(setRestaurants(restaurants));
  };
}
```

**App.jsx**

```jsx
import React, { useEffect } from 'react';

import CategoriesContainer from './CategoriesContainer';
import RestaurantsContainer from './RestaurantsContainer';
import RestaurantCreateContainer from './RestaurantCreateContainer';

import { useDispatch } from 'react-redux';

import { loadRestaurants, loadCategories } from './actions';

export default function App() {
  const dispatch = useDispatch();

  useEffect(() => {
    dispatch(loadCategories());
    dispatch(loadRestaurants());
  }, []);

  return (
    <div>
      <h1>Restaurants</h1>
      <CategoriesContainer />
      <RestaurantsContainer />
      <RestaurantCreateContainer />
    </div>
  );
}
```

이렇게 App 컴포넌트가 굉장히 깔끔해진 것을 확인할 수 있다.

**이제 그럼 테스트 파일을 확인해보자**

```jsx
expect(dispatch).toBeCalledWith({
  type: 'setRestaurants',
  payload: { restaurants: [] },
});
```

![](https://velog.velcdn.com/images/yxxnhx/post/ca1bff89-09d7-4fa5-b153-baec7f531401/image.png)

비동기 함수를 반환하게 하여 더이상 이전의 테스트가 통과되지 못하는 것을 확인할 수 있다.

일단은 내용을 하지 않고 그냥 실행이 되었다는 여부만 확인을 하게 변경해보자.

```jsx
expect(dispatch).toBeCalled();
```

혹은 dispatch가 카테고리와 레스토랑 총 두번 실행이 되니 두번 실행이 되었다는 여부를 확인할 수도 있다.

```jsx
expect(dispatch).toBeCalledTimes(2);
```

테스트가 정상적으로 통과되는 것을 확인할 수 있다.
