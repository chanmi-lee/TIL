# BOM (Browser Object Model)
- 웹 브라우저의 창이나 프레임을 추상화해서 제어할 수 있도록 제공하는 수단
- 전역객체인 Window의 property와 method를 통해 제어

---

#### Window
- 전역 객체
- 모든 객체가 소속된 객체 (전역변수, 함수 등)
- 창, 프레임을 의미
- 식별자 window를 통해 얻거나, 생략 가능

```
function func() {
  alert('Hello?');
}

func();
window.finc();
// JavaScript에서 모든 객체는 전역객체 window의 프로퍼티다!
```
- 호스트 환경에 따라 전역객체의 이름이 다르다.
  - 웹브라우저에선 window
  - node.js에선 global


- 호스트 환경이란?
  + 자바스크립트가 구동되는 환경을 의미
  + 자바스크립트는 브라우저를 위한 언어로 시작했지만, 더 이상 브라우저만을 위한 언어가 아니다.
    + node.js : 서버측에서 실행되는 자바스크립트
    + Google Apps Script : google spreadsheet와 같은 구글 제품 위에서 동작하는 자바스크립트


- Link: [전역객체 API](https://developer.mozilla.org/en-US/docs/Web/API/Window)

---

##### location
- 문서의 주소와 관련된 객체
- Window 객체의 property

#### navigator
- 브라우저의 정보를 제공하는 객체
- cross-browsing 등을 위해 사용됨


##### document
- Window 객체의 property
- window 객체가 창을 의미한다면, document 객체는 윈도우에 로드된 문서를 의미한다고 할 수 있다.
