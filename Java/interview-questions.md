# Java Interview Questions

* * *

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

* * *

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
모두 List 인터페이스를 구현하고, 삽입 순서를 유지한다는 점, 동적으로 크기가 조절되는 배열을 필요로 할 때 사용된다는 점에서 ArrayList와 비슷하다.

> **Vector는 동기화(synchronnize)되어 있다는 점에서 차이가 있다.**
*즉, 한 번에 하나의 스레드만 Vector 메서드를 호출할 수 있다. (Thread-safe)*


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


**Stack** <br />
말 그대로 데이터를 쌓아올린다(stack)는 의미 <br />
LIFO(Last-In-First-Out)에 따라 자료를 배열

- ```pop()```: 가장 위에 있는 항목을 제거
- ```push()```: item 하나를 가장 윗 부분에 추가
- ```peek()```: 가장 위에 있는 항목을 반환
- ```isEmpty()```: 스택이 비어있을 때 true 반환

*같은 방향에서 item을 추가/삭제한다는 조건 하에 LinkedList로 구현할 수도 있다.*

```
class Stack {
  int top;
  int[] stack;
  int size;

  public Stack(int size) {
    top = -1;
    stack = new int[size];
    this.size = size;
  }

  public int peek() {
      return stack[top];
  }

  public int push (int value) {
    stack[top++] = value;
  }

  public int pop () {
    return stack[top--];
  }  
}
```


**Queue** <br />
FIFO (First-In-First-Out) like 매표소 줄

- ```add(item)```: item을 리스트의 끝 부분에 추가
- ```remove()```: 리스트의 첫 번째 항목 제거
- ```peek()```: 가장 위에 있는 항목 반환
- ```isEmpty()```: 큐가 비어있을 때 true 반환

* * *

> Q3. Algorithm

**Binary Search**

```
public class BinarySearch {
  public static void main (String[] args) {
    List<Integer> list = new ArrayList<>();

    System.out.println(binarySearch(list));
  }

  public static boolean binarySearch(final List<Integer> numbers, Integer value) {
    if (numbers == null || numbers.isEmpty()) {
      return false;
    }

    Integer comparsion = numbers.get(numbers.size() / 2);

    if (value.equals(comparsion)) return true;

    if (value < comparsion) {
      return binarySearch(numbers.subList(0, numbers.size() / 2), value);
    } else {
      return binarySearch(numbers.subList(numbers.size() / 2, numbers.size()), value);
    }
  }
}
```

---

**Bubble Sort**

*서로 인접한 두 원소를 검사하여 정렬하는 알고리즘*

> 실행시간 : O(n<sup>2</sup>) = (n-1)+(n-2)+...+2+1

*최악, 최선, 평균 모두 Q(n<sup>2</sup>)*

배열의 첫 원소부터 순차적으로 진행하며, 현재 원소가 그 다음 원소의 값보다 크면 두 원소를 바꾸는 작업을 반복한다.

```
public class BubbleSortExample {
  static void bubbleSort(int[] arr) {
    int len = arr.length;
    int temp = 0;

    for (int i = 0; i < len; i++) {
      for (int j = 1; j < (len-1); j++) {
        if (arr[j-1] > arr[j]) {
          // swap elements
          temp = arr[j-1];
          arr[j-1] = arr[j];
          arr[j] = temp;
        }
      }
    }
  }

  public static void main(String[] args) {
    int[] arr= {3,2,30,45,230,1,503};

    System.out.println("Array Before Bubble Sort");
    for (int i : arr) {
      System.out.print(i + " ");
    }

    bubbleSort(arr);

    System.out.println("Array After Bubble Sort");
    for (int j : arr) {
      System.out.print(j + " ");
    }
  }
}
```

Output:
```
Array Before Bubble Sort
3 2 30 45 230 1 503
Array After Bubble Sort
1 2 3 30 45 230 503
```

---

**Selection sort**

> 시간복잡도 T(n) = O(n<sup>2</sup>) = (n-1)+(n-2)+...+2+1

*최악, 최선, 평균 모두 Q(n<sup>2</sup>)*

배열을 선형 탐색(linear scan)하며 가장 작은 원소를 배열 맨 앞으로 보낸다 (맨 앞에 있던 원소와 자리를 바꾼다).

```
public class SelectionSortExample {
  public static void main(String[] args) {
    int[] arr = {3,2,30,45,230,1,503};
    int temp = 0;

    for (int i = 0; i < arr.length-1; i++) {
      for (int j = i+1; j < arr.length; j++) {
        if (arr[i] > arr[j]) {
          temp = a[j];
          a[j] = a[i];
          a[i] = temp;
        }
      }
    }
  }
}
```

---

**Quick sort**

무작위로 선정된 한 원소를 사용하여 배열을 분할하는데, 선정된 원소보다 작은 원소들은 앞에, 큰 원소들은 뒤로 보낸다.

*배열 분할에 사용되는 원소가 중간값(median), 적어도 중간값에 가까운 값이 되리라는 보장이 없기 때문에, 정렬 알고리즘이 느리게 동작할 수도 있다.
**최악의 경우ㅡpivot 값을 기준으로 모든 값들이 한 편으로 몰리는 경우, 수행 시간이 O(n^2)이 될 수도 있다.*

```
public class QuickSortExample {
  void quickSort(int[] arr, int left, int right) {
    if (left >= right) return;  // 원소 1개인 경우

    int index = partition(arr, left, right);

    if (left < index - 1) {  // 왼쪽 절반 정렬
      quickSort(arr, left, index - 1);
    }
    if (index < right) {    // 오른쪽 절반 정렬
      quickSort(arr, index, right);
    }
  }

  int partition(int[] arr, int left, int right) {
    int pivot = arr[(left+right)/2]; // 분할 기준 원소 생성

    while (left <= right) {
      // 왼쪽에서 오른쪽으로 옮겨야 하는 원소 탐색
      while (arr[left] < pivot) left++;

      // 오른쪽에서 왼쪽으로 옮겨야 하는 원소 탐색
      while (arr[right] > pivot) right--;

      // 원소를 스왑한 뒤 left와 right를 이동
      if (left <= right) {
        swap(arr, left, right);
        left++;
        right--;
      }
    }
    return left;
  }

  void swap(int[] arr, int left, int right) {
    int temp = arr[left];
    arr[left] = arr[right];
    arr[right] = temp;
  }
}
```

---

**Merge Sort**

분할정복법이라는 전략을 사용하는 알고리즘
* 분할: 해결하고자 하는 문제를 작은 크기의 동일한 문제들로 분할 (divide)
* 정복: 각각의 작은 문제를 순환적으로 해결 (recursively sort)
* 합병: 작은 문제의 해를 합하여 (merge) 원래 문제에 대한 해를 구함

ex) 주어진 배열의 최대값을 구해야 할 때, 반으로 나눠 (**분할**) 각각 최대값을 찾은 후 (**정복**) 값을 비교하여 최대값을 도출 (**합병**)

```
public class MergeSortExample {
  void mergeSort(int[] arr, int p, int r) {
    if (p < r) {
      int middle = (p+r)/2;

      mergeSort(arr, p, middle);
      mergeSort(arr, middle+1, r);
      merge(arr, p, middle, r);
    }
  }

  void merge(int[] data, int p, int m, int r) {
    int i = p, j = m+1, k = p;
    int[] tmp[data.length()];

    while (i <= q && j <= r) {
      if (data[i] <= data[j]) {
        tmp[k++] = data[i++];
      } else {
        tmp[k++] = data[j++];
      }
    }

    while (i <= q) {
      // 앞의 배열에만 원소가 남아있을 때
      tmp[k++] = data[i++];
    }
    while (j <= r) {
      // 뒤의 배열에만 원소가 남아있을 때
      tmp[k++] = data[j++];
    }

    for (int i = p; i <= r; i++) {
      data[i] = tmp[i];
    }
  }
}
```

---

> Q4. final vs finalize vs finally

| final | finalize | finally |
| --- | --- | --- |
| 변수, 메서드 또는 클래스가 **변경 불가능**하도록 만든다 | GC(Garbage Collector)가 더 이상의 참조가 존재하지 않는 객체를 삭제할 때 사용된다 | try/catch 구문이 종료될 때 항상 실행될 코드 블록을 정의하는데 사용된다 |

final keyword는 사용되는 문맥에 따라 다르다

* *final* fileds, parameters, and variables
: **상수를 정의** 할 때 사용한다

```
public class Parent {
  int field1 = 1;
  final int field2 = 2; // 상수 정의, 선언 시 반드시 초기값을 지정해야 한다

  Parent() {
    field1 = 2; // OK
    field2 = 3; // Compilation error : 한 번 정의되면 값을 변경할 수 없다
  }
}
```

* *final* method
: **해당 메서드는 더 이상 오버라이딩 할 수 없음** 을 의미한다

```
public class Child extends Parent {
  @Override
  void method1(int arg1, int arg2) {
    // OK
  }

  @Override
  final void method2() {
    // Compilation error
  }
}
```

> Java API에 있는 final method의 좋은 예는 **Object 클래스의 getClass 메서드** 이다. 서브클래스 객체가 자신이 속한 클래스를 속일 수 있게 하는 것은 좋지 않으므로 이 메서드는 절대 변경될 수 없게 만들어 놓았다.

```
    public final Class<?> getClass()
```

```
    Number n = 0; 
    Class<? extends Number> c = n.getClass();

    Object obj = System.out;
    Class<?> cl = obj.getClass();

    System.out.println("This object is an instance of " + cl.getName());

    String className = "java.util.Scanner";
    cl = Class.forName(className);
        // java.util.Scanner 클래스를 기술하는 객체다.
    cl = java.util.Scanner.class;
    System.out.println(cl.getName());
    Class<?> cl2 = String[].class; // String[] 배열 타입을 기술한다.
    System.out.println(cl2.getName());
    System.out.println(cl2.getCanonicalName());
    Class<?> cl3 = Runnable.class; // Runnable  인터페이스를 기술한다. 
    System.out.println(cl3.getName());
    Class<?> cl4 = int.class; // int 타입을 기술한다.
    System.out.println(cl4.getName());
    Class<?> cl5 = void.class; // void 타입을 기술한다.
    System.out.println(cl5.getName());
```

* *final* class
: **클래스를 상속받을 수 없음** 을 의미한다

```
public final class Child extends Parent {
  // ...
}
```

```
public class GrandChild extends Child {
  // Compilation error
}
```

* * *

> Q5. Type Parameter Naming Convention

A type variable can be any **non-primitive type** you specify: *any class type, any interface type, any array type, or even another type variable.*

By convention, type parameter names are single, uppercase letters.

The most commonly used type parameter names are:

* E - Element (used extensively by the Java Collections Framework)
* K - Key
* N - Number
* T - Type
* V - Value
* S,U,V etc. - 2nd, 3rd, 4th types


* * *

OOP

> 1. Abstraction (추상화)

공통된 특징, 즉 추상적 특징을 파악하여 하나의 개념(집합)으로 다루는 것을 의미

```
switch (carType)
case '아우디':
case '벤츠':
case 'BMW':
...
end switch()
```

```
void changeEngineOil (Car c) {
  c.changeEngineOil();
}
```

> 2. Encapsulation (캡슐화)

**정보은닉 (information hiding)** 을 통해 높은 응집도와 낮은 결합도를 갖도록 한다.
- 응집도(cohesion): 클래스나 모듈 안의 요소들이 얼마나 밀접하게 관련되어 있는지 나타낸다
- 결합도(Coupling): 어떤 기능을 실행하는데 다른 클래스나 모듈들에게 얼마나 의존적인지를 나타낸다

> 3. Polymorphism (다형성)

서로 다른 클래스의 객체가 각자의 방식으로 동작하는 방식을 의미한다
일반적으로 상속과 연계되어 동일한 부모로부터 상속받은 메소드를 달리 정의하는 오버라이딩과 연관지어 생각할 수 있다.
