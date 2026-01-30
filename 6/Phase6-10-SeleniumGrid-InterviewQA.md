# Phase 6-10: Selenium Grid Interview Questions & Answers

## Table of Contents
- [Grid Architecture](#grid-architecture)
- [Hub vs Node](#hub-vs-node)
- [Parallel Execution](#parallel-execution)
- [Thread Safety](#thread-safety)
- [Cloud Testing](#cloud-testing)
- [Docker Integration](#docker-integration)
- [Troubleshooting & Advanced](#troubleshooting--advanced)

---

## Grid Architecture

### Q1: What is Selenium Grid and why do we need it?

**Answer:**

Selenium Grid is a distributed testing infrastructure that allows you to run Selenium tests on multiple machines, browsers, and operating systems simultaneously.

**Why we need it:**

| Need | How Grid Solves It |
|------|--------------------|
| **Slow test suites** | Parallel execution across multiple nodes reduces total time |
| **Cross-browser testing** | Run the same tests on Chrome, Firefox, Edge, Safari in parallel |
| **Cross-platform testing** | Test on Windows, macOS, Linux simultaneously |
| **Scalability** | Add/remove nodes as workload changes |
| **Resource optimization** | Centralize browser infrastructure, run tests from any machine |
| **CI/CD integration** | Dedicated test infrastructure for automated pipelines |

**Example:** A test suite of 100 tests taking 2 minutes each:
- Sequential execution: 100 x 2 = 200 minutes (~3.3 hours)
- Parallel on 10 nodes: 200 / 10 = 20 minutes

---

### Q2: Explain the architecture of Selenium Grid 4. How is it different from Grid 3?

**Answer:**

**Grid 3 Architecture (Monolithic):**
```
Test → Hub (single process) → Node 1
                             → Node 2
                             → Node 3
```
- Hub was a single monolithic component handling everything
- JSON Wire Protocol
- Limited observability

**Grid 4 Architecture (Component-Based):**
```
Test → Router → Session Queue → Distributor → Node
                 ↕                    ↕
            Event Bus ← Session Map
```

**Grid 4 Components:**

| Component | Purpose |
|-----------|---------|
| **Router** | Entry point; routes requests to the correct component |
| **Distributor** | Maintains node registry, matches capabilities, assigns sessions |
| **Session Map** | Maps active session IDs to their host Nodes |
| **Session Queue** | Buffers new session requests when capacity is exhausted |
| **Event Bus** | Asynchronous messaging between components |
| **Node** | Manages browsers and executes test commands |

**Key Differences:**

| Aspect | Grid 3 | Grid 4 |
|--------|--------|--------|
| **Architecture** | Monolithic Hub | Component-based (6 components) |
| **Protocol** | JSON Wire Protocol | W3C WebDriver Protocol |
| **Deployment** | Hub + Nodes only | Standalone, Hub-Node, or Distributed |
| **Observability** | Limited | GraphQL endpoint, structured logging |
| **Docker** | Community images | Official Docker support |
| **UI** | Basic console | Modern React-based UI |
| **Language** | Java | Java (complete rewrite) |

---

### Q3: What are the three deployment modes of Selenium Grid 4?

**Answer:**

**1. Standalone Mode:**
All components run in a single process. Best for local development.
```bash
java -jar selenium-server-4.x.jar standalone
```
- Simplest setup
- No network communication between components
- Limited scalability
- Good for: Local development, small CI pipelines

**2. Hub-Node Mode:**
Hub bundles (Router + Distributor + Session Map + Queue + Event Bus) with separate Nodes.
```bash
# Hub
java -jar selenium-server-4.x.jar hub
# Node
java -jar selenium-server-4.x.jar node --hub http://hub:4444
```
- Moderate complexity
- Nodes can be on different machines
- Good for: Team environments, medium CI/CD

**3. Distributed Mode:**
Each component runs as an independent process.
```bash
java -jar selenium-server-4.x.jar event-bus
java -jar selenium-server-4.x.jar sessions
java -jar selenium-server-4.x.jar sessionqueue
java -jar selenium-server-4.x.jar distributor
java -jar selenium-server-4.x.jar router
java -jar selenium-server-4.x.jar node
```
- Maximum flexibility and scalability
- Each component can be scaled independently
- Good for: Enterprise, cloud deployments, high-load environments

---

### Q4: What is the role of the Event Bus in Selenium Grid 4?

**Answer:**

The Event Bus is the **internal communication backbone** of Selenium Grid 4. It enables asynchronous, decoupled communication between components.

**Events handled:**
1. **Node Registration** - When a Node starts, it publishes a registration event. The Distributor subscribes to this and adds the Node to its registry.
2. **Node Heartbeat** - Nodes periodically publish heartbeat events. If heartbeats stop, the Node is considered offline.
3. **Session Events** - Session creation, completion, and timeout events flow through the bus.
4. **Status Updates** - Component health and availability changes.

**Communication model:**
```
Publisher (Node) → Event Bus → Subscriber (Distributor)
                             → Subscriber (Session Map)
                             → Subscriber (Router)
```

The Event Bus uses a publish/subscribe pattern with two ports:
- **Publish port** (default 4442): Components publish events here
- **Subscribe port** (default 4443): Components subscribe to events here

---

## Hub vs Node

### Q5: What is the difference between Hub and Node in Selenium Grid?

**Answer:**

| Aspect | Hub | Node |
|--------|-----|------|
| **Role** | Central coordinator | Test executor |
| **Browsers** | None (no browsers run on Hub) | Manages browser instances |
| **Receives** | New session requests from tests | Commands forwarded by Hub |
| **Count** | One per Grid | One or many per Grid |
| **Location** | Central server | Can be on any machine in the network |
| **Components** | Router, Distributor, Session Map, Queue, Event Bus | Node process + browser drivers |
| **Port** | Default 4444 | Default 5555 (configurable) |

**Analogy:**
The Hub is like an **air traffic controller** that directs incoming flights (test requests) to available runways (Nodes). The Nodes are the **runways** where planes (browser sessions) actually land and operate.

---

### Q6: What happens when a test sends a new session request to the Grid?

**Answer:**

The following sequence occurs:

```
Step 1: Test sends POST /session to Grid URL (Router)
         ↓
Step 2: Router forwards the request to Session Queue
         ↓
Step 3: Session Queue stores the request in FIFO order
         ↓
Step 4: Distributor polls Session Queue for pending requests
         ↓
Step 5: Distributor checks available Nodes and matches capabilities
         (e.g., browserName=chrome, platformName=linux)
         ↓
Step 6: Distributor sends "create session" command to matching Node
         ↓
Step 7: Node launches browser, creates WebDriver session
         ↓
Step 8: Node returns session ID to Distributor
         ↓
Step 9: Distributor adds entry to Session Map: {sessionId → nodeUrl}
         ↓
Step 10: Session ID returned to the test script
         ↓
Step 11: Subsequent commands: Router looks up Session Map → forwards to Node
```

**What if no Node matches?**
- The request stays in the Session Queue
- If a matching Node becomes available within the timeout, it is processed
- If the timeout expires (default 300 seconds), an error is returned to the test

---

### Q7: Can a single Node support multiple browsers? How do you configure Node capacity?

**Answer:**

Yes, a single Node can support multiple browsers if the machine has multiple browser drivers installed.

**Configuration via command line:**
```bash
java -jar selenium-server-4.x.jar node \
  --max-sessions 6 \
  --override-max-sessions true \
  --hub http://hub:4444
```

**Configuration via TOML file:**
```toml
# node-config.toml
[node]
detect-drivers = true
max-sessions = 6

# Or explicitly define browser configurations:
[[node.driver-configuration]]
display-name = "Chrome"
stereotype = '{"browserName": "chrome"}'
max-sessions = 3

[[node.driver-configuration]]
display-name = "Firefox"
stereotype = '{"browserName": "firefox"}'
max-sessions = 2

[[node.driver-configuration]]
display-name = "Edge"
stereotype = '{"browserName": "MicrosoftEdge"}'
max-sessions = 1
```

**Capacity recommendation:** Total max sessions should not exceed the number of CPU cores on the Node machine, as each browser instance is resource-intensive.

---

## Parallel Execution

### Q8: How do you achieve parallel test execution with TestNG and Selenium Grid?

**Answer:**

TestNG provides built-in parallel execution through its XML configuration:

```xml
<!-- testng.xml -->
<suite name="Parallel Suite" parallel="methods" thread-count="5">
    <test name="Tests">
        <classes>
            <class name="tests.LoginTest"/>
            <class name="tests.SearchTest"/>
            <class name="tests.CheckoutTest"/>
        </classes>
    </test>
</suite>
```

**Parallel levels:**

| Level | `parallel="methods"` | `parallel="classes"` | `parallel="tests"` |
|-------|---------------------|---------------------|---------------------|
| **Scope** | Each @Test method | Each test class | Each `<test>` tag |
| **Thread** | Method gets its own thread | Class gets its own thread | `<test>` gets its own thread |
| **Use Case** | Max parallelism | Class-level isolation | Cross-browser testing |

**Cross-browser example:**
```xml
<suite name="Cross-Browser" parallel="tests" thread-count="3">
    <test name="Chrome">
        <parameter name="browser" value="chrome"/>
        <classes><class name="tests.LoginTest"/></classes>
    </test>
    <test name="Firefox">
        <parameter name="browser" value="firefox"/>
        <classes><class name="tests.LoginTest"/></classes>
    </test>
    <test name="Edge">
        <parameter name="browser" value="edge"/>
        <classes><class name="tests.LoginTest"/></classes>
    </test>
</suite>
```

**Test code:**
```java
public class LoginTest {
    private WebDriver driver;

    @Parameters({"browser"})
    @BeforeMethod
    public void setup(String browser) throws Exception {
        URL gridUrl = new URL("http://localhost:4444/wd/hub");
        switch (browser) {
            case "chrome": driver = new RemoteWebDriver(gridUrl, new ChromeOptions()); break;
            case "firefox": driver = new RemoteWebDriver(gridUrl, new FirefoxOptions()); break;
            case "edge": driver = new RemoteWebDriver(gridUrl, new EdgeOptions()); break;
        }
    }

    @Test
    public void testLogin() {
        driver.get("https://example.com/login");
        // test logic...
    }

    @AfterMethod
    public void teardown() { if (driver != null) driver.quit(); }
}
```

---

### Q9: What is the difference between parallel="methods", parallel="classes", and parallel="tests"?

**Answer:**

Consider 2 classes, each with 2 test methods, and `thread-count="4"`:

**`parallel="methods"`:**
```
Thread-1: ClassA.test1()
Thread-2: ClassA.test2()
Thread-3: ClassB.test1()
Thread-4: ClassB.test2()
(All 4 methods run simultaneously)
```

**`parallel="classes"`:**
```
Thread-1: ClassA.test1() → ClassA.test2()
Thread-2: ClassB.test1() → ClassB.test2()
(Classes run in parallel, methods within a class run sequentially)
```

**`parallel="tests"`:**
```
<test name="T1">   → Thread-1: ClassA.test1() → ClassA.test2()
  ClassA
</test>
<test name="T2">   → Thread-2: ClassB.test1() → ClassB.test2()
  ClassB
</test>
(Each <test> tag runs in its own thread)
```

**When to use each:**
- **methods**: Independent tests, no shared state, maximum speed
- **classes**: Tests within a class share setup/teardown, need ordering
- **tests**: Cross-browser testing, environment-specific grouping

---

## Thread Safety

### Q10: Why is ThreadLocal important for parallel test execution? What happens without it?

**Answer:**

**Without ThreadLocal (the problem):**
```java
public class BaseTest {
    public static WebDriver driver; // SHARED across all threads!

    @BeforeMethod
    public void setup() {
        driver = new ChromeDriver(); // Thread B overwrites Thread A's driver
    }

    @Test
    public void test1() {
        driver.get("https://page1.com"); // Which driver is this? Unpredictable!
    }
}
```

When running in parallel:
```
Time 0: Thread-A calls setup() → driver = ChromeDriver#1
Time 1: Thread-B calls setup() → driver = ChromeDriver#2  (overwrites #1!)
Time 2: Thread-A calls driver.get() → Uses ChromeDriver#2 (WRONG!)
Time 3: Thread-A calls teardown() → Quits ChromeDriver#2
Time 4: Thread-B calls driver.get() → ChromeDriver#2 already quit (ERROR!)
```

**With ThreadLocal (the solution):**
```java
public class BaseTest {
    private static ThreadLocal<WebDriver> driverThread = new ThreadLocal<>();

    public static WebDriver getDriver() {
        return driverThread.get();
    }

    @BeforeMethod
    public void setup() {
        WebDriver driver = new ChromeDriver();
        driverThread.set(driver); // Stored per-thread
    }

    @Test
    public void test1() {
        getDriver().get("https://page1.com"); // Always gets THIS thread's driver
    }

    @AfterMethod
    public void teardown() {
        getDriver().quit();
        driverThread.remove(); // Prevents memory leaks
    }
}
```

With ThreadLocal:
```
Thread-A storage: { driverThread → ChromeDriver#1 }
Thread-B storage: { driverThread → ChromeDriver#2 }
(Each thread has isolated storage - no interference)
```

---

### Q11: What is the complete ThreadLocal pattern for a Selenium Grid framework?

**Answer:**

```java
// DriverManager.java - Centralized thread-safe driver management
public final class DriverManager {

    private static final ThreadLocal<WebDriver> DRIVER = new ThreadLocal<>();
    private static final String GRID_URL = "http://localhost:4444/wd/hub";

    private DriverManager() {} // Prevent instantiation

    public static WebDriver getDriver() {
        WebDriver driver = DRIVER.get();
        if (driver == null) {
            throw new IllegalStateException(
                "WebDriver not initialized for thread " + Thread.currentThread().getId()
            );
        }
        return driver;
    }

    public static void initDriver(String browser) {
        try {
            URL gridUrl = new URL(GRID_URL);
            WebDriver driver;

            switch (browser.toLowerCase()) {
                case "chrome":
                    driver = new RemoteWebDriver(gridUrl, new ChromeOptions());
                    break;
                case "firefox":
                    driver = new RemoteWebDriver(gridUrl, new FirefoxOptions());
                    break;
                case "edge":
                    driver = new RemoteWebDriver(gridUrl, new EdgeOptions());
                    break;
                default:
                    throw new IllegalArgumentException("Unknown browser: " + browser);
            }

            driver.manage().window().maximize();
            driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(10));
            DRIVER.set(driver);

        } catch (MalformedURLException e) {
            throw new RuntimeException("Invalid Grid URL", e);
        }
    }

    public static void quitDriver() {
        WebDriver driver = DRIVER.get();
        if (driver != null) {
            try {
                driver.quit();
            } finally {
                DRIVER.remove(); // Always remove, even if quit() throws
            }
        }
    }
}
```

**Critical points:**
1. Always call `DRIVER.remove()` in a `finally` block
2. Never store ThreadLocal references in local variables across method boundaries
3. Use `getDriver()` method instead of direct field access
4. Initialize in `@BeforeMethod`, clean up in `@AfterMethod`

---

### Q12: What are the common pitfalls with parallel execution and how do you avoid them?

**Answer:**

| Pitfall | Problem | Solution |
|---------|---------|----------|
| **Shared static WebDriver** | Threads overwrite each other's driver | Use `ThreadLocal<WebDriver>` |
| **Shared test data** | Tests modify same records simultaneously | Use unique data per test or API-generated data |
| **Test ordering dependency** | Test B assumes Test A ran first | Make each test independent with its own setup |
| **Database state** | Tests conflict on shared DB rows | Isolate data per test, use transactions/rollback |
| **File system conflicts** | Tests write to same file simultaneously | Use unique file names with thread ID or timestamp |
| **Memory leaks** | ThreadLocal not removed after test | Always call `threadLocal.remove()` in teardown |
| **Port conflicts** | Tests start servers on same port | Use dynamic port allocation |
| **Screenshot conflicts** | Screenshots overwrite each other | Include thread ID/test name in filename |

```java
// Example: Unique test data per thread
@Test
public void testRegistration() {
    String uniqueEmail = "user_" + Thread.currentThread().getId()
        + "_" + System.currentTimeMillis() + "@test.com";
    // Use uniqueEmail for registration - no conflicts
}

// Example: Unique screenshot names
public void takeScreenshot(String testName) {
    String filename = String.format("screenshot_%s_thread%d_%d.png",
        testName,
        Thread.currentThread().getId(),
        System.currentTimeMillis()
    );
    // Save screenshot with unique filename
}
```

---

## Cloud Testing

### Q13: How do you integrate Selenium tests with BrowserStack/Sauce Labs?

**Answer:**

Cloud integration requires only changing the **RemoteWebDriver URL** and adding **vendor-specific capabilities**. No other code changes are needed.

```java
// Local Grid
WebDriver driver = new RemoteWebDriver(
    new URL("http://localhost:4444/wd/hub"),
    new ChromeOptions()
);

// BrowserStack
ChromeOptions options = new ChromeOptions();
Map<String, Object> bstackOptions = new HashMap<>();
bstackOptions.put("os", "Windows");
bstackOptions.put("osVersion", "11");
bstackOptions.put("browserVersion", "120.0");
bstackOptions.put("sessionName", "My Test");
options.setCapability("bstack:options", bstackOptions);

WebDriver driver = new RemoteWebDriver(
    new URL("https://USER:KEY@hub-cloud.browserstack.com/wd/hub"),
    options
);

// Sauce Labs
ChromeOptions sauceOptions = new ChromeOptions();
Map<String, Object> sauceCaps = new HashMap<>();
sauceCaps.put("name", "My Test");
sauceCaps.put("build", "Build 1");
sauceOptions.setCapability("sauce:options", sauceCaps);
sauceOptions.setPlatformName("Windows 11");
sauceOptions.setBrowserVersion("120");

WebDriver driver = new RemoteWebDriver(
    new URL("https://USER:KEY@ondemand.us-west-1.saucelabs.com:443/wd/hub"),
    sauceOptions
);
```

**Key differences in vendor capabilities:**
| Provider | Capability Key | Hub URL Pattern |
|----------|---------------|-----------------|
| BrowserStack | `bstack:options` | `hub-cloud.browserstack.com/wd/hub` |
| Sauce Labs | `sauce:options` | `ondemand.{region}.saucelabs.com:443/wd/hub` |
| LambdaTest | `LT:Options` | `hub.lambdatest.com/wd/hub` |

---

### Q14: What is the difference between local Grid and cloud-based Grid services?

**Answer:**

| Aspect | Local Grid | Cloud Grid (BrowserStack/Sauce Labs) |
|--------|------------|--------------------------------------|
| **Infrastructure** | You manage servers, VMs, or Docker | Fully managed by provider |
| **Cost** | Hardware + maintenance costs | Subscription-based pricing |
| **Browser Variety** | Limited to what you install | 3000+ browser/OS combinations |
| **Real Devices** | Not available | Mobile real devices included |
| **Scalability** | Limited by hardware | Virtually unlimited |
| **Maintenance** | OS updates, driver updates, patches | Zero maintenance |
| **Setup Time** | Hours to days | Minutes (change URL + capabilities) |
| **Debugging** | Manual log collection | Built-in video, screenshots, logs, network HAR |
| **Geo-Testing** | Single location | Multiple global data centers |
| **Local Testing** | Direct access | Requires tunnel (BrowserStack Local, Sauce Connect) |
| **Data Security** | Full control | Data passes through third-party servers |

**When to use each:**
- **Local Grid**: Sensitive data, cost-conscious teams, development environments
- **Cloud Grid**: CI/CD pipelines, cross-browser/cross-platform, mobile testing, teams without DevOps

---

## Docker Integration

### Q15: How do you set up Selenium Grid using Docker? What are the advantages?

**Answer:**

**Setup with docker-compose:**
```yaml
version: "3.8"
services:
  hub:
    image: selenium/hub:4.18.1
    ports:
      - "4444:4444"
  chrome:
    image: selenium/node-chrome:4.18.1
    depends_on:
      - hub
    environment:
      - SE_EVENT_BUS_HOST=hub
      - SE_EVENT_BUS_PUBLISH_PORT=4442
      - SE_EVENT_BUS_SUBSCRIBE_PORT=4443
      - SE_NODE_MAX_SESSIONS=3
    shm_size: "2gb"
  firefox:
    image: selenium/node-firefox:4.18.1
    depends_on:
      - hub
    environment:
      - SE_EVENT_BUS_HOST=hub
      - SE_EVENT_BUS_PUBLISH_PORT=4442
      - SE_EVENT_BUS_SUBSCRIBE_PORT=4443
      - SE_NODE_MAX_SESSIONS=3
    shm_size: "2gb"
```

```bash
docker-compose up -d                   # Start Grid
docker-compose up -d --scale chrome=5  # Scale to 5 Chrome nodes
docker-compose down                    # Tear down
```

**Advantages of Docker Grid:**

| Advantage | Explanation |
|-----------|-------------|
| **Consistency** | Same container image everywhere (dev, CI, staging) |
| **Reproducibility** | `docker-compose up` creates identical Grid every time |
| **Isolation** | Each node in its own container, no conflicts |
| **Easy Scaling** | `--scale chrome=10` instantly adds capacity |
| **Fast Teardown** | `docker-compose down` removes everything cleanly |
| **CI/CD Integration** | Spin up Grid as part of pipeline, tear down after |
| **No Installation** | No need to install browsers or drivers |
| **Version Pinning** | Use specific image tags for reproducible environments |
| **Resource Control** | Docker limits CPU/memory per container |

---

### Q16: Why is `shm_size: "2gb"` important in Docker Selenium configurations?

**Answer:**

`shm_size` sets the **shared memory** (`/dev/shm`) size for the Docker container.

**Why it matters:**
- Chrome and other browsers use `/dev/shm` (shared memory) for inter-process communication
- Docker's default shared memory is only **64MB**
- When Chrome runs out of shared memory, it crashes with errors like `session deleted because of page crash` or `unknown error: Chrome failed to start`

**The fix:**
```yaml
# docker-compose.yml
services:
  chrome:
    image: selenium/node-chrome:4.18.1
    shm_size: "2gb"  # Increase shared memory to 2GB
```

**Alternative fix (mount host's /dev/shm):**
```yaml
services:
  chrome:
    image: selenium/node-chrome:4.18.1
    volumes:
      - /dev/shm:/dev/shm
```

---

## Troubleshooting & Advanced

### Q17: How do you troubleshoot "Could not start a new session" errors on Grid?

**Answer:**

**Common causes and solutions:**

| Cause | Symptoms | Solution |
|-------|----------|----------|
| **No matching Node** | Requested capabilities don't match any Node | Check Node capabilities; ensure correct browserName/platform |
| **All Nodes at capacity** | Request queued then times out | Scale Nodes or increase max-sessions |
| **Node not registered** | Node process running but not visible in Hub | Check Hub URL in Node config; verify network connectivity |
| **Driver not found** | Node cannot find browser driver executable | Ensure ChromeDriver/GeckoDriver is installed and in PATH |
| **Session Queue timeout** | Request waited too long | Increase `SE_SESSION_REQUEST_TIMEOUT` or add more Nodes |
| **Port conflicts** | Hub or Node fails to start | Use different ports; check no other process uses 4444/5555 |

**Debugging steps:**
```bash
# 1. Check Grid status
curl http://localhost:4444/status | python3 -m json.tool

# 2. Check Grid UI
# Open http://localhost:4444/ui

# 3. Check Grid logs
# Look for "Node registered" messages in Hub output

# 4. Check Node availability
curl http://localhost:4444/status | python3 -c "
import json, sys
data = json.load(sys.stdin)
for node in data['value']['nodes']:
    print(f\"Node: {node['uri']}, Available: {node['availability']}\")
    for slot in node['slots']:
        print(f\"  Slot: {slot['stereotype']}, Session: {slot['session']}\")
"
```

---

### Q18: How do you handle session timeout and cleanup in Selenium Grid?

**Answer:**

```bash
# Grid 4 timeout configuration
java -jar selenium-server-4.x.jar hub \
  --session-request-timeout 300 \
  --session-timeout 300

# Node-level timeout
java -jar selenium-server-4.x.jar node \
  --session-timeout 300 \
  --hub http://hub:4444
```

**Docker environment variables:**
```yaml
environment:
  - SE_SESSION_REQUEST_TIMEOUT=300    # Timeout for queued requests
  - SE_NODE_SESSION_TIMEOUT=300       # Timeout for idle sessions
  - SE_SESSION_RETRY_INTERVAL=2       # Retry interval for queued requests
```

**In test code - always use try/finally:**
```java
WebDriver driver = null;
try {
    driver = new RemoteWebDriver(gridUrl, options);
    // test logic
} catch (Exception e) {
    // handle error
} finally {
    if (driver != null) {
        driver.quit(); // Always release the Grid session
    }
}
```

**Why cleanup matters:**
- Each unreleased session consumes a slot on the Node
- If tests crash without `driver.quit()`, sessions become "zombie sessions"
- Grid's session timeout eventually cleans them, but this wastes capacity
- `@AfterMethod` with `driver.quit()` ensures timely cleanup

---

### Q19: How do you use Selenium Grid with a CI/CD pipeline?

**Answer:**

**GitHub Actions example with Docker Grid:**
```yaml
# .github/workflows/selenium-tests.yml
name: Selenium Grid Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest

    services:
      selenium-hub:
        image: selenium/hub:4.18.1
        ports:
          - 4444:4444
          - 4442:4442
          - 4443:4443

      chrome-node:
        image: selenium/node-chrome:4.18.1
        env:
          SE_EVENT_BUS_HOST: selenium-hub
          SE_EVENT_BUS_PUBLISH_PORT: 4442
          SE_EVENT_BUS_SUBSCRIBE_PORT: 4443
          SE_NODE_MAX_SESSIONS: 3
        options: --shm-size="2gb"

      firefox-node:
        image: selenium/node-firefox:4.18.1
        env:
          SE_EVENT_BUS_HOST: selenium-hub
          SE_EVENT_BUS_PUBLISH_PORT: 4442
          SE_EVENT_BUS_SUBSCRIBE_PORT: 4443
          SE_NODE_MAX_SESSIONS: 3
        options: --shm-size="2gb"

    steps:
      - uses: actions/checkout@v4

      - name: Set up JDK
        uses: actions/setup-java@v4
        with:
          java-version: '17'
          distribution: 'temurin'

      - name: Wait for Grid to be ready
        run: |
          for i in $(seq 1 30); do
            if curl -s http://localhost:4444/status | grep -q '"ready":true'; then
              echo "Grid is ready!"
              break
            fi
            echo "Waiting for Grid... ($i/30)"
            sleep 2
          done

      - name: Run tests
        run: mvn test -DsuiteXmlFile=testng-grid.xml -Dgrid.url=http://localhost:4444/wd/hub

      - name: Upload test results
        if: always()
        uses: actions/upload-artifact@v4
        with:
          name: test-results
          path: target/surefire-reports/
```

---

### Q20: What is the GraphQL endpoint in Selenium Grid 4 and how do you use it?

**Answer:**

Grid 4 exposes a GraphQL endpoint at `http://grid-url:4444/graphql` for querying Grid status programmatically.

```graphql
# Query all sessions
{
  sessionsInfo {
    sessions {
      id
      capabilities
      startTime
      uri
      nodeId
      nodeUri
      sessionDurationMillis
    }
  }
}

# Query Grid status
{
  grid {
    uri
    totalSlots
    usedSlots
    sessionCount
    maxSession
    nodeCount
    version
  }
}

# Query Node details
{
  nodesInfo {
    nodes {
      id
      uri
      status
      maxSession
      slotCount
      sessions {
        id
        capabilities
        sessionDurationMillis
      }
    }
  }
}
```

**Using curl to query:**
```bash
curl -X POST http://localhost:4444/graphql \
  -H "Content-Type: application/json" \
  -d '{"query":"{ grid { totalSlots usedSlots nodeCount } }"}'
```

**Use cases:**
- Custom monitoring dashboards
- Automated capacity management
- Integration with alerting systems
- CI/CD pipeline status checks

---

## Quick Reference Card

| Topic | Key Point |
|-------|-----------|
| **Grid 4 Components** | Router, Distributor, Session Map, Session Queue, Event Bus, Node |
| **Deployment Modes** | Standalone, Hub-Node, Distributed |
| **RemoteWebDriver** | Sends HTTP commands to Grid; requires URL + Options |
| **Parallel (TestNG)** | methods, classes, tests, instances |
| **ThreadLocal** | Thread-isolated WebDriver storage; always call `remove()` |
| **Cloud Providers** | BrowserStack (`bstack:options`), Sauce Labs (`sauce:options`), LambdaTest (`LT:Options`) |
| **Docker Grid** | `docker-compose up -d`; use `shm_size: "2gb"` for Chrome |
| **Session Flow** | Test -> Router -> Queue -> Distributor -> Node |
| **Troubleshooting** | Check `/status`, `/ui`, logs; verify Node registration |
| **GraphQL** | `POST /graphql` for programmatic Grid querying |

---

*Review these questions thoroughly. Focus on understanding the architecture flow, ThreadLocal pattern, and Docker setup -- these are the most commonly asked topics in interviews.*
