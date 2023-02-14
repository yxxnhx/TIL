# Intersection

`Union type`은 발생할 수 있는 모든 타입 중에서 하나였다면 `Intersection`은 그 모든 것을 다 합한 성격을 가지고 있다.

쉽게 설명하자면 Union Type은 or | 이었다면 Intersection Type은 and & 의 개념이라고 생각하면 쉽다.

다음 예시를 보자

```tsx
type Student = {
  name: string;
  score: number;
};

type Worker = {
  employeeId: number;
  work: () => void;
};

function intern(person: Student & Worker) {
  console.log(person.name, person.score, person.employeeId, person.work());
}
```

intern이라는 함수는 Student와 Worker 타입을 모두 받는다고 설정을 하면 이름과 점수 그리고 employeeId와 work 함수까지 모두 접근할 수 있다.

그렇다면 호출할 때도 동일하게 설정을 하여 호출해야 한다.

```tsx
intern({
  name: "yxxn",
  score: 4.5,
  employeeId: 111,
  work: () => {},
});
```

이처럼 Intersection Type을 이용하면 다양한 타입들을 하나로 묶어서 사용할 수 있다.
