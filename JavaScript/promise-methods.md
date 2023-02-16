# 프로미스 함수들

```jsx
function fetchEgg(chicken) {
  return new Promise((resolve, reject);
}
```

기존대로라면 promise를 사용하기 위해서는 new 생성자 함수를 이용하여 콜백함수를 통해서 위와 같이 생성을 해왔다. 그리고 프로미스 여러가지를 이용하여 체이닝할 수도 있다. 프로미스를 잘 수행하면 다음 프로미스를, 그 다음 프로미스를 등을 이용하는 것을 체이닝이라고 한다,

<br/>

**프로미스에서 사용할 수 있는 static method들이 있다.**

<br/>

### Promise.all

프로미스가 담겨있는 배열 등의 이터러블을 인자로 받는다. 그리고 전달받은 모든 프로미스를 병렬로 처리하고 그 처리 결과를 resolve하는 새로운 프로미스를 반환한다.

여러 개의 프로미스를 동시에 실행시키고 모든 프로미스가 준비될 때까지 기다린다고 해보자.

```jsx
Promise.all([
  new Promise((resolve) => setTimeout(() => resolve(1), 3000)), // 1
  new Promise((resolve) => setTimeout(() => resolve(2), 2000)), // 2
  new Promise((resolve) => setTimeout(() => resolve(3), 1000)), // 3
])
  .then(console.log) // [ 1, 2, 3 ]
  .catch(console.log);
```

→ 첫번째 프로미스는 3초, 두번째 프로미스는 2초, 세번째 프로미스는 1초후에 처리하여 반환한다.

이때 모든 프로미스의 처리가 종료될 때까지 기다린 후 모든 처리 결과를 resolve 또는 reject한다.

<br/>

한번 다음 예시를 보자

```jsx
function getBanana() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve('🍌');
    }, 1000);
  });
}

function getApple() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve('🍎');
    }, 3000);
  });
}

function getOrange() {
  return Promise.reject(new Error('no orange'));
}
```

**여기에서 바나나와 사과를 같이 배열로 가지고 오도록 해보자**

```jsx
getBanana()
  .then((banana) => getApple().then((apple) => [banana, apple]))
  .then(console.log); //[ '🍌', '🍎' ]
```

바나나는 1초, 사과는 3초 후에 가지고 오기 때문에 총 4초 후에 콘솔에 [ '🍌', '🍎' ]이 찍히는 것을 볼 수 있다.

**그렇다면 Promise.all을 이용하여 병렬적으로 한꺼번에 가져오도록 해보자**

```jsx
Promise.all([getBanana(), getApple()]).then((fruits) =>
  console.log('all', fruits),
); //all [ '🍌', '🍎' ]
```

→ 바나나 1초, 사과는 3초 뒤에 가지고 오기 때문에 3초 후에 all [ '🍌', '🍎' ]이 찍히는 것을 볼 수 있다.

조금 더 효율적으로 동시다발적으로 가져올 수 있다.

<br/>

**그런데 만약에 한꺼번에 가져온 함수들 중에 에러가 난다면 어떻게 될까?**

```jsx
function getBanana() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve('🍌');
    }, 1000);
  });
}

function getApple() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve('🍎');
    }, 3000);
  });
}

function getOrange() {
  return Promise.reject(new Error('no orange'));
}

Promise.all([getBanana(), getApple(), getOrange()]).then((fruits) =>
  console.log('all-error', fruits),
);
```

![Untitled](/img/promise%20%EC%98%A4%ED%9B%84%203.42.48.png)

→ 앱이 크러쉬되어 죽는 것을 확인할 수 있다. 에러가 났는데 잡을 catch가 없기 때문에 에러가 난다

**그렇다면 catch를 추가해보자**

```jsx
Promise.all([getBanana(), getApple(), getOrange()])
  .then((fruits) => console.log('all-error', fruits))
  .catch((error) => console.log(error));
```

![Untitled](/img/promise%20%EC%98%A4%ED%9B%84%203.45.14.png)

→ 에러를 잡고 그 이후의 프로미스들이 출력되는 것을 확인할 수 있다. 대신에 에러가 났기 때문에 then은 실행이 되지 않은 것을 알 수 있다.

**그런데 만약, 에러가 난 것이랑 성공한 것을 모두 받아오고 싶다면 어떻게 해야 할까?**

다음 Promise.allSettled를 사용해보자

### Promise.allSettled

```jsx
Promise.allSettled([getBanana(), getApple(), getOrange()])
  .then((fruits) => console.log('all-settled', fruits))
  .catch((error) => console.log(error));
```

![Untitled](/img/promise%20%EC%98%A4%ED%9B%84%203.48.07.png)

→ 실패하든 성공하든 배열로 묶어서 보내준다

### Promise.race

Promise.race 메소드는 Promise.all 메소드와 동일하게 프로미스가 담겨 있는 배열 등의 이터러블
을 인자로 전달 받는다.

그리고 Promise.race 메소드는 Promise.all 메소드처럼 모든 프로미스를 병렬 처리하는 것이 아니라 가장 먼저 처리된 프로미스가 resolve한 처리 결과를 resolve하는 새로운 프로미스를 반환한다.

즉, 쉽게 말해 주어진 프로미스 중에 제일 빨리 수행된 것이 이긴다 레이싱을 한다고 생각하면 쉽다.

```jsx
Promise.race([
  new Promise((resolve) => setTimeout(() => resolve(1), 3000)), // 1
  new Promise((resolve) => setTimeout(() => resolve(2), 2000)), // 2
  new Promise((resolve) => setTimeout(() => resolve(3), 1000)), // 3
])
  .then(console.log) // 3
  .catch(console.log);
```

에러가 발생한 경우는 Promise.all 메소드와 동일하게 처리된다.

즉, Promise.race 메소드에 전달된 프로미스 처리가 하나라도 실패하면 가장 먼저 실패한 프로미스가 reject한 에러를 reject하는 새로운 프로미스를 즉시 반환한다.

**다음 예시를 보자**

```jsx
Promise.race([getBanana(), getApple()]).then(
  (fruit) => console.log('race', fruit), //race 🍌
);
```

→ 바나나가 1초, 사과가 3초이기 때문에 1초만에 바나나만 출력된 것을 확인할 수 있다.

### Promise.resolve(value)

존재하는 값을 Promise로 래핑하기 위해 사용한다.

호환성을 위해 함수가 프로미스를 반환하도록 해야 할 때 사용할 수 있다.

Promise.all 메소드는 프로미스가 담겨 있는 배열 등의 이터러블을 인자로 전달 받는다. 그리고 전달받은 모든 프로미스를 병렬로 처리하고 그 처리 결과를 resolve하는 새로운 프로미스를 반환한다.

```jsx
function fetchEgg(chicken) {
  return Promise.resolve(`${chicken} => 🥚`);
}

fetchEgg('🐔').then((egg) => console.log(egg)); //🐔 => 🥚
```

**체이닝을 해보자**

```jsx
function fetchEgg(chicken) {
  return Promise.resolve(`${chicken} => 🥚`);
}

function fryEgg(egg) {
  return Promise.resolve(`${egg} => 🍳`);
}

function getChicken() {
  return Promise.resolve(`🪴 => 🐓`);
}

getChicken()
  .then((chicken) => fetchEgg(chicken))
  .then((egg) => fryEgg(egg))
  .then((friedEgg) => console.log(friedEgg)); //🪴 => 🐓 => 🥚 => 🍳
```

여기에서 조금 더 코드를 간단하게 만들어낼 수 있다.

```jsx
getChicken().then((chicken) => fetchEgg(chicken));
```

```jsx
getChicken().then((chicken) => {
  return fetchEgg(chicken);
});
```

→ 모두 동일하다. 값이 하나일 경우에는 return을 쓰지 않고 바로 리턴할 값을 화살표 뒤에 넣어도 된다

### Promise.reject(reason)

위의 경우에서 에러 reject를 만들어보자

```jsx
function fetchEgg(chicken) {
  return Promise.resolve(`${chicken} => 🥚`);
}

function fryEgg(egg) {
  return Promise.resolve(`${egg} => 🍳`);
}

function getChicken() {
  return Promise.reject(new Error('치킨을 가고 올  수 없음'));
}

getChicken()
  .then((chicken) => fetchEgg(chicken))
  .then((egg) => fryEgg(egg))
  .then((friedEgg) => console.log(friedEgg))
  .catch((error) => console.error(error.name)); //Error
```

→ 에러는 버블링이 되어 getChicken에서 일어난 에러라도 맨 마지막에서 에러가 잡혀서 나오는 것을 확인할 수 있다.

<br/>

**그렇다면 에러의 위치를 변경해보자**

<br/>

```jsx
getChicken()
  .catch((error) => {
    console.error(error.name);
    return '🐔';
  })
  .then((chicken) => fetchEgg(chicken))
  .then((egg) => fryEgg(egg))
  .then((friedEgg) => console.log(friedEgg)); //Error 🐔 => 🥚 => 🍳
```

→ 만약 에러가 난다면 에러의 이름과 치킨을 반환해주고 그 다음을 이행해줘

<br/>

**코드를 조금 더 간단하게 해보자**

전달하는 인자와 호출하는 인자가 동일하다면 함수의 이름 호출하는 참조값만 불러도 무관하다

```jsx
getChicken()
  .catch(() => '🐔')
  .then(fetchEgg)
  .then(fryEgg)
  .then(console.log); //🪴 => 🐓 => 🥚 => 🍳
```
