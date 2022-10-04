# CSS boxmodel

block 또는 inline-block 특성을 가지는 요소는 box 형태를 가지며, box 형태의 세부 사항을 수정할 수 있음

> 웹 디자인은 contents를 담을 box model을 정의하고 CSS 속성을 통해 스타일(배경, 폰트와 텍스트 등)과 위치 및 정렬을 지정하는 것.

- 모든 HTML 요소는 box 형태로 되어있다.
- 하나의 박스는 네 부분(영역)으로 이루어 진다.
  - content / padding / border / margin

![https://lastcow9000.github.io/assets/ImagesForPosts/2021-03-11-CSS%20Box%20Model/css-box-model-box-sizing.png](https://lastcow9000.github.io/assets/ImagesForPosts/2021-03-11-CSS%20Box%20Model/css-box-model-box-sizing.png)

### box model 프로퍼티

1. Content
   - 글이나 이미지, 비디오 등 요소의 실제 내용
   - width, height property를 가짐
2. Padding (안쪽 여백)
   - Border(테두리) 안쪽의 내부 여백
   - 배경색, 이미지 지정 가능(기본색은 투명)
3. Border
   - 테두리 영역, 두께 지정 가능
4. Margin (바깥쪽 여백)
   - 테두리 바깥의 외부 여백
   - 배경색 지정 불가

```jsx
  <style>
      div {
        background-color: lightcoral;
        width: 200px;
        padding: 20px;
        border: 10px solid lightgreen;
        margin: 20px;
      }
  </style>
</head>
<body>
  <div>CSS BOX MODEL TEST 입니다. CSS BOX MODEL TEST 입니다.</div>
</body>
```

**※ 마진 상쇄**

- block의 top 및 bottom margin이 때로는 (결합되는 마진 중 크기가) 가장 큰 한 마진으로 결합(combine, 상쇄(collapsed))된다.

### 주요 프로퍼티

### **box-sizing, width / height 프로퍼티**

- `box-sizing` property는 default로 `content-box`가 지정되어 있다.
  - 이 경우 `width` / `height`는 content 영역의 너비와 높이다.
- `box-sizing` property가 `border-box`로 지정되어 있으면
  - 이 경우 `width` / `height`는 content + padding + border 영역의 너비와 높이다.
- `width` / `height` 를 포함하여 모든 box model 관련 property (box-sizing, padding, border, margin 등)는 **상속**되지 않는다.

<aside>
💡 컨텐츠가 지정된 width / height 보다 크면 content 영역 밖으로 컨텐츠가 넘칠 수 있는데,이 때, overflow property를 hidden으로 설정하면 넘친 컨텐츠를 감출 수 있다. ( 차지했던 영역도 같이 삭제)

</aside>

```jsx
    <style>
      div {
        background-color: lightcoral;
        width: 200px;
        height: 200px;
        border: 10px solid lightgreen;
      }
    </style>
  </head>
  <body>
    <div>
      Lorem ipsum dolor sit amet consectetur adipisicing elit. Libero, incidunt ut doloribus, laudantium similique explicabo quaerat totam perferendis minima delectus voluptas possimus veniam aut
      numquam laboriosam unde autem mollitia quisquam.
    </div>
    <p>TEST</p>
  </body>
```

→ box-sizing : content-box 1.1 overflow : hiddenbox-sizing : border-box 2.1 overflow : hidden

**※ 참고 : CSS 스타일링과 box-sizing**

- css 적용 시, 모든 block 요소는 box-sizing을 border-box로 설정하는 것이 일반적
- box-sizing은 default로 상속이 되지 않고 HTML 요소의 default box-sizing은 content-box 이므로, 전체 요소에 box-sizing을 border-box로 설정하기 위해 다음과 같은 css 설정을 많이 함

```jsx
,
*::before,
*::after { box-sizing: border-box;
}
```

### **max-width / max-height 프로퍼티**

요소 너비가 브라우저 너비보다 클 경우, 가로 스크롤바가 생길 수 있음

- 이 때, `max-width`를 사용하면 요소 너비가 브라우저 너비보다 클 경우, 자동으로 요소 너비가 줄어듬

```jsx
    <style>
      div {
        background-color: lightcoral;
        width: 1200px;
        border: 10px solid lightgreen;
      }
    </style>
  </head>
  <body>
    <div>
      Lorem ipsum dolor sit amet consectetur adipisicing elit. Libero, incidunt ut doloribus, laudantium similique explicabo quaerat totam perferendis minima delectus voluptas possimus veniam aut
      numquam laboriosam unde autem mollitia quisquam.
    </div>
    <p>TEST</p>
  </body>
```

> width :1200pxmax-width : 1200px

### **margin / padding property[Permalink](https://lastcow9000.github.io/frontend/CSS-Box-Model/#margin--padding-property)**

- `margin` 또는 `padding` 에 -top, -bottom, -left, -right 를 붙여서 설정 가능
- `margin` 또는 `padding` 에 윗쪽, 우측, 아랫쪽, 좌측(시계방향)의 값을 순서대로 작성하여 단축 property 사용 가능
  → 이 외에도 몇 개의 값을 지정하느냐에 따라, 단축 프로퍼티 설정이 달라짐 - 4개의 property 값 설정 시
      ```jsx
          <style>
            div {
              background-color: lightcoral;
              border: 10px solid lightgreen;
              margin: 10px 20px 30px 40px;
              /*  아래 설정과 같다.
              margin-top: 10px;
              margin-right: 20px;
              margin-bottom: 30px;
              margin-left: 40px;
              */
            }
          </style>
        </head>
        <body>
          <div>드뇌 빌뇌브</div>
          <div>Arrival</div>
          <div>시카리오</div>
        </body>
      ```

      - 3개의 property 값 설정 시

      ```jsx
      <style>
        div {
              background-color: lightcoral;
              border: 10px solid lightgreen;
              margin: 10px 20px 40px;
              /*  아래 설정과 같다.
              margin-top: 10px;
              margin-right: 20px;
              margin-left: 20px;
              margin-bottom: 40px;
              */
            }
      </style>
      ```

      - 2개의 property 값 설정 시

      ```jsx
      <style>
      	div {
              background-color: lightcoral;
              border: 10px solid lightgreen;
              margin: 10px 40px;
              /*  아래 설정과 같다.
              margin-top: 10px;
              margin-bottom: 10px;
              margin-right: 40px;
              margin-left: 40px;
              */
      	}
      </style>
      ```

      - 1개의 property 값 설정 시

      ```jsx
      <style>
      	div {
              background-color: lightcoral;
              border: 10px solid lightgreen;
              margin: 40px;
              /*  아래 설정과 같다.
              margin-top: 40px;
              margin-right: 40px;
              margin-bottom: 40px;
              margin-left: 40px;
              */
      	}
      </style>
      ```

**※ block 특성을 가진 요소에 대한 중앙 정렬**

1. `width` 지정
2. `margin-left`: `auto`, `margin-right` : `auto` 또는 `margin` : `10px` `auto`

<aside>
💡 block : 한 줄 모두 차지 (대표 element - <div>)inline : 콘텐츠 크기 만큼만 차지 (대표 element - <span>)

</aside>

```jsx
    <style>
      div {
        background-color: lightcoral;
        border: 10px solid lightgreen;
        width: 200px; /* 지정해줘야 함 */
        margin: 1px auto;
      }
    </style>
  </head>
  <body>
    <div>드뇌 빌뇌브</div>
    <div>Arrival</div>
    <div>시카리오</div>
  </body>
```

### **border 프로퍼티**

- `border-style` : 선 스타일 설정
  - [(참조)border-style MDN](https://developer.mozilla.org/ko/docs/Web/CSS/border-style)
- `border-width` : 선 굵기 설정, border-style이 설정되어있어야 사용 가능
  - 1px 와 같이 설정하거나 thin, medium, thick 키워드로 설정 가능
  - 다음과 같이 방향별로 설정 가능
    - `border-top-width`
    - `border-right-width`
    - `border-bottom-width`
    - `border-left-width`
  ```jsx
      <style>
        div {
          background-color: lightcoral;
          width: 300px;
          border-style: dashed;
          border-top-width: thick;
        }
      </style>
    </head>
    <body>
      <div>Lorem ipsum dolor sit amet consectetur adipisicing elit.
        Illo similique error quod sint sed ab nihil sunt ipsam quis commodi
        excepturi incidunt
      </div>
    </body>
  ```
- `border-color` : 선 색상 설정, border-style이 설정되어있어야 사용 가능
- `border-radius` : 테두리 모서리를 둥글게 표시
  - [(참조)border-radius MDN](https://developer.mozilla.org/ko/docs/Web/CSS/border-radius)

```jsx
    <style>
      div {
        background-color: lightcoral;
        width: 200px;
        border-style: solid;
        border-width: medium;
        border-color: cadetblue;
        border-radius: 10%;
      }
    </style>
  </head>
  <body>
    <div>Lorem ipsum dolor sit amet consectetur adipisicing elit. Illo similique error quod sint sed ab nihil sunt ipsam quis commodi excepturi incidunt</div>
  </body>
```

### **border 단축 프로퍼티**

- border-width, border-style, border-color 순으로 한번에 설정가능
