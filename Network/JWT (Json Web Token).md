# JWT (Json Web Token)

> Json 포맷을 이용하여 사용자에 대한 속성을 저장하는 Claim 기반의 Web Token
>
> 모바일이나 웹의 사용자 인증을 위해서 사용하는 암호화된 토큰을 의미



JWT(Json Web Token)란 Json 포맷을 이용하여 사용자에 대한 속성을 저장하는 Claim 기반의 Web Token이다.

JWT는 토큰 자체를 정보로 사용하는 Self-Contained 방식으로 정보를 안전하게 전달한다.

주로 회원 인증이나 정보 전달에 사용되는 JWT 는 아래의 로직을 따라서 처리된다.

![img](https://blog.kakaocdn.net/dn/rdboS/btqArUrgcMr/HWY80zNL9reAv6FeE6AYE1/img.png)

앱이 실행될 때, JWT를 static 변수와 로컬 스토리지에 저장하게 된다.

Static 변수에 저장되는 이유는 HTTP 통신을 할 때마다 JWT를 HTTP 헤더에 담아서 보내야 하는데, 이를 로컬 스토리지에서 계속 불러오면 오버헤드가 발생하기 때문이다.

클라이언트에서 JWT를 포함해 요청을 보내면 서버는 허가된 JWT 인지를 검사한다.

또한 로그아웃을 할 경우 로컬 스토리지에 저장된 JWT 데이터를 제거한다.



## 구조

> Header, Payload, Signature 3부분으로 이루어진다.

세 파트로 나누어지고, 각 파트는 `.` (점) 으로 구분하여 aaaaa.bbbbbb.cccccc 이런식으로 표현된다.

JWT는 URL 에서 파라미터로 사용할 수 있도록 URL_Safe 한 Base64url 인코딩을 사용한다.



```
curl http://127.0.0.1:8000/toktokhan/ -H 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c

header = eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
payload = eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ
signature = SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```



위의 값을 디코딩

### header

토큰 타입과 해시 암호화 알고리즘으로 구성

```json
{
  "typ" : "JWT",
  "alg" : "HS256"
}
```



### payload

토큰에 담을 정보가 들어있다.

여기에 담은 정보의 한 '조각'을 클레임(claim)이라 부르고, 이는 name/value 의 한 쌍으로 이뤄져있다.

클레임의 종류는 등록된 클레임 (registered), 공배 클레임 (public), 비공개 클레임(private)이  존재한다.

```json
{
  "sub": "1234567890", // 등록된 플레임
  "name": "John Doe", // 비공개 플레임
  "iat": 1516239022  // 등록된 플레임
}
```



### signature

서명은 [헤더 base64 + 페이로드 base64 + SECRET_KEY]를 사용하여 JWT 백엔드에서 발행된다.

각 요청시 서명이 확인된다.

헤더 또는 페이로드의 정보가 클라이언트에 의해 변경된 경우 서명이 무효화 된다.



## 장점

- 무상태 (stateless). 확장성이 있다. 기존 서버에 세션을 저장하는 방식에서 서버 여러대를 사용하여 요청을 분산하였다면 어떤 유저가 로그인했을 때 그 유저는 처음 로그인한 서버에만 요청을 내보내도록 설정해야 한다. 하지만 토큰을 사용하면 토큰 값만 알고 있다면 어떤 서버로 요청이 들어가던 상관이 없다. 즉, 세션 스토리지가 필요없다.
- 보안성 쿠키를 전달하지 않아도 되므로 쿠키를 사용함으로써 발생하는 취약점이 사라진다.
- 여러 플랫폼 및 도메인 어플리케이션 규모가 커지면 여러 디바이스를 호환 시키고 더 많은 종류의 서비스를 제공하는데, 토큰을 사용하면 그 어떤 디바이스에서도 그 어떤 도메인에서도 토큰만 유효하다면 요청이 정상적으로 처리된다.



## 단점

- 길이 claim에 넣는 데이터가 많아질 수록 JWT 토큰이 길어진다. API 호출 시 매 호출마다 토큰 데이터를 서버에 전달해야 하는 데 길이가 길다는 것은 그만큼 네트워크 대역폭 낭비가 심할 수 있다.
- 보안 JWT는 기본적으로 Payload에 대한 정보를 암호화 하지 않는다. 단순히 BASE64로 인코딩만 하기 때문에 중간에 패킷을 가로채거나 기타 방법으로 토큰을 취득했으면 디코딩을 통해 데이터를 볼 수 있다. 그래서 JWE(JSON Web Encryption) 을 통해 암호화 하거나 중요데이터를 Payload에 넣지 말아야 한다.



---

참고사이트 

https://tech.toktokhan.dev/2021/04/30/JWT/

https://mangkyu.tistory.com/56

https://inpa.tistory.com/entry/WEB-%F0%9F%93%9A-JWTjson-web-token-%EB%9E%80-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC