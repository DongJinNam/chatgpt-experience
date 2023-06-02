# inversion-of-control

### IoC 란 무엇인가 그리고 예제 코드 설명

Inversion of Control (IoC) is a fundamental principle in software development and is a core concept in the Spring framework. It refers to the shift of control over the creation and management of objects from the application code to an external entity, often referred to as the IoC container or the framework itself.

In the context of the Spring framework, IoC is implemented through the Dependency Injection (DI) mechanism. DI allows objects to be loosely coupled by providing their dependencies externally, rather than the objects themselves creating or managing their dependencies. The Spring IoC container takes control of creating objects, wiring their dependencies, and managing their lifecycle.

Here's an example to illustrate the usage of IoC and DI in Spring:

Consider a simple application that has a `UserService` and a `UserRepository`. The `UserService` depends on the `UserRepository` to perform operations related to user management.

First, let's define the `UserRepository` interface:

```java
public interface UserRepository {
    void save(User user);
}
```

Next, let's create a concrete implementation of the `UserRepository`:

```java
public class UserRepositoryImpl implements UserRepository {
    public void save(User user) {
        // Implementation for saving the user
    }
}
```

Now, let's define the `UserService` that depends on the `UserRepository`:

```java
public class UserService {
    private UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public void createUser(User user) {
        // Business logic for creating a user
        userRepository.save(user);
    }
}
```

In the above example, the `UserService` relies on the `UserRepository` for saving user data. Instead of instantiating the `UserRepository` itself, the `UserService` constructor takes a `UserRepository` parameter and assigns it to the `userRepository` field. This is the constructor injection approach.

Now, let's see how Spring's IoC container manages the dependencies and wires everything together:

```java
@Configuration
public class AppConfig {
    @Bean
    public UserRepository userRepository() {
        return new UserRepositoryImpl();
    }

    @Bean
    public UserService userService(UserRepository userRepository) {
        return new UserService(userRepository);
    }
}
```

In the above code, the `AppConfig` class is annotated with `@Configuration`, indicating that it contains Spring bean configurations. The `userRepository()` method is annotated with `@Bean`, which tells Spring that it should manage the lifecycle of the returned object as a bean. Similarly, the `userService()` method is also annotated with `@Bean` and receives the `UserRepository` bean as a parameter, fulfilling the dependency of the `UserService` class.

With this configuration, the Spring IoC container will create and manage instances of `UserRepository` and `UserService`. When the `UserService` instance is created, the container automatically resolves the dependency and injects the appropriate `UserRepository` instance.

By leveraging IoC and DI, the application code becomes more modular, testable, and maintainable. The dependencies are explicitly declared, making it easier to understand and manage the relationships between objects. It also allows for better flexibility and the ability to swap different implementations of dependencies without modifying the client code.

In summary, the IoC principle in Spring promotes loose coupling and decoupling of dependencies by allowing the framework to manage the instantiation, wiring, and lifecycle of objects. DI, as an implementation of IoC, helps achieve this by injecting dependencies into objects, reducing their responsibility for managing dependencies and increasing flexibility and modularity in the codebase.
