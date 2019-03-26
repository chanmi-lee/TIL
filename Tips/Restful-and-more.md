# RESTful

> ROA (Resource Oriented Architecture)
: 자원 지향 아키텍처

**Keyword** HTTP Protocol로 데이터를 전달하는 프레임워크

리소스를 등록하고 저장해두는 * **중간 매개체** (ex- SOAP에서의 UDDI)* 없이 리소스 제공자가 직접 리소스 요청자에게 제공한다

> 구성

* 자원 (Resource) - URI (Uniform Resource Identifier)
* 행위 (Verb) - HTTP Method
* 표현 (Representations)

> REST API 디자인 가이드

* URI는 정보의 자원을 표현해야 한다
  - 이미지, 텍스트, DB 내용 등의 모든 자원에 고유한 ID인 HTTP URI를 부여하고, 이 자원언 Server에 존재한다

* 자원에 대한 행위는 HTTP Method (GET, POST, PUT, DELETE)로 표현한다

* Client가 자원의 상태(정보)에 대해 요청하면 Server는 이에 적절한 응답(Representation)을 보낸다

> 특징

1) **Server-Client 구조**
  - 자원이 있는 쪽이 Server, 자원을 요청하는 쪽이 Client가 된다
    - REST Server: API를 제공하고 비즈니스 로직 처리 및 저장을 책임진다
    - Client: 사용자 인증, context (login, session) 등을 관리한다

2) **Stateless** (무상태)
  - HTTP protocol은 stateless protocol 이므로 REST 역시 무상태성을 갖는다
  - 즉, 어떠한 이전 요청과도 무관한 각각의 요청을 독립적인 트랜잭션으로 취급한다 (이전 요쳥이 다음 요청의 처리에 연관되어서는 안된다)
  - Server의 처리 방식에 일관성을 부여하고, 부담이 줄어들며, 서비스의 자유도가 높아진다

3) **Cacheable**
  - HTTP Protocol이 웹 표준이고 이를 그대로 사용하므로 웹에서 사용하는 기존의 인프라를 그대로 활용 할 수 있다.
  - Last-Modified 태그나 E-Tag를 이용하면 캐싱 구현이 가능하다

4) **Uniform Interface**
  - URI로 지정한 자원에 대한 조작을 통일되고 한정적인 인터페이스로 수행한다

5) **Layered System**  
  - REST Server는 다중 계층으로 구성될 수 있다
    - API Server는 순수 비즈니스 로직만 수행하고, 앞단에 보안/로드밸런싱/암호화/사용자 인증 등을 추가하여 구조상의 유연성을 줄 수 있다
    - 로드밸런싱/공유 캐시 등을 통해 확장성과 보안성을 향상시킬 수 있다
    - Proxy, gateway와 같은 네트워크 기반의 중간 매체를 사용할 수 있다

6) **Self-descriptiveness** (자체표현구조)
  - REST API 메시지만 보고도 이를 쉽게 이해 할 수 있는 자체 표현 구조로 되어 있다


---

# SOAP

(Simle Object Access Protocol)

> SOA (Service Oriented Architecture)
: 서비스 지향 아키텍처

**Keyword** 모든 데이터가 XML로 표현된다

> UDDI (Universial Description Discovery and Integration)

웹 서비스를 등록하고 검색하기 위한 저장소로, 웹 서비스를 공개적으로 접근, 검색이 가능하도록 공개된 레지스트리

![](../src/img/SOAP-architecture.PNG)

SOAP은 XML을 근간으로 헤더(optional)와 바디를 조합하는 디자인 패턴으로 설계되어 있다. 헤더는

데이터와 데이터를 다루는 오퍼레이션들이 **WSDL (Web Service Description Language)** 로 정의되면, UDDI라는 전역적인 서비스 저장소에 등록되어 *누구라도 서비스를 찾을 수 있도록 공개*

> WSDL (Web Service Description Language)

웹 서비스 기술언어 또는 기술된 정의 파일의 총칭으로 XML로 기술된다

서비스 제공 장소, 서비스 메시지 포맷, 프로토콜 등이 기술된다

> 특징

* 플랫폼/프로그래밍 언어에 독립적이다

@[W3C](https://www.w3schools.com/xml/xml_soap.asp)
```
SOAP provides a way to communicate between applications running on different operating systems, with different technologies and programming language...
```

* 메시지 인코딩/디코딩 과정과 불필요한 정보 등으로 인해 처리 속도가 늦어질 수 있다

> 예제

```
<?xml version="1.0"?>

<soap:Envelope
  xmlns:soap="http://www.w3.org/2003/05/soap-envelope"
  soap:encodingStyle="http://www.w3.org/2003/05/soap-encoiding">

  <soap:Body>
    <m:getPrice xmlns:m="https://www.w3schools.com/prices">
      <m:Item>Apples</m:Item>
    </m:getPrice>
  </soap:Body>
</soap:Envelope>
```

---

Ref

* [W3S - XML Soap](https://www.w3schools.com/xml/xml_soap.asp)
* [SOAP 기반/ RESTful 기반 웹서비스 비교](https://www.slideshare.net/seunghochoi4/soap-restful)
* [REST API 제대로 알고 사용하기](https://meetup.toast.com/posts/92)
