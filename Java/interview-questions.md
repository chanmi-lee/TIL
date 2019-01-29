# Java Interview Questions



> Q1. Overloading vs Overriding

**Overrloading** 은 두 메서드가 같은 이름을 갖고 있으나, 인자의 수나 자료형이 다른 경우를 지칭한다.

```
public double computeArea(Circle c) { ... }
public double computeArea(Square s) { ... }
```

**Overriding** 은 상위 클래스의 메서드와 이름과 용례(signature)가 같은 함수를 하위 클래스에 재정의하는 것을 의미한다.

```
public abstract class Shape {
  public void printMe() {
    System.out.println("I am a shape.");
  }
  public abstract double computeArea();
}

public class Circle extends Shape {
  private double radious = 5;
  public void printMe() {
    System.out.println("I am a circle.");
  }

  public double computeArea() {
    return radious * radious * 3.15;
  }
}

public class Ambiguous extends Shape {
  private double area = 10;
  public double computeArea() {
    return area;
  }
}

public class IntroductionOverriding {
  public static void main (String[] args) {
    Shape[] shapes = new Shape[2];
    Circle circle = new Circle();
    Ambiguous ambiguous = new Ambiguous();

    shapes[0] = circle;
    shapes[1] = ambiguous;

    for (Shape s : shapes) {
      s.printMe();
      System.out.println(s.computeArea());
    }
  }
}
```

위 코드를 실행한 출력 결과는 다음과 같다.
```
I am a circle.  // Ambiguous는 printMe 메소드를 재정의(Overriding)
78.75
I am a shape.   // Ambiguous는 printMe 메소드를 그대로 둠
10.0
```

---

> Q2. Collection framework

**ArrayList** <br />
동적으로 크기가 조절되는 배열

```
ArrayList arr = new ArrayList();
arr.add("one");
arr.add("two");
System.out.println(arr.get(0));
```


**Vector** <br />
ArrayList와 비슷하지만 동기화(synchronnize)되어 있다는 차이가 있다.


**LinkedList** <br />
순환자(iterator)를 어떻게 사용하는지 잘 보여줌

```
LinkedList likedList = new LinkedList();
linkedList.add("two");
linkedList.addFirst("one");
Iterator iter = linkedList.iterator();
while(iter.hasNext()) {
  System.out.println(iter.next());
}
```

**HashMap** <br />
key-value 쌍으로 이루어진다. <br />
검색과 삽입에 O(1) 시간이 소요된다. <br />
키를 기준으로 순회할 때 키의 순서는 무작위로 섞여있다.

> Q2-1. TreeMap, HashMap, LinkedHashMap의 차이?

세 가지 모두 key-value의 대응 관계가 있고, key를 기준으로 순회(iterate) 가능하다. <br />
시간 복잡도와 키가 놓이는 순서에 차이가 있다.

| HashMap | Tree Map | LinkedHashMap |
| ---- | ---- | ---- |
| 검색/삽입에 O(1) | 검색/삽입에 O(logN) | 검색/삽입에 O(1) |
| 키 순서 무작위 | 정렬된 순서로 키 순회 가능 | 키는 삽입된 순서대로 정렬 <br /> 양방향 연결 버킷(double-linked bucket)으로 구현 |
