# Phase 3-01: Java Programming Foundation - Theory

## Course Overview
This comprehensive guide covers Java programming fundamentals specifically designed for **automation testers** preparing for **10-year experience positions**. The content focuses on practical testing scenarios, Selenium WebDriver integration, and industry best practices.

---

## 1. Introduction to Java for Testers

### Why Java for Test Automation?

Java is the most widely used language for test automation, particularly with Selenium WebDriver. Here's why:

**Advantages for Testers:**
- **Platform Independence**: Write once, run anywhere (WORA)
- **Rich Ecosystem**: Extensive libraries (TestNG, JUnit, Cucumber, RestAssured)
- **Strong Community Support**: Large community, extensive documentation
- **Enterprise Adoption**: Most enterprises use Java for automation
- **Integration**: Easy integration with CI/CD tools (Jenkins, Maven, Gradle)
- **Object-Oriented**: Helps create maintainable and scalable test frameworks

### Java vs Other Languages for Automation

| Feature | Java | Python | JavaScript | C# |
|---------|------|--------|------------|-----|
| Learning Curve | Moderate | Easy | Easy | Moderate |
| Selenium Support | Excellent | Excellent | Good | Excellent |
| Performance | High | Moderate | Moderate | High |
| Enterprise Use | Very High | Growing | Growing | High |
| Type Safety | Strong (Static) | Weak (Dynamic) | Weak (Dynamic) | Strong (Static) |
| Mobile Testing | Excellent (Appium) | Good | Moderate | Good |
| API Testing | Excellent (RestAssured) | Excellent (Requests) | Good (Axios) | Good (RestSharp) |

### Testing Scenario Example

```java
// Simple Selenium test structure
public class LoginTest {
    WebDriver driver;

    @BeforeMethod
    public void setUp() {
        driver = new ChromeDriver();
        driver.get("https://example.com/login");
    }

    @Test
    public void testValidLogin() {
        driver.findElement(By.id("username")).sendKeys("testuser");
        driver.findElement(By.id("password")).sendKeys("password123");
        driver.findElement(By.id("loginBtn")).click();

        String welcomeMsg = driver.findElement(By.id("welcome")).getText();
        Assert.assertEquals(welcomeMsg, "Welcome, testuser!");
    }

    @AfterMethod
    public void tearDown() {
        driver.quit();
    }
}
```

### Common Mistakes to Avoid
1. **Not understanding Java basics before Selenium**: Master Java first
2. **Copying code without understanding**: Understand the logic
3. **Ignoring OOP principles**: Essential for framework design
4. **Not using proper exception handling**: Leads to unstable tests
5. **Hardcoding values**: Use configuration files

### Interview Tips
- **Q: Why Java for automation?** Focus on stability, enterprise support, rich libraries
- **Q: Java vs Python?** Discuss type safety, performance, enterprise preference
- Be ready to explain OOP concepts with testing examples

---

## 2. Setting Up Java Development Environment

### Components Needed

#### A. Java Development Kit (JDK)

**What is JDK?**
- JDK = JRE (Java Runtime Environment) + Development Tools (compiler, debugger)
- JRE = JVM (Java Virtual Machine) + Libraries

**JDK Version Selection:**
| Version | Release | LTS | Usage in Testing |
|---------|---------|-----|------------------|
| Java 8 | 2014 | Yes | Widely used, Lambda support |
| Java 11 | 2018 | Yes | Modern standard, var keyword |
| Java 17 | 2021 | Yes | Latest LTS, recommended |
| Java 21 | 2023 | Yes | Newest features |

**Installation Steps:**

```bash
# Check if Java is installed
java -version

# Check JDK installation
javac -version

# Set JAVA_HOME environment variable (Windows)
setx JAVA_HOME "C:\Program Files\Java\jdk-17"
setx PATH "%PATH%;%JAVA_HOME%\bin"

# Set JAVA_HOME (Mac/Linux)
export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk-17.jdk/Contents/Home
export PATH=$JAVA_HOME/bin:$PATH
```

#### B. Integrated Development Environment (IDE)

**IntelliJ IDEA (Recommended)**

Advantages:
- Intelligent code completion
- Built-in Maven/Gradle support
- Excellent debugging tools
- Git integration
- Plugin ecosystem

**Installation & Configuration:**

```
1. Download IntelliJ IDEA Community Edition
2. Install and launch
3. Configure JDK: File > Project Structure > SDKs
4. Install useful plugins:
   - TestNG
   - Cucumber for Java
   - Maven Helper
   - Git Integration
   - SonarLint
```

**Eclipse IDE**

Popular alternative, especially in enterprises:
- Free and open-source
- Extensive plugin support
- Lighter than IntelliJ

#### C. Build Tools

**Maven**

```xml
<!-- pom.xml structure -->
<project xmlns="http://maven.apache.org/POM/4.0.0">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.testing</groupId>
    <artifactId>selenium-automation</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <!-- Selenium WebDriver -->
        <dependency>
            <groupId>org.seleniumhq.selenium</groupId>
            <artifactId>selenium-java</artifactId>
            <version>4.16.1</version>
        </dependency>

        <!-- TestNG -->
        <dependency>
            <groupId>org.testng</groupId>
            <artifactId>testng</artifactId>
            <version>7.8.0</version>
        </dependency>
    </dependencies>
</project>
```

**Gradle**

```gradle
// build.gradle
plugins {
    id 'java'
}

group 'com.testing'
version '1.0-SNAPSHOT'

repositories {
    mavenCentral()
}

dependencies {
    testImplementation 'org.seleniumhq.selenium:selenium-java:4.16.1'
    testImplementation 'org.testng:testng:7.8.0'
}

test {
    useTestNG()
}
```

### First Java Program

```java
// HelloWorld.java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, Automation Tester!");
    }
}
```

**Compilation & Execution:**

```bash
# Compile
javac HelloWorld.java

# Run
java HelloWorld

# Output: Hello, Automation Tester!
```

### Common Setup Issues & Solutions

| Issue | Solution |
|-------|----------|
| 'javac' not recognized | Set JAVA_HOME and PATH correctly |
| Wrong Java version | Use `update-alternatives` (Linux) or reinstall |
| IDE not detecting JDK | Point IDE to JDK installation directory |
| Maven dependencies not downloading | Check internet, update Maven settings |

### Interview Tips
- **Q: Difference between JDK, JRE, JVM?** Be clear on each component
- **Q: Why use build tools?** Dependency management, build automation
- Know how to troubleshoot environment issues

---

## 3. Java Basics

### A. Data Types

Java is a **strongly-typed** language. Every variable must have a declared type.

#### Primitive Data Types

| Type | Size | Range | Default | Example |
|------|------|-------|---------|---------|
| byte | 1 byte | -128 to 127 | 0 | byte age = 25; |
| short | 2 bytes | -32,768 to 32,767 | 0 | short year = 2024; |
| int | 4 bytes | -2³¹ to 2³¹-1 | 0 | int count = 100; |
| long | 8 bytes | -2⁶³ to 2⁶³-1 | 0L | long population = 8000000000L; |
| float | 4 bytes | ~7 decimal digits | 0.0f | float price = 99.99f; |
| double | 8 bytes | ~15 decimal digits | 0.0d | double salary = 75000.50; |
| char | 2 bytes | 0 to 65,535 (Unicode) | '\u0000' | char grade = 'A'; |
| boolean | 1 bit | true or false | false | boolean isValid = true; |

**Testing Example:**

```java
public class TestDataTypes {
    public static void main(String[] args) {
        // Test data
        int testCaseCount = 50;
        long executionTime = 120000L; // milliseconds
        float passPercentage = 96.5f;
        double accuracy = 99.9999;
        boolean testPassed = true;
        char priority = 'H'; // High priority

        System.out.println("Total Test Cases: " + testCaseCount);
        System.out.println("Execution Time: " + executionTime + "ms");
        System.out.println("Pass Percentage: " + passPercentage + "%");
        System.out.println("Accuracy: " + accuracy);
        System.out.println("Test Passed: " + testPassed);
        System.out.println("Priority: " + priority);
    }
}
```

**Output:**
```
Total Test Cases: 50
Execution Time: 120000ms
Pass Percentage: 96.5%
Accuracy: 99.9999
Test Passed: true
Priority: H
```

#### Non-Primitive (Reference) Data Types

```java
public class ReferenceTypes {
    public static void main(String[] args) {
        // String
        String browserName = "Chrome";

        // Array
        String[] browsers = {"Chrome", "Firefox", "Safari", "Edge"};

        // Class object
        WebDriver driver = new ChromeDriver();

        // Interface implementation
        List<String> testCases = new ArrayList<>();

        System.out.println("Browser: " + browserName);
        System.out.println("Browsers array length: " + browsers.length);
        System.out.println("First browser: " + browsers[0]);
    }
}
```

### B. Variables

**Variable Declaration & Initialization:**

```java
public class VariablesDemo {
    // Instance variables (non-static)
    String testName;
    int priority;

    // Class/Static variables
    static String environment = "QA";
    static int totalTests = 0;

    public void executeTest() {
        // Local variables
        int attempts = 0;
        boolean passed = false;

        // Final variable (constant)
        final int MAX_RETRIES = 3;

        System.out.println("Environment: " + environment);
        System.out.println("Max Retries: " + MAX_RETRIES);
    }
}
```

**Variable Types Comparison:**

| Type | Scope | Memory | Access | Example Use Case |
|------|-------|---------|--------|------------------|
| Local | Method block | Stack | Within method only | Loop counters, temp values |
| Instance | Object | Heap | Through object | Test data, element locators |
| Static | Class | Heap (Method Area) | Through class name | Configuration, constants |
| Final | Any | Depends | Read-only | URLs, timeout values |

**Naming Conventions:**

```java
public class NamingConventions {
    // Variables: camelCase
    String userName = "admin";
    int retryCount = 3;

    // Constants: UPPER_CASE
    static final String BASE_URL = "https://example.com";
    static final int TIMEOUT_SECONDS = 30;

    // Classes: PascalCase
    class LoginPage { }

    // Methods: camelCase
    public void clickLoginButton() { }

    // Packages: lowercase
    // package com.testing.automation;
}
```

### C. Operators

#### Arithmetic Operators

```java
public class ArithmeticOperators {
    public static void main(String[] args) {
        int totalTests = 100;
        int failedTests = 15;
        int passedTests = totalTests - failedTests;

        // Basic arithmetic
        System.out.println("Addition: " + (10 + 5)); // 15
        System.out.println("Subtraction: " + (10 - 5)); // 5
        System.out.println("Multiplication: " + (10 * 5)); // 50
        System.out.println("Division: " + (10 / 5)); // 2
        System.out.println("Modulus: " + (10 % 3)); // 1

        // Testing scenario
        double passPercentage = ((double) passedTests / totalTests) * 100;
        System.out.println("Pass Percentage: " + passPercentage + "%");
        // Output: Pass Percentage: 85.0%

        // Increment/Decrement
        int count = 0;
        count++; // Post-increment: use then increment
        ++count; // Pre-increment: increment then use
        System.out.println("Count: " + count); // 2

        count--; // Post-decrement
        --count; // Pre-decrement
        System.out.println("Count: " + count); // 0
    }
}
```

#### Comparison Operators

```java
public class ComparisonOperators {
    public static void main(String[] args) {
        int expected = 10;
        int actual = 10;

        System.out.println("Equal: " + (expected == actual)); // true
        System.out.println("Not Equal: " + (expected != actual)); // false
        System.out.println("Greater than: " + (expected > actual)); // false
        System.out.println("Less than: " + (expected < actual)); // false
        System.out.println("Greater or Equal: " + (expected >= actual)); // true
        System.out.println("Less or Equal: " + (expected <= actual)); // true

        // Testing scenario
        String expectedTitle = "Login Page";
        String actualTitle = "Login Page";
        boolean titleMatch = expectedTitle.equals(actualTitle);
        System.out.println("Title Match: " + titleMatch); // true

        // WARNING: Don't use == for String comparison!
        String str1 = new String("Test");
        String str2 = new String("Test");
        System.out.println("== comparison: " + (str1 == str2)); // false
        System.out.println("equals() comparison: " + str1.equals(str2)); // true
    }
}
```

#### Logical Operators

```java
public class LogicalOperators {
    public static void main(String[] args) {
        boolean isLoggedIn = true;
        boolean hasPermission = true;
        boolean isElementVisible = false;

        // AND (&&) - Both conditions must be true
        System.out.println("AND: " + (isLoggedIn && hasPermission)); // true

        // OR (||) - At least one condition must be true
        System.out.println("OR: " + (isLoggedIn || isElementVisible)); // true

        // NOT (!) - Inverts boolean value
        System.out.println("NOT: " + (!isElementVisible)); // true

        // Testing scenario
        boolean testPassed = (isLoggedIn && hasPermission) && !isElementVisible;
        System.out.println("Test Passed: " + testPassed); // false

        // Short-circuit evaluation
        int count = 0;
        if (false && (++count > 0)) {
            // ++count is never executed
        }
        System.out.println("Count: " + count); // 0
    }
}
```

#### Assignment Operators

```java
public class AssignmentOperators {
    public static void main(String[] args) {
        int testCount = 10;

        testCount += 5; // testCount = testCount + 5
        System.out.println("After +=: " + testCount); // 15

        testCount -= 3; // testCount = testCount - 3
        System.out.println("After -=: " + testCount); // 12

        testCount *= 2; // testCount = testCount * 2
        System.out.println("After *=: " + testCount); // 24

        testCount /= 4; // testCount = testCount / 4
        System.out.println("After /=: " + testCount); // 6

        testCount %= 4; // testCount = testCount % 4
        System.out.println("After %=: " + testCount); // 2
    }
}
```

#### Ternary Operator

```java
public class TernaryOperator {
    public static void main(String[] args) {
        // Syntax: condition ? valueIfTrue : valueIfFalse

        int score = 85;
        String result = (score >= 75) ? "Pass" : "Fail";
        System.out.println("Result: " + result); // Pass

        // Testing scenario
        boolean isElementPresent = true;
        String action = isElementPresent ? "Click element" : "Skip test";
        System.out.println("Action: " + action); // Click element

        // Nested ternary (use sparingly)
        int marks = 92;
        String grade = (marks >= 90) ? "A" : (marks >= 80) ? "B" : "C";
        System.out.println("Grade: " + grade); // A
    }
}
```

### D. Type Casting

#### Implicit Casting (Widening)

Automatic conversion from smaller to larger type.

```java
public class ImplicitCasting {
    public static void main(String[] args) {
        // byte -> short -> int -> long -> float -> double

        int intValue = 100;
        long longValue = intValue; // Automatic casting
        double doubleValue = intValue; // Automatic casting

        System.out.println("int: " + intValue); // 100
        System.out.println("long: " + longValue); // 100
        System.out.println("double: " + doubleValue); // 100.0

        // Testing scenario
        int totalTests = 50;
        int passedTests = 45;
        double passPercentage = (passedTests * 100.0) / totalTests;
        System.out.println("Pass %: " + passPercentage); // 90.0
    }
}
```

#### Explicit Casting (Narrowing)

Manual conversion from larger to smaller type (may lose data).

```java
public class ExplicitCasting {
    public static void main(String[] args) {
        double doubleValue = 99.99;
        int intValue = (int) doubleValue; // Explicit casting
        System.out.println("double: " + doubleValue); // 99.99
        System.out.println("int: " + intValue); // 99 (decimal lost)

        // Be careful with data loss
        int largeInt = 130;
        byte byteValue = (byte) largeInt; // Data loss!
        System.out.println("byte: " + byteValue); // -126 (overflow)

        // String to primitive (not casting, but parsing)
        String strNumber = "123";
        int number = Integer.parseInt(strNumber);
        System.out.println("Parsed int: " + number); // 123

        double decimal = Double.parseDouble("45.67");
        System.out.println("Parsed double: " + decimal); // 45.67

        // Primitive to String
        int count = 100;
        String strCount = String.valueOf(count);
        System.out.println("String: " + strCount); // "100"
    }
}
```

**Type Casting in Testing:**

```java
public class TestingTypeCasting {
    public static void main(String[] args) {
        // Selenium example: WebElement to specific type
        // WebElement element = driver.findElement(By.id("input"));
        // String value = element.getAttribute("value"); // Returns String
        // int numericValue = Integer.parseInt(value);

        // Wait time calculation
        double waitTime = 30.5; // seconds
        int timeout = (int) Math.ceil(waitTime); // Round up
        System.out.println("Timeout: " + timeout + " seconds"); // 31

        // Test data conversion
        String testData = "45";
        int expectedValue = Integer.parseInt(testData);
        System.out.println("Expected: " + expectedValue);
    }
}
```

### Common Mistakes to Avoid

1. **Integer Division:**
```java
int a = 5, b = 2;
double result = a / b; // Wrong! Result is 2.0
double correctResult = (double) a / b; // Correct! Result is 2.5
```

2. **String Comparison:**
```java
String s1 = "test";
String s2 = "test";
// Wrong
if (s1 == s2) { } // Compares references, not values
// Correct
if (s1.equals(s2)) { } // Compares values
```

3. **Uninitialized Local Variables:**
```java
public void method() {
    int count; // Not initialized
    // System.out.println(count); // Compilation error!
    count = 0; // Must initialize before use
    System.out.println(count); // Now OK
}
```

4. **Overflow:**
```java
byte b = 127; // Max value for byte
b++; // Overflow! Now b = -128
```

### Interview Tips

**Common Questions:**
1. **Q: Difference between primitive and reference types?**
   - Primitives: Store actual values, stack memory, faster
   - Reference: Store memory addresses, heap memory, slower

2. **Q: Why use 'L' with long and 'f' with float?**
   - Distinguish from int and double literals

3. **Q: Difference between == and .equals()?**
   - ==: Compares references (memory address)
   - .equals(): Compares actual content

4. **Q: What is autoboxing?**
   - Automatic conversion between primitive and wrapper classes
   - Example: int to Integer, boolean to Boolean

**Code Exercise:**
```java
// Write code to calculate test pass percentage
public class PassPercentageCalculator {
    public static void main(String[] args) {
        int totalTests = 150;
        int passedTests = 135;
        int failedTests = totalTests - passedTests;

        double passPercentage = ((double) passedTests / totalTests) * 100;
        double failPercentage = 100 - passPercentage;

        System.out.println("Total Tests: " + totalTests);
        System.out.println("Passed: " + passedTests);
        System.out.println("Failed: " + failedTests);
        System.out.println("Pass %: " + passPercentage);
        System.out.println("Fail %: " + failPercentage);
    }
}
```

---

## 4. Control Structures

Control structures determine the flow of program execution based on conditions and loops.

### A. if-else Statement

**Basic Syntax:**

```java
public class IfElseDemo {
    public static void main(String[] args) {
        int testScore = 85;

        // Simple if
        if (testScore >= 75) {
            System.out.println("Test Passed!");
        }

        // if-else
        if (testScore >= 75) {
            System.out.println("Test Passed!");
        } else {
            System.out.println("Test Failed!");
        }

        // if-else-if ladder
        if (testScore >= 90) {
            System.out.println("Grade: A");
        } else if (testScore >= 80) {
            System.out.println("Grade: B");
        } else if (testScore >= 70) {
            System.out.println("Grade: C");
        } else {
            System.out.println("Grade: F");
        }
    }
}
```

**Testing Scenario:**

```java
public class LoginValidation {
    public static void main(String[] args) {
        String username = "admin";
        String password = "admin123";
        boolean isElementVisible = true;

        // Nested if
        if (username.equals("admin")) {
            if (password.equals("admin123")) {
                if (isElementVisible) {
                    System.out.println("Login successful!");
                } else {
                    System.out.println("Element not visible");
                }
            } else {
                System.out.println("Invalid password");
            }
        } else {
            System.out.println("Invalid username");
        }

        // Better approach using logical operators
        if (username.equals("admin") && password.equals("admin123") && isElementVisible) {
            System.out.println("Login successful!");
        } else {
            System.out.println("Login failed!");
        }
    }
}
```

**Real-World Example:**

```java
public class WebElementValidation {
    public static void validateElement(boolean isDisplayed, boolean isEnabled) {
        if (isDisplayed && isEnabled) {
            System.out.println("Element is ready for interaction");
        } else if (isDisplayed && !isEnabled) {
            System.out.println("Element is displayed but disabled");
        } else if (!isDisplayed && isEnabled) {
            System.out.println("Element is enabled but not visible");
        } else {
            System.out.println("Element is not ready");
        }
    }

    public static void main(String[] args) {
        validateElement(true, true);   // Element is ready for interaction
        validateElement(true, false);  // Element is displayed but disabled
        validateElement(false, true);  // Element is enabled but not visible
        validateElement(false, false); // Element is not ready
    }
}
```

### B. switch Statement

**Basic Syntax:**

```java
public class SwitchDemo {
    public static void main(String[] args) {
        String browser = "chrome";

        switch (browser.toLowerCase()) {
            case "chrome":
                System.out.println("Launching Chrome browser");
                break;
            case "firefox":
                System.out.println("Launching Firefox browser");
                break;
            case "safari":
                System.out.println("Launching Safari browser");
                break;
            case "edge":
                System.out.println("Launching Edge browser");
                break;
            default:
                System.out.println("Invalid browser: " + browser);
        }
    }
}
```

**Output:**
```
Launching Chrome browser
```

**Without break (Fall-through):**

```java
public class SwitchFallThrough {
    public static void main(String[] args) {
        int priority = 1;

        switch (priority) {
            case 1:
                System.out.println("Critical priority");
                // No break - falls through
            case 2:
                System.out.println("High priority");
                break;
            case 3:
                System.out.println("Medium priority");
                break;
            default:
                System.out.println("Low priority");
        }
    }
}
```

**Output:**
```
Critical priority
High priority
```

**Testing Scenarios:**

```java
public class TestEnvironmentSetup {
    public static void setupEnvironment(String env) {
        String url;

        switch (env.toUpperCase()) {
            case "DEV":
                url = "https://dev.example.com";
                System.out.println("Environment: Development");
                System.out.println("URL: " + url);
                break;
            case "QA":
            case "TEST":
                url = "https://qa.example.com";
                System.out.println("Environment: QA/Test");
                System.out.println("URL: " + url);
                break;
            case "STAGING":
                url = "https://staging.example.com";
                System.out.println("Environment: Staging");
                System.out.println("URL: " + url);
                break;
            case "PROD":
            case "PRODUCTION":
                url = "https://www.example.com";
                System.out.println("Environment: Production");
                System.out.println("URL: " + url);
                break;
            default:
                System.out.println("Invalid environment: " + env);
                url = "https://localhost:8080";
                System.out.println("Using default URL: " + url);
        }
    }

    public static void main(String[] args) {
        setupEnvironment("QA");
        System.out.println("---");
        setupEnvironment("PROD");
    }
}
```

**Java 12+ Enhanced Switch (Preview):**

```java
public class EnhancedSwitch {
    public static void main(String[] args) {
        String browser = "chrome";

        // Java 12+ switch expression
        String driver = switch (browser.toLowerCase()) {
            case "chrome" -> "ChromeDriver";
            case "firefox" -> "FirefoxDriver";
            case "safari" -> "SafariDriver";
            case "edge" -> "EdgeDriver";
            default -> "UnknownDriver";
        };

        System.out.println("Using: " + driver);
    }
}
```

**if-else vs switch Comparison:**

| Feature | if-else | switch |
|---------|---------|--------|
| Condition Types | Any boolean expression | Equality checks only |
| Data Types | Any | byte, short, int, char, String, enum |
| Performance | Slower for many conditions | Faster (jump table) |
| Readability | Less readable with many conditions | More readable |
| Range Checks | Supported (>=, <=) | Not supported |
| Use Case | Complex conditions | Discrete value matching |

### C. Loops

#### for Loop

**Basic Syntax:**

```java
public class ForLoopDemo {
    public static void main(String[] args) {
        // Basic for loop
        for (int i = 1; i <= 5; i++) {
            System.out.println("Iteration: " + i);
        }

        // Output:
        // Iteration: 1
        // Iteration: 2
        // Iteration: 3
        // Iteration: 4
        // Iteration: 5
    }
}
```

**Testing Scenarios:**

```java
public class TestDataIteration {
    public static void main(String[] args) {
        // Execute test multiple times
        int testRuns = 3;
        for (int run = 1; run <= testRuns; run++) {
            System.out.println("Executing test run: " + run);
            System.out.println("Test completed\n");
        }

        // Array iteration
        String[] browsers = {"Chrome", "Firefox", "Safari", "Edge"};
        for (int i = 0; i < browsers.length; i++) {
            System.out.println("Testing on: " + browsers[i]);
        }

        // Nested loop - Cross-browser testing
        String[] environments = {"DEV", "QA", "PROD"};
        for (int i = 0; i < browsers.length; i++) {
            for (int j = 0; j < environments.length; j++) {
                System.out.println("Browser: " + browsers[i] +
                                 ", Environment: " + environments[j]);
            }
        }
    }
}
```

#### Enhanced for Loop (for-each)

```java
public class EnhancedForLoop {
    public static void main(String[] args) {
        String[] testCases = {"Login", "Logout", "AddToCart", "Checkout"};

        // Enhanced for loop
        for (String testCase : testCases) {
            System.out.println("Executing: " + testCase);
        }

        // With collections
        List<String> browsers = Arrays.asList("Chrome", "Firefox", "Safari");
        for (String browser : browsers) {
            System.out.println("Browser: " + browser);
        }
    }
}
```

**When to use for vs enhanced for:**

| Scenario | Use |
|----------|-----|
| Need index | Regular for |
| Simple iteration | Enhanced for |
| Modify elements | Regular for |
| Read-only access | Enhanced for |
| Reverse iteration | Regular for |
| All elements forward | Enhanced for |

#### while Loop

```java
public class WhileLoopDemo {
    public static void main(String[] args) {
        int attempts = 0;
        int maxAttempts = 3;
        boolean loginSuccessful = false;

        while (attempts < maxAttempts && !loginSuccessful) {
            attempts++;
            System.out.println("Login attempt: " + attempts);

            // Simulate login logic
            if (attempts == 2) {
                loginSuccessful = true;
                System.out.println("Login successful!");
            } else {
                System.out.println("Login failed, retrying...");
            }
        }

        if (!loginSuccessful) {
            System.out.println("Login failed after " + maxAttempts + " attempts");
        }
    }
}
```

**Testing Scenario - Element Wait:**

```java
public class ElementWaitSimulation {
    public static void main(String[] args) throws InterruptedException {
        int waitTime = 0;
        int maxWait = 10; // seconds
        boolean elementFound = false;

        System.out.println("Waiting for element...");

        while (waitTime < maxWait && !elementFound) {
            Thread.sleep(1000); // Wait 1 second
            waitTime++;

            // Simulate element check
            if (waitTime == 5) {
                elementFound = true;
                System.out.println("Element found after " + waitTime + " seconds");
            } else {
                System.out.println("Waiting... " + waitTime + "s");
            }
        }

        if (!elementFound) {
            System.out.println("Timeout: Element not found after " + maxWait + " seconds");
        }
    }
}
```

#### do-while Loop

```java
public class DoWhileLoop {
    public static void main(String[] args) {
        int retryCount = 0;
        int maxRetries = 3;
        boolean testPassed = false;

        // Executes at least once
        do {
            retryCount++;
            System.out.println("Test execution attempt: " + retryCount);

            // Simulate test logic
            if (retryCount == 2) {
                testPassed = true;
                System.out.println("Test passed!");
            } else {
                System.out.println("Test failed, retrying...");
            }
        } while (retryCount < maxRetries && !testPassed);

        if (!testPassed) {
            System.out.println("Test failed after " + maxRetries + " attempts");
        }
    }
}
```

**while vs do-while:**

| Feature | while | do-while |
|---------|-------|----------|
| Execution | Condition checked first | Executes at least once |
| Use Case | May not execute at all | Must execute once |
| Example | Optional retry | Mandatory first attempt |

### D. break and continue

#### break Statement

```java
public class BreakDemo {
    public static void main(String[] args) {
        // Break in loop
        for (int i = 1; i <= 10; i++) {
            if (i == 5) {
                System.out.println("Breaking at: " + i);
                break; // Exit loop immediately
            }
            System.out.println("Count: " + i);
        }
        System.out.println("Loop ended");

        // Output:
        // Count: 1
        // Count: 2
        // Count: 3
        // Count: 4
        // Breaking at: 5
        // Loop ended
    }
}
```

**Testing Scenario:**

```java
public class TestExecutionBreak {
    public static void main(String[] args) {
        String[] testCases = {"Login", "AddToCart", "Checkout", "Payment", "Confirmation"};
        boolean criticalFailure = false;

        for (String testCase : testCases) {
            System.out.println("Executing: " + testCase);

            // Simulate failure at Checkout
            if (testCase.equals("Checkout")) {
                criticalFailure = true;
                System.out.println("Critical failure in " + testCase);
                break; // Stop execution
            }

            System.out.println(testCase + " - Passed");
        }

        if (criticalFailure) {
            System.out.println("Test suite stopped due to critical failure");
        }
    }
}
```

#### continue Statement

```java
public class ContinueDemo {
    public static void main(String[] args) {
        // Skip specific iterations
        for (int i = 1; i <= 5; i++) {
            if (i == 3) {
                System.out.println("Skipping: " + i);
                continue; // Skip rest of current iteration
            }
            System.out.println("Count: " + i);
        }

        // Output:
        // Count: 1
        // Count: 2
        // Skipping: 3
        // Count: 4
        // Count: 5
    }
}
```

**Testing Scenario:**

```java
public class SkipDisabledTests {
    public static void main(String[] args) {
        String[] testCases = {"Test1", "Test2", "Test3", "Test4", "Test5"};
        boolean[] enabled = {true, false, true, false, true};

        for (int i = 0; i < testCases.length; i++) {
            if (!enabled[i]) {
                System.out.println("Skipping disabled test: " + testCases[i]);
                continue; // Skip this test
            }

            System.out.println("Executing: " + testCases[i]);
            System.out.println(testCases[i] + " - Passed\n");
        }
    }
}
```

**Labeled break and continue:**

```java
public class LabeledBreakContinue {
    public static void main(String[] args) {
        // Labeled break
        outerLoop:
        for (int i = 1; i <= 3; i++) {
            for (int j = 1; j <= 3; j++) {
                if (i == 2 && j == 2) {
                    System.out.println("Breaking outer loop at i=" + i + ", j=" + j);
                    break outerLoop; // Breaks outer loop
                }
                System.out.println("i=" + i + ", j=" + j);
            }
        }

        System.out.println("\n--- Labeled Continue ---\n");

        // Labeled continue
        outer:
        for (int i = 1; i <= 3; i++) {
            for (int j = 1; j <= 3; j++) {
                if (j == 2) {
                    continue outer; // Continue outer loop
                }
                System.out.println("i=" + i + ", j=" + j);
            }
        }
    }
}
```

### Common Mistakes to Avoid

1. **Infinite Loops:**
```java
// Wrong
int i = 0;
while (i < 10) {
    System.out.println(i);
    // Forgot to increment i - infinite loop!
}

// Correct
int i = 0;
while (i < 10) {
    System.out.println(i);
    i++;
}
```

2. **Off-by-One Errors:**
```java
int[] arr = {1, 2, 3, 4, 5};

// Wrong - ArrayIndexOutOfBoundsException
for (int i = 0; i <= arr.length; i++) {
    System.out.println(arr[i]);
}

// Correct
for (int i = 0; i < arr.length; i++) {
    System.out.println(arr[i]);
}
```

3. **Missing break in switch:**
```java
String browser = "chrome";
switch (browser) {
    case "chrome":
        System.out.println("Chrome");
        // Missing break - falls through!
    case "firefox":
        System.out.println("Firefox");
        break;
}
// Output: Chrome\nFirefox (both printed!)
```

4. **Modifying collection while iterating:**
```java
List<String> list = new ArrayList<>(Arrays.asList("A", "B", "C"));

// Wrong - ConcurrentModificationException
for (String item : list) {
    if (item.equals("B")) {
        list.remove(item); // Don't modify during iteration!
    }
}

// Correct - use Iterator
Iterator<String> it = list.iterator();
while (it.hasNext()) {
    if (it.next().equals("B")) {
        it.remove();
    }
}
```

### Interview Tips

**Common Questions:**

1. **Q: Difference between break and continue?**
   - break: Exits the loop entirely
   - continue: Skips current iteration, continues with next

2. **Q: When to use while vs for?**
   - for: Known number of iterations
   - while: Unknown iterations, condition-based

3. **Q: Can you use break without loop/switch?**
   - No, compilation error. Only valid in loops and switch.

4. **Q: What is an infinite loop? Give example.**
```java
while (true) {
    // Infinite loop - no exit condition
    // Use break or return to exit
}
```

5. **Q: Difference between for and enhanced for loop?**
   - for: Has index, can modify, any order
   - enhanced for: No index, read-only, forward only

**Practical Exercise:**

```java
public class TestSuiteExecution {
    public static void main(String[] args) {
        String[] testCases = {
            "ValidLogin", "InvalidLogin", "ForgotPassword",
            "AddToCart", "RemoveFromCart", "Checkout",
            "Payment", "OrderConfirmation"
        };

        String[] priority = {"P0", "P1", "P0", "P1", "P2", "P0", "P0", "P1"};
        boolean[] enabled = {true, true, false, true, true, true, false, true};

        int executed = 0, passed = 0, failed = 0, skipped = 0;

        for (int i = 0; i < testCases.length; i++) {
            // Skip disabled tests
            if (!enabled[i]) {
                System.out.println("[SKIPPED] " + testCases[i] + " - Disabled");
                skipped++;
                continue;
            }

            // Execute only P0 and P1 priority tests
            if (priority[i].equals("P2")) {
                System.out.println("[SKIPPED] " + testCases[i] + " - Low priority");
                skipped++;
                continue;
            }

            executed++;
            System.out.println("[RUNNING] " + testCases[i] + " (Priority: " + priority[i] + ")");

            // Simulate test execution
            boolean testPassed = !testCases[i].equals("InvalidLogin");

            if (testPassed) {
                System.out.println("[PASSED] " + testCases[i]);
                passed++;
            } else {
                System.out.println("[FAILED] " + testCases[i]);
                failed++;
            }

            System.out.println();
        }

        // Summary
        System.out.println("===== TEST SUMMARY =====");
        System.out.println("Total Tests: " + testCases.length);
        System.out.println("Executed: " + executed);
        System.out.println("Passed: " + passed);
        System.out.println("Failed: " + failed);
        System.out.println("Skipped: " + skipped);
        System.out.println("Pass Rate: " + (passed * 100.0 / executed) + "%");
    }
}
```

---

## 5. Object-Oriented Programming (OOP)

Object-Oriented Programming is the foundation of Java and essential for building maintainable test automation frameworks.

### A. Classes and Objects

**What is a Class?**
- Blueprint/template for creating objects
- Defines properties (fields) and behaviors (methods)
- Does not occupy memory until object is created

**What is an Object?**
- Instance of a class
- Occupies memory
- Has state (field values) and behavior (methods)

**Basic Syntax:**

```java
// Class definition
public class LoginPage {
    // Fields (instance variables)
    String url;
    String username;
    String password;

    // Method
    public void login() {
        System.out.println("Logging in with username: " + username);
    }

    public void displayInfo() {
        System.out.println("URL: " + url);
        System.out.println("Username: " + username);
    }
}

// Main class to create objects
public class TestLoginPage {
    public static void main(String[] args) {
        // Creating object (instantiation)
        LoginPage page1 = new LoginPage();

        // Setting values
        page1.url = "https://example.com/login";
        page1.username = "testuser1";
        page1.password = "pass123";

        // Calling methods
        page1.displayInfo();
        page1.login();

        // Creating another object
        LoginPage page2 = new LoginPage();
        page2.url = "https://qa.example.com/login";
        page2.username = "testuser2";
        page2.password = "pass456";

        page2.displayInfo();
        page2.login();
    }
}
```

**Output:**
```
URL: https://example.com/login
Username: testuser1
Logging in with username: testuser1
URL: https://qa.example.com/login
Username: testuser2
Logging in with username: testuser2
```

### B. Constructors

**What is a Constructor?**
- Special method to initialize objects
- Same name as class
- No return type (not even void)
- Called automatically when object is created

**Types of Constructors:**

```java
public class WebPage {
    String title;
    String url;
    int loadTime;

    // 1. Default Constructor (no parameters)
    public WebPage() {
        title = "Default Title";
        url = "https://default.com";
        loadTime = 0;
        System.out.println("Default constructor called");
    }

    // 2. Parameterized Constructor
    public WebPage(String title, String url) {
        this.title = title;
        this.url = url;
        this.loadTime = 0;
        System.out.println("Parameterized constructor called");
    }

    // 3. Constructor Overloading
    public WebPage(String title, String url, int loadTime) {
        this.title = title;
        this.url = url;
        this.loadTime = loadTime;
        System.out.println("Fully parameterized constructor called");
    }

    public void displayInfo() {
        System.out.println("Title: " + title);
        System.out.println("URL: " + url);
        System.out.println("Load Time: " + loadTime + "ms\n");
    }
}

public class TestWebPage {
    public static void main(String[] args) {
        // Using default constructor
        WebPage page1 = new WebPage();
        page1.displayInfo();

        // Using parameterized constructor
        WebPage page2 = new WebPage("Google", "https://google.com");
        page2.displayInfo();

        // Using fully parameterized constructor
        WebPage page3 = new WebPage("Amazon", "https://amazon.com", 1500);
        page3.displayInfo();
    }
}
```

**Constructor Chaining:**

```java
public class TestCase {
    String name;
    String priority;
    String browser;
    boolean enabled;

    // Constructor 1
    public TestCase(String name) {
        this(name, "P1"); // Calls constructor 2
        System.out.println("Constructor 1 called");
    }

    // Constructor 2
    public TestCase(String name, String priority) {
        this(name, priority, "Chrome"); // Calls constructor 3
        System.out.println("Constructor 2 called");
    }

    // Constructor 3
    public TestCase(String name, String priority, String browser) {
        this(name, priority, browser, true); // Calls constructor 4
        System.out.println("Constructor 3 called");
    }

    // Constructor 4 (Master constructor)
    public TestCase(String name, String priority, String browser, boolean enabled) {
        this.name = name;
        this.priority = priority;
        this.browser = browser;
        this.enabled = enabled;
        System.out.println("Constructor 4 (Master) called");
    }

    public void displayInfo() {
        System.out.println("\nTest Case Details:");
        System.out.println("Name: " + name);
        System.out.println("Priority: " + priority);
        System.out.println("Browser: " + browser);
        System.out.println("Enabled: " + enabled);
    }

    public static void main(String[] args) {
        TestCase tc = new TestCase("LoginTest");
        tc.displayInfo();
    }
}
```

**Output:**
```
Constructor 4 (Master) called
Constructor 3 called
Constructor 2 called
Constructor 1 called

Test Case Details:
Name: LoginTest
Priority: P1
Browser: Chrome
Enabled: true
```

### C. Methods

**Method Structure:**

```java
accessModifier returnType methodName(parameters) {
    // method body
    return value; // if return type is not void
}
```

**Types of Methods:**

```java
public class Calculator {
    // 1. Method with no parameters, no return
    public void displayWelcome() {
        System.out.println("Welcome to Calculator");
    }

    // 2. Method with parameters, no return
    public void printSum(int a, int b) {
        System.out.println("Sum: " + (a + b));
    }

    // 3. Method with no parameters, with return
    public String getMessage() {
        return "Calculator is ready";
    }

    // 4. Method with parameters and return
    public int add(int a, int b) {
        return a + b;
    }

    // 5. Method overloading (same name, different parameters)
    public int add(int a, int b, int c) {
        return a + b + c;
    }

    public double add(double a, double b) {
        return a + b;
    }

    public static void main(String[] args) {
        Calculator calc = new Calculator();

        calc.displayWelcome();
        calc.printSum(5, 3);

        String msg = calc.getMessage();
        System.out.println(msg);

        int sum1 = calc.add(10, 20);
        int sum2 = calc.add(10, 20, 30);
        double sum3 = calc.add(10.5, 20.5);

        System.out.println("Sum1: " + sum1);
        System.out.println("Sum2: " + sum2);
        System.out.println("Sum3: " + sum3);
    }
}
```

**Testing Example:**

```java
public class TestUtility {
    // Generate random email
    public String generateEmail(String prefix) {
        long timestamp = System.currentTimeMillis();
        return prefix + timestamp + "@test.com";
    }

    // Verify page title
    public boolean verifyTitle(String actualTitle, String expectedTitle) {
        return actualTitle.equals(expectedTitle);
    }

    // Wait for element
    public void waitForElement(int seconds) {
        try {
            Thread.sleep(seconds * 1000);
            System.out.println("Waited for " + seconds + " seconds");
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    // Get test data (varargs example)
    public void printTestData(String testName, String... data) {
        System.out.println("Test: " + testName);
        System.out.println("Data: ");
        for (String item : data) {
            System.out.println("  - " + item);
        }
    }

    public static void main(String[] args) {
        TestUtility util = new TestUtility();

        // Generate email
        String email = util.generateEmail("testuser");
        System.out.println("Generated Email: " + email);

        // Verify title
        boolean isValid = util.verifyTitle("Login Page", "Login Page");
        System.out.println("Title valid: " + isValid);

        // Wait
        util.waitForElement(2);

        // Print test data (varargs)
        util.printTestData("LoginTest", "admin", "password123", "Chrome");
    }
}
```

### D. The 'this' Keyword

**Usage of 'this':**

1. **Refer to current instance variable**
2. **Invoke current instance method**
3. **Invoke current class constructor**
4. **Return current instance**
5. **Pass as argument**

```java
public class WebElement {
    String locator;
    String type;

    // 1. Refer to instance variable
    public WebElement(String locator, String type) {
        this.locator = locator; // 'this.locator' is instance variable
        this.type = type;       // 'locator' is parameter
    }

    // 2. Invoke current instance method
    public void click() {
        this.waitForElement(); // Optional 'this'
        System.out.println("Clicking on element: " + locator);
    }

    public void waitForElement() {
        System.out.println("Waiting for element: " + locator);
    }

    // 3. Constructor chaining already shown above

    // 4. Return current instance (Method chaining)
    public WebElement sendKeys(String text) {
        System.out.println("Typing '" + text + "' in element: " + locator);
        return this; // Returns current object
    }

    public WebElement clear() {
        System.out.println("Clearing element: " + locator);
        return this;
    }

    public void submit() {
        System.out.println("Submitting element: " + locator);
    }

    public static void main(String[] args) {
        WebElement element = new WebElement("//input[@id='username']", "xpath");

        // Method chaining (possible because methods return 'this')
        element.clear()
               .sendKeys("testuser")
               .submit();
    }
}
```

**Output:**
```
Clearing element: //input[@id='username']
Typing 'testuser' in element: //input[@id='username']
Submitting element: //input[@id='username']
```

**Passing 'this' as argument:**

```java
public class TestReport {
    String testName;

    public TestReport(String testName) {
        this.testName = testName;
    }

    public void generateReport() {
        ReportGenerator generator = new ReportGenerator();
        generator.create(this); // Passing current object
    }
}

class ReportGenerator {
    public void create(TestReport report) {
        System.out.println("Generating report for: " + report.testName);
    }
}
```

### E. Static vs Instance Members

**Instance Members:**
- Belong to object
- Accessed through object reference
- Each object has its own copy

**Static Members:**
- Belong to class
- Accessed through class name
- Shared by all objects

```java
public class TestConfig {
    // Instance variables
    String testName;
    String browser;

    // Static variables
    static String environment = "QA";
    static int totalTests = 0;
    static final String BASE_URL = "https://example.com"; // Static constant

    // Constructor
    public TestConfig(String testName, String browser) {
        this.testName = testName;
        this.browser = browser;
        totalTests++; // Increment count for each test created
    }

    // Instance method
    public void displayTestInfo() {
        System.out.println("Test: " + testName);
        System.out.println("Browser: " + browser);
        System.out.println("Environment: " + environment); // Can access static
    }

    // Static method
    public static void displayEnvironment() {
        System.out.println("Environment: " + environment);
        System.out.println("Total Tests: " + totalTests);
        // System.out.println(testName); // ERROR! Cannot access instance variable
    }

    public static void main(String[] args) {
        // Access static members without object
        System.out.println("Base URL: " + TestConfig.BASE_URL);
        TestConfig.displayEnvironment();

        System.out.println("\n--- Creating Test Objects ---\n");

        // Create objects
        TestConfig test1 = new TestConfig("LoginTest", "Chrome");
        TestConfig test2 = new TestConfig("LogoutTest", "Firefox");
        TestConfig test3 = new TestConfig("SearchTest", "Safari");

        // Instance method calls
        test1.displayTestInfo();
        System.out.println();
        test2.displayTestInfo();
        System.out.println();
        test3.displayTestInfo();

        System.out.println("\n--- Static Method Call ---\n");

        // Static method call
        TestConfig.displayEnvironment(); // totalTests = 3
    }
}
```

**Output:**
```
Base URL: https://example.com
Environment: QA
Total Tests: 0

--- Creating Test Objects ---

Test: LoginTest
Browser: Chrome
Environment: QA

Test: LogoutTest
Browser: Firefox
Environment: QA

Test: SearchTest
Browser: Safari
Environment: QA

--- Static Method Call ---

Environment: QA
Total Tests: 3
```

**Static Block:**

```java
public class DriverManager {
    static WebDriver driver;
    static String driverPath;

    // Static block - executes once when class is loaded
    static {
        System.out.println("Static block executed");
        driverPath = "/usr/local/bin/chromedriver";
        System.out.println("Driver path set: " + driverPath);
    }

    public static void initializeDriver() {
        System.out.println("Initializing driver...");
        // driver = new ChromeDriver();
    }

    public static void main(String[] args) {
        System.out.println("Main method started");
        DriverManager.initializeDriver();
    }
}
```

**Output:**
```
Static block executed
Driver path set: /usr/local/bin/chromedriver
Main method started
Initializing driver...
```

### Common Mistakes to Avoid

1. **Confusing Class and Object:**
```java
// Wrong - treating class as object
LoginPage.login(); // ERROR if login() is not static

// Correct
LoginPage page = new LoginPage();
page.login();
```

2. **Not using 'this' with same-named variables:**
```java
public class Test {
    String name;

    // Wrong - ambiguous
    public Test(String name) {
        name = name; // Assigns parameter to itself!
    }

    // Correct
    public Test(String name) {
        this.name = name; // Assigns parameter to instance variable
    }
}
```

3. **Accessing instance members from static context:**
```java
public class Example {
    int instanceVar = 10;

    public static void staticMethod() {
        // System.out.println(instanceVar); // ERROR!

        // Correct way
        Example obj = new Example();
        System.out.println(obj.instanceVar); // OK
    }
}
```

4. **Creating unnecessary objects:**
```java
// Wrong - creating object for static method
TestUtils utils = new TestUtils();
utils.staticMethod();

// Correct
TestUtils.staticMethod();
```

### Interview Tips

**Common Questions:**

1. **Q: Difference between class and object?**
   - Class: Blueprint, template, no memory
   - Object: Instance of class, occupies memory

2. **Q: Can we have multiple classes in a Java file?**
   - Yes, but only one can be public
   - Public class name must match filename

3. **Q: What is constructor chaining?**
   - Calling one constructor from another using this()

4. **Q: Can constructor be private?**
   - Yes! Used in Singleton pattern

5. **Q: Difference between static and instance method?**
   - Static: Belongs to class, no object needed
   - Instance: Belongs to object, needs object

6. **Q: What happens if we don't define a constructor?**
   - Java provides default no-arg constructor
   - If we define any constructor, default is not provided

**Singleton Pattern Example (Common in Automation):**

```java
public class DriverManager {
    private static DriverManager instance;
    private WebDriver driver;

    // Private constructor
    private DriverManager() {
        // driver = new ChromeDriver();
    }

    // Public static method to get instance
    public static DriverManager getInstance() {
        if (instance == null) {
            instance = new DriverManager();
        }
        return instance;
    }

    public WebDriver getDriver() {
        return driver;
    }
}

// Usage
// DriverManager dm1 = DriverManager.getInstance();
// DriverManager dm2 = DriverManager.getInstance();
// dm1 == dm2 returns true (same instance)
```

---

*This file continues with sections 6-17. Would you like me to continue with the next sections?*

## 6. Access Modifiers

Access modifiers control the visibility and accessibility of classes, methods, and variables.

### Types of Access Modifiers

| Modifier | Class | Package | Subclass | World |
|----------|-------|---------|----------|-------|
| public | Yes | Yes | Yes | Yes |
| protected | Yes | Yes | Yes | No |
| default (no modifier) | Yes | Yes | No | No |
| private | Yes | No | No | No |

### A. public

```java
// Accessible from anywhere
public class PublicExample {
    public String url = "https://example.com";

    public void openBrowser() {
        System.out.println("Browser opened");
    }
}

// Another class in different package
public class TestClass {
    public static void main(String[] args) {
        PublicExample obj = new PublicExample();
        System.out.println(obj.url); // Accessible
        obj.openBrowser(); // Accessible
    }
}
```

### B. private

```java
public class LoginPage {
    private String username = "admin";
    private String password = "admin123";

    // Private method
    private void validateCredentials() {
        System.out.println("Validating credentials");
    }

    // Public method to access private members
    public void login(String user, String pass) {
        this.username = user;
        this.password = pass;
        validateCredentials(); // Can call private method within class
        System.out.println("Login successful");
    }

    // Getter method
    public String getUsername() {
        return username;
    }

    // Setter method
    public void setUsername(String username) {
        this.username = username;
    }
}

public class TestLogin {
    public static void main(String[] args) {
        LoginPage page = new LoginPage();
        // System.out.println(page.username); // ERROR! Private member
        // page.validateCredentials(); // ERROR! Private method

        // Access through public methods
        System.out.println(page.getUsername()); // OK
        page.setUsername("testuser"); // OK
        page.login("testuser", "pass123"); // OK
    }
}
```

### C. protected

```java
package com.testing.pages;

public class BasePage {
    protected String baseUrl = "https://example.com";

    protected void waitForPageLoad() {
        System.out.println("Waiting for page load");
    }
}

// Same package
package com.testing.pages;

public class HomePage extends BasePage {
    public void navigate() {
        System.out.println("URL: " + baseUrl); // Accessible (same package)
        waitForPageLoad(); // Accessible (inheritance)
    }
}

// Different package
package com.testing.tests;

import com.testing.pages.BasePage;

public class LoginTest extends BasePage {
    public void test() {
        System.out.println("URL: " + baseUrl); // Accessible (through inheritance)
        waitForPageLoad(); // Accessible (through inheritance)
    }
}

// Different package, no inheritance
package com.testing.utils;

import com.testing.pages.BasePage;

public class TestUtil {
    public void method() {
        BasePage page = new BasePage();
        // System.out.println(page.baseUrl); // ERROR! Not accessible
        // page.waitForPageLoad(); // ERROR! Not accessible
    }
}
```

### D. default (package-private)

```java
// No access modifier = default
package com.testing.pages;

class DefaultPage {
    String title = "Default Title";

    void displayTitle() {
        System.out.println("Title: " + title);
    }
}

// Same package - Accessible
package com.testing.pages;

public class TestSamePackage {
    public static void main(String[] args) {
        DefaultPage page = new DefaultPage();
        System.out.println(page.title); // OK
        page.displayTitle(); // OK
    }
}

// Different package - Not accessible
package com.testing.tests;

public class TestDifferentPackage {
    public static void main(String[] args) {
        // DefaultPage page = new DefaultPage(); // ERROR! Not visible
    }
}
```

### Testing Framework Example

```java
package com.framework.base;

public class BaseTest {
    // Public - accessible everywhere
    public String environment = "QA";

    // Protected - accessible in subclasses
    protected void setUp() {
        System.out.println("Base setUp");
    }

    // Default - accessible in same package
    void initializeReport() {
        System.out.println("Initializing report");
    }

    // Private - only within this class
    private void loadConfig() {
        System.out.println("Loading config");
    }

    public void runTest() {
        loadConfig(); // Can call private method
        setUp(); // Can call protected method
        initializeReport(); // Can call default method
    }
}

package com.framework.tests;

import com.framework.base.BaseTest;

public class LoginTest extends BaseTest {
    @Override
    protected void setUp() {
        super.setUp(); // Call parent setUp
        System.out.println("LoginTest setUp");
    }

    public void testLogin() {
        System.out.println("Environment: " + environment); // public - OK
        setUp(); // protected - OK (through inheritance)
        // initializeReport(); // ERROR! default - not accessible (different package)
        // loadConfig(); // ERROR! private - not accessible
    }
}
```

### Common Mistakes to Avoid

1. **Making everything public:**
```java
// Wrong - exposes internal implementation
public class LoginPage {
    public String username; // Anyone can modify
    public String password; // Security risk!
}

// Correct - encapsulation
public class LoginPage {
    private String username;
    private String password;

    public void setCredentials(String username, String password) {
        this.username = username;
        this.password = password;
    }
}
```

2. **Using default access unintentionally:**
```java
// Missing public - not accessible outside package
class TestUtil { // Should be: public class TestUtil
    void helper() { } // Should be: public void helper()
}
```

3. **Protected misuse:**
```java
// Don't use protected just for testing
protected void internalMethod() {
    // Use default or private instead
}
```

### Interview Tips

**Q: When to use which access modifier?**
- **public**: Public API, exposed functionality
- **protected**: Methods to be used/overridden by subclasses
- **default**: Package-level utilities
- **private**: Internal implementation details

**Q: Why use getters/setters if we can make fields public?**
- Encapsulation and data hiding
- Validation before setting values
- Can change internal implementation without affecting API

**Q: Can we have a private constructor?**
- Yes! Used in Singleton pattern and utility classes

---

## 7. Inheritance and Polymorphism

### A. Inheritance

Inheritance allows a class to acquire properties and methods of another class.

**Syntax:**
```java
class Child extends Parent {
    // Child class code
}
```

**Types of Inheritance in Java:**

| Type | Support | Example |
|------|---------|---------|
| Single | Yes | Class B extends Class A |
| Multilevel | Yes | Class C extends B extends A |
| Hierarchical | Yes | Classes B, C extend A |
| Multiple | No (via classes) | Not supported |
| Hybrid | Yes (via interfaces) | Combination |

**Single Inheritance:**

```java
// Parent class
class BasePage {
    String url;
    String title;

    public void openPage() {
        System.out.println("Opening page: " + url);
    }

    public void getTitle() {
        System.out.println("Title: " + title);
    }
}

// Child class
class LoginPage extends BasePage {
    String username;
    String password;

    public void login() {
        System.out.println("Logging in with: " + username);
    }

    public void displayInfo() {
        System.out.println("URL: " + url); // Inherited field
        System.out.println("Title: " + title); // Inherited field
        openPage(); // Inherited method
        login(); // Own method
    }
}

public class InheritanceTest {
    public static void main(String[] args) {
        LoginPage page = new LoginPage();
        page.url = "https://example.com/login";
        page.title = "Login Page";
        page.username = "admin";
        page.password = "admin123";

        page.displayInfo();
    }
}
```

**Multilevel Inheritance:**

```java
// Level 1
class WebPage {
    public void load() {
        System.out.println("Loading webpage");
    }
}

// Level 2
class FormPage extends WebPage {
    public void fillForm() {
        System.out.println("Filling form");
    }
}

// Level 3
class RegistrationPage extends FormPage {
    public void register() {
        System.out.println("Registering user");
    }
}

public class MultilevelTest {
    public static void main(String[] args) {
        RegistrationPage page = new RegistrationPage();
        page.load(); // From WebPage (grandparent)
        page.fillForm(); // From FormPage (parent)
        page.register(); // Own method
    }
}
```

**Hierarchical Inheritance:**

```java
class TestCase {
    public void setUp() {
        System.out.println("Test setup");
    }

    public void tearDown() {
        System.out.println("Test teardown");
    }
}

class LoginTest extends TestCase {
    public void testLogin() {
        System.out.println("Testing login");
    }
}

class LogoutTest extends TestCase {
    public void testLogout() {
        System.out.println("Testing logout");
    }
}

class SearchTest extends TestCase {
    public void testSearch() {
        System.out.println("Testing search");
    }
}

public class HierarchicalTest {
    public static void main(String[] args) {
        LoginTest loginTest = new LoginTest();
        loginTest.setUp();
        loginTest.testLogin();
        loginTest.tearDown();

        System.out.println();

        LogoutTest logoutTest = new LogoutTest();
        logoutTest.setUp();
        logoutTest.testLogout();
        logoutTest.tearDown();
    }
}
```

**super Keyword:**

```java
class ParentTest {
    String name = "Parent";

    public ParentTest() {
        System.out.println("Parent constructor");
    }

    public ParentTest(String name) {
        this.name = name;
        System.out.println("Parent parameterized constructor: " + name);
    }

    public void display() {
        System.out.println("Parent display");
    }
}

class ChildTest extends ParentTest {
    String name = "Child";

    public ChildTest() {
        super(); // Call parent no-arg constructor
        System.out.println("Child constructor");
    }

    public ChildTest(String name) {
        super(name); // Call parent parameterized constructor
        System.out.println("Child parameterized constructor: " + name);
    }

    public void display() {
        System.out.println("Child name: " + name);
        System.out.println("Parent name: " + super.name);
        super.display(); // Call parent method
    }

    public void show() {
        display(); // Calls child's display()
        super.display(); // Calls parent's display()
    }
}

public class SuperKeywordTest {
    public static void main(String[] args) {
        ChildTest child = new ChildTest("Test");
        System.out.println();
        child.display();
        System.out.println();
        child.show();
    }
}
```

### B. Polymorphism

**Poly** = Many, **Morphism** = Forms → One interface, multiple implementations

**Types:**
1. **Compile-time (Static) Polymorphism** - Method Overloading
2. **Runtime (Dynamic) Polymorphism** - Method Overriding

#### Method Overloading (Compile-time Polymorphism)

Same method name, different parameters (number, type, or order)

```java
public class TestUtility {
    // Method 1: No parameters
    public void log() {
        System.out.println("Log message");
    }

    // Method 2: One parameter
    public void log(String message) {
        System.out.println("Log: " + message);
    }

    // Method 3: Two parameters
    public void log(String level, String message) {
        System.out.println("[" + level + "] " + message);
    }

    // Method 4: Different type
    public void log(int errorCode) {
        System.out.println("Error code: " + errorCode);
    }

    // Method 5: Different order
    public void log(int code, String message) {
        System.out.println("Error " + code + ": " + message);
    }

    public void log(String message, int code) {
        System.out.println(message + " (Code: " + code + ")");
    }

    public static void main(String[] args) {
        TestUtility util = new TestUtility();

        util.log(); // Calls method 1
        util.log("Test message"); // Calls method 2
        util.log("INFO", "Application started"); // Calls method 3
        util.log(404); // Calls method 4
        util.log(500, "Internal server error"); // Calls method 5
        util.log("Not found", 404); // Calls method 6
    }
}
```

**Testing Example:**

```java
public class ElementLocator {
    // Find by ID
    public WebElement findElement(String id) {
        System.out.println("Finding by ID: " + id);
        return null; // Placeholder
    }

    // Find by locator type and value
    public WebElement findElement(String locatorType, String value) {
        System.out.println("Finding by " + locatorType + ": " + value);
        return null;
    }

    // Find with wait time
    public WebElement findElement(String id, int waitSeconds) {
        System.out.println("Finding " + id + " with wait: " + waitSeconds + "s");
        return null;
    }

    // Find multiple elements
    public List<WebElement> findElements(String locator) {
        System.out.println("Finding multiple elements: " + locator);
        return null;
    }
}
```

#### Method Overriding (Runtime Polymorphism)

Child class provides specific implementation of parent class method

**Rules:**
- Same method signature (name, parameters, return type)
- Access modifier: same or more permissive
- Cannot override: static, final, private methods
- Use @Override annotation

```java
class Browser {
    public void launch() {
        System.out.println("Launching browser");
    }

    public void close() {
        System.out.println("Closing browser");
    }

    public void maximize() {
        System.out.println("Maximizing window");
    }
}

class ChromeBrowser extends Browser {
    @Override
    public void launch() {
        System.out.println("Launching Chrome browser");
        System.out.println("Chrome version: 120.0");
    }

    @Override
    public void close() {
        System.out.println("Closing Chrome browser");
        System.out.println("Clearing cache");
    }

    // Additional method specific to Chrome
    public void openDevTools() {
        System.out.println("Opening Chrome DevTools");
    }
}

class FirefoxBrowser extends Browser {
    @Override
    public void launch() {
        System.out.println("Launching Firefox browser");
        System.out.println("Firefox version: 121.0");
    }

    @Override
    public void close() {
        System.out.println("Closing Firefox browser");
    }
}

public class PolymorphismDemo {
    public static void main(String[] args) {
        // Upcasting - Parent reference, Child object
        Browser browser1 = new ChromeBrowser();
        Browser browser2 = new FirefoxBrowser();

        browser1.launch(); // Calls ChromeBrowser's launch()
        browser1.maximize(); // Calls Browser's maximize()
        browser1.close(); // Calls ChromeBrowser's close()

        System.out.println();

        browser2.launch(); // Calls FirefoxBrowser's launch()
        browser2.close(); // Calls FirefoxBrowser's close()

        // browser1.openDevTools(); // ERROR! Parent reference can't see child-specific methods

        // Downcasting - Access child-specific method
        ChromeBrowser chrome = (ChromeBrowser) browser1;
        chrome.openDevTools(); // Now OK
    }
}
```

**Output:**
```
Launching Chrome browser
Chrome version: 120.0
Maximizing window
Closing Chrome browser
Clearing cache

Launching Firefox browser
Firefox version: 121.0
Closing Firefox browser
Opening Chrome DevTools
```

**Real-World Testing Example:**

```java
// Base class
abstract class TestCase {
    public void setUp() {
        System.out.println("Common setup");
    }

    public abstract void executeTest(); // Must be overridden

    public void tearDown() {
        System.out.println("Common teardown");
    }

    // Template method pattern
    public void run() {
        setUp();
        executeTest();
        tearDown();
    }
}

// Concrete test classes
class LoginTest extends TestCase {
    @Override
    public void executeTest() {
        System.out.println("Executing Login Test");
        System.out.println("- Enter username");
        System.out.println("- Enter password");
        System.out.println("- Click login button");
    }
}

class SearchTest extends TestCase {
    @Override
    public void executeTest() {
        System.out.println("Executing Search Test");
        System.out.println("- Enter search term");
        System.out.println("- Click search button");
        System.out.println("- Verify results");
    }

    @Override
    public void setUp() {
        super.setUp(); // Call parent setUp
        System.out.println("Additional search setup");
    }
}

class CheckoutTest extends TestCase {
    @Override
    public void executeTest() {
        System.out.println("Executing Checkout Test");
        System.out.println("- Add item to cart");
        System.out.println("- Proceed to checkout");
        System.out.println("- Complete payment");
    }
}

public class TestRunner {
    public static void main(String[] args) {
        TestCase[] tests = {
            new LoginTest(),
            new SearchTest(),
            new CheckoutTest()
        };

        for (TestCase test : tests) {
            test.run();
            System.out.println("---");
        }
    }
}
```

**Overloading vs Overriding:**

| Feature | Overloading | Overriding |
|---------|-------------|------------|
| Definition | Multiple methods with same name | Child redefines parent method |
| Parameters | Must differ | Must be same |
| Return type | Can differ | Same or covariant |
| Access modifier | Any | Same or more permissive |
| Binding | Compile-time (static) | Runtime (dynamic) |
| Inheritance | Not required | Required |
| Performance | Faster | Slightly slower |

### Common Mistakes to Avoid

1. **Forgetting @Override annotation:**
```java
class Parent {
    public void display() { }
}

class Child extends Parent {
    // Typo - creates new method instead of overriding
    public void Display() { } // Should be: display()

    // Better - use @Override, will catch typos
    @Override
    public void display() { } // Compiler will verify
}
```

2. **Trying to override static methods:**
```java
class Parent {
    public static void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {
    public static void show() { // Not overriding, but hiding
        System.out.println("Child");
    }
}

// Output depends on reference type, not object type
Parent obj = new Child();
obj.show(); // Prints "Parent" (not polymorphic!)
```

3. **Overriding with more restrictive access:**
```java
class Parent {
    public void method() { }
}

class Child extends Parent {
    // ERROR! Cannot reduce visibility
    protected void method() { }
}
```

4. **Confusing overloading with overriding:**
```java
class Parent {
    public void test(int a) { }
}

class Child extends Parent {
    // This is OVERLOADING, not OVERRIDING
    public void test(double a) { }
}
```

### Interview Tips

**Common Questions:**

1. **Q: Can we override private methods?**
   - No, private methods are not inherited

2. **Q: Can we override static methods?**
   - No, static methods are hidden, not overridden (method hiding)

3. **Q: Can we override final methods?**
   - No, final methods cannot be overridden

4. **Q: Difference between overloading and overriding?**
   - See comparison table above

5. **Q: What is covariant return type?**
```java
class Animal {
    public Animal getAnimal() {
        return new Animal();
    }
}

class Dog extends Animal {
    @Override
    public Dog getAnimal() { // Covariant return type (Dog is subtype of Animal)
        return new Dog();
    }
}
```

6. **Q: Can constructors be overridden?**
   - No, constructors cannot be inherited or overridden

---

## 8. Abstraction and Interfaces

### A. Abstraction

**Abstraction** = Hiding implementation details, showing only essential features

**Ways to achieve abstraction:**
1. Abstract classes (0-100% abstraction)
2. Interfaces (100% abstraction until Java 8)

#### Abstract Class

```java
// Abstract class - cannot be instantiated
abstract class WebPage {
    String url;
    String title;

    // Concrete method
    public void loadPage() {
        System.out.println("Loading page: " + url);
    }

    // Abstract method - no implementation
    public abstract void fillForm();
    public abstract void submitForm();

    // Constructor
    public WebPage(String url) {
        this.url = url;
    }
}

// Concrete class - must implement all abstract methods
class LoginPage extends WebPage {
    public LoginPage(String url) {
        super(url);
    }

    @Override
    public void fillForm() {
        System.out.println("Entering username and password");
    }

    @Override
    public void submitForm() {
        System.out.println("Clicking login button");
    }
}

class RegistrationPage extends WebPage {
    public RegistrationPage(String url) {
        super(url);
    }

    @Override
    public void fillForm() {
        System.out.println("Entering registration details");
    }

    @Override
    public void submitForm() {
        System.out.println("Clicking register button");
    }
}

public class AbstractionTest {
    public static void main(String[] args) {
        // WebPage page = new WebPage("url"); // ERROR! Cannot instantiate abstract class

        WebPage loginPage = new LoginPage("https://example.com/login");
        loginPage.loadPage(); // Concrete method
        loginPage.fillForm(); // Overridden method
        loginPage.submitForm();

        System.out.println();

        WebPage regPage = new RegistrationPage("https://example.com/register");
        regPage.loadPage();
        regPage.fillForm();
        regPage.submitForm();
    }
}
```

**Testing Framework Example:**

```java
abstract class BaseTest {
    String browser;
    String environment;

    public BaseTest(String browser, String environment) {
        this.browser = browser;
        this.environment = environment;
    }

    // Concrete methods
    public void setUp() {
        System.out.println("Setting up test");
        System.out.println("Browser: " + browser);
        System.out.println("Environment: " + environment);
    }

    public void tearDown() {
        System.out.println("Tearing down test");
        System.out.println("Closing browser");
    }

    // Abstract methods - must be implemented by child classes
    public abstract void runTest();
    public abstract void verifyResults();

    // Template method
    public final void execute() {
        setUp();
        runTest();
        verifyResults();
        tearDown();
    }
}

class LoginTest extends BaseTest {
    public LoginTest(String browser, String environment) {
        super(browser, environment);
    }

    @Override
    public void runTest() {
        System.out.println("Executing login test");
        System.out.println("- Navigate to login page");
        System.out.println("- Enter credentials");
        System.out.println("- Click login");
    }

    @Override
    public void verifyResults() {
        System.out.println("Verifying login success");
    }
}

public class TestRunner {
    public static void main(String[] args) {
        BaseTest test = new LoginTest("Chrome", "QA");
        test.execute();
    }
}
```

### B. Interfaces

**Interface** = Contract/Blueprint that a class must follow

**Java 8+:**
- Abstract methods (implicitly public abstract)
- Default methods (with body)
- Static methods
- Constants (implicitly public static final)

**Java 9+:**
- Private methods

```java
// Basic interface
interface WebDriver {
    // Constants (public static final by default)
    String VERSION = "4.16.0";
    int TIMEOUT = 30;

    // Abstract methods (public abstract by default)
    void get(String url);
    void findElement(String locator);
    void click(String locator);
    void quit();

    // Default method (Java 8+)
    default void maximize() {
        System.out.println("Maximizing browser window");
    }

    // Static method (Java 8+)
    static void printVersion() {
        System.out.println("WebDriver version: " + VERSION);
    }
}

// Implementing interface
class ChromeDriver implements WebDriver {
    @Override
    public void get(String url) {
        System.out.println("Chrome: Navigating to " + url);
    }

    @Override
    public void findElement(String locator) {
        System.out.println("Chrome: Finding element " + locator);
    }

    @Override
    public void click(String locator) {
        System.out.println("Chrome: Clicking element " + locator);
    }

    @Override
    public void quit() {
        System.out.println("Chrome: Closing browser");
    }

    // Can override default method
    @Override
    public void maximize() {
        System.out.println("Chrome: Maximizing window");
    }
}

class FirefoxDriver implements WebDriver {
    @Override
    public void get(String url) {
        System.out.println("Firefox: Navigating to " + url);
    }

    @Override
    public void findElement(String locator) {
        System.out.println("Firefox: Finding element " + locator);
    }

    @Override
    public void click(String locator) {
        System.out.println("Firefox: Clicking element " + locator);
    }

    @Override
    public void quit() {
        System.out.println("Firefox: Closing browser");
    }

    // Uses default maximize() from interface
}

public class InterfaceDemo {
    public static void main(String[] args) {
        WebDriver driver1 = new ChromeDriver();
        driver1.get("https://google.com");
        driver1.findElement("//input[@name='q']");
        driver1.maximize();
        driver1.quit();

        System.out.println();

        WebDriver driver2 = new FirefoxDriver();
        driver2.get("https://amazon.com");
        driver2.maximize(); // Uses default method
        driver2.quit();

        System.out.println();

        // Static method call
        WebDriver.printVersion();
    }
}
```

**Multiple Inheritance through Interfaces:**

```java
interface TestCase {
    void executeTest();
}

interface Reportable {
    void generateReport();
}

interface Retryable {
    void retry();
    default int getMaxRetries() {
        return 3;
    }
}

// Class implementing multiple interfaces
class SeleniumTest implements TestCase, Reportable, Retryable {
    @Override
    public void executeTest() {
        System.out.println("Executing Selenium test");
    }

    @Override
    public void generateReport() {
        System.out.println("Generating test report");
    }

    @Override
    public void retry() {
        System.out.println("Retrying failed test");
    }

    public void runTest() {
        executeTest();
        generateReport();
        System.out.println("Max retries: " + getMaxRetries());
    }
}

public class MultipleInheritanceDemo {
    public static void main(String[] args) {
        SeleniumTest test = new SeleniumTest();
        test.runTest();
        test.retry();
    }
}
```

**Functional Interface (Java 8+):**

```java
// Interface with exactly one abstract method
@FunctionalInterface
interface TestValidator {
    boolean validate(String actual, String expected);

    // Can have default methods
    default void logResult(boolean result) {
        System.out.println("Test " + (result ? "Passed" : "Failed"));
    }

    // Can have static methods
    static void printInfo() {
        System.out.println("Test Validator Interface");
    }
}

public class FunctionalInterfaceDemo {
    public static void main(String[] args) {
        // Lambda expression
        TestValidator validator = (actual, expected) -> actual.equals(expected);

        boolean result = validator.validate("Login Page", "Login Page");
        validator.logResult(result);

        result = validator.validate("Home Page", "Login Page");
        validator.logResult(result);

        TestValidator.printInfo();
    }
}
```

**Interface vs Abstract Class:**

| Feature | Interface | Abstract Class |
|---------|-----------|----------------|
| Methods | Abstract + Default + Static | Abstract + Concrete |
| Variables | public static final only | Any type |
| Constructor | No | Yes |
| Inheritance | Multiple | Single |
| Access Modifiers | public only | Any |
| When to use | "Can do" capability | "Is a" relationship |
| Example | Runnable, Comparable | BaseTest, BasePage |

### Real-World Example: Page Object Model

```java
// Interface defining page actions
interface Page {
    void navigateTo(String url);
    boolean isPageLoaded();
    String getPageTitle();
}

interface FormPage extends Page {
    void fillField(String fieldName, String value);
    void submitForm();
    void clearForm();
}

interface Searchable {
    void search(String keyword);
    int getResultCount();
}

// Abstract base class
abstract class BasePage implements Page {
    protected WebDriver driver;
    protected String url;

    public BasePage(WebDriver driver, String url) {
        this.driver = driver;
        this.url = url;
    }

    @Override
    public void navigateTo(String url) {
        System.out.println("Navigating to: " + url);
    }

    @Override
    public boolean isPageLoaded() {
        System.out.println("Checking if page is loaded");
        return true;
    }

    @Override
    public String getPageTitle() {
        return "Page Title";
    }
}

// Concrete page class
class LoginPage extends BasePage implements FormPage {
    public LoginPage(WebDriver driver) {
        super(driver, "https://example.com/login");
    }

    @Override
    public void fillField(String fieldName, String value) {
        System.out.println("Filling " + fieldName + " with " + value);
    }

    @Override
    public void submitForm() {
        System.out.println("Submitting login form");
    }

    @Override
    public void clearForm() {
        System.out.println("Clearing login form");
    }

    public void login(String username, String password) {
        navigateTo(url);
        fillField("username", username);
        fillField("password", password);
        submitForm();
    }
}

class HomePage extends BasePage implements Searchable {
    public HomePage(WebDriver driver) {
        super(driver, "https://example.com/home");
    }

    @Override
    public void search(String keyword) {
        System.out.println("Searching for: " + keyword);
    }

    @Override
    public int getResultCount() {
        System.out.println("Getting result count");
        return 10;
    }
}
```

### Common Mistakes to Avoid

1. **Trying to instantiate interface:**
```java
// WebDriver driver = new WebDriver(); // ERROR!
WebDriver driver = new ChromeDriver(); // Correct
```

2. **Not implementing all abstract methods:**
```java
class MyDriver implements WebDriver {
    // ERROR! Must implement all abstract methods
    @Override
    public void get(String url) { }
    // Missing other methods!
}
```

3. **Using variables in interface incorrectly:**
```java
interface Test {
    int count = 10; // public static final (constant)
}

// count = 20; // ERROR! Cannot modify
```

4. **Confusing when to use abstract class vs interface:**
```java
// Use abstract class when:
// - You have common implementation
// - You need constructors
// - You want to maintain state

// Use interface when:
// - Defining contract/capability
// - Need multiple inheritance
// - Working with lambda expressions
```

### Interview Tips

**Common Questions:**

1. **Q: Can interface have constructors?**
   - No, interfaces cannot have constructors

2. **Q: Can we create object of interface?**
   - No, but we can create reference variable

3. **Q: Difference between abstract class and interface?**
   - See comparison table above

4. **Q: Can interface extend another interface?**
```java
interface A {
    void methodA();
}

interface B extends A {
    void methodB();
}

class C implements B {
    public void methodA() { } // Must implement both
    public void methodB() { }
}
```

5. **Q: What is marker interface?**
   - Interface with no methods
   - Example: Serializable, Cloneable
   - Used to mark classes for special treatment

6. **Q: Can interface have private methods?**
   - Yes, from Java 9+ (to support default methods)

7. **Q: Diamond problem in Java?**
```java
interface A {
    default void show() {
        System.out.println("A");
    }
}

interface B {
    default void show() {
        System.out.println("B");
    }
}

class C implements A, B {
    // Must override to resolve conflict
    @Override
    public void show() {
        A.super.show(); // Call A's method
        // or B.super.show(); // Call B's method
    }
}
```

---

## 9. Encapsulation and SOLID Principles

### A. Encapsulation

**Encapsulation** = Wrapping data (variables) and code (methods) together as a single unit and restricting access to them.

**Benefits:**
- Data hiding and security
- Increased flexibility
- Reusability
- Easy to test and maintain

**Implementation:**
1. Make variables private
2. Provide public getter/setter methods

```java
// Without encapsulation - Bad practice
class UserBad {
    public String username;
    public String password;
    public int age;
}

// With encapsulation - Good practice
class User {
    private String username;
    private String password;
    private int age;

    // Getter methods
    public String getUsername() {
        return username;
    }

    public String getPassword() {
        return "****"; // Don't expose actual password
    }

    public int getAge() {
        return age;
    }

    // Setter methods with validation
    public void setUsername(String username) {
        if (username != null && !username.isEmpty()) {
            this.username = username;
        } else {
            System.out.println("Invalid username");
        }
    }

    public void setPassword(String password) {
        if (password != null && password.length() >= 8) {
            this.password = password;
        } else {
            System.out.println("Password must be at least 8 characters");
        }
    }

    public void setAge(int age) {
        if (age > 0 && age < 150) {
            this.age = age;
        } else {
            System.out.println("Invalid age");
        }
    }
}

public class EncapsulationDemo {
    public static void main(String[] args) {
        User user = new User();

        user.setUsername("testuser");
        user.setPassword("pass123456");
        user.setAge(25);

        System.out.println("Username: " + user.getUsername());
        System.out.println("Password: " + user.getPassword()); // Returns ****
        System.out.println("Age: " + user.getAge());

        // Validation in action
        user.setPassword("123"); // Too short
        user.setAge(200); // Invalid age
    }
}
```

**Testing Example: Page Object with Encapsulation**

```java
class LoginPage {
    // Private WebElements (encapsulated)
    private WebElement usernameField;
    private WebElement passwordField;
    private WebElement loginButton;
    private WebDriver driver;

    // Constructor
    public LoginPage(WebDriver driver) {
        this.driver = driver;
        initElements();
    }

    // Private helper method
    private void initElements() {
        // usernameField = driver.findElement(By.id("username"));
        // passwordField = driver.findElement(By.id("password"));
        // loginButton = driver.findElement(By.id("loginBtn"));
    }

    // Public methods to interact with page
    public void enterUsername(String username) {
        if (username != null && !username.isEmpty()) {
            System.out.println("Entering username: " + username);
            // usernameField.sendKeys(username);
        }
    }

    public void enterPassword(String password) {
        if (password != null && !password.isEmpty()) {
            System.out.println("Entering password: ****");
            // passwordField.sendKeys(password);
        }
    }

    public void clickLogin() {
        System.out.println("Clicking login button");
        // loginButton.click();
    }

    // High-level method combining operations
    public void login(String username, String password) {
        enterUsername(username);
        enterPassword(password);
        clickLogin();
    }

    // Read-only property
    public String getPageTitle() {
        // return driver.getTitle();
        return "Login Page";
    }
}

public class EncapsulatedTest {
    public static void main(String[] args) {
        WebDriver driver = null; // Placeholder
        LoginPage loginPage = new LoginPage(driver);

        // Cannot directly access private elements
        // loginPage.usernameField.sendKeys("test"); // ERROR!

        // Use public methods
        loginPage.login("admin", "admin123");
        System.out.println("Page: " + loginPage.getPageTitle());
    }
}
```

### B. SOLID Principles

SOLID is an acronym for five design principles that make software more maintainable and scalable.

#### 1. Single Responsibility Principle (SRP)

**Definition:** A class should have only one reason to change (one responsibility)

**Bad Example:**
```java
// Violates SRP - multiple responsibilities
class TestExecutor {
    public void executeTest() {
        // Test execution
        System.out.println("Executing test");
    }

    public void generateReport() {
        // Reporting
        System.out.println("Generating report");
    }

    public void sendEmail() {
        // Email notification
        System.out.println("Sending email");
    }

    public void logResults() {
        // Logging
        System.out.println("Logging results");
    }
}
```

**Good Example:**
```java
// Each class has single responsibility
class TestExecutor {
    public void executeTest() {
        System.out.println("Executing test");
    }
}

class ReportGenerator {
    public void generateReport() {
        System.out.println("Generating report");
    }
}

class EmailNotifier {
    public void sendEmail(String message) {
        System.out.println("Sending email: " + message);
    }
}

class Logger {
    public void log(String message) {
        System.out.println("Log: " + message);
    }
}

// Orchestrator class
class TestManager {
    private TestExecutor executor;
    private ReportGenerator reporter;
    private EmailNotifier notifier;
    private Logger logger;

    public TestManager() {
        executor = new TestExecutor();
        reporter = new ReportGenerator();
        notifier = new EmailNotifier();
        logger = new Logger();
    }

    public void runTest() {
        logger.log("Starting test");
        executor.executeTest();
        reporter.generateReport();
        notifier.sendEmail("Test completed");
        logger.log("Test finished");
    }
}
```

#### 2. Open/Closed Principle (OCP)

**Definition:** Classes should be open for extension but closed for modification

**Bad Example:**
```java
class TestReporter {
    public void generateReport(String type) {
        if (type.equals("HTML")) {
            System.out.println("Generating HTML report");
        } else if (type.equals("PDF")) {
            System.out.println("Generating PDF report");
        } else if (type.equals("XML")) {
            System.out.println("Generating XML report");
        }
        // Need to modify this class for each new report type!
    }
}
```

**Good Example:**
```java
// Interface for report generation
interface ReportGenerator {
    void generate();
}

// Concrete implementations
class HTMLReportGenerator implements ReportGenerator {
    @Override
    public void generate() {
        System.out.println("Generating HTML report");
    }
}

class PDFReportGenerator implements ReportGenerator {
    @Override
    public void generate() {
        System.out.println("Generating PDF report");
    }
}

class XMLReportGenerator implements ReportGenerator {
    @Override
    public void generate() {
        System.out.println("Generating XML report");
    }
}

// Can add new report types without modifying existing code
class ExcelReportGenerator implements ReportGenerator {
    @Override
    public void generate() {
        System.out.println("Generating Excel report");
    }
}

class TestReporter {
    public void generateReport(ReportGenerator generator) {
        generator.generate(); // No modification needed for new types
    }
}

public class OCPDemo {
    public static void main(String[] args) {
        TestReporter reporter = new TestReporter();

        reporter.generateReport(new HTMLReportGenerator());
        reporter.generateReport(new PDFReportGenerator());
        reporter.generateReport(new ExcelReportGenerator());
    }
}
```

#### 3. Liskov Substitution Principle (LSP)

**Definition:** Objects of a superclass should be replaceable with objects of its subclasses without breaking the application

**Bad Example:**
```java
class Rectangle {
    protected int width;
    protected int height;

    public void setWidth(int width) {
        this.width = width;
    }

    public void setHeight(int height) {
        this.height = height;
    }

    public int getArea() {
        return width * height;
    }
}

class Square extends Rectangle {
    @Override
    public void setWidth(int width) {
        this.width = width;
        this.height = width; // Violates LSP!
    }

    @Override
    public void setHeight(int height) {
        this.width = height;
        this.height = height; // Violates LSP!
    }
}

// This breaks when using Square
public class LSPBad {
    public static void testRectangle(Rectangle rect) {
        rect.setWidth(5);
        rect.setHeight(4);
        System.out.println("Expected area: 20");
        System.out.println("Actual area: " + rect.getArea());
        // For Square, area would be 16, not 20!
    }

    public static void main(String[] args) {
        Rectangle rect = new Rectangle();
        testRectangle(rect); // Works correctly

        Rectangle square = new Square();
        testRectangle(square); // Breaks expectation!
    }
}
```

**Good Example:**
```java
interface Shape {
    int getArea();
}

class Rectangle implements Shape {
    private int width;
    private int height;

    public Rectangle(int width, int height) {
        this.width = width;
        this.height = height;
    }

    @Override
    public int getArea() {
        return width * height;
    }
}

class Square implements Shape {
    private int side;

    public Square(int side) {
        this.side = side;
    }

    @Override
    public int getArea() {
        return side * side;
    }
}

public class LSPGood {
    public static void printArea(Shape shape) {
        System.out.println("Area: " + shape.getArea());
    }

    public static void main(String[] args) {
        Shape rect = new Rectangle(5, 4);
        Shape square = new Square(5);

        printArea(rect); // Area: 20
        printArea(square); // Area: 25
        // Both work correctly!
    }
}
```

**Testing Example:**
```java
interface WebDriver {
    void get(String url);
    void findElement(String locator);
    void quit();
}

class ChromeDriver implements WebDriver {
    @Override
    public void get(String url) {
        System.out.println("Chrome: Navigating to " + url);
    }

    @Override
    public void findElement(String locator) {
        System.out.println("Chrome: Finding " + locator);
    }

    @Override
    public void quit() {
        System.out.println("Chrome: Quitting");
    }
}

class FirefoxDriver implements WebDriver {
    @Override
    public void get(String url) {
        System.out.println("Firefox: Navigating to " + url);
    }

    @Override
    public void findElement(String locator) {
        System.out.println("Firefox: Finding " + locator);
    }

    @Override
    public void quit() {
        System.out.println("Firefox: Quitting");
    }
}

// Test works with any WebDriver implementation
class LoginTest {
    public void testLogin(WebDriver driver) {
        driver.get("https://example.com");
        driver.findElement("username");
        driver.findElement("password");
        driver.quit();
    }
}

public class LSPTest {
    public static void main(String[] args) {
        LoginTest test = new LoginTest();

        // Works with Chrome
        test.testLogin(new ChromeDriver());

        // Works with Firefox (LSP satisfied)
        test.testLogin(new FirefoxDriver());
    }
}
```

#### 4. Interface Segregation Principle (ISP)

**Definition:** Clients should not be forced to depend on interfaces they don't use

**Bad Example:**
```java
// Fat interface - violates ISP
interface Worker {
    void work();
    void eat();
    void sleep();
    void attendMeeting();
    void writeCode();
}

// Robot doesn't need eat() and sleep()
class Robot implements Worker {
    @Override
    public void work() {
        System.out.println("Robot working");
    }

    @Override
    public void eat() {
        // Forced to implement, but doesn't make sense!
        throw new UnsupportedOperationException();
    }

    @Override
    public void sleep() {
        // Forced to implement!
        throw new UnsupportedOperationException();
    }

    @Override
    public void attendMeeting() {
        throw new UnsupportedOperationException();
    }

    @Override
    public void writeCode() {
        System.out.println("Robot writing code");
    }
}
```

**Good Example:**
```java
// Segregated interfaces
interface Workable {
    void work();
}

interface Eatable {
    void eat();
}

interface Sleepable {
    void sleep();
}

interface Attendable {
    void attendMeeting();
}

// Human implements all
class Human implements Workable, Eatable, Sleepable, Attendable {
    @Override
    public void work() {
        System.out.println("Human working");
    }

    @Override
    public void eat() {
        System.out.println("Human eating");
    }

    @Override
    public void sleep() {
        System.out.println("Human sleeping");
    }

    @Override
    public void attendMeeting() {
        System.out.println("Human attending meeting");
    }
}

// Robot only implements what it needs
class RobotWorker implements Workable {
    @Override
    public void work() {
        System.out.println("Robot working");
    }
}
```

**Testing Example:**
```java
// Bad - Fat interface
interface TestActions {
    void openBrowser();
    void closeBrowser();
    void takeScreenshot();
    void generateReport();
    void sendEmail();
    void uploadToCloud();
}

// Good - Segregated interfaces
interface BrowserActions {
    void openBrowser();
    void closeBrowser();
}

interface ScreenshotCapture {
    void takeScreenshot();
}

interface ReportGeneration {
    void generateReport();
}

interface Notification {
    void sendEmail();
}

interface CloudIntegration {
    void uploadToCloud();
}

// Simple test only needs browser actions
class SimpleTest implements BrowserActions {
    @Override
    public void openBrowser() {
        System.out.println("Opening browser");
    }

    @Override
    public void closeBrowser() {
        System.out.println("Closing browser");
    }
}

// Advanced test needs multiple capabilities
class AdvancedTest implements BrowserActions, ScreenshotCapture, ReportGeneration {
    @Override
    public void openBrowser() {
        System.out.println("Opening browser");
    }

    @Override
    public void closeBrowser() {
        System.out.println("Closing browser");
    }

    @Override
    public void takeScreenshot() {
        System.out.println("Taking screenshot");
    }

    @Override
    public void generateReport() {
        System.out.println("Generating report");
    }
}
```

#### 5. Dependency Inversion Principle (DIP)

**Definition:** High-level modules should not depend on low-level modules. Both should depend on abstractions.

**Bad Example:**
```java
// Low-level modules
class MySQLDatabase {
    public void save(String data) {
        System.out.println("Saving to MySQL: " + data);
    }
}

// High-level module depends on low-level module
class TestResultManager {
    private MySQLDatabase database; // Tight coupling!

    public TestResultManager() {
        this.database = new MySQLDatabase();
    }

    public void saveResult(String result) {
        database.save(result);
    }
}
// Problem: Cannot easily switch to MongoDB or Oracle
```

**Good Example:**
```java
// Abstraction
interface Database {
    void save(String data);
}

// Low-level modules implement abstraction
class MySQLDatabase implements Database {
    @Override
    public void save(String data) {
        System.out.println("Saving to MySQL: " + data);
    }
}

class MongoDatabase implements Database {
    @Override
    public void save(String data) {
        System.out.println("Saving to MongoDB: " + data);
    }
}

class OracleDatabase implements Database {
    @Override
    public void save(String data) {
        System.out.println("Saving to Oracle: " + data);
    }
}

// High-level module depends on abstraction
class TestResultManager {
    private Database database; // Depends on abstraction!

    // Dependency Injection through constructor
    public TestResultManager(Database database) {
        this.database = database;
    }

    public void saveResult(String result) {
        database.save(result);
    }
}

public class DIPDemo {
    public static void main(String[] args) {
        // Easy to switch databases
        TestResultManager manager1 = new TestResultManager(new MySQLDatabase());
        manager1.saveResult("Test passed");

        TestResultManager manager2 = new TestResultManager(new MongoDatabase());
        manager2.saveResult("Test failed");

        TestResultManager manager3 = new TestResultManager(new OracleDatabase());
        manager3.saveResult("Test skipped");
    }
}
```

**Testing Example with Dependency Injection:**

```java
// Abstraction for browser
interface WebDriver {
    void navigate(String url);
    void click(String element);
    void quit();
}

// Concrete implementations
class ChromeDriver implements WebDriver {
    @Override
    public void navigate(String url) {
        System.out.println("Chrome: Navigating to " + url);
    }

    @Override
    public void click(String element) {
        System.out.println("Chrome: Clicking " + element);
    }

    @Override
    public void quit() {
        System.out.println("Chrome: Quitting");
    }
}

class FirefoxDriver implements WebDriver {
    @Override
    public void navigate(String url) {
        System.out.println("Firefox: Navigating to " + url);
    }

    @Override
    public void click(String element) {
        System.out.println("Firefox: Clicking " + element);
    }

    @Override
    public void quit() {
        System.out.println("Firefox: Quitting");
    }
}

// Test class depends on abstraction
class LoginTest {
    private WebDriver driver;

    // Constructor injection
    public LoginTest(WebDriver driver) {
        this.driver = driver;
    }

    // Setter injection
    public void setDriver(WebDriver driver) {
        this.driver = driver;
    }

    public void executeTest() {
        driver.navigate("https://example.com/login");
        driver.click("username");
        driver.click("password");
        driver.click("loginButton");
        driver.quit();
    }
}

public class DIPTestDemo {
    public static void main(String[] args) {
        // Easy to switch browsers
        LoginTest test1 = new LoginTest(new ChromeDriver());
        test1.executeTest();

        System.out.println();

        LoginTest test2 = new LoginTest(new FirefoxDriver());
        test2.executeTest();
    }
}
```

### SOLID Principles Summary

| Principle | Meaning | Key Benefit |
|-----------|---------|-------------|
| **S**RP | One class, one responsibility | Easy to understand and maintain |
| **O**CP | Open for extension, closed for modification | Add features without breaking existing code |
| **L**SP | Subclasses should be substitutable | Prevents unexpected behavior |
| **I**SP | Many specific interfaces better than one general | Clients use only what they need |
| **D**IP | Depend on abstractions, not concretions | Loose coupling, easy to test |

### Common Mistakes to Avoid

1. **Not using getters/setters:**
```java
// Bad
public class User {
    public String name; // Direct access
}

// Good
public class User {
    private String name;
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
}
```

2. **Violating SRP:**
```java
// Bad - one class doing too much
class PageObject {
    void clickElement() { }
    void generateReport() { } // Different responsibility!
    void sendEmail() { } // Different responsibility!
}
```

3. **Tight coupling:**
```java
// Bad
class TestClass {
    ChromeDriver driver = new ChromeDriver(); // Tight coupling
}

// Good
class TestClass {
    WebDriver driver; // Loose coupling
    public TestClass(WebDriver driver) {
        this.driver = driver;
    }
}
```

### Interview Tips

**Q: What is encapsulation?**
- Bundling data and methods, restricting direct access

**Q: Benefits of SOLID principles?**
- Maintainability, scalability, testability, reusability

**Q: Real-world example of SRP in testing?**
- Separate classes for: Page Objects, Test Data, Test Execution, Reporting

**Q: How does DIP help in testing?**
- Easy to mock dependencies, switch implementations (browsers, databases)

**Q: Example of OCP in automation framework?**
- Adding new report formats without modifying existing reporter code

---

## 10. Packages and Imports

### A. Packages

**Package** = Grouping related classes and interfaces together

**Benefits:**
- Organization and categorization
- Name conflict prevention
- Access control
- Code reusability

**Naming Convention:**
```
com.companyname.projectname.modulename
```

**Package Structure Example:**
```
com.testing.automation/
├── pages/
│   ├── LoginPage.java
│   ├── HomePage.java
│   └── BasePage.java
├── tests/
│   ├── LoginTest.java
│   ├── SearchTest.java
│   └── BaseTest.java
├── utils/
│   ├── Driver Manager.java
│   ├── ConfigReader.java
│   └── TestDataProvider.java
├── config/
│   ├── TestConfig.java
│   └── EnvironmentConfig.java
└── reports/
    ├── HTMLReporter.java
    └── ExtentReporter.java
```

**Creating a Package:**

```java
// File: com/testing/pages/LoginPage.java
package com.testing.pages;

public class LoginPage {
    public void login(String username, String password) {
        System.out.println("Logging in: " + username);
    }
}

// File: com/testing/pages/HomePage.java
package com.testing.pages;

public class HomePage {
    public void search(String keyword) {
        System.out.println("Searching: " + keyword);
    }
}

// File: com/testing/tests/LoginTest.java
package com.testing.tests;

import com.testing.pages.LoginPage; // Import from another package

public class LoginTest {
    public static void main(String[] args) {
        LoginPage page = new LoginPage();
        page.login("admin", "admin123");
    }
}
```

**Built-in Java Packages:**

| Package | Description | Example Classes |
|---------|-------------|-----------------|
| java.lang | Fundamental classes (auto-imported) | String, System, Integer |
| java.util | Utility classes | ArrayList, HashMap, Scanner |
| java.io | Input/Output operations | File, FileReader, BufferedReader |
| java.sql | Database connectivity | Connection, Statement, ResultSet |
| java.net | Networking | URL, Socket, HttpURLConnection |

### B. Import Statements

**Types of Imports:**

```java
package com.testing.tests;

// 1. Explicit import (recommended)
import java.util.ArrayList;
import java.util.HashMap;
import com.testing.pages.LoginPage;

// 2. Wildcard import (imports all classes from package)
import java.util.*; // Not recommended for large packages
import com.testing.pages.*;

// 3. Static import (for static members)
import static java.lang.Math.PI;
import static java.lang.Math.sqrt;

// 4. Static wildcard import
import static java.lang.System.*; // Import all static members

public class ImportDemo {
    public static void main(String[] args) {
        // Using imported classes
        ArrayList<String> list = new ArrayList<>();
        HashMap<String, Integer> map = new HashMap<>();

        // Using static imports
        out.println("PI value: " + PI); // No need for System.out and Math.PI
        out.println("Square root: " + sqrt(16));

        // Without import, use fully qualified name
        java.util.Date date = new java.util.Date();
        System.out.println("Date: " + date);
    }
}
```

**Import Conflicts:**

```java
package com.testing;

// Both packages have a Date class
import java.util.Date; // Utility date
// import java.sql.Date; // SQL date - Conflict!

public class ConflictDemo {
    public static void main(String[] args) {
        // Solution 1: Import one, use fully qualified name for other
        Date utilDate = new Date(); // java.util.Date
        java.sql.Date sqlDate = new java.sql.Date(System.currentTimeMillis());

        // Solution 2: Use fully qualified names for both
        java.util.Date d1 = new java.util.Date();
        java.sql.Date d2 = new java.sql.Date(System.currentTimeMillis());
    }
}
```

### C. Testing Framework Package Structure

**Example: Page Object Model Structure**

```java
// File: src/main/java/com/automation/base/BasePage.java
package com.automation.base;

public class BasePage {
    protected void waitForElement(String locator) {
        System.out.println("Waiting for: " + locator);
    }

    protected void click(String locator) {
        System.out.println("Clicking: " + locator);
    }
}

// File: src/main/java/com/automation/pages/LoginPage.java
package com.automation.pages;

import com.automation.base.BasePage;

public class LoginPage extends BasePage {
    private String usernameField = "//input[@id='username']";
    private String passwordField = "//input[@id='password']";
    private String loginButton = "//button[@id='login']";

    public void enterUsername(String username) {
        click(usernameField);
        System.out.println("Entering username: " + username);
    }

    public void enterPassword(String password) {
        click(passwordField);
        System.out.println("Entering password");
    }

    public void clickLoginButton() {
        click(loginButton);
    }

    public void login(String username, String password) {
        enterUsername(username);
        enterPassword(password);
        clickLoginButton();
    }
}

// File: src/main/java/com/automation/utils/ConfigReader.java
package com.automation.utils;

public class ConfigReader {
    public static String getProperty(String key) {
        // Read from config file
        if (key.equals("browser")) {
            return "Chrome";
        } else if (key.equals("url")) {
            return "https://example.com";
        }
        return null;
    }
}

// File: src/main/java/com/automation/utils/DriverManager.java
package com.automation.utils;

public class DriverManager {
    private static String driver;

    public static void initializeDriver(String browser) {
        System.out.println("Initializing " + browser + " driver");
        driver = browser + "Driver";
    }

    public static String getDriver() {
        return driver;
    }

    public static void quitDriver() {
        System.out.println("Quitting driver");
        driver = null;
    }
}

// File: src/test/java/com/automation/tests/BaseTest.java
package com.automation.tests;

import com.automation.utils.ConfigReader;
import com.automation.utils.DriverManager;

public class BaseTest {
    protected void setUp() {
        String browser = ConfigReader.getProperty("browser");
        DriverManager.initializeDriver(browser);
    }

    protected void tearDown() {
        DriverManager.quitDriver();
    }
}

// File: src/test/java/com/automation/tests/LoginTest.java
package com.automation.tests;

import com.automation.pages.LoginPage;
import com.automation.utils.ConfigReader;

public class LoginTest extends BaseTest {
    public void testValidLogin() {
        setUp();

        String url = ConfigReader.getProperty("url");
        System.out.println("Navigating to: " + url);

        LoginPage loginPage = new LoginPage();
        loginPage.login("admin", "admin123");

        System.out.println("Verifying login success");

        tearDown();
    }

    public static void main(String[] args) {
        LoginTest test = new LoginTest();
        test.testValidLogin();
    }
}
```

**Output:**
```
Initializing Chrome driver
Navigating to: https://example.com
Clicking: //input[@id='username']
Entering username: admin
Clicking: //input[@id='password']
Entering password
Clicking: //button[@id='login']
Verifying login success
Quitting driver
```

### D. Access Control with Packages

```java
// File: com/testing/utils/Helper.java
package com.testing.utils;

public class Helper {
    // public - accessible everywhere
    public void publicMethod() {
        System.out.println("Public method");
    }

    // protected - accessible in same package and subclasses
    protected void protectedMethod() {
        System.out.println("Protected method");
    }

    // default - accessible only in same package
    void defaultMethod() {
        System.out.println("Default method");
    }

    // private - accessible only within this class
    private void privateMethod() {
        System.out.println("Private method");
    }

    public void testAccess() {
        publicMethod(); // OK
        protectedMethod(); // OK
        defaultMethod(); // OK
        privateMethod(); // OK
    }
}

// File: com/testing/utils/HelperTest.java (same package)
package com.testing.utils;

public class HelperTest {
    public static void main(String[] args) {
        Helper helper = new Helper();
        helper.publicMethod(); // OK
        helper.protectedMethod(); // OK (same package)
        helper.defaultMethod(); // OK (same package)
        // helper.privateMethod(); // ERROR!
    }
}

// File: com/testing/tests/AccessTest.java (different package)
package com.testing.tests;

import com.testing.utils.Helper;

public class AccessTest {
    public static void main(String[] args) {
        Helper helper = new Helper();
        helper.publicMethod(); // OK
        // helper.protectedMethod(); // ERROR! (different package, no inheritance)
        // helper.defaultMethod(); // ERROR! (different package)
        // helper.privateMethod(); // ERROR!
    }
}

// File: com/testing/tests/SubHelper.java (different package, with inheritance)
package com.testing.tests;

import com.testing.utils.Helper;

public class SubHelper extends Helper {
    public void test() {
        publicMethod(); // OK
        protectedMethod(); // OK (through inheritance)
        // defaultMethod(); // ERROR! (different package)
        // privateMethod(); // ERROR!
    }
}
```

### Common Mistakes to Avoid

1. **Not following naming conventions:**
```java
// Wrong
package MyPackage; // Should be lowercase
package testing; // Too generic

// Correct
package com.companyname.testing;
package com.automation.pages;
```

2. **Circular dependencies:**
```java
// Package A
package com.testing.a;
import com.testing.b.ClassB;

// Package B
package com.testing.b;
import com.testing.a.ClassA; // Circular dependency - avoid!
```

3. **Using wildcard imports unnecessarily:**
```java
// Bad
import java.util.*; // Imports 100+ classes
import com.testing.pages.*; // Unclear which classes are used

// Good
import java.util.ArrayList;
import java.util.HashMap;
import com.testing.pages.LoginPage;
```

4. **Package and folder mismatch:**
```java
// File location: src/com/testing/tests/LoginTest.java
// Wrong package declaration
package com.testing.pages; // Doesn't match folder structure!

// Correct
package com.testing.tests;
```

### Interview Tips

**Q: What is a package?**
- Grouping of related classes and interfaces, provides namespace and access control

**Q: Difference between import and package?**
- package: declares which package a class belongs to
- import: uses classes from other packages

**Q: Can a class be in multiple packages?**
- No, a class belongs to exactly one package

**Q: What is the default package?**
- Classes without package declaration belong to the default unnamed package
- Not recommended for production code

**Q: Difference between java.util.Date and java.sql.Date?**
- java.util.Date: represents date and time
- java.sql.Date: represents SQL DATE (date only, no time)

**Q: Why use packages in automation framework?**
- Organization: pages, tests, utils, config separate
- Maintainability: easy to locate and modify
- Reusability: import and use across projects
- Access control: restrict visibility

---

*File continues with sections 11-17...*
## 11. Exception Handling

### A. What are Exceptions?

**Exception** = An unexpected event that disrupts the normal flow of program execution

**Exception Hierarchy:**
```
Throwable
├── Error (System-level, irrecoverable)
│   ├── OutOfMemoryError
│   ├── StackOverflowError
│   └── VirtualMachineError
└── Exception
    ├── IOException
    ├── SQLException
    ├── RuntimeException (Unchecked)
    │   ├── NullPointerException
    │   ├── ArrayIndexOutOfBoundsException
    │   ├── ArithmeticException
    │   └── NumberFormatException
    └── Checked Exceptions
        ├── FileNotFoundException
        ├── ClassNotFoundException
        └── InterruptedException
```

**Types of Exceptions:**

| Type | Check Time | Must Handle? | Examples |
|------|------------|--------------|----------|
| Checked | Compile-time | Yes | IOException, SQLException |
| Unchecked | Runtime | No (but should) | NullPointerException, ArithmeticException |
| Error | Runtime | No (can't recover) | OutOfMemoryError, StackOverflowError |

### B. try-catch-finally

**Basic Syntax:**

```java
public class ExceptionDemo {
    public static void main(String[] args) {
        // Example 1: ArithmeticException
        try {
            int result = 10 / 0; // Division by zero
            System.out.println("Result: " + result);
        } catch (ArithmeticException e) {
            System.out.println("Error: Cannot divide by zero");
            System.out.println("Exception: " + e.getMessage());
        }

        System.out.println("Program continues...");

        // Example 2: ArrayIndexOutOfBoundsException
        try {
            int[] numbers = {1, 2, 3};
            System.out.println(numbers[5]); // Index out of bounds
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("Error: Array index out of bounds");
        }

        // Example 3: NullPointerException
        try {
            String text = null;
            System.out.println(text.length()); // Null reference
        } catch (NullPointerException e) {
            System.out.println("Error: Null reference");
        }
    }
}
```

**Output:**
```
Error: Cannot divide by zero
Exception: / by zero
Program continues...
Error: Array index out of bounds
Error: Null reference
```

**Multiple catch Blocks:**

```java
public class MultipleCatchDemo {
    public static void main(String[] args) {
        try {
            String text = "abc";
            int num = Integer.parseInt(text); // NumberFormatException
            int result = 10 / 0; // ArithmeticException
            int[] arr = {1, 2};
            System.out.println(arr[5]); // ArrayIndexOutOfBoundsException
        } catch (NumberFormatException e) {
            System.out.println("Invalid number format: " + e.getMessage());
        } catch (ArithmeticException e) {
            System.out.println("Arithmetic error: " + e.getMessage());
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("Array index error: " + e.getMessage());
        } catch (Exception e) {
            // Generic catch - catches any remaining exceptions
            System.out.println("Unknown error: " + e.getMessage());
        }
    }
}
```

**Multi-catch (Java 7+):**

```java
public class MultiCatchDemo {
    public static void main(String[] args) {
        try {
            int num = Integer.parseInt("abc");
        } catch (NumberFormatException | ArithmeticException e) {
            // Single catch for multiple exception types
            System.out.println("Number error: " + e.getMessage());
        }
    }
}
```

**finally Block:**

```java
public class FinallyDemo {
    public static void main(String[] args) {
        try {
            System.out.println("In try block");
            int result = 10 / 0;
        } catch (ArithmeticException e) {
            System.out.println("In catch block");
        } finally {
            // Always executes, even if there's a return statement
            System.out.println("In finally block - cleanup code");
        }

        System.out.println("After finally");
    }
}
```

**Output:**
```
In try block
In catch block
In finally block - cleanup code
After finally
```

**Testing Example:**

```java
public class WebElementTest {
    public static void clickElement(String locator) {
        WebDriver driver = null;

        try {
            // Initialize driver
            System.out.println("Initializing driver");
            driver = new ChromeDriver();

            // Navigate
            driver.get("https://example.com");

            // Find and click element
            WebElement element = driver.findElement(By.id(locator));
            element.click();

            System.out.println("Element clicked successfully");

        } catch (NoSuchElementException e) {
            System.out.println("Element not found: " + locator);
            // Take screenshot
            System.out.println("Taking screenshot");
        } catch (WebDriverException e) {
            System.out.println("WebDriver error: " + e.getMessage());
        } finally {
            // Always close driver
            if (driver != null) {
                System.out.println("Closing driver");
                driver.quit();
            }
        }
    }

    public static void main(String[] args) {
        clickElement("loginButton");
    }
}
```

### C. try-with-resources (Java 7+)

Automatically closes resources (files, database connections, etc.)

```java
import java.io.*;

public class TryWithResourcesDemo {
    public static void main(String[] args) {
        // Old way - manual closing
        BufferedReader reader = null;
        try {
            reader = new BufferedReader(new FileReader("test.txt"));
            String line = reader.readLine();
            System.out.println(line);
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if (reader != null) {
                    reader.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

        // New way - automatic closing
        try (BufferedReader br = new BufferedReader(new FileReader("test.txt"))) {
            String line = br.readLine();
            System.out.println(line);
            // br.close() called automatically
        } catch (IOException e) {
            e.printStackTrace();
        }

        // Multiple resources
        try (
            BufferedReader br = new BufferedReader(new FileReader("input.txt"));
            BufferedWriter bw = new BufferedWriter(new FileWriter("output.txt"))
        ) {
            String line;
            while ((line = br.readLine()) != null) {
                bw.write(line);
                bw.newLine();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### D. throw and throws

**throw:** Explicitly throw an exception

```java
public class ThrowDemo {
    public static void validateAge(int age) {
        if (age < 18) {
            throw new IllegalArgumentException("Age must be at least 18");
        }
        System.out.println("Age is valid: " + age);
    }

    public static void main(String[] args) {
        try {
            validateAge(15);
        } catch (IllegalArgumentException e) {
            System.out.println("Validation error: " + e.getMessage());
        }

        validateAge(25); // Valid age
    }
}
```

**throws:** Declare that a method may throw exceptions (caller must handle)

```java
import java.io.*;

public class ThrowsDemo {
    // Method declares it throws IOException
    public static void readFile(String filename) throws IOException {
        BufferedReader reader = new BufferedReader(new FileReader(filename));
        String line = reader.readLine();
        System.out.println(line);
        reader.close();
    }

    public static void main(String[] args) {
        try {
            readFile("test.txt"); // Must handle IOException
        } catch (IOException e) {
            System.out.println("File error: " + e.getMessage());
        }
    }
}
```

**Testing Example:**

```java
public class LoginPage {
    public void login(String username, String password) throws IllegalArgumentException {
        // Validate inputs
        if (username == null || username.isEmpty()) {
            throw new IllegalArgumentException("Username cannot be empty");
        }

        if (password == null || password.length() < 8) {
            throw new IllegalArgumentException("Password must be at least 8 characters");
        }

        System.out.println("Logging in with: " + username);
    }

    public static void main(String[] args) {
        LoginPage page = new LoginPage();

        try {
            page.login("", "password"); // Empty username
        } catch (IllegalArgumentException e) {
            System.out.println("Login failed: " + e.getMessage());
        }

        try {
            page.login("admin", "123"); // Short password
        } catch (IllegalArgumentException e) {
            System.out.println("Login failed: " + e.getMessage());
        }

        try {
            page.login("admin", "admin12345"); // Valid
        } catch (IllegalArgumentException e) {
            System.out.println("Login failed: " + e.getMessage());
        }
    }
}
```

### E. Custom Exceptions

```java
// Custom checked exception
class ElementNotFoundException extends Exception {
    public ElementNotFoundException(String message) {
        super(message);
    }

    public ElementNotFoundException(String message, Throwable cause) {
        super(message, cause);
    }
}

// Custom unchecked exception
class InvalidTestDataException extends RuntimeException {
    public InvalidTestDataException(String message) {
        super(message);
    }
}

// Custom exception with additional fields
class TestFailedException extends Exception {
    private String testName;
    private String errorScreenshot;

    public TestFailedException(String message, String testName, String screenshot) {
        super(message);
        this.testName = testName;
        this.errorScreenshot = screenshot;
    }

    public String getTestName() {
        return testName;
    }

    public String getErrorScreenshot() {
        return errorScreenshot;
    }

    @Override
    public String toString() {
        return "TestFailedException: " + getMessage() +
               "\nTest: " + testName +
               "\nScreenshot: " + errorScreenshot;
    }
}

// Using custom exceptions
public class CustomExceptionDemo {
    public static void findElement(String locator) throws ElementNotFoundException {
        boolean elementFound = false; // Simulate element search

        if (!elementFound) {
            throw new ElementNotFoundException("Element not found: " + locator);
        }

        System.out.println("Element found: " + locator);
    }

    public static void validateTestData(String data) {
        if (data == null || data.isEmpty()) {
            throw new InvalidTestDataException("Test data cannot be null or empty");
        }
        System.out.println("Valid test data: " + data);
    }

    public static void executeTest(String testName) throws TestFailedException {
        try {
            System.out.println("Executing test: " + testName);
            // Simulate test execution
            boolean passed = false;

            if (!passed) {
                throw new TestFailedException(
                    "Test assertion failed",
                    testName,
                    "/screenshots/error_" + System.currentTimeMillis() + ".png"
                );
            }
        } catch (TestFailedException e) {
            System.out.println(e);
            throw e; // Re-throw
        }
    }

    public static void main(String[] args) {
        // Example 1: ElementNotFoundException
        try {
            findElement("//button[@id='submit']");
        } catch (ElementNotFoundException e) {
            System.out.println("Error: " + e.getMessage());
        }

        System.out.println();

        // Example 2: InvalidTestDataException
        try {
            validateTestData("");
        } catch (InvalidTestDataException e) {
            System.out.println("Error: " + e.getMessage());
        }

        System.out.println();

        // Example 3: TestFailedException
        try {
            executeTest("LoginTest");
        } catch (TestFailedException e) {
            System.out.println("Test execution failed");
        }
    }
}
```

### F. Exception Best Practices in Testing

```java
public class TestExceptionHandling {
    // 1. Catch specific exceptions
    public static void clickElement(String locator) {
        try {
            // driver.findElement(By.id(locator)).click();
            System.out.println("Clicking: " + locator);
        } catch (NoSuchElementException e) {
            System.out.println("Element not found: " + locator);
            // Take screenshot
            takeScreenshot();
        } catch (ElementNotInteractableException e) {
            System.out.println("Element not interactable: " + locator);
            // Wait and retry
            waitAndRetry(locator);
        } catch (StaleElementReferenceException e) {
            System.out.println("Stale element: " + locator);
            // Relocate element
            relocateAndClick(locator);
        }
    }

    // 2. Use finally for cleanup
    public static void runTest() {
        WebDriver driver = null;
        try {
            driver = initializeDriver();
            executeTestSteps();
        } catch (Exception e) {
            System.out.println("Test failed: " + e.getMessage());
            takeScreenshot();
        } finally {
            if (driver != null) {
                driver.quit();
            }
        }
    }

    // 3. Don't swallow exceptions
    public static void badPractice() {
        try {
            // Some code
        } catch (Exception e) {
            // Don't do this - silent failure!
        }
    }

    public static void goodPractice() {
        try {
            // Some code
        } catch (Exception e) {
            // Log the error
            System.out.println("Error: " + e.getMessage());
            e.printStackTrace();
            // Or re-throw
            throw e;
        }
    }

    // 4. Provide meaningful error messages
    public static void validateElement(WebElement element, String elementName) {
        if (element == null) {
            throw new IllegalArgumentException(
                "Element '" + elementName + "' is null. " +
                "Check if locator is correct and element exists on page."
            );
        }
    }

    // Placeholder methods
    private static void takeScreenshot() {
        System.out.println("Screenshot taken");
    }

    private static void waitAndRetry(String locator) {
        System.out.println("Waiting and retrying: " + locator);
    }

    private static void relocateAndClick(String locator) {
        System.out.println("Relocating and clicking: " + locator);
    }

    private static WebDriver initializeDriver() {
        System.out.println("Driver initialized");
        return null;
    }

    private static void executeTestSteps() {
        System.out.println("Executing test steps");
    }
}
```

### Common Mistakes to Avoid

1. **Catching Exception too broadly:**
```java
// Bad
try {
    // code
} catch (Exception e) { // Too broad
    // Hard to understand what went wrong
}

// Good
try {
    // code
} catch (NoSuchElementException e) {
    // Specific handling
} catch (TimeoutException e) {
    // Specific handling
}
```

2. **Not using finally for cleanup:**
```java
// Bad
WebDriver driver = new ChromeDriver();
try {
    // test code
} catch (Exception e) {
    e.printStackTrace();
}
// driver might not close if exception occurs!

// Good
WebDriver driver = new ChromeDriver();
try {
    // test code
} catch (Exception e) {
    e.printStackTrace();
} finally {
    driver.quit(); // Always closes
}
```

3. **Throwing generic exceptions:**
```java
// Bad
public void method() throws Exception {
    // Caller doesn't know what to expect
}

// Good
public void method() throws ElementNotFoundException, TimeoutException {
    // Caller knows specific exceptions to handle
}
```

### Interview Tips

**Q: Difference between checked and unchecked exceptions?**
- Checked: Compiler forces handling (IOException, SQLException)
- Unchecked: Not forced (NullPointerException, ArithmeticException)

**Q: Difference between throw and throws?**
- throw: Explicitly throw an exception (used inside method)
- throws: Declare exceptions a method might throw (in method signature)

**Q: Can we have try without catch?**
- Yes, with finally or try-with-resources

**Q: Can we have multiple catch blocks?**
- Yes, but specific exceptions before generic ones

**Q: What is exception propagation?**
- Exception moves up the call stack until caught or program terminates

**Q: When to create custom exceptions?**
- For business-specific error conditions (InvalidTestDataException, ElementNotFoundException)

---

## 12. Collections Framework

### A. Introduction to Collections

**Collection** = Group of objects as a single unit

**Collections Framework Hierarchy:**
```
Collection (Interface)
├── List (Interface) - Ordered, allows duplicates
│   ├── ArrayList (Class)
│   ├── LinkedList (Class)
│   └── Vector (Class)
├── Set (Interface) - Unordered, no duplicates
│   ├── HashSet (Class)
│   ├── LinkedHashSet (Class)
│   └── TreeSet (Class)
└── Queue (Interface) - FIFO order
    ├── PriorityQueue (Class)
    └── Deque (Interface)
        └── ArrayDeque (Class)

Map (Interface) - Key-value pairs
├── HashMap (Class)
├── LinkedHashMap (Class)
├── TreeMap (Class)
└── Hashtable (Class)
```

### B. ArrayList

**ArrayList** = Resizable array, maintains insertion order, allows duplicates

```java
import java.util.ArrayList;
import java.util.List;

public class ArrayListDemo {
    public static void main(String[] args) {
        // Creating ArrayList
        ArrayList<String> browsers = new ArrayList<>();

        // Adding elements
        browsers.add("Chrome");
        browsers.add("Firefox");
        browsers.add("Safari");
        browsers.add("Edge");
        browsers.add("Chrome"); // Duplicates allowed

        System.out.println("Browsers: " + browsers);
        System.out.println("Size: " + browsers.size());

        // Accessing elements
        System.out.println("First browser: " + browsers.get(0));
        System.out.println("Last browser: " + browsers.get(browsers.size() - 1));

        // Modifying elements
        browsers.set(1, "Firefox ESR");
        System.out.println("After modification: " + browsers);

        // Checking if element exists
        boolean hasChrome = browsers.contains("Chrome");
        System.out.println("Has Chrome: " + hasChrome);

        // Finding index
        int index = browsers.indexOf("Safari");
        System.out.println("Safari index: " + index);

        // Removing elements
        browsers.remove("Edge"); // Remove by value
        browsers.remove(0); // Remove by index
        System.out.println("After removal: " + browsers);

        // Iterating
        System.out.println("\nIterating:");
        for (String browser : browsers) {
            System.out.println("- " + browser);
        }

        // Clear all elements
        browsers.clear();
        System.out.println("After clear: " + browsers);
        System.out.println("Is empty: " + browsers.isEmpty());
    }
}
```

**Output:**
```
Browsers: [Chrome, Firefox, Safari, Edge, Chrome]
Size: 5
First browser: Chrome
Last browser: Chrome
After modification: [Chrome, Firefox ESR, Safari, Edge, Chrome]
Has Chrome: true
Safari index: 2
After removal: [Firefox ESR, Safari, Chrome]

Iterating:
- Firefox ESR
- Safari
- Chrome
After clear: []
Is empty: true
```

**Testing Example:**

```java
import java.util.ArrayList;
import java.util.List;

public class TestCaseManager {
    public static void main(String[] args) {
        // Store test cases
        List<String> testCases = new ArrayList<>();

        testCases.add("TC001: Valid Login");
        testCases.add("TC002: Invalid Login");
        testCases.add("TC003: Forgot Password");
        testCases.add("TC004: Add to Cart");
        testCases.add("TC005: Checkout");

        System.out.println("Total test cases: " + testCases.size());

        // Execute tests
        List<String> passed = new ArrayList<>();
        List<String> failed = new ArrayList<>();

        for (String testCase : testCases) {
            System.out.println("Executing: " + testCase);

            // Simulate test execution
            boolean result = !testCase.contains("Invalid");

            if (result) {
                passed.add(testCase);
            } else {
                failed.add(testCase);
            }
        }

        // Report
        System.out.println("\n===== TEST REPORT =====");
        System.out.println("Total: " + testCases.size());
        System.out.println("Passed: " + passed.size());
        System.out.println("Failed: " + failed.size());

        System.out.println("\nFailed Tests:");
        for (String test : failed) {
            System.out.println("- " + test);
        }
    }
}
```

### C. LinkedList

**LinkedList** = Doubly linked list, efficient insertion/deletion

```java
import java.util.LinkedList;

public class LinkedListDemo {
    public static void main(String[] args) {
        LinkedList<String> urls = new LinkedList<>();

        // Adding elements
        urls.add("https://google.com");
        urls.add("https://amazon.com");
        urls.add("https://facebook.com");

        // Add at beginning
        urls.addFirst("https://example.com");

        // Add at end
        urls.addLast("https://twitter.com");

        System.out.println("URLs: " + urls);

        // Access first and last
        System.out.println("First: " + urls.getFirst());
        System.out.println("Last: " + urls.getLast());

        // Remove first and last
        String removedFirst = urls.removeFirst();
        String removedLast = urls.removeLast();

        System.out.println("Removed first: " + removedFirst);
        System.out.println("Removed last: " + removedLast);
        System.out.println("After removal: " + urls);

        // LinkedList as Queue
        LinkedList<String> queue = new LinkedList<>();
        queue.offer("Task 1"); // Add to end
        queue.offer("Task 2");
        queue.offer("Task 3");

        System.out.println("\nQueue: " + queue);
        System.out.println("Poll: " + queue.poll()); // Remove from beginning
        System.out.println("After poll: " + queue);

        // LinkedList as Stack
        LinkedList<String> stack = new LinkedList<>();
        stack.push("Page 1"); // Add to beginning
        stack.push("Page 2");
        stack.push("Page 3");

        System.out.println("\nStack: " + stack);
        System.out.println("Pop: " + stack.pop()); // Remove from beginning
        System.out.println("After pop: " + stack);
    }
}
```

**ArrayList vs LinkedList:**

| Feature | ArrayList | LinkedList |
|---------|-----------|------------|
| Structure | Dynamic array | Doubly linked list |
| Access Time | O(1) - Fast | O(n) - Slow |
| Insertion/Deletion | O(n) - Slow | O(1) - Fast |
| Memory | Less overhead | More (node pointers) |
| Use Case | Frequent access | Frequent add/remove |

### D. HashSet

**HashSet** = Unordered, no duplicates, fast lookup

```java
import java.util.HashSet;
import java.util.Set;

public class HashSetDemo {
    public static void main(String[] args) {
        Set<String> browsers = new HashSet<>();

        // Adding elements
        browsers.add("Chrome");
        browsers.add("Firefox");
        browsers.add("Safari");
        browsers.add("Chrome"); // Duplicate - not added

        System.out.println("Browsers: " + browsers);
        System.out.println("Size: " + browsers.size()); // 3, not 4

        // Checking existence
        System.out.println("Has Firefox: " + browsers.contains("Firefox"));

        // Removing
        browsers.remove("Safari");
        System.out.println("After removal: " + browsers);

        // Iterating (order not guaranteed)
        System.out.println("\nIterating:");
        for (String browser : browsers) {
            System.out.println("- " + browser);
        }
    }
}
```

**Testing Example: Remove Duplicates**

```java
import java.util.*;

public class DuplicateRemover {
    public static void main(String[] args) {
        // Test data with duplicates
        List<String> testData = new ArrayList<>();
        testData.add("admin");
        testData.add("user1");
        testData.add("admin"); // Duplicate
        testData.add("user2");
        testData.add("user1"); // Duplicate
        testData.add("guest");

        System.out.println("Original: " + testData);
        System.out.println("Size: " + testData.size());

        // Remove duplicates using HashSet
        Set<String> uniqueData = new HashSet<>(testData);

        System.out.println("\nUnique: " + uniqueData);
        System.out.println("Size: " + uniqueData.size());

        // Convert back to List
        List<String> uniqueList = new ArrayList<>(uniqueData);
        System.out.println("\nUnique List: " + uniqueList);
    }
}
```

### E. HashMap

**HashMap** = Key-value pairs, fast lookup by key

```java
import java.util.HashMap;
import java.util.Map;

public class HashMapDemo {
    public static void main(String[] args) {
        // Creating HashMap
        Map<String, String> testData = new HashMap<>();

        // Adding key-value pairs
        testData.put("username", "admin");
        testData.put("password", "admin123");
        testData.put("email", "admin@test.com");
        testData.put("phone", "1234567890");

        System.out.println("Test Data: " + testData);

        // Getting values
        String username = testData.get("username");
        System.out.println("Username: " + username);

        // Checking if key exists
        boolean hasEmail = testData.containsKey("email");
        System.out.println("Has email: " + hasEmail);

        // Checking if value exists
        boolean hasAdmin = testData.containsValue("admin");
        System.out.println("Has 'admin' value: " + hasAdmin);

        // Updating value
        testData.put("username", "testuser"); // Overwrites existing
        System.out.println("Updated username: " + testData.get("username"));

        // Removing
        testData.remove("phone");
        System.out.println("After removal: " + testData);

        // Iterating over keys
        System.out.println("\nKeys:");
        for (String key : testData.keySet()) {
            System.out.println("- " + key);
        }

        // Iterating over values
        System.out.println("\nValues:");
        for (String value : testData.values()) {
            System.out.println("- " + value);
        }

        // Iterating over entries
        System.out.println("\nEntries:");
        for (Map.Entry<String, String> entry : testData.entrySet()) {
            System.out.println(entry.getKey() + " = " + entry.getValue());
        }

        // Size and empty check
        System.out.println("\nSize: " + testData.size());
        System.out.println("Is empty: " + testData.isEmpty());
    }
}
```

**Testing Example: Test Configuration**

```java
import java.util.HashMap;
import java.util.Map;

public class TestConfig {
    private Map<String, String> config;

    public TestConfig() {
        config = new HashMap<>();
        loadConfig();
    }

    private void loadConfig() {
        config.put("browser", "Chrome");
        config.put("url", "https://example.com");
        config.put("timeout", "30");
        config.put("environment", "QA");
        config.put("headless", "false");
    }

    public String getProperty(String key) {
        return config.get(key);
    }

    public void setProperty(String key, String value) {
        config.put(key, value);
    }

    public void displayConfig() {
        System.out.println("===== TEST CONFIGURATION =====");
        for (Map.Entry<String, String> entry : config.entrySet()) {
            System.out.println(entry.getKey() + ": " + entry.getValue());
        }
    }

    public static void main(String[] args) {
        TestConfig config = new TestConfig();
        config.displayConfig();

        System.out.println("\nBrowser: " + config.getProperty("browser"));
        System.out.println("URL: " + config.getProperty("url"));

        // Update configuration
        config.setProperty("environment", "PROD");
        config.setProperty("headless", "true");

        System.out.println("\n===== UPDATED CONFIGURATION =====");
        config.displayConfig();
    }
}
```

**Advanced HashMap Example:**

```java
import java.util.HashMap;
import java.util.Map;

public class TestResultTracker {
    public static void main(String[] args) {
        // Map of test name to result
        Map<String, Boolean> testResults = new HashMap<>();

        testResults.put("LoginTest", true);
        testResults.put("LogoutTest", true);
        testResults.put("SearchTest", false);
        testResults.put("CheckoutTest", true);
        testResults.put("PaymentTest", false);

        // Calculate pass/fail count
        int passed = 0;
        int failed = 0;

        for (Map.Entry<String, Boolean> entry : testResults.entrySet()) {
            if (entry.getValue()) {
                passed++;
            } else {
                failed++;
            }
        }

        System.out.println("===== TEST RESULTS =====");
        System.out.println("Total: " + testResults.size());
        System.out.println("Passed: " + passed);
        System.out.println("Failed: " + failed);
        System.out.println("Pass Rate: " + (passed * 100.0 / testResults.size()) + "%");

        System.out.println("\nFailed Tests:");
        for (Map.Entry<String, Boolean> entry : testResults.entrySet()) {
            if (!entry.getValue()) {
                System.out.println("- " + entry.getKey());
            }
        }

        // Nested Map - Test results with details
        Map<String, Map<String, String>> detailedResults = new HashMap<>();

        Map<String, String> loginDetails = new HashMap<>();
        loginDetails.put("status", "Passed");
        loginDetails.put("duration", "5s");
        loginDetails.put("browser", "Chrome");

        Map<String, String> searchDetails = new HashMap<>();
        searchDetails.put("status", "Failed");
        searchDetails.put("duration", "3s");
        searchDetails.put("error", "Element not found");

        detailedResults.put("LoginTest", loginDetails);
        detailedResults.put("SearchTest", searchDetails);

        System.out.println("\n===== DETAILED RESULTS =====");
        for (Map.Entry<String, Map<String, String>> entry : detailedResults.entrySet()) {
            System.out.println("\n" + entry.getKey() + ":");
            for (Map.Entry<String, String> detail : entry.getValue().entrySet()) {
                System.out.println("  " + detail.getKey() + ": " + detail.getValue());
            }
        }
    }
}
```

### F. Iterator

**Iterator** = Traverse collection and remove elements safely

```java
import java.util.*;

public class IteratorDemo {
    public static void main(String[] args) {
        List<String> browsers = new ArrayList<>();
        browsers.add("Chrome");
        browsers.add("Firefox");
        browsers.add("Safari");
        browsers.add("Edge");
        browsers.add("IE"); // To be removed

        System.out.println("Original: " + browsers);

        // Using Iterator
        Iterator<String> iterator = browsers.iterator();

        System.out.println("\nIterating with Iterator:");
        while (iterator.hasNext()) {
            String browser = iterator.next();
            System.out.println("- " + browser);

            // Remove IE
            if (browser.equals("IE")) {
                iterator.remove(); // Safe removal
            }
        }

        System.out.println("\nAfter removal: " + browsers);

        // ListIterator (bidirectional)
        ListIterator<String> listIterator = browsers.listIterator();

        System.out.println("\nForward iteration:");
        while (listIterator.hasNext()) {
            System.out.println("- " + listIterator.next());
        }

        System.out.println("\nBackward iteration:");
        while (listIterator.hasPrevious()) {
            System.out.println("- " + listIterator.previous());
        }
    }
}
```

**Testing Example:**

```java
import java.util.*;

public class TestExecutor {
    public static void main(String[] args) {
        List<String> testCases = new ArrayList<>();
        testCases.add("LoginTest");
        testCases.add("DisabledTest"); // To skip
        testCases.add("SearchTest");
        testCases.add("SkipTest"); // To skip
        testCases.add("CheckoutTest");

        System.out.println("Scheduled tests: " + testCases);

        // Remove tests to skip
        Iterator<String> iterator = testCases.iterator();
        int skipped = 0;

        while (iterator.hasNext()) {
            String test = iterator.next();
            if (test.contains("Disabled") || test.contains("Skip")) {
                System.out.println("Skipping: " + test);
                iterator.remove();
                skipped++;
            }
        }

        System.out.println("\nTests to execute: " + testCases);
        System.out.println("Skipped: " + skipped);

        // Execute remaining tests
        System.out.println("\n===== EXECUTING TESTS =====");
        for (String test : testCases) {
            System.out.println("Executing: " + test);
        }
    }
}
```

### Collections Comparison Table

| Collection | Ordered | Duplicates | Null | Thread-Safe | Use Case |
|------------|---------|------------|------|-------------|----------|
| ArrayList | Yes | Yes | Yes | No | Random access, frequent reads |
| LinkedList | Yes | Yes | Yes | No | Frequent add/remove |
| HashSet | No | No | One | No | Unique elements, fast lookup |
| LinkedHashSet | Yes | No | One | No | Unique + maintain order |
| TreeSet | Sorted | No | No | No | Sorted unique elements |
| HashMap | No | Keys: No, Values: Yes | One null key | No | Key-value pairs, fast lookup |
| LinkedHashMap | Yes | Keys: No, Values: Yes | One null key | No | Maintain insertion order |
| TreeMap | Sorted | Keys: No, Values: Yes | No | No | Sorted key-value pairs |

### Common Mistakes to Avoid

1. **Modifying collection during iteration:**
```java
// Wrong - ConcurrentModificationException
List<String> list = new ArrayList<>(Arrays.asList("A", "B", "C"));
for (String item : list) {
    if (item.equals("B")) {
        list.remove(item); // ERROR!
    }
}

// Correct - use Iterator
Iterator<String> it = list.iterator();
while (it.hasNext()) {
    if (it.next().equals("B")) {
        it.remove(); // OK
    }
}
```

2. **Using wrong collection:**
```java
// Wrong - ArrayList for frequent add/remove at beginning
List<String> list = new ArrayList<>();
for (int i = 0; i < 10000; i++) {
    list.add(0, "Item"); // Slow - O(n)
}

// Correct - LinkedList
List<String> list = new LinkedList<>();
for (int i = 0; i < 10000; i++) {
    list.add(0, "Item"); // Fast - O(1)
}
```

3. **Not specifying generic type:**
```java
// Wrong - raw type
List list = new ArrayList(); // Allows any type
list.add("String");
list.add(123);
String str = (String) list.get(1); // Runtime error!

// Correct - use generics
List<String> list = new ArrayList<>();
list.add("String");
// list.add(123); // Compilation error - type safe!
```

### Interview Tips

**Q: Difference between ArrayList and LinkedList?**
- See comparison table above

**Q: Difference between HashSet and TreeSet?**
- HashSet: Unordered, faster (O(1))
- TreeSet: Sorted, slower (O(log n))

**Q: Difference between HashMap and Hashtable?**
- HashMap: Not synchronized, allows null
- Hashtable: Synchronized, no null

**Q: How does HashMap work internally?**
- Uses hashing, array of buckets, linked list/tree for collisions

**Q: Why use ArrayList over array?**
- Dynamic size, built-in methods, generic support

**Q: When to use which collection?**
- ArrayList: Random access
- LinkedList: Frequent insertion/deletion
- HashSet: Unique elements
- HashMap: Key-value mapping

---

*File continues with sections 13-17...*
## 13. Generics

### A. What are Generics?

**Generics** = Type parameters that allow classes, interfaces, and methods to work with different data types while providing compile-time type safety.

**Benefits:**
- Type safety at compile-time
- No need for type casting
- Enables writing generic algorithms

**Before Generics (Java < 5):**

```java
// Without generics
List list = new ArrayList();
list.add("Hello");
list.add(123);
list.add(45.6);

String str = (String) list.get(0); // Type casting required
Integer num = (Integer) list.get(1); // Risk of ClassCastException
```

**With Generics (Java 5+):**

```java
// With generics
List<String> list = new ArrayList<String>();
list.add("Hello");
// list.add(123); // Compilation error - type safe!

String str = list.get(0); // No casting needed
```

### B. Generic Classes

```java
// Generic class with type parameter T
class Box<T> {
    private T content;

    public void set(T content) {
        this.content = content;
    }

    public T get() {
        return content;
    }

    public void display() {
        System.out.println("Content: " + content + " (" + content.getClass().getSimpleName() + ")");
    }
}

public class GenericClassDemo {
    public static void main(String[] args) {
        // Box of String
        Box<String> stringBox = new Box<>();
        stringBox.set("Hello Generics");
        String str = stringBox.get(); // No casting
        stringBox.display();

        // Box of Integer
        Box<Integer> intBox = new Box<>();
        intBox.set(123);
        Integer num = intBox.get(); // No casting
        intBox.display();

        // Box of custom object
        Box<WebDriver> driverBox = new Box<>();
        // driverBox.set(new ChromeDriver());
        // WebDriver driver = driverBox.get();
    }
}
```

**Output:**
```
Content: Hello Generics (String)
Content: 123 (Integer)
```

**Testing Example:**

```java
class TestResult<T> {
    private String testName;
    private T result;
    private boolean passed;

    public TestResult(String testName, T result, boolean passed) {
        this.testName = testName;
        this.result = result;
        this.passed = passed;
    }

    public void displayResult() {
        System.out.println("Test: " + testName);
        System.out.println("Result: " + result);
        System.out.println("Status: " + (passed ? "PASSED" : "FAILED"));
        System.out.println("---");
    }
}

public class TestResultDemo {
    public static void main(String[] args) {
        // String result
        TestResult<String> test1 = new TestResult<>("LoginTest", "User logged in successfully", true);
        test1.displayResult();

        // Integer result (count)
        TestResult<Integer> test2 = new TestResult<>("SearchTest", 45, true);
        test2.displayResult();

        // Boolean result
        TestResult<Boolean> test3 = new TestResult<>("ValidationTest", false, false);
        test3.displayResult();
    }
}
```

**Multiple Type Parameters:**

```java
class Pair<K, V> {
    private K key;
    private V value;

    public Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }

    public K getKey() {
        return key;
    }

    public V getValue() {
        return value;
    }

    public void display() {
        System.out.println(key + " = " + value);
    }
}

public class PairDemo {
    public static void main(String[] args) {
        Pair<String, String> config1 = new Pair<>("browser", "Chrome");
        config1.display();

        Pair<String, Integer> config2 = new Pair<>("timeout", 30);
        config2.display();

        Pair<Integer, Boolean> result = new Pair<>(1, true);
        result.display();
    }
}
```

### C. Generic Methods

```java
public class GenericMethodDemo {
    // Generic method
    public static <T> void printArray(T[] array) {
        for (T element : array) {
            System.out.print(element + " ");
        }
        System.out.println();
    }

    // Generic method with return type
    public static <T> T getFirstElement(T[] array) {
        if (array != null && array.length > 0) {
            return array[0];
        }
        return null;
    }

    // Multiple type parameters
    public static <K, V> void printMap(K key, V value) {
        System.out.println(key + " -> " + value);
    }

    public static void main(String[] args) {
        // Integer array
        Integer[] intArray = {1, 2, 3, 4, 5};
        printArray(intArray);

        // String array
        String[] strArray = {"Chrome", "Firefox", "Safari"};
        printArray(strArray);

        // First element
        String first = getFirstElement(strArray);
        System.out.println("First browser: " + first);

        // Multiple types
        printMap("browser", "Chrome");
        printMap("timeout", 30);
        printMap(1, true);
    }
}
```

**Testing Example:**

```java
public class TestUtils {
    // Generic method to find element in array
    public static <T> boolean contains(T[] array, T element) {
        for (T item : array) {
            if (item.equals(element)) {
                return true;
            }
        }
        return false;
    }

    // Generic method to count elements
    public static <T> int count(List<T> list, T element) {
        int count = 0;
        for (T item : list) {
            if (item.equals(element)) {
                count++;
            }
        }
        return count;
    }

    // Generic method to get random element
    public static <T> T getRandomElement(List<T> list) {
        if (list == null || list.isEmpty()) {
            return null;
        }
        int randomIndex = (int) (Math.random() * list.size());
        return list.get(randomIndex);
    }

    public static void main(String[] args) {
        String[] browsers = {"Chrome", "Firefox", "Safari", "Edge"};
        System.out.println("Has Chrome: " + contains(browsers, "Chrome"));
        System.out.println("Has Opera: " + contains(browsers, "Opera"));

        List<String> results = Arrays.asList("Pass", "Fail", "Pass", "Pass", "Fail");
        System.out.println("Pass count: " + count(results, "Pass"));
        System.out.println("Fail count: " + count(results, "Fail"));

        List<String> testData = Arrays.asList("user1", "user2", "user3");
        System.out.println("Random user: " + getRandomElement(testData));
    }
}
```

### D. Bounded Type Parameters

```java
// Upper bound - T must be Number or its subclass
class Calculator<T extends Number> {
    private T num1;
    private T num2;

    public Calculator(T num1, T num2) {
        this.num1 = num1;
        this.num2 = num2;
    }

    public double add() {
        return num1.doubleValue() + num2.doubleValue();
    }

    public double multiply() {
        return num1.doubleValue() * num2.doubleValue();
    }
}

public class BoundedTypeDemo {
    public static void main(String[] args) {
        Calculator<Integer> intCalc = new Calculator<>(10, 20);
        System.out.println("Int Add: " + intCalc.add());

        Calculator<Double> doubleCalc = new Calculator<>(10.5, 20.5);
        System.out.println("Double Add: " + doubleCalc.add());

        // Calculator<String> strCalc = new Calculator<>("10", "20"); // ERROR! String is not a Number
    }
}
```

**Multiple Bounds:**

```java
interface Testable {
    void runTest();
}

class BaseTest {
    public void setUp() {
        System.out.println("Setup");
    }
}

// T must extend BaseTest AND implement Testable
class TestRunner<T extends BaseTest & Testable> {
    private T test;

    public TestRunner(T test) {
        this.test = test;
    }

    public void execute() {
        test.setUp(); // From BaseTest
        test.runTest(); // From Testable
    }
}
```

### E. Wildcards

**1. Unbounded Wildcard (?)** - Any type

```java
public class UnboundedWildcardDemo {
    public static void printList(List<?> list) {
        for (Object element : list) {
            System.out.print(element + " ");
        }
        System.out.println();
    }

    public static void main(String[] args) {
        List<String> strList = Arrays.asList("Chrome", "Firefox", "Safari");
        List<Integer> intList = Arrays.asList(1, 2, 3, 4, 5);
        List<Double> doubleList = Arrays.asList(1.1, 2.2, 3.3);

        printList(strList);
        printList(intList);
        printList(doubleList);
    }
}
```

**2. Upper Bounded Wildcard (? extends Type)** - Type or its subclasses

```java
public class UpperBoundedDemo {
    // Accepts List of Number or its subclasses (Integer, Double, etc.)
    public static double sum(List<? extends Number> numbers) {
        double total = 0;
        for (Number num : numbers) {
            total += num.doubleValue();
        }
        return total;
    }

    public static void main(String[] args) {
        List<Integer> intList = Arrays.asList(1, 2, 3, 4, 5);
        List<Double> doubleList = Arrays.asList(1.5, 2.5, 3.5);

        System.out.println("Int sum: " + sum(intList));
        System.out.println("Double sum: " + sum(doubleList));
    }
}
```

**3. Lower Bounded Wildcard (? super Type)** - Type or its superclasses

```java
public class LowerBoundedDemo {
    public static void addNumbers(List<? super Integer> list) {
        for (int i = 1; i <= 5; i++) {
            list.add(i);
        }
    }

    public static void main(String[] args) {
        List<Integer> intList = new ArrayList<>();
        List<Number> numList = new ArrayList<>();
        List<Object> objList = new ArrayList<>();

        addNumbers(intList); // OK
        addNumbers(numList); // OK (Number is superclass of Integer)
        addNumbers(objList); // OK (Object is superclass of Integer)

        System.out.println("Int list: " + intList);
        System.out.println("Num list: " + numList);
        System.out.println("Obj list: " + objList);
    }
}
```

### F. Generic Interfaces

```java
interface Repository<T> {
    void save(T entity);
    T findById(int id);
    List<T> findAll();
    void delete(T entity);
}

class TestCase {
    private int id;
    private String name;

    public TestCase(int id, String name) {
        this.id = id;
        this.name = name;
    }

    public int getId() {
        return id;
    }

    @Override
    public String toString() {
        return "TestCase{id=" + id + ", name='" + name + "'}";
    }
}

class TestCaseRepository implements Repository<TestCase> {
    private List<TestCase> testCases = new ArrayList<>();

    @Override
    public void save(TestCase entity) {
        testCases.add(entity);
        System.out.println("Saved: " + entity);
    }

    @Override
    public TestCase findById(int id) {
        for (TestCase tc : testCases) {
            if (tc.getId() == id) {
                return tc;
            }
        }
        return null;
    }

    @Override
    public List<TestCase> findAll() {
        return new ArrayList<>(testCases);
    }

    @Override
    public void delete(TestCase entity) {
        testCases.remove(entity);
        System.out.println("Deleted: " + entity);
    }
}

public class GenericInterfaceDemo {
    public static void main(String[] args) {
        Repository<TestCase> repo = new TestCaseRepository();

        repo.save(new TestCase(1, "LoginTest"));
        repo.save(new TestCase(2, "LogoutTest"));
        repo.save(new TestCase(3, "SearchTest"));

        System.out.println("\nAll test cases:");
        for (TestCase tc : repo.findAll()) {
            System.out.println(tc);
        }

        System.out.println("\nFind by ID:");
        TestCase found = repo.findById(2);
        System.out.println(found);
    }
}
```

### Common Mistakes to Avoid

1. **Using raw types:**
```java
// Bad
List list = new ArrayList(); // Raw type
list.add("String");
list.add(123);

// Good
List<String> list = new ArrayList<>();
list.add("String");
```

2. **Cannot create generic array:**
```java
// ERROR! Cannot create generic array
// List<String>[] array = new List<String>[10];

// Workaround
List<String>[] array = (List<String>[]) new List[10];
```

3. **Cannot use primitives with generics:**
```java
// ERROR!
// List<int> list = new ArrayList<>();

// Correct - use wrapper classes
List<Integer> list = new ArrayList<>();
```

### Interview Tips

**Q: What are generics?**
- Type parameters for compile-time type safety

**Q: Benefits of generics?**
- Type safety, no casting, code reusability

**Q: Difference between <?> and <T>?**
- <?> wildcard - read-only, any type
- <T> type parameter - can read and write

**Q: Difference between <? extends T> and <? super T>?**
- extends: upper bound - read only
- super: lower bound - write only (PECS: Producer Extends, Consumer Super)

**Q: Can we use primitives with generics?**
- No, only reference types. Use wrapper classes.

---

## 14. String Manipulation and Regular Expressions

### A. String Class

**String** = Immutable sequence of characters

```java
public class StringDemo {
    public static void main(String[] args) {
        // Creating strings
        String str1 = "Hello"; // String literal
        String str2 = new String("Hello"); // Using new keyword
        String str3 = "Hello"; // Reuses str1 from string pool

        System.out.println("str1 == str2: " + (str1 == str2)); // false (different objects)
        System.out.println("str1 == str3: " + (str1 == str3)); // true (same object in pool)
        System.out.println("str1.equals(str2): " + str1.equals(str2)); // true (same content)

        // String methods
        String text = "Selenium WebDriver Automation Testing";

        System.out.println("\n--- String Methods ---");
        System.out.println("Length: " + text.length());
        System.out.println("Char at 0: " + text.charAt(0));
        System.out.println("Substring(0, 8): " + text.substring(0, 8));
        System.out.println("Index of 'Web': " + text.indexOf("Web"));
        System.out.println("Last index of 'a': " + text.lastIndexOf("a"));
        System.out.println("Contains 'Auto': " + text.contains("Auto"));
        System.out.println("Starts with 'Sel': " + text.startsWith("Sel"));
        System.out.println("Ends with 'ing': " + text.endsWith("ing"));
        System.out.println("Uppercase: " + text.toUpperCase());
        System.out.println("Lowercase: " + text.toLowerCase());
        System.out.println("Replace 'Testing' with 'Framework': " + text.replace("Testing", "Framework"));

        // Trim
        String spacedText = "  Hello World  ";
        System.out.println("\nOriginal: '" + spacedText + "'");
        System.out.println("Trimmed: '" + spacedText.trim() + "'");

        // Split
        String browsers = "Chrome,Firefox,Safari,Edge";
        String[] browserArray = browsers.split(",");
        System.out.println("\nSplit result:");
        for (String browser : browserArray) {
            System.out.println("- " + browser);
        }

        // Empty and blank checks
        String empty = "";
        String blank = "   ";

        System.out.println("\nEmpty string isEmpty: " + empty.isEmpty());
        System.out.println("Empty string isBlank: " + empty.isBlank()); // Java 11+
        System.out.println("Blank string isEmpty: " + blank.isEmpty());
        System.out.println("Blank string isBlank: " + blank.isBlank()); // Java 11+
    }
}
```

**Testing Examples:**

```java
public class StringTestingExamples {
    public static void main(String[] args) {
        // Extract text
        String fullText = "Welcome, Admin User!";
        String username = fullText.substring(9, 19);
        System.out.println("Username: " + username);

        // Validate email
        String email = "test@example.com";
        if (email.contains("@") && email.contains(".")) {
            System.out.println("Valid email format");
        } else {
            System.out.println("Invalid email format");
        }

        // URL manipulation
        String baseUrl = "https://example.com";
        String loginPath = "/login";
        String fullUrl = baseUrl + loginPath;
        System.out.println("Full URL: " + fullUrl);

        // Case-insensitive comparison
        String expected = "Login Successful";
        String actual = "login successful";
        if (expected.equalsIgnoreCase(actual)) {
            System.out.println("Test Passed");
        }

        // Extract numbers
        String priceText = "Price: $125.99";
        String price = priceText.replaceAll("[^0-9.]", "");
        System.out.println("Extracted price: " + price);

        // Parse test result
        String result = "Test: LoginTest | Status: Passed | Duration: 5s";
        String[] parts = result.split("\\|");
        for (String part : parts) {
            System.out.println(part.trim());
        }
    }
}
```

### B. StringBuilder and StringBuffer

**Why use StringBuilder/StringBuffer?**
- Strings are immutable - every modification creates new object
- StringBuilder/StringBuffer are mutable - more efficient for frequent modifications

```java
public class StringBuilderDemo {
    public static void main(String[] args) {
        // String concatenation (inefficient)
        String str = "";
        long startTime = System.currentTimeMillis();
        for (int i = 0; i < 10000; i++) {
            str += "a"; // Creates new object each time!
        }
        long endTime = System.currentTimeMillis();
        System.out.println("String time: " + (endTime - startTime) + "ms");

        // StringBuilder (efficient)
        StringBuilder sb = new StringBuilder();
        startTime = System.currentTimeMillis();
        for (int i = 0; i < 10000; i++) {
            sb.append("a"); // Modifies same object
        }
        endTime = System.currentTimeMillis();
        System.out.println("StringBuilder time: " + (endTime - startTime) + "ms");

        // StringBuilder methods
        StringBuilder builder = new StringBuilder("Selenium");

        builder.append(" WebDriver"); // Add to end
        System.out.println("After append: " + builder);

        builder.insert(8, " Java"); // Insert at position
        System.out.println("After insert: " + builder);

        builder.delete(8, 13); // Delete range
        System.out.println("After delete: " + builder);

        builder.reverse(); // Reverse
        System.out.println("After reverse: " + builder);

        builder.reverse(); // Reverse back
        builder.replace(0, 8, "Appium"); // Replace range
        System.out.println("After replace: " + builder);

        // Convert to String
        String result = builder.toString();
        System.out.println("Final string: " + result);
    }
}
```

**StringBuilder vs StringBuffer:**

| Feature | StringBuilder | StringBuffer |
|---------|---------------|--------------|
| Thread-safe | No | Yes |
| Performance | Faster | Slower |
| Since | Java 5 | Java 1.0 |
| Use when | Single thread | Multiple threads |

**Testing Example:**

```java
public class ReportBuilder {
    public static String generateReport(Map<String, Boolean> results) {
        StringBuilder report = new StringBuilder();

        report.append("===== TEST EXECUTION REPORT =====\n");
        report.append("Date: ").append(new Date()).append("\n");
        report.append("Total Tests: ").append(results.size()).append("\n\n");

        int passed = 0;
        int failed = 0;

        report.append("Test Results:\n");
        for (Map.Entry<String, Boolean> entry : results.entrySet()) {
            report.append("- ")
                  .append(entry.getKey())
                  .append(": ")
                  .append(entry.getValue() ? "PASSED" : "FAILED")
                  .append("\n");

            if (entry.getValue()) {
                passed++;
            } else {
                failed++;
            }
        }

        report.append("\nSummary:\n");
        report.append("Passed: ").append(passed).append("\n");
        report.append("Failed: ").append(failed).append("\n");
        report.append("Pass Rate: ").append(passed * 100.0 / results.size()).append("%\n");

        return report.toString();
    }

    public static void main(String[] args) {
        Map<String, Boolean> results = new HashMap<>();
        results.put("LoginTest", true);
        results.put("LogoutTest", true);
        results.put("SearchTest", false);
        results.put("CheckoutTest", true);

        String report = generateReport(results);
        System.out.println(report);
    }
}
```

### C. Regular Expressions (Regex)

**Regular Expression** = Pattern for matching text

**Common Regex Patterns:**

| Pattern | Meaning | Example |
|---------|---------|---------|
| . | Any character | a.c matches abc, adc |
| * | 0 or more | ab*c matches ac, abc, abbc |
| + | 1 or more | ab+c matches abc, abbc |
| ? | 0 or 1 | ab?c matches ac, abc |
| \\d | Digit [0-9] | \\d+ matches 123 |
| \\D | Non-digit | \\D+ matches abc |
| \\w | Word character [a-zA-Z0-9_] | \\w+ matches test123 |
| \\W | Non-word character | \\W matches @ |
| \\s | Whitespace | \\s matches space, tab |
| \\S | Non-whitespace | \\S+ matches hello |
| ^ | Start of string | ^Hello matches Hello... |
| $ | End of string | ...end$ matches ...end |
| [abc] | Any of a, b, c | [aeiou] matches vowels |
| [^abc] | Not a, b, c | [^0-9] matches non-digits |
| {n} | Exactly n times | \\d{3} matches 123 |
| {n,} | n or more times | \\d{3,} matches 123, 1234 |
| {n,m} | Between n and m | \\d{2,4} matches 12, 123, 1234 |

**Java Regex Example:**

```java
import java.util.regex.*;

public class RegexDemo {
    public static void main(String[] args) {
        // Email validation
        String email = "test@example.com";
        String emailPattern = "^[A-Za-z0-9+_.-]+@[A-Za-z0-9.-]+\\.[A-Za-z]{2,}$";

        if (email.matches(emailPattern)) {
            System.out.println("Valid email");
        } else {
            System.out.println("Invalid email");
        }

        // Phone number validation
        String phone = "123-456-7890";
        String phonePattern = "\\d{3}-\\d{3}-\\d{4}";

        if (phone.matches(phonePattern)) {
            System.out.println("Valid phone");
        }

        // Extract all numbers
        String text = "Order ID: 12345, Price: $99.99, Quantity: 3";
        Pattern pattern = Pattern.compile("\\d+");
        Matcher matcher = pattern.matcher(text);

        System.out.println("\nExtracted numbers:");
        while (matcher.find()) {
            System.out.println(matcher.group());
        }

        // Replace
        String input = "Hello123World456";
        String output = input.replaceAll("\\d+", "");
        System.out.println("\nAfter removing numbers: " + output);

        // Split by regex
        String data = "Chrome,  Firefox,  Safari,  Edge";
        String[] browsers = data.split(",\\s*");
        System.out.println("\nSplit result:");
        for (String browser : browsers) {
            System.out.println("- " + browser);
        }
    }
}
```

**Testing Examples:**

```java
import java.util.regex.*;

public class RegexTestingExamples {
    // Validate password (8+ chars, 1 uppercase, 1 lowercase, 1 digit, 1 special)
    public static boolean validatePassword(String password) {
        String pattern = "^(?=.*[a-z])(?=.*[A-Z])(?=.*\\d)(?=.*[@#$%^&+=]).{8,}$";
        return password.matches(pattern);
    }

    // Extract error messages
    public static void extractErrors(String pageSource) {
        Pattern pattern = Pattern.compile("Error: (.+?)(?=<|$)");
        Matcher matcher = pattern.matcher(pageSource);

        System.out.println("Errors found:");
        while (matcher.find()) {
            System.out.println("- " + matcher.group(1));
        }
    }

    // Validate URL
    public static boolean isValidURL(String url) {
        String pattern = "^(https?://)?(www\\.)?[a-zA-Z0-9-]+(\\.[a-zA-Z]{2,})+(/.*)?$";
        return url.matches(pattern);
    }

    // Extract XPath locators
    public static void extractXPaths(String code) {
        Pattern pattern = Pattern.compile("//[^\"]+");
        Matcher matcher = pattern.matcher(code);

        System.out.println("XPath locators:");
        while (matcher.find()) {
            System.out.println("- " + matcher.group());
        }
    }

    public static void main(String[] args) {
        // Password validation
        System.out.println("Password valid: " + validatePassword("Test@123"));
        System.out.println("Password valid: " + validatePassword("weak"));

        // URL validation
        System.out.println("\nURL valid: " + isValidURL("https://example.com"));
        System.out.println("URL valid: " + isValidURL("not a url"));

        // Extract errors
        String html = "<div>Error: Invalid username</div><div>Error: Password required</div>";
        extractErrors(html);

        // Extract XPaths
        String testCode = "driver.findElement(By.xpath(\"//button[@id='submit']\")).click();";
        extractXPaths(testCode);
    }
}
```

### Common Mistakes to Avoid

1. **Forgetting to escape special characters:**
```java
// Wrong
String pattern = ".";  // Matches any character
// Correct
String pattern = "\\."; // Matches literal dot
```

2. **Not using raw strings for backslashes:**
```java
// Confusing
String pattern = "\\d+"; // One backslash in regex, two in Java string
// Same as
String regex = "\\\\d+"; // Would be wrong!
```

3. **Inefficient string concatenation:**
```java
// Bad
String str = "";
for (int i = 0; i < 1000; i++) {
    str += "a"; // Creates 1000 objects!
}

// Good
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 1000; i++) {
    sb.append("a"); // One object
}
```

### Interview Tips

**Q: Difference between String, StringBuilder, StringBuffer?**
- See comparison table above

**Q: Why is String immutable?**
- Security, thread-safety, string pool optimization

**Q: String pool?**
- Memory area for string literals, reuses existing strings

**Q: What is regex?**
- Pattern matching for text validation and extraction

**Q: Common regex in testing?**
- Email, phone, URL validation, error extraction

---

*File completes with sections 15-17 in next part...*
## 15. File I/O Operations

### A. File Class

**File** = Represents file or directory path

```java
import java.io.File;
import java.io.IOException;

public class FileDemo {
    public static void main(String[] args) {
        // Creating File object
        File file = new File("test.txt");

        // File properties
        System.out.println("Name: " + file.getName());
        System.out.println("Path: " + file.getPath());
        System.out.println("Absolute path: " + file.getAbsolutePath());
        System.out.println("Parent: " + file.getParent());
        System.out.println("Exists: " + file.exists());
        System.out.println("Is file: " + file.isFile());
        System.out.println("Is directory: " + file.isDirectory());
        System.out.println("Can read: " + file.canRead());
        System.out.println("Can write: " + file.canWrite());
        System.out.println("File size: " + file.length() + " bytes");

        // Create new file
        try {
            if (file.createNewFile()) {
                System.out.println("\nFile created: " + file.getName());
            } else {
                System.out.println("\nFile already exists");
            }
        } catch (IOException e) {
            e.printStackTrace();
        }

        // Create directory
        File dir = new File("test_reports");
        if (dir.mkdir()) {
            System.out.println("Directory created: " + dir.getName());
        }

        // List files in directory
        File currentDir = new File(".");
        String[] files = currentDir.list();
        System.out.println("\nFiles in current directory:");
        if (files != null) {
            for (String filename : files) {
                System.out.println("- " + filename);
            }
        }

        // Delete file
        // if (file.delete()) {
        //     System.out.println("\nFile deleted");
        // }
    }
}
```

### B. Reading Files

**1. FileReader with BufferedReader (Character streams):**

```java
import java.io.*;

public class FileReadingDemo {
    public static void main(String[] args) {
        // Method 1: BufferedReader (recommended for text files)
        try (BufferedReader reader = new BufferedReader(new FileReader("test.txt"))) {
            String line;
            System.out.println("Reading file:");
            while ((line = reader.readLine()) != null) {
                System.out.println(line);
            }
        } catch (FileNotFoundException e) {
            System.out.println("File not found: " + e.getMessage());
        } catch (IOException e) {
            System.out.println("Error reading file: " + e.getMessage());
        }

        // Method 2: Scanner
        try (Scanner scanner = new Scanner(new File("test.txt"))) {
            System.out.println("\nReading with Scanner:");
            while (scanner.hasNextLine()) {
                System.out.println(scanner.nextLine());
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }

        // Method 3: Files.readAllLines (Java 7+)
        try {
            List<String> lines = Files.readAllLines(Paths.get("test.txt"));
            System.out.println("\nReading with Files.readAllLines:");
            for (String line : lines) {
                System.out.println(line);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### C. Writing Files

```java
import java.io.*;
import java.nio.file.*;
import java.util.*;

public class FileWritingDemo {
    public static void main(String[] args) {
        // Method 1: BufferedWriter
        try (BufferedWriter writer = new BufferedWriter(new FileWriter("output.txt"))) {
            writer.write("Line 1: Selenium WebDriver");
            writer.newLine();
            writer.write("Line 2: Test Automation");
            writer.newLine();
            writer.write("Line 3: Java Programming");
            System.out.println("File written successfully");
        } catch (IOException e) {
            e.printStackTrace();
        }

        // Method 2: FileWriter (append mode)
        try (FileWriter writer = new FileWriter("output.txt", true)) {
            writer.write("\nLine 4: Appended text");
            System.out.println("Text appended successfully");
        } catch (IOException e) {
            e.printStackTrace();
        }

        // Method 3: Files.write (Java 7+)
        List<String> lines = Arrays.asList(
            "Test Case 1: Login",
            "Test Case 2: Logout",
            "Test Case 3: Search"
        );

        try {
            Files.write(Paths.get("testcases.txt"), lines);
            System.out.println("File written using Files.write");
        } catch (IOException e) {
            e.printStackTrace();
        }

        // Method 4: PrintWriter
        try (PrintWriter writer = new PrintWriter("report.txt")) {
            writer.println("===== TEST REPORT =====");
            writer.printf("Total Tests: %d%n", 10);
            writer.printf("Passed: %d%n", 8);
            writer.printf("Failed: %d%n", 2);
            writer.println("======================");
            System.out.println("Report written successfully");
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```

### D. Properties Files

**Properties file** = Key-value pairs for configuration

**config.properties:**
```properties
browser=Chrome
url=https://example.com
timeout=30
username=admin
password=admin123
environment=QA
```

**Reading Properties:**

```java
import java.io.*;
import java.util.Properties;

public class PropertiesDemo {
    public static void main(String[] args) {
        Properties props = new Properties();

        // Load properties from file
        try (FileInputStream fis = new FileInputStream("config.properties")) {
            props.load(fis);

            // Read properties
            String browser = props.getProperty("browser");
            String url = props.getProperty("url");
            String timeout = props.getProperty("timeout");

            System.out.println("Browser: " + browser);
            System.out.println("URL: " + url);
            System.out.println("Timeout: " + timeout);

            // Read with default value
            String headless = props.getProperty("headless", "false");
            System.out.println("Headless: " + headless);

            // List all properties
            System.out.println("\nAll properties:");
            props.forEach((key, value) -> System.out.println(key + " = " + value));

        } catch (IOException e) {
            e.printStackTrace();
        }

        // Write properties to file
        Properties newProps = new Properties();
        newProps.setProperty("browser", "Firefox");
        newProps.setProperty("url", "https://newsite.com");
        newProps.setProperty("timeout", "60");

        try (FileOutputStream fos = new FileOutputStream("new_config.properties")) {
            newProps.store(fos, "Test Configuration");
            System.out.println("\nProperties written to file");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### E. Testing Framework Examples

**1. Test Data Management:**

```java
import java.io.*;
import java.util.*;

public class TestDataManager {
    private Map<String, String> testData = new HashMap<>();

    public void loadTestData(String filename) {
        try (BufferedReader reader = new BufferedReader(new FileReader(filename))) {
            String line;
            while ((line = reader.readLine()) != null) {
                if (line.trim().isEmpty() || line.startsWith("#")) {
                    continue; // Skip empty lines and comments
                }

                String[] parts = line.split("=", 2);
                if (parts.length == 2) {
                    testData.put(parts[0].trim(), parts[1].trim());
                }
            }
            System.out.println("Test data loaded: " + testData.size() + " entries");
        } catch (IOException e) {
            System.out.println("Error loading test data: " + e.getMessage());
        }
    }

    public String getData(String key) {
        return testData.get(key);
    }

    public void displayData() {
        System.out.println("===== TEST DATA =====");
        testData.forEach((key, value) -> System.out.println(key + " = " + value));
    }

    public static void main(String[] args) {
        TestDataManager manager = new TestDataManager();
        manager.loadTestData("testdata.txt");
        manager.displayData();

        System.out.println("\nUsername: " + manager.getData("username"));
        System.out.println("Password: " + manager.getData("password"));
    }
}
```

**2. Test Report Generator:**

```java
import java.io.*;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import java.util.*;

public class TestReportGenerator {
    private String reportFile;
    private StringBuilder report;

    public TestReportGenerator(String reportFile) {
        this.reportFile = reportFile;
        this.report = new StringBuilder();
    }

    public void startReport() {
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
        String timestamp = LocalDateTime.now().format(formatter);

        report.append("=====================================\n");
        report.append("     TEST EXECUTION REPORT\n");
        report.append("=====================================\n");
        report.append("Execution Time: ").append(timestamp).append("\n");
        report.append("=====================================\n\n");
    }

    public void addTestResult(String testName, boolean passed, String duration) {
        String status = passed ? "PASSED" : "FAILED";
        report.append(String.format("%-30s | %-10s | %s%n", testName, status, duration));
    }

    public void addSummary(int total, int passed, int failed, int skipped) {
        report.append("\n=====================================\n");
        report.append("              SUMMARY\n");
        report.append("=====================================\n");
        report.append("Total Tests:   ").append(total).append("\n");
        report.append("Passed:        ").append(passed).append("\n");
        report.append("Failed:        ").append(failed).append("\n");
        report.append("Skipped:       ").append(skipped).append("\n");

        double passRate = (passed * 100.0) / (total - skipped);
        report.append("Pass Rate:     ").append(String.format("%.2f%%", passRate)).append("\n");
        report.append("=====================================\n");
    }

    public void saveReport() {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter(reportFile))) {
            writer.write(report.toString());
            System.out.println("Report saved: " + reportFile);
        } catch (IOException e) {
            System.out.println("Error saving report: " + e.getMessage());
        }
    }

    public static void main(String[] args) {
        TestReportGenerator generator = new TestReportGenerator("test_report.txt");

        generator.startReport();

        generator.addTestResult("LoginTest", true, "2.5s");
        generator.addTestResult("LogoutTest", true, "1.2s");
        generator.addTestResult("SearchTest", false, "3.8s");
        generator.addTestResult("CheckoutTest", true, "4.5s");
        generator.addTestResult("PaymentTest", false, "2.1s");

        generator.addSummary(5, 3, 2, 0);
        generator.saveReport();
    }
}
```

**3. Screenshot Management:**

```java
import java.io.*;
import java.nio.file.*;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

public class ScreenshotManager {
    private String screenshotDir;

    public ScreenshotManager(String screenshotDir) {
        this.screenshotDir = screenshotDir;
        createDirectoryIfNotExists();
    }

    private void createDirectoryIfNotExists() {
        File dir = new File(screenshotDir);
        if (!dir.exists()) {
            if (dir.mkdirs()) {
                System.out.println("Screenshot directory created: " + screenshotDir);
            }
        }
    }

    public String generateScreenshotName(String testName) {
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyyMMdd_HHmmss");
        String timestamp = LocalDateTime.now().format(formatter);
        return testName + "_" + timestamp + ".png";
    }

    public String getScreenshotPath(String testName) {
        String filename = generateScreenshotName(testName);
        return screenshotDir + File.separator + filename;
    }

    public void saveScreenshot(String testName, byte[] screenshotData) {
        String filepath = getScreenshotPath(testName);

        try {
            Files.write(Paths.get(filepath), screenshotData);
            System.out.println("Screenshot saved: " + filepath);
        } catch (IOException e) {
            System.out.println("Error saving screenshot: " + e.getMessage());
        }
    }

    public void listScreenshots() {
        File dir = new File(screenshotDir);
        File[] files = dir.listFiles((d, name) -> name.endsWith(".png"));

        if (files != null && files.length > 0) {
            System.out.println("\nScreenshots:");
            for (File file : files) {
                System.out.println("- " + file.getName() + " (" + file.length() + " bytes)");
            }
        } else {
            System.out.println("No screenshots found");
        }
    }

    public static void main(String[] args) {
        ScreenshotManager manager = new ScreenshotManager("screenshots");

        System.out.println("Screenshot path: " + manager.getScreenshotPath("LoginTest"));
        System.out.println("Screenshot path: " + manager.getScreenshotPath("SearchTest"));

        manager.listScreenshots();
    }
}
```

### Common Mistakes to Avoid

1. **Not closing resources:**
```java
// Bad
FileReader reader = new FileReader("file.txt");
// Forgot to close - resource leak!

// Good - try-with-resources
try (FileReader reader = new FileReader("file.txt")) {
    // Automatically closed
}
```

2. **Not handling exceptions:**
```java
// Bad
File file = new File("test.txt");
file.createNewFile(); // Compilation error! Must handle IOException

// Good
try {
    file.createNewFile();
} catch (IOException e) {
    e.printStackTrace();
}
```

3. **Using absolute paths:**
```java
// Bad - not portable
File file = new File("C:\\Users\\Test\\file.txt");

// Good - relative path
File file = new File("testdata/file.txt");
```

### Interview Tips

**Q: Difference between FileReader and BufferedReader?**
- FileReader: Reads one character at a time
- BufferedReader: Buffers input, reads line by line, more efficient

**Q: How to read properties file?**
- Use Properties class with FileInputStream

**Q: How to handle file not found exception?**
- try-catch with FileNotFoundException

**Q: Best practice for file operations?**
- Use try-with-resources for automatic closing

---

## 16. Lambda Expressions and Streams API (Java 8+)

### A. Lambda Expressions

**Lambda** = Anonymous function (function without name)

**Syntax:**
```
(parameters) -> expression
(parameters) -> { statements; }
```

**Before Lambda (Java 7):**

```java
// Traditional way using anonymous class
List<String> browsers = Arrays.asList("Chrome", "Firefox", "Safari", "Edge");

Collections.sort(browsers, new Comparator<String>() {
    @Override
    public int compare(String b1, String b2) {
        return b1.compareTo(b2);
    }
});
```

**With Lambda (Java 8+):**

```java
// Lambda expression
List<String> browsers = Arrays.asList("Chrome", "Firefox", "Safari", "Edge");

Collections.sort(browsers, (b1, b2) -> b1.compareTo(b2));
// Or even simpler
Collections.sort(browsers, String::compareTo);
```

**Lambda Examples:**

```java
import java.util.*;
import java.util.function.*;

public class LambdaDemo {
    public static void main(String[] args) {
        // 1. No parameters
        Runnable r = () -> System.out.println("Running test");
        r.run();

        // 2. Single parameter
        Consumer<String> print = s -> System.out.println(s);
        print.accept("Hello Lambda");

        // 3. Multiple parameters
        BiFunction<Integer, Integer, Integer> add = (a, b) -> a + b;
        System.out.println("Sum: " + add.apply(10, 20));

        // 4. With code block
        BiFunction<Integer, Integer, Integer> multiply = (a, b) -> {
            int result = a * b;
            return result;
        };
        System.out.println("Product: " + multiply.apply(5, 6));

        // 5. Predicate (boolean-valued function)
        Predicate<String> isLong = s -> s.length() > 5;
        System.out.println("Chrome is long: " + isLong.test("Chrome"));
        System.out.println("IE is long: " + isLong.test("IE"));

        // 6. Function (transforms input to output)
        Function<String, Integer> getLength = s -> s.length();
        System.out.println("Length: " + getLength.apply("Selenium"));
    }
}
```

**Testing Examples:**

```java
import java.util.*;
import java.util.function.*;

public class LambdaTestingExamples {
    public static void main(String[] args) {
        List<String> testCases = Arrays.asList(
            "LoginTest", "LogoutTest", "SearchTest",
            "DisabledTest", "CheckoutTest", "PaymentTest"
        );

        // Filter enabled tests (remove disabled)
        System.out.println("All tests: " + testCases);

        List<String> enabledTests = new ArrayList<>();
        testCases.forEach(test -> {
            if (!test.contains("Disabled")) {
                enabledTests.add(test);
            }
        });

        System.out.println("Enabled tests: " + enabledTests);

        // Validate test names
        Predicate<String> isValidTestName = name ->
            name.endsWith("Test") && name.length() > 4;

        System.out.println("\nTest name validation:");
        testCases.forEach(test ->
            System.out.println(test + ": " + isValidTestName.test(test))
        );

        // Transform test names to uppercase
        Function<String, String> toUpper = String::toUpperCase;

        System.out.println("\nUppercase tests:");
        testCases.forEach(test -> System.out.println(toUpper.apply(test)));

        // Custom test executor
        Consumer<String> executeTest = testName -> {
            System.out.println("Executing: " + testName);
            System.out.println("Status: Passed");
            System.out.println("---");
        };

        System.out.println("\nExecuting tests:");
        enabledTests.forEach(executeTest);
    }
}
```

### B. Streams API

**Stream** = Sequence of elements supporting sequential and parallel operations

**Stream Operations:**
- **Intermediate**: filter, map, sorted (return Stream)
- **Terminal**: forEach, collect, reduce (return result)

```java
import java.util.*;
import java.util.stream.*;

public class StreamsDemo {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

        // 1. Filter - select elements
        List<Integer> evenNumbers = numbers.stream()
            .filter(n -> n % 2 == 0)
            .collect(Collectors.toList());
        System.out.println("Even numbers: " + evenNumbers);

        // 2. Map - transform elements
        List<Integer> squares = numbers.stream()
            .map(n -> n * n)
            .collect(Collectors.toList());
        System.out.println("Squares: " + squares);

        // 3. Sorted - sort elements
        List<Integer> sorted = numbers.stream()
            .sorted(Comparator.reverseOrder())
            .collect(Collectors.toList());
        System.out.println("Sorted (desc): " + sorted);

        // 4. Distinct - remove duplicates
        List<Integer> duplicates = Arrays.asList(1, 2, 2, 3, 3, 3, 4, 4, 5);
        List<Integer> unique = duplicates.stream()
            .distinct()
            .collect(Collectors.toList());
        System.out.println("Unique: " + unique);

        // 5. Limit - take first n elements
        List<Integer> first5 = numbers.stream()
            .limit(5)
            .collect(Collectors.toList());
        System.out.println("First 5: " + first5);

        // 6. Skip - skip first n elements
        List<Integer> afterSkip = numbers.stream()
            .skip(5)
            .collect(Collectors.toList());
        System.out.println("After skip 5: " + afterSkip);

        // 7. Count
        long count = numbers.stream()
            .filter(n -> n > 5)
            .count();
        System.out.println("Count > 5: " + count);

        // 8. AnyMatch, AllMatch, NoneMatch
        boolean anyMatch = numbers.stream().anyMatch(n -> n > 5);
        boolean allMatch = numbers.stream().allMatch(n -> n > 0);
        boolean noneMatch = numbers.stream().noneMatch(n -> n < 0);

        System.out.println("Any > 5: " + anyMatch);
        System.out.println("All > 0: " + allMatch);
        System.out.println("None < 0: " + noneMatch);

        // 9. FindFirst, FindAny
        Optional<Integer> first = numbers.stream()
            .filter(n -> n > 5)
            .findFirst();
        System.out.println("First > 5: " + first.orElse(-1));

        // 10. Reduce
        int sum = numbers.stream()
            .reduce(0, (a, b) -> a + b);
        System.out.println("Sum: " + sum);

        int max = numbers.stream()
            .reduce(Integer.MIN_VALUE, Integer::max);
        System.out.println("Max: " + max);

        // 11. Chaining operations
        List<Integer> result = numbers.stream()
            .filter(n -> n % 2 == 0)
            .map(n -> n * 2)
            .sorted()
            .collect(Collectors.toList());
        System.out.println("Chained result: " + result);
    }
}
```

**Testing Examples:**

```java
import java.util.*;
import java.util.stream.*;

class TestResult {
    private String name;
    private boolean passed;
    private int duration; // seconds

    public TestResult(String name, boolean passed, int duration) {
        this.name = name;
        this.passed = passed;
        this.duration = duration;
    }

    public String getName() { return name; }
    public boolean isPassed() { return passed; }
    public int getDuration() { return duration; }

    @Override
    public String toString() {
        return name + " (" + (passed ? "PASSED" : "FAILED") + ", " + duration + "s)";
    }
}

public class StreamsTestingExamples {
    public static void main(String[] args) {
        List<TestResult> results = Arrays.asList(
            new TestResult("LoginTest", true, 5),
            new TestResult("LogoutTest", true, 2),
            new TestResult("SearchTest", false, 8),
            new TestResult("CheckoutTest", true, 12),
            new TestResult("PaymentTest", false, 6),
            new TestResult("ProfileTest", true, 4)
        );

        System.out.println("All results: " + results);

        // 1. Filter passed tests
        List<TestResult> passedTests = results.stream()
            .filter(TestResult::isPassed)
            .collect(Collectors.toList());
        System.out.println("\nPassed tests: " + passedTests);

        // 2. Get test names only
        List<String> testNames = results.stream()
            .map(TestResult::getName)
            .collect(Collectors.toList());
        System.out.println("\nTest names: " + testNames);

        // 3. Failed test names
        List<String> failedTests = results.stream()
            .filter(t -> !t.isPassed())
            .map(TestResult::getName)
            .collect(Collectors.toList());
        System.out.println("\nFailed tests: " + failedTests);

        // 4. Sort by duration
        List<TestResult> sortedByDuration = results.stream()
            .sorted(Comparator.comparingInt(TestResult::getDuration))
            .collect(Collectors.toList());
        System.out.println("\nSorted by duration: " + sortedByDuration);

        // 5. Count passed and failed
        long passedCount = results.stream().filter(TestResult::isPassed).count();
        long failedCount = results.stream().filter(t -> !t.isPassed()).count();
        System.out.println("\nPassed: " + passedCount + ", Failed: " + failedCount);

        // 6. Total duration
        int totalDuration = results.stream()
            .mapToInt(TestResult::getDuration)
            .sum();
        System.out.println("Total duration: " + totalDuration + "s");

        // 7. Average duration
        double avgDuration = results.stream()
            .mapToInt(TestResult::getDuration)
            .average()
            .orElse(0.0);
        System.out.println("Average duration: " + avgDuration + "s");

        // 8. Longest test
        Optional<TestResult> longest = results.stream()
            .max(Comparator.comparingInt(TestResult::getDuration));
        longest.ifPresent(t -> System.out.println("Longest test: " + t));

        // 9. All tests passed?
        boolean allPassed = results.stream().allMatch(TestResult::isPassed);
        System.out.println("All tests passed: " + allPassed);

        // 10. Any test > 10 seconds?
        boolean anyLong = results.stream().anyMatch(t -> t.getDuration() > 10);
        System.out.println("Any test > 10s: " + anyLong);

        // 11. Group by result
        Map<Boolean, List<TestResult>> groupedByResult = results.stream()
            .collect(Collectors.groupingBy(TestResult::isPassed));
        System.out.println("\nGrouped by result:");
        System.out.println("Passed: " + groupedByResult.get(true));
        System.out.println("Failed: " + groupedByResult.get(false));

        // 12. Partition by result
        Map<Boolean, List<TestResult>> partitioned = results.stream()
            .collect(Collectors.partitioningBy(TestResult::isPassed));
        System.out.println("\nPartitioned:");
        System.out.println("Passed: " + partitioned.get(true));
        System.out.println("Failed: " + partitioned.get(false));
    }
}
```

**Advanced Stream Operations:**

```java
import java.util.*;
import java.util.stream.*;

public class AdvancedStreamsDemo {
    public static void main(String[] args) {
        List<String> browsers = Arrays.asList("Chrome", "Firefox", "Safari", "Edge");

        // 1. Parallel stream (for performance)
        browsers.parallelStream()
            .forEach(b -> System.out.println("Testing on: " + b));

        // 2. FlatMap - flatten nested structures
        List<List<String>> nestedList = Arrays.asList(
            Arrays.asList("TC001", "TC002"),
            Arrays.asList("TC003", "TC004", "TC005"),
            Arrays.asList("TC006")
        );

        List<String> flatList = nestedList.stream()
            .flatMap(List::stream)
            .collect(Collectors.toList());
        System.out.println("\nFlattened: " + flatList);

        // 3. Collectors.joining
        String browserList = browsers.stream()
            .collect(Collectors.joining(", "));
        System.out.println("\nBrowser list: " + browserList);

        // 4. Collectors.toMap
        Map<String, Integer> browserLengths = browsers.stream()
            .collect(Collectors.toMap(
                b -> b,
                String::length
            ));
        System.out.println("\nBrowser lengths: " + browserLengths);

        // 5. Collectors.counting
        Map<Integer, Long> lengthCounts = browsers.stream()
            .collect(Collectors.groupingBy(
                String::length,
                Collectors.counting()
            ));
        System.out.println("\nLength counts: " + lengthCounts);

        // 6. Collectors.summarizingInt
        List<Integer> durations = Arrays.asList(5, 3, 8, 2, 6, 4);
        IntSummaryStatistics stats = durations.stream()
            .collect(Collectors.summarizingInt(Integer::intValue));

        System.out.println("\nDuration statistics:");
        System.out.println("Count: " + stats.getCount());
        System.out.println("Sum: " + stats.getSum());
        System.out.println("Min: " + stats.getMin());
        System.out.println("Max: " + stats.getMax());
        System.out.println("Average: " + stats.getAverage());
    }
}
```

### Common Mistakes to Avoid

1. **Reusing streams:**
```java
// Wrong - stream already operated upon
Stream<String> stream = list.stream();
stream.forEach(System.out::println);
stream.forEach(System.out::println); // ERROR!

// Correct - create new stream
list.stream().forEach(System.out::println);
list.stream().forEach(System.out::println); // OK
```

2. **Modifying source during stream operation:**
```java
// Wrong - concurrent modification
List<String> list = new ArrayList<>(Arrays.asList("A", "B", "C"));
list.stream().forEach(item -> list.remove(item)); // ERROR!
```

3. **Not understanding terminal vs intermediate operations:**
```java
// No output - missing terminal operation!
list.stream().filter(s -> s.length() > 3); // Does nothing!

// Correct - add terminal operation
list.stream().filter(s -> s.length() > 3).forEach(System.out::println);
```

### Interview Tips

**Q: What is lambda expression?**
- Anonymous function, enables functional programming

**Q: Benefits of lambda?**
- Concise code, easier to read, enables Streams API

**Q: What is Stream API?**
- Process collections declaratively, supports functional-style operations

**Q: Difference between Collection and Stream?**
- Collection: Stores data
- Stream: Processes data (doesn't store)

**Q: Parallel vs Sequential stream?**
- Sequential: Processes one at a time
- Parallel: Uses multiple threads, faster for large data

---

## 17. Best Practices for Test Automation

### A. Code Organization

**1. Page Object Model (POM):**

```java
// BasePage.java
public class BasePage {
    protected WebDriver driver;

    public BasePage(WebDriver driver) {
        this.driver = driver;
    }

    protected void click(By locator) {
        driver.findElement(locator).click();
    }

    protected void sendKeys(By locator, String text) {
        driver.findElement(locator).sendKeys(text);
    }

    protected String getText(By locator) {
        return driver.findElement(locator).getText();
    }
}

// LoginPage.java
public class LoginPage extends BasePage {
    private By usernameField = By.id("username");
    private By passwordField = By.id("password");
    private By loginButton = By.id("loginBtn");

    public LoginPage(WebDriver driver) {
        super(driver);
    }

    public void enterUsername(String username) {
        sendKeys(usernameField, username);
    }

    public void enterPassword(String password) {
        sendKeys(passwordField, password);
    }

    public void clickLogin() {
        click(loginButton);
    }

    public void login(String username, String password) {
        enterUsername(username);
        enterPassword(password);
        clickLogin();
    }
}
```

**2. Test Data Separation:**

```java
public class TestDataProvider {
    private static Properties props = new Properties();

    static {
        try {
            props.load(new FileInputStream("testdata.properties"));
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static String getUsername() {
        return props.getProperty("username");
    }

    public static String getPassword() {
        return props.getProperty("password");
    }

    public static String getUrl() {
        return props.getProperty("url");
    }
}
```

### B. Naming Conventions

```java
public class NamingBestPractices {
    // 1. Test Class: <Feature>Test
    public class LoginTest { }

    // 2. Test Method: test<Scenario>
    @Test
    public void testValidLogin() { }

    @Test
    public void testInvalidLogin() { }

    // 3. Page Class: <Page>Page
    public class HomePage { }

    // 4. Variables: descriptive, camelCase
    private String expectedTitle = "Dashboard";
    private int maxRetryCount = 3;

    // 5. Constants: UPPER_CASE
    private static final String BASE_URL = "https://example.com";
    private static final int DEFAULT_TIMEOUT = 30;

    // 6. Methods: verb + noun
    public void clickLoginButton() { }
    public String getPageTitle() { }
    public boolean isElementVisible() { }
}
```

### C. Error Handling

```java
public class ErrorHandlingBestPractices {
    // 1. Use specific exceptions
    public void findElement(String locator) {
        try {
            driver.findElement(By.xpath(locator));
        } catch (NoSuchElementException e) {
            System.out.println("Element not found: " + locator);
            takeScreenshot();
            throw e;
        } catch (TimeoutException e) {
            System.out.println("Timeout waiting for: " + locator);
            throw e;
        }
    }

    // 2. Custom exceptions
    public class ElementNotFoundException extends Exception {
        public ElementNotFoundException(String message) {
            super(message);
        }
    }

    // 3. Meaningful error messages
    public void validateTitle(String expected) {
        String actual = driver.getTitle();
        if (!actual.equals(expected)) {
            throw new AssertionError(
                "Title mismatch!\n" +
                "Expected: " + expected + "\n" +
                "Actual: " + actual
            );
        }
    }

    private void takeScreenshot() {
        System.out.println("Screenshot taken");
    }
}
```

### D. Wait Strategies

```java
public class WaitStrategies {
    private WebDriver driver;
    private WebDriverWait wait;

    public WaitStrategies(WebDriver driver) {
        this.driver = driver;
        this.wait = new WebDriverWait(driver, Duration.ofSeconds(30));
    }

    // 1. Explicit wait for element
    public WebElement waitForElement(By locator) {
        return wait.until(ExpectedConditions.presenceOfElementLocated(locator));
    }

    // 2. Wait for visibility
    public WebElement waitForVisibility(By locator) {
        return wait.until(ExpectedConditions.visibilityOfElementLocated(locator));
    }

    // 3. Wait for clickable
    public WebElement waitForClickable(By locator) {
        return wait.until(ExpectedConditions.elementToBeClickable(locator));
    }

    // 4. Wait for text
    public boolean waitForTextPresent(By locator, String text) {
        return wait.until(ExpectedConditions.textToBePresentInElementLocated(locator, text));
    }

    // 5. Custom wait condition
    public boolean waitForPageLoad() {
        return wait.until(driver -> 
            ((JavascriptExecutor) driver).executeScript("return document.readyState").equals("complete")
        );
    }
}
```

### E. Reusability

```java
public class ReusableUtils {
    // 1. Generic click method
    public static void click(WebDriver driver, By locator) {
        WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(30));
        WebElement element = wait.until(ExpectedConditions.elementToBeClickable(locator));
        element.click();
    }

    // 2. Generic sendKeys method
    public static void sendKeys(WebDriver driver, By locator, String text) {
        WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(30));
        WebElement element = wait.until(ExpectedConditions.visibilityOfElementLocated(locator));
        element.clear();
        element.sendKeys(text);
    }

    // 3. Screenshot utility
    public static void takeScreenshot(WebDriver driver, String filename) {
        try {
            TakesScreenshot ts = (TakesScreenshot) driver;
            byte[] screenshot = ts.getScreenshotAs(OutputType.BYTES);
            Files.write(Paths.get("screenshots/" + filename + ".png"), screenshot);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    // 4. Random data generator
    public static String generateRandomEmail() {
        return "test" + System.currentTimeMillis() + "@example.com";
    }

    public static String generateRandomString(int length) {
        String chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
        Random random = new Random();
        StringBuilder sb = new StringBuilder();

        for (int i = 0; i < length; i++) {
            sb.append(chars.charAt(random.nextInt(chars.length())));
        }

        return sb.toString();
    }
}
```

### F. Logging

```java
import org.apache.logging.log4j.*;

public class LoggingBestPractices {
    private static final Logger logger = LogManager.getLogger(LoggingBestPractices.class);

    public void executeTest() {
        logger.info("Starting test execution");

        try {
            logger.debug("Navigating to login page");
            // Navigation code

            logger.info("Entering credentials");
            // Login code

            logger.info("Test passed");
        } catch (Exception e) {
            logger.error("Test failed: " + e.getMessage(), e);
            throw e;
        } finally {
            logger.info("Test execution completed");
        }
    }
}
```

### G. Code Quality Tips

**1. Don't Repeat Yourself (DRY):**
```java
// Bad
public void testLogin1() {
    driver.get("https://example.com");
    driver.findElement(By.id("username")).sendKeys("admin");
    driver.findElement(By.id("password")).sendKeys("admin123");
    driver.findElement(By.id("loginBtn")).click();
}

public void testLogin2() {
    driver.get("https://example.com");
    driver.findElement(By.id("username")).sendKeys("user");
    driver.findElement(By.id("password")).sendKeys("user123");
    driver.findElement(By.id("loginBtn")).click();
}

// Good
public void login(String username, String password) {
    driver.get("https://example.com");
    driver.findElement(By.id("username")).sendKeys(username);
    driver.findElement(By.id("password")).sendKeys(password);
    driver.findElement(By.id("loginBtn")).click();
}

@Test
public void testAdminLogin() {
    login("admin", "admin123");
}

@Test
public void testUserLogin() {
    login("user", "user123");
}
```

**2. Single Responsibility:**
```java
// Bad - multiple responsibilities
public void testAndReport() {
    // Test execution
    driver.get(url);
    // Reporting
    generateReport();
    // Email notification
    sendEmail();
}

// Good - separated concerns
public void executeTest() {
    driver.get(url);
}

public void generateReport() {
    // Report logic
}

public void sendNotification() {
    // Email logic
}
```

### H. Summary Checklist

- Use Page Object Model for maintainability
- Separate test data from test code
- Use meaningful naming conventions
- Handle exceptions properly
- Implement proper wait strategies
- Create reusable utilities
- Add logging for debugging
- Follow DRY principle
- Keep methods small and focused
- Use version control (Git)
- Review code regularly
- Write clean, readable code
- Document complex logic
- Use assertions effectively
- Implement retry mechanism for flaky tests

---

## Conclusion

This comprehensive guide covered Java programming fundamentals essential for test automation engineers. Key takeaways:

**Core Concepts:**
- Object-Oriented Programming (Classes, Inheritance, Polymorphism)
- SOLID Principles for maintainable code
- Exception Handling for robust tests
- Collections Framework for data management

**Advanced Features:**
- Generics for type safety
- Streams API for efficient data processing
- Lambda Expressions for concise code
- File I/O for test data and reports

**Best Practices:**
- Page Object Model design pattern
- Proper error handling and logging
- Reusable utilities and methods
- Clean code principles

**Next Steps:**
1. Practice coding exercises
2. Build a sample automation framework
3. Learn Selenium WebDriver
4. Study TestNG/JUnit frameworks
5. Explore Maven/Gradle build tools
6. Master design patterns
7. Learn CI/CD integration

**Resources for Practice:**
- LeetCode for Java problems
- HackerRank for automation challenges
- GitHub for framework examples
- Udemy/Coursera for advanced courses

Remember: **Practice is key!** Build real-world projects and contribute to open-source automation frameworks.

---

**End of Phase 3-01: Java Programming Foundation - Theory**

---
