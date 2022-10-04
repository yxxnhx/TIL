# CSS block & inline

모든 HTML 태그는 각 태그마다 default로 block 또는 inline 특성을 가짐

해당 속성은 display property를 통해 변경 가능하다.

### **block 특성**

- 항상 새로운 라인에 표시
- 화면 너비 전체를 차지 (자동으로 width: 100%, height: auto 가 됨)
- width, height, margin, padding property 설정 가능
- block 요소 안에 inline 요소 포함 가능
- default로 block 특성을 가지는 주요 HTML 태그
  - div
  - h1 ~ h6
  - p
  - ol, ul, li
  - hr
  - form
  - table

### **inline 특성**

- 새로운 라인으로 시작x (동일한 라인에 다른 요소와 함께 위치 가능)
- content 너비만큼 가로폭 차지
- width, height, margin, padding property 설정 불가
  - 상, 하 여백은 line-height로 설정 가능
- inline 요소 안에 block 요소 포함 불가
- inline 특성을 가지는 요소 뒤에 공백(Enter 또는 Space, Tab 등)이 있을 경우, 정의하지 않은 Space(4px)가 자동으로 지정됨
  - 어떤 공백이든 임의로 Space(4px) 로 변환되어 표현됨
- default로 inline 특성을 가지는 주요 HTML 태그
  - span
  - a
  - strong
  - img
  - br
  - input
  - button
  - textarea
  - select

```jsx
<div style="background-color: lightcoral; width: 150px">
    block <span>특성</span> 테스트</div> /*block 요소 안에 inline 요소 포함 가능*/
<br />
<span style="background-color: lightcoral; width: 150px">
    inline <div>특성</div> 테스트	/*inline 요소 안에 block 요소 포함 불가*/
</span>
<span>span사이에 엔터가 공백(4px로)</span> /*span태그 사이에 엔터*/
```

### **display 프로퍼티는 요소의 display 프로퍼티를 설정**

default로 가진 block 또는 inline 특성을 변경 가능하다

| property 값  | 설명                                                              |
| ------------ | ----------------------------------------------------------------- |
| block        | block 특성을 가지는 요소로 설정                                   |
| inline       | inline 특성을 가지는 요소로 설정                                  |
| inline-block | inline-block 특성을 가지는 요소로 설정                            |
| none         | 해당 요소를 화면에 표시X (해당 요소가 차지하는 공간까지 사라진다) |

```jsx
<div style="background-color: lightcoral; width: 150px">block <span>특성</span> 테스트</div>
<br />
<span style="background-color: lightcoral; width: 150px">
    inline
    <div>특성</div>
    테스트
</span>
<span>span사이에 엔터가 공백(4px로)</span>
<span style="background-color: lightcoral; width: 150px; display: block">
    inline
    <div>특성</div>
    테스트
</span>
```

### **inline-block 특성**

block과 inline 특성 모두를 가진다.

- 다른 요소와 함께 동일 라인에 표시
- width, height, margin, padding property 설정 가능
  - 상, 하 여백은 margin 또는 inline-height 둘 다 가능
- content 너비만큼 가로폭 차지
- inline 특성을 가지는 요소 뒤에 공백(Enter 또는 Space, Tab 등)이 있을 경우, 정의하지 않은 Space(4px)가 자동으로 지정됨
  - 어떤 공백이든 임의로 Space(4px) 로 변환되어 표현됨
  - 이를 제거하기 위해서는 다음과 같은 방법을 사용
  ```jsx
  <!-- 1.태그와 태그 사이를 붙임-->
  <span>스페이스제거</span><span>
  스페이스제거</span>

  <!-- 2.태그 닫기와 열기를 붙임-->
  <span>스페이스제거</span
  ><span>스페이스제거</span>

  <!-- 3.태그와 태그 사이를 주석 처리함-->
  <span>스페이스제거</span><!--
  --><span>스페이스제거</span>
  ```
- inline block 예제 코드

```jsx
<div style="background-color: lightcoral; width: 150px">
    block <span>특성</span> 테스트
</div>
<br />
<span style="background-color: lightcoral; width: 150px; display: inline-block">
    inline-block 테스트
</span>
<span style="background-color: lightcoral; width: 150px; display: inline-block">
    inline-block 테스트
</span>
```

### **visibility 프로퍼티**

요소를 보이게 할 것인지 여부 설정

<aside>
💡 display none 과 visibility hidden의 차이점 알아두자

</aside>

| property 값 | 설명                                                                                                                                           |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| visible     | 해당 요소를 보이게 함(default)                                                                                                                 |
| hidden      | 해당 요소를 보이지 않게 함. display: none 은 해당 요소가 차지하는 공간까지 사라지지만, visibility: hidden 은 해당 요소가 차지하는공간은 남겨둠 |
| collapse    | table 요소에 사용하며 행이나 열을 보이지 않게 함                                                                                               |
