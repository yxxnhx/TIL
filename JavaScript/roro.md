# RORO 패턴

roro 패턴의 장점

- 네임드 파라미터(Named parameters)
- 더 명확한 디폴트 파라미터(Cleaner default parameters)
- 더 풍부한 리턴 값(Richer return values)
- 쉬운 함수 컴포지션(Easy function composition)

### 네임드 파라미터(Named Parameters)

주어진 역할(Role)에 있는 사용자(Users) 목록을 리턴하는 함수가 있다고 가정하고, 각 사용자의 연락처 정보와 비활성 사용자를 포함하는 또 다른 옵션을 제공해야한다고 가정 해 보자. 일반적으로 다음과 같이 작성할 수 있다.

```jsx
function findUsersByRole (
  role,
  withContactInfo,
  includeInactive
) {...}
```

이 함수 호출은 다음과 같이 한다.

```jsx
findUsersByRole('admin', true, true);
```

마지막 두 파라미터가 얼마나 모호한지 보자. “true, true”가 무엇일까?

우리 앱에서는 연락처(Contact Info)는 거의 필요 없지만 비활성 사용자(Inactive Users)는 대부분의 경우 필요하다면 어떻게 될까? 필요가 없는 경우에도 우리는 그 중간(두 번째) 파라미터를 항상 넣어주어야 한다(나중에 이것에 대해서 다루겠다).

간단히 말해서, 이 전통적인 접근법은 우리에게 모호하고 시끄러운(noisy) 코드를 만든다. 이 코드는 이해하기 어렵고 작성하기가 더 까다롭다.

그 대신 하나의 객체를 받으면 어떻게 되는지 보자.

```jsx
function findUsersByRole ({
  role,
  withContactInfo,
  includeInactive
}) {...}
```

함수 파라미터 주위에 중괄호를 두었다는 점을 제외하고는 거의 동일해 보인다. 이는 세 개의 별개 파라미터를 받는 대신 우리 함수가 role, withContactInfo, 과 includeInactive라는 속성(property)을 가진 단일 객체를 기대한다는 것을 나타낸다.

이는 ES2015에서 소개 된 자바스크립트의 Destructuring 기능으로 인해 작동한다.

이제 다음과 같이 함수를 호출 할 수 있다.

```jsx
findUsersByRole({
  role: 'admin',
  withContactInfo: true,
  includeInactive: true,
});
```

이것은 모호하지 않고 읽고 이해하기가 훨씬 쉽다. 게다가 파라미터를 생략하거나 재정렬하는 것은 더 이상 문제가 아니다. 이제는 객체의 명명 된 속성이기 때문이다.
