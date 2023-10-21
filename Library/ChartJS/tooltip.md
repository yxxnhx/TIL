
# Chart.js 두 개의 데이터를 하나의 툴팁에 넣기

![](https://velog.velcdn.com/images/yxxnhx/post/bb84e008-886b-482c-b6ce-7dc9c3f09a12/image.png)

기존 디폴트로는 chart.js 에서 두 개의 데이터를 사용하게 되면 각각 표출하게 된다.
line 형식에서는 괜찮지만 아래와 같은 bar 형식의 그래프에서 하나의 툴팁에 보여주게 하고 싶은 경우 어떻게 해야할까?
![](https://velog.velcdn.com/images/yxxnhx/post/7062f34e-c1f6-4289-adc5-f9fbe54e5799/image.png)

방법은 생각보다 간단하다.

```js
const config = {
  type: 'bar',
  data: chartData,
  options: {
    interaction: {
      mode: 'index',
      intersect: false,
    },
  }
}
```

![](https://velog.velcdn.com/images/yxxnhx/post/ade4e499-6892-4608-ac5e-64eee3781f03/image.png)

옵션에서 index 모드를 설정해주면 해당 인덱스에 맞게 그에 해당하는 데이터가 툴팁에 들어가게 된다.
