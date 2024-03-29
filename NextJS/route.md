# Nextjs의 가장 큰 특징 중 하나인 라우팅에 대하여 알아보자.

버전에 따라서 라우팅의 방식이 달라졌으니 먼저 12버전 이전까지의 방식을 알아보자.
### 12ver
```
📂 src
  ㄴ 📂 pages
      📄 about.tsx
      📄 contact.tsx
      📄 myPage.tsx
      📂 prodruct
          ㄴ 📄 best.tsx
          ㄴ 📄 new.tsx
          📂 [id]
              ㄴ 📄 index.tsx
```
12버전에서는 위와 같이 라우팅을 따로 설정하지 않아도 ``pages`` 폴더 안에 지정한 파일명이 곧 라우팅될 경로이다.
리액트였다면 react-router를 이용하여 일일히 경로를 지정해주어야 했었다.

매우 편리하지만 하나의 불편한 점이 있었다.
바로 공통으로 사용되는 부분을 재사용하는 것이다.
예를 들면 전체 페이지에서 사용되는 사이드 박스라거나 product 페이지에서 사용될 요소들이 매번 사용되어 공통된다면 매번 새롭게 그려야하는 불편함이 있었다.

이 부분을 보완하기 위해 나온 13버전을 확인해보자.

### 13ver
```
📂 src
	ㄴ 📂 app
    	ㄴ 📂 about
        	ㄴ 📄 page.tsx
        ㄴ 📂 contact
        	ㄴ 📄 page.tsx
        ㄴ 📂 product
        	ㄴ 📄 page.tsx
```
13버전은 위와 같이 app 폴더 안에 라우팅 될 경로 이름으로 폴더를 생성하고 그 안에서 페이지를 구성하면 따로 설정하지 않아도 되는 편리함과 함께 공통된 컴포넌트들을 재사용할 수 있다는 점을 보완하였다.
