# AOP (Aspect Oriented Programming)

어플리케이션 전반에 필요하지만 중복해서 작성해야 하는 핵심 이외의 코드들은 외부로 빼놓는 것

핵심 관심 모듈 내에선 횡단 관심 모듈을 직접 호출하지 않고 위빙(Weaving)이라 불리는 작업을 통해 횡단 관심 코드가 삽입되도록 한다.
예를 들어, 로그나 보안 처리, 트랜젝션 등의 작업이 있다.

위빙(Weaving)을 하기 위해서는 1) 어디에, 2) 어제, 3) 어떤 코드를 삽입할 지가 중요

- #### Pointcut : 어느 부분(Where)에 횡단 관심 모듈을 삽입할 지 정의

`@Pointcut("execution(public * (@org.springframework.sterotype.Controller com.biz..*).*(..))")`


- #### Joinpoint : 언제(When) 횡단 관심 모듈을 삽입할 지 정의
  1) @Before : 메서드 실행 전
  2) @After : 메서드 실행 후
  3) @AfterReturning: 반환된 후
  4) @AfterThrowing: 예외가 던져지는 시점
  5) @Around: 메서드 실행 전/후 (@Before + @After)
  
- #### Advice : 핵심 관심 모듈에 삽입 될 횡단 관심 모듈 자체(What)를 의미

```
@Around("execution(* com.tutorials.biz.Controller) || execution(* com.tutorials.biz.Service)")
```

> Pointcut expression에는 AND, OR, NOT과 같은 관계연산자를 이용할 수 있다.

---

ex)

```
@Aspect
@Component
public class LogAdvice {
  private static final Logger logger = LoggerFactory.getLogger(LogAdvice.class);
  
  @Around("execution(* com.tutorials.biz.Controller) || execution(* com.tutorials.biz.Service)")
  public Object logPrint(ProceedingJoinPoint joinPoint) throws Throwable {
    long start = System.currentTimeMills();
    
    Object result = joinPoint.proceed();
    
    String type = joinPoint.getSignature().getDeclaringTypeName();
    String name = "";
    
    if (type.contains("Controller")) {
      name = "Controller : ";
    } else if (type.contains("Service")) {
      name = "Service : ";
    }
    
    long end = System.currentTimeMills();
    
    logger.info(name + type + "."+joinPoint.getSignature().getName());
    logger.info("Args/Params : " + Arrays.toString(joinPoint.getArgs()));
    logger.info("Execution Time : " + (end-start));
    logger.info("=================================================================================");
    
    return result;
  }
}
```

---

Ref

- [AOP를 이용한 LogAdvice 작성](https://doublesprogramming.tistory.com/207)
