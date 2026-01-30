# Phase 6.1: Cypress - Practicals (Hands-on Exercises)

## Practice Website: https://www.saucedemo.com

---

## Exercise 1: Login Tests

```javascript
describe('SauceDemo Login', () => {
  beforeEach(() => {
    cy.visit('https://www.saucedemo.com')
  })

  it('should login with valid credentials', () => {
    cy.get('[data-test="username"]').type('standard_user')
    cy.get('[data-test="password"]').type('secret_sauce')
    cy.get('[data-test="login-button"]').click()
    cy.url().should('include', '/inventory.html')
    cy.get('.title').should('have.text', 'Products')
  })

  it('should show error for locked out user', () => {
    cy.get('[data-test="username"]').type('locked_out_user')
    cy.get('[data-test="password"]').type('secret_sauce')
    cy.get('[data-test="login-button"]').click()
    cy.get('[data-test="error"]')
      .should('be.visible')
      .and('contain', 'locked out')
  })

  it('should show error for empty credentials', () => {
    cy.get('[data-test="login-button"]').click()
    cy.get('[data-test="error"]')
      .should('be.visible')
      .and('contain', 'Username is required')
  })

  it('should show error for empty password', () => {
    cy.get('[data-test="username"]').type('standard_user')
    cy.get('[data-test="login-button"]').click()
    cy.get('[data-test="error"]')
      .should('be.visible')
      .and('contain', 'Password is required')
  })
})
```

---

## Exercise 2: Product Listing

```javascript
describe('Product Listing', () => {
  beforeEach(() => {
    cy.visit('https://www.saucedemo.com')
    cy.get('[data-test="username"]').type('standard_user')
    cy.get('[data-test="password"]').type('secret_sauce')
    cy.get('[data-test="login-button"]').click()
  })

  it('should display 6 products', () => {
    cy.get('.inventory_item').should('have.length', 6)
  })

  it('should display product names and prices', () => {
    cy.get('.inventory_item').each(($item) => {
      cy.wrap($item).find('.inventory_item_name').should('not.be.empty')
      cy.wrap($item).find('.inventory_item_price').should('not.be.empty')
    })
  })

  it('should sort products by price low to high', () => {
    cy.get('[data-test="product-sort-container"]').select('lohi')

    cy.get('.inventory_item_price').then(($prices) => {
      const prices = [...$prices].map(el =>
        parseFloat(el.innerText.replace('$', ''))
      )
      const sorted = [...prices].sort((a, b) => a - b)
      expect(prices).to.deep.equal(sorted)
    })
  })

  it('should sort products by name A to Z', () => {
    cy.get('[data-test="product-sort-container"]').select('az')

    cy.get('.inventory_item_name').then(($names) => {
      const names = [...$names].map(el => el.innerText)
      const sorted = [...names].sort()
      expect(names).to.deep.equal(sorted)
    })
  })
})
```

---

## Exercise 3: Shopping Cart

```javascript
describe('Shopping Cart', () => {
  beforeEach(() => {
    cy.visit('https://www.saucedemo.com')
    cy.get('[data-test="username"]').type('standard_user')
    cy.get('[data-test="password"]').type('secret_sauce')
    cy.get('[data-test="login-button"]').click()
  })

  it('should add item to cart', () => {
    cy.get('[data-test="add-to-cart-sauce-labs-backpack"]').click()
    cy.get('.shopping_cart_badge').should('have.text', '1')
  })

  it('should remove item from cart', () => {
    cy.get('[data-test="add-to-cart-sauce-labs-backpack"]').click()
    cy.get('[data-test="remove-sauce-labs-backpack"]').click()
    cy.get('.shopping_cart_badge').should('not.exist')
  })

  it('should add multiple items and verify cart', () => {
    cy.get('[data-test="add-to-cart-sauce-labs-backpack"]').click()
    cy.get('[data-test="add-to-cart-sauce-labs-bike-light"]').click()
    cy.get('.shopping_cart_badge').should('have.text', '2')

    // Go to cart
    cy.get('.shopping_cart_link').click()
    cy.get('.cart_item').should('have.length', 2)
  })

  it('should complete checkout', () => {
    // Add item
    cy.get('[data-test="add-to-cart-sauce-labs-backpack"]').click()

    // Go to cart
    cy.get('.shopping_cart_link').click()

    // Checkout
    cy.get('[data-test="checkout"]').click()

    // Fill info
    cy.get('[data-test="firstName"]').type('John')
    cy.get('[data-test="lastName"]').type('Doe')
    cy.get('[data-test="postalCode"]').type('12345')
    cy.get('[data-test="continue"]').click()

    // Verify and finish
    cy.get('.summary_total_label').should('contain', '$')
    cy.get('[data-test="finish"]').click()

    // Success
    cy.get('.complete-header').should('have.text', 'Thank you for your order!')
  })
})
```

---

## Exercise 4: Custom Commands

```javascript
// cypress/support/commands.js
Cypress.Commands.add('loginToSauceDemo', (username = 'standard_user') => {
  cy.visit('https://www.saucedemo.com')
  cy.get('[data-test="username"]').type(username)
  cy.get('[data-test="password"]').type('secret_sauce')
  cy.get('[data-test="login-button"]').click()
  cy.url().should('include', '/inventory')
})

Cypress.Commands.add('addToCart', (productName) => {
  const testId = productName.toLowerCase().replace(/ /g, '-')
  cy.get(`[data-test="add-to-cart-${testId}"]`).click()
})

// Usage in tests
describe('Using Custom Commands', () => {
  it('should add products using custom command', () => {
    cy.loginToSauceDemo()
    cy.addToCart('sauce-labs-backpack')
    cy.addToCart('sauce-labs-bike-light')
    cy.get('.shopping_cart_badge').should('have.text', '2')
  })
})
```

---

## Exercise 5: Network Interception

```javascript
describe('Network Interception', () => {
  it('should intercept API calls', () => {
    cy.intercept('GET', 'https://jsonplaceholder.typicode.com/posts').as('getPosts')

    cy.visit('https://jsonplaceholder.typicode.com')
    // Verify the API was called
  })

  it('should stub API response', () => {
    cy.intercept('GET', '**/posts', {
      statusCode: 200,
      body: [
        { id: 1, title: 'Stubbed Post 1' },
        { id: 2, title: 'Stubbed Post 2' }
      ]
    }).as('stubbedPosts')

    cy.visit('https://jsonplaceholder.typicode.com')
  })

  it('should test error scenarios', () => {
    cy.intercept('GET', '**/posts', {
      statusCode: 500,
      body: { error: 'Server Error' }
    }).as('serverError')

    cy.visit('https://jsonplaceholder.typicode.com')
  })
})
```

---

## Exercise 6: Fixtures and Data-Driven Tests

```javascript
// cypress/fixtures/login-data.json
// [
//   { "username": "standard_user", "password": "secret_sauce", "expected": "success" },
//   { "username": "locked_out_user", "password": "secret_sauce", "expected": "error" },
//   { "username": "problem_user", "password": "secret_sauce", "expected": "success" }
// ]

describe('Data-Driven Login Tests', () => {
  beforeEach(() => {
    cy.visit('https://www.saucedemo.com')
  })

  // Load fixture data
  const users = require('../fixtures/login-data.json')

  users.forEach((user) => {
    it(`should test login for ${user.username}`, () => {
      cy.get('[data-test="username"]').type(user.username)
      cy.get('[data-test="password"]').type(user.password)
      cy.get('[data-test="login-button"]').click()

      if (user.expected === 'success') {
        cy.url().should('include', '/inventory')
      } else {
        cy.get('[data-test="error"]').should('be.visible')
      }
    })
  })
})
```

---

## Exercise 7: Responsive Testing

```javascript
describe('Responsive Design Tests', () => {
  const sizes = ['iphone-6', 'ipad-2', [1280, 720]]

  sizes.forEach((size) => {
    it(`should display correctly on ${size}`, () => {
      if (Array.isArray(size)) {
        cy.viewport(size[0], size[1])
      } else {
        cy.viewport(size)
      }

      cy.visit('https://www.saucedemo.com')
      cy.get('[data-test="login-button"]').should('be.visible')
      cy.screenshot(`login-${size}`)
    })
  })
})
```

---

## Exercise 8: API Testing with cy.request()

```javascript
describe('API Testing - JSONPlaceholder', () => {
  const baseUrl = 'https://jsonplaceholder.typicode.com'

  it('GET - should retrieve all posts', () => {
    cy.request(`${baseUrl}/posts`).then((response) => {
      expect(response.status).to.eq(200)
      expect(response.body).to.have.length(100)
      expect(response.body[0]).to.have.property('title')
    })
  })

  it('POST - should create a post', () => {
    cy.request({
      method: 'POST',
      url: `${baseUrl}/posts`,
      body: {
        title: 'Cypress Test Post',
        body: 'This is a test post from Cypress',
        userId: 1
      }
    }).then((response) => {
      expect(response.status).to.eq(201)
      expect(response.body.title).to.eq('Cypress Test Post')
      expect(response.body.id).to.exist
    })
  })

  it('PUT - should update a post', () => {
    cy.request({
      method: 'PUT',
      url: `${baseUrl}/posts/1`,
      body: {
        title: 'Updated Title',
        body: 'Updated body',
        userId: 1
      }
    }).then((response) => {
      expect(response.status).to.eq(200)
      expect(response.body.title).to.eq('Updated Title')
    })
  })

  it('DELETE - should delete a post', () => {
    cy.request('DELETE', `${baseUrl}/posts/1`).then((response) => {
      expect(response.status).to.eq(200)
    })
  })
})
```

---

## Exercise 9: Screenshots and Visual Testing

```javascript
describe('Screenshot Tests', () => {
  it('should capture screenshots at each step', () => {
    cy.visit('https://www.saucedemo.com')
    cy.screenshot('01-login-page')

    cy.get('[data-test="username"]').type('standard_user')
    cy.get('[data-test="password"]').type('secret_sauce')
    cy.screenshot('02-credentials-entered')

    cy.get('[data-test="login-button"]').click()
    cy.screenshot('03-after-login')

    cy.get('.inventory_item').first().screenshot('04-first-product')
  })
})
```

---

## Exercise 10: Complete E2E User Journey

```javascript
describe('Complete E2E - SauceDemo', () => {
  it('should complete full purchase flow', () => {
    // Login
    cy.visit('https://www.saucedemo.com')
    cy.get('[data-test="username"]').type('standard_user')
    cy.get('[data-test="password"]').type('secret_sauce')
    cy.get('[data-test="login-button"]').click()

    // Browse products
    cy.get('.inventory_item').should('have.length', 6)

    // Add 2 items to cart
    cy.get('[data-test="add-to-cart-sauce-labs-backpack"]').click()
    cy.get('[data-test="add-to-cart-sauce-labs-bolt-t-shirt"]').click()
    cy.get('.shopping_cart_badge').should('have.text', '2')

    // View cart
    cy.get('.shopping_cart_link').click()
    cy.url().should('include', '/cart')
    cy.get('.cart_item').should('have.length', 2)

    // Checkout
    cy.get('[data-test="checkout"]').click()
    cy.get('[data-test="firstName"]').type('John')
    cy.get('[data-test="lastName"]').type('Doe')
    cy.get('[data-test="postalCode"]').type('12345')
    cy.get('[data-test="continue"]').click()

    // Review order
    cy.get('.cart_item').should('have.length', 2)
    cy.get('.summary_info').should('be.visible')

    // Complete order
    cy.get('[data-test="finish"]').click()
    cy.get('.complete-header').should('have.text', 'Thank you for your order!')

    // Back to products
    cy.get('[data-test="back-to-products"]').click()
    cy.url().should('include', '/inventory')

    // Logout
    cy.get('#react-burgerMenu-btn').click()
    cy.get('#logout_sidebar_link').click()
    cy.url().should('include', 'saucedemo.com')
  })
})
```

---

## Practice Resources:

1. **https://www.saucedemo.com** - E-commerce demo
2. **https://the-internet.herokuapp.com** - Various test scenarios
3. **https://demoqa.com** - Forms, widgets, interactions
4. **https://jsonplaceholder.typicode.com** - Free REST API
5. **https://reqres.in** - REST API for testing
6. **https://automationexercise.com** - Full e-commerce

---

Happy Testing with Cypress! 🚀
