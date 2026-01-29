# Phase 5.4: API Testing - Practicals

## Hands-On API Testing Exercises

### Exercise 1: GET Request - Retrieve User

```java
@Test
public void testGetSingleUser() {
    RestAssured.baseURI = "https://reqres.in";
    
    given()
        .pathParam("id", 2)
    .when()
        .get("/api/users/{id}")
    .then()
        .statusCode(200)
        .body("data.id", equalTo(2))
        .body("data.first_name", equalTo("Janet"))
        .body("data.last_name", equalTo("Weaver"))
        .log().all();
}
```

### Exercise 2: POST Request - Create User

```java
@Test
public void testCreateUser() {
    RestAssured.baseURI = "https://reqres.in";
    
    JSONObject requestBody = new JSONObject();
    requestBody.put("name", "John Doe");
    requestBody.put("job", "QA Engineer");
    
    given()
        .header("Content-Type", "application/json")
        .body(requestBody.toString())
    .when()
        .post("/api/users")
    .then()
        .statusCode(201)
        .body("name", equalTo("John Doe"))
        .body("job", equalTo("QA Engineer"))
        .body("id", notNullValue())
        .body("createdAt", notNullValue())
        .log().all();
}
```

### Exercise 3: PUT Request - Update User

```java
@Test
public void testUpdateUser() {
    RestAssured.baseURI = "https://reqres.in";
    
    JSONObject requestBody = new JSONObject();
    requestBody.put("name", "John Updated");
    requestBody.put("job", "Senior QA Engineer");
    
    given()
        .header("Content-Type", "application/json")
        .body(requestBody.toString())
        .pathParam("id", 2)
    .when()
        .put("/api/users/{id}")
    .then()
        .statusCode(200)
        .body("name", equalTo("John Updated"))
        .body("job", equalTo("Senior QA Engineer"))
        .body("updatedAt", notNullValue())
        .log().all();
}
```

### Exercise 4: DELETE Request - Remove User

```java
@Test
public void testDeleteUser() {
    RestAssured.baseURI = "https://reqres.in";
    
    given()
        .pathParam("id", 2)
    .when()
        .delete("/api/users/{id}")
    .then()
        .statusCode(204)
        .log().all();
}
```

### Exercise 5: Query Parameters

```java
@Test
public void testGetUsersWithQueryParams() {
    RestAssured.baseURI = "https://reqres.in";
    
    given()
        .queryParam("page", 2)
        .queryParam("per_page", 3)
    .when()
        .get("/api/users")
    .then()
        .statusCode(200)
        .body("page", equalTo(2))
        .body("per_page", equalTo(3))
        .body("data.size()", equalTo(3))
        .log().all();
}
```

### Exercise 6: Response Extraction

```java
@Test
public void testResponseExtraction() {
    RestAssured.baseURI = "https://reqres.in";
    
    Response response = given()
        .when()
        .get("/api/users/2");
    
    // Extract values
    int statusCode = response.getStatusCode();
    String body = response.getBody().asString();
    String firstName = response.jsonPath().getString("data.first_name");
    String email = response.jsonPath().getString("data.email");
    
    // Assertions
    Assert.assertEquals(statusCode, 200);
    Assert.assertEquals(firstName, "Janet");
    Assert.assertTrue(email.contains("@reqres.in"));
    
    System.out.println("Status Code: " + statusCode);
    System.out.println("First Name: " + firstName);
    System.out.println("Email: " + email);
}
```

### Exercise 7: Authentication

```java
@Test
public void testBasicAuth() {
    RestAssured.baseURI = "https://postman-echo.com";
    
    given()
        .auth()
        .basic("postman", "password")
    .when()
        .get("/basic-auth")
    .then()
        .statusCode(200)
        .body("authenticated", equalTo(true))
        .log().all();
}

@Test
public void testBearerToken() {
    String token = "your-bearer-token";
    
    given()
        .header("Authorization", "Bearer " + token)
    .when()
        .get("/api/protected-endpoint")
    .then()
        .statusCode(200);
}
```

### Exercise 8: JSON Schema Validation

```java
@Test
public void testJSONSchemaValidation() {
    RestAssured.baseURI = "https://reqres.in";
    
    given()
        .pathParam("id", 2)
    .when()
        .get("/api/users/{id}")
    .then()
        .statusCode(200)
        .body(matchesJsonSchemaInClasspath("user-schema.json"));
}
```

### Exercise 9: Headers Validation

```java
@Test
public void testResponseHeaders() {
    RestAssured.baseURI = "https://reqres.in";
    
    given()
    .when()
        .get("/api/users/1")
    .then()
        .statusCode(200)
        .header("Content-Type", "application/json; charset=utf-8")
        .header("Connection", "keep-alive")
        .headers("Content-Type", containsString("json"))
        .log().headers();
}
```

### Exercise 10: Response Time Validation

```java
@Test
public void testResponseTime() {
    RestAssured.baseURI = "https://reqres.in";
    
    given()
    .when()
        .get("/api/users")
    .then()
        .statusCode(200)
        .time(lessThan(2000L), TimeUnit.MILLISECONDS)
        .log().all();
}
```

### Complete API Test Class Example

```java
import io.restassured.RestAssured;
import io.restassured.response.Response;
import org.testng.annotations.*;
import static io.restassured.RestAssured.*;
import static org.hamcrest.Matchers.*;
import org.json.JSONObject;

public class ReqResAPITests {
    
    @BeforeClass
    public void setup() {
        RestAssured.baseURI = "https://reqres.in";
        RestAssured.basePath = "/api";
    }
    
    @Test(priority = 1)
    public void testListUsers() {
        given()
            .queryParam("page", 1)
        .when()
            .get("/users")
        .then()
            .statusCode(200)
            .body("page", equalTo(1))
            .body("data", notNullValue())
            .body("data.size()", greaterThan(0));
    }
    
    @Test(priority = 2)
    public void testCreateUser() {
        JSONObject request = new JSONObject();
        request.put("name", "TestUser");
        request.put("job", "Tester");
        
        Response response = given()
            .header("Content-Type", "application/json")
            .body(request.toString())
        .when()
            .post("/users")
        .then()
            .statusCode(201)
            .extract().response();
        
        String userId = response.jsonPath().getString("id");
        System.out.println("Created User ID: " + userId);
    }
    
    @Test(priority = 3)
    public void testUpdateUser() {
        JSONObject request = new JSONObject();
        request.put("name", "UpdatedUser");
        request.put("job", "Senior Tester");
        
        given()
            .header("Content-Type", "application/json")
            .body(request.toString())
        .when()
            .put("/users/2")
        .then()
            .statusCode(200)
            .body("name", equalTo("UpdatedUser"))
            .body("job", equalTo("Senior Tester"));
    }
    
    @Test(priority = 4)
    public void testDeleteUser() {
        given()
        .when()
            .delete("/users/2")
        .then()
            .statusCode(204);
    }
    
    @Test
    public void testUserNotFound() {
        given()
        .when()
            .get("/users/9999")
        .then()
            .statusCode(404);
    }
    
    @AfterClass
    public void tearDown() {
        System.out.println("All API tests completed");
    }
}
```

### Practice Resources:

1. **https://reqres.in** - Free REST API for testing
2. **https://jsonplaceholder.typicode.com** - Fake Online REST API
3. **https://postman-echo.com** - Test different HTTP methods
4. **https://restful-booker.herokuapp.com** - Booking API for testing

### Key Takeaways:

- REST Assured provides BDD-style syntax
- Easy to validate status codes, headers, and response body
- Supports all HTTP methods
- Can handle authentication
- Integration with TestNG/JUnit
- Excellent for API automation

---

🚀 Practice these exercises to master API testing!
