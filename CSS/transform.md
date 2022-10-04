# CSS transform

요소에 이동, 회전, 크기 조절, 기울이기 효과를 부여하는 함수를 제공한다

- 애니메이션 효과를 제공하지 않기 때문에 애니메이션 효과가 필요할 경우 트랜지션 or 애니메이션과 함께 사용하면 된다

### CSS transform 사용법

요소의 회전, 크기 조절, 기울이기, 이동 효과를 부여하는 함수(function)를 제공하며, 다음과 같은 형태로 설정할 수 있음

```
/* 쉼표없이 효과를 적용할 순서대로 나열 */
transform: func1 func2 ...

```

css transform 함수로 요소 이동 및 변형 시 다음과 같이 요소 변형을 설정할 수 있다.

### **주요 transform 함수**

| function              | 설명                                                 | 단위                                       |
| --------------------- | ---------------------------------------------------- | ------------------------------------------ |
| translate(x,y)        | 요소 위치를 X축으로 x만큼, Y축으로 y만큼 이동        | px, %, em 등                               |
| translateX(n)         | 요소 위치를 X축으로 x만큼 이동                       | px, %, em 등                               |
| translateY(n)         | 요소 위치를 Y축으로 y만큼 이동                       | px, %, em 등                               |
| scale(x,y)            | 요소 크기를 X축으로 x배, Y축으로 y배 확대 또는 축소  | 0과 양수(1보다 크면 확대, 0~1 사이면 축소) |
| scaleX(n)             | 요소 크기를 X축으로 x배 확대 또는 축소               | 0과 양수(1보다 크면 확대, 0~1 사이면 축소) |
| scaleY(n)             | 요소 크기를 Y축으로 y배 확대 또는 축소               | 0과 양수(1보다 크면 확대, 0~1 사이면 축소) |
| skew(x-angle,y-angle) | 요소를 X축으로 x 각도만큼, Y축으로 y 각도만큼 기울임 | +/- 각도(deg)                              |
| skewX(x-angle)        | 요소를 X축으로 x 각도만큼 기울임                     | +/- 각도(deg)                              |
| skewY(y-angle)        | 요소를 Y축으로 y 각도만큼 기울임                     | +/- 각도(deg)                              |
| rotate(angle)         | 요소를 angle만큼 회전                                | +/- 각도(deg)                              |

<aside>
💡 3d는 translate3d, scale3d, rotate3d

</aside>

```jsx
  <style>
    .tmp {
      margin-top: 100px;
    }
    .box {
      width: 100px;
      height: 100px;
      margin: 20px auto;
      border-radius: 5px;
    }
    .box1 {
      background: palegreen;
    }
    .box2 {
      background: paleturquoise;
    }
    .box3 {
      background: palevioletred;
    }
    .box4 {
      background: peru;
    }
    .translate:hover {
      transition: transform 1s;
      transform: translate(30px, -30px);
    }
    .scale:hover {
      transition: transform 1s;
      transform: scale(2, 2);
    }
    .skew:hover {
      transition: transform 1s;
      transform: skew(40deg, 60deg);
    }
    .rotate:hover {
      transition: transform 1s;
      transform: rotate(45deg);
    }
  </style>
</head>
<body>
  <div class="tmp"></div>
  <div class="box box1 translate">translate</div>
  <div class="box box2 scale">scale</div>
  <div class="box box3 skew">skew</div>
  <div class="box box4 rotate">rotate</div>
</body>
```

### **transform-origin 프로퍼티**

- 주요 transfrom 함수들의 동작은 기본적으로 해당 요소의 중심(50%, 50%)을 기준으로 동작
- 요소의 기준점을 변경하려면 `transform-origin` property를 사용하면 됨
- 이동은 기준점을 변경해도 일정 거리만큼 이동하므로 의미가 없다.

```jsx
transform-origin: x축 | y축 | z축
```

각 축의 값은 %, px, left, center, right, length, 등이 가능0, 0은 top left, 100% 100%는 bottom right와 같다

```jsx
  <style>
    .box {
      width: 100px;
      height: 100px;
      position: absolute;
      top: 50%;
      left: 50%;
      border-radius: 5px;
      background: plum;
    }
    .box1 {
      z-index: 1;
    }
    .box2 {
      z-index: 0;
    }
    .rotate:hover {
      transform-origin: 100% 100%;
      background: rgba(0, 0, 0, 0.2);
      animation: turn 5s infinite;
    }
    @keyframes turn {
      50% {
        transform: skew(180deg, 180deg);
      }
      to {
        transform: skew(360deg, 360deg);
      }
    }
  </style>
</head>
<body>
  <div class="box box1 rotate">up</div>
  <div class="box box2">down</div>
</body>
```

→ bottom, right 기준으로 애니메이션 발생
