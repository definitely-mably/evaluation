# AGENTS.md

## Project Overview

Spring Boot 2.7.12 microservice using Java 17 with Spring Cloud and Spring Cloud Alibaba.

## Build Commands

```bash
# Build entire project
mvn clean install

# Build without tests
mvn clean install -DskipTests

# Run application
mvn spring-boot:run

# Run all tests
mvn test

# Run single test class
mvn test -Dtest=ClassName

# Run single test method
mvn test -Dtest=ClassName#methodName

# Run tests in package
mvn test -Dtest=package.path.*

# Package without tests
mvn clean package -DskipTests

# Verify build (compile, test, package)
mvn verify

# Clean build artifacts
mvn clean
```

## Technology Stack

- Java 17
- Spring Boot 2.7.12
- Spring Cloud 2021.0.3
- Spring Cloud Alibaba 2021.0.4.0
- MyBatis Plus 3.5.7
- Hutool 5.8.11
- Sa-Token 1.44.0 (authentication/authorization with OAuth2 support)
- MySQL 8.0.23
- Redis (session storage)
- Elasticsearch 7.12.1
- Lombok 1.18.20

## Code Style Guidelines

### Package Structure

- Base package: `org.evaluation.[module]`
- Controllers: `org.evaluation.[module].controller`
- Services: `org.evaluation.[module].service` and `org.evaluation.[module].service.impl`
- Entities/Models: `org.evaluation.[module].entity` or `domain`
- Mappers: `org.evaluation.[module].mapper`
- DTOs: `org.evaluation.[module].dto`
- Exceptions: `org.evaluation.[module].exception`
- Utils: `org.evaluation.[module].util`

### Naming Conventions

- Classes: PascalCase (e.g., `UserService`, `OrderController`)
- Methods: camelCase (e.g., `getUserById`, `saveOrder`)
- Constants: UPPER_SNAKE_CASE (e.g., `MAX_RETRY_COUNT`)
- Variables: camelCase (e.g., `userId`, `orderList`)
- Database tables: snake_case (e.g., `user_account`, `order_detail`)
- API endpoints: kebab-case (e.g., `/api/users/{id}`)

### Imports

- Import statements grouped: Java stdlib, third-party, Spring, project-specific
- Use wildcard imports sparingly (avoid `*` except for static imports)
- No unused imports allowed

### Annotations

- Use Lombok annotations: `@Data`, `@Getter`, `@Setter`, `@Builder`, `@NoArgsConstructor`, `@AllArgsConstructor`
- Spring annotations:
  - `@RestController` for API controllers
  - `@Service` for service layer
  - `@Repository` or `@Mapper` for data access
  - `@Component` for generic components
  - `@Autowired` for dependency injection (constructor injection preferred)
- Sa-Token: `StpUtil.login()`, `StpUtil.logout()`, `StpUtil.getLoginIdAsInt()`
- Use `@FeignClient` for service-to-service communication

### Error Handling

- Create custom exceptions in `exception` package (e.g., `BusinessException`)
- Use `@RestControllerAdvice` and `@ExceptionHandler` for global exception handling
- Return `Result<T>` format with error code and message for consistency
- Log errors appropriately using SLF4J logger

### Logging

- Use SLF4J with Lombok's `@Slf4j` annotation
- Log levels: ERROR for exceptions, WARN for recoverable issues, INFO for important events, DEBUG for debugging
- Use parameterized logging: `log.info("User {} logged in", userId)`
- Avoid string concatenation in log statements

### API Design

- RESTful principles for API endpoints
- Use appropriate HTTP methods (GET, POST, PUT, DELETE, PATCH)
- Return `Result<T>` response format for consistency
- Use DTOs for request/response, avoid exposing entities directly
- Validate input with `@Valid` and Bean Validation annotations

### Database/MyBatis Plus

- Use MyBatis Plus annotations: `@TableName`, `@TableId`, `@TableField`
- Mapper interfaces extend `BaseMapper<T>`
- Use service layer for business logic, not in controllers

### Type Safety

- Use generic types appropriately (e.g., `List<User>`, `Map<String, Object>`)
- Avoid raw types
- Use Optional for nullable return values from services
- Validate input parameters, especially in public methods

### Code Organization

- Keep classes focused and single-responsibility
- Prefer composition over inheritance
- Extract common functionality to utility classes or base classes
- Keep method length reasonable (<50 lines preferred)

### Formatting

- Use 4-space indentation (Java standard)
- Maximum line length: 120 characters
- Opening braces on same line for classes, methods
- Blank line between methods
- One statement per line

### Testing

- Use JUnit 5 (Jupiter) and Spring Boot Test
- Test classes: `ClassNameTest.java`
- Use `@SpringBootTest` for integration tests
- Use `@MockBean` for mocking Spring beans
- Test naming: `should[ExpectedBehavior]When[StateUnderTest]`
- Arrange-Act-Assert (AAA) pattern in test methods
- Keep tests independent and repeatable
