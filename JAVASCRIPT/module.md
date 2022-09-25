# 모듈 module

**index.html**

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <script src="counter.js"></script>
    <script src="main.js"></script>
  </head>
  <body></body>
</html>
```

**counter.js**

```jsx
let count = 0;
function increase() {
  count++;
  console.log(count);
}
```

**main.js**

```jsx
console.log(count);
increase();
console.log(count);
count = -10;
console.log(count);
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f9ef6fea-5479-4529-8df7-a72a4767e0ee/Untitled.png)

→ 이렇게 counter에서 선언하고 생성한 count와 increase()를 main에서도 재선언을 하고 값을 재할당을 할 수 있다.

<br />

이렇게 되면 서로 코드가 꼬이고 문제가 생기기 때문에 자바스크립트를 모듈화 해주어야 하는데 어떻게 할까?

**방법은 간단하다! script의 type을 지정해주면 된다!**

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <script type="module" src="counter.js"></script>
    <script type="module" src="main.js"></script>
  </head>
  <body></body>
</html>
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4b21af18-c800-4d89-a1b2-d3313d33700c/Untitled.png)

이렇게 각각의 스크립트들의 타입을 모듈로 설정을 해두면 count에서 선언한 increase 함수와 count를 사용할 수 없어 이전에 main에서 재할당하고 선언하였던 부분에서 에러가 나는 것을 볼 수 있다.

<br />

**이렇게 모듈화를 하면 어떤 부분은 외부에서 컨트롤 할 수 있게, 어떤 부분은 외부에서 볼 수 없게 컨트롤이 가능하다!**

```jsx
let count = 0;
export default function increase() {
  count++;
  console.log(count);
}
```

→ count는 외부에서 접근이 불가하게 모듈화하지만 increase()는 외부에서 컨트롤할 수 있게 export를 이용하여 설정할 수 있다

즉, export 시킬 건데 이게 default란다

<br />

```jsx
import increase from "./counter.js";

increase();
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/65ba08a2-db50-413d-b88e-600f187cce29/Untitled.png)

→ 이렇게 main에서 사용하고 싶을 때는 import 원하는 이름 from 위치로 경로를 설정해주면 오류 없이 사용할 수 있는 것을 확인할 수 있다.

여기에서 원하는 이름을 마음대로 설정을 해도 문제가 되지 않는다

왜냐하면 export default를 사용하게 되면 그 파일에서 하나만 export할 수 있기 때문이다

**만약에 하나 초과를 export하기를 희망한다면 default를 사용할 수 없다.**

<br />

```jsx
let count = 0;
export function increase() {
  count++;
  console.log(count);
}
```

```jsx
import { increase } from "./counter.js";

increase();
```

또한 **원하는 이름을 마음대로 지정할 수 없고 기존 export 파일에서 지정한 이름 그대로 선언하여 불러와야 한다**

<br />

**그렇다면 만약 이름을 바꾸고 싶다면 어떻게 해야 할까?**

```jsx
import { increase as i } from "./counter.js";

i();
```

→ 이렇게 as를 이용하여 원하는 이름으로 받아오겠다는 선언을 하면 된다

**그렇다면 카운트 수를 세는 함수를 만들어보자**

```jsx
let count = 0;
export function increase() {
  count++;
  console.log(count);
}

export function getCount() {
  return count;
}
```

```jsx
import { increase, getCount } from "./counter.js";

increase();
increase();
increase();

const count = getCount();
console.log(count);
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5908ff45-2b11-43f8-8834-6cc2e51e24ef/Untitled.png)

→ 여전히 main에서 count의 let으로 선언한 count 기본값은 접근을 할 수 없지만 몇 번 count 되었는지에 관해서는 접근하여 확인할 수 있다

<br />

**위와 같이 하나하나 불러오는 방법도 있지만 전체로 다 불러와서 사용하는 방법도 있다**

```jsx
import * as counter from "./counter.js";

counter.increase();
counter.increase();
counter.increase();

const count = counter.getCount();
console.log(count);
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5908ff45-2b11-43f8-8834-6cc2e51e24ef/Untitled.png)

→ counter.js에서 counter이라는 이름으로 전체를 가져올 것이라고 한 후 모든 함수나 변수에 앞에 지정한 이름을 붙여주면 동일하게 작동하는 것을 확인할 수 있다.**(그룹화)**

### 즉, 각각의 파일을 모듈화를 해주면 각각 파일별로 고립된 형태로 캡슐화해줄 수 있으며 어떤 것을 외부로 노출할 것인지 여부에 따라 지정해줄 수 있다.

→ export로 변수, 함수 등 모두 가져올 수 있다
그러나 가급적 변수에 직접 export 해오기보다는 getter, setter를 호출하는 것을 추천한다
