# Chartjs 커스텀 툴팁 만들기

기존 chartjs에서 제공하는 툴팁을 사용하면 편하지만 디자인에 맞추어서 변경해야 하는 경우가 있다.
열심히 뒤져봐도 제대로 나오지 않아 한동안 헤매다가 역시 답은 공식문서에 있다는 말을 알게 되었다.

### 1. custom tooltip 함수 만들기
```js
const config = {
  type: 'line',
  data: data,
  options: {
    interaction: {
      mode: 'index',
      intersect: false,
    },
    plugins: {
      title: {
        display: true,
        text: 'Chart.js Line Chart - External Tooltips'
      },
      tooltip: {
        enabled: false,
        position: 'nearest',
        external: externalTooltipHandler
      }
    }
  }
};

const getOrCreateTooltip = (chart) => {
  let tooltipEl = chart.canvas.parentNode.querySelector('div');

  if (!tooltipEl) {
    tooltipEl = document.createElement('div');
    tooltipEl.style.background = 'rgba(0, 0, 0, 0.7)';
    tooltipEl.style.borderRadius = '3px';
    tooltipEl.style.color = 'white';
    tooltipEl.style.opacity = 1;
    tooltipEl.style.pointerEvents = 'none';
    tooltipEl.style.position = 'absolute';
    tooltipEl.style.transform = 'translate(-50%, 0)';
    tooltipEl.style.transition = 'all .1s ease';

    const table = document.createElement('table');
    table.style.margin = '0px';

    tooltipEl.appendChild(table);
    chart.canvas.parentNode.appendChild(tooltipEl);
  }

  return tooltipEl;
};

const externalTooltipHandler = (context) => {
  // Tooltip Element
  const {chart, tooltip} = context;
  const tooltipEl = getOrCreateTooltip(chart);

  // Hide if no tooltip
  if (tooltip.opacity === 0) {
    tooltipEl.style.opacity = 0;
    return;
  }

  // Set Text
  if (tooltip.body) {
    const titleLines = tooltip.title || [];
    const bodyLines = tooltip.body.map(b => b.lines);

    const tableHead = document.createElement('thead');

    titleLines.forEach(title => {
      const tr = document.createElement('tr');
      tr.style.borderWidth = 0;

      const th = document.createElement('th');
      th.style.borderWidth = 0;
      const text = document.createTextNode(title);

      th.appendChild(text);
      tr.appendChild(th);
      tableHead.appendChild(tr);
    });

    const tableBody = document.createElement('tbody');
    bodyLines.forEach((body, i) => {
      const colors = tooltip.labelColors[i];

      const span = document.createElement('span');
      span.style.background = colors.backgroundColor;
      span.style.borderColor = colors.borderColor;
      span.style.borderWidth = '2px';
      span.style.marginRight = '10px';
      span.style.height = '10px';
      span.style.width = '10px';
      span.style.display = 'inline-block';

      const tr = document.createElement('tr');
      tr.style.backgroundColor = 'inherit';
      tr.style.borderWidth = 0;

      const td = document.createElement('td');
      td.style.borderWidth = 0;

      const text = document.createTextNode(body);

      td.appendChild(span);
      td.appendChild(text);
      tr.appendChild(td);
      tableBody.appendChild(tr);
    });

    const tableRoot = tooltipEl.querySelector('table');

    // Remove old children
    while (tableRoot.firstChild) {
      tableRoot.firstChild.remove();
    }

    // Add new children
    tableRoot.appendChild(tableHead);
    tableRoot.appendChild(tableBody);
  }

  const {offsetLeft: positionX, offsetTop: positionY} = chart.canvas;

  // Display, position, and set styles for font
  tooltipEl.style.opacity = 1;
  tooltipEl.style.left = positionX + tooltip.caretX + 'px';
  tooltipEl.style.top = positionY + tooltip.caretY + 'px';
  tooltipEl.style.font = tooltip.options.bodyFont.string;
  tooltipEl.style.padding = tooltip.options.padding + 'px ' + tooltip.options.padding + 'px';
};
```

이렇게 툴팁 엘리먼트를 새롭게 만들고 그 안에서 그려내는 것이다.
개발자모드로 확인해보면
![](https://velog.velcdn.com/images/yxxnhx/post/7a8d9a2d-5050-4af1-95a1-b00d81347d8a/image.png)
아래에 tooltip div가 생겨있는 것을 확인할 수 있다.

이 getOrCreateTooltip 함수는 툴팁의 전체 틀을 관리하는 것으로 툴팁의 배경색, 폰트 컬러, border 등 틀을 잡아낼 수 있다.
툴팁 안에 내용은 externalTooltipHandler 함수로 컨트롤할 수 있다.
``tooltip.title``은 기존에 데이터 설정을 할 때 사용했던 dataset의 label이다.
이것을 이용해서 차트에서 표출하는 label 외에 툴팁에서 더 추가해서 데이터를 보여주어야 할 경우에는 조금 더 응용해서 사용이 가능하다.

그리고 툴팁의 위치를 수정할 때는 
```js
  const {offsetLeft: positionX, offsetTop: positionY} = chart.canvas;

  // Display, position, and set styles for font
  tooltipEl.style.opacity = 1;
  tooltipEl.style.left = positionX + tooltip.caretX + 'px';
  tooltipEl.style.top = positionY + tooltip.caretY + 'px';
  tooltipEl.style.font = tooltip.options.bodyFont.string;
  tooltipEl.style.padding = tooltip.options.padding + 'px ' + tooltip.options.padding + 'px';
```
이 부분을 응용하여 할 수 있다.
나는 데이터가 0이어서 차트가 표출되지 않을 때에도 툴팁에서 데이터는 0이라고 나오게 하고 싶은데 이 상태로 한다면 0일 경우에는 툴팁이 차트의 하단에 붙어서 표출이 되는 이슈가 있었다.


### 2. custom tooltip 위치 지정하기

```js
tooltipEl.style.top = tooltip.caretY === chart.chartArea.bottom ? '' : positionY + tooltip.caretY - tooltip.height + 'px';
tooltipEl.style.bottom = tooltip.caretY === chart.chartArea.bottom ? chart.chartArea.height / 2 + 'px' : '';
```
그래서 열심히 찾아본 결과 chart 안에 chartArea를 살펴보면 
![](https://velog.velcdn.com/images/yxxnhx/post/1be4fb13-8fd0-4ff9-b051-4388a214ad4d/image.png)
이와 같이 chart 영역에 대한 데이터를 받아볼 수 있다.
그래서 0일 경우에는 chart 영역의 반에 위치하여 툴팁이 나올 수 있게 조건을 수정해주었다.

### 3. 조건에 따른 tooltip 데이터 구성하기
이 이외에도 externalTooltipHandler 함수에서 각각의 데이터에는 다른 조건의 타이틀이 들어가게 하고 싶다면 이 방법을 해볼 수도 있다.
``tooltip.body.map(b => b.lines)``이 기존에 데이터 설정을 할 때 사용했던 dataset의 data이다.
만약 두 개의 데이터를 한꺼번에 표출하는 툴팁을 그려내고 싶다면
```js
if (i === 0) {
  const emitCount = data[dataIndex].EMIT_count;
  const text = document.createTextNode(`공급: ${emitCount}건`);
  td.appendChild(span);
  td.appendChild(text);
} else if (i === 1) {
  const supplyCount = data[dataIndex].SUPPLY_count;
  const text = document.createTextNode(`배출: ${supplyCount}건`);
  td.appendChild(span);
  td.appendChild(text);
}
```
![](https://velog.velcdn.com/images/yxxnhx/post/5fd4c124-9c5f-4b3c-82ba-37a59ed79b3b/image.png)
와 같이 ``index``를 이용해서 보여줄 수 있다.


#### 참고
공식문서: https://www.chartjs.org/docs/latest/samples/tooltip/html.html
