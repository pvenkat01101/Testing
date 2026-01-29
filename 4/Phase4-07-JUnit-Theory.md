# Phase 4.3 - JUnit Framework: Comprehensive Theory

## Part 1: Introduction to JUnit

### What is JUnit?

JUnit is a popular unit testing framework for Java. It's widely used for test-driven development (TDD) and provides annotations to identify test methods and assertions to verify expected results.

### Brief History

- **JUnit 3**: Old version, tests extended TestCase class
- **JUnit 4**: Introduced annotations (@Test), released 2006
- **JUnit 5**: Complete rewrite, modular architecture, released 2017

**Current Standard**: JUnit 5 (also called JUnit Jupiter)

### Why JUnit 5?

**Advantages over JUnit 4**:
- Modular architecture (Platform, Jupiter, Vintage)
- Better support for Java 8+ features (lambdas, streams)
- Multiple extension points
- Improved parameterized tests
- Better assertions (assertAll)
- Nested tests
- Dynamic tests
- Conditional test execution

### JUnit 5 vs TestNG

| Feature | JUnit 5 | TestNG |
|---------|---------|--------|
| Architecture | Modular (3 modules) | Monolithic |
| Annotations | @Test, @BeforeEach | @Test, @BeforeMethod |
| Parameterization | Multiple annotations | @DataProvider |
| Grouping | @Tag | @Test(groups) |
| Parallel execution | Limited | Built-in |
| Dependency testing | No | Yes |
| Test configuration | junit-platform.properties | testng.xml |
| Maturity | Newer (2017) | Older (2004) |

**When to use**:
- **JUnit 5**: Unit testing, modern Java projects, simpler setup
- **TestNG**: Complex test scenarios, parallel execution, Selenium automation

---

## Part 2: JUnit 5 Architecture

### Three Main Modules

```
JUnit 5
  ├── JUnit Platform (foundation for launching tests)
  ├── JUnit Jupiter (new programming model and extension model)
  └── JUnit Vintage (backward compatibility with JUnit 3/4)
```

**JUnit Platform**:
- Foundation for launching testing frameworks on JVM
- Defines TestEngine API
- Console Launcher
- Integration with build tools (Maven, Gradle)

**JUnit Jupiter**:
- New programming and extension model
- New annotations (@Test, @BeforeEach, etc.)
- New assertions and assumptions
- @ParameterizedTest
- Extension API

**JUnit Vintage**:
- Runs JUnit 3 and JUnit 4 tests
- Backward compatibility
- Helps migration

---

## Part 3: Setup JUnit 5

### Maven Dependency

```xml
<dependencies>
    <!-- JUnit 5 (Jupiter) -->
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter</artifactId>
        <version>5.10.1</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

**Note**: `junit-jupiter` is an aggregator that includes:
- `junit-jupiter-api` (write tests)
- `junit-jupiter-engine` (run tests)
- `junit-jupiter-params` (parameterized tests)

### Maven Surefire Plugin

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

### First JUnit 5 Test

```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class FirstTest {

    @Test
    void testAddition() {
        int result = 2 + 3;
        assertEquals(5, result);
        System.out.println("Test passed!");
    }
}
```

**Run**: `mvn test`

---

## Part 4: JUnit 5 Annotations

### Core Annotations

#### @Test

Marks a method as a test method.

```java
@Test
void myTest() {
    // Test logic
}
```

**JUnit 4 vs JUnit 5**:
```java
// JUnit 4
@Test(expected = Exception.class, timeout = 1000)
public void test() { }

// JUnit 5
@Test
void test() {
    assertThrows(Exception.class, () -> {
        // Code that should throw
    });
}

@Test
@Timeout(value = 1, unit = TimeUnit.SECONDS)
void test() { }
```

#### @BeforeEach and @AfterEach

Run before/after each test method.

```java
import org.junit.jupiter.api.*;

class LifecycleTest {

    @BeforeEach
    void setup() {
        System.out.println("Setup before each test");
    }

    @Test
    void test1() {
        System.out.println("Test 1");
    }

    @Test
    void test2() {
        System.out.println("Test 2");
    }

    @AfterEach
    void teardown() {
        System.out.println("Cleanup after each test\n");
    }
}

/* Output:
Setup before each test
Test 1
Cleanup after each test

Setup before each test
Test 2
Cleanup after each test
*/
```

**JUnit 4 equivalent**: @Before, @After

#### @BeforeAll and @AfterAll

Run once before/after all tests in class.

```java
class SetupTest {

    @BeforeAll
    static void setupAll() {
        System.out.println("Setup once for all tests\n");
    }

    @Test
    void test1() {
        System.out.println("Test 1");
    }

    @Test
    void test2() {
        System.out.println("Test 2");
    }

    @AfterAll
    static void teardownAll() {
        System.out.println("\nCleanup once for all tests");
    }
}

/* Output:
Setup once for all tests

Test 1
Test 2

Cleanup once for all tests
*/
```

**Important**: Must be static (unless using @TestInstance(Lifecycle.PER_CLASS))

**JUnit 4 equivalent**: @BeforeClass, @AfterClass

#### @DisplayName

Provides custom display name for tests.

```java
@DisplayName("Calculator Tests")
class CalculatorTest {

    @Test
    @DisplayName("Test addition of two positive numbers")
    void testAddition() {
        assertEquals(5, 2 + 3);
    }

    @Test
    @DisplayName("Test division by zero throws exception")
    void testDivisionByZero() {
        assertThrows(ArithmeticException.class, () -> {
            int result = 10 / 0;
        });
    }
}
```

Improves readability in test reports.

#### @Disabled

Disables a test or test class.

```java
@Disabled("Feature not implemented yet")
@Test
void testFeature() {
    // Will not execute
}

@Disabled
class EntireTestClass {
    // All tests skipped
}
```

**JUnit 4 equivalent**: @Ignore

#### @Tag

Tags tests for filtering/grouping.

```java
@Tag("smoke")
@Test
void smokeTest() {
    // Critical test
}

@Tag("slow")
@Test
void slowTest() {
    // Long-running test
}
```

**Run specific tags**:
```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <configuration>
        <groups>smoke</groups>  <!-- Only smoke tests -->
    </configuration>
</plugin>
```

---

## Part 5: Assertions

### Basic Assertions

```java
import static org.junit.jupiter.api.Assertions.*;

@Test
void assertionsTest() {
    // Equality
    assertEquals(5, 2 + 3);
    assertEquals(5, 2 + 3, "2 + 3 should equal 5");

    // Boolean
    assertTrue(5 > 3);
    assertFalse(5 < 3);

    // Null checks
    assertNull(null);
    assertNotNull("text");

    // Same reference
    String str = "hello";
    assertSame(str, str);
    assertNotSame("hello", new String("hello"));

    // Array equality
    int[] expected = {1, 2, 3};
    int[] actual = {1, 2, 3};
    assertArrayEquals(expected, actual);

    // Fail explicitly
    fail("Test failed intentionally");
}
```

### assertThrows (Exception Testing)

```java
@Test
void testException() {
    // Verify exception is thrown
    assertThrows(ArithmeticException.class, () -> {
        int result = 10 / 0;
    });

    // Capture exception for further validation
    Exception exception = assertThrows(IllegalArgumentException.class, () -> {
        throw new IllegalArgumentException("Invalid input");
    });

    assertEquals("Invalid input", exception.getMessage());
}
```

### assertAll (Grouped Assertions)

Collects multiple assertions and reports all failures.

```java
@Test
void testUser() {
    User user = new User("John", "Doe", 30);

    // All assertions execute, all failures reported
    assertAll("user properties",
        () -> assertEquals("John", user.getFirstName()),
        () -> assertEquals("Doe", user.getLastName()),
        () -> assertEquals(30, user.getAge()),
        () -> assertNotNull(user.getEmail())  // If fails, other assertions still run
    );
}
```

**vs Regular Assertions**:
```java
// Regular - stops at first failure
assertEquals("John", user.getFirstName());
assertEquals("Doe", user.getLastName());  // Never reaches if first fails
```

### assertTimeout

Verify execution completes within timeout.

```java
@Test
void testTimeout() {
    assertTimeout(Duration.ofSeconds(2), () -> {
        // Code that should complete within 2 seconds
        Thread.sleep(1000);  // Passes
    });

    assertTimeout(Duration.ofMillis(100), () -> {
        Thread.sleep(200);  // Fails - timeout exceeded
    });
}
```

---

## Part 6: Assumptions

Assumptions allow conditional test execution. If assumption fails, test is skipped (not failed).

```java
import static org.junit.jupiter.api.Assumptions.*;

@Test
void testOnlyOnCI() {
    assumeTrue("CI".equals(System.getenv("ENV")));
    // Rest of test only runs if ENV=CI
    // Otherwise test is skipped
}

@Test
void testNotOnProduction() {
    assumeFalse("production".equals(System.getProperty("env")));
    // Test skipped if env=production
}

@Test
void testAssumingThat() {
    assumingThat("DEV".equals(System.getenv("ENV")),
        () -> {
            // Runs only on DEV environment
            System.out.println("Running on DEV");
        }
    );

    // This always runs
    assertEquals(4, 2 + 2);
}
```

**Use Cases**:
- Environment-specific tests
- Tests requiring specific system properties
- Tests needing external resources

---

## Part 7: Parameterized Tests

### @ParameterizedTest

Run same test with different parameters.

#### @ValueSource

Simple values (String, int, long, double).

```java
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.ValueSource;

@ParameterizedTest
@ValueSource(strings = {"hello", "world", "junit"})
void testWithStrings(String word) {
    assertNotNull(word);
    assertTrue(word.length() > 3);
}

@ParameterizedTest
@ValueSource(ints = {1, 2, 3, 5, 8})
void testWithInts(int number) {
    assertTrue(number > 0);
}
```

#### @CsvSource

CSV-style parameters.

```java
@ParameterizedTest
@CsvSource({
    "apple, 1",
    "banana, 2",
    "cherry, 3"
})
void testWithCsv(String fruit, int rank) {
    assertNotNull(fruit);
    assertTrue(rank > 0);
}

@ParameterizedTest
@CsvSource({
    "2, 3, 5",
    "10, 5, 15",
    "7, 8, 15"
})
void testAddition(int a, int b, int expected) {
    assertEquals(expected, a + b);
}
```

#### @CsvFileSource

Read from CSV file.

```java
// testdata.csv:
// John,30
// Jane,25
// Bob,35

@ParameterizedTest
@CsvFileSource(resources = "/testdata.csv")
void testWithCsvFile(String name, int age) {
    assertNotNull(name);
    assertTrue(age > 0);
}
```

#### @MethodSource

Complex data from method.

```java
@ParameterizedTest
@MethodSource("provideTestData")
void testWithMethodSource(String username, String password, boolean valid) {
    // Test logic
    System.out.println("Testing: " + username);
}

static Stream<Arguments> provideTestData() {
    return Stream.of(
        Arguments.of("user1", "pass1", true),
        Arguments.of("user2", "pass2", true),
        Arguments.of("invalid", "wrong", false)
    );
}
```

#### @EnumSource

Test with enum values.

```java
enum Day {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
}

@ParameterizedTest
@EnumSource(Day.class)
void testWithEnum(Day day) {
    assertNotNull(day);
}

@ParameterizedTest
@EnumSource(value = Day.class, names = {"SATURDAY", "SUNDAY"})
void testWeekends(Day day) {
    assertTrue(day.toString().endsWith("DAY"));
}
```

---

## Part 8: Nested Tests

Organize related tests hierarchically.

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
        void testPositiveAddition() {
            assertEquals(5, calculator.add(2, 3));
        }

        @Test
        @DisplayName("Add negative numbers")
        void testNegativeAddition() {
            assertEquals(-5, calculator.add(-2, -3));
        }
    }

    @Nested
    @DisplayName("Division Tests")
    class DivisionTests {

        @Test
        @DisplayName("Divide two numbers")
        void testDivision() {
            assertEquals(2, calculator.divide(10, 5));
        }

        @Test
        @DisplayName("Division by zero throws exception")
        void testDivisionByZero() {
            assertThrows(ArithmeticException.class, () -> {
                calculator.divide(10, 0);
            });
        }
    }
}
```

**Benefits**:
- Better organization
- Shared setup for nested tests
- Improved reporting structure

---

## Part 9: Repeated Tests

Run test multiple times.

```java
@RepeatedTest(5)
void repeatedTest() {
    System.out.println("Test executed");
    // Runs 5 times
}

@RepeatedTest(value = 3, name = "{displayName} - repetition {currentRepetition} of {totalRepetitions}")
@DisplayName("Custom repeated test")
void customRepeatedTest(RepetitionInfo repetitionInfo) {
    System.out.println("Repetition " + repetitionInfo.getCurrentRepetition());
}
```

---

## Part 10: Test Lifecycle

### Test Instance Lifecycle

**Default (PER_METHOD)**:
- New test instance created for each @Test method
- @BeforeAll/@AfterAll must be static

**PER_CLASS**:
- Single instance for all tests in class
- @BeforeAll/@AfterAll can be non-static

```java
@TestInstance(TestInstance.Lifecycle.PER_CLASS)
class LifecycleTest {

    @BeforeAll
    void setup() {  // No need for static
        System.out.println("Setup once");
    }

    @Test
    void test1() { }

    @Test
    void test2() { }
}
```

### Complete Lifecycle Example

```java
class CompleteLifecycleTest {

    @BeforeAll
    static void beforeAll() {
        System.out.println("1. Before All");
    }

    @BeforeEach
    void beforeEach() {
        System.out.println("  2. Before Each");
    }

    @Test
    void test1() {
        System.out.println("    3. Test 1");
    }

    @Test
    void test2() {
        System.out.println("    3. Test 2");
    }

    @AfterEach
    void afterEach() {
        System.out.println("  4. After Each\n");
    }

    @AfterAll
    static void afterAll() {
        System.out.println("5. After All");
    }
}

/* Output:
1. Before All
  2. Before Each
    3. Test 1
  4. After Each

  2. Before Each
    3. Test 2
  4. After Each

5. After All
*/
```

---

## Part 11: Extensions

JUnit 5's extension model is more powerful than JUnit 4's runners.

### Built-in Extensions

#### @Timeout

```java
@Test
@Timeout(value = 2, unit = TimeUnit.SECONDS)
void testTimeout() {
    // Must complete within 2 seconds
}
```

#### @TempDir

Temporary directory for tests.

```java
@Test
void testWithTempDir(@TempDir Path tempDir) {
    Path file = tempDir.resolve("test.txt");
    Files.writeString(file, "Hello");

    assertTrue(Files.exists(file));
    // Cleaned up automatically after test
}
```

### Custom Extensions

Implement Extension interface.

```java
import org.junit.jupiter.api.extension.*;

public class TimingExtension implements BeforeTestExecutionCallback,
                                       AfterTestExecutionCallback {

    @Override
    public void beforeTestExecution(ExtensionContext context) {
        context.getStore(Namespace.GLOBAL).put("startTime", System.currentTimeMillis());
    }

    @Override
    public void afterTestExecution(ExtensionContext context) {
        long startTime = context.getStore(Namespace.GLOBAL).get("startTime", long.class);
        long duration = System.currentTimeMillis() - startTime;
        System.out.println("Test " + context.getDisplayName() + " took " + duration + " ms");
    }
}

// Use extension
@ExtendWith(TimingExtension.class)
class MyTest {
    @Test
    void test() {
        // Test logic
    }
}
```

---

## Part 12: Integration with Selenium

```java
import org.junit.jupiter.api.*;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import io.github.bonigarcia.wdm.WebDriverManager;

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

    @Test
    @DisplayName("Test Wikipedia homepage")
    void testWikipedia() {
        driver.get("https://www.wikipedia.org");
        String title = driver.getTitle();
        assertTrue(title.contains("Wikipedia"));
    }

    @AfterEach
    void teardown() {
        if (driver != null) {
            driver.quit();
        }
    }
}
```

---

## Part 13: JUnit 4 vs JUnit 5

| Feature | JUnit 4 | JUnit 5 |
|---------|---------|---------|
| Architecture | Monolithic | Modular (Platform, Jupiter, Vintage) |
| Java version | Java 5+ | Java 8+ |
| Annotations | @Test, @Before, @After, @BeforeClass, @AfterClass | @Test, @BeforeEach, @AfterEach, @BeforeAll, @AfterAll |
| Test class | Can be package-private | Must be package-private or public |
| Test method visibility | Must be public | Can be package-private |
| Assertions | org.junit.Assert | org.junit.jupiter.api.Assertions |
| Assumptions | org.junit.Assume | org.junit.jupiter.api.Assumptions |
| @Ignore | @Ignore | @Disabled |
| @Category | @Category | @Tag |
| Exception testing | @Test(expected=...) | assertThrows() |
| Timeout | @Test(timeout=...) | @Timeout or assertTimeout() |
| Parameterized | @RunWith(Parameterized.class) | @ParameterizedTest |
| Nested tests | No | @Nested |
| Grouped assertions | No | assertAll() |
| Extensions | @RunWith, @Rule | @ExtendWith |

---

## Part 14: Best Practices

1. **Use Descriptive Names**:
```java
@Test
@DisplayName("Login should fail with invalid credentials")
void testInvalidLogin() { }
```

2. **One Assertion Per Test** (when possible):
```java
// Good
@Test
void testAddition() {
    assertEquals(5, 2 + 3);
}

// Better than combining multiple unrelated assertions
```

3. **Use assertAll for Related Assertions**:
```java
@Test
void testUser() {
    assertAll(
        () -> assertEquals("John", user.getFirstName()),
        () -> assertEquals("Doe", user.getLastName())
    );
}
```

4. **Use @BeforeEach for Test Isolation**:
```java
@BeforeEach
void setup() {
    // Fresh state for each test
}
```

5. **Test Independence**:
- Each test should run independently
- Don't rely on test execution order

6. **Meaningful Failure Messages**:
```java
assertEquals(expected, actual, "User age should be 30");
```

7. **Use Parameterized Tests for Multiple Scenarios**:
```java
@ParameterizedTest
@ValueSource(strings = {"user1", "user2", "user3"})
void testMultipleUsers(String username) { }
```

8. **Clean Up Resources**:
```java
@AfterEach
void teardown() {
    if (driver != null) {
        driver.quit();
    }
}
```

---

## Conclusion

JUnit 5 is a powerful, modern testing framework with:
- Modular architecture
- Rich annotation set
- Flexible parameterization
- Better assertions (assertAll, assertThrows)
- Extension model
- Nested tests
- Java 8+ features support

**JUnit 5 vs TestNG**:
- JUnit 5: Better for unit tests, modern Java, simpler
- TestNG: Better for integration/Selenium tests, more features for test configuration

Choose based on project needs. Both are excellent frameworks!
