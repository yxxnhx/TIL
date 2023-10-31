# Chart.js CANVAS is already in use 에러 해결하기

![](https://velog.velcdn.com/images/yxxnhx/post/f4539666-39a8-4895-9635-72edbfcf0a7e/image.png)

초기에 차트를 표출했다가 새롭게 다시 차트를 그릴 때 기존 그래프가 초기화되지 않아서 일어난 에러이다.
처음에는 차트를 여기서밖에 안 쓰는데 대체 어디서 썼다는 건지 당최 이해할 수 없어서 스크립트만 열심히 디버깅해보고 있었다.

그러나 모든 문제는 기초에서부터 시작하듯 공식문서를 다시 뜯어보니 기존 차트의 인스턴스를 날리고 싶으면 ``destroy`` 메서드를 쓰라고 안내되어 있었다.

참고 문서 : https://www.chartjs.org/docs/latest/developers/api.html

### 해결 방법
```js
let chartStatus = Chart.getChart('차트이름');
if (chartStatus !== undefined) {
  chartStatus.destroy();
}
```
``getChart``, 차트를 불러올 수 있다면, 즉 기존 차트의 인스턴스가 있다면 그 차트를 불러서 ``destroy 초기화``를 시키고 다시 그려내는 것이다.

