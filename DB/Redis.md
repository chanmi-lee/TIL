# Redis (Remote Dictionary Server)

> **휘발성**이면서 **영속성**을 가진 key-value 저장소

**[휘발성]** Redis는 디스크 기반이 아닌, 메모리에 데이터를 read/write하는 **in-memory 솔루션**

- 메모리에 데이터를 read/write 하기 때문에 매우 빠르다
- cache 방식을 통한 DB 부하 감소

**[영속성]** Redis는 지속성을 보장하기 위해 데이터를 디스크에 저장할 수 있다. DISK에 데이터를 저장하는 방식은 크게 두 가지가 있다.

- Snapshotting (RDB) 방식 : 순간적으로 메모리에 있는 내용을 디스크에 옮겨담는 방식
  - SAVE : blocking 방식으로 순간적으로 redis의 모든 동작을 정지시키고, 그 대의 snapshot을 디스크에 저장
  - BGSAVE : non-blocking 방식으로 별도의 process를 띄워, 명령어 수행 당시의 메모리 snapshot을 디스크에 저장 
- AOF (Append On File) 방식 : Redis의 모든 write/update 연산 자체를 모두 log 파일에 기록하는 방식
  - pros: log 파일에 append만 하기 때문에 log write 속도가 빠르며 서버 다운시에도 파일은 유지되므로 데이터 유실 위험이 적다
  - cons: operation이 발생할 때마다 매 번 기록하기 대문에 log 데이터 양이 과대하게 크며, 서버가 재기동 될 때 저장된 write/update operation을 순차적으로 재실행하여 데이터를 복구하므로 restart 속도가 느리다.

---

> NoSQL의 데이터 모델

* Key-Value
  - key:value=1:1
  - key로만 접근 가능
* Column
  - key:value=1:多
  - 중첩된 HashMap구조
* Document
  - value가 Json이나 XML Document를 갖는 데이터 모델
  - value의 일부로 질의하고 일부만 가져올 수 있음
* Graph
  - 관계에 특화된 데이터 모델
  
*Redis는 key-value 데이터 모델 뿐만 아니라 다양한 형태의 데이터 모델을 사용*

> Redis is *not a plain key-value store*, it is actually a data structures server, **supporting different kinds of values.** 
What this means is that, while in traditional key-value stores you associated string keys to string values, in Redis the value is not limited to a simple string, but can also hold more complex data structures.

[Redis : data-type-intro](https://redis.io/topics/data-types-intro)

---

> Redis가 제공하는 자료구조

* String
* Set
* Sorted Set
* Hashes
  - Map 자료
* List
  - `push` 통해 데이터를 저장, `pop` 을 통해 데이터를 추출 가능하며 Stack이나 Queue로 활용 가능

---

> Configuration @Spring Boot Project

* Maven Dependencies

```
<dependency>
    <groupId>org.springframework.data</groupId>
    <artifactId>spring-data-redis</artifactId>
    <version>2.0.3.RELEASE</version>
 </dependency>
 
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>2.9.0</version>
    <type>jar</type>
</dependency>
```

* Redis Client

- [Jedis](http://redis.io/clients#java)
- [spring-data-redis]() : Spring official Dependency, RedisTemplate 등의 기능 제공

```
@Configuration
public class RedisConfig {

  @Value
  private String host;
  
  @Value
  private String port;

  @Bean
  public JedisConnectionFactory connectionFactory() {
    JedisConnectionFactory connectionFactory = new JedisConnectionFactory();
    connectionFactory.setHostName(host);
    connectionFactory.setPort(port);
    return connectionFactory;
  }
  
  @Bean
  public RedisTemplate redisTemplate() {
    RedisTemplate redisTemplate = new RedisTemplate();
    redisTemplate.setConnectionFactory(connectionFactory());
    redisTemplate.setKeySerializer(new StringRedisSerializer());
    redisTemplate.setValueSerializer(new GenericJackson2JsonRedisSerializer());
    redisTemplate.setValueSerializer(new );
    return redisTemplate;
  }
}
```

*Redis-server*의 포트는 기본 값으로 6379를 사용


---

Ref

- [Spring data redis](http://arahansa.github.io/docs_spring/redis.html)
- [자바 직렬화, 그것이 알고싶다. 실무편 - 우아한형제들 기술블로그](http://woowabros.github.io/experience/2017/10/17/java-serialize2.html)
