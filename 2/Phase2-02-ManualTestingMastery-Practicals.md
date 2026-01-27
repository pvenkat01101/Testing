# Phase 2 - Manual Testing Mastery: Practical Exercises (Beginner Step-by-Step)

## Exercise 1: Create a Test Strategy (High Level)

### Objective
Build a high-level test strategy for a new release.

### Scenario
You are testing a banking app release that adds "Instant Fund Transfer".

### Step-by-step (Beginner)
1. Create a new document named `TestStrategy_InstantTransfer.md`.
2. Write a short **Purpose** section (2-3 lines) explaining why testing is needed.
3. Add a **Scope** section:
   - In scope: Instant transfer feature and related notifications.
   - Out of scope: Old transfer methods or unrelated modules.
4. Add **Test Types** you will use:
   - Functional, regression, security (basic), and API tests.
5. Add **Risk Areas**:
   - Payment failure, incorrect balances, OTP/2FA, audit log missing.
6. Add **Entry/Exit Criteria** (high level):
   - Entry: Build is stable, test data ready.
   - Exit: P1 tests complete, no critical defects.
7. Add **Environment** details:
   - Devices, OS, browsers, test accounts.
8. Add **Roles/Responsibilities**:
   - Tester 1: UI, Tester 2: API, Developer: fix defects.
9. Review for clarity and keep it to 1 page.

**Deliverable**: One-page test strategy document

---

## Exercise 2: Create a Detailed Test Plan

### Objective
Write a complete test plan for a specific feature.

### Scenario
E-commerce website adds "Apply Coupon" during checkout.

### Step-by-step (Beginner)
1. Create a document named `TestPlan_ApplyCoupon.md`.
2. Write a **Test Plan ID** (example: TP_COUPON_001).
3. Add **Scope**:
   - In scope: Applying coupons, recalculating totals, invalid coupon handling.
   - Out of scope: Payment gateway issues, shipping partner bugs.
4. List **Features to Test**:
   - Apply valid coupon
   - Apply expired coupon
   - Remove coupon
   - Multiple coupons (if supported)
5. Add **Test Approach**:
   - Positive and negative testing
   - Regression of checkout flow
6. Define **Test Data**:
   - Valid coupon, expired coupon, invalid code, high-value cart.
7. Add **Entry Criteria**:
   - Build deployed, cart feature working, coupons available in DB.
8. Add **Exit Criteria**:
   - P1/P2 cases executed, no critical defects, summary report created.
9. Add **Risks and Mitigation**:
   - Risk: coupon service down -> Mitigation: mock data.
10. Add **Schedule and Resources**:
    - 1 tester, 2 days testing.

**Deliverable**: Full test plan document

---

## Exercise 3: Risk-Based Testing Matrix

### Objective
Prioritize tests using impact and probability.

### Scenario
You have 6 features in a release:
- Login
- Payment
- Search
- Profile update
- Notifications
- Help page

### Step-by-step (Beginner)
1. Create a table with columns: Feature, Impact, Probability, Risk Score, Priority.
2. For each feature, assign Impact (High, Medium, Low).
3. Assign Probability (High, Medium, Low).
4. Convert Impact and Probability to numbers (High=3, Medium=2, Low=1).
5. Risk Score = Impact x Probability.
6. Assign Priority:
   - 7-9 = P1
   - 4-6 = P2
   - 1-3 = P3
7. Review if the list matches common sense (payment should be P1).

**Deliverable**: Risk matrix table with priorities

---

## Exercise 4: Three-Point Estimation

### Objective
Estimate testing effort using three-point estimation.

### Scenario
Testing "Password Reset" feature.

### Step-by-step (Beginner)
1. Create a table with tasks: Test case design, Test execution, Regression.
2. For each task, estimate:
   - O (Optimistic), M (Most likely), P (Pessimistic)
3. Use formula: (O + 4M + P) / 6
4. Calculate the estimated hours for each task.
5. Add total estimated effort at bottom.

**Deliverable**: Estimation table

---

## Exercise 5: Entry and Exit Criteria

### Objective
Define clear start and stop rules for testing.

### Scenario
You are testing a "Loan Application" module.

### Step-by-step (Beginner)
1. Create two lists: Entry Criteria and Exit Criteria.
2. Add at least 5 items for each list.
3. Entry examples: Build deployed, test data ready, test environment stable.
4. Exit examples: All P1 tests executed, no critical defects, summary report done.
5. Ensure each item is clear and measurable.

**Deliverable**: Criteria list

---

## Exercise 6: Pairwise Testing

### Objective
Reduce test cases while maintaining coverage.

### Scenario
You must test a login feature with:
- Browser: Chrome, Firefox, Safari
- OS: Windows, macOS
- Language: English, Spanish

### Step-by-step (Beginner)
1. List all parameters and values.
2. Write all possible pairs:
   - Browser + OS
   - Browser + Language
   - OS + Language
3. Create a table of test cases where each pair appears at least once.
4. Aim for 6-7 test cases instead of 12.
5. Verify all pairs are covered.

**Deliverable**: Pairwise combination table

---

## Exercise 7: Classification Tree Method

### Objective
Derive test cases from classification tree.

### Scenario
User registration with fields:
- Email (valid, invalid)
- Password (valid, weak)
- OTP (valid, expired)

### Step-by-step (Beginner)
1. Draw a simple tree with each input as a branch.
2. Under each input, list the classes (valid/invalid, weak/strong, etc.).
3. Combine one choice from each branch to create a test case.
4. Create at least 6 test cases mixing valid and invalid values.
5. Add expected result for each test case.

**Deliverable**: Tree + list of test cases

---

## Exercise 8: Banking Domain Test Scenarios

### Objective
Create realistic test scenarios for banking.

### Scenario
Fund transfer between two accounts.

### Step-by-step (Beginner)
1. List key conditions: valid user, sufficient balance, OTP required.
2. Write 5 positive scenarios (successful transfers).
3. Write 5 negative scenarios (invalid OTP, insufficient funds, invalid beneficiary).
4. Add expected results for each scenario.
5. Mark which scenarios are P1 (critical).

**Deliverable**: 10 scenarios with expected results

---

## Exercise 9: E-Commerce Checkout Validation

### Objective
Validate pricing and promotions.

### Scenario
Cart has 3 items, 1 coupon, and a shipping charge.

### Step-by-step (Beginner)
1. Write down item prices and quantities.
2. Calculate subtotal manually.
3. Apply coupon discount manually.
4. Add tax and shipping manually.
5. Compare with UI total in the app/site.
6. Create at least 6 test cases:
   - Valid coupon
   - Invalid coupon
   - Minimum cart value not met
   - Free shipping threshold
   - Tax applied correctly
   - Remove coupon

**Deliverable**: Test case list with expected totals

---

## Exercise 10: Accessibility Checklist

### Objective
Perform basic accessibility testing.

### Scenario
Login page with username, password, and login button.

### Step-by-step (Beginner)
1. Open the login page in a browser.
2. Press Tab to navigate only with keyboard.
3. Check that focus moves logically: username -> password -> login button.
4. Verify each field has a visible label.
5. Enter invalid data and confirm error message is clear and visible.
6. Use a contrast checker (or browser dev tools) to verify text readability.

**Deliverable**: Accessibility report (pass/fail + notes)

---

## Exercise 11: Localization Testing

### Objective
Validate UI for different locales.

### Scenario
Dashboard supports US and France locales.

### Step-by-step (Beginner)
1. Switch locale to US in the application.
2. Verify date, currency, and decimal formats (MM/DD/YYYY, $).
3. Switch locale to France.
4. Verify date, currency, and decimal formats (DD/MM/YYYY, EUR, comma decimal).
5. Check long text does not break layout.
6. Record any formatting or UI issues.

**Deliverable**: Localization checklist with results

---

## Exercise 12: Cross-Browser and Responsive Testing

### Objective
Ensure UI consistency across browsers and devices.

### Scenario
Product listing page must work on desktop and mobile.

### Step-by-step (Beginner)
1. Create a test matrix:
   - Browsers: Chrome, Firefox, Safari, Edge
   - Devices: Desktop, Tablet, Mobile
2. Open the page in each browser on desktop.
3. Check layout, images, filters, and buttons.
4. Use browser dev tools to simulate tablet/mobile sizes.
5. Check responsiveness: no overlap, buttons visible, usable scrolling.
6. Record results in a table.

**Deliverable**: Browser-device matrix with test results

---

## Exercise 13: Database Validation

### Objective
Verify UI actions update the database correctly.

### Scenario
User places an order.

### Step-by-step (Beginner)
1. Identify expected DB tables (orders, order_items).
2. Place an order in the UI and note the order ID.
3. Query DB for that order ID (or ask dev/DBA to provide query).
4. Verify:
   - Order total matches UI
   - Item count matches cart
   - Status is correct (e.g., NEW)
5. Record findings.

**Deliverable**: DB validation checklist

---

## Exercise 14: Manual API Testing with Postman

### Objective
Test a REST API using Postman.

### Scenario
API endpoint: POST /login

### Step-by-step (Beginner)
1. Open Postman and create a new collection named "Login API".
2. Create a new request: POST `https://example.com/login` (use your test URL).
3. Set header: `Content-Type: application/json`.
4. Add request body (raw JSON) with valid username/password.
5. Click Send and verify:
   - Status code = 200
   - Response contains token
6. Change password to invalid and send again.
7. Verify status code = 401 or 403.
8. Save the request in the collection.
9. Add a short note of results in a text file.

**Deliverable**: Postman collection and test notes
