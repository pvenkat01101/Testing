# Phase 3-02: Java Programming Practicals for Testers

## Overview
This document contains 15 hands-on Java programming exercises designed specifically for testers and QA professionals. Each exercise builds upon previous concepts and focuses on real-world testing scenarios.

---

## Exercise 1: Setting Up Java Environment and First Program

### Objective
Set up your Java development environment and create your first Java program that prints a test execution report.

### Scenario
As a tester, you need to verify your Java environment is properly configured before writing automation scripts. Your first program will simulate a basic test report.

### Step-by-Step Instructions

1. **Verify Java Installation**
   - Open terminal/command prompt
   - Run: `java -version`
   - Run: `javac -version`
   - Both should show Java version 8 or higher

2. **Create Your First Java File**
   - Create a file named `FirstTestReport.java`
   - Add the code from the template below
   - Save the file

3. **Compile and Run**
   - Compile: `javac FirstTestReport.java`
   - Run: `java FirstTestReport`

### Code Template

```java
public class FirstTestReport {
    public static void main(String[] args) {
        // TODO: Print test execution report
        // Include: Test Suite Name, Total Tests, Passed, Failed, Execution Time
    }
}
```

### Expected Output

```
========================================
        TEST EXECUTION REPORT
========================================
Test Suite: Login Module Tests
Total Tests: 10
Passed: 8
Failed: 2
Execution Time: 45 seconds
========================================
```

### Solution Code

```java
public class FirstTestReport {
    public static void main(String[] args) {
        System.out.println("========================================");
        System.out.println("        TEST EXECUTION REPORT");
        System.out.println("========================================");
        System.out.println("Test Suite: Login Module Tests");
        System.out.println("Total Tests: 10");
        System.out.println("Passed: 8");
        System.out.println("Failed: 2");
        System.out.println("Execution Time: 45 seconds");
        System.out.println("========================================");
    }
}
```

### Deliverables
- FirstTestReport.java file
- Screenshot of successful compilation and execution
- Document any issues faced during setup

### Extension Challenges
1. Add current date and time to the report
2. Calculate and display the pass percentage
3. Add color codes using ANSI escape sequences (green for passed, red for failed)

---

## Exercise 2: Java Basics - Variables, Data Types, and Operators

### Objective
Learn to work with different data types and operators by creating a test metrics calculator.

### Scenario
You need to calculate various test metrics like pass percentage, defect density, and test execution efficiency using different data types and arithmetic operators.

### Step-by-Step Instructions

1. **Create TestMetricsCalculator.java**
2. **Declare variables for:**
   - Total test cases (int)
   - Passed test cases (int)
   - Failed test cases (int)
   - Execution time in minutes (double)
   - Test suite name (String)
   - Is automation enabled (boolean)

3. **Calculate and display:**
   - Pass percentage
   - Fail percentage
   - Average time per test
   - Test efficiency score

### Code Template

```java
public class TestMetricsCalculator {
    public static void main(String[] args) {
        // TODO: Declare variables
        int totalTests = 0;
        int passedTests = 0;
        int failedTests = 0;

        // TODO: Calculate metrics
        // TODO: Display results
    }
}
```

### Expected Output

```
========================================
        TEST METRICS CALCULATOR
========================================
Test Suite: Shopping Cart Tests
Total Tests: 50
Passed: 42
Failed: 8
Execution Time: 125.5 minutes
Automation Enabled: true
----------------------------------------
Pass Percentage: 84.0%
Fail Percentage: 16.0%
Average Time per Test: 2.51 minutes
Test Efficiency Score: 83.8
========================================
```

### Solution Code

```java
public class TestMetricsCalculator {
    public static void main(String[] args) {
        // Declare and initialize variables
        String testSuiteName = "Shopping Cart Tests";
        int totalTests = 50;
        int passedTests = 42;
        int failedTests = 8;
        double executionTime = 125.5; // in minutes
        boolean isAutomationEnabled = true;

        // Calculate metrics
        double passPercentage = (passedTests * 100.0) / totalTests;
        double failPercentage = (failedTests * 100.0) / totalTests;
        double avgTimePerTest = executionTime / totalTests;
        double efficiencyScore = (passPercentage + (100 - avgTimePerTest)) / 2;

        // Display results
        System.out.println("========================================");
        System.out.println("        TEST METRICS CALCULATOR");
        System.out.println("========================================");
        System.out.println("Test Suite: " + testSuiteName);
        System.out.println("Total Tests: " + totalTests);
        System.out.println("Passed: " + passedTests);
        System.out.println("Failed: " + failedTests);
        System.out.println("Execution Time: " + executionTime + " minutes");
        System.out.println("Automation Enabled: " + isAutomationEnabled);
        System.out.println("----------------------------------------");
        System.out.println("Pass Percentage: " + passPercentage + "%");
        System.out.println("Fail Percentage: " + failPercentage + "%");
        System.out.println("Average Time per Test: " + String.format("%.2f", avgTimePerTest) + " minutes");
        System.out.println("Test Efficiency Score: " + String.format("%.1f", efficiencyScore));
        System.out.println("========================================");
    }
}
```

### Deliverables
- TestMetricsCalculator.java file
- Test run with different test data values
- Document explaining each metric calculation

### Extension Challenges
1. Add defect density calculation (defects per 1000 lines of code)
2. Include test coverage percentage calculation
3. Calculate cost per test case (total cost / total tests)
4. Add conditional formatting based on pass percentage thresholds

---

## Exercise 3: Control Structures - If-Else and Switch

### Objective
Master conditional statements by creating a test priority classifier and test status evaluator.

### Scenario
Create a program that assigns priority levels to test cases based on their severity and likelihood, and evaluates test execution status to determine next actions.

### Step-by-Step Instructions

1. **Create TestPriorityClassifier.java**
2. **Implement priority classification using if-else:**
   - Critical & High Likelihood = P0
   - Critical & Medium Likelihood = P1
   - High & High Likelihood = P1
   - Medium & Medium Likelihood = P2
   - Low or Info = P3

3. **Implement status action using switch:**
   - PASSED = Log success
   - FAILED = Create bug report
   - BLOCKED = Notify team
   - SKIPPED = Add to rerun list

### Code Template

```java
public class TestPriorityClassifier {
    public static void main(String[] args) {
        // Test case details
        String severity = "Critical"; // Critical, High, Medium, Low, Info
        String likelihood = "High";    // High, Medium, Low
        String status = "FAILED";      // PASSED, FAILED, BLOCKED, SKIPPED

        // TODO: Classify priority using if-else

        // TODO: Determine action using switch
    }
}
```

### Expected Output

```
========================================
    TEST PRIORITY CLASSIFIER
========================================
Test Case: TC_LOGIN_001
Severity: Critical
Likelihood: High
Status: FAILED
----------------------------------------
Priority: P0
Action: Creating bug report and notifying team lead
Recommended Next Step: Fix immediately and retest
========================================
```

### Solution Code

```java
public class TestPriorityClassifier {
    public static void main(String[] args) {
        // Test case details
        String testCaseId = "TC_LOGIN_001";
        String severity = "Critical";
        String likelihood = "High";
        String status = "FAILED";
        String priority;
        String action;
        String nextStep;

        // Classify priority using if-else
        if (severity.equals("Critical") && likelihood.equals("High")) {
            priority = "P0";
        } else if (severity.equals("Critical") && likelihood.equals("Medium")) {
            priority = "P1";
        } else if (severity.equals("High") && likelihood.equals("High")) {
            priority = "P1";
        } else if (severity.equals("Medium") && likelihood.equals("Medium")) {
            priority = "P2";
        } else if (severity.equals("High") && likelihood.equals("Medium")) {
            priority = "P2";
        } else {
            priority = "P3";
        }

        // Determine action using switch
        switch (status) {
            case "PASSED":
                action = "Logging success in test management system";
                nextStep = "Move to next test case";
                break;
            case "FAILED":
                action = "Creating bug report and notifying team lead";
                nextStep = "Fix immediately and retest";
                break;
            case "BLOCKED":
                action = "Notifying development team about blocker";
                nextStep = "Wait for blocker resolution";
                break;
            case "SKIPPED":
                action = "Adding to rerun list";
                nextStep = "Schedule for next execution";
                break;
            default:
                action = "Unknown status - manual review required";
                nextStep = "Contact test lead";
                break;
        }

        // Display results
        System.out.println("========================================");
        System.out.println("    TEST PRIORITY CLASSIFIER");
        System.out.println("========================================");
        System.out.println("Test Case: " + testCaseId);
        System.out.println("Severity: " + severity);
        System.out.println("Likelihood: " + likelihood);
        System.out.println("Status: " + status);
        System.out.println("----------------------------------------");
        System.out.println("Priority: " + priority);
        System.out.println("Action: " + action);
        System.out.println("Recommended Next Step: " + nextStep);
        System.out.println("========================================");
    }
}
```

### Deliverables
- TestPriorityClassifier.java file
- Test with at least 5 different combinations
- Decision matrix document showing all priority classifications

### Extension Challenges
1. Add nested if-else for more complex priority rules
2. Implement ternary operator for simple conditions
3. Create a defect severity classifier
4. Add business impact factor to priority calculation

---

## Exercise 4: Loops - For, While, and Do-While

### Objective
Master different loop structures by creating test case generators and test data iterators.

### Scenario
Create programs that generate multiple test cases, iterate through test data sets, and perform batch test validations using different loop types.

### Step-by-Step Instructions

1. **Create TestLoopsDemo.java**
2. **Use FOR loop to:**
   - Generate 10 test case IDs
   - Create a multiplication table for test data

3. **Use WHILE loop to:**
   - Execute tests until failure occurs
   - Retry failed tests up to 3 times

4. **Use DO-WHILE loop to:**
   - Execute at least once and continue based on condition
   - Menu-driven test execution

### Code Template

```java
public class TestLoopsDemo {
    public static void main(String[] args) {
        // TODO: Generate test case IDs using for loop

        // TODO: Simulate test execution with while loop

        // TODO: Implement retry logic with do-while loop
    }
}
```

### Expected Output

```
========================================
        FOR LOOP: Test Case Generator
========================================
Generating test cases for Login Module:
TC_LOGIN_001
TC_LOGIN_002
TC_LOGIN_003
...
TC_LOGIN_010

========================================
        WHILE LOOP: Test Execution
========================================
Executing test 1... PASSED
Executing test 2... PASSED
Executing test 3... FAILED
Test execution stopped due to failure.
Tests executed: 3

========================================
        DO-WHILE LOOP: Test Retry
========================================
Attempt 1: Executing critical test... FAILED
Attempt 2: Executing critical test... FAILED
Attempt 3: Executing critical test... PASSED
Test passed on attempt 3
========================================
```

### Solution Code

```java
public class TestLoopsDemo {
    public static void main(String[] args) {

        // FOR LOOP: Generate test case IDs
        System.out.println("========================================");
        System.out.println("        FOR LOOP: Test Case Generator");
        System.out.println("========================================");
        System.out.println("Generating test cases for Login Module:");

        for (int i = 1; i <= 10; i++) {
            String testCaseId = String.format("TC_LOGIN_%03d", i);
            System.out.println(testCaseId);
        }

        // WHILE LOOP: Test Execution until failure
        System.out.println("\n========================================");
        System.out.println("        WHILE LOOP: Test Execution");
        System.out.println("========================================");

        int testNumber = 1;
        boolean testPassed = true;

        while (testPassed && testNumber <= 10) {
            System.out.print("Executing test " + testNumber + "... ");

            // Simulate test result (fail on test 3)
            if (testNumber == 3) {
                System.out.println("FAILED");
                testPassed = false;
            } else {
                System.out.println("PASSED");
            }
            testNumber++;
        }

        System.out.println("Test execution stopped due to failure.");
        System.out.println("Tests executed: " + (testNumber - 1));

        // DO-WHILE LOOP: Test Retry Logic
        System.out.println("\n========================================");
        System.out.println("        DO-WHILE LOOP: Test Retry");
        System.out.println("========================================");

        int attempt = 1;
        int maxAttempts = 3;
        boolean success = false;

        do {
            System.out.print("Attempt " + attempt + ": Executing critical test... ");

            // Simulate test success on attempt 3
            if (attempt == 3) {
                System.out.println("PASSED");
                success = true;
            } else {
                System.out.println("FAILED");
            }
            attempt++;
        } while (!success && attempt <= maxAttempts);

        if (success) {
            System.out.println("Test passed on attempt " + (attempt - 1));
        } else {
            System.out.println("Test failed after " + maxAttempts + " attempts");
        }
        System.out.println("========================================");

        // Nested loops: Test data combinations
        System.out.println("\n========================================");
        System.out.println("    NESTED LOOPS: Test Combinations");
        System.out.println("========================================");

        String[] browsers = {"Chrome", "Firefox", "Edge"};
        String[] environments = {"DEV", "QA", "STAGING"};

        int combinationCount = 0;
        for (int i = 0; i < browsers.length; i++) {
            for (int j = 0; j < environments.length; j++) {
                combinationCount++;
                System.out.println("Combination " + combinationCount + ": " +
                                   browsers[i] + " on " + environments[j]);
            }
        }
        System.out.println("Total combinations: " + combinationCount);
        System.out.println("========================================");
    }
}
```

### Deliverables
- TestLoopsDemo.java file
- Output showing all three loop types
- Document explaining when to use each loop type

### Extension Challenges
1. Create a test data pyramid generator using nested loops
2. Implement break and continue statements in test execution
3. Generate Fibonacci sequence for performance testing iterations
4. Create a pattern printer for test case matrices

---

## Exercise 5: Methods and Method Overloading

### Objective
Learn to create reusable methods and understand method overloading by building a test utility library.

### Scenario
Create a utility class with methods for common testing operations like logging, validation, and test data generation. Implement method overloading to handle different parameter combinations.

### Step-by-Step Instructions

1. **Create TestUtilities.java**
2. **Create methods for:**
   - Logging test results (overloaded for different parameters)
   - Validating test data (overloaded for different data types)
   - Generating test data (overloaded for different formats)
   - Calculating test metrics

3. **Demonstrate:**
   - Method with no parameters
   - Method with parameters
   - Method with return values
   - Overloaded methods

### Code Template

```java
public class TestUtilities {

    // TODO: Create log method with different signatures

    // TODO: Create validate method for different data types

    // TODO: Create calculatePassPercentage method

    // TODO: Create generateTestId method (overloaded)

    public static void main(String[] args) {
        // TODO: Demonstrate all methods
    }
}
```

### Expected Output

```
========================================
      TEST UTILITIES DEMONSTRATION
========================================

--- Logging Examples ---
[INFO] Test started
[ERROR] Login test failed: Invalid credentials
[PASSED] Test completed in 5 seconds

--- Validation Examples ---
Email validation: true
Age validation: true
Username validation: false

--- Test Metrics ---
Pass Percentage: 85.0%

--- Test ID Generation ---
Generated ID: TC_001
Generated ID: TC_LOGIN_001
Generated ID: TC_LOGIN_2024_001
========================================
```

### Solution Code

```java
public class TestUtilities {

    // Method 1: Log message (no parameters)
    public static void logTestStart() {
        System.out.println("[INFO] Test started");
    }

    // Method 2: Log message with level and message (method overloading)
    public static void log(String level, String message) {
        System.out.println("[" + level + "] " + message);
    }

    // Method 3: Log test result with execution time (method overloading)
    public static void log(String status, int executionTime) {
        System.out.println("[" + status + "] Test completed in " + executionTime + " seconds");
    }

    // Method 4: Calculate pass percentage (with return value)
    public static double calculatePassPercentage(int passed, int total) {
        if (total == 0) {
            return 0.0;
        }
        return (passed * 100.0) / total;
    }

    // Method 5: Validate email (overloading for different validations)
    public static boolean validate(String email) {
        return email.contains("@") && email.contains(".");
    }

    // Method 6: Validate age range (method overloading)
    public static boolean validate(int age) {
        return age >= 18 && age <= 100;
    }

    // Method 7: Validate username length (method overloading)
    public static boolean validate(String username, int minLength) {
        return username.length() >= minLength;
    }

    // Method 8: Generate test ID (simple)
    public static String generateTestId(int number) {
        return String.format("TC_%03d", number);
    }

    // Method 9: Generate test ID with module (method overloading)
    public static String generateTestId(String module, int number) {
        return String.format("TC_%s_%03d", module, number);
    }

    // Method 10: Generate test ID with module and year (method overloading)
    public static String generateTestId(String module, int year, int number) {
        return String.format("TC_%s_%d_%03d", module, year, number);
    }

    // Method 11: Display test summary (void method with multiple parameters)
    public static void displayTestSummary(String suiteName, int passed, int failed, int skipped) {
        System.out.println("\n--- Test Summary ---");
        System.out.println("Suite: " + suiteName);
        System.out.println("Passed: " + passed);
        System.out.println("Failed: " + failed);
        System.out.println("Skipped: " + skipped);
        System.out.println("Total: " + (passed + failed + skipped));
    }

    // Main method to demonstrate all utilities
    public static void main(String[] args) {
        System.out.println("========================================");
        System.out.println("      TEST UTILITIES DEMONSTRATION");
        System.out.println("========================================");

        // Demonstrate logging methods
        System.out.println("\n--- Logging Examples ---");
        logTestStart();
        log("ERROR", "Login test failed: Invalid credentials");
        log("PASSED", 5);

        // Demonstrate validation methods
        System.out.println("\n--- Validation Examples ---");
        System.out.println("Email validation: " + validate("test@example.com"));
        System.out.println("Age validation: " + validate(25));
        System.out.println("Username validation: " + validate("abc", 5));

        // Demonstrate calculation methods
        System.out.println("\n--- Test Metrics ---");
        double passPercentage = calculatePassPercentage(85, 100);
        System.out.println("Pass Percentage: " + passPercentage + "%");

        // Demonstrate test ID generation methods
        System.out.println("\n--- Test ID Generation ---");
        System.out.println("Generated ID: " + generateTestId(1));
        System.out.println("Generated ID: " + generateTestId("LOGIN", 1));
        System.out.println("Generated ID: " + generateTestId("LOGIN", 2024, 1));

        // Demonstrate test summary
        displayTestSummary("Login Module Tests", 45, 3, 2);

        System.out.println("\n========================================");
    }
}
```

### Deliverables
- TestUtilities.java file with at least 10 methods
- Documentation of each method's purpose and parameters
- Test run demonstrating all methods

### Extension Challenges
1. Add methods for date/time formatting
2. Create methods for random test data generation
3. Implement method overloading for screenshot capture
4. Add methods for report generation in different formats

---

## Exercise 6: Classes and Objects - Create a Test Data Class

### Objective
Understand object-oriented programming fundamentals by creating classes to represent test cases and test data.

### Scenario
Create a TestCase class that encapsulates all information about a test case, including its ID, name, priority, status, and execution details. Then create multiple test case objects to manage a test suite.

### Step-by-Step Instructions

1. **Create TestCase.java class with:**
   - Instance variables (testId, testName, priority, status, etc.)
   - Methods to display test case details
   - Methods to update test status

2. **Create TestSuiteManager.java to:**
   - Create multiple TestCase objects
   - Demonstrate object creation and manipulation
   - Display all test cases in a suite

### Code Template

```java
// TestCase.java
public class TestCase {
    // TODO: Declare instance variables

    // TODO: Create method to set test details

    // TODO: Create method to display test case

    // TODO: Create method to execute test
}

// TestSuiteManager.java
public class TestSuiteManager {
    public static void main(String[] args) {
        // TODO: Create multiple TestCase objects
        // TODO: Manipulate and display objects
    }
}
```

### Expected Output

```
========================================
        TEST SUITE MANAGER
========================================

Test Case Details:
-------------------
Test ID: TC_LOGIN_001
Test Name: Verify Valid Login
Module: Authentication
Priority: P0
Status: Not Executed
Expected Result: User should login successfully
Estimated Time: 5 minutes

Executing test: TC_LOGIN_001
Test Status: PASSED
Actual Execution Time: 4 minutes

Test Case Details:
-------------------
Test ID: TC_LOGIN_002
Test Name: Verify Invalid Password
Module: Authentication
Priority: P1
Status: PASSED
Expected Result: Error message displayed
Estimated Time: 3 minutes

========================================
Suite Summary:
Total Test Cases: 3
Executed: 2
Passed: 2
Failed: 0
========================================
```

### Solution Code

```java
// TestCase.java
public class TestCase {
    // Instance variables
    String testId;
    String testName;
    String module;
    String priority;
    String status;
    String expectedResult;
    int estimatedTime; // in minutes
    int actualTime;

    // Method to set test details
    public void setTestDetails(String id, String name, String mod, String pri,
                               String expected, int estTime) {
        this.testId = id;
        this.testName = name;
        this.module = mod;
        this.priority = pri;
        this.status = "Not Executed";
        this.expectedResult = expected;
        this.estimatedTime = estTime;
        this.actualTime = 0;
    }

    // Method to display test case details
    public void displayTestCase() {
        System.out.println("\nTest Case Details:");
        System.out.println("-------------------");
        System.out.println("Test ID: " + testId);
        System.out.println("Test Name: " + testName);
        System.out.println("Module: " + module);
        System.out.println("Priority: " + priority);
        System.out.println("Status: " + status);
        System.out.println("Expected Result: " + expectedResult);
        System.out.println("Estimated Time: " + estimatedTime + " minutes");
        if (actualTime > 0) {
            System.out.println("Actual Execution Time: " + actualTime + " minutes");
        }
    }

    // Method to execute test
    public void executeTest(int executionTime) {
        System.out.println("\nExecuting test: " + testId);
        this.actualTime = executionTime;
        // Simulate test execution (randomly pass/fail for demo)
        if (executionTime <= estimatedTime) {
            this.status = "PASSED";
        } else {
            this.status = "PASSED"; // For demo, making all pass
        }
        System.out.println("Test Status: " + status);
        System.out.println("Actual Execution Time: " + actualTime + " minutes");
    }

    // Method to update status manually
    public void updateStatus(String newStatus) {
        this.status = newStatus;
    }

    // Method to get status
    public String getStatus() {
        return this.status;
    }
}

// TestSuiteManager.java
public class TestSuiteManager {
    public static void main(String[] args) {
        System.out.println("========================================");
        System.out.println("        TEST SUITE MANAGER");
        System.out.println("========================================");

        // Create first test case object
        TestCase test1 = new TestCase();
        test1.setTestDetails("TC_LOGIN_001",
                            "Verify Valid Login",
                            "Authentication",
                            "P0",
                            "User should login successfully",
                            5);

        // Display test case 1
        test1.displayTestCase();

        // Execute test case 1
        test1.executeTest(4);

        // Create second test case object
        TestCase test2 = new TestCase();
        test2.setTestDetails("TC_LOGIN_002",
                            "Verify Invalid Password",
                            "Authentication",
                            "P1",
                            "Error message displayed",
                            3);

        // Execute test case 2
        test2.executeTest(3);

        // Display test case 2
        test2.displayTestCase();

        // Create third test case object
        TestCase test3 = new TestCase();
        test3.setTestDetails("TC_LOGIN_003",
                            "Verify Empty Username",
                            "Authentication",
                            "P1",
                            "Validation error shown",
                            2);

        // Display suite summary
        System.out.println("\n========================================");
        System.out.println("Suite Summary:");
        int totalTests = 3;
        int executed = 2;
        int passed = 0;
        int failed = 0;

        if (test1.getStatus().equals("PASSED")) passed++;
        if (test2.getStatus().equals("PASSED")) passed++;

        System.out.println("Total Test Cases: " + totalTests);
        System.out.println("Executed: " + executed);
        System.out.println("Passed: " + passed);
        System.out.println("Failed: " + failed);
        System.out.println("========================================");
    }
}
```

### Deliverables
- TestCase.java file
- TestSuiteManager.java file
- Documentation explaining the class structure
- Output showing object creation and manipulation

### Extension Challenges
1. Add a method to calculate test execution variance
2. Create a method to categorize tests by priority
3. Add defect tracking to each test case
4. Create a method to export test case to CSV format

---

## Exercise 7: Constructors and This Keyword

### Objective
Master constructors and the 'this' keyword by creating a robust TestCase class with multiple constructor options.

### Scenario
Enhance the TestCase class with constructors to initialize objects in different ways. Use the 'this' keyword to avoid naming conflicts and implement constructor chaining.

### Step-by-Step Instructions

1. **Create EnhancedTestCase.java with:**
   - Default constructor
   - Parameterized constructor (partial)
   - Parameterized constructor (full)
   - Constructor chaining
   - Use of 'this' keyword

2. **Demonstrate:**
   - Creating objects with different constructors
   - How 'this' resolves naming conflicts
   - Constructor overloading

### Code Template

```java
public class EnhancedTestCase {
    // Instance variables
    private String testId;
    private String testName;
    private String priority;
    private String status;

    // TODO: Create default constructor

    // TODO: Create constructor with id and name

    // TODO: Create constructor with all parameters

    // TODO: Add methods using 'this' keyword

    public static void main(String[] args) {
        // TODO: Create objects using different constructors
    }
}
```

### Expected Output

```
========================================
    CONSTRUCTOR DEMONSTRATION
========================================

--- Default Constructor ---
Test ID: NOT_ASSIGNED
Test Name: Unnamed Test
Priority: P3
Status: Not Executed

--- Partial Constructor ---
Test ID: TC_001
Test Name: Login Test
Priority: P3
Status: Not Executed

--- Full Constructor ---
Test ID: TC_002
Test Name: Registration Test
Priority: P0
Status: Ready

--- After Update ---
Test ID: TC_001
Test Name: Login Test
Priority: P1
Status: In Progress

========================================
        COPY CONSTRUCTOR DEMO
========================================
Original Test: TC_003 - Logout Test
Copied Test: TC_003 - Logout Test
========================================
```

### Solution Code

```java
public class EnhancedTestCase {
    // Instance variables
    private String testId;
    private String testName;
    private String priority;
    private String status;
    private String module;
    private int estimatedTime;

    // Default Constructor
    public EnhancedTestCase() {
        this.testId = "NOT_ASSIGNED";
        this.testName = "Unnamed Test";
        this.priority = "P3";
        this.status = "Not Executed";
        this.module = "General";
        this.estimatedTime = 0;
        System.out.println("Default constructor called");
    }

    // Constructor with testId and testName (Constructor Chaining)
    public EnhancedTestCase(String testId, String testName) {
        this(); // Call default constructor
        this.testId = testId;
        this.testName = testName;
        System.out.println("Partial constructor called");
    }

    // Constructor with testId, testName, and priority
    public EnhancedTestCase(String testId, String testName, String priority) {
        this(testId, testName); // Call partial constructor
        this.priority = priority;
        System.out.println("Three-parameter constructor called");
    }

    // Full Constructor
    public EnhancedTestCase(String testId, String testName, String priority,
                           String status, String module, int estimatedTime) {
        this.testId = testId;
        this.testName = testName;
        this.priority = priority;
        this.status = status;
        this.module = module;
        this.estimatedTime = estimatedTime;
        System.out.println("Full constructor called");
    }

    // Copy Constructor
    public EnhancedTestCase(EnhancedTestCase other) {
        this.testId = other.testId;
        this.testName = other.testName;
        this.priority = other.priority;
        this.status = other.status;
        this.module = other.module;
        this.estimatedTime = other.estimatedTime;
        System.out.println("Copy constructor called");
    }

    // Method using 'this' to set values (demonstrating parameter shadowing)
    public void updateTestDetails(String testId, String testName, String priority, String status) {
        // 'this' keyword differentiates instance variables from parameters
        this.testId = testId;
        this.testName = testName;
        this.priority = priority;
        this.status = status;
    }

    // Method that returns current object using 'this'
    public EnhancedTestCase setPriority(String priority) {
        this.priority = priority;
        return this; // Method chaining
    }

    public EnhancedTestCase setStatus(String status) {
        this.status = status;
        return this; // Method chaining
    }

    public EnhancedTestCase setModule(String module) {
        this.module = module;
        return this; // Method chaining
    }

    // Display method
    public void display() {
        System.out.println("\nTest ID: " + this.testId);
        System.out.println("Test Name: " + this.testName);
        System.out.println("Priority: " + this.priority);
        System.out.println("Status: " + this.status);
        System.out.println("Module: " + this.module);
        System.out.println("Estimated Time: " + this.estimatedTime + " minutes");
    }

    // Compact display
    public void displayCompact() {
        System.out.println(this.testId + " - " + this.testName);
    }

    // Main method for demonstration
    public static void main(String[] args) {
        System.out.println("========================================");
        System.out.println("    CONSTRUCTOR DEMONSTRATION");
        System.out.println("========================================");

        // Using default constructor
        System.out.println("\n--- Default Constructor ---");
        EnhancedTestCase test1 = new EnhancedTestCase();
        test1.display();

        // Using partial constructor
        System.out.println("\n--- Partial Constructor ---");
        EnhancedTestCase test2 = new EnhancedTestCase("TC_001", "Login Test");
        test2.display();

        // Using full constructor
        System.out.println("\n--- Full Constructor ---");
        EnhancedTestCase test3 = new EnhancedTestCase("TC_002",
                                                       "Registration Test",
                                                       "P0",
                                                       "Ready",
                                                       "User Management",
                                                       10);
        test3.display();

        // Demonstrating 'this' in update method
        System.out.println("\n--- After Update ---");
        test2.updateTestDetails("TC_001", "Login Test", "P1", "In Progress");
        test2.display();

        // Demonstrating method chaining with 'this'
        System.out.println("\n--- Method Chaining with 'this' ---");
        EnhancedTestCase test4 = new EnhancedTestCase("TC_004", "Checkout Test");
        test4.setPriority("P0").setStatus("Ready").setModule("E-commerce");
        test4.display();

        // Demonstrating copy constructor
        System.out.println("\n========================================");
        System.out.println("        COPY CONSTRUCTOR DEMO");
        System.out.println("========================================");
        EnhancedTestCase test5 = new EnhancedTestCase("TC_003",
                                                       "Logout Test",
                                                       "P2",
                                                       "Ready",
                                                       "Authentication",
                                                       3);
        System.out.print("Original Test: ");
        test5.displayCompact();

        EnhancedTestCase test6 = new EnhancedTestCase(test5);
        System.out.print("Copied Test: ");
        test6.displayCompact();

        System.out.println("========================================");
    }
}
```

### Deliverables
- EnhancedTestCase.java with multiple constructors
- Documentation explaining each constructor
- Demonstration of all constructors and 'this' keyword usage

### Extension Challenges
1. Add validation logic in constructors
2. Create a builder pattern as alternative to multiple constructors
3. Implement a static factory method
4. Add constructor that reads from configuration file

---

## Exercise 8: Inheritance - Create Page Object Hierarchy

### Objective
Master inheritance by creating a page object model hierarchy commonly used in test automation frameworks.

### Scenario
Create a base page class with common functionalities, then create specific page classes (LoginPage, HomePage, CheckoutPage) that inherit from the base class.

### Step-by-Step Instructions

1. **Create BasePage.java with:**
   - Common properties (driver, URL, page title)
   - Common methods (navigate, getTitle, waitForElement)

2. **Create child classes:**
   - LoginPage extends BasePage
   - HomePage extends BasePage
   - CheckoutPage extends BasePage

3. **Demonstrate:**
   - Method inheritance
   - Method overriding
   - Super keyword usage
   - Access modifiers (protected, public)

### Code Template

```java
// BasePage.java
public class BasePage {
    // TODO: Common properties

    // TODO: Common methods

}

// LoginPage.java
public class LoginPage extends BasePage {
    // TODO: Login-specific properties and methods

}

// TestRunner.java
public class TestRunner {
    public static void main(String[] args) {
        // TODO: Demonstrate inheritance
    }
}
```

### Expected Output

```
========================================
    INHERITANCE DEMONSTRATION
        Page Object Model
========================================

--- Base Page Functionality ---
[BasePage] Initializing page
[BasePage] Navigating to: https://example.com
[BasePage] Current page title: Example Domain
[BasePage] Waiting for page to load...
[BasePage] Page loaded successfully

--- Login Page ---
[BasePage] Initializing page
[LoginPage] Setting up Login Page
Login Page URL: https://example.com/login
[LoginPage] Entering username: testuser
[LoginPage] Entering password: ********
[LoginPage] Clicking login button
[BasePage] Navigating to: https://example.com/home
Login successful!

--- Home Page ---
[BasePage] Initializing page
[HomePage] Setting up Home Page
Home Page URL: https://example.com/home
[HomePage] Displaying welcome message
[HomePage] Loading dashboard widgets
Home page loaded successfully

--- Checkout Page ---
[BasePage] Initializing page
[CheckoutPage] Setting up Checkout Page
Checkout Page URL: https://example.com/checkout
[CheckoutPage] Validating cart items
[CheckoutPage] Calculating total: $299.99
[CheckoutPage] Processing payment
Checkout completed successfully

========================================
        Inheritance Hierarchy
========================================
LoginPage IS-A BasePage: true
HomePage IS-A BasePage: true
CheckoutPage IS-A BasePage: true
========================================
```

### Solution Code

```java
// BasePage.java
public class BasePage {
    // Protected variables accessible to child classes
    protected String pageUrl;
    protected String pageTitle;
    protected String pageName;
    protected boolean isLoaded;

    // Constructor
    public BasePage() {
        System.out.println("[BasePage] Initializing page");
        this.isLoaded = false;
    }

    public BasePage(String pageUrl, String pageTitle) {
        this();
        this.pageUrl = pageUrl;
        this.pageTitle = pageTitle;
    }

    // Common method: Navigate to page
    public void navigateTo(String url) {
        System.out.println("[BasePage] Navigating to: " + url);
        this.pageUrl = url;
        waitForPageLoad();
    }

    // Common method: Get page title
    public String getPageTitle() {
        System.out.println("[BasePage] Current page title: " + this.pageTitle);
        return this.pageTitle;
    }

    // Common method: Wait for page load
    protected void waitForPageLoad() {
        System.out.println("[BasePage] Waiting for page to load...");
        try {
            Thread.sleep(1000); // Simulate wait
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        this.isLoaded = true;
        System.out.println("[BasePage] Page loaded successfully");
    }

    // Common method: Check if element exists (simulated)
    protected boolean isElementPresent(String elementName) {
        System.out.println("[BasePage] Checking if element exists: " + elementName);
        return true;
    }

    // Common method: Click element
    protected void click(String elementName) {
        System.out.println("[BasePage] Clicking element: " + elementName);
    }

    // Common method: Enter text
    protected void enterText(String elementName, String text) {
        System.out.println("[BasePage] Entering text in " + elementName);
    }

    // Display page info
    public void displayPageInfo() {
        System.out.println("Page Name: " + this.pageName);
        System.out.println("Page URL: " + this.pageUrl);
        System.out.println("Page Title: " + this.pageTitle);
        System.out.println("Is Loaded: " + this.isLoaded);
    }
}

// LoginPage.java
public class LoginPage extends BasePage {
    // Login page specific properties
    private String usernameField;
    private String passwordField;
    private String loginButton;

    // Constructor
    public LoginPage() {
        super("https://example.com/login", "Login - Example");
        System.out.println("[LoginPage] Setting up Login Page");
        this.pageName = "Login Page";
        this.usernameField = "username";
        this.passwordField = "password";
        this.loginButton = "loginBtn";
    }

    // Login page specific method
    public void login(String username, String password) {
        System.out.println("[LoginPage] Entering username: " + username);
        enterText(usernameField, username);

        System.out.println("[LoginPage] Entering password: " + "********");
        enterText(passwordField, password);

        System.out.println("[LoginPage] Clicking login button");
        click(loginButton);

        // Navigate to home after login
        navigateTo("https://example.com/home");
        System.out.println("Login successful!");
    }

    // Overriding parent method
    @Override
    public void displayPageInfo() {
        System.out.println("Login Page URL: " + this.pageUrl);
        super.displayPageInfo(); // Call parent method
    }
}

// HomePage.java
public class HomePage extends BasePage {
    // Home page specific properties
    private String welcomeMessage;
    private String logoutButton;

    // Constructor
    public HomePage() {
        super("https://example.com/home", "Home - Example");
        System.out.println("[HomePage] Setting up Home Page");
        this.pageName = "Home Page";
        this.welcomeMessage = "Welcome to Example Application";
        this.logoutButton = "logoutBtn";
    }

    // Home page specific methods
    public void displayWelcomeMessage() {
        System.out.println("[HomePage] Displaying welcome message");
        System.out.println(this.welcomeMessage);
    }

    public void loadDashboard() {
        System.out.println("[HomePage] Loading dashboard widgets");
        waitForPageLoad();
        System.out.println("Home page loaded successfully");
    }

    public void logout() {
        System.out.println("[HomePage] Logging out");
        click(logoutButton);
        navigateTo("https://example.com/login");
    }

    // Overriding display method
    @Override
    public void displayPageInfo() {
        System.out.println("Home Page URL: " + this.pageUrl);
    }
}

// CheckoutPage.java
public class CheckoutPage extends BasePage {
    // Checkout specific properties
    private double cartTotal;
    private int itemCount;
    private String paymentButton;

    // Constructor
    public CheckoutPage() {
        super("https://example.com/checkout", "Checkout - Example");
        System.out.println("[CheckoutPage] Setting up Checkout Page");
        this.pageName = "Checkout Page";
        this.cartTotal = 0.0;
        this.itemCount = 0;
        this.paymentButton = "payBtn";
    }

    // Checkout specific methods
    public void validateCart(int items, double total) {
        System.out.println("[CheckoutPage] Validating cart items");
        this.itemCount = items;
        this.cartTotal = total;
        System.out.println("[CheckoutPage] Items: " + items);
        System.out.println("[CheckoutPage] Calculating total: $" + total);
    }

    public void processPayment(String paymentMethod) {
        System.out.println("[CheckoutPage] Processing payment");
        System.out.println("[CheckoutPage] Payment method: " + paymentMethod);
        click(paymentButton);
        waitForPageLoad();
        System.out.println("Checkout completed successfully");
    }

    // Overriding display method
    @Override
    public void displayPageInfo() {
        System.out.println("Checkout Page URL: " + this.pageUrl);
        System.out.println("Cart Total: $" + this.cartTotal);
        System.out.println("Items in Cart: " + this.itemCount);
    }
}

// TestRunner.java
public class TestRunner {
    public static void main(String[] args) {
        System.out.println("========================================");
        System.out.println("    INHERITANCE DEMONSTRATION");
        System.out.println("        Page Object Model");
        System.out.println("========================================");

        // Demonstrate base page
        System.out.println("\n--- Base Page Functionality ---");
        BasePage basePage = new BasePage("https://example.com", "Example Domain");
        basePage.navigateTo("https://example.com");
        basePage.getPageTitle();

        // Demonstrate Login Page
        System.out.println("\n--- Login Page ---");
        LoginPage loginPage = new LoginPage();
        loginPage.displayPageInfo();
        loginPage.login("testuser", "password123");

        // Demonstrate Home Page
        System.out.println("\n--- Home Page ---");
        HomePage homePage = new HomePage();
        homePage.displayPageInfo();
        homePage.displayWelcomeMessage();
        homePage.loadDashboard();

        // Demonstrate Checkout Page
        System.out.println("\n--- Checkout Page ---");
        CheckoutPage checkoutPage = new CheckoutPage();
        checkoutPage.displayPageInfo();
        checkoutPage.validateCart(3, 299.99);
        checkoutPage.processPayment("Credit Card");

        // Demonstrate polymorphism
        System.out.println("\n========================================");
        System.out.println("        Inheritance Hierarchy");
        System.out.println("========================================");
        System.out.println("LoginPage IS-A BasePage: " + (loginPage instanceof BasePage));
        System.out.println("HomePage IS-A BasePage: " + (homePage instanceof BasePage));
        System.out.println("CheckoutPage IS-A BasePage: " + (checkoutPage instanceof BasePage));
        System.out.println("========================================");
    }
}
```

### Deliverables
- BasePage.java with common functionality
- At least 3 child page classes
- TestRunner.java demonstrating inheritance
- Class diagram showing inheritance hierarchy

### Extension Challenges
1. Add a multilevel inheritance (AdminPage extends HomePage)
2. Create abstract methods in BasePage
3. Implement a mobile version of pages using inheritance
4. Add error page handling in base class

---

## Exercise 9: Polymorphism and Method Overriding

### Objective
Understand polymorphism, method overriding, and dynamic method dispatch by creating a test execution framework.

### Scenario
Create different types of test executors (Unit, Integration, E2E) that override common execution methods but can be treated uniformly through polymorphism.

### Step-by-Step Instructions

1. **Create TestExecutor base class with:**
   - Common execution methods
   - Setup and teardown methods

2. **Create child executor classes:**
   - UnitTestExecutor
   - IntegrationTestExecutor
   - E2ETestExecutor

3. **Demonstrate:**
   - Method overriding with @Override
   - Runtime polymorphism
   - Dynamic method dispatch

### Code Template

```java
// TestExecutor.java
public class TestExecutor {
    // TODO: Common properties and methods

}

// UnitTestExecutor.java
public class UnitTestExecutor extends TestExecutor {
    // TODO: Override execution methods

}

// TestFramework.java
public class TestFramework {
    public static void main(String[] args) {
        // TODO: Demonstrate polymorphism
    }
}
```

### Expected Output

```
========================================
    POLYMORPHISM DEMONSTRATION
    Test Execution Framework
========================================

--- Executing Unit Tests ---
[TestExecutor] Starting test execution
[UnitTestExecutor] Setting up unit test environment
[UnitTestExecutor] Loading test classes
[UnitTestExecutor] Executing unit tests
  Running test: testCalculation
  Running test: testValidation
  Running test: testDataProcessing
[UnitTestExecutor] Unit tests completed
[UnitTestExecutor] Tests: 3, Passed: 3, Failed: 0
[UnitTestExecutor] Execution time: 2 seconds
[UnitTestExecutor] Cleaning up unit test environment
[TestExecutor] Test execution completed

--- Executing Integration Tests ---
[TestExecutor] Starting test execution
[IntegrationTestExecutor] Setting up integration test environment
[IntegrationTestExecutor] Starting database connections
[IntegrationTestExecutor] Starting API services
[IntegrationTestExecutor] Executing integration tests
  Running test: testDatabaseIntegration
  Running test: testAPIIntegration
[IntegrationTestExecutor] Integration tests completed
[IntegrationTestExecutor] Tests: 2, Passed: 2, Failed: 0
[IntegrationTestExecutor] Execution time: 15 seconds
[IntegrationTestExecutor] Stopping services
[IntegrationTestExecutor] Cleaning up integration test environment
[TestExecutor] Test execution completed

--- Executing E2E Tests ---
[TestExecutor] Starting test execution
[E2ETestExecutor] Setting up E2E test environment
[E2ETestExecutor] Launching browser: Chrome
[E2ETestExecutor] Loading application URL
[E2ETestExecutor] Executing E2E tests
  Running test: testCompleteUserJourney
  Running test: testCheckoutFlow
[E2ETestExecutor] E2E tests completed
[E2ETestExecutor] Tests: 2, Passed: 1, Failed: 1
[E2ETestExecutor] Execution time: 45 seconds
[E2ETestExecutor] Closing browser
[E2ETestExecutor] Cleaning up E2E test environment
[TestExecutor] Test execution completed

========================================
    POLYMORPHIC TEST EXECUTION
========================================
Executing all test types polymorphically...

Executor Type: Unit Tests
[TestExecutor] Starting test execution
[UnitTestExecutor] Executing unit tests
[TestExecutor] Test execution completed

Executor Type: Integration Tests
[TestExecutor] Starting test execution
[IntegrationTestExecutor] Executing integration tests
[TestExecutor] Test execution completed

Executor Type: E2E Tests
[TestExecutor] Starting test execution
[E2ETestExecutor] Executing E2E tests
[TestExecutor] Test execution completed

========================================
Total execution time: 62 seconds
========================================
```

### Solution Code

```java
// TestExecutor.java
public class TestExecutor {
    protected String executorType;
    protected int totalTests;
    protected int passedTests;
    protected int failedTests;
    protected long startTime;
    protected long endTime;

    // Constructor
    public TestExecutor() {
        this.executorType = "Generic Test Executor";
        this.totalTests = 0;
        this.passedTests = 0;
        this.failedTests = 0;
    }

    // Method to be overridden
    public void setup() {
        System.out.println("[TestExecutor] Setting up test environment");
    }

    // Method to be overridden
    public void executeTests() {
        System.out.println("[TestExecutor] Executing tests");
        this.startTime = System.currentTimeMillis();
    }

    // Method to be overridden
    public void teardown() {
        System.out.println("[TestExecutor] Cleaning up test environment");
        this.endTime = System.currentTimeMillis();
    }

    // Common method - not overridden
    public void runTestSuite() {
        System.out.println("[TestExecutor] Starting test execution");
        setup();
        executeTests();
        generateReport();
        teardown();
        System.out.println("[TestExecutor] Test execution completed");
    }

    // Method to be overridden
    public void generateReport() {
        System.out.println("[TestExecutor] Generating test report");
        System.out.println("[TestExecutor] Tests: " + totalTests +
                          ", Passed: " + passedTests +
                          ", Failed: " + failedTests);
    }

    // Getter methods
    public String getExecutorType() {
        return executorType;
    }

    public int getTotalTests() {
        return totalTests;
    }

    public long getExecutionTime() {
        return (endTime - startTime) / 1000; // in seconds
    }
}

// UnitTestExecutor.java
public class UnitTestExecutor extends TestExecutor {

    public UnitTestExecutor() {
        super();
        this.executorType = "Unit Tests";
    }

    @Override
    public void setup() {
        System.out.println("[UnitTestExecutor] Setting up unit test environment");
        System.out.println("[UnitTestExecutor] Loading test classes");
    }

    @Override
    public void executeTests() {
        super.executeTests(); // Call parent method
        System.out.println("[UnitTestExecutor] Executing unit tests");

        // Simulate unit test execution
        String[] unitTests = {
            "testCalculation",
            "testValidation",
            "testDataProcessing"
        };

        this.totalTests = unitTests.length;
        this.passedTests = unitTests.length;
        this.failedTests = 0;

        for (String test : unitTests) {
            System.out.println("  Running test: " + test);
        }

        System.out.println("[UnitTestExecutor] Unit tests completed");

        // Simulate execution time
        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    @Override
    public void generateReport() {
        System.out.println("[UnitTestExecutor] Tests: " + totalTests +
                          ", Passed: " + passedTests +
                          ", Failed: " + failedTests);
        System.out.println("[UnitTestExecutor] Execution time: " + 2 + " seconds");
    }

    @Override
    public void teardown() {
        System.out.println("[UnitTestExecutor] Cleaning up unit test environment");
        super.teardown(); // Call parent method
    }
}

// IntegrationTestExecutor.java
public class IntegrationTestExecutor extends TestExecutor {

    public IntegrationTestExecutor() {
        super();
        this.executorType = "Integration Tests";
    }

    @Override
    public void setup() {
        System.out.println("[IntegrationTestExecutor] Setting up integration test environment");
        System.out.println("[IntegrationTestExecutor] Starting database connections");
        System.out.println("[IntegrationTestExecutor] Starting API services");
    }

    @Override
    public void executeTests() {
        super.executeTests();
        System.out.println("[IntegrationTestExecutor] Executing integration tests");

        // Simulate integration test execution
        String[] integrationTests = {
            "testDatabaseIntegration",
            "testAPIIntegration"
        };

        this.totalTests = integrationTests.length;
        this.passedTests = integrationTests.length;
        this.failedTests = 0;

        for (String test : integrationTests) {
            System.out.println("  Running test: " + test);
        }

        System.out.println("[IntegrationTestExecutor] Integration tests completed");

        // Simulate execution time
        try {
            Thread.sleep(1500);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    @Override
    public void generateReport() {
        System.out.println("[IntegrationTestExecutor] Tests: " + totalTests +
                          ", Passed: " + passedTests +
                          ", Failed: " + failedTests);
        System.out.println("[IntegrationTestExecutor] Execution time: " + 15 + " seconds");
    }

    @Override
    public void teardown() {
        System.out.println("[IntegrationTestExecutor] Stopping services");
        System.out.println("[IntegrationTestExecutor] Cleaning up integration test environment");
        super.teardown();
    }
}

// E2ETestExecutor.java
public class E2ETestExecutor extends TestExecutor {

    private String browser;

    public E2ETestExecutor() {
        super();
        this.executorType = "E2E Tests";
        this.browser = "Chrome";
    }

    @Override
    public void setup() {
        System.out.println("[E2ETestExecutor] Setting up E2E test environment");
        System.out.println("[E2ETestExecutor] Launching browser: " + browser);
        System.out.println("[E2ETestExecutor] Loading application URL");
    }

    @Override
    public void executeTests() {
        super.executeTests();
        System.out.println("[E2ETestExecutor] Executing E2E tests");

        // Simulate E2E test execution
        String[] e2eTests = {
            "testCompleteUserJourney",
            "testCheckoutFlow"
        };

        this.totalTests = e2eTests.length;
        this.passedTests = 1;
        this.failedTests = 1;

        for (String test : e2eTests) {
            System.out.println("  Running test: " + test);
        }

        System.out.println("[E2ETestExecutor] E2E tests completed");

        // Simulate execution time
        try {
            Thread.sleep(3000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    @Override
    public void generateReport() {
        System.out.println("[E2ETestExecutor] Tests: " + totalTests +
                          ", Passed: " + passedTests +
                          ", Failed: " + failedTests);
        System.out.println("[E2ETestExecutor] Execution time: " + 45 + " seconds");
    }

    @Override
    public void teardown() {
        System.out.println("[E2ETestExecutor] Closing browser");
        System.out.println("[E2ETestExecutor] Cleaning up E2E test environment");
        super.teardown();
    }

    // Additional method specific to E2E
    public void captureScreenshot(String testName) {
        System.out.println("[E2ETestExecutor] Capturing screenshot for: " + testName);
    }
}

// TestFramework.java
public class TestFramework {

    // Polymorphic method - accepts any TestExecutor type
    public static void runTests(TestExecutor executor) {
        System.out.println("\nExecutor Type: " + executor.getExecutorType());
        executor.runTestSuite();
    }

    public static void main(String[] args) {
        System.out.println("========================================");
        System.out.println("    POLYMORPHISM DEMONSTRATION");
        System.out.println("    Test Execution Framework");
        System.out.println("========================================");

        // Create different executor objects
        System.out.println("\n--- Executing Unit Tests ---");
        UnitTestExecutor unitExecutor = new UnitTestExecutor();
        unitExecutor.runTestSuite();

        System.out.println("\n--- Executing Integration Tests ---");
        IntegrationTestExecutor integrationExecutor = new IntegrationTestExecutor();
        integrationExecutor.runTestSuite();

        System.out.println("\n--- Executing E2E Tests ---");
        E2ETestExecutor e2eExecutor = new E2ETestExecutor();
        e2eExecutor.runTestSuite();

        // Demonstrate polymorphism
        System.out.println("\n========================================");
        System.out.println("    POLYMORPHIC TEST EXECUTION");
        System.out.println("========================================");
        System.out.println("Executing all test types polymorphically...");

        // Array of TestExecutor (parent type) holding different child objects
        TestExecutor[] executors = {
            new UnitTestExecutor(),
            new IntegrationTestExecutor(),
            new E2ETestExecutor()
        };

        // Polymorphic execution - same method call, different behavior
        for (TestExecutor executor : executors) {
            runTests(executor);
        }

        System.out.println("\n========================================");
        System.out.println("Total execution time: 62 seconds");
        System.out.println("========================================");
    }
}
```

### Deliverables
- TestExecutor.java base class
- At least 3 executor child classes
- TestFramework.java demonstrating polymorphism
- Document explaining runtime polymorphism

### Extension Challenges
1. Add performance test executor
2. Implement test prioritization in executors
3. Add parallel execution capability
4. Create a test executor factory

---

## Exercise 10: Interfaces and Abstraction

### Objective
Master interfaces and abstract classes by creating a flexible test reporting framework that supports multiple report formats.

### Scenario
Create an interface for test reporters that can generate reports in different formats (HTML, JSON, XML, PDF). Use abstract classes to provide common functionality.

### Step-by-Step Instructions

1. **Create TestReporter interface with:**
   - Method signatures for report generation
   - Default methods (Java 8+)

2. **Create AbstractReporter abstract class:**
   - Implement common functionality
   - Leave specific methods abstract

3. **Create concrete reporter classes:**
   - HTMLReporter
   - JSONReporter
   - XMLReporter

### Code Template

```java
// TestReporter.java (Interface)
public interface TestReporter {
    // TODO: Define method signatures

}

// AbstractReporter.java
public abstract class AbstractReporter implements TestReporter {
    // TODO: Common properties and methods
    // TODO: Abstract methods

}

// HTMLReporter.java
public class HTMLReporter extends AbstractReporter {
    // TODO: Implement abstract methods

}
```

### Expected Output

```
========================================
    INTERFACE & ABSTRACTION DEMO
    Test Reporting Framework
========================================

--- HTML Reporter ---
[AbstractReporter] Initializing reporter
[HTMLReporter] Setting up HTML reporter
[AbstractReporter] Collecting test data...
[AbstractReporter] Test Suite: Regression Suite
[AbstractReporter] Total Tests: 50
[AbstractReporter] Passed: 45, Failed: 5
[HTMLReporter] Generating HTML report
[HTMLReporter] Creating HTML header
[HTMLReporter] Creating test summary table
[HTMLReporter] Adding test details
[HTMLReporter] Adding charts and graphs
[HTMLReporter] Creating HTML footer
[HTMLReporter] Report saved to: test-report.html
Report generated successfully!

--- JSON Reporter ---
[AbstractReporter] Initializing reporter
[JSONReporter] Setting up JSON reporter
[AbstractReporter] Collecting test data...
[AbstractReporter] Test Suite: Regression Suite
[AbstractReporter] Total Tests: 50
[AbstractReporter] Passed: 45, Failed: 5
[JSONReporter] Generating JSON report
{
  "testSuite": "Regression Suite",
  "totalTests": 50,
  "passed": 45,
  "failed": 5,
  "executionTime": "120 seconds"
}
[JSONReporter] Report saved to: test-report.json
Report generated successfully!

--- XML Reporter ---
[AbstractReporter] Initializing reporter
[XMLReporter] Setting up XML reporter
[AbstractReporter] Collecting test data...
[AbstractReporter] Test Suite: Regression Suite
[AbstractReporter] Total Tests: 50
[AbstractReporter] Passed: 45, Failed: 5
[XMLReporter] Generating XML report
<?xml version="1.0" encoding="UTF-8"?>
<testReport>
  <testSuite>Regression Suite</testSuite>
  <totalTests>50</totalTests>
  <passed>45</passed>
  <failed>5</failed>
  <executionTime>120 seconds</executionTime>
</testReport>
[XMLReporter] Report saved to: test-report.xml
Report generated successfully!

========================================
    MULTIPLE INTERFACE IMPLEMENTATION
========================================
[PDFReporter] Exporting to PDF format
[PDFReporter] Sending email notification
========================================
```

### Solution Code

```java
// TestReporter.java (Interface)
public interface TestReporter {

    // Abstract methods - must be implemented by classes
    void generateReport();
    void saveReport(String filename);

    // Default method (Java 8+) - can be overridden
    default void printReportInfo() {
        System.out.println("Report generated successfully!");
    }

    // Static method in interface
    static String getReportVersion() {
        return "Report Generator v2.0";
    }
}

// Exportable.java (Second Interface)
public interface Exportable {
    void exportToFile(String format);
}

// Notifiable.java (Third Interface)
public interface Notifiable {
    void sendNotification(String recipient);
}

// AbstractReporter.java
public abstract class AbstractReporter implements TestReporter {

    // Common properties
    protected String reportName;
    protected String testSuiteName;
    protected int totalTests;
    protected int passedTests;
    protected int failedTests;
    protected String executionTime;
    protected String reportPath;

    // Constructor
    public AbstractReporter(String reportName) {
        this.reportName = reportName;
        System.out.println("[AbstractReporter] Initializing reporter");
    }

    // Concrete method - common to all reporters
    public void collectTestData(String suite, int total, int passed, int failed, String time) {
        System.out.println("[AbstractReporter] Collecting test data...");
        this.testSuiteName = suite;
        this.totalTests = total;
        this.passedTests = passed;
        this.failedTests = failed;
        this.executionTime = time;

        System.out.println("[AbstractReporter] Test Suite: " + testSuiteName);
        System.out.println("[AbstractReporter] Total Tests: " + totalTests);
        System.out.println("[AbstractReporter] Passed: " + passedTests + ", Failed: " + failedTests);
    }

    // Concrete method
    public void validateData() {
        if (totalTests != (passedTests + failedTests)) {
            System.out.println("[AbstractReporter] Warning: Test count mismatch!");
        }
    }

    // Abstract method - must be implemented by child classes
    public abstract void formatReport();

    // Implemented interface method
    @Override
    public void saveReport(String filename) {
        this.reportPath = filename;
        System.out.println("[AbstractReporter] Report saved to: " + filename);
    }

    // Common getter
    public String getReportPath() {
        return reportPath;
    }
}

// HTMLReporter.java
public class HTMLReporter extends AbstractReporter {

    public HTMLReporter() {
        super("HTML Report");
        System.out.println("[HTMLReporter] Setting up HTML reporter");
    }

    @Override
    public void formatReport() {
        System.out.println("[HTMLReporter] Creating HTML header");
        System.out.println("[HTMLReporter] Creating test summary table");
        System.out.println("[HTMLReporter] Adding test details");
        System.out.println("[HTMLReporter] Adding charts and graphs");
        System.out.println("[HTMLReporter] Creating HTML footer");
    }

    @Override
    public void generateReport() {
        System.out.println("[HTMLReporter] Generating HTML report");
        validateData();
        formatReport();
    }

    @Override
    public void saveReport(String filename) {
        super.saveReport(filename + ".html");
    }
}

// JSONReporter.java
public class JSONReporter extends AbstractReporter {

    public JSONReporter() {
        super("JSON Report");
        System.out.println("[JSONReporter] Setting up JSON reporter");
    }

    @Override
    public void formatReport() {
        System.out.println("{");
        System.out.println("  \"testSuite\": \"" + testSuiteName + "\",");
        System.out.println("  \"totalTests\": " + totalTests + ",");
        System.out.println("  \"passed\": " + passedTests + ",");
        System.out.println("  \"failed\": " + failedTests + ",");
        System.out.println("  \"executionTime\": \"" + executionTime + "\"");
        System.out.println("}");
    }

    @Override
    public void generateReport() {
        System.out.println("[JSONReporter] Generating JSON report");
        validateData();
        formatReport();
    }

    @Override
    public void saveReport(String filename) {
        super.saveReport(filename + ".json");
    }
}

// XMLReporter.java
public class XMLReporter extends AbstractReporter {

    public XMLReporter() {
        super("XML Report");
        System.out.println("[XMLReporter] Setting up XML reporter");
    }

    @Override
    public void formatReport() {
        System.out.println("<?xml version=\"1.0\" encoding=\"UTF-8\"?>");
        System.out.println("<testReport>");
        System.out.println("  <testSuite>" + testSuiteName + "</testSuite>");
        System.out.println("  <totalTests>" + totalTests + "</totalTests>");
        System.out.println("  <passed>" + passedTests + "</passed>");
        System.out.println("  <failed>" + failedTests + "</failed>");
        System.out.println("  <executionTime>" + executionTime + "</executionTime>");
        System.out.println("</testReport>");
    }

    @Override
    public void generateReport() {
        System.out.println("[XMLReporter] Generating XML report");
        validateData();
        formatReport();
    }

    @Override
    public void saveReport(String filename) {
        super.saveReport(filename + ".xml");
    }
}

// PDFReporter.java (Multiple Interface Implementation)
public class PDFReporter extends AbstractReporter implements Exportable, Notifiable {

    public PDFReporter() {
        super("PDF Report");
        System.out.println("[PDFReporter] Setting up PDF reporter");
    }

    @Override
    public void formatReport() {
        System.out.println("[PDFReporter] Creating PDF document");
        System.out.println("[PDFReporter] Adding header and footer");
        System.out.println("[PDFReporter] Creating test summary");
    }

    @Override
    public void generateReport() {
        System.out.println("[PDFReporter] Generating PDF report");
        validateData();
        formatReport();
    }

    @Override
    public void saveReport(String filename) {
        super.saveReport(filename + ".pdf");
    }

    // Implementing Exportable interface
    @Override
    public void exportToFile(String format) {
        System.out.println("[PDFReporter] Exporting to " + format + " format");
    }

    // Implementing Notifiable interface
    @Override
    public void sendNotification(String recipient) {
        System.out.println("[PDFReporter] Sending email notification to: " + recipient);
    }
}

// ReportingFramework.java
public class ReportingFramework {

    // Polymorphic method accepting interface type
    public static void generateAndSave(TestReporter reporter, String filename) {
        reporter.generateReport();
        reporter.saveReport(filename);
        reporter.printReportInfo();
    }

    public static void main(String[] args) {
        System.out.println("========================================");
        System.out.println("    INTERFACE & ABSTRACTION DEMO");
        System.out.println("    Test Reporting Framework");
        System.out.println("========================================");

        // Test data
        String suite = "Regression Suite";
        int total = 50;
        int passed = 45;
        int failed = 5;
        String time = "120 seconds";

        // HTML Reporter
        System.out.println("\n--- HTML Reporter ---");
        HTMLReporter htmlReporter = new HTMLReporter();
        htmlReporter.collectTestData(suite, total, passed, failed, time);
        generateAndSave(htmlReporter, "test-report");

        // JSON Reporter
        System.out.println("\n--- JSON Reporter ---");
        JSONReporter jsonReporter = new JSONReporter();
        jsonReporter.collectTestData(suite, total, passed, failed, time);
        generateAndSave(jsonReporter, "test-report");

        // XML Reporter
        System.out.println("\n--- XML Reporter ---");
        XMLReporter xmlReporter = new XMLReporter();
        xmlReporter.collectTestData(suite, total, passed, failed, time);
        generateAndSave(xmlReporter, "test-report");

        // PDF Reporter with multiple interfaces
        System.out.println("\n========================================");
        System.out.println("    MULTIPLE INTERFACE IMPLEMENTATION");
        System.out.println("========================================");
        PDFReporter pdfReporter = new PDFReporter();
        pdfReporter.collectTestData(suite, total, passed, failed, time);
        pdfReporter.generateReport();
        pdfReporter.exportToFile("PDF");
        pdfReporter.sendNotification("team@example.com");

        // Using interface static method
        System.out.println("\nUsing static interface method:");
        System.out.println(TestReporter.getReportVersion());

        System.out.println("========================================");
    }
}
```

### Deliverables
- TestReporter interface
- AbstractReporter abstract class
- At least 3 concrete reporter implementations
- ReportingFramework.java demonstrating usage
- Document explaining interface vs abstract class

### Extension Challenges
1. Add functional interface for custom report filters
2. Implement CSV reporter
3. Create composite reports combining multiple formats
4. Add report templates using interface

---

## Exercise 11: Exception Handling for Test Automation

### Objective
Master exception handling techniques essential for robust test automation frameworks.

### Scenario
Create a test execution engine that gracefully handles various exceptions that can occur during test execution, such as element not found, timeout, database errors, and API failures.

### Step-by-Step Instructions

1. **Create custom exception classes:**
   - TestExecutionException
   - ElementNotFoundException
   - TestDataException

2. **Create TestEngine class with:**
   - Try-catch blocks
   - Multiple catch blocks
   - Finally block
   - Throw and throws keywords
   - Custom exception handling

### Code Template

```java
// Custom Exception Classes
class TestExecutionException extends Exception {
    // TODO: Implement custom exception
}

// TestEngine.java
public class TestEngine {
    // TODO: Methods with exception handling

    public static void main(String[] args) {
        // TODO: Demonstrate exception handling
    }
}
```

### Expected Output

```
========================================
    EXCEPTION HANDLING DEMONSTRATION
    Test Automation Framework
========================================

--- Test Scenario 1: Normal Execution ---
[TestEngine] Starting test execution
[TestEngine] Initializing browser
[TestEngine] Navigating to login page
[TestEngine] Finding element: username
[TestEngine] Element found successfully
[TestEngine] Entering test data
[TestEngine] Clicking login button
[TestEngine] Test passed successfully
[TestEngine] Cleaning up resources
[TestEngine] Browser closed

--- Test Scenario 2: Element Not Found ---
[TestEngine] Starting test execution
[TestEngine] Initializing browser
[TestEngine] Navigating to login page
[TestEngine] Finding element: invalid_element
[ERROR] Element not found: invalid_element
[ERROR] Test failed: Could not locate element
[TestEngine] Taking screenshot for failure
[TestEngine] Cleaning up resources
[TestEngine] Browser closed

--- Test Scenario 3: Test Data Error ---
[TestEngine] Starting test execution
[TestEngine] Initializing browser
[TestEngine] Reading test data from file
[ERROR] Test data file not found: testdata.csv
[ERROR] Using default test data instead
[TestEngine] Continuing with default values
[TestEngine] Test completed with warnings
[TestEngine] Cleaning up resources
[TestEngine] Browser closed

--- Test Scenario 4: Multiple Exceptions ---
[TestEngine] Starting test execution
[TestEngine] Initializing browser
[TestEngine] Connecting to database
[ERROR] Database connection failed: Connection timeout
[TestEngine] Attempting retry (1/3)
[ERROR] Database connection failed: Connection timeout
[TestEngine] Attempting retry (2/3)
[TestEngine] Database connected successfully
[TestEngine] Test completed
[TestEngine] Cleaning up resources
[TestEngine] Browser closed
[TestEngine] Database connection closed

--- Test Scenario 5: Custom Exception ---
[TestEngine] Validating test prerequisites
[ERROR] Critical error: Browser version incompatible
Test execution aborted: Browser Chrome version 90 is incompatible. Required: 95+
[TestEngine] Cleaning up resources

========================================
    EXCEPTION HANDLING SUMMARY
========================================
Total Scenarios: 5
Successful: 2
Failed: 2
Errors Handled: 5
========================================
```

### Solution Code

```java
// Custom Exception Classes

// Base custom exception for test execution
class TestExecutionException extends Exception {
    private String testCaseId;
    private String errorCode;

    public TestExecutionException(String message) {
        super(message);
    }

    public TestExecutionException(String message, String testCaseId) {
        super(message);
        this.testCaseId = testCaseId;
    }

    public TestExecutionException(String message, Throwable cause) {
        super(message, cause);
    }

    public String getTestCaseId() {
        return testCaseId;
    }
}

// Element not found exception
class ElementNotFoundException extends TestExecutionException {
    private String elementName;

    public ElementNotFoundException(String elementName) {
        super("Element not found: " + elementName);
        this.elementName = elementName;
    }

    public String getElementName() {
        return elementName;
    }
}

// Test data exception
class TestDataException extends TestExecutionException {
    private String dataFile;

    public TestDataException(String message, String dataFile) {
        super(message);
        this.dataFile = dataFile;
    }

    public String getDataFile() {
        return dataFile;
    }
}

// Database exception
class DatabaseException extends TestExecutionException {
    public DatabaseException(String message) {
        super(message);
    }

    public DatabaseException(String message, Throwable cause) {
        super(message, cause);
    }
}

// TestEngine.java
public class TestEngine {

    private String browserName;
    private boolean isBrowserInitialized;
    private boolean isDatabaseConnected;

    public TestEngine(String browserName) {
        this.browserName = browserName;
        this.isBrowserInitialized = false;
        this.isDatabaseConnected = false;
    }

    // Method with throws declaration
    public void initializeBrowser() throws TestExecutionException {
        System.out.println("[TestEngine] Initializing browser");
        try {
            // Simulate browser initialization
            Thread.sleep(1000);
            this.isBrowserInitialized = true;
        } catch (InterruptedException e) {
            throw new TestExecutionException("Failed to initialize browser", e);
        }
    }

    // Method with try-catch
    public void navigateToPage(String url) {
        System.out.println("[TestEngine] Navigating to " + url);
        try {
            // Simulate navigation
            if (url == null || url.isEmpty()) {
                throw new IllegalArgumentException("URL cannot be null or empty");
            }
            Thread.sleep(500);
        } catch (IllegalArgumentException e) {
            System.out.println("[ERROR] Invalid URL: " + e.getMessage());
        } catch (InterruptedException e) {
            System.out.println("[ERROR] Navigation interrupted");
        }
    }

    // Method throwing custom exception
    public void findElement(String elementName) throws ElementNotFoundException {
        System.out.println("[TestEngine] Finding element: " + elementName);

        // Simulate element not found
        if (elementName.equals("invalid_element")) {
            throw new ElementNotFoundException(elementName);
        }

        System.out.println("[TestEngine] Element found successfully");
    }

    // Method with multiple exception handling
    public void readTestData(String filename) throws TestDataException {
        System.out.println("[TestEngine] Reading test data from file");

        try {
            // Simulate file reading
            if (!filename.endsWith(".csv")) {
                throw new IllegalArgumentException("Invalid file format");
            }

            if (filename.equals("testdata.csv")) {
                throw new java.io.FileNotFoundException("File not found");
            }

        } catch (java.io.FileNotFoundException e) {
            // Catch specific exception and throw custom exception
            throw new TestDataException("Test data file not found: " + filename, filename);
        } catch (IllegalArgumentException e) {
            throw new TestDataException("Invalid test data format: " + e.getMessage(), filename);
        }
    }

    // Method with finally block
    public void executeTestWithCleanup(String testName) {
        System.out.println("[TestEngine] Starting test execution");

        try {
            initializeBrowser();
            navigateToPage("login page");
            findElement("username");
            System.out.println("[TestEngine] Entering test data");
            System.out.println("[TestEngine] Clicking login button");
            System.out.println("[TestEngine] Test passed successfully");

        } catch (TestExecutionException e) {
            System.out.println("[ERROR] Test failed: " + e.getMessage());
            takeScreenshot(testName);
        } finally {
            // Always executes
            cleanup();
        }
    }

    // Method with try-with-resources simulation
    public void connectToDatabase(int maxRetries) {
        System.out.println("[TestEngine] Connecting to database");
        int attempts = 0;
        boolean connected = false;

        while (attempts < maxRetries && !connected) {
            try {
                attempts++;
                // Simulate connection
                if (attempts < 3) {
                    throw new DatabaseException("Connection timeout");
                }
                this.isDatabaseConnected = true;
                connected = true;
                System.out.println("[TestEngine] Database connected successfully");

            } catch (DatabaseException e) {
                System.out.println("[ERROR] Database connection failed: " + e.getMessage());
                if (attempts < maxRetries) {
                    System.out.println("[TestEngine] Attempting retry (" + attempts + "/" + maxRetries + ")");
                }
            }
        }

        if (!connected) {
            System.out.println("[ERROR] Failed to connect after " + maxRetries + " attempts");
        }
    }

    // Method that throws exception based on conditions
    public void validatePrerequisites(String browser, int version) throws TestExecutionException {
        System.out.println("[TestEngine] Validating test prerequisites");

        if (version < 95) {
            throw new TestExecutionException(
                "Browser " + browser + " version " + version + " is incompatible. Required: 95+"
            );
        }
    }

    private void takeScreenshot(String testName) {
        System.out.println("[TestEngine] Taking screenshot for failure");
    }

    private void cleanup() {
        System.out.println("[TestEngine] Cleaning up resources");
        if (isBrowserInitialized) {
            System.out.println("[TestEngine] Browser closed");
            isBrowserInitialized = false;
        }
        if (isDatabaseConnected) {
            System.out.println("[TestEngine] Database connection closed");
            isDatabaseConnected = false;
        }
    }

    // Nested try-catch example
    public void executeComplexTest() {
        try {
            initializeBrowser();

            try {
                findElement("username");
                findElement("password");
            } catch (ElementNotFoundException e) {
                System.out.println("[ERROR] Element issue: " + e.getMessage());
                // Try alternative locator
                System.out.println("[TestEngine] Trying alternative element locator");
            }

        } catch (TestExecutionException e) {
            System.out.println("[ERROR] Browser initialization failed");
        } finally {
            cleanup();
        }
    }

    public static void main(String[] args) {
        System.out.println("========================================");
        System.out.println("    EXCEPTION HANDLING DEMONSTRATION");
        System.out.println("    Test Automation Framework");
        System.out.println("========================================");

        TestEngine engine = new TestEngine("Chrome");
        int successful = 0;
        int failed = 0;
        int errors = 0;

        // Scenario 1: Normal execution
        System.out.println("\n--- Test Scenario 1: Normal Execution ---");
        try {
            engine.executeTestWithCleanup("TC_001");
            successful++;
        } catch (Exception e) {
            failed++;
        }

        // Scenario 2: Element not found
        System.out.println("\n--- Test Scenario 2: Element Not Found ---");
        try {
            engine.initializeBrowser();
            engine.navigateToPage("login page");
            engine.findElement("invalid_element");
        } catch (ElementNotFoundException e) {
            System.out.println("[ERROR] " + e.getMessage());
            System.out.println("[ERROR] Test failed: Could not locate element");
            engine.takeScreenshot("TC_002");
            failed++;
            errors++;
        } catch (TestExecutionException e) {
            System.out.println("[ERROR] " + e.getMessage());
        } finally {
            engine.cleanup();
        }

        // Scenario 3: Test data error with recovery
        System.out.println("\n--- Test Scenario 3: Test Data Error ---");
        try {
            engine.initializeBrowser();
            engine.readTestData("testdata.csv");
        } catch (TestDataException e) {
            System.out.println("[ERROR] " + e.getMessage());
            System.out.println("[ERROR] Using default test data instead");
            System.out.println("[TestEngine] Continuing with default values");
            System.out.println("[TestEngine] Test completed with warnings");
            successful++;
            errors++;
        } catch (TestExecutionException e) {
            System.out.println("[ERROR] " + e.getMessage());
        } finally {
            engine.cleanup();
        }

        // Scenario 4: Database connection with retry
        System.out.println("\n--- Test Scenario 4: Multiple Exceptions ---");
        try {
            engine.initializeBrowser();
            engine.connectToDatabase(3);
            System.out.println("[TestEngine] Test completed");
            successful++;
            errors += 2;
        } catch (TestExecutionException e) {
            failed++;
        } finally {
            engine.cleanup();
        }

        // Scenario 5: Custom exception
        System.out.println("\n--- Test Scenario 5: Custom Exception ---");
        try {
            engine.validatePrerequisites("Chrome", 90);
        } catch (TestExecutionException e) {
            System.out.println("[ERROR] Critical error: " + e.getMessage());
            System.out.println("Test execution aborted: " + e.getMessage());
            failed++;
            errors++;
        } finally {
            engine.cleanup();
        }

        // Summary
        System.out.println("\n========================================");
        System.out.println("    EXCEPTION HANDLING SUMMARY");
        System.out.println("========================================");
        System.out.println("Total Scenarios: 5");
        System.out.println("Successful: " + successful);
        System.out.println("Failed: " + failed);
        System.out.println("Errors Handled: " + errors);
        System.out.println("========================================");
    }
}
```

### Deliverables
- Custom exception classes
- TestEngine.java with comprehensive exception handling
- Document explaining each exception handling scenario
- Best practices guide for exception handling in test automation

### Extension Challenges
1. Implement exception chaining for root cause analysis
2. Create exception logger that writes to file
3. Add exception recovery strategies
4. Implement circuit breaker pattern for external service calls

---

## Exercise 12: Collections - ArrayList and HashMap for Test Data

### Objective
Master Java Collections Framework by managing test data, test cases, and test results using ArrayList and HashMap.

### Scenario
Create a comprehensive test data management system using collections to store test cases, test data sets, and test execution results.

### Step-by-Step Instructions

1. **Create TestDataManager.java using:**
   - ArrayList to store test cases
   - HashMap to store test data key-value pairs
   - Iterate through collections
   - Sort and filter collections

2. **Implement functionality for:**
   - Adding and removing test cases
   - Searching test cases by criteria
   - Grouping tests by priority
   - Managing test execution history

### Code Template

```java
import java.util.*;

public class TestDataManager {

    // TODO: Declare ArrayList and HashMap

    // TODO: Methods to add, remove, search test data

    // TODO: Methods to filter and sort

    public static void main(String[] args) {
        // TODO: Demonstrate collections usage
    }
}
```

### Expected Output

```
========================================
    COLLECTIONS DEMONSTRATION
    Test Data Management System
========================================

--- Adding Test Cases to ArrayList ---
Added: TC_001 - Login with valid credentials
Added: TC_002 - Login with invalid password
Added: TC_003 - Logout functionality
Added: TC_004 - Registration with valid data
Added: TC_005 - Password reset
Total test cases: 5

--- Displaying All Test Cases ---
1. TC_001 - Login with valid credentials [P0] - PASSED
2. TC_002 - Login with invalid password [P1] - PASSED
3. TC_003 - Logout functionality [P2] - FAILED
4. TC_004 - Registration with valid data [P0] - NOT_RUN
5. TC_005 - Password reset [P1] - PASSED

--- HashMap: Test Configuration ---
Test Configuration Data:
  browser: Chrome
  environment: QA
  timeout: 30
  retryCount: 3
  headless: false

--- Searching Test Cases ---
Search by ID 'TC_003':
Found: TC_003 - Logout functionality [P2]

Search by Priority 'P0':
  TC_001 - Login with valid credentials
  TC_004 - Registration with valid data

--- Filtering Failed Tests ---
Failed Tests:
  TC_003 - Logout functionality [P2]

--- Test Execution Statistics ---
Total Tests: 5
Passed: 3 (60.0%)
Failed: 1 (20.0%)
Not Run: 1 (20.0%)

--- HashMap: Test Results by Module ---
Module: Authentication
  TC_001: PASSED
  TC_002: PASSED
  TC_003: FAILED

Module: User Management
  TC_004: NOT_RUN
  TC_005: PASSED

--- Sorting Tests by Priority ---
Sorted by Priority:
  [P0] TC_001 - Login with valid credentials
  [P0] TC_004 - Registration with valid data
  [P1] TC_002 - Login with invalid password
  [P1] TC_005 - Password reset
  [P2] TC_003 - Logout functionality

--- Test Data Sets (HashMap) ---
Test Data Set 1:
  username: testuser1
  password: Pass@123
  email: user1@test.com

Test Data Set 2:
  username: testuser2
  password: Pass@456
  email: user2@test.com

========================================
    ADVANCED COLLECTIONS OPERATIONS
========================================

--- Nested Collections: Test Suite Structure ---
Suite: Smoke Tests
  TC_001 - Login with valid credentials
  TC_003 - Logout functionality

Suite: Regression Tests
  TC_001 - Login with valid credentials
  TC_002 - Login with invalid password
  TC_004 - Registration with valid data
  TC_005 - Password reset

--- Test Execution History (LinkedList) ---
Execution History:
  1. 2024-01-15 10:30 - TC_001 - PASSED - 5s
  2. 2024-01-15 10:31 - TC_002 - PASSED - 4s
  3. 2024-01-15 10:32 - TC_003 - FAILED - 8s

========================================
Collections Demo Completed!
========================================
```

### Solution Code

```java
import java.util.*;

// TestCase class to store in collections
class TestCase {
    String id;
    String name;
    String priority;
    String status;
    String module;
    int executionTime;

    public TestCase(String id, String name, String priority, String module) {
        this.id = id;
        this.name = name;
        this.priority = priority;
        this.module = module;
        this.status = "NOT_RUN";
        this.executionTime = 0;
    }

    public void setStatus(String status) {
        this.status = status;
    }

    public void setExecutionTime(int time) {
        this.executionTime = time;
    }

    @Override
    public String toString() {
        return id + " - " + name + " [" + priority + "] - " + status;
    }

    public String toDetailString() {
        return id + " - " + name + " [" + priority + "] - Module: " + module + " - Status: " + status;
    }
}

// Test execution record
class TestExecution {
    String date;
    String testId;
    String status;
    int duration;

    public TestExecution(String date, String testId, String status, int duration) {
        this.date = date;
        this.testId = testId;
        this.status = status;
        this.duration = duration;
    }

    @Override
    public String toString() {
        return date + " - " + testId + " - " + status + " - " + duration + "s";
    }
}

public class TestDataManager {

    // ArrayList to store test cases
    private ArrayList<TestCase> testCases;

    // HashMap to store configuration
    private HashMap<String, String> configuration;

    // HashMap to store test results by module
    private HashMap<String, ArrayList<String>> resultsByModule;

    // HashMap for test data sets
    private ArrayList<HashMap<String, String>> testDataSets;

    // LinkedList for execution history
    private LinkedList<TestExecution> executionHistory;

    // HashMap for test suites
    private HashMap<String, ArrayList<String>> testSuites;

    public TestDataManager() {
        testCases = new ArrayList<>();
        configuration = new HashMap<>();
        resultsByModule = new HashMap<>();
        testDataSets = new ArrayList<>();
        executionHistory = new LinkedList<>();
        testSuites = new HashMap<>();
    }

    // Add test case to ArrayList
    public void addTestCase(TestCase testCase) {
        testCases.add(testCase);
        System.out.println("Added: " + testCase.toString());
    }

    // Remove test case
    public boolean removeTestCase(String testId) {
        return testCases.removeIf(tc -> tc.id.equals(testId));
    }

    // Search test case by ID
    public TestCase findTestCaseById(String testId) {
        for (TestCase tc : testCases) {
            if (tc.id.equals(testId)) {
                return tc;
            }
        }
        return null;
    }

    // Find all test cases by priority
    public ArrayList<TestCase> findByPriority(String priority) {
        ArrayList<TestCase> result = new ArrayList<>();
        for (TestCase tc : testCases) {
            if (tc.priority.equals(priority)) {
                result.add(tc);
            }
        }
        return result;
    }

    // Find all failed tests
    public ArrayList<TestCase> getFailedTests() {
        ArrayList<TestCase> failed = new ArrayList<>();
        for (TestCase tc : testCases) {
            if (tc.status.equals("FAILED")) {
                failed.add(tc);
            }
        }
        return failed;
    }

    // Display all test cases
    public void displayAllTestCases() {
        System.out.println("\n--- Displaying All Test Cases ---");
        for (int i = 0; i < testCases.size(); i++) {
            System.out.println((i + 1) + ". " + testCases.get(i).toString());
        }
    }

    // Set configuration using HashMap
    public void setConfiguration(String key, String value) {
        configuration.put(key, value);
    }

    // Display configuration
    public void displayConfiguration() {
        System.out.println("\n--- HashMap: Test Configuration ---");
        System.out.println("Test Configuration Data:");
        for (Map.Entry<String, String> entry : configuration.entrySet()) {
            System.out.println("  " + entry.getKey() + ": " + entry.getValue());
        }
    }

    // Add test results by module
    public void addResultToModule(String module, String result) {
        if (!resultsByModule.containsKey(module)) {
            resultsByModule.put(module, new ArrayList<>());
        }
        resultsByModule.get(module).add(result);
    }

    // Display results by module
    public void displayResultsByModule() {
        System.out.println("\n--- HashMap: Test Results by Module ---");
        for (Map.Entry<String, ArrayList<String>> entry : resultsByModule.entrySet()) {
            System.out.println("Module: " + entry.getKey());
            for (String result : entry.getValue()) {
                System.out.println("  " + result);
            }
            System.out.println();
        }
    }

    // Add test data set
    public void addTestDataSet(HashMap<String, String> dataSet) {
        testDataSets.add(dataSet);
    }

    // Display test data sets
    public void displayTestDataSets() {
        System.out.println("\n--- Test Data Sets (HashMap) ---");
        for (int i = 0; i < testDataSets.size(); i++) {
            System.out.println("Test Data Set " + (i + 1) + ":");
            HashMap<String, String> dataSet = testDataSets.get(i);
            for (Map.Entry<String, String> entry : dataSet.entrySet()) {
                System.out.println("  " + entry.getKey() + ": " + entry.getValue());
            }
            System.out.println();
        }
    }

    // Calculate statistics
    public void displayStatistics() {
        System.out.println("\n--- Test Execution Statistics ---");
        int total = testCases.size();
        int passed = 0, failed = 0, notRun = 0;

        for (TestCase tc : testCases) {
            switch (tc.status) {
                case "PASSED": passed++; break;
                case "FAILED": failed++; break;
                case "NOT_RUN": notRun++; break;
            }
        }

        System.out.println("Total Tests: " + total);
        System.out.println("Passed: " + passed + " (" + String.format("%.1f", passed * 100.0 / total) + "%)");
        System.out.println("Failed: " + failed + " (" + String.format("%.1f", failed * 100.0 / total) + "%)");
        System.out.println("Not Run: " + notRun + " (" + String.format("%.1f", notRun * 100.0 / total) + "%)");
    }

    // Sort tests by priority
    public void displaySortedByPriority() {
        System.out.println("\n--- Sorting Tests by Priority ---");
        ArrayList<TestCase> sorted = new ArrayList<>(testCases);

        Collections.sort(sorted, new Comparator<TestCase>() {
            @Override
            public int compare(TestCase t1, TestCase t2) {
                return t1.priority.compareTo(t2.priority);
            }
        });

        System.out.println("Sorted by Priority:");
        for (TestCase tc : sorted) {
            System.out.println("  [" + tc.priority + "] " + tc.id + " - " + tc.name);
        }
    }

    // Add to test suite
    public void addToTestSuite(String suiteName, String testId) {
        if (!testSuites.containsKey(suiteName)) {
            testSuites.put(suiteName, new ArrayList<>());
        }
        testSuites.get(suiteName).add(testId);
    }

    // Display test suites
    public void displayTestSuites() {
        System.out.println("\n--- Nested Collections: Test Suite Structure ---");
        for (Map.Entry<String, ArrayList<String>> entry : testSuites.entrySet()) {
            System.out.println("Suite: " + entry.getKey());
            for (String testId : entry.getValue()) {
                TestCase tc = findTestCaseById(testId);
                if (tc != null) {
                    System.out.println("  " + tc.id + " - " + tc.name);
                }
            }
            System.out.println();
        }
    }

    // Add execution history
    public void addExecutionHistory(String date, String testId, String status, int duration) {
        executionHistory.add(new TestExecution(date, testId, status, duration));
    }

    // Display execution history
    public void displayExecutionHistory() {
        System.out.println("\n--- Test Execution History (LinkedList) ---");
        System.out.println("Execution History:");
        int count = 1;
        for (TestExecution execution : executionHistory) {
            System.out.println("  " + count++ + ". " + execution.toString());
        }
    }

    public static void main(String[] args) {
        System.out.println("========================================");
        System.out.println("    COLLECTIONS DEMONSTRATION");
        System.out.println("    Test Data Management System");
        System.out.println("========================================");

        TestDataManager manager = new TestDataManager();

        // Adding test cases to ArrayList
        System.out.println("\n--- Adding Test Cases to ArrayList ---");
        manager.addTestCase(new TestCase("TC_001", "Login with valid credentials", "P0", "Authentication"));
        manager.addTestCase(new TestCase("TC_002", "Login with invalid password", "P1", "Authentication"));
        manager.addTestCase(new TestCase("TC_003", "Logout functionality", "P2", "Authentication"));
        manager.addTestCase(new TestCase("TC_004", "Registration with valid data", "P0", "User Management"));
        manager.addTestCase(new TestCase("TC_005", "Password reset", "P1", "User Management"));

        System.out.println("Total test cases: " + manager.testCases.size());

        // Update test statuses
        manager.findTestCaseById("TC_001").setStatus("PASSED");
        manager.findTestCaseById("TC_002").setStatus("PASSED");
        manager.findTestCaseById("TC_003").setStatus("FAILED");
        manager.findTestCaseById("TC_005").setStatus("PASSED");

        // Display all test cases
        manager.displayAllTestCases();

        // HashMap: Configuration
        manager.setConfiguration("browser", "Chrome");
        manager.setConfiguration("environment", "QA");
        manager.setConfiguration("timeout", "30");
        manager.setConfiguration("retryCount", "3");
        manager.setConfiguration("headless", "false");
        manager.displayConfiguration();

        // Search operations
        System.out.println("\n--- Searching Test Cases ---");
        System.out.println("Search by ID 'TC_003':");
        TestCase found = manager.findTestCaseById("TC_003");
        if (found != null) {
            System.out.println("Found: " + found.toString());
        }

        System.out.println("\nSearch by Priority 'P0':");
        ArrayList<TestCase> p0Tests = manager.findByPriority("P0");
        for (TestCase tc : p0Tests) {
            System.out.println("  " + tc.id + " - " + tc.name);
        }

        // Filter failed tests
        System.out.println("\n--- Filtering Failed Tests ---");
        ArrayList<TestCase> failedTests = manager.getFailedTests();
        System.out.println("Failed Tests:");
        for (TestCase tc : failedTests) {
            System.out.println("  " + tc.toString());
        }

        // Statistics
        manager.displayStatistics();

        // Results by module
        manager.addResultToModule("Authentication", "TC_001: PASSED");
        manager.addResultToModule("Authentication", "TC_002: PASSED");
        manager.addResultToModule("Authentication", "TC_003: FAILED");
        manager.addResultToModule("User Management", "TC_004: NOT_RUN");
        manager.addResultToModule("User Management", "TC_005: PASSED");
        manager.displayResultsByModule();

        // Sort tests
        manager.displaySortedByPriority();

        // Test data sets
        HashMap<String, String> dataSet1 = new HashMap<>();
        dataSet1.put("username", "testuser1");
        dataSet1.put("password", "Pass@123");
        dataSet1.put("email", "user1@test.com");
        manager.addTestDataSet(dataSet1);

        HashMap<String, String> dataSet2 = new HashMap<>();
        dataSet2.put("username", "testuser2");
        dataSet2.put("password", "Pass@456");
        dataSet2.put("email", "user2@test.com");
        manager.addTestDataSet(dataSet2);

        manager.displayTestDataSets();

        // Advanced operations
        System.out.println("========================================");
        System.out.println("    ADVANCED COLLECTIONS OPERATIONS");
        System.out.println("========================================");

        // Test suites
        manager.addToTestSuite("Smoke Tests", "TC_001");
        manager.addToTestSuite("Smoke Tests", "TC_003");
        manager.addToTestSuite("Regression Tests", "TC_001");
        manager.addToTestSuite("Regression Tests", "TC_002");
        manager.addToTestSuite("Regression Tests", "TC_004");
        manager.addToTestSuite("Regression Tests", "TC_005");
        manager.displayTestSuites();

        // Execution history
        manager.addExecutionHistory("2024-01-15 10:30", "TC_001", "PASSED", 5);
        manager.addExecutionHistory("2024-01-15 10:31", "TC_002", "PASSED", 4);
        manager.addExecutionHistory("2024-01-15 10:32", "TC_003", "FAILED", 8);
        manager.displayExecutionHistory();

        System.out.println("\n========================================");
        System.out.println("Collections Demo Completed!");
        System.out.println("========================================");
    }
}
```

### Deliverables
- TestDataManager.java with ArrayList and HashMap implementation
- TestCase class for collection storage
- Demonstration of all major collection operations
- Performance comparison document

### Extension Challenges
1. Implement TreeSet for automatically sorted test cases
2. Use LinkedHashMap to maintain insertion order
3. Create a test data cache using HashMap
4. Implement concurrent collections for parallel test execution

---

## Exercise 13: File I/O - Reading Test Data from Files

### Objective
Master file input/output operations by creating a system to read and write test data, test results, and configuration files.

### Scenario
Create a file-based test data management system that reads test cases from CSV files, writes test results to files, and manages configuration through properties files.

### Step-by-Step Instructions

1. **Create TestDataFileManager.java with:**
   - Read test data from CSV files
   - Write test results to text files
   - Read/write properties files
   - Handle file exceptions
   - Append to existing files

2. **Create sample data files:**
   - testcases.csv (test case data)
   - config.properties (configuration)
   - results.txt (test results output)

### Code Template

```java
import java.io.*;
import java.util.*;

public class TestDataFileManager {

    // TODO: Method to read CSV file

    // TODO: Method to write results to file

    // TODO: Method to read/write properties

    public static void main(String[] args) {
        // TODO: Demonstrate file operations
    }
}
```

### Expected Output

```
========================================
    FILE I/O DEMONSTRATION
    Test Data Management
========================================

--- Reading Test Cases from CSV ---
Reading file: testcases.csv
Loaded 5 test cases from file

Test Cases:
1. TC_001 | Login Valid | P0 | Authentication | PASSED
2. TC_002 | Login Invalid | P1 | Authentication | FAILED
3. TC_003 | Registration | P0 | User Management | PASSED
4. TC_004 | Password Reset | P1 | User Management | PASSED
5. TC_005 | Profile Update | P2 | User Management | NOT_RUN

--- Writing Test Results to File ---
Writing results to: test_results.txt
Test results written successfully
File size: 542 bytes

--- Reading Configuration from Properties File ---
Reading configuration from: config.properties
Configuration loaded:
  browser = Chrome
  environment = QA
  timeout = 30
  baseUrl = https://test.example.com
  retryCount = 3

--- Writing Test Report ---
Creating detailed test report: detailed_report.txt
Report created successfully

--- Appending to Execution Log ---
Appending to: execution_log.txt
Log entry added: 2024-01-15 14:30:00 - Test Suite Executed

--- Reading Test Report ---
Contents of test_results.txt:
========================================
        TEST EXECUTION REPORT
========================================
Execution Date: 2024-01-15 14:30:00
Test Suite: Regression Suite
Total Tests: 5
Passed: 3
Failed: 1
Not Run: 1
Pass Rate: 60.0%
========================================

--- Creating CSV Report ---
Writing CSV report to: test_summary.csv
CSV report created successfully

--- Reading Binary Test Data ---
Reading serialized test data...
Test data deserialized successfully
Loaded 3 test data sets

--- File Validation ---
Checking file existence:
  testcases.csv: EXISTS
  config.properties: EXISTS
  test_results.txt: EXISTS
  missing_file.txt: NOT FOUND

========================================
    File Operations Completed!
========================================
Total files processed: 6
Total bytes read: 1234
Total bytes written: 890
========================================
```

### Solution Code

```java
import java.io.*;
import java.util.*;
import java.text.SimpleDateFormat;

// TestCase class for file operations
class TestCaseData {
    String id;
    String name;
    String priority;
    String module;
    String status;

    public TestCaseData(String id, String name, String priority, String module, String status) {
        this.id = id;
        this.name = name;
        this.priority = priority;
        this.module = module;
        this.status = status;
    }

    public String toCSV() {
        return id + "," + name + "," + priority + "," + module + "," + status;
    }

    @Override
    public String toString() {
        return id + " | " + name + " | " + priority + " | " + module + " | " + status;
    }
}

public class TestDataFileManager {

    // Read test cases from CSV file
    public static ArrayList<TestCaseData> readTestCasesFromCSV(String filename) {
        ArrayList<TestCaseData> testCases = new ArrayList<>();

        System.out.println("Reading file: " + filename);

        BufferedReader reader = null;
        try {
            reader = new BufferedReader(new FileReader(filename));
            String line;
            boolean isFirstLine = true;

            while ((line = reader.readLine()) != null) {
                // Skip header line
                if (isFirstLine) {
                    isFirstLine = false;
                    continue;
                }

                // Split CSV line
                String[] parts = line.split(",");
                if (parts.length >= 5) {
                    TestCaseData testCase = new TestCaseData(
                        parts[0].trim(),
                        parts[1].trim(),
                        parts[2].trim(),
                        parts[3].trim(),
                        parts[4].trim()
                    );
                    testCases.add(testCase);
                }
            }

            System.out.println("Loaded " + testCases.size() + " test cases from file");

        } catch (FileNotFoundException e) {
            System.out.println("Error: File not found - " + filename);
        } catch (IOException e) {
            System.out.println("Error reading file: " + e.getMessage());
        } finally {
            try {
                if (reader != null) {
                    reader.close();
                }
            } catch (IOException e) {
                System.out.println("Error closing file: " + e.getMessage());
            }
        }

        return testCases;
    }

    // Write test results to file
    public static void writeTestResults(String filename, ArrayList<TestCaseData> testCases) {
        System.out.println("Writing results to: " + filename);

        try (BufferedWriter writer = new BufferedWriter(new FileWriter(filename))) {

            // Write header
            writer.write("========================================\n");
            writer.write("        TEST EXECUTION REPORT\n");
            writer.write("========================================\n");

            // Write date
            SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
            writer.write("Execution Date: " + sdf.format(new Date()) + "\n");
            writer.write("Test Suite: Regression Suite\n");

            // Calculate statistics
            int total = testCases.size();
            int passed = 0, failed = 0, notRun = 0;

            for (TestCaseData tc : testCases) {
                switch (tc.status) {
                    case "PASSED": passed++; break;
                    case "FAILED": failed++; break;
                    case "NOT_RUN": notRun++; break;
                }
            }

            // Write statistics
            writer.write("Total Tests: " + total + "\n");
            writer.write("Passed: " + passed + "\n");
            writer.write("Failed: " + failed + "\n");
            writer.write("Not Run: " + notRun + "\n");
            writer.write("Pass Rate: " + String.format("%.1f", passed * 100.0 / total) + "%\n");
            writer.write("========================================\n");

            System.out.println("Test results written successfully");

            // Get file size
            File file = new File(filename);
            System.out.println("File size: " + file.length() + " bytes");

        } catch (IOException e) {
            System.out.println("Error writing file: " + e.getMessage());
        }
    }

    // Write detailed test report
    public static void writeDetailedReport(String filename, ArrayList<TestCaseData> testCases) {
        System.out.println("Creating detailed test report: " + filename);

        try (PrintWriter writer = new PrintWriter(new FileWriter(filename))) {

            writer.println("========================================");
            writer.println("    DETAILED TEST EXECUTION REPORT");
            writer.println("========================================");
            writer.println();

            // Group by module
            HashMap<String, ArrayList<TestCaseData>> byModule = new HashMap<>();
            for (TestCaseData tc : testCases) {
                if (!byModule.containsKey(tc.module)) {
                    byModule.put(tc.module, new ArrayList<>());
                }
                byModule.get(tc.module).add(tc);
            }

            // Write grouped results
            for (Map.Entry<String, ArrayList<TestCaseData>> entry : byModule.entrySet()) {
                writer.println("Module: " + entry.getKey());
                writer.println("----------------------------------------");

                for (TestCaseData tc : entry.getValue()) {
                    writer.printf("  %-15s %-30s [%s] %s%n",
                                 tc.id, tc.name, tc.priority, tc.status);
                }
                writer.println();
            }

            writer.println("========================================");
            System.out.println("Report created successfully");

        } catch (IOException e) {
            System.out.println("Error writing detailed report: " + e.getMessage());
        }
    }

    // Read configuration from properties file
    public static Properties readConfiguration(String filename) {
        Properties properties = new Properties();

        System.out.println("Reading configuration from: " + filename);

        try (FileInputStream fis = new FileInputStream(filename)) {
            properties.load(fis);
            System.out.println("Configuration loaded:");

            for (String key : properties.stringPropertyNames()) {
                System.out.println("  " + key + " = " + properties.getProperty(key));
            }

        } catch (FileNotFoundException e) {
            System.out.println("Configuration file not found: " + filename);
        } catch (IOException e) {
            System.out.println("Error reading configuration: " + e.getMessage());
        }

        return properties;
    }

    // Write configuration to properties file
    public static void writeConfiguration(String filename, Properties properties) {
        try (FileOutputStream fos = new FileOutputStream(filename)) {
            properties.store(fos, "Test Configuration");
            System.out.println("Configuration saved to: " + filename);
        } catch (IOException e) {
            System.out.println("Error writing configuration: " + e.getMessage());
        }
    }

    // Append to log file
    public static void appendToLog(String filename, String message) {
        System.out.println("Appending to: " + filename);

        try (FileWriter fw = new FileWriter(filename, true);
             BufferedWriter bw = new BufferedWriter(fw);
             PrintWriter pw = new PrintWriter(bw)) {

            SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
            pw.println(sdf.format(new Date()) + " - " + message);

            System.out.println("Log entry added: " + sdf.format(new Date()) + " - " + message);

        } catch (IOException e) {
            System.out.println("Error appending to log: " + e.getMessage());
        }
    }

    // Read file contents
    public static void readAndDisplayFile(String filename) {
        System.out.println("\nContents of " + filename + ":");

        try (BufferedReader reader = new BufferedReader(new FileReader(filename))) {
            String line;
            while ((line = reader.readLine()) != null) {
                System.out.println(line);
            }
        } catch (FileNotFoundException e) {
            System.out.println("File not found: " + filename);
        } catch (IOException e) {
            System.out.println("Error reading file: " + e.getMessage());
        }
    }

    // Write CSV report
    public static void writeCSVReport(String filename, ArrayList<TestCaseData> testCases) {
        System.out.println("Writing CSV report to: " + filename);

        try (PrintWriter writer = new PrintWriter(new FileWriter(filename))) {

            // Write header
            writer.println("Test ID,Test Name,Priority,Module,Status");

            // Write data
            for (TestCaseData tc : testCases) {
                writer.println(tc.toCSV());
            }

            System.out.println("CSV report created successfully");

        } catch (IOException e) {
            System.out.println("Error writing CSV report: " + e.getMessage());
        }
    }

    // Check if file exists
    public static boolean fileExists(String filename) {
        File file = new File(filename);
        return file.exists();
    }

    // Get file information
    public static void displayFileInfo(String filename) {
        File file = new File(filename);

        if (file.exists()) {
            System.out.println("File: " + filename);
            System.out.println("  Size: " + file.length() + " bytes");
            System.out.println("  Last Modified: " + new Date(file.lastModified()));
            System.out.println("  Can Read: " + file.canRead());
            System.out.println("  Can Write: " + file.canWrite());
        } else {
            System.out.println("File does not exist: " + filename);
        }
    }

    // Create sample CSV file
    public static void createSampleCSV(String filename) {
        try (PrintWriter writer = new PrintWriter(new FileWriter(filename))) {
            writer.println("Test ID,Test Name,Priority,Module,Status");
            writer.println("TC_001,Login Valid,P0,Authentication,PASSED");
            writer.println("TC_002,Login Invalid,P1,Authentication,FAILED");
            writer.println("TC_003,Registration,P0,User Management,PASSED");
            writer.println("TC_004,Password Reset,P1,User Management,PASSED");
            writer.println("TC_005,Profile Update,P2,User Management,NOT_RUN");
        } catch (IOException e) {
            System.out.println("Error creating sample CSV: " + e.getMessage());
        }
    }

    // Create sample properties file
    public static void createSampleProperties(String filename) {
        Properties props = new Properties();
        props.setProperty("browser", "Chrome");
        props.setProperty("environment", "QA");
        props.setProperty("timeout", "30");
        props.setProperty("baseUrl", "https://test.example.com");
        props.setProperty("retryCount", "3");

        writeConfiguration(filename, props);
    }

    public static void main(String[] args) {
        System.out.println("========================================");
        System.out.println("    FILE I/O DEMONSTRATION");
        System.out.println("    Test Data Management");
        System.out.println("========================================");

        // Create sample files
        String csvFile = "testcases.csv";
        String propsFile = "config.properties";
        String resultsFile = "test_results.txt";
        String detailedReportFile = "detailed_report.txt";
        String logFile = "execution_log.txt";
        String csvReportFile = "test_summary.csv";

        createSampleCSV(csvFile);
        createSampleProperties(propsFile);

        // Read test cases from CSV
        System.out.println("\n--- Reading Test Cases from CSV ---");
        ArrayList<TestCaseData> testCases = readTestCasesFromCSV(csvFile);

        System.out.println("\nTest Cases:");
        for (int i = 0; i < testCases.size(); i++) {
            System.out.println((i + 1) + ". " + testCases.get(i).toString());
        }

        // Write test results
        System.out.println("\n--- Writing Test Results to File ---");
        writeTestResults(resultsFile, testCases);

        // Read configuration
        System.out.println("\n--- Reading Configuration from Properties File ---");
        Properties config = readConfiguration(propsFile);

        // Write detailed report
        System.out.println("\n--- Writing Test Report ---");
        writeDetailedReport(detailedReportFile, testCases);

        // Append to log
        System.out.println("\n--- Appending to Execution Log ---");
        appendToLog(logFile, "Test Suite Executed");
        appendToLog(logFile, "Total Tests: " + testCases.size());

        // Read and display results file
        System.out.println("\n--- Reading Test Report ---");
        readAndDisplayFile(resultsFile);

        // Create CSV report
        System.out.println("\n--- Creating CSV Report ---");
        writeCSVReport(csvReportFile, testCases);

        // File validation
        System.out.println("\n--- File Validation ---");
        System.out.println("Checking file existence:");
        String[] filesToCheck = {csvFile, propsFile, resultsFile, "missing_file.txt"};

        for (String file : filesToCheck) {
            System.out.println("  " + file + ": " + (fileExists(file) ? "EXISTS" : "NOT FOUND"));
        }

        // Summary
        System.out.println("\n========================================");
        System.out.println("    File Operations Completed!");
        System.out.println("========================================");
        System.out.println("Total files processed: 6");

        // Calculate total bytes
        long totalBytesRead = 0;
        long totalBytesWritten = 0;

        File[] files = {new File(csvFile), new File(resultsFile), new File(detailedReportFile)};
        for (File f : files) {
            if (f.exists()) {
                totalBytesWritten += f.length();
            }
        }

        System.out.println("Total bytes written: " + totalBytesWritten);
        System.out.println("========================================");
    }
}
```

### Deliverables
- TestDataFileManager.java with file I/O operations
- Sample CSV and properties files
- Generated test report files
- Error handling documentation

### Extension Challenges
1. Implement Excel file reading using Apache POI
2. Add JSON file support for test data
3. Implement file encryption for sensitive test data
4. Create file watcher to auto-load test data changes

---

## Exercise 14: String Manipulation and Regular Expressions

### Objective
Master string manipulation and regular expressions for test data validation, log parsing, and response verification.

### Scenario
Create utilities for validating test inputs, extracting data from logs and API responses, and performing string operations commonly needed in test automation.

### Step-by-Step Instructions

1. **Create StringTestUtilities.java with:**
   - String validation methods
   - Regular expression patterns for common validations
   - Log parsing utilities
   - Response data extraction
   - String formatting for test data

2. **Implement validations for:**
   - Email addresses
   - Phone numbers
   - URLs
   - Dates
   - Credit card numbers (test data)

### Code Template

```java
import java.util.regex.*;
import java.util.*;

public class StringTestUtilities {

    // TODO: Email validation using regex

    // TODO: Phone number validation

    // TODO: Extract data from strings using regex

    // TODO: Parse log files

    public static void main(String[] args) {
        // TODO: Demonstrate string operations
    }
}
```

### Expected Output

```
========================================
    STRING MANIPULATION & REGEX
    Test Data Validation Utilities
========================================

--- Email Validation ---
test@example.com: VALID
invalid-email: INVALID
user.name@domain.co.uk: VALID
@nodomain.com: INVALID

--- Phone Number Validation ---
+1-234-567-8900: VALID
(555) 123-4567: VALID
123456: INVALID
+44 20 1234 5678: VALID

--- URL Validation ---
https://www.example.com: VALID
http://localhost:8080/api: VALID
ftp://files.example.com: VALID
not a url: INVALID

--- Password Strength Validation ---
Pass@123: STRONG (Has uppercase, lowercase, digit, special char, length >= 8)
password: WEAK (Missing uppercase, digit, special char)
Pass123: MEDIUM (Missing special char)
P@ss: WEAK (Too short)

--- Credit Card Validation (Test Data) ---
4532-1234-5678-9010: VALID VISA
5425-2334-3010-9903: VALID MASTERCARD
1234-5678-9012-3456: INVALID

--- Date Format Validation ---
2024-01-15: VALID (yyyy-MM-dd)
01/15/2024: VALID (MM/dd/yyyy)
15-Jan-2024: VALID (dd-MMM-yyyy)
2024/13/45: INVALID

--- Extracting Data from API Response ---
API Response:
{"status":"success","userId":"12345","userName":"testUser","email":"test@example.com"}

Extracted Data:
  Status: success
  User ID: 12345
  User Name: testUser
  Email: test@example.com

--- Parsing Log File ---
Log Entry:
[2024-01-15 14:30:25] ERROR - Login failed for user: testuser@example.com - Reason: Invalid credentials

Parsed Log Data:
  Timestamp: 2024-01-15 14:30:25
  Level: ERROR
  Message: Login failed for user: testuser@example.com
  User: testuser@example.com
  Reason: Invalid credentials

--- String Sanitization for Test Data ---
Original: "  Test Data 123!@#  "
Trimmed: "Test Data 123!@#"
Alphanumeric Only: "TestData123"
Remove Special Chars: "Test Data 123"

--- Extracting Test Case IDs from Text ---
Text: "Execute TC_001, TC_002, and TC_003 for smoke testing"
Found Test Cases: [TC_001, TC_002, TC_003]

--- SQL Injection Detection ---
SELECT * FROM users: SAFE
SELECT * FROM users; DROP TABLE users;--: DANGEROUS
username' OR '1'='1: DANGEROUS
normal_username: SAFE

--- Masking Sensitive Data ---
Original Credit Card: 4532-1234-5678-9010
Masked: ****-****-****-9010

Original Email: testuser@example.com
Masked: tes****@example.com

--- String Pattern Matching ---
Finding all error codes in text:
Text: "Errors occurred: ERR_001, ERR_042, ERR_103"
Error Codes Found: [ERR_001, ERR_042, ERR_103]

--- Test Data Generation ---
Generated Email: user_test_12345@example.com
Generated Username: test_user_67890
Generated Phone: +1-555-123-4567

========================================
    String Utilities Demo Completed!
========================================
Total validations performed: 25
Valid inputs: 18
Invalid inputs: 7
========================================
```

### Solution Code

```java
import java.util.regex.*;
import java.util.*;

public class StringTestUtilities {

    // Regular Expression Patterns
    private static final String EMAIL_PATTERN =
        "^[A-Za-z0-9+_.-]+@[A-Za-z0-9.-]+\\.[A-Za-z]{2,}$";

    private static final String PHONE_PATTERN =
        "^[+]?[(]?[0-9]{1,4}[)]?[-\\s.]?[(]?[0-9]{1,4}[)]?[-\\s.]?[0-9]{1,4}[-\\s.]?[0-9]{1,9}$";

    private static final String URL_PATTERN =
        "^(https?|ftp)://[a-zA-Z0-9.-]+(:[0-9]+)?(/.*)?$";

    private static final String DATE_PATTERN_1 = "^\\d{4}-\\d{2}-\\d{2}$"; // yyyy-MM-dd
    private static final String DATE_PATTERN_2 = "^\\d{2}/\\d{2}/\\d{4}$"; // MM/dd/yyyy
    private static final String DATE_PATTERN_3 = "^\\d{2}-[A-Za-z]{3}-\\d{4}$"; // dd-MMM-yyyy

    private static final String VISA_PATTERN = "^4[0-9]{12}(?:[0-9]{3})?$";
    private static final String MASTERCARD_PATTERN = "^5[1-5][0-9]{14}$";

    private static final String TEST_CASE_ID_PATTERN = "TC_\\d{3}";

    // Email Validation
    public static boolean isValidEmail(String email) {
        Pattern pattern = Pattern.compile(EMAIL_PATTERN);
        Matcher matcher = pattern.matcher(email);
        return matcher.matches();
    }

    // Phone Number Validation
    public static boolean isValidPhone(String phone) {
        Pattern pattern = Pattern.compile(PHONE_PATTERN);
        Matcher matcher = pattern.matcher(phone);
        return matcher.matches();
    }

    // URL Validation
    public static boolean isValidURL(String url) {
        Pattern pattern = Pattern.compile(URL_PATTERN);
        Matcher matcher = pattern.matcher(url);
        return matcher.matches();
    }

    // Password Strength Validation
    public static String validatePasswordStrength(String password) {
        int strength = 0;
        String feedback = "";

        if (password.length() >= 8) strength++;
        else feedback += "Too short, ";

        if (password.matches(".*[A-Z].*")) strength++;
        else feedback += "Missing uppercase, ";

        if (password.matches(".*[a-z].*")) strength++;
        else feedback += "Missing lowercase, ";

        if (password.matches(".*\\d.*")) strength++;
        else feedback += "Missing digit, ";

        if (password.matches(".*[!@#$%^&*(),.?\":{}|<>].*")) strength++;
        else feedback += "Missing special char, ";

        if (strength >= 5) {
            return "STRONG (Has uppercase, lowercase, digit, special char, length >= 8)";
        } else if (strength >= 3) {
            return "MEDIUM (" + feedback.replaceAll(", $", "") + ")";
        } else {
            return "WEAK (" + feedback.replaceAll(", $", "") + ")";
        }
    }

    // Credit Card Validation (for test data)
    public static String validateCreditCard(String cardNumber) {
        // Remove spaces and hyphens
        String cleanNumber = cardNumber.replaceAll("[\\s-]", "");

        if (cleanNumber.matches(VISA_PATTERN)) {
            return "VALID VISA";
        } else if (cleanNumber.matches(MASTERCARD_PATTERN)) {
            return "VALID MASTERCARD";
        } else {
            return "INVALID";
        }
    }

    // Date Format Validation
    public static boolean isValidDate(String date) {
        return date.matches(DATE_PATTERN_1) ||
               date.matches(DATE_PATTERN_2) ||
               date.matches(DATE_PATTERN_3);
    }

    // Extract data from API JSON response using regex
    public static String extractFromJSON(String json, String key) {
        Pattern pattern = Pattern.compile("\"" + key + "\"\\s*:\\s*\"([^\"]+)\"");
        Matcher matcher = pattern.matcher(json);

        if (matcher.find()) {
            return matcher.group(1);
        }
        return null;
    }

    // Parse log entry
    public static HashMap<String, String> parseLogEntry(String logEntry) {
        HashMap<String, String> logData = new HashMap<>();

        // Extract timestamp
        Pattern timestampPattern = Pattern.compile("\\[(\\d{4}-\\d{2}-\\d{2} \\d{2}:\\d{2}:\\d{2})\\]");
        Matcher timestampMatcher = timestampPattern.matcher(logEntry);
        if (timestampMatcher.find()) {
            logData.put("timestamp", timestampMatcher.group(1));
        }

        // Extract log level
        Pattern levelPattern = Pattern.compile("\\] (ERROR|INFO|WARN|DEBUG) -");
        Matcher levelMatcher = levelPattern.matcher(logEntry);
        if (levelMatcher.find()) {
            logData.put("level", levelMatcher.group(1));
        }

        // Extract email
        Pattern emailPattern = Pattern.compile(EMAIL_PATTERN);
        Matcher emailMatcher = emailPattern.matcher(logEntry);
        if (emailMatcher.find()) {
            logData.put("user", emailMatcher.group());
        }

        // Extract message
        Pattern messagePattern = Pattern.compile("\\] (?:ERROR|INFO|WARN|DEBUG) - (.+)");
        Matcher messageMatcher = messagePattern.matcher(logEntry);
        if (messageMatcher.find()) {
            logData.put("message", messageMatcher.group(1).split(" - Reason:")[0]);
        }

        // Extract reason
        Pattern reasonPattern = Pattern.compile("Reason: (.+)$");
        Matcher reasonMatcher = reasonPattern.matcher(logEntry);
        if (reasonMatcher.find()) {
            logData.put("reason", reasonMatcher.group(1));
        }

        return logData;
    }

    // String Sanitization
    public static String removeSpecialChars(String input) {
        return input.replaceAll("[^a-zA-Z0-9\\s]", "");
    }

    public static String getAlphanumericOnly(String input) {
        return input.replaceAll("[^a-zA-Z0-9]", "");
    }

    // Extract all test case IDs from text
    public static ArrayList<String> extractTestCaseIds(String text) {
        ArrayList<String> testCases = new ArrayList<>();
        Pattern pattern = Pattern.compile(TEST_CASE_ID_PATTERN);
        Matcher matcher = pattern.matcher(text);

        while (matcher.find()) {
            testCases.add(matcher.group());
        }

        return testCases;
    }

    // SQL Injection Detection
    public static boolean isSQLInjectionAttempt(String input) {
        String[] dangerousPatterns = {
            ".*;.*",
            ".*--.*",
            ".*'.*OR.*'.*=.*'.*",
            ".*DROP.*TABLE.*",
            ".*INSERT.*INTO.*",
            ".*DELETE.*FROM.*"
        };

        for (String pattern : dangerousPatterns) {
            if (input.toUpperCase().matches(pattern.toUpperCase())) {
                return true;
            }
        }
        return false;
    }

    // Mask sensitive data
    public static String maskCreditCard(String cardNumber) {
        if (cardNumber.length() < 4) return cardNumber;
        String last4 = cardNumber.substring(cardNumber.length() - 4);
        return "****-****-****-" + last4;
    }

    public static String maskEmail(String email) {
        int atIndex = email.indexOf('@');
        if (atIndex <= 3) return email;

        String username = email.substring(0, atIndex);
        String domain = email.substring(atIndex);
        String maskedUsername = username.substring(0, 3) + "****";

        return maskedUsername + domain;
    }

    // Extract all matches using regex
    public static ArrayList<String> extractPattern(String text, String pattern) {
        ArrayList<String> matches = new ArrayList<>();
        Pattern p = Pattern.compile(pattern);
        Matcher m = p.matcher(text);

        while (m.find()) {
            matches.add(m.group());
        }

        return matches;
    }

    // Generate test data
    public static String generateTestEmail() {
        return "user_test_" + (int)(Math.random() * 100000) + "@example.com";
    }

    public static String generateTestUsername() {
        return "test_user_" + (int)(Math.random() * 100000);
    }

    public static String generateTestPhone() {
        return "+1-555-" + String.format("%03d", (int)(Math.random() * 1000)) + "-" +
               String.format("%04d", (int)(Math.random() * 10000));
    }

    public static void main(String[] args) {
        System.out.println("========================================");
        System.out.println("    STRING MANIPULATION & REGEX");
        System.out.println("    Test Data Validation Utilities");
        System.out.println("========================================");

        int totalValidations = 0;
        int validCount = 0;
        int invalidCount = 0;

        // Email Validation
        System.out.println("\n--- Email Validation ---");
        String[] emails = {"test@example.com", "invalid-email", "user.name@domain.co.uk", "@nodomain.com"};
        for (String email : emails) {
            boolean valid = isValidEmail(email);
            System.out.println(email + ": " + (valid ? "VALID" : "INVALID"));
            totalValidations++;
            if (valid) validCount++; else invalidCount++;
        }

        // Phone Validation
        System.out.println("\n--- Phone Number Validation ---");
        String[] phones = {"+1-234-567-8900", "(555) 123-4567", "123456", "+44 20 1234 5678"};
        for (String phone : phones) {
            boolean valid = isValidPhone(phone);
            System.out.println(phone + ": " + (valid ? "VALID" : "INVALID"));
            totalValidations++;
            if (valid) validCount++; else invalidCount++;
        }

        // URL Validation
        System.out.println("\n--- URL Validation ---");
        String[] urls = {"https://www.example.com", "http://localhost:8080/api",
                        "ftp://files.example.com", "not a url"};
        for (String url : urls) {
            boolean valid = isValidURL(url);
            System.out.println(url + ": " + (valid ? "VALID" : "INVALID"));
            totalValidations++;
            if (valid) validCount++; else invalidCount++;
        }

        // Password Strength
        System.out.println("\n--- Password Strength Validation ---");
        String[] passwords = {"Pass@123", "password", "Pass123", "P@ss"};
        for (String pwd : passwords) {
            System.out.println(pwd + ": " + validatePasswordStrength(pwd));
        }

        // Credit Card Validation
        System.out.println("\n--- Credit Card Validation (Test Data) ---");
        String[] cards = {"4532-1234-5678-9010", "5425-2334-3010-9903", "1234-5678-9012-3456"};
        for (String card : cards) {
            System.out.println(card + ": " + validateCreditCard(card));
            totalValidations++;
        }

        // Date Validation
        System.out.println("\n--- Date Format Validation ---");
        String[] dates = {"2024-01-15", "01/15/2024", "15-Jan-2024", "2024/13/45"};
        for (String date : dates) {
            boolean valid = isValidDate(date);
            System.out.println(date + ": " + (valid ? "VALID" : "INVALID"));
            totalValidations++;
            if (valid) validCount++; else invalidCount++;
        }

        // Extract from JSON
        System.out.println("\n--- Extracting Data from API Response ---");
        String jsonResponse = "{\"status\":\"success\",\"userId\":\"12345\"," +
                            "\"userName\":\"testUser\",\"email\":\"test@example.com\"}";
        System.out.println("API Response:");
        System.out.println(jsonResponse);
        System.out.println("\nExtracted Data:");
        System.out.println("  Status: " + extractFromJSON(jsonResponse, "status"));
        System.out.println("  User ID: " + extractFromJSON(jsonResponse, "userId"));
        System.out.println("  User Name: " + extractFromJSON(jsonResponse, "userName"));
        System.out.println("  Email: " + extractFromJSON(jsonResponse, "email"));

        // Parse Log
        System.out.println("\n--- Parsing Log File ---");
        String logEntry = "[2024-01-15 14:30:25] ERROR - Login failed for user: " +
                         "testuser@example.com - Reason: Invalid credentials";
        System.out.println("Log Entry:");
        System.out.println(logEntry);
        System.out.println("\nParsed Log Data:");
        HashMap<String, String> logData = parseLogEntry(logEntry);
        System.out.println("  Timestamp: " + logData.get("timestamp"));
        System.out.println("  Level: " + logData.get("level"));
        System.out.println("  Message: " + logData.get("message"));
        System.out.println("  User: " + logData.get("user"));
        System.out.println("  Reason: " + logData.get("reason"));

        // String Sanitization
        System.out.println("\n--- String Sanitization for Test Data ---");
        String testData = "  Test Data 123!@#  ";
        System.out.println("Original: \"" + testData + "\"");
        System.out.println("Trimmed: \"" + testData.trim() + "\"");
        System.out.println("Alphanumeric Only: \"" + getAlphanumericOnly(testData) + "\"");
        System.out.println("Remove Special Chars: \"" + removeSpecialChars(testData.trim()) + "\"");

        // Extract Test Case IDs
        System.out.println("\n--- Extracting Test Case IDs from Text ---");
        String text = "Execute TC_001, TC_002, and TC_003 for smoke testing";
        System.out.println("Text: \"" + text + "\"");
        System.out.println("Found Test Cases: " + extractTestCaseIds(text));

        // SQL Injection Detection
        System.out.println("\n--- SQL Injection Detection ---");
        String[] inputs = {"SELECT * FROM users", "SELECT * FROM users; DROP TABLE users;--",
                          "username' OR '1'='1", "normal_username"};
        for (String input : inputs) {
            System.out.println(input + ": " + (isSQLInjectionAttempt(input) ? "DANGEROUS" : "SAFE"));
        }

        // Masking
        System.out.println("\n--- Masking Sensitive Data ---");
        String creditCard = "4532-1234-5678-9010";
        String email = "testuser@example.com";
        System.out.println("Original Credit Card: " + creditCard);
        System.out.println("Masked: " + maskCreditCard(creditCard));
        System.out.println("\nOriginal Email: " + email);
        System.out.println("Masked: " + maskEmail(email));

        // Pattern Matching
        System.out.println("\n--- String Pattern Matching ---");
        String errorText = "Errors occurred: ERR_001, ERR_042, ERR_103";
        System.out.println("Finding all error codes in text:");
        System.out.println("Text: \"" + errorText + "\"");
        System.out.println("Error Codes Found: " + extractPattern(errorText, "ERR_\\d{3}"));

        // Test Data Generation
        System.out.println("\n--- Test Data Generation ---");
        System.out.println("Generated Email: " + generateTestEmail());
        System.out.println("Generated Username: " + generateTestUsername());
        System.out.println("Generated Phone: " + generateTestPhone());

        // Summary
        System.out.println("\n========================================");
        System.out.println("    String Utilities Demo Completed!");
        System.out.println("========================================");
        System.out.println("Total validations performed: " + totalValidations);
        System.out.println("Valid inputs: " + validCount);
        System.out.println("Invalid inputs: " + invalidCount);
        System.out.println("========================================");
    }
}
```

### Deliverables
- StringTestUtilities.java with validation and parsing methods
- Regex pattern library for common validations
- Test data generator utilities
- Documentation of all regex patterns used

### Extension Challenges
1. Add XML parsing using regex
2. Implement HTML tag extraction and validation
3. Create log file analyzer with statistics
4. Build test data anonymizer using regex

---

## Exercise 15: Complete Mini-Project - Test Data Management Utility

### Objective
Apply all learned Java concepts to build a comprehensive Test Data Management Utility that handles test cases, test data, execution, reporting, and file operations.

### Scenario
Create a complete test automation support system that manages the entire test lifecycle: loading test cases from files, managing test data, executing tests, handling exceptions, generating reports, and providing a command-line interface.

### Project Requirements

Build a system with the following components:

1. **TestCase Model** - Class representing a test case
2. **TestDataManager** - Manages test cases and test data
3. **TestExecutor** - Executes tests with different strategies
4. **ReportGenerator** - Creates reports in multiple formats
5. **FileHandler** - Manages file I/O operations
6. **ValidationUtility** - Validates test data
7. **CommandLineInterface** - User interface for the system

### Step-by-Step Instructions

1. **Design Phase:**
   - Create class diagram
   - Define interfaces and abstract classes
   - Plan file structure

2. **Implementation Phase:**
   - Implement model classes
   - Create utility classes
   - Build executor framework
   - Develop reporting module
   - Create CLI interface

3. **Testing Phase:**
   - Test each component
   - Integration testing
   - Handle edge cases

### Project Structure

```
TestDataManagementUtility/
├── models/
│   ├── TestCase.java
│   ├── TestData.java
│   └── TestResult.java
├── managers/
│   ├── TestDataManager.java
│   └── TestExecutionManager.java
├── executors/
│   ├── TestExecutor.java (interface)
│   ├── UnitTestExecutor.java
│   └── IntegrationTestExecutor.java
├── reporters/
│   ├── ReportGenerator.java (interface)
│   ├── HTMLReporter.java
│   └── CSVReporter.java
├── utils/
│   ├── FileHandler.java
│   ├── ValidationUtility.java
│   └── StringUtility.java
├── exceptions/
│   └── TestManagementException.java
└── TestManagementApp.java (Main application)
```

### Expected Output

```
========================================
  TEST DATA MANAGEMENT UTILITY v1.0
========================================

Welcome to Test Data Management System!

--- Main Menu ---
1. Load Test Cases from File
2. Add New Test Case
3. View All Test Cases
4. Execute Tests
5. Generate Reports
6. Search Test Cases
7. Configuration
8. Exit

Enter your choice: 1

--- Load Test Cases ---
Enter file path: testcases.csv
Reading test cases from file...
Successfully loaded 10 test cases
Test cases added to system

Enter your choice: 3

--- View All Test Cases ---
========================================
Total Test Cases: 10
========================================

ID: TC_001
Name: Verify User Login
Module: Authentication
Priority: P0
Status: NOT_EXECUTED
Expected Result: User should be logged in successfully
----------------------------------------

ID: TC_002
Name: Verify Invalid Password
Module: Authentication
Priority: P1
Status: NOT_EXECUTED
Expected Result: Error message should be displayed
----------------------------------------

... (8 more test cases)

Enter your choice: 4

--- Execute Tests ---
Select execution mode:
1. Execute All Tests
2. Execute by Priority
3. Execute by Module
4. Execute Specific Test

Enter your choice: 1

Executing all tests...

[TestExecutor] Initializing test environment
[TestExecutor] Starting test execution batch
----------------------------------------
Executing: TC_001 - Verify User Login
  Setting up test data
  Running test steps
  Result: PASSED (Execution time: 3.2s)
----------------------------------------
Executing: TC_002 - Verify Invalid Password
  Setting up test data
  Running test steps
  Result: PASSED (Execution time: 2.1s)
----------------------------------------
Executing: TC_003 - Verify Registration
  Setting up test data
  Running test steps
  Error: Element not found
  Result: FAILED (Execution time: 5.4s)
  Screenshot saved: TC_003_failure.png
----------------------------------------
Executing: TC_004 - Verify Password Reset
  Setting up test data
  Running test steps
  Result: PASSED (Execution time: 4.0s)
----------------------------------------
... (6 more tests)

Test Execution Summary:
========================================
Total Tests Executed: 10
Passed: 8
Failed: 2
Skipped: 0
Execution Time: 45.3 seconds
Pass Rate: 80.0%
========================================

Enter your choice: 5

--- Generate Reports ---
Select report format:
1. HTML Report
2. CSV Report
3. JSON Report
4. All Formats

Enter your choice: 4

Generating reports in all formats...

[HTMLReporter] Creating HTML report
  Report saved: test_report_2024-01-15_14-30-00.html

[CSVReporter] Creating CSV report
  Report saved: test_report_2024-01-15_14-30-00.csv

[JSONReporter] Creating JSON report
  Report saved: test_report_2024-01-15_14-30-00.json

All reports generated successfully!

Enter your choice: 6

--- Search Test Cases ---
Search by:
1. Test ID
2. Module
3. Priority
4. Status

Enter your choice: 2

Enter module name: Authentication

Found 3 test case(s):
1. TC_001 - Verify User Login [P0] - PASSED
2. TC_002 - Verify Invalid Password [P1] - PASSED
3. TC_003 - Verify Logout [P2] - NOT_EXECUTED

Enter your choice: 7

--- Configuration ---
Current Configuration:
  Max Retry Attempts: 3
  Test Timeout: 30 seconds
  Screenshot on Failure: true
  Detailed Logging: true
  Parallel Execution: false

Modify configuration? (y/n): y

Select option to modify:
1. Max Retry Attempts
2. Test Timeout
3. Screenshot on Failure
4. Detailed Logging
5. Parallel Execution
6. Save and Exit

Enter your choice: 1

Enter new value for Max Retry Attempts (current: 3): 5
Configuration updated: Max Retry Attempts = 5

Configuration saved successfully!

Enter your choice: 8

========================================
Generating final execution summary...

SESSION SUMMARY:
================
Session Duration: 15 minutes 32 seconds
Test Cases Loaded: 10
Tests Executed: 10
Reports Generated: 3
Configuration Changes: 1

Thank you for using Test Data Management Utility!
========================================
```

### Solution Code

```java
// ============================================
// FILE: models/TestCase.java
// ============================================
package models;

public class TestCase {
    private String id;
    private String name;
    private String module;
    private String priority;
    private String status;
    private String expectedResult;
    private String actualResult;
    private int estimatedTime;
    private double actualTime;
    private String failureReason;

    public TestCase(String id, String name, String module, String priority) {
        this.id = id;
        this.name = name;
        this.module = module;
        this.priority = priority;
        this.status = "NOT_EXECUTED";
        this.actualTime = 0.0;
    }

    // Getters and setters
    public String getId() { return id; }
    public String getName() { return name; }
    public String getModule() { return module; }
    public String getPriority() { return priority; }
    public String getStatus() { return status; }
    public void setStatus(String status) { this.status = status; }
    public String getExpectedResult() { return expectedResult; }
    public void setExpectedResult(String expectedResult) { this.expectedResult = expectedResult; }
    public String getActualResult() { return actualResult; }
    public void setActualResult(String actualResult) { this.actualResult = actualResult; }
    public double getActualTime() { return actualTime; }
    public void setActualTime(double actualTime) { this.actualTime = actualTime; }
    public String getFailureReason() { return failureReason; }
    public void setFailureReason(String failureReason) { this.failureReason = failureReason; }

    @Override
    public String toString() {
        return String.format("ID: %s\nName: %s\nModule: %s\nPriority: %s\nStatus: %s",
                           id, name, module, priority, status);
    }

    public String toCSV() {
        return String.format("%s,%s,%s,%s,%s,%.2f",
                           id, name, module, priority, status, actualTime);
    }
}

// ============================================
// FILE: models/TestResult.java
// ============================================
package models;

public class TestResult {
    private String testId;
    private String status;
    private double executionTime;
    private String timestamp;
    private String failureReason;

    public TestResult(String testId, String status, double executionTime, String timestamp) {
        this.testId = testId;
        this.status = status;
        this.executionTime = executionTime;
        this.timestamp = timestamp;
    }

    // Getters and setters
    public String getTestId() { return testId; }
    public String getStatus() { return status; }
    public double getExecutionTime() { return executionTime; }
    public String getTimestamp() { return timestamp; }
    public String getFailureReason() { return failureReason; }
    public void setFailureReason(String failureReason) { this.failureReason = failureReason; }
}

// ============================================
// FILE: exceptions/TestManagementException.java
// ============================================
package exceptions;

public class TestManagementException extends Exception {
    private String errorCode;

    public TestManagementException(String message) {
        super(message);
    }

    public TestManagementException(String message, String errorCode) {
        super(message);
        this.errorCode = errorCode;
    }

    public TestManagementException(String message, Throwable cause) {
        super(message, cause);
    }

    public String getErrorCode() {
        return errorCode;
    }
}

// ============================================
// FILE: utils/FileHandler.java
// ============================================
package utils;

import models.TestCase;
import java.io.*;
import java.util.ArrayList;

public class FileHandler {

    public static ArrayList<TestCase> loadTestCasesFromCSV(String filename) throws IOException {
        ArrayList<TestCase> testCases = new ArrayList<>();

        try (BufferedReader reader = new BufferedReader(new FileReader(filename))) {
            String line;
            boolean isFirstLine = true;

            while ((line = reader.readLine()) != null) {
                if (isFirstLine) {
                    isFirstLine = false;
                    continue;
                }

                String[] parts = line.split(",");
                if (parts.length >= 4) {
                    TestCase testCase = new TestCase(
                        parts[0].trim(),
                        parts[1].trim(),
                        parts[2].trim(),
                        parts[3].trim()
                    );
                    if (parts.length >= 5) {
                        testCase.setExpectedResult(parts[4].trim());
                    }
                    testCases.add(testCase);
                }
            }
        }

        return testCases;
    }

    public static void saveReportToFile(String filename, String content) throws IOException {
        try (PrintWriter writer = new PrintWriter(new FileWriter(filename))) {
            writer.print(content);
        }
    }
}

// ============================================
// FILE: utils/ValidationUtility.java
// ============================================
package utils;

import java.util.regex.*;

public class ValidationUtility {

    public static boolean isValidTestId(String testId) {
        return testId.matches("TC_\\d{3}");
    }

    public static boolean isValidPriority(String priority) {
        return priority.matches("P[0-3]");
    }

    public static boolean isValidEmail(String email) {
        String emailPattern = "^[A-Za-z0-9+_.-]+@[A-Za-z0-9.-]+\\.[A-Za-z]{2,}$";
        return email.matches(emailPattern);
    }

    public static boolean isNullOrEmpty(String str) {
        return str == null || str.trim().isEmpty();
    }
}

// ============================================
// FILE: managers/TestDataManager.java
// ============================================
package managers;

import models.TestCase;
import models.TestResult;
import java.util.*;

public class TestDataManager {
    private ArrayList<TestCase> testCases;
    private HashMap<String, TestResult> testResults;
    private HashMap<String, String> configuration;

    public TestDataManager() {
        this.testCases = new ArrayList<>();
        this.testResults = new HashMap<>();
        this.configuration = new HashMap<>();
        initializeDefaultConfiguration();
    }

    private void initializeDefaultConfiguration() {
        configuration.put("maxRetry", "3");
        configuration.put("timeout", "30");
        configuration.put("screenshotOnFailure", "true");
        configuration.put("detailedLogging", "true");
        configuration.put("parallelExecution", "false");
    }

    public void addTestCase(TestCase testCase) {
        testCases.add(testCase);
    }

    public void addTestCases(ArrayList<TestCase> cases) {
        testCases.addAll(cases);
    }

    public ArrayList<TestCase> getAllTestCases() {
        return new ArrayList<>(testCases);
    }

    public TestCase getTestCaseById(String id) {
        for (TestCase tc : testCases) {
            if (tc.getId().equals(id)) {
                return tc;
            }
        }
        return null;
    }

    public ArrayList<TestCase> getTestCasesByModule(String module) {
        ArrayList<TestCase> result = new ArrayList<>();
        for (TestCase tc : testCases) {
            if (tc.getModule().equalsIgnoreCase(module)) {
                result.add(tc);
            }
        }
        return result;
    }

    public ArrayList<TestCase> getTestCasesByPriority(String priority) {
        ArrayList<TestCase> result = new ArrayList<>();
        for (TestCase tc : testCases) {
            if (tc.getPriority().equals(priority)) {
                result.add(tc);
            }
        }
        return result;
    }

    public ArrayList<TestCase> getTestCasesByStatus(String status) {
        ArrayList<TestCase> result = new ArrayList<>();
        for (TestCase tc : testCases) {
            if (tc.getStatus().equals(status)) {
                result.add(tc);
            }
        }
        return result;
    }

    public void addTestResult(String testId, TestResult result) {
        testResults.put(testId, result);
    }

    public HashMap<String, TestResult> getAllTestResults() {
        return new HashMap<>(testResults);
    }

    public void setConfiguration(String key, String value) {
        configuration.put(key, value);
    }

    public String getConfiguration(String key) {
        return configuration.get(key);
    }

    public HashMap<String, String> getAllConfiguration() {
        return new HashMap<>(configuration);
    }

    public int getTotalTests() {
        return testCases.size();
    }

    public int getPassedCount() {
        return (int) testCases.stream()
                              .filter(tc -> "PASSED".equals(tc.getStatus()))
                              .count();
    }

    public int getFailedCount() {
        return (int) testCases.stream()
                              .filter(tc -> "FAILED".equals(tc.getStatus()))
                              .count();
    }

    public int getNotExecutedCount() {
        return (int) testCases.stream()
                              .filter(tc -> "NOT_EXECUTED".equals(tc.getStatus()))
                              .count();
    }
}

// ============================================
// FILE: executors/TestExecutor.java (Interface)
// ============================================
package executors;

import models.TestCase;
import exceptions.TestManagementException;

public interface TestExecutor {
    void setup();
    void executeTest(TestCase testCase) throws TestManagementException;
    void teardown();
    String getExecutorType();
}

// ============================================
// FILE: executors/StandardTestExecutor.java
// ============================================
package executors;

import models.TestCase;
import models.TestResult;
import exceptions.TestManagementException;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.Random;

public class StandardTestExecutor implements TestExecutor {

    private Random random;

    public StandardTestExecutor() {
        this.random = new Random();
    }

    @Override
    public void setup() {
        System.out.println("[TestExecutor] Initializing test environment");
    }

    @Override
    public void executeTest(TestCase testCase) throws TestManagementException {
        System.out.println("----------------------------------------");
        System.out.println("Executing: " + testCase.getId() + " - " + testCase.getName());

        try {
            // Simulate test execution
            System.out.println("  Setting up test data");
            Thread.sleep(500);

            System.out.println("  Running test steps");
            Thread.sleep(1000);

            // Simulate random pass/fail (80% pass rate)
            double executionTime = 2.0 + random.nextDouble() * 4.0;
            boolean passed = random.nextDouble() < 0.8;

            testCase.setActualTime(executionTime);

            if (passed) {
                testCase.setStatus("PASSED");
                testCase.setActualResult("Test completed successfully");
                System.out.println("  Result: PASSED (Execution time: " +
                                 String.format("%.1fs", executionTime) + ")");
            } else {
                testCase.setStatus("FAILED");
                testCase.setFailureReason("Element not found");
                testCase.setActualResult("Test failed: Element not found");
                System.out.println("  Error: Element not found");
                System.out.println("  Result: FAILED (Execution time: " +
                                 String.format("%.1fs", executionTime) + ")");
                System.out.println("  Screenshot saved: " + testCase.getId() + "_failure.png");
            }

        } catch (InterruptedException e) {
            throw new TestManagementException("Test execution interrupted", e);
        }

        System.out.println("----------------------------------------");
    }

    @Override
    public void teardown() {
        System.out.println("[TestExecutor] Cleaning up test environment");
    }

    @Override
    public String getExecutorType() {
        return "Standard Test Executor";
    }
}

// ============================================
// FILE: reporters/ReportGenerator.java (Interface)
// ============================================
package reporters;

import managers.TestDataManager;
import java.io.IOException;

public interface ReportGenerator {
    String generateReport(TestDataManager manager);
    void saveReport(String filename, String content) throws IOException;
    String getReportFormat();
}

// ============================================
// FILE: reporters/HTMLReporter.java
// ============================================
package reporters;

import managers.TestDataManager;
import models.TestCase;
import utils.FileHandler;
import java.io.IOException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class HTMLReporter implements ReportGenerator {

    @Override
    public String generateReport(TestDataManager manager) {
        StringBuilder html = new StringBuilder();

        html.append("<!DOCTYPE html>\n");
        html.append("<html>\n<head>\n");
        html.append("<title>Test Execution Report</title>\n");
        html.append("<style>\n");
        html.append("body { font-family: Arial, sans-serif; margin: 20px; }\n");
        html.append("h1 { color: #333; }\n");
        html.append("table { border-collapse: collapse; width: 100%; }\n");
        html.append("th, td { border: 1px solid #ddd; padding: 8px; text-align: left; }\n");
        html.append("th { background-color: #4CAF50; color: white; }\n");
        html.append(".passed { color: green; font-weight: bold; }\n");
        html.append(".failed { color: red; font-weight: bold; }\n");
        html.append(".summary { background-color: #f2f2f2; padding: 15px; margin: 20px 0; }\n");
        html.append("</style>\n");
        html.append("</head>\n<body>\n");

        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        html.append("<h1>Test Execution Report</h1>\n");
        html.append("<p>Generated: ").append(sdf.format(new Date())).append("</p>\n");

        // Summary
        html.append("<div class='summary'>\n");
        html.append("<h2>Summary</h2>\n");
        html.append("<p>Total Tests: ").append(manager.getTotalTests()).append("</p>\n");
        html.append("<p class='passed'>Passed: ").append(manager.getPassedCount()).append("</p>\n");
        html.append("<p class='failed'>Failed: ").append(manager.getFailedCount()).append("</p>\n");
        double passRate = (manager.getPassedCount() * 100.0) / manager.getTotalTests();
        html.append("<p>Pass Rate: ").append(String.format("%.1f%%", passRate)).append("</p>\n");
        html.append("</div>\n");

        // Test cases table
        html.append("<h2>Test Cases</h2>\n");
        html.append("<table>\n");
        html.append("<tr><th>ID</th><th>Name</th><th>Module</th><th>Priority</th><th>Status</th><th>Time</th></tr>\n");

        for (TestCase tc : manager.getAllTestCases()) {
            String statusClass = tc.getStatus().equals("PASSED") ? "passed" : "failed";
            html.append("<tr>");
            html.append("<td>").append(tc.getId()).append("</td>");
            html.append("<td>").append(tc.getName()).append("</td>");
            html.append("<td>").append(tc.getModule()).append("</td>");
            html.append("<td>").append(tc.getPriority()).append("</td>");
            html.append("<td class='").append(statusClass).append("'>").append(tc.getStatus()).append("</td>");
            html.append("<td>").append(String.format("%.1fs", tc.getActualTime())).append("</td>");
            html.append("</tr>\n");
        }

        html.append("</table>\n");
        html.append("</body>\n</html>");

        return html.toString();
    }

    @Override
    public void saveReport(String filename, String content) throws IOException {
        FileHandler.saveReportToFile(filename, content);
        System.out.println("  Report saved: " + filename);
    }

    @Override
    public String getReportFormat() {
        return "HTML";
    }
}

// ============================================
// FILE: reporters/CSVReporter.java
// ============================================
package reporters;

import managers.TestDataManager;
import models.TestCase;
import utils.FileHandler;
import java.io.IOException;

public class CSVReporter implements ReportGenerator {

    @Override
    public String generateReport(TestDataManager manager) {
        StringBuilder csv = new StringBuilder();

        csv.append("Test ID,Test Name,Module,Priority,Status,Execution Time\n");

        for (TestCase tc : manager.getAllTestCases()) {
            csv.append(tc.toCSV()).append("\n");
        }

        return csv.toString();
    }

    @Override
    public void saveReport(String filename, String content) throws IOException {
        FileHandler.saveReportToFile(filename, content);
        System.out.println("  Report saved: " + filename);
    }

    @Override
    public String getReportFormat() {
        return "CSV";
    }
}

// ============================================
// FILE: TestManagementApp.java (Main Application)
// ============================================
import managers.TestDataManager;
import models.TestCase;
import executors.StandardTestExecutor;
import executors.TestExecutor;
import reporters.*;
import utils.FileHandler;
import exceptions.TestManagementException;

import java.io.IOException;
import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.Date;
import java.util.Scanner;

public class TestManagementApp {

    private TestDataManager manager;
    private Scanner scanner;
    private long sessionStartTime;

    public TestManagementApp() {
        this.manager = new TestDataManager();
        this.scanner = new Scanner(System.in);
        this.sessionStartTime = System.currentTimeMillis();
    }

    public void run() {
        System.out.println("========================================");
        System.out.println("  TEST DATA MANAGEMENT UTILITY v1.0");
        System.out.println("========================================");
        System.out.println("\nWelcome to Test Data Management System!\n");

        boolean running = true;
        while (running) {
            displayMainMenu();
            int choice = getUserChoice();

            switch (choice) {
                case 1:
                    loadTestCasesFromFile();
                    break;
                case 2:
                    addNewTestCase();
                    break;
                case 3:
                    viewAllTestCases();
                    break;
                case 4:
                    executeTests();
                    break;
                case 5:
                    generateReports();
                    break;
                case 6:
                    searchTestCases();
                    break;
                case 7:
                    manageConfiguration();
                    break;
                case 8:
                    running = false;
                    displaySessionSummary();
                    break;
                default:
                    System.out.println("Invalid choice. Please try again.");
            }
        }

        scanner.close();
    }

    private void displayMainMenu() {
        System.out.println("\n--- Main Menu ---");
        System.out.println("1. Load Test Cases from File");
        System.out.println("2. Add New Test Case");
        System.out.println("3. View All Test Cases");
        System.out.println("4. Execute Tests");
        System.out.println("5. Generate Reports");
        System.out.println("6. Search Test Cases");
        System.out.println("7. Configuration");
        System.out.println("8. Exit");
        System.out.print("\nEnter your choice: ");
    }

    private int getUserChoice() {
        try {
            return Integer.parseInt(scanner.nextLine());
        } catch (NumberFormatException e) {
            return -1;
        }
    }

    private void loadTestCasesFromFile() {
        System.out.println("\n--- Load Test Cases ---");
        System.out.print("Enter file path: ");
        String filename = scanner.nextLine();

        try {
            System.out.println("Reading test cases from file...");
            ArrayList<TestCase> testCases = FileHandler.loadTestCasesFromCSV(filename);
            manager.addTestCases(testCases);
            System.out.println("Successfully loaded " + testCases.size() + " test cases");
            System.out.println("Test cases added to system");
        } catch (IOException e) {
            System.out.println("Error loading file: " + e.getMessage());
        }
    }

    private void addNewTestCase() {
        System.out.println("\n--- Add New Test Case ---");
        System.out.print("Enter Test ID: ");
        String id = scanner.nextLine();
        System.out.print("Enter Test Name: ");
        String name = scanner.nextLine();
        System.out.print("Enter Module: ");
        String module = scanner.nextLine();
        System.out.print("Enter Priority (P0-P3): ");
        String priority = scanner.nextLine();

        TestCase testCase = new TestCase(id, name, module, priority);
        manager.addTestCase(testCase);
        System.out.println("Test case added successfully!");
    }

    private void viewAllTestCases() {
        System.out.println("\n--- View All Test Cases ---");
        System.out.println("========================================");
        System.out.println("Total Test Cases: " + manager.getTotalTests());
        System.out.println("========================================\n");

        ArrayList<TestCase> testCases = manager.getAllTestCases();
        if (testCases.isEmpty()) {
            System.out.println("No test cases found.");
        } else {
            for (TestCase tc : testCases) {
                System.out.println(tc.toString());
                if (tc.getExpectedResult() != null) {
                    System.out.println("Expected Result: " + tc.getExpectedResult());
                }
                System.out.println("----------------------------------------\n");
            }
        }
    }

    private void executeTests() {
        System.out.println("\n--- Execute Tests ---");
        System.out.println("Select execution mode:");
        System.out.println("1. Execute All Tests");
        System.out.println("2. Execute by Priority");
        System.out.println("3. Execute by Module");
        System.out.println("4. Execute Specific Test");
        System.out.print("\nEnter your choice: ");

        int choice = getUserChoice();
        ArrayList<TestCase> testsToExecute = new ArrayList<>();

        switch (choice) {
            case 1:
                testsToExecute = manager.getAllTestCases();
                System.out.println("\nExecuting all tests...\n");
                break;
            case 2:
                System.out.print("Enter priority (P0-P3): ");
                String priority = scanner.nextLine();
                testsToExecute = manager.getTestCasesByPriority(priority);
                break;
            case 3:
                System.out.print("Enter module name: ");
                String module = scanner.nextLine();
                testsToExecute = manager.getTestCasesByModule(module);
                break;
            case 4:
                System.out.print("Enter test ID: ");
                String testId = scanner.nextLine();
                TestCase tc = manager.getTestCaseById(testId);
                if (tc != null) {
                    testsToExecute.add(tc);
                }
                break;
        }

        if (testsToExecute.isEmpty()) {
            System.out.println("No tests to execute.");
            return;
        }

        executeTestBatch(testsToExecute);
    }

    private void executeTestBatch(ArrayList<TestCase> testCases) {
        TestExecutor executor = new StandardTestExecutor();
        executor.setup();

        long startTime = System.currentTimeMillis();
        System.out.println("[TestExecutor] Starting test execution batch");

        for (TestCase tc : testCases) {
            try {
                executor.executeTest(tc);
            } catch (TestManagementException e) {
                System.out.println("Error executing test: " + e.getMessage());
                tc.setStatus("ERROR");
                tc.setFailureReason(e.getMessage());
            }
        }

        long endTime = System.currentTimeMillis();
        double totalTime = (endTime - startTime) / 1000.0;

        executor.teardown();

        displayExecutionSummary(testCases, totalTime);
    }

    private void displayExecutionSummary(ArrayList<TestCase> testCases, double totalTime) {
        int total = testCases.size();
        int passed = 0, failed = 0, skipped = 0;

        for (TestCase tc : testCases) {
            switch (tc.getStatus()) {
                case "PASSED": passed++; break;
                case "FAILED": failed++; break;
                case "NOT_EXECUTED": skipped++; break;
            }
        }

        System.out.println("\nTest Execution Summary:");
        System.out.println("========================================");
        System.out.println("Total Tests Executed: " + total);
        System.out.println("Passed: " + passed);
        System.out.println("Failed: " + failed);
        System.out.println("Skipped: " + skipped);
        System.out.println("Execution Time: " + String.format("%.1f", totalTime) + " seconds");
        System.out.println("Pass Rate: " + String.format("%.1f%%", (passed * 100.0 / total)));
        System.out.println("========================================");
    }

    private void generateReports() {
        if (manager.getTotalTests() == 0) {
            System.out.println("No test data available for reporting.");
            return;
        }

        System.out.println("\n--- Generate Reports ---");
        System.out.println("Select report format:");
        System.out.println("1. HTML Report");
        System.out.println("2. CSV Report");
        System.out.println("3. All Formats");
        System.out.print("\nEnter your choice: ");

        int choice = getUserChoice();
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd_HH-mm-ss");
        String timestamp = sdf.format(new Date());

        System.out.println("\nGenerating reports...\n");

        try {
            switch (choice) {
                case 1:
                    generateHTMLReport(timestamp);
                    break;
                case 2:
                    generateCSVReport(timestamp);
                    break;
                case 3:
                    generateHTMLReport(timestamp);
                    generateCSVReport(timestamp);
                    break;
                default:
                    System.out.println("Invalid choice.");
                    return;
            }

            System.out.println("\nAll reports generated successfully!");

        } catch (IOException e) {
            System.out.println("Error generating reports: " + e.getMessage());
        }
    }

    private void generateHTMLReport(String timestamp) throws IOException {
        System.out.println("[HTMLReporter] Creating HTML report");
        ReportGenerator htmlReporter = new HTMLReporter();
        String htmlContent = htmlReporter.generateReport(manager);
        String htmlFilename = "test_report_" + timestamp + ".html";
        htmlReporter.saveReport(htmlFilename, htmlContent);
    }

    private void generateCSVReport(String timestamp) throws IOException {
        System.out.println("[CSVReporter] Creating CSV report");
        ReportGenerator csvReporter = new CSVReporter();
        String csvContent = csvReporter.generateReport(manager);
        String csvFilename = "test_report_" + timestamp + ".csv";
        csvReporter.saveReport(csvFilename, csvContent);
    }

    private void searchTestCases() {
        System.out.println("\n--- Search Test Cases ---");
        System.out.println("Search by:");
        System.out.println("1. Test ID");
        System.out.println("2. Module");
        System.out.println("3. Priority");
        System.out.println("4. Status");
        System.out.print("\nEnter your choice: ");

        int choice = getUserChoice();
        ArrayList<TestCase> results = new ArrayList<>();

        switch (choice) {
            case 1:
                System.out.print("Enter test ID: ");
                String testId = scanner.nextLine();
                TestCase tc = manager.getTestCaseById(testId);
                if (tc != null) results.add(tc);
                break;
            case 2:
                System.out.print("Enter module name: ");
                String module = scanner.nextLine();
                results = manager.getTestCasesByModule(module);
                break;
            case 3:
                System.out.print("Enter priority: ");
                String priority = scanner.nextLine();
                results = manager.getTestCasesByPriority(priority);
                break;
            case 4:
                System.out.print("Enter status: ");
                String status = scanner.nextLine();
                results = manager.getTestCasesByStatus(status);
                break;
        }

        System.out.println("\nFound " + results.size() + " test case(s):");
        for (int i = 0; i < results.size(); i++) {
            TestCase testCase = results.get(i);
            System.out.println((i + 1) + ". " + testCase.getId() + " - " +
                             testCase.getName() + " [" + testCase.getPriority() + "] - " +
                             testCase.getStatus());
        }
    }

    private void manageConfiguration() {
        System.out.println("\n--- Configuration ---");
        System.out.println("Current Configuration:");

        var config = manager.getAllConfiguration();
        System.out.println("  Max Retry Attempts: " + config.get("maxRetry"));
        System.out.println("  Test Timeout: " + config.get("timeout") + " seconds");
        System.out.println("  Screenshot on Failure: " + config.get("screenshotOnFailure"));
        System.out.println("  Detailed Logging: " + config.get("detailedLogging"));
        System.out.println("  Parallel Execution: " + config.get("parallelExecution"));

        System.out.print("\nModify configuration? (y/n): ");
        String response = scanner.nextLine();

        if (response.equalsIgnoreCase("y")) {
            System.out.println("\nSelect option to modify:");
            System.out.println("1. Max Retry Attempts");
            System.out.println("2. Test Timeout");
            System.out.print("\nEnter your choice: ");

            int choice = getUserChoice();
            switch (choice) {
                case 1:
                    System.out.print("Enter new value for Max Retry Attempts (current: " +
                                   config.get("maxRetry") + "): ");
                    String newRetry = scanner.nextLine();
                    manager.setConfiguration("maxRetry", newRetry);
                    System.out.println("Configuration updated: Max Retry Attempts = " + newRetry);
                    break;
                case 2:
                    System.out.print("Enter new value for Test Timeout (current: " +
                                   config.get("timeout") + "): ");
                    String newTimeout = scanner.nextLine();
                    manager.setConfiguration("timeout", newTimeout);
                    System.out.println("Configuration updated: Test Timeout = " + newTimeout);
                    break;
            }

            System.out.println("\nConfiguration saved successfully!");
        }
    }

    private void displaySessionSummary() {
        System.out.println("\n========================================");
        System.out.println("Generating final execution summary...\n");

        long sessionDuration = (System.currentTimeMillis() - sessionStartTime) / 1000;
        int minutes = (int) (sessionDuration / 60);
        int seconds = (int) (sessionDuration % 60);

        System.out.println("SESSION SUMMARY:");
        System.out.println("================");
        System.out.println("Session Duration: " + minutes + " minutes " + seconds + " seconds");
        System.out.println("Test Cases Loaded: " + manager.getTotalTests());
        System.out.println("Tests Executed: " + (manager.getPassedCount() + manager.getFailedCount()));

        System.out.println("\nThank you for using Test Data Management Utility!");
        System.out.println("========================================");
    }

    public static void main(String[] args) {
        TestManagementApp app = new TestManagementApp();
        app.run();
    }
}
```

### Deliverables

1. **Complete Source Code:**
   - All model classes
   - Manager classes
   - Executor implementations
   - Report generators
   - Utility classes
   - Main application

2. **Sample Data Files:**
   - testcases.csv with sample test cases
   - config.properties with configuration

3. **Documentation:**
   - Class diagram
   - User manual
   - API documentation
   - Installation guide

4. **Test Results:**
   - Screenshots of application running
   - Sample generated reports (HTML, CSV)
   - Test execution logs

### Extension Challenges

1. **Advanced Features:**
   - Add database connectivity for persistent storage
   - Implement parallel test execution using threads
   - Add email notification for test completion
   - Create web interface using servlets

2. **Integration:**
   - Integrate with Jenkins for CI/CD
   - Add REST API endpoints
   - Implement test scheduler
   - Add chart generation for metrics

3. **Enhanced Reporting:**
   - PDF report generation
   - Excel report with formatting
   - Real-time dashboard
   - Historical trend analysis

4. **Security:**
   - Add user authentication
   - Implement role-based access control
   - Encrypt sensitive test data
   - Add audit logging

---

## Conclusion

Congratulations on completing all 15 Java practicals! You have learned:

1. Java environment setup and basic syntax
2. Variables, data types, and operators
3. Control structures and loops
4. Methods and method overloading
5. Object-oriented programming concepts
6. Constructors and the 'this' keyword
7. Inheritance and the 'super' keyword
8. Polymorphism and dynamic method dispatch
9. Interfaces and abstract classes
10. Exception handling
11. Collections framework
12. File I/O operations
13. String manipulation and regular expressions
14. Complete application development

### Next Steps

1. Practice each exercise multiple times
2. Customize the mini-project with your own features
3. Study Java testing frameworks (JUnit, TestNG)
4. Learn Selenium WebDriver for web automation
5. Explore Maven for project management
6. Study design patterns for test automation

### Additional Resources

- Official Java Documentation
- Java Coding Standards
- Testing Best Practices
- Design Patterns for Test Automation
- Advanced Java Topics

---

**End of Phase 3-02: Java Programming Practicals for Testers**

