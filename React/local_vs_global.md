## 지역변수 vs 전역변수

### 1. state를 어떻게 관리할까?
1. 리액트에 한정짓지 않고 프로그래밍 관점에서 비교해보자.
- Scope
- Lifespan
- Namepsace collision
- var vs let vs const
2. 해당 변수를 어디에서 사용하는가?
그에 따라서 달라지겠지만 2단계 이상으로 내려갈 때에는 전역변수로 빼는 것도 하나의 방법이다.
계속해서 내려주다보면 ``props drilling``이 일어날 수 있기 때문이다.

3. 전역변수 설정하는 기준?
그렇다면 기준을 어떻게 가져야할까?
각 프로젝트와 회사의 컨벤션에 따라 다르겠지만 하위 2단계까지는 설정해주는 것이 좋다. 그 이상 3단계, 4단계 이상으로 넘어가다보면 불필요한 ``props drilling``이 일어날 수 있다.


### 2. 전역 상태 관리의 필요성

1. Container - Presenter 방식
이전에는 상태를 모두 관리하는 ``container``, 그리고 화면에 표출하는 기능만 하는 ``presenter``로 나누는 방식이 인기있었다.
![](https://velog.velcdn.com/images/yxxnhx/post/eec06d90-2d89-4d08-9271-67a2d78a7ba1/image.png)

그러나 그림과 같이 상태값을 계속 하위의 하위, 그 하위의 하위로 내려가다보면 ``props drilling``이 일어나게 된다.

**그렇다면 전역적으로 상태는 관리하면서 전역변수를 사용하지 않는 방법이 무엇이 있을까?**

1. Flux && Redux
![](https://velog.velcdn.com/images/yxxnhx/post/523fc8d7-5cdd-4822-8659-6c33ce336864/image.png)


### 3. 전역변수 툴 비교
#### Context API

1. 특징
- `State` 를 `Provide` 하는 방식
- React 내장 기능이라 별도 라이브러리 설치가 필요하지 않음
- HTTP Request도 Context에서 모두 관리
- `context`라는 단어에 맞게 해당 관심사와 관련된 모든 것을 context가 처리

2. 단점
- 비지니스 로직이 복잡해지는 경우 Provider 다수 생성 필요
- Provider 1개의 경우에도 설정할 것들이 많음

#### Redux

1. 특징
- FLUX 패턴을 적용함

**FLUX 패턴이 왜 나오게 되었을까?**
![](https://velog.velcdn.com/images/yxxnhx/post/c6a9b45c-55a4-414e-bff5-0a7e704833c7/image.png)
기존에는 MVC 패턴으로 `Controller`가 `Model`들을 가지고 `View` 즉 화면을 보여주는 구성이다. 실제로 `Controller`가 `Mode`, `View` 모두 관리해야 하는 상황에서는 위 사진처럼 구조가 매우 복잡해질 수 있다
그래서 페이스북에서 비 MVC 패턴을 보완하여 내놓은 것이 FLUX 패턴이다.
![](https://velog.velcdn.com/images/yxxnhx/post/e2261f3e-8d3b-42b0-a3fc-6f9e4ca77d25/image.png)


1. 특징
- FLUX 패턴을 적용함
- Store 하나에서 변수를 관리하기 때문에 여러 Provider를 선언하지 않아도 됨
- Thunk, saga 등의 Middleware를 통한 비동기처리
- Redux ToolKit 나오고 설정이 비교적 수월해짐
2. 단점
- 설정할 것이 엄청 많음
- FLUX 패턴을 적용했기 때문에 효율이 떨어질 수도 있음
- 요즘 새로 시작하는 프로젝트에서는 잘 안씀

#### Recoil
1. 특징
- 내가 필요한 값만 `subscribe`하는 느낌
- `useXXXX`와 같이 리액트 개발자들에게 익숙한 문법
- 구조가 간단해서 적용하기 쉬움
- Concurrent mode 지원
- 렌더링 효율이 좋다는 일반적인 의견

2. 단점
- 커뮤니티 서포트가 많지 않음
- atom이 엄청 많아질수도
- 미들웨어 지원 없음
-> HTTP Request 처리 별도로 해야함

#### Zustand 

1. 특징
- `context api`, `redux`, `recoil`, `zustand`중에 설정이 제일 간단함
- 별다른 패턴 없이 자유롭게 적용 가능함
- Shallow comparison을 통한 효율 개선
- 함수형, 클래스형 다 지원함

2. 단점
- Recoil과 마찬가지로 커뮤니티 지원 부족
- 미들웨어 없음

**그렇다면 가장 성능이 좋은 툴은 무엇일까?**
결론부터 말하자면 사실 큰 의미는 없다. 굳이 따지자면 `context api`가 제일 안 좋을 것이다.
상태 변경 시에 동시에 너무 많은 부분들을 리랜더링하기 때문이다. 그러나 이마저도 프로젝트 설정에 따라서 달라질 수 있기 때문에 확정지을 수는 없다.


**그럼 뭘 쓰는게 좋을까?**
포트폴리오를 만드는 나와 같은 신입/취준 개발자라면,
아무거나 하나 잡아서 진득하게 정말 끝까지 파보고 그 하나에 대한 장단점을 몸소 겪은 후에 새로운 것을 하는 것이 좋다. 특히 면접이나 포트폴리오에서 `해당 라이브러리를 선택한 이유`를 설명할 수 있어야 한다.
> "왜 사용했는가"가 중요하다

회사에 일하고 있는 주나어 개발자라면
회사에서 사용하는 툴을 깊게 공부하고 새로 나오거나 사용해보지 못한 라이브러리에 대해 궁금하다면 가볍게 토이프로젝트를 통해 사용해보고 블로그로 회고하는 것을 추천한다.


### 4. 전역변수를 사용하지 않을 수는 없을까?

1. Toss SLASH 23(https://www.youtube.com/watch?v=NwLWX2RNVcw) 참고
- 페이지 routing을 하지 않고 component로 관리
- 관련성 있는 코드의 “응집도"를 높이는 방식

2. 항상 이런 방식을 택할수는 없음
- 개발팀의 컨벤션 중요함
- 컨벤션을 지키는 코드가 가독성에 유리
