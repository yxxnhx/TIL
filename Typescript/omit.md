# Omit type

Omit type은 Pick과 반대로 선택한 것을 뺄 수 있다.

**Omit의 정의를 한번 확인해보자**

```tsx
/**
 * Construct a type with the properties of T except for those in type K.
 */
type Omit<T, K extends keyof any> = Pick<T, Exclude<keyof T, K>>;
```

→ Omit은 전달된 T 타입에 any타입의 키들을 상속하는 K

**이 말은 무엇일까?**

Omit의 전달된 키가 기존 타입에 없는 타입도 가능하다는 이야기이다.

```tsx
type Video = {
  id: string;
  title: string;
  url: string;
  data: string;
};

type VideoMetadata = Omit<Video, "url" | "data" | "score">; // 오류 없음
```

→ 기존 타입에 이 키들이 있다면 그것들을 제외하고 만들기 때문에 다른 어떤 종류의 키도 전달 가능하다.

그렇게 받아온 키들에서 Pick을 이용한다.

T 타입을 그대로 유지하면서 T에 있는 키들 중에 K를 제외하여 Pick 하여 사용한다는 뜻이다.

그렇다면 Exclude 정의를 확인해보자.

```tsx
/**
 * Exclude from T those types that are assignable to U
 */
type Exclude<T, U> = T extends U ? never : T;
```

→ U가 T 안에 들어있다면 never, 그 타입은 절대 사용하지 않을 것이고, 없을 때만 T를 사용하겠다는 뜻이다.

즉, Exclude는 전달된 T에 U가 제외된 것만 사용한다는 것이다.

이처럼 내가 빼고자 하는 것이 명확하다면 Omit을, 선택하고자 하는 것이 명확하다면 Pick을 이용하여 코드를 짤 수 있다.
