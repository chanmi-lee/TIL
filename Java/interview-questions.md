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
public class MyStack {
  private static class StackNode {
    private T data;
    private StackNode next;
    public StackNode(T data) {
      this.data = data;
    }
  }

  private StackNode top;
  public T pop() {
    if (top == null) throw new EmptyStackException();
    T item = top.data;
    top = top.next;
    return item;
  }

  public void push(T item) {
    StackNode t = new StackNode(item);
    t.next = top;
    top = t;
  }

  public T peek() {
    if (top == null) throw new EmptyStackException();
    return top.data;
  }

  public boolean isEmpty() {
    return top == null;
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

**Bouble Sort**

*서로 인접한 두 원소를 검사하여 정렬하는 알고리즘*

배열의 첫 원소부터 순차적으로 진행하며, 현재 원소가 그 다음 원소의 값보다 크면 두 원소를 바꾸는 작업을 반복한다.

```
public class BubbleSortExample {
  static void bubbleSort(int[] arr) {
    int len = arr.length;
    int temp = 0;

    for (int i = 0; i < len; i++) {
      for (int j = 1; k < (len-1); j++) {
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

**Selection sort**

배열을 선형 탐색(linear scan)하며 가장 작은 원소를 배열 맨 앞으로 보낸다 (맨 앞에 있던 원소와 자리를 바꾼다).

```
public clas SelectionSortExample {
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
