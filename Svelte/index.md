# Svelte

[Svelte](https://svelte.dev/)

스벨트는 자바스크립트 프레임워크이다.

프레임워크가 없는 프레임 워크 혹은 컴파일러라고 소개한다.

가상돔 virtual dom이 없고 런타임에 로드할 프레임워크가 없음을 의미한다.

기본적으로 빌드 단계에서 구성 요소를 컴파일하는 도구이므로 페이지에 단일 번들(bundle.js)을 로드하여 앱을 랜더링할 수 있다.

### 설치하기

```jsx
npx degit sveltejs/template svelte-app
cd svelte-app
```

### 시작하기

```jsx
cd svelte-app
npm install
npm run dev
```

### package 구조

```jsx
{
  "name": "svelte-app",
  "version": "1.0.0",
  "private": true,
  "type": "module",
  "scripts": {
    "build": "rollup -c",
    "dev": "rollup -c -w",
    "start": "sirv public --no-clear"
  },
  "devDependencies": {
    "@rollup/plugin-commonjs": "^24.0.0",
    "@rollup/plugin-node-resolve": "^15.0.0",
    "@rollup/plugin-terser": "^0.4.0",
    "rollup": "^3.15.0",
    "rollup-plugin-css-only": "^4.3.0",
    "rollup-plugin-livereload": "^2.0.0",
    "rollup-plugin-svelte": "^7.1.2",
    "svelte": "^3.55.0"
  },
  "dependencies": {
    "sirv-cli": "^2.0.0"
  }
}
```

## 폴더 구조

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a33ae4d3-2cc2-4547-a82a-f79ee8d75a05/Untitled.png)

- /public/build : svelte가 수행한 컴파일 결과
- /src
- rollup.config.js : Rollup이라는 자바스크립트용 모듈 번들러(웹팩과 비슷)

### main.js

```jsx
import App from "./App.svelte";

var app = new App({
  target: document.body, // public 폴더 안의 index.html 문서의 body
});

export default app;
```
