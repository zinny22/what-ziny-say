# CORS 개념과 해결 방법

***CORS(Cross-Origin Resource Sharing)*** 에러는 브라우저의 보안 정책 때문에 발생합니다.

> CORS는 브라우저가 서로 다른 출처간의 요청을 제한하기 때문에 발생합니다.
> 보안상의 이유로, 의도하지 않은 외부 요청을 막기 위한 정책 입니다.
<br/>

## 💡 출처(origin)는?

브라우저가 보안상 동일한 위치로 간주하는 기준이며, 세가지 요소로 구성됩니다: 
> 프로토콜 + 도메인(호스트) + 포트번호

### 🔎 예시

| 주소(URL) | origin |
|------|------|
| https://www.example.com | https://www.example.com(port 생략시 443)|
| http://example.com:8080/dashboard | http://example.com:8080 |
| http://example.com:8888 | http://example.com:8888 |
 
<br/>

## ⁉️ 출처는 왜 중요할까

브라우저는 보안을 위해서 **다른 origin끼리는 자바스크립트로 접근하거나, 요청을 제한**합니다.<br/>
이를 Same-Origin Policy(동일 출처 정책)라고 부릅니다.

> 다른 출처의 API에 요청을 보내려면 서버가 COSR를 허용해주어야 합니다.
<br/>

## 그럼 CORS는 언제 발생할까?
```
프론트-> http://example.com:8888
백엔드 -> http://example.com:4000
```
- 두 URL은 origin이 다르기 때문에 브라우저는 CORS검사를 진행합니다.
- 만약 서버가 Access-Control-Allow-Origin 헤더를 주지 않으면 브라우저는 요청을 차단합니다.

<br/>

## ✅ CORS 해결 방법

1. 서버에서 CORS 허용 헤더를 설정한다.
   ```
   Access-Control-Allow-Origin: http://localhost:3000
   Access-Control-Allow-Credentials: true

   // 예: Express + CORS 미들웨어
   import cors from "cors";

   app.use(cors({
    origin: "http://localhost:3000",
    credentials: true
   }));
   ```
2. 모든 출처를 허용한다.
   ```
   app.use(cors({
    origin: "*"
   }));
   ```
   > 보안상 주의가 필요하고, 인증 정보는 전달되지 않습니다.
3. 프록시 서버를 이용하기
   ```
   // vite.config.ts 예시
   server: {
    proxy: {
      '/api': 'http://localhost:4000'
      }
    }
   ```
   - 개발 환경에서는 프론트 서버에서 API요청을 프록시로 우회해 <br/>
     같은 origin에서 요청이 발생한 것 처럼 처리 할 수 있습니다.
<br/>

## 🗣️ What-ziny-say
CORS는 브라우저가 사용자의 데이터를 보호하기 위해서<br/>
**서로 다른 출처의 요청을 제한**하는 보안 정책입니다.

개발 중에는 특히나 프론트와 백엔드를 다른 포트로 띄워서 작업하는 경우가 많아<br/>
CORS에러를 자주 마주하게 되었습니다.<br/>
처음엔 프론트 코드에 문제가 없어 보이는데 에러가 발생해서 당황했지만,
알고보니 이는 보안상으로 중요한 정책이였습니다.

특히 의도하지 않은 도메인 간 요청을 차단해 사용자 세션 탈취나,<br/>
데이터의 유출을 막는 중요한 역할을 한다는 것을 알게되었습니다.

CORS는 **브라우저 차원의 정책**이기 때문에,<br/>
프론트에서 단순하게 해결할 수는는 없습니다.<br/>
따라서 서버쪽에서 CORS 허용 관련 헤더를 설정하거나,<br/>
개발 중에는 프론트 서버에서 **프록시 설정을 통해 우회하는 방법**을 사용하기도 했습니다.

초기에 사이드 프로젝트를 할때 CORS로 고생한적이 있는데,<br/>
그 당시에는 프론트 서버에서 프록시 처리해 <br/>
**같은 origin에서 요청이 발생한것 처럼** 보이도록 우회하여 문제를 해결했었습니다

이후에는 서버단에서 CORS 설정을 명시해 주는 것이<br/>
더 안정적이라는 걸 알게 되었습니다.


