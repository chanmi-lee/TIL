## HTTP Connection

**Questions**
- HTTP는 어떻게 TCP 커넥션은 사용하는가
- 병렬 커넥션, `keep-alive` 커넥션, 커넥션 파이프라인을 활용한 HTTP의 최적화
- 커넥션 관리를 위해 따라야 할 규칙들

---

#### HTTP Connection

전 세계 모든 HTTP 통신은, 패킷 교환 네트워크 프로토콜들의 계층화된 집합인 **TCP/IP** 를 통해 이루어진다.

**TCP 스트림은 Segment로 나뉘어 IP 패킷을 통해 전송된다**

TCP는 IP패킷(혹은 IP 데이터그램)이라고 불리는 작은 조각을 통해 데이터를 전송한다.

![](https://www.oreilly.com/library/view/http-the-definitive/1565925092/httpatomoreillycomsourceoreillyimages96902.png)

`HTTP`는 'IP, TCP, HTTP'로 구성된 `프로토콜 스택`에서 최상위 계층이며, HTTP에 보안 기능을 더한 `HTTPS`는 `TLS` 혹은 `SSL`이라 불리기도 하며 HTTP와 TCP 사이에 있는 `암호화(cryptographic encryption)` 계층이다.

TCP 커넥션 형식은 `<발신지 IP주소, 발신지 포트, 수신지 IP주소, 수신지 포트>`로 유일한 커넥션을 생성한다.

---

#### HTTP 커넥션 최적화

**병렬 (parallel) 커넥션**

클라이언트가 `여러 개의 TCP 커넥션을 맺음`으로써 할당받은 각 TCP 커넥션상의 트랜잭션을 통해 동시에 내려받는다.

병렬 커넥션이 일반적으로 더 빠르긴 하지만, 클라이언트의 네트워크 대역폭이 좁을 때나 다수의 커넥션을 맺는 경우 서버의 부하가 증가하는 등의 이유로 항상 그렇지는 않다.

**지속 (persistent) 커넥션**

커넥션을 맺고 끊는 데서 발생하는 지연을 제거하기 위한 `TCP 커넥션의 재활용`

병렬 커넥션에 비해 지속 커넥션은 `커넥션을 맺기 위한 사전 작업과 지연을 줄여주고`, `커넥션 수를 줄여준다`는 장점이 있다.

그러나, 이를 잘못 관리할 경우 커넥션이 계속 연결된 상태로 남아있어 로컬 리소스와 서버의 리소스에 불필요한 소모를 발생시킨다는 단점이 있다.

오늘날 많은 웹 애플리케이션은 두 가지 방법을 함께 사용한다. 즉, 적은 수의 병렬 커넥션만을 맺고 그것을 유지한다.

**파이프라인 (pipelined) 커넥션**

공유 TCP 커넥션을 통한 병렬 HTTP 요청


---

Reference

- [MDN: HTTP/1.x의 커넥션 관리](https://developer.mozilla.org/ko/docs/Web/HTTP/Connection_management_in_HTTP_1.x)
- [MDN: Keep-Alive](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Keep-Alive)
