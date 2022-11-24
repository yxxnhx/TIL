# Redux HTTPie를 활용한 리뷰 작성하기

로그인을 통해서 얻은 accessToken으로 리뷰 작업을 해보자.
리뷰 외에도 로그인을 해서 얻은 권한으로 처리할 수 있는 것들은 전부 이러한 방식으로 진행될 것이다.
사용자 정보 인증을 하는 것을 Authentication이라고 한다.
인증을 받아 내가 쓸 수 있는 것을 Authorization이라고 한다.
Authentication을 인증, Authorization 인가라고 한다.

### **Authorization 헤더**

- MDN - [Authorization](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Authorization)

HTTP Authorization 요청 헤더는 유저 에이전트에서 서버에 인증정보를 전달하기 위해 사용된다.

```jsx
Authorization: <type> <credentials>
Authorization: Basic YWxhZGRpbjpvcGVuc2VzYW1l
```

서버에 type과 credentials을 전달해야 한다.
type에 대하여 잠시 간단히 알아보자면 다음과 같다

- Basic
- Bearer
- Digest
- HOBA
- Mutual

인증 방식에 따라서 type이 갈리고 현재 이번에는 Bearer 타입을 사용해보자.

그렇다면 우리가 지난번에 로그인을 하여 받은 accessToken으로 처리를 해보자.

<aside>
💡 eyJhbGciOiJIUzI1NiJ9.eyJ1c2VySWQiOjMsIm5hbWUiOiLthYzsiqTthLAifQ.3qbijgcDDeFcVviXBZ45BS8CgdtpQvaCoXTkktEsPks

</aside>

먼저 `restaurants/1` 에 대해서 가져와보자

```jsx
http GET https://eatgo-customer-api.ahastudio.com/restaurants/1
```

![](https://velog.velcdn.com/images/yxxnhx/post/ccd1d503-0269-4da6-a571-bc82c4ff39c1/image.png)

→ `restaurants/1`에 달린 리뷰들을 받아와지는 것을 볼 수 있다.

이제 그렇다면 리뷰들을 추가하는 작업을 해보자.

```jsx
http POST [https://eatgo-customer-api.ahastudio.com/restaurants/1/reviews](https://eatgo-customer-api.ahastudio.com/restaurants/1/reviews) score=5
description=good
"Authorization: Bearer eyJhbGciOiJIUzI1NiJ9.eyJ1c2VySWQiOjMsIm5hbWUiOiLthYzsiq
TthLAifQ.3qbijgcDDeFcVviXBZ45BS8CgdtpQvaCoXTkktEsPks"
```

![](https://velog.velcdn.com/images/yxxnhx/post/a3d7a3ba-8722-49e8-9f71-0daad1c43100/image.png)

→ 201 정상적으로 Created 생성되었다는 코드가 나오는 것을 확인할 수 있다.

이제 다시 달린 리뷰를 확인해보자.

![](https://velog.velcdn.com/images/yxxnhx/post/9517e2d5-ca37-406a-a35f-fdb157caefb6/image.png)

→ 가장 마지막에 방금 입력한 리뷰 데이터가 추가되어 있는 것을 확인할 수 있다.

이것을 이제 웹으로 출력하고 남겨보는 작업을 해보자!

**그렇다면 리뷰를 남기는 것은 어디에서 보여야 할까?**

![](https://velog.velcdn.com/images/yxxnhx/post/6ef1dd4d-0dd3-4089-98b1-2acc03d01dba/image.png)

여기 상세페이지 아래에 입력하는 form이 있어야 한다.

**RestaurantPage.test.jsx**

먼저 테스트로 점검하고 추가하자

```jsx
it('renders review write form', () => {
  const params = { id: 1 };

  const { queryByLabelText } = render(<RestaurantPage params={params} />);

  expect(queryByLabelText('평점')).not.toBeNull();
});
```

당연히 null이 나오고 테스트가 깨질 것이다.

그렇다면 form을 그려주기 전에 RestaurantContainer 테스트에서도 점검하고 추가해주자.

**RestaurantContainer.test.jsx**

```jsx
it('renders review write form', () => {
  const { queryByLabelText } = render(<RestaurantContainer restaurantId="1" />);

  expect(queryByLabelText('평점')).not.toBeNull();
});
```

여전히 평점이 없어서 null이 뜨고 테스트가 깨질 것이다.

이제 그렇다면 평점 form을 그려주자.

**RestaurantContainer.jsx**

```jsx
function ReviewForm() {
  return (
    <div>
      <label htmlFor="review-score">평점</label>
      <input type="text" id="review-score" />
    </div>
  );
}

/* 생략 */

return (
  <>
    <ReviewForm />
    <RestaurantDetail restaurant={restaurant} />
  </>
);
```

정상적으로 테스트가 통과되는 것을 확인할 수 있다.

**그렇다면 이제 해주어야 하는 것은 무엇일까?**

평점을 select 해서 바꿨더니 넘어가고 description을 처리해주는 것이다.

```jsx
it('renders review write form', () => {
    const { getByLabelText } = render((
      <RestaurantContainer restaurantId='1'/>
    ));

    fireEvent.change(getByLabelText('평점'), {
      target: { value: '5' }
    })

    fireEvent.change(getByLabelText('리뷰'), {
      target: { value: '존맛탱🥰' }
    })
  })
})
```

이제 그렇다면 리뷰를 추가해주자.

```jsx
function ReviewForm() {
  return (
    <>
      <div>
        <label htmlFor="review-score">평점</label>
        <input type="number" id="review-score" />
      </div>
      <div>
        <label htmlFor="review-description">리뷰</label>
        <input type="text" id="review-description" />
      </div>
    </>
  );
}
```

→ 테스트가 정상적으로 통과되는 것을 볼 수 있다.

이제 그렇다면 handleChange를 통해 값이 잘 들어오는지 확인하자

```jsx
it('renders review write form', () => {
  const { getByLabelText } = render(<RestaurantContainer restaurantId="1" />);

  fireEvent.change(getByLabelText('평점'), {
    target: { value: '5' },
  });

  expect(dispatch).toBeCalledWith({
    type: 'changeReviewField',
    payload: { name: 'score', value: '5' },
  });

  fireEvent.change(getByLabelText('리뷰'), {
    target: { value: '존맛탱🥰' },
  });

  expect(dispatch).toBeCalledWith({
    type: 'changeReviewField',
    payload: { name: 'description', value: '존맛탱🥰' },
  });
});
```

호출이 되지 않아 에러가 뜨니 다시 컴포넌트로 돌아가서 handleChange를 처리하자

```jsx
function ReviewForm({ onChange }) {
  function handleChange(event) {
    const {
      target: { name, value },
    } = event;
    onChange({ name, value });
  }

  return (
    <>
      <div>
        <label htmlFor="review-score">평점</label>
        <input
          type="number"
          id="review-score"
          name="score"
          onChange={handleChange}
        />
      </div>
      <div>
        <label htmlFor="review-description">리뷰</label>
        <input
          type="text"
          id="review-description"
          name="description"
          onChange={handleChange}
        />
      </div>
    </>
  );
}

export default function RestaurantContainer({ restaurantId }) {
  /* 생략 */
  function handleChange({ name, value }) {
    dispatch(changeReviewField({ name, value }));
  }

  return (
    <>
      <RestaurantDetail restaurant={restaurant} />
      <ReviewForm onChange={handleChange} />
    </>
  );
}
```

그렇다면 changeReviewField를 action에 처리하자

```jsx
export function changeReviewField({ name, value }) {
  return {
    type: 'changeReviewField',
    payload: { name, value },
  };
}
```

이제 reducer 테스트로 가서 changeReviewField에 대하여 처리하자

```jsx
describe('changeReviewField', () => {
  it('changes review', () => {
    const initialState = {
      reviewFields: {
        score: '',
        description: '',
      },
    };

    const state = reducer(
      initialState,
      changeReviewField({
        name: 'score',
        value: '5',
      }),
    );

    expect(state.reviewFields.score).toBe('5');
  });
});
```

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
    score: '',
    description: '',
  },
	accessToken: '',
};

changeReviewField(state, { payload: { name, value } }) {
  return {
    ...state,
    reviewFields: {
      ...state.reviewFields,
      [name]: value,
    },
  };
},
```

→ 테스트가 정상적으로 통과되는 것을 볼 수 있다.

그런데 컨테이너의 테스트를 보면 render만 한다고 했는데 change 이벤트까지 확인하고 있다.
조금 더 세분화해서 테스트를 수정해보자.

```jsx
it('renders review write form', () => {
  const { queryByLabelText } = render(<RestaurantContainer restaurantId="1" />);

  expect(queryByLabelText('평점')).not.toBeNull();
  expect(queryByLabelText('리뷰')).not.toBeNull();
});

it('listens score change event', () => {
  const { getByLabelText } = render(<RestaurantContainer restaurantId="1" />);

  fireEvent.change(getByLabelText('평점'), {
    target: { value: '5' },
  });

  expect(dispatch).toBeCalledWith({
    type: 'changeReviewField',
    payload: { name: 'score', value: '5' },
  });
});

it('listens description change event', () => {
  const { getByLabelText } = render(<RestaurantContainer restaurantId="1" />);

  fireEvent.change(getByLabelText('리뷰'), {
    target: { value: '존맛탱🥰' },
  });

  expect(dispatch).toBeCalledWith({
    type: 'changeReviewField',
    payload: { name: 'description', value: '존맛탱🥰' },
  });
});
```

이렇게 각각 랜더되는지, 평점에 change가 되는지, 리뷰에 change가 되는지 테스트를 해줄 수 있다.
그런데 한꺼번에 테스트를 해보는 것이 더 깔끔하고 좋다.
한번 다시 합쳐보자.

```jsx
it('listens change events', () => {
  const { getByLabelText } = render(<RestaurantContainer restaurantId="1" />);

  const controls = [
    { label: '평점', name: 'score', value: '5' },
    { label: '리뷰', name: 'description', value: 'good' },
  ];

  controls.forEach(({ label, name, value }) => {
    const input = getByLabelText(label);

    fireEvent.change(input, { target: { value } });

    expect(dispatch).toBeCalledWith({
      type: 'changeReviewField',
      payload: { name, value },
    });
  });
});
```

그렇다면 이제 ReviewForm을 컴포넌트로 분리시켜주자

```jsx
import React from 'react';

export default function ReviewForm({ onChange }) {
  function handleChange(event) {
    const {
      target: { name, value },
    } = event;
    onChange({ name, value });
  }

  return (
    <>
      <div>
        <label htmlFor="review-score">평점</label>
        <input
          type="number"
          id="review-score"
          name="score"
          onChange={handleChange}
        />
      </div>
      <div>
        <label htmlFor="review-description">리뷰</label>
        <input
          type="text"
          id="review-description"
          name="description"
          onChange={handleChange}
        />
      </div>
    </>
  );
}
```

```jsx
import React from 'react';

import { render, fireEvent } from '@testing-library/react';

import ReviewForm from './ReviewForm';

describe('ReviewForm', () => {
  const handleChange = jest.fn();

  it('renders review write fields', () => {
    const { queryByLabelText } = render(<ReviewForm onChange={handleChange} />);

    expect(queryByLabelText('평점')).not.toBeNull();
    expect(queryByLabelText('리뷰')).not.toBeNull();
  });

  it('listens change events', () => {
    const { getByLabelText } = render(<ReviewForm onChange={handleChange} />);

    const controls = [
      { label: '평점', name: 'score', value: '5' },
      { label: '리뷰', name: 'description', value: 'good' },
    ];

    controls.forEach(({ label, name, value }) => {
      const input = getByLabelText(label);

      fireEvent.change(input, { target: { value } });

      expect(handleChange).toBeCalledWith({ name, value });
    });
  });
});
```

→ 테스트가 정상적으로 통과되는 것을 볼 수 있다.
그런데 현재 form 컴포넌트에 계속해서 반복되어 나타나고 있으니 이것 또한 분리가 가능하다

**TextField.jsx, TextField.test.jsx 생성**

```jsx
import React from 'react';

export default function TextField({ onChange }) {
  function handleChange(event) {
    const {
      target: { name, value },
    } = event;
    onChange({ name, value });
  }

  return (
    <>
      <div>
        <label htmlFor="review-score">평점</label>
        <input
          type="number"
          id="review-score"
          name="score"
          onChange={handleChange}
        />
      </div>
    </>
  );
}
```

```jsx
describe('TextField', () => {
  const handleChange = jest.fn();

  it('renders label and input control', () => {
    const { queryByLabelText } = render((
      <TextField
        label='평점'
        name='score'
        onChange={handleChange}
      />
    ));

    expect(queryByLabelText('평점')).not.toBeNull();
  })
```

→ 먼저 간단하게 테스트를 통과시켰다

그렇다면 TextField에는 label, name을 각각 props 받아서 화면에 출력될 수 있도록 수정하자.

```jsx
import React from 'react';

export default function TextField({ label, type = 'text', name, onChange }) {
  const id = `input-${name}`;
  function handleChange(event) {
    const {
      target: { value },
    } = event;
    onChange({ name, value });
  }

  return (
    <>
      <div>
        <label htmlFor={id}>{label}</label>
        <input type={type} id={id} name={name} onChange={handleChange} />
      </div>
    </>
  );
}
```

→ 기본값으로 type은 text를 줘서 type 값이 없을 경우, text를 줄 수 있도록 설정해주었다.
그리고 결국 name은 name과 동일하기 때문에 target에서도 value만 꺼내와서 사용할 수 있게 하였다.
그렇다면 테스트에서도 각각의 케이스를 나누어 진행해보자.

```jsx
describe('TextField', () => {
  const handleChange = jest.fn();

  context('with type', () => {
    function renderTextField() {
      return render(
        <TextField
          label="평점"
          name="score"
          type="number"
          onChange={handleChange}
        />,
      );
    }

    it('renders label and input control', () => {
      const { queryByLabelText } = renderTextField();

      expect(queryByLabelText('평점')).not.toBeNull();
    });

    it('renders "number" input control', () => {
      const { container } = renderTextField();

      expect(container).toContainHTML('type="number"');
    });
  });

  context('without type', () => {
    function renderTextField() {
      return render(
        <TextField label="리뷰" name="description" onChange={handleChange} />,
      );
    }

    it('renders label and input control', () => {
      const { queryByLabelText } = renderTextField();

      expect(queryByLabelText('리뷰')).not.toBeNull();
    });

    it('renders "text" input control', () => {
      const { container } = renderTextField();

      expect(container).toContainHTML('type="text"');
    });
  });
});
```

→ 테스트가 정상적으로 통과되는 것을 확인할 수 있다.

이제 그렇다면 handleChange 이벤트에 대해서도 테스트를 해보자.

```jsx
it('listens change events', () => {
  const name = 'score';
  const value = '5';

  const { getByLabelText } = render(
    <TextField label="평점" name={name} onChange={handleChange} />,
  );

  fireEvent.change(getByLabelText('평점'), { target: { value } });

  expect(handleChange).toBeCalledWith({ name, value });
});
```

이제 모든 테스트가 통과했으니 ReviewForm을 정리해보자.

```jsx
import React from 'react';

import TextField from './TextField';

export default function ReviewForm({ onChange }) {
  return (
    <>
      <TextField label="평점" name="score" type="number" onChange={onChange} />
      <TextField
        label="리뷰"
        name="description"
        type="text"
        onChange={onChange}
      />
    </>
  );
}
```

→ 뭔진 잘 모르겠는데 그냥 label, name, type, onChange를 넘겨서 이런 내용이 들어갈 것이라고 보여준다

이제 그렇다면 전송 버튼을 만들어보자.

```jsx
import React from 'react';

import { render, fireEvent } from '@testing-library/react';

import ReviewForm from './ReviewForm';

describe('ReviewForm', () => {
  const handleChange = jest.fn();
  const handleSubmit = jest.fn();

  beforeEach(() => {
    handleChange.mockClear();
    handleSubmit.mockClear();
  });

  function renderReviewForm() {
    return render(
      <ReviewForm onChange={handleChange} onSubmit={handleSubmit} />,
    );
  }

  it('renders review write fields', () => {
    const { queryByLabelText } = renderReviewForm();

    expect(queryByLabelText('평점')).not.toBeNull();
    expect(queryByLabelText('리뷰')).not.toBeNull();
  });

  it('listens change events', () => {
    const { getByLabelText } = renderReviewForm();

    const controls = [
      { label: '평점', name: 'score', value: '5' },
      { label: '리뷰', name: 'description', value: 'good' },
    ];

    controls.forEach(({ label, name, value }) => {
      const input = getByLabelText(label);

      fireEvent.change(input, { target: { value } });

      expect(handleChange).toBeCalledWith({ name, value });
    });
  });

  it('renders "send" button', () => {
    const { getByText } = renderReviewForm();

    fireEvent.click(getByText('리뷰 남기기'));

    expect(handleSubmit).toBeCalled();
  });
});
```

리뷰 남기기가 없어 테스트가 깨지는 것을 확인할 수 있다.

그렇다면 만들어주자

```jsx
import React from 'react';

import TextField from './TextField';

export default function ReviewForm({ onChange, onSubmit }) {
  return (
    <>
      <TextField label="평점" name="score" type="number" onChange={onChange} />
      <TextField
        label="리뷰"
        name="description"
        type="text"
        onChange={onChange}
      />
      <button type="button" onClick={onSubmit}>
        리뷰 남기기
      </button>
    </>
  );
}
```

이제 onSubmit에 대해서 RestaurantContainer에 테스트부터 해보자

```jsx
it('renders "리뷰 남기기" button', () => {
  const { getByText } = render(<RestaurantContainer restaurantId="1" />);

  fireEvent.click(getByText('리뷰 남기기'));

  expect(dispatch).toBeCalled();
});
```

현재 RestaurantContainer 컴포넌트에 onSubmit에 대하여 설정을 하지 않았기 때문에 테스트가 깨져야 하는데 왜 통과가 될까?

```jsx
useEffect(() => {
  dispatch(loadRestaurant({ restaurantId }));
}, []);
```

바로 클릭 여부에 상관없이 계속해서 loadRestaurant를 디스패치해오기 때문이다.

calledWith를 사용하기에는 리뷰 남기는 과정을 비동기로 처리하기 때문에 적절치 않아 횟수를 테스트하자.

loadRestaurant 한번, onSubmit 한번 총 두번이 불리게 테스트 조건을 수정하자

```jsx
expect(dispatch).toBeCalledTimes(2);
```

→ 한번밖에 불리지 않다고 테스트가 깨지는 것을 볼 수 있다.

혹은 redux-mock-store 라이브러리를 이용해서 조금 더 정확하게 하는 것도 좋다

그렇다면 이제 다시 수정해보자.

```jsx
function handleSubmit () {
    dispatch(sendReview())
  }

  return (
    <>
      <RestaurantDetail restaurant={restaurant} />
      <ReviewForm
        onChange={handleChange}
        onSubmit={handleSubmit}
      />
    </>
  )
}
```

action에서 sendReview 처리를 해보자.

```jsx
export function sendReview({ restaurantId }) {
  return async (dispatch, getState) => {
    //TODO: dispatch(loadRestaurant) 레스토랑 리뷰 보이기

    const {
      accessToken,
      reviewFields: { score, description },
    } = getState();

    await postReview({ accessToken, restaurantId, score, description });
  };
}
```

```jsx
function handleSubmit() {
  dispatch(sendReview({ restaurantId }));
}
```

→ 어떤 레스토랑에 리뷰를 남길지 레스토랑 아이디를 받아와야 하는데 위와 같이 바로 넘겨주어도 되고, 혹은 getState를 활용해서 restaurant를 받아와서 restaurant.id를 넘겨주어도 된다.

그렇다면 api에서 postReview를 설정해보자.

```jsx
export async function postReview({
  accessToken,
  restaurantId,
  score,
  description,
}) {
  const url = `https://eatgo-customer-api.ahastudio.com/restaurants/${restaurantId}/reviews`;

  const response = await fetch(url, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      Authorization: `Bearer ${accessToken}`,
    },
    body: JSON.stringify({ score, description }),
  });

  await response.json();
}
```

로컬로 가서 제대로 작동하는지 확인해보면 한 가지 오류가 있다.
로그인을 해야만 리뷰를 남길 수 있도록 해야 하는데, 로그인을 하지 않고도 리뷰창이 보여서 리뷰를 남기면 로그인이 되어있지 않은 상태에서 리뷰를 남겨 에러가 뜬다.
그렇다면 로그인 상태를 계속 유지할 수 있도록 해야 한다.
이 부분은 localStorage를 활용해서 설정해보자.

**RestaurantContainer.jsx**

```jsx
const accessToken = useSelector(get('accessToken'));
```

먼저 accessToken을 꺼내오자.

```jsx
return (
  <>
    <RestaurantDetail restaurant={restaurant} />
    {accessToken ? (
      <ReviewForm onChange={handleChange} onSubmit={handleSubmit} />
    ) : null}
  </>
);
```

그 후에 accessToken이 있을 경우에만 ReviewForm이 보이도록 하였다.

이것에 대한 테스트를 함께 진행해보자

```jsx
context('without logged in', () => {
  it("doesn't renders review write fields", () => {
    const { queryByLabelText } = render(
      <RestaurantContainer restaurantId="1" />,
    );

    expect(queryByLabelText('평점')).toBeNull();
    expect(queryByLabelText('리뷰')).toBeNull();
  });
});
```

그렇다면 로그인한 상태에서는 보이도록 해야하는데 어떻게 해야 할까?

given2 라이브러리를 이용하여 accessToken을 세팅해보자

### given2

[given2](https://www.npmjs.com/package/given2)

```jsx
npm install given2
```

**jest.config.js**

```jsx
module.exports = {
  testEnvironment: 'jsdom',
  setupFilesAfterEnv: [
    'given2/setup', // 추가
    'jest-plugin-context/setup',
    './jest.setup',
  ],
};
```

이제 그럼 given을 사용해서 accessToken 세팅을 해주자.

**RestaurantContainer.test.jsx**

```jsx
given('accessToken', () => 'ACCESS_TOKEN');
```

useSelector로 가져오는 기본 값에도 설정해주자.

```jsx
useSelector.mockImplementation((selector) =>
  selector({
    restaurant: {
      id: 1,
      name: '마법사주방',
      address: '서울시 강남구',
    },
    reviewFields: {
      score: '',
      description: '',
    },
    accessToken: given.accessToken,
  }),
);
```

이렇게 해두면 이제 컨테이너는 해결되었으나 의존성이 함께 있는 page에서는 해결되지 않았다.

그러나 페이지는 그저 레스토랑이 랜더링되는지 여부만 확인하면 되는 곳이기 때문에 따로 로그인 여부에 따른 테스트를 해주지 않아도 되니 초기값에만 설정해주자.

```jsx
useSelector.mockImplementation((state) =>
  state({
    restaurant: {
      id: 1,
      name: '마법사주방',
      address: '서울시 강남구',
    },
    reviewFields: {
      score: '',
      description: '',
    },
    accessToken: 'ACCESS_TOKEN',
  }),
);
```

→ 이제 모든 테스트가 정상적으로 통과되는 것을 확인할 수 있다.

그렇다면 로컬에서 확인해보자.

로그인을 하지 않으면 더이상 보이지 않고, 입력하면 201 제대로 입력도 제대로 통과가 되는 것을 확인할 수 있다.

![](https://velog.velcdn.com/images/yxxnhx/post/492bc3d5-4972-4544-b730-a1354f0f87b8/image.png)

그렇다면 내가 입력한 값이 정말 서버에 제대로 들어갔는지 확인해보자

![](https://velog.velcdn.com/images/yxxnhx/post/e95cb21b-fff5-45df-a207-e392a21c93ce/image.png)

→ 테스트로 들어간 내용이 제대로 서버에 들어가있는 것을 확인할 수 있다
