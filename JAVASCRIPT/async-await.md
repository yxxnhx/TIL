# async / await

한번 위의 예시 중에서 바나나와 사과를 배열로 가지고 올 수 있는 함수를 만들어보자

```jsx
function fetchFruits() {
  return getBanana() //
    .then((banana) =>
      getApple() //
        .then((apple) => [banana, apple]),
    )
    .then(console.log);
}

fetchFruits().then((fruits) => console.log(fruits));
```

→ 지금 then이 계속 중첩되어 있는 형태들이 이전에 프로미스를 사용하지 않고 쓰는 중첩된 콜백 지옥과 다를 바가 없다.

**그렇다면 어떻게 해결할 수 있을까?**

**async / await를 이용하여 코드를 조금 더 깔끔하게 정리를 해보자**

```jsx
async function fetchFruits() {
  const banana = await getBanana();
  const apple = await getApple();
  return [banana, apple];
}
```

→ 기다렸다가 getBanana를 호출하는 것을 banana, getApple을 호출하는 것을 apple로 선언을 하고 그 banana와 apple을 배열에 담아 리턴하는 함수를 async로 만들어낸 것이다

# async await 퀴즈

```jsx
function fetchEgg(chicken) {
  return Promise.resolve(`${chicken} => 🥚`);
}

function fryEgg(egg) {
  return Promise.resolve(`${egg} => 🍳`);
}

function getChicken() {
  return Promise.resolve(`🪴 => 🐓`);
  // return Promise.reject(new Error('치킨을 가고 올  수 없음'));
}

getChicken()
  .catch(() => '🐔')
  .then(fetchEgg)
  .then(fryEgg)
  .then(console.log);
```

위의 코드를 async await로 변경을 해보자

먼저 한번에 가져올 함수를 만들어보자

```jsx
function makeFriedEgg() {
  return getChicken()
    .catch(() => '🐔')
    .then(fetchEgg)
    .then(fryEgg)
    .then(console.log);
}

makeFriedEgg().then(console.log); //undefined
```

이렇게 호출하면 undefined가 나오는 것을 확인할 수 있다.

왜냐하면 makeFriedEgg 함수가 getChicken 프로미스를 리턴하는 경우라면 마지막으로 준 fryEgg를 전달하고 출력하지만 리턴하지는 않기 때문에 콘솔에는 undefined가 뜨는 것이다.

****************************\*\*\*\*****************************그렇다면 어떻게 해야 할까?****************************\*\*\*\*****************************

```jsx
function makeFriedEgg() {
  return getChicken()
    .catch(() => '🐔')
    .then(fetchEgg)
    .then(fryEgg)
    .then((result) => {
      console.log(result);
      return result;
    });
}

makeFriedEgg().then(console.log); //🪴 => 🐓 => 🥚 => 🍳
```

→ 결과를 받아서 콘솔에도 찍고 결과를 리턴하게 선언을 해주어야 정상적으로 값이 출력되는 것을 확인할 수 있다.

**********\*\*\*\***********이제 한번 async await로 변경을 해보자**********\*\*\*\***********

먼저 변경하기 전에 makeFriedEgg 앞에 전구가 뜨는 것을 확인할 수 있다.

```
async function makeFriedEgg() {
  let chicken;
  try {
    chicken = await getChicken();
  } catch {
    chicken = '🐔';
  }
  const egg = await fetchEgg(chicken);
  const data = await fryEgg(egg);
  return console.log(data);
}
```

→ 전구에 커서를 옮기면 비동기 함수로 변환을 해주는 옵션이 있는데 그 옵션을 누르면 다음과 같이 출력이 되는 것을 확인할 수 있다.

try catch는 Promise.reject를 활용하여 에러를 만들었기 때문에 생기었고 chicken이라는 변수를 먼저 선언하고 정상적으로 resolve 된다면 getChicken을 실행하고 아니라면 '🐔'을 불러줘

그 이후에 fetchEgg를, fryEgg를 await를 활용하여 만든 후에 해당 데이터를 콘솔에 반환해준다
