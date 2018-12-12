# JavaScript

#### Data Type

- Premitive type
  - String
  - Number
  - Boolean
  - null
  - undefined
  - Symbol (new In ES6)
    + a unique & immutable primirive value and used as the key of an Object property
    + [More Details](https://developer.mozilla.org/en-US/docs/Glossary/Symbol)
- Object
  - Array / typed Array
  - keyed collections: (weak) Map, (weak) Set
  - structured data: JSON
  - Dates (built-in)

---

#### Run JavaScript

- Script tag in HTML
```
<script>
  document.querySelector('body').style.backgroundColor = 'black';
</script>
```
- Event Handler
  + onclick, onchange, onkeydown 등의 tag attribute는 javascript code를 값으로 가진다.
- console
  + console 창에서 실행을 유보하고 싶을 때? **Shift + Enter**

---

#### Statement

- Conditional
- Loop
- Function
  + parameter (매개변수)
  + argument (인자)

```
function sum(left, right) { // left, right : parameters
  return left+right;
}
sum(2,3); // 5
```
- Object
  - Property
    + Object에 소속된 변수
    + key(or name) : value 로 구성
      + key : strings (or Symbols)
      + value : anything including function
    + ``` objectName.propertyName ``` 과 같이 접근한다. (도트표기법)
    + null과 undefined를 제외한 모든 원시 타입도 객체로 취급. (즉, 원시타입에도 프로퍼티가 추가되며 객체로서의 특징을 갖는다)
  - Method: Object와 연관된 함수

```
var myObj = {
  myMethod: function(params) {
    // ...do something
  }
}
```

  - 모든 객체들은 최소한 하나의 다른 객체로부터 상속을 받는다.
    + 상속을 제공하는 객체를 **ProtoType**
    + 상속되는 속성들은 ```prototype```이라는 생성자 객체에서 찾을 수 있다.


#### Useful Links

+ [Mozilla - JavaScript 안내서](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide)
+ [OpenTutorials - JavaScript 사전](https://opentutorials.org/course/50)
+ [DOM Event](https://developer.mozilla.org/en-US/docs/Web/Events)
