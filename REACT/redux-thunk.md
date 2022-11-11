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
