# Clean Code

클린코드 요약 내용 및 개발자가 반드시 알아야될 내용입니다.

**Summary of "Clean Code" by Robert C. Martin:**

"Clean Code" advocates for software craftsmanship by emphasizing the importance of writing code that is easy to understand, read, and maintain. It encourages developers to view code as a form of communication, and highlights principles, patterns, and practices for writing clean, efficient, and maintainable code that not only works well but also communicates its intention clearly to other developers.

**Things a Software Developer Needs to Know about Writing Clean Code with Java:**

1. **Naming Conventions**: Choose meaningful and pronounceable names for variables, functions, classes, and other identifiers. Use CamelCase for naming methods, variables, and classes. 

    Bad:
    ```java
    int d; // elapsed time in days
    ```

    Good:
    ```java
    int elapsedTimeInDays;
    ```

2. **Functions Should Do One Thing**: Functions should do exactly one thing. They should do it well, and do it only. Single Responsibility Principle (SRP) is a key principle here.

    Bad:
    ```java
    public void emailClients(List<Client> clients) {
        for (Client client : clients) {
            String clientRecord = client.getName() + "," + client.getEmail();
            createEmail(clientRecord);
            sendEmail(clientRecord);
        }
    }
    ```

    Good:
    ```java
    public void emailClients(List<Client> clients) {
        for (Client client : clients) {
            String clientRecord = createClientRecord(client);
            sendEmail(clientRecord);
        }
    }

    private String createClientRecord(Client client){
        return client.getName() + "," + client.getEmail();
    }

    private void sendEmail(String clientRecord){
        // logic to send email
    }
    ```

3. **Commenting**: Comments should be used sparingly. Your code should be the primary source of documentation. Good code mostly documents itself. When necessary, comments should explain why certain code exists rather than what it's doing.

4. **Avoid Deep Nesting**: Too many levels of indentation can make code harder to read and follow. Try to avoid deep nesting by returning early or breaking the code into smaller functions.

5. **Error Handling is Important**: Don't ignore caught errors, handle them properly. This will make your application more robust and easier to debug.

6. **Keep All Entities Small**: This applies to functions, classes, and modules. Smaller entities are easier to understand, test, and debug.

7. **Don’t Repeat Yourself (DRY)**: Repeated code should be abstracted into functions or classes to promote reusability and maintainability.

8. **Use Descriptive Variable Names**: Variables with clear, understandable names make your code much easier to read.

9. **Consistent Formatting**: Code formatting should be consistent across a codebase. This includes indentation, spacing, and bracket placement.

10. **Unit Testing**: The code should be thoroughly tested with unit tests. Each test should examine one tiny bit of functionality.

Remember, it's not about writing code that works only, but code that is clear, simple, and easy to understand and maintain by others.
