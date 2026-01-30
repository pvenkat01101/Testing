# Phase 6-09: Selenium Grid Practicals

## Table of Contents
- [Exercise 1: Setting Up Grid Locally (Standalone)](#exercise-1-setting-up-grid-locally-standalone)
- [Exercise 2: Hub-Node Grid Setup](#exercise-2-hub-node-grid-setup)
- [Exercise 3: Running Tests on Grid with RemoteWebDriver](#exercise-3-running-tests-on-grid-with-remotewebdriver)
- [Exercise 4: Parallel Execution with TestNG](#exercise-4-parallel-execution-with-testng)
- [Exercise 5: ThreadLocal Implementation](#exercise-5-threadlocal-implementation)
- [Exercise 6: Docker Grid Setup](#exercise-6-docker-grid-setup)
- [Exercise 7: BrowserStack Integration](#exercise-7-browserstack-integration)
- [Exercise 8: Complete Framework with Grid Support](#exercise-8-complete-framework-with-grid-support)

---

## Exercise 1: Setting Up Grid Locally (Standalone)

### Objective
Set up a Selenium Grid 4 Standalone server on your local machine and verify it is running.

### Prerequisites
- Java 11+ installed
- Selenium Server JAR downloaded

### Step 1: Download Selenium Server

```bash
# Download Selenium Server 4.x
# Visit: https://github.com/SeleniumHQ/selenium/releases
# Or use wget/curl:
curl -O https://github.com/SeleniumHQ/selenium/releases/download/selenium-4.18.1/selenium-server-4.18.1.jar
```

### Step 2: Start Standalone Grid

```bash
# Start in standalone mode (all components in one process)
java -jar selenium-server-4.18.1.jar standalone

# With custom port
java -jar selenium-server-4.18.1.jar standalone --port 4444

# With maximum sessions configured
java -jar selenium-server-4.18.1.jar standalone --max-sessions 6

# With specific log level
java -jar selenium-server-4.18.1.jar standalone --log-level FINE
```

### Step 3: Verify Grid is Running

Open your browser and navigate to:
```
http://localhost:4444/ui
```

You should see the Selenium Grid Console showing:
- Grid status (ready/not ready)
- Available browser types (auto-detected from your machine)
- Number of available sessions
- Node information

**Alternative: Check via API**
```bash
# Check Grid status via API
curl http://localhost:4444/status

# Expected response:
# {
#   "value": {
#     "ready": true,
#     "message": "Selenium Grid ready.",
#     "nodes": [...]
#   }
# }
```

### Step 4: Run a Quick Test

```java
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.openqa.selenium.remote.RemoteWebDriver;
import java.net.URL;

public class StandaloneGridTest {
    public static void main(String[] args) throws Exception {
        ChromeOptions options = new ChromeOptions();
        WebDriver driver = new RemoteWebDriver(
            new URL("http://localhost:4444/wd/hub"), options
        );

        driver.get("https://www.google.com");
        System.out.println("Title: " + driver.getTitle());
        System.out.println("Grid Standalone test PASSED!");

        driver.quit();
    }
}
```

---

## Exercise 2: Hub-Node Grid Setup

### Objective
Set up a Grid with a separate Hub and multiple Nodes.

### Step 1: Start the Hub

```bash
# Terminal 1 - Start Hub
java -jar selenium-server-4.18.1.jar hub --port 4444
```

**Expected output:**
```
[INFO] Started Selenium Hub 4.18.1: http://192.168.1.100:4444
```

### Step 2: Start Chrome Node

```bash
# Terminal 2 - Start Chrome Node
java -jar selenium-server-4.18.1.jar node \
  --hub http://localhost:4444 \
  --port 5555 \
  --max-sessions 3
```

### Step 3: Start Firefox Node

```bash
# Terminal 3 - Start Firefox Node
java -jar selenium-server-4.18.1.jar node \
  --hub http://localhost:4444 \
  --port 5556 \
  --max-sessions 3
```

### Step 4: Start Edge Node

```bash
# Terminal 4 - Start Edge Node
java -jar selenium-server-4.18.1.jar node \
  --hub http://localhost:4444 \
  --port 5557 \
  --max-sessions 2
```

### Step 5: Verify All Nodes Are Registered

```bash
# Check Grid status
curl http://localhost:4444/status | python3 -m json.tool
```

Visit `http://localhost:4444/ui` to see all registered nodes.

### Step 6: Configure Nodes with TOML File

Instead of command-line arguments, you can use a TOML configuration file:

```toml
# chrome-node-config.toml
[server]
port = 5555
max-sessions = 5

[node]
detect-drivers = false
max-sessions = 5

[[node.driver-configuration]]
display-name = "Chrome"
stereotype = '{"browserName": "chrome", "browserVersion": "120", "platformName": "linux"}'
max-sessions = 5
```

```bash
# Start Node with config file
java -jar selenium-server-4.18.1.jar node \
  --hub http://localhost:4444 \
  --config chrome-node-config.toml
```

---

## Exercise 3: Running Tests on Grid with RemoteWebDriver

### Objective
Write and execute tests that run on the Selenium Grid using RemoteWebDriver.

### Maven Dependencies

```xml
<!-- pom.xml -->
<dependencies>
    <dependency>
        <groupId>org.seleniumhq.selenium</groupId>
        <artifactId>selenium-java</artifactId>
        <version>4.18.1</version>
    </dependency>
    <dependency>
        <groupId>org.testng</groupId>
        <artifactId>testng</artifactId>
        <version>7.9.0</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

### Basic RemoteWebDriver Test

```java
package tests;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeOptions;
import org.openqa.selenium.firefox.FirefoxOptions;
import org.openqa.selenium.remote.RemoteWebDriver;
import org.testng.Assert;
import org.testng.annotations.*;
import java.net.MalformedURLException;
import java.net.URL;

public class GridBasicTest {

    private WebDriver driver;
    private static final String GRID_URL = "http://localhost:4444/wd/hub";

    @Parameters({"browser"})
    @BeforeMethod
    public void setup(@Optional("chrome") String browser) throws MalformedURLException {
        URL gridUrl = new URL(GRID_URL);

        switch (browser.toLowerCase()) {
            case "chrome":
                ChromeOptions chromeOptions = new ChromeOptions();
                chromeOptions.addArguments("--start-maximized");
                driver = new RemoteWebDriver(gridUrl, chromeOptions);
                break;

            case "firefox":
                FirefoxOptions firefoxOptions = new FirefoxOptions();
                driver = new RemoteWebDriver(gridUrl, firefoxOptions);
                break;

            default:
                throw new IllegalArgumentException("Unsupported browser: " + browser);
        }

        driver.manage().window().maximize();
    }

    @Test
    public void testGoogleSearch() {
        driver.get("https://www.google.com");
        Assert.assertTrue(driver.getTitle().contains("Google"),
            "Page title should contain 'Google'");
    }

    @Test
    public void testNavigateToSelenium() {
        driver.get("https://www.selenium.dev");
        Assert.assertTrue(driver.getTitle().toLowerCase().contains("selenium"),
            "Page title should contain 'Selenium'");
    }

    @AfterMethod
    public void teardown() {
        if (driver != null) {
            driver.quit();
        }
    }
}
```

### TestNG XML for Cross-Browser Testing

```xml
<!-- testng-grid.xml -->
<!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd">
<suite name="Grid Cross-Browser Suite" parallel="tests" thread-count="3">

    <test name="Chrome Tests">
        <parameter name="browser" value="chrome"/>
        <classes>
            <class name="tests.GridBasicTest"/>
        </classes>
    </test>

    <test name="Firefox Tests">
        <parameter name="browser" value="firefox"/>
        <classes>
            <class name="tests.GridBasicTest"/>
        </classes>
    </test>

    <test name="Edge Tests">
        <parameter name="browser" value="edge"/>
        <classes>
            <class name="tests.GridBasicTest"/>
        </classes>
    </test>

</suite>
```

### Running the Tests

```bash
# Run with Maven
mvn test -DsuiteXmlFile=testng-grid.xml

# Or run with specific browser
mvn test -Dbrowser=chrome
```

---

## Exercise 4: Parallel Execution with TestNG

### Objective
Execute tests in parallel using different TestNG parallelization strategies.

### Test Classes Setup

```java
// tests/LoginTest.java
package tests;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.openqa.selenium.remote.RemoteWebDriver;
import org.testng.Assert;
import org.testng.annotations.*;
import java.net.URL;

public class LoginTest {
    private WebDriver driver;

    @BeforeMethod
    public void setup() throws Exception {
        ChromeOptions options = new ChromeOptions();
        driver = new RemoteWebDriver(new URL("http://localhost:4444/wd/hub"), options);
        driver.manage().window().maximize();
    }

    @Test
    public void testValidLogin() {
        driver.get("https://the-internet.herokuapp.com/login");
        driver.findElement(By.id("username")).sendKeys("tomsmith");
        driver.findElement(By.id("password")).sendKeys("SuperSecretPassword!");
        driver.findElement(By.cssSelector("button[type='submit']")).click();

        String message = driver.findElement(By.id("flash")).getText();
        Assert.assertTrue(message.contains("You logged into a secure area!"));

        System.out.println("[LoginTest - testValidLogin] Thread: "
            + Thread.currentThread().getId());
    }

    @Test
    public void testInvalidLogin() {
        driver.get("https://the-internet.herokuapp.com/login");
        driver.findElement(By.id("username")).sendKeys("invaliduser");
        driver.findElement(By.id("password")).sendKeys("wrongpassword");
        driver.findElement(By.cssSelector("button[type='submit']")).click();

        String message = driver.findElement(By.id("flash")).getText();
        Assert.assertTrue(message.contains("Your username is invalid!"));

        System.out.println("[LoginTest - testInvalidLogin] Thread: "
            + Thread.currentThread().getId());
    }

    @AfterMethod
    public void teardown() {
        if (driver != null) driver.quit();
    }
}
```

```java
// tests/NavigationTest.java
package tests;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.openqa.selenium.remote.RemoteWebDriver;
import org.testng.Assert;
import org.testng.annotations.*;
import java.net.URL;

public class NavigationTest {
    private WebDriver driver;

    @BeforeMethod
    public void setup() throws Exception {
        ChromeOptions options = new ChromeOptions();
        driver = new RemoteWebDriver(new URL("http://localhost:4444/wd/hub"), options);
        driver.manage().window().maximize();
    }

    @Test
    public void testCheckboxPage() {
        driver.get("https://the-internet.herokuapp.com/checkboxes");
        Assert.assertTrue(driver.findElement(By.id("checkboxes")).isDisplayed());

        System.out.println("[NavigationTest - testCheckboxPage] Thread: "
            + Thread.currentThread().getId());
    }

    @Test
    public void testDropdownPage() {
        driver.get("https://the-internet.herokuapp.com/dropdown");
        Assert.assertTrue(driver.findElement(By.id("dropdown")).isDisplayed());

        System.out.println("[NavigationTest - testDropdownPage] Thread: "
            + Thread.currentThread().getId());
    }

    @AfterMethod
    public void teardown() {
        if (driver != null) driver.quit();
    }
}
```

### Parallel at Method Level

```xml
<!-- testng-parallel-methods.xml -->
<!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd">
<suite name="Parallel Methods Suite" parallel="methods" thread-count="4">
    <test name="All Tests">
        <classes>
            <class name="tests.LoginTest"/>
            <class name="tests.NavigationTest"/>
        </classes>
    </test>
</suite>
```

**Behavior:** Each `@Test` method runs in its own thread. With `thread-count="4"`, up to 4 test methods run simultaneously.

```
Thread 14: LoginTest.testValidLogin()
Thread 15: LoginTest.testInvalidLogin()
Thread 16: NavigationTest.testCheckboxPage()
Thread 17: NavigationTest.testDropdownPage()
```

### Parallel at Class Level

```xml
<!-- testng-parallel-classes.xml -->
<!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd">
<suite name="Parallel Classes Suite" parallel="classes" thread-count="2">
    <test name="All Tests">
        <classes>
            <class name="tests.LoginTest"/>
            <class name="tests.NavigationTest"/>
        </classes>
    </test>
</suite>
```

**Behavior:** Each test class runs in its own thread. Methods within a class run sequentially.

```
Thread 14: LoginTest.testValidLogin() → LoginTest.testInvalidLogin()
Thread 15: NavigationTest.testCheckboxPage() → NavigationTest.testDropdownPage()
```

### Parallel at Test Level (Cross-Browser)

```xml
<!-- testng-parallel-tests.xml -->
<!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd">
<suite name="Cross Browser Suite" parallel="tests" thread-count="3">

    <test name="Chrome Tests">
        <parameter name="browser" value="chrome"/>
        <classes>
            <class name="tests.LoginTest"/>
            <class name="tests.NavigationTest"/>
        </classes>
    </test>

    <test name="Firefox Tests">
        <parameter name="browser" value="firefox"/>
        <classes>
            <class name="tests.LoginTest"/>
            <class name="tests.NavigationTest"/>
        </classes>
    </test>

    <test name="Edge Tests">
        <parameter name="browser" value="edge"/>
        <classes>
            <class name="tests.LoginTest"/>
            <class name="tests.NavigationTest"/>
        </classes>
    </test>

</suite>
```

**Behavior:** Each `<test>` tag runs in its own thread. All classes within a `<test>` run in the same thread.

---

## Exercise 5: ThreadLocal Implementation

### Objective
Implement a thread-safe driver management system using ThreadLocal for parallel execution.

### Step 1: DriverManager with ThreadLocal

```java
// driver/DriverManager.java
package driver;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.openqa.selenium.edge.EdgeOptions;
import org.openqa.selenium.firefox.FirefoxOptions;
import org.openqa.selenium.remote.RemoteWebDriver;
import java.net.MalformedURLException;
import java.net.URL;
import java.time.Duration;

public class DriverManager {

    // ThreadLocal ensures each thread has its own WebDriver instance
    private static final ThreadLocal<WebDriver> driverThreadLocal = new ThreadLocal<>();

    private static final String GRID_URL = "http://localhost:4444/wd/hub";

    /**
     * Get the WebDriver for the current thread
     */
    public static WebDriver getDriver() {
        return driverThreadLocal.get();
    }

    /**
     * Initialize WebDriver for the current thread
     */
    public static void initDriver(String browser) {
        if (driverThreadLocal.get() != null) {
            System.out.println("WARNING: Driver already exists for thread "
                + Thread.currentThread().getId() + ". Quitting existing driver.");
            quitDriver();
        }

        WebDriver driver;
        try {
            URL gridUrl = new URL(GRID_URL);

            switch (browser.toLowerCase()) {
                case "chrome":
                    ChromeOptions chromeOptions = new ChromeOptions();
                    chromeOptions.addArguments("--start-maximized");
                    chromeOptions.addArguments("--disable-notifications");
                    driver = new RemoteWebDriver(gridUrl, chromeOptions);
                    break;

                case "firefox":
                    FirefoxOptions firefoxOptions = new FirefoxOptions();
                    driver = new RemoteWebDriver(gridUrl, firefoxOptions);
                    break;

                case "edge":
                    EdgeOptions edgeOptions = new EdgeOptions();
                    edgeOptions.addArguments("--start-maximized");
                    driver = new RemoteWebDriver(gridUrl, edgeOptions);
                    break;

                default:
                    throw new IllegalArgumentException("Unsupported browser: " + browser);
            }

            driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(10));
            driver.manage().timeouts().pageLoadTimeout(Duration.ofSeconds(30));
            driver.manage().window().maximize();

            driverThreadLocal.set(driver);

            System.out.println("Driver initialized for thread "
                + Thread.currentThread().getId()
                + " - Browser: " + browser);

        } catch (MalformedURLException e) {
            throw new RuntimeException("Invalid Grid URL: " + GRID_URL, e);
        }
    }

    /**
     * Quit the WebDriver for the current thread and remove from ThreadLocal
     */
    public static void quitDriver() {
        WebDriver driver = driverThreadLocal.get();
        if (driver != null) {
            try {
                driver.quit();
                System.out.println("Driver quit for thread "
                    + Thread.currentThread().getId());
            } catch (Exception e) {
                System.err.println("Error quitting driver: " + e.getMessage());
            } finally {
                driverThreadLocal.remove(); // CRITICAL: Prevent memory leaks
            }
        }
    }
}
```

### Step 2: Base Test Class

```java
// tests/BaseTest.java
package tests;

import driver.DriverManager;
import org.openqa.selenium.WebDriver;
import org.testng.annotations.*;

public class BaseTest {

    @Parameters({"browser"})
    @BeforeMethod
    public void setUp(@Optional("chrome") String browser) {
        DriverManager.initDriver(browser);
    }

    @AfterMethod
    public void tearDown() {
        DriverManager.quitDriver();
    }

    /**
     * Convenience method to get driver in test classes
     */
    protected WebDriver getDriver() {
        return DriverManager.getDriver();
    }
}
```

### Step 3: Test Classes Using ThreadLocal Driver

```java
// tests/ThreadSafeLoginTest.java
package tests;

import org.openqa.selenium.By;
import org.testng.Assert;
import org.testng.annotations.Test;

public class ThreadSafeLoginTest extends BaseTest {

    @Test
    public void testValidLogin() {
        getDriver().get("https://the-internet.herokuapp.com/login");

        getDriver().findElement(By.id("username")).sendKeys("tomsmith");
        getDriver().findElement(By.id("password")).sendKeys("SuperSecretPassword!");
        getDriver().findElement(By.cssSelector("button[type='submit']")).click();

        String flash = getDriver().findElement(By.id("flash")).getText();
        Assert.assertTrue(flash.contains("You logged into a secure area!"),
            "Success message should be displayed");

        logThreadInfo("testValidLogin");
    }

    @Test
    public void testInvalidLogin() {
        getDriver().get("https://the-internet.herokuapp.com/login");

        getDriver().findElement(By.id("username")).sendKeys("wrong");
        getDriver().findElement(By.id("password")).sendKeys("wrong");
        getDriver().findElement(By.cssSelector("button[type='submit']")).click();

        String flash = getDriver().findElement(By.id("flash")).getText();
        Assert.assertTrue(flash.contains("Your username is invalid!"),
            "Error message should be displayed");

        logThreadInfo("testInvalidLogin");
    }

    private void logThreadInfo(String testName) {
        System.out.println(String.format(
            "[%s] Thread ID: %d | Session: %s",
            testName,
            Thread.currentThread().getId(),
            ((org.openqa.selenium.remote.RemoteWebDriver) getDriver())
                .getSessionId()
        ));
    }
}
```

```java
// tests/ThreadSafeNavigationTest.java
package tests;

import org.openqa.selenium.By;
import org.openqa.selenium.support.ui.Select;
import org.testng.Assert;
import org.testng.annotations.Test;

public class ThreadSafeNavigationTest extends BaseTest {

    @Test
    public void testDropdown() {
        getDriver().get("https://the-internet.herokuapp.com/dropdown");

        Select dropdown = new Select(getDriver().findElement(By.id("dropdown")));
        dropdown.selectByVisibleText("Option 1");

        Assert.assertEquals(dropdown.getFirstSelectedOption().getText(), "Option 1");

        System.out.println("[testDropdown] Thread: " + Thread.currentThread().getId());
    }

    @Test
    public void testCheckboxes() {
        getDriver().get("https://the-internet.herokuapp.com/checkboxes");

        var checkboxes = getDriver().findElements(By.cssSelector("#checkboxes input"));
        Assert.assertEquals(checkboxes.size(), 2, "Should have 2 checkboxes");

        System.out.println("[testCheckboxes] Thread: " + Thread.currentThread().getId());
    }

    @Test
    public void testHovers() {
        getDriver().get("https://the-internet.herokuapp.com/hovers");

        var figures = getDriver().findElements(By.cssSelector(".figure"));
        Assert.assertEquals(figures.size(), 3, "Should have 3 figure elements");

        System.out.println("[testHovers] Thread: " + Thread.currentThread().getId());
    }
}
```

### Step 4: TestNG XML for Parallel Execution

```xml
<!-- testng-threadlocal.xml -->
<!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd">
<suite name="ThreadLocal Parallel Suite" parallel="methods" thread-count="5">

    <test name="All Tests - Chrome">
        <parameter name="browser" value="chrome"/>
        <classes>
            <class name="tests.ThreadSafeLoginTest"/>
            <class name="tests.ThreadSafeNavigationTest"/>
        </classes>
    </test>

</suite>
```

```xml
<!-- testng-threadlocal-crossbrowser.xml -->
<!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd">
<suite name="ThreadLocal Cross-Browser Suite" parallel="tests" thread-count="3">

    <test name="Chrome">
        <parameter name="browser" value="chrome"/>
        <classes>
            <class name="tests.ThreadSafeLoginTest"/>
            <class name="tests.ThreadSafeNavigationTest"/>
        </classes>
    </test>

    <test name="Firefox">
        <parameter name="browser" value="firefox"/>
        <classes>
            <class name="tests.ThreadSafeLoginTest"/>
            <class name="tests.ThreadSafeNavigationTest"/>
        </classes>
    </test>

    <test name="Edge">
        <parameter name="browser" value="edge"/>
        <classes>
            <class name="tests.ThreadSafeLoginTest"/>
            <class name="tests.ThreadSafeNavigationTest"/>
        </classes>
    </test>

</suite>
```

### Expected Output (Parallel Methods)

```
Driver initialized for thread 14 - Browser: chrome
Driver initialized for thread 15 - Browser: chrome
Driver initialized for thread 16 - Browser: chrome
Driver initialized for thread 17 - Browser: chrome
Driver initialized for thread 18 - Browser: chrome

[testValidLogin] Thread ID: 14 | Session: abc123
[testDropdown] Thread: 15
[testCheckboxes] Thread: 16
[testInvalidLogin] Thread ID: 17 | Session: def456
[testHovers] Thread: 18

Driver quit for thread 14
Driver quit for thread 15
Driver quit for thread 16
Driver quit for thread 17
Driver quit for thread 18
```

---

## Exercise 6: Docker Grid Setup

### Objective
Set up a Selenium Grid using Docker and docker-compose.

### Step 1: Basic docker-compose.yml

```yaml
# docker-compose.yml
version: "3.8"

services:
  # Selenium Hub
  selenium-hub:
    image: selenium/hub:4.18.1
    container_name: selenium-hub
    ports:
      - "4442:4442"
      - "4443:4443"
      - "4444:4444"
    environment:
      - GRID_MAX_SESSION=10
      - SE_SESSION_REQUEST_TIMEOUT=300
      - SE_SESSION_RETRY_INTERVAL=2
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:4444/status"]
      interval: 10s
      timeout: 5s
      retries: 5

  # Chrome Node
  chrome-node:
    image: selenium/node-chrome:4.18.1
    depends_on:
      selenium-hub:
        condition: service_healthy
    environment:
      - SE_EVENT_BUS_HOST=selenium-hub
      - SE_EVENT_BUS_PUBLISH_PORT=4442
      - SE_EVENT_BUS_SUBSCRIBE_PORT=4443
      - SE_NODE_MAX_SESSIONS=3
      - SE_NODE_OVERRIDE_MAX_SESSIONS=true
      - SE_NODE_SESSION_TIMEOUT=300
    shm_size: "2gb"
    deploy:
      replicas: 2  # Start 2 Chrome nodes

  # Firefox Node
  firefox-node:
    image: selenium/node-firefox:4.18.1
    depends_on:
      selenium-hub:
        condition: service_healthy
    environment:
      - SE_EVENT_BUS_HOST=selenium-hub
      - SE_EVENT_BUS_PUBLISH_PORT=4442
      - SE_EVENT_BUS_SUBSCRIBE_PORT=4443
      - SE_NODE_MAX_SESSIONS=3
      - SE_NODE_OVERRIDE_MAX_SESSIONS=true
      - SE_NODE_SESSION_TIMEOUT=300
    shm_size: "2gb"

  # Edge Node
  edge-node:
    image: selenium/node-edge:4.18.1
    depends_on:
      selenium-hub:
        condition: service_healthy
    environment:
      - SE_EVENT_BUS_HOST=selenium-hub
      - SE_EVENT_BUS_PUBLISH_PORT=4442
      - SE_EVENT_BUS_SUBSCRIBE_PORT=4443
      - SE_NODE_MAX_SESSIONS=2
      - SE_NODE_OVERRIDE_MAX_SESSIONS=true
    shm_size: "2gb"
```

### Step 2: Start the Docker Grid

```bash
# Start all services
docker-compose up -d

# Verify services are running
docker-compose ps

# Check Hub logs
docker-compose logs selenium-hub

# Check that nodes registered successfully
docker-compose logs chrome-node
docker-compose logs firefox-node

# Open Grid Console
# http://localhost:4444/ui
```

### Step 3: Scale Nodes

```bash
# Scale Chrome nodes to 5
docker-compose up -d --scale chrome-node=5

# Scale Firefox nodes to 3
docker-compose up -d --scale firefox-node=3

# Verify scaling
docker-compose ps
```

### Step 4: Docker Compose with VNC Access (for Debugging)

```yaml
# docker-compose-debug.yml
version: "3.8"

services:
  selenium-hub:
    image: selenium/hub:4.18.1
    container_name: selenium-hub
    ports:
      - "4442:4442"
      - "4443:4443"
      - "4444:4444"

  chrome-node:
    image: selenium/node-chrome:4.18.1
    depends_on:
      - selenium-hub
    ports:
      - "7900:7900"   # noVNC web interface
    environment:
      - SE_EVENT_BUS_HOST=selenium-hub
      - SE_EVENT_BUS_PUBLISH_PORT=4442
      - SE_EVENT_BUS_SUBSCRIBE_PORT=4443
      - SE_NODE_MAX_SESSIONS=2
      - SE_VNC_NO_PASSWORD=1
    shm_size: "2gb"

  firefox-node:
    image: selenium/node-firefox:4.18.1
    depends_on:
      - selenium-hub
    ports:
      - "7901:7900"   # Different host port to avoid conflict
    environment:
      - SE_EVENT_BUS_HOST=selenium-hub
      - SE_EVENT_BUS_PUBLISH_PORT=4442
      - SE_EVENT_BUS_SUBSCRIBE_PORT=4443
      - SE_NODE_MAX_SESSIONS=2
      - SE_VNC_NO_PASSWORD=1
    shm_size: "2gb"
```

```bash
# Start with VNC
docker-compose -f docker-compose-debug.yml up -d

# Access VNC for Chrome node: http://localhost:7900
# Access VNC for Firefox node: http://localhost:7901
# Default VNC password: secret (unless SE_VNC_NO_PASSWORD is set)
```

### Step 5: Docker Compose with Video Recording

```yaml
# docker-compose-video.yml
version: "3.8"

services:
  selenium-hub:
    image: selenium/hub:4.18.1
    container_name: selenium-hub
    ports:
      - "4442:4442"
      - "4443:4443"
      - "4444:4444"

  chrome:
    image: selenium/node-chrome:4.18.1
    container_name: chrome-node
    depends_on:
      - selenium-hub
    environment:
      - SE_EVENT_BUS_HOST=selenium-hub
      - SE_EVENT_BUS_PUBLISH_PORT=4442
      - SE_EVENT_BUS_SUBSCRIBE_PORT=4443
      - SE_NODE_MAX_SESSIONS=2
    shm_size: "2gb"

  chrome-video:
    image: selenium/video:ffmpeg-6.1-20240123
    container_name: chrome-video
    depends_on:
      - chrome
    environment:
      - DISPLAY_CONTAINER_NAME=chrome-node
      - SE_VIDEO_FILE_NAME=chrome_test_recording
    volumes:
      - ./recordings:/videos
```

```bash
# Start with video recording
docker-compose -f docker-compose-video.yml up -d

# Run your tests...
# Videos will be saved to ./recordings/ directory

# Stop containers
docker-compose -f docker-compose-video.yml down
```

### Step 6: Run Tests Against Docker Grid

```java
// The Grid URL remains the same: http://localhost:4444/wd/hub
// No code changes needed in your tests!

public class DockerGridTest extends BaseTest {

    @Test
    public void testOnDockerGrid() {
        getDriver().get("https://www.selenium.dev");
        Assert.assertTrue(
            getDriver().getTitle().toLowerCase().contains("selenium"),
            "Should be on Selenium website"
        );
        System.out.println("Test executed on Docker Grid successfully!");
    }
}
```

### Step 7: Clean Up

```bash
# Stop and remove all containers
docker-compose down

# Stop, remove containers, AND remove volumes
docker-compose down -v

# Remove Selenium images (to free disk space)
docker rmi selenium/hub:4.18.1
docker rmi selenium/node-chrome:4.18.1
docker rmi selenium/node-firefox:4.18.1
docker rmi selenium/node-edge:4.18.1
```

---

## Exercise 7: BrowserStack Integration

### Objective
Configure and run tests on BrowserStack cloud Grid.

### Step 1: BrowserStack Configuration

```java
// config/BrowserStackConfig.java
package config;

public class BrowserStackConfig {

    // Use environment variables (NEVER hardcode credentials)
    public static final String USERNAME = System.getenv("BROWSERSTACK_USERNAME");
    public static final String ACCESS_KEY = System.getenv("BROWSERSTACK_ACCESS_KEY");

    public static final String HUB_URL = String.format(
        "https://%s:%s@hub-cloud.browserstack.com/wd/hub",
        USERNAME, ACCESS_KEY
    );

    // Validate credentials are set
    public static void validate() {
        if (USERNAME == null || USERNAME.isEmpty()) {
            throw new RuntimeException(
                "BROWSERSTACK_USERNAME environment variable is not set"
            );
        }
        if (ACCESS_KEY == null || ACCESS_KEY.isEmpty()) {
            throw new RuntimeException(
                "BROWSERSTACK_ACCESS_KEY environment variable is not set"
            );
        }
    }
}
```

### Step 2: BrowserStack Driver Factory

```java
// driver/BrowserStackDriverFactory.java
package driver;

import config.BrowserStackConfig;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.openqa.selenium.firefox.FirefoxOptions;
import org.openqa.selenium.remote.RemoteWebDriver;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.HashMap;
import java.util.Map;

public class BrowserStackDriverFactory {

    public static WebDriver createDriver(
            String browser,
            String browserVersion,
            String os,
            String osVersion,
            String testName
    ) throws MalformedURLException {

        BrowserStackConfig.validate();

        // Common BrowserStack options
        Map<String, Object> bstackOptions = new HashMap<>();
        bstackOptions.put("os", os);
        bstackOptions.put("osVersion", osVersion);
        bstackOptions.put("projectName", "Selenium Grid Course");
        bstackOptions.put("buildName", "Build-" +
            System.getProperty("build.number", "local"));
        bstackOptions.put("sessionName", testName);
        bstackOptions.put("local", "false");
        bstackOptions.put("seleniumVersion", "4.18.1");
        bstackOptions.put("debug", "true");
        bstackOptions.put("consoleLogs", "info");
        bstackOptions.put("networkLogs", "true");

        WebDriver driver;

        switch (browser.toLowerCase()) {
            case "chrome":
                ChromeOptions chromeOptions = new ChromeOptions();
                chromeOptions.setBrowserVersion(browserVersion);
                chromeOptions.setCapability("bstack:options", bstackOptions);
                driver = new RemoteWebDriver(
                    new URL(BrowserStackConfig.HUB_URL), chromeOptions
                );
                break;

            case "firefox":
                FirefoxOptions firefoxOptions = new FirefoxOptions();
                firefoxOptions.setBrowserVersion(browserVersion);
                firefoxOptions.setCapability("bstack:options", bstackOptions);
                driver = new RemoteWebDriver(
                    new URL(BrowserStackConfig.HUB_URL), firefoxOptions
                );
                break;

            default:
                throw new IllegalArgumentException("Unsupported browser: " + browser);
        }

        driver.manage().window().maximize();
        return driver;
    }

    /**
     * Mark test status on BrowserStack dashboard
     */
    public static void markTestStatus(WebDriver driver, String status, String reason) {
        ((org.openqa.selenium.JavascriptExecutor) driver).executeScript(
            String.format(
                "browserstack_executor: {\"action\": \"setSessionStatus\", " +
                "\"arguments\": {\"status\": \"%s\", \"reason\": \"%s\"}}",
                status, reason
            )
        );
    }
}
```

### Step 3: BrowserStack Test

```java
// tests/BrowserStackTest.java
package tests;

import driver.BrowserStackDriverFactory;
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.testng.Assert;
import org.testng.ITestResult;
import org.testng.annotations.*;

public class BrowserStackTest {

    private WebDriver driver;

    @Parameters({"browser", "browserVersion", "os", "osVersion"})
    @BeforeMethod
    public void setup(
            @Optional("chrome") String browser,
            @Optional("120.0") String browserVersion,
            @Optional("Windows") String os,
            @Optional("11") String osVersion,
            java.lang.reflect.Method method
    ) throws Exception {
        driver = BrowserStackDriverFactory.createDriver(
            browser, browserVersion, os, osVersion, method.getName()
        );
    }

    @Test
    public void testBrowserStackLogin() {
        driver.get("https://the-internet.herokuapp.com/login");
        driver.findElement(By.id("username")).sendKeys("tomsmith");
        driver.findElement(By.id("password")).sendKeys("SuperSecretPassword!");
        driver.findElement(By.cssSelector("button[type='submit']")).click();

        String message = driver.findElement(By.id("flash")).getText();
        Assert.assertTrue(message.contains("You logged into a secure area!"));
    }

    @Test
    public void testBrowserStackSearch() {
        driver.get("https://www.google.com");
        Assert.assertTrue(driver.getTitle().contains("Google"));
    }

    @AfterMethod
    public void teardown(ITestResult result) {
        if (driver != null) {
            // Mark test status on BrowserStack
            if (result.getStatus() == ITestResult.SUCCESS) {
                BrowserStackDriverFactory.markTestStatus(
                    driver, "passed", "Test passed successfully"
                );
            } else {
                BrowserStackDriverFactory.markTestStatus(
                    driver, "failed",
                    result.getThrowable() != null
                        ? result.getThrowable().getMessage()
                        : "Test failed"
                );
            }
            driver.quit();
        }
    }
}
```

### Step 4: BrowserStack TestNG XML

```xml
<!-- testng-browserstack.xml -->
<!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd">
<suite name="BrowserStack Cross-Browser Suite" parallel="tests" thread-count="4">

    <test name="Windows 11 - Chrome">
        <parameter name="browser" value="chrome"/>
        <parameter name="browserVersion" value="120.0"/>
        <parameter name="os" value="Windows"/>
        <parameter name="osVersion" value="11"/>
        <classes>
            <class name="tests.BrowserStackTest"/>
        </classes>
    </test>

    <test name="Windows 11 - Firefox">
        <parameter name="browser" value="firefox"/>
        <parameter name="browserVersion" value="121.0"/>
        <parameter name="os" value="Windows"/>
        <parameter name="osVersion" value="11"/>
        <classes>
            <class name="tests.BrowserStackTest"/>
        </classes>
    </test>

    <test name="macOS Sonoma - Chrome">
        <parameter name="browser" value="chrome"/>
        <parameter name="browserVersion" value="120.0"/>
        <parameter name="os" value="OS X"/>
        <parameter name="osVersion" value="Sonoma"/>
        <classes>
            <class name="tests.BrowserStackTest"/>
        </classes>
    </test>

    <test name="macOS Sonoma - Safari">
        <parameter name="browser" value="safari"/>
        <parameter name="browserVersion" value="17.0"/>
        <parameter name="os" value="OS X"/>
        <parameter name="osVersion" value="Sonoma"/>
        <classes>
            <class name="tests.BrowserStackTest"/>
        </classes>
    </test>

</suite>
```

### Step 5: Run BrowserStack Tests

```bash
# Set environment variables
export BROWSERSTACK_USERNAME="your_username"
export BROWSERSTACK_ACCESS_KEY="your_access_key"

# Run tests
mvn test -DsuiteXmlFile=testng-browserstack.xml

# View results on BrowserStack dashboard:
# https://automate.browserstack.com/dashboard
```

---

## Exercise 8: Complete Framework with Grid Support

### Objective
Build a unified test framework that supports local, Grid, and cloud execution.

### Project Structure

```
src/
  main/java/
    config/
      ConfigReader.java
      EnvironmentConfig.java
    driver/
      DriverFactory.java
      DriverManager.java
    pages/
      BasePage.java
      LoginPage.java
  test/java/
    tests/
      BaseTest.java
      LoginTest.java
  test/resources/
    config.properties
    testng-local.xml
    testng-grid.xml
    testng-browserstack.xml
docker-compose.yml
pom.xml
```

### Configuration File

```properties
# src/test/resources/config.properties

# Execution mode: local | grid | browserstack
execution.mode=grid

# Grid settings
grid.url=http://localhost:4444/wd/hub

# Browser settings
browser=chrome
headless=false

# Application settings
base.url=https://the-internet.herokuapp.com

# Timeout settings
implicit.wait=10
page.load.timeout=30
explicit.wait=15

# BrowserStack settings (values from env vars)
browserstack.url=hub-cloud.browserstack.com/wd/hub
browserstack.os=Windows
browserstack.os.version=11
browserstack.browser.version=120.0
```

### Unified DriverFactory

```java
// driver/DriverFactory.java
package driver;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.openqa.selenium.edge.EdgeDriver;
import org.openqa.selenium.edge.EdgeOptions;
import org.openqa.selenium.firefox.FirefoxDriver;
import org.openqa.selenium.firefox.FirefoxOptions;
import org.openqa.selenium.remote.RemoteWebDriver;
import java.net.MalformedURLException;
import java.net.URL;
import java.time.Duration;
import java.util.HashMap;
import java.util.Map;
import java.util.Properties;

public class DriverFactory {

    /**
     * Create a WebDriver based on execution mode
     */
    public static WebDriver createDriver(Properties config, String browser, String testName) {
        String executionMode = config.getProperty("execution.mode", "local");
        boolean headless = Boolean.parseBoolean(config.getProperty("headless", "false"));

        WebDriver driver;

        switch (executionMode.toLowerCase()) {
            case "local":
                driver = createLocalDriver(browser, headless);
                break;
            case "grid":
                driver = createGridDriver(config, browser, headless);
                break;
            case "browserstack":
                driver = createBrowserStackDriver(config, browser, testName);
                break;
            default:
                throw new IllegalArgumentException("Unknown execution mode: " + executionMode);
        }

        // Common configuration
        int implicitWait = Integer.parseInt(config.getProperty("implicit.wait", "10"));
        int pageLoadTimeout = Integer.parseInt(config.getProperty("page.load.timeout", "30"));
        driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(implicitWait));
        driver.manage().timeouts().pageLoadTimeout(Duration.ofSeconds(pageLoadTimeout));
        driver.manage().window().maximize();

        return driver;
    }

    private static WebDriver createLocalDriver(String browser, boolean headless) {
        switch (browser.toLowerCase()) {
            case "chrome":
                ChromeOptions chromeOptions = new ChromeOptions();
                if (headless) chromeOptions.addArguments("--headless=new");
                return new ChromeDriver(chromeOptions);
            case "firefox":
                FirefoxOptions firefoxOptions = new FirefoxOptions();
                if (headless) firefoxOptions.addArguments("-headless");
                return new FirefoxDriver(firefoxOptions);
            case "edge":
                EdgeOptions edgeOptions = new EdgeOptions();
                if (headless) edgeOptions.addArguments("--headless=new");
                return new EdgeDriver(edgeOptions);
            default:
                throw new IllegalArgumentException("Unsupported browser: " + browser);
        }
    }

    private static WebDriver createGridDriver(Properties config, String browser, boolean headless) {
        String gridUrl = config.getProperty("grid.url", "http://localhost:4444/wd/hub");

        try {
            URL url = new URL(gridUrl);
            switch (browser.toLowerCase()) {
                case "chrome":
                    ChromeOptions chromeOpts = new ChromeOptions();
                    if (headless) chromeOpts.addArguments("--headless=new");
                    return new RemoteWebDriver(url, chromeOpts);
                case "firefox":
                    FirefoxOptions ffOpts = new FirefoxOptions();
                    if (headless) ffOpts.addArguments("-headless");
                    return new RemoteWebDriver(url, ffOpts);
                case "edge":
                    EdgeOptions edgeOpts = new EdgeOptions();
                    if (headless) edgeOpts.addArguments("--headless=new");
                    return new RemoteWebDriver(url, edgeOpts);
                default:
                    throw new IllegalArgumentException("Unsupported browser: " + browser);
            }
        } catch (MalformedURLException e) {
            throw new RuntimeException("Invalid Grid URL: " + gridUrl, e);
        }
    }

    private static WebDriver createBrowserStackDriver(Properties config, String browser, String testName) {
        String username = System.getenv("BROWSERSTACK_USERNAME");
        String accessKey = System.getenv("BROWSERSTACK_ACCESS_KEY");
        String hubUrl = "https://" + username + ":" + accessKey + "@"
            + config.getProperty("browserstack.url");

        Map<String, Object> bstackOptions = new HashMap<>();
        bstackOptions.put("os", config.getProperty("browserstack.os", "Windows"));
        bstackOptions.put("osVersion", config.getProperty("browserstack.os.version", "11"));
        bstackOptions.put("projectName", "Grid Framework");
        bstackOptions.put("buildName", "Build-" + System.getProperty("build.number", "local"));
        bstackOptions.put("sessionName", testName);

        try {
            ChromeOptions options = new ChromeOptions();
            options.setBrowserVersion(
                config.getProperty("browserstack.browser.version", "120.0")
            );
            options.setCapability("bstack:options", bstackOptions);
            return new RemoteWebDriver(new URL(hubUrl), options);
        } catch (MalformedURLException e) {
            throw new RuntimeException("Invalid BrowserStack URL", e);
        }
    }
}
```

### Running the Complete Framework

```bash
# Run locally
mvn test -Dexecution.mode=local -Dbrowser=chrome

# Run on local Docker Grid
docker-compose up -d
mvn test -Dexecution.mode=grid -Dbrowser=chrome -DsuiteXmlFile=testng-grid.xml

# Run on BrowserStack
export BROWSERSTACK_USERNAME=your_username
export BROWSERSTACK_ACCESS_KEY=your_key
mvn test -Dexecution.mode=browserstack -DsuiteXmlFile=testng-browserstack.xml
```

---

## Summary of Exercises

| Exercise | Topic | Key Skills |
|----------|-------|------------|
| **1** | Standalone Grid Setup | Starting Grid, verifying via UI and API |
| **2** | Hub-Node Setup | Multi-terminal setup, TOML configuration |
| **3** | RemoteWebDriver Tests | Writing Grid-compatible tests, cross-browser XML |
| **4** | TestNG Parallel | Method/Class/Test-level parallelism |
| **5** | ThreadLocal | Thread-safe driver management, DriverManager pattern |
| **6** | Docker Grid | docker-compose, scaling, VNC, video recording |
| **7** | BrowserStack | Cloud integration, capabilities, test status marking |
| **8** | Complete Framework | Unified factory supporting local/Grid/cloud execution |

---

*Practice each exercise sequentially. Start with the standalone setup (Exercise 1) and work your way up to the complete framework (Exercise 8).*
