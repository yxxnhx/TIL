# Next.js의 Layout에 대하여 알아보자

Next.js의 가장 큰 특징 중 하나인 레이아웃에 대해 알아보자.
```
import './globals.css';

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang='en'>
      {/*
        <head /> will contain the components returned by the nearest parent
        head.tsx. Find out more at https://beta.nextjs.org/docs/api-reference/file-conventions/head
      */}
      <body>
        {children}
      </body>
    </html>
  );
}
```
NextJS를 사용하여 설치를 하면 기본적으로 제공되는 레이아웃 파일이 있다.
주석으로 된 부분을 읽어보자면 ``<head />에는 가장 가까운 부모가 반환하는 구성 요소가 포함됩니다``라고 한다.
즉, ``children``은 하위 페이지들이 들어가 공통 레이아웃을 손쉽게 적용시킬 수 있는 것이다.
하나하나 import 시킬 필요도 없이 아주 간편해지고 코드도 줄었다.

**그런데 아래와 같이 페이지 별로 공통으로 적용될 레이아웃이 있다면 어떻게 적용시켜야 할까?**
```
📂 pages
	📂 product
    	📝 page.tsx
        📂 [slug]
        	📝 page.tsx
```


### 여기서 Next.js는 13버전 이후로 더 업그레이드를 했다!
방법은 아주 간단하다.
공통으로 넣어줄 페이지 폴더 안에 layout 파일을 만들어주면 된다

```
import Link from 'next/link';
import styles from './layout.module.css';

export default function ProductsLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <>
      <nav className={styles.nav}>
        <Link href='/products/jeans'>Jeans</Link>
        <Link href='/products/shirts'>Shirts</Link>
        <Link href='/products/socks'>Socks</Link>
      </nav>
      <section className={styles.product}>{children}</section>
    </>
  );
}
```

이처럼 동일하게 지정해주면 아주 쉽게 적용을 시킬 수 있다.

리액트로 컴포넌트화를 해도 계속 import를 시키거나 타고 타고 넘어가게 만들어야 했었는데 Nextjs에서는 레이아웃 파일 하나로 아주 손쉽게 적용이 가능하다.
