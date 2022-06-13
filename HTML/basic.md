# 기본 구조

```html
<!DOCTYPE html>
<html lang="ko">
→ html 파일이 시작된다

<head>
→ 문서의 기본적인 정보
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>문서의 제목</title>
</head>
<body>
 → 페이지를 구성하는 부분
</body>
</html>
→ 문서가 종료된다
```

→ emit으로 ! / html:5로 입력 가능

- html - 문서의 구조
- tag - 요소 element
    
             속성 = 값 // attribute=“value”
    

               html에 필수적으로 부여해야 하는 속성

- 파일명 - 한글x 띄어쓰기x 굳이 띄어쓰고 싶다면 -, _으로 구분할 것

## DOCTYPE

```html
<!DOCTYPE html> 
```

→ HTML 문서는 최상단에 항상 위 태그를 사용해야 한다. 해당 문서를 브라우저가 다르게 랜더링하지 않도록 하기 위해 HTML 문서임을 알려 주는 특수한 태그 / 해당 버전에 맞춰서 랜더링이 시작됨

(해당 부분은 html 5 버전임, 대소문자 상관없음)

## HTML

```html
<html lang="ko">
```

문서 범위 설정 태그로 html 문서는 해당 태그로 오픈한 후, 적성되어야 하고, 맨 마지막에 해당 태그로 닫아야 함

현재 작성하고 있는 문서의 언어를 적어주면 됨

| 속성 | 설명 | 주요 값 |
| --- | --- | --- |
| lang | 문서 언어 설정(ISO 639-1) | ko, en, ja 등(한국어, 영어, 일본어) |

## HEAD, BODY

```html
<head>
</head>
<body>
</body>
```

html 문서는 태그와 태그로 이루어짐

### HEAD

head 태그 안에는 html 문서 전체를 대표하거나, html 문서 전체에서 필요한 데이터를 넣음

**head 태그 안에서 사용되는 주요 태그**

- title: html 문서 제목, 주로 해당 웹페이지를 보여 주는 웹브라우저 상단에 표시

     예) 웹브라우저 탭 상에 표시

- meta: html 문서 전체를 대표하는 정보를 넣어 사용

### BODY

body 태그 안에는 html 문서에 표시되는 내용을 넣음

| 속성 | 설명 | 주요 값 |
| --- | --- | --- |
| lang | 문서 언어 설정(ISO 639-1) | ko, en, ja 등(한국어, 영어, 일본어) |

## META

```html
<meta>
```

문서 전반에 걸친 청보를 표시하기 위한 설정 / 기본적인 요소의 데이터를 설명해주는 것

| 속성 | 설명 | 주요 값 |
| --- | --- | --- |
| charset | 문자 인코딩 설정 | utf-8, euc-kr 등 (유니코드, 한국어) |
| name | 메타 정보 이름 | author: 저자 이름
description: HTML 문서 설명 등 |

### 주요 meta name 값 (가장 일반적으로 많이 사용)

```html
<meta name=“keyword” content=“html5, 웹표준”>
<mata name=“description” content=“html5를 통해 웹 표준 공부하기”>
<meta name=“author” content=“kyunghee ko”>
```

```html
<meta name=“keyword” content=“html5, 웹표준”>
```

→ 해당 문서의 키워드

```html
<mata name=“description” content=“html5를 통해 웹 표준 공부하기”>
```

→ 해당 문서의 설명

```html
<meta name=“author” content=“kyunghee ko”>
```

→ 해당 문서의 소유자 또는 제작자

```html
<meta charset=‘utf-8’>
```

- charset(character set): 글자

→ 이 문서의 글자는 다국어가 지원되게 합니다

### 호환성 관련 태그

```html
<meta http-equiv="X-UA-Compatible" content="IE=edge">
```

인터넷 익스플로러(IE)에서 최신 표준 모드로 렌더링되도록 하는 설정

인터넷 익스플로러 호환성 이슈로 기본적으로 html 문서에 포함되는 것이 좋음

→ 모든 웹페이지가 동일하게 보이기 위해

### 반응형 웹 관련 태그

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

**viewport(뷰포트)란?**

현재 화면에 보여지고 있는 직사각형 영역, 웹브라우저 상에서는 현재 창에서 문서를 볼 수 있는 전체 화면을 의미

**meta viewport 설정**

초기 viewport에 대한 설멍을 의미 ★기본세팅 필수★

| 속성 | 설명 | 주요 값 |
| --- | --- | --- |
| width | 초기 뷰포트 너비 설정 | device-width 또는 양의 정수(디바이스 너비 또는 특정 너비 |
| user-scalable | 웹페이지 확대 허용 여부 설정 | no 또는 yes
(기본값은 yes이지만, no로 설정하는 경우가 많음. no일 경우, 사용자가 웹페이지를 확대할 수 없으나, 브라우저 설정에 따라 무시할 수 있음) |
| initial-scale | 디바이스 너비와 뷰포트 너비 비율 설정 | 0.0과 10.0 사이의 수 (주로 1.0을 많이 사용함) |
| maximum-scale | 최대 확대 비율 설정 | 0.0과 10.0 사이의 수 (주로 1.0을 많이 사용함) |
| minimum-scale | 최소 확대 비율 설정 | 0.0과 10.0 사이의 수 (주로 1.0을 많이 사용함) |

## LINK

```html
<link rel="stylesheet" href="style.css">
<link rel="icon" href="favicon.png">
```

html 문서에 필요한 외부 데이터를 가져오기 위해 사용

주로 css 파일과 아이콘 파일을 가져오기 위해 사용

| 속성 | 설명 | 주요 값 |
| --- | --- | --- |
| rel | html 문서와 외부 데이터와의 관계 표시 | stylesheet, icon 등(css 파일, 아이콘 파일 |
| href | 외부 데이터 파일 위치 지정 | 파일 경로(상대 경로 또는 절대 경로로 설정 |

+) favicon - 웹사이트 로고

### 상대 경로

- 상대경로 : 현재 만들고 있는 문서 기준으로 연결할 문서나 이미지의 위치 지정
    - 같은 위치 : 같은 디렉토리 상에 위치함(같은 폴더 내에 위치) - 파일명.확장자 = ./파일명.확장자
    - 위에 위치 : 위 디렉토리에 위치함 - ../파일명.확장자
    - 아래 위치 : 아래 디렉토리에 위치함 - 폴더명/파일명.확장자
        
        → 만약 아래아래를 원한다면 하위폴더명/그다음 폴더명/파일명.확장자
        
- 현재 디렉토리 : ./ (생략 가능)
- 절대경로 : 웹서버의 주소로 연결(http 혹은 부트)

## STYLE

css 코드를 html 문서 내에서 작성할 때 사용

css코드는 파일 형태로 link 태그를 사용해서 가져오거나, style 태그를 사용해 직접 html 문서에 넣을 수 있는 두 가지 방법으로 적용 가능

```html
<style>
        #test {
            filter: blur(100px);
        }
        
        #test:hover {
            filter: blur(0) brightness(-150%) drop-shadow(3px 3px 5px #e0e0e0);
            transition: all 0.35s;
        }

        #test2:hover {
            filter: hue-rotate(170deg) blur(30px);
        }
       
    </style>
```

## SCRIPT

java script 코드를 html 문서 내에서 작성할 때 사용

 java script 코드는 파일 형태로 link 태그를 사용해서 가져오거나, script 태그를 사용해 직접 html 문서에 넣을 수 있는 두 가지 방법으로 적용 가능

**가장 일반적인 사용법**

```html
<script src="js/main.js"></script>
```

기존에는 다음과 같이 작성하였으나

```html
<script type="js/main.js"></script>
```

html5에서는 script만 쓰면 된다

```html
<script>
	function check_password()
</script>
```

## div

divisiondml 약자로, html 문서의 특정 부분을 지정하는데 사용 (화면에 표시가 달라지는 부분은 없는 특이한 태그

모던 웹에서 css 또는 javascript로 html 문서의 특정 부분을 제어할 때 사용

            → class  id
