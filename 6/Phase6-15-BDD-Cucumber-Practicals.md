# Phase 6-15: BDD with Cucumber - Practicals

## Table of Contents
1. [Creating Feature Files for Login Scenarios](#1-creating-feature-files-for-login-scenarios)
2. [Writing Step Definitions](#2-writing-step-definitions)
3. [Configuring the Runner Class](#3-configuring-the-runner-class)
4. [Scenario Outline with Examples](#4-scenario-outline-with-examples)
5. [Data Tables](#5-data-tables)
6. [Tags for Test Organization](#6-tags-for-test-organization)
7. [Hooks Implementation](#7-hooks-implementation)
8. [Integrating Cucumber with Selenium WebDriver](#8-integrating-cucumber-with-selenium-webdriver)
9. [Complete BDD Framework with POM](#9-complete-bdd-framework-with-pom)
10. [Generating Cucumber Reports](#10-generating-cucumber-reports)

---

## 1. Creating Feature Files for Login Scenarios

### Exercise 1.1: Basic Login Feature File

Create `src/test/resources/features/login.feature`:

```gherkin
@login
Feature: User Login Functionality
  As a registered user of the application
  I want to be able to log in with my credentials
  So that I can access my personal dashboard

  Background:
    Given the user navigates to the login page

  @smoke @positive
  Scenario: Successful login with valid credentials
    When the user enters username "tomsmith" and password "SuperSecretPassword!"
    And the user clicks the login button
    Then the user should be redirected to the secure area
    And the page should display a success message containing "You logged into a secure area!"

  @negative
  Scenario: Login fails with invalid username
    When the user enters username "invaliduser" and password "SuperSecretPassword!"
    And the user clicks the login button
    Then the user should remain on the login page
    And the page should display an error message containing "Your username is invalid!"

  @negative
  Scenario: Login fails with invalid password
    When the user enters username "tomsmith" and password "wrongpassword"
    And the user clicks the login button
    Then the user should remain on the login page
    And the page should display an error message containing "Your password is invalid!"

  @negative
  Scenario: Login fails with empty credentials
    When the user clicks the login button
    Then the user should remain on the login page
    And the page should display an error message containing "Your username is invalid!"
```

### Exercise 1.2: E-Commerce Login Feature

Create `src/test/resources/features/ecommerce_login.feature`:

```gherkin
@ecommerce @login
Feature: E-Commerce Login
  As an online shopper
  I want to log in to my account
  So that I can view my orders and manage my profile

  Background:
    Given the user is on the e-commerce home page
    And the user clicks on the "Sign In" link

  @smoke
  Scenario: Login with registered email and password
    When the user enters email "buyer@test.com"
    And the user enters password "Test@1234"
    And the user clicks the "Sign In" button
    Then the user should see "Welcome, Test User" on the page
    And the "My Account" link should be visible

  @security
  Scenario: Account locks after 3 failed attempts
    When the user enters email "buyer@test.com"
    And the user enters wrong password 3 times
    Then the account should be locked
    And the message "Your account has been locked" should be displayed

  @negative
  Scenario: Login with unregistered email
    When the user enters email "unknown@test.com"
    And the user enters password "AnyPassword1"
    And the user clicks the "Sign In" button
    Then the error message "No account found with this email" should be displayed
```

---

## 2. Writing Step Definitions

### Exercise 2.1: Login Step Definitions

Create `src/test/java/com/framework/stepdefinitions/LoginSteps.java`:

```java
package com.framework.stepdefinitions;

import com.framework.pages.LoginPage;
import com.framework.pages.SecureAreaPage;
import com.framework.utils.DriverManager;
import io.cucumber.java.en.And;
import io.cucumber.java.en.Given;
import io.cucumber.java.en.Then;
import io.cucumber.java.en.When;
import org.openqa.selenium.WebDriver;
import org.testng.Assert;

public class LoginSteps {

    private WebDriver driver = DriverManager.getDriver();
    private LoginPage loginPage;
    private SecureAreaPage secureAreaPage;

    @Given("the user navigates to the login page")
    public void navigateToLoginPage() {
        loginPage = new LoginPage(driver);
        loginPage.navigateTo();
    }

    @When("the user enters username {string} and password {string}")
    public void enterCredentials(String username, String password) {
        loginPage.enterUsername(username);
        loginPage.enterPassword(password);
    }

    @When("the user clicks the login button")
    public void clickLoginButton() {
        loginPage.clickLogin();
    }

    @Then("the user should be redirected to the secure area")
    public void verifyRedirectToSecureArea() {
        secureAreaPage = new SecureAreaPage(driver);
        Assert.assertTrue(secureAreaPage.isPageLoaded(),
                "User was not redirected to the secure area");
    }

    @Then("the page should display a success message containing {string}")
    public void verifySuccessMessage(String expectedMessage) {
        String actualMessage = secureAreaPage.getFlashMessage();
        Assert.assertTrue(actualMessage.contains(expectedMessage),
                "Expected: '" + expectedMessage + "' but got: '" + actualMessage + "'");
    }

    @Then("the user should remain on the login page")
    public void verifyStillOnLoginPage() {
        Assert.assertTrue(loginPage.isLoginPageDisplayed(),
                "User is not on the login page");
    }

    @Then("the page should display an error message containing {string}")
    public void verifyErrorMessage(String expectedError) {
        String actualError = loginPage.getErrorMessage();
        Assert.assertTrue(actualError.contains(expectedError),
                "Expected error: '" + expectedError + "' but got: '" + actualError + "'");
    }
}
```

### Exercise 2.2: Common / Shared Step Definitions

Create `src/test/java/com/framework/stepdefinitions/CommonSteps.java`:

```java
package com.framework.stepdefinitions;

import com.framework.utils.DriverManager;
import io.cucumber.java.en.Given;
import io.cucumber.java.en.Then;
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.testng.Assert;

public class CommonSteps {

    private WebDriver driver = DriverManager.getDriver();

    @Given("the user is on the {string} page")
    public void navigateToPage(String pageName) {
        String baseUrl = "https://the-internet.herokuapp.com";
        switch (pageName.toLowerCase()) {
            case "login":
                driver.get(baseUrl + "/login");
                break;
            case "home":
                driver.get(baseUrl);
                break;
            case "dropdown":
                driver.get(baseUrl + "/dropdown");
                break;
            default:
                throw new IllegalArgumentException("Unknown page: " + pageName);
        }
    }

    @Then("the page title should be {string}")
    public void verifyPageTitle(String expectedTitle) {
        Assert.assertEquals(driver.getTitle(), expectedTitle);
    }

    @Then("the URL should contain {string}")
    public void verifyUrlContains(String expectedText) {
        Assert.assertTrue(driver.getCurrentUrl().contains(expectedText),
                "URL does not contain: " + expectedText);
    }

    @Then("the text {string} should be visible on the page")
    public void verifyTextVisible(String text) {
        boolean isVisible = driver.findElement(
                By.xpath("//*[contains(text(),'" + text + "')]")).isDisplayed();
        Assert.assertTrue(isVisible, "Text not visible: " + text);
    }
}
```

---

## 3. Configuring the Runner Class

### Exercise 3.1: TestNG Runner

Create `src/test/java/com/framework/runner/TestRunner.java`:

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
        "junit:target/cucumber-reports/cucumber.xml"
    },
    monochrome = true,
    dryRun = false
)
public class TestRunner extends AbstractTestNGCucumberTests {

    /**
     * Override the scenarios DataProvider to enable parallel execution.
     * Each scenario runs in its own thread.
     */
    @Override
    @DataProvider(parallel = true)
    public Object[][] scenarios() {
        return super.scenarios();
    }
}
```

### Exercise 3.2: Separate Runners per Tag

**Smoke Runner:**

```java
@CucumberOptions(
    features = "src/test/resources/features",
    glue = {"com.framework.stepdefinitions", "com.framework.hooks"},
    tags = "@smoke",
    plugin = {
        "pretty",
        "json:target/cucumber-reports/smoke-cucumber.json"
    }
)
public class SmokeRunner extends AbstractTestNGCucumberTests {}
```

**Regression Runner:**

```java
@CucumberOptions(
    features = "src/test/resources/features",
    glue = {"com.framework.stepdefinitions", "com.framework.hooks"},
    tags = "@regression and not @wip",
    plugin = {
        "pretty",
        "json:target/cucumber-reports/regression-cucumber.json"
    }
)
public class RegressionRunner extends AbstractTestNGCucumberTests {}
```

### Exercise 3.3: Dry Run for Step Validation

```java
@CucumberOptions(
    features = "src/test/resources/features",
    glue = {"com.framework.stepdefinitions"},
    dryRun = true,   // only checks for missing step definitions
    monochrome = true
)
public class DryRunValidator extends AbstractTestNGCucumberTests {}
```

Running this will output snippets for any undefined steps, for example:

```
You can implement missing steps with the snippets below:

@Given("the user is on the e-commerce home page")
public void the_user_is_on_the_e_commerce_home_page() {
    // Write code here that turns the phrase above into concrete actions
    throw new io.cucumber.java.PendingException();
}
```

---

## 4. Scenario Outline with Examples

### Exercise 4.1: Login with Multiple Credentials

Create `src/test/resources/features/login_data_driven.feature`:

```gherkin
@login @data-driven
Feature: Data-Driven Login Testing

  Scenario Outline: Login with various credential combinations
    Given the user navigates to the login page
    When the user enters username "<username>" and password "<password>"
    And the user clicks the login button
    Then the login result should be "<result>"
    And the message should contain "<message>"

    @smoke
    Examples: Valid credentials
      | username | password             | result  | message                             |
      | tomsmith | SuperSecretPassword! | success | You logged into a secure area!      |

    @regression
    Examples: Invalid credentials
      | username    | password             | result  | message                  |
      | tomsmith    | wrongpassword        | failure | Your password is invalid!|
      | invaliduser | SuperSecretPassword! | failure | Your username is invalid!|
      |             | SuperSecretPassword! | failure | Your username is invalid!|
      | tomsmith    |                      | failure | Your password is invalid!|

    @regression
    Examples: Special characters
      | username       | password    | result  | message                  |
      | tom<script>    | pass123     | failure | Your username is invalid!|
      | tomsmith       | p@ss w0rd   | failure | Your password is invalid!|
```

### Exercise 4.2: Step Definitions for Scenario Outline

```java
package com.framework.stepdefinitions;

import com.framework.pages.LoginPage;
import com.framework.pages.SecureAreaPage;
import com.framework.utils.DriverManager;
import io.cucumber.java.en.Then;
import org.testng.Assert;

public class DataDrivenLoginSteps {

    // Reuse LoginSteps for Given/When; only new Then steps here

    @Then("the login result should be {string}")
    public void verifyLoginResult(String expectedResult) {
        if (expectedResult.equals("success")) {
            SecureAreaPage securePage = new SecureAreaPage(DriverManager.getDriver());
            Assert.assertTrue(securePage.isPageLoaded(),
                    "Expected successful login but page did not load");
        } else {
            LoginPage loginPage = new LoginPage(DriverManager.getDriver());
            Assert.assertTrue(loginPage.isLoginPageDisplayed(),
                    "Expected to remain on login page");
        }
    }

    @Then("the message should contain {string}")
    public void verifyMessage(String expectedMessage) {
        String currentUrl = DriverManager.getDriver().getCurrentUrl();
        String actualMessage;

        if (currentUrl.contains("/secure")) {
            actualMessage = new SecureAreaPage(DriverManager.getDriver()).getFlashMessage();
        } else {
            actualMessage = new LoginPage(DriverManager.getDriver()).getErrorMessage();
        }

        Assert.assertTrue(actualMessage.contains(expectedMessage),
                "Expected message containing '" + expectedMessage
                        + "' but got: '" + actualMessage + "'");
    }
}
```

### Exercise 4.3: Search Feature with Scenario Outline

```gherkin
@search
Feature: Product Search

  Scenario Outline: Search products by category
    Given the user is on the search page
    When the user selects category "<category>"
    And the user enters search term "<searchTerm>"
    And the user clicks search
    Then the results count should be greater than <minResults>
    And the first result should contain "<expectedText>"

    Examples:
      | category    | searchTerm | minResults | expectedText |
      | Electronics | laptop     | 5          | Laptop       |
      | Books       | java       | 3          | Java         |
      | Clothing    | shirt      | 10         | Shirt        |
```

---

## 5. Data Tables

### Exercise 5.1: Registration with Data Table

Create `src/test/resources/features/registration.feature`:

```gherkin
@registration
Feature: User Registration

  @smoke
  Scenario: Register a new user with all required fields
    Given the user is on the registration page
    When the user fills in the registration form with:
      | Field       | Value              |
      | First Name  | John               |
      | Last Name   | Doe                |
      | Email       | john.doe@test.com  |
      | Password    | SecurePass@123     |
      | Confirm     | SecurePass@123     |
      | Phone       | +1-555-0100        |
    And the user accepts the terms and conditions
    And the user clicks the Register button
    Then the registration should be successful
    And a confirmation email should be sent to "john.doe@test.com"
```

**Step Definition:**

```java
@When("the user fills in the registration form with:")
public void fillRegistrationForm(DataTable dataTable) {
    Map<String, String> formData = dataTable.asMap(String.class, String.class);

    registrationPage.enterFirstName(formData.get("First Name"));
    registrationPage.enterLastName(formData.get("Last Name"));
    registrationPage.enterEmail(formData.get("Email"));
    registrationPage.enterPassword(formData.get("Password"));
    registrationPage.enterConfirmPassword(formData.get("Confirm"));
    registrationPage.enterPhone(formData.get("Phone"));
}
```

### Exercise 5.2: Adding Multiple Items to Cart (List of Maps)

```gherkin
@cart
Feature: Shopping Cart

  Scenario: Add multiple products to cart
    Given the user is logged in
    When the user adds the following products to the cart:
      | productName | quantity | size   | color |
      | T-Shirt     | 2        | Medium | Blue  |
      | Jeans       | 1        | Large  | Black |
      | Sneakers    | 1        | 10     | White |
    Then the cart should contain 3 items
    And the cart summary should show:
      | productName | quantity | subtotal |
      | T-Shirt     | 2        | $39.98   |
      | Jeans       | 1        | $59.99   |
      | Sneakers    | 1        | $89.99   |
```

**Step Definition:**

```java
@When("the user adds the following products to the cart:")
public void addProductsToCart(DataTable dataTable) {
    List<Map<String, String>> products = dataTable.asMaps(String.class, String.class);

    for (Map<String, String> product : products) {
        String name     = product.get("productName");
        int quantity    = Integer.parseInt(product.get("quantity"));
        String size     = product.get("size");
        String color    = product.get("color");

        productPage.searchProduct(name);
        productPage.selectSize(size);
        productPage.selectColor(color);
        productPage.setQuantity(quantity);
        productPage.clickAddToCart();

        System.out.println("Added to cart: " + name
                + " (qty=" + quantity + ", size=" + size + ", color=" + color + ")");
    }
}

@Then("the cart summary should show:")
public void verifyCartSummary(DataTable dataTable) {
    List<Map<String, String>> expectedItems = dataTable.asMaps(String.class, String.class);

    for (Map<String, String> item : expectedItems) {
        String productName     = item.get("productName");
        String expectedQty     = item.get("quantity");
        String expectedSubtotal = item.get("subtotal");

        Assert.assertEquals(cartPage.getQuantity(productName), expectedQty);
        Assert.assertEquals(cartPage.getSubtotal(productName), expectedSubtotal);
    }
}
```

### Exercise 5.3: Simple List Data Table

```gherkin
Scenario: Verify navigation menu items
  Given the user is on the home page
  Then the navigation menu should contain the following items:
    | Home       |
    | Products   |
    | About Us   |
    | Contact    |
    | My Account |
```

**Step Definition:**

```java
@Then("the navigation menu should contain the following items:")
public void verifyMenuItems(List<String> expectedItems) {
    List<String> actualItems = homePage.getNavigationMenuItems();
    Assert.assertEquals(actualItems, expectedItems,
            "Navigation menu items do not match");
}
```

---

## 6. Tags for Test Organization

### Exercise 6.1: Feature File with Tags

```gherkin
@ecommerce
Feature: Order Management

  @smoke @order
  Scenario: Place a new order
    Given the user has items in the cart
    When the user proceeds to checkout
    And the user completes payment
    Then the order should be placed successfully

  @regression @order
  Scenario: Cancel an existing order
    Given the user has a pending order "ORD-001"
    When the user cancels the order
    Then the order status should be "Cancelled"

  @regression @order @refund
  Scenario: Refund a cancelled order
    Given the user has a cancelled order "ORD-002"
    When the user requests a refund
    Then the refund should be processed within 5 business days

  @wip
  Scenario: Track order shipment
    Given the user has a shipped order "ORD-003"
    When the user clicks "Track Order"
    Then the shipment tracking details should be displayed
```

### Exercise 6.2: Running with Different Tag Combinations

**Runner for smoke tests only:**

```java
@CucumberOptions(
    features = "src/test/resources/features",
    glue = {"com.framework.stepdefinitions", "com.framework.hooks"},
    tags = "@smoke"
)
public class SmokeRunner extends AbstractTestNGCucumberTests {}
```

**Runner for regression excluding work in progress:**

```java
@CucumberOptions(
    features = "src/test/resources/features",
    glue = {"com.framework.stepdefinitions", "com.framework.hooks"},
    tags = "@regression and not @wip"
)
public class RegressionRunner extends AbstractTestNGCucumberTests {}
```

**Maven command-line tag override:**

```bash
# Run only smoke tests
mvn clean test -Dcucumber.filter.tags="@smoke"

# Run order-related regression tests
mvn clean test -Dcucumber.filter.tags="@regression and @order"

# Exclude WIP and slow tests
mvn clean test -Dcucumber.filter.tags="not @wip and not @slow"

# Run either smoke or regression
mvn clean test -Dcucumber.filter.tags="@smoke or @regression"
```

### Exercise 6.3: Tag-Based Hook Execution

```java
@Before("@ui")
public void setupBrowser() {
    DriverManager.createDriver();
}

@Before("@api")
public void setupApiContext() {
    RestAssured.baseURI = ConfigReader.getProperty("api.base.url");
}

@After("@ui")
public void closeBrowser(Scenario scenario) {
    if (scenario.isFailed()) {
        byte[] screenshot = ((TakesScreenshot) DriverManager.getDriver())
                .getScreenshotAs(OutputType.BYTES);
        scenario.attach(screenshot, "image/png", "failure-screenshot");
    }
    DriverManager.quitDriver();
}

@After("@api")
public void logApiResponse() {
    System.out.println("API scenario completed");
}
```

---

## 7. Hooks Implementation

### Exercise 7.1: Complete Hooks Class

Create `src/test/java/com/framework/hooks/Hooks.java`:

```java
package com.framework.hooks;

import com.framework.utils.ConfigReader;
import com.framework.utils.DriverManager;
import io.cucumber.java.After;
import io.cucumber.java.AfterAll;
import io.cucumber.java.AfterStep;
import io.cucumber.java.Before;
import io.cucumber.java.BeforeAll;
import io.cucumber.java.Scenario;
import org.openqa.selenium.OutputType;
import org.openqa.selenium.TakesScreenshot;

import java.text.SimpleDateFormat;
import java.util.Date;

public class Hooks {

    // ============================
    // SUITE-LEVEL HOOKS
    // ============================

    @BeforeAll
    public static void globalSetup() {
        System.out.println("================================================");
        System.out.println("  BDD Test Suite Starting");
        System.out.println("  Environment: " + ConfigReader.getProperty("env", "qa"));
        System.out.println("  Time: " + new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(new Date()));
        System.out.println("================================================");
    }

    @AfterAll
    public static void globalTeardown() {
        System.out.println("================================================");
        System.out.println("  BDD Test Suite Complete");
        System.out.println("  Time: " + new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(new Date()));
        System.out.println("================================================");
    }

    // ============================
    // SCENARIO-LEVEL HOOKS
    // ============================

    @Before(order = 0)
    public void initializeDriver(Scenario scenario) {
        System.out.println("\n--- Starting Scenario: " + scenario.getName() + " ---");
        System.out.println("    Tags: " + scenario.getSourceTagNames());
        DriverManager.createDriver();
    }

    @Before(value = "@db", order = 1)
    public void setupDatabase() {
        System.out.println("    [Hook] Setting up test data in database...");
        // DatabaseUtil.seedTestData();
    }

    @After(order = 1)
    public void captureFailureEvidence(Scenario scenario) {
        if (scenario.isFailed()) {
            System.out.println("    [Hook] Scenario FAILED -- capturing screenshot");
            try {
                byte[] screenshot = ((TakesScreenshot) DriverManager.getDriver())
                        .getScreenshotAs(OutputType.BYTES);
                scenario.attach(screenshot, "image/png",
                        "Failure_" + scenario.getName().replaceAll(" ", "_"));
            } catch (Exception e) {
                System.err.println("    [Hook] Could not capture screenshot: " + e.getMessage());
            }
        }
    }

    @After(order = 0)
    public void closeDriver(Scenario scenario) {
        System.out.println("--- Finished Scenario: " + scenario.getName()
                + " | Status: " + scenario.getStatus() + " ---\n");
        DriverManager.quitDriver();
    }

    @After(value = "@db", order = 2)
    public void cleanDatabase() {
        System.out.println("    [Hook] Cleaning up test data from database...");
        // DatabaseUtil.cleanTestData();
    }

    // ============================
    // STEP-LEVEL HOOKS
    // ============================

    @AfterStep
    public void afterEachStep(Scenario scenario) {
        // Optional: capture screenshot after every step for a step-by-step report
        if (ConfigReader.getBooleanProperty("screenshot.every.step", false)) {
            try {
                byte[] screenshot = ((TakesScreenshot) DriverManager.getDriver())
                        .getScreenshotAs(OutputType.BYTES);
                scenario.attach(screenshot, "image/png", "Step_Screenshot");
            } catch (Exception ignored) {
                // non-critical
            }
        }
    }
}
```

### Exercise 7.2: Hooks Execution Order Diagram

```
SUITE START
  |
  +-- @BeforeAll (globalSetup)
  |
  +-- SCENARIO 1
  |     +-- @Before(order=0) --> initializeDriver
  |     +-- @Before(order=1) --> setupDatabase (if @db tag)
  |     |
  |     +-- Step 1
  |     |     +-- @AfterStep --> afterEachStep
  |     +-- Step 2
  |     |     +-- @AfterStep --> afterEachStep
  |     +-- Step N
  |     |     +-- @AfterStep --> afterEachStep
  |     |
  |     +-- @After(order=2) --> cleanDatabase (if @db tag)
  |     +-- @After(order=1) --> captureFailureEvidence
  |     +-- @After(order=0) --> closeDriver
  |
  +-- SCENARIO 2
  |     +-- (same sequence)
  |
  +-- @AfterAll (globalTeardown)
  |
SUITE END
```

---

## 8. Integrating Cucumber with Selenium WebDriver

### Exercise 8.1: Thread-Safe DriverManager

Create `src/main/java/com/framework/utils/DriverManager.java`:

```java
package com.framework.utils;

import io.github.bonigarcia.wdm.WebDriverManager;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.openqa.selenium.firefox.FirefoxDriver;
import org.openqa.selenium.firefox.FirefoxOptions;
import org.openqa.selenium.edge.EdgeDriver;

import java.time.Duration;

public class DriverManager {

    private static ThreadLocal<WebDriver> driverThreadLocal = new ThreadLocal<>();

    public static void createDriver() {
        String browser = ConfigReader.getProperty("browser", "chrome");
        WebDriver driver;

        switch (browser.toLowerCase()) {
            case "chrome":
                WebDriverManager.chromedriver().setup();
                ChromeOptions chromeOpts = new ChromeOptions();
                chromeOpts.addArguments("--start-maximized");
                chromeOpts.addArguments("--disable-notifications");
                if (ConfigReader.getBooleanProperty("headless", false)) {
                    chromeOpts.addArguments("--headless=new");
                    chromeOpts.addArguments("--window-size=1920,1080");
                }
                driver = new ChromeDriver(chromeOpts);
                break;

            case "firefox":
                WebDriverManager.firefoxdriver().setup();
                FirefoxOptions ffOpts = new FirefoxOptions();
                if (ConfigReader.getBooleanProperty("headless", false)) {
                    ffOpts.addArguments("--headless");
                }
                driver = new FirefoxDriver(ffOpts);
                driver.manage().window().maximize();
                break;

            case "edge":
                WebDriverManager.edgedriver().setup();
                driver = new EdgeDriver();
                driver.manage().window().maximize();
                break;

            default:
                throw new RuntimeException("Unsupported browser: " + browser);
        }

        int implicitWait = ConfigReader.getIntProperty("implicit.wait", 10);
        driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(implicitWait));

        driverThreadLocal.set(driver);
    }

    public static WebDriver getDriver() {
        return driverThreadLocal.get();
    }

    public static void quitDriver() {
        WebDriver driver = driverThreadLocal.get();
        if (driver != null) {
            driver.quit();
            driverThreadLocal.remove();
        }
    }
}
```

### Exercise 8.2: Page Object Classes

**LoginPage.java:**

```java
package com.framework.pages;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.support.FindBy;
import org.openqa.selenium.support.PageFactory;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;

import java.time.Duration;

public class LoginPage {

    private WebDriver driver;
    private WebDriverWait wait;

    @FindBy(id = "username")
    private WebElement usernameField;

    @FindBy(id = "password")
    private WebElement passwordField;

    @FindBy(css = "button[type='submit']")
    private WebElement loginButton;

    @FindBy(id = "flash")
    private WebElement flashMessage;

    @FindBy(css = "h2")
    private WebElement pageHeading;

    public LoginPage(WebDriver driver) {
        this.driver = driver;
        this.wait = new WebDriverWait(driver, Duration.ofSeconds(10));
        PageFactory.initElements(driver, this);
    }

    public void navigateTo() {
        driver.get("https://the-internet.herokuapp.com/login");
        wait.until(ExpectedConditions.visibilityOf(loginButton));
    }

    public void enterUsername(String username) {
        usernameField.clear();
        usernameField.sendKeys(username);
    }

    public void enterPassword(String password) {
        passwordField.clear();
        passwordField.sendKeys(password);
    }

    public void clickLogin() {
        loginButton.click();
    }

    public String getErrorMessage() {
        wait.until(ExpectedConditions.visibilityOf(flashMessage));
        return flashMessage.getText();
    }

    public boolean isLoginPageDisplayed() {
        return pageHeading.getText().contains("Login Page");
    }
}
```

**SecureAreaPage.java:**

```java
package com.framework.pages;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.support.FindBy;
import org.openqa.selenium.support.PageFactory;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;

import java.time.Duration;

public class SecureAreaPage {

    private WebDriver driver;
    private WebDriverWait wait;

    @FindBy(id = "flash")
    private WebElement flashMessage;

    @FindBy(css = "h2")
    private WebElement heading;

    @FindBy(css = "a[href='/logout']")
    private WebElement logoutButton;

    public SecureAreaPage(WebDriver driver) {
        this.driver = driver;
        this.wait = new WebDriverWait(driver, Duration.ofSeconds(10));
        PageFactory.initElements(driver, this);
    }

    public boolean isPageLoaded() {
        try {
            wait.until(ExpectedConditions.visibilityOf(heading));
            return heading.getText().contains("Secure Area");
        } catch (Exception e) {
            return false;
        }
    }

    public String getFlashMessage() {
        wait.until(ExpectedConditions.visibilityOf(flashMessage));
        return flashMessage.getText();
    }

    public void clickLogout() {
        logoutButton.click();
    }
}
```

---

## 9. Complete BDD Framework with POM

### Exercise 9.1: Full Project Structure

```
bdd-framework/
├── pom.xml
├── src/
│   ├── main/java/com/framework/
│   │   ├── pages/
│   │   │   ├── BasePage.java
│   │   │   ├── LoginPage.java
│   │   │   ├── SecureAreaPage.java
│   │   │   ├── ProductPage.java
│   │   │   └── CartPage.java
│   │   └── utils/
│   │       ├── ConfigReader.java
│   │       ├── DriverManager.java
│   │       └── WaitUtils.java
│   └── test/
│       ├── java/com/framework/
│       │   ├── stepdefinitions/
│       │   │   ├── LoginSteps.java
│       │   │   ├── ProductSteps.java
│       │   │   ├── CartSteps.java
│       │   │   └── CommonSteps.java
│       │   ├── hooks/
│       │   │   └── Hooks.java
│       │   └── runner/
│       │       ├── TestRunner.java
│       │       ├── SmokeRunner.java
│       │       └── RegressionRunner.java
│       └── resources/
│           ├── features/
│           │   ├── login.feature
│           │   ├── login_data_driven.feature
│           │   ├── product.feature
│           │   └── cart.feature
│           ├── config/
│           │   ├── config-qa.properties
│           │   ├── config-staging.properties
│           │   └── config-prod.properties
│           └── cucumber.properties
```

### Exercise 9.2: BasePage (Abstract Page Object)

```java
package com.framework.pages;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.support.PageFactory;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;

import java.time.Duration;

public abstract class BasePage {

    protected WebDriver driver;
    protected WebDriverWait wait;

    public BasePage(WebDriver driver) {
        this.driver = driver;
        this.wait = new WebDriverWait(driver, Duration.ofSeconds(15));
        PageFactory.initElements(driver, this);
    }

    protected void click(WebElement element) {
        wait.until(ExpectedConditions.elementToBeClickable(element)).click();
    }

    protected void type(WebElement element, String text) {
        wait.until(ExpectedConditions.visibilityOf(element));
        element.clear();
        element.sendKeys(text);
    }

    protected String getText(WebElement element) {
        return wait.until(ExpectedConditions.visibilityOf(element)).getText();
    }

    protected boolean isDisplayed(WebElement element) {
        try {
            return wait.until(ExpectedConditions.visibilityOf(element)).isDisplayed();
        } catch (Exception e) {
            return false;
        }
    }

    protected void waitForUrl(String urlFragment) {
        wait.until(ExpectedConditions.urlContains(urlFragment));
    }

    public String getPageTitle() {
        return driver.getTitle();
    }

    public String getCurrentUrl() {
        return driver.getCurrentUrl();
    }
}
```

### Exercise 9.3: Complete pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                             http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.framework</groupId>
    <artifactId>bdd-cucumber-framework</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
        <cucumber.version>7.14.0</cucumber.version>
        <selenium.version>4.15.0</selenium.version>
        <testng.version>7.8.0</testng.version>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <dependencies>
        <!-- Cucumber -->
        <dependency>
            <groupId>io.cucumber</groupId>
            <artifactId>cucumber-java</artifactId>
            <version>${cucumber.version}</version>
        </dependency>
        <dependency>
            <groupId>io.cucumber</groupId>
            <artifactId>cucumber-testng</artifactId>
            <version>${cucumber.version}</version>
        </dependency>

        <!-- Selenium -->
        <dependency>
            <groupId>org.seleniumhq.selenium</groupId>
            <artifactId>selenium-java</artifactId>
            <version>${selenium.version}</version>
        </dependency>

        <!-- WebDriverManager -->
        <dependency>
            <groupId>io.github.bonigarcia</groupId>
            <artifactId>webdrivermanager</artifactId>
            <version>5.6.2</version>
        </dependency>

        <!-- TestNG -->
        <dependency>
            <groupId>org.testng</groupId>
            <artifactId>testng</artifactId>
            <version>${testng.version}</version>
            <scope>test</scope>
        </dependency>

        <!-- REST Assured (for API scenarios) -->
        <dependency>
            <groupId>io.rest-assured</groupId>
            <artifactId>rest-assured</artifactId>
            <version>5.3.2</version>
            <scope>test</scope>
        </dependency>

        <!-- Cucumber Reporting (advanced HTML reports) -->
        <dependency>
            <groupId>net.masterthought</groupId>
            <artifactId>cucumber-reporting</artifactId>
            <version>5.7.7</version>
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
                    <systemPropertyVariables>
                        <env>${env}</env>
                        <browser>${browser}</browser>
                    </systemPropertyVariables>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.11.0</version>
                <configuration>
                    <source>17</source>
                    <target>17</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

### Exercise 9.4: Running the Framework

```bash
# Run all smoke tests on QA environment
mvn clean test -Denv=qa -Dcucumber.filter.tags="@smoke"

# Run regression on staging with Firefox headless
mvn clean test -Denv=staging -Dbrowser=firefox -Dheadless=true \
    -Dcucumber.filter.tags="@regression and not @wip"

# Dry run to check for missing step definitions
mvn clean test -Dcucumber.options="--dry-run"
```

---

## 10. Generating Cucumber Reports

### Exercise 10.1: Built-in HTML and JSON Reports

Already configured in the runner:

```java
plugin = {
    "pretty",                                          // console
    "html:target/cucumber-reports/cucumber.html",      // basic HTML
    "json:target/cucumber-reports/cucumber.json",      // JSON (input for advanced)
    "junit:target/cucumber-reports/cucumber.xml"       // JUnit XML for CI
}
```

### Exercise 10.2: Advanced Reports with Maven Cucumber Reporting

```java
package com.framework.reports;

import net.masterthought.cucumber.Configuration;
import net.masterthought.cucumber.ReportBuilder;
import net.masterthought.cucumber.Reportable;
import net.masterthought.cucumber.json.support.Status;
import net.masterthought.cucumber.presentation.PresentationMode;

import java.io.File;
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class CucumberReportGenerator {

    public static void generateReport() {
        File reportOutputDir = new File("target/advanced-cucumber-report");

        List<String> jsonFiles = new ArrayList<>();
        jsonFiles.add("target/cucumber-reports/cucumber.json");

        String projectName = "BDD Automation Framework";

        Configuration config = new Configuration(reportOutputDir, projectName);

        // Optional metadata
        config.addClassifications("Environment",
                System.getProperty("env", "qa").toUpperCase());
        config.addClassifications("Browser",
                System.getProperty("browser", "chrome"));
        config.addClassifications("OS", System.getProperty("os.name"));
        config.addClassifications("Java", System.getProperty("java.version"));
        config.addClassifications("Branch",
                System.getProperty("branch", "main"));

        config.setNotFailingStatuses(Collections.singleton(Status.SKIPPED));
        config.addPresentationModes(PresentationMode.EXPAND_ALL_STEPS);

        ReportBuilder builder = new ReportBuilder(jsonFiles, config);
        Reportable result = builder.generateReports();

        // Print summary to console
        if (result != null) {
            System.out.println("\n===== CUCUMBER REPORT SUMMARY =====");
            System.out.println("Scenarios: Passed=" + result.getPassedScenarios()
                    + " Failed=" + result.getFailedScenarios());
            System.out.println("Steps:     Passed=" + result.getPassedSteps()
                    + " Failed=" + result.getFailedSteps()
                    + " Skipped=" + result.getSkippedSteps());
            System.out.println("Report:    " + reportOutputDir.getAbsolutePath()
                    + "/cucumber-html-reports/overview-features.html");
            System.out.println("====================================\n");
        }
    }
}
```

### Exercise 10.3: Trigger Report Generation in @AfterAll Hook

```java
@AfterAll
public static void globalTeardown() {
    CucumberReportGenerator.generateReport();
    System.out.println("BDD Suite Complete. Advanced report generated.");
}
```

### Exercise 10.4: Cucumber Reports Service (Cloud)

In `src/test/resources/cucumber.properties`:

```properties
cucumber.publish.enabled=true
cucumber.publish.quiet=true
```

After running tests, the console will display a URL:

```
View your Cucumber Report at:
https://reports.cucumber.io/reports/abc123-def456
```

This URL is shareable and lives for 24 hours (free tier).

### Exercise 10.5: Allure Reports with Cucumber

Add dependency:

```xml
<dependency>
    <groupId>io.qameta.allure</groupId>
    <artifactId>allure-cucumber7-jvm</artifactId>
    <version>2.25.0</version>
    <scope>test</scope>
</dependency>
```

Add plugin to runner:

```java
plugin = {
    "io.qameta.allure.cucumber7jvm.AllureCucumber7Jvm"
}
```

Generate the report:

```bash
mvn clean test
allure serve target/allure-results
```

---

## Summary Checklist

| Exercise | Topic | Key Takeaway |
|----------|-------|-------------|
| 1 | Feature files | Gherkin syntax, Background, tags, declarative style |
| 2 | Step definitions | Cucumber expressions, parameter types, page object delegation |
| 3 | Runner class | @CucumberOptions, parallel DataProvider, dry run |
| 4 | Scenario Outline | Examples table, multiple data sets, tagged Examples |
| 5 | Data Tables | Map, List of Maps, simple List -- structured input to steps |
| 6 | Tags | Filtering, Maven override, conditional hooks |
| 7 | Hooks | @Before/@After ordering, screenshots, @BeforeAll/@AfterAll |
| 8 | Selenium integration | ThreadLocal driver, POM with Cucumber |
| 9 | Complete framework | Full project structure, BasePage, pom.xml, CLI commands |
| 10 | Reports | Built-in, masterthought, Cucumber Reports cloud, Allure |

> **Next Step:** Phase 6-16 covers BDD Cucumber interview questions and answers.
