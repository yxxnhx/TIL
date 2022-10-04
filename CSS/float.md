# CSS float

웹 요소를 문서 위에 떠 있게 만드는 것

left, right, none(float 속성을 해지하고 싶을 때) 으로 위치 지정

\*\* position 속성을 주기 전에는 컨텐츠는 겹칠 수 없음

\*\* 영역은 겹칠 수 있으나 컨텐츠는 겹칠 수 없음

### **float property**

- 요소를 떠 있게(요소가 기본 레이아웃 흐름에서 벗어나 요소의 모서리가 페이지의 왼쪽이나 오른쪽에 이동하는 것) 한다.
- 사용 시, 요소의 위치를 고정시키는 position: absolute를 사용하면 안됨
- 다양한 오류 케이스가 나타날 수 있지만 공식적인 해결방안은 없음(비공식적인 방안만 많음)
- 수평 정렬을 위해 사용가능하지만 차라리 flexbox를 사용하는 것이 더 효율적이다.

| Value | 설명                             |
| ----- | -------------------------------- |
| none  | 요소를 떠 있지 않게 함 (default) |
| right | 요소를 오른쪽으로 이동           |
| left  | 요소를 왼쪽으로 이동             |

### **clear property**

- float를 해제하는 property

| Value | 설명                             |
| ----- | -------------------------------- |
| none  | 양쪽 float를 사용 가능 (default) |
| right | 우측 float를 사용 해제           |
| left  | 좌측 float를 사용 해제           |
| both  | 양쪽 float를 사용 해제           |

```jsx
<body>
  <img src="hos.gif" style="float: right; width: 100px; height: 50px" />
  <div>Vue는 input form의 value 설정시 v-bind를 사용한다</div>
  <div style="clear: right">
    람다식을 자유롭게 쓰는 그날까지 브론즈 문제는 람다로.. print((lambda x, y: x
    * y // 2)(*map(int, input().split())))
  </div>
</body>
```

### **정렬(float)**

- 수평 정렬할 요소들을 float: left로 설정 시 좌측 정렬, right로 설정 시 우측 정렬(수평 중앙 정렬은 margin property를 사용해야 함)
- 우측 정렬 시 먼저 설정한 요소부터 우측 끝에 출력됨

```jsx
  <style>
    div {
      color: white;
      font-weight: bold;
      font-size: 50px;
      border-radius: 5px;
      width: 100px;
      height: 100px;
    }
    .box1 {
      background: red;
      float: left;
    }
    .box2 {
      background: green;
      float: left;
    }
    .box3 {
      background: blue;
      float: right;
    }
    .box4 {
      background: orange;
      float: right;
    }
  </style>
</head>
<body>
  <div class="box1">1</div>
  <div class="box2">2</div>
  <div class="box3">3</div>
  <div class="box4">4</div>
</body>
```

### **특성**

```jsx
  <style>
    div {
      margin: 0 auto;
      border-radius: 5px;
    }
    .box1 {
      background: pink;
      color: blueviolet;
      font-weight: bold;
      float: left;
    }
    .box2 {
      background: orange;
      color: blueviolet;
      font-weight: bold;
    }
    .box3 {
      background: skyblue;
      color: blueviolet;
      font-weight: bold;
      float: left;
      width: 100px;
      height: 100px;
    }
  </style>
</head>
<body>
  <div class="box1">box1 div</div>
  <span class="box3">box3 sapn</span>
  <div class="box2">box2 div</div>
</body>
```

- float property를 설정한 요소는 display 특성을 block 특성으로 변경함
- box3은 span이라 원래 inline 특성을 가지지만 block으로 변경되어 width와 height 설정이 가능해짐
- block 특성을 가진 요소는 width를 미설정 시 자동으로 100%가 되지만 float property를 설정한 요소는 요소의 크기만큼 width가 설정되어 inline-block으로 설정해준 것과 동일하게 동작함
- float property가 설정된 요소에 content가 있을 경우 해당 content의 너비와 높이는 다음 요소의 content에 영향을 줌(position: absolute처럼 완벽한 부유 특성과 다른 부유 특성을 가짐)

### clear

float으로 뜬 영역에 들어가지 않고 무효화할 수 있는 방법

```html
clear: both;
```

(float 준 속성대로 left, right 등으로 clear에 선언하면 되지만 보통은 both로 선언함)

→ 주로 both 사용

위에 있는 형제 요소들이 float 되었을 때 아래 요소들이 끌려가지 않도록

(원치 않아도 공간이 남으면 올라가게 되니까)

(형제 관계에서 사용된다고 보기)

\*\* ul은 기본적인 높이가 있음

(ul에 높이를 주지 않으면 li 높이만큼 li의 높이는 컨텐츠의 높이만큼 자리를 차지함)

so, 자식 요소가 float 되었을 때 부모 요소의 높이가 0이 된다.

\***\* 해결 방법 \*\***

1. 부모 요소의 height 값 설정
2. overflow: hidden 이용
3. ::befor / ::after을 이용하여 .clearfix 클래스를 만들어서 사용

→ 주로 2, 3번 사용 / height 값을 지정할 수 있는 상황과 불가한 상황이 있기 때문

각각 컨텐츠에 따라서 높이가 유동적이기 때문에 무조건  height 값을 지정해두면 나머지 요소들의 구성이 망가짐

ex. 네이버 메인 페이지를 보면 뉴스나 블로그 글들이 짧을수도 길수도 있는데 1000px이라고 지정해버리면 길어질 경우에는 다른 곳에 가서 붙어버릴 수 있음

- overflow
- hidden : 영역보다 넘치면 자르기
- scroll : 영역보다 넘치지 않아도 스크롤 창이 자동으로 생김
- auto : 영역이 넘치면 스크롤 창이 자동으로 생성이 됨(넘치지 않으면 생기지 않음)
- visible : 기본 default 값

\*\* scroll과 auto 구분하기

\*\* overflow를 지정하면 따로 height 값을 지정해주지 않아도 알아서 지정해줌

넘친 부분을 지정하려면 하위 요소의 height 값을 알아야 지정할 수 있으므로 자동으로 하위 요소의 height 값을 계산하여 상위요소에도 동일하게 height 값을 지정해줌

그러나 넘쳐나서 scroll을 해야 하거나 auto를 줬을 때 일정해지지 않음
