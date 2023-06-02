# Easy to Mistake

### Spring Webflux 를 사용하면서 흔히 하는 실수, 관련 예제 코드

When using Spring WebFlux, there are a few common mistakes that developers may make. Here are some of them along with the corresponding Java code examples:

1. Blocking Operations:
One common mistake is inadvertently introducing blocking operations within a reactive chain, which can undermine the non-blocking nature of WebFlux. For example:

```java
@GetMapping("/users")
public Flux<User> getAllUsers() {
    List<User> userList = userRepository.findAll(); // Blocking operation
    return Flux.fromIterable(userList); // Conversion to Flux
}
```

In the above code, the `userRepository.findAll()` method returns a `List<User>` synchronously, blocking the execution. To rectify this mistake, you should use the non-blocking reactive repository methods directly:

```java
@GetMapping("/users")
public Flux<User> getAllUsers() {
    return userRepository.findAll(); // Non-blocking operation
}
```

2. Missing Error Handling:
Another common mistake is not handling errors properly when working with reactive types. It's important to handle errors explicitly to ensure proper error propagation and handling. For example:

```java
@GetMapping("/user/{id}")
public Mono<User> getUserById(@PathVariable("id") String id) {
    return userRepository.findById(id)
            .switchIfEmpty(Mono.error(new NotFoundException("User not found"))); // Error handling
}
```

In the above code, the `switchIfEmpty` operator is used to handle the case when no user is found. It returns an error signal (`Mono.error`) with a custom exception (`NotFoundException`).

3. Blocking Operations in WebClient:
When using `WebClient` for making HTTP calls, it's important to avoid accidentally introducing blocking operations. WebClient provides non-blocking methods like `exchange()` or `retrieve()` to perform asynchronous HTTP requests. Here's an example of a common mistake:

```java
public Mono<User> getUser() {
    return WebClient.create()
            .get()
            .uri("https://api.example.com/users")
            .retrieve()
            .bodyToMono(User.class); // Blocking operation
}
```

To avoid this mistake, you should use the non-blocking alternatives like `exchange()` and handle the response asynchronously:

```java
public Mono<User> getUser() {
    return WebClient.create()
            .get()
            .uri("https://api.example.com/users")
            .exchangeToMono(response -> response.bodyToMono(User.class)); // Non-blocking operation
}
```

By using `exchangeToMono`, you ensure non-blocking behavior and proper handling of the response.

These are just a few examples of mistakes that can be made when working with Spring WebFlux. It's important to understand the reactive nature of WebFlux, avoid blocking operations, handle errors appropriately, and use non-blocking alternatives throughout your reactive codebase to fully leverage the benefits of reactive programming.
