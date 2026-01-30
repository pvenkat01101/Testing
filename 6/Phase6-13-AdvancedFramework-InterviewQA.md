# Phase 6-13: Advanced Framework Development - Interview Q&A

## Table of Contents
1. [Framework Architecture](#framework-architecture)
2. [Reporting - Extent vs Allure](#reporting---extent-vs-allure)
3. [Retry Mechanism](#retry-mechanism)
4. [Listeners](#listeners)
5. [Exception Handling](#exception-handling)
6. [Cross-Browser Strategy](#cross-browser-strategy)
7. [Environment Management](#environment-management)
8. [CI/CD Integration](#cicd-integration)

---

## Framework Architecture

### Q1: Explain the architecture of your automation framework.

**Answer:**

Our framework follows a **hybrid framework** design that combines the Page Object Model (POM) with data-driven and keyword-driven elements. The architecture has these layers:

```
┌─────────────────────────────────────────────┐
│              Test Layer (TestNG)             │
│  - Test classes organized by module          │
│  - @Test methods with assertions             │
├─────────────────────────────────────────────┤
│           Business Logic / Steps             │
│  - Reusable workflows (login, checkout)      │
├─────────────────────────────────────────────┤
│           Page Object Layer (POM)            │
│  - One class per page/component              │
│  - Locators + action methods                 │
├─────────────────────────────────────────────┤
│            Utilities Layer                   │
│  - WaitUtils, ScreenshotUtil, ExcelReader    │
├─────────────────────────────────────────────┤
│            Core / Base Layer                 │
│  - BaseTest, BrowserFactory, ConfigReader    │
├─────────────────────────────────────────────┤
│           Reporting & Listeners              │
│  - ExtentReports, RetryAnalyzer, Listeners   │
├─────────────────────────────────────────────┤
│           Configuration Layer                │
│  - config-qa.properties, testng.xml          │
└─────────────────────────────────────────────┘
```

**Key design decisions:**
- **ThreadLocal WebDriver** for parallel execution
- **Factory Pattern** for browser creation
- **Singleton Pattern** for report and config managers
- **Properties files per environment** for environment-specific data
- **TestNG XML** for suite configuration, grouping, and parameterisation

---

### Q2: What design patterns do you use in your framework?

**Answer:**

| Pattern | Where Used | Purpose |
|---------|-----------|---------|
| **Page Object Model** | Page classes | Separate locators and actions from tests |
| **Factory** | BrowserFactory | Create WebDriver instances without exposing creation logic |
| **Singleton** | ExtentReportManager, ConfigReader | Single shared instance across the suite |
| **Strategy** | Different browser strategies | Swap browser implementations at runtime |
| **Builder** | Fluent page methods returning `this` | Chain actions for readable test code |
| **ThreadLocal** | BrowserFactory driver storage | Thread-safe parallel execution |

---

### Q3: How do you handle test data in your framework?

**Answer:**

We use multiple test data approaches depending on the scenario:

1. **Properties files** -- environment-specific configuration (URLs, timeouts)
2. **Excel files (Apache POI)** -- large data sets for data-driven tests via `@DataProvider`
3. **JSON / YAML files** -- complex, structured test data (API payloads, user profiles)
4. **TestNG DataProvider** -- inline data or data read from files
5. **Database queries** -- dynamic data that must be current

```java
@DataProvider(name = "loginData")
public Object[][] getLoginData() throws IOException {
    return ExcelReader.readSheet("src/test/resources/testdata/LoginData.xlsx", "Sheet1");
}

@Test(dataProvider = "loginData")
public void testLogin(String username, String password, String expected) {
    LoginPage loginPage = new LoginPage(driver);
    loginPage.login(username, password);
    Assert.assertEquals(loginPage.getMessage(), expected);
}
```

---

### Q4: How do you manage WebDriver instances in a parallel execution environment?

**Answer:**

We use **ThreadLocal** to ensure each thread gets its own WebDriver instance:

```java
private static ThreadLocal<WebDriver> driverThread = new ThreadLocal<>();

public static WebDriver createDriver(String browser) {
    WebDriver driver = // ... create based on browser
    driverThread.set(driver);
    return driver;
}

public static WebDriver getDriver() {
    return driverThread.get();
}

public static void quitDriver() {
    WebDriver d = driverThread.get();
    if (d != null) { d.quit(); driverThread.remove(); }
}
```

**Why ThreadLocal?**
- Each parallel thread stores its driver independently
- No race conditions or shared-state issues
- `remove()` prevents memory leaks

---

## Reporting - Extent vs Allure

### Q5: What reporting tools have you used? Compare Extent Reports and Allure Reports.

**Answer:**

| Feature | Extent Reports | Allure Reports |
|---------|---------------|----------------|
| **Setup** | Simple Maven dependency | Requires Allure CLI + plugin |
| **Integration** | Manual via listeners | Annotation-based (`@Step`, `@Attachment`) |
| **Screenshots** | Base64 or file-path embedding | `@Attachment` annotation |
| **Dashboard** | Built-in single HTML file | Separate server or CI plugin |
| **Customisation** | Programmatic API (themes, filters) | Categories, environments, history |
| **CI/CD** | Copy HTML artifact | Jenkins/GitLab Allure plugins |
| **Parallel support** | Thread-safe with ThreadLocal map | Natively thread-safe |
| **Learning curve** | Easy | Moderate |

**When to choose Extent:** simpler projects, quick setup, self-contained HTML reports.

**When to choose Allure:** enterprise projects with CI/CD dashboards, trend analysis, and step-level detail.

---

### Q6: How do you attach screenshots to reports on test failure?

**Answer:**

Using a TestNG listener:

```java
@Override
public void onTestFailure(ITestResult result) {
    WebDriver driver = ((BaseTest) result.getInstance()).getDriver();
    String base64 = ((TakesScreenshot) driver).getScreenshotAs(OutputType.BASE64);

    ExtentReportManager.getTest().fail("Screenshot",
        MediaEntityBuilder.createScreenCaptureFromBase64String(base64).build());
}
```

**Why Base64 over file paths?**
- Report is self-contained (no broken image links when moving the HTML file)
- Works across CI/CD agents regardless of file system paths

---

## Retry Mechanism

### Q7: How do you handle flaky tests?

**Answer:**

We use TestNG's `IRetryAnalyzer` to automatically re-run failed tests:

```java
public class RetryAnalyzer implements IRetryAnalyzer {
    private int count = 0;
    private static final int MAX = 2;

    @Override
    public boolean retry(ITestResult result) {
        if (count < MAX) { count++; return true; }
        return false;
    }
}
```

**Applying globally** via `IAnnotationTransformer`:

```java
public class RetryTransformer implements IAnnotationTransformer {
    @Override
    public void transform(ITestAnnotation annotation, Class cls,
                          Constructor ctr, Method method) {
        annotation.setRetryAnalyzer(RetryAnalyzer.class);
    }
}
```

**Important:** Retries hide real bugs. We treat retries as a safety net and track flaky-test metrics. If a test fails consistently, we fix the test or the application -- not just increase the retry count.

---

### Q8: How do you ensure retried tests are accurately reflected in reports?

**Answer:**

By default, TestNG marks the original failed attempts as "skipped" and only the final attempt as passed or failed. In the Extent Report listener:

- `onTestFailure` -- log the failure and screenshot (this runs on every failed attempt)
- `onTestSkipped` -- log the skip (previous retry attempts)
- After retries are exhausted and the test still fails, the final failure is the one that matters

Some teams also use a custom `onFinish` to remove duplicate skipped results:

```java
@Override
public void onFinish(ITestContext context) {
    // Remove retried-then-passed tests from the "skipped" set
    context.getSkippedTests().getAllResults().removeIf(r ->
        context.getPassedTests().getAllResults().stream()
            .anyMatch(p -> p.getMethod().equals(r.getMethod()))
    );
    ExtentReportManager.flushReport();
}
```

---

## Listeners

### Q9: What TestNG listeners have you implemented and why?

**Answer:**

| Listener | Purpose |
|----------|---------|
| `ITestListener` | Log test start, pass, fail, skip; attach screenshots to reports |
| `ISuiteListener` | Global setup (report init) and teardown (send email, flush report) |
| `IAnnotationTransformer` | Dynamically attach RetryAnalyzer to all tests |
| `IInvokedMethodListener` | Log before/after every method (including config methods) for debugging |
| `IReporter` | Generate custom reports after the suite |

---

### Q10: How do you register listeners? What are the different approaches?

**Answer:**

Three approaches, in order of preference:

1. **testng.xml** (recommended for production suites):
   ```xml
   <listeners>
       <listener class-name="com.framework.listeners.TestListener"/>
   </listeners>
   ```

2. **@Listeners annotation** (useful for quick, class-specific registration):
   ```java
   @Listeners(TestListener.class)
   public class LoginTest { ... }
   ```

3. **Service Loader** (auto-discovery without XML or annotation):
   - Create file `META-INF/services/org.testng.ITestNGListener`
   - Add the fully-qualified class name of the listener

**Best practice:** Use testng.xml for CI/CD because it is the single source of truth for suite configuration.

---

## Exception Handling

### Q11: How do you handle exceptions in your framework?

**Answer:**

We use a layered approach:

1. **Custom exceptions** for framework-specific errors:
   ```java
   public class PageLoadException extends RuntimeException {
       public PageLoadException(String page, long timeout) {
           super("Page '" + page + "' did not load within " + timeout + " seconds");
       }
   }

   public class ElementNotFoundException extends RuntimeException {
       public ElementNotFoundException(String locator) {
           super("Element not found: " + locator);
       }
   }
   ```

2. **Try-catch in utilities**, not in tests:
   ```java
   // In WaitUtils
   public static WebElement waitForElement(WebDriver driver, By locator, int seconds) {
       try {
           return new WebDriverWait(driver, Duration.ofSeconds(seconds))
                   .until(ExpectedConditions.visibilityOfElementLocated(locator));
       } catch (TimeoutException e) {
           throw new ElementNotFoundException(locator.toString());
       }
   }
   ```

3. **Listeners catch unhandled exceptions** and log them with screenshots.

4. **Soft assertions** when multiple checks are needed in a single test:
   ```java
   SoftAssert soft = new SoftAssert();
   soft.assertEquals(actual1, expected1, "Check 1 failed");
   soft.assertEquals(actual2, expected2, "Check 2 failed");
   soft.assertAll(); // Reports all failures at once
   ```

---

### Q12: What is the difference between hard assert and soft assert?

**Answer:**

| Aspect | Hard Assert | Soft Assert |
|--------|------------|-------------|
| **Behaviour on failure** | Immediately throws `AssertionError`, stops the test | Records the failure, continues the test |
| **When to use** | Critical checks (e.g., page loaded, login succeeded) | Multiple independent validations in one test |
| **Syntax** | `Assert.assertEquals(a, b)` | `softAssert.assertEquals(a, b)` then `softAssert.assertAll()` |
| **Reporting** | Only first failure visible | All failures visible at once |

---

## Cross-Browser Strategy

### Q13: How do you implement cross-browser testing?

**Answer:**

Our strategy has three layers:

1. **Browser Factory** -- creates the correct driver based on a string parameter
2. **TestNG parameterisation** -- testng.xml defines browser per `<test>` block
3. **Maven/CLI override** -- `-Dbrowser=firefox` for CI pipelines

```xml
<suite name="Cross Browser" parallel="tests" thread-count="3">
    <test name="Chrome">
        <parameter name="browser" value="chrome"/>
        <classes><class name="com.tests.LoginTest"/></classes>
    </test>
    <test name="Firefox">
        <parameter name="browser" value="firefox"/>
        <classes><class name="com.tests.LoginTest"/></classes>
    </test>
</suite>
```

**For large-scale cross-browser:**
- **Selenium Grid** -- run against a hub with multiple node VMs
- **Cloud platforms** -- BrowserStack, Sauce Labs, LambdaTest (RemoteWebDriver with capabilities)

```java
if (ConfigReader.getProperty("execution.mode").equals("remote")) {
    URL hubUrl = new URL(ConfigReader.getProperty("grid.url"));
    driver = new RemoteWebDriver(hubUrl, capabilities);
} else {
    driver = new ChromeDriver(options);
}
```

---

## Environment Management

### Q14: How do you manage multiple test environments?

**Answer:**

We maintain separate properties files per environment:

```
config-qa.properties      --> base.url=https://qa.example.com
config-staging.properties --> base.url=https://staging.example.com
config-prod.properties    --> base.url=https://www.example.com
```

The active environment is selected via a system property:

```bash
mvn clean test -Denv=staging
```

`ConfigReader` loads the correct file at startup:

```java
String env = System.getProperty("env", "qa");
String path = "config/config-" + env + ".properties";
properties.load(new FileInputStream(path));
```

**Priority order for any property:**
1. System property (`-Dbase.url=...`)
2. Environment-specific properties file
3. Default properties file
4. Hard-coded default in code

---

## CI/CD Integration

### Q15: How do you integrate your framework with CI/CD pipelines?

**Answer:**

**Jenkins Pipeline example (Jenkinsfile):**

```groovy
pipeline {
    agent any

    parameters {
        choice(name: 'ENV', choices: ['qa', 'staging', 'prod'], description: 'Target environment')
        choice(name: 'BROWSER', choices: ['chrome-headless', 'firefox-headless'], description: 'Browser')
    }

    stages {
        stage('Checkout') {
            steps { git branch: 'main', url: 'https://github.com/org/automation.git' }
        }
        stage('Build & Test') {
            steps {
                sh "mvn clean test -Denv=${params.ENV} -Dbrowser=${params.BROWSER}"
            }
        }
        stage('Publish Report') {
            steps {
                publishHTML(target: [
                    reportDir: 'test-output',
                    reportFiles: '*.html',
                    reportName: 'Extent Report'
                ])
            }
        }
    }

    post {
        always { archiveArtifacts artifacts: 'test-output/**' }
        failure { emailext subject: 'Tests Failed', body: 'Check Jenkins', to: 'team@example.com' }
    }
}
```

**GitHub Actions example:**

```yaml
name: Automation Tests
on:
  push:
    branches: [main]
  schedule:
    - cron: '0 6 * * *'  # daily at 6 AM UTC

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        browser: [chrome-headless, firefox-headless]
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-java@v4
        with:
          java-version: '17'
          distribution: 'temurin'
      - name: Run Tests
        run: mvn clean test -Denv=qa -Dbrowser=${{ matrix.browser }}
      - name: Upload Report
        if: always()
        uses: actions/upload-artifact@v4
        with:
          name: test-report-${{ matrix.browser }}
          path: test-output/
```

---

### Q16: What are the best practices for running tests in CI/CD?

**Answer:**

1. **Headless browsers** -- no GUI needed on CI agents
2. **Parallel execution** -- reduce total run time
3. **Retry flaky tests** -- 1-2 retries maximum; track flaky-test ratio
4. **Independent tests** -- no test should depend on another test's state
5. **Clean state** -- start each test with a fresh browser and known data
6. **Fail fast for smoke, complete for regression** -- separate suite configurations
7. **Artifact archiving** -- always upload reports and screenshots regardless of result
8. **Notifications** -- email, Slack, or Teams alerts on failure
9. **Environment isolation** -- use dedicated QA environments, not shared developer instances
10. **Version-controlled config** -- testng.xml, pom.xml, Jenkinsfile all in the repo

---

### Q17: How do you handle test data in CI/CD?

**Answer:**

- **Static data** (usernames, URLs) -- properties files checked into version control
- **Dynamic data** -- generated at runtime or fetched from APIs
- **Sensitive data** (passwords, tokens) -- stored in CI/CD secrets (Jenkins Credentials, GitHub Secrets), injected as environment variables:
  ```bash
  mvn clean test -Dadmin.password=$ADMIN_PASSWORD
  ```
- **Database seeding** -- a `@BeforeSuite` hook runs SQL scripts or API calls to prepare data
- **Test isolation** -- each test creates its own data and cleans up afterward (or uses unique identifiers)

---

### Q18: Explain your framework's logging strategy.

**Answer:**

We use **Log4j2** for structured logging:

```java
import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;

public class LoginPage {
    private static final Logger log = LogManager.getLogger(LoginPage.class);

    public void login(String user, String pass) {
        log.info("Logging in as: {}", user);
        usernameField.sendKeys(user);
        passwordField.sendKeys("****"); // never log real passwords
        loginButton.click();
        log.info("Login button clicked");
    }
}
```

**Logging levels:**
| Level | Usage |
|-------|-------|
| ERROR | Test infrastructure failures (driver crash, config missing) |
| WARN | Retried operations, slow page loads |
| INFO | Test steps, navigation, major actions |
| DEBUG | Element interactions, wait details |
| TRACE | Full request/response payloads (API tests) |

Logs are written to both console and a rolling file (`logs/automation.log`). In CI, the log file is archived alongside the report.

---

## Quick Reference Card

| Topic | Key Concept | Interview Tip |
|-------|------------|--------------|
| Architecture | Layered: Base, Pages, Tests, Utils, Reports | Draw the diagram; mention design patterns |
| Extent Reports | Thread-safe manager + listener + Base64 screenshots | Explain why Base64 over file paths |
| Allure Reports | Annotation-driven, CI dashboard integration | Compare with Extent; mention trends & history |
| Retry | IRetryAnalyzer + IAnnotationTransformer | Emphasise that retries are a safety net, not a fix |
| Listeners | ITestListener for events; ISuiteListener for global setup | Know when each listener fires |
| Exceptions | Custom exceptions + try-catch in utils, not tests | Mention Soft Assert for multi-check scenarios |
| Cross-Browser | Factory + ThreadLocal + testng.xml params | Mention Grid / cloud for scale |
| Environments | Per-env properties + system property override | Show priority order |
| CI/CD | Jenkinsfile / GitHub Actions; headless; artifacts | Show a real pipeline snippet |

> **Next Step:** Phase 6-14 covers BDD with Cucumber -- theory and fundamentals.
