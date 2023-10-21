# Chart.js 그래프 안의 그리드를 없애고 x축, y축만 컬러 넣기

chart에서 디자인에 맞추어 표출해야할 때 제일 어려웠던 것을 꼽자면 x축, y축에만 컬러를 넣는 것이었다.
고분군투기를 한번 남겨보자.

### 1. 차트 내부의 그리드 지우기
```js
options: {
  scales: {
    x: {
      beginAtZero: true,
        scaleLineColor: 'red',
          grid: {
            color: 'transparent',
          },
       },
     y: {
       beginAtZero: true,
         grid: {
           color: 'transparent',
         },
     }
  }
}
     
```
위와 같이 각 축의 grid의 컬러를 transparent로 지정해주면 내부의 선은 사라지는 것을 쉽게 확인할 수 있다.
![](https://velog.velcdn.com/images/yxxnhx/post/a39b7a52-c56a-4637-933f-26b5ce7f5051/image.png)

그렇다면 x축, y축의 컬러만 넣으려면 어떻게 해야할까?
### 2. x축, y축에만 컬러 넣기
결론만 말하자면 해당 시도는 사실 틀린 방법이다.
그러나 기록을 위해 남겨놓겠다.

#### 1) 각축의 첫번째 축을 찾아 컬러 넣기
```js
options: {
  scales: {
    x: {
      beginAtZero: true,
        grid: {
          color: (ctx) => (ctx.index === 0 ? '#712' : 'transparent'),
        },
        },
          y: {
            beginAtZero: true,
              grid: {
                color: function (context) {
                  if (context.tick.value === 0) {
                    return '#712';
                  }
                  return 'transparent';
                },
              },
          },
    },
}
```
x축에서는 ctx 즉, chart의 index가 0번째 첫번째의 축에만 컬러를 넣기
y축에서는 context를 콘솔에 찍어보면 
![](https://velog.velcdn.com/images/yxxnhx/post/cce6dda4-ac08-4f03-81ef-eca7a02b6d47/image.png)
위와 같이 tick의 value 즉, ``datasets``의 data 범위를 나타내는 y축의 0을 나타내는 축에만 컬러를 넣는 것이다.
![](https://velog.velcdn.com/images/yxxnhx/post/69de654a-5e5b-4113-a2f9-6494c79c6930/image.png)
사진을 보면 얼추 비슷해보이지만 삐죽삐죽 튀어나온 부분이 영 거슬릴 수가 없다.

한참을 뒤지고 뒤져보니 공식문서에 축의 디폴트 컬러를 설정할 수 있는 것을 찾아내었다!

#### 2) default color를 지정해주기
```js
Chart.defaults.borderColor = '#712';
const myChart = new Chart(document.getElementById('myChart'), config);
```
default borderColor를 지정해주면 그게 내가 찾던 축만 컬러를 넣는 것이었다!
여기서 주의해야할 점은 new Chart로 새롭게 차트를 그리기 전에 컬러를 지정해주어야 한다.
그렇지 않으면 ``Chart is not defined`` 라는 에러를 만날 수 있다.

![](https://velog.velcdn.com/images/yxxnhx/post/275378d3-6682-4547-b606-8588ebcb32d6/image.png)

참고자료: https://www.chartjs.org/docs/latest/axes/
