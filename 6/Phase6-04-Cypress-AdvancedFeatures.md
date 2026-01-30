# Phase 6.1: Cypress - Advanced Features

## Table of Contents
1. [Custom Commands](#custom-commands)
2. [Fixtures](#fixtures)
3. [Environment Variables](#environment-variables)
4. [Configuration](#configuration)
5. [Plugins](#plugins)
6. [Screenshots and Videos](#screenshots-and-videos)
7. [Cypress Cloud (Dashboard)](#cypress-cloud)

---

## 1. Custom Commands

Custom commands extend Cypress with reusable, application-specific actions.

### Creating Custom Commands:

**File: `cypress/support/commands.js`**

```javascript
// Login command
Cypress.Commands.add('login', (username, password) => {
  cy.visit('/login')
  cy.get('#username').type(username)
  cy.get('#password').type(password)
  cy.get('#login-btn').click()
  cy.url().should('include', '/dashboard')
})

// API login (bypass UI for speed)
Cypress.Commands.add('loginViaAPI', (username, password) => {
  cy.request({
    method: 'POST',
    url: '/api/auth/login',
    body: { username, password }
  }).then((response) => {
    window.localStorage.setItem('token', response.body.token)
  })
})

// Get element by data-testid
Cypress.Commands.add('getByTestId', (testId) => {
  return cy.get(`[data-testid="${testId}"]`)
})

// Assert toast notification
Cypress.Commands.add('shouldShowToast', (message) => {
  cy.get('.toast-message')
    .should('be.visible')
    .and('contain', message)
})

// Drag and Drop
Cypress.Commands.add('dragTo', { prevSubject: true }, (subject, targetSelector) => {
  cy.wrap(subject)
    .trigger('mousedown', { which: 1 })
  cy.get(targetSelector)
    .trigger('mousemove')
    .trigger('mouseup', { force: true })
})

// Wait for loading spinner to disappear
Cypress.Commands.add('waitForLoading', () => {
  cy.get('.loading-spinner', { timeout: 15000 }).should('not.exist')
})

// Fill form fields
Cypress.Commands.add('fillForm', (formData) => {
  Object.entries(formData).forEach(([field, value]) => {
    cy.get(`[data-testid="${field}"]`).clear().type(value)
  })
})
```

### Using Custom Commands:

```javascript
// In test files
describe('Dashboard', () => {
  beforeEach(() => {
    cy.login('admin', 'admin123')
    // OR faster:
    cy.loginViaAPI('admin', 'admin123')
  })

  it('should display welcome message', () => {
    cy.getByTestId('welcome-msg').should('contain', 'Welcome')
  })

  it('should fill registration form', () => {
    cy.fillForm({
      'first-name': 'John',
      'last-name': 'Doe',
      'email': 'john@example.com'
    })
    cy.getByTestId('submit-btn').click()
    cy.shouldShowToast('Registration successful!')
  })
})
```

### Overwriting Existing Commands:

```javascript
// Override cy.visit to always include auth token
Cypress.Commands.overwrite('visit', (originalFn, url, options) => {
  const token = localStorage.getItem('token')
  if (token) {
    options = options || {}
    options.headers = options.headers || {}
    options.headers['Authorization'] = `Bearer ${token}`
  }
  return originalFn(url, options)
})
```

### TypeScript Support for Custom Commands:

```typescript
// cypress/support/index.d.ts
declare namespace Cypress {
  interface Chainable {
    login(username: string, password: string): Chainable<void>
    loginViaAPI(username: string, password: string): Chainable<void>
    getByTestId(testId: string): Chainable<JQuery<HTMLElement>>
    shouldShowToast(message: string): Chainable<void>
  }
}
```

---

## 2. Fixtures

Fixtures provide external test data stored as JSON files.

### Creating Fixtures:

**File: `cypress/fixtures/users.json`**
```json
{
  "validUser": {
    "username": "standard_user",
    "password": "secret_sauce"
  },
  "invalidUser": {
    "username": "invalid_user",
    "password": "wrong_pass"
  },
  "lockedUser": {
    "username": "locked_out_user",
    "password": "secret_sauce"
  }
}
```

**File: `cypress/fixtures/products.json`**
```json
[
  {
    "id": 1,
    "name": "Sauce Labs Backpack",
    "price": 29.99,
    "category": "bags"
  },
  {
    "id": 2,
    "name": "Sauce Labs Bike Light",
    "price": 9.99,
    "category": "accessories"
  }
]
```

### Using Fixtures:

**Method 1: cy.fixture()**
```javascript
it('should login with fixture data', () => {
  cy.fixture('users.json').then((users) => {
    cy.get('#username').type(users.validUser.username)
    cy.get('#password').type(users.validUser.password)
    cy.get('#login-btn').click()
  })
})
```

**Method 2: Using this (beforeEach)**
```javascript
describe('Login Tests', function() {  // Must use function(), not arrow
  beforeEach(function() {
    cy.fixture('users.json').as('users')
  })

  it('should login with valid user', function() {
    cy.get('#username').type(this.users.validUser.username)
    cy.get('#password').type(this.users.validUser.password)
    cy.get('#login-btn').click()
    cy.url().should('include', '/inventory')
  })

  it('should fail with invalid user', function() {
    cy.get('#username').type(this.users.invalidUser.username)
    cy.get('#password').type(this.users.invalidUser.password)
    cy.get('#login-btn').click()
    cy.get('[data-test="error"]').should('be.visible')
  })
})
```

**Method 3: Import directly**
```javascript
import users from '../fixtures/users.json'

it('login test', () => {
  cy.get('#username').type(users.validUser.username)
})
```

### Fixtures for API Mocking:

```javascript
// Mock API response with fixture data
cy.intercept('GET', '/api/products', { fixture: 'products.json' }).as('getProducts')

cy.visit('/products')
cy.wait('@getProducts')

cy.get('.product-item').should('have.length', 2)
```

---

## 3. Environment Variables

### Setting Environment Variables:

**Method 1: cypress.config.js**
```javascript
module.exports = defineConfig({
  e2e: {
    env: {
      username: 'admin',
      password: 'admin123',
      apiUrl: 'https://api.example.com'
    }
  }
})
```

**Method 2: cypress.env.json (recommended for secrets)**
```json
{
  "username": "admin",
  "password": "admin123",
  "apiUrl": "https://api.example.com",
  "apiKey": "sk-1234567890"
}
```
*Add `cypress.env.json` to `.gitignore`!*

**Method 3: CLI**
```bash
npx cypress run --env username=admin,password=secret
```

**Method 4: System environment variables**
```bash
CYPRESS_username=admin CYPRESS_password=secret npx cypress run
```

### Accessing Environment Variables:

```javascript
// In tests
const username = Cypress.env('username')
const apiUrl = Cypress.env('apiUrl')

cy.get('#username').type(Cypress.env('username'))
cy.get('#password').type(Cypress.env('password'))

// API request with env variable
cy.request({
  url: `${Cypress.env('apiUrl')}/users`,
  headers: {
    'Authorization': `Bearer ${Cypress.env('apiKey')}`
  }
})
```

### Multiple Environments:

```javascript
// cypress.config.js
const { defineConfig } = require('cypress')

module.exports = defineConfig({
  e2e: {
    env: {
      environment: 'qa'
    },
    setupNodeEvents(on, config) {
      const environment = config.env.environment || 'qa'

      const environments = {
        qa: {
          baseUrl: 'https://qa.example.com',
          apiUrl: 'https://qa-api.example.com'
        },
        staging: {
          baseUrl: 'https://staging.example.com',
          apiUrl: 'https://staging-api.example.com'
        },
        prod: {
          baseUrl: 'https://example.com',
          apiUrl: 'https://api.example.com'
        }
      }

      config.baseUrl = environments[environment].baseUrl
      config.env.apiUrl = environments[environment].apiUrl

      return config
    }
  }
})
```

**Usage:**
```bash
npx cypress run --env environment=staging
```

---

## 4. Configuration

### cypress.config.js - Complete Options:

```javascript
const { defineConfig } = require('cypress')

module.exports = defineConfig({
  e2e: {
    // URLs
    baseUrl: 'https://www.saucedemo.com',

    // Test files
    specPattern: 'cypress/e2e/**/*.cy.{js,jsx,ts,tsx}',
    supportFile: 'cypress/support/e2e.js',
    fixturesFolder: 'cypress/fixtures',

    // Viewport
    viewportWidth: 1280,
    viewportHeight: 720,

    // Timeouts
    defaultCommandTimeout: 10000,
    pageLoadTimeout: 60000,
    requestTimeout: 15000,
    responseTimeout: 30000,
    execTimeout: 60000,
    taskTimeout: 60000,

    // Screenshots & Videos
    screenshotOnRunFailure: true,
    screenshotsFolder: 'cypress/screenshots',
    video: true,
    videosFolder: 'cypress/videos',
    videoCompression: 32,

    // Retries
    retries: {
      runMode: 2,
      openMode: 0
    },

    // Browser
    chromeWebSecurity: false,
    modifyObstructiveCode: true,

    // Other
    watchForFileChanges: true,
    numTestsKeptInMemory: 50,
    experimentalMemoryManagement: true,

    // Node events
    setupNodeEvents(on, config) {
      // Register plugins
      return config
    }
  }
})
```

---

## 5. Plugins

### Popular Cypress Plugins:

```bash
# File upload
npm install --save-dev cypress-file-upload

# XPath support
npm install --save-dev cypress-xpath

# Real events (hover, etc.)
npm install --save-dev cypress-real-events

# Mochawesome reporter
npm install --save-dev mochawesome mochawesome-merge mochawesome-report-generator

# Accessibility testing
npm install --save-dev cypress-axe
```

### Setup in support/e2e.js:

```javascript
// Import custom commands
import './commands'

// Import plugins
import 'cypress-file-upload'
import 'cypress-real-events'

// XPath support
require('cypress-xpath')

// Accessibility
import 'cypress-axe'
```

### Using Plugins:

```javascript
// XPath
cy.xpath('//button[@id="submit"]').click()

// Real hover (not just trigger)
cy.get('.menu').realHover()

// Accessibility check
cy.visit('/page')
cy.injectAxe()
cy.checkA11y()
```

---

## 6. Screenshots and Videos

### Screenshots:

```javascript
// Manual screenshot
cy.screenshot('homepage')
cy.screenshot('login-form', { capture: 'viewport' })

// Element screenshot
cy.get('.header').screenshot('header-component')

// Full page
cy.screenshot('full-page', { capture: 'fullPage' })

// Automatic on failure (default: true in cypress.config.js)
// screenshotOnRunFailure: true
```

### Videos:

```javascript
// cypress.config.js
module.exports = defineConfig({
  e2e: {
    video: true,                    // Enable video recording
    videosFolder: 'cypress/videos', // Output folder
    videoCompression: 32,           // Compression level (0-51)
  }
})
```

Videos are automatically recorded during `cypress run` (headless mode).

---

## 7. Cypress Cloud

### What is Cypress Cloud?

Paid service for:
- **Parallel execution** across multiple machines
- **Test analytics** and flake detection
- **Video and screenshot** storage
- **GitHub/GitLab/Bitbucket** integration
- **Smart test orchestration**

### Setup:

```bash
# Record tests
npx cypress run --record --key YOUR_RECORD_KEY
```

### CI Configuration:

```yaml
# GitHub Actions
- name: Cypress run
  uses: cypress-io/github-action@v6
  with:
    record: true
  env:
    CYPRESS_RECORD_KEY: ${{ secrets.CYPRESS_RECORD_KEY }}
```

---

## Summary

**Advanced Features Mastered:**
1. ✅ Custom Commands - Reusable test actions
2. ✅ Fixtures - External test data management
3. ✅ Environment Variables - Multi-environment support
4. ✅ Configuration - Comprehensive test settings
5. ✅ Plugins - Extended functionality
6. ✅ Screenshots & Videos - Visual evidence
7. ✅ Cypress Cloud - CI/CD integration

**Next:** Cypress Network Requests & Best Practices
