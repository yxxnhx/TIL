## Promise

비동기적으로 수행하는 그 수행하는 값을 끝났다는 것을 알려주는 **오브젝트이다**

내가 무겁고 오래 걸리는 일이 있다면 코드 내부에서 조금 더 비동기적으로 처리할 수 있도록 도와준다.

만약 내가 언제 끝날지는 모르겠는데 여기에 promise가 있으니 언젠가 끝나면 알려줄게 일종의 약속을 해준다. 그래서 promise에는 일이 성공적으로 끝났을 때의 then, 에러가 났을 때의 catch, 최종적으로 마무리하는 finally가 있다.

### **promise의 상태**

- pending : 이제 막 프로미스가 만들어진 상태, 일이 끝나지 않은 상태
- fulfiled : 일이 성공적으로 끝난 상태
- rejected : 실패 에러가 난 상태

```jsx
function runInDelay(callback, seconds) {
  if (!callback) {
    throw new Error("callback 함수를 전달해야 함");
  }
  if (!seconds || seconds < 0) {
    throw new Error("seconds는 0보다 커야 함!");
  }
  setTimeout(callback, seconds * 1000);
}

try {
  runInDelay(() => {
    console.log("타이머 완료");
  }, 2);
} catch (error) {}
```

### **이 코드를 프로미스를 활용해서 조금 더 깔끔하게 작성해보도록 하자**

프로미스를 사용하면 위와 같이 콜백을 전달하고 전달하는 콜백이 중첩되는 형식이 필요가 없어진다.

**조금 더 깔끔하게 함수형 프로그래밍이 가능해진다!**

```jsx
function runInDelay(seconds) {
  return new Promise();
}
```

→ 프로미스를 사용하게 되면 callback을 전달받지 않는다.

→ 바로 return을 활용해서 무언가를 수행하는 것이 아니라 이 함수는 promise 오브젝트를 return하게 해준다. new 생성자를 활용하여 Promise를 만들어낸다

즉, runInDelay를 호출하면 즉각적으로 Promise를 리턴한다.

프로미스라는 객체를 가지고 있으면 어느 시점에 몇 초가 지나서 타임아웃이 완료가 되면 성공적으로 끝났는지 실패했는지 알려줄게!

```jsx
try {
  runInDelay(() => {
    console.log("first");
  }, 0);
} catch (error) {}
```

즉, 사용자의 입장에서 위와 같이 try, catch로 잡아내지 않아도 다음과 같이 then, catch, finally를 활용하여 코드를 깔끔하게 작성할 수 있다.

```jsx
runInDelay(2)
 .then(필요한 일을 수행)
 .catch(에러를 처리)
 .finally(최종적으로 무조건 호출된다)
```

→ runInDelay 함수가 2초 뒤에 함수가 성공적으로 끝나면 then, 에러가 난다면 catch, 최종적으로 무조건 호출되는 finally를 이용해 수행할 일을 처리할 수 있도록 한다

```jsx
setTimeout(() => {
  resolve();
}, seconds * 1000);
setTimeout(resolve, seconds * 1000);
```

→ 아무것도 인자를 호출하지도 받지도 않기 때문에 다음과 같이 생략이 가능하다

```jsx
runInDelay()
  .then(() => {
    console.log("타이머 완료");
  })
  // .catch((error) => console.error(error))
  // 전달되는 인자와 호출할 때 전달하는 인자가 똑같으므로 다음과 같이 생략이 가능하다
  .catch(console.error)
  .finally(() => console.log("끝"));
```

→ then, catch, finally를 활용하여 수행할 일을 만들어준다

**그렇다면 프로미스를 어떻게 성공하고 실패하게 할 것인지 지정을 해주자.**

```jsx
function runInDelay(seconds) {
  return new Promise((resolve, reject) => {
    // setTimeout(() => {
    //   resolve();
    // }, seconds * 1000);
    // 아무것도 인자를 호출하지도 받지도 않기 때문에
    // 함수의 이름만 사용하여 다음과 같이 생략이 가능하다
    setTimeout(resolve, seconds * 1000);
  });
}
```

→ 프로미스를 만들 때 인자를 받아서 무언가를 처리할 콜백함수를 만들어야 한다.

이 프로미스 콜백함수 안에서 비동기적으로 일을 처리할 수 있게 해야 하는데 성공적으로 수행했을 때 즉 then일 때 사용할 resolved와 실패했을 때, error일 때 reject를 받아와야 한다.

```jsx
function runInDelay(seconds) {
  return new Promise((resolve, reject) => {
    if (!seconds || seconds < 0) {
      reject(new Error("seconds가 0보다 작음"));
    }
    setTimeout(resolve, seconds * 1000);
  });
}
```

→ 부합하지 않는 조건, 즉 error의 조건을 if 문을 활용하여 작성한다

reject를 할 때에는 new 연산자를 이용하여 Error 오브젝트를 만들어야 한다

### 이제 함수를 실행해보자

![Untitled](/img/promise_1.png)

→ runInDelay에 2초를 부여하니 성공적으로 타이머 완료와 끝이 출력되는 것을 확인할 수 있다.

**그렇다면 초를 부여하지 않게 에러가 뜨면 어떻게 처리가 될까?**

![Untitled](/img/promise_2.png)

→ error 메시지와 함께 어느 위치에서 에러가 떴는지 안내 그리고 최종적으로 무조건 호출이 되는 finally까지 호출된 것을 확인할 수 있다.

### 만약 catch가 없다면 어떻게 될까?

프로미스를 만들 때에는 new 연산자를 이용하여 생성해야 하고, Promise 생성자 안에는 promise를 만들 수 있는 콜백함수를 전달해야 한다. 이 콜백함수는 프로미스를 통해 호출이 될 것이고, 콜백함수에는 resolve 성공하였을 때, reject 에러, 실패했을 때의 각각의 콜백함수를 받아와서 호출해올 것이다.
