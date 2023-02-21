# Partial type

기존의 타입이 있을 때 부분적인 것만 허용하고 싶을 때 사용 가능하다.

**다음 예제를 보며 확인해보자.**

```tsx
type ToDo = {
  title: string;
  description: string;
  label: string;
  priority: "high" | "low";
};
```

위의 ToDo라는 타입을 활용하여 updateTodo라는 함수가 일부만 업데이트 시키고 싶다면 어떻게 해야할까?

```tsx
function updateTodo(todo: ToDo, fieldsToUpdate: Partial<ToDo>): ToDo {
  return {
    ...todo,
    ...fieldsToUpdate,
  };
}

const todo: ToDo = {
  title: "learn typescript",
  description: "study hard",
  label: "study",
  priority: "high",
};

const update = updateTodo(todo, { priority: "low" });
console.log(update);
/* {
  title: 'learn typescript',
  description: 'study hard',
  label: 'study',
  priority: 'low'
} */
```

위처럼 todo를 Todo로 받아오고 업데이트하고 싶은 부분을 fieldsTodoUpdate라는 인자로 받아오되 Partial 프로퍼티로 받아올 수 있게 설정하였다

그 이후에 원하는 것을 업데이트 해주면 정상적으로 원하는 부분만 변경되는 것을 확인할 수 있다.

그러나 여기서 한가지 의문이 생긴다.


![스크린샷 2023-02-21 오전 12 06 14](https://user-images.githubusercontent.com/50559373/220396361-221ef285-5f77-448f-b80b-7cc5df68d10b.png)

priority에 마우스를 올려보면 선언할 수 있는 것들이 high, low 외에 설정해주지 않은 undefined가 뜨는 것을 확인할 수 있다.

**왜일까?**

답은 생각보다 매우 간단하다.

우리가 원하는 것을 변경하기 위해 Partial을 활용하였는데 Partial 프로퍼티 설명을 확인해보자.

```tsx
/**
 * Make all properties in T optional
 */
type Partial<T> = {
  [P in keyof T]?: T[P];
};
```

→ 바로 Partial의 T가 모든 프로퍼티를 옵셔널로 받아와서 변경할 수 있게 하였기 때문에 undefined까지 선언할 수 있도록 된 것이다.
