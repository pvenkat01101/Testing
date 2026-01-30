# Phase 6-16: BDD with Cucumber - Interview Q&A

## Table of Contents
1. [BDD Fundamentals](#bdd-fundamentals)
2. [Gherkin Syntax and Feature Files](#gherkin-syntax-and-feature-files)
3. [Step Definitions](#step-definitions)
4. [Scenario vs Scenario Outline](#scenario-vs-scenario-outline)
5. [Hooks and Tags](#hooks-and-tags)
6. [Cucumber with Selenium](#cucumber-with-selenium)
7. [Background and Data Tables](#background-and-data-tables)
8. [Reporting](#reporting)
9. [CI/CD Integration](#cicd-integration)
10. [Advanced Topics](#advanced-topics)

---

## BDD Fundamentals

### Q1: What is BDD and how does it differ from TDD?

**Answer:**

**BDD (Behaviour-Driven Development)** is a collaborative software development approach where business stakeholders, developers, and testers define application behaviour using plain-language scenarios before implementation begins. It was introduced by Dan North as an evolution of TDD.

| Aspect | TDD | BDD |
|--------|-----|-----|
| **Focus** | Code correctness at unit level | Expected behaviour from a user perspective |
| **Written by** | Developers | Business + Developers + Testers (Three Amigos) |
| **Language** | Programming language | Natural language (Gherkin) |
| **Scope** | Individual units/methods | Features and user stories |
| **Format** | Test methods with assertions | Given-When-Then scenarios |
| **Documentation** | Code and test names | Feature files (living documentation) |
| **Tools** | JUnit, TestNG | Cucumber, SpecFlow, Behave |

**Key difference:** TDD asks "Does the code work correctly?" while BDD asks "Does the application behave as the business expects?"

**Example:**

```
TDD:  assertEquals(calculator.add(2, 3), 5);

BDD:  Given the calculator is open
      When I add 2 and 3
      Then the result should be 5
```

---

### Q2: What are the benefits of using BDD with Cucumber?

**Answer:**

1. **Improved collaboration** -- Product owners, developers, and testers all contribute to feature files using a shared language
2. **Living documentation** -- Feature files serve as always-current specifications that describe how the system actually works
3. **Early defect detection** -- Discussing scenarios before coding catches misunderstandings during the requirements phase
4. **Readable tests** -- Non-technical stakeholders can read and validate test scenarios without understanding code
5. **Reusability** -- Step definitions are reusable across multiple scenarios and feature files
6. **Traceability** -- Each scenario maps directly to a user story or acceptance criterion
7. **Test organization** -- Tags and feature files provide natural grouping and filtering
8. **Multiple test runners** -- Cucumber integrates with TestNG, JUnit, and Maven for flexible execution

---

### Q3: What is the Three Amigos meeting in BDD?

**Answer:**

The Three Amigos is a collaborative meeting held before development of each user story. The three roles are:

1. **Product Owner / Business Analyst** -- explains the desired behaviour and business value
2. **Developer** -- discusses technical feasibility and implementation approach
3. **Tester** -- identifies edge cases, negative scenarios, and testability concerns

**Output:** A set of agreed-upon Gherkin scenarios that serve as the acceptance criteria for the story.

**Example outcome:**

```gherkin
# Agreed scenarios for "User Login" story:
Scenario: Successful login with valid credentials
Scenario: Login fails with wrong password
Scenario: Login fails with non-existent username
Scenario: Account locks after 5 failed attempts
Scenario: Password field masks input characters
```

---

## Gherkin Syntax and Feature Files

### Q4: What is Gherkin? List the main Gherkin keywords.

**Answer:**

Gherkin is a structured plain-text language with specific keywords used to write BDD scenarios. It is designed to be readable by both humans and machines.

| Keyword | Purpose | Example |
|---------|---------|---------|
| `Feature` | Describes a high-level feature | `Feature: User Login` |
| `Scenario` | A single test case (concrete example) | `Scenario: Valid login` |
| `Scenario Outline` | Parameterised template for data-driven tests | `Scenario Outline: Login with <role>` |
| `Given` | Precondition (arrange) | `Given the user is on the login page` |
| `When` | Action (act) | `When the user submits valid credentials` |
| `Then` | Expected result (assert) | `Then the dashboard is displayed` |
| `And` | Additional step (same type as previous) | `And the welcome message is shown` |
| `But` | Negative additional step | `But the admin panel is not visible` |
| `Background` | Steps shared across all scenarios in a feature | `Background: Given the user is logged in` |
| `Examples` | Data table for Scenario Outline | Table of test data rows |
| `@tag` | Label for filtering | `@smoke`, `@regression` |
| `\|` | Data table / Examples delimiter | `\| username \| password \|` |
| `"""` | Doc string (multi-line text) | Used for JSON, XML, or large text |
| `#` | Comment | `# This is a comment` |

---

### Q5: Explain the Given-When-Then structure with an example.

**Answer:**

Given-When-Then maps to the **Arrange-Act-Assert** pattern:

| Keyword | Role | Analogy |
|---------|------|---------|
| `Given` | Set up preconditions | Arrange |
| `When` | Perform the action under test | Act |
| `Then` | Verify the expected outcome | Assert |

**Example:**

```gherkin
Feature: ATM Withdrawal

  Scenario: Successful withdrawal with sufficient balance
    Given the user has a bank account with a balance of $1000
    And the user has a valid ATM card
    When the user withdraws $200
    Then $200 should be dispensed
    And the account balance should be $800
    And a receipt should be printed
```

**Best practices:**
- `Given` should set up the world (not perform actions)
- `When` should describe exactly one action
- `Then` should only verify outcomes (no actions)
- Use `And` / `But` for additional steps of the same type

---

### Q6: What is a Feature file? What are the best practices for writing feature files?

**Answer:**

A feature file is a plain-text file with a `.feature` extension that contains one or more Gherkin scenarios describing a single feature of the application.

**Structure:**

```gherkin
@tag
Feature: Feature Name
  As a [role]
  I want [capability]
  So that [benefit]

  Background:
    Given common preconditions

  Scenario: First scenario
    Given ...
    When ...
    Then ...
```

**Best practices:**

| Practice | Reason |
|----------|--------|
| One feature per file | Keeps files focused and maintainable |
| Use the user story template | Provides context and business justification |
| Write declarative, not imperative steps | "the user logs in" vs. "the user types in username field" |
| Keep scenarios independent | Each scenario must work in isolation; no shared state |
| Use meaningful scenario names | Acts as documentation and appears in reports |
| Limit scenarios to 3-8 steps | If more, the feature may need splitting |
| Use Background for common preconditions | Reduces duplication |
| Tag scenarios for organisation | Enables selective execution |
| Avoid UI-specific language | "the user submits the form" not "the user clicks the Submit button" |

---

## Step Definitions

### Q7: What are step definitions? How does Cucumber match them to feature file steps?

**Answer:**

Step definitions are Java methods annotated with Cucumber annotations (`@Given`, `@When`, `@Then`, `@And`, `@But`) that contain the actual automation code for each Gherkin step.

**Matching mechanism:**

1. Cucumber reads a step from the feature file, e.g., `When the user enters username "admin"`
2. It scans all classes in the `glue` package for a matching step definition
3. Matching is done using **Cucumber expressions** or **regular expressions**
4. Parameters (e.g., `"admin"`) are extracted and passed as method arguments

```java
// Cucumber expression (recommended)
@When("the user enters username {string} and password {string}")
public void enterCredentials(String username, String password) {
    loginPage.enterUsername(username);
    loginPage.enterPassword(password);
}

// Regular expression (alternative)
@When("^the user enters username \"([^\"]*)\" and password \"([^\"]*)\"$")
public void enterCredentials(String username, String password) {
    loginPage.enterUsername(username);
    loginPage.enterPassword(password);
}
```

**Parameter types in Cucumber expressions:**

| Expression | Java Type | Matches |
|-----------|-----------|---------|
| `{string}` | `String` | `"quoted text"` |
| `{int}` | `int` | `42`, `100` |
| `{float}` | `float` | `3.14` |
| `{word}` | `String` | Single word, no spaces |
| `{bigdecimal}` | `BigDecimal` | Precise decimal values |

---

### Q8: What happens if a step definition is not found? What about duplicate step definitions?

**Answer:**

**Missing step definition:**
- Cucumber throws `io.cucumber.java.PendingException` or marks the step as **undefined**
- The scenario is marked as **UNDEFINED** (yellow in reports)
- Cucumber prints a code snippet you can copy into a step definition class:

```
You can implement missing steps with the snippets below:

@When("the user searches for {string}")
public void the_user_searches_for(String string) {
    // Write code here
    throw new io.cucumber.java.PendingException();
}
```

**Duplicate step definition (same pattern matches multiple methods):**
- Cucumber throws `io.cucumber.core.exception.CucumberException: Duplicate step definitions`
- The test fails immediately
- Fix: Ensure each step pattern is unique across all step definition classes
- Use `dryRun = true` in the runner to detect duplicates before running tests

**Ambiguous step definition (multiple patterns could match):**
- Cucumber throws `AmbiguousStepDefinitionsException`
- Fix: Make patterns more specific

---

## Scenario vs Scenario Outline

### Q9: What is the difference between Scenario and Scenario Outline?

**Answer:**

| Aspect | Scenario | Scenario Outline |
|--------|----------|-----------------|
| **Data** | Hard-coded values | Parameterised with `<placeholder>` |
| **Execution** | Runs once | Runs once per row in the Examples table |
| **Use case** | Single, specific test case | Data-driven testing with multiple inputs |
| **Examples table** | Not used | Required |
| **Readability** | Very clear for single cases | Compact for multiple similar cases |

**Scenario (single test case):**

```gherkin
Scenario: Valid login
  Given the user is on the login page
  When the user enters username "admin" and password "admin123"
  Then the user should see the dashboard
```

**Scenario Outline (multiple test cases, same flow):**

```gherkin
Scenario Outline: Login with various credentials
  Given the user is on the login page
  When the user enters username "<username>" and password "<password>"
  Then the result should be "<expectedResult>"

  Examples:
    | username | password  | expectedResult |
    | admin    | admin123  | success        |
    | admin    | wrong     | failure        |
    | unknown  | admin123  | failure        |
```

**When to use which:**
- Use **Scenario** when the test case is unique or has distinct steps
- Use **Scenario Outline** when the same steps apply to multiple sets of data

---

### Q10: Can a Scenario Outline have multiple Examples tables?

**Answer:**

Yes. Multiple `Examples` blocks allow you to group data logically and tag them independently.

```gherkin
Scenario Outline: Login with role-based credentials
  Given the user is on the login page
  When the user logs in as "<role>" with password "<password>"
  Then the user should have access to "<dashboard>"

  @admin
  Examples: Admin users
    | role         | password   | dashboard       |
    | super_admin  | sa123      | Admin Dashboard  |
    | site_admin   | site123    | Admin Dashboard  |

  @regular
  Examples: Regular users
    | role     | password | dashboard        |
    | buyer    | buy123   | Buyer Dashboard   |
    | seller   | sell123  | Seller Dashboard  |

  @negative
  Examples: Invalid users
    | role    | password | dashboard   |
    | guest   | guest    | Error Page  |
    | blocked | block123 | Error Page  |
```

**Benefits:**
- Run only admin scenarios: `@admin`
- Run only negative cases: `@negative`
- Clear logical grouping in reports

---

## Hooks and Tags

### Q11: What are Cucumber Hooks? List all hook types and their execution order.

**Answer:**

Hooks are methods that run automatically at specific points in the Cucumber lifecycle. They are defined in any class within the `glue` path.

| Hook | When It Runs | Scope | Must Be Static? |
|------|-------------|-------|-----------------|
| `@BeforeAll` | Once before the entire suite | Suite | Yes |
| `@Before` | Before each scenario | Scenario | No |
| `@BeforeStep` | Before each step | Step | No |
| `@AfterStep` | After each step | Step | No |
| `@After` | After each scenario | Scenario | No |
| `@AfterAll` | Once after the entire suite | Suite | Yes |

**Execution order:**

```
@BeforeAll               (once, static)
  @Before(order=0)       (each scenario, lowest order first)
  @Before(order=1)
    @BeforeStep           (each step)
      STEP EXECUTES
    @AfterStep            (each step)
  @After(order=1)        (each scenario, HIGHEST order first)
  @After(order=0)
@AfterAll                (once, static)
```

**Important:** `@After` hooks run in **reverse order** -- higher order numbers execute first.

**Common usage:**

```java
@Before(order = 0)
public void startBrowser() { DriverManager.createDriver(); }

@Before(order = 1)
public void navigateToApp() { DriverManager.getDriver().get(baseUrl); }

@After(order = 1)
public void takeScreenshotOnFailure(Scenario scenario) {
    if (scenario.isFailed()) {
        byte[] png = ((TakesScreenshot) DriverManager.getDriver())
                .getScreenshotAs(OutputType.BYTES);
        scenario.attach(png, "image/png", scenario.getName());
    }
}

@After(order = 0)
public void closeBrowser() { DriverManager.quitDriver(); }
```

---

### Q12: What are Cucumber Tags? How do you use tag expressions?

**Answer:**

Tags are annotations prefixed with `@` that label features, scenarios, or Examples blocks. They enable selective execution.

**Applying tags:**

```gherkin
@module-login            # applied to all scenarios in this feature
Feature: Login

  @smoke @priority-high
  Scenario: Valid login
    ...

  @regression @negative
  Scenario: Invalid login
    ...
```

**Tag expressions in the runner:**

```java
@CucumberOptions(tags = "@smoke and not @wip")
```

**Tag expression operators:**

| Expression | Meaning |
|-----------|---------|
| `@smoke` | Scenarios tagged `@smoke` |
| `@smoke and @login` | Both tags present |
| `@smoke or @regression` | Either tag present |
| `not @wip` | Tag NOT present |
| `(@smoke or @regression) and not @slow` | Combined logic |

**Command-line override (Maven):**

```bash
mvn clean test -Dcucumber.filter.tags="@regression and @login"
```

**Conditional hooks with tags:**

```java
@Before("@api")
public void setupApi() {
    // Only runs for scenarios tagged @api
}

@Before("@ui and not @headless")
public void setupBrowserWithUI() {
    // Only for UI scenarios that are not headless
}
```

---

## Cucumber with Selenium

### Q13: How do you integrate Cucumber with Selenium WebDriver?

**Answer:**

The integration follows this pattern:

1. **DriverManager** manages WebDriver lifecycle (ThreadLocal for parallel safety)
2. **Hooks** create the driver in `@Before` and quit it in `@After`
3. **Page Objects** encapsulate locators and actions
4. **Step definitions** use page objects to interact with the UI
5. **Runner** ties everything together with `@CucumberOptions`

**Architecture flow:**

```
Feature File
    --> Step Definition (calls Page Object methods)
        --> Page Object (uses WebDriver to interact with UI)
            --> DriverManager (provides thread-safe WebDriver)
                --> Hooks (create/destroy driver per scenario)
```

**Key code:**

```java
// Hooks.java
@Before
public void setUp() {
    DriverManager.createDriver();
}

@After
public void tearDown(Scenario scenario) {
    if (scenario.isFailed()) {
        byte[] screenshot = ((TakesScreenshot) DriverManager.getDriver())
                .getScreenshotAs(OutputType.BYTES);
        scenario.attach(screenshot, "image/png", scenario.getName());
    }
    DriverManager.quitDriver();
}

// LoginSteps.java
@Given("the user is on the login page")
public void navigateToLogin() {
    loginPage = new LoginPage(DriverManager.getDriver());
    loginPage.navigateTo();
}
```

**Why ThreadLocal?** When running scenarios in parallel (via `@DataProvider(parallel = true)`), each thread must have its own WebDriver instance. ThreadLocal ensures thread safety without synchronisation overhead.

---

### Q14: How do you share state between step definition classes in Cucumber?

**Answer:**

Since step definitions may be split across multiple classes, sharing state (like WebDriver or page objects) requires a mechanism:

**Approach 1: Dependency Injection with PicoContainer (most common)**

```xml
<dependency>
    <groupId>io.cucumber</groupId>
    <artifactId>cucumber-picocontainer</artifactId>
    <version>7.14.0</version>
    <scope>test</scope>
</dependency>
```

```java
// Shared context class
public class TestContext {
    private WebDriver driver;
    private LoginPage loginPage;
    private Scenario scenario;

    public TestContext() {
        this.driver = DriverManager.getDriver();
    }

    public WebDriver getDriver() { return driver; }
    public LoginPage getLoginPage() {
        if (loginPage == null) loginPage = new LoginPage(driver);
        return loginPage;
    }
}

// Step definition -- PicoContainer injects TestContext via constructor
public class LoginSteps {
    private TestContext context;

    public LoginSteps(TestContext context) {
        this.context = context;
    }

    @Given("the user is on the login page")
    public void navigateToLogin() {
        context.getLoginPage().navigateTo();
    }
}

public class DashboardSteps {
    private TestContext context;

    public DashboardSteps(TestContext context) {
        this.context = context;
    }

    @Then("the dashboard should be displayed")
    public void verifyDashboard() {
        Assert.assertTrue(context.getDriver().getCurrentUrl().contains("/dashboard"));
    }
}
```

**Approach 2: Static ThreadLocal utility (simpler, no DI library needed)**

```java
// DriverManager with static ThreadLocal
public class DriverManager {
    private static ThreadLocal<WebDriver> driver = new ThreadLocal<>();
    public static WebDriver getDriver() { return driver.get(); }
}

// Each step class just calls DriverManager.getDriver()
```

**Approach 3: Spring (for complex enterprise frameworks)**

```xml
<dependency>
    <groupId>io.cucumber</groupId>
    <artifactId>cucumber-spring</artifactId>
    <version>7.14.0</version>
</dependency>
```

---

## Background and Data Tables

### Q15: What is the Background keyword? When should you use it?

**Answer:**

`Background` defines steps that are common to **all scenarios in a feature file**. It runs before each scenario, similar to `@BeforeMethod` in TestNG.

```gherkin
Feature: Account Management

  Background:
    Given the user is logged in as "admin"
    And the user navigates to the account settings page

  Scenario: Change display name
    When the user changes the display name to "New Name"
    Then the display name should be updated to "New Name"

  Scenario: Change email address
    When the user changes the email to "new@test.com"
    Then a verification email should be sent to "new@test.com"
```

**When to use Background:**
- All scenarios in the file share the same preconditions
- The shared steps are short (1-3 steps)
- The preconditions are about the business context (not technical setup)

**When NOT to use Background:**
- Only some scenarios need those steps (use `Given` in individual scenarios instead)
- The setup is long or complex (use tagged hooks instead)
- The steps are technical rather than behavioural (use `@Before` hooks)

**Important:** Background runs before EVERY scenario, not once per file.

---

### Q16: How do you use Data Tables in Cucumber? What are the different formats?

**Answer:**

Data Tables pass structured data to a step. Cucumber provides several ways to read them in Java.

**Format 1: Map (key-value pairs, two columns)**

```gherkin
When the user fills in the form:
  | field    | value          |
  | name     | John Doe       |
  | email    | john@test.com  |
  | phone    | 555-0100       |
```

```java
@When("the user fills in the form:")
public void fillForm(DataTable dataTable) {
    Map<String, String> data = dataTable.asMap(String.class, String.class);
    formPage.enterName(data.get("name"));
    formPage.enterEmail(data.get("email"));
    formPage.enterPhone(data.get("phone"));
}
```

**Format 2: List of Maps (multiple rows, first row = headers)**

```gherkin
When the user creates the following users:
  | name     | email           | role    |
  | Alice    | alice@test.com  | admin   |
  | Bob      | bob@test.com    | user    |
  | Charlie  | charlie@test.com| viewer  |
```

```java
@When("the user creates the following users:")
public void createUsers(DataTable dataTable) {
    List<Map<String, String>> users = dataTable.asMaps(String.class, String.class);
    for (Map<String, String> user : users) {
        adminPage.createUser(user.get("name"), user.get("email"), user.get("role"));
    }
}
```

**Format 3: List of Lists (raw rows/columns)**

```java
List<List<String>> rows = dataTable.asLists(String.class);
// rows.get(0) = ["name", "email", "role"]  (headers)
// rows.get(1) = ["Alice", "alice@test.com", "admin"]
```

**Format 4: Simple List (single column, no headers)**

```gherkin
Then the following categories should be displayed:
  | Electronics |
  | Books       |
  | Clothing    |
```

```java
@Then("the following categories should be displayed:")
public void verifyCategories(List<String> expectedCategories) {
    List<String> actual = categoryPage.getCategories();
    Assert.assertEquals(actual, expectedCategories);
}
```

---

## Reporting

### Q17: What reporting options are available in Cucumber?

**Answer:**

| Report Type | Configuration | Output |
|-------------|--------------|--------|
| **Pretty** (console) | `plugin = {"pretty"}` | Coloured Gherkin in terminal |
| **HTML** (built-in) | `"html:target/cucumber.html"` | Basic single-page HTML |
| **JSON** | `"json:target/cucumber.json"` | Machine-readable JSON |
| **JUnit XML** | `"junit:target/cucumber.xml"` | CI/CD compatible XML |
| **Timeline** | `"timeline:target/timeline"` | Parallel execution visualisation |
| **Cucumber Reports** (cloud) | `publish = true` | Hosted report with 24-hour link |
| **Masterthought** | Maven Cucumber Reporting library | Advanced HTML with charts and trends |
| **Allure** | `allure-cucumber7-jvm` plugin | Enterprise dashboard with history |
| **Extent Reports** | Adapter library (`ExtentCucumberAdapter`) | Detailed HTML with screenshots |

**Recommended setup:**

```java
@CucumberOptions(
    plugin = {
        "pretty",                                    // console
        "html:target/cucumber-reports/index.html",   // basic HTML
        "json:target/cucumber-reports/cucumber.json", // for masterthought
        "junit:target/cucumber-reports/cucumber.xml"  // for CI
    }
)
```

Then generate the advanced report from the JSON file using Masterthought:

```java
ReportBuilder builder = new ReportBuilder(jsonFiles, config);
builder.generateReports();
```

---

### Q18: How do you attach screenshots to Cucumber reports on test failure?

**Answer:**

Use the `Scenario` object's `attach` method in an `@After` hook:

```java
@After
public void afterScenario(Scenario scenario) {
    if (scenario.isFailed()) {
        // Capture screenshot as byte array
        byte[] screenshot = ((TakesScreenshot) DriverManager.getDriver())
                .getScreenshotAs(OutputType.BYTES);

        // Attach to the scenario (appears in HTML/JSON reports)
        scenario.attach(screenshot, "image/png",
                "Failure_" + scenario.getName());
    }
    DriverManager.quitDriver();
}
```

**Parameters of `scenario.attach()`:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `data` | `byte[]` | The screenshot bytes |
| `mediaType` | `String` | MIME type (`"image/png"`) |
| `name` | `String` | Display name in the report |

**For step-level screenshots (every step):**

```java
@AfterStep
public void afterStep(Scenario scenario) {
    byte[] screenshot = ((TakesScreenshot) DriverManager.getDriver())
            .getScreenshotAs(OutputType.BYTES);
    scenario.attach(screenshot, "image/png", "Step_Screenshot");
}
```

---

## CI/CD Integration

### Q19: How do you run Cucumber tests in a CI/CD pipeline?

**Answer:**

**Jenkins Pipeline:**

```groovy
pipeline {
    agent any
    parameters {
        string(name: 'TAGS', defaultValue: '@smoke', description: 'Cucumber tags')
        choice(name: 'ENV', choices: ['qa', 'staging'], description: 'Environment')
    }
    stages {
        stage('Checkout') {
            steps { git branch: 'main', url: 'https://github.com/org/bdd-tests.git' }
        }
        stage('Test') {
            steps {
                sh """mvn clean test \
                    -Denv=${params.ENV} \
                    -Dcucumber.filter.tags="${params.TAGS}" \
                    -Dbrowser=chrome-headless"""
            }
        }
        stage('Report') {
            steps {
                cucumber buildStatus: 'UNSTABLE',
                    fileIncludePattern: '**/cucumber.json'
            }
        }
    }
    post {
        always { archiveArtifacts artifacts: 'target/cucumber-reports/**' }
    }
}
```

**GitHub Actions:**

```yaml
name: BDD Tests
on:
  push:
    branches: [main]
  workflow_dispatch:
    inputs:
      tags:
        description: 'Cucumber tags'
        default: '@smoke'

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-java@v4
        with: { java-version: '17', distribution: 'temurin' }
      - run: mvn clean test -Dcucumber.filter.tags="${{ github.event.inputs.tags || '@smoke' }}"
      - uses: actions/upload-artifact@v4
        if: always()
        with:
          name: cucumber-report
          path: target/cucumber-reports/
```

**Key CI/CD practices:**
1. Use headless browsers (`--headless=new`)
2. Pass tags via pipeline parameters for flexible execution
3. Always archive reports as build artifacts
4. Use JUnit XML output for native CI test result parsing
5. Trigger smoke tests on every commit, full regression nightly

---

## Advanced Topics

### Q20: What is the difference between Cucumber Hooks and TestNG @Before/@After?

**Answer:**

| Aspect | Cucumber Hooks | TestNG Annotations |
|--------|---------------|-------------------|
| **Defined in** | Any class in the `glue` package | Test class or base class |
| **Annotation** | `@Before`, `@After` (io.cucumber.java) | `@BeforeMethod`, `@AfterMethod` (org.testng) |
| **Scope** | Cucumber scenario lifecycle | TestNG test method lifecycle |
| **Tag filtering** | `@Before("@smoke")` | Not natively supported |
| **Scenario access** | `Scenario` parameter for name, status, attach | `ITestResult` via TestNG listeners |
| **Ordering** | `order` attribute (0 = first) | `dependsOnMethods` or priority |
| **Step-level** | `@BeforeStep`, `@AfterStep` available | No equivalent |

**Important:** When using Cucumber with TestNG, use **Cucumber hooks** for scenario setup/teardown, not TestNG annotations. TestNG is just the runner; Cucumber manages the lifecycle.

---

### Q21: How do you implement parallel execution with Cucumber?

**Answer:**

**With TestNG runner:**

```java
@CucumberOptions(features = "src/test/resources/features",
                 glue = {"com.steps", "com.hooks"})
public class TestRunner extends AbstractTestNGCucumberTests {

    @Override
    @DataProvider(parallel = true)
    public Object[][] scenarios() {
        return super.scenarios();
    }
}
```

**testng.xml:**

```xml
<suite name="BDD" parallel="methods" thread-count="4">
    <test name="Cucumber">
        <classes>
            <class name="com.framework.runner.TestRunner"/>
        </classes>
    </test>
</suite>
```

**Prerequisites for parallel execution:**
1. **ThreadLocal WebDriver** -- each thread gets its own browser
2. **No shared mutable state** -- step definition classes should not use static fields for test data
3. **Independent scenarios** -- no scenario depends on another's outcome
4. **Thread-safe page objects** -- each scenario creates its own page object instances

---

### Q22: What is a Doc String in Cucumber? When do you use it?

**Answer:**

A **Doc String** is a multi-line text block delimited by triple quotes (`"""`). It is used to pass large text payloads (JSON, XML, messages) to a step.

```gherkin
Scenario: Create a user via API
  Given the API is available
  When I send a POST request to "/api/users" with body:
    """json
    {
      "name": "John Doe",
      "email": "john@example.com",
      "role": "admin"
    }
    """
  Then the response status should be 201
```

```java
@When("I send a POST request to {string} with body:")
public void sendPostRequest(String endpoint, String body) {
    response = RestAssured.given()
            .header("Content-Type", "application/json")
            .body(body)
            .post(endpoint);
}
```

**Use cases:**
- JSON payloads for API tests
- XML request bodies
- Email templates to verify
- SQL queries to execute
- Long text comparisons

---

### Q23: How do you handle test data management in a Cucumber BDD framework?

**Answer:**

| Approach | When to Use | Example |
|----------|------------|---------|
| **Inline in feature files** | Small, static data | `When the user enters "admin"` |
| **Examples table** | Data-driven scenarios | `Scenario Outline` with `Examples` |
| **Data Tables** | Structured input to a step | Registration form fields |
| **Doc Strings** | Large payloads | JSON request bodies |
| **Properties files** | Environment-specific config | URLs, timeouts |
| **Excel / CSV** | Large external data sets | Read in `@Before` or DataProvider |
| **Database** | Dynamic, current data | Query in step definition |
| **API calls** | Generate test data at runtime | Create user via API in `Given` step |
| **Faker library** | Random realistic data | `new Faker().name().fullName()` |

**Best practice:** Keep feature files readable by business stakeholders. If data is complex, abstract it:

```gherkin
# GOOD (declarative)
Given a standard user account exists

# AVOID (too much data detail in feature file)
Given a user with name "John", email "john@test.com", role "user",
      status "active", plan "premium", created "2024-01-01"
```

Move the data details into the step definition or a test data factory.

---

### Q24: What is the cucumber.properties file and what can you configure in it?

**Answer:**

`cucumber.properties` is placed in `src/test/resources/` and provides default configuration values.

```properties
# Publish to Cucumber Reports cloud service
cucumber.publish.enabled=true
cucumber.publish.quiet=true

# Default tags (overridden by runner or CLI)
cucumber.filter.tags=@smoke

# Snippet style for undefined steps
cucumber.snippet-type=camelcase

# Object factory (for dependency injection)
cucumber.object-factory=io.cucumber.picocontainer.PicoFactory

# Execution mode
cucumber.execution.parallel.enabled=true
cucumber.execution.parallel.config.fixed.parallelism=4
```

**Priority order (highest to lowest):**
1. Command-line system properties (`-Dcucumber.filter.tags=...`)
2. `@CucumberOptions` in the runner class
3. `cucumber.properties` file
4. Built-in defaults

---

### Q25: How do you organise a large-scale Cucumber project?

**Answer:**

**Feature files by domain:**
```
features/
├── authentication/
│   ├── login.feature
│   ├── registration.feature
│   └── password_reset.feature
├── products/
│   ├── search.feature
│   ├── catalog.feature
│   └── reviews.feature
├── orders/
│   ├── checkout.feature
│   ├── payment.feature
│   └── order_history.feature
└── api/
    ├── user_api.feature
    └── product_api.feature
```

**Step definitions by domain:**
```
stepdefinitions/
├── auth/
│   ├── LoginSteps.java
│   └── RegistrationSteps.java
├── products/
│   ├── SearchSteps.java
│   └── CatalogSteps.java
├── orders/
│   └── CheckoutSteps.java
├── api/
│   └── UserApiSteps.java
└── common/
    ├── NavigationSteps.java
    └── ValidationSteps.java
```

**Multiple runners:**
```
runner/
├── SmokeRunner.java          (@smoke)
├── RegressionRunner.java     (@regression and not @wip)
├── ApiRunner.java            (@api)
└── NightlyRunner.java        (all scenarios)
```

**Key principles:**
1. Group feature files by business domain, not by page
2. Share common steps via a `common` package
3. Use dependency injection (PicoContainer) to share state between step classes
4. Keep step definitions thin -- delegate logic to page objects and service classes
5. Use tags extensively for organisation and selective execution
6. Maintain a consistent naming convention across the team

---

## Quick Reference Card

| Topic | Key Point | Interview Tip |
|-------|----------|---------------|
| BDD vs TDD | BDD = behaviour, TDD = code correctness | Mention Three Amigos and living documentation |
| Gherkin | Given = Arrange, When = Act, Then = Assert | Emphasise declarative over imperative style |
| Feature files | One per feature, user story format | Mention best practices and independence |
| Step definitions | Cucumber expressions match steps to Java methods | Know `{string}`, `{int}`, `{word}` types |
| Scenario Outline | Template + Examples for data-driven tests | Can have multiple tagged Examples blocks |
| Hooks | `@Before`/`@After` per scenario; `@BeforeAll`/`@AfterAll` per suite | Know the execution order and `order` attribute |
| Tags | Filter scenarios: `@smoke and not @wip` | Show CLI override with `-Dcucumber.filter.tags` |
| Selenium integration | ThreadLocal driver + POM + Hooks | Explain why ThreadLocal for parallelism |
| Sharing state | PicoContainer DI or static ThreadLocal | PicoContainer is the Cucumber-recommended approach |
| Data Tables | Map, List of Maps, List of Lists | Know all four formats |
| Reports | JSON + masterthought or Allure for advanced | Mention screenshot attachment via `scenario.attach()` |
| CI/CD | Headless browser, tag-based filtering, artifact archiving | Show a pipeline snippet |
| Parallel | `@DataProvider(parallel = true)` + ThreadLocal | Prerequisites: independent scenarios, no shared state |
| Doc Strings | Triple-quote blocks for multi-line data | Common for API JSON payloads |
| cucumber.properties | Default config, overridden by CLI | Know the priority order |

> **Preparation Tip:** In interviews, always relate BDD concepts back to real project experience. Describe how your team used feature files for requirement discussions, how you organised a large test suite with tags, and how you integrated Cucumber into CI/CD pipelines.
