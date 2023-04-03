# 클로저 Closure

함수가 선언될 때 자동으로 생성되는 렉시컬 환경에 대한 설명입니다. 이러한 렉시컬 환경은 스코프 체인(scope chain)을 형성하게 되는데, 스코프 체인은 함수가 선언될 때의 모든 변수와 함수를 포함하는 렉시컬 스코프(lexical scope)를 형성합니다. 외부 함수가 실행 되고 반환된 후에도 외부 함수의 범위 내의 함수에 체이닝을 할 수 있는 함수 입니다. 정보를 은닉하기 위해서 주로 사용 합니다.**-> 매우 중요한 개념이므로 꼭 이해하고 면접에 임하세요!**

```tsx
function outer {
	const outerVa1 = '난 외부 변수야';

	function inner() {
		console.log(outerVa1);
	}

	return inner;
}

const innerExecute = outer();
innerExecute(); // '난 외부 변수야'
```

→ outer는 inner를 return 했습니다.innerExecute에는 outer에서 반환한 inner함수가 할당되게 됩니다. innerExecute를 호출할 때마다, outer에서 정의한 outerVal을 출력하게 됩니다. 이때 inner 함수는 outer 함수의 스코프 체인을 유지하게 되는데, 이러한 특성을 이용하여 클로저를 구현한 것입니다.
