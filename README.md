

# RocketFoodDelivery - REST API Documentation

## Overview

**RocketFoodDelivery** is a Spring Boot-based RESTful API for managing a food delivery platform. The system covers a wide domain: restaurants, products, orders, users, employees, and couriers. It supports authentication, resource creation, updates, queries, and filtering, all implemented with a layered architecture and robust validation and error handling.

**Key functionalities:**

- **User Authentication & JWT**: Secure login and token-based authentication.
- **Resource Management**: CRUD operations on restaurants, products, orders, customers, employees, and couriers.
- **Data Validation & Error Handling**: Ensures integrity and consistent error responses.
- **Filtering & Query**: Retrieve restaurants by rating and price range; fetch orders by customer, restaurant, or courier.
- **Testing & Quality Assurance**: Integration and unit tests ensure API correctness and reliability.

---

## Key Technologies and Components

- **Spring Boot & Spring MVC**: Core framework for building RESTful endpoints.
- **Spring Data JPA & Hibernate**: Database interactions and ORM.
- **Spring Security & JWT**: Handles authentication and authorization with JWT tokens.
- **MySQL**: Persistent data storage, configured via `application.properties`.
- **Lombok**: Reduces boilerplate code.
- **Jakarta Validation**: Ensures request payloads are properly formatted and validated.
- **JUnit & Mockito**: Testing frameworks for integration and unit tests.
- **Maven & Maven Wrapper (`mvnw`, `mvnw.cmd`)**: Standardized builds and dependency management.

---

## Project Structure

**Main Directories:**

- `src/main/java/com/rocketFoodDelivery/rocketFood/`
    - **controller/**: REST API controllers (handle HTTP requests)
    - **dtos/**: Data Transfer Objects for requests and responses
    - **exception/**: Custom exception classes
    - **models/**: Domain entities representing database tables
    - **repository/**: Interfaces for database operations
    - **security/**: Security and JWT configuration classes
    - **service/**: Business logic and interaction with repositories
    - **util/**: Utility classes (e.g., `ResponseBuilder` for consistent responses)

- `src/main/resources/`
    - **application.properties**: Main application configuration (DB connection, logging, etc.)
    - **templates/**: HTML templates (if needed for front-end integration)

- `src/test/java/com/rocketFoodDelivery/rocketFood/`
    - **api/**: Integration tests for API controllers and endpoints.

- `src/test/resources/`
    - **application-test.properties**: Test environment configurations

- `target/`
    - Compiled classes, build artifacts.

---

## Layers and Components

### Controllers (API Layer)

Handle incoming requests, map them to service methods, and produce responses. Found in `com.rocketFoodDelivery.rocketFood.controller.api`:

- **AuthController** (`/api/auth`): Authentication endpoints, login, JWT generation.
- **OrderApiController** (`/api/order`): Creates orders, changes order status, retrieves orders by type.
- **ProductApiController** (`/api/products`): Fetches products by restaurant ID.
- **RestaurantApiController** (`/api/restaurants`): CRUD for restaurants, filter by rating/price range.

Use of DTOs, validation (`@Valid`), and global error handling ensures robust and clear interactions.

### DTOs (Data Transfer Objects)

Located in `com.rocketFoodDelivery.rocketFood.dtos`, these define request/response shapes:

- Auth: `AuthRequestDto`, `AuthResponseSuccessDto`, `AuthResponseErrorDto`
- Orders: `ApiOrderDto`, `ApiOrderRequestDto`, `ApiOrderStatusDto`
- Products: `ApiProductDto`, `ApiProductForOrderApiDto`, `ApiProductOrderRequestDto`
- Restaurants: `ApiRestaurantDto`, `ApiCreateRestaurantDto`
- Address: `ApiAddressDto`
- Common: `ApiErrorDto`, `ApiResponseDto`, `ApiResponseStatusDto`

DTOs ensure that internal models remain decoupled from external representations.

### Global Exception Handling

`GlobalExceptionHandler` handles `ResourceNotFoundException`, `ValidationException`, `BadRequestException`, and other exceptions, returning standardized JSON error responses. This provides consistent feedback and simplifies debugging for clients.

### Domain Models (Entities)

Located in `com.rocketFoodDelivery.rocketFood.models`, these classes map to database tables:

- **UserEntity**: Implements `UserDetails` for security.
- **Address**, **Restaurant**, **Product**, **Customer**, **Employee**, **Courier**, **CourierStatus**, **Order**, **OrderStatus**, **ProductOrder**.

Annotations and validation ensure referential integrity and correctness. JSON annotations prevent infinite recursion in bidirectional relationships.

### Repositories (Data Access Layer)

In `com.rocketFoodDelivery.rocketFood.repository`, Spring Data JPA repositories provide CRUD and custom queries. Entities like `UserEntity`, `Restaurant`, `Product`, `Order` have corresponding repositories for modular data access.

### Services (Business Logic Layer)

Services in `com.rocketFoodDelivery.rocketFood.service` integrate repositories and implement business rules:

- **AuthService**: Authentication logic.
- **AddressService**, **CourierService**, **CustomerService**, **EmployeeService**, **OrderService**, **ProductService**, **RestaurantService**, **OrderStatusService**, **ProductOrderService**, **CourierStatusService**.

They ensure a clean separation of concerns, centralizing complex logic away from controllers.

### Security Configuration

`com.rocketFoodDelivery.rocketFood.security` contains:

- **JwtUtil**: Token generation and validation.
- **JwtTokenFilter**: Extracts and validates the token from the `Authorization` header.
- **SecurityConfig**: Defines which endpoints are secured or open.

### Configuration and Environment

- `application.properties`: Configure MySQL, logging, Hibernate, JWT secret, etc.
- **Maven and Maven Wrapper**:
    - `pom.xml` defines dependencies, plugins, Java version.
    - Use `./mvnw` (Unix) or `mvnw.cmd` (Windows) to ensure a consistent Maven environment.

**Build & Run:**
```bash
./mvnw clean install
./mvnw spring-boot:run
```

### Logging, Validation, and Responses

- **Logging (SLF4J)**: Aids in debugging and monitoring.
- **Validation**: `@Valid` and constraint annotations ensure only valid data enters service logic.
- **ResponseBuilder**: Centralizes JSON response formats for both success and error states.

---

## Installation and Setup

1. **Clone the repository:**
   ```bash
   git clone https://github.com/your-repo/rocket-food-delivery.git
   cd rocket-food-delivery
   ```

2. **Install Dependencies:**
   If you have Maven installed:
   ```bash
   mvn install
   ```
   Or use the Maven Wrapper:
   ```bash
   ./mvnw install
   ```

3. **Set Up MySQL:**
    - Install MySQL and create a database:
      ```sql
      CREATE DATABASE rdelivery;
      CREATE USER 'your_username'@'localhost' IDENTIFIED BY 'your_password';
      GRANT ALL PRIVILEGES ON rdelivery.* TO 'your_username'@'localhost';
      FLUSH PRIVILEGES;
      ```
   Replace `your_username` and `your_password` accordingly.

4. **Configure the Application:**
   Update `src/main/resources/application.properties`:
   ```properties
   spring.datasource.url=jdbc:mysql://localhost:3306/rdelivery
   spring.datasource.username=your_username
   spring.datasource.password=your_password
   spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

   spring.jpa.database-platform=org.hibernate.dialect.MySQLDialect
   spring.jpa.hibernate.ddl-auto=update
   spring.jpa.show-sql=true

   logging.level.root=INFO
   logging.level.com.rocketFoodDelivery=DEBUG
   logging.level.org.hibernate.SQL=DEBUG

   spring.thymeleaf.prefix=classpath:/templates/
   spring.thymeleaf.suffix=.html
   spring.thymeleaf.mode=HTML5
   spring.thymeleaf.encoding=UTF-8
   spring.thymeleaf.cache=false

   app.jwt.secret="your_jwt_secret"
   ```
   Replace `your_username`, `your_password`, and `your_jwt_secret`.

5. **Run the Application:**
   ```bash
   ./mvnw spring-boot:run
   ```

---

## API Endpoints (Examples)

**Authentication:**
- **POST /api/auth**:  
  Request:
  ```json
  {
    "email": "user@example.com",
    "password": "yourpassword"
  }
  ```

**Restaurants:**
- **POST /api/restaurants**:
  ```json
  {
    "user_id": 1,
    "name": "Test Restaurant",
    "phone": "15141234567",
    "email": "test.restaurant@example.com",
    "price_range": 2,
    "address": {
      "street_address": "123 Test St.",
      "city": "Test City",
      "postal_code": "12345"
    }
  }
  ```
- **GET /api/restaurants?price_range=3&rating=4**
- **GET /api/restaurants**
- **GET /api/restaurants/{id}**
- **PUT /api/restaurants/{id}**
- **DELETE /api/restaurants/{id}**

**Orders:**
- **POST /api/order**:
  ```json
  {
    "restaurant_id": 1,
    "customer_id": 2,
    "products": [
      { "id": 1, "quantity": 2 }
    ]
  }
  ```
- **POST /api/order/{order_id}/status** updates order status.
- **GET /api/order?type=customer&id=1** fetches orders by customer.
- **GET /api/order/{id}**, **PUT /api/order/{id}**, **DELETE /api/order/{id}**.

**Products:**
- **GET /api/products?restaurant=1** retrieves products for a restaurant.
- CRUD operations via `POST`, `PUT`, `DELETE` as specified in the code.

**Users:**
- CRUD endpoints to manage user profiles and accounts.

---

## Testing

Add `application-test.properties` to `src/test/resources/`:
```properties
spring.datasource.url=jdbc:mysql://localhost:3306/rdelivery_test
spring.datasource.username=test_username
spring.datasource.password=test_password
spring.jpa.hibernate.ddl-auto=create-drop
spring.jpa.show-sql=true
logging.level.root=INFO
logging.level.com.rocketFoodDelivery=DEBUG
logging.level.org.hibernate.SQL=DEBUG
```

Run tests:
```bash
./mvnw test
```

Integration tests and unit tests ensure the reliability of authentication, orders, products, and restaurant operations.

---

## Summary of Key Features

- **Layered Architecture**: Clear separation of concerns.
- **Rich Domain Model & DTOs**: Stable API contracts.
- **JWT Authentication**: Secure, stateless access.
- **Global Error Handling & Validation**: Consistent and clear responses.
- **Filtering, Querying, and Robust Testing**: Enhanced flexibility and quality assurance.
- **Maven Wrapper & CI-Ready**: Easy builds and integration into deployment pipelines.

---

## Acknowledgments

- **Spring Boot** for the framework.
- **MySQL** for database management.
- **CodeBoxx Academy** for training and educational environment.

