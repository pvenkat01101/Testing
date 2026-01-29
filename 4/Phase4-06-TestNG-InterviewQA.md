# Phase 4.2 - TestNG Framework: Interview Questions and Answers

## Section 1: TestNG Basics (Questions 1-10)

### Q1: What is TestNG and why is it used?

**Answer**:
TestNG (Test Next Generation) is a testing framework for Java inspired by JUnit and NUnit, designed for test configuration flexibility and powerful features.

**Why use TestNG**:
- Flexible test configuration
- Advanced annotations (@BeforeClass, @AfterClass, @DataProvider)
- Test grouping and prioritization
- Parameterization support
- Parallel execution
- Built-in HTML reports
- Dependency testing
- Integration with Maven, Jenkins, CI/CD

**Practical Example**:
```java
@Test
public void testLogin() {
    // Test logic with automatic reporting
}
```

**In-depth Explanation**:
TestNG provides enterprise-level features missing in JUnit 4, making it ideal for complex test scenarios, data-driven testing, and Selenium automation frameworks.

---

### Q2: What are the advantages of TestNG over JUnit?

**Answer**:

| Feature | TestNG | JUnit 4 | JUnit 5 |
|---------|--------|---------|---------|
| Annotations | More flexible | Basic | Improved |
| Data Provider | @DataProvider | @RunWith | @ParameterizedTest |
| Grouping | Yes | No | @Tag |
| Parallel execution | Built-in | No | Limited |
| Dependency testing | Yes | No | No |
| Test configuration | testng.xml | No | Limited |
| Reports | Built-in HTML | No | No |
| Priority | Yes | No | @Order |

**Practical Example**:
```java
// TestNG - Built-in parameterization
@DataProvider
public Object[][] data() {
    return new Object[][] {{"user1", "pass1"}, {"user2", "pass2"}};
}

@Test(dataProvider = "data")
public void test(String user, String pass) { }

// JUnit 4 - Requires external library
```

**In-depth Explanation**:
TestNG was built specifically for testing with features testers need, while JUnit evolved from unit testing origins.

---

### Q3: Explain the TestNG annotation hierarchy.

**Answer**:
TestNG annotations execute in specific order:

```
@BeforeSuite        (Once per suite)
    @BeforeTest     (Once per <test> tag)
        @BeforeClass    (Once per class)
            @BeforeMethod   (Before each @Test)
                @Test           (Test method)
            @AfterMethod    (After each @Test)
        @AfterClass     (Once per class)
    @AfterTest      (Once per <test> tag)
@AfterSuite         (Once per suite)
```

**Practical Example**:
```java
@BeforeSuite
public void suiteSetup() {
    // Database connection, environment setup
}

@BeforeClass
public void classSetup() {
    // Initialize WebDriver
}

@BeforeMethod
public void methodSetup() {
    // Login, navigate to page
}

@Test
public void testCase() {
    // Test logic
}
```

**In-depth Explanation**:
Use @BeforeSuite for expensive one-time operations, @BeforeClass for resource initialization, @BeforeMethod for test isolation.

---

### Q4: What is the difference between @BeforeMethod and @BeforeClass?

**Answer**:

| Aspect | @BeforeMethod | @BeforeClass |
|--------|---------------|--------------|
| Execution | Before each @Test method | Once before all tests in class |
| Frequency | Multiple times | Once |
| Use case | Test isolation, fresh state | Expensive setup (driver, DB) |
| Performance | Slower (runs repeatedly) | Faster (runs once) |

**Practical Example**:
```java
public class LoginTest {

    WebDriver driver;

    @BeforeClass
    public void setupDriver() {
        // Expensive operation - once per class
        driver = new ChromeDriver();
        System.out.println("Driver initialized");
    }

    @BeforeMethod
    public void navigateToLoginPage() {
        // Before each test - fresh state
        driver.get("https://app.com/login");
        System.out.println("Navigated to login");
    }

    @Test
    public void test1() { }

    @Test
    public void test2() { }

    @AfterClass
    public void closeDriver() {
        driver.quit();
    }
}

/* Output:
Driver initialized
Navigated to login
Test 1
Navigated to login
Test 2
Driver closed
*/
```

**In-depth Explanation**:
Use @BeforeClass for expensive operations to improve performance. Use @BeforeMethod when tests need complete isolation.

---

### Q5: What is testng.xml and why is it used?

**Answer**:
testng.xml is an XML configuration file that controls test execution: which tests to run, parameters, parallel settings, grouping.

**Purpose**:
- Organize multiple test classes
- Group tests (smoke, regression)
- Pass parameters to tests
- Control parallel execution
- Include/exclude specific methods
- Define test suites

**Practical Example**:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd">

<suite name="Regression Suite" parallel="tests" thread-count="2">
    <test name="Smoke Tests">
        <groups>
            <run>
                <include name="smoke"/>
            </run>
        </groups>
        <classes>
            <class name="com.tests.LoginTest"/>
            <class name="com.tests.CheckoutTest"/>
        </classes>
    </test>

    <test name="Regression Tests">
        <parameter name="browser" value="chrome"/>
        <classes>
            <class name="com.tests.FullRegressionTest"/>
        </classes>
    </test>
</suite>
```

**In-depth Explanation**:
testng.xml provides centralized test configuration, enabling flexible test execution without changing code. Essential for CI/CD integration.

---

### Q6: How do you pass parameters in TestNG?

**Answer**:
Two ways: @Parameters from XML and @DataProvider from code.

**Method 1: @Parameters (from testng.xml)**:
```java
// Test class
@Parameters({"browser", "url"})
@BeforeClass
public void setup(String browser, String url) {
    System.out.println("Browser: " + browser);
    System.out.println("URL: " + url);
}
```

```xml
<!-- testng.xml -->
<suite name="Suite">
    <test name="Chrome Test">
        <parameter name="browser" value="chrome"/>
        <parameter name="url" value="https://app.com"/>
        <classes>
            <class name="com.tests.Test"/>
        </classes>
    </test>
</suite>
```

**Method 2: @DataProvider (from code)**:
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
    System.out.println("Testing: " + username);
}
```

**When to use**:
- @Parameters: Environment/browser configuration
- @DataProvider: Multiple test data sets

---

### Q7: What is @DataProvider in TestNG?

**Answer**:
@DataProvider is a TestNG annotation that supplies data to test methods, enabling data-driven testing.

**Features**:
- Returns Object[][]
- Each inner array = one test iteration
- Can be in same class or separate class
- Supports multiple data sets

**Practical Example**:
```java
@DataProvider(name = "testData")
public Object[][] getTestData() {
    return new Object[][] {
        {"standard_user", "secret_sauce", true},
        {"locked_user", "secret_sauce", false},
        {"invalid_user", "wrong_pass", false}
    };
}

@Test(dataProvider = "testData")
public void testLogin(String username, String password, boolean shouldPass) {
    loginPage.login(username, password);

    if (shouldPass) {
        Assert.assertTrue(dashboardPage.isDisplayed());
    } else {
        Assert.assertTrue(loginPage.isErrorDisplayed());
    }
}
```

**External DataProvider**:
```java
// DataProviders.java
public class DataProviders {
    @DataProvider(name = "data")
    public static Object[][] getData() {
        return new Object[][] { /* data */ };
    }
}

// Test class
@Test(dataProvider = "data", dataProviderClass = DataProviders.class)
public void test(String param) { }
```

**In-depth Explanation**:
DataProvider is powerful for testing multiple scenarios without duplicating code. Can read from Excel, CSV, databases for large datasets.

---

### Q8: What is test grouping in TestNG?

**Answer**:
Grouping allows organizing tests into categories (smoke, regression, etc.) and running specific groups.

**Creating Groups**:
```java
@Test(groups = {"smoke"})
public void testLogin() {
    // Critical test
}

@Test(groups = {"regression"})
public void testCheckout() {
    // Full regression test
}

@Test(groups = {"smoke", "regression"})
public void testSearch() {
    // Belongs to both groups
}
```

**Running Groups in testng.xml**:
```xml
<suite name="Suite">
    <test name="Smoke Tests">
        <groups>
            <run>
                <include name="smoke"/>
            </run>
        </groups>
        <classes>
            <class name="com.tests.AllTests"/>
        </classes>
    </test>
</suite>
```

**Use Cases**:
- Smoke tests before deployment
- Regression after each release
- Priority-based execution
- Environment-specific tests

**In-depth Explanation**:
Groups provide flexibility to run different test subsets without changing code, crucial for CI/CD pipelines.

---

### Q9: What are assertions in TestNG?

**Answer**:
Assertions verify expected vs actual results. TestNG uses Assert class.

**Common Assertions**:
```java
// Equality
Assert.assertEquals(actual, expected);
Assert.assertEquals(actual, expected, "Error message");

// Boolean
Assert.assertTrue(condition);
Assert.assertFalse(condition);

// Null checks
Assert.assertNull(object);
Assert.assertNotNull(object);

// Reference equality
Assert.assertSame(obj1, obj2);
Assert.assertNotSame(obj1, obj2);

// Explicit failure
Assert.fail("Test failed");
```

**Practical Example**:
```java
@Test
public void testLogin() {
    driver.get("https://app.com");
    loginPage.login("user", "pass");

    String actualUrl = driver.getCurrentUrl();
    Assert.assertTrue(actualUrl.contains("/dashboard"), "Should navigate to dashboard");

    WebElement logoutBtn = driver.findElement(By.id("logout"));
    Assert.assertTrue(logoutBtn.isDisplayed(), "Logout button should be visible");
}
```

**In-depth Explanation**:
Assertions stop test execution on failure (hard assertion). Use meaningful messages for debugging.

---

### Q10: What is Soft Assert in TestNG?

**Answer**:
Soft Assert collects all assertion failures and reports them at the end, allowing tests to continue after failures.

**Hard Assert vs Soft Assert**:

| Aspect | Hard Assert | Soft Assert |
|--------|-------------|-------------|
| Stops execution | Yes | No |
| Continues after failure | No | Yes |
| Class | Assert | SoftAssert |
| Use case | Single critical check | Multiple validations |

**Practical Example**:
```java
@Test
public void testDashboard() {
    SoftAssert softAssert = new SoftAssert();

    // Check 1 - passes
    String title = driver.getTitle();
    softAssert.assertEquals(title, "Dashboard", "Title check");

    // Check 2 - fails, but continues
    WebElement logo = driver.findElement(By.id("logo"));
    softAssert.assertTrue(logo.isDisplayed(), "Logo should be visible");

    // Check 3 - continues even though Check 2 failed
    int productCount = driver.findElements(By.className("product")).size();
    softAssert.assertTrue(productCount > 0, "Products should be displayed");

    // Check 4 - continues
    String username = driver.findElement(By.id("username")).getText();
    softAssert.assertEquals(username, "John Doe", "Username check");

    // MUST call assertAll() at end
    softAssert.assertAll();  // Reports all failures now
}
```

**Important**: Always call `softAssert.assertAll()` or failures won't be reported.

---

## Section 2: Advanced TestNG (Questions 11-20)

### Q11: What are test dependencies in TestNG?

**Answer**:
Dependencies allow tests to run in specific order. If dependency fails, dependent tests are skipped.

**Method Dependencies**:
```java
@Test
public void createAccount() {
    System.out.println("Account created");
}

@Test(dependsOnMethods = {"createAccount"})
public void login() {
    System.out.println("Login successful");
    // Skipped if createAccount fails
}

@Test(dependsOnMethods = {"login"})
public void updateProfile() {
    System.out.println("Profile updated");
    // Skipped if login fails
}
```

**Group Dependencies**:
```java
@Test(groups = {"init"})
public void setupData() {
    System.out.println("Data setup");
}

@Test(dependsOnGroups = {"init"})
public void runTest() {
    System.out.println("Test running");
}
```

**alwaysRun**:
```java
@Test
public void test1() {
    Assert.fail("Intentional failure");
}

@Test(dependsOnMethods = {"test1"})
public void test2() {
    // Skipped
}

@Test(dependsOnMethods = {"test1"}, alwaysRun = true)
public void test3() {
    // Runs even though test1 failed
}
```

**Use Cases**:
- Sequential workflow tests
- Cleanup operations (alwaysRun)
- Test prerequisites

**In-depth Explanation**:
Dependencies help manage test order but can make debugging harder. Use sparingly, prefer independent tests when possible.

---

### Q12: How do you prioritize tests in TestNG?

**Answer**:
Use `priority` attribute to control execution order.

**Syntax**:
```java
@Test(priority = 1)
public void highPriorityTest() {
    System.out.println("Runs first");
}

@Test(priority = 2)
public void mediumPriorityTest() {
    System.out.println("Runs second");
}

@Test(priority = 3)
public void lowPriorityTest() {
    System.out.println("Runs third");
}

@Test  // No priority = 0 (default)
public void defaultTest() {
    System.out.println("Runs before priority 1");
}
```

**Practical Example**:
```java
@Test(priority = 1)
public void login() {
    loginPage.login();
}

@Test(priority = 2)
public void addToCart() {
    productPage.addProduct();
}

@Test(priority = 3)
public void checkout() {
    checkoutPage.completeOrder();
}
```

**Important Notes**:
- Lower number = higher priority
- Default priority = 0
- Tests with same priority run alphabetically
- Priority doesn't guarantee order in parallel execution

**In-depth Explanation**:
Priority is useful for workflow tests but makes tests interdependent. Consider using dependencies instead for clearer relationships.

---

### Q13: How do you enable parallel execution in TestNG?

**Answer**:
Configure parallel execution in testng.xml using `parallel` and `thread-count` attributes.

**Parallel Levels**:

**1. Parallel Methods**:
```xml
<suite name="Suite" parallel="methods" thread-count="3">
    <test name="Test">
        <classes>
            <class name="com.tests.TestClass"/>
        </classes>
    </test>
</suite>
```
All @Test methods run in parallel (max 3 threads).

**2. Parallel Classes**:
```xml
<suite name="Suite" parallel="classes" thread-count="2">
    <test name="Test">
        <classes>
            <class name="com.tests.LoginTest"/>
            <class name="com.tests.CheckoutTest"/>
        </classes>
    </test>
</suite>
```
Classes run in parallel.

**3. Parallel Tests**:
```xml
<suite name="Suite" parallel="tests" thread-count="2">
    <test name="Chrome Test">
        <classes><class name="com.tests.Test"/></classes>
    </test>
    <test name="Firefox Test">
        <classes><class name="com.tests.Test"/></classes>
    </test>
</suite>
```

**Thread Safety**:
```java
// Use ThreadLocal for thread-safe WebDriver
private static ThreadLocal<WebDriver> driver = new ThreadLocal<>();

@BeforeMethod
public void setup() {
    driver.set(new ChromeDriver());
}

public static WebDriver getDriver() {
    return driver.get();
}

@AfterMethod
public void teardown() {
    getDriver().quit();
    driver.remove();
}
```

**Benefits**:
- Faster execution
- Better resource utilization
- Reduced total test time

---

### Q14: What are listeners in TestNG?

**Answer**:
Listeners "listen" to test events and execute custom logic (logging, screenshots, reporting).

**Common Listeners**:

**1. ITestListener** (most common):
```java
import org.testng.ITestListener;
import org.testng.ITestResult;

public class TestListener implements ITestListener {

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
        takeScreenshot(result);
    }

    @Override
    public void onTestSkipped(ITestResult result) {
        System.out.println("Test Skipped: " + result.getName());
    }

    private void takeScreenshot(ITestResult result) {
        // Screenshot logic
    }
}
```

**Using Listener**:

Method 1 - Annotation:
```java
@Listeners(TestListener.class)
public class LoginTest {
    @Test
    public void test() { }
}
```

Method 2 - testng.xml:
```xml
<suite name="Suite">
    <listeners>
        <listener class-name="com.listeners.TestListener"/>
    </listeners>
    <test name="Test">
        <classes><class name="com.tests.LoginTest"/></classes>
    </test>
</suite>
```

**2. IRetryAnalyzer** (retry failed tests):
```java
public class RetryAnalyzer implements IRetryAnalyzer {
    private int retryCount = 0;
    private static final int maxRetryCount = 2;

    @Override
    public boolean retry(ITestResult result) {
        if (retryCount < maxRetryCount) {
            retryCount++;
            return true;  // Retry
        }
        return false;  // Don't retry
    }
}

// Use in test
@Test(retryAnalyzer = RetryAnalyzer.class)
public void flakyTest() { }
```

**Use Cases**:
- Auto screenshots on failure
- Custom logging
- Retry flaky tests
- Email notifications
- Custom reporting

---

### Q15: How do you handle flaky tests in TestNG?

**Answer**:
Use IRetryAnalyzer to automatically retry failed tests.

**Implementation**:
```java
import org.testng.IRetryAnalyzer;
import org.testng.ITestResult;

public class RetryAnalyzer implements IRetryAnalyzer {

    private int retryCount = 0;
    private static final int maxRetryCount = 2;

    @Override
    public boolean retry(ITestResult result) {
        if (!result.isSuccess()) {
            if (retryCount < maxRetryCount) {
                System.out.println("Retrying test: " + result.getName() +
                        " Attempt: " + (retryCount + 1));
                retryCount++;
                return true;
            }
        }
        return false;
    }
}
```

**Usage**:
```java
@Test(retryAnalyzer = RetryAnalyzer.class)
public void flakyNetworkTest() {
    // Test that occasionally fails due to network
}
```

**Global Retry** (all tests):
```java
public class RetryListener implements IAnnotationTransformer {
    @Override
    public void transform(ITestAnnotation annotation, Class testClass,
                          Constructor testConstructor, Method testMethod) {
        annotation.setRetryAnalyzer(RetryAnalyzer.class);
    }
}

// Add to testng.xml
<listeners>
    <listener class-name="com.listeners.RetryListener"/>
</listeners>
```

**Best Practices**:
- Limit retry count (2-3 max)
- Log retry attempts
- Fix flaky tests rather than rely on retries
- Use for genuinely flaky scenarios (network, timing)

---

### Q16: How do you integrate TestNG with Selenium?

**Answer**:
TestNG provides structure for Selenium tests: setup/teardown, assertions, parameterization, reporting.

**Basic Integration**:
```java
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.testng.annotations.*;
import org.testng.Assert;

public class SeleniumTestNGTest {

    WebDriver driver;

    @BeforeClass
    public void setup() {
        WebDriverManager.chromedriver().setup();
        driver = new ChromeDriver();
        driver.manage().window().maximize();
    }

    @Test(priority = 1)
    public void testLogin() {
        driver.get("https://www.saucedemo.com");
        driver.findElement(By.id("user-name")).sendKeys("standard_user");
        driver.findElement(By.id("password")).sendKeys("secret_sauce");
        driver.findElement(By.id("login-button")).click();

        String url = driver.getCurrentUrl();
        Assert.assertTrue(url.contains("inventory"), "Should navigate to inventory page");
    }

    @Test(priority = 2, dependsOnMethods = {"testLogin"})
    public void testAddToCart() {
        driver.findElement(By.id("add-to-cart-sauce-labs-backpack")).click();

        String cartBadge = driver.findElement(By.className("shopping_cart_badge")).getText();
        Assert.assertEquals(cartBadge, "1", "Cart should have 1 item");
    }

    @AfterClass
    public void teardown() {
        if (driver != null) {
            driver.quit();
        }
    }
}
```

**Cross-Browser with Parameters**:
```java
@Parameters("browser")
@BeforeClass
public void setup(String browser) {
    if (browser.equals("chrome")) {
        driver = new ChromeDriver();
    } else if (browser.equals("firefox")) {
        driver = new FirefoxDriver();
    }
}
```

```xml
<suite name="Cross Browser Suite">
    <test name="Chrome">
        <parameter name="browser" value="chrome"/>
        <classes><class name="com.tests.Test"/></classes>
    </test>
    <test name="Firefox">
        <parameter name="browser" value="firefox"/>
        <classes><class name="com.tests.Test"/></classes>
    </test>
</suite>
```

**Benefits**:
- Test lifecycle management
- Assertions for verification
- Parallel execution for faster results
- Built-in reporting
- Data-driven testing

---

### Q17: How do you generate reports in TestNG?

**Answer**:
TestNG generates built-in reports automatically. Can also integrate third-party tools.

**Built-in Reports**:
After test execution, check `test-output` folder:
1. **index.html**: Main HTML report with detailed results
2. **emailable-report.html**: Summary report for email
3. **testng-results.xml**: Machine-readable XML results

**Customizing Reports**:
```xml
<suite name="Custom Suite Name">  <!-- Appears in report -->
    <test name="Custom Test Name">
        <!-- tests -->
    </test>
</suite>
```

**ExtentReports Integration** (popular):
```xml
<!-- pom.xml -->
<dependency>
    <groupId>com.aventstack</groupId>
    <artifactId>extentreports</artifactId>
    <version>5.0.9</version>
</dependency>
```

```java
import com.aventstack.extentreports.*;
import com.aventstack.extentreports.reporter.ExtentSparkReporter;

public class ExtentManager {
    private static ExtentReports extent;

    public static ExtentReports getInstance() {
        if (extent == null) {
            ExtentSparkReporter htmlReporter =
                    new ExtentSparkReporter("extent-report.html");
            extent = new ExtentReports();
            extent.attachReporter(htmlReporter);
        }
        return extent;
    }
}

// In test
ExtentTest test = ExtentManager.getInstance().createTest("Login Test");
test.pass("Login successful");
test.fail("Login failed");
ExtentManager.getInstance().flush();
```

**Allure Reports** (another option):
- Beautiful, detailed reports
- Trends over time
- Test history

---

### Q18: How do you handle test configuration in TestNG?

**Answer**:
TestNG offers multiple configuration options for flexibility.

**1. testng.xml Configuration**:
```xml
<suite name="Suite" verbose="1">
    <parameter name="env" value="staging"/>

    <test name="Test" preserve-order="true">
        <classes>
            <class name="com.tests.Test"/>
        </classes>
    </test>
</suite>
```

**2. System Properties**:
```java
@Test
public void test() {
    String env = System.getProperty("env", "dev");
    System.out.println("Environment: " + env);
}
```

Run with: `mvn test -Denv=production`

**3. Properties File**:
```properties
# config.properties
browser=chrome
url=https://staging.app.com
timeout=10
```

```java
Properties prop = new Properties();
FileInputStream fis = new FileInputStream("config.properties");
prop.load(fis);

String browser = prop.getProperty("browser");
String url = prop.getProperty("url");
```

**4. Environment Variables**:
```java
String apiKey = System.getenv("API_KEY");
```

**Best Practice**: Use testng.xml for test organization, properties files for environment config, system properties for CI/CD overrides.

---

### Q19: How do you skip tests in TestNG?

**Answer**:
Multiple ways to skip tests based on different scenarios.

**1. enabled = false** (permanent skip):
```java
@Test(enabled = false)
public void skipThisTest() {
    // Will not execute
}
```

**2. throw SkipException** (conditional skip):
```java
@Test
public void conditionalTest() {
    String env = System.getProperty("env");

    if (env.equals("production")) {
        throw new SkipException("Skipping test in production");
    }

    // Test logic
}
```

**3. Assumptions** (skip if condition not met):
```java
import org.testng.SkipException;

@Test
public void databaseTest() {
    boolean dbAvailable = checkDatabaseConnection();

    if (!dbAvailable) {
        throw new SkipException("Database not available");
    }

    // Database test logic
}
```

**4. Groups (skip entire group)**:
```xml
<suite name="Suite">
    <test name="Test">
        <groups>
            <run>
                <include name="smoke"/>
                <exclude name="slow"/>  <!-- Skip slow tests -->
            </run>
        </groups>
        <classes>
            <class name="com.tests.AllTests"/>
        </classes>
    </test>
</suite>
```

**5. Exclude in testng.xml**:
```xml
<classes>
    <class name="com.tests.LoginTest">
        <methods>
            <exclude name="testOAuthLogin"/>  <!-- Skip this method -->
        </methods>
    </class>
</classes>
```

---

### Q20: What is the difference between hard assertion and soft assertion?

**Answer**:

| Aspect | Hard Assertion | Soft Assertion |
|--------|----------------|----------------|
| Class | Assert | SoftAssert |
| Stops on failure | Yes | No |
| Reports immediately | Yes | At assertAll() |
| Use case | Single critical check | Multiple validations |
| Example | Login verification | UI validation (multiple elements) |

**Hard Assertion**:
```java
@Test
public void testHard() {
    Assert.assertEquals(2 + 2, 4);  // Pass
    Assert.assertTrue(5 > 10);      // Fail, stops here
    Assert.assertNotNull("test");   // Never reaches here
}
```

**Soft Assertion**:
```java
@Test
public void testSoft() {
    SoftAssert soft = new SoftAssert();

    soft.assertEquals(2 + 2, 4);    // Pass
    soft.assertTrue(5 > 10);        // Fail, continues
    soft.assertNotNull("test");     // Executes
    soft.assertEquals("a", "b");    // Fail, continues

    soft.assertAll();  // Reports all failures at end
}
```

**When to Use**:
- **Hard**: Login, critical flow, dependencies
- **Soft**: UI validation, multiple independent checks, comprehensive testing

**Important**: Always call `softAssert.assertAll()` or failures won't fail the test.

---

## Section 3: Best Practices & Troubleshooting (Questions 21-25)

### Q21: What are TestNG best practices?

**Answer**:

**1. Test Independence**: Each test should run standalone
```java
// Good - independent
@BeforeMethod
public void setup() {
    driver.get("https://app.com");
    loginPage.login("user", "pass");
}

// Bad - dependent on previous test
@Test
public void test2() {
    // Assumes test1 already logged in
}
```

**2. Use Appropriate Annotations**:
- @BeforeClass: Expensive setup (driver initialization)
- @BeforeMethod: Test isolation (fresh state)
- @AfterClass: Cleanup (close resources)

**3. Meaningful Test Names**:
```java
// Good
@Test
public void testValidLoginWithCorrectCredentials() { }

// Bad
@Test
public void test1() { }
```

**4. Use Assertions with Messages**:
```java
Assert.assertTrue(element.isDisplayed(), "Element should be visible after login");
```

**5. Group Tests Logically**:
```java
@Test(groups = {"smoke", "login"})
```

**6. Externalize Test Data**:
```java
@DataProvider
public Object[][] getData() {
    // Read from Excel, CSV, database
}
```

**7. Use testng.xml for Organization**:
- Group related tests
- Configure parallel execution
- Set parameters

**8. Implement Listeners**:
- Screenshots on failure
- Custom logging
- Retry logic

**9. Keep Tests Fast**:
- Use parallel execution
- Avoid unnecessary waits
- Mock external dependencies

**10. Regular Maintenance**:
- Update locators
- Remove obsolete tests
- Refactor duplicated code

---

### Q22: How do you debug failing TestNG tests?

**Answer**:

**1. Check Test Output**:
- Console logs
- test-output/index.html
- Stack traces

**2. Take Screenshots on Failure**:
```java
public class TestListener implements ITestListener {
    @Override
    public void onTestFailure(ITestResult result) {
        WebDriver driver = ((BaseTest) result.getInstance()).driver;
        TakesScreenshot ts = (TakesScreenshot) driver;
        File screenshot = ts.getScreenshotAs(OutputType.FILE);
        // Save screenshot
    }
}
```

**3. Add Detailed Logging**:
```java
@Test
public void test() {
    System.out.println("Step 1: Navigating to page");
    driver.get(url);

    System.out.println("Step 2: Entering username");
    driver.findElement(By.id("username")).sendKeys("user");

    // More logging
}
```

**4. Run Single Test**:
```xml
<classes>
    <class name="com.tests.LoginTest">
        <methods>
            <include name="testValidLogin"/>  <!-- Only this test -->
        </methods>
    </class>
</classes>
```

**5. Use Debug Mode**:
- Set breakpoints
- Step through code
- Inspect variables

**6. Check Execution Order**:
- Verify priority settings
- Check dependencies
- Review testng.xml configuration

**7. Validate Test Data**:
- Check DataProvider values
- Verify parameters from testng.xml

**8. Thread Safety Issues** (parallel execution):
- Ensure ThreadLocal for WebDriver
- Check shared resources

---

### Q23: How do you integrate TestNG with Maven?

**Answer**:

**1. Add TestNG Dependency**:
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

**2. Configure Surefire Plugin**:
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

**3. Run Tests**:
```bash
mvn clean test                    # Run all tests
mvn test -Dtest=LoginTest         # Run specific class
mvn test -Dgroups=smoke           # Run specific group
```

**4. Multiple testng.xml Files**:
```xml
<configuration>
    <suiteXmlFiles>
        <suiteXmlFile>smoke-tests.xml</suiteXmlFile>
        <suiteXmlFile>regression-tests.xml</suiteXmlFile>
    </suiteXmlFiles>
</configuration>
```

**5. Pass Parameters**:
```bash
mvn test -Dbrowser=chrome -Denv=staging
```

**6. Reports Location**:
- Maven: `target/surefire-reports/`
- TestNG: `test-output/`

---

### Q24: How do you handle parallel execution issues in TestNG?

**Answer**:

**Common Issues**:

**1. WebDriver Thread Safety**:
```java
// Problem: Shared driver instance
WebDriver driver;  // Not thread-safe

// Solution: ThreadLocal
private static ThreadLocal<WebDriver> driver = new ThreadLocal<>();

@BeforeMethod
public void setup() {
    driver.set(new ChromeDriver());
}

public static WebDriver getDriver() {
    return driver.get();
}

@AfterMethod
public void teardown() {
    getDriver().quit();
    driver.remove();  // Important!
}
```

**2. Shared Data Issues**:
```java
// Problem: Static shared variable
public static String username;  // Not thread-safe

// Solution: Test-level data
@BeforeMethod
public void setup() {
    String username = "user_" + Thread.currentThread().getId();
}
```

**3. Resource Conflicts**:
```java
// Problem: Same test data for all threads
@DataProvider
public Object[][] getData() {
    return new Object[][] {
        {"user1", "pass1"}  // Same for all threads
    };
}

// Solution: Unique data per iteration
@DataProvider
public Object[][] getData() {
    return new Object[][] {
        {"user1", "pass1"},
        {"user2", "pass2"},
        {"user3", "pass3"}
    };
}
```

**4. Synchronization**:
```java
// For shared resources
private static synchronized void updateSharedResource() {
    // Thread-safe operation
}
```

**Best Practices**:
- Use ThreadLocal for WebDriver
- Avoid shared mutable state
- Unique test data per thread
- Proper resource cleanup

---

### Q25: What is the execution order in TestNG without priority?

**Answer**:

**Without Priority**:
- Tests execute in alphabetical order (method names)
- Not guaranteed, depends on reflection API

**Example**:
```java
@Test
public void zebra() {
    System.out.println("Zebra");
}

@Test
public void apple() {
    System.out.println("Apple");
}

@Test
public void mango() {
    System.out.println("Mango");
}

/* Likely output (not guaranteed):
Apple
Mango
Zebra
*/
```

**With Priority**:
```java
@Test(priority = 3)
public void zebra() {
    System.out.println("Zebra - Priority 3");
}

@Test(priority = 1)
public void apple() {
    System.out.println("Apple - Priority 1");
}

@Test(priority = 2)
public void mango() {
    System.out.println("Mango - Priority 2");
}

/* Output (guaranteed):
Apple - Priority 1
Mango - Priority 2
Zebra - Priority 3
*/
```

**Recommendation**: Don't rely on alphabetical order. Use priority or dependencies for specific order.

---

## Summary: Key Interview Points

### Must-Know Concepts:
1. **TestNG vs JUnit**: Features, advantages
2. **Annotations**: Hierarchy, use cases
3. **testng.xml**: Configuration, grouping, parallel
4. **Parameterization**: @Parameters, @DataProvider
5. **Assertions**: Hard vs Soft
6. **Grouping**: Organization, selective execution
7. **Dependencies**: Method and group dependencies
8. **Parallel Execution**: Configuration, thread safety
9. **Listeners**: ITestListener, IRetryAnalyzer
10. **Integration**: Selenium, Maven, CI/CD
11. **Best Practices**: Independence, organization, reporting

### Interview Tips:
- Provide practical examples from projects
- Explain "when" and "why" to use features
- Show understanding of thread safety for parallel execution
- Mention real challenges faced and solutions
- Be ready to write code samples
- Know differences from JUnit

Good luck with your TestNG interviews!
