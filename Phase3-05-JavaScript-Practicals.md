# Phase 3 - JavaScript Practicals for Cypress Test Automation

## Table of Contents
1. [JavaScript Basics - Variables and Data Types](#exercise-1)
2. [Control Structures - If-Else and Loops](#exercise-2)
3. [Functions and Arrow Functions](#exercise-3)
4. [Objects and Arrays Manipulation](#exercise-4)
5. [Array Methods - Map, Filter, Reduce](#exercise-5)
6. [Asynchronous JavaScript - Promises](#exercise-6)
7. [Async/Await in Test Automation](#exercise-7)
8. [ES6 Features - Destructuring and Spread](#exercise-8)
9. [Working with JSON Test Data](#exercise-9)
10. [Creating Custom Cypress Commands](#exercise-10)
11. [Page Object Model in Cypress](#exercise-11)
12. [API Testing with Cypress](#exercise-12)
13. [Handling Async Operations in Tests](#exercise-13)
14. [Error Handling in Test Automation](#exercise-14)
15. [Complete Mini-Project - E-Commerce Test Suite](#exercise-15)

---

## Exercise 1: JavaScript Basics - Variables and Data Types {#exercise-1}

### Objective
Master JavaScript variable declarations and data types for writing effective Cypress tests.

### Scenario
You're testing a login form and need to store various test data including usernames, passwords, expected URLs, and boolean flags for test conditions.

### Step-by-Step Instructions

1. Create a new spec file: `cypress/e2e/01-variables-datatypes.cy.js`
2. Declare variables using `const`, `let`, and understand when to use each
3. Store different data types: strings, numbers, booleans, arrays, objects
4. Use template literals for dynamic test data
5. Practice type checking and type conversion

### Code Template

```javascript
describe('Exercise 1: Variables and Data Types', () => {

  it('should demonstrate variable declarations', () => {
    // TODO: Declare variables for test data
    // const for values that won't change
    // let for values that might change
  });

  it('should work with different data types', () => {
    // TODO: Create variables for:
    // - username (string)
    // - password (string)
    // - loginAttempts (number)
    // - isLoggedIn (boolean)
    // - testEnvironments (array)
    // - userProfile (object)
  });

  it('should use template literals', () => {
    // TODO: Create dynamic error messages using template literals
  });
});
```

### Expected Output
- Tests should pass
- Console logs should show different data types
- Variables should be properly scoped

### Solution Code

```javascript
describe('Exercise 1: Variables and Data Types', () => {

  it('should demonstrate variable declarations', () => {
    // const - cannot be reassigned
    const baseUrl = 'https://example.cypress.io';
    const timeout = 10000;

    // let - can be reassigned
    let currentPage = 'login';
    cy.log(`Current page: ${currentPage}`);

    currentPage = 'dashboard';
    cy.log(`Updated page: ${currentPage}`);

    // Verify the values
    expect(baseUrl).to.equal('https://example.cypress.io');
    expect(currentPage).to.equal('dashboard');
  });

  it('should work with different data types', () => {
    // String
    const username = 'testuser@example.com';
    const password = 'SecurePass123!';

    // Number
    const loginAttempts = 3;
    const maxTimeout = 5000;
    const completionRate = 98.5;

    // Boolean
    const isLoggedIn = false;
    const hasAdminAccess = true;

    // Array
    const testEnvironments = ['dev', 'staging', 'production'];
    const testPriorities = [1, 2, 3, 4, 5];

    // Object
    const userProfile = {
      firstName: 'John',
      lastName: 'Doe',
      email: username,
      role: 'tester',
      active: true
    };

    // Type checking
    expect(username).to.be.a('string');
    expect(loginAttempts).to.be.a('number');
    expect(isLoggedIn).to.be.a('boolean');
    expect(testEnvironments).to.be.an('array');
    expect(userProfile).to.be.an('object');

    // Log values for verification
    cy.log('Username:', username);
    cy.log('Login Attempts:', loginAttempts);
    cy.log('Environments:', JSON.stringify(testEnvironments));
    cy.log('User Profile:', JSON.stringify(userProfile));
  });

  it('should use template literals for dynamic messages', () => {
    const testName = 'Login Test';
    const status = 'passed';
    const duration = 2.5;
    const timestamp = new Date().toISOString();

    // Template literals with expressions
    const reportMessage = `Test: ${testName} | Status: ${status} | Duration: ${duration}s`;
    const detailedReport = `
      ====== Test Report ======
      Test Name: ${testName}
      Status: ${status.toUpperCase()}
      Duration: ${duration} seconds
      Timestamp: ${timestamp}
      Result: ${status === 'passed' ? '✓ PASS' : '✗ FAIL'}
      ========================
    `;

    cy.log(reportMessage);
    cy.log(detailedReport);

    // Using template literals in assertions
    expect(reportMessage).to.include(testName);
    expect(reportMessage).to.include(status);
  });

  it('should demonstrate type conversion', () => {
    // String to Number
    const stringNumber = '42';
    const actualNumber = Number(stringNumber);
    const parsedInt = parseInt('100px');
    const parsedFloat = parseFloat('3.14');

    expect(actualNumber).to.equal(42);
    expect(parsedInt).to.equal(100);
    expect(parsedFloat).to.equal(3.14);

    // Number to String
    const count = 150;
    const countString = String(count);
    const countToString = count.toString();

    expect(countString).to.equal('150');
    expect(countToString).to.be.a('string');

    // Boolean conversion
    const truthyValue = Boolean(1);
    const falsyValue = Boolean(0);

    expect(truthyValue).to.be.true;
    expect(falsyValue).to.be.false;

    cy.log('Type conversions completed successfully');
  });

  it('should work with null and undefined', () => {
    let uninitializedVariable;
    const nullValue = null;

    expect(uninitializedVariable).to.be.undefined;
    expect(nullValue).to.be.null;

    // Common use case in testing
    const response = {
      data: null,
      error: undefined
    };

    if (response.data === null) {
      cy.log('No data received');
    }

    if (response.error === undefined) {
      cy.log('No errors occurred');
    }
  });
});
```

### Deliverables Checklist
- [ ] Created spec file with all test cases
- [ ] Demonstrated const and let usage
- [ ] Used all primitive data types (string, number, boolean)
- [ ] Created arrays and objects
- [ ] Implemented template literals
- [ ] Performed type conversions
- [ ] All tests pass successfully

### Extension Challenges
1. Create a test data generator function that returns random user objects
2. Implement a type validator function that checks if test data is valid
3. Create a configuration object with nested properties for different environments
4. Use Symbol and BigInt data types in a testing context

---

## Exercise 2: Control Structures - If-Else and Loops {#exercise-2}

### Objective
Master conditional statements and loops for creating dynamic and flexible Cypress tests.

### Scenario
You need to test a dynamic application where elements appear conditionally, and you need to validate multiple items in lists or tables.

### Step-by-Step Instructions

1. Create spec file: `cypress/e2e/02-control-structures.cy.js`
2. Implement if-else conditions for conditional testing
3. Use switch statements for multiple test scenarios
4. Practice for loops, while loops, and for...of loops
5. Apply loops to validate multiple elements

### Code Template

```javascript
describe('Exercise 2: Control Structures', () => {

  it('should use if-else for conditional logic', () => {
    // TODO: Implement conditional checks based on element presence
  });

  it('should use switch statements', () => {
    // TODO: Create test scenarios based on environment
  });

  it('should iterate with loops', () => {
    // TODO: Loop through test data and perform actions
  });
});
```

### Expected Output
- Conditional logic executes correctly
- Loops iterate through all items
- All validations pass

### Solution Code

```javascript
describe('Exercise 2: Control Structures - If-Else and Loops', () => {

  before(() => {
    cy.log('Setting up test data for control structures');
  });

  it('should use if-else for conditional logic', () => {
    const testEnvironment = 'staging';
    const userRole = 'admin';
    const elementsCount = 5;

    // Simple if-else
    if (testEnvironment === 'production') {
      cy.log('Running in PRODUCTION mode - using live data');
    } else {
      cy.log('Running in TEST mode - using mock data');
    }

    // If-else with multiple conditions
    if (userRole === 'admin') {
      cy.log('Testing admin features');
      const adminFeatures = ['user-management', 'settings', 'reports'];
      expect(adminFeatures).to.have.length.greaterThan(0);
    } else if (userRole === 'manager') {
      cy.log('Testing manager features');
    } else {
      cy.log('Testing basic user features');
    }

    // Conditional test execution
    if (elementsCount > 0) {
      cy.log(`Found ${elementsCount} elements to test`);
      expect(elementsCount).to.be.greaterThan(0);
    } else {
      cy.log('No elements found - skipping validation');
    }

    // Ternary operator
    const message = elementsCount > 3
      ? 'Multiple elements found'
      : 'Few elements found';
    cy.log(message);
    expect(message).to.equal('Multiple elements found');
  });

  it('should use switch statements for multiple scenarios', () => {
    const testScenarios = ['login', 'registration', 'checkout'];

    testScenarios.forEach(scenario => {
      let testData;

      switch(scenario) {
        case 'login':
          testData = {
            username: 'test@example.com',
            password: 'Test123!',
            expectedUrl: '/dashboard'
          };
          cy.log('Login scenario:', JSON.stringify(testData));
          break;

        case 'registration':
          testData = {
            email: 'newuser@example.com',
            password: 'NewPass123!',
            confirmPassword: 'NewPass123!',
            expectedUrl: '/welcome'
          };
          cy.log('Registration scenario:', JSON.stringify(testData));
          break;

        case 'checkout':
          testData = {
            items: ['item1', 'item2'],
            total: 99.99,
            expectedUrl: '/order-confirmation'
          };
          cy.log('Checkout scenario:', JSON.stringify(testData));
          break;

        default:
          testData = {};
          cy.log('Unknown scenario');
      }

      expect(testData).to.not.be.empty;
    });
  });

  it('should use for loops to iterate', () => {
    const testCases = [
      { id: 1, input: 'test1', expected: 'TEST1' },
      { id: 2, input: 'test2', expected: 'TEST2' },
      { id: 3, input: 'test3', expected: 'TEST3' }
    ];

    // Traditional for loop
    for (let i = 0; i < testCases.length; i++) {
      const testCase = testCases[i];
      const result = testCase.input.toUpperCase();

      cy.log(`Test Case ${testCase.id}: ${testCase.input} => ${result}`);
      expect(result).to.equal(testCase.expected);
    }

    // For loop with conditional break
    for (let i = 0; i < 10; i++) {
      if (i === 5) {
        cy.log('Breaking at iteration 5');
        break;
      }
      cy.log(`Iteration: ${i}`);
    }

    // For loop with continue
    for (let i = 0; i < 5; i++) {
      if (i === 2) {
        cy.log('Skipping iteration 2');
        continue;
      }
      cy.log(`Processing iteration: ${i}`);
    }
  });

  it('should use for...of loop for arrays', () => {
    const userRoles = ['admin', 'manager', 'user', 'guest'];
    const permissions = [];

    for (const role of userRoles) {
      let permission;

      if (role === 'admin') {
        permission = 'full-access';
      } else if (role === 'manager') {
        permission = 'read-write';
      } else if (role === 'user') {
        permission = 'read-only';
      } else {
        permission = 'limited';
      }

      permissions.push({ role, permission });
      cy.log(`${role}: ${permission}`);
    }

    expect(permissions).to.have.length(4);
    expect(permissions[0].permission).to.equal('full-access');
  });

  it('should use for...in loop for objects', () => {
    const formData = {
      username: 'testuser',
      email: 'test@example.com',
      age: 25,
      country: 'USA'
    };

    const validations = [];

    for (const field in formData) {
      const value = formData[field];
      let isValid = false;

      if (typeof value === 'string' && value.length > 0) {
        isValid = true;
      } else if (typeof value === 'number' && value > 0) {
        isValid = true;
      }

      validations.push({ field, value, isValid });
      cy.log(`${field}: ${value} - Valid: ${isValid}`);
    }

    const allValid = validations.every(v => v.isValid);
    expect(allValid).to.be.true;
  });

  it('should use while loop for conditional iteration', () => {
    let retryCount = 0;
    const maxRetries = 3;
    let success = false;

    while (retryCount < maxRetries && !success) {
      retryCount++;
      cy.log(`Attempt ${retryCount} of ${maxRetries}`);

      // Simulate a condition that succeeds on 3rd try
      if (retryCount === 3) {
        success = true;
        cy.log('Operation successful!');
      } else {
        cy.log('Retrying...');
      }
    }

    expect(success).to.be.true;
    expect(retryCount).to.equal(3);
  });

  it('should validate multiple elements with loops', () => {
    // Simulate testing multiple navigation links
    const navigationLinks = [
      { text: 'Home', href: '/', visible: true },
      { text: 'Products', href: '/products', visible: true },
      { text: 'About', href: '/about', visible: true },
      { text: 'Contact', href: '/contact', visible: true },
      { text: 'Admin', href: '/admin', visible: false }
    ];

    let visibleCount = 0;
    let hiddenCount = 0;

    for (const link of navigationLinks) {
      cy.log(`Validating link: ${link.text}`);

      if (link.visible) {
        visibleCount++;
        expect(link.href).to.not.be.empty;
        cy.log(`✓ ${link.text} is visible`);
      } else {
        hiddenCount++;
        cy.log(`✗ ${link.text} is hidden`);
      }
    }

    cy.log(`Visible links: ${visibleCount}, Hidden links: ${hiddenCount}`);
    expect(visibleCount).to.equal(4);
    expect(hiddenCount).to.equal(1);
  });

  it('should use nested loops for complex validations', () => {
    const testMatrix = [
      { browser: 'chrome', resolutions: ['1920x1080', '1366x768'] },
      { browser: 'firefox', resolutions: ['1920x1080', '1366x768'] },
      { browser: 'edge', resolutions: ['1920x1080'] }
    ];

    const results = [];

    for (const config of testMatrix) {
      for (const resolution of config.resolutions) {
        const testResult = {
          browser: config.browser,
          resolution: resolution,
          status: 'pass',
          timestamp: new Date().toISOString()
        };

        results.push(testResult);
        cy.log(`Testing ${config.browser} at ${resolution}: PASS`);
      }
    }

    expect(results).to.have.length(5);
    expect(results.every(r => r.status === 'pass')).to.be.true;
  });
});
```

### Deliverables Checklist
- [ ] Implemented if-else statements
- [ ] Used switch statements for multiple scenarios
- [ ] Created for loops with break and continue
- [ ] Used for...of loops for arrays
- [ ] Used for...in loops for objects
- [ ] Implemented while loops
- [ ] Created nested loops for complex scenarios
- [ ] All tests pass

### Extension Challenges
1. Create a retry mechanism using while loop that stops after success or max attempts
2. Implement a test data validator that uses nested loops to check multi-dimensional arrays
3. Build a dynamic test skipper that uses conditionals to skip tests based on environment
4. Create a loop that validates table data row by row with conditional checks

---

## Exercise 3: Functions and Arrow Functions {#exercise-3}

### Objective
Master function declarations, expressions, and arrow functions for creating reusable test utilities.

### Scenario
Create reusable utility functions for common testing operations like data generation, validation, and formatting.

### Step-by-Step Instructions

1. Create spec file: `cypress/e2e/03-functions.cy.js`
2. Write traditional function declarations
3. Create function expressions
4. Implement arrow functions
5. Understand function scope and closures
6. Create higher-order functions

### Code Template

```javascript
describe('Exercise 3: Functions and Arrow Functions', () => {

  it('should use function declarations', () => {
    // TODO: Create named functions for test utilities
  });

  it('should use arrow functions', () => {
    // TODO: Implement arrow functions for callbacks
  });

  it('should demonstrate function scope', () => {
    // TODO: Show variable scope in functions
  });
});
```

### Expected Output
- Functions execute correctly
- Return values are accurate
- Scope is properly managed

### Solution Code

```javascript
describe('Exercise 3: Functions and Arrow Functions', () => {

  // Traditional function declaration
  function generateTestUser(role = 'user') {
    return {
      username: `test_${role}_${Date.now()}`,
      email: `${role}@test.com`,
      role: role,
      createdAt: new Date().toISOString()
    };
  }

  // Function expression
  const validateEmail = function(email) {
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    return emailRegex.test(email);
  };

  // Arrow function
  const calculateTotal = (items) => {
    return items.reduce((sum, item) => sum + item.price, 0);
  };

  // Concise arrow function
  const isValidPassword = (password) => password.length >= 8;

  it('should use function declarations', () => {
    const user = generateTestUser('admin');

    cy.log('Generated user:', JSON.stringify(user));
    expect(user.username).to.include('test_admin_');
    expect(user.role).to.equal('admin');
    expect(user.email).to.equal('admin@test.com');
  });

  it('should use function expressions', () => {
    const validEmail = 'test@example.com';
    const invalidEmail = 'invalid-email';

    expect(validateEmail(validEmail)).to.be.true;
    expect(validateEmail(invalidEmail)).to.be.false;

    cy.log('Email validation working correctly');
  });

  it('should use arrow functions', () => {
    const items = [
      { name: 'Item 1', price: 10.99 },
      { name: 'Item 2', price: 25.50 },
      { name: 'Item 3', price: 5.75 }
    ];

    const total = calculateTotal(items);
    cy.log(`Total price: $${total.toFixed(2)}`);
    expect(total).to.equal(42.24);
  });

  it('should use concise arrow functions', () => {
    expect(isValidPassword('short')).to.be.false;
    expect(isValidPassword('LongPassword123')).to.be.true;
  });

  it('should demonstrate function parameters and defaults', () => {
    // Function with default parameters
    const createTestConfig = (env = 'staging', timeout = 5000, retries = 2) => ({
      environment: env,
      timeout: timeout,
      retries: retries
    });

    const defaultConfig = createTestConfig();
    const customConfig = createTestConfig('production', 10000, 3);

    expect(defaultConfig.environment).to.equal('staging');
    expect(customConfig.timeout).to.equal(10000);

    cy.log('Default config:', JSON.stringify(defaultConfig));
    cy.log('Custom config:', JSON.stringify(customConfig));
  });

  it('should use rest parameters', () => {
    const logTestResults = (testName, ...results) => {
      cy.log(`Test: ${testName}`);
      cy.log(`Total assertions: ${results.length}`);

      const allPassed = results.every(r => r === true);
      return allPassed;
    };

    const result = logTestResults('Login Test', true, true, true, true);
    expect(result).to.be.true;

    const failedResult = logTestResults('Failed Test', true, false, true);
    expect(failedResult).to.be.false;
  });

  it('should demonstrate closures', () => {
    function createCounter(initialValue = 0) {
      let count = initialValue;

      return {
        increment: () => ++count,
        decrement: () => --count,
        getValue: () => count,
        reset: () => { count = initialValue; return count; }
      };
    }

    const counter = createCounter(10);

    expect(counter.increment()).to.equal(11);
    expect(counter.increment()).to.equal(12);
    expect(counter.decrement()).to.equal(11);
    expect(counter.getValue()).to.equal(11);
    expect(counter.reset()).to.equal(10);

    cy.log('Counter operations completed successfully');
  });

  it('should use higher-order functions', () => {
    // Function that returns a function
    const createValidator = (minLength) => {
      return (value) => value.length >= minLength;
    };

    const validateUsername = createValidator(5);
    const validatePassword = createValidator(8);

    expect(validateUsername('john')).to.be.false;
    expect(validateUsername('johndoe')).to.be.true;
    expect(validatePassword('pass')).to.be.false;
    expect(validatePassword('password123')).to.be.true;
  });

  it('should use functions as callbacks', () => {
    const testData = [
      { id: 1, name: 'Test 1', status: 'pass' },
      { id: 2, name: 'Test 2', status: 'fail' },
      { id: 3, name: 'Test 3', status: 'pass' },
      { id: 4, name: 'Test 4', status: 'skip' }
    ];

    // Filter with arrow function callback
    const passedTests = testData.filter(test => test.status === 'pass');
    expect(passedTests).to.have.length(2);

    // Map with arrow function callback
    const testNames = testData.map(test => test.name);
    expect(testNames).to.deep.equal(['Test 1', 'Test 2', 'Test 3', 'Test 4']);

    // ForEach with arrow function callback
    testData.forEach(test => {
      cy.log(`${test.name}: ${test.status.toUpperCase()}`);
    });
  });

  it('should create utility functions for test automation', () => {
    // Wait utility
    const waitFor = (condition, timeout = 5000) => {
      cy.log(`Waiting for condition (timeout: ${timeout}ms)`);
      return condition;
    };

    // Random string generator
    const generateRandomString = (length = 10) => {
      const chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
      return Array.from({ length }, () => chars.charAt(Math.floor(Math.random() * chars.length))).join('');
    };

    // Date formatter
    const formatTestDate = (date = new Date()) => {
      return date.toISOString().split('T')[0];
    };

    const randomStr = generateRandomString(15);
    const today = formatTestDate();

    expect(randomStr).to.have.length(15);
    expect(today).to.match(/^\d{4}-\d{2}-\d{2}$/);

    cy.log('Random string:', randomStr);
    cy.log('Formatted date:', today);
  });

  it('should compose functions', () => {
    // Function composition
    const trim = (str) => str.trim();
    const toLowerCase = (str) => str.toLowerCase();
    const removeSpaces = (str) => str.replace(/\s+/g, '-');

    // Compose function
    const compose = (...functions) => {
      return (value) => {
        return functions.reduceRight((acc, fn) => fn(acc), value);
      };
    };

    const normalizeString = compose(removeSpaces, toLowerCase, trim);

    const result = normalizeString('  Hello World Test  ');
    expect(result).to.equal('hello-world-test');

    cy.log('Normalized string:', result);
  });

  it('should use immediately invoked function expressions (IIFE)', () => {
    const result = (function(name) {
      const greeting = 'Hello';
      return `${greeting}, ${name}!`;
    })('Cypress');

    expect(result).to.equal('Hello, Cypress!');
    cy.log(result);
  });
});
```

### Deliverables Checklist
- [ ] Created function declarations
- [ ] Implemented function expressions
- [ ] Used arrow functions
- [ ] Demonstrated default parameters
- [ ] Used rest parameters
- [ ] Implemented closures
- [ ] Created higher-order functions
- [ ] Used functions as callbacks
- [ ] All tests pass

### Extension Challenges
1. Create a memoization function that caches test results
2. Implement a curry function for partial application
3. Build a retry mechanism using recursive functions
4. Create a function pipeline for test data transformation

---

## Exercise 4: Objects and Arrays Manipulation {#exercise-4}

### Objective
Master object and array manipulation techniques essential for handling test data in Cypress.

### Scenario
Manage complex test data structures including user profiles, product catalogs, and test results.

### Step-by-Step Instructions

1. Create spec file: `cypress/e2e/04-objects-arrays.cy.js`
2. Create and manipulate objects
3. Work with nested objects
4. Perform array operations (push, pop, shift, unshift, splice)
5. Clone objects and arrays
6. Merge and combine data structures

### Code Template

```javascript
describe('Exercise 4: Objects and Arrays', () => {

  it('should create and manipulate objects', () => {
    // TODO: Create user objects with properties
  });

  it('should work with arrays', () => {
    // TODO: Perform array operations
  });

  it('should handle nested structures', () => {
    // TODO: Access and modify nested data
  });
});
```

### Expected Output
- Objects are created and modified correctly
- Array operations work as expected
- Nested data is accessible

### Solution Code

```javascript
describe('Exercise 4: Objects and Arrays Manipulation', () => {

  it('should create and manipulate objects', () => {
    // Creating objects
    const user = {
      id: 1,
      username: 'testuser',
      email: 'test@example.com',
      isActive: true
    };

    // Accessing properties
    expect(user.username).to.equal('testuser');
    expect(user['email']).to.equal('test@example.com');

    // Adding properties
    user.lastName = 'Doe';
    user['firstName'] = 'John';

    expect(user.firstName).to.equal('John');
    expect(user.lastName).to.equal('Doe');

    // Modifying properties
    user.email = 'updated@example.com';
    expect(user.email).to.equal('updated@example.com');

    // Deleting properties
    delete user.isActive;
    expect(user.isActive).to.be.undefined;

    cy.log('User object:', JSON.stringify(user));
  });

  it('should work with object methods', () => {
    const testConfig = {
      browser: 'chrome',
      viewport: { width: 1920, height: 1080 },
      timeout: 5000
    };

    // Object.keys()
    const keys = Object.keys(testConfig);
    expect(keys).to.deep.equal(['browser', 'viewport', 'timeout']);

    // Object.values()
    const values = Object.values(testConfig);
    expect(values).to.have.length(3);

    // Object.entries()
    const entries = Object.entries(testConfig);
    cy.log('Config entries:', JSON.stringify(entries));
    expect(entries[0]).to.deep.equal(['browser', 'chrome']);

    // Object.assign() - merging objects
    const additionalConfig = { headless: true, video: false };
    const mergedConfig = Object.assign({}, testConfig, additionalConfig);

    expect(mergedConfig.headless).to.be.true;
    expect(mergedConfig.browser).to.equal('chrome');
  });

  it('should work with nested objects', () => {
    const testSuite = {
      name: 'E-Commerce Tests',
      environment: 'staging',
      config: {
        baseUrl: 'https://staging.example.com',
        credentials: {
          admin: {
            username: 'admin@test.com',
            password: 'admin123'
          },
          user: {
            username: 'user@test.com',
            password: 'user123'
          }
        }
      },
      tests: ['login', 'checkout', 'search']
    };

    // Accessing nested properties
    expect(testSuite.config.baseUrl).to.include('staging');
    expect(testSuite.config.credentials.admin.username).to.equal('admin@test.com');

    // Modifying nested properties
    testSuite.config.credentials.user.password = 'newpass123';
    expect(testSuite.config.credentials.user.password).to.equal('newpass123');

    // Adding to nested arrays
    testSuite.tests.push('wishlist');
    expect(testSuite.tests).to.have.length(4);

    cy.log('Test Suite:', JSON.stringify(testSuite, null, 2));
  });

  it('should perform array operations', () => {
    let testCases = ['login', 'registration', 'checkout'];

    // Push - add to end
    testCases.push('logout');
    expect(testCases).to.have.length(4);
    expect(testCases[3]).to.equal('logout');

    // Pop - remove from end
    const removed = testCases.pop();
    expect(removed).to.equal('logout');
    expect(testCases).to.have.length(3);

    // Unshift - add to beginning
    testCases.unshift('homepage');
    expect(testCases[0]).to.equal('homepage');
    expect(testCases).to.have.length(4);

    // Shift - remove from beginning
    const first = testCases.shift();
    expect(first).to.equal('homepage');
    expect(testCases).to.have.length(3);

    cy.log('Test cases:', JSON.stringify(testCases));
  });

  it('should use splice to modify arrays', () => {
    let priorities = ['high', 'medium', 'low', 'critical'];

    // Remove elements
    const removed = priorities.splice(2, 1); // Remove 1 element at index 2
    expect(removed).to.deep.equal(['low']);
    expect(priorities).to.deep.equal(['high', 'medium', 'critical']);

    // Add elements
    priorities.splice(1, 0, 'urgent'); // Add at index 1, remove 0
    expect(priorities).to.deep.equal(['high', 'urgent', 'medium', 'critical']);

    // Replace elements
    priorities.splice(2, 1, 'normal'); // Replace 1 element at index 2
    expect(priorities).to.deep.equal(['high', 'urgent', 'normal', 'critical']);

    cy.log('Priorities:', JSON.stringify(priorities));
  });

  it('should use slice to create sub-arrays', () => {
    const allTests = ['test1', 'test2', 'test3', 'test4', 'test5'];

    // Slice without modifying original
    const subset1 = allTests.slice(0, 3);
    const subset2 = allTests.slice(2);
    const subset3 = allTests.slice(-2);

    expect(subset1).to.deep.equal(['test1', 'test2', 'test3']);
    expect(subset2).to.deep.equal(['test3', 'test4', 'test5']);
    expect(subset3).to.deep.equal(['test4', 'test5']);
    expect(allTests).to.have.length(5); // Original unchanged
  });

  it('should concatenate arrays', () => {
    const smokeTests = ['login', 'logout'];
    const regressionTests = ['checkout', 'payment', 'search'];
    const sanityTests = ['homepage'];

    const allTests = smokeTests.concat(regressionTests, sanityTests);

    expect(allTests).to.have.length(6);
    expect(allTests).to.deep.equal([
      'login', 'logout', 'checkout', 'payment', 'search', 'homepage'
    ]);
  });

  it('should find elements in arrays', () => {
    const users = [
      { id: 1, name: 'John', role: 'admin' },
      { id: 2, name: 'Jane', role: 'user' },
      { id: 3, name: 'Bob', role: 'manager' }
    ];

    // indexOf
    const names = users.map(u => u.name);
    expect(names.indexOf('Jane')).to.equal(1);
    expect(names.indexOf('Unknown')).to.equal(-1);

    // includes
    expect(names.includes('Bob')).to.be.true;
    expect(names.includes('Alice')).to.be.false;

    // find
    const admin = users.find(user => user.role === 'admin');
    expect(admin.name).to.equal('John');

    // findIndex
    const managerIndex = users.findIndex(user => user.role === 'manager');
    expect(managerIndex).to.equal(2);

    // some
    const hasAdmin = users.some(user => user.role === 'admin');
    expect(hasAdmin).to.be.true;

    // every
    const allHaveId = users.every(user => user.id > 0);
    expect(allHaveId).to.be.true;
  });

  it('should clone objects and arrays', () => {
    const original = {
      name: 'Test Suite',
      count: 5,
      tags: ['smoke', 'regression']
    };

    // Shallow clone with spread operator
    const clone1 = { ...original };
    clone1.name = 'Modified Suite';

    expect(original.name).to.equal('Test Suite');
    expect(clone1.name).to.equal('Modified Suite');

    // Array clone
    const originalArray = [1, 2, 3, 4, 5];
    const clonedArray = [...originalArray];
    clonedArray.push(6);

    expect(originalArray).to.have.length(5);
    expect(clonedArray).to.have.length(6);

    // Deep clone with JSON (for simple objects)
    const deepOriginal = {
      level1: {
        level2: {
          value: 'test'
        }
      }
    };

    const deepClone = JSON.parse(JSON.stringify(deepOriginal));
    deepClone.level1.level2.value = 'modified';

    expect(deepOriginal.level1.level2.value).to.equal('test');
    expect(deepClone.level1.level2.value).to.equal('modified');
  });

  it('should reverse and sort arrays', () => {
    // Reverse
    let numbers = [1, 2, 3, 4, 5];
    numbers.reverse();
    expect(numbers).to.deep.equal([5, 4, 3, 2, 1]);

    // Sort strings
    let names = ['Charlie', 'Alice', 'Bob'];
    names.sort();
    expect(names).to.deep.equal(['Alice', 'Bob', 'Charlie']);

    // Sort numbers (requires compare function)
    let scores = [100, 25, 50, 75, 10];
    scores.sort((a, b) => a - b); // Ascending
    expect(scores).to.deep.equal([10, 25, 50, 75, 100]);

    // Sort objects
    let tests = [
      { name: 'Test C', priority: 3 },
      { name: 'Test A', priority: 1 },
      { name: 'Test B', priority: 2 }
    ];
    tests.sort((a, b) => a.priority - b.priority);
    expect(tests[0].name).to.equal('Test A');
  });

  it('should work with array destructuring', () => {
    const testResults = ['pass', 'fail', 'skip', 'pass'];

    // Basic destructuring
    const [first, second] = testResults;
    expect(first).to.equal('pass');
    expect(second).to.equal('fail');

    // Skip elements
    const [, , third] = testResults;
    expect(third).to.equal('skip');

    // Rest operator
    const [firstResult, ...remaining] = testResults;
    expect(firstResult).to.equal('pass');
    expect(remaining).to.have.length(3);
  });

  it('should work with object destructuring', () => {
    const testConfig = {
      browser: 'chrome',
      headless: true,
      viewport: { width: 1920, height: 1080 },
      timeout: 5000
    };

    // Basic destructuring
    const { browser, timeout } = testConfig;
    expect(browser).to.equal('chrome');
    expect(timeout).to.equal(5000);

    // Rename variables
    const { headless: isHeadless } = testConfig;
    expect(isHeadless).to.be.true;

    // Default values
    const { retries = 2 } = testConfig;
    expect(retries).to.equal(2);

    // Nested destructuring
    const { viewport: { width } } = testConfig;
    expect(width).to.equal(1920);
  });

  it('should create test data structures', () => {
    // Array of test users
    const testUsers = [
      { username: 'user1', password: 'pass1', role: 'admin' },
      { username: 'user2', password: 'pass2', role: 'user' },
      { username: 'user3', password: 'pass3', role: 'manager' }
    ];

    // Test matrix
    const testMatrix = {
      browsers: ['chrome', 'firefox', 'edge'],
      viewports: [
        { width: 1920, height: 1080, name: 'desktop' },
        { width: 768, height: 1024, name: 'tablet' },
        { width: 375, height: 667, name: 'mobile' }
      ],
      environments: ['dev', 'staging', 'production']
    };

    // Test results structure
    const testResults = {
      suiteName: 'E2E Tests',
      startTime: new Date().toISOString(),
      tests: [],
      summary: {
        total: 0,
        passed: 0,
        failed: 0,
        skipped: 0
      }
    };

    // Add test result
    testResults.tests.push({
      name: 'Login Test',
      status: 'pass',
      duration: 2500
    });
    testResults.summary.total++;
    testResults.summary.passed++;

    expect(testUsers).to.have.length(3);
    expect(testMatrix.browsers).to.include('chrome');
    expect(testResults.summary.passed).to.equal(1);

    cy.log('Test data structures created successfully');
  });
});
```

### Deliverables Checklist
- [ ] Created and manipulated objects
- [ ] Used Object methods (keys, values, entries, assign)
- [ ] Worked with nested objects
- [ ] Performed array operations (push, pop, shift, unshift)
- [ ] Used splice and slice
- [ ] Found elements with indexOf, includes, find
- [ ] Cloned objects and arrays
- [ ] Sorted and reversed arrays
- [ ] All tests pass

### Extension Challenges
1. Create a deep merge function for combining test configurations
2. Implement a function to flatten nested objects
3. Build a data generator that creates random test datasets
4. Create a utility to compare two objects and return differences

---

## Exercise 5: Array Methods - Map, Filter, Reduce {#exercise-5}

### Objective
Master functional array methods for data transformation and processing in test automation.

### Scenario
Process test results, filter test cases, transform test data, and aggregate test metrics.

### Step-by-Step Instructions

1. Create spec file: `cypress/e2e/05-array-methods.cy.js`
2. Use map() to transform data
3. Use filter() to select specific items
4. Use reduce() to aggregate data
5. Chain multiple array methods
6. Apply to real test automation scenarios

### Code Template

```javascript
describe('Exercise 5: Array Methods', () => {

  it('should use map to transform data', () => {
    // TODO: Transform test data
  });

  it('should use filter to select items', () => {
    // TODO: Filter test cases
  });

  it('should use reduce to aggregate', () => {
    // TODO: Calculate test metrics
  });
});
```

### Expected Output
- Data transformations work correctly
- Filtering produces expected results
- Aggregations are accurate

### Solution Code

```javascript
describe('Exercise 5: Array Methods - Map, Filter, Reduce', () => {

  const testResults = [
    { id: 1, name: 'Login Test', status: 'pass', duration: 2500, priority: 'high' },
    { id: 2, name: 'Registration Test', status: 'pass', duration: 3200, priority: 'high' },
    { id: 3, name: 'Checkout Test', status: 'fail', duration: 4100, priority: 'critical' },
    { id: 4, name: 'Search Test', status: 'pass', duration: 1800, priority: 'medium' },
    { id: 5, name: 'Profile Update', status: 'skip', duration: 0, priority: 'low' },
    { id: 6, name: 'Payment Test', status: 'fail', duration: 3500, priority: 'critical' },
    { id: 7, name: 'Wishlist Test', status: 'pass', duration: 2100, priority: 'medium' }
  ];

  it('should use map to transform data', () => {
    // Extract test names
    const testNames = testResults.map(test => test.name);
    expect(testNames).to.have.length(7);
    expect(testNames[0]).to.equal('Login Test');

    // Transform to uppercase
    const uppercaseNames = testResults.map(test => test.name.toUpperCase());
    expect(uppercaseNames[0]).to.equal('LOGIN TEST');

    // Create summary objects
    const summaries = testResults.map(test => ({
      name: test.name,
      passed: test.status === 'pass',
      timeInSeconds: test.duration / 1000
    }));

    expect(summaries[0].timeInSeconds).to.equal(2.5);
    cy.log('Summaries:', JSON.stringify(summaries, null, 2));

    // Add calculated property
    const enhanced = testResults.map(test => ({
      ...test,
      statusEmoji: test.status === 'pass' ? '✓' : test.status === 'fail' ? '✗' : '○'
    }));

    expect(enhanced[0].statusEmoji).to.equal('✓');
  });

  it('should use filter to select specific items', () => {
    // Filter passed tests
    const passedTests = testResults.filter(test => test.status === 'pass');
    expect(passedTests).to.have.length(4);

    // Filter failed tests
    const failedTests = testResults.filter(test => test.status === 'fail');
    expect(failedTests).to.have.length(2);

    // Filter by priority
    const criticalTests = testResults.filter(test => test.priority === 'critical');
    expect(criticalTests).to.have.length(2);

    // Filter by duration
    const quickTests = testResults.filter(test => test.duration < 2500 && test.duration > 0);
    expect(quickTests).to.have.length(2);

    // Multiple conditions
    const criticalFailures = testResults.filter(
      test => test.status === 'fail' && test.priority === 'critical'
    );
    expect(criticalFailures).to.have.length(2);

    cy.log('Critical failures:', JSON.stringify(criticalFailures, null, 2));
  });

  it('should use reduce to aggregate data', () => {
    // Calculate total duration
    const totalDuration = testResults.reduce((sum, test) => sum + test.duration, 0);
    expect(totalDuration).to.equal(17200);
    cy.log(`Total duration: ${totalDuration}ms`);

    // Count test statuses
    const statusCounts = testResults.reduce((counts, test) => {
      counts[test.status] = (counts[test.status] || 0) + 1;
      return counts;
    }, {});

    expect(statusCounts.pass).to.equal(4);
    expect(statusCounts.fail).to.equal(2);
    expect(statusCounts.skip).to.equal(1);
    cy.log('Status counts:', JSON.stringify(statusCounts));

    // Group by priority
    const byPriority = testResults.reduce((groups, test) => {
      if (!groups[test.priority]) {
        groups[test.priority] = [];
      }
      groups[test.priority].push(test);
      return groups;
    }, {});

    expect(byPriority.high).to.have.length(2);
    expect(byPriority.critical).to.have.length(2);

    // Calculate average duration (excluding skipped)
    const activeTests = testResults.filter(test => test.status !== 'skip');
    const avgDuration = activeTests.reduce((sum, test) => sum + test.duration, 0) / activeTests.length;
    expect(avgDuration).to.be.closeTo(2866, 1);
    cy.log(`Average duration: ${avgDuration.toFixed(2)}ms`);
  });

  it('should chain array methods', () => {
    // Get names of failed high-priority tests
    const criticalFailedNames = testResults
      .filter(test => test.status === 'fail')
      .filter(test => test.priority === 'critical')
      .map(test => test.name);

    expect(criticalFailedNames).to.deep.equal(['Checkout Test', 'Payment Test']);

    // Calculate total duration of passed tests
    const passedDuration = testResults
      .filter(test => test.status === 'pass')
      .reduce((sum, test) => sum + test.duration, 0);

    expect(passedDuration).to.equal(9600);

    // Get priority counts for passed tests
    const passedPriorities = testResults
      .filter(test => test.status === 'pass')
      .map(test => test.priority)
      .reduce((counts, priority) => {
        counts[priority] = (counts[priority] || 0) + 1;
        return counts;
      }, {});

    expect(passedPriorities.high).to.equal(2);
    expect(passedPriorities.medium).to.equal(2);
  });

  it('should use forEach for side effects', () => {
    const report = [];

    testResults.forEach((test, index) => {
      const statusSymbol = test.status === 'pass' ? '✓' : test.status === 'fail' ? '✗' : '○';
      const message = `${index + 1}. ${statusSymbol} ${test.name} - ${test.status}`;
      report.push(message);
      cy.log(message);
    });

    expect(report).to.have.length(7);
  });

  it('should use find and findIndex', () => {
    // Find first failed test
    const firstFailure = testResults.find(test => test.status === 'fail');
    expect(firstFailure.name).to.equal('Checkout Test');

    // Find index of specific test
    const paymentTestIndex = testResults.findIndex(test => test.name === 'Payment Test');
    expect(paymentTestIndex).to.equal(5);

    // Find test with longest duration
    const longestTest = testResults.reduce((longest, test) => {
      return test.duration > longest.duration ? test : longest;
    }, testResults[0]);

    expect(longestTest.name).to.equal('Checkout Test');
    expect(longestTest.duration).to.equal(4100);
  });

  it('should use some and every', () => {
    // Check if any test failed
    const hasFailures = testResults.some(test => test.status === 'fail');
    expect(hasFailures).to.be.true;

    // Check if all tests have an id
    const allHaveId = testResults.every(test => test.id > 0);
    expect(allHaveId).to.be.true;

    // Check if all tests passed
    const allPassed = testResults.every(test => test.status === 'pass');
    expect(allPassed).to.be.false;

    // Check if any critical test failed
    const criticalFailure = testResults.some(
      test => test.priority === 'critical' && test.status === 'fail'
    );
    expect(criticalFailure).to.be.true;
  });

  it('should create test report with array methods', () => {
    const report = {
      totalTests: testResults.length,
      passed: testResults.filter(t => t.status === 'pass').length,
      failed: testResults.filter(t => t.status === 'fail').length,
      skipped: testResults.filter(t => t.status === 'skip').length,
      totalDuration: testResults.reduce((sum, t) => sum + t.duration, 0),
      avgDuration: 0,
      passRate: 0,
      criticalFailures: [],
      slowestTests: []
    };

    // Calculate average (excluding skipped)
    const activeTests = testResults.filter(t => t.status !== 'skip');
    report.avgDuration = Math.round(
      activeTests.reduce((sum, t) => sum + t.duration, 0) / activeTests.length
    );

    // Calculate pass rate
    report.passRate = Math.round((report.passed / report.totalTests) * 100);

    // Get critical failures
    report.criticalFailures = testResults
      .filter(t => t.status === 'fail' && t.priority === 'critical')
      .map(t => t.name);

    // Get slowest tests
    report.slowestTests = testResults
      .filter(t => t.duration > 0)
      .sort((a, b) => b.duration - a.duration)
      .slice(0, 3)
      .map(t => ({ name: t.name, duration: t.duration }));

    cy.log('Test Report:', JSON.stringify(report, null, 2));

    expect(report.totalTests).to.equal(7);
    expect(report.passed).to.equal(4);
    expect(report.passRate).to.equal(57);
    expect(report.criticalFailures).to.have.length(2);
    expect(report.slowestTests[0].name).to.equal('Checkout Test');
  });

  it('should process test data with flatMap', () => {
    const testSuites = [
      { name: 'Auth Tests', tests: ['login', 'logout', 'register'] },
      { name: 'E-Commerce Tests', tests: ['cart', 'checkout', 'payment'] },
      { name: 'Search Tests', tests: ['basic-search', 'advanced-search'] }
    ];

    // Extract all test names
    const allTests = testSuites.flatMap(suite => suite.tests);
    expect(allTests).to.have.length(8);
    expect(allTests).to.include('login');

    // Create full test names
    const fullNames = testSuites.flatMap(suite =>
      suite.tests.map(test => `${suite.name}::${test}`)
    );

    expect(fullNames[0]).to.equal('Auth Tests::login');
    cy.log('Full test names:', JSON.stringify(fullNames, null, 2));
  });

  it('should work with real Cypress scenario', () => {
    // Simulate API responses
    const apiResponses = [
      { endpoint: '/api/users', status: 200, time: 150 },
      { endpoint: '/api/products', status: 200, time: 320 },
      { endpoint: '/api/orders', status: 500, time: 8000 },
      { endpoint: '/api/cart', status: 200, time: 180 },
      { endpoint: '/api/checkout', status: 404, time: 5000 }
    ];

    // Analyze API responses
    const successfulCalls = apiResponses.filter(r => r.status === 200);
    const failedCalls = apiResponses.filter(r => r.status >= 400);
    const slowCalls = apiResponses.filter(r => r.time > 1000);

    const avgResponseTime = Math.round(
      apiResponses.reduce((sum, r) => sum + r.time, 0) / apiResponses.length
    );

    const healthReport = {
      total: apiResponses.length,
      successful: successfulCalls.length,
      failed: failedCalls.length,
      slow: slowCalls.length,
      avgResponseTime: avgResponseTime,
      failedEndpoints: failedCalls.map(r => r.endpoint)
    };

    cy.log('API Health Report:', JSON.stringify(healthReport, null, 2));

    expect(healthReport.successful).to.equal(3);
    expect(healthReport.failed).to.equal(2);
    expect(healthReport.failedEndpoints).to.include('/api/orders');
  });
});
```

### Deliverables Checklist
- [ ] Used map() to transform arrays
- [ ] Used filter() to select items
- [ ] Used reduce() to aggregate data
- [ ] Chained multiple array methods
- [ ] Used find() and findIndex()
- [ ] Used some() and every()
- [ ] Created comprehensive test reports
- [ ] Applied to real testing scenarios
- [ ] All tests pass

### Extension Challenges
1. Create a function that generates test statistics from raw test data
2. Implement a data pipeline using method chaining for test result processing
3. Build a test prioritizer that sorts tests based on multiple criteria
4. Create a performance analyzer for API response times

---

## Exercise 6: Asynchronous JavaScript - Promises {#exercise-6}

### Objective
Understand Promises and asynchronous operations essential for Cypress testing.

### Scenario
Handle asynchronous operations like API calls, file operations, and delayed actions in tests.

### Step-by-Step Instructions

1. Create spec file: `cypress/e2e/06-promises.cy.js`
2. Understand Promise states (pending, fulfilled, rejected)
3. Create and resolve Promises
4. Handle Promise rejection
5. Chain Promises with .then()
6. Use Promise.all() and Promise.race()

### Code Template

```javascript
describe('Exercise 6: Promises', () => {

  it('should create and resolve promises', () => {
    // TODO: Create a promise that resolves
  });

  it('should handle promise rejection', () => {
    // TODO: Handle rejected promises
  });

  it('should chain promises', () => {
    // TODO: Chain multiple promises
  });
});
```

### Expected Output
- Promises resolve correctly
- Rejections are handled
- Chaining works as expected

### Solution Code

```javascript
describe('Exercise 6: Asynchronous JavaScript - Promises', () => {

  // Helper function to simulate async operations
  const simulateApiCall = (data, delay = 1000, shouldFail = false) => {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        if (shouldFail) {
          reject(new Error(`API call failed for: ${JSON.stringify(data)}`));
        } else {
          resolve({ success: true, data: data, timestamp: Date.now() });
        }
      }, delay);
    });
  };

  it('should understand promise basics', () => {
    // Creating a simple promise
    const simplePromise = new Promise((resolve, reject) => {
      const success = true;
      if (success) {
        resolve('Operation completed successfully');
      } else {
        reject('Operation failed');
      }
    });

    // Consuming the promise
    simplePromise.then(result => {
      cy.log('Promise resolved:', result);
      expect(result).to.equal('Operation completed successfully');
    });
  });

  it('should create and resolve promises', () => {
    // Create a promise that resolves immediately
    const promise1 = Promise.resolve('Immediate value');

    promise1.then(value => {
      expect(value).to.equal('Immediate value');
      cy.log('Promise 1 resolved:', value);
    });

    // Create a promise with delayed resolution
    const promise2 = new Promise(resolve => {
      setTimeout(() => {
        resolve('Delayed value');
      }, 100);
    });

    promise2.then(value => {
      expect(value).to.equal('Delayed value');
      cy.log('Promise 2 resolved:', value);
    });
  });

  it('should handle promise rejection', () => {
    // Create a rejected promise
    const failedPromise = Promise.reject(new Error('Something went wrong'));

    // Handle rejection with .catch()
    failedPromise.catch(error => {
      expect(error.message).to.equal('Something went wrong');
      cy.log('Error caught:', error.message);
    });

    // Handle with .then() second parameter
    const anotherPromise = new Promise((resolve, reject) => {
      reject(new Error('Test error'));
    });

    anotherPromise.then(
      result => {
        cy.log('Success:', result);
      },
      error => {
        expect(error.message).to.equal('Test error');
        cy.log('Error handled:', error.message);
      }
    );
  });

  it('should chain promises', () => {
    cy.wrap(null).then(() => {
      return simulateApiCall({ step: 1 }, 100)
        .then(result1 => {
          cy.log('Step 1 completed:', JSON.stringify(result1));
          expect(result1.success).to.be.true;
          return simulateApiCall({ step: 2, previousData: result1.data }, 100);
        })
        .then(result2 => {
          cy.log('Step 2 completed:', JSON.stringify(result2));
          expect(result2.data.previousData.step).to.equal(1);
          return simulateApiCall({ step: 3 }, 100);
        })
        .then(result3 => {
          cy.log('Step 3 completed:', JSON.stringify(result3));
          expect(result3.data.step).to.equal(3);
        });
    });
  });

  it('should use Promise.all for parallel operations', () => {
    cy.wrap(null).then(() => {
      const promise1 = simulateApiCall({ endpoint: '/users' }, 100);
      const promise2 = simulateApiCall({ endpoint: '/products' }, 150);
      const promise3 = simulateApiCall({ endpoint: '/orders' }, 200);

      return Promise.all([promise1, promise2, promise3])
        .then(results => {
          cy.log('All promises resolved');
          expect(results).to.have.length(3);
          expect(results[0].data.endpoint).to.equal('/users');
          expect(results[1].data.endpoint).to.equal('/products');
          expect(results[2].data.endpoint).to.equal('/orders');

          results.forEach((result, index) => {
            cy.log(`Result ${index + 1}:`, JSON.stringify(result));
          });
        });
    });
  });

  it('should handle Promise.all with failure', () => {
    cy.wrap(null).then(() => {
      const promise1 = simulateApiCall({ endpoint: '/users' }, 100);
      const promise2 = simulateApiCall({ endpoint: '/products' }, 150, true); // This will fail
      const promise3 = simulateApiCall({ endpoint: '/orders' }, 200);

      return Promise.all([promise1, promise2, promise3])
        .then(results => {
          cy.log('This should not execute');
        })
        .catch(error => {
          cy.log('Promise.all failed:', error.message);
          expect(error.message).to.include('API call failed');
        });
    });
  });

  it('should use Promise.race', () => {
    cy.wrap(null).then(() => {
      const slowPromise = simulateApiCall({ type: 'slow' }, 500);
      const fastPromise = simulateApiCall({ type: 'fast' }, 100);
      const mediumPromise = simulateApiCall({ type: 'medium' }, 300);

      return Promise.race([slowPromise, fastPromise, mediumPromise])
        .then(result => {
          cy.log('First promise resolved:', JSON.stringify(result));
          expect(result.data.type).to.equal('fast');
        });
    });
  });

  it('should use Promise.allSettled', () => {
    cy.wrap(null).then(() => {
      const promise1 = simulateApiCall({ id: 1 }, 100);
      const promise2 = simulateApiCall({ id: 2 }, 150, true); // Fails
      const promise3 = simulateApiCall({ id: 3 }, 200);

      return Promise.allSettled([promise1, promise2, promise3])
        .then(results => {
          cy.log('All promises settled');

          expect(results).to.have.length(3);
          expect(results[0].status).to.equal('fulfilled');
          expect(results[1].status).to.equal('rejected');
          expect(results[2].status).to.equal('fulfilled');

          const fulfilled = results.filter(r => r.status === 'fulfilled');
          const rejected = results.filter(r => r.status === 'rejected');

          cy.log(`Fulfilled: ${fulfilled.length}, Rejected: ${rejected.length}`);
        });
    });
  });

  it('should use Promise.any', () => {
    cy.wrap(null).then(() => {
      const promise1 = simulateApiCall({ server: 1 }, 300, true); // Fails
      const promise2 = simulateApiCall({ server: 2 }, 200); // Succeeds first
      const promise3 = simulateApiCall({ server: 3 }, 400); // Succeeds later

      return Promise.any([promise1, promise2, promise3])
        .then(result => {
          cy.log('First successful promise:', JSON.stringify(result));
          expect(result.data.server).to.equal(2);
        });
    });
  });

  it('should create a promise-based retry mechanism', () => {
    let attempts = 0;
    const maxAttempts = 3;

    const attemptOperation = () => {
      attempts++;
      cy.log(`Attempt ${attempts}/${maxAttempts}`);

      return new Promise((resolve, reject) => {
        setTimeout(() => {
          if (attempts < 3) {
            reject(new Error(`Attempt ${attempts} failed`));
          } else {
            resolve({ success: true, attempts: attempts });
          }
        }, 100);
      });
    };

    const retryOperation = (operation, maxRetries) => {
      return operation().catch(error => {
        if (maxRetries <= 1) {
          throw error;
        }
        cy.log('Retrying after failure...');
        return retryOperation(operation, maxRetries - 1);
      });
    };

    cy.wrap(null).then(() => {
      return retryOperation(attemptOperation, maxAttempts)
        .then(result => {
          cy.log('Operation succeeded:', JSON.stringify(result));
          expect(result.success).to.be.true;
          expect(result.attempts).to.equal(3);
        });
    });
  });

  it('should simulate Cypress-like promise chain', () => {
    // Simulating a login flow with promises
    cy.wrap(null).then(() => {
      return simulateApiCall({ action: 'navigate', url: '/login' }, 100)
        .then(navResult => {
          cy.log('Navigation complete:', JSON.stringify(navResult));
          return simulateApiCall({ action: 'type', field: 'username', value: 'testuser' }, 50);
        })
        .then(typeResult1 => {
          cy.log('Username entered:', JSON.stringify(typeResult1));
          return simulateApiCall({ action: 'type', field: 'password', value: 'password123' }, 50);
        })
        .then(typeResult2 => {
          cy.log('Password entered:', JSON.stringify(typeResult2));
          return simulateApiCall({ action: 'click', element: 'login-button' }, 100);
        })
        .then(clickResult => {
          cy.log('Login button clicked:', JSON.stringify(clickResult));
          return simulateApiCall({ action: 'verify', url: '/dashboard' }, 200);
        })
        .then(verifyResult => {
          cy.log('Verification complete:', JSON.stringify(verifyResult));
          expect(verifyResult.success).to.be.true;
        })
        .catch(error => {
          cy.log('Login flow failed:', error.message);
          throw error;
        });
    });
  });

  it('should use finally block', () => {
    let cleanupExecuted = false;

    cy.wrap(null).then(() => {
      return simulateApiCall({ test: 'data' }, 100)
        .then(result => {
          cy.log('Operation succeeded:', JSON.stringify(result));
          expect(result.success).to.be.true;
        })
        .catch(error => {
          cy.log('Operation failed:', error.message);
        })
        .finally(() => {
          cleanupExecuted = true;
          cy.log('Cleanup executed');
        })
        .then(() => {
          expect(cleanupExecuted).to.be.true;
        });
    });
  });
});
```

### Deliverables Checklist
- [ ] Created and resolved Promises
- [ ] Handled Promise rejection
- [ ] Chained Promises with .then()
- [ ] Used Promise.all() for parallel operations
- [ ] Used Promise.race() for competitive operations
- [ ] Used Promise.allSettled()
- [ ] Implemented retry mechanism with Promises
- [ ] Used .finally() for cleanup
- [ ] All tests pass

### Extension Challenges
1. Create a Promise-based timeout function
2. Implement a Promise queue that executes operations sequentially
3. Build a circuit breaker pattern using Promises
4. Create a caching mechanism with Promise memoization

---

## Exercise 7: Async/Await in Test Automation {#exercise-7}

### Objective
Master async/await syntax for writing cleaner asynchronous test code.

### Scenario
Refactor Promise-based code to use async/await for better readability in Cypress tests.

### Step-by-Step Instructions

1. Create spec file: `cypress/e2e/07-async-await.cy.js`
2. Convert Promise chains to async/await
3. Handle errors with try/catch
4. Use async/await with Cypress commands
5. Combine async/await with Promise utilities

### Code Template

```javascript
describe('Exercise 7: Async/Await', () => {

  it('should use async/await syntax', async () => {
    // TODO: Use async/await instead of .then()
  });

  it('should handle errors with try/catch', async () => {
    // TODO: Implement error handling
  });
});
```

### Expected Output
- Async functions execute correctly
- Error handling works
- Code is more readable

### Solution Code

```javascript
describe('Exercise 7: Async/Await in Test Automation', () => {

  // Helper function that returns a promise
  const fetchUserData = (userId) => {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        if (userId > 0) {
          resolve({
            id: userId,
            username: `user${userId}`,
            email: `user${userId}@example.com`,
            role: 'user'
          });
        } else {
          reject(new Error('Invalid user ID'));
        }
      }, 100);
    });
  };

  const fetchUserOrders = (userId) => {
    return new Promise((resolve) => {
      setTimeout(() => {
        resolve({
          userId: userId,
          orders: [
            { id: 1, total: 99.99, status: 'completed' },
            { id: 2, total: 149.99, status: 'pending' }
          ]
        });
      }, 150);
    });
  };

  const processPayment = (orderId, amount) => {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        if (amount > 0) {
          resolve({
            orderId: orderId,
            amount: amount,
            status: 'success',
            transactionId: `TXN${Date.now()}`
          });
        } else {
          reject(new Error('Invalid payment amount'));
        }
      }, 200);
    });
  };

  it('should use basic async/await syntax', async () => {
    // Using async/await instead of .then()
    const user = await fetchUserData(1);

    cy.log('User data:', JSON.stringify(user));
    expect(user.id).to.equal(1);
    expect(user.username).to.equal('user1');
  });

  it('should handle sequential async operations', async () => {
    // Sequential operations with async/await
    const user = await fetchUserData(2);
    cy.log('Fetched user:', JSON.stringify(user));

    const orders = await fetchUserOrders(user.id);
    cy.log('Fetched orders:', JSON.stringify(orders));

    expect(user.id).to.equal(2);
    expect(orders.orders).to.have.length(2);
  });

  it('should handle errors with try/catch', async () => {
    try {
      const user = await fetchUserData(-1); // Invalid ID
      cy.log('This should not execute');
    } catch (error) {
      cy.log('Error caught:', error.message);
      expect(error.message).to.equal('Invalid user ID');
    }
  });

  it('should handle multiple async operations', async () => {
    try {
      // Fetch user
      const user = await fetchUserData(3);
      cy.log('Step 1 - User fetched:', user.username);

      // Fetch orders
      const orders = await fetchUserOrders(user.id);
      cy.log('Step 2 - Orders fetched:', orders.orders.length);

      // Process first order payment
      const payment = await processPayment(orders.orders[0].id, orders.orders[0].total);
      cy.log('Step 3 - Payment processed:', payment.transactionId);

      expect(user.id).to.equal(3);
      expect(orders.orders).to.have.length(2);
      expect(payment.status).to.equal('success');
    } catch (error) {
      cy.log('Error in flow:', error.message);
      throw error;
    }
  });

  it('should use Promise.all with async/await', async () => {
    // Parallel operations
    const [user1, user2, user3] = await Promise.all([
      fetchUserData(1),
      fetchUserData(2),
      fetchUserData(3)
    ]);

    cy.log('User 1:', user1.username);
    cy.log('User 2:', user2.username);
    cy.log('User 3:', user3.username);

    expect(user1.id).to.equal(1);
    expect(user2.id).to.equal(2);
    expect(user3.id).to.equal(3);
  });

  it('should combine async/await with Promise.allSettled', async () => {
    const results = await Promise.allSettled([
      fetchUserData(1),
      fetchUserData(-1), // This will fail
      fetchUserData(2)
    ]);

    cy.log('Results:', JSON.stringify(results, null, 2));

    expect(results[0].status).to.equal('fulfilled');
    expect(results[1].status).to.equal('rejected');
    expect(results[2].status).to.equal('fulfilled');

    const successful = results.filter(r => r.status === 'fulfilled');
    expect(successful).to.have.length(2);
  });

  it('should create async utility functions', async () => {
    // Async function to login user
    const loginUser = async (username, password) => {
      cy.log(`Logging in user: ${username}`);

      // Simulate API calls
      const authToken = await new Promise(resolve => {
        setTimeout(() => resolve(`token_${username}_${Date.now()}`), 100);
      });

      const userData = await fetchUserData(1);

      return {
        token: authToken,
        user: userData
      };
    };

    const loginResult = await loginUser('testuser', 'password123');

    expect(loginResult.token).to.include('token_testuser');
    expect(loginResult.user.username).to.equal('user1');
    cy.log('Login successful:', JSON.stringify(loginResult));
  });

  it('should implement retry logic with async/await', async () => {
    let attempts = 0;

    const unreliableOperation = async () => {
      attempts++;
      if (attempts < 3) {
        throw new Error(`Attempt ${attempts} failed`);
      }
      return { success: true, attempts };
    };

    const retryAsync = async (operation, maxRetries = 3, delay = 100) => {
      for (let i = 0; i < maxRetries; i++) {
        try {
          cy.log(`Attempt ${i + 1} of ${maxRetries}`);
          return await operation();
        } catch (error) {
          if (i === maxRetries - 1) throw error;
          cy.log(`Retry after ${delay}ms...`);
          await new Promise(resolve => setTimeout(resolve, delay));
        }
      }
    };

    const result = await retryAsync(unreliableOperation, 5);
    expect(result.success).to.be.true;
    expect(result.attempts).to.equal(3);
    cy.log('Operation succeeded after retries');
  });

  it('should use async/await in forEach alternative', async () => {
    const userIds = [1, 2, 3];
    const users = [];

    // Cannot use forEach with async/await, use for...of
    for (const userId of userIds) {
      const user = await fetchUserData(userId);
      users.push(user);
      cy.log(`Fetched user ${userId}:`, user.username);
    }

    expect(users).to.have.length(3);
    expect(users[0].id).to.equal(1);
    expect(users[2].id).to.equal(3);
  });

  it('should handle parallel and sequential mixed operations', async () => {
    // Parallel: Fetch multiple users
    const [user1, user2] = await Promise.all([
      fetchUserData(1),
      fetchUserData(2)
    ]);

    cy.log('Fetched users in parallel');

    // Sequential: Fetch orders for each
    const orders1 = await fetchUserOrders(user1.id);
    cy.log(`User ${user1.id} orders:`, orders1.orders.length);

    const orders2 = await fetchUserOrders(user2.id);
    cy.log(`User ${user2.id} orders:`, orders2.orders.length);

    expect(user1.id).to.equal(1);
    expect(user2.id).to.equal(2);
    expect(orders1.orders).to.have.length(2);
    expect(orders2.orders).to.have.length(2);
  });

  it('should implement timeout with async/await', async () => {
    const withTimeout = (promise, timeoutMs) => {
      return Promise.race([
        promise,
        new Promise((_, reject) =>
          setTimeout(() => reject(new Error('Operation timed out')), timeoutMs)
        )
      ]);
    };

    const slowOperation = new Promise(resolve => {
      setTimeout(() => resolve('Success'), 500);
    });

    try {
      // This should timeout
      await withTimeout(slowOperation, 200);
      cy.log('Should not reach here');
    } catch (error) {
      expect(error.message).to.equal('Operation timed out');
      cy.log('Timeout handled correctly');
    }
  });

  it('should chain async operations elegantly', async () => {
    // Clean async/await chain
    const executeTestFlow = async () => {
      const user = await fetchUserData(1);
      const orders = await fetchUserOrders(user.id);
      const firstOrder = orders.orders[0];
      const payment = await processPayment(firstOrder.id, firstOrder.total);

      return {
        user,
        orders: orders.orders,
        payment
      };
    };

    const result = await executeTestFlow();

    cy.log('Test flow result:', JSON.stringify(result, null, 2));

    expect(result.user.id).to.equal(1);
    expect(result.orders).to.have.length(2);
    expect(result.payment.status).to.equal('success');
  });

  it('should use async/await with Cypress tasks', () => {
    // Note: In real Cypress, we wrap async operations
    cy.wrap(null).then(async () => {
      const user = await fetchUserData(5);
      expect(user.id).to.equal(5);

      const orders = await fetchUserOrders(user.id);
      expect(orders.orders).to.have.length(2);

      cy.log('Async operations in Cypress completed');
    });
  });

  it('should implement async data processor', async () => {
    const processTestResults = async (testIds) => {
      const results = [];

      for (const testId of testIds) {
        try {
          const data = await fetchUserData(testId);
          results.push({ id: testId, status: 'success', data });
        } catch (error) {
          results.push({ id: testId, status: 'error', error: error.message });
        }
      }

      return results;
    };

    const testIds = [1, 2, -1, 3]; // -1 will fail
    const results = await processTestResults(testIds);

    cy.log('Process results:', JSON.stringify(results, null, 2));

    expect(results).to.have.length(4);
    expect(results[0].status).to.equal('success');
    expect(results[2].status).to.equal('error');
  });
});
```

### Deliverables Checklist
- [ ] Used async/await syntax
- [ ] Handled errors with try/catch
- [ ] Performed sequential async operations
- [ ] Combined async/await with Promise.all()
- [ ] Implemented retry logic
- [ ] Created async utility functions
- [ ] Used for...of with async/await
- [ ] Implemented timeout mechanism
- [ ] All tests pass

### Extension Challenges
1. Create an async queue that processes items one at a time
2. Implement a rate limiter for API calls using async/await
3. Build an async cache with expiration
4. Create a parallel batch processor with concurrency limits

---

## Exercise 8: ES6 Features - Destructuring and Spread {#exercise-8}

### Objective
Master ES6 destructuring and spread operators for cleaner test code.

### Scenario
Extract data from API responses, clone test configurations, and merge test data efficiently.

### Step-by-Step Instructions

1. Create spec file: `cypress/e2e/08-es6-features.cy.js`
2. Practice array destructuring
3. Practice object destructuring
4. Use spread operator with arrays
5. Use spread operator with objects
6. Apply to real Cypress testing scenarios

### Code Template

```javascript
describe('Exercise 8: ES6 Features', () => {

  it('should use array destructuring', () => {
    // TODO: Destructure arrays
  });

  it('should use object destructuring', () => {
    // TODO: Destructure objects
  });

  it('should use spread operator', () => {
    // TODO: Use spread with arrays and objects
  });
});
```

### Expected Output
- Destructuring extracts values correctly
- Spread operators clone and merge properly
- Code is concise and readable

### Solution Code

```javascript
describe('Exercise 8: ES6 Features - Destructuring and Spread', () => {

  it('should use array destructuring basics', () => {
    const testResults = ['pass', 'fail', 'skip', 'pass', 'pass'];

    // Basic destructuring
    const [first, second] = testResults;
    expect(first).to.equal('pass');
    expect(second).to.equal('fail');

    // Skip elements
    const [, , third] = testResults;
    expect(third).to.equal('skip');

    // Rest operator
    const [firstResult, ...remaining] = testResults;
    expect(firstResult).to.equal('pass');
    expect(remaining).to.have.length(4);

    cy.log('First:', first);
    cy.log('Remaining:', JSON.stringify(remaining));
  });

  it('should use array destructuring with default values', () => {
    const browserList = ['chrome', 'firefox'];

    // With defaults
    const [primary = 'chrome', secondary = 'firefox', tertiary = 'edge'] = browserList;

    expect(primary).to.equal('chrome');
    expect(secondary).to.equal('firefox');
    expect(tertiary).to.equal('edge'); // Default value used

    cy.log('Browsers:', primary, secondary, tertiary);
  });

  it('should use object destructuring basics', () => {
    const testConfig = {
      browser: 'chrome',
      viewport: { width: 1920, height: 1080 },
      timeout: 5000,
      baseUrl: 'https://example.com'
    };

    // Basic destructuring
    const { browser, timeout } = testConfig;
    expect(browser).to.equal('chrome');
    expect(timeout).to.equal(5000);

    // Rename variables
    const { baseUrl: url } = testConfig;
    expect(url).to.equal('https://example.com');

    // Default values
    const { retries = 3 } = testConfig;
    expect(retries).to.equal(3);

    cy.log('Config:', browser, timeout, url);
  });

  it('should use nested destructuring', () => {
    const apiResponse = {
      status: 200,
      data: {
        user: {
          id: 1,
          name: 'John Doe',
          email: 'john@example.com',
          address: {
            city: 'New York',
            country: 'USA'
          }
        },
        token: 'abc123'
      }
    };

    // Nested destructuring
    const {
      status,
      data: {
        user: {
          name,
          email,
          address: { city }
        },
        token
      }
    } = apiResponse;

    expect(status).to.equal(200);
    expect(name).to.equal('John Doe');
    expect(email).to.equal('john@example.com');
    expect(city).to.equal('New York');
    expect(token).to.equal('abc123');

    cy.log('User:', name, 'from', city);
  });

  it('should use destructuring in function parameters', () => {
    // Function with destructured parameters
    const runTest = ({ testName, browser = 'chrome', timeout = 5000 }) => {
      cy.log(`Running: ${testName} on ${browser} (timeout: ${timeout}ms)`);
      return {
        name: testName,
        browser,
        timeout,
        status: 'completed'
      };
    };

    const result1 = runTest({ testName: 'Login Test', browser: 'firefox' });
    expect(result1.browser).to.equal('firefox');
    expect(result1.timeout).to.equal(5000); // Default

    const result2 = runTest({ testName: 'Checkout Test', timeout: 10000 });
    expect(result2.browser).to.equal('chrome'); // Default
    expect(result2.timeout).to.equal(10000);
  });

  it('should use array spread operator', () => {
    const smokeTests = ['login', 'logout'];
    const regressionTests = ['checkout', 'payment', 'search'];

    // Combine arrays
    const allTests = [...smokeTests, ...regressionTests];
    expect(allTests).to.have.length(5);
    expect(allTests).to.deep.equal(['login', 'logout', 'checkout', 'payment', 'search']);

    // Clone array
    const testsCopy = [...allTests];
    testsCopy.push('newTest');
    expect(allTests).to.have.length(5); // Original unchanged
    expect(testsCopy).to.have.length(6);

    // Add items while spreading
    const extendedTests = ['homepage', ...allTests, 'cleanup'];
    expect(extendedTests[0]).to.equal('homepage');
    expect(extendedTests[extendedTests.length - 1]).to.equal('cleanup');

    cy.log('All tests:', JSON.stringify(allTests));
  });

  it('should use object spread operator', () => {
    const defaultConfig = {
      browser: 'chrome',
      headless: false,
      viewport: { width: 1920, height: 1080 }
    };

    const customConfig = {
      headless: true,
      timeout: 10000
    };

    // Merge objects (later properties override earlier ones)
    const finalConfig = { ...defaultConfig, ...customConfig };

    expect(finalConfig.browser).to.equal('chrome'); // From default
    expect(finalConfig.headless).to.be.true; // Override from custom
    expect(finalConfig.timeout).to.equal(10000); // From custom
    expect(finalConfig.viewport.width).to.equal(1920); // From default

    // Clone object
    const configCopy = { ...finalConfig };
    configCopy.browser = 'firefox';
    expect(finalConfig.browser).to.equal('chrome'); // Original unchanged

    cy.log('Final config:', JSON.stringify(finalConfig, null, 2));
  });

  it('should use spread for function arguments', () => {
    const logTestResults = (testName, ...results) => {
      cy.log(`Test: ${testName}`);
      cy.log(`Results: ${results.join(', ')}`);
      return results.every(r => r === 'pass');
    };

    const allPassed = logTestResults('Suite 1', 'pass', 'pass', 'pass');
    expect(allPassed).to.be.true;

    const someFailed = logTestResults('Suite 2', 'pass', 'fail', 'pass');
    expect(someFailed).to.be.false;

    // Use spread to pass array as arguments
    const testResults = ['pass', 'pass', 'pass'];
    const result = logTestResults('Suite 3', ...testResults);
    expect(result).to.be.true;
  });

  it('should destructure in Cypress API responses', () => {
    // Simulate API response
    const apiResponse = {
      status: 200,
      data: {
        users: [
          { id: 1, name: 'Alice', role: 'admin' },
          { id: 2, name: 'Bob', role: 'user' },
          { id: 3, name: 'Charlie', role: 'manager' }
        ],
        totalCount: 3,
        page: 1
      }
    };

    // Destructure response
    const { data: { users, totalCount } } = apiResponse;

    expect(users).to.have.length(3);
    expect(totalCount).to.equal(3);

    // Destructure individual users
    const [admin, , manager] = users;
    expect(admin.role).to.equal('admin');
    expect(manager.role).to.equal('manager');

    cy.log('Admin user:', JSON.stringify(admin));
  });

  it('should create test configuration with spread', () => {
    const baseConfig = {
      baseUrl: 'https://example.com',
      viewportWidth: 1920,
      viewportHeight: 1080
    };

    const developmentConfig = {
      ...baseConfig,
      baseUrl: 'https://dev.example.com',
      watchForFileChanges: true
    };

    const productionConfig = {
      ...baseConfig,
      video: true,
      screenshotOnRunFailure: true
    };

    expect(developmentConfig.baseUrl).to.include('dev');
    expect(developmentConfig.viewportWidth).to.equal(1920);
    expect(productionConfig.video).to.be.true;
    expect(productionConfig.viewportWidth).to.equal(1920);

    cy.log('Dev config:', JSON.stringify(developmentConfig, null, 2));
  });

  it('should use rest and spread with test data', () => {
    const testUser = {
      id: 1,
      username: 'testuser',
      email: 'test@example.com',
      password: 'secret123',
      role: 'user',
      createdAt: '2024-01-01'
    };

    // Extract password separately (exclude from logs)
    const { password, ...userWithoutPassword } = testUser;

    cy.log('Safe to log:', JSON.stringify(userWithoutPassword));
    expect(userWithoutPassword.password).to.be.undefined;
    expect(password).to.equal('secret123');

    // Add additional properties
    const enhancedUser = {
      ...userWithoutPassword,
      lastLogin: '2024-01-15',
      isActive: true
    };

    expect(enhancedUser.lastLogin).to.exist;
    expect(enhancedUser.password).to.be.undefined;
  });

  it('should swap variables with destructuring', () => {
    let browser1 = 'chrome';
    let browser2 = 'firefox';

    cy.log(`Before swap: ${browser1}, ${browser2}`);

    // Swap using destructuring
    [browser1, browser2] = [browser2, browser1];

    cy.log(`After swap: ${browser1}, ${browser2}`);
    expect(browser1).to.equal('firefox');
    expect(browser2).to.equal('chrome');
  });

  it('should extract values from function returns', () => {
    const getTestMetrics = () => {
      return {
        total: 100,
        passed: 85,
        failed: 10,
        skipped: 5,
        duration: 45000
      };
    };

    // Destructure return value
    const { passed, failed, total } = getTestMetrics();
    const passRate = Math.round((passed / total) * 100);

    expect(passRate).to.equal(85);
    cy.log(`Pass rate: ${passRate}%`);
  });

  it('should use destructuring with loops', () => {
    const testResults = [
      { name: 'Test 1', status: 'pass', duration: 1000 },
      { name: 'Test 2', status: 'fail', duration: 2000 },
      { name: 'Test 3', status: 'pass', duration: 1500 }
    ];

    // Destructure in for...of loop
    for (const { name, status, duration } of testResults) {
      cy.log(`${name}: ${status} (${duration}ms)`);
      expect(name).to.be.a('string');
      expect(status).to.match(/^(pass|fail|skip)$/);
    }

    // Destructure with forEach
    testResults.forEach(({ name, status }) => {
      expect(name).to.exist;
      expect(status).to.exist;
    });
  });

  it('should merge nested objects with spread', () => {
    const defaultViewport = {
      width: 1920,
      height: 1080,
      deviceScaleFactor: 1
    };

    const mobileConfig = {
      viewport: {
        ...defaultViewport,
        width: 375,
        height: 667,
        isMobile: true
      }
    };

    expect(mobileConfig.viewport.width).to.equal(375);
    expect(mobileConfig.viewport.deviceScaleFactor).to.equal(1);
    expect(mobileConfig.viewport.isMobile).to.be.true;

    cy.log('Mobile config:', JSON.stringify(mobileConfig, null, 2));
  });

  it('should use practical Cypress example', () => {
    // Simulate Cypress test scenario
    const testData = {
      user: {
        email: 'test@example.com',
        password: 'Test123!',
        firstName: 'John',
        lastName: 'Doe'
      },
      product: {
        id: 'PROD-001',
        name: 'Test Product',
        price: 99.99,
        quantity: 2
      }
    };

    // Destructure for login
    const { user: { email, password } } = testData;
    cy.log(`Logging in with: ${email}`);

    // Destructure for product
    const { product: { name: productName, price, quantity } } = testData;
    const total = price * quantity;

    expect(total).to.equal(199.98);
    cy.log(`Product: ${productName}, Total: $${total}`);

    // Create order with spread
    const order = {
      ...testData.product,
      userEmail: email,
      total,
      status: 'pending',
      createdAt: new Date().toISOString()
    };

    expect(order.userEmail).to.equal(email);
    expect(order.total).to.equal(total);
    cy.log('Order created:', JSON.stringify(order, null, 2));
  });
});
```

### Deliverables Checklist
- [ ] Used array destructuring with rest operator
- [ ] Used object destructuring with defaults
- [ ] Practiced nested destructuring
- [ ] Used destructuring in function parameters
- [ ] Applied array spread operator
- [ ] Applied object spread operator
- [ ] Combined rest and spread patterns
- [ ] Used in real Cypress scenarios
- [ ] All tests pass

### Extension Challenges
1. Create a configuration merger that deep merges nested objects
2. Implement a test data sanitizer that removes sensitive fields
3. Build a response parser that extracts and transforms nested API data
4. Create a test suite generator using spread and destructuring

---

## Exercise 9: Working with JSON Test Data {#exercise-9}

### Objective
Master JSON parsing, stringification, and manipulation for test data management.

### Scenario
Load test data from JSON, parse API responses, create test fixtures, and validate JSON structures.

### Step-by-Step Instructions

1. Create spec file: `cypress/e2e/09-json-data.cy.js`
2. Create JSON fixtures for test data
3. Parse and stringify JSON
4. Load JSON from files using Cypress
5. Validate JSON structure
6. Transform JSON data

### Code Template

```javascript
describe('Exercise 9: JSON Test Data', () => {

  it('should parse and stringify JSON', () => {
    // TODO: Convert between JSON and objects
  });

  it('should load JSON fixtures', () => {
    // TODO: Use cy.fixture() to load data
  });

  it('should validate JSON structure', () => {
    // TODO: Check JSON properties and types
  });
});
```

### Expected Output
- JSON parsing works correctly
- Fixtures load successfully
- JSON validation passes

### Solution Code

First, create test fixtures. Create these files in `cypress/fixtures/`:

**cypress/fixtures/users.json:**
```json
{
  "admin": {
    "username": "admin@example.com",
    "password": "Admin123!",
    "role": "admin",
    "permissions": ["read", "write", "delete"]
  },
  "user": {
    "username": "user@example.com",
    "password": "User123!",
    "role": "user",
    "permissions": ["read"]
  },
  "manager": {
    "username": "manager@example.com",
    "password": "Manager123!",
    "role": "manager",
    "permissions": ["read", "write"]
  }
}
```

**cypress/fixtures/products.json:**
```json
[
  {
    "id": "PROD-001",
    "name": "Laptop",
    "category": "Electronics",
    "price": 999.99,
    "inStock": true,
    "tags": ["computers", "tech"]
  },
  {
    "id": "PROD-002",
    "name": "Mouse",
    "category": "Accessories",
    "price": 29.99,
    "inStock": true,
    "tags": ["accessories", "peripherals"]
  },
  {
    "id": "PROD-003",
    "name": "Monitor",
    "category": "Electronics",
    "price": 299.99,
    "inStock": false,
    "tags": ["displays", "tech"]
  }
]
```

Now the spec file:

```javascript
describe('Exercise 9: Working with JSON Test Data', () => {

  it('should parse and stringify JSON', () => {
    // JavaScript object
    const testData = {
      name: 'Login Test',
      status: 'pass',
      duration: 2500,
      tags: ['smoke', 'critical']
    };

    // Convert to JSON string
    const jsonString = JSON.stringify(testData);
    cy.log('JSON String:', jsonString);
    expect(jsonString).to.be.a('string');
    expect(jsonString).to.include('"name":"Login Test"');

    // Pretty print JSON
    const prettyJson = JSON.stringify(testData, null, 2);
    cy.log('Pretty JSON:', prettyJson);

    // Parse JSON string back to object
    const parsedData = JSON.parse(jsonString);
    expect(parsedData).to.deep.equal(testData);
    expect(parsedData.name).to.equal('Login Test');
  });

  it('should handle JSON parsing errors', () => {
    const invalidJson = '{"name": "Test", invalid}';

    try {
      JSON.parse(invalidJson);
      cy.log('Should not reach here');
    } catch (error) {
      cy.log('Parse error caught:', error.message);
      expect(error).to.be.instanceOf(SyntaxError);
    }
  });

  it('should use JSON.stringify with replacer', () => {
    const userData = {
      id: 1,
      username: 'testuser',
      password: 'secret123',
      email: 'test@example.com',
      role: 'user'
    };

    // Exclude password from JSON
    const safeJson = JSON.stringify(userData, (key, value) => {
      if (key === 'password') return undefined;
      return value;
    }, 2);

    cy.log('Safe JSON:', safeJson);
    expect(safeJson).to.not.include('secret123');
    expect(safeJson).to.include('testuser');

    // Only include specific properties
    const limitedJson = JSON.stringify(userData, ['username', 'email', 'role'], 2);
    cy.log('Limited JSON:', limitedJson);
    expect(limitedJson).to.not.include('password');
    expect(limitedJson).to.not.include('"id"');
  });

  it('should load JSON fixtures with cy.fixture()', () => {
    cy.fixture('users.json').then((users) => {
      cy.log('Loaded users:', JSON.stringify(users, null, 2));

      // Access specific user
      expect(users.admin.username).to.equal('admin@example.com');
      expect(users.user.role).to.equal('user');
      expect(users.manager.permissions).to.include('write');

      // Verify structure
      expect(users).to.have.property('admin');
      expect(users).to.have.property('user');
      expect(users).to.have.property('manager');
    });
  });

  it('should load and iterate through array fixtures', () => {
    cy.fixture('products.json').then((products) => {
      cy.log('Loaded products:', JSON.stringify(products, null, 2));

      expect(products).to.be.an('array');
      expect(products).to.have.length(3);

      // Find specific product
      const laptop = products.find(p => p.name === 'Laptop');
      expect(laptop.price).to.equal(999.99);
      expect(laptop.inStock).to.be.true;

      // Filter products
      const inStockProducts = products.filter(p => p.inStock);
      expect(inStockProducts).to.have.length(2);

      // Map to get names
      const productNames = products.map(p => p.name);
      expect(productNames).to.deep.equal(['Laptop', 'Mouse', 'Monitor']);
    });
  });

  it('should validate JSON structure', () => {
    const apiResponse = {
      status: 'success',
      data: {
        user: {
          id: 1,
          email: 'test@example.com'
        },
        token: 'abc123'
      },
      timestamp: '2024-01-01T00:00:00Z'
    };

    // Validate top-level properties
    expect(apiResponse).to.have.property('status');
    expect(apiResponse).to.have.property('data');
    expect(apiResponse).to.have.property('timestamp');

    // Validate nested properties
    expect(apiResponse.data).to.have.property('user');
    expect(apiResponse.data).to.have.property('token');
    expect(apiResponse.data.user).to.have.property('id');
    expect(apiResponse.data.user).to.have.property('email');

    // Validate types
    expect(apiResponse.status).to.be.a('string');
    expect(apiResponse.data.user.id).to.be.a('number');
    expect(apiResponse.timestamp).to.be.a('string');
  });

  it('should create custom JSON validator', () => {
    const validateUserSchema = (user) => {
      const errors = [];

      if (!user.username || typeof user.username !== 'string') {
        errors.push('Invalid username');
      }
      if (!user.email || !user.email.includes('@')) {
        errors.push('Invalid email');
      }
      if (!user.role || !['admin', 'user', 'manager'].includes(user.role)) {
        errors.push('Invalid role');
      }
      if (!Array.isArray(user.permissions)) {
        errors.push('Permissions must be an array');
      }

      return {
        valid: errors.length === 0,
        errors
      };
    };

    // Valid user
    const validUser = {
      username: 'testuser',
      email: 'test@example.com',
      role: 'user',
      permissions: ['read']
    };

    const result1 = validateUserSchema(validUser);
    expect(result1.valid).to.be.true;
    expect(result1.errors).to.have.length(0);

    // Invalid user
    const invalidUser = {
      username: 'testuser',
      email: 'invalid-email',
      role: 'superadmin',
      permissions: 'read'
    };

    const result2 = validateUserSchema(invalidUser);
    expect(result2.valid).to.be.false;
    expect(result2.errors).to.have.length.greaterThan(0);
    cy.log('Validation errors:', JSON.stringify(result2.errors));
  });

  it('should transform JSON data', () => {
    const rawApiData = [
      { userId: 1, userName: 'John', userEmail: 'john@example.com' },
      { userId: 2, userName: 'Jane', userEmail: 'jane@example.com' },
      { userId: 3, userName: 'Bob', userEmail: 'bob@example.com' }
    ];

    // Transform to different format
    const transformedData = rawApiData.map(item => ({
      id: item.userId,
      name: item.userName,
      email: item.userEmail,
      displayName: `${item.userName} (${item.userEmail})`
    }));

    expect(transformedData[0]).to.have.property('displayName');
    expect(transformedData[0].displayName).to.equal('John (john@example.com)');

    cy.log('Transformed data:', JSON.stringify(transformedData, null, 2));
  });

  it('should merge JSON configurations', () => {
    const defaultConfig = {
      timeout: 5000,
      retries: 2,
      viewport: {
        width: 1920,
        height: 1080
      },
      env: 'staging'
    };

    const userConfig = {
      timeout: 10000,
      viewport: {
        width: 1366
      },
      video: true
    };

    // Deep merge (simple version)
    const mergedConfig = {
      ...defaultConfig,
      ...userConfig,
      viewport: {
        ...defaultConfig.viewport,
        ...userConfig.viewport
      }
    };

    expect(mergedConfig.timeout).to.equal(10000); // Overridden
    expect(mergedConfig.retries).to.equal(2); // From default
    expect(mergedConfig.viewport.width).to.equal(1366); // Overridden
    expect(mergedConfig.viewport.height).to.equal(1080); // From default
    expect(mergedConfig.video).to.be.true; // From user

    cy.log('Merged config:', JSON.stringify(mergedConfig, null, 2));
  });

  it('should work with nested JSON arrays', () => {
    const testSuites = {
      smoke: [
        { name: 'Login', priority: 1 },
        { name: 'Logout', priority: 1 }
      ],
      regression: [
        { name: 'Checkout', priority: 2 },
        { name: 'Search', priority: 2 },
        { name: 'Wishlist', priority: 3 }
      ]
    };

    // Flatten all tests
    const allTests = Object.values(testSuites).flat();
    expect(allTests).to.have.length(5);

    // Filter by priority
    const highPriority = allTests.filter(test => test.priority === 1);
    expect(highPriority).to.have.length(2);

    // Group by priority
    const byPriority = allTests.reduce((acc, test) => {
      if (!acc[test.priority]) acc[test.priority] = [];
      acc[test.priority].push(test.name);
      return acc;
    }, {});

    cy.log('Grouped by priority:', JSON.stringify(byPriority, null, 2));
    expect(byPriority[1]).to.deep.equal(['Login', 'Logout']);
  });

  it('should create test data generator', () => {
    const generateTestData = (count) => {
      return Array.from({ length: count }, (_, index) => ({
        id: index + 1,
        username: `user${index + 1}`,
        email: `user${index + 1}@example.com`,
        createdAt: new Date().toISOString(),
        isActive: Math.random() > 0.5
      }));
    };

    const testUsers = generateTestData(5);
    const json = JSON.stringify(testUsers, null, 2);

    cy.log('Generated test data:', json);
    expect(testUsers).to.have.length(5);
    expect(testUsers[0].username).to.equal('user1');
    expect(testUsers[4].username).to.equal('user5');
  });

  it('should handle deeply nested JSON', () => {
    const complexData = {
      level1: {
        level2: {
          level3: {
            level4: {
              value: 'deep value',
              array: [1, 2, 3]
            }
          }
        }
      }
    };

    // Access deep value
    const deepValue = complexData.level1.level2.level3.level4.value;
    expect(deepValue).to.equal('deep value');

    // Safe access with optional chaining
    const safeValue = complexData?.level1?.level2?.level3?.level4?.value;
    expect(safeValue).to.equal('deep value');

    // Non-existent path returns undefined
    const missingValue = complexData?.level1?.missing?.level3?.value;
    expect(missingValue).to.be.undefined;

    cy.log('Deep value:', deepValue);
  });

  it('should compare JSON objects', () => {
    const expected = {
      id: 1,
      name: 'Test',
      tags: ['a', 'b']
    };

    const actual = {
      id: 1,
      name: 'Test',
      tags: ['a', 'b']
    };

    // Deep equality
    expect(actual).to.deep.equal(expected);

    // Convert to JSON strings for comparison
    const expectedJson = JSON.stringify(expected);
    const actualJson = JSON.stringify(actual);
    expect(actualJson).to.equal(expectedJson);
  });

  it('should use JSON in API mocking scenario', () => {
    // Mock API response
    const mockResponse = {
      status: 200,
      body: {
        success: true,
        data: {
          products: [
            { id: 1, name: 'Product 1', price: 10.99 },
            { id: 2, name: 'Product 2', price: 20.99 }
          ]
        }
      }
    };

    // Simulate API intercept
    cy.wrap(mockResponse).then((response) => {
      expect(response.status).to.equal(200);
      expect(response.body.success).to.be.true;
      expect(response.body.data.products).to.have.length(2);

      cy.log('Mock response:', JSON.stringify(response.body, null, 2));
    });
  });
});
```

### Deliverables Checklist
- [ ] Parsed and stringified JSON
- [ ] Loaded JSON fixtures with cy.fixture()
- [ ] Validated JSON structure
- [ ] Used JSON.stringify with replacer
- [ ] Transformed JSON data
- [ ] Merged JSON configurations
- [ ] Created test data generators
- [ ] Handled nested JSON
- [ ] Compared JSON objects
- [ ] All tests pass

### Extension Challenges
1. Create a JSON schema validator using a validation library
2. Build a test data factory that generates complex nested JSON
3. Implement a JSON diff tool that shows differences between objects
4. Create a fixture manager that loads and caches multiple JSON files

---

## Exercise 10: Creating Custom Cypress Commands {#exercise-10}

### Objective
Create reusable custom Cypress commands to improve test maintainability.

### Scenario
Build custom commands for common operations like login, navigation, form filling, and assertions.

### Step-by-Step Instructions

1. Create file: `cypress/support/commands.js`
2. Create simple custom commands
3. Create commands with parameters
4. Create commands that chain
5. Use custom commands in tests

### Code Template

**cypress/support/commands.js:**
```javascript
// TODO: Add custom commands here
```

**cypress/e2e/10-custom-commands.cy.js:**
```javascript
describe('Exercise 10: Custom Commands', () => {

  it('should use custom commands', () => {
    // TODO: Use your custom commands
  });
});
```

### Expected Output
- Custom commands are defined
- Commands execute correctly
- Tests are more readable

### Solution Code

**cypress/support/commands.js:**
```javascript
// ================================================
// Custom Cypress Commands for Exercise 10
// ================================================

// Simple custom command - Login
Cypress.Commands.add('login', (username, password) => {
  cy.log(`Logging in as: ${username}`);
  cy.visit('/login');
  cy.get('[data-cy="username"]').type(username);
  cy.get('[data-cy="password"]').type(password);
  cy.get('[data-cy="login-button"]').click();
  cy.url().should('include', '/dashboard');
  cy.log('Login successful');
});

// Command with default parameters
Cypress.Commands.add('loginAsRole', (role = 'user') => {
  const credentials = {
    admin: { username: 'admin@example.com', password: 'Admin123!' },
    user: { username: 'user@example.com', password: 'User123!' },
    manager: { username: 'manager@example.com', password: 'Manager123!' }
  };

  const { username, password } = credentials[role];
  cy.login(username, password);
});

// Command with options object
Cypress.Commands.add('loginWithOptions', (options = {}) => {
  const {
    username = 'default@example.com',
    password = 'Default123!',
    rememberMe = false,
    redirectUrl = '/dashboard'
  } = options;

  cy.visit('/login');
  cy.get('[data-cy="username"]').type(username);
  cy.get('[data-cy="password"]').type(password);

  if (rememberMe) {
    cy.get('[data-cy="remember-me"]').check();
  }

  cy.get('[data-cy="login-button"]').click();
  cy.url().should('include', redirectUrl);
});

// Command that returns a value
Cypress.Commands.add('generateTestUser', () => {
  const timestamp = Date.now();
  return {
    username: `testuser_${timestamp}`,
    email: `testuser_${timestamp}@example.com`,
    password: 'Test123!',
    firstName: 'Test',
    lastName: 'User'
  };
});

// Command for form filling
Cypress.Commands.add('fillForm', (formData) => {
  Object.entries(formData).forEach(([field, value]) => {
    cy.get(`[data-cy="${field}"]`).clear().type(value);
  });
  cy.log('Form filled successfully');
});

// Command for API login (sets token)
Cypress.Commands.add('loginViaAPI', (username, password) => {
  cy.log(`API Login: ${username}`);

  cy.request({
    method: 'POST',
    url: '/api/login',
    body: { username, password }
  }).then((response) => {
    expect(response.status).to.eq(200);
    const token = response.body.token;

    // Store token in localStorage
    window.localStorage.setItem('authToken', token);
    cy.log('Token stored:', token);
  });
});

// Child command (chainable)
Cypress.Commands.add('assertText', { prevSubject: true }, (subject, expectedText) => {
  cy.wrap(subject).should('have.text', expectedText);
  cy.log(`Asserted text: ${expectedText}`);
  return subject; // Return for further chaining
});

// Child command for custom assertion
Cypress.Commands.add('shouldBeVisible', { prevSubject: true }, (subject) => {
  cy.wrap(subject).should('be.visible');
  cy.log('Element is visible');
  return subject;
});

// Command for waiting
Cypress.Commands.add('waitForElement', (selector, timeout = 10000) => {
  cy.get(selector, { timeout }).should('exist').and('be.visible');
  cy.log(`Element found: ${selector}`);
});

// Command for multiple assertions
Cypress.Commands.add('assertElementState', (selector, expectedState) => {
  const element = cy.get(selector);

  if (expectedState.visible !== undefined) {
    element.should(expectedState.visible ? 'be.visible' : 'not.be.visible');
  }
  if (expectedState.enabled !== undefined) {
    element.should(expectedState.enabled ? 'be.enabled' : 'be.disabled');
  }
  if (expectedState.text) {
    element.should('contain.text', expectedState.text);
  }
  if (expectedState.value) {
    element.should('have.value', expectedState.value);
  }

  cy.log('Element state verified');
});

// Command for navigation
Cypress.Commands.add('navigateTo', (page) => {
  const routes = {
    home: '/',
    dashboard: '/dashboard',
    products: '/products',
    cart: '/cart',
    checkout: '/checkout',
    profile: '/profile'
  };

  const url = routes[page] || page;
  cy.visit(url);
  cy.log(`Navigated to: ${page} (${url})`);
});

// Command for test data cleanup
Cypress.Commands.add('cleanupTestData', (dataType) => {
  cy.log(`Cleaning up: ${dataType}`);

  cy.request({
    method: 'DELETE',
    url: `/api/test-data/${dataType}`,
    failOnStatusCode: false
  }).then((response) => {
    cy.log(`Cleanup response: ${response.status}`);
  });
});

// Command for screenshot with custom name
Cypress.Commands.add('takeScreenshot', (name) => {
  const timestamp = new Date().toISOString().replace(/[:.]/g, '-');
  const filename = `${name}_${timestamp}`;
  cy.screenshot(filename);
  cy.log(`Screenshot saved: ${filename}`);
});

// Command for retry mechanism
Cypress.Commands.add('retryUntil', (selector, assertion, maxAttempts = 5) => {
  let attempts = 0;

  const attemptAssertion = () => {
    attempts++;
    cy.log(`Attempt ${attempts}/${maxAttempts}`);

    cy.get(selector).then(($element) => {
      try {
        assertion($element);
      } catch (error) {
        if (attempts < maxAttempts) {
          cy.wait(1000);
          attemptAssertion();
        } else {
          throw error;
        }
      }
    });
  };

  attemptAssertion();
});

// Command for table operations
Cypress.Commands.add('getTableRow', (tableSelector, rowIndex) => {
  return cy.get(`${tableSelector} tbody tr`).eq(rowIndex);
});

Cypress.Commands.add('getTableCell', (tableSelector, row, column) => {
  return cy.get(`${tableSelector} tbody tr`).eq(row).find('td').eq(column);
});

// Command for local storage operations
Cypress.Commands.add('setLocalStorage', (key, value) => {
  window.localStorage.setItem(key, JSON.stringify(value));
  cy.log(`LocalStorage set: ${key}`);
});

Cypress.Commands.add('getLocalStorage', (key) => {
  const value = window.localStorage.getItem(key);
  return value ? JSON.parse(value) : null;
});

Cypress.Commands.add('clearLocalStorage', () => {
  window.localStorage.clear();
  cy.log('LocalStorage cleared');
});

// Command for session management
Cypress.Commands.add('saveSession', (sessionName) => {
  cy.session(sessionName, () => {
    cy.loginAsRole('user');
  });
  cy.log(`Session saved: ${sessionName}`);
});

// Command for API mocking
Cypress.Commands.add('mockAPI', (method, url, response, statusCode = 200) => {
  cy.intercept(method, url, {
    statusCode,
    body: response
  }).as('mockedAPI');
  cy.log(`API mocked: ${method} ${url}`);
});
```

**cypress/e2e/10-custom-commands.cy.js:**
```javascript
describe('Exercise 10: Creating Custom Cypress Commands', () => {

  // Note: For demonstration, we'll use log commands instead of actual DOM operations
  // In real scenarios, these would interact with actual elements

  it('should use basic login command', () => {
    // In real test: cy.login('user@example.com', 'Password123!');
    cy.log('Simulating login command usage');
    cy.wrap(null).then(() => {
      cy.log('Custom login command would be executed here');
    });
  });

  it('should use loginAsRole command', () => {
    // cy.loginAsRole('admin');
    cy.log('Simulating role-based login');
    cy.wrap(null).then(() => {
      cy.log('Would login as admin role');
    });
  });

  it('should use loginWithOptions command', () => {
    const options = {
      username: 'test@example.com',
      password: 'Test123!',
      rememberMe: true,
      redirectUrl: '/profile'
    };

    cy.log('Login options:', JSON.stringify(options, null, 2));
    // cy.loginWithOptions(options);
  });

  it('should generate test user', () => {
    const user = cy.generateTestUser().then((user) => {
      cy.log('Generated user:', JSON.stringify(user, null, 2));
      expect(user.username).to.include('testuser_');
      expect(user.email).to.include('@example.com');
      return user;
    });
  });

  it('should demonstrate fillForm command', () => {
    const formData = {
      firstName: 'John',
      lastName: 'Doe',
      email: 'john.doe@example.com',
      phone: '1234567890'
    };

    cy.log('Would fill form with:', JSON.stringify(formData, null, 2));
    // cy.fillForm(formData);
  });

  it('should demonstrate chainable commands', () => {
    cy.log('Demonstrating chainable command pattern');

    // Simulated element
    const mockElement = { text: 'Hello Cypress' };

    cy.wrap(mockElement)
      .then((el) => {
        cy.log('Element text:', el.text);
        expect(el.text).to.equal('Hello Cypress');
      });

    // In real test: cy.get('[data-cy="title"]').shouldBeVisible().assertText('Hello');
  });

  it('should demonstrate navigation command', () => {
    const pages = ['home', 'dashboard', 'products', 'cart'];

    pages.forEach((page) => {
      cy.log(`Would navigate to: ${page}`);
      // cy.navigateTo(page);
    });
  });

  it('should demonstrate localStorage commands', () => {
    const testData = {
      userId: 123,
      username: 'testuser',
      preferences: {
        theme: 'dark',
        language: 'en'
      }
    };

    cy.log('Would set localStorage:', JSON.stringify(testData, null, 2));
    // cy.setLocalStorage('userData', testData);
    // cy.getLocalStorage('userData').then((data) => {
    //   expect(data.userId).to.equal(123);
    // });
  });

  it('should demonstrate API mocking command', () => {
    const mockResponse = {
      success: true,
      data: {
        products: [
          { id: 1, name: 'Product 1', price: 10.99 },
          { id: 2, name: 'Product 2', price: 20.99 }
        ]
      }
    };

    cy.log('Would mock API with response:', JSON.stringify(mockResponse, null, 2));
    // cy.mockAPI('GET', '/api/products', mockResponse);
    // cy.visit('/products');
    // cy.wait('@mockedAPI');
  });

  it('should demonstrate table commands', () => {
    const tableData = [
      ['John', 'Doe', 'john@example.com'],
      ['Jane', 'Smith', 'jane@example.com'],
      ['Bob', 'Johnson', 'bob@example.com']
    ];

    cy.log('Table data:', JSON.stringify(tableData, null, 2));
    cy.log('Would use getTableRow and getTableCell commands');
    // cy.getTableRow('[data-cy="users-table"]', 0)
    //   .should('contain', 'John');
    // cy.getTableCell('[data-cy="users-table"]', 1, 2)
    //   .should('have.text', 'jane@example.com');
  });

  it('should demonstrate cleanup command', () => {
    const dataTypes = ['users', 'orders', 'products'];

    dataTypes.forEach((type) => {
      cy.log(`Would cleanup: ${type}`);
      // cy.cleanupTestData(type);
    });
  });

  it('should demonstrate screenshot command', () => {
    cy.log('Would take screenshot with timestamp');
    // cy.takeScreenshot('test-scenario');
  });

  it('should demonstrate complete test flow with custom commands', () => {
    cy.log('===== Complete Test Flow Simulation =====');

    // 1. Login
    cy.log('Step 1: Login as admin');
    // cy.loginAsRole('admin');

    // 2. Navigate
    cy.log('Step 2: Navigate to products');
    // cy.navigateTo('products');

    // 3. Fill form
    const productData = {
      name: 'New Product',
      price: '99.99',
      category: 'Electronics'
    };
    cy.log('Step 3: Fill product form');
    // cy.fillForm(productData);

    // 4. Verify element
    cy.log('Step 4: Verify success message');
    // cy.waitForElement('[data-cy="success-message"]');
    // cy.get('[data-cy="success-message"]')
    //   .shouldBeVisible()
    //   .assertText('Product created successfully');

    // 5. Screenshot
    cy.log('Step 5: Take screenshot');
    // cy.takeScreenshot('product-created');

    // 6. Cleanup
    cy.log('Step 6: Cleanup test data');
    // cy.cleanupTestData('products');

    cy.log('===== Test Flow Complete =====');
  });

  it('should demonstrate benefits of custom commands', () => {
    cy.log('Benefits of Custom Commands:');
    cy.log('1. Reusability - Write once, use many times');
    cy.log('2. Maintainability - Update in one place');
    cy.log('3. Readability - Tests are more descriptive');
    cy.log('4. Abstraction - Hide complex logic');
    cy.log('5. Consistency - Standard way of doing things');

    // Without custom commands:
    cy.log('Without: Multiple lines of repetitive code');

    // With custom commands:
    cy.log('With: cy.login(username, password)');
  });
});
```

### Deliverables Checklist
- [ ] Created commands.js file with custom commands
- [ ] Implemented simple commands
- [ ] Created commands with parameters
- [ ] Built commands with default values
- [ ] Made chainable child commands
- [ ] Created API-related commands
- [ ] Built utility commands
- [ ] Used commands in actual tests
- [ ] All tests pass

### Extension Challenges
1. Create a command library for e-commerce scenarios (add to cart, checkout, etc.)
2. Build commands that integrate with external APIs
3. Create data-driven test commands that load from fixtures
4. Implement custom assertion commands for domain-specific validations

---

## Exercise 11: Page Object Model in Cypress {#exercise-11}

### Objective
Implement the Page Object Model pattern for better test organization and maintainability.

### Scenario
Create page objects for login, dashboard, and product pages to separate test logic from page structure.

### Step-by-Step Instructions

1. Create `cypress/pages/` directory
2. Create page object classes for different pages
3. Implement methods for page interactions
4. Create tests using page objects
5. Compare tests with and without POM

### Code Template

**cypress/pages/BasePage.js:**
```javascript
// TODO: Create base page class
```

**cypress/pages/LoginPage.js:**
```javascript
// TODO: Create login page object
```

**cypress/e2e/11-page-objects.cy.js:**
```javascript
// TODO: Use page objects in tests
```

### Expected Output
- Page objects are well-structured
- Tests are clean and maintainable
- Page interactions are reusable

### Solution Code

**cypress/pages/BasePage.js:**
```javascript
class BasePage {
  visit(url) {
    cy.visit(url);
    cy.log(`Visited: ${url}`);
  }

  getElement(selector) {
    return cy.get(selector);
  }

  clickElement(selector) {
    this.getElement(selector).click();
    cy.log(`Clicked: ${selector}`);
  }

  typeInElement(selector, text) {
    this.getElement(selector).clear().type(text);
    cy.log(`Typed in ${selector}: ${text}`);
  }

  verifyText(selector, expectedText) {
    this.getElement(selector).should('contain.text', expectedText);
    cy.log(`Verified text in ${selector}: ${expectedText}`);
  }

  verifyUrl(expectedUrl) {
    cy.url().should('include', expectedUrl);
    cy.log(`Verified URL includes: ${expectedUrl}`);
  }

  waitForElement(selector, timeout = 10000) {
    this.getElement(selector, { timeout }).should('exist').and('be.visible');
    cy.log(`Waited for element: ${selector}`);
  }
}

export default BasePage;
```

**cypress/pages/LoginPage.js:**
```javascript
import BasePage from './BasePage';

class LoginPage extends BasePage {
  constructor() {
    super();
    this.url = '/login';

    // Selectors
    this.selectors = {
      usernameInput: '[data-cy="username"]',
      passwordInput: '[data-cy="password"]',
      loginButton: '[data-cy="login-button"]',
      errorMessage: '[data-cy="error-message"]',
      rememberMeCheckbox: '[data-cy="remember-me"]',
      forgotPasswordLink: '[data-cy="forgot-password"]',
      signUpLink: '[data-cy="sign-up"]'
    };
  }

  visitLoginPage() {
    this.visit(this.url);
    return this;
  }

  enterUsername(username) {
    this.typeInElement(this.selectors.usernameInput, username);
    return this;
  }

  enterPassword(password) {
    this.typeInElement(this.selectors.passwordInput, password);
    return this;
  }

  clickLoginButton() {
    this.clickElement(this.selectors.loginButton);
    return this;
  }

  checkRememberMe() {
    this.getElement(this.selectors.rememberMeCheckbox).check();
    cy.log('Checked remember me');
    return this;
  }

  login(username, password) {
    this.enterUsername(username);
    this.enterPassword(password);
    this.clickLoginButton();
    cy.log(`Logged in as: ${username}`);
    return this;
  }

  verifyErrorMessage(expectedMessage) {
    this.verifyText(this.selectors.errorMessage, expectedMessage);
    return this;
  }

  clickForgotPassword() {
    this.clickElement(this.selectors.forgotPasswordLink);
    return this;
  }

  clickSignUp() {
    this.clickElement(this.selectors.signUpLink);
    return this;
  }
}

export default LoginPage;
```

**cypress/pages/DashboardPage.js:**
```javascript
import BasePage from './BasePage';

class DashboardPage extends BasePage {
  constructor() {
    super();
    this.url = '/dashboard';

    this.selectors = {
      welcomeMessage: '[data-cy="welcome-message"]',
      userMenu: '[data-cy="user-menu"]',
      logoutButton: '[data-cy="logout"]',
      navigationMenu: '[data-cy="nav-menu"]',
      productsLink: '[data-cy="nav-products"]',
      ordersLink: '[data-cy="nav-orders"]',
      settingsLink: '[data-cy="nav-settings"]',
      statsCards: '[data-cy="stats-card"]',
      recentActivity: '[data-cy="recent-activity"]'
    };
  }

  visitDashboard() {
    this.visit(this.url);
    return this;
  }

  verifyDashboardLoaded() {
    this.verifyUrl(this.url);
    this.waitForElement(this.selectors.welcomeMessage);
    cy.log('Dashboard loaded successfully');
    return this;
  }

  verifyWelcomeMessage(username) {
    this.verifyText(this.selectors.welcomeMessage, username);
    return this;
  }

  navigateToProducts() {
    this.clickElement(this.selectors.productsLink);
    cy.log('Navigated to products');
    return this;
  }

  navigateToOrders() {
    this.clickElement(this.selectors.ordersLink);
    cy.log('Navigated to orders');
    return this;
  }

  navigateToSettings() {
    this.clickElement(this.selectors.settingsLink);
    cy.log('Navigated to settings');
    return this;
  }

  logout() {
    this.clickElement(this.selectors.userMenu);
    this.clickElement(this.selectors.logoutButton);
    cy.log('Logged out');
  }

  getStatsCards() {
    return this.getElement(this.selectors.statsCards);
  }

  verifyStatsCardCount(expectedCount) {
    this.getStatsCards().should('have.length', expectedCount);
    cy.log(`Verified ${expectedCount} stats cards`);
    return this;
  }
}

export default DashboardPage;
```

**cypress/pages/ProductsPage.js:**
```javascript
import BasePage from './BasePage';

class ProductsPage extends BasePage {
  constructor() {
    super();
    this.url = '/products';

    this.selectors = {
      productList: '[data-cy="product-list"]',
      productCard: '[data-cy="product-card"]',
      productName: '[data-cy="product-name"]',
      productPrice: '[data-cy="product-price"]',
      addToCartButton: '[data-cy="add-to-cart"]',
      searchInput: '[data-cy="search-products"]',
      filterDropdown: '[data-cy="filter-category"]',
      sortDropdown: '[data-cy="sort-by"]',
      cartIcon: '[data-cy="cart-icon"]',
      cartCount: '[data-cy="cart-count"]'
    };
  }

  visitProductsPage() {
    this.visit(this.url);
    return this;
  }

  searchProduct(productName) {
    this.typeInElement(this.selectors.searchInput, productName);
    cy.log(`Searched for: ${productName}`);
    return this;
  }

  filterByCategory(category) {
    this.getElement(this.selectors.filterDropdown).select(category);
    cy.log(`Filtered by category: ${category}`);
    return this;
  }

  sortBy(sortOption) {
    this.getElement(this.selectors.sortDropdown).select(sortOption);
    cy.log(`Sorted by: ${sortOption}`);
    return this;
  }

  getProductCards() {
    return this.getElement(this.selectors.productCard);
  }

  getProductByName(productName) {
    return this.getElement(this.selectors.productCard)
      .contains(productName)
      .parents(this.selectors.productCard);
  }

  addProductToCart(productName) {
    this.getProductByName(productName)
      .find(this.selectors.addToCartButton)
      .click();
    cy.log(`Added to cart: ${productName}`);
    return this;
  }

  verifyProductCount(expectedCount) {
    this.getProductCards().should('have.length', expectedCount);
    cy.log(`Verified ${expectedCount} products displayed`);
    return this;
  }

  verifyCartCount(expectedCount) {
    this.verifyText(this.selectors.cartCount, expectedCount.toString());
    cy.log(`Cart count: ${expectedCount}`);
    return this;
  }

  openCart() {
    this.clickElement(this.selectors.cartIcon);
    cy.log('Opened cart');
  }
}

export default ProductsPage;
```

**cypress/pages/index.js:**
```javascript
// Central export file for all page objects
import LoginPage from './LoginPage';
import DashboardPage from './DashboardPage';
import ProductsPage from './ProductsPage';

export {
  LoginPage,
  DashboardPage,
  ProductsPage
};
```

**cypress/e2e/11-page-objects.cy.js:**
```javascript
import { LoginPage, DashboardPage, ProductsPage } from '../pages';

describe('Exercise 11: Page Object Model in Cypress', () => {

  let loginPage;
  let dashboardPage;
  let productsPage;

  beforeEach(() => {
    // Initialize page objects
    loginPage = new LoginPage();
    dashboardPage = new DashboardPage();
    productsPage = new ProductsPage();
  });

  it('should demonstrate login with page objects', () => {
    cy.log('Test: Login with Page Objects');

    // Using page object methods
    loginPage
      .visitLoginPage()
      .enterUsername('testuser@example.com')
      .enterPassword('Test123!')
      .clickLoginButton();

    cy.log('Login flow completed using page objects');
  });

  it('should demonstrate method chaining', () => {
    cy.log('Test: Method Chaining');

    // All methods return 'this' for chaining
    loginPage
      .visitLoginPage()
      .login('admin@example.com', 'Admin123!')
      .verifyUrl('/dashboard');

    cy.log('Chained methods executed successfully');
  });

  it('should demonstrate dashboard navigation', () => {
    cy.log('Test: Dashboard Navigation');

    // Login first
    loginPage.visitLoginPage().login('user@example.com', 'User123!');

    // Navigate dashboard
    dashboardPage
      .verifyDashboardLoaded()
      .verifyWelcomeMessage('Test User')
      .navigateToProducts();

    cy.log('Dashboard navigation completed');
  });

  it('should demonstrate product search and filter', () => {
    cy.log('Test: Product Search and Filter');

    // Navigate to products
    productsPage
      .visitProductsPage()
      .searchProduct('Laptop')
      .filterByCategory('Electronics')
      .sortBy('price-low-high');

    cy.log('Product search and filter applied');
  });

  it('should demonstrate add to cart flow', () => {
    cy.log('Test: Add to Cart Flow');

    productsPage
      .visitProductsPage()
      .addProductToCart('Laptop')
      .verifyCartCount(1)
      .addProductToCart('Mouse')
      .verifyCartCount(2);

    cy.log('Products added to cart');
  });

  it('should demonstrate complete e2e flow with page objects', () => {
    cy.log('===== Complete E2E Flow =====');

    // Step 1: Login
    cy.log('Step 1: Login');
    loginPage
      .visitLoginPage()
      .login('testuser@example.com', 'Test123!');

    // Step 2: Verify Dashboard
    cy.log('Step 2: Verify Dashboard');
    dashboardPage
      .verifyDashboardLoaded()
      .verifyStatsCardCount(4);

    // Step 3: Navigate to Products
    cy.log('Step 3: Navigate to Products');
    dashboardPage.navigateToProducts();

    // Step 4: Search and Add Products
    cy.log('Step 4: Search and Add Products');
    productsPage
      .searchProduct('Laptop')
      .addProductToCart('Laptop')
      .verifyCartCount(1);

    // Step 5: Open Cart
    cy.log('Step 5: Open Cart');
    productsPage.openCart();

    cy.log('===== E2E Flow Complete =====');
  });

  it('should compare tests with and without POM', () => {
    cy.log('========== WITHOUT Page Objects ==========');
    cy.log('cy.visit("/login")');
    cy.log('cy.get("[data-cy=username]").type("user@example.com")');
    cy.log('cy.get("[data-cy=password]").type("Pass123!")');
    cy.log('cy.get("[data-cy=login-button]").click()');
    cy.log('cy.url().should("include", "/dashboard")');
    cy.log('cy.get("[data-cy=nav-products]").click()');
    cy.log('cy.get("[data-cy=search-products]").type("Laptop")');
    cy.log('// Repeated across multiple tests...');

    cy.log('========== WITH Page Objects ==========');
    cy.log('loginPage.visitLoginPage().login(email, password)');
    cy.log('dashboardPage.navigateToProducts()');
    cy.log('productsPage.searchProduct("Laptop")');
    cy.log('// Reusable, maintainable, readable!');
  });

  it('should demonstrate benefits of POM', () => {
    cy.log('Benefits of Page Object Model:');
    cy.log('1. Separation of Concerns - Test logic vs page structure');
    cy.log('2. Reusability - Page methods used across multiple tests');
    cy.log('3. Maintainability - Update selectors in one place');
    cy.log('4. Readability - Tests read like business requirements');
    cy.log('5. Encapsulation - Hide implementation details');
    cy.log('6. Reduced Duplication - DRY principle');
  });

  it('should demonstrate page object inheritance', () => {
    cy.log('Page Object Inheritance:');
    cy.log('BasePage provides common methods:');
    cy.log('- visit(), getElement(), clickElement()');
    cy.log('- typeInElement(), verifyText()');
    cy.log('');
    cy.log('Child pages extend BasePage:');
    cy.log('- LoginPage, DashboardPage, ProductsPage');
    cy.log('- Inherit common methods');
    cy.log('- Add page-specific methods');
  });

  it('should demonstrate error handling in page objects', () => {
    cy.log('Error Handling Example');

    try {
      loginPage
        .visitLoginPage()
        .login('invalid@example.com', 'wrongpass')
        .verifyErrorMessage('Invalid credentials');

      cy.log('Error message verified successfully');
    } catch (error) {
      cy.log('Error caught:', error.message);
    }
  });
});
```

### Deliverables Checklist
- [ ] Created BasePage with common methods
- [ ] Created page-specific page objects
- [ ] Implemented method chaining
- [ ] Used inheritance effectively
- [ ] Created centralized selector management
- [ ] Built reusable page methods
- [ ] Wrote tests using page objects
- [ ] Compared POM vs non-POM approaches
- [ ] All tests pass

### Extension Challenges
1. Create page objects for a complete e-commerce application
2. Implement page object factory pattern
3. Add TypeScript types to page objects
4. Create component objects for reusable UI components (modals, forms, etc.)

---

## Exercise 12: API Testing with Cypress {#exercise-12}

### Objective
Master API testing using Cypress request commands and validate responses.

### Scenario
Test REST API endpoints for user management, authentication, and data operations.

### Step-by-Step Instructions

1. Create spec file: `cypress/e2e/12-api-testing.cy.js`
2. Make GET, POST, PUT, DELETE requests
3. Validate response status, headers, and body
4. Chain API requests
5. Use API responses in tests
6. Implement API test utilities

### Code Template

```javascript
describe('Exercise 12: API Testing', () => {

  it('should make GET request', () => {
    // TODO: Test GET endpoint
  });

  it('should make POST request', () => {
    // TODO: Test POST endpoint
  });

  it('should validate response', () => {
    // TODO: Validate API response
  });
});
```

### Expected Output
- API requests execute successfully
- Response validation works
- Status codes are correct

### Solution Code

```javascript
describe('Exercise 12: API Testing with Cypress', () => {

  // API base URL
  const API_BASE_URL = 'https://jsonplaceholder.typicode.com';

  // Test data
  const testUser = {
    name: 'Test User',
    username: 'testuser',
    email: 'test@example.com'
  };

  it('should make GET request and validate response', () => {
    cy.request({
      method: 'GET',
      url: `${API_BASE_URL}/users/1`
    }).then((response) => {
      // Validate status code
      expect(response.status).to.eq(200);

      // Validate headers
      expect(response.headers).to.have.property('content-type');
      expect(response.headers['content-type']).to.include('application/json');

      // Validate body
      expect(response.body).to.have.property('id');
      expect(response.body).to.have.property('name');
      expect(response.body).to.have.property('email');
      expect(response.body.id).to.eq(1);

      cy.log('GET request successful');
      cy.log('User:', JSON.stringify(response.body, null, 2));
    });
  });

  it('should make POST request to create resource', () => {
    cy.request({
      method: 'POST',
      url: `${API_BASE_URL}/users`,
      body: testUser
    }).then((response) => {
      expect(response.status).to.eq(201); // Created
      expect(response.body).to.have.property('id');
      expect(response.body.name).to.eq(testUser.name);
      expect(response.body.email).to.eq(testUser.email);

      cy.log('POST request successful');
      cy.log('Created user ID:', response.body.id);
    });
  });

  it('should make PUT request to update resource', () => {
    const updatedUser = {
      name: 'Updated Name',
      username: 'updateduser',
      email: 'updated@example.com'
    };

    cy.request({
      method: 'PUT',
      url: `${API_BASE_URL}/users/1`,
      body: updatedUser
    }).then((response) => {
      expect(response.status).to.eq(200);
      expect(response.body.name).to.eq(updatedUser.name);
      expect(response.body.email).to.eq(updatedUser.email);

      cy.log('PUT request successful');
      cy.log('Updated user:', JSON.stringify(response.body, null, 2));
    });
  });

  it('should make PATCH request for partial update', () => {
    cy.request({
      method: 'PATCH',
      url: `${API_BASE_URL}/users/1`,
      body: { email: 'newemail@example.com' }
    }).then((response) => {
      expect(response.status).to.eq(200);
      expect(response.body.email).to.eq('newemail@example.com');

      cy.log('PATCH request successful');
    });
  });

  it('should make DELETE request', () => {
    cy.request({
      method: 'DELETE',
      url: `${API_BASE_URL}/users/1`
    }).then((response) => {
      expect(response.status).to.eq(200);

      cy.log('DELETE request successful');
    });
  });

  it('should handle query parameters', () => {
    cy.request({
      method: 'GET',
      url: `${API_BASE_URL}/posts`,
      qs: {
        userId: 1,
        _limit: 5
      }
    }).then((response) => {
      expect(response.status).to.eq(200);
      expect(response.body).to.be.an('array');
      expect(response.body).to.have.length(5);

      // Verify all posts belong to userId 1
      const allFromUser1 = response.body.every(post => post.userId === 1);
      expect(allFromUser1).to.be.true;

      cy.log('Query parameters handled successfully');
    });
  });

  it('should handle request headers', () => {
    cy.request({
      method: 'GET',
      url: `${API_BASE_URL}/users/1`,
      headers: {
        'Authorization': 'Bearer fake-token-123',
        'Custom-Header': 'custom-value'
      }
    }).then((response) => {
      expect(response.status).to.eq(200);

      cy.log('Custom headers sent successfully');
    });
  });

  it('should chain API requests', () => {
    let userId;

    // First request: Create user
    cy.request('POST', `${API_BASE_URL}/users`, testUser)
      .then((response) => {
        expect(response.status).to.eq(201);
        userId = response.body.id;
        cy.log('Created user ID:', userId);

        // Second request: Get the created user
        return cy.request('GET', `${API_BASE_URL}/users/${userId}`);
      })
      .then((response) => {
        expect(response.status).to.eq(200);
        expect(response.body.id).to.eq(userId);
        cy.log('Retrieved user:', JSON.stringify(response.body, null, 2));

        // Third request: Update the user
        return cy.request('PUT', `${API_BASE_URL}/users/${userId}`, {
          ...testUser,
          name: 'Updated Name'
        });
      })
      .then((response) => {
        expect(response.status).to.eq(200);
        expect(response.body.name).to.eq('Updated Name');
        cy.log('Updated user successfully');
      });
  });

  it('should validate response schema', () => {
    cy.request('GET', `${API_BASE_URL}/users/1`)
      .then((response) => {
        const user = response.body;

        // Validate structure
        const requiredFields = ['id', 'name', 'username', 'email', 'address', 'phone', 'website', 'company'];
        requiredFields.forEach(field => {
          expect(user).to.have.property(field);
        });

        // Validate types
        expect(user.id).to.be.a('number');
        expect(user.name).to.be.a('string');
        expect(user.email).to.be.a('string');
        expect(user.address).to.be.an('object');

        // Validate nested object
        expect(user.address).to.have.property('street');
        expect(user.address).to.have.property('city');
        expect(user.address).to.have.property('zipcode');

        cy.log('Schema validation passed');
      });
  });

  it('should validate array responses', () => {
    cy.request('GET', `${API_BASE_URL}/users`)
      .then((response) => {
        expect(response.status).to.eq(200);
        expect(response.body).to.be.an('array');
        expect(response.body).to.have.length.greaterThan(0);

        // Validate first item structure
        const firstUser = response.body[0];
        expect(firstUser).to.have.property('id');
        expect(firstUser).to.have.property('name');
        expect(firstUser).to.have.property('email');

        // Validate all items
        response.body.forEach((user, index) => {
          expect(user.id).to.be.a('number');
          expect(user.email).to.include('@');
          cy.log(`User ${index + 1}: ${user.name}`);
        });
      });
  });

  it('should handle error responses', () => {
    cy.request({
      method: 'GET',
      url: `${API_BASE_URL}/users/999999`,
      failOnStatusCode: false // Don't fail test on 404
    }).then((response) => {
      expect(response.status).to.eq(404);
      cy.log('404 error handled correctly');
    });
  });

  it('should test authentication flow', () => {
    // Simulate login
    const credentials = {
      username: 'testuser',
      password: 'password123'
    };

    cy.log('Step 1: Login');
    // In real API, this would return a token
    cy.wrap({ token: 'fake-jwt-token-123' })
      .then((authResponse) => {
        const token = authResponse.token;
        cy.log('Received token:', token);

        // Step 2: Use token in subsequent requests
        cy.log('Step 2: Make authenticated request');
        return cy.request({
          method: 'GET',
          url: `${API_BASE_URL}/users/1`,
          headers: {
            'Authorization': `Bearer ${token}`
          }
        });
      })
      .then((response) => {
        expect(response.status).to.eq(200);
        cy.log('Authenticated request successful');
      });
  });

  it('should test pagination', () => {
    const page = 1;
    const limit = 5;

    cy.request({
      method: 'GET',
      url: `${API_BASE_URL}/posts`,
      qs: {
        _page: page,
        _limit: limit
      }
    }).then((response) => {
      expect(response.status).to.eq(200);
      expect(response.body).to.be.an('array');
      expect(response.body).to.have.length(limit);

      // Check pagination headers
      cy.log('Total posts:', response.headers['x-total-count'] || 'N/A');
      cy.log('Received:', response.body.length);
    });
  });

  it('should test search functionality', () => {
    cy.request({
      method: 'GET',
      url: `${API_BASE_URL}/posts`,
      qs: {
        title_like: 'qui'
      }
    }).then((response) => {
      expect(response.status).to.eq(200);
      expect(response.body).to.be.an('array');

      // Verify all results match search criteria
      response.body.forEach(post => {
        cy.log(`Post: ${post.title}`);
      });
    });
  });

  it('should measure response time', () => {
    const startTime = Date.now();

    cy.request('GET', `${API_BASE_URL}/users`)
      .then((response) => {
        const endTime = Date.now();
        const responseTime = endTime - startTime;

        expect(response.status).to.eq(200);
        cy.log(`Response time: ${responseTime}ms`);

        // Assert response time is reasonable
        expect(responseTime).to.be.lessThan(5000); // Less than 5 seconds
      });
  });

  it('should test CRUD operations flow', () => {
    let postId;

    cy.log('===== CRUD Operations Test =====');

    // CREATE
    cy.log('1. CREATE - Creating new post');
    cy.request({
      method: 'POST',
      url: `${API_BASE_URL}/posts`,
      body: {
        title: 'Test Post',
        body: 'This is a test post',
        userId: 1
      }
    }).then((response) => {
      expect(response.status).to.eq(201);
      postId = response.body.id;
      cy.log(`Created post with ID: ${postId}`);

      // READ
      cy.log('2. READ - Reading created post');
      return cy.request('GET', `${API_BASE_URL}/posts/${postId}`);
    }).then((response) => {
      expect(response.status).to.eq(200);
      expect(response.body.title).to.eq('Test Post');
      cy.log('Post retrieved successfully');

      // UPDATE
      cy.log('3. UPDATE - Updating post');
      return cy.request({
        method: 'PUT',
        url: `${API_BASE_URL}/posts/${postId}`,
        body: {
          title: 'Updated Test Post',
          body: 'Updated body',
          userId: 1
        }
      });
    }).then((response) => {
      expect(response.status).to.eq(200);
      expect(response.body.title).to.eq('Updated Test Post');
      cy.log('Post updated successfully');

      // DELETE
      cy.log('4. DELETE - Deleting post');
      return cy.request('DELETE', `${API_BASE_URL}/posts/${postId}`);
    }).then((response) => {
      expect(response.status).to.eq(200);
      cy.log('Post deleted successfully');
      cy.log('===== CRUD Operations Complete =====');
    });
  });

  it('should create API test helper utilities', () => {
    // API test utilities
    const apiUtils = {
      get: (endpoint) => cy.request('GET', `${API_BASE_URL}${endpoint}`),

      post: (endpoint, body) => cy.request({
        method: 'POST',
        url: `${API_BASE_URL}${endpoint}`,
        body
      }),

      put: (endpoint, body) => cy.request({
        method: 'PUT',
        url: `${API_BASE_URL}${endpoint}`,
        body
      }),

      delete: (endpoint) => cy.request('DELETE', `${API_BASE_URL}${endpoint}`),

      validateStatus: (response, expectedStatus) => {
        expect(response.status).to.eq(expectedStatus);
      },

      validateResponseTime: (startTime, maxTime = 3000) => {
        const responseTime = Date.now() - startTime;
        expect(responseTime).to.be.lessThan(maxTime);
        cy.log(`Response time: ${responseTime}ms`);
      }
    };

    // Use utilities
    const startTime = Date.now();

    apiUtils.get('/users/1')
      .then((response) => {
        apiUtils.validateStatus(response, 200);
        apiUtils.validateResponseTime(startTime);
        expect(response.body).to.have.property('name');
        cy.log('API utilities work correctly');
      });
  });

  it('should test API with intercept and mock', () => {
    const mockUsers = [
      { id: 1, name: 'Mock User 1', email: 'mock1@example.com' },
      { id: 2, name: 'Mock User 2', email: 'mock2@example.com' }
    ];

    // Intercept API call and return mock data
    cy.intercept('GET', `${API_BASE_URL}/users`, {
      statusCode: 200,
      body: mockUsers
    }).as('getUsers');

    // Make the request
    cy.request('GET', `${API_BASE_URL}/users`)
      .then((response) => {
        expect(response.status).to.eq(200);
        expect(response.body).to.deep.equal(mockUsers);
        cy.log('Mocked API response received');
      });
  });
});
```

### Deliverables Checklist
- [ ] Made GET, POST, PUT, PATCH, DELETE requests
- [ ] Validated response status codes
- [ ] Validated response headers
- [ ] Validated response body structure
- [ ] Chained API requests
- [ ] Handled query parameters
- [ ] Tested error responses
- [ ] Measured response times
- [ ] Created API test utilities
- [ ] All tests pass

### Extension Challenges
1. Create a complete API test suite for a RESTful service
2. Implement API contract testing with schema validation
3. Build performance tests for API endpoints
4. Create data-driven API tests using fixtures

---

## Exercise 13: Handling Async Operations in Tests {#exercise-13}

### Objective
Master handling asynchronous operations, waits, and retries in Cypress tests.

### Scenario
Handle dynamic content loading, AJAX requests, animations, and timing-dependent operations.

### Step-by-Step Instructions

1. Create spec file: `cypress/e2e/13-async-operations.cy.js`
2. Use cy.wait() appropriately
3. Implement custom waiting strategies
4. Handle dynamic elements
5. Use retry mechanisms
6. Work with Cypress automatic retry

### Code Template

```javascript
describe('Exercise 13: Async Operations', () => {

  it('should wait for elements', () => {
    // TODO: Wait for dynamic elements
  });

  it('should handle AJAX requests', () => {
    // TODO: Wait for AJAX to complete
  });

  it('should retry operations', () => {
    // TODO: Implement retry logic
  });
});
```

### Expected Output
- Waits work correctly
- No race conditions
- Tests are stable

### Solution Code

```javascript
describe('Exercise 13: Handling Async Operations in Tests', () => {

  // Simulate async data loading
  const loadDataAsync = (delay = 1000) => {
    return new Promise(resolve => {
      setTimeout(() => {
        resolve({
          users: [
            { id: 1, name: 'User 1', status: 'active' },
            { id: 2, name: 'User 2', status: 'inactive' },
            { id: 3, name: 'User 3', status: 'active' }
          ],
          timestamp: Date.now()
        });
      }, delay);
    });
  };

  it('should understand Cypress automatic retry', () => {
    cy.log('Cypress automatically retries assertions');

    // Simulate element appearing after delay
    let elementVisible = false;

    setTimeout(() => {
      elementVisible = true;
      cy.log('Element became visible');
    }, 2000);

    // Cypress will retry this assertion until it passes or times out
    cy.wrap(null).then(() => {
      cy.log('Waiting for element to become visible...');
      // In real test: cy.get('[data-cy="dynamic-element"]').should('be.visible');
    });

    cy.log('Automatic retry demonstration complete');
  });

  it('should use cy.wait() with time', () => {
    cy.log('Starting test...');

    // Fixed wait (generally discouraged)
    cy.wait(1000);
    cy.log('Waited 1 second');

    // Better to wait for specific conditions
    cy.wait(500);
    cy.log('Waited additional 500ms');

    cy.log('Fixed waits completed');
  });

  it('should wait for network requests with intercept', () => {
    cy.log('Setting up API intercept');

    // Intercept API call
    cy.intercept('GET', '**/api/users', {
      statusCode: 200,
      body: {
        users: [
          { id: 1, name: 'User 1' },
          { id: 2, name: 'User 2' }
        ]
      },
      delay: 1000 // Simulate network delay
    }).as('getUsers');

    cy.log('Making API request');

    // Trigger the request
    cy.request('GET', 'https://example.com/api/users');

    // Wait for the intercepted request
    cy.wait('@getUsers').then((interception) => {
      expect(interception.response.statusCode).to.eq(200);
      expect(interception.response.body.users).to.have.length(2);
      cy.log('API request completed and intercepted');
    });
  });

  it('should wait for multiple network requests', () => {
    cy.intercept('GET', '**/api/users', { body: [] }).as('getUsers');
    cy.intercept('GET', '**/api/products', { body: [] }).as('getProducts');
    cy.intercept('GET', '**/api/orders', { body: [] }).as('getOrders');

    // Trigger requests (in real test)
    cy.log('Would trigger multiple API requests');

    // Wait for all requests
    // cy.wait(['@getUsers', '@getProducts', '@getOrders']);

    cy.log('Would wait for all API requests to complete');
  });

  it('should handle dynamic content loading', () => {
    cy.log('Simulating dynamic content loading');

    // Wrap promise for Cypress to handle
    cy.wrap(loadDataAsync(1500)).then((data) => {
      cy.log('Data loaded:', JSON.stringify(data, null, 2));

      expect(data.users).to.have.length(3);
      expect(data.users[0].name).to.eq('User 1');

      // Process loaded data
      const activeUsers = data.users.filter(u => u.status === 'active');
      expect(activeUsers).to.have.length(2);

      cy.log(`Active users: ${activeUsers.length}`);
    });
  });

  it('should use custom wait conditions', () => {
    cy.log('Custom wait condition example');

    // Simulate checking for condition
    let conditionMet = false;

    setTimeout(() => {
      conditionMet = true;
      cy.log('Condition met');
    }, 2000);

    // Custom wait function
    const waitForCondition = (checkFunction, timeout = 5000) => {
      const startTime = Date.now();

      const check = () => {
        return cy.wrap(null).then(() => {
          if (checkFunction()) {
            cy.log('Condition satisfied');
            return true;
          } else if (Date.now() - startTime > timeout) {
            throw new Error('Condition timeout');
          } else {
            cy.wait(100);
            return check();
          }
        });
      };

      return check();
    };

    // Use custom wait
    waitForCondition(() => conditionMet).then(() => {
      cy.log('Custom wait completed successfully');
    });
  });

  it('should handle element visibility with retry', () => {
    cy.log('Element visibility retry example');

    // Simulate element appearing
    let visible = false;
    let attempts = 0;

    setTimeout(() => {
      visible = true;
      cy.log('Element is now visible');
    }, 1500);

    // Retry checking visibility
    const checkVisibility = () => {
      return cy.wrap(null).then(() => {
        attempts++;
        cy.log(`Attempt ${attempts}: Checking visibility`);

        if (visible) {
          cy.log('Element found!');
          return true;
        } else if (attempts < 20) {
          cy.wait(100);
          return checkVisibility();
        } else {
          throw new Error('Element never became visible');
        }
      });
    };

    checkVisibility().then(() => {
      expect(attempts).to.be.lessThan(20);
      cy.log(`Found after ${attempts} attempts`);
    });
  });

  it('should handle sequential async operations', () => {
    cy.log('Sequential async operations');

    cy.wrap(loadDataAsync(500))
      .then((data1) => {
        cy.log('Step 1 complete:', data1.users.length, 'users');
        return loadDataAsync(300);
      })
      .then((data2) => {
        cy.log('Step 2 complete:', data2.users.length, 'users');
        return loadDataAsync(200);
      })
      .then((data3) => {
        cy.log('Step 3 complete:', data3.users.length, 'users');
        cy.log('All sequential operations completed');
      });
  });

  it('should handle parallel async operations', () => {
    cy.log('Parallel async operations');

    Promise.all([
      loadDataAsync(500),
      loadDataAsync(300),
      loadDataAsync(400)
    ]).then((results) => {
      cy.log('All parallel operations completed');
      expect(results).to.have.length(3);

      results.forEach((result, index) => {
        cy.log(`Result ${index + 1}:`, result.users.length, 'users');
      });
    });
  });

  it('should handle race conditions', () => {
    cy.log('Race condition handling');

    // Simulate competing operations
    const operation1 = new Promise(resolve => {
      setTimeout(() => resolve({ name: 'Operation 1', time: 500 }), 500);
    });

    const operation2 = new Promise(resolve => {
      setTimeout(() => resolve({ name: 'Operation 2', time: 300 }), 300);
    });

    const operation3 = new Promise(resolve => {
      setTimeout(() => resolve({ name: 'Operation 3', time: 700 }), 700);
    });

    // First to complete wins
    Promise.race([operation1, operation2, operation3])
      .then((winner) => {
        cy.log('Winner:', winner.name);
        cy.log('Completed in:', winner.time, 'ms');
        expect(winner.name).to.eq('Operation 2'); // Fastest
      });
  });

  it('should implement exponential backoff retry', () => {
    cy.log('Exponential backoff retry');

    let attempts = 0;

    const unreliableOperation = () => {
      attempts++;
      if (attempts < 4) {
        return Promise.reject(new Error(`Attempt ${attempts} failed`));
      }
      return Promise.resolve({ success: true, attempts });
    };

    const retryWithBackoff = (operation, maxRetries = 5, baseDelay = 100) => {
      let currentAttempt = 0;

      const attempt = () => {
        return operation().catch((error) => {
          currentAttempt++;

          if (currentAttempt >= maxRetries) {
            throw error;
          }

          const delay = baseDelay * Math.pow(2, currentAttempt - 1);
          cy.log(`Retry ${currentAttempt} after ${delay}ms`);

          return cy.wait(delay).then(() => attempt());
        });
      };

      return attempt();
    };

    cy.wrap(null).then(() => {
      return retryWithBackoff(unreliableOperation);
    }).then((result) => {
      cy.log('Operation succeeded:', JSON.stringify(result));
      expect(result.success).to.be.true;
      expect(result.attempts).to.eq(4);
    });
  });

  it('should handle timeout scenarios', () => {
    cy.log('Timeout handling');

    const slowOperation = () => {
      return new Promise((resolve) => {
        setTimeout(() => {
          resolve({ data: 'Slow response' });
        }, 3000);
      });
    };

    const withTimeout = (promise, timeoutMs) => {
      const timeout = new Promise((_, reject) => {
        setTimeout(() => {
          reject(new Error(`Operation timed out after ${timeoutMs}ms`));
        }, timeoutMs);
      });

      return Promise.race([promise, timeout]);
    };

    // This should timeout
    cy.wrap(null).then(() => {
      return withTimeout(slowOperation(), 1000);
    }).then(
      (result) => {
        cy.log('Should not reach here');
      },
      (error) => {
        cy.log('Timeout caught:', error.message);
        expect(error.message).to.include('timed out');
      }
    );
  });

  it('should handle debouncing in tests', () => {
    cy.log('Debounce simulation');

    let executionCount = 0;
    let lastResult = null;

    const debounce = (func, delay) => {
      let timeoutId;
      return (...args) => {
        clearTimeout(timeoutId);
        timeoutId = setTimeout(() => {
          func(...args);
        }, delay);
      };
    };

    const searchFunction = (query) => {
      executionCount++;
      lastResult = query;
      cy.log(`Search executed (${executionCount}): ${query}`);
    };

    const debouncedSearch = debounce(searchFunction, 500);

    // Rapid calls
    debouncedSearch('a');
    debouncedSearch('ab');
    debouncedSearch('abc');
    debouncedSearch('abcd');

    // Wait for debounce to complete
    cy.wait(600).then(() => {
      cy.log('Execution count:', executionCount);
      cy.log('Last result:', lastResult);

      expect(executionCount).to.eq(1); // Only executed once
      expect(lastResult).to.eq('abcd'); // Last value
    });
  });

  it('should handle polling for state changes', () => {
    cy.log('Polling for state changes');

    let jobStatus = 'pending';
    let pollCount = 0;

    // Simulate job completing after some time
    setTimeout(() => {
      jobStatus = 'processing';
    }, 1000);

    setTimeout(() => {
      jobStatus = 'completed';
    }, 2500);

    const pollUntilComplete = (checkInterval = 200, maxPolls = 20) => {
      const poll = () => {
        return cy.wrap(null).then(() => {
          pollCount++;
          cy.log(`Poll ${pollCount}: Status = ${jobStatus}`);

          if (jobStatus === 'completed') {
            return { status: jobStatus, polls: pollCount };
          } else if (pollCount >= maxPolls) {
            throw new Error('Max polls reached');
          } else {
            return cy.wait(checkInterval).then(() => poll());
          }
        });
      };

      return poll();
    };

    pollUntilComplete().then((result) => {
      cy.log('Job completed after', result.polls, 'polls');
      expect(result.status).to.eq('completed');
      expect(result.polls).to.be.lessThan(20);
    });
  });

  it('should demonstrate Cypress command queue', () => {
    cy.log('Command queue demonstration');

    cy.log('1. First command (queued)');
    cy.log('2. Second command (queued)');
    cy.log('3. Third command (queued)');

    cy.wait(500).then(() => {
      cy.log('4. After wait (queued)');
    });

    cy.log('5. This logs after previous commands complete');

    // All commands execute in order
    cy.wrap(null).then(() => {
      cy.log('All commands in queue completed');
    });
  });

  it('should handle async test data preparation', () => {
    cy.log('Async test data preparation');

    const prepareTestData = async () => {
      cy.log('Preparing test users...');
      await new Promise(resolve => setTimeout(resolve, 500));

      cy.log('Preparing test products...');
      await new Promise(resolve => setTimeout(resolve, 300));

      cy.log('Preparing test orders...');
      await new Promise(resolve => setTimeout(resolve, 400));

      return {
        users: ['user1', 'user2', 'user3'],
        products: ['prod1', 'prod2'],
        orders: ['order1']
      };
    };

    cy.wrap(null).then(async () => {
      const testData = await prepareTestData();

      cy.log('Test data prepared:', JSON.stringify(testData, null, 2));

      expect(testData.users).to.have.length(3);
      expect(testData.products).to.have.length(2);
      expect(testData.orders).to.have.length(1);
    });
  });
});
```

### Deliverables Checklist
- [ ] Used cy.wait() appropriately
- [ ] Implemented network request waiting with intercept
- [ ] Created custom wait conditions
- [ ] Handled dynamic content loading
- [ ] Implemented retry with backoff
- [ ] Handled timeout scenarios
- [ ] Implemented polling mechanism
- [ ] Understood Cypress command queue
- [ ] All tests pass

### Extension Challenges
1. Create a robust waiting utility library
2. Implement smart waiting that adapts to application behavior
3. Build a request queue manager for API tests
4. Create performance monitoring for async operations

---

## Exercise 14: Error Handling in Test Automation {#exercise-14}

### Objective
Implement comprehensive error handling strategies for robust test automation.

### Scenario
Handle expected errors, unexpected failures, network issues, and implement graceful degradation.

### Step-by-Step Instructions

1. Create spec file: `cypress/e2e/14-error-handling.cy.js`
2. Handle expected errors
3. Implement try-catch blocks
4. Use failOnStatusCode option
5. Create error recovery mechanisms
6. Implement custom error handlers

### Code Template

```javascript
describe('Exercise 14: Error Handling', () => {

  it('should handle expected errors', () => {
    // TODO: Test error scenarios
  });

  it('should recover from errors', () => {
    // TODO: Implement recovery logic
  });

  it('should handle network failures', () => {
    // TODO: Handle network issues
  });
});
```

### Expected Output
- Errors are handled gracefully
- Tests don't fail unexpectedly
- Error messages are informative

### Solution Code

```javascript
describe('Exercise 14: Error Handling in Test Automation', () => {

  it('should handle expected errors with try-catch', () => {
    cy.log('Testing expected error handling');

    try {
      const data = JSON.parse('{ invalid json }');
      cy.log('Should not reach here');
    } catch (error) {
      cy.log('Error caught:', error.message);
      expect(error).to.be.instanceOf(SyntaxError);
      expect(error.message).to.include('JSON');
    }

    cy.log('Test continued after error handling');
  });

  it('should handle API errors with failOnStatusCode', () => {
    cy.log('Testing API error handling');

    // Allow non-2xx status codes
    cy.request({
      method: 'GET',
      url: 'https://jsonplaceholder.typicode.com/users/99999',
      failOnStatusCode: false
    }).then((response) => {
      if (response.status === 404) {
        cy.log('Resource not found (404) - handled gracefully');
        expect(response.status).to.eq(404);
      } else if (response.status >= 500) {
        cy.log('Server error - handled gracefully');
        expect(response.status).to.be.gte(500);
      } else {
        cy.log('Unexpected status:', response.status);
      }
    });
  });

  it('should validate data and handle validation errors', () => {
    const validateUser = (user) => {
      const errors = [];

      if (!user.email || !user.email.includes('@')) {
        errors.push('Invalid email address');
      }

      if (!user.password || user.password.length < 8) {
        errors.push('Password must be at least 8 characters');
      }

      if (!user.age || user.age < 18) {
        errors.push('User must be 18 or older');
      }

      return {
        valid: errors.length === 0,
        errors
      };
    };

    // Invalid user data
    const invalidUser = {
      email: 'invalid-email',
      password: 'short',
      age: 15
    };

    const validation = validateUser(invalidUser);

    if (!validation.valid) {
      cy.log('Validation failed with errors:');
      validation.errors.forEach(error => cy.log(`- ${error}`));

      expect(validation.errors).to.have.length(3);
      expect(validation.errors[0]).to.include('Invalid email');
    }

    // Valid user data
    const validUser = {
      email: 'valid@example.com',
      password: 'SecurePass123',
      age: 25
    };

    const validation2 = validateUser(validUser);
    expect(validation2.valid).to.be.true;
    expect(validation2.errors).to.have.length(0);
  });

  it('should implement retry on error', () => {
    cy.log('Retry on error demonstration');

    let attempts = 0;

    const unreliableFunction = () => {
      attempts++;
      cy.log(`Attempt ${attempts}`);

      if (attempts < 3) {
        throw new Error(`Failure on attempt ${attempts}`);
      }

      return { success: true, attempts };
    };

    const retryOnError = (func, maxRetries = 3) => {
      let currentAttempt = 0;

      const attempt = () => {
        try {
          return func();
        } catch (error) {
          currentAttempt++;

          if (currentAttempt < maxRetries) {
            cy.log(`Retrying... (${currentAttempt}/${maxRetries})`);
            return attempt();
          } else {
            cy.log('Max retries reached');
            throw error;
          }
        }
      };

      return attempt();
    };

    const result = retryOnError(unreliableFunction, 5);

    expect(result.success).to.be.true;
    expect(result.attempts).to.eq(3);
    cy.log('Operation succeeded after retries');
  });

  it('should handle network timeouts', () => {
    cy.log('Network timeout handling');

    const slowRequest = () => {
      return new Promise((resolve) => {
        setTimeout(() => {
          resolve({ data: 'Response' });
        }, 5000); // 5 second delay
      });
    };

    const withTimeout = (promise, timeoutMs, onTimeout) => {
      const timeoutPromise = new Promise((_, reject) => {
        setTimeout(() => {
          if (onTimeout) {
            onTimeout();
          }
          reject(new Error(`Request timed out after ${timeoutMs}ms`));
        }, timeoutMs);
      });

      return Promise.race([promise, timeoutPromise]);
    };

    cy.wrap(null).then(() => {
      return withTimeout(
        slowRequest(),
        2000,
        () => cy.log('Timeout handler: Cleaning up resources')
      );
    }).then(
      (result) => {
        cy.log('Request succeeded:', result);
      },
      (error) => {
        cy.log('Timeout error handled:', error.message);
        expect(error.message).to.include('timed out');
      }
    );
  });

  it('should implement circuit breaker pattern', () => {
    cy.log('Circuit breaker pattern');

    class CircuitBreaker {
      constructor(threshold = 3, timeout = 5000) {
        this.failureCount = 0;
        this.threshold = threshold;
        this.timeout = timeout;
        this.state = 'CLOSED'; // CLOSED, OPEN, HALF_OPEN
        this.nextAttempt = Date.now();
      }

      async call(func) {
        if (this.state === 'OPEN') {
          if (Date.now() < this.nextAttempt) {
            throw new Error('Circuit breaker is OPEN');
          }
          this.state = 'HALF_OPEN';
        }

        try {
          const result = await func();
          this.onSuccess();
          return result;
        } catch (error) {
          this.onFailure();
          throw error;
        }
      }

      onSuccess() {
        this.failureCount = 0;
        this.state = 'CLOSED';
        cy.log('Circuit: CLOSED');
      }

      onFailure() {
        this.failureCount++;
        cy.log(`Failure count: ${this.failureCount}`);

        if (this.failureCount >= this.threshold) {
          this.state = 'OPEN';
          this.nextAttempt = Date.now() + this.timeout;
          cy.log('Circuit: OPEN');
        }
      }
    }

    const breaker = new CircuitBreaker(3, 2000);
    let callCount = 0;

    const unreliableAPI = () => {
      callCount++;
      if (callCount < 4) {
        return Promise.reject(new Error('API Error'));
      }
      return Promise.resolve({ success: true });
    };

    // Multiple attempts
    const attempts = [];

    for (let i = 0; i < 5; i++) {
      attempts.push(
        breaker.call(unreliableAPI)
          .then(result => ({ attempt: i + 1, result }))
          .catch(error => ({ attempt: i + 1, error: error.message }))
      );
    }

    Promise.all(attempts).then(results => {
      cy.log('Circuit breaker results:', JSON.stringify(results, null, 2));
      expect(results[4].error).to.include('Circuit breaker is OPEN');
    });
  });

  it('should handle element not found errors', () => {
    cy.log('Element not found handling');

    const safeGetElement = (selector, options = {}) => {
      const { timeout = 5000, fallback = null } = options;

      return cy.get('body').then($body => {
        const element = $body.find(selector);

        if (element.length > 0) {
          cy.log(`Element found: ${selector}`);
          return cy.wrap(element);
        } else {
          cy.log(`Element not found: ${selector}, using fallback`);
          return fallback;
        }
      });
    };

    // Simulate checking for optional element
    safeGetElement('[data-cy="non-existent"]', { fallback: 'default' })
      .then(result => {
        cy.log('Result:', result);
        expect(result).to.eq('default');
      });
  });

  it('should create error logging utility', () => {
    cy.log('Error logging utility');

    const errorLogger = {
      errors: [],

      log(error, context = {}) {
        const errorEntry = {
          message: error.message,
          stack: error.stack,
          context,
          timestamp: new Date().toISOString()
        };

        this.errors.push(errorEntry);
        cy.log('Error logged:', error.message);

        return errorEntry;
      },

      getErrors() {
        return this.errors;
      },

      clearErrors() {
        this.errors = [];
      },

      hasErrors() {
        return this.errors.length > 0;
      }
    };

    // Simulate errors
    try {
      throw new Error('Test error 1');
    } catch (error) {
      errorLogger.log(error, { test: 'Error handling', step: 1 });
    }

    try {
      throw new Error('Test error 2');
    } catch (error) {
      errorLogger.log(error, { test: 'Error handling', step: 2 });
    }

    expect(errorLogger.hasErrors()).to.be.true;
    expect(errorLogger.getErrors()).to.have.length(2);

    cy.log('All logged errors:', JSON.stringify(errorLogger.getErrors(), null, 2));
  });

  it('should handle assertion errors gracefully', () => {
    cy.log('Graceful assertion error handling');

    const softAssert = (condition, message) => {
      try {
        expect(condition).to.be.true;
        return { passed: true, message };
      } catch (error) {
        cy.log(`Soft assertion failed: ${message}`);
        return { passed: false, message, error: error.message };
      }
    };

    const results = [];

    results.push(softAssert(1 + 1 === 2, 'Math works'));
    results.push(softAssert('hello'.length === 10, 'String length check'));
    results.push(softAssert([1, 2, 3].length === 3, 'Array length check'));
    results.push(softAssert(true === false, 'Boolean check'));

    cy.log('Assertion results:', JSON.stringify(results, null, 2));

    const failed = results.filter(r => !r.passed);
    cy.log(`Failed assertions: ${failed.length}`);

    expect(failed).to.have.length(2);
  });

  it('should implement error recovery mechanism', () => {
    cy.log('Error recovery mechanism');

    const executeWithRecovery = (operation, recovery) => {
      return cy.wrap(null).then(() => {
        try {
          return operation();
        } catch (error) {
          cy.log('Error occurred, attempting recovery...');
          cy.log('Error:', error.message);

          try {
            const result = recovery(error);
            cy.log('Recovery successful');
            return result;
          } catch (recoveryError) {
            cy.log('Recovery failed');
            throw recoveryError;
          }
        }
      });
    };

    // Test with recovery
    const riskyOperation = () => {
      throw new Error('Operation failed');
    };

    const recoveryAction = (error) => {
      cy.log('Executing recovery action');
      return { recovered: true, originalError: error.message };
    };

    executeWithRecovery(riskyOperation, recoveryAction)
      .then(result => {
        cy.log('Result:', JSON.stringify(result));
        expect(result.recovered).to.be.true;
      });
  });

  it('should handle multiple error types', () => {
    cy.log('Handling multiple error types');

    const handleError = (error) => {
      if (error instanceof TypeError) {
        cy.log('Type Error:', error.message);
        return 'TYPE_ERROR';
      } else if (error instanceof ReferenceError) {
        cy.log('Reference Error:', error.message);
        return 'REFERENCE_ERROR';
      } else if (error instanceof SyntaxError) {
        cy.log('Syntax Error:', error.message);
        return 'SYNTAX_ERROR';
      } else {
        cy.log('Generic Error:', error.message);
        return 'GENERIC_ERROR';
      }
    };

    // Type Error
    try {
      null.toString();
    } catch (error) {
      const errorType = handleError(error);
      expect(errorType).to.eq('TYPE_ERROR');
    }

    // Syntax Error
    try {
      JSON.parse('{ invalid }');
    } catch (error) {
      const errorType = handleError(error);
      expect(errorType).to.eq('SYNTAX_ERROR');
    }

    cy.log('Multiple error types handled successfully');
  });

  it('should implement comprehensive error handling flow', () => {
    cy.log('===== Comprehensive Error Handling Flow =====');

    const errorHandler = {
      handle(error, context) {
        cy.log(`[${context.stage}] Error occurred:`, error.message);

        // Log error
        this.logError(error, context);

        // Attempt recovery
        if (this.canRecover(error)) {
          cy.log('Recovery possible, attempting...');
          return this.recover(error, context);
        }

        // Graceful degradation
        cy.log('No recovery possible, using fallback');
        return this.fallback(context);
      },

      logError(error, context) {
        cy.log('Logging error to system');
        cy.log('Context:', JSON.stringify(context));
      },

      canRecover(error) {
        return error.message.includes('timeout') ||
               error.message.includes('network');
      },

      recover(error, context) {
        cy.log('Executing recovery strategy');
        return { recovered: true, context };
      },

      fallback(context) {
        cy.log('Using fallback strategy');
        return { fallback: true, context };
      }
    };

    // Test the error handler
    const testError = new Error('Network timeout occurred');
    const context = { stage: 'API Call', operation: 'fetchUsers' };

    const result = errorHandler.handle(testError, context);

    cy.log('Error handling result:', JSON.stringify(result, null, 2));
    expect(result.recovered).to.be.true;

    cy.log('===== Error Handling Flow Complete =====');
  });
});
```

### Deliverables Checklist
- [ ] Handled expected errors with try-catch
- [ ] Used failOnStatusCode for API errors
- [ ] Implemented retry mechanisms
- [ ] Handled network timeouts
- [ ] Created circuit breaker pattern
- [ ] Implemented error logging
- [ ] Created soft assertions
- [ ] Built error recovery mechanisms
- [ ] All tests pass

### Extension Challenges
1. Create a comprehensive error handling library
2. Implement error reporting with screenshots
3. Build an error recovery decision tree
4. Create custom error types for domain-specific errors

---

## Exercise 15: Complete Mini-Project - E-Commerce Test Suite {#exercise-15}

### Objective
Build a complete end-to-end test suite for an e-commerce application using all learned concepts.

### Scenario
Create a comprehensive test suite covering user registration, login, product browsing, cart management, and checkout flow.

### Step-by-Step Instructions

1. Create project structure with page objects
2. Create test data fixtures
3. Create custom commands
4. Implement API tests
5. Create E2E user journey tests
6. Add error handling and retry logic
7. Generate test reports

### Code Template

```javascript
// This is a comprehensive project - see solution for complete structure
```

### Expected Output
- Complete, working test suite
- Good code organization
- Reusable components
- Comprehensive coverage

### Solution Code

**Project Structure:**
```
cypress/
├── e2e/
│   ├── 15-ecommerce-suite.cy.js
│   ├── api/
│   │   ├── auth.api.cy.js
│   │   ├── products.api.cy.js
│   │   └── orders.api.cy.js
│   └── e2e/
│       ├── user-registration.e2e.cy.js
│       ├── shopping-flow.e2e.cy.js
│       └── checkout.e2e.cy.js
├── fixtures/
│   ├── users.json
│   ├── products.json
│   └── test-config.json
├── pages/
│   ├── BasePage.js
│   ├── HomePage.js
│   ├── LoginPage.js
│   ├── ProductsPage.js
│   ├── CartPage.js
│   └── CheckoutPage.js
├── support/
│   ├── commands.js
│   ├── api-utils.js
│   └── test-utils.js
└── utils/
    ├── data-generator.js
    └── validators.js
```

**cypress/e2e/15-ecommerce-suite.cy.js:**
```javascript
describe('Exercise 15: Complete E-Commerce Test Suite', () => {

  // Test data
  const testUser = {
    firstName: 'John',
    lastName: 'Doe',
    email: `testuser_${Date.now()}@example.com`,
    password: 'SecurePass123!',
    address: {
      street: '123 Test Street',
      city: 'Test City',
      state: 'TS',
      zip: '12345',
      country: 'TestCountry'
    },
    payment: {
      cardNumber: '4111111111111111',
      cardName: 'John Doe',
      expiryMonth: '12',
      expiryYear: '2025',
      cvv: '123'
    }
  };

  const products = [
    { id: 1, name: 'Laptop', price: 999.99, category: 'Electronics' },
    { id: 2, name: 'Mouse', price: 29.99, category: 'Accessories' },
    { id: 3, name: 'Keyboard', price: 79.99, category: 'Accessories' }
  ];

  describe('Test Suite Setup', () => {
    it('should verify test environment', () => {
      cy.log('Environment: Staging');
      cy.log('Test user:', testUser.email);
      cy.log('Products to test:', products.length);

      expect(testUser.email).to.include('@');
      expect(products).to.have.length(3);
    });
  });

  describe('User Registration Flow', () => {
    it('should register new user successfully', () => {
      cy.log('===== User Registration =====');

      cy.log('Step 1: Navigate to registration page');
      // In real test: cy.visit('/register');

      cy.log('Step 2: Fill registration form');
      const formData = {
        firstName: testUser.firstName,
        lastName: testUser.lastName,
        email: testUser.email,
        password: testUser.password,
        confirmPassword: testUser.password
      };

      cy.log('Form data:', JSON.stringify(formData, null, 2));

      cy.log('Step 3: Submit registration');
      // In real test: cy.get('[data-cy="register-button"]').click();

      cy.log('Step 4: Verify registration success');
      // In real test: cy.url().should('include', '/welcome');
      // In real test: cy.get('[data-cy="success-message"]').should('be.visible');

      cy.log('User registered successfully');
    });

    it('should validate registration form fields', () => {
      cy.log('Testing form validation');

      const invalidCases = [
        { email: 'invalid-email', expected: 'Invalid email format' },
        { password: 'short', expected: 'Password too short' },
        { password: 'noDigits', expected: 'Password must contain digits' }
      ];

      invalidCases.forEach((testCase, index) => {
        cy.log(`Validation test ${index + 1}:`, testCase.expected);
        // In real test: Submit form with invalid data and verify error
      });

      cy.log('Form validation tests completed');
    });
  });

  describe('Authentication Flow', () => {
    it('should login with valid credentials', () => {
      cy.log('===== Login Flow =====');

      cy.log('Step 1: Navigate to login page');
      // cy.visit('/login');

      cy.log('Step 2: Enter credentials');
      // cy.get('[data-cy="email"]').type(testUser.email);
      // cy.get('[data-cy="password"]').type(testUser.password);

      cy.log('Step 3: Submit login form');
      // cy.get('[data-cy="login-button"]').click();

      cy.log('Step 4: Verify successful login');
      // cy.url().should('include', '/dashboard');
      // cy.get('[data-cy="user-menu"]').should('contain', testUser.firstName);

      cy.log('Login successful');
    });

    it('should handle invalid login attempts', () => {
      cy.log('Testing invalid login');

      const invalidCredentials = {
        email: testUser.email,
        password: 'WrongPassword123!'
      };

      cy.log('Attempting login with invalid password');
      // Submit form with invalid credentials

      cy.log('Verifying error message');
      // cy.get('[data-cy="error-message"]').should('contain', 'Invalid credentials');

      cy.log('Invalid login handled correctly');
    });
  });

  describe('Product Browsing and Search', () => {
    beforeEach(() => {
      cy.log('Setup: Login user');
      // cy.loginAsRole('user');
      // cy.visit('/products');
    });

    it('should display all products', () => {
      cy.log('===== Product Listing =====');

      cy.log('Step 1: Navigate to products page');

      cy.log('Step 2: Verify products are displayed');
      // cy.get('[data-cy="product-card"]').should('have.length.greaterThan', 0);

      products.forEach(product => {
        cy.log(`Product: ${product.name} - $${product.price}`);
      });

      cy.log('All products displayed');
    });

    it('should search for products', () => {
      cy.log('Testing product search');

      const searchTerm = 'Laptop';

      cy.log(`Searching for: ${searchTerm}`);
      // cy.get('[data-cy="search-input"]').type(searchTerm);
      // cy.get('[data-cy="search-button"]').click();

      cy.log('Verifying search results');
      // cy.get('[data-cy="product-card"]').should('contain', searchTerm);

      cy.log('Search completed successfully');
    });

    it('should filter products by category', () => {
      cy.log('Testing product filtering');

      const category = 'Electronics';

      cy.log(`Filtering by: ${category}`);
      // cy.get('[data-cy="filter-category"]').select(category);

      cy.log('Verifying filtered results');
      // Verify all displayed products are in selected category

      cy.log('Filter applied successfully');
    });

    it('should sort products by price', () => {
      cy.log('Testing product sorting');

      cy.log('Sorting by price: Low to High');
      // cy.get('[data-cy="sort-dropdown"]').select('price-low-high');

      cy.log('Verifying sort order');
      // Verify products are in ascending price order

      cy.log('Sorting completed');
    });
  });

  describe('Shopping Cart Management', () => {
    beforeEach(() => {
      cy.log('Setup: Login and navigate to products');
      // cy.loginAsRole('user');
      // cy.visit('/products');
    });

    it('should add products to cart', () => {
      cy.log('===== Add to Cart =====');

      products.slice(0, 2).forEach(product => {
        cy.log(`Adding to cart: ${product.name}`);
        // cy.contains('[data-cy="product-card"]', product.name)
        //   .find('[data-cy="add-to-cart"]')
        //   .click();

        cy.log(`${product.name} added to cart`);
      });

      cy.log('Verifying cart count');
      // cy.get('[data-cy="cart-count"]').should('contain', '2');

      cy.log('Products added to cart successfully');
    });

    it('should update product quantity in cart', () => {
      cy.log('Testing quantity update');

      cy.log('Navigate to cart');
      // cy.get('[data-cy="cart-icon"]').click();

      cy.log('Update first product quantity to 3');
      // cy.get('[data-cy="quantity-input"]').first().clear().type('3');

      cy.log('Verify total price updated');
      // Verify calculated total matches expected

      cy.log('Quantity updated successfully');
    });

    it('should remove product from cart', () => {
      cy.log('Testing product removal');

      cy.log('Navigate to cart');
      // cy.get('[data-cy="cart-icon"]').click();

      const initialCount = 2;

      cy.log(`Initial cart count: ${initialCount}`);

      cy.log('Remove first product');
      // cy.get('[data-cy="remove-item"]').first().click();

      cy.log('Verify cart updated');
      // cy.get('[data-cy="cart-item"]').should('have.length', initialCount - 1);

      cy.log('Product removed successfully');
    });

    it('should calculate cart total correctly', () => {
      cy.log('Testing cart total calculation');

      const cartItems = [
        { product: products[0], quantity: 2 },
        { product: products[1], quantity: 1 }
      ];

      const expectedTotal = cartItems.reduce(
        (total, item) => total + (item.product.price * item.quantity),
        0
      );

      cy.log('Expected total:', expectedTotal.toFixed(2));

      cy.log('Navigate to cart and verify total');
      // cy.get('[data-cy="cart-total"]').should('contain', expectedTotal.toFixed(2));

      cy.log('Cart total verified');
    });
  });

  describe('Checkout Process', () => {
    beforeEach(() => {
      cy.log('Setup: Login, add products, navigate to checkout');
      // cy.loginAsRole('user');
      // Add products to cart
      // Navigate to checkout
    });

    it('should complete checkout with valid payment', () => {
      cy.log('===== Checkout Flow =====');

      cy.log('Step 1: Verify cart summary');
      // Verify items and total

      cy.log('Step 2: Enter shipping address');
      Object.entries(testUser.address).forEach(([field, value]) => {
        cy.log(`${field}: ${value}`);
        // cy.get(`[data-cy="address-${field}"]`).type(value);
      });

      cy.log('Step 3: Select shipping method');
      // cy.get('[data-cy="shipping-standard"]').click();

      cy.log('Step 4: Enter payment details');
      Object.entries(testUser.payment).forEach(([field, value]) => {
        if (field !== 'cvv') { // Don't log CVV
          cy.log(`${field}: ${value}`);
        }
        // cy.get(`[data-cy="payment-${field}"]`).type(value);
      });

      cy.log('Step 5: Review order');
      // Verify all details are correct

      cy.log('Step 6: Place order');
      // cy.get('[data-cy="place-order-button"]').click();

      cy.log('Step 7: Verify order confirmation');
      // cy.url().should('include', '/order-confirmation');
      // cy.get('[data-cy="order-number"]').should('exist');

      cy.log('Order placed successfully');
    });

    it('should validate payment information', () => {
      cy.log('Testing payment validation');

      const invalidPaymentCases = [
        { cardNumber: '1234', error: 'Invalid card number' },
        { expiryMonth: '13', error: 'Invalid month' },
        { cvv: '12', error: 'CVV must be 3 digits' }
      ];

      invalidPaymentCases.forEach((testCase, index) => {
        cy.log(`Validation test ${index + 1}: ${testCase.error}`);
        // Submit form with invalid data and verify error
      });

      cy.log('Payment validation completed');
    });

    it('should calculate order total with tax and shipping', () => {
      cy.log('Testing order total calculation');

      const subtotal = 1029.98;
      const shipping = 9.99;
      const taxRate = 0.08;
      const tax = subtotal * taxRate;
      const total = subtotal + shipping + tax;

      cy.log('Subtotal:', subtotal.toFixed(2));
      cy.log('Shipping:', shipping.toFixed(2));
      cy.log('Tax (8%):', tax.toFixed(2));
      cy.log('Total:', total.toFixed(2));

      cy.log('Verify calculated total');
      // cy.get('[data-cy="order-total"]').should('contain', total.toFixed(2));

      cy.log('Total calculation verified');
    });
  });

  describe('Order History', () => {
    it('should display order history', () => {
      cy.log('===== Order History =====');

      cy.log('Navigate to order history');
      // cy.visit('/orders');

      cy.log('Verify orders are displayed');
      // cy.get('[data-cy="order-item"]').should('have.length.greaterThan', 0);

      cy.log('Verify order details');
      // Check order number, date, total, status

      cy.log('Order history displayed correctly');
    });

    it('should view order details', () => {
      cy.log('Testing order details view');

      cy.log('Click on first order');
      // cy.get('[data-cy="order-item"]').first().click();

      cy.log('Verify order details page');
      // cy.url().should('include', '/orders/');

      cy.log('Verify all order information');
      // Items, quantities, prices, shipping, payment

      cy.log('Order details verified');
    });
  });

  describe('API Integration Tests', () => {
    it('should fetch products via API', () => {
      cy.log('Testing products API');

      cy.request('GET', '/api/products').then((response) => {
        expect(response.status).to.eq(200);
        expect(response.body).to.be.an('array');
        expect(response.body).to.have.length.greaterThan(0);

        cy.log('Products API response:', JSON.stringify(response.body.slice(0, 2), null, 2));
      });
    });

    it('should create order via API', () => {
      cy.log('Testing order creation API');

      const orderData = {
        items: [
          { productId: products[0].id, quantity: 2 },
          { productId: products[1].id, quantity: 1 }
        ],
        shippingAddress: testUser.address,
        paymentMethod: 'credit_card'
      };

      cy.log('Order data:', JSON.stringify(orderData, null, 2));

      // In real test:
      // cy.request('POST', '/api/orders', orderData).then((response) => {
      //   expect(response.status).to.eq(201);
      //   expect(response.body).to.have.property('orderId');
      //   expect(response.body).to.have.property('total');
      // });

      cy.log('Order created via API');
    });
  });

  describe('End-to-End User Journey', () => {
    it('should complete full shopping journey', () => {
      cy.log('===== COMPLETE E2E TEST =====');

      cy.log('Journey Step 1: User Registration');
      // Register new user
      cy.log(`Registered: ${testUser.email}`);

      cy.log('Journey Step 2: Browse Products');
      // Navigate to products, view different categories
      cy.log('Browsed 10 products');

      cy.log('Journey Step 3: Search and Filter');
      // Search for specific product
      // Apply filters
      cy.log('Found desired products');

      cy.log('Journey Step 4: Add to Cart');
      // Add multiple products to cart
      cy.log('Added 3 products to cart');

      cy.log('Journey Step 5: Manage Cart');
      // Update quantities
      // Remove one item
      cy.log('Cart updated: 2 items remaining');

      cy.log('Journey Step 6: Checkout');
      // Enter shipping and payment details
      // Place order
      cy.log('Order placed successfully');

      cy.log('Journey Step 7: Order Confirmation');
      // Verify order confirmation
      // Note order number
      cy.log('Order #12345 confirmed');

      cy.log('Journey Step 8: View Order History');
      // Navigate to order history
      // Verify order appears
      cy.log('Order visible in history');

      cy.log('===== E2E JOURNEY COMPLETE =====');
      cy.log('✓ Full user journey successful!');
    });
  });

  describe('Test Suite Summary', () => {
    it('should generate test report', () => {
      cy.log('===== Test Suite Summary =====');

      const summary = {
        totalTests: 25,
        passed: 23,
        failed: 0,
        skipped: 2,
        duration: '2m 34s',
        coverage: {
          registration: '100%',
          authentication: '100%',
          productBrowsing: '100%',
          cart: '100%',
          checkout: '100%',
          orders: '100%',
          api: '100%'
        }
      };

      cy.log('Test Results:', JSON.stringify(summary, null, 2));

      const passRate = Math.round((summary.passed / summary.totalTests) * 100);
      cy.log(`Pass Rate: ${passRate}%`);

      expect(passRate).to.be.greaterThan(90);

      cy.log('===== Test Suite Complete =====');
    });
  });
});
```

### Deliverables Checklist
- [ ] Created complete project structure
- [ ] Implemented page object model
- [ ] Created reusable custom commands
- [ ] Built comprehensive test fixtures
- [ ] Implemented API tests
- [ ] Created E2E user journey tests
- [ ] Added error handling
- [ ] Used all JavaScript concepts learned
- [ ] Tests are maintainable and scalable
- [ ] Documentation is clear
- [ ] All tests pass

### Extension Challenges
1. Add TypeScript for type safety
2. Implement visual regression testing
3. Add accessibility testing
4. Create CI/CD pipeline configuration
5. Implement test data management system
6. Add performance testing
7. Create detailed test reports with Mochawesome
8. Implement test parallelization

---

## Conclusion

Congratulations on completing all 15 JavaScript exercises for Cypress test automation! You've covered:

1. **JavaScript Fundamentals**: Variables, data types, control structures
2. **Functions**: Declarations, expressions, arrow functions, higher-order functions
3. **Data Structures**: Objects, arrays, and their manipulation
4. **Functional Programming**: Map, filter, reduce
5. **Asynchronous JavaScript**: Promises, async/await
6. **Modern ES6+**: Destructuring, spread operator, template literals
7. **JSON**: Parsing, manipulation, validation
8. **Cypress-Specific**: Custom commands, page objects, API testing
9. **Advanced Patterns**: Error handling, retry mechanisms, async operations
10. **Complete Project**: Full e-commerce test suite

### Next Steps

- Practice these concepts regularly
- Build your own test automation projects
- Explore advanced Cypress features
- Learn TypeScript for enhanced type safety
- Study continuous integration and deployment
- Contribute to open-source testing projects

### Resources

- Cypress Documentation: https://docs.cypress.io
- JavaScript MDN: https://developer.mozilla.org/en-US/docs/Web/JavaScript
- Testing Best Practices: https://testingjavascript.com

Happy Testing! 🚀

