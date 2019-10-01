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

> ### **웹 애플리케이션과 부하의 관계**

![](https://lh3.googleusercontent.com/F31JDV6X4-hBB27Fhs2NBvKsQHwHCSQTygW2ts8rNyUwEfukiL7E8Iu1Aygx7UbkNtXaim9Zo5VzUN6FpFkX49_NmDHDc9kP_lgMMtVQg_b-Gf24CbyDM0uS-cSzspgQhhoU1n92QndmgAD6cFH2HjPExfeReq9NqI_mQWv1xZ9mW5nLcHX-bOeZN6vQl6TgBqtIYvn8dAN6DS_W78E1At1-gFC3JRYi8MuI5qok2cJblDsbkA8bQyRwiZxrZt6Q6OHurx6-CHo2dqffUeP6417Izf2yKLDSG5IkgfHey7-gLMjF051KmENXtoR00mjJ0Nllxo0nCHHQa7QCQvYSyCcTra0KPcvIRCTr_KLvIdLIjTyLhk5XWne30apMUa_OJrAcnq88jUS4Kc_6J7TSCbRoRxpBCNrmOZDcWFW5tJEtGPQQidB7BX_O4lPo1xw_FLdvzW3VPbCbgk9UQMYL9oZ9ZacwlhKFY99a9QxVilnU9rAoAaZJztOO3DT0dqNRC20P7x9UPOa8N5X5Z9xJgORgJNYdGJzpUzsvz9BVr7DCA3LWbX3DkcydTOIVQLGA4iSmtkU4FGA7aBAt5J7wolQV_MDbVzmmOzozjaXjllaxEPti1Eq-qsC8c_hv-YjsWXRffBf3HlFcWcRAPGAmQ2g16SrH8c4=w1431-h968-no)
웹 애플리케이션의 3단 구조에는 1) Proxy, 2) AP Server, 3) DB가 있다.

웹 애플리케이션에 (1), (2) 라는 요청이 오고 (3) DB에 도달해서 I/O가 발생하고, (4) 이 I/O가 발생해서 되돌아온 콘텐츠를 변경한 후 클라이언트로 응답한다.

기본적으로 AP 서버에는 I/O 부하가 걸리지 않고 DB 측에 I/O 부하가 걸린다.

AP 서버에는 **CPU 부하** 만 걸리므로 분산이 간단하다. 그 이유는 기본적으로 데이터를 분산해서 갖고 있는 것이 아니므로 2', 2''와 동일한 호스트가 동일하게 작업을 처리하기만 하면 분산할 수 있다.
따라서 Scale-out 전략으로 확장성을 확보하며 Load Balancer를 통해 요청을 균등하게 분산할 수 있다.

그러나, **I/O 부하** 에는 문제가 있다. DB 3'을 추가한다고 가정해보면, DB 3이 지닌 데이터와 DB 3'의 데이터를 **어떻게 동기화할 것인가** 라는 문제가 생긴다.
또한 DB에서는 디스크를 많이 사용하므로 디스크 I/O를 많이 발생시키는 구성으로 되어 있으면 속도 문제도 생긴다.
