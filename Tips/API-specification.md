# API 설계에서 고려해야 할 것들

> Q. API를 설계할 때 필요한 기본 원칙들과 API를 작성할 때 가장 중요하게 생각하는 요소들은 무엇인가요?

1. URL rule
계층 표현은 `/`로 표현하되 마지막에는 `/`를 사용하지 않는다

| Bad | Good |
| --- | --- |
| http://domain.com/v1/orders/ | http://domain.com/v1/orders |

2. 소문자를 사용한다

| Bad | Good |
| --- | --- |
| http://domain.com/v1/ORDERS | http://domain.com/v1/orders |

3. URL에 HTTP 메서드를 노출하지 않는다
동사 대신 명사를 사용하여 리소스 표현한다
| Bad | Good |
| --- | --- |
| http://domain.com/v1/create-order | http://domain.com/v1/orders |

4. 의미에 맞는 HTTP 상태를 반환한다
예를 들어 응답은 `200 OK`인데, 내부의 사용자 정의 코드나 메시지가 에러를 뱉어내는 형태로 표현하면 안된다.
성공한 응답은 200, 201(Created) 등의 HTTP 상태 코드를 반환하고, 실패한 응답의 경우 4XX로 응답한다.
(400: Bad Request, 401: Unauthorized, 403: Forbidden, 404: Not Found,...)

5. API 버전을 명시한다
API는 언제든 수정사항이 발생할 수 있으며, 데이터 구조가 수정되는 일이 빈번하다.
따라서 가급적 상위 버전은 하위 버전의 하위 호환성을 유지해야 하며, 해당 API가 변경됨에 따라 클라이언트 측에 미치는 영향을 고려해야 한다.

| Bad | Good |
| --- | --- |
| http://domain.com/orders | http://domain.com/v1/orders |

6. 정렬, 필터, 페이징 등의 기능은 쿼리 파라미터를 활용한다

| 정렬 | 필터링 | 페이징 |
| --- | --- | --- |
| /orders?sort=price_desc | /orders?fields=item.price.qty | /orders?offset=0&limit=10 |

7. 문서화
swagger 등으로 스펙 정의 뿐만 아니라 실행 테스트 환경에서 확인할 수 있어야 한다.
