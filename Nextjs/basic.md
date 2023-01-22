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

또한 react를 import하지 않고 jsx를 사용할 수 있다. 그러나 react method(useState, userEffect)를 사용하려면 react를 import해줘야한다.

**next.js**의 가장 좋은 점 중에 하나는 모든 페이지들이 **pre-rendering** 된다는 것이다. 이것들은 정적(static)으로 생성된다.

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

![https://blog.kakaocdn.net/dn/cxEMK3/btrwn4SSjAC/1hw7M5EsJKf81BWKJRltr0/img.png](https://blog.kakaocdn.net/dn/cxEMK3/btrwn4SSjAC/1hw7M5EsJKf81BWKJRltr0/img.png)

next.js는 react.js를 백엔드에서 동작시켜서 해당 페이지를 미리 만든다. 이게 component들을 render시킨다. 렌더링이 끝나면 이게 html이 되고, 만들어진 html을 아까 봤던 페이지 소스 안에 넣어준다.

페이지 소스안에 html이 미리 들어가있게 된다면 SEO에 유리하다는 장점을 가진다.
