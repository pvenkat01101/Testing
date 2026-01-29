# Phase 5.1: Advanced Selenium WebDriver - Theory

## Table of Contents
1. [Advanced Synchronization Strategies](#advanced-synchronization-strategies)
2. [Complex User Interactions](#complex-user-interactions)
3. [Handling Alerts, Frames, and Windows](#handling-alerts-frames-and-windows)
4. [File Upload and Download](#file-upload-and-download)
5. [Taking Screenshots](#taking-screenshots)
6. [Browser Options and Capabilities](#browser-options-and-capabilities)
7. [Headless Browser Testing](#headless-browser-testing)
8. [Advanced Troubleshooting](#advanced-troubleshooting)

---

## 1. Advanced Synchronization Strategies

### 1.1 Custom Expected Conditions

Beyond the built-in ExpectedConditions, you can create custom wait conditions:

```java
// Custom wait condition example
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));

// Wait for element to have specific text
wait.until(new ExpectedCondition<Boolean>() {
    public Boolean apply(WebDriver driver) {
        WebElement element = driver.findElement(By.id("status"));
        return element.getText().equals("Complete");
    }
});

// Using lambda (Java 8+)
wait.until(driver -> {
    WebElement element = driver.findElement(By.id("status"));
    return element.getText().equals("Complete");
});
```

### 1.2 Fluent Wait in Detail

Fluent Wait provides more control over polling and exception handling:

```java
Wait<WebDriver> wait = new FluentWait<>(driver)
    .withTimeout(Duration.ofSeconds(30))
    .pollingEvery(Duration.ofMillis(500))
    .ignoring(NoSuchElementException.class)
    .ignoring(StaleElementReferenceException.class)
    .withMessage("Element not found after 30 seconds");

WebElement element = wait.until(driver ->
    driver.findElement(By.id("dynamicElement"))
);
```

**Key Features:**
- Custom polling interval
- Ignore specific exceptions
- Custom timeout message
- More flexible than WebDriverWait

### 1.3 Handling StaleElementReferenceException

```java
public WebElement retryingFindElement(By by, int maxRetries) {
    WebElement element = null;
    int attempts = 0;

    while(attempts < maxRetries) {
        try {
            element = driver.findElement(by);
            return element;
        } catch(StaleElementReferenceException e) {
            attempts++;
            if(attempts >= maxRetries) {
                throw e;
            }
        }
    }
    return element;
}
```

### 1.4 Page Load Strategy

```java
ChromeOptions options = new ChromeOptions();
options.setPageLoadStrategy(PageLoadStrategy.NORMAL);  // Wait for full page load
// options.setPageLoadStrategy(PageLoadStrategy.EAGER);   // Wait for DOMContentLoaded
// options.setPageLoadStrategy(PageLoadStrategy.NONE);    // Don't wait

WebDriver driver = new ChromeDriver(options);
```

**Strategies:**
- **NORMAL**: Waits for complete page load (default)
- **EAGER**: Waits for DOMContentLoaded event
- **NONE**: Returns immediately

---

## 2. Complex User Interactions

### 2.1 Actions Class - Advanced Usage

The Actions class handles complex user gestures:

```java
Actions actions = new Actions(driver);
```

#### Mouse Hover (Move to Element)

```java
WebElement menu = driver.findElement(By.id("mainMenu"));
actions.moveToElement(menu).perform();

// Hover and click sub-menu
WebElement subMenu = driver.findElement(By.id("subMenu"));
actions.moveToElement(menu)
       .moveToElement(subMenu)
       .click()
       .perform();
```

#### Drag and Drop

```java
WebElement source = driver.findElement(By.id("draggable"));
WebElement target = driver.findElement(By.id("droppable"));

// Method 1: Using dragAndDrop
actions.dragAndDrop(source, target).perform();

// Method 2: Using clickAndHold and release
actions.clickAndHold(source)
       .moveToElement(target)
       .release()
       .perform();

// Method 3: Using offset
actions.clickAndHold(source)
       .moveByOffset(100, 50)
       .release()
       .perform();
```

#### Right Click (Context Click)

```java
WebElement element = driver.findElement(By.id("rightClickBtn"));
actions.contextClick(element).perform();
```

#### Double Click

```java
WebElement element = driver.findElement(By.id("doubleClickBtn"));
actions.doubleClick(element).perform();
```

#### Click and Hold

```java
WebElement slider = driver.findElement(By.id("slider"));
actions.clickAndHold(slider)
       .moveByOffset(50, 0)
       .release()
       .perform();
```

### 2.2 Keyboard Actions

```java
WebElement searchBox = driver.findElement(By.id("search"));

// Type in uppercase
actions.keyDown(Keys.SHIFT)
       .sendKeys("selenium")
       .keyUp(Keys.SHIFT)
       .perform();

// Copy-Paste
actions.sendKeys(Keys.CONTROL + "a")     // Select all
       .sendKeys(Keys.CONTROL + "c")     // Copy
       .sendKeys(Keys.TAB)               // Move to next field
       .sendKeys(Keys.CONTROL + "v")     // Paste
       .perform();

// Multiple keys
actions.keyDown(Keys.CONTROL)
       .keyDown(Keys.SHIFT)
       .sendKeys("a")
       .keyUp(Keys.SHIFT)
       .keyUp(Keys.CONTROL)
       .perform();
```

### 2.3 Composite Actions

Chain multiple actions together:

```java
actions.moveToElement(menu)
       .pause(Duration.ofSeconds(1))
       .click(subMenu)
       .pause(Duration.ofSeconds(1))
       .sendKeys(Keys.ARROW_DOWN)
       .sendKeys(Keys.ENTER)
       .perform();
```

### 2.4 Scrolling

```java
// Scroll to element
WebElement element = driver.findElement(By.id("footer"));
actions.scrollToElement(element).perform();

// Scroll by amount
actions.scrollByAmount(0, 500).perform();  // Scroll down 500 pixels

// Scroll from element
WebElement scrollableDiv = driver.findElement(By.id("scrollableDiv"));
actions.scrollFromOrigin(
    WheelInput.ScrollOrigin.fromElement(scrollableDiv),
    0,
    300
).perform();
```

---

## 3. Handling Alerts, Frames, and Windows

### 3.1 JavaScript Alerts

```java
// Click button that triggers alert
driver.findElement(By.id("alertBtn")).click();

// Switch to alert
Alert alert = driver.switchTo().alert();

// Get alert text
String alertText = alert.getText();
System.out.println("Alert text: " + alertText);

// Accept alert (OK button)
alert.accept();
```

### 3.2 Confirmation Dialog

```java
driver.findElement(By.id("confirmBtn")).click();

Alert confirmAlert = driver.switchTo().alert();
System.out.println(confirmAlert.getText());

// Accept (OK)
confirmAlert.accept();

// OR Dismiss (Cancel)
// confirmAlert.dismiss();
```

### 3.3 Prompt Dialog

```java
driver.findElement(By.id("promptBtn")).click();

Alert promptAlert = driver.switchTo().alert();
System.out.println(promptAlert.getText());

// Send text to prompt
promptAlert.sendKeys("Selenium WebDriver");

// Accept
promptAlert.accept();
```

### 3.4 Handling Frames/iFrames

Frames are separate HTML documents embedded in a page. You must switch to a frame before interacting with its elements.

```java
// Method 1: Switch by index
driver.switchTo().frame(0);

// Method 2: Switch by name or ID
driver.switchTo().frame("frameName");
driver.switchTo().frame("frameId");

// Method 3: Switch by WebElement
WebElement frameElement = driver.findElement(By.id("myFrame"));
driver.switchTo().frame(frameElement);

// Interact with elements inside frame
driver.findElement(By.id("insideFrameElement")).click();

// Switch back to main content
driver.switchTo().defaultContent();

// Switch to parent frame (if nested frames)
driver.switchTo().parentFrame();
```

#### Nested Frames

```java
// Switch to outer frame
driver.switchTo().frame("outerFrame");

// Switch to inner frame
driver.switchTo().frame("innerFrame");

// Interact with element in inner frame
driver.findElement(By.id("innerElement")).click();

// Go back one level
driver.switchTo().parentFrame();  // Now in outerFrame

// Go back to main content
driver.switchTo().defaultContent();
```

### 3.5 Handling Multiple Windows/Tabs

```java
// Get current window handle
String mainWindow = driver.getWindowHandle();
System.out.println("Main window: " + mainWindow);

// Click link that opens new window
driver.findElement(By.id("newWindowBtn")).click();

// Get all window handles
Set<String> allWindows = driver.getWindowHandles();
System.out.println("Total windows: " + allWindows.size());

// Switch to new window
for(String window : allWindows) {
    if(!window.equals(mainWindow)) {
        driver.switchTo().window(window);
        break;
    }
}

// Perform actions in new window
System.out.println("New window title: " + driver.getTitle());

// Close new window
driver.close();

// Switch back to main window
driver.switchTo().window(mainWindow);
```

#### Opening New Tab

```java
// Open new tab
driver.switchTo().newWindow(WindowType.TAB);

// Open new window
driver.switchTo().newWindow(WindowType.WINDOW);
```

#### Better Window Handling

```java
public void switchToWindowByTitle(String title) {
    Set<String> windows = driver.getWindowHandles();
    for(String window : windows) {
        driver.switchTo().window(window);
        if(driver.getTitle().equals(title)) {
            break;
        }
    }
}
```

---

## 4. File Upload and Download

### 4.1 File Upload

#### Simple File Upload (input type="file")

```java
WebElement uploadElement = driver.findElement(By.id("fileUpload"));
String filePath = "/Users/username/Documents/test.pdf";
uploadElement.sendKeys(filePath);

// Click upload button
driver.findElement(By.id("uploadBtn")).click();
```

**Important Notes:**
- File path must be absolute
- No need to click "Browse" button
- sendKeys() directly sets the file path

#### Multiple File Upload

```java
WebElement uploadElement = driver.findElement(By.id("multipleFileUpload"));
String file1 = "/path/to/file1.txt";
String file2 = "/path/to/file2.txt";

// Upload multiple files (separate with newline)
uploadElement.sendKeys(file1 + "\n" + file2);
```

#### File Upload using Robot Class (Windows Dialog)

For non-standard file upload dialogs:

```java
import java.awt.Robot;
import java.awt.Toolkit;
import java.awt.datatransfer.StringSelection;
import java.awt.event.KeyEvent;

// Click upload button that opens Windows dialog
driver.findElement(By.id("uploadBtn")).click();

// Copy file path to clipboard
StringSelection filePath = new StringSelection("/path/to/file.txt");
Toolkit.getDefaultToolkit().getSystemClipboard().setContents(filePath, null);

// Use Robot to paste and press Enter
Robot robot = new Robot();
Thread.sleep(1000);  // Wait for dialog to open

robot.keyPress(KeyEvent.VK_CONTROL);
robot.keyPress(KeyEvent.VK_V);
robot.keyRelease(KeyEvent.VK_V);
robot.keyRelease(KeyEvent.VK_CONTROL);

robot.keyPress(KeyEvent.VK_ENTER);
robot.keyRelease(KeyEvent.VK_ENTER);
```

### 4.2 File Download

#### Setting Download Directory (Chrome)

```java
String downloadPath = "/Users/username/Downloads/selenium";

ChromeOptions options = new ChromeOptions();
Map<String, Object> prefs = new HashMap<>();
prefs.put("download.default_directory", downloadPath);
prefs.put("download.prompt_for_download", false);
prefs.put("plugins.always_open_pdf_externally", true);  // For PDFs

options.setExperimentalOption("prefs", prefs);

WebDriver driver = new ChromeDriver(options);
```

#### Setting Download Directory (Firefox)

```java
String downloadPath = "/Users/username/Downloads/selenium";

FirefoxOptions options = new FirefoxOptions();
options.addPreference("browser.download.folderList", 2);
options.addPreference("browser.download.dir", downloadPath);
options.addPreference("browser.download.useDownloadDir", true);
options.addPreference("browser.helperApps.neverAsk.saveToDisk",
    "application/pdf,application/zip,text/csv");

WebDriver driver = new FirefoxDriver(options);
```

#### Verifying Download

```java
public boolean isFileDownloaded(String downloadPath, String fileName, int timeoutSeconds) {
    File file = new File(downloadPath + "/" + fileName);
    int waitTime = 0;

    while(waitTime < timeoutSeconds) {
        if(file.exists() && file.length() > 0) {
            return true;
        }
        try {
            Thread.sleep(1000);
        } catch(InterruptedException e) {
            e.printStackTrace();
        }
        waitTime++;
    }
    return false;
}

// Usage
driver.findElement(By.id("downloadBtn")).click();
boolean downloaded = isFileDownloaded("/path/to/downloads", "report.pdf", 30);
```

---

## 5. Taking Screenshots

### 5.1 Full Page Screenshot

```java
// Cast driver to TakesScreenshot
TakesScreenshot ts = (TakesScreenshot) driver;

// Capture screenshot as file
File source = ts.getScreenshotAs(OutputType.FILE);

// Save to desired location
File destination = new File("./screenshots/homepage.png");
FileUtils.copyFile(source, destination);

// OR using Files.copy
Files.copy(source.toPath(), destination.toPath());
```

### 5.2 Element Screenshot (Selenium 4)

```java
WebElement element = driver.findElement(By.id("logo"));

// Take screenshot of specific element
File source = element.getScreenshotAs(OutputType.FILE);
File destination = new File("./screenshots/logo.png");
FileUtils.copyFile(source, destination);
```

### 5.3 Screenshot as Base64

```java
TakesScreenshot ts = (TakesScreenshot) driver;
String base64Screenshot = ts.getScreenshotAs(OutputType.BASE64);

// Can be embedded in HTML reports
String imageTag = "<img src='data:image/png;base64," + base64Screenshot + "' />";
```

### 5.4 Screenshot on Failure (Utility Method)

```java
public void captureScreenshot(String testName) {
    try {
        TakesScreenshot ts = (TakesScreenshot) driver;
        File source = ts.getScreenshotAs(OutputType.FILE);

        String timestamp = new SimpleDateFormat("yyyyMMdd_HHmmss").format(new Date());
        String fileName = testName + "_" + timestamp + ".png";

        File destination = new File("./screenshots/" + fileName);
        FileUtils.copyFile(source, destination);

        System.out.println("Screenshot saved: " + fileName);
    } catch(Exception e) {
        System.out.println("Failed to capture screenshot: " + e.getMessage());
    }
}
```

### 5.5 Full Page Screenshot (Chrome DevTools)

```java
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.devtools.DevTools;
import org.openqa.selenium.devtools.v120.page.Page;

ChromeDriver driver = new ChromeDriver();
DevTools devTools = driver.getDevTools();
devTools.createSession();

// Capture full page screenshot
var screenshot = devTools.send(Page.captureScreenshot(
    Optional.empty(),
    Optional.empty(),
    Optional.empty(),
    Optional.empty(),
    Optional.of(true)  // Capture beyond viewport
));

// Save screenshot
Files.write(
    Paths.get("fullpage.png"),
    Base64.getDecoder().decode(screenshot)
);
```

---

## 6. Browser Options and Capabilities

### 6.1 Chrome Options

```java
ChromeOptions options = new ChromeOptions();

// Start maximized
options.addArguments("--start-maximized");

// Disable notifications
options.addArguments("--disable-notifications");

// Incognito mode
options.addArguments("--incognito");

// Disable extensions
options.addArguments("--disable-extensions");

// Disable popup blocking
options.addArguments("--disable-popup-blocking");

// Set window size
options.addArguments("--window-size=1920,1080");

// Accept insecure certificates
options.setAcceptInsecureCerts(true);

// Set user data directory (maintain session)
options.addArguments("--user-data-dir=/path/to/profile");

// Disable images for faster loading
Map<String, Object> prefs = new HashMap<>();
prefs.put("profile.default_content_setting_values.images", 2);
options.setExperimentalOption("prefs", prefs);

// Set download directory
prefs.put("download.default_directory", "/path/to/downloads");
options.setExperimentalOption("prefs", prefs);

WebDriver driver = new ChromeDriver(options);
```

### 6.2 Firefox Options

```java
FirefoxOptions options = new FirefoxOptions();

// Start maximized
options.addArguments("--start-maximized");

// Private browsing
options.addArguments("-private");

// Set preferences
options.addPreference("dom.webnotifications.enabled", false);
options.addPreference("geo.enabled", false);

// Set download directory
options.addPreference("browser.download.dir", "/path/to/downloads");
options.addPreference("browser.download.folderList", 2);

// Set Firefox binary location
options.setBinary("/path/to/firefox");

// Set profile
FirefoxProfile profile = new FirefoxProfile();
profile.setPreference("browser.download.dir", "/path/to/downloads");
options.setProfile(profile);

WebDriver driver = new FirefoxDriver(options);
```

### 6.3 Edge Options

```java
EdgeOptions options = new EdgeOptions();

options.addArguments("--start-maximized");
options.addArguments("--inprivate");
options.addArguments("--disable-notifications");

WebDriver driver = new EdgeDriver(options);
```

### 6.4 Safari Options

```java
SafariOptions options = new SafariOptions();
options.setAutomaticInspection(true);
options.setAutomaticProfiling(true);

WebDriver driver = new SafariDriver(options);
```

### 6.5 Desired Capabilities (Legacy - Selenium 3)

```java
DesiredCapabilities capabilities = new DesiredCapabilities();
capabilities.setBrowserName("chrome");
capabilities.setPlatform(Platform.WINDOWS);
capabilities.setVersion("120.0");

// Merge with options
ChromeOptions options = new ChromeOptions();
options.merge(capabilities);

WebDriver driver = new ChromeDriver(options);
```

---

## 7. Headless Browser Testing

Headless mode runs browser without GUI - faster execution, good for CI/CD.

### 7.1 Chrome Headless

```java
ChromeOptions options = new ChromeOptions();
options.addArguments("--headless=new");  // New headless mode (Selenium 4)
// options.addArguments("--headless");   // Old headless mode

// Additional headless-friendly arguments
options.addArguments("--disable-gpu");
options.addArguments("--window-size=1920,1080");
options.addArguments("--no-sandbox");
options.addArguments("--disable-dev-shm-usage");

WebDriver driver = new ChromeDriver(options);
```

### 7.2 Firefox Headless

```java
FirefoxOptions options = new FirefoxOptions();
options.addArguments("--headless");
options.addArguments("--width=1920");
options.addArguments("--height=1080");

WebDriver driver = new FirefoxDriver(options);
```

### 7.3 Edge Headless

```java
EdgeOptions options = new EdgeOptions();
options.addArguments("--headless=new");
options.addArguments("--disable-gpu");

WebDriver driver = new EdgeDriver(options);
```

### 7.4 HTMLUnit Driver (Headless by Default)

```java
WebDriver driver = new HtmlUnitDriver();

// With JavaScript support
WebDriver driver = new HtmlUnitDriver(true);
```

**Pros:**
- Very fast (no actual browser)
- Low memory usage
- Good for simple tests

**Cons:**
- Doesn't support modern JavaScript frameworks well
- Can't test visual elements
- No screenshot capability

### 7.5 PhantomJS (Deprecated)

Note: PhantomJS is deprecated and no longer maintained. Use Chrome/Firefox headless instead.

---

## 8. Advanced Troubleshooting

### 8.1 Common Exceptions and Solutions

#### NoSuchElementException

**Cause:** Element not found on page

**Solutions:**
```java
// 1. Add explicit wait
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
wait.until(ExpectedConditions.presenceOfElementLocated(By.id("element")));

// 2. Verify locator is correct
// 3. Check if element is in iframe
// 4. Check if element loads dynamically
```

#### StaleElementReferenceException

**Cause:** Element reference is no longer valid (DOM changed)

**Solutions:**
```java
// 1. Re-find the element
WebElement element = driver.findElement(By.id("dynamic"));
element.click();  // If fails, refind:
element = driver.findElement(By.id("dynamic"));
element.click();

// 2. Use explicit wait
wait.until(ExpectedConditions.refreshed(
    ExpectedConditions.elementToBeClickable(By.id("dynamic"))
));

// 3. Retry logic (shown earlier)
```

#### ElementNotInteractableException

**Cause:** Element is present but not visible/clickable

**Solutions:**
```java
// 1. Wait for element to be clickable
wait.until(ExpectedConditions.elementToBeClickable(By.id("button")));

// 2. Scroll element into view
WebElement element = driver.findElement(By.id("button"));
((JavascriptExecutor) driver).executeScript("arguments[0].scrollIntoView(true);", element);

// 3. Use JavaScript click
((JavascriptExecutor) driver).executeScript("arguments[0].click();", element);
```

#### TimeoutException

**Cause:** Wait condition not met within timeout

**Solutions:**
```java
// 1. Increase timeout
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(30));

// 2. Use correct wait condition
// 3. Check network speed/application performance
// 4. Verify element is actually loading
```

#### ElementClickInterceptedException

**Cause:** Another element is covering the target element

**Solutions:**
```java
// 1. Wait for overlay to disappear
wait.until(ExpectedConditions.invisibilityOfElementLocated(By.className("overlay")));

// 2. Scroll element into view
((JavascriptExecutor) driver).executeScript("arguments[0].scrollIntoView(true);", element);

// 3. Use JavaScript click
((JavascriptExecutor) driver).executeScript("arguments[0].click();", element);
```

### 8.2 JavaScript Executor - Advanced Usage

```java
JavascriptExecutor js = (JavascriptExecutor) driver;

// Click element
js.executeScript("arguments[0].click();", element);

// Send keys
js.executeScript("arguments[0].value='Selenium';", element);

// Scroll to element
js.executeScript("arguments[0].scrollIntoView(true);", element);

// Scroll to bottom of page
js.executeScript("window.scrollTo(0, document.body.scrollHeight)");

// Get element attribute
String value = (String) js.executeScript("return arguments[0].value;", element);

// Check if element is present
Boolean isPresent = (Boolean) js.executeScript(
    "return document.getElementById('elementId') != null;"
);

// Highlight element (for debugging)
js.executeScript("arguments[0].style.border='3px solid red'", element);

// Remove attribute
js.executeScript("arguments[0].removeAttribute('readonly')", element);

// Wait for page load complete
js.executeScript("return document.readyState").equals("complete");

// Execute async JavaScript
js.executeAsyncScript(
    "var callback = arguments[arguments.length - 1];" +
    "setTimeout(function(){ callback('Done'); }, 3000);"
);
```

### 8.3 Handling Dynamic Content

```java
// Wait for AJAX calls to complete
public void waitForAjax(int timeoutSeconds) {
    WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(timeoutSeconds));
    wait.until(driver -> {
        JavascriptExecutor js = (JavascriptExecutor) driver;
        return (Boolean) js.executeScript("return jQuery.active == 0");
    });
}

// Wait for Angular to load
public void waitForAngular() {
    JavascriptExecutor js = (JavascriptExecutor) driver;
    js.executeAsyncScript(
        "var callback = arguments[arguments.length - 1];" +
        "angular.element(document.body).injector().get('$browser')." +
        "notifyWhenNoOutstandingRequests(callback);"
    );
}
```

### 8.4 Network Conditions

```java
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.devtools.DevTools;
import org.openqa.selenium.devtools.v120.network.Network;
import org.openqa.selenium.devtools.v120.network.model.ConnectionType;

ChromeDriver driver = new ChromeDriver();
DevTools devTools = driver.getDevTools();
devTools.createSession();

// Enable network
devTools.send(Network.enable(Optional.empty(), Optional.empty(), Optional.empty()));

// Set network conditions (simulate slow 3G)
devTools.send(Network.emulateNetworkConditions(
    false,                      // offline
    100,                        // latency (ms)
    20000,                      // download throughput (bytes/sec)
    10000,                      // upload throughput (bytes/sec)
    Optional.of(ConnectionType.CELLULAR3G)
));
```

### 8.5 Geolocation

```java
ChromeDriver driver = new ChromeDriver();
DevTools devTools = driver.getDevTools();
devTools.createSession();

// Set geolocation
devTools.send(Emulation.setGeolocationOverride(
    Optional.of(37.7749),    // latitude (San Francisco)
    Optional.of(-122.4194),  // longitude
    Optional.of(1)           // accuracy
));
```

### 8.6 Browser Logs

```java
// Get browser console logs
LogEntries logs = driver.manage().logs().get(LogType.BROWSER);
for(LogEntry entry : logs) {
    System.out.println(entry.getLevel() + " " + entry.getMessage());
}

// Get performance logs (Chrome)
LogEntries perfLogs = driver.manage().logs().get(LogType.PERFORMANCE);
```

### 8.7 Cookies Management

```java
// Get all cookies
Set<Cookie> cookies = driver.manage().getCookies();

// Get specific cookie
Cookie cookie = driver.manage().getCookieNamed("sessionId");

// Add cookie
Cookie newCookie = new Cookie("username", "testuser");
driver.manage().addCookie(newCookie);

// Delete cookie
driver.manage().deleteCookieNamed("sessionId");

// Delete all cookies
driver.manage().deleteAllCookies();
```

### 8.8 Browser Navigation

```java
// Navigate to URL
driver.navigate().to("https://example.com");

// Go back
driver.navigate().back();

// Go forward
driver.navigate().forward();

// Refresh page
driver.navigate().refresh();
```

### 8.9 Timeouts Configuration

```java
// Implicit wait (applies to all findElement calls)
driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(10));

// Page load timeout
driver.manage().timeouts().pageLoadTimeout(Duration.ofSeconds(30));

// Script timeout (for async JavaScript)
driver.manage().timeouts().scriptTimeout(Duration.ofSeconds(20));
```

---

## Best Practices for Advanced Selenium

### 1. Use Explicit Waits Over Implicit Waits
- More reliable for dynamic content
- Better control over waiting conditions
- Avoid mixing implicit and explicit waits

### 2. Create Custom Wait Conditions
- Encapsulate complex wait logic
- Make tests more readable
- Reuse across tests

### 3. Handle Exceptions Gracefully
- Use try-catch blocks appropriately
- Implement retry logic for flaky elements
- Log meaningful error messages

### 4. Use Actions Class for Complex Interactions
- More reliable for mouse/keyboard operations
- Mimics real user behavior
- Better for testing interactive UIs

### 5. Take Screenshots on Failure
- Helps debugging
- Include in test reports
- Name with timestamp and test name

### 6. Use Headless Mode for CI/CD
- Faster execution
- Lower resource usage
- Better for automated pipelines

### 7. Manage Browser Options
- Set download directories
- Disable notifications
- Configure for test environment

### 8. Close Resources Properly
- Use try-finally or try-with-resources
- Close all windows/tabs
- Quit driver in @AfterMethod/@After

### 9. Use JavaScript Executor Sparingly
- Only when Selenium methods fail
- Document why it's needed
- Prefer Selenium native methods

### 10. Test in Multiple Browsers
- Use browser-specific options
- Test headless and headed modes
- Verify cross-browser compatibility

---

## Summary

Advanced Selenium WebDriver skills include:

1. **Synchronization**: Custom waits, fluent waits, handling stale elements
2. **Interactions**: Mouse hover, drag-drop, keyboard actions, scrolling
3. **Popups**: Alerts, frames, multiple windows handling
4. **Files**: Upload (simple and complex), download verification
5. **Screenshots**: Full page, element-specific, on-failure capture
6. **Configuration**: Browser options, capabilities, preferences
7. **Headless**: Running tests without GUI for CI/CD
8. **Troubleshooting**: Common exceptions, JavaScript executor, network conditions

These advanced techniques are essential for building robust, maintainable automation frameworks that handle complex web applications effectively.

---

**Next Steps:**
- Practice each advanced concept with hands-on exercises
- Build reusable utility classes for common operations
- Learn Page Object Model to organize advanced Selenium code
- Integrate with testing frameworks (TestNG/JUnit)
- Explore framework development for enterprise projects
