# Chart.js bar형 그래프 위에 값 넣기
datalabels 플러그인을 활용하여 bar형 차트에서 각각의 bar 위에 값을 넣어보자.

### 1. 라이브러리 불러오기
```js
// 설치하기
npm install chartjs-plugin-datalabels --save

// cdn 이용하기
<script src="https://cdn.jsdelivr.net/npm/chartjs-plugin-datalabels@2.0.0"></script>
```
각자 편한 방법으로 이용하여 사용하면 된다.


### 2. chart에 라이브러리 등록하기
```js
const config = {
  type: 'bar',
  data,
  plugins: [ChartDataLabels],
  // 생략
}
```

### 3. 위치 및 색상 커스터마이징 하기
```js
// 색상 설정
Chart.defaults.set('plugins.datalabels', {
  color: '#FE777B',
});

// 위치 설정
const data = {
  labels: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
  datasets: [
    {
      label: 'Weekly Sales',
      data: [18, 12, 6, 9, 12, 3, 9],
      // 생략
      datalabels: {
        align: 'end',
        anchor: 'end',
      },
      borderWidth: 1,
    },
    {
      label: 'Weekly Sales',
      data: [9, 3, 12, 9, 12, 18],
      // 생략
      datalabels: {
        align: 'end',
        anchor: 'end',
      },
      borderWidth: 1,
    },
  ],
};
```
위와 같이 컬러와 위치를 각각 지정해주면 원하는 위치에 맞게 지정한 값이 표출될 수 있다.
![](https://velog.velcdn.com/images/yxxnhx/post/b0cd9e68-c23b-4257-9f9e-1a5c9581bb75/image.png)

이 외에도 다양하게 커스터마이징을 할 수 있으니 공식문서를 참고하여 이용해보자!

공식문서:  https://chartjs-plugin-datalabels.netlify.app/
