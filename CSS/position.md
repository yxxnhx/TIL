# CSS position

### **position 프로퍼티**

html 요소 위치를 결정하는 방식 설정으로, 다음과 같이 크게 4가지 방식이 있다.

**1. 정적 위치 static position 지정 방식 -** default

- 다른 태그와의 관계에 의해 자동으로 배치, 임의로 위치 설정 불가
- 상속 등으로 설정된 position을 리셋할 때 사용

**2. 상대 위치 relative position 지정 방식**

- 기본 위치(static으로 지정되었을 때의 위치)를 기준으로 좌표 지정 가능
- static으로 지정되었을 때의 공간을 다른 요소가 차지하지 않음
- 문서의 일반적인 흐름에 포함됨
  → 문서의 일반적인 흐름에 포함되므로, relative 미설정시 원래 있어야 하는 위치의 공간은 남겨두고, 요소가 상대적 위치로 이동하는 것임

**3. 절대 위치 absolute position 지정방식**

- 가장 가까운 위치에 있는 position 속성이 **relative 인 부모 요소**를 기준으로 좌표 프로퍼티(top, bottom, left, right)만큼 이동
- position 속성이 relative인 부모 요소가 없으면 body를 기준으로 위치
- 문서의 일반적인 흐름에 포함되지 않고, 페이지 레이아웃 공간도 배정하지 않음
  → 다른 요소가 먼저 위치를 점유하고 있어도 뒤로 밀리지 않고 덮어쓰게 됨(부유 특성 or 부유 객체라고 함)

**4. 고정 위치 fixed position 지정 방식**

- 부모 요소와 관계없이 브라우저의 viewport를 기준으로 좌표프로퍼티(top, bottom, left, right)를 사용하여 위치에 배치(스크롤되도 움직이지 않음)
- 문서의 일반적인 흐름에 배제, 웹페이지 레이아웃 공간도 배정하지 않음
  → 따라서, 다른 요소가 먼저 위치를 점유해도 덮어씌우게 됨(이런 특성을 부유 또는 부유 객체라고 한다)

```jsx
<style>
      div {
        display: inline-block;
        width: 200px;
        height: 200px;
        border: 3px solid sienna;
        color: aliceblue;
      }
      .box1 {
        background: red;
      }
      .box2 {
        background: blue;
        position: relative;
        top: 50px;
        left: 50px;
      }
      .box3 {
        background: green;
        position: absolute;
        top: 50px;
        left: 50px;
      }
      .box4 {
        background: sandybrown;
        position: absolute;
        top: 50px;
        left: 50px;
      }
      .box5 {
        background: black;
        position: fixed;
        top: 50px;
        left: 400px;
      }
    </style>
  </head>
  <body>
    <div class="box1">default</div>
    <div class="box2">
      relative
      <div class="box4">absolute</div>
    </div>
    <div class="box3">absolute</div>
    <div class="box5">fixed</div>
    <p>abcde</p>
    <p>abcde</p>
    <p>abcde</p>
    <p>abcde</p>
    <p>abcde</p>
    <p>abcde</p>
    <p>abcde</p>
    <p>abcde</p>
    <p>abcde</p>
    <p>abcde</p>
    <p>abcde</p>
    <p>abcde</p>
    <p>abcde</p>
    <p>abcde</p>
    <p>abcde</p>
    <p>abcde</p>
    <p>abcde</p>
```

→ green absolute는 relative 특성을 가진 부모요소가 없으므로 body를 기준

### **z-index**

- z-index property에 **큰 숫자값**을 지정할수록 화면 **전면**에 출력
- positon property가 **static 이외**인 요소에만 적용

```jsx
    <style>
      div {
        display: inline-block;
        width: 200px;
        height: 200px;
        border: 3px solid sienna;
        color: aliceblue;
        border-radius: 10px;
      }
      .box1 {
        background: red;
        position: absolute;
        top: 30px;
        left: 30px;
        z-index: 1000;
      }
      .box2 {
        background: green;
        position: absolute;
        top: 70px;
        left: 70px;
        z-index: 100;
      }
      .box3 {
        background: blue;
        top: 150px;
        left: 150px;
        z-index: 10;
      }
      .box4 {
        background: yellow;
        position: absolute;
        top: 100px;
        left: 100px;
        z-index: 1;
      }
    </style>
  </head>
  <body>
    <div class="box1"></div>
    <div class="box2"></div>
    <div class="box3"></div>
    <div class="box4"></div>
  </body>
```

→ blue box는 position property가 static이므로 z-index와 위치property 적용x

### **overflow**

overflow 프로퍼티는 자식 요소가 부모 요소의 영역를 벗어났을 때 처리 방법을 정의한다.

| value   | 설명                                                                                           |
| ------- | ---------------------------------------------------------------------------------------------- |
| visible | 부모 영역을 벗어난 부분을 표시 (default)                                                       |
| hidden  | 부모 영역을 벗어난 부분을 잘라내어 보이지 않게 함                                              |
| scroll  | 부모 영역을 벗어난 부분이 없어도 스크롤 표시 (현재 대부분 브라우저는 auto와 동일하게 작동한다) |
| auto    | 부모 영역을 벗어난 부분이 있을때만 스크롤 표시                                                 |

```jsx
    <style>
      div {
        display: inline-block;
        width: 150px;
        height: 150px;
        border: 3px solid sienna;
        border-radius: 10px;
      }
      .box1 {
        background: pink;
        position: absolute;
        top: 10px;
        left: 10px;
        overflow: visible;
      }
      .box2 {
        background: lightgreen;
        position: absolute;
        top: 10px;
        left: 170px;
        overflow: hidden;
      }
      .box3 {
        background: lightskyblue;
        position: absolute;
        top: 10px;
        left: 330px;
        overflow: scroll;
      }
      .box4 {
        background: lightsalmon;
        position: absolute;
        top: 10px;
        left: 490px;
        overflow: auto;
      }
    </style>
  </head>
  <body>
    <div class="box1">Lorem ipsum, dolor sit amet consectetur adipisicing elit. Ad saepe sequi sed fugiat ex id optio in, tenetur quaerat expedita quas nobis quae sapiente tempore recusandae laborum accusantium praesentium officia.</div>
    <div class="box2">Lorem ipsum, dolor sit amet consectetur adipisicing elit. Ad saepe sequi sed fugiat ex id optio in, tenetur quaerat expedita quas nobis quae sapiente tempore recusandae laborum accusantium praesentium officia.</div>
    <div class="box3">Lorem ipsum, dolor sit amet consectetur adipisicing elit</div>
    <div class="box4">Lorem ipsum, dolor sit amet consectetur adipisicing elit. Ad saepe sequi sed fugiat ex id optio in, tenetur quaerat expedita quas nobis quae sapiente tempore recusandae laborum accusantium praesentium officia.</div>
  </body>
```
