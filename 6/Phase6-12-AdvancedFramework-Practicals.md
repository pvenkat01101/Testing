# Phase 6-12: Advanced Framework Development - Practicals

## Table of Contents
1. [Setting Up Extent Reports with Screenshots](#1-setting-up-extent-reports-with-screenshots)
2. [Implementing IRetryAnalyzer](#2-implementing-iretryanalyzer)
3. [Creating TestNG Listeners (ITestListener)](#3-creating-testng-listeners-itestlistener)
4. [Building Browser Factory Pattern](#4-building-browser-factory-pattern)
5. [Environment-Specific Configuration](#5-environment-specific-configuration)
6. [Creating Email Notifications on Test Completion](#6-creating-email-notifications-on-test-completion)

---

## 1. Setting Up Extent Reports with Screenshots

### Exercise 1.1: Add Extent Reports Dependencies

Add the following to your `pom.xml`:

```xml
<dependencies>
    <!-- Extent Reports -->
    <dependency>
        <groupId>com.aventstack</groupId>
        <artifactId>extentreports</artifactId>
        <version>5.1.1</version>
    </dependency>

    <!-- Selenium WebDriver -->
    <dependency>
        <groupId>org.seleniumhq.selenium</groupId>
        <artifactId>selenium-java</artifactId>
        <version>4.15.0</version>
    </dependency>

    <!-- TestNG -->
    <dependency>
        <groupId>org.testng</groupId>
        <artifactId>testng</artifactId>
        <version>7.8.0</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

### Exercise 1.2: Create the ExtentReportManager Class

```java
package com.framework.reports;

import com.aventstack.extentreports.ExtentReports;
import com.aventstack.extentreports.ExtentTest;
import com.aventstack.extentreports.reporter.ExtentSparkReporter;
import com.aventstack.extentreports.reporter.configuration.Theme;

import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.HashMap;
import java.util.Map;

public class ExtentReportManager {

    private static ExtentReports extent;
    private static Map<Long, ExtentTest> testMap = new HashMap<>();

    // Private constructor to prevent instantiation
    private ExtentReportManager() {}

    /**
     * Initialize the ExtentReports instance (call once before all tests).
     */
    public static synchronized ExtentReports getInstance() {
        if (extent == null) {
            String timestamp = new SimpleDateFormat("yyyy-MM-dd_HH-mm-ss").format(new Date());
            String reportPath = System.getProperty("user.dir")
                    + "/test-output/ExtentReport_" + timestamp + ".html";

            ExtentSparkReporter sparkReporter = new ExtentSparkReporter(reportPath);
            sparkReporter.config().setDocumentTitle("Automation Test Report");
            sparkReporter.config().setReportName("Regression Suite Results");
            sparkReporter.config().setTheme(Theme.STANDARD);
            sparkReporter.config().setEncoding("utf-8");
            sparkReporter.config().setTimeStampFormat("EEEE, MMMM dd, yyyy, hh:mm a '('zzz')'");

            extent = new ExtentReports();
            extent.attachReporter(sparkReporter);

            // System information shown in the report dashboard
            extent.setSystemInfo("OS", System.getProperty("os.name"));
            extent.setSystemInfo("Java Version", System.getProperty("java.version"));
            extent.setSystemInfo("User", System.getProperty("user.name"));
            extent.setSystemInfo("Environment", "QA");
        }
        return extent;
    }

    /**
     * Create a test entry in the report (one per @Test method).
     */
    public static synchronized ExtentTest createTest(String testName, String description) {
        ExtentTest test = getInstance().createTest(testName, description);
        testMap.put(Thread.currentThread().getId(), test);
        return test;
    }

    /**
     * Get the ExtentTest for the current thread (thread-safe for parallel runs).
     */
    public static synchronized ExtentTest getTest() {
        return testMap.get(Thread.currentThread().getId());
    }

    /**
     * Flush the report (call once after all tests complete).
     */
    public static synchronized void flushReport() {
        if (extent != null) {
            extent.flush();
        }
    }
}
```

### Exercise 1.3: Screenshot Utility

```java
package com.framework.utils;

import org.apache.commons.io.FileUtils;
import org.openqa.selenium.OutputType;
import org.openqa.selenium.TakesScreenshot;
import org.openqa.selenium.WebDriver;

import java.io.File;
import java.io.IOException;
import java.text.SimpleDateFormat;
import java.util.Base64;
import java.util.Date;

public class ScreenshotUtil {

    /**
     * Capture a screenshot and save it to disk.
     * Returns the absolute file path of the saved image.
     */
    public static String captureScreenshot(WebDriver driver, String screenshotName) {
        String timestamp = new SimpleDateFormat("yyyyMMdd_HHmmss").format(new Date());
        String fileName = screenshotName + "_" + timestamp + ".png";
        String directory = System.getProperty("user.dir") + "/test-output/screenshots/";
        String filePath = directory + fileName;

        try {
            File source = ((TakesScreenshot) driver).getScreenshotAs(OutputType.FILE);
            FileUtils.copyFile(source, new File(filePath));
            System.out.println("Screenshot saved: " + filePath);
        } catch (IOException e) {
            System.err.println("Failed to capture screenshot: " + e.getMessage());
        }
        return filePath;
    }

    /**
     * Capture a screenshot and return it as a Base64-encoded string.
     * Useful for embedding directly in Extent Reports without linking files.
     */
    public static String captureScreenshotAsBase64(WebDriver driver) {
        return ((TakesScreenshot) driver).getScreenshotAs(OutputType.BASE64);
    }
}
```

### Exercise 1.4: Integrating Extent Reports with TestNG Listener

```java
package com.framework.listeners;

import com.aventstack.extentreports.ExtentTest;
import com.aventstack.extentreports.MediaEntityBuilder;
import com.aventstack.extentreports.Status;
import com.framework.base.BaseTest;
import com.framework.reports.ExtentReportManager;
import com.framework.utils.ScreenshotUtil;

import org.testng.ITestContext;
import org.testng.ITestListener;
import org.testng.ITestResult;

public class ExtentTestListener implements ITestListener {

    @Override
    public void onStart(ITestContext context) {
        System.out.println("===== Suite started: " + context.getName() + " =====");
        ExtentReportManager.getInstance();
    }

    @Override
    public void onTestStart(ITestResult result) {
        String testName = result.getMethod().getMethodName();
        String description = result.getMethod().getDescription();
        ExtentReportManager.createTest(testName,
                description != null ? description : "No description provided");
    }

    @Override
    public void onTestSuccess(ITestResult result) {
        ExtentReportManager.getTest().log(Status.PASS,
                "Test PASSED: " + result.getMethod().getMethodName());
    }

    @Override
    public void onTestFailure(ITestResult result) {
        ExtentTest test = ExtentReportManager.getTest();
        test.log(Status.FAIL, "Test FAILED: " + result.getMethod().getMethodName());
        test.log(Status.FAIL, "Cause: " + result.getThrowable());

        // Attach a screenshot on failure
        try {
            Object testClass = result.getInstance();
            // BaseTest is expected to expose a getDriver() method
            if (testClass instanceof BaseTest) {
                String base64 = ScreenshotUtil.captureScreenshotAsBase64(
                        ((BaseTest) testClass).getDriver());
                test.fail("Screenshot on failure",
                        MediaEntityBuilder.createScreenCaptureFromBase64String(base64).build());
            }
        } catch (Exception e) {
            test.log(Status.WARNING, "Could not attach screenshot: " + e.getMessage());
        }
    }

    @Override
    public void onTestSkipped(ITestResult result) {
        ExtentReportManager.getTest().log(Status.SKIP,
                "Test SKIPPED: " + result.getMethod().getMethodName());
    }

    @Override
    public void onFinish(ITestContext context) {
        ExtentReportManager.flushReport();
        System.out.println("===== Suite finished: " + context.getName() + " =====");
    }
}
```

### Exercise 1.5: Sample Test Class Using Extent Reports

```java
package com.framework.tests;

import com.framework.base.BaseTest;
import com.framework.listeners.ExtentTestListener;
import com.framework.reports.ExtentReportManager;

import org.openqa.selenium.By;
import org.openqa.selenium.WebElement;
import org.testng.Assert;
import org.testng.annotations.Listeners;
import org.testng.annotations.Test;

@Listeners(ExtentTestListener.class)
public class LoginTest extends BaseTest {

    @Test(description = "Verify successful login with valid credentials")
    public void testValidLogin() {
        ExtentReportManager.getTest().info("Navigating to login page");
        driver.get("https://the-internet.herokuapp.com/login");

        ExtentReportManager.getTest().info("Entering username");
        driver.findElement(By.id("username")).sendKeys("tomsmith");

        ExtentReportManager.getTest().info("Entering password");
        driver.findElement(By.id("password")).sendKeys("SuperSecretPassword!");

        ExtentReportManager.getTest().info("Clicking Login button");
        driver.findElement(By.cssSelector("button[type='submit']")).click();

        WebElement successMessage = driver.findElement(By.id("flash"));
        Assert.assertTrue(successMessage.getText().contains("You logged into a secure area!"),
                "Login success message not displayed");
    }

    @Test(description = "Verify error message with invalid credentials")
    public void testInvalidLogin() {
        driver.get("https://the-internet.herokuapp.com/login");
        driver.findElement(By.id("username")).sendKeys("invalidUser");
        driver.findElement(By.id("password")).sendKeys("wrongPass");
        driver.findElement(By.cssSelector("button[type='submit']")).click();

        WebElement errorMessage = driver.findElement(By.id("flash"));
        Assert.assertTrue(errorMessage.getText().contains("Your username is invalid!"),
                "Error message not displayed");
    }
}
```

---

## 2. Implementing IRetryAnalyzer

### Exercise 2.1: Basic Retry Analyzer

```java
package com.framework.retry;

import org.testng.IRetryAnalyzer;
import org.testng.ITestResult;

public class RetryAnalyzer implements IRetryAnalyzer {

    private int retryCount = 0;
    private static final int MAX_RETRY_COUNT = 2; // Retry up to 2 times

    @Override
    public boolean retry(ITestResult result) {
        if (retryCount < MAX_RETRY_COUNT) {
            retryCount++;
            System.out.println("[RETRY] Retrying test: " + result.getName()
                    + " | Attempt: " + retryCount + " of " + MAX_RETRY_COUNT);
            return true;  // true = re-run the test
        }
        return false; // false = stop retrying
    }
}
```

### Exercise 2.2: Configurable Retry Analyzer

```java
package com.framework.retry;

import com.framework.config.ConfigReader;
import org.testng.IRetryAnalyzer;
import org.testng.ITestResult;

/**
 * Reads the max retry count from config.properties so it can be
 * changed without recompiling.
 */
public class ConfigurableRetryAnalyzer implements IRetryAnalyzer {

    private int retryCount = 0;
    private int maxRetryCount;

    public ConfigurableRetryAnalyzer() {
        // Read from properties file; default to 1 if not set
        String retryValue = ConfigReader.getProperty("retry.count");
        maxRetryCount = (retryValue != null) ? Integer.parseInt(retryValue) : 1;
    }

    @Override
    public boolean retry(ITestResult result) {
        if (retryCount < maxRetryCount) {
            retryCount++;
            System.out.println("[RETRY] " + result.getName()
                    + " | Attempt " + retryCount + "/" + maxRetryCount);
            return true;
        }
        return false;
    }
}
```

### Exercise 2.3: Applying Retry to Individual Tests

```java
package com.framework.tests;

import com.framework.retry.RetryAnalyzer;
import org.testng.Assert;
import org.testng.annotations.Test;

public class RetryDemoTest {

    @Test(retryAnalyzer = RetryAnalyzer.class)
    public void testFlaky() {
        // Simulating a flaky test (fails randomly)
        double random = Math.random();
        System.out.println("Random value: " + random);
        Assert.assertTrue(random > 0.5, "Random value was <= 0.5, test fails this run");
    }
}
```

### Exercise 2.4: Applying Retry to All Tests Using AnnotationTransformer

```java
package com.framework.retry;

import org.testng.IAnnotationTransformer;
import org.testng.annotations.ITestAnnotation;

import java.lang.reflect.Constructor;
import java.lang.reflect.Method;

/**
 * Automatically attaches RetryAnalyzer to every @Test method so you
 * do not need to add retryAnalyzer = ... on each test individually.
 *
 * Register this transformer in testng.xml:
 * <listeners>
 *   <listener class-name="com.framework.retry.RetryTransformer"/>
 * </listeners>
 */
public class RetryTransformer implements IAnnotationTransformer {

    @Override
    public void transform(ITestAnnotation annotation, Class testClass,
                          Constructor testConstructor, Method testMethod) {
        // Only set the retry analyzer if one has not already been specified
        if (annotation.getRetryAnalyzerClass() == null
                || annotation.getRetryAnalyzerClass() == org.testng.IRetryAnalyzer.class) {
            annotation.setRetryAnalyzer(RetryAnalyzer.class);
        }
    }
}
```

### Exercise 2.5: testng.xml with Retry Transformer

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd">
<suite name="Regression Suite" verbose="1">

    <listeners>
        <listener class-name="com.framework.retry.RetryTransformer"/>
        <listener class-name="com.framework.listeners.ExtentTestListener"/>
    </listeners>

    <test name="Smoke Tests">
        <classes>
            <class name="com.framework.tests.LoginTest"/>
            <class name="com.framework.tests.RetryDemoTest"/>
        </classes>
    </test>

</suite>
```

---

## 3. Creating TestNG Listeners (ITestListener)

### Exercise 3.1: Full ITestListener Implementation

```java
package com.framework.listeners;

import org.testng.ITestContext;
import org.testng.ITestListener;
import org.testng.ITestResult;

import java.util.Arrays;

/**
 * A comprehensive TestNG listener that logs every lifecycle event.
 */
public class TestListener implements ITestListener {

    @Override
    public void onStart(ITestContext context) {
        System.out.println("=========================================");
        System.out.println("SUITE STARTED : " + context.getName());
        System.out.println("Start Time    : " + context.getStartDate());
        System.out.println("=========================================");
    }

    @Override
    public void onTestStart(ITestResult result) {
        System.out.println("[START] " + getTestMethodName(result));
        System.out.println("  Parameters: " + Arrays.toString(result.getParameters()));
    }

    @Override
    public void onTestSuccess(ITestResult result) {
        long duration = result.getEndMillis() - result.getStartMillis();
        System.out.println("[PASS]  " + getTestMethodName(result)
                + "  (" + duration + " ms)");
    }

    @Override
    public void onTestFailure(ITestResult result) {
        long duration = result.getEndMillis() - result.getStartMillis();
        System.out.println("[FAIL]  " + getTestMethodName(result)
                + "  (" + duration + " ms)");
        System.out.println("  Error: " + result.getThrowable().getMessage());
    }

    @Override
    public void onTestSkipped(ITestResult result) {
        System.out.println("[SKIP]  " + getTestMethodName(result));
        if (result.getThrowable() != null) {
            System.out.println("  Reason: " + result.getThrowable().getMessage());
        }
    }

    @Override
    public void onTestFailedButWithinSuccessPercentage(ITestResult result) {
        System.out.println("[PARTIAL PASS] " + getTestMethodName(result));
    }

    @Override
    public void onFinish(ITestContext context) {
        System.out.println("=========================================");
        System.out.println("SUITE FINISHED: " + context.getName());
        System.out.println("Passed  : " + context.getPassedTests().size());
        System.out.println("Failed  : " + context.getFailedTests().size());
        System.out.println("Skipped : " + context.getSkippedTests().size());
        System.out.println("=========================================");
    }

    // --- helper ---
    private String getTestMethodName(ITestResult result) {
        return result.getTestClass().getName() + "." + result.getMethod().getMethodName();
    }
}
```

### Exercise 3.2: ISuiteListener Implementation

```java
package com.framework.listeners;

import org.testng.ISuite;
import org.testng.ISuiteListener;

/**
 * Runs once before/after the entire suite.
 * Good for global setup (DB connections, report init, etc.)
 */
public class SuiteListener implements ISuiteListener {

    @Override
    public void onStart(ISuite suite) {
        System.out.println(">>>>> SUITE [" + suite.getName() + "] STARTING >>>>>");
        // Global setup: initialize report, create directories, etc.
    }

    @Override
    public void onFinish(ISuite suite) {
        System.out.println("<<<<< SUITE [" + suite.getName() + "] FINISHED <<<<<");
        // Global teardown: flush reports, send emails, etc.
    }
}
```

### Exercise 3.3: IInvokedMethodListener for Method-Level Hooks

```java
package com.framework.listeners;

import org.testng.IInvokedMethod;
import org.testng.IInvokedMethodListener;
import org.testng.ITestResult;

/**
 * Fires before and after EVERY method (including @BeforeMethod, @AfterMethod).
 * Useful for logging or injecting behaviour around configuration methods.
 */
public class MethodListener implements IInvokedMethodListener {

    @Override
    public void beforeInvocation(IInvokedMethod method, ITestResult testResult) {
        String type = method.isTestMethod() ? "TEST" : "CONFIG";
        System.out.println("[BEFORE " + type + "] "
                + method.getTestMethod().getMethodName());
    }

    @Override
    public void afterInvocation(IInvokedMethod method, ITestResult testResult) {
        String type = method.isTestMethod() ? "TEST" : "CONFIG";
        System.out.println("[AFTER  " + type + "] "
                + method.getTestMethod().getMethodName());
    }
}
```

### Exercise 3.4: Registering Listeners in testng.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd">
<suite name="Full Regression" verbose="1">

    <listeners>
        <listener class-name="com.framework.listeners.TestListener"/>
        <listener class-name="com.framework.listeners.SuiteListener"/>
        <listener class-name="com.framework.listeners.MethodListener"/>
        <listener class-name="com.framework.listeners.ExtentTestListener"/>
        <listener class-name="com.framework.retry.RetryTransformer"/>
    </listeners>

    <test name="Login Module">
        <classes>
            <class name="com.framework.tests.LoginTest"/>
        </classes>
    </test>

</suite>
```

### Exercise 3.5: Using @Listeners Annotation (Alternative)

```java
package com.framework.tests;

import com.framework.listeners.TestListener;
import com.framework.listeners.ExtentTestListener;
import org.testng.annotations.Listeners;
import org.testng.annotations.Test;

@Listeners({TestListener.class, ExtentTestListener.class})
public class SampleTest {

    @Test
    public void testExample() {
        System.out.println("Running testExample");
        // test logic
    }
}
```

---

## 4. Building Browser Factory Pattern

### Exercise 4.1: Browser Factory with WebDriverManager

```java
package com.framework.factory;

import io.github.bonigarcia.wdm.WebDriverManager;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.openqa.selenium.edge.EdgeDriver;
import org.openqa.selenium.edge.EdgeOptions;
import org.openqa.selenium.firefox.FirefoxDriver;
import org.openqa.selenium.firefox.FirefoxOptions;

/**
 * Factory that creates WebDriver instances based on the requested browser.
 * Uses WebDriverManager to handle driver binaries automatically.
 */
public class BrowserFactory {

    // Thread-local storage so parallel tests each get their own driver
    private static ThreadLocal<WebDriver> driverThreadLocal = new ThreadLocal<>();

    public enum BrowserType {
        CHROME, FIREFOX, EDGE, CHROME_HEADLESS, FIREFOX_HEADLESS
    }

    /**
     * Create and return a WebDriver instance for the given browser.
     */
    public static WebDriver createDriver(BrowserType browser) {
        WebDriver driver;

        switch (browser) {
            case CHROME:
                WebDriverManager.chromedriver().setup();
                ChromeOptions chromeOptions = new ChromeOptions();
                chromeOptions.addArguments("--start-maximized");
                chromeOptions.addArguments("--disable-notifications");
                driver = new ChromeDriver(chromeOptions);
                break;

            case CHROME_HEADLESS:
                WebDriverManager.chromedriver().setup();
                ChromeOptions headlessChrome = new ChromeOptions();
                headlessChrome.addArguments("--headless=new");
                headlessChrome.addArguments("--window-size=1920,1080");
                headlessChrome.addArguments("--disable-gpu");
                driver = new ChromeDriver(headlessChrome);
                break;

            case FIREFOX:
                WebDriverManager.firefoxdriver().setup();
                FirefoxOptions firefoxOptions = new FirefoxOptions();
                driver = new FirefoxDriver(firefoxOptions);
                driver.manage().window().maximize();
                break;

            case FIREFOX_HEADLESS:
                WebDriverManager.firefoxdriver().setup();
                FirefoxOptions headlessFirefox = new FirefoxOptions();
                headlessFirefox.addArguments("--headless");
                driver = new FirefoxDriver(headlessFirefox);
                break;

            case EDGE:
                WebDriverManager.edgedriver().setup();
                EdgeOptions edgeOptions = new EdgeOptions();
                driver = new EdgeDriver(edgeOptions);
                driver.manage().window().maximize();
                break;

            default:
                throw new IllegalArgumentException("Unsupported browser: " + browser);
        }

        driverThreadLocal.set(driver);
        return driver;
    }

    /**
     * Create a driver from a browser name string (e.g., from config file).
     */
    public static WebDriver createDriver(String browserName) {
        BrowserType type;
        switch (browserName.toLowerCase().trim()) {
            case "chrome":          type = BrowserType.CHROME;          break;
            case "chrome-headless": type = BrowserType.CHROME_HEADLESS; break;
            case "firefox":         type = BrowserType.FIREFOX;         break;
            case "firefox-headless":type = BrowserType.FIREFOX_HEADLESS;break;
            case "edge":            type = BrowserType.EDGE;            break;
            default:
                throw new IllegalArgumentException("Unknown browser: " + browserName);
        }
        return createDriver(type);
    }

    /**
     * Get the driver for the current thread.
     */
    public static WebDriver getDriver() {
        return driverThreadLocal.get();
    }

    /**
     * Quit the driver and clean up thread-local storage.
     */
    public static void quitDriver() {
        WebDriver driver = driverThreadLocal.get();
        if (driver != null) {
            driver.quit();
            driverThreadLocal.remove();
        }
    }
}
```

### Exercise 4.2: BaseTest Using Browser Factory

```java
package com.framework.base;

import com.framework.config.ConfigReader;
import com.framework.factory.BrowserFactory;
import org.openqa.selenium.WebDriver;
import org.testng.annotations.AfterMethod;
import org.testng.annotations.BeforeMethod;
import org.testng.annotations.Optional;
import org.testng.annotations.Parameters;

import java.time.Duration;

/**
 * All test classes extend this base.
 * Browser can be chosen via testng.xml parameter or config.properties.
 */
public class BaseTest {

    protected WebDriver driver;

    @Parameters({"browser"})
    @BeforeMethod
    public void setUp(@Optional String browser) {
        // Priority: testng.xml parameter > system property > config file
        if (browser == null || browser.isEmpty()) {
            browser = System.getProperty("browser",
                    ConfigReader.getProperty("browser"));
        }
        if (browser == null || browser.isEmpty()) {
            browser = "chrome"; // ultimate default
        }

        driver = BrowserFactory.createDriver(browser);
        driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(10));
        driver.manage().timeouts().pageLoadTimeout(Duration.ofSeconds(30));
    }

    @AfterMethod
    public void tearDown() {
        BrowserFactory.quitDriver();
    }

    public WebDriver getDriver() {
        return driver;
    }
}
```

### Exercise 4.3: Cross-Browser testng.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd">
<suite name="Cross Browser Suite" parallel="tests" thread-count="3">

    <test name="Chrome Tests">
        <parameter name="browser" value="chrome"/>
        <classes>
            <class name="com.framework.tests.LoginTest"/>
            <class name="com.framework.tests.HomePageTest"/>
        </classes>
    </test>

    <test name="Firefox Tests">
        <parameter name="browser" value="firefox"/>
        <classes>
            <class name="com.framework.tests.LoginTest"/>
            <class name="com.framework.tests.HomePageTest"/>
        </classes>
    </test>

    <test name="Edge Tests">
        <parameter name="browser" value="edge"/>
        <classes>
            <class name="com.framework.tests.LoginTest"/>
            <class name="com.framework.tests.HomePageTest"/>
        </classes>
    </test>

</suite>
```

### Exercise 4.4: Running Cross-Browser Tests via Maven

```bash
# Run with the default browser from config.properties
mvn clean test

# Override the browser at the command line
mvn clean test -Dbrowser=firefox

# Run a specific suite file
mvn clean test -DsuiteXmlFile=crossbrowser-testng.xml
```

---

## 5. Environment-Specific Configuration

### Exercise 5.1: Configuration Reader Utility

```java
package com.framework.config;

import java.io.FileInputStream;
import java.io.IOException;
import java.util.Properties;

/**
 * Reads key-value pairs from a .properties file.
 * Supports loading different files per environment.
 */
public class ConfigReader {

    private static Properties properties;
    private static final String DEFAULT_ENV = "qa";

    static {
        loadConfig();
    }

    private static void loadConfig() {
        properties = new Properties();

        // Determine the active environment
        String env = System.getProperty("env", DEFAULT_ENV).toLowerCase();
        String configFile = "src/test/resources/config/config-" + env + ".properties";

        try (FileInputStream fis = new FileInputStream(configFile)) {
            properties.load(fis);
            System.out.println("Loaded configuration for environment: " + env);
        } catch (IOException e) {
            System.err.println("Config file not found: " + configFile
                    + " -- falling back to default config.properties");
            try (FileInputStream fis =
                         new FileInputStream("src/test/resources/config.properties")) {
                properties.load(fis);
            } catch (IOException ex) {
                throw new RuntimeException("Could not load any configuration file", ex);
            }
        }
    }

    public static String getProperty(String key) {
        // System properties always take precedence
        String sysValue = System.getProperty(key);
        return sysValue != null ? sysValue : properties.getProperty(key);
    }

    public static String getProperty(String key, String defaultValue) {
        String value = getProperty(key);
        return value != null ? value : defaultValue;
    }

    public static int getIntProperty(String key, int defaultValue) {
        String value = getProperty(key);
        return value != null ? Integer.parseInt(value) : defaultValue;
    }

    public static boolean getBooleanProperty(String key, boolean defaultValue) {
        String value = getProperty(key);
        return value != null ? Boolean.parseBoolean(value) : defaultValue;
    }
}
```

### Exercise 5.2: Environment-Specific Properties Files

**`src/test/resources/config/config-qa.properties`**

```properties
# QA Environment Configuration
base.url=https://qa.example.com
api.base.url=https://api-qa.example.com
browser=chrome
implicit.wait=10
explicit.wait=20
page.load.timeout=30
retry.count=2

# Database
db.host=qa-db.example.com
db.port=3306
db.name=testdb_qa

# Credentials (for demo only -- use a secrets manager in real projects)
admin.username=qa_admin
admin.password=qa_pass123
```

**`src/test/resources/config/config-staging.properties`**

```properties
# Staging Environment Configuration
base.url=https://staging.example.com
api.base.url=https://api-staging.example.com
browser=chrome-headless
implicit.wait=15
explicit.wait=25
page.load.timeout=45
retry.count=1

db.host=staging-db.example.com
db.port=3306
db.name=testdb_staging

admin.username=staging_admin
admin.password=staging_pass123
```

**`src/test/resources/config/config-prod.properties`**

```properties
# Production Environment Configuration
base.url=https://www.example.com
api.base.url=https://api.example.com
browser=chrome-headless
implicit.wait=10
explicit.wait=20
page.load.timeout=30
retry.count=0

db.host=prod-db.example.com
db.port=3306
db.name=testdb_prod

admin.username=prod_readonly
admin.password=prod_readonly_pass
```

### Exercise 5.3: Running Tests Against Different Environments

```bash
# QA (default)
mvn clean test

# Staging
mvn clean test -Denv=staging

# Production
mvn clean test -Denv=prod

# Override individual properties
mvn clean test -Denv=qa -Dbrowser=firefox -Dbase.url=https://custom.example.com
```

### Exercise 5.4: Environment-Aware BaseTest

```java
package com.framework.base;

import com.framework.config.ConfigReader;
import com.framework.factory.BrowserFactory;
import org.openqa.selenium.WebDriver;
import org.testng.annotations.AfterMethod;
import org.testng.annotations.BeforeMethod;
import org.testng.annotations.BeforeSuite;

import java.time.Duration;

public class BaseTest {

    protected WebDriver driver;
    protected String baseUrl;

    @BeforeSuite
    public void suiteSetup() {
        String env = ConfigReader.getProperty("env", "qa");
        System.out.println("========================================");
        System.out.println("  Running tests on environment: " + env.toUpperCase());
        System.out.println("  Base URL: " + ConfigReader.getProperty("base.url"));
        System.out.println("========================================");
    }

    @BeforeMethod
    public void setUp() {
        String browser = ConfigReader.getProperty("browser", "chrome");
        driver = BrowserFactory.createDriver(browser);

        int implicitWait = ConfigReader.getIntProperty("implicit.wait", 10);
        int pageLoad = ConfigReader.getIntProperty("page.load.timeout", 30);
        driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(implicitWait));
        driver.manage().timeouts().pageLoadTimeout(Duration.ofSeconds(pageLoad));

        baseUrl = ConfigReader.getProperty("base.url");
    }

    @AfterMethod
    public void tearDown() {
        BrowserFactory.quitDriver();
    }

    public WebDriver getDriver() {
        return driver;
    }
}
```

---

## 6. Creating Email Notifications on Test Completion

### Exercise 6.1: Add JavaMail Dependency

```xml
<dependency>
    <groupId>com.sun.mail</groupId>
    <artifactId>javax.mail</artifactId>
    <version>1.6.2</version>
</dependency>
```

### Exercise 6.2: Email Utility Class

```java
package com.framework.utils;

import javax.mail.*;
import javax.mail.internet.*;
import java.io.File;
import java.util.Properties;

public class EmailUtil {

    /**
     * Send an email with an optional file attachment (e.g., the Extent report).
     */
    public static void sendEmail(String to, String subject,
                                  String body, String attachmentPath) {

        final String fromEmail = "automation@example.com";
        final String password  = "your-app-password";   // use app passwords or env vars
        final String host      = "smtp.gmail.com";

        Properties props = new Properties();
        props.put("mail.smtp.host", host);
        props.put("mail.smtp.port", "587");
        props.put("mail.smtp.auth", "true");
        props.put("mail.smtp.starttls.enable", "true");

        Session session = Session.getInstance(props, new Authenticator() {
            @Override
            protected PasswordAuthentication getPasswordAuthentication() {
                return new PasswordAuthentication(fromEmail, password);
            }
        });

        try {
            MimeMessage message = new MimeMessage(session);
            message.setFrom(new InternetAddress(fromEmail));
            message.setRecipients(Message.RecipientType.TO,
                    InternetAddress.parse(to));
            message.setSubject(subject);

            // Body part
            MimeBodyPart textPart = new MimeBodyPart();
            textPart.setContent(body, "text/html; charset=utf-8");

            Multipart multipart = new MimeMultipart();
            multipart.addBodyPart(textPart);

            // Attachment part (optional)
            if (attachmentPath != null && new File(attachmentPath).exists()) {
                MimeBodyPart attachPart = new MimeBodyPart();
                attachPart.attachFile(new File(attachmentPath));
                multipart.addBodyPart(attachPart);
            }

            message.setContent(multipart);
            Transport.send(message);
            System.out.println("Email sent successfully to: " + to);

        } catch (Exception e) {
            System.err.println("Failed to send email: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```

### Exercise 6.3: Email-Sending Listener (Post-Suite)

```java
package com.framework.listeners;

import com.framework.config.ConfigReader;
import com.framework.utils.EmailUtil;
import org.testng.ISuite;
import org.testng.ISuiteListener;
import org.testng.ISuiteResult;

import java.io.File;
import java.util.Map;

/**
 * After the entire suite finishes, compose a summary email
 * and attach the latest Extent report.
 */
public class EmailListener implements ISuiteListener {

    @Override
    public void onStart(ISuite suite) {
        // nothing needed before suite
    }

    @Override
    public void onFinish(ISuite suite) {
        // Build summary counts
        int passed = 0, failed = 0, skipped = 0;
        for (ISuiteResult result : suite.getResults().values()) {
            passed  += result.getTestContext().getPassedTests().size();
            failed  += result.getTestContext().getFailedTests().size();
            skipped += result.getTestContext().getSkippedTests().size();
        }
        int total = passed + failed + skipped;

        // Build HTML body
        String body = "<html><body>"
                + "<h2>Test Execution Report</h2>"
                + "<table border='1' cellpadding='6' cellspacing='0'>"
                + "<tr><th>Metric</th><th>Count</th></tr>"
                + "<tr><td>Total Tests</td><td>" + total + "</td></tr>"
                + "<tr style='color:green'><td>Passed</td><td>" + passed + "</td></tr>"
                + "<tr style='color:red'><td>Failed</td><td>" + failed + "</td></tr>"
                + "<tr style='color:orange'><td>Skipped</td><td>" + skipped + "</td></tr>"
                + "</table>"
                + "<p>Please find the detailed report attached.</p>"
                + "</body></html>";

        // Find the latest report file
        String reportPath = findLatestReport();

        // Send the email
        String recipients = ConfigReader.getProperty("email.recipients",
                "team@example.com");
        EmailUtil.sendEmail(recipients,
                "Automation Report - " + suite.getName(),
                body,
                reportPath);
    }

    /**
     * Scan the test-output folder and return the newest .html file.
     */
    private String findLatestReport() {
        File dir = new File(System.getProperty("user.dir") + "/test-output");
        File[] htmlFiles = dir.listFiles((d, name) ->
                name.startsWith("ExtentReport") && name.endsWith(".html"));

        if (htmlFiles == null || htmlFiles.length == 0) return null;

        File latest = htmlFiles[0];
        for (File f : htmlFiles) {
            if (f.lastModified() > latest.lastModified()) {
                latest = f;
            }
        }
        return latest.getAbsolutePath();
    }
}
```

### Exercise 6.4: Register the EmailListener

```xml
<listeners>
    <listener class-name="com.framework.listeners.ExtentTestListener"/>
    <listener class-name="com.framework.listeners.EmailListener"/>
    <listener class-name="com.framework.retry.RetryTransformer"/>
</listeners>
```

### Exercise 6.5: Email Properties in Configuration

Add these to your `config-qa.properties`:

```properties
# Email notification settings
email.enabled=true
email.recipients=qa-lead@example.com,dev-lead@example.com
email.smtp.host=smtp.gmail.com
email.smtp.port=587
email.from=automation@example.com
```

---

## Complete Project Structure

```
src/
 +-- main/java/com/framework/
 |    +-- base/
 |    |    +-- BaseTest.java
 |    +-- config/
 |    |    +-- ConfigReader.java
 |    +-- factory/
 |    |    +-- BrowserFactory.java
 |    +-- listeners/
 |    |    +-- ExtentTestListener.java
 |    |    +-- TestListener.java
 |    |    +-- SuiteListener.java
 |    |    +-- MethodListener.java
 |    |    +-- EmailListener.java
 |    +-- reports/
 |    |    +-- ExtentReportManager.java
 |    +-- retry/
 |    |    +-- RetryAnalyzer.java
 |    |    +-- ConfigurableRetryAnalyzer.java
 |    |    +-- RetryTransformer.java
 |    +-- utils/
 |         +-- ScreenshotUtil.java
 |         +-- EmailUtil.java
 +-- test/
      +-- java/com/framework/tests/
      |    +-- LoginTest.java
      |    +-- HomePageTest.java
      |    +-- RetryDemoTest.java
      +-- resources/
           +-- config/
           |    +-- config-qa.properties
           |    +-- config-staging.properties
           |    +-- config-prod.properties
           +-- testng.xml
           +-- crossbrowser-testng.xml
```

---

## Summary Checklist

| Exercise | Topic | Key Takeaway |
|----------|-------|-------------|
| 1.1-1.5 | Extent Reports | Thread-safe reporting with automatic screenshots on failure |
| 2.1-2.5 | IRetryAnalyzer | Automatic re-run of flaky tests; configurable retry count |
| 3.1-3.5 | TestNG Listeners | Lifecycle hooks for logging, reporting, and custom actions |
| 4.1-4.4 | Browser Factory | ThreadLocal WebDriver; cross-browser from config or CLI |
| 5.1-5.4 | Environment Config | Property files per env; system property overrides |
| 6.1-6.5 | Email Notifications | HTML summary email with report attachment after suite |

> **Next Step:** Phase 6-13 covers interview questions on all these advanced framework topics.
