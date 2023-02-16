# git hub

- cd : 위치이동
- dir: 해당 폴더안에 파일들 보기 (mac인 경우 ls)
- mkdir: 새 폴더 만들기
- code : 비쥬얼스튜디오 여는 커맨드

1. Git 설치하기 : [https://git-scm.com/](https://git-scm.com/)
2. 설치 완료 후 Git bash 열기
3. git bash 에서 환경설정 하기
   - Step 1 : 유저이름 설정
4. `git config --global user.name "your_name"`
   - Step 2 : 유저 이메일 설정하기
5. `git config --global user.email "your_email"`

   Github가입시 사용한 이메일을 써주세요!

   - Step 3 : 정보 확인하기

6. `git config --list`

#

# **Github에 처음 코드 업로드하기** 🏋️‍♂️

1. 초기화

   ```
   git init

   ```

2. 추가할 파일 더하기

   ```
   git add .

   ```

   **_._(점)** 은 모든 파일이라는 뜻

   선택적으로 올리고 싶으면 add뒤에 파일 이름 붙여주면 된다 (예. git add index.html)

3. 상태 확인 (선택사항)

   ```
   git status

   ```

4. 히스토리 만들기

   ```
   git commit -m "first commit"

   ```

   - m 은 메세지의 준말로 뒤에 “” 안에 주고싶은 히스토리 이름을 입력하면 된다

   즉, 꼭 first commit일 필요가 없다는 뜻^^

5. Github repository랑 내 로컬 프로젝트랑 연결

   ```
   git remote add origin [본인 주소]

   ```

   이 명령어는 github에서 복사해서 붙여와야함

6. 잘 연결됬는지 확인 (선택사항)

   ```
   git remote -v

   ```

   내가 연결한 주소값이 잘 뜨면 성공!🎇

7. Github로 올리기

   ```
   git push origin master

   ```

   master 자리에는 branch이름이 들어가면 됨 branch이름이 main라하면 git push origin main 이라고 써야함

# **Github에 계속 업데이트 하는법** 🤹‍♂️

1. 추가할 파일 더하기

   ```
   git add .

   ```

2. 히스토리 만들기

   ```
   git commit -m "first commit"

   ```

3. Github로 올리기

   ```
   git push origin master

   ```

내 컴퓨터에 소스코드를 업데이트를 하고 싶으면 이 **세개의 스텝만** 계속 반복하면 됨.

### Remote origin already exists 오류 뜰 때

1. git remote remove origin

→ 연결된 저장소 끊기

1. git remote add origin [새롭게 연결할 깃 레파지토리 주소] 명령어를 입력

### **src refspec master does not match any 오류 뜰 때**

```
git init
git branch -m main
git remote add origin "github.com/your_ropo.git"
git add .
git commit -m "first commit"
git push -u origin main
```

→ 터미널로 해당 프로젝트 폴더로 이동해서 넣기

```jsx
git init
```

깃을 초기화해준다 → 깃 저장소를 만든다는 의미

```jsx
shift + command + .
```

→ 숨김파일 보임

```jsx
git config --global user.name
git config --global user.email
```

저장하는 공간이 3개임 → working, staging, git 디렉토리

파일을 추적(트래킹)하게 해주고 싶으면 staging 상태로 올라가야함

버전을 올려놓으면 git 디렉토리 상태로 올려줘야함

파일 옆에 U자가 있으면 아무것도 없는 상태임

하나의 파일만 깃에 올릴 때는 git add 파일명 입력해주면 됨

두개의 파일 올릴 떄 git add b.html c.html

파일 옆에 A가 나오면 트래킹 되고 있는 상태임

- **현재 깃의 상태가 어떤지 알아보는 것**

```jsx
git status
```

→ 파일을 수정하고나서 올려주지 않으면 다시 언트래킹 상태가 됨

- **파일이 어떻게 수정됐는지 보기**

```jsx
git diff
```

- **확장자별로 파일을 올려주고 싶을 때**

```jsx
git add
```

_.확장자 (예를 들면 git add _.css 이런식으로)

→ commit을 해주면 디렉토리로 감

```jsx
git commit -m " "
```

→ "" 안에 메세지를 입력하면 됨

리드미파일, 이모트 레파지토리에 push까지

### **마크다운 문법**

[MarkDown 사용법 총정리](https://heropy.blog/2017/09/30/markdown/)

→ 블로그 참조

- .gitignore

→ 무시하고 가는 파일

- **git log**를 하면 commit 누가 몇시에 작성했는지 기록이 보이고, 메세지가 보임

```jsx
commit 53e23093a1d5250617626779196114ab3fedd3e7 (HEAD -> master)
```

→ commit 뒤에 있는 건 고유의 # 값임

git log 하고나서 다른 거 입력하려면 ': 랑 q' 눌러야 함

```jsx
git log --oneline
```

→ 로그를 한줄로 약식으로 보여줌

**commit은 history를 스냅샷으로 남겨두는 것이라고 생각하자**

- **파일 수정하고나서 다시 강제로 업로드 하는 방법**

```jsx
git push --force
```

→ 자주 쓰면 좋지 않음

- **파일 수정하고나서 다시 업로드하는 방법**

```jsx
git commit -m 'update a.html'
git push
```

→ 리액트는 ignore랑 readme를 깔지 않아도 기본으로 깔려 있음

html, css 파일은 ignore와 readme 를 만들고 싶으면 따로 만들어줘야함
