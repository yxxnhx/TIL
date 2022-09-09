## Web Storage

- **Local Storage**

key value의 쌍으로 데이터를 저장 및 조회할 수 있다.

일종의 저장소라고 볼 수 있다.

서버에 전달되지 않는다.

지우지 않는 이상, 브라우저의 탭을 닫거나 다시 열어도 유지된다

- **Session Storage**

key value의 쌍으로 데이터를 저장 및 조회할 수 있다.

일종의 저장소라고 볼 수 있다.

서버에 전달되지 않는다.

같은 탭 안에서만 유지가 된다

탑을 닫거나 브라우저를 다시 열면 데이터는 지워져있다.(휘발성)

### setItem / getItem

- localStorage.setItem(’name’, ‘홍길동')
- localStorage.getItem(’name’)

→ 문자열, 숫자, 오브젝트, 배열 모두 가능

→ 문자열로 반환하여 저장하기 때문에 오브젝트 형태를 그대로 반환되지 못한다

**그렇다면 이 문자열의 형태열을 어떻게 해야 할까?**

- JSON.stringify 오브젝트, 객체, 배열을 json 문자열로 변환
- JSON.parse 문자열을 다시 오브젝트 형식으로 반환

<br />

- **localStorage.removeItem('age')**

→ age가 삭제된 것을 확인할 수 있다.

<br />
- **localStorage.clear()**

→ 전부 삭제된 것을 확인할 수 있다
