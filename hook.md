# Hook

[useCount(custom hook)](https://www.notion.so/useCount-custom-hook-e88a58b333f446f6a12c1144534d9d0d)

**● 훅의 규칙**

1. 훅은 무조건 최상위 레벨에서만 호출해야 한다.

2. 반복문이나 조건문 또는 중첩된 함수들 안에서 훅을 호출하면 안된다.

3. 훅은 컴포넌트가 렌더링 될 떄마다 매번 같은 순서로 호출되어야 한다.

**● 플러그인**

eslint-plugin-react-hooks

- > 의존성 배열이 잘못되면 알려줌

![https://postfiles.pstatic.net/MjAyMjA3MjlfMjUx/MDAxNjU5MDc3MTk0OTIw._zl5GE_q75OqqO-A1ZWiwWLqMmR0HQHfeyNDtytZDfog.5mPAFmiUqGdJg3mliTZJR5CcaFvymmZElnmZF8K3ZGog.PNG.kkag8997/httpswww.npmjs.compackage.png?type=w773](https://postfiles.pstatic.net/MjAyMjA3MjlfMjUx/MDAxNjU5MDc3MTk0OTIw._zl5GE_q75OqqO-A1ZWiwWLqMmR0HQHfeyNDtytZDfog.5mPAFmiUqGdJg3mliTZJR5CcaFvymmZElnmZF8K3ZGog.PNG.kkag8997/httpswww.npmjs.compackage.png?type=w773)

**● Custom Hook 만들기**

반복적으로 사용되는 로직을 훅으로 추출해서 만들어 두고 필요할때마다 재사용하기 위해 만든다.

![https://postfiles.pstatic.net/MjAyMjA3MjlfOTEg/MDAxNjU5MDc3MjA1MDYx.7y9mNwWCn6w4-GnXFko6RUAb5RuCtiM1OW3EckbCuBEg.O9Qtv4FTC9YzoG6i4R3p7GrsII_rVJ9I7nGxI_GuULsg.PNG.kkag8997/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2022-07-29_%EC%98%A4%EC%A0%84_10.16.13.png?type=w773](https://postfiles.pstatic.net/MjAyMjA3MjlfOTEg/MDAxNjU5MDc3MjA1MDYx.7y9mNwWCn6w4-GnXFko6RUAb5RuCtiM1OW3EckbCuBEg.O9Qtv4FTC9YzoG6i4R3p7GrsII_rVJ9I7nGxI_GuULsg.PNG.kkag8997/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2022-07-29_%EC%98%A4%EC%A0%84_10.16.13.png?type=w773)

**● Custom Hook 추출하기**

이름이 use로 시작하고 내부에서 다른 훅을 호출하는 하나의 자바스크립트 함수

![https://postfiles.pstatic.net/MjAyMjA3MjlfMjY4/MDAxNjU5MDc3MjE1Njcz.EhACdLsSoB2g1GE174OjGsdsgNYZ6Gtyo7wA2XB7AtMg.iksQo55cDQN2ZKIutLVeufGPPmAnHa4MBqPar0TkweIg.PNG.kkag8997/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2022-07-29_%EC%98%A4%EC%A0%84_10.19.47.png?type=w773](https://postfiles.pstatic.net/MjAyMjA3MjlfMjY4/MDAxNjU5MDc3MjE1Njcz.EhACdLsSoB2g1GE174OjGsdsgNYZ6Gtyo7wA2XB7AtMg.iksQo55cDQN2ZKIutLVeufGPPmAnHa4MBqPar0TkweIg.PNG.kkag8997/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2022-07-29_%EC%98%A4%EC%A0%84_10.19.47.png?type=w773)

**● Custom Hook 사용하기**

![https://postfiles.pstatic.net/MjAyMjA3MjlfMTMx/MDAxNjU5MDc3MjI0MzE2.B4859e3ifUQE48Tomf14WqULxyaFgn0a0kIX0YyK6r4g.fZonemItAkw2FW2BCtbJovBEz5VKt37bSpYY99aKGnkg.PNG.kkag8997/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2022-07-29_%EC%98%A4%EC%A0%84_10.20.55.png?type=w773](https://postfiles.pstatic.net/MjAyMjA3MjlfMTMx/MDAxNjU5MDc3MjI0MzE2.B4859e3ifUQE48Tomf14WqULxyaFgn0a0kIX0YyK6r4g.fZonemItAkw2FW2BCtbJovBEz5VKt37bSpYY99aKGnkg.PNG.kkag8997/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2022-07-29_%EC%98%A4%EC%A0%84_10.20.55.png?type=w773)

Custom Hook을 잘 만들어놓으면 생산성이 향상되고 개발 속도가 빨라진다.

여러개의 컴포넌트에서 하나의 Custom Hook을 사용할 때 컴포넌트 내부에 있는 모든 state와 effects는 전부 분리되어 있다.

각 Custom Hook 호출에 대해서 분리된 state를 얻게 된다.

각 Custom Hook 호출 또한 완전히 독립적이다.

**● Hook들 사이에서 데이터를 공유하는 방법**

![https://postfiles.pstatic.net/MjAyMjA3MjlfMTg3/MDAxNjU5MDc3MjQ2Njky.poE19h-8aO4xxohS4gO3VkZdjXCQ7P8A6MU_JCd3b-Eg.UwdaRVxjen6bl0KzaUKgnWfnhKmnrwPZxrT1nfn8c2kg.PNG.kkag8997/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2022-07-29_%EC%98%A4%EC%A0%84_10.25.36.png?type=w773](https://postfiles.pstatic.net/MjAyMjA3MjlfMTg3/MDAxNjU5MDc3MjQ2Njky.poE19h-8aO4xxohS4gO3VkZdjXCQ7P8A6MU_JCd3b-Eg.UwdaRVxjen6bl0KzaUKgnWfnhKmnrwPZxrT1nfn8c2kg.PNG.kkag8997/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2022-07-29_%EC%98%A4%EC%A0%84_10.25.36.png?type=w773)

![https://postfiles.pstatic.net/MjAyMjA3MjlfMTMx/MDAxNjU5MDc3MjUwNTY3.ErZtH4p7-4UUb50eFrCtwkwQIsKZDmJAfAqBfUAro4sg.oeJQkj50xDD1vgE8iTg2nO0Vm_OkjQ5yEvuAZ3IidqMg.PNG.kkag8997/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2022-07-29_%EC%98%A4%EC%A0%84_10.27.34.png?type=w773](https://postfiles.pstatic.net/MjAyMjA3MjlfMTMx/MDAxNjU5MDc3MjUwNTY3.ErZtH4p7-4UUb50eFrCtwkwQIsKZDmJAfAqBfUAro4sg.oeJQkj50xDD1vgE8iTg2nO0Vm_OkjQ5yEvuAZ3IidqMg.PNG.kkag8997/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2022-07-29_%EC%98%A4%EC%A0%84_10.27.34.png?type=w773)

훅은 컴포넌트 상단에 모아놓는 것이 좋다.
