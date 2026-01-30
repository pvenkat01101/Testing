# Phase 6-11: Advanced Framework Development Theory

## Table of Contents
- [Extent Reports](#extent-reports)
- [Allure Reports](#allure-reports)
- [IRetryAnalyzer - Retry Mechanism](#iretryanalyzer---retry-mechanism)
- [TestNG Listeners](#testng-listeners)
- [Exception Handling Patterns](#exception-handling-patterns)
- [Multi-Browser Factory Pattern](#multi-browser-factory-pattern)
- [Environment Management](#environment-management)
- [Email & Slack Notifications](#email--slack-notifications)
- [Summary](#summary)

---

## Extent Reports

### What is Extent Reports?

Extent Reports is a popular open-source reporting library for test automation that generates rich, interactive HTML reports. It supports Java, .NET, and other languages.

**Features:**
- Beautiful HTML reports with charts and statistics
- Test categorization and tagging
- Screenshot and log attachment
- Parallel test support
- Dashboard with pass/fail/skip statistics
- Customizable themes (Standard, Klov)

### Setup and Dependencies

```xml
<!-- pom.xml -->
<dependency>
    <groupId>com.aventstack</groupId>
    <artifactId>extentreports</artifactId>
    <version>5.1.1</version>
</dependency>
```

### Basic Configuration

```java
// reports/ExtentReportManager.java
package reports;

import com.aventstack.extentreports.ExtentReports;
import com.aventstack.extentreports.reporter.ExtentSparkReporter;
import com.aventstack.extentreports.reporter.configuration.Theme;

public class ExtentReportManager {

    private static ExtentReports extent;

    /**
     * Get or create the ExtentReports instance (singleton)
     */
    public static ExtentReports getInstance() {
        if (extent == null) {
            extent = createInstance();
        }
        return extent;
    }

    private static ExtentReports createInstance() {
        String reportPath = System.getProperty("user.dir")
            + "/reports/TestReport_"
            + java.time.LocalDateTime.now()
                .format(java.time.format.DateTimeFormatter.ofPattern("yyyyMMdd_HHmmss"))
            + ".html";

        // Configure the Spark Reporter (HTML Report)
        ExtentSparkReporter sparkReporter = new ExtentSparkReporter(reportPath);
        sparkReporter.config().setTheme(Theme.STANDARD);
        sparkReporter.config().setDocumentTitle("Test Automation Report");
        sparkReporter.config().setReportName("Regression Test Suite");
        sparkReporter.config().setTimeStampFormat("EEEE, MMMM dd, yyyy, hh:mm a '('zzz')'");
        sparkReporter.config().setEncoding("UTF-8");

        // Optional: Configure CSS
        sparkReporter.config().setCss(".badge-primary { background-color: #1976d2; }");

        // Create ExtentReports and attach reporter
        ExtentReports extent = new ExtentReports();
        extent.attachReporter(sparkReporter);

        // System/Environment information
        extent.setSystemInfo("OS", System.getProperty("os.name"));
        extent.setSystemInfo("Java Version", System.getProperty("java.version"));
        extent.setSystemInfo("Browser", System.getProperty("browser", "chrome"));
        extent.setSystemInfo("Environment", System.getProperty("env", "QA"));
        extent.setSystemInfo("Tester", System.getProperty("user.name"));

        return extent;
    }

    /**
     * Flush the report (MUST be called at the end)
     */
    public static void flushReport() {
        if (extent != null) {
            extent.flush();
        }
    }
}
```

### Adding Test Details

```java
// reports/ExtentTestManager.java
package reports;

import com.aventstack.extentreports.ExtentReports;
import com.aventstack.extentreports.ExtentTest;

import java.util.HashMap;
import java.util.Map;

public class ExtentTestManager {

    // Thread-safe test management for parallel execution
    private static final Map<Long, ExtentTest> testMap = new HashMap<>();
    private static final ExtentReports extent = ExtentReportManager.getInstance();

    /**
     * Create a new test entry in the report
     */
    public static synchronized ExtentTest startTest(String testName, String description) {
        ExtentTest test = extent.createTest(testName, description);
        testMap.put(Thread.currentThread().getId(), test);
        return test;
    }

    /**
     * Get the current thread's test
     */
    public static synchronized ExtentTest getTest() {
        return testMap.get(Thread.currentThread().getId());
    }

    /**
     * Remove the current thread's test (cleanup)
     */
    public static synchronized void removeTest() {
        testMap.remove(Thread.currentThread().getId());
    }
}
```

### Using Extent Reports in Tests

```java
// tests/BaseTest.java
package tests;

import com.aventstack.extentreports.ExtentTest;
import com.aventstack.extentreports.Status;
import reports.ExtentReportManager;
import reports.ExtentTestManager;
import org.testng.ITestResult;
import org.testng.annotations.*;

public class BaseTest {

    @BeforeMethod
    public void beforeMethod(java.lang.reflect.Method method) {
        // Create test entry in report
        ExtentTestManager.startTest(
            method.getName(),
            method.getDeclaringClass().getSimpleName() + " - " + method.getName()
        );
    }

    @AfterMethod
    public void afterMethod(ITestResult result) {
        ExtentTest test = ExtentTestManager.getTest();

        switch (result.getStatus()) {
            case ITestResult.SUCCESS:
                test.log(Status.PASS, "Test PASSED: " + result.getName());
                break;

            case ITestResult.FAILURE:
                test.log(Status.FAIL, "Test FAILED: " + result.getName());
                test.log(Status.FAIL, "Cause: " + result.getThrowable().getMessage());
                test.fail(result.getThrowable()); // Attach stack trace
                break;

            case ITestResult.SKIP:
                test.log(Status.SKIP, "Test SKIPPED: " + result.getName());
                if (result.getThrowable() != null) {
                    test.skip(result.getThrowable());
                }
                break;
        }

        ExtentTestManager.removeTest();
    }

    @AfterSuite
    public void afterSuite() {
        ExtentReportManager.flushReport(); // Generate the HTML file
    }
}
```

### Adding Screenshots to Reports

```java
// utils/ScreenshotUtils.java
package utils;

import com.aventstack.extentreports.MediaEntityBuilder;
import com.aventstack.extentreports.Status;
import driver.DriverManager;
import org.openqa.selenium.OutputType;
import org.openqa.selenium.TakesScreenshot;
import reports.ExtentTestManager;
import java.util.Base64;

public class ScreenshotUtils {

    /**
     * Capture screenshot and attach to Extent Report as Base64
     * (Base64 embeds the image directly in the HTML - no external files)
     */
    public static void captureAndAttachScreenshot(String stepDescription) {
        try {
            TakesScreenshot ts = (TakesScreenshot) DriverManager.getDriver();
            String base64Screenshot = ts.getScreenshotAs(OutputType.BASE64);

            ExtentTestManager.getTest().log(
                Status.INFO,
                stepDescription,
                MediaEntityBuilder.createScreenCaptureFromBase64String(base64Screenshot).build()
            );
        } catch (Exception e) {
            ExtentTestManager.getTest().log(Status.WARNING,
                "Failed to capture screenshot: " + e.getMessage());
        }
    }

    /**
     * Capture screenshot on failure
     */
    public static void captureScreenshotOnFailure(String testName) {
        try {
            TakesScreenshot ts = (TakesScreenshot) DriverManager.getDriver();
            String base64Screenshot = ts.getScreenshotAs(OutputType.BASE64);

            ExtentTestManager.getTest().fail(
                "Screenshot at failure point",
                MediaEntityBuilder.createScreenCaptureFromBase64String(base64Screenshot).build()
            );
        } catch (Exception e) {
            ExtentTestManager.getTest().log(Status.WARNING,
                "Could not capture failure screenshot: " + e.getMessage());
        }
    }

    /**
     * Alternative: Save screenshot as file and reference by path
     */
    public static String captureScreenshotAsFile(String testName) {
        try {
            TakesScreenshot ts = (TakesScreenshot) DriverManager.getDriver();
            byte[] screenshot = ts.getScreenshotAs(OutputType.BYTES);

            String filePath = System.getProperty("user.dir")
                + "/reports/screenshots/"
                + testName + "_" + System.currentTimeMillis() + ".png";

            java.nio.file.Files.createDirectories(
                java.nio.file.Path.of(filePath).getParent()
            );
            java.nio.file.Files.write(java.nio.file.Path.of(filePath), screenshot);

            return filePath;
        } catch (Exception e) {
            return null;
        }
    }
}
```

### Adding Logs to Reports

```java
// In test methods
@Test
public void testLogin() {
    ExtentTest test = ExtentTestManager.getTest();

    test.log(Status.INFO, "Navigating to login page");
    getDriver().get("https://example.com/login");

    test.log(Status.INFO, "Entering username");
    getDriver().findElement(By.id("username")).sendKeys("admin");

    test.log(Status.INFO, "Entering password");
    getDriver().findElement(By.id("password")).sendKeys("password");

    test.log(Status.INFO, "Clicking login button");
    getDriver().findElement(By.id("login-btn")).click();

    // Add categorization
    test.assignCategory("Smoke", "Login");
    test.assignAuthor("QA Team");
    test.assignDevice("Chrome - Windows 11");

    test.log(Status.PASS, "Login successful");
}
```

### Extent Report with Multiple Reporters

```java
// Generate both HTML and PDF reports
ExtentSparkReporter htmlReporter = new ExtentSparkReporter("reports/report.html");
htmlReporter.config().setTheme(Theme.DARK);

// JSON reporter for programmatic access
ExtentSparkReporter jsonReporter = new ExtentSparkReporter("reports/report.json");

ExtentReports extent = new ExtentReports();
extent.attachReporter(htmlReporter, jsonReporter);
```

---

## Allure Reports

### What is Allure Reports?

Allure is an open-source test reporting framework designed to create detailed, interactive reports. It integrates with TestNG, JUnit, Cucumber, and many other frameworks.

**Advantages over Extent Reports:**
- Better CI/CD integration (Jenkins plugin, GitHub Actions)
- Built-in trend analysis across builds
- Test categorization by defects, features, stories
- Timeline view for parallel execution analysis
- Automatic retry history tracking

### Setup and Dependencies

```xml
<!-- pom.xml -->
<dependencies>
    <dependency>
        <groupId>io.qameta.allure</groupId>
        <artifactId>allure-testng</artifactId>
        <version>2.25.0</version>
    </dependency>
</dependencies>

<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>3.2.5</version>
            <configuration>
                <argLine>
                    -javaagent:"${settings.localRepository}/org/aspectj/aspectjweaver/1.9.21/aspectjweaver-1.9.21.jar"
                </argLine>
            </configuration>
            <dependencies>
                <dependency>
                    <groupId>org.aspectj</groupId>
                    <artifactId>aspectjweaver</artifactId>
                    <version>1.9.21</version>
                </dependency>
            </dependencies>
        </plugin>
        <plugin>
            <groupId>io.qameta.allure</groupId>
            <artifactId>allure-maven</artifactId>
            <version>2.12.0</version>
        </plugin>
    </plugins>
</build>
```

### Allure Annotations

```java
package tests;

import io.qameta.allure.*;
import org.testng.Assert;
import org.testng.annotations.Test;

@Epic("User Management")
@Feature("Login")
public class AllureLoginTest extends BaseTest {

    @Test
    @Story("Valid Login")
    @Severity(SeverityLevel.CRITICAL)
    @Description("Verify that a user can log in with valid credentials")
    @Owner("QA Team")
    @Link(name = "JIRA", url = "https://jira.company.com/browse/TC-001")
    @TmsLink("TC-001")
    @Issue("BUG-123")
    public void testValidLogin() {
        navigateToLoginPage();
        enterCredentials("admin", "password123");
        clickLoginButton();
        verifyDashboard();
    }

    @Test
    @Story("Invalid Login")
    @Severity(SeverityLevel.NORMAL)
    @Description("Verify error message for invalid credentials")
    public void testInvalidLogin() {
        navigateToLoginPage();
        enterCredentials("wrong", "wrong");
        clickLoginButton();
        verifyErrorMessage("Your username is invalid!");
    }

    // Using @Step annotation for detailed reporting
    @Step("Navigate to login page")
    private void navigateToLoginPage() {
        getDriver().get("https://the-internet.herokuapp.com/login");
    }

    @Step("Enter credentials: username={username}")
    private void enterCredentials(String username, String password) {
        getDriver().findElement(By.id("username")).sendKeys(username);
        getDriver().findElement(By.id("password")).sendKeys(password);
    }

    @Step("Click login button")
    private void clickLoginButton() {
        getDriver().findElement(By.cssSelector("button[type='submit']")).click();
    }

    @Step("Verify dashboard is displayed")
    private void verifyDashboard() {
        String message = getDriver().findElement(By.id("flash")).getText();
        Assert.assertTrue(message.contains("You logged into a secure area!"));
    }

    @Step("Verify error message: {expectedMessage}")
    private void verifyErrorMessage(String expectedMessage) {
        String message = getDriver().findElement(By.id("flash")).getText();
        Assert.assertTrue(message.contains(expectedMessage));
    }
}
```

### Allure Annotations Reference

| Annotation | Purpose | Example |
|------------|---------|---------|
| `@Epic` | Top-level grouping | `@Epic("User Management")` |
| `@Feature` | Feature within an epic | `@Feature("Login")` |
| `@Story` | User story within a feature | `@Story("Valid Login")` |
| `@Step` | Individual test step | `@Step("Click login button")` |
| `@Severity` | Test priority/severity | `@Severity(SeverityLevel.CRITICAL)` |
| `@Description` | Detailed test description | `@Description("Verify login flow")` |
| `@Owner` | Test owner/author | `@Owner("QA Team")` |
| `@Link` | External link | `@Link(name="JIRA", url="...")` |
| `@TmsLink` | Test Management System link | `@TmsLink("TC-001")` |
| `@Issue` | Related bug/issue | `@Issue("BUG-123")` |
| `@Attachment` | Attach files to report | Used programmatically |

### Allure Attachments

```java
import io.qameta.allure.Allure;
import io.qameta.allure.Attachment;

public class AllureAttachmentUtils {

    /**
     * Attach screenshot using @Attachment annotation
     */
    @Attachment(value = "Screenshot", type = "image/png")
    public static byte[] attachScreenshot() {
        return ((TakesScreenshot) DriverManager.getDriver())
            .getScreenshotAs(OutputType.BYTES);
    }

    /**
     * Attach screenshot using Allure lifecycle API
     */
    public static void addScreenshotToAllure(String name) {
        byte[] screenshot = ((TakesScreenshot) DriverManager.getDriver())
            .getScreenshotAs(OutputType.BYTES);
        Allure.addAttachment(name, "image/png",
            new java.io.ByteArrayInputStream(screenshot), "png");
    }

    /**
     * Attach text content
     */
    @Attachment(value = "Page Source", type = "text/html")
    public static String attachPageSource() {
        return DriverManager.getDriver().getPageSource();
    }

    /**
     * Attach JSON data
     */
    @Attachment(value = "API Response", type = "application/json")
    public static String attachJson(String json) {
        return json;
    }

    /**
     * Attach log content
     */
    public static void attachLog(String logContent) {
        Allure.addAttachment("Test Log", "text/plain", logContent);
    }
}
```

### Generating Allure Reports

```bash
# Run tests (generates allure-results directory)
mvn clean test

# Generate and open HTML report
mvn allure:serve

# Generate report without opening browser
mvn allure:report
# Report generated at: target/site/allure-maven-plugin/index.html

# Using Allure CLI directly
allure generate target/allure-results -o allure-report --clean
allure open allure-report
```

### Extent Reports vs Allure Reports Comparison

| Feature | Extent Reports | Allure Reports |
|---------|---------------|----------------|
| **Setup Complexity** | Simple (single JAR) | Moderate (AspectJ weaver needed) |
| **Report Format** | HTML | HTML (multi-page) |
| **Trend Analysis** | No | Yes (across builds) |
| **CI/CD Plugins** | Limited | Jenkins, GitHub Actions, GitLab |
| **Annotations** | Programmatic API | Annotation-based (`@Step`, `@Epic`) |
| **Screenshot Support** | Yes | Yes |
| **Categorization** | Tags, Categories | Epic > Feature > Story hierarchy |
| **Timeline View** | No | Yes |
| **Retry History** | No | Yes (shows retry attempts) |
| **Parallel Visualization** | Basic | Timeline chart |
| **Learning Curve** | Easy | Moderate |

---

## IRetryAnalyzer - Retry Mechanism

### What is IRetryAnalyzer?

`IRetryAnalyzer` is a TestNG interface that allows you to automatically retry failed tests. This is useful for handling flaky tests caused by environmental issues, timing problems, or intermittent failures.

### Basic Implementation

```java
// retry/RetryAnalyzer.java
package retry;

import org.testng.IRetryAnalyzer;
import org.testng.ITestResult;

public class RetryAnalyzer implements IRetryAnalyzer {

    private int retryCount = 0;
    private static final int MAX_RETRY_COUNT = 2; // Retry up to 2 times

    @Override
    public boolean retry(ITestResult result) {
        if (retryCount < MAX_RETRY_COUNT) {
            retryCount++;
            System.out.println(String.format(
                "Retrying test '%s' - Attempt %d of %d",
                result.getName(), retryCount, MAX_RETRY_COUNT
            ));
            return true;  // true = retry the test
        }
        return false;     // false = do not retry, mark as failed
    }
}
```

### Using RetryAnalyzer

```java
// Method 1: Per-test annotation
@Test(retryAnalyzer = RetryAnalyzer.class)
public void testFlaky() {
    // This test will be retried up to 2 times if it fails
    Assert.assertTrue(someUnstableCondition());
}

// Method 2: Apply to all tests using a listener (recommended)
```

### Configurable Retry Analyzer

```java
// retry/ConfigurableRetryAnalyzer.java
package retry;

import org.testng.IRetryAnalyzer;
import org.testng.ITestResult;

public class ConfigurableRetryAnalyzer implements IRetryAnalyzer {

    private int retryCount = 0;
    private int maxRetry;

    public ConfigurableRetryAnalyzer() {
        // Read from system property or default to 2
        String retryProp = System.getProperty("retry.count", "2");
        this.maxRetry = Integer.parseInt(retryProp);
    }

    @Override
    public boolean retry(ITestResult result) {
        if (retryCount < maxRetry) {
            retryCount++;
            System.out.println(String.format(
                "[RETRY] Test: %s | Attempt: %d/%d | Error: %s",
                result.getName(),
                retryCount,
                maxRetry,
                result.getThrowable() != null
                    ? result.getThrowable().getMessage()
                    : "Unknown"
            ));
            return true;
        }
        return false;
    }
}
```

### Applying Retry to All Tests via Annotation Transformer

```java
// retry/RetryTransformer.java
package retry;

import org.testng.IAnnotationTransformer;
import org.testng.annotations.ITestAnnotation;
import java.lang.reflect.Constructor;
import java.lang.reflect.Method;

/**
 * Automatically applies RetryAnalyzer to ALL test methods
 * without needing to add retryAnalyzer attribute to each @Test
 */
public class RetryTransformer implements IAnnotationTransformer {

    @Override
    public void transform(
            ITestAnnotation annotation,
            Class testClass,
            Constructor testConstructor,
            Method testMethod
    ) {
        // Only set retry if not already specified
        if (annotation.getRetryAnalyzerClass() == null) {
            annotation.setRetryAnalyzer(ConfigurableRetryAnalyzer.class);
        }
    }
}
```

Register in TestNG XML:
```xml
<suite name="Suite">
    <listeners>
        <listener class-name="retry.RetryTransformer"/>
    </listeners>
    <test name="Tests">
        <classes>
            <class name="tests.LoginTest"/>
        </classes>
    </test>
</suite>
```

### Retry with Conditional Logic

```java
// retry/SmartRetryAnalyzer.java
package retry;

import org.testng.IRetryAnalyzer;
import org.testng.ITestResult;

/**
 * Smart retry: only retries for specific exception types
 */
public class SmartRetryAnalyzer implements IRetryAnalyzer {

    private int retryCount = 0;
    private static final int MAX_RETRY = 2;

    // Exceptions worth retrying
    private static final Class<?>[] RETRYABLE_EXCEPTIONS = {
        org.openqa.selenium.TimeoutException.class,
        org.openqa.selenium.StaleElementReferenceException.class,
        org.openqa.selenium.NoSuchSessionException.class,
        org.openqa.selenium.WebDriverException.class
    };

    @Override
    public boolean retry(ITestResult result) {
        if (retryCount >= MAX_RETRY) {
            return false;
        }

        Throwable cause = result.getThrowable();
        if (cause != null && isRetryable(cause)) {
            retryCount++;
            System.out.println(String.format(
                "[SMART RETRY] %s failed with %s - Retry %d/%d",
                result.getName(),
                cause.getClass().getSimpleName(),
                retryCount,
                MAX_RETRY
            ));
            return true;
        }

        // Don't retry assertion errors (genuine test failures)
        return false;
    }

    private boolean isRetryable(Throwable throwable) {
        for (Class<?> retryableException : RETRYABLE_EXCEPTIONS) {
            if (retryableException.isInstance(throwable)) {
                return true;
            }
        }
        return false;
    }
}
```

---

## TestNG Listeners

### What are TestNG Listeners?

TestNG Listeners are interfaces that allow you to hook into the test execution lifecycle and perform custom actions at various stages (before/after test, on pass/fail/skip, etc.).

### ITestListener

The most commonly used listener for test-level events.

```java
// listeners/TestListener.java
package listeners;

import com.aventstack.extentreports.Status;
import reports.ExtentTestManager;
import utils.ScreenshotUtils;
import org.testng.ITestContext;
import org.testng.ITestListener;
import org.testng.ITestResult;

public class TestListener implements ITestListener {

    @Override
    public void onStart(ITestContext context) {
        System.out.println("======================================");
        System.out.println("Test Suite Started: " + context.getName());
        System.out.println("======================================");
    }

    @Override
    public void onFinish(ITestContext context) {
        System.out.println("======================================");
        System.out.println("Test Suite Finished: " + context.getName());
        System.out.println("Passed: " + context.getPassedTests().size());
        System.out.println("Failed: " + context.getFailedTests().size());
        System.out.println("Skipped: " + context.getSkippedTests().size());
        System.out.println("======================================");
    }

    @Override
    public void onTestStart(ITestResult result) {
        System.out.println(">>> Starting test: " + result.getName());

        // Create entry in Extent Report
        ExtentTestManager.startTest(
            result.getMethod().getMethodName(),
            result.getMethod().getDescription()
        );
    }

    @Override
    public void onTestSuccess(ITestResult result) {
        System.out.println("<<< PASSED: " + result.getName()
            + " (" + getExecutionTime(result) + "ms)");

        ExtentTestManager.getTest().log(Status.PASS, "Test Passed");
    }

    @Override
    public void onTestFailure(ITestResult result) {
        System.out.println("<<< FAILED: " + result.getName());
        System.out.println("    Cause: " + result.getThrowable().getMessage());

        // Capture screenshot on failure
        ScreenshotUtils.captureScreenshotOnFailure(result.getName());

        // Log failure in Extent Report
        ExtentTestManager.getTest().log(Status.FAIL,
            "Test Failed: " + result.getThrowable().getMessage());
        ExtentTestManager.getTest().fail(result.getThrowable());
    }

    @Override
    public void onTestSkipped(ITestResult result) {
        System.out.println("<<< SKIPPED: " + result.getName());

        ExtentTestManager.getTest().log(Status.SKIP, "Test Skipped");
        if (result.getThrowable() != null) {
            ExtentTestManager.getTest().skip(result.getThrowable());
        }
    }

    @Override
    public void onTestFailedButWithinSuccessPercentage(ITestResult result) {
        // Called when a test fails but is within the success percentage
        System.out.println("Test failed within success percentage: " + result.getName());
    }

    private long getExecutionTime(ITestResult result) {
        return result.getEndMillis() - result.getStartMillis();
    }
}
```

### ISuiteListener

Suite-level listener for actions before/after the entire test suite.

```java
// listeners/SuiteListener.java
package listeners;

import reports.ExtentReportManager;
import org.testng.ISuite;
import org.testng.ISuiteListener;

public class SuiteListener implements ISuiteListener {

    @Override
    public void onStart(ISuite suite) {
        System.out.println("=== Suite Starting: " + suite.getName() + " ===");

        // Initialize reporting
        ExtentReportManager.getInstance();

        // Initialize test environment
        System.out.println("Environment: " + System.getProperty("env", "QA"));
        System.out.println("Browser: " + System.getProperty("browser", "chrome"));
    }

    @Override
    public void onFinish(ISuite suite) {
        System.out.println("=== Suite Finished: " + suite.getName() + " ===");

        // Flush Extent Report
        ExtentReportManager.flushReport();

        // Print summary
        int total = suite.getResults().values().stream()
            .mapToInt(r -> r.getTestContext().getAllTestMethods().length)
            .sum();
        System.out.println("Total tests executed: " + total);
    }
}
```

### IMethodInterceptor

Reorder or filter test methods before execution.

```java
// listeners/MethodInterceptor.java
package listeners;

import org.testng.IMethodInstance;
import org.testng.IMethodInterceptor;
import org.testng.ITestContext;
import java.util.ArrayList;
import java.util.List;

/**
 * Filter tests based on priority or custom criteria
 */
public class MethodInterceptor implements IMethodInterceptor {

    @Override
    public List<IMethodInstance> intercept(
            List<IMethodInstance> methods, ITestContext context) {

        List<IMethodInstance> filteredMethods = new ArrayList<>();

        String targetGroup = System.getProperty("test.group", "all");

        for (IMethodInstance method : methods) {
            String[] groups = method.getMethod().getGroups();

            if (targetGroup.equals("all")) {
                filteredMethods.add(method);
            } else {
                for (String group : groups) {
                    if (group.equalsIgnoreCase(targetGroup)) {
                        filteredMethods.add(method);
                        break;
                    }
                }
            }
        }

        System.out.println("Total methods: " + methods.size());
        System.out.println("Filtered methods: " + filteredMethods.size());

        return filteredMethods;
    }
}
```

### Custom Listener Combining Multiple Interfaces

```java
// listeners/MasterListener.java
package listeners;

import org.testng.*;

/**
 * A single listener class implementing multiple listener interfaces
 */
public class MasterListener implements ITestListener, ISuiteListener, IInvokedMethodListener {

    // --- ISuiteListener ---
    @Override
    public void onStart(ISuite suite) {
        System.out.println("Suite started: " + suite.getName());
    }

    @Override
    public void onFinish(ISuite suite) {
        System.out.println("Suite finished: " + suite.getName());
    }

    // --- ITestListener ---
    @Override
    public void onTestStart(ITestResult result) {
        System.out.println("Test started: " + result.getName()
            + " [Thread-" + Thread.currentThread().getId() + "]");
    }

    @Override
    public void onTestSuccess(ITestResult result) {
        System.out.println("Test passed: " + result.getName());
    }

    @Override
    public void onTestFailure(ITestResult result) {
        System.out.println("Test failed: " + result.getName());
    }

    @Override
    public void onTestSkipped(ITestResult result) {
        System.out.println("Test skipped: " + result.getName());
    }

    // --- IInvokedMethodListener ---
    @Override
    public void beforeInvocation(IInvokedMethod method, ITestResult testResult) {
        // Called before EVERY method (including @Before/@After)
        if (method.isTestMethod()) {
            System.out.println("About to invoke test method: "
                + method.getTestMethod().getMethodName());
        }
    }

    @Override
    public void afterInvocation(IInvokedMethod method, ITestResult testResult) {
        if (method.isTestMethod()) {
            System.out.println("Finished invoking: "
                + method.getTestMethod().getMethodName()
                + " - Status: " + (testResult.isSuccess() ? "PASS" : "FAIL"));
        }
    }
}
```

### Registering Listeners

**Method 1: TestNG XML**
```xml
<suite name="Suite">
    <listeners>
        <listener class-name="listeners.TestListener"/>
        <listener class-name="listeners.SuiteListener"/>
        <listener class-name="listeners.RetryTransformer"/>
    </listeners>
    <test name="Tests">
        <classes>
            <class name="tests.LoginTest"/>
        </classes>
    </test>
</suite>
```

**Method 2: @Listeners annotation**
```java
@Listeners({TestListener.class, SuiteListener.class})
public class LoginTest extends BaseTest {
    @Test
    public void testLogin() { /* ... */ }
}
```

**Method 3: ServiceLoader (META-INF)**
```
// src/main/resources/META-INF/services/org.testng.ITestNGListener
listeners.TestListener
listeners.SuiteListener
listeners.RetryTransformer
```

### TestNG Listeners Summary

| Listener | Events | Use Case |
|----------|--------|----------|
| `ITestListener` | onTestStart, onTestSuccess, onTestFailure, onTestSkipped | Reporting, screenshots, logging |
| `ISuiteListener` | onStart, onFinish | Suite-level setup/teardown, report initialization |
| `IAnnotationTransformer` | transform | Apply retry analyzer to all tests |
| `IMethodInterceptor` | intercept | Filter/reorder tests before execution |
| `IInvokedMethodListener` | beforeInvocation, afterInvocation | Logging every method call |
| `IReporter` | generateReport | Custom report generation |
| `IConfigurationListener` | onConfigurationSuccess/Failure/Skip | Monitor @Before/@After methods |
| `IExecutionListener` | onExecutionStart, onExecutionFinish | CI/CD notifications |

---

## Exception Handling Patterns

### Framework-Level Exception Handling

```java
// exceptions/FrameworkException.java
package exceptions;

/**
 * Base exception for all framework-specific errors
 */
public class FrameworkException extends RuntimeException {

    public FrameworkException(String message) {
        super(message);
    }

    public FrameworkException(String message, Throwable cause) {
        super(message, cause);
    }
}
```

```java
// exceptions/PageLoadException.java
package exceptions;

public class PageLoadException extends FrameworkException {
    public PageLoadException(String url) {
        super("Failed to load page: " + url);
    }

    public PageLoadException(String url, Throwable cause) {
        super("Failed to load page: " + url, cause);
    }
}
```

```java
// exceptions/ElementNotFoundException.java
package exceptions;

public class ElementNotFoundException extends FrameworkException {
    public ElementNotFoundException(String locator) {
        super("Element not found: " + locator);
    }

    public ElementNotFoundException(String locator, Throwable cause) {
        super("Element not found: " + locator, cause);
    }
}
```

### Safe Action Wrapper Pattern

```java
// utils/SafeActions.java
package utils;

import driver.DriverManager;
import exceptions.ElementNotFoundException;
import org.openqa.selenium.*;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;
import java.time.Duration;

/**
 * Wrapper methods that handle exceptions gracefully
 */
public class SafeActions {

    private static final int DEFAULT_TIMEOUT = 10;

    /**
     * Safely click an element with retry logic
     */
    public static void safeClick(By locator) {
        int attempts = 0;
        while (attempts < 3) {
            try {
                WebDriverWait wait = new WebDriverWait(
                    DriverManager.getDriver(),
                    Duration.ofSeconds(DEFAULT_TIMEOUT)
                );
                WebElement element = wait.until(
                    ExpectedConditions.elementToBeClickable(locator)
                );
                element.click();
                return; // Success
            } catch (StaleElementReferenceException e) {
                attempts++;
                if (attempts >= 3) {
                    throw new ElementNotFoundException(
                        locator.toString() + " (stale after 3 attempts)", e
                    );
                }
            } catch (ElementClickInterceptedException e) {
                // Try JavaScript click as fallback
                WebElement element = DriverManager.getDriver().findElement(locator);
                ((JavascriptExecutor) DriverManager.getDriver())
                    .executeScript("arguments[0].click();", element);
                return;
            } catch (TimeoutException e) {
                throw new ElementNotFoundException(locator.toString(), e);
            }
        }
    }

    /**
     * Safely type text into an element
     */
    public static void safeType(By locator, String text) {
        try {
            WebDriverWait wait = new WebDriverWait(
                DriverManager.getDriver(),
                Duration.ofSeconds(DEFAULT_TIMEOUT)
            );
            WebElement element = wait.until(
                ExpectedConditions.visibilityOfElementLocated(locator)
            );
            element.clear();
            element.sendKeys(text);
        } catch (TimeoutException e) {
            throw new ElementNotFoundException(locator.toString(), e);
        } catch (InvalidElementStateException e) {
            // Element might be readonly, try JS approach
            WebElement element = DriverManager.getDriver().findElement(locator);
            ((JavascriptExecutor) DriverManager.getDriver())
                .executeScript(
                    "arguments[0].value = arguments[1];", element, text
                );
        }
    }

    /**
     * Safely get text from element (returns empty string instead of exception)
     */
    public static String safeGetText(By locator) {
        try {
            WebDriverWait wait = new WebDriverWait(
                DriverManager.getDriver(),
                Duration.ofSeconds(DEFAULT_TIMEOUT)
            );
            WebElement element = wait.until(
                ExpectedConditions.visibilityOfElementLocated(locator)
            );
            return element.getText();
        } catch (TimeoutException | NoSuchElementException e) {
            return "";
        }
    }

    /**
     * Check if element is present (no exception thrown)
     */
    public static boolean isElementPresent(By locator) {
        try {
            DriverManager.getDriver()
                .manage().timeouts().implicitlyWait(Duration.ofSeconds(2));
            boolean present = !DriverManager.getDriver()
                .findElements(locator).isEmpty();
            // Restore default timeout
            DriverManager.getDriver()
                .manage().timeouts().implicitlyWait(Duration.ofSeconds(DEFAULT_TIMEOUT));
            return present;
        } catch (Exception e) {
            return false;
        }
    }
}
```

---

## Multi-Browser Factory Pattern

### Design Pattern

The Factory Pattern creates WebDriver instances based on configuration, abstracting browser-specific setup from test code.

```java
// driver/BrowserFactory.java
package driver;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.openqa.selenium.edge.EdgeDriver;
import org.openqa.selenium.edge.EdgeOptions;
import org.openqa.selenium.firefox.FirefoxDriver;
import org.openqa.selenium.firefox.FirefoxOptions;
import org.openqa.selenium.firefox.FirefoxProfile;
import org.openqa.selenium.remote.RemoteWebDriver;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.HashMap;
import java.util.Map;

public class BrowserFactory {

    public enum BrowserType {
        CHROME, FIREFOX, EDGE, CHROME_HEADLESS, FIREFOX_HEADLESS
    }

    public enum ExecutionMode {
        LOCAL, GRID, BROWSERSTACK, LAMBDATEST
    }

    /**
     * Create WebDriver based on browser type and execution mode
     */
    public static WebDriver createDriver(BrowserType browserType, ExecutionMode mode) {
        switch (mode) {
            case LOCAL:
                return createLocalDriver(browserType);
            case GRID:
                return createGridDriver(browserType);
            case BROWSERSTACK:
                return createBrowserStackDriver(browserType);
            default:
                throw new IllegalArgumentException("Unknown mode: " + mode);
        }
    }

    // --- Local Drivers ---

    private static WebDriver createLocalDriver(BrowserType browserType) {
        switch (browserType) {
            case CHROME:
                return new ChromeDriver(getChromeOptions(false));
            case CHROME_HEADLESS:
                return new ChromeDriver(getChromeOptions(true));
            case FIREFOX:
                return new FirefoxDriver(getFirefoxOptions(false));
            case FIREFOX_HEADLESS:
                return new FirefoxDriver(getFirefoxOptions(true));
            case EDGE:
                return new EdgeDriver(getEdgeOptions(false));
            default:
                throw new IllegalArgumentException("Unsupported browser: " + browserType);
        }
    }

    // --- Grid Drivers ---

    private static WebDriver createGridDriver(BrowserType browserType) {
        String gridUrl = System.getProperty("grid.url", "http://localhost:4444/wd/hub");
        try {
            URL url = new URL(gridUrl);
            switch (browserType) {
                case CHROME:
                case CHROME_HEADLESS:
                    return new RemoteWebDriver(url,
                        getChromeOptions(browserType == BrowserType.CHROME_HEADLESS));
                case FIREFOX:
                case FIREFOX_HEADLESS:
                    return new RemoteWebDriver(url,
                        getFirefoxOptions(browserType == BrowserType.FIREFOX_HEADLESS));
                case EDGE:
                    return new RemoteWebDriver(url, getEdgeOptions(false));
                default:
                    throw new IllegalArgumentException("Unsupported browser: " + browserType);
            }
        } catch (MalformedURLException e) {
            throw new RuntimeException("Invalid Grid URL: " + gridUrl, e);
        }
    }

    // --- BrowserStack ---

    private static WebDriver createBrowserStackDriver(BrowserType browserType) {
        String username = System.getenv("BROWSERSTACK_USERNAME");
        String accessKey = System.getenv("BROWSERSTACK_ACCESS_KEY");
        String url = "https://" + username + ":" + accessKey
            + "@hub-cloud.browserstack.com/wd/hub";

        Map<String, Object> bstackOptions = new HashMap<>();
        bstackOptions.put("os", System.getProperty("bs.os", "Windows"));
        bstackOptions.put("osVersion", System.getProperty("bs.os.version", "11"));
        bstackOptions.put("projectName", "Automation Framework");
        bstackOptions.put("buildName", System.getProperty("build.name", "Local Build"));

        try {
            ChromeOptions options = getChromeOptions(false);
            options.setCapability("bstack:options", bstackOptions);
            return new RemoteWebDriver(new URL(url), options);
        } catch (MalformedURLException e) {
            throw new RuntimeException("Invalid BrowserStack URL", e);
        }
    }

    // --- Browser Options ---

    private static ChromeOptions getChromeOptions(boolean headless) {
        ChromeOptions options = new ChromeOptions();
        options.addArguments("--start-maximized");
        options.addArguments("--disable-notifications");
        options.addArguments("--disable-popup-blocking");
        options.addArguments("--no-sandbox");
        options.addArguments("--disable-dev-shm-usage");

        if (headless) {
            options.addArguments("--headless=new");
            options.addArguments("--window-size=1920,1080");
        }

        // Download preferences
        Map<String, Object> prefs = new HashMap<>();
        prefs.put("download.default_directory", System.getProperty("user.dir") + "/downloads");
        prefs.put("download.prompt_for_download", false);
        options.setExperimentalOption("prefs", prefs);

        return options;
    }

    private static FirefoxOptions getFirefoxOptions(boolean headless) {
        FirefoxOptions options = new FirefoxOptions();

        if (headless) {
            options.addArguments("-headless");
        }

        FirefoxProfile profile = new FirefoxProfile();
        profile.setPreference("browser.download.folderList", 2);
        profile.setPreference("browser.download.dir",
            System.getProperty("user.dir") + "/downloads");
        profile.setPreference("browser.helperApps.neverAsk.saveToDisk",
            "application/pdf,application/zip,text/csv");
        options.setProfile(profile);

        return options;
    }

    private static EdgeOptions getEdgeOptions(boolean headless) {
        EdgeOptions options = new EdgeOptions();
        options.addArguments("--start-maximized");
        if (headless) {
            options.addArguments("--headless=new");
        }
        return options;
    }
}
```

### Usage

```java
// Using the factory
WebDriver driver = BrowserFactory.createDriver(
    BrowserFactory.BrowserType.CHROME,
    BrowserFactory.ExecutionMode.LOCAL
);

// In configuration-driven tests
String browser = System.getProperty("browser", "chrome");
String mode = System.getProperty("execution.mode", "local");

BrowserFactory.BrowserType browserType =
    BrowserFactory.BrowserType.valueOf(browser.toUpperCase());
BrowserFactory.ExecutionMode execMode =
    BrowserFactory.ExecutionMode.valueOf(mode.toUpperCase());

WebDriver driver = BrowserFactory.createDriver(browserType, execMode);
```

---

## Environment Management

### Configuration-Driven Environment Management

```java
// config/EnvironmentManager.java
package config;

import java.io.FileInputStream;
import java.io.IOException;
import java.util.Properties;

/**
 * Manages environment-specific configurations
 */
public class EnvironmentManager {

    private static Properties props;
    private static String activeEnvironment;

    /**
     * Load configuration for the specified environment
     */
    public static void loadEnvironment(String environment) {
        activeEnvironment = environment;
        props = new Properties();

        // Load default properties
        loadPropertiesFile("config/default.properties");

        // Load environment-specific properties (overrides defaults)
        loadPropertiesFile("config/" + environment + ".properties");

        // System properties override everything
        props.putAll(System.getProperties());

        System.out.println("Loaded environment: " + environment);
        System.out.println("Base URL: " + get("base.url"));
    }

    private static void loadPropertiesFile(String path) {
        try {
            String fullPath = "src/test/resources/" + path;
            FileInputStream fis = new FileInputStream(fullPath);
            props.load(fis);
            fis.close();
        } catch (IOException e) {
            System.out.println("Properties file not found: " + path
                + " (using defaults)");
        }
    }

    public static String get(String key) {
        String value = props.getProperty(key);
        if (value == null) {
            throw new RuntimeException(
                "Property '" + key + "' not found in environment: " + activeEnvironment
            );
        }
        return value;
    }

    public static String get(String key, String defaultValue) {
        return props.getProperty(key, defaultValue);
    }

    public static int getInt(String key) {
        return Integer.parseInt(get(key));
    }

    public static boolean getBoolean(String key) {
        return Boolean.parseBoolean(get(key));
    }

    public static String getActiveEnvironment() {
        return activeEnvironment;
    }
}
```

### Environment-Specific Property Files

```properties
# src/test/resources/config/default.properties
browser=chrome
headless=false
implicit.wait=10
page.load.timeout=30
explicit.wait=15
execution.mode=local
grid.url=http://localhost:4444/wd/hub
```

```properties
# src/test/resources/config/qa.properties
base.url=https://qa.myapp.com
api.url=https://qa-api.myapp.com
db.host=qa-db.internal.com
db.name=qa_database
admin.username=qa_admin
admin.password=${QA_ADMIN_PASSWORD}
```

```properties
# src/test/resources/config/staging.properties
base.url=https://staging.myapp.com
api.url=https://staging-api.myapp.com
db.host=staging-db.internal.com
db.name=staging_database
admin.username=staging_admin
admin.password=${STAGING_ADMIN_PASSWORD}
```

```properties
# src/test/resources/config/prod.properties
base.url=https://www.myapp.com
api.url=https://api.myapp.com
execution.mode=browserstack
headless=true
```

### Usage in Tests

```java
// Base test class
public class BaseTest {

    @BeforeSuite
    public void loadConfiguration() {
        String env = System.getProperty("env", "qa");
        EnvironmentManager.loadEnvironment(env);
    }

    @BeforeMethod
    public void setup() {
        String browser = EnvironmentManager.get("browser");
        DriverManager.initDriver(browser);
        getDriver().get(EnvironmentManager.get("base.url"));
    }
}
```

```bash
# Run on different environments
mvn test -Denv=qa
mvn test -Denv=staging
mvn test -Denv=prod -Dbrowser=chrome
```

---

## Email & Slack Notifications

### Email Notification After Test Execution

```java
// notifications/EmailNotifier.java
package notifications;

import javax.mail.*;
import javax.mail.internet.*;
import java.io.File;
import java.util.Properties;

public class EmailNotifier {

    private final String smtpHost;
    private final String smtpPort;
    private final String username;
    private final String password;

    public EmailNotifier() {
        this.smtpHost = System.getenv("SMTP_HOST");
        this.smtpPort = System.getenv("SMTP_PORT");
        this.username = System.getenv("SMTP_USERNAME");
        this.password = System.getenv("SMTP_PASSWORD");
    }

    /**
     * Send test execution report via email
     */
    public void sendTestReport(
            String[] recipients,
            String subject,
            String body,
            String reportFilePath
    ) {
        Properties props = new Properties();
        props.put("mail.smtp.auth", "true");
        props.put("mail.smtp.starttls.enable", "true");
        props.put("mail.smtp.host", smtpHost);
        props.put("mail.smtp.port", smtpPort);

        Session session = Session.getInstance(props, new Authenticator() {
            @Override
            protected PasswordAuthentication getPasswordAuthentication() {
                return new PasswordAuthentication(username, password);
            }
        });

        try {
            Message message = new MimeMessage(session);
            message.setFrom(new InternetAddress(username));
            for (String recipient : recipients) {
                message.addRecipient(Message.RecipientType.TO,
                    new InternetAddress(recipient));
            }
            message.setSubject(subject);

            // Create multipart message
            Multipart multipart = new MimeMultipart();

            // Text body
            MimeBodyPart textPart = new MimeBodyPart();
            textPart.setContent(body, "text/html");
            multipart.addBodyPart(textPart);

            // Attachment (report file)
            if (reportFilePath != null) {
                File reportFile = new File(reportFilePath);
                if (reportFile.exists()) {
                    MimeBodyPart attachmentPart = new MimeBodyPart();
                    attachmentPart.attachFile(reportFile);
                    multipart.addBodyPart(attachmentPart);
                }
            }

            message.setContent(multipart);
            Transport.send(message);

            System.out.println("Email sent successfully to: "
                + String.join(", ", recipients));

        } catch (Exception e) {
            System.err.println("Failed to send email: " + e.getMessage());
        }
    }

    /**
     * Build HTML email body with test results
     */
    public static String buildEmailBody(
            int totalTests, int passed, int failed, int skipped,
            String suiteName, long duration
    ) {
        return String.format("""
            <html>
            <body style="font-family: Arial, sans-serif;">
            <h2>Test Execution Report: %s</h2>
            <table border="1" cellpadding="10" cellspacing="0"
                   style="border-collapse: collapse;">
                <tr style="background-color: #1976d2; color: white;">
                    <th>Metric</th><th>Count</th>
                </tr>
                <tr><td>Total Tests</td><td>%d</td></tr>
                <tr style="background-color: #c8e6c9;">
                    <td>Passed</td><td>%d</td>
                </tr>
                <tr style="background-color: #ffcdd2;">
                    <td>Failed</td><td>%d</td>
                </tr>
                <tr style="background-color: #fff9c4;">
                    <td>Skipped</td><td>%d</td>
                </tr>
                <tr><td>Duration</td><td>%d seconds</td></tr>
                <tr><td>Pass Rate</td><td>%.1f%%</td></tr>
            </table>
            <p>Please see the attached report for details.</p>
            </body>
            </html>
            """,
            suiteName, totalTests, passed, failed, skipped,
            duration / 1000,
            (totalTests > 0) ? (passed * 100.0 / totalTests) : 0
        );
    }
}
```

### Slack Notification

```java
// notifications/SlackNotifier.java
package notifications;

import java.io.OutputStream;
import java.net.HttpURLConnection;
import java.net.URL;
import java.nio.charset.StandardCharsets;

public class SlackNotifier {

    private final String webhookUrl;

    public SlackNotifier() {
        this.webhookUrl = System.getenv("SLACK_WEBHOOK_URL");
    }

    public SlackNotifier(String webhookUrl) {
        this.webhookUrl = webhookUrl;
    }

    /**
     * Send a simple text message to Slack
     */
    public void sendMessage(String message) {
        String payload = String.format("{\"text\": \"%s\"}", escapeJson(message));
        sendPayload(payload);
    }

    /**
     * Send a rich formatted notification with test results
     */
    public void sendTestResults(
            String suiteName,
            int totalTests,
            int passed,
            int failed,
            int skipped,
            long durationMs,
            String reportUrl
    ) {
        double passRate = totalTests > 0 ? (passed * 100.0 / totalTests) : 0;
        String statusEmoji = failed == 0 ? ":white_check_mark:" : ":x:";
        String statusColor = failed == 0 ? "#36a64f" : "#ff0000";

        String payload = String.format("""
            {
                "blocks": [
                    {
                        "type": "header",
                        "text": {
                            "type": "plain_text",
                            "text": "%s Test Execution Complete %s"
                        }
                    },
                    {
                        "type": "section",
                        "fields": [
                            {"type": "mrkdwn", "text": "*Suite:*\\n%s"},
                            {"type": "mrkdwn", "text": "*Duration:*\\n%d seconds"},
                            {"type": "mrkdwn", "text": "*Total:*\\n%d tests"},
                            {"type": "mrkdwn", "text": "*Pass Rate:*\\n%.1f%%"},
                            {"type": "mrkdwn", "text": "*:white_check_mark: Passed:*\\n%d"},
                            {"type": "mrkdwn", "text": "*:x: Failed:*\\n%d"},
                            {"type": "mrkdwn", "text": "*:fast_forward: Skipped:*\\n%d"},
                            {"type": "mrkdwn", "text": "*Report:*\\n<%s|View Report>"}
                        ]
                    }
                ]
            }
            """,
            statusEmoji, statusEmoji,
            suiteName, durationMs / 1000,
            totalTests, passRate,
            passed, failed, skipped,
            reportUrl != null ? reportUrl : "#"
        );

        sendPayload(payload);
    }

    private void sendPayload(String jsonPayload) {
        if (webhookUrl == null || webhookUrl.isEmpty()) {
            System.out.println("Slack webhook URL not configured, skipping notification");
            return;
        }

        try {
            URL url = new URL(webhookUrl);
            HttpURLConnection conn = (HttpURLConnection) url.openConnection();
            conn.setRequestMethod("POST");
            conn.setRequestProperty("Content-Type", "application/json");
            conn.setDoOutput(true);

            try (OutputStream os = conn.getOutputStream()) {
                byte[] input = jsonPayload.getBytes(StandardCharsets.UTF_8);
                os.write(input, 0, input.length);
            }

            int responseCode = conn.getResponseCode();
            if (responseCode == 200) {
                System.out.println("Slack notification sent successfully");
            } else {
                System.err.println("Slack notification failed. Response code: "
                    + responseCode);
            }

            conn.disconnect();
        } catch (Exception e) {
            System.err.println("Failed to send Slack notification: " + e.getMessage());
        }
    }

    private String escapeJson(String text) {
        return text.replace("\\", "\\\\")
                   .replace("\"", "\\\"")
                   .replace("\n", "\\n");
    }
}
```

### Integrating Notifications with TestNG Listener

```java
// listeners/NotificationListener.java
package listeners;

import notifications.EmailNotifier;
import notifications.SlackNotifier;
import org.testng.ISuite;
import org.testng.ISuiteListener;
import org.testng.ISuiteResult;

public class NotificationListener implements ISuiteListener {

    private long startTime;

    @Override
    public void onStart(ISuite suite) {
        startTime = System.currentTimeMillis();
    }

    @Override
    public void onFinish(ISuite suite) {
        long duration = System.currentTimeMillis() - startTime;

        // Calculate totals
        int passed = 0, failed = 0, skipped = 0;
        for (ISuiteResult result : suite.getResults().values()) {
            passed += result.getTestContext().getPassedTests().size();
            failed += result.getTestContext().getFailedTests().size();
            skipped += result.getTestContext().getSkippedTests().size();
        }
        int total = passed + failed + skipped;

        // Send Slack notification
        SlackNotifier slack = new SlackNotifier();
        slack.sendTestResults(
            suite.getName(), total, passed, failed, skipped,
            duration, System.getProperty("report.url")
        );

        // Send email notification (if enabled)
        if (Boolean.parseBoolean(System.getProperty("send.email", "false"))) {
            EmailNotifier email = new EmailNotifier();
            String body = EmailNotifier.buildEmailBody(
                total, passed, failed, skipped, suite.getName(), duration
            );
            email.sendTestReport(
                new String[]{"team@company.com"},
                "Test Report: " + suite.getName()
                    + " - " + (failed > 0 ? "FAILURES DETECTED" : "ALL PASSED"),
                body,
                System.getProperty("user.dir") + "/reports/TestReport.html"
            );
        }
    }
}
```

Register in TestNG XML:
```xml
<suite name="Suite">
    <listeners>
        <listener class-name="listeners.NotificationListener"/>
    </listeners>
    <!-- ... -->
</suite>
```

---

## Summary

### Framework Components Overview

```
Advanced Framework Architecture
================================

┌──────────────────────────────────────────────────────┐
│                  Test Layer                            │
│  ┌────────────┐ ┌────────────┐ ┌────────────┐       │
│  │ LoginTest  │ │ SearchTest │ │ CartTest   │       │
│  └────────────┘ └────────────┘ └────────────┘       │
│         │              │              │               │
│         └──────────────┼──────────────┘               │
│                        v                              │
│  ┌──────────────────────────────────────────┐        │
│  │           BaseTest                        │        │
│  │  - setUp() / tearDown()                   │        │
│  │  - Environment loading                    │        │
│  │  - Report initialization                  │        │
│  └──────────────────────────────────────────┘        │
└──────────────────────────────────────────────────────┘
         │                    │                   │
         v                    v                   v
┌──────────────┐  ┌─────────────────┐  ┌──────────────┐
│ Driver Layer │  │ Reporting Layer │  │ Utility Layer│
│              │  │                 │  │              │
│ DriverManager│  │ ExtentReports   │  │ SafeActions  │
│ (ThreadLocal)│  │ AllureReports   │  │ Screenshots  │
│              │  │                 │  │ Exceptions   │
│ BrowserFactory│ │ TestListener    │  │ Config       │
│ (Local/Grid/ │  │ SuiteListener   │  │ Data Utils   │
│  Cloud)      │  │                 │  │              │
└──────────────┘  └─────────────────┘  └──────────────┘
         │                    │                   │
         v                    v                   v
┌──────────────┐  ┌─────────────────┐  ┌──────────────┐
│ Infra Layer  │  │ Notification    │  │ Retry Layer  │
│              │  │                 │  │              │
│ Selenium Grid│  │ Email           │  │ IRetryAnalyzer│
│ Docker       │  │ Slack           │  │ Retry        │
│ BrowserStack │  │                 │  │ Transformer  │
│ LambdaTest   │  │                 │  │              │
└──────────────┘  └─────────────────┘  └──────────────┘
```

### Key Takeaways

| Topic | Key Points |
|-------|------------|
| **Extent Reports** | `ExtentSparkReporter` for HTML; use `ExtentTestManager` with ThreadLocal for parallel; always call `flush()` |
| **Allure Reports** | Annotation-driven (`@Step`, `@Epic`, `@Feature`); better CI/CD integration; trend analysis |
| **IRetryAnalyzer** | Implement `retry()` method; use `IAnnotationTransformer` to apply globally; smart retry based on exception type |
| **ITestListener** | Most used listener; captures pass/fail/skip events; attach screenshots on failure |
| **ISuiteListener** | Suite-level events; initialize/finalize reports; send notifications |
| **Exception Handling** | Custom exception hierarchy; safe action wrappers; graceful fallbacks |
| **Browser Factory** | Factory pattern for multi-browser; support Local/Grid/Cloud modes; centralize options |
| **Environment Management** | Properties files per environment; system property overrides; `EnvironmentManager` class |
| **Email Notifications** | JavaMail API; HTML body with test stats; attach report file |
| **Slack Notifications** | Webhook API; rich Block Kit formatting; integrate via `ISuiteListener` |

---

*This document covers the advanced framework components needed to build a production-grade test automation framework. Each component can be implemented incrementally as the framework matures.*
