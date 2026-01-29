# Phase 3-04: JavaScript Programming Foundation - Theory

## Course Overview
This comprehensive guide covers JavaScript programming fundamentals specifically designed for **automation testers** preparing for **10-year experience positions**. The content focuses on practical testing scenarios, Cypress/Playwright integration, and industry best practices for modern web test automation.

---

## 1. Introduction to JavaScript for Testers

### Why JavaScript for Test Automation?

JavaScript has become increasingly popular for test automation, especially for web applications. Here's why:

**Advantages for Testers:**
- **Native Browser Language**: Direct interaction with web applications
- **Asynchronous by Nature**: Perfect for handling modern web apps
- **Rich Testing Ecosystem**: Cypress, Playwright, WebdriverIO, Jest, Mocha
- **Full-Stack Testing**: Frontend, backend (Node.js), API testing
- **Fast Execution**: No compilation needed, quick feedback
- **JSON Native Support**: Perfect for API testing
- **Growing Enterprise Adoption**: Modern companies prefer JavaScript frameworks
- **Same Language as Developers**: Better collaboration with dev teams

### JavaScript vs Other Languages for Automation

| Feature | JavaScript | Java | Python | C# |
|---------|------------|------|--------|-----|
| Learning Curve | Easy | Moderate | Easy | Moderate |
| Async Handling | Native (Promises) | Complex (CompletableFuture) | Good (async/await) | Good (async/await) |
| Web Testing | Excellent (Cypress, Playwright) | Good (Selenium) | Excellent (Selenium, Playwright) | Good (Selenium) |
| Performance | Very Fast | High | Moderate | High |
| Type Safety | Weak (Dynamic) / Strong (TypeScript) | Strong (Static) | Weak (Dynamic) | Strong (Static) |
| API Testing | Excellent (Axios, Fetch) | Excellent (RestAssured) | Excellent (Requests) | Good (RestSharp) |
| Mobile Testing | Good (Appium, Detox) | Excellent (Appium) | Good (Appium) | Good (Appium) |
| CI/CD Integration | Excellent | Excellent | Excellent | Excellent |
| Community Support | Very Large | Very Large | Very Large | Large |
| Modern Frameworks | Cypress, Playwright, WebdriverIO | Selenium 4 | Selenium, Playwright | Selenium 4 |

### Modern Testing Tools Using JavaScript

**1. Cypress (Most Popular)**
```javascript
// Cypress test example - Modern, fast, reliable
describe('Login Functionality', () => {
    beforeEach(() => {
        cy.visit('https://example.com/login');
    });

    it('should login with valid credentials', () => {
        cy.get('#username').type('testuser');
        cy.get('#password').type('password123');
        cy.get('#loginBtn').click();

        cy.get('#welcome').should('contain.text', 'Welcome, testuser!');
        cy.url().should('include', '/dashboard');
    });

    it('should show error with invalid credentials', () => {
        cy.get('#username').type('invalid');
        cy.get('#password').type('wrong');
        cy.get('#loginBtn').click();

        cy.get('.error-message')
            .should('be.visible')
            .and('contain', 'Invalid credentials');
    });
});
```

**2. Playwright (Cross-Browser)**
```javascript
// Playwright test - Supports all browsers
const { test, expect } = require('@playwright/test');

test.describe('Login Tests', () => {
    test('should login successfully', async ({ page }) => {
        await page.goto('https://example.com/login');

        await page.fill('#username', 'testuser');
        await page.fill('#password', 'password123');
        await page.click('#loginBtn');

        const welcomeMsg = await page.textContent('#welcome');
        expect(welcomeMsg).toContain('Welcome, testuser!');
        expect(page.url()).toContain('/dashboard');
    });
});
```

**3. WebdriverIO (Selenium-based)**
```javascript
// WebdriverIO test
describe('Login Page', () => {
    it('should allow valid login', async () => {
        await browser.url('https://example.com/login');

        await $('#username').setValue('testuser');
        await $('#password').setValue('password123');
        await $('#loginBtn').click();

        const welcome = await $('#welcome').getText();
        expect(welcome).toContain('Welcome, testuser!');
    });
});
```

**4. API Testing with Jest**
```javascript
// API testing with Jest and Axios
const axios = require('axios');

describe('User API Tests', () => {
    test('GET /users should return user list', async () => {
        const response = await axios.get('https://api.example.com/users');

        expect(response.status).toBe(200);
        expect(Array.isArray(response.data)).toBe(true);
        expect(response.data.length).toBeGreaterThan(0);
    });

    test('POST /users should create new user', async () => {
        const newUser = {
            name: 'Test User',
            email: 'test@example.com'
        };

        const response = await axios.post('https://api.example.com/users', newUser);

        expect(response.status).toBe(201);
        expect(response.data).toHaveProperty('id');
        expect(response.data.name).toBe(newUser.name);
    });
});
```

### Why JavaScript is Perfect for Modern Testing

**1. Asynchronous Nature**
Modern web apps are asynchronous. JavaScript handles this naturally:

```javascript
// JavaScript handles async operations elegantly
async function testUserFlow() {
    await page.goto(url);                    // Wait for navigation
    await page.fill('#email', 'test@test.com'); // Wait for element
    const response = await fetch(apiUrl);     // Wait for API call
    const data = await response.json();       // Wait for JSON parsing
}

// Compare with Java (more verbose)
// WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
// wait.until(ExpectedConditions.visibilityOfElementLocated(By.id("email")));
```

**2. JSON Native Support**
```javascript
// JavaScript - JSON is native
const user = { name: "John", age: 30 };
const jsonString = JSON.stringify(user);    // Easy conversion
const parsed = JSON.parse(jsonString);      // Easy parsing

// Perfect for API testing
const response = await fetch(url);
const data = await response.json();         // One line!
```

**3. Modern Syntax**
```javascript
// Arrow functions for cleaner code
const users = ['John', 'Jane', 'Bob'];
const upperNames = users.map(name => name.toUpperCase());

// Destructuring for better readability
const { email, password } = testData.credentials;

// Template literals for dynamic strings
cy.get(`[data-test="${testId}"]`);
```

### Testing Scenario Comparison

**Scenario: Test Dynamic E-commerce Product Search**

**JavaScript (Cypress):**
```javascript
describe('Product Search', () => {
    it('should filter products dynamically', () => {
        cy.visit('/products');

        // Type and wait for dynamic results
        cy.get('#search').type('laptop');

        // Automatically waits for DOM updates
        cy.get('.product-card').should('have.length.greaterThan', 0);

        // Chain assertions naturally
        cy.get('.product-card').first()
            .should('be.visible')
            .and('contain', 'laptop');

        // Intercept API calls
        cy.intercept('GET', '/api/products*').as('searchAPI');
        cy.get('#search').clear().type('phone');
        cy.wait('@searchAPI').its('response.statusCode').should('eq', 200);
    });
});
```

**Java (Selenium):**
```java
@Test
public void testProductSearch() {
    driver.get("https://example.com/products");

    // More verbose waits
    WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
    WebElement searchBox = wait.until(
        ExpectedConditions.visibilityOfElementLocated(By.id("search"))
    );

    searchBox.sendKeys("laptop");

    // Manual wait for results
    wait.until(ExpectedConditions.numberOfElementsToBeMoreThan(
        By.className("product-card"), 0
    ));

    List<WebElement> products = driver.findElements(By.className("product-card"));
    Assert.assertTrue(products.size() > 0);
    Assert.assertTrue(products.get(0).getText().contains("laptop"));
}
```

### Common Mistakes to Avoid

1. **Not Understanding Asynchronous JavaScript**
```javascript
// WRONG - Not waiting for async operations
function badTest() {
    fetch(url);  // Fire and forget
    console.log(data);  // data is undefined!
}

// CORRECT - Using async/await
async function goodTest() {
    const response = await fetch(url);
    const data = await response.json();
    console.log(data);  // data is available
}
```

2. **Ignoring Promises**
```javascript
// WRONG
function badApiTest() {
    axios.get(url);  // Promise not handled
}

// CORRECT
async function goodApiTest() {
    const response = await axios.get(url);
    return response.data;
}
```

3. **Using var Instead of let/const**
```javascript
// WRONG - var has function scope issues
for (var i = 0; i < 5; i++) {
    setTimeout(() => console.log(i), 100);  // Prints 5, 5, 5, 5, 5
}

// CORRECT - let has block scope
for (let i = 0; i < 5; i++) {
    setTimeout(() => console.log(i), 100);  // Prints 0, 1, 2, 3, 4
}
```

4. **Not Using Modern Syntax**
```javascript
// WRONG - Old callback style
cy.get('#button').then(function($btn) {
    var text = $btn.text();
    expect(text).to.equal('Click');
});

// CORRECT - Modern arrow functions
cy.get('#button').then($btn => {
    const text = $btn.text();
    expect(text).to.equal('Click');
});
```

5. **Comparing with == Instead of ===**
```javascript
// WRONG - Type coercion issues
if (age == "30") { }  // true even if age is number 30

// CORRECT - Strict equality
if (age === 30) { }   // Only true if age is number 30
```

### Interview Tips for JavaScript Testing

**Q1: Why choose JavaScript over Java for automation?**

**Strong Answer:**
"While Java is excellent for enterprise automation, JavaScript offers several advantages for modern web testing:

1. **Native async handling**: Modern web apps are heavily asynchronous. JavaScript's async/await makes handling promises natural, while Java requires more boilerplate.

2. **Faster feedback**: No compilation needed. Write code, run tests immediately.

3. **Developer collaboration**: Using the same language as frontend developers improves collaboration and code review.

4. **Modern tools**: Cypress and Playwright offer better developer experience with automatic waiting, time-travel debugging, and real-time reloading.

5. **API testing**: Native JSON support makes API testing cleaner. One line (`await response.json()`) vs multiple lines in Java.

However, for large enterprise frameworks with strict type safety needs, TypeScript (JavaScript with types) or Java might be better choices."

**Q2: What is the difference between Cypress and Selenium?**

**Strong Answer:**
"Key differences:

1. **Architecture**: Cypress runs in the browser alongside your app, while Selenium executes commands remotely. This makes Cypress faster and more reliable.

2. **Waiting mechanism**: Cypress automatically waits for elements, no need for explicit waits. Selenium requires WebDriverWait.

3. **Browser support**: Selenium supports all browsers including IE. Cypress supports Chrome, Edge, Firefox, Electron.

4. **Debugging**: Cypress offers time-travel, automatic screenshots/videos. Selenium requires manual setup.

5. **Language**: Cypress is JavaScript-only. Selenium supports Java, Python, C#, JavaScript.

6. **Network stubbing**: Cypress has built-in network mocking. Selenium needs external tools.

Choose Cypress for modern web apps with JavaScript teams. Choose Selenium for multi-language support or legacy browser testing."

**Q3: Explain Promises and async/await in testing context**

**Strong Answer:**
"Promises handle asynchronous operations, critical in testing:

```javascript
// Promise - represents future value
function loginTest() {
    return cy.get('#username')  // Returns a promise
        .type('user')
        .get('#password')
        .type('pass')
        .get('#login')
        .click();
}

// async/await - cleaner syntax for promises
async function apiTest() {
    const response = await fetch('/api/users');
    const users = await response.json();
    expect(users).toHaveLength(5);
}
```

In Cypress, commands are automatically enqueued and executed in order. In Playwright/Jest, we use async/await to ensure operations complete before assertions."

**Q4: What are the advantages of TypeScript for test automation?**

**Strong Answer:**
"TypeScript adds static typing to JavaScript, providing:

1. **Type Safety**: Catch errors at compile time
```typescript
function login(username: string, password: string): void {
    // username and password must be strings
}
login(123, 456);  // Error caught before runtime
```

2. **Better IntelliSense**: IDEs provide better autocomplete
3. **Refactoring**: Easier to refactor with confidence
4. **Documentation**: Types serve as inline documentation
5. **Enterprise preference**: Large teams prefer type safety

Many modern frameworks (Playwright, Cypress 10+) support TypeScript out of the box."

**Q5: How do you handle flaky tests in JavaScript?**

**Strong Answer:**
"Strategies for handling flaky tests:

1. **Use framework retries**:
```javascript
// Cypress
it('test', { retries: 2 }, () => { });

// Playwright
test('test', async ({ page }) => { });
// Configure in playwright.config.js: retries: 2
```

2. **Proper waiting**:
```javascript
// WRONG - Fixed delays
await page.waitForTimeout(5000);

// CORRECT - Wait for specific conditions
await page.waitForSelector('.result', { state: 'visible' });
```

3. **Isolate tests**: Each test should be independent
4. **Network stability**: Mock external APIs
5. **Test data**: Reset state before each test

Root cause flakiness rather than just adding retries."

### Best Practices for JavaScript Test Automation

1. **Use const by default, let when needed, never var**
2. **Prefer async/await over callbacks**
3. **Write atomic, independent tests**
4. **Use descriptive test names**
5. **Implement Page Object Model**
6. **Handle asynchronous operations properly**
7. **Use TypeScript for large projects**
8. **Implement proper error handling**
9. **Keep tests DRY (Don't Repeat Yourself)**
10. **Use appropriate assertions**

### Real-World Testing Framework Structure

```
cypress-automation/
├── cypress/
│   ├── e2e/
│   │   ├── login/
│   │   │   ├── login.spec.js
│   │   │   └── loginValidation.spec.js
│   │   ├── products/
│   │   │   └── productSearch.spec.js
│   │   └── checkout/
│   │       └── checkout.spec.js
│   ├── fixtures/
│   │   ├── users.json
│   │   └── products.json
│   ├── support/
│   │   ├── commands.js
│   │   ├── pageObjects/
│   │   │   ├── LoginPage.js
│   │   │   └── ProductPage.js
│   │   └── e2e.js
│   └── plugins/
│       └── index.js
├── cypress.config.js
├── package.json
└── README.md
```

---

## 2. Setting Up JavaScript Development Environment

### Components Needed for JavaScript Test Automation

#### A. Node.js and npm

**What is Node.js?**
- Runtime environment to execute JavaScript outside the browser
- Built on Chrome's V8 JavaScript engine
- Includes npm (Node Package Manager) for managing dependencies
- Essential for running JavaScript test frameworks

**What is npm?**
- Package manager for JavaScript
- Manages project dependencies
- Similar to Maven/Gradle in Java
- Contains over 2 million packages

**Node.js Version Selection:**

| Version | Type | Release | Usage in Testing |
|---------|------|---------|------------------|
| 16.x | LTS | 2021 | Stable, widely used |
| 18.x | LTS | 2022 | Current LTS, recommended |
| 20.x | LTS | 2023 | Latest LTS, modern features |
| 21.x | Current | 2023 | Latest features, less stable |

**Recommendation**: Use latest LTS (Long Term Support) version for testing projects.

**Installation Steps:**

**Windows:**
```bash
# Download from nodejs.org and install

# Verify installation
node --version
# Output: v18.17.0

npm --version
# Output: 9.8.1

# Alternative: Using nvm (Node Version Manager)
# Download nvm-windows from GitHub
nvm install 18.17.0
nvm use 18.17.0
```

**macOS:**
```bash
# Using Homebrew (recommended)
brew install node

# Verify
node --version
npm --version

# Using nvm (best for managing multiple versions)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
nvm install 18
nvm use 18
```

**Linux:**
```bash
# Using package manager (Ubuntu/Debian)
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs

# Verify
node --version
npm --version

# Using nvm (recommended)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
source ~/.bashrc
nvm install 18
nvm use 18
```

**Understanding npm vs npx:**

```bash
# npm - Package manager, installs packages
npm install cypress

# npx - Package executor, runs packages without installing
npx create-react-app my-app

# npm installs globally (available system-wide)
npm install -g cypress

# npm installs locally (project-specific)
npm install cypress --save-dev
```

#### B. Code Editor - Visual Studio Code (Recommended)

**Why VS Code for JavaScript Testing?**
- Excellent JavaScript/TypeScript support
- Integrated terminal
- Git integration built-in
- Vast extension marketplace
- IntelliSense (auto-completion)
- Debugging support
- Free and lightweight

**Installation:**
1. Download from https://code.visualstudio.com/
2. Install for your operating system
3. Launch VS Code

**Essential VS Code Extensions for Testers:**

```javascript
// Essential Extensions (Install from Extensions panel)
1. ESLint - JavaScript linting
2. Prettier - Code formatter
3. Cypress Snippets - Cypress code snippets
4. JavaScript (ES6) code snippets
5. Path Intellisense - Auto-complete file paths
6. GitLens - Enhanced Git capabilities
7. Live Server - Local development server
8. Thunder Client - API testing (like Postman)
9. Error Lens - Inline error display
10. Material Icon Theme - Better file icons
```

**VS Code Configuration for Testing:**

```json
// .vscode/settings.json
{
    "editor.formatOnSave": true,
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "editor.codeActionsOnSave": {
        "source.fixAll.eslint": true
    },
    "javascript.suggest.autoImports": true,
    "javascript.updateImportsOnFileMove.enabled": "always",
    "files.autoSave": "onFocusChange",
    "editor.tabSize": 2,
    "editor.fontSize": 14,
    "terminal.integrated.fontSize": 13,
    "workbench.colorTheme": "Default Dark+",
    "editor.minimap.enabled": true,
    "editor.wordWrap": "on"
}
```

**VS Code Keyboard Shortcuts for Productivity:**

| Shortcut (Windows/Linux) | Shortcut (Mac) | Action |
|--------------------------|----------------|---------|
| Ctrl + ` | Ctrl + ` | Toggle terminal |
| Ctrl + P | Cmd + P | Quick file open |
| Ctrl + Shift + P | Cmd + Shift + P | Command palette |
| Ctrl + / | Cmd + / | Toggle comment |
| Ctrl + D | Cmd + D | Select next occurrence |
| Alt + Click | Option + Click | Multi-cursor |
| Ctrl + Shift + F | Cmd + Shift + F | Find in files |
| F5 | F5 | Start debugging |
| Ctrl + Shift + ` | Ctrl + Shift + ` | New terminal |

#### C. Creating Your First JavaScript Test Project

**Step 1: Initialize a New Project**

```bash
# Create project directory
mkdir cypress-automation
cd cypress-automation

# Initialize npm project
npm init -y

# Output: Created package.json
```

**Generated package.json:**
```json
{
  "name": "cypress-automation",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

**Step 2: Install Cypress**

```bash
# Install Cypress as dev dependency
npm install cypress --save-dev

# This creates:
# - node_modules/ folder (dependencies)
# - package-lock.json (dependency tree)
# - Updates package.json with Cypress
```

**Updated package.json:**
```json
{
  "name": "cypress-automation",
  "version": "1.0.0",
  "description": "Cypress Test Automation Framework",
  "main": "index.js",
  "scripts": {
    "test": "cypress run",
    "test:headed": "cypress run --headed",
    "test:chrome": "cypress run --browser chrome",
    "test:firefox": "cypress run --browser firefox",
    "cy:open": "cypress open"
  },
  "keywords": ["testing", "automation", "cypress"],
  "author": "Your Name",
  "license": "ISC",
  "devDependencies": {
    "cypress": "^13.6.0"
  }
}
```

**Step 3: Open Cypress**

```bash
# Open Cypress Test Runner
npx cypress open

# This creates Cypress folder structure:
# cypress/
# ├── e2e/
# ├── fixtures/
# ├── support/
# cypress.config.js
```

**Step 4: Project Structure**

```
cypress-automation/
├── node_modules/          # Dependencies (auto-generated)
├── cypress/
│   ├── e2e/              # Test files
│   │   └── spec.cy.js
│   ├── fixtures/         # Test data (JSON files)
│   │   └── example.json
│   ├── support/          # Custom commands, utilities
│   │   ├── commands.js
│   │   └── e2e.js
│   └── screenshots/      # Auto-generated on failure
│   └── videos/           # Auto-generated test recordings
├── cypress.config.js     # Cypress configuration
├── package.json          # Project metadata & dependencies
├── package-lock.json     # Dependency lock file
└── .gitignore           # Git ignore file
```

**Step 5: Configure Cypress**

```javascript
// cypress.config.js
const { defineConfig } = require('cypress');

module.exports = defineConfig({
  e2e: {
    baseUrl: 'https://example.com',
    viewportWidth: 1280,
    viewportHeight: 720,
    video: true,
    screenshotOnRunFailure: true,
    defaultCommandTimeout: 10000,
    pageLoadTimeout: 60000,

    setupNodeEvents(on, config) {
      // implement node event listeners here
    },

    // Test file patterns
    specPattern: 'cypress/e2e/**/*.cy.{js,jsx,ts,tsx}',

    // Support file
    supportFile: 'cypress/support/e2e.js',

    // Environment variables
    env: {
      apiUrl: 'https://api.example.com',
      username: 'testuser',
      password: 'password123'
    }
  },
});
```

**Step 6: Create Your First Test**

```javascript
// cypress/e2e/login.cy.js
describe('Login Tests', () => {
    beforeEach(() => {
        // Runs before each test
        cy.visit('/login');
    });

    it('should display login page elements', () => {
        cy.get('#username').should('be.visible');
        cy.get('#password').should('be.visible');
        cy.get('#loginBtn').should('be.visible');
        cy.get('h1').should('contain.text', 'Login');
    });

    it('should login with valid credentials', () => {
        cy.get('#username').type('testuser');
        cy.get('#password').type('password123');
        cy.get('#loginBtn').click();

        cy.url().should('include', '/dashboard');
        cy.get('.welcome-message').should('contain', 'Welcome');
    });

    it('should show error with invalid credentials', () => {
        cy.get('#username').type('invalid');
        cy.get('#password').type('wrong');
        cy.get('#loginBtn').click();

        cy.get('.error-message')
            .should('be.visible')
            .and('contain', 'Invalid credentials');
    });

    it('should validate empty fields', () => {
        cy.get('#loginBtn').click();

        cy.get('#username:invalid').should('exist');
        cy.get('#password:invalid').should('exist');
    });
});
```

**Step 7: Run Tests**

```bash
# Run tests in headless mode (CI/CD)
npm test

# Run tests in headed mode (see browser)
npm run test:headed

# Open Cypress Test Runner (interactive)
npm run cy:open

# Run specific test file
npx cypress run --spec "cypress/e2e/login.cy.js"

# Run in specific browser
npm run test:chrome
npm run test:firefox

# Run with specific tags
npx cypress run --env grep=smoke
```

#### D. Alternative Setup - Playwright

**Installing Playwright:**

```bash
# Create new project
mkdir playwright-automation
cd playwright-automation

# Initialize npm
npm init -y

# Install Playwright
npm init playwright@latest

# This will:
# 1. Install Playwright
# 2. Install browsers (Chrome, Firefox, Safari)
# 3. Create example tests
# 4. Create playwright.config.js
```

**Playwright Project Structure:**

```
playwright-automation/
├── tests/
│   └── example.spec.js
├── playwright.config.js
├── package.json
└── package-lock.json
```

**Sample Playwright Test:**

```javascript
// tests/login.spec.js
const { test, expect } = require('@playwright/test');

test.describe('Login Tests', () => {
    test.beforeEach(async ({ page }) => {
        await page.goto('https://example.com/login');
    });

    test('should login successfully', async ({ page }) => {
        await page.fill('#username', 'testuser');
        await page.fill('#password', 'password123');
        await page.click('#loginBtn');

        await expect(page).toHaveURL(/.*dashboard/);
        await expect(page.locator('.welcome-message')).toContainText('Welcome');
    });

    test('should show validation errors', async ({ page }) => {
        await page.click('#loginBtn');

        const usernameError = page.locator('#username-error');
        await expect(usernameError).toBeVisible();
        await expect(usernameError).toContainText('Username is required');
    });
});
```

**Run Playwright Tests:**

```bash
# Run all tests
npx playwright test

# Run in UI mode (interactive)
npx playwright test --ui

# Run in headed mode
npx playwright test --headed

# Run specific browser
npx playwright test --project=chromium
npx playwright test --project=firefox
npx playwright test --project=webkit

# Generate test report
npx playwright show-report
```

#### E. Package Management Deep Dive

**Understanding package.json:**

```json
{
  "name": "test-automation",
  "version": "1.0.0",
  "description": "Automated testing framework",

  // Entry point
  "main": "index.js",

  // NPM scripts (shortcuts for commands)
  "scripts": {
    "test": "cypress run",
    "test:ui": "cypress open",
    "test:chrome": "cypress run --browser chrome",
    "test:headed": "cypress run --headed",
    "lint": "eslint cypress/**/*.js",
    "format": "prettier --write cypress/**/*.js",
    "report": "cypress run --reporter mochawesome"
  },

  // Project metadata
  "keywords": ["testing", "automation", "cypress", "e2e"],
  "author": "Your Name <your.email@example.com>",
  "license": "MIT",

  // Production dependencies (needed at runtime)
  "dependencies": {
    "axios": "^1.5.0",
    "dotenv": "^16.3.1"
  },

  // Development dependencies (only needed for development)
  "devDependencies": {
    "cypress": "^13.6.0",
    "@cypress/xpath": "^2.0.3",
    "eslint": "^8.50.0",
    "prettier": "^3.0.3",
    "mochawesome": "^7.1.3"
  },

  // Supported Node.js versions
  "engines": {
    "node": ">=16.0.0",
    "npm": ">=8.0.0"
  }
}
```

**npm Commands Reference:**

```bash
# Install all dependencies from package.json
npm install

# Install specific package
npm install cypress --save-dev
npm install axios --save

# Install specific version
npm install cypress@13.6.0

# Install globally
npm install -g @playwright/test

# Uninstall package
npm uninstall cypress

# Update packages
npm update

# Check for outdated packages
npm outdated

# List installed packages
npm list
npm list --depth=0  # Only top-level

# Run scripts
npm run test
npm run cy:open

# Clean install (delete node_modules and reinstall)
rm -rf node_modules package-lock.json
npm install

# Check package info
npm info cypress
npm view cypress versions
```

**Understanding Semantic Versioning (SemVer):**

```json
{
  "dependencies": {
    "axios": "1.5.0",      // Exact version
    "cypress": "^13.6.0",  // Compatible with 13.6.0 (^)
    "mocha": "~10.2.0",    // Approximately 10.2.x (~)
    "chai": "*",           // Latest version (avoid in production)
    "lodash": ">=4.17.0"   // Greater than or equal
  }
}
```

**Caret (^) vs Tilde (~):**

```
^1.2.3  →  Allows: 1.2.3, 1.2.4, 1.3.0, 1.9.9  (Not 2.0.0)
           Updates: Minor and patch versions

~1.2.3  →  Allows: 1.2.3, 1.2.4, 1.2.9  (Not 1.3.0)
           Updates: Only patch versions
```

**package-lock.json:**
- Auto-generated file
- Locks exact versions of all dependencies
- Ensures consistent installs across environments
- Should be committed to version control
- Regenerated on `npm install`

#### F. Environment Setup Best Practices

**1. Use .gitignore**

```bash
# .gitignore for JavaScript test projects
node_modules/
package-lock.json  # Some teams don't ignore this
cypress/videos/
cypress/screenshots/
.env
.vscode/
.idea/
*.log
dist/
build/
coverage/
test-results/
playwright-report/
```

**2. Use Environment Variables**

```bash
# .env file (never commit this)
BASE_URL=https://staging.example.com
API_URL=https://api-staging.example.com
USERNAME=testuser
PASSWORD=password123
API_KEY=your-secret-key
```

```javascript
// Load environment variables
require('dotenv').config();

// Access in tests
const baseUrl = process.env.BASE_URL;
const apiUrl = process.env.API_URL;
const username = process.env.USERNAME;

describe('Environment Tests', () => {
    it('should use environment variables', () => {
        cy.visit(baseUrl);
        cy.get('#username').type(username);
    });
});
```

**3. Create .env.example**

```bash
# .env.example (commit this as template)
BASE_URL=https://your-app-url.com
API_URL=https://your-api-url.com
USERNAME=your-username
PASSWORD=your-password
API_KEY=your-api-key
```

**4. Configure ESLint**

```bash
# Install ESLint
npm install eslint --save-dev

# Initialize ESLint
npx eslint --init
```

```json
// .eslintrc.json
{
  "env": {
    "browser": true,
    "es2021": true,
    "node": true,
    "cypress/globals": true
  },
  "extends": [
    "eslint:recommended",
    "plugin:cypress/recommended"
  ],
  "plugins": ["cypress"],
  "parserOptions": {
    "ecmaVersion": 12,
    "sourceType": "module"
  },
  "rules": {
    "no-console": "warn",
    "no-unused-vars": "error",
    "semi": ["error", "always"],
    "quotes": ["error", "single"]
  }
}
```

**5. Configure Prettier**

```json
// .prettierrc.json
{
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "es5",
  "printWidth": 80,
  "arrowParens": "avoid"
}
```

```bash
# Format all files
npm run format

# Check formatting
npx prettier --check cypress/**/*.js
```

**6. Complete Professional Setup**

```bash
# 1. Create project
mkdir my-test-automation
cd my-test-automation

# 2. Initialize npm
npm init -y

# 3. Install dependencies
npm install cypress --save-dev
npm install dotenv --save-dev
npm install eslint prettier --save-dev
npm install @cypress/xpath cypress-mochawesome-reporter --save-dev

# 4. Create folder structure
mkdir -p cypress/e2e/tests
mkdir -p cypress/support/pageObjects
mkdir -p cypress/support/utils
mkdir -p cypress/fixtures

# 5. Initialize ESLint
npx eslint --init

# 6. Create config files
touch .env .env.example .gitignore .prettierrc.json

# 7. Open Cypress
npx cypress open
```

### Common Setup Issues and Solutions

**Issue 1: Cypress not opening**
```bash
# Clear Cypress cache
npx cypress cache clear
npx cypress install

# Check Cypress binary
npx cypress verify
```

**Issue 2: Node version mismatch**
```bash
# Check current version
node --version

# Install correct version using nvm
nvm install 18
nvm use 18
```

**Issue 3: Permission errors (Mac/Linux)**
```bash
# Fix npm permissions
sudo chown -R $(whoami) ~/.npm
sudo chown -R $(whoami) /usr/local/lib/node_modules
```

**Issue 4: Slow npm install**
```bash
# Use npm cache
npm cache clean --force

# Or use yarn (faster alternative)
npm install -g yarn
yarn install
```

### Interview Tips - Development Environment

**Q: What is the difference between npm and npx?**

**Answer:** "npm is a package manager used to install dependencies, while npx is a package executor that runs packages without installing them globally. For example:

- `npm install cypress` installs Cypress in the project
- `npx cypress open` executes Cypress without global installation

npx is useful for running CLI tools and one-time commands without cluttering global installations."

**Q: Explain package.json vs package-lock.json**

**Answer:** "package.json defines your project dependencies and metadata with version ranges (like ^13.6.0), while package-lock.json locks the exact versions of all dependencies and sub-dependencies. package-lock.json ensures consistent installations across different environments and should be committed to version control to prevent 'works on my machine' issues."

**Q: What is the purpose of devDependencies vs dependencies?**

**Answer:** "dependencies are required for the application to run in production, while devDependencies are only needed during development and testing. For test automation:

- Cypress, Playwright, ESLint go in devDependencies (only needed for testing)
- Libraries used in test helpers might go in dependencies

This separation keeps production bundles smaller and makes the project structure clearer."

---

## 3. JavaScript Basics - Variables, Data Types, and Operators

### Variables in JavaScript

Variables are containers for storing data values. JavaScript has three ways to declare variables: `var`, `let`, and `const`.

#### 3.1 var (Old Way - Avoid)

**Syntax:**
```javascript
var variableName = value;
```

**Characteristics of var:**
1. **Function-scoped** (not block-scoped)
2. Can be re-declared
3. Can be updated
4. Hoisted to the top
5. Can be declared without initialization

**Examples:**

```javascript
// Basic declaration
var username = 'testuser';
var age = 30;
var isActive = true;

console.log(username);  // Output: testuser
console.log(age);       // Output: 30
console.log(isActive);  // Output: true

// Re-declaration allowed (problematic)
var username = 'testuser';
var username = 'newuser';  // No error!
console.log(username);     // Output: newuser

// Function scope
function testVarScope() {
    var x = 10;
    if (true) {
        var x = 20;  // Same variable!
        console.log(x);  // Output: 20
    }
    console.log(x);  // Output: 20 (not 10!)
}

// Hoisting behavior
console.log(name);  // Output: undefined (not error!)
var name = 'John';
console.log(name);  // Output: John

// Above code is interpreted as:
var name;  // Declaration hoisted
console.log(name);  // undefined
name = 'John';  // Assignment stays
console.log(name);  // John
```

**Problems with var (Why to Avoid):**

```javascript
// Problem 1: No block scope
function demonstrateVarIssue() {
    var items = ['item1', 'item2', 'item3'];

    for (var i = 0; i < items.length; i++) {
        setTimeout(function() {
            console.log(i);  // What will this print?
        }, 1000);
    }
    // Output: 3, 3, 3 (not 0, 1, 2!)
    // Because var i is function-scoped, not block-scoped
}

// Problem 2: Accidental global variables
function testGlobalLeak() {
    if (true) {
        var result = 'test';  // Leaks to function scope
    }
    console.log(result);  // Output: test (might not want this)
}

// Problem 3: Re-declaration confusion
var baseUrl = 'https://staging.com';
// ... 100 lines of code later
var baseUrl = 'https://production.com';  // Oops! No warning
```

**Testing Scenario with var (Anti-Pattern):**

```javascript
// BAD - Using var in test automation
describe('Login Tests', () => {
    var testData = {
        username: 'testuser',
        password: 'pass123'
    };

    it('test 1', () => {
        var testData = { username: 'user1' };  // Shadows outer variable
        cy.get('#username').type(testData.username);
    });

    it('test 2', () => {
        // Which testData? Confusing!
        cy.get('#username').type(testData.username);
    });
});
```

#### 3.2 let (Modern Way - Block Scoped)

**Syntax:**
```javascript
let variableName = value;
```

**Characteristics of let:**
1. **Block-scoped** (limited to {} block)
2. Cannot be re-declared in same scope
3. Can be updated
4. Hoisted but not initialized (Temporal Dead Zone)
5. Can be declared without initialization

**Examples:**

```javascript
// Basic declaration
let username = 'testuser';
let age = 30;
let isLoggedIn = true;

console.log(username);  // Output: testuser

// Can be updated
username = 'newuser';
console.log(username);  // Output: newuser

// Cannot be re-declared
let username = 'another';  // Error: Identifier 'username' has already been declared

// Block scope
function testLetScope() {
    let x = 10;
    if (true) {
        let x = 20;  // Different variable!
        console.log(x);  // Output: 20
    }
    console.log(x);  // Output: 10 (preserved!)
}

// Loop scope (fixes var issue)
function demonstrateLetFix() {
    let items = ['item1', 'item2', 'item3'];

    for (let i = 0; i < items.length; i++) {
        setTimeout(function() {
            console.log(i);  // Each iteration has its own i
        }, 1000);
    }
    // Output: 0, 1, 2 (correct!)
}

// Temporal Dead Zone
console.log(name);  // Error: Cannot access 'name' before initialization
let name = 'John';

// Block scoping in conditionals
if (true) {
    let message = 'Inside block';
    console.log(message);  // Output: Inside block
}
console.log(message);  // Error: message is not defined
```

**Testing Scenarios with let:**

```javascript
// GOOD - Using let in test automation
describe('Product Search Tests', () => {
    let searchTerm;
    let results;

    beforeEach(() => {
        cy.visit('/products');
        searchTerm = 'laptop';  // Reset for each test
    });

    it('should search for products', () => {
        cy.get('#search').type(searchTerm);
        cy.get('.product-card').then($products => {
            let productCount = $products.length;
            expect(productCount).to.be.greaterThan(0);
        });
    });

    it('should filter results', () => {
        cy.get('#search').type(searchTerm);

        // Block-scoped filter
        cy.get('#price-filter').select('under-1000');
        cy.get('.product-card').then($products => {
            let filteredCount = $products.length;

            // Different scope, different variable
            cy.get('#price-filter').select('all');
            cy.get('.product-card').then($allProducts => {
                let totalCount = $allProducts.length;
                expect(totalCount).to.be.greaterThan(filteredCount);
            });
        });
    });
});

// Real-world API testing example
async function testUserAPI() {
    let userId;
    let createdUser;

    // Create user
    try {
        let response = await axios.post('/api/users', {
            name: 'Test User',
            email: 'test@test.com'
        });
        createdUser = response.data;
        userId = createdUser.id;
        console.log('Created user:', userId);

        // Get user
        let getResponse = await axios.get(`/api/users/${userId}`);
        let fetchedUser = getResponse.data;

        console.log('Fetched user:', fetchedUser);
        // userId is still accessible here

    } catch (error) {
        console.error('Test failed:', error);
    }
    // userId is still accessible here
}
```

#### 3.3 const (Modern Way - Constant Reference)

**Syntax:**
```javascript
const variableName = value;  // Must be initialized
```

**Characteristics of const:**
1. **Block-scoped** (like let)
2. Cannot be re-declared
3. Cannot be re-assigned
4. Must be initialized at declaration
5. Hoisted but not initialized
6. **For objects/arrays: reference is constant, not content**

**Examples:**

```javascript
// Basic declaration
const API_URL = 'https://api.example.com';
const MAX_RETRIES = 3;
const IS_PRODUCTION = false;

console.log(API_URL);  // Output: https://api.example.com

// Cannot be re-assigned
const API_URL = 'https://new-api.com';  // Error: Assignment to constant variable

// Must be initialized
const username;  // Error: Missing initializer in const declaration

// Block scoped (like let)
if (true) {
    const message = 'Hello';
    console.log(message);  // Output: Hello
}
console.log(message);  // Error: message is not defined

// IMPORTANT: Objects and Arrays
// const means constant REFERENCE, not constant VALUE

// Arrays - Reference is constant, content can change
const testData = ['user1', 'user2', 'user3'];
console.log(testData);  // Output: ['user1', 'user2', 'user3']

testData.push('user4');  // Allowed! Modifying content
console.log(testData);  // Output: ['user1', 'user2', 'user3', 'user4']

testData[0] = 'newUser1';  // Allowed! Modifying content
console.log(testData);  // Output: ['newUser1', 'user2', 'user3', 'user4']

testData = ['new array'];  // Error: Assignment to constant variable

// Objects - Reference is constant, properties can change
const config = {
    baseUrl: 'https://test.com',
    timeout: 5000
};

console.log(config);  // Output: { baseUrl: 'https://test.com', timeout: 5000 }

config.timeout = 10000;  // Allowed! Modifying property
config.retries = 3;      // Allowed! Adding property

console.log(config);
// Output: { baseUrl: 'https://test.com', timeout: 10000, retries: 3 }

config = {};  // Error: Assignment to constant variable

// Freeze object to make it truly immutable
const frozenConfig = Object.freeze({
    apiUrl: 'https://api.com',
    version: 'v1'
});

frozenConfig.version = 'v2';  // Silently fails (strict mode: Error)
console.log(frozenConfig.version);  // Output: v1 (unchanged)
```

**Testing Scenarios with const:**

```javascript
// BEST PRACTICE - Use const by default
describe('E-commerce Tests', () => {
    // Test configuration - should never change
    const config = {
        baseUrl: 'https://shop.example.com',
        timeout: 10000,
        viewport: { width: 1920, height: 1080 }
    };

    // Test data - reference won't change
    const validCredentials = {
        email: 'test@example.com',
        password: 'Test@123'
    };

    const testProducts = [
        { id: 1, name: 'Laptop', price: 999 },
        { id: 2, name: 'Mouse', price: 29 },
        { id: 3, name: 'Keyboard', price: 79 }
    ];

    beforeEach(() => {
        cy.visit(config.baseUrl);
    });

    it('should add products to cart', () => {
        const expectedTotal = testProducts[0].price + testProducts[1].price;

        // Add first two products
        testProducts.slice(0, 2).forEach(product => {
            cy.get(`[data-product-id="${product.id}"]`).click();
        });

        cy.get('.cart-total').then($total => {
            const actualTotal = parseFloat($total.text().replace('$', ''));
            expect(actualTotal).to.equal(expectedTotal);
        });
    });

    it('should handle checkout process', () => {
        // Login
        cy.get('#email').type(validCredentials.email);
        cy.get('#password').type(validCredentials.password);
        cy.get('#login-btn').click();

        // Add product
        const selectedProduct = testProducts[0];
        cy.get(`[data-product-id="${selectedProduct.id}"]`).click();

        // Proceed to checkout
        cy.get('#checkout-btn').click();

        // Verify order
        cy.get('.order-summary').should('contain', selectedProduct.name);
    });
});

// API Testing with const
async function testProductAPI() {
    const API_CONFIG = {
        baseURL: 'https://api.example.com',
        headers: {
            'Content-Type': 'application/json',
            'Authorization': 'Bearer token123'
        }
    };

    // Test data
    const newProduct = {
        name: 'Test Product',
        price: 99.99,
        category: 'Electronics'
    };

    try {
        // POST - Create product
        const createResponse = await axios.post(
            `${API_CONFIG.baseURL}/products`,
            newProduct,
            { headers: API_CONFIG.headers }
        );

        const createdProduct = createResponse.data;
        const productId = createdProduct.id;

        console.log('Created product:', createdProduct);

        // GET - Retrieve product
        const getResponse = await axios.get(
            `${API_CONFIG.baseURL}/products/${productId}`,
            { headers: API_CONFIG.headers }
        );

        const retrievedProduct = getResponse.data;
        console.log('Retrieved product:', retrievedProduct);

        // Assertions
        console.assert(retrievedProduct.name === newProduct.name);
        console.assert(retrievedProduct.price === newProduct.price);

    } catch (error) {
        console.error('API test failed:', error.message);
    }
}
```

### Comparison: var vs let vs const

| Feature | var | let | const |
|---------|-----|-----|-------|
| Scope | Function | Block | Block |
| Re-declaration | Allowed | Not allowed | Not allowed |
| Re-assignment | Allowed | Allowed | Not allowed |
| Initialization | Optional | Optional | Required |
| Hoisting | Yes (initialized undefined) | Yes (TDZ) | Yes (TDZ) |
| Global property | Yes | No | No |
| Best for | Never use | Changing values | Constants, default choice |

**Practical Decision Tree:**

```javascript
// Use const by default
const maxRetries = 3;
const apiUrl = 'https://api.com';
const users = ['user1', 'user2'];

// Use let when value will change
let counter = 0;
let currentUser = null;
let isProcessing = false;

for (let i = 0; i < 10; i++) {  // Use let in loops
    counter += i;
}

// Never use var in modern code
// var oldStyle = 'avoid this';  // DON'T USE
```

### Variables in Testing - Best Practices

**1. Use const by default**
```javascript
describe('Login Tests', () => {
    const testUser = {  // Won't reassign entire object
        email: 'test@test.com',
        password: 'Test@123'
    };

    const selectors = {  // Selectors don't change
        emailInput: '#email',
        passwordInput: '#password',
        submitBtn: '#login-btn'
    };

    it('should login successfully', () => {
        cy.get(selectors.emailInput).type(testUser.email);
        cy.get(selectors.passwordInput).type(testUser.password);
        cy.get(selectors.submitBtn).click();
    });
});
```

**2. Use let for counters and loops**
```javascript
it('should verify multiple elements', () => {
    cy.get('.product-card').then($products => {
        let count = 0;  // Will be modified

        for (let i = 0; i < $products.length; i++) {  // Loop variable
            if ($products[i].textContent.includes('Sale')) {
                count++;
            }
        }

        expect(count).to.be.greaterThan(0);
    });
});
```

**3. Never use var**
```javascript
// DON'T
var baseUrl = 'https://test.com';

// DO
const baseUrl = 'https://test.com';
```

**4. Meaningful variable names**
```javascript
// BAD
const x = 'https://api.com';
const a = 3;
let b = false;

// GOOD
const API_BASE_URL = 'https://api.com';
const MAX_RETRY_ATTEMPTS = 3;
let isTestPassed = false;
```

**5. Constants in UPPER_CASE (convention)**
```javascript
const MAX_TIMEOUT = 30000;
const API_VERSION = 'v2';
const DEFAULT_BROWSER = 'chrome';

// Regular const in camelCase
const testData = { name: 'test' };
const userList = [];
```

### Data Types in JavaScript

JavaScript has 8 data types: 7 primitive types and 1 object type.

#### 3.4 Primitive Data Types

**1. String**

```javascript
// String - Text data
const firstName = 'John';
const lastName = "Doe";
const fullName = `John Doe`;  // Template literal

console.log(typeof firstName);  // Output: string

// String methods useful for testing
const email = '  TEST@EXAMPLE.COM  ';

console.log(email.toLowerCase());      // Output:   test@example.com
console.log(email.trim());             // Output: TEST@EXAMPLE.COM
console.log(email.trim().toLowerCase()); // Output: test@example.com

const url = 'https://example.com/products/123';
console.log(url.includes('products'));  // Output: true
console.log(url.startsWith('https'));   // Output: true
console.log(url.endsWith('123'));       // Output: true

const productId = url.split('/')[4];   // Split by /
console.log(productId);                 // Output: 123

// Testing scenario
const errorMessage = 'Error: Invalid username';
console.log(errorMessage.includes('Invalid'));  // Output: true
console.log(errorMessage.substring(7));  // Output: Invalid username
```

**2. Number**

```javascript
// Number - Integer and floating-point
const age = 30;
const price = 99.99;
const quantity = -5;

console.log(typeof age);    // Output: number
console.log(typeof price);  // Output: number

// Number operations
const subtotal = 100;
const tax = 8.5;
const total = subtotal + (subtotal * tax / 100);
console.log(total);  // Output: 108.5

// Parse strings to numbers (important for testing)
const priceText = '$99.99';
const priceValue = parseFloat(priceText.replace('$', ''));
console.log(priceValue);  // Output: 99.99

const countText = '42';
const count = parseInt(countText);
console.log(count);  // Output: 42

// Check if value is a number
console.log(isNaN('hello'));  // Output: true
console.log(isNaN('123'));    // Output: false
console.log(isNaN(123));      // Output: false

// Special number values
const infinity = 1 / 0;
console.log(infinity);        // Output: Infinity

const notANumber = 'text' / 2;
console.log(notANumber);      // Output: NaN
```

**3. Boolean**

```javascript
// Boolean - true or false
const isActive = true;
const isLoggedIn = false;
const hasPermission = true;

console.log(typeof isActive);  // Output: boolean

// Boolean in conditions (very common in testing)
if (isActive) {
    console.log('User is active');
}

// Comparison operations return boolean
const age = 25;
console.log(age > 18);     // Output: true
console.log(age === 30);   // Output: false

// Testing scenario
const isElementVisible = true;
const isTestPassed = false;

if (isElementVisible && !isTestPassed) {
    console.log('Element visible but test failed');
}
```

**4. Undefined**

```javascript
// Undefined - Variable declared but not assigned
let username;
console.log(username);        // Output: undefined
console.log(typeof username); // Output: undefined

// Function with no return
function doSomething() {
    let x = 5;
    // No return statement
}

const result = doSomething();
console.log(result);  // Output: undefined

// Testing scenario - element not found
let element;
if (false) {  // Some condition
    element = 'button';
}
console.log(element);  // Output: undefined
```

**5. Null**

```javascript
// Null - Intentional absence of value
let selectedUser = null;  // Explicitly set to null
console.log(selectedUser);        // Output: null
console.log(typeof selectedUser); // Output: object (JavaScript quirk!)

// Null vs Undefined
let variable1;          // undefined (not initialized)
let variable2 = null;   // null (intentionally empty)

console.log(variable1);  // Output: undefined
console.log(variable2);  // Output: null

// Testing scenario
let testResult = null;  // No test run yet

function runTest() {
    // ... test logic
    testResult = { passed: true, message: 'Success' };
}

console.log(testResult);  // Output: null (before test)
runTest();
console.log(testResult);  // Output: { passed: true, message: 'Success' }
```

**6. BigInt (Large Integers)**

```javascript
// BigInt - For very large integers
const bigNumber = 1234567890123456789012345678901234567890n;
console.log(typeof bigNumber);  // Output: bigint

// Regular number limit
const maxSafeInteger = Number.MAX_SAFE_INTEGER;
console.log(maxSafeInteger);  // Output: 9007199254740991

// BigInt can exceed this
const evenBigger = 9007199254740991n * 2n;
console.log(evenBigger);  // Output: 18014398509481982n

// Note: Can't mix BigInt and Number
// const mixed = 10n + 5;  // Error!
const valid = 10n + 5n;    // Correct
```

**7. Symbol (Unique Identifiers)**

```javascript
// Symbol - Unique and immutable
const id1 = Symbol('id');
const id2 = Symbol('id');

console.log(id1 === id2);  // Output: false (always unique!)
console.log(typeof id1);   // Output: symbol

// Used for unique object properties (advanced)
const TEST_ID = Symbol('testId');
const testCase = {
    [TEST_ID]: 'TC001',
    name: 'Login Test'
};

console.log(testCase[TEST_ID]);  // Output: TC001
```

#### 3.5 Object Data Type (Non-Primitive)

```javascript
// Object - Collection of key-value pairs
const user = {
    name: 'John Doe',
    email: 'john@example.com',
    age: 30,
    isActive: true
};

console.log(typeof user);  // Output: object
console.log(user.name);    // Output: John Doe
console.log(user['email']); // Output: john@example.com

// Array - Special type of object
const users = ['John', 'Jane', 'Bob'];
console.log(typeof users);       // Output: object
console.log(Array.isArray(users)); // Output: true

// Function - Special type of object
function greet() {
    return 'Hello';
}
console.log(typeof greet);  // Output: function
```

### Type Checking in Testing

```javascript
// Useful type checking functions
function validateTestData(data) {
    // Check if string
    if (typeof data.username === 'string') {
        console.log('Username is valid string');
    }

    // Check if number
    if (typeof data.age === 'number' && !isNaN(data.age)) {
        console.log('Age is valid number');
    }

    // Check if boolean
    if (typeof data.isActive === 'boolean') {
        console.log('isActive is valid boolean');
    }

    // Check if object
    if (typeof data.address === 'object' && data.address !== null) {
        console.log('Address is valid object');
    }

    // Check if array
    if (Array.isArray(data.roles)) {
        console.log('Roles is valid array');
    }

    // Check if undefined
    if (typeof data.optional === 'undefined') {
        console.log('Optional field is not provided');
    }

    // Check if null
    if (data.deletedAt === null) {
        console.log('Item not deleted');
    }
}

// Test data validation
const testUser = {
    username: 'testuser',
    age: 25,
    isActive: true,
    address: { city: 'New York' },
    roles: ['user', 'admin'],
    deletedAt: null
};

validateTestData(testUser);
```

### Operators in JavaScript

#### 3.6 Arithmetic Operators

```javascript
// Basic arithmetic
let a = 10;
let b = 3;

console.log(a + b);  // Addition: 13
console.log(a - b);  // Subtraction: 7
console.log(a * b);  // Multiplication: 30
console.log(a / b);  // Division: 3.3333...
console.log(a % b);  // Modulus (remainder): 1
console.log(a ** b); // Exponentiation: 1000 (10^3)

// Increment / Decrement
let count = 5;
count++;  // count = count + 1
console.log(count);  // Output: 6

count--;  // count = count - 1
console.log(count);  // Output: 5

// Pre vs Post increment
let x = 5;
let y = ++x;  // Pre-increment: x becomes 6, then assigned to y
console.log(x, y);  // Output: 6 6

let m = 5;
let n = m++;  // Post-increment: m assigned to n, then m becomes 6
console.log(m, n);  // Output: 6 5

// Testing scenario - Price calculations
const itemPrice = 99.99;
const quantity = 3;
const discount = 10;  // 10%

const subtotal = itemPrice * quantity;
const discountAmount = subtotal * (discount / 100);
const total = subtotal - discountAmount;

console.log('Subtotal:', subtotal);        // 299.97
console.log('Discount:', discountAmount);  // 29.997
console.log('Total:', total);              // 269.973
console.log('Rounded:', total.toFixed(2)); // 269.97
```

#### 3.7 Assignment Operators

```javascript
let num = 10;

// Basic assignment
num = 20;
console.log(num);  // 20

// Compound assignments
num += 5;   // num = num + 5
console.log(num);  // 25

num -= 3;   // num = num - 3
console.log(num);  // 22

num *= 2;   // num = num * 2
console.log(num);  // 44

num /= 4;   // num = num / 4
console.log(num);  // 11

num %= 3;   // num = num % 3
console.log(num);  // 2

num **= 3;  // num = num ** 3
console.log(num);  // 8

// Testing scenario - accumulating results
let passedTests = 0;
let failedTests = 0;
let totalTests = 0;

// Test 1
passedTests += 1;
totalTests += 1;

// Test 2
failedTests += 1;
totalTests += 1;

// Test 3
passedTests += 1;
totalTests += 1;

console.log(`Passed: ${passedTests}/${totalTests}`);  // Passed: 2/3
console.log(`Failed: ${failedTests}/${totalTests}`);  // Failed: 1/3
```

#### 3.8 Comparison Operators

```javascript
// Equality operators
console.log(5 == '5');   // true (loose equality, type coercion)
console.log(5 === '5');  // false (strict equality, no type coercion)

console.log(5 != '5');   // false (loose inequality)
console.log(5 !== '5');  // true (strict inequality)

// Always use === and !== in testing!

// Relational operators
console.log(10 > 5);   // true
console.log(10 < 5);   // false
console.log(10 >= 10); // true
console.log(10 <= 9);  // false

// Testing scenarios
const expectedCount = 5;
const actualCount = 5;

if (actualCount === expectedCount) {
    console.log('Test Passed: Count matches');
}

const minPrice = 10;
const maxPrice = 100;
const productPrice = 50;

if (productPrice >= minPrice && productPrice <= maxPrice) {
    console.log('Price is in valid range');
}

// String comparison
console.log('apple' < 'banana');  // true (alphabetical)
console.log('10' > '9');          // false (string comparison!)
console.log(10 > 9);              // true (number comparison)
```

**Comparison Table:**

| Operator | Description | Example | Result |
|----------|-------------|---------|--------|
| == | Loose equality | 5 == '5' | true |
| === | Strict equality | 5 === '5' | false |
| != | Loose inequality | 5 != '5' | false |
| !== | Strict inequality | 5 !== '5' | true |
| > | Greater than | 10 > 5 | true |
| < | Less than | 10 < 5 | false |
| >= | Greater than or equal | 10 >= 10 | true |
| <= | Less than or equal | 10 <= 9 | false |

#### 3.9 Logical Operators

```javascript
// AND (&&) - Both conditions must be true
console.log(true && true);   // true
console.log(true && false);  // false
console.log(false && false); // false

// OR (||) - At least one condition must be true
console.log(true || true);   // true
console.log(true || false);  // true
console.log(false || false); // false

// NOT (!) - Inverts boolean value
console.log(!true);   // false
console.log(!false);  // true

// Testing scenarios
const isLoggedIn = true;
const hasPermission = true;
const isActive = false;

// Check multiple conditions
if (isLoggedIn && hasPermission) {
    console.log('Access granted');
}

if (isLoggedIn || hasPermission) {
    console.log('At least one condition met');
}

if (!isActive) {
    console.log('User is not active');
}

// Complex conditions
const age = 25;
const hasLicense = true;
const hasInsurance = true;

if (age >= 18 && hasLicense && hasInsurance) {
    console.log('Can rent a car');
}

// Short-circuit evaluation
const user = null;
const username = user && user.name;  // undefined (user is null, stops here)
console.log(username);

const defaultName = username || 'Guest';  // 'Guest' (username is falsy)
console.log(defaultName);
```

**Logical Operators Truth Table:**

| A | B | A && B | A \|\| B | !A |
|---|---|--------|----------|-----|
| true | true | true | true | false |
| true | false | false | true | false |
| false | true | false | true | true |
| false | false | false | false | true |

#### 3.10 Ternary Operator (Conditional Operator)

```javascript
// Syntax: condition ? valueIfTrue : valueIfFalse

const age = 20;
const status = age >= 18 ? 'Adult' : 'Minor';
console.log(status);  // Output: Adult

// Traditional if-else equivalent
let status2;
if (age >= 18) {
    status2 = 'Adult';
} else {
    status2 = 'Minor';
}

// Testing scenarios
const testResult = true;
const message = testResult ? 'Test Passed' : 'Test Failed';
console.log(message);  // Test Passed

// Nested ternary (use sparingly)
const score = 85;
const grade = score >= 90 ? 'A' :
              score >= 80 ? 'B' :
              score >= 70 ? 'C' :
              'F';
console.log(grade);  // B

// Better as if-else for readability
let grade2;
if (score >= 90) grade2 = 'A';
else if (score >= 80) grade2 = 'B';
else if (score >= 70) grade2 = 'C';
else grade2 = 'F';

// Cypress testing example
cy.get('.status').then($status => {
    const statusText = $status.text();
    const isActive = statusText === 'Active' ? true : false;
    expect(isActive).to.be.true;
});
```

#### 3.11 String Operators

```javascript
// Concatenation with +
const firstName = 'John';
const lastName = 'Doe';
const fullName = firstName + ' ' + lastName;
console.log(fullName);  // John Doe

// Concatenation with +=
let message = 'Hello';
message += ' ';
message += 'World';
console.log(message);  // Hello World

// Template literals (preferred modern way)
const name = 'John';
const age = 30;
const greeting = `My name is ${name} and I am ${age} years old`;
console.log(greeting);  // My name is John and I am 30 years old

// Multi-line strings
const address = `
    123 Main St
    New York, NY 10001
    USA
`;
console.log(address);

// Testing scenario
const testName = 'Login Test';
const testStatus = 'Passed';
const testDuration = 2.5;

const testReport = `
Test: ${testName}
Status: ${testStatus}
Duration: ${testDuration}s
`;

console.log(testReport);

// Building dynamic selectors
const userId = '12345';
const selector = `[data-user-id="${userId}"]`;
cy.get(selector).click();
```

#### 3.12 Type Operators

```javascript
// typeof - Returns type of operand
console.log(typeof 42);          // number
console.log(typeof 'hello');     // string
console.log(typeof true);        // boolean
console.log(typeof undefined);   // undefined
console.log(typeof null);        // object (JavaScript quirk!)
console.log(typeof {});          // object
console.log(typeof []);          // object
console.log(typeof function(){}); // function

// instanceof - Checks if object is instance of class
const arr = [1, 2, 3];
console.log(arr instanceof Array);   // true
console.log(arr instanceof Object);  // true

const date = new Date();
console.log(date instanceof Date);   // true

// Testing scenario - validate data types
function validateAPIResponse(response) {
    if (typeof response.status !== 'number') {
        throw new Error('Invalid status type');
    }

    if (typeof response.data !== 'object') {
        throw new Error('Invalid data type');
    }

    if (!Array.isArray(response.data.users)) {
        throw new Error('Users must be an array');
    }

    return true;
}
```

### Common Mistakes to Avoid

**1. Using == instead of ===**
```javascript
// WRONG
if (count == '5') { }  // true even if count is number 5

// CORRECT
if (count === 5) { }   // only true if count is number 5
```

**2. Type coercion confusion**
```javascript
console.log('5' + 3);   // '53' (string concatenation)
console.log('5' - 3);   // 2 (number subtraction)
console.log('5' * 3);   // 15 (number multiplication)

// Be explicit
const num1 = Number('5') + 3;  // 8
const str1 = String(5) + '3';  // '53'
```

**3. Forgetting const/let**
```javascript
// WRONG
username = 'John';  // Creates global variable (strict mode: error)

// CORRECT
const username = 'John';
```

**4. Using var**
```javascript
// WRONG
var message = 'test';

// CORRECT
const message = 'test';
```

**5. Not handling null/undefined**
```javascript
// WRONG
const user = null;
console.log(user.name);  // Error: Cannot read property 'name' of null

// CORRECT
const user = null;
const name = user?.name;  // undefined (optional chaining)
// or
const name2 = user && user.name;  // null
```

### Interview Tips - JavaScript Basics

**Q1: What is the difference between == and ===?**

**Answer:** "== is loose equality that performs type coercion, while === is strict equality that checks both value and type.

```javascript
console.log(5 == '5');   // true (coerced to same type)
console.log(5 === '5');  // false (different types)
console.log(null == undefined);  // true
console.log(null === undefined); // false
```

In testing, always use === to avoid unexpected type coercion bugs. It makes tests more predictable and catches type mismatches."

**Q2: Explain var, let, and const**

**Answer:** "All three declare variables, but with different scoping and mutability:

- **var**: Function-scoped, can be re-declared, hoisted. Deprecated.
- **let**: Block-scoped, can be reassigned, cannot be re-declared in same scope.
- **const**: Block-scoped, cannot be reassigned, must be initialized.

Best practice: Use const by default, let when the value will change, never use var.

```javascript
const MAX_RETRIES = 3;  // Won't change
let currentAttempt = 0;  // Will change
for (let i = 0; i < 5; i++) { }  // Loop variable
```

For objects and arrays, const means the reference is constant, not the content."

**Q3: What are JavaScript data types?**

**Answer:** "JavaScript has 8 data types:

**Primitives (7):**
1. String: `'text'`
2. Number: `42`, `3.14`
3. Boolean: `true`, `false`
4. Undefined: `undefined`
5. Null: `null`
6. BigInt: `123n`
7. Symbol: `Symbol('id')`

**Non-primitive (1):**
8. Object: `{}`, `[]`, `function(){}`

In testing, we mainly work with strings, numbers, booleans, objects, and arrays. Understanding null vs undefined is crucial for API response validation."

**Q4: Explain truthy and falsy values**

**Answer:** "In JavaScript, values are coerced to boolean in conditional contexts.

**Falsy values (8):**
- `false`
- `0`
- `''` (empty string)
- `null`
- `undefined`
- `NaN`
- `0n` (BigInt zero)
- `document.all` (legacy)

**Everything else is truthy**, including:
- `'0'` (string)
- `'false'` (string)
- `[]` (empty array)
- `{}` (empty object)

```javascript
if ('0') console.log('truthy');  // Prints (string '0' is truthy)
if (0) console.log('falsy');     // Doesn't print (number 0 is falsy)
```

This is important when checking API responses or element existence."

### Real-World Testing Examples

**Example 1: Form Validation Test**

```javascript
describe('Registration Form', () => {
    const validUser = {
        email: 'test@example.com',
        password: 'Test@123',
        age: 25
    };

    const invalidUsers = [
        { email: 'invalid', password: 'Test@123', age: 25 },
        { email: 'test@example.com', password: '123', age: 25 },
        { email: 'test@example.com', password: 'Test@123', age: 15 }
    ];

    it('should accept valid user', () => {
        cy.get('#email').type(validUser.email);
        cy.get('#password').type(validUser.password);
        cy.get('#age').type(validUser.age);
        cy.get('#submit').click();

        cy.get('.success-message').should('be.visible');
    });

    invalidUsers.forEach((user, index) => {
        it(`should reject invalid user ${index + 1}`, () => {
            cy.get('#email').type(user.email);
            cy.get('#password').type(user.password);
            cy.get('#age').type(user.age);
            cy.get('#submit').click();

            cy.get('.error-message').should('be.visible');
        });
    });
});
```

**Example 2: API Response Validation**

```javascript
async function validateProductAPI() {
    const API_URL = 'https://api.example.com/products';
    const MIN_PRICE = 0;
    const MAX_PRICE = 10000;

    try {
        const response = await axios.get(API_URL);
        const products = response.data;

        // Validate array
        if (!Array.isArray(products)) {
            throw new Error('Response is not an array');
        }

        // Validate each product
        products.forEach((product, index) => {
            // Check required fields
            if (typeof product.id !== 'number') {
                throw new Error(`Product ${index}: Invalid id type`);
            }

            if (typeof product.name !== 'string') {
                throw new Error(`Product ${index}: Invalid name type`);
            }

            if (typeof product.price !== 'number') {
                throw new Error(`Product ${index}: Invalid price type`);
            }

            // Validate price range
            if (product.price < MIN_PRICE || product.price > MAX_PRICE) {
                throw new Error(`Product ${index}: Price out of range`);
            }

            // Check boolean field
            if (typeof product.inStock !== 'boolean') {
                throw new Error(`Product ${index}: Invalid inStock type`);
            }
        });

        console.log(`✓ All ${products.length} products validated successfully`);
        return true;

    } catch (error) {
        console.error('Validation failed:', error.message);
        return false;
    }
}
```

---
## 4. Control Structures

Control structures determine the flow of program execution based on conditions and loops.

### 4.1 if-else Statements

**Basic if Statement:**

```javascript
const age = 18;

if (age >= 18) {
    console.log('You are an adult');
}
// Output: You are an adult

// Testing scenario
const isElementVisible = true;

if (isElementVisible) {
    console.log('Element is visible - test can proceed');
    // Perform test actions
}
```

**if-else Statement:**

```javascript
const testResult = false;

if (testResult) {
    console.log('Test Passed');
} else {
    console.log('Test Failed');
}
// Output: Test Failed

// Testing scenario - Login validation
const username = 'testuser';
const password = 'correct';

if (username === 'testuser' && password === 'correct') {
    console.log('Login successful');
} else {
    console.log('Invalid credentials');
}
// Output: Login successful
```

**if-else if-else Statement:**

```javascript
const score = 85;

if (score >= 90) {
    console.log('Grade: A');
} else if (score >= 80) {
    console.log('Grade: B');
} else if (score >= 70) {
    console.log('Grade: C');
} else if (score >= 60) {
    console.log('Grade: D');
} else {
    console.log('Grade: F');
}
// Output: Grade: B

// Testing scenario - HTTP status codes
const statusCode = 404;

if (statusCode >= 200 && statusCode < 300) {
    console.log('Success');
} else if (statusCode >= 300 && statusCode < 400) {
    console.log('Redirection');
} else if (statusCode >= 400 && statusCode < 500) {
    console.log('Client Error');
} else if (statusCode >= 500) {
    console.log('Server Error');
}
// Output: Client Error
```

**Nested if Statements:**

```javascript
const isLoggedIn = true;
const hasPermission = true;
const isActive = true;

if (isLoggedIn) {
    if (hasPermission) {
        if (isActive) {
            console.log('Access granted');
        } else {
            console.log('Account inactive');
        }
    } else {
        console.log('No permission');
    }
} else {
    console.log('Please login');
}
// Output: Access granted

// Better approach - flattened
if (!isLoggedIn) {
    console.log('Please login');
} else if (!hasPermission) {
    console.log('No permission');
} else if (!isActive) {
    console.log('Account inactive');
} else {
    console.log('Access granted');
}
```

**Real-World Testing Example:**

```javascript
describe('E-commerce Checkout', () => {
    it('should validate checkout conditions', () => {
        cy.visit('/cart');

        cy.get('.cart-item').then($items => {
            const itemCount = $items.length;

            if (itemCount === 0) {
                cy.get('.empty-cart-message').should('be.visible');
                cy.get('#checkout-btn').should('be.disabled');
            } else if (itemCount > 0 && itemCount <= 10) {
                cy.get('#checkout-btn').should('be.enabled');
                cy.get('.standard-shipping').should('be.visible');
            } else {
                cy.get('#checkout-btn').should('be.enabled');
                cy.get('.bulk-order-notice').should('be.visible');
            }
        });
    });
});

// API Testing Example
async function validateAPIResponse(response) {
    if (response.status === 200) {
        console.log('✓ Request successful');

        if (response.data && Array.isArray(response.data)) {
            console.log(`✓ Received ${response.data.length} items`);

            if (response.data.length > 0) {
                console.log('✓ Data is not empty');
                return { success: true, data: response.data };
            } else {
                console.log('⚠ Warning: Empty response array');
                return { success: true, data: [], warning: 'No data' };
            }
        } else {
            console.error('✗ Invalid response format');
            return { success: false, error: 'Invalid format' };
        }
    } else if (response.status === 404) {
        console.error('✗ Resource not found');
        return { success: false, error: 'Not found' };
    } else if (response.status >= 500) {
        console.error('✗ Server error');
        return { success: false, error: 'Server error' };
    } else {
        console.error(`✗ Unexpected status: ${response.status}`);
        return { success: false, error: 'Unexpected status' };
    }
}
```

### 4.2 Ternary Operator (Conditional Operator)

**Basic Syntax:**

```javascript
// condition ? valueIfTrue : valueIfFalse

const age = 20;
const category = age >= 18 ? 'Adult' : 'Minor';
console.log(category);  // Output: Adult

// Testing scenario
const testPassed = true;
const status = testPassed ? 'PASS' : 'FAIL';
console.log(`Test Status: ${status}`);  // Test Status: PASS
```

**Inline Usage:**

```javascript
// In logging
const itemsCount = 5;
console.log(`Found ${itemsCount} ${itemsCount === 1 ? 'item' : 'items'}`);
// Output: Found 5 items

// In Cypress assertions
cy.get('.product-card').then($products => {
    const count = $products.length;
    const message = count > 0 ? 'Products found' : 'No products';
    cy.log(message);
});
```

### Real-World Testing Examples

**Example 1: Data-Driven Testing**

```javascript
describe('Login with Multiple Users', () => {
    const testUsers = [
        { username: 'admin', password: 'admin123', role: 'admin' },
        { username: 'user1', password: 'user123', role: 'user' },
        { username: 'manager', password: 'manager123', role: 'manager' }
    ];

    for (const user of testUsers) {
        it(`should login as ${user.role}`, () => {
            cy.visit('/login');
            cy.get('#username').type(user.username);
            cy.get('#password').type(user.password);
            cy.get('#login-btn').click();

            cy.url().should('include', '/dashboard');
            cy.get('.user-role').should('contain', user.role);
        });
    }
});
```

Due to the large size (7000-8000 lines), I'll continue creating this file in multiple chunks. Would you like me to continue by creating the remaining sections (5-17)?


## 5. Functions in JavaScript

Functions are reusable blocks of code that perform specific tasks. Critical for test automation to avoid code duplication.

### 5.1 Function Declaration

```javascript
// Basic function
function greet() {
    console.log('Hello, World!');
}

greet();  // Output: Hello, World!

// Function with parameters
function login(username, password) {
    cy.get('#username').type(username);
    cy.get('#password').type(password);
    cy.get('#login-btn').click();
}

login('testuser', 'password123');

// Function with return value
function add(a, b) {
    return a + b;
}

const sum = add(5, 3);
console.log(sum);  // Output: 8
```

### 5.2 Function Expression

```javascript
// Function assigned to variable
const greet = function() {
    console.log('Hello!');
};

greet();  // Output: Hello!

// Testing scenario
const loginPage = {
    login: function(username, password) {
        cy.get('#username').type(username);
        cy.get('#password').type(password);
        cy.get('#login-btn').click();
    }
};

loginPage.login('testuser', 'pass123');
```

### 5.3 Arrow Functions

```javascript
// Traditional function
const add1 = function(a, b) {
    return a + b;
};

// Arrow function - concise
const add2 = (a, b) => a + b;

console.log(add2(5, 3));  // Output: 8

// Single parameter
const square = x => x * x;
console.log(square(5));  // Output: 25

// No parameters
const greet = () => console.log('Hello!');

// Testing with arrow functions
describe('Login Tests', () => {
    beforeEach(() => {
        cy.visit('/login');
    });

    it('should login successfully', () => {
        cy.get('#email').type('test@test.com');
        cy.get('#password').type('Test@123');
        cy.get('#login-btn').click();
        cy.url().should('include', '/dashboard');
    });
});

// Array methods with arrow functions
const testResults = ['pass', 'fail', 'pass', 'pass', 'fail'];
const passCount = testResults.filter(result => result === 'pass').length;
console.log(passCount);  // 3
```

### 5.4 Function Parameters

```javascript
// Default parameters
function greet(name = 'Guest') {
    console.log(`Hello, ${name}!`);
}

greet();        // Hello, Guest!
greet('John');  // Hello, John!

// Rest parameters
function sum(...numbers) {
    return numbers.reduce((total, num) => total + num, 0);
}

console.log(sum(1, 2, 3));        // 6
console.log(sum(1, 2, 3, 4, 5));  // 15

// Testing scenario
function waitForElement(selector, timeout = 5000) {
    cy.get(selector, { timeout: timeout });
}

waitForElement('.loader');  // Uses default 5000ms
waitForElement('.slow-element', 10000);  // Uses 10000ms
```

### 5.5 Closures

```javascript
// Closure - Function remembers outer scope
function createCounter() {
    let count = 0;  // Private variable

    return function() {
        count++;
        return count;
    };
}

const counter = createCounter();
console.log(counter());  // 1
console.log(counter());  // 2
console.log(counter());  // 3

// Testing scenario - API client factory
function createAPIClient(baseURL, apiKey) {
    const config = {
        baseURL: baseURL,
        headers: {
            'Authorization': `Bearer ${apiKey}`,
            'Content-Type': 'application/json'
        }
    };

    return {
        get: async function(endpoint) {
            const response = await fetch(`${config.baseURL}${endpoint}`, {
                headers: config.headers
            });
            return response.json();
        },

        post: async function(endpoint, data) {
            const response = await fetch(`${config.baseURL}${endpoint}`, {
                method: 'POST',
                headers: config.headers,
                body: JSON.stringify(data)
            });
            return response.json();
        }
    };
}

const apiClient = createAPIClient('https://api.example.com', 'secret-key');
// apiClient remembers baseURL and apiKey through closure
```

### Interview Tips - Functions

**Q: What is the difference between function declaration and expression?**

**Answer:** "Function declarations are hoisted (can be called before definition), while expressions are not.

```javascript
// Declaration - hoisted
greet();  // Works!
function greet() { console.log('Hi'); }

// Expression - not hoisted
sayHi();  // Error!
const sayHi = function() { console.log('Hi'); };
```

**Q: Explain closures**

**Answer:** "A closure is when a function remembers variables from its outer scope. Useful for creating private variables and factories in test frameworks."

---

## 6. Objects and Arrays

### 6.1 Objects

**Creating Objects:**

```javascript
// Object literal
const user = {
    name: 'John Doe',
    age: 30,
    email: 'john@example.com',
    isActive: true
};

console.log(user.name);  // John Doe
console.log(user['email']);  // john@example.com

// Testing scenario
const testConfig = {
    baseUrl: 'https://example.com',
    timeout: 10000,
    browser: 'chrome',
    headless: false
};

cy.visit(testConfig.baseUrl, { timeout: testConfig.timeout });
```

**Accessing Properties:**

```javascript
const product = {
    id: 101,
    name: 'Laptop',
    price: 999.99,
    inStock: true
};

// Dot notation
console.log(product.name);  // Laptop

// Bracket notation
console.log(product['price']);  // 999.99

// Dynamic property access
const propertyName = 'inStock';
console.log(product[propertyName]);  // true
```

**Adding/Modifying Properties:**

```javascript
const user = {
    name: 'John'
};

// Add property
user.age = 30;
user['email'] = 'john@example.com';

console.log(user);  // { name: 'John', age: 30, email: 'john@example.com' }

// Modify property
user.name = 'Jane';
console.log(user.name);  // Jane

// Delete property
delete user.age;
console.log(user);  // { name: 'Jane', email: 'john@example.com' }
```

**Object Methods:**

```javascript
const user = {
    firstName: 'John',
    lastName: 'Doe',
    age: 30,

    // Method
    getFullName: function() {
        return `${this.firstName} ${this.lastName}`;
    },

    // Shorthand method syntax (ES6)
    greet() {
        return `Hello, I'm ${this.getFullName()}`;
    }
};

console.log(user.getFullName());  // John Doe
console.log(user.greet());  // Hello, I'm John Doe
```

**Object Destructuring:**

```javascript
const user = {
    name: 'John',
    age: 30,
    email: 'john@example.com'
};

// Extract properties
const { name, age, email } = user;
console.log(name);  // John
console.log(age);   // 30

// Rename variables
const { name: userName, age: userAge } = user;
console.log(userName);  // John

// Default values
const { country = 'USA' } = user;
console.log(country);  // USA (default value)

// Testing scenario
const testData = {
    username: 'testuser',
    password: 'Test@123',
    email: 'test@example.com'
};

const { username, password } = testData;

cy.get('#username').type(username);
cy.get('#password').type(password);
```

**Object Methods:**

```javascript
const user = {
    name: 'John',
    age: 30,
    email: 'john@example.com'
};

// Object.keys() - Get all keys
const keys = Object.keys(user);
console.log(keys);  // ['name', 'age', 'email']

// Object.values() - Get all values
const values = Object.values(user);
console.log(values);  // ['John', 30, 'john@example.com']

// Object.entries() - Get key-value pairs
const entries = Object.entries(user);
console.log(entries);
// [['name', 'John'], ['age', 30], ['email', 'john@example.com']]

// Iterate over object
for (const [key, value] of Object.entries(user)) {
    console.log(`${key}: ${value}`);
}
// Output:
// name: John
// age: 30
// email: john@example.com

// Object.assign() - Copy/merge objects
const defaults = { timeout: 5000, retries: 3 };
const customConfig = { timeout: 10000 };

const finalConfig = Object.assign({}, defaults, customConfig);
console.log(finalConfig);  // { timeout: 10000, retries: 3 }

// Spread operator (modern way)
const merged = { ...defaults, ...customConfig };
console.log(merged);  // { timeout: 10000, retries: 3 }
```

### 6.2 Arrays

**Creating Arrays:**

```javascript
// Array literal
const fruits = ['apple', 'banana', 'orange'];
console.log(fruits);  // ['apple', 'banana', 'orange']

// Array constructor
const numbers = new Array(1, 2, 3, 4, 5);
console.log(numbers);  // [1, 2, 3, 4, 5]

// Empty array
const empty = [];

// Mixed types
const mixed = [1, 'text', true, { name: 'John' }, [1, 2, 3]];
console.log(mixed);  // [1, 'text', true, { name: 'John' }, [1, 2, 3]]

// Testing scenario
const testUsers = [
    { username: 'user1', password: 'pass1' },
    { username: 'user2', password: 'pass2' },
    { username: 'user3', password: 'pass3' }
];
```

**Accessing Array Elements:**

```javascript
const fruits = ['apple', 'banana', 'orange', 'grape'];

// By index (0-based)
console.log(fruits[0]);  // apple
console.log(fruits[2]);  // orange

// Last element
console.log(fruits[fruits.length - 1]);  // grape

// Array length
console.log(fruits.length);  // 4

// Check if array
console.log(Array.isArray(fruits));  // true
```

**Modifying Arrays:**

```javascript
const fruits = ['apple', 'banana'];

// Add to end
fruits.push('orange');
console.log(fruits);  // ['apple', 'banana', 'orange']

// Add to beginning
fruits.unshift('grape');
console.log(fruits);  // ['grape', 'apple', 'banana', 'orange']

// Remove from end
const last = fruits.pop();
console.log(last);  // orange
console.log(fruits);  // ['grape', 'apple', 'banana']

// Remove from beginning
const first = fruits.shift();
console.log(first);  // grape
console.log(fruits);  // ['apple', 'banana']

// Update element
fruits[0] = 'mango';
console.log(fruits);  // ['mango', 'banana']
```

**Array Destructuring:**

```javascript
const colors = ['red', 'green', 'blue'];

// Extract elements
const [first, second, third] = colors;
console.log(first);   // red
console.log(second);  // green

// Skip elements
const [primaryColor, , tertiaryColor] = colors;
console.log(primaryColor);    // red
console.log(tertiaryColor);   // blue

// Rest elements
const numbers = [1, 2, 3, 4, 5];
const [one, two, ...rest] = numbers;
console.log(one);   // 1
console.log(two);   // 2
console.log(rest);  // [3, 4, 5]

// Testing scenario
const credentials = ['testuser', 'password123'];
const [username, password] = credentials;

cy.get('#username').type(username);
cy.get('#password').type(password);
```

**Array Methods:**

```javascript
const numbers = [1, 2, 3, 4, 5];

// forEach() - Iterate
numbers.forEach(num => {
    console.log(num * 2);
});
// Output: 2 4 6 8 10

// map() - Transform array
const doubled = numbers.map(num => num * 2);
console.log(doubled);  // [2, 4, 6, 8, 10]

// filter() - Filter array
const evenNumbers = numbers.filter(num => num % 2 === 0);
console.log(evenNumbers);  // [2, 4]

// find() - Find first match
const firstEven = numbers.find(num => num % 2 === 0);
console.log(firstEven);  // 2

// findIndex() - Find index of first match
const index = numbers.findIndex(num => num === 3);
console.log(index);  // 2

// includes() - Check if element exists
console.log(numbers.includes(3));  // true
console.log(numbers.includes(10)); // false

// indexOf() - Get index
console.log(numbers.indexOf(3));   // 2
console.log(numbers.indexOf(10));  // -1 (not found)

// some() - Check if any match
const hasEven = numbers.some(num => num % 2 === 0);
console.log(hasEven);  // true

// every() - Check if all match
const allPositive = numbers.every(num => num > 0);
console.log(allPositive);  // true

// reduce() - Accumulate value
const sum = numbers.reduce((total, num) => total + num, 0);
console.log(sum);  // 15

// slice() - Extract portion (doesn't modify original)
const sliced = numbers.slice(1, 4);
console.log(sliced);  // [2, 3, 4]
console.log(numbers); // [1, 2, 3, 4, 5] (unchanged)

// splice() - Add/remove elements (modifies original)
const fruits = ['apple', 'banana', 'orange', 'grape'];
fruits.splice(2, 1, 'mango', 'kiwi');  // At index 2, remove 1, add 2
console.log(fruits);  // ['apple', 'banana', 'mango', 'kiwi', 'grape']

// concat() - Combine arrays
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const combined = arr1.concat(arr2);
console.log(combined);  // [1, 2, 3, 4, 5, 6]

// join() - Join elements into string
const words = ['Hello', 'World'];
const sentence = words.join(' ');
console.log(sentence);  // Hello World

// reverse() - Reverse array (modifies original)
const letters = ['a', 'b', 'c'];
letters.reverse();
console.log(letters);  // ['c', 'b', 'a']

// sort() - Sort array (modifies original)
const unsorted = [3, 1, 4, 1, 5, 9];
unsorted.sort((a, b) => a - b);  // Ascending
console.log(unsorted);  // [1, 1, 3, 4, 5, 9]
```

**Testing Scenarios with Arrays:**

```javascript
// Test data management
const testUsers = [
    { id: 1, username: 'user1', role: 'admin' },
    { id: 2, username: 'user2', role: 'user' },
    { id: 3, username: 'user3', role: 'user' }
];

// Get admin users
const admins = testUsers.filter(user => user.role === 'admin');
console.log(admins);

// Get usernames
const usernames = testUsers.map(user => user.username);
console.log(usernames);  // ['user1', 'user2', 'user3']

// Check if any admin exists
const hasAdmin = testUsers.some(user => user.role === 'admin');
console.log(hasAdmin);  // true

// Validate all users
const allValid = testUsers.every(user => user.id && user.username);
console.log(allValid);  // true

// API response validation
async function validateAPIResponse() {
    const response = await fetch('https://api.example.com/products');
    const products = await response.json();

    // Check if array
    if (!Array.isArray(products)) {
        throw new Error('Response is not an array');
    }

    // Filter valid products
    const validProducts = products.filter(p =>
        p.id &&
        p.name &&
        typeof p.price === 'number' &&
        p.price >= 0
    );

    console.log(`Valid: ${validProducts.length}/${products.length}`);
    return validProducts;
}
```

### Interview Tips - Objects and Arrays

**Q: What is the difference between array methods map() and forEach()?**

**Answer:** "forEach() iterates but returns undefined, while map() returns a new transformed array.

```javascript
const numbers = [1, 2, 3];

// forEach - no return value
numbers.forEach(n => console.log(n * 2));  // undefined

// map - returns new array
const doubled = numbers.map(n => n * 2);  // [2, 4, 6]
```

Use forEach for side effects (logging, DOM manipulation), map for transforming data."

**Q: Explain destructuring**

**Answer:** "Destructuring extracts values from arrays/objects into variables.

```javascript
// Array
const [first, second] = ['a', 'b'];

// Object
const { name, age } = { name: 'John', age: 30 };
```

Common in testing for extracting test data and API responses."

---


## 7. Spread and Rest Operators

The spread (...) and rest (...) operators look identical but serve different purposes based on context.

### 7.1 Spread Operator - Expanding Elements

**Spread in Arrays:**

```javascript
// Copying arrays
const original = [1, 2, 3];
const copy = [...original];
console.log(copy);  // [1, 2, 3]

// Arrays are independent
copy.push(4);
console.log(original);  // [1, 2, 3] (unchanged)
console.log(copy);      // [1, 2, 3, 4]

// Combining arrays
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const combined = [...arr1, ...arr2];
console.log(combined);  // [1, 2, 3, 4, 5, 6]

// Adding elements while spreading
const numbers = [2, 3, 4];
const expanded = [1, ...numbers, 5];
console.log(expanded);  // [1, 2, 3, 4, 5]

// Testing scenario - Combining test data
const defaultUsers = [
    { username: 'admin', password: 'admin123' },
    { username: 'user1', password: 'user123' }
];

const additionalUsers = [
    { username: 'manager', password: 'manager123' }
];

const allUsers = [...defaultUsers, ...additionalUsers];
console.log(`Total users: ${allUsers.length}`);  // 3

// Cypress testing example
describe('Multi-user Login Tests', () => {
    const baseUsers = [
        { email: 'user1@test.com', role: 'user' },
        { email: 'user2@test.com', role: 'user' }
    ];

    const adminUsers = [
        { email: 'admin@test.com', role: 'admin' }
    ];

    const allTestUsers = [...baseUsers, ...adminUsers];

    allTestUsers.forEach(user => {
        it(`should login as ${user.role}`, () => {
            cy.visit('/login');
            cy.get('#email').type(user.email);
            cy.get('#password').type('Test@123');
            cy.get('#login-btn').click();
            cy.get('.user-role').should('contain', user.role);
        });
    });
});
```

**Spread in Objects:**

```javascript
// Copying objects
const original = { name: 'John', age: 30 };
const copy = { ...original };
console.log(copy);  // { name: 'John', age: 30 }

// Merging objects
const defaults = { timeout: 5000, retries: 3, headless: false };
const customConfig = { timeout: 10000, browser: 'chrome' };

const finalConfig = { ...defaults, ...customConfig };
console.log(finalConfig);
// { timeout: 10000, retries: 3, headless: false, browser: 'chrome' }

// Adding properties while spreading
const user = { name: 'John', age: 30 };
const userWithEmail = { ...user, email: 'john@example.com' };
console.log(userWithEmail);
// { name: 'John', age: 30, email: 'john@example.com' }

// Overriding properties (order matters!)
const config1 = { ...defaults, timeout: 15000 };  // Override after spread
console.log(config1.timeout);  // 15000

const config2 = { timeout: 15000, ...defaults };  // Override before spread
console.log(config2.timeout);  // 5000 (defaults overrides it)

// Testing scenario - Test configuration management
const baseConfig = {
    baseUrl: 'https://test.example.com',
    timeout: 10000,
    viewportWidth: 1920,
    viewportHeight: 1080
};

// Smoke test config - faster
const smokeConfig = {
    ...baseConfig,
    timeout: 5000,
    video: false
};

// Regression test config - comprehensive
const regressionConfig = {
    ...baseConfig,
    timeout: 30000,
    video: true,
    screenshotOnRunFailure: true
};

console.log('Smoke Config:', smokeConfig);
console.log('Regression Config:', regressionConfig);

// Cypress config example
function getTestConfig(environment) {
    const baseConfig = {
        viewportWidth: 1280,
        viewportHeight: 720,
        defaultCommandTimeout: 10000
    };

    const envConfigs = {
        dev: {
            baseUrl: 'http://localhost:3000',
            video: false
        },
        staging: {
            baseUrl: 'https://staging.example.com',
            video: true
        },
        production: {
            baseUrl: 'https://example.com',
            video: true,
            videoCompression: 32
        }
    };

    return { ...baseConfig, ...envConfigs[environment] };
}

console.log(getTestConfig('staging'));
```

**Spread in Function Calls:**

```javascript
// Spread array as function arguments
const numbers = [1, 5, 3, 9, 2];

// Without spread
console.log(Math.max(numbers));  // NaN (array as single argument)

// With spread
console.log(Math.max(...numbers));  // 9

// Testing scenario - Dynamic test execution
function runTest(testName, priority, timeout, retry) {
    console.log(`Test: ${testName}`);
    console.log(`Priority: ${priority}`);
    console.log(`Timeout: ${timeout}ms`);
    console.log(`Retry: ${retry}`);
}

const testConfig = ['Login Test', 'P0', 5000, true];
runTest(...testConfig);
// Output:
// Test: Login Test
// Priority: P0
// Timeout: 5000ms
// Retry: true

// API testing with spread
const apiTestParams = {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ name: 'Test' })
};

fetch('https://api.example.com/users', { ...apiTestParams })
    .then(response => response.json())
    .then(data => console.log(data));
```

### 7.2 Rest Operator - Collecting Elements

**Rest in Function Parameters:**

```javascript
// Collect remaining arguments into array
function sum(...numbers) {
    console.log(numbers);  // Array of all arguments
    return numbers.reduce((total, num) => total + num, 0);
}

console.log(sum(1, 2, 3));        // 6
console.log(sum(1, 2, 3, 4, 5));  // 15

// Mix regular and rest parameters
function createTestReport(testName, status, ...details) {
    console.log(`Test Name: ${testName}`);
    console.log(`Status: ${status}`);
    console.log(`Additional Details:`, details);
}

createTestReport('Login Test', 'Passed', 'Duration: 2s', 'Browser: Chrome', 'Retries: 0');
// Output:
// Test Name: Login Test
// Status: Passed
// Additional Details: ['Duration: 2s', 'Browser: Chrome', 'Retries: 0']

// Testing scenario - Flexible assertion function
function assertValues(actual, ...expected) {
    const isValid = expected.includes(actual);
    
    if (isValid) {
        console.log(`✓ ${actual} is valid (expected: ${expected.join(', ')})`);
        return true;
    } else {
        console.error(`✗ ${actual} is not in expected values: ${expected.join(', ')}`);
        return false;
    }
}

assertValues(200, 200, 201, 204);  // ✓ 200 is valid
assertValues(404, 200, 201, 204);  // ✗ 404 is not valid

// Cypress custom command with rest parameters
Cypress.Commands.add('fillForm', (formSelector, ...fieldData) => {
    fieldData.forEach(({ selector, value }) => {
        cy.get(`${formSelector} ${selector}`).type(value);
    });
});

// Usage
cy.fillForm('#registration-form',
    { selector: '#name', value: 'John Doe' },
    { selector: '#email', value: 'john@example.com' },
    { selector: '#phone', value: '1234567890' }
);
```

**Rest in Array Destructuring:**

```javascript
const numbers = [1, 2, 3, 4, 5];

// Extract first elements, rest into array
const [first, second, ...rest] = numbers;
console.log(first);   // 1
console.log(second);  // 2
console.log(rest);    // [3, 4, 5]

// Skip elements
const [one, , three, ...remaining] = numbers;
console.log(one);        // 1
console.log(three);      // 3
console.log(remaining);  // [4, 5]

// Testing scenario - Test data management
const testResults = ['TC001-PASS', 'TC002-FAIL', 'TC003-PASS', 'TC004-PASS', 'TC005-FAIL'];

const [firstTest, ...otherTests] = testResults;
console.log(`First: ${firstTest}`);
console.log(`Remaining: ${otherTests.length} tests`);

// API response handling
const apiResponse = {
    data: [
        { id: 1, name: 'Product 1', price: 100 },
        { id: 2, name: 'Product 2', price: 200 },
        { id: 3, name: 'Product 3', price: 300 },
        { id: 4, name: 'Product 4', price: 400 }
    ]
};

const [firstProduct, secondProduct, ...otherProducts] = apiResponse.data;
console.log('First two products:', firstProduct, secondProduct);
console.log('Other products:', otherProducts.length);
```

**Rest in Object Destructuring:**

```javascript
const user = {
    id: 1,
    name: 'John Doe',
    email: 'john@example.com',
    age: 30,
    city: 'New York',
    country: 'USA'
};

// Extract specific properties, rest into object
const { name, email, ...otherInfo } = user;
console.log(name);       // John Doe
console.log(email);      // john@example.com
console.log(otherInfo);  // { id: 1, age: 30, city: 'New York', country: 'USA' }

// Testing scenario - Separating test metadata
const testCase = {
    id: 'TC001',
    name: 'Login Test',
    priority: 'P0',
    timeout: 10000,
    description: 'Test login functionality',
    steps: ['Navigate to login', 'Enter credentials', 'Click login'],
    expectedResult: 'User successfully logged in'
};

const { id, name, ...testDetails } = testCase;
console.log(`Running test: ${id} - ${name}`);
console.log('Test details:', testDetails);

// API response cleanup
const apiUser = {
    id: 123,
    username: 'testuser',
    password: 'encrypted',
    token: 'secret-token',
    email: 'test@example.com',
    firstName: 'Test',
    lastName: 'User'
};

// Remove sensitive data
const { password, token, ...publicUserInfo } = apiUser;
console.log('Public info:', publicUserInfo);
// { id: 123, username: 'testuser', email: 'test@example.com', firstName: 'Test', lastName: 'User' }
```

### 7.3 Spread vs Rest - Key Differences

| Feature | Spread (...) | Rest (...) |
|---------|--------------|------------|
| Purpose | Expand/unpack elements | Collect/gather elements |
| Used in | Array literals, object literals, function calls | Function parameters, destructuring |
| Direction | Spreads OUT | Collects IN |
| Example | `[...arr]` `{...obj}` `func(...arr)` | `function(...args)` `const [a, ...rest] = arr` |

**Visual Comparison:**

```javascript
// SPREAD - Expanding (spreading OUT)
const arr1 = [1, 2, 3];
const arr2 = [...arr1, 4, 5];  // Spreads arr1 into individual elements
console.log(arr2);  // [1, 2, 3, 4, 5]

// REST - Collecting (gathering IN)
function sum(...numbers) {  // Collects arguments into numbers array
    return numbers.reduce((a, b) => a + b, 0);
}
console.log(sum(1, 2, 3, 4, 5));  // 15

// Same operator, different context!
```

### 7.4 Real-World Testing Examples

**Example 1: Test Data Management**

```javascript
// Base test user
const baseUser = {
    password: 'Test@123',
    country: 'USA',
    agreedToTerms: true
};

// Create multiple test users with variations
const testUsers = [
    { ...baseUser, email: 'user1@test.com', role: 'user' },
    { ...baseUser, email: 'admin@test.com', role: 'admin' },
    { ...baseUser, email: 'manager@test.com', role: 'manager' }
];

describe('Multi-Role Login Tests', () => {
    testUsers.forEach(user => {
        it(`should login as ${user.role}`, () => {
            cy.visit('/register');
            
            // Use spread to fill form
            const formData = { ...user };
            Object.entries(formData).forEach(([field, value]) => {
                cy.get(`#${field}`).type(String(value));
            });
            
            cy.get('#submit').click();
            cy.get('.user-role').should('contain', user.role);
        });
    });
});
```

**Example 2: API Test Data Builder**

```javascript
// Test data builder using spread
class TestDataBuilder {
    constructor() {
        this.data = {
            name: 'Test User',
            email: 'test@example.com',
            age: 25
        };
    }

    withName(name) {
        this.data = { ...this.data, name };
        return this;
    }

    withEmail(email) {
        this.data = { ...this.data, email };
        return this;
    }

    withAge(age) {
        this.data = { ...this.data, age };
        return this;
    }

    withCustomFields(...fields) {
        fields.forEach(({ key, value }) => {
            this.data = { ...this.data, [key]: value };
        });
        return this;
    }

    build() {
        return { ...this.data };  // Return copy
    }
}

// Usage
const user1 = new TestDataBuilder()
    .withName('John Doe')
    .withEmail('john@test.com')
    .build();

const user2 = new TestDataBuilder()
    .withName('Jane Doe')
    .withCustomFields(
        { key: 'phone', value: '1234567890' },
        { key: 'address', value: '123 Main St' }
    )
    .build();

console.log(user1);  // { name: 'John Doe', email: 'john@test.com', age: 25 }
console.log(user2);  // Includes phone and address
```

**Example 3: Configuration Management**

```javascript
// Environment configurations with spread
const commonConfig = {
    timeout: 10000,
    retries: 3,
    viewportWidth: 1920,
    viewportHeight: 1080,
    video: false
};

const environments = {
    local: {
        ...commonConfig,
        baseUrl: 'http://localhost:3000',
        timeout: 5000
    },
    dev: {
        ...commonConfig,
        baseUrl: 'https://dev.example.com'
    },
    staging: {
        ...commonConfig,
        baseUrl: 'https://staging.example.com',
        video: true
    },
    production: {
        ...commonConfig,
        baseUrl: 'https://example.com',
        video: true,
        retries: 5
    }
};

// Get config for environment
function getConfig(env, ...overrides) {
    let config = { ...environments[env] };
    
    // Apply overrides
    overrides.forEach(override => {
        config = { ...config, ...override };
    });
    
    return config;
}

// Usage
const localConfig = getConfig('local');
const stagingWithCustomTimeout = getConfig('staging', { timeout: 15000 });

console.log(stagingWithCustomTimeout);
```

**Example 4: Flexible Test Utilities**

```javascript
// Utility function with rest parameters
function logTestResult(testName, status, ...metadata) {
    const timestamp = new Date().toISOString();
    console.log(`[${timestamp}] ${testName}: ${status}`);
    
    if (metadata.length > 0) {
        console.log('Metadata:', ...metadata);
    }
}

logTestResult('Login Test', 'PASSED');
logTestResult('Checkout Test', 'FAILED', 'Error: Timeout', 'Browser: Chrome', 'Retry: 1');

// Array manipulation utility
function mergeTestResults(...resultArrays) {
    return [].concat(...resultArrays);
}

const suite1Results = ['TC001-PASS', 'TC002-PASS'];
const suite2Results = ['TC003-FAIL', 'TC004-PASS'];
const suite3Results = ['TC005-PASS'];

const allResults = mergeTestResults(suite1Results, suite2Results, suite3Results);
console.log(allResults);
// ['TC001-PASS', 'TC002-PASS', 'TC003-FAIL', 'TC004-PASS', 'TC005-PASS']
```

### Common Mistakes to Avoid

**1. Confusing spread and rest**

```javascript
// WRONG - Can't use rest in middle
function test(a, ...middle, z) { }  // Error!

// CORRECT - Rest must be last
function test(a, z, ...rest) { }

// WRONG - Multiple rest parameters
function test(...args1, ...args2) { }  // Error!

// CORRECT - Only one rest parameter
function test(...args) { }
```

**2. Shallow copy issue**

```javascript
// Spread creates SHALLOW copy
const original = {
    name: 'John',
    address: {
        city: 'New York',
        country: 'USA'
    }
};

const copy = { ...original };
copy.address.city = 'Boston';  // Modifies nested object

console.log(original.address.city);  // Boston (changed!)

// CORRECT - Deep copy for nested objects
const deepCopy = {
    ...original,
    address: { ...original.address }
};

deepCopy.address.city = 'Boston';
console.log(original.address.city);  // New York (unchanged)

// Or use JSON for simple objects
const jsonCopy = JSON.parse(JSON.stringify(original));
```

**3. Order matters in spread**

```javascript
const defaults = { timeout: 5000, retries: 3 };
const custom = { timeout: 10000 };

// WRONG - defaults override custom
const config1 = { ...custom, ...defaults };
console.log(config1.timeout);  // 5000 (not what we want!)

// CORRECT - custom overrides defaults
const config2 = { ...defaults, ...custom };
console.log(config2.timeout);  // 10000 (correct!)
```

### Interview Tips - Spread and Rest Operators

**Q: What is the difference between spread and rest operators?**

**Answer:** "While they look identical (...), they serve opposite purposes:

**Spread** expands/unpacks elements:
```javascript
const arr = [1, 2, 3];
const newArr = [...arr, 4, 5];  // [1, 2, 3, 4, 5]
```

**Rest** collects/gathers elements:
```javascript
function sum(...numbers) {  // Collects all arguments
    return numbers.reduce((a, b) => a + b, 0);
}
```

In testing, spread is used for merging test data/configs, rest for flexible assertion functions."

**Q: Why use spread operator instead of Object.assign()?**

**Answer:** "Spread operator is more concise and readable:

```javascript
// Object.assign
const merged = Object.assign({}, defaults, custom);

// Spread (cleaner)
const merged = { ...defaults, ...custom };
```

Both create shallow copies. For testing, spread is preferred for clarity and works with arrays too."

**Q: What is a shallow copy and why does it matter?**

**Answer:** "Shallow copy only copies first level properties. Nested objects/arrays are still referenced:

```javascript
const original = { user: { name: 'John' } };
const copy = { ...original };
copy.user.name = 'Jane';
console.log(original.user.name);  // 'Jane' (changed!)
```

In testing, this can cause test data pollution. Solution: deep copy nested structures or use test data factories."

---

## 8. Template Literals and String Methods

### 8.1 Template Literals (Template Strings)

**Basic Template Literals:**

```javascript
// Traditional string concatenation
const name = 'John';
const age = 30;
const message1 = 'My name is ' + name + ' and I am ' + age + ' years old.';
console.log(message1);

// Template literal - cleaner syntax
const message2 = `My name is ${name} and I am ${age} years old.`;
console.log(message2);
// Output: My name is John and I am 30 years old.

// Testing scenario
const testName = 'Login Test';
const status = 'PASSED';
const duration = 2.5;

const report = `Test: ${testName} | Status: ${status} | Duration: ${duration}s`;
console.log(report);
// Output: Test: Login Test | Status: PASSED | Duration: 2.5s
```

**Multi-line Strings:**

```javascript
// Traditional - requires concatenation or escape characters
const multiline1 = 'Line 1\n' +
                   'Line 2\n' +
                   'Line 3';

// Template literal - natural line breaks
const multiline2 = `Line 1
Line 2
Line 3`;

console.log(multiline2);
// Output:
// Line 1
// Line 2
// Line 3

// Testing scenario - Test report
const testReport = `
============================================
           TEST EXECUTION REPORT
============================================
Test Suite: Login Tests
Environment: Staging
Browser: Chrome
Total Tests: 10
Passed: 8
Failed: 2
Duration: 45.3s
============================================
`;

console.log(testReport);
```

**Expressions in Template Literals:**

```javascript
const a = 10;
const b = 20;

// Any expression can be used
console.log(`Sum: ${a + b}`);           // Sum: 30
console.log(`Product: ${a * b}`);       // Product: 200
console.log(`Is a > b? ${a > b}`);      // Is a > b? false

// Function calls
function getStatus(passed) {
    return passed ? 'PASSED' : 'FAILED';
}

const testPassed = true;
console.log(`Test ${getStatus(testPassed)}`);  // Test PASSED

// Ternary operator
const count = 5;
console.log(`Found ${count} ${count === 1 ? 'item' : 'items'}`);
// Output: Found 5 items

// Testing scenario - Dynamic selectors
const userId = '12345';
const selector = `[data-user-id="${userId}"]`;
cy.get(selector).click();

const buttonType = 'submit';
const buttonSelector = `button[type="${buttonType}"]`;

// Dynamic test messages
const expected = 10;
const actual = 8;
const assertionMsg = `Expected ${expected} but got ${actual} (diff: ${expected - actual})`;
console.log(assertionMsg);
// Output: Expected 10 but got 8 (diff: 2)
```

**Nested Template Literals:**

```javascript
const users = ['Alice', 'Bob', 'Charlie'];
const userList = `Users: ${users.map(user => `"${user}"`).join(', ')}`;
console.log(userList);
// Output: Users: "Alice", "Bob", "Charlie"

// Testing scenario - HTML generation for reports
function generateTestRow(test) {
    return `
        <tr>
            <td>${test.id}</td>
            <td>${test.name}</td>
            <td class="${test.status === 'PASS' ? 'success' : 'failure'}">
                ${test.status}
            </td>
        </tr>
    `;
}

const test = { id: 'TC001', name: 'Login Test', status: 'PASS' };
console.log(generateTestRow(test));
```

### 8.2 String Methods

**Length and Access:**

```javascript
const str = 'Hello, World!';

// Length
console.log(str.length);  // 13

// Character access
console.log(str[0]);      // H
console.log(str.charAt(0));  // H
console.log(str.charAt(7));  // W

// Character code
console.log(str.charCodeAt(0));  // 72 (ASCII code of 'H')

// Testing scenario - Validate string length
function validatePassword(password) {
    if (password.length < 8) {
        return 'Password must be at least 8 characters';
    }
    if (password.length > 20) {
        return 'Password must not exceed 20 characters';
    }
    return 'Valid password length';
}

console.log(validatePassword('Test@123'));  // Valid password length
console.log(validatePassword('123'));       // Password must be at least 8 characters
```

**Case Conversion:**

```javascript
const text = 'Hello World';

console.log(text.toLowerCase());  // hello world
console.log(text.toUpperCase());  // HELLO WORLD

// Original string unchanged
console.log(text);  // Hello World

// Testing scenario - Case-insensitive comparison
function compareText(actual, expected) {
    return actual.toLowerCase() === expected.toLowerCase();
}

console.log(compareText('SUCCESS', 'success'));  // true
console.log(compareText('Error', 'ERROR'));      // true

// Normalize email for testing
const userEmail = '  TEST@EXAMPLE.COM  ';
const normalizedEmail = userEmail.trim().toLowerCase();
console.log(normalizedEmail);  // test@example.com

// Cypress example
cy.get('.status-message').then($el => {
    const statusText = $el.text().toLowerCase().trim();
    expect(statusText).to.equal('success');
});
```

**Searching Strings:**

```javascript
const message = 'The quick brown fox jumps over the lazy dog';

// includes() - Check if substring exists (case-sensitive)
console.log(message.includes('quick'));  // true
console.log(message.includes('Quick'));  // false
console.log(message.includes('cat'));    // false

// startsWith() - Check if string starts with substring
console.log(message.startsWith('The'));   // true
console.log(message.startsWith('the'));   // false

// endsWith() - Check if string ends with substring
console.log(message.endsWith('dog'));   // true
console.log(message.endsWith('Dog'));   // false

// indexOf() - Find first occurrence (returns index or -1)
console.log(message.indexOf('fox'));     // 16
console.log(message.indexOf('cat'));     // -1 (not found)
console.log(message.indexOf('o'));       // 12 (first 'o')

// lastIndexOf() - Find last occurrence
console.log(message.lastIndexOf('o'));   // 41 (last 'o')

// search() - Search with regex
console.log(message.search(/brown/));    // 10
console.log(message.search(/Brown/i));   // 10 (case-insensitive)

// Testing scenarios
const errorMessage = 'Error: Invalid username or password';

if (errorMessage.includes('Invalid')) {
    console.log('✓ Error message contains "Invalid"');
}

if (errorMessage.startsWith('Error:')) {
    console.log('✓ Error message properly formatted');
}

// Validate URL
const url = 'https://example.com/api/users';
if (url.startsWith('https://')) {
    console.log('✓ URL uses HTTPS');
}

if (url.includes('/api/')) {
    console.log('✓ URL is an API endpoint');
}

// Cypress assertion
cy.get('.error-message').then($error => {
    const errorText = $error.text();
    expect(errorText.includes('required')).to.be.true;
});
```

**Extracting Substrings:**

```javascript
const str = 'Hello, World!';

// slice(start, end) - Extract portion (end not included)
console.log(str.slice(0, 5));    // Hello
console.log(str.slice(7, 12));   // World
console.log(str.slice(7));       // World! (from index 7 to end)
console.log(str.slice(-6));      // World! (last 6 characters)
console.log(str.slice(-6, -1));  // World (negative indices)

// substring(start, end) - Similar to slice (no negative indices)
console.log(str.substring(0, 5));   // Hello
console.log(str.substring(7, 12));  // World

// substr(start, length) - Deprecated, use slice instead
console.log(str.substr(7, 5));   // World

// Testing scenarios
const testId = 'TC-2023-001';
const year = testId.slice(3, 7);
const number = testId.slice(8);
console.log(`Year: ${year}, Number: ${number}`);
// Output: Year: 2023, Number: 001

// Extract domain from email
const email = 'test@example.com';
const domain = email.slice(email.indexOf('@') + 1);
console.log(domain);  // example.com

// Extract product ID from URL
const productUrl = 'https://shop.com/products/12345';
const productId = productUrl.slice(productUrl.lastIndexOf('/') + 1);
console.log(productId);  // 12345

// Parse API response
const responseCode = 'STATUS_200_OK';
const statusCode = responseCode.slice(7, 10);
console.log(statusCode);  // 200

// Cypress example - Extract and verify order number
cy.get('.order-number').then($el => {
    const fullText = $el.text();  // "Order Number: ORD-12345"
    const orderNum = fullText.slice(fullText.indexOf('ORD-'));
    expect(orderNum).to.match(/ORD-\d{5}/);
});
```

**Splitting and Joining:**

```javascript
// split() - Split string into array
const csv = 'John,Doe,30,john@example.com';
const parts = csv.split(',');
console.log(parts);  // ['John', 'Doe', '30', 'john@example.com']

const [firstName, lastName, age, email] = csv.split(',');
console.log(firstName);  // John
console.log(email);      // john@example.com

// Split by space
const sentence = 'The quick brown fox';
const words = sentence.split(' ');
console.log(words);  // ['The', 'quick', 'brown', 'fox']

// Split into characters
const word = 'Hello';
const chars = word.split('');
console.log(chars);  // ['H', 'e', 'l', 'l', 'o']

// Limit splits
const limitedSplit = 'a-b-c-d-e'.split('-', 3);
console.log(limitedSplit);  // ['a', 'b', 'c']

// Testing scenarios
const testResult = 'TC001:PASSED:2.5s:Chrome';
const [testId, status, duration, browser] = testResult.split(':');
console.log(`Test ${testId} ${status} in ${duration} on ${browser}`);

// Parse URL path
const url = 'https://example.com/api/v1/users/123';
const pathParts = url.split('/');
const userId = pathParts[pathParts.length - 1];
console.log(userId);  // 123

// Parse CSV test data
const testData = `
user1@test.com,Password1
user2@test.com,Password2
user3@test.com,Password3
`.trim();

const users = testData.split('\n').map(line => {
    const [email, password] = line.split(',');
    return { email, password };
});

console.log(users);
// [
//   { email: 'user1@test.com', password: 'Password1' },
//   { email: 'user2@test.com', password: 'Password2' },
//   { email: 'user3@test.com', password: 'Password3' }
// ]

// Array join() - Join array into string
const arr = ['Test', 'Automation', 'Framework'];
console.log(arr.join(' '));    // Test Automation Framework
console.log(arr.join('-'));    // Test-Automation-Framework
console.log(arr.join(''));     // TestAutomationFramework

// Create test report
const results = ['10 tests', '8 passed', '2 failed'];
const report = results.join(' | ');
console.log(report);  // 10 tests | 8 passed | 2 failed
```

**Trimming:**

```javascript
const str = '   Hello, World!   ';

console.log(str.trim());       // 'Hello, World!' (both ends)
console.log(str.trimStart());  // 'Hello, World!   ' (left)
console.log(str.trimEnd());    // '   Hello, World!' (right)

// Original unchanged
console.log(str);  // '   Hello, World!   '

// Testing scenarios - Clean user input
function cleanInput(input) {
    return input.trim().toLowerCase();
}

const userInput = '  TEST@EXAMPLE.COM  ';
const cleaned = cleanInput(userInput);
console.log(cleaned);  // test@example.com

// Validate form data
function validateFormField(value) {
    const trimmed = value.trim();
    
    if (trimmed === '') {
        return 'Field cannot be empty';
    }
    
    if (trimmed.length < 3) {
        return 'Minimum 3 characters required';
    }
    
    return 'Valid';
}

console.log(validateFormField('   '));      // Field cannot be empty
console.log(validateFormField('  AB  '));   // Minimum 3 characters required
console.log(validateFormField('  Test  ')); // Valid

// Cypress example
cy.get('#email').invoke('val').then(value => {
    const trimmedValue = value.trim();
    expect(trimmedValue).to.not.be.empty;
});
```

**Replacing:**

```javascript
const str = 'Hello World, Hello Universe';

// replace() - Replace first occurrence
console.log(str.replace('Hello', 'Hi'));
// Output: Hi World, Hello Universe

// replaceAll() - Replace all occurrences
console.log(str.replaceAll('Hello', 'Hi'));
// Output: Hi World, Hi Universe

// With regex - global flag
console.log(str.replace(/Hello/g, 'Hi'));
// Output: Hi World, Hi Universe

// Case-insensitive replace
const text = 'JavaScript is great. javascript is powerful.';
console.log(text.replace(/javascript/gi, 'JS'));
// Output: JS is great. JS is powerful.

// Testing scenarios
// Clean test data
const testEmail = 'test+user@example.com';
const cleanEmail = testEmail.replace('+', '');
console.log(cleanEmail);  // testuser@example.com

// Sanitize input
function sanitizeFilename(filename) {
    return filename.replace(/[^a-zA-Z0-9.-]/g, '_');
}

console.log(sanitizeFilename('test file (1).txt'));  // test_file__1_.txt

// Replace placeholders
const template = 'Hello {{name}}, your order {{orderId}} is ready!';
const message = template
    .replace('{{name}}', 'John')
    .replace('{{orderId}}', 'ORD-12345');
console.log(message);
// Output: Hello John, your order ORD-12345 is ready!

// Dynamic URL generation
function generateUrl(template, params) {
    let url = template;
    for (const [key, value] of Object.entries(params)) {
        url = url.replace(`{${key}}`, value);
    }
    return url;
}

const urlTemplate = 'https://api.example.com/{version}/users/{userId}';
const finalUrl = generateUrl(urlTemplate, { version: 'v1', userId: '123' });
console.log(finalUrl);  // https://api.example.com/v1/users/123
```

**Padding:**

```javascript
// padStart(length, fillString) - Pad beginning
console.log('5'.padStart(3, '0'));      // 005
console.log('42'.padStart(5, '0'));     // 00042
console.log('test'.padStart(10, '*'));  // ******test

// padEnd(length, fillString) - Pad end
console.log('5'.padEnd(3, '0'));       // 500
console.log('test'.padEnd(10, '.'));   // test......

// Testing scenarios
// Format test IDs
function formatTestId(id) {
    return `TC-${String(id).padStart(4, '0')}`;
}

console.log(formatTestId(1));     // TC-0001
console.log(formatTestId(42));    // TC-0042
console.log(formatTestId(999));   // TC-0999

// Format duration
function formatDuration(seconds) {
    const mins = Math.floor(seconds / 60);
    const secs = seconds % 60;
    return `${String(mins).padStart(2, '0')}:${String(secs).padStart(2, '0')}`;
}

console.log(formatDuration(65));   // 01:05
console.log(formatDuration(125));  // 02:05
console.log(formatDuration(5));    // 00:05

// Align test results
const tests = [
    { name: 'Login', status: 'PASS' },
    { name: 'Search Products', status: 'FAIL' },
    { name: 'Checkout', status: 'PASS' }
];

tests.forEach(test => {
    const name = test.name.padEnd(20, '.');
    console.log(`${name} ${test.status}`);
});
// Output:
// Login............... PASS
// Search Products..... FAIL
// Checkout............ PASS
```

**Repeating:**

```javascript
// repeat(count) - Repeat string
console.log('*'.repeat(10));        // **********
console.log('abc'.repeat(3));       // abcabcabc
console.log('- '.repeat(5));        // - - - - - 

// Testing scenarios
// Generate separator
function printSeparator(char = '=', length = 50) {
    console.log(char.repeat(length));
}

printSeparator();           // ==================================================
printSeparator('-', 30);    // ------------------------------

// Generate test report header
console.log('='.repeat(60));
console.log('TEST EXECUTION REPORT'.padStart(40));
console.log('='.repeat(60));

// Progress indicator
function showProgress(completed, total) {
    const percentage = Math.floor((completed / total) * 100);
    const filled = '█'.repeat(percentage / 2);
    const empty = '░'.repeat(50 - (percentage / 2));
    console.log(`[${filled}${empty}] ${percentage}%`);
}

showProgress(25, 100);  // [████████████░░░░░░░░░░░░░░░░░░░░░░░░░░░░] 25%
showProgress(75, 100);  // [█████████████████████████████████████░░░░░] 75%
```

**Match and Regular Expressions:**

```javascript
const str = 'Contact us at support@example.com or sales@example.com';

// match() - Find matches
const emails = str.match(/[\w.-]+@[\w.-]+\.\w+/g);
console.log(emails);  // ['support@example.com', 'sales@example.com']

// matchAll() - Get detailed matches
const text = 'Test1: PASS, Test2: FAIL, Test3: PASS';
const matches = [...text.matchAll(/Test(\d+): (\w+)/g)];

matches.forEach(match => {
    console.log(`${match[0]} - Number: ${match[1]}, Status: ${match[2]}`);
});
// Output:
// Test1: PASS - Number: 1, Status: PASS
// Test2: FAIL - Number: 2, Status: FAIL
// Test3: PASS - Number: 3, Status: PASS

// Testing scenarios
// Extract all URLs from text
const content = 'Visit https://example.com or http://test.com for more';
const urls = content.match(/https?:\/\/[^\s]+/g);
console.log(urls);  // ['https://example.com', 'http://test.com']

// Validate email format
function isValidEmail(email) {
    const emailPattern = /^[\w.-]+@[\w.-]+\.\w+$/;
    return emailPattern.test(email);
}

console.log(isValidEmail('test@example.com'));  // true
console.log(isValidEmail('invalid-email'));     // false

// Extract test IDs
const report = 'Executed: TC001, TC002, TC005, TC010';
const testIds = report.match(/TC\d+/g);
console.log(testIds);  // ['TC001', 'TC002', 'TC005', 'TC010']
```

### 8.3 Real-World Testing Examples

**Example 1: Test Data Sanitization**

```javascript
class TestDataSanitizer {
    static sanitizeEmail(email) {
        return email.trim().toLowerCase();
    }

    static sanitizeUsername(username) {
        return username.trim().replace(/[^a-zA-Z0-9_]/g, '');
    }

    static sanitizePhone(phone) {
        return phone.replace(/[^0-9]/g, '');
    }

    static maskSensitive(text, visibleChars = 4) {
        if (text.length <= visibleChars) return text;
        const visible = text.slice(-visibleChars);
        const masked = '*'.repeat(text.length - visibleChars);
        return masked + visible;
    }
}

// Usage
console.log(TestDataSanitizer.sanitizeEmail('  TEST@EXAMPLE.COM  '));
// test@example.com

console.log(TestDataSanitizer.sanitizeUsername('test_user#123!'));
// test_user123

console.log(TestDataSanitizer.sanitizePhone('(123) 456-7890'));
// 1234567890

console.log(TestDataSanitizer.maskSensitive('1234567890'));
// ******7890
```

**Example 2: Dynamic Selector Builder**

```javascript
class SelectorBuilder {
    static byId(id) {
        return `#${id}`;
    }

    static byClass(className) {
        return `.${className}`;
    }

    static byAttribute(attr, value) {
        return `[${attr}="${value}"]`;
    }

    static byTestId(testId) {
        return `[data-testid="${testId}"]`;
    }

    static combine(...selectors) {
        return selectors.join(' ');
    }

    static withText(selector, text) {
        return `${selector}:contains("${text}")`;
    }
}

// Usage in Cypress
cy.get(SelectorBuilder.byTestId('login-form'));
cy.get(SelectorBuilder.byAttribute('type', 'submit'));
cy.get(SelectorBuilder.combine('.modal', SelectorBuilder.byClass('close-btn')));
```

**Example 3: Test Report Generator**

```javascript
class TestReportGenerator {
    static generateSummary(results) {
        const total = results.length;
        const passed = results.filter(r => r.status === 'PASS').length;
        const failed = total - passed;
        const passRate = ((passed / total) * 100).toFixed(2);

        return `
${'='.repeat(50)}
           TEST EXECUTION SUMMARY
${'='.repeat(50)}
Total Tests:      ${String(total).padStart(5)}
Passed:           ${String(passed).padStart(5)} (${passRate}%)
Failed:           ${String(failed).padStart(5)}
${'='.repeat(50)}
        `;
    }

    static generateDetailedReport(results) {
        let report = '\nDetailed Results:\n';
        report += '-'.repeat(70) + '\n';
        report += `${'Test ID'.padEnd(15)} | ${'Test Name'.padEnd(30)} | ${'Status'.padEnd(10)} | Duration\n`;
        report += '-'.repeat(70) + '\n';

        results.forEach(result => {
            const id = result.id.padEnd(15);
            const name = result.name.padEnd(30);
            const status = result.status.padEnd(10);
            const duration = `${result.duration}s`;
            report += `${id} | ${name} | ${status} | ${duration}\n`;
        });

        report += '-'.repeat(70) + '\n';
        return report;
    }
}

// Usage
const testResults = [
    { id: 'TC001', name: 'Login Test', status: 'PASS', duration: 2.5 },
    { id: 'TC002', name: 'Search Products', status: 'FAIL', duration: 3.2 },
    { id: 'TC003', name: 'Add to Cart', status: 'PASS', duration: 1.8 }
];

console.log(TestReportGenerator.generateSummary(testResults));
console.log(TestReportGenerator.generateDetailedReport(testResults));
```

### Common Mistakes to Avoid

**1. Forgetting strings are immutable**

```javascript
// WRONG - Strings are immutable
const str = 'hello';
str[0] = 'H';  // Doesn't work!
console.log(str);  // still 'hello'

// CORRECT - Create new string
const newStr = 'H' + str.slice(1);
console.log(newStr);  // Hello
```

**2. Not handling null/undefined**

```javascript
// WRONG - Can cause errors
function processText(text) {
    return text.trim().toLowerCase();  // Error if text is null/undefined
}

// CORRECT - Validate input
function processText(text) {
    if (!text) return '';
    return text.trim().toLowerCase();
}

// Or use optional chaining and nullish coalescing
function processText(text) {
    return (text?.trim()?.toLowerCase()) ?? '';
}
```

**3. Case-sensitive comparisons**

```javascript
// WRONG - Case-sensitive
if (status === 'success') { }  // Fails if status is 'SUCCESS'

// CORRECT - Normalize case
if (status.toLowerCase() === 'success') { }
```

### Interview Tips - Template Literals and Strings

**Q: What are template literals and their advantages?**

**Answer:** "Template literals (backticks) provide cleaner string interpolation and multi-line strings:

```javascript
// Old way
const msg = 'Hello ' + name + ', you have ' + count + ' messages';

// Template literal
const msg = `Hello ${name}, you have ${count} messages`;
```

Advantages:
1. Cleaner syntax with ${} for variables
2. Natural multi-line strings
3. Expression evaluation
4. No + concatenation needed

Essential for dynamic selectors and test messages in automation."

**Q: Explain the difference between split() and slice()**

**Answer:** "Completely different purposes:

**split()** - Converts string to array:
```javascript
'a,b,c'.split(',')  // ['a', 'b', 'c']
```

**slice()** - Extracts substring:
```javascript
'hello'.slice(1, 4)  // 'ell'
```

In testing, split() parses CSV data, slice() extracts portions like IDs from URLs."

---

## 9. Advanced Array Methods

### 9.1 map() - Transform Arrays

**Basic map():**

```javascript
// Transform each element
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map(num => num * 2);
console.log(doubled);  // [2, 4, 6, 8, 10]

// Original unchanged
console.log(numbers);  // [1, 2, 3, 4, 5]

// With index and array parameters
const withIndex = numbers.map((num, index, arr) => {
    return {
        value: num,
        index: index,
        isLast: index === arr.length - 1
    };
});

console.log(withIndex);
// [
//   { value: 1, index: 0, isLast: false },
//   { value: 2, index: 1, isLast: false },
//   ...
// ]

// Testing scenarios
const testIds = [1, 2, 3, 4, 5];
const formattedIds = testIds.map(id => `TC-${String(id).padStart(3, '0')}`);
console.log(formattedIds);
// ['TC-001', 'TC-002', 'TC-003', 'TC-004', 'TC-005']

// Extract specific properties
const users = [
    { id: 1, name: 'John', email: 'john@test.com' },
    { id: 2, name: 'Jane', email: 'jane@test.com' },
    { id: 3, name: 'Bob', email: 'bob@test.com' }
];

const userNames = users.map(user => user.name);
console.log(userNames);  // ['John', 'Jane', 'Bob']

const userEmails = users.map(user => user.email);
console.log(userEmails);  // ['john@test.com', 'jane@test.com', 'bob@test.com']

// Transform test data
const rawTestData = [
    'user1@test.com|Password1',
    'user2@test.com|Password2',
    'user3@test.com|Password3'
];

const testUsers = rawTestData.map(line => {
    const [email, password] = line.split('|');
    return { email, password };
});

console.log(testUsers);
// [
//   { email: 'user1@test.com', password: 'Password1' },
//   { email: 'user2@test.com', password: 'Password2' },
//   { email: 'user3@test.com', password: 'Password3' }
// ]

// Cypress example - Test multiple URLs
const paths = ['/home', '/products', '/about', '/contact'];
const fullUrls = paths.map(path => `https://example.com${path}`);

fullUrls.forEach(url => {
    cy.visit(url);
    cy.get('h1').should('be.visible');
});
```

**Advanced map() examples:**

```javascript
// API response transformation
const apiResponse = {
    data: [
        { productId: 101, productName: 'Laptop', price: 999.99 },
        { productId: 102, productName: 'Mouse', price: 29.99 },
        { productId: 103, productName: 'Keyboard', price: 79.99 }
    ]
};

const products = apiResponse.data.map(item => ({
    id: item.productId,
    name: item.productName,
    price: item.price,
    formattedPrice: `$${item.price.toFixed(2)}`
}));

console.log(products);

// Calculate derived values
const testResults = [
    { name: 'Test 1', duration: 2.5 },
    { name: 'Test 2', duration: 3.7 },
    { name: 'Test 3', duration: 1.2 }
];

const withDurationInMs = testResults.map(test => ({
    ...test,
    durationMs: test.duration * 1000,
    durationFormatted: `${test.duration}s`
}));

console.log(withDurationInMs);
```

### 9.2 filter() - Filter Arrays

**Basic filter():**

```javascript
// Filter elements that match condition
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

const evenNumbers = numbers.filter(num => num % 2 === 0);
console.log(evenNumbers);  // [2, 4, 6, 8, 10]

const greaterThanFive = numbers.filter(num => num > 5);
console.log(greaterThanFive);  // [6, 7, 8, 9, 10]

// With index
const firstThreeEven = numbers.filter((num, index) => num % 2 === 0 && index < 6);
console.log(firstThreeEven);  // [2, 4, 6]

// Testing scenarios
const testResults = [
    { id: 'TC001', name: 'Login Test', status: 'PASS', duration: 2.5 },
    { id: 'TC002', name: 'Search Test', status: 'FAIL', duration: 3.2 },
    { id: 'TC003', name: 'Checkout Test', status: 'PASS', duration: 4.1 },
    { id: 'TC004', name: 'Payment Test', status: 'FAIL', duration: 2.8 },
    { id: 'TC005', name: 'Logout Test', status: 'PASS', duration: 1.5 }
];

// Get failed tests
const failedTests = testResults.filter(test => test.status === 'FAIL');
console.log(failedTests);
// [
//   { id: 'TC002', name: 'Search Test', status: 'FAIL', duration: 3.2 },
//   { id: 'TC004', name: 'Payment Test', status: 'FAIL', duration: 2.8 }
// ]

// Get slow tests (> 3 seconds)
const slowTests = testResults.filter(test => test.duration > 3);
console.log(slowTests);

// Complex filtering
const criticalFailures = testResults.filter(test =>
    test.status === 'FAIL' &&
    test.name.includes('Payment')
);

// Filter users by role
const users = [
    { id: 1, username: 'admin1', role: 'admin', isActive: true },
    { id: 2, username: 'user1', role: 'user', isActive: true },
    { id: 3, username: 'admin2', role: 'admin', isActive: false },
    { id: 4, username: 'user2', role: 'user', isActive: true }
];

const activeAdmins = users.filter(user =>
    user.role === 'admin' && user.isActive
);
console.log(activeAdmins);

// Remove nullish values
const mixedArray = [1, null, 2, undefined, 3, 0, '', 4, false, 5];
const truthyValues = mixedArray.filter(Boolean);
console.log(truthyValues);  // [1, 2, 3, 4, 5]

const definedValues = mixedArray.filter(val => val !== null && val !== undefined);
console.log(definedValues);  // [1, 2, 3, 0, '', 4, false, 5]

// Cypress example - Filter visible elements
cy.get('.product-card').then($cards => {
    const visibleCards = [...$cards].filter(card =>
        Cypress.$(card).is(':visible')
    );
    expect(visibleCards.length).to.be.greaterThan(0);
});
```

**Advanced filter() examples:**

```javascript
// API response filtering
async function getActiveProducts() {
    const response = await fetch('https://api.example.com/products');
    const products = await response.json();
    
    // Filter active products in stock
    const available = products.filter(p =>
        p.isActive &&
        p.stock > 0 &&
        p.price > 0
    );
    
    return available;
}

// Multi-condition filtering
const testCases = [
    { id: 'TC001', priority: 'P0', automated: true, status: 'pass' },
    { id: 'TC002', priority: 'P1', automated: false, status: 'pass' },
    { id: 'TC003', priority: 'P0', automated: true, status: 'fail' },
    { id: 'TC004', priority: 'P2', automated: true, status: 'pass' }
];

const criticalAutomatedFailures = testCases.filter(tc =>
    tc.priority === 'P0' &&
    tc.automated &&
    tc.status === 'fail'
);

console.log(criticalAutomatedFailures);

// Filter by date range
const testExecutions = [
    { date: '2024-01-15', passed: 45, failed: 5 },
    { date: '2024-01-16', passed: 47, failed: 3 },
    { date: '2024-01-17', passed: 43, failed: 7 }
];

const successfulDays = testExecutions.filter(exec =>
    (exec.passed / (exec.passed + exec.failed)) > 0.9
);
```

### 9.3 reduce() - Accumulate Values

**Basic reduce():**

```javascript
// Sum all numbers
const numbers = [1, 2, 3, 4, 5];
const sum = numbers.reduce((accumulator, current) => accumulator + current, 0);
console.log(sum);  // 15

// Step by step:
// accumulator = 0 (initial), current = 1 → return 0 + 1 = 1
// accumulator = 1, current = 2 → return 1 + 2 = 3
// accumulator = 3, current = 3 → return 3 + 3 = 6
// accumulator = 6, current = 4 → return 6 + 4 = 10
// accumulator = 10, current = 5 → return 10 + 5 = 15

// Product
const product = numbers.reduce((acc, curr) => acc * curr, 1);
console.log(product);  // 120

// Maximum value
const max = numbers.reduce((max, curr) => curr > max ? curr : max, numbers[0]);
console.log(max);  // 5

// Testing scenarios
const testResults = [
    { name: 'Test 1', duration: 2.5, passed: true },
    { name: 'Test 2', duration: 3.2, passed: false },
    { name: 'Test 3', duration: 1.8, passed: true },
    { name: 'Test 4', duration: 4.1, passed: true }
];

// Total duration
const totalDuration = testResults.reduce((sum, test) => sum + test.duration, 0);
console.log(`Total: ${totalDuration}s`);  // Total: 11.6s

// Count passed tests
const passedCount = testResults.reduce((count, test) =>
    test.passed ? count + 1 : count, 0
);
console.log(`Passed: ${passedCount}`);  // Passed: 3

// Average duration
const avgDuration = testResults.reduce((sum, test) => sum + test.duration, 0) / testResults.length;
console.log(`Average: ${avgDuration.toFixed(2)}s`);  // Average: 2.90s
```

**Advanced reduce() examples:**

```javascript
// Group by property
const tests = [
    { id: 'TC001', suite: 'Login', status: 'pass' },
    { id: 'TC002', suite: 'Login', status: 'fail' },
    { id: 'TC003', suite: 'Checkout', status: 'pass' },
    { id: 'TC004', suite: 'Checkout', status: 'pass' },
    { id: 'TC005', suite: 'Login', status: 'pass' }
];

const groupedBySuite = tests.reduce((groups, test) => {
    const suite = test.suite;
    if (!groups[suite]) {
        groups[suite] = [];
    }
    groups[suite].push(test);
    return groups;
}, {});

console.log(groupedBySuite);
// {
//   Login: [TC001, TC002, TC005],
//   Checkout: [TC003, TC004]
// }

// Count occurrences
const statuses = ['pass', 'fail', 'pass', 'pass', 'fail', 'skip', 'pass'];
const statusCount = statuses.reduce((counts, status) => {
    counts[status] = (counts[status] || 0) + 1;
    return counts;
}, {});

console.log(statusCount);
// { pass: 4, fail: 2, skip: 1 }

// Build object from array
const users = [
    { id: 1, name: 'John' },
    { id: 2, name: 'Jane' },
    { id: 3, name: 'Bob' }
];

const usersById = users.reduce((obj, user) => {
    obj[user.id] = user;
    return obj;
}, {});

console.log(usersById);
// {
//   '1': { id: 1, name: 'John' },
//   '2': { id: 2, name: 'Jane' },
//   '3': { id: 3, name: 'Bob' }
// }

// Flatten nested arrays
const nested = [[1, 2], [3, 4], [5, 6]];
const flattened = nested.reduce((flat, arr) => flat.concat(arr), []);
console.log(flattened);  // [1, 2, 3, 4, 5, 6]

// Or use flat() (modern approach)
const flattened2 = nested.flat();
console.log(flattened2);  // [1, 2, 3, 4, 5, 6]

// Calculate test statistics
const executionResults = [
    { suite: 'Login', passed: 5, failed: 1, skipped: 0 },
    { suite: 'Search', passed: 8, failed: 2, skipped: 1 },
    { suite: 'Checkout', passed: 6, failed: 0, skipped: 0 }
];

const stats = executionResults.reduce((total, suite) => ({
    totalPassed: total.totalPassed + suite.passed,
    totalFailed: total.totalFailed + suite.failed,
    totalSkipped: total.totalSkipped + suite.skipped,
    totalTests: total.totalTests + suite.passed + suite.failed + suite.skipped
}), { totalPassed: 0, totalFailed: 0, totalSkipped: 0, totalTests: 0 });

console.log(stats);
// { totalPassed: 19, totalFailed: 3, totalSkipped: 1, totalTests: 23 }
```

### 9.4 find() and findIndex()

**find() - Find first match:**

```javascript
const users = [
    { id: 1, username: 'john', role: 'user' },
    { id: 2, username: 'admin', role: 'admin' },
    { id: 3, username: 'jane', role: 'user' }
];

// Find first admin
const admin = users.find(user => user.role === 'admin');
console.log(admin);  // { id: 2, username: 'admin', role: 'admin' }

// Find by ID
const user = users.find(u => u.id === 3);
console.log(user);  // { id: 3, username: 'jane', role: 'user' }

// Not found returns undefined
const notFound = users.find(u => u.id === 999);
console.log(notFound);  // undefined

// Testing scenarios
const testCases = [
    { id: 'TC001', name: 'Login Test', priority: 'P0' },
    { id: 'TC002', name: 'Search Test', priority: 'P1' },
    { id: 'TC003', name: 'Checkout Test', priority: 'P0' }
];

// Find specific test
const loginTest = testCases.find(tc => tc.id === 'TC001');
console.log(loginTest.name);  // Login Test

// Find first P0 test
const criticalTest = testCases.find(tc => tc.priority === 'P0');
console.log(criticalTest);

// Cypress example
cy.get('.product-card').then($cards => {
    const cards = [...$cards];
    const laptopCard = cards.find(card =>
        Cypress.$(card).find('.product-name').text().includes('Laptop')
    );
    if (laptopCard) {
        cy.wrap(laptopCard).click();
    }
});
```

**findIndex() - Find index of first match:**

```javascript
const numbers = [10, 20, 30, 40, 50];

// Find index of first number > 25
const index = numbers.findIndex(num => num > 25);
console.log(index);  // 2 (value is 30)

// Not found returns -1
const notFoundIndex = numbers.findIndex(num => num > 100);
console.log(notFoundIndex);  // -1

// Testing scenario
const testQueue = [
    { id: 'TC001', status: 'pending' },
    { id: 'TC002', status: 'running' },
    { id: 'TC003', status: 'pending' }
];

// Find index of running test
const runningIndex = testQueue.findIndex(t => t.status === 'running');
console.log(`Running test at index: ${runningIndex}`);  // 1

// Update specific test
if (runningIndex !== -1) {
    testQueue[runningIndex].status = 'completed';
}
```

### 9.5 some() and every()

**some() - Check if any element matches:**

```javascript
const numbers = [1, 2, 3, 4, 5];

// Check if any number is even
const hasEven = numbers.some(num => num % 2 === 0);
console.log(hasEven);  // true

// Check if any number > 10
const hasLarge = numbers.some(num => num > 10);
console.log(hasLarge);  // false

// Testing scenarios
const testResults = [
    { name: 'Test 1', status: 'PASS' },
    { name: 'Test 2', status: 'PASS' },
    { name: 'Test 3', status: 'FAIL' }
];

// Check if any test failed
const hasFailures = testResults.some(test => test.status === 'FAIL');
console.log(hasFailures);  // true

if (hasFailures) {
    console.log('⚠ Some tests failed!');
}

// Check if any user is admin
const users = [
    { name: 'John', role: 'user' },
    { name: 'Jane', role: 'admin' },
    { name: 'Bob', role: 'user' }
];

const hasAdmin = users.some(user => user.role === 'admin');
console.log(hasAdmin);  // true

// Validate form has errors
const formFields = [
    { name: 'email', value: 'test@test.com', valid: true },
    { name: 'password', value: '', valid: false },
    { name: 'name', value: 'John', valid: true }
];

const hasErrors = formFields.some(field => !field.valid);
console.log(`Form has errors: ${hasErrors}`);  // true
```

**every() - Check if all elements match:**

```javascript
const numbers = [2, 4, 6, 8, 10];

// Check if all numbers are even
const allEven = numbers.every(num => num % 2 === 0);
console.log(allEven);  // true

// Check if all numbers > 5
const allLarge = numbers.every(num => num > 5);
console.log(allLarge);  // false

// Testing scenarios
const testResults = [
    { name: 'Test 1', status: 'PASS' },
    { name: 'Test 2', status: 'PASS' },
    { name: 'Test 3', status: 'PASS' }
];

// Check if all tests passed
const allPassed = testResults.every(test => test.status === 'PASS');
console.log(allPassed);  // true

if (allPassed) {
    console.log('✓ All tests passed!');
}

// Validate all required fields filled
const formData = {
    username: 'testuser',
    email: 'test@test.com',
    password: 'Test@123'
};

const requiredFields = ['username', 'email', 'password'];
const allFieldsFilled = requiredFields.every(field =>
    formData[field] && formData[field].trim() !== ''
);

console.log(`All fields filled: ${allFieldsFilled}`);  // true

// API response validation
const products = [
    { id: 1, name: 'Product 1', price: 100 },
    { id: 2, name: 'Product 2', price: 200 },
    { id: 3, name: 'Product 3', price: 300 }
];

const allProductsValid = products.every(p =>
    p.id &&
    p.name &&
    typeof p.price === 'number' &&
    p.price > 0
);

console.log(`All products valid: ${allProductsValid}`);  // true
```

### 9.6 forEach() vs map()

**Comparison:**

```javascript
const numbers = [1, 2, 3, 4, 5];

// forEach - For side effects, returns undefined
numbers.forEach(num => {
    console.log(num * 2);
});
// Output: 2 4 6 8 10

const result1 = numbers.forEach(num => num * 2);
console.log(result1);  // undefined

// map - For transformation, returns new array
const doubled = numbers.map(num => num * 2);
console.log(doubled);  // [2, 4, 6, 8, 10]

// Use forEach when:
// - Performing side effects (logging, DOM manipulation, API calls)
// - Don't need return value

// Use map when:
// - Transforming data
// - Need new array with results

// Testing examples
const testIds = ['TC001', 'TC002', 'TC003'];

// forEach - Execute tests (side effect)
testIds.forEach(id => {
    console.log(`Executing test: ${id}`);
    // Execute test logic
});

// map - Transform test data
const testObjects = testIds.map(id => ({
    id: id,
    status: 'pending',
    startTime: Date.now()
}));
```

### 9.7 Chaining Array Methods

**Powerful combinations:**

```javascript
const testResults = [
    { id: 'TC001', status: 'PASS', duration: 2.5, suite: 'Login' },
    { id: 'TC002', status: 'FAIL', duration: 3.2, suite: 'Login' },
    { id: 'TC003', status: 'PASS', duration: 4.1, suite: 'Checkout' },
    { id: 'TC004', status: 'PASS', duration: 1.8, suite: 'Checkout' },
    { id: 'TC005', status: 'FAIL', duration: 2.9, suite: 'Search' }
];

// Chain: filter, map, reduce
const totalPassedDuration = testResults
    .filter(test => test.status === 'PASS')        // Get passed tests
    .map(test => test.duration)                     // Extract durations
    .reduce((sum, duration) => sum + duration, 0);  // Sum them

console.log(`Total passed duration: ${totalPassedDuration}s`);

// Get names of failed tests
const failedTestNames = testResults
    .filter(test => test.status === 'FAIL')
    .map(test => test.name);

// Count tests by suite
const testsBySuite = testResults
    .filter(test => test.status === 'PASS')
    .reduce((counts, test) => {
        counts[test.suite] = (counts[test.suite] || 0) + 1;
        return counts;
    }, {});

console.log(testsBySuite);
// { Login: 1, Checkout: 2 }

// Complex transformation
const summary = testResults
    .filter(test => test.duration > 2)              // Slow tests only
    .map(test => ({
        ...test,
        performance: test.duration > 3 ? 'slow' : 'moderate'
    }))
    .sort((a, b) => b.duration - a.duration);       // Sort by duration

console.log(summary);
```

### 9.8 Real-World Testing Examples

**Example 1: Test Result Analysis**

```javascript
class TestResultAnalyzer {
    constructor(results) {
        this.results = results;
    }

    getTotalDuration() {
        return this.results
            .map(test => test.duration)
            .reduce((sum, duration) => sum + duration, 0);
    }

    getPassRate() {
        const total = this.results.length;
        const passed = this.results.filter(t => t.status === 'PASS').length;
        return ((passed / total) * 100).toFixed(2);
    }

    getSlowTests(threshold = 3) {
        return this.results.filter(test => test.duration > threshold);
    }

    getFailedTests() {
        return this.results
            .filter(test => test.status === 'FAIL')
            .map(test => ({
                id: test.id,
                name: test.name,
                duration: test.duration,
                error: test.error || 'No error message'
            }));
    }

    getStatsBySuite() {
        return this.results.reduce((stats, test) => {
            const suite = test.suite;
            if (!stats[suite]) {
                stats[suite] = { total: 0, passed: 0, failed: 0 };
            }
            stats[suite].total++;
            if (test.status === 'PASS') stats[suite].passed++;
            if (test.status === 'FAIL') stats[suite].failed++;
            return stats;
        }, {});
    }
}

// Usage
const results = [
    { id: 'TC001', name: 'Login', status: 'PASS', duration: 2.5, suite: 'Auth' },
    { id: 'TC002', name: 'Logout', status: 'PASS', duration: 1.2, suite: 'Auth' },
    { id: 'TC003', name: 'Search', status: 'FAIL', duration: 4.3, suite: 'Search' }
];

const analyzer = new TestResultAnalyzer(results);
console.log('Total Duration:', analyzer.getTotalDuration());
console.log('Pass Rate:', analyzer.getPassRate());
console.log('Slow Tests:', analyzer.getSlowTests());
console.log('Stats:', analyzer.getStatsBySuite());
```

**Example 2: API Response Validator**

```javascript
class APIResponseValidator {
    static validateProducts(products) {
        // Check if array
        if (!Array.isArray(products)) {
            return { valid: false, error: 'Response is not an array' };
        }

        // Find invalid products
        const invalid = products.filter(p =>
            !p.id ||
            !p.name ||
            typeof p.price !== 'number' ||
            p.price < 0
        );

        if (invalid.length > 0) {
            return {
                valid: false,
                error: `${invalid.length} invalid products found`,
                invalidProducts: invalid
            };
        }

        return { valid: true, count: products.length };
    }

    static getOutOfStockProducts(products) {
        return products.filter(p => p.stock === 0 || p.stock < 0);
    }

    static getActiveProducts(products) {
        return products.filter(p => p.isActive && p.stock > 0);
    }

    static getAveragePrice(products) {
        const total = products.reduce((sum, p) => sum + p.price, 0);
        return (total / products.length).toFixed(2);
    }
}

// Usage
const products = [
    { id: 1, name: 'Laptop', price: 999, stock: 5, isActive: true },
    { id: 2, name: 'Mouse', price: 29, stock: 0, isActive: true },
    { id: 3, name: 'Keyboard', price: 79, stock: 10, isActive: false }
];

const validation = APIResponseValidator.validateProducts(products);
console.log('Validation:', validation);

const outOfStock = APIResponseValidator.getOutOfStockProducts(products);
console.log('Out of stock:', outOfStock);

const avgPrice = APIResponseValidator.getAveragePrice(products);
console.log('Average price:', avgPrice);
```

### Common Mistakes to Avoid

**1. Mutating original array**

```javascript
// WRONG - sort() mutates original
const numbers = [3, 1, 4, 1, 5];
numbers.sort();  // Modifies numbers!
console.log(numbers);  // [1, 1, 3, 4, 5]

// CORRECT - Create copy first
const sorted = [...numbers].sort();
// Or
const sorted2 = numbers.slice().sort();
```

**2. Forgetting to return in map()**

```javascript
// WRONG - No return
const doubled = numbers.map(num => {
    num * 2;  // Missing return!
});
console.log(doubled);  // [undefined, undefined, ...]

// CORRECT
const doubled = numbers.map(num => num * 2);
// Or with braces
const doubled2 = numbers.map(num => {
    return num * 2;
});
```

**3. Using forEach() when map() is needed**

```javascript
// WRONG - forEach returns undefined
const doubled = numbers.forEach(num => num * 2);
console.log(doubled);  // undefined

// CORRECT - Use map
const doubled = numbers.map(num => num * 2);
```

### Interview Tips - Array Methods

**Q: Explain the difference between map() and forEach()**

**Answer:** "Both iterate arrays, but:

**map()** returns new array:
```javascript
const doubled = [1, 2, 3].map(n => n * 2);  // [2, 4, 6]
```

**forEach()** returns undefined, used for side effects:
```javascript
[1, 2, 3].forEach(n => console.log(n));  // undefined
```

Use map() for transformation, forEach() for iteration without return value. In testing, map() transforms test data, forEach() executes tests."

**Q: When to use filter(), find(), and some()?**

**Answer:**
- **filter()**: Get all matching elements → array
- **find()**: Get first match → single element or undefined
- **some()**: Check if any match → boolean

```javascript
const tests = [/*...*/];

filter(t => t.status === 'FAIL')  // All failed tests
find(t => t.status === 'FAIL')    // First failed test
some(t => t.status === 'FAIL')    // Any failures? true/false
```

**Q: Explain reduce() with an example**

**Answer:** "reduce() accumulates array into single value:

```javascript
// Sum
[1, 2, 3].reduce((sum, num) => sum + num, 0)  // 6

// Group by
tests.reduce((groups, test) => {
    groups[test.suite] = groups[test.suite] || [];
    groups[test.suite].push(test);
    return groups;
}, {})
```

Common in testing for aggregating results, counting, grouping data."

---

## 10. Asynchronous JavaScript

### 10.1 Understanding Asynchronous Programming

**Synchronous vs Asynchronous:**

```javascript
// SYNCHRONOUS - Blocking (waits for completion)
console.log('Start');
console.log('Middle');
console.log('End');
// Output: Start Middle End (in order)

// ASYNCHRONOUS - Non-blocking (doesn't wait)
console.log('Start');

setTimeout(() => {
    console.log('Async operation');
}, 1000);

console.log('End');
// Output: Start End Async operation (not in order!)

// Why? setTimeout is async - doesn't block execution
```

**The Event Loop:**

```javascript
console.log('1');

setTimeout(() => {
    console.log('2');
}, 0);  // Even with 0ms delay!

console.log('3');

// Output: 1 3 2

// Explanation:
// 1. console.log('1') - executes immediately
// 2. setTimeout - added to task queue
// 3. console.log('3') - executes immediately
// 4. Event loop picks setTimeout from queue
// 5. console.log('2') - executes
```

### 10.2 Callbacks

**Basic Callbacks:**

```javascript
// Function that accepts callback
function fetchData(callback) {
    setTimeout(() => {
        const data = { id: 1, name: 'Test Data' };
        callback(data);
    }, 1000);
}

// Using the callback
fetchData((data) => {
    console.log('Received:', data);
});
// Output (after 1s): Received: { id: 1, name: 'Test Data' }

// Testing scenario - Waiting for element
function waitForElement(selector, callback) {
    const checkElement = () => {
        const element = document.querySelector(selector);
        if (element) {
            callback(element);
        } else {
            setTimeout(checkElement, 100);  // Check again
        }
    };
    checkElement();
}

// Usage
waitForElement('.product-card', (element) => {
    console.log('Element found:', element);
    // Perform test actions
});
```

**Callback Hell (Pyramid of Doom):**

```javascript
// PROBLEM - Nested callbacks (hard to read and maintain)
function testUserFlow() {
    loginUser((user) => {
        getUserProfile(user.id, (profile) => {
            getOrders(profile.id, (orders) => {
                getOrderDetails(orders[0].id, (details) => {
                    console.log('Order details:', details);
                    // 4 levels of nesting!
                });
            });
        });
    });
}

// This is hard to:
// - Read
// - Debug
// - Handle errors
// - Maintain
```

**Error Handling with Callbacks:**

```javascript
// Error-first callback pattern (Node.js style)
function fetchUser(id, callback) {
    setTimeout(() => {
        if (id <= 0) {
            callback(new Error('Invalid user ID'), null);
        } else {
            callback(null, { id: id, name: 'Test User' });
        }
    }, 1000);
}

// Usage
fetchUser(1, (error, user) => {
    if (error) {
        console.error('Error:', error.message);
        return;
    }
    console.log('User:', user);
});
```

### 10.3 Promises

**Creating Promises:**

```javascript
// Promise represents eventual completion/failure of async operation
const promise = new Promise((resolve, reject) => {
    // Async operation
    setTimeout(() => {
        const success = true;
        
        if (success) {
            resolve('Operation successful!');  // Success
        } else {
            reject('Operation failed!');        // Failure
        }
    }, 1000);
});

// Consuming promise
promise
    .then(result => {
        console.log(result);  // If resolved
    })
    .catch(error => {
        console.error(error);  // If rejected
    });

// Testing scenario - API call
function loginUser(credentials) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            if (credentials.username && credentials.password) {
                resolve({
                    id: 123,
                    username: credentials.username,
                    token: 'abc123xyz'
                });
            } else {
                reject(new Error('Invalid credentials'));
            }
        }, 1000);
    });
}

// Usage
loginUser({ username: 'testuser', password: 'Test@123' })
    .then(user => {
        console.log('Logged in:', user);
        return user.token;  // Can chain!
    })
    .then(token => {
        console.log('Token:', token);
    })
    .catch(error => {
        console.error('Login failed:', error.message);
    });
```

**Promise States:**

```javascript
// 1. Pending - Initial state
const pending = new Promise((resolve, reject) => {
    // Not resolved or rejected yet
});

// 2. Fulfilled - Operation completed successfully
const fulfilled = new Promise((resolve) => {
    resolve('Success!');
});

// 3. Rejected - Operation failed
const rejected = new Promise((resolve, reject) => {
    reject('Failed!');
});

// Once settled (fulfilled or rejected), cannot change state
```

**Promise Chaining:**

```javascript
// Solve callback hell with promise chaining
function loginUser() {
    return new Promise(resolve => {
        setTimeout(() => resolve({ id: 1, name: 'John' }), 1000);
    });
}

function getUserProfile(userId) {
    return new Promise(resolve => {
        setTimeout(() => resolve({ userId, email: 'john@test.com' }), 1000);
    });
}

function getOrders(userId) {
    return new Promise(resolve => {
        setTimeout(() => resolve([{ id: 101, total: 99.99 }]), 1000);
    });
}

// Clean chain (much better than callback hell!)
loginUser()
    .then(user => {
        console.log('1. Logged in:', user);
        return getUserProfile(user.id);
    })
    .then(profile => {
        console.log('2. Got profile:', profile);
        return getOrders(profile.userId);
    })
    .then(orders => {
        console.log('3. Got orders:', orders);
    })
    .catch(error => {
        console.error('Error:', error);
    })
    .finally(() => {
        console.log('4. Cleanup');
    });

// Output (after 3 seconds):
// 1. Logged in: { id: 1, name: 'John' }
// 2. Got profile: { userId: 1, email: 'john@test.com' }
// 3. Got orders: [{ id: 101, total: 99.99 }]
// 4. Cleanup
```

**Promise.all() - Run in Parallel:**

```javascript
// Execute multiple promises concurrently
const promise1 = new Promise(resolve => setTimeout(() => resolve('Result 1'), 1000));
const promise2 = new Promise(resolve => setTimeout(() => resolve('Result 2'), 1500));
const promise3 = new Promise(resolve => setTimeout(() => resolve('Result 3'), 500));

Promise.all([promise1, promise2, promise3])
    .then(results => {
        console.log(results);  // ['Result 1', 'Result 2', 'Result 3']
        // All completed after 1.5 seconds (not 3!)
    })
    .catch(error => {
        console.error('One failed:', error);
        // If ANY promise rejects, whole thing rejects
    });

// Testing scenario - Parallel API calls
async function fetchAllTestData() {
    const users = fetch('/api/users').then(r => r.json());
    const products = fetch('/api/products').then(r => r.json());
    const orders = fetch('/api/orders').then(r => r.json());

    try {
        const [usersData, productsData, ordersData] = await Promise.all([
            users,
            products,
            orders
        ]);

        console.log('All data fetched:', {
            users: usersData.length,
            products: productsData.length,
            orders: ordersData.length
        });

        return { usersData, productsData, ordersData };
    } catch (error) {
        console.error('Failed to fetch data:', error);
    }
}
```

**Promise.race() - First to Complete:**

```javascript
// Returns first promise that settles (resolves or rejects)
const slow = new Promise(resolve => setTimeout(() => resolve('Slow'), 2000));
const fast = new Promise(resolve => setTimeout(() => resolve('Fast'), 500));

Promise.race([slow, fast])
    .then(result => {
        console.log(result);  // 'Fast' (wins the race)
    });

// Testing scenario - Timeout implementation
function fetchWithTimeout(url, timeout = 5000) {
    const fetchPromise = fetch(url);
    const timeoutPromise = new Promise((_, reject) => {
        setTimeout(() => reject(new Error('Request timeout')), timeout);
    });

    return Promise.race([fetchPromise, timeoutPromise]);
}

// Usage
fetchWithTimeout('https://api.example.com/slow-endpoint', 3000)
    .then(response => response.json())
    .then(data => console.log('Data:', data))
    .catch(error => console.error('Error:', error.message));
```

**Promise.allSettled() - Wait for All:**

```javascript
// Returns all results (both fulfilled and rejected)
const promises = [
    Promise.resolve('Success 1'),
    Promise.reject('Error 1'),
    Promise.resolve('Success 2'),
    Promise.reject('Error 2')
];

Promise.allSettled(promises)
    .then(results => {
        console.log(results);
        // [
        //   { status: 'fulfilled', value: 'Success 1' },
        //   { status: 'rejected', reason: 'Error 1' },
        //   { status: 'fulfilled', value: 'Success 2' },
        //   { status: 'rejected', reason: 'Error 2' }
        // ]

        const successful = results.filter(r => r.status === 'fulfilled');
        const failed = results.filter(r => r.status === 'rejected');

        console.log(`${successful.length} succeeded, ${failed.length} failed`);
    });

// Testing scenario - Run all tests regardless of failures
async function runAllTests(tests) {
    const testPromises = tests.map(test => executeTest(test));
    const results = await Promise.allSettled(testPromises);

    const passed = results.filter(r => r.status === 'fulfilled').length;
    const failed = results.filter(r => r.status === 'rejected').length;

    console.log(`Results: ${passed} passed, ${failed} failed`);
    return results;
}
```

### 10.4 Async/Await (Modern Approach)

**Basic Async/Await:**

```javascript
// async function always returns a promise
async function fetchUser() {
    return { id: 1, name: 'John' };  // Automatically wrapped in Promise
}

fetchUser().then(user => console.log(user));

// await pauses execution until promise resolves
async function getUserData() {
    console.log('Fetching user...');
    
    // Wait for promise to resolve
    const user = await fetchUser();
    console.log('User:', user);

    // Continue after promise resolves
    console.log('Done');
}

getUserData();
// Output:
// Fetching user...
// User: { id: 1, name: 'John' }
// Done

// Without async/await (promise chain)
function getUserDataOld() {
    console.log('Fetching user...');
    
    fetchUser()
        .then(user => {
            console.log('User:', user);
            console.log('Done');
        });
}
```

**Error Handling with Async/Await:**

```javascript
// Using try/catch
async function fetchData() {
    try {
        const response = await fetch('https://api.example.com/data');
        
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        
        const data = await response.json();
        console.log('Data:', data);
        return data;
        
    } catch (error) {
        console.error('Failed to fetch data:', error.message);
        throw error;  // Re-throw if needed
    }
}

// Usage
fetchData()
    .then(data => console.log('Success:', data))
    .catch(error => console.error('Error:', error));
```

**Sequential vs Parallel with Async/Await:**

```javascript
// SEQUENTIAL - One after another (slower)
async function fetchSequential() {
    console.time('Sequential');
    
    const user = await fetchUser();        // Wait 1s
    const profile = await fetchProfile();  // Wait 1s
    const orders = await fetchOrders();    // Wait 1s
    
    console.timeEnd('Sequential');  // ~3 seconds
    return { user, profile, orders };
}

// PARALLEL - All at once (faster)
async function fetchParallel() {
    console.time('Parallel');
    
    // Start all at once
    const userPromise = fetchUser();
    const profilePromise = fetchProfile();
    const ordersPromise = fetchOrders();
    
    // Wait for all
    const user = await userPromise;
    const profile = await profilePromise;
    const orders = await ordersPromise;
    
    console.timeEnd('Parallel');  // ~1 second (fastest of the three)
    return { user, profile, orders };
}

// Or use Promise.all for cleaner syntax
async function fetchParallelClean() {
    console.time('Parallel');
    
    const [user, profile, orders] = await Promise.all([
        fetchUser(),
        fetchProfile(),
        fetchOrders()
    ]);
    
    console.timeEnd('Parallel');  // ~1 second
    return { user, profile, orders };
}
```

**Real-World Testing Examples:**

```javascript
// Example 1: API Testing
class APITester {
    constructor(baseUrl) {
        this.baseUrl = baseUrl;
    }

    async get(endpoint) {
        try {
            const response = await fetch(`${this.baseUrl}${endpoint}`);
            
            if (!response.ok) {
                throw new Error(`GET ${endpoint} failed: ${response.status}`);
            }
            
            return await response.json();
        } catch (error) {
            console.error(`API Error:`, error.message);
            throw error;
        }
    }

    async post(endpoint, data) {
        try {
            const response = await fetch(`${this.baseUrl}${endpoint}`, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(data)
            });
            
            if (!response.ok) {
                throw new Error(`POST ${endpoint} failed: ${response.status}`);
            }
            
            return await response.json();
        } catch (error) {
            console.error(`API Error:`, error.message);
            throw error;
        }
    }

    async testUserFlow() {
        console.log('Starting user flow test...');
        
        try {
            // 1. Create user
            const newUser = await this.post('/users', {
                name: 'Test User',
                email: 'test@example.com'
            });
            console.log('✓ User created:', newUser.id);
            
            // 2. Get user details
            const user = await this.get(`/users/${newUser.id}`);
            console.log('✓ User retrieved:', user.name);
            
            // 3. Create order
            const order = await this.post('/orders', {
                userId: user.id,
                items: [{ productId: 1, quantity: 2 }]
            });
            console.log('✓ Order created:', order.id);
            
            // 4. Verify order
            const orderDetails = await this.get(`/orders/${order.id}`);
            console.log('✓ Order verified:', orderDetails);
            
            console.log('✓ User flow test completed successfully');
            return true;
            
        } catch (error) {
            console.error('✗ User flow test failed:', error.message);
            return false;
        }
    }
}

// Usage
const tester = new APITester('https://api.example.com');
tester.testUserFlow();
```

**Example 2: Playwright/Puppeteer Testing:**

```javascript
// Async/await with Playwright
async function testLogin(page) {
    try {
        // Navigate
        await page.goto('https://example.com/login');
        console.log('✓ Navigated to login page');
        
        // Fill form
        await page.fill('#email', 'test@example.com');
        await page.fill('#password', 'Test@123');
        console.log('✓ Filled credentials');
        
        // Click login
        await page.click('#login-btn');
        console.log('✓ Clicked login button');
        
        // Wait for navigation
        await page.waitForURL('**/dashboard');
        console.log('✓ Redirected to dashboard');
        
        // Verify welcome message
        const welcomeText = await page.textContent('.welcome-message');
        console.log('✓ Welcome message:', welcomeText);
        
        if (welcomeText.includes('Welcome')) {
            console.log('✓✓ Login test PASSED');
            return true;
        } else {
            console.log('✗✗ Login test FAILED');
            return false;
        }
        
    } catch (error) {
        console.error('✗ Login test error:', error.message);
        return false;
    }
}
```

**Example 3: Retry Logic:**

```javascript
// Retry failed operations
async function retryOperation(operation, maxRetries = 3, delay = 1000) {
    for (let attempt = 1; attempt <= maxRetries; attempt++) {
        try {
            console.log(`Attempt ${attempt}/${maxRetries}`);
            const result = await operation();
            console.log('✓ Success');
            return result;
            
        } catch (error) {
            console.error(`✗ Attempt ${attempt} failed:`, error.message);
            
            if (attempt === maxRetries) {
                throw new Error(`Failed after ${maxRetries} attempts`);
            }
            
            console.log(`Waiting ${delay}ms before retry...`);
            await new Promise(resolve => setTimeout(resolve, delay));
        }
    }
}

// Usage
async function unstableAPICall() {
    const response = await fetch('https://api.example.com/unstable');
    if (!response.ok) throw new Error('API call failed');
    return await response.json();
}

retryOperation(unstableAPICall, 3, 2000)
    .then(data => console.log('Final result:', data))
    .catch(error => console.error('All retries failed:', error));
```

### 10.5 Cypress and Async Handling

**Cypress Commands are Already Async:**

```javascript
// Cypress handles async automatically - no async/await needed!

describe('Login Test', () => {
    it('should login successfully', () => {
        // All Cypress commands are automatically queued and chained
        cy.visit('/login');                    // Async - waits for page load
        cy.get('#email').type('test@test.com'); // Async - waits for element
        cy.get('#password').type('Test@123');   // Async
        cy.get('#login-btn').click();           // Async
        cy.url().should('include', '/dashboard'); // Async - waits
        
        // No need for await! Cypress handles it
    });
});

// Commands are enqueued, not executed immediately
it('demonstrates command queue', () => {
    cy.log('First');
    console.log('A');  // Executes immediately!
    cy.log('Second');
    console.log('B');  // Executes immediately!
    cy.log('Third');
});

// Output order:
// Console: A B
// Cypress: First Second Third
```

**When You Need async/await in Cypress:**

```javascript
// 1. Using Cypress.Promise
it('should wait for promise', () => {
    cy.wrap(null).then(async () => {
        const result = await fetch('https://api.example.com/data');
        const data = await result.json();
        expect(data).to.have.property('id');
    });
});

// 2. Custom commands with async operations
Cypress.Commands.add('apiLogin', async (credentials) => {
    const response = await fetch('/api/login', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(credentials)
    });
    
    const data = await response.json();
    return data.token;
});

// Usage
it('should login via API', () => {
    cy.apiLogin({ email: 'test@test.com', password: 'Test@123' })
        .then(token => {
            expect(token).to.exist;
            cy.setCookie('auth_token', token);
        });
});

// 3. Task with async operation
// cypress.config.js
module.exports = {
    e2e: {
        setupNodeEvents(on, config) {
            on('task', {
                async queryDatabase(query) {
                    const result = await database.query(query);
                    return result;
                }
            });
        }
    }
};

// Test file
it('should query database', () => {
    cy.task('queryDatabase', 'SELECT * FROM users')
        .then(results => {
            expect(results).to.have.length.greaterThan(0);
        });
});
```

### Common Mistakes to Avoid

**1. Forgetting await**

```javascript
// WRONG - Forgot await
async function fetchData() {
    const data = fetch(url);  // Returns promise, not data!
    console.log(data);  // Promise object
}

// CORRECT
async function fetchData() {
    const response = await fetch(url);
    const data = await response.json();
    console.log(data);  // Actual data
}
```

**2. Using await in non-async function**

```javascript
// WRONG - Can't use await outside async
function getData() {
    const data = await fetch(url);  // Error!
}

// CORRECT
async function getData() {
    const data = await fetch(url);  // Works
}
```

**3. Not handling errors**

```javascript
// WRONG - Unhandled promise rejection
async function fetchData() {
    const data = await fetch(url);  // Might fail!
    return data;
}

// CORRECT - Handle errors
async function fetchData() {
    try {
        const response = await fetch(url);
        if (!response.ok) throw new Error('Fetch failed');
        return await response.json();
    } catch (error) {
        console.error('Error:', error);
        throw error;
    }
}
```

**4. Sequential when parallel is better**

```javascript
// WRONG - Sequential (slow)
const user = await fetchUser();
const orders = await fetchOrders();
// Takes 2 seconds if each takes 1 second

// CORRECT - Parallel (fast)
const [user, orders] = await Promise.all([
    fetchUser(),
    fetchOrders()
]);
// Takes 1 second total
```

### Interview Tips - Async JavaScript

**Q: What is the difference between Promises and async/await?**

**Answer:** "async/await is syntactic sugar over Promises, making async code look synchronous:

**Promise:**
```javascript
fetchData()
    .then(data => processData(data))
    .then(result => console.log(result))
    .catch(error => console.error(error));
```

**Async/await:**
```javascript
try {
    const data = await fetchData();
    const result = await processData(data);
    console.log(result);
} catch (error) {
    console.error(error);
}
```

Async/await is more readable and easier to debug. Both work the same under the hood."

**Q: How does Cypress handle async operations?**

**Answer:** "Cypress automatically queues and chains commands, no async/await needed:

```javascript
cy.visit('/page');  // Queued
cy.get('#button').click();  // Queued after visit
cy.get('.result').should('be.visible');  // Queued after click
```

Commands execute in order automatically. Only need async/await for non-Cypress operations like fetch() or custom tasks."

**Q: Explain Promise.all() vs Promise.race()**

**Answer:**
- **Promise.all()**: Waits for ALL promises, returns array of all results. Rejects if any fails.
- **Promise.race()**: Returns FIRST promise that settles (resolves or rejects).

```javascript
// all - fetch all data concurrently
const [users, products] = await Promise.all([fetchUsers(), fetchProducts()]);

// race - implement timeout
const result = await Promise.race([
    fetchData(),
    timeout(5000)
]);
```

Use Promise.all() for parallel operations, Promise.race() for timeouts or competitive requests."

---


## 11. ES6+ Modern JavaScript Features

### 11.1 Let and Const vs Var

**Comparison:**

| Feature | var | let | const |
|---------|-----|-----|-------|
| Scope | Function | Block | Block |
| Hoisting | Yes (undefined) | Yes (TDZ) | Yes (TDZ) |
| Reassignment | Yes | Yes | No |
| Redeclaration | Yes | No | No |
| Best for | Legacy code | Variables | Constants |

```javascript
// var - function scoped
function testVar() {
    if (true) {
        var x = 10;
    }
    console.log(x);  // 10 - accessible outside block
}

// let - block scoped
function testLet() {
    if (true) {
        let x = 10;
    }
    console.log(x);  // ReferenceError - not accessible
}

// const - block scoped, immutable binding
const API_URL = 'https://api.example.com';
API_URL = 'https://other.com';  // TypeError

// BUT const objects are mutable
const config = { timeout: 5000 };
config.timeout = 10000;  // ✅ Allowed - object properties can change
config = {};  // ❌ Error - cannot reassign
```

### 11.2 Destructuring Assignment

**Array Destructuring:**

```javascript
// Basic
const browsers = ['Chrome', 'Firefox', 'Safari'];
const [primary, secondary, tertiary] = browsers;
console.log(primary);  // Chrome

// Skip elements
const [first, , third] = browsers;

// Default values
const [a, b, c, d = 'Edge'] = browsers;
console.log(d);  // Edge

// Rest operator
const [head, ...rest] = browsers;
console.log(rest);  // ['Firefox', 'Safari']

// Cypress example
cy.wrap([200, 'Success', { id: 1 }]).then(([status, msg, data]) => {
    expect(status).to.equal(200);
    expect(msg).to.equal('Success');
});
```

**Object Destructuring:**

```javascript
// Basic
const user = { name: 'John', email: 'john@test.com', role: 'admin' };
const { name, email } = user;

// Rename variables
const { name: username, email: userEmail } = user;

// Default values
const { age = 25 } = user;

// Nested destructuring
const response = {
    status: 200,
    data: { id: 1, name: 'Product' },
    headers: { 'content-type': 'application/json' }
};

const { 
    status, 
    data: { name: productName }, 
    headers 
} = response;

// Cypress API testing
cy.request('/api/users/1').then(({ status, body }) => {
    expect(status).to.eq(200);
    expect(body).to.have.property('name');
});
```

### 11.3 Classes (ES6)

```javascript
class BasePage {
    constructor(url) {
        this.url = url;
    }
    
    visit() {
        cy.visit(this.url);
        return this;  // Method chaining
    }
    
    getTitle() {
        return cy.title();
    }
}

class LoginPage extends BasePage {
    constructor() {
        super('/login');  // Call parent constructor
        this.usernameField = '#username';
        this.passwordField = '#password';
        this.loginButton = '#login-btn';
    }
    
    login(username, password) {
        cy.get(this.usernameField).type(username);
        cy.get(this.passwordField).type(password);
        cy.get(this.loginButton).click();
        return this;
    }
    
    // Static method
    static createTestUser() {
        return { username: 'test', password: 'Test@123' };
    }
}

// Usage
const loginPage = new LoginPage();
loginPage.visit().login('admin', 'password');

const testUser = LoginPage.createTestUser();
```

### 11.4 Modules (Import/Export)

**Exporting:**

```javascript
// utils.js - Named exports
export const API_URL = 'https://api.example.com';

export function login(username, password) {
    cy.get('#username').type(username);
    cy.get('#password').type(password);
    cy.get('#login').click();
}

export class User {
    constructor(name) {
        this.name = name;
    }
}

// Default export
export default function authenticate(credentials) {
    // auth logic
}
```

**Importing:**

```javascript
// test.spec.js
import authenticate from './utils.js';  // Default import
import { login, User, API_URL } from './utils.js';  // Named imports
import * as Utils from './utils.js';  // Import all

// Usage
login('admin', 'password');
const user = new User('John');
console.log(API_URL);

// Cypress example
import { loginPage } from '../pages/LoginPage';
import testData from '../fixtures/users.json';

describe('Login Tests', () => {
    it('should login successfully', () => {
        loginPage.visit();
        loginPage.login(testData.validUser.username, testData.validUser.password);
    });
});
```

### 11.5 Optional Chaining (?.)

```javascript
// Without optional chaining
const user = getUser();
const street = user && user.address && user.address.street;

// With optional chaining (ES2020)
const street = user?.address?.street;

// Cypress examples
cy.get('.user').then($el => {
    const name = $el[0]?.dataset?.username;  // Safe access
});

// API response
cy.request('/api/user').then(response => {
    const email = response.body?.user?.email;
    const firstOrder = response.body?.orders?.[0]?.id;
});

// Optional function call
const onSuccess = config?.callbacks?.onSuccess;
onSuccess?.();  // Only calls if exists
```

### 11.6 Nullish Coalescing (??)

```javascript
// || returns right side for any falsy value (0, '', false, null, undefined)
const count = 0;
const value1 = count || 10;  // 10 (0 is falsy)

// ?? returns right side only for null or undefined
const value2 = count ?? 10;  // 0 (0 is not null/undefined)

// Test automation examples
const timeout = config.timeout ?? 5000;  // Use 5000 only if timeout is null/undefined
const retries = options.retries ?? 0;
const headless = settings.headless ?? false;  // false is valid, not replaced

// Cypress config
Cypress.config('defaultCommandTimeout', userTimeout ?? 10000);
```

---

## 12. Error Handling in JavaScript

### 12.1 Try-Catch-Finally

```javascript
function divideNumbers(a, b) {
    try {
        if (b === 0) {
            throw new Error('Cannot divide by zero');
        }
        return a / b;
    } catch (error) {
        console.error('Error:', error.message);
        return null;
    } finally {
        console.log('Division operation completed');
    }
}

console.log(divideNumbers(10, 2));  // 5
console.log(divideNumbers(10, 0));  // null
```

### 12.2 Custom Error Classes

```javascript
class TestExecutionError extends Error {
    constructor(testName, reason) {
        super(`Test "${testName}" failed: ${reason}`);
        this.name = 'TestExecutionError';
        this.testName = testName;
        this.timestamp = new Date();
    }
}

class ElementNotFoundError extends Error {
    constructor(selector, timeout) {
        super(`Element "${selector}" not found after ${timeout}ms`);
        this.name = 'ElementNotFoundError';
        this.selector = selector;
        this.timeout = timeout;
    }
}

// Usage
try {
    throw new TestExecutionError('Login Test', 'Invalid credentials');
} catch (error) {
    if (error instanceof TestExecutionError) {
        console.log(`Test: ${error.testName}`);
        console.log(`Time: ${error.timestamp}`);
    }
}
```

### 12.3 Error Handling in Cypress

```javascript
// Cypress handles errors automatically, but you can customize

// 1. Catch specific errors
cy.on('fail', (error) => {
    if (error.message.includes('Timed out')) {
        // Handle timeout differently
        cy.screenshot('timeout-error');
        throw error;
    }
});

// 2. Conditional error handling
cy.get('#optional-element', { failOnStatusCode: false })
    .then($el => {
        if ($el.length === 0) {
            cy.log('Element not found, continuing test');
        }
    });

// 3. Try-catch with async
it('handles API errors', async () => {
    try {
        const response = await fetch('/api/data');
        expect(response.ok).to.be.true;
    } catch (error) {
        cy.log('API request failed:', error.message);
    }
});

// 4. Custom command with error handling
Cypress.Commands.add('safeClick', (selector) => {
    cy.get(selector).then($el => {
        if ($el.is(':visible') && $el.is(':enabled')) {
            cy.wrap($el).click();
        } else {
            throw new Error(`Cannot click ${selector} - not visible or disabled`);
        }
    });
});
```

### 12.4 Async Error Handling

```javascript
// Promise error handling
function fetchTestData(id) {
    return fetch(`/api/test/${id}`)
        .then(response => {
            if (!response.ok) {
                throw new Error(`HTTP ${response.status}`);
            }
            return response.json();
        })
        .catch(error => {
            console.error('Fetch error:', error);
            throw error;  // Re-throw to propagate
        });
}

// Async/await with try-catch
async function runTest(testId) {
    try {
        const testData = await fetchTestData(testId);
        const result = await executeTest(testData);
        return result;
    } catch (error) {
        console.error(`Test ${testId} failed:`, error.message);
        // Take screenshot
        await takeScreenshot(`test-${testId}-error`);
        throw error;
    } finally {
        // Cleanup
        await cleanup();
    }
}
```

---

## 13. JSON Handling

### 13.1 JSON.parse() and JSON.stringify()

```javascript
// JSON.stringify() - Object to JSON string
const testData = {
    username: 'admin',
    password: 'Test@123',
    rememberMe: true,
    roles: ['admin', 'user']
};

const jsonString = JSON.stringify(testData);
console.log(jsonString);
// {"username":"admin","password":"Test@123","rememberMe":true,"roles":["admin","user"]}

// Pretty print with indentation
const prettyJson = JSON.stringify(testData, null, 2);
console.log(prettyJson);

// JSON.parse() - JSON string to Object
const parsedData = JSON.parse(jsonString);
console.log(parsedData.username);  // admin
```

### 13.2 Working with Test Data (Cypress)

```javascript
// Reading fixture files
describe('User Tests', () => {
    let userData;
    
    before(() => {
        cy.fixture('users.json').then(data => {
            userData = data;
        });
    });
    
    it('should login with fixture data', () => {
        cy.visit('/login');
        cy.get('#username').type(userData.admin.username);
        cy.get('#password').type(userData.admin.password);
        cy.get('#login').click();
    });
});

// Writing test results to JSON
const results = {
    testName: 'Login Test',
    status: 'passed',
    duration: 1500,
    timestamp: new Date().toISOString()
};

cy.writeFile('cypress/results/test-results.json', results);

// Appending to existing JSON
cy.readFile('cypress/results/all-results.json').then(data => {
    data.push(results);
    cy.writeFile('cypress/results/all-results.json', data);
});
```

### 13.3 API Response Handling

```javascript
// Validating JSON response structure
cy.request('/api/users/1').then(response => {
    expect(response.body).to.have.property('id');
    expect(response.body).to.have.property('name');
    expect(response.body.roles).to.be.an('array');
    
    // Deep comparison
    const expectedUser = {
        id: 1,
        name: 'John Doe',
        email: 'john@test.com'
    };
    expect(response.body).to.deep.include(expectedUser);
});

// Schema validation
const schema = {
    id: 'number',
    name: 'string',
    email: 'string',
    active: 'boolean'
};

cy.request('/api/users/1').then(response => {
    Object.keys(schema).forEach(key => {
        expect(response.body).to.have.property(key);
        expect(typeof response.body[key]).to.equal(schema[key]);
    });
});
```

---

## 14. DOM Manipulation Basics

### 14.1 Understanding the DOM

```javascript
// DOM structure
// document
//   └── html
//       ├── head
//       │   └── title
//       └── body
//           ├── div#app
//           │   ├── header
//           │   ├── main
//           │   └── footer
//           └── script
```

### 14.2 Selecting Elements (For Understanding)

```javascript
// In browser console or custom tasks

// By ID
const element = document.getElementById('login-button');

// By class
const elements = document.getElementsByClassName('error-message');

// By tag
const paragraphs = document.getElementsByTagName('p');

// Query selector (CSS selector)
const button = document.querySelector('#login-button');
const buttons = document.querySelectorAll('.btn');

// In Cypress (preferred)
cy.get('#login-button');  // ID
cy.get('.error-message');  // Class
cy.get('button');  // Tag
cy.get('button[type="submit"]');  // Attribute
cy.contains('Login');  // Text content
```

### 14.3 Element Properties and Attributes

```javascript
// Cypress examples
cy.get('input#email').then($el => {
    // Get attribute
    const placeholder = $el.attr('placeholder');
    const type = $el.attr('type');
    
    // Get property
    const value = $el.val();
    const isDisabled = $el.prop('disabled');
    
    // Check visibility
    const isVisible = $el.is(':visible');
    
    // Get text
    const text = $el.text();
    
    // Get HTML
    const html = $el.html();
});

// Cypress commands
cy.get('#email')
    .should('have.attr', 'type', 'email')
    .should('have.attr', 'required')
    .should('be.visible')
    .should('not.be.disabled');
```

### 14.4 Understanding Events (For Test Automation)

```javascript
// Common events testers should know
// Click events: click, dblclick, contextmenu
// Form events: submit, change, input, focus, blur
// Keyboard: keydown, keyup, keypress
// Mouse: mouseenter, mouseleave, mouseover

// Cypress triggers events automatically
cy.get('button').click();  // Triggers click event
cy.get('input').type('text');  // Triggers keydown, input, keyup
cy.get('select').select('option2');  // Triggers change event

// Manual event triggering
cy.get('input').trigger('focus');
cy.get('button').trigger('mouseover');

// Custom events
cy.get('#custom').trigger('customEvent', { detail: 'data' });
```

---

## 15. TypeScript Basics

### 15.1 Why TypeScript for Test Automation?

**Benefits:**
- Type safety (catch errors at compile time)
- Better IDE support (autocomplete, refactoring)
- Enhanced code documentation
- Easier maintenance for large projects

### 15.2 Basic Types

```typescript
// Primitive types
let testName: string = 'Login Test';
let testCount: number = 10;
let isPassed: boolean = true;
let data: null = null;
let value: undefined = undefined;

// Array types
let browsers: string[] = ['Chrome', 'Firefox'];
let scores: Array<number> = [95, 87, 92];

// Tuple (fixed-length array with specific types)
let result: [string, number, boolean] = ['TC001', 1500, true];

// Any (avoid when possible)
let dynamicData: any = 'string';
dynamicData = 123;  // OK but loses type safety

// Unknown (safer than any)
let unknownData: unknown = 'test';
if (typeof unknownData === 'string') {
    console.log(unknownData.toUpperCase());  // Type guard required
}
```

### 15.3 Interfaces

```typescript
// Define shape of objects
interface User {
    id: number;
    username: string;
    email: string;
    role?: string;  // Optional property
    readonly createdAt: Date;  // Read-only
}

const testUser: User = {
    id: 1,
    username: 'john_doe',
    email: 'john@test.com',
    createdAt: new Date()
};

// testUser.createdAt = new Date();  // Error - readonly

// Interface for functions
interface LoginFunction {
    (username: string, password: string): void;
}

const login: LoginFunction = (user, pass) => {
    cy.get('#username').type(user);
    cy.get('#password').type(pass);
    cy.get('#login').click();
};

// Interface for Page Objects
interface BasePage {
    visit(): void;
    getTitle(): Cypress.Chainable<string>;
}

class LoginPage implements BasePage {
    visit(): void {
        cy.visit('/login');
    }
    
    getTitle(): Cypress.Chainable<string> {
        return cy.title();
    }
    
    login(username: string, password: string): void {
        cy.get('#username').type(username);
        cy.get('#password').type(password);
        cy.get('#login').click();
    }
}
```

### 15.4 Type Aliases

```typescript
// Type alias
type TestStatus = 'passed' | 'failed' | 'skipped';
type TestPriority = 'high' | 'medium' | 'low';

interface TestCase {
    id: string;
    name: string;
    status: TestStatus;
    priority: TestPriority;
}

const test: TestCase = {
    id: 'TC001',
    name: 'Login Test',
    status: 'passed',  // Must be one of the allowed values
    priority: 'high'
};

// Union types
type ID = string | number;

function getTestById(id: ID): void {
    console.log(`Fetching test ${id}`);
}

getTestById('TC001');  // OK
getTestById(123);      // OK

// Function types
type TestFunction = (testData: any) => boolean;

const executeTest: TestFunction = (data) => {
    // test logic
    return true;
};
```

### 15.5 Generics

```typescript
// Generic function
function wrapData<T>(data: T): { value: T; timestamp: Date } {
    return {
        value: data,
        timestamp: new Date()
    };
}

const wrappedString = wrapData<string>('test');
const wrappedNumber = wrapData<number>(123);

// Generic interface
interface TestResult<T> {
    testName: string;
    data: T;
    passed: boolean;
}

const stringResult: TestResult<string> = {
    testName: 'TC001',
    data: 'Success',
    passed: true
};

const objectResult: TestResult<{ code: number; message: string }> = {
    testName: 'TC002',
    data: { code: 200, message: 'OK' },
    passed: true
};

// Cypress with TypeScript
cy.wrap<string>('test data').should('equal', 'test data');
cy.request<{ id: number; name: string }>('/api/user').then(response => {
    expect(response.body.id).to.be.a('number');
});
```

### 15.6 Cypress with TypeScript

**cypress.config.ts:**
```typescript
import { defineConfig } from 'cypress';

export default defineConfig({
    e2e: {
        baseUrl: 'https://example.com',
        defaultCommandTimeout: 10000,
        viewportWidth: 1280,
        viewportHeight: 720,
        setupNodeEvents(on, config) {
            // Plugin configuration
        }
    }
});
```

**Custom Command Types:**
```typescript
// cypress/support/commands.ts
declare global {
    namespace Cypress {
        interface Chainable {
            login(username: string, password: string): Chainable<void>;
            getByDataCy(value: string): Chainable<JQuery<HTMLElement>>;
        }
    }
}

Cypress.Commands.add('login', (username: string, password: string): void => {
    cy.get('#username').type(username);
    cy.get('#password').type(password);
    cy.get('#login-btn').click();
});

Cypress.Commands.add('getByDataCy', (value: string) => {
    return cy.get(`[data-cy="${value}"]`);
});
```

---

## 16. Modern JavaScript for Testing

### 16.1 Cypress Best Practices

```javascript
// 1. Use data-* attributes for selectors
cy.get('[data-cy="submit-button"]').click();

// 2. Avoid using cy.wait() with fixed time
cy.wait(5000);  // ❌ Bad

cy.get('.loader').should('not.exist');  // ✅ Good
cy.get('.result').should('be.visible');  // ✅ Good

// 3. Chain commands properly
cy.get('form')
    .find('input[name="email"]')
    .type('test@example.com')
    .should('have.value', 'test@example.com');

// 4. Use custom commands for reusable logic
Cypress.Commands.add('loginAs', (userType) => {
    const users = {
        admin: { username: 'admin', password: 'Admin@123' },
        user: { username: 'user', password: 'User@123' }
    };
    
    const { username, password } = users[userType];
    cy.visit('/login');
    cy.get('#username').type(username);
    cy.get('#password').type(password);
    cy.get('#login').click();
});

// Usage
cy.loginAs('admin');

// 5. Use aliases for reusability
cy.get('.user-list').as('users');
cy.get('@users').should('have.length', 5);
cy.get('@users').first().click();
```

### 16.2 Test Organization

```javascript
// Good test structure
describe('Login Functionality', () => {
    // Setup - runs once before all tests
    before(() => {
        cy.log('Test suite setup');
    });
    
    // Setup - runs before each test
    beforeEach(() => {
        cy.visit('/login');
        cy.clearCookies();
        cy.clearLocalStorage();
    });
    
    // Context for grouping related tests
    context('Valid Credentials', () => {
        it('should login with admin credentials', () => {
            cy.get('#username').type('admin');
            cy.get('#password').type('Admin@123');
            cy.get('#login').click();
            cy.url().should('include', '/dashboard');
        });
        
        it('should login with user credentials', () => {
            cy.get('#username').type('user');
            cy.get('#password').type('User@123');
            cy.get('#login').click();
            cy.url().should('include', '/home');
        });
    });
    
    context('Invalid Credentials', () => {
        it('should show error for invalid password', () => {
            cy.get('#username').type('admin');
            cy.get('#password').type('wrong');
            cy.get('#login').click();
            cy.get('.error').should('be.visible')
                .and('contain', 'Invalid credentials');
        });
    });
    
    // Cleanup - runs after each test
    afterEach(() => {
        cy.log('Test cleanup');
    });
    
    // Cleanup - runs once after all tests
    after(() => {
        cy.log('Test suite cleanup');
    });
});
```

### 16.3 Working with Environment Variables

```javascript
// cypress.config.js
module.exports = {
    env: {
        apiUrl: 'https://api.example.com',
        username: 'testuser',
        password: 'Test@123'
    }
};

// Using in tests
describe('API Tests', () => {
    it('should call API', () => {
        const apiUrl = Cypress.env('apiUrl');
        cy.request(`${apiUrl}/users`).then(response => {
            expect(response.status).to.eq(200);
        });
    });
    
    it('should login', () => {
        cy.get('#username').type(Cypress.env('username'));
        cy.get('#password').type(Cypress.env('password'));
        cy.get('#login').click();
    });
});

// Command line override
// npx cypress run --env apiUrl=https://staging.example.com
```

---

## 17. Best Practices for Test Automation with JavaScript

### 17.1 Code Organization

```
cypress/
├── e2e/
│   ├── auth/
│   │   ├── login.cy.js
│   │   └── logout.cy.js
│   ├── products/
│   │   ├── search.cy.js
│   │   └── details.cy.js
│   └── checkout/
│       └── payment.cy.js
├── fixtures/
│   ├── users.json
│   └── products.json
├── support/
│   ├── commands.js
│   ├── page-objects/
│   │   ├── LoginPage.js
│   │   └── DashboardPage.js
│   └── e2e.js
└── downloads/
```

### 17.2 Naming Conventions

```javascript
// Variables and functions: camelCase
const testData = {};
function loginUser() {}

// Classes: PascalCase
class LoginPage {}

// Constants: UPPER_SNAKE_CASE
const API_BASE_URL = 'https://api.example.com';
const MAX_RETRIES = 3;

// Files: kebab-case
// login-page.js
// user-management.cy.js

// Test descriptions: Human-readable
describe('User Authentication', () => {
    it('should login successfully with valid credentials', () => {
        // test
    });
    
    it('should show error message for invalid credentials', () => {
        // test
    });
});
```

### 17.3 Avoiding Common Mistakes

```javascript
// ❌ BAD: Hardcoded waits
cy.wait(5000);
cy.get('.result').should('be.visible');

// ✅ GOOD: Smart waits
cy.get('.result', { timeout: 10000 }).should('be.visible');

// ❌ BAD: Testing implementation details
cy.get('.css-xyz123').click();

// ✅ GOOD: Test user-facing behavior
cy.get('[data-cy="submit-button"]').click();
cy.contains('button', 'Submit').click();

// ❌ BAD: Multiple assertions without chaining
cy.get('#name').should('be.visible');
cy.get('#name').should('not.be.disabled');
cy.get('#name').should('have.value', 'John');

// ✅ GOOD: Chain assertions
cy.get('#name')
    .should('be.visible')
    .and('not.be.disabled')
    .and('have.value', 'John');

// ❌ BAD: Not using Page Object Model
it('should login', () => {
    cy.visit('/login');
    cy.get('#username').type('admin');
    cy.get('#password').type('pass');
    cy.get('#login').click();
});

// ✅ GOOD: Use Page Objects
it('should login', () => {
    loginPage.visit();
    loginPage.login('admin', 'pass');
});
```

### 17.4 Performance Tips

```javascript
// 1. Minimize cy.visit() calls
beforeEach(() => {
    cy.visit('/');  // Visit once
});

// 2. Use cy.session() for auth
cy.session('user-session', () => {
    cy.visit('/login');
    cy.get('#username').type('admin');
    cy.get('#password').type('password');
    cy.get('#login').click();
});

// 3. Parallel execution
// cypress.config.js
module.exports = {
    e2e: {
        experimentalRunAllSpecs: true  // Run specs in parallel
    }
};

// 4. Disable unnecessary features
cy.visit('/', {
    onBeforeLoad: (win) => {
        win.localStorage.setItem('analytics', 'disabled');
    }
});
```

### 17.5 Maintainability

```javascript
// Use constants for repeated values
const SELECTORS = {
    username: '#username',
    password: '#password',
    loginButton: '#login-btn',
    errorMessage: '.error'
};

const TEST_DATA = {
    validUser: { username: 'admin', password: 'Admin@123' },
    invalidUser: { username: 'fake', password: 'wrong' }
};

// Reusable helper functions
function fillLoginForm(username, password) {
    cy.get(SELECTORS.username).type(username);
    cy.get(SELECTORS.password).type(password);
}

function submitForm() {
    cy.get(SELECTORS.loginButton).click();
}

// Use in tests
it('should login with valid credentials', () => {
    fillLoginForm(TEST_DATA.validUser.username, TEST_DATA.validUser.password);
    submitForm();
    cy.url().should('include', '/dashboard');
});
```

### 17.6 Documentation

```javascript
/**
 * Logs in a user with the specified credentials
 * @param {string} username - The username
 * @param {string} password - The password
 * @returns {void}
 * @example
 * login('admin', 'Admin@123');
 */
function login(username, password) {
    cy.get('#username').type(username);
    cy.get('#password').type(password);
    cy.get('#login').click();
}

/**
 * LoginPage class - Represents the login page
 */
class LoginPage {
    /**
     * Creates an instance of LoginPage
     */
    constructor() {
        this.url = '/login';
        this.usernameInput = '#username';
        this.passwordInput = '#password';
        this.loginButton = '#login-btn';
    }
    
    /**
     * Visits the login page
     */
    visit() {
        cy.visit(this.url);
    }
    
    /**
     * Performs login action
     * @param {string} username - The username
     * @param {string} password - The password
     */
    login(username, password) {
        cy.get(this.usernameInput).type(username);
        cy.get(this.passwordInput).type(password);
        cy.get(this.loginButton).click();
    }
}
```

---

## Summary

Congratulations! You've completed the comprehensive JavaScript theory for test automation. You now have knowledge of:

✅ JavaScript fundamentals (variables, data types, operators)
✅ Control structures and functions
✅ Objects, arrays, and modern array methods
✅ Asynchronous JavaScript (callbacks, promises, async/await)
✅ ES6+ features (destructuring, spread/rest, classes, modules)
✅ Error handling and custom errors
✅ JSON handling for test data
✅ DOM basics for understanding web testing
✅ TypeScript fundamentals
✅ Cypress best practices
✅ Code organization and maintainability

**Next Steps:**
- Practice with hands-on exercises (Phase3-05-JavaScript-Practicals.md)
- Prepare for interviews (Phase3-06-JavaScript-InterviewQA.md)
- Build Cypress test automation framework
- Explore Playwright and other JavaScript testing tools

---

*Phase 3 JavaScript Theory Complete - Ready for Practicals!*
