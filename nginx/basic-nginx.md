# Nginx

Nginx는 Event-Driven 방식으로 동시접속 처리에 특화된 웹 서버이다.

![](https://mdn.mozillademos.org/files/8659/web-server.svg)

> [Web server : MDN](https://developer.mozilla.org/ko/docs/Learn/Common_questions/What_is_a_web_server)

웹 서버는 정적 파일을 처리하는 HTTP 서버로서의 역할을 한다. 브라우저로부터 HTTP 요청을 받아 HTML, CSS, JavaScript, 이미지와 같은 요청된 리소스를 전송해준다.

또한, `Reverse Proxy` 역할을 하는데, 클라이언트와 서버 간 직접 통신을 하지 않고 중계 서버인 프록시 서버를 통해 대리 통신을 수행한다. 프록시 서버를 사용하는 경우 보안, 트래픽 분산, 성능 향상 등의 장점이 있다.

> `Reverse proxy` 서버를 두는 이유는 요청에 대한 버퍼링이 있기 때문인데, 클라이언트가 직접 서버에 요청하는 경우 프로세스 1개가 응답 대기(pending) 상태가 되어야만 한다. 따라서 프록시 서버를 두어 요청을 배분한다.

#### 주요 디렉터리 구조와 명령어

```/etc/nginx```
: nginx server가 사용하는 기본 설정이 저장된 root directory

```/etc/nginx/nginx.conf```
: nginx 기본 설정 파일. 워커 프로세스 개수, 튜닝 등과 같은 글로벌 설정 항목을 포함하며 /etc/nginx/conf.d/ 내에 위치한 모든 설정 파일을 포함하는 최상위 http 블록을 갖고 있다

> nginx server를 중단하지 않고, 설정 파일만 리로딩 하고자 하는 경우엔 ```nginx -s reload``` 명령을 사용


```/etc/nginx/conf.d```
: 기본 HTTP 서버 설정 파일을 포함하며, 해당 디렉터리 내 파일 중 .conf 확장자인 파일은 /etc/nginx/nginx.conf 파일이 가진 최상위 http 블록에 포함된다.

```
// HTTP 프로토콜과 80 포트를 이용해 /usr/share/nginx/html/ 경로에 저장된 정적 콘텐츠를 제공하는 설정 파일의 예
server {
  listen 80 default_server;
  server_name www.example.com;

  location / {
    root /usr/share/nginx/html;
    # alias /usr/share/nginx/html;
    index index.html index.htm;
  }
}
```

```/var/log/nginx```
: nginx 로그가 저장되는 디렉터리로, access.log와 error.log 파일이 있다. (access.log는 nginx sever가 수신한 개별 요청에 대한 로그를 저장하며, error.log는 오류 발생 시 이벤트 내용을 저장)

---

#### nginx 보안 제어

> Client IP Address를 사용하여 특정 리소스에 대한 접근을 제어하는 경우

```
location /admin/ {
  deny 10.0.0.1;
  allow 10.0.0.0/20;
  allow 2001:0db8::/32;
  deny all;
}
```

```deny``` 지시자는 주어진 컨텍스트에서 사용자의 요청을 차단하며, ```allow``` 지시자는 특정 대역대의 요청만 허용한다.

차단/허용 대상은 IP 주소로 구분하며, IPv4/IPv6 형식 단일 주소 뿐만 아니라 CIDR (Classless Inter-Domain Routing) 블록 방식으로도 대역을 지정할 수 있다.

> CORS 접근 허용하는 경우

CORS는 두 개 이상의 서로 다른 도메인에서 비동기로 리소스를 가져와 사용하는 상황을 의미한다.
자바스크립트는 리소스 요청 시 자신이 호스팅되는 도메인이 아니면 CORS 정책을 필요로 하며,
요청이 CORS 상황이라면 브라우저는 서버가 내려준 CORS 정책을 따르며 적당한 CORS 헤더를 찾지 못하는 경우 해당 리소스 사용을 중지한다.

CORS 접근을 허용하기 위해서는, 사용자 요청의 메서드에 따라 응답 헤더를 변경한다

```
# GET, POST 메서드를 그룹화하여 처리
map $request_method $cors_method {
  OPTIONS 11;
  GET 1;
  POST 1;
  default 0;
}
# GET, POST, OPTIONS 메서드를 허용
# Access-Control-Allow-Origin 헤더를 통해 http://example.com의 여러 하위 도메인에서 서버 리소스에 접근 가능함을 알려줌
server {
  location / {
    if ($cors_method ~ '1') {
      add_header 'Access-Control-Allow-Methods' 'GET,POST,OPTIONS';
      add_header 'Access-Control-Allow-Origin' '*.example.com';
      add_header 'Access-Control-Allow-Headers'
                 'DNT,
                 Keep-Alice,
                 User-Agent,
                 X-Requested-With,
                 If-Modified-Since,
                 Cache-Control,
                 Content-Type';
    }
    if ($cors_method = '11') {
      add_header 'Access-Control-Max-Age' 1728000;
      add_header 'Content-Type' 'text/plain; charset=UTF-8';
      add_header 'Content-Length' 0;
      return 204;
    }
  }
}
```

> HTTP로 수신된 요청을 HTTPS로 리다이렉트하기

HTTP를 사용해야 하는 특별한 이유가 없다면, 요청이 항상 HTTPS를 사용하도록 리다이렉트 하는 편이 좋다.
아래 설정을 통해, 동일한 호스트명과 URI에 대해 HTTPS로 리다이렉트 할 수 있다.

```
server {
  listen 80 default_server;
  listen [::]80 default_server;
  server_name _;
  return 301 https://$host$request_uri;
}
```

모든 요청을 HTTPS로 리다이렉트 하는 대신, 클라이언트와 서버 간에 주고받는 민감한 데이터만 HTTPS로 리다이렉트 하기 위해서는 아래 설정을 참고한다.

```
# X-Forwarded-Proto 헤더 값이 http인 경우에만 301 리다이렉트
server {
  listen 80 default_server;
  listen [::]80 default_server;
  server_name _;
  if ($http_x_forwarded_proto = 'http') {
    return 301 https://$host$request_uri;
  }
}
```

또는 `Strict-Transport-Security` 헤더를 설정해 HSTS (HTTP Strict Transport Security) 확장을 사용하는 방법도 있다. 이 경우, 브라우저는 HTTP 요청이 발생하면 내부 리다이렉트를 통해 모든 요청이 항상 HTTPS를 이용하도록 강제하게 된다.

```
// Strict-Transport-Security 헤더를 유효가간 1년 (60초 * 60분 * 24시간 * 365일 = 31,536,000초)으로 지정
add_header Strict-Transport-Security: max-age=31536000;
```

---

#### Useful Links

+ [Configuring NGINX and NGINX Plus as a Web Server](https://docs.nginx.com/nginx/admin-guide/web-server/web-server/)

+ [CIDR - Wikipedia](https://ko.wikipedia.org/wiki/%EC%82%AC%EC%9D%B4%EB%8D%94_(%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%82%B9))

+ [X-Forwarded-Proto - MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Forwarded-Proto)

+ [Strict-Transport-Security - MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Strict-Transport-Security)
