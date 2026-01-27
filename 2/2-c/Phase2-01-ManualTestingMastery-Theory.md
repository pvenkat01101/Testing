# Phase 2 - Manual Testing Mastery: Comprehensive Theory

**Duration**: Months 4-6
**Focus**: Advanced manual testing concepts, test planning, domain knowledge, and specialized testing
**Target**: Preparing for 10-year testing interview position

---

## Table of Contents

1. [Test Planning & Strategy](#1-test-planning--strategy)
2. [Advanced Test Design Techniques](#2-advanced-test-design-techniques)
3. [Domain Knowledge](#3-domain-knowledge)
4. [Specialized Testing Types](#4-specialized-testing-types)
5. [Test Management Tools](#5-test-management-tools)
6. [Best Practices](#6-best-practices)

---

## 1. Test Planning & Strategy

### 1.1 Test Strategy vs Test Plan

#### Test Strategy

**Definition**: A high-level document that defines the testing approach for the entire organization or project.

**Key Components**:
```
1. Scope (what to test, what not to test)
2. Testing objectives and goals
3. Test levels (unit, integration, system, UAT)
4. Test types (functional, performance, security)
5. Testing approach (manual vs automated)
6. Tools and technologies
7. Risk management approach
8. Resource requirements
9. Training needs
10. Roles and responsibilities
```

**Example Test Strategy Structure**:
```markdown
# Test Strategy for E-Commerce Platform

## 1. Introduction
Purpose and scope of testing

## 2. Testing Objectives
- Ensure functional correctness
- Validate performance under load
- Verify security compliance
- Confirm cross-browser compatibility

## 3. Test Levels
- Unit Testing (Developers)
- Integration Testing (Developers + QA)
- System Testing (QA)
- UAT (Business users)

## 4. Test Types
- Functional: 70% effort
- Performance: 15% effort
- Security: 10% effort
- Accessibility: 5% effort

## 5. Testing Approach
- Manual testing for exploratory and usability
- Automation for regression and API testing
- Risk-based prioritization

## 6. Tools
- Jira: Test management
- Selenium: UI automation
- Postman: API testing
- JMeter: Performance testing

## 7. Entry/Exit Criteria
Entry: Build deployed, test data ready
Exit: 100% P1 tests pass, <5% P2 failures

## 8. Risks
- Environment instability
- Incomplete requirements
- Resource constraints
```

#### Test Plan

**Definition**: A detailed, project-specific document outlining what to test, how to test, when to test, and who will test.

**Key Components**:
```
1. Test Plan ID
2. References (requirements, design docs)
3. Test items (features to test)
4. Features to be tested
5. Features not to be tested
6. Test approach
7. Test deliverables
8. Testing tasks
9. Environmental needs
10. Responsibilities
11. Staffing and training needs
12. Schedule
13. Risks and contingencies
14. Approvals
```

**Example Test Plan**:
```markdown
# Test Plan: User Registration Feature

**Test Plan ID**: TP_REG_001
**Date**: January 27, 2026
**Author**: QA Lead

## 1. Introduction
This test plan covers testing of user registration feature for v2.5 release.

## 2. Test Items
- User registration form
- Email verification
- Password validation
- OTP verification
- Welcome email

## 3. Features to be Tested
✅ Valid registration with all mandatory fields
✅ Email format validation
✅ Password strength validation
✅ Duplicate email check
✅ OTP generation and validation
✅ Email delivery
✅ Database record creation

## 4. Features NOT to be Tested
❌ Social media login (not in scope)
❌ Payment integration (future release)

## 5. Test Approach
- Manual exploratory testing
- Scripted test cases for critical paths
- Automated API tests for backend validation

## 6. Test Environment
- Browser: Chrome 120+, Firefox 115+, Safari 17+
- OS: Windows 11, macOS 14
- Devices: Desktop, Tablet, Mobile
- Test Server: qa-env.example.com
- Database: PostgreSQL 15

## 7. Test Data
- 50 valid email addresses
- 20 invalid email formats
- 30 weak passwords
- 10 strong passwords

## 8. Entry Criteria
✅ Registration feature deployed on QA
✅ Email service configured
✅ Test data populated
✅ Requirements document approved

## 9. Exit Criteria
✅ 100% P1 test cases executed
✅ 95% P2 test cases executed
✅ Zero open critical/high defects
✅ Test summary report completed

## 10. Schedule
- Test design: Jan 20-22 (3 days)
- Test execution: Jan 23-29 (5 days)
- Regression: Jan 30-31 (2 days)
- **Total: 10 days**

## 11. Resources
- QA Lead: Overall coordination
- Tester 1: UI testing
- Tester 2: API testing
- Developer: Bug fixes

## 12. Risks
| Risk | Impact | Probability | Mitigation |
|------|--------|-------------|------------|
| Email service down | High | Medium | Mock email service |
| Environment unstable | High | Low | Daily health checks |
| Incomplete requirements | Medium | Medium | Early stakeholder review |

## 13. Deliverables
- Test cases document
- Test execution report
- Defect report
- Test summary report

## 14. Approvals
- QA Manager: __________
- Dev Manager: __________
- Product Owner: __________
```

#### Comparison Table

| Aspect | Test Strategy | Test Plan |
|--------|---------------|-----------|
| **Level** | Organization/Project | Release/Feature |
| **Scope** | High-level, broad | Detailed, specific |
| **Duration** | Long-term | Short-term (per release) |
| **Owner** | QA Manager/Director | QA Lead/Test Manager |
| **Content** | Why, how (approach) | What, when, who (details) |
| **Updates** | Rarely (yearly/project) | Frequently (per sprint/release) |
| **Example** | "Use risk-based testing" | "Test login 50 times with different data" |

---

### 1.2 Risk-Based Testing (RBT)

**Definition**: A testing approach that prioritizes test activities based on the probability and impact of risks.

**Why Risk-Based Testing?**
- Limited time and resources
- Need to focus on critical areas
- Maximize defect detection
- Reduce business impact

**Risk Assessment Process**:

```
Step 1: Identify Risks
↓
Step 2: Analyze Risks (Impact & Probability)
↓
Step 3: Calculate Risk Score
↓
Step 4: Prioritize Testing
↓
Step 5: Monitor and Adjust
```

**Risk Identification Sources**:
1. **Functional Risks**
   - Complex business logic
   - Integration points
   - New features
   - Modified code areas

2. **Technical Risks**
   - Third-party dependencies
   - Performance bottlenecks
   - Security vulnerabilities
   - Browser/device compatibility

3. **Project Risks**
   - Tight deadlines
   - Resource constraints
   - Unclear requirements
   - Environment issues

**Risk Rating Matrix**:

```
Impact Levels:
- High (3): Financial loss, data corruption, security breach
- Medium (2): Feature not working, poor user experience
- Low (1): UI issues, minor bugs

Probability Levels:
- High (3): Very likely to occur (>60%)
- Medium (2): Moderate chance (30-60%)
- Low (1): Unlikely (<30%)

Risk Score = Impact × Probability
```

**Risk Matrix**:

|  | **Low Probability (1)** | **Medium Probability (2)** | **High Probability (3)** |
|---|---|---|---|
| **High Impact (3)** | 3 (Medium) | 6 (High) | 9 (Critical) |
| **Medium Impact (2)** | 2 (Low) | 4 (Medium) | 6 (High) |
| **Low Impact (1)** | 1 (Low) | 2 (Low) | 3 (Medium) |

**Priority Mapping**:
```
Risk Score 7-9: P1 (Critical) - Test immediately and thoroughly
Risk Score 4-6: P2 (High) - Test after P1
Risk Score 1-3: P3 (Low) - Test if time permits
```

**Real-World Example: E-Commerce Application**

| Feature | Impact | Probability | Risk Score | Priority | Test Effort |
|---------|--------|-------------|-----------|----------|-------------|
| Payment Processing | High (3) | Medium (2) | 6 | P1 | 40% |
| Checkout Flow | High (3) | Medium (2) | 6 | P1 | 30% |
| User Login | Medium (2) | High (3) | 6 | P1 | 15% |
| Product Search | Medium (2) | Medium (2) | 4 | P2 | 10% |
| Wishlist | Low (1) | Low (1) | 1 | P3 | 3% |
| Help/FAQ | Low (1) | Low (1) | 1 | P3 | 2% |

**Risk-Based Test Case Prioritization**:

**P1 Features (60 test cases)**:
```
Payment Processing (30 TC):
- Valid credit card payment
- Invalid card number
- Expired card
- Insufficient funds
- 3D Secure authentication
- Payment timeout
- Multiple currency support
- Refund processing

Checkout Flow (20 TC):
- Add items to cart
- Apply coupon
- Calculate tax
- Select shipping
- Address validation
- Order confirmation

Login (10 TC):
- Valid credentials
- Invalid credentials
- Forgot password
- Session timeout
- Multi-factor authentication
```

**P2 Features (30 test cases)**:
```
Product Search (30 TC):
- Search by keyword
- Filter by category
- Sort by price
- Pagination
```

**P3 Features (10 test cases)**:
```
Wishlist (5 TC)
Help/FAQ (5 TC)
```

**Benefits of RBT**:
- ✅ Optimized test coverage
- ✅ Clear prioritization
- ✅ Stakeholder alignment
- ✅ Defensible decisions
- ✅ Better resource allocation

---

### 1.3 Test Estimation Techniques

Test estimation helps predict the time and resources needed for testing activities.

#### 1.3.1 Three-Point Estimation

**Formula**:
```
E = (O + 4M + P) / 6

Where:
O = Optimistic (best case)
M = Most Likely (realistic case)
P = Pessimistic (worst case)
```

**Example**:

**Task**: Test case design for Login feature

```
O = 2 hours (everything goes smoothly)
M = 4 hours (normal conditions)
P = 10 hours (many interruptions, requirement changes)

E = (2 + 4×4 + 10) / 6
E = (2 + 16 + 10) / 6
E = 28 / 6
E = 4.67 hours ≈ 5 hours
```

**Complete Project Estimation**:

| Task | O (hrs) | M (hrs) | P (hrs) | Estimate (hrs) |
|------|---------|---------|---------|----------------|
| Requirements analysis | 2 | 4 | 8 | 4.67 |
| Test plan creation | 4 | 8 | 16 | 9.33 |
| Test case design | 16 | 24 | 40 | 26.00 |
| Test data preparation | 4 | 8 | 12 | 8.00 |
| Test execution | 20 | 32 | 50 | 33.67 |
| Regression testing | 8 | 12 | 20 | 13.33 |
| Defect reporting | 4 | 6 | 10 | 6.33 |
| Test reporting | 2 | 4 | 6 | 4.00 |
| **TOTAL** | **60** | **98** | **162** | **105.33** |

#### 1.3.2 Work Breakdown Structure (WBS)

**Concept**: Break down testing into smaller, manageable tasks and estimate each.

**Example WBS for Registration Feature**:

```
Registration Feature Testing (40 hours)
│
├── Test Planning (5 hours)
│   ├── Review requirements (1 hour)
│   ├── Define test approach (2 hours)
│   └── Create test plan (2 hours)
│
├── Test Design (12 hours)
│   ├── Identify test scenarios (3 hours)
│   ├── Design positive test cases (4 hours)
│   ├── Design negative test cases (3 hours)
│   └── Review test cases (2 hours)
│
├── Test Data Preparation (4 hours)
│   ├── Valid email addresses (1 hour)
│   ├── Invalid email formats (1 hour)
│   ├── Password variations (1 hour)
│   └── OTP test scenarios (1 hour)
│
├── Test Execution (15 hours)
│   ├── Functional testing (8 hours)
│   ├── Cross-browser testing (4 hours)
│   └── Mobile responsive testing (3 hours)
│
└── Test Reporting (4 hours)
    ├── Defect logging (2 hours)
    └── Test summary report (2 hours)
```

#### 1.3.3 Function Point Analysis

**Concept**: Estimate based on the functional size of the application.

**Steps**:
1. Count function points (inputs, outputs, inquiries, files, interfaces)
2. Apply complexity factors
3. Calculate total function points
4. Convert to test effort

**Simplified Example**:

```
Simple Function: 1 FP × 2 test cases = 2 TC
Medium Function: 2 FP × 3 test cases = 6 TC
Complex Function: 3 FP × 5 test cases = 15 TC

If 1 test case = 30 minutes execution time
Total effort = Total TC × 30 minutes
```

#### 1.3.4 Historical Data Method

**Concept**: Use past project data to estimate similar projects.

**Example**:

```
Previous Project: User Login Testing
- Test cases: 50
- Execution time: 20 hours
- Defects found: 15
- Fix and retest: 10 hours
- Total: 30 hours

Current Project: User Registration (similar complexity)
- Estimated: 30 hours (+/- 20% buffer = 24-36 hours)
```

#### 1.3.5 Delphi Technique

**Concept**: Multiple experts provide estimates independently, then converge through discussion.

**Process**:
```
Round 1: Expert A: 40 hours, Expert B: 50 hours, Expert C: 35 hours
         Average: 41.67 hours

Round 2: Discuss differences
         Expert A: 42 hours, Expert B: 45 hours, Expert C: 40 hours
         Average: 42.33 hours

Round 3: Final consensus: 42 hours
```

---

### 1.4 Entry and Exit Criteria

#### Entry Criteria

**Definition**: Conditions that must be met before testing can begin.

**Purpose**:
- Avoid starting testing prematurely
- Ensure test environment readiness
- Prevent wasted effort

**Common Entry Criteria**:

```
1. Requirements & Design
   ✅ Requirements document approved
   ✅ Design document available
   ✅ User stories baselined
   ✅ Acceptance criteria defined

2. Build & Deployment
   ✅ Build deployed to test environment
   ✅ Build passed smoke tests
   ✅ Build version documented
   ✅ No showstopper bugs from previous build

3. Test Artifacts
   ✅ Test plan approved
   ✅ Test cases reviewed
   ✅ Test data prepared
   ✅ RTM created

4. Environment
   ✅ Test environment available
   ✅ Test environment configured correctly
   ✅ Test accounts created
   ✅ Third-party integrations working (payment, email)

5. Resources
   ✅ Testers available
   ✅ Test tools configured
   ✅ Access permissions granted
```

**Example Entry Criteria for E-Commerce Checkout Testing**:

```markdown
## Entry Criteria: Checkout Testing

### Mandatory (Must Have)
1. ✅ Checkout feature deployed on QA environment
2. ✅ Payment gateway integration completed (test mode)
3. ✅ Test credit cards available
4. ✅ Product catalog populated with test data
5. ✅ User accounts created (registered and guest users)
6. ✅ Inventory management system functional
7. ✅ Email service configured
8. ✅ Tax calculation service working
9. ✅ Smoke test passed (can add items to cart)
10. ✅ No critical bugs blocking checkout flow

### Desirable (Should Have)
1. ⚠️ Test plan reviewed and approved
2. ⚠️ Test cases peer-reviewed
3. ⚠️ Known issues documented
4. ⚠️ Rollback plan available

### Sign-off Required
- Dev Lead: ___________
- QA Lead: ___________
- Date: ___________
```

#### Exit Criteria

**Definition**: Conditions that must be met before testing can be considered complete.

**Purpose**:
- Ensure adequate testing coverage
- Define "done"
- Support release decision

**Common Exit Criteria**:

```
1. Test Execution
   ✅ 100% of P1 test cases executed
   ✅ 95% of P2 test cases executed
   ✅ 80% of P3 test cases executed
   ✅ All failed test cases analyzed

2. Defect Status
   ✅ Zero open critical defects
   ✅ Zero open high-severity defects
   ✅ <5 open medium-severity defects
   ✅ Low-severity defects documented for next release

3. Coverage
   ✅ All requirements covered
   ✅ RTM shows 100% coverage
   ✅ All user stories tested
   ✅ Critical business scenarios validated

4. Regression
   ✅ Regression suite executed
   ✅ Regression suite passed (95%+)
   ✅ No new defects in existing functionality

5. Reporting
   ✅ Test summary report completed
   ✅ Defect report generated
   ✅ Metrics calculated
   ✅ Stakeholder sign-off obtained

6. Documentation
   ✅ Known issues documented
   ✅ Release notes updated
   ✅ Test artifacts archived
```

**Example Exit Criteria for E-Commerce Checkout Testing**:

```markdown
## Exit Criteria: Checkout Testing

### Critical (Must Achieve)
1. ✅ 100% P1 test cases passed
2. ✅ Zero critical defects open
3. ✅ Zero high-severity defects open
4. ✅ Payment processing successful for all major cards
5. ✅ Order confirmation email sent successfully
6. ✅ Inventory updated correctly after order
7. ✅ Regression suite passed (100%)

### Important (Should Achieve)
1. ⚠️ 95% P2 test cases executed
2. ⚠️ <3 medium-severity defects open
3. ⚠️ Cross-browser testing completed (Chrome, Firefox, Safari)
4. ⚠️ Mobile responsive testing completed
5. ⚠️ Performance: Checkout completes in <5 seconds

### Acceptable (May Defer)
1. ℹ️ P3 test cases (nice-to-have features) = 80% executed
2. ℹ️ Low-severity UI bugs documented for next release

### Metrics
- Test Execution Rate: 97%
- Pass Percentage: 94%
- Defect Density: 2.5 defects per test case
- Coverage: 100% requirements tested

### Sign-off Required
- QA Lead: ___________
- Product Owner: ___________
- Release Manager: ___________
- Date: ___________
```

#### Entry vs Exit Criteria Comparison

| Aspect | Entry Criteria | Exit Criteria |
|--------|----------------|---------------|
| **When** | Before testing starts | Before testing ends |
| **Purpose** | Prevent premature start | Prevent premature release |
| **Focus** | Readiness | Completeness |
| **Examples** | Build deployed, data ready | All tests passed, bugs fixed |
| **Owner** | Dev + QA | QA + Product Owner |
| **Risk** | Starting too early wastes time | Exiting too early risks quality |

---

### 1.5 Test Environment Planning

**Definition**: Planning and setting up the infrastructure needed to execute tests.

#### Components of Test Environment

```
1. Hardware
   - Servers
   - Network devices
   - Storage
   - Load balancers

2. Software
   - Operating systems
   - Browsers
   - Databases
   - Application servers
   - Middleware

3. Test Data
   - Master data
   - Transaction data
   - Reference data
   - User accounts

4. Network Configuration
   - Firewalls
   - VPNs
   - Domain settings
   - Proxy settings

5. Third-Party Integrations
   - Payment gateways
   - Email services
   - SMS gateways
   - APIs

6. Tools
   - Test management tools
   - Defect tracking tools
   - Automation tools
   - Monitoring tools
```

#### Test Environment Planning Process

```
Step 1: Understand Requirements
↓
Step 2: Design Environment Architecture
↓
Step 3: Procure Resources
↓
Step 4: Setup and Configuration
↓
Step 5: Validation and Testing
↓
Step 6: Maintenance and Monitoring
```

#### Example Environment Plan

**Application**: E-Commerce Website

```markdown
## Environment Specification

### 1. Test Environment Types

#### A. Development Environment (DEV)
- Purpose: Developer testing
- Data: Sample/mock data
- Access: Developers only
- Uptime: 9 AM - 6 PM

#### B. Quality Assurance Environment (QA)
- Purpose: Functional testing
- Data: Realistic test data
- Access: QA team
- Uptime: 24/7
- **Primary focus for manual testing**

#### C. Staging Environment (STG)
- Purpose: Pre-production validation
- Data: Prod-like data (masked)
- Access: QA + Business users
- Uptime: 24/7
- Configuration: Identical to production

#### D. User Acceptance Testing (UAT)
- Purpose: Business validation
- Data: Real data (subset)
- Access: Business users + QA
- Uptime: Business hours

### 2. QA Environment Details

**Hardware**:
```
Web Server:
- OS: Ubuntu 22.04 LTS
- CPU: 4 cores
- RAM: 16GB
- Storage: 500GB SSD

Database Server:
- OS: Ubuntu 22.04 LTS
- Database: PostgreSQL 15
- CPU: 8 cores
- RAM: 32GB
- Storage: 1TB SSD

Application Server:
- OS: Ubuntu 22.04 LTS
- App Server: Node.js 18
- CPU: 4 cores
- RAM: 16GB
```

**Software**:
```
Frontend:
- React 18.2
- Nginx 1.24

Backend:
- Node.js 18
- Express 4.18
- PostgreSQL 15

External Services:
- Payment: Stripe (test mode)
- Email: SendGrid (test account)
- SMS: Twilio (test numbers)
```

**Browsers**:
```
Desktop:
- Chrome 120+ (primary)
- Firefox 115+
- Safari 17+
- Edge 120+

Mobile:
- Chrome Mobile
- Safari Mobile
- Samsung Internet
```

**Test Accounts**:
```
Admin User:
- Email: admin@test.com
- Password: Test@1234
- Role: Administrator

Regular User:
- Email: user@test.com
- Password: Test@1234
- Role: Customer

Guest User:
- No account required
- Checkout as guest enabled
```

**Test Credit Cards** (Stripe Test Mode):
```
Valid Card:
- Number: 4242424242424242
- Expiry: Any future date
- CVV: Any 3 digits

Declined Card:
- Number: 4000000000000002

Insufficient Funds:
- Number: 4000000000009995
```

**Test Data**:
```
Products:
- 100 products across 10 categories
- Price range: $10 - $1000
- In-stock and out-of-stock items

Coupons:
- SAVE10: 10% off
- SAVE20: 20% off (expired for testing)
- FREESHIP: Free shipping

Tax Rates:
- California: 7.25%
- New York: 8.875%
- Texas: 6.25%
```

### 3. Environment Risks and Mitigation

| Risk | Impact | Mitigation |
|------|--------|------------|
| Environment downtime | High | Monitoring, alerts, redundancy |
| Data corruption | High | Daily backups, restore procedures |
| Integration failures | Medium | Mock services, fallback configs |
| Performance issues | Medium | Regular health checks |
| Access issues | Low | IAM, documentation |

### 4. Environment Maintenance

**Daily**:
- Health check at 9 AM
- Clear test data (orders, carts)
- Review logs for errors

**Weekly**:
- Database backup
- Performance review
- Update test data if needed

**Monthly**:
- Security patches
- Dependency updates
- Environment audit

### 5. Environment Access

**URL**: https://qa.example.com

**VPN Required**: Yes

**Access Request Process**:
1. Submit ticket to IT
2. Manager approval
3. VPN credentials sent
4. Training completed

**Monitoring Dashboard**: https://monitor.qa.example.com
```

#### Common Environment Issues and Solutions

| Issue | Symptom | Solution |
|-------|---------|----------|
| Environment not available | Cannot access URL | Check server status, restart if needed |
| Slow performance | Pages load slowly | Check resource usage, restart services |
| Database connection error | Application crashes | Verify DB credentials, check connectivity |
| Integration failures | Payment/email not working | Verify API keys, check service status |
| Test data issues | Cannot login/checkout | Refresh test data from backup |
| Configuration mismatch | Features work differently | Compare config with expected values |

---

## 2. Advanced Test Design Techniques

### 2.1 Pairwise Testing (All-Pairs Testing)

**Definition**: A test design technique that tests all possible discrete combinations of two parameters at least once.

**Why Pairwise?**
- Most bugs are caused by interactions between 2 parameters (studies show 70-80%)
- Reduces test cases from exhaustive to manageable
- Maintains good coverage

**When to Use**:
- Multiple input parameters (3+)
- Large number of combinations
- Configuration testing
- Limited testing time

**Example 1: Simple Pairwise**

**Scenario**: Testing a login page

**Parameters**:
```
Browser: Chrome, Firefox, Safari
OS: Windows, macOS
Language: English, Spanish
```

**Exhaustive Testing** (all combinations):
```
Total combinations = 3 × 2 × 2 = 12 test cases

1. Chrome + Windows + English
2. Chrome + Windows + Spanish
3. Chrome + macOS + English
4. Chrome + macOS + Spanish
5. Firefox + Windows + English
6. Firefox + Windows + Spanish
7. Firefox + macOS + English
8. Firefox + macOS + Spanish
9. Safari + Windows + English
10. Safari + Windows + Spanish
11. Safari + macOS + English
12. Safari + macOS + Spanish
```

**Pairwise Testing** (all pairs covered):
```
Reduced to 6-7 test cases:

1. Chrome + Windows + English
2. Chrome + macOS + Spanish
3. Firefox + Windows + Spanish
4. Firefox + macOS + English
5. Safari + Windows + English
6. Safari + macOS + Spanish

Pairs covered:
- Browser + OS: All 6 pairs (Chrome-Windows, Chrome-macOS, etc.)
- Browser + Language: All 6 pairs (Chrome-English, Chrome-Spanish, etc.)
- OS + Language: All 4 pairs (Windows-English, Windows-Spanish, etc.)

Reduction: From 12 to 6 test cases (50% reduction)
```

**Example 2: Complex Pairwise**

**Scenario**: E-Commerce checkout testing

**Parameters**:
```
A. Payment Method: Credit Card, Debit Card, PayPal, Apple Pay
B. Shipping: Standard, Express, Overnight
C. User Type: Registered, Guest
D. Coupon: Yes, No
E. Location: Domestic, International
```

**Exhaustive**: 4 × 3 × 2 × 2 × 2 = 96 test cases

**Pairwise**: Approximately 12-15 test cases (84% reduction!)

**Sample Pairwise Test Cases**:

| TC | Payment | Shipping | User | Coupon | Location |
|----|---------|----------|------|--------|----------|
| 1 | Credit Card | Standard | Registered | Yes | Domestic |
| 2 | Credit Card | Express | Guest | No | International |
| 3 | Credit Card | Overnight | Registered | No | Domestic |
| 4 | Debit Card | Standard | Guest | No | International |
| 5 | Debit Card | Express | Registered | Yes | Domestic |
| 6 | Debit Card | Overnight | Guest | Yes | International |
| 7 | PayPal | Standard | Registered | No | International |
| 8 | PayPal | Express | Guest | Yes | Domestic |
| 9 | PayPal | Overnight | Registered | Yes | International |
| 10 | Apple Pay | Standard | Guest | Yes | Domestic |
| 11 | Apple Pay | Express | Registered | No | Domestic |
| 12 | Apple Pay | Overnight | Guest | No | International |

**Verification**:
- Every pair of values appears in at least one test case
- For example: (Credit Card, Standard), (Credit Card, Registered), (Standard, Registered) all covered

**Tools for Pairwise**:
- **PICT** (Microsoft's Pairwise Independent Combinatorial Testing tool)
- **AllPairs** (James Bach's tool)
- **ACTS** (NIST's tool)

**PICT Example**:

Input file (`config.txt`):
```
Browser: Chrome, Firefox, Safari
OS: Windows, macOS
Language: English, Spanish
```

Command:
```bash
pict config.txt
```

Output:
```
Browser     OS          Language
Chrome      Windows     English
Chrome      macOS       Spanish
Firefox     Windows     Spanish
Firefox     macOS       English
Safari      Windows     English
Safari      macOS       Spanish
```

---

### 2.2 Orthogonal Array Testing

**Definition**: Uses orthogonal arrays (mathematical tables) to design test cases with maximum coverage and minimum test cases.

**When to Use**:
- Very large number of parameters (5+)
- Multiple values per parameter
- Need scientific/mathematical rigor
- Telecom, embedded systems, ERP

**Difference from Pairwise**:
- Pairwise: Tests all 2-way interactions
- Orthogonal Array: Can test 2-way, 3-way, or higher interactions
- Orthogonal Array: More structured, uses mathematical arrays

**Example**:

**Scenario**: Testing a printer configuration

**Parameters**:
```
A. Paper Type: Plain, Glossy, Photo (3 values)
B. Color Mode: Color, Black & White (2 values)
C. Quality: Draft, Normal, High (3 values)
D. Paper Size: A4, Letter, Legal (3 values)
```

**Exhaustive**: 3 × 2 × 3 × 3 = 54 test cases

**Using Orthogonal Array L9 (3^4)**:

| TC | Paper Type | Color Mode | Quality | Paper Size |
|----|------------|------------|---------|------------|
| 1 | Plain | Color | Draft | A4 |
| 2 | Plain | B&W | Normal | Letter |
| 3 | Plain | Color | High | Legal |
| 4 | Glossy | Color | Normal | Legal |
| 5 | Glossy | B&W | High | A4 |
| 6 | Glossy | Color | Draft | Letter |
| 7 | Photo | Color | High | Letter |
| 8 | Photo | B&W | Draft | Legal |
| 9 | Photo | Color | Normal | A4 |

**Result**: 9 test cases instead of 54 (83% reduction)

**Common Orthogonal Arrays**:
```
L4 (2^3): 4 tests, 3 parameters, 2 values each
L8 (2^7): 8 tests, 7 parameters, 2 values each
L9 (3^4): 9 tests, 4 parameters, 3 values each
L16 (2^15): 16 tests, 15 parameters, 2 values each
L27 (3^13): 27 tests, 13 parameters, 3 values each
```

---

### 2.3 Classification Tree Method (CTM)

**Definition**: A systematic test design technique that classifies inputs into categories and combinations.

**Process**:
```
Step 1: Identify input classifications
↓
Step 2: Define classes (valid/invalid values)
↓
Step 3: Create classification tree
↓
Step 4: Derive test cases by combining classes
```

**Example**: User Registration

**Classification Tree**:

```
User Registration
│
├── Email
│   ├── Valid Format (valid@example.com)
│   └── Invalid Format (invalid-email, @example.com, test@)
│
├── Password
│   ├── Valid (8+ chars, uppercase, lowercase, number, special)
│   ├── Too Short (<8 chars)
│   ├── No Uppercase
│   ├── No Number
│   └── No Special Character
│
├── Age
│   ├── Valid (18-100)
│   ├── Below Minimum (<18)
│   └── Above Maximum (>100)
│
└── Terms & Conditions
    ├── Accepted
    └── Not Accepted
```

**Derived Test Cases**:

| TC | Email | Password | Age | Terms | Expected |
|----|-------|----------|-----|-------|----------|
| 1 | Valid | Valid | Valid | Accepted | Success |
| 2 | Invalid | Valid | Valid | Accepted | Error: Invalid email |
| 3 | Valid | Too Short | Valid | Accepted | Error: Password too short |
| 4 | Valid | No Uppercase | Valid | Accepted | Error: Needs uppercase |
| 5 | Valid | Valid | Below Min | Accepted | Error: Must be 18+ |
| 6 | Valid | Valid | Valid | Not Accepted | Error: Accept terms |
| 7 | Invalid | Too Short | Below Min | Not Accepted | Multiple errors |

---

### 2.4 Combinatorial Testing

**Definition**: Testing technique that systematically covers combinations of input parameters.

**Levels**:
- **1-way**: Each parameter value tested at least once
- **2-way (Pairwise)**: All pairs of values tested
- **3-way**: All triples tested
- **t-way**: All t-tuples tested

**Strength vs. Number of Tests**:

```
Example: 5 parameters, 3 values each = 243 exhaustive tests

1-way: 5 tests (96% reduction, poor coverage)
2-way: 9-15 tests (93-94% reduction, good coverage)
3-way: 25-30 tests (88-90% reduction, excellent coverage)
4-way: 60-80 tests (67-75% reduction)
5-way: 243 tests (exhaustive)
```

**When to Use**:
- Configuration testing (browsers, OS, settings)
- Input field validation
- API parameter testing
- Feature flag combinations

---

## 3. Domain Knowledge

Understanding domain-specific requirements, workflows, and testing challenges is crucial for effective testing.

### 3.1 Banking Domain

#### Key Concepts

**1. Account Types**:
```
- Savings Account
- Current Account
- Fixed Deposit (FD)
- Recurring Deposit (RD)
- Loan Accounts (Personal, Home, Auto)
- Credit Card Accounts
```

**2. Transaction Types**:
```
- Deposit
- Withdrawal
- Fund Transfer (same bank, other bank)
- Bill Payment
- Mobile Recharge
- EMI Payment
```

**3. Security Features**:
```
- Multi-factor Authentication (MFA)
- OTP (One-Time Password)
- Transaction Password
- Session Timeout
- Biometric Authentication
- Device Binding
```

#### Critical Test Scenarios

**Scenario 1: Fund Transfer**

**Test Flow**:
```
1. User logs in
2. Navigates to Fund Transfer
3. Selects beneficiary
4. Enters amount
5. Verifies transaction details
6. Enters transaction password/OTP
7. Confirms transfer
8. Receives confirmation
```

**Test Cases**:

```
TC_FT_001: Valid fund transfer within same bank
Precondition: User logged in, sufficient balance
Steps:
1. Select beneficiary (same bank)
2. Enter amount: 500
3. Enter transaction password
4. Click Transfer
Expected:
- Source account debited by 500
- Destination account credited by 500
- Transaction ID generated
- SMS notification sent
- Email confirmation sent
- Transaction appears in statement

TC_FT_002: Insufficient balance
Steps: Transfer amount > available balance
Expected: Error "Insufficient balance"

TC_FT_003: Daily limit exceeded
Steps: Transfer amount > daily limit (e.g., $10,000)
Expected: Error "Daily limit exceeded"

TC_FT_004: Invalid OTP
Steps: Enter wrong OTP
Expected: Error "Invalid OTP", max 3 attempts

TC_FT_005: OTP expired
Steps: Wait 5 minutes, then enter OTP
Expected: Error "OTP expired", resend option

TC_FT_006: Beneficiary not activated
Steps: Transfer to recently added beneficiary
Expected: Error "Beneficiary not activated"

TC_FT_007: Transaction timeout
Steps: Wait at confirmation screen for 10 minutes
Expected: Session timeout, transaction rolled back

TC_FT_008: Concurrent transactions
Steps: Initiate 2 transfers simultaneously
Expected: Only one succeeds, other blocked

TC_FT_009: RTGS vs NEFT vs IMPS
Steps: Compare transfer methods
Expected: Different processing times and limits
```

**Validations**:
```
Balance Accuracy:
- Source balance before = X
- Transfer amount = Y
- Source balance after = X - Y
- Destination balance after = Previous + Y

Audit Trail:
- Transaction logged
- Timestamp recorded
- User ID logged
- IP address captured
- Device info captured

Notifications:
- SMS sent immediately
- Email sent within 1 minute
- App notification (if enabled)
```

#### Banking Domain Defects (Common)

| Defect Type | Example | Severity |
|-------------|---------|----------|
| Balance mismatch | Debit from source but no credit to destination | Critical |
| Rounding errors | 123.456 becomes 123.45 or 123.46 | High |
| Currency conversion | Incorrect exchange rate | High |
| Transaction reversal | Reversal not completing | Critical |
| Audit log missing | Transaction not logged | Critical |
| OTP bypass | Can proceed without OTP | Critical |
| Session fixation | Session not invalidated after logout | High |

---

### 3.2 E-Commerce Domain

#### Key Workflows

**1. Product Catalog**:
```
- Browse categories
- Search products
- Filter and sort
- View product details
- Compare products
```

**2. Shopping Cart**:
```
- Add to cart
- Update quantity
- Remove from cart
- Apply coupon
- Calculate total
```

**3. Checkout**:
```
- Enter shipping address
- Select shipping method
- Enter payment details
- Review order
- Place order
- Order confirmation
```

**4. Order Management**:
```
- Track order
- Cancel order
- Return/refund
- Review product
```

#### Critical Test Scenarios

**Scenario 1: Pricing Calculation**

**Test Flow**:
```
1. Add 3 items to cart
2. Apply coupon
3. Select shipping
4. Verify total calculation
```

**Calculation Logic**:
```
Subtotal = Sum of (Item Price × Quantity)
Discount = Coupon discount amount
Tax = (Subtotal - Discount) × Tax Rate
Shipping = Based on method and location
Total = Subtotal - Discount + Tax + Shipping
```

**Example Calculation**:

```
Cart Items:
- Product A: $50 × 2 = $100
- Product B: $30 × 1 = $30
- Product C: $20 × 3 = $60

Subtotal: $190

Coupon SAVE10 (10% off): -$19
After Discount: $171

Tax (8%): $13.68
Shipping (Standard): $10

Total: $194.68
```

**Test Cases**:

```
TC_CHK_001: Valid pricing calculation
Steps: Add items, apply coupon, select shipping
Expected: Total = $194.68

TC_CHK_002: Minimum cart value for coupon
Coupon: SAVE10 (min $100)
Cart: $95
Expected: Error "Minimum cart value not met"

TC_CHK_003: Free shipping threshold
Cart total > $50 → Free shipping
Cart: $60, Shipping selected: Standard
Expected: Shipping = $0

TC_CHK_004: Expired coupon
Coupon: SAVE20 (expired yesterday)
Expected: Error "Coupon expired"

TC_CHK_005: Invalid coupon code
Coupon: INVALIDCODE
Expected: Error "Invalid coupon"

TC_CHK_006: Remove coupon
Steps: Apply coupon, then remove
Expected: Total recalculated without discount

TC_CHK_007: Tax calculation by location
Location: California (7.25% tax)
Expected: Tax = Subtotal × 0.0725

TC_CHK_008: International shipping
Location: Outside US
Expected: International shipping charges applied

TC_CHK_009: Product out of stock
Steps: Item in cart, stock becomes 0
Expected: Error "Product out of stock"

TC_CHK_010: Inventory update
Steps: Place order
Expected: Product quantity decremented
```

#### E-Commerce Defects (Common)

| Defect Type | Example | Severity |
|-------------|---------|----------|
| Pricing errors | Discount not applied | Critical |
| Inventory issues | Overselling out-of-stock items | Critical |
| Payment failures | Payment success but order not created | Critical |
| Email not sent | No order confirmation email | High |
| Tax miscalculation | Wrong tax rate applied | High |
| Coupon abuse | Coupon used multiple times | High |
| Cart persistence | Cart cleared on logout | Medium |
| Search relevance | Irrelevant search results | Medium |

---

### 3.3 Healthcare Domain

#### Key Concepts

**1. PHI (Protected Health Information)**:
```
- Patient demographics
- Medical history
- Lab results
- Prescriptions
- Insurance information
- Billing details
```

**2. Compliance Requirements**:
```
- HIPAA (US)
- GDPR (EU)
- HITECH
- FDA regulations (for medical devices)
```

**3. Key Workflows**:
```
- Patient Registration
- Appointment Scheduling
- Electronic Health Records (EHR)
- Prescription Management
- Lab Orders and Results
- Billing and Insurance Claims
```

#### Critical Test Scenarios

**Scenario 1: Access Control**

**Test Cases**:

```
TC_AC_001: Doctor access to assigned patient
Steps: Doctor logs in, views patient record
Expected: Access granted

TC_AC_002: Doctor access to unassigned patient
Steps: Doctor logs in, tries to view unassigned patient
Expected: Access denied "Unauthorized access"

TC_AC_003: Nurse access to lab results
Steps: Nurse logs in, views lab results
Expected: Access granted

TC_AC_004: Receptionist access to medical records
Steps: Receptionist logs in, tries to view medical records
Expected: Access denied

TC_AC_005: Audit log for access
Steps: View patient record
Expected: Access logged with user ID, timestamp, patient ID

TC_AC_006: Data encryption
Steps: Intercept network traffic
Expected: Data encrypted in transit (HTTPS)

TC_AC_007: Password policy
Steps: Create weak password
Expected: Error "Password must be at least 12 characters"

TC_AC_008: Session timeout
Steps: Leave system idle for 15 minutes
Expected: Auto logout
```

**Scenario 2: Appointment Scheduling**

```
TC_AP_001: Book available slot
Steps: Select doctor, date, time
Expected: Appointment confirmed

TC_AP_002: Double booking
Steps: Book slot already booked
Expected: Error "Slot not available"

TC_AP_003: Appointment reminder
Steps: Book appointment 24 hours ahead
Expected: Reminder SMS/email sent 24 hours before

TC_AP_004: Cancel appointment
Steps: Cancel confirmed appointment
Expected: Slot available, patient notified

TC_AP_005: Reschedule appointment
Steps: Change appointment date/time
Expected: Old slot freed, new slot booked
```

#### Healthcare Domain Defects (Common)

| Defect Type | Example | Severity |
|-------------|---------|----------|
| Data breach | Unauthorized access to PHI | Critical |
| Wrong patient data | Prescription sent to wrong patient | Critical |
| Missing audit log | Access not logged | Critical |
| Weak encryption | Data not encrypted | Critical |
| Incorrect dosage | Prescription calculation error | Critical |
| Insurance claim error | Wrong claim amount | High |

---

### 3.4 ERP (Enterprise Resource Planning) Systems

#### Key Modules

```
1. Finance & Accounting
   - General Ledger
   - Accounts Payable/Receivable
   - Fixed Assets
   - Cash Management

2. Human Resources
   - Employee Management
   - Payroll
   - Attendance
   - Performance Management

3. Supply Chain
   - Procurement
   - Inventory Management
   - Warehouse Management
   - Logistics

4. Manufacturing
   - Production Planning
   - Bill of Materials (BOM)
   - Quality Control
   - Shop Floor Control

5. Sales & Marketing
   - CRM
   - Sales Orders
   - Quotations
   - Campaign Management
```

#### Critical Test Scenarios

**Scenario 1: Purchase Order to Payment Flow**

**Test Flow**:
```
1. Procurement creates Purchase Order (PO)
2. Manager approves PO
3. PO sent to vendor
4. Goods received
5. Goods Receipt Note (GRN) created
6. Invoice received from vendor
7. Invoice matched with PO and GRN (3-way match)
8. Payment processed
9. Accounting entries created
```

**Test Cases**:

```
TC_PO_001: Create Purchase Order
Steps: Create PO for 100 units @ $10 each
Expected: PO created, status = Pending Approval

TC_PO_002: Approve PO (authorized)
Steps: Manager approves PO
Expected: Status = Approved, email sent to vendor

TC_PO_003: Approve PO (unauthorized)
Steps: Regular employee tries to approve
Expected: Error "Insufficient privileges"

TC_PO_004: Create GRN (full quantity)
Steps: Receive 100 units
Expected: GRN created, inventory updated (+100)

TC_PO_005: Create GRN (partial quantity)
Steps: Receive 80 units
Expected: GRN created, PO partially received

TC_PO_006: Invoice matching (exact match)
Steps: Invoice for 100 units @ $10
Expected: 3-way match successful, payment approved

TC_PO_007: Invoice matching (quantity mismatch)
Steps: Invoice for 110 units, but received 100
Expected: Error "Quantity mismatch", hold for review

TC_PO_008: Invoice matching (price mismatch)
Steps: Invoice @ $12, PO @ $10
Expected: Error "Price mismatch", hold for review

TC_PO_009: Payment processing
Steps: Process payment after match
Expected:
- Payment created in system
- Vendor account credited
- GL entries: Debit Inventory, Credit Accounts Payable

TC_PO_010: End-to-end data integrity
Verify:
- PO quantity = GRN quantity = Invoice quantity
- PO amount = Invoice amount = Payment amount
- Inventory balance correct
- Vendor balance correct
- GL balances correct
```

#### ERP Integration Testing

**Key Integration Points**:

```
1. Finance ↔ Procurement
   - PO creation updates budget
   - Invoice posting creates GL entries

2. HR ↔ Finance
   - Payroll posting to GL
   - Employee expenses to AP

3. Sales ↔ Inventory
   - Sales order reduces inventory
   - Returns increase inventory

4. Manufacturing ↔ Inventory
   - Production consumes raw materials
   - Finished goods added to inventory

5. Inventory ↔ Finance
   - Inventory valuation
   - Cost of goods sold (COGS)
```

**Integration Test Cases**:

```
TC_INT_001: Sales Order → Inventory → Finance
Steps:
1. Create sales order (10 units @ $50)
2. Ship order
Verify:
- Inventory: -10 units
- Finance: Revenue $500, COGS debited

TC_INT_002: Purchase → Inventory → Finance
Steps:
1. Receive goods (100 units @ $10)
2. Post invoice
Verify:
- Inventory: +100 units, value $1000
- Finance: Inventory account debited, AP credited

TC_INT_003: Payroll → Finance
Steps:
1. Run payroll
Verify:
- HR: Salaries calculated
- Finance: Salary expense debited, Cash credited
```

#### ERP Defects (Common)

| Defect Type | Example | Severity |
|-------------|---------|----------|
| Data inconsistency | Inventory count different in two modules | Critical |
| Workflow broken | Approval not triggering next step | Critical |
| Integration failure | GL entry not created | Critical |
| Permission bypass | Unauthorized user can approve | Critical |
| Calculation error | Wrong tax calculation | High |
| Report inaccuracy | Incorrect totals in financial reports | High |

---

## 4. Specialized Testing Types

### 4.1 Accessibility Testing

**Definition**: Testing to ensure the application is usable by people with disabilities.

**Why Important**:
- Legal requirement in many countries (ADA in US, EAA in EU)
- Expands user base
- Improves overall usability
- Social responsibility

#### WCAG 2.1 Guidelines

**Four Principles (POUR)**:

```
1. Perceivable
   - Information must be presentable to users in ways they can perceive

2. Operable
   - Interface components must be operable by all users

3. Understandable
   - Information and interface must be understandable

4. Robust
   - Content must be robust enough to work with various technologies
```

**Conformance Levels**:

```
Level A (Minimum):
- Basic accessibility features
- Essential for some users

Level AA (Recommended):
- Removes major barriers
- Required by most regulations

Level AAA (Enhanced):
- Highest level of accessibility
- Not always achievable for all content
```

#### Common Accessibility Issues

**1. Keyboard Navigation**:

```
Issue: Cannot navigate with keyboard alone
Impact: Users with motor disabilities cannot use the site

Test:
- Press Tab key to navigate
- Verify focus moves logically
- Verify all interactive elements reachable
- No keyboard trap (can escape from any element)

Expected:
- Tab order: Logo → Nav → Main Content → Footer
- Enter/Space activates buttons/links
- Arrow keys navigate menus
- Esc closes modals
```

**2. Screen Reader Compatibility**:

```
Issue: Missing alt text for images
Impact: Blind users don't know image content

Test:
<img src="product.jpg" alt=""> <!-- Bad -->
<img src="product.jpg" alt="Red running shoes"> <!-- Good -->

Screen reader announces: "Red running shoes"
```

**3. Color Contrast**:

```
Issue: Low contrast between text and background
Impact: Users with visual impairments cannot read text

Minimum Ratio:
- Normal text: 4.5:1
- Large text (18pt+): 3:1
- UI components: 3:1

Example:
Bad: Light gray text (#999) on white background (#FFF) = 2.8:1
Good: Dark gray text (#595959) on white background = 7:1

Tool: WAVE, axe DevTools, Color Contrast Analyzer
```

**4. Form Labels**:

```
Issue: Form fields without labels
Impact: Screen readers don't announce field purpose

Bad:
<input type="text" name="email">

Good:
<label for="email">Email Address</label>
<input type="text" id="email" name="email">

Screen reader announces: "Email Address, edit text"
```

**5. Focus Indicators**:

```
Issue: No visible focus indicator
Impact: Keyboard users don't know where they are

Bad:
button:focus {
  outline: none; /* Removes focus indicator */
}

Good:
button:focus {
  outline: 2px solid blue;
  outline-offset: 2px;
}
```

#### Accessibility Testing Checklist

```
☐ 1. Keyboard Navigation
   ☐ Can navigate entire site with keyboard
   ☐ Tab order is logical
   ☐ Focus visible at all times
   ☐ No keyboard traps
   ☐ Enter/Space activate buttons

☐ 2. Screen Reader
   ☐ All images have alt text
   ☐ Form labels present
   ☐ Headings structured (H1 → H2 → H3)
   ☐ Links have descriptive text
   ☐ Error messages read aloud

☐ 3. Visual
   ☐ Text contrast >= 4.5:1
   ☐ Text resizable to 200%
   ☐ Color not sole indicator
   ☐ Focus indicators visible

☐ 4. Forms
   ☐ Labels associated with inputs
   ☐ Error messages clear
   ☐ Required fields indicated
   ☐ Instructions provided

☐ 5. Media
   ☐ Videos have captions
   ☐ Audio has transcripts
   ☐ No auto-play audio

☐ 6. Structure
   ☐ Semantic HTML used
   ☐ Landmarks defined (header, nav, main, footer)
   ☐ Skip links provided
   ☐ Page titles unique and descriptive

☐ 7. Interactive
   ☐ Modals announced
   ☐ Dynamic content updates announced
   ☐ Tooltips accessible
   ☐ Dropdown menus keyboard accessible
```

#### Tools for Accessibility Testing

**Automated Tools**:
```
1. axe DevTools (browser extension)
2. WAVE (Web Accessibility Evaluation Tool)
3. Lighthouse (Chrome DevTools)
4. Pa11y (command-line tool)
5. AccessibilityInsights
```

**Manual Testing Tools**:
```
1. NVDA (screen reader - Windows, free)
2. JAWS (screen reader - Windows, commercial)
3. VoiceOver (screen reader - macOS, built-in)
4. TalkBack (screen reader - Android, built-in)
5. Color Contrast Analyzer
6. HeadingsMap (browser extension)
```

**Test Cases**:

```
TC_ACC_001: Keyboard-only navigation
Steps:
1. Disconnect mouse
2. Navigate entire page with Tab, Shift+Tab, Enter, Arrow keys
Expected: Can reach and activate all interactive elements

TC_ACC_002: Screen reader announcements
Steps:
1. Enable screen reader (NVDA/VoiceOver)
2. Navigate page
Expected: All content announced clearly, images described

TC_ACC_003: Color contrast
Steps:
1. Use Color Contrast Analyzer
2. Check text vs background
Expected: Ratio >= 4.5:1 for normal text

TC_ACC_004: Form labels
Steps:
1. Inspect form fields
2. Verify labels
Expected: All inputs have associated labels

TC_ACC_005: Zoom to 200%
Steps:
1. Zoom browser to 200%
Expected: Content reflows, no horizontal scrolling

TC_ACC_006: Focus indicators
Steps:
1. Tab through page
Expected: Focus clearly visible on each element

TC_ACC_007: Alt text for images
Steps:
1. Inspect images
2. Check alt attributes
Expected: Meaningful alt text present

TC_ACC_008: Skip links
Steps:
1. Tab on page load
Expected: "Skip to main content" link appears

TC_ACC_009: Error messages
Steps:
1. Submit form with errors
2. Enable screen reader
Expected: Errors announced and associated with fields

TC_ACC_010: Video captions
Steps:
1. Play video
2. Enable captions
Expected: Accurate captions displayed
```

---

### 4.2 Localization (L10N) and Internationalization (I18N)

#### Definitions

**Internationalization (I18N)**:
```
The process of designing software so it can be adapted to various languages and regions without engineering changes.

"I" + 18 letters + "N" = I18N

Developer Responsibility:
- Use Unicode for character encoding
- Support multiple languages
- Externalize strings (not hardcoded)
- Handle date/time formats dynamically
- Support different currencies
- Design UI for text expansion
```

**Localization (L10N)**:
```
The process of adapting software for a specific region or language.

"L" + 10 letters + "N" = L10N

Tester Responsibility:
- Verify translations accurate
- Check date/time formats
- Validate currency symbols
- Test text expansion/truncation
- Verify images culturally appropriate
- Test right-to-left (RTL) languages
```

#### Key Areas to Test

**1. Language Translation**:

```
TC_L10N_001: UI Text Translation
Steps:
1. Switch language to Spanish
2. Navigate all pages
Expected: All UI text translated correctly

Common Issues:
- Missing translations (shows English)
- Truncated text (doesn't fit)
- Untranslated error messages
- Mixed languages on same page
```

**2. Date and Time Formats**:

```
US Format: MM/DD/YYYY (01/31/2026)
EU Format: DD/MM/YYYY (31/01/2026)
ISO Format: YYYY-MM-DD (2026-01-31)

TC_L10N_002: Date Format
Steps:
1. Set locale to US
2. View date: 01/31/2026
3. Set locale to UK
4. View date: 31/01/2026
Expected: Date format changes correctly

Time Formats:
US: 12-hour (3:30 PM)
EU: 24-hour (15:30)

TC_L10N_003: Time Format
Expected: Time format matches locale
```

**3. Currency**:

```
TC_L10N_004: Currency Format
Locale: US
Price: $1,234.56 (dollar sign, comma thousands separator, period decimal)

Locale: Germany
Price: 1.234,56 € (euro sign, period thousands separator, comma decimal)

Locale: Japan
Price: ¥1234 (yen sign, no decimal)
```

**4. Number Formats**:

```
US: 1,234,567.89
EU: 1.234.567,89
India: 12,34,567.89 (different grouping)

TC_L10N_005: Number Format
Expected: Thousands separator and decimal point match locale
```

**5. Text Expansion**:

```
English: "Submit" (6 characters)
German: "Einreichen" (10 characters, 67% expansion)
French: "Soumettre" (9 characters, 50% expansion)

TC_L10N_006: Text Expansion
Steps:
1. Switch to German
Expected:
- Text fits in buttons
- No truncation
- No overlapping
- No horizontal scrolling
```

**6. Right-to-Left (RTL) Languages**:

```
Languages: Arabic, Hebrew, Persian, Urdu

TC_L10N_007: RTL Layout
Steps:
1. Switch to Arabic
Expected:
- Text flows right-to-left
- UI mirrored (menu on right, back button points right)
- Numbers remain left-to-right
- Icons flipped appropriately

Example:
English: [← Back] [Content...] [Menu]
Arabic: [Menu] [...المحتوى] [عودة →]
```

**7. Address Formats**:

```
US:
John Doe
123 Main Street
Apartment 4B
New York, NY 10001
United States

UK:
John Doe
123 Main Street
Flat 4B
London
SW1A 1AA
United Kingdom

Japan:
〒100-0001
東京都千代田区千代田1-1
山田太郎
```

**8. Cultural Considerations**:

```
TC_L10N_008: Images and Icons
- Check mark (✓): Positive in West, negative in Japan
- Thumbs up (👍): Positive in US, offensive in some countries
- Colors: Red = danger in West, luck in China

TC_L10N_009: Content Appropriateness
- Avoid religious symbols that may offend
- Check clothing in images (modest for some cultures)
- Verify marketing messages culturally appropriate
```

#### Localization Testing Checklist

```
☐ UI Translation
   ☐ All text translated
   ☐ No truncation
   ☐ Consistent terminology
   ☐ Error messages translated

☐ Date & Time
   ☐ Date format correct
   ☐ Time format correct
   ☐ Time zone handling

☐ Currency
   ☐ Currency symbol correct
   ☐ Decimal separator correct
   ☐ Thousands separator correct
   ☐ Currency conversion (if applicable)

☐ Numbers
   ☐ Decimal format
   ☐ Thousands grouping
   ☐ Phone number format

☐ Text Handling
   ☐ No text expansion issues
   ☐ Line breaks appropriate
   ☐ Hyphenation correct
   ☐ Sorting correct for language

☐ RTL Languages
   ☐ Text flows RTL
   ☐ Layout mirrored
   ☐ Numbers remain LTR
   ☐ Icons flipped

☐ Input Validation
   ☐ Accepts local characters
   ☐ Postal code validation
   ☐ Phone number validation
   ☐ Name formats

☐ Cultural
   ☐ Images appropriate
   ☐ Colors appropriate
   ☐ Content respectful
   ☐ Marketing messages suitable
```

---

### 4.3 Cross-Browser Testing

**Definition**: Testing web applications across different browsers to ensure consistent behavior and appearance.

#### Why Cross-Browser Testing?

**Browser Rendering Differences**:
```
- Different rendering engines:
  - Chrome/Edge: Blink
  - Firefox: Gecko
  - Safari: WebKit

- JavaScript engine differences
- CSS interpretation differences
- HTML5 feature support
- Default styles
```

**Browser Market Share** (example):
```
1. Chrome: 65%
2. Safari: 19%
3. Edge: 5%
4. Firefox: 3%
5. Others: 8%
```

**Risk-Based Approach**:
```
Priority 1 (All features): Chrome, Safari
Priority 2 (Critical features): Edge, Firefox
Priority 3 (Smoke test): Opera, Samsung Internet
```

#### Cross-Browser Testing Matrix

**Desktop Browsers**:

| Browser | Versions | OS | Priority |
|---------|----------|-----|----------|
| Chrome | Latest, Latest-1 | Windows, macOS | P1 |
| Safari | Latest, Latest-1 | macOS | P1 |
| Edge | Latest | Windows | P2 |
| Firefox | Latest | Windows, macOS | P2 |

**Mobile Browsers**:

| Browser | Versions | OS | Device | Priority |
|---------|----------|-----|--------|----------|
| Chrome Mobile | Latest | Android 12+ | Samsung, Pixel | P1 |
| Safari Mobile | Latest | iOS 16+ | iPhone | P1 |
| Samsung Internet | Latest | Android | Samsung | P2 |

#### Common Cross-Browser Issues

**1. CSS Rendering Differences**:

```
Issue: Flexbox behaves differently
Chrome: Works as expected
Safari: Requires -webkit- prefix

Example:
/* Works in Chrome, not Safari */
.container {
  display: flex;
}

/* Works in both */
.container {
  display: -webkit-flex; /* Safari */
  display: flex;
}
```

**2. JavaScript Compatibility**:

```
Issue: Modern JS features not supported in older browsers

Example:
// Not supported in IE11
const arr = [1, 2, 3];
const doubled = arr.map(x => x * 2);

// Supported everywhere
var arr = [1, 2, 3];
var doubled = arr.map(function(x) {
  return x * 2;
});
```

**3. Date Handling**:

```
Issue: Date parsing differs
new Date('01-31-2026') // Works in Chrome, fails in Safari

Solution:
new Date('2026-01-31') // ISO format, works everywhere
```

**4. Form Validation**:

```
Issue: HTML5 validation support varies
<input type="email" required>

Chrome: Shows validation message
Safari: May not show message
Solution: Use JavaScript validation as fallback
```

**5. Font Rendering**:

```
Issue: Fonts render differently
Chrome: Anti-aliasing smooth
Firefox: Slightly different anti-aliasing
Safari: macOS-specific rendering

Test: Visual comparison
```

#### Cross-Browser Testing Strategy

**1. Define Scope**:

```
Step 1: Analyze user analytics
- 70% Chrome → Test all features
- 20% Safari → Test critical features
- 10% Others → Smoke test

Step 2: Define browser matrix
- Must test: Chrome, Safari
- Should test: Edge, Firefox
- Nice to test: Opera, Samsung Internet

Step 3: Define test scope
- P1 browsers: Full regression
- P2 browsers: Critical paths
- P3 browsers: Smoke test
```

**2. Testing Approach**:

```
Manual Testing:
- Visual inspection
- Functional testing
- Usability testing

Automated Testing:
- Selenium WebDriver
- Cross-browser grids (BrowserStack, Sauce Labs)
- Visual regression tools (Percy, Applitools)
```

**3. Test Cases**:

```
TC_CB_001: Login functionality
Browsers: Chrome, Safari, Edge, Firefox
Steps: Login with valid credentials
Expected: Login successful in all browsers

TC_CB_002: Form submission
Browsers: Chrome, Safari
Steps: Submit registration form
Expected: Validation works, form submits successfully

TC_CB_003: Responsive layout
Browsers: Chrome, Safari, Edge
Viewports: 1920x1080, 1366x768, 768x1024
Expected: Layout adapts correctly

TC_CB_004: CSS grid layout
Browsers: Chrome, Safari, Firefox
Expected: Grid displays correctly

TC_CB_005: JavaScript animations
Browsers: Chrome, Safari
Expected: Animations smooth, no jank

TC_CB_006: File upload
Browsers: Chrome, Safari, Edge
Expected: File uploads successfully

TC_CB_007: Date picker
Browsers: All
Expected: Date picker functional, format correct

TC_CB_008: Video playback
Browsers: Chrome, Safari, Firefox
Expected: Video plays, controls work

TC_CB_009: Print functionality
Browsers: Chrome, Safari, Edge
Expected: Print preview correct, prints correctly

TC_CB_010: Browser console errors
Browsers: All
Expected: No JavaScript errors in console
```

**4. Visual Regression Testing**:

```
Tool: Percy, Applitools, BackstopJS

Process:
1. Capture baseline screenshots (Chrome)
2. Capture screenshots on other browsers
3. Compare pixel-by-pixel
4. Highlight differences
5. Approve or reject changes

Example:
Baseline: Homepage on Chrome 120
Compare: Homepage on Safari 17
Result: 5px difference in header height
Action: Investigate and fix
```

#### Tools for Cross-Browser Testing

**Local Testing**:
```
1. BrowserStack Local
2. LambdaTest
3. CrossBrowserTesting
4. Manual testing on VMs
```

**Cloud-Based Testing**:
```
1. BrowserStack (3000+ browser/device combinations)
2. Sauce Labs
3. LambdaTest
4. TestingBot
```

**Automated Testing**:
```
1. Selenium WebDriver
2. Playwright (multi-browser)
3. Cypress (Chrome family)
4. TestCafe (all browsers)
```

**Visual Testing**:
```
1. Percy
2. Applitools Eyes
3. BackstopJS
4. Chromatic (for Storybook)
```

---

### 4.4 Mobile Responsive Testing

**Definition**: Testing web applications on various screen sizes and orientations to ensure optimal user experience.

#### Key Concepts

**Responsive Design**:
```
- Fluid grids (relative units like %, em, rem)
- Flexible images (max-width: 100%)
- Media queries (CSS breakpoints)
- Mobile-first approach
```

**Breakpoints** (common):
```
Mobile: 320px - 767px
Tablet: 768px - 1023px
Desktop: 1024px+

Example:
/* Mobile first */
.container {
  width: 100%;
  padding: 10px;
}

/* Tablet */
@media (min-width: 768px) {
  .container {
    width: 750px;
    margin: 0 auto;
  }
}

/* Desktop */
@media (min-width: 1024px) {
  .container {
    width: 1000px;
  }
}
```

#### Testing Approach

**1. Device Matrix**:

| Device Category | Screen Size | Orientation | Priority |
|-----------------|-------------|-------------|----------|
| Small Mobile | 320x568 (iPhone SE) | Portrait, Landscape | P1 |
| Medium Mobile | 375x667 (iPhone 8) | Portrait, Landscape | P1 |
| Large Mobile | 414x896 (iPhone 11) | Portrait, Landscape | P1 |
| Small Tablet | 768x1024 (iPad Mini) | Portrait, Landscape | P2 |
| Large Tablet | 1024x1366 (iPad Pro) | Portrait, Landscape | P2 |
| Desktop | 1920x1080 | N/A | P1 |

**2. Test Cases**:

```
TC_RESP_001: Layout at 320px width
Steps:
1. Resize browser to 320px width
Expected:
- Content fits without horizontal scroll
- No overlapping elements
- Images scaled appropriately
- Text readable

TC_RESP_002: Navigation menu (mobile)
Steps:
1. View on mobile (375px)
Expected:
- Hamburger menu displayed
- Menu expands on tap
- Menu items accessible
- Menu closes on tap outside

TC_RESP_003: Forms (mobile)
Steps:
1. Fill form on mobile
Expected:
- Input fields full width
- Appropriate keyboard (email, number, tel)
- Labels visible
- Submit button accessible

TC_RESP_004: Images responsive
Steps:
1. View on various sizes
Expected:
- Images scale proportionally
- No pixelation
- No excessive whitespace
- Alt text present

TC_RESP_005: Tables (mobile)
Steps:
1. View table on mobile
Expected:
- Table scrollable horizontally OR
- Table stacked vertically OR
- Table columns prioritized

TC_RESP_006: Touch targets
Steps:
1. Tap buttons/links on mobile
Expected:
- Touch targets minimum 44x44px
- Adequate spacing between targets
- No accidental taps

TC_RESP_007: Orientation change
Steps:
1. Rotate device portrait → landscape
Expected:
- Layout adjusts smoothly
- No content cut off
- No broken layout
- Session persists

TC_RESP_008: Modal dialogs (mobile)
Steps:
1. Open modal on mobile
Expected:
- Modal fills viewport
- Scrollable if content exceeds height
- Close button accessible
- Background dimmed

TC_RESP_009: Performance (mobile)
Steps:
1. Load page on 3G network
Expected:
- Page loads < 5 seconds
- Images lazy-loaded
- Critical CSS inlined
- JavaScript deferred

TC_RESP_010: Font size (mobile)
Steps:
1. View text on mobile
Expected:
- Text readable (min 16px)
- Line height adequate
- No horizontal scrolling to read
```

**3. Testing Methods**:

**Browser DevTools**:
```
Chrome DevTools:
1. Press F12
2. Click device toolbar icon (or Ctrl+Shift+M)
3. Select device preset or custom size
4. Toggle orientation
5. Throttle network (3G, 4G)

Advantages:
- Quick
- Free
- Multiple devices
- Network throttling

Limitations:
- Not real device
- Approximation only
- Touch events simulated
```

**Real Devices**:
```
Advantages:
- Accurate representation
- Real touch interactions
- Actual performance
- Real network conditions

Limitations:
- Expensive
- Limited device variety
- Maintenance overhead

Recommended Devices:
- iPhone (latest, latest-1)
- Samsung Galaxy (latest)
- Google Pixel (latest)
- iPad (latest)
```

**Cloud Testing Platforms**:
```
BrowserStack:
- Real devices in cloud
- 3000+ device combinations
- Screenshots, recordings
- Integration with Selenium

Sauce Labs:
- Similar to BrowserStack
- Live testing
- Automated testing

LambdaTest:
- Real devices
- Responsive testing tool
- Screenshot testing
```

#### Responsive Design Issues (Common)

| Issue | Symptom | Solution |
|-------|---------|----------|
| Horizontal scroll | Content wider than viewport | Use max-width: 100%, overflow-x: hidden |
| Text too small | Unreadable text | Min font-size 16px, rem units |
| Touch targets too small | Difficult to tap | Min 44x44px, adequate spacing |
| Images not scaling | Images overflow | max-width: 100%, height: auto |
| Fixed width elements | Breaks layout | Use relative units (%, rem) |
| Unreadable content | Text overlaps | Adjust line-height, padding |
| Navigation broken | Menu not accessible | Hamburger menu, off-canvas menu |
| Form fields too wide | Extends beyond screen | Input width: 100%, max-width |
| Table not responsive | Horizontal scroll | Stack rows, horizontal scroll, prioritize columns |
| Performance issues | Slow loading | Lazy load images, optimize assets |

---

### 4.5 Database Testing

**Definition**: Testing the database schema, tables, columns, keys, indexes, stored procedures, triggers, and data integrity.

#### Types of Database Testing

**1. Structural Testing**:
```
- Schema validation
- Table structure
- Column data types
- Keys (primary, foreign)
- Indexes
- Constraints
- Views
- Stored procedures
- Triggers
```

**2. Functional Testing**:
```
- CRUD operations (Create, Read, Update, Delete)
- Transaction management
- Business logic in stored procedures
- Data manipulation
```

**3. Non-Functional Testing**:
```
- Performance testing (query optimization)
- Load testing (concurrent users)
- Security testing (SQL injection)
- Data volume testing
```

#### Database Testing Checklist

**Schema Testing**:

```
TC_DB_001: Table structure
Steps:
1. Verify table exists
2. Check column names
3. Verify data types
4. Check NOT NULL constraints
5. Verify default values

Example:
Table: users

Expected Schema:
id INT PRIMARY KEY AUTO_INCREMENT
email VARCHAR(255) NOT NULL UNIQUE
password VARCHAR(255) NOT NULL
first_name VARCHAR(100)
last_name VARCHAR(100)
created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
updated_at TIMESTAMP ON UPDATE CURRENT_TIMESTAMP

Query:
DESCRIBE users;
```

**Data Integrity Testing**:

```
TC_DB_002: Primary key constraint
Steps:
1. Insert record
2. Try to insert duplicate primary key
Expected: Error "Duplicate entry"

TC_DB_003: Foreign key constraint
Steps:
1. Try to insert order with non-existent user_id
Expected: Error "Foreign key constraint fails"

TC_DB_004: NOT NULL constraint
Steps:
1. Try to insert user without email
Expected: Error "Column 'email' cannot be null"

TC_DB_005: UNIQUE constraint
Steps:
1. Insert user with email
2. Insert another user with same email
Expected: Error "Duplicate entry for key 'email'"

TC_DB_006: Check constraint
Steps:
1. Try to insert age = -5
Expected: Error "Check constraint fails" (if age >= 0 constraint)
```

**CRUD Operations**:

```
TC_DB_007: Insert (Create)
SQL:
INSERT INTO users (email, password, first_name, last_name)
VALUES ('test@example.com', 'hashed_password', 'John', 'Doe');

Expected:
- 1 row inserted
- Auto-increment ID assigned
- created_at timestamp set
- Query: SELECT * FROM users WHERE email = 'test@example.com';

TC_DB_008: Select (Read)
SQL:
SELECT * FROM users WHERE id = 1;

Expected:
- Returns correct user record
- All columns present
- Data accurate

TC_DB_009: Update
SQL:
UPDATE users SET first_name = 'Jane' WHERE id = 1;

Expected:
- 1 row updated
- first_name changed to 'Jane'
- updated_at timestamp updated
- Other columns unchanged

TC_DB_010: Delete
SQL:
DELETE FROM users WHERE id = 1;

Expected:
- 1 row deleted
- Cascade delete (if configured) for related records
- Record not found on SELECT
```

**Transaction Testing**:

```
TC_DB_011: Transaction commit
SQL:
START TRANSACTION;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;

Expected:
- Both updates successful
- Data persisted after commit

TC_DB_012: Transaction rollback
SQL:
START TRANSACTION;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 999; -- Invalid ID
ROLLBACK;

Expected:
- First update rolled back
- Balance of account 1 unchanged
- Data consistent

TC_DB_013: Transaction isolation
Setup: Two concurrent transactions updating same record
Expected:
- Depending on isolation level (READ COMMITTED, REPEATABLE READ, etc.)
- No dirty reads
- Proper locking behavior
```

**Stored Procedure Testing**:

```
TC_DB_014: Stored procedure execution
Stored Procedure:
CREATE PROCEDURE GetUserOrders(IN userId INT)
BEGIN
  SELECT * FROM orders WHERE user_id = userId;
END;

Test:
CALL GetUserOrders(1);

Expected:
- Returns all orders for user 1
- Correct columns
- Accurate data

TC_DB_015: Stored procedure with output parameter
Stored Procedure:
CREATE PROCEDURE GetUserCount(OUT userCount INT)
BEGIN
  SELECT COUNT(*) INTO userCount FROM users;
END;

Test:
CALL GetUserCount(@count);
SELECT @count;

Expected:
- Returns correct user count
```

**Data Validation**:

```
TC_DB_016: UI to DB consistency
Steps:
1. Register user via UI
   Email: test@example.com
   Name: John Doe
2. Query database:
   SELECT * FROM users WHERE email = 'test@example.com';

Expected:
- Record exists
- Email matches
- Name matches
- Password hashed (not plain text)
- Timestamps populated

TC_DB_017: Order calculation accuracy
Steps:
1. Place order via UI
   Order total: $150
2. Query database:
   SELECT SUM(quantity * price) AS total
   FROM order_items
   WHERE order_id = 123;

Expected:
- Total = $150
- Item quantities correct
- Prices correct

TC_DB_018: Inventory update
Steps:
1. Product stock before: 100
2. Order 10 units
3. Query database:
   SELECT stock FROM products WHERE id = 456;

Expected:
- Stock = 90
- Inventory decremented correctly
```

**Security Testing**:

```
TC_DB_019: SQL injection prevention
Steps:
1. Enter malicious input:
   Email: test@example.com' OR '1'='1
2. Attempt login

Expected:
- Login fails
- No SQL error
- Query parameterized (not vulnerable)

Bad (Vulnerable):
query = "SELECT * FROM users WHERE email = '" + email + "'";

Good (Safe):
query = "SELECT * FROM users WHERE email = ?";
preparedStatement.setString(1, email);

TC_DB_020: Password storage
Steps:
1. Register user with password "Test@123"
2. Query database:
   SELECT password FROM users WHERE email = 'test@example.com';

Expected:
- Password hashed (bcrypt, argon2, etc.)
- NOT plain text "Test@123"
- Example: $2b$10$N9qo8uLOickgx2ZMRZoMye/IlqJxd7cW...
```

#### Common Database Issues

| Issue | Example | Impact | Solution |
|-------|---------|--------|----------|
| Missing index | Slow query on large table | Performance | Add index on frequently queried columns |
| N+1 query problem | 1 query for list + N queries for details | Performance | Use JOIN or eager loading |
| Deadlock | Two transactions waiting for each other | Crash | Proper locking order, timeout |
| Data inconsistency | Balance doesn't match transactions | Critical | Use transactions, proper constraints |
| Orphaned records | Order exists but user deleted | Data integrity | Use foreign keys with CASCADE |
| SQL injection | Malicious input not sanitized | Security | Use parameterized queries |
| Slow query | Query takes 10+ seconds | Performance | Optimize query, add indexes |
| Data truncation | String too long for column | Error | Increase column size or validate input |

#### Database Testing Tools

**Query Tools**:
```
1. MySQL Workbench
2. pgAdmin (PostgreSQL)
3. SQL Server Management Studio (SSMS)
4. DBeaver (universal)
5. DataGrip (JetBrains)
```

**Testing Tools**:
```
1. DBUnit (Java)
2. Database Rider (Java)
3. tSQLt (SQL Server)
4. pgTAP (PostgreSQL)
5. SQL Test (Redgate)
```

**Load Testing Tools**:
```
1. Apache JMeter
2. HammerDB
3. SysBench
4. Percona Toolkit
```

---

### 4.6 API Testing (Manual with Postman)

**Definition**: Testing application programming interfaces (APIs) to verify functionality, reliability, performance, and security.

#### Why API Testing?

**Advantages over UI Testing**:
```
✅ Faster execution (no browser rendering)
✅ Earlier testing (APIs ready before UI)
✅ Easier automation
✅ Better coverage (direct access to business logic)
✅ Language independent
✅ Easier to isolate issues
```

**When to Test APIs**:
```
- Microservices architecture
- Mobile apps (API backend)
- Third-party integrations
- Web applications (AJAX calls)
- Backend validation
```

#### API Testing Types

**1. Functional Testing**:
```
- Correct HTTP status codes
- Response structure/schema
- Response data accuracy
- Error handling
- Business logic validation
```

**2. Integration Testing**:
```
- API chaining (output of API1 → input of API2)
- Third-party integration
- Database integration
```

**3. Security Testing**:
```
- Authentication
- Authorization
- Data encryption
- Input validation (SQL injection, XSS)
```

**4. Performance Testing**:
```
- Response time
- Throughput
- Concurrent requests
- Load testing
```

#### HTTP Refresher

**HTTP Methods**:

```
GET: Retrieve data (Read)
POST: Create new resource (Create)
PUT: Update entire resource (Update)
PATCH: Update part of resource (Partial Update)
DELETE: Remove resource (Delete)
```

**HTTP Status Codes**:

```
2xx Success:
200 OK - Request successful
201 Created - Resource created
204 No Content - Success, no response body

3xx Redirection:
301 Moved Permanently
302 Found (Temporary Redirect)
304 Not Modified (Cached)

4xx Client Errors:
400 Bad Request - Invalid syntax
401 Unauthorized - Authentication required
403 Forbidden - No permission
404 Not Found - Resource doesn't exist
409 Conflict - Duplicate resource
422 Unprocessable Entity - Validation failed

5xx Server Errors:
500 Internal Server Error - Server crashed
502 Bad Gateway - Upstream server error
503 Service Unavailable - Server down
504 Gateway Timeout - Upstream timeout
```

**HTTP Headers** (common):

```
Request Headers:
Content-Type: application/json
Authorization: Bearer <token>
Accept: application/json
User-Agent: Postman/10.0

Response Headers:
Content-Type: application/json
Set-Cookie: session_id=abc123; HttpOnly
Cache-Control: max-age=3600
Location: /api/users/123 (for 201 Created)
```

#### Postman Basics

**1. Creating Requests**:

```
Step 1: Create Collection
- Collection: "User API"
- Description: "User management endpoints"

Step 2: Add Request
- Name: "Create User"
- Method: POST
- URL: https://api.example.com/users
- Headers:
  Content-Type: application/json
- Body (raw, JSON):
  {
    "email": "test@example.com",
    "password": "Test@123",
    "name": "John Doe"
  }

Step 3: Send Request

Step 4: Verify Response
- Status: 201 Created
- Body:
  {
    "id": 123,
    "email": "test@example.com",
    "name": "John Doe",
    "created_at": "2026-01-27T10:30:00Z"
  }
```

**2. Environment Variables**:

```
Setup:
Environment: "QA"
Variables:
- base_url: https://qa.api.example.com
- auth_token: (empty, will be set dynamically)

Usage in Requests:
GET {{base_url}}/users
Authorization: Bearer {{auth_token}}

Benefits:
- Easy environment switching (QA, Staging, Prod)
- Reuse values across requests
- Dynamic values
```

**3. Pre-Request Scripts**:

```javascript
// Generate timestamp
pm.environment.set("timestamp", new Date().toISOString());

// Generate random email
const randomEmail = `test${Math.floor(Math.random() * 10000)}@example.com`;
pm.environment.set("email", randomEmail);

// Set authorization header
pm.request.headers.add({
  key: "Authorization",
  value: "Bearer " + pm.environment.get("auth_token")
});
```

**4. Tests (Assertions)**:

```javascript
// Test 1: Status code is 201
pm.test("Status code is 201", function () {
  pm.response.to.have.status(201);
});

// Test 2: Response time < 500ms
pm.test("Response time < 500ms", function () {
  pm.expect(pm.response.responseTime).to.be.below(500);
});

// Test 3: Response has JSON body
pm.test("Response is JSON", function () {
  pm.response.to.be.json;
});

// Test 4: Response contains ID
pm.test("Response has id field", function () {
  var jsonData = pm.response.json();
  pm.expect(jsonData).to.have.property("id");
});

// Test 5: Email matches request
pm.test("Email matches", function () {
  var jsonData = pm.response.json();
  pm.expect(jsonData.email).to.eql("test@example.com");
});

// Test 6: Save token for next request
pm.test("Save auth token", function () {
  var jsonData = pm.response.json();
  pm.environment.set("auth_token", jsonData.token);
});

// Test 7: Schema validation
const schema = {
  "type": "object",
  "required": ["id", "email", "name"],
  "properties": {
    "id": {"type": "number"},
    "email": {"type": "string", "format": "email"},
    "name": {"type": "string"}
  }
};

pm.test("Schema is valid", function () {
  pm.expect(tv4.validate(pm.response.json(), schema)).to.be.true;
});
```

#### API Test Cases

**Example API: User Management**

**Base URL**: `https://api.example.com`

**Endpoints**:
```
POST /users - Create user
GET /users/{id} - Get user by ID
GET /users - Get all users
PUT /users/{id} - Update user
DELETE /users/{id} - Delete user
POST /login - User login
```

**Test Cases**:

```
TC_API_001: Create user with valid data
Request:
POST /users
Body: {"email": "test@example.com", "password": "Test@123", "name": "John"}

Expected:
- Status: 201 Created
- Body contains: id, email, name, created_at
- Password NOT in response
- Location header: /users/{id}

TC_API_002: Create user with missing required field
Request:
POST /users
Body: {"email": "test@example.com"} (missing password)

Expected:
- Status: 400 Bad Request
- Error message: "Password is required"

TC_API_003: Create user with invalid email format
Request:
POST /users
Body: {"email": "invalid-email", "password": "Test@123"}

Expected:
- Status: 400 Bad Request
- Error message: "Invalid email format"

TC_API_004: Create user with duplicate email
Request:
POST /users
Body: {"email": "existing@example.com", "password": "Test@123"}

Expected:
- Status: 409 Conflict
- Error message: "Email already exists"

TC_API_005: Get user by valid ID
Request:
GET /users/123

Expected:
- Status: 200 OK
- Body contains: id=123, email, name

TC_API_006: Get user by invalid ID
Request:
GET /users/999999

Expected:
- Status: 404 Not Found
- Error message: "User not found"

TC_API_007: Get all users (pagination)
Request:
GET /users?page=1&limit=10

Expected:
- Status: 200 OK
- Body is array of users (max 10)
- Response contains pagination metadata

TC_API_008: Update user with valid data
Request:
PUT /users/123
Headers: Authorization: Bearer <token>
Body: {"name": "Jane Doe", "email": "jane@example.com"}

Expected:
- Status: 200 OK
- Body contains updated data
- updated_at timestamp changed

TC_API_009: Update user without authorization
Request:
PUT /users/123
(No Authorization header)

Expected:
- Status: 401 Unauthorized
- Error message: "Authentication required"

TC_API_010: Delete user
Request:
DELETE /users/123
Headers: Authorization: Bearer <admin_token>

Expected:
- Status: 204 No Content
- Subsequent GET /users/123 returns 404

TC_API_011: Login with valid credentials
Request:
POST /login
Body: {"email": "test@example.com", "password": "Test@123"}

Expected:
- Status: 200 OK
- Body contains: token, user{id, email, name}
- Token is JWT format

TC_API_012: Login with invalid credentials
Request:
POST /login
Body: {"email": "test@example.com", "password": "wrong"}

Expected:
- Status: 401 Unauthorized
- Error message: "Invalid credentials"

TC_API_013: Response time
Request: GET /users/123

Expected:
- Response time < 500ms

TC_API_014: Concurrent requests
Request: 10 simultaneous GET /users/123

Expected:
- All return 200 OK
- Consistent data
- No errors

TC_API_015: SQL injection prevention
Request:
POST /login
Body: {"email": "test@example.com' OR '1'='1", "password": "any"}

Expected:
- Status: 401 Unauthorized
- No SQL error
- Login fails
```

#### Postman Collection Structure

```
User API Collection
│
├── Environment Setup
│   └── Set Base URL
│
├── Authentication
│   ├── POST Login (Valid)
│   ├── POST Login (Invalid)
│   └── POST Logout
│
├── User Management
│   ├── Positive Tests
│   │   ├── POST Create User
│   │   ├── GET User by ID
│   │   ├── GET All Users
│   │   ├── PUT Update User
│   │   └── DELETE User
│   │
│   └── Negative Tests
│       ├── POST Create User (Missing Field)
│       ├── POST Create User (Invalid Email)
│       ├── POST Create User (Duplicate)
│       ├── GET User (Invalid ID)
│       ├── PUT Update User (Unauthorized)
│       └── DELETE User (Forbidden)
│
└── Performance Tests
    ├── Response Time
    └── Concurrent Requests
```

---

## 5. Test Management Tools

### 5.1 Jira

**Definition**: Jira is a project management and issue tracking tool widely used for test management and defect tracking.

#### Jira Basics

**Issue Types**:

```
1. Epic
   - Large body of work
   - Contains multiple stories
   - Example: "User Authentication Module"

2. Story
   - User requirement
   - Example: "As a user, I want to reset my password"

3. Task
   - Work item
   - Example: "Design database schema for users table"

4. Bug/Defect
   - Issue found during testing
   - Example: "Login button not working on Firefox"

5. Sub-task
   - Breakdown of Story/Task
   - Example: "Write unit tests for login function"
```

**Issue Workflow** (typical):

```
Story Workflow:
TO DO → IN PROGRESS → CODE REVIEW → TESTING → DONE

Bug Workflow:
NEW → ASSIGNED → OPEN → FIXED → RETEST → CLOSED
                                 ↓
                              REOPENED (if fails retest)
```

**Example Bug Report in Jira**:

```
Summary: Login button not clickable on Firefox

Issue Type: Bug

Priority: High

Severity: Critical

Environment:
- Browser: Firefox 115
- OS: Windows 11
- URL: https://qa.example.com/login

Description:
The login button on the login page is not clickable on Firefox.
Users cannot login using Firefox browser.

Steps to Reproduce:
1. Open https://qa.example.com/login in Firefox 115
2. Enter valid username: test@example.com
3. Enter valid password: Test@123
4. Click Login button

Expected Result:
- Login successful
- Redirected to dashboard

Actual Result:
- Button does not respond to click
- No action occurs
- No error message displayed

Attachments:
- screenshot.png
- console_errors.txt
- network_log.har

Links:
- Relates to: STORY-123 "Implement user login"
- Blocks: STORY-456 "Dashboard development"

Labels: login, firefox, P1

Reporter: John Doe (QA)
Assignee: Jane Smith (Developer)
```

#### JQL (Jira Query Language)

**Basic Queries**:

```
# All open bugs
project = "MYPROJECT" AND issuetype = Bug AND status = Open

# High priority bugs assigned to me
assignee = currentUser() AND priority = High AND issuetype = Bug

# Stories in current sprint
project = "MYPROJECT" AND sprint in openSprints()

# Bugs created in last 7 days
created >= -7d AND issuetype = Bug

# Unresolved P1 bugs
priority = "P1" AND resolution = Unresolved AND issuetype = Bug

# Bugs found in testing phase
project = "MYPROJECT" AND status = "In Testing" AND issuetype = Bug

# My assigned tasks
assignee = currentUser() AND issuetype = Task AND status != Done

# Overdue issues
duedate < now() AND resolution = Unresolved

# Bugs by component
component = "Login" AND issuetype = Bug

# Stories without test cases
project = "MYPROJECT" AND issuetype = Story AND "Test Case Count" = 0
```

**Advanced Queries**:

```
# Complex query with multiple conditions
project = "MYPROJECT" AND
issuetype = Bug AND
priority in (Critical, High) AND
status in (Open, "In Progress", Reopened) AND
created >= startOfMonth() AND
component = "Checkout"

# Bugs fixed but not yet retested
status = Fixed AND "Retest Status" = "Not Tested"

# Test execution status
"Test Execution" = "Failed" AND
sprint in openSprints()
```

---

### 5.2 Test Case Management in Excel

While dedicated tools exist, Excel remains popular for small teams and projects.

#### Excel Template Structure

**Columns**:

```
A: Test Case ID (TC_MOD_###)
B: Module/Feature
C: Test Scenario
D: Test Case Title
E: Test Cas Description
F: Preconditions
G: Test Steps
H: Test Data
I: Expected Result
J: Actual Result
K: Status (Pass/Fail/Blocked)
L: Priority (P1/P2/P3)
M: Severity (Critical/High/Medium/Low)
N: Requirement ID
O: Tested By
P: Date Tested
Q: Comments
R: Defect ID (if failed)
```

**Example Test Case**:

```
Test Case ID: TC_LOGIN_001
Module: Authentication
Scenario: Valid Login
Title: Login with valid email and password
Description: Verify user can login with valid credentials

Preconditions:
- User registered with email test@example.com
- Password is Test@123

Test Steps:
1. Navigate to https://example.com/login
2. Enter email: test@example.com
3. Enter password: Test@123
4. Click Login button

Test Data:
Email: test@example.com
Password: Test@123

Expected Result:
1. User logged in successfully
2. Redirected to dashboard
3. Welcome message displayed
4. Logout option available

Actual Result:
As expected

Status: Pass
Priority: P1
Severity: Critical
Requirement ID: REQ-001
Tested By: John Doe
Date: 2026-01-27
Comments: Tested on Chrome, Firefox, Safari - all passed
Defect ID: N/A
```

#### Best Practices

```
1. Consistent Naming Convention
   - TC_MOD_###
   - Example: TC_LOGIN_001, TC_LOGIN_002

2. Clear Steps
   - Numbered steps
   - Action-oriented
   - No ambiguity

3. Specific Test Data
   - Exact values
   - Not "Enter valid email" but "Enter test@example.com"

4. Detailed Expected Results
   - Observable outcomes
   - Measurable criteria
   - No "Should work correctly"

5. Traceability
   - Link to requirements
   - Link to user stories
   - Link to defects

6. Version Control
   - File naming: TestCases_v1.2_2026-01-27.xlsx
   - Change log tab
   - Review history

7. Test Suite Organization
   - Separate sheets by module
   - Summary dashboard
   - Execution tracking
```

#### Excel Formulas for Test Management

```excel
# Count total test cases
=COUNTA(A2:A1000)

# Count passed test cases
=COUNTIF(K2:K1000, "Pass")

# Count failed test cases
=COUNTIF(K2:K1000, "Fail")

# Pass percentage
=COUNTIF(K2:K1000, "Pass")/COUNTA(K2:K1000)*100

# Count P1 test cases
=COUNTIF(L2:L1000, "P1")

# Count test cases by module
=COUNTIF(B2:B1000, "Login")

# Conditional formatting for status
- Green: Pass
- Red: Fail
- Yellow: Blocked

# Count test cases not executed
=COUNTIF(K2:K1000, "")

# Execution percentage
=(COUNTA(K2:K1000)-COUNTBLANK(K2:K1000))/COUNTA(K2:K1000)*100
```

---

### 5.3 Azure DevOps Test Plans

**Definition**: Microsoft's integrated test management solution within Azure DevOps.

#### Key Features

**1. Test Plans**:
```
- Organize test suites
- Track progress
- Manage configurations
- Assign testers
```

**2. Test Suites**:
```
- Static Suite: Manual grouping
- Requirement-based Suite: Linked to requirements
- Query-based Suite: Dynamic based on query
```

**3. Test Configurations**:
```
- OS: Windows, macOS, Linux
- Browser: Chrome, Firefox, Safari
- Environment: QA, Staging
```

**4. Test Execution**:
```
- Run tests
- Update results
- Attach screenshots
- Log bugs
```

**5. Reporting**:
```
- Test progress
- Pass/fail rates
- Charts and dashboards
```

---

## 6. Best Practices

### Testing Best Practices

```
1. Test Early, Test Often
   - Shift-left testing
   - Continuous testing

2. Risk-Based Prioritization
   - Focus on critical features
   - Test high-risk areas first

3. Clear Test Cases
   - Specific steps
   - Expected results
   - Test data included

4. Maintain Traceability
   - Requirements → Test Cases
   - Test Cases → Defects

5. Continuous Learning
   - Learn domain knowledge
   - Stay updated with tools
   - Share knowledge with team

6. Effective Communication
   - Clear bug reports
   - Regular status updates
   - Collaborative approach

7. Automation Where Appropriate
   - Repetitive tests
   - Regression testing
   - API testing

8. Exploratory Testing
   - Don't rely only on scripted tests
   - Think like an end user
   - Find edge cases

9. Test Data Management
   - Separate test data
   - Refresh regularly
   - Realistic data

10. Documentation
    - Test plans
    - Test cases
    - Lessons learned
```

---

## Summary

Phase 2 - Manual Testing Mastery covers:

✅ **Test Planning & Strategy**
- Creating test strategies and plans
- Risk-based testing
- Test estimation techniques
- Entry and exit criteria
- Environment planning

✅ **Advanced Test Design**
- Pairwise testing
- Orthogonal array testing
- Classification tree method
- Combinatorial testing

✅ **Domain Knowledge**
- Banking domain testing
- E-commerce testing
- Healthcare testing
- ERP systems testing

✅ **Specialized Testing Types**
- Accessibility testing (WCAG)
- Localization and internationalization
- Cross-browser testing
- Mobile responsive testing
- Database testing
- API testing with Postman

✅ **Test Management Tools**
- Jira (issue tracking, workflows, JQL)
- Excel test case management
- Azure DevOps Test Plans

**Next Steps**:
- Practice with real applications
- Complete hands-on exercises (Phase2-02)
- Prepare for interviews (Phase2-03)
- Move to Phase 3: Programming Foundation

---

*Phase 2 Complete - Ready for Programming Foundation!*
