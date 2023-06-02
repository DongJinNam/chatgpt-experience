# SOLID 패턴

### SOLID 패턴 설명 및 각각 예제 코드

The SOLID principles are a set of design principles that promote clean, maintainable, and scalable software architecture. The acronym SOLID stands for:

1. Single Responsibility Principle (SRP): A class should have only one reason to change. It states that a class should have a single responsibility or purpose and should not be responsible for multiple unrelated tasks.

Example code adhering to SRP:

```java
public class UserService {
    public void createUser(User user) {
        // Business logic for creating a user
    }
}

public class UserRepository {
    public void save(User user) {
        // Database interaction code to save the user
    }
}
```

In the above code, the `UserService` class is responsible for handling user-related business logic, such as creating a user. The `UserRepository` class is responsible for persisting the user in the database. Each class has a single responsibility, adhering to the SRP.

2. Open-Closed Principle (OCP): Software entities (classes, modules, functions, etc.) should be open for extension but closed for modification. It suggests that the behavior of a software component should be easily extended without modifying its existing code.

Example code adhering to OCP:

```java
public interface PaymentProvider {
    void pay(double amount);
}

public class CreditCardPaymentProvider implements PaymentProvider {
    public void pay(double amount) {
        // Logic for making a payment using a credit card
    }
}

public class PayPalPaymentProvider implements PaymentProvider {
    public void pay(double amount) {
        // Logic for making a payment using PayPal
    }
}
```

In the above code, the `PaymentProvider` interface defines a contract for payment providers. The `CreditCardPaymentProvider` and `PayPalPaymentProvider` classes implement this interface and provide specific implementations for making payments. If a new payment provider needs to be added, a new class can be created that implements the `PaymentProvider` interface without modifying the existing code.

3. Liskov Substitution Principle (LSP): Subtypes should be substitutable for their base types. It states that objects of a superclass should be able to be replaced with objects of its subclasses without affecting the correctness of the program.

Example code adhering to LSP:

```java
public abstract class Shape {
    public abstract double area();
}

public class Rectangle extends Shape {
    private double width;
    private double height;

    public double area() {
        return width * height;
    }
}

public class Square extends Shape {
    private double side;

    public double area() {
        return side * side;
    }
}
```

In the above code, both the `Rectangle` and `Square` classes inherit from the `Shape` class. The `area()` method is overridden in each subclass to calculate the area based on the specific shape. Since a `Square` is a specific type of `Rectangle`, it can be used interchangeably with a `Rectangle` wherever a `Shape` is expected, adhering to the LSP.

4. Interface Segregation Principle (ISP): Clients should not be forced to depend on interfaces they do not use. It suggests that interfaces should be fine-grained and segregated to avoid imposing unnecessary dependencies on clients.

Example code adhering to ISP:

```java
public interface Printer {
    void print();
}

public interface Scanner {
    void scan();
}

public class AllInOnePrinter implements Printer, Scanner {
    public void print() {
        // Logic for printing
    }

    public void scan() {
        // Logic for scanning
    }
}
```

In the above code, the `Printer` interface defines a contract for printers, and the `Scanner` interface defines a contract for scanners. The `AllInOnePrinter` class implements both interfaces, providing an

 implementation for printing and scanning. This segregation allows clients to depend only on the interfaces they require, preventing unnecessary dependencies.

5. Dependency Inversion Principle (DIP): High-level modules should not depend on low-level modules. Both should depend on abstractions. It suggests that the high-level modules should depend on abstractions (interfaces or abstract classes) rather than concrete implementations.

Example code adhering to DIP:

```java
public interface MessageSender {
    void sendMessage(String message);
}

public class EmailSender implements MessageSender {
    public void sendMessage(String message) {
        // Logic for sending an email
    }
}

public class SMSSender implements MessageSender {
    public void sendMessage(String message) {
        // Logic for sending an SMS
    }
}

public class NotificationService {
    private MessageSender messageSender;

    public NotificationService(MessageSender messageSender) {
        this.messageSender = messageSender;
    }

    public void sendNotification(String message) {
        messageSender.sendMessage(message);
    }
}
```

In the above code, the `NotificationService` class depends on the `MessageSender` interface. It can be injected with any implementation of the `MessageSender` interface, such as `EmailSender` or `SMSSender`. This promotes loose coupling and allows for easier testing and swapping of different message senders.

By following the SOLID principles, developers can create software that is modular, flexible, and easier to maintain. These principles promote code reuse, separation of concerns, and improved scalability, resulting in more robust and maintainable applications.
