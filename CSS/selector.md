# CSS selector

## 전체 selector - \*

html 문서 전체를 선택

```html
<style>
* { margin: 0
	  padding: 0
}
```

## 태그 selector

태그명으로 선택

→ 많이 사용되지는 않음 tag에 class/id를 부여하여 사용함 (css 우선 순위 때문)

```html
main { padding: 10px; display: flex; flex-wrap: wrap; }
```

## ID selector

#아이디명으로 선택 (#를 붙여야 아이디인지를 구분할 수 있음

```html
#main { color: blue; }
```

## Class selector

.클래스명으로 선택 “.(점)”을 붙여야 아이디인지를 구분할 수 있음

```html
.main { color: blue; }
```

## attribute(속성) selector

- [속성] attr 속성을 가지는 모든 태그(요소)

```html
[attr] { color: red; }
```

→ 해당 속성을 가진 모든 태그에 color red를 반영해라

- [속성=값] attr 속성값이 정확히 속성을 가지는 모든 태그(요소), 대소문자 구분하지 않음

```html
[attr='value'] { color: red; }
```

→ 특정한 값을 지정한 태그에만 반영해라

- [속성~=값] attr 속성값이 ‘value’를 (공백으로 분리된) 단어로 포함하는 모든 태그(요소), 대소문자 구분하지 않음

```html
[attr~=value] { color: red; }
```

→ 해당 속성을 가진 모든 태그에 color red를 반영해라

- [속성|=값] attr 속성값이 “value”이거나 “value”로 시작하면서 - 문자가 곧바로 뒤에 따라붙는 모든 태그(요소), 대소문자 구분하지 않음

```html
[attr|="value"] { color: red; }
```

- [속성^=값] a 태그의 요소 중 attr 속성 값이 “value”로 시작하는 모든 태그(요소), 대소문자 구분

```html
[attr] { color: red; }
```

- [속성^=값] a 태그의 요소 중 attr 속성 값이 “value”로 시작하는 모든 태그(요소), 대소문자 구분

```html
[attr] { color: red; }
```

- [속성^=값] a 태그의 요소 중 attr 속성 값이 “value”로 시작하는 모든 태그(요소), 대소문자 구분

```html
[attr] { color: red; }
```

- [속성$=값] a 태그의 요소 중 attr 속성 값이 “value”로 끝나는 모든 태그(요소), 대소문자 구분

```html
[attr] { color: red; }
```

- [속성*=값] a 태그의 요소 중 attr 속성 값이 “value”를 포함하는 모든 태그(요소), 대소문자 구분

```html
[attr] { color: red; }
```

## 다양한 CSS selector 조합

- 태그, 아이디, 클래스 selector를 조합해서 복합적으로 사용 가능
- HTML 문서 특정 태그에 여러 클래스가 지정될 경우, 다음과 같이 스페이스를 이용해서 여러 클래스를 설정할 수 있음
  - class=”클래스명1 클래스명2 클래스명3 ”

  - 이 경우 css selector는 .클래스명1.클래스명2.클래스명3와 같이 사용 가능

```html
h1 .title #id { color: red; }
```

## 복합 selector(Combinator)

- 태그 안에, 또 다른 태그를 넣을 수 있으므로, 각 태그 또는 요소 간에 부모/자식의 관계가 매겨짐
- 관계를 기반으로 HTML 문서 특정 부분을 선택할 수 있는 문법

```html
<div>
  <h1>tiltle</h1>
  <p>hello world</p>
  <p>good content<span>good</span>conent</p>
</div>
```

→ 하나의 태그 안에 중첩되어 들어간 또다른 태그가 있을 수 있고, 또 동일한 레벨에서 중첩된 태그가 있는 경우가 있을 때, 부모와 자식관계인지, 인접한 동일한 레벨의 관계인지 구분하여 정확하게 선택할 수 있다

## 후손 셀렉터(Descendent Selector): 스페이스로 표시

- 부모 태그 안에 있는 모든 하위 태그를 하위 요소, 후손 요소라고 부름
- 부모 태그(selector1) 안에 있는 모든 태그 중에 select2를 선택

```html
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <style>
      h1 {
        color: black;
      }
      div span {
        color: royalblue;
      }
    </style>
  </head>
  <body>
    <div>
      <h1>Dave Lee</h1>
      <p>잔재미코딩을 운영하고 있습니다. 현업 기획자 및 개발자입니다.</p>
      <p><span>좋은 컨텐츠와 서비스</span>를 만드는 일에 집중하고 있습니다.</p>
    </div>
  </body>
</html>
```

## 자식 셀렉터(child selector): >로 표시

- 부모 태그(selector1) 안에 있는 바로 다음 레벨의 태그 중에 selector2를 선택

```html
selector1 > selector2
```

## 인접 형제 셀렉터 (Adjacent Sibling Selector): +로 표시

- 태그(selector1)와 동일 레벨에 위치하고, selector1의 바로 뒤에 위치하는 selector2를 선택

```html
selector1 + selector2
```

→ selector1과 selector2 사이에 다른 태그 위치시 선택 불가

## 일반 형제 셀렉터 (Adjacent Sibling Selector): ~로 표시

- 태그(selector1)와 동일 레벨에 위치하고, selector1의 로 뒤에 위치하는 selector2를 선택

```html
selector1 ~ selector2
```

→ selector1과 selector2 사이에 다른 태그 위치해도 선택 가능(그 뒤에 있기만 하면 가능)

## 가상 클래스 셀렉터(Pseudo-Class Selector)

- 가상 클래스는 요소에 특정 이벤트 발생시 선택하는 문법

## 가상 클래스 종류

- **link**: 방문하지 않은 링크가 적용된 요소
  → a태그로 링크가 적용된 요소를 한 번도 클릭하지 않은 경우
- **visited**: 방문한 링크가 적용된 요소
  → a태그로 링크가 적용된 요소를 한번이라도 클릭한 경우
- **hover**: 특정 요소에 마우스가 올라간 상태
- **active**: 링크 요소를 클릭한 상태
  → a태그로 링크가 적용된 요소를 마우스로 클릭하고 있는 상태
- **focus**: 특정 요소에 포커스가 있는 상태
  → input 태그에 포커스가 있어서 해당 입력창에 커서가 깜빡이는 상태

> 가상 셀렉터는 가상 요소 셀렉터 이외에는 한 개의 콜론:dmf tkdydgka
