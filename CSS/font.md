# CSS font

## **font-size 프로퍼티**

문자 크기 설정

- `medium` : 웹 브라우저의 기본 글자 크기(16px)(default 값)
- `xx-small`, `x-small`, `small`, `large`, `x-large`, `xx-large` : medium에 대한 상대적인 크기 설정 값
- `smaller`, `larger` : 부모 요소의 글자 크기에 대한 상대적인 글자 크기 설정값
- `length` : px, %, em, rem 등의 CSS 단위를 사용한 글자 크기 설정값

```jsx
<p>default font size</p>
<p style="font-size: medium">medium font size</p>
<p style="font-size: 16px">16px font size</p>
<p style="font-size: larger">larger font size</p>
<p style="font-size: xx-small">xx-small font size</p>
```

## **font-family 프로퍼티**

문자 폰트 설정

```jsx
font-family: 첫 번째 폰트, 두 번째 폰트, 세 번째 폰트... ;
```

- 폰트는 사용자의 PC에 설치되어 있어야 함. 맨 앞의 폰트를 먼저 찾고 없다면 두 번째 폰트, 또 없으면 세 번째 폰트순으로 설정을 시도함.
- 일반적으로 세 가지 폰트를 설정하는데, 마지막 폰트는 어느 PC에서 있는 generic-family font를 설정함. generic-family font는 아래와 같다.

```jsx
<font style="font-family: serif">serif</font><br />
<font style="font-family: sans-serif">sans-serif</font><br />
<font style="font-family: monospace">monospace</font><br />
<font style="font-family: cursive">cursive</font><br />
<font style="font-family: fantasy">fantasy</font><br />
<font style="font-family: system-ui">system-ui</font>
```

## **Web Font 사용법**

- 폰트는 사용자 PC에 해당 폰트가 설치되어 있어야 해당 폰트로 웹페이지가 표시됨
- 웹폰트를 사용하면 사용자 PC에 해당 폰트가 설치되어 있지 않더라도, 웹브라우저에서 해당 폰트를 다운받아서 웹페이지가 표시됨(PC에 설치되는 폰트 파일보다 경량화됨)

**대표적으로 Google Font가 있다.**

[Google Fonts](https://fonts.google.com/)

- link 태그 복사 / font-family 설정에서 폰트 이름 확인하기
  ```jsx
    <link href="https://fonts.googleapis.com/css2?family=Fredoka+One&display=swap" rel="stylesheet" />
  </head>
  <body>
    Web Font<br>
    <font style="font-family: 'Fredoka One', cursive">Web Font</font>
  </body>
  ```

## **font-style 프로퍼티**

글자 모양 설정

- `normal` : 보통 글자 모양
- `italic` : 기울임 글자 모양(필기체 느낌)
- `oblique` : 기울임 글자 모양(보통에서 기울이기만 한 것)

```jsx
<font style="font-size: 20px">기본 TEST</font><br/>
<font style="font-size: 20px; font-style: normal">noraml TEST</font><br/>
<font style="font-size: 20px; font-style: italic">italic TEST</font><br/>
<font style="font-size: 20px; font-style: oblique">oblique TEST</font><br/>
```

## **font-weight 프로퍼티**

글자 굵기 설정

- number : `100` ~ `900` (폰트에서 지원해야 함)
- `normal` : 보통 굵기, 400과 같음
- `bold` : 굵은 굵기, 700과 같음
- `bolder` : 상속된 값 보다 굵은 굵기
- `lighter` : 상속된 값 보다 얇은 굵기

```jsx
    <font style="font-weight: normal">font weight noraml</font><br/>
    <font style="font-weight: bold">font weight bold</font><br/>
    <font style="font-weight: 100">font weight 100</font><br/>
    <font style="font-weight: 200">font weight 200</font><br/>
    <font style="font-weight: 400">font weight 400</font><br/>
    <font style="font-weight: 700">font weight 700</font><br/>
    <font style="font-weight: 900">font weight 900</font><br/>
    <font style="font-weight: bolder">font weight bolder</font><br/>
    <font style="font-weight: lighter">font weight lighter</font>
```

## **font-variant 프로퍼티**

소문자를 소문자 크기의 대문자로 바꾸는 설정

- `normal` : 소문자 그대로 표시
- `small-caps` : 소문자를 소문자 크기의 대문자로 변경

```jsx
<font>variant test</font><br/>
<font style="font-variant: normal">variant normal test</font><br/>
<font style="font-variant: small-caps">variant small-caps test</font>
```

## **line-height 프로퍼티**

문단과 문단 사이 간격(margin) 설정

- `normal` : 웹브라우저에서 정한 기본값. 1.2
- length : px, em, rem, % 등 CSS 단위로 설정
- number : 글자 크기의 몇 배로 설정

```jsx
<p>테스트</p>
<p style="line-height: normal">normal 테스트 normal 테스트 normal 테스트 normal 테스트</p>
<p style="line-height: 1.2">
	1.2 테스트 1.2 테스트 1.2 테스트 1.2 테스트 1.2 테스트 1.2 테스트 1.2 테스트 1.2 테스트 1.2 테스트 1.2 테스트 1.2 테스트 1.2 테스트 1.2 테	 스트 1.2 테스트 1.2 테스트 1.2 테스트 1.2 테스트 1.2 테스트 1.2 테스트
</p>
<p style="line-height: 30px">
	30px 테스트 30px 테스트 30px 테스트 30px 테스트 30px 테스트 30px 테스트 30px 테스트 30px 테스트 30px 테스트 30px 테스트 30px 테스트 30px 	테스트 30px 테스트 30px 테스트 30px 테스트 30px 테스트 30px 테스트
</p>
<p style="line-height: 40%">
   	40%테스트 40%테스트 40%테스트 40%테스트 40%테스트 40%테스트 40%테스트 40%테스트 40%테스트 40%테스트 40%테스트 40%테스트 40%테스트 40%테스트
	40%테스트 40%테스트 40%테스트 40%테스트 40%테스트 40%테스트
</p>
<p style="line-height: 5">
	5테스트 5테스트 5테스트 5테스트 5테스트 5테스트 5테스트 5테스트 5테스트 5테스트 5테스트 5테스트 5테스트 5테스트 5테스트 5테스트 5테스트 5테스트 5	   테스트 5테스트 5테스트 5테스트 5테스트 5테스트 5테스트
</p>
```

## **font 단축 프로퍼티**

> font-style: italic;
> font-weight: bold;
> font-size: .8em;
> line-height: 1.2;
> font-family: Arial, sans-serif;
> 위 속성들을 아래와 같이 단축할 수 있다.font: italic bold .8em/1.2 Arial, sans-serif;

**사용법**

```jsx
font: font-style(옵션) font-variant(옵션) font-weight(옵션) font-size(필수)/line-height(옵션) font-familty(필수)
```

예시

```jsx
<p style="font: italic small-caps bolder 20px/2 sans-serif">
  Lorem ipsum dolor sit amet consectetur adipisicing elit. Error, veniam. Laboriosam nam, ea accusantium numquam quo tempora distinctio, officia culpa nemo natus ipsam necessitatibus earum rem.
    Fuga dolorem commodi neque? Sunt praesentium ipsum, similique quidem eos laborum id rem molestias harum placeat fugit magnam aperiam tenetur ea unde necessitatibus neque labore voluptate
    accusamus tempore ipsam, soluta porro vitae molestiae! Rerum. Aut commodi eveniet eos, voluptatem accusantium et id expedita adipisci qui perspiciatis sed veniam explicabo, omnis dolore est
    libero. Nemo iure magnam, quas doloribus sunt exercitationem ratione. Nemo, tempore omnis.
</p>
<p style="text-align: justify">
</p>
```

## **letter-spacing, word-spacing 프로퍼티**

- `letter-spacing` : 글자 사이 간격
- ``word-spacing` : 단어 사이 간격
- 보통 px 단위를 사용하여 설정, 값이 커질수록 간격이 넓어지고 음수도 가능

```jsx
<p>테스트</p>
<p style="letter-spacing: 20px">letter-spacing 테스트</p>
<p style="word-spacing: -20px">Lorem ipsum dolor sit amet consectetur adipisicing elit. Error, veniam. Laboriosam nam, ea accusantium numquam quo tempora distinctio, officia culpa nemo natus ipsam necessitatibus earum rem.
    Fuga dolorem commodi neque? Sunt praesentium ipsum, similique quidem eos laborum id rem molestias harum placeat fugit magnam aperiam tenetur ea unde necessitatibus neque labore voluptate
    accusamus tempore ipsam, soluta porro vitae molestiae! Rerum. Aut commodi eveniet eos, voluptatem accusantium et id expedita adipisci qui perspiciatis sed veniam explicabo, omnis dolore est
    libero. Nemo iure magnam, quas doloribus sunt exercitationem ratione. Nemo, tempore omnis.
</p>
<p style="text-align: justify">
</p>
```

## **text-align 프로퍼티**

글자 수평 정렬 설정

- `left` : 왼쪽 정렬
- `right` : 오른쪽 정렬
- `center` : 가운데 정렬
- `justify` : 양쪽 정렬

```jsx
<p>기본값 테스트</p>
<p style="text-align: left">left 테스트</p>
<p style="text-align: right">right 테스트</p>
<p style="text-align: center">center 테스트</p>
<p>
    Lorem ipsum dolor sit amet consectetur adipisicing elit. Error, veniam. Laboriosam nam, ea accusantium numquam quo tempora distinctio, officia culpa nemo natus ipsam necessitatibus earum rem.
    Fuga dolorem commodi neque? Sunt praesentium ipsum, similique quidem eos laborum id rem molestias harum placeat fugit magnam aperiam tenetur ea unde necessitatibus neque labore voluptate
    accusamus tempore ipsam, soluta porro vitae molestiae! Rerum. Aut commodi eveniet eos, voluptatem accusantium et id expedita adipisci qui perspiciatis sed veniam explicabo, omnis dolore est
    libero. Nemo iure magnam, quas doloribus sunt exercitationem ratione. Nemo, tempore omnis.
</p>
<p style="text-align: justify">
    Lorem ipsum dolor sit amet consectetur adipisicing elit. Error, veniam. Laboriosam nam, ea accusantium numquam quo tempora distinctio, officia culpa nemo natus ipsam necessitatibus earum rem.
    Fuga dolorem commodi neque? Sunt praesentium ipsum, similique quidem eos laborum id rem molestias harum placeat fugit magnam aperiam tenetur ea unde necessitatibus neque labore voluptate
    accusamus tempore ipsam, soluta porro vitae molestiae! Rerum. Aut commodi eveniet eos, voluptatem accusantium et id expedita adipisci qui perspiciatis sed veniam explicabo, omnis dolore est
    libero. Nemo iure magnam, quas doloribus sunt exercitationem ratione. Nemo, tempore omnis.
</p>
```

## **text-decoration 프로퍼티**

글자에 선을 넣는 설정

- `none` : 선x
- `line-through` : 글자 중간에 선
- `overline` : 글자 위에 선
- `underline` : 글자 아래에 선

```jsx
<p>테스트</p>
<p style="text-decoration: none">none 테스트</p>
<p style="text-decoration: line-through">line-through 테스트</p>
<p style="text-decoration: overline">overline 테스트</p>
<p style="text-decoration: underline">underline 테스트</p>
```

## **white-space 프로퍼티**

글자에 들어 있는 스페이스, 탭, 줄바꿈, 자동 줄바꿈 설정

| property | space and tab             | 개행문자(줄바꿈)          | 자동 줄바꿈 |
| -------- | ------------------------- | ------------------------- | ----------- |
| normal   | 병합하여 1개만 표시       | 병합하여 1개만 표시       | O           |
| nowrap   | 병합하여 1개만 표시       | 병합하여 1개만 표시       | X           |
| pre      | 유지하여 있는 그대로 표시 | 유지하여 있는 그대로 표시 | X           |
| pre-wrap | 유지하여 있는 그대로 표시 | 유지하여 있는 그대로 표시 | O           |
| pre-line | 병합하여 1개만 표시       | 유지하여 있는 그대로 표시 | O           |

```jsx
<p>
      Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris
  nisi ut aliquip ex ea commodo consequat.     Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident,
  sunt in culpa qui officia deserunt mollit anim id est laborum.
</p><br>

<p style="white-space: normal">
      Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris
  nisi ut aliquip ex ea commodo consequat.     Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident,
  sunt in culpa qui officia deserunt mollit anim id est laborum.
</p><br>

<p style="white-space: nowrap">
      Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris
  nisi ut aliquip ex ea commodo consequat.     Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident,
  sunt in culpa qui officia deserunt mollit anim id est laborum.
</p><br>

<p style="white-space: pre">
      Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris
  nisi ut aliquip ex ea commodo consequat.     Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident,
  sunt in culpa qui officia deserunt mollit anim id est laborum.
</p><br>

<p style="white-space: pre-wrap">
      Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris
  nisi ut aliquip ex ea commodo consequat.     Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident,
  sunt in culpa qui officia deserunt mollit anim id est laborum.
</p><br>

<p style="white-space: pre-line">
      Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris
  nisi ut aliquip ex ea commodo consequat.     Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident,
  sunt in culpa qui officia deserunt mollit anim id est laborum.
</p>
```

## **text-overflow 프로퍼티**

문자열이 넘칠 경우(부모 영역을 벗어날 경우)에 자동 줄바꿈이 되지 않은 문자열 처리 설정.

- 다음 조건이 설정되어 있는 상태에서 설정이 가능하다.
  > width property 설정white-space: nowrap 설정overflow property가 visible 이외의 값으로 설정
- `clip` : 텍스트를 잘라냄
- `ellipsis` : 텍스트가 잘렸다는 것을 말줄임표(…)로 표시

```
    <style>
      .clip {
        width: 300px;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: clip;
      }
      .ellipsis {
        width: 300px;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
      }
    </style>
  </head>
  <body>
    <p>
      Lorem ipsum dolor sit amet, consectetur adipiscing elit,
      sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
    </p>
    <br />
    <p class="clip">
      Lorem ipsum dolor sit amet, consectetur adipiscing elit,
      sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
    </p>
    <br />
    <p class="ellipsis">
      Lorem ipsum dolor sit amet, consectetur adipiscing elit,
      sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
    </p>

```
