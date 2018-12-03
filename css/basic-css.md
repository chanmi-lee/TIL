# CSS (Cascading Style Sheet)

- Cascading: 하나의 Element에 대해 다양한 효과가 영향력을 행사하려고 할 때의 우선순위를 어떻게 설정하는가?
  + 웹브라우저 < 사용자 < 웹페이지 저자
  + tag selector < class selector < id selector < style attribute ( < !important; )


- 선택자, 선언, 속성, ...

```
a { // selector (선택자)
  color: red; // Declaration (선언)
  // property : value;
}
```

디자인하려는 태그를
1. 선택하고 (선택자)
2. 선택한 대상에게 효과를 준다 (선언)

![](http://www.pxleyes.com/blog/wp-content/uploads/2010/03/css-cheatsheet.png)


- Link: [상속하는 속성과 하지않는 속성](https://www.w3.org/TR/CSS21/propidx.html)

- psuedo class selector : 클래스 선택자는 아니지만, element의 상태에 따라 마치 클래스 선택자처럼 여러 element를 선택할 수 있다는 점에서 붙은 이름

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
