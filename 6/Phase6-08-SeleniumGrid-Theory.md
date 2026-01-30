# Phase 6-08: Selenium Grid & Parallel Execution Theory

## Table of Contents
- [Introduction to Selenium Grid](#introduction-to-selenium-grid)
- [Selenium Grid 4 Architecture](#selenium-grid-4-architecture)
- [Grid Components Deep Dive](#grid-components-deep-dive)
- [Hub-Node Setup Models](#hub-node-setup-models)
- [RemoteWebDriver](#remotewebdriver)
- [DesiredCapabilities & Options](#desiredcapabilities--options)
- [Parallel Execution Concepts](#parallel-execution-concepts)
- [ThreadLocal for Thread Safety](#threadlocal-for-thread-safety)
- [Cloud-Based Grids](#cloud-based-grids)
- [Docker with Selenium Grid](#docker-with-selenium-grid)
- [Summary](#summary)

---

## Introduction to Selenium Grid

### What is Selenium Grid?

Selenium Grid is a component of the Selenium Suite that enables **distributed test execution** across multiple machines, browsers, and operating systems simultaneously. It allows you to run tests in parallel, significantly reducing total execution time.

### Why Use Selenium Grid?

| Benefit | Description |
|---------|-------------|
| **Parallel Execution** | Run multiple tests simultaneously across different machines |
| **Cross-Browser Testing** | Execute the same test on Chrome, Firefox, Edge, Safari in parallel |
| **Cross-Platform Testing** | Test on Windows, macOS, Linux simultaneously |
| **Reduced Execution Time** | Divide test suite across multiple nodes for faster feedback |
| **Centralized Management** | Single hub manages all test distribution |
| **Scalability** | Add more nodes as test suites grow |

### Evolution of Selenium Grid

```
Grid 1 (2008)     --> Grid 2 (2011)        --> Grid 3 (2016)       --> Grid 4 (2021)
- Separate project    - Merged into Selenium   - Improved stability    - Complete rewrite
- Basic routing       - Hub-Node model         - Better error handling - New architecture
                      - JSON Wire Protocol     - JSON Wire Protocol   - W3C Protocol
                                                                       - GraphQL support
                                                                       - Docker support
                                                                       - Observability
```

---

## Selenium Grid 4 Architecture

### Overview

Selenium Grid 4 was a complete architectural rewrite, introducing a modern, component-based design. Unlike Grid 3 (monolithic Hub-Node model), Grid 4 separates concerns into distinct components.

### Architecture Diagram

```
                    ┌──────────────────────────────────┐
                    │         Selenium Grid 4           │
                    │                                    │
  Test Scripts      │  ┌──────────┐    ┌─────────────┐ │
  ──────────────────┼─>│  Router   │───>│ Distributor  │ │
  (RemoteWebDriver) │  └──────────┘    └──────┬──────┘ │
                    │        │                 │        │
                    │        v                 v        │
                    │  ┌──────────┐    ┌─────────────┐ │
                    │  │ Session  │    │    Event     │ │
                    │  │   Map    │    │     Bus      │ │
                    │  └──────────┘    └──────┬──────┘ │
                    │                         │        │
                    │         ┌───────────────┼────┐   │
                    │         v               v    v   │
                    │  ┌──────────┐  ┌─────┐ ┌─────┐  │
                    │  │  Node 1  │  │ N2  │ │ N3  │  │
                    │  │ (Chrome) │  │(FF) │ │(Edge)│  │
                    │  └──────────┘  └─────┘ └─────┘  │
                    └──────────────────────────────────┘
```

### Core Components

| Component | Role | Description |
|-----------|------|-------------|
| **Router** | Entry point | Receives all external requests and routes them to the correct component |
| **Distributor** | Session creator | Maintains a model of available locations, assigns new sessions to nodes |
| **Session Map** | Session tracker | Maps session IDs to the node where the session is running |
| **Session Queue** | Request queue | Holds new session requests when no capacity is available |
| **Node** | Test executor | Manages browser instances and executes test commands |
| **Event Bus** | Communication | Internal message bus for component-to-component communication |

---

## Grid Components Deep Dive

### Router

The Router is the **single entry point** to the Grid. All external HTTP requests first hit the Router.

**Responsibilities:**
- Accepts new session requests from RemoteWebDriver
- Routes new session requests to the Session Queue
- Forwards active session commands to the correct Node (via Session Map lookup)
- Exposes the Grid status endpoint (`/status`)
- Serves the Grid UI console

```
Client Request Flow:
1. New Session Request → Router → Session Queue → Distributor → Node
2. Active Session Command → Router → Session Map (lookup node) → Node
3. Status Request → Router → /status endpoint
```

### Distributor

The Distributor manages **session allocation** across available Nodes.

**Responsibilities:**
- Maintains awareness of all registered Nodes and their capabilities
- Pulls new session requests from the Session Queue
- Matches requested capabilities with available Node capabilities
- Creates sessions on the most appropriate Node
- Handles node registration and deregistration

**Session matching logic:**
```
Requested: {browserName: "chrome", platformName: "linux"}

Node 1: {browserName: "chrome", platformName: "windows"} → NO MATCH
Node 2: {browserName: "chrome", platformName: "linux"}   → MATCH ✓
Node 3: {browserName: "firefox", platformName: "linux"}  → NO MATCH
```

### Session Map

The Session Map maintains a **mapping of session IDs to Nodes**.

```
Session Map (in-memory store):
┌─────────────────────────────────┬──────────────────┐
│ Session ID                      │ Node URI          │
├─────────────────────────────────┼──────────────────┤
│ abc-123-def-456                 │ http://node1:5555 │
│ ghi-789-jkl-012                 │ http://node2:5556 │
│ mno-345-pqr-678                 │ http://node3:5557 │
└─────────────────────────────────┴──────────────────┘
```

**How it works:**
1. When a new session is created, the Distributor adds an entry to the Session Map
2. When a command arrives for an existing session, the Router looks up the Node URI from the Session Map
3. When a session is deleted (browser quit), the entry is removed

### Session Queue

The Session Queue **buffers new session requests** when all Nodes are at capacity.

**Behavior:**
- Requests are queued in FIFO order
- The Distributor polls the queue for pending requests
- When a Node becomes available, the next request in the queue is processed
- Requests have a configurable timeout (default: 300 seconds)
- Timed-out requests return an error to the client

### Node

The Node is responsible for **managing browser instances** and executing test commands.

**Responsibilities:**
- Registers itself with the Distributor (announces capabilities)
- Manages WebDriver sessions (create, forward commands, destroy)
- Reports health status and available capacity
- Supports multiple concurrent sessions (configurable)

**Node configuration options:**
```
Max Sessions:        5          (total concurrent browsers)
Max Sessions per Browser:
  Chrome:            3
  Firefox:           2
  Edge:              1
```

### Event Bus

The Event Bus provides **asynchronous communication** between Grid components.

**Message types:**
- Node registration/deregistration events
- New session request events
- Session created/completed events
- Heartbeat/health check events

---

## Hub-Node Setup Models

Selenium Grid 4 supports multiple deployment models:

### 1. Standalone Mode

All components run in a **single process**. Ideal for local development and small-scale testing.

```
┌──────────────────────────────────────┐
│          Standalone Server            │
│                                       │
│  Router + Distributor + Session Map   │
│  + Session Queue + Event Bus + Node   │
│                                       │
│  Browsers: Chrome, Firefox, Edge      │
└──────────────────────────────────────┘
```

```bash
# Start standalone
java -jar selenium-server-4.x.jar standalone

# With specific port
java -jar selenium-server-4.x.jar standalone --port 4444
```

### 2. Hub and Node Mode

The classic model: a **Hub** (Router + Distributor + Session Map + Queue + Event Bus) with separate **Nodes**.

```
┌───────────────────────┐
│         Hub            │     ┌──────────────┐
│  (Router, Distributor, │────>│   Node 1     │
│   Session Map, Queue,  │     │  (Chrome)    │
│   Event Bus)           │     └──────────────┘
│                        │     ┌──────────────┐
│  http://hub:4444       │────>│   Node 2     │
│                        │     │  (Firefox)   │
│                        │     └──────────────┘
│                        │     ┌──────────────┐
│                        │────>│   Node 3     │
│                        │     │  (Edge)      │
└───────────────────────┘     └──────────────┘
```

```bash
# Start Hub
java -jar selenium-server-4.x.jar hub --port 4444

# Start Nodes (on different machines or ports)
java -jar selenium-server-4.x.jar node --hub http://hub-ip:4444 --port 5555
java -jar selenium-server-4.x.jar node --hub http://hub-ip:4444 --port 5556
```

### 3. Distributed Mode

Each component runs as a **separate process**. Maximum scalability and flexibility.

```
┌──────────┐  ┌──────────────┐  ┌─────────────┐  ┌──────────────┐
│  Router   │  │ Distributor  │  │ Session Map │  │ Session Queue│
│ :4444     │  │  :5553       │  │  :5556      │  │  :5559       │
└──────────┘  └──────────────┘  └─────────────┘  └──────────────┘
      │               │                │                 │
      └───────────────┴────────────────┴─────────────────┘
                              │
                        ┌──────────┐
                        │ Event Bus│
                        │  :4442   │
                        │  :4443   │
                        └──────────┘
                              │
              ┌───────────────┼───────────────┐
              v               v               v
        ┌──────────┐  ┌──────────┐  ┌──────────┐
        │  Node 1  │  │  Node 2  │  │  Node 3  │
        │  :5555   │  │  :5556   │  │  :5557   │
        └──────────┘  └──────────┘  └──────────┘
```

```bash
# Start Event Bus
java -jar selenium-server-4.x.jar event-bus \
  --publish-events tcp://localhost:4442 \
  --subscribe-events tcp://localhost:4443

# Start Session Map
java -jar selenium-server-4.x.jar sessions \
  --publish-events tcp://localhost:4442 \
  --subscribe-events tcp://localhost:4443

# Start Session Queue
java -jar selenium-server-4.x.jar sessionqueue \
  --publish-events tcp://localhost:4442 \
  --subscribe-events tcp://localhost:4443

# Start Distributor
java -jar selenium-server-4.x.jar distributor \
  --publish-events tcp://localhost:4442 \
  --subscribe-events tcp://localhost:4443 \
  --sessions http://localhost:5556 \
  --sessionqueue http://localhost:5559

# Start Router
java -jar selenium-server-4.x.jar router \
  --distributor http://localhost:5553 \
  --sessions http://localhost:5556 \
  --sessionqueue http://localhost:5559

# Start Nodes
java -jar selenium-server-4.x.jar node \
  --publish-events tcp://localhost:4442 \
  --subscribe-events tcp://localhost:4443
```

### Comparison of Deployment Models

| Feature | Standalone | Hub-Node | Distributed |
|---------|------------|----------|-------------|
| **Setup Complexity** | Very simple | Moderate | Complex |
| **Scalability** | Low | Medium | High |
| **Fault Tolerance** | None | Limited | High |
| **Use Case** | Local dev, CI | Medium teams | Enterprise, cloud |
| **Components** | Single process | 2+ processes | 6+ processes |
| **Maintenance** | Minimal | Moderate | Complex |

---

## RemoteWebDriver

### What is RemoteWebDriver?

`RemoteWebDriver` is the implementation of WebDriver that communicates with a remote Selenium server (Grid Hub/Standalone). Instead of launching a browser locally, it sends commands over HTTP to a remote machine.

### How RemoteWebDriver Works

```
┌──────────────────┐     HTTP/W3C Protocol      ┌──────────────┐
│  Test Machine     │ ─────────────────────────> │ Selenium Grid │
│                   │                             │               │
│  RemoteWebDriver  │ <───────────────────────── │  Node Browser │
│  (sends commands) │     JSON Responses          │  (executes)   │
└──────────────────┘                             └──────────────┘
```

### Code Examples

```java
import org.openqa.selenium.remote.RemoteWebDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import java.net.URL;

public class RemoteWebDriverExample {

    public void runOnGrid() throws Exception {
        // Grid Hub URL
        URL gridUrl = new URL("http://localhost:4444/wd/hub");

        // Configure browser options
        ChromeOptions options = new ChromeOptions();
        options.addArguments("--start-maximized");
        options.addArguments("--disable-notifications");

        // Create RemoteWebDriver pointing to Grid
        WebDriver driver = new RemoteWebDriver(gridUrl, options);

        // Execute test commands (sent to Grid, executed on Node)
        driver.get("https://example.com");
        System.out.println("Title: " + driver.getTitle());

        // Always quit to release Grid resources
        driver.quit();
    }
}
```

**With different browsers:**
```java
// Chrome
ChromeOptions chromeOptions = new ChromeOptions();
WebDriver chromeDriver = new RemoteWebDriver(gridUrl, chromeOptions);

// Firefox
FirefoxOptions firefoxOptions = new FirefoxOptions();
WebDriver firefoxDriver = new RemoteWebDriver(gridUrl, firefoxOptions);

// Edge
EdgeOptions edgeOptions = new EdgeOptions();
WebDriver edgeDriver = new RemoteWebDriver(gridUrl, edgeOptions);

// Safari
SafariOptions safariOptions = new SafariOptions();
WebDriver safariDriver = new RemoteWebDriver(gridUrl, safariOptions);
```

---

## DesiredCapabilities & Options

### Evolution: DesiredCapabilities to Options

In Selenium 4, `DesiredCapabilities` is deprecated in favor of browser-specific **Options** classes. However, understanding both is important.

```java
// OLD way (Selenium 3) - DesiredCapabilities (DEPRECATED)
DesiredCapabilities caps = new DesiredCapabilities();
caps.setBrowserName("chrome");
caps.setPlatform(Platform.LINUX);
caps.setCapability("version", "120");
WebDriver driver = new RemoteWebDriver(gridUrl, caps);

// NEW way (Selenium 4) - Browser Options (RECOMMENDED)
ChromeOptions options = new ChromeOptions();
options.setPlatformName("linux");
options.setBrowserVersion("120");
options.addArguments("--headless");
WebDriver driver = new RemoteWebDriver(gridUrl, options);
```

### Common Options Configuration

```java
// Chrome Options
ChromeOptions chromeOptions = new ChromeOptions();
chromeOptions.addArguments("--headless=new");
chromeOptions.addArguments("--disable-gpu");
chromeOptions.addArguments("--window-size=1920,1080");
chromeOptions.addArguments("--no-sandbox");
chromeOptions.addArguments("--disable-dev-shm-usage");
chromeOptions.setAcceptInsecureCerts(true);
chromeOptions.setPageLoadStrategy(PageLoadStrategy.NORMAL);

// Firefox Options
FirefoxOptions firefoxOptions = new FirefoxOptions();
firefoxOptions.addArguments("-headless");
firefoxOptions.addPreference("browser.download.folderList", 2);
firefoxOptions.addPreference("browser.download.dir", "/tmp/downloads");
firefoxOptions.setAcceptInsecureCerts(true);

// Edge Options
EdgeOptions edgeOptions = new EdgeOptions();
edgeOptions.addArguments("--headless=new");
edgeOptions.addArguments("--disable-gpu");
```

### W3C Capabilities

```java
// Standard W3C capabilities
ChromeOptions options = new ChromeOptions();
options.setCapability("browserName", "chrome");
options.setCapability("browserVersion", "120");
options.setCapability("platformName", "linux");
options.setCapability("acceptInsecureCerts", true);
options.setCapability("pageLoadStrategy", "normal");
options.setCapability("unhandledPromptBehavior", "dismiss");

// Setting timeouts
options.setCapability("timeouts", Map.of(
    "implicit", 0,
    "pageLoad", 300000,
    "script", 30000
));
```

---

## Parallel Execution Concepts

### Why Parallel Execution?

```
Sequential Execution:          Parallel Execution (3 threads):
Test 1: ████████ (2 min)       Thread 1: Test 1 ████████
Test 2: ████████ (2 min)       Thread 2: Test 2 ████████
Test 3: ████████ (2 min)       Thread 3: Test 3 ████████
Test 4: ████████ (2 min)       Thread 1: Test 4 ████████
Test 5: ████████ (2 min)       Thread 2: Test 5 ████████
Test 6: ████████ (2 min)       Thread 3: Test 6 ████████
─────────────────              ─────────────────
Total: 12 minutes              Total: 4 minutes
```

### Parallel Execution with TestNG

TestNG provides built-in support for parallel test execution at different levels:

```xml
<!-- testng.xml -->

<!-- Parallel at METHOD level -->
<suite name="Parallel Suite" parallel="methods" thread-count="3">
  <test name="Test">
    <classes>
      <class name="tests.LoginTest"/>
      <class name="tests.SearchTest"/>
    </classes>
  </test>
</suite>

<!-- Parallel at CLASS level -->
<suite name="Parallel Suite" parallel="classes" thread-count="3">
  <test name="Test">
    <classes>
      <class name="tests.LoginTest"/>
      <class name="tests.SearchTest"/>
      <class name="tests.CheckoutTest"/>
    </classes>
  </test>
</suite>

<!-- Parallel at TEST level (each <test> tag runs in its own thread) -->
<suite name="Cross Browser Suite" parallel="tests" thread-count="3">
  <test name="Chrome Tests">
    <parameter name="browser" value="chrome"/>
    <classes>
      <class name="tests.LoginTest"/>
    </classes>
  </test>
  <test name="Firefox Tests">
    <parameter name="browser" value="firefox"/>
    <classes>
      <class name="tests.LoginTest"/>
    </classes>
  </test>
  <test name="Edge Tests">
    <parameter name="browser" value="edge"/>
    <classes>
      <class name="tests.LoginTest"/>
    </classes>
  </test>
</suite>

<!-- Parallel at INSTANCE level -->
<suite name="Parallel Suite" parallel="instances" thread-count="3">
  <test name="Test">
    <classes>
      <class name="tests.DataDrivenTest"/>
    </classes>
  </test>
</suite>
```

### Parallel Execution Levels Comparison

| Level | Behavior | Use Case |
|-------|----------|----------|
| `methods` | Each `@Test` method runs in its own thread | Maximum parallelism, independent tests |
| `classes` | Each test class runs in its own thread | Class-level setup/teardown needed |
| `tests` | Each `<test>` tag runs in its own thread | Cross-browser testing |
| `instances` | Each class instance runs in its own thread | Data-driven with `@Factory` |

---

## ThreadLocal for Thread Safety

### The Thread Safety Problem

When running tests in parallel, multiple threads share static/instance fields. Without thread safety, threads overwrite each other's WebDriver instance.

```java
// PROBLEM: Not thread-safe!
public class BaseTest {
    public static WebDriver driver; // Shared across ALL threads!

    @BeforeMethod
    public void setup() {
        driver = new ChromeDriver(); // Thread 2 overwrites Thread 1's driver!
    }

    @AfterMethod
    public void teardown() {
        driver.quit(); // Thread 1 might quit Thread 2's driver!
    }
}
```

### The ThreadLocal Solution

`ThreadLocal<T>` provides **thread-isolated storage**. Each thread gets its own copy of the variable.

```java
// SOLUTION: Thread-safe with ThreadLocal
public class BaseTest {
    // Each thread gets its own WebDriver instance
    private static ThreadLocal<WebDriver> threadLocalDriver = new ThreadLocal<>();

    public static WebDriver getDriver() {
        return threadLocalDriver.get();
    }

    @BeforeMethod
    public void setup() {
        WebDriver driver = new ChromeDriver();
        threadLocalDriver.set(driver); // Stored per-thread
    }

    @AfterMethod
    public void teardown() {
        if (getDriver() != null) {
            getDriver().quit();
            threadLocalDriver.remove(); // Clean up to prevent memory leaks
        }
    }
}
```

### How ThreadLocal Works Internally

```
┌─────────────────────────────────────────────┐
│              ThreadLocal<WebDriver>          │
│                                              │
│  Thread-1 ──> ChromeDriver instance #1       │
│  Thread-2 ──> FirefoxDriver instance #2      │
│  Thread-3 ──> ChromeDriver instance #3       │
│                                              │
│  Each thread sees ONLY its own instance      │
│  No synchronization needed                   │
└─────────────────────────────────────────────┘
```

### Complete ThreadLocal Pattern for Grid

```java
public class DriverManager {
    private static ThreadLocal<WebDriver> driver = new ThreadLocal<>();

    public static WebDriver getDriver() {
        return driver.get();
    }

    public static void setDriver(WebDriver driverInstance) {
        driver.set(driverInstance);
    }

    public static void initDriver(String browser) throws MalformedURLException {
        URL gridUrl = new URL("http://localhost:4444/wd/hub");
        WebDriver webDriver;

        switch (browser.toLowerCase()) {
            case "chrome":
                ChromeOptions chromeOptions = new ChromeOptions();
                webDriver = new RemoteWebDriver(gridUrl, chromeOptions);
                break;
            case "firefox":
                FirefoxOptions firefoxOptions = new FirefoxOptions();
                webDriver = new RemoteWebDriver(gridUrl, firefoxOptions);
                break;
            case "edge":
                EdgeOptions edgeOptions = new EdgeOptions();
                webDriver = new RemoteWebDriver(gridUrl, edgeOptions);
                break;
            default:
                throw new IllegalArgumentException("Unsupported browser: " + browser);
        }

        webDriver.manage().window().maximize();
        webDriver.manage().timeouts().implicitlyWait(Duration.ofSeconds(10));
        setDriver(webDriver);
    }

    public static void quitDriver() {
        if (getDriver() != null) {
            getDriver().quit();
            driver.remove();  // IMPORTANT: prevent memory leaks
        }
    }
}
```

```java
// Test class using DriverManager
public class LoginTest {

    @Parameters({"browser"})
    @BeforeMethod
    public void setup(@Optional("chrome") String browser) throws MalformedURLException {
        DriverManager.initDriver(browser);
    }

    @Test
    public void testLogin() {
        DriverManager.getDriver().get("https://example.com/login");
        // ... test code using DriverManager.getDriver()
    }

    @AfterMethod
    public void teardown() {
        DriverManager.quitDriver();
    }
}
```

---

## Cloud-Based Grids

### Overview

Cloud-based testing platforms provide hosted Selenium Grids, eliminating the need to maintain your own infrastructure.

### BrowserStack

```
┌───────────────┐        HTTPS          ┌──────────────────┐
│  Your Tests   │ ────────────────────> │   BrowserStack    │
│  (local)      │                        │   Cloud Grid      │
│               │                        │                   │
│ RemoteDriver  │  hub-cloud.browser    │  3000+ devices    │
│ pointing to   │  stack.com/wd/hub     │  Real browsers    │
│ BrowserStack  │                        │  Real OS          │
└───────────────┘                        └──────────────────┘
```

```java
// BrowserStack Configuration
public class BrowserStackSetup {
    public static final String USERNAME = System.getenv("BROWSERSTACK_USERNAME");
    public static final String ACCESS_KEY = System.getenv("BROWSERSTACK_ACCESS_KEY");
    public static final String URL = "https://" + USERNAME + ":" + ACCESS_KEY
        + "@hub-cloud.browserstack.com/wd/hub";

    public WebDriver createDriver() throws MalformedURLException {
        ChromeOptions options = new ChromeOptions();

        // BrowserStack-specific capabilities
        HashMap<String, Object> bstackOptions = new HashMap<>();
        bstackOptions.put("os", "Windows");
        bstackOptions.put("osVersion", "11");
        bstackOptions.put("browserVersion", "120.0");
        bstackOptions.put("projectName", "My Project");
        bstackOptions.put("buildName", "Build 1.0");
        bstackOptions.put("sessionName", "Login Test");
        bstackOptions.put("local", "false");

        options.setCapability("bstack:options", bstackOptions);

        return new RemoteWebDriver(new URL(URL), options);
    }
}
```

### Sauce Labs

```java
// Sauce Labs Configuration
public class SauceLabsSetup {
    public static final String USERNAME = System.getenv("SAUCE_USERNAME");
    public static final String ACCESS_KEY = System.getenv("SAUCE_ACCESS_KEY");
    public static final String URL = "https://" + USERNAME + ":" + ACCESS_KEY
        + "@ondemand.us-west-1.saucelabs.com:443/wd/hub";

    public WebDriver createDriver() throws MalformedURLException {
        ChromeOptions options = new ChromeOptions();
        options.setPlatformName("Windows 11");
        options.setBrowserVersion("120");

        // Sauce Labs-specific capabilities
        Map<String, Object> sauceOptions = new HashMap<>();
        sauceOptions.put("name", "Login Test");
        sauceOptions.put("build", "Build-" + System.getenv("BUILD_NUMBER"));
        sauceOptions.put("screenResolution", "1920x1080");
        sauceOptions.put("extendedDebugging", true);

        options.setCapability("sauce:options", sauceOptions);

        return new RemoteWebDriver(new URL(URL), options);
    }
}
```

### LambdaTest

```java
// LambdaTest Configuration
public class LambdaTestSetup {
    public static final String USERNAME = System.getenv("LT_USERNAME");
    public static final String ACCESS_KEY = System.getenv("LT_ACCESS_KEY");
    public static final String URL = "https://" + USERNAME + ":" + ACCESS_KEY
        + "@hub.lambdatest.com/wd/hub";

    public WebDriver createDriver() throws MalformedURLException {
        ChromeOptions options = new ChromeOptions();
        options.setPlatformName("Windows 11");
        options.setBrowserVersion("120.0");

        // LambdaTest-specific capabilities
        HashMap<String, Object> ltOptions = new HashMap<>();
        ltOptions.put("build", "Build 1.0");
        ltOptions.put("name", "Login Test");
        ltOptions.put("resolution", "1920x1080");
        ltOptions.put("visual", true);
        ltOptions.put("video", true);
        ltOptions.put("console", true);
        ltOptions.put("network", true);

        options.setCapability("LT:Options", ltOptions);

        return new RemoteWebDriver(new URL(URL), options);
    }
}
```

### Cloud Provider Comparison

| Feature | BrowserStack | Sauce Labs | LambdaTest |
|---------|-------------|------------|------------|
| **Real Devices** | Yes (3000+) | Yes (limited) | Yes |
| **Browser Range** | Extensive | Extensive | Extensive |
| **Parallel Tests** | Plan-based | Plan-based | Plan-based |
| **Geo-locations** | Yes | Yes | Yes |
| **Local Testing** | BrowserStack Local | Sauce Connect | LambdaTest Tunnel |
| **Video Recording** | Yes | Yes | Yes |
| **Screenshots** | Yes | Yes | Yes |
| **Analytics** | Yes | Yes | Yes |
| **CI Integration** | Jenkins, GitHub Actions, etc. | Jenkins, GitHub Actions, etc. | Jenkins, GitHub Actions, etc. |
| **Free Tier** | Limited trial | Limited trial | Generous free tier |

---

## Docker with Selenium Grid

### Why Docker for Selenium Grid?

| Benefit | Description |
|---------|-------------|
| **Consistency** | Same environment everywhere (dev, CI, staging) |
| **Isolation** | Each container is independent |
| **Scalability** | Spin up/down nodes instantly |
| **No Setup** | No browser/driver installation needed |
| **Reproducibility** | `docker-compose up` creates identical Grid every time |
| **CI/CD Friendly** | Easy integration with CI pipelines |

### Official Selenium Docker Images

```
selenium/hub                    - Grid Hub component
selenium/node-chrome            - Chrome Node
selenium/node-firefox           - Firefox Node
selenium/node-edge              - Edge Node
selenium/standalone-chrome      - Standalone Chrome (all-in-one)
selenium/standalone-firefox     - Standalone Firefox (all-in-one)
selenium/standalone-edge        - Standalone Edge (all-in-one)
selenium/node-chrome-debug      - Chrome Node with VNC (deprecated)
selenium/video                  - Video recording sidecar
```

**Note on debug images:** In newer versions, VNC support is included in the standard images. The `-debug` suffix images are deprecated.

### Docker Network Concepts for Grid

```
┌────────────────── Docker Network (grid-network) ──────────────────┐
│                                                                    │
│  ┌──────────┐     ┌───────────────┐     ┌───────────────┐        │
│  │ Hub      │     │ Chrome Node   │     │ Firefox Node  │        │
│  │ :4444    │<────│ :5555         │     │ :5556         │        │
│  │          │<────│               │     │               │        │
│  └──────────┘     └───────────────┘     └───────────────┘        │
│       ↑                                                           │
│       │                                                           │
└───────│───────────────────────────────────────────────────────────┘
        │
   Host Machine
   (test scripts connect to localhost:4444)
```

### docker-compose.yml for Selenium Grid

```yaml
# docker-compose.yml - Basic Grid Setup
version: "3.8"

services:
  selenium-hub:
    image: selenium/hub:4.18.1
    container_name: selenium-hub
    ports:
      - "4442:4442"   # Event Bus publish
      - "4443:4443"   # Event Bus subscribe
      - "4444:4444"   # Hub port
    environment:
      - GRID_MAX_SESSION=10
      - SE_SESSION_REQUEST_TIMEOUT=300

  chrome-node:
    image: selenium/node-chrome:4.18.1
    depends_on:
      - selenium-hub
    environment:
      - SE_EVENT_BUS_HOST=selenium-hub
      - SE_EVENT_BUS_PUBLISH_PORT=4442
      - SE_EVENT_BUS_SUBSCRIBE_PORT=4443
      - SE_NODE_MAX_SESSIONS=3
      - SE_NODE_OVERRIDE_MAX_SESSIONS=true
    shm_size: "2gb"   # Important for Chrome stability

  firefox-node:
    image: selenium/node-firefox:4.18.1
    depends_on:
      - selenium-hub
    environment:
      - SE_EVENT_BUS_HOST=selenium-hub
      - SE_EVENT_BUS_PUBLISH_PORT=4442
      - SE_EVENT_BUS_SUBSCRIBE_PORT=4443
      - SE_NODE_MAX_SESSIONS=3
      - SE_NODE_OVERRIDE_MAX_SESSIONS=true
    shm_size: "2gb"

  edge-node:
    image: selenium/node-edge:4.18.1
    depends_on:
      - selenium-hub
    environment:
      - SE_EVENT_BUS_HOST=selenium-hub
      - SE_EVENT_BUS_PUBLISH_PORT=4442
      - SE_EVENT_BUS_SUBSCRIBE_PORT=4443
      - SE_NODE_MAX_SESSIONS=2
      - SE_NODE_OVERRIDE_MAX_SESSIONS=true
    shm_size: "2gb"
```

### Scaling Nodes with Docker

```bash
# Start the Grid
docker-compose up -d

# Scale Chrome nodes to 5 instances
docker-compose up -d --scale chrome-node=5

# Scale Firefox nodes to 3 instances
docker-compose up -d --scale firefox-node=3

# Check status
docker-compose ps

# View logs
docker-compose logs -f selenium-hub

# Shut down
docker-compose down
```

### Docker Compose with Video Recording

```yaml
# docker-compose-with-video.yml
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
    depends_on:
      - selenium-hub
    environment:
      - SE_EVENT_BUS_HOST=selenium-hub
      - SE_EVENT_BUS_PUBLISH_PORT=4442
      - SE_EVENT_BUS_SUBSCRIBE_PORT=4443
      - SE_NODE_MAX_SESSIONS=2
    shm_size: "2gb"
    volumes:
      - /dev/shm:/dev/shm

  chrome-video:
    image: selenium/video:ffmpeg-6.1-20240123
    depends_on:
      - chrome
    environment:
      - DISPLAY_CONTAINER_NAME=chrome
      - SE_VIDEO_FILE_NAME=chrome_video.mp4
    volumes:
      - ./videos:/videos
```

### Standalone Docker (Quick Start)

For quick local testing without a full Grid:

```bash
# Single Chrome browser
docker run -d -p 4444:4444 -p 7900:7900 --shm-size="2g" \
  selenium/standalone-chrome:4.18.1

# Access browser via VNC at http://localhost:7900 (password: secret)

# Single Firefox browser
docker run -d -p 4444:4444 -p 7900:7900 --shm-size="2g" \
  selenium/standalone-firefox:4.18.1
```

---

## Summary

### Key Takeaways

| Concept | Key Point |
|---------|-----------|
| **Grid 4 Architecture** | Component-based: Router, Distributor, Session Map, Queue, Event Bus, Node |
| **Deployment Models** | Standalone (simple), Hub-Node (medium), Distributed (enterprise) |
| **RemoteWebDriver** | Connects tests to remote Grid via HTTP; uses browser Options |
| **Options vs DesiredCapabilities** | Options is the Selenium 4 standard; DesiredCapabilities is deprecated |
| **Parallel Execution** | TestNG supports methods, classes, tests, instances parallelism |
| **ThreadLocal** | Essential for thread-safe WebDriver in parallel execution |
| **Cloud Grids** | BrowserStack, Sauce Labs, LambdaTest provide hosted infrastructure |
| **Docker Grid** | Consistent, scalable, reproducible Grid setup using containers |

### What to Remember for Interviews

1. **Grid 4 is a complete rewrite** - not an incremental update from Grid 3
2. **Router is the entry point** - all requests flow through it
3. **Distributor matches capabilities** - it decides which Node handles a session
4. **Session Map tracks active sessions** - maps session IDs to Nodes
5. **ThreadLocal is mandatory** for parallel execution thread safety
6. **Always call `threadLocal.remove()`** to prevent memory leaks
7. **Docker compose with `--scale`** enables easy horizontal scaling
8. **`shm_size: "2gb"`** is critical for Chrome stability in Docker

---

*This document covers the theoretical foundations of Selenium Grid and parallel execution. Proceed to Phase 6-09 for hands-on practicals.*
