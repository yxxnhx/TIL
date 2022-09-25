# 구조 분해 할당 Destructuring Assignment

데이터 뭉치(그룹화)를 쉽게 만들 수 있다

```jsx
const fruits = ["🍏", "🥝", "🍓", "🍌"];
```

→ 이러한 배열이 있을 때 첫번째 인자를 받아오고 싶을 때에는 어떻게 해야 할까?
<br />

```jsx
console.log(fruits[0]); //🍏
```

→ 이와 같이 인덱스 번호를 이용하여 값을 출력해왔다.

<br />

**그러나 구조 분해 할당을 이용하면 고유한 변수 값으로 불러올 수 있다!**

```jsx
const [first, second, ...others] = fruits;
console.log(first); //🍏
console.log(others); //[ '🍓', '🍌' ]
```

→ fruits의 배열을 풀어서 가져오는데 first에는 첫 번째 값, second에는 두 번째 값, others에는 나머지 값이 들어간 것을 확인할 수 있다.

<br />

```jsx
const point = [1, 2];
const [x, y] = point;
console.log(x); //1
console.log(y); //2
```

→ 이와 같이 고유한 의미있는 이름으로 호출하여 사용이 가능하다!

<br />

```jsx
const [x, y, z = 0] = point;
console.log(z); // 0
```

→ point 안에 z값이 없다면 기본 값을 설정해줄 수도 있다

<br />

```jsx
const point = [1, 2, 8];
const [x, y, z = 0] = point;
console.log(z); //8
```

→ 당연히 point에 값이 있다면 8이 출력되는 것을 확인할 수 있다

<br />

**이것을 함수로도 사용이 가능하다!**

→ react에서 많이 사용한다

```jsx
function createEmoji() {
  return ["apple", "🍎"];
}

const [title, emoji] = createEmoji();
console.log(title); //apple
console.log(emoji); //🍎
```

<br />

**오브젝트에서는 어떻게 활용할 수 있을까?**

```jsx
const yxxn = {
  name: "yxxn",
  age: 20,
  job: "front-end engineer",
};

function display(person) {
  console.log("이름", person.name);
  console.log("나이", person.age);
  console.log("직업", person.job);
}
```

→ 원래대로라면 이렇게 계속 person.을 반복해야 하는데 이것을 처음부터 person이 아닌 구조를 분해해서 가져올 수 있다

<br />

```jsx
function display({ name, age, job }) {
  console.log("이름", name); //이름 yxxn
  console.log("나이", age); //나이 20
  console.log("직업", job); //직업 front-end engineer
}

display(yxxn);
```

→ 이와 같이 깔끔하게 정리될 수 있는 것을 확인할 수 있다

<br />

**함수를 이용하지 않고 처음부터 구조를 분해하여 가져올 수도 있다**

```jsx
const { name, age, job } = yxxn;
console.log(name); //yxxn
console.log(age); //20
console.log(job); //front-end engineer
```

<br />

**배열에서처럼 기존 값이 없다면 기본값을 지정해줄 수도 있다**

```jsx
const { name, age, job, pet = "dog" } = yxxn;
console.log(pet); // dog
```

<br />

**여기서 job이라는 값을 occupation이라는 키값으로 변경을 해서 사용도 가능하다**

```jsx
const { name, age, job: occupation, pet = "dog" } = yxxn;
console.log(job);
console.log(occupation); // front-end engineer
```

<br />

<img width="550" alt="스크린샷 2022-09-25 오후 3 30 01" src="https://user-images.githubusercontent.com/50559373/192131686-f636725f-3bab-4749-b109-51a481eb8873.png">

→ 콘솔에 job을 찍으면 job을 찾을 수 없다는 error가 나는 것을 확인할 수 있다.

<br />

[구조분해할당 Quiz](https://www.notion.so/Quiz-9d6d46e8c30b4bba96b1ca2922e8a886)

## 구조분해할당 Quiz

```jsx
const prop = {
  name: "Button",
  styles: {
    size: 20,
    color: "black",
  },
};

function changeColor(인자를 만들어보세요) {
  console.log(color);
}
```

→ prop.styles.color으로 접근하지 않고 바로 color로 접근할 수 있도록 만들어보기

<br />

```jsx
function changeColor({ styles: { color } }) {
  console.log(color);
}
changeColor(prop); //black
```

→ 구조분해할당은 구조를 하나하나 분해할 수 있기 때문에 styles라는 키에서 한번 더 구조를 분해하여 color에 접근할 수 있다.

<br />

**단, 여기서 styles를 콘솔에 찍어보면 다음과 같은 오류가 난다**

<img width="559" alt="스크린샷 2022-09-25 오후 3 38 51" src="https://user-images.githubusercontent.com/50559373/192131693-03e91ab4-2337-45bc-9834-1ea45a70aced.png">

→ 변수가 아니기 때문에 접근이 불가능하여 styles is not defined라는 오류가 나는 것이다
