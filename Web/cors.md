# Cors

면접에서 cors가 무엇인지, 대응 방안이 어떻게 되는지에 대해 질문을 받았다.
애매모호하게 헛소리를 신나게 해서 다시 공부하라는 조언을 받고 다시 정리해본다.

# CORS (Cross-Origin Resource Sharing)

나는 Next.js로 토이프로젝트를 진행하던 중, 이 이슈를 만나게 되었다.
당시 rewrites를 활용해서 url 주소를 마스킹해서 API 키를 숨기게 설정해서 사용했었다.
그러고 vercel을 이용해서 배포를 하고 기능을 추가하면서 사용하던 중에 난데없이 에러를 만나게 되었다.

> 🚨 Access to fetch at ‘https://api.lubycon.com/me’ from origin ‘http://vercel주소’ has been blocked by CORS policy: No ‘Access-Control-Allow-Origin’ header is present on the requested resource. If an opaque response serves your needs, set the request’s mode to ‘no-cors’ to fetch the resource with CORS disabled.

처음 봤을 때 어리둥절해서 잠시 눈물 한번 찍어내고 당황해서 vercel 주소로 변경해두었던 걸 localhost로 돌려놨더니 얼렁뚱땅 해결되길래 흐린눈 하면서 보냈다.

그랬으면 안됐는데😢

이제라도 한번 제대로 알아보고 정리해보자.

## CORS란 무엇인가?

일단 요청이 실패해서 뜨는 에러인건 알겠는데 뭘까?
![](https://velog.velcdn.com/images/yxxnhx/post/471e850b-0f10-469f-bc2e-feffdb632163/image.png)
요청이 실패했는지 알기 위해선 **도메인, 프로토콜, 포트 세 가지에 의해 결정되는 오리진(origin)** 이라는 핵심부터 알아야 한다.

- Protocol(Scheme) : http, https
- Host : 사이트 도메인
- Port : 포트 번호
- Path : 사이트 내부 경로
- Query string : 요청의 key와 value값
- Fragment : 해시 태그

도메인이나 서브도메인, 프로토콜, 포트가 다른 곳에 요청을 보내는 것을 Cross-Origin Request(크로스 오리진 요청)이라고 한다.

## SOP Same Origin Policy

**다른 출처의 리소스를 사용하는 것에 제한하는 보안 방식**
URL의 프로토콜, 호스트, 포트를 통해 같은 출처인지, 다른 출처인지 판단할 수 있다.
이 셋 중에 하나라도 다르면 다른 출처라고 판단하는 것이고, 이 세 가지가 모두 같아야만 같은 출처라고 이야기한다.

출처, 즉 오리진에 대해 알아보았으니 Same Origin 정책과 Cross Origin 정책을 알아보자.
SOP는 말 그대로 동일 출처에 대한 정책을 말한다. SOP 정책은 '동일한 출처에서만 리소스를 공유할 수 있다.'라는 법률을 가지고 있다.
즉, 동일 출처(Same-Origin) 서버에 있는 리소스는 자유로이 가져올수 있지만, 다른 출처(Cross-Origin) 서버에 있는 이미지나 유튜브 영상 같은 리소스는 상호작용이 불가능하다는 말이다.

### 그렇다면 여기서 문제

**다음 중 http://localhost 와 동일 출처인 URL은 무엇일까?**

```
① https://localhost
② http://localhost:80
③ http://127.0.0.1
④ http://localhost/api/cors
```

**바로 정답은 ②, ④번이다.**
`① https://localhost `
→ 프로토콜이 다르므로 다른 출처이다.
`② http://localhost:80 `
→ http 기본 포트는 80이므로 동일 출처이다.(문제에서는 생략되었다)
`③ http://127.0.0.1 `
→ 이건 왜 정답이 아닐까?
127.0.0.1의 IP는 localhost인데 왜 다른 출처일까?
브라우저 입장에서는 string value를 비교를 해서 127.0.0.1과 localhost는 서로 다른 출처라고 판단하는 것이다.
`④ http://localhost/api/cors `
→ 프로토콜과 호스트가 모두 동일하므로 동일 출처이다.

크로스 오리진 요청을 보내려면 리모트 오리진에서 전송받은 특별한 헤더가 필요하다.
이러한 정책을 **'Cross-Origin Resource Sharing, 크로스 오리진 리소스 공유'** 라고 부른다.

참고자료 출처:
https://ko.javascript.info/fetch-crossorigin
https://www.bannerbear.com/blog/what-is-a-cors-error-and-how-to-fix-it-3-ways/
https://www.youtube.com/watch?v=-2TgkKYmJt4
https://inpa.tistory.com/entry/WEB-%F0%9F%93%9A-CORS-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC-%ED%95%B4%EA%B2%B0-%EB%B0%A9%EB%B2%95-%F0%9F%91%8F
