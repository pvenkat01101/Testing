# Phase 6-07: Cypress Interview Questions & Answers

## Table of Contents
- [Fundamentals](#fundamentals)
- [Cypress vs Selenium](#cypress-vs-selenium)
- [Architecture & Internals](#architecture--internals)
- [Auto-Waiting & Retry-ability](#auto-waiting--retry-ability)
- [Custom Commands & Utilities](#custom-commands--utilities)
- [Network Interception (cy.intercept)](#network-interception-cyintercept)
- [Fixtures & Test Data](#fixtures--test-data)
- [Best Practices](#best-practices)
- [Debugging](#debugging)
- [Limitations & Workarounds](#limitations--workarounds)
- [Advanced Topics](#advanced-topics)

---

## Fundamentals

### Q1: What is Cypress and why is it used?

**Answer:**
Cypress is a next-generation front-end testing tool built for the modern web. It is used for writing end-to-end tests, integration tests, and unit tests for web applications.

**Key characteristics:**
- Runs directly in the browser alongside the application
- Written entirely in JavaScript
- Does not use Selenium or WebDriver
- Provides a real-time interactive Test Runner
- Has built-in automatic waiting, screenshots, and video recording

```javascript
// A simple Cypress test
describe('Login Page', () => {
  it('should log in successfully with valid credentials', () => {
    cy.visit('/login');
    cy.get('#username').type('testuser');
    cy.get('#password').type('password123');
    cy.get('button[type="submit"]').click();
    cy.url().should('include', '/dashboard');
    cy.get('.welcome-message').should('contain', 'Welcome, testuser');
  });
});
```

---

### Q2: What are the different types of testing Cypress supports?

**Answer:**

| Test Type | Description | Example |
|-----------|-------------|---------|
| **End-to-End (E2E)** | Tests the full application flow from the user's perspective | Login, checkout, form submissions |
| **Component** | Tests individual UI components in isolation | React/Angular/Vue component rendering |
| **API Testing** | Tests REST API endpoints using `cy.request()` | Validating response status, body, headers |
| **Integration** | Tests interaction between multiple components/modules | Form submission triggering API call |

```javascript
// E2E Test
it('completes a purchase flow', () => {
  cy.visit('/products');
  cy.get('.product-card').first().click();
  cy.get('#add-to-cart').click();
  cy.get('#checkout').click();
  cy.get('#confirm-purchase').click();
  cy.get('.success-message').should('be.visible');
});

// API Test
it('fetches user data via API', () => {
  cy.request('GET', '/api/users/1').then((response) => {
    expect(response.status).to.eq(200);
    expect(response.body).to.have.property('name');
    expect(response.body.name).to.eq('John Doe');
  });
});

// Component Test (React example)
import { mount } from 'cypress/react';
import Button from './Button';

it('renders a button with correct text', () => {
  mount(<Button label="Click Me" />);
  cy.get('button').should('have.text', 'Click Me');
});
```

---

## Cypress vs Selenium

### Q3: What are the key differences between Cypress and Selenium?

**Answer:**

| Feature | Cypress | Selenium |
|---------|---------|----------|
| **Architecture** | Runs inside the browser | Communicates via WebDriver protocol |
| **Language Support** | JavaScript/TypeScript only | Java, Python, C#, Ruby, JavaScript, etc. |
| **Browser Support** | Chrome, Firefox, Edge, Electron | Chrome, Firefox, Edge, Safari, IE |
| **Speed** | Very fast (no network latency) | Slower (HTTP commands over network) |
| **Auto-Waiting** | Built-in | Manual waits required |
| **Setup** | `npm install cypress` (zero config) | Requires driver binaries, language bindings |
| **Parallel Execution** | Via Cypress Cloud or third-party tools | Native support via Selenium Grid |
| **Multi-tab/window** | Not supported natively | Fully supported |
| **iFrame Support** | Limited (requires plugin) | Native support |
| **Cross-Origin** | Limited (improved in v12+) | Full support |
| **Mobile Testing** | No native mobile browser testing | Supported via Appium |
| **Community** | Growing rapidly | Very large, mature ecosystem |
| **Debugging** | Time-travel, snapshots, DevTools | Screenshots, logs, remote debugging |

---

### Q4: When would you choose Cypress over Selenium and vice versa?

**Answer:**

**Choose Cypress when:**
- Your application is a modern JavaScript SPA (React, Angular, Vue)
- Your team is primarily JavaScript/TypeScript developers
- You want fast test execution and quick feedback
- You need built-in time-travel debugging
- You are testing a single-domain application
- You want minimal setup and configuration

**Choose Selenium when:**
- You need multi-browser support including Safari and IE
- Your team works with Java, Python, C#, or other non-JS languages
- You need to test multi-tab or multi-window scenarios
- You require cross-origin testing across multiple domains
- You need mobile browser testing
- You need integration with existing enterprise CI/CD using Selenium Grid
- Your application involves heavy iframe usage

---

## Architecture & Internals

### Q5: Explain Cypress architecture. How does it differ from Selenium's?

**Answer:**

**Selenium Architecture:**
```
Test Script --> WebDriver API --> Browser Driver (chromedriver) --> Browser
     |                                    |
     └──── HTTP/JSON Wire Protocol ───────┘
```
Selenium operates outside the browser, sending commands over HTTP to a browser driver, which then controls the browser. This introduces network latency.

**Cypress Architecture:**
```
┌─────────────────────────────────────┐
│           Browser                    │
│  ┌─────────────────────────────┐    │
│  │   Application Under Test    │    │
│  │   (your-app.com)            │    │
│  └─────────────────────────────┘    │
│  ┌─────────────────────────────┐    │
│  │   Cypress Test Code         │    │
│  │   (spec files)              │    │
│  └─────────────────────────────┘    │
│                                      │
│  Both run in the SAME event loop     │
└─────────────────────────────────────┘
         │
         │ (IPC - Inter-Process Communication)
         │
┌─────────────────────────────────────┐
│   Node.js Server Process             │
│   - File system access               │
│   - Network proxy                    │
│   - Task execution (cy.task)         │
│   - Plugin execution                 │
└─────────────────────────────────────┘
```

**Key architectural differences:**
1. **Cypress runs inside the browser** - it has direct access to the DOM, window, document, and application code
2. **Same event loop** - test code and application code share the same JavaScript execution context
3. **Node.js backend** - a Node.js process handles tasks that cannot be done in the browser (file I/O, database operations)
4. **Network proxy** - Cypress acts as a proxy to intercept and modify network requests/responses

---

### Q6: What is the Cypress command queue and how does it work?

**Answer:**

Cypress commands are **asynchronous** and are **enqueued** rather than executed immediately. When you write Cypress code, each `cy.*` command is added to a queue, and Cypress executes them serially in order.

```javascript
it('demonstrates command queue', () => {
  // These commands are ENQUEUED, not executed immediately
  cy.visit('/page');          // Command 1 - added to queue
  cy.get('.element');         // Command 2 - added to queue
  cy.click();                // Command 3 - added to queue

  // This runs BEFORE any cy commands execute!
  console.log('This prints first!');
});
```

**How the queue works:**
1. Cypress collects all `cy.*` commands during test function execution
2. After the test function returns, Cypress begins executing commands from the queue
3. Each command waits for the previous one to complete before running
4. Commands automatically retry until they pass or timeout

```javascript
// Correct way to work with command results
cy.get('.count').invoke('text').then((text) => {
  // Use .then() to access the yielded value
  const count = parseInt(text);
  expect(count).to.be.greaterThan(0);
});

// WRONG - this will not work as expected
const element = cy.get('.count'); // Returns Chainable, not the element
console.log(element.text());     // Error! Not how Cypress works
```

---

## Auto-Waiting & Retry-ability

### Q7: How does Cypress auto-waiting work?

**Answer:**

Cypress automatically waits for commands and assertions to pass before moving on. There is no need for explicit waits like `Thread.sleep()` or `WebDriverWait`.

**What Cypress waits for automatically:**
- DOM elements to exist
- Elements to become visible
- Elements to not be covered by another element
- Elements to not be disabled
- Elements to stop animating
- Page loads and transitions
- XHR/Fetch requests (when intercepted)

```javascript
// Cypress automatically waits up to defaultCommandTimeout (4 seconds by default)
cy.get('.dynamic-element')        // Waits for element to exist in DOM
  .should('be.visible')           // Waits for element to be visible
  .click();                       // Waits for element to be clickable

// No explicit waits needed!
// Compare with Selenium (Java):
// WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
// wait.until(ExpectedConditions.elementToBeClickable(By.css(".dynamic-element")));
// driver.findElement(By.css(".dynamic-element")).click();
```

**Configuring timeouts:**
```javascript
// cypress.config.js
module.exports = defineConfig({
  e2e: {
    defaultCommandTimeout: 4000,    // Default wait for DOM commands
    pageLoadTimeout: 60000,         // Wait for page load
    requestTimeout: 5000,           // Wait for cy.request()
    responseTimeout: 30000,         // Wait for server response
  }
});

// Per-command timeout override
cy.get('.slow-element', { timeout: 10000 }).should('be.visible');
```

---

### Q8: What is retry-ability in Cypress? Which commands retry and which do not?

**Answer:**

**Retry-ability** means Cypress will automatically re-query the DOM and re-run assertions until they pass or timeout.

**Commands that RETRY:**
- `cy.get()` - Queries the DOM repeatedly
- `cy.find()` - Queries within a subject repeatedly
- `cy.contains()` - Searches text repeatedly
- `.should()` / `.and()` - Re-runs the assertion chain

**Commands that DO NOT RETRY:**
- `cy.click()`, `cy.type()`, `cy.check()` - Action commands
- `cy.request()` - HTTP requests
- `cy.exec()` - Shell commands
- `cy.task()` - Node.js tasks
- `.then()` - Callback commands

```javascript
// CORRECT: Assertion retries until element text changes
cy.get('.status').should('have.text', 'Completed');

// INCORRECT: .then() does NOT retry - this may fail for dynamic content
cy.get('.status').then(($el) => {
  expect($el.text()).to.eq('Completed'); // Runs only once!
});

// CORRECT: Use should() for retry-able assertions
cy.get('.items-list')
  .find('li')                           // Retries finding li elements
  .should('have.length', 5);            // Retries assertion

// IMPORTANT: Only the LAST command in the chain retries
cy.get('.parent')                       // This retries
  .find('.child')                       // This retries (last query)
  .should('have.class', 'active');      // This retries (assertion)
```

---

## Custom Commands & Utilities

### Q9: What are Cypress custom commands? How do you create them?

**Answer:**

Custom commands allow you to create reusable, chainable commands that extend Cypress's built-in functionality.

```javascript
// cypress/support/commands.js

// Parent command - starts a new chain
Cypress.Commands.add('login', (username, password) => {
  cy.visit('/login');
  cy.get('#username').type(username);
  cy.get('#password').type(password);
  cy.get('button[type="submit"]').click();
  cy.url().should('include', '/dashboard');
});

// Child command - chains off a previous command
Cypress.Commands.add('selectDropdownOption', { prevSubject: 'element' }, (subject, optionText) => {
  cy.wrap(subject).click();
  cy.get('.dropdown-option').contains(optionText).click();
});

// Dual command - works as both parent and child
Cypress.Commands.add('highlight', { prevSubject: 'optional' }, (subject) => {
  if (subject) {
    cy.wrap(subject).then(($el) => {
      $el.css('border', '2px solid red');
    });
  }
});
```

**Usage in tests:**
```javascript
describe('Dashboard', () => {
  beforeEach(() => {
    cy.login('admin', 'password123');  // Custom parent command
  });

  it('selects a category', () => {
    cy.get('#category-dropdown')
      .selectDropdownOption('Electronics');  // Custom child command
    cy.get('.products').should('be.visible');
  });
});
```

**TypeScript support (adding type definitions):**
```typescript
// cypress/support/index.d.ts
declare namespace Cypress {
  interface Chainable<Subject = any> {
    login(username: string, password: string): Chainable<void>;
    selectDropdownOption(optionText: string): Chainable<JQuery<HTMLElement>>;
    highlight(): Chainable<JQuery<HTMLElement>>;
  }
}
```

---

### Q10: What is the difference between custom commands and utility functions?

**Answer:**

| Aspect | Custom Commands | Utility Functions |
|--------|----------------|-------------------|
| **Defined in** | `Cypress.Commands.add()` | Regular JS/TS functions |
| **Chainable** | Yes, integrates with command queue | No, returns plain values |
| **Retry-able** | Yes, follows Cypress retry logic | No |
| **Namespace** | `cy.commandName()` | Import and call directly |
| **Best for** | UI interactions, Cypress-specific operations | Data manipulation, calculations, formatting |

```javascript
// Custom Command - for Cypress operations
Cypress.Commands.add('loginViaAPI', (username, password) => {
  cy.request('POST', '/api/auth/login', { username, password })
    .its('body.token')
    .then((token) => {
      window.localStorage.setItem('authToken', token);
    });
});

// Utility Function - for data processing
// cypress/support/utils.js
export function generateRandomEmail() {
  const timestamp = Date.now();
  return `testuser_${timestamp}@example.com`;
}

export function formatPrice(amount) {
  return `$${amount.toFixed(2)}`;
}

// Usage in test
import { generateRandomEmail, formatPrice } from '../support/utils';

it('registers a new user', () => {
  const email = generateRandomEmail();
  cy.visit('/register');
  cy.get('#email').type(email);
  // ...
});
```

---

## Network Interception (cy.intercept)

### Q11: What is cy.intercept() and how do you use it?

**Answer:**

`cy.intercept()` allows you to intercept, modify, and stub network requests and responses. It replaces the older `cy.route()` and `cy.server()` commands.

```javascript
// 1. SPY on a request (observe without modifying)
cy.intercept('GET', '/api/users').as('getUsers');
cy.visit('/users');
cy.wait('@getUsers').its('response.statusCode').should('eq', 200);

// 2. STUB a response (return fake data)
cy.intercept('GET', '/api/users', {
  statusCode: 200,
  body: [
    { id: 1, name: 'John Doe' },
    { id: 2, name: 'Jane Smith' }
  ]
}).as('getUsers');

// 3. MODIFY a response (intercept and change)
cy.intercept('GET', '/api/users', (req) => {
  req.continue((res) => {
    // Modify the response before it reaches the app
    res.body.push({ id: 999, name: 'Injected User' });
    res.send();
  });
}).as('getUsers');

// 4. DELAY a response
cy.intercept('GET', '/api/users', (req) => {
  req.continue((res) => {
    res.setDelay(3000);  // 3-second delay
    res.send();
  });
}).as('getUsers');

// 5. FORCE an error response
cy.intercept('GET', '/api/users', {
  statusCode: 500,
  body: { error: 'Internal Server Error' }
}).as('getUsers');
```

---

### Q12: How do you use cy.intercept() with URL patterns and request matching?

**Answer:**

```javascript
// String matching
cy.intercept('GET', '/api/users');
cy.intercept('POST', '/api/users');

// Glob pattern matching
cy.intercept('GET', '/api/users/*');          // Matches /api/users/1, /api/users/abc
cy.intercept('GET', '/api/**/comments');       // Matches /api/posts/1/comments

// Regex pattern matching
cy.intercept({ method: 'GET', url: /\/api\/users\/\d+/ });

// Match by multiple criteria
cy.intercept({
  method: 'POST',
  url: '/api/orders',
  headers: {
    'Content-Type': 'application/json'
  },
  body: {
    productId: 123      // Only match when body contains this
  }
}).as('createOrder');

// Conditional interception using routeHandler
cy.intercept('GET', '/api/products*', (req) => {
  const url = new URL(req.url);
  const category = url.searchParams.get('category');

  if (category === 'electronics') {
    req.reply({
      statusCode: 200,
      body: [{ id: 1, name: 'Laptop', category: 'electronics' }]
    });
  } else {
    req.continue(); // Let other requests pass through
  }
}).as('getProducts');

// Wait for multiple requests
cy.intercept('GET', '/api/users').as('getUsers');
cy.intercept('GET', '/api/products').as('getProducts');
cy.intercept('GET', '/api/settings').as('getSettings');

cy.visit('/dashboard');
cy.wait(['@getUsers', '@getProducts', '@getSettings']);
```

---

### Q13: How do you test loading states and error handling using cy.intercept()?

**Answer:**

```javascript
describe('Loading and Error States', () => {

  it('displays loading spinner while data is being fetched', () => {
    cy.intercept('GET', '/api/users', (req) => {
      req.continue((res) => {
        res.setDelay(2000); // Simulate slow network
        res.send();
      });
    }).as('getUsers');

    cy.visit('/users');
    cy.get('.loading-spinner').should('be.visible');
    cy.wait('@getUsers');
    cy.get('.loading-spinner').should('not.exist');
    cy.get('.user-list').should('be.visible');
  });

  it('displays error message when API fails', () => {
    cy.intercept('GET', '/api/users', {
      statusCode: 500,
      body: { message: 'Server Error' }
    }).as('getUsers');

    cy.visit('/users');
    cy.wait('@getUsers');
    cy.get('.error-message')
      .should('be.visible')
      .and('contain', 'Something went wrong');
    cy.get('.retry-button').should('be.visible');
  });

  it('handles network failure gracefully', () => {
    cy.intercept('GET', '/api/users', { forceNetworkError: true }).as('getUsers');

    cy.visit('/users');
    cy.wait('@getUsers');
    cy.get('.network-error').should('be.visible');
  });

  it('shows empty state when no data returned', () => {
    cy.intercept('GET', '/api/users', {
      statusCode: 200,
      body: []
    }).as('getUsers');

    cy.visit('/users');
    cy.wait('@getUsers');
    cy.get('.empty-state').should('be.visible');
    cy.get('.empty-state').should('contain', 'No users found');
  });
});
```

---

## Fixtures & Test Data

### Q14: What are fixtures in Cypress and how do you use them?

**Answer:**

Fixtures are external data files (JSON, CSV, images, etc.) stored in the `cypress/fixtures` directory. They provide test data for your tests.

```
cypress/
  fixtures/
    users.json
    products.json
    images/
      logo.png
    responses/
      login-success.json
      login-failure.json
```

```json
// cypress/fixtures/users.json
{
  "validUser": {
    "username": "testuser",
    "password": "Test@1234",
    "email": "testuser@example.com"
  },
  "adminUser": {
    "username": "admin",
    "password": "Admin@5678",
    "email": "admin@example.com"
  },
  "invalidUser": {
    "username": "invalid",
    "password": "wrong"
  }
}
```

```javascript
// Method 1: Using cy.fixture()
describe('Login Tests', () => {
  it('logs in with valid credentials', () => {
    cy.fixture('users').then((users) => {
      cy.visit('/login');
      cy.get('#username').type(users.validUser.username);
      cy.get('#password').type(users.validUser.password);
      cy.get('button[type="submit"]').click();
      cy.url().should('include', '/dashboard');
    });
  });
});

// Method 2: Using fixtures with cy.intercept()
it('displays user profile', () => {
  cy.intercept('GET', '/api/profile', { fixture: 'users.json' }).as('getProfile');
  cy.visit('/profile');
  cy.wait('@getProfile');
  cy.get('.username').should('contain', 'testuser');
});

// Method 3: Using this context (requires function keyword, not arrow)
describe('Product Tests', function() {
  beforeEach(function() {
    cy.fixture('products').as('productData');
  });

  it('displays product list', function() {
    // Access via this.productData
    cy.intercept('GET', '/api/products', this.productData).as('getProducts');
    cy.visit('/products');
    cy.wait('@getProducts');
  });
});

// Method 4: Import directly (for JSON files)
import userData from '../fixtures/users.json';

it('logs in using imported fixture', () => {
  cy.visit('/login');
  cy.get('#username').type(userData.validUser.username);
  cy.get('#password').type(userData.validUser.password);
});
```

---

## Best Practices

### Q15: What are Cypress best practices for writing stable and maintainable tests?

**Answer:**

**1. Use data-* attributes for selectors:**
```javascript
// BAD - fragile selectors
cy.get('.btn-primary');                    // CSS class may change
cy.get('#submit');                         // ID may change
cy.get('button:nth-child(3)');            // Position may change
cy.get('[style="color: red"]');           // Style may change

// GOOD - dedicated test attribute
cy.get('[data-testid="submit-button"]');
cy.get('[data-cy="login-form"]');
```

**2. Never use cy.wait() with arbitrary time:**
```javascript
// BAD
cy.wait(5000);

// GOOD - wait for specific conditions
cy.intercept('GET', '/api/data').as('getData');
cy.wait('@getData');

// GOOD - wait for element state
cy.get('.element').should('be.visible');
```

**3. Set state programmatically, not through the UI:**
```javascript
// BAD - Using UI to set up preconditions
beforeEach(() => {
  cy.visit('/login');
  cy.get('#username').type('admin');
  cy.get('#password').type('password');
  cy.get('button').click();
  cy.url().should('include', '/dashboard');
});

// GOOD - Use API to set up preconditions
beforeEach(() => {
  cy.request('POST', '/api/auth/login', {
    username: 'admin',
    password: 'password'
  }).then((response) => {
    window.localStorage.setItem('token', response.body.token);
  });
  cy.visit('/dashboard');
});
```

**4. Tests should be independent and not rely on each other:**
```javascript
// BAD - tests depend on order
it('creates a user', () => { /* ... */ });
it('edits the user created above', () => { /* ... */ });  // Depends on previous test!

// GOOD - each test sets up its own state
it('edits a user', () => {
  // Create user via API first
  cy.request('POST', '/api/users', { name: 'Test User' }).then((response) => {
    const userId = response.body.id;
    cy.visit(`/users/${userId}/edit`);
    // ... perform edit
  });
});
```

**5. Use Page Object Model or App Actions:**
```javascript
// Page Object Pattern
class LoginPage {
  visit() { cy.visit('/login'); }
  fillUsername(username) { cy.get('[data-cy="username"]').type(username); }
  fillPassword(password) { cy.get('[data-cy="password"]').type(password); }
  submit() { cy.get('[data-cy="submit"]').click(); }

  login(username, password) {
    this.visit();
    this.fillUsername(username);
    this.fillPassword(password);
    this.submit();
  }
}

export const loginPage = new LoginPage();

// Usage in test
import { loginPage } from '../pages/LoginPage';

it('logs in successfully', () => {
  loginPage.login('admin', 'password');
  cy.url().should('include', '/dashboard');
});
```

---

### Q16: How do you handle authentication in Cypress tests efficiently?

**Answer:**

```javascript
// Method 1: API-based login (RECOMMENDED - fastest)
Cypress.Commands.add('loginViaAPI', (username = 'testuser', password = 'password123') => {
  cy.request({
    method: 'POST',
    url: '/api/auth/login',
    body: { username, password }
  }).then((response) => {
    window.localStorage.setItem('authToken', response.body.token);
    window.localStorage.setItem('user', JSON.stringify(response.body.user));
  });
});

// Method 2: Session caching (Cypress 12+)
Cypress.Commands.add('loginWithSession', (username, password) => {
  cy.session([username, password], () => {
    cy.visit('/login');
    cy.get('#username').type(username);
    cy.get('#password').type(password);
    cy.get('button[type="submit"]').click();
    cy.url().should('include', '/dashboard');
  }, {
    validate() {
      cy.request('/api/auth/validate').its('status').should('eq', 200);
    }
  });
});

// Method 3: Programmatic cookie/token setup
Cypress.Commands.add('loginWithCookie', () => {
  cy.setCookie('session_id', 'abc123def456');
  cy.setCookie('auth_token', 'valid-jwt-token-here');
});

// Usage
describe('Authenticated Tests', () => {
  beforeEach(() => {
    cy.loginViaAPI('admin', 'admin123');
    cy.visit('/dashboard');
  });

  it('displays user dashboard', () => {
    cy.get('.dashboard').should('be.visible');
  });
});
```

---

## Debugging

### Q17: What debugging techniques does Cypress provide?

**Answer:**

**1. Time Travel & Snapshots:**
The Cypress Test Runner captures a DOM snapshot at each step. Click any command in the Command Log to see the state of the app at that moment.

**2. cy.debug() and cy.pause():**
```javascript
it('debugs a test', () => {
  cy.visit('/page');
  cy.get('.element')
    .debug()              // Opens browser DevTools debugger
    .should('be.visible');

  cy.pause();             // Pauses test execution; step through manually
  cy.get('.next-element').click();
});
```

**3. cy.log() for custom messages:**
```javascript
it('logs custom messages', () => {
  cy.log('Starting login flow');
  cy.get('#username').type('admin');
  cy.log('Username entered');
  cy.get('#password').type('password');
  cy.log('Password entered');
});
```

**4. Console logging with .then():**
```javascript
cy.get('.items').then(($items) => {
  console.log('Number of items:', $items.length);
  console.log('First item text:', $items.first().text());
});
```

**5. Screenshots and Videos:**
```javascript
// Manual screenshot
cy.screenshot('before-submit');

// Automatic screenshot on failure (enabled by default)
// cypress.config.js
module.exports = defineConfig({
  screenshotOnRunFailure: true,
  video: true,
  videosFolder: 'cypress/videos',
  screenshotsFolder: 'cypress/screenshots'
});
```

**6. cy.task() for Node.js debugging:**
```javascript
// cypress.config.js
module.exports = defineConfig({
  e2e: {
    setupNodeEvents(on, config) {
      on('task', {
        log(message) {
          console.log('TASK LOG:', message);
          return null;
        },
        queryDatabase(query) {
          // Run database queries for debugging
          return db.execute(query);
        }
      });
    }
  }
});

// In test
cy.task('log', 'Checking database state');
cy.task('queryDatabase', 'SELECT * FROM users WHERE id = 1').then((result) => {
  expect(result).to.have.length(1);
});
```

---

### Q18: How do you handle flaky tests in Cypress?

**Answer:**

```javascript
// 1. Use test retries (built-in)
// cypress.config.js
module.exports = defineConfig({
  retries: {
    runMode: 2,      // Retry twice in CI (cypress run)
    openMode: 0       // No retries in interactive mode (cypress open)
  }
});

// Per-test retry configuration
it('flaky test with retries', { retries: 3 }, () => {
  // ...
});

// 2. Ensure proper waiting
cy.intercept('GET', '/api/data').as('getData');
cy.visit('/page');
cy.wait('@getData');  // Wait for data to load before asserting

// 3. Use should() instead of expect() in then()
// BAD - no retry
cy.get('.count').then(($el) => {
  expect($el.text()).to.eq('5');
});

// GOOD - retries automatically
cy.get('.count').should('have.text', '5');

// 4. Avoid using fixed element indices
// BAD - fragile
cy.get('li').eq(2).click();

// GOOD - find by content
cy.contains('li', 'Target Item').click();

// 5. Disable CSS animations in tests
// cypress/support/e2e.js
beforeEach(() => {
  cy.document().then((doc) => {
    const style = doc.createElement('style');
    style.innerHTML = '*, *::before, *::after { transition: none !important; animation: none !important; }';
    doc.head.appendChild(style);
  });
});
```

---

## Limitations & Workarounds

### Q19: What are the limitations of Cypress?

**Answer:**

| Limitation | Details | Workaround |
|------------|---------|------------|
| **Single browser tab** | Cannot natively open or switch between tabs | Use `cy.request()` for the second tab's content, or `window.open` stub |
| **Same-origin policy** | Cannot visit two different superdomains in one test | Use `cy.origin()` (Cypress 12+) or split into separate tests |
| **iFrame support** | No native iframe support | Use `cypress-iframe` plugin or custom commands |
| **File downloads** | No built-in file download verification | Use `cypress-downloadfile` plugin or `cy.readFile()` |
| **Hover** | No native `.hover()` command | Use `.trigger('mouseover')` or `cypress-real-events` plugin |
| **JavaScript only** | Tests must be written in JS/TS | N/A - use Selenium for other languages |
| **Mobile browsers** | Cannot test on real mobile devices | Use viewport resizing or dedicated mobile tools |
| **Shadow DOM** | Requires specific configuration | Use `includeShadowDom: true` option |

```javascript
// Workaround: Multi-tab testing
it('handles new tab link', () => {
  // Remove target="_blank" to prevent new tab
  cy.get('a[target="_blank"]')
    .invoke('removeAttr', 'target')
    .click();
  cy.url().should('include', '/new-page');
});

// Workaround: iFrame interaction
Cypress.Commands.add('getIframeBody', (iframeSelector) => {
  return cy.get(iframeSelector)
    .its('0.contentDocument.body')
    .should('not.be.empty')
    .then(cy.wrap);
});

cy.getIframeBody('#my-iframe').find('.button').click();

// Workaround: Hover
cy.get('.menu-item').trigger('mouseover');
cy.get('.dropdown-menu').should('be.visible');

// Workaround: Shadow DOM
cy.get('my-component')
  .shadow()
  .find('.inner-element')
  .should('be.visible');

// Or globally in config:
// cypress.config.js: { includeShadowDom: true }
```

---

## Advanced Topics

### Q20: How do you set up Cypress for CI/CD pipelines?

**Answer:**

```yaml
# GitHub Actions example
# .github/workflows/cypress.yml
name: Cypress Tests

on: [push, pull_request]

jobs:
  cypress-run:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        browser: [chrome, firefox, edge]
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 20

      - name: Install dependencies
        run: npm ci

      - name: Run Cypress tests
        uses: cypress-io/github-action@v6
        with:
          browser: ${{ matrix.browser }}
          start: npm start
          wait-on: 'http://localhost:3000'
          wait-on-timeout: 120

      - name: Upload screenshots on failure
        uses: actions/upload-artifact@v4
        if: failure()
        with:
          name: cypress-screenshots-${{ matrix.browser }}
          path: cypress/screenshots

      - name: Upload videos
        uses: actions/upload-artifact@v4
        if: always()
        with:
          name: cypress-videos-${{ matrix.browser }}
          path: cypress/videos
```

```javascript
// cypress.config.js - CI-optimized configuration
const { defineConfig } = require('cypress');

module.exports = defineConfig({
  e2e: {
    baseUrl: 'http://localhost:3000',
    video: true,
    screenshotOnRunFailure: true,
    retries: {
      runMode: 2,
      openMode: 0
    },
    viewportWidth: 1280,
    viewportHeight: 720
  }
});
```

---

### Q21: How do you implement data-driven testing in Cypress?

**Answer:**

```javascript
// Method 1: Array-based parameterized tests
const testCases = [
  { username: 'user1', password: 'pass1', expected: 'Welcome, user1' },
  { username: 'user2', password: 'pass2', expected: 'Welcome, user2' },
  { username: 'admin', password: 'admin', expected: 'Welcome, admin' },
];

describe('Data-driven login tests', () => {
  testCases.forEach(({ username, password, expected }) => {
    it(`logs in successfully as ${username}`, () => {
      cy.visit('/login');
      cy.get('#username').type(username);
      cy.get('#password').type(password);
      cy.get('button[type="submit"]').click();
      cy.get('.welcome').should('contain', expected);
    });
  });
});

// Method 2: Fixture-driven tests
describe('Product search tests', () => {
  beforeEach(function() {
    cy.fixture('searchTerms').as('searches');
  });

  it('searches products from fixture data', function() {
    this.searches.forEach((search) => {
      cy.visit('/products');
      cy.get('#search').clear().type(search.term);
      cy.get('#search-button').click();
      cy.get('.results-count').should('contain', search.expectedCount);
    });
  });
});

// Method 3: CSV-driven tests (using cy.readFile)
describe('CSV data-driven tests', () => {
  it('processes CSV data', () => {
    cy.readFile('cypress/fixtures/testdata.csv').then((csvData) => {
      const rows = csvData.split('\n').slice(1); // Skip header
      rows.forEach((row) => {
        const [email, name, role] = row.split(',');
        cy.request('POST', '/api/users', { email, name, role })
          .its('status')
          .should('eq', 201);
      });
    });
  });
});
```

---

### Q22: How do you handle environment variables and multiple environments in Cypress?

**Answer:**

```javascript
// cypress.config.js
const { defineConfig } = require('cypress');

module.exports = defineConfig({
  env: {
    apiUrl: 'http://localhost:3000/api',
    username: 'default_user',
    password: 'default_pass'
  },
  e2e: {
    baseUrl: 'http://localhost:3000'
  }
});
```

```json
// cypress.env.json (gitignored - for local secrets)
{
  "username": "local_admin",
  "password": "local_secret_password",
  "apiKey": "abc-123-secret"
}
```

```javascript
// Environment-specific config files
// cypress/config/staging.json
{
  "baseUrl": "https://staging.myapp.com",
  "env": {
    "apiUrl": "https://staging.myapp.com/api"
  }
}
```

```javascript
// Accessing environment variables in tests
it('uses env variables', () => {
  const apiUrl = Cypress.env('apiUrl');
  const username = Cypress.env('username');

  cy.request(`${apiUrl}/health`).its('status').should('eq', 200);
  cy.visit('/login');
  cy.get('#username').type(username);
});

// CLI usage with different environments
// npx cypress run --env apiUrl=https://staging.api.com,username=stagingUser
// npx cypress run --config-file cypress/config/staging.json
// CYPRESS_API_URL=https://prod.api.com npx cypress run
```

---

### Q23: Explain cy.origin() and how to handle cross-origin scenarios.

**Answer:**

`cy.origin()` (Cypress 12+) allows you to interact with pages on different origins within a single test.

```javascript
it('handles OAuth login flow across domains', () => {
  cy.visit('https://myapp.com/login');
  cy.get('#login-with-google').click();

  // Switch to Google's origin
  cy.origin('https://accounts.google.com', () => {
    cy.get('#identifierId').type('testuser@gmail.com');
    cy.get('#identifierNext').click();
    cy.get('input[type="password"]').type('password123');
    cy.get('#passwordNext').click();
  });

  // Back to original origin automatically
  cy.url().should('include', 'myapp.com/dashboard');
  cy.get('.user-name').should('contain', 'Test User');
});

// Passing data to cy.origin()
it('passes data across origins', () => {
  const credentials = { user: 'test', pass: 'secret' };

  cy.origin('https://auth.example.com', { args: credentials }, ({ user, pass }) => {
    cy.get('#username').type(user);
    cy.get('#password').type(pass);
    cy.get('#submit').click();
  });
});
```

---

## Quick Reference Card

| Topic | Key Point |
|-------|-----------|
| **Architecture** | Cypress runs inside the browser alongside the app |
| **Auto-waiting** | Automatically waits for elements and assertions |
| **Retry-ability** | Query commands and assertions retry; action commands do not |
| **Custom commands** | Extend `cy.*` with `Cypress.Commands.add()` |
| **cy.intercept()** | Spy, stub, or modify network requests/responses |
| **Fixtures** | External test data in `cypress/fixtures/` directory |
| **Debugging** | Time-travel, `cy.debug()`, `cy.pause()`, screenshots |
| **Best Practice** | Use `data-cy` attributes, API login, independent tests |
| **Limitations** | Single tab, same-origin, no native mobile, JS only |
| **CI/CD** | `cypress run` with `--browser`, `--record`, `--parallel` |

---

*This document covers the most frequently asked Cypress interview questions. Practice writing tests and understanding the underlying architecture for a thorough preparation.*
