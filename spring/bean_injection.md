# Bean 주입 방식

## Eager loading vs Lazy loading

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
