# LIST TAG

# ★ ul, ol, li ★

리스트 관련 태그로 모던 웹에서 많이 사용됨

## ul(unordered list) / li(list item)

순서가 필요하지 않은 목록

** ul 안에 자식 태그는 li만 올 수 있음 그 외 다른 태그 불가

** li 태그 안에는 다른 태그들이 들어와도 상관없음(ol이 다시 들어와도 가능)

# ol(ordered list)  / li(list item)

순서가 필요한 목록

** ol 안에 자식 태그는 li만 올 수 있음 그 외 다른 태그 불가

** li 태그 안에는 다른 태그들이 들어와도 상관없음

```html
<ul>
	<li>1일차
		<ol>		
			<li>해녀박물관</li>			
			<li>낚시체험</li>			
		</ol>	
	</li>	
	<li>2일차	
		<ol>		
			<li>용눈이오름</li>			
			<li>만장굴</li>			
			<li>카약체험</li>		
		</ol>	
	</li>
</ul>
```

→ 가능한 태그

## dl - 정의가 있는 설명태그

## dl(description list) - dt, dd 필수 요소

- **dt** 태그 제목
- **dd** 태그 설명

** dt 하나에 dd가 여러개일 수 있음

** dl 하나에 dt가 여러개일 수 있음(목록을 나눠서 설명)

** 상황에 따라서 dl을 각각 끊어서 사용도 가능

** dd에 ul, ol 설정 가능

*** 목록 태그를 사용하면 자동으로 들여쓰기가 들어가서 스타일이 적용됨

### a, href - 링크

```html
<a href="#" target="_blank">La Mer</a>
```

| 속성 | 설명 | 주요 값 |
| --- | --- | --- |
| href | 하이퍼링크 URL | URL 또는 tel:전화번호, mailto: 메일주소 ( 하이퍼링크 외 전화번호, 이메일 주소 등 가능 |
| target | 링크된 URL 이동 방법 지정 | • _blank : 새창으로 열기
• _self : 현재 창에서 열기 |

** 주소창 전체: location

** 주소창: href

** 속성과 속성 사이는 한 칸 띄어쓰기