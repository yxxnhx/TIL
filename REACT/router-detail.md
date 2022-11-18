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
