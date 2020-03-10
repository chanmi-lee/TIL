## URL & Resource

---

### URL (Uniform Resource Locator)

- 브라우저가 정보를 찾는데 필요한 리소스의 위치
- URL은 URN과 함께 URI (Uniform Resource Identifier, 통합 자원 식별자)를 구성한다.
  - URN: 현재 리소스가 어디에 존재하든 상관없이 **이름만으로 리소스를 식별**
  - URL: 현재 리소스의 위치로 식별

`https://developer.mozilla.org/ko/docs/Web/HTTP` 이라는 URL을 불러오는 경우,
  - `https` 는 URL의 Scheme으로, 웹 클라이언트가 리소스에 어떻게 접근하는지 알려준다. 해당 경우는 URL이 HTTP 프로토콜을 사용한다. 이 외에도 `mailto`, `ftp`, `rtsp`, `telnet` 등이 있다.
  - `developer.mozilla.org`는 서버의 위치로, 웹 클라이언트가 리소스가 어디에 호스팅 되어 있는지 알려준다.
  - `/ko/docs/Web/HTTP`은 리소스의 경로로, 서버에 존재하는 로컬 리소스들 중에서 요청받은 리소스를 보여준다.

` scheme (how) + host (where) + path (what) `

#### URL 문법

- URL 문법은 Scheme에 따라 달라진다.

`<스킴>://<사용자이름>:<비밀번호>@<호스트>:<포트>/<경로>;<파라미터>?<질의>#<프레그먼트>`

![](https://mdn.mozillademos.org/files/15766/mdn-url-protocol@x2_update.png)
- Scheme
  - URL을 해석하는 애플리케이션(여기선 브라우저)가 어떤 프로토콜을 사용하여 리소스에 접근하는지 알려주는 정보
  - 알파벳으로 시작하고 대소문자를 구분하지 않는다


![](https://mdn.mozillademos.org/files/8015/mdn-url-domain@x2.png)
- Host
  - 접근하려는 리소스를 호스팅하고 있는 장비
  - `mdn.mozillademos.org`와 같은 DNS나 IP 주소로 제공한다

![](https://mdn.mozillademos.org/files/8017/mdn-url-port@x2.png)
- Port
  - 서버가 열어놓은 네트워크 포트
  - `http`는 80, `https`는 443

![](https://mdn.mozillademos.org/files/8019/mdn-url-path@x2.png)
- Path
  - 접근하는 리소스가 서버의 어디에 위치하는지 알려주는 정보

![](https://mdn.mozillademos.org/files/8021/mdn-url-parameters@x2.png)
- Parameter
  - 리소스에 접근하는데 필요한 추가 정보
  - Key/Value 쌍의 리스트로 구분

![](https://mdn.mozillademos.org/files/8023/mdn-url-anchor@x2.png)
- Anchor / Fragment
  - 리소스의 특정 부분을 가리킬 수 있도록 전달하는 정보 (= Bookmark)
  - 서버로 보내지 않고 클라이언트에서만 사용된다

#### 단축 URL

- URL은 상대(Relative) URL과 절대(Absolute) URL 두 가지로 나뉜다. 절대 URL은 리소스에 접근하는데 필요한 모든 정보를 가지고 있는 반면, 상대 URL은 모든 정보를 담고 있지는 않다.
- URL을 처리하는 브라우저같은 애플리케이션은 상대 URL과 절대 URL 간의 상호 변환을 할 수 있어야 한다.

---

Reference

- [MDN: What is a URL?](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_is_a_URL)
- [MDN: HTTP](https://developer.mozilla.org/ko/docs/Web/HTTP)
