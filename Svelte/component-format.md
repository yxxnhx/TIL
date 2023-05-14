# Component format

### 컴포넌트 포멧

```jsx
<script>
	// logic goes here
</script>

<!-- markup (zero or more items) goes here -->

<style>
	/* styles go here */
</style>
```

상단에 script 태그 내부에서 로직이 이루어지고 그 아래에 화면에 출력될 요소들이 배치된다. 그리고 그 하단에 스타일링이 들어간다.

리액트에서는

```jsx
import React from "react";

export default function App() {
  return <div></div>;
}
```

화면에 출력될 부분은 클래스형 컴포넌트에서는 render, 함수형 컴포넌트에서는 return을 활용하여 마크업을 하고, 로직은 컴포넌트 내부(return 문 위) 혹은 상황에 따라 컴포넌트 외부에 배치하였는데 이 점은 다르다.

### <script>

script 블록은 컴포넌트 인스턴스가 생성되었을 때 실행되는 스크립트이다.

변수를 선언하거나 import를 최상위에서 선언한 경우, 해당 컴포넌트 마크업 구조에서 사용이 가능하다.

**이 스크립트에는 4가지 조건이 있다.**

- 컴포넌트 props을 생성하기 위해서는 export를 해야 한다.

```jsx
<script>
  export let foo; // Values that are passed in as props // are immediately
  available console.log({foo});
</script>
```

```jsx
//실제 사용 예시
<script lang="ts">
    export let text: string;
</script>

<span> {text} </span>
```

리액트에서 컴포넌트 안에서 사용할 때는 export를 사용하지 않고도 접근이 가능하다는 것이 가장 큰 차이점이다.

```jsx
<script>
  export let bar = 'optional default initial value'; export let baz = undefined;
</script>
```

미리 디폴트 파라미터를 설정해줄 수 있다.

```jsx
<script>
	// these are readonly
	export const thisIs = 'readonly';

	export function greet(name) {
		alert(`hello ${name}!`);
	}

	// this is a prop
	export let format = n => n.toFixed(2);
</script>
```

변수 외에도 함수 또한 export를 활용하여 사용해야 한다.

- Assignments are ‘reactive’

컴포넌트의 상태를 변경하거나 리랜더링을 원할 경우에는 `할당 =` 을 이용해서 가능하다.

다음 예시를 보자.

```jsx
<script>
	let count = 0;

	function handleClick () {
		// calling this function will trigger an
		// update if the markup references `count`
		count = count + 1;
	}
</script>
```

- $

어떤 최상위 상태에서도 `$:` 를 이용하여 반응성을 가지도록 할 수 있다.

반응성 구문은 컴포넌트가 값의 변경에 의해 업데이트 되기 전에 신속하게 실행된다.

```jsx
<script>
	export let title;
	export let person;

	// this will update `document.title` whenever
	// the `title` prop changes
	$: document.title = title;

	$: {
		console.log(`multiple statements can be combined`);
		console.log(`the current title is ${title}`);
	}

	// this will update `name` when 'person' changes
	$: ({ name } = person);

	// don't do this. it will run before the previous line
	let name2 = name;
</script>
```

→ 리액트에서 hook를 꺼내쓴 것처럼 상태를 변경 가능하다.

- \***\*Prefix stores with `$` to access their values**

```jsx
<script>
  import {writable} from 'svelte/store'; const count = writable(0);
  console.log($count); // logs 0 count.set(1); console.log($count); // logs 1
  $count = 2; console.log($count); // logs 2
</script>
```

스토어 앞에 $를 활용하여 스토어에 대한 참조를 만들 수 있다.

$변수가 재할당이 될 때는 writable한 스토어야 하며 할당이 이루어질 때는 store의 set 함수를 호출한다.

store은 최상단에서 선언되어야하며 if / 함수 블럭 내부에서 선언되면 안된다.

또한 스토어 값을 전달하지 않는 지역 변수는 절대로 $를 접두사로 이용하면 안된다.
