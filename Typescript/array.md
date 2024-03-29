# Array 배열

배열에 타입을 설정할 수 있는 방법은 두 가지가 있다.

```tsx
const scores: number[] = [1, 2, 3];
const score: Array<number> = [1, 2, 3];
```

이와 같이 number[]을 이용하거나 Array<number>을 이용하여 설정하는 방법이 있다.
이 둘의 차이는 제너릭 파트에서 더 자세하게 알아보도록 하자
만약 배열을 출력하는 printArray라는 함수가 있다고 치자.
그런데 여기에서 주어진 데이터를 이용해서 무언가를 출력하거나 다른 것을 할 수 있지만 변경하거나 업데이트 하지 못하게 하고 싶다면 어떻게 해야 할까?

- **readonly**

```tsx
function printArray(fruits: readonly string[]) {
  fruits.push;
}
```

![스크린샷 2023-02-13 오전 12 31 38](https://user-images.githubusercontent.com/50559373/218321949-d5bce4a9-6633-4286-b396-2982f6a77476.png)


→ 바로 사용할 수 없다고 에러가 뜨는 것을 확인할 수 있다.

그러나 Array<> 형식에서는 제공되지 않는 것을 확인할 수 있다.

![스크린샷 2023-02-13 오전 12 36 25](https://user-images.githubusercontent.com/50559373/218321956-3b5f25d7-1293-4893-804f-d26a16daa707.png)

만약 readonly를 작성해야할 때에는 string[] 형식을 이용하도록 하자.
오브젝트의 불변성을 보장하는 것은 매우 중요하기 때문에 readonly가 많이 쓰인다.
그러므로 코드의 통일성을 위해서는 string[]의 형식으로 작성하는 것이 좋다.
