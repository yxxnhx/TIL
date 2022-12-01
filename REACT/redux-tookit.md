# 최적화 하기

이번에는 Redux에서 반복되는 코드를 정리하기 위하여 리덕스 툴킷을 사용해보자.

## **리덕스 툴킷 Redux Toolkit**

- [Redux Toolkit 공식 문서](https://redux-toolkit.js.org/)
- [Redux Toolkit - 한글 번역](https://soyoung210.github.io/redux-toolkit/tutorials/basic-tutorial/)

지금까지 redux를 이용해서 개발하실 때 다들 `아 너무 복잡하다. 무슨 코드가 이렇게 많아야 하지?` 란 생각을 하게 된다. 어플리케이션이 복잡해질수록 다양한 액션과 리듀서에 대한 관리가 복잡해지며 편의를 위한 패키지 추가가 많이 필요했다. `immer`, `ducks pattern` 등을 공식적으로 지원한다. `redux-thunk`가 비동기 처리를 위해 기본으로 포함됐다.

- ducks pattern: redux를 사용할 때 action과 reducer를 하나의 파일에서 사용하는 패턴. redux-toolkit은 이를 slice.js 란 파일에 모아둔다.

그냥 사용해보시면 딱 느껴지실 것이다.

<aside>
💡 와... 편하다!

</aside>

redux의 악명 높았던 사용법이 정말 많이 개선된 걸 느낄 수 있다. 또한 입맛에 따라 `redux-saga`, `redux-observable`와 같은 비동기 처리 패키지를 추가할 수 있다.

### **configureStore**

Redux의 `createStore` 함수에서 개발을 더 쉽게 하게 도와주는 기본 설정들이 들어있는 스토어 생성 함수이다. `configureStore`를 사용하면 기본으로 Redux thunk가 추가된다.

- [기본으로 포함된 미들웨어](https://redux-toolkit.js.org/api/getDefaultMiddleware#included-default-middleware)

**설치**

```jsx
npm install @reduxjs/toolkit
```

### slice

slice에 action creator과 reducer를 동시에 만들어주고, action에 대한 정의까지 한꺼번에 해서 정리를 해보자.

**기본 형식**

```jsx
import { createSlice } from '@reduxjs/toolkit';

const initialState = {
  value: 0,
};

export const counterSlice = createSlice({
  name: 'counter',
  initialState,
  reducers: {
    increment: (state) => {
      // Redux Toolkit allows us to write "mutating" logic in reducers. It
      // doesn't actually mutate the state because it uses the Immer library,
      // which detects changes to a "draft state" and produces a brand new
      // immutable state based off those changes
      state.value += 1;
    },
    decrement: (state) => {
      state.value -= 1;
    },
    incrementByAmount: (state, action) => {
      state.value += action.payload;
    },
  },
});

// Action creators are generated for each case reducer function
export const { increment, decrement, incrementByAmount } = counterSlice.actions;

export default counterSlice.reducer;
```

**slice.js 생성**

```jsx
import { createSlice } from '@reduxjs/toolkit';

const slice = createSlice({});

const { actions, reducer } = slice;
```

이렇게 slice 안에는 action과 reducer이 들어있어서 빼서 사용할 수 있도록 할 수 있다.

그렇다면 애초부터 불러서 사용할 때 아래와 같이 구조분해할당을 이용해서 빼내올 수 있지 않을까?

```jsx
const { actions, reducer } = createSlice({});
```

그리고 꺼내서 사용할 수 있도록 export 시켜주자.

```jsx
*export* { actions, reducer };
```

이제 그렇다면 본격적으로 안에 설정을 해주자.

기본 틀을 참고해보면 `name`, `initailState`, `reducers`를 설정을 해주어야 한다.

```jsx
const { actions, reducer } = createSlice({
  name: '',
  initialState: {},
  reducers: {},
});
```

그렇다면 reducers.js에 있던 내용들을 가져와서 넣어보자.

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
  reviewFields: {
    ...initialReviewFields,
  },
  accessToken: '',
  reviews: '',
};
```

initialState를 그대로 initialState 안에 넣어주기 때문에

```jsx
initialState: initialState;
// 축약
initialState,
```

위와 같이 축약해서 사용할 수 있다.

혹은 위의 내용을 그대로 넣어서 사용해줄 수도 있다.

그렇다면 reducers도 한번 동일하게 설정을 해주자.

```jsx
const reducers = {
  setRegions(state, { payload: { regions } }) {
    return {
      ...state,
      regions,
    };
  },

  setCategories(state, { payload: { categories } }) {
    return {
      ...state,
      categories,
    };
  },

  setRestaurants(state, { payload: { restaurants } }) {
    return {
      ...state,
      restaurants,
    };
  },

  setRestaurant(state, { payload: { restaurant } }) {
    return {
      ...state,
      restaurant,
    };
  },

  selectRegion(state, { payload: { regionId } }) {
    const { regions } = state;

    return {
      ...state,
      selectedRegion: regions.find(equal('id', regionId)),
    };
  },

  selectCategory(state, { payload: { categoryId } }) {
    const { categories } = state;

    return {
      ...state,
      selectedCategory: categories.find(equal('id', categoryId)),
    };
  },

  changeLoginField(state, { payload: { name, value } }) {
    return {
      ...state,
      loginFields: {
        ...state.loginFields,
        [name]: value,
      },
    };
  },

  changeReviewField(state, { payload: { name, value } }) {
    return {
      ...state,
      reviewFields: {
        ...state.reviewFields,
        [name]: value,
      },
    };
  },

  setAccessToken(state, { payload: { accessToken } }) {
    return {
      ...state,
      accessToken,
    };
  },

  setReviews(state, { payload: { reviews } }) {
    const { restaurant } = state;
    return {
      ...state,
      restaurant: {
        ...restaurant,
        reviews,
      },
    };
  },

  logout(state) {
    return {
      ...state,
      accessToken: '',
    };
  },

  clearReviewFields(state) {
    return {
      ...state,
      reviewFields: {
        ...initialReviewFields,
      },
    };
  },
};
```

initialState를 그대로 reducers 안에 넣어주기 때문에

```jsx
reducers: reducers;
// 축약
reducers,
```

위와 같이 축약해서 사용할 수 있다.

혹은 위의 내용을 그대로 넣어서 사용해줄 수도 있다.

이름도 한번 설정해주자. 이에 맞는 이름을 설정해주면 된다. 여기서는 `application`이라고 설정해보자.

전체 application에 대한 reducer들을 만들어주기 때문이다.

```jsx
const { actions, reducer } = createSlice({
  name: 'application',
  initialState,
  reducers,
});
```

- 혹은 위처럼 따로 빼지 말고 바로 넣어보자.
  ```
  import { createSlice } from '@reduxjs/toolkit';

  import { equal } from './utils';

  const initialReviewFields = {
    score: '',
    description: '',
  };

  const { actions, reducer } = createSlice({
    name: 'application',
    initialState: {
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
      reviewFields: {
        ...initialReviewFields,
      },
      accessToken: '',
      reviews: '',
    },
    reducers: {
      setRegions(state, { payload: { regions } }) {
        return {
          ...state,
          regions,
        };
      },

      setCategories(state, { payload: { categories } }) {
        return {
          ...state,
          categories,
        };
      },

      setRestaurants(state, { payload: { restaurants } }) {
        return {
          ...state,
          restaurants,
        };
      },

      setRestaurant(state, { payload: { restaurant } }) {
        return {
          ...state,
          restaurant,
        };
      },

      selectRegion(state, { payload: { regionId } }) {
        const { regions } = state;

        return {
          ...state,
          selectedRegion: regions.find(equal('id', regionId)),
        };
      },

      selectCategory(state, { payload: { categoryId } }) {
        const { categories } = state;

        return {
          ...state,
          selectedCategory: categories.find(equal('id', categoryId)),
        };
      },

      changeLoginField(state, { payload: { name, value } }) {
        return {
          ...state,
          loginFields: {
            ...state.loginFields,
            [name]: value,
          },
        };
      },

      changeReviewField(state, { payload: { name, value } }) {
        return {
          ...state,
          reviewFields: {
            ...state.reviewFields,
            [name]: value,
          },
        };
      },

      setAccessToken(state, { payload: { accessToken } }) {
        return {
          ...state,
          accessToken,
        };
      },

      setReviews(state, { payload: { reviews } }) {
        const { restaurant } = state;
        return {
          ...state,
          restaurant: {
            ...restaurant,
            reviews,
          },
        };
      },

      logout(state) {
        return {
          ...state,
          accessToken: '',
        };
      },

      clearReviewFields(state) {
        return {
          ...state,
          reviewFields: {
            ...initialReviewFields,
          },
        };
      },
    },
  });

  export { actions, reducer };
  ```

이제 그렇다면 reducer.js에서 slice에 있는 reducer를 사용할 수 있게 해보자.

```jsx
import { reducer } from './slice';

export default reducer;
```

이제 action을 정리해보자.

action에서 비동기로 처리하는 것 외의 것들은 reducer과 동일하기 때문에 정리해보자.

```jsx
import {
  fetchCategories,
  fetchRegions,
  fetchRestaurants,
  fetchRestaurant,
  postLogin,
  postReview,
} from './services/api';

import { actions } from './slice';

export const {
  setRegions,
  setCategories,
  setRestaurants,
  setRestaurant,
  selectRegion,
  selectCategory,
  changeLoginField,
  changeReviewField,
  setAccessToken,
  setReviews,
  logout,
  clearReviewFields,
} = actions;

export function loadInitialData() {
  return async (dispatch) => {
    const regions = await fetchRegions();
    dispatch(setRegions(regions));

    const categories = await fetchCategories();
    dispatch(setCategories(categories));
  };
}

export function loadRestaurants() {
  return async (dispatch, getState) => {
    const { selectedRegion: region, selectedCategory: category } = getState();

    if (!region || !category) {
      return;
    }

    const restaurants = await fetchRestaurants({
      regionName: region.name,
      categoryId: category.id,
    });

    dispatch(setRestaurants(restaurants));
  };
}

export function loadRestaurant({ restaurantId }) {
  return async (dispatch) => {
    dispatch(setRestaurant(null));

    const restaurant = await fetchRestaurant({ restaurantId });

    dispatch(setRestaurant(restaurant));
  };
}

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

export function loadReview({ restaurantId }) {
  return async (dispatch) => {
    const restaurant = await fetchRestaurant({ restaurantId });

    dispatch(setReviews(restaurant.reviews));
  };
}

export function sendReview({ restaurantId }) {
  return async (dispatch, getState) => {
    const {
      accessToken,
      reviewFields: { score, description },
    } = getState();

    await postReview({ accessToken, restaurantId, score, description });

    dispatch(loadReview({ restaurantId }));

    dispatch(clearReviewFields());
  };
}
```

→ 이와 같이 비동기 처리 외에는 reducer와 모두 동일하였기 때문에 정리를 해줄 수 있다.

그러나 여기서 테스트를 확인해보면 에러가 뜨는 것을 확인할 수 있다.

redux toolkit을 사용하게 되면 자동으로 payload 형식을 만들어주기 때문에 일어나는 에러이다.

기존에는 아래와 같이 payload로 꺼내올 것을 구조분해할당을 하여 오브젝트 형식으로 꺼내오게 하였었다.

```jsx
setReviews(state, { payload: { reviews }}) {
  const { restaurant } = state;
  return {
    ...state,
    restaurant: {
      ...restaurant,
      reviews,
    },
  };
},
```

그런데 이렇게 하지 않고 해결할 수 있는 방법은 두 가지가 있다.

- **사용하는 부분에서 설정해주기**

```jsx
export function loadReview({ restaurantId }) {
  return async (dispatch) => {
    const restaurant = await fetchRestaurant({ restaurantId });

    dispatch(setReviews({ restaurant.reviews }));
  };
}
```

→ 이렇게 사용하는 부분에서 정확하게 오브젝트 형식으로 꺼내오게 시킨다.

- **slice에서 설정할 때부터 지정해주기**

```
setReviews(state, { payload: reviews }) {
  const { restaurant } = state;
  return {
    ...state,
    restaurant: {
      ...restaurant,
      reviews,
    },
  };
},
```

이렇게 설정을 해주자.

그렇다면 이제 각각 테스트에서 type이 잘못 설정했다고 에러가 뜨는 것을 볼 수 있다.

```jsx
it('call dispatch with "setAccessToken" action', () => {
  renderApp({ path: '/' });

  expect(dispatch).toBeCalledWith({
    type: 'setAccessToken',
    payload: accessToken,
  });
});
```

여기에서 이제는 application이라는 이름을 가진 slice의 action이기 때문에 아래와 같이 수정해주면 된다.

```jsx
type: 'application/setAccessToken',
```

→ 전체적으로 type에 application을 붙여주면 테스트는 정상적으로 통과되는 것을 볼 수 있다.

이렇게 전체적으로 정리를 해보니 중복되어서 지정한 것들이 모두 사라지는 것을 볼 수 있다.

- **actions.js**
  ```jsx
  import {
    fetchCategories,
    fetchRegions,
    fetchRestaurants,
    fetchRestaurant,
    postLogin,
    postReview,
  } from './services/api';

  import { actions } from './slice';

  export const {
    setRegions,
    setCategories,
    setRestaurants,
    setRestaurant,
    selectRegion,
    selectCategory,
    changeLoginField,
    changeReviewField,
    setAccessToken,
    setReviews,
    logout,
    clearReviewFields,
  } = actions;

  export function loadInitialData() {
    return async (dispatch) => {
      const regions = await fetchRegions();
      dispatch(setRegions(regions));

      const categories = await fetchCategories();
      dispatch(setCategories(categories));
    };
  }

  export function loadRestaurants() {
    return async (dispatch, getState) => {
      const { selectedRegion: region, selectedCategory: category } = getState();

      if (!region || !category) {
        return;
      }

      const restaurants = await fetchRestaurants({
        regionName: region.name,
        categoryId: category.id,
      });

      dispatch(setRestaurants(restaurants));
    };
  }

  export function loadRestaurant({ restaurantId }) {
    return async (dispatch) => {
      dispatch(setRestaurant(null));

      const restaurant = await fetchRestaurant({ restaurantId });

      dispatch(setRestaurant(restaurant));
    };
  }

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

  export function loadReview({ restaurantId }) {
    return async (dispatch) => {
      const restaurant = await fetchRestaurant({ restaurantId });

      dispatch(setReviews(restaurant.reviews));
    };
  }

  export function sendReview({ restaurantId }) {
    return async (dispatch, getState) => {
      const {
        accessToken,
        reviewFields: { score, description },
      } = getState();

      await postReview({ accessToken, restaurantId, score, description });

      dispatch(loadReview({ restaurantId }));

      dispatch(clearReviewFields());
    };
  }
  ```
- **reducer.js**
  ```jsx
  import { reducer } from './slice';

  export default reducer;
  ```
- **slice.js**
  ```jsx
  import { createSlice } from '@reduxjs/toolkit';

  import { equal } from './utils';

  const initialReviewFields = {
    score: '',
    description: '',
  };

  const { actions, reducer } = createSlice({
    name: 'application',
    initialState: {
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
      reviewFields: {
        ...initialReviewFields,
      },
      accessToken: '',
      reviews: '',
    },
    reducers: {
      setRegions(state, { payload: regions }) {
        return {
          ...state,
          regions,
        };
      },

      setCategories(state, { payload: categories }) {
        return {
          ...state,
          categories,
        };
      },

      setRestaurants(state, { payload: restaurants }) {
        return {
          ...state,
          restaurants,
        };
      },

      setRestaurant(state, { payload: restaurant }) {
        return {
          ...state,
          restaurant,
        };
      },

      selectRegion(state, { payload: regionId }) {
        const { regions } = state;

        return {
          ...state,
          selectedRegion: regions.find(equal('id', regionId)),
        };
      },

      selectCategory(state, { payload: categoryId }) {
        const { categories } = state;

        return {
          ...state,
          selectedCategory: categories.find(equal('id', categoryId)),
        };
      },

      changeLoginField(state, { payload: { name, value } }) {
        return {
          ...state,
          loginFields: {
            ...state.loginFields,
            [name]: value,
          },
        };
      },

      changeReviewField(state, { payload: { name, value } }) {
        return {
          ...state,
          reviewFields: {
            ...state.reviewFields,
            [name]: value,
          },
        };
      },

      setAccessToken(state, { payload: accessToken }) {
        return {
          ...state,
          accessToken,
        };
      },

      setReviews(state, { payload: reviews }) {
        const { restaurant } = state;
        return {
          ...state,
          restaurant: {
            ...restaurant,
            reviews,
          },
        };
      },

      logout(state) {
        return {
          ...state,
          accessToken: '',
        };
      },

      clearReviewFields(state) {
        return {
          ...state,
          reviewFields: {
            ...initialReviewFields,
          },
        };
      },
    },
  });

  export { actions, reducer };
  ```

이전에는 action에 일어날 일들을 지정하고 reducer에 가서 그 action과 payload를 지정하고 또 테스트를 하는 단계에서 오타가 하나라도 나면 모든 테스트가 깨지는 어려움이 있었지만 이제는 slice에서 지정만 제대로 해주면 actions에서 import 해서 사용만 하면 된다!

**그렇다면 여기서 조금 더 가볍게 처리를 해보자!**

slice에서는 reducer를 기본적으로 뽑아서 줄 수 있게 처리를 해보자.

**slice.js**

```jsx
*export* *default* reducer;
```

그렇다면 reducer의 테스트에서는 reducer를 slice에서 바로 뽑아서 쓸 수 있게 해주자

```jsx
*import* reducer *from* './slice';
```

**이제 그렇다면 slice의 action들은 어떻게 할까?**

바깥으로 뽑아서 쓸 수 있도록 처리를 해주자.

```jsx
export const {
  setRegions,
  setCategories,
  setRestaurants,
  setRestaurant,
  selectRegion,
  selectCategory,
  changeLoginField,
  changeReviewField,
  setAccessToken,
  setReviews,
  logout,
  clearReviewFields,
} = actions;
```

즉, default로 얻으면 reducer이 되고 하나씩 뽑으면 action들이 나갈 수 있도록 처리해주는 것이다.

**actions.js**

```jsx
import {
  setRegions,
  setCategories,
  setRestaurants,
  setRestaurant,
  selectRegion,
  selectCategory,
  changeLoginField,
  changeReviewField,
  setAccessToken,
  setReviews,
  logout,
  clearReviewFields,
} from './slice';
```

이렇게 뽑아오게 되는 것이다.

위와 같이 설정을 하면 export를 해주지 않아 모든 테스트가 깨지는 것을 볼 수 있다. 다시 export를 해주자

```jsx
export {
  setRegions,
  setCategories,
  setRestaurants,
  setRestaurant,
  selectRegion,
  selectCategory,
  changeLoginField,
  changeReviewField,
  setAccessToken,
  setReviews,
  logout,
  clearReviewFields,
};
```

그렇다면 이제 비동기 action들은 그대로 actions에 남겨두었는데 비동기들도 slice로 모두 옮기면 어떨까?

**slice.js**

```jsx
import {
  fetchCategories,
  fetchRegions,
  fetchRestaurants,
  fetchRestaurant,
  postLogin,
  postReview,
} from './services/api';

import { saveItem } from './services/storage';

import { createSlice } from '@reduxjs/toolkit';

import { equal } from './utils';

const initialReviewFields = {
  score: '',
  description: '',
};

const { actions, reducer } = createSlice({
  name: 'application',
  initialState: {
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
    reviewFields: {
      ...initialReviewFields,
    },
    accessToken: '',
    reviews: '',
  },
  reducers: {
    setRegions(state, { payload: regions }) {
      return {
        ...state,
        regions,
      };
    },

    setCategories(state, { payload: categories }) {
      return {
        ...state,
        categories,
      };
    },

    setRestaurants(state, { payload: restaurants }) {
      return {
        ...state,
        restaurants,
      };
    },

    setRestaurant(state, { payload: restaurant }) {
      return {
        ...state,
        restaurant,
      };
    },

    selectRegion(state, { payload: regionId }) {
      const { regions } = state;

      return {
        ...state,
        selectedRegion: regions.find(equal('id', regionId)),
      };
    },

    selectCategory(state, { payload: categoryId }) {
      const { categories } = state;

      return {
        ...state,
        selectedCategory: categories.find(equal('id', categoryId)),
      };
    },

    changeLoginField(state, { payload: { name, value } }) {
      return {
        ...state,
        loginFields: {
          ...state.loginFields,
          [name]: value,
        },
      };
    },

    changeReviewField(state, { payload: { name, value } }) {
      return {
        ...state,
        reviewFields: {
          ...state.reviewFields,
          [name]: value,
        },
      };
    },

    setAccessToken(state, { payload: accessToken }) {
      return {
        ...state,
        accessToken,
      };
    },

    setReviews(state, { payload: reviews }) {
      const { restaurant } = state;
      return {
        ...state,
        restaurant: {
          ...restaurant,
          reviews,
        },
      };
    },

    logout(state) {
      return {
        ...state,
        accessToken: '',
      };
    },

    clearReviewFields(state) {
      return {
        ...state,
        reviewFields: {
          ...initialReviewFields,
        },
      };
    },
  },
});

export const {
  setRegions,
  setCategories,
  setRestaurants,
  setRestaurant,
  selectRegion,
  selectCategory,
  changeLoginField,
  changeReviewField,
  setAccessToken,
  setReviews,
  logout,
  clearReviewFields,
} = actions;

export function loadInitialData() {
  return async (dispatch) => {
    const regions = await fetchRegions();
    dispatch(setRegions(regions));

    const categories = await fetchCategories();
    dispatch(setCategories(categories));
  };
}

export function loadRestaurants() {
  return async (dispatch, getState) => {
    const { selectedRegion: region, selectedCategory: category } = getState();

    if (!region || !category) {
      return;
    }

    const restaurants = await fetchRestaurants({
      regionName: region.name,
      categoryId: category.id,
    });

    dispatch(setRestaurants(restaurants));
  };
}

export function loadRestaurant({ restaurantId }) {
  return async (dispatch) => {
    dispatch(setRestaurant(null));

    const restaurant = await fetchRestaurant({ restaurantId });

    dispatch(setRestaurant(restaurant));
  };
}

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

export function loadReview({ restaurantId }) {
  return async (dispatch) => {
    const restaurant = await fetchRestaurant({ restaurantId });

    dispatch(setReviews(restaurant.reviews));
  };
}

export function sendReview({ restaurantId }) {
  return async (dispatch, getState) => {
    const {
      accessToken,
      reviewFields: { score, description },
    } = getState();

    await postReview({ accessToken, restaurantId, score, description });

    dispatch(loadReview({ restaurantId }));

    dispatch(clearReviewFields());
  };
}

export default reducer;
```

**actions.js**

```jsx
import {
  setRegions,
  setCategories,
  setRestaurants,
  setRestaurant,
  selectRegion,
  selectCategory,
  changeLoginField,
  changeReviewField,
  setAccessToken,
  setReviews,
  logout,
  clearReviewFields,
} from './slice';

export {
  setRegions,
  setCategories,
  setRestaurants,
  setRestaurant,
  selectRegion,
  selectCategory,
  changeLoginField,
  changeReviewField,
  setAccessToken,
  setReviews,
  logout,
  clearReviewFields,
};
```

이렇게 비동기 action까지 모두 옮기니 actions 파일이 더이상 필요가 없어졌다.

그렇다면 actions 파일을 지우고 actions를 import 해왔던 부분들을 모두 고쳐주자

모든 테스트가 통과되는 것을 확인할 수 있다.

그렇다면 마찬가지로 reducer도 그에 대한 내용이 별로 없다.

```jsx
import { reducer } from './slice';

// function defaultReducer(state) {
//   return state;
// }

// export default function reducer(state = initialState, action) {
//   return (reducers[action.type] || defaultReducer)(state, action);
// }
export default reducer;
```

그러므로 reducer도 함께 지워서 정리해주자

**store.js**

```jsx
import { applyMiddleware, createStore } from 'redux';

import thunk from 'redux-thunk';

import reducer from './slice';

const store = createStore(reducer, applyMiddleware(thunk));

export default store;
```

전에는 actions과 reducer 두 파일의 연결에 민감했다. 동일한 type과 payload이어야만 일어날 수 있도록 말이다.

지금은 slice 파일 하나에서 모든 것을 다같이 내려주고 있다.

이것을 ducks pattern이라고 한다.

### ducks pattern

```jsx
// widgets.js

// Actions
const LOAD = 'my-app/widgets/LOAD';
const CREATE = 'my-app/widgets/CREATE';
const UPDATE = 'my-app/widgets/UPDATE';
const REMOVE = 'my-app/widgets/REMOVE';

// Reducer
export default function reducer(state = {}, action = {}) {
  switch (action.type) {
    // do reducer stuff
    default:
      return state;
  }
}

// Action Creators
export function loadWidgets() {
  return { type: LOAD };
}

export function createWidget(widget) {
  return { type: CREATE, widget };
}

export function updateWidget(widget) {
  return { type: UPDATE, widget };
}

export function removeWidget(widget) {
  return { type: REMOVE, widget };
}

// side effects, only as applicable
// e.g. thunks, epics, etc
export function getWidget() {
  return (dispatch) =>
    get('/widget').then((widget) => dispatch(updateWidget(widget)));
}
```

이렇게 파일 하나에서 action이 있고, reducer이 있고, action creator이 있는 모든 걸 정리하는것을 제안하는 것이다.

이것을 redux toolkit에서 받아들인 것이다.

참고로 우리는 이전에 redux thunk를 직접 설치해서 사용했는데 toolkit에서는 지원한다.

```json
"dependencies": {
  "@emotion/react": "^11.10.5",
  "@emotion/styled": "^11.10.5",
  "@reduxjs/toolkit": "^1.9.0",
  "react": "^18.2.0",
  "react-dom": "^18.2.0",
  "react-redux": "^8.0.4",
  "react-router-dom": "^6.4.3",
  "redux": "^4.2.0",
  "redux-thunk": "^2.4.2"
}
```

그러므로 uninstall을 해도 정상적으로 작동을 한다.

```jsx
npm uninstall redux-thunk
```

정리하자면, Redux Toolkit은 스토어를 만들 때 편리하게 만들 수 있도록, 기본 옵션들이 포함된 store를 만들 수 있게 해주는 `configureStore`함수를 지원한다. 그리고 이 함수로 store를 만들 때 기본으로 `thunk` 미들웨어를 지원한다. 따라서 `createStore` 대신 `configureStore`함수를 이용해서 store를 생성해야 올바르게 동작한다.
