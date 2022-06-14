# TABLE TAG

표 구현을 위한 태그 / 기본 속성은 각 셀별로 떨어져 있음 

```html
<caption>표연습</caption>
	<tbody> -> tbody만 있을 경우 생략 가능	
	<tr>	
			<td>1</td>			
			<td>2</td>		
		</tr>		
		<tr>		
			<td>1</td>			
			<td>2</td>		
		</tr>		
		<tr>		
			<td>1</td>		
			<td>2</td>		
		</tr>
	</tbody>
</table>
```

### table, tr, td

각 칸의 너비를 지정해주지 않으면 총 너비의 n분의 1로 지정

사이즈를 정하지 않으면 데이터의 크기에 따라서 셀의 크기가 달라짐

### caption

table 안에 caption 태그로 해당 표의 설명을 넣어야 함

```html
<table class="bbs1">
```

**thead나 tfoot은 표에서 한 번도 안 나오거나, 한 번만 나와야 함. tfoot은 thead 보다 뒤에 위치해야 함**

- **thead**: 제목으로 구성된 열들을 가진 행을 표시
- **th:** 제목으로 구성된 각 열을 표시
- **tbody**: 표의 내용을 나타내는 행들을 표시
    
    → tbody만 있는 표일 경우 생략 가능
    
- **tr**: 표의 내용을 나타내는 각 행들을 표시
- **td**: 표의 내용을 나타내는 각 열들을 표시
- **tfoot** - head 뒤, tbody 뒤 아무 곳이나 지정 가능
- **thead** - 각 표의 제목을 표시
    
    → thead에서는 th로 각 열을 지정
    

### scope

접근성을 위해서는 scope="col”를 넣는 것이 좋음

**row** - 열 / **col** - 행

### 셀/행 병합

| 속성 | 설명 | 주요 값 |
| --- | --- | --- |
| colspan | td 태그에서 사용, 열을 확장 | 확장할 열의 숫자 |
| rowspan | td 태그에서 사용, 행을 확장 | 확장할 행의 숫자 |

```html
<table>
    <tr>
        <td colspan="2">열의 확장</td>
    </tr>
    <tr>
        <td rowspan="2">행의 확장</td>
        <td>열1</td>
    </tr>
    <tr>
        <td>열2</td>
    </tr>
</table>
```

**셀병합을 하고 싶을 때**

```html
<tr>
	<td colspan=2>1</td>
	<td>2</td>
</tr>
```

열병합을 하고 싶을 때는 같은 tr 내에서 td를 병합하고 열 갯수만큼 지우고 colspan을 부여하면됨

행병합을 하고 싶을 때는 해당 tr 내에서 td를 병합할 행 갯수만큼 지우고 rowspan을 부여하기

### colgroup

각 행별로 그룹을 지어 사용할 때 caption 밑에서 colgroup 선언 후 필요한 갯수만큼 col 생성

```html
<colgroup>
	<col span="2" style="background-color: #ccc; width: 100px;">
	→ col을 2개를 묶어서 지정하고 싶을 때 span 사용
	<col>
	<col style="background-color: #ccc; width: 50px";>
	<col>
	<col>
</colgroup>
```

→ 이렇게 colgroup을 사용하면 행별로 너비/색상 등 지정 가능

**한줄로 하나의 표처럼 보이고 싶을 때**

```html
border-collapse: collapse;
```

→ 대개 reset 파일에 지정해둠