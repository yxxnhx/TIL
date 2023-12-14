# NextJS 스타일링을 해보자
**그렇다면 간단하게 스타일링을 입혀보자**

```jsx
<nav>
  <Link href="/" style={{ color: router.pathname === '/' ? 'red' : 'blue' }}>
    Home
  </Link>
  <Link
    href="about"
    style={{ color: router.pathname === '/about' ? 'red' : 'blue' }}
  >
    About
  </Link>
</nav>
```

![스크린샷 2023-01-24 오후 11 40 26](https://user-images.githubusercontent.com/50559373/214345941-212900a4-eeb1-4113-a508-6413f3604403.png)

→ 삼항연산자를 이용하여 간단하게 현재 경로일 때는 빨간색 아닐 때는 파란색이 뜰 수 있도록 하였다.

### 그렇다면 CSS Modules을 활용하여 스타일링을 본격적으로 해보자!

NavBar.module.css 생성

```css
.nav {
  display: flex;
  justify-content: space-between;
  background-color: tomato;
}
```

먼저 간단하게 nav 전체에 백그라운드 컬러를 설정하여보자.

그렇다면 이제 NavBar 컴포넌트에서 설정해보자.

```jsx
*import* styles *from* './NavBar.module.css';
```

먼저 css 모듈을 import 한 후 어떻게 설정해야 할까?

```jsx
<nav className="nav">
  <Link href="/">Home</Link>
  <Link href="about">About</Link>
</nav>
```

위처럼 평소 스타일링하던 것처럼 하면 과연 먹힐까?
정답은 아니다. 모듈을 사용하기 위해서는 string으로 불러오는 것이 아닌 모듈이기 때문에 오브젝트에서 프로퍼티 형식으로 적어주어야 한다.

```jsx
<nav className={styles.nav}>
  <Link href="/">Home</Link>
  <Link href="about">About</Link>
</nav>
```

→ 이제 정상적으로 배경색이 들어간 것을 확인할 수 있다.

그렇다면 이제 이전에 인라인 형식으로 넣었던 스타일링을 모듈로 한번 작성해보자.

```css
.active {
  color: tomato;
}
```

먼저 눌렸을 때의 색깔이 변하게 스타일링을 설정 후에 어떻게 하면 될까?

```jsx
<nav>
  <Link href="/" className={router.pathname === '/' ? styles.active : ''}>
    Home
  </Link>
  <Link
    href="about"
    className={router.pathname === '/about' ? styles.active : ''}
  >
    About
  </Link>
</nav>
```

이렇게 간단하게 문제를 해결할 수 있었다.

그렇다면 만약 기본적인 link 스타일링에 active가 붙었을 때만 스타일링이 추가되게 하고 싶다면 어떻게 해야 할까?

방법은 두 가지이다.

- 백틱을 이용하여 더하기

```jsx
<Link
  href="/"
  className={`${styles.link} + ${router.pathname === '/' ? styles.active : ''}`}
>
  Home
</Link>
```

- 배열 형식으로 사용하기

```jsx
<Link
  href="about"
  className={[
    styles.link,
    router.pathname === '/about' ? styles.active : '',
  ].join(' ')}
>
  About
</Link>
```

→ 한 클래스 이름과 다른 클래스 이름의 배열을 만들고 공백을 간격으로 한 문자열로서 합쳐주는 것
join은 한 배열을 다른 한 문자열로 바꿔주는 방법이다.

이렇게 주로 사용하는 방법 두 가지를 확인하며 장단점을 알 수 있다.

module의 장점
- 클래스 혹은 아이디 이름이 중복되어도 모듈화가 되기 때문에 오버라이딩이 되거나 문제시가 되지 않는다.

module의 단점
- 파일을 하나씩 새로 만들어야 한다.
- 클래스 이름을 모두 기억하고 있어야만 한다.
- 클래스 이름 자체를 구현하는 것이 조건이 붙었을 때 복잡해진다.

**그렇다면 모듈을 이용하지 않고 스타일링할 수 있는 방법이 없을까?**

### Built-In CSS Support (내장 CSS 지원)

Next.js를 사용하면 JavaScript 파일에서 CSS 파일을 가져올 수 있다.
이것은 Next.js가 import 개념을 JavaScript 이상으로 확장하기 때문에 가능하다

- CSS-in-JS

격리된 범위 CSS에 대한 지원을 제공하기 위해 styled-jsx를 번들로 제공한다.
목표는 서버 렌더링을 지원하지 않고 JS 전용인 Web Components와 유사한 "Shadow CSS"를 지원한다

[Basic Features: Built-in CSS Support | Next.js](https://nextjs.org/docs/basic-features/built-in-css-support#css-in-js)

### styled-jsx

styled-jsx를 사용하는 컴포넌트를 확인해보자

```jsx
<style jsx>
  {`
		CSS 스타일..
	`}
</style>
```

**styled-jsx**

[https://github.com/vercel/styled-jsx](https://github.com/vercel/styled-jsx)

한번 적용시켜보자.

```jsx
<nav>
  <Link href="/" legacyBehavior>
    <a className={router.pathname === '/' ? 'active' : ''}>Home</a>
  </Link>
  <Link href="about" legacyBehavior>
    <a className={router.pathname === '/about' ? 'active' : ''}>About</a>
  </Link>
  <style jsx>{`
    nav {
      background-color: tomato;
    }
    a {
      text-decoration: none;
    }
    .active {
      color: white;
    }
  `}</style>
</nav>
```

→ Link 자체에는 클래스 네임을 주거나 스타일링을 줄 수 없으니 legacyBehavior를 추가하여 a태그를 넣어도 문제시 되지 않을 수 있도록 설정해주었다.

**legacyBehavior**

기본값은 false인데 link의 자식 요소로 a가 들어가도 true가 될 수 있도록 변경해주는 것이다.

[next/link | Next.js](https://nextjs.org/docs/api-reference/next/link)

정상적으로 스타일링이 들어가는 것을 확인할 수 있다.

**그런데 만약 동일한 클래스 네임으로 다른 컴포넌트에서 사용한다면 어떻게 될까?**

정답은 styled jsx를 사용하기 때문에 사용한 이 스코프, 이 컴포넌트 내부로만 범위가 한정되기 때문에 오버라이딩 되지 않아 문제시 되지 않는다.
즉, 다른 컴포넌트에서 NavBar의 스타일링을 바꾸려고 해도 바뀌지 않는다.

- Adding Component-Level CSS

Next.js는[name].module.css 파일 명명 규칙을 사용하여 CSS Module을 지원한다.

- Sass Support

Next.js를 사용하면 .scss 및.sass 확장자를 모두 사용하여 Sass를 가져올 수 있다.

### 그렇다면 이제 글로벌 스타일에 대해서 알아보자

그 전에 nextjs의 구조부터 제대로 파악해야 할 수 있다.
먼저 index와 about에 각각 NavBar 컴포넌트가 import 되어 있다.
그리고 NavBar 안에 Link와 a가 있고 그 각각의 태그들에게 style jsx로 스타일링이 되어있다.

```jsx
import NavBar from '@/components/NavBar';

export default function Home() {
  return (
    <div>
      <NavBar />
      <h1 className="a">Hello</h1>
      <style jsx>{`
        .a {
          color: yellow;
        }
      `}</style>
    </div>
  );
}
```

그런데 NavBar를 import 한 Home에서 a는 노란색으로 설정하였을 때 먹혔을까?
정답은 먹히지 않고 NavBar에서 설정한 active 하얀색만 스타일링된 것을 확인할 수 있다.
그렇다면 전역적으로 Global style을 만들고 싶다면 어떻게 해야할까?

props로 global을 넘기는 것이다.

```jsx
import NavBar from '@/components/NavBar';

export default function Home() {
  return (
    <div>
      <NavBar />
      <h1 className="a">Hello</h1>
      <style jsx global>{`
        a {
          color: yellow;
        }
      `}</style>
    </div>
  );
}
```

그러나 여기서 문제가 하나 있다.

Home에서는 노란색으로 나오지만 about 에서는 원래대로 흰색으로 나오는 것을 확인할 수 있다.
그렇다면 About 컴포넌트에서 각각 global을 설정한 style을 넣어주면 되겠지만 매 페이지마다 해주어야하는 불편함이 있다.

**이 문제를 어떻게 해결할 수 있을까?**

### App Component

Next.js는 App 컴포넌트를 사용하여 page를 초기화한다.

이를 재정의하고 페이지 초기화를 제어할 수 있습니다. 이를 통해 아래의 일들을 해결할 수 있다!

1. 페이지 변경 간에 레이아웃 유지
2. 페이지 탐색 시 state 유지
3. componentDidCatch를 사용한 Custom 에러 처리
4. 페이지에 추가 데이터 삽입
5. Global CSS 추가

기본 App을 재정의하려면 아래와 같이 ./pages/\_app.js 파일 생성

```jsx
export default function MyApp({ Component, pageProps }) {
  return <Component {...pageProps} />;
}
```

[Advanced Features: Custom `App` | Next.js](https://nextjs.org/docs/advanced-features/custom-app)

NextJS는 화면을 랜더링할 때 \_app.js를 먼저 확인 후에 index.js의 내용을 랜더링한다.

즉, \_app.js는 하나의 blueprint, 청사진이다.
여기서 청사진이란 무언가의 설꼐나 뼈대가 되는 것을 의미한다.
어떻게 페이지가 있어야하는지, 어떤 컴포넌트가 어떤 페이지에 있어야만 하는지를 알려주는 것이다.
\_app.js에서는 component와 pageProps를 넘겨 받아서 화면에 출력하게 해준다.
즉 pageProps는 정확히 어떤 것들이 있는지는 알지 못한 채 props로 넘기는 것이다.
예를 들어 about 컴포넌트의 예시를 들자면 Component에는 About.js, pageProps는 그 안에 적힌 내용들이 넘어 가는 것이다.

다음을 참고하여 확인하여보자.

```jsx
export default function MyApp({ Component, pageProps }) {
  return (
    <div>
      <Component {...pageProps} />
      <span>hello</span>
    </div>
  );
}
```

→ 먼저 페이지를 넘기고 그 다음 hello가 출력되게 해보자.
로컬에서 확인해보면 기존의 home과 about이 모두 정상적으로 출력되고 그 밑에 hello가 랜더링된 것을 확인할 수 있다.

**그렇다면 \_app.js를 활용하여 계속해서 반복되게 넣었던 NavBar와 글로벌 스타일 이슈를 해결해보자!**

```jsx
import NavBar from '@/components/NavBar';

export default function MyApp({ Component, pageProps }) {
  return (
    <>
      <NavBar />
      <Component {...pageProps} />
      <style jsx global>{`
        a {
          color: yellow;
        }
      `}</style>
    </>
  );
}
```

→ 정상적으로 화면이 출력되는 것을 확인할 수 있다.
그런데 그럼 각각의 컴포넌트에서는 css를 import 할 수 없을까?
정답은 없다이다.

NextJS에서 css 파일을 import하고 싶다면 모듈만 가능하다.
그러나 \_app.js에서는 가능하다!

### Custom App (with 타입스크립트)

\_app.ts가 아닌 \_app.tsx파일을 만들고 아래와 같이 작성

```jsx
import type { AppProps } from 'next/app';

export default function MyApp({ Component, pageProps }: AppProps) {
  return <Component {...pageProps} />;
}
```

[Basic Features: TypeScript | Next.js](https://nextjs.org/docs/basic-features/typescript#custom-app)

- 파일명.module.css 파일 형태를 제외한 모든 나머지 css파일들은 \_app.js에서만 import해와서 사용해야 한다. (글로벌 css간의 충돌을 피하기 위해서이다.)

[css-global | Next.js](https://nextjs.org/docs/messages/css-global)
