# Phase 6.1: Cypress - Core Concepts

## Table of Contents
1. [Automatic Waiting](#automatic-waiting)
2. [Retry-ability](#retry-ability)
3. [Command Chaining](#command-chaining)
4. [Asynchronous Nature](#asynchronous-nature)
5. [Time Travel & Debugging](#time-travel--debugging)
6. [Subject & Yielding](#subject--yielding)
7. [Aliases](#aliases)
8. [Variables & Closures in Cypress](#variables--closures-in-cypress)

---

## 1. Automatic Waiting

### The Biggest Advantage of Cypress

In Selenium, you need explicit waits everywhere:

```java
// Selenium - Need explicit waits
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
wait.until(ExpectedConditions.elementToBeClickable(By.id("btn"))).click();
```

In Cypress, **waiting is built-in**:

```javascript
// Cypress - No waits needed!
cy.get('#btn').click()
```

### What Cypress Automatically Waits For:

**Before performing actions (click, type, etc.):**
- Element exists in the DOM
- Element is visible
- Element is not hidden
- Element is not disabled
- Element is not animating
- Element is not covered by another element
- Element has scrolled into view

**Before asserting:**
- Retries assertion until it passes or times out

### Configurable Timeouts:

```javascript
// Global timeout (cypress.config.js)
module.exports = defineConfig({
  e2e: {
    defaultCommandTimeout: 10000,    // Commands: 10 seconds
    pageLoadTimeout: 60000,          // Page loads: 60 seconds
    requestTimeout: 15000,           // Network requests: 15 seconds
    responseTimeout: 30000,          // Network responses: 30 seconds
  }
})

// Per-command timeout
cy.get('#slow-element', { timeout: 15000 }).should('be.visible')

// Per-assertion timeout
cy.get('#dynamic-text').should('contain', 'Loaded', { timeout: 20000 })
```

### Example - No More Flaky Waits:

```javascript
// This JUST WORKS - no waits needed
it('loads data after API call', () => {
  cy.visit('/dashboard')

  // Cypress automatically waits for:
  // 1. Page to load
  // 2. Element to appear in DOM
  // 3. Element to become visible
  // 4. Text content to update
  cy.get('.dashboard-title').should('have.text', 'Welcome')
  cy.get('.data-table').should('be.visible')
  cy.get('.row').should('have.length.greaterThan', 0)
})
```

---

## 2. Retry-ability

### How Retry Works:

When an assertion fails, Cypress **retries the entire command chain** leading up to the assertion.

```javascript
// Cypress retries finding the element AND checking the assertion
cy.get('.list-item').should('have.length', 5)

// If only 3 items exist, Cypress will:
// 1. Query for .list-item elements
// 2. Check if length is 5
// 3. If not, WAIT and try again
// 4. Repeat until length is 5 OR timeout
```

### What Gets Retried:

```javascript
// ✅ RETRIED - Queries
cy.get('.item')              // Retried
cy.contains('Submit')        // Retried
cy.find('.child')            // Retried

// ❌ NOT RETRIED - Actions
cy.click()                   // NOT retried
cy.type('hello')             // NOT retried
cy.visit('/page')            // NOT retried
```

### Important Retry Rule:

**Only the last command before the assertion is retried:**

```javascript
// ✅ GOOD - cy.get() is retried until .btn has class 'active'
cy.get('.btn').should('have.class', 'active')

// ⚠️ CAREFUL - Only .find() is retried, NOT cy.get()
cy.get('.container')    // Queried once
  .find('.btn')         // Retried
  .should('exist')      // Assertion

// ✅ BETTER - Merge into single query
cy.get('.container .btn').should('exist')
```

### Retry vs No-Retry Examples:

```javascript
// ❌ NOT retried properly - .click() is NOT retried
cy.get('.btn').click().should('have.class', 'active')
// If .click() doesn't add 'active' class immediately, test fails

// ✅ CORRECT - Separate action from assertion
cy.get('.btn').click()
cy.get('.btn').should('have.class', 'active')
```

---

## 3. Command Chaining

### How Commands Work:

Cypress commands are **queued** and executed **serially**. Each command yields a subject to the next command.

```javascript
cy.visit('https://example.com')      // Visit page
  .get('#username')                   // Find element (yields element)
  .type('testuser')                   // Type into yielded element
  .get('#password')                   // Find another element
  .type('password123')                // Type into yielded element
  .get('#login-btn')                  // Find button
  .click()                            // Click yielded element
```

### Command Types:

**1. Parent Commands (start a new chain):**
```javascript
cy.visit('/login')
cy.get('#username')
cy.contains('Submit')
cy.request('/api/users')
cy.url()
cy.title()
```

**2. Child Commands (chain from previous):**
```javascript
cy.get('.menu')
  .find('.item')              // Child of .menu
  .first()                    // First .item
  .click()                    // Click first item
```

**3. Dual Commands (parent OR child):**
```javascript
// As parent
cy.contains('Login')

// As child
cy.get('.form').contains('Login')
```

### Chaining Examples:

```javascript
// Chain multiple actions
cy.get('.dropdown')
  .click()                    // Open dropdown
  .get('.dropdown-menu')      // Get menu
  .find('.option')            // Find options
  .contains('Option 2')      // Find specific option
  .click()                    // Click it

// Chain assertions
cy.get('#result')
  .should('be.visible')
  .and('have.text', 'Success')
  .and('have.css', 'color', 'rgb(0, 128, 0)')
```

---

## 4. Asynchronous Nature

### Commands Are NOT Promises

A common misconception: Cypress commands look synchronous but are **asynchronous**. However, they are NOT Promises.

```javascript
// ❌ WRONG - Can't use like promises
const element = cy.get('#username')  // Returns Chainable, NOT the element
element.type('hello')                // This won't work as expected

// ❌ WRONG - Can't assign return value
const text = cy.get('#title').text() // text is NOT a string

// ✅ CORRECT - Use .then() to access values
cy.get('#title').then(($el) => {
  const text = $el.text()
  cy.log('Title is: ' + text)
})

// ✅ CORRECT - Use .invoke() for jQuery methods
cy.get('#title').invoke('text').then((text) => {
  cy.log('Title is: ' + text)
})
```

### Understanding the Command Queue:

```javascript
it('demonstrates command queue', () => {
  // These are NOT executed immediately
  // They are QUEUED and run in order
  cy.visit('/page')          // Queue: [visit]
  cy.get('#btn')             // Queue: [visit, get]
  cy.get('#btn').click()     // Queue: [visit, get, click]

  // All commands above are queued FIRST
  // Then executed one by one
})
```

### Mixing Sync and Async:

```javascript
it('sync vs async', () => {
  // This log runs BEFORE Cypress commands
  console.log('1 - This runs first (synchronous)')

  cy.visit('/page')
  cy.log('2 - This runs after visit')

  cy.get('#element').then(($el) => {
    console.log('3 - This runs when element is found')
  })

  // This log runs BEFORE Cypress commands too!
  console.log('4 - This also runs first (synchronous)')

  // Output order: 1, 4, visit, 2, get, 3
})
```

### Using .then() for Synchronous Work:

```javascript
it('accessing element values', () => {
  cy.get('#price').then(($el) => {
    // Inside .then() you have access to the element
    const price = parseFloat($el.text().replace('$', ''))

    if (price > 100) {
      cy.get('#discount-code').type('SAVE20')
    }
  })
})
```

---

## 5. Time Travel & Debugging

### Time Travel:

Cypress takes **DOM snapshots** at each command. You can hover over any command in the Command Log to see the state of the DOM at that exact moment.

**How to use:**
1. Run test in interactive mode (`npx cypress open`)
2. Click on any command in the left panel
3. The app preview shows the DOM at that moment
4. Compare before/after states

### Debugging Tools:

**1. cy.log() - Print messages:**
```javascript
cy.log('About to click the button')
cy.get('#btn').click()
cy.log('Button was clicked')
```

**2. cy.pause() - Pause test execution:**
```javascript
cy.visit('/page')
cy.get('#username').type('admin')
cy.pause()  // Test pauses here - you can inspect the DOM
cy.get('#password').type('secret')
```

**3. cy.debug() - Debugger statement:**
```javascript
cy.get('#element').debug()  // Opens browser DevTools debugger
```

**4. .then() with debugger:**
```javascript
cy.get('#element').then(($el) => {
  debugger  // Chrome DevTools will pause here
  // Inspect $el in the console
})
```

**5. Screenshots:**
```javascript
// Manual screenshot
cy.screenshot('before-login')

// Automatic on failure (enabled by default)
// cypress.config.js: screenshotOnRunFailure: true
```

### Console Output:

When you click a command in the Test Runner, detailed info appears in the browser console:
- Element selector used
- Number of elements found
- Yielded value
- Applied assertions
- Retry count

---

## 6. Subject & Yielding

### What is a Subject?

Each Cypress command **yields** a subject to the next command. The subject is typically a DOM element or a value.

```javascript
cy.get('button')        // Yields: jQuery element <button>
  .click()              // Yields: same <button> element
  .should('be.disabled') // Asserts on <button>

cy.url()                // Yields: URL string
  .should('include', '/dashboard')

cy.title()              // Yields: page title string
  .should('eq', 'Dashboard')
```

### Yielding Rules:

```javascript
// Commands yield different things
cy.get('#name')           // Yields: jQuery element
cy.get('#name').type('hi') // Yields: same element
cy.get('#name').invoke('val') // Yields: element's value (string)
cy.get('#name').its('length') // Yields: number

cy.request('/api/users')  // Yields: response object
cy.wrap('hello')          // Yields: 'hello' string
cy.wrap(42)               // Yields: 42 number
```

### Using .its() to Access Properties:

```javascript
// Get property of yielded subject
cy.get('.items').its('length').should('be.gt', 0)

cy.request('/api/users').its('body').should('have.length', 5)

cy.request('/api/users').its('status').should('eq', 200)

cy.window().its('localStorage').invoke('getItem', 'token')
```

### Using .invoke() to Call Methods:

```javascript
// Call method on yielded subject
cy.get('#name').invoke('val').should('eq', 'John')

cy.get('.list').invoke('text').should('include', 'Item 1')

cy.get('#toggle').invoke('attr', 'aria-expanded').should('eq', 'true')

// Call jQuery methods
cy.get('.hidden-div').invoke('show')
cy.get('#element').invoke('css', 'background-color')
```

---

## 7. Aliases

### What are Aliases?

Aliases let you **save a reference** to a command's yielded subject and reuse it later.

```javascript
// Create alias with .as()
cy.get('#username').as('usernameInput')

// Use alias with @
cy.get('@usernameInput').type('admin')
cy.get('@usernameInput').should('have.value', 'admin')
```

### Aliasing Elements:

```javascript
beforeEach(() => {
  cy.visit('/login')

  // Alias elements in beforeEach
  cy.get('#username').as('username')
  cy.get('#password').as('password')
  cy.get('#login-btn').as('loginBtn')
})

it('should login', () => {
  cy.get('@username').type('admin')
  cy.get('@password').type('secret')
  cy.get('@loginBtn').click()
})
```

### Aliasing Network Requests:

```javascript
// Alias a network intercept
cy.intercept('GET', '/api/users').as('getUsers')

cy.visit('/users')

// Wait for the aliased request
cy.wait('@getUsers').then((interception) => {
  expect(interception.response.statusCode).to.eq(200)
  expect(interception.response.body).to.have.length.greaterThan(0)
})
```

### Aliasing Values:

```javascript
// Alias a computed value
cy.get('#price').invoke('text').as('price')

// Use later with .then()
cy.get('@price').then((price) => {
  cy.log('Price was: ' + price)
})
```

### Sharing Context with this:

```javascript
// Aliases are available as this.aliasName
beforeEach(function() {  // Must use function(), NOT arrow function
  cy.fixture('users.json').as('userData')
})

it('uses fixture data', function() {
  // Access via this.userData
  cy.get('#username').type(this.userData.username)
  cy.get('#password').type(this.userData.password)
})
```

**Important:** Must use `function()` not arrow functions `() =>` to access `this`.

---

## 8. Variables & Closures in Cypress

### The Variable Problem:

```javascript
// ❌ WRONG - cy.get() returns Chainable, not the value
const username = cy.get('#username')
username.type('admin')  // This won't work as expected

// ❌ WRONG - Can't assign text to variable directly
let text
cy.get('#title').then(($el) => {
  text = $el.text()
})
cy.log(text)  // text is UNDEFINED here (async issue)
```

### The Correct Approaches:

**1. Use .then() closures:**
```javascript
cy.get('#title').then(($el) => {
  const text = $el.text()
  // Use text inside .then()
  expect(text).to.equal('Dashboard')
})
```

**2. Use aliases:**
```javascript
cy.get('#title').invoke('text').as('titleText')

cy.get('@titleText').then((text) => {
  cy.log('Title: ' + text)
})
```

**3. Use cy.wrap() for external values:**
```javascript
const myData = { name: 'John', age: 30 }
cy.wrap(myData).its('name').should('eq', 'John')
```

**4. Chain everything:**
```javascript
// Best practice - chain and assert inline
cy.get('#title')
  .should('have.text', 'Dashboard')
  .and('be.visible')
```

### Comparing Values Between Elements:

```javascript
// Compare two element values
cy.get('#price1').invoke('text').then((price1) => {
  cy.get('#price2').invoke('text').then((price2) => {
    expect(parseFloat(price1)).to.be.lessThan(parseFloat(price2))
  })
})

// Or use aliases
cy.get('#price1').invoke('text').as('price1')
cy.get('#price2').invoke('text').as('price2')

cy.get('@price1').then(function(price1) {
  cy.get('@price2').then(function(price2) {
    expect(parseFloat(price1)).to.be.lessThan(parseFloat(price2))
  })
})
```

---

## Summary

**Core Concepts Mastered:**

1. **Automatic Waiting** - No explicit waits; Cypress waits for elements automatically
2. **Retry-ability** - Assertions retry until passing or timeout
3. **Command Chaining** - Commands queue and execute serially, yielding subjects
4. **Asynchronous Nature** - Commands are async but NOT promises; use .then()
5. **Time Travel** - DOM snapshots at each step for debugging
6. **Subject & Yielding** - Each command yields a value to the next
7. **Aliases** - Save references with .as() and reuse with @
8. **Variables** - Use closures and .then() instead of direct assignment

**Key Takeaway:** Cypress's architecture eliminates most flakiness issues found in Selenium. Understanding the command queue and async nature is essential for writing reliable tests.

**Next:** Writing Tests & Assertions in Cypress
