# CSS inheritance

## 상속과 css 우선순위

요소 간에는 부모와 자식관계가 있고, 상속은 부모 요소의 프로퍼티 값을 자식 요소가 물려받는 것을 의미함

생산성을 높일 수 있는 기능이지만, 실제 코드 작성시에는 복잡한 상속관계로 복잡도가 올라갈 수 있음

### 상속

요소 간에는 부모와 자식관계가 있고, 상속은 부모 요소의 프로퍼티 값을 자식 요소가 물려받는 것을 의미함

**코드로 확인하기**

p 태그의 color 프로퍼티 값은 span 태그에도 적용됨(상속)

h1 태그의 border 프로퍼티 값은 span 태그에 적용되지 않음

→ span 태그에 적용되었다면 span 태그를 왼쪽 오른쪽에도 빨간 선이 있어야 함

```jsx
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <style>
      p {
        color: OrangeRed;
      }
      h1 {
        border: 1px solid OrangeRed;
      }
    </style>
  </head>
  <body>
    <p>p 태그 안에 <span>span 태그를</span> 넣어보았습니다.</p>
    <h1>p 태그 안에 <span>span 태그를</span> 넣어보았습니다.</h1>
  </body>
</html>
```

### 주요 프로퍼티별 상속 여부 정리

프로퍼티 별로, 상속되는 프로퍼티와 상속이 안 되는 프로퍼티가 존재함(태그별로도 프로퍼티 상속이 안 되는 태그도 존재함

- 상속 가능: text-align, line-height, color, font, visibility, opacity
- 상속 불가: width, height, margin, padding, display, box-sizing, background, vertical-align, position(top, bottom, left), z-index, overflow, float

### 강제 상속 설정(inherit)

부모의 프로퍼티 중 상속되지 않는 프로퍼티 값을 상속하고 싶을 때에는, 자식 요소에 해당 프로퍼티를 inherit으로 설정하면 된다.

```jsx
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <style>
      p {
        color: OrangeRed;
      }
      h1 {
        border: 1px solid OrangeRed;
      }
    </style>
  </head>
  <body>
    <p>p 태그 안에 <span>span 태그를</span> 넣어보았습니다.</p>
    <h1>p 태그 안에 <span>span 태그를</span> 넣어보았습니다.</h1>
  </body>
</html>
```

## css 우선순위와 Cascading

다양한 css 프로퍼티 적용과 상속으로 인해, 특정 요소에 어떤 프로퍼티값이 적용될지를 결정해야 함

이를 위해 각 프로퍼티 설정 형태에 따른 우선순위를 정해놓고, 이에 기반해서 특정 요소에 적용할 프로퍼티값을 정함

→ 이를 캐스케이딩(cascading order)라고도 함

### cascading 기본 규칙

**중요도: css 어디에 선언했는지에 따라 우선순위가 달라짐**

1. head 태그 안에 style 요소
2. head 태그 안에 style 태그 안의 @import
3. <link> 로 연결된 css 파일
4. <link> 로 연결된 css 파일 안의 @import 문

**명시도 : 대상을 명확하게 지정할수록 우선순위가 높아짐**

**선언순서: html 문서에서 뒤에 나오는 css가 우선순위 높음**

<aside>
💡 **css 우선 순위 (명시도 계산) 기본 규칙**
중요도, 선언 순서보다 명시도가 주로 우선 순위에 많이 영향을 미치며, 계산 방식을 가볍게는 알고 있어야 함

</aside>

1. html 문서에서 뒤에 나오는 css가 우선순위가 높음
2. 기본 우선순위(높은 순으로 정렬, 우선순위 점수)

   - 프로퍼티 값 뒤에 !important를 적은 경우 (우선순위 무한대)

   ```jsx
   .yellow {
   background-color: yellow; !important
   }
   ```

   - 태그 안에 속성으로 적은 style에 의해 설정된 프로퍼티(우선순위 점수: 1000점)

   ```jsx
   <p style="text-align:center">스타일</p>
   ```

   - id로 선택한 css selector에서 적동된 프로퍼티(우선순위 점수 100점)

   ```jsx
   #id {
   color: blue;
   }
   ```

   - class, html 속성(attribute), 수도 클래스(pseudo class, :link 등)로 선택한 css selector에서 적용된 프로퍼티(우선순위 10점)

   ```jsx
   .class {color: green;}
   [title = 'title']{color: green;}
   :link{color: red;}
   ```

   - 태그 또는 가상 요소 셀렉터(::first-letter, ::first-line, ::after, ::before, ::selection 등)로 선택한 css selector에서 적용된 프로퍼티(우선순위 1점)

   ```jsx
   h1 { color: red;}
   ```

   - 전체(\*)로 선택한 css selector에서 적용된 프로퍼티(우선순위 0점)

   ```jsx
   * { margin: 0;}
   ```

   - 상속된 프로퍼티 (우선순위 마이너스)

<aside>
💡 **!important > lnline style > id > class > tag**

</aside>

!important와 태그 안에 적은 style은 사용을 제한한다

→ 복잡도를 더 높일 수 있기 때문이다
