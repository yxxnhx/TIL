# Storybook Figma와 연동하기

스토리북과 피그마를 연동하는 방법은 총 두 가지가 있다.

- 피그마에서 스토리북 보기
  - 피그마에서 제공되는 플러그인 이용
- 스토리북에서 피그마 자료 보기
  - 스토리북에서 제공되는 addon을 이용

### 1. 피그마에서 스토리북 보기

피그마에서 스토리북을 볼 수 있게 되면 개발자가 더이상 따로 무언가를 하지 않아도 디자이너가 본인의 작업물이 어떻게 구현되었는지 확인할 수 있게 되어 협업 시에 편리하다.

**단, 주의해야 할 점은 chromatic으로 배포된 storybook이 있어야만 연동이 가능하다!**

프로세스를 간단하게 정리하자면 다음과 같다

chromatic으로 story 배포 > figma에서 플러그인 다운로드 > chromatic 계정 인증 > figma 컴포넌트와 연결

여기서 주의해야할 점은 컴포넌트화된 작업물만 스토리북과 연동이 가능하다는 점이다.

먼저 간단하게 피그마에서 버튼 컴포넌트를 생성한다.

그 이후에 Storybook Connect 플러그인을 다운 받는다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6fcc1379-bf87-48d4-8e46-5c731dc1781e/Untitled.png)

그 이후 chromatic과 연결한 후 연동하고자 하는 컴포넌트의 링크를 연결해주면 끝!

대신에 컴포넌트 하나하나 연결을 해주어야 한다.

연결을 완료하면 아래와 같이 우측 하단에 스토리를 볼 수 있거나 스토리북 링크로 들어가 다른 스토리들을 볼 수도 있다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6b88fc2a-3530-4e21-aeb7-73da40ae9d5a/Untitled.png)

### 2. 스토리에서 피그마 보기

피그마에서 완성된 작업물을 스토리에서 출력하게 하는 것이다.

그렇게 하면 개발자는 본인의 구현물과 디자인을 비교하며 작업이 가능해진다.

일일히 피그마에서 하나하나 확인하면서 작업할 필요가 없어지는 것이다.

피그마 작업물과 storybook을 연결하기 위해선 Design Addon이 필요하다.

Design Addon은 storybook의 플러그인이라고 생각하면 쉽다.

**그럼 한번 세팅해보자!**

- 설치하기

```jsx
npm install --save-dev storybook-addon-designs
```

- module 설정하기

.storybook>main.js

```
module.exports = {
  addons: ['storybook-addon-designs'],
}
```

addons의 설정에 'storybook-addon-designs' 추가하기

- story 파일에 추가해주기

```jsx
import { withDesign } from 'storybook-addon-designs';

export default {
  title: 'My stories',
  component: Button,
  decorators: [withDesign],
};

export const myStory = () => <Button>Hello, World!</Button>;

myStory.parameters = {
  design: {
    type: 'figma',
    url: 'https://www.figma.com/file/LKQ4FJ4bTnCSjedbRpk931/Sample-File',
  },
};
```

→ 본인이 import 하고 싶은 요소에 파라미터를 추가하여 링크를 연결해주면 끝

모든 설정을 마친 후에 npm run storybook을 하면

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/27c3ec75-905d-44f7-b038-bf53ef7806b8/Untitled.png)

이와 같이 연동이 된 것을 확인할 수 있다.
