# How to iterate over a collection in Java

Assume that there is an array containing String like below:
```
List<String> Countries = Arrays.asList("China", "England", "France", "Germany", "South Korea", "Japan", "United States")
```

> Way1. Use For-loop

```
for (int i = 0; i < countries.size(); i++) {
  System.out.println(countries.get(i));
}
```

```.get(int index)``` 메소드를 가지는 List 타입의 객체만 사용 가능하며, 상위 타입인 Collection이나 Set, Map 등의 이종 타입은 사용 불가하다.

> Way2. Use enhanced For loop: for-each

```
for (String country : countries) {
  System.out.println(country);
}
```

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

> Way4. Use Stream

*Java8부터 추가된 기능*으로, **주로 Collection, Array와 같은 저장 요소를 하나씩 참조하며 함수형 인터페이스(람다식)을 적용하며 반복적으로 처리할 수 있도록 해주는 기능** 이다.

- 스트림 생성

```
// Collection의 stream() 사용
List<String> list = Arrays.asList("a", "b", "c");
Stream<String> listStream = list.stream();

// Stream과 Array에 static 메서드로 정의
Stream<T> Stream.of(T... values); // 가변인자
Stream<T> Stream.of(T[]);
Stream<T> Arrays.stream(T[]);
Stream<T> Arrays.stream(T[] array, int startInclusive, int endExclusive);
```

- 중간연산
  + Filter, Map, Peek, Sorted, Limit, Distinct, Skip etc...
- 최종연산
  + reduce, forEach, collection, iterator, allMatch, anyMatch, noneMatch etc...
