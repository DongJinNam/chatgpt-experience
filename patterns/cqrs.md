# CQRS (Command and Query Responsibilkity Segregation)

### cqrs 패턴 구현(command database - mysql, query database - elasticsearch, event sender - kafka)

To implement the CQRS pattern with Event-Driven Development (EDD), MySQL as the command database, Elasticsearch as the query database, and Kafka as the event sender in a Spring Boot application, you can follow these steps:

1. Set up the Dependencies:
Add the required dependencies to your `pom.xml` file:

```xml
<!-- MySQL Connector -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>

<!-- Elasticsearch -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-elasticsearch</artifactId>
</dependency>

<!-- Apache Kafka -->
<dependency>
    <groupId>org.springframework.kafka</groupId>
    <artifactId>spring-kafka</artifactId>
</dependency>
```

2. Configure MySQL Database:
Configure the MySQL database for handling commands. Update your `application.properties` file with the necessary database configurations:

```properties
# MySQL Database Configuration
spring.datasource.url=jdbc:mysql://localhost:3306/cqrs_commands
spring.datasource.username=root
spring.datasource.password=your_password
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.jpa.hibernate.ddl-auto=create
```

3. Configure Elasticsearch:
Configure Elasticsearch for handling queries. Update your `application.properties` file with the necessary Elasticsearch configurations:

```properties
# Elasticsearch Configuration
spring.data.elasticsearch.cluster-nodes=localhost:9200
spring.data.elasticsearch.cluster-name=your_cluster_name
spring.data.elasticsearch.repositories.enabled=true
```

4. Configure Kafka:
Configure Kafka as the event sender. Update your `application.properties` file with the necessary Kafka configurations:

```properties
# Kafka Configuration
spring.kafka.bootstrap-servers=localhost:9092
```

5. Create Command Model and Repository:
Create the command model representing the data to be stored in the MySQL database and the corresponding repository.

```java
@Entity
@Table(name = "users")
public class UserCommand {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String username;
    private String email;
    // other properties, getters, and setters
}

@Repository
public interface UserCommandRepository extends JpaRepository<UserCommand, Long> {
    // Additional methods if needed
}
```

6. Create Query Model and Repository:
Create the query model representing the data to be stored in Elasticsearch and the corresponding repository.

```java
@Document(indexName = "users")
public class UserQuery {
    @Id
    private String id;
    
    private String username;
    private String email;
    // other properties, getters, and setters
}

@Repository
public interface UserQueryRepository extends ElasticsearchRepository<UserQuery, String> {
    // Additional methods if needed
}
```

7. Create Command and Query Services:
Create services to handle the commands and queries. These services will interact with the respective repositories.

```java
@Service
@Transactional
public class UserCommandService {
    private final UserCommandRepository commandRepository;
    private final KafkaTemplate<String, UserCreatedEvent> kafkaTemplate;
    
    public UserCommandService(UserCommandRepository commandRepository, KafkaTemplate<String, UserCreatedEvent> kafkaTemplate) {
        this.commandRepository = commandRepository;
        this.kafkaTemplate = kafkaTemplate;
    }
    
    public void createUser(CreateUserCommand command) {
        UserCommand userCommand = new UserCommand();
        userCommand.setUsername(command.getUsername());
        userCommand.setEmail(command.getEmail());
        // set other properties
        
        commandRepository.save(userCommand);
        
        UserCreatedEvent event = new UserCreatedEvent(userCommand.getId(), userCommand.getUsername(), userCommand.getEmail());
        kafkaTemplate.send("user-created-topic", event);
    }
    
    // Other command methods
}

@Service
@Transactional(readOnly = true)


public class UserQueryService {
    private final UserQueryRepository queryRepository;
    
    public UserQueryService(UserQueryRepository queryRepository) {
        this.queryRepository = queryRepository;
    }
    
    public User getUser(String userId) {
        return queryRepository.findById(userId)
            .orElseThrow(() -> new IllegalArgumentException("User not found"));
    }
    
    // Other query methods
}
```

8. Create Event Listeners:
Create event listeners to handle events and update the Elasticsearch database accordingly.

```java
@Component
public class UserCreatedEventListener {
    private final UserQueryRepository queryRepository;
    
    public UserCreatedEventListener(UserQueryRepository queryRepository) {
        this.queryRepository = queryRepository;
    }
    
    @KafkaListener(topics = "user-created-topic")
    public void handleUserCreatedEvent(UserCreatedEvent event) {
        UserQuery userQuery = new UserQuery();
        userQuery.setId(event.getUserId());
        userQuery.setUsername(event.getUsername());
        userQuery.setEmail(event.getEmail());
        // set other properties
        
        queryRepository.save(userQuery);
    }
}
```

9. Create Controllers:
Create controllers to handle the REST endpoints for commands and queries.

```java
@RestController
@RequestMapping("/users")
public class UserController {
    private final UserCommandService commandService;
    private final UserQueryService queryService;
    
    public UserController(UserCommandService commandService, UserQueryService queryService) {
        this.commandService = commandService;
        this.queryService = queryService;
    }
    
    @PostMapping
    public ResponseEntity<String> createUser(@RequestBody CreateUserCommand command) {
        commandService.createUser(command);
        
        return ResponseEntity.ok("User created successfully");
    }
    
    @GetMapping("/{userId}")
    public ResponseEntity<User> getUser(@PathVariable String userId) {
        User user = queryService.getUser(userId);
        
        return ResponseEntity.ok(user);
    }
}
```

In this example, the CQRS pattern is implemented using separate databases (MySQL for commands and Elasticsearch for queries). Command operations are handled by the `UserCommandService` and stored in the MySQL database. Query operations are handled by the `UserQueryService` and retrieved from the Elasticsearch database. The `UserCreatedEventListener` listens to events produced by the `UserCommandService` and updates the Elasticsearch database accordingly. Kafka is used as the event sender to publish and consume events.

Note: The example provided is a simplified version for illustration purposes. In a real-world application, you would likely have more complex business logic, error handling, and event-driven messaging mechanisms in place.
