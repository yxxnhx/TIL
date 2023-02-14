# Discriminated

먼저 Discriminate의 뜻에 대해서 알아보자

<aside>
💡 Discriminate : 구별의, 차별의

</aside>

```tsx
type SuccessState = {
  result: "success";
  response: {
    body: string;
  };
};

type FailState = {
  result: "fail";
  reason: string;
};
```

타입별로 동일한 result라는 키를 가지고 있지만 어떠한 state냐에 따라서 다른 값이 지정되어있다.

```tsx
function login(): LoginState {
  return {
    result: "success",
    response: {
      body: "logged in",
    },
  };
}
```

result를 타입에 맞게 넣게 되면 자동으로 suceess를 추천하는 것을 확인할 수 있다.

만약 실패의 경우를 반환하게 된다면 response 부분에 에러가 뜨는 것 또한 볼 수 있다. 이 부분은 reason으로 변경하면 에러가 사라지는 것을 확인할 수 있다.

이처럼 타입이 보장되면서 서로 다른 state를 간편하게 만들 수 있다.

**그렇다면 로그인 상태를 프린트해주는 함수를 만들어보자.**

```tsx
function printLoginState(state: LoginState) {
  if ("response" in state) {
    console.log(state.response.body);
  } else {
    console.log(state.reason);
  }
}
```

이처럼 간단하게 resonse in state를 활용해서 풀어낼 수도 있지만 뭔가 코드가 더럽다는 것을 확인할 수 있다.

그렇다면 discriminated type을 이용하여 변경해보자.

```tsx
function printLoginState(state: LoginState) {
  if (state.result === "success") {
    console.log(state.response.body);
  } else {
    console.log(state.reason);
  }
}
```

discriminated type을 활용하면 현재 printLoginState의 state는 LoginState 타입을 바라보고 있지만 성공일 수도 있고 실패일 수도 있다. 성공 상태와 실패 상태 모두 result라는 키를 공통적으로 가지고 있으므로 바로 접근이 가능해졌다.

이렇게 discriminated type을 이용하면 조금 더 직관적으로 코드를 작성할 수 있게 되었다.
