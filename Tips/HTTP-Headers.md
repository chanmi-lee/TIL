# HTTP Headers

> ### **User-Agent**

contains a characteristic string that allows the network protocol peers to identify 
**the application type, operating system, software vendor or software version** of the requesting software user agent.

[Syntax]

Common format for web browsers: 

`User-Agent: Mozilla/<version> (<system-information>) <platform> (<platform-details>) <extensions>`

*Examples*

- Firefox: broken down into four components, (more details on [here](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent/Firefox)

`Mozilla/5.0 (platform; rv:geckoversion) Gecko/geckotrail Firefox/firefoxversion`
  - `Mozilla/5.0` is the general token that says the browser is Mozilla compatible, and is common to almost every browser today
  - `platform` describes the native platform the browser is running on (e.g. Windows, Mac, Linux or Android), and whether or not it's a mobile phone

- Chrome: similar with FireFox's one but adds strings like "KHTML, like Gecko" and "Safari".

`Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.103 Safari/537.36`

---

> ### **X-Forwarded-For**

identifying the originating IP address of a client connecting to a web server through an HTTP proxy or a load balancer.

[Syntax]

`X-Forwarded-For: <client>, <proxy1>, <proxy2>`

*Examples*

```
X-Forwarded-For: 2001:db8:85a3:8d3:1319:8a2e:370:7348
X-Forwarded-For: 203.0.113.195
X-Forwarded-For: 203.0.113.195, 70.41.3.18, 150.172.238.178
```

X-Forwarded-For를 통해 request client IP 조회하는 메소드

```
private HttpServletRequest request;

public String getRemoteIp() {
  String remoteIp = Optional.ofNullable(request.getRequestHeader(HTTP_HEADER_X_FORWARDED)).orElse(request.getRemoreIp());
  
  return remoteIp;
}
```

---

> ### **Authorization**

contains the credentials to authenticate a user agent with a server, 
usually after the server has responded with a 401 Unauthorized status and the WWW-Authenticate header.

[Syntax]

`Authorization: <type> <credentials>`

- type: [Authentication type](https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication#Authentication_schemes), usually `Basic`, which transmits credentials as user ID/password pairs, **encoded using base64**.
  - **HTTPS / TLS** should be used in conjunction with basic authentication.
- cretentials
  - The username and the password are combined with a colon (e.g. `aladdin:opensesame`).
  - The resulting string is base64 encoded (e.g. `YWxhZGRpbjpvcGVuc2VzYW1l`).

*Example*

`Authorization: Basic YWxhZGRpbjpvcGVuc2VzYW1l`

---

Ref

- [User-Agent](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent)
- [X-Forwarded-For](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Forwarded-For)
- [Authorization](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Authorization)
