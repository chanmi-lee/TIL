# Lombok

---

자바에서 Model (DTO, VO, Domain) Object를 만들 때, 멤버 필드에 대한 getter/setter, toString와 생성자를 만드는 코드 등 
불필요하게 반복되는 코드를 어노테이션을 통해 줄여주는 라이브러리

---

> @Data

A shortcut for 
- @ToString
- @EqualsAndHashCode
- @Getter on all fields
- @Setter on all non-final fields
  - AccessLevel 속성을 사용하여 `PUBLIC` (default), `PRIVATE`, `PACKAGE`, `PROTECTED`로 지정 가능
- @RequiredArgsConstructor

```
@Data
public class DataExample {
  private final String name;
  @Setter(AccessLevel.PACKAGE) private int age;
  private double score;
  private String[] tags;
  
  @ToString(includeFieldNames=true)
  @Data(staticConstructor="of")
  public static class Exercise<T> {
    private final String name;
    private final T value;
  }
}
```

@Data의 속성에는 staticConstructor가 있는데 이를 사용할 경우 static한 생성자를 만들어 주며, 
`new` 키워드로 객체를 생성하는 대신 아래와 같이 사용이 가능하다.

```
// instead of new Exercise("Test", 5);
Exercise.of("Test", 5);
```

---

> @NoArgsConstructor, @RequiredArgsConstructor and @AllArgsConstructor

- @NoArgsConstructor: generate a constructor with no parameters
- @RequiredArgsConstructor: `final` 접근자나 `@NotNull` annotation이 붙은 필드를 멤버변수로 하는 생성자를 생성
- @AllArgsConstructor: 모든 필드를 멤버변수로 하는 생성자를 생성

```
@RequiredArgsConstructor(staticName = "of")
@AllArgsConstructor(access = AccessLevel.PROTECTED)
public class ConstructorExample<T> {
  private int x, y;
  @NonNull private T description;
  
  @NoArgsConstructor
  public static class NoArgsExample {
    @NonNull private String field;
  }
}
```

---

> @NotNull

: a field (e.g. CharSequence, Collection, Map, or Array) constrained with `@NotNull` is valid as long as it’s not null, but it can be empty.

```
public class NotNullExample extends Something {
  @NotNull(message = "id is required")
  private String id;
  ...
}
```

ref)

> @NotEmpty

: a field (e.g. CharSequence, Collection, Map, or Array) constrained with `@NotEmpty` must be not null and its size/length must be greater than zero

```
@NotEmpty(message = "Name may not be empty")
@Size(min = 2, max = 32, message = "Name must be between 2 and 32 characters long") 
private String name;
```

> @NotBlank

: a constrained String is valid as long as it’s not null and the trimmed length is greater than zero.
It uses the NotBlankValidator class, which checks that a character sequence’s trimmed length is not empty

---

> @Value

: the **immutable** variant of @Data; all fields are made `private` and `final` by default, and setters are not generated.

```
public class RedisConfig {
  @Value("${spring.redis.host}")
  private String host;
  
  @Value("${spring.redis.port}")
  private int port;
  
  ...
}
```

---

Ref

- [Lombok features](https://projectlombok.org/features)
- [Difference between @NotNull, @NotEmpty and @NotBlank in Bean Validation in Java](https://www.baeldung.com/java-bean-validation-not-null-empty-blank)
