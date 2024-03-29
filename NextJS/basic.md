# Next.js

### 1. 설치하기

```jsx
npx create-next-app@latest --ts
# or
yarn create next-app --typescript
# or
pnpm create next-app --ts
```

`create-react-app` 과 처럼 `create-next-app` 은 Next.js 어플리케이션을 빠르게 생성하기 위한 CLI 툴

별다른 플래그 없이 사용하면 Next.js의 기본 템플릿을 사용하게 되지만, `--example` 플래그를 사용하면 **약 300개 안팎의** 스택 별, 유형 별 예제 템플릿들 중 선택하여 세팅할 수 있다.

이외에도 플래그들은 아래와 같이 붙일 수 있다

- `-ts, --typescript` : Next.js 어플리케이션을 **타입스크립트 기반**으로 초기화해준다.
- `e, --example [name]|[github-url]` : Next.js 어플리케이션을 **특정 예제 템플릿 바탕으로 초기화**해준다. 이 때의 예제는 **Next.js 레포지토리의 예제명** 또는 **원하는 깃허브 URL**이 될 수 있다. 단 `/` 기호가 들어가는 경우에는 아래의 `-example-path` 플래그를 **추가로** 사용한다.
- `-example-path [path-to-example]` : 예제 템플릿을 **깃허브 URL**로부터 가져오는 경우, 경로명에 `/` 기호가 사용된다면 이 플래그를 사용해야 한다. 브랜치 명에 `/` 기호가 들어갈 수도 있고, 하위 경로를 명시하느라 `/` 기호를 사용해야 할 수도 있다. 이러한 경우 **반드시 경로를 따로 명시해 주어야 한다**: `-example-path foo/bar`

### 2. 프로젝트 실행

초기화가 완료된 프로젝트는 `npm run dev` 로 웹 상에서 확인해볼 수 있다. 기본적으로 `localhost 3000`번 포트를 사용한다.

```jsx
"scripts": {
  "dev": "next dev",
  "build": "next build",
  "start": "next start",
  "lint": "next lint"
},
```

이제 프로그램이 어떻게 굴러가는지 확인하기 위하여 pages 폴더 안에 있는 모든 폴더들을 삭제해보자.

그 전에 라이브러리와 프레임 워크의 차이점을 알아보자

### 라이브러리와 프레임워크의 주요 차이점

라이브러리와 프레임워크의 주요 차이점은 "Inversion of Control"(통제의 역전)이다

라이브러리에서 메서드를 호출하면 사용자가 제어할 수 있다

그러나 프레임워크에서는 제어가 역전되어 프레임워크가 사용자를 호출

- **라이브러리** : 사용자가 파일 이름이나 구조 등을 정하고, 모든 결정을 내림
- **프레임워크** : 파일 이름이나 구조 등을 정해진 규칙에 따라 만들고 따름

그렇다면 한번 실행해보자.

**pages > index.js 생성**

```jsx
export default function Home() {
  return 'hi';
}
```

아무런 설정도 하지 않았는데 `localhost 3000`에서 hi가 출력되는 것을 확인할 수 있다.

그렇다면 하나 더 해보자.

**pages > about.js 생성**

```jsx
export default function about() {
  return 'about us';
}
```

`localhost 3000/about`으로 들어가면 about us가 출력되어있는 것을 볼 수 있다.

이제 우리는 알 수 있다.

파일에 작성한 파일의 이름이 곧 url 경로가 되는 것이다.

파일에 작성한 컴포넌트의 이름은 파일명과 같을 필요는 없다.

단! `export default`를 해주어야만 웹페이지에서 볼 수 있다

만약 파일명이 about-us라면 /about-us 경로로 가면 about.js라는 페이지를 볼 수 있을 것이다

next.js에서는 404 페이지를 커스텀할 수 있기 때문에 매우 유용하게 쓰일 수 있다.

pages 폴더 안에 있는 파일명에 따라 route가 결정된다.

pages/about.js 생성 시

- localhost:3000/about (O)
- localhost:3000/about-us(X)

다만 예외사항으로, index.js의 경우에는 앱이 시작하는 파일이라고 보면 된다.

즉 localhost:3000 그 자체다 뒤에 /index 로 붙이면 안된다.

이 강의를 들을 때는 import react from "react"를 쓸 필요가 없다.

다만 useState,useEffect, lifecycle method 같은 애들을 써야 할 경우에는 꼭 import를 해줘야 한다.

또한 react를 import하지 않고 jsx를 사용할 수 있다. 그러나 react method(useState, userEffect)를 사용하려면 react를 import해줘야한다.

**next.js**의 가장 좋은 점 중에 하나는 모든 페이지들이 **pre-rendering, 즉 미리 랜더링**된다는 것이다. 이것들은 정적(static)으로 생성된다.

create react app을 사용하면 CSR(Client Side Render)를 하는 애플리케이션을 만들게 된다.

**CSR이란** 브라우저가 유저가 보는 UI를 만드는 모든 것을 하는 것을 의미한다.

모든 버튼과 검색창 등의 UI들이 js로 렌더링 되어있고, 사용자가 보는 html 안에 포함되어 있지 않다. CRA를 사용한 웹페이지의 소스코드를 보면 하나의 div만 존재하고 모두 js로 이루어져 있다.

브라우저가 자바스크립트를 가져와서 Client-side 자바스크립트가 모든 UI를 만드는 것이 CSR이다.

브라우저가 HTML을 가져올 때 비어있는 div만을 가져오고, 그 후에 모든 자바스크립트를 요청해서 자바스크립트와 react.js를 실행시키고 나서야 유저는 웹페이지를 볼 수 있다. js를 가져와야만 화면에 모든 UI를 띄울 수 있기 때문에 만약 네트워크 연결이 느리다면, 처음에 흰 화면만 보이고 천천히 화면이 로딩이 되는 것을 볼 수있다.

→ _client의 브라우저가 모든 일을 하기 때문에 초기 로딩 속도가 느리다. 또한 웹에서 js를 비활성화 하면 화면이 작동하지 않는다._

그러나 next에서는 js 비활성화를 해도 똑같이 잘 작동하는 것을 볼 수 있다. 그 이유는 nextjs를 사용하는 웹페이지의 소스코드를 보면 실제로 사용하는 html이 있는 것을 볼 수 있다.

그렇기 때문에 js를 사용하지 않거나 네트워크가 느리더도 사용자에게 html은 보여질 수 있다. 미리 렌더링이 되는 것이다.

index.js

```jsx
import { useState } from 'react';

export default function Home() {
  const [counter, setCounter] = useState(0);
  return (
    <div>
      <h1>Hello {counter}</h1>
      <button onClick={() => setCounter((prev) => prev + 1)}>+</button>
    </div>
  );
}
```

다음과 같이 작성하고 http://localhost:3000/ 로 접속한다.

오른쪽 버튼 클릭해서 페이지소스보기를 누르면

![https://blog.kakaocdn.net/dn/bzyXlX/btrwA5qBbam/0s5b0AQpoHtpSeXIlY0YW1/img.png](https://blog.kakaocdn.net/dn/bzyXlX/btrwA5qBbam/0s5b0AQpoHtpSeXIlY0YW1/img.png)

다음과 같은 코드가 보이고 자세히 살펴보면 위에서 작성했던 코드인

![https://blog.kakaocdn.net/dn/eBgmcx/btrwJv9AJkL/msT40f8TdhcEK2dfavAtS1/img.png](https://blog.kakaocdn.net/dn/eBgmcx/btrwJv9AJkL/msT40f8TdhcEK2dfavAtS1/img.png)

을 발견할 수 있다.

페이지를 딱 열면 보는 것이 위에서 봤던 html코드이다. 그리고 나서 react.js가 클라이언트로 전송됐을 때, react.js앱이 된다. 이렇게 react.js를 프론트엔드안에서 실행하는 걸 **hydration**이라고 부른다.

```jsx
import { useState } from 'react';

export default function Home() {
  const [counter, setCounter] = useState(0);
  return (
    <div>
      <h1>Hello {counter}</h1>
      <button onClick={() => setCounter((prev) => prev + 1)}>+</button>
    </div>
  );
}
```

next.js는 react.js를 백엔드에서 동작시켜서 해당 페이지를 미리 만든다. 이게 component들을 render시킨다. 렌더링이 끝나면 이게 html이 되고, 만들어진 html을 아까 봤던 페이지 소스 안에 넣어준다.

페이지 소스안에 html이 미리 들어가있게 된다면 SEO에 유리하다는 장점을 가진다.

### 그렇다면 이제 네비게이션을 만들면서 routing을 해보자.

components > NavBar.js 생성

```jsx
export default function NavBar() {
  return (
    <nav>
      <a href="/">Home</a>
      <a href="/about">About</a>
    </nav>
  );
}
```

이 NavBar 컴포넌트를 각각 index와 about에 import를 시키면 정상적으로 네비게이션 바가 출력이 되어 이동이 되는 것을 확인할 수 있다.
그런데 다시 NavBar로 돌아가서 보면 다음과 같은 에러가 떠 있는 것을 확인할 수 있다.

![스크린샷 2023-01-24 오후 11 19 31](https://user-images.githubusercontent.com/50559373/214345524-07007511-e097-4303-b9f4-a0d48fff6b40.png)

→ nextjs에서 a로 navigate를 하지 말라는 에러가 뜨는 것을 확인할 수 있다.
그 대신 Link를 사용하라는 추천도 함께 있다.

이유는 React와 동일하다.
리액트에서 라우팅할 때 a로 할 경우 계속해서 새로고침이 되면서 속도가 느려지고
페이지 간 클라이언트 측 경로 전환을 활성화하고 single-page app 경험을 제공하려면 Link컴포넌트가 필요하다.

[no-html-link-for-pages | Next.js](https://nextjs.org/docs/messages/no-html-link-for-pages)

그렇다면 Link를 활용하여 다시 설정해보자.

```jsx
import Link from 'next/link';

export default function NavBar() {
  return (
    <nav>
      <Link href="/">Home</Link>
      <Link href="about">About</Link>
    </nav>
  );
}
```

→ 더이상 에러가 뜨지 않는 것을 확인할 수 있다.
또한 네트워크에서 확인해보면 더이상 새로고침이 되지 않는 것 또한 확인할 수 있다.
그렇다면 한번 현재의 경로를 확인할 수 있는 useRouter를 활용하여 확인해보자.

```jsx
const router = useRouter();
console.log(router);
```

![스크린샷 2023-01-24 오후 11 25 25](https://user-images.githubusercontent.com/50559373/214345876-5c29c316-76a6-489e-bb91-30b4d6c5c03c.png)

→ 현재의 경로와 라우팅 정보를 가져와 어디인지 확인할 수 있다.
앱의 함수 컴포넌트에서 router객체 내부에 접근하려면 userRouter()훅을 사용할 수 있다.
useRouter는 React Hook이다.
즉, 클래스와 함께 사용할 수 없다. withRouter를 사용하거나 클래스를 함수 컴포넌트로 래핑할 수 있다.

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
