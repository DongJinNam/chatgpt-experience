# Interview Questions

## show the relational database locking types details about concept, pros and cons compared to each case as korean

answers
데이터베이스 락킹은 데이터베이스 관리 시스템(DBMS)이 데이터 무결성을 보장하고 동시성 문제를 관리하는 방법 중 하나입니다. 데이터베이스 락킹의 주요 유형과 각각의 장단점을 한국어로 설명하겠습니다.

1. **행 레벨 락킹 (Row-Level Locking)**

    **개념**: 행 레벨 락킹은 특정 행에 대한 동시 액세스를 제한하는 데 사용됩니다. 트랜잭션은 행 레벨 락을 사용하여 선택한 행에만 락을 걸 수 있습니다.
   
    **장점**: 
    - 높은 동시성을 제공합니다. 여러 트랜잭션이 다른 행에 대해 동시에 작업할 수 있습니다.
    - 데드락(Deadlock) 발생 가능성이 낮습니다.
   
    **단점**: 
    - 많은 수의 락이 필요할 수 있으며, 이로 인해 락 관리에 더 많은 시스템 리소스가 사용될 수 있습니다.
    - 행이 아닌 범위를 잠그려야 하는 쿼리의 경우 비효율적일 수 있습니다.
   
2. **페이지 레벨 락킹 (Page-Level Locking)**

    **개념**: 페이지 레벨 락킹은 데이터베이스 페이지(일반적으로 여러 행을 포함하는 블록)에 락을 거는 방식입니다.
   
    **장점**: 
    - 행 레벨 락킹보다 더 적은 수의 락이 필요하여 락 관리에 대한 오버헤드가 줄어듭니다.
    - 범위 쿼리에 효율적입니다.
   
    **단점**: 
    - 페이지에 있는 다른 행에 대한 동시 액세스를 차단할 수 있으므로 동시성이 떨어질 수 있습니다.
    - 데드락 발생 가능성이 높아질 수 있습니다.

3. **테이블 레벨 락킹 (Table-Level Locking)**

    **개념**: 테이블 레벨 락킹은 전체 테이블에 락을 거는 방식으로, 트랜잭션이 테이블의 모든 행에 대한 액세스를 차단합니다.
   
    **장점**: 
    - 락 관리에 대한 오버헤드가 적습니다.
    - 단순한 쿼리에서 효과적입니다.
   
    **단점**: 
    - 동시성이 매우 낮습니다. 한 번에 하나의 트랜잭션만 테이블에 액세스 할 수 있습니다.
    - 데드락과 경쟁 조건이 빈번하게 발생할 수 있습니다.

이 세 가지 락킹 유형은 서로 다른 케이스와 요구 사항에 따라 적합합니다. 데이터베이스의 크기, 트랜잭션의 복잡성, 동시성 요구 사항 등을 고려하여 적절한 락킹 전략을 선택하는 것이 중요합니다.

## show me the differences between pessimistic locking and optimistic locking with example java codes compared to each case as korean

낙관적 락킹(optimistic locking)과 비관적 락킹(pessimistic locking)은 데이터베이스의 동시성 제어를 위한 두 가지 다른 접근 방법입니다. 각각의 개념과 장단점, 그리고 Java 코드 예제를 통해 살펴보겠습니다.

### 비관적 락킹 (Pessimistic Locking)

**개념**: 비관적 락킹은 데이터가 변경될 때 충돌이 발생할 것으로 예상하고, 데이터를 처음 읽을 때 락을 걸어 다른 트랜잭션이 해당 데이터를 변경하는 것을 방지하는 방식입니다.

**장점**: 
- 실제로 데이터 충돌이 발생할 가능성이 높은 상황에서 더 안정적입니다.
- 충돌을 피하기 위해 추가적인 로직 없이 락을 사용하여 데이터를 보호합니다.

**단점**: 
- 동시성이 떨어질 수 있으며, 데드락에 취약합니다.
- 리소스가 오랜 시간 동안 잠겨있을 수 있습니다.

**Java 코드 예제**:
```java
// JPA를 사용하는 경우

EntityManager entityManager = // ... (EntityManager 초기화)
EntityTransaction transaction = entityManager.getTransaction();
transaction.begin();

// 비관적 락킹으로 데이터 읽기
Account account = entityManager.find(Account.class, accountId, LockModeType.PESSIMISTIC_WRITE);

// 데이터 변경
account.setBalance(account.getBalance() - amount);

transaction.commit();
```

### 낙관적 락킹 (Optimistic Locking)

**개념**: 낙관적 락킹은 데이터 충돌이 자주 발생하지 않을 것으로 가정하고, 데이터를 변경하는 시점에 충돌이 발생했는지 확인하는 방식입니다. 일반적으로 버전 번호나 타임스탬프를 사용하여 구현됩니다.

**장점**: 
- 동시성이 더 높고, 데드락에 덜 취약합니다.
- 락으로 인한 리소스 대기 시간이 적습니다.

**단점**: 
- 충돌이 발생하면 충돌 해결 로직이 필요합니다.
- 데이터 변경 시 충돌을 감지하기 위한 추가적인 체크가 필요합니다.

**Java 코드 예제**:
```java
@Entity
public class Account {

    @Id
    private Long id;
    private BigDecimal balance;

    @Version
    private int version;

    // getters and setters...
}

// 서비스 레이어에서

@Service
public class AccountService {

    @Autowired
    private AccountRepository accountRepository;

    @Transactional
    public void transferFunds(Long accountId, BigDecimal amount) {
        Account account = accountRepository.findById(accountId).orElseThrow();

        // 데이터 변경
        account.setBalance(account.getBalance().subtract(amount));

        // 트랜잭

션 커밋 시 버전 충돌이 감지되면 예외가 발생합니다.
    }
}
```

비관적 락킹은 데이터의 충돌 가능성이 높고, 데이터 일관성을 유지해야 하는 경우에 적합한 반면, 낙관적 락킹은 동시성이 중요하고 충돌이 자주 발생하지 않는 시나리오에 더 적합합니다. 상황과 요구 사항에 따라 적절한 방법을 선택하는 것이 중요합니다.

## show me the actual cases of deadlock in spring boot application using jpa with mysql and show me how to solve this problem with details including example codes as korean.

Spring Boot 애플리케이션에서 JPA와 MySQL을 사용할 때 데드락(Deadlock)이 발생하는 경우가 있습니다. 데드락은 두 개 이상의 트랜잭션이 서로 다른 자원을 기다리고 있어서 진행이 멈추는 현상입니다.

예를 들어, 두 개의 트랜잭션이 서로 다른 순서로 행을 잠그려고 하면 데드락이 발생할 수 있습니다.

### 예시 코드 (데드락 발생)

**Account.java**
```java
@Entity
public class Account {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private BigDecimal balance;

    // getters and setters...
}
```

**AccountService.java**
```java
@Service
public class AccountService {

    @Autowired
    private AccountRepository accountRepository;

    @Transactional
    public void transfer(Long fromAccountId, Long toAccountId, BigDecimal amount) {
        Account fromAccount = accountRepository.findById(fromAccountId).orElseThrow();
        Account toAccount = accountRepository.findById(toAccountId).orElseThrow();

        fromAccount.setBalance(fromAccount.getBalance().subtract(amount));
        toAccount.setBalance(toAccount.getBalance().add(amount));
    }
}
```

만약 두 개의 서로 다른 스레드가 동시에 서로 반대로 계좌 이체를 시도하는 경우 데드락이 발생할 수 있습니다.

### 데드락 해결 방법

1. **자원에 대한 접근 순서를 일관되게 유지**: 자원에 대한 접근 순서를 일관되게 만들어서 데드락이 발생하지 않도록 할 수 있습니다.

**AccountService.java 수정된 코드**
```java
@Service
public class AccountService {

    @Autowired
    private AccountRepository accountRepository;

    @Transactional
    public void transfer(Long fromAccountId, Long toAccountId, BigDecimal amount) {
        Long minId = Math.min(fromAccountId, toAccountId);
        Long maxId = Math.max(fromAccountId, toAccountId);

        Account firstAccount = accountRepository.findById(minId).orElseThrow();
        Account secondAccount = accountRepository.findById(maxId).orElseThrow();

        if (fromAccountId < toAccountId) {
            firstAccount.setBalance(firstAccount.getBalance().subtract(amount));
            secondAccount.setBalance(secondAccount.getBalance().add(amount));
        } else {
            secondAccount.setBalance(secondAccount.getBalance().add(amount));
            firstAccount.setBalance(firstAccount.getBalance().subtract(amount));
        }
    }
}
```

2. **락 타임아웃 설정**: MySQL에서 락 타임아웃을 설정하여 특정 시간 내에 락을 획득하지 못하면 트랜잭션을 중단하도록 할 수 있습니다. 이렇게하면 데드락 상태가 지속되지 않습니다.

MySQL 설정:
```sql
SET GLOBAL innodb_lock_wait_timeout = 5;
```

위의 방법들은 데드락 문제를 완화하는 데 도움이 됩니다. 

## show me the details of innodb what developer need to know for interview questions as korean

면접 질문에 대비하여 MySQL의 InnoDB 스토리지 엔진에 대해 알아야 할 주요 사항들을 한국어로 설명하겠습니다.

1. **InnoDB 란 무엇인가요?**
   - InnoDB는 MySQL의 대표적인 스토리지 엔진 중 하나로, 트랜잭션을 지원하며, 높은 동시성, 뛰어난 성능, 외래키(Foreign Key) 제약, crash recovery 등의 기능을 제공합니다.

2. **ACID 트랜잭션을 설명해주세요.**
   - InnoDB는 ACID(Atomicity, Consistency, Isolation, Durability) 트랜잭션 모델을 지원합니다.
     - **Atomicity**: 트랜잭션은 완전히 수행되거나 전혀 수행되지 않아야 합니다. 
     - **Consistency**: 트랜잭션은 데이터베이스를 일관된 상태에서 다른 일관된 상태로 변경해야 합니다.
     - **Isolation**: 동시에 실행되는 트랜잭션은 서로에게 영향을 미치지 않아야 합니다.
     - **Durability**: 일단 트랜잭션이 커밋되면, 결과는 영구적으로 데이터베이스에 저장되어야 합니다.

3. **InnoDB의 락킹 메커니즘을 설명해주세요.**
   - InnoDB는 레코드 레벨 락킹과, 자동 데드락 탐지 기능을 제공합니다. 또한 비관적 락킹과 낙관적 락킹을 모두 지원합니다.

4. **MVCC (Multi-Version Concurrency Control) 이란?**
   - InnoDB에서는 MVCC를 통해 동시성을 높이고 락 대기 시간을 줄입니다. MVCC는 각 트랜잭션이 데이터의 특정 버전을 보도록 하여, 여러 트랜잭션을 동시에 안정적으로 처리할 수 있도록 합니다.

5. **인덱싱에 대해 설명해주세요.**
   - InnoDB는 클러스터드 인덱스(Clustered Index)를 사용하여 테이블의 레코드를 저장합니다. 기본 키로 클러스터드 인덱스가 생성되며, 레코드는 이 인덱스를 기준으로 정렬됩니다. 또한 보조 인덱스(Secondary Index)를 통해 검색 성능을 높일 수 있습니다.

6. **버퍼 풀(Buffer Pool)이란?**
   - 버퍼 풀은 InnoDB의 캐시로, 디스크 대신 메모리에 데이터와 인덱스를 저장하여 빠른 액세스를 가능하게 합니다.

7. **Write-Ahead Logging(WAL)이란 무엇인가요?**
   - InnoDB는 변경 사항을 먼저 로그에 기록한 다음 실제 데이터를 디스크에 쓰는 방법을 사용합니다. 이를 통해 성능을 향상시키고, crash recovery를 빠르게 수행할 수 있습니다.

8. **InnoDB 테이블의 특정 공간을 정리하는 방법은 무엇인가요?**
   - `OPTIMIZE TABLE` 명령을 사용하여 InnoDB 테이블의 공간을 정리할 수 있습니다. 이는 테이블을 다시 빌드하여 공간을 회수하고, 인덱스를 정렬합니다.

이상과 같은 주제들은 면접에서 InnoDB에 대해 물어볼 수 있는 핵심적인 내용들입니다. 기본적인 개념을 정확히 이해하고 있으면 유용할 것으로 기대됩니다.

## show me the actual cases of spring boot application problems what a lot of developers need to experience and explain how to solve these issues with details including before and after as korean.

Spring Boot 애플리케이션 개발 시 많은 개발자들이 경험하는 문제들과 해결 방법에 대해 설명드리겠습니다.

### 1. 메모리 부족 (Out of Memory)

#### 문제 상황:
Spring Boot 애플리케이션에서 대량의 데이터를 처리하거나 많은 요청을 처리할 때, 메모리 부족으로 인해 애플리케이션이 종료되는 경우가 있습니다.

#### 해결 방법:
- **JVM 옵션 조정**: 애플리케이션에 할당된 메모리를 증가시킵니다. 예: `-Xmx1024m` 옵션으로 최대 힙 크기를 1024MB로 설정합니다.
- **데이터 처리 최적화**: 대량의 데이터를 처리할 때는 적절한 batch size로 나누어 처리합니다.

### 2. 느린 기동 시간

#### 문제 상황:
Spring Boot 애플리케이션의 기동 시간이 너무 길어서 개발 효율이 떨어지는 경우가 있습니다.

#### 해결 방법:
- **Lazy Initialization**: 애플리케이션의 빈(Bean)들을 필요할 때 로드하도록 설정합니다.
  - `spring.main.lazy-initialization=true` 을 `application.properties`에 설정합니다.

### 3. 순환 의존성 (Circular Dependency)

#### 문제 상황:
두 개 이상의 빈이 서로를 참조하여 순환 의존성이 발생하면, Spring Boot 애플리케이션은 시작되지 않습니다.

#### 해결 방법:
- **의존성 분리**: 순환 의존성을 제거하기 위해 클래스간 의존성을 재구성합니다. 
- **@Lazy 어노테이션 사용**: 필요한 빈을 늦춰서 로드하도록 설정하여 순환 의존성 문제를 해결할 수 있습니다.

### 4. 데이터베이스 연결 문제

#### 문제 상황:
데이터베이스에 연결할 수 없거나, 연결이 끊어지는 문제가 발생합니다.

#### 해결 방법:
- **연결 정보 확인**: `application.properties` 파일에서 데이터베이스 연결 정보를 확인합니다.
- **연결 풀 설정**: HikariCP와 같은 연결 풀을 사용하여 데이터베이스 연결을 효율적으로 관리합니다.

### 5. CORS (Cross-Origin Resource Sharing) 문제

#### 문제 상황:
다른 도메인에서 호스팅되는 웹 페이지가 Spring Boot 애플리케이션의 API를 호출할 때 CORS 문제로 인해 요청이 거부됩니다.

#### 해결 방법:
- **CORS 설정 추가**: Spring

 Boot에서 제공하는 CORS 설정을 사용하여 허용된 출처를 지정합니다.

#### 예시 코드 (Before):
```java
@RestController
public class MyController {
    @GetMapping("/api/data")
    public Data getData() {
        return new Data();
    }
}
```

#### 예시 코드 (After):
```java
@RestController
public class MyController {

    @CrossOrigin(origins = "https://allowed-origin.com")
    @GetMapping("/api/data")
    public Data getData() {
        return new Data();
    }
}
```

이상과 같이 Spring Boot 애플리케이션 개발 시 자주 발생하는 문제들을 인지하고 적절한 해결 방법을 적용함으로써, 애플리케이션의 안정성과 개발 효율을 높일 수 있습니다.

## show me the differences of bean injection on spring boot between eager loading and lazy loading and explain what is good bean injection depends on??

In Spring Boot, beans can be loaded in two ways: Eager Loading and Lazy Loading. Let me explain the differences and when to use each.

### Eager Loading:

#### What it is:
By default, Spring Boot loads all the beans at the startup of the application. This means that whether or not you use a specific bean, it is created and injected as soon as the application context is loaded.

#### Pros:
- Since all beans are ready to use at startup, it ensures faster response times for the first request that needs them.
- You will know at the startup if there are any issues in the bean configuration or bean creation.

#### Cons:
- Increases the startup time of the application.
- Consumes more memory from the beginning even for those beans which might not be used.

### Lazy Loading:

#### What it is:
In contrast, with lazy loading, the bean is created and initialized only when it is first requested. You can mark a bean as lazy by using `@Lazy` annotation.

#### Pros:
- Faster application startup time because not all beans are created at once.
- Consumes less memory at startup as beans are only created when needed.

#### Cons:
- The first request to access the bean might be slower because the bean needs to be created.
- Any configuration issues related to that bean won’t be known until it is first accessed.

### Choosing the Right Approach:

- **For Development Environment**: Using lazy loading can be beneficial during development as it reduces the startup time and you might not need all the beans loaded for development/testing purposes.
- **For Critical Beans**: For beans that are essential for application functioning and are sure to be used, eager loading is a good choice.
- **For Optional or Rarely Used Beans**: If a bean is not critical and might not be used frequently, lazy loading would be a better choice to save resources.
- **Memory Considerations**: If your application is memory-sensitive, then lazy loading could help in utilizing memory more efficiently.

Example of Lazy Loading:
```java
@Configuration
@Lazy
public class MyConfiguration {
    //...
}
```

In summary, the decision between eager and lazy loading should be based on the specific use case, how critical the bean is, the application's startup time requirements, and memory considerations.


