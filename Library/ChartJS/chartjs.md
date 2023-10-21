# Chart.js
차트를 그리기 위해 라이브러리를 이용해보았다.
내가 사용한 차트는 line, bar, doughnut 모양이다.

공식문서를 참고해서 작업을 시작해보자.
https://www.chartjs.org/docs/latest/getting-started/

```js
<div>
  <canvas id="myChart"></canvas>
</div>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<script>
  const ctx = document.getElementById('myChart');

  new Chart(ctx, {
    type: 'bar',
    data: {
      labels: ['Red', 'Blue', 'Yellow', 'Green', 'Purple', 'Orange'],
      datasets: [{
        label: '# of Votes',
        data: [12, 19, 3, 5, 2, 3],
        borderWidth: 1
      }]
    },
    options: {
      scales: {
        y: {
          beginAtZero: true
        }
      }
    }
  });
</script>
 
```

굉장히 간단한 문법만으로도 chart를 그릴 수 있다.
하지만 문제가 있다면 디자인에 맞춰야 한다는 것이다.
그냥 있는 그대로 표출만 한다면 5초만에 끝낼 수 있지만 디자인에 맞춰야하다보니 거의 하루종일 공식문서와 스텍오버플로우를 부여잡고 살았다.

그렇다면 내가 마주한 문제들과 해결해나간 과정들을 기록해보려한다.
나중에 다시 사용해야할 때 까먹을까봐 더 지나기 전에 기록해보자!

### 1. 두 개의 데이터를 하나의 툴팁에 넣기


### 2. 커스텀 툴팁 만들기


### 3. 그래프 안의 그리드를 없애고 x축, y축만 컬러 넣기


### 4. bar형 그래프 위에 값 넣기


### 5. 그래프에 좌우 스크롤 넣기


### 6. 데이터 조건에 따라 그래프 새로 그리기
