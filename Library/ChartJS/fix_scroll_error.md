# Chart.js 그래프 스크롤에 따른 툴팁 에러 해결하기

이전에 차트의 데이터에 따라 동적으로 스크롤이 생기게 했었다.
세상 신나게 이제 차트는 끝이다!! 하고 외쳤는데 테스트를 하다보니 에러를 찾게 되었다.
새로운 데이터로 업데이트 되어 가로 스크롤이 생기면 차트 위에 호버를 했을 때 툴팁이 해당 위치에 맞지 않고 이상한 위치에 표출되는 것을 확인하게 되었다.

#### 왜 그런걸까?
```html
<style>
  .chartBox {
  width: 700px;
  padding: 20px;
  border-radius: 20px;
  border: solid 3px rgba(54, 162, 235, 1);
  background: white;
  }
  .container {
  width: 700px;
  max-width: 350px;
  overflow-x: scroll;
  }

  .containerBody {
  height: 500px;
  }
</style>

<div class="chartBox">
  <div class="container">
    <div class="containerBody">
      <canvas id="myChart"></canvas>
    </div>
  </div>
</div>
```
```js
const {offsetLeft: positionX, offsetTop: positionY} = chart.canvas;
tooltipEl.style.opacity = 1;
tooltipEl.style.left = positionX + tooltip.caretX + 'px';
tooltipEl.style.top = positionY + tooltip.caretY - tooltip.height + 'px'; 
```
커스텀 툴팁을 만들어 위치를 지정해주었지만 
실제 위치는 기존 container에 지정한 넓이 700px에 맞추어서 계산되어 툴팁을 표출하고 있었다.

스크롤한 위치만큼 계산해서 그려주어야 그에 맞게 제대로 표출이 될 것이다.

#### 그렇다면 어떻게 계산해야 할까?
```js
const position = chart.canvas.getBoundingClientRect();

tooltipEl.style.left = position.left + tooltip.caretX + 'px';
tooltipEl.style.top = position.top + tooltip.caretY + window.scrollY  'px';
```
chart의 캔버스 실제 넓이와 위치를 구하면 정상적으로 툴팁이 표출되는 것을 확인할 수 있다.
조금만 깊게 생각해보면 쉽게 답을 얻을 수 있는데 한참을 검색해봐도 나오지 않아 꽤나 머리를 쥐어뜯었었다.
