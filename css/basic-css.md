# CSS (Cascading Style Sheet)

- Cascading: 하나의 Element에 대해 다양한 효과가 영향력을 행사하려고 할 때의 우선순위를 어떻게 설정하는가?
  + 웹브라우저 < 사용자 < 웹페이지 저자
  + tag selector < class selector < id selector < style attribute ( < !important; )

디자인하려는 태그를
1. 선택하고 (선택자)
2. 선택한 대상에게 효과를 준다 (선언)

![](http://www.pxleyes.com/blog/wp-content/uploads/2010/03/css-cheatsheet.png)

---

![](https://www.w3schools.com/css/selector.gif)

> **Rule Set**

*HTML 페이지 안의 특정 요소들을 어떻게 렌더링 할 것인지 브라우저에게 알려주는 CSS 문장*

{ } (semicolon) 앞 부분 모두가 선택자(selector) 로 다음과 같은 종류가 있다.

| 종류 | 예제 | 의미 |
| --- | --- | --- |
| universial selector | * | HTML페이지 내부의 모든 태그를 선택 |
| id selector | #firstname | id=firstname인 태그를 선택 |
| class selector | .intro | class=intro인 태그를 선택
| element selector | p, div | 태그명이 p, div인 특정 태그를 선택 |
| attribute selector | [target=_blank] | target 속성값이 _black인 특정 태그를 선택 |
| Pseudo-Classes selector | a:visited, a:focus | a:visited 선택자는 이미 방문한 적이 있는 a 태그를, a:focus 선택자는 해당 요소에 초점이 맞춰져 있는 상태일 때 적용 |

* **가상 클래스(Pseudo-Classes)** 는 웹 문서의 소스에는 실제로 존재하지 않지만 필요에 의해 임의로 가상의 선택자를 지정하여 사용하는 것을 의미한다*

```
a:link {  /* 방문한 적이 없는 링크 */
    color: black;
}
a:visited {  /* 방문한적이 있는 링크 */
    color: red;
}
a:hover {   /* 마우스를 롤오버 했을 때 */
    color: yello;
}
a:active {  /* 마우스를 클릭했을 때 */
    color: green;
}
```

> **[Tip]** "@" 키워드로 시작하는 문장은 Rule Set이 아닌 At-Rule으로 선택자가 아니다. (하단 Media Query 부분 참고)

---

- Web font : 사용자가 가지고 있지 않은 폰트를 웹 페이지에서 사용하기 위한 방법으로, 폰트를 서버에서 다운로드하는 방식
  + Link: [Google web fonts](https://fonts.google.com/?authuser=1)

```
<html>
  <head>
    <link href="https://fonts.googleapis.com/css?family=Nanum+Myeongjo|PT+Sans|Roboto+Condensed" rel="stylesheet">
  </head>
  <body>
    ...
  </body>
</html>
```


---

## Responsive Design

- 화면의 종류와 크기에 따라 웹 페이지의 각 요소들이 반응하도록 디자인을 달리 주는 것

#### Media Query

```
/* For Mobile */
<meta name="viewport" content="width=device-width, initial-scale=1.0">

@media(min-width:800px) {  // screen width > 800px
  div {
    display: none;
  }
}

@media(max-width:800px) {
  #grid {
    display: block;
  }
}
```

---

## CSS Preprocessor

표준화된 CSS의 문법을 따르지 않고, CSS가 제공하지 않는 새로운 기능을 더한 새로운 언어에 따라 작성한 코드로 결과적으로 컴파일러를 통해 CSS포맷으로 반환된다

- [LESS](http://lesscss.org/)
- [SASS](http://sass-lang.com/)
  + CSS와 거의 유사한 문법!
  + 대표적인 차이는 {} 와 ;의 유무
  + 좀 더 자세한 내용은 To Be Continued...
- [stylus-lang](http://stylus-lang.com/)


#### Pros
- 재사용성
- 유지관리

---

### Minify

CSS 코드의 크기를 줄여주는 도구

- [clean-css](https://github.com/jakubpawlowicz/clean-css)

Pre-req) node.js install

```sh
> sudo npm install -g clean-css
```

```sh
> clean-css -o minify.min.css minify.css
```


---

References

* [W3SCHOOL - CSS Syntax and Selector](https://www.w3schools.com/css/css_syntax.asp)
* Link: [상속하는 속성과 하지않는 속성](https://www.w3.org/TR/CSS21/propidx.html)
