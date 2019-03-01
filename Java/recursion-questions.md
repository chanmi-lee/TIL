## Recursion

> Q1. 최대공약수 (via Euclid Method)

m &ge; n인 두 양의 정수 m과 n에 대해서 m이 n의 배수이면 gcd(m,n)=n이고, 그렇지 않으면 gcd(m,n)=gcd(n,m%n)이다.

예를 들어, 두 수를 24, 30이라 할 때,

24 = 2 x 2 x 2 x 3,
30 = 2 x 3 x 5

최대 공약수(gcd) = 6 (공통된 약수는 2 * 3)

```
public static double gcd(int m, int n) {
  if (m < n) {
    // swap m and n
    int temp = m;
    m = n;
    n = temp;
  }

  if (m%n == 0) {
    // m이 n의 배수이면 gcd(m,n)=n
    return n;
  } else {
    return gcd(n, m%n);
  }
}
```


좀 더 간단한 버전은,

```
public static int gcd(int p, int q) {
  if (q==0) {
    return p;
  } else {
    return gcd(q, p%q);
  }
}
```

---

> Q2. 문자열의 길이 계산

간단하게 라이브러리를 사용해서 구할 수도 있지만, 재귀를 사용하여 문자열의 길이를 계산해보면 아래와 같이 생각할 수 있다.

```
public static int length(String str) {
  if (str.equals("")) { // 빈 문자열인 경우
    return 0;
  } else {
    return 1+length(str.substring(1));
  }
}
```

---

> Q3. 문자열을 한 글자씩 출력하기

이 역시 재귀를 사용해보자.

```
public static void printChars(String str) {
  if (str.length()==0) {  // 빈 문자열인 경우
    return;
  } else {
    System.out.print(str.charAt(0));  // 맨 앞의 문자열을 출력
    printChars(str.subString(1)); // 이후 문자열을 매개로 하여 재귀함수 호출
  }
}
```

---

> Q4. 문자열을 역순으로 출력하기

잘 생각해보면 Q3의 경우와 조금만 다르다!

```
public static void printCharsReverse(String str) {
  if (str.length()==0) {
    return;
  } else {
    printCharsReverse(str.substring(1));
    System.out.print(str.charAt(0));
  }
}
```

---

> Q5. 2진수로 변환하여 출력

재귀함수를 사용하여 음이 아닌 정수 n을 이진수로 변환하여 출력한다.

```
public void printInBinary(int n) {
  if (n < 2) { // 즉, 0 혹은 1이라면
    System.out.print(n);
  } else {
    // n을 2로 나눈 몫을 먼저 2진수로 변환하여 출력한다
    printInBinary(n/2);

    // n을 2로 나눈 나머지를 출력한다
    System.out.print(n%2);
  }
}
```

---

> Q6. 배열의 합 구하기

```
public static int sum(int n, int[] data) {
  if (n <= 0) { // 더 이상 더할 정수가 없다면
    return 0;
  } else {
    // 입력받은 data[0]에서 data[n-1]까지의 합을 구하여 출력한다
    return sum(n-1, data) + data[n-1];
  }
}
```

---

> Q7. 데이터파일로부터 n개의 정수 읽어오기

Scanner 함수를 통해 참조하는 파일로부터 n개의 정수를 입력받아 배열 data에 저장한다.

```
public void readFrom(int n, int[] data, Scanner in) {
  if (n == 0) {
      return;
  } else {
    readFrom(n-1, data, in);
    data[n-1] = in.nextInt();
  }
}
```

---

> Q8. 순차탐색

배열의 앞에서부터 target 변수값과 일치하는 값을 찾아 반환한다.

```
public int search(int[] data, int begin, int end, int target) {
  if (begin > end) {
    return -1;
  } else if (target == items[begin]) {
    return begin;
  } else {
    return search(data, begin+1, end, target);
  }
}
```

배열을 반으로 나눠 찾는 경우엔 아래와 같다

```
public int search(int[] data, int begin, int end, int target) {
  if (begin > end) {
    return -1;
  } else {
    int middle = (begin+end)/2;
    if (data[middle] == target) return middle;

    int idx = search(data, begin, middle-1, target);
    if (idx != -1) {  // 앞에서 찾은 경우
       return idx;
    } else {  // 뒤에서 찾은 경우
      return search(data, middle+1, end, target);
    }
  }
}
```

---

> Q9. 최대값 찾기

배열의 첫번째 값과 나머지 값을 비교하여 찾는 방법
```
public int findMax(int[] data, int begin, int end) {
  if (begin == end) {
    return data[begin];
  } else {
    return Math.max(data[begin], findMax(data, begin+1, end));
  }
}
```

혹은, 배열을 반으로 나눠 찾는 방법도 있다.

```
public int findMax(int[] data, int begin, int end) {
  if (begin == end) {
    return data[begin];
  } else {
    int middle = (begin+end)/2;
    int max1 = findMax(data, begin, middle);
    int max2 = findMax(data, middle+1, end);
    return Math.max(max1, max2);
  }
}
```

---

> Q10. Binary Search

기본적으로 배열에 데이터가 크기순 혹은 오름차순으로 정렬되어 있을 때 적용할 수 있는 방법이다.

```
public static int binarySearch(String[] items, String target, int begin, int end) {
  if (begin > end) {  // 빈 배열일 때
    return -1;
  } else {
    int middle = (begin+end)/2;
    int compResult = target.compareTo(items[middle]);
    if (compResult == 0) {
      return middle;
    } else if (compResult < 0) {
      return binarySearch(items, target, begin, middle-1)
    } else {
      return binarySearch(items, target, middle+1, end);
    }
  }
}
```

---

##### Tips
- **Recursion vs Iteration**
  - 모든 순환함수(Recursion)은 반복문(Iteration)으로 변경 가능하다
  - 그 역도 성립한다. 즉, 모든 반복문은 Recursion으로 표현 가능하다
  - 순환함수로 복잡한 알고리즘을 단순하고 알기쉽게 표현할 수 있지만, 함수 호출에 따른 오버헤드가 있다 (매개변수 전달, 액티베이션 프레임 생성)
