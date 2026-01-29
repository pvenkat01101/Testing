# Phase 3-06: JavaScript Interview Q&A for Test Automation Engineers

## Table of Contents
1. [JavaScript Basics (Q1-Q5)](#section-1-javascript-basics)
2. [Functions and Closures (Q6-Q10)](#section-2-functions-and-closures)
3. [Objects and Arrays (Q11-Q15)](#section-3-objects-and-arrays)
4. [Asynchronous JavaScript (Q16-Q20)](#section-4-asynchronous-javascript)
5. [ES6+ Features (Q21-Q25)](#section-5-es6-features)
6. [Testing & Cypress (Q26-Q30)](#section-6-testing--cypress)

---

## Section 1: JavaScript Basics

### Q1: Explain the difference between `var`, `let`, and `const`. How does this affect your Cypress tests?

**Answer:**

JavaScript has three ways to declare variables, each with different scoping rules and behaviors:

| Feature | var | let | const |
|---------|-----|-----|-------|
| Scope | Function-scoped | Block-scoped | Block-scoped |
| Hoisting | Hoisted (initialized as undefined) | Hoisted (not initialized) | Hoisted (not initialized) |
| Reassignment | Allowed | Allowed | Not allowed |
| Redeclaration | Allowed | Not allowed | Not allowed |
| Temporal Dead Zone | No | Yes | Yes |

**Code Example:**

```javascript
// var - Function scoped
function testVar() {
  if (true) {
    var x = 10;
  }
  console.log(x); // Output: 10 (accessible outside block)
}

// let - Block scoped
function testLet() {
  if (true) {
    let y = 20;
  }
  console.log(y); // Error: y is not defined
}

// const - Block scoped, immutable binding
function testConst() {
  const z = 30;
  z = 40; // Error: Assignment to constant variable

  const obj = { name: 'John' };
  obj.name = 'Jane'; // Allowed (object properties can change)
  obj = {}; // Error: Assignment to constant variable
}
```

**Cypress Testing Scenario:**

```javascript
describe('Variable Declaration in Cypress', () => {
  it('should demonstrate proper variable usage', () => {
    // BAD: Using var can lead to unexpected behavior
    cy.get('.user-item').each(($el, index) => {
      var userId = $el.attr('data-id'); // var is function-scoped
      cy.wrap(userId).should('not.be.empty');
    });

    // GOOD: Using const/let provides block scoping
    cy.get('.user-item').each(($el, index) => {
      const userId = $el.attr('data-id'); // const is block-scoped
      cy.wrap(userId).should('not.be.empty');
    });
  });

  it('should handle aliases properly', () => {
    cy.get('@userList').then((users) => {
      const userCount = users.length; // Use const for values that won't change
      let processedCount = 0; // Use let for counters

      users.each((index, user) => {
        processedCount++;
      });

      expect(processedCount).to.equal(userCount);
    });
  });
});
```

**Common Mistakes to Avoid:**

1. Using `var` in loops with asynchronous operations
2. Trying to reassign `const` variables
3. Forgetting that `const` doesn't make objects immutable

**Interview Tips:**

- Always prefer `const` by default
- Use `let` only when you need to reassign
- Avoid `var` in modern JavaScript
- Understand the temporal dead zone concept

**Tricky Follow-up Questions:**

**Q: What is hoisting and how does it differ between var, let, and const?**

A: Hoisting is JavaScript's behavior of moving declarations to the top of their scope during compilation. `var` declarations are hoisted and initialized with `undefined`. `let` and `const` are hoisted but not initialized, creating a "temporal dead zone" from the start of the block until the declaration is encountered.

```javascript
console.log(a); // undefined (var is hoisted and initialized)
var a = 5;

console.log(b); // ReferenceError: Cannot access 'b' before initialization
let b = 10;
```

---

### Q2: What are JavaScript data types? Explain primitive vs reference types with Cypress examples.

**Answer:**

JavaScript has 8 data types: 7 primitive types and 1 reference type.

**Primitive Types (Immutable):**
1. `String`
2. `Number`
3. `BigInt`
4. `Boolean`
5. `Undefined`
6. `Null`
7. `Symbol`

**Reference Type (Mutable):**
8. `Object` (includes Arrays, Functions, Date, etc.)

**Key Differences:**

| Aspect | Primitive Types | Reference Types |
|--------|----------------|-----------------|
| Storage | Stored in stack | Stored in heap, reference in stack |
| Comparison | By value | By reference |
| Mutability | Immutable | Mutable |
| Copy behavior | Creates new copy | Copies reference |

**Code Example:**

```javascript
// Primitive types - compared by value
let str1 = "hello";
let str2 = "hello";
console.log(str1 === str2); // Output: true

let num1 = 42;
let num2 = num1;
num2 = 100;
console.log(num1); // Output: 42 (original unchanged)

// Reference types - compared by reference
let obj1 = { name: "John" };
let obj2 = { name: "John" };
console.log(obj1 === obj2); // Output: false (different references)

let obj3 = obj1;
obj3.name = "Jane";
console.log(obj1.name); // Output: "Jane" (both point to same object)

// Array behavior
let arr1 = [1, 2, 3];
let arr2 = arr1;
arr2.push(4);
console.log(arr1); // Output: [1, 2, 3, 4] (original modified)
```

**Type Checking:**

```javascript
// typeof operator
console.log(typeof "hello");        // "string"
console.log(typeof 42);             // "number"
console.log(typeof true);           // "boolean"
console.log(typeof undefined);      // "undefined"
console.log(typeof null);           // "object" (JavaScript quirk!)
console.log(typeof Symbol());       // "symbol"
console.log(typeof 123n);           // "bigint"
console.log(typeof {});             // "object"
console.log(typeof []);             // "object"
console.log(typeof function(){});   // "function"

// Better array checking
console.log(Array.isArray([]));     // true
```

**Cypress Testing Scenario:**

```javascript
describe('Data Types in Test Automation', () => {
  it('should handle primitive type comparisons correctly', () => {
    cy.get('[data-testid="price"]').invoke('text').then((price) => {
      const priceValue = parseFloat(price); // Convert string to number
      expect(typeof priceValue).to.equal('number');
      expect(priceValue).to.be.greaterThan(0);
    });
  });

  it('should properly clone objects to avoid reference issues', () => {
    const testData = {
      username: 'testuser',
      email: 'test@example.com'
    };

    // BAD: Direct reference (modifies original)
    const userData1 = testData;
    userData1.username = 'modified';
    console.log(testData.username); // Output: "modified"

    // GOOD: Shallow copy
    const userData2 = { ...testData };
    userData2.username = 'newuser';
    console.log(testData.username); // Output: "testuser"

    // GOOD: Deep copy for nested objects
    const complexData = {
      user: { name: 'John', address: { city: 'NYC' } }
    };
    const clonedData = JSON.parse(JSON.stringify(complexData));
    clonedData.user.address.city = 'LA';
    console.log(complexData.user.address.city); // Output: "NYC"
  });

  it('should validate data types in API responses', () => {
    cy.request('GET', '/api/users/1').then((response) => {
      expect(response.status).to.be.a('number');
      expect(response.body.id).to.be.a('number');
      expect(response.body.name).to.be.a('string');
      expect(response.body.isActive).to.be.a('boolean');
      expect(Array.isArray(response.body.roles)).to.be.true;
    });
  });

  it('should handle null vs undefined in element checks', () => {
    cy.get('body').then(($body) => {
      // undefined - property doesn't exist
      const nonExistent = $body.attr('data-undefined');
      expect(nonExistent).to.be.undefined;

      // null - intentional absence of value
      const nullValue = null;
      expect(nullValue).to.be.null;
    });
  });
});
```

**Common Mistakes to Avoid:**

1. Using `typeof null` expecting "null" (it returns "object")
2. Comparing objects with `===` expecting value comparison
3. Modifying reference types without realizing it affects the original
4. Not using `Array.isArray()` for array checking

**Interview Tips:**

- Know the quirk: `typeof null === "object"`
- Understand shallow vs deep copy
- Be aware of coercion in comparisons (`==` vs `===`)
- Know when to use `Object.is()` for special cases

**Tricky Follow-up Questions:**

**Q: What's the difference between `null` and `undefined`?**

A:
- `undefined`: Variable declared but not assigned, or property doesn't exist
- `null`: Intentional absence of value, explicitly set by programmer

```javascript
let a;
console.log(a); // undefined

let b = null;
console.log(b); // null

let obj = { name: 'John' };
console.log(obj.age); // undefined (property doesn't exist)
```

**Q: How do you properly clone nested objects?**

A:
```javascript
// Shallow copy - only copies first level
const shallow = { ...original };
const shallow2 = Object.assign({}, original);

// Deep copy options:
// 1. JSON method (doesn't handle functions, dates, undefined)
const deep1 = JSON.parse(JSON.stringify(original));

// 2. Structured clone (modern browsers)
const deep2 = structuredClone(original);

// 3. Lodash library
const deep3 = _.cloneDeep(original);
```

---

### Q3: Explain == vs === and type coercion. Why is this important in test assertions?

**Answer:**

JavaScript has two equality operators with different behaviors:

| Operator | Name | Type Coercion | Strict |
|----------|------|---------------|--------|
| == | Loose equality | Yes | No |
| === | Strict equality | No | Yes |

**Type Coercion Rules for ==:**

```javascript
// Number comparisons
console.log(5 == "5");        // true (string coerced to number)
console.log(5 === "5");       // false (different types)

// Boolean coercion
console.log(1 == true);       // true (true coerced to 1)
console.log(0 == false);      // true (false coerced to 0)
console.log(1 === true);      // false (different types)

// Null and undefined
console.log(null == undefined);   // true (special rule)
console.log(null === undefined);  // false (different types)

// Object to primitive
console.log([1] == 1);        // true (array coerced to "1", then to 1)
console.log([1] === 1);       // false (different types)

// Empty string coercion
console.log("" == 0);         // true (empty string coerced to 0)
console.log("" === 0);        // false (different types)

// Falsy values
console.log(false == 0);      // true
console.log(false == "");     // true
console.log(0 == "");         // true
console.log(false === 0);     // false
```

**Falsy Values in JavaScript:**

```javascript
// These 6 values are falsy:
false
0
"" (empty string)
null
undefined
NaN

// Everything else is truthy, including:
"0"          // non-empty string
[]           // empty array
{}           // empty object
function(){} // functions
```

**Cypress Testing Scenario:**

```javascript
describe('Equality and Type Coercion in Tests', () => {
  it('should use strict equality in assertions', () => {
    cy.get('[data-testid="count"]').invoke('text').then((text) => {
      const count = parseInt(text);

      // BAD: Loose equality can lead to false positives
      expect(count == "5").to.be.true;  // Passes even if types differ

      // GOOD: Strict equality catches type mismatches
      expect(count).to.equal(5);        // Uses === internally
      expect(count).to.strictly.equal(5);
    });
  });

  it('should handle API response validation correctly', () => {
    cy.request('/api/status').then((response) => {
      const { status, isActive, count } = response.body;

      // Strict type checking
      expect(status).to.equal(200);           // Not "200"
      expect(isActive).to.be.true;            // Not 1 or "true"
      expect(count).to.be.a('number');

      // Avoid loose equality bugs
      if (isActive === true) {  // Good
        cy.log('User is active');
      }

      if (isActive == 1) {      // Bad - relies on coercion
        cy.log('This might pass unexpectedly');
      }
    });
  });

  it('should handle null and undefined checks properly', () => {
    cy.get('body').then(($body) => {
      const attr = $body.attr('data-optional');

      // Check for null or undefined
      if (attr == null) {  // Catches both null and undefined
        cy.log('Attribute is null or undefined');
      }

      // Distinguish between null and undefined
      if (attr === undefined) {
        cy.log('Attribute does not exist');
      }

      if (attr === null) {
        cy.log('Attribute explicitly set to null');
      }
    });
  });

  it('should avoid coercion pitfalls in conditionals', () => {
    const testCases = [
      { value: 0, expected: 'zero' },
      { value: '', expected: 'empty' },
      { value: false, expected: 'false' }
    ];

    testCases.forEach(({ value, expected }) => {
      // BAD: All falsy values treated the same
      if (!value) {
        cy.log(`Value is falsy: ${expected}`);
      }

      // GOOD: Explicit checks
      if (value === 0) {
        cy.log('Value is specifically zero');
      } else if (value === '') {
        cy.log('Value is specifically empty string');
      } else if (value === false) {
        cy.log('Value is specifically false');
      }
    });
  });

  it('should validate form input types correctly', () => {
    cy.get('[data-testid="age-input"]').invoke('val').then((value) => {
      // Input values are always strings!
      expect(typeof value).to.equal('string');

      // Convert and validate
      const age = Number(value);
      expect(age).to.be.a('number');
      expect(age).to.be.greaterThan(0);

      // BAD: Comparing string with number
      if (value == 18) {  // Works but not recommended
        cy.log('Age is 18');
      }

      // GOOD: Convert first, then compare
      if (Number(value) === 18) {
        cy.log('Age is exactly 18');
      }
    });
  });
});
```

**Type Coercion Examples:**

```javascript
// Surprising coercions
console.log([] + []);           // "" (empty string)
console.log([] + {});           // "[object Object]"
console.log({} + []);           // "[object Object]"
console.log(true + false);      // 1
console.log("5" + 3);           // "53" (string concatenation)
console.log("5" - 3);           // 2 (numeric subtraction)
console.log("5" * "2");         // 10 (numeric multiplication)

// Comparison quirks
console.log([1, 2] == "1,2");   // true
console.log([5] < [6]);         // true (both converted to strings)
```

**Common Mistakes to Avoid:**

1. Using `==` in test assertions (always use `===` or Cypress's `.equal()`)
2. Not converting input values before comparison
3. Relying on implicit coercion in conditionals
4. Forgetting that `null == undefined` is true

**Interview Tips:**

- Always prefer `===` over `==`
- Know the 6 falsy values by heart
- Understand that `null == undefined` but `null !== undefined`
- Be explicit about type conversions in your code

**Tricky Follow-up Questions:**

**Q: When would you use == instead of ===?**

A: The only acceptable use case is checking for both `null` and `undefined`:

```javascript
if (value == null) {  // Catches both null and undefined
  // Handle missing value
}

// This is cleaner than:
if (value === null || value === undefined) {
  // Handle missing value
}
```

**Q: What is the output of these comparisons?**

```javascript
console.log([] == ![]);    // true (very tricky!)
// Explanation:
// ![] is false (empty array is truthy)
// [] == false
// "" == false (array to string)
// 0 == false (string to number)
// 0 == 0 → true

console.log(null > 0);     // false
console.log(null == 0);    // false
console.log(null >= 0);    // true (WTF!)
// Explanation: >= uses different coercion rules than ==
```

---

### Q4: What is scope in JavaScript? Explain global, function, and block scope with testing examples.

**Answer:**

Scope determines the accessibility of variables, functions, and objects in your code. JavaScript has three types of scope:

**Scope Types:**

| Scope Type | Created By | Accessible From | Variables |
|------------|-----------|-----------------|-----------|
| Global | Outside all functions | Everywhere | var, let, const, functions |
| Function | Function declaration | Inside function only | var, let, const, parameters |
| Block | {} curly braces | Inside block only | let, const (not var!) |

**Code Example:**

```javascript
// GLOBAL SCOPE
const globalVar = "I'm global";
var globalVar2 = "Also global";

function demonstrateScope() {
  // FUNCTION SCOPE
  var functionVar = "Function scoped";
  const functionConst = "Also function scoped";

  console.log(globalVar);      // Accessible
  console.log(functionVar);    // Accessible

  if (true) {
    // BLOCK SCOPE
    let blockLet = "Block scoped";
    const blockConst = "Also block scoped";
    var notBlockScoped = "Function scoped!";

    console.log(blockLet);       // Accessible
    console.log(functionVar);    // Accessible
    console.log(globalVar);      // Accessible
  }

  console.log(notBlockScoped);  // Accessible (var is function-scoped)
  console.log(blockLet);        // Error: blockLet is not defined
}

console.log(functionVar);       // Error: functionVar is not defined
```

**Scope Chain:**

```javascript
const a = "global";

function outer() {
  const b = "outer";

  function inner() {
    const c = "inner";

    console.log(a); // "global" (found in global scope)
    console.log(b); // "outer" (found in outer function scope)
    console.log(c); // "inner" (found in current scope)
  }

  inner();
  console.log(c); // Error: c is not defined
}

outer();
```

**Cypress Testing Scenario:**

```javascript
// Global test configuration
const BASE_URL = 'https://example.com';
const DEFAULT_TIMEOUT = 10000;

describe('Scope in Cypress Tests', () => {
  // Suite-level scope
  let testUser;
  const testConfig = {
    retries: 3,
    viewport: 'iphone-x'
  };

  before(() => {
    // Runs once before all tests
    testUser = {
      email: 'test@example.com',
      password: 'password123'
    };
  });

  beforeEach(() => {
    // Function scope - only accessible in beforeEach
    const timestamp = Date.now();
    cy.log(`Test started at: ${timestamp}`);

    // testUser is accessible (suite-level scope)
    cy.visit(BASE_URL);
  });

  it('should demonstrate proper variable scoping', () => {
    // Test function scope
    const testData = { username: 'testuser' };

    cy.get('[data-testid="username"]').type(testData.username);

    // Block scope in conditional
    if (testUser.email) {
      let emailField = '[data-testid="email"]';
      const emailValue = testUser.email;

      cy.get(emailField).type(emailValue);
      // emailField and emailValue accessible here
    }

    // emailField not accessible here (block scope)
    // console.log(emailField); // Error

    // testUser accessible (suite-level scope)
    cy.get('[data-testid="password"]').type(testUser.password);
  });

  it('should avoid scope pollution', () => {
    // BAD: Accidentally creating global variable
    function badPractice() {
      undeclaredVar = "I'm global now!"; // No var/let/const
      window.anotherGlobal = "Also global";
    }

    // GOOD: Proper scoping
    function goodPractice() {
      const localVar = "I'm local";
      let anotherLocal = "Also local";
    }

    goodPractice();
    // localVar not accessible here
  });

  it('should handle closure scope correctly', () => {
    const users = ['user1', 'user2', 'user3'];

    // BAD: Using var in loops
    for (var i = 0; i < users.length; i++) {
      cy.get(`.user-${i}`).click().then(() => {
        cy.log(`Clicked user ${i}`); // i is always 3!
      });
    }

    // GOOD: Using let in loops
    for (let j = 0; j < users.length; j++) {
      cy.get(`.user-${j}`).click().then(() => {
        cy.log(`Clicked user ${j}`); // j has correct value
      });
    }

    // ALTERNATIVE: Using forEach
    users.forEach((user, index) => {
      cy.get(`.user-${index}`).click().then(() => {
        cy.log(`Clicked ${user}`);
      });
    });
  });

  it('should use aliases to avoid scope issues', () => {
    // Cypress aliases help manage scope
    cy.get('[data-testid="user-list"]').as('userList');

    cy.get('@userList').find('.user-item').then(($items) => {
      const itemCount = $items.length;

      // Use alias in other commands
      cy.get('@userList').should('have.length', itemCount);
    });

    // Alias accessible in other tests too
    cy.get('@userList').should('be.visible');
  });

  it('should manage scope with custom commands', () => {
    // Custom command has its own scope
    Cypress.Commands.add('loginUser', (credentials) => {
      const { email, password } = credentials; // Function parameter scope

      cy.visit('/login');
      cy.get('[data-testid="email"]').type(email);
      cy.get('[data-testid="password"]').type(password);
      cy.get('[type="submit"]').click();

      // Return value escapes function scope
      return cy.get('[data-testid="user-profile"]');
    });

    // Use custom command
    cy.loginUser(testUser).should('be.visible');
  });
});

// Page Object Pattern - Managing scope effectively
class LoginPage {
  // Class-level properties (instance scope)
  constructor() {
    this.emailInput = '[data-testid="email"]';
    this.passwordInput = '[data-testid="password"]';
    this.submitButton = '[type="submit"]';
  }

  // Method scope
  login(email, password) {
    // Method parameters are function-scoped
    cy.get(this.emailInput).type(email);
    cy.get(this.passwordInput).type(password);
    cy.get(this.submitButton).click();

    // Return for method chaining
    return this;
  }

  verifyLogin() {
    // this.emailInput accessible (instance scope)
    cy.url().should('include', '/dashboard');
    return this;
  }
}

describe('Using Page Objects with Proper Scope', () => {
  it('should login using page object', () => {
    const loginPage = new LoginPage(); // Test function scope

    loginPage
      .login('test@example.com', 'password123')
      .verifyLogin();
  });
});
```

**Lexical Scope Example:**

```javascript
function createCounter() {
  let count = 0; // Enclosed in outer function scope

  return {
    increment: function() {
      count++; // Accesses outer scope variable
      return count;
    },
    decrement: function() {
      count--;
      return count;
    },
    getCount: function() {
      return count;
    }
  };
}

const counter = createCounter();
console.log(counter.increment()); // 1
console.log(counter.increment()); // 2
console.log(counter.getCount());  // 2
console.log(count);               // Error: count is not defined
```

**Common Mistakes to Avoid:**

1. Accidentally creating global variables by omitting declarations
2. Using `var` in loops with asynchronous callbacks
3. Not understanding closure scope in callbacks
4. Polluting global scope in test files

**Interview Tips:**

- Understand the scope chain and how JavaScript resolves variables
- Know that `var` is function-scoped while `let`/`const` are block-scoped
- Be familiar with lexical scoping and closures
- Use strict mode (`'use strict'`) to catch scope-related errors

**Tricky Follow-up Questions:**

**Q: What will this code output and why?**

```javascript
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
// Output: 3, 3, 3

for (let j = 0; j < 3; j++) {
  setTimeout(() => console.log(j), 100);
}
// Output: 0, 1, 2

// Explanation: var is function-scoped, let is block-scoped
```

**Q: How does JavaScript resolve variable names?**

A: JavaScript uses lexical scoping and follows these steps:
1. Look in the current scope
2. If not found, look in the outer scope
3. Continue up the scope chain until global scope
4. If still not found, throw ReferenceError (or create global in non-strict mode)

```javascript
const x = 10;

function outer() {
  const x = 20;

  function inner() {
    const x = 30;
    console.log(x); // 30 (current scope)
  }

  inner();
  console.log(x); // 20 (outer function scope)
}

outer();
console.log(x); // 10 (global scope)
```

---

### Q5: Explain truthy and falsy values. How do you handle them in test conditions?

**Answer:**

In JavaScript, values are coerced to boolean in conditional contexts. Understanding truthy and falsy values is crucial for writing reliable test conditions.

**Falsy Values (Only 6):**

```javascript
false         // Boolean false
0             // Number zero
-0            // Negative zero
0n            // BigInt zero
""            // Empty string
null          // Null
undefined     // Undefined
NaN           // Not a Number
```

**Truthy Values (Everything Else):**

```javascript
true          // Boolean true
1, -1, 100    // Any non-zero number
"0"           // Non-empty string (even "false")
"false"       // String "false"
[]            // Empty array
{}            // Empty object
function(){}  // Functions
new Date()    // Date objects
Infinity      // Infinity
-Infinity     // Negative Infinity
```

**Code Example:**

```javascript
// Boolean context conversions
if ("hello") {
  console.log("Truthy"); // Executes
}

if (0) {
  console.log("Won't execute"); // 0 is falsy
}

if ([]) {
  console.log("Empty array is truthy"); // Executes
}

// Explicit boolean conversion
console.log(Boolean(0));          // false
console.log(Boolean(""));         // false
console.log(Boolean("0"));        // true
console.log(Boolean([]));         // true
console.log(Boolean({}));         // true

// Double negation (!! operator)
console.log(!!0);                 // false
console.log(!!"hello");           // true
console.log(!![]);                // true

// Nullish vs falsy
const value1 = 0;
const value2 = null;

console.log(value1 || "default");     // "default" (0 is falsy)
console.log(value2 || "default");     // "default" (null is falsy)

console.log(value1 ?? "default");     // 0 (0 is not nullish)
console.log(value2 ?? "default");     // "default" (null is nullish)
```

**Cypress Testing Scenario:**

```javascript
describe('Truthy and Falsy in Test Automation', () => {
  it('should handle existence checks properly', () => {
    cy.get('body').then(($body) => {
      // BAD: Truthy check might not mean what you think
      const attr = $body.attr('data-count');

      if (attr) {
        cy.log('Attribute exists');
      }
      // Problem: If attr is "0", this block won't execute!

      // GOOD: Explicit null/undefined check
      if (attr !== null && attr !== undefined) {
        cy.log('Attribute definitely exists');
      }

      // GOOD: Check for null/undefined
      if (attr != null) {  // Catches both null and undefined
        cy.log('Attribute exists');
      }
    });
  });

  it('should validate numeric values correctly', () => {
    cy.request('/api/cart').then((response) => {
      const { itemCount, totalPrice } = response.body;

      // BAD: Falsy check fails for zero
      if (itemCount) {
        cy.log(`Cart has ${itemCount} items`);
      } else {
        cy.log('Cart is empty'); // Executes even if itemCount is 0!
      }

      // GOOD: Explicit comparison
      if (itemCount > 0) {
        cy.log(`Cart has ${itemCount} items`);
      } else if (itemCount === 0) {
        cy.log('Cart is empty');
      } else {
        cy.log('Invalid item count');
      }

      // Handle zero price
      if (totalPrice === 0) {
        cy.log('Total is zero');
      } else if (totalPrice) {
        cy.log(`Total: $${totalPrice}`);
      }
    });
  });

  it('should use default values appropriately', () => {
    const config = {
      timeout: 0,
      retries: null,
      baseUrl: '',
      headers: undefined
    };

    // BAD: OR operator treats all falsy values the same
    const timeout1 = config.timeout || 5000;      // 5000 (wrong! timeout was 0)
    const retries1 = config.retries || 3;         // 3
    const baseUrl1 = config.baseUrl || 'http://localhost'; // 'http://localhost'

    // GOOD: Nullish coalescing for null/undefined only
    const timeout2 = config.timeout ?? 5000;      // 0 (correct!)
    const retries2 = config.retries ?? 3;         // 3
    const baseUrl2 = config.baseUrl ?? 'http://localhost'; // '' (empty string is not nullish)

    // When you want to treat empty string as falsy
    const baseUrl3 = config.baseUrl || 'http://localhost'; // 'http://localhost'

    cy.log(`timeout: ${timeout2}, retries: ${retries2}`);
  });

  it('should handle array and object checks', () => {
    cy.request('/api/users').then((response) => {
      const users = response.body;

      // BAD: Empty array is truthy
      if (users) {
        cy.log('Users exist'); // Always executes, even for []!
      }

      // GOOD: Check array length
      if (users.length > 0) {
        cy.log(`Found ${users.length} users`);
        users.forEach(user => {
          expect(user.id).to.exist;
        });
      } else {
        cy.log('No users found');
      }

      // Check if array
      if (Array.isArray(users) && users.length > 0) {
        cy.log('Valid non-empty array');
      }
    });
  });

  it('should validate form inputs with truthy/falsy awareness', () => {
    cy.visit('/form');

    cy.get('[data-testid="age-input"]').invoke('val').then((age) => {
      // Input values are strings!

      // BAD: Empty string is falsy
      if (age) {
        cy.log(`Age is: ${age}`);
      } else {
        cy.log('No age entered'); // Correct for empty string
      }

      // BAD: "0" is truthy (it's a string!)
      if (age) {
        cy.log('Age exists'); // Executes even for "0"!
      }

      // GOOD: Convert and compare
      const ageNum = Number(age);
      if (ageNum > 0) {
        cy.log(`Valid age: ${ageNum}`);
      } else if (ageNum === 0) {
        cy.log('Age is zero');
      } else if (isNaN(ageNum)) {
        cy.log('Invalid age input');
      }
    });
  });

  it('should handle optional chaining with nullish coalescing', () => {
    const response = {
      data: {
        user: {
          name: 'John',
          age: 0,
          address: null
        }
      }
    };

    // Optional chaining to avoid null/undefined errors
    const userName = response?.data?.user?.name ?? 'Anonymous';
    const userAge = response?.data?.user?.age ?? 18;
    const userAddress = response?.data?.user?.address ?? 'No address';
    const userPhone = response?.data?.user?.phone ?? 'No phone';

    cy.log(`Name: ${userName}`);      // "John"
    cy.log(`Age: ${userAge}`);        // 0 (not 18!)
    cy.log(`Address: ${userAddress}`);// "No address"
    cy.log(`Phone: ${userPhone}`);    // "No phone"

    // Compare with OR operator
    const userAge2 = response?.data?.user?.age || 18;
    cy.log(`Age with ||: ${userAge2}`); // 18 (wrong! age was 0)
  });

  it('should use explicit checks in assertions', () => {
    cy.get('[data-testid="status"]').invoke('text').then((status) => {
      // BAD: Chai's .ok checks for truthy
      expect(status).to.be.ok; // Passes for any truthy value

      // GOOD: Explicit assertions
      expect(status).to.not.be.null;
      expect(status).to.not.be.undefined;
      expect(status).to.be.a('string');
      expect(status.length).to.be.greaterThan(0);
    });

    cy.get('[data-testid="count"]').invoke('text').then((count) => {
      const num = parseInt(count);

      // BAD: Truthy check fails for 0
      if (num) {
        expect(num).to.be.greaterThan(0);
      }

      // GOOD: Explicit check allows 0
      if (num >= 0) {
        expect(num).to.be.at.least(0);
      }
    });
  });
});

// Utility functions for handling truthy/falsy
const testUtils = {
  // Check if value is null or undefined
  isNullish: (value) => value == null,

  // Check if value exists and is not empty
  hasValue: (value) => {
    if (value == null) return false;
    if (typeof value === 'string') return value.length > 0;
    if (Array.isArray(value)) return value.length > 0;
    if (typeof value === 'object') return Object.keys(value).length > 0;
    return true;
  },

  // Safe default with type checking
  getDefault: (value, defaultValue) => {
    return value ?? defaultValue;
  },

  // Convert to boolean explicitly
  toBoolean: (value) => Boolean(value)
};

describe('Using Utility Functions', () => {
  it('should use utility functions for clarity', () => {
    const testCases = [
      { value: 0, expected: true },
      { value: '', expected: false },
      { value: null, expected: true },
      { value: undefined, expected: true },
      { value: [], expected: false },
      { value: {}, expected: false }
    ];

    testCases.forEach(({ value, expected }) => {
      const isNull = testUtils.isNullish(value);
      cy.log(`isNullish(${JSON.stringify(value)}): ${isNull}`);

      const hasVal = testUtils.hasValue(value);
      cy.log(`hasValue(${JSON.stringify(value)}): ${hasVal}`);
    });
  });
});
```

**Comparison Table:**

| Expression | Result | Reason |
|------------|--------|--------|
| `0 \|\| 5` | 5 | 0 is falsy |
| `0 ?? 5` | 0 | 0 is not nullish |
| `'' \|\| 'default'` | 'default' | '' is falsy |
| `'' ?? 'default'` | '' | '' is not nullish |
| `false \|\| true` | true | false is falsy |
| `false ?? true` | false | false is not nullish |
| `null \|\| 'default'` | 'default' | null is falsy & nullish |
| `null ?? 'default'` | 'default' | null is nullish |
| `[] \|\| [1]` | [] | [] is truthy |
| `!![]` | true | [] is truthy |
| `[] == false` | true | Coercion: [] → "" → 0 → false |

**Common Mistakes to Avoid:**

1. Using truthy checks when you specifically want to check for null/undefined
2. Forgetting that 0, "", and false are falsy but might be valid values
3. Using `||` for default values when the value could legitimately be 0 or ""
4. Assuming empty arrays/objects are falsy (they're truthy!)

**Interview Tips:**

- Memorize the 6 falsy values
- Understand the difference between `||` and `??`
- Use explicit comparisons in test assertions
- Know that empty arrays and objects are truthy

**Tricky Follow-up Questions:**

**Q: What's the difference between `||`, `&&`, and `??` operators?**

A:
```javascript
// || (OR) - returns first truthy value or last value
console.log(0 || 1);              // 1
console.log(null || undefined);   // undefined

// && (AND) - returns first falsy value or last value
console.log(0 && 1);              // 0
console.log(1 && 2);              // 2

// ?? (Nullish coalescing) - returns right side only if left is null/undefined
console.log(0 ?? 1);              // 0
console.log(null ?? 1);           // 1
console.log(undefined ?? 1);      // 1
```

**Q: Why is `[] == ![]` true?**

A:
```javascript
// Step by step evaluation:
[] == ![]
[] == false          // ![] is false (array is truthy)
"" == false          // [] converts to ""
0 == false           // "" converts to 0
0 == 0               // false converts to 0
true                 // Result
```

This demonstrates why you should always use `===` instead of `==`!

---


## Section 2: Functions and Closures

### Q6: Explain different types of functions in JavaScript. Which ones are commonly used in Cypress tests?

**Answer:**

JavaScript has multiple ways to define functions, each with different characteristics:

**Function Types:**

| Type | Syntax | `this` Binding | Hoisting | Arguments Object |
|------|--------|----------------|----------|------------------|
| Function Declaration | `function name() {}` | Dynamic | Yes (full) | Yes |
| Function Expression | `const fn = function() {}` | Dynamic | No | Yes |
| Arrow Function | `const fn = () => {}` | Lexical | No | No |
| Method Shorthand | `obj = { method() {} }` | Dynamic | N/A | Yes |
| Constructor Function | `function Person() {}` | Instance | Yes | Yes |
| Generator Function | `function* gen() {}` | Dynamic | Yes | Yes |
| Async Function | `async function fn() {}` | Dynamic | Yes | Yes |

**1. Function Declaration:**

```javascript
// Hoisted - can be called before declaration
greet('John'); // Works!

function greet(name) {
  return `Hello, ${name}!`;
}

console.log(greet('Jane')); // Output: "Hello, Jane!"
```

**2. Function Expression:**

```javascript
// Not hoisted - cannot be called before declaration
// sayHello('John'); // Error: Cannot access before initialization

const sayHello = function(name) {
  return `Hello, ${name}!`;
};

console.log(sayHello('Jane')); // Output: "Hello, Jane!"

// Named function expression (useful for recursion and debugging)
const factorial = function fact(n) {
  if (n <= 1) return 1;
  return n * fact(n - 1); // Can use 'fact' internally
};

console.log(factorial(5)); // Output: 120
```

**3. Arrow Functions:**

```javascript
// Concise syntax
const add = (a, b) => a + b;
console.log(add(2, 3)); // Output: 5

// With block body
const multiply = (a, b) => {
  const result = a * b;
  return result;
};

// Lexical 'this' - doesn't have its own 'this'
const obj = {
  value: 10,
  regularFunction: function() {
    console.log(this.value); // 10
  },
  arrowFunction: () => {
    console.log(this.value); // undefined (lexical this)
  }
};

// Single parameter - parentheses optional
const square = x => x * x;
console.log(square(4)); // Output: 16

// No parameters - parentheses required
const getRandom = () => Math.random();
```

**Cypress Testing Scenario:**

```javascript
describe('Function Types in Cypress', () => {
  // Function declaration - available throughout describe block
  function validateEmail(email) {
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    return emailRegex.test(email);
  }

  // Arrow function for test cases (common practice)
  it('should demonstrate arrow functions in tests', () => {
    cy.visit('/login');

    // Arrow functions in then() callbacks
    cy.get('[data-testid="email"]').then(($el) => {
      const email = $el.val();
      expect(validateEmail(email)).to.be.true;
    });

    // Arrow functions in each() iteration
    cy.get('.user-item').each(($item, index) => {
      cy.wrap($item).should('have.attr', 'data-index', index.toString());
    });
  });

  it('should understand "this" binding differences', () => {
    // Regular function - 'this' is the test context
    cy.wrap({ name: 'test' }).as('testData');

    cy.get('@testData').then(function(data) {
      // 'this' refers to test context
      expect(this.testData).to.exist;
      expect(data.name).to.equal('test');
    });

    // Arrow function - 'this' is lexical (from outer scope)
    cy.get('@testData').then((data) => {
      // 'this' is NOT the test context
      expect(data.name).to.equal('test');
    });
  });
});
```

**Common Mistakes to Avoid:**

1. Using arrow functions as methods when you need `this`
2. Expecting `arguments` object in arrow functions
3. Using arrow functions as constructors
4. Not understanding hoisting differences

**Interview Tips:**

- Use arrow functions for callbacks and short utilities
- Use regular functions when you need `this` binding or `arguments`
- Prefer `const` with function expressions over `var`
- Understand when to use each function type

---

### Q7: What are closures? Provide practical examples in test automation.

**Answer:**

A closure is a function that has access to variables in its outer (enclosing) lexical scope, even after the outer function has returned.

**How Closures Work:**

```javascript
function outer() {
  const outerVar = 'I am from outer scope';

  function inner() {
    console.log(outerVar); // Can access outerVar
  }

  return inner;
}

const closureFunc = outer();
closureFunc(); // Output: "I am from outer scope"
```

**Classic Closure Examples:**

```javascript
// Counter with private state
function createCounter() {
  let count = 0; // Private variable

  return {
    increment: function() {
      count++;
      return count;
    },
    decrement: function() {
      count--;
      return count;
    },
    getCount: function() {
      return count;
    }
  };
}

const counter = createCounter();
console.log(counter.increment()); // 1
console.log(counter.increment()); // 2
console.log(counter.getCount());  // 2
console.log(counter.count);       // undefined (private!)
```

**Cypress Testing Scenario:**

```javascript
describe('Closures in Test Automation', () => {
  // Test data factory with closure
  const createTestUser = () => {
    let userCount = 0;

    return {
      generate: () => {
        userCount++;
        return {
          id: userCount,
          username: `testuser${userCount}`,
          email: `testuser${userCount}@example.com`,
          password: 'Test123!'
        };
      },
      getCount: () => userCount,
      reset: () => { userCount = 0; }
    };
  };

  it('should use closure-based test data generator', () => {
    const userFactory = createTestUser();

    const user1 = userFactory.generate();
    const user2 = userFactory.generate();

    expect(user1.id).to.equal(1);
    expect(user2.id).to.equal(2);
    expect(userFactory.getCount()).to.equal(2);
  });

  // Event handler with closure (common pitfall)
  it('should handle closures in loops correctly', () => {
    const buttons = ['btn1', 'btn2', 'btn3'];

    // BAD: Using var creates closure over same variable
    for (var i = 0; i < buttons.length; i++) {
      cy.get(`[data-testid="${buttons[i]}"]`).click().then(() => {
        cy.log(`Clicked button ${i}`); // i is always 3!
      });
    }

    // GOOD: Using let creates new binding for each iteration
    for (let j = 0; j < buttons.length; j++) {
      cy.get(`[data-testid="${buttons[j]}"]`).click().then(() => {
        cy.log(`Clicked button ${j}`); // Correct value
      });
    }
  });
});
```

**Common Mistakes to Avoid:**

1. Creating closures in loops with `var` (use `let` or IIFE)
2. Closing over large objects unnecessarily (memory leaks)
3. Not understanding that closures can cause memory retention

**Interview Tips:**

- Closures give functions a "memory" of their creation environment
- Useful for data privacy, factories, and partial application
- Be aware of memory implications


---

### Q8: Explain the 'this' keyword in JavaScript. How does it behave in Cypress tests?

**Answer:**

The `this` keyword refers to the context in which a function is executed. Its value depends on how the function is called, not where it's defined.

**'this' Binding Rules:**

| Context | 'this' refers to |
|---------|-----------------|
| Global scope | `window` (browser) or `global` (Node.js) |
| Regular function call | `window` (non-strict) or `undefined` (strict) |
| Method call | Object that owns the method |
| Constructor | New instance being created |
| Arrow function | Lexical context (inherited) |
| `call()/apply()/bind()` | Explicitly set object |
| Event handler | Element that received the event |

**Code Examples:**

```javascript
// 1. Global context
console.log(this); // window (in browser)

function globalFunc() {
  'use strict';
  console.log(this); // undefined (strict mode)
}

// 2. Object method
const person = {
  name: 'John',
  greet: function() {
    console.log(`Hello, ${this.name}`);
  },
  greetArrow: () => {
    console.log(`Hello, ${this.name}`); // undefined!
  }
};

person.greet(); // "Hello, John"
person.greetArrow(); // "Hello, undefined"

// 3. Method extraction problem
const greetFunc = person.greet;
greetFunc(); // "Hello, undefined" - lost context!

// 4. Constructor function
function Person(name) {
  this.name = name;
  this.sayHello = function() {
    console.log(`Hi, I'm ${this.name}`);
  };
}

const john = new Person('John');
john.sayHello(); // "Hi, I'm John"

// 5. Explicit binding
const jane = { name: 'Jane' };
john.sayHello.call(jane); // "Hi, I'm Jane"
john.sayHello.apply(jane); // "Hi, I'm Jane"
const boundFunc = john.sayHello.bind(jane);
boundFunc(); // "Hi, I'm Jane"

// 6. Arrow functions inherit 'this'
const obj = {
  value: 100,
  regularMethod: function() {
    setTimeout(function() {
      console.log(this.value); // undefined
    }, 100);
  },
  arrowMethod: function() {
    setTimeout(() => {
      console.log(this.value); // 100
    }, 100);
  }
};
```

**Cypress Testing Scenario:**

```javascript
describe('Understanding "this" in Cypress', () => {
  // Cypress test context
  beforeEach(function() {
    // Regular function - has access to test context
    this.testData = {
      username: 'testuser',
      email: 'test@example.com'
    };

    cy.wrap({ count: 5 }).as('counter');
  });

  it('should access test context with regular function', function() {
    // Regular function callback - 'this' is test context
    expect(this.testData).to.exist;
    expect(this.testData.username).to.equal('testuser');

    // Access aliased data via 'this'
    expect(this.counter.count).to.equal(5);
  });

  it('should NOT access test context with arrow function', () => {
    // Arrow function - 'this' is NOT test context
    // console.log(this.testData); // undefined!

    // Must use cy.get() with aliases
    cy.get('@counter').then((data) => {
      expect(data.count).to.equal(5);
    });
  });

  it('should handle "this" in nested callbacks', function() {
    cy.visit('/login');

    // Save context reference
    const self = this;

    cy.get('[data-testid="email"]').then(function($el) {
      // 'this' here is Cypress context
      // Use saved reference for test context
      $el.type(self.testData.email);
    });

    // Or use arrow function to preserve 'this'
    cy.get('[data-testid="username"]').then(($el) => {
      $el.type(this.testData.username);
    });
  });

  it('should demonstrate "this" in custom commands', function() {
    // Custom command
    Cypress.Commands.add('loginWithTestData', function() {
      // 'this' is Cypress context
      const parent = this;

      cy.visit('/login');
      cy.get('[data-testid="email"]').type('test@example.com');
      cy.get('[data-testid="password"]').type('password');

      return cy.get('[type="submit"]').click();
    });

    cy.loginWithTestData();
  });

  it('should use "this" in Page Objects', function() {
    class LoginPage {
      constructor() {
        this.selectors = {
          email: '[data-testid="email"]',
          password: '[data-testid="password"]',
          submit: '[type="submit"]'
        };
      }

      visit() {
        cy.visit('/login');
        return this; // Enable method chaining
      }

      fillEmail(email) {
        cy.get(this.selectors.email).type(email);
        return this;
      }

      fillPassword(password) {
        cy.get(this.selectors.password).type(password);
        return this;
      }

      submit() {
        cy.get(this.selectors.submit).click();
        return this;
      }
    }

    const loginPage = new LoginPage();
    loginPage
      .visit()
      .fillEmail(this.testData.email)
      .fillPassword('password123')
      .submit();
  });

  it('should handle "this" in event handlers', () => {
    cy.get('button').then(($btn) => {
      // jQuery element wraps DOM element
      // 'this' in jQuery event handler is DOM element

      $btn.on('click', function(e) {
        // 'this' is the DOM element
        cy.log(this.tagName); // 'BUTTON'
        cy.wrap(this).should('have.prop', 'tagName', 'BUTTON');
      });

      $btn.on('click', (e) => {
        // Arrow function - 'this' is lexical
        cy.log(this); // Not the DOM element!
      });
    });
  });
});

// Demonstrating binding methods
describe('Explicit Binding in Tests', () => {
  const apiHelper = {
    baseUrl: 'https://api.example.com',
    timeout: 5000,

    makeRequest: function(endpoint) {
      return cy.request({
        url: `${this.baseUrl}${endpoint}`,
        timeout: this.timeout
      });
    },

    // Arrow function - won't work correctly
    makeRequestArrow: (endpoint) => {
      return cy.request({
        url: `${this.baseUrl}${endpoint}`, // this.baseUrl is undefined!
        timeout: this.timeout
      });
    }
  };

  it('should use call() for single invocation', () => {
    const customHelper = {
      baseUrl: 'https://custom.com',
      timeout: 10000
    };

    // Call with different context
    apiHelper.makeRequest.call(customHelper, '/users')
      .then((response) => {
        expect(response.status).to.equal(200);
      });
  });

  it('should use bind() for reusable function', () => {
    const testConfig = {
      baseUrl: 'https://test.com',
      timeout: 3000
    };

    // Create bound function
    const testRequest = apiHelper.makeRequest.bind(testConfig);

    testRequest('/users');
    testRequest('/posts');
  });

  it('should preserve context in array methods', () => {
    const processor = {
      multiplier: 10,

      processNumbers: function(numbers) {
        // BAD: Lost context in callback
        const results1 = numbers.map(function(num) {
          return num * this.multiplier; // this.multiplier is undefined!
        });

        // GOOD: Use arrow function
        const results2 = numbers.map((num) => {
          return num * this.multiplier;
        });

        // GOOD: Pass thisArg to map
        const results3 = numbers.map(function(num) {
          return num * this.multiplier;
        }, this); // Second argument is 'this' for callback

        // GOOD: Bind the callback
        const results4 = numbers.map(function(num) {
          return num * this.multiplier;
        }.bind(this));

        return results2;
      }
    };

    const result = processor.processNumbers([1, 2, 3]);
    expect(result).to.deep.equal([10, 20, 30]);
  });
});
```

**'this' Priority Rules:**

```javascript
// Priority (highest to lowest):
// 1. new binding
const obj1 = new MyFunction(); // this = new object

// 2. Explicit binding (call, apply, bind)
myFunc.call(obj2); // this = obj2

// 3. Implicit binding (method call)
obj3.method(); // this = obj3

// 4. Default binding
myFunc(); // this = window (or undefined in strict mode)

// 5. Arrow functions (lexical)
const arrow = () => {}; // this = enclosing scope
```

**Common Mistakes to Avoid:**

1. Using arrow functions when you need dynamic `this`
2. Losing context when passing methods as callbacks
3. Not understanding `this` is determined by call-site, not definition
4. Mixing regular functions and arrow functions incorrectly

**Interview Tips:**

- Remember: `this` is about the call-site, not the definition
- Arrow functions don't have their own `this`
- Use `.bind()` to permanently set `this`
- In Cypress, use regular functions to access test context

**Tricky Follow-up Questions:**

**Q: What will this code output?**

```javascript
const obj = {
  value: 42,
  getValue: function() {
    return this.value;
  }
};

console.log(obj.getValue()); // 42

const getValue = obj.getValue;
console.log(getValue()); // undefined (lost context)

const boundGetValue = obj.getValue.bind(obj);
console.log(boundGetValue()); // 42

const arrowObj = {
  value: 42,
  getValue: () => this.value
};
console.log(arrowObj.getValue()); // undefined (arrow inherits global this)
```

**Q: How do you fix the lost context problem?**

```javascript
class EventEmitter {
  constructor() {
    this.events = {};
  }

  on(event, callback) {
    if (!this.events[event]) {
      this.events[event] = [];
    }
    this.events[event].push(callback);
  }

  emit(event, data) {
    if (this.events[event]) {
      this.events[event].forEach(callback => {
        callback(data);
      });
    }
  }
}

class UserManager {
  constructor() {
    this.users = [];
    this.emitter = new EventEmitter();

    // PROBLEM: Lost context when callback is called
    this.emitter.on('userAdded', this.logUser);

    // SOLUTION 1: Bind in constructor
    this.emitter.on('userAdded', this.logUser.bind(this));

    // SOLUTION 2: Arrow function
    this.emitter.on('userAdded', (user) => this.logUser(user));

    // SOLUTION 3: Arrow function property (class field)
    // this.logUser = (user) => { ... }
  }

  logUser(user) {
    console.log(`User added: ${user.name}`);
    console.log(`Total users: ${this.users.length}`);
  }

  addUser(user) {
    this.users.push(user);
    this.emitter.emit('userAdded', user);
  }
}
```

---

### Q9: What are callback functions? How are they used in Cypress?

**Answer:**

A callback function is a function passed as an argument to another function, to be executed later (asynchronously or synchronously).

**Callback Types:**

1. **Synchronous Callbacks**: Executed immediately
2. **Asynchronous Callbacks**: Executed after an operation completes

**Code Examples:**

```javascript
// 1. Synchronous callback
function processArray(arr, callback) {
  const result = [];
  for (let i = 0; i < arr.length; i++) {
    result.push(callback(arr[i], i));
  }
  return result;
}

const numbers = [1, 2, 3, 4];
const doubled = processArray(numbers, (num) => num * 2);
console.log(doubled); // [2, 4, 6, 8]

// 2. Asynchronous callback
function fetchData(url, callback) {
  setTimeout(() => {
    const data = { id: 1, name: 'John' };
    callback(null, data); // Error first, then data
  }, 1000);
}

fetchData('/api/user', (error, data) => {
  if (error) {
    console.error(error);
  } else {
    console.log(data);
  }
});

// 3. Event callbacks
document.addEventListener('click', function(event) {
  console.log('Clicked at:', event.clientX, event.clientY);
});

// 4. Array methods with callbacks
const fruits = ['apple', 'banana', 'cherry'];

fruits.forEach((fruit, index) => {
  console.log(`${index}: ${fruit}`);
});

const longFruits = fruits.filter((fruit) => fruit.length > 5);
const upperFruits = fruits.map((fruit) => fruit.toUpperCase());
const hasCherry = fruits.some((fruit) => fruit === 'cherry');
```

**Callback Hell (Pyramid of Doom):**

```javascript
// BAD: Nested callbacks
asyncOperation1((error1, result1) => {
  if (error1) {
    handleError(error1);
  } else {
    asyncOperation2(result1, (error2, result2) => {
      if (error2) {
        handleError(error2);
      } else {
        asyncOperation3(result2, (error3, result3) => {
          if (error3) {
            handleError(error3);
          } else {
            console.log('Final result:', result3);
          }
        });
      }
    });
  }
});

// BETTER: Promises or async/await (covered in later questions)
```

**Cypress Testing Scenario:**

```javascript
describe('Callbacks in Cypress', () => {
  it('should use .then() callback for command results', () => {
    cy.get('[data-testid="username"]').then(($el) => {
      // $el is the yielded subject
      const text = $el.text();
      expect(text).to.not.be.empty;

      // Can perform additional operations
      if (text.includes('admin')) {
        cy.get('[data-testid="admin-panel"]').should('be.visible');
      }
    });
  });

  it('should chain callbacks with .then()', () => {
    cy.request('/api/user/1')
      .then((response) => {
        // First callback
        expect(response.status).to.equal(200);
        return response.body.id;
      })
      .then((userId) => {
        // Second callback - receives return value from previous
        cy.log(`User ID: ${userId}`);
        return cy.request(`/api/posts?userId=${userId}`);
      })
      .then((postsResponse) => {
        // Third callback
        expect(postsResponse.body).to.be.an('array');
      });
  });

  it('should use .each() with callback for iteration', () => {
    cy.get('.user-item').each(($el, index, $list) => {
      // Callback for each element
      cy.wrap($el)
        .should('have.attr', 'data-index', index.toString())
        .find('.username')
        .should('not.be.empty');
    });
  });

  it('should use .should() with callback assertions', () => {
    cy.get('[data-testid="count"]').should(($el) => {
      // Callback for custom assertion
      const count = parseInt($el.text());
      expect(count).to.be.greaterThan(0);
      expect(count).to.be.lessThan(100);
    });
  });

  it('should handle custom callback functions', () => {
    // Custom validation function
    const validateUserData = (user, callback) => {
      const isValid = user.email && user.name;
      if (isValid) {
        callback(null, user);
      } else {
        callback(new Error('Invalid user data'));
      }
    };

    cy.request('/api/user/1').then((response) => {
      const user = response.body;

      // Use custom callback
      cy.wrap(null).then(() => {
        return new Promise((resolve, reject) => {
          validateUserData(user, (error, validUser) => {
            if (error) {
              reject(error);
            } else {
              resolve(validUser);
            }
          });
        });
      }).then((validUser) => {
        cy.log(`Valid user: ${validUser.name}`);
      });
    });
  });

  it('should use .within() with callback for scoping', () => {
    cy.get('[data-testid="user-form"]').within(() => {
      // All commands within this callback are scoped to the form
      cy.get('[name="email"]').type('test@example.com');
      cy.get('[name="password"]').type('password123');
      cy.get('[type="submit"]').click();
    });
  });

  it('should use .wrap() with .then() for custom operations', () => {
    const processData = (data) => {
      return data.map(item => ({
        ...item,
        processed: true,
        timestamp: Date.now()
      }));
    };

    cy.request('/api/data')
      .its('body')
      .then((data) => {
        const processed = processData(data);
        return cy.wrap(processed);
      })
      .then((processedData) => {
        expect(processedData[0]).to.have.property('processed', true);
      });
  });

  it('should handle error callbacks', () => {
    cy.request({
      url: '/api/protected',
      failOnStatusCode: false
    }).then((response) => {
      if (response.status === 401) {
        cy.log('Unauthorized - redirecting to login');
        cy.visit('/login');
      } else {
        cy.log('Access granted');
      }
    });
  });
});

// Custom helper with callbacks
const testHelper = {
  // Callback-based retry mechanism
  retryUntil: (operation, condition, maxAttempts = 5, callback) => {
    let attempts = 0;

    const attempt = () => {
      attempts++;
      operation().then((result) => {
        if (condition(result) || attempts >= maxAttempts) {
          callback(null, result, attempts);
        } else {
          cy.wait(1000);
          attempt();
        }
      }).catch((error) => {
        callback(error, null, attempts);
      });
    };

    attempt();
  }
};

describe('Custom Callback Helpers', () => {
  it('should use custom retry helper', () => {
    const operation = () => cy.request('/api/status');
    const condition = (result) => result.body.ready === true;

    cy.wrap(null).then(() => {
      return new Promise((resolve, reject) => {
        testHelper.retryUntil(operation, condition, 5, (error, result, attempts) => {
          if (error) {
            reject(error);
          } else {
            cy.log(`Operation succeeded after ${attempts} attempts`);
            resolve(result);
          }
        });
      });
    });
  });
});

// Array callback methods in tests
describe('Array Methods with Callbacks', () => {
  it('should use array methods for data processing', () => {
    cy.request('/api/users').then((response) => {
      const users = response.body;

      // filter callback
      const activeUsers = users.filter((user) => user.isActive);
      expect(activeUsers.length).to.be.greaterThan(0);

      // map callback
      const userIds = users.map((user) => user.id);
      expect(userIds).to.be.an('array');

      // find callback
      const adminUser = users.find((user) => user.role === 'admin');
      expect(adminUser).to.exist;

      // some callback
      const hasAdmins = users.some((user) => user.role === 'admin');
      expect(hasAdmins).to.be.true;

      // every callback
      const allHaveEmail = users.every((user) => user.email);
      expect(allHaveEmail).to.be.true;

      // reduce callback
      const totalAge = users.reduce((sum, user) => sum + user.age, 0);
      expect(totalAge).to.be.greaterThan(0);
    });
  });
});
```

**Error-First Callback Pattern:**

```javascript
// Node.js convention
function readFile(path, callback) {
  fs.readFile(path, 'utf8', (error, data) => {
    if (error) {
      callback(error, null);
    } else {
      callback(null, data);
    }
  });
}

// Usage
readFile('file.txt', (error, data) => {
  if (error) {
    console.error('Error:', error);
    return;
  }
  console.log('Data:', data);
});
```

**Common Mistakes to Avoid:**

1. Callback hell (excessive nesting)
2. Not handling errors in callbacks
3. Calling callback multiple times
4. Mixing callbacks and promises incorrectly

**Interview Tips:**

- Callbacks are fundamental to async JavaScript
- Understand synchronous vs asynchronous callbacks
- Know about callback hell and how to avoid it
- In Cypress, `.then()` is the primary callback mechanism

**Tricky Follow-up Questions:**

**Q: What's wrong with this code?**

```javascript
// PROBLEM 1: Callback hell
getData((data) => {
  processData(data, (processed) => {
    saveData(processed, (result) => {
      console.log(result);
    });
  });
});

// SOLUTION: Use promises or async/await
async function handleData() {
  const data = await getData();
  const processed = await processData(data);
  const result = await saveData(processed);
  console.log(result);
}

// PROBLEM 2: Calling callback multiple times
function badAsync(callback) {
  callback(null, 'first call');

  setTimeout(() => {
    callback(null, 'second call'); // BUG!
  }, 1000);
}

// PROBLEM 3: Not handling errors
function riskyOperation(callback) {
  try {
    const result = doSomething();
    callback(result); // No error parameter!
  } catch (error) {
    // Error not passed to callback
    console.error(error);
  }
}

// SOLUTION: Error-first callbacks
function safeOperation(callback) {
  try {
    const result = doSomething();
    callback(null, result);
  } catch (error) {
    callback(error, null);
  }
}
```

---

### Q10: Explain higher-order functions. Provide examples used in test automation.

**Answer:**

A higher-order function is a function that either:
1. Takes one or more functions as arguments, OR
2. Returns a function as its result

**Why Higher-Order Functions?**

- Code reusability
- Abstraction
- Functional composition
- Cleaner, more maintainable code

**Common Higher-Order Functions:**

```javascript
// 1. Functions that take functions as arguments

// map
const numbers = [1, 2, 3, 4];
const doubled = numbers.map((n) => n * 2);
console.log(doubled); // [2, 4, 6, 8]

// filter
const evens = numbers.filter((n) => n % 2 === 0);
console.log(evens); // [2, 4]

// reduce
const sum = numbers.reduce((acc, n) => acc + n, 0);
console.log(sum); // 10

// forEach
numbers.forEach((n) => console.log(n));

// find
const found = numbers.find((n) => n > 2);
console.log(found); // 3

// some
const hasEven = numbers.some((n) => n % 2 === 0);
console.log(hasEven); // true

// every
const allPositive = numbers.every((n) => n > 0);
console.log(allPositive); // true

// 2. Functions that return functions

function multiplier(factor) {
  return function(number) {
    return number * factor;
  };
}

const double = multiplier(2);
const triple = multiplier(3);

console.log(double(5)); // 10
console.log(triple(5)); // 15

// 3. Functions that do both

function compose(f, g) {
  return function(x) {
    return f(g(x));
  };
}

const add5 = (x) => x + 5;
const multiply2 = (x) => x * 2;

const add5ThenMultiply2 = compose(multiply2, add5);
console.log(add5ThenMultiply2(10)); // (10 + 5) * 2 = 30
```

**Cypress Testing Scenario:**

```javascript
describe('Higher-Order Functions in Tests', () => {
  // 1. Custom assertion builder
  const createAssertion = (property, expectedValue) => {
    return (actualValue) => {
      return actualValue[property] === expectedValue;
    };
  };

  it('should use custom assertion functions', () => {
    const isAdmin = createAssertion('role', 'admin');
    const isActive = createAssertion('status', 'active');

    cy.request('/api/user/1').then((response) => {
      const user = response.body;

      expect(isAdmin(user)).to.be.true;
      expect(isActive(user)).to.be.true;
    });
  });

  // 2. Test data generators
  const createUserGenerator = (baseUser) => {
    let count = 0;

    return (overrides = {}) => {
      count++;
      return {
        ...baseUser,
        id: count,
        username: `${baseUser.username}${count}`,
        email: `${baseUser.username}${count}@example.com`,
        ...overrides
      };
    };
  };

  it('should use function generators for test data', () => {
    const generateUser = createUserGenerator({
      username: 'testuser',
      role: 'user',
      status: 'active'
    });

    const user1 = generateUser();
    const user2 = generateUser({ role: 'admin' });

    expect(user1.id).to.equal(1);
    expect(user2.id).to.equal(2);
    expect(user2.role).to.equal('admin');
  });

  // 3. Retry mechanism
  const retry = (fn, maxAttempts = 3, delay = 1000) => {
    return function(...args) {
      let attempts = 0;

      const attempt = () => {
        attempts++;
        return fn(...args).catch((error) => {
          if (attempts < maxAttempts) {
            cy.wait(delay);
            cy.log(`Retry attempt ${attempts + 1}/${maxAttempts}`);
            return attempt();
          }
          throw error;
        });
      };

      return attempt();
    };
  };

  it('should use retry wrapper', () => {
    const apiCall = () => cy.request('/api/flaky');
    const retriedCall = retry(apiCall, 3, 500);

    retriedCall().then((response) => {
      expect(response.status).to.equal(200);
    });
  });

  // 4. Conditional execution
  const when = (condition) => {
    return (action) => {
      return (value) => {
        if (condition(value)) {
          return action(value);
        }
        return cy.wrap(value);
      };
    };
  };

  it('should conditionally execute actions', () => {
    const isLongList = (list) => list.length > 10;
    const logWarning = (list) => {
      cy.log(`Warning: Long list with ${list.length} items`);
      return cy.wrap(list);
    };

    const warnIfLong = when(isLongList)(logWarning);

    cy.request('/api/items').its('body').then(warnIfLong);
  });

  // 5. Data transformation pipeline
  const pipe = (...fns) => {
    return (initialValue) => {
      return fns.reduce((value, fn) => fn(value), initialValue);
    };
  };

  it('should transform data through pipeline', () => {
    const filterActive = (users) => users.filter(u => u.isActive);
    const sortByName = (users) => [...users].sort((a, b) => a.name.localeCompare(b.name));
    const extractNames = (users) => users.map(u => u.name);

    const processUsers = pipe(
      filterActive,
      sortByName,
      extractNames
    );

    cy.fixture('users').then((users) => {
      const result = processUsers(users);
      expect(result).to.be.an('array');
      expect(result[0]).to.be.a('string');
    });
  });

  // 6. Validator factory
  const createValidator = (rules) => {
    return (value) => {
      const errors = [];

      rules.forEach(({ test, message }) => {
        if (!test(value)) {
          errors.push(message);
        }
      });

      return {
        isValid: errors.length === 0,
        errors
      };
    };
  };

  it('should use custom validators', () => {
    const emailValidator = createValidator([
      {
        test: (v) => v && v.length > 0,
        message: 'Email is required'
      },
      {
        test: (v) => v.includes('@'),
        message: 'Email must contain @'
      },
      {
        test: (v) => v.length <= 100,
        message: 'Email is too long'
      }
    ]);

    const result1 = emailValidator('test@example.com');
    expect(result1.isValid).to.be.true;

    const result2 = emailValidator('invalid');
    expect(result2.isValid).to.be.false;
    expect(result2.errors).to.include('Email must contain @');
  });

  // 7. Memoization
  const memoize = (fn) => {
    const cache = new Map();

    return (...args) => {
      const key = JSON.stringify(args);

      if (cache.has(key)) {
        cy.log('Cache hit');
        return cache.get(key);
      }

      cy.log('Cache miss');
      const result = fn(...args);
      cache.set(key, result);
      return result;
    };
  };

  it('should memoize expensive operations', () => {
    const expensiveCalc = (n) => {
      let sum = 0;
      for (let i = 0; i < n; i++) {
        sum += i;
      }
      return sum;
    };

    const memoizedCalc = memoize(expensiveCalc);

    const result1 = memoizedCalc(1000); // Calculated
    const result2 = memoizedCalc(1000); // From cache
    const result3 = memoizedCalc(2000); // Calculated

    expect(result1).to.equal(result2);
  });

  // 8. Currying
  const curry = (fn) => {
    return function curried(...args) {
      if (args.length >= fn.length) {
        return fn.apply(this, args);
      }
      return (...moreArgs) => {
        return curried.apply(this, [...args, ...moreArgs]);
      };
    };
  };

  it('should use curried functions', () => {
    const formatLog = (level, category, message) => {
      return `[${level}] [${category}] ${message}`;
    };

    const curriedLog = curry(formatLog);

    const errorLog = curriedLog('ERROR');
    const errorApiLog = errorLog('API');

    const log1 = errorApiLog('Request failed');
    const log2 = errorApiLog('Timeout occurred');

    expect(log1).to.equal('[ERROR] [API] Request failed');
    expect(log2).to.equal('[ERROR] [API] Timeout occurred');
  });
});

// Practical test utilities using higher-order functions
const TestUtils = {
  // Repeat action n times
  times: (n) => (fn) => {
    for (let i = 0; i < n; i++) {
      fn(i);
    }
  },

  // Delay execution
  delay: (ms) => (fn) => {
    return cy.wait(ms).then(fn);
  },

  // Log execution time
  timed: (label) => (fn) => {
    const start = Date.now();
    const result = fn();
    const end = Date.now();
    cy.log(`${label} took ${end - start}ms`);
    return result;
  },

  // Combine multiple predicates with AND
  allOf: (...predicates) => (value) => {
    return predicates.every(pred => pred(value));
  },

  // Combine multiple predicates with OR
  anyOf: (...predicates) => (value) => {
    return predicates.some(pred => pred(value));
  }
};

describe('Test Utilities with Higher-Order Functions', () => {
  it('should use utility functions', () => {
    // Repeat
    TestUtils.times(3)((i) => {
      cy.log(`Iteration ${i}`);
    });

    // Combine predicates
    const isValidAge = (age) => age >= 18 && age <= 100;
    const isEven = (n) => n % 2 === 0;

    const isValidEvenAge = TestUtils.allOf(isValidAge, isEven);

    expect(isValidEvenAge(20)).to.be.true;
    expect(isValidEvenAge(21)).to.be.false;
    expect(isValidEvenAge(17)).to.be.false;
  });
});
```

**Function Composition:**

```javascript
// Compose (right to left)
const compose = (...fns) => (x) =>
  fns.reduceRight((acc, fn) => fn(acc), x);

// Pipe (left to right)
const pipe = (...fns) => (x) =>
  fns.reduce((acc, fn) => fn(acc), x);

// Example
const add10 = (x) => x + 10;
const multiply3 = (x) => x * 3;
const subtract5 = (x) => x - 5;

const composed = compose(subtract5, multiply3, add10);
console.log(composed(5)); // ((5 + 10) * 3) - 5 = 40

const piped = pipe(add10, multiply3, subtract5);
console.log(piped(5)); // ((5 + 10) * 3) - 5 = 40
```

**Common Mistakes to Avoid:**

1. Overcomplicating simple logic with higher-order functions
2. Not understanding function composition order
3. Creating too many abstraction layers
4. Forgetting to handle edge cases in returned functions

**Interview Tips:**

- Higher-order functions enable functional programming patterns
- They're fundamental to array methods (map, filter, reduce)
- Understanding them is key to modern JavaScript
- Used extensively in React (hooks), testing frameworks

**Tricky Follow-up Questions:**

**Q: Implement your own map function.**

```javascript
Array.prototype.myMap = function(callback) {
  const result = [];

  for (let i = 0; i < this.length; i++) {
    result.push(callback(this[i], i, this));
  }

  return result;
};

const numbers = [1, 2, 3];
const doubled = numbers.myMap(n => n * 2);
console.log(doubled); // [2, 4, 6]
```

**Q: What's the difference between map and forEach?**

```javascript
const numbers = [1, 2, 3];

// map returns new array
const doubled = numbers.map(n => n * 2);
console.log(doubled); // [2, 4, 6]

// forEach returns undefined
const result = numbers.forEach(n => n * 2);
console.log(result); // undefined

// forEach is for side effects
numbers.forEach(n => console.log(n)); // Logs each number
```

---


## Section 3: Objects and Arrays

### Q11: Explain object creation patterns and prototypes. How do you use objects in test automation?

**Answer:**

JavaScript has multiple ways to create and work with objects. Understanding prototypes is crucial for advanced JavaScript.

**Object Creation Methods:**

```javascript
// 1. Object literal
const user1 = {
  name: 'John',
  age: 30,
  greet() {
    return `Hello, I'm ${this.name}`;
  }
};

// 2. Object constructor
const user2 = new Object();
user2.name = 'Jane';
user2.age = 25;

// 3. Object.create()
const personPrototype = {
  greet() {
    return `Hello, I'm ${this.name}`;
  }
};

const user3 = Object.create(personPrototype);
user3.name = 'Bob';
user3.age = 35;

// 4. Constructor function
function Person(name, age) {
  this.name = name;
  this.age = age;
}

Person.prototype.greet = function() {
  return `Hello, I'm ${this.name}`;
};

const user4 = new Person('Alice', 28);

// 5. ES6 Class
class User {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  greet() {
    return `Hello, I'm ${this.name}`;
  }
}

const user5 = new User('Charlie', 32);

// 6. Factory function
function createUser(name, age) {
  return {
    name,
    age,
    greet() {
      return `Hello, I'm ${this.name}`;
    }
  };
}

const user6 = createUser('Diana', 27);
```

**Prototype Chain:**

```javascript
// Every object has a prototype
const obj = {};
console.log(obj.__proto__ === Object.prototype); // true

// Prototype chain
function Animal(name) {
  this.name = name;
}

Animal.prototype.makeSound = function() {
  return 'Some sound';
};

function Dog(name, breed) {
  Animal.call(this, name);
  this.breed = breed;
}

Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

Dog.prototype.bark = function() {
  return 'Woof!';
};

const dog = new Dog('Buddy', 'Labrador');
console.log(dog.name); // 'Buddy' (own property)
console.log(dog.bark()); // 'Woof!' (Dog.prototype)
console.log(dog.makeSound()); // 'Some sound' (Animal.prototype)
```

**Cypress Testing Scenario:**

```javascript
describe('Objects in Test Automation', () => {
  // 1. Test data objects
  const testUsers = {
    admin: {
      email: 'admin@example.com',
      password: 'Admin123!',
      role: 'admin'
    },
    regularUser: {
      email: 'user@example.com',
      password: 'User123!',
      role: 'user'
    },
    guest: {
      email: 'guest@example.com',
      password: 'Guest123!',
      role: 'guest'
    }
  };

  it('should use test data objects', () => {
    const user = testUsers.admin;

    cy.visit('/login');
    cy.get('[data-testid="email"]').type(user.email);
    cy.get('[data-testid="password"]').type(user.password);
    cy.get('[type="submit"]').click();

    cy.url().should('include', '/dashboard');
  });

  // 2. Page Object pattern with class
  class LoginPage {
    constructor() {
      this.selectors = {
        emailInput: '[data-testid="email"]',
        passwordInput: '[data-testid="password"]',
        submitButton: '[type="submit"]',
        errorMessage: '.error-message'
      };
    }

    visit() {
      cy.visit('/login');
      return this;
    }

    login(email, password) {
      cy.get(this.selectors.emailInput).type(email);
      cy.get(this.selectors.passwordInput).type(password);
      cy.get(this.selectors.submitButton).click();
      return this;
    }

    verifyError(message) {
      cy.get(this.selectors.errorMessage).should('contain', message);
      return this;
    }
  }

  it('should use page object class', () => {
    const loginPage = new LoginPage();

    loginPage
      .visit()
      .login('wrong@example.com', 'wrongpassword')
      .verifyError('Invalid credentials');
  });

  // 3. Factory function for test data
  const createTestUser = ({ role = 'user', verified = true } = {}) => {
    const timestamp = Date.now();

    return {
      id: timestamp,
      username: `user${timestamp}`,
      email: `user${timestamp}@example.com`,
      password: 'Test123!',
      role,
      verified,
      createdAt: new Date().toISOString()
    };
  };

  it('should use factory function for test data', () => {
    const adminUser = createTestUser({ role: 'admin' });
    const regularUser = createTestUser();

    expect(adminUser.role).to.equal('admin');
    expect(regularUser.role).to.equal('user');
    expect(adminUser.id).to.not.equal(regularUser.id);
  });

  // 4. Object methods and manipulation
  it('should manipulate objects', () => {
    cy.request('/api/user/1').then((response) => {
      const user = response.body;

      // Check properties
      expect(user).to.have.property('name');
      expect(user).to.have.property('email');

      // Get keys
      const keys = Object.keys(user);
      expect(keys).to.include('name');

      // Get values
      const values = Object.values(user);
      expect(values.length).to.be.greaterThan(0);

      // Get entries
      const entries = Object.entries(user);
      entries.forEach(([key, value]) => {
        expect(key).to.be.a('string');
      });

      // Check if property exists
      expect('name' in user).to.be.true;
      expect(user.hasOwnProperty('name')).to.be.true;
    });
  });

  // 5. Object spread and merge
  it('should merge objects for test scenarios', () => {
    const defaultConfig = {
      baseUrl: 'http://localhost',
      timeout: 5000,
      retries: 3
    };

    const testConfig = {
      timeout: 10000,
      headers: {
        'Authorization': 'Bearer token'
      }
    };

    // Shallow merge
    const mergedConfig = { ...defaultConfig, ...testConfig };

    expect(mergedConfig.baseUrl).to.equal('http://localhost');
    expect(mergedConfig.timeout).to.equal(10000); // Overridden
    expect(mergedConfig.headers).to.exist;

    // Object.assign
    const assignedConfig = Object.assign({}, defaultConfig, testConfig);
    expect(assignedConfig).to.deep.equal(mergedConfig);
  });

  // 6. Property descriptors
  it('should work with property descriptors', () => {
    const config = {};

    Object.defineProperty(config, 'apiKey', {
      value: 'secret123',
      writable: false,
      enumerable: false,
      configurable: false
    });

    expect(config.apiKey).to.equal('secret123');

    // Cannot change
    config.apiKey = 'newvalue';
    expect(config.apiKey).to.equal('secret123'); // Still old value

    // Not enumerable
    expect(Object.keys(config)).to.not.include('apiKey');
  });

  // 7. Object sealing and freezing
  it('should understand seal and freeze', () => {
    const config = {
      apiUrl: 'http://api.example.com',
      timeout: 5000
    };

    // Seal: Can modify existing properties but not add/delete
    Object.seal(config);
    config.timeout = 10000; // Works
    config.newProp = 'value'; // Doesn't work

    expect(config.timeout).to.equal(10000);
    expect(config.newProp).to.be.undefined;
    expect(Object.isSealed(config)).to.be.true;

    // Freeze: Cannot modify, add, or delete
    const frozenConfig = Object.freeze({ ...config });
    frozenConfig.timeout = 20000; // Doesn't work

    expect(frozenConfig.timeout).to.equal(10000);
    expect(Object.isFrozen(frozenConfig)).to.be.true;
  });

  // 8. Computed property names
  it('should use computed property names', () => {
    const field = 'username';
    const value = 'testuser';

    const formData = {
      [field]: value,
      [`${field}_verified`]: true,
      [field.toUpperCase()]: value.toUpperCase()
    };

    expect(formData.username).to.equal('testuser');
    expect(formData.username_verified).to.be.true;
    expect(formData.USERNAME).to.equal('TESTUSER');
  });

  // 9. Object destructuring in tests
  it('should use object destructuring', () => {
    cy.request('/api/user/1').then((response) => {
      const { status, body } = response;
      const { id, name, email, address } = body;
      const { city, country } = address;

      expect(status).to.equal(200);
      expect(name).to.be.a('string');
      expect(email).to.include('@');
      expect(city).to.exist;
      expect(country).to.exist;
    });
  });

  // 10. Prototype chain in practice
  class ApiClient {
    constructor(baseUrl) {
      this.baseUrl = baseUrl;
    }

    request(endpoint) {
      return cy.request(`${this.baseUrl}${endpoint}`);
    }
  }

  class AuthApiClient extends ApiClient {
    constructor(baseUrl, token) {
      super(baseUrl);
      this.token = token;
    }

    request(endpoint) {
      return cy.request({
        url: `${this.baseUrl}${endpoint}`,
        headers: {
          'Authorization': `Bearer ${this.token}`
        }
      });
    }
  }

  it('should use inheritance in API clients', () => {
    const authClient = new AuthApiClient('http://api.example.com', 'token123');

    authClient.request('/users').then((response) => {
      expect(response.status).to.equal(200);
    });

    expect(authClient instanceof AuthApiClient).to.be.true;
    expect(authClient instanceof ApiClient).to.be.true;
  });
});
```

**Common Object Patterns:**

```javascript
// Singleton pattern
const DatabaseConnection = (function() {
  let instance;

  function createInstance() {
    return {
      connect() {
        return 'Connected to database';
      },
      query(sql) {
        return `Executing: ${sql}`;
      }
    };
  }

  return {
    getInstance() {
      if (!instance) {
        instance = createInstance();
      }
      return instance;
    }
  };
})();

const db1 = DatabaseConnection.getInstance();
const db2 = DatabaseConnection.getInstance();
console.log(db1 === db2); // true (same instance)

// Module pattern
const UserModule = (function() {
  let users = [];

  function add(user) {
    users.push(user);
  }

  function getAll() {
    return [...users];
  }

  function clear() {
    users = [];
  }

  return {
    add,
    getAll,
    clear
  };
})();
```

**Common Mistakes to Avoid:**

1. Modifying object prototypes directly
2. Not understanding prototype chain lookup
3. Forgetting `new` keyword with constructor functions
4. Shallow copying objects with nested properties

**Interview Tips:**

- Understand the prototype chain
- Know difference between `__proto__` and `prototype`
- Be familiar with `Object.create()`, `Object.assign()`, spread operator
- Understand when to use classes vs factory functions

**Tricky Follow-up Questions:**

**Q: What's the difference between `__proto__` and `prototype`?**

```javascript
function Person(name) {
  this.name = name;
}

const john = new Person('John');

// prototype: Property of constructor function
console.log(Person.prototype); // Object with constructor

// __proto__: Property of instance, points to constructor's prototype
console.log(john.__proto__ === Person.prototype); // true

// Prototype chain
console.log(john.__proto__.__proto__ === Object.prototype); // true
console.log(john.__proto__.__proto__.__proto__); // null
```

**Q: How do you create a truly empty object?**

```javascript
// Regular object has Object.prototype
const obj1 = {};
console.log(obj1.toString); // function (inherited from Object.prototype)

// Truly empty object
const obj2 = Object.create(null);
console.log(obj2.toString); // undefined (no prototype chain)
```

---

### Q12: Explain array methods. Which ones are most useful in test automation?

**Answer:**

Arrays are fundamental to JavaScript. Understanding array methods is crucial for effective data manipulation in tests.

**Array Creation:**

```javascript
// Literals
const arr1 = [1, 2, 3];

// Constructor
const arr2 = new Array(3); // [empty × 3]
const arr3 = new Array(1, 2, 3); // [1, 2, 3]

// Array.from
const arr4 = Array.from('hello'); // ['h', 'e', 'l', 'l', 'o']
const arr5 = Array.from({ length: 3 }, (_, i) => i); // [0, 1, 2]

// Array.of
const arr6 = Array.of(3); // [3]
const arr7 = Array.of(1, 2, 3); // [1, 2, 3]

// Spread
const arr8 = [...arr1]; // [1, 2, 3] (copy)
```

**Iteration Methods:**

```javascript
const numbers = [1, 2, 3, 4, 5];

// forEach - Iterate (no return value)
numbers.forEach((num, index) => {
  console.log(`${index}: ${num}`);
});

// map - Transform each element
const doubled = numbers.map(num => num * 2);
// [2, 4, 6, 8, 10]

// filter - Select elements
const evens = numbers.filter(num => num % 2 === 0);
// [2, 4]

// find - Find first matching element
const found = numbers.find(num => num > 3);
// 4

// findIndex - Find index of first match
const index = numbers.findIndex(num => num > 3);
// 3

// some - Check if any element matches
const hasEven = numbers.some(num => num % 2 === 0);
// true

// every - Check if all elements match
const allPositive = numbers.every(num => num > 0);
// true

// reduce - Reduce to single value
const sum = numbers.reduce((acc, num) => acc + num, 0);
// 15

// reduceRight - Reduce from right to left
const reversed = numbers.reduceRight((acc, num) => {
  acc.push(num);
  return acc;
}, []);
// [5, 4, 3, 2, 1]
```

**Modification Methods:**

```javascript
const arr = [1, 2, 3];

// push - Add to end (mutates)
arr.push(4); // [1, 2, 3, 4]

// pop - Remove from end (mutates)
const last = arr.pop(); // last = 4, arr = [1, 2, 3]

// unshift - Add to beginning (mutates)
arr.unshift(0); // [0, 1, 2, 3]

// shift - Remove from beginning (mutates)
const first = arr.shift(); // first = 0, arr = [1, 2, 3]

// splice - Add/remove at any position (mutates)
arr.splice(1, 1, 'a', 'b'); // [1, 'a', 'b', 3]

// slice - Extract portion (doesn't mutate)
const sliced = arr.slice(1, 3); // ['a', 'b']

// concat - Merge arrays (doesn't mutate)
const arr2 = [4, 5];
const merged = arr.concat(arr2); // [1, 'a', 'b', 3, 4, 5]

// flat - Flatten nested arrays
const nested = [1, [2, [3, [4]]]];
const flat1 = nested.flat(); // [1, 2, [3, [4]]]
const flat2 = nested.flat(2); // [1, 2, 3, [4]]
const flatAll = nested.flat(Infinity); // [1, 2, 3, 4]

// flatMap - Map then flatten
const words = ['hello world', 'foo bar'];
const letters = words.flatMap(word => word.split(' '));
// ['hello', 'world', 'foo', 'bar']
```

**Search and Test Methods:**

```javascript
const arr = [1, 2, 3, 4, 5];

// includes - Check if value exists
arr.includes(3); // true

// indexOf - Find first index
arr.indexOf(3); // 2
arr.indexOf(10); // -1

// lastIndexOf - Find last index
[1, 2, 3, 2, 1].lastIndexOf(2); // 3

// join - Convert to string
arr.join(', '); // "1, 2, 3, 4, 5"

// reverse - Reverse order (mutates)
arr.reverse(); // [5, 4, 3, 2, 1]

// sort - Sort (mutates)
[3, 1, 4, 1, 5].sort(); // [1, 1, 3, 4, 5]
[10, 5, 40, 25].sort((a, b) => a - b); // [5, 10, 25, 40]
```

**Cypress Testing Scenario:**

```javascript
describe('Array Methods in Test Automation', () => {
  // 1. Processing API responses
  it('should process array of users', () => {
    cy.request('/api/users').then((response) => {
      const users = response.body;

      // Filter active users
      const activeUsers = users.filter(user => user.isActive);
      expect(activeUsers.length).to.be.greaterThan(0);

      // Map to usernames
      const usernames = users.map(user => user.username);
      expect(usernames).to.be.an('array');

      // Find specific user
      const admin = users.find(user => user.role === 'admin');
      expect(admin).to.exist;
      expect(admin.role).to.equal('admin');

      // Check if any user is verified
      const hasVerified = users.some(user => user.verified);
      expect(hasVerified).to.be.true;

      // Check if all users have email
      const allHaveEmail = users.every(user => user.email);
      expect(allHaveEmail).to.be.true;

      // Calculate total age
      const totalAge = users.reduce((sum, user) => sum + user.age, 0);
      const avgAge = totalAge / users.length;
      expect(avgAge).to.be.greaterThan(0);
    });
  });

  // 2. Validating UI lists
  it('should validate list of elements', () => {
    cy.get('.user-item').then(($elements) => {
      const elements = Array.from($elements);

      // Check count
      expect(elements.length).to.be.greaterThan(0);

      // Verify each element
      elements.forEach((el, index) => {
        expect(el).to.have.attr('data-index', index.toString());
      });

      // Check if any element has specific class
      const hasActive = elements.some(el => el.classList.contains('active'));
      expect(hasActive).to.be.true;

      // Map to text content
      const texts = elements.map(el => el.textContent.trim());
      expect(texts).to.not.include('');
    });
  });

  // 3. Test data transformation
  it('should transform test data', () => {
    const testUsers = [
      { name: 'John', age: 30, role: 'admin' },
      { name: 'Jane', age: 25, role: 'user' },
      { name: 'Bob', age: 35, role: 'user' }
    ];

    // Chain methods
    const result = testUsers
      .filter(user => user.age > 25)
      .map(user => ({
        ...user,
        displayName: user.name.toUpperCase()
      }))
      .sort((a, b) => b.age - a.age);

    expect(result).to.have.length(2);
    expect(result[0].name).to.equal('Bob');
    expect(result[0].displayName).to.equal('BOB');
  });

  // 4. Array grouping and categorization
  it('should group data by category', () => {
    const items = [
      { name: 'Apple', category: 'fruit' },
      { name: 'Carrot', category: 'vegetable' },
      { name: 'Banana', category: 'fruit' },
      { name: 'Broccoli', category: 'vegetable' }
    ];

    // Group by category
    const grouped = items.reduce((acc, item) => {
      if (!acc[item.category]) {
        acc[item.category] = [];
      }
      acc[item.category].push(item);
      return acc;
    }, {});

    expect(grouped.fruit).to.have.length(2);
    expect(grouped.vegetable).to.have.length(2);
  });

  // 5. Flatten nested data
  it('should flatten nested arrays', () => {
    const nestedData = [
      { id: 1, tags: ['tag1', 'tag2'] },
      { id: 2, tags: ['tag3'] },
      { id: 3, tags: ['tag1', 'tag4'] }
    ];

    // Get all unique tags
    const allTags = nestedData.flatMap(item => item.tags);
    const uniqueTags = [...new Set(allTags)];

    expect(allTags).to.have.length(5);
    expect(uniqueTags).to.have.length(4);
  });

  // 6. Sort and compare arrays
  it('should sort arrays correctly', () => {
    const numbers = [10, 5, 40, 25, 1000, 1];

    // Default sort (lexicographical)
    const sorted1 = [...numbers].sort();
    expect(sorted1[0]).to.equal(1);
    expect(sorted1[1]).to.equal(10); // "10" < "1000"

    // Numeric sort
    const sorted2 = [...numbers].sort((a, b) => a - b);
    expect(sorted2[0]).to.equal(1);
    expect(sorted2[1]).to.equal(5);

    // Sort objects
    const users = [
      { name: 'John', age: 30 },
      { name: 'Jane', age: 25 },
      { name: 'Bob', age: 35 }
    ];

    const sortedByAge = [...users].sort((a, b) => a.age - b.age);
    expect(sortedByAge[0].name).to.equal('Jane');

    const sortedByName = [...users].sort((a, b) => a.name.localeCompare(b.name));
    expect(sortedByName[0].name).to.equal('Bob');
  });

  // 7. Array utility functions
  it('should use array utilities', () => {
    const arr1 = [1, 2, 3];
    const arr2 = [3, 4, 5];

    // Intersection
    const intersection = arr1.filter(x => arr2.includes(x));
    expect(intersection).to.deep.equal([3]);

    // Difference
    const difference = arr1.filter(x => !arr2.includes(x));
    expect(difference).to.deep.equal([1, 2]);

    // Union
    const union = [...new Set([...arr1, ...arr2])];
    expect(union).to.deep.equal([1, 2, 3, 4, 5]);

    // Chunk array
    const chunk = (arr, size) => {
      return Array.from(
        { length: Math.ceil(arr.length / size) },
        (_, i) => arr.slice(i * size, i * size + size)
      );
    };

    const chunked = chunk([1, 2, 3, 4, 5], 2);
    expect(chunked).to.deep.equal([[1, 2], [3, 4], [5]]);
  });

  // 8. Array validation
  it('should validate array data', () => {
    cy.request('/api/posts').then((response) => {
      const posts = response.body;

      // Check type
      expect(Array.isArray(posts)).to.be.true;

      // Check not empty
      expect(posts.length).to.be.greaterThan(0);

      // Validate structure
      const validStructure = posts.every(post =>
        post.id &&
        post.title &&
        post.body &&
        typeof post.id === 'number'
      );

      expect(validStructure).to.be.true;

      // Check unique IDs
      const ids = posts.map(p => p.id);
      const uniqueIds = new Set(ids);
      expect(uniqueIds.size).to.equal(ids.length);
    });
  });

  // 9. Complex array transformations
  it('should perform complex transformations', () => {
    const orders = [
      {
        id: 1,
        items: [
          { name: 'Item1', price: 10, qty: 2 },
          { name: 'Item2', price: 20, qty: 1 }
        ]
      },
      {
        id: 2,
        items: [
          { name: 'Item3', price: 15, qty: 3 }
        ]
      }
    ];

    // Calculate total for all orders
    const total = orders.reduce((orderTotal, order) => {
      const itemsTotal = order.items.reduce((itemTotal, item) => {
        return itemTotal + (item.price * item.qty);
      }, 0);
      return orderTotal + itemsTotal;
    }, 0);

    expect(total).to.equal(85); // (10*2 + 20*1) + (15*3)
  });

  // 10. Performance considerations
  it('should use efficient array methods', () => {
    const largeArray = Array.from({ length: 10000 }, (_, i) => i);

    // GOOD: Use find for early exit
    const start1 = Date.now();
    const found1 = largeArray.find(n => n === 100);
    const time1 = Date.now() - start1;

    // BAD: Filter checks all elements
    const start2 = Date.now();
    const found2 = largeArray.filter(n => n === 100)[0];
    const time2 = Date.now() - start2;

    expect(found1).to.equal(found2);
    cy.log(`find: ${time1}ms, filter: ${time2}ms`);

    // Use Set for lookups
    const set = new Set(largeArray);
    const hasValue = set.has(5000); // O(1)
    expect(hasValue).to.be.true;
  });
});

// Utility functions for arrays
const ArrayUtils = {
  // Remove duplicates
  unique: (arr) => [...new Set(arr)],

  // Shuffle array
  shuffle: (arr) => {
    const result = [...arr];
    for (let i = result.length - 1; i > 0; i--) {
      const j = Math.floor(Math.random() * (i + 1));
      [result[i], result[j]] = [result[j], result[i]];
    }
    return result;
  },

  // Partition array
  partition: (arr, predicate) => {
    return arr.reduce(([pass, fail], elem) => {
      return predicate(elem) ? [[...pass, elem], fail] : [pass, [...fail, elem]];
    }, [[], []]);
  },

  // Sample random elements
  sample: (arr, n) => {
    const shuffled = ArrayUtils.shuffle(arr);
    return shuffled.slice(0, n);
  },

  // Deep flatten
  deepFlatten: (arr) => {
    return arr.reduce((acc, val) => {
      return Array.isArray(val) ?
        acc.concat(ArrayUtils.deepFlatten(val)) :
        acc.concat(val);
    }, []);
  }
};

describe('Array Utility Functions', () => {
  it('should use utility functions', () => {
    const numbers = [1, 2, 2, 3, 3, 3, 4, 5];

    // Unique
    const unique = ArrayUtils.unique(numbers);
    expect(unique).to.deep.equal([1, 2, 3, 4, 5]);

    // Partition
    const [evens, odds] = ArrayUtils.partition(numbers, n => n % 2 === 0);
    expect(evens).to.deep.equal([2, 2, 4]);
    expect(odds).to.deep.equal([1, 3, 3, 3, 5]);

    // Deep flatten
    const nested = [1, [2, [3, [4, 5]]]];
    const flat = ArrayUtils.deepFlatten(nested);
    expect(flat).to.deep.equal([1, 2, 3, 4, 5]);
  });
});
```

**Common Mistakes to Avoid:**

1. Mutating arrays when you want immutability (use slice, spread, etc.)
2. Forgetting that `sort()` mutates the original array
3. Using `map()` when you don't need the return value (use `forEach()`)
4. Not understanding sparse arrays

**Interview Tips:**

- Know which methods mutate vs return new arrays
- Understand time complexity (find vs filter for single element)
- Master reduce for complex transformations
- Be familiar with modern methods (flat, flatMap, etc.)

**Tricky Follow-up Questions:**

**Q: What's the difference between these?**

```javascript
const arr = [1, 2, 3];

// Creates new array
const arr2 = arr.map(x => x * 2);

// Modifies in place
arr.forEach((x, i) => arr[i] = x * 2);

// Which to use?
// - Use map when you want a new array
// - Use forEach for side effects only
```

**Q: How do you remove duplicates from array of objects?**

```javascript
const users = [
  { id: 1, name: 'John' },
  { id: 2, name: 'Jane' },
  { id: 1, name: 'John' }
];

// Using Map
const uniqueUsers = [...new Map(users.map(u => [u.id, u])).values()];

// Using reduce
const uniqueUsers2 = users.reduce((acc, user) => {
  if (!acc.find(u => u.id === user.id)) {
    acc.push(user);
  }
  return acc;
}, []);

console.log(uniqueUsers); // [{ id: 1, name: 'John' }, { id: 2, name: 'Jane' }]
```

---


### Q13: Explain destructuring in JavaScript. How is it useful in test automation?

**Answer:**

Destructuring is a syntax that allows unpacking values from arrays or properties from objects into distinct variables.

**Array Destructuring:**

```javascript
// Basic array destructuring
const arr = [1, 2, 3, 4, 5];
const [first, second] = arr;
console.log(first); // 1
console.log(second); // 2

// Skip elements
const [a, , c] = arr;
console.log(a); // 1
console.log(c); // 3

// Rest operator
const [head, ...tail] = arr;
console.log(head); // 1
console.log(tail); // [2, 3, 4, 5]

// Default values
const [x, y, z = 10] = [1, 2];
console.log(z); // 10

// Swapping variables
let var1 = 'a';
let var2 = 'b';
[var1, var2] = [var2, var1];
console.log(var1); // 'b'
console.log(var2); // 'a'
```

**Object Destructuring:**

```javascript
// Basic object destructuring
const user = {
  name: 'John',
  age: 30,
  email: 'john@example.com'
};

const { name, age } = user;
console.log(name); // 'John'
console.log(age); // 30

// Rename variables
const { name: userName, age: userAge } = user;
console.log(userName); // 'John'

// Default values
const { name, role = 'user' } = user;
console.log(role); // 'user'

// Nested destructuring
const person = {
  name: 'Jane',
  address: {
    city: 'NYC',
    country: 'USA'
  }
};

const { name, address: { city, country } } = person;
console.log(city); // 'NYC'

// Rest in objects
const { name, ...rest } = user;
console.log(rest); // { age: 30, email: 'john@example.com' }
```

**Function Parameters:**

```javascript
// Array parameter destructuring
function sum([a, b]) {
  return a + b;
}

console.log(sum([10, 20])); // 30

// Object parameter destructuring
function createUser({ name, age, role = 'user' }) {
  return {
    name,
    age,
    role,
    createdAt: new Date()
  };
}

const newUser = createUser({ name: 'John', age: 30 });
console.log(newUser.role); // 'user'

// Nested parameters
function processResponse({ status, data: { users, total } }) {
  console.log(status); // 200
  console.log(users.length); // X
  console.log(total); // Y
}

processResponse({
  status: 200,
  data: {
    users: [{}, {}],
    total: 2
  }
});
```

**Cypress Testing Scenario:**

```javascript
describe('Destructuring in Test Automation', () => {
  // 1. API response destructuring
  it('should destructure API responses', () => {
    cy.request('/api/user/1').then(({ status, body }) => {
      expect(status).to.equal(200);

      const { id, name, email, address } = body;
      expect(id).to.be.a('number');
      expect(name).to.be.a('string');
      expect(email).to.include('@');

      // Nested destructuring
      const { city, zipCode } = address;
      expect(city).to.not.be.empty;
      expect(zipCode).to.match(/^\d{5}$/);
    });
  });

  // 2. Test data setup
  it('should use destructuring for test data', () => {
    const testUser = {
      username: 'testuser',
      email: 'test@example.com',
      password: 'Test123!',
      profile: {
        firstName: 'Test',
        lastName: 'User',
        age: 25
      }
    };

    const {
      username,
      email,
      password,
      profile: { firstName, lastName }
    } = testUser;

    cy.visit('/register');
    cy.get('[name="username"]').type(username);
    cy.get('[name="email"]').type(email);
    cy.get('[name="password"]').type(password);
    cy.get('[name="firstName"]').type(firstName);
    cy.get('[name="lastName"]').type(lastName);
  });

  // 3. Array destructuring in element operations
  it('should destructure element arrays', () => {
    cy.get('.user-item').then(($elements) => {
      const [first, second, ...others] = $elements;

      cy.wrap(first).should('have.class', 'first');
      cy.wrap(second).should('be.visible');
      expect(others.length).to.be.greaterThan(0);
    });
  });

  // 4. Configuration objects
  it('should destructure configuration', () => {
    const config = {
      baseUrl: 'http://localhost:3000',
      timeout: 10000,
      retries: 3,
      env: {
        apiUrl: 'http://api.localhost',
        apiKey: 'test-key'
      }
    };

    const {
      baseUrl,
      timeout = 5000,
      retries = 1,
      env: { apiUrl, apiKey }
    } = config;

    cy.request({
      url: `${apiUrl}/users`,
      timeout,
      headers: { 'X-API-Key': apiKey }
    });
  });

  // 5. Custom command with destructured params
  Cypress.Commands.add('login', ({ username, password, rememberMe = false }) => {
    cy.visit('/login');
    cy.get('[name="username"]').type(username);
    cy.get('[name="password"]').type(password);

    if (rememberMe) {
      cy.get('[name="remember"]').check();
    }

    cy.get('[type="submit"]').click();
  });

  it('should use custom command with destructuring', () => {
    cy.login({
      username: 'testuser',
      password: 'Test123!',
      rememberMe: true
    });
  });

  // 6. Iterating with destructuring
  it('should destructure in iterations', () => {
    const users = [
      { id: 1, name: 'John', role: 'admin' },
      { id: 2, name: 'Jane', role: 'user' },
      { id: 3, name: 'Bob', role: 'user' }
    ];

    users.forEach(({ id, name, role }) => {
      cy.log(`User ${id}: ${name} (${role})`);

      if (role === 'admin') {
        cy.log(`${name} has admin privileges`);
      }
    });

    // In map
    const userNames = users.map(({ name }) => name);
    expect(userNames).to.deep.equal(['John', 'Jane', 'Bob']);

    // In filter
    const admins = users.filter(({ role }) => role === 'admin');
    expect(admins).to.have.length(1);
  });

  // 7. Destructuring with defaults for missing data
  it('should handle missing data with defaults', () => {
    const partialUser = {
      name: 'John'
      // email and role are missing
    };

    const {
      name,
      email = 'noemail@example.com',
      role = 'guest'
    } = partialUser;

    expect(name).to.equal('John');
    expect(email).to.equal('noemail@example.com');
    expect(role).to.equal('guest');
  });

  // 8. Renaming during destructuring
  it('should rename variables during destructuring', () => {
    cy.request('/api/user/1').then((response) => {
      const {
        body: userData,
        status: httpStatus,
        headers: responseHeaders
      } = response;

      expect(httpStatus).to.equal(200);
      expect(userData).to.have.property('name');
      expect(responseHeaders).to.have.property('content-type');
    });
  });

  // 9. Destructuring in complex data structures
  it('should destructure complex nested structures', () => {
    const apiResponse = {
      status: 'success',
      data: {
        users: [
          {
            id: 1,
            profile: {
              personal: {
                name: 'John',
                age: 30
              },
              contact: {
                email: 'john@example.com',
                phone: '123-456-7890'
              }
            }
          }
        ],
        pagination: {
          total: 1,
          page: 1,
          perPage: 10
        }
      }
    };

    const {
      status,
      data: {
        users: [{
          profile: {
            personal: { name, age },
            contact: { email }
          }
        }],
        pagination: { total }
      }
    } = apiResponse;

    expect(status).to.equal('success');
    expect(name).to.equal('John');
    expect(age).to.equal(30);
    expect(email).to.equal('john@example.com');
    expect(total).to.equal(1);
  });

  // 10. Rest/Spread with destructuring
  it('should use rest and spread patterns', () => {
    const allUsers = [
      { id: 1, name: 'John', role: 'admin' },
      { id: 2, name: 'Jane', role: 'user' },
      { id: 3, name: 'Bob', role: 'user' }
    ];

    // Extract first user, keep rest
    const [admin, ...regularUsers] = allUsers;
    expect(admin.role).to.equal('admin');
    expect(regularUsers).to.have.length(2);

    // Extract specific properties, keep rest
    const { id, ...userWithoutId } = allUsers[0];
    expect(id).to.equal(1);
    expect(userWithoutId).to.not.have.property('id');

    // Combine with spread
    const newUser = {
      ...userWithoutId,
      id: 999,
      verified: true
    };

    expect(newUser.id).to.equal(999);
    expect(newUser.name).to.equal('John');
    expect(newUser.verified).to.be.true;
  });
});

// Helper functions using destructuring
const TestHelpers = {
  // Validate user object
  validateUser: ({ id, name, email }) => {
    return (
      typeof id === 'number' &&
      typeof name === 'string' && name.length > 0 &&
      typeof email === 'string' && email.includes('@')
    );
  },

  // Create API request config
  createRequest: ({
    method = 'GET',
    url,
    headers = {},
    body,
    timeout = 10000
  }) => {
    return {
      method,
      url,
      headers: {
        'Content-Type': 'application/json',
        ...headers
      },
      ...(body && { body }),
      timeout
    };
  },

  // Extract specific fields
  extractFields: (obj, fields) => {
    return fields.reduce((acc, field) => {
      if (field in obj) {
        acc[field] = obj[field];
      }
      return acc;
    }, {});
  }
};

describe('Helper Functions with Destructuring', () => {
  it('should use helper functions', () => {
    // Validate user
    const user = { id: 1, name: 'John', email: 'john@example.com' };
    const isValid = TestHelpers.validateUser(user);
    expect(isValid).to.be.true;

    // Create request
    const requestConfig = TestHelpers.createRequest({
      url: '/api/users',
      method: 'POST',
      body: { name: 'Test' }
    });

    expect(requestConfig.method).to.equal('POST');
    expect(requestConfig.headers['Content-Type']).to.equal('application/json');
  });
});
```

**Advanced Patterns:**

```javascript
// 1. Destructuring with computed property names
const key = 'username';
const obj = { username: 'john', email: 'john@example.com' };
const { [key]: value } = obj;
console.log(value); // 'john'

// 2. Destructuring in catch blocks
try {
  throw new Error('Something went wrong');
} catch ({ message, stack }) {
  console.log(message); // 'Something went wrong'
}

// 3. Mixed array and object destructuring
const response = {
  data: ['value1', 'value2', 'value3']
};

const { data: [first, second] } = response;
console.log(first); // 'value1'

// 4. Destructuring for variable swapping
let a = 1, b = 2, c = 3;
[a, b, c] = [b, c, a];
console.log(a, b, c); // 2 3 1
```

**Common Mistakes to Avoid:**

1. Destructuring `undefined` or `null` (causes TypeError)
2. Too deep nesting making code hard to read
3. Not using default values when data might be missing
4. Confusion between renaming and default values

**Interview Tips:**

- Destructuring makes code cleaner and more readable
- Use defaults to handle missing data
- Common in React, modern frameworks, and test automation
- Combines well with spread/rest operators

**Tricky Follow-up Questions:**

**Q: What happens if you destructure undefined?**

```javascript
const { name } = undefined; // TypeError: Cannot destructure property 'name' of 'undefined'

// Safe destructuring
const obj = undefined;
const { name } = obj || {}; // No error, name is undefined

// Or with optional chaining
const { name } = obj ?? {}; // No error, name is undefined
```

**Q: How do you destructure and rename with defaults?**

```javascript
const user = { username: 'john' };

// Rename AND provide default
const { username: name, email: userEmail = 'no-email' } = user;

console.log(name); // 'john'
console.log(userEmail); // 'no-email'
```

---

### Q14: What is the spread operator and rest parameters? Provide test automation examples.

**Answer:**

The spread operator (`...`) and rest parameters use the same syntax but serve different purposes:
- **Spread**: Expands an iterable into individual elements
- **Rest**: Collects multiple elements into an array

**Spread Operator:**

```javascript
// 1. Array spreading
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];

// Combine arrays
const combined = [...arr1, ...arr2];
console.log(combined); // [1, 2, 3, 4, 5, 6]

// Copy array
const copy = [...arr1];
console.log(copy); // [1, 2, 3]

// Add elements
const extended = [0, ...arr1, 4];
console.log(extended); // [0, 1, 2, 3, 4]

// 2. Object spreading
const obj1 = { a: 1, b: 2 };
const obj2 = { c: 3, d: 4 };

// Combine objects
const merged = { ...obj1, ...obj2 };
console.log(merged); // { a: 1, b: 2, c: 3, d: 4 }

// Override properties
const updated = { ...obj1, b: 20, e: 5 };
console.log(updated); // { a: 1, b: 20, e: 5 }

// Clone object
const cloned = { ...obj1 };

// 3. Function arguments
function sum(a, b, c) {
  return a + b + c;
}

const numbers = [1, 2, 3];
console.log(sum(...numbers)); // 6

// 4. String spreading
const str = 'hello';
const chars = [...str];
console.log(chars); // ['h', 'e', 'l', 'l', 'o']
```

**Rest Parameters:**

```javascript
// 1. Function rest parameters
function sum(...numbers) {
  return numbers.reduce((total, num) => total + num, 0);
}

console.log(sum(1, 2, 3)); // 6
console.log(sum(1, 2, 3, 4, 5)); // 15

// 2. Mixed parameters
function greet(greeting, ...names) {
  return `${greeting} ${names.join(', ')}!`;
}

console.log(greet('Hello', 'John', 'Jane', 'Bob')); 
// "Hello John, Jane, Bob!"

// 3. Array destructuring with rest
const [first, second, ...rest] = [1, 2, 3, 4, 5];
console.log(first); // 1
console.log(second); // 2
console.log(rest); // [3, 4, 5]

// 4. Object destructuring with rest
const { name, age, ...otherProps } = {
  name: 'John',
  age: 30,
  email: 'john@example.com',
  phone: '123-456-7890'
};

console.log(name); // 'John'
console.log(age); // 30
console.log(otherProps); // { email: '...', phone: '...' }
```

**Cypress Testing Scenario:**

```javascript
describe('Spread and Rest in Test Automation', () => {
  // 1. Merging test configurations
  it('should merge configurations with spread', () => {
    const defaultConfig = {
      baseUrl: 'http://localhost',
      timeout: 5000,
      retries: 1
    };

    const testConfig = {
      timeout: 10000,
      headers: { 'Authorization': 'Bearer token' }
    };

    // Override defaults
    const finalConfig = { ...defaultConfig, ...testConfig };

    expect(finalConfig.baseUrl).to.equal('http://localhost');
    expect(finalConfig.timeout).to.equal(10000); // Overridden
    expect(finalConfig.headers).to.exist;

    cy.request({
      url: `${finalConfig.baseUrl}/api/users`,
      timeout: finalConfig.timeout,
      headers: finalConfig.headers
    });
  });

  // 2. Cloning test data
  it('should clone test data safely', () => {
    const originalUser = {
      id: 1,
      name: 'John',
      email: 'john@example.com'
    };

    // Shallow clone
    const userCopy = { ...originalUser };
    userCopy.name = 'Jane';

    expect(originalUser.name).to.equal('John'); // Unchanged
    expect(userCopy.name).to.equal('Jane');

    // Add properties
    const extendedUser = {
      ...originalUser,
      verified: true,
      createdAt: new Date()
    };

    expect(extendedUser.verified).to.be.true;
    expect(extendedUser.name).to.equal('John');
  });

  // 3. Array operations
  it('should manipulate arrays with spread', () => {
    const users1 = [{ id: 1 }, { id: 2 }];
    const users2 = [{ id: 3 }, { id: 4 }];

    // Combine arrays
    const allUsers = [...users1, ...users2];
    expect(allUsers).to.have.length(4);

    // Add elements
    const newUser = { id: 5 };
    const updatedUsers = [...allUsers, newUser];
    expect(updatedUsers).to.have.length(5);

    // Insert in middle
    const withMiddle = [
      ...allUsers.slice(0, 2),
      { id: 99 },
      ...allUsers.slice(2)
    ];
    expect(withMiddle[2].id).to.equal(99);

    // Remove duplicates
    const withDuplicates = [1, 2, 2, 3, 3, 3];
    const unique = [...new Set(withDuplicates)];
    expect(unique).to.deep.equal([1, 2, 3]);
  });

  // 4. Variable arguments in helpers
  const createRequest = (method, url, ...options) => {
    const [headers, body, timeout] = options;

    return cy.request({
      method,
      url,
      ...(headers && { headers }),
      ...(body && { body }),
      ...(timeout && { timeout })
    });
  };

  it('should use rest parameters in helpers', () => {
    // Call with different number of arguments
    createRequest('GET', '/api/users');
    createRequest('GET', '/api/users', { 'X-Custom': 'header' });
    createRequest('POST', '/api/users', 
      { 'Content-Type': 'application/json' },
      { name: 'John' }
    );
  });

  // 5. Flexible assertion helpers
  const assertAllTrue = (...conditions) => {
    conditions.forEach((condition, index) => {
      expect(condition, `Condition ${index} should be true`).to.be.true;
    });
  };

  it('should use rest for flexible assertions', () => {
    const user = { id: 1, name: 'John', email: 'john@example.com', verified: true };

    assertAllTrue(
      user.id > 0,
      user.name.length > 0,
      user.email.includes('@'),
      user.verified === true
    );
  });

  // 6. Extracting and spreading properties
  it('should extract and spread properties', () => {
    const fullUser = {
      id: 1,
      password: 'secret',
      name: 'John',
      email: 'john@example.com',
      role: 'user'
    };

    // Remove sensitive data
    const { password, ...safeUser } = fullUser;

    expect(safeUser).to.not.have.property('password');
    expect(safeUser).to.have.property('name');

    // Send safe data
    cy.request({
      method: 'POST',
      url: '/api/public/users',
      body: safeUser
    });
  });

  // 7. Function composition with spread
  const addTimestamp = (obj) => ({ ...obj, timestamp: Date.now() });
  const addId = (obj) => ({ ...obj, id: Math.random() });
  const addStatus = (obj) => ({ ...obj, status: 'active' });

  it('should compose functions with spread', () => {
    const baseUser = { name: 'John', email: 'john@example.com' };

    const enrichedUser = [addTimestamp, addId, addStatus]
      .reduce((user, fn) => fn(user), baseUser);

    expect(enrichedUser).to.have.property('timestamp');
    expect(enrichedUser).to.have.property('id');
    expect(enrichedUser).to.have.property('status');
    expect(enrichedUser.name).to.equal('John');
  });

  // 8. Conditional spreading
  it('should conditionally spread properties', () => {
    const includeAuth = true;
    const includeTimeout = false;

    const requestConfig = {
      method: 'GET',
      url: '/api/users',
      ...(includeAuth && { headers: { 'Authorization': 'Bearer token' } }),
      ...(includeTimeout && { timeout: 10000 })
    };

    expect(requestConfig.headers).to.exist;
    expect(requestConfig.timeout).to.be.undefined;
  });

  // 9. Rest in array destructuring
  it('should use rest in destructuring', () => {
    cy.get('.user-item').then(($elements) => {
      const [first, ...others] = Array.from($elements);

      cy.wrap(first).should('have.class', 'first');
      expect(others.length).to.be.greaterThan(0);

      others.forEach((el) => {
        cy.wrap(el).should('not.have.class', 'first');
      });
    });
  });

  // 10. Complex data manipulation
  it('should perform complex operations with spread/rest', () => {
    const orders = [
      { id: 1, items: ['item1', 'item2'], total: 100 },
      { id: 2, items: ['item3'], total: 50 },
      { id: 3, items: ['item4', 'item5', 'item6'], total: 150 }
    ];

    // Extract all items
    const allItems = orders.flatMap(({ items }) => items);
    expect(allItems).to.have.length(6);

    // Sum totals
    const grandTotal = orders.reduce((sum, { total }) => sum + total, 0);
    expect(grandTotal).to.equal(300);

    // Update orders with discount
    const discountedOrders = orders.map(order => ({
      ...order,
      total: order.total * 0.9,
      discounted: true
    }));

    expect(discountedOrders[0].total).to.equal(90);
    expect(discountedOrders[0].discounted).to.be.true;
  });
});

// Utility functions using spread/rest
const TestUtils = {
  // Merge multiple objects
  mergeAll: (...objects) => Object.assign({}, ...objects),

  // Create array from arguments
  toArray: (...items) => items,

  // Log multiple values
  log: (...messages) => {
    messages.forEach(msg => cy.log(String(msg)));
  },

  // Pick properties from object
  pick: (obj, ...keys) => {
    return keys.reduce((acc, key) => {
      if (key in obj) {
        acc[key] = obj[key];
      }
      return acc;
    }, {});
  },

  // Omit properties from object
  omit: (obj, ...keys) => {
    const { ...rest } = obj;
    keys.forEach(key => delete rest[key]);
    return rest;
  },

  // Flatten arrays
  flatten: (...arrays) => {
    return arrays.reduce((acc, arr) => [...acc, ...arr], []);
  }
};

describe('Utility Functions with Spread/Rest', () => {
  it('should use utility functions', () => {
    const obj1 = { a: 1, b: 2 };
    const obj2 = { c: 3, d: 4 };
    const obj3 = { e: 5 };

    // Merge all
    const merged = TestUtils.mergeAll(obj1, obj2, obj3);
    expect(merged).to.deep.equal({ a: 1, b: 2, c: 3, d: 4, e: 5 });

    // Pick properties
    const picked = TestUtils.pick(merged, 'a', 'c', 'e');
    expect(picked).to.deep.equal({ a: 1, c: 3, e: 5 });

    // Omit properties
    const omitted = TestUtils.omit(merged, 'b', 'd');
    expect(omitted).to.deep.equal({ a: 1, c: 3, e: 5 });

    // Flatten
    const flattened = TestUtils.flatten([1, 2], [3, 4], [5]);
    expect(flattened).to.deep.equal([1, 2, 3, 4, 5]);
  });
});
```

**Deep vs Shallow Copy:**

```javascript
// Shallow copy - nested objects are referenced
const original = {
  name: 'John',
  address: {
    city: 'NYC'
  }
};

const shallow = { ...original };
shallow.address.city = 'LA';

console.log(original.address.city); // 'LA' (modified!)

// Deep copy options
// 1. JSON (doesn't work with functions, dates, undefined)
const deep1 = JSON.parse(JSON.stringify(original));

// 2. Structured clone (modern)
const deep2 = structuredClone(original);

// 3. Manual deep clone
const deepClone = (obj) => {
  if (obj === null || typeof obj !== 'object') return obj;
  if (Array.isArray(obj)) return obj.map(deepClone);

  return Object.fromEntries(
    Object.entries(obj).map(([key, value]) => [key, deepClone(value)])
  );
};
```

**Common Mistakes to Avoid:**

1. Thinking spread creates deep copies (it's shallow!)
2. Using rest parameter not as last parameter
3. Spreading objects with non-enumerable properties
4. Performance issues with large arrays/objects

**Interview Tips:**

- Spread and rest use same syntax (`...`) but different contexts
- Spread is for expanding, rest is for collecting
- Very common in modern JavaScript and React
- Understand shallow vs deep copying

**Tricky Follow-up Questions:**

**Q: What's the difference between these?**

```javascript
// Spread in array literal
const arr1 = [...[1, 2, 3]]; // [1, 2, 3]

// Rest in array destructuring
const [a, ...arr2] = [1, 2, 3]; // a = 1, arr2 = [2, 3]

// Spread in function call
Math.max(...[1, 2, 3]); // 3

// Rest in function parameters
function sum(...numbers) { // numbers = [1, 2, 3]
  return numbers.reduce((a, b) => a + b);
}
```

**Q: How do you deep clone an object with spread?**

```javascript
// WRONG: Shallow copy
const original = { a: 1, b: { c: 2 } };
const copy = { ...original };
copy.b.c = 3;
console.log(original.b.c); // 3 (modified!)

// RIGHT: Manual deep clone for one level
const deepCopy = {
  ...original,
  b: { ...original.b }
};
deepCopy.b.c = 3;
console.log(original.b.c); // 2 (not modified)

// BEST: Use structured clone or library
const realDeepCopy = structuredClone(original);
```

---

### Q15: Explain Map, Set, WeakMap, and WeakSet. When would you use them in tests?

**Answer:**

ES6 introduced new collection types that provide better performance and functionality for specific use cases.

**Map:**

A collection of key-value pairs where keys can be any type (not just strings).

```javascript
// Creating Maps
const map = new Map();

// Setting values
map.set('name', 'John');
map.set(1, 'one');
map.set(true, 'boolean');
map.set({ id: 1 }, 'object key');

// Getting values
console.log(map.get('name')); // 'John'
console.log(map.get(1)); // 'one'

// Checking existence
console.log(map.has('name')); // true
console.log(map.has('age')); // false

// Size
console.log(map.size); // 4

// Deleting
map.delete('name');
console.log(map.has('name')); // false

// Clearing all
map.clear();
console.log(map.size); // 0

// Iterating
const userMap = new Map([
  ['john', { age: 30, email: 'john@example.com' }],
  ['jane', { age: 25, email: 'jane@example.com' }]
]);

// For...of
for (const [key, value] of userMap) {
  console.log(`${key}:`, value);
}

// forEach
userMap.forEach((value, key) => {
  console.log(`${key}:`, value);
});

// Keys, Values, Entries
console.log([...userMap.keys()]); // ['john', 'jane']
console.log([...userMap.values()]); // [{ age: 30... }, { age: 25... }]
console.log([...userMap.entries()]); // [['john', {...}], ['jane', {...}]]
```

**Set:**

A collection of unique values.

```javascript
// Creating Sets
const set = new Set();

// Adding values
set.add(1);
set.add(2);
set.add(2); // Duplicate, ignored
set.add(3);

console.log(set.size); // 3
console.log(set.has(2)); // true

// From array
const arr = [1, 2, 2, 3, 3, 3, 4];
const uniqueSet = new Set(arr);
console.log([...uniqueSet]); // [1, 2, 3, 4]

// Deleting
set.delete(2);
console.log(set.has(2)); // false

// Iterating
for (const value of set) {
  console.log(value);
}

set.forEach(value => console.log(value));

// Set operations
const setA = new Set([1, 2, 3]);
const setB = new Set([3, 4, 5]);

// Union
const union = new Set([...setA, ...setB]); // {1, 2, 3, 4, 5}

// Intersection
const intersection = new Set([...setA].filter(x => setB.has(x))); // {3}

// Difference
const difference = new Set([...setA].filter(x => !setB.has(x))); // {1, 2}
```

**WeakMap:**

Like Map, but keys must be objects and are weakly held (can be garbage collected).

```javascript
const weakMap = new WeakMap();

let obj = { id: 1 };
weakMap.set(obj, 'some value');

console.log(weakMap.get(obj)); // 'some value'
console.log(weakMap.has(obj)); // true

// When obj is garbage collected, entry is automatically removed
obj = null; // Entry can be garbage collected
```

**WeakSet:**

Like Set, but only holds objects weakly.

```javascript
const weakSet = new WeakSet();

let obj1 = { id: 1 };
let obj2 = { id: 2 };

weakSet.add(obj1);
weakSet.add(obj2);

console.log(weakSet.has(obj1)); // true

obj1 = null; // Can be garbage collected
```

**Cypress Testing Scenario:**

```javascript
describe('Map, Set, WeakMap, WeakSet in Tests', () => {
  // 1. Using Map for test data storage
  it('should use Map for test data', () => {
    const testData = new Map();

    testData.set('admin', {
      username: 'admin',
      password: 'Admin123!',
      role: 'admin'
    });

    testData.set('user', {
      username: 'user',
      password: 'User123!',
      role: 'user'
    });

    // Get data
    const adminData = testData.get('admin');

    cy.visit('/login');
    cy.get('[name="username"]').type(adminData.username);
    cy.get('[name="password"]').type(adminData.password);

    // Check all roles
    expect(testData.size).to.equal(2);
    expect(testData.has('admin')).to.be.true;
  });

  // 2. Using Set for unique values
  it('should use Set for unique validation', () => {
    cy.request('/api/users').then((response) => {
      const users = response.body;

      // Check unique IDs
      const ids = users.map(u => u.id);
      const uniqueIds = new Set(ids);

      expect(uniqueIds.size).to.equal(ids.length, 'All IDs should be unique');

      // Check unique emails
      const emails = users.map(u => u.email);
      const uniqueEmails = new Set(emails);

      expect(uniqueEmails.size).to.equal(emails.length, 'All emails should be unique');
    });
  });

  // 3. Map with object keys
  it('should use Map with object keys', () => {
    const elementStateMap = new Map();

    cy.get('.user-item').each(($el) => {
      const element = $el[0]; // DOM element

      elementStateMap.set(element, {
        checked: false,
        validated: false,
        timestamp: Date.now()
      });
    }).then(() => {
      expect(elementStateMap.size).to.be.greaterThan(0);

      // Update state
      elementStateMap.forEach((state, element) => {
        state.checked = true;
      });
    });
  });

  // 4. Caching with Map
  it('should use Map for caching', () => {
    const cache = new Map();

    const fetchUser = (id) => {
      if (cache.has(id)) {
        cy.log(`Cache hit for user ${id}`);
        return cy.wrap(cache.get(id));
      }

      cy.log(`Cache miss for user ${id}`);
      return cy.request(`/api/users/${id}`).then((response) => {
        cache.set(id, response.body);
        return response.body;
      });
    };

    // First call - fetches from API
    fetchUser(1).then((user) => {
      expect(user).to.have.property('id', 1);
    });

    // Second call - gets from cache
    fetchUser(1).then((user) => {
      expect(user).to.have.property('id', 1);
      expect(cache.size).to.equal(1);
    });
  });

  // 5. Set operations for data comparison
  it('should use Set for data comparison', () => {
    const expectedIds = new Set([1, 2, 3, 4, 5]);

    cy.request('/api/users').then((response) => {
      const actualIds = new Set(response.body.map(u => u.id));

      // Check if all expected IDs are present
      const hasAllExpected = [...expectedIds].every(id => actualIds.has(id));
      expect(hasAllExpected).to.be.true;

      // Find missing IDs
      const missingIds = [...expectedIds].filter(id => !actualIds.has(id));
      expect(missingIds).to.be.empty;

      // Find unexpected IDs
      const unexpectedIds = [...actualIds].filter(id => !expectedIds.has(id));
      cy.log(`Unexpected IDs: ${[...unexpectedIds]}`);
    });
  });

  // 6. WeakMap for metadata
  it('should use WeakMap for element metadata', () => {
    const elementMetadata = new WeakMap();

    cy.get('.user-item').each(($el) => {
      const element = $el[0];

      // Store metadata
      elementMetadata.set(element, {
        clickCount: 0,
        lastClicked: null
      });

      cy.wrap($el).click().then(() => {
        const meta = elementMetadata.get(element);
        meta.clickCount++;
        meta.lastClicked = new Date();
      });
    });

    // Metadata automatically cleaned when elements are removed
  });

  // 7. Removing duplicates with Set
  it('should remove duplicates with Set', () => {
    const users = [
      { id: 1, name: 'John' },
      { id: 2, name: 'Jane' },
      { id: 1, name: 'John' }, // Duplicate
      { id: 3, name: 'Bob' }
    ];

    // Remove duplicate IDs
    const uniqueUsers = Array.from(
      new Map(users.map(u => [u.id, u])).values()
    );

    expect(uniqueUsers).to.have.length(3);
    expect(uniqueUsers.map(u => u.id)).to.deep.equal([1, 2, 3]);
  });

  // 8. Map for configuration
  it('should use Map for configuration', () => {
    const config = new Map([
      ['apiUrl', 'http://api.example.com'],
      ['timeout', 10000],
      ['retries', 3],
      ['headers', { 'Content-Type': 'application/json' }]
    ]);

    cy.request({
      url: `${config.get('apiUrl')}/users`,
      timeout: config.get('timeout'),
      headers: config.get('headers')
    });

    // Easy to update
    config.set('timeout', 20000);
    expect(config.get('timeout')).to.equal(20000);
  });

  // 9. Set for visited URLs
  it('should track visited URLs with Set', () => {
    const visitedUrls = new Set();

    const visitUnique = (url) => {
      if (visitedUrls.has(url)) {
        cy.log(`Already visited: ${url}`);
        return cy.wrap(null);
      }

      visitedUrls.add(url);
      return cy.visit(url);
    };

    visitUnique('/page1');
    visitUnique('/page2');
    visitUnique('/page1'); // Skipped

    cy.wrap(null).then(() => {
      expect(visitedUrls.size).to.equal(2);
    });
  });

  // 10. Map vs Object comparison
  it('should understand Map vs Object differences', () => {
    // Object - limited to string/symbol keys
    const obj = {
      name: 'John',
      age: 30
    };

    // Map - any type as key
    const map = new Map([
      ['name', 'John'],
      ['age', 30],
      [1, 'number key'],
      [true, 'boolean key'],
      [{ id: 1 }, 'object key']
    ]);

    expect(Object.keys(obj).length).to.equal(2);
    expect(map.size).to.equal(5);

    // Map preserves insertion order
    const keys = [...map.keys()];
    expect(keys[0]).to.equal('name');

    // Map is iterable
    for (const [key, value] of map) {
      cy.log(`${key}: ${value}`);
    }

    // Object requires Object.entries()
    for (const [key, value] of Object.entries(obj)) {
      cy.log(`${key}: ${value}`);
    }
  });
});

// Utility functions with Map and Set
const CollectionUtils = {
  // Create Map from array of objects
  toMap: (arr, keyFn) => {
    return new Map(arr.map(item => [keyFn(item), item]));
  },

  // Create lookup Set
  toLookupSet: (arr, keyFn) => {
    return new Set(arr.map(keyFn));
  },

  // Intersect Sets
  intersect: (setA, setB) => {
    return new Set([...setA].filter(x => setB.has(x)));
  },

  // Union Sets
  union: (setA, setB) => {
    return new Set([...setA, ...setB]);
  },

  // Difference Sets
  difference: (setA, setB) => {
    return new Set([...setA].filter(x => !setB.has(x)));
  }
};

describe('Collection Utilities', () => {
  it('should use collection utilities', () => {
    const users = [
      { id: 1, name: 'John' },
      { id: 2, name: 'Jane' },
      { id: 3, name: 'Bob' }
    ];

    // Convert to Map
    const userMap = CollectionUtils.toMap(users, u => u.id);
    expect(userMap.get(1).name).to.equal('John');

    // Create lookup Set
    const idSet = CollectionUtils.toLookupSet(users, u => u.id);
    expect(idSet.has(1)).to.be.true;
    expect(idSet.has(99)).to.be.false;

    // Set operations
    const setA = new Set([1, 2, 3, 4]);
    const setB = new Set([3, 4, 5, 6]);

    const intersection = CollectionUtils.intersect(setA, setB);
    expect([...intersection]).to.deep.equal([3, 4]);

    const union = CollectionUtils.union(setA, setB);
    expect([...union]).to.deep.equal([1, 2, 3, 4, 5, 6]);

    const difference = CollectionUtils.difference(setA, setB);
    expect([...difference]).to.deep.equal([1, 2]);
  });
});
```

**Comparison Table:**

| Feature | Object | Map | Set | WeakMap | WeakSet |
|---------|--------|-----|-----|---------|---------|
| Key Types | String/Symbol | Any | N/A | Object only | N/A |
| Value Types | Any | Any | Any (unique) | Any | Object only |
| Size Property | No | Yes | Yes | No | No |
| Iterable | No | Yes | Yes | No | No |
| Ordered | No | Yes | Yes | No | No |
| Garbage Collection | No | No | No | Yes | Yes |
| Use Case | General | Key-value with any key | Unique values | Private data | Object tracking |

**Common Mistakes to Avoid:**

1. Using objects when Map would be better (non-string keys)
2. Not using Set for uniqueness checks
3. Trying to iterate WeakMap/WeakSet
4. Not understanding garbage collection in Weak collections

**Interview Tips:**

- Map is better than Object for dynamic key-value pairs
- Set is perfect for unique values
- WeakMap/WeakSet prevent memory leaks in certain scenarios
- Know when to use each collection type

**Tricky Follow-up Questions:**

**Q: When would you use WeakMap over Map?**

A: Use WeakMap when:
- Keys are objects that might be garbage collected
- You want to store private data
- You don't need to iterate over entries

```javascript
// Example: Store private data
const privateData = new WeakMap();

class User {
  constructor(name) {
    privateData.set(this, { password: 'secret' });
    this.name = name;
  }

  getPassword() {
    return privateData.get(this).password;
  }
}

const user = new User('John');
console.log(user.password); // undefined (private)
console.log(user.getPassword()); // 'secret'
```

**Q: How do you convert Map to Object and vice versa?**

```javascript
// Object to Map
const obj = { a: 1, b: 2, c: 3 };
const map = new Map(Object.entries(obj));

// Map to Object
const mapToObj = Object.fromEntries(map);

console.log(map); // Map(3) { 'a' => 1, 'b' => 2, 'c' => 3 }
console.log(mapToObj); // { a: 1, b: 2, c: 3 }
```

---


## Section 4: Asynchronous JavaScript

### Q16: Explain the Event Loop. How does JavaScript handle asynchronous operations?

**Answer:**

JavaScript is single-threaded but can handle asynchronous operations through the Event Loop mechanism.

**Event Loop Components:**

1. **Call Stack**: Where function calls are executed
2. **Web APIs**: Browser APIs (setTimeout, fetch, DOM events)
3. **Task Queue (Macro tasks)**: setTimeout, setInterval callbacks
4. **Microtask Queue**: Promises, queueMicrotask
5. **Event Loop**: Coordinates execution between queues

**Execution Order:**

```
1. Execute synchronous code (Call Stack)
2. Execute all Microtasks
3. Render (if needed)
4. Execute one Macrotask
5. Repeat from step 2
```

**Code Examples:**

```javascript
// Basic event loop
console.log('1'); // Synchronous

setTimeout(() => {
  console.log('2'); // Macrotask
}, 0);

Promise.resolve().then(() => {
  console.log('3'); // Microtask
});

console.log('4'); // Synchronous

// Output: 1, 4, 3, 2
// Explanation:
// 1. Sync code: 1, 4
// 2. Microtasks: 3
// 3. Macrotasks: 2
```

**Detailed Example:**

```javascript
console.log('Start');

setTimeout(() => {
  console.log('Timeout 1');
}, 0);

Promise.resolve()
  .then(() => {
    console.log('Promise 1');
    return Promise.resolve();
  })
  .then(() => {
    console.log('Promise 2');
  });

setTimeout(() => {
  console.log('Timeout 2');
  Promise.resolve().then(() => {
    console.log('Promise in Timeout');
  });
}, 0);

Promise.resolve().then(() => {
  console.log('Promise 3');
});

console.log('End');

// Output:
// Start
// End
// Promise 1
// Promise 3
// Promise 2
// Timeout 1
// Timeout 2
// Promise in Timeout
```

**Task Types:**

| Type | Examples | Priority |
|------|----------|----------|
| Synchronous | Regular code | Highest (immediate) |
| Microtasks | Promises, queueMicrotask, MutationObserver | High (after sync, before render) |
| Macrotasks | setTimeout, setInterval, setImmediate | Low (after microtasks) |
| Rendering | DOM updates, paint | Between macro and micro |

**Cypress Testing Scenario:**

```javascript
describe('Event Loop in Cypress', () => {
  it('should understand async execution order', () => {
    const executionOrder = [];

    // Synchronous
    executionOrder.push('sync-1');

    // Cypress commands are queued
    cy.wrap(null).then(() => {
      executionOrder.push('cypress-1');
    });

    // More sync code
    executionOrder.push('sync-2');

    // Promise
    Promise.resolve().then(() => {
      executionOrder.push('promise-1');
    });

    // setTimeout (macrotask)
    setTimeout(() => {
      executionOrder.push('timeout-1');
    }, 0);

    executionOrder.push('sync-3');

    cy.wrap(null).then(() => {
      executionOrder.push('cypress-2');

      // Check execution order
      cy.log(executionOrder.join(' -> '));

      // Note: Cypress commands run after microtasks
      expect(executionOrder).to.include('sync-1');
      expect(executionOrder).to.include('sync-2');
      expect(executionOrder).to.include('sync-3');
    });
  });

  it('should handle promise chains correctly', () => {
    const results = [];

    Promise.resolve(1)
      .then((value) => {
        results.push(value);
        return value + 1;
      })
      .then((value) => {
        results.push(value);
        return value + 1;
      })
      .then((value) => {
        results.push(value);
      });

    cy.wrap(null).then(() => {
      // Promises complete before next Cypress command
      expect(results).to.deep.equal([1, 2, 3]);
    });
  });

  it('should understand Cypress command queue', () => {
    cy.log('Command 1'); // Queued

    cy.wrap(null).then(() => {
      cy.log('Command 2'); // Queued inside then()
    });

    cy.log('Command 3'); // Queued

    // Commands execute in order: 1, 2, 3
    // Even though Command 2 is inside then()
  });

  it('should handle mixed async operations', () => {
    let counter = 0;

    cy.wrap(null).then(() => {
      counter++;
      cy.log(`Counter: ${counter}`); // 1
    });

    cy.wrap(null).then(() => {
      counter++;

      return new Promise((resolve) => {
        setTimeout(() => {
          counter++;
          resolve();
        }, 100);
      });
    });

    cy.wrap(null).then(() => {
      expect(counter).to.equal(3); // All previous operations completed
    });
  });

  it('should demonstrate microtask priority', () => {
    const order = [];

    setTimeout(() => order.push('timeout'), 0);

    Promise.resolve()
      .then(() => order.push('promise-1'))
      .then(() => order.push('promise-2'));

    queueMicrotask(() => order.push('microtask'));

    cy.wrap(null).then(() => {
      // All microtasks run before macrotasks
      expect(order[0]).to.equal('promise-1');
      expect(order[1]).to.equal('promise-2');
      expect(order[2]).to.equal('microtask');
      expect(order[3]).to.equal('timeout');
    });
  });
});

// Visualizing the event loop
describe('Event Loop Visualization', () => {
  it('should track execution phases', () => {
    const phases = {
      sync: [],
      micro: [],
      macro: []
    };

    // Synchronous
    phases.sync.push('start');

    // Macrotask
    setTimeout(() => {
      phases.macro.push('timeout-1');

      // Microtask inside macrotask
      Promise.resolve().then(() => {
        phases.micro.push('promise-in-timeout');
      });
    }, 0);

    // Microtask
    Promise.resolve().then(() => {
      phases.micro.push('promise-1');

      // Nested microtask
      Promise.resolve().then(() => {
        phases.micro.push('promise-2');
      });
    });

    // Another macrotask
    setTimeout(() => {
      phases.macro.push('timeout-2');
    }, 0);

    phases.sync.push('end');

    cy.wrap(null).then(() => {
      cy.log('Sync:', phases.sync.join(', '));
      cy.log('Micro:', phases.micro.join(', '));
      cy.log('Macro:', phases.macro.join(', '));

      expect(phases.sync).to.deep.equal(['start', 'end']);
      expect(phases.micro.length).to.be.greaterThan(0);
      expect(phases.macro.length).to.be.greaterThan(0);
    });
  });
});
```

**Common Patterns:**

```javascript
// 1. Debounce (uses event loop timing)
function debounce(fn, delay) {
  let timeoutId;

  return function(...args) {
    clearTimeout(timeoutId);

    timeoutId = setTimeout(() => {
      fn.apply(this, args);
    }, delay);
  };
}

// 2. Throttle
function throttle(fn, limit) {
  let inThrottle;

  return function(...args) {
    if (!inThrottle) {
      fn.apply(this, args);
      inThrottle = true;

      setTimeout(() => {
        inThrottle = false;
      }, limit);
    }
  };
}

// 3. Request Animation Frame (rendering task)
function animate() {
  // Update animation
  requestAnimationFrame(animate);
}
```

**Common Mistakes to Avoid:**

1. Assuming setTimeout(fn, 0) executes immediately
2. Not understanding microtask priority over macrotasks
3. Creating infinite loops with microtasks
4. Blocking the event loop with heavy synchronous code

**Interview Tips:**

- Event loop is crucial for understanding async JavaScript
- Microtasks always run before macrotasks
- Cypress commands have their own queue system
- Know the difference between micro and macro tasks

**Tricky Follow-up Questions:**

**Q: What's the output and why?**

```javascript
console.log('1');

setTimeout(() => console.log('2'), 0);

Promise.resolve().then(() => console.log('3'));

Promise.resolve().then(() => {
  console.log('4');
  setTimeout(() => console.log('5'), 0);
});

console.log('6');

// Output: 1, 6, 3, 4, 2, 5
// 1. Sync: 1, 6
// 2. Microtasks: 3, 4 (and queues setTimeout for 5)
// 3. Macrotasks: 2, 5
```

**Q: What happens if you create an infinite microtask queue?**

```javascript
// BAD: Infinite microtasks (blocks rendering)
function infiniteMicrotasks() {
  Promise.resolve().then(infiniteMicrotasks);
}

infiniteMicrotasks(); // Browser freezes!

// Macrotasks allow rendering between executions
function infiniteMacrotasks() {
  setTimeout(infiniteMacrotasks, 0);
}

infiniteMacrotasks(); // Browser can still render
```

---

### Q17: Explain Promises. How do you use them in test automation?

**Answer:**

A Promise represents the eventual completion (or failure) of an asynchronous operation and its resulting value.

**Promise States:**

1. **Pending**: Initial state, neither fulfilled nor rejected
2. **Fulfilled**: Operation completed successfully
3. **Rejected**: Operation failed

**Creating Promises:**

```javascript
// Basic promise
const promise = new Promise((resolve, reject) => {
  const success = true;

  if (success) {
    resolve('Operation successful');
  } else {
    reject('Operation failed');
  }
});

// Using the promise
promise
  .then((result) => {
    console.log(result); // 'Operation successful'
  })
  .catch((error) => {
    console.error(error);
  })
  .finally(() => {
    console.log('Cleanup');
  });
```

**Promise Methods:**

```javascript
// 1. Promise.resolve() - Create fulfilled promise
const resolved = Promise.resolve('value');
resolved.then(value => console.log(value)); // 'value'

// 2. Promise.reject() - Create rejected promise
const rejected = Promise.reject('error');
rejected.catch(error => console.error(error)); // 'error'

// 3. Promise.all() - Wait for all promises
Promise.all([
  Promise.resolve(1),
  Promise.resolve(2),
  Promise.resolve(3)
]).then(([a, b, c]) => {
  console.log(a, b, c); // 1 2 3
});

// Fails if any promise rejects
Promise.all([
  Promise.resolve(1),
  Promise.reject('error'),
  Promise.resolve(3)
]).catch(error => console.error(error)); // 'error'

// 4. Promise.allSettled() - Wait for all, regardless of outcome
Promise.allSettled([
  Promise.resolve(1),
  Promise.reject('error'),
  Promise.resolve(3)
]).then(results => {
  console.log(results);
  // [
  //   { status: 'fulfilled', value: 1 },
  //   { status: 'rejected', reason: 'error' },
  //   { status: 'fulfilled', value: 3 }
  // ]
});

// 5. Promise.race() - First settled promise wins
Promise.race([
  new Promise(resolve => setTimeout(() => resolve('slow'), 100)),
  new Promise(resolve => setTimeout(() => resolve('fast'), 50))
]).then(result => console.log(result)); // 'fast'

// 6. Promise.any() - First fulfilled promise wins
Promise.any([
  Promise.reject('error1'),
  Promise.resolve('success'),
  Promise.reject('error2')
]).then(result => console.log(result)); // 'success'
```

**Promise Chaining:**

```javascript
// Sequential execution
Promise.resolve(1)
  .then(value => {
    console.log(value); // 1
    return value + 1;
  })
  .then(value => {
    console.log(value); // 2
    return value + 1;
  })
  .then(value => {
    console.log(value); // 3
  });

// Returning promises in then()
fetch('/api/user/1')
  .then(response => response.json())
  .then(user => {
    return fetch(`/api/posts?userId=${user.id}`);
  })
  .then(response => response.json())
  .then(posts => {
    console.log(posts);
  })
  .catch(error => {
    console.error('Error:', error);
  });
```

**Error Handling:**

```javascript
// 1. catch() catches all previous errors
Promise.reject('error1')
  .then(() => console.log('Won\'t execute'))
  .catch(error => console.error(error)) // Catches error1
  .then(() => console.log('Executes after catch'));

// 2. Multiple catch blocks
Promise.reject('error1')
  .catch(error => {
    console.error('First catch:', error);
    throw new Error('error2');
  })
  .catch(error => {
    console.error('Second catch:', error); // Catches error2
  });

// 3. finally() always executes
Promise.resolve('success')
  .then(result => console.log(result))
  .catch(error => console.error(error))
  .finally(() => console.log('Cleanup'));
```

**Cypress Testing Scenario:**

```javascript
describe('Promises in Test Automation', () => {
  // 1. Basic promise usage
  it('should handle promises in tests', () => {
    const fetchUser = (id) => {
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          if (id > 0) {
            resolve({ id, name: `User ${id}` });
          } else {
            reject('Invalid ID');
          }
        }, 100);
      });
    };

    return fetchUser(1).then((user) => {
      expect(user.id).to.equal(1);
      expect(user.name).to.equal('User 1');
    });
  });

  // 2. Promise chaining with API calls
  it('should chain promises for related requests', () => {
    return cy.request('/api/users/1')
      .then((response) => {
        const userId = response.body.id;
        return cy.request(`/api/users/${userId}/posts`);
      })
      .then((response) => {
        expect(response.body).to.be.an('array');
        return cy.request(`/api/posts/${response.body[0].id}`);
      })
      .then((response) => {
        expect(response.body).to.have.property('title');
      });
  });

  // 3. Promise.all for parallel requests
  it('should make parallel requests with Promise.all', () => {
    const requests = [
      cy.request('/api/users/1').then(r => r.body),
      cy.request('/api/users/2').then(r => r.body),
      cy.request('/api/users/3').then(r => r.body)
    ];

    return Promise.all(requests).then(([user1, user2, user3]) => {
      expect(user1.id).to.equal(1);
      expect(user2.id).to.equal(2);
      expect(user3.id).to.equal(3);
    });
  });

  // 4. Promise.allSettled for handling partial failures
  it('should handle mixed success/failure with allSettled', () => {
    const requests = [
      cy.request({ url: '/api/users/1', failOnStatusCode: false }),
      cy.request({ url: '/api/users/999', failOnStatusCode: false }),
      cy.request({ url: '/api/users/2', failOnStatusCode: false })
    ];

    return Promise.allSettled(requests).then((results) => {
      results.forEach((result, index) => {
        if (result.status === 'fulfilled') {
          cy.log(`Request ${index} succeeded`);
        } else {
          cy.log(`Request ${index} failed: ${result.reason}`);
        }
      });

      const successful = results.filter(r => r.status === 'fulfilled');
      expect(successful.length).to.be.greaterThan(0);
    });
  });

  // 5. Promise.race for timeout scenarios
  it('should use Promise.race for timeouts', () => {
    const apiCall = cy.request('/api/users/1').then(r => r.body);

    const timeout = new Promise((_, reject) => {
      setTimeout(() => reject('Timeout'), 5000);
    });

    return Promise.race([apiCall, timeout])
      .then((result) => {
        expect(result).to.have.property('id');
      })
      .catch((error) => {
        if (error === 'Timeout') {
          throw new Error('API call timed out');
        }
        throw error;
      });
  });

  // 6. Error handling in promises
  it('should handle promise errors gracefully', () => {
    return cy.request({
      url: '/api/invalid-endpoint',
      failOnStatusCode: false
    })
      .then((response) => {
        if (response.status !== 200) {
          throw new Error(`Request failed with status ${response.status}`);
        }
        return response.body;
      })
      .catch((error) => {
        cy.log(`Caught error: ${error.message}`);
        expect(error.message).to.include('failed');
      });
  });

  // 7. Converting callbacks to promises
  it('should convert callback-based APIs to promises', () => {
    const delayCallback = (ms, callback) => {
      setTimeout(() => callback(null, 'done'), ms);
    };

    const delayPromise = (ms) => {
      return new Promise((resolve, reject) => {
        delayCallback(ms, (error, result) => {
          if (error) reject(error);
          else resolve(result);
        });
      });
    };

    return delayPromise(100).then((result) => {
      expect(result).to.equal('done');
    });
  });

  // 8. Promise retry mechanism
  it('should implement retry with promises', () => {
    let attempts = 0;

    const unreliableRequest = () => {
      attempts++;
      return new Promise((resolve, reject) => {
        if (attempts < 3) {
          reject(`Attempt ${attempts} failed`);
        } else {
          resolve(`Success on attempt ${attempts}`);
        }
      });
    };

    const retry = (fn, maxAttempts) => {
      return fn().catch((error) => {
        if (attempts < maxAttempts) {
          cy.log(`Retrying... (${attempts}/${maxAttempts})`);
          return retry(fn, maxAttempts);
        }
        throw error;
      });
    };

    return retry(unreliableRequest, 3).then((result) => {
      expect(result).to.include('Success');
      expect(attempts).to.equal(3);
    });
  });

  // 9. Promise chaining with transformations
  it('should transform data through promise chain', () => {
    return cy.request('/api/users')
      .then(response => response.body)
      .then(users => users.filter(u => u.isActive))
      .then(activeUsers => activeUsers.map(u => u.id))
      .then(ids => {
        expect(ids).to.be.an('array');
        expect(ids.every(id => typeof id === 'number')).to.be.true;
      });
  });

  // 10. Combining Cypress and vanilla promises
  it('should combine Cypress commands with promises', () => {
    const customAsyncOperation = () => {
      return new Promise((resolve) => {
        setTimeout(() => {
          resolve({ processed: true, timestamp: Date.now() });
        }, 500);
      });
    };

    cy.visit('/dashboard');

    cy.wrap(null).then(() => {
      return customAsyncOperation();
    }).then((result) => {
      expect(result.processed).to.be.true;
      cy.log(`Processed at: ${result.timestamp}`);
    });
  });
});

// Helper functions using promises
const PromiseHelpers = {
  // Delay/sleep function
  delay: (ms) => new Promise(resolve => setTimeout(resolve, ms)),

  // Retry with exponential backoff
  retryWithBackoff: async (fn, maxAttempts = 3, baseDelay = 1000) => {
    for (let attempt = 1; attempt <= maxAttempts; attempt++) {
      try {
        return await fn();
      } catch (error) {
        if (attempt === maxAttempts) throw error;

        const delay = baseDelay * Math.pow(2, attempt - 1);
        await PromiseHelpers.delay(delay);
      }
    }
  },

  // Timeout wrapper
  withTimeout: (promise, ms) => {
    const timeout = new Promise((_, reject) =>
      setTimeout(() => reject(new Error('Timeout')), ms)
    );
    return Promise.race([promise, timeout]);
  },

  // Sequential execution
  sequential: async (promises) => {
    const results = [];
    for (const promise of promises) {
      results.push(await promise);
    }
    return results;
  }
};

describe('Promise Helper Functions', () => {
  it('should use promise helpers', () => {
    // Delay
    const start = Date.now();
    return PromiseHelpers.delay(100).then(() => {
      const elapsed = Date.now() - start;
      expect(elapsed).to.be.at.least(100);
    });
  });

  it('should use timeout wrapper', () => {
    const slowPromise = new Promise(resolve =>
      setTimeout(() => resolve('done'), 2000)
    );

    return PromiseHelpers.withTimeout(slowPromise, 1000)
      .catch((error) => {
        expect(error.message).to.equal('Timeout');
      });
  });
});
```

**Common Mistakes to Avoid:**

1. Not returning promises from test functions
2. Mixing callbacks and promises incorrectly
3. Not handling promise rejections (unhandled promise rejection)
4. Creating promise chains that are too complex

**Interview Tips:**

- Promises are the foundation of async/await
- Always return promises in Cypress `.then()` callbacks
- Know the difference between Promise.all, allSettled, race, any
- Understand promise chaining and error propagation

**Tricky Follow-up Questions:**

**Q: What's the difference between these?**

```javascript
// 1. Not returning promise
promise.then(result => {
  anotherPromise(); // Not waited for!
});

// 2. Returning promise
promise.then(result => {
  return anotherPromise(); // Waited for
});

// 3. Async/await (covered in Q18)
async function example() {
  const result = await promise;
  await anotherPromise();
}
```

**Q: How do you handle errors in promise chains?**

```javascript
// Error bubbles down to first catch
Promise.resolve()
  .then(() => {
    throw new Error('Error in first then');
  })
  .then(() => {
    console.log('Skipped');
  })
  .catch(error => {
    console.error('Caught:', error.message);
    return 'recovery'; // Recover from error
  })
  .then(result => {
    console.log(result); // 'recovery'
  });
```

---


### Q18: Explain async/await. How does it improve upon Promises?

**Answer:**

Async/await is syntactic sugar over Promises that makes asynchronous code look and behave more like synchronous code.

**Basic Syntax:**

```javascript
// Promise version
function fetchUserData() {
  return fetch('/api/user')
    .then(response => response.json())
    .then(user => {
      return fetch(`/api/posts?userId=${user.id}`);
    })
    .then(response => response.json())
    .then(posts => {
      console.log(posts);
    })
    .catch(error => {
      console.error(error);
    });
}

// Async/await version
async function fetchUserDataAsync() {
  try {
    const response = await fetch('/api/user');
    const user = await response.json();

    const postsResponse = await fetch(`/api/posts?userId=${user.id}`);
    const posts = await postsResponse.json();

    console.log(posts);
  } catch (error) {
    console.error(error);
  }
}
```

**Key Points:**

1. `async` functions always return a Promise
2. `await` pauses execution until Promise resolves
3. Can only use `await` inside `async` functions
4. Error handling with try/catch

**Async Function Characteristics:**

```javascript
// Returns a Promise
async function example() {
  return 'value';
}

example().then(value => console.log(value)); // 'value'

// Equivalent to:
function examplePromise() {
  return Promise.resolve('value');
}

// Async with explicit Promise
async function asyncPromise() {
  return Promise.resolve('value');
}

// Throwing errors
async function asyncError() {
  throw new Error('Something went wrong');
}

asyncError().catch(error => console.error(error));
```

**Multiple Awaits:**

```javascript
// Sequential (one after another)
async function sequential() {
  const user = await fetchUser(); // Waits
  const posts = await fetchPosts(); // Waits for user first
  const comments = await fetchComments(); // Waits for posts
  return { user, posts, comments };
}

// Parallel (all at once)
async function parallel() {
  const [user, posts, comments] = await Promise.all([
    fetchUser(),
    fetchPosts(),
    fetchComments()
  ]);
  return { user, posts, comments };
}

// Parallel with individual variables
async function parallelIndividual() {
  const userPromise = fetchUser();
  const postsPromise = fetchPosts();
  const commentsPromise = fetchComments();

  const user = await userPromise;
  const posts = await postsPromise;
  const comments = await commentsPromise;

  return { user, posts, comments };
}
```

**Cypress Testing Scenario:**

```javascript
describe('Async/Await in Cypress', () => {
  // Note: Cypress commands are already async and use their own queue
  // But you can use async/await with custom functions

  it('should use async/await with custom functions', async () => {
    const fetchData = async () => {
      const response = await fetch('https://jsonplaceholder.typicode.com/users/1');
      return await response.json();
    };

    const data = await fetchData();
    expect(data.id).to.equal(1);
  });

  it('should combine Cypress commands with async/await', () => {
    cy.wrap(null).then(async () => {
      // Async function inside Cypress .then()
      const wait = (ms) => new Promise(resolve => setTimeout(resolve, ms));

      await wait(100);
      cy.log('Waited 100ms');

      const result = await Promise.resolve('test data');
      expect(result).to.equal('test data');
    });
  });

  it('should handle sequential async operations', () => {
    const processUser = async (userId) => {
      const userResponse = await cy.request(`/api/users/${userId}`).then(r => r.body);
      const postsResponse = await cy.request(`/api/users/${userId}/posts`).then(r => r.body);

      return {
        user: userResponse,
        postCount: postsResponse.length
      };
    };

    cy.wrap(null).then(async () => {
      const result = await processUser(1);
      expect(result.user.id).to.equal(1);
      expect(result.postCount).to.be.a('number');
    });
  });

  it('should handle parallel async operations', () => {
    const fetchMultipleUsers = async (ids) => {
      const promises = ids.map(id =>
        cy.request(`/api/users/${id}`).then(r => r.body)
      );

      return await Promise.all(promises);
    };

    cy.wrap(null).then(async () => {
      const users = await fetchMultipleUsers([1, 2, 3]);
      expect(users).to.have.length(3);
      expect(users[0].id).to.equal(1);
      expect(users[1].id).to.equal(2);
      expect(users[2].id).to.equal(3);
    });
  });

  it('should handle errors with try/catch', () => {
    const riskyOperation = async () => {
      try {
        const response = await cy.request({
          url: '/api/invalid',
          failOnStatusCode: false
        }).then(r => r);

        if (response.status !== 200) {
          throw new Error(`HTTP ${response.status}`);
        }

        return response.body;
      } catch (error) {
        cy.log(`Error caught: ${error.message}`);
        return { error: error.message };
      }
    };

    cy.wrap(null).then(async () => {
      const result = await riskyOperation();
      expect(result).to.have.property('error');
    });
  });

  it('should implement retry logic with async/await', () => {
    let attempts = 0;

    const retryableOperation = async () => {
      attempts++;
      if (attempts < 3) {
        throw new Error(`Attempt ${attempts} failed`);
      }
      return 'success';
    };

    const retry = async (fn, maxRetries = 3) => {
      for (let i = 0; i < maxRetries; i++) {
        try {
          return await fn();
        } catch (error) {
          if (i === maxRetries - 1) throw error;
          cy.log(`Retry ${i + 1}/${maxRetries}`);
          await new Promise(resolve => setTimeout(resolve, 100));
        }
      }
    };

    cy.wrap(null).then(async () => {
      const result = await retry(retryableOperation);
      expect(result).to.equal('success');
      expect(attempts).to.equal(3);
    });
  });

  it('should use async/await with loops', () => {
    const processItems = async (items) => {
      const results = [];

      // Sequential processing
      for (const item of items) {
        const result = await Promise.resolve(item * 2);
        results.push(result);
      }

      return results;
    };

    cy.wrap(null).then(async () => {
      const results = await processItems([1, 2, 3, 4, 5]);
      expect(results).to.deep.equal([2, 4, 6, 8, 10]);
    });
  });

  it('should handle async/await with conditional logic', () => {
    const conditionalFetch = async (shouldFetch) => {
      if (shouldFetch) {
        const response = await cy.request('/api/users/1').then(r => r.body);
        return response;
      }

      return { id: 0, name: 'Default User' };
    };

    cy.wrap(null).then(async () => {
      const user1 = await conditionalFetch(true);
      expect(user1.id).to.equal(1);

      const user2 = await conditionalFetch(false);
      expect(user2.id).to.equal(0);
    });
  });

  it('should use async/await for data transformation pipeline', () => {
    const pipeline = async (data) => {
      // Step 1
      const filtered = await Promise.resolve(
        data.filter(x => x > 5)
      );

      // Step 2
      const doubled = await Promise.resolve(
        filtered.map(x => x * 2)
      );

      // Step 3
      const sum = await Promise.resolve(
        doubled.reduce((acc, x) => acc + x, 0)
      );

      return sum;
    };

    cy.wrap(null).then(async () => {
      const result = await pipeline([1, 3, 5, 7, 9, 11]);
      // Filtered: [7, 9, 11]
      // Doubled: [14, 18, 22]
      // Sum: 54
      expect(result).to.equal(54);
    });
  });

  it('should handle async IIFE', () => {
    cy.wrap(null).then(async () => {
      // Async IIFE
      const result = await (async () => {
        const data = await Promise.resolve('test');
        const processed = data.toUpperCase();
        return processed;
      })();

      expect(result).to.equal('TEST');
    });
  });
});

// Helper functions with async/await
const AsyncHelpers = {
  // Async sleep
  sleep: (ms) => new Promise(resolve => setTimeout(resolve, ms)),

  // Retry with exponential backoff
  retryWithBackoff: async (fn, maxAttempts = 3) => {
    for (let attempt = 1; attempt <= maxAttempts; attempt++) {
      try {
        return await fn();
      } catch (error) {
        if (attempt === maxAttempts) throw error;
        await AsyncHelpers.sleep(Math.pow(2, attempt) * 1000);
      }
    }
  },

  // Timeout for async operations
  withTimeout: async (promise, ms, timeoutMessage = 'Timeout') => {
    let timeoutHandle;

    const timeoutPromise = new Promise((_, reject) => {
      timeoutHandle = setTimeout(() => reject(new Error(timeoutMessage)), ms);
    });

    try {
      const result = await Promise.race([promise, timeoutPromise]);
      clearTimeout(timeoutHandle);
      return result;
    } catch (error) {
      clearTimeout(timeoutHandle);
      throw error;
    }
  },

  // Batch processing
  processBatch: async (items, batchSize, processFn) => {
    const results = [];

    for (let i = 0; i < items.length; i += batchSize) {
      const batch = items.slice(i, i + batchSize);
      const batchResults = await Promise.all(batch.map(processFn));
      results.push(...batchResults);
    }

    return results;
  },

  // Sequential async map
  asyncMap: async (array, asyncFn) => {
    const results = [];
    for (const item of array) {
      results.push(await asyncFn(item));
    }
    return results;
  },

  // Parallel async filter
  asyncFilter: async (array, asyncPredicate) => {
    const results = await Promise.all(
      array.map(async (item) => ({
        item,
        pass: await asyncPredicate(item)
      }))
    );
    return results.filter(({ pass }) => pass).map(({ item }) => item);
  }
};

describe('Async Helper Functions', () => {
  it('should use async helpers', () => {
    cy.wrap(null).then(async () => {
      // Sleep
      const start = Date.now();
      await AsyncHelpers.sleep(100);
      const elapsed = Date.now() - start;
      expect(elapsed).to.be.at.least(100);

      // Timeout
      const slowPromise = new Promise(resolve =>
        setTimeout(() => resolve('done'), 2000)
      );

      try {
        await AsyncHelpers.withTimeout(slowPromise, 500);
      } catch (error) {
        expect(error.message).to.equal('Timeout');
      }

      // Async map
      const numbers = [1, 2, 3];
      const doubled = await AsyncHelpers.asyncMap(numbers, async (n) => {
        await AsyncHelpers.sleep(10);
        return n * 2;
      });
      expect(doubled).to.deep.equal([2, 4, 6]);
    });
  });
});
```

**Async/Await Best Practices:**

```javascript
// 1. Always use try/catch for error handling
async function example1() {
  try {
    const result = await riskyOperation();
    return result;
  } catch (error) {
    console.error('Error:', error);
    throw error; // Re-throw if needed
  }
}

// 2. Avoid unnecessary awaits
// BAD: Unnecessary await
async function bad() {
  return await Promise.resolve('value');
}

// GOOD: Return promise directly
async function good() {
  return Promise.resolve('value');
}

// 3. Parallelize independent operations
// BAD: Sequential when not needed
async function sequential() {
  const user = await fetchUser();
  const posts = await fetchPosts(); // Doesn't depend on user
  return { user, posts };
}

// GOOD: Parallel
async function parallel() {
  const [user, posts] = await Promise.all([
    fetchUser(),
    fetchPosts()
  ]);
  return { user, posts };
}

// 4. Use async iterators for streams
async function* asyncGenerator() {
  yield await Promise.resolve(1);
  yield await Promise.resolve(2);
  yield await Promise.resolve(3);
}

(async () => {
  for await (const value of asyncGenerator()) {
    console.log(value); // 1, 2, 3
  }
})();
```

**Common Mistakes to Avoid:**

1. Forgetting `async` keyword when using `await`
2. Not handling errors with try/catch
3. Sequential awaits when operations could run in parallel
4. Returning `await` when not necessary

**Interview Tips:**

- Async/await is syntactic sugar for Promises
- Makes async code easier to read and maintain
- Always handle errors with try/catch
- Know when to use sequential vs parallel execution

**Tricky Follow-up Questions:**

**Q: What's wrong with this code?**

```javascript
// PROBLEM: forEach doesn't wait for async operations
async function processItems(items) {
  items.forEach(async (item) => {
    await processItem(item);
  });
  console.log('Done'); // Logs immediately!
}

// SOLUTION 1: Use for...of
async function processItems(items) {
  for (const item of items) {
    await processItem(item);
  }
  console.log('Done'); // Logs after all items processed
}

// SOLUTION 2: Use Promise.all for parallel
async function processItems(items) {
  await Promise.all(items.map(item => processItem(item)));
  console.log('Done');
}
```

**Q: How do you handle async/await in top-level code?**

```javascript
// BAD: Can't use await at top level (in older JavaScript)
// const data = await fetchData(); // SyntaxError

// SOLUTION 1: IIFE
(async () => {
  const data = await fetchData();
  console.log(data);
})();

// SOLUTION 2: Top-level await (ES2022+, modules only)
// In a module file:
const data = await fetchData();
console.log(data);
```

---

### Q19: Explain callbacks vs Promises vs async/await. When to use each?

**Answer:**

Three patterns for handling asynchronous operations, each improving upon the previous.

**Comparison Table:**

| Feature | Callbacks | Promises | Async/Await |
|---------|-----------|----------|-------------|
| Syntax | Nested functions | .then() chains | Looks synchronous |
| Error Handling | Error-first callback | .catch() | try/catch |
| Readability | Poor (callback hell) | Better | Best |
| Composition | Difficult | Easier | Easiest |
| Debugging | Hard | Better | Best |
| Browser Support | All | ES6+ | ES8+ |

**1. Callbacks:**

```javascript
// Basic callback
function fetchUser(id, callback) {
  setTimeout(() => {
    callback(null, { id, name: 'John' });
  }, 100);
}

fetchUser(1, (error, user) => {
  if (error) {
    console.error(error);
  } else {
    console.log(user);
  }
});

// Callback Hell
fetchUser(1, (error1, user) => {
  if (error1) return console.error(error1);

  fetchPosts(user.id, (error2, posts) => {
    if (error2) return console.error(error2);

    fetchComments(posts[0].id, (error3, comments) => {
      if (error3) return console.error(error3);

      console.log(comments);
    });
  });
});
```

**2. Promises:**

```javascript
// Promise version
function fetchUser(id) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve({ id, name: 'John' });
    }, 100);
  });
}

// Better than callbacks
fetchUser(1)
  .then(user => fetchPosts(user.id))
  .then(posts => fetchComments(posts[0].id))
  .then(comments => console.log(comments))
  .catch(error => console.error(error));

// Even better with async/await below...
```

**3. Async/Await:**

```javascript
// Same logic, cleanest syntax
async function getUserComments() {
  try {
    const user = await fetchUser(1);
    const posts = await fetchPosts(user.id);
    const comments = await fetchComments(posts[0].id);
    console.log(comments);
  } catch (error) {
    console.error(error);
  }
}
```

**When to Use Each:**

```javascript
// 1. Use Callbacks when:
// - Working with older APIs (fs, http in Node.js)
// - Event handlers
// - One-time callbacks

button.addEventListener('click', (event) => {
  console.log('Clicked!');
});

// 2. Use Promises when:
// - You need to return async value
// - You want Promise.all, Promise.race, etc.
// - Chaining async operations

Promise.all([
  fetch('/api/users'),
  fetch('/api/posts'),
  fetch('/api/comments')
]).then(([users, posts, comments]) => {
  console.log('All loaded');
});

// 3. Use Async/Await when:
// - Writing new code
// - You want clean, readable syntax
// - Complex control flow (loops, conditionals)

async function processUsers() {
  const users = await fetchUsers();

  for (const user of users) {
    if (user.isActive) {
      await processUser(user);
    }
  }
}
```

**Cypress Testing Scenario:**

```javascript
describe('Async Patterns Comparison', () => {
  // 1. Callback style (older Cypress)
  it('should use callbacks (old style)', (done) => {
    setTimeout(() => {
      expect(true).to.be.true;
      done(); // Signal test completion
    }, 100);
  });

  // 2. Promise style (modern Cypress)
  it('should use promises', () => {
    return new Promise((resolve) => {
      setTimeout(() => {
        expect(true).to.be.true;
        resolve();
      }, 100);
    });
  });

  // 3. Async/await style (modern Cypress)
  it('should use async/await', () => {
    cy.wrap(null).then(async () => {
      await new Promise(resolve => setTimeout(resolve, 100));
      expect(true).to.be.true;
    });
  });

  // Comparing all three for API calls
  it('should compare patterns for API calls', () => {
    // Callback version
    const fetchWithCallback = (url, callback) => {
      cy.request(url).then((response) => {
        callback(null, response.body);
      });
    };

    // Promise version
    const fetchWithPromise = (url) => {
      return cy.request(url).then(r => r.body);
    };

    // Async/await version
    const fetchWithAsync = async (url) => {
      const response = await cy.request(url).then(r => r);
      return response.body;
    };

    // Using them
    cy.wrap(null).then(async () => {
      // Promise version is cleanest for single call
      const user1 = await fetchWithPromise('/api/users/1');
      expect(user1.id).to.equal(1);

      // Async/await best for multiple related calls
      const user2 = await fetchWithAsync('/api/users/1');
      const posts = await fetchWithAsync(`/api/users/${user2.id}/posts`);
      expect(posts).to.be.an('array');
    });
  });

  // Practical example: Sequential vs Parallel
  it('should show sequential vs parallel patterns', () => {
    cy.wrap(null).then(async () => {
      // Sequential with async/await
      const start1 = Date.now();
      const user1 = await cy.request('/api/users/1').then(r => r.body);
      const user2 = await cy.request('/api/users/2').then(r => r.body);
      const user3 = await cy.request('/api/users/3').then(r => r.body);
      const time1 = Date.now() - start1;
      cy.log(`Sequential: ${time1}ms`);

      // Parallel with Promise.all
      const start2 = Date.now();
      const [user4, user5, user6] = await Promise.all([
        cy.request('/api/users/1').then(r => r.body),
        cy.request('/api/users/2').then(r => r.body),
        cy.request('/api/users/3').then(r => r.body)
      ]);
      const time2 = Date.now() - start2;
      cy.log(`Parallel: ${time2}ms`);

      // Parallel should be faster
      expect(time2).to.be.lessThan(time1);
    });
  });

  // Converting between patterns
  it('should convert between async patterns', () => {
    // Callback to Promise
    const callbackFn = (callback) => {
      setTimeout(() => callback(null, 'result'), 100);
    };

    const promiseFn = () => {
      return new Promise((resolve, reject) => {
        callbackFn((error, result) => {
          if (error) reject(error);
          else resolve(result);
        });
      });
    };

    // Promise to async/await
    const asyncFn = async () => {
      return await promiseFn();
    };

    cy.wrap(null).then(async () => {
      const result = await asyncFn();
      expect(result).to.equal('result');
    });
  });
});

// Real-world examples
describe('Real-World Async Patterns', () => {
  // 1. Login flow - Sequential async/await
  it('should handle login flow with async/await', () => {
    cy.wrap(null).then(async () => {
      // Get login page
      cy.visit('/login');

      // Get CSRF token (async)
      const token = await cy.get('meta[name="csrf-token"]')
        .invoke('attr', 'content')
        .then(val => val);

      // Login with token
      const response = await cy.request({
        method: 'POST',
        url: '/api/login',
        headers: { 'X-CSRF-Token': token },
        body: { username: 'test', password: 'test123' }
      }).then(r => r);

      expect(response.status).to.equal(200);

      // Verify logged in
      cy.url().should('include', '/dashboard');
    });
  });

  // 2. Bulk operations - Parallel promises
  it('should handle bulk operations in parallel', () => {
    const users = [
      { name: 'User1', email: 'user1@example.com' },
      { name: 'User2', email: 'user2@example.com' },
      { name: 'User3', email: 'user3@example.com' }
    ];

    cy.wrap(null).then(async () => {
      // Create all users in parallel
      const createdUsers = await Promise.all(
        users.map(user =>
          cy.request('POST', '/api/users', user).then(r => r.body)
        )
      );

      expect(createdUsers).to.have.length(3);
      expect(createdUsers.every(u => u.id)).to.be.true;

      // Cleanup in parallel
      await Promise.all(
        createdUsers.map(user =>
          cy.request('DELETE', `/api/users/${user.id}`)
        )
      );
    });
  });

  // 3. Polling - Callbacks and promises
  it('should implement polling with promises', () => {
    const poll = async (fn, interval = 1000, maxAttempts = 5) => {
      for (let attempt = 1; attempt <= maxAttempts; attempt++) {
        try {
          const result = await fn();
          if (result) return result;
        } catch (error) {
          if (attempt === maxAttempts) throw error;
        }
        await new Promise(resolve => setTimeout(resolve, interval));
      }
      throw new Error('Polling timeout');
    };

    cy.wrap(null).then(async () => {
      const checkStatus = async () => {
        const response = await cy.request('/api/status').then(r => r.body);
        return response.ready === true ? response : null;
      };

      const result = await poll(checkStatus, 500, 10);
      expect(result.ready).to.be.true;
    });
  });
});
```

**Migration Guide:**

```javascript
// From callbacks to Promises
function oldStyleCallback(callback) {
  setTimeout(() => callback(null, 'data'), 100);
}

function newStylePromise() {
  return new Promise((resolve, reject) => {
    oldStyleCallback((error, data) => {
      if (error) reject(error);
      else resolve(data);
    });
  });
}

// From Promises to async/await
// Old
function fetchDataPromise() {
  return fetch('/api/data')
    .then(response => response.json())
    .then(data => processData(data))
    .then(result => saveResult(result))
    .catch(error => handleError(error));
}

// New
async function fetchDataAsync() {
  try {
    const response = await fetch('/api/data');
    const data = await response.json();
    const processed = await processData(data);
    const result = await saveResult(processed);
    return result;
  } catch (error) {
    handleError(error);
  }
}
```

**Common Mistakes to Avoid:**

1. Mixing patterns inconsistently
2. Not understanding when each pattern is appropriate
3. Callback hell (use Promises or async/await)
4. Not handling errors properly in each pattern

**Interview Tips:**

- Async/await is generally preferred for new code
- Promises are good for parallel operations (Promise.all)
- Callbacks still used in event handlers and some APIs
- Know how to convert between patterns

**Tricky Follow-up Questions:**

**Q: How do you handle multiple async operations efficiently?**

```javascript
// WRONG: Sequential when not needed
async function inefficient() {
  const user1 = await fetchUser(1); // Waits
  const user2 = await fetchUser(2); // Waits for user1
  const user3 = await fetchUser(3); // Waits for user2
  return [user1, user2, user3];
}

// RIGHT: Parallel
async function efficient() {
  const [user1, user2, user3] = await Promise.all([
    fetchUser(1),
    fetchUser(2),
    fetchUser(3)
  ]);
  return [user1, user2, user3];
}
```

**Q: What if you need sequential AND parallel?**

```javascript
async function complex() {
  // First get user (must be first)
  const user = await fetchUser(1);

  // Then fetch posts and profile in parallel
  const [posts, profile] = await Promise.all([
    fetchPosts(user.id),
    fetchProfile(user.id)
  ]);

  // Then process results sequentially
  const processedPosts = await processPosts(posts);
  const processedProfile = await processProfile(profile);

  return { user, posts: processedPosts, profile: processedProfile };
}
```

---

### Q20: What is the difference between setTimeout, setInterval, and requestAnimationFrame?

**Answer:**

JavaScript provides three main timing functions, each suited for different use cases.

**1. setTimeout:**

Executes code once after a specified delay.

```javascript
// Basic usage
setTimeout(() => {
  console.log('Executed after 1 second');
}, 1000);

// With arguments
setTimeout((name, age) => {
  console.log(`Hello ${name}, you are ${age}`);
}, 1000, 'John', 30);

// Clearing timeout
const timeoutId = setTimeout(() => {
  console.log('This won\'t execute');
}, 1000);

clearTimeout(timeoutId);

// Recursive setTimeout for repeating
function repeat() {
  console.log('Repeating...');
  setTimeout(repeat, 1000);
}

repeat();
```

**2. setInterval:**

Executes code repeatedly at fixed intervals.

```javascript
// Basic usage
const intervalId = setInterval(() => {
  console.log('Executes every second');
}, 1000);

// Stop after some time
setTimeout(() => {
  clearInterval(intervalId);
  console.log('Interval stopped');
}, 5000);

// Counter example
let count = 0;
const counter = setInterval(() => {
  count++;
  console.log(count);

  if (count >= 5) {
    clearInterval(counter);
  }
}, 1000);
```

**3. requestAnimationFrame:**

Executes code before the next repaint (60fps typically).

```javascript
// Animation loop
function animate() {
  // Update animation
  console.log('Frame updated');

  requestAnimationFrame(animate);
}

animate();

// With cancellation
let frameId;

function animateWithStop() {
  console.log('Animating...');

  frameId = requestAnimationFrame(animateWithStop);
}

animateWithStop();

// Stop after some time
setTimeout(() => {
  cancelAnimationFrame(frameId);
}, 5000);

// Smooth animation example
let position = 0;

function smoothAnimation() {
  position += 1;

  element.style.left = position + 'px';

  if (position < 500) {
    requestAnimationFrame(smoothAnimation);
  }
}

requestAnimationFrame(smoothAnimation);
```

**Comparison Table:**

| Feature | setTimeout | setInterval | requestAnimationFrame |
|---------|-----------|-------------|----------------------|
| Execution | Once | Repeatedly | Before each repaint |
| Timing | Specified delay | Fixed interval | ~60fps (~16.67ms) |
| Precision | Not guaranteed | Not guaranteed | Synced with display |
| Use Case | Delayed actions | Polling, timers | Animations |
| Pauses | No | No | Yes (when tab inactive) |
| Performance | Moderate | Can drift | Optimized |

**Cypress Testing Scenario:**

```javascript
describe('Timing Functions in Tests', () => {
  // 1. setTimeout for delayed actions
  it('should use setTimeout for delays', () => {
    let executed = false;

    setTimeout(() => {
      executed = true;
    }, 100);

    cy.wrap(null).then(() => {
      return new Promise(resolve => {
        setTimeout(() => {
          expect(executed).to.be.true;
          resolve();
        }, 150);
      });
    });
  });

  // 2. setInterval for polling
  it('should use setInterval for polling', () => {
    let count = 0;
    let intervalId;

    const startPolling = () => {
      return new Promise((resolve) => {
        intervalId = setInterval(() => {
          count++;
          cy.log(`Poll count: ${count}`);

          if (count >= 5) {
            clearInterval(intervalId);
            resolve(count);
          }
        }, 100);
      });
    };

    cy.wrap(null).then(() => {
      return startPolling();
    }).then((finalCount) => {
      expect(finalCount).to.equal(5);
    });
  });

  // 3. Testing timeout behavior
  it('should handle timeout cancellation', () => {
    let executed = false;

    const timeoutId = setTimeout(() => {
      executed = true;
    }, 100);

    clearTimeout(timeoutId);

    cy.wait(150).then(() => {
      expect(executed).to.be.false;
    });
  });

  // 4. Waiting for async operations
  it('should wait for async operations', () => {
    const asyncOperation = () => {
      return new Promise((resolve) => {
        setTimeout(() => {
          resolve('Operation complete');
        }, 500);
      });
    };

    cy.wrap(null).then(() => {
      return asyncOperation();
    }).then((result) => {
      expect(result).to.equal('Operation complete');
    });
  });

  // 5. Polling until condition is met
  it('should poll until condition is met', () => {
    let attempts = 0;
    let ready = false;

    // Simulate something becoming ready
    setTimeout(() => {
      ready = true;
    }, 1000);

    const pollUntilReady = () => {
      return new Promise((resolve, reject) => {
        const intervalId = setInterval(() => {
          attempts++;

          if (ready) {
            clearInterval(intervalId);
            resolve(attempts);
          }

          if (attempts >= 20) {
            clearInterval(intervalId);
            reject(new Error('Polling timeout'));
          }
        }, 100);
      });
    };

    cy.wrap(null).then(() => {
      return pollUntilReady();
    }).then((totalAttempts) => {
      expect(ready).to.be.true;
      expect(totalAttempts).to.be.greaterThan(0);
      cy.log(`Took ${totalAttempts} attempts`);
    });
  });

  // 6. Debounce implementation
  it('should implement debounce with setTimeout', () => {
    let callCount = 0;

    const debounce = (fn, delay) => {
      let timeoutId;

      return function(...args) {
        clearTimeout(timeoutId);
        timeoutId = setTimeout(() => {
          fn.apply(this, args);
        }, delay);
      };
    };

    const debouncedFn = debounce(() => {
      callCount++;
    }, 100);

    // Call multiple times rapidly
    debouncedFn();
    debouncedFn();
    debouncedFn();
    debouncedFn();

    // Should only execute once
    cy.wait(150).then(() => {
      expect(callCount).to.equal(1);
    });
  });

  // 7. Throttle implementation
  it('should implement throttle with setTimeout', () => {
    let callCount = 0;

    const throttle = (fn, limit) => {
      let inThrottle;

      return function(...args) {
        if (!inThrottle) {
          fn.apply(this, args);
          inThrottle = true;

          setTimeout(() => {
            inThrottle = false;
          }, limit);
        }
      };
    };

    const throttledFn = throttle(() => {
      callCount++;
    }, 100);

    // Call multiple times
    cy.wrap(null).then(() => {
      return new Promise(resolve => {
        throttledFn(); // Executes
        setTimeout(() => throttledFn(), 50); // Blocked
        setTimeout(() => throttledFn(), 120); // Executes
        setTimeout(() => throttledFn(), 150); // Blocked
        setTimeout(() => {
          resolve();
        }, 300);
      });
    }).then(() => {
      expect(callCount).to.equal(2);
    });
  });

  // 8. setTimeout vs setInterval precision
  it('should compare setTimeout and setInterval precision', () => {
    const results = {
      setTimeout: [],
      setInterval: []
    };

    const testSetTimeout = () => {
      return new Promise(resolve => {
        let count = 0;
        const start = Date.now();

        function repeat() {
          count++;
          results.setTimeout.push(Date.now() - start);

          if (count < 5) {
            setTimeout(repeat, 100);
          } else {
            resolve();
          }
        }

        repeat();
      });
    };

    const testSetInterval = () => {
      return new Promise(resolve => {
        let count = 0;
        const start = Date.now();

        const intervalId = setInterval(() => {
          count++;
          results.setInterval.push(Date.now() - start);

          if (count >= 5) {
            clearInterval(intervalId);
            resolve();
          }
        }, 100);
      });
    };

    cy.wrap(null).then(() => {
      return testSetTimeout();
    }).then(() => {
      return testSetInterval();
    }).then(() => {
      cy.log('setTimeout timings:', results.setTimeout);
      cy.log('setInterval timings:', results.setInterval);

      // Both should execute roughly 5 times
      expect(results.setTimeout).to.have.length(5);
      expect(results.setInterval).to.have.length(5);
    });
  });
});

// Utility functions
const TimingUtils = {
  // Promisified setTimeout
  sleep: (ms) => new Promise(resolve => setTimeout(resolve, ms)),

  // Retry with delay
  retryWithDelay: async (fn, maxAttempts = 3, delay = 1000) => {
    for (let attempt = 1; attempt <= maxAttempts; attempt++) {
      try {
        return await fn();
      } catch (error) {
        if (attempt === maxAttempts) throw error;
        await TimingUtils.sleep(delay);
      }
    }
  },

  // Poll until condition
  pollUntil: (conditionFn, interval = 100, timeout = 5000) => {
    return new Promise((resolve, reject) => {
      const startTime = Date.now();

      const intervalId = setInterval(() => {
        if (conditionFn()) {
          clearInterval(intervalId);
          resolve();
        } else if (Date.now() - startTime >= timeout) {
          clearInterval(intervalId);
          reject(new Error('Poll timeout'));
        }
      }, interval);
    });
  },

  // Debounce
  debounce: (fn, delay) => {
    let timeoutId;

    return function(...args) {
      clearTimeout(timeoutId);
      timeoutId = setTimeout(() => fn.apply(this, args), delay);
    };
  },

  // Throttle
  throttle: (fn, limit) => {
    let inThrottle;

    return function(...args) {
      if (!inThrottle) {
        fn.apply(this, args);
        inThrottle = true;
        setTimeout(() => { inThrottle = false; }, limit);
      }
    };
  }
};

describe('Timing Utilities', () => {
  it('should use timing utilities', () => {
    cy.wrap(null).then(async () => {
      // Sleep
      const start = Date.now();
      await TimingUtils.sleep(200);
      const elapsed = Date.now() - start;
      expect(elapsed).to.be.at.least(200);

      // Poll until
      let ready = false;
      setTimeout(() => { ready = true; }, 500);

      await TimingUtils.pollUntil(() => ready, 100, 1000);
      expect(ready).to.be.true;
    });
  });
});
```

**When to Use Each:**

```javascript
// Use setTimeout for:
// - One-time delayed execution
// - Debouncing
// - Yielding control back to browser
setTimeout(() => {
  console.log('Delayed action');
}, 1000);

// Use setInterval for:
// - Repeating actions at fixed intervals
// - Polling
// - Countdown timers
const countdownId = setInterval(() => {
  console.log(`Time: ${--seconds}`);
  if (seconds <= 0) clearInterval(countdownId);
}, 1000);

// Use requestAnimationFrame for:
// - Smooth animations
// - Visual updates
// - Any rendering-related updates
function animate() {
  updatePosition();
  render();
  requestAnimationFrame(animate);
}
```

**Common Mistakes to Avoid:**

1. Using setInterval without clearing it (memory leak)
2. Assuming precise timing (not guaranteed)
3. Blocking event loop with heavy synchronous code
4. Not understanding setInterval drift over time

**Interview Tips:**

- setTimeout/setInterval timing is not guaranteed (event loop dependency)
- requestAnimationFrame is better for animations (60fps, synced with display)
- Always clear intervals and timeouts when done
- Know about debounce and throttle patterns

**Tricky Follow-up Questions:**

**Q: Why does setInterval drift over time?**

```javascript
// setInterval tries to maintain exact intervals
// but execution time affects next interval
let count = 0;
const start = Date.now();

const intervalId = setInterval(() => {
  // If this takes 50ms to execute...
  heavyOperation(); // 50ms

  count++;
  const elapsed = Date.now() - start;
  console.log(`Interval ${count}: ${elapsed}ms`);

  // Expected: 100, 200, 300, 400...
  // Actual: 100, 200, 300, 400... but drifts over time

  if (count >= 10) clearInterval(intervalId);
}, 100);

// Solution: Use recursive setTimeout
function accurateInterval() {
  const start = Date.now();

  function repeat() {
    heavyOperation();

    const elapsed = Date.now() - start;
    const next = 100 - (elapsed % 100);

    setTimeout(repeat, next);
  }

  repeat();
}
```

**Q: What's the minimum delay for setTimeout?**

```javascript
// Even setTimeout(fn, 0) isn't immediate
console.log('1');

setTimeout(() => {
  console.log('3'); // Executes after current call stack
}, 0);

console.log('2');

// Output: 1, 2, 3

// Browser has minimum delay (usually 4ms)
// Nested timeouts have 4ms minimum after 5th nesting
```

---


## Section 5: ES6+ Features

### Q21: Explain template literals and tagged templates. How are they useful in tests?

**Answer:**

Template literals provide a more powerful way to work with strings in JavaScript, introduced in ES6.

**Basic Template Literals:**

```javascript
// Traditional string concatenation
const name = 'John';
const age = 30;
const message = 'Hello, my name is ' + name + ' and I am ' + age + ' years old.';

// Template literal (backticks)
const message2 = `Hello, my name is ${name} and I am ${age} years old.`;

// Multi-line strings
const html = `
  <div>
    <h1>Hello</h1>
    <p>This is a multi-line string</p>
  </div>
`;

// Expressions in templates
const price = 10;
const tax = 2;
const total = `Total: $${price + tax}`;

// Function calls
const greeting = `Hello ${getName().toUpperCase()}!`;

// Nested templates
const user = { name: 'John', age: 30 };
const info = `User: ${user.name}, Status: ${user.age >= 18 ? 'Adult' : 'Minor'}`;
```

**Tagged Templates:**

```javascript
// Tag function receives strings and values
function myTag(strings, ...values) {
  console.log('Strings:', strings); // Array of string parts
  console.log('Values:', values);   // Array of expressions

  return strings.reduce((result, str, i) => {
    return result + str + (values[i] || '');
  }, '');
}

const name = 'John';
const age = 30;
const result = myTag`Hello ${name}, you are ${age} years old`;

// Strings: ['Hello ', ', you are ', ' years old']
// Values: ['John', 30]
```

**Practical Tagged Templates:**

```javascript
// 1. SQL query builder
function sql(strings, ...values) {
  // Escape values to prevent SQL injection
  const escaped = values.map(value => {
    if (typeof value === 'string') {
      return `'${value.replace(/'/g, "''")}'`;
    }
    return value;
  });

  return strings.reduce((query, str, i) => {
    return query + str + (escaped[i] || '');
  }, '');
}

const userId = 5;
const query = sql`SELECT * FROM users WHERE id = ${userId}`;

// 2. HTML escaping
function html(strings, ...values) {
  const escape = (str) => {
    return String(str)
      .replace(/&/g, '&amp;')
      .replace(/</g, '&lt;')
      .replace(/>/g, '&gt;')
      .replace(/"/g, '&quot;')
      .replace(/'/g, '&#39;');
  };

  return strings.reduce((result, str, i) => {
    return result + str + (values[i] ? escape(values[i]) : '');
  }, '');
}

const userInput = '<script>alert("XSS")</script>';
const safe = html`<div>${userInput}</div>`;
// Result: <div>&lt;script&gt;alert(&quot;XSS&quot;)&lt;/script&gt;</div>

// 3. Styled components (React)
const Button = styled.button`
  background: ${props => props.primary ? 'blue' : 'gray'};
  color: white;
  padding: 10px 20px;
`;
```

**Cypress Testing Scenario:**

```javascript
describe('Template Literals in Tests', () => {
  // 1. Dynamic selectors
  it('should use template literals for selectors', () => {
    const fieldName = 'username';
    const selector = `[data-testid="${fieldName}"]`;

    cy.get(selector).type('testuser');

    // Multi-level selectors
    const section = 'profile';
    const field = 'email';
    const complexSelector = `[data-section="${section}"] [data-field="${field}"]`;

    cy.get(complexSelector).should('be.visible');
  });

  // 2. Dynamic test data
  it('should generate test data with templates', () => {
    const timestamp = Date.now();
    const testUser = {
      username: `user_${timestamp}`,
      email: `user_${timestamp}@example.com`,
      password: 'Test123!'
    };

    cy.visit('/register');
    cy.get('[name="username"]').type(testUser.username);
    cy.get('[name="email"]').type(testUser.email);
    cy.get('[name="password"]').type(testUser.password);
  });

  // 3. Multi-line test messages
  it('should use multi-line strings for logging', () => {
    const results = {
      passed: 5,
      failed: 2,
      skipped: 1
    };

    const summary = `
      Test Results:
      - Passed: ${results.passed}
      - Failed: ${results.failed}
      - Skipped: ${results.skipped}
      - Total: ${results.passed + results.failed + results.skipped}
    `;

    cy.log(summary);
  });

  // 4. Dynamic URLs
  it('should build URLs with template literals', () => {
    const userId = 123;
    const endpoint = 'posts';
    const params = new URLSearchParams({ limit: 10, offset: 0 });

    const url = `/api/users/${userId}/${endpoint}?${params}`;

    cy.request(url).then((response) => {
      expect(response.status).to.equal(200);
    });
  });

  // 5. Custom tagged template for test data
  function testData(strings, ...values) {
    const timestamp = Date.now();

    return strings.reduce((result, str, i) => {
      let value = values[i];

      // Replace placeholders
      if (value === 'unique_id') {
        value = timestamp;
      } else if (value === 'random_email') {
        value = `test_${timestamp}@example.com`;
      }

      return result + str + (value || '');
    }, '');
  }

  it('should use tagged templates for test data', () => {
    const username = testData`user_${'unique_id'}`;
    const email = testData`${'random_email'}`;

    cy.log(`Username: ${username}`);
    cy.log(`Email: ${email}`);

    expect(username).to.include('user_');
    expect(email).to.include('@example.com');
  });

  // 6. Tagged template for CSS selectors
  function selector(strings, ...values) {
    return strings.reduce((result, str, i) => {
      let value = values[i];

      // Escape special characters
      if (typeof value === 'string') {
        value = value.replace(/"/g, '\\"');
      }

      return result + str + (value || '');
    }, '');
  }

  it('should use tagged templates for safe selectors', () => {
    const testId = 'user-form';
    const sel = selector`[data-testid="${testId}"]`;

    cy.get(sel).should('exist');
  });

  // 7. API request builder
  function apiRequest(strings, ...values) {
    const baseUrl = 'https://api.example.com';

    const path = strings.reduce((result, str, i) => {
      return result + str + (values[i] || '');
    }, '');

    return `${baseUrl}${path}`;
  }

  it('should build API requests with tagged templates', () => {
    const userId = 1;
    const resourceType = 'posts';

    const url = apiRequest`/users/${userId}/${resourceType}`;

    expect(url).to.equal('https://api.example.com/users/1/posts');
  });

  // 8. Parameterized test names
  it('should use template literals in test names', () => {
    const testCases = [
      { input: 'test@example.com', valid: true },
      { input: 'invalid-email', valid: false },
      { input: 'another@test.com', valid: true }
    ];

    testCases.forEach(({ input, valid }) => {
      cy.log(`Testing ${input} - Expected: ${valid ? 'valid' : 'invalid'}`);

      const isValid = input.includes('@');
      expect(isValid).to.equal(valid);
    });
  });

  // 9. Complex string formatting
  it('should format complex strings', () => {
    const user = {
      firstName: 'John',
      lastName: 'Doe',
      age: 30,
      roles: ['admin', 'user']
    };

    const formatted = `
      Full Name: ${user.firstName} ${user.lastName}
      Age: ${user.age}
      Roles: ${user.roles.join(', ')}
      Is Adult: ${user.age >= 18 ? 'Yes' : 'No'}
    `.trim();

    cy.log(formatted);
  });

  // 10. Conditional string building
  it('should conditionally build strings', () => {
    const config = {
      protocol: 'https',
      host: 'example.com',
      port: 8080,
      path: '/api/users',
      secure: true
    };

    const url = `
      ${config.protocol}://${config.host}${config.port ? `:${config.port}` : ''}${config.path}${config.secure ? '?secure=true' : ''}
    `.replace(/\s/g, '');

    expect(url).to.equal('https://example.com:8080/api/users?secure=true');
  });
});

// Utility tagged templates
const TestTemplates = {
  // SQL-like query builder
  query(strings, ...values) {
    return strings.reduce((result, str, i) => {
      const value = values[i];
      const escaped = typeof value === 'string' ? 
        `"${value.replace(/"/g, '\\"')}"` : value;

      return result + str + (escaped !== undefined ? escaped : '');
    }, '');
  },

  // HTML template
  html(strings, ...values) {
    const escape = (str) => {
      return String(str)
        .replace(/&/g, '&amp;')
        .replace(/</g, '&lt;')
        .replace(/>/g, '&gt;')
        .replace(/"/g, '&quot;');
    };

    return strings.reduce((result, str, i) => {
      const value = values[i];
      return result + str + (value !== undefined ? escape(value) : '');
    }, '');
  },

  // Log formatter
  log(strings, ...values) {
    const timestamp = new Date().toISOString();

    return `[${timestamp}] ` + strings.reduce((result, str, i) => {
      return result + str + (values[i] !== undefined ? values[i] : '');
    }, '');
  }
};

describe('Tagged Template Utilities', () => {
  it('should use custom tagged templates', () => {
    // Query builder
    const userId = 5;
    const name = "John's User";
    const query = TestTemplates.query`SELECT * FROM users WHERE id = ${userId} AND name = ${name}`;

    cy.log(query);
    expect(query).to.include('John\\'s User');

    // HTML escape
    const userInput = '<script>alert("XSS")</script>';
    const safeHtml = TestTemplates.html`<div>${userInput}</div>`;

    expect(safeHtml).to.include('&lt;script&gt;');

    // Log formatter
    const message = TestTemplates.log`Test ${'passed'} with ${5} assertions`;
    cy.log(message);
  });
});
```

**Common Mistakes to Avoid:**

1. Forgetting to use backticks (using quotes instead)
2. Not escaping user input in tagged templates
3. Over-complicating with tagged templates when simple strings suffice
4. Not understanding how tagged template parameters work

**Interview Tips:**

- Template literals use backticks, not quotes
- `${}` can contain any JavaScript expression
- Tagged templates are powerful for DSLs (Domain-Specific Languages)
- Very useful for dynamic test data and selectors

**Tricky Follow-up Questions:**

**Q: What are the parameters of a tag function?**

```javascript
function tag(strings, ...values) {
  // strings: array of literal parts
  // values: array of expression values

  console.log(strings.raw); // Raw strings (with escapes)
  console.log(strings[0]); // First literal part

  return 'result';
}

const name = 'John';
const result = tag`Hello ${name}\nWorld`;

// strings: ['Hello ', '\nWorld']
// strings.raw: ['Hello ', '\\nWorld']
// values: ['John']
```

**Q: How do you access raw strings?**

```javascript
// Normal template literal processes escapes
const normal = `Line 1\nLine 2`;
console.log(normal); // Line 1
                     // Line 2

// String.raw preserves escapes
const raw = String.raw`Line 1\nLine 2`;
console.log(raw); // Line 1\nLine 2

// In tag function
function myTag(strings, ...values) {
  console.log(strings.raw[0]); // Access raw strings
  return String.raw({ raw: strings }, ...values);
}
```

---

### Q22: Explain ES6 classes. How do you use them in Page Object Model?

**Answer:**

ES6 classes provide a cleaner syntax for creating objects and implementing inheritance, built on top of JavaScript's prototypal inheritance.

**Basic Class Syntax:**

```javascript
// Class declaration
class Person {
  // Constructor
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  // Method
  greet() {
    return `Hello, I'm ${this.name}`;
  }

  // Getter
  get info() {
    return `${this.name}, ${this.age} years old`;
  }

  // Setter
  set age(value) {
    if (value < 0) {
      throw new Error('Age cannot be negative');
    }
    this._age = value;
  }

  // Static method
  static create(name, age) {
    return new Person(name, age);
  }

  // Static property
  static species = 'Homo sapiens';
}

// Creating instance
const john = new Person('John', 30);
console.log(john.greet()); // "Hello, I'm John"

// Using static method
const jane = Person.create('Jane', 25);

// Class expression
const Animal = class {
  constructor(name) {
    this.name = name;
  }
};
```

**Inheritance:**

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    return `${this.name} makes a sound`;
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name); // Call parent constructor
    this.breed = breed;
  }

  speak() {
    return `${this.name} barks`;
  }

  // New method
  fetch() {
    return `${this.name} fetches the ball`;
  }
}

const buddy = new Dog('Buddy', 'Labrador');
console.log(buddy.speak()); // "Buddy barks"
console.log(buddy instanceof Dog); // true
console.log(buddy instanceof Animal); // true
```

**Private Fields (ES2022):**

```javascript
class BankAccount {
  #balance = 0; // Private field

  constructor(initialBalance) {
    this.#balance = initialBalance;
  }

  deposit(amount) {
    this.#balance += amount;
  }

  getBalance() {
    return this.#balance;
  }

  // Private method
  #validate(amount) {
    return amount > 0;
  }
}

const account = new BankAccount(100);
account.deposit(50);
console.log(account.getBalance()); // 150
// console.log(account.#balance); // SyntaxError: Private field
```

**Cypress Testing Scenario - Page Object Model:**

```javascript
// Base Page class
class BasePage {
  constructor() {
    this.timeout = 10000;
  }

  visit(path) {
    cy.visit(path);
    return this;
  }

  waitForElement(selector) {
    cy.get(selector, { timeout: this.timeout }).should('be.visible');
    return this;
  }

  clickElement(selector) {
    cy.get(selector).click();
    return this;
  }

  typeText(selector, text) {
    cy.get(selector).clear().type(text);
    return this;
  }

  verifyUrl(expectedUrl) {
    cy.url().should('include', expectedUrl);
    return this;
  }

  verifyText(selector, expectedText) {
    cy.get(selector).should('contain', expectedText);
    return this;
  }
}

// Login Page class
class LoginPage extends BasePage {
  constructor() {
    super();

    // Selectors
    this.selectors = {
      emailInput: '[data-testid="email"]',
      passwordInput: '[data-testid="password"]',
      submitButton: '[data-testid="submit"]',
      errorMessage: '.error-message',
      forgotPasswordLink: '[data-testid="forgot-password"]'
    };

    this.url = '/login';
  }

  open() {
    return this.visit(this.url);
  }

  enterEmail(email) {
    return this.typeText(this.selectors.emailInput, email);
  }

  enterPassword(password) {
    return this.typeText(this.selectors.passwordInput, password);
  }

  submit() {
    return this.clickElement(this.selectors.submitButton);
  }

  login(email, password) {
    this.enterEmail(email);
    this.enterPassword(password);
    return this.submit();
  }

  verifyError(message) {
    return this.verifyText(this.selectors.errorMessage, message);
  }

  clickForgotPassword() {
    return this.clickElement(this.selectors.forgotPasswordLink);
  }
}

// Dashboard Page class
class DashboardPage extends BasePage {
  constructor() {
    super();

    this.selectors = {
      welcomeMessage: '[data-testid="welcome"]',
      logoutButton: '[data-testid="logout"]',
      profileMenu: '[data-testid="profile-menu"]',
      notificationBell: '[data-testid="notifications"]',
      searchInput: '[data-testid="search"]'
    };

    this.url = '/dashboard';
  }

  verifyWelcomeMessage(username) {
    return this.verifyText(this.selectors.welcomeMessage, username);
  }

  logout() {
    this.clickElement(this.selectors.profileMenu);
    return this.clickElement(this.selectors.logoutButton);
  }

  search(query) {
    return this.typeText(this.selectors.searchInput, query);
  }

  getNotificationCount() {
    return cy.get(this.selectors.notificationBell)
      .invoke('text')
      .then(text => parseInt(text));
  }
}

// User Management Page
class UserManagementPage extends BasePage {
  constructor() {
    super();

    this.selectors = {
      addUserButton: '[data-testid="add-user"]',
      userTable: '[data-testid="user-table"]',
      searchInput: '[data-testid="search-users"]',
      deleteButton: '[data-testid="delete-user"]',
      editButton: '[data-testid="edit-user"]'
    };

    this.url = '/users';
  }

  open() {
    return this.visit(this.url);
  }

  clickAddUser() {
    return this.clickElement(this.selectors.addUserButton);
  }

  searchUser(name) {
    return this.typeText(this.selectors.searchInput, name);
  }

  getUserRow(username) {
    return cy.get(this.selectors.userTable)
      .contains('tr', username);
  }

  editUser(username) {
    this.getUserRow(username)
      .find(this.selectors.editButton)
      .click();
    return this;
  }

  deleteUser(username) {
    this.getUserRow(username)
      .find(this.selectors.deleteButton)
      .click();
    return this;
  }

  verifyUserExists(username) {
    this.getUserRow(username).should('exist');
    return this;
  }

  verifyUserNotExists(username) {
    this.getUserRow(username).should('not.exist');
    return this;
  }
}

// API Client class
class ApiClient {
  constructor(baseUrl = Cypress.config('baseUrl')) {
    this.baseUrl = baseUrl;
    this.defaultHeaders = {
      'Content-Type': 'application/json'
    };
  }

  setAuthToken(token) {
    this.defaultHeaders['Authorization'] = `Bearer ${token}`;
    return this;
  }

  request(method, endpoint, options = {}) {
    return cy.request({
      method,
      url: `${this.baseUrl}${endpoint}`,
      headers: { ...this.defaultHeaders, ...options.headers },
      body: options.body,
      failOnStatusCode: options.failOnStatusCode !== false
    });
  }

  get(endpoint, options) {
    return this.request('GET', endpoint, options);
  }

  post(endpoint, body, options = {}) {
    return this.request('POST', endpoint, { ...options, body });
  }

  put(endpoint, body, options = {}) {
    return this.request('PUT', endpoint, { ...options, body });
  }

  delete(endpoint, options) {
    return this.request('DELETE', endpoint, options);
  }
}

// Test using Page Objects
describe('Page Object Model with Classes', () => {
  let loginPage;
  let dashboardPage;
  let userManagementPage;
  let apiClient;

  beforeEach(() => {
    loginPage = new LoginPage();
    dashboardPage = new DashboardPage();
    userManagementPage = new UserManagementPage();
    apiClient = new ApiClient();
  });

  it('should login successfully', () => {
    loginPage
      .open()
      .login('test@example.com', 'password123');

    dashboardPage
      .verifyUrl('/dashboard')
      .verifyWelcomeMessage('test@example.com');
  });

  it('should show error for invalid credentials', () => {
    loginPage
      .open()
      .login('wrong@example.com', 'wrongpassword')
      .verifyError('Invalid credentials');
  });

  it('should manage users', () => {
    // Login first
    loginPage
      .open()
      .login('admin@example.com', 'admin123');

    // Navigate to user management
    userManagementPage
      .open()
      .clickAddUser();

    // Add user via UI
    cy.get('[name="username"]').type('newuser');
    cy.get('[name="email"]').type('newuser@example.com');
    cy.get('[type="submit"]').click();

    // Verify user exists
    userManagementPage
      .verifyUserExists('newuser');

    // Delete user
    userManagementPage
      .deleteUser('newuser')
      .verifyUserNotExists('newuser');
  });

  it('should use API client', () => {
    // Login via API
    apiClient
      .post('/api/login', {
        email: 'test@example.com',
        password: 'password123'
      })
      .then((response) => {
        expect(response.status).to.equal(200);
        const token = response.body.token;

        // Set auth token
        apiClient.setAuthToken(token);

        // Make authenticated request
        return apiClient.get('/api/users');
      })
      .then((response) => {
        expect(response.status).to.equal(200);
        expect(response.body).to.be.an('array');
      });
  });

  it('should use inheritance features', () => {
    // All page objects inherit from BasePage
    expect(loginPage instanceof BasePage).to.be.true;
    expect(dashboardPage instanceof BasePage).to.be.true;
    expect(userManagementPage instanceof BasePage).to.be.true;

    // Can use base class methods
    loginPage.waitForElement(loginPage.selectors.emailInput);
  });
});

// Advanced: Component class for reusable UI components
class NavigationComponent extends BasePage {
  constructor() {
    super();

    this.selectors = {
      homeLink: '[data-nav="home"]',
      usersLink: '[data-nav="users"]',
      settingsLink: '[data-nav="settings"]',
      profileDropdown: '[data-testid="profile-dropdown"]'
    };
  }

  goToHome() {
    return this.clickElement(this.selectors.homeLink);
  }

  goToUsers() {
    return this.clickElement(this.selectors.usersLink);
  }

  goToSettings() {
    return this.clickElement(this.selectors.settingsLink);
  }

  openProfileMenu() {
    return this.clickElement(this.selectors.profileDropdown);
  }
}

// Page with component
class PageWithNavigation extends BasePage {
  constructor() {
    super();
    this.navigation = new NavigationComponent();
  }

  navigateToUsers() {
    return this.navigation.goToUsers();
  }
}

describe('Components in Page Objects', () => {
  it('should use reusable components', () => {
    const page = new PageWithNavigation();

    page.visit('/dashboard');
    page.navigateToUsers();

    cy.url().should('include', '/users');
  });
});
```

**Static Properties and Methods:**

```javascript
class TestConfig {
  static baseUrl = 'http://localhost:3000';
  static timeout = 10000;
  static retries = 3;

  static getConfig() {
    return {
      baseUrl: this.baseUrl,
      timeout: this.timeout,
      retries: this.retries
    };
  }

  static setBaseUrl(url) {
    this.baseUrl = url;
  }
}

// Usage (no instance needed)
console.log(TestConfig.baseUrl);
TestConfig.setBaseUrl('http://example.com');
```

**Common Mistakes to Avoid:**

1. Forgetting `super()` in child class constructor
2. Trying to access `this` before `super()` in derived class
3. Not returning `this` for method chaining
4. Confusing class declarations with instances

**Interview Tips:**

- Classes are syntactic sugar over prototype-based inheritance
- Use classes for Page Object Model in test automation
- Inheritance with `extends` keyword
- Private fields start with `#` (ES2022+)

**Tricky Follow-up Questions:**

**Q: What's the difference between class and function constructor?**

```javascript
// Function constructor (old way)
function Person(name) {
  this.name = name;
}

Person.prototype.greet = function() {
  return `Hello, ${this.name}`;
};

// Class (new way)
class PersonClass {
  constructor(name) {
    this.name = name;
  }

  greet() {
    return `Hello, ${this.name}`;
  }
}

// Key difference: Classes can't be called without 'new'
Person('John'); // Works (but 'this' is window)
PersonClass('John'); // TypeError: Class constructor cannot be invoked without 'new'
```

**Q: How do you create private methods before ES2022?**

```javascript
// Using closure (before private fields)
class OldPrivate {
  constructor() {
    // Private variable via closure
    let privateVar = 'secret';

    // Public method accessing private
    this.getPrivate = () => privateVar;
  }
}

// Using WeakMap
const privateData = new WeakMap();

class WeakMapPrivate {
  constructor() {
    privateData.set(this, { secret: 'value' });
  }

  getSecret() {
    return privateData.get(this).secret;
  }
}
```

---


### Q23: Explain ES6 modules (import/export). How do you organize test code with modules?

**Answer:**

ES6 modules provide a way to organize code into reusable, maintainable pieces with explicit dependencies.

**Export Syntax:**

```javascript
// Named exports
export const PI = 3.14159;
export function add(a, b) {
  return a + b;
}

export class Calculator {
  // ...
}

// Or export at end
const multiply = (a, b) => a * b;
const divide = (a, b) => a / b;

export { multiply, divide };

// Rename exports
export { multiply as mult, divide as div };

// Default export (one per module)
export default class User {
  constructor(name) {
    this.name = name;
  }
}

// Or
class Product {
  // ...
}

export default Product;
```

**Import Syntax:**

```javascript
// Named imports
import { add, multiply } from './math.js';

// Rename imports
import { add as addition } from './math.js';

// Import all
import * as MathUtils from './math.js';

// Default import
import User from './User.js';

// Mix default and named
import User, { validateEmail, formatName } from './User.js';

// Import for side effects only
import './polyfills.js';

// Dynamic import
const module = await import('./module.js');
```

**Cypress Testing Scenario:**

```javascript
// File: cypress/support/pageObjects/BasePage.js
export class BasePage {
  visit(url) {
    cy.visit(url);
    return this;
  }

  clickElement(selector) {
    cy.get(selector).click();
    return this;
  }

  typeText(selector, text) {
    cy.get(selector).type(text);
    return this;
  }
}

// File: cypress/support/pageObjects/LoginPage.js
import { BasePage } from './BasePage';

export class LoginPage extends BasePage {
  constructor() {
    super();
    this.url = '/login';
    this.selectors = {
      email: '[data-testid="email"]',
      password: '[data-testid="password"]',
      submit: '[data-testid="submit"]'
    };
  }

  open() {
    return this.visit(this.url);
  }

  login(email, password) {
    this.typeText(this.selectors.email, email);
    this.typeText(this.selectors.password, password);
    return this.clickElement(this.selectors.submit);
  }
}

// File: cypress/support/testData/users.js
export const testUsers = {
  admin: {
    email: 'admin@example.com',
    password: 'Admin123!',
    role: 'admin'
  },
  regular: {
    email: 'user@example.com',
    password: 'User123!',
    role: 'user'
  }
};

export function generateUser(role = 'user') {
  const timestamp = Date.now();
  return {
    email: `user${timestamp}@example.com`,
    password: 'Test123!',
    role
  };
}

// File: cypress/support/utils/apiHelpers.js
export class ApiHelper {
  static baseUrl = Cypress.config('baseUrl');

  static request(method, endpoint, options = {}) {
    return cy.request({
      method,
      url: `${this.baseUrl}${endpoint}`,
      ...options
    });
  }

  static get(endpoint) {
    return this.request('GET', endpoint);
  }

  static post(endpoint, body) {
    return this.request('POST', endpoint, { body });
  }

  static put(endpoint, body) {
    return this.request('PUT', endpoint, { body });
  }

  static delete(endpoint) {
    return this.request('DELETE', endpoint);
  }
}

export const authenticate = (email, password) => {
  return ApiHelper.post('/api/login', { email, password })
    .then(response => response.body.token);
};

// File: cypress/support/utils/validators.js
export function validateEmail(email) {
  const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return regex.test(email);
}

export function validatePassword(password) {
  return password.length >= 8 && /[A-Z]/.test(password) && /\d/.test(password);
}

export function validateUrl(url) {
  try {
    new URL(url);
    return true;
  } catch {
    return false;
  }
}

export default {
  validateEmail,
  validatePassword,
  validateUrl
};

// File: cypress/e2e/login.cy.js
import { LoginPage } from '../support/pageObjects/LoginPage';
import { testUsers, generateUser } from '../support/testData/users';
import { validateEmail } from '../support/utils/validators';
import { authenticate } from '../support/utils/apiHelpers';

describe('Login Tests', () => {
  let loginPage;

  beforeEach(() => {
    loginPage = new LoginPage();
  });

  it('should login with valid credentials', () => {
    const user = testUsers.admin;

    expect(validateEmail(user.email)).to.be.true;

    loginPage
      .open()
      .login(user.email, user.password);

    cy.url().should('include', '/dashboard');
  });

  it('should login via API', () => {
    const user = testUsers.regular;

    cy.wrap(null).then(() => {
      return authenticate(user.email, user.password);
    }).then((token) => {
      expect(token).to.be.a('string');
      cy.log(`Token: ${token}`);
    });
  });

  it('should create and login new user', () => {
    const newUser = generateUser('admin');

    expect(validateEmail(newUser.email)).to.be.true;

    // Test with generated user
    cy.log(`Testing with: ${newUser.email}`);
  });
});

// File: cypress/support/commands.js
import { LoginPage } from './pageObjects/LoginPage';
import { ApiHelper } from './utils/apiHelpers';

Cypress.Commands.add('loginViaUI', (email, password) => {
  const loginPage = new LoginPage();
  loginPage.open().login(email, password);
});

Cypress.Commands.add('loginViaAPI', (email, password) => {
  return ApiHelper.post('/api/login', { email, password })
    .then((response) => {
      window.localStorage.setItem('token', response.body.token);
      return response.body.token;
    });
});

// File: cypress/support/fixtures/fixtureHelper.js
export class FixtureHelper {
  static load(fixtureName) {
    return cy.fixture(fixtureName);
  }

  static loadMultiple(fixtureNames) {
    return Promise.all(
      fixtureNames.map(name => cy.fixture(name).then(data => data))
    );
  }

  static async loadAndProcess(fixtureName, processor) {
    const data = await cy.fixture(fixtureName).then(d => d);
    return processor(data);
  }
}

// Usage in test
import { FixtureHelper } from '../support/fixtures/fixtureHelper';

describe('Fixture Tests', () => {
  it('should load fixtures', () => {
    FixtureHelper.load('users.json').then((users) => {
      expect(users).to.be.an('array');
    });
  });

  it('should load multiple fixtures', () => {
    cy.wrap(null).then(() => {
      return FixtureHelper.loadMultiple(['users.json', 'products.json']);
    }).then(([users, products]) => {
      expect(users).to.be.an('array');
      expect(products).to.be.an('array');
    });
  });
});
```

**Organizing Test Code Structure:**

```
cypress/
├── e2e/
│   ├── auth/
│   │   ├── login.cy.js
│   │   └── signup.cy.js
│   ├── users/
│   │   ├── userManagement.cy.js
│   │   └── userProfile.cy.js
│   └── products/
│       └── productCatalog.cy.js
├── support/
│   ├── pageObjects/
│   │   ├── BasePage.js
│   │   ├── LoginPage.js
│   │   ├── DashboardPage.js
│   │   └── index.js (re-export all)
│   ├── api/
│   │   ├── AuthAPI.js
│   │   ├── UserAPI.js
│   │   └── index.js
│   ├── utils/
│   │   ├── validators.js
│   │   ├── helpers.js
│   │   └── index.js
│   ├── testData/
│   │   ├── users.js
│   │   ├── products.js
│   │   └── index.js
│   ├── commands.js
│   └── e2e.js
└── fixtures/
    ├── users.json
    └── products.json
```

**Re-exporting Pattern:**

```javascript
// File: cypress/support/pageObjects/index.js
export { BasePage } from './BasePage';
export { LoginPage } from './LoginPage';
export { DashboardPage } from './DashboardPage';
export { UserManagementPage } from './UserManagementPage';

// Usage in tests
import { LoginPage, DashboardPage } from '../support/pageObjects';

// Or import all
import * as Pages from '../support/pageObjects';

const loginPage = new Pages.LoginPage();
```

**Dynamic Imports:**

```javascript
// Lazy load heavy modules
describe('Heavy Tests', () => {
  it('should dynamically import module', () => {
    cy.wrap(null).then(async () => {
      // Load module only when needed
      const { HeavyProcessor } = await import('../support/utils/heavyProcessor');

      const processor = new HeavyProcessor();
      const result = processor.process(data);

      expect(result).to.exist;
    });
  });

  it('should conditionally import', () => {
    const environment = Cypress.env('ENVIRONMENT');

    cy.wrap(null).then(async () => {
      let config;

      if (environment === 'production') {
        config = await import('../config/prod.config');
      } else {
        config = await import('../config/dev.config');
      }

      cy.log(`Using config: ${config.default.baseUrl}`);
    });
  });
});
```

**Common Mistakes to Avoid:**

1. Circular dependencies between modules
2. Not using consistent import/export patterns
3. Over-modularizing (too many small modules)
4. Forgetting file extensions in import paths

**Interview Tips:**

- Modules provide encapsulation and reusability
- Named exports for utilities, default export for main class
- Use barrel files (index.js) for clean imports
- Dynamic imports for code splitting and lazy loading

**Tricky Follow-up Questions:**

**Q: What's the difference between named and default exports?**

```javascript
// Named exports - can have multiple
export const func1 = () => {};
export const func2 = () => {};

// Import must use exact names
import { func1, func2 } from './module';

// Default export - only one per module
export default class MyClass {}

// Import can use any name
import WhateverName from './module';
import AnotherName from './module'; // Same class, different name
```

**Q: How do you handle circular dependencies?**

```javascript
// BAD: Circular dependency
// fileA.js
import { functionB } from './fileB';
export const functionA = () => functionB();

// fileB.js
import { functionA } from './fileA';
export const functionB = () => functionA(); // Circular!

// SOLUTION 1: Refactor to extract common dependency
// common.js
export const sharedFunction = () => {};

// fileA.js
import { sharedFunction } from './common';
export const functionA = () => sharedFunction();

// fileB.js
import { sharedFunction } from './common';
export const functionB = () => sharedFunction();

// SOLUTION 2: Dynamic import
// fileB.js
export const functionB = async () => {
  const { functionA } = await import('./fileA');
  return functionA();
};
```

---

### Q24: What are symbols and iterators? How are they used?

**Answer:**

Symbols and iterators are advanced ES6 features that enable custom iteration behavior and unique property keys.

**Symbols:**

```javascript
// Create unique symbol
const sym1 = Symbol();
const sym2 = Symbol();
console.log(sym1 === sym2); // false (always unique)

// Symbol with description
const sym3 = Symbol('my symbol');
console.log(sym3.toString()); // "Symbol(my symbol)"

// Symbols as object keys
const obj = {
  [sym1]: 'value1',
  [sym2]: 'value2',
  regularKey: 'value3'
};

console.log(obj[sym1]); // 'value1'

// Symbols are not enumerable
console.log(Object.keys(obj)); // ['regularKey']
console.log(Object.getOwnPropertySymbols(obj)); // [sym1, sym2]

// Global symbol registry
const globalSym1 = Symbol.for('app.id');
const globalSym2 = Symbol.for('app.id');
console.log(globalSym1 === globalSym2); // true

// Get key from global symbol
console.log(Symbol.keyFor(globalSym1)); // 'app.id'

// Well-known symbols
Symbol.iterator
Symbol.toStringTag
Symbol.toPrimitive
// ... and more
```

**Iterators:**

```javascript
// Custom iterator
const myIterator = {
  current: 1,
  last: 5,

  [Symbol.iterator]() {
    return {
      current: this.current,
      last: this.last,

      next() {
        if (this.current <= this.last) {
          return { value: this.current++, done: false };
        } else {
          return { done: true };
        }
      }
    };
  }
};

// Using the iterator
for (const num of myIterator) {
  console.log(num); // 1, 2, 3, 4, 5
}

// Spread operator uses iterator
const arr = [...myIterator]; // [1, 2, 3, 4, 5]

// Manual iteration
const iterator = myIterator[Symbol.iterator]();
console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
```

**Generator Functions:**

```javascript
// Generator function (creates iterators)
function* numberGenerator(start, end) {
  for (let i = start; i <= end; i++) {
    yield i;
  }
}

const gen = numberGenerator(1, 5);

for (const num of gen) {
  console.log(num); // 1, 2, 3, 4, 5
}

// Generator with yield*
function* combinedGenerator() {
  yield* [1, 2, 3];
  yield* [4, 5, 6];
}

console.log([...combinedGenerator()]); // [1, 2, 3, 4, 5, 6]

// Infinite generator
function* infiniteSequence() {
  let i = 0;
  while (true) {
    yield i++;
  }
}

const infinite = infiniteSequence();
console.log(infinite.next().value); // 0
console.log(infinite.next().value); // 1
```

**Cypress Testing Scenario:**

```javascript
describe('Symbols and Iterators in Tests', () => {
  // 1. Using symbols for private data
  it('should use symbols for private properties', () => {
    const _privateData = Symbol('privateData');

    class TestHelper {
      constructor(data) {
        this[_privateData] = data;
      }

      getData() {
        return this[_privateData];
      }

      getPublicKeys() {
        return Object.keys(this);
      }
    }

    const helper = new TestHelper({ secret: 'value' });

    expect(helper.getData()).to.deep.equal({ secret: 'value' });
    expect(helper.getPublicKeys()).to.be.empty; // Symbol not enumerable
  });

  // 2. Custom iterator for test data
  it('should create custom iterator for test cases', () => {
    class TestCaseIterator {
      constructor(testCases) {
        this.testCases = testCases;
      }

      [Symbol.iterator]() {
        let index = 0;
        const cases = this.testCases;

        return {
          next() {
            if (index < cases.length) {
              return { value: cases[index++], done: false };
            }
            return { done: true };
          }
        };
      }
    }

    const testCases = new TestCaseIterator([
      { input: 'test1@example.com', valid: true },
      { input: 'invalid', valid: false },
      { input: 'test2@example.com', valid: true }
    ]);

    let validCount = 0;

    for (const testCase of testCases) {
      if (testCase.valid) {
        validCount++;
      }
    }

    expect(validCount).to.equal(2);
  });

  // 3. Generator for test data
  it('should use generator for test data', () => {
    function* userGenerator(count) {
      for (let i = 1; i <= count; i++) {
        yield {
          id: i,
          username: `user${i}`,
          email: `user${i}@example.com`
        };
      }
    }

    const users = [...userGenerator(3)];

    expect(users).to.have.length(3);
    expect(users[0].username).to.equal('user1');
    expect(users[2].username).to.equal('user3');
  });

  // 4. Infinite generator with limit
  it('should use infinite generator', () => {
    function* uniqueIdGenerator() {
      let id = 1;
      while (true) {
        yield `ID-${id++}`;
      }
    }

    const idGen = uniqueIdGenerator();
    const ids = [];

    for (let i = 0; i < 5; i++) {
      ids.push(idGen.next().value);
    }

    expect(ids).to.deep.equal(['ID-1', 'ID-2', 'ID-3', 'ID-4', 'ID-5']);
  });

  // 5. Symbol for custom behavior
  it('should customize object behavior with symbols', () => {
    class CustomArray {
      constructor(...items) {
        this.items = items;
      }

      [Symbol.iterator]() {
        return this.items[Symbol.iterator]();
      }

      [Symbol.toStringTag]() {
        return 'CustomArray';
      }

      get [Symbol.toStringTag]() {
        return 'CustomArray';
      }
    }

    const arr = new CustomArray(1, 2, 3, 4, 5);

    // Can use for...of
    const doubled = [];
    for (const num of arr) {
      doubled.push(num * 2);
    }

    expect(doubled).to.deep.equal([2, 4, 6, 8, 10]);

    // Custom toString tag
    expect(Object.prototype.toString.call(arr)).to.equal('[object CustomArray]');
  });

  // 6. Generator for async operations
  it('should use generator for sequential async', () => {
    function* asyncTaskRunner() {
      yield cy.request('/api/users/1');
      yield cy.request('/api/users/2');
      yield cy.request('/api/users/3');
    }

    const runner = asyncTaskRunner();
    const results = [];

    runner.next().value.then((r) => {
      results.push(r.body.id);
      return runner.next().value;
    }).then((r) => {
      results.push(r.body.id);
      return runner.next().value;
    }).then((r) => {
      results.push(r.body.id);

      cy.wrap(results).should('deep.equal', [1, 2, 3]);
    });
  });

  // 7. Well-known symbols
  it('should use well-known symbols', () => {
    class Range {
      constructor(start, end) {
        this.start = start;
        this.end = end;
      }

      [Symbol.iterator]() {
        let current = this.start;
        const end = this.end;

        return {
          next() {
            if (current <= end) {
              return { value: current++, done: false };
            }
            return { done: true };
          }
        };
      }

      [Symbol.toPrimitive](hint) {
        if (hint === 'number') {
          return this.end - this.start;
        }
        if (hint === 'string') {
          return `Range(${this.start} to ${this.end})`;
        }
        return true;
      }
    }

    const range = new Range(1, 5);

    // Iterator
    expect([...range]).to.deep.equal([1, 2, 3, 4, 5]);

    // toPrimitive
    expect(Number(range)).to.equal(4);
    expect(String(range)).to.equal('Range(1 to 5)');
  });
});

// Utility generators
const TestGenerators = {
  // Generate sequential IDs
  *idGenerator(prefix = 'ID') {
    let id = 1;
    while (true) {
      yield `${prefix}-${id++}`;
    }
  },

  // Generate test users
  *userGenerator(count, role = 'user') {
    for (let i = 1; i <= count; i++) {
      yield {
        id: i,
        username: `user${i}`,
        email: `user${i}@example.com`,
        role
      };
    }
  },

  // Generate date range
  *dateRange(start, end) {
    const current = new Date(start);
    const endDate = new Date(end);

    while (current <= endDate) {
      yield new Date(current);
      current.setDate(current.getDate() + 1);
    }
  },

  // Generate combinations
  *combinations(arr1, arr2) {
    for (const a of arr1) {
      for (const b of arr2) {
        yield [a, b];
      }
    }
  }
};

describe('Test Generators Utilities', () => {
  it('should use utility generators', () => {
    // ID generator
    const idGen = TestGenerators.idGenerator('USER');
    const ids = [
      idGen.next().value,
      idGen.next().value,
      idGen.next().value
    ];
    expect(ids).to.deep.equal(['USER-1', 'USER-2', 'USER-3']);

    // User generator
    const users = [...TestGenerators.userGenerator(3, 'admin')];
    expect(users).to.have.length(3);
    expect(users[0].role).to.equal('admin');

    // Combinations
    const combos = [...TestGenerators.combinations(['a', 'b'], [1, 2])];
    expect(combos).to.deep.equal([
      ['a', 1], ['a', 2],
      ['b', 1], ['b', 2]
    ]);
  });
});
```

**Common Mistakes to Avoid:**

1. Thinking symbols are equal when they're not
2. Expecting symbols to show up in Object.keys()
3. Not implementing proper iterator protocol
4. Infinite generators without break conditions

**Interview Tips:**

- Symbols create unique, non-enumerable properties
- Iterators enable custom iteration with for...of
- Generators are functions that can pause/resume execution
- Well-known symbols customize object behavior

**Tricky Follow-up Questions:**

**Q: How do you create a lazy infinite sequence?**

```javascript
function* fibonacci() {
  let [a, b] = [0, 1];

  while (true) {
    yield a;
    [a, b] = [b, a + b];
  }
}

const fib = fibonacci();

// Take first 10
const first10 = [];
for (let i = 0; i < 10; i++) {
  first10.push(fib.next().value);
}

console.log(first10); // [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```

**Q: What's the difference between Symbol() and Symbol.for()?**

```javascript
// Symbol() creates unique symbol every time
const s1 = Symbol('test');
const s2 = Symbol('test');
console.log(s1 === s2); // false

// Symbol.for() uses global registry
const g1 = Symbol.for('test');
const g2 = Symbol.for('test');
console.log(g1 === g2); // true

// Useful for cross-realm symbols
const key = Symbol.for('app.config');
// Can access same symbol from different contexts
```

---

### Q25: Explain optional chaining and nullish coalescing. When to use them?

**Answer:**

These ES2020+ features make working with potentially null/undefined values safer and cleaner.

**Optional Chaining (?.):**

```javascript
// Without optional chaining
const user = {
  name: 'John',
  address: {
    city: 'NYC'
  }
};

// Risky - might throw error
const zipCode = user.address.zipCode.toString(); // Error!

// Safe but verbose
const zipCode2 = user && user.address && user.address.zipCode && user.address.zipCode.toString();

// With optional chaining
const zipCode3 = user?.address?.zipCode?.toString();
// Returns undefined if any part is null/undefined

// Optional chaining with arrays
const firstUser = users?.[0]?.name;

// Optional chaining with function calls
const result = obj.method?.();

// Optional chaining with dynamic properties
const value = obj?.[dynamicKey]?.property;
```

**Nullish Coalescing (??):**

```javascript
// Logical OR (||) problem
const count = 0;
const result1 = count || 10; // 10 (0 is falsy)

// Nullish coalescing only checks null/undefined
const result2 = count ?? 10; // 0 (0 is not nullish)

// Other examples
const name = null ?? 'Default'; // 'Default'
const name2 = undefined ?? 'Default'; // 'Default'
const name3 = '' ?? 'Default'; // '' (empty string is not nullish)
const name4 = false ?? true; // false (false is not nullish)
const name5 = 0 ?? 100; // 0 (0 is not nullish)

// Combined with optional chaining
const city = user?.address?.city ?? 'Unknown';
```

**Comparison:**

| Value | `|| 'default'` | `?? 'default'` |
|-------|----------------|----------------|
| null | 'default' | 'default' |
| undefined | 'default' | 'default' |
| false | 'default' | false |
| 0 | 'default' | 0 |
| '' | 'default' | '' |
| NaN | 'default' | NaN |
| 'value' | 'value' | 'value' |

**Cypress Testing Scenario:**

```javascript
describe('Optional Chaining and Nullish Coalescing', () => {
  // 1. Safe API response handling
  it('should safely access API response data', () => {
    cy.request('/api/user/1').then((response) => {
      const user = response.body;

      // Safe nested access
      const street = user?.address?.street ?? 'No address';
      const zipCode = user?.address?.zipCode ?? '00000';
      const phoneNumber = user?.contact?.phone?.number ?? 'No phone';

      cy.log(`Street: ${street}`);
      cy.log(`Zip: ${zipCode}`);
      cy.log(`Phone: ${phoneNumber}`);

      // Safe method calls
      const uppercaseName = user?.name?.toUpperCase() ?? 'UNKNOWN';
      expect(uppercaseName).to.be.a('string');
    });
  });

  // 2. Safe element property access
  it('should safely access element properties', () => {
    cy.get('body').then(($body) => {
      // Safe attribute access
      const dataId = $body.attr('data-id') ?? 'no-id';
      const className = $body.attr('class') ?? '';

      cy.log(`Data ID: ${dataId}`);
      cy.log(`Class: ${className}`);

      // Safe array element access
      const firstElement = $body.find('.item')?.[0]?.textContent ?? 'No items';
      cy.log(`First item: ${firstElement}`);
    });
  });

  // 3. Configuration with defaults
  it('should handle configuration with nullish coalescing', () => {
    const config = {
      timeout: 0, // Valid value!
      retries: null, // Should use default
      baseUrl: '', // Valid value!
      headers: undefined // Should use default
    };

    // WRONG: Using ||
    const wrongTimeout = config.timeout || 5000; // 5000 (wrong!)
    const wrongBaseUrl = config.baseUrl || 'http://localhost'; // 'http://localhost' (wrong!)

    // CORRECT: Using ??
    const timeout = config.timeout ?? 5000; // 0 (correct!)
    const retries = config.retries ?? 3; // 3
    const baseUrl = config.baseUrl ?? 'http://localhost'; // '' (correct!)
    const headers = config.headers ?? {}; // {}

    expect(timeout).to.equal(0);
    expect(retries).to.equal(3);
    expect(baseUrl).to.equal('');
    expect(headers).to.deep.equal({});
  });

  // 4. Safe function calls
  it('should safely call optional methods', () => {
    const obj = {
      doSomething: () => 'result',
      nested: {
        method: () => 'nested result'
      }
    };

    // Safe method calls
    const result1 = obj.doSomething?.(); // 'result'
    const result2 = obj.doNothing?.(); // undefined
    const result3 = obj.nested?.method?.(); // 'nested result'
    const result4 = obj.missing?.nested?.method?.(); // undefined

    expect(result1).to.equal('result');
    expect(result2).to.be.undefined;
    expect(result3).to.equal('nested result');
    expect(result4).to.be.undefined;
  });

  // 5. Array operations with optional chaining
  it('should safely access array elements', () => {
    const users = [
      { id: 1, name: 'John', profile: { age: 30 } },
      { id: 2, name: 'Jane' }, // No profile
      null, // Null element
      { id: 3, name: 'Bob', profile: null }
    ];

    // Safe array access
    const firstUserAge = users[0]?.profile?.age ?? 0; // 30
    const secondUserAge = users[1]?.profile?.age ?? 0; // 0
    const thirdUserAge = users[2]?.profile?.age ?? 0; // 0
    const fourthUserAge = users[3]?.profile?.age ?? 0; // 0

    const ages = users.map(user => user?.profile?.age ?? 0);
    expect(ages).to.deep.equal([30, 0, 0, 0]);
  });

  // 6. Form validation with safe access
  it('should safely validate form data', () => {
    const formData = {
      personal: {
        firstName: 'John',
        lastName: null,
        age: 0 // Valid age!
      },
      contact: {
        email: '',
        phone: undefined
      }
    };

    // Validation with safe access
    const firstName = formData?.personal?.firstName ?? 'Required';
    const lastName = formData?.personal?.lastName ?? 'Required';
    const age = formData?.personal?.age ?? 18; // Should be 0, not 18!
    const email = formData?.contact?.email ?? 'Required';
    const phone = formData?.contact?.phone ?? 'Required';

    expect(firstName).to.equal('John');
    expect(lastName).to.equal('Required');
    expect(age).to.equal(0); // Correct: 0 is not nullish
    expect(email).to.equal(''); // Correct: '' is not nullish
    expect(phone).to.equal('Required');
  });

  // 7. Conditional element interaction
  it('should conditionally interact with elements', () => {
    cy.get('body').then(($body) => {
      const modal = $body.find('.modal')[0];

      // Safe interaction
      modal?.querySelector('.close-button')?.click();

      // Won't throw error if modal or button doesn't exist
      cy.log('Modal interaction attempted');
    });
  });

  // 8. Complex nested data structures
  it('should handle complex nested structures', () => {
    const apiResponse = {
      status: 'success',
      data: {
        users: [
          {
            id: 1,
            profile: {
              personal: {
                name: 'John',
                contact: {
                  emails: ['john@example.com']
                }
              }
            }
          }
        ]
      }
    };

    // Deep optional chaining
    const email = apiResponse?.data?.users?.[0]?.profile?.personal?.contact?.emails?.[0] ?? 'no-email';

    expect(email).to.equal('john@example.com');

    // Non-existent path
    const phone = apiResponse?.data?.users?.[1]?.profile?.personal?.contact?.phone ?? 'no-phone';
    expect(phone).to.equal('no-phone');
  });

  // 9. Default assignment pattern
  it('should use default assignment pattern', () => {
    function processUser(user) {
      const name = user?.name ?? 'Anonymous';
      const age = user?.age ?? 0;
      const verified = user?.verified ?? false;
      const roles = user?.roles ?? [];

      return { name, age, verified, roles };
    }

    const user1 = processUser({ name: 'John', age: 30 });
    const user2 = processUser({ age: 0 }); // 0 is valid!
    const user3 = processUser(null);

    expect(user1.age).to.equal(30);
    expect(user2.age).to.equal(0); // Not replaced with default
    expect(user3.name).to.equal('Anonymous');
  });

  // 10. Combining both operators
  it('should combine optional chaining and nullish coalescing', () => {
    const config = {
      server: {
        host: 'example.com',
        port: 0, // Port 0 is valid!
        ssl: {
          enabled: false // false is valid!
        }
      }
    };

    // Correct usage
    const host = config?.server?.host ?? 'localhost';
    const port = config?.server?.port ?? 8080;
    const sslEnabled = config?.server?.ssl?.enabled ?? true;
    const cert = config?.server?.ssl?.cert ?? 'default.pem';

    expect(host).to.equal('example.com');
    expect(port).to.equal(0); // Preserved
    expect(sslEnabled).to.equal(false); // Preserved
    expect(cert).to.equal('default.pem'); // Default used
  });
});

// Utility functions
const SafeAccess = {
  // Get nested property safely
  get(obj, path, defaultValue = undefined) {
    return path.split('.').reduce((acc, part) => acc?.[part], obj) ?? defaultValue;
  },

  // Set nested property safely
  set(obj, path, value) {
    const parts = path.split('.');
    const last = parts.pop();
    const target = parts.reduce((acc, part) => {
      if (!acc[part]) acc[part] = {};
      return acc[part];
    }, obj);
    target[last] = value;
  },

  // Check if nested property exists
  has(obj, path) {
    return path.split('.').reduce((acc, part) => acc?.[part], obj) !== undefined;
  }
};

describe('Safe Access Utilities', () => {
  it('should use safe access utilities', () => {
    const obj = {
      user: {
        profile: {
          name: 'John',
          age: 0
        }
      }
    };

    // Get
    expect(SafeAccess.get(obj, 'user.profile.name')).to.equal('John');
    expect(SafeAccess.get(obj, 'user.profile.age', 18)).to.equal(0); // 0 preserved
    expect(SafeAccess.get(obj, 'user.address.city', 'NYC')).to.equal('NYC');

    // Has
    expect(SafeAccess.has(obj, 'user.profile.name')).to.be.true;
    expect(SafeAccess.has(obj, 'user.address')).to.be.false;

    // Set
    SafeAccess.set(obj, 'user.address.city', 'NYC');
    expect(obj.user.address.city).to.equal('NYC');
  });
});
```

**Common Mistakes to Avoid:**

1. Using `||` when you want `??` (forgetting about 0, '', false)
2. Chaining too deeply (hard to debug)
3. Using optional chaining where required properties should exist
4. Not understanding that `?.` short-circuits the entire chain

**Interview Tips:**

- `?.` prevents errors from null/undefined access
- `??` only checks null/undefined, not all falsy values
- Very useful for API responses and optional properties
- Understand the difference from `||` operator

**Tricky Follow-up Questions:**

**Q: What's the output of this code?**

```javascript
const obj = {
  a: {
    b: {
      c: 0
    }
  }
};

console.log(obj?.a?.b?.c || 100); // 100 (0 is falsy)
console.log(obj?.a?.b?.c ?? 100); // 0 (0 is not nullish)
console.log(obj?.a?.b?.d || 100); // 100
console.log(obj?.a?.b?.d ?? 100); // 100
```

**Q: How does optional chaining short-circuit?**

```javascript
let callCount = 0;

function sideEffect() {
  callCount++;
  return null;
}

// Short-circuits - sideEffect not called
const result = sideEffect()?.property?.method();

console.log(callCount); // 1 (called once, then short-circuited)

// vs regular chaining
try {
  const result2 = sideEffect().property.method(); // Error!
} catch (e) {
  console.log('Error thrown');
}
```

---


## Section 6: Testing & Cypress

### Q26: Explain Cypress command chaining and asynchronous nature. How does it differ from regular promises?

**Answer:**

Cypress commands are enqueued and run asynchronously, but use a custom command queue rather than standard JavaScript promises.

**Cypress Command Queue:**

```javascript
// Commands are enqueued, not executed immediately
cy.visit('/page');           // Queued
cy.get('.button');           // Queued
cy.click();                  // Queued

console.log('This runs first!'); // Executes immediately

// Commands execute after all sync code completes

describe('Command Queue Demonstration', () => {
  it('demonstrates async queue', () => {
    console.log('1'); // Runs immediately

    cy.visit('/page').then(() => {
      console.log('3'); // Runs after visit
    });

    console.log('2'); // Runs before visit

    // Output: 1, 2, 3
  });
});
```

**Key Differences from Promises:**

```javascript
// Regular Promises
fetch('/api/data')
  .then(response => response.json())
  .then(data => console.log(data));

// Runs immediately when created

// Cypress Commands
cy.request('/api/data')
  .then(response => console.log(response.body));

// Queued and run later
```

**Comparison Table:**

| Feature | Cypress Commands | JavaScript Promises |
|---------|-----------------|-------------------|
| Execution | Enqueued, serial | Immediate |
| Retry Logic | Built-in retry | No retry |
| Assertions | Retry until timeout | One-time check |
| Chain Breaking | Difficult | Easy with async/await |
| Debugging | Time-travel | Standard debugger |
| Error Handling | Automatic screenshot | try/catch |

**Practical Examples:**

```javascript
describe('Cypress Command Chaining', () => {
  // 1. Basic chaining
  it('should chain commands', () => {
    cy.visit('/dashboard')
      .get('[data-testid="username"]')
      .should('be.visible')
      .type('testuser')
      .should('have.value', 'testuser');
  });

  // 2. Yielding subjects
  it('should understand yielding', () => {
    cy.get('.user-item')    // Yields: jQuery element
      .first()              // Yields: first element
      .find('.name')        // Yields: .name element
      .invoke('text')       // Yields: text string
      .should('not.be.empty'); // Asserts on string
  });

  // 3. .then() for custom logic
  it('should use .then() for custom logic', () => {
    cy.get('[data-testid="count"]')
      .invoke('text')
      .then((text) => {
        const count = parseInt(text);

        if (count > 10) {
          cy.log('High count detected');
          cy.get('.warning').should('be.visible');
        } else {
          cy.log('Normal count');
        }
      });
  });

  // 4. Breaking the chain
  it('should break and restart chain', () => {
    cy.get('.first-element').as('firstEl');

    // Do other stuff
    cy.get('.second-element').click();

    // Return to first element
    cy.get('@firstEl').should('be.visible');
  });

  // 5. Mixing sync and async
  it('should mix synchronous and asynchronous', () => {
    let capturedText;

    cy.get('[data-testid="text"]')
      .invoke('text')
      .then((text) => {
        capturedText = text; // Capture in closure
      });

    // Can't use capturedText here - command hasn't run yet!
    // console.log(capturedText); // undefined!

    cy.then(() => {
      // Can use it here - commands have run
      console.log(capturedText); // Has value
    });
  });

  // 6. Returning values from .then()
  it('should return values from .then()', () => {
    cy.get('[data-testid="price"]')
      .invoke('text')
      .then((price) => {
        const numericPrice = parseFloat(price.replace('$', ''));
        return numericPrice; // Return to next command
      })
      .should('be.greaterThan', 0);
  });

  // 7. Returning Cypress commands
  it('should return Cypress commands from .then()', () => {
    cy.get('[data-testid="user-type"]')
      .invoke('text')
      .then((userType) => {
        if (userType === 'admin') {
          return cy.get('[data-testid="admin-panel"]');
        } else {
          return cy.get('[data-testid="user-panel"]');
        }
      })
      .should('be.visible');
  });

  // 8. Using .wrap() to queue values
  it('should use .wrap() for values', () => {
    const user = {
      name: 'John',
      email: 'john@example.com'
    };

    cy.wrap(user)
      .should('have.property', 'name', 'John')
      .and('have.property', 'email')
      .and('include', '@');
  });

  // 9. Combining Cypress and vanilla promises
  it('should combine with vanilla promises', () => {
    cy.wrap(null).then(() => {
      // Inside .then(), can use vanilla promises
      return new Promise((resolve) => {
        setTimeout(() => {
          resolve('Delayed result');
        }, 1000);
      });
    }).then((result) => {
      expect(result).to.equal('Delayed result');
    });
  });

  // 10. Parallel vs sequential execution
  it('should understand parallel vs sequential', () => {
    // Sequential (one after another)
    cy.request('/api/users/1')
      .then(() => cy.request('/api/users/2'))
      .then(() => cy.request('/api/users/3'));

    // Still sequential - Cypress always is!
    cy.request('/api/users/1');
    cy.request('/api/users/2');
    cy.request('/api/users/3');

    // For true parallel, use Promise.all inside .then()
    cy.wrap(null).then(() => {
      return Promise.all([
        cy.request('/api/users/1').then(r => r.body),
        cy.request('/api/users/2').then(r => r.body),
        cy.request('/api/users/3').then(r => r.body)
      ]);
    }).then(([user1, user2, user3]) => {
      cy.log('All users loaded');
    });
  });
});

// Advanced patterns
describe('Advanced Command Patterns', () => {
  // Retry-ability
  it('should demonstrate retry-ability', () => {
    // This retries until assertion passes or timeout
    cy.get('[data-testid="loading"]').should('not.exist');

    // Custom retry logic
    const checkCondition = () => {
      return cy.get('[data-testid="status"]')
        .invoke('text')
        .then((text) => {
          if (text === 'ready') {
            return true;
          }
          throw new Error('Not ready yet');
        });
    };

    cy.wrap(null).then(function retry() {
      return checkCondition().catch(() => {
        cy.wait(500);
        return retry.call(this);
      });
    });
  });

  // Custom commands with proper chaining
  Cypress.Commands.add('loginAs', (username, password) => {
    cy.visit('/login');
    cy.get('[data-testid="username"]').type(username);
    cy.get('[data-testid="password"]').type(password);
    cy.get('[type="submit"]').click();

    // Return element for further chaining
    return cy.get('[data-testid="user-profile"]');
  });

  it('should chain custom commands', () => {
    cy.loginAs('test@example.com', 'password123')
      .should('be.visible')
      .and('contain', 'test@example.com');
  });

  // Conditional testing
  it('should handle conditional logic', () => {
    cy.get('body').then(($body) => {
      if ($body.find('.modal').length > 0) {
        cy.get('.modal .close').click();
      }

      cy.get('[data-testid="main-content"]').should('be.visible');
    });
  });
});
```

**Best Practices:**

```javascript
describe('Cypress Best Practices', () => {
  // ❌ BAD: Trying to use Cypress commands in vanilla async
  it('bad: async/await with Cypress commands', async () => {
    // DON'T DO THIS!
    // const element = await cy.get('.button'); // Won't work!
  });

  // ✅ GOOD: Use .then() instead
  it('good: using .then()', () => {
    cy.get('.button').then(($button) => {
      // Work with element here
    });
  });

  // ❌ BAD: Conditional testing with if/else directly
  it('bad: direct conditional', () => {
    // if (cy.get('.modal').length > 0) { // Won't work!
    //   cy.get('.close').click();
    // }
  });

  // ✅ GOOD: Conditional testing with .then()
  it('good: conditional with .then()', () => {
    cy.get('body').then(($body) => {
      if ($body.find('.modal').length > 0) {
        cy.get('.modal .close').click();
      }
    });
  });

  // ❌ BAD: Storing Cypress command results in variables
  it('bad: storing command results', () => {
    // const button = cy.get('.button'); // This is a Chainable, not element!
    // button.click(); // Won't work as expected
  });

  // ✅ GOOD: Using aliases or .then()
  it('good: using aliases', () => {
    cy.get('.button').as('myButton');
    cy.get('@myButton').click();
  });
});
```

**Common Mistakes to Avoid:**

1. Using `async/await` with Cypress commands
2. Trying to access command results before they execute
3. Breaking the chain unnecessarily
4. Not understanding subject yielding

**Interview Tips:**

- Cypress commands are enqueued, not executed immediately
- Commands retry automatically until timeout
- Always return Cypress commands from `.then()` for chaining
- Use `.wrap()` to add non-Cypress values to the chain

**Tricky Follow-up Questions:**

**Q: Why doesn't this work?**

```javascript
// WRONG
it('wrong approach', () => {
  const text = cy.get('.element').invoke('text'); // Chainable, not string!
  console.log(text); // Not what you expect!
});

// CORRECT
it('correct approach', () => {
  cy.get('.element').invoke('text').then((text) => {
    console.log(text); // Now it's the actual text
  });
});
```

**Q: How do you make API calls in parallel in Cypress?**

```javascript
it('parallel requests', () => {
  cy.wrap(null).then(() => {
    return Promise.all([
      cy.request('/api/users/1').then(r => r.body),
      cy.request('/api/users/2').then(r => r.body),
      cy.request('/api/users/3').then(r => r.body)
    ]);
  }).then(([user1, user2, user3]) => {
    // All three requests completed in parallel
    expect(user1.id).to.equal(1);
    expect(user2.id).to.equal(2);
    expect(user3.id).to.equal(3);
  });
});
```

---

### Q27: How do you handle test data in Cypress? Explain fixtures, factories, and API test data.

**Answer:**

Test data management is crucial for reliable, maintainable tests. Cypress provides multiple approaches.

**1. Fixtures:**

```javascript
// cypress/fixtures/users.json
{
  "admin": {
    "email": "admin@example.com",
    "password": "Admin123!",
    "role": "admin"
  },
  "regularUser": {
    "email": "user@example.com",
    "password": "User123!",
    "role": "user"
  }
}

// Using fixtures in tests
describe('Fixture Data', () => {
  it('should load fixture data', () => {
    cy.fixture('users').then((users) => {
      const admin = users.admin;

      cy.visit('/login');
      cy.get('[name="email"]').type(admin.email);
      cy.get('[name="password"]').type(admin.password);
      cy.get('[type="submit"]').click();
    });
  });

  it('should load fixture in beforeEach', function() {
    // Load into this.users
    cy.fixture('users').then((users) => {
      this.users = users;
    });
  });

  it('should use loaded fixture', function() {
    // Access from this.users
    const user = this.users.regularUser;

    cy.visit('/login');
    cy.get('[name="email"]').type(user.email);
  });

  it('should use fixture with alias', () => {
    cy.fixture('users').as('userData');

    cy.get('@userData').then((users) => {
      cy.log(users.admin.email);
    });
  });
});
```

**2. Factory Functions:**

```javascript
// cypress/support/factories/userFactory.js
export const UserFactory = {
  // Generate unique user
  build: (overrides = {}) => {
    const timestamp = Date.now();
    return {
      id: timestamp,
      username: `user${timestamp}`,
      email: `user${timestamp}@example.com`,
      password: 'Test123!',
      firstName: 'Test',
      lastName: 'User',
      role: 'user',
      verified: false,
      createdAt: new Date().toISOString(),
      ...overrides
    };
  },

  // Build specific types
  buildAdmin: (overrides = {}) => {
    return UserFactory.build({
      role: 'admin',
      verified: true,
      ...overrides
    });
  },

  buildVerified: (overrides = {}) => {
    return UserFactory.build({
      verified: true,
      ...overrides
    });
  },

  // Build multiple users
  buildList: (count, overrides = {}) => {
    return Array.from({ length: count }, () => UserFactory.build(overrides));
  },

  // Build with specific traits
  buildWithTraits: (traits = [], overrides = {}) => {
    let user = UserFactory.build();

    if (traits.includes('admin')) {
      user.role = 'admin';
    }

    if (traits.includes('verified')) {
      user.verified = true;
    }

    if (traits.includes('premium')) {
      user.subscription = 'premium';
    }

    return { ...user, ...overrides };
  }
};

// Usage
describe('Factory Pattern', () => {
  it('should use factory to generate test data', () => {
    const user = UserFactory.build();

    cy.visit('/register');
    cy.get('[name="username"]').type(user.username);
    cy.get('[name="email"]').type(user.email);
    cy.get('[name="password"]').type(user.password);
  });

  it('should create admin user', () => {
    const admin = UserFactory.buildAdmin({ username: 'superadmin' });

    expect(admin.role).to.equal('admin');
    expect(admin.verified).to.be.true;
    expect(admin.username).to.equal('superadmin');
  });

  it('should create multiple users', () => {
    const users = UserFactory.buildList(5);

    expect(users).to.have.length(5);
    expect(users[0].username).to.not.equal(users[1].username);
  });

  it('should use traits', () => {
    const user = UserFactory.buildWithTraits(['admin', 'verified', 'premium']);

    expect(user.role).to.equal('admin');
    expect(user.verified).to.be.true;
    expect(user.subscription).to.equal('premium');
  });
});
```

**3. API Test Data Setup:**

```javascript
// cypress/support/api/testDataManager.js
export class TestDataManager {
  constructor() {
    this.baseUrl = Cypress.config('baseUrl');
    this.createdResources = [];
  }

  // Create user via API
  createUser(userData) {
    return cy.request('POST', `${this.baseUrl}/api/users`, userData)
      .then((response) => {
        const user = response.body;
        this.createdResources.push({ type: 'user', id: user.id });
        return user;
      });
  }

  // Create product via API
  createProduct(productData) {
    return cy.request('POST', `${this.baseUrl}/api/products`, productData)
      .then((response) => {
        const product = response.body;
        this.createdResources.push({ type: 'product', id: product.id });
        return product;
      });
  }

  // Cleanup all created resources
  cleanup() {
    const cleanupPromises = this.createdResources.map((resource) => {
      return cy.request('DELETE', `${this.baseUrl}/api/${resource.type}s/${resource.id}`);
    });

    return cy.wrap(Promise.all(cleanupPromises)).then(() => {
      this.createdResources = [];
    });
  }

  // Reset database to known state
  resetDatabase() {
    return cy.request('POST', `${this.baseUrl}/api/test/reset-db`);
  }

  // Seed specific scenario
  seedScenario(scenarioName) {
    return cy.request('POST', `${this.baseUrl}/api/test/seed`, { scenario: scenarioName });
  }
}

// Usage
describe('API Test Data', () => {
  let dataManager;

  beforeEach(() => {
    dataManager = new TestDataManager();
  });

  afterEach(() => {
    // Cleanup after each test
    dataManager.cleanup();
  });

  it('should create user via API', () => {
    const userData = {
      username: 'testuser',
      email: 'test@example.com',
      password: 'Test123!'
    };

    dataManager.createUser(userData).then((user) => {
      expect(user.id).to.exist;
      expect(user.username).to.equal('testuser');

      // Now test UI with this user
      cy.visit(`/users/${user.id}`);
      cy.contains(user.username).should('be.visible');
    });
  });

  it('should seed specific scenario', () => {
    dataManager.seedScenario('users-with-posts').then(() => {
      cy.visit('/dashboard');
      cy.get('.user-item').should('have.length.greaterThan', 0);
    });
  });
});
```

**4. Environment-Specific Data:**

```javascript
// cypress.env.json
{
  "testUsers": {
    "dev": {
      "admin": "dev-admin@example.com",
      "user": "dev-user@example.com"
    },
    "staging": {
      "admin": "staging-admin@example.com",
      "user": "staging-user@example.com"
    }
  }
}

// Using environment data
describe('Environment Data', () => {
  it('should use environment-specific data', () => {
    const env = Cypress.env('ENVIRONMENT') || 'dev';
    const testUsers = Cypress.env('testUsers')[env];

    cy.log(`Testing in ${env} environment`);
    cy.log(`Admin: ${testUsers.admin}`);
  });
});
```

**5. Comprehensive Example:**

```javascript
// cypress/support/testData/index.js
import { UserFactory } from '../factories/userFactory';
import { TestDataManager } from '../api/testDataManager';

export class TestDataHelper {
  constructor() {
    this.dataManager = new TestDataManager();
  }

  // Setup complete test scenario
  setupUserWithPosts() {
    const user = UserFactory.build();

    return this.dataManager.createUser(user).then((createdUser) => {
      const posts = [
        { title: 'Post 1', body: 'Content 1', userId: createdUser.id },
        { title: 'Post 2', body: 'Content 2', userId: createdUser.id }
      ];

      return Promise.all(
        posts.map(post => this.dataManager.createProduct(post))
      ).then((createdPosts) => {
        return {
          user: createdUser,
          posts: createdPosts
        };
      });
    });
  }

  // Load from fixture and enhance with unique data
  loadAndEnhanceFixture(fixtureName) {
    return cy.fixture(fixtureName).then((data) => {
      const timestamp = Date.now();

      return {
        ...data,
        uniqueId: timestamp,
        email: `${data.username}_${timestamp}@example.com`
      };
    });
  }

  // Cleanup
  cleanup() {
    return this.dataManager.cleanup();
  }
}

// Usage in tests
describe('Complete Test Data Example', () => {
  let testData;

  beforeEach(() => {
    testData = new TestDataHelper();
  });

  afterEach(() => {
    testData.cleanup();
  });

  it('should setup and test complete scenario', () => {
    testData.setupUserWithPosts().then(({ user, posts }) => {
      // Test with created data
      cy.visit(`/users/${user.id}`);
      cy.contains(user.username).should('be.visible');

      posts.forEach((post) => {
        cy.contains(post.title).should('be.visible');
      });
    });
  });

  it('should combine fixture and factory', () => {
    testData.loadAndEnhanceFixture('users').then((userData) => {
      const user = UserFactory.build({
        ...userData.admin,
        // Override with unique email
        email: userData.email
      });

      cy.log(`Generated user: ${user.email}`);
    });
  });
});
```

**Data Management Strategies:**

```javascript
describe('Test Data Strategies', () => {
  // Strategy 1: Fresh data for each test
  it('uses fresh factory data', () => {
    const user = UserFactory.build();
    // Use user...
  });

  // Strategy 2: Shared fixture data
  before(() => {
    cy.fixture('users').as('users');
  });

  it('uses shared fixture', function() {
    const user = this.users.admin;
    // Use user...
  });

  // Strategy 3: API-created data
  it('creates data via API', () => {
    cy.request('POST', '/api/users', UserFactory.build())
      .then(response => {
        const user = response.body;
        // Use user...
      });
  });

  // Strategy 4: Hybrid approach
  it('combines strategies', () => {
    cy.fixture('users').then((fixedUsers) => {
      const customUser = UserFactory.build({
        ...fixedUsers.template,
        email: `unique_${Date.now()}@example.com`
      });

      cy.request('POST', '/api/users', customUser);
    });
  });
});
```

**Common Mistakes to Avoid:**

1. Hardcoding test data in tests
2. Not cleaning up created test data
3. Using same data across tests (conflicts)
4. Not making data unique (email, username conflicts)

**Interview Tips:**

- Use fixtures for static, reusable data
- Use factories for dynamic, unique data
- Create data via API for speed
- Always clean up test data

**Tricky Follow-up Questions:**

**Q: How do you handle test data dependencies?**

```javascript
describe('Data Dependencies', () => {
  it('creates dependent data', () => {
    // Create user first
    cy.request('POST', '/api/users', UserFactory.build())
      .then((userResponse) => {
        const userId = userResponse.body.id;

        // Create post for that user
        return cy.request('POST', '/api/posts', {
          title: 'Test Post',
          userId: userId
        });
      })
      .then((postResponse) => {
        // Create comment on that post
        return cy.request('POST', '/api/comments', {
          body: 'Test Comment',
          postId: postResponse.body.id
        });
      });
  });
});
```

**Q: How do you ensure test data uniqueness?**

```javascript
// Use timestamps
const timestamp = Date.now();
const email = `test_${timestamp}@example.com`;

// Use UUID
import { v4 as uuidv4 } from 'uuid';
const userId = uuidv4();

// Use random strings
const randomString = Math.random().toString(36).substring(7);

// Combination
const uniqueUser = {
  id: uuidv4(),
  username: `user_${Date.now()}`,
  email: `test_${Math.random().toString(36).substring(7)}@example.com`
};
```

---


### Q28: Explain custom commands in Cypress. How do you create reusable, maintainable test utilities?

**Answer:**

Custom commands allow you to extend Cypress functionality with reusable utilities that encapsulate common patterns.

**Creating Custom Commands:**

```javascript
// cypress/support/commands.js

// 1. Simple custom command
Cypress.Commands.add('login', (email, password) => {
  cy.visit('/login');
  cy.get('[data-testid="email"]').type(email);
  cy.get('[data-testid="password"]').type(password);
  cy.get('[type="submit"]').click();
});

// Usage
cy.login('test@example.com', 'password123');

// 2. Custom command with options
Cypress.Commands.add('loginWithOptions', (email, password, options = {}) => {
  const { rememberMe = false, redirectUrl = '/dashboard' } = options;

  cy.visit('/login');
  cy.get('[data-testid="email"]').type(email);
  cy.get('[data-testid="password"]').type(password);

  if (rememberMe) {
    cy.get('[data-testid="remember-me"]').check();
  }

  cy.get('[type="submit"]').click();
  cy.url().should('include', redirectUrl);
});

// Usage
cy.loginWithOptions('test@example.com', 'password123', {
  rememberMe: true,
  redirectUrl: '/dashboard'
});

// 3. Custom command that returns a value
Cypress.Commands.add('getAuthToken', () => {
  return cy.request('POST', '/api/login', {
    email: 'test@example.com',
    password: 'password123'
  }).then((response) => {
    return response.body.token;
  });
});

// Usage
cy.getAuthToken().then((token) => {
  cy.log(`Token: ${token}`);
});

// 4. Child command (operates on subject)
Cypress.Commands.add('shouldHaveValue', { prevSubject: 'element' }, (subject, expectedValue) => {
  cy.wrap(subject).should('have.value', expectedValue);
  return subject; // Return for chaining
});

// Usage
cy.get('[data-testid="input"]')
  .type('hello')
  .shouldHaveValue('hello')
  .clear();

// 5. Dual command (works with or without subject)
Cypress.Commands.add('console', { prevSubject: 'optional' }, (subject, method = 'log', ...args) => {
  if (subject) {
    console[method]('Subject:', subject, ...args);
    return subject;
  } else {
    console[method](...args);
  }
});

// Usage
cy.get('.element').console('log', 'Found element');
cy.console('log', 'No subject');
```

**Advanced Custom Commands:**

```javascript
// cypress/support/commands.js

// API Authentication
Cypress.Commands.add('loginViaAPI', (email, password) => {
  return cy.request({
    method: 'POST',
    url: '/api/auth/login',
    body: { email, password }
  }).then((response) => {
    const token = response.body.token;

    // Store in localStorage
    window.localStorage.setItem('authToken', token);

    // Set in cookies
    cy.setCookie('auth_token', token);

    return token;
  });
});

// Database operations
Cypress.Commands.add('seedDatabase', (scenario) => {
  return cy.task('db:seed', { scenario });
});

Cypress.Commands.add('clearDatabase', () => {
  return cy.task('db:clear');
});

// Wait for API endpoint
Cypress.Commands.add('waitForAPI', (url, method = 'GET', alias = 'apiCall') => {
  cy.intercept(method, url).as(alias);
  return cy.wait(`@${alias}`);
});

// File upload
Cypress.Commands.add('uploadFile', (selector, filePath, fileName) => {
  return cy.get(selector).then(($input) => {
    return cy.fixture(filePath, 'binary')
      .then(Cypress.Blob.binaryStringToBlob)
      .then((blob) => {
        const file = new File([blob], fileName);
        const dataTransfer = new DataTransfer();
        dataTransfer.items.add(file);

        $input[0].files = dataTransfer.files;
        $input[0].dispatchEvent(new Event('change', { bubbles: true }));
      });
  });
});

// Visual testing
Cypress.Commands.add('matchImageSnapshot', (name, options = {}) => {
  const fileName = `${Cypress.currentTest.title}-${name}`;

  cy.screenshot(fileName, options);

  // Compare with baseline (pseudo-code)
  return cy.task('compareImage', { fileName, ...options });
});

// Accessibility testing
Cypress.Commands.add('checkA11y', (context, options = {}) => {
  cy.injectAxe();
  cy.checkA11y(context, options, violations => {
    violations.forEach(violation => {
      cy.log(`Accessibility violation: ${violation.description}`);
    });
  });
});

// Local storage operations
Cypress.Commands.add('saveLocalStorage', () => {
  return cy.window().then((window) => {
    const storage = {};
    for (let key in window.localStorage) {
      storage[key] = window.localStorage.getItem(key);
    }
    cy.wrap(storage).as('localStorage');
  });
});

Cypress.Commands.add('restoreLocalStorage', () => {
  return cy.get('@localStorage').then((storage) => {
    cy.window().then((window) => {
      Object.keys(storage).forEach(key => {
        window.localStorage.setItem(key, storage[key]);
      });
    });
  });
});

// Retry command
Cypress.Commands.add('retryUntil', (checkFn, maxAttempts = 10, interval = 500) => {
  let attempts = 0;

  function attempt() {
    attempts++;

    return cy.wrap(null).then(() => {
      return checkFn().then((result) => {
        if (result || attempts >= maxAttempts) {
          return result;
        }

        cy.wait(interval);
        return attempt();
      }).catch((error) => {
        if (attempts >= maxAttempts) {
          throw error;
        }

        cy.wait(interval);
        return attempt();
      });
    });
  }

  return attempt();
});
```

**Organized Command Structure:**

```javascript
// cypress/support/commands/auth.commands.js
export const authCommands = {
  login: (email, password) => {
    cy.visit('/login');
    cy.get('[data-testid="email"]').type(email);
    cy.get('[data-testid="password"]').type(password);
    cy.get('[type="submit"]').click();
  },

  loginViaAPI: (email, password) => {
    return cy.request('POST', '/api/login', { email, password })
      .then(response => {
        window.localStorage.setItem('token', response.body.token);
        return response.body.token;
      });
  },

  logout: () => {
    cy.get('[data-testid="logout"]').click();
    cy.url().should('include', '/login');
  }
};

// Register commands
Object.keys(authCommands).forEach((commandName) => {
  Cypress.Commands.add(commandName, authCommands[commandName]);
});

// cypress/support/commands/api.commands.js
export const apiCommands = {
  createUser: (userData) => {
    return cy.request('POST', '/api/users', userData)
      .then(response => response.body);
  },

  deleteUser: (userId) => {
    return cy.request('DELETE', `/api/users/${userId}`);
  },

  updateUser: (userId, updates) => {
    return cy.request('PUT', `/api/users/${userId}`, updates)
      .then(response => response.body);
  }
};

Object.keys(apiCommands).forEach((commandName) => {
  Cypress.Commands.add(commandName, apiCommands[commandName]);
});

// cypress/support/commands/ui.commands.js
export const uiCommands = {
  clickByText: (text) => {
    cy.contains(text).click();
  },

  typeInField: (label, value) => {
    cy.contains('label', label).next('input').type(value);
  },

  selectDropdown: (label, value) => {
    cy.contains('label', label).next('select').select(value);
  },

  fillForm: (formData) => {
    Object.keys(formData).forEach((field) => {
      cy.get(`[name="${field}"]`).type(formData[field]);
    });
  }
};

Object.keys(uiCommands).forEach((commandName) => {
  Cypress.Commands.add(commandName, uiCommands[commandName]);
});

// cypress/support/commands/index.js
import './auth.commands';
import './api.commands';
import './ui.commands';
```

**Using Custom Commands:**

```javascript
describe('Custom Commands Usage', () => {
  beforeEach(() => {
    cy.clearDatabase();
    cy.seedDatabase('basic-users');
  });

  it('should use auth commands', () => {
    cy.login('test@example.com', 'password123');
    cy.url().should('include', '/dashboard');
    cy.logout();
  });

  it('should use API commands', () => {
    cy.createUser({
      username: 'newuser',
      email: 'new@example.com',
      password: 'Test123!'
    }).then((user) => {
      expect(user.id).to.exist;

      cy.updateUser(user.id, { verified: true }).then((updated) => {
        expect(updated.verified).to.be.true;
      });

      cy.deleteUser(user.id);
    });
  });

  it('should use UI commands', () => {
    cy.visit('/register');

    cy.fillForm({
      username: 'testuser',
      email: 'test@example.com',
      password: 'Test123!'
    });

    cy.clickByText('Register');
  });

  it('should use retry command', () => {
    cy.retryUntil(() => {
      return cy.request('/api/status')
        .then(response => response.body.ready === true);
    }, 10, 1000).then((result) => {
      expect(result).to.be.true;
    });
  });

  it('should use local storage commands', () => {
    cy.visit('/settings');
    cy.get('[data-testid="theme"]').select('dark');

    cy.saveLocalStorage();

    cy.visit('/other-page');

    cy.restoreLocalStorage();

    cy.visit('/settings');
    cy.get('[data-testid="theme"]').should('have.value', 'dark');
  });
});
```

**Best Practices:**

```javascript
// ✅ GOOD: Return chainable
Cypress.Commands.add('myCommand', () => {
  return cy.get('.element'); // Can chain further
});

// ❌ BAD: Don't return
Cypress.Commands.add('badCommand', () => {
  cy.get('.element'); // Can't chain
});

// ✅ GOOD: Descriptive names
Cypress.Commands.add('loginAsAdmin', () => {});
Cypress.Commands.add('createUserViaAPI', () => {});

// ❌ BAD: Generic names
Cypress.Commands.add('doStuff', () => {});
Cypress.Commands.add('test', () => {});

// ✅ GOOD: Single responsibility
Cypress.Commands.add('login', (email, password) => {
  // Only handles login
});

// ❌ BAD: Multiple responsibilities
Cypress.Commands.add('loginAndCreatePost', () => {
  // Does too much
});
```

**Common Mistakes to Avoid:**

1. Not returning chainables from custom commands
2. Creating commands that do too much
3. Not organizing commands into categories
4. Forgetting to handle errors

**Interview Tips:**

- Custom commands improve code reusability
- Organize commands by category (auth, API, UI)
- Always return chainables for further chaining
- Use prevSubject for commands that operate on elements

**Tricky Follow-up Questions:**

**Q: What's the difference between parent, child, and dual commands?**

```javascript
// Parent command (no prevSubject)
Cypress.Commands.add('parentCmd', () => {
  return cy.get('.element');
});

cy.parentCmd(); // ✅ Works

// Child command (requires prevSubject)
Cypress.Commands.add('childCmd', { prevSubject: 'element' }, (subject) => {
  return cy.wrap(subject).click();
});

cy.get('.button').childCmd(); // ✅ Works
cy.childCmd(); // ❌ Error - needs subject

// Dual command (prevSubject optional)
Cypress.Commands.add('dualCmd', { prevSubject: 'optional' }, (subject) => {
  if (subject) {
    return cy.wrap(subject);
  }
  return cy.get('.default');
});

cy.get('.element').dualCmd(); // ✅ Works
cy.dualCmd(); // ✅ Also works
```

**Q: How do you overwrite existing Cypress commands?**

```javascript
// Overwrite existing command
Cypress.Commands.overwrite('visit', (originalFn, url, options) => {
  // Add custom logic before visit
  cy.log(`Visiting: ${url}`);

  // Call original with modified options
  return originalFn(url, {
    ...options,
    onBeforeLoad: (win) => {
      // Custom logic
      win.localStorage.setItem('testMode', 'true');

      // Call original callback if exists
      if (options?.onBeforeLoad) {
        options.onBeforeLoad(win);
      }
    }
  });
});

// Now all cy.visit() calls use custom logic
cy.visit('/page'); // Logs and sets testMode
```

---

### Q29: How do you handle dynamic elements, waits, and flaky tests in Cypress?

**Answer:**

Handling timing issues and dynamic content is crucial for stable, reliable tests.

**1. Implicit Waiting (Built-in):**

```javascript
describe('Implicit Waiting', () => {
  it('waits automatically for elements', () => {
    cy.visit('/page');

    // Cypress automatically retries for 4 seconds (default)
    cy.get('.dynamic-element').should('be.visible');

    // Custom timeout
    cy.get('.slow-element', { timeout: 10000 }).should('exist');
  });

  it('retries assertions', () => {
    // This retries the entire assertion chain
    cy.get('.counter')
      .invoke('text')
      .should('not.equal', '0'); // Retries until not 0
  });
});
```

**2. Explicit Waiting:**

```javascript
describe('Explicit Waiting', () => {
  it('should wait for fixed time', () => {
    cy.visit('/page');

    // Wait 1 second
    cy.wait(1000);

    // ⚠️ Avoid this - use .should() instead
  });

  it('should wait for network request', () => {
    // Intercept and wait
    cy.intercept('GET', '/api/users').as('getUsers');
    cy.visit('/users');
    cy.wait('@getUsers').then((interception) => {
      expect(interception.response.statusCode).to.equal(200);
    });
  });

  it('should wait for multiple requests', () => {
    cy.intercept('GET', '/api/users').as('getUsers');
    cy.intercept('GET', '/api/settings').as('getSettings');

    cy.visit('/dashboard');

    cy.wait(['@getUsers', '@getSettings']).then((interceptions) => {
      expect(interceptions[0].response.statusCode).to.equal(200);
      expect(interceptions[1].response.statusCode).to.equal(200);
    });
  });
});
```

**3. Handling Dynamic Elements:**

```javascript
describe('Dynamic Elements', () => {
  it('should wait for element to appear', () => {
    cy.visit('/page');

    // Element appears after delay
    cy.get('.dynamic-content', { timeout: 10000 })
      .should('be.visible');
  });

  it('should wait for element to disappear', () => {
    cy.visit('/page');

    // Wait for loading spinner to disappear
    cy.get('.loading-spinner').should('not.exist');

    // Or
    cy.get('.loading-spinner').should('not.be.visible');
  });

  it('should wait for specific text', () => {
    cy.visit('/page');

    cy.get('.status')
      .should('contain', 'Ready'); // Retries until text appears
  });

  it('should wait for attribute change', () => {
    cy.get('.button')
      .should('not.have.attr', 'disabled')
      .click();
  });

  it('should wait for class change', () => {
    cy.get('.item')
      .should('have.class', 'active');
  });

  it('should wait for element count', () => {
    cy.get('.user-item')
      .should('have.length', 5); // Waits until 5 items
  });
});
```

**4. Retry-ability Patterns:**

```javascript
describe('Retry Patterns', () => {
  it('should use .should() for retry', () => {
    // ✅ GOOD: Retries entire chain
    cy.get('.counter')
      .invoke('text')
      .should('not.equal', '0');
  });

  it('should NOT use .then() for retry', () => {
    // ❌ BAD: Only evaluates once
    cy.get('.counter').then(($el) => {
      expect($el.text()).to.not.equal('0'); // Might fail!
    });
  });

  it('should combine .should() and .then()', () => {
    // ✅ GOOD: Retry then process
    cy.get('.counter')
      .should('not.be.empty')
      .then(($el) => {
        const count = parseInt($el.text());
        expect(count).to.be.greaterThan(0);
      });
  });

  it('should use custom retry logic', () => {
    cy.get('.status').should(($status) => {
      const text = $status.text();

      if (text !== 'Ready') {
        throw new Error('Not ready yet'); // Retries
      }

      expect(text).to.equal('Ready');
    });
  });

  it('should implement polling', () => {
    function checkStatus() {
      return cy.request('/api/status')
        .then((response) => {
          if (response.body.ready !== true) {
            cy.wait(1000);
            return checkStatus();
          }
          return response.body;
        });
    }

    checkStatus().then((status) => {
      expect(status.ready).to.be.true;
    });
  });
});
```

**5. Handling Flaky Tests:**

```javascript
describe('Preventing Flaky Tests', () => {
  it('should wait for page to be ready', () => {
    cy.visit('/page');

    // ✅ GOOD: Wait for critical elements
    cy.get('[data-testid="main-content"]').should('be.visible');
    cy.get('.loading').should('not.exist');

    // Now safe to interact
    cy.get('.button').click();
  });

  it('should wait for API before interaction', () => {
    cy.intercept('GET', '/api/data').as('getData');

    cy.visit('/page');

    // ✅ GOOD: Wait for API
    cy.wait('@getData');

    // Now data is loaded
    cy.get('.data-item').should('have.length.greaterThan', 0);
  });

  it('should handle race conditions', () => {
    cy.visit('/page');

    // ❌ BAD: Race condition
    // cy.get('.button').click();
    // cy.get('.result').should('contain', 'Success');

    // ✅ GOOD: Wait for state
    cy.get('.button')
      .should('not.have.attr', 'disabled')
      .click();

    cy.get('.result')
      .should('be.visible')
      .and('contain', 'Success');
  });

  it('should handle animations', () => {
    // ❌ BAD: Element might be animating
    // cy.get('.modal').click();

    // ✅ GOOD: Wait for animation
    cy.get('.modal')
      .should('be.visible')
      .and('have.css', 'opacity', '1') // Fully visible
      .click();
  });

  it('should handle debounced input', () => {
    // Input has 500ms debounce
    cy.get('[data-testid="search"]').type('query');

    // ✅ GOOD: Wait for debounce
    cy.wait(600); // Wait longer than debounce

    // Or wait for API call
    cy.wait('@searchAPI');
  });

  it('should retry entire test on failure', { retries: 2 }, () => {
    // Flaky test gets 2 retries
    cy.visit('/flaky-page');
    cy.get('.sometimes-missing').should('exist');
  });
});
```

**6. Advanced Waiting Strategies:**

```javascript
describe('Advanced Waiting', () => {
  it('should wait for multiple conditions', () => {
    cy.visit('/page');

    // Wait for all conditions
    cy.get('.container').should(($container) => {
      expect($container).to.be.visible;
      expect($container.find('.item')).to.have.length.greaterThan(0);
      expect($container).to.not.have.class('loading');
    });
  });

  it('should wait for window property', () => {
    cy.visit('/page');

    // Wait for window.dataLoaded to be true
    cy.window().its('dataLoaded').should('equal', true);
  });

  it('should wait for localStorage', () => {
    cy.visit('/page');

    cy.window().then((win) => {
      // Retry until localStorage has token
      cy.wrap(win.localStorage).invoke('getItem', 'token')
        .should('exist');
    });
  });

  it('should wait with custom timeout per assertion', () => {
    cy.get('.slow-element', { timeout: 30000 })
      .should('be.visible', { timeout: 10000 })
      .and('contain', 'text', { timeout: 5000 });
  });

  it('should implement smart retry', () => {
    Cypress.Commands.add('waitUntil', (checkFunction, options = {}) => {
      const {
        timeout = 5000,
        interval = 100,
        errorMsg = 'Timed out waiting for condition'
      } = options;

      const startTime = Date.now();

      function check() {
        return cy.wrap(null).then(() => {
          const result = checkFunction();

          if (result) {
            return result;
          }

          if (Date.now() - startTime >= timeout) {
            throw new Error(errorMsg);
          }

          cy.wait(interval);
          return check();
        });
      }

      return check();
    });

    // Usage
    cy.waitUntil(() => {
      return cy.window().then((win) => win.appReady === true);
    }, {
      timeout: 10000,
      errorMsg: 'App did not become ready'
    });
  });

  it('should chain multiple waits', () => {
    cy.intercept('GET', '/api/config').as('getConfig');
    cy.intercept('GET', '/api/data').as('getData');

    cy.visit('/page');

    // Sequential waits
    cy.wait('@getConfig')
      .then(() => cy.wait('@getData'))
      .then(() => {
        cy.get('.data-loaded').should('exist');
      });
  });
});
```

**7. Debugging Flaky Tests:**

```javascript
describe('Debugging Flaky Tests', () => {
  it('should log intermediate states', () => {
    cy.visit('/page');

    cy.get('.element')
      .should(($el) => {
        cy.log('Element text:', $el.text());
        cy.log('Element visible:', $el.is(':visible'));
        cy.log('Element disabled:', $el.is(':disabled'));
      });
  });

  it('should take screenshots on failure', () => {
    cy.on('fail', (error) => {
      cy.screenshot('failure-screenshot');
      throw error;
    });

    // Test code
  });

  it('should pause on failure in open mode', () => {
    // Cypress pauses on failures in interactive mode
    cy.visit('/page');
    cy.get('.element').should('exist');
  });
});
```

**Common Mistakes to Avoid:**

1. Using `cy.wait(milliseconds)` instead of conditional waits
2. Not waiting for API requests to complete
3. Using `.then()` when `.should()` would retry
4. Not handling loading states

**Interview Tips:**

- Cypress automatically retries commands and assertions
- Use `.should()` for retry-able assertions
- Use `.wait()` for network requests, not arbitrary delays
- Handle loading states explicitly

**Tricky Follow-up Questions:**

**Q: Why doesn't this work as expected?**

```javascript
// ❌ WRONG: .then() doesn't retry
cy.get('.counter').then(($el) => {
  expect($el.text()).to.equal('10'); // Checked only once!
});

// ✅ CORRECT: .should() retries
cy.get('.counter').should('have.text', '10');

// Or
cy.get('.counter').should(($el) => {
  expect($el.text()).to.equal('10'); // Retries!
});
```

**Q: How do you wait for an API call to finish before continuing?**

```javascript
// Setup intercept
cy.intercept('POST', '/api/save').as('saveData');

// Trigger action
cy.get('.save-button').click();

// Wait for API
cy.wait('@saveData').then((interception) => {
  expect(interception.response.statusCode).to.equal(200);

  // Now safe to verify UI
  cy.get('.success-message').should('be.visible');
});
```

---

### Q30: Explain best practices for organizing Cypress tests at scale. How do you structure a large test suite?

**Answer:**

Organizing tests well is crucial for maintainability, scalability, and team collaboration.

**Project Structure:**

```
cypress/
├── e2e/                        # Test files
│   ├── auth/
│   │   ├── login.cy.js
│   │   ├── signup.cy.js
│   │   ├── password-reset.cy.js
│   │   └── two-factor.cy.js
│   ├── users/
│   │   ├── user-crud.cy.js
│   │   ├── user-profile.cy.js
│   │   └── user-permissions.cy.js
│   ├── products/
│   │   ├── product-catalog.cy.js
│   │   ├── product-details.cy.js
│   │   └── product-search.cy.js
│   └── checkout/
│       ├── cart.cy.js
│       ├── payment.cy.js
│       └── order-confirmation.cy.js
│
├── support/                    # Support files
│   ├── commands/               # Custom commands
│   │   ├── auth.commands.js
│   │   ├── api.commands.js
│   │   ├── ui.commands.js
│   │   └── index.js
│   │
│   ├── pageObjects/            # Page Object Model
│   │   ├── BasePage.js
│   │   ├── LoginPage.js
│   │   ├── DashboardPage.js
│   │   ├── UserManagementPage.js
│   │   └── index.js
│   │
│   ├── api/                    # API helpers
│   │   ├── AuthAPI.js
│   │   ├── UserAPI.js
│   │   ├── ProductAPI.js
│   │   └── index.js
│   │
│   ├── factories/              # Test data factories
│   │   ├── UserFactory.js
│   │   ├── ProductFactory.js
│   │   └── index.js
│   │
│   ├── utils/                  # Utility functions
│   │   ├── validators.js
│   │   ├── formatters.js
│   │   ├── generators.js
│   │   └── index.js
│   │
│   ├── fixtures/               # Fixture helpers
│   │   └── fixtureHelper.js
│   │
│   ├── e2e.js                  # Global setup
│   └── commands.js             # Command registration
│
├── fixtures/                   # Test data
│   ├── users.json
│   ├── products.json
│   └── test-scenarios/
│       ├── checkout-flow.json
│       └── admin-workflow.json
│
├── plugins/                    # Plugins
│   └── index.js
│
└── config/                     # Environment configs
    ├── dev.json
    ├── staging.json
    └── production.json
```

**Test Organization Patterns:**

```javascript
// 1. Feature-based organization
describe('User Management', () => {
  describe('User Creation', () => {
    it('should create user with valid data', () => {});
    it('should validate required fields', () => {});
    it('should handle duplicate emails', () => {});
  });

  describe('User Update', () => {
    it('should update user profile', () => {});
    it('should update user permissions', () => {});
  });

  describe('User Deletion', () => {
    it('should delete user', () => {});
    it('should handle cascade deletion', () => {});
  });
});

// 2. User journey organization
describe('E-Commerce User Journey', () => {
  it('should complete full purchase flow', () => {
    cy.visit('/');
    cy.searchProduct('laptop');
    cy.selectProduct('MacBook Pro');
    cy.addToCart();
    cy.proceedToCheckout();
    cy.fillShippingInfo();
    cy.fillPaymentInfo();
    cy.placeOrder();
    cy.verifyOrderConfirmation();
  });
});

// 3. Critical path tests
describe('Smoke Tests', { tags: '@smoke' }, () => {
  it('should login', () => {});
  it('should view dashboard', () => {});
  it('should create record', () => {});
});

describe('Regression Tests', { tags: '@regression' }, () => {
  // All detailed tests
});
```

**Test Suites with Tags:**

```javascript
// cypress/e2e/auth/login.cy.js
describe('Login', { tags: ['@auth', '@critical'] }, () => {
  it('should login with valid credentials', { tags: '@smoke' }, () => {
    cy.login('test@example.com', 'password123');
  });

  it('should show error for invalid credentials', { tags: '@regression' }, () => {
    cy.login('wrong@example.com', 'wrong');
    cy.get('.error').should('be.visible');
  });
});

// Run specific tags
// npx cypress run --env grepTags=@smoke
// npx cypress run --env grepTags=@critical
```

**Shared Setup and Teardown:**

```javascript
// cypress/support/e2e.js
beforeEach(() => {
  // Global setup before each test
  cy.clearCookies();
  cy.clearLocalStorage();

  // Setup test user
  cy.task('db:seed', { scenario: 'basic-user' });
});

afterEach(() => {
  // Global cleanup
  cy.task('db:cleanup');

  // Take screenshot on failure
  if (Cypress.currentTest.state === 'failed') {
    cy.screenshot();
  }
});

// Test-specific setup
describe('User Tests', () => {
  beforeEach(() => {
    // Setup specific to this suite
    cy.loginViaAPI('test@example.com', 'password123');
    cy.visit('/users');
  });

  it('test 1', () => {});
  it('test 2', () => {});
});
```

**Configuration Management:**

```javascript
// cypress.config.js
const { defineConfig } = require('cypress');

module.exports = defineConfig({
  e2e: {
    baseUrl: 'http://localhost:3000',
    specPattern: 'cypress/e2e/**/*.cy.{js,jsx,ts,tsx}',
    supportFile: 'cypress/support/e2e.js',

    env: {
      apiUrl: 'http://localhost:3001',
      testUser: {
        email: 'test@example.com',
        password: 'Test123!'
      }
    },

    retries: {
      runMode: 2,
      openMode: 0
    },

    video: true,
    screenshotOnRunFailure: true,
    viewportWidth: 1280,
    viewportHeight: 720,

    setupNodeEvents(on, config) {
      // Load environment-specific config
      const environment = config.env.ENVIRONMENT || 'dev';
      const envConfig = require(`./cypress/config/${environment}.json`);

      config.env = { ...config.env, ...envConfig };

      return config;
    }
  }
});

// cypress/config/dev.json
{
  "baseUrl": "http://localhost:3000",
  "apiUrl": "http://localhost:3001"
}

// cypress/config/staging.json
{
  "baseUrl": "https://staging.example.com",
  "apiUrl": "https://api-staging.example.com"
}
```

**Test Data Management:**

```javascript
// cypress/support/testData/testDataManager.js
export class TestDataManager {
  constructor() {
    this.createdResources = [];
  }

  async setupScenario(scenarioName) {
    const scenario = await cy.fixture(`test-scenarios/${scenarioName}`);

    // Create users
    for (const user of scenario.users) {
      const created = await this.createUser(user);
      this.createdResources.push({ type: 'user', id: created.id });
    }

    // Create products
    for (const product of scenario.products) {
      const created = await this.createProduct(product);
      this.createdResources.push({ type: 'product', id: created.id });
    }

    return scenario;
  }

  async cleanup() {
    for (const resource of this.createdResources.reverse()) {
      await cy.request('DELETE', `/api/${resource.type}s/${resource.id}`);
    }
    this.createdResources = [];
  }
}

// Usage
describe('Checkout Flow', () => {
  let testData;

  beforeEach(() => {
    testData = new TestDataManager();
    return testData.setupScenario('checkout-flow');
  });

  afterEach(() => {
    return testData.cleanup();
  });

  it('should complete checkout', () => {
    // Test with setup data
  });
});
```

**Parallel Execution:**

```javascript
// package.json
{
  "scripts": {
    "test": "cypress run",
    "test:parallel": "cypress run --parallel --record --key=<key>",
    "test:smoke": "cypress run --env grepTags=@smoke",
    "test:regression": "cypress run --env grepTags=@regression",
    "test:chrome": "cypress run --browser chrome",
    "test:firefox": "cypress run --browser firefox"
  }
}

// Parallel execution with GitHub Actions
// .github/workflows/cypress.yml
name: Cypress Tests

on: [push]

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        containers: [1, 2, 3, 4]

    steps:
      - uses: actions/checkout@v2
      - uses: cypress-io/github-action@v4
        with:
          record: true
          parallel: true
          group: 'E2E Tests'
```

**Reporting:**

```javascript
// cypress.config.js
const { defineConfig } = require('cypress');

module.exports = defineConfig({
  e2e: {
    setupNodeEvents(on, config) {
      // Mochawesome reporter
      require('cypress-mochawesome-reporter/plugin')(on);

      // Custom reporting
      on('after:spec', (spec, results) => {
        console.log(`Finished: ${spec.relative}`);
        console.log(`Tests: ${results.stats.tests}`);
        console.log(`Passes: ${results.stats.passes}`);
        console.log(`Failures: ${results.stats.failures}`);
      });

      return config;
    }
  },
  reporter: 'cypress-mochawesome-reporter',
  reporterOptions: {
    reportDir: 'cypress/reports',
    overwrite: false,
    html: true,
    json: true
  }
});
```

**Best Practices Summary:**

```javascript
describe('Best Practices', () => {
  // 1. One assertion per test (when possible)
  it('should display username', () => {
    cy.get('[data-testid="username"]').should('contain', 'John');
  });

  // 2. Use data-testid for selectors
  it('should use stable selectors', () => {
    cy.get('[data-testid="submit-button"]').click(); // ✅
    // cy.get('.btn.btn-primary.submit'); // ❌ Brittle
  });

  // 3. Keep tests independent
  it('test 1', () => {
    // Setup
    cy.createUser();
    // Test
    // Cleanup
    cy.deleteUser();
  });

  it('test 2', () => {
    // Doesn't depend on test 1
  });

  // 4. Use Page Objects
  it('should use page objects', () => {
    const loginPage = new LoginPage();
    loginPage.open().login('email', 'pass');
  });

  // 5. Setup via API, test via UI
  it('should setup via API', () => {
    cy.createUserViaAPI();
    cy.visit('/users');
    // Verify in UI
  });
});
```

**Common Mistakes to Avoid:**

1. Too many tests in one file
2. Not using Page Object Model
3. Hardcoding test data
4. Not cleaning up test data
5. Tests depending on each other

**Interview Tips:**

- Organize by feature/domain
- Use Page Object Model
- Separate test data from tests
- Tag tests for different suites
- Clean up after tests

**Tricky Follow-up Questions:**

**Q: How do you handle shared state between tests?**

```javascript
// ❌ BAD: Sharing state
describe('Bad State Sharing', () => {
  let sharedUser; // Dangerous!

  it('test 1', () => {
    sharedUser = createUser();
  });

  it('test 2', () => {
    // Depends on test 1 running first
    useUser(sharedUser);
  });
});

// ✅ GOOD: Independent tests
describe('Good Independence', () => {
  beforeEach(() => {
    // Each test gets fresh state
    cy.createUser().as('testUser');
  });

  it('test 1', function() {
    cy.get('@testUser').then(user => {
      // Use user
    });
  });

  it('test 2', function() {
    cy.get('@testUser').then(user => {
      // Each test has own user
    });
  });
});
```

**Q: How do you handle test execution order?**

```javascript
// Tests should work in ANY order

// ✅ GOOD: Tests are order-independent
describe('Order Independent', () => {
  beforeEach(() => {
    cy.setupTestData();
  });

  it('test A', () => {
    // Works standalone
  });

  it('test B', () => {
    // Works standalone
  });
});

// ❌ BAD: Tests depend on order
describe('Order Dependent', () => {
  it('creates user', () => {
    cy.createUser();
  });

  it('updates user', () => {
    cy.updateUser(); // Fails if run alone!
  });
});
```

---

## Conclusion

This comprehensive guide covers the most important JavaScript interview questions for test automation engineers with 10 years of experience, focusing on Cypress.

**Key Takeaways:**

1. **JavaScript Basics**: Understand variables, data types, operators, scope, and truthy/falsy values
2. **Functions & Closures**: Master function types, closures, `this` keyword, callbacks, and higher-order functions
3. **Objects & Arrays**: Know object creation, prototypes, array methods, destructuring, spread/rest, Map/Set
4. **Async JavaScript**: Understand event loop, promises, async/await, and timing functions
5. **ES6+ Features**: Template literals, classes, modules, symbols, iterators, optional chaining
6. **Testing & Cypress**: Master command chaining, test data management, custom commands, handling dynamic elements, and test organization

**Study Tips:**

- Practice writing code examples for each concept
- Understand the "why" behind each feature
- Know common pitfalls and how to avoid them
- Be prepared to explain trade-offs between different approaches
- Relate concepts to real-world Cypress testing scenarios

Good luck with your interviews!

---

**Document Information:**
- **Created**: 2026-01-28
- **Total Questions**: 30
- **Sections**: 6
- **Focus**: JavaScript for Test Automation Engineers (10+ years experience)
- **Framework**: Cypress
- **Level**: Senior/Expert

