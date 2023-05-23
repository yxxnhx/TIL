# sveltekit

[SvelteKit](https://kit.svelte.dev/)

### load

next.js의 getServerSideProps, getStaticProps와 매우 유사하다.

단, load는 클라이언트, 서버 모두 위에서 작동한다는 차이점이 있다.

load를 활용하여 fetch를 할 경우 세 가지 특징이 있다.

- 서버의 쿠키를 access할 수 있다.
- HTTP 호출 발생하지 않고 앱의 자체 포인트에 대해 요청할 수 있다.
- 사용자의 hydration을 위해 response의 복사본을 만들어 초기 페이지 로드할 때 사용한다.
