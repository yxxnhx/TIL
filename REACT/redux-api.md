# Redux HTTPie를 활용하여 통신해보자

사용자의 로그인을 하고 로그인을 기반해서 기능들을 추가해주자
현재 사용하고 있는 서버는 로그인 서버가 따로 있어 로그인을 확인할 수 있다.
간단히 HTTPpie라는 프로그램을 이용해서 먼저 테스트를 해보고 옮기는 작업을 해보자.

## **HTTPie**

[HTTPie - API testing client that flows with you](https://httpie.io/)

HTTP 클라이언트 CLI 도구이다. 간단한 데이터 조회 및 전송을 테스트해볼 수 있다.
우리가 자주 사용하는 `curl`로도 테스트할 수 있지만 사용하기 불편한 점이 있고, 또 Postman을 사용할 수도 있지만 간단하게 테스트할 때는 HTTPie로 해보는 것도 좋다.

#### HTTPie 설치

```jsx
brew install httpie
```

먼저 사용하기 전, REST API에서 사용하는 네 가지 메서드를 짚고 넘어가자.

- GET
- POST
- PUT/PATCH
- DELETE

그 중에서 우리는 로그인을 위해서 session이라는 리소스를 POST 만들 것(Create)이다.
→ CRUD로 대응하니까!

그렇다면 터미널창에 다음과 같이 입력해보자

```jsx
http POST eatgo-login-api.ahastudio.com
```

![](https://velog.velcdn.com/images/yxxnhx/post/5c36e601-7110-4db1-8169-3b81e507d813/image.png)

위와 같이 입력을 하게 되면 제대로 이메일과 패스워드를 입력해주지 않아 접근이 되지 않는 것을 볼 수 있다.
그렇다면 이제 테스트 아이디와 패스워드를 입력하여 제대로 접근해보자.

```jsx
http POST https://eatgo-login-api.ahastudio.com email=tester@example.com password=test
```

![](https://velog.velcdn.com/images/yxxnhx/post/d3382745-0a4d-4d5b-a70e-349eb26b46b5/image.png)

→ 404 Not Found가 뜨는 것을 볼 수 있다.

**왜일까?**

앞서 말했듯, 로그인을 위해서 session이라는 리소스를 활용해서 접근할 것인데 session을 넣지 않고 그냥 주소만 입력해서 일어난 에러이다.
그렇다면 다시 `https://eatgo-login-api.ahastudio.com/session`으로 입력해보자

```jsx
http POST https://eatgo-login-api.ahastudio.com/session
email=tester@example.com password=test
```

![](https://velog.velcdn.com/images/yxxnhx/post/f7de8c4a-8efd-49d3-b550-51e721dcfc0e/image.png)

→ 정상적으로 접근이 되어 acessTocken이 출력된 것을 확인할 수 있다.
즉, session이라는 리소스를 POST하는데, email과 password를 넣었을 경우 create가 성공한 201이 나오게 된다.

만약, 잘못된 이메일 혹은 패스워드로 접근할 때는 어떻게 나올까?

```jsx
http POST https://eatgo-login-api.ahastudio.com/session
email=test@example.com password=test
```

![](https://velog.velcdn.com/images/yxxnhx/post/1a7739f1-e1ea-40bd-8652-22ff7c02fe63/image.png)

→ 아무것도 나오지 않고, 요청에 대해서도 400 에러가 나는 것을 확인할 수 있다.

**여기에서 코드에 대해서 알아보자**

- 200 - 성공
- 300 - 리다이랙션
- 400 - 클라이언트가 잘못됨
- 500 - 서버가 내부에서 문제가 생겼음

그러므로 현재, 클라이언트가 입력을 잘못했으므로 400 에러가 나는 것이다.

#### 이제 그렇다면 올바르게 입력해서 나오는 토큰을 활용하여 요청을 해보자.

예를 들어서 리뷰를 작성한다고 가정해보자.
누가 리뷰를 작성했는가? 내가 했다는 것을 알려야 한다.

**그렇다면 입력해보자.**

```jsx
http POST [https://eatgo-customer-api.ahastudio.com](https://eatgo-customer-api.ahastudio.com/)/restaurants/1/reviews
```

![](https://velog.velcdn.com/images/yxxnhx/post/4227dd08-9d0d-4c2b-a9de-600a2159e8ea/image.png)

→ body가 없다는 에러가 뜨는 것을 볼 수 있다.

그렇다면 리뷰 항목에 맞게 body를 입력해주자.

```jsx
http POST [https://eatgo-customer-api.ahastudio.com/restaurants/1/reviews](https://eatgo-customer-api.ahastudio.com/restaurants/1/reviews)
score=5 description=good
```

![](https://velog.velcdn.com/images/yxxnhx/post/02173ab8-16eb-4d18-80ff-493299c01a54/image.png)

![](https://velog.velcdn.com/images/yxxnhx/post/37b4f133-67ff-4579-b1ab-7b63c7e03291/image.png)

→ 정상적으로 넘겼으나, 사용자가 없어서 에러가 나는 것을 확인할 수 있다.
이럴 경우에는 아까 받은 accessTocken을 넣어서 문제를 해결할 수 있다.

이 부분은 먼저 UI를 만든 후 다시 확인해보자.
