# Phase 5.2: Page Object Model (POM) - Theory

## Table of Contents
1. [Introduction to Page Object Model](#introduction-to-page-object-model)
2. [Why Page Object Model?](#why-page-object-model)
3. [POM Design Principles](#pom-design-principles)
4. [Basic Page Object Structure](#basic-page-object-structure)
5. [Page Factory Pattern](#page-factory-pattern)
6. [Advanced POM Concepts](#advanced-pom-concepts)
7. [POM Best Practices](#pom-best-practices)
8. [Common POM Patterns](#common-pom-patterns)

---

## 1. Introduction to Page Object Model

### What is Page Object Model?

**Page Object Model (POM)** is a design pattern in Selenium that creates an Object Repository for web UI elements. Each web page is represented as a class, and the elements on that page are defined as variables, with methods representing actions on those elements.

### Key Concepts:

- **Page Class**: Represents a web page
- **Page Elements**: WebElements as class variables
- **Page Methods**: Actions that can be performed on the page
- **Separation of Concerns**: Test logic separated from page structure

### Without POM (Bad Practice):

```java
public class LoginTest {
    @Test
    public void testLogin() {
        driver.get("https://example.com/login");
        driver.findElement(By.id("username")).sendKeys("testuser");
        driver.findElement(By.id("password")).sendKeys("password123");
        driver.findElement(By.id("loginButton")).click();

        String welcomeMessage = driver.findElement(By.id("welcome")).getText();
        assert welcomeMessage.contains("Welcome");
    }

    @Test
    public void testInvalidLogin() {
        driver.get("https://example.com/login");
        driver.findElement(By.id("username")).sendKeys("invaliduser");
        driver.findElement(By.id("password")).sendKeys("wrongpass");
        driver.findElement(By.id("loginButton")).click();

        String errorMessage = driver.findElement(By.id("error")).getText();
        assert errorMessage.contains("Invalid credentials");
    }
}
```

**Problems:**
- Locators duplicated across tests
- Hard to maintain when UI changes
- Test logic mixed with element location
- No reusability

### With POM (Good Practice):

```java
// LoginPage.java
public class LoginPage {
    WebDriver driver;

    // Locators
    By usernameField = By.id("username");
    By passwordField = By.id("password");
    By loginButton = By.id("loginButton");
    By welcomeMessage = By.id("welcome");
    By errorMessage = By.id("error");

    // Constructor
    public LoginPage(WebDriver driver) {
        this.driver = driver;
    }

    // Actions
    public void enterUsername(String username) {
        driver.findElement(usernameField).sendKeys(username);
    }

    public void enterPassword(String password) {
        driver.findElement(passwordField).sendKeys(password);
    }

    public void clickLogin() {
        driver.findElement(loginButton).click();
    }

    public String getWelcomeMessage() {
        return driver.findElement(welcomeMessage).getText();
    }

    public String getErrorMessage() {
        return driver.findElement(errorMessage).getText();
    }

    // Composite action
    public void login(String username, String password) {
        enterUsername(username);
        enterPassword(password);
        clickLogin();
    }
}

// LoginTest.java
public class LoginTest extends BaseTest {
    LoginPage loginPage;

    @BeforeMethod
    public void setup() {
        super.setup();
        driver.get("https://example.com/login");
        loginPage = new LoginPage(driver);
    }

    @Test
    public void testValidLogin() {
        loginPage.login("testuser", "password123");
        String message = loginPage.getWelcomeMessage();
        assert message.contains("Welcome");
    }

    @Test
    public void testInvalidLogin() {
        loginPage.login("invaliduser", "wrongpass");
        String error = loginPage.getErrorMessage();
        assert error.contains("Invalid credentials");
    }
}
```

**Benefits:**
- Locators centralized in page class
- Easy to maintain
- Test code more readable
- High reusability

---

## 2. Why Page Object Model?

### Advantages of POM:

#### 1. **Code Reusability**
```java
// LoginPage can be used across multiple test classes
public class LoginTest extends BaseTest {
    @Test
    public void testLogin() {
        LoginPage loginPage = new LoginPage(driver);
        loginPage.login("user", "pass");
    }
}

public class UserProfileTest extends BaseTest {
    @Test
    public void testProfileAccess() {
        LoginPage loginPage = new LoginPage(driver);
        loginPage.login("user", "pass");  // Reusing same method
        // Continue with profile tests
    }
}
```

#### 2. **Easy Maintenance**
When a locator changes, update it in one place:

```java
// Before: Element ID changed from "username" to "user-name"
// Only need to update in LoginPage class
By usernameField = By.id("user-name");  // Single update

// All tests using LoginPage automatically updated
// No need to modify test files
```

#### 3. **Improved Readability**
```java
// Without POM - Hard to read
driver.findElement(By.xpath("//input[@id='user']")).sendKeys("john");
driver.findElement(By.xpath("//input[@id='pass']")).sendKeys("secret");
driver.findElement(By.xpath("//button[@type='submit']")).click();

// With POM - Easy to read
loginPage.login("john", "secret");
```

#### 4. **Separation of Concerns**
- **Page Classes**: Know about page structure
- **Test Classes**: Know about test logic
- **Utility Classes**: Know about helper functions

#### 5. **Reduced Code Duplication**
```java
// Multiple tests can use the same login method
@Test
public void test1() {
    loginPage.login("user1", "pass1");
    // test logic
}

@Test
public void test2() {
    loginPage.login("user2", "pass2");
    // test logic
}
```

#### 6. **Better Collaboration**
- Developers can work on page classes
- Testers can focus on test scenarios
- Clear separation of responsibilities

### When to Use POM:

✅ **Use POM when:**
- Working on large projects
- Multiple team members
- Frequent UI changes expected
- Many test cases for same pages
- Long-term maintenance required

❌ **May skip POM when:**
- Very small projects (< 10 tests)
- Proof of concept
- One-time scripts
- Prototype testing

---

## 3. POM Design Principles

### SOLID Principles in POM:

#### 1. **Single Responsibility Principle (SRP)**
Each page class should represent one page only:

```java
// GOOD - Each page has its own class
public class LoginPage { }
public class HomePage { }
public class ProfilePage { }

// BAD - One class for multiple pages
public class AllPages {
    // Contains login, home, profile methods
}
```

#### 2. **Open/Closed Principle**
Page classes should be open for extension but closed for modification:

```java
// Base Page Class
public class BasePage {
    protected WebDriver driver;
    protected WebDriverWait wait;

    public BasePage(WebDriver driver) {
        this.driver = driver;
        this.wait = new WebDriverWait(driver, Duration.ofSeconds(10));
    }

    protected void click(By locator) {
        wait.until(ExpectedConditions.elementToBeClickable(locator)).click();
    }

    protected void type(By locator, String text) {
        WebElement element = wait.until(ExpectedConditions.visibilityOfElementLocated(locator));
        element.clear();
        element.sendKeys(text);
    }
}

// Specific Page extends BasePage
public class LoginPage extends BasePage {
    public LoginPage(WebDriver driver) {
        super(driver);
    }

    public void login(String username, String password) {
        type(usernameField, username);
        type(passwordField, password);
        click(loginButton);
    }
}
```

#### 3. **DRY (Don't Repeat Yourself)**
```java
// Create common methods in BasePage
public class BasePage {
    protected String getElementText(By locator) {
        return driver.findElement(locator).getText();
    }

    protected boolean isElementDisplayed(By locator) {
        try {
            return driver.findElement(locator).isDisplayed();
        } catch(NoSuchElementException e) {
            return false;
        }
    }
}

// All page classes can use these methods
public class HomePage extends BasePage {
    public String getWelcomeMessage() {
        return getElementText(welcomeMessageLocator);
    }

    public boolean isLogoutButtonVisible() {
        return isElementDisplayed(logoutButton);
    }
}
```

#### 4. **Meaningful Method Names**
```java
// GOOD - Clear and descriptive
public void clickLoginButton() { }
public void enterUsername(String username) { }
public boolean isErrorMessageDisplayed() { }
public String getWelcomeText() { }

// BAD - Vague and unclear
public void doLogin() { }
public void input(String text) { }
public boolean check() { }
public String get() { }
```

#### 5. **Return Page Objects for Navigation**
```java
public class LoginPage {
    public HomePage loginAsValidUser(String username, String password) {
        enterUsername(username);
        enterPassword(password);
        clickLogin();
        return new HomePage(driver);  // Return next page object
    }

    public LoginPage loginAsInvalidUser(String username, String password) {
        enterUsername(username);
        enterPassword(password);
        clickLogin();
        return this;  // Stay on same page
    }
}

// Usage in tests
@Test
public void testValidLogin() {
    LoginPage loginPage = new LoginPage(driver);
    HomePage homePage = loginPage.loginAsValidUser("user", "pass");
    // Continue with HomePage actions
    homePage.verifyUserLoggedIn();
}
```

---

## 4. Basic Page Object Structure

### Standard Page Object Structure:

```java
package pages;

import org.openqa.selenium.*;
import org.openqa.selenium.support.ui.*;
import java.time.Duration;

public class LoginPage {

    // 1. WebDriver instance
    private WebDriver driver;
    private WebDriverWait wait;

    // 2. Locators (By objects)
    private By usernameField = By.id("username");
    private By passwordField = By.id("password");
    private By loginButton = By.cssSelector("button[type='submit']");
    private By errorMessage = By.className("error-message");
    private By forgotPasswordLink = By.linkText("Forgot Password?");

    // 3. Constructor
    public LoginPage(WebDriver driver) {
        this.driver = driver;
        this.wait = new WebDriverWait(driver, Duration.ofSeconds(10));
    }

    // 4. Action Methods (Public)
    public void enterUsername(String username) {
        wait.until(ExpectedConditions.visibilityOfElementLocated(usernameField))
            .sendKeys(username);
    }

    public void enterPassword(String password) {
        wait.until(ExpectedConditions.visibilityOfElementLocated(passwordField))
            .sendKeys(password);
    }

    public void clickLogin() {
        wait.until(ExpectedConditions.elementToBeClickable(loginButton)).click();
    }

    public HomePage loginWith(String username, String password) {
        enterUsername(username);
        enterPassword(password);
        clickLogin();
        return new HomePage(driver);
    }

    // 5. Verification Methods
    public boolean isErrorMessageDisplayed() {
        try {
            return driver.findElement(errorMessage).isDisplayed();
        } catch(NoSuchElementException e) {
            return false;
        }
    }

    public String getErrorMessage() {
        return wait.until(ExpectedConditions.visibilityOfElementLocated(errorMessage))
                   .getText();
    }

    // 6. Navigation Methods
    public ForgotPasswordPage clickForgotPassword() {
        driver.findElement(forgotPasswordLink).click();
        return new ForgotPasswordPage(driver);
    }
}
```

### Page Object Layers:

```
┌─────────────────────────────────────┐
│        Test Layer                    │
│  (LoginTest, HomeTest, etc.)        │
└──────────────┬──────────────────────┘
               │ uses
┌──────────────▼──────────────────────┐
│        Page Object Layer            │
│  (LoginPage, HomePage, etc.)        │
└──────────────┬──────────────────────┘
               │ uses
┌──────────────▼──────────────────────┐
│        Base Page Layer              │
│  (Common methods, waits, etc.)      │
└──────────────┬──────────────────────┘
               │ uses
┌──────────────▼──────────────────────┐
│        WebDriver                     │
└─────────────────────────────────────┘
```

### Project Structure:

```
src/
├── main/
│   └── java/
│       ├── pages/
│       │   ├── BasePage.java
│       │   ├── LoginPage.java
│       │   ├── HomePage.java
│       │   └── ProfilePage.java
│       ├── utils/
│       │   ├── DriverFactory.java
│       │   └── ConfigReader.java
│       └── constants/
│           └── Constants.java
└── test/
    └── java/
        ├── tests/
        │   ├── BaseTest.java
        │   ├── LoginTest.java
        │   ├── HomeTest.java
        │   └── ProfileTest.java
        └── testdata/
            └── TestData.java
```

---

## 5. Page Factory Pattern

### What is Page Factory?

Page Factory is a class provided by Selenium that supports the Page Object Model. It uses annotations to initialize web elements when the page object is created.

### Without Page Factory:

```java
public class LoginPage {
    WebDriver driver;
    By usernameField = By.id("username");

    public LoginPage(WebDriver driver) {
        this.driver = driver;
    }

    public void enterUsername(String username) {
        driver.findElement(usernameField).sendKeys(username);
    }
}
```

### With Page Factory:

```java
import org.openqa.selenium.support.*;

public class LoginPage {
    WebDriver driver;

    // @FindBy annotation to locate element
    @FindBy(id = "username")
    private WebElement usernameField;

    @FindBy(id = "password")
    private WebElement passwordField;

    @FindBy(css = "button[type='submit']")
    private WebElement loginButton;

    // Constructor with PageFactory.initElements()
    public LoginPage(WebDriver driver) {
        this.driver = driver;
        PageFactory.initElements(driver, this);
    }

    // Use WebElement directly
    public void enterUsername(String username) {
        usernameField.sendKeys(username);
    }

    public void enterPassword(String password) {
        passwordField.sendKeys(password);
    }

    public void clickLogin() {
        loginButton.click();
    }
}
```

### @FindBy Annotations:

```java
// By ID
@FindBy(id = "username")
private WebElement usernameField;

// By Name
@FindBy(name = "password")
private WebElement passwordField;

// By ClassName
@FindBy(className = "error-message")
private WebElement errorMsg;

// By TagName
@FindBy(tagName = "h1")
private WebElement heading;

// By LinkText
@FindBy(linkText = "Sign Up")
private WebElement signUpLink;

// By PartialLinkText
@FindBy(partialLinkText = "Sign")
private WebElement signUpLink;

// By CSS Selector
@FindBy(css = "button.btn-primary")
private WebElement submitButton;

// By XPath
@FindBy(xpath = "//button[@type='submit']")
private WebElement submitButton;
```

### @FindBys (AND condition):

```java
// Element must match ALL conditions
@FindBys({
    @FindBy(tagName = "input"),
    @FindBy(name = "username")
})
private WebElement usernameField;

// Equivalent to: //input[@name='username']
```

### @FindAll (OR condition):

```java
// Element matches ANY condition
@FindAll({
    @FindBy(id = "submitBtn"),
    @FindBy(name = "submit"),
    @FindBy(xpath = "//button[@type='submit']")
})
private WebElement submitButton;
```

### List of Elements:

```java
@FindBy(className = "product-item")
private List<WebElement> products;

public int getProductCount() {
    return products.size();
}

public void clickFirstProduct() {
    products.get(0).click();
}
```

### @CacheLookup:

```java
// Cache element (good for static elements)
@FindBy(id = "logo")
@CacheLookup
private WebElement logo;

// Don't cache dynamic elements
@FindBy(id = "dynamic-content")
private WebElement dynamicContent;  // No @CacheLookup
```

**When to use @CacheLookup:**
- ✅ Static elements (logo, header, footer)
- ❌ Dynamic elements (search results, notifications)

### Page Factory with Explicit Waits:

```java
public class LoginPage {
    WebDriver driver;
    WebDriverWait wait;

    @FindBy(id = "username")
    private WebElement usernameField;

    @FindBy(id = "password")
    private WebElement passwordField;

    @FindBy(css = "button[type='submit']")
    private WebElement loginButton;

    public LoginPage(WebDriver driver) {
        this.driver = driver;
        this.wait = new WebDriverWait(driver, Duration.ofSeconds(10));
        PageFactory.initElements(driver, this);
    }

    public void enterUsername(String username) {
        wait.until(ExpectedConditions.visibilityOf(usernameField))
            .sendKeys(username);
    }

    public void enterPassword(String password) {
        wait.until(ExpectedConditions.visibilityOf(passwordField))
            .sendKeys(password);
    }

    public void clickLogin() {
        wait.until(ExpectedConditions.elementToBeClickable(loginButton))
            .click();
    }
}
```

### AjaxElementLocatorFactory (Page Factory with Timeout):

```java
public class LoginPage {
    WebDriver driver;

    @FindBy(id = "username")
    private WebElement usernameField;

    public LoginPage(WebDriver driver) {
        this.driver = driver;

        // Set implicit timeout for PageFactory elements
        AjaxElementLocatorFactory factory = new AjaxElementLocatorFactory(driver, 10);
        PageFactory.initElements(factory, this);
    }

    public void enterUsername(String username) {
        usernameField.sendKeys(username);  // Waits up to 10 seconds
    }
}
```

### Page Factory vs Standard POM:

| Feature | Standard POM | Page Factory |
|---------|--------------|--------------|
| Element Definition | By locators | @FindBy annotations |
| Element Initialization | Every findElement call | Once at page creation |
| Readability | More verbose | More concise |
| Dynamic Elements | Better support | Limited |
| Lazy Loading | Manual | Automatic |
| Industry Usage | Very common | Less common now |

**Modern Recommendation:** Standard POM is preferred over Page Factory because:
- Better handling of dynamic elements
- More flexibility with waits
- Easier to debug
- Better IDE support

---

## 6. Advanced POM Concepts

### 1. Base Page Class

```java
package pages;

import org.openqa.selenium.*;
import org.openqa.selenium.support.ui.*;
import java.time.Duration;

public class BasePage {
    protected WebDriver driver;
    protected WebDriverWait wait;

    public BasePage(WebDriver driver) {
        this.driver = driver;
        this.wait = new WebDriverWait(driver, Duration.ofSeconds(10));
    }

    // Common wait methods
    protected void waitForElementToBeVisible(By locator) {
        wait.until(ExpectedConditions.visibilityOfElementLocated(locator));
    }

    protected void waitForElementToBeClickable(By locator) {
        wait.until(ExpectedConditions.elementToBeClickable(locator));
    }

    // Common action methods
    protected void click(By locator) {
        waitForElementToBeClickable(locator);
        driver.findElement(locator).click();
    }

    protected void type(By locator, String text) {
        waitForElementToBeVisible(locator);
        WebElement element = driver.findElement(locator);
        element.clear();
        element.sendKeys(text);
    }

    protected String getText(By locator) {
        waitForElementToBeVisible(locator);
        return driver.findElement(locator).getText();
    }

    protected boolean isElementDisplayed(By locator) {
        try {
            return driver.findElement(locator).isDisplayed();
        } catch(NoSuchElementException e) {
            return false;
        }
    }

    // JavaScript actions
    protected void clickUsingJS(By locator) {
        WebElement element = driver.findElement(locator);
        JavascriptExecutor js = (JavascriptExecutor) driver;
        js.executeScript("arguments[0].click();", element);
    }

    protected void scrollToElement(By locator) {
        WebElement element = driver.findElement(locator);
        JavascriptExecutor js = (JavascriptExecutor) driver;
        js.executeScript("arguments[0].scrollIntoView(true);", element);
    }

    // Navigation
    protected void navigateTo(String url) {
        driver.get(url);
    }

    protected String getCurrentUrl() {
        return driver.getCurrentUrl();
    }

    protected String getPageTitle() {
        return driver.getTitle();
    }
}
```

### 2. Page Component Pattern

For reusable components like headers, footers, sidebars:

```java
// HeaderComponent.java
public class HeaderComponent extends BasePage {
    private By logo = By.id("logo");
    private By searchBox = By.id("search");
    private By cartIcon = By.id("cart");
    private By userMenu = By.id("user-menu");

    public HeaderComponent(WebDriver driver) {
        super(driver);
    }

    public void search(String query) {
        type(searchBox, query);
        driver.findElement(searchBox).submit();
    }

    public void clickCart() {
        click(cartIcon);
    }

    public void clickUserMenu() {
        click(userMenu);
    }

    public boolean isLogoDisplayed() {
        return isElementDisplayed(logo);
    }
}

// HomePage.java
public class HomePage extends BasePage {
    private HeaderComponent header;

    public HomePage(WebDriver driver) {
        super(driver);
        this.header = new HeaderComponent(driver);
    }

    public HeaderComponent getHeader() {
        return header;
    }

    // Home page specific methods
    public void clickFeaturedProduct() {
        // ...
    }
}

// Test usage
@Test
public void testSearch() {
    HomePage homePage = new HomePage(driver);
    homePage.getHeader().search("Selenium");
    // Assertions
}
```

### 3. Page Object Chaining (Fluent Interface)

```java
public class LoginPage extends BasePage {
    private By usernameField = By.id("username");
    private By passwordField = By.id("password");
    private By loginButton = By.id("loginBtn");

    public LoginPage(WebDriver driver) {
        super(driver);
    }

    public LoginPage enterUsername(String username) {
        type(usernameField, username);
        return this;  // Return this for chaining
    }

    public LoginPage enterPassword(String password) {
        type(passwordField, password);
        return this;  // Return this for chaining
    }

    public HomePage clickLogin() {
        click(loginButton);
        return new HomePage(driver);  // Return next page
    }
}

// Usage with method chaining
@Test
public void testLogin() {
    HomePage homePage = new LoginPage(driver)
        .enterUsername("testuser")
        .enterPassword("password123")
        .clickLogin();

    homePage.verifyUserLoggedIn();
}
```

### 4. LoadableComponent Pattern

```java
import org.openqa.selenium.support.ui.LoadableComponent;

public class LoginPage extends LoadableComponent<LoginPage> {
    private WebDriver driver;
    private By usernameField = By.id("username");

    public LoginPage(WebDriver driver) {
        this.driver = driver;
    }

    @Override
    protected void load() {
        driver.get("https://example.com/login");
    }

    @Override
    protected void isLoaded() throws Error {
        String url = driver.getCurrentUrl();
        if(!url.contains("/login")) {
            throw new Error("Login page not loaded. Current URL: " + url);
        }

        if(!driver.findElement(usernameField).isDisplayed()) {
            throw new Error("Username field not visible");
        }
    }

    public void login(String username, String password) {
        // Login logic
    }
}

// Usage
@Test
public void testLogin() {
    LoginPage loginPage = new LoginPage(driver).get();  // Ensures page is loaded
    loginPage.login("user", "pass");
}
```

### 5. Generic Page Class for Similar Pages

```java
public class FormPage<T extends FormPage<T>> extends BasePage {
    public FormPage(WebDriver driver) {
        super(driver);
    }

    @SuppressWarnings("unchecked")
    public T fillField(By locator, String value) {
        type(locator, value);
        return (T) this;
    }

    @SuppressWarnings("unchecked")
    public T clickButton(By locator) {
        click(locator);
        return (T) this;
    }
}

// Specific form pages
public class RegistrationPage extends FormPage<RegistrationPage> {
    private By firstNameField = By.id("firstName");
    private By submitButton = By.id("submit");

    public RegistrationPage(WebDriver driver) {
        super(driver);
    }

    public RegistrationPage enterFirstName(String name) {
        return fillField(firstNameField, name);
    }

    public HomePage submitForm() {
        clickButton(submitButton);
        return new HomePage(driver);
    }
}
```

---

## 7. POM Best Practices

### 1. One Page Object Per Web Page

```java
// GOOD
public class LoginPage { }
public class HomePage { }
public class ProfilePage { }

// BAD
public class AllPages {
    public void loginPage() { }
    public void homePage() { }
}
```

### 2. Use Meaningful Names

```java
// GOOD
public void enterUsername(String username) { }
public boolean isLoginButtonEnabled() { }
public String getErrorMessage() { }

// BAD
public void input1(String text) { }
public boolean check() { }
public String get() { }
```

### 3. Return Page Objects for Navigation

```java
public class LoginPage {
    public HomePage loginAsUser(String username, String password) {
        // Login logic
        return new HomePage(driver);
    }

    public ForgotPasswordPage clickForgotPassword() {
        // Click forgot password
        return new ForgotPasswordPage(driver);
    }
}
```

### 4. Keep Assertions Out of Page Objects

```java
// BAD - Assertion in page object
public class LoginPage {
    public void verifyLogin() {
        assert driver.findElement(By.id("welcome")).isDisplayed();
    }
}

// GOOD - Assertion in test
public class LoginTest {
    @Test
    public void testLogin() {
        loginPage.login("user", "pass");
        assert loginPage.isWelcomeMessageDisplayed();  // Assertion in test
    }
}
```

### 5. Use BasePage for Common Methods

```java
public class BasePage {
    protected void waitAndClick(By locator) { }
    protected void waitAndType(By locator, String text) { }
    protected String waitAndGetText(By locator) { }
}

// All pages extend BasePage
public class LoginPage extends BasePage { }
public class HomePage extends BasePage { }
```

### 6. Encapsulate Waits in Page Objects

```java
// GOOD - Wait logic in page object
public class LoginPage {
    public void clickLogin() {
        wait.until(ExpectedConditions.elementToBeClickable(loginButton)).click();
    }
}

// BAD - Wait logic in test
@Test
public void testLogin() {
    WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
    wait.until(ExpectedConditions.elementToBeClickable(By.id("login"))).click();
}
```

### 7. Use Enums for Test Data

```java
public enum UserType {
    VALID_USER("testuser", "password123"),
    INVALID_USER("wronguser", "wrongpass"),
    ADMIN_USER("admin", "adminpass");

    private String username;
    private String password;

    UserType(String username, String password) {
        this.username = username;
        this.password = password;
    }

    public String getUsername() { return username; }
    public String getPassword() { return password; }
}

// Usage
@Test
public void testValidLogin() {
    loginPage.login(
        UserType.VALID_USER.getUsername(),
        UserType.VALID_USER.getPassword()
    );
}
```

### 8. Keep Page Objects Independent

```java
// Each page object should be self-contained
public class LoginPage {
    // Should not depend on HomePage or other page objects
    // Only returns them for navigation
}
```

### 9. Use Constants for Repeated Values

```java
public class Constants {
    public static final String BASE_URL = "https://example.com";
    public static final int DEFAULT_TIMEOUT = 10;
    public static final String VALID_USERNAME = "testuser";
}

// Usage
public class LoginPage {
    public void navigateToLogin() {
        driver.get(Constants.BASE_URL + "/login");
    }
}
```

### 10. Handle Dynamic Elements Properly

```java
public class ProductPage {
    private By productItem(String productName) {
        return By.xpath("//div[contains(text(),'" + productName + "')]");
    }

    public void selectProduct(String productName) {
        click(productItem(productName));
    }
}
```

---

## 8. Common POM Patterns

### 1. Simple POM (Basic Structure)
```
Pages -> Tests
```

### 2. POM with Base Page
```
BasePage -> Pages -> Tests
```

### 3. POM with Page Components
```
BasePage -> Pages + Components -> Tests
```

### 4. POM with Page Factory
```
BasePage (PageFactory) -> Pages -> Tests
```

### 5. POM with Service Layer
```
BasePage -> Pages -> Services -> Tests
```

### 6. Hybrid POM
```
BasePage -> Pages + Components + Services -> Tests + Data -> Reports
```

---

## Summary

**Page Object Model** is a design pattern that:

1. **Separates** page structure from test logic
2. **Improves** code reusability and maintainability
3. **Enhances** test readability
4. **Reduces** code duplication
5. **Facilitates** collaboration

**Key Components:**
- Page Classes (one per web page)
- Locators (element identifiers)
- Action Methods (what you can do on page)
- Verification Methods (check page state)
- Navigation Methods (move between pages)

**Best Practices:**
- One page object per page
- Use BasePage for common functionality
- Return page objects for navigation
- Keep assertions in tests, not pages
- Encapsulate waits and actions
- Use meaningful method names

**Modern Approach:**
- Standard POM over Page Factory
- Explicit waits over implicit
- Components for reusable UI elements
- Service layer for business logic
- Data-driven with external sources

---

**Next Steps:**
- Practice creating page objects
- Build a complete POM framework
- Learn advanced patterns (components, services)
- Integrate with TestNG/JUnit
- Add reporting and logging
- Implement CI/CD integration

**Recommended Reading:**
- Martin Fowler's Page Object article
- Selenium documentation on POM
- Design patterns in test automation
- Clean Code principles

---

🚀 **Ready to implement POM in practical exercises!**
