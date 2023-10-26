# Chart.js 데이터 조건에 따라 그래프 새로 그리기

차트를 그저 데이터를 초기에 딱 한번 받아서 표출하고 끝이나면 얼마나 좋을까.
하지만 실제로는 데이터에 조건에 따라 새롭게 데이터를 받아서 그에 맞게 차트를 업데이트 해야하는 경우가 많다.

그렇다면 동적으로 데이터를 업데이트를 해보자.
다행이도 공식문서에 꽤나 친절하게 그 방법에 대하여 알려주고 있다.
```js
function addData(chart, label, newData) {
    chart.data.labels.push(label);
    chart.data.datasets.forEach((dataset) => {
        dataset.data.push(newData);
    });
    chart.update();
}

function removeData(chart) {
    chart.data.labels.pop();
    chart.data.datasets.forEach((dataset) => {
        dataset.data.pop();
    });
    chart.update();
}
 
```
데이터 배열을 변경하면 데이터 추가 및 제거가 지원됩니다. 
데이터를 추가하려면 이 예제에 표시된 대로 데이터 배열에 데이터를 추가하기만 하면 됩니다. 
이를 제거하려면 다시 팝하면 됩니다.
라고 설명이 되어있다.

참고 자료 : https://www.chartjs.org/docs/latest/developers/updates.html

#### 그렇다면 데이터를 전체 지웠다가 다시 그리게 할 수는 없을까?
예를 들어 10월 23일~10월 27일까지의 통계 데이터를 차트로 표출했다가
새로이 10월 1일~10월 9일의 데이터를 받아 표출하려고 한다.
그러면 기존의 데이터를 지우고 새로운 데이터로 그리는 것이 훨씬 깔끔하고 에러의 위험이 줄어든다.

```js
const removeData = (chart) => {
  chart.data.labels = [];
  chart.data.datasets.forEach((dataset) => {
    dataset.data = [];
  });
  chart.update();
}
```
이렇게 그냥 빈 데이터를 넣어서 지우고

```js
const addData = (chart, newData, scrollElement) => {
        removeData(chart);
  // 생략
}
```
이렇게 addData를 하기 전, 전체를 지우고 새로운 데이터를 받아 넣어주면 된다.
