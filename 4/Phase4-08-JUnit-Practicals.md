# Phase 4.3 - JUnit Framework: Practical Exercises

## Setup Exercises

### Exercise 1: Setup JUnit 5 Project

#### Objective
Create a Maven project with JUnit 5 dependencies.

#### Step-by-step (Beginner)
1. Create Maven project:
   ```bash
   mvn archetype:generate -DgroupId=com.junit.practice -DartifactId=junit5-basics -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
   ```
2. Navigate: `cd junit5-basics`
3. Open `pom.xml`
4. Add JUnit 5 dependency:
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
5. Add Surefire plugin:
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
6. Run: `mvn clean install`

**Deliverable**: Working JUnit 5 project

---

### Exercise 2: First JUnit 5 Test

#### Objective
Write and run your first JUnit 5 test.

#### Step-by-step (Beginner)
1. Create package: `src/test/java/com/junit/practice`
2. Create class: `FirstTest.java`
3. Write code:
   ```java
   package com.junit.practice;

   import org.junit.jupiter.api.Test;
   import static org.junit.jupiter.api.Assertions.*;

   class FirstTest {

       @Test
       void testAddition() {
           int result = 2 + 3;
           assertEquals(5, result);
           System.out.println("Addition test passed!");
       }

       @Test
       void testSubtraction() {
           int result = 10 - 5;
           assertEquals(5, result);
           System.out.println("Subtraction test passed!");
       }

       @Test
       void testMultiplication() {
           int result = 4 * 3;
           assertEquals(12, result);
           System.out.println("Multiplication test passed!");
       }
   }
   ```
4. Run: `mvn test`
5. Check test results in console and `target/surefire-reports`

**Deliverable**: First successful JUnit 5 tests

---

## Annotation Exercises

### Exercise 3: @BeforeEach and @AfterEach

#### Objective
Understand method-level setup and teardown.

#### Step-by-step (Beginner)
1. Create class: `LifecycleTest.java`
2. Implement:
   ```java
   package com.junit.practice;

   import org.junit.jupiter.api.*;

   class LifecycleTest {

       @BeforeEach
       void setup() {
           System.out.println("@BeforeEach: Setup before each test");
       }

       @Test
       void test1() {
           System.out.println("Executing Test 1");
       }

       @Test
       void test2() {
           System.out.println("Executing Test 2");
       }

       @Test
       void test3() {
           System.out.println("Executing Test 3");
       }

       @AfterEach
       void teardown() {
           System.out.println("@AfterEach: Cleanup after each test\n");
       }
   }
   ```
3. Run and observe console output
4. Notice setup and teardown run for each test

**Deliverable**: Understanding of @BeforeEach and @AfterEach

---

### Exercise 4: @BeforeAll and @AfterAll

#### Objective
Understand class-level setup and teardown.

#### Step-by-step (Beginner)
1. Create class: `BeforeAfterAllTest.java`
2. Implement:
   ```java
   package com.junit.practice;

   import org.junit.jupiter.api.*;

   class BeforeAfterAllTest {

       @BeforeAll
       static void setupAll() {
           System.out.println("@BeforeAll: Setup once for all tests\n");
       }

       @Test
       void test1() {
           System.out.println("Executing Test 1");
       }

       @Test
       void test2() {
           System.out.println("Executing Test 2");
       }

       @Test
       void test3() {
           System.out.println("Executing Test 3");
       }

       @AfterAll
       static void teardownAll() {
           System.out.println("\n@AfterAll: Cleanup once for all tests");
       }
   }
   ```
3. Run and observe that setup/teardown run only once
4. Note: Methods must be static (or use @TestInstance(Lifecycle.PER_CLASS))

**Deliverable**: Understanding class-level annotations

---

### Exercise 5: @DisplayName

#### Objective
Add custom display names to tests.

#### Step-by-step (Beginner)
1. Create class: `DisplayNameTest.java`
2. Implement:
   ```java
   package com.junit.practice;

   import org.junit.jupiter.api.*;
   import static org.junit.jupiter.api.Assertions.*;

   @DisplayName("Calculator Tests Suite")
   class DisplayNameTest {

       @Test
       @DisplayName("Test addition of two positive numbers")
       void testAddition() {
           assertEquals(5, 2 + 3);
       }

       @Test
       @DisplayName("Test subtraction with positive and negative numbers")
       void testSubtraction() {
           assertEquals(-2, 5 - 7);
       }

       @Test
       @DisplayName("Test multiplication by zero returns zero")
       void testMultiplicationByZero() {
           assertEquals(0, 5 * 0);
       }

       @Test
       @DisplayName("Test division by zero throws ArithmeticException")
       void testDivisionByZero() {
           assertThrows(ArithmeticException.class, () -> {
               int result = 10 / 0;
           });
       }
   }
   ```
3. Run and check test report for readable test names

**Deliverable**: Tests with descriptive display names

---

### Exercise 6: @Disabled

#### Objective
Skip tests temporarily.

#### Step-by-step (Beginner)
1. Create class: `DisabledTest.java`
2. Implement:
   ```java
   package com.junit.practice;

   import org.junit.jupiter.api.*;
   import static org.junit.jupiter.api.Assertions.*;

   class DisabledTest {

       @Test
       void enabledTest() {
           System.out.println("This test runs");
           assertTrue(true);
       }

       @Disabled("Feature not implemented yet")
       @Test
       void disabledTest() {
           System.out.println("This test is skipped");
           fail("Should not execute");
       }

       @Disabled
       @Test
       void anotherDisabledTest() {
           System.out.println("Also skipped");
       }
   }
   ```
3. Run and verify disabled tests are skipped

**Deliverable**: Understanding of @Disabled annotation

---

## Assertion Exercises

### Exercise 7: Basic Assertions

#### Objective
Practice different assertion methods.

#### Step-by-step (Beginner)
1. Create class: `AssertionsTest.java`
2. Implement:
   ```java
   package com.junit.practice;

   import org.junit.jupiter.api.Test;
   import static org.junit.jupiter.api.Assertions.*;

   class AssertionsTest {

       @Test
       void testEquality() {
           String actual = "Hello";
           String expected = "Hello";
           assertEquals(expected, actual, "Strings should match");
       }

       @Test
       void testBoolean() {
           boolean condition = 5 > 3;
           assertTrue(condition, "5 should be greater than 3");
           assertFalse(5 < 3, "5 should not be less than 3");
       }

       @Test
       void testNull() {
           String nullValue = null;
           String notNullValue = "Not null";
           assertNull(nullValue, "Value should be null");
           assertNotNull(notNullValue, "Value should not be null");
       }

       @Test
       void testArrays() {
           int[] expected = {1, 2, 3};
           int[] actual = {1, 2, 3};
           assertArrayEquals(expected, actual, "Arrays should match");
       }

       @Test
       void testSameReference() {
           String str1 = "hello";
           String str2 = str1;
           String str3 = new String("hello");

           assertSame(str1, str2, "Should be same reference");
           assertNotSame(str1, str3, "Should be different references");
       }
   }
   ```
3. Run and verify all assertions pass
4. Try changing values to see failures

**Deliverable**: Understanding of assertion methods

---

### Exercise 8: assertThrows

#### Objective
Test exception handling.

#### Step-by-step (Beginner)
1. Create class: `ExceptionTest.java`
2. Implement:
   ```java
   package com.junit.practice;

   import org.junit.jupiter.api.Test;
   import static org.junit.jupiter.api.Assertions.*;

   class ExceptionTest {

       @Test
       void testDivisionByZero() {
           assertThrows(ArithmeticException.class, () -> {
               int result = 10 / 0;
           }, "Should throw ArithmeticException");
       }

       @Test
       void testExceptionMessage() {
           Exception exception = assertThrows(IllegalArgumentException.class, () -> {
               throw new IllegalArgumentException("Invalid input");
           });

           assertEquals("Invalid input", exception.getMessage());
       }

       @Test
       void testNumberFormatException() {
           assertThrows(NumberFormatException.class, () -> {
               Integer.parseInt("abc");
           });
       }

       @Test
       void testNullPointerException() {
           assertThrows(NullPointerException.class, () -> {
               String str = null;
               str.length();
           });
       }
   }
   ```

**Deliverable**: Exception testing with assertThrows

---

### Exercise 9: assertAll (Grouped Assertions)

#### Objective
Test multiple assertions together.

#### Step-by-step (Beginner)
1. Create simple User class:
   ```java
   package com.junit.practice;

   public class User {
       private String firstName;
       private String lastName;
       private int age;

       public User(String firstName, String lastName, int age) {
           this.firstName = firstName;
           this.lastName = lastName;
           this.age = age;
       }

       // Getters
       public String getFirstName() { return firstName; }
       public String getLastName() { return lastName; }
       public int getAge() { return age; }
   }
   ```

2. Create test class:
   ```java
   package com.junit.practice;

   import org.junit.jupiter.api.Test;
   import static org.junit.jupiter.api.Assertions.*;

   class GroupedAssertionsTest {

       @Test
       void testUser() {
           User user = new User("John", "Doe", 30);

           // All assertions execute, all failures reported
           assertAll("User properties",
               () -> assertEquals("John", user.getFirstName(), "First name should match"),
               () -> assertEquals("Doe", user.getLastName(), "Last name should match"),
               () -> assertEquals(30, user.getAge(), "Age should match")
           );
       }

       @Test
       void testCalculations() {
           assertAll("Math operations",
               () -> assertEquals(5, 2 + 3),
               () -> assertEquals(2, 10 / 5),
               () -> assertEquals(15, 3 * 5),
               () -> assertEquals(3, 8 - 5)
           );
       }
   }
   ```

**Deliverable**: Grouped assertion testing

---

### Exercise 10: assertTimeout

#### Objective
Test execution time constraints.

#### Step-by-step (Beginner)
1. Create class: `TimeoutTest.java`
2. Implement:
   ```java
   package com.junit.practice;

   import org.junit.jupiter.api.Test;
   import java.time.Duration;
   import static org.junit.jupiter.api.Assertions.*;

   class TimeoutTest {

       @Test
       void testTimeout() {
           assertTimeout(Duration.ofSeconds(2), () -> {
               // Simulate operation that takes 1 second
               Thread.sleep(1000);
               System.out.println("Operation completed");
           }, "Should complete within 2 seconds");
       }

       @Test
       void testTimeoutExceeded() {
           assertTimeout(Duration.ofMillis(100), () -> {
               Thread.sleep(50);  // Passes - completes within 100ms
           });
       }

       @Test
       void testTimeoutPreemptively() {
           // Aborts execution if timeout exceeded
           assertTimeoutPreemptively(Duration.ofSeconds(1), () -> {
               Thread.sleep(500);
           });
       }
   }
   ```

**Deliverable**: Timeout testing

---

## Assumption Exercises

### Exercise 11: Assumptions

#### Objective
Conditionally skip tests based on assumptions.

#### Step-by-step (Beginner)
1. Create class: `AssumptionsTest.java`
2. Implement:
   ```java
   package com.junit.practice;

   import org.junit.jupiter.api.Test;
   import static org.junit.jupiter.api.Assertions.*;
   import static org.junit.jupiter.api.Assumptions.*;

   class AssumptionsTest {

       @Test
       void testOnlyOnCI() {
           // Test runs only if ENV=CI
           assumeTrue("CI".equals(System.getenv("ENV")));
           System.out.println("Running on CI environment");
           // Rest of test logic
       }

       @Test
       void testNotOnProduction() {
           // Test skipped if env=production
           assumeFalse("production".equals(System.getProperty("env")));
           System.out.println("Not running on production");
           // Rest of test logic
       }

       @Test
       void testAssumingThat() {
           assumingThat("DEV".equals(System.getenv("ENV")),
               () -> {
                   System.out.println("Running DEV-specific tests");
                   assertEquals(4, 2 + 2);
               }
           );

           // This always runs regardless of assumption
           System.out.println("This always executes");
           assertTrue(true);
       }

       @Test
       void testWithMultipleAssumptions() {
           String os = System.getProperty("os.name");
           assumeTrue(os.contains("Windows") || os.contains("Mac"));

           System.out.println("Running on Windows or Mac");
           // Test logic
       }
   }
   ```
3. Run with different environments to see conditional execution

**Deliverable**: Conditional test execution

---

## Parameterized Test Exercises

### Exercise 12: @ValueSource

#### Objective
Run test with multiple simple values.

#### Step-by-step (Beginner)
1. Create class: `ValueSourceTest.java`
2. Implement:
   ```java
   package com.junit.practice;

   import org.junit.jupiter.params.ParameterizedTest;
   import org.junit.jupiter.params.provider.ValueSource;
   import static org.junit.jupiter.api.Assertions.*;

   class ValueSourceTest {

       @ParameterizedTest
       @ValueSource(strings = {"hello", "world", "junit", "testing"})
       void testWithStrings(String word) {
           assertNotNull(word);
           assertTrue(word.length() > 3);
           System.out.println("Testing word: " + word);
       }

       @ParameterizedTest
       @ValueSource(ints = {1, 2, 3, 5, 8, 13})
       void testWithIntegers(int number) {
           assertTrue(number > 0);
           System.out.println("Testing number: " + number);
       }

       @ParameterizedTest
       @ValueSource(doubles = {1.5, 2.5, 3.5})
       void testWithDoubles(double value) {
           assertTrue(value > 1.0);
       }
   }
   ```
3. Run and see test execute multiple times

**Deliverable**: Basic parameterized testing

---

### Exercise 13: @CsvSource

#### Objective
Pass multiple parameters using CSV format.

#### Step-by-step (Beginner)
1. Create class: `CsvSourceTest.java`
2. Implement:
   ```java
   package com.junit.practice;

   import org.junit.jupiter.params.ParameterizedTest;
   import org.junit.jupiter.params.provider.CsvSource;
   import static org.junit.jupiter.api.Assertions.*;

   class CsvSourceTest {

       @ParameterizedTest
       @CsvSource({
           "apple, 1",
           "banana, 2",
           "cherry, 3",
           "date, 4"
       })
       void testWithCsv(String fruit, int rank) {
           assertNotNull(fruit);
           assertTrue(rank > 0);
           System.out.println(fruit + " - Rank: " + rank);
       }

       @ParameterizedTest
       @CsvSource({
           "2, 3, 5",
           "10, 5, 15",
           "7, 8, 15",
           "100, 50, 150"
       })
       void testAddition(int a, int b, int expected) {
           assertEquals(expected, a + b);
           System.out.println(a + " + " + b + " = " + expected);
       }

       @ParameterizedTest
       @CsvSource({
           "test@example.com, true",
           "invalid.email, false",
           "user@domain.com, true"
       })
       void testEmailValidation(String email, boolean valid) {
           boolean containsAt = email.contains("@");
           assertEquals(valid, containsAt);
       }
   }
   ```

**Deliverable**: Multi-parameter testing with CSV

---

### Exercise 14: @MethodSource

#### Objective
Use method to provide complex test data.

#### Step-by-step (Beginner)
1. Create class: `MethodSourceTest.java`
2. Implement:
   ```java
   package com.junit.practice;

   import org.junit.jupiter.params.ParameterizedTest;
   import org.junit.jupiter.params.provider.MethodSource;
   import org.junit.jupiter.params.provider.Arguments;
   import java.util.stream.Stream;
   import static org.junit.jupiter.api.Assertions.*;

   class MethodSourceTest {

       @ParameterizedTest
       @MethodSource("provideStrings")
       void testWithMethodSource(String word) {
           assertNotNull(word);
           System.out.println("Testing: " + word);
       }

       static Stream<String> provideStrings() {
           return Stream.of("hello", "world", "junit", "test");
       }

       @ParameterizedTest
       @MethodSource("provideLoginData")
       void testLogin(String username, String password, boolean shouldPass) {
           System.out.println("Username: " + username + ", Password: " + password + ", Valid: " + shouldPass);
           assertNotNull(username);
           assertNotNull(password);
       }

       static Stream<Arguments> provideLoginData() {
           return Stream.of(
               Arguments.of("user1", "pass1", true),
               Arguments.of("user2", "pass2", true),
               Arguments.of("invalid", "wrong", false)
           );
       }

       @ParameterizedTest
       @MethodSource("provideCalculations")
       void testCalculations(int a, int b, int expected, String operation) {
           int result = 0;
           switch(operation) {
               case "add": result = a + b; break;
               case "subtract": result = a - b; break;
               case "multiply": result = a * b; break;
           }
           assertEquals(expected, result);
       }

       static Stream<Arguments> provideCalculations() {
           return Stream.of(
               Arguments.of(2, 3, 5, "add"),
               Arguments.of(10, 5, 5, "subtract"),
               Arguments.of(4, 3, 12, "multiply")
           );
       }
   }
   ```

**Deliverable**: Method source parameterization

---

### Exercise 15: @CsvFileSource

#### Objective
Read test data from CSV file.

#### Step-by-step (Beginner)
1. Create file: `src/test/resources/testdata.csv`
   ```csv
   John,30
   Jane,25
   Bob,35
   Alice,28
   ```

2. Create test class:
   ```java
   package com.junit.practice;

   import org.junit.jupiter.params.ParameterizedTest;
   import org.junit.jupiter.params.provider.CsvFileSource;
   import static org.junit.jupiter.api.Assertions.*;

   class CsvFileSourceTest {

       @ParameterizedTest
       @CsvFileSource(resources = "/testdata.csv")
       void testWithCsvFile(String name, int age) {
           assertNotNull(name);
           assertTrue(age > 0);
           assertTrue(age < 100);
           System.out.println("Name: " + name + ", Age: " + age);
       }
   }
   ```

**Deliverable**: File-based data-driven testing

---

## Nested Test Exercises

### Exercise 16: Nested Tests

#### Objective
Organize tests hierarchically.

#### Step-by-step (Beginner)
1. Create Calculator class:
   ```java
   package com.junit.practice;

   public class Calculator {
       public int add(int a, int b) {
           return a + b;
       }

       public int subtract(int a, int b) {
           return a - b;
       }

       public int multiply(int a, int b) {
           return a * b;
       }

       public int divide(int a, int b) {
           if (b == 0) throw new ArithmeticException("Cannot divide by zero");
           return a / b;
       }
   }
   ```

2. Create nested test class:
   ```java
   package com.junit.practice;

   import org.junit.jupiter.api.*;
   import static org.junit.jupiter.api.Assertions.*;

   @DisplayName("Calculator Tests")
   class NestedCalculatorTest {

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

           @Test
           @DisplayName("Add positive and negative")
           void testMixedAddition() {
               assertEquals(2, calculator.add(5, -3));
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

       @Nested
       @DisplayName("Multiplication Tests")
       class MultiplicationTests {

           @Test
           void testMultiply() {
               assertEquals(12, calculator.multiply(3, 4));
           }

           @Test
           void testMultiplyByZero() {
               assertEquals(0, calculator.multiply(5, 0));
           }
       }
   }
   ```

**Deliverable**: Organized nested test structure

---

## Advanced Exercises

### Exercise 17: Repeated Tests

#### Objective
Run test multiple times.

#### Step-by-step (Beginner)
1. Create class: `RepeatedTestDemo.java`
2. Implement:
   ```java
   package com.junit.practice;

   import org.junit.jupiter.api.*;
   import static org.junit.jupiter.api.Assertions.*;

   class RepeatedTestDemo {

       @RepeatedTest(5)
       void repeatedTest() {
           System.out.println("Test executed");
           assertTrue(true);
       }

       @RepeatedTest(value = 3, name = "{displayName} - repetition {currentRepetition} of {totalRepetitions}")
       @DisplayName("Custom repeated test")
       void customRepeatedTest(RepetitionInfo repetitionInfo) {
           int current = repetitionInfo.getCurrentRepetition();
           int total = repetitionInfo.getTotalRepetitions();
           System.out.println("Repetition " + current + " of " + total);
       }

       @RepeatedTest(3)
       void repeatedTestWithInfo(TestInfo testInfo, RepetitionInfo repetitionInfo) {
           System.out.println("Test: " + testInfo.getDisplayName());
           System.out.println("Rep: " + repetitionInfo.getCurrentRepetition());
       }
   }
   ```

**Deliverable**: Repeated test execution

---

### Exercise 18: @TempDir

#### Objective
Use temporary directory in tests.

#### Step-by-step (Beginner)
1. Create class: `TempDirTest.java`
2. Implement:
   ```java
   package com.junit.practice;

   import org.junit.jupiter.api.Test;
   import org.junit.jupiter.api.io.TempDir;
   import java.nio.file.*;
   import static org.junit.jupiter.api.Assertions.*;

   class TempDirTest {

       @Test
       void testWithTempDir(@TempDir Path tempDir) throws Exception {
           // Create file in temp directory
           Path file = tempDir.resolve("test.txt");
           Files.writeString(file, "Hello JUnit 5");

           // Verify file exists
           assertTrue(Files.exists(file));

           // Read and verify content
           String content = Files.readString(file);
           assertEquals("Hello JUnit 5", content);

           System.out.println("Temp dir: " + tempDir);
           System.out.println("File created: " + file);

           // Temp dir is automatically deleted after test
       }

       @Test
       void testMultipleFiles(@TempDir Path tempDir) throws Exception {
           Path file1 = tempDir.resolve("file1.txt");
           Path file2 = tempDir.resolve("file2.txt");

           Files.writeString(file1, "Content 1");
           Files.writeString(file2, "Content 2");

           assertTrue(Files.exists(file1));
           assertTrue(Files.exists(file2));
       }
   }
   ```

**Deliverable**: Working with temporary directories

---

### Exercise 19: @Tag for Test Organization

#### Objective
Tag and filter tests.

#### Step-by-step (Beginner)
1. Create class: `TaggedTests.java`
2. Implement:
   ```java
   package com.junit.practice;

   import org.junit.jupiter.api.*;
   import static org.junit.jupiter.api.Assertions.*;

   class TaggedTests {

       @Tag("smoke")
       @Test
       void smokeTest1() {
           System.out.println("Smoke test 1");
           assertTrue(true);
       }

       @Tag("smoke")
       @Test
       void smokeTest2() {
           System.out.println("Smoke test 2");
           assertTrue(true);
       }

       @Tag("regression")
       @Test
       void regressionTest1() {
           System.out.println("Regression test 1");
           assertTrue(true);
       }

       @Tag("slow")
       @Test
       void slowTest() throws InterruptedException {
           System.out.println("Slow test");
           Thread.sleep(1000);
           assertTrue(true);
       }

       @Tag("smoke")
       @Tag("fast")
       @Test
       void fastSmokeTest() {
           System.out.println("Fast smoke test");
           assertTrue(true);
       }
   }
   ```

3. Configure Maven to run specific tags:
   ```xml
   <plugin>
       <groupId>org.apache.maven.plugins</groupId>
       <artifactId>maven-surefire-plugin</artifactId>
       <configuration>
           <groups>smoke</groups>  <!-- Only smoke tests -->
       </configuration>
   </plugin>
   ```

**Deliverable**: Tagged and filtered tests

---

## Selenium Integration Exercises

### Exercise 20: Selenium with JUnit 5

#### Objective
Integrate Selenium with JUnit 5.

#### Step-by-step (Beginner)
1. Add Selenium dependencies to pom.xml
2. Create class: `SeleniumJUnit5Test.java`
3. Implement:
   ```java
   package com.junit.practice;

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
           assertTrue(title.contains("Google"), "Title should contain 'Google'");
       }

       @Test
       @DisplayName("Test Wikipedia homepage")
       void testWikipedia() {
           driver.get("https://www.wikipedia.org");
           String title = driver.getTitle();
           assertTrue(title.contains("Wikipedia"), "Title should contain 'Wikipedia'");
       }

       @AfterEach
       void teardown() {
           if (driver != null) {
               driver.quit();
           }
       }
   }
   ```

**Deliverable**: Selenium + JUnit 5 integration

---

### Exercise 21: Parameterized Browser Testing

#### Objective
Run tests on multiple browsers using parameters.

#### Step-by-step (Beginner)
1. Create class: `CrossBrowserTest.java`
2. Implement:
   ```java
   package com.junit.practice;

   import org.junit.jupiter.params.ParameterizedTest;
   import org.junit.jupiter.params.provider.ValueSource;
   import org.openqa.selenium.WebDriver;
   import org.openqa.selenium.chrome.ChromeDriver;
   import org.openqa.selenium.firefox.FirefoxDriver;
   import io.github.bonigarcia.wdm.WebDriverManager;
   import static org.junit.jupiter.api.Assertions.*;

   class CrossBrowserTest {

       @ParameterizedTest
       @ValueSource(strings = {"chrome", "firefox"})
       void testOnMultipleBrowsers(String browser) {
           WebDriver driver = null;

           try {
               if (browser.equals("chrome")) {
                   WebDriverManager.chromedriver().setup();
                   driver = new ChromeDriver();
               } else if (browser.equals("firefox")) {
                   WebDriverManager.firefoxdriver().setup();
                   driver = new FirefoxDriver();
               }

               driver.manage().window().maximize();
               driver.get("https://www.saucedemo.com");

               String url = driver.getCurrentUrl();
               assertTrue(url.contains("saucedemo"));
               System.out.println("Test passed on " + browser);

           } finally {
               if (driver != null) {
                   driver.quit();
               }
           }
       }
   }
   ```

**Deliverable**: Cross-browser testing with parameters

---

### Exercise 22: Nested Selenium Tests

#### Objective
Organize Selenium tests with nested structure.

#### Step-by-step (Beginner)
1. Create class: `NestedSeleniumTest.java`
2. Implement:
   ```java
   package com.junit.practice;

   import org.junit.jupiter.api.*;
   import org.openqa.selenium.By;
   import org.openqa.selenium.WebDriver;
   import org.openqa.selenium.chrome.ChromeDriver;
   import io.github.bonigarcia.wdm.WebDriverManager;
   import static org.junit.jupiter.api.Assertions.*;

   @DisplayName("Sauce Demo Tests")
   class NestedSeleniumTest {

       static WebDriver driver;

       @BeforeAll
       static void setupAll() {
           WebDriverManager.chromedriver().setup();
       }

       @BeforeEach
       void setup() {
           driver = new ChromeDriver();
           driver.manage().window().maximize();
           driver.get("https://www.saucedemo.com");
       }

       @Nested
       @DisplayName("Login Tests")
       class LoginTests {

           @Test
           @DisplayName("Valid login")
           void testValidLogin() {
               driver.findElement(By.id("user-name")).sendKeys("standard_user");
               driver.findElement(By.id("password")).sendKeys("secret_sauce");
               driver.findElement(By.id("login-button")).click();

               String url = driver.getCurrentUrl();
               assertTrue(url.contains("inventory"));
           }

           @Test
           @DisplayName("Invalid login")
           void testInvalidLogin() {
               driver.findElement(By.id("user-name")).sendKeys("invalid");
               driver.findElement(By.id("password")).sendKeys("wrong");
               driver.findElement(By.id("login-button")).click();

               assertTrue(driver.findElement(By.className("error-message")).isDisplayed());
           }
       }

       @AfterEach
       void teardown() {
           if (driver != null) {
               driver.quit();
           }
       }
   }
   ```

**Deliverable**: Organized Selenium test suite

---

## Complete Exercise

### Exercise 23: End-to-End Test Suite

#### Objective
Build complete test suite with all JUnit 5 features.

#### Step-by-step (Beginner)
1. Create comprehensive test suite combining:
   - @BeforeAll/@AfterAll for driver setup
   - @BeforeEach/@AfterEach for test isolation
   - @DisplayName for readability
   - @ParameterizedTest for data-driven tests
   - @Nested for organization
   - assertAll for grouped assertions
   - @Tag for filtering

2. Example structure:
   ```java
   @DisplayName("E-Commerce Test Suite")
   @TestInstance(TestInstance.Lifecycle.PER_CLASS)
   class E2ETestSuite {

       WebDriver driver;

       @BeforeAll
       void setupSuite() {
           // One-time setup
       }

       @Nested
       @Tag("smoke")
       @DisplayName("Smoke Tests")
       class SmokeTests {
           // Critical tests
       }

       @Nested
       @Tag("regression")
       @DisplayName("Regression Tests")
       class RegressionTests {
           // Full regression
       }

       @AfterAll
       void teardownSuite() {
           // Cleanup
       }
   }
   ```

**Deliverable**: Production-ready test framework

---

## Summary

These 23 exercises cover:
- ✓ JUnit 5 setup and annotations
- ✓ All assertion types
- ✓ Exception and timeout testing
- ✓ Assumptions for conditional execution
- ✓ Parameterized tests (@ValueSource, @CsvSource, @MethodSource)
- ✓ Nested test organization
- ✓ Repeated tests
- ✓ @TempDir for file testing
- ✓ @Tag for test filtering
- ✓ Selenium integration
- ✓ Cross-browser testing
- ✓ Complete E2E framework

Practice these exercises to master JUnit 5 for modern test automation!

**Recommended Practice Sites**:
- https://www.saucedemo.com
- http://the-internet.herokuapp.com
- https://practice.expandtesting.com
