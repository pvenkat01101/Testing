# Phase 5.3: Framework Development - Part 1 - Theory

## Table of Contents
1. [Introduction to Test Automation Frameworks](#introduction-to-test-automation-frameworks)
2. [Types of Frameworks](#types-of-frameworks)
3. [Hybrid Framework Architecture](#hybrid-framework-architecture)
4. [Configuration Management](#configuration-management)
5. [Data-Driven Framework](#data-driven-framework)
6. [Logging and Reporting](#logging-and-reporting)
7. [Framework Components](#framework-components)
8. [Best Practices](#best-practices)

---

## 1. Introduction to Test Automation Frameworks

### What is a Test Automation Framework?

A **Test Automation Framework** is a set of guidelines, coding standards, concepts, processes, practices, project hierarchies, modularity, reporting mechanism, test data injections, and more that help testers automate testing more efficiently.

### Why Do We Need a Framework?

**Without Framework (Script-Based):**
```java
// Test1.java
driver.get("https://example.com");
driver.findElement(By.id("user")).sendKeys("admin");
driver.findElement(By.id("pass")).sendKeys("pass123");
driver.findElement(By.id("login")).click();

// Test2.java (Duplicate code!)
driver.get("https://example.com");
driver.findElement(By.id("user")).sendKeys("user2");
driver.findElement(By.id("pass")).sendKeys("pass456");
driver.findElement(By.id("login")).click();
```

**With Framework:**
```java
// Centralized configuration, reusable methods, data-driven
@Test(dataProvider = "loginData")
public void testLogin(String username, String password) {
    LoginPage loginPage = new LoginPage(driver);
    loginPage.login(username, password);
    Assert.assertTrue(homePage.isLoggedIn());
}
```

### Framework Benefits:

1. **Reusability:** Common utilities used across tests
2. **Maintainability:** Changes in one place
3. **Scalability:** Easy to add new tests
4. **Readability:** Structured and organized
5. **Reporting:** Automatic test reports
6. **Data Management:** External test data
7. **Configuration:** Centralized settings
8. **Error Handling:** Consistent error management

---

## 2. Types of Frameworks

### 2.1 Linear/Record-Playback Framework

**Simplest form** - Record user actions and play them back.

**Characteristics:**
- No reusability
- Hard to maintain
- Good for quick POC only
- Not recommended for real projects

**Example:**
```java
// Test1 - Recorded steps
driver.get("url1");
driver.findElement(By.id("id1")).click();
driver.findElement(By.id("id2")).sendKeys("text");
// ...50 more lines

// Test2 - Duplicate code
driver.get("url1");
driver.findElement(By.id("id1")).click();
driver.findElement(By.id("id2")).sendKeys("different text");
// ...duplicate steps
```

---

### 2.2 Modular Framework

**Divide application into modules** - Each module has its own test script.

**Structure:**
```
tests/
├── LoginModule.java
├── HomeModule.java
├── ProfileModule.java
└── CheckoutModule.java
```

**Example:**
```java
// LoginModule.java
public class LoginModule {
    public void login(String user, String pass) {
        // Login steps
    }
    
    public void logout() {
        // Logout steps
    }
}

// Test uses modules
@Test
public void testUserJourney() {
    LoginModule loginModule = new LoginModule();
    loginModule.login("user", "pass");
    
    HomeModule homeModule = new HomeModule();
    homeModule.searchProduct("laptop");
    
    loginModule.logout();
}
```

**Pros:** ✅ Better organization, ✅ Some reusability
**Cons:** ❌ Test data hardcoded, ❌ No configuration management

---

### 2.3 Data-Driven Framework

**Separates test data from test logic** - Data stored in external files (Excel, CSV, JSON).

**Structure:**
```
framework/
├── tests/
│   └── LoginTest.java
├── testdata/
│   ├── testdata.xlsx
│   ├── testdata.csv
│   └── testdata.json
└── utils/
    └── ExcelReader.java
```

**Example:**
```java
@Test(dataProvider = "loginData")
public void testLogin(String username, String password, String expected) {
    loginPage.login(username, password);
    
    if(expected.equals("pass")) {
        Assert.assertTrue(homePage.isLoggedIn());
    } else {
        Assert.assertTrue(loginPage.isErrorDisplayed());
    }
}

@DataProvider(name = "loginData")
public Object[][] getLoginData() {
    return ExcelReader.getData("testdata.xlsx", "LoginSheet");
    // Returns: {{"user1", "pass1", "pass"}, {"user2", "wrong", "fail"}}
}
```

**Pros:** ✅ Test data externalized, ✅ Same test, multiple datasets
**Cons:** ❌ Need utility for data reading, ❌ Still needs better structure

---

### 2.4 Keyword-Driven Framework

**Actions represented as keywords** - Non-technical users can create tests.

**Excel Sheet:**
| TestCase | Keyword | Locator | TestData |
|----------|---------|---------|----------|
| TC001 | openBrowser | chrome | |
| TC001 | navigate | | https://example.com |
| TC001 | type | id=username | admin |
| TC001 | type | id=password | pass123 |
| TC001 | click | id=login | |
| TC001 | verify | id=welcome | Welcome |

**Framework Code:**
```java
public void executeKeyword(String keyword, String locator, String testData) {
    switch(keyword) {
        case "openBrowser":
            driver = new ChromeDriver();
            break;
        case "navigate":
            driver.get(testData);
            break;
        case "type":
            driver.findElement(getLocator(locator)).sendKeys(testData);
            break;
        case "click":
            driver.findElement(getLocator(locator)).click();
            break;
        case "verify":
            String actual = driver.findElement(getLocator(locator)).getText();
            Assert.assertTrue(actual.contains(testData));
            break;
    }
}
```

**Pros:** ✅ Non-programmers can write tests, ✅ Highly reusable
**Cons:** ❌ Complex to build, ❌ Maintenance overhead for keywords

---

### 2.5 Hybrid Framework

**Combines multiple framework types** - Best of all worlds.

**Typical Combination:**
- ✅ Data-Driven (external test data)
- ✅ Modular (Page Object Model)
- ✅ Keyword-Driven (optional, for complex actions)
- ✅ Configuration management
- ✅ Logging and reporting
- ✅ Utility functions

**Most companies use Hybrid frameworks.**

---

## 3. Hybrid Framework Architecture

### Typical Structure:

```
selenium-hybrid-framework/
├── src/
│   ├── main/
│   │   └── java/
│   │       ├── pages/               # Page Object Model
│   │       │   ├── BasePage.java
│   │       │   ├── LoginPage.java
│   │       │   └── HomePage.java
│   │       ├── utils/               # Utilities
│   │       │   ├── DriverFactory.java
│   │       │   ├── ConfigReader.java
│   │       │   ├── ExcelReader.java
│   │       │   ├── ScreenshotUtil.java
│   │       │   └── WaitUtil.java
│   │       ├── constants/           # Constants
│   │       │   └── AppConstants.java
│   │       └── listeners/           # TestNG Listeners
│   │           └── TestListener.java
│   └── test/
│       ├── java/
│       │   └── tests/               # Test Classes
│       │       ├── BaseTest.java
│       │       ├── LoginTest.java
│       │       └── HomeTest.java
│       └── resources/
│           ├── config.properties    # Configuration
│           ├── log4j2.xml          # Logging config
│           └── testng.xml          # TestNG suite
├── testdata/                        # Test Data
│   ├── testdata.xlsx
│   └── testdata.json
├── logs/                            # Application logs
├── screenshots/                     # Screenshots
├── test-output/                     # TestNG reports
├── extent-reports/                  # Extent reports
└── pom.xml                         # Maven dependencies
```

### Key Components:

1. **Page Objects:** UI element interactions
2. **Test Classes:** Test logic and assertions
3. **Test Data:** External data files
4. **Configuration:** Environment settings
5. **Utilities:** Reusable helper methods
6. **Reporting:** Test execution reports
7. **Logging:** Debug and info logs

---

## 4. Configuration Management

### config.properties File:

```properties
# Application URLs
base.url=https://www.example.com
qa.url=https://qa.example.com
staging.url=https://staging.example.com

# Browser Configuration
browser=chrome
headless=false
maximize=true

# Timeouts
implicit.wait=10
explicit.wait=15
page.load.timeout=30

# Test Data
test.data.file=./testdata/testdata.xlsx

# Screenshots
screenshot.path=./screenshots/
take.screenshot.on.failure=true

# Reporting
extent.report.path=./extent-reports/
extent.report.name=TestReport.html

# Logging
log.level=INFO
log.file.path=./logs/application.log

# Credentials (Better: use vault/secrets manager)
valid.username=testuser
valid.password=Test@123
```

### ConfigReader.java:

```java
package utils;

import java.io.*;
import java.util.Properties;

public class ConfigReader {
    private static Properties properties;
    private static final String CONFIG_FILE = "./src/test/resources/config.properties";
    
    static {
        try {
            FileInputStream fis = new FileInputStream(CONFIG_FILE);
            properties = new Properties();
            properties.load(fis);
            fis.close();
        } catch(IOException e) {
            e.printStackTrace();
            throw new RuntimeException("Failed to load config.properties");
        }
    }
    
    public static String getProperty(String key) {
        return properties.getProperty(key);
    }
    
    public static String getBaseUrl() {
        return properties.getProperty("base.url");
    }
    
    public static String getBrowser() {
        return properties.getProperty("browser");
    }
    
    public static int getImplicitWait() {
        return Integer.parseInt(properties.getProperty("implicit.wait"));
    }
    
    public static boolean isHeadless() {
        return Boolean.parseBoolean(properties.getProperty("headless"));
    }
    
    public static String getUsername() {
        return properties.getProperty("valid.username");
    }
    
    public static String getPassword() {
        return properties.getProperty("valid.password");
    }
}
```

### Usage:

```java
@Test
public void testLogin() {
    driver.get(ConfigReader.getBaseUrl());
    loginPage.login(
        ConfigReader.getUsername(),
        ConfigReader.getPassword()
    );
}
```

**Benefits:**
- Change config without code changes
- Environment-specific settings
- Security (externalize credentials)
- Easy switching between environments

---

## 5. Data-Driven Framework

### Why Data-Driven?

- **Multiple test scenarios** with same test logic
- **Test data** separated from test scripts
- **Easy maintenance** - Update data, not code
- **Non-technical users** can add test data

### Data Sources:

#### 1. Excel (Most Common)

**testdata.xlsx:**
| Username | Password | ExpectedResult |
|----------|----------|----------------|
| validuser | pass123 | success |
| invaliduser | wrong | failure |
| emptyuser | | failure |

**ExcelReader.java:**
```java
package utils;

import org.apache.poi.ss.usermodel.*;
import org.apache.poi.xssf.usermodel.XSSFWorkbook;
import java.io.*;

public class ExcelReader {
    
    public static Object[][] getTestData(String filePath, String sheetName) {
        Object[][] data = null;
        
        try {
            FileInputStream fis = new FileInputStream(filePath);
            Workbook workbook = new XSSFWorkbook(fis);
            Sheet sheet = workbook.getSheet(sheetName);
            
            int rowCount = sheet.getLastRowNum();
            int colCount = sheet.getRow(0).getLastCellNum();
            
            data = new Object[rowCount][colCount];
            
            for(int i = 0; i < rowCount; i++) {
                Row row = sheet.getRow(i + 1);
                for(int j = 0; j < colCount; j++) {
                    Cell cell = row.getCell(j);
                    data[i][j] = getCellValue(cell);
                }
            }
            
            workbook.close();
            fis.close();
            
        } catch(IOException e) {
            e.printStackTrace();
        }
        
        return data;
    }
    
    private static String getCellValue(Cell cell) {
        if(cell == null) return "";
        
        switch(cell.getCellType()) {
            case STRING:
                return cell.getStringCellValue();
            case NUMERIC:
                return String.valueOf((int) cell.getNumericCellValue());
            case BOOLEAN:
                return String.valueOf(cell.getBooleanCellValue());
            default:
                return "";
        }
    }
}
```

**Test Usage:**
```java
@Test(dataProvider = "loginData")
public void testLogin(String username, String password, String expected) {
    loginPage.login(username, password);
    
    if(expected.equals("success")) {
        Assert.assertTrue(homePage.isLoggedIn());
    } else {
        Assert.assertTrue(loginPage.isErrorDisplayed());
    }
}

@DataProvider(name = "loginData")
public Object[][] getLoginData() {
    return ExcelReader.getTestData("./testdata/testdata.xlsx", "LoginSheet");
}
```

#### 2. JSON

**testdata.json:**
```json
{
  "loginTests": [
    {
      "username": "validuser",
      "password": "pass123",
      "expected": "success"
    },
    {
      "username": "invaliduser",
      "password": "wrong",
      "expected": "failure"
    }
  ]
}
```

**JSONReader.java:**
```java
import org.json.simple.*;
import org.json.simple.parser.*;

public class JSONReader {
    public static JSONArray getTestData(String filePath, String key) {
        try {
            FileReader reader = new FileReader(filePath);
            JSONParser parser = new JSONParser();
            JSONObject jsonObject = (JSONObject) parser.parse(reader);
            return (JSONArray) jsonObject.get(key);
        } catch(Exception e) {
            e.printStackTrace();
            return null;
        }
    }
}
```

#### 3. CSV

**testdata.csv:**
```csv
username,password,expected
validuser,pass123,success
invaliduser,wrong,failure
```

**CSVReader.java:**
```java
import com.opencsv.CSVReader;

public class CSVDataReader {
    public static List<String[]> readCSV(String filePath) {
        try {
            CSVReader reader = new CSVReader(new FileReader(filePath));
            List<String[]> data = reader.readAll();
            reader.close();
            return data;
        } catch(Exception e) {
            e.printStackTrace();
            return null;
        }
    }
}
```

---

## 6. Logging and Reporting

### Why Logging?

- **Debugging:** Understand test execution flow
- **Troubleshooting:** Find issues quickly
- **Audit:** Track what happened
- **Analysis:** Performance and failure analysis

### Log4j2 Configuration:

**log4j2.xml:**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="WARN">
    <Appenders>
        <!-- Console Appender -->
        <Console name="Console" target="SYSTEM_OUT">
            <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
        </Console>
        
        <!-- File Appender -->
        <File name="File" fileName="logs/application.log">
            <PatternLayout pattern="%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n"/>
        </File>
    </Appenders>
    
    <Loggers>
        <Root level="info">
            <AppenderRef ref="Console"/>
            <AppenderRef ref="File"/>
        </Root>
    </Loggers>
</Configuration>
```

**Usage in Tests:**
```java
import org.apache.logging.log4j.*;

public class LoginTest {
    private static final Logger logger = LogManager.getLogger(LoginTest.class);
    
    @Test
    public void testLogin() {
        logger.info("Starting login test");
        
        logger.debug("Navigating to login page");
        driver.get(ConfigReader.getBaseUrl());
        
        logger.info("Entering credentials");
        loginPage.login("user", "pass");
        
        logger.info("Verifying login success");
        Assert.assertTrue(homePage.isLoggedIn());
        
        logger.info("Login test passed");
    }
}
```

### Extent Reports:

**Most popular HTML reporting tool** for Selenium.

**ExtentReportManager.java:**
```java
import com.aventstack.extentreports.*;
import com.aventstack.extentreports.reporter.ExtentSparkReporter;

public class ExtentReportManager {
    private static ExtentReports extent;
    private static ExtentTest test;
    
    public static void initReports() {
        ExtentSparkReporter sparkReporter = new ExtentSparkReporter("extent-reports/TestReport.html");
        sparkReporter.config().setDocumentTitle("Automation Report");
        sparkReporter.config().setReportName("Test Execution Report");
        
        extent = new ExtentReports();
        extent.attachReporter(sparkReporter);
        extent.setSystemInfo("Environment", "QA");
        extent.setSystemInfo("User", "Tester");
    }
    
    public static void createTest(String testName) {
        test = extent.createTest(testName);
    }
    
    public static void logPass(String message) {
        test.pass(message);
    }
    
    public static void logFail(String message) {
        test.fail(message);
    }
    
    public static void logInfo(String message) {
        test.info(message);
    }
    
    public static void attachScreenshot(String screenshotPath) {
        test.addScreenCaptureFromPath(screenshotPath);
    }
    
    public static void flushReports() {
        extent.flush();
    }
}
```

---

## 7. Framework Components Summary

### Essential Components:

1. **DriverFactory:** Browser initialization
2. **Page Objects:** UI interactions
3. **Test Classes:** Test logic
4. **Test Data:** External data files
5. **Configuration:** Properties file
6. **Utilities:** Helper methods
7. **Logging:** Log4j2
8. **Reporting:** Extent Reports
9. **Listeners:** TestNG listeners
10. **CI/CD:** Jenkins/GitHub Actions integration

---

## 8. Best Practices

### Framework Design:

✅ **Use Page Object Model**
✅ **Separate test data from tests**
✅ **Centralized configuration**
✅ **Implement proper waits (explicit over implicit)**
✅ **Screenshot on failure**
✅ **Comprehensive logging**
✅ **Detailed reports (Extent, Allure)**
✅ **Independent tests (no dependencies)**
✅ **Parallel execution support**
✅ **CI/CD integration**

### Code Quality:

✅ **Follow naming conventions**
✅ **Add meaningful comments**
✅ **DRY principle (Don't Repeat Yourself)**
✅ **SOLID principles**
✅ **Exception handling**
✅ **Code reviews**
✅ **Version control (Git)**

### Test Management:

✅ **Organize tests by modules/features**
✅ **Use test groups (smoke, regression, etc.)**
✅ **Priority-based execution**
✅ **Skip flaky tests temporarily**
✅ **Retry failed tests**
✅ **Comprehensive assertions**

---

## Summary

A good automation framework should:
- Be **easy to maintain**
- Support **multiple environments**
- Handle **test data** externally
- Provide **detailed reports**
- Have **comprehensive logging**
- Support **parallel execution**
- Integrate with **CI/CD**
- Follow **best practices** and **design patterns**

**Next:** Practical implementation of hybrid framework with all components!

---

🚀 Ready to build your own automation framework!
