# JSON

### JavaScript Object Natation

서버와 클라이언트(브라우저, 모바일) 간의 HTTP 통신을 위한 오브젝트 형태의 텍스트 포멧

- stringifgy(object) : JSON
- parse(JSON) : object

다음 예시를 보자

```jsx
const yxxn = {
  name: 'yxxn',
  age: 20,
  eat: () => {
    console.log('eat');
  },
};
```

위와 같은 오브젝트가 있다고 가정하자.

### JSON.stringify

객체를 제이슨 문자열로 변환

```jsx
const json = JSON.stringify(yxxn);
```

위 오브젝트를 JSON 문자열로 변환하여 콘솔에 찍어 각각 비교해보자

```jsx
console.log(yxxn); //{ name: 'yxxn', age: 20, eat: [Function: eat] }
console.log(json); //{"name":"yxxn","age":20}
```

→ 제이슨 문자열로 변환하면 eat 함수는 변환이 되지 않은 것을 확인할 수 있다.

제이슨을 이용하여 변환을 하면 프로퍼티 즉, 상태만 변환이 되는 것을 확인할 수 있다.

### **JSON으로 인코딩된 객체는 일반 객체와 다른 특징을 보인다**

- 문자열은 큰따옴표로 감싸야 한다.
  JSON에선 작은따옴표나 백틱을 사용할 수 없다
  → `'yxxn'`이 `"yxxn"`으로 변경된 것을 통해 이를 확인할 수 있다
- 객체 프로퍼티 이름은 큰따옴표로 감싸야 한다
  → `age:20`이 `"age":20`으로 변한 것을 통해 이를 확인할 수 있다.

### `JSON.stringify`는 객체뿐만 아니라 원시값에도 적용할 수 있다

- 객체 `{ ... }`
- 배열 `[ ... ]`
- 원시형:
  - 문자형
  - 숫자형
  - 불린형 값 `true`와 `false`
  - `null`

******************\*\*******************그렇다면 이 json 문자열을 다시 오브젝트로 변환해보자******************\*\*******************

### JSON.parse

제이슨을 객체로 변환

```jsx
const obj = JSON.parse(json);
console.log(obj); //{ name: 'yxxn', age: 20 }
```

제이슨에 있던 프로퍼티 상태들이 다시 오브젝트로 변환이 되는 것을 확인할 수 있다.

- 직렬화 Serailizing: 객체를 문자열로 변환
- 역직렬화 Deserailizing : 문자열 데이터를 자바스크립트 객체로 변환
