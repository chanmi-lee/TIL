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
    + JavaScript에서 함수는 값이다.
     - 다른 함수의 인자로 전달될 수도 (parameter)
     - 함수의 리턴 값으로 사용될 수도 (like callback function)
     - 배열의 값으로도 사용될 수도 있다.

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

---

#### Scope
- 유효범위, 즉 변수의 수명을 의미
  + 전역변수
  + 지역변수
- JavaScript는 함수에 대한 유효범위만을 제공한다. (<-> 다른 많은 언어들이 블록단위({})에 대한 유효범위를 제공하는 것과 다른 점)

```
// JavaScript
for (var i = 0; i < 1; i++) {
  var name = 'Kate';
}
alert(name);  // return Kate
```

```
// Java
for (int i = 0; i < 10; i++) {
  String name = "Kate";
}
System.out.println(name); // do not return anything
```
- JavaScript는 함수가 선언된 시점에서의 유효범위를 갖는다. 이를 정적 유효범위(static scoping) 혹은 렉시컬(lexical scoping)이라 한다.

```
var i = 10;

function a() {
  var i = 5;
  b();
}

function b() {
  document.write(i);
}

a();  // 5
```


---

#### Events
- capturing: event가 부모에서부터 발생하여 자식으로 전파되는 것
  + ie 낮은 버전에서는 작동하지 않는다

---

#### Useful Links

+ [Mozilla - JavaScript 안내서](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide)
+ [OpenTutorials - JavaScript 사전](https://opentutorials.org/course/50)
+ [DOM Event](https://developer.mozilla.org/en-US/docs/Web/Events)
