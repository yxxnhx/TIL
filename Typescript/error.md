# Exception

### 에러 처리하기

어플리케이션을 사용하다보면 예상하지 못한 오류가 발생하거나 메모리에 문제가 있거나 심각한 이슈가 발생하여 앱이 죽는 경우가 있다. 어플리케이션이 갑자기 중지되거나 사라진 경험들이 있을 것이다.

이렇게 갑자기 예상하지 못한 상황의 에러를 `Exception`이라고 한다.

이러한 exeption을 사용자가 사용하다가 에러들을 만나게 그대로 두는 것이 아니라 발생할 수 있는 exception을 잘 처리해야한다. 이러한 exception을 잘 처리, 핸들링하게 되면 안정성 뿐만 아니라 유지보수성 또한 높일 수 있다.

### 다음 에러 예시들을 보며 확인해보자.

다른 프로그래밍 언어 Java에서는 exception이라는 클래스, 오브젝트가 있다.

JavaScript나 TypeScript에서는 Error라는 클래스가 있다.

예를 들어 아래와 같이 엄청난, 커다란 범위의 배열을 만든다고 해보자.

```jsx
const array = new Array(1000000000000);
```

→ 만들 때는 에러가 발생하지 않는다.

![스크린샷 2023-02-20 오전 12 07 27](https://user-images.githubusercontent.com/50559373/220395499-a777f69c-8200-481a-8849-7c65568425ba.png)

그러나 ts-node를 활용하여 확인해보면 유효하지 않은 길이의 배열을 사용했다는 에러가 발생하는 것을 확인할 수 있다.

여기에서 RangeError란 Error 클래스를 상속한 세부적인 Error 클래스이다.

이처럼 전혀 예상치 못한 이슈가 발생할 때 쓸 수 있는 것이 에러이다.

**그렇다면 다른 예시를 보면서 한번 더 확인해보자**

```tsx
type Direction = "up" | "down" | "left" | "right";

function move(direction: Direction) {
  switch (direction) {
    case "up":
      return (position.y += 1);
    case "down":
      return (position.y -= 1);
    case "left":
      return (position.x -= 1);
    case "right":
      return (position.x += 1);
    default:
      throw Error("unknown direction");
  }
}
```

→ switch 문 안에서 Error를 쓴 것을 확인할 수 있다.

즉, move라는 함수를 호출할 때 전달할 수 있는 Direction, 모든 유니언 타입에 대해서 각각의 케이스별로 처리를 해주고 이 케이스들을 제대로 처리해주지 않으면 에러가 발생하게 된다.

**만약 이 함수를 쓰는 개발자들이 실수로 유니언 타입에 케이스를 추가하고 그에 대한 처리를 해주지 않았다고 해보자.**

```jsx
type Direction = "up" | "down" | "left" | "right" | "strange";
```

그렇다면 에러가 발생하며 설정해두었던 문구가 출력되는 것을 확인할 수 있다.

그러나 컴파일을 돌릴 때 에러가 발생하여 그것을 수정할 수 있도록 만드는 것이 좋다.

여기에서 쓸 수 있는 한가지 트릭이 있다.

```jsx
const invalid: never = direction;
```

이와 같이 direction이라고 선언을 하였지만 타입은 never이라고 해보자.


![스크린샷 2023-02-20 오전 12 16 04](https://user-images.githubusercontent.com/50559373/220395579-c4e47482-ac45-46ec-abaf-0be49d8448a1.png)

→ string 타입은 never 타입에 할당할 수 없다는 에러가 발생하는 것을 확인할 수 있다.

바로 strange를 처리해주지 않아 invalid에는 never를 사용할 수 없다는 것이다.

```tsx
case "strange":
  return (position.x += 1);
```

→ 추가로 strange의 케이스를 추가해주자 에러가 사라지는 것을 확인할 수 있다.

### 이제 본격적으로 Exception 처리를 어떻게 하는지 알아보자

에러 핸들링은 총 세 가지 단계로 나눌 수 있다.

- try : 에러가 발생할 수 있는 부분을 실행한다
- catch : 에러가 발생했다면 에러를 잡아낸다
- finally : 에러 발생 여부에는 상관없이 마무리 한다.

물론 정말 심각한 에러라면 어플리케이션이 죽을 수밖에 없을 것이다.

다음 예시를 보며 에러 처리에 대해 조금 더 자세하게 알아보자

```tsx
function readFile(fileName: string): string {
  if (fileName === "not exist") {
    throw new Error(`file not exist! ${fileName}`);
  }
  return "file contents";
}

function closeFile(fileName: string) {
  // 파일 닫기
}

const fileName = "file";
console.log(readFile(fileName)); // file
closeFile(fileName);
```

그렇다면 not exist로 파일 이름을 넘겨주면 어떻게 될까?

```tsx
const fileName = "not exist";
console.log(readFile(fileName));
closeFile(fileName);
```

![스크린샷 2023-02-20 오전 12 27 43](https://user-images.githubusercontent.com/50559373/220395639-6e3d0e0d-6908-44ee-ac6a-0509bf42a22d.png)

→ 앱이 죽고 파일이 존재하지 않고 그 파일 이름과 함께 정확한 위치정보까지 알려주고 있다.

이처럼 예상하지 못한 발생할 수 있는 에러를 쓸 경우에는 최대한 정확한 위치, 그 에러가 발생할 수 있는 위치에서 `try-catch`를 이용하여 에러 핸들링을 해야 한다.

```tsx
const fileName = "not exist";

try {
  console.log(readFile(fileName));
} catch (error) {
  console.log("catch");
}

console.log("종료");
closeFile(fileName);
```

→ 실행해보면 에러가 발생해서 catch가 호출되었고 마지막까지 어플리케이션이 정상적으로 동작하는 것을 확인할 수 있다.

**그렇다면 finally까지 추가해보자.**

```tsx
try {
  console.log(readFile(fileName));
} catch (error) {
  console.log("catch");
} finally {
  closeFile(fileName);
  console.log("finally");
}
console.log("종료");
```

→ 에러가 발생했든 안했든 catch, finally, 종료까지 모두 정상적으로 출력되어 작동되는 것을 확인할 수 있다.

이처럼, 실행의 성공여부에 상관없이 무조건 작동해야하는 것이 있다면 finally에 적는 것이 좋다.

**그렇다면 굳이 finally를 작성하지 않고 그냥 catch 밑에 적어도 실행되지 않을까?**

다음 예시를 보면서 알아보자.

우리는 run이라는 함수를 호출하여 file을 읽어오는데 거기에서 에러가 발생하면 에러를 잡고 에러 발생 여부와는 상관없게 파일을 닫는 함수 closeFile을 실행하고 종료까지 하기를 원한다고 가정해보자.

```tsx
function run() {
  const fileName = "not exist";

  try {
    console.log(readFile(fileName));
  } catch (error) {
    console.log("catch");
  }
  closeFile(fileName);
  console.log("close");
}

run();
```

→ 위의 상황에서는 run 함수를 호출했을 때 다행히도 정상적으로 error를 잡고 closeFile까지 작동 후에 close가 찍힌 것을 확인할 수 있다.

**그런데 만약 error가 발생했을 경우 더이상 코드를 실행하지 않고 리턴하도록 설정해보자.**

```tsx
function run() {
  const fileName = "not exist";

  try {
    console.log(readFile(fileName));
  } catch (error) {
    console.log("catch");
    return;
  }
  closeFile(fileName);
  console.log("close");
}

run();
```

이 상황에서는 에러를 잡기만 하고 closeFile이 작동하지 않은 것을 확인할 수 있다.

그러나 우리는 어느 상황에서도 closeFile이 작동하고 run 함수가 종료되기를 원한다.

이처럼 finally에는 try와 연관된 마무리되어야 할 일들을 작성하는 것이 좋다.

```tsx
function run() {
  const fileName = "not exist";

  try {
    console.log(readFile(fileName));
  } catch (error) {
    console.log("catch");
    return;
  } finally {
    closeFile(fileName);
    console.log("close");
  }
}

run();
```

→ 이제 정상적으로 finally까지 작동하는 것을 볼 수 있다.

**그렇다면 에러 핸들링에 대해서 다른 예시를 한번 더 보자**

```tsx
class NetworkClient {
  tryConnect(): void {
    throw new Error("no network");
  }
}

class UserService {
  constructor(private client: NetworkClient) {}

  login() {
    this.client.tryConnect();
    // login
  }
}

class App {
  constructor(private userService: UserService) {}

  run() {
    this.userService.login();
  }
}

const client = new NetworkClient();
const service = new UserService(client);

const app = new App(service);
app.run();
```

![스크린샷 2023-02-20 오전 12 53 06](https://user-images.githubusercontent.com/50559373/220395700-030993ad-9281-406f-9472-689e51884a41.png)

→ 현재 network가 없어서 에러가 뜨는 것을 확인할 수 있다.

호출한 곳은 NeteworkClinet의 tryConnect, 그리고 UserService의 login, App의 run이다.

이렇게 다양한 곳에서 사용하다가 에러가 나는 경우 어디에서 에러 핸들링 처리를 해주어야 할까?

```tsx
class NetworkClient {
  tryConnect(): void {
    throw new Error("no network");
  }
}

class UserService {
  constructor(private client: NetworkClient) {}

  login() {
    try {
      this.client.tryConnect();
    } catch (error) {
      console.log("caught");
    }
  }
}
```

NetworkClient에서는 tryConnect를 통해 에러가 발생하고 처음으로 쓰는 곳이 UserService이니까 그 안에서 처리를 해주면 되지 않을까?

그런데 한번 생각해보자.

UserSerivce에서 error를 잡아서 의미있게 할 수 있는 일이 있을까?

현재는 로그인만 하는 로직인데 error를 잡아서 할 수 있는 일이 없다.

```tsx
class App {
  constructor(private userService: UserService) {}

  run() {
    this.userService.login();
  }
}
```

App에서는 userService의 login만 호출하고 아무런 것도 받지 않았다.

즉, App에서는 에러가 발생했는지 여부를 전혀 모르는 상태이다.

여기에서 가장 중요한 것은 내가 에러를 잡아서 정확하게 처리를 해줄 수 없다면 처리하지 않는 것이 더 좋다.

결국 UserService에서 어정쩡하게 에러를 잡기 보다 처리할 수 있는 곳에서 처리하는 것이 가장 좋은 방법이다.

```tsx
class App {
  constructor(private userService: UserService) {}

  run() {
    try {
      this.userService.login();
    } catch (error) {
      // 사용자에게 다이얼로그를 보여주기
    }
  }
}
```

그러므로 App에서 에러를 처리하여 의미있게 사용자에게 다이얼로그를 보여주는 등의 의미있는 에러처리를 할 수 있게 작성하는 것이 좋다.
