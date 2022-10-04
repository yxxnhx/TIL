# FLEX BOX

- **부모요소** - container
- **자식요소** - item

부모요소에 display: flex; 선언 -> 자식 요소는 수평으로 정렬이 됨(block, inline 상관없이)

부모요소의 height 값을 없애도 float처럼 사라지지 않음

flexbox를 지정하면 팽창과 축소하는 성질을 가지고 있음

부모요소의 넓이와 높이에 맞게 축소할 수 있음

## <부모 컨테이너에 설정할 수 있는 속성>

### flex-direction 메인축 설정

- row: default 값
- row-reverse : 수평정렬 역순
- column : 수직정렬
- column-reverse : 수직정렬 역순

### flex-wrap - 줄바꿈 속성

- nowrap : wrapping이 되지 않고 튀어나가게 됨
- wrap : wrapping이 되어 나머지는 밑으로 내려가게 됨
- wrap-reverse : 역순으로 떨어져나가게 만들기

## **justify-content -** 주축 기준의 배치 방법 지정

- **flex-start** : 시작점을 기준으로 배치 (default)

- **flex-end :** 끝점을 기준으로 배치

- **center** : 가운데 배치

- **space-between** - 처음, 마지막 향목을 양 끝에 붙인 후에 중앙 항목을 같은 간격으로 배치

- **spece-around** - 사이 간격 동일 지정

## align-content - 여러 줄일 때 교차축의 방향 배치 방법 지정

- flex-start

- flex-end

- center
- stretch

- space-between

- spece-around

## align-items - 교차축 정렬 지정(한 줄 일 때)

- stretch - 플랙스 항목을 확장해 교차축을 꽉 채움 default (height 값이 지정되지 않았을 때 확인 가능)

- flex-start - 교차축의 시작점을 기준으로 배치

- flex-end - 교차축의 끝점을 기준으로 배치

- center - 교차축의 중앙을 기준으로 배치

- baseline - 글자에 맞춰 배치

## <자식 아이템에 설정할 수 있는 속성>

### order 속성 - 플랙스 항목의 배치 순서 바꾸기 / 특별한 경우가 아니면 잘 사용하지 않음

→ item에 직접 정수로 적용해야 출력이 됨

```html
.wrap1>div:nth-child(1) { order: 1 } .wrap1>div:nth-child(2) { order: 0 }
.wrap1>div:nth-child(3) { order: 2 } .wrap1>div:nth-child(4) { order: 3 }
.wrap1>div:nth-child(5) { order: 4 }
```

### flex-grow - 플랙스 항목의 너비를 얼마나 늘릴지(default 값 0)

```html
.wrap1{ display: flex; width: 600px; height: 40px; background-color: #666; }
.wrap>div { width: 100px; } .wrap1>div:nth-child(1){ background-color: red;
flex-grow: 2; } .wrap1>div:nth-child(2){ background-color: green; flex-grow: 1;
} .wrap1>div:nth-child(3){ background-color: blue; flex-grow: 1; }
```

전체 공간을 비율로 나눠서 각 div의 width값을 지정해도 강제로 너비가 지정됨(width 값을 지정해도 소용없음)

grow값이 확장되었을 때 어떤 비율로 해당 공간이 채워질지 지정하는 것

default값이 0이므로 그 외의 정수들로 지정해주면 됨

### flex-shrink - 플랙스 항목의 너비를 얼마나 줄일지(default 값 1)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/07f37b2d-8eac-4257-9965-1af24a7e0508/Untitled.png)

→ 부모를 600px, 자식을 각각 300px씩 width 값을 따로 지정을 해도 자동으로 1:1:1로 출력이 됨

```html
.wrap2 { display: flex; width: 600px; height: 40px; background-color: #666;
margin-top: 50px; } .wrap2>div { width: 300px; } .wrap2>div:nth-child(1){
background-color: red; flex-shrink: 2; } .wrap2>div:nth-child(2){
background-color: green; flex-shrink: 1; } .wrap2>div:nth-child(3){
background-color: blue; flex-shrink: 1; }
```

→ shrink를 2로 준 것이 더 작게 출력이 됨

Why? 앞에 각 자식들을 300px씩 지정을 하였으니 300 - ((-300/4) \* 지정한 shrink 값)

                                                                       → 기본 width 값 + (-넘어간 width 값/지정한 정수 총 값)

    1번째 자식은 2만큼 축소, 2, 3번째 자식은 1만큼 축소가 되었기 때문에

- 1번 : width 150px

- 2, 3번 : width 225px

### flex-basis - 기본 너비값 지정 (default 값 auto)

```html
<div class="wrap5">
  <div class="box">박스 박스 1</div>
  <div class="box">박스 박스 2</div>
  <div class="box">박스 3</div>
</div>
```

```html
.wrap5 { display: flex; width: 600px; height: 40px; background-color: #666;
margin-top: 50px; } .wrap5>div { flex-basis: auto; } .wrap5>div:nth-child(1){
background-color: red; } .wrap5>div:nth-child(2){ background-color: green; }
.wrap5>div:nth-child(3){ background-color: blue; }
```

→ auto 설정 시 컨텐츠 값만큼 차지하게 됨

```html
.wrap5 { display: flex; width: 600px; height: 40px; background-color: #666;
margin-top: 50px; } .wrap5>div { flex-basis: 400px; } .wrap5>div:nth-child(1){
background-color: red; } .wrap5>div:nth-child(2){ background-color: green; }
.wrap5>div:nth-child(3){ background-color: blue; }
```

→ width를 지정해도 동일하게 부여하게 됨

### initial

### auto

### 축약 - flex : flex-grow flex-shrink flex-basis;

```html
flex: grow shrink basis;
```

```html
flex: 1 1 50px
```

→ glow는 1, shrink는 1, basis는 50px

```html
flex: 0 1 100px
```

→ glow는 0(none), shrink는 1, basis는 50px

```html
flex: 1
```

→ glow는 1, shrink는 1, basis는 0

```html
flex: 50px
```

→ glow는 1, shrink는 1, basis는 50px

```html
flex: auto
```

→ glow는 1, shrink는 1, basis는 auto

```html
flex: initial
```

→ glow는 0, shrink는 1, basis는 auto

```html
flex: none
```

→ glow는 0, shrink는 0, basis는 auto

```html
flex: 1 2
```

→ glow는 1, shrink는 2, basis는 0

```html
flex: 100%;
```

→ shrink 1 grow 1 basis 100

### align-self - 개별적 배치

```html
.container { width: 400px; height: 400px; border: 5px solid red;
background-color: #ddd; } .container1 { display: flex; flex-direction: row;
justify-content: space-between; align-items: center; } .container>div.box1 {
align-self: flex-start; } .container>div.box2{ align-self: flex-start; }
.container>div.box3 { align-self: flex-end; }
```

- flex-start
- flex-end
- stratch
- center
- baselind
- auto(기본값)

→ auto로 선언할 시에는 부모에서 지정한 flex 값을 그대로 상속받아 출력됨

그 외에 개별적으로 지정하고 싶을 때에는 align-self로 지정하면 된다.

+) flex를 연습할 수 있는 사이트
