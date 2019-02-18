# Internet

인터넷이 동작하는데 가장 중요한 원리들을 (간략하게) 알아봅니다.

> ##### IP (Internet Protocol) Address


인터넷이라고 하는 이 체계가 동작하기 위해서는, 이 체계 위에 있는 여러 단말들이 있습니다. 각각의 단말들이 준수해야하는 규칙들이 존재하는데, 바로 그러한 규칙을 Internet Protocol, IP라고 합니다.

예를 들면, 전화라는 통신망 위에서 동작하는 전화기들을 생각해봅시자. 각각의 전화가 서로 통신을 주고받기 위해서는 각자가 고유한 식별자(a.k.a 전화번호)를 갖고 있어야 합니다.

웹사이트에 접속하는 방법

- IP
- Domain Name

> Domain Name System ?

Domain Name이 동작하는 체계를 의미하며,
Name Server는 각각의 도메인별 IP 정보를 가지고 있습니다.
DNS는 인터넷 상에서 사람들이 인식하기 쉬운 Domain Name을 서버가 인식하는 IP주소로 변환해주는 시스템입니다.

내 IP 주소를 알기 위해선,
```
$ ipconfig /all
```

혹은 my ip를 알려주는 여러 서비스를 통해 알아낼 수 있다.

특정 사이트의 IP 주소를 알기 위해선,
```
$ ping google.com

Ping google.com [172.217.25.78] 32바이트 데이터 사용:
172.217.25.78의 응답: 바이트=32 시간=35ms TTL=53
172.217.25.78의 응답: 바이트=32 시간=35ms TTL=53
172.217.25.78의 응답: 바이트=32 시간=35ms TTL=53
...
```


1. PC 브라우저에서 www.google.com 을 입력하면, 미리 설정되어있는 **(Local) DNS에게 "www.google.com 이라는 hostname"에 대한 IP 주소** 를 물어봅니다.
2. Local DNS에 IP 주소가 있다면, 바로 PC에 IP 주소를 주고 끝나지만 없다면 다음 단계를 거치게 됩니다.
3. Local DNS는 "www.google.com 에 대한 IP 주소"를 찾아내기 위해 다른 DNS 서버들과 통신(**DNS 메시지**)을 시작합니다. 먼저, **Root DNS 서버에 요청** 을 하는데 이를 위해선 Local DNS 서버에 Root DNS 서버의 정보(IP주소)가 미리 설정되어 있어야 합니다.
4. Root DNS 서버는 전 세계에 13대 구축되어 있습니다. (미국 10대, 일본/네덜란드/노르웨이에 각 1대씩) 우리나라의 경우 Root DNS 서버에 대한 미러 서버를 3대 운용하고 있다고 합니다. (KISA-한국인터넷진흥원, KT, KNIX-한국인터넷연동센터)
5. Root DNS 서버는 "www.google.com 의 IP 주소"를 모릅니다. 그래서 Local DNS 서버에게 "난 www.google.com 에 대한 IP 주소를 몰라. 나 말고 내가 알려주는 다른 DNS 서버에게 물어봐" 라고 응답을 합니다.
6. 이 다른 DNS 서버는 com DNS, "com 도메인"을 관리하는 DNS 서버, 입니다.
7. **Local DNS 서버는 com DNS 서버에 "www.google.com 의 IP 주소"를 요청** 합니다.
8. 역시 com DNS 서버도 해당 정보가 없어 다른 DNS 서버에 요청할 것을 응답합니다. 이는 "google.com 도메인"을 관리하는 DNS 서버입니다.
9. **local DNS 서버는 google.com DNS 서버에게 "www.google.com 의 IP주소"를 요청** 합니다. 해당 정보를 수신한 Local DNS 서버는 www.google.com 에 대한 IP 주소를 캐싱(사용자 컴퓨터에 해당 정보를 저장하여 매 번 컴퓨터가 google.com의 IP 주소를 요청하지 않게 하기 위함)하고 그 IP 주소 정보를 단말(PC)에 전달해줍니다.

> 이와 같이 Local DNS 서버가 여러 DNS 서버를 차례대로 (Root DNS 서버 -> com DNS 서버 -> google.com DNS 서버) 물어봐서 그 답을 찾는 과정을 **Recursive Query** 라고 부릅니다.


* https://www.google.com/ --> URL
* www.google.com  --> Host Name
* .com --> Top-level Domain Name (TLD)
* .google.com --> Second-level Domain Name

> **Root Server** 는 .com, .net, .org와 같은 *최상위 도메인* 과 .kr(한국), .cn(중국), .uk(영국) 등의 *모든 국가 도메인* 에 대한 정보를 알고 있습니다.

> **TLD** 는 2차 도메인 (Second-level Domain Name), 즉 .com, .net, .org 앞에 사용하는 단어에 대한 정보를 저장합니다. (ex- google, naver, daum, ...)

---

> ##### IPv6

기존 IPv4의 주소 고갈 문제를 해결하기 위하여 기존의 IPv4 주소 체계를 128비트 크기로 확장한 차세대 인터넷 프로토콜 주소 체계 입니다.

Q. IP의 숫자는 몇개일까요?

0.0.0.0 ~ 255.255.255.255 사이에 있는 것들이 IP의 주소 체계이며, 약 42억개의 경우(2의 32승)의 수가 있습니다.

충분한 것 같지만, 인터넷이 엄청나게 빠른 속도로 발전하면서 2011년도에 모든 IP를 사용하게 되면서 그 이후로는 새로운 IP가 발급되지 않고 있습니다.

```
192.0.2.52 (IPv4)

0000:0000:0000:0000:0000:0000:C000:0234 (IPv6)
```

이를 해결하기 위한 방법 중 하나로 IPv6라는 새로운 주소 체계가 등장하게 됩니다. IPv6로는 2의 128승, 사실상 향후 몇 백년 동안은 걱정할 필요 없을 정도로 엄청나게 많은 경우의 수가 가능하므로 IP 고갈의 문제를 해결하게 되었습니다.

IPv6와 기존 IPv4 사이의 가장 큰 차이점은 IP 주소의 길이가 32비트에서 128비트로 늘어났다는 점입니다.

---


Ref

* [생활코딩 - Internet](https://opentutorials.org/course/1688/9483)
* [NETMANIAS - DNS 기본 동작 설명](https://www.netmanias.com/ko/post/blog/5353/dns/dns-basic-operation)
* [KISA-한국인터넷진흥원 - DNS](http://www.kisa.or.kr/business/address/address3_sub1.jsp)
* [Wikipedia - IPv6](https://ko.wikipedia.org/wiki/IPv6)
