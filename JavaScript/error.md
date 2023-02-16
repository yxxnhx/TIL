# 에러 처리 Error Handling

```jsx
function readFile(path) {
  return "파일의 내용";
}

function processFile(path) {
  const content = readFile(path);
  const result = "hi" + content;
  return result;
}

const result = processFile("경로");
console.log(result); //hi파일의 내용
```

위와 같은 파일을 읽고 프로세싱하는 함수들이 있다고 가정하자.

<br />

**여기에서 error를 강제로 새롭게 throw를 하게 되면 어떻게 될까?**

```jsx
function readFile(path) {
  throw new Error("파일 경로를 찾을 수 없음");
  return "파일의 내용";
}

function processFile(path) {
  const content = readFile(path);
  const result = "hi" + content;
  return result;
}

const result = processFile("경로");
console.log(result);
```
<img width="564" alt="스크린샷 2022-09-25 오후 9 43 39" src="https://user-images.githubusercontent.com/50559373/192153597-52df83e3-8429-4e21-9dac-b9789ad8218a.png">

→ readFile의 return은 죽고 app crashed 된 것을 확인할 수 있다

<br />

**이와 같이 error를 효과적으로 잡아내기 위해서는 try catch finally를 이용하면 좋다**

```jsx
function readFile(path) {
  throw new Error("파일 경로를 찾을 수 없음");
  return "파일의 내용";
}

function processFile(path) {
  let content;
  try {
    content = readFile(path);
  } catch (error) {
    console.log(error.name); //Error
    console.log(error.message); //파일 경로를 찾을 수 없음
    console.log(error.stack); //Error: 파일 경로를 찾을 수 없음
    content = "기본내용"; //hi기본내용
  }
  const result = "hi" + content;
  return result;
}

const result = processFile("경로");
console.log(result);
```
<img width="561" alt="스크린샷 2022-09-25 오후 9 50 30" src="https://user-images.githubusercontent.com/50559373/192153606-8522eea4-461f-4f6d-9917-b51867573313.png">

→ 위와 같이 try catch문을 이용하면 app crashed가 되지 않고 에러의 이름, 메세지, 위치를 찾아내고 기본 내용을 출력해내어 조금 더 우아하게 에러를 잡아낼 수 있다

→ 먼저 content를 readFile의 path로 읽어본 다음 에러가 난다면 에러의 이름, 메세지, 위치를 잡아주고 컨텐츠로 기본내용을 반환해줘

<br />

**여기에서 finally를 추가해보자!**

```jsx
function readFile(path) {
  throw new Error("파일 경로를 찾을 수 없음");
  return "파일의 내용";
}

function processFile(path) {
  let content;
  try {
    content = readFile(path);
  } catch (error) {
    console.log(error.name);
    console.log(error.message);
    console.log(error.stack);
    content = "기본내용";
  } finally {
    console.log("성공하든 실패하든 마지막으로 리소스를 정리할 수 있음");
  }
  const result = "hi" + content;
  return result;
}

const result = processFile("경로");
console.log(result);
```

→ finally는 성공하든 실패하든 마지막으로 리소스를 정리할 수 있다.

 <br />

## 에러의 버블링 error bubbling

```jsx
function a() {
  throw new Error("error!");
}

function b() {
  a();
}

function c() {
  b();
}

try {
  c();
} catch (error) {
  console.log("Catched!");
}
console.log("done!");
```

→ 위와 같이 a 함수에서 에러가 났어도 b 함수에서 a를 호출하고 c함수에서 b를 호출하기 때문에 c까지 에러가 버블링되어 전달되는 것을 확인할 수 있다

→ 위는 가장 마지막에 호출한 곳에서 에러를 잡는 방식이고

<br />

```jsx
function a() {
  throw new Error("error!");
}

function b() {
  try {
    a();
  } catch (error) {
    console.log(error);
  }
}

function c() {
  b();
}

try {
  c();
} catch (error) {
  console.log("Catched!");
}
console.log("done!");
```

→ 이것은 에러가 난 곳과 가장 근접한 곳에서 에러를 잡는 방식이다

<br />

**에러를 내가 어디서 잡고 싶은가에 따라서 위치를 잡으면 된다.**

```jsx
function b() {
  try {
    a();
  } catch (error) {
    console.log("생각해보니까 이 에러는 내가 핸들링 불가!");
    throw error;
  }
}
```

→ 에러를 잡긴 잡았는데 해결을 못할 시에는 위와 같이 다시 던질 수도 있다
