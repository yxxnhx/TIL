# index type

```tsx
const obj = {
  name: "yxxn",
};
```

오브젝트의 이름에 접근하고 싶다면 어떻게 해야 할까?

```tsx
obj.name; // yxxn
obj["name"]; // yxxn
```

→ 쉽게 [obj.name](http://obj.name) 혹은 obj의 키에 접근해서 이름을 출력하는 경우 총 두 가지의 방법이 있었다.

**이처럼 index type도 동일하다**

- 키에 접근해서 원하는 타입을 설정할 수가 있다.

다음의 예시를 보자.

```tsx
type Animal = {
  name: string;
  age: number;
  gender: "male" | "female";
};
```

애니멀이라는 타입에는 이름과 나이, 그리고 성별의 데이터를 담고 있다.

```tsx
type Name = Animal["name"]; // string

const text: Name = "hello";
const text: Name = 12; // 불가능
```

이와 같이 애니멀의 이름이라는 키에 접근하여 이름의 타입인 문자를 타입으로 선언할 수 있다.

그래서 확인해보면 text에는 문자열 외에는 다른 타입의 데이터가 들어오지 못하는 것을 확인할 수 있다.

```tsx
type Gender = Animal["gender"]; *// "male" | "female"*
```

→ 이와 같이 gender라는 유니온 타입을 빼와서 설정할 수도 있다.

그렇게 된다면 Gender 타입에는 male과 female 외에는 다른 것이 할당되지 못하는 것을 확인할 수 있다.

- keyof를 활용하면 모든 데이터 타입을 가져올 수 있다

```tsx
type Keys = keyof Animal; // 'name' | 'age' | 'gender'
const key: Keys = "age";
```

→ key 안에서 데이터를 할당하려는 순간 name과 age, gender가 추천으로 뜨는 것을 확인할 수 있다.

- 타입을 선언할 때도 인덱스 타입을 활용할 수 있다

```tsx
type Person = {
  name: string;
  gender: Animal["gender"];
};

const person: Person = {
  name: "yxxn",
  gender: "female",
};
```

→ 타입을 선언할 때도 애니멀의 젠더의 키에 접근하여 선언해주면 실제로 그 타입을 사용할 때 female 과 male의 선택지가 추천되는 것을 확인할 수 있다.

이처럼 인덱스 타입을 이용하면 다른 타입에 있는 키에 접근하여 그 키의 value의 타입을 그대로 다시 선언할 수 있다.
