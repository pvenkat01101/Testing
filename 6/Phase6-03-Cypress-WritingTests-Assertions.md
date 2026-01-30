# Phase 6.1: Cypress - Writing Tests & Assertions

## Table of Contents
1. [Test Structure](#test-structure)
2. [Querying Elements](#querying-elements)
3. [Interacting with Elements](#interacting-with-elements)
4. [Assertions](#assertions)
5. [Working with Different Elements](#working-with-different-elements)
6. [Handling Special Scenarios](#handling-special-scenarios)

---

## 1. Test Structure

### Basic Test Organization:

```javascript
// describe - test suite
describe('Login Feature', () => {

  // context - sub-group (alias for describe)
  context('Valid Credentials', () => {

    // Hooks
    before(() => {
      // Runs once before ALL tests in this block
    })

    beforeEach(() => {
      // Runs before EACH test
      cy.visit('/login')
    })

    afterEach(() => {
      // Runs after EACH test
    })

    after(() => {
      // Runs once after ALL tests
    })

    // Test cases
    it('should login with valid username and password', () => {
      cy.get('#username').type('admin')
      cy.get('#password').type('admin123')
      cy.get('#loginBtn').click()
      cy.url().should('include', '/dashboard')
    })

    it('should display welcome message', () => {
      // test code
    })
  })

  context('Invalid Credentials', () => {
    it('should show error for wrong password', () => {
      // test code
    })
  })
})
```

### Skip and Only:

```javascript
// Run only this test
it.only('focused test', () => { })

// Skip this test
it.skip('skipped test', () => { })

// Run only this suite
describe.only('focused suite', () => { })

// Skip this suite
describe.skip('skipped suite', () => { })
```

---

## 2. Querying Elements

### cy.get() - By CSS Selector:

```javascript
// By ID
cy.get('#username')

// By Class
cy.get('.btn-primary')

// By Tag
cy.get('button')

// By Attribute
cy.get('[data-testid="submit-btn"]')
cy.get('[type="email"]')
cy.get('[name="password"]')
cy.get('[placeholder="Enter username"]')

// Combined selectors
cy.get('input#username.form-control')
cy.get('div.container > form > input')

// Pseudo selectors
cy.get('li:first')
cy.get('li:last')
cy.get('li:nth-child(3)')
cy.get('input:visible')
cy.get('button:disabled')

// Multiple elements
cy.get('.item').should('have.length', 5)
```

### cy.contains() - By Text Content:

```javascript
// Find element containing text
cy.contains('Submit')
cy.contains('Login')

// With selector
cy.contains('button', 'Submit')         // button containing "Submit"
cy.contains('.nav-link', 'Dashboard')    // .nav-link containing "Dashboard"
cy.contains('a', 'Sign Up')             // Link containing "Sign Up"

// Case-sensitive by default
cy.contains('submit')                    // Won't match "Submit"
cy.contains(/submit/i)                  // Case-insensitive with regex
```

### cy.find() - Within Parent Element:

```javascript
// Find children within a parent
cy.get('.form-group').find('input')
cy.get('#nav').find('.menu-item')
cy.get('table').find('tr').should('have.length.gt', 0)
```

### cy.filter() and cy.not():

```javascript
// Filter elements
cy.get('li').filter('.active')           // Only active list items
cy.get('input').filter(':visible')       // Only visible inputs

// Exclude elements
cy.get('li').not('.disabled')            // All except disabled
cy.get('button').not('[disabled]')       // All enabled buttons
```

### cy.first(), cy.last(), cy.eq():

```javascript
cy.get('.item').first()                  // First item
cy.get('.item').last()                   // Last item
cy.get('.item').eq(2)                    // Third item (0-indexed)
cy.get('.item').eq(-1)                   // Last item
```

### cy.parent(), cy.children(), cy.siblings():

```javascript
cy.get('#child').parent()                // Parent element
cy.get('#parent').children()             // Direct children
cy.get('#element').siblings()            // Sibling elements
cy.get('#element').prev()                // Previous sibling
cy.get('#element').next()                // Next sibling
cy.get('#deep').closest('.container')    // Closest ancestor
```

### cy.within() - Scope commands:

```javascript
// Scope all commands to within this element
cy.get('.login-form').within(() => {
  cy.get('#username').type('admin')
  cy.get('#password').type('secret')
  cy.get('button').click()
})
```

### Best Practice - data-testid:

```html
<!-- Add data-testid attributes to elements -->
<button data-testid="submit-btn">Submit</button>
<input data-testid="email-input" />
```

```javascript
// Query by data-testid (most reliable)
cy.get('[data-testid="submit-btn"]').click()
cy.get('[data-testid="email-input"]').type('test@test.com')
```

---

## 3. Interacting with Elements

### Typing:

```javascript
// Type text
cy.get('#username').type('admin')

// Type with special keys
cy.get('#search').type('Cypress{enter}')       // Press Enter
cy.get('#input').type('{backspace}')            // Backspace
cy.get('#input').type('{del}')                  // Delete
cy.get('#input').type('{selectall}')            // Select all
cy.get('#input').type('{movetostart}')          // Move to start
cy.get('#input').type('{movetoend}')            // Move to end
cy.get('#input').type('{uparrow}')              // Arrow up
cy.get('#input').type('{downarrow}')            // Arrow down

// Modifier keys
cy.get('#input').type('{ctrl+a}')               // Ctrl+A
cy.get('#input').type('{ctrl+c}')               // Ctrl+C

// Type slowly (for animations)
cy.get('#input').type('hello', { delay: 100 })  // 100ms between keys

// Force type (bypass visibility checks)
cy.get('#hidden-input').type('text', { force: true })
```

### Clicking:

```javascript
// Simple click
cy.get('#btn').click()

// Double click
cy.get('#btn').dblclick()

// Right click
cy.get('#btn').rightclick()

// Click at position
cy.get('#btn').click('topLeft')
cy.get('#btn').click('center')
cy.get('#btn').click('bottomRight')

// Click at coordinates
cy.get('#btn').click(15, 40)

// Force click (bypass actionability checks)
cy.get('#hidden-btn').click({ force: true })

// Click multiple elements
cy.get('.checkbox').click({ multiple: true })
```

### Clearing:

```javascript
// Clear input field
cy.get('#search').clear()

// Clear and type
cy.get('#search').clear().type('new text')
```

### Checkboxes and Radio Buttons:

```javascript
// Check
cy.get('#agree').check()
cy.get('[type="checkbox"]').check()

// Uncheck
cy.get('#agree').uncheck()

// Check specific value
cy.get('[type="checkbox"]').check('option1')
cy.get('[type="checkbox"]').check(['option1', 'option2'])

// Radio buttons
cy.get('[type="radio"]').check('male')

// Force check
cy.get('#hidden-checkbox').check({ force: true })
```

### Dropdowns (Select):

```javascript
// Select by text
cy.get('#country').select('India')

// Select by value
cy.get('#country').select('IN')

// Select by index
cy.get('#country').select(2)

// Multiple select
cy.get('#multi-select').select(['Option 1', 'Option 2'])
```

### Scrolling:

```javascript
// Scroll element into view
cy.get('#footer').scrollIntoView()

// Scroll to position
cy.scrollTo('bottom')
cy.scrollTo('top')
cy.scrollTo(0, 500)

// Scroll within element
cy.get('.scroll-container').scrollTo('bottom')
cy.get('.scroll-container').scrollTo(0, 300)
```

### Trigger Events:

```javascript
// Trigger custom events
cy.get('#element').trigger('mouseover')
cy.get('#element').trigger('mousedown')
cy.get('#element').trigger('mouseup')
cy.get('#element').trigger('change')
cy.get('#element').trigger('input')

// Drag and drop
cy.get('#draggable')
  .trigger('mousedown', { which: 1 })
  .trigger('mousemove', { clientX: 300, clientY: 200 })
  .trigger('mouseup', { force: true })
```

### Focus and Blur:

```javascript
cy.get('#input').focus()
cy.get('#input').blur()
```

---

## 4. Assertions

### Implicit Assertions (.should()):

```javascript
// Visibility
cy.get('#element').should('be.visible')
cy.get('#element').should('not.be.visible')
cy.get('#element').should('exist')
cy.get('#element').should('not.exist')

// Text content
cy.get('#title').should('have.text', 'Dashboard')
cy.get('#title').should('contain', 'Dash')
cy.get('#title').should('not.contain', 'Login')
cy.get('#title').should('include.text', 'board')

// Value
cy.get('#input').should('have.value', 'hello')
cy.get('#input').should('not.have.value', '')

// CSS classes
cy.get('#btn').should('have.class', 'active')
cy.get('#btn').should('not.have.class', 'disabled')

// Attributes
cy.get('#link').should('have.attr', 'href', '/dashboard')
cy.get('#link').should('have.attr', 'target', '_blank')
cy.get('#input').should('have.attr', 'placeholder', 'Enter name')

// CSS properties
cy.get('#text').should('have.css', 'color', 'rgb(255, 0, 0)')
cy.get('#box').should('have.css', 'display', 'flex')

// State
cy.get('#btn').should('be.disabled')
cy.get('#btn').should('not.be.disabled')
cy.get('#btn').should('be.enabled')
cy.get('#checkbox').should('be.checked')
cy.get('#checkbox').should('not.be.checked')
cy.get('#input').should('be.focused')

// Length
cy.get('.items').should('have.length', 5)
cy.get('.items').should('have.length.gt', 3)
cy.get('.items').should('have.length.gte', 3)
cy.get('.items').should('have.length.lt', 10)
cy.get('.items').should('have.length.lte', 10)

// URL and Title
cy.url().should('include', '/dashboard')
cy.url().should('eq', 'https://example.com/dashboard')
cy.title().should('eq', 'Dashboard')

// Chained assertions (.and)
cy.get('#btn')
  .should('be.visible')
  .and('have.class', 'btn-primary')
  .and('contain', 'Submit')
  .and('not.be.disabled')
```

### Explicit Assertions (expect):

```javascript
// Using .then() with expect
cy.get('#title').then(($el) => {
  const text = $el.text()
  expect(text).to.equal('Dashboard')
  expect(text).to.include('Dash')
  expect(text).to.have.length.greaterThan(0)
})

// Chai assertions
cy.get('.items').then(($items) => {
  expect($items).to.have.length(5)
  expect($items.first()).to.have.class('active')
})

// Assert on response
cy.request('/api/users').then((response) => {
  expect(response.status).to.eq(200)
  expect(response.body).to.have.length.greaterThan(0)
  expect(response.body[0]).to.have.property('name')
})
```

### Negative Assertions:

```javascript
cy.get('#element').should('not.exist')
cy.get('#element').should('not.be.visible')
cy.get('#btn').should('not.be.disabled')
cy.get('#input').should('not.have.value', 'old value')
cy.url().should('not.include', '/login')
```

---

## 5. Working with Different Elements

### Tables:

```javascript
// Get table rows
cy.get('table tbody tr').should('have.length', 10)

// Get specific cell
cy.get('table tbody tr').eq(0).find('td').eq(1).should('have.text', 'John')

// Iterate over rows
cy.get('table tbody tr').each(($row, index) => {
  cy.wrap($row).find('td').eq(0).should('not.be.empty')
})

// Find row by content
cy.contains('tr', 'John Doe').find('td:last').should('have.text', 'Active')
```

### Date Pickers:

```javascript
// Type date directly
cy.get('#date-input').type('2024-01-15')

// Clear and set date
cy.get('#date-input').clear().type('2024-12-25')

// For custom date pickers
cy.get('.datepicker-toggle').click()
cy.get('.datepicker-month').select('December')
cy.get('.datepicker-year').select('2024')
cy.get('.datepicker-day').contains('25').click()
```

### Iframes:

```javascript
// Access iframe content
cy.get('iframe#myFrame')
  .its('0.contentDocument.body')
  .should('not.be.empty')
  .then(cy.wrap)
  .find('#element-inside-iframe')
  .type('Hello')

// Custom command for iframes
Cypress.Commands.add('getIframeBody', (iframeSelector) => {
  return cy.get(iframeSelector)
    .its('0.contentDocument.body')
    .should('not.be.empty')
    .then(cy.wrap)
})

// Usage
cy.getIframeBody('#myFrame').find('#btn').click()
```

### File Upload:

```javascript
// Using cypress-file-upload plugin
// npm install --save-dev cypress-file-upload

// In support/commands.js
import 'cypress-file-upload'

// In test
cy.get('input[type="file"]').attachFile('test-file.pdf')

// Native Cypress (v12+)
cy.get('input[type="file"]').selectFile('cypress/fixtures/test-file.pdf')

// Drag and drop
cy.get('.dropzone').selectFile('cypress/fixtures/image.png', { action: 'drag-drop' })

// Multiple files
cy.get('input[type="file"]').selectFile([
  'cypress/fixtures/file1.txt',
  'cypress/fixtures/file2.txt'
])
```

### Alerts/Confirms/Prompts:

```javascript
// Cypress auto-accepts alerts
// But you can assert on them:

// Alert
cy.on('window:alert', (text) => {
  expect(text).to.equal('Hello World!')
})
cy.get('#alert-btn').click()

// Confirm (auto-accepted)
cy.on('window:confirm', (text) => {
  expect(text).to.equal('Are you sure?')
  return true   // Click OK
  // return false  // Click Cancel
})
cy.get('#confirm-btn').click()

// Prompt
cy.window().then((win) => {
  cy.stub(win, 'prompt').returns('My Answer')
})
cy.get('#prompt-btn').click()
```

### Shadow DOM:

```javascript
// Access shadow DOM elements
cy.get('my-component').shadow().find('.inner-element')

// Or configure globally in cypress.config.js
// includeShadowDom: true

// Then query normally
cy.get('.inner-element')
```

---

## 6. Handling Special Scenarios

### Handling New Windows/Tabs:

```javascript
// Cypress doesn't support multiple tabs
// But you can test the link target

// Remove target="_blank" and visit directly
cy.get('a[target="_blank"]')
  .invoke('removeAttr', 'target')
  .click()

// Or verify the href and visit it
cy.get('a[target="_blank"]')
  .should('have.attr', 'href')
  .then((href) => {
    cy.visit(href)
  })
```

### Handling Cookies:

```javascript
// Get cookie
cy.getCookie('session_id').should('have.property', 'value', 'abc123')

// Get all cookies
cy.getCookies().should('have.length', 2)

// Set cookie
cy.setCookie('name', 'value')

// Clear cookie
cy.clearCookie('session_id')

// Clear all cookies
cy.clearCookies()

// Preserve cookies between tests
Cypress.Cookies.defaults({
  preserve: 'session_id'
})
```

### Local Storage:

```javascript
// Get value
cy.window().its('localStorage').invoke('getItem', 'token')
  .should('exist')

// Set value
cy.window().then((win) => {
  win.localStorage.setItem('token', 'abc123')
})

// Clear
cy.clearLocalStorage()
cy.clearLocalStorage('token')
```

### Viewport / Responsive Testing:

```javascript
// Set viewport size
cy.viewport(1280, 720)
cy.viewport('iphone-x')
cy.viewport('macbook-15')

// Available presets
cy.viewport('iphone-6')
cy.viewport('iphone-xr')
cy.viewport('samsung-s10')
cy.viewport('ipad-2')
cy.viewport('macbook-13')

// Test responsive behavior
it('should show mobile menu on small screens', () => {
  cy.viewport('iphone-x')
  cy.get('.mobile-menu-btn').should('be.visible')
  cy.get('.desktop-nav').should('not.be.visible')
})
```

---

## Summary

**Key Skills Acquired:**
1. ✅ Test structure (describe, it, context, hooks)
2. ✅ All querying methods (get, contains, find, filter, within)
3. ✅ All interaction methods (type, click, check, select, scroll)
4. ✅ Comprehensive assertions (should, expect, and)
5. ✅ Working with tables, iframes, file uploads, alerts
6. ✅ Handling special scenarios (cookies, local storage, viewport)

**Best Practice:** Use `data-testid` attributes for element selection - they're the most reliable and decouple tests from CSS/structure changes.

**Next:** Cypress Advanced Features (Custom Commands, Fixtures, Environment Variables)
