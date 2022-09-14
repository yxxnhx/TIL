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
