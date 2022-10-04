# GRID

## Container (부모요소) 속성

- grid

→ block 속성처럼 한 줄을 모두 차지함

- inline-grid

→ 인라인 속성처럼 컨텐츠의 크기만큼만 크기를 가짐

### <행과 열 길이 조절>

- grid-template-columns

```html
.container { border: 5px solid #000; display: grid; grid-template-columns: auto
auto auto; }
```

→ 나누고 싶은 만큼 auto를 넣으면 해당 갯수만큼 등분되어서 나타남

```html
.container { border: 5px solid #000; display: grid; grid-template-columns: 1fr
2fr 1fr; }
```

→ fr: fraction - 비율적으로 n등분 flex-gird와 비슷한 개념

→ auto는 세 개까지만 가능함

- grid-template-rows

```html
.container { border: 5px solid #000; display: grid; grid-template-columns: 1fr
2fr 1fr; grid-template-rows: 100px 200px; }
```

→ 값을 주지 않거나 auto로 설정할 경우 컨텐츠의 크기만큼 높이를 차지함

### 축약형

```html
gird: row / colum; gird: 100px 50px / 1fr 2fr 1fr;
```

### <간격 조절>

- grid-column-gap(column-gap)

→ 수직 여백

- grid-row-gap(row-gap)

→ 수평 여백

### 축약형

```html
gird-gap: row / colum; gird-gap: 100px / 60px; gird-gap: 100px;
```

→ 값을 하나만 지정할 경우, 수직 수평 모두 동일한 간격으로 떨어짐

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/04efed67-3eb1-49f2-8fe6-2bf9e094600d/Untitled.png)

- **justify-items**

- start : defult

- end : 하단 정렬

- cener : 가운데 정렬

- stretch : 꽉 채움(자식 요소에 width 값이 없어야 함)

- **align-items**

- start : defult

- end : 하단 정렬

- cener : 가운데 정렬

- stretch : 꽉 채움(자식 요소에 height 값이 없어야 함)

```html
.container1 { justify-content: center; grid-template-columns: auto auto auto; }
.container2 { justify-content: center; grid-template-columns: 1fr 1fr 1fr; }
```

→ fr 전체 넓이에서 n분의 1 auto로 하면 자식요소의 너비만 가져와서 템플릿 자체가 중앙에 모이게 할 수 있음

- justify-content
- align-content

## Item (자식요소) 속성

- justify-self : 하나하나 따로 움직이기
- align-self : 하나하나 따로 움직이기

- start

- end

- center

- gird-column-start
- gird-column-end

→ column들을 어디서부터 어디까지 합칠 것인지 지정하는 것

위와 같이 1번 박스가 2번 박스 위치까지 확장시키고 싶을 때,

축이 1, 2, 3, 4로 나뉘는 것을 확인할 수 있음

```html
.container .item { background-color: red; grid-column-start: 1; grid-column-end:
3; }
```

→ column라인 기준으로 start와 end 지점 모두 지정해주는 방법

```html
.container .item { background-color: red; grid-column: 1 / span 2; }
```

→ 축약으로 지정하는 방법

grid-column : 시작하는 칸 / 끝나는 칸

```jsx
.parent{
	border:1px solid #000;
	width: 900px;
	height: 200px;
	background-color: aqua;
	display: grid;
	grid-template-columns: auto auto;
	justify-items: center;
	align-items: center;
}
```

```jsx
.parent div{
	color: #fff;
	font-size: 30px;
	background-color: blue;
	/*  width: 100px;
	height: 100px;*/
	text-align: center;
}

.parent div:nth-child(2){
	align-self:end; → 얘만 올려주거나 내려주려면 자식요소의 속성에서 변경해주기
} → 컬럼 안에서 자식요소의 넓이 높이를 꽉 채울 때
```

```jsx
.container{
	border:1px solid #000;
	height: 300px;
	background-color: rgb(229, 204, 219);
	display: grid;
	grid-template-columns: auto auto auto;
	grid-gap: 10px;
	margin: 50px auto;
}

.container div{
	color: #fff;
	text-align: center;
	font-size: 30px;
	background-color: rgb(233, 44, 211);
}

.container div.item{
	background-color: red;
	/* grid-column-start: 1;
	grid-column-end: 3;*/
	grid-column: 1 / span 2; → 시작하는 라인부터 몇개의 column이 합쳐질지 (span이 몇개의 column이 합쳐질지임)
	→ column라인을 기준으로 시작위치 = 첫번째 칸에서 2개 만큼 확장
}
```

```jsx
.container2{
	border:3px solid #000;
	height: 300px;
	background-color: rgb(241, 238, 149);
	display: grid;
	grid-template-columns: auto auto auto auto auto auto;
	grid-template-rows: auto auto auto;
	grid-gap: 10px;
	margin: 50px auto;
}

.container2>div{
	color: #000;
	border: 1px solid #000;
	text-align: center;
	font-size: 30px;
}

.container2>div:nth-child(8){
	background-color: red;
	/* grid-row: 1 /span 4;
	grid-column: 2/ span 4; */
	grid-area: 1 /2 / 5 /6;
→  시작 가로 / 시작 세로/ 끝나는 부분의 가로 / 끝나는 부분의 세로 → 선부터 1로 시작한다고 생각
}
```

### **알아둘 것들**

- grid-area 계산할 때 span을 쓰면 칸으로 계산하고 span 없이 셀 때는 선으로 계산하기
- grid-area은 /(슬래쉬)로 구분지어 grid-row-start, grid-column-start, grid-row-end, grid-column-end순으로 입력
- 포지션 없이 z-index 사용해서 올릴 수 있음
- row의 순서 바꿀 때는 grid-row
- 한 셀의 순서 바꿀 때는 order를 사용
- grid 연습할 수 있는 사이트 : grid Garden
