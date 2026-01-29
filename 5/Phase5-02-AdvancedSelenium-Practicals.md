# Phase 5.1: Advanced Selenium WebDriver - Practicals

## Table of Contents
1. [Setup and Prerequisites](#setup-and-prerequisites)
2. [Advanced Synchronization Exercises](#advanced-synchronization-exercises)
3. [Complex User Interactions](#complex-user-interactions)
4. [Handling Alerts, Frames, and Windows](#handling-alerts-frames-and-windows)
5. [File Upload and Download](#file-upload-and-download)
6. [Screenshots and Reporting](#screenshots-and-reporting)
7. [Browser Configuration](#browser-configuration)
8. [Real-World Scenarios](#real-world-scenarios)

---

## Setup and Prerequisites

### Required Dependencies (pom.xml)

```xml
<dependencies>
    <!-- Selenium WebDriver -->
    <dependency>
        <groupId>org.seleniumhq.selenium</groupId>
        <artifactId>selenium-java</artifactId>
        <version>4.16.1</version>
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
        <version>7.8.0</version>
        <scope>test</scope>
    </dependency>

    <!-- Apache Commons IO (for file operations) -->
    <dependency>
        <groupId>commons-io</groupId>
        <artifactId>commons-io</artifactId>
        <version>2.15.0</version>
    </dependency>
</dependencies>
```

---

## Exercise 1: Custom Explicit Wait with Lambda

**Objective:** Create custom wait conditions using lambda expressions.

**Test Website:** https://the-internet.herokuapp.com/dynamic_loading/1

```java
import org.openqa.selenium.*;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.support.ui.WebDriverWait;
import org.testng.annotations.*;
import io.github.bonigarcia.wdm.WebDriverManager;
import java.time.Duration;

public class CustomWaitExercise {
    WebDriver driver;
    WebDriverWait wait;

    @BeforeMethod
    public void setup() {
        WebDriverManager.chromedriver().setup();
        driver = new ChromeDriver();
        driver.manage().window().maximize();
        wait = new WebDriverWait(driver, Duration.ofSeconds(10));
    }

    @Test
    public void testCustomWaitForTextChange() {
        driver.get("https://the-internet.herokuapp.com/dynamic_loading/1");

        // Click start button
        driver.findElement(By.cssSelector("#start button")).click();

        // Custom wait for text to appear
        wait.until(driver -> {
            WebElement finishElement = driver.findElement(By.id("finish"));
            return finishElement.isDisplayed() &&
                   finishElement.getText().equals("Hello World!");
        });

        // Verify text
        String text = driver.findElement(By.id("finish")).getText();
        System.out.println("Text displayed: " + text);
        assert text.equals("Hello World!");
    }

    @Test
    public void testCustomWaitForElementCount() {
        driver.get("https://the-internet.herokuapp.com/add_remove_elements/");

        // Add 5 buttons
        for(int i = 0; i < 5; i++) {
            driver.findElement(By.xpath("//button[text()='Add Element']")).click();
        }

        // Wait until exactly 5 delete buttons are present
        wait.until(driver -> {
            var elements = driver.findElements(By.className("added-manually"));
            return elements.size() == 5;
        });

        int buttonCount = driver.findElements(By.className("added-manually")).size();
        System.out.println("Number of buttons: " + buttonCount);
        assert buttonCount == 5;
    }

    @AfterMethod
    public void tearDown() {
        if(driver != null) {
            driver.quit();
        }
    }
}
```

**Expected Output:**
- Text "Hello World!" appears after loading
- Exactly 5 delete buttons created
- No timeout exceptions

**Learning Points:**
- Lambda expressions for custom wait conditions
- Checking multiple conditions in wait
- Verifying element count dynamically

---

## Exercise 2: Fluent Wait with Retry Logic

**Objective:** Implement Fluent Wait to handle intermittent failures.

**Test Website:** https://the-internet.herokuapp.com/dynamic_loading/2

```java
import org.openqa.selenium.support.ui.FluentWait;
import org.openqa.selenium.support.ui.Wait;
import java.util.NoSuchElementException;

public class FluentWaitExercise {
    WebDriver driver;

    @BeforeMethod
    public void setup() {
        WebDriverManager.chromedriver().setup();
        driver = new ChromeDriver();
        driver.manage().window().maximize();
    }

    @Test
    public void testFluentWaitWithPolling() {
        driver.get("https://the-internet.herokuapp.com/dynamic_loading/2");

        // Click start button
        driver.findElement(By.cssSelector("#start button")).click();

        // Configure Fluent Wait
        Wait<WebDriver> fluentWait = new FluentWait<>(driver)
            .withTimeout(Duration.ofSeconds(15))
            .pollingEvery(Duration.ofMillis(500))
            .ignoring(NoSuchElementException.class)
            .ignoring(StaleElementReferenceException.class)
            .withMessage("Element not found within 15 seconds");

        // Wait for finish element
        WebElement finishElement = fluentWait.until(driver -> {
            return driver.findElement(By.id("finish"));
        });

        System.out.println("Text: " + finishElement.getText());
        assert finishElement.isDisplayed();
    }

    @Test
    public void testRetryFindElement() {
        driver.get("https://the-internet.herokuapp.com/dynamic_content");

        // Element that might become stale
        WebElement element = retryingFindElement(
            By.xpath("//div[@id='content']//div[1]/div[2]"),
            3
        );

        System.out.println("Element text: " + element.getText());
        assert element != null;
    }

    // Utility method for retry logic
    private WebElement retryingFindElement(By by, int maxRetries) {
        WebElement element = null;
        int attempts = 0;

        while(attempts < maxRetries) {
            try {
                element = driver.findElement(by);
                return element;
            } catch(StaleElementReferenceException e) {
                System.out.println("Stale element, retrying... Attempt " + (attempts + 1));
                attempts++;
                if(attempts >= maxRetries) {
                    throw e;
                }
            }
        }
        return element;
    }

    @AfterMethod
    public void tearDown() {
        if(driver != null) {
            driver.quit();
        }
    }
}
```

**Expected Output:**
- Successfully handles dynamic content loading
- Retries on stale element exceptions
- Polls every 500ms until element found

**Learning Points:**
- Fluent Wait configuration
- Ignoring specific exceptions
- Implementing retry logic for flaky elements

---

## Exercise 3: Mouse Hover and Dropdown Navigation

**Objective:** Navigate through multi-level dropdown menus.

**Test Website:** https://the-internet.herokuapp.com/hovers

```java
import org.openqa.selenium.interactions.Actions;

public class MouseHoverExercise {
    WebDriver driver;
    Actions actions;

    @BeforeMethod
    public void setup() {
        WebDriverManager.chromedriver().setup();
        driver = new ChromeDriver();
        driver.manage().window().maximize();
        actions = new Actions(driver);
    }

    @Test
    public void testHoverAndVerifyInfo() {
        driver.get("https://the-internet.herokuapp.com/hovers");

        // Hover over first image
        WebElement firstImage = driver.findElement(By.xpath("(//div[@class='figure'])[1]/img"));
        actions.moveToElement(firstImage).perform();

        // Wait for caption to appear
        WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(5));
        WebElement caption = wait.until(ExpectedConditions.visibilityOfElementLocated(
            By.xpath("(//div[@class='figcaption'])[1]")
        ));

        System.out.println("Caption text: " + caption.getText());
        assert caption.isDisplayed();

        // Click on "View profile" link
        WebElement profileLink = caption.findElement(By.tagName("a"));
        profileLink.click();

        System.out.println("Current URL: " + driver.getCurrentUrl());
        assert driver.getCurrentUrl().contains("/users/1");
    }

    @Test
    public void testHoverOnAllImages() {
        driver.get("https://the-internet.herokuapp.com/hovers");

        var images = driver.findElements(By.className("figure"));
        System.out.println("Total images: " + images.size());

        for(int i = 0; i < images.size(); i++) {
            WebElement image = images.get(i).findElement(By.tagName("img"));

            // Hover over image
            actions.moveToElement(image).perform();

            // Verify caption appears
            WebElement caption = images.get(i).findElement(By.className("figcaption"));
            assert caption.isDisplayed();

            String captionText = caption.getText();
            System.out.println("Image " + (i + 1) + ": " + captionText);

            // Small pause between hovers
            try {
                Thread.sleep(500);
            } catch(InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    @AfterMethod
    public void tearDown() {
        if(driver != null) {
            driver.quit();
        }
    }
}
```

**Expected Output:**
- Caption appears on hover
- Profile link is clickable
- Successfully navigates to user profile page

**Learning Points:**
- Using Actions class for mouse movements
- Hovering to reveal hidden elements
- Combining hover with click actions

---

## Exercise 4: Drag and Drop

**Objective:** Perform drag and drop operations.

**Test Website:** https://the-internet.herokuapp.com/drag_and_drop

```java
public class DragDropExercise {
    WebDriver driver;
    Actions actions;

    @BeforeMethod
    public void setup() {
        WebDriverManager.chromedriver().setup();
        driver = new ChromeDriver();
        driver.manage().window().maximize();
        actions = new Actions(driver);
    }

    @Test
    public void testDragAndDrop() {
        driver.get("https://the-internet.herokuapp.com/drag_and_drop");

        // Get initial text
        WebElement columnA = driver.findElement(By.id("column-a"));
        WebElement columnB = driver.findElement(By.id("column-b"));

        String initialTextA = columnA.getText();
        String initialTextB = columnB.getText();

        System.out.println("Before drag - Column A: " + initialTextA + ", Column B: " + initialTextB);

        // Perform drag and drop
        actions.dragAndDrop(columnA, columnB).perform();

        // Wait a moment for the action to complete
        try {
            Thread.sleep(1000);
        } catch(InterruptedException e) {
            e.printStackTrace();
        }

        // Get text after drag and drop
        String afterTextA = columnA.getText();
        String afterTextB = columnB.getText();

        System.out.println("After drag - Column A: " + afterTextA + ", Column B: " + afterTextB);

        // Verify swap occurred
        assert afterTextA.equals(initialTextB);
        assert afterTextB.equals(initialTextA);
    }

    @Test
    public void testDragAndDropWithClickAndHold() {
        driver.get("https://the-internet.herokuapp.com/drag_and_drop");

        WebElement source = driver.findElement(By.id("column-a"));
        WebElement target = driver.findElement(By.id("column-b"));

        // Alternative method: clickAndHold -> moveToElement -> release
        actions.clickAndHold(source)
               .moveToElement(target)
               .release()
               .perform();

        try {
            Thread.sleep(1000);
        } catch(InterruptedException e) {
            e.printStackTrace();
        }

        String textA = source.getText();
        System.out.println("Column A after alternative drag: " + textA);
    }

    @Test
    public void testDragByOffset() {
        driver.get("https://the-internet.herokuapp.com/drag_and_drop");

        WebElement source = driver.findElement(By.id("column-a"));

        // Drag by pixel offset
        actions.clickAndHold(source)
               .moveByOffset(200, 0)  // Move 200 pixels right
               .release()
               .perform();

        try {
            Thread.sleep(1000);
        } catch(InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("Drag by offset completed");
    }

    @AfterMethod
    public void tearDown() {
        if(driver != null) {
            driver.quit();
        }
    }
}
```

**Expected Output:**
- Columns A and B swap positions
- Text changes from A to B and B to A
- Alternative drag methods work

**Learning Points:**
- dragAndDrop() method
- clickAndHold() and release() combination
- Moving elements by pixel offset

---

## Exercise 5: Right Click and Double Click

**Objective:** Perform right-click and double-click actions.

**Test Website:** https://the-internet.herokuapp.com/ (custom HTML needed)

```java
public class ClickActionsExercise {
    WebDriver driver;
    Actions actions;

    @BeforeMethod
    public void setup() {
        WebDriverManager.chromedriver().setup();
        driver = new ChromeDriver();
        driver.manage().window().maximize();
        actions = new Actions(driver);
    }

    @Test
    public void testRightClick() {
        driver.get("https://swisnl.github.io/jQuery-contextMenu/demo.html");

        WebElement rightClickButton = driver.findElement(
            By.xpath("//span[contains(text(),'right click me')]")
        );

        // Right click
        actions.contextClick(rightClickButton).perform();

        // Wait for context menu
        WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(5));
        WebElement editOption = wait.until(ExpectedConditions.visibilityOfElementLocated(
            By.xpath("//span[text()='Edit']")
        ));

        assert editOption.isDisplayed();
        System.out.println("Context menu appeared successfully");

        // Click on Edit option
        editOption.click();

        // Handle alert
        try {
            Thread.sleep(500);
            Alert alert = driver.switchTo().alert();
            String alertText = alert.getText();
            System.out.println("Alert text: " + alertText);
            alert.accept();
        } catch(Exception e) {
            System.out.println("No alert found");
        }
    }

    @Test
    public void testDoubleClick() {
        driver.get("https://testautomationpractice.blogspot.com/");

        // Scroll to double-click button
        WebElement doubleClickBtn = driver.findElement(By.xpath("//button[text()='Copy Text']"));
        ((JavascriptExecutor) driver).executeScript("arguments[0].scrollIntoView(true);", doubleClickBtn);

        try {
            Thread.sleep(500);
        } catch(InterruptedException e) {
            e.printStackTrace();
        }

        // Double click
        actions.doubleClick(doubleClickBtn).perform();

        try {
            Thread.sleep(1000);
        } catch(InterruptedException e) {
            e.printStackTrace();
        }

        // Verify text was copied
        WebElement field2 = driver.findElement(By.id("field2"));
        String copiedText = field2.getAttribute("value");
        System.out.println("Copied text: " + copiedText);

        assert !copiedText.isEmpty();
    }

    @AfterMethod
    public void tearDown() {
        if(driver != null) {
            driver.quit();
        }
    }
}
```

**Expected Output:**
- Context menu appears on right-click
- Menu options are clickable
- Double-click triggers copy action

**Learning Points:**
- contextClick() for right-click
- doubleClick() method
- Handling context menus

---

## Exercise 6: Keyboard Actions

**Objective:** Perform keyboard operations like copy-paste, shortcuts.

```java
public class KeyboardActionsExercise {
    WebDriver driver;
    Actions actions;

    @BeforeMethod
    public void setup() {
        WebDriverManager.chromedriver().setup();
        driver = new ChromeDriver();
        driver.manage().window().maximize();
        actions = new Actions(driver);
    }

    @Test
    public void testCopyPaste() {
        driver.get("https://the-internet.herokuapp.com/inputs");

        WebElement inputField = driver.findElement(By.tagName("input"));

        // Type text
        inputField.sendKeys("12345");

        // Select all and copy
        actions.keyDown(Keys.CONTROL)
               .sendKeys("a")
               .keyUp(Keys.CONTROL)
               .keyDown(Keys.CONTROL)
               .sendKeys("c")
               .keyUp(Keys.CONTROL)
               .perform();

        // Clear field
        inputField.clear();

        // Paste
        actions.keyDown(Keys.CONTROL)
               .sendKeys("v")
               .keyUp(Keys.CONTROL)
               .perform();

        String pastedValue = inputField.getAttribute("value");
        System.out.println("Pasted value: " + pastedValue);
        assert pastedValue.equals("12345");
    }

    @Test
    public void testUppercaseInput() {
        driver.get("https://the-internet.herokuapp.com/inputs");

        WebElement inputField = driver.findElement(By.tagName("input"));
        inputField.click();

        // Type in uppercase using SHIFT
        actions.keyDown(Keys.SHIFT)
               .sendKeys("selenium")
               .keyUp(Keys.SHIFT)
               .perform();

        String value = inputField.getAttribute("value");
        System.out.println("Input value: " + value);
        // Note: This inputs numbers in this field, for text fields it would be uppercase
    }

    @Test
    public void testTabNavigation() {
        driver.get("https://www.facebook.com");

        WebElement emailField = driver.findElement(By.id("email"));
        emailField.sendKeys("test@example.com");

        // Press TAB to move to password field
        actions.sendKeys(Keys.TAB).perform();

        // Type password in focused field
        actions.sendKeys("password123").perform();

        System.out.println("Tab navigation successful");
    }

    @Test
    public void testEnterKey() {
        driver.get("https://www.google.com");

        WebElement searchBox = driver.findElement(By.name("q"));
        searchBox.sendKeys("Selenium WebDriver");

        // Press Enter instead of clicking Search button
        actions.sendKeys(Keys.ENTER).perform();

        // Wait for results
        WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
        wait.until(ExpectedConditions.titleContains("Selenium WebDriver"));

        System.out.println("Page title: " + driver.getTitle());
        assert driver.getTitle().contains("Selenium WebDriver");
    }

    @AfterMethod
    public void tearDown() {
        if(driver != null) {
            driver.quit();
        }
    }
}
```

**Expected Output:**
- Text copied and pasted successfully
- Tab key navigates between fields
- Enter key submits search

**Learning Points:**
- keyDown() and keyUp() for modifier keys
- Copy-paste with keyboard shortcuts
- Tab and Enter key navigation

---

## Exercise 7: Handling JavaScript Alerts

**Objective:** Handle different types of alerts.

**Test Website:** https://the-internet.herokuapp.com/javascript_alerts

```java
public class AlertsExercise {
    WebDriver driver;
    WebDriverWait wait;

    @BeforeMethod
    public void setup() {
        WebDriverManager.chromedriver().setup();
        driver = new ChromeDriver();
        driver.manage().window().maximize();
        wait = new WebDriverWait(driver, Duration.ofSeconds(10));
    }

    @Test
    public void testSimpleAlert() {
        driver.get("https://the-internet.herokuapp.com/javascript_alerts");

        // Click button to trigger alert
        driver.findElement(By.xpath("//button[text()='Click for JS Alert']")).click();

        // Wait for alert
        wait.until(ExpectedConditions.alertIsPresent());

        // Switch to alert
        Alert alert = driver.switchTo().alert();

        // Get alert text
        String alertText = alert.getText();
        System.out.println("Alert text: " + alertText);
        assert alertText.equals("I am a JS Alert");

        // Accept alert
        alert.accept();

        // Verify result
        WebElement result = driver.findElement(By.id("result"));
        System.out.println("Result: " + result.getText());
        assert result.getText().equals("You successfully clicked an alert");
    }

    @Test
    public void testConfirmAlert_Accept() {
        driver.get("https://the-internet.herokuapp.com/javascript_alerts");

        driver.findElement(By.xpath("//button[text()='Click for JS Confirm']")).click();

        Alert confirmAlert = wait.until(ExpectedConditions.alertIsPresent());

        System.out.println("Confirm text: " + confirmAlert.getText());

        // Click OK
        confirmAlert.accept();

        String result = driver.findElement(By.id("result")).getText();
        System.out.println("Result: " + result);
        assert result.equals("You clicked: Ok");
    }

    @Test
    public void testConfirmAlert_Dismiss() {
        driver.get("https://the-internet.herokuapp.com/javascript_alerts");

        driver.findElement(By.xpath("//button[text()='Click for JS Confirm']")).click();

        Alert confirmAlert = wait.until(ExpectedConditions.alertIsPresent());

        // Click Cancel
        confirmAlert.dismiss();

        String result = driver.findElement(By.id("result")).getText();
        System.out.println("Result: " + result);
        assert result.equals("You clicked: Cancel");
    }

    @Test
    public void testPromptAlert() {
        driver.get("https://the-internet.herokuapp.com/javascript_alerts");

        driver.findElement(By.xpath("//button[text()='Click for JS Prompt']")).click();

        Alert promptAlert = wait.until(ExpectedConditions.alertIsPresent());

        System.out.println("Prompt text: " + promptAlert.getText());

        // Send text
        String inputText = "Selenium Automation";
        promptAlert.sendKeys(inputText);

        // Accept
        promptAlert.accept();

        String result = driver.findElement(By.id("result")).getText();
        System.out.println("Result: " + result);
        assert result.equals("You entered: " + inputText);
    }

    @AfterMethod
    public void tearDown() {
        if(driver != null) {
            driver.quit();
        }
    }
}
```

**Expected Output:**
- Simple alert accepted successfully
- Confirm alert accept/dismiss works
- Prompt alert receives text input

**Learning Points:**
- Switching to alerts with switchTo().alert()
- accept() and dismiss() methods
- sendKeys() for prompt input

---

## Exercise 8: Handling iFrames

**Objective:** Switch between frames and interact with elements inside.

**Test Website:** https://the-internet.herokuapp.com/iframe

```java
public class FramesExercise {
    WebDriver driver;
    WebDriverWait wait;

    @BeforeMethod
    public void setup() {
        WebDriverManager.chromedriver().setup();
        driver = new ChromeDriver();
        driver.manage().window().maximize();
        wait = new WebDriverWait(driver, Duration.ofSeconds(10));
    }

    @Test
    public void testSwitchToFrame() {
        driver.get("https://the-internet.herokuapp.com/iframe");

        // Switch to frame by ID
        driver.switchTo().frame("mce_0_ifr");

        // Find element inside frame
        WebElement editor = driver.findElement(By.id("tinymce"));

        // Clear and type text
        editor.clear();
        String textToType = "Hello from Selenium WebDriver!";
        editor.sendKeys(textToType);

        String actualText = editor.getText();
        System.out.println("Text in editor: " + actualText);
        assert actualText.equals(textToType);

        // Switch back to main content
        driver.switchTo().defaultContent();

        // Verify we're back in main content
        String heading = driver.findElement(By.tagName("h3")).getText();
        System.out.println("Heading: " + heading);
    }

    @Test
    public void testSwitchToFrameByElement() {
        driver.get("https://the-internet.herokuapp.com/iframe");

        // Find frame element
        WebElement frameElement = driver.findElement(By.id("mce_0_ifr"));

        // Switch using WebElement
        driver.switchTo().frame(frameElement);

        WebElement editor = driver.findElement(By.id("tinymce"));
        editor.clear();

        // Type using Actions class
        Actions actions = new Actions(driver);
        actions.click(editor)
               .sendKeys("Testing frame switching")
               .perform();

        System.out.println("Frame switching by element successful");

        driver.switchTo().defaultContent();
    }

    @Test
    public void testNestedFrames() {
        driver.get("https://the-internet.herokuapp.com/nested_frames");

        // Switch to top frame
        driver.switchTo().frame("frame-top");

        // Switch to left frame (nested inside top)
        driver.switchTo().frame("frame-left");

        // Get text from left frame
        String leftText = driver.findElement(By.tagName("body")).getText();
        System.out.println("Left frame text: " + leftText);

        // Go back to parent (top frame)
        driver.switchTo().parentFrame();

        // Switch to middle frame
        driver.switchTo().frame("frame-middle");
        String middleText = driver.findElement(By.id("content")).getText();
        System.out.println("Middle frame text: " + middleText);

        // Go back to main content
        driver.switchTo().defaultContent();

        // Switch to bottom frame
        driver.switchTo().frame("frame-bottom");
        String bottomText = driver.findElement(By.tagName("body")).getText();
        System.out.println("Bottom frame text: " + bottomText);
    }

    @Test
    public void testFrameCount() {
        driver.get("https://the-internet.herokuapp.com/nested_frames");

        // Count total frames on page
        int totalFrames = driver.findElements(By.tagName("frame")).size();
        System.out.println("Total frames on page: " + totalFrames);

        // Switch to top frame and count nested frames
        driver.switchTo().frame("frame-top");
        int nestedFrames = driver.findElements(By.tagName("frame")).size();
        System.out.println("Nested frames in top frame: " + nestedFrames);

        driver.switchTo().defaultContent();
    }

    @AfterMethod
    public void tearDown() {
        if(driver != null) {
            driver.quit();
        }
    }
}
```

**Expected Output:**
- Successfully types text in iframe editor
- Switches between nested frames
- Returns to main content correctly

**Learning Points:**
- frame() method with ID, name, or WebElement
- parentFrame() for nested frames
- defaultContent() to return to main page

---

## Exercise 9: Handling Multiple Windows

**Objective:** Switch between multiple browser windows/tabs.

**Test Website:** https://the-internet.herokuapp.com/windows

```java
import java.util.*;

public class MultipleWindowsExercise {
    WebDriver driver;
    WebDriverWait wait;

    @BeforeMethod
    public void setup() {
        WebDriverManager.chromedriver().setup();
        driver = new ChromeDriver();
        driver.manage().window().maximize();
        wait = new WebDriverWait(driver, Duration.ofSeconds(10));
    }

    @Test
    public void testSwitchToNewWindow() {
        driver.get("https://the-internet.herokuapp.com/windows");

        // Get main window handle
        String mainWindow = driver.getWindowHandle();
        System.out.println("Main window handle: " + mainWindow);

        // Click link that opens new window
        driver.findElement(By.linkText("Click Here")).click();

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

        // Verify we're in new window
        String newWindowTitle = driver.getTitle();
        System.out.println("New window title: " + newWindowTitle);
        assert newWindowTitle.equals("New Window");

        String heading = driver.findElement(By.tagName("h3")).getText();
        System.out.println("Heading in new window: " + heading);

        // Close new window
        driver.close();

        // Switch back to main window
        driver.switchTo().window(mainWindow);

        // Verify we're back
        String mainWindowTitle = driver.getTitle();
        System.out.println("Back to main window: " + mainWindowTitle);
        assert mainWindowTitle.contains("The Internet");
    }

    @Test
    public void testMultipleWindows() {
        driver.get("https://the-internet.herokuapp.com/windows");

        String mainWindow = driver.getWindowHandle();

        // Open multiple windows
        driver.findElement(By.linkText("Click Here")).click();

        try {
            Thread.sleep(1000);
        } catch(InterruptedException e) {
            e.printStackTrace();
        }

        // Get all windows
        Set<String> windows = driver.getWindowHandles();
        System.out.println("Number of windows: " + windows.size());

        // Switch to each window and print title
        for(String window : windows) {
            driver.switchTo().window(window);
            System.out.println("Window title: " + driver.getTitle());
        }

        // Close all except main
        for(String window : windows) {
            if(!window.equals(mainWindow)) {
                driver.switchTo().window(window);
                driver.close();
            }
        }

        // Switch back to main
        driver.switchTo().window(mainWindow);
        System.out.println("Final window count: " + driver.getWindowHandles().size());
    }

    @Test
    public void testOpenNewTab() {
        driver.get("https://the-internet.herokuapp.com");

        String originalWindow = driver.getWindowHandle();

        // Open new tab (Selenium 4)
        driver.switchTo().newWindow(WindowType.TAB);

        // Navigate in new tab
        driver.get("https://the-internet.herokuapp.com/login");
        System.out.println("New tab URL: " + driver.getCurrentUrl());

        // Switch back to original tab
        driver.switchTo().window(originalWindow);
        System.out.println("Original tab URL: " + driver.getCurrentUrl());
    }

    // Utility method
    public void switchToWindowByTitle(String title) {
        Set<String> windows = driver.getWindowHandles();
        for(String window : windows) {
            driver.switchTo().window(window);
            if(driver.getTitle().equals(title)) {
                System.out.println("Switched to window with title: " + title);
                return;
            }
        }
    }

    @Test
    public void testUtilityMethod() {
        driver.get("https://the-internet.herokuapp.com/windows");

        driver.findElement(By.linkText("Click Here")).click();

        // Use utility method
        switchToWindowByTitle("New Window");

        String heading = driver.findElement(By.tagName("h3")).getText();
        System.out.println("Heading: " + heading);
        assert heading.equals("New Window");
    }

    @AfterMethod
    public void tearDown() {
        if(driver != null) {
            driver.quit();
        }
    }
}
```

**Expected Output:**
- Successfully switches to new window
- Can handle multiple windows
- Closes child windows and returns to main
- Utility method works correctly

**Learning Points:**
- getWindowHandle() vs getWindowHandles()
- Switching between windows
- Opening new tabs with Selenium 4
- Creating utility methods for window handling

---

## Exercise 10: File Upload

**Objective:** Upload files using sendKeys() method.

**Test Website:** https://the-internet.herokuapp.com/upload

```java
import java.io.File;

public class FileUploadExercise {
    WebDriver driver;
    WebDriverWait wait;

    @BeforeMethod
    public void setup() {
        WebDriverManager.chromedriver().setup();
        driver = new ChromeDriver();
        driver.manage().window().maximize();
        wait = new WebDriverWait(driver, Duration.ofSeconds(10));
    }

    @Test
    public void testFileUpload() {
        driver.get("https://the-internet.herokuapp.com/upload");

        // Create a test file
        String fileName = "test-upload.txt";
        String filePath = createTestFile(fileName, "This is a test file for upload.");

        // Find upload element
        WebElement uploadElement = driver.findElement(By.id("file-upload"));

        // Send file path
        uploadElement.sendKeys(filePath);

        // Click upload button
        driver.findElement(By.id("file-submit")).click();

        // Wait for success message
        WebElement heading = wait.until(ExpectedConditions.presenceOfElementLocated(
            By.tagName("h3")
        ));

        System.out.println("Upload result: " + heading.getText());
        assert heading.getText().equals("File Uploaded!");

        // Verify uploaded file name
        WebElement uploadedFileName = driver.findElement(By.id("uploaded-files"));
        System.out.println("Uploaded file: " + uploadedFileName.getText());
        assert uploadedFileName.getText().equals(fileName);
    }

    @Test
    public void testMultipleFileUpload() {
        // Note: The herokuapp site doesn't support multiple files,
        // but this shows the technique

        driver.get("https://the-internet.herokuapp.com/upload");

        // Create test files
        String file1 = createTestFile("file1.txt", "Content 1");
        String file2 = createTestFile("file2.txt", "Content 2");

        WebElement uploadElement = driver.findElement(By.id("file-upload"));

        // For multiple files, separate with newline
        uploadElement.sendKeys(file1 + "\n" + file2);

        System.out.println("Multiple files specified for upload");
    }

    @Test
    public void testUploadDifferentFileTypes() {
        driver.get("https://the-internet.herokuapp.com/upload");

        // Test with image file
        String imagePath = createTestFile("test-image.png", "fake image content");

        WebElement uploadElement = driver.findElement(By.id("file-upload"));
        uploadElement.sendKeys(imagePath);

        driver.findElement(By.id("file-submit")).click();

        wait.until(ExpectedConditions.presenceOfElementLocated(By.tagName("h3")));
        System.out.println("Image upload completed");
    }

    // Utility method to create test file
    private String createTestFile(String fileName, String content) {
        try {
            File file = new File(System.getProperty("user.dir") + "/" + fileName);
            java.io.FileWriter writer = new java.io.FileWriter(file);
            writer.write(content);
            writer.close();
            System.out.println("Test file created: " + file.getAbsolutePath());
            return file.getAbsolutePath();
        } catch(Exception e) {
            e.printStackTrace();
            return null;
        }
    }

    @AfterMethod
    public void tearDown() {
        if(driver != null) {
            driver.quit();
        }
    }
}
```

**Expected Output:**
- File uploads successfully
- Success message appears
- Uploaded file name displayed

**Learning Points:**
- sendKeys() for file upload
- Absolute file paths required
- Creating test files programmatically

---

## Exercise 11: File Download and Verification

**Objective:** Configure download directory and verify file download.

```java
import org.apache.commons.io.FileUtils;

public class FileDownloadExercise {
    WebDriver driver;
    String downloadPath;

    @BeforeMethod
    public void setup() {
        WebDriverManager.chromedriver().setup();

        // Set download directory
        downloadPath = System.getProperty("user.dir") + "/downloads";
        File downloadDir = new File(downloadPath);
        if(!downloadDir.exists()) {
            downloadDir.mkdir();
        }

        // Configure Chrome options
        ChromeOptions options = new ChromeOptions();
        Map<String, Object> prefs = new HashMap<>();
        prefs.put("download.default_directory", downloadPath);
        prefs.put("download.prompt_for_download", false);
        prefs.put("plugins.always_open_pdf_externally", true);
        options.setExperimentalOption("prefs", prefs);

        driver = new ChromeDriver(options);
        driver.manage().window().maximize();
    }

    @Test
    public void testFileDownload() {
        driver.get("https://the-internet.herokuapp.com/download");

        // Click first download link
        WebElement downloadLink = driver.findElement(By.linkText("some-file.txt"));
        String fileName = downloadLink.getText();

        System.out.println("Downloading file: " + fileName);
        downloadLink.click();

        // Verify download
        boolean isDownloaded = isFileDownloaded(downloadPath, fileName, 30);
        assert isDownloaded : "File was not downloaded";

        System.out.println("File downloaded successfully: " + fileName);

        // Verify file size
        File downloadedFile = new File(downloadPath + "/" + fileName);
        long fileSize = downloadedFile.length();
        System.out.println("File size: " + fileSize + " bytes");
        assert fileSize > 0;
    }

    @Test
    public void testMultipleDownloads() {
        driver.get("https://the-internet.herokuapp.com/download");

        // Get all download links
        var links = driver.findElements(By.cssSelector("a[href*='download']"));
        System.out.println("Total download links: " + links.size());

        // Download first 3 files
        for(int i = 0; i < Math.min(3, links.size()); i++) {
            String fileName = links.get(i).getText();
            System.out.println("Downloading: " + fileName);
            links.get(i).click();

            // Wait a bit between downloads
            try {
                Thread.sleep(2000);
            } catch(InterruptedException e) {
                e.printStackTrace();
            }
        }

        System.out.println("Multiple downloads initiated");
    }

    // Utility method to check if file downloaded
    private boolean isFileDownloaded(String downloadPath, String fileName, int timeoutSeconds) {
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

    @AfterMethod
    public void tearDown() {
        if(driver != null) {
            driver.quit();
        }

        // Optional: Clean up downloaded files
        try {
            File downloadDir = new File(downloadPath);
            if(downloadDir.exists()) {
                FileUtils.cleanDirectory(downloadDir);
            }
        } catch(Exception e) {
            System.out.println("Could not clean download directory");
        }
    }
}
```

**Expected Output:**
- File downloads to specified directory
- Download verification works
- File size is greater than 0

**Learning Points:**
- Configuring download directory in ChromeOptions
- Verifying file download programmatically
- Handling multiple downloads

---

## Exercise 12: Taking Screenshots

**Objective:** Capture screenshots of full page and specific elements.

```java
import org.apache.commons.io.FileUtils;
import java.text.SimpleDateFormat;
import java.util.Date;

public class ScreenshotExercise {
    WebDriver driver;
    String screenshotPath;

    @BeforeMethod
    public void setup() {
        WebDriverManager.chromedriver().setup();
        driver = new ChromeDriver();
        driver.manage().window().maximize();

        // Create screenshot directory
        screenshotPath = System.getProperty("user.dir") + "/screenshots";
        File dir = new File(screenshotPath);
        if(!dir.exists()) {
            dir.mkdir();
        }
    }

    @Test
    public void testFullPageScreenshot() {
        driver.get("https://the-internet.herokuapp.com");

        // Take screenshot
        String fileName = captureScreenshot("homepage");
        System.out.println("Screenshot saved: " + fileName);

        // Verify file exists
        File screenshot = new File(screenshotPath + "/" + fileName);
        assert screenshot.exists() : "Screenshot file not created";
        assert screenshot.length() > 0 : "Screenshot file is empty";
    }

    @Test
    public void testElementScreenshot() {
        driver.get("https://the-internet.herokuapp.com");

        // Find specific element
        WebElement heading = driver.findElement(By.tagName("h1"));

        // Take element screenshot (Selenium 4)
        File source = heading.getScreenshotAs(OutputType.FILE);

        String fileName = "heading_" + getTimestamp() + ".png";
        File destination = new File(screenshotPath + "/" + fileName);

        try {
            FileUtils.copyFile(source, destination);
            System.out.println("Element screenshot saved: " + fileName);
        } catch(Exception e) {
            e.printStackTrace();
        }

        assert destination.exists();
    }

    @Test
    public void testScreenshotOnFailure() {
        driver.get("https://the-internet.herokuapp.com/login");

        driver.findElement(By.id("username")).sendKeys("wronguser");
        driver.findElement(By.id("password")).sendKeys("wrongpass");
        driver.findElement(By.cssSelector("button[type='submit']")).click();

        // Verify error message
        try {
            WebElement errorMsg = driver.findElement(By.id("flash"));
            if(errorMsg.getText().contains("invalid")) {
                // Take screenshot of failure
                String fileName = captureScreenshot("login_failure");
                System.out.println("Failure screenshot captured: " + fileName);
            }
        } catch(Exception e) {
            captureScreenshot("unexpected_error");
        }
    }

    @Test
    public void testBase64Screenshot() {
        driver.get("https://the-internet.herokuapp.com");

        // Get screenshot as Base64 string
        TakesScreenshot ts = (TakesScreenshot) driver;
        String base64Screenshot = ts.getScreenshotAs(OutputType.BASE64);

        System.out.println("Base64 screenshot length: " + base64Screenshot.length());
        assert base64Screenshot.length() > 0;

        // Can be used in HTML reports
        String htmlImage = "<img src='data:image/png;base64," + base64Screenshot + "' />";
        System.out.println("Screenshot embedded in HTML");
    }

    @Test
    public void testMultipleScreenshots() {
        String[] urls = {
            "https://the-internet.herokuapp.com",
            "https://the-internet.herokuapp.com/login",
            "https://the-internet.herokuapp.com/upload"
        };

        for(int i = 0; i < urls.length; i++) {
            driver.get(urls[i]);
            captureScreenshot("page_" + (i + 1));
        }

        System.out.println("Multiple screenshots captured");
    }

    // Utility method for screenshot
    private String captureScreenshot(String testName) {
        try {
            TakesScreenshot ts = (TakesScreenshot) driver;
            File source = ts.getScreenshotAs(OutputType.FILE);

            String fileName = testName + "_" + getTimestamp() + ".png";
            File destination = new File(screenshotPath + "/" + fileName);

            FileUtils.copyFile(source, destination);
            return fileName;
        } catch(Exception e) {
            System.out.println("Failed to capture screenshot: " + e.getMessage());
            return null;
        }
    }

    // Utility method for timestamp
    private String getTimestamp() {
        return new SimpleDateFormat("yyyyMMdd_HHmmss").format(new Date());
    }

    @AfterMethod
    public void tearDown() {
        if(driver != null) {
            driver.quit();
        }
    }
}
```

**Expected Output:**
- Screenshots saved with timestamps
- Element screenshots work
- Base64 screenshots generated
- Screenshot files exist and have content

**Learning Points:**
- TakesScreenshot interface
- Element.getScreenshotAs() in Selenium 4
- Base64 screenshot format
- Creating screenshot utility methods

---

## Exercise 13: Headless Browser Testing

**Objective:** Run tests in headless mode.

```java
public class HeadlessBrowserExercise {
    WebDriver driver;

    @Test
    public void testChromeHeadless() {
        WebDriverManager.chromedriver().setup();

        ChromeOptions options = new ChromeOptions();
        options.addArguments("--headless=new");
        options.addArguments("--window-size=1920,1080");
        options.addArguments("--disable-gpu");

        driver = new ChromeDriver(options);

        driver.get("https://www.google.com");
        System.out.println("Page title (headless): " + driver.getTitle());

        assert driver.getTitle().contains("Google");
        driver.quit();
    }

    @Test
    public void testFirefoxHeadless() {
        WebDriverManager.firefoxdriver().setup();

        FirefoxOptions options = new FirefoxOptions();
        options.addArguments("--headless");
        options.addArguments("--width=1920");
        options.addArguments("--height=1080");

        driver = new FirefoxDriver(options);

        driver.get("https://www.google.com");
        System.out.println("Page title (Firefox headless): " + driver.getTitle());

        assert driver.getTitle().contains("Google");
        driver.quit();
    }

    @Test
    public void testHeadlessPerformance() {
        long startTime = System.currentTimeMillis();

        WebDriverManager.chromedriver().setup();
        ChromeOptions options = new ChromeOptions();
        options.addArguments("--headless=new");
        options.addArguments("--disable-gpu");
        options.addArguments("--no-sandbox");

        driver = new ChromeDriver(options);
        driver.get("https://the-internet.herokuapp.com");

        long endTime = System.currentTimeMillis();
        long loadTime = endTime - startTime;

        System.out.println("Page load time (headless): " + loadTime + "ms");

        driver.quit();
    }

    @Test
    public void testHeadlessWithScreenshot() {
        WebDriverManager.chromedriver().setup();

        ChromeOptions options = new ChromeOptions();
        options.addArguments("--headless=new");
        options.addArguments("--window-size=1920,1080");

        driver = new ChromeDriver(options);
        driver.get("https://the-internet.herokuapp.com");

        // Take screenshot in headless mode
        TakesScreenshot ts = (TakesScreenshot) driver;
        File screenshot = ts.getScreenshotAs(OutputType.FILE);

        System.out.println("Screenshot captured in headless mode");
        assert screenshot.length() > 0;

        driver.quit();
    }
}
```

**Expected Output:**
- Tests run without opening browser window
- Page title retrieved successfully
- Screenshots work in headless mode
- Faster execution compared to headed mode

**Learning Points:**
- --headless flag for Chrome/Firefox
- Window size configuration for headless
- Screenshot capability in headless mode
- Performance benefits

---

## Exercise 14: JavaScript Executor Advanced

**Objective:** Use JavaScript Executor for complex operations.

```java
public class JavaScriptExecutorExercise {
    WebDriver driver;
    JavascriptExecutor js;

    @BeforeMethod
    public void setup() {
        WebDriverManager.chromedriver().setup();
        driver = new ChromeDriver();
        driver.manage().window().maximize();
        js = (JavascriptExecutor) driver;
    }

    @Test
    public void testClickUsingJS() {
        driver.get("https://the-internet.herokuapp.com/add_remove_elements/");

        WebElement addButton = driver.findElement(By.xpath("//button[text()='Add Element']"));

        // Click using JavaScript
        js.executeScript("arguments[0].click();", addButton);

        // Verify button added
        WebElement deleteButton = driver.findElement(By.className("added-manually"));
        assert deleteButton.isDisplayed();
        System.out.println("JavaScript click successful");
    }

    @Test
    public void testScrollOperations() {
        driver.get("https://the-internet.herokuapp.com");

        // Scroll to bottom
        js.executeScript("window.scrollTo(0, document.body.scrollHeight)");
        System.out.println("Scrolled to bottom");

        try {
            Thread.sleep(1000);
        } catch(InterruptedException e) {
            e.printStackTrace();
        }

        // Scroll to top
        js.executeScript("window.scrollTo(0, 0)");
        System.out.println("Scrolled to top");

        try {
            Thread.sleep(1000);
        } catch(InterruptedException e) {
            e.printStackTrace();
        }

        // Scroll to specific element
        WebElement footer = driver.findElement(By.id("page-footer"));
        js.executeScript("arguments[0].scrollIntoView(true);", footer);
        System.out.println("Scrolled to footer");
    }

    @Test
    public void testHighlightElement() {
        driver.get("https://the-internet.herokuapp.com/login");

        WebElement username = driver.findElement(By.id("username"));

        // Highlight element (useful for debugging)
        js.executeScript("arguments[0].style.border='3px solid red'", username);

        try {
            Thread.sleep(2000);
        } catch(InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("Element highlighted");

        // Remove highlight
        js.executeScript("arguments[0].style.border=''", username);
    }

    @Test
    public void testGetPageInfo() {
        driver.get("https://the-internet.herokuapp.com");

        // Get page title
        String title = (String) js.executeScript("return document.title;");
        System.out.println("Page title via JS: " + title);

        // Get URL
        String url = (String) js.executeScript("return document.URL;");
        System.out.println("URL via JS: " + url);

        // Get domain
        String domain = (String) js.executeScript("return document.domain;");
        System.out.println("Domain: " + domain);

        // Get page height
        Long height = (Long) js.executeScript("return document.body.scrollHeight;");
        System.out.println("Page height: " + height);
    }

    @Test
    public void testSendKeysUsingJS() {
        driver.get("https://the-internet.herokuapp.com/login");

        WebElement username = driver.findElement(By.id("username"));
        WebElement password = driver.findElement(By.id("password"));

        // Send keys using JavaScript
        js.executeScript("arguments[0].value='tomsmith';", username);
        js.executeScript("arguments[0].value='SuperSecretPassword!';", password);

        System.out.println("Username: " + username.getAttribute("value"));
        System.out.println("Password: " + password.getAttribute("value"));
    }

    @Test
    public void testRemoveAttribute() {
        driver.get("https://the-internet.herokuapp.com/inputs");

        WebElement input = driver.findElement(By.tagName("input"));

        // Check if readonly (if it were)
        // Remove readonly attribute
        js.executeScript("arguments[0].removeAttribute('readonly')", input);

        input.sendKeys("12345");
        System.out.println("Attribute manipulation successful");
    }

    @Test
    public void testWaitForPageLoad() {
        driver.get("https://the-internet.herokuapp.com");

        // Check if page fully loaded
        Boolean isLoaded = (Boolean) js.executeScript("return document.readyState").equals("complete");
        System.out.println("Page loaded: " + isLoaded);
        assert isLoaded;
    }

    @Test
    public void testGenerateAlert() {
        driver.get("https://the-internet.herokuapp.com");

        // Generate alert using JS
        js.executeScript("alert('This is a JavaScript alert!');");

        try {
            Thread.sleep(1000);
        } catch(InterruptedException e) {
            e.printStackTrace();
        }

        // Handle alert
        Alert alert = driver.switchTo().alert();
        System.out.println("Alert text: " + alert.getText());
        alert.accept();
    }

    @AfterMethod
    public void tearDown() {
        if(driver != null) {
            driver.quit();
        }
    }
}
```

**Expected Output:**
- JavaScript click works
- Scrolling operations successful
- Elements highlighted temporarily
- Page information retrieved
- Attributes modified

**Learning Points:**
- executeScript() for synchronous JS
- arguments[0] references WebElement
- return keyword for getting values
- Useful for handling difficult elements

---

## Exercise 15: Comprehensive E2E Test

**Objective:** Combine multiple advanced techniques in end-to-end test.

```java
public class E2EAdvancedTest {
    WebDriver driver;
    WebDriverWait wait;
    Actions actions;
    JavascriptExecutor js;
    String screenshotPath;

    @BeforeClass
    public void setup() {
        WebDriverManager.chromedriver().setup();

        ChromeOptions options = new ChromeOptions();
        options.addArguments("--start-maximized");
        options.addArguments("--disable-notifications");

        driver = new ChromeDriver(options);
        wait = new WebDriverWait(driver, Duration.ofSeconds(15));
        actions = new Actions(driver);
        js = (JavascriptExecutor) driver;

        screenshotPath = System.getProperty("user.dir") + "/screenshots";
        new File(screenshotPath).mkdir();
    }

    @Test(priority = 1)
    public void testCompleteUserJourney() {
        try {
            // Step 1: Navigate to homepage
            driver.get("https://the-internet.herokuapp.com");
            captureScreenshot("01_homepage");
            System.out.println("Step 1: Navigated to homepage");

            // Step 2: Navigate to Form Authentication
            driver.findElement(By.linkText("Form Authentication")).click();
            wait.until(ExpectedConditions.presenceOfElementLocated(By.id("username")));
            captureScreenshot("02_login_page");
            System.out.println("Step 2: Reached login page");

            // Step 3: Login with valid credentials
            driver.findElement(By.id("username")).sendKeys("tomsmith");
            driver.findElement(By.id("password")).sendKeys("SuperSecretPassword!");
            driver.findElement(By.cssSelector("button[type='submit']")).click();

            // Step 4: Verify login success
            WebElement successMessage = wait.until(
                ExpectedConditions.presenceOfElementLocated(By.id("flash"))
            );
            assert successMessage.getText().contains("You logged into a secure area!");
            captureScreenshot("03_login_success");
            System.out.println("Step 3: Login successful");

            // Step 5: Navigate back to homepage
            driver.findElement(By.linkText("Elemental Selenium")).click();

            // Switch to new window
            String mainWindow = driver.getWindowHandle();
            Set<String> allWindows = driver.getWindowHandles();
            for(String window : allWindows) {
                if(!window.equals(mainWindow)) {
                    driver.switchTo().window(window);
                    break;
                }
            }

            System.out.println("Step 4: Switched to new window");
            System.out.println("New window title: " + driver.getTitle());

            // Close new window and return
            driver.close();
            driver.switchTo().window(mainWindow);

            System.out.println("✓ Complete user journey test passed");

        } catch(Exception e) {
            captureScreenshot("error");
            System.out.println("Test failed: " + e.getMessage());
            throw e;
        }
    }

    @Test(priority = 2)
    public void testAdvancedInteractions() {
        driver.get("https://the-internet.herokuapp.com");

        // Scroll to bottom
        js.executeScript("window.scrollTo(0, document.body.scrollHeight)");

        // Find and hover over link
        WebElement link = driver.findElement(By.linkText("Hovers"));
        actions.moveToElement(link).perform();

        // Wait a moment
        wait.until(ExpectedConditions.elementToBeClickable(link));

        // Click using JS if normal click fails
        try {
            link.click();
        } catch(Exception e) {
            js.executeScript("arguments[0].click();", link);
        }

        // Verify navigation
        wait.until(ExpectedConditions.titleContains("The Internet"));
        System.out.println("✓ Advanced interactions test passed");
    }

    // Utility method
    private void captureScreenshot(String name) {
        try {
            TakesScreenshot ts = (TakesScreenshot) driver;
            File source = ts.getScreenshotAs(OutputType.FILE);
            File dest = new File(screenshotPath + "/" + name + ".png");
            FileUtils.copyFile(source, dest);
        } catch(Exception e) {
            System.out.println("Screenshot failed: " + e.getMessage());
        }
    }

    @AfterClass
    public void tearDown() {
        if(driver != null) {
            driver.quit();
        }
    }
}
```

**Expected Output:**
- Complete user journey executes successfully
- Screenshots captured at each step
- Multiple windows handled
- Advanced interactions work
- All assertions pass

**Learning Points:**
- Combining multiple Selenium techniques
- Error handling and recovery
- Screenshot on each step
- Real-world test scenario patterns

---

## Summary of Exercises

You've now completed 15 advanced Selenium exercises covering:

1. ✅ Custom Explicit Waits with Lambda
2. ✅ Fluent Wait with Retry Logic
3. ✅ Mouse Hover and Dropdown Navigation
4. ✅ Drag and Drop Operations
5. ✅ Right Click and Double Click
6. ✅ Keyboard Actions (Copy/Paste, Shortcuts)
7. ✅ Handling JavaScript Alerts (Simple, Confirm, Prompt)
8. ✅ Handling iFrames (Single and Nested)
9. ✅ Handling Multiple Windows/Tabs
10. ✅ File Upload
11. ✅ File Download and Verification
12. ✅ Taking Screenshots (Full Page, Element, Base64)
13. ✅ Headless Browser Testing
14. ✅ JavaScript Executor Advanced Usage
15. ✅ Comprehensive E2E Test

**Key Skills Acquired:**
- Advanced synchronization strategies
- Complex user interactions
- Window and frame handling
- File operations
- Screenshot capture
- Headless testing
- JavaScript execution
- Error handling and recovery
- Utility method creation

**Next Steps:**
- Practice these exercises multiple times
- Create your own variations
- Build a utility library with reusable methods
- Move on to Page Object Model design pattern
- Learn framework development

---

## Additional Practice Websites

- https://the-internet.herokuapp.com (Comprehensive test scenarios)
- https://testautomationpractice.blogspot.com (Various elements)
- https://demoqa.com (Widgets, interactions, forms)
- https://www.saucedemo.com (E-commerce practice)
- https://opensource-demo.orangehrmlive.com (Enterprise app practice)

Happy Testing! 🚀
