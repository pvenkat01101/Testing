# Phase 5.4: API Testing - Theory

## Table of Contents
1. [Introduction to API Testing](#introduction-to-api-testing)
2. [REST API Fundamentals](#rest-api-fundamentals)
3. [HTTP Methods](#http-methods)
4. [Status Codes](#status-codes)
5. [API Testing Tools](#api-testing-tools)
6. [REST Assured Framework](#rest-assured-framework)
7. [Authentication in APIs](#authentication-in-apis)
8. [Best Practices](#best-practices)

---

## 1. Introduction to API Testing

### What is API?

**API (Application Programming Interface)** is a set of rules that allows programs to talk to each other. It defines methods and data formats that applications can use to communicate.

### What is API Testing?

**API Testing** is a type of software testing that validates Application Programming Interfaces. It tests the business logic, security, performance, and reliability of APIs.

### Why API Testing?

**Advantages over UI Testing:**
- ✅ **Faster:** No UI rendering, direct backend testing
- ✅ **More Stable:** Less flaky than UI tests
- ✅ **Earlier Testing:** Test before UI is ready
- ✅ **Better Coverage:** Test all edge cases easily
- ✅ **Language Independent:** Any language can test APIs
- ✅ **Easy Automation:** Simple request-response validation

### API vs UI Testing:

| Aspect | UI Testing | API Testing |
|--------|------------|-------------|
| Speed | Slow | Fast |
| Stability | Flaky | Stable |
| Maintenance | High | Low |
| Coverage | Limited | Comprehensive |
| When | After UI ready | Before UI |

---

## 2. REST API Fundamentals

### What is REST?

**REST (Representational State Transfer)** is an architectural style for designing networked applications.

### REST Principles:

1. **Client-Server:** Separation of concerns
2. **Stateless:** Each request contains all information
3. **Cacheable:** Responses can be cached
4. **Uniform Interface:** Standard HTTP methods
5. **Layered System:** Multiple layers between client-server

### REST API Components:

**1. Endpoint (URL):**
```
https://api.example.com/users/123
```

**2. HTTP Method:**
- GET, POST, PUT, PATCH, DELETE

**3. Headers:**
```
Content-Type: application/json
Authorization: Bearer token123
Accept: application/json
```

**4. Request Body (for POST/PUT):**
```json
{
  "name": "John Doe",
  "email": "john@example.com"
}
```

**5. Response:**
```json
{
  "id": 123,
  "name": "John Doe",
  "email": "john@example.com",
  "created_at": "2024-01-01"
}
```

**6. Status Code:**
- 200 OK, 201 Created, 400 Bad Request, etc.

---

## 3. HTTP Methods

### GET - Retrieve Data

**Purpose:** Fetch resources from server

**Example:**
```
GET https://api.example.com/users
GET https://api.example.com/users/123
```

**Characteristics:**
- No request body
- Idempotent (same result on multiple calls)
- Safe (doesn't modify data)
- Can be cached
- Bookmarkable

**Response:**
```json
{
  "id": 123,
  "name": "John Doe",
  "email": "john@example.com"
}
```

### POST - Create Data

**Purpose:** Create new resources

**Example:**
```
POST https://api.example.com/users

Body:
{
  "name": "John Doe",
  "email": "john@example.com"
}
```

**Characteristics:**
- Has request body
- NOT idempotent (creates new resource each time)
- NOT safe (modifies data)
- NOT cacheable

**Response:**
```json
{
  "id": 456,
  "name": "John Doe",
  "email": "john@example.com",
  "created_at": "2024-01-29"
}
```

### PUT - Update (Full Replace)

**Purpose:** Update entire resource

**Example:**
```
PUT https://api.example.com/users/123

Body:
{
  "name": "John Updated",
  "email": "johnupdated@example.com",
  "phone": "1234567890"
}
```

**Characteristics:**
- Replaces entire resource
- Idempotent
- Requires all fields

### PATCH - Update (Partial)

**Purpose:** Update specific fields

**Example:**
```
PATCH https://api.example.com/users/123

Body:
{
  "email": "newemail@example.com"
}
```

**Characteristics:**
- Updates only specified fields
- More efficient than PUT
- Partial update

### DELETE - Remove Data

**Purpose:** Delete resources

**Example:**
```
DELETE https://api.example.com/users/123
```

**Characteristics:**
- No request body (usually)
- Idempotent
- Response: 204 No Content or 200 OK

---

## 4. Status Codes

### 1xx - Informational

- **100** Continue
- **101** Switching Protocols

### 2xx - Success

- **200** OK - Request succeeded
- **201** Created - Resource created successfully
- **202** Accepted - Request accepted, processing not complete
- **204** No Content - Success but no body to return

### 3xx - Redirection

- **301** Moved Permanently
- **302** Found (Temporary Redirect)
- **304** Not Modified (Cached)

### 4xx - Client Errors

- **400** Bad Request - Invalid syntax
- **401** Unauthorized - Authentication required
- **403** Forbidden - No permission
- **404** Not Found - Resource doesn't exist
- **405** Method Not Allowed
- **409** Conflict - Resource conflict
- **422** Unprocessable Entity - Validation failed
- **429** Too Many Requests - Rate limit exceeded

### 5xx - Server Errors

- **500** Internal Server Error
- **501** Not Implemented
- **502** Bad Gateway
- **503** Service Unavailable
- **504** Gateway Timeout

---

## 5. API Testing Tools

### 1. Postman

**Most popular API testing tool**

**Features:**
- Manual API testing
- Collections for organizing requests
- Environment variables
- Pre-request scripts
- Tests with JavaScript
- Mock servers
- API documentation

**Use Cases:**
- Quick manual testing
- API exploration
- Creating test collections
- Sharing with team

### 2. REST Assured (Java)

**Java library for API automation**

**Features:**
- BDD style syntax
- Easy request/response validation
- JSON/XML parsing
- Authentication support
- Integration with TestNG/JUnit

**Example:**
```java
given()
    .header("Content-Type", "application/json")
    .body(requestBody)
.when()
    .post("/users")
.then()
    .statusCode(201)
    .body("name", equalTo("John Doe"));
```

### 3. Other Tools

- **SoapUI:** SOAP and REST APIs
- **JMeter:** Performance testing
- **Karate DSL:** BDD style API testing
- **Cypress:** Modern web testing (can test APIs)
- **newman:** Postman CLI runner

---

## 6. REST Assured Framework

### Setup (Maven):

```xml
<dependency>
    <groupId>io.rest-assured</groupId>
    <artifactId>rest-assured</artifactId>
    <version>5.4.0</version>
</dependency>
```

### Basic Structure:

```java
given()  // Setup: headers, body, auth, params
.when()   // Action: HTTP method and endpoint
.then()   // Validation: status, body, headers
```

### Complete Example:

```java
import io.restassured.RestAssured;
import static io.restassured.RestAssured.*;
import static org.hamcrest.Matchers.*;

public class APITest {
    
    @BeforeClass
    public void setup() {
        RestAssured.baseURI = "https://reqres.in";
    }
    
    @Test
    public void testGetUser() {
        given()
            .pathParam("id", 2)
        .when()
            .get("/api/users/{id}")
        .then()
            .statusCode(200)
            .body("data.id", equalTo(2))
            .body("data.email", containsString("@reqres.in"))
            .body("data.first_name", notNullValue());
    }
    
    @Test
    public void testCreateUser() {
        String requestBody = "{\n" +
            "  \"name\": \"John\",\n" +
            "  \"job\": \"QA Engineer\"\n" +
            "}";
        
        given()
            .header("Content-Type", "application/json")
            .body(requestBody)
        .when()
            .post("/api/users")
        .then()
            .statusCode(201)
            .body("name", equalTo("John"))
            .body("job", equalTo("QA Engineer"))
            .body("id", notNullValue())
            .body("createdAt", notNullValue());
    }
}
```

### Key Features:

**1. Request Specification:**
```java
RequestSpecification request = given()
    .baseUri("https://api.example.com")
    .header("Authorization", "Bearer token")
    .header("Content-Type", "application/json");
```

**2. Response Extraction:**
```java
Response response = given()
    .when()
    .get("/users/1");

int statusCode = response.getStatusCode();
String body = response.getBody().asString();
String name = response.jsonPath().getString("name");
```

**3. JSON Path:**
```java
.then()
    .body("data[0].name", equalTo("John"))
    .body("data.size()", equalTo(5))
    .body("data.id", hasItems(1, 2, 3));
```

**4. Query Parameters:**
```java
given()
    .queryParam("page", 2)
    .queryParam("per_page", 10)
.when()
    .get("/users");
```

**5. Path Parameters:**
```java
given()
    .pathParam("id", 123)
.when()
    .get("/users/{id}");
```

---

## 7. Authentication in APIs

### 1. No Auth

Public APIs without authentication.

### 2. API Key

```java
given()
    .header("X-API-Key", "your-api-key")
.when()
    .get("/protected-endpoint");
```

### 3. Bearer Token

```java
given()
    .header("Authorization", "Bearer " + token)
.when()
    .get("/user/profile");
```

### 4. Basic Auth

```java
given()
    .auth()
    .basic("username", "password")
.when()
    .get("/secure-data");
```

### 5. OAuth 2.0

```java
given()
    .auth()
    .oauth2(accessToken)
.when()
    .get("/api/data");
```

---

## 8. Best Practices

### API Testing Best Practices:

✅ **Test Organization:**
- Group tests by resource/endpoint
- Use meaningful test names
- Independent tests

✅ **Validations:**
- Status code
- Response time
- Response body structure
- Response data values
- Headers
- Content-Type

✅ **Test Data:**
- Use dynamic test data
- Clean up created data
- Don't depend on existing data

✅ **Assertions:**
- Multiple assertions per test
- Validate both positive and negative scenarios
- Test edge cases

✅ **Reusability:**
- Base URI configuration
- Common headers in @BeforeClass
- Reusable request specifications
- Utility methods

✅ **Error Handling:**
- Test error responses
- Validate error messages
- Check status codes

### What to Test in APIs:

1. **Functionality:**
   - API returns expected data
   - All CRUD operations work
   - Business logic is correct

2. **Response Validation:**
   - Status codes
   - Response body
   - Response time
   - Headers

3. **Error Handling:**
   - Invalid inputs
   - Missing parameters
   - Authentication failures
   - Server errors

4. **Security:**
   - Authentication
   - Authorization
   - Data encryption
   - SQL injection
   - XSS attacks

5. **Performance:**
   - Response time
   - Load handling
   - Concurrent users

---

## Summary

**API Testing** is essential for:
- Faster feedback
- Better test coverage
- Early defect detection
- Stable automation

**Key Concepts:**
- REST principles
- HTTP methods (GET, POST, PUT, PATCH, DELETE)
- Status codes
- Request/Response structure
- Authentication

**REST Assured:**
- given-when-then syntax
- Easy validations
- Java integration
- TestNG/JUnit compatible

**Next:** Hands-on API testing with REST Assured!

---

🚀 Ready to automate API testing!
