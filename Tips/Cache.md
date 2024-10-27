# Cache

### 캐시의 생명 주기

HTTP에서 리소스(Resource)란 웹 브라우저가 HTTP 요청으로 가져올 수 있는 모든 종류의 파일을 말합니다. 대표적으로 HTML, CSS, JS, 이미지, 비디오 파일 등이 리소스에 해당합니다.

웹 브라우저가 서버에서 지금까지 요청한 적이 없는 리소스를 가져오려고 할 때, 서버와 브라우저는 완전한 HTTP 요청/응답을 주고받습니다.
HTTP 응답에 포함된 **Cache-Control 헤더**에 따라 받은 리소스의 생명 주기가 결정됩니다.

### 캐시의 유효 기간: max-age

서버의 Cache-Control 헤더의 값으로 **max-age=<seconds>** 값을 지정하면, 이 리소스의 캐시가 유효한 시간은 <seconds> 초가 됩니다.
max-age 의 최대 기간은 1년인데, 따라서 보통 캐시 무효화 전략을 사용하면 1년으로 캐시 유효 기간을 설정합니다.

> Note: Cache-Control max-age 값 대신 Expires 헤더로 캐시 만료 시간을 정확히 지정할 수도 있습니다.

### HTTP 캐시 무효화 (Cache Busting)

- 파일명에 버전 포함하기 (ex: bundle.v2.js)
- 경로에 버전 포함하기 (ex: /v2/bundle.js)
- 쿼리 스트링에 버전 포함하기 (ex: bundle.js?v=2)
- 파일명에 파일의 해시 포함하기 (ex: bundle.ec0901c5aef93e779cd20e0db130b28b.js)
- 쿼리 스트링에 파일의 해시 포함하기 (e.g. bundle.js?v=ec2935c5aef93e273cd90e0db891b82b)

> 캐시 무효화 주의점
```
<!DOCTYPE HTML>
<html lang="ko">
<head>
  <script src="./bundle.ec0901c5aef93e779cd20e0db130b28b.js"></script>
  <meta content="text/html; charset=UTF-8" http-equiv="Content-Type"/>
</head>
<body>
	Hello, World!
</body>
</html>
```
bundle.js 나 common.css 와 같은 정적 파일을 포함하고 있는 위와 같은 html 파일 등을 Main Resource 라고 합니다.

당연하지만, 이런 Main Resource 에는 캐시 무효화 전략을 사용할 수 없습니다.
Main Resource의 경우에는 `no-store` 를 사용하여 아예 캐시하지 않도록 하거나, `no-cache` 를 사용하여 캐시하되, 항상 캐시 유효성 검증을 하도록 설정해주어야 합니다.


### no-cache, no-store

Cache-Control에서 가장 헷갈리는 두 가지 값이 있다면 바로 no-cache 와 no-store 입니다. 이름은 비슷하지만 두 값의 동작은 매우 다릅니다.

`no-cache` 값은 대부분의 브라우저에서 **max-age=0 과 동일한 뜻**을 가집니다. 즉, **캐시는 저장하지만 사용하려고 할 때마다 서버에 재검증 요청**을 보내야 합니다.

`no-store` 값은 **캐시를 절대로 해서는 안 되는 리소스일 때 사용**합니다.
캐시를 만들어서 저장조차 하지 말라는 가장 강력한 Cache-Control 값입니다. `no-store`를 사용하면 브라우저는 어떤 경우에도 캐시 저장소에 해당 리소스를 저장하지 않습니다.

### 결론

캐시 설정을 섬세히 제어함으로써 사용자는 더 빠르게 HTTP 리소스를 로드할 수 있고, 개발자는 트래픽 비용을 절감할 수 있습니다.
위에서 Cache-Control와 ETag 헤더를 리소스의 성격에 따라 잘 설정하는 것만으로 캐시를 정확하게 설정할 수 있습니다.
