# Next.js에서 Script 추가하기

자바스크립트 파일을 추가를 해주고 싶은데 계속해서 경로를 못찾거나 자바스크립트 로드가 되지 않는 에러가 일어났었다.
`localhost:3000/js경로`로 해줘도 404 에러가 떠서 파일이 로드가 되지 않았다.

### Script 사용하기

```jsx
import Script from "next/script";

export default function Dashboard() {
  return (
    <>
      <Script src="https://example.com/script.js" />
    </>
  );
}
```

스크립트를 불러올 때 사용할 수 있는 `Optional Props`가 있다.

### strategy

strategy를 사용하게 되면 컴포넌트 어디에서 호출하더라도 명시적으로 타이밍을 결정할 수 있다

- `beforeInteractive`: Next.js 코드가 로드되기 전에 불러온다. - head에 위치하며 속성으로 defer를 사용한다.
  서버에서 초기에 Html을 보낼 때 주입되고, 번들된 자바스크립트가 실행되기 전에 먼저 실행한다.
  보통 페이지가 상호작용 되기 전에 반드시 스크립트가 필요로할 때 생성한다.

```jsx
    <Html lang="en">
      <Head>
        <Script
          src=".js경로"
          strategy="beforeInteractive"
          defer
        />
      </Head>
```

- `afterInteractive`(default) : 페이지가 상호작용가능해진 직후에 즉시 로드

```jsx
    <Html lang="en">
      <Head>
        <Script
          src=".js경로"
          strategy="afterInteractive"
        />
      </Head>
```

- `lazyOnload`: 페이지 라이프사이클 로드 이후에 body 태그 마지막에 추가되어진다.

```jsx
<Html lang="en">
  <Head />
  <body>
    <Main />
    <NextScript />
    <Script src="js경로" strategy="lazyOnload" />
  </body>
</Html>
```

- `worker`: web worker에 로드된다

현재 나의 상황은 자바스크립트가 모두 로드된 이후에 그 스크립트를 이용해서 화면을 구성해야 하므로 `beforeInteractive`가 적절하다.

참고: https://nextjs.org/docs/pages/api-reference/components/script#required-props
