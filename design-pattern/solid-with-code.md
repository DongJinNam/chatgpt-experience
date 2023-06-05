# SOLID pattern

### Pattern with java codes before and after

Sure! The SOLID principles are a set of guidelines for writing clean and maintainable code. They are widely used in object-oriented programming and can help improve code readability, reusability, and testability. Let's go through each principle with Java code examples, including examples of code that do not follow the principles.

1. Single Responsibility Principle (SRP):
   This principle states that a class should have only one reason to change.

   Example - Following SRP:
   ```java
   // Good example
   class Order {
       public void calculateTotal() {
           // Calculate order total
       }
   
       public void printInvoice() {
           // Print invoice
       }
   }
   ```

   Example - Not following SRP:
   ```java
   // Bad example
   class Order {
       public void calculateTotal() {
           // Calculate order total
       }
   
       public void printInvoice() {
           // Print invoice
       }
   
       public void saveToDatabase() {
           // Save order to the database
       }
   }
   ```

2. Open/Closed Principle (OCP):
   This principle states that classes should be open for extension but closed for modification.

   Example - Following OCP:
   ```java
   // Good example
   interface Shape {
       double calculateArea();
   }
   
   class Rectangle implements Shape {
       private double length;
       private double width;
   
       public Rectangle(double length, double width) {
           this.length = length;
           this.width = width;
       }
   
       public double calculateArea() {
           return length * width;
       }
   }
   
   class Circle implements Shape {
       private double radius;
   
       public Circle(double radius) {
           this.radius = radius;
       }
   
       public double calculateArea() {
           return Math.PI * radius * radius;
       }
   }
   ```

   Example - Not following OCP:
   ```java
   // Bad example
   class Shape {
       private String type;
   
       public Shape(String type) {
           this.type = type;
       }
   
       public double calculateArea() {
           if (type.equals("Rectangle")) {
               // Calculate rectangle area
           } else if (type.equals("Circle")) {
               // Calculate circle area
           }
           // ...
       }
   }
   ```

3. Liskov Substitution Principle (LSP):
   This principle states that objects of a superclass should be replaceable with objects of its subclasses without affecting the correctness of the program.

   Example - Following LSP:
   ```java
   // Good example
   class Shape {
       protected double area;
   
       public double getArea() {
           return area;
       }
   }
   
   class Rectangle extends Shape {
       private double length;
       private double width;
   
       public Rectangle(double length, double width) {
           this.length = length;
           this.width = width;
           this.area = length * width;
       }
   }
   
   class Square extends Shape {
       private double side;
   
       public Square(double side) {
           this.side = side;
           this.area = side * side;
       }
   }
   ```

   Example - Not following LSP:
   ```java
   // Bad example
   class Rectangle {
       protected double length;
       protected double width;
   
       public void setLength(double length) {
           this.length = length;
       }
   
       public void setWidth(double width) {
           this.width = width;
       }
   
       public double calculateArea() {
           return length * width;
       }
   }
   
   class Square extends Rectangle {
       public void setLength(double length) {
           this.length = length;
           this.width = length;
       }
   
       public

 void setWidth(double width) {
           this.length = width;
           this.width = width;
       }
   }
   ```

4. Interface Segregation Principle (ISP):
   This principle states that clients should not be forced to depend on interfaces they do not use.

   Example - Following ISP:
   ```java
   // Good example
   interface Printable {
       void print();
   }
   
   interface Faxable {
       void fax();
   }
   
   class Printer implements Printable {
       public void print() {
           // Print the document
       }
   }
   
   class FaxMachine implements Faxable {
       public void fax() {
           // Fax the document
       }
   }
   ```

   Example - Not following ISP:
   ```java
   // Bad example
   interface Machine {
       void print();
       void fax();
       void scan();
   }
   
   class AllInOneMachine implements Machine {
       public void print() {
           // Print the document
       }
   
       public void fax() {
           // Fax the document
       }
   
       public void scan() {
           // Scan the document
       }
   }
   ```

5. Dependency Inversion Principle (DIP):
   This principle states that high-level modules should not depend on low-level modules. Both should depend on abstractions.

   Example - Following DIP:
   ```java
   // Good example
   interface MessageSender {
       void sendMessage(String message);
   }
   
   class EmailSender implements MessageSender {
       public void sendMessage(String message) {
           // Send message via email
       }
   }
   
   class SMSsender implements MessageSender {
       public void sendMessage(String message) {
           // Send message via SMS
       }
   }
   
   class NotificationService {
       private MessageSender sender;
   
       public NotificationService(MessageSender sender) {
           this.sender = sender;
       }
   
       public void sendNotification(String message) {
           sender.sendMessage(message);
       }
   }
   ```

   Example - Not following DIP:
   ```java
   // Bad example
   class EmailSender {
       public void sendEmail(String email, String message) {
           // Send email
       }
   }
   
   class NotificationService {
       private EmailSender sender;
   
       public NotificationService(EmailSender sender) {
           this.sender = sender;
       }
   
       public void sendNotification(String email, String message) {
           sender.sendEmail(email, message);
       }
   }
   ```

These examples demonstrate the SOLID principles and provide comparisons between code that follows and code that violates the principles. Remember, following these principles can lead to more maintainable and flexible code in the long run.
