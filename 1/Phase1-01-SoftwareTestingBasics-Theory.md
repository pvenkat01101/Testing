# Phase 1 - Software Testing Basics: Theory

## 1. What is Software Testing?

### Definition
Software Testing is a process of evaluating a software application or system to identify defects, verify that it meets specified requirements, and ensure it functions as expected. It involves executing software components using manual or automated tools to check for errors, gaps, or missing requirements.

### Importance of Testing
- **Quality Assurance**: Ensures software meets quality standards
- **Cost Reduction**: Detecting bugs early reduces fix costs significantly
- **Customer Satisfaction**: Delivers reliable, bug-free products
- **Security**: Identifies vulnerabilities before deployment
- **Performance**: Ensures application performs under expected load
- **Compliance**: Meets regulatory and industry standards

### Role of a Tester in SDLC
1. **Requirements Analysis**: Review and understand requirements
2. **Test Planning**: Create test strategy and test plans
3. **Test Design**: Design test cases and test scenarios
4. **Test Execution**: Execute tests and log defects
5. **Defect Tracking**: Monitor and report bug status
6. **Regression Testing**: Verify fixes don't break existing functionality
7. **Sign-off**: Provide test completion reports

### Cost of Bugs in Different Phases

| Phase | Detection Cost | Fix Cost | Cost Multiplier |
|-------|----------------|----------|-----------------|
| Requirements | $100 | $100 | 1x |
| Design | $100 | $500 | 5x |
| Development | $100 | $1,000 | 10x |
| Testing | $100 | $1,500 | 15x |
| Production | $100 | $10,000+ | 100x+ |

**Key Insight**: The later a bug is found, the more expensive it is to fix.

### Quality vs Testing

| Quality | Testing |
|---------|---------|
| Proactive approach | Reactive approach |
| Prevention-oriented | Detection-oriented |
| Process-focused | Product-focused |
| Everyone's responsibility | Tester's primary responsibility |
| Ensures right product is built | Ensures product works correctly |
| Quality Assurance (QA) | Quality Control (QC) |

### Verification vs Validation

| Verification | Validation |
|--------------|------------|
| "Are we building the product right?" | "Are we building the right product?" |
| Static testing (reviews, walkthroughs) | Dynamic testing (executing code) |
| Done without executing code | Requires code execution |
| Finds errors in design/development | Finds defects in actual software |
| Examples: Code reviews, inspections | Examples: Functional testing, UAT |
| Happens earlier in SDLC | Happens later in SDLC |

---

## 2. SDLC Models

### A. Waterfall Model

**Description**: Sequential, linear approach where each phase must be completed before the next begins.

**Phases**:
1. Requirements Gathering
2. System Design
3. Implementation (Coding)
4. Testing
5. Deployment
6. Maintenance

**Advantages**:
- Simple and easy to understand
- Well-defined stages with clear milestones
- Easy to manage due to rigidity
- Works well for small, well-understood projects

**Disadvantages**:
- No working software until late in cycle
- High risk and uncertainty
- Difficult to accommodate changes
- Not suitable for complex projects

**When to Use**:
- Requirements are clear and fixed
- Technology is well understood
- Short projects with no changes expected

### B. V-Model (Validation and Verification Model)

**Description**: Extension of waterfall where testing activities run parallel to development activities.

**Left Side (Development)**:
1. Requirements → Acceptance Test Planning
2. System Design → System Test Planning
3. Architecture Design → Integration Test Planning
4. Module Design → Unit Test Planning
5. Coding

**Right Side (Testing)**:
1. Unit Testing
2. Integration Testing
3. System Testing
4. Acceptance Testing

**Advantages**:
- Testing starts early
- Clear test objectives for each phase
- High success rate
- Good for small to medium projects

**Disadvantages**:
- Still rigid like waterfall
- No working software until late
- Expensive and time-consuming

**When to Use**:
- Requirements are clear
- Small to medium-sized projects
- Safety-critical applications

### C. Agile Methodology

**Description**: Iterative and incremental approach with emphasis on collaboration, flexibility, and customer feedback.

**Core Principles**:
1. Individuals and interactions over processes and tools
2. Working software over comprehensive documentation
3. Customer collaboration over contract negotiation
4. Responding to change over following a plan

**Scrum Framework**:
- **Sprint**: 2-4 week development cycle
- **Sprint Planning**: Plan work for upcoming sprint
- **Daily Standup**: 15-minute sync meeting
- **Sprint Review**: Demo completed work
- **Sprint Retrospective**: Team improvement discussion
- **Roles**: Product Owner, Scrum Master, Development Team

**Kanban Framework**:
- Visualize workflow on board
- Limit work in progress (WIP)
- Continuous delivery
- Focus on flow optimization
- No fixed iterations

**Advantages**:
- Quick delivery of working software
- Flexible to changes
- Customer involvement throughout
- Early defect detection
- Team collaboration

**Disadvantages**:
- Requires experienced team
- Can lose focus without proper discipline
- Documentation may be lacking
- Difficult to predict timelines

**When to Use**:
- Requirements are evolving
- Customer wants to see incremental progress
- Team is experienced and self-organized

### D. DevOps and CI/CD Basics

**DevOps**: Cultural practice combining Development and Operations for faster, reliable delivery.

**Key Practices**:
- Continuous Integration (CI)
- Continuous Delivery/Deployment (CD)
- Infrastructure as Code
- Automated Testing
- Monitoring and Logging

**CI/CD Pipeline**:
1. **Code Commit** → Developers push code to repository
2. **Build** → Code is compiled
3. **Test** → Automated tests run
4. **Deploy** → Application deployed to environment
5. **Monitor** → Application performance monitored

**Testing in DevOps**:
- Shift-left testing (test early)
- Test automation is critical
- Fast feedback loops
- Everyone owns quality

---

## 3. Test Levels

### A. Unit Testing

**Definition**: Testing individual components or functions in isolation.

**Who Performs**: Developers

**Characteristics**:
- Smallest testable parts
- Done during coding phase
- Uses white-box techniques
- Fast execution
- Automated

**Example**: Testing a calculateTotal() function with different inputs

**Tools**: JUnit, TestNG, NUnit, PyTest

### B. Integration Testing

**Definition**: Testing interactions between integrated units/modules.

**Approaches**:

1. **Big Bang**: Integrate all modules at once and test
2. **Top-Down**: Start from top-level modules, use stubs for lower modules
3. **Bottom-Up**: Start from lowest modules, use drivers for higher modules
4. **Sandwich/Hybrid**: Combination of top-down and bottom-up

**Who Performs**: Testers or Developers

**Focus Areas**:
- Data flow between modules
- Interface compatibility
- API interactions
- Database connections

**Example**: Testing if Login module correctly passes data to Dashboard module

### C. System Testing

**Definition**: Testing the complete integrated system as a whole.

**Who Performs**: Independent testing team

**Characteristics**:
- Black-box testing
- End-to-end testing
- Tests against requirements
- Performed in test environment similar to production

**Types**:
- Functional testing
- Performance testing
- Security testing
- Usability testing
- Compatibility testing

**Example**: Testing complete e-commerce website from product search to checkout

### D. Acceptance Testing (UAT)

**Definition**: Validation that system meets business requirements and is ready for delivery.

**Types**:

1. **User Acceptance Testing (UAT)**: Real users test the application
2. **Business Acceptance Testing (BAT)**: Verify business objectives are met
3. **Contract Acceptance Testing**: Verify contractual requirements
4. **Compliance/Regulation Acceptance**: Ensure regulatory compliance
5. **Alpha Testing**: Done by internal employees before release
6. **Beta Testing**: Done by real users in production-like environment

**Who Performs**: End users, customers, business analysts

**Example**: Bank customers testing new mobile banking app before launch

### E. Regression Testing

**Definition**: Re-testing to ensure new changes haven't broken existing functionality.

**When Performed**:
- After bug fixes
- After new feature additions
- After configuration changes
- Before major releases

**Types**:
1. **Corrective Regression**: No changes in specification
2. **Retest-All Regression**: Execute all test cases
3. **Selective Regression**: Select subset of test cases
4. **Progressive Regression**: New test cases for new functionalities
5. **Complete Regression**: When significant changes occur
6. **Partial Regression**: When small changes occur

**Best Practices**:
- Automate regression suites
- Prioritize test cases
- Maintain regression test suite
- Run after every build

### F. Smoke Testing vs Sanity Testing

| Smoke Testing | Sanity Testing |
|---------------|----------------|
| Verifies critical functionalities work | Verifies specific functionality works after changes |
| Done after build is received | Done after bug fixes or small changes |
| Broad and shallow | Narrow and deep |
| Documented or scripted | Usually not documented |
| Can be automated | Usually manual |
| "Is build stable enough for detailed testing?" | "Does the specific feature work correctly?" |
| Done by developers or testers | Done by testers |

**Smoke Test Example**: Can user login, access main page, and logout?

**Sanity Test Example**: After fixing "Add to Cart" bug, verify only that specific feature

---

## 4. Test Types

### A. Functional Testing

**Definition**: Testing software functions as per requirements.

**What is Tested**:
- User interface
- APIs
- Database
- Security
- Client/server communication
- Business logic

**Techniques**:
- Equivalence Partitioning
- Boundary Value Analysis
- Decision Table Testing
- State Transition Testing

**Examples**:
- Login with valid/invalid credentials
- Form submission
- Search functionality
- Navigation between pages

### B. Non-Functional Testing

**Definition**: Testing aspects that don't relate to specific functions but to quality attributes.

**Types**:

1. **Performance Testing**
   - Load Testing: Test under expected load
   - Stress Testing: Test beyond normal load
   - Endurance Testing: Test over extended period
   - Spike Testing: Sudden increase in load
   - Volume Testing: Large amounts of data

2. **Security Testing**
   - Authentication
   - Authorization
   - Data encryption
   - Vulnerability scanning
   - Penetration testing

3. **Usability Testing**
   - User-friendliness
   - Navigation ease
   - Content clarity
   - Accessibility

4. **Compatibility Testing**
   - Browser compatibility
   - Operating system compatibility
   - Device compatibility
   - Database compatibility

5. **Reliability Testing**
   - Application stability
   - Failure recovery
   - Data integrity

### C. Black Box Testing

**Definition**: Testing without knowledge of internal code structure.

**Characteristics**:
- Based on requirements
- Tests functionality
- Tester perspective
- No programming knowledge needed

**Techniques**:
- Equivalence Partitioning
- Boundary Value Analysis
- Decision Table
- State Transition
- Use Case Testing
- Error Guessing

**Example**: Testing login without knowing how authentication code works

### D. White Box Testing

**Definition**: Testing with full knowledge of internal code structure.

**Characteristics**:
- Based on code logic
- Tests code paths
- Developer perspective
- Requires programming knowledge

**Techniques**:
- Statement Coverage
- Branch Coverage
- Path Coverage
- Condition Coverage
- Loop Testing

**Example**: Testing all if-else conditions in a function

### E. Grey Box Testing

**Definition**: Testing with partial knowledge of internal structure.

**Characteristics**:
- Combination of black box and white box
- Tests both functionality and structure
- Access to architecture documents
- Database testing

**Example**: API testing where you know endpoints but not full implementation

---

## 5. Test Design Techniques

### A. Equivalence Partitioning (EP)

**Definition**: Divide input data into valid and invalid partitions where all values in a partition behave similarly.

**Concept**: If one value in partition works, all should work; if one fails, all should fail.

**Example**:
**Requirement**: Age field accepts 18-60

| Partition | Range | Expected Result |
|-----------|-------|-----------------|
| Invalid (below) | < 18 | Error |
| Valid | 18-60 | Accept |
| Invalid (above) | > 60 | Error |

**Test Cases**: Select one value from each partition (e.g., 10, 30, 70)

### B. Boundary Value Analysis (BVA)

**Definition**: Test at boundaries of equivalence partitions.

**Concept**: Errors often occur at boundaries.

**Example**:
**Requirement**: Age field accepts 18-60

**Test Values**:
- Lower boundary: 17, 18, 19
- Upper boundary: 59, 60, 61

**Typically Test**: Min, Min+1, Max-1, Max

### C. Decision Table Testing

**Definition**: Test different combinations of inputs and their corresponding outputs.

**When to Use**:
- Multiple conditions
- Complex business rules
- Combination testing needed

**Example**: Login functionality

| User Exists | Password Correct | Account Active | Result |
|-------------|------------------|----------------|--------|
| Y | Y | Y | Login Success |
| Y | Y | N | Account Locked Message |
| Y | N | Y | Invalid Password |
| Y | N | N | Invalid Password |
| N | - | - | User Not Found |

### D. State Transition Testing

**Definition**: Test system behavior when it transitions between different states.

**When to Use**: Systems with different states based on inputs

**Example**: Online order status

**States**: Placed → Confirmed → Shipped → Delivered → Cancelled

**Transitions**:
- Payment success: Placed → Confirmed
- Item dispatched: Confirmed → Shipped
- Item received: Shipped → Delivered
- Cancel order: Any state → Cancelled (if allowed)

**Test**: Valid transitions, invalid transitions, all possible paths

### E. Use Case Testing

**Definition**: Test based on user scenarios and use cases.

**Components**:
- Actor
- Preconditions
- Main flow
- Alternative flows
- Postconditions

**Example**: Online shopping use case
- Actor: Customer
- Precondition: User logged in
- Main flow: Browse → Add to cart → Checkout → Payment → Confirmation
- Alternative: Remove from cart, Apply coupon
- Postcondition: Order placed

### F. Error Guessing

**Definition**: Use tester's experience to guess potential errors.

**Based On**:
- Past experience
- Common mistakes
- Application knowledge
- Intuition

**Examples**:
- Leaving mandatory fields blank
- Entering special characters
- Very long strings
- SQL injection attempts
- Zero or negative values
- Duplicate entries

### G. Exploratory Testing

**Definition**: Simultaneous learning, test design, and execution without predefined test cases.

**Characteristics**:
- Unscripted testing
- Tester freedom
- Creative approach
- Quick feedback

**When to Use**:
- Time constraints
- Early in development
- Supplement to scripted testing
- Learning new application

**Techniques**:
- Session-based testing
- Time-boxed exploration
- Charter-based testing

---

## 6. Defect/Bug Life Cycle

### What is a Defect?

**Definition**: Deviation of actual result from expected result. Also called bug, issue, or problem.

**Defect vs Error vs Failure**:
- **Error**: Human mistake (incorrect requirement, code mistake)
- **Defect/Bug**: Error found in application
- **Failure**: System unable to perform required function

### Defect States

1. **New**: Tester logs new defect
2. **Assigned**: Assigned to developer by lead
3. **Open**: Developer starts working on it
4. **Fixed**: Developer fixes and marks as fixed
5. **Retest**: Sent back to tester for verification
6. **Verified**: Tester confirms fix works
7. **Closed**: Defect is closed
8. **Reopened**: If issue persists after fix
9. **Rejected**: Not a valid defect
10. **Deferred**: Fix postponed to future release
11. **Duplicate**: Same as another reported defect

### Severity vs Priority

**Severity**: Impact of defect on application functionality

| Level | Description | Example |
|-------|-------------|---------|
| Critical | Application crash, data loss | App crashes on launch |
| High | Major functionality broken | Cannot checkout |
| Medium | Minor functionality broken | Filter not working |
| Low | UI issues, cosmetic | Button color wrong |

**Priority**: Urgency of fixing the defect

| Level | Description | Timeframe |
|-------|-------------|-----------|
| P1 - Immediate | Fix ASAP | Same day |
| P2 - High | Fix in current sprint | 1-2 days |
| P3 - Medium | Fix in next sprint | 1 week |
| P4 - Low | Fix when time permits | Future |

**Examples Combining Both**:

| Scenario | Severity | Priority |
|----------|----------|----------|
| App crashes on payment | Critical | P1 |
| Spelling mistake on homepage | Low | P1 (for release) |
| Rarely used report errors out | High | P3 |
| Optional feature not working | Medium | P4 |

### Writing Effective Bug Reports

**Essential Components**:

1. **Bug ID**: Unique identifier
2. **Summary**: One-line description
3. **Description**: Detailed explanation
4. **Steps to Reproduce**:
   - Step 1: Action
   - Step 2: Action
   - Step 3: Action
5. **Expected Result**: What should happen
6. **Actual Result**: What actually happened
7. **Environment**: OS, browser, version
8. **Attachments**: Screenshots, videos, logs
9. **Severity**: Impact level
10. **Priority**: Urgency level

**Example**:
```
Bug ID: BUG-1234
Summary: Login button not working with Enter key

Description:
User cannot login by pressing Enter key after typing credentials.
Must click Login button with mouse.

Steps to Reproduce:
1. Navigate to www.example.com/login
2. Enter valid username: test@example.com
3. Enter valid password: Test@123
4. Press Enter key

Expected Result: User should be logged in successfully
Actual Result: Nothing happens. Form does not submit.

Environment:
- OS: Windows 11
- Browser: Chrome 120.0
- Build: v2.3.5

Severity: Medium
Priority: P2
Attachments: login_bug.mp4
```

### Defect Metrics

1. **Defect Density**: Defects per size of software
   - Formula: Total defects / Size (KLOC or function points)

2. **Defect Leakage**: Defects found in production
   - Formula: (Production defects / Total defects) × 100

3. **Defect Removal Efficiency**: Defects found before release
   - Formula: (Defects found in testing / Total defects) × 100

4. **Defect Age**: Time to fix a defect
   - From: Date reported
   - To: Date closed

5. **Defect Rejection Ratio**:
   - Formula: (Rejected defects / Total reported) × 100

---

## 7. Test Artifacts

### A. Test Plan Document

**Definition**: Document describing scope, approach, resources, and schedule for testing.

**Contents**:
1. **Test Plan ID**: Unique identifier
2. **Introduction**: Project overview
3. **Scope**: What will be tested, what won't
4. **Test Objectives**: Goals of testing
5. **Test Strategy**: High-level approach
6. **Entry Criteria**: When testing can start
7. **Exit Criteria**: When testing can stop
8. **Test Environment**: Hardware, software, network
9. **Test Schedule**: Timeline
10. **Resources**: People, tools required
11. **Risks**: Potential issues
12. **Deliverables**: What will be produced
13. **Approvals**: Sign-offs

### B. Test Scenarios vs Test Cases

**Test Scenario**: High-level functionality to be tested

**Example**: "User login functionality"

**Test Case**: Detailed steps to test specific condition

**Example**: "Verify login with valid credentials"

| Aspect | Test Scenario | Test Case |
|--------|---------------|-----------|
| Detail Level | High-level | Detailed |
| Purpose | What to test | How to test |
| Derived From | Requirements | Test scenarios |
| Example | "Payment processing" | "Verify payment with valid credit card" |

### C. Test Case Design and Format

**Standard Test Case Format**:

| Field | Description |
|-------|-------------|
| Test Case ID | TC_001 |
| Test Scenario | User authentication |
| Test Case Description | Verify login with valid credentials |
| Preconditions | User registered, app accessible |
| Test Steps | 1. Go to login<br>2. Enter username<br>3. Enter password<br>4. Click login |
| Test Data | user@test.com / Pass@123 |
| Expected Result | User logged in, redirected to dashboard |
| Actual Result | [To be filled during execution] |
| Status | [Pass/Fail/Blocked/Skip] |
| Priority | P1 |
| Created By | Tester name |
| Executed By | [To be filled] |
| Execution Date | [To be filled] |
| Comments | [Any notes] |

### D. Test Data Preparation

**Definition**: Data used to execute test cases.

**Types**:
1. **Valid Data**: Should be accepted
2. **Invalid Data**: Should be rejected
3. **Boundary Data**: At limits
4. **Special Characters**: @, #, $, etc.
5. **Null/Empty**: Blank fields
6. **Large Volume**: Performance testing

**Best Practices**:
- Maintain separate test data repository
- Use data masking for production data
- Version control test data
- Document data dependencies
- Create reusable data sets

### E. Test Summary Report

**Definition**: Document summarizing testing activities and results.

**Contents**:
1. **Report ID and Date**
2. **Project Information**
3. **Testing Scope**
4. **Test Environment**
5. **Test Execution Summary**:
   - Total test cases
   - Executed
   - Passed
   - Failed
   - Blocked
   - Not executed
6. **Defect Summary**:
   - Total defects
   - By severity
   - By priority
   - By status
7. **Test Metrics**:
   - Pass percentage
   - Defect density
   - Test coverage
8. **Risks and Issues**
9. **Recommendations**
10. **Sign-off**

### F. RTM (Requirements Traceability Matrix)

**Definition**: Document mapping requirements to test cases.

**Purpose**:
- Ensure all requirements are tested
- Track requirement coverage
- Impact analysis for changes
- Verify no missing requirements

**Sample RTM**:

| Req ID | Requirement | Test Case ID | Status | Comments |
|--------|-------------|--------------|--------|----------|
| REQ-001 | User can login | TC_001, TC_002, TC_003 | Passed | |
| REQ-002 | User can register | TC_004, TC_005 | In Progress | |
| REQ-003 | Password reset | TC_006 | Failed | Bug-123 |

---

## Summary

Phase 1 covers the fundamental concepts every tester must know:

1. **Testing Basics**: Understanding what testing is and why it matters
2. **SDLC Models**: Different development approaches and when to use them
3. **Test Levels**: Different stages of testing from unit to acceptance
4. **Test Types**: Functional, non-functional, and different box testing approaches
5. **Test Design**: Techniques to create effective test cases
6. **Defect Management**: Bug lifecycle and reporting
7. **Test Artifacts**: Documents and deliverables in testing

**Next Steps**:
- Practice creating test cases using different techniques
- Learn test management tools
- Understand bug tracking systems
- Move to Phase 1.2: Web Fundamentals

---

*Note: This theory forms the foundation for all future testing work. Master these concepts before moving to advanced topics.*
