# Chart.js 그래프에 좌우 스크롤 넣기

그래프 영역에 좌우 스크롤을 넣어보자.

#### HTML 구조
```html
<div class="chartBox">
  <div class="container">
    <div class="containerBody">
      <canvas id="myChart"></canvas>
    </div>
  </div>
</div>
```
```css
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
```
- chartBox : 차트 상위 div
- container : 차트 스크롤을 감싸는 영역
- containerBody : 차트 canvas를 담고 있는 영역

#### script

```js
<script>
  // setup
  const data = {
    labels: [
      'Mon', 'Tue', 'Wed','Thu','Fri','Sat','Sun',
      'Mon', 'Tue', 'Wed','Thu','Fri','Sat','Sun',
    ],
    datasets: [
      {
        label: 'Weekly Sales',
        data: [18, 12, 6, 9, 12, 3, 9, 18, 12, 6, 9, 12, 3, 9],
        backgroundColor: [
          'rgba(255, 26, 104, 0.2)',
          'rgba(54, 162, 235, 0.2)',
          'rgba(255, 206, 86, 0.2)',
          'rgba(75, 192, 192, 0.2)',
          'rgba(153, 102, 255, 0.2)',
          'rgba(255, 159, 64, 0.2)',
          'rgba(0, 0, 0, 0.2)',
        ],
        borderColor: [
          'rgba(255, 26, 104, 1)',
          'rgba(54, 162, 235, 1)',
          'rgba(255, 206, 86, 1)',
          'rgba(75, 192, 192, 1)',
          'rgba(153, 102, 255, 1)',
          'rgba(255, 159, 64, 1)',
          'rgba(0, 0, 0, 1)',
        ],
        borderWidth: 1,
      },
    ],
  };

// config
const config = {
  type: 'bar',
  data,
  options: {
    maintainAspectRatio: false,
    scales: {
      y: {
        beginAtZero: true,
        grid: {
          borderWidth: 105,
          lineWidth: 0,
        },
      },
    },
  },
};

// render init block
const myChart = new Chart(document.getElementById('myChart'), config);
</script>
```
기본 구조 그대로 넣는다면 지정하였던 container 넓이에 맞춰서 차트가 축소되어 표출되는 것을 확인할 수 있다.
그렇다면 어떻게 해야 크기는 그대로이나 데이터 길이에 따라 동적으로 넓이가 늘어나서 스크롤이 생길까?

#### chart의 구조를 생각해보자.
chart 내 에서 데이터 길이를 구하려면 어떻게 해야할까?
방법은 생각보다 굉장히 간단하다.

``label의 길이``를 구하면 된다. data가 결국은 datasets에서 지정한 label이다.
```js
const container = document.querySelector('.containerBody');
const totalLables = myChart.data.labels.length;
if (totalLables > 7) {
  const newWidth = 700 + (totalLables - 7) * 30;
  container.style.width = newWidth + 'px';
}
```
이렇게 총 길이가 지정한 갯수 이상을 넘어가게 되면 원하는 크기만큼 늘어나게 해주면 데이터의 길이에 따라 동적으로 크기가 늘어나 스크롤이 생기게 할 수 있게 되었다.

참고 자료: https://www.youtube.com/watch?v=oa0_EsFzxBg
