# Annotation

---

Basically, an annotation assigns extra metadata to the source code it’s bound to. By adding an annotation to a method, interface, class, or field, we can:

- Inform the compiler about warnings and errors
- Manipulate source code at compilation time
- Modify or examine behavior at runtime

---

> Java Built-in Annotations

- **@Override** : to indicate that a method overrides or replaces the behavior of an inherited method
- **@SuppressWarnings** : to ignore certain warnings from a part of the code
- **@Deprecated** : to mark an API as not intended for use anymore
- **@SafeVarargs** : acts on a type of warning related to using varargs
- **@FunctionalInterface** (introduced in Java 8 ~)

```
  @FunctionalInterface
  public interface Adder {
      int add(int a, int b);
  }

  Adder adder = (a,b) -> a + b;
  int result = adder.add(4,5);
```


---

> Meta Annotations

- **@Target** : to determine the target elements of a custom annotation

  - *ElementType.ANNOTATION_TYPE* can be applied to an annotation type. (i.e. another annotation)
  - *ElementType.CONSTRUCTOR* can be applied to a constructor.
  - *ElementType.FIELD* can be applied to a field or property.
  - *ElementType.LOCAL_VARIABLE* can be applied to a local variable.
  - *ElementType.METHOD* can be applied to a method-level annotation.
  - *ElementType.PACKAGE* can be applied to a package declaration.
  - *ElementType.PARAMETER* can be applied to the parameters of a method.
  - *ElementType.TYPE* can be applied to any element of a class.

```
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.CONSTRUCTOR, ElementType.METHOD})
public @interface SafeVarargs {
}
```

- **@Retention** : to say where in our program’s lifecycle our annotation applies

  - *RetentionPolicy.SOURCE* : (DEFAULT) The marked annotation is retained **only in the source level and is ignored by the compiler**
  - *RetentionPolicy.CLASS* : The marked annotation is retained **by the compiler at compile time, but is ignored by the Java Virtual Machine (JVM)**
  - *RetentionPolicy.RUNTIME* : The marked annotation is retained **by the JVM so it can be used by the runtime environment**

- **@Inherited** : to make our annotation propagate from an annotated class to its subclasses
- **@Documented**
- **@Repeatable** : (introduced in Java 8 ~) to specify the same annotation more than once on a given Java element

---

> Creating custom annotations

- Class level annotation

1) Declare it using the @interface keyword 
2) Add meta-annotations to specify the scope and the target of our custom annotation:

```
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.Type)
public @interface JsonSerializable {
}
```

- Field level annotation

```
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.FIELD)
public @interface JsonElement {
    public String key() default "";
}
```

> **[Caution]** 
Be aware that these methods must have **no parameters**, and **cannot throw an exception**. 
And, the return types are restricted to primitives, String, Class, enums, annotations, and arrays of these types, and the default value cannot be null


- Method level annotation

```
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Init {
}
```

---

> Applying custom annotations

```
@JsonSerializable
public class Person {
 
    @JsonElement
    private String firstName;
 
    @JsonElement
    private String lastName;
 
    @JsonElement(key = "personAge")
    private String age;
 
    private String address;
 
    @Init
    private void initNames() {
        this.firstName = this.firstName.substring(0, 1).toUpperCase() 
          + this.firstName.substring(1);
        this.lastName = this.lastName.substring(0, 1).toUpperCase() 
          + this.lastName.substring(1);
    }
 
    // Standard getters and setters
}
```

---

> Web-bind annotations

- @RequestMapping
  - @PostMapping: Shortcut for @RequestMapping(method = RequestMethod.POST)
  - @GetMapping: @RequestMapping(method = RequestMethod.GET)
  - @PutMapping: @RequestMapping(method = RequestMethod.PUT)
  - @DeleteMapping: @RequestMapping(method = RequestMethod.DELETE)
  - @PatchMapping: @RequestMapping(method = RequestMethod.PATCH)
  
```
@Data
public class Product {
  
  @NotNull(message = "product Id is required")
  private String prodId;
  
  @NotNull
  private String name;
  
  @NotNull
  private String type;
  
  @NotNull
  private String useYn;

  ...
}
```

```
@RestController // conbines @Controller and @ResponseBody
@RequestMapping(value = "/products", method = RequestMethod.POST)
public class ProductController {
  
  @Autowired
  private ProductService productService;
  
  @RequestMapping(value = "/getProductList")
  public ResponseEntity<ProductInfoOut> getProductList(@RequestBody ProductInfoIn in) throws Exception {
    ProductOut result = productService.getProductList(in);
    return ResponseEntity<ProductInfoOut>(result, HttpStatus.OK);
  }
}
```
  
- @PathVariable: Maps a method argument to a URI template variable
```
@RequestMapping("/{id}")
Product getProduct(@PathVariable long id) {
    // ...
}
```

- @RequestParam: Maps HTTP request parameters to method arguments
```
Product getProductByProdCd(
    @RequestParam("prodCd") String prodCd
    , @RequestParam(value = "useYN", required = false) String useYN
    , @RequestParam(value = "prodCnt", required = false, defaultValue = 1) int prodCnt) {
  
}
```
---
:bulb: @PathVariable vs @RequestParam

> Even though `@PathVariable` and `@RequestParam` are both used **to extract values from the URL**, their usage is largely determined by how a site is designed.
The `@PathVariable` is used **for data passed in the URI (e.g. RESTful web services)** while `@RequestParam` is used **to extract the data found in query parameters.**

If we head chosen to use query parameters, our URL would be:

`http://localhost:8080/orders?id=100`

This would be implemented in a controller method like the following:

```
@GetMapping("/orders")
@ResponseBody
public String getOrder(@RequestParam(value = "id", required = true) String id) {
  return "Order ID: " + id;
}
```

> So, these annotations can be mixed together inside the same controller.
---

- @RequestBody: Maps the body of the HTTP request (body) to an object
```
@PostMapping("/saveMappingProduct")
void saveProduct(@RequestBody Product product) {
  //...
}
```

- @RequestHeader: Get HTTP request header info
```
@RequestMapping(value = "/**")
public String redirect(@RequestHeader("User-Agent") String userAgent, HttpServletRequest request) {
  String versionCheckForRedirect = versionCheck(userAgent);
  if (versionCheckForRedirect.indexOf("redirect") > -1) {
    return versionCheckForRedirect;
  }
  
  String redirectUrl = redirectUrl(userAgent, request);
  if (redirectUrl.indexOf("redirect") > -1) {
    return redirectUrl;
  }
  
  return "forward:/home";
}
```

---

> @ Bean vs @Component

- @Bean : setter나 builder 등을 통해서 사용자가 프로퍼티를 변경해서 생성한 인스턴스를 스프링에게 관리하라고 맡기는 것

```
@Target({ElementType.METHOD, ElementType.ANNOTATION_TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Bean {
  @AliasFor("name")
  String[] value() default {};
  
  @AliasFor("value")
  String[] name() default {};
  
  @Deprecated
  Autorwire autowire() default Autowire.NO;
  
  boolean autowireCandidate() default true;
  
  String initMethod() default "";
  
  String destroyMethod() default "(inferred)";
}
```
> @Target이 METHOD & ANNOTATION_TYPE으로 지정

- @Component : 클래스를 스프링에게 알아서 인스턴스 생성한 후에 bean으로 등록하라고 맡기는 것

```
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Component {
  String value() default "";
}
```

> @Target이 TYPE으로 지정, 즉 Class annotation

---

Ref
- [Pre-defined Java Annotations](https://docs.oracle.com/javase/tutorial/java/annotations/predefined.html)
- [Overview of Java Built-in annotations](https://www.baeldung.com/java-default-annotations)
- [Custom annotations in Java](https://www.baeldung.com/java-custom-annotation)
- [Spring MVC Annotations](https://www.baeldung.com/spring-mvc-annotations)
