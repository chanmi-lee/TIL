## Web Server

---

- `Web server`는 `resource server`로 클라이언트로부터의 HTTP 요청을 처리하고 응답을 제공한다.

![](https://www.oreilly.com/library/view/http-the-definitive/1565925092/httpatomoreillycomsourceoreillyimages96944.png)

  1. **클라이언트와 커넥션을 맺는다**
    - 클라이언트가 웹 서버에 TCP 커넥션을 요청하면, `Web server`는 그 커넥션을 맺고 TCP 커넥션에서 IP 주소를 추출하여 어떤 클라이언트인지 확인한다. 기존에 맺어진 지속적 커넥션이 있다면 이를 사용한다.
    - 대부분의 `Web server`는 `역방향 DNS(reverse DNS)`를 사용하여 클라이언트의 IP 주소를 클라이언트의 호스트 명으로 변환하도록 설정되어 있다. `hostname lookup`은 꽤 시간이 많이 드는 작업으로, 많은 대용량 `Web server`는 `hostname resolution`을 꺼두거나 특정 콘텐츠에 대해서만 켜놓는다.
    - 몇 `Web server`는 `IETF ident protocol`을 지원한다. 이는 어떤 사용자 이름이 HTTP 커넥션을 초기화했는지 찾아내도록 하는 방법으로 클라이언트는 `113 port`를 listen한다. 그 후 서버가 자신의 커넥션을 이를 향해 열고, 새 커넥션에 대응하는 사용자 이름을 묻는 간단한 요청을 보낸다. 주로, 조직 내부에서 잘 사용하는 기법이다.

  2. **`HTTP Request`를 받는다**
    - `Web server`는 받은 테이터를 읽고 파싱하여 요청 메시지를 구성한다.

  3. `HTTP Request message`를 처리한다
    - `Web server`는 요청으로부터 `HTTP Method`, `Resource`, `Headers`, `Entity`(optional)을 처리한다.

  4. 요청 받은 `resource`에 접근한다
    - 요청 메시지의 `URI`에 대응하는 알맞은 콘텐츠나 콘텐츠 생성기를 찾아 그 콘텐츠의 원천을 식별한다.
    - `Web server`는 다양한 리소스 매핑 방법을 사용한다.
      - URI를 **웹 서버의 파일 시스템 안에 있는 파일 이름** 으로 사용 (Simple)
        - `Docroot`: 시스템의 특별한 폴더를 웹 콘텐츠를 위해 예약해두는데, httpd.conf 파일에 DocumentRoot 줄을 추가하여 아파치 웹 서버의 문서 루트를 설정할 수 있다.
        ```
        DocumentRoot /user/local/httpd/files
        ```
        - `가상 호스팅된 docroot`: 각 사이트에 분리된 `Docroot`를 주는 방법으로, 하나의 `Web server`에서 여러 개의 웹 사이트를 호스팅한다. 가상 호스팅 웹 서버는 `URI`나 `Host header에서 얻은 IP 주소`나 `hostname`을 이용해 올바를 `Docroot`를 식별한다.
        ![](https://www.oreilly.com/library/view/http-the-definitive/1565925092/httpatomoreillycomsourceoreillyimages96962.png)
      - URI를 파일이 아닌 디렉터리에 대한 요청으로 사용
        - 해당 디렉터리 안에서 `index.html`를 찾아 이를 반환
      - URI를 **동적 리소스에 매핑**
      - 또는, `Web server`는 요청받은 리소스에 접근 제어를 할당하여 클라이언트의 IP 주소에 근거하여 제어하거나 접근하기 위한 비밀번호를 요청하기도 한다.

  5. 올바른 헤더를 포함한 `HTTP Response message`를 만든다
  ![](https://www.oreilly.com/library/view/http-the-definitive/1565925092/httpatomoreillycomsourceoreillyimages96962.png)
    - `Response entity`의 **MIME type** 을 서술하는 Content-Type 헤더
    - `Response entity`의 길이를 서술하는 Content-Length 헤더
    - 혹은 성공 메시지(2XX)대신 리다이렉션 응답(3XX)을 반환한다. `Location` 헤더는 콘텐츠의 새로운 혹은 선호하는 위치에 대한 URI를 포함한다.

```
ex)
301 Moved Permanently: 영구히 리소스가 옮겨진 경우
303 See Other / 307 Temporary Redirect:
1) 임시로 리소스가 옮겨진 경우
2) 서버가 상태 정보를 추가하여 리다이렉트 (URL 증강)
3) 부하 분산을 위한 리다이렉트
...
```      

  6. `HTTP Response`를 돌려준다

  7. `Transaction`을 `LOG`로 남긴다

---

Reference

- HTTP: The Definitive Guide - Chapter 5. Web Server
