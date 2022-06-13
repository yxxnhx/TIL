# FORM TAG

사용자가 입력을 받는 태그로 input 태그 등과 같이 사용됨

### 주요 속성

| 속성 | 설명 | 주요 값 |
| --- | --- | --- |
| action | 사용자가 입력 받은 데이터를 처리할 URL |  |
| method | HTTP method | post(POST 메서드) 또는 get(GET 메서드) |
| target | 링크된 URL 이동 방법 지정 | _self: 현재 브라우저에 띄움
_blank: 새로운 브라우저 탭에 씌움
(또는 브라우저 설정에 따라 새로운 브라우저 창) |

## <input>

사용자 데이터를 입력받는 주요 태그

**text email checkbox radio file reset submit - 가장 많이 사용**

주요 속성 외에도 다양한 속성을 가지고 있다.

| 속성 | 설명 | 주요 값 |
| --- | --- | --- |
| type | 사용자 입력 받는 인터페이스 설정 | button, checkbox, color, email, radio, text 등
→ 이외에도 다양함 |
| mexlength | 사용자 입력 문자열 최대 길이 | 정수 |
| minlength | 사용자 입력 문자열 최소 길이 | 정수 |
| autofocus | 페이지 로드 시, 해당 사용자 입력창에 커서가 자동으로 놓이도록 하는 설정 | autofocus |
| autocomplete | 자동 완성 기능 설정
→ 기존에 작성한 내용이 뜨게 함 | on 또는 off |

```html
<form action="login.html" method="post">
	ID <input type="text" name="ID">
	PW <input type="text" name="PW">
</form>
```

### method - 동작 지정

**get** 사용자가 입력한 내용 그대로

**post** 사용자가 입력한 내용 드러나지 않음(id/pass)

- **name** - 폼의 이름 지정 (*주로 name만 지정)
- **action** - 서버 상의 프로그램 지정
- **target** - action에서 지정한 스크립트 파일 열릴 위치 지정
- **autocomplete** - 자동 완성 기능

```html
<form action=“register.php” autocomplete=“on/off”
```

- **label -** label의 for와 input의 id가 일치해야 함
- **fieldset** - 비슷한 양식끼리 묶어주는 것 / 웹접근성 상을 위해 넣음
→ 웹접근성을 위하여 fieldset, legend, label 필수 지정

```html
<form name="frm1">
        <fieldset>
            <legend>회원가입정보
                <ul>
                    <li>
                        <label for="userId">아이디</label> 
										     <input type="text" id="userId"
			→ label의 for과 input의 id는 일치해야 함
			  placeholder="아이디는 이메일 형식입니다" maxlength="20">
			→ 창에 뜰 안내						    → 최대 글자 수
                    </li>
                </ul>
            </legend>
        </fieldset>
    </form>
```

+) label을 보이고 싶지 않다면 css에서 display: none; 처리 가능

### placeholder

입력 창에 뜨는 안내 문구

```html
<li><input type="datetime"></li>
<li><input type="date"></li>
<li><input type="datetime-local"></li>
<li><input type="month"></li>
<li><input type="week"></li>
<li><input type="time"></li>
<li><input type="number" min="0" max="3"></li>
<li><input type="range"></li>
<li><input type="color"></li>
```

→ 위 태그들은 익스플로어에서는 지원이 되지 않는 태그

### radio

```html
<input type="radio" name="subject"><label for="math" value="math">수학</label>
<input type="radio" name="subject"><label for="eng" value="eng">영어</label>
<input type="radio" name="subject"><label for="ko" value="ko">국어</label>
```

### checkbox

```html
<input type="checkbox" id="movie"><label for="movie">영화</label>
<input type="checkbox" id="music"><label for="music">음악</label>
<input type="checkbox" id="book" checked><label for="book">독서</label>
```

### select

```html
<select name="" id="">
   <option value="10">10개</option>
   <option value="20">20개</option>
   <option value="30">30개</option>
   <option value="40">40개</option>
   <option value="50">50개</option>
</select>
```

select의 값은 option으로 지정

```html
<select>
  <optgroup label="이과">
      <option value="math">수학</option>
      <option value="ko">국어</option>
      <option value="eng">영어</option>
  </optgroup>
  <optgroup label="문과">
      <option value="math">수학</option>
      <option value="ko">국어</option>
      <option value="eng">영어</option>
  </optgroup>
</select>
```

→ 선택 값을 각각의 그룹으로 묶고 싶을 때 사용

```html
<input type="submit" value="제출하기">
```

form 태그에 지정한 action 경로에 제출

```html
<input type="reset" value="취소하기">
```

기입하거나 선택한 내용 초기화

```html
<input type="button" value="버튼">
```

그냥 일반 태그

```html
<button>클릭</button>
```

**default 값 type=submit**

* 필수 지정값을 넣고 싶을 때 - required 지정해주면 됨

```html
<label for="userMail">이메일</label><input type="email" id="userMail" required>
```

* 읽기만 하게 하고 싶을 때 - **readonly placeholder**

```html
<input type="text" readonly placeholder="이메일 필수 입력">
```

* **autofocus** - 입력 커서 자동으로 표시

```html
<input type="submit" value="제출하기" autofocus required>
```

**CSS에서 속성 선택하는 방법**
```html
header .search-box input[type="text"]
```
