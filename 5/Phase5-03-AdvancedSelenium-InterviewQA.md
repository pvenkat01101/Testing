# Phase 5.1: Advanced Selenium WebDriver - Interview Q&A

## Table of Contents
1. [Advanced Synchronization](#advanced-synchronization)
2. [Complex User Interactions](#complex-user-interactions)
3. [Handling Popups and Windows](#handling-popups-and-windows)
4. [File Operations](#file-operations)
5. [Screenshots and Reporting](#screenshots-and-reporting)
6. [Browser Configuration](#browser-configuration)
7. [Troubleshooting and Best Practices](#troubleshooting-and-best-practices)
8. [Real-World Scenarios](#real-world-scenarios)

---

## Advanced Synchronization

### Q1: What is the difference between Implicit Wait, Explicit Wait, and Fluent Wait?

**Answer:**

**Implicit Wait:**
- Global timeout applied to ALL findElement() calls
- Waits for specified duration before throwing NoSuchElementException
- Set once and applies throughout driver lifetime
- Not recommended with Explicit Wait (can cause unexpected delays)

```java
driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(10));
```

**Explicit Wait:**
- Waits for specific conditions on specific elements
- More flexible with various ExpectedConditions
- Applied to individual elements
- Recommended over Implicit Wait

```java
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
wait.until(ExpectedConditions.elementToBeClickable(By.id("submit")));
```

**Fluent Wait:**
- Most flexible wait mechanism
- Configurable polling interval
- Can ignore specific exceptions
- Custom wait messages
- Best for handling intermittent failures

```java
Wait<WebDriver> wait = new FluentWait<>(driver)
    .withTimeout(Duration.ofSeconds(30))
    .pollingEvery(Duration.ofMillis(500))
    .ignoring(NoSuchElementException.class)
    .withMessage("Element not found");
```

**Key Differences:**
| Feature | Implicit | Explicit | Fluent |
|---------|----------|----------|--------|
| Scope | Global | Specific | Specific |
| Polling | Fixed | Fixed | Configurable |
| Exception Handling | No | No | Yes |
| Conditions | None | Multiple | Custom |
| Best For | Simple tests | Most scenarios | Complex scenarios |

**Interview Tip:** Always prefer Explicit/Fluent Wait over Implicit Wait for better control and reliability.

---

### Q2: How do you handle StaleElementReferenceException?

**Answer:**

**Cause:** Element reference becomes invalid when DOM changes (page refresh, AJAX updates, element recreation).

**Solutions:**

**1. Re-find the Element:**
```java
WebElement element = driver.findElement(By.id("dynamic"));
try {
    element.click();
} catch(StaleElementReferenceException e) {
    element = driver.findElement(By.id("dynamic"));
    element.click();
}
```

**2. Use Explicit Wait with Refresh:**
```java
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
wait.until(ExpectedConditions.refreshed(
    ExpectedConditions.elementToBeClickable(By.id("dynamic"))
));
```

**3. Retry Logic:**
```java
public WebElement retryingFindElement(By by, int maxRetries) {
    int attempts = 0;
    while(attempts < maxRetries) {
        try {
            return driver.findElement(by);
        } catch(StaleElementReferenceException e) {
            attempts++;
            if(attempts >= maxRetries) throw e;
        }
    }
    return null;
}
```

**4. Use Fluent Wait:**
```java
Wait<WebDriver> wait = new FluentWait<>(driver)
    .withTimeout(Duration.ofSeconds(10))
    .pollingEvery(Duration.ofMillis(500))
    .ignoring(StaleElementReferenceException.class);

WebElement element = wait.until(driver -> driver.findElement(By.id("dynamic")));
```

**Best Practices:**
- Avoid storing WebElement references too long
- Re-find elements after page updates
- Use Page Object Model to encapsulate element finding
- Implement retry logic for flaky elements

---

### Q3: How do you create custom Expected Conditions?

**Answer:**

Custom Expected Conditions allow you to wait for specific application states.

**Method 1: Using Lambda (Java 8+):**
```java
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));

wait.until(driver -> {
    WebElement element = driver.findElement(By.id("status"));
    return element.getText().equals("Complete");
});
```

**Method 2: Using ExpectedCondition Interface:**
```java
public class CustomConditions {

    // Wait for element text to match
    public static ExpectedCondition<Boolean> textMatches(By locator, String text) {
        return new ExpectedCondition<Boolean>() {
            public Boolean apply(WebDriver driver) {
                try {
                    WebElement element = driver.findElement(locator);
                    return element.getText().equals(text);
                } catch(Exception e) {
                    return false;
                }
            }

            public String toString() {
                return String.format("text to be: %s", text);
            }
        };
    }

    // Wait for element count
    public static ExpectedCondition<Boolean> numberOfElementsToBe(
        By locator, int expectedCount) {
        return new ExpectedCondition<Boolean>() {
            public Boolean apply(WebDriver driver) {
                List<WebElement> elements = driver.findElements(locator);
                return elements.size() == expectedCount;
            }
        };
    }

    // Wait for attribute value
    public static ExpectedCondition<Boolean> attributeContains(
        By locator, String attribute, String value) {
        return driver -> {
            WebElement element = driver.findElement(locator);
            String attrValue = element.getAttribute(attribute);
            return attrValue != null && attrValue.contains(value);
        };
    }

    // Wait for jQuery AJAX to complete
    public static ExpectedCondition<Boolean> jQueryAjaxComplete() {
        return driver -> {
            JavascriptExecutor js = (JavascriptExecutor) driver;
            return (Boolean) js.executeScript("return jQuery.active == 0");
        };
    }
}

// Usage
wait.until(CustomConditions.textMatches(By.id("status"), "Success"));
wait.until(CustomConditions.numberOfElementsToBe(By.className("item"), 5));
wait.until(CustomConditions.jQueryAjaxComplete());
```

**Real-World Example:**
```java
// Wait for loading spinner to disappear
public static ExpectedCondition<Boolean> spinnerToDisappear() {
    return driver -> {
        List<WebElement> spinners = driver.findElements(By.className("loading-spinner"));
        return spinners.isEmpty() || !spinners.get(0).isDisplayed();
    };
}

wait.until(spinnerToDisappear());
```

**Benefits:**
- Encapsulate complex wait logic
- Reusable across tests
- More readable test code
- Better error messages

---

## Complex User Interactions

### Q4: Explain the Actions class and when to use it.

**Answer:**

The **Actions class** is used for complex user interactions that simulate real user behavior.

**When to Use Actions Class:**
1. Mouse hover operations
2. Drag and drop
3. Right-click (context menu)
4. Double-click
5. Keyboard shortcuts (Ctrl+C, Ctrl+V)
6. Complex click sequences
7. Slider manipulation

**Common Methods:**

**Mouse Operations:**
```java
Actions actions = new Actions(driver);

// Move to element (hover)
actions.moveToElement(element).perform();

// Click
actions.click(element).perform();

// Double-click
actions.doubleClick(element).perform();

// Right-click
actions.contextClick(element).perform();

// Click and hold
actions.clickAndHold(element).perform();

// Release
actions.release().perform();

// Move by offset
actions.moveByOffset(100, 50).perform();
```

**Drag and Drop:**
```java
// Method 1
actions.dragAndDrop(source, target).perform();

// Method 2
actions.clickAndHold(source)
       .moveToElement(target)
       .release()
       .perform();

// Method 3 - By offset
actions.clickAndHold(source)
       .moveByOffset(200, 0)
       .release()
       .perform();
```

**Keyboard Operations:**
```java
// Press and release keys
actions.keyDown(Keys.CONTROL)
       .sendKeys("a")
       .keyUp(Keys.CONTROL)
       .perform();

// Send keys
actions.sendKeys("text").perform();

// Composite action
actions.keyDown(Keys.SHIFT)
       .sendKeys("hello")
       .keyUp(Keys.SHIFT)
       .perform();
```

**Chaining Actions:**
```java
actions.moveToElement(menu)
       .pause(Duration.ofSeconds(1))
       .moveToElement(subMenu)
       .click()
       .perform();
```

**Important Notes:**
- Always call `perform()` to execute actions
- Actions can be chained for complex interactions
- More reliable for hover operations than JavaScript
- Simulates real user behavior
- Use `pause()` for delays between actions

**Example - Menu Navigation:**
```java
Actions actions = new Actions(driver);

WebElement mainMenu = driver.findElement(By.id("menu"));
WebElement subMenu = driver.findElement(By.id("submenu"));
WebElement menuItem = driver.findElement(By.id("item"));

actions.moveToElement(mainMenu)
       .pause(Duration.ofMillis(500))
       .moveToElement(subMenu)
       .pause(Duration.ofMillis(500))
       .click(menuItem)
       .perform();
```

---

### Q5: What's the difference between click() and JavaScript click?

**Answer:**

**Selenium click():**
```java
element.click();
```

**Advantages:**
- Mimics real user interaction
- Validates element is visible and interactable
- Checks if element is enabled
- Scrolls element into view automatically
- Throws appropriate exceptions if element not clickable
- Respects overlays and modals

**Disadvantages:**
- Can fail with ElementClickInterceptedException
- Slower than JS click
- May fail if element is hidden or disabled

**JavaScript click():**
```java
JavascriptExecutor js = (JavascriptExecutor) driver;
js.executeScript("arguments[0].click();", element);
```

**Advantages:**
- Works on hidden elements
- Bypasses visibility checks
- Faster execution
- Works when element is covered by another element
- No scroll required

**Disadvantages:**
- Doesn't mimic real user behavior
- Bypasses validation (can click disabled elements)
- May not trigger all event listeners
- Not a real-world user action
- Can hide test issues

**When to Use Each:**

**Use Selenium click() (Preferred):**
- Normal clickable elements
- When you want to validate element is interactive
- Testing real user scenarios
- When element should be visible

**Use JavaScript click():**
- Hidden elements that need to be clicked programmatically
- When ElementClickInterceptedException can't be resolved
- As a last resort after Selenium click() fails
- Performance-critical scenarios
- When element is covered by fixed headers/footers

**Best Practice:**
```java
// Try Selenium click first
try {
    WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
    WebElement element = wait.until(ExpectedConditions.elementToBeClickable(By.id("btn")));
    element.click();
} catch(ElementClickInterceptedException e) {
    // Fallback to JavaScript click
    System.out.println("Using JavaScript click as fallback");
    JavascriptExecutor js = (JavascriptExecutor) driver;
    js.executeScript("arguments[0].click();", element);
}
```

**Interview Tip:** Always prefer Selenium's native click() as it better represents user behavior. Use JavaScript click() only when necessary and document why.

---

### Q6: How do you handle dynamic dropdowns that appear on hover?

**Answer:**

Dynamic dropdowns require the Actions class for hovering and explicit waits for visibility.

**Approach:**

```java
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
Actions actions = new Actions(driver);

// Step 1: Hover over parent menu
WebElement parentMenu = driver.findElement(By.id("mainMenu"));
actions.moveToElement(parentMenu).perform();

// Step 2: Wait for dropdown to be visible
WebElement dropdown = wait.until(
    ExpectedConditions.visibilityOfElementLocated(By.id("dropdown"))
);

// Step 3: Hover over submenu (if nested)
WebElement subMenu = driver.findElement(By.id("subMenu"));
actions.moveToElement(subMenu).perform();

// Step 4: Wait for final menu item
WebElement menuItem = wait.until(
    ExpectedConditions.elementToBeClickable(By.id("menuItem"))
);

// Step 5: Click
menuItem.click();
```

**Multi-Level Dropdown:**
```java
public void navigateToMenuItem(String... menuPath) {
    Actions actions = new Actions(driver);
    WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));

    for(String menuText : menuPath) {
        WebElement menuElement = wait.until(
            ExpectedConditions.presenceOfElementLocated(
                By.xpath("//a[text()='" + menuText + "']")
            )
        );
        actions.moveToElement(menuElement).perform();

        // Small pause for menu to appear
        actions.pause(Duration.ofMillis(300)).perform();
    }

    // Click the last item
    actions.click().perform();
}

// Usage
navigateToMenuItem("Products", "Electronics", "Mobile Phones");
```

**With Retry Logic:**
```java
public void hoverAndClick(By parentLocator, By childLocator) {
    Actions actions = new Actions(driver);
    WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));

    int attempts = 0;
    int maxAttempts = 3;

    while(attempts < maxAttempts) {
        try {
            WebElement parent = driver.findElement(parentLocator);
            actions.moveToElement(parent).perform();

            WebElement child = wait.until(
                ExpectedConditions.elementToBeClickable(childLocator)
            );
            child.click();
            break;  // Success

        } catch(StaleElementReferenceException | NoSuchElementException e) {
            attempts++;
            if(attempts >= maxAttempts) {
                throw e;
            }
            System.out.println("Retry attempt " + attempts);
        }
    }
}
```

**Common Issues and Solutions:**

1. **Menu disappears too quickly:**
   - Add pause between actions
   - Increase wait timeout
   - Use composite actions

2. **StaleElementReferenceException:**
   - Implement retry logic
   - Re-find elements after hover

3. **Menu doesn't appear:**
   - Verify hover is on correct element
   - Check if JavaScript hover is needed
   - Verify element is in viewport

**Alternative - JavaScript Hover:**
```java
JavascriptExecutor js = (JavascriptExecutor) driver;
String script = "var event = new MouseEvent('mouseover', {" +
                "'view': window," +
                "'bubbles': true," +
                "'cancelable': true" +
                "}); arguments[0].dispatchEvent(event);";
js.executeScript(script, parentMenu);
```

---

## Handling Popups and Windows

### Q7: How do you handle JavaScript alerts, confirms, and prompts?

**Answer:**

Selenium provides the `Alert` interface to handle JavaScript popups.

**Types of JavaScript Popups:**

**1. Simple Alert:**
```java
// Click button that triggers alert
driver.findElement(By.id("alertButton")).click();

// Switch to alert
Alert alert = driver.switchTo().alert();

// Get alert text
String alertText = alert.getText();
System.out.println("Alert: " + alertText);

// Accept (click OK)
alert.accept();
```

**2. Confirmation Dialog:**
```java
driver.findElement(By.id("confirmButton")).click();

Alert confirmAlert = driver.switchTo().alert();
System.out.println("Confirm: " + confirmAlert.getText());

// Accept (OK)
confirmAlert.accept();

// OR Dismiss (Cancel)
// confirmAlert.dismiss();
```

**3. Prompt Dialog:**
```java
driver.findElement(By.id("promptButton")).click();

Alert promptAlert = driver.switchTo().alert();
System.out.println("Prompt: " + promptAlert.getText());

// Send text
promptAlert.sendKeys("Selenium WebDriver");

// Accept
promptAlert.accept();
```

**With Explicit Wait:**
```java
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));

// Wait for alert to appear
Alert alert = wait.until(ExpectedConditions.alertIsPresent());
alert.accept();
```

**Alert Utility Methods:**
```java
public class AlertHandler {

    public static String getAlertText(WebDriver driver) {
        WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(5));
        Alert alert = wait.until(ExpectedConditions.alertIsPresent());
        return alert.getText();
    }

    public static void acceptAlert(WebDriver driver) {
        Alert alert = driver.switchTo().alert();
        alert.accept();
    }

    public static void dismissAlert(WebDriver driver) {
        Alert alert = driver.switchTo().alert();
        alert.dismiss();
    }

    public static void sendTextToPrompt(WebDriver driver, String text) {
        Alert alert = driver.switchTo().alert();
        alert.sendKeys(text);
        alert.accept();
    }

    public static boolean isAlertPresent(WebDriver driver) {
        try {
            driver.switchTo().alert();
            return true;
        } catch(NoAlertPresentException e) {
            return false;
        }
    }
}
```

**Handling Unexpected Alerts:**
```java
// In @AfterMethod or catch block
try {
    driver.switchTo().alert().accept();
} catch(NoAlertPresentException e) {
    // No alert present, continue
}
```

**Important Notes:**
- Use `switchTo().alert()` to switch control to alert
- Alert must be present before switching (use explicit wait)
- After accepting/dismissing, control returns to main page
- Can't use regular locators on alerts (not part of DOM)
- getText(), accept(), dismiss(), sendKeys() are only methods available

**Interview Tip:** Explain that JavaScript alerts are modal and block page interaction until handled. Always use explicit wait before switching to alert to avoid NoAlertPresentException.

---

### Q8: How do you handle iFrames? Explain with nested iFrames.

**Answer:**

**iFrame Basics:**
iFrames are separate HTML documents embedded in a page. You must switch to a frame before interacting with its elements.

**Switching to Frame - 3 Ways:**

**1. By Index:**
```java
driver.switchTo().frame(0);  // First frame
```

**2. By Name or ID:**
```java
driver.switchTo().frame("frameName");
driver.switchTo().frame("frameId");
```

**3. By WebElement (Recommended):**
```java
WebElement frameElement = driver.findElement(By.id("myFrame"));
driver.switchTo().frame(frameElement);
```

**Complete Example:**
```java
// Switch to frame
driver.switchTo().frame("contentFrame");

// Interact with elements inside frame
WebElement element = driver.findElement(By.id("insideFrame"));
element.sendKeys("Hello");

// Switch back to main content
driver.switchTo().defaultContent();

// Now can interact with main page elements
driver.findElement(By.id("outsideFrame")).click();
```

**Nested iFrames:**
```java
// HTML Structure:
// <html>
//   <iframe id="outerFrame">
//     <iframe id="innerFrame">
//       <input id="targetElement">
//     </iframe>
//   </iframe>
// </html>

// Navigate through nested frames
driver.switchTo().frame("outerFrame");      // Switch to outer frame
driver.switchTo().frame("innerFrame");      // Switch to inner frame (nested)

// Interact with element in innermost frame
driver.findElement(By.id("targetElement")).sendKeys("Nested frame content");

// Go back one level to outer frame
driver.switchTo().parentFrame();

// Or go back to main content
driver.switchTo().defaultContent();
```

**Frame Navigation Methods:**
```java
// Switch to frame
driver.switchTo().frame(frameElement);

// Switch back to parent frame
driver.switchTo().parentFrame();

// Switch back to main content (top level)
driver.switchTo().defaultContent();
```

**Utility Class for Frames:**
```java
public class FrameHandler {

    public static void switchToFrameByIdOrName(WebDriver driver, String idOrName) {
        driver.switchTo().frame(idOrName);
    }

    public static void switchToFrameByElement(WebDriver driver, By locator) {
        WebElement frameElement = driver.findElement(locator);
        driver.switchTo().frame(frameElement);
    }

    public static void switchToFrameByIndex(WebDriver driver, int index) {
        driver.switchTo().frame(index);
    }

    public static int getFrameCount(WebDriver driver) {
        return driver.findElements(By.tagName("iframe")).size();
    }

    public static void switchToParent(WebDriver driver) {
        driver.switchTo().parentFrame();
    }

    public static void switchToDefault(WebDriver driver) {
        driver.switchTo().defaultContent();
    }

    public static boolean isFrameAvailable(WebDriver driver, String frameIdOrName) {
        try {
            driver.switchTo().frame(frameIdOrName);
            driver.switchTo().defaultContent();
            return true;
        } catch(NoSuchFrameException e) {
            return false;
        }
    }
}
```

**With Explicit Wait:**
```java
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));

// Wait for frame to be available and switch
wait.until(ExpectedConditions.frameToBeAvailableAndSwitchToIt("frameName"));

// OR with By locator
wait.until(ExpectedConditions.frameToBeAvailableAndSwitchToIt(By.id("frameId")));

// OR with WebElement
WebElement frame = driver.findElement(By.id("myFrame"));
wait.until(ExpectedConditions.frameToBeAvailableAndSwitchToIt(frame));
```

**Common Mistakes:**

1. **Forgetting to switch to frame:**
```java
// WRONG - NoSuchElementException
driver.findElement(By.id("insideFrame")).click();

// CORRECT
driver.switchTo().frame("myFrame");
driver.findElement(By.id("insideFrame")).click();
driver.switchTo().defaultContent();
```

2. **Not switching back to main content:**
```java
driver.switchTo().frame("frame1");
// ... do work ...
// PROBLEM: Still in frame context

// SOLUTION: Always switch back
driver.switchTo().defaultContent();
```

3. **Wrong frame navigation:**
```java
// If in innerFrame and want to go to outerFrame
// WRONG
driver.switchTo().frame("outerFrame");  // Will look for outerFrame inside innerFrame

// CORRECT
driver.switchTo().parentFrame();  // Go up one level
// OR
driver.switchTo().defaultContent();  // Go to main content
driver.switchTo().frame("outerFrame");  // Then switch to outer frame
```

**Real-World Example:**
```java
@Test
public void testIframeContent() {
    driver.get("https://www.example.com");

    // Count frames
    int frameCount = driver.findElements(By.tagName("iframe")).size();
    System.out.println("Total frames: " + frameCount);

    // Wait for frame to be available
    WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
    wait.until(ExpectedConditions.frameToBeAvailableAndSwitchToIt("contentFrame"));

    // Interact with frame content
    WebElement editor = wait.until(
        ExpectedConditions.presenceOfElementLocated(By.id("editor"))
    );
    editor.clear();
    editor.sendKeys("Hello from Selenium");

    // Switch back to main content
    driver.switchTo().defaultContent();

    // Verify we're back on main page
    String pageTitle = driver.findElement(By.tagName("h1")).getText();
    System.out.println("Page title: " + pageTitle);
}
```

**Interview Tips:**
- Explain that frames are separate documents with their own DOM
- Must switch context before interacting with frame elements
- Always switch back to defaultContent() after frame operations
- Use parentFrame() for nested frames
- frameToBeAvailableAndSwitchToIt() is best practice with explicit wait

---

### Q9: How do you handle multiple browser windows/tabs in Selenium?

**Answer:**

**Window Handle Basics:**
Each browser window/tab has a unique identifier called "window handle" (alphanumeric string).

**Key Methods:**
```java
// Get current window handle
String mainWindow = driver.getWindowHandle();

// Get all window handles
Set<String> allWindows = driver.getWindowHandles();

// Switch to window
driver.switchTo().window(windowHandle);

// Get window count
int windowCount = driver.getWindowHandles().size();
```

**Scenario 1: Switching to New Window:**
```java
// Main window
driver.get("https://www.example.com");
String mainWindow = driver.getWindowHandle();

// Click link that opens new window
driver.findElement(By.id("newWindowLink")).click();

// Get all windows
Set<String> allWindows = driver.getWindowHandles();

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

**Scenario 2: Opening New Tab (Selenium 4):**
```java
// Get main window
String originalWindow = driver.getWindowHandle();

// Open new tab
driver.switchTo().newWindow(WindowType.TAB);

// Navigate in new tab
driver.get("https://www.example.com");

// Switch back to original tab
driver.switchTo().window(originalWindow);

// Open new window (not tab)
driver.switchTo().newWindow(WindowType.WINDOW);
```

**Utility Methods:**
```java
public class WindowHandler {

    // Switch to window by title
    public static void switchToWindowByTitle(WebDriver driver, String title) {
        Set<String> windows = driver.getWindowHandles();
        for(String window : windows) {
            driver.switchTo().window(window);
            if(driver.getTitle().equals(title)) {
                return;
            }
        }
        throw new NoSuchWindowException("Window with title '" + title + "' not found");
    }

    // Switch to window by URL
    public static void switchToWindowByUrl(WebDriver driver, String url) {
        Set<String> windows = driver.getWindowHandles();
        for(String window : windows) {
            driver.switchTo().window(window);
            if(driver.getCurrentUrl().contains(url)) {
                return;
            }
        }
    }

    // Close all windows except main
    public static void closeAllExceptMain(WebDriver driver, String mainWindow) {
        Set<String> windows = driver.getWindowHandles();
        for(String window : windows) {
            if(!window.equals(mainWindow)) {
                driver.switchTo().window(window);
                driver.close();
            }
        }
        driver.switchTo().window(mainWindow);
    }

    // Get current window count
    public static int getWindowCount(WebDriver driver) {
        return driver.getWindowHandles().size();
    }

    // Switch to latest opened window
    public static void switchToLatestWindow(WebDriver driver) {
        Set<String> windows = driver.getWindowHandles();
        String latestWindow = "";
        for(String window : windows) {
            latestWindow = window;
        }
        driver.switchTo().window(latestWindow);
    }
}
```

**Complete Example:**
```java
@Test
public void testMultipleWindows() {
    driver.get("https://www.example.com");
    String mainWindow = driver.getWindowHandle();

    // Open new window
    driver.findElement(By.id("openWindow")).click();

    // Wait for new window
    WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
    wait.until(driver -> driver.getWindowHandles().size() == 2);

    // Switch to new window using utility
    WindowHandler.switchToLatestWindow(driver);

    // Perform actions
    System.out.println("New window URL: " + driver.getCurrentUrl());
    WebElement element = wait.until(
        ExpectedConditions.presenceOfElementLocated(By.id("content"))
    );
    element.click();

    // Close new window
    driver.close();

    // Switch back
    driver.switchTo().window(mainWindow);

    // Verify back on main window
    assert driver.getWindowHandles().size() == 1;
}
```

**Handling Multiple Windows:**
```java
// Store all windows in list
List<String> windows = new ArrayList<>(driver.getWindowHandles());

// Switch to first window
driver.switchTo().window(windows.get(0));

// Switch to second window
driver.switchTo().window(windows.get(1));

// Switch to nth window
driver.switchTo().window(windows.get(n));
```

**Best Practices:**

1. **Always store main window handle:**
```java
String mainWindow = driver.getWindowHandle();
```

2. **Wait for new window to open:**
```java
wait.until(driver -> driver.getWindowHandles().size() > 1);
```

3. **Close child windows before switching:**
```java
driver.close();  // Closes current window
driver.switchTo().window(mainWindow);  // Switch back
```

4. **Use quit() vs close():**
```java
driver.close();  // Closes current window only
driver.quit();   // Closes all windows and ends session
```

**Common Issues:**

1. **NoSuchWindowException:**
   - Window handle no longer valid
   - Window was closed
   - Switch to valid window handle

2. **Window not found:**
   - Add explicit wait for window to open
   - Verify window actually opens
   - Check for popups being blocked

**Interview Tips:**
- Explain difference between window handle and window index
- Mention Selenium 4's newWindow() feature
- Discuss importance of switching back to main window
- Explain close() vs quit() difference
- Know how to handle dynamic window counts

---

## File Operations

### Q10: How do you upload files in Selenium? What if the upload dialog is not standard?

**Answer:**

**Standard File Upload (input type="file"):**

This is the most common and simplest approach:

```java
// Find the file input element
WebElement uploadElement = driver.findElement(By.id("fileUpload"));

// Send absolute file path
String filePath = "/Users/username/Documents/test.pdf";
uploadElement.sendKeys(filePath);

// Click upload button
driver.findElement(By.id("uploadButton")).click();
```

**Important Points:**
- Use `sendKeys()` directly on file input element
- File path must be absolute
- No need to click "Browse" button
- Works for most standard HTML file inputs

**Multiple File Upload:**
```java
WebElement uploadElement = driver.findElement(By.id("multipleFileUpload"));

String file1 = "/path/to/file1.txt";
String file2 = "/path/to/file2.txt";

// Separate multiple files with newline character
uploadElement.sendKeys(file1 + "\n" + file2);
```

**Non-Standard Upload Dialog (Windows File Dialog):**

When clicking browse opens OS native dialog, Selenium cannot interact with it directly. Use one of these approaches:

**Method 1: Robot Class (Windows)**
```java
import java.awt.Robot;
import java.awt.Toolkit;
import java.awt.datatransfer.StringSelection;
import java.awt.event.KeyEvent;

// Click browse button that opens dialog
driver.findElement(By.id("browseButton")).click();

// Wait for dialog to open
Thread.sleep(2000);

// Copy file path to clipboard
StringSelection filePath = new StringSelection("C:\\Users\\test\\document.pdf");
Toolkit.getDefaultToolkit().getSystemClipboard().setContents(filePath, null);

// Use Robot to paste and press Enter
Robot robot = new Robot();

// Paste (Ctrl + V)
robot.keyPress(KeyEvent.VK_CONTROL);
robot.keyPress(KeyEvent.VK_V);
robot.keyRelease(KeyEvent.VK_V);
robot.keyRelease(KeyEvent.VK_CONTROL);

// Wait a bit
Thread.sleep(1000);

// Press Enter
robot.keyPress(KeyEvent.VK_ENTER);
robot.keyRelease(KeyEvent.VK_ENTER);
```

**Limitations of Robot Class:**
- Platform-dependent (Windows, Mac, Linux need different key codes)
- Unreliable (timing issues)
- Not recommended for CI/CD pipelines
- Focus must be on correct window

**Method 2: AutoIT (Windows Only)**

1. Install AutoIT
2. Create AutoIT script (.au3):
```autoit
WinWaitActive("Open")  ; Wait for file dialog
Send("C:\path\to\file.txt")  ; Type file path
Send("{ENTER}")  ; Press Enter
```

3. Compile to .exe
4. Execute from Java:
```java
driver.findElement(By.id("browseButton")).click();
Runtime.getRuntime().exec("C:\\path\\to\\FileUpload.exe");
Thread.sleep(3000);
```

**Method 3: JavaScript (Recommended Workaround)**

If possible, set file path directly using JavaScript:

```java
JavascriptExecutor js = (JavascriptExecutor) driver;
WebElement fileInput = driver.findElement(By.id("hiddenFileInput"));

// Make hidden input visible
js.executeScript("arguments[0].style.display='block';", fileInput);

// Send file path
fileInput.sendKeys("/path/to/file.txt");
```

**Method 4: Sikuli (Image-Based Automation)**

Uses image recognition to interact with native dialogs:

```java
import org.sikuli.script.*;

driver.findElement(By.id("browseButton")).click();

Screen screen = new Screen();
screen.wait("browse_button_image.png", 5);
screen.type("C:\\path\\to\\file.txt");
screen.type(Key.ENTER);
```

**Best Practice Approach:**
```java
public class FileUploadHandler {

    public static void uploadFile(WebDriver driver, By uploadElement, String filePath) {
        // Create file and verify it exists
        File file = new File(filePath);
        if(!file.exists()) {
            throw new IllegalArgumentException("File does not exist: " + filePath);
        }

        // Get absolute path
        String absolutePath = file.getAbsolutePath();

        // Find upload element
        WebElement element = driver.findElement(uploadElement);

        // Send file path
        element.sendKeys(absolutePath);

        System.out.println("File uploaded: " + absolutePath);
    }

    public static void createTestFile(String fileName, String content) {
        try {
            File file = new File(fileName);
            FileWriter writer = new FileWriter(file);
            writer.write(content);
            writer.close();
        } catch(IOException e) {
            e.printStackTrace();
        }
    }
}

// Usage
String testFile = System.getProperty("user.dir") + "/testfile.txt";
FileUploadHandler.createTestFile(testFile, "Test content");
FileUploadHandler.uploadFile(driver, By.id("fileUpload"), testFile);
```

**Complete Example:**
```java
@Test
public void testFileUpload() {
    driver.get("https://the-internet.herokuapp.com/upload");

    // Create test file
    String fileName = "test-upload.txt";
    String filePath = System.getProperty("user.dir") + "/" + fileName;

    try {
        File file = new File(filePath);
        FileWriter writer = new FileWriter(file);
        writer.write("This is test file content");
        writer.close();
    } catch(IOException e) {
        e.printStackTrace();
    }

    // Upload file
    WebElement uploadElement = driver.findElement(By.id("file-upload"));
    uploadElement.sendKeys(filePath);

    // Submit
    driver.findElement(By.id("file-submit")).click();

    // Verify upload
    WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
    WebElement successMessage = wait.until(
        ExpectedConditions.presenceOfElementLocated(By.tagName("h3"))
    );

    assert successMessage.getText().equals("File Uploaded!");
    System.out.println("Upload successful!");
}
```

**Interview Tips:**
- Always prefer sendKeys() for standard file inputs
- Explain Robot class as fallback but mention limitations
- Discuss cross-platform compatibility issues
- Mention that JavaScript workaround is most reliable for non-standard uploads
- Explain that file path must be absolute, not relative
- Know how to create test files programmatically

---

### Q11: How do you verify file download in Selenium?

**Answer:**

Selenium cannot directly verify downloads, but you can configure download directory and verify file existence.

**Step 1: Configure Download Directory**

**Chrome:**
```java
String downloadPath = System.getProperty("user.dir") + "/downloads";

// Create directory if doesn't exist
File downloadDir = new File(downloadPath);
if(!downloadDir.exists()) {
    downloadDir.mkdir();
}

ChromeOptions options = new ChromeOptions();
Map<String, Object> prefs = new HashMap<>();
prefs.put("download.default_directory", downloadPath);
prefs.put("download.prompt_for_download", false);  // Disable download prompt
prefs.put("plugins.always_open_pdf_externally", true);  // Download PDFs instead of opening

options.setExperimentalOption("prefs", prefs);

WebDriver driver = new ChromeDriver(options);
```

**Firefox:**
```java
String downloadPath = System.getProperty("user.dir") + "/downloads";

FirefoxOptions options = new FirefoxOptions();
options.addPreference("browser.download.folderList", 2);  // Custom location
options.addPreference("browser.download.dir", downloadPath);
options.addPreference("browser.download.useDownloadDir", true);
options.addPreference("browser.helperApps.neverAsk.saveToDisk",
    "application/pdf,application/zip,text/csv,application/vnd.ms-excel");
options.addPreference("pdfjs.disabled", true);  // Disable PDF viewer

WebDriver driver = new FirefoxDriver(options);
```

**Step 2: Download File**
```java
driver.get("https://example.com/downloads");
driver.findElement(By.linkText("Download File")).click();
```

**Step 3: Verify Download**
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
String fileName = "report.pdf";
boolean downloaded = isFileDownloaded(downloadPath, fileName, 30);
assert downloaded : "File was not downloaded";
```

**Complete Download Handler:**
```java
public class DownloadHandler {

    public static boolean waitForFileDownload(String downloadPath, String fileName, int timeout) {
        File file = new File(downloadPath, fileName);
        int elapsed = 0;

        while(elapsed < timeout) {
            if(file.exists()) {
                // Wait for file to finish downloading (size stops changing)
                long initialSize = file.length();
                try {
                    Thread.sleep(1000);
                } catch(InterruptedException e) {
                    e.printStackTrace();
                }
                long finalSize = file.length();

                if(initialSize == finalSize && finalSize > 0) {
                    return true;
                }
            }

            try {
                Thread.sleep(1000);
            } catch(InterruptedException e) {
                e.printStackTrace();
            }
            elapsed++;
        }
        return false;
    }

    public static long getFileSize(String filePath) {
        File file = new File(filePath);
        return file.exists() ? file.length() : 0;
    }

    public static void deleteFile(String filePath) {
        File file = new File(filePath);
        if(file.exists()) {
            file.delete();
        }
    }

    public static void cleanDownloadDirectory(String downloadPath) {
        File dir = new File(downloadPath);
        if(dir.exists() && dir.isDirectory()) {
            for(File file : dir.listFiles()) {
                file.delete();
            }
        }
    }

    public static List<String> getDownloadedFiles(String downloadPath) {
        List<String> files = new ArrayList<>();
        File dir = new File(downloadPath);
        if(dir.exists() && dir.isDirectory()) {
            for(File file : dir.listFiles()) {
                if(file.isFile()) {
                    files.add(file.getName());
                }
            }
        }
        return files;
    }
}
```

**Test Example:**
```java
@Test
public void testFileDownload() {
    String downloadPath = System.getProperty("user.dir") + "/downloads";
    String fileName = "sample.pdf";

    // Clean download directory before test
    DownloadHandler.cleanDownloadDirectory(downloadPath);

    // Navigate and download
    driver.get("https://www.example.com/downloads");
    driver.findElement(By.linkText("Download PDF")).click();

    // Verify download
    boolean downloaded = DownloadHandler.waitForFileDownload(downloadPath, fileName, 30);
    assert downloaded : "File was not downloaded within 30 seconds";

    // Verify file size
    long fileSize = DownloadHandler.getFileSize(downloadPath + "/" + fileName);
    System.out.println("Downloaded file size: " + fileSize + " bytes");
    assert fileSize > 0 : "Downloaded file is empty";

    // Clean up
    DownloadHandler.deleteFile(downloadPath + "/" + fileName);
}
```

**Handling .crdownload files (Chrome):**
```java
public static boolean waitForChromeDownloadComplete(String downloadPath, int timeout) {
    File dir = new File(downloadPath);
    int elapsed = 0;

    while(elapsed < timeout) {
        File[] files = dir.listFiles((dir1, name) -> name.endsWith(".crdownload"));

        // No .crdownload files means download complete
        if(files == null || files.length == 0) {
            return true;
        }

        try {
            Thread.sleep(1000);
        } catch(InterruptedException e) {
            e.printStackTrace();
        }
        elapsed++;
    }
    return false;
}
```

**Download with Original File Name:**
```java
public static String getLatestDownloadedFile(String downloadPath) {
    File dir = new File(downloadPath);
    File[] files = dir.listFiles();

    if(files == null || files.length == 0) {
        return null;
    }

    File lastModifiedFile = files[0];
    for(File file : files) {
        if(file.lastModified() > lastModifiedFile.lastModified()) {
            lastModifiedFile = file;
        }
    }

    return lastModifiedFile.getName();
}

// Usage - when you don't know the exact file name
driver.findElement(By.id("downloadBtn")).click();
Thread.sleep(3000);  // Wait for download to start
String downloadedFileName = getLatestDownloadedFile(downloadPath);
System.out.println("Downloaded file: " + downloadedFileName);
```

**Interview Tips:**
- Explain that Selenium cannot directly verify downloads
- Must configure download directory in browser options
- File polling is necessary to verify download completion
- Mention .crdownload files in Chrome
- Discuss cleaning download directory before/after tests
- Explain timeout mechanism for slow downloads
- Know how to handle unknown file names using lastModified

---

(Continuing with remaining questions in next part due to length...)

## Screenshots and Reporting

### Q12: Explain different ways to capture screenshots in Selenium.

**Answer:**

Selenium provides multiple ways to capture screenshots for test reporting and debugging.

**1. Full Page Screenshot:**
```java
// Cast driver to TakesScreenshot
TakesScreenshot ts = (TakesScreenshot) driver;

// Capture as FILE
File source = ts.getScreenshotAs(OutputType.FILE);

// Save to destination
File destination = new File("./screenshots/page.png");
FileUtils.copyFile(source, destination);

// OR using Files.copy
Files.copy(source.toPath(), destination.toPath());
```

**2. Element Screenshot (Selenium 4+):**
```java
WebElement element = driver.findElement(By.id("logo"));

// Take screenshot of specific element
File source = element.getScreenshotAs(OutputType.FILE);
File destination = new File("./screenshots/element.png");
FileUtils.copyFile(source, destination);
```

**3. Screenshot as Base64:**
```java
TakesScreenshot ts = (TakesScreenshot) driver;
String base64Screenshot = ts.getScreenshotAs(OutputType.BASE64);

// Can be embedded in HTML reports
String htmlImage = "<img src='data:image/png;base64," + base64Screenshot + "' />";
```

**4. Screenshot as Bytes:**
```java
TakesScreenshot ts = (TakesScreenshot) driver;
byte[] screenshot = ts.getScreenshotAs(OutputType.BYTES);

// Useful for streaming or network operations
```

**Screenshot Utility Class:**
```java
public class ScreenshotUtil {

    public static String captureScreenshot(WebDriver driver, String testName) {
        try {
            TakesScreenshot ts = (TakesScreenshot) driver;
            File source = ts.getScreenshotAs(OutputType.FILE);

            String timestamp = new SimpleDateFormat("yyyyMMdd_HHmmss").format(new Date());
            String fileName = testName + "_" + timestamp + ".png";
            String screenshotPath = System.getProperty("user.dir") + "/screenshots/" + fileName;

            File destination = new File(screenshotPath);
            FileUtils.copyFile(source, destination);

            System.out.println("Screenshot saved: " + fileName);
            return screenshotPath;

        } catch(Exception e) {
            System.out.println("Failed to capture screenshot: " + e.getMessage());
            return null;
        }
    }

    public static String captureElementScreenshot(WebElement element, String elementName) {
        try {
            File source = element.getScreenshotAs(OutputType.FILE);

            String timestamp = new SimpleDateFormat("yyyyMMdd_HHmmss").format(new Date());
            String fileName = elementName + "_" + timestamp + ".png";
            String path = "./screenshots/" + fileName;

            FileUtils.copyFile(source, new File(path));
            return path;

        } catch(Exception e) {
            System.out.println("Failed to capture element screenshot: " + e.getMessage());
            return null;
        }
    }

    public static void captureScreenshotOnFailure(WebDriver driver, String testName) {
        if(driver != null) {
            captureScreenshot(driver, testName + "_FAILED");
        }
    }

    public static String getBase64Screenshot(WebDriver driver) {
        TakesScreenshot ts = (TakesScreenshot) driver;
        return ts.getScreenshotAs(OutputType.BASE64);
    }
}
```

**Integration with TestNG:**
```java
public class BaseTest {
    WebDriver driver;

    @BeforeMethod
    public void setup() {
        driver = new ChromeDriver();
        // Create screenshots directory
        new File("./screenshots").mkdirs();
    }

    @AfterMethod
    public void tearDown(ITestResult result) {
        // Capture screenshot on failure
        if(result.getStatus() == ITestResult.FAILURE) {
            ScreenshotUtil.captureScreenshot(driver, result.getName());
        }

        if(driver != null) {
            driver.quit();
        }
    }
}
```

**Full Page Screenshot using Chrome DevTools (Selenium 4):**
```java
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.devtools.DevTools;
import org.openqa.selenium.devtools.v120.page.Page;

ChromeDriver driver = new ChromeDriver();
DevTools devTools = driver.getDevTools();
devTools.createSession();

// Capture full page (beyond viewport)
var screenshot = devTools.send(Page.captureScreenshot(
    Optional.empty(),
    Optional.empty(),
    Optional.empty(),
    Optional.empty(),
    Optional.of(true)  // Capture beyond viewport
));

// Decode and save
byte[] imageBytes = Base64.getDecoder().decode(screenshot);
Files.write(Paths.get("fullpage.png"), imageBytes);
```

**Best Practices:**

1. **Include Timestamp:**
```java
String timestamp = new SimpleDateFormat("yyyyMMdd_HHmmss").format(new Date());
```

2. **Organize by Test Name:**
```java
String path = "./screenshots/" + testClassName + "/" + testName + "_" + timestamp + ".png";
```

3. **Screenshot on Failure Only:**
```java
@AfterMethod
public void tearDown(ITestResult result) {
    if(result.getStatus() == ITestResult.FAILURE) {
        captureScreenshot(result.getName());
    }
}
```

4. **Clean Old Screenshots:**
```java
public static void cleanOldScreenshots(String directory, int daysOld) {
    File dir = new File(directory);
    long cutoffTime = System.currentTimeMillis() - (daysOld * 24 * 60 * 60 * 1000);

    for(File file : dir.listFiles()) {
        if(file.lastModified() < cutoffTime) {
            file.delete();
        }
    }
}
```

5. **Attach to Reports:**
```java
// For Extent Reports
test.addScreenCaptureFromPath(screenshotPath);

// For Allure
@Attachment(value = "Screenshot", type = "image/png")
public byte[] saveScreenshot() {
    return ((TakesScreenshot) driver).getScreenshotAs(OutputType.BYTES);
}
```

**Interview Tips:**
- Explain TakesScreenshot interface
- Mention Selenium 4's element screenshot feature
- Discuss OutputType options (FILE, BYTES, BASE64)
- Explain integration with test frameworks and reports
- Know how to capture full page vs viewport
- Discuss screenshot naming conventions and organization

---

(Due to length constraints, I'll summarize the remaining questions briefly. Let me know if you need detailed answers for specific questions.)

## Q13-25: Additional Interview Questions (Summary)

**Q13: Browser Options and Capabilities**
- ChromeOptions, FirefoxOptions configuration
- Headless mode setup
- Download directory, notifications, extensions
- Accept insecure certificates

**Q14: Headless Browser Testing**
- When and why to use headless
- --headless=new flag
- Window size configuration
- CI/CD integration benefits

**Q15: JavaScript Executor Use Cases**
- When Selenium methods fail
- Clicking hidden elements
- Scrolling operations
- Removing attributes
- Page load checking

**Q16: Handling Dynamic Content**
- AJAX wait strategies
- Custom conditions for Angular/React
- Polling mechanisms
- jQuery.active checking

**Q17: Cross-Browser Testing**
- WebDriverManager usage
- Browser-specific options
- Handling browser differences
- Selenium Grid for parallel execution

**Q18: Exception Handling**
- NoSuchElementException
- TimeoutException
- ElementNotInteractableException
- StaleElementReferenceException
- Retry mechanisms

**Q19: Cookies Management**
- Adding/deleting cookies
- Session management
- Authentication via cookies
- Cookie expiry handling

**Q20: Page Load Strategies**
- NORMAL, EAGER, NONE
- When to use each
- Performance implications

**Q21: Network Interception (Selenium 4)**
- Chrome DevTools Protocol
- Network conditions
- Request/response monitoring
- Performance metrics

**Q22: Geolocation Testing**
- Setting geolocation
- Testing location-based features
- Chrome DevTools for geolocation

**Q23: Browser Logs**
- Console logs
- Performance logs
- Network logs
- Debugging with logs

**Q24: Best Practices**
- Explicit waits over implicit
- Page Object Model
- Independent tests
- Proper cleanup
- Error handling

**Q25: Framework Integration**
- TestNG/JUnit integration
- Extent Reports
- Allure Reports
- CI/CD pipelines
- Parallel execution

---

**Interview Success Tips:**

1. **Understand the "Why":** Don't just memorize syntax, understand when and why to use each technique

2. **Provide Examples:** Always back up your answers with code examples

3. **Discuss Alternatives:** Mention multiple approaches and their trade-offs

4. **Real-World Experience:** Share challenges you've faced and how you solved them

5. **Best Practices:** Always mention industry best practices

6. **Latest Features:** Know Selenium 4 features (relative locators, element screenshots, newWindow, etc.)

7. **Framework Knowledge:** Connect advanced Selenium with framework concepts

8. **Performance:** Discuss optimization techniques (headless, parallel execution)

9. **CI/CD:** Explain how Selenium tests integrate with CI/CD pipelines

10. **Troubleshooting:** Be ready to discuss common issues and debugging techniques

---

**Recommended Study Approach:**

1. Practice each concept hands-on
2. Build a comprehensive automation framework
3. Contribute to open-source projects
4. Stay updated with Selenium releases
5. Learn complementary tools (TestNG, Maven, Jenkins)
6. Master Page Object Model and design patterns
7. Understand test automation architecture
8. Learn API testing for full-stack automation
9. Practice on real-world applications
10. Prepare scenarios for behavioral questions

---

**Next Steps After Mastering Advanced Selenium:**

1. ✅ Page Object Model (POM) design pattern
2. ✅ Framework Development (hybrid frameworks)
3. ✅ API Testing with REST Assured
4. ✅ CI/CD Integration (Jenkins, GitHub Actions)
5. ✅ Docker for test execution
6. ✅ Selenium Grid for parallel testing
7. ✅ BDD with Cucumber
8. ✅ Performance testing basics
9. ✅ Cloud testing platforms (BrowserStack, Sauce Labs)
10. ✅ Test management tools (Jira, TestRail)

Good luck with your interviews! 🚀
