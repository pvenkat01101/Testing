# Phase 4.1 - Selenium WebDriver: Practical Exercises (Beginner Level)

## Setup Exercises

### Exercise 1: Setup Selenium Project with Maven

#### Objective
Create a Maven project and add Selenium dependencies.

#### Step-by-step (Beginner)
1. Create Maven project:
   ```bash
   mvn archetype:generate -DgroupId=com.selenium.practice -DartifactId=selenium-basics -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
   ```
2. Navigate to project: `cd selenium-basics`
3. Open `pom.xml`
4. Add Selenium dependency:
   ```xml
   <dependencies>
       <dependency>
           <groupId>org.seleniumhq.selenium</groupId>
           <artifactId>selenium-java</artifactId>
           <version>4.16.0</version>
       </dependency>
       <dependency>
           <groupId>io.github.bonigarcia</groupId>
           <artifactId>webdrivermanager</artifactId>
           <version>5.6.2</version>
       </dependency>
   </dependencies>
   ```
5. Save and run: `mvn clean install`
6. Wait for dependencies to download

**Deliverable**: Working Maven project with Selenium

---

### Exercise 2: First Selenium Script

#### Objective
Write and run your first Selenium automation script.

#### Step-by-step (Beginner)
1. Create package: `src/main/java/com/selenium/practice`
2. Create class: `FirstTest.java`
3. Write code:
   ```java
   package com.selenium.practice;

   import org.openqa.selenium.WebDriver;
   import org.openqa.selenium.chrome.ChromeDriver;
   import io.github.bonigarcia.wdm.WebDriverManager;

   public class FirstTest {
       public static void main(String[] args) {
           // Setup ChromeDriver
           WebDriverManager.chromedriver().setup();

           // Create driver instance
           WebDriver driver = new ChromeDriver();

           // Navigate to Google
           driver.get("https://www.google.com");

           // Print title
           System.out.println("Page Title: " + driver.getTitle());

           // Print URL
           System.out.println("Current URL: " + driver.getCurrentUrl());

           // Close browser
           driver.quit();

           System.out.println("Test completed successfully!");
       }
   }
   ```
4. Run the class
5. Observe Chrome browser opening, navigating, and closing

**Deliverable**: First successful Selenium script

---

## Locator Exercises

### Exercise 3: Locate Elements by ID

#### Objective
Practice finding elements using ID locator.

#### Test Site
Use: https://www.saucedemo.com

#### Step-by-step (Beginner)
1. Create class: `LocatorsByID.java`
2. Navigate to Sauce Demo site
3. Locate username field: `driver.findElement(By.id("user-name"))`
4. Locate password field: `driver.findElement(By.id("password"))`
5. Locate login button: `driver.findElement(By.id("login-button"))`
6. Enter credentials:
   ```java
   driver.findElement(By.id("user-name")).sendKeys("standard_user");
   driver.findElement(By.id("password")).sendKeys("secret_sauce");
   ```
7. Click login: `driver.findElement(By.id("login-button")).click();`
8. Verify login successful

**Deliverable**: Successful login using ID locators

---

### Exercise 4: Locate Elements by Name

#### Objective
Practice finding elements using Name locator.

#### Test Site
Create a simple HTML file or use: http://demo.guru99.com/test/newtours/

#### Step-by-step (Beginner)
1. Create class: `LocatorsByName.java`
2. Navigate to site
3. Find elements by name:
   ```java
   driver.findElement(By.name("userName")).sendKeys("testuser");
   driver.findElement(By.name("password")).sendKeys("testpass");
   driver.findElement(By.name("submit")).click();
   ```
4. Observe the behavior

**Deliverable**: Element interaction using Name locators

---

### Exercise 5: Locate Elements by Class Name

#### Objective
Practice finding elements using Class Name.

#### Test Site
Use: https://www.saucedemo.com

#### Step-by-step (Beginner)
1. Create class: `LocatorsByClassName.java`
2. Login to Sauce Demo
3. Find product containers:
   ```java
   List<WebElement> products = driver.findElements(By.className("inventory_item"));
   System.out.println("Number of products: " + products.size());
   ```
4. Print all product names:
   ```java
   for (WebElement product : products) {
       String name = product.findElement(By.className("inventory_item_name")).getText();
       System.out.println(name);
   }
   ```

**Deliverable**: List of all products using className

---

### Exercise 6: Locate Links by Link Text

#### Objective
Practice finding links using Link Text and Partial Link Text.

#### Test Site
Use: https://www.wikipedia.org

#### Step-by-step (Beginner)
1. Create class: `LocatorsByLinkText.java`
2. Navigate to Wikipedia
3. Find and click link by exact text:
   ```java
   driver.findElement(By.linkText("English")).click();
   ```
4. Go back: `driver.navigate().back();`
5. Find link by partial text:
   ```java
   driver.findElement(By.partialLinkText("Eng")).click();
   ```

**Deliverable**: Navigation using link text locators

---

### Exercise 7: CSS Selectors - Basic

#### Objective
Master basic CSS selector syntax.

#### Step-by-step (Beginner)
1. Create class: `CSSSelectors.java`
2. Navigate to https://www.saucedemo.com
3. Practice CSS selectors:
   ```java
   // By ID
   driver.findElement(By.cssSelector("#user-name")).sendKeys("standard_user");

   // By class
   driver.findElement(By.cssSelector(".btn_action")).click();

   // By attribute
   driver.findElement(By.cssSelector("input[name='password']")).sendKeys("secret_sauce");

   // By tag and attribute
   driver.findElement(By.cssSelector("input[type='submit']")).click();
   ```

**Deliverable**: Login using only CSS selectors

---

### Exercise 8: CSS Selectors - Advanced

#### Objective
Practice advanced CSS selector combinations.

#### Step-by-step (Beginner)
1. Create class: `CSSSelectorsAdvanced.java`
2. After login to Sauce Demo, practice:
   ```java
   // Child selector
   driver.findElement(By.cssSelector("div.inventory_list > div")).click();

   // Descendant selector
   driver.findElement(By.cssSelector("div.inventory_item button")).click();

   // Contains attribute
   driver.findElement(By.cssSelector("button[id*='add-to-cart']")).click();

   // Starts with
   driver.findElement(By.cssSelector("button[id^='add']")).click();

   // Multiple classes
   driver.findElement(By.cssSelector("button.btn_primary.btn_inventory")).click();
   ```

**Deliverable**: Complex element interactions using CSS

---

### Exercise 9: XPath - Basic

#### Objective
Learn basic XPath syntax.

#### Step-by-step (Beginner)
1. Create class: `XPathBasic.java`
2. Navigate to Sauce Demo
3. Practice XPath:
   ```java
   // Absolute XPath (avoid in real projects)
   driver.findElement(By.xpath("/html/body/div/div/div/div/form/div/input[1]"));

   // Relative XPath with attribute
   driver.findElement(By.xpath("//input[@id='user-name']")).sendKeys("standard_user");

   // XPath with multiple attributes
   driver.findElement(By.xpath("//input[@type='password' and @id='password']")).sendKeys("secret_sauce");

   // XPath with text
   driver.findElement(By.xpath("//input[@value='Login']")).click();
   ```

**Deliverable**: Login using XPath locators

---

### Exercise 10: XPath - Advanced

#### Objective
Master advanced XPath functions.

#### Step-by-step (Beginner)
1. Create class: `XPathAdvanced.java`
2. After login, practice:
   ```java
   // Contains
   driver.findElement(By.xpath("//button[contains(@id, 'add-to-cart')]")).click();

   // Starts-with
   driver.findElement(By.xpath("//button[starts-with(@id, 'remove')]")).click();

   // Text matching
   driver.findElement(By.xpath("//div[text()='Sauce Labs Backpack']")).click();

   // Contains text
   driver.findElement(By.xpath("//div[contains(text(), 'Backpack')]")).click();

   // Parent
   driver.findElement(By.xpath("//div[@class='inventory_item_name']/parent::a")).click();

   // Following-sibling
   driver.findElement(By.xpath("//div[@class='inventory_item_name']/following-sibling::div")).getText();

   // Preceding-sibling
   driver.findElement(By.xpath("//div[@class='inventory_item_price']/preceding-sibling::div")).getText();
   ```

**Deliverable**: Complex navigation using XPath axes

---

## Web Element Interaction Exercises

### Exercise 11: Text Input and Clear

#### Objective
Practice entering and clearing text.

#### Step-by-step (Beginner)
1. Create class: `TextInput.java`
2. Navigate to Sauce Demo
3. Practice:
   ```java
   WebElement username = driver.findElement(By.id("user-name"));

   // Enter text
   username.sendKeys("wrong_user");
   Thread.sleep(1000);  // Just for visualization

   // Clear text
   username.clear();
   Thread.sleep(1000);

   // Enter correct text
   username.sendKeys("standard_user");

   // Do same for password
   WebElement password = driver.findElement(By.id("password"));
   password.sendKeys("wrong_password");
   Thread.sleep(1000);
   password.clear();
   password.sendKeys("secret_sauce");

   // Login
   driver.findElement(By.id("login-button")).click();
   ```

**Deliverable**: Understanding sendKeys() and clear()

---

### Exercise 12: Checkboxes and Radio Buttons

#### Objective
Handle checkbox and radio button interactions.

#### Test Site
Use: http://demo.guru99.com/test/radio.html

#### Step-by-step (Beginner)
1. Create class: `CheckboxRadio.java`
2. Navigate to test site
3. Handle radio buttons:
   ```java
   WebElement radio1 = driver.findElement(By.id("vfb-7-1"));

   // Check if already selected
   if (!radio1.isSelected()) {
       radio1.click();
   }

   // Verify selection
   System.out.println("Radio 1 selected: " + radio1.isSelected());
   ```
4. Handle checkboxes:
   ```java
   WebElement checkbox1 = driver.findElement(By.id("vfb-6-0"));
   WebElement checkbox2 = driver.findElement(By.id("vfb-6-1"));

   // Select both
   checkbox1.click();
   checkbox2.click();

   // Verify
   System.out.println("Checkbox 1: " + checkbox1.isSelected());
   System.out.println("Checkbox 2: " + checkbox2.isSelected());

   // Unselect first
   checkbox1.click();
   System.out.println("Checkbox 1 after uncheck: " + checkbox1.isSelected());
   ```

**Deliverable**: Checkbox and radio button handling

---

### Exercise 13: Dropdowns with Select Class

#### Objective
Handle dropdown selections.

#### Test Site
Use: http://demo.guru99.com/test/newtours/register.php

#### Step-by-step (Beginner)
1. Create class: `DropdownHandling.java`
2. Navigate to registration page
3. Handle country dropdown:
   ```java
   WebElement countryDropdown = driver.findElement(By.name("country"));
   Select select = new Select(countryDropdown);

   // Select by visible text
   select.selectByVisibleText("INDIA");
   Thread.sleep(1000);

   // Select by value
   select.selectByValue("AUSTRALIA");
   Thread.sleep(1000);

   // Select by index
   select.selectByIndex(5);

   // Get selected option
   WebElement selectedOption = select.getFirstSelectedOption();
   System.out.println("Selected country: " + selectedOption.getText());

   // Get all options
   List<WebElement> allOptions = select.getOptions();
   System.out.println("Total countries: " + allOptions.size());

   // Print all options
   for (WebElement option : allOptions) {
       System.out.println(option.getText());
   }
   ```

**Deliverable**: Complete dropdown handling

---

### Exercise 14: Get Element Information

#### Objective
Retrieve various properties from elements.

#### Step-by-step (Beginner)
1. Create class: `ElementInfo.java`
2. Navigate to Sauce Demo and login
3. Get information from product:
   ```java
   WebElement product = driver.findElement(By.xpath("//div[@class='inventory_item'][1]"));

   // Get text
   String name = product.findElement(By.className("inventory_item_name")).getText();
   System.out.println("Product name: " + name);

   // Get attribute
   WebElement button = product.findElement(By.tagName("button"));
   String buttonId = button.getAttribute("id");
   System.out.println("Button ID: " + buttonId);

   // Get CSS value
   String color = button.getCssValue("background-color");
   System.out.println("Button color: " + color);

   // Get tag name
   String tag = button.getTagName();
   System.out.println("Tag name: " + tag);

   // Check visibility
   System.out.println("Is displayed: " + button.isDisplayed());

   // Check enabled
   System.out.println("Is enabled: " + button.isEnabled());
   ```

**Deliverable**: Element property extraction

---

## Wait Exercises

### Exercise 15: Implicit Wait

#### Objective
Implement implicit wait globally.

#### Step-by-step (Beginner)
1. Create class: `ImplicitWaitDemo.java`
2. Setup implicit wait:
   ```java
   WebDriverManager.chromedriver().setup();
   WebDriver driver = new ChromeDriver();

   // Set implicit wait
   driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(10));

   driver.get("https://www.saucedemo.com");

   // All findElement calls will wait up to 10 seconds
   driver.findElement(By.id("user-name")).sendKeys("standard_user");
   driver.findElement(By.id("password")).sendKeys("secret_sauce");
   driver.findElement(By.id("login-button")).click();

   // This will wait if element takes time to load
   WebElement product = driver.findElement(By.className("inventory_item"));
   System.out.println("Product found: " + product.isDisplayed());

   driver.quit();
   ```

**Deliverable**: Understanding implicit wait behavior

---

### Exercise 16: Explicit Wait

#### Objective
Use explicit wait for specific conditions.

#### Test Site
Use: http://the-internet.herokuapp.com/dynamic_loading/2

#### Step-by-step (Beginner)
1. Create class: `ExplicitWaitDemo.java`
2. Implement explicit waits:
   ```java
   WebDriverManager.chromedriver().setup();
   WebDriver driver = new ChromeDriver();
   driver.get("http://the-internet.herokuapp.com/dynamic_loading/2");

   // Create WebDriverWait
   WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));

   // Click start button
   driver.findElement(By.cssSelector("#start button")).click();

   // Wait for element to be visible
   WebElement finishText = wait.until(
       ExpectedConditions.visibilityOfElementLocated(By.id("finish"))
   );

   System.out.println("Text: " + finishText.getText());

   driver.quit();
   ```

**Deliverable**: Explicit wait with ExpectedConditions

---

### Exercise 17: Multiple Explicit Wait Conditions

#### Objective
Practice different ExpectedConditions.

#### Step-by-step (Beginner)
1. Create class: `ExplicitWaitConditions.java`
2. Test different conditions:
   ```java
   WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));

   // Wait for element to be clickable
   WebElement element = wait.until(
       ExpectedConditions.elementToBeClickable(By.id("submit"))
   );

   // Wait for element to be present (in DOM)
   wait.until(ExpectedConditions.presenceOfElementLocated(By.id("result")));

   // Wait for element to be visible
   wait.until(ExpectedConditions.visibilityOfElementLocated(By.id("message")));

   // Wait for text to be present
   wait.until(ExpectedConditions.textToBePresentInElementLocated(
       By.id("status"), "Success"
   ));

   // Wait for title
   wait.until(ExpectedConditions.titleContains("Dashboard"));

   // Wait for URL
   wait.until(ExpectedConditions.urlContains("/home"));

   // Wait for element to be invisible
   wait.until(ExpectedConditions.invisibilityOfElementLocated(By.id("loading")));
   ```

**Deliverable**: Multiple wait condition examples

---

### Exercise 18: Fluent Wait

#### Objective
Implement fluent wait with custom polling.

#### Step-by-step (Beginner)
1. Create class: `FluentWaitDemo.java`
2. Implement fluent wait:
   ```java
   Wait<WebDriver> wait = new FluentWait<>(driver)
       .withTimeout(Duration.ofSeconds(30))
       .pollingEvery(Duration.ofSeconds(2))
       .ignoring(NoSuchElementException.class)
       .withMessage("Element not found after 30 seconds");

   // Use fluent wait
   WebElement element = wait.until(driver -> {
       return driver.findElement(By.id("dynamic-element"));
   });

   // Or with lambda
   WebElement element2 = wait.until(d -> d.findElement(By.id("element")));
   ```

**Deliverable**: Fluent wait implementation

---

## Special Elements Exercises

### Exercise 19: Handling Alerts

#### Objective
Handle JavaScript alerts, confirms, and prompts.

#### Test Site
Use: http://the-internet.herokuapp.com/javascript_alerts

#### Step-by-step (Beginner)
1. Create class: `AlertHandling.java`
2. Handle simple alert:
   ```java
   driver.findElement(By.xpath("//button[text()='Click for JS Alert']")).click();

   // Switch to alert
   Alert alert = driver.switchTo().alert();

   // Get alert text
   System.out.println("Alert text: " + alert.getText());

   // Accept alert
   alert.accept();

   // Verify result
   String result = driver.findElement(By.id("result")).getText();
   System.out.println("Result: " + result);
   ```
3. Handle confirm:
   ```java
   driver.findElement(By.xpath("//button[text()='Click for JS Confirm']")).click();
   Alert confirm = driver.switchTo().alert();

   // Accept (OK)
   confirm.accept();

   // Or dismiss (Cancel)
   // confirm.dismiss();
   ```
4. Handle prompt:
   ```java
   driver.findElement(By.xpath("//button[text()='Click for JS Prompt']")).click();
   Alert prompt = driver.switchTo().alert();

   // Type text
   prompt.sendKeys("Hello Selenium");

   // Accept
   prompt.accept();
   ```

**Deliverable**: Complete alert handling

---

### Exercise 20: Handling Frames

#### Objective
Switch between frames and main content.

#### Test Site
Use: http://the-internet.herokuapp.com/iframe

#### Step-by-step (Beginner)
1. Create class: `FrameHandling.java`
2. Switch to frame and interact:
   ```java
   driver.get("http://the-internet.herokuapp.com/iframe");

   // Switch to frame by ID
   driver.switchTo().frame("mce_0_ifr");

   // Find element in frame
   WebElement editor = driver.findElement(By.id("tinymce"));

   // Clear and type
   editor.clear();
   editor.sendKeys("Hello from Selenium!");

   // Switch back to main content
   driver.switchTo().defaultContent();

   // Now interact with elements outside frame
   String heading = driver.findElement(By.tagName("h3")).getText();
   System.out.println("Heading: " + heading);
   ```

**Deliverable**: Frame switching and interaction

---

### Exercise 21: Multiple Windows/Tabs

#### Objective
Handle multiple browser windows and tabs.

#### Test Site
Use: http://the-internet.herokuapp.com/windows

#### Step-by-step (Beginner)
1. Create class: `WindowHandling.java`
2. Handle multiple windows:
   ```java
   driver.get("http://the-internet.herokuapp.com/windows");

   // Get main window handle
   String mainWindow = driver.getWindowHandle();
   System.out.println("Main window: " + mainWindow);

   // Click link to open new window
   driver.findElement(By.linkText("Click Here")).click();

   // Get all window handles
   Set<String> allWindows = driver.getWindowHandles();
   System.out.println("Total windows: " + allWindows.size());

   // Switch to new window
   for (String window : allWindows) {
       if (!window.equals(mainWindow)) {
           driver.switchTo().window(window);
           break;
       }
   }

   // Perform action in new window
   String newWindowText = driver.findElement(By.tagName("h3")).getText();
   System.out.println("New window text: " + newWindowText);

   // Close new window
   driver.close();

   // Switch back to main window
   driver.switchTo().window(mainWindow);

   // Verify we're back
   System.out.println("Back to main: " + driver.findElement(By.tagName("h3")).getText());
   ```

**Deliverable**: Multi-window navigation

---

### Exercise 22: Selenium 4 - New Tab/Window

#### Objective
Use Selenium 4 feature to open new tab/window.

#### Step-by-step (Beginner)
1. Create class: `NewTabWindow.java`
2. Open new tab:
   ```java
   driver.get("https://www.google.com");
   String originalWindow = driver.getWindowHandle();

   // Open new tab
   driver.switchTo().newWindow(WindowType.TAB);

   // Navigate in new tab
   driver.get("https://www.wikipedia.org");
   System.out.println("New tab title: " + driver.getTitle());

   // Switch back to original
   driver.switchTo().window(originalWindow);
   System.out.println("Original tab title: " + driver.getTitle());
   ```
3. Open new window:
   ```java
   // Open new window
   driver.switchTo().newWindow(WindowType.WINDOW);
   driver.get("https://www.github.com");
   ```

**Deliverable**: New tab/window handling (Selenium 4)

---

## Actions Class Exercises

### Exercise 23: Mouse Hover

#### Objective
Perform mouse hover actions.

#### Test Site
Use: http://the-internet.herokuapp.com/hovers

#### Step-by-step (Beginner)
1. Create class: `MouseHover.java`
2. Implement hover:
   ```java
   driver.get("http://the-internet.herokuapp.com/hovers");

   Actions actions = new Actions(driver);

   // Hover over first image
   WebElement image1 = driver.findElement(By.xpath("(//img[@alt='User Avatar'])[1]"));
   actions.moveToElement(image1).perform();

   // Wait to see the effect
   Thread.sleep(1000);

   // Get hidden text that appears on hover
   WebElement caption = driver.findElement(By.xpath("(//div[@class='figcaption'])[1]"));
   System.out.println("Caption: " + caption.getText());

   // Hover over second image
   WebElement image2 = driver.findElement(By.xpath("(//img[@alt='User Avatar'])[2]"));
   actions.moveToElement(image2).perform();
   Thread.sleep(1000);
   ```

**Deliverable**: Mouse hover interaction

---

### Exercise 24: Drag and Drop

#### Objective
Perform drag and drop operation.

#### Test Site
Use: http://the-internet.herokuapp.com/drag_and_drop

#### Step-by-step (Beginner)
1. Create class: `DragAndDrop.java`
2. Implement drag and drop:
   ```java
   driver.get("http://the-internet.herokuapp.com/drag_and_drop");

   Actions actions = new Actions(driver);

   WebElement source = driver.findElement(By.id("column-a"));
   WebElement target = driver.findElement(By.id("column-b"));

   System.out.println("Before drag:");
   System.out.println("Column A: " + source.getText());
   System.out.println("Column B: " + target.getText());

   // Method 1: dragAndDrop
   actions.dragAndDrop(source, target).perform();
   Thread.sleep(1000);

   System.out.println("\nAfter drag:");
   System.out.println("Column A: " + source.getText());
   System.out.println("Column B: " + target.getText());

   // Method 2: clickAndHold + moveToElement + release
   // actions.clickAndHold(source)
   //        .moveToElement(target)
   //        .release()
   //        .perform();
   ```

**Deliverable**: Drag and drop automation

---

### Exercise 25: Right Click and Double Click

#### Objective
Perform context click (right-click) and double-click.

#### Test Site
Use: http://demo.guru99.com/test/simple_context_menu.html

#### Step-by-step (Beginner)
1. Create class: `ClickActions.java`
2. Right click:
   ```java
   driver.get("http://demo.guru99.com/test/simple_context_menu.html");

   Actions actions = new Actions(driver);

   // Right click
   WebElement rightClickButton = driver.findElement(By.xpath("//span[text()='right click me']"));
   actions.contextClick(rightClickButton).perform();

   // Click an option from context menu
   driver.findElement(By.xpath("//span[text()='Edit']")).click();

   // Handle alert
   Alert alert = driver.switchTo().alert();
   System.out.println("Alert text: " + alert.getText());
   alert.accept();
   ```
3. Double click:
   ```java
   // Double click
   WebElement doubleClickButton = driver.findElement(By.xpath("//button[text()='Double-Click Me To See Alert']"));
   actions.doubleClick(doubleClickButton).perform();

   // Handle alert
   Alert alert2 = driver.switchTo().alert();
   System.out.println("Alert text: " + alert2.getText());
   alert2.accept();
   ```

**Deliverable**: Right-click and double-click actions

---

### Exercise 26: Keyboard Actions

#### Objective
Perform keyboard actions using Actions class.

#### Step-by-step (Beginner)
1. Create class: `KeyboardActions.java`
2. Implement keyboard operations:
   ```java
   driver.get("https://www.google.com");

   Actions actions = new Actions(driver);

   WebElement searchBox = driver.findElement(By.name("q"));

   // Type in search box
   actions.sendKeys(searchBox, "Selenium WebDriver").perform();

   // Press Enter
   actions.sendKeys(Keys.ENTER).perform();
   Thread.sleep(2000);

   // Go back
   driver.navigate().back();

   // Select all text (Ctrl+A / Cmd+A)
   searchBox.clear();
   searchBox.sendKeys("Test Automation");

   actions.keyDown(Keys.CONTROL).sendKeys("a").keyUp(Keys.CONTROL).perform();

   // Copy (Ctrl+C)
   actions.keyDown(Keys.CONTROL).sendKeys("c").keyUp(Keys.CONTROL).perform();

   // Clear and paste (Ctrl+V)
   searchBox.clear();
   actions.keyDown(Keys.CONTROL).sendKeys("v").keyUp(Keys.CONTROL).perform();
   ```

**Deliverable**: Keyboard automation

---

## Advanced Exercises

### Exercise 27: JavaScript Executor - Scroll

#### Objective
Use JavaScriptExecutor to scroll page.

#### Step-by-step (Beginner)
1. Create class: `JSExecutorScroll.java`
2. Implement scrolling:
   ```java
   driver.get("https://www.amazon.com");

   JavascriptExecutor js = (JavascriptExecutor) driver;

   // Scroll down by pixels
   js.executeScript("window.scrollBy(0, 500);");
   Thread.sleep(1000);

   // Scroll to bottom
   js.executeScript("window.scrollTo(0, document.body.scrollHeight);");
   Thread.sleep(1000);

   // Scroll to specific element
   WebElement footer = driver.findElement(By.id("navFooter"));
   js.executeScript("arguments[0].scrollIntoView(true);", footer);
   Thread.sleep(1000);

   // Scroll back to top
   js.executeScript("window.scrollTo(0, 0);");
   ```

**Deliverable**: Scrolling with JavaScript

---

### Exercise 28: JavaScript Executor - Click

#### Objective
Use JavaScriptExecutor when normal click doesn't work.

#### Step-by-step (Beginner)
1. Create class: `JSExecutorClick.java`
2. Implement JS click:
   ```java
   driver.get("https://www.saucedemo.com");

   JavascriptExecutor js = (JavascriptExecutor) driver;

   // Enter credentials
   driver.findElement(By.id("user-name")).sendKeys("standard_user");
   driver.findElement(By.id("password")).sendKeys("secret_sauce");

   // Click using JavaScript (alternative to normal click)
   WebElement loginButton = driver.findElement(By.id("login-button"));
   js.executeScript("arguments[0].click();", loginButton);

   Thread.sleep(2000);

   // JS click on product
   WebElement product = driver.findElement(By.xpath("(//button[contains(@id, 'add-to-cart')])[1]"));
   js.executeScript("arguments[0].click();", product);
   ```

**Deliverable**: JavaScript click automation

---

### Exercise 29: Taking Screenshots

#### Objective
Capture full page and element screenshots.

#### Step-by-step (Beginner)
1. Create class: `ScreenshotDemo.java`
2. Add Apache Commons IO dependency to pom.xml:
   ```xml
   <dependency>
       <groupId>commons-io</groupId>
       <artifactId>commons-io</artifactId>
       <version>2.11.0</version>
   </dependency>
   ```
3. Capture screenshots:
   ```java
   import org.apache.commons.io.FileUtils;
   import java.io.File;
   import java.text.SimpleDateFormat;
   import java.util.Date;

   public class ScreenshotDemo {
       public static void main(String[] args) throws Exception {
           WebDriverManager.chromedriver().setup();
           WebDriver driver = new ChromeDriver();
           driver.manage().window().maximize();

           driver.get("https://www.saucedemo.com");

           // Full page screenshot
           TakesScreenshot ts = (TakesScreenshot) driver;
           File source = ts.getScreenshotAs(OutputType.FILE);

           // Save with timestamp
           String timestamp = new SimpleDateFormat("yyyyMMdd_HHmmss").format(new Date());
           File destination = new File("screenshots/fullpage_" + timestamp + ".png");
           FileUtils.copyFile(source, destination);

           System.out.println("Screenshot saved: " + destination.getAbsolutePath());

           // Element screenshot (Selenium 4)
           WebElement logo = driver.findElement(By.className("login_logo"));
           File elementSource = logo.getScreenshotAs(OutputType.FILE);
           File elementDest = new File("screenshots/logo_" + timestamp + ".png");
           FileUtils.copyFile(elementSource, elementDest);

           driver.quit();
       }
   }
   ```

**Deliverable**: Screenshot capture functionality

---

### Exercise 30: Complete End-to-End Test

#### Objective
Combine all learned concepts in one complete test.

#### Scenario
Complete shopping flow on Sauce Demo.

#### Step-by-step (Beginner)
1. Create class: `E2ETest.java`
2. Implement complete flow:
   ```java
   public class E2ETest {
       public static void main(String[] args) throws Exception {
           WebDriverManager.chromedriver().setup();
           WebDriver driver = new ChromeDriver();
           WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
           driver.manage().window().maximize();

           try {
               // 1. Login
               driver.get("https://www.saucedemo.com");
               driver.findElement(By.id("user-name")).sendKeys("standard_user");
               driver.findElement(By.id("password")).sendKeys("secret_sauce");
               driver.findElement(By.id("login-button")).click();

               // 2. Verify login
               wait.until(ExpectedConditions.visibilityOfElementLocated(By.className("inventory_list")));
               System.out.println("Login successful!");

               // 3. Add products to cart
               driver.findElement(By.id("add-to-cart-sauce-labs-backpack")).click();
               driver.findElement(By.id("add-to-cart-sauce-labs-bike-light")).click();

               // 4. Go to cart
               driver.findElement(By.className("shopping_cart_link")).click();

               // 5. Verify cart items
               List<WebElement> cartItems = driver.findElements(By.className("cart_item"));
               System.out.println("Items in cart: " + cartItems.size());

               // 6. Proceed to checkout
               driver.findElement(By.id("checkout")).click();

               // 7. Fill checkout information
               driver.findElement(By.id("first-name")).sendKeys("John");
               driver.findElement(By.id("last-name")).sendKeys("Doe");
               driver.findElement(By.id("postal-code")).sendKeys("12345");
               driver.findElement(By.id("continue")).click();

               // 8. Verify checkout overview
               wait.until(ExpectedConditions.visibilityOfElementLocated(By.className("summary_info")));
               String total = driver.findElement(By.className("summary_total_label")).getText();
               System.out.println("Total: " + total);

               // 9. Complete order
               driver.findElement(By.id("finish")).click();

               // 10. Verify success
               wait.until(ExpectedConditions.visibilityOfElementLocated(By.className("complete-header")));
               String successMessage = driver.findElement(By.className("complete-header")).getText();
               System.out.println("Order status: " + successMessage);

               // 11. Take screenshot of success
               TakesScreenshot ts = (TakesScreenshot) driver;
               File source = ts.getScreenshotAs(OutputType.FILE);
               FileUtils.copyFile(source, new File("screenshots/order_success.png"));

               System.out.println("\n=== E2E Test Completed Successfully! ===");

           } catch (Exception e) {
               // Take screenshot on failure
               TakesScreenshot ts = (TakesScreenshot) driver;
               File source = ts.getScreenshotAs(OutputType.FILE);
               FileUtils.copyFile(source, new File("screenshots/error.png"));

               System.out.println("Test failed: " + e.getMessage());
               e.printStackTrace();

           } finally {
               // Always quit
               driver.quit();
           }
       }
   }
   ```

**Deliverable**: Complete end-to-end automated test

---

## Summary

These 30 exercises cover:
- ✓ Project setup and first script
- ✓ All 8 locator strategies
- ✓ Web element interactions
- ✓ All types of waits
- ✓ Special elements (alerts, frames, windows)
- ✓ Actions class (hover, drag-drop, clicks, keyboard)
- ✓ JavaScript Executor
- ✓ Screenshots
- ✓ Complete end-to-end test

Practice these exercises multiple times with different websites to build strong Selenium skills!

**Recommended Practice Sites**:
- https://www.saucedemo.com
- http://the-internet.herokuapp.com
- http://demo.guru99.com
- https://practice.expandtesting.com
- http://uitestingplayground.com
