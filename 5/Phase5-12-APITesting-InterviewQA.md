# Phase 5.4: API Testing - Interview Q&A

## Common API Testing Interview Questions

### Q1: What is API Testing? Why is it important?

**Answer:** API Testing is a type of software testing that validates Application Programming Interfaces (APIs). It tests the business logic, security, performance, and reliability of APIs at the message layer without focusing on the UI.

**Importance:**
- **Faster Feedback:** Tests run faster than UI tests
- **Early Testing:** Can test before UI is ready
- **Better Coverage:** Can test all edge cases easily
- **More Stable:** Less flaky than UI tests
- **Language Independent:** Any client can test the API

### Q2: What is the difference between API Testing and UI Testing?

**Answer:**

| Aspect | API Testing | UI Testing |
|--------|------------|------------|
| **Level** | Tests business logic directly | Tests through user interface |
| **Speed** | Fast (milliseconds) | Slow (seconds) |
| **Stability** | Very stable | Can be flaky |
| **Maintenance** | Low | High |
| **Coverage** | Can test all scenarios | Limited by UI |
| **Timing** | Early in development | After UI is ready |
| **Flakiness** | Minimal | Common |

### Q3: What is REST API? Explain REST principles.

**Answer:** REST (Representational State Transfer) is an architectural style for designing networked applications.

**REST Principles:**
1. **Client-Server:** Separation between client and server
2. **Stateless:** Each request contains all necessary information
3. **Cacheable:** Responses can be cached
4. **Uniform Interface:** Standard HTTP methods (GET, POST, etc.)
5. **Layered System:** Can have multiple layers between client-server

**REST API Example:**
```
GET https://api.example.com/users/123
POST https://api.example.com/users
PUT https://api.example.com/users/123
DELETE https://api.example.com/users/123
```

### Q4: Explain different HTTP methods and when to use them.

**Answer:**

**GET - Retrieve Data:**
- Fetches resources from server
- No request body
- Idempotent (same result on multiple calls)
- Example: Get user details, list products

**POST - Create Data:**
- Creates new resources
- Has request body
- NOT idempotent
- Example: Create new user, submit order

**PUT - Update (Full):**
- Updates entire resource
- Idempotent
- Requires all fields
- Example: Update complete user profile

**PATCH - Update (Partial):**
- Updates specific fields only
- More efficient than PUT
- Example: Update just email address

**DELETE - Remove Data:**
- Deletes resources
- Idempotent
- Usually no request body
- Example: Delete user account

### Q5: Explain common HTTP status codes.

**Answer:**

**2xx - Success:**
- **200 OK:** Request succeeded
- **201 Created:** Resource created successfully
- **204 No Content:** Success but no response body

**4xx - Client Errors:**
- **400 Bad Request:** Invalid request syntax
- **401 Unauthorized:** Authentication required
- **403 Forbidden:** No permission
- **404 Not Found:** Resource doesn't exist
- **422 Unprocessable Entity:** Validation failed
- **429 Too Many Requests:** Rate limit exceeded

**5xx - Server Errors:**
- **500 Internal Server Error:** Server crashed
- **502 Bad Gateway:** Invalid response from upstream
- **503 Service Unavailable:** Server temporarily down
- **504 Gateway Timeout:** Upstream server timeout

### Q6: What is REST Assured? Why do we use it?

**Answer:** REST Assured is a Java library for testing REST APIs. It provides BDD-style syntax for API testing.

**Why REST Assured:**
- ✅ Simple BDD syntax (given-when-then)
- ✅ Easy JSON/XML validation
- ✅ Supports all HTTP methods
- ✅ Built-in authentication support
- ✅ Integration with TestNG/JUnit
- ✅ Easy request/response logging
- ✅ JSON Path and XML Path support

**Example:**
```java
given()
    .header("Content-Type", "application/json")
    .body(requestBody)
.when()
    .post("/users")
.then()
    .statusCode(201)
    .body("name", equalTo("John"));
```

### Q7: How do you handle authentication in API testing?

**Answer:**

**1. No Auth:** Public APIs
```java
given().when().get("/public/data")
```

**2. API Key:**
```java
given()
    .header("X-API-Key", "your-api-key")
.when().get("/protected")
```

**3. Bearer Token:**
```java
given()
    .header("Authorization", "Bearer " + token)
.when().get("/user/profile")
```

**4. Basic Auth:**
```java
given()
    .auth().basic("username", "password")
.when().get("/secure-data")
```

**5. OAuth 2.0:**
```java
given()
    .auth().oauth2(accessToken)
.when().get("/api/data")
```

### Q8: What validations do you perform in API testing?

**Answer:**

**1. Status Code Validation:**
```java
.then().statusCode(200)
```

**2. Response Body Validation:**
```java
.then()
    .body("name", equalTo("John"))
    .body("email", containsString("@"))
    .body("id", notNullValue())
```

**3. Response Time:**
```java
.then().time(lessThan(2000L), TimeUnit.MILLISECONDS)
```

**4. Headers:**
```java
.then()
    .header("Content-Type", "application/json")
    .header("Connection", "keep-alive")
```

**5. JSON Schema:**
```java
.then().body(matchesJsonSchemaInClasspath("schema.json"))
```

**6. Response Structure:**
- Check data types
- Required fields present
- Array sizes
- Nested objects

### Q9: How do you handle dynamic data in API tests?

**Answer:**

**Problem:** IDs, timestamps, etc. change on every response

**Solutions:**

**1. Extract and Use:**
```java
// Extract userId from POST response
Response response = given().body(request).post("/users");
String userId = response.jsonPath().getString("id");

// Use in subsequent request
given().pathParam("id", userId).get("/users/{id}");
```

**2. Validate Data Type, Not Value:**
```java
.then()
    .body("id", notNullValue())
    .body("createdAt", matchesPattern("\\d{4}-\\d{2}-\\d{2}"))
```

**3. Regular Expressions:**
```java
.body("email", matchesPattern("[a-zA-Z0-9]+@[a-zA-Z]+\\.[a-zA-Z]+"))
```

**4. Use Matchers:**
```java
.body("price", greaterThan(0))
.body("status", isOneOf("active", "inactive"))
```

### Q10: Explain given-when-then in REST Assured.

**Answer:**

REST Assured uses BDD (Behavior Driven Development) style:

**given()** - Setup/Preconditions:
- Headers
- Request body
- Authentication
- Query/Path parameters
```java
given()
    .header("Content-Type", "application/json")
    .auth().basic("user", "pass")
    .body(requestBody)
    .queryParam("page", 1)
```

**when()** - Action:
- HTTP method (GET, POST, PUT, DELETE)
- Endpoint
```java
.when()
    .post("/api/users")
```

**then()** - Validation:
- Status code
- Response body
- Headers
- Response time
```java
.then()
    .statusCode(201)
    .body("name", equalTo("John"))
    .time(lessThan(2000L))
```

### Q11: How do you test negative scenarios in APIs?

**Answer:**

**1. Invalid Data:**
```java
@Test
public void testInvalidEmail() {
    JSONObject request = new JSONObject();
    request.put("email", "invalid-email");  // Invalid format
    
    given().body(request).post("/users")
    .then()
        .statusCode(400)
        .body("error", containsString("Invalid email"));
}
```

**2. Missing Required Fields:**
```java
@Test
public void testMissingRequiredField() {
    JSONObject request = new JSONObject();
    // Not including required 'name' field
    request.put("email", "test@test.com");
    
    given().body(request).post("/users")
    .then().statusCode(400);
}
```

**3. Unauthorized Access:**
```java
@Test
public void testUnauthorizedAccess() {
    given()  // No auth header
    .when().get("/protected-resource")
    .then().statusCode(401);
}
```

**4. Resource Not Found:**
```java
@Test
public void testResourceNotFound() {
    given()
    .when().get("/users/99999")  // Non-existent ID
    .then().statusCode(404);
}
```

### Q12: What is the difference between PUT and PATCH?

**Answer:**

**PUT - Full Update:**
- Replaces entire resource
- Must send all fields
- Idempotent

```java
PUT /users/123
{
  "name": "John",
  "email": "john@example.com",
  "phone": "1234567890",
  "address": "123 Street"
}
```

**PATCH - Partial Update:**
- Updates only specified fields
- More efficient
- Send only fields to update

```java
PATCH /users/123
{
  "email": "newemail@example.com"
}
```

**Use Cases:**
- **PUT:** When you want to replace entire record
- **PATCH:** When you want to update specific fields (preferred)

### Q13: How do you integrate API tests with CI/CD?

**Answer:**

**1. Maven/TestNG Setup:**
```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <configuration>
        <suiteXmlFiles>
            <suiteXmlFile>testng.xml</suiteXmlFile>
        </suiteXmlFiles>
    </configuration>
</plugin>
```

**2. Jenkins Pipeline:**
```groovy
pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/repo/api-tests.git'
            }
        }
        stage('Run API Tests') {
            steps {
                sh 'mvn clean test'
            }
        }
        stage('Publish Reports') {
            steps {
                publishHTML([
                    reportDir: 'test-output',
                    reportFiles: 'index.html',
                    reportName: 'API Test Report'
                ])
            }
        }
    }
}
```

**3. GitHub Actions:**
```yaml
name: API Tests
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Set up JDK
        uses: actions/setup-java@v2
        with:
          java-version: '11'
      - name: Run tests
        run: mvn clean test
```

### Q14: What challenges have you faced in API testing?

**Answer:**

**1. Dynamic Data:**
- **Challenge:** IDs and timestamps change
- **Solution:** Extract values and use in subsequent requests

**2. Authentication:**
- **Challenge:** Token expiration
- **Solution:** Refresh tokens or generate new ones before tests

**3. Data Dependencies:**
- **Challenge:** Test order matters
- **Solution:** Make tests independent, create required data in @BeforeMethod

**4. Environment Issues:**
- **Challenge:** Different environments (QA, Staging, Prod)
- **Solution:** Use configuration files for environment-specific data

**5. Rate Limiting:**
- **Challenge:** Too many requests blocked
- **Solution:** Add delays, use different test accounts

### Q15: How do you perform data-driven testing for APIs?

**Answer:**

**Using TestNG DataProvider:**

```java
@Test(dataProvider = "userData")
public void testCreateUser(String name, String job, int expectedStatus) {
    JSONObject request = new JSONObject();
    request.put("name", name);
    request.put("job", job);
    
    given()
        .body(request.toString())
    .when()
        .post("/users")
    .then()
        .statusCode(expectedStatus);
}

@DataProvider(name = "userData")
public Object[][] getUserData() {
    return new Object[][] {
        {"John Doe", "Engineer", 201},
        {"Jane Smith", "Manager", 201},
        {"", "Developer", 400},  // Empty name - should fail
    };
}
```

**Using Excel:**
```java
@DataProvider(name = "excelData")
public Object[][] getDataFromExcel() {
    return ExcelReader.getData("testdata.xlsx", "APITests");
}
```

---

## Key Interview Tips:

1. **Explain with Examples:** Always provide code examples
2. **Mention Tools:** Postman, REST Assured, SoapUI
3. **Discuss Framework:** Explain your API testing framework structure
4. **Real Experience:** Share challenges and how you solved them
5. **Best Practices:** Validation strategies, data management, reporting
6. **CI/CD Integration:** How tests run in pipeline
7. **Performance:** Mention response time validation
8. **Security:** Authentication testing, data validation

---

## Additional Topics to Prepare:

- SOAP vs REST
- GraphQL APIs
- WebSocket testing
- Microservices testing
- Contract testing
- Mock servers
- API documentation (Swagger/OpenAPI)
- Performance testing (JMeter)
- Security testing
- Versioning in APIs

---

Good luck with your API testing interviews! 🚀
