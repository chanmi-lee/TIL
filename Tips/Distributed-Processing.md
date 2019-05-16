# Distributed Processing


> ### **GCDN (Global CDN, Content Delivery Network)**

CSS와 JavaScript, Image와 같이 공통으로 호출되는 리소스는 한 번 업로드되면 잘 변하지 않는다.
이런 리소스를 메인 페이지의 웹 서버에서 직접 제공하면 트래픽 부하가 엄청나게 가중된다.
때문에, **공통적으로 호출되는 리소스의 부하 분산을 위해 GCDN을 사용** 하며, 이를 통해 트래픽을 절감할 수 있다.

최근에는 음악, 실시간 동영상 스트리밍, 대용량 파일 다운로드와 같은 멀티미디어 컨텐츠 이용이 급증하면서 네트워크 상에서 전송되는 트래픽이 더욱 늘어나고 있다.

일시적으로 수 많은 사용자들이 동시에 컨텐츠 서버에 접속을 시도하거나, 서버 이중화를 고려하지 않은 환경에서 컨텐츠 서버와 연결된 네트워크에 장애가 발생하면 
극단적인 경우 컨텐츠 제공자(Content Providor)는 사용자들에게 해당 컨텐츠를 제공할 수 없게 된다.

이와 같은 문제들을 해결하기 위해서는 네트워크/서버 용량 증설, 서버 이중화 등의 끊임없는 네트워크 및 시스템 확장이라는 요구 사항이 동반된다.
그러나, 인프라 확장 속도는 YouTube와 같이 하루에도 수 없이 생산되는 컨텐츠 제공자들에게도 큰 부담이 아닐 수 없다.

CDN 이후에는, 컨텐츠 서버와 연결된 네트워크에 장애가 발생하더라도 컨텐츠가 다른 여러 지역에 위치한 캐시 서버에 저장되어 있으므로 (Geographical Redundancy) 정상적으로 사용자에게 컨텐츠를 제공할 수 있게 된다.
전 세계 대륙/나라별로 cache server를 두어 이용자는 origin server가 아닌 지리적으로 가까운 곳에 위치한 cache server로부터 content를 가져오는 식으로 동작한다.

이와 같은 Global CDN을 제공하는 사업자는 Akamai, Limelight Networks, Level 3, CDNetworks 등이 있다.

---

> ### **SSI**

웹 서버 (Apache, NGINX 등)에서 지원하는 Server-side Script language로, 서버에 있는 특정 파일을 읽어오거나 특정 쿠키 유무의 판별 등 간단한 기능을 실행할 수 있다.
SSI를 사용해 웹 서버에서 기능을 처리하면 WAS의 부담을 줄여 WAS의 성능에 여유를 줄 수 있게 되어 서버의 자원을 더 효율적으로 사용할 수 있다.

서버가 SSI를 처리하려면 `httpd.conf` 파일이나 `.htaccess` 파일에서 다음 지시어를 사용해야 한다.

```
Options +Includes
```

기본 SSI 지시어의 사용법은 다음과 같다

[Syntax]
```
<!--#element attribute=value attribute=value ... -->
```

`<!--#echo var="DATE_LOCAL" -->`

`echo` element는 변수 값을 그대로 출력한다.

`<!--#config timefmt="%A %B %d, %Y" -->`

`config` element의 `timefmt` attribute를 통해 날짜 출력 형식을 변경할 수도 있다.

또한, 일반적인 SSI 사용법 중 하나로, `include` element를 통해 방문수 카운터 같은 CGI 프로그램 결과를 출력한다.

`<!--#include virtual="/cgi-bin/counter.pl" -->`

또는,

`<!--#include virtual="/footer.html" -->`

방식을 통해 여러 페이지가 있는 사이트에서, 상단과 하단을 파일로 포함하여 표준 외관을 가지도록 설정할 수 있다.
`file` attribute는 **현재 디렉토리에 상대적인 파일 경로** 를 값으로 한다.

---

> ### **Circuit breaker**

애플리케이션의 안정성과 장애 저항력을 높이는 것을 목적으로, 
트래픽 폭증 및 네트워크 일시 단절 등으로 인한 API 서버 등 외부 서비스의 장애로 인한 연쇄적 장애 전파를 막기 위해 자동으로 외부 서비스와 연결을 차단 및 복구하는 역할을 한다.

일시적인 장애라면 재시도로 정상 데이터를 수신할 수 있으나, 계속 재시도하는 것이 무의미한 경우 이를 포기하고 미리 준비된 응답을 사용자에게 전달한다.

서킷은 다음과 같은 세 가지 상태를 가진다.

- Closed 상태: 메서드가 정상적으로 작동해 서킷이 닫힌 상태
- Open 상태: 메서드에 문제가 발생해 서킷이 열린 상태
- Half-Open 상태: Open과 Closed의 중간 상태로, 메서드를 주기적으로 확인해 정상이라고 판단되면 Closed 상태로 전환하고 아닌 경우 Open 상태를 유지

**Netflix OSS** 의 서킷 브레이커 라이브러리인 [Netflix OSS Hystrix](https://github.com/Netflix/hystrix) 등장 이후 이를 적극적으로 사용하고 있다.

또한, 이를 Spring Cloud 프로젝트에서 사용할 수 있도록 래핑한 [Spring Cloud Netflix](https://cloud.spring.io/spring-cloud-netflix/)를 활용하면 Spring application에서 이를 손쉽게 사용할 수 있다.

---

> ### **Service Discovery**

동적으로 생성, 삭제되는 Server Instance에 대한 IP 주소와 포트를 자동으로 찾아 설정할 수 있게 하는 기능이다.

![](https://d2.naver.com/content/images/2018/11/helloworld-201810-naver_main-08.png)

**Service discovery server** 는 각 서버군의 서버 목록을 관리한다.
서버는 시작할 때 자신의 정보 (IP 주소, 포트)를 Service discovery server로 보내고, Service discovery server는 이 정보를 받아 서버군의 서버 목록을 갱신한다.
각 서버군에 있는 서버는 주기적으로 Service discovery server로 목록을 요청해 서버 목록을 최신으로 유지한다.

이러한 방법으로 서버는 재시작 등 외부의 개입이 없어도 자동으로 항상 최신 서버 목록을 확보할 수 있다.

기존 On-premise 환경에서는 서버를 추가, 삭제하는 일이 많지 않았지만, 
클라우드 환경에서는 (특히 오토스케일링 등의 기능을 사용한다면 더욱 더) 수시로 서버가 추가, 삭제되므로 Service discovery는 아주 유용하게 사용된다.




---

Ref

- [CDN(Content Delivery Network) 이라는 참 좋은 기술](https://www.netmanias.com/ko/post/blog/5350/cdn/the-advantage-of-the-cdn-content-delivery-network)
- [Apache tutorial: Server Side Includes](https://httpd.apache.org/docs/2.4/howto/ssi.html)
- [Naver D2 - 네이버 메인 페이지의 트래픽 처리](https://d2.naver.com/helloworld/6070967)
