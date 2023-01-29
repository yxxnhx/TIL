# Storybook 커스텀 폰트 적용하기

먼저 폰트를 지정해주자.

index.html

```html
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
<link
  href="https://fonts.googleapis.com/css2?family=Unbounded&display=swap"
  rel="stylesheet"
/>
```

→ 사용할 폰트를 구글 폰트에서 링크로 가져왔다.

Button.tsx

```tsx
const style = {
  backgroundColor,
  padding: `${scale * 0.5}rem ${scale * 1}rem`,
  border: 'none',
  color: 'white',
  fontFamily: 'Unbounded',
};
```

→ 스타일에 폰트 패밀리를 추가해주었다

이제 간단하게 로컬에서 확인할 수 있도록 App.tsx에 버튼을 출력해보자.

```tsx
import './App.css';
import Button from './components/Button';

function App() {
  return (
    <>
      <Button label="Red" backgroundColor="red" />
    </>
  );
}

export default App;
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7831e3b8-6fff-4bf1-b6e3-12843496bf0b/Untitled.png)

로컬에서는 정상적으로 Unbounded 폰트가 제대로 반영되어 출력된 것을 확인할 수 있지만 스토리북에서는 이 폰트 대신에 기본 폰트가 출력되는 것을 확인할 수 있다.

왜일까?

[Story rendering](https://storybook.js.org/docs/react/configure/story-rendering)

스토리북에서는 폰트 링크가 지정이 되어있지 않아 일어난 일이다.

리액트에서 index.html이 있듯이 스토리북에서도 preview-head.html에도 동일하게 링크를 추가해주면 문제는 깔끔하게 해결이 된다.
