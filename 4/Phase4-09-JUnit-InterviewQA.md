# Phase 4.3 - JUnit Framework: Interview Questions and Answers

## Section 1: JUnit Basics (Questions 1-10)

### Q1: What is JUnit and why is it used?

**Answer**:
JUnit is a unit testing framework for Java that provides annotations, assertions, and test runners to write and execute automated tests.

**Current Version**: JUnit 5 (Jupiter) - released 2017

**Why use JUnit**:
- Simple and easy to use
- Annotations for test lifecycle (@Test, @BeforeEach, @AfterEach)
- Rich assertion library
- Parameterized tests
- Integration with Maven, Gradle, IDEs
- Large community support
- Test-driven development (TDD) support

**Practical Example**:
```java
@Test
void testAddition() {
    assertEquals(5, 2 + 3);
}
```

**In-depth Explanation**:
JUnit is the de facto standard for Java unit testing. JUnit 5 is a complete rewrite with modern features supporting Java 8+, making it more powerful and flexible than JUnit 4.

---

### Q2: What are the main differences between JUnit 4 and JUnit 5?

**Answer**:

| Feature | JUnit 4 | JUnit 5 |
|---------|---------|---------|
| Architecture | Monolithic | Modular (Platform, Jupiter, Vintage) |
| Java Version | Java 5+ | Java 8+ |
| Package | org.junit | org.junit.jupiter.api |
| Annotations | @Before, @After, @BeforeClass, @AfterClass | @BeforeEach, @AfterEach, @BeforeAll, @AfterAll |
| Test Method | Must be public | Can be package-private |
| Assertions | org.junit.Assert | org.junit.jupiter.api.Assertions |
| @Ignore | @Ignore | @Disabled |
| Exception Testing | @Test(expected=...) | assertThrows() |
| Timeout | @Test(timeout=...) | @Timeout or assertTimeout() |
| Parameterized | @RunWith(Parameterized.class) | @ParameterizedTest with multiple providers |
| Nested Tests | No | @Nested |
| Grouped Assertions | No | assertAll() |
| Extension Model | @Rule, @RunWith | @ExtendWith |

**Practical Example**:
```java
// JUnit 4
@Test(expected = ArithmeticException.class)
public void testDivisionByZero() {
    int result = 10 / 0;
}

// JUnit 5
@Test
void testDivisionByZero() {
    assertThrows(ArithmeticException.class, () -> {
        int result = 10 / 0;
    });
}
```

**In-depth Explanation**:
JUnit 5 provides better separation of concerns with modular architecture, improved parameterized testing, and better support for modern Java features like lambdas.

---

### Q3: Explain the JUnit 5 architecture.

**Answer**:
JUnit 5 consists of three main modules:

**1. JUnit Platform**:
- Foundation for launching testing frameworks on JVM
- Defines TestEngine API
- Console Launcher for running tests
- Integration with build tools (Maven, Gradle)

**2. JUnit Jupiter**:
- New programming model for writing tests
- New annotations (@Test, @BeforeEach, etc.)
- New assertion and assumption methods
- Extension API
- @ParameterizedTest support

**3. JUnit Vintage**:
- Backward compatibility with JUnit 3 and JUnit 4
- Allows running old tests on JUnit 5 platform
- Helps migration

**Practical Example**:
```xml
<!-- Maven dependencies -->
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter</artifactId>  <!-- Includes Jupiter API and Engine -->
</dependency>

<dependency>
    <groupId>org.junit.vintage</groupId>
    <artifactId>junit-vintage-engine</artifactId>  <!-- Run JUnit 4 tests -->
</dependency>
```

**In-depth Explanation**:
Modular architecture allows different testing frameworks to run on same platform and enables backward compatibility while introducing modern features.

---

### Q4: What are the key annotations in JUnit 5?

**Answer**:

**Lifecycle Annotations**:
```java
@BeforeAll    // Runs once before all tests (must be static)
@BeforeEach   // Runs before each test
@Test         // Marks test method
@AfterEach    // Runs after each test
@AfterAll     // Runs once after all tests (must be static)
```

**Other Important Annotations**:
```java
@DisplayName("Custom test name")  // Readable test names
@Disabled("Reason")               // Skip test
@Tag("smoke")                     // Tag for filtering
@Nested                           // Nested test classes
@RepeatedTest(5)                  // Repeat test 5 times
@ParameterizedTest               // Parameterized test
@Timeout(value = 2, unit = SECONDS)  // Timeout
```

**Practical Example**:
```java
class LifecycleTest {

    @BeforeAll
    static void setupAll() {
        System.out.println("Setup once");
    }

    @BeforeEach
    void setup() {
        System.out.println("Setup before each");
    }

    @Test
    @DisplayName("Test addition")
    void testAdd() {
        assertEquals(5, 2 + 3);
    }

    @Test
    @Disabled("Not implemented")
    void testFeature() { }

    @AfterEach
    void teardown() {
        System.out.println("Cleanup after each");
    }

    @AfterAll
    static void teardownAll() {
        System.out.println("Cleanup once");
    }
}
```

**In-depth Explanation**:
JUnit 5 uses more descriptive annotation names (@BeforeEach vs @Before) and provides additional annotations for better test organization and control.

---

### Q5: What are assertions in JUnit 5?

**Answer**:
Assertions verify expected vs actual results. JUnit 5 provides assertions in `org.junit.jupiter.api.Assertions` class.

**Common Assertions**:
```java
import static org.junit.jupiter.api.Assertions.*;

// Equality
assertEquals(expected, actual);
assertEquals(expected, actual, "Error message");

// Boolean
assertTrue(condition);
assertFalse(condition);

// Null checks
assertNull(object);
assertNotNull(object);

// Array equality
assertArrayEquals(expectedArray, actualArray);

// Reference equality
assertSame(obj1, obj2);        // Same object reference
assertNotSame(obj1, obj2);     // Different references

// Exception testing
assertThrows(Exception.class, () -> {
    // Code that should throw
});

// Timeout testing
assertTimeout(Duration.ofSeconds(2), () -> {
    // Code that should complete within 2 seconds
});

// Grouped assertions (all execute, all failures reported)
assertAll("user",
    () -> assertEquals("John", user.getFirstName()),
    () -> assertEquals("Doe", user.getLastName())
);
```

**Practical Example**:
```java
@Test
void testUser() {
    User user = new User("John", "Doe", 30);

    assertAll("User properties",
        () -> assertEquals("John", user.getFirstName()),
        () -> assertEquals("Doe", user.getLastName()),
        () -> assertEquals(30, user.getAge())
    );
}
```

**In-depth Explanation**:
JUnit 5 adds powerful assertions like `assertAll()` (execute all assertions even if some fail) and `assertThrows()` (cleaner exception testing with lambdas).

---

### Q6: What is @DisplayName and why use it?

**Answer**:
@DisplayName provides custom, readable names for tests and test classes, improving test report readability.

**Without @DisplayName**:
```java
@Test
void testValidLoginWithCorrectCredentials() {
    // Test report shows: testValidLoginWithCorrectCredentials
}
```

**With @DisplayName**:
```java
@Test
@DisplayName("Login should succeed with valid credentials")
void testValidLogin() {
    // Test report shows: Login should succeed with valid credentials
}
```

**Class Level**:
```java
@DisplayName("Calculator Test Suite")
class CalculatorTest {

    @Test
    @DisplayName("Add two positive numbers")
    void testAddition() {
        assertEquals(5, 2 + 3);
    }

    @Test
    @DisplayName("Divide by zero should throw ArithmeticException")
    void testDivisionByZero() {
        assertThrows(ArithmeticException.class, () -> {
            int result = 10 / 0;
        });
    }
}
```

**Benefits**:
- More readable test reports
- Supports spaces, special characters, emojis
- Better communication of test intent
- Non-technical stakeholders can understand reports

**In-depth Explanation**:
@DisplayName separates technical method names from business-readable descriptions, improving test documentation and reporting.

---

### Q7: How do you handle exceptions in JUnit 5?

**Answer**:
Use `assertThrows()` method (replaces JUnit 4's `@Test(expected=...)`).

**Syntax**:
```java
assertThrows(ExceptionClass.class, () -> {
    // Code that should throw exception
});
```

**Practical Examples**:
```java
@Test
void testDivisionByZero() {
    assertThrows(ArithmeticException.class, () -> {
        int result = 10 / 0;
    });
}

@Test
void testExceptionMessage() {
    Exception exception = assertThrows(IllegalArgumentException.class, () -> {
        throw new IllegalArgumentException("Invalid input");
    });

    assertEquals("Invalid input", exception.getMessage());
    assertTrue(exception.getMessage().contains("Invalid"));
}

@Test
void testMultipleExceptionScenarios() {
    // Scenario 1
    assertThrows(NullPointerException.class, () -> {
        String str = null;
        str.length();
    });

    // Scenario 2
    assertThrows(NumberFormatException.class, () -> {
        Integer.parseInt("abc");
    });
}
```

**JUnit 4 vs JUnit 5**:
```java
// JUnit 4
@Test(expected = ArithmeticException.class)
public void test() {
    int result = 10 / 0;
}

// JUnit 5 - more control
@Test
void test() {
    Exception e = assertThrows(ArithmeticException.class, () -> {
        int result = 10 / 0;
    });
    // Can now verify exception details
}
```

**In-depth Explanation**:
`assertThrows()` provides better control - you can verify exception type, message, cause, and continue test execution after catching exception.

---

### Q8: What are assumptions in JUnit 5?

**Answer**:
Assumptions allow conditional test execution. If assumption fails, test is **skipped** (not failed).

**Common Assumptions**:
```java
import static org.junit.jupiter.api.Assumptions.*;

assumeTrue(condition);       // Skip test if condition is false
assumeFalse(condition);      // Skip test if condition is true
assumingThat(condition, executable);  // Execute only if condition true
```

**Practical Examples**:
```java
@Test
void testOnlyOnCI() {
    // Test runs only if ENV=CI
    assumeTrue("CI".equals(System.getenv("ENV")));

    // Rest of test logic
    System.out.println("Running on CI");
}

@Test
void testNotOnProduction() {
    // Test skipped if env=production
    assumeFalse("production".equals(System.getProperty("env")));

    // Test logic
}

@Test
void testConditionalLogic() {
    String os = System.getProperty("os.name");

    assumingThat(os.contains("Windows"), () -> {
        // Runs only on Windows
        System.out.println("Windows-specific test");
    });

    // This always runs
    assertEquals(4, 2 + 2);
}
```

**Assumptions vs Assertions**:
- **Assertion fails** → Test **fails** ❌
- **Assumption fails** → Test **skipped** ⊘

**Use Cases**:
- Environment-specific tests
- Tests requiring specific resources
- OS-specific tests
- Feature toggle testing

---

### Q9: What is @Disabled in JUnit 5?

**Answer**:
@Disabled skips test execution (replacement for JUnit 4's @Ignore).

**Usage**:
```java
@Disabled("Feature not implemented yet")
@Test
void testNewFeature() {
    // Test is skipped
}

@Disabled
@Test
void anotherSkippedTest() {
    // Test is skipped
}
```

**Class Level**:
```java
@Disabled("Entire class disabled")
class DisabledTestClass {
    // All tests in this class are skipped
}
```

**Conditional Disabling**:
Use assumptions for conditional skipping:
```java
@Test
void conditionalTest() {
    assumeTrue("DEV".equals(System.getenv("ENV")));
    // Test runs only on DEV, skipped otherwise
}
```

**Best Practices**:
- Always provide reason in @Disabled
- Avoid disabling tests permanently
- Use assumptions for environment-specific skipping
- Review disabled tests regularly
- Use @Disabled temporarily during development

**In-depth Explanation**:
@Disabled is useful during development or when features are not ready, but disabled tests should be temporary and reviewed regularly to ensure they get re-enabled.

---

### Q10: How do you run JUnit 5 tests with Maven?

**Answer**:

**1. Add Dependencies**:
```xml
<dependencies>
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter</artifactId>
        <version>5.10.1</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

**2. Configure Surefire Plugin**:
```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>2.22.2</version>
        </plugin>
    </plugins>
</build>
```

**3. Run Tests**:
```bash
mvn test                    # Run all tests
mvn test -Dtest=LoginTest   # Run specific class
mvn test -Dgroups=smoke     # Run specific tags
```

**4. Tag-Based Execution**:
```xml
<plugin>
    <artifactId>maven-surefire-plugin</artifactId>
    <configuration>
        <groups>smoke</groups>  <!-- Run only smoke tests -->
    </configuration>
</plugin>
```

**5. Test Reports**:
After running, check:
- Console output
- `target/surefire-reports/` (XML and TXT reports)

---

## Section 2: Parameterized Tests (Questions 11-15)

### Q11: What are parameterized tests in JUnit 5?

**Answer**:
Parameterized tests run same test logic with different input values using @ParameterizedTest.

**Basic Example**:
```java
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.ValueSource;

@ParameterizedTest
@ValueSource(strings = {"hello", "world", "junit"})
void testWithStrings(String word) {
    assertNotNull(word);
    assertTrue(word.length() > 3);
}
```

**Why Use**:
- Avoid code duplication
- Test multiple scenarios efficiently
- Data-driven testing
- Better test coverage with less code

**Available Providers**:
- @ValueSource - Simple values
- @CsvSource - CSV format data
- @CsvFileSource - Data from CSV file
- @MethodSource - Data from method
- @EnumSource - Enum values
- @ArgumentsSource - Custom provider

**In-depth Explanation**:
JUnit 5's parameterized tests are more flexible than JUnit 4, supporting multiple data sources and eliminating need for @RunWith(Parameterized.class).

---

### Q12: Explain @ValueSource with examples.

**Answer**:
@ValueSource provides simple array of values (strings, ints, longs, doubles).

**Syntax**:
```java
@ParameterizedTest
@ValueSource(type = {values})
void testMethod(type parameter) { }
```

**Examples**:
```java
// Strings
@ParameterizedTest
@ValueSource(strings = {"apple", "banana", "cherry"})
void testFruits(String fruit) {
    assertNotNull(fruit);
}

// Integers
@ParameterizedTest
@ValueSource(ints = {1, 2, 3, 5, 8, 13})
void testPositiveNumbers(int number) {
    assertTrue(number > 0);
}

// Longs
@ParameterizedTest
@ValueSource(longs = {100L, 200L, 300L})
void testLongs(long value) {
    assertTrue(value >= 100);
}

// Doubles
@ParameterizedTest
@ValueSource(doubles = {1.5, 2.5, 3.5})
void testDoubles(double value) {
    assertTrue(value > 1.0);
}
```

**Limitations**:
- Single parameter only
- Simple types only (no objects)
- Use @CsvSource or @MethodSource for multiple parameters

---

### Q13: How does @CsvSource work?

**Answer**:
@CsvSource provides CSV-formatted data, supporting multiple parameters per iteration.

**Syntax**:
```java
@ParameterizedTest
@CsvSource({
    "param1, param2, param3",
    "value1, value2, value3"
})
void testMethod(Type1 p1, Type2 p2, Type3 p3) { }
```

**Practical Examples**:
```java
// Two parameters
@ParameterizedTest
@CsvSource({
    "apple, 1",
    "banana, 2",
    "cherry, 3"
})
void testWithTwoParams(String fruit, int rank) {
    assertNotNull(fruit);
    assertTrue(rank > 0);
}

// Three parameters - Calculator test
@ParameterizedTest
@CsvSource({
    "2, 3, 5",
    "10, 5, 15",
    "7, 8, 15"
})
void testAddition(int a, int b, int expected) {
    assertEquals(expected, a + b);
}

// Mixed types
@ParameterizedTest
@CsvSource({
    "John, 30, true",
    "Jane, 25, true",
    "Bob, 17, false"
})
void testUser(String name, int age, boolean isAdult) {
    assertEquals(isAdult, age >= 18);
}

// Custom delimiter
@ParameterizedTest
@CsvSource(value = {
    "apple:1",
    "banana:2"
}, delimiter = ':')
void testWithCustomDelimiter(String fruit, int rank) {
    assertNotNull(fruit);
}
```

**Benefits**:
- Multiple parameters
- Different data types
- Readable inline data
- Custom delimiters

---

### Q14: What is @MethodSource?

**Answer**:
@MethodSource provides test data from a static method, allowing complex data structures.

**Basic Example**:
```java
@ParameterizedTest
@MethodSource("provideStrings")
void testWithMethodSource(String word) {
    assertNotNull(word);
}

static Stream<String> provideStrings() {
    return Stream.of("hello", "world", "junit");
}
```

**Multiple Parameters**:
```java
@ParameterizedTest
@MethodSource("provideLoginData")
void testLogin(String username, String password, boolean shouldPass) {
    System.out.println("Testing: " + username);
}

static Stream<Arguments> provideLoginData() {
    return Stream.of(
        Arguments.of("user1", "pass1", true),
        Arguments.of("user2", "pass2", true),
        Arguments.of("invalid", "wrong", false)
    );
}
```

**Complex Objects**:
```java
@ParameterizedTest
@MethodSource("provideUsers")
void testUsers(User user) {
    assertNotNull(user.getName());
    assertTrue(user.getAge() > 0);
}

static Stream<User> provideUsers() {
    return Stream.of(
        new User("John", 30),
        new User("Jane", 25),
        new User("Bob", 35)
    );
}
```

**From File/Database**:
```java
static Stream<Arguments> provideDataFromFile() throws IOException {
    return Files.lines(Paths.get("testdata.txt"))
                .map(line -> line.split(","))
                .map(data -> Arguments.of(data[0], data[1]));
}
```

**Benefits**:
- Complex data structures
- Data from external sources
- Reusable across tests
- Type-safe

---

### Q15: What is @CsvFileSource?

**Answer**:
@CsvFileSource reads test data from CSV file.

**CSV File** (`testdata.csv`):
```csv
John,30
Jane,25
Bob,35
Alice,28
```

**Test Class**:
```java
@ParameterizedTest
@CsvFileSource(resources = "/testdata.csv")
void testWithCsvFile(String name, int age) {
    assertNotNull(name);
    assertTrue(age > 0);
    System.out.println("Name: " + name + ", Age: " + age);
}
```

**With Headers**:
```csv
name,age,city
John,30,NYC
Jane,25,LA
```

```java
@ParameterizedTest
@CsvFileSource(resources = "/testdata.csv", numLinesToSkip = 1)
void testWithHeaders(String name, int age, String city) {
    assertNotNull(name);
    assertNotNull(city);
}
```

**Multiple Files**:
```java
@ParameterizedTest
@CsvFileSource(resources = {"/data1.csv", "/data2.csv"})
void testMultipleFiles(String param1, String param2) {
    // Test logic
}
```

**Benefits**:
- Separate test data from code
- Easy to maintain large datasets
- Non-developers can update data
- Good for data-driven testing

---

## Section 3: Advanced Features (Questions 16-20)

### Q16: What are nested tests in JUnit 5?

**Answer**:
@Nested allows hierarchical test organization within a test class.

**Example**:
```java
@DisplayName("Calculator Tests")
class CalculatorTest {

    Calculator calculator;

    @BeforeEach
    void setup() {
        calculator = new Calculator();
    }

    @Nested
    @DisplayName("Addition Tests")
    class AdditionTests {

        @Test
        @DisplayName("Add two positive numbers")
        void testPositive() {
            assertEquals(5, calculator.add(2, 3));
        }

        @Test
        @DisplayName("Add negative numbers")
        void testNegative() {
            assertEquals(-5, calculator.add(-2, -3));
        }
    }

    @Nested
    @DisplayName("Division Tests")
    class DivisionTests {

        @Test
        void testDivision() {
            assertEquals(2, calculator.divide(10, 5));
        }

        @Test
        void testDivisionByZero() {
            assertThrows(ArithmeticException.class, () -> {
                calculator.divide(10, 0);
            });
        }
    }
}
```

**Benefits**:
- Better test organization
- Logical grouping
- Shared setup for related tests
- Improved test reports
- Better documentation

**Test Report Structure**:
```
Calculator Tests
├── Addition Tests
│   ├── Add two positive numbers ✓
│   └── Add negative numbers ✓
└── Division Tests
    ├── testDivision ✓
    └── testDivisionByZero ✓
```

---

### Q17: What is @RepeatedTest?

**Answer**:
@RepeatedTest runs same test multiple times.

**Basic Usage**:
```java
@RepeatedTest(5)
void repeatedTest() {
    assertTrue(true);
    // Runs 5 times
}
```

**With RepetitionInfo**:
```java
@RepeatedTest(value = 3, name = "{displayName} - repetition {currentRepetition} of {totalRepetitions}")
@DisplayName("Custom repeated test")
void customRepeatedTest(RepetitionInfo repetitionInfo) {
    int current = repetitionInfo.getCurrentRepetition();
    int total = repetitionInfo.getTotalRepetitions();
    System.out.println("Repetition " + current + " of " + total);
}
```

**Use Cases**:
- Flaky test verification
- Performance testing
- Random data testing
- Concurrency testing

**Output**:
```
Custom repeated test - repetition 1 of 3 ✓
Custom repeated test - repetition 2 of 3 ✓
Custom repeated test - repetition 3 of 3 ✓
```

---

### Q18: What is @TempDir in JUnit 5?

**Answer**:
@TempDir creates temporary directory for test, automatically cleaned up after test.

**Usage**:
```java
import org.junit.jupiter.api.io.TempDir;
import java.nio.file.*;

@Test
void testWithTempDir(@TempDir Path tempDir) throws IOException {
    // Create file in temp directory
    Path file = tempDir.resolve("test.txt");
    Files.writeString(file, "Hello JUnit 5");

    // Verify file exists
    assertTrue(Files.exists(file));

    // Read and verify content
    String content = Files.readString(file);
    assertEquals("Hello JUnit 5", content);

    // Temp dir automatically deleted after test
}
```

**Field Injection**:
```java
class FileTest {

    @TempDir
    Path tempDir;

    @Test
    void test1() throws IOException {
        Path file = tempDir.resolve("file1.txt");
        Files.writeString(file, "Content");
        assertTrue(Files.exists(file));
    }

    @Test
    void test2() throws IOException {
        // New temp dir for this test
        Path file = tempDir.resolve("file2.txt");
        Files.writeString(file, "Content");
        assertTrue(Files.exists(file));
    }
}
```

**Use Cases**:
- File I/O testing
- Testing file uploads
- Database file testing
- Log file testing

**Benefits**:
- Automatic cleanup
- Isolated test environment
- No manual directory management
- Safe parallel execution

---

### Q19: What is @Tag in JUnit 5?

**Answer**:
@Tag marks tests for filtering/grouping (similar to TestNG groups).

**Usage**:
```java
@Tag("smoke")
@Test
void smokeTest() {
    // Critical test
}

@Tag("regression")
@Test
void regressionTest() {
    // Full regression test
}

@Tag("slow")
@Test
void slowTest() {
    // Long-running test
}
```

**Multiple Tags**:
```java
@Tag("smoke")
@Tag("fast")
@Test
void fastSmokeTest() {
    // Belongs to both groups
}
```

**Class Level**:
```java
@Tag("integration")
class IntegrationTests {
    // All tests tagged as integration
}
```

**Maven Configuration**:
```xml
<plugin>
    <artifactId>maven-surefire-plugin</artifactId>
    <configuration>
        <groups>smoke</groups>  <!-- Run only smoke tests -->
    </configuration>
</plugin>
```

**Run Specific Tags**:
```bash
mvn test -Dgroups=smoke
mvn test -Dgroups="smoke | regression"
mvn test -DexcludedGroups=slow
```

**Use Cases**:
- Smoke tests before deployment
- Regression tests after release
- Fast vs slow tests
- Environment-specific tests

---

### Q20: How do you integrate JUnit 5 with Selenium?

**Answer**:

**Setup**:
```xml
<dependencies>
    <!-- JUnit 5 -->
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter</artifactId>
        <version>5.10.1</version>
    </dependency>

    <!-- Selenium -->
    <dependency>
        <groupId>org.seleniumhq.selenium</groupId>
        <artifactId>selenium-java</artifactId>
        <version>4.16.0</version>
    </dependency>

    <!-- WebDriverManager -->
    <dependency>
        <groupId>io.github.bonigarcia</groupId>
        <artifactId>webdrivermanager</artifactId>
        <version>5.6.2</version>
    </dependency>
</dependencies>
```

**Test Class**:
```java
import org.junit.jupiter.api.*;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import io.github.bonigarcia.wdm.WebDriverManager;
import static org.junit.jupiter.api.Assertions.*;

class SeleniumJUnit5Test {

    WebDriver driver;

    @BeforeAll
    static void setupDriver() {
        WebDriverManager.chromedriver().setup();
    }

    @BeforeEach
    void setup() {
        driver = new ChromeDriver();
        driver.manage().window().maximize();
    }

    @Test
    @DisplayName("Test Google homepage title")
    void testGoogleTitle() {
        driver.get("https://www.google.com");
        String title = driver.getTitle();
        assertTrue(title.contains("Google"));
    }

    @AfterEach
    void teardown() {
        if (driver != null) {
            driver.quit();
        }
    }
}
```

**Benefits**:
- Clean lifecycle management
- Good assertions
- Parameterized tests for cross-browser
- Nested tests for organization
- Tags for test filtering

---

## Section 4: JUnit 5 vs TestNG (Questions 21-25)

### Q21: When should you use JUnit 5 vs TestNG?

**Answer**:

**Use JUnit 5 When**:
- Unit testing focus
- Modern Java projects (Java 8+)
- Simple test scenarios
- Integration with Spring Boot
- Standard Java testing
- Smaller learning curve

**Use TestNG When**:
- Complex test scenarios
- Parallel execution critical
- Test dependencies needed
- Selenium/UI automation
- Flexible test configuration (testng.xml)
- DataProvider for complex data
- More mature framework needed

**Feature Comparison**:

| Feature | JUnit 5 | TestNG |
|---------|---------|--------|
| Learning Curve | Easier | Steeper |
| Parallel Execution | Limited | Built-in, flexible |
| Test Dependencies | No | Yes |
| Configuration | junit-platform.properties | testng.xml (powerful) |
| DataProvider | Multiple annotations | @DataProvider (powerful) |
| Grouping | @Tag | @Test(groups) |
| Suite Management | Limited | Excellent |
| Reports | Basic | Built-in HTML |
| Community | Growing | Mature |

**Recommendation**:
- **JUnit 5**: Unit tests, Spring applications
- **TestNG**: Selenium automation, integration tests, complex scenarios

---

### Q22: What are the limitations of JUnit 5?

**Answer**:

**1. No Test Dependencies**:
```java
// TestNG can do this, JUnit 5 cannot
@Test(dependsOnMethods = {"login"})
public void checkout() { }
```

**2. Limited Parallel Execution**:
- JUnit 5 has parallel support but less flexible than TestNG
- TestNG: parallel methods, classes, tests, suites
- JUnit 5: Limited parallel support

**3. No Built-in HTML Reports**:
- TestNG generates HTML reports automatically
- JUnit 5 needs third-party tools (Allure, ExtentReports)

**4. Less Flexible Configuration**:
- TestNG has powerful testng.xml for suite management
- JUnit 5 has limited configuration options

**5. No Group Dependencies**:
```java
// TestNG supports this
@Test(dependsOnGroups = {"init"})
```

**6. DataProvider Less Powerful**:
- TestNG's @DataProvider is more flexible
- JUnit 5 requires multiple annotations for different sources

**Workarounds**:
- Use assumptions for conditional execution
- Use @Order for execution order (not dependency)
- Use third-party tools for reporting
- Use extensions for custom behavior

---

### Q23: What are best practices for JUnit 5?

**Answer**:

**1. Use Descriptive Names**:
```java
@Test
@DisplayName("Login should fail with invalid credentials")
void testInvalidLogin() { }
```

**2. One Assertion Per Test** (when possible):
```java
// Good - focused
@Test
void testUserAge() {
    assertEquals(30, user.getAge());
}

// Better than testing everything in one test
```

**3. Use assertAll for Related Assertions**:
```java
@Test
void testUser() {
    assertAll(
        () -> assertEquals("John", user.getFirstName()),
        () -> assertEquals("Doe", user.getLastName())
    );
}
```

**4. Use @BeforeEach for Test Isolation**:
```java
@BeforeEach
void setup() {
    // Fresh state for each test
    user = new User();
}
```

**5. Test Independence**:
- Each test should run standalone
- Don't rely on test execution order
- Use @Order only when absolutely necessary

**6. Meaningful Failure Messages**:
```java
assertEquals(expected, actual, "User age should be 30");
```

**7. Use Parameterized Tests**:
```java
@ParameterizedTest
@ValueSource(strings = {"user1", "user2"})
void testMultipleUsers(String username) { }
```

**8. Organize with @Nested**:
```java
@Nested
class LoginTests {
    // Related tests grouped
}
```

**9. Clean Up Resources**:
```java
@AfterEach
void teardown() {
    if (driver != null) {
        driver.quit();
    }
}
```

**10. Use @Tag for Organization**:
```java
@Tag("smoke")
@Tag("fast")
@Test
void criticalTest() { }
```

---

### Q24: How do you migrate from JUnit 4 to JUnit 5?

**Answer**:

**Step-by-Step Migration**:

**1. Update Dependencies**:
```xml
<!-- Remove JUnit 4 -->
<!-- <dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.13.2</version>
</dependency> -->

<!-- Add JUnit 5 -->
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter</artifactId>
    <version>5.10.1</version>
</dependency>
```

**2. Update Imports**:
```java
// JUnit 4
import org.junit.Test;
import org.junit.Before;
import org.junit.After;
import org.junit.Assert;

// JUnit 5
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.AfterEach;
import static org.junit.jupiter.api.Assertions.*;
```

**3. Update Annotations**:
```java
// JUnit 4 → JUnit 5
@Before → @BeforeEach
@After → @AfterEach
@BeforeClass → @BeforeAll (must be static)
@AfterClass → @AfterAll (must be static)
@Ignore → @Disabled
```

**4. Update Assertions**:
```java
// JUnit 4
Assert.assertEquals(expected, actual);

// JUnit 5
assertEquals(expected, actual);
```

**5. Update Exception Testing**:
```java
// JUnit 4
@Test(expected = Exception.class)
public void test() { }

// JUnit 5
@Test
void test() {
    assertThrows(Exception.class, () -> { });
}
```

**6. Remove public modifiers** (tests can be package-private in JUnit 5)

**7. Run both versions during migration** (use JUnit Vintage):
```xml
<dependency>
    <groupId>org.junit.vintage</groupId>
    <artifactId>junit-vintage-engine</artifactId>
</dependency>
```

---

### Q25: What are JUnit 5 extensions?

**Answer**:
Extensions add custom behavior to tests (replacement for JUnit 4 Rules and Runners).

**Extension Types**:
1. BeforeAllCallback / AfterAllCallback
2. BeforeEachCallback / AfterEachCallback
3. BeforeTestExecutionCallback / AfterTestExecutionCallback
4. TestInstancePostProcessor
5. ParameterResolver

**Simple Extension Example**:
```java
import org.junit.jupiter.api.extension.*;

public class TimingExtension implements BeforeTestExecutionCallback,
                                       AfterTestExecutionCallback {

    @Override
    public void beforeTestExecution(ExtensionContext context) {
        long startTime = System.currentTimeMillis();
        context.getStore(Namespace.GLOBAL).put("startTime", startTime);
    }

    @Override
    public void afterTestExecution(ExtensionContext context) {
        long startTime = context.getStore(Namespace.GLOBAL).get("startTime", long.class);
        long duration = System.currentTimeMillis() - startTime;
        System.out.println("Test took " + duration + " ms");
    }
}
```

**Using Extension**:
```java
@ExtendWith(TimingExtension.class)
class MyTest {
    @Test
    void test() {
        // Test automatically timed
    }
}
```

**Built-in Extensions**:
- @TempDir - Temporary directory
- @Timeout - Test timeout
- RandomParametersExtension
- SpringExtension (Spring Boot)

**Benefits**:
- More flexible than Rules/Runners
- Composable (multiple extensions)
- Type-safe
- Modern API

---

## Summary: Key Interview Points

### Must-Know Concepts:
1. **JUnit 5 Architecture**: Platform, Jupiter, Vintage
2. **Annotations**: @Test, @BeforeEach, @AfterEach, @BeforeAll, @AfterAll
3. **Assertions**: assertEquals, assertThrows, assertAll, assertTimeout
4. **Parameterized Tests**: @ValueSource, @CsvSource, @MethodSource
5. **Advanced Features**: @Nested, @DisplayName, @Tag, @Disabled
6. **JUnit 4 vs JUnit 5**: Key differences
7. **JUnit 5 vs TestNG**: When to use each
8. **Best Practices**: Test independence, meaningful names, cleanup

### Interview Tips:
- Explain JUnit 5's modular architecture
- Show understanding of migration from JUnit 4
- Mention advantages over JUnit 4 (assertAll, assertThrows, parameterized)
- Know when to use JUnit 5 vs TestNG
- Provide practical code examples
- Mention experience with Selenium integration

Good luck with your JUnit interviews!
