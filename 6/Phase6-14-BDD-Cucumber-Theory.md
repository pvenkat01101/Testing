# Phase 6-14: BDD with Cucumber - Theory

## Table of Contents
1. [BDD Fundamentals](#1-bdd-fundamentals)
2. [Gherkin Language](#2-gherkin-language)
3. [Feature Files](#3-feature-files)
4. [Scenarios and Scenario Outlines](#4-scenarios-and-scenario-outlines)
5. [Background, Data Tables, and Examples](#5-background-data-tables-and-examples)
6. [Tags](#6-tags)
7. [Cucumber Setup with Java](#7-cucumber-setup-with-java)
8. [Step Definitions](#8-step-definitions)
9. [Runner Class](#9-runner-class)
10. [Hooks (@Before, @After)](#10-hooks-before-after)
11. [Cucumber with Selenium Integration](#11-cucumber-with-selenium-integration)
12. [Cucumber with TestNG/JUnit](#12-cucumber-with-testng-junit)
13. [Cucumber with REST Assured](#13-cucumber-with-rest-assured)
14. [Cucumber Reports](#14-cucumber-reports)

---

## 1. BDD Fundamentals

### What is BDD?

**Behaviour-Driven Development (BDD)** is a software development methodology that bridges the gap between business stakeholders, developers, and testers by using a shared, natural-language format to describe application behaviour.

BDD was introduced by **Dan North** in 2003 as an evolution of Test-Driven Development (TDD).

### Core Principles

| Principle | Description |
|-----------|-------------|
| **Collaboration** | Business, dev, and QA write specifications together |
| **Ubiquitous Language** | Everyone uses the same vocabulary (no jargon) |
| **Living Documentation** | Feature files serve as always-up-to-date requirements |
| **Outside-In Development** | Start from expected behaviour, then implement |
| **Executable Specifications** | Gherkin scenarios are runnable automated tests |

### BDD vs TDD

| Aspect | TDD | BDD |
|--------|-----|-----|
| **Focus** | Code correctness | Business behaviour |
| **Written by** | Developers | Business + Dev + QA |
| **Language** | Programming language (JUnit, TestNG) | Natural language (Gherkin) |
| **Scope** | Unit / method level | Feature / user story level |
| **Format** | Test methods with assertions | Given-When-Then scenarios |
| **Documentation** | Code comments and test names | Feature files readable by anyone |

### Benefits of BDD

1. **Improved communication** -- non-technical stakeholders can read and validate scenarios
2. **Clear requirements** -- ambiguity is resolved before coding starts
3. **Living documentation** -- feature files stay in sync with the application
4. **Better test coverage** -- scenarios are derived from real user stories
5. **Faster feedback** -- executable specs catch misunderstandings early
6. **Reusable step definitions** -- common steps are shared across features

### The Three Amigos

BDD encourages a **Three Amigos** meeting before each user story:

- **Business Analyst / Product Owner** -- describes what the feature should do
- **Developer** -- discusses how it will be implemented
- **Tester** -- identifies edge cases and test scenarios

The output of this meeting is a set of Gherkin scenarios that everyone agrees on.

---

## 2. Gherkin Language

### What is Gherkin?

**Gherkin** is the structured, plain-text language used to write BDD scenarios. It uses a set of keywords to define features, scenarios, and steps.

### Gherkin Keywords

| Keyword | Purpose | Example |
|---------|---------|---------|
| `Feature` | High-level description of a feature | `Feature: User Login` |
| `Scenario` | A single test case | `Scenario: Valid login` |
| `Scenario Outline` | A parameterised scenario template | `Scenario Outline: Login with <role>` |
| `Given` | Precondition / initial state | `Given the user is on the login page` |
| `When` | Action performed by the user | `When the user enters valid credentials` |
| `Then` | Expected outcome | `Then the dashboard should be displayed` |
| `And` | Additional step (same type as preceding keyword) | `And the welcome message is shown` |
| `But` | Negative additional step | `But the error count should be zero` |
| `Background` | Steps common to all scenarios in a feature | `Background: User is logged in` |
| `Examples` | Data table for Scenario Outline | `Examples:` followed by a table |
| `@tag` | Metadata for filtering scenarios | `@smoke @login` |
| `#` | Comment | `# This is a comment` |
| `"""` | Doc string (multi-line text) | Used for large text blocks |
| `\|` | Data table delimiter | `| username | password |` |

### Given-When-Then Explained

```gherkin
Given  --> ARRANGE  (set up the preconditions)
When   --> ACT      (perform the action under test)
Then   --> ASSERT   (verify the expected outcome)
```

**Rules for writing good steps:**
- Write in **third person** or **first person** consistently
- Each step should do **one thing**
- Avoid implementation details (no "click button", "enter text") -- describe behaviour
- Use **declarative** style over imperative:

```gherkin
# BAD (imperative -- too detailed)
Given the user opens Chrome browser
And the user navigates to "https://example.com/login"
And the user enters "admin" in the username field
And the user enters "pass123" in the password field
And the user clicks the Login button

# GOOD (declarative -- behaviour-focused)
Given the user is on the login page
When the user logs in with valid credentials
Then the dashboard is displayed
```

---

## 3. Feature Files

### Structure of a Feature File

A feature file has the extension `.feature` and lives in `src/test/resources/features/`.

```gherkin
@login
Feature: User Login
  As a registered user
  I want to log in to the application
  So that I can access my account

  Background:
    Given the application is accessible

  Scenario: Successful login with valid credentials
    Given the user is on the login page
    When the user enters username "tomsmith" and password "SuperSecretPassword!"
    Then the user should be redirected to the secure area
    And the success message should contain "You logged into a secure area!"

  Scenario: Failed login with invalid password
    Given the user is on the login page
    When the user enters username "tomsmith" and password "wrongpassword"
    Then an error message should be displayed
    And the error message should contain "Your password is invalid!"
```

### Feature File Best Practices

| Practice | Explanation |
|----------|-------------|
| One feature per file | Keeps files focused and manageable |
| Descriptive feature name | `Feature: User Login` not `Feature: Test1` |
| User story format | `As a... I want to... So that...` for context |
| Independent scenarios | Each scenario must run in isolation |
| Reusable steps | Write steps that can be shared across features |
| Meaningful tags | `@smoke`, `@regression`, `@login` for filtering |

---

## 4. Scenarios and Scenario Outlines

### Scenario

A **Scenario** is a single concrete example of a feature's behaviour.

```gherkin
Scenario: Add item to cart
  Given the user is on the product page for "Laptop"
  When the user clicks "Add to Cart"
  Then the cart should contain 1 item
  And the cart total should be "$999.99"
```

### Scenario Outline

A **Scenario Outline** is a template that runs multiple times with different data from an `Examples` table.

```gherkin
Scenario Outline: Login with different credentials
  Given the user is on the login page
  When the user enters username "<username>" and password "<password>"
  Then the result should be "<result>"

  Examples:
    | username   | password              | result              |
    | tomsmith   | SuperSecretPassword!  | Login successful     |
    | tomsmith   | wrongpassword         | Invalid password     |
    | invalidusr | SuperSecretPassword!  | Invalid username     |
    |            | SuperSecretPassword!  | Username required    |
    | tomsmith   |                       | Password required    |
```

### When to Use Each

| Use Case | Use Scenario | Use Scenario Outline |
|----------|-------------|---------------------|
| Single example | Yes | No |
| Multiple data sets, same steps | No | Yes |
| Different steps per case | Yes (separate scenarios) | No |
| Data-driven testing | No | Yes |

---

## 5. Background, Data Tables, and Examples

### Background

The `Background` keyword defines steps that run **before every scenario** in the feature file. It replaces duplicated `Given` steps.

```gherkin
Feature: Shopping Cart

  Background:
    Given the user is logged in
    And the user has an empty cart

  Scenario: Add single item
    When the user adds "Laptop" to the cart
    Then the cart should contain 1 item

  Scenario: Add multiple items
    When the user adds "Laptop" to the cart
    And the user adds "Mouse" to the cart
    Then the cart should contain 2 items
```

**Background rules:**
- Placed after `Feature` and before the first `Scenario`
- Runs before **every** scenario (not once per file)
- Keep it short -- only truly common steps
- Do not use it for complex setup; prefer hooks for technical setup

### Data Tables

Data tables pass structured data to a single step.

```gherkin
Scenario: Register a new user
  Given the user is on the registration page
  When the user fills in the registration form:
    | field     | value              |
    | firstName | John               |
    | lastName  | Doe                |
    | email     | john.doe@test.com  |
    | password  | SecurePass123      |
  Then the account should be created successfully
```

**Step definition for a data table:**

```java
@When("the user fills in the registration form:")
public void fillRegistrationForm(DataTable dataTable) {
    Map<String, String> data = dataTable.asMap(String.class, String.class);
    registrationPage.enterFirstName(data.get("firstName"));
    registrationPage.enterLastName(data.get("lastName"));
    registrationPage.enterEmail(data.get("email"));
    registrationPage.enterPassword(data.get("password"));
    registrationPage.submit();
}
```

**List of Maps (multiple rows):**

```gherkin
Scenario: Add multiple products
  When the user adds the following products:
    | name   | quantity | price  |
    | Laptop | 1        | 999.99 |
    | Mouse  | 2        | 29.99  |
    | Keyboard | 1      | 79.99  |
  Then the cart total should be "$1139.96"
```

```java
@When("the user adds the following products:")
public void addProducts(DataTable dataTable) {
    List<Map<String, String>> products = dataTable.asMaps(String.class, String.class);
    for (Map<String, String> product : products) {
        cartPage.addProduct(
            product.get("name"),
            Integer.parseInt(product.get("quantity")),
            Double.parseDouble(product.get("price"))
        );
    }
}
```

### Examples (with Scenario Outline)

`Examples` provide the data rows for a `Scenario Outline`. You can have **multiple Examples blocks** with different tags:

```gherkin
Scenario Outline: Search for products
  Given the user is on the search page
  When the user searches for "<query>"
  Then "<count>" results should be displayed

  @smoke
  Examples: Popular searches
    | query    | count |
    | laptop   | 50    |
    | phone    | 120   |

  @regression
  Examples: Edge cases
    | query    | count |
    | ""       | 0     |
    | xyz123   | 0     |
```

---

## 6. Tags

### What are Tags?

Tags are labels prefixed with `@` that allow you to **filter** which scenarios to run.

```gherkin
@smoke @login
Feature: User Login

  @positive
  Scenario: Successful login
    ...

  @negative
  Scenario: Failed login
    ...
```

### Tag Expressions

| Expression | Meaning |
|-----------|---------|
| `@smoke` | Run scenarios tagged `@smoke` |
| `@smoke and @login` | Both tags must be present |
| `@smoke or @regression` | Either tag |
| `not @wip` | Exclude work-in-progress |
| `@smoke and not @slow` | Smoke tests that are not slow |
| `(@smoke or @regression) and not @skip` | Combined expression |

### Using Tags in the Runner

```java
@CucumberOptions(
    features = "src/test/resources/features",
    glue = "com.framework.stepdefinitions",
    tags = "@smoke and not @wip"
)
```

### Common Tag Conventions

| Tag | Purpose |
|-----|---------|
| `@smoke` | Quick sanity tests |
| `@regression` | Full regression suite |
| `@wip` | Work in progress (excluded from CI) |
| `@manual` | Cannot be automated yet |
| `@api` | API-level tests |
| `@ui` | UI/Selenium tests |
| `@slow` | Long-running tests |
| `@bug-1234` | Linked to a bug tracker issue |

---

## 7. Cucumber Setup with Java

### Maven Dependencies

```xml
<properties>
    <cucumber.version>7.14.0</cucumber.version>
    <java.version>17</java.version>
</properties>

<dependencies>
    <!-- Cucumber Core -->
    <dependency>
        <groupId>io.cucumber</groupId>
        <artifactId>cucumber-java</artifactId>
        <version>${cucumber.version}</version>
    </dependency>

    <!-- Cucumber with TestNG -->
    <dependency>
        <groupId>io.cucumber</groupId>
        <artifactId>cucumber-testng</artifactId>
        <version>${cucumber.version}</version>
    </dependency>

    <!-- OR Cucumber with JUnit 4 -->
    <!--
    <dependency>
        <groupId>io.cucumber</groupId>
        <artifactId>cucumber-junit</artifactId>
        <version>${cucumber.version}</version>
    </dependency>
    -->

    <!-- Selenium WebDriver -->
    <dependency>
        <groupId>org.seleniumhq.selenium</groupId>
        <artifactId>selenium-java</artifactId>
        <version>4.15.0</version>
    </dependency>

    <!-- WebDriverManager -->
    <dependency>
        <groupId>io.github.bonigarcia</groupId>
        <artifactId>webdrivermanager</artifactId>
        <version>5.6.2</version>
    </dependency>
</dependencies>

<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>3.2.2</version>
            <configuration>
                <includes>
                    <include>**/TestRunner.java</include>
                </includes>
            </configuration>
        </plugin>
    </plugins>
</build>
```

### Project Structure

```
project-root/
├── pom.xml
├── src/
│   ├── main/java/com/framework/
│   │   ├── pages/              # Page Object classes
│   │   │   ├── LoginPage.java
│   │   │   └── DashboardPage.java
│   │   └── utils/              # Utility classes
│   │       ├── DriverManager.java
│   │       └── ConfigReader.java
│   └── test/
│       ├── java/com/framework/
│       │   ├── stepdefinitions/ # Step definition classes
│       │   │   ├── LoginSteps.java
│       │   │   └── CommonSteps.java
│       │   ├── hooks/          # Cucumber hooks
│       │   │   └── Hooks.java
│       │   └── runner/         # Test runner classes
│       │       └── TestRunner.java
│       └── resources/
│           ├── features/       # Feature files
│           │   ├── login.feature
│           │   └── cart.feature
│           ├── config.properties
│           └── cucumber.properties
```

---

## 8. Step Definitions

### What are Step Definitions?

Step definitions are Java methods that **map Gherkin steps to executable code**. Cucumber matches each step in a feature file to a step definition using regular expressions or Cucumber expressions.

### Cucumber Expressions vs Regular Expressions

**Cucumber Expressions (recommended):**

```java
@Given("the user is on the login page")
public void userOnLoginPage() {
    driver.get("https://example.com/login");
}

@When("the user enters username {string} and password {string}")
public void enterCredentials(String username, String password) {
    loginPage.enterUsername(username);
    loginPage.enterPassword(password);
    loginPage.clickLogin();
}

@Then("the cart should contain {int} item(s)")
public void verifyCartCount(int count) {
    Assert.assertEquals(cartPage.getItemCount(), count);
}
```

**Regular Expressions:**

```java
@When("^the user enters username \"([^\"]*)\" and password \"([^\"]*)\"$")
public void enterCredentials(String username, String password) {
    // same implementation
}
```

### Parameter Types

| Cucumber Expression | Matches | Java Type |
|--------------------|---------|-----------|
| `{string}` | Quoted string (`"..."` or `'...'`) | `String` |
| `{int}` | Integer | `int` |
| `{float}` | Decimal number | `float` |
| `{word}` | Single word (no spaces) | `String` |
| `{bigdecimal}` | Large decimal | `BigDecimal` |
| `{}` | Any type (anonymous) | `String` |

### Custom Parameter Types

```java
@ParameterType("enabled|disabled")
public Boolean status(String status) {
    return "enabled".equals(status);
}

// Usage in step:
@Then("the feature should be {status}")
public void verifyFeature(Boolean isEnabled) {
    Assert.assertEquals(featurePage.isEnabled(), isEnabled);
}
```

### Step Definition Best Practices

1. **One step definition class per feature area** (LoginSteps, CartSteps, etc.)
2. **Reuse steps** across features -- do not duplicate
3. **Keep steps thin** -- delegate to page objects or service classes
4. **Use dependency injection** (PicoContainer or Spring) to share state between step classes
5. **Avoid coupling** -- steps should not call other steps

---

## 9. Runner Class

### TestNG Runner

```java
package com.framework.runner;

import io.cucumber.testng.AbstractTestNGCucumberTests;
import io.cucumber.testng.CucumberOptions;
import org.testng.annotations.DataProvider;

@CucumberOptions(
    features = "src/test/resources/features",
    glue = {"com.framework.stepdefinitions", "com.framework.hooks"},
    tags = "@smoke",
    plugin = {
        "pretty",
        "html:target/cucumber-reports/cucumber.html",
        "json:target/cucumber-reports/cucumber.json",
        "junit:target/cucumber-reports/cucumber.xml",
        "io.qameta.allure.cucumber7jvm.AllureCucumber7Jvm"
    },
    monochrome = true,
    dryRun = false
)
public class TestRunner extends AbstractTestNGCucumberTests {

    @Override
    @DataProvider(parallel = true)
    public Object[][] scenarios() {
        return super.scenarios();
    }
}
```

### JUnit Runner

```java
package com.framework.runner;

import io.cucumber.junit.Cucumber;
import io.cucumber.junit.CucumberOptions;
import org.junit.runner.RunWith;

@RunWith(Cucumber.class)
@CucumberOptions(
    features = "src/test/resources/features",
    glue = {"com.framework.stepdefinitions", "com.framework.hooks"},
    tags = "@smoke",
    plugin = {
        "pretty",
        "html:target/cucumber-reports/cucumber.html",
        "json:target/cucumber-reports/cucumber.json"
    },
    monochrome = true,
    dryRun = false
)
public class TestRunner {
    // No body needed
}
```

### CucumberOptions Explained

| Option | Purpose | Example |
|--------|---------|---------|
| `features` | Path to `.feature` files | `"src/test/resources/features"` |
| `glue` | Packages containing step definitions and hooks | `{"com.steps", "com.hooks"}` |
| `tags` | Tag expression to filter scenarios | `"@smoke and not @wip"` |
| `plugin` | Report formatters | `"html:target/report.html"` |
| `monochrome` | Clean console output (no ANSI colours) | `true` |
| `dryRun` | Validate step mappings without executing | `true` (for development) |
| `publish` | Publish to Cucumber Reports service | `true` |
| `snippets` | Snippet style for undefined steps | `SnippetType.CAMELCASE` |

---

## 10. Hooks (@Before, @After)

### What are Hooks?

Hooks are methods that run **before and after each scenario**. They are defined in any class within the `glue` path.

### Hook Types

```java
package com.framework.hooks;

import io.cucumber.java.After;
import io.cucumber.java.AfterAll;
import io.cucumber.java.AfterStep;
import io.cucumber.java.Before;
import io.cucumber.java.BeforeAll;
import io.cucumber.java.BeforeStep;
import io.cucumber.java.Scenario;

public class Hooks {

    @BeforeAll
    public static void beforeAll() {
        // Runs ONCE before the entire suite (static method)
        System.out.println("Starting Cucumber suite...");
    }

    @Before
    public void before(Scenario scenario) {
        // Runs before EACH scenario
        System.out.println("Starting scenario: " + scenario.getName());
        DriverManager.createDriver();
    }

    @Before("@api")
    public void beforeApi() {
        // Runs only before scenarios tagged @api
        System.out.println("Setting up API context...");
    }

    @BeforeStep
    public void beforeStep() {
        // Runs before EACH step (rarely used)
    }

    @AfterStep
    public void afterStep(Scenario scenario) {
        // Runs after EACH step -- useful for step-level screenshots
        if (scenario.isFailed()) {
            takeScreenshot(scenario);
        }
    }

    @After
    public void after(Scenario scenario) {
        // Runs after EACH scenario
        if (scenario.isFailed()) {
            takeScreenshot(scenario);
        }
        DriverManager.quitDriver();
        System.out.println("Finished scenario: " + scenario.getName()
                + " | Status: " + scenario.getStatus());
    }

    @AfterAll
    public static void afterAll() {
        // Runs ONCE after the entire suite (static method)
        System.out.println("Cucumber suite complete.");
    }

    private void takeScreenshot(Scenario scenario) {
        byte[] screenshot = ((TakesScreenshot) DriverManager.getDriver())
                .getScreenshotAs(OutputType.BYTES);
        scenario.attach(screenshot, "image/png", scenario.getName());
    }
}
```

### Hook Execution Order

```
@BeforeAll           (once)
  @Before            (each scenario)
    @BeforeStep      (each step)
      Step 1
    @AfterStep       (each step)
    @BeforeStep
      Step 2
    @AfterStep
    ...
  @After             (each scenario)
@AfterAll            (once)
```

### Hook Order Attribute

When multiple hooks exist, control the order:

```java
@Before(order = 0)   // runs first (lowest number = first)
public void setupDriver() { ... }

@Before(order = 1)   // runs second
public void navigateToApp() { ... }

@After(order = 1)    // runs first in @After (reverse order)
public void takeScreenshot() { ... }

@After(order = 0)    // runs last in @After
public void closeDriver() { ... }
```

---

## 11. Cucumber with Selenium Integration

### DriverManager (Shared WebDriver)

```java
package com.framework.utils;

import io.github.bonigarcia.wdm.WebDriverManager;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.chrome.ChromeOptions;

public class DriverManager {

    private static ThreadLocal<WebDriver> driverThread = new ThreadLocal<>();

    public static void createDriver() {
        WebDriverManager.chromedriver().setup();
        ChromeOptions options = new ChromeOptions();
        options.addArguments("--start-maximized");
        WebDriver driver = new ChromeDriver(options);
        driverThread.set(driver);
    }

    public static WebDriver getDriver() {
        return driverThread.get();
    }

    public static void quitDriver() {
        WebDriver driver = driverThread.get();
        if (driver != null) {
            driver.quit();
            driverThread.remove();
        }
    }
}
```

### Page Object with Cucumber

```java
package com.framework.pages;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.support.FindBy;
import org.openqa.selenium.support.PageFactory;

public class LoginPage {

    private WebDriver driver;

    @FindBy(id = "username")
    private WebElement usernameField;

    @FindBy(id = "password")
    private WebElement passwordField;

    @FindBy(css = "button[type='submit']")
    private WebElement loginButton;

    @FindBy(id = "flash")
    private WebElement flashMessage;

    public LoginPage(WebDriver driver) {
        this.driver = driver;
        PageFactory.initElements(driver, this);
    }

    public void navigateToLoginPage() {
        driver.get("https://the-internet.herokuapp.com/login");
    }

    public void enterCredentials(String username, String password) {
        usernameField.clear();
        usernameField.sendKeys(username);
        passwordField.clear();
        passwordField.sendKeys(password);
    }

    public void clickLogin() {
        loginButton.click();
    }

    public String getFlashMessage() {
        return flashMessage.getText();
    }
}
```

### Step Definitions Using Page Objects

```java
package com.framework.stepdefinitions;

import com.framework.pages.LoginPage;
import com.framework.utils.DriverManager;
import io.cucumber.java.en.Given;
import io.cucumber.java.en.Then;
import io.cucumber.java.en.When;
import org.testng.Assert;

public class LoginSteps {

    private LoginPage loginPage;

    @Given("the user is on the login page")
    public void userOnLoginPage() {
        loginPage = new LoginPage(DriverManager.getDriver());
        loginPage.navigateToLoginPage();
    }

    @When("the user enters username {string} and password {string}")
    public void enterCredentials(String username, String password) {
        loginPage.enterCredentials(username, password);
        loginPage.clickLogin();
    }

    @Then("the success message should contain {string}")
    public void verifySuccessMessage(String expectedMessage) {
        Assert.assertTrue(loginPage.getFlashMessage().contains(expectedMessage),
                "Expected message not found: " + expectedMessage);
    }

    @Then("the error message should contain {string}")
    public void verifyErrorMessage(String expectedMessage) {
        Assert.assertTrue(loginPage.getFlashMessage().contains(expectedMessage),
                "Expected error not found: " + expectedMessage);
    }
}
```

---

## 12. Cucumber with TestNG / JUnit

### Cucumber + TestNG

```java
// Runner extends AbstractTestNGCucumberTests
@CucumberOptions(/* ... */)
public class TestRunner extends AbstractTestNGCucumberTests {

    @Override
    @DataProvider(parallel = true)
    public Object[][] scenarios() {
        return super.scenarios();
    }
}
```

**testng.xml for Cucumber:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd">
<suite name="BDD Suite" parallel="methods" thread-count="4">
    <test name="Cucumber Tests">
        <classes>
            <class name="com.framework.runner.TestRunner"/>
        </classes>
    </test>
</suite>
```

### Cucumber + JUnit 4

```java
@RunWith(Cucumber.class)
@CucumberOptions(/* ... */)
public class TestRunner {
    // JUnit discovers and runs the scenarios
}
```

### Cucumber + JUnit 5 (JUnit Platform)

```xml
<dependency>
    <groupId>io.cucumber</groupId>
    <artifactId>cucumber-junit-platform-engine</artifactId>
    <version>${cucumber.version}</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.junit.platform</groupId>
    <artifactId>junit-platform-suite</artifactId>
    <version>1.10.1</version>
    <scope>test</scope>
</dependency>
```

```java
@Suite
@IncludeEngines("cucumber")
@SelectPackages("com.framework")
@ConfigurationParameter(key = GLUE_PROPERTY_NAME,
        value = "com.framework.stepdefinitions,com.framework.hooks")
@ConfigurationParameter(key = FEATURES_PROPERTY_NAME,
        value = "src/test/resources/features")
public class TestRunner {
}
```

---

## 13. Cucumber with REST Assured

### Feature File for API Testing

```gherkin
@api
Feature: User API

  Scenario: Get user by ID
    Given the API base URL is "https://jsonplaceholder.typicode.com"
    When I send a GET request to "/users/1"
    Then the response status code should be 200
    And the response body should contain "Leanne Graham"

  Scenario: Create a new user
    Given the API base URL is "https://jsonplaceholder.typicode.com"
    When I send a POST request to "/users" with body:
      """
      {
        "name": "John Doe",
        "username": "johndoe",
        "email": "john@example.com"
      }
      """
    Then the response status code should be 201
    And the response body should contain "johndoe"
```

### API Step Definitions

```java
package com.framework.stepdefinitions;

import io.cucumber.java.en.Given;
import io.cucumber.java.en.Then;
import io.cucumber.java.en.When;
import io.restassured.RestAssured;
import io.restassured.response.Response;
import io.restassured.specification.RequestSpecification;

import static org.testng.Assert.assertEquals;
import static org.testng.Assert.assertTrue;

public class ApiSteps {

    private RequestSpecification request;
    private Response response;

    @Given("the API base URL is {string}")
    public void setBaseUrl(String baseUrl) {
        RestAssured.baseURI = baseUrl;
        request = RestAssured.given()
                .header("Content-Type", "application/json");
    }

    @When("I send a GET request to {string}")
    public void sendGet(String endpoint) {
        response = request.get(endpoint);
    }

    @When("I send a POST request to {string} with body:")
    public void sendPost(String endpoint, String body) {
        response = request.body(body).post(endpoint);
    }

    @Then("the response status code should be {int}")
    public void verifyStatusCode(int expectedCode) {
        assertEquals(response.getStatusCode(), expectedCode);
    }

    @Then("the response body should contain {string}")
    public void verifyResponseBody(String expectedText) {
        assertTrue(response.getBody().asString().contains(expectedText),
                "Response does not contain: " + expectedText);
    }
}
```

---

## 14. Cucumber Reports

### Built-in Reports (via plugin option)

| Plugin | Output | Use Case |
|--------|--------|----------|
| `pretty` | Coloured console output | Local development |
| `html:target/cucumber.html` | Simple HTML report | Quick review |
| `json:target/cucumber.json` | JSON report | Input for other report tools |
| `junit:target/cucumber.xml` | JUnit XML | CI/CD integration (Jenkins, etc.) |
| `timeline:target/timeline` | Timeline HTML | Visualise parallel execution |

### Cucumber Reports Service

```java
@CucumberOptions(
    plugin = {"pretty", "html:target/cucumber.html"},
    publish = true   // publishes to https://reports.cucumber.io
)
```

Or set via properties file `src/test/resources/cucumber.properties`:

```properties
cucumber.publish.enabled=true
cucumber.publish.quiet=true
```

### Generating Advanced Reports with Maven Cucumber Reporting

```xml
<dependency>
    <groupId>net.masterthought</groupId>
    <artifactId>cucumber-reporting</artifactId>
    <version>5.7.7</version>
</dependency>
```

```java
package com.framework.reports;

import net.masterthought.cucumber.Configuration;
import net.masterthought.cucumber.ReportBuilder;

import java.io.File;
import java.util.ArrayList;
import java.util.List;

public class CucumberReportGenerator {

    public static void generateReport() {
        File reportOutputDirectory = new File("target/cucumber-advanced-report");
        List<String> jsonFiles = new ArrayList<>();
        jsonFiles.add("target/cucumber-reports/cucumber.json");

        Configuration config = new Configuration(reportOutputDirectory, "My Project");
        config.addClassifications("Environment", "QA");
        config.addClassifications("Browser", "Chrome");
        config.addClassifications("Branch", "main");

        ReportBuilder builder = new ReportBuilder(jsonFiles, config);
        builder.generateReports();
    }
}
```

### Report Generation in Hooks

```java
@AfterAll
public static void afterAll() {
    CucumberReportGenerator.generateReport();
}
```

---

## Summary Table

| Topic | Key Concept | Remember |
|-------|------------|----------|
| BDD | Behaviour-focused collaboration | Three Amigos, living documentation |
| Gherkin | Given-When-Then | Declarative style, not imperative |
| Feature files | One feature per `.feature` file | User story format for context |
| Scenario Outline | Parameterised template + Examples | Replaces duplicate scenarios |
| Background | Shared preconditions | Runs before every scenario |
| Data Tables | Structured data in steps | `DataTable` in Java |
| Tags | Filtering and organisation | `@smoke`, `@regression`, `not @wip` |
| Step Definitions | Gherkin-to-code mapping | Cucumber expressions preferred |
| Runner | `@CucumberOptions` configuration | `glue`, `features`, `tags`, `plugin` |
| Hooks | `@Before`, `@After`, `@BeforeAll` | Screenshot on failure in `@After` |
| Selenium | ThreadLocal driver + POM | Same patterns as non-BDD framework |
| REST Assured | API step definitions | Doc strings for JSON bodies |
| Reports | Built-in + masterthought + Allure | JSON output feeds advanced reports |

> **Next Step:** Phase 6-15 covers hands-on BDD Cucumber practicals with complete exercises.
