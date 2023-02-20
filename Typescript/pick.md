# Pick type

방대한 양의 데이터 중 원하는 것을 골라서 제한적으로 사용할 때 사용할 수 있다.

다음 예시를 보며 확인해보자

```tsx
type Video = {
  id: string;
  title: string;
  url: string;
  data: string;
};
```

비디오 타입에는 id title 외에 다양한 데이터들이 있는 무거운 데이터라고 해보자.

```tsx
function getVideo(id: string): Video {
  return {
    id,
    title: "video",
    url: "https://~",
    data: "byte-data",
  };
}
```

getVideo 함수에서는 id를 받아서 Video 타입을 반환한다.

```tsx
function getVideoMetadata(id: string) {
  return {
    id,
    title: "title",
  };
}
```

그런데 getVideoMetadata 함수에서는 아이디와 타이틀만 반환하게 하고 싶다면 어떻게 해야할까?

**Pick 프로퍼티를 활용해보자!**

먼저 Pick이 정의된 부분부터 확인해보자

```tsx
/**
 * From T, pick a set of properties whose keys are in the union K
 */
type Pick<T, K extends keyof T> = {
  [P in K]: T[P];
};
```

→ T라는 타입과, T라는 타입의 키들을 받아와 상속한 K를 받아서 사용한다.

즉, T에 선언된 key들만 사용할 수 있다는 뜻이다.

그렇게 받아온 K를 빙글빙글 돌면서 타입을 결정하게 도와주는 프로퍼티이다.

그렇다면 한번 사용해보자

```tsx
function getVideoMetadata(id: string): Pick<Video, "id" | "title"> {
  return {
    id,
    title: "title",
  };
}
```

이처럼 직접적으로 사용해도 되지만 타입을 선언하여서 이 타입을 계속 재사용할 수 있도록 하는 것이 좋다.

한번 타입을 선언해서 사용해보자

```tsx
type VideoMetadata = Pick<Video, "id" | "title">;

function getVideoMetadata(id: string): VideoMetadata {
  return {
    id,
    title: "title",
  };
}
```

이처럼 정보들이 많은 타입이 있고 그중 몇가지만 다룰 때는 Pick을 이용하여 사용하면 좋다.
