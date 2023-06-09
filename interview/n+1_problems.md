# N+1 Problems

N+1 문제는 데이터베이스 쿼리 최적화와 관련된 일반적인 문제입니다. 이 문제를 이해하고 다양한 방법으로 해결하는 방법을 알아봅시다.

### N+1 문제란?

ORM(Object-Relational Mapping)을 사용하여 데이터를 가져올 때 발생하는 문제로, 하나의 쿼리로 데이터를 가져온 후, 각 데이터에 대해 추가적인 쿼리를 수행하는 패턴입니다. 예를 들어, 게시글 목록을 가져온 후 (1번의 쿼리), 각 게시글에 대해 댓글을 가져오는 쿼리를 수행하는 경우 (N번의 쿼리) N+1 문제가 발생합니다.

### 해결 방법

#### 1. JOIN FETCH 사용

JOIN FETCH를 사용하여 연관된 엔티티를 한 번의 쿼리로 가져올 수 있습니다.

**장점**:
- 쿼리의 수가 크게 줄어듭니다.
- 간단한 경우에 쉽게 적용할 수 있습니다.

**단점**:
- 데이터 중복이 발생할 수 있습니다.
- 복잡한 쿼리의 경우 성능이 저하될 수 있습니다.

#### 예시:
```java
@EntityGraph(attributePaths = {"comments"})
List<Post> findAll();
```

#### 2. Batch Size 설정

Batch Size를 설정하여 한 번에 가져올 연관된 엔티티의 개수를 제한합니다.

**장점**:
- 불필요한 데이터 중복을 줄일 수 있습니다.
- 대량의 데이터 처리에 적합합니다.

**단점**:
- 적절한 batch size를 설정하는 것이 중요합니다. 너무 크거나 작을 경우 성능에 영향을 미칠 수 있습니다.

#### 예시:
```java
@BatchSize(size = 5)
private List<Comment> comments;
```

#### 3. DTO(데이터 전송 객체) 사용

원하는 필드만 선택하여 DTO로 직접 조회합니다.

**장점**:
- 불필요한 데이터를 가져오지 않아 성능을 최적화할 수 있습니다.

**단점**:
- 별도의 DTO 클래스가 필요합니다.

#### 예시:
```java
@Query("select new com.example.PostDto(p.id, p.title, p.content, c.content) from Post p join p.comments c")
List<PostDto> findAllDtos();
```

### 결론

N+1 문제는 데이터베이스 성능에 큰 영향을 미치므로 주의하여야 합니다. 위의 방법들 중 상황에 맞는 최적의 방법을 선택하여 적용하는 것이 중요합니다. JOIN FETCH는 간단한 경우에 적합하고, Batch Size는 대량의 데이터를 처리할 때, DTO 사용은 특정 필드만 필요한 경우에 적합합니다.
