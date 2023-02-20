# Mapped type

mapped type을 이용하면 기존의 타입을 이용하여 다른 형태로 변환할 수 있다.

**다음 예시를 보며 index type과 차이점을 확인해보자.**

비디오라는 타입이 있다고 해보자

```tsx
type Video = {
  title: string;
  author: string;
};
```

여기에서 title과 author의 정보가 있어도 되고 없어도 되는 옵셔널 타입이 하나 더 필요해졌다고 하면 어떻게 해야할까?

```tsx
type VideoOptional = {
  title?: string;
  author?: string;
};
```

이와 같이 VideoOptional이라는 타입을 설정하여 옵셔널을 이용하여 있어도 되고 없어도 되는 데이터로 설정할 수 있다.

그런데 만약 Video 타입에 데이터가 추가 되었다면 어떻게 해야할까?

```tsx
type Video = {
  title: string;
  author: string;
  description: string;
};

type VideoOptional = {
  title?: string;
  author?: string;
  description?: string;
};
```

또 이렇게 각각 추가를 해주었다.

이번에는 readonly인 데이터를 만들고 싶다면 어떻게 해야할까?

```tsx
type VideoReadonly = {
  readonly title: string;
  readonly author: string;
  readonly description: string;
};
```

이렇게 또 타입을 새롭게 생성하여 만들었다.

**이렇게 매번 하나가 바뀔 때마다, 기존의 타입을 이용하여 다른 새로운 타입을 만들고 싶을 때마다 일일히 하나하나 다 만들어야할까?**

mapped type을 이용하면 간편하게 하고 재사용을 할 수 있다!

```tsx
type Optional<T> = {
  [P in keyof T]?: T[P];
};
```

[]를 사용하면 for in과 비슷한 역할을 한다.

for in은 오브젝트에 있는 모든 키들을 모두 하나하나씩 도는 연산자

즉, Optional 타입 오브젝트 안에서 제너릭으로 T를 받는데 T를 뱅글뱅글 돌면서 T 타입에 있는 모든 키들을 P에 할당하여 T의 키, P의 타입을 할당해주는 것이다.

그러므로 여기에서는 옵셔널 타입으로 할당해준 것이다.

이제 Optional 타입을 이용하여 옵셔널 타입을 만들어보자.

```tsx
type VideoOptional = Optional<Video>;
```

```tsx
type VideoOptional = {
  title?: string;
  author?: string;
  description?: string;
};
```

→ 이렇게 타입을 이용하여 설정해주면 아래와 동일하게 설정이 되어지는 것이다.

그렇다면 다음 예시를 보며 재사용을 해보자

```tsx
type Animal = {
  name: string;
  age: number;
};

const animal: Optional<Animal> = {
  name: "yxxn",
};

animal.name = "ha";
```

→ 옵셔널 타입이기 때문에 age를 선언해주지 않아도 문제가 되지 않는 것을 확인할 수 있다.

그러나 readonly 타입이 아니기 때문에 안에 있는 데이터를 바꿀 수가 있다.

그렇다면 readonly 타입도 만들어보자.

```tsx
type ReadOnly<T> = {
  readonly [P in keyof T]: T[P];
};
```

```tsx
const readonlyAnimal: ReadOnly<Animal> = {
  name: "초코",
  age: 20,
};

readonlyAnimal.name = "보리"; // 불가능
```

→ readonly 타입으로 설정이 되어진 것을 확인할 수 있다.

이처럼 mapped type을 이용하면 기존의 타입에서 다른 타입으로 성질을 변화할 수 있다.

```tsx
type Nullable<T> = {
  [P in keyof T]: T[P] | null;
};

const obj2: Nullable<Video> = {
  title: "null",
  author: null,
  description: null,
};
```

→ 위와 같이 null 또는 가져온 데이터의 타입을 할당할 수 있도록 설정해줄 수도 있다.

```tsx
type Proxy<T> = {
  get(): T;
  set(value: T): void;
};

type Proxify<T> = {
  [P in keyof T]: Proxy<T[P]>;
};
```

→ 위처럼 타입을 가져오되 Proxy라는 타입으로 한단계 감싸서 가져올 수도 있다.
