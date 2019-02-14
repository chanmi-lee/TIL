# How to iterate over a collection in Java

Assume that there is an array containing String like below:
```
List<String> Countries = Arrays.asList("China", "England", "France", "Germany", "South Korea", "Japan", "United States")
```

---

> Way1. Use For-loop

```
for (int i = 0; i < countries.size(); i++) {
  System.out.println(countries.get(i));
}
```

```.get(int index)``` 메소드를 가지는 List 타입의 객체만 사용 가능하며, 상위 타입인 Collection이나 Set, Map 등의 이종 타입은 사용 불가하다.

---

> Way2. Use enhanced For loop: for-each

```
for (String country : countries) {
  System.out.println(country);
}
```

---

> Way3. Use Iterator

```
for (Iterator<String> iter = countries.iterator(); iter.hasNext();) {
  System.out.println(iter.next());
}
```

(참고) Iterator Interface는 아래와 같다
```
public interface Iterator<E> {
  boolean hasNext();
  E next();
  void remove();  // optional
}
```

---

> Way4. Use Stream

*Java8부터 추가된 기능*으로, **주로 Collection, Array와 같은 저장 요소를 하나씩 참조하며 함수형 인터페이스(람다식)을 적용하며 반복적으로 처리할 수 있도록 해주는 기능** 이다.

- 스트림 생성

스트림의 소스가 될 수 있는 대상은 배열, 컬렉션, 임의의 수, 파일 등 다양하다.

> Collection to Stream

```
// Collection의 stream() 사용
List<String> list = Arrays.asList("a", "b", "c");
Stream<String> listStream = list.stream();
```

> Array to Stream

```
// Stream과 Array에 static 메서드로 정의
Stream<T> Stream.of(T... values); // 가변인자
Stream<T> Stream.of(T[]);
Stream<T> Arrays.stream(T[]);
Stream<T> Arrays.stream(T[] array, int startInclusive, int endExclusive);
```

스트림에 정의된 메서드 중에서 데이터 소스를 다루는 작업을 수행하는 것을 연산(operation)이라고 한다. 스트림이 제공하는 연산은 **중간연산과 최종연산**으로 분류할 수 있다.

- 중간연산: 연산결과를 스트림으로 반환하기 때문에 중간 연산을 연속해서 연결할 수 있다.
  + Filter, Map, Peek, Sorted, Limit, Distinct, Skip etc...
- 최종연산: 스트림의 요소를 소모하면서 연산을 수행하기 때문에, 단 한번의 연산이 가능하다.
  + reduce, forEach, collection, iterator, allMatch, anyMatch, noneMatch etc...

```
Stream.of("a1", "b2", "c3", "a2", "b3")
  .filter(s -> {
    System.out.println("fileter: " + s);
    return false;
    });
```


중간연산의 중요한 특짐은 **나태함**이다. 위의 코드를 실행하면 콘솔에는 아무것도 출력되지 않는데, 이는 중간 연산은 단말 연산이 존재해야 실행되기 때문이다.

모든 가능한 스트림 연산을 보려면 [Stream Javadoc](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html) 을 참고하자.


```
int max = IntStream.of(20, 98, 12, 7, 35)
  .max()
  .getAsInt();  // returns 98
```

일반적인 객체 스트림 말고도, Java 8은 primitive type인 int와 long, double을 사용해서 작업하기 위한 특수한 종류의 스트림들을 들고 있다. 바로 IntStream, LongStream, DoubleStream이다.

*원시 타입을 쓰면 객체 생성 비용이 없기 때문에 데이터를 훨씬 효율적으로 처리할 수 있다.*

---

#### References
- [Java 8 Tutorial](https://winterbe.com/posts/2014/03/16/java-8-tutorial/)
- [Primitive Type Streams in Java8](https://www.baeldung.com/java-8-primitive-streams)
- [Naver D2 - 람다가 이끌어 갈 모던 Java](https://d2.naver.com/helloworld/4911107)
