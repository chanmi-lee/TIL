# Java

#### Type
> Primitive Type (기본자료형)
    - Boolean
    - Numeric
      + Integral
        - Integer: short, int, long
        - Floating Point: float, double
      + Character: char

*primitive type은 반드시 사용하기 전에 선언(Declared) 되어야 한다*

*비객체 타입으로 null 값을 가질 수 없다.*

---

> Reference Type (참조 자료형)
    - Interface (annotation을 포함)
    - array
    - class (enum(열거자료형)을 포함)
      + class의 멤버로는 field, method, member class, member interface 등이 있다.


*기본적으로 ```java.lang.Object```를 상속받는다.*

** ref)**

String Class
- reference type에 속하지만 기본적인 사용은 primitive type처럼 사용된다
- 불변(immutable) 객체
- 기본형 비교는 ```==``` 연산자를 사용하나 (identity comparison), String 객체간의 비교는 ```.equals()``` 메소드를 사용

Wrapper Class
- primitive type을 class로 감싼 형태
- primitive type에 null을 넣고 싶다면 Wrapper Class를 활용


| Primitive type | Wrapper Class |
| ------ | ------ |
| byte | [Byte](https://docs.oracle.com/javase/7/docs/api/java/lang/Byte.html) |
| short | [Short](https://docs.oracle.com/javase/7/docs/api/java/lang/Short.html) |
| int | [Integer](https://docs.oracle.com/javase/7/docs/api/java/lang/Integer.html) |
| long | [Long](https://docs.oracle.com/javase/7/docs/api/java/lang/Long.html) |
| float | [Float](https://docs.oracle.com/javase/7/docs/api/java/lang/Float.html) |
| double | [Double](https://docs.oracle.com/javase/7/docs/api/java/lang/Double.html) |
| char | [Character](https://docs.oracle.com/javase/7/docs/api/java/lang/Character.html) |
| boolean | [Boolean](https://docs.oracle.com/javase/7/docs/api/java/lang/Boolean.html) |

  - Autoboxing & Unboxing
    + Autoboxing: primitive type -> an object
    + Unboxing: wrapper object -> primitive value

```
List<Integer> list = new ArrayList<>();
list.add(1); // autoboxing

Integer val = 2; // autoboxing
// Internally,
Integer val = Integer.valueOf(2);
```  

*Internally, it uses the ```valueOf()```method to facilitate the conversion.*

```
Integer object = new Integer(1);
int initVal = object; // unboxing
int initVal2 = getSquareValue(object); // unboxing

public static int getSquareValue(int i) {
  return i*i;
}
```

> Caution

기본 타입과 래퍼 타입 사이의 변환은 예외 하나만 제외하고 거의 신경 쓸 필요가 없다.

==와 != 연산자는 객체의 내용이 아니라 객체 참조를 비교한다.

즉, 조건 `if (numbers.get(i) == numbers.get(j))`는 인덱스 i와 j에 있는 두 숫자가 같은지 검사하지 않는다.
따라서, *문자열처럼 래퍼 객체로 eqauls 메서드를 호출하여 객체를 비교해야 한다* 는 점을 기억하자.


---

##### 3 main types of references:
  + Strong references
  ```
    Integer prime = 1;
  ```
    - Any object which has a strong reference pointing to it is not eligible for GC.

  + Soft references
  ```
    Integer prime = 1;
    SoftReference<Integer> soft = new SoftReference<Integer>(prime);
    prime = null;   // After making that strong reference null, a prime object is eligible for GC but will be collected only when JVM absolutely needs memory.
  ```
    - An object that hs a SoftReference pointing to it won't be GC until the JVM absolutely needs memory.

  + Weak references
    - In previous example, when we made a prime referenec null, the prime object will be GC in the next GC cycle, as there is no other strong reference pointing to it.

** WeakHashMap as an efficient memory cache


---

##### 4 Initialization block

There are two types of Initialization block:

* instance initialization blocks
* static initialization blocks.

```
public class Test() {
    static int id;
    private String name = "";
    private int age;
    
    // static Initialization block 
    // Runs once (when the class is initialized)
    static {
        System.out.println("Static initialization block");
        Random generator = new Random();
        id = 1 + generator.nextInt(1_000_000);
    }
    
    // Instance initialization block
    // Runs each time you instantiate an object
    {
        System.out.println("Instance initialization block");
        age = 20;
    }
    
    public Test() {
        System.out.println("Constructor");
    }
    
    public static void main(String[] args) {
        new Test();
        new Test();
    }
}
```

Output
```
Static initalization
Instance initialization
Constructor
Instance initialization
Constructor
```

> 실행순서

- static initialization blocks of super classes
- static initialization blocks of the class
- instance initialization blocks of super classes
- constructors of super classes
- instance initialization blocks of the class
- constructor of the class.


---

Ref

* [Stackoverflow - what-is-an-initialization-block?](https://stackoverflow.com/questions/3987428/what-is-an-initialization-block)
