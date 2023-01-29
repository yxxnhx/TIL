# Storybook 배포하기

우리가 협업을 할 때 만든 컴포넌트들을 팀원들과 공유하기 위해서는 배포는 필수이다!

이제 한번 공식 문서를 참고하여 배포를 해보는 시간을 가져보자.

[Storybook Tutorials](https://storybook.js.org/tutorials/intro-to-storybook/react/en/deploy/)

배포를 할 때 사용할 툴은 크로마틱 chromatic이다.

크로마틱은 스토리북을 배포할 때 도와주는 툴이다.

[Automatically review, test, and document Storybook](https://www.chromatic.com/)

먼저 준비해야 할 것은 스토리북 파일을 깃허브 레파지에 업로드 해두어야 한다.

그 후 이제 차근차근히 진행하여보자.

### 1. chromatic 설치하기

```jsx
//yarn
yarn add -D chromatic

//npm
npm install -D chromatic
```

### 2. chromatic과 깃허브 연동하기

크로마틱에 가입이 되어있지 않은 사람들은 가입을 하여 연동을 하고 기존 아이디가 있는 경우 레파지를 선택하여 연동하면 손쉽게 연동할 수 있다.

### 3. 토큰을 받아 프로젝트와 연동하기

```jsx
//yarn
yarn chromatic --project-token=<project-token>

//npx
npx chromatic --project-token=<project-token>
```

프로젝트마다 토큰이 다르기 때문에 크로마틱에서 주는 토큰을 받아 그대로 넣으면 된다

여기서 문제는 하나 이상의 커밋이 있어야만 된다는 에러가 뜰 것이다.

그 이유는 이전에 push 해둔 것은 크로마틱이 설치되기 전이기 때문에 지금 그 레파지와 연동을 할 수가 없어 뜨는 에러이다. 그러니 크로마틱을 설치한 후에 다시 한번 더 push 후 시도해보면 문제가 깔끔히 해결될 것이다!

빌드가 완료되면 터미널에 Build passed! 라는 글과 함께 크로마틱의 링크가 생성될 것이다.

그 링크로 들어가면 빌드된 내역들을 쭉 볼 수 있고 view storybook으로 들어가 그 링크를 팀원들에게 공유하면 된다.

그런데 우리는 또 하나의 욕심이 생길 것이다

내가 깃허브에 push하면 바로 배포되었으면 좋겠다는 욕심!

한번 연동 작업을 해보자

### 4. push 후 바로 배포 설정하기

이 설정은 github action을 이용하면 쉽게 해결할 수 있다.

push를 하자마자 바로 스토리북에서 배포가 될 수 있게 한번 설정해보자

[Storybook Tutorials](https://storybook.js.org/tutorials/intro-to-storybook/react/en/deploy/)

```
# .github/workflows/chromatic.yml

# Workflow name
name: 'Chromatic'

# Event for the workflow
on: push

# List of jobs
jobs:
  chromatic-deployment:
    # Operating System
    runs-on: ubuntu-latest
    # Job steps
    steps:
      - uses: actions/checkout@v1
      - name: Install dependencies
        # 👇 Install dependencies with the same package manager used in the project (replace it as needed), e.g. yarn, npm, pnpm
        run: yarn
        # 👇 Adds Chromatic as a step in the workflow
      - name: Publish to Chromatic
        uses: chromaui/action@v1
        # Chromatic GitHub Action options
        with:
          # 👇 Chromatic projectToken, refer to the manage page to obtain it.
          projectToken: ${{ secrets.CHROMATIC_PROJECT_TOKEN }}
```

후에 깃허브 레파지>세팅>Secrets and variables>Actions>Repository secrets의 본인 프로젝트 토큰을 넣어주면 자동으로 설정이 된다!

여기에서 설정을 하면 계속해서 빌드에서 에러가 나는 것을 확인할 수 있다.

```yaml
# Workflow name
name: 'Chromatic Dev'

on:
  push:
    branches:
      - dev

defaults:
  run:
    working-directory: buoy-workspace

jobs:
  chromatic-deployment:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v3
        with:
          fetch-depth: 0
      - name: Install dependencies and build
        run: |
          npm ci
          npm run build-buoy
      - name: Publish to Chromatic
        uses: chromaui/action@v1
        with:
          workingDir: buoy-workspace
          projectToken: ${{ secrets.CHROMATIC_PROJECT_TOKEN }}
          exitZeroOnChanges: true
```

으로 수정해주면 정상적으로 되는 것을 확인할 수 있다.
