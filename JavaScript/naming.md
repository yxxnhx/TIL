# 네이밍 규칙

### 식별자 네이밍 규칙

식별자는 특수 문자를 제외한 문자, 숫자, 언더스코어, $를 포함할 수 있다.
특수 문자는 가급적 사용하지 않는다.
단, 상수의 이름에서 단어를 구분하기 위한 용도와 Private 지시자 표시를 위해 언더스코어를 사용한다.

- 단어 구분 : RESTAURANT_DETAIL
- Private 표시 : \_secret
  식별자는 숫자로 시작하는 것이 불가능하다.
  예약어는 식별자로 사용 불가능하다.
  await, break, catch, case, const, new ... 등

### 네이밍 컨벤션

- camelCase : 일반적으로 변수나 함수에 사용
- snake_case
- PascalCase : 생성자 함수, 클래스의 이름, 모듈에 사용
- 헝가리언 케이스 : v0.5.4 이전에 작성된 코드는 헝가리언 표기법 사용으로 기존 코드의 유지보수를 위해 통일성이 필요한 경우 사용할 수 있으나 그 외에는 사용하지 않음

```javascript
var strFistName; // type + idnetifier
var $elem = document.getElementById("userId");
```
