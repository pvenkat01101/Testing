# Phase 4.2 - TestNG Framework: Practical Exercises

## Setup Exercises

### Exercise 1: Setup TestNG Project

#### Objective
Create a Maven project with TestNG dependency.

#### Step-by-step (Beginner)
1. Create Maven project:
   ```bash
   mvn archetype:generate -DgroupId=com.testng.practice -DartifactId=testng-basics -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
   ```
2. Navigate: `cd testng-basics`
3. Open `pom.xml`
4. Add TestNG dependency:
   ```xml
   <dependencies>
       <dependency>
           <groupId>org.testng</groupId>
           <artifactId>testng</artifactId>
           <version>7.8.0</version>
           <scope>test</scope>
       </dependency>
   </dependencies>
   ```
5. Run: `mvn clean install`

**Deliverable**: Working TestNG project

---

### Exercise 2: First TestNG Test

#### Objective
Write and run your first TestNG test.

#### Step-by-step (Beginner)
1. Create package: `src/test/java/com/testng/practice`
2. Create class: `FirstTest.java`
3. Write code:
   ```java
   package com.testng.practice;

   import org.testng.annotations.Test;
   import org.testng.Assert;

   public class FirstTest {

       @Test
       public void testAddition() {
           int result = 2 + 3;
           Assert.assertEquals(result, 5);
           System.out.println("Addition test passed!");
       }

       @Test
       public void testSubtraction() {
           int result = 10 - 5;
           Assert.assertEquals(result, 5);
           System.out.println("Subtraction test passed!");
       }

       @Test
       public void testMultiplication() {
           int result = 4 * 3;
           Assert.assertEquals(result, 12);
           System.out.println("Multiplication test passed!");
       }
   }
   ```
4. Run: Right-click class → Run as TestNG Test
5. Or run via Maven: `mvn test`
6. Check console output and test-output folder

**Deliverable**: First successful TestNG tests

---

## Annotation Exercises

### Exercise 3: @BeforeMethod and @AfterMethod

#### Objective
Understand method-level setup and teardown.

#### Step-by-step (Beginner)
1. Create class: `BeforeAfterMethodTest.java`
2. Implement:
   ```java
   package com.testng.practice;

   import org.testng.annotations.*;

   public class BeforeAfterMethodTest {

       @BeforeMethod
       public void setup() {
           System.out.println("@BeforeMethod: Setup before each test");
       }

       @Test
       public void test1() {
           System.out.println("Executing Test 1");
       }

       @Test
       public void test2() {
           System.out.println("Executing Test 2");
       }

       @Test
       public void test3() {
           System.out.println("Executing Test 3");
       }

       @AfterMethod
       public void teardown() {
           System.out.println("@AfterMethod: Cleanup after each test\n");
       }
   }
   ```
3. Run and observe console output
4. Notice setup and teardown run for each test

**Deliverable**: Understanding of @BeforeMethod and @AfterMethod

---

### Exercise 4: @BeforeClass and @AfterClass

#### Objective
Understand class-level setup and teardown.

#### Step-by-step (Beginner)
1. Create class: `BeforeAfterClassTest.java`
2. Implement:
   ```java
   package com.testng.practice;

   import org.testng.annotations.*;

   public class BeforeAfterClassTest {

       @BeforeClass
       public void setupClass() {
           System.out.println("@BeforeClass: Setup once for the class\n");
       }

       @Test
       public void test1() {
           System.out.println("Executing Test 1");
       }

       @Test
       public void test2() {
           System.out.println("Executing Test 2");
       }

       @Test
       public void test3() {
           System.out.println("Executing Test 3");
       }

       @AfterClass
       public void teardownClass() {
           System.out.println("\n@AfterClass: Cleanup once for the class");
       }
   }
   ```
3. Run and observe that setup/teardown run only once

**Deliverable**: Understanding class-level annotations

---

### Exercise 5: Complete Annotation Hierarchy

#### Objective
See all annotations in action.

#### Step-by-step (Beginner)
1. Create class: `AnnotationHierarchyTest.java`
2. Implement:
   ```java
   package com.testng.practice;

   import org.testng.annotations.*;

   public class AnnotationHierarchyTest {

       @BeforeSuite
       public void beforeSuite() {
           System.out.println("1. @BeforeSuite");
       }

       @BeforeTest
       public void beforeTest() {
           System.out.println("2. @BeforeTest");
       }

       @BeforeClass
       public void beforeClass() {
           System.out.println("3. @BeforeClass");
       }

       @BeforeMethod
       public void beforeMethod() {
           System.out.println("  4. @BeforeMethod");
       }

       @Test
       public void test1() {
           System.out.println("    5. Test 1 executing");
       }

       @Test
       public void test2() {
           System.out.println("    5. Test 2 executing");
       }

       @AfterMethod
       public void afterMethod() {
           System.out.println("  6. @AfterMethod\n");
       }

       @AfterClass
       public void afterClass() {
           System.out.println("7. @AfterClass");
       }

       @AfterTest
       public void afterTest() {
           System.out.println("8. @AfterTest");
       }

       @AfterSuite
       public void afterSuite() {
           System.out.println("9. @AfterSuite");
       }
   }
   ```
3. Run and carefully observe execution order

**Deliverable**: Complete understanding of annotation hierarchy

---

## Assertion Exercises

### Exercise 6: Hard Assertions

#### Objective
Practice different assertion methods.

#### Step-by-step (Beginner)
1. Create class: `AssertionsTest.java`
2. Implement:
   ```java
   package com.testng.practice;

   import org.testng.annotations.Test;
   import org.testng.Assert;

   public class AssertionsTest {

       @Test
       public void testEquality() {
           String actual = "Hello";
           String expected = "Hello";
           Assert.assertEquals(actual, expected, "Strings should match");
           System.out.println("Equality assertion passed");
       }

       @Test
       public void testBoolean() {
           boolean condition = 5 > 3;
           Assert.assertTrue(condition, "5 should be greater than 3");
           System.out.println("Boolean assertion passed");
       }

       @Test
       public void testNull() {
           String value = null;
           Assert.assertNull(value, "Value should be null");
           System.out.println("Null assertion passed");
       }

       @Test
       public void testNotNull() {
           String value = "Not null";
           Assert.assertNotNull(value, "Value should not be null");
           System.out.println("NotNull assertion passed");
       }

       @Test
       public void testArrays() {
           int[] actual = {1, 2, 3};
           int[] expected = {1, 2, 3};
           Assert.assertEquals(actual, expected, "Arrays should match");
           System.out.println("Array assertion passed");
       }
   }
   ```
3. Run and verify all tests pass
4. Try changing values to see failures

**Deliverable**: Understanding of assertion methods

---

### Exercise 7: Soft Assertions

#### Objective
Collect multiple assertion failures.

#### Step-by-step (Beginner)
1. Create class: `SoftAssertTest.java`
2. Implement:
   ```java
   package com.testng.practice;

   import org.testng.annotations.Test;
   import org.testng.asserts.SoftAssert;

   public class SoftAssertTest {

       @Test
       public void testMultipleConditions() {
           SoftAssert softAssert = new SoftAssert();

           // First assertion - passes
           softAssert.assertEquals(2 + 2, 4, "Addition check");
           System.out.println("First assertion executed");

           // Second assertion - fails but continues
           softAssert.assertTrue(5 > 10, "5 should be > 10");
           System.out.println("Second assertion executed");

           // Third assertion - passes
           softAssert.assertEquals("hello", "hello", "String check");
           System.out.println("Third assertion executed");

           // Fourth assertion - fails but continues
           softAssert.assertNull("not null", "Should be null");
           System.out.println("Fourth assertion executed");

           System.out.println("All assertions executed, now reporting...");

           // MUST call assertAll() to report all failures
           softAssert.assertAll();
       }
   }
   ```
3. Run and see all assertions execute
4. Test fails at end with all failure details

**Deliverable**: Understanding soft assertions

---

## TestNG XML Exercises

### Exercise 8: Basic testng.xml

#### Objective
Create and run tests using testng.xml.

#### Step-by-step (Beginner)
1. Create multiple test classes:
   - `LoginTest.java`
   - `CheckoutTest.java`
   - `LogoutTest.java`

2. Each with simple tests:
   ```java
   package com.testng.practice;

   import org.testng.annotations.Test;

   public class LoginTest {
       @Test
       public void testValidLogin() {
           System.out.println("LoginTest: Valid login test");
       }

       @Test
       public void testInvalidLogin() {
           System.out.println("LoginTest: Invalid login test");
       }
   }
   ```

3. Create `testng.xml` in project root:
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd">

   <suite name="Test Suite">
       <test name="All Tests">
           <classes>
               <class name="com.testng.practice.LoginTest"/>
               <class name="com.testng.practice.CheckoutTest"/>
               <class name="com.testng.practice.LogoutTest"/>
           </classes>
       </test>
   </suite>
   ```

4. Run: Right-click testng.xml → Run as TestNG Suite
5. Or via Maven (configure in pom.xml first)

**Deliverable**: Running tests via testng.xml

---

### Exercise 9: Multiple Test Tags

#### Objective
Organize tests into multiple test groups.

#### Step-by-step (Beginner)
1. Create `testng-multiple.xml`:
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd">

   <suite name="Suite">
       <test name="Login Tests">
           <classes>
               <class name="com.testng.practice.LoginTest"/>
           </classes>
       </test>

       <test name="Checkout Tests">
           <classes>
               <class name="com.testng.practice.CheckoutTest"/>
           </classes>
       </test>

       <test name="Logout Tests">
           <classes>
               <class name="com.testng.practice.LogoutTest"/>
           </classes>
       </test>
   </suite>
   ```
2. Run and observe separate test execution

**Deliverable**: Multiple test organization

---

### Exercise 10: Include/Exclude Specific Methods

#### Objective
Run only specific test methods.

#### Step-by-step (Beginner)
1. Create `testng-selective.xml`:
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd">

   <suite name="Selective Suite">
       <test name="Selective Tests">
           <classes>
               <class name="com.testng.practice.LoginTest">
                   <methods>
                       <include name="testValidLogin"/>
                       <!-- Excludes testInvalidLogin -->
                   </methods>
               </class>
           </classes>
       </test>
   </suite>
   ```
2. Run and verify only included method executes

**Deliverable**: Selective method execution

---

## Grouping Exercises

### Exercise 11: Test Groups

#### Objective
Create and run test groups.

#### Step-by-step (Beginner)
1. Create class: `GroupingTest.java`
2. Implement:
   ```java
   package com.testng.practice;

   import org.testng.annotations.Test;

   public class GroupingTest {

       @Test(groups = {"smoke"})
       public void test1() {
           System.out.println("Test 1 - Smoke group");
       }

       @Test(groups = {"smoke"})
       public void test2() {
           System.out.println("Test 2 - Smoke group");
       }

       @Test(groups = {"regression"})
       public void test3() {
           System.out.println("Test 3 - Regression group");
       }

       @Test(groups = {"regression"})
       public void test4() {
           System.out.println("Test 4 - Regression group");
       }

       @Test(groups = {"smoke", "regression"})
       public void test5() {
           System.out.println("Test 5 - Both groups");
       }

       @Test(groups = {"sanity"})
       public void test6() {
           System.out.println("Test 6 - Sanity group");
       }
   }
   ```

3. Create `testng-groups.xml`:
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd">

   <suite name="Grouped Suite">
       <test name="Smoke Tests">
           <groups>
               <run>
                   <include name="smoke"/>
               </run>
           </groups>
           <classes>
               <class name="com.testng.practice.GroupingTest"/>
           </classes>
       </test>
   </suite>
   ```

4. Run and verify only smoke tests execute

**Deliverable**: Test grouping implementation

---

### Exercise 12: Multiple Groups and Exclusions

#### Objective
Include multiple groups and exclude specific ones.

#### Step-by-step (Beginner)
1. Create `testng-multigroups.xml`:
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd">

   <suite name="Multi-Group Suite">
       <test name="Smoke and Regression">
           <groups>
               <run>
                   <include name="smoke"/>
                   <include name="regression"/>
                   <exclude name="sanity"/>
               </run>
           </groups>
           <classes>
               <class name="com.testng.practice.GroupingTest"/>
           </classes>
       </test>
   </suite>
   ```
2. Run and verify sanity tests are excluded

**Deliverable**: Multi-group filtering

---

## Parameterization Exercises

### Exercise 13: Parameters from testng.xml

#### Objective
Pass parameters from XML to tests.

#### Step-by-step (Beginner)
1. Create class: `ParameterTest.java`
2. Implement:
   ```java
   package com.testng.practice;

   import org.testng.annotations.Parameters;
   import org.testng.annotations.Test;

   public class ParameterTest {

       @Parameters({"username", "password"})
       @Test
       public void testLogin(String username, String password) {
           System.out.println("Username: " + username);
           System.out.println("Password: " + password);
           System.out.println("Login test executed\n");
       }
   }
   ```

3. Create `testng-parameters.xml`:
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd">

   <suite name="Parameter Suite">
       <test name="Test with User1">
           <parameter name="username" value="user1"/>
           <parameter name="password" value="pass1"/>
           <classes>
               <class name="com.testng.practice.ParameterTest"/>
           </classes>
       </test>

       <test name="Test with User2">
           <parameter name="username" value="user2"/>
           <parameter name="password" value="pass2"/>
           <classes>
               <class name="com.testng.practice.ParameterTest"/>
           </classes>
       </test>
   </suite>
   ```

4. Run and see test execute twice with different parameters

**Deliverable**: XML parameter passing

---

### Exercise 14: @DataProvider - Basic

#### Objective
Use DataProvider for data-driven testing.

#### Step-by-step (Beginner)
1. Create class: `DataProviderTest.java`
2. Implement:
   ```java
   package com.testng.practice;

   import org.testng.annotations.DataProvider;
   import org.testng.annotations.Test;

   public class DataProviderTest {

       @DataProvider(name = "loginData")
       public Object[][] getData() {
           return new Object[][] {
               {"user1", "pass1"},
               {"user2", "pass2"},
               {"user3", "pass3"},
               {"user4", "pass4"}
           };
       }

       @Test(dataProvider = "loginData")
       public void testLogin(String username, String password) {
           System.out.println("Testing with Username: " + username + ", Password: " + password);
       }
   }
   ```
3. Run and see test execute 4 times

**Deliverable**: Basic DataProvider usage

---

### Exercise 15: @DataProvider - Complex Data

#### Objective
Pass complex data through DataProvider.

#### Step-by-step (Beginner)
1. Create class: `ComplexDataProviderTest.java`
2. Implement:
   ```java
   package com.testng.practice;

   import org.testng.annotations.DataProvider;
   import org.testng.annotations.Test;
   import org.testng.Assert;

   public class ComplexDataProviderTest {

       @DataProvider(name = "calculatorData")
       public Object[][] getCalculatorData() {
           return new Object[][] {
               {2, 3, 5, "addition"},
               {10, 5, 5, "subtraction"},
               {4, 3, 12, "multiplication"},
               {10, 2, 5, "division"}
           };
       }

       @Test(dataProvider = "calculatorData")
       public void testCalculator(int num1, int num2, int expected, String operation) {
           int result = 0;

           switch(operation) {
               case "addition":
                   result = num1 + num2;
                   break;
               case "subtraction":
                   result = num1 - num2;
                   break;
               case "multiplication":
                   result = num1 * num2;
                   break;
               case "division":
                   result = num1 / num2;
                   break;
           }

           System.out.println(operation + ": " + num1 + " and " + num2 + " = " + result);
           Assert.assertEquals(result, expected);
       }
   }
   ```

**Deliverable**: Complex data-driven testing

---

### Exercise 16: DataProvider in Separate Class

#### Objective
Reuse DataProvider across multiple test classes.

#### Step-by-step (Beginner)
1. Create class: `DataProviders.java`
   ```java
   package com.testng.practice;

   import org.testng.annotations.DataProvider;

   public class DataProviders {

       @DataProvider(name = "validLogins")
       public static Object[][] getValidLogins() {
           return new Object[][] {
               {"user1", "pass1", true},
               {"user2", "pass2", true}
           };
       }

       @DataProvider(name = "invalidLogins")
       public static Object[][] getInvalidLogins() {
           return new Object[][] {
               {"invalid1", "wrongpass", false},
               {"invalid2", "wrongpass", false}
           };
       }
   }
   ```

2. Create test class:
   ```java
   package com.testng.practice;

   import org.testng.annotations.Test;

   public class LoginDataTest {

       @Test(dataProvider = "validLogins", dataProviderClass = DataProviders.class)
       public void testValidLogins(String username, String password, boolean shouldPass) {
           System.out.println("Valid login test: " + username + " / " + password);
       }

       @Test(dataProvider = "invalidLogins", dataProviderClass = DataProviders.class)
       public void testInvalidLogins(String username, String password, boolean shouldPass) {
           System.out.println("Invalid login test: " + username + " / " + password);
       }
   }
   ```

**Deliverable**: Reusable DataProvider

---

## Dependency Exercises

### Exercise 17: Method Dependencies

#### Objective
Create dependent test methods.

#### Step-by-step (Beginner)
1. Create class: `DependencyTest.java`
2. Implement:
   ```java
   package com.testng.practice;

   import org.testng.annotations.Test;

   public class DependencyTest {

       @Test
       public void createUser() {
           System.out.println("1. User created");
       }

       @Test(dependsOnMethods = {"createUser"})
       public void loginUser() {
           System.out.println("2. User logged in (depends on createUser)");
       }

       @Test(dependsOnMethods = {"loginUser"})
       public void verifyDashboard() {
           System.out.println("3. Dashboard verified (depends on loginUser)");
       }

       @Test(dependsOnMethods = {"verifyDashboard"})
       public void logoutUser() {
           System.out.println("4. User logged out (depends on verifyDashboard)");
       }
   }
   ```
3. Run and observe execution order
4. Make createUser() fail and see dependent tests skip

**Deliverable**: Method dependency chain

---

### Exercise 18: Handling Failed Dependencies

#### Objective
Use alwaysRun with dependencies.

#### Step-by-step (Beginner)
1. Create class: `FailedDependencyTest.java`
2. Implement:
   ```java
   package com.testng.practice;

   import org.testng.Assert;
   import org.testng.annotations.Test;

   public class FailedDependencyTest {

       @Test
       public void test1() {
           System.out.println("Test 1");
           Assert.fail("Intentional failure");
       }

       @Test(dependsOnMethods = {"test1"})
       public void test2() {
           System.out.println("Test 2 - will be SKIPPED");
       }

       @Test(dependsOnMethods = {"test1"}, alwaysRun = true)
       public void test3() {
           System.out.println("Test 3 - will RUN even though test1 failed");
       }
   }
   ```
3. Run and observe test2 skipped but test3 runs

**Deliverable**: alwaysRun understanding

---

## Priority Exercises

### Exercise 19: Test Priority

#### Objective
Control test execution order using priority.

#### Step-by-step (Beginner)
1. Create class: `PriorityTest.java`
2. Implement:
   ```java
   package com.testng.practice;

   import org.testng.annotations.Test;

   public class PriorityTest {

       @Test(priority = 1)
       public void highPriorityTest() {
           System.out.println("Priority 1 - Executes first");
       }

       @Test(priority = 5)
       public void lowPriorityTest() {
           System.out.println("Priority 5 - Executes last");
       }

       @Test(priority = 3)
       public void mediumPriorityTest() {
           System.out.println("Priority 3 - Executes third");
       }

       @Test(priority = 2)
       public void secondPriorityTest() {
           System.out.println("Priority 2 - Executes second");
       }

       @Test  // No priority = 0 by default
       public void defaultPriorityTest() {
           System.out.println("No priority (0) - Executes before priority 1");
       }
   }
   ```
3. Run and verify execution order

**Deliverable**: Test prioritization

---

## Selenium Integration Exercises

### Exercise 20: Selenium with TestNG - Basic

#### Objective
Integrate Selenium with TestNG annotations.

#### Step-by-step (Beginner)
1. Add Selenium dependencies to pom.xml
2. Create class: `SeleniumTestNGTest.java`
3. Implement:
   ```java
   package com.testng.practice;

   import org.openqa.selenium.WebDriver;
   import org.openqa.selenium.chrome.ChromeDriver;
   import org.testng.Assert;
   import org.testng.annotations.*;
   import io.github.bonigarcia.wdm.WebDriverManager;

   public class SeleniumTestNGTest {

       WebDriver driver;

       @BeforeClass
       public void setup() {
           WebDriverManager.chromedriver().setup();
           driver = new ChromeDriver();
           driver.manage().window().maximize();
           System.out.println("Browser launched");
       }

       @Test
       public void testGoogleTitle() {
           driver.get("https://www.google.com");
           String title = driver.getTitle();
           Assert.assertTrue(title.contains("Google"));
           System.out.println("Google title test passed");
       }

       @Test
       public void testWikipediaTitle() {
           driver.get("https://www.wikipedia.org");
           String title = driver.getTitle();
           Assert.assertTrue(title.contains("Wikipedia"));
           System.out.println("Wikipedia title test passed");
       }

       @AfterClass
       public void teardown() {
           if (driver != null) {
               driver.quit();
               System.out.println("Browser closed");
           }
       }
   }
   ```

**Deliverable**: Selenium + TestNG integration

---

### Exercise 21: Cross-Browser Testing with Parameters

#### Objective
Run tests on different browsers using parameters.

#### Step-by-step (Beginner)
1. Create class: `CrossBrowserTest.java`
2. Implement:
   ```java
   package com.testng.practice;

   import org.openqa.selenium.WebDriver;
   import org.openqa.selenium.chrome.ChromeDriver;
   import org.openqa.selenium.firefox.FirefoxDriver;
   import org.testng.Assert;
   import org.testng.annotations.*;
   import io.github.bonigarcia.wdm.WebDriverManager;

   public class CrossBrowserTest {

       WebDriver driver;

       @Parameters("browser")
       @BeforeClass
       public void setup(String browser) {
           if (browser.equalsIgnoreCase("chrome")) {
               WebDriverManager.chromedriver().setup();
               driver = new ChromeDriver();
           } else if (browser.equalsIgnoreCase("firefox")) {
               WebDriverManager.firefoxdriver().setup();
               driver = new FirefoxDriver();
           }
           driver.manage().window().maximize();
           System.out.println("Browser launched: " + browser);
       }

       @Test
       public void testHomePage() {
           driver.get("https://www.saucedemo.com");
           String url = driver.getCurrentUrl();
           Assert.assertTrue(url.contains("saucedemo"));
           System.out.println("Test passed on browser");
       }

       @AfterClass
       public void teardown() {
           if (driver != null) {
               driver.quit();
           }
       }
   }
   ```

3. Create `testng-crossbrowser.xml`:
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd">

   <suite name="Cross Browser Suite">
       <test name="Chrome Test">
           <parameter name="browser" value="chrome"/>
           <classes>
               <class name="com.testng.practice.CrossBrowserTest"/>
           </classes>
       </test>

       <test name="Firefox Test">
           <parameter name="browser" value="firefox"/>
           <classes>
               <class name="com.testng.practice.CrossBrowserTest"/>
           </classes>
       </test>
   </suite>
   ```

**Deliverable**: Cross-browser testing

---

### Exercise 22: Page Object Model with TestNG

#### Objective
Implement POM pattern with TestNG.

#### Step-by-step (Beginner)
1. Create `LoginPage.java`:
   ```java
   package com.testng.practice.pages;

   import org.openqa.selenium.By;
   import org.openqa.selenium.WebDriver;

   public class LoginPage {
       WebDriver driver;

       By usernameField = By.id("user-name");
       By passwordField = By.id("password");
       By loginButton = By.id("login-button");

       public LoginPage(WebDriver driver) {
           this.driver = driver;
       }

       public void enterUsername(String username) {
           driver.findElement(usernameField).sendKeys(username);
       }

       public void enterPassword(String password) {
           driver.findElement(passwordField).sendKeys(password);
       }

       public void clickLogin() {
           driver.findElement(loginButton).click();
       }

       public void login(String username, String password) {
           enterUsername(username);
           enterPassword(password);
           clickLogin();
       }
   }
   ```

2. Create test class:
   ```java
   package com.testng.practice;

   import org.openqa.selenium.WebDriver;
   import org.openqa.selenium.chrome.ChromeDriver;
   import org.testng.Assert;
   import org.testng.annotations.*;
   import io.github.bonigarcia.wdm.WebDriverManager;
   import com.testng.practice.pages.LoginPage;

   public class POMTest {

       WebDriver driver;
       LoginPage loginPage;

       @BeforeClass
       public void setup() {
           WebDriverManager.chromedriver().setup();
           driver = new ChromeDriver();
           driver.manage().window().maximize();
           loginPage = new LoginPage(driver);
       }

       @Test
       public void testValidLogin() {
           driver.get("https://www.saucedemo.com");
           loginPage.login("standard_user", "secret_sauce");

           // Verify login successful
           String url = driver.getCurrentUrl();
           Assert.assertTrue(url.contains("inventory"));
       }

       @AfterClass
       public void teardown() {
           if (driver != null) {
               driver.quit();
           }
       }
   }
   ```

**Deliverable**: POM with TestNG

---

## Parallel Execution Exercises

### Exercise 23: Parallel Methods

#### Objective
Run test methods in parallel.

#### Step-by-step (Beginner)
1. Create class with multiple tests
2. Create `testng-parallel-methods.xml`:
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd">

   <suite name="Parallel Methods Suite" parallel="methods" thread-count="3">
       <test name="Test">
           <classes>
               <class name="com.testng.practice.GroupingTest"/>
           </classes>
       </test>
   </suite>
   ```
3. Run and observe parallel execution

**Deliverable**: Parallel method execution

---

### Exercise 24: Parallel Classes

#### Objective
Run test classes in parallel.

#### Step-by-step (Beginner)
1. Create `testng-parallel-classes.xml`:
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd">

   <suite name="Parallel Classes Suite" parallel="classes" thread-count="2">
       <test name="Test">
           <classes>
               <class name="com.testng.practice.LoginTest"/>
               <class name="com.testng.practice.CheckoutTest"/>
           </classes>
       </test>
   </suite>
   ```
2. Run and observe classes execute in parallel

**Deliverable**: Parallel class execution

---

## Listener Exercises

### Exercise 25: ITestListener Implementation

#### Objective
Create a custom listener for test events.

#### Step-by-step (Beginner)
1. Create class: `CustomListener.java`
2. Implement:
   ```java
   package com.testng.practice.listeners;

   import org.testng.ITestContext;
   import org.testng.ITestListener;
   import org.testng.ITestResult;

   public class CustomListener implements ITestListener {

       @Override
       public void onStart(ITestContext context) {
           System.out.println("\n========================================");
           System.out.println("Test Suite Started: " + context.getName());
           System.out.println("========================================\n");
       }

       @Override
       public void onFinish(ITestContext context) {
           System.out.println("\n========================================");
           System.out.println("Test Suite Finished: " + context.getName());
           System.out.println("Tests Passed: " + context.getPassedTests().size());
           System.out.println("Tests Failed: " + context.getFailedTests().size());
           System.out.println("Tests Skipped: " + context.getSkippedTests().size());
           System.out.println("========================================\n");
       }

       @Override
       public void onTestStart(ITestResult result) {
           System.out.println(">> Test Started: " + result.getName());
       }

       @Override
       public void onTestSuccess(ITestResult result) {
           System.out.println("✓ Test Passed: " + result.getName());
       }

       @Override
       public void onTestFailure(ITestResult result) {
           System.out.println("✗ Test Failed: " + result.getName());
           System.out.println("Reason: " + result.getThrowable().getMessage());
       }

       @Override
       public void onTestSkipped(ITestResult result) {
           System.out.println("⊘ Test Skipped: " + result.getName());
       }
   }
   ```

3. Use listener in testng.xml:
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd">

   <suite name="Listener Suite">
       <listeners>
           <listener class-name="com.testng.practice.listeners.CustomListener"/>
       </listeners>

       <test name="Test">
           <classes>
               <class name="com.testng.practice.LoginTest"/>
           </classes>
       </test>
   </suite>
   ```

**Deliverable**: Custom listener implementation

---

### Exercise 26: Screenshot on Failure with Listener

#### Objective
Automatically capture screenshots on test failure.

#### Step-by-step (Beginner)
1. Create class: `ScreenshotListener.java`
2. Implement:
   ```java
   package com.testng.practice.listeners;

   import org.openqa.selenium.OutputType;
   import org.openqa.selenium.TakesScreenshot;
   import org.openqa.selenium.WebDriver;
   import org.testng.ITestContext;
   import org.testng.ITestListener;
   import org.testng.ITestResult;
   import org.apache.commons.io.FileUtils;

   import java.io.File;
   import java.text.SimpleDateFormat;
   import java.util.Date;

   public class ScreenshotListener implements ITestListener {

       @Override
       public void onTestFailure(ITestResult result) {
           System.out.println("Test Failed: " + result.getName());

           // Get WebDriver instance from test class
           Object testClass = result.getInstance();
           WebDriver driver = ((BaseTest) testClass).driver;

           // Take screenshot
           if (driver != null) {
               String timestamp = new SimpleDateFormat("yyyyMMdd_HHmmss").format(new Date());
               String fileName = "failure_" + result.getName() + "_" + timestamp + ".png";

               try {
                   TakesScreenshot ts = (TakesScreenshot) driver;
                   File source = ts.getScreenshotAs(OutputType.FILE);
                   File destination = new File("screenshots/" + fileName);
                   FileUtils.copyFile(source, destination);
                   System.out.println("Screenshot saved: " + destination.getAbsolutePath());
               } catch (Exception e) {
                   System.out.println("Failed to capture screenshot: " + e.getMessage());
               }
           }
       }
   }
   ```

**Deliverable**: Auto screenshot on failure

---

### Exercise 27: Retry Analyzer

#### Objective
Automatically retry failed tests.

#### Step-by-step (Beginner)
1. Create class: `RetryAnalyzer.java`
2. Implement:
   ```java
   package com.testng.practice.listeners;

   import org.testng.IRetryAnalyzer;
   import org.testng.ITestResult;

   public class RetryAnalyzer implements IRetryAnalyzer {

       private int retryCount = 0;
       private static final int maxRetryCount = 2;

       @Override
       public boolean retry(ITestResult result) {
           if (retryCount < maxRetryCount) {
               System.out.println("Retrying test: " + result.getName() +
                       " for " + (retryCount + 1) + " time(s)");
               retryCount++;
               return true;  // Retry
           }
           return false;  // Don't retry
       }
   }
   ```

3. Use in test:
   ```java
   @Test(retryAnalyzer = RetryAnalyzer.class)
   public void flakyTest() {
       // Flaky test logic
   }
   ```

**Deliverable**: Test retry mechanism

---

## Advanced Exercises

### Exercise 28: Complete E2E Test with TestNG

#### Objective
Build complete end-to-end test suite with all TestNG features.

#### Step-by-step (Beginner)
1. Create BaseTest class with common setup
2. Create multiple test classes
3. Create testng.xml with groups, parameters, listeners
4. Implement data-driven tests
5. Add assertions and reporting

**Deliverable**: Complete test framework

---

### Exercise 29: TestNG with Maven

#### Objective
Configure and run TestNG tests via Maven.

#### Step-by-step (Beginner)
1. Add Surefire plugin to pom.xml:
   ```xml
   <build>
       <plugins>
           <plugin>
               <groupId>org.apache.maven.plugins</groupId>
               <artifactId>maven-surefire-plugin</artifactId>
               <version>2.22.2</version>
               <configuration>
                   <suiteXmlFiles>
                       <suiteXmlFile>testng.xml</suiteXmlFile>
                   </suiteXmlFiles>
               </configuration>
           </plugin>
       </plugins>
   </build>
   ```
2. Run: `mvn clean test`
3. Check target/surefire-reports for results

**Deliverable**: Maven-TestNG integration

---

### Exercise 30: Generating TestNG Reports

#### Objective
Generate and customize TestNG reports.

#### Step-by-step (Beginner)
1. Run any test suite
2. Navigate to `test-output` folder
3. Open `index.html` in browser (main report)
4. Open `emailable-report.html` (summary report)
5. Review `testng-results.xml` (machine-readable)
6. Customize report title in testng.xml:
   ```xml
   <suite name="Custom Report Title">
       <!-- ... -->
   </suite>
   ```

**Deliverable**: Understanding of TestNG reports

---

## Summary

These 30 exercises cover:
- ✓ TestNG setup and annotations
- ✓ Assertions (hard and soft)
- ✓ testng.xml configuration
- ✓ Test grouping and organization
- ✓ Parameterization (@Parameters, @DataProvider)
- ✓ Test dependencies and priorities
- ✓ Selenium integration
- ✓ Page Object Model with TestNG
- ✓ Parallel execution
- ✓ Listeners and custom behavior
- ✓ Retry mechanism
- ✓ Maven integration
- ✓ Reporting

Practice these exercises to master TestNG framework for robust test automation!
