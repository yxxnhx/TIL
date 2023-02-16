# 이벤트 EVENT

### JavaScript 파일로 html 파일 수정하려면 어떻게 해야 할까?

- 지금 js파일이 있기 때문에 js를 통해 html의 내용을 가져올 수 있다
- document가 html이 js파일을 load하기 때문에 존재
  → 그 다음에 browser가 우리가 document에 접근할 수 있게 해줌
- element의 내부를 보고 싶으면 console.dir()
  기본적으로 object로 표시한 element를 보여줌(전부 js object)
  그 element 중 앞에 on이 붙은 것들은 event이다
- event: 어떤 행위를 하는 것
  모든 event는 js가 listen할 수 있음
- eventListener : event를 listen함
  → js에게 무슨 event를 listen하고 싶은 지 알려줘야 함
- title.addEventListener("click") : 누군가가 title을 click하는 것을 listen할 것이다 → 무언가를 해줘야한다!

```
const title = document.querySelector("div.hello:first-child h1");

function handleTitleClick(){
  title.style.color = "blue";
}
title.addEventListener("click",handleTitleClick);
//click하면 handleTitleClick이라는 function이 동작하길 원함
//그래서 handle~ 함수에 () 를 안넣은 것
//즉, js가 대신 실행시켜주길 바라는 것!
```

- function이 js에게 넘겨주고 유저가 title을 click할 경우에 js가 실행버튼을 대신 눌러주길 바라는 것( 직접 handleTitleClick(); 이렇게 하는 것이 아니라)
- 함수에서 () 이 두 괄호를 추가함으로써 실행버튼을 누를 수 있다

```
title.onclick = handleMouseEnter;
title.addEventListener(“mouseenter” , handleMouseEnter);
```

위에 두 코드는 동일하나 addEventListener를 선호하는 이유는
removeEventListener을 통해서 event listener을 제거할 수 있기 때문이다.

document에서 body,head,title 은 중요해서 언제든
ex) document.body 로 가져 올 수있지만
div나 h1 등 element 들은 querySelector getElementById 등으로 찾아야한다.

```
document.querySelector(“h1”);
```

### window는 기본제공

```
function handleWindowResize(){
  document.body.style.backgroundColor = “tomato”;
}
function handleWindowCopy(){
  alert(“copier”);
}

window.addEventListener(“resize”, handleWindowResize);
window.addEventListener(“copy”, handleWindowCopy);
```

## Quiz

조건에 유의하여 만들기

```
// <⚠️ DONT DELETE THIS ⚠️>
import "./styles.css";
const colors = ["#1abc9c", "#3498db", "#9b59b6", "#f39c12", "#e74c3c"];
// <⚠️ /DONT DELETE THIS ⚠️>

/*
✅ The text of the title should change when the mouse is on top of it.
✅ The text of the title should change when the mouse is leaves it.
✅ When the window is resized the title should change.
✅ On right click the title should also change.
✅ The colors of the title should come from a color from the colors array.
✅ DO NOT CHANGE .css, or .html files.
✅ ALL function handlers should be INSIDE of "superEventHandler"
*/
const title = document.querySelector("h2");
const superEventHandler = {
  handleTitleOver() {
    title.style.color = colors[0];
    title.innerHTML = "The mouse is here!";
  },
  handleTitleLeave() {
    title.style.color = colors[1];
    title.innerHTML = "The mouse is over!";
  },
  handleRightClick() {
    title.style.color = colors[2];
    title.innerHTML = "That was a right click!";
  },
  handleResize() {
    title.style.color = colors[3];
    title.innerHTML = "You just resized!";
  }
};

title.addEventListener("mouseover", superEventHandler.handleTitleOver);
title.addEventListener("mouseout", superEventHandler.handleTitleLeave);
window.addEventListener("contextmenu", superEventHandler.handleRightClick);
window.addEventListener("resize", superEventHandler.handleResize);
```
