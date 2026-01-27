# Phase 1 - Software Testing Basics: Practical Exercises

## Exercise 1: Understanding Cost of Bugs

### Objective
Understand why finding bugs early saves money and time.

### Scenario
You're testing an e-commerce website. A critical bug is found at different phases.

**Bug**: Payment gateway processes double charges

### Task
Calculate and compare the cost impact if this bug is found in:

1. **Requirements Phase**
   - Cost to fix: Update requirement doc
   - Time: 30 minutes
   - Impact: None (caught before development)
   - Estimated cost: $50

2. **Development Phase**
   - Cost to fix: Modify code, unit test
   - Time: 2 hours
   - Impact: Delays feature by 1 day
   - Estimated cost: $500

3. **Production Phase**
   - Cost to fix: Emergency fix, testing, deployment
   - Time: 1 week
   - Impact: Customer refunds, reputation damage, legal issues
   - Estimated cost: $50,000+

**Deliverable**: Create a document showing cost comparison with graphs

---

## Exercise 2: Create Your First Test Plan

### Objective
Learn to write a comprehensive test plan document.

### Scenario
You're assigned to test a "User Registration" feature for a mobile app.

**Requirements**:
- User can register with email and password
- Email must be unique
- Password must be 8-16 characters with 1 uppercase, 1 lowercase, 1 number
- User receives verification email
- User must verify email within 24 hours

### Task
Create a test plan document including:

1. **Test Plan ID**: TP_Registration_001
2. **Introduction**: Brief overview of registration feature
3. **Scope**:
   - In Scope: Email registration, password validation, email verification
   - Out of Scope: Social media login, password recovery
4. **Test Objectives**:
   - Verify successful registration with valid data
   - Verify validation errors with invalid data
   - Verify email verification process
5. **Test Strategy**:
   - Manual testing for UI
   - API testing for backend
   - Both positive and negative testing
6. **Entry Criteria**:
   - Feature deployed in test environment
   - Test data prepared
   - Email server configured
7. **Exit Criteria**:
   - All high-priority test cases executed
   - No critical/high severity bugs open
   - Test coverage > 95%
8. **Test Environment**:
   - Android 12 and above
   - iOS 15 and above
   - Test email server
9. **Resources**:
   - 2 testers
   - 1 test device (Android)
   - 1 test device (iOS)
10. **Risks**:
    - Email server downtime
    - Limited test devices
11. **Schedule**:
    - Test planning: 1 day
    - Test case creation: 2 days
    - Test execution: 3 days
    - Reporting: 1 day

**Deliverable**: Complete test plan document (PDF or Word)

---

## Exercise 3: Equivalence Partitioning Practice

### Objective
Master the technique of dividing inputs into partitions.

### Scenario 1: Age Field Validation
**Requirement**: A website allows users aged 18-65 to register.

**Task**:
1. Identify valid and invalid partitions
2. Select test values from each partition

**Solution**:

| Partition Type | Range | Test Value | Expected Result |
|----------------|-------|------------|-----------------|
| Invalid (Too Young) | < 18 | 10 | Error: "Must be 18 or older" |
| Valid | 18-65 | 30 | Registration successful |
| Invalid (Too Old) | > 65 | 70 | Error: "Age limit exceeded" |

### Scenario 2: Discount Code
**Requirement**:
- Discount code must be 6 alphanumeric characters
- Only uppercase letters and numbers allowed
- Cannot be all numbers or all letters

**Task**: Create partitions and test cases

**Your Turn**: Create a table similar to above with:
- At least 5 partitions
- Test values for each
- Expected results

---

## Exercise 4: Boundary Value Analysis Practice

### Objective
Test at boundaries where errors commonly occur.

### Scenario 1: Product Quantity
**Requirement**: Users can order 1-10 items of a product.

**Task**: Identify boundary values and create test cases

**Solution**:

| Test Case | Input Value | Expected Result | Reason |
|-----------|-------------|-----------------|--------|
| TC_001 | 0 | Error: "Minimum 1 item required" | Below minimum |
| TC_002 | 1 | Order successful | Minimum boundary |
| TC_003 | 2 | Order successful | Just above minimum |
| TC_004 | 9 | Order successful | Just below maximum |
| TC_005 | 10 | Order successful | Maximum boundary |
| TC_006 | 11 | Error: "Maximum 10 items allowed" | Above maximum |

### Scenario 2: File Upload Size
**Requirement**: File upload accepts files between 100KB and 5MB.

**Your Task**:
1. Convert to same unit (KB or MB)
2. Identify boundary values
3. Create 6 test cases covering:
   - Below minimum
   - At minimum
   - Just above minimum
   - Just below maximum
   - At maximum
   - Above maximum

**Deliverable**: Test case table with test data

---

## Exercise 5: Decision Table Testing

### Objective
Test complex business rules with multiple conditions.

### Scenario: Online Shopping Discount
**Business Rules**:
- Registered users get 10% discount
- Orders above $100 get additional 5% discount
- Coupon code adds 15% discount
- Maximum total discount is 25%

**Task**: Create decision table and test cases

**Solution Framework**:

| Rule # | Registered User | Order > $100 | Has Coupon | Expected Discount |
|--------|-----------------|--------------|------------|-------------------|
| 1 | N | N | N | 0% |
| 2 | Y | N | N | 10% |
| 3 | N | Y | N | 5% |
| 4 | N | N | Y | 15% |
| 5 | Y | Y | N | 15% |
| 6 | Y | N | Y | 25% (capped) |
| 7 | N | Y | Y | 20% |
| 8 | Y | Y | Y | 25% (capped) |

**Your Task**:
1. Complete the table
2. Create test cases for each rule
3. Include test data (sample order amounts)
4. Document expected vs actual results

---

## Exercise 6: State Transition Testing

### Objective
Test different states and transitions in an application.

### Scenario: Shopping Cart States

**States**:
1. Empty
2. Has Items
3. Checkout Initiated
4. Payment Processing
5. Order Confirmed
6. Order Cancelled

**Valid Transitions**:
- Empty → Has Items (add item)
- Has Items → Empty (remove all items)
- Has Items → Checkout Initiated (proceed to checkout)
- Checkout Initiated → Payment Processing (enter payment)
- Payment Processing → Order Confirmed (payment success)
- Any state → Order Cancelled (cancel order)

**Task 1**: Create State Transition Diagram
```
[Empty] --add item--> [Has Items] --checkout--> [Checkout Initiated]
   ^                        |                           |
   |                   remove all                  enter payment
   |                        |                           ↓
   +------------------------+              [Payment Processing]
                                                        |
                                                  payment success
                                                        ↓
                                              [Order Confirmed]
```

**Task 2**: Create Test Cases

| TC ID | Current State | Action | Next State | Valid? |
|-------|---------------|--------|------------|--------|
| TC_001 | Empty | Add item | Has Items | Yes |
| TC_002 | Has Items | Remove all | Empty | Yes |
| TC_003 | Has Items | Checkout | Checkout Initiated | Yes |
| TC_004 | Empty | Checkout | Empty | No - Invalid |
| TC_005 | Payment Processing | Cancel | Order Cancelled | Yes |

**Your Task**:
- Complete with 10 more test cases
- Test both valid and invalid transitions
- Document expected error messages for invalid transitions

---

## Exercise 7: Writing Bug Reports

### Objective
Practice writing clear, actionable bug reports.

### Scenario: You found 3 bugs while testing a login page

**Bug 1**: Login button doesn't work with Enter key
**Bug 2**: Password shown in plain text
**Bug 3**: Wrong error message for invalid email

**Task**: Write detailed bug reports for each

**Template**:
```markdown
**Bug ID**: BUG-XXX
**Summary**: [One line description]
**Module**: Login
**Severity**: [Critical/High/Medium/Low]
**Priority**: [P1/P2/P3/P4]

**Description**:
[Detailed explanation of the issue]

**Preconditions**:
- [Any setup needed]

**Steps to Reproduce**:
1.
2.
3.

**Expected Result**:
[What should happen]

**Actual Result**:
[What actually happens]

**Environment**:
- OS:
- Browser:
- Version:

**Attachments**:
- Screenshot/Video

**Additional Information**:
[Any other relevant details]
```

**Your Deliverable**: 3 complete bug reports

---

## Exercise 8: Create Test Cases

### Objective
Write detailed test cases from requirements.

### Scenario: Login Functionality
**Requirements**:
1. Email field is mandatory
2. Password field is mandatory
3. Email must be valid format
4. "Remember Me" checkbox is optional
5. "Forgot Password" link available
6. Login button enabled only when both fields filled
7. Maximum 3 failed attempts, then account locked for 30 minutes
8. Successful login redirects to dashboard

**Task**: Create at least 15 test cases covering:

**Positive Test Cases** (5):
1. Login with valid credentials
2. Login with valid credentials and "Remember Me" checked
3. Login after one failed attempt
4. Login redirects to dashboard
5. Logout and login again

**Negative Test Cases** (8):
1. Login with empty email
2. Login with empty password
3. Login with invalid email format
4. Login with unregistered email
5. Login with wrong password
6. Login with correct email, wrong password (3 times)
7. Login while account is locked
8. SQL injection in email field

**UI Test Cases** (2):
1. Verify "Forgot Password" link is visible
2. Verify login button disabled when fields empty

**Test Case Format**:

| TC ID | Test Scenario | Description | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|-------|---------------|-------------|---------------|------------|-----------|-----------------|----------|
| TC_LOGIN_001 | Valid Login | Verify user can login with valid credentials | User registered | 1. Enter email<br>2. Enter password<br>3. Click Login | Email: test@email.com<br>Pass: Test@123 | Login successful, redirect to dashboard | P1 |

**Deliverable**: Excel/Google Sheet with all test cases

---

## Exercise 9: Requirements Traceability Matrix (RTM)

### Objective
Create RTM to ensure all requirements are covered.

### Scenario: Calculator App Requirements

**Requirements**:
1. REQ-001: User can add two numbers
2. REQ-002: User can subtract two numbers
3. REQ-003: User can multiply two numbers
4. REQ-004: User can divide two numbers
5. REQ-005: Division by zero shows error
6. REQ-006: User can clear screen
7. REQ-007: Results display with 2 decimal places
8. REQ-008: User can perform continuous calculations

**Task**: Create RTM mapping requirements to test cases

**RTM Template**:

| Req ID | Requirement Description | Test Case IDs | Total TCs | Executed | Passed | Failed | Coverage % | Status |
|--------|------------------------|---------------|-----------|----------|--------|--------|------------|--------|
| REQ-001 | Add two numbers | TC_001, TC_002, TC_003 | 3 | 3 | 3 | 0 | 100% | Complete |
| REQ-002 | Subtract two numbers | TC_004, TC_005 | 2 | 2 | 2 | 0 | 100% | Complete |

**Your Task**:
1. Complete the RTM for all requirements
2. Create test cases for each requirement
3. Assume some test cases failed and update status
4. Calculate overall coverage percentage

---

## Exercise 10: Test Execution and Reporting

### Objective
Execute test cases and create test summary report.

### Scenario: E-commerce Website Testing

**Given**: 50 test cases created for an e-commerce site

**Test Execution Results**:
- Executed: 45 test cases
- Not Executed: 5 (due to environment issues)
- Passed: 30
- Failed: 10
- Blocked: 5 (dependent feature not ready)

**Defects Found**:
- Critical: 2 (Payment processing failed, cart data lost)
- High: 3 (Search not working, images not loading, checkout slow)
- Medium: 4 (Filter issues, wrong sorting, UI alignment)
- Low: 1 (Typo in footer)

**Task**: Create Test Summary Report

**Report Structure**:
```markdown
# Test Summary Report

## Project Information
- Project Name: E-commerce Website
- Test Phase: System Testing
- Test Start Date: [Date]
- Test End Date: [Date]
- Report Date: [Date]
- Prepared By: [Your Name]

## Test Execution Summary
### Overall Statistics
- Total Test Cases: 50
- Executed: 45 (90%)
- Not Executed: 5 (10%)

### Execution Results
- Passed: 30 (67%)
- Failed: 10 (22%)
- Blocked: 5 (11%)

### Visual Representation
[Create a pie chart]

## Defect Summary
### Defects by Severity
| Severity | Count | Percentage |
|----------|-------|------------|
| Critical | 2 | 20% |
| High | 3 | 30% |
| Medium | 4 | 40% |
| Low | 1 | 10% |
| **Total** | **10** | **100%** |

### Defects by Status
| Status | Count |
|--------|-------|
| Open | 6 |
| Fixed | 3 |
| Retest | 1 |

## Test Metrics
- Test Pass Percentage: 67%
- Defect Detection Rate: 10 defects / 45 executed = 22%
- Critical Defect Density: 2/50 = 4%

## Risk and Issues
1. Environment instability caused 5 test cases to be skipped
2. 2 critical defects in payment module - high risk
3. Performance issues observed in checkout process

## Recommendations
1. Fix critical payment defects before release
2. Stabilize test environment
3. Conduct focused performance testing
4. Retest all failed and blocked test cases
5. Execute pending 5 test cases

## Entry Criteria Status
✅ Test environment ready
✅ Test data prepared
✅ Test cases reviewed and approved

## Exit Criteria Status
❌ Not all test cases executed (45/50)
❌ Critical defects still open (2 open)
❌ Pass percentage below threshold (67% < 90%)
**Recommendation: DO NOT RELEASE**

## Sign-off
- Test Lead: _______________
- QA Manager: _______________
- Project Manager: _______________
```

**Your Deliverable**: Complete test summary report with charts

---

## Exercise 11: Defect Severity and Priority Matrix

### Objective
Learn to correctly assign severity and priority to defects.

### Task
For each scenario below, determine Severity and Priority:

| Scenario | Severity | Priority | Justification |
|----------|----------|----------|---------------|
| 1. Application crashes when user clicks login | ? | ? | |
| 2. Company logo has wrong color | ? | ? | |
| 3. Admin panel accessible to regular users | ? | ? | |
| 4. Typo in "About Us" page | ? | ? | |
| 5. Search returns no results for all queries | ? | ? | |
| 6. Email notifications not sent | ? | ? | |
| 7. Report generation takes 5 minutes (rarely used) | ? | ? | |
| 8. Payment gateway charges double amount | ? | ? | |
| 9. Wrong company name on invoice before launch | ? | ? | |
| 10. Help button not working | ? | ? | |

**Your Task**:
1. Fill in Severity (Critical/High/Medium/Low)
2. Fill in Priority (P1/P2/P3/P4)
3. Provide justification for each
4. Discuss: Can a low severity bug have high priority? Give examples.

---

## Exercise 12: Smoke Test Suite Creation

### Objective
Create a smoke test suite to verify build stability.

### Scenario: Banking Application

**Critical Functionalities**:
1. User login
2. View account balance
3. Transfer money
4. Pay bills
5. View transaction history
6. Logout

**Task**: Create smoke test cases (max 10-15 test cases, 30 minutes execution)

**Requirements**:
- Cover only critical paths
- Should be executable quickly
- No deep testing
- Automated preferred

**Sample Test Cases**:

| TC ID | Test Case | Steps | Expected Result | Execution Time |
|-------|-----------|-------|-----------------|----------------|
| SMOKE_001 | Verify login | 1. Launch app<br>2. Enter valid credentials<br>3. Click login | User logged in | 1 min |
| SMOKE_002 | View balance | 1. After login<br>2. Go to accounts<br>3. Check balance displays | Balance visible | 1 min |

**Your Task**: Complete remaining 8-13 test cases

**Deliverable**:
- Smoke test suite document
- Checklist format for quick execution
- Mark which tests should be automated first

---

## Exercise 13: Use Case Testing Practice

### Objective
Convert use cases into test scenarios and test cases.

### Scenario: ATM Withdrawal

**Use Case: Withdraw Cash**

**Actors**: Bank Customer, ATM System, Bank Server

**Preconditions**:
- ATM is operational
- ATM has cash available
- User has valid card
- User has sufficient balance

**Main Flow**:
1. User inserts card
2. System reads card
3. System prompts for PIN
4. User enters PIN
5. System validates PIN with bank server
6. System displays main menu
7. User selects "Withdraw Cash"
8. System displays withdrawal amounts
9. User selects amount
10. System checks balance with bank server
11. System dispenses cash
12. System prints receipt
13. System returns card
14. Use case ends

**Alternative Flows**:

**A1: Invalid PIN**
- At step 5: System displays "Invalid PIN, 2 attempts remaining"
- Return to step 3
- After 3 failures: System retains card

**A2: Insufficient Balance**
- At step 10: System displays "Insufficient balance"
- Return to step 8 or exit

**A3: ATM Out of Cash**
- At step 11: System displays "ATM out of service"
- System ejects card
- Use case ends

**A4: Connection Error**
- At any step: If connection lost
- System displays "Connection error, try again later"
- System ejects card

**Task**:
1. Create test scenarios from main flow (at least 5)
2. Create test scenarios from alternative flows (at least 4)
3. For each scenario, create detailed test cases with steps
4. Identify positive and negative test cases

**Deliverable**:
- Test scenarios list
- At least 15 detailed test cases
- Test data for each case

---

## Exercise 14: Exploratory Testing Session

### Objective
Practice unscripted, creative testing.

### Scenario: Test a Public Website

**Task**: Perform 30-minute exploratory testing on any public website (e.g., amazon.com, wikipedia.org)

**Session Charter**:
"Explore the search and navigation functionality to discover potential usability issues and bugs"

**Testing Focus**:
- Navigation menus
- Search functionality
- Links
- Forms
- Responsiveness
- Error messages

**During Session, Note Down**:
- What you tested
- How you tested it
- What issues you found
- Ideas for further testing

**Deliverable - Session Report**:

```markdown
# Exploratory Testing Session Report

## Session Information
- Website: [URL]
- Tester: [Your Name]
- Date: [Date]
- Session Duration: 30 minutes
- Charter: [Your charter]

## Areas Explored
1. Homepage navigation
2. Search functionality
3. [Add more]

## Test Ideas Executed
1. Tested search with special characters
2. Clicked all navigation links
3. [Add more]

## Issues Found
1. **Issue 1**: Search returns no results for valid term "xyz"
   - Severity: Medium
   - Steps: [Steps to reproduce]

2. **Issue 2**: Broken link in footer
   - Severity: Low
   - Steps: [Steps to reproduce]

## Areas Not Explored (Need more time)
1. Mobile responsiveness
2. Different browsers
3. [Add more]

## Questions Raised
1. Is search supposed to support wildcards?
2. Should navigation menu be sticky?

## Recommendations
1. Fix search functionality
2. Verify all links regularly
3. [Add more]
```

---

## Exercise 15: End-to-End Test Case Creation

### Objective
Create comprehensive test cases for complete user journey.

### Scenario: Online Food Ordering App

**User Journey**: Order Food End-to-End
1. Open app
2. Login/Register
3. Browse restaurants
4. Select restaurant
5. Browse menu
6. Add items to cart
7. Modify cart (add/remove/quantity)
8. Apply coupon
9. Proceed to checkout
10. Enter delivery address
11. Select payment method
12. Place order
13. Receive confirmation
14. Track order
15. Receive order
16. Rate and review

**Task**: Create test cases covering:

**Functional Testing** (20 test cases):
- Each step in happy path
- Alternative flows
- Error scenarios

**Non-Functional Testing** (5 test cases):
- App loads within 3 seconds
- Can handle 1000 concurrent users
- Works on Android 10+ and iOS 14+
- Payment details encrypted
- Accessible to screen readers

**Your Deliverable**:
- 25 detailed test cases
- Test data for each scenario
- Expected results
- Priority for each test case
- Group test cases by feature/module

---

## Mini Project: Complete Testing Cycle

### Objective
Apply all learned concepts in a real-world simulation.

### Project: Test a "To-Do List" Web Application

**Application Features**:
1. User registration and login
2. Create new task
3. Edit existing task
4. Delete task
5. Mark task as complete
6. Filter tasks (all, active, completed)
7. Search tasks
8. User can logout

**Your Tasks** (2-week project):

### Week 1:
1. **Day 1-2**:
   - Write Test Plan
   - Identify test scenarios (at least 20)

2. **Day 3-5**:
   - Create test cases (minimum 50)
   - Include positive, negative, boundary, equivalence partitioning
   - Create RTM

3. **Day 6-7**:
   - Design test data
   - Prepare test environment checklist

### Week 2:
1. **Day 8-10**:
   - Execute all test cases (use any live to-do app or https://todomvc.com/)
   - Log bugs found
   - Track execution in test management tool/Excel

2. **Day 11-12**:
   - Retest fixed bugs
   - Perform regression testing
   - Perform exploratory testing (2 hours)

3. **Day 13-14**:
   - Create test summary report
   - Calculate metrics
   - Present findings

**Deliverables**:
1. Test Plan document
2. Test case repository (Excel/Google Sheets)
3. Requirements Traceability Matrix
4. Bug reports (at least 5)
5. Test execution report
6. Test summary report with metrics
7. Lessons learned document

**Evaluation Criteria**:
- Completeness of test coverage
- Quality of test cases
- Clarity of bug reports
- Professional documentation
- Metrics calculation accuracy

---

## Practice Resources

### Websites for Practice Testing:
1. **https://the-internet.herokuapp.com/** - Various testing scenarios
2. **https://demo.opencart.com/** - Full e-commerce site
3. **https://automationexercise.com/** - Practice automation site
4. **https://rahulshettyacademy.com/seleniumPractise/** - Practice form
5. **https://saucedemo.com/** - Demo login application
6. **https://todomvc.com/** - To-do applications

### Daily Practice Routine:
- **Week 1**: Exercises 1-5 (Fundamentals)
- **Week 2**: Exercises 6-10 (Test artifacts)
- **Week 3**: Exercises 11-15 (Advanced concepts)
- **Week 4**: Mini Project

---

## Self-Assessment Checklist

After completing these exercises, you should be able to:

- [ ] Explain different SDLC models
- [ ] Differentiate between test levels
- [ ] Apply test design techniques (EP, BVA, Decision Table, State Transition)
- [ ] Write clear, detailed test cases
- [ ] Create test plan documents
- [ ] Write effective bug reports
- [ ] Understand severity and priority
- [ ] Create RTM for requirements coverage
- [ ] Execute test cases and document results
- [ ] Create test summary reports
- [ ] Perform exploratory testing
- [ ] Calculate basic test metrics

**Next Steps**:
- Complete all 15 exercises
- Attempt the mini project
- Review theory concepts
- Move to Phase 1-3: Interview Q&A preparation

---

*Remember: Practice is key! Don't just read - actually create these documents and artifacts.*
