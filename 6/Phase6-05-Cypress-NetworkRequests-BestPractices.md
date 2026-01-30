# Phase 6.1: Cypress - Network Requests & Best Practices

## Table of Contents
1. [cy.intercept() - Network Interception](#cyintercept)
2. [Stubbing Responses](#stubbing-responses)
3. [Waiting for Requests](#waiting-for-requests)
4. [cy.request() - Direct API Calls](#cyrequest)
5. [Cypress Best Practices](#cypress-best-practices)
6. [Anti-Patterns to Avoid](#anti-patterns-to-avoid)

---

## 1. cy.intercept() - Network Interception

### Basic Interception:

```javascript
// Intercept GET requests to /api/users
cy.intercept('GET', '/api/users').as('getUsers')

cy.visit('/users')
cy.wait('@getUsers')

// Intercept with URL pattern
cy.intercept('GET', '/api/users/*').as('getUser')

// Intercept with regex
cy.intercept('GET', /\/api\/users\/\d+/).as('getUserById')
```

### Assert on Request/Response:

```javascript
cy.intercept('GET', '/api/users').as('getUsers')
cy.visit('/users')

cy.wait('@getUsers').then((interception) => {
  // Assert on response
  expect(interception.response.statusCode).to.eq(200)
  expect(interception.response.body).to.have.length.greaterThan(0)
  expect(interception.response.body[0]).to.have.property('name')

  // Assert on request
  expect(interception.request.url).to.include('/api/users')
  expect(interception.request.method).to.eq('GET')
})
```

### Intercept POST Requests:

```javascript
cy.intercept('POST', '/api/users').as('createUser')

cy.get('#name').type('John Doe')
cy.get('#email').type('john@example.com')
cy.get('#submit').click()

cy.wait('@createUser').then((interception) => {
  // Verify request body
  expect(interception.request.body.name).to.eq('John Doe')
  expect(interception.request.body.email).to.eq('john@example.com')

  // Verify response
  expect(interception.response.statusCode).to.eq(201)
  expect(interception.response.body.id).to.exist
})
```

---

## 2. Stubbing Responses

### Static Response:

```javascript
// Return static data instead of hitting real API
cy.intercept('GET', '/api/users', {
  statusCode: 200,
  body: [
    { id: 1, name: 'John', email: 'john@test.com' },
    { id: 2, name: 'Jane', email: 'jane@test.com' }
  ]
}).as('getUsers')

cy.visit('/users')
cy.wait('@getUsers')
cy.get('.user-card').should('have.length', 2)
```

### Using Fixtures:

```javascript
// Return data from fixture file
cy.intercept('GET', '/api/products', { fixture: 'products.json' }).as('getProducts')

cy.visit('/products')
cy.wait('@getProducts')
```

### Error Responses:

```javascript
// Simulate server error
cy.intercept('GET', '/api/users', {
  statusCode: 500,
  body: { error: 'Internal Server Error' }
}).as('serverError')

cy.visit('/users')
cy.wait('@serverError')
cy.get('.error-message').should('contain', 'Something went wrong')
```

### Network Failure:

```javascript
// Simulate network failure
cy.intercept('GET', '/api/users', { forceNetworkError: true }).as('networkError')

cy.visit('/users')
cy.get('.error-message').should('contain', 'Network error')
```

### Delayed Response:

```javascript
// Simulate slow API
cy.intercept('GET', '/api/users', (req) => {
  req.reply((res) => {
    res.delay = 3000  // 3 second delay
    res.send({ body: [{ name: 'John' }] })
  })
}).as('slowRequest')

cy.visit('/users')
cy.get('.loading-spinner').should('be.visible')
cy.wait('@slowRequest')
cy.get('.loading-spinner').should('not.exist')
```

### Modify Response:

```javascript
// Modify real API response
cy.intercept('GET', '/api/users', (req) => {
  req.continue((res) => {
    // Modify response before it reaches the app
    res.body[0].name = 'Modified Name'
    res.send()
  })
}).as('modifiedResponse')
```

---

## 3. Waiting for Requests

```javascript
// Wait for single request
cy.wait('@getUsers')

// Wait for multiple requests
cy.wait(['@getUsers', '@getProducts'])

// Wait with timeout
cy.wait('@getUsers', { timeout: 15000 })

// Wait for request to be called multiple times
cy.wait('@getUsers')  // First call
cy.wait('@getUsers')  // Second call

// Assert after waiting
cy.wait('@getUsers').its('response.statusCode').should('eq', 200)
```

---

## 4. cy.request() - Direct API Calls

### Making Direct HTTP Requests:

```javascript
// GET request
cy.request('GET', '/api/users').then((response) => {
  expect(response.status).to.eq(200)
  expect(response.body).to.be.an('array')
})

// POST request
cy.request({
  method: 'POST',
  url: '/api/users',
  body: { name: 'John', email: 'john@test.com' },
  headers: { 'Content-Type': 'application/json' }
}).then((response) => {
  expect(response.status).to.eq(201)
  expect(response.body.id).to.exist
})

// With authentication
cy.request({
  method: 'GET',
  url: '/api/protected',
  headers: { 'Authorization': `Bearer ${token}` }
})

// With failOnStatusCode: false (don't fail on 4xx/5xx)
cy.request({
  method: 'GET',
  url: '/api/nonexistent',
  failOnStatusCode: false
}).then((response) => {
  expect(response.status).to.eq(404)
})
```

### API Login (Bypass UI):

```javascript
// Fast login via API instead of UI
Cypress.Commands.add('loginViaAPI', () => {
  cy.request({
    method: 'POST',
    url: '/api/auth/login',
    body: {
      username: Cypress.env('username'),
      password: Cypress.env('password')
    }
  }).then((response) => {
    window.localStorage.setItem('authToken', response.body.token)
  })
})

// Usage - much faster than UI login
beforeEach(() => {
  cy.loginViaAPI()
  cy.visit('/dashboard')
})
```

### CRUD Operations:

```javascript
describe('API CRUD Tests', () => {
  let userId

  it('CREATE user', () => {
    cy.request('POST', '/api/users', {
      name: 'Test User',
      email: 'test@test.com'
    }).then((response) => {
      expect(response.status).to.eq(201)
      userId = response.body.id
    })
  })

  it('READ user', () => {
    cy.request('GET', `/api/users/${userId}`).then((response) => {
      expect(response.status).to.eq(200)
      expect(response.body.name).to.eq('Test User')
    })
  })

  it('UPDATE user', () => {
    cy.request('PUT', `/api/users/${userId}`, {
      name: 'Updated User'
    }).then((response) => {
      expect(response.status).to.eq(200)
    })
  })

  it('DELETE user', () => {
    cy.request('DELETE', `/api/users/${userId}`).then((response) => {
      expect(response.status).to.eq(204)
    })
  })
})
```

---

## 5. Cypress Best Practices

### 1. Use data-testid for Selectors

```html
<!-- HTML -->
<button data-testid="submit-btn" class="btn btn-primary">Submit</button>
```

```javascript
// ✅ BEST - Resilient to CSS changes
cy.get('[data-testid="submit-btn"]')

// ⚠️ OK - But fragile
cy.get('.btn-primary')
cy.get('#submit')

// ❌ AVOID - Very fragile
cy.get('div > form > button:nth-child(3)')
```

### 2. Don't Use Page Objects (Use App Actions)

```javascript
// ❌ Cypress discourages Page Objects
class LoginPage {
  visit() { cy.visit('/login') }
  typeUsername(username) { cy.get('#username').type(username) }
}

// ✅ Use Custom Commands instead
Cypress.Commands.add('login', (username, password) => {
  cy.visit('/login')
  cy.get('#username').type(username)
  cy.get('#password').type(password)
  cy.get('#login-btn').click()
})

// ✅ Or use App Actions
cy.visit('/login')
cy.window().its('store').invoke('dispatch', {
  type: 'LOGIN',
  payload: { username: 'admin', password: 'secret' }
})
```

### 3. Login via API, Not UI

```javascript
// ❌ SLOW - Login via UI for every test
beforeEach(() => {
  cy.visit('/login')
  cy.get('#username').type('admin')
  cy.get('#password').type('secret')
  cy.get('#login-btn').click()
})

// ✅ FAST - Login via API
beforeEach(() => {
  cy.request('POST', '/api/login', {
    username: 'admin',
    password: 'secret'
  }).then((resp) => {
    window.localStorage.setItem('token', resp.body.token)
  })
  cy.visit('/dashboard')
})
```

### 4. Test Isolation

```javascript
// ✅ Each test should be independent
it('test 1', () => {
  // Setup own state
  cy.loginViaAPI()
  cy.visit('/page1')
  // test logic
})

it('test 2', () => {
  // Don't depend on test 1
  cy.loginViaAPI()
  cy.visit('/page2')
  // test logic
})
```

### 5. Avoid Unnecessary Waiting

```javascript
// ❌ BAD - arbitrary wait
cy.wait(5000)

// ✅ GOOD - wait for specific condition
cy.get('.loading').should('not.exist')
cy.get('.data-table').should('be.visible')

// ✅ GOOD - wait for network request
cy.intercept('GET', '/api/data').as('getData')
cy.wait('@getData')
```

### 6. Organize Tests by Feature

```
cypress/e2e/
├── auth/
│   ├── login.cy.js
│   ├── logout.cy.js
│   └── registration.cy.js
├── products/
│   ├── browse.cy.js
│   ├── search.cy.js
│   └── details.cy.js
├── cart/
│   ├── add-item.cy.js
│   └── checkout.cy.js
└── api/
    ├── users.cy.js
    └── products.cy.js
```

### 7. Use beforeEach for Common Setup

```javascript
describe('Product Page', () => {
  beforeEach(() => {
    cy.loginViaAPI()
    cy.visit('/products')
    cy.intercept('GET', '/api/products').as('getProducts')
    cy.wait('@getProducts')
  })

  it('test 1', () => { /* already logged in and on products page */ })
  it('test 2', () => { /* already logged in and on products page */ })
})
```

---

## 6. Anti-Patterns to Avoid

### 1. Don't Assign Return Values

```javascript
// ❌ WRONG
const button = cy.get('#btn')
button.click()

// ✅ CORRECT
cy.get('#btn').click()
```

### 2. Don't Use cy.wait() with Arbitrary Time

```javascript
// ❌ WRONG
cy.wait(3000)

// ✅ CORRECT
cy.get('.element').should('be.visible')
```

### 3. Don't Test Third-Party Sites

```javascript
// ❌ WRONG - Don't test Google login
cy.visit('https://accounts.google.com')

// ✅ CORRECT - Mock the authentication
cy.request('POST', '/api/auth/google', { token: 'mock-token' })
```

### 4. Don't Create Tiny Tests

```javascript
// ❌ WRONG - Too granular
it('should visit page', () => { cy.visit('/login') })
it('should see username', () => { cy.get('#username').should('exist') })
it('should type username', () => { cy.get('#username').type('admin') })

// ✅ CORRECT - Meaningful test
it('should login with valid credentials', () => {
  cy.visit('/login')
  cy.get('#username').type('admin')
  cy.get('#password').type('secret')
  cy.get('#login-btn').click()
  cy.url().should('include', '/dashboard')
})
```

### 5. Don't Couple Tests Together

```javascript
// ❌ WRONG - Tests depend on each other
it('creates a user', () => { /* creates user */ })
it('edits the user', () => { /* depends on user from test 1 */ })
it('deletes the user', () => { /* depends on tests 1 & 2 */ })

// ✅ CORRECT - Independent tests
it('edits a user', () => {
  // Create user via API first
  cy.request('POST', '/api/users', { name: 'John' }).then((resp) => {
    const userId = resp.body.id
    cy.visit(`/users/${userId}/edit`)
    // Edit logic
  })
})
```

---

## Summary

**Network Testing Mastered:**
1. ✅ cy.intercept() for monitoring/stubbing network requests
2. ✅ Stubbing responses (static, fixtures, errors, delays)
3. ✅ Waiting for network requests with assertions
4. ✅ cy.request() for direct API calls
5. ✅ API login for faster tests
6. ✅ CRUD API testing

**Best Practices Applied:**
1. ✅ Use data-testid selectors
2. ✅ Custom commands over page objects
3. ✅ API login instead of UI login
4. ✅ Test isolation
5. ✅ Wait for conditions, not time
6. ✅ Organize by feature
7. ✅ Avoid common anti-patterns

**Next:** Cypress Practicals - Hands-on exercises
