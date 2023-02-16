# Styles

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
