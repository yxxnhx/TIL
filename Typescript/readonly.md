# readonly type

데이터를 보여주기만 하고 수정할 수 없게 설정할 수 있는 타입이다.

```tsx
type ToDo = {
  title: string;
  description: string;
};

function display(todo: ToDo) {
  todo.title = "sleep";
}
```

위와 같이 readonly를 설정해두지 않는다면 title이나 description에 접근하여 변경할 수 있다.

그러나 변경하지 못하게 한다면 readonly를 이용할 수 있다.

```tsx
type ToDo = {
  readonly title: string;
  readonly description: string;
};
```

이렇게 설정하고 싶은 곳에 readonly를 설정해주어도 되지만 전체를 설정하고 싶을 때에는 다른 방법이 있다.

**바로 Readonly 프로퍼티를 이용하는 것이다!**

```tsx
function display(todo: Readonly<ToDo>) {
  todo.title = "sleep"; // 불가능
}
```

```tsx
/**
 * Make all properties in T readonly
 */
type Readonly<T> = {
  readonly [P in keyof T]: T[P];
};
```

내장되어있는 프로퍼티의 설명으로 들어가보면 모든 것이 정의되어있고 설정되어있는 것을 확인할 수 있다.
