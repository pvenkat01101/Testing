# Phase 6.1: Cypress - Getting Started

## Table of Contents
1. [Why Cypress?](#why-cypress)
2. [Cypress vs Selenium](#cypress-vs-selenium)
3. [Cypress Architecture](#cypress-architecture)
4. [Installation & Setup](#installation--setup)
5. [Project Folder Structure](#project-folder-structure)
6. [First Cypress Test](#first-cypress-test)
7. [Running Tests](#running-tests)
8. [Cypress Test Runner](#cypress-test-runner)

---

## 1. Why Cypress?

### What is Cypress?

Cypress is a modern, JavaScript-based end-to-end testing framework built for the web. Unlike Selenium, Cypress runs directly in the browser alongside your application, giving it native access to the DOM, events, network layer, and more.

### Key Advantages:

1. **Runs in the browser** - Not through an external driver like Selenium
2. **Automatic waiting** - No need for explicit/implicit waits
3. **Time travel** - Snapshots taken at each step for debugging
4. **Real-time reloads** - Re-runs automatically when test files change
5. **Network traffic control** - Stub and intercept network requests
6. **Consistent results** - Flake-resistant architecture
7. **Screenshots & videos** - Automatic capture on failure
8. **Developer experience** - Fast, intuitive, excellent error messages

### When to Choose Cypress:

✅ **Choose Cypress when:**
- Building modern JavaScript web apps (React, Angular, Vue)
- Need fast, reliable E2E tests
- Team is comfortable with JavaScript
- Want excellent debugging tools
- Testing single-origin applications
- Need network stubbing/mocking

❌ **Consider alternatives when:**
- Need multi-tab/multi-browser tests
- Need to test native mobile apps
- Multi-origin navigation is required
- Need to support IE/older browsers
- Team primarily knows Java/Python

---

## 2. Cypress vs Selenium

| Feature | Cypress | Selenium |
|---------|---------|----------|
| **Language** | JavaScript/TypeScript only | Java, Python, C#, Ruby, JS |
| **Architecture** | Runs inside browser | Uses WebDriver protocol |
| **Speed** | Very fast | Slower (network hops) |
| **Setup** | `npm install cypress` | WebDriver + browser drivers |
| **Waiting** | Automatic | Manual (implicit/explicit) |
| **Debugging** | Time travel, DOM snapshots | Screenshots, logs |
| **Browsers** | Chrome, Firefox, Edge, Electron | All major browsers + IE |
| **Cross-origin** | Limited (improved in v12+) | Full support |
| **Multiple tabs** | Not supported natively | Supported |
| **Network control** | Built-in (cy.intercept) | Not built-in |
| **Mobile testing** | Web only | With Appium |
| **Parallel execution** | Via Cypress Cloud (paid) | Selenium Grid (free) |
| **Community** | Growing rapidly | Very mature |

### Architecture Comparison:

**Selenium Architecture:**
```
Test Script → WebDriver → Browser Driver → Browser
     (network hop)         (network hop)
```

**Cypress Architecture:**
```
Test Script ← runs inside → Browser
     (same execution loop - no network hops)
```

Cypress runs in the same run loop as your application. This means it has native access to every object in your application — the window, document, DOM elements, cookies, local storage, and more.

---

## 3. Cypress Architecture

### How Cypress Works:

```
┌─────────────────────────────────────────────┐
│                  Browser                     │
│                                              │
│  ┌──────────────┐  ┌─────────────────────┐  │
│  │  Cypress      │  │   Application        │  │
│  │  Test Runner  │  │   Under Test (AUT)   │  │
│  │              │  │                      │  │
│  │  - Commands  │  │   - DOM              │  │
│  │  - Assertions│←→│   - Network          │  │
│  │  - Spies     │  │   - Events           │  │
│  │  - Stubs     │  │   - Storage          │  │
│  └──────────────┘  └─────────────────────┘  │
│                                              │
└──────────────┬──────────────────────────────┘
               │
┌──────────────▼──────────────────────────────┐
│            Node.js Server                    │
│  - File system access                        │
│  - Network proxy                             │
│  - Plugin system                             │
│  - Task execution                            │
└─────────────────────────────────────────────┘
```

### Key Architectural Features:

1. **In-Browser Execution:** Cypress runs inside the browser, not externally
2. **Network Proxy:** All HTTP traffic goes through Cypress's proxy
3. **Node.js Server:** Handles tasks outside the browser (file I/O, DB access)
4. **Command Queue:** Commands are queued and executed asynchronously
5. **Automatic Serialization:** Commands execute one after another

---

## 4. Installation & Setup

### Prerequisites:
- **Node.js** (version 18.x or above)
- **npm** (comes with Node.js)
- A modern browser (Chrome, Firefox, or Edge)

### Step 1: Verify Node.js Installation

```bash
node --version
# Should output: v18.x.x or higher

npm --version
# Should output: 8.x.x or higher
```

### Step 2: Create Project

```bash
# Create project directory
mkdir cypress-testing
cd cypress-testing

# Initialize npm project
npm init -y
```

### Step 3: Install Cypress

```bash
# Install Cypress as dev dependency
npm install cypress --save-dev

# OR install globally
npm install -g cypress
```

### Step 4: Open Cypress

```bash
# Using npx (recommended)
npx cypress open

# Using npm scripts (add to package.json)
# "scripts": { "cy:open": "cypress open" }
npm run cy:open
```

### Step 5: Choose Testing Type

When Cypress opens for the first time:
1. Select **E2E Testing**
2. Cypress creates configuration files automatically
3. Select a browser (Chrome recommended)
4. Start writing tests

---

## 5. Project Folder Structure

After initialization, Cypress creates the following structure:

```
cypress-testing/
├── cypress/
│   ├── e2e/                  # Test files go here
│   │   ├── login.cy.js       # Test file (naming: *.cy.js)
│   │   ├── homepage.cy.js
│   │   └── checkout.cy.js
│   ├── fixtures/             # Test data (JSON files)
│   │   ├── users.json
│   │   └── products.json
│   ├── support/              # Reusable commands and config
│   │   ├── commands.js       # Custom commands
│   │   └── e2e.js           # Runs before each test file
│   └── downloads/            # Downloaded files during tests
├── cypress.config.js         # Cypress configuration
├── node_modules/             # npm packages
├── package.json              # Project dependencies
└── package-lock.json
```

### Key Directories:

**`cypress/e2e/`** - Where test files live
- Files must end with `.cy.js` or `.cy.ts`
- Organize by feature/module
- Subdirectories supported

**`cypress/fixtures/`** - External test data
- JSON files for test data
- Loaded with `cy.fixture()`
- Keeps test data separate from tests

**`cypress/support/`** - Shared code
- `commands.js` - Custom Cypress commands
- `e2e.js` - Global configuration, imports

**`cypress.config.js`** - Main configuration file

### Configuration File (cypress.config.js):

```javascript
const { defineConfig } = require('cypress')

module.exports = defineConfig({
  e2e: {
    // Base URL for your application
    baseUrl: 'https://www.saucedemo.com',

    // Spec file pattern
    specPattern: 'cypress/e2e/**/*.cy.{js,jsx,ts,tsx}',

    // Support file
    supportFile: 'cypress/support/e2e.js',

    // Viewport dimensions
    viewportWidth: 1280,
    viewportHeight: 720,

    // Timeouts
    defaultCommandTimeout: 10000,    // 10 seconds for commands
    pageLoadTimeout: 60000,          // 60 seconds for page loads
    requestTimeout: 15000,           // 15 seconds for network requests

    // Screenshots and Videos
    screenshotOnRunFailure: true,
    video: true,
    videosFolder: 'cypress/videos',
    screenshotsFolder: 'cypress/screenshots',

    // Retries
    retries: {
      runMode: 2,      // Retries during cypress run
      openMode: 0       // No retries during cypress open
    },

    // Browser
    chromeWebSecurity: false,  // Disable cross-origin restrictions

    // Setup node events
    setupNodeEvents(on, config) {
      // Register plugins here
    },
  },
})
```

---

## 6. First Cypress Test

### Test File Structure:

```javascript
// cypress/e2e/my-first-test.cy.js

// describe() - Test suite
describe('My First Test Suite', () => {

  // before() - Run once before all tests
  before(() => {
    cy.log('Running before all tests')
  })

  // beforeEach() - Run before each test
  beforeEach(() => {
    cy.visit('https://www.saucedemo.com')
  })

  // it() - Individual test case
  it('should visit the homepage', () => {
    // Assertions
    cy.url().should('include', 'saucedemo.com')
    cy.title().should('include', 'Swag Labs')
  })

  it('should display login form', () => {
    cy.get('#user-name').should('be.visible')
    cy.get('#password').should('be.visible')
    cy.get('#login-button').should('be.visible')
  })

  it('should login successfully with valid credentials', () => {
    // Type username
    cy.get('#user-name').type('standard_user')

    // Type password
    cy.get('#password').type('secret_sauce')

    // Click login button
    cy.get('#login-button').click()

    // Verify successful login
    cy.url().should('include', '/inventory.html')
    cy.get('.title').should('have.text', 'Products')
  })

  it('should show error for invalid credentials', () => {
    cy.get('#user-name').type('invalid_user')
    cy.get('#password').type('wrong_password')
    cy.get('#login-button').click()

    // Verify error message
    cy.get('[data-test="error"]')
      .should('be.visible')
      .and('contain', 'Username and password do not match')
  })

  // afterEach() - Run after each test
  afterEach(() => {
    cy.log('Test completed')
  })

  // after() - Run once after all tests
  after(() => {
    cy.log('All tests completed')
  })
})
```

### Understanding the Test:

1. **`describe()`** - Groups related tests (test suite)
2. **`it()`** - Defines an individual test case
3. **`cy.visit()`** - Navigates to a URL
4. **`cy.get()`** - Selects an element (like findElement)
5. **`.type()`** - Types into an element
6. **`.click()`** - Clicks an element
7. **`.should()`** - Asserts something about the element
8. **`cy.url()`** - Gets the current URL
9. **`cy.title()`** - Gets the page title

### Key Differences from Selenium:

```javascript
// Selenium (Java)
driver.get("https://www.saucedemo.com");
driver.findElement(By.id("user-name")).sendKeys("standard_user");
driver.findElement(By.id("password")).sendKeys("secret_sauce");
driver.findElement(By.id("login-button")).click();

// Cypress (JavaScript)
cy.visit('https://www.saucedemo.com')
cy.get('#user-name').type('standard_user')
cy.get('#password').type('secret_sauce')
cy.get('#login-button').click()
```

Notice: **No waits needed in Cypress!** It automatically waits for elements to exist and be actionable.

---

## 7. Running Tests

### Interactive Mode (Test Runner):

```bash
# Opens Cypress Test Runner GUI
npx cypress open
```

**Features:**
- Visual test execution
- Real-time browser view
- Time travel debugging
- DOM snapshots
- Command log
- Browser selector

### Headless Mode (CLI):

```bash
# Run all tests headlessly
npx cypress run

# Run specific test file
npx cypress run --spec "cypress/e2e/login.cy.js"

# Run with specific browser
npx cypress run --browser chrome
npx cypress run --browser firefox
npx cypress run --browser edge

# Run headless with video
npx cypress run --browser chrome --headed

# Run with environment variables
npx cypress run --env username=admin,password=secret
```

### npm Scripts (package.json):

```json
{
  "scripts": {
    "cy:open": "cypress open",
    "cy:run": "cypress run",
    "cy:run:chrome": "cypress run --browser chrome",
    "cy:run:firefox": "cypress run --browser firefox",
    "cy:run:headed": "cypress run --headed",
    "cy:run:login": "cypress run --spec 'cypress/e2e/login.cy.js'",
    "cy:run:smoke": "cypress run --spec 'cypress/e2e/smoke/**/*.cy.js'"
  }
}
```

**Usage:**
```bash
npm run cy:open
npm run cy:run
npm run cy:run:chrome
```

---

## 8. Cypress Test Runner

### Test Runner Features:

1. **Command Log (Left Panel):**
   - Shows each Cypress command executed
   - Click any command to see DOM snapshot at that moment
   - Shows assertion results (pass/fail)

2. **App Preview (Right Panel):**
   - Shows your application running
   - Updates in real-time during test execution
   - Highlights elements being interacted with

3. **Time Travel:**
   - Hover over commands in the Command Log
   - See exactly how the app looked at each step
   - DOM snapshots at each command

4. **Selector Playground:**
   - Built-in tool to find element selectors
   - Click any element to get the best selector
   - Generates unique, reliable selectors

5. **Browser Selector:**
   - Choose which browser to run tests in
   - Chrome, Firefox, Edge, Electron supported

### Test Runner Tips:

- **Pin Snapshots:** Click a command to pin its DOM snapshot
- **Console Output:** View detailed info in browser DevTools console
- **Network Tab:** See all network requests during test
- **Selector Playground:** Use the crosshair icon to find selectors

---

## Summary

You've learned:
1. ✅ Why Cypress exists and its advantages
2. ✅ How Cypress differs from Selenium
3. ✅ Cypress architecture (runs inside browser)
4. ✅ Installation and project setup
5. ✅ Folder structure and configuration
6. ✅ Writing your first Cypress test
7. ✅ Running tests (interactive vs headless)
8. ✅ Using the Test Runner for debugging

**Key Takeaway:** Cypress runs inside the browser alongside your app, making it faster, more reliable, and easier to debug than traditional tools.

**Next:** Cypress Core Concepts (Auto-waiting, Retry-ability, Command Queue)
