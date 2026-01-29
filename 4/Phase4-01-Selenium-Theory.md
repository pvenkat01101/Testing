# Phase 4.1 - Selenium WebDriver: Comprehensive Theory (Beginner Level)

## Part 1: Introduction to Selenium

### What is Selenium?

Selenium is an open-source automation framework for testing web applications. It allows you to automate browser interactions and verify application behavior programmatically.

### History and Evolution

- **Selenium RC (Remote Control)**: First version, now deprecated
- **Selenium WebDriver**: Current standard, direct browser communication
- **Selenium Grid**: Parallel and distributed test execution
- **Selenium IDE**: Record and playback tool

### Why Selenium?

**Advantages**:
- Open source and free
- Supports multiple browsers (Chrome, Firefox, Safari, Edge)
- Supports multiple languages (Java, Python, C#, JavaScript)
- Large community and documentation
- Integration with testing frameworks (TestNG, JUnit)
- Cross-platform (Windows, Mac, Linux)

**Limitations**:
- Web applications only (no desktop apps)
- Requires programming knowledge
- No built-in reporting (needs frameworks)
- No built-in image comparison
- Can be slower than manual testing for simple cases

### Selenium Components

1. **Selenium WebDriver**: Core API for browser automation
2. **Selenium Grid**: Distributed test execution
3. **Selenium IDE**: Browser extension for record/playback

---

## Part 2: Selenium WebDriver Architecture

### How WebDriver Works

```
Test Script (Java/Python)
         ↓
Selenium WebDriver API
         ↓
Browser Driver (ChromeDriver, GeckoDriver)
         ↓
Browser (Chrome, Firefox)
```

**Flow**:
1. Test script calls WebDriver methods
2. WebDriver converts to browser-specific commands
3. Browser driver communicates with actual browser
4. Browser performs actions
5. Results return to test script

### Browser Drivers

Each browser needs its own driver:

| Browser | Driver | Download |
|---------|--------|----------|
| Chrome | ChromeDriver | chromedriver.chromium.org |
| Firefox | GeckoDriver | github.com/mozilla/geckodriver |
| Edge | EdgeDriver | developer.microsoft.com/en-us/microsoft-edge/tools/webdriver/ |
| Safari | SafariDriver | Built into Safari |

**WebDriverManager** (Recommended):
Automatically downloads and manages drivers.

```java
WebDriverManager.chromedriver().setup();
WebDriver driver = new ChromeDriver();
```

---

## Part 3: Setting Up Selenium

### Prerequisites

1. **Java JDK** (8 or higher)
2. **IDE** (Eclipse, IntelliJ IDEA, VS Code)
3. **Maven** or **Gradle** for dependency management
4. **Browser** installed

### Maven Setup

Add to `pom.xml`:

```xml
<dependencies>
    <!-- Selenium Java -->
    <dependency>
        <groupId>org.seleniumhq.selenium</groupId>
        <artifactId>selenium-java</artifactId>
        <version>4.16.0</version>
    </dependency>

    <!-- WebDriverManager -->
    <dependency>
        <groupId>io.github.bonigarcia</groupId>
        <artifactId>webdrivermanager</artifactId>
        <version>5.6.2</version>
    </dependency>
</dependencies>
```

### First Selenium Script

```java
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import io.github.bonigarcia.wdm.WebDriverManager;

public class FirstTest {
    public static void main(String[] args) {
        // Setup driver
        WebDriverManager.chromedriver().setup();

        // Create driver instance
        WebDriver driver = new ChromeDriver();

        // Navigate to URL
        driver.get("https://www.google.com");

        // Get title
        String title = driver.getTitle();
        System.out.println("Page title: " + title);

        // Close browser
        driver.quit();
    }
}
```

---

## Part 4: WebDriver Basics

### WebDriver Interface

`WebDriver` is the main interface with methods to control browser:

```java
WebDriver driver = new ChromeDriver();
```

### Common WebDriver Methods

#### Navigation Methods

```java
// Open URL
driver.get("https://example.com");

// Navigate forward
driver.navigate().forward();

// Navigate back
driver.navigate().back();

// Refresh page
driver.navigate().refresh();

// Navigate to URL (same as get)
driver.navigate().to("https://example.com");
```

#### Browser Methods

```java
// Get current URL
String url = driver.getCurrentUrl();

// Get page title
String title = driver.getTitle();

// Get page source
String source = driver.getPageSource();

// Close current window
driver.close();

// Close all windows and quit driver
driver.quit();
```

**Important**: Always use `driver.quit()` at the end to close browser and end session.

#### Window Management

```java
// Maximize window
driver.manage().window().maximize();

// Minimize window (Selenium 4+)
driver.manage().window().minimize();

// Full screen
driver.manage().window().fullscreen();

// Set custom size
Dimension size = new Dimension(1024, 768);
driver.manage().window().setSize(size);

// Set position
Point position = new Point(0, 0);
driver.manage().window().setPosition(position);
```

---

## Part 5: Locating Elements

### What are Locators?

Locators identify web elements on a page. Selenium provides 8 locator strategies.

### 8 Locator Types

#### 1. ID (Most Preferred)

Most unique and fastest.

```java
driver.findElement(By.id("username"));
```

**HTML**:
```html
<input id="username" type="text">
```

#### 2. Name

```java
driver.findElement(By.name("email"));
```

**HTML**:
```html
<input name="email" type="text">
```

#### 3. Class Name

```java
driver.findElement(By.className("login-button"));
```

**HTML**:
```html
<button class="login-button">Login</button>
```

**Note**: Use only one class name, not multiple.

#### 4. Tag Name

```java
driver.findElement(By.tagName("h1"));
```

**HTML**:
```html
<h1>Welcome</h1>
```

#### 5. Link Text

For links (anchor tags) with exact text.

```java
driver.findElement(By.linkText("Click Here"));
```

**HTML**:
```html
<a href="/page">Click Here</a>
```

#### 6. Partial Link Text

For links with partial text match.

```java
driver.findElement(By.partialLinkText("Click"));
```

Matches: "Click Here", "Click Me", "Click to Continue"

#### 7. CSS Selector (Powerful)

```java
// By ID
driver.findElement(By.cssSelector("#username"));

// By class
driver.findElement(By.cssSelector(".login-button"));

// By attribute
driver.findElement(By.cssSelector("input[name='email']"));

// By tag and class
driver.findElement(By.cssSelector("button.login-button"));

// Child element
driver.findElement(By.cssSelector("div#login > input"));

// Descendant
driver.findElement(By.cssSelector("div#login input"));
```

#### 8. XPath (Most Flexible)

```java
// Absolute XPath (not recommended)
driver.findElement(By.xpath("/html/body/div/form/input"));

// Relative XPath (recommended)
driver.findElement(By.xpath("//input[@id='username']"));

// By text
driver.findElement(By.xpath("//button[text()='Login']"));

// Contains
driver.findElement(By.xpath("//input[contains(@id, 'user')]"));

// Starts with
driver.findElement(By.xpath("//input[starts-with(@id, 'user')]"));

// Multiple attributes
driver.findElement(By.xpath("//input[@type='text' and @name='email']"));

// Parent
driver.findElement(By.xpath("//input[@id='username']/parent::div"));

// Following sibling
driver.findElement(By.xpath("//label[@for='username']/following-sibling::input"));
```

### CSS vs XPath

| Feature | CSS Selector | XPath |
|---------|--------------|-------|
| Speed | Faster | Slower |
| Readability | More readable | Less readable |
| Parent navigation | No | Yes |
| Text matching | No | Yes |
| Complex logic | Limited | More powerful |

**Recommendation**: Use CSS for simple cases, XPath when you need parent/text navigation.

### findElement vs findElements

```java
// Returns single WebElement (throws exception if not found)
WebElement element = driver.findElement(By.id("username"));

// Returns List<WebElement> (returns empty list if not found)
List<WebElement> elements = driver.findElements(By.className("item"));
```

---

## Part 6: Interacting with Elements

### WebElement Interface

Once located, you interact with elements using WebElement methods.

```java
WebElement element = driver.findElement(By.id("username"));
```

### Common WebElement Methods

#### Text Input

```java
// Type text
element.sendKeys("test@example.com");

// Clear existing text
element.clear();

// Clear and type
element.clear();
element.sendKeys("new text");
```

#### Clicks

```java
// Click element
element.click();
```

#### Retrieving Information

```java
// Get visible text
String text = element.getText();

// Get attribute value
String value = element.getAttribute("value");
String href = element.getAttribute("href");

// Get CSS property
String color = element.getCssValue("color");

// Get tag name
String tag = element.getTagName();

// Check if displayed
boolean isDisplayed = element.isDisplayed();

// Check if enabled
boolean isEnabled = element.isEnabled();

// Check if selected (checkboxes/radio)
boolean isSelected = element.isSelected();
```

### Dropdowns

#### Select Class

For `<select>` elements:

```java
WebElement dropdown = driver.findElement(By.id("country"));
Select select = new Select(dropdown);

// Select by visible text
select.selectByVisibleText("United States");

// Select by value
select.selectByValue("us");

// Select by index (0-based)
select.selectByIndex(1);

// Get selected option
WebElement selected = select.getFirstSelectedOption();
String selectedText = selected.getText();

// Get all options
List<WebElement> options = select.getOptions();

// Check if multi-select
boolean isMultiple = select.isMultiple();

// Deselect (multi-select only)
select.deselectAll();
select.deselectByValue("us");
```

### Checkboxes and Radio Buttons

```java
WebElement checkbox = driver.findElement(By.id("terms"));

// Check if selected
if (!checkbox.isSelected()) {
    checkbox.click();  // Select it
}

// Uncheck
if (checkbox.isSelected()) {
    checkbox.click();  // Deselect it
}
```

### File Upload

```java
WebElement upload = driver.findElement(By.id("file-upload"));
upload.sendKeys("/path/to/file.txt");  // Provide absolute path
```

---

## Part 7: Waits in Selenium

### Why Waits are Needed

Web pages load dynamically. Elements may not be immediately available. Waits ensure synchronization between script and page.

### Types of Waits

#### 1. Implicit Wait

Waits for all elements globally for specified time.

```java
driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(10));
```

**How it works**:
- Applied once, affects all findElement calls
- Waits up to specified time for element to appear
- Polls every 500ms by default

**Pros**: Easy to implement
**Cons**: Can slow down tests if overused

#### 2. Explicit Wait

Waits for specific condition for specific element.

```java
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));

// Wait for element to be clickable
WebElement element = wait.until(ExpectedConditions.elementToBeClickable(By.id("submit")));

// Wait for element to be visible
wait.until(ExpectedConditions.visibilityOfElementLocated(By.id("message")));

// Wait for element to be present
wait.until(ExpectedConditions.presenceOfElementLocated(By.id("result")));

// Wait for title
wait.until(ExpectedConditions.titleContains("Success"));

// Wait for alert
wait.until(ExpectedConditions.alertIsPresent());
```

**Common ExpectedConditions**:
- `visibilityOfElementLocated()`
- `elementToBeClickable()`
- `presenceOfElementLocated()`
- `invisibilityOfElementLocated()`
- `textToBePresentInElement()`
- `titleContains()`
- `urlContains()`
- `alertIsPresent()`
- `frameToBeAvailableAndSwitchToIt()`

**Pros**: More precise control
**Cons**: More code

#### 3. Fluent Wait

More flexible explicit wait with custom polling interval.

```java
Wait<WebDriver> wait = new FluentWait<>(driver)
    .withTimeout(Duration.ofSeconds(30))
    .pollingEvery(Duration.ofSeconds(2))
    .ignoring(NoSuchElementException.class);

WebElement element = wait.until(new Function<WebDriver, WebElement>() {
    public WebElement apply(WebDriver driver) {
        return driver.findElement(By.id("dynamic-element"));
    }
});
```

**Lambda version**:
```java
WebElement element = wait.until(driver ->
    driver.findElement(By.id("dynamic-element"))
);
```

#### 4. Thread.sleep() (Not Recommended)

```java
Thread.sleep(5000);  // Hard wait, avoid in production code
```

**Why avoid**:
- Unconditional wait (wastes time)
- No synchronization with page state
- Makes tests slower

**When to use**: Only for debugging or rare cases where other waits don't work.

### Best Practices for Waits

1. **Prefer Explicit Waits**: More reliable and faster
2. **Avoid Implicit + Explicit together**: Can cause unpredictable behavior
3. **Don't use Thread.sleep()**: Use explicit waits instead
4. **Set reasonable timeouts**: 10-30 seconds typical
5. **Wait for specific conditions**: Be precise (clickable vs visible vs present)

---

## Part 8: Handling Special Elements

### Alerts

JavaScript alerts, confirms, and prompts.

```java
// Switch to alert
Alert alert = driver.switchTo().alert();

// Get alert text
String text = alert.getText();

// Accept alert (OK button)
alert.accept();

// Dismiss alert (Cancel button)
alert.dismiss();

// Type in prompt
alert.sendKeys("text");
alert.accept();
```

**With Explicit Wait**:
```java
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
wait.until(ExpectedConditions.alertIsPresent());
Alert alert = driver.switchTo().alert();
alert.accept();
```

### Frames and iFrames

Frames are separate HTML documents within a page.

```java
// Switch by index
driver.switchTo().frame(0);

// Switch by name or ID
driver.switchTo().frame("frameName");

// Switch by WebElement
WebElement frameElement = driver.findElement(By.id("myFrame"));
driver.switchTo().frame(frameElement);

// Switch back to main content
driver.switchTo().defaultContent();

// Switch to parent frame
driver.switchTo().parentFrame();
```

**Important**: Must switch to frame before interacting with elements inside it.

### Windows and Tabs

```java
// Get current window handle
String mainWindow = driver.getWindowHandle();

// Click link that opens new window/tab
driver.findElement(By.linkText("Open New Window")).click();

// Get all window handles
Set<String> allWindows = driver.getWindowHandles();

// Switch to new window
for (String window : allWindows) {
    if (!window.equals(mainWindow)) {
        driver.switchTo().window(window);
        break;
    }
}

// Perform actions in new window
System.out.println(driver.getTitle());

// Close current window
driver.close();

// Switch back to main window
driver.switchTo().window(mainWindow);
```

**Selenium 4 - New Tab/Window**:
```java
// Open new tab
driver.switchTo().newWindow(WindowType.TAB);

// Open new window
driver.switchTo().newWindow(WindowType.WINDOW);
```

---

## Part 9: Actions Class (Advanced Interactions)

### What is Actions Class?

For complex user interactions: hover, drag-drop, double-click, right-click.

```java
Actions actions = new Actions(driver);
```

### Common Actions

#### Hover (Mouse Over)

```java
WebElement element = driver.findElement(By.id("menu"));
actions.moveToElement(element).perform();
```

#### Drag and Drop

```java
WebElement source = driver.findElement(By.id("drag"));
WebElement target = driver.findElement(By.id("drop"));

// Method 1
actions.dragAndDrop(source, target).perform();

// Method 2
actions.clickAndHold(source)
       .moveToElement(target)
       .release()
       .perform();
```

#### Double Click

```java
WebElement element = driver.findElement(By.id("button"));
actions.doubleClick(element).perform();
```

#### Right Click (Context Click)

```java
WebElement element = driver.findElement(By.id("element"));
actions.contextClick(element).perform();
```

#### Click and Hold

```java
WebElement element = driver.findElement(By.id("element"));
actions.clickAndHold(element).perform();
```

#### Keyboard Actions

```java
// Press key
actions.sendKeys(Keys.ENTER).perform();

// Key combination
actions.keyDown(Keys.CONTROL).sendKeys("a").keyUp(Keys.CONTROL).perform();

// Type in element
WebElement input = driver.findElement(By.id("search"));
actions.sendKeys(input, "selenium").perform();

// Shift + click
actions.keyDown(Keys.SHIFT).click(element).keyUp(Keys.SHIFT).perform();
```

#### Chaining Actions

```java
actions.moveToElement(menu)
       .click()
       .moveToElement(submenu)
       .click()
       .perform();
```

**Important**: Always call `.perform()` at the end to execute actions.

---

## Part 10: JavaScript Executor

### What is JavaScriptExecutor?

Interface to execute JavaScript in browser context. Useful when Selenium methods don't work.

```java
JavascriptExecutor js = (JavascriptExecutor) driver;
```

### Common Use Cases

#### Scroll

```java
// Scroll to bottom
js.executeScript("window.scrollTo(0, document.body.scrollHeight);");

// Scroll to element
WebElement element = driver.findElement(By.id("footer"));
js.executeScript("arguments[0].scrollIntoView(true);", element);

// Scroll by pixels
js.executeScript("window.scrollBy(0, 500);");
```

#### Click

When normal click doesn't work:

```java
WebElement element = driver.findElement(By.id("button"));
js.executeScript("arguments[0].click();", element);
```

#### Send Keys

```java
WebElement input = driver.findElement(By.id("email"));
js.executeScript("arguments[0].value='test@example.com';", input);
```

#### Get Page Information

```java
// Get title
String title = (String) js.executeScript("return document.title;");

// Get URL
String url = (String) js.executeScript("return document.URL;");

// Get inner text
String text = (String) js.executeScript("return document.documentElement.innerText;");
```

#### Highlight Element (For Debugging)

```java
WebElement element = driver.findElement(By.id("username"));
js.executeScript("arguments[0].style.border='3px solid red'", element);
```

#### Refresh Page

```java
js.executeScript("location.reload();");
```

---

## Part 11: Taking Screenshots

### Why Screenshots?

- Debugging failed tests
- Evidence of defects
- Test reports documentation

### Full Page Screenshot

```java
// Cast driver to TakesScreenshot
TakesScreenshot ts = (TakesScreenshot) driver;

// Capture screenshot
File source = ts.getScreenshotAs(OutputType.FILE);

// Save to destination
File destination = new File("screenshot.png");
FileUtils.copyFile(source, destination);
```

**With timestamp**:
```java
String timestamp = new SimpleDateFormat("yyyyMMdd_HHmmss").format(new Date());
File destination = new File("screenshot_" + timestamp + ".png");
FileUtils.copyFile(source, destination);
```

### Element Screenshot (Selenium 4+)

```java
WebElement element = driver.findElement(By.id("logo"));
File source = element.getScreenshotAs(OutputType.FILE);
FileUtils.copyFile(source, new File("element.png"));
```

---

## Part 12: Selenium 4 New Features

### Relative Locators

Find elements relative to other elements.

```java
import static org.openqa.selenium.support.locators.RelativeLocator.with;

// Element above
driver.findElement(with(By.tagName("input")).above(passwordField));

// Element below
driver.findElement(with(By.tagName("label")).below(usernameField));

// Element to left
driver.findElement(with(By.tagName("label")).toLeftOf(inputField));

// Element to right
driver.findElement(with(By.tagName("button")).toRightOf(inputField));

// Element near (within 50 pixels)
driver.findElement(with(By.tagName("span")).near(errorMessage));
```

### Chrome DevTools Protocol (CDP)

```java
ChromeDriver driver = new ChromeDriver();

// Emulate network conditions
driver.executeCdpCommand("Network.emulateNetworkConditions",
    Map.of("offline", false, "latency", 100, "downloadThroughput", 1024, "uploadThroughput", 1024));

// Capture console logs
driver.getDevTools().createSession();
driver.getDevTools().send(Log.enable());
driver.getDevTools().addListener(Log.entryAdded(), logEntry -> {
    System.out.println(logEntry.getText());
});
```

### New Window/Tab

```java
// Open new tab
driver.switchTo().newWindow(WindowType.TAB);

// Open new window
driver.switchTo().newWindow(WindowType.WINDOW);
```

---

## Part 13: Best Practices

### 1. Use Page Object Model (POM)

Separate page structure from tests.

```java
// LoginPage.java
public class LoginPage {
    WebDriver driver;

    By usernameField = By.id("username");
    By passwordField = By.id("password");
    By loginButton = By.id("login");

    public LoginPage(WebDriver driver) {
        this.driver = driver;
    }

    public void login(String username, String password) {
        driver.findElement(usernameField).sendKeys(username);
        driver.findElement(passwordField).sendKeys(password);
        driver.findElement(loginButton).click();
    }
}
```

### 2. Use Explicit Waits

More reliable than implicit waits.

### 3. Always Use quit()

```java
try {
    // Test code
} finally {
    driver.quit();
}
```

### 4. Externalize Test Data

Don't hardcode data in scripts. Use properties files, Excel, or databases.

### 5. Use WebDriverManager

Avoid manual driver management.

### 6. Meaningful Locators

Prefer ID, name, or custom data attributes over complex XPath.

### 7. Handle Exceptions

```java
try {
    driver.findElement(By.id("element")).click();
} catch (NoSuchElementException e) {
    System.out.println("Element not found");
}
```

### 8. Avoid Thread.sleep()

Use explicit waits instead.

### 9. Use Assertions

Verify expected vs actual results.

```java
String actualTitle = driver.getTitle();
Assert.assertEquals(actualTitle, "Expected Title");
```

### 10. Maintain Test Independence

Each test should run independently, not depend on previous tests.

---

## Part 14: Common Exceptions

### NoSuchElementException

Element not found on page.

**Causes**:
- Wrong locator
- Element not loaded yet
- Element in frame/new window

**Solution**: Verify locator, add wait, switch to frame/window

### StaleElementReferenceException

Element reference is no longer valid (DOM changed).

**Solution**: Re-find element after page refresh/navigation

### TimeoutException

Element didn't appear within wait time.

**Solution**: Increase timeout or check if element actually appears

### ElementNotInteractableException

Element exists but not interactable (hidden, disabled).

**Solution**: Wait for element to be clickable, scroll into view

### ElementClickInterceptedException

Another element is blocking the click.

**Solution**: Scroll to element, wait for overlay to disappear, use JavaScript click

---

## Part 15: Selenium Grid (Brief Overview)

### What is Selenium Grid?

Tool for running tests on multiple machines, browsers, and OS combinations in parallel.

### Components

- **Hub**: Central point, receives test requests
- **Nodes**: Machines that execute tests

### Benefits

- Parallel execution (faster results)
- Cross-browser testing
- Cross-platform testing
- Better resource utilization

### Basic Setup

```bash
# Start hub
java -jar selenium-server.jar hub

# Start node
java -jar selenium-server.jar node --hub http://hub-ip:4444
```

---

## Conclusion

Selenium WebDriver is a powerful tool for web automation testing. Key takeaways:

1. **Setup**: Use Maven + WebDriverManager
2. **Locators**: ID > Name > CSS > XPath
3. **Waits**: Prefer explicit waits
4. **Interactions**: WebElement methods, Actions class
5. **Special elements**: Alerts, frames, windows
6. **Advanced**: JavaScriptExecutor, screenshots
7. **Best practices**: POM, waits, quit(), error handling

Master these concepts through practice. The practical exercises will reinforce this theory!
