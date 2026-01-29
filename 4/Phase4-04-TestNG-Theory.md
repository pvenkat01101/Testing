# Phase 4.2 - TestNG Framework: Comprehensive Theory

## Part 1: Introduction to TestNG

### What is TestNG?

TestNG (Test Next Generation) is a testing framework for Java inspired by JUnit and NUnit, but with more powerful features designed for modern testing needs.

### Why TestNG?

**Advantages over JUnit 4**:
- More flexible test configuration
- Better annotations (@BeforeClass, @AfterClass, etc.)
- Parameterized tests with @DataProvider
- Test grouping and prioritization
- Parallel test execution
- Built-in HTML reports
- Dependency testing
- Easy integration with Maven, Jenkins

### TestNG vs JUnit

| Feature | TestNG | JUnit 4 | JUnit 5 |
|---------|--------|---------|---------|
| Annotations | More flexible | Basic | Improved |
| Parameterization | @DataProvider | @RunWith | @ParameterizedTest |
| Grouping | Yes | No | @Tag |
| Parallel execution | Built-in | No | Limited |
| Dependency testing | Yes | No | No |
| Reports | Built-in HTML | No | No |
| Configuration | testng.xml | No | junit-platform.properties |

---

## Part 2: Setup TestNG

### Maven Dependency

Add to `pom.xml`:

```xml
<dependencies>
    <dependency>
        <groupId>org.testng</groupId>
        <artifactId>testng</artifactId>
        <version>7.8.0</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

### First TestNG Test

```java
import org.testng.annotations.Test;
import org.testng.Assert;

public class FirstTest {

    @Test
    public void testAddition() {
        int result = 2 + 3;
        Assert.assertEquals(result, 5);
        System.out.println("Test passed!");
    }
}
```

**Running**:
- Right-click → Run as TestNG Test (Eclipse)
- Run using Maven: `mvn test`

---

## Part 3: TestNG Annotations

### Annotation Hierarchy

```
@BeforeSuite
    @BeforeTest
        @BeforeClass
            @BeforeMethod
                @Test
            @AfterMethod
        @AfterClass
    @AfterTest
@AfterSuite
```

### Core Annotations

#### 1. @Test

Marks a method as a test case.

```java
@Test
public void loginTest() {
    // Test logic
}
```

**Attributes**:
```java
@Test(description = "Verify valid login")
public void testValidLogin() { }

@Test(priority = 1)
public void firstTest() { }

@Test(priority = 2)
public void secondTest() { }

@Test(enabled = false)  // Skip this test
public void skipTest() { }

@Test(timeOut = 5000)  // Fail if takes > 5 seconds
public void performanceTest() { }

@Test(expectedExceptions = ArithmeticException.class)
public void exceptionTest() {
    int result = 10 / 0;  // Expected to throw exception
}
```

#### 2. @BeforeMethod and @AfterMethod

Run before/after each @Test method.

```java
public class LoginTest {

    @BeforeMethod
    public void setup() {
        System.out.println("Setup before each test");
        // Initialize browser, login
    }

    @Test
    public void test1() {
        System.out.println("Test 1");
    }

    @Test
    public void test2() {
        System.out.println("Test 2");
    }

    @AfterMethod
    public void teardown() {
        System.out.println("Cleanup after each test");
        // Close browser
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

#### 3. @BeforeClass and @AfterClass

Run once before/after all tests in the class.

```java
public class TestClass {

    @BeforeClass
    public void setupClass() {
        System.out.println("Setup once for class");
        // Initialize WebDriver, configure settings
    }

    @Test
    public void test1() {
        System.out.println("Test 1");
    }

    @Test
    public void test2() {
        System.out.println("Test 2");
    }

    @AfterClass
    public void teardownClass() {
        System.out.println("Cleanup once for class");
        // Close browser, cleanup resources
    }
}

/* Output:
Setup once for class
Test 1
Test 2
Cleanup once for class
*/
```

#### 4. @BeforeTest and @AfterTest

Run before/after all test classes in `<test>` tag in testng.xml.

```java
@BeforeTest
public void setupTest() {
    System.out.println("Before all test classes in <test> tag");
}

@AfterTest
public void teardownTest() {
    System.out.println("After all test classes in <test> tag");
}
```

#### 5. @BeforeSuite and @AfterSuite

Run before/after all tests in the suite.

```java
@BeforeSuite
public void setupSuite() {
    System.out.println("Setup once for entire suite");
    // Database connection, environment setup
}

@AfterSuite
public void teardownSuite() {
    System.out.println("Cleanup once for entire suite");
    // Close database, generate reports
}
```

### Complete Execution Order Example

```java
public class AnnotationOrderTest {

    @BeforeSuite
    public void beforeSuite() {
        System.out.println("1. Before Suite");
    }

    @BeforeTest
    public void beforeTest() {
        System.out.println("2. Before Test");
    }

    @BeforeClass
    public void beforeClass() {
        System.out.println("3. Before Class");
    }

    @BeforeMethod
    public void beforeMethod() {
        System.out.println("4. Before Method");
    }

    @Test
    public void test1() {
        System.out.println("5. Test 1");
    }

    @Test
    public void test2() {
        System.out.println("5. Test 2");
    }

    @AfterMethod
    public void afterMethod() {
        System.out.println("6. After Method");
    }

    @AfterClass
    public void afterClass() {
        System.out.println("7. After Class");
    }

    @AfterTest
    public void afterTest() {
        System.out.println("8. After Test");
    }

    @AfterSuite
    public void afterSuite() {
        System.out.println("9. After Suite");
    }
}

/* Output:
1. Before Suite
2. Before Test
3. Before Class
4. Before Method
5. Test 1
6. After Method
4. Before Method
5. Test 2
6. After Method
7. After Class
8. After Test
9. After Suite
*/
```

---

## Part 4: Assertions in TestNG

### Assert vs Verify

**Assert**: Hard assertion - test stops if fails
**Soft Assert**: Collects all failures, reports at end

### Common Assertions

```java
import org.testng.Assert;

// Equality
Assert.assertEquals(actual, expected);
Assert.assertEquals(actual, expected, "Error message");

// Boolean
Assert.assertTrue(condition);
Assert.assertTrue(condition, "Should be true");
Assert.assertFalse(condition);

// Null check
Assert.assertNull(object);
Assert.assertNotNull(object);

// Same object reference
Assert.assertSame(obj1, obj2);
Assert.assertNotSame(obj1, obj2);

// Fail test explicitly
Assert.fail("Test failed intentionally");
```

### Practical Examples

```java
@Test
public void testLogin() {
    String actualTitle = driver.getTitle();
    String expectedTitle = "Dashboard";
    Assert.assertEquals(actualTitle, expectedTitle, "Title mismatch");

    boolean isLogoutVisible = driver.findElement(By.id("logout")).isDisplayed();
    Assert.assertTrue(isLogoutVisible, "Logout button not visible");
}
```

### Soft Assertions

Collects all assertion failures, reports at end.

```java
import org.testng.asserts.SoftAssert;

@Test
public void testMultipleChecks() {
    SoftAssert softAssert = new SoftAssert();

    softAssert.assertEquals(2 + 2, 4, "Addition check");
    softAssert.assertTrue(false, "This will fail");  // Continues
    softAssert.assertEquals("hello", "hello", "String check");

    // Must call assertAll() at end
    softAssert.assertAll();  // Reports all failures now
}
```

---

## Part 5: TestNG XML Configuration

### What is testng.xml?

XML file to configure test execution: which tests to run, in what order, with what parameters, in parallel, etc.

### Basic testng.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd">

<suite name="Test Suite">
    <test name="Login Tests">
        <classes>
            <class name="com.example.tests.LoginTest"/>
            <class name="com.example.tests.LogoutTest"/>
        </classes>
    </test>
</suite>
```

### Suite Structure

```xml
<suite name="Main Suite">
    <test name="Test 1">
        <classes>
            <class name="TestClass1"/>
        </classes>
    </test>

    <test name="Test 2">
        <classes>
            <class name="TestClass2"/>
        </classes>
    </test>
</suite>
```

### Running Specific Methods

```xml
<suite name="Suite">
    <test name="Test">
        <classes>
            <class name="com.example.LoginTest">
                <methods>
                    <include name="testValidLogin"/>
                    <include name="testInvalidLogin"/>
                    <exclude name="testOAuth"/>
                </methods>
            </class>
        </classes>
    </test>
</suite>
```

### Multiple Test Classes

```xml
<suite name="Regression Suite">
    <test name="All Tests">
        <classes>
            <class name="com.example.LoginTest"/>
            <class name="com.example.CheckoutTest"/>
            <class name="com.example.PaymentTest"/>
        </classes>
    </test>
</suite>
```

### Package Level

```xml
<suite name="Suite">
    <test name="Test">
        <packages>
            <package name="com.example.tests.*"/>
        </packages>
    </test>
</suite>
```

---

## Part 6: Grouping Tests

### Creating Groups

```java
@Test(groups = {"smoke"})
public void testLogin() {
    System.out.println("Login test - Smoke");
}

@Test(groups = {"regression"})
public void testCheckout() {
    System.out.println("Checkout test - Regression");
}

@Test(groups = {"smoke", "regression"})
public void testSearch() {
    System.out.println("Search test - Both groups");
}
```

### Running Groups in testng.xml

```xml
<suite name="Suite">
    <test name="Smoke Tests">
        <groups>
            <run>
                <include name="smoke"/>
            </run>
        </groups>
        <classes>
            <class name="com.example.AllTests"/>
        </classes>
    </test>
</suite>
```

### Multiple Groups

```xml
<suite name="Suite">
    <test name="Test">
        <groups>
            <run>
                <include name="smoke"/>
                <include name="regression"/>
                <exclude name="slow"/>
            </run>
        </groups>
        <classes>
            <class name="com.example.AllTests"/>
        </classes>
    </test>
</suite>
```

### Group of Groups (Meta Groups)

```xml
<suite name="Suite">
    <test name="Test">
        <groups>
            <define name="all">
                <include name="smoke"/>
                <include name="regression"/>
            </define>
            <run>
                <include name="all"/>
            </run>
        </groups>
        <classes>
            <class name="com.example.AllTests"/>
        </classes>
    </test>
</suite>
```

---

## Part 7: Parameterization

### 1. Parameters from testng.xml

```xml
<suite name="Suite">
    <test name="Chrome Test">
        <parameter name="browser" value="chrome"/>
        <parameter name="url" value="https://www.saucedemo.com"/>
        <classes>
            <class name="com.example.LoginTest"/>
        </classes>
    </test>

    <test name="Firefox Test">
        <parameter name="browser" value="firefox"/>
        <parameter name="url" value="https://www.saucedemo.com"/>
        <classes>
            <class name="com.example.LoginTest"/>
        </classes>
    </test>
</suite>
```

**Test Class**:
```java
public class LoginTest {

    @Parameters({"browser", "url"})
    @BeforeClass
    public void setup(String browser, String url) {
        System.out.println("Browser: " + browser);
        System.out.println("URL: " + url);

        if (browser.equals("chrome")) {
            driver = new ChromeDriver();
        } else if (browser.equals("firefox")) {
            driver = new FirefoxDriver();
        }

        driver.get(url);
    }

    @Test
    public void testLogin() {
        // Test logic
    }
}
```

### 2. @DataProvider

For data-driven testing with multiple sets of data.

**Basic DataProvider**:
```java
@DataProvider(name = "loginData")
public Object[][] getData() {
    return new Object[][] {
        {"user1", "pass1"},
        {"user2", "pass2"},
        {"user3", "pass3"}
    };
}

@Test(dataProvider = "loginData")
public void testLogin(String username, String password) {
    System.out.println("Testing with: " + username + " / " + password);
    // Actual test logic
}

/* Output:
Testing with: user1 / pass1
Testing with: user2 / pass2
Testing with: user3 / pass3
*/
```

**DataProvider in Separate Class**:
```java
// DataProviders.java
public class DataProviders {

    @DataProvider(name = "loginData")
    public Object[][] getLoginData() {
        return new Object[][] {
            {"standard_user", "secret_sauce", true},
            {"locked_user", "secret_sauce", false},
            {"invalid_user", "invalid_pass", false}
        };
    }
}

// Test class
public class LoginTest {

    @Test(dataProvider = "loginData", dataProviderClass = DataProviders.class)
    public void testLogin(String username, String password, boolean shouldPass) {
        // Test logic
        System.out.println("User: " + username + ", Should pass: " + shouldPass);
    }
}
```

**DataProvider with Excel/CSV**:
```java
@DataProvider(name = "excelData")
public Object[][] getDataFromExcel() throws IOException {
    // Read from Excel file
    // Return data as Object[][]
}
```

---

## Part 8: Test Dependencies

### Method Dependencies

Run tests in specific order.

```java
@Test
public void login() {
    System.out.println("1. Login");
}

@Test(dependsOnMethods = {"login"})
public void addToCart() {
    System.out.println("2. Add to cart (depends on login)");
}

@Test(dependsOnMethods = {"addToCart"})
public void checkout() {
    System.out.println("3. Checkout (depends on add to cart)");
}

/* If login fails, addToCart and checkout are skipped */
```

### Group Dependencies

```java
@Test(groups = {"init"})
public void setupData() {
    System.out.println("Setup data");
}

@Test(dependsOnGroups = {"init"})
public void runTest() {
    System.out.println("Run test (depends on init group)");
}
```

### Strong vs Weak Dependencies

```java
// Strong dependency (default) - skips dependent test if dependency fails
@Test
public void test1() {
    Assert.fail("Intentional failure");
}

@Test(dependsOnMethods = {"test1"})
public void test2() {
    // This is SKIPPED because test1 failed
}

// Weak dependency (alwaysRun) - runs even if dependency fails
@Test(dependsOnMethods = {"test1"}, alwaysRun = true)
public void test3() {
    // This RUNS even though test1 failed
}
```

---

## Part 9: Parallel Execution

### Benefits of Parallel Testing

- Faster execution
- Better resource utilization
- Reduced test execution time

### Parallel at Different Levels

**testng.xml configuration**:

#### 1. Parallel Methods

```xml
<suite name="Suite" parallel="methods" thread-count="3">
    <test name="Test">
        <classes>
            <class name="com.example.TestClass"/>
        </classes>
    </test>
</suite>
```

All @Test methods run in parallel (max 3 threads).

#### 2. Parallel Classes

```xml
<suite name="Suite" parallel="classes" thread-count="2">
    <test name="Test">
        <classes>
            <class name="com.example.LoginTest"/>
            <class name="com.example.CheckoutTest"/>
        </classes>
    </test>
</suite>
```

Test classes run in parallel.

#### 3. Parallel Tests

```xml
<suite name="Suite" parallel="tests" thread-count="2">
    <test name="Test1">
        <classes>
            <class name="com.example.Test1"/>
        </classes>
    </test>
    <test name="Test2">
        <classes>
            <class name="com.example.Test2"/>
        </classes>
    </test>
</suite>
```

`<test>` tags run in parallel.

#### 4. Parallel Instances

```xml
<suite name="Suite" parallel="instances" thread-count="2">
    <!-- ... -->
</suite>
```

Instances of the same class run in parallel.

### Thread Safety

**Important**: Ensure WebDriver is thread-safe for parallel execution.

**Solution - ThreadLocal**:
```java
public class BaseTest {

    private static ThreadLocal<WebDriver> driver = new ThreadLocal<>();

    @BeforeMethod
    public void setup() {
        WebDriver webDriver = new ChromeDriver();
        driver.set(webDriver);
    }

    public static WebDriver getDriver() {
        return driver.get();
    }

    @AfterMethod
    public void teardown() {
        getDriver().quit();
        driver.remove();
    }
}

// In tests
public class LoginTest extends BaseTest {

    @Test
    public void test() {
        getDriver().get("https://example.com");
    }
}
```

---

## Part 10: Listeners in TestNG

### What are Listeners?

Listeners "listen" to test events and execute custom logic.

### Common Listeners

#### 1. ITestListener

Listen to test method events.

```java
import org.testng.ITestContext;
import org.testng.ITestListener;
import org.testng.ITestResult;

public class TestListener implements ITestListener {

    @Override
    public void onStart(ITestContext context) {
        System.out.println("Test Suite Started: " + context.getName());
    }

    @Override
    public void onFinish(ITestContext context) {
        System.out.println("Test Suite Finished: " + context.getName());
    }

    @Override
    public void onTestStart(ITestResult result) {
        System.out.println("Test Started: " + result.getName());
    }

    @Override
    public void onTestSuccess(ITestResult result) {
        System.out.println("Test Passed: " + result.getName());
    }

    @Override
    public void onTestFailure(ITestResult result) {
        System.out.println("Test Failed: " + result.getName());
        // Take screenshot
    }

    @Override
    public void onTestSkipped(ITestResult result) {
        System.out.println("Test Skipped: " + result.getName());
    }
}
```

**Using Listener**:

Method 1 - In test class:
```java
@Listeners(TestListener.class)
public class LoginTest {
    @Test
    public void test1() {
        // Test
    }
}
```

Method 2 - In testng.xml:
```xml
<suite name="Suite">
    <listeners>
        <listener class-name="com.example.TestListener"/>
    </listeners>

    <test name="Test">
        <classes>
            <class name="com.example.LoginTest"/>
        </classes>
    </test>
</suite>
```

#### 2. IRetryAnalyzer

Retry failed tests automatically.

```java
import org.testng.IRetryAnalyzer;
import org.testng.ITestResult;

public class RetryAnalyzer implements IRetryAnalyzer {

    private int retryCount = 0;
    private static final int maxRetryCount = 2;

    @Override
    public boolean retry(ITestResult result) {
        if (retryCount < maxRetryCount) {
            System.out.println("Retrying test: " + result.getName() + " for " + (retryCount + 1) + " time");
            retryCount++;
            return true;  // Retry
        }
        return false;  // Don't retry
    }
}

// Use in test
@Test(retryAnalyzer = RetryAnalyzer.class)
public void flaky Test() {
    // Test that sometimes fails
}
```

---

## Part 11: Reporting in TestNG

### Built-in Reports

After test execution, TestNG generates:
1. **test-output/index.html**: HTML report
2. **test-output/emailable-report.html**: Email-friendly report
3. **testng-results.xml**: XML results

### Customizing Reports

#### ExtentReports Integration

Popular third-party reporting library.

**Add dependency**:
```xml
<dependency>
    <groupId>com.aventstack</groupId>
    <artifactId>extentreports</artifactId>
    <version>5.0.9</version>
</dependency>
```

**Usage**:
```java
import com.aventstack.extentreports.ExtentReports;
import com.aventstack.extentreports.ExtentTest;
import com.aventstack.extentreports.reporter.ExtentSparkReporter;

public class ExtentManager {
    private static ExtentReports extent;

    public static ExtentReports createInstance() {
        ExtentSparkReporter htmlReporter = new ExtentSparkReporter("extent-report.html");
        extent = new ExtentReports();
        extent.attachReporter(htmlReporter);
        return extent;
    }
}

// In test
ExtentReports extent = ExtentManager.createInstance();
ExtentTest test = extent.createTest("Login Test");
test.pass("Login successful");
extent.flush();
```

---

## Part 12: TestNG with Maven

### Running TestNG from Maven

**pom.xml**:
```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>2.22.2</version>
            <configuration>
                <suiteXmlFiles>
                    <suiteXmlFile>testng.xml</suiteXmlFile>
                </suiteXmlFiles>
            </configuration>
        </plugin>
    </plugins>
</build>
```

**Run**:
```bash
mvn test
mvn clean test
```

---

## Part 13: Best Practices

1. **Use testng.xml for test organization**
2. **Group tests logically** (smoke, regression, etc.)
3. **Use @BeforeClass for expensive operations** (driver initialization)
4. **Use @BeforeMethod for test isolation** (fresh state per test)
5. **Always call assertAll() with SoftAssert**
6. **Use @DataProvider for data-driven tests**
7. **Use listeners for logging and reporting**
8. **Implement retry logic for flaky tests**
9. **Use ThreadLocal for parallel execution**
10. **Keep tests independent** (no dependencies between tests)
11. **Use meaningful test names and descriptions**
12. **Add assertions to verify expected behavior**

---

## Conclusion

TestNG is a powerful testing framework with:
- Flexible annotations for test lifecycle
- Built-in parameterization and data-driven testing
- Test grouping and prioritization
- Parallel execution support
- Dependency management
- Listeners for custom behavior
- Built-in reporting

Master TestNG to build robust, maintainable test automation frameworks!
