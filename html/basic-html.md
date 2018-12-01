# HTML (HyperText Markup Language)

- HyperText : 단순 텍스트 이상의 요소들(링크, 이미지 등)이 포함된 텍스트
- Markup : <, >로 이루어진 Tag를 사용하는 규격

즉, 웹사이트에서 화면에 표시되는 정보들을 약속한 것이 HTML이다.
HTML외에도 XML도 대표적인 마크업 언어 중 하나이다.

HTML에서는 몇가지 태그가 미리 정의되어 있으며, 일반적으로 <tag>content</tag>와 같이 닫는 형식이다.
<br>, <img> 등 일부 태그는 닫는 태그가 없는 태그인데, 이는 태그 내부에 넣을 값이 없기 때문이다.

XHTML (XML + HTML)에서는 Self-Closing (ex- <br />)을 강제화 했지만, HTML5에서는 닫아주지 않아도 된다.



# DOCTYPE
HTML의 문서 규격을 표시하기 위해 사용되며, 문서 최상단에 위치한다. HTML, XHTML, HTML5의 세 가지 문서 유형이 존재하며, 유형에 따라 마크업 요소와 속성등을 처리하는 기준이 결정된다.
```sh
<!DOCTYPE html>
```
XHTML문서는 아래와 같은 DOCTYPE을 사용한다
```sh
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
```

문서형 정의(DTD, Document Type Definition)를 생략하는 경우, 웹 브라우저가 비표준모드로 렌더링되어 크로스 브라우징에 어려움을 겪는다.



# MIME (Multipart Internet Mail Extensions)
a.k.a 콘텐츠 타입

- HTTP Head 요소에 포함
- 문서 처리 방식 결정 기준

> type/substype

'/'로 구분된 두 개의 문자열인 타입과 서브타입으로 구분
타입의 종류로는 text, image, audio, video, application 그리고 multipart가 있다.

```sh
<meta http-equiv="Content-type" content="text/html; charset=utf-8" />
```
text/html; charset=utf-8 로 전달되는 문서는 HTML 파일로 해석된다.
