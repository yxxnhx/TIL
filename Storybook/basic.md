# Storybook

리액트로 개발을 하다보면 구성요소를 컴포넌트로 나누어서 상태를 관리하는 일이 많다.

버튼이란 컴포넌트가 많이 이용되니 하나를 만들어서 재사용을 하여 다양한 버튼들을 만들어낸다.

그 외에도 textinput, containe 등 다양한 구성요소들을 컴포넌트로 분리하여 재사용하며 하나의 어플리케이션을 만들어낼 때 매우 유용하게 사용한다.

이 구성요소들을 조금 더 체계적으로 관리하고 테스트할 때 좋은 스토리북에 대하여 알아보자

### 1. 스토리북이란 무엇일까?

UI 컴포넌트를 독립적으로 분리해 개별 관리, 테스트를 도와주는 도구이다.

### 2. 그렇다면 왜 사용할까?

리액트 내에서도 충분히 분리하고 관리할 수 있는데 왜 사용할까?

- 개별 정리 정돈 편리하다
- 재사용성을 고려한 디자인과 개발이 가능하다
- 테스트에 용이하다

### +) 재사용성의 필요성

그렇다면 왜 재사용을 해야할까?

<개발자의 관점>

- 불필요한 작업을 줄여준다
- 안정적으로 검증된 코드를 사용할 수 있다
- 캡슐화를 통해 테스트가 쉬워진다

<디자이너의 관점>

- 디자인 변경 시 쉽게 수정이 가능하다
- 사용자에게 일관된 경험을 제공한다
- 개발자와 소통이 쉬워진다

<기타 장점>

- 모던 웹의 구성 요소들에게 익숙해진다
- 재사용성을 고려한 효율적 업무가 가능하다
- 기획과 업무의 진행이 빨라진다

## 그렇다면 직접 한번 사용해보자

### 1. storybook 설치

React app에 설치하기

```jsx
npx sb init
```

### 2. storybook 실행

```jsx
# Storybook 구동
npm run storybook

# Storybook 빌드
npm run build-storybook
```

### 3. storybook 사용해보기

먼저 간단하게 버튼 컴포넌트를 만들어보자

src/component/Button.tsx 생성

```tsx
export interface ButtonProps {
  clickHandler: () => void;
  label: string;
  size: 'sm' | 'md' | 'lg';
  backgroundColor: string;
  color: string;
}

function Button({
  label,
  backgroundColor = 'red',
  size = 'md',
  color,
  clickHandler,
}: ButtonProps) {
  let scale = 1;
  if (size === 'sm') scale = 0.75;
  if (size === 'lg') scale = 1.55;

  const style = {
    backgroundColor,
    padding: `${scale * 0.5}rem ${scale * 1}rem`,
    border: 'none',
    color,
  };

  return (
    <button onClick={clickHandler} style={style}>
      {label}
    </button>
  );
}

export default Button;
```

src/stories/Button.stories.tsx 생성

```tsx
import Button, { ButtonProps } from '../../components/Button';
import { Meta, Story } from '@storybook/react';

export default {
  title: 'Button',
  component: Button,
  argTypes: { clickHandler: { action: 'clicked' } },
} as Meta;

const Template: Story<ButtonProps> = (args) => <Button {...args} />;

export const Red = Template.bind({});
Red.args = {
  label: 'Red',
  backgroundColor: 'red',
  size: 'md',
  color: 'white',
};

export const Blue = Template.bind({});
Blue.args = {
  label: 'Blue',
  backgroundColor: 'blue',
  size: 'md',
  color: 'white',
};
```

스토리북이 작동하는 원리는 컴포넌트를 메인 프레임으로 잡고 그 컴포넌트를 바탕으로 export해서 표현하는 것이다.

현재의 예시에서는 버튼 컴포넌트를 하나 메인 프레임으로 만들어준 이후에 그 버튼 컴포넌트를 바탕으로 새로운 red, blue로 원하는 컴포넌트를 export해서 버튼 컴포넌트를 통해서 표현하는 것이다.

**조금 더 자세히 살펴보자.**

```jsx
*import* Button, {ButtonProps} *from* '../../components/Button';
```

먼저 Button 컴포넌트와 ButtonProps를 만들어두었다.

타입스크립트를 사용하기 때문에 interface를 만들어둔 것이다.

```tsx
export interface ButtonProps {
  clickHandler: () => void;
  label: string;
  size: 'sm' | 'md' | 'lg';
  backgroundColor: string;
  color: string;
}
```

인터페이스와 버튼 컴포넌트를 함께 먼저 받아온다.

**+) 여기에서 인터페이스란?**

인터페이스는 상호 간에 정의한 약속 혹은 규칙을 의미한다. 타입스크립트에서의 인터페이스는 다음의 예시와 같은 범주에 대해 약속할 수 있다.

- 객체의 속성과 속성의 타입
- 함수의 파라미터
- 함수의 파라미터와 반환 타입
- 배열과 객체를 접근하는 방식
- 클래스

**그 다음을 살펴보자**

```jsx
*import* {Meta, Story} *from* '@storybook/react';
```

Meta와 Story는 스토리북에 있는 요소이다.

이 요소들은 타입스크립트를 위해 받아온 것이다.

```tsx
export default {
  title: 'Button', // title은 Button
  component: Button, // 사용할 컴포넌트는 Button이다
  // 위 두가지는 stories.tsx의 기본 필수 요소이다.
  argTypes: { clickHandler: { action: 'clicked' } },
  // argTypes는 추가적으로 함수를 추가해준 것이다. 그 함수의 action은 cliked이다.
  // 만약 함수를 더 추가적으로 해주고 싶다면 그 뒤에 추가해주면 된다.
} as Meta;

const Template: Story<ButtonProps> = (args) => <Button {...args} />;
```

만약 일반 자바스크립트를 사용할 시에는 사용할 필요는 없다.

```tsx
const Template: Story<ButtonProps> = (args) => <Button {...args} />;

export const Red = Template.bind({});
Red.args = {
  label: 'Red',
  backgroundColor: 'red',
  size: 'md',
  color: 'white',
};

export const Blue = Template.bind({});
Blue.args = {
  label: 'Blue',
  backgroundColor: 'blue',
  size: 'md',
  color: 'white',
};
```

그리고 원래는 그냥 템플릿을 만들지 않고 바로 args에 label, backgroundColor 등 속성들을 넣어서 바로 생성할 수 있지만 동일한 버튼에 blue와 red 버튼을 만들려면 계속해서 동일한 코드를 반복해야하기 때문에 Template을 만들어서 불필요한 코드 반복을 없앴다.
