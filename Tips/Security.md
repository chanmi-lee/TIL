# Security

---

### Cross-site Scripting (XSS)

: 사용자가 입력한 정보를 출력할 때, 스크립트가 실행되도록 하는 공격 기법

관리자가 아닌 이가 웹 페이지에 악성 스크립트를 삽입하여 다른 사용자들의 정보(쿠키, 세션 등)를 탈취하거나, 자동으로 비정상적인 기능을 수행하게 할 수 있다

주로 다른 웹 사이트와 정보를 교환하는 식으로 작동하므로 크로스-사이트 스크립팅이라 한다

> 유형

1) **저장 XSS 공격**

: 웹 서버에 공격용 스크립트를 저장시켜두어 방문자가 해당 스크립트가 삽입되어 있는 페이지를 읽는 순간 방문자의 브라우저를 공격하는 방식

2) **반사 XSS 공격**

: 악성 스크립트가 포함된 URL을 사용자가 클릭하도록 유도하여 URL을 클릭하면 클라이언트의 민감한 정보(ID/PW, session 정보 등) 방식

예)
```
http://www.google.com/search/?q=<script>alert(document.cookie)</script>&x=0&y=0
```

3) **DOM 기반 XSS 공격**

: DOM 환경에서 악성 URL을 통해 사용자의 브라우저를 공격하는 방식

저장/반사 XSS 공격의 경우, 서버 측 애플리케이션 취약점으로 인해 응답 페이지에 악성 스크립트가 포함되어 브라우저로 전달되면서 공격하는 것인 반면 해당 경우는 서버와 관계없이 브라우저에서 발생하는 것이 차이점이다


:bulb: How to protect XSS?

`<meta>`의 http-equiv은 http header의 이름을 값으로 가질 수 있다.
해당 속성의 값으로 server나 User agent의 작동 방식을 변경할 수 있다.

- `content-security-policy` 값을 이용해 현재 페이지에 대한 [Content Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy) 을 정의할 수 있다. Content Policy는 허용된 server origin과 script endpoints를 명시하므로써 XSS 공격을 막는것을 돕는다.

ex)

```
<!-- 인라인 JavaScript의 실행과 모든 플러그인을 차단하여 XSS 공격에 대비 -->
<meta http-equiv="Content-Security-Policy" content="script-src 'self'; object-src 'none'">
```

---

### SQL Injection

: 보안 상의 허점을 의도적으로 이용해, 악의적인 SQL문이 실행되게 함으로써 데이터베이스를 비정상적으로 조작하는 코드 인젝션 공격 기법

예) 사용자로부터 아이디와 비밀번호를 입력받아 일치하는 행을 반환하는 SQL문이 있다고 할 때,
```
SELECT * FROM users WHERE username={username} AND password={password}
```
유저 네임에 ```admin``` 패스워드에 ```password OR 1=1 ```을 입력하면 1=1(항상 참)이므로 비밀번호를 입력하지 않고 로그인할 수 있게 된다

혹은 Batched SQL구문을 통해 의도하지 않은 데이터베이스 작업이 수행되게 할 수도 있다.

```
SELECT * FROM users; DROP TABLE users;
```


---

Ref

- [Wiki pedia - Cross-site scripting](https://ko.wikipedia.org/wiki/%EC%82%AC%EC%9D%B4%ED%8A%B8_%EA%B0%84_%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8C%85)
- [생활코딩 - Cross-site scripting](https://opentutorials.org/course/692/3961)
- [KISA - XSS 공격 종류 및 대응 방법](http://www.kisa.or.kr/uploadfile/201312/201312161355109566.pdf)
- [Wiki pedia - SQL Injection](https://ko.wikipedia.org/wiki/SQL_%EC%82%BD%EC%9E%85)
- [W3C - SQL Injection](https://www.w3schools.com/sql/sql_injection.asp)
- [Content Security Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP)
