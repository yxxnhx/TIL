# **Emotion**

## **CSS-in-JS**

- [CSS-in-JS에 관해 알아야 할 모든 것](https://d0gf00t.tistory.com/22)

이전까진 CSS를 `xxx.css` 와 같이 별도의 문서로 관리했지만 요즘은 자바스크립트 코드에서 CSS를 작성

CSS-in-JS라는 명칭은 방법론이다

실제로 작성을 돕는 라이브러리는 매우 많다. 대표적으로 `styled-components`와 `emotion`이 있다.

이번에는 `emotion`을 사용해보자.

- [Emotion 공식 문서](https://emotion.sh/docs/introduction)

**설치**

```jsx
npm i @emotion/react @emotion/styled
```

이제 emotion으로 헤더부터 한번 설정해보자.

기본 형식은 `styled-component`와 동일하다

styled-component는 백틱을 사용해서 string처럼 사용하였다면 emotion에서는 오브젝트로 사용이 가능하다

그 외에는 모든 사용이 동일하다.

### 1. emotion import 해오기

```jsx
*import* styled *from* '@emotion/styled';
```

### 2. style 지정하기

```jsx
return (
  <Container>
    <Header>
      <h1>
        <Link to="/">EatGo</Link>
      </h1>
    </Header>
    <Routes>
      <Route exact path="/" element={<HomePage />} />
      <Route path="/about" element={<AboutPage />} />
      <Route path="/login" element={<LoginPage />} />
      <Route path="/restaurants" element={<RestaurantsPage />} />
      <Route path="/restaurants/:id" element={<RestaurantPage />} />
      <Route path="*" element={<NotFoundPage />} />
    </Routes>
  </Container>
);
```

```jsx
const Container = styled.div({
  margin: '0 auto',
  width: '90%',
});

const Header = styled.header({
  backgroundColor: '#eee',
  '& h1': {
    margin: 0,
    padding: '1rem .5em',
    fontSize: '1.5em',
  },
  '& a': {
    color: '#555',
    textDecoration: 'none',
    '&:hover': {
      color: '#712',
    },
  },
});
```

→ 태그를 컴포넌트처럼 만들어서 사용하면 된다.

그 안에서 중첩되어 사용되는 것은 Sass와 동일하다.

동일하게 계속해서 반복되는 것은 컴포넌트로 따로 빼서 정리하여도 된다.

```jsx
import styled from '@emotion/styled';

const MenuList = styled.ul({
  margin: 0,
  padding: '.4em 0',
  listStyle: 'none',
  display: 'flex',
});

export default MenuList;
```

이렇게 설정 후에 그냥 일반 컴포넌트를 import 하듯이 가져와서 태그명만 컴포넌트처럼 변경해주면 된다.

그렇다면 눌렸을 때 스타일이 바뀌게 하려면 어떻게 해야 할까?

여기에서는 함수처럼 사용이 가능하다.

아래의 예시를 한번 보자.

```jsx
return (
  <MenuList>
    {
      regions.map((region) => (
        <MenuItem
          key={region.id}
        >
          <button
            type='button'
            onClick={() => handleClick(region.id)}
          >
            {region.name}
            {
              selectedRegion
              ?  (region.id === selectedRegion.id ? '(V)' : null)
              : null
            }
          </button>
        </MenuItem>
      ))
    }
  </MenuList>
)
}
```

selectedRegion이 있을 경우에만 체크표시가 나올 수 있게 하였다.

그렇다면 이 상태를 MenuItem에 active라는 상태로 조건을 추가를 해줘보자

```
<MenuItem
  key={region.id}
  active={selectedRegion && region.id === selectedRegion.id}
>
```

그 후 MenuItem 스타일 컴포넌트로 가보자.

```jsx
import styled from '@emotion/styled';

const MenuItem = styled.li(({ active }) => ({
  marginRight: '1rem',

  '& button': {
    padding: '.4em 1em',
    border: '1px solid #ccc',
    backgroundColor: active ? '#eee' : 'transparent',
    textDecoration: 'none',
    color: '#333',
    cursor: 'pointer',

    '&:hover': {
      fontWeight: 'bold',
      color: '#712',
    },
  },
}));

export default MenuItem;
```

props로 active를 받아 삼항 연산자로 active 일 경우에만 색이 보이고 아니라면 투명으로 설정되게 할 수도 있는 것이다.
