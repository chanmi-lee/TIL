## HTTP Message

- HTTP Message는 서버와 클라이언트 간에 데이터가 교환되는 방식
- 메시지 타입은 두 가지로, 클라이언트로부터의 요청 (`request`)이나 서버로부터의 응답(`response`)이다

![](https://mdn.mozillademos.org/files/13827/HTTPMsgStructure2.png)

- 메시지는 `시작줄 (start-line)`, `헤더 블록(HTTP headers)`, `본문(body)` 세 부분으로 이루어진다.
  - 시작줄은 이것이 어떤 메시지인지 서술하며, 헤더 블록은 속성을, 본문은 데이터를 담고 있으며 optional하다.

---

#### HTTP Request

```
<메서드> <요청URL> <버전>
<헤더>

<본문>
```

**start-line**

- HTTP 메서드: 서버가 수행해야 할 동작을 나타냄 (ex- GET, POST, PUT, HEAD, OPTIONS)
- 요청 URL: HTTP 메소드에 따라 포맷이 달라진다.
  - origin 형식: 끝에 `?`와 쿼리 문자열이 붙는 절대 경로로 `GET`, `POST`, `HEAD`, `OPTIONS` 메서드와 함께 사용
```
POST / HTTP 1.1
GET /background.png HTTP/1.0
HEAD /test.html?query=alibaba HTTP/1.1
OPTIONS /anypage.html HTTP/1.0
```
  - absolute 형식: 프록시에 연결하는 부분 대부분 `GET`과 함께 사용
```
GET http://developer.mozilla.org/en-US/docs/Web/HTTP/Messages HTTP/1.1
```
  - authority 형식: 도메인 이름 및 옵션 포트로 이루어진 형식으로 HTTP 터널을 구축하는 경우에만 `CONNECT`와 함께 사용
```
CONNECT developer.mozilla.org:80 HTTP/1.1
```
  - asterisk 형식: `OPTIONS`와 함께 `*`로 전체를 나타내는 형식
```
OPTIONS * HTTP/1.1
```

**Headers**
![](https://mdn.mozillademos.org/files/13821/HTTP_Request_Headers2.png)
클라이언트와 서버가 요청 또는 응답으로 부가적인 정보를 전송할 수 있도록 하며, 이름과 콜론(`:`) 값으로 이루어져 있으며 대소문자를 구분하지 않는다.

**Entity body**

 요청의 마지막 부분으로 optional하다. `GET`, `HEAD`, `DELETE`, `OPTIONS`처럼 리소스를 가져오는 요청은 보통 본문이 필요없으나, `POST`와 같이 업데이트를 하는 경우 서버에 데이터를 전송한다.

---

#### HTTP Response

```
<버전><상태 코드><사유 구절>
<헤더>

<본문>
```

**status-line**

- 프로토콜 버전: 보통 `HTTP/1.1`
- 상태 코드: 요청의 성공 여부
- 사유 구절 (reason-phrase): 숫자로 된 상태코드의 의미를 사람이 이해할 수 있게 설명해주는 짧은 문구

**Headers**
![](https://mdn.mozillademos.org/files/13823/HTTP_Response_Headers2.png)

**Entity body**

---

Reference

- [MDN: HTTP Message](https://developer.mozilla.org/ko/docs/Web/HTTP/Messages)
- [MDN: HTTP Headers](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers)
