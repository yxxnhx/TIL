# CRA 없이 웹팩으로 react와 typescript를 설정해보자!

### 1. React 설정
```
npm init -y
npm i react react-dom
```
**index.html**
📂 public
```
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <div id="root"></div>
  </body>
</html>
```
**index.tsx**
📂 src
```
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

ReactDOM.render(<App />, document.getElementById('root'));
```
**App.tsx**
📂 src
```
const App = () => {
  return (
    <div id='app'>
      <h1>hi</h1>
    </div>
  );
};
export default App;
```
리액트를 사용할 때 기본적으로 필요한 ``index.html``, ``index.tsx``, ``App.tsx``를 생성 및 설정한다.

### 2. Typescript 설정
```
npm i -D typescript @types/react @types/react-dom
npm tsc --init
```

**tsconfig.json**
```
{
  "compilerOptions": {
    "target": "es5", 
    ➡️ 타입스크립트를 어떤 버전의 자바스크립트로 컴파일할지 지정
    	(es5 / es6)
    "module": "commonjs", 
    ➡️ 자바스크립트 파일 간의 import 문법 결정
    	(commonjs: require / es2015, esnext: import)
    "lib": ["dom", "ES2015", "ES2016", "ES2017", "ES2018", "ES2019", "ES2020"],
    ➡️ 타입스크립트 파일을 자바스크립트로 컴파일 할 때 포함될 라이브러리 목록
    "allowJs": true,
    ➡️ 타입스크립트 컴파일 작업을 진행할 때 자바스크립트 파일도 포함될 수 있는지를 설정
    	(true / false)
       js 파일을 ts에서 import하여 쓸 수 있게 함
       자바스크립트 프로젝트에서 타입스크립트를 적용할 때 사용하면 좋다!
    "jsx": "preserve",
    ➡️ jsx가 자바스크립트 파일에서 내보내지는 방식을 제어하는 옵션
    	- preserve: jsx를 변경하지 않고 jsx 파일로 내보냄
        - react: React.creatElement 호출로 변경된 jsx로 js 파일을 내보냄
        - react-native: preserve와 같이 jsx를 변경하지는 않지만 js파일을 내보냄
    "sourceMap": true,
    ➡️ 빌드 된 결과물이 모두 .map으로 생성
    	(true / false)
💡 프로젝트를 배포할 때 코드가 압축, 컴파일, 최소화의 과정을 거치게 되는데 그 결과 사람이 읽기 힘든 언어로 컴파일된다. 때문에 배포 중 에러가 나면 버그를 찾아내기 어려워지는데 sourceMap 설정을 하면 컴파일된 코드를 번역된 map 파일을 이용하여 배포 단계의 버그를 조금 더 쉽게 찾아낼 수 있다.
    "outDir": "./dist",
    ➡️ 컴파일 후 생성되는 자바스크립트 파일을 저장될 폴더명 지정
    "isolatedModules": true,
    ➡️ 각 파일을 분리된 모듈로 트랜스파일링 될지 여부 (true / false)
    파일에서 import 또는 export를 사용하면 그 파일은 모듈이 됨
    "strict": true,
    ➡️ 모든 엄격한 타입 확인 옵션 활성화 여부 (true / false)
    "moduleResolution": "node",
    ➡️ 모듈의 해석 방법 결정
    - node: Node.js
    - classic: Typescript
    "baseUrl": "./",
    ➡️ 모듈 이름을 처리할 기준 디렉토리 지정
    "paths": {
      "@components/*": ["components/*"],
      "@pages/*": ["pages/*"]
    },
    ➡️ baseUrl을 기준으로 모듈 위치 재지정
    "allowSyntheticDefaultImports": true,
    ➡️ default export가 아닌 모듈에서도 default import가 가능하게 할지 여부 결정
    (true / false)
    "esModuleInterop": true,
    ➡️ 모든 imports에 대한 namespace 생성을 통해 CommonJS와 ES Modules 간의 상호작용의 가능 여부를 결정
    (true / false)
    "forceConsistentCasingInFileNames": true
    ➡️ 일관되지 않는 참조의 허용 유무 (true / false)
  }
}
```

### Babel 설정
```
npm i -D babel-loader @babel/core @babel/preset-env
npm i -D @babel/preset-react @babel/preset-typescript
```
- babel-loader, @babel/core, @babel/preset-env
es6 이상의 문법을 이용할 경우 크롬 외 구형 브라우저의 경우 지원되지 않는 경우가 있다
그러므로 구형 브라우저에서도 지원할 수 있도록 자바스크립트를 컴파일한다


- babel/preset-typescript
원래 바벨과 타입스크립트는 따로 작업이 되었지만,``babel/preset-typescript``으로 타입스크립트와 바벨이 조화롭게 병합하여 사용하게 된다.
TypeScript를 사용한다면 필요한 플러그인

- babel/preset-react
jsx로 작성된 코드들을 createElement 함수를 이용한 코드로 변환해 줌
React를 사용한다면 필요한 플러그인

**babel.confing.js**
```
module.exports = {
  presets: [
    "@babel/preset-react",
    "@babel/preset-env",
    "@babel/preset-typescript",
  ],
};
```

### 3. Webpack 설정
```
npm i -D webpack webpack-cli webpack-dev-server webpack-merge
npm i -D clean-webpack-plugin
npm i -D html-webpack-plugin ts-loader
```
- webpack
웹팩 패키지

- webpack-cli
터미널에서 webpack 커맨드를 실행할 수 있도록 도와주는 도구

- webpack-dev-server
실시간 reload를 지원하는 서버
💡 디스크에 저장되지 않는 메모리 컴파일을 이용하기 때문에 컴파일 속도가 빠름

- webpack-merge
webpack을 ``dev``, ``prod`` 모드로 분리하여 구축할 수 있도록 도와줌

- clean-webpack-plugin
성공적으로 다시 빌드 한 후 webpack의 output.path 디렉토리에있는 모든 파일과 사용하지 않는 모든 웹팩 자산을 제거

- html-webpack-plugin
``html`` 파일을 템플릿으로 생성

- ts-loader
타입스크립트 코드를 자바스크립트로 변환


**webpack.common.js**
```
const HtmlWebpackPlugin = require("html-webpack-plugin");
const { CleanWebpackPlugin } = require("clean-webpack-plugin");
const path = require("path");

module.exports = {
  entry: "./src/index.tsx",
  ➡️ 처음 실행되는 기본 시작 파일
  resolve: {
    extensions: [".js", ".jsx", ".ts", ".tsx"],
  },
  ➡️ 확장자나 경로 처리
  module: {
  ➡️ ts-loader, babel-loader 설정
  💡 loader는 오른쪽에서 왼쪽으로 적용되기 때문에 ts-loader보다 babel-loader를 먼저 작성해야한다
    rules: [
      {
        test: /\.tsx?$/,
        use: ["babel-loader", "ts-loader"],
      },
      {
        test: /\.(png|jpe?g|gif)$/,
        use: [
          {
            loader: "file-loader",
          },
        ],
      },
    ],
  },
  output: {
  ➡️ 번들화 된 파일을 export할 경로와 파일명 설정
    path: path.join(__dirname, "/dist"),
    filename: "bundle.js",
  },
  plugins: [
  ➡️ 설치한 플러그인을 적용하는 옵션
    new HtmlWebpackPlugin({
      template: "./public/index.html",
    }),
    new CleanWebpackPlugin(),
  ],
};
```
-> ``prod``, ``dev`` 모드 둘 다 공통적으로 사용됨

**webpack.dev.js**
```
const { merge } = require("webpack-merge");
const common = require("./webpack.common.js");

module.exports = merge(common, {
  mode: "development",
  ➡️ 현재 어떤 모드로 동작할지 지정
  - development: 개발
  - production: 배포
  devtool: "eval",
  ➡️ 어떤 모드로 구조 보여줄 것인지
  - eval: 최대 성능을 갖춘 개발 빌드를 위해 개발 환경에 추천
  - hidden-source-map: 느리지만 외부에서 리액트 구조를 확인할 수 없게 설정되므로 배포 단계에 추천
  devServer: {
    historyApiFallback: true,
    port: 3000,
    hot: true,
  },
});
```

**webpack.prod.js**
```
const { merge } = require("webpack-merge");
const common = require("./webpack.common.js");

module.exports = merge(common, {
  mode: "production",
  devtool: "hidden-source-map",
});
```

### 4. script 설정
**package.json**
```
  "scripts": {
    "dev": "webpack-dev-server --config webpack.dev.js --open --hot",
    "build": "webpack --config webpack.prod.js",
    "start": "webpack --config webpack.dev.js"
  },
```

### 5. webpack 설정하면서 마주친 에러들
- [React is not defined]()
- [exports is not defined]()
- [createRoot(): Target container is not a DOM element]()
