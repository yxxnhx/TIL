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
