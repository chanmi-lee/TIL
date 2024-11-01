# Web Application with Scalability

[Data Structure](#) | [Memory](#) | [OS](#) | [DB](#) | [Server / Infra](#)

> ### **Scale-out**

서버를 횡으로 전개, 즉 서버의 역할을 분담하거나 대수를 늘림으로써 시스템의 전체적인 처리능력을 높여서 부하를 분산하는 방법

Ref) Scale-up : 하드웨어의 서능을 높여 처리능력을 끌어올리는 방법

대량의 액세스가 있는 서비스에서는 서버 1대로 처리할 수 없는 부하를 어떻게 처리할 것인지가 가장 큰 문제이다.
일반적으로 하드웨어의 성능과 가격은 비례하지 않는다. 저가의 하드웨어를 횡으로 나열해서 **확장성** *scalability* 을 확보하는 것이 Scale-out 전략이다.
Scale-up 보다 Scale-out 전략이 주류이며, 
이유는 다양하겠지만 주로 웹 서비스에 적합한 형태이고 비용이 저렴하다는 점과 시스템 구성에 유연성이 있다는 점이 포인트이다.

---

> ### 로드 밸런싱

특정 시점이 지나면 웹 애플리케이션은 복잡해지며 거대해진다. 사용자 증가로 인해 쌓이는 데이터 건수의 크기도 늘어나며, 요청 또한 늘어나기 마련이다. 단순히 서버의 scale-out이나 애플리케이션 튜닝만으로는 비용을 최적화하지 못한다.

로드 밸런싱은 `서버의 트래픽을 적절히 분배하여 가뇽성을 높이는 가장 대표적인 방법`이다. 여러 대의 서버나 네트워크 디바이스에 분산된 트래픽을 균등하게 분배하여 시스템의 성능, 가용성, 안정성을 향상 시킨다. 대표적으로는 L4/L7 스위치를 통해 트래픽을 분산한다.

#### L4 로드 밸런서
전송 계층(Transport Layer, Layer 4) 에서 작동하는 로드 밸런서로 `IP 주소와 포트 번호를 사용하여 로드 밸런싱을 수행`한다. 즉 서버에서 IP 주소와 포트 번호를 사용하여 서버로 들어오는 트래픽을 여러 대의 서버로 분산 시킨다. TCP 및 UDP 기반 전송 계층 프로토콜을 사용하며, 성능과 속도 측면에서 단순하고 안정적인 로드 밸런싱을 수행한다

#### L7 로드 밸런서
OSI 모델의 최상위 레이어인 `애플리케이션 레이어에서 동작하는 로드 밸런서`이다. 응용 계층에서는 트래픽을 요청 내용 (HTTP 헤더, 요청 URL, 쿠키, 세션 등)을 기반으로 분산 시킨다. (그래서 Context 기반 스위칭이라고 불리기도 한다.)

L4 로드 밸런서는 단지 부하만 분산 시키는 것이라면, L7은 요청의 세부사항을 두고 분산하여 결제만 담당하는 서버, 회원가입만을 담당하는 서버 등으로 분리해서 가볍고 작은 단위로 여러 개의 서비스를 운영하고 요청을 각각의 서버에 분산할 수 있다. L7 로드 벨런서는 더 고급 기능을 제공하기 때문에 L4 보다는 더 복잡한 웹 애플리케이션에 사용된다.


> ### 라운드 로빈

요청 트래픽이 증가할 때, 서버를 늘리며 각 서버에 차례로 요청을 전송해 처리하는 방식을 라운드 로빈 방식이라고 한다. 라운드 로빈(Round Robin)은 여러 대상 사이를 순환하며 균등하게 분산하는 로드 밸런싱 알고리즘 중 하나로, 라운드 로빈 알고리즘은 각 대상에게 균등한 처리량을 보장하며, 구현이 간단하고 효율적이라는 장점이 있다.

라운드 로빈 알고리즘은 각 대상에게 요청을 차례대로 전달하는 방식으로 동작한다. 예를 들어, 3개의 서버가 있을 때 첫 번째 요청은 서버1에, 두 번째 요청은 서버2에, 세 번째 요청은 서버3에 전달되고, 네 번째 요청부터는 다시 서버1에 전달된다. 이런 식으로 요청이 순환하며 대상을 균등하게 처리한다.

#### 단점
라운드 로빈 알고리즘은 대상의 상태나 부하를 고려하지 않고 균등하게 분산하므로, 대상의 수가 일정하고 모든 대상이 동일한 성능을 가지고 있다면 적절한 알고리즘이 될 수 있습니다. 하지만 대상의 수나 성능이 변할 경우, 특정 대상이 부하를 많이 받거나 느려지는 등의 문제가 발생할 수 있다. 이러한 문제를 해결하기 위해서는 대상의 상태나 부하를 고려하는 다른 로드 밸런싱 알고리즘을 사용해야 한다.

> ### 가중 라운드 로빈
가중 라운드 로빈 알고리즘은 `각 대상에 가중치(weight)를 부여하여 부하가 큰 대상에 더 많은 요청을 보내도록 하는 알고리즘`이다. 예를 들어, 성능이 우수한 서버에 가중치를 높게 설정하고, 성능이 좋지 않은 서버에는 가중치를 낮게 설정하는 방식으로 구현할 수 있다.

#### 단점
가중 라운드 로빈 알고리즘의 주요 단점은 가중치를 설정하는 과정에서 서버의 부하나 성능을 제대로 파악하지 못하면, 부하가 큰 서버에 더 많은 요청이 집중되어 서버의 부하가 더욱 심해질 수 있다는 것입니다. 또한, 가중치 설정이 일정하지 않은 경우 부하 분산이 골고루 이루어지지 않을 수 있다.

> #### 최소 연결 수 (least connection)
최소 연결 수 알고리즘은 `현재 연결된 수가 가장 적은 대상에 우선적으로 요청을 전달하는 알고리즘`이다. 이 알고리즘은 대상의 부하나 성능 상태를 고려하지 않고, 현재 연결된 수가 적은 대상에 더 많은 요청이 발생하므로, 서버 부하를 골고루 분산시킬 수 있다.

#### 단점
최소 연결 수 알고리즘의 주요 단점은 연결 수가 가장 적은 서버에만 요청을 전달하기 때문에, 대상 서버의 부하나 성능 상태를 고려하지 않기 때문에, 불균형한 부하 분산이 발생할 수 있다.

> 이 외에도 IP Hash나 Least Response Time 등의 로드 밸런싱 알고리즘이 있다
 
---

> ### **웹 애플리케이션과 부하의 관계**

![](https://lh3.googleusercontent.com/F31JDV6X4-hBB27Fhs2NBvKsQHwHCSQTygW2ts8rNyUwEfukiL7E8Iu1Aygx7UbkNtXaim9Zo5VzUN6FpFkX49_NmDHDc9kP_lgMMtVQg_b-Gf24CbyDM0uS-cSzspgQhhoU1n92QndmgAD6cFH2HjPExfeReq9NqI_mQWv1xZ9mW5nLcHX-bOeZN6vQl6TgBqtIYvn8dAN6DS_W78E1At1-gFC3JRYi8MuI5qok2cJblDsbkA8bQyRwiZxrZt6Q6OHurx6-CHo2dqffUeP6417Izf2yKLDSG5IkgfHey7-gLMjF051KmENXtoR00mjJ0Nllxo0nCHHQa7QCQvYSyCcTra0KPcvIRCTr_KLvIdLIjTyLhk5XWne30apMUa_OJrAcnq88jUS4Kc_6J7TSCbRoRxpBCNrmOZDcWFW5tJEtGPQQidB7BX_O4lPo1xw_FLdvzW3VPbCbgk9UQMYL9oZ9ZacwlhKFY99a9QxVilnU9rAoAaZJztOO3DT0dqNRC20P7x9UPOa8N5X5Z9xJgORgJNYdGJzpUzsvz9BVr7DCA3LWbX3DkcydTOIVQLGA4iSmtkU4FGA7aBAt5J7wolQV_MDbVzmmOzozjaXjllaxEPti1Eq-qsC8c_hv-YjsWXRffBf3HlFcWcRAPGAmQ2g16SrH8c4=w1431-h968-no)
웹 애플리케이션의 3단 구조에는 1) Proxy, 2) AP Server, 3) DB가 있다.

웹 애플리케이션에 (1), (2) 라는 요청이 오고 (3) DB에 도달해서 I/O가 발생하고, (4) 이 I/O가 발생해서 되돌아온 콘텐츠를 변경한 후 클라이언트로 응답한다.

기본적으로 AP 서버에는 I/O 부하가 걸리지 않고 DB 측에 I/O 부하가 걸린다.

AP 서버에는 **CPU 부하** 만 걸리므로 분산이 간단하다. 그 이유는 기본적으로 데이터를 분산해서 갖고 있는 것이 아니므로 2', 2''와 동일한 호스트가 동일하게 작업을 처리하기만 하면 분산할 수 있다.
따라서 Scale-out 전략으로 확장성을 확보하며 Load Balancer를 통해 요청을 균등하게 분산할 수 있다.

그러나, **I/O 부하** 에는 문제가 있다. DB 3'을 추가한다고 가정해보면, DB 3이 지닌 데이터와 DB 3'의 데이터를 **어떻게 동기화할 것인가** 라는 문제가 생긴다.
또한 DB에서는 디스크를 많이 사용하므로 디스크 I/O를 많이 발생시키는 구성으로 되어 있으면 속도 문제도 생긴다.

