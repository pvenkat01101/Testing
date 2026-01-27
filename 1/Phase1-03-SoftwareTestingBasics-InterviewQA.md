# Phase 1 - Software Testing Basics: Interview Questions & Answers

## Section 1: Software Testing Fundamentals

### Q1: What is Software Testing? Why is it important?

**Answer**:
Software Testing is a process of evaluating software applications to identify defects, verify functionality, and ensure the product meets specified requirements and quality standards.

**Importance**:
1. **Quality Assurance**: Ensures software meets expected quality
2. **Cost Reduction**: Early bug detection reduces fix costs (bug found in requirements costs $100, same bug in production costs $10,000+)
3. **Customer Satisfaction**: Delivers reliable, bug-free products
4. **Security**: Identifies vulnerabilities before hackers
5. **Performance**: Ensures application handles expected load
6. **Compliance**: Meets legal and regulatory requirements
7. **Brand Reputation**: Poor quality damages company reputation

**Example**: In 2012, Knight Capital lost $440 million in 45 minutes due to a software glitch that wasn't properly tested. This shows the critical importance of thorough testing.

**Interview Tip**: Always mention both technical aspects and business impact.

---

### Q2: What is the difference between Quality Assurance (QA) and Quality Control (QC)?

**Answer**:

| Aspect | Quality Assurance (QA) | Quality Control (QC) |
|--------|------------------------|----------------------|
| **Focus** | Process-oriented | Product-oriented |
| **Approach** | Proactive (Prevention) | Reactive (Detection) |
| **Goal** | Prevent defects | Identify defects |
| **Responsibility** | Everyone's responsibility | Primarily testers |
| **Activities** | Process definition, reviews, audits | Testing, inspection |
| **When** | Throughout SDLC | After product is built |
| **Example** | Code reviews, following standards | Functional testing, bug logging |
| **Type** | Statistical techniques | Testing techniques |

**Real-World Analogy**:
- **QA**: Like having a good recipe and following cooking procedures correctly (process)
- **QC**: Like tasting the food to ensure it's delicious (product)

**Deep Explanation**:
- **QA** ensures we're building the product the right way by establishing processes, standards, and best practices. Example: Having coding standards, conducting design reviews, following Agile ceremonies.
- **QC** ensures the product itself is built correctly by testing and validating. Example: Running test cases, finding bugs, verifying fixes.

**Interview Tip**: Many companies use "QA" to refer to testing roles, but understanding the difference shows maturity.

---

### Q3: What is the difference between Verification and Validation?

**Answer**:

| Verification | Validation |
|--------------|------------|
| **Question** | "Are we building the product right?" | "Are we building the right product?" |
| **Focus** | Process and documents | End product |
| **Method** | Static testing (no code execution) | Dynamic testing (code execution) |
| **When** | Before validation | After verification |
| **Activities** | Reviews, walkthroughs, inspections | Functional testing, system testing, UAT |
| **Finds** | Errors in design and documentation | Defects in actual software |
| **Cost** | Lower cost to fix | Higher cost to fix |

**Example Scenario**: Building a Banking App

**Verification**:
- Reviewing requirement documents: "Does the spec say transfer limit is $10,000?"
- Code reviews: "Is the code implementing the logic correctly?"
- Design reviews: "Does the database schema match requirements?"

**Validation**:
- Testing: "Can user actually transfer $9,000 successfully?"
- UAT: "Do bank employees confirm this meets their needs?"
- System testing: "Does the complete app work as expected?"

**Memory Trick**:
- **Verification** = "Are we building it **right**?" (Internal correctness - reviews)
- **Validation** = "Are we building the **right** thing?" (External correctness - testing)

**IEEE Definition**:
- Verification: "The process of evaluating software to determine whether the products of a given development phase satisfy the conditions imposed at the start of that phase."
- Validation: "The process of evaluating software during or at the end of the development process to determine whether it satisfies specified requirements."

---

### Q4: Explain the cost of fixing defects at different SDLC phases.

**Answer**:

**Rule of 10**: Each phase increases the cost of fixing defects by 10x.

| Phase | Example Defect | Time to Fix | Cost Estimate | Why? |
|-------|----------------|-------------|---------------|------|
| **Requirements** | Incorrect requirement identified | 30 min | $100 | Just update document |
| **Design** | Design flaw found | 2-3 hours | $500 | Revise design, update docs |
| **Development** | Bug in code | 4-5 hours | $1,000 | Fix code, unit test, code review |
| **Testing** | Bug found in QA | 1-2 days | $1,500 | Fix, test, regression test |
| **Production** | Bug in live system | 1-2 weeks | $10,000-$100,000+ | Emergency fix, testing, deployment, customer impact, reputation damage |

**Real-World Examples**:

1. **Ariane 5 Rocket Explosion (1996)**:
   - **Defect**: Integer overflow in code reused from Ariane 4
   - **Cost**: $370 million rocket destroyed
   - **Could have been prevented**: Better requirement analysis and testing

2. **Healthcare.gov Launch (2013)**:
   - **Defect**: Poor performance, crashes, security issues
   - **Cost**: $1.7 billion (additional fixes and reputation damage)
   - **Could have been prevented**: Proper performance and load testing

**Why Cost Increases**:
1. **More people involved**: Fix requires developers, testers, managers, customers
2. **Documentation updates**: All docs (design, test cases, user manuals) need updates
3. **Regression impact**: Need to test everything again
4. **Customer impact**: Refunds, compensation, support costs
5. **Reputation damage**: Lost customers, bad reviews
6. **Legal issues**: Potential lawsuits, fines

**Interview Tip**: Always emphasize "shift-left testing" - test as early as possible to reduce costs.

---

### Q5: What is the role of a Software Tester in SDLC?

**Answer**:

**Comprehensive Role Breakdown**:

**1. Requirements Phase**:
- Review requirements for testability
- Identify ambiguous or missing requirements
- Provide feedback on feasibility
- Start test planning
- **Example**: "Requirement says 'fast response' - need to clarify: how fast? (< 2 seconds?)"

**2. Design Phase**:
- Review design documents
- Identify potential test scenarios
- Suggest testable design improvements
- Prepare test strategy
- **Example**: Suggest adding test hooks in architecture

**3. Development Phase**:
- Create detailed test cases
- Prepare test data
- Set up test environment
- Review code (if white-box testing)
- Develop automation scripts
- **Example**: Create 50+ test cases for login module

**4. Testing Phase**:
- Execute test cases
- Log defects with clear reproduction steps
- Verify bug fixes
- Perform regression testing
- Conduct exploratory testing
- **Example**: Found critical bug where payment processes twice

**5. Deployment Phase**:
- Perform smoke testing on production
- Monitor initial production issues
- Support go-live activities
- **Example**: Verify all critical paths work in production

**6. Maintenance Phase**:
- Test patches and updates
- Update test cases for changes
- Perform regression testing
- **Example**: Test security patch doesn't break existing features

**Key Responsibilities Summary**:
1. ✅ Test Planning and Strategy
2. ✅ Test Case Design and Development
3. ✅ Test Execution
4. ✅ Defect Reporting and Tracking
5. ✅ Test Automation (if required)
6. ✅ Regression Testing
7. ✅ Performance Testing (basic)
8. ✅ Collaboration with developers and stakeholders
9. ✅ Test Metrics and Reporting
10. ✅ Continuous Learning and Improvement

**Skills Required**:
- **Technical**: Testing tools, basic programming, SQL, API testing
- **Analytical**: Problem-solving, attention to detail
- **Communication**: Clear bug reports, stakeholder updates
- **Domain Knowledge**: Understanding of business domain

**Interview Tip**: Emphasize that testers are not just "bug finders" but quality advocates involved throughout SDLC.

---

## Section 2: SDLC Models

### Q6: Explain different SDLC models and when to use each.

**Answer**:

### 1. Waterfall Model

**Description**: Linear, sequential approach where each phase completes before next begins.

**Phases**: Requirements → Design → Implementation → Testing → Deployment → Maintenance

**When to Use**:
- ✅ Requirements are clear and fixed
- ✅ Technology is well understood
- ✅ Short, simple projects
- ✅ Regulatory projects requiring extensive documentation
- **Example**: Developing a simple calculator app with fixed requirements

**Advantages**:
- Simple to understand and manage
- Well-defined stages and milestones
- Easy to measure progress
- Works well for small projects

**Disadvantages**:
- ❌ No working software until late
- ❌ Cannot accommodate changes easily
- ❌ High risk if requirements are misunderstood
- ❌ Testing happens very late

**Real Example**: NASA shuttle software (safety-critical, requirements cannot change)

---

### 2. V-Model (Validation & Verification Model)

**Description**: Extension of waterfall where testing activities parallel development activities.

**Left Side (Development)** → **Right Side (Testing)**:
- Requirements → Acceptance Testing
- System Design → System Testing
- Architecture Design → Integration Testing
- Module Design → Unit Testing
- ↓ Coding ↓

**When to Use**:
- ✅ Requirements are clear and well understood
- ✅ Small to medium projects
- ✅ Where testing needs to start early
- ✅ Safety-critical systems
- **Example**: Medical device software, aviation systems

**Advantages**:
- Testing starts early (test planning during design)
- Clear test objectives for each development phase
- Higher success rate than waterfall
- Defects found early

**Disadvantages**:
- Still rigid like waterfall
- Cannot handle changing requirements
- No prototype until late
- Not good for complex projects

**Real Example**: Automotive embedded systems (e.g., car braking system software)

---

### 3. Agile Methodology

**Description**: Iterative, incremental approach with frequent feedback and flexibility.

**Core Values** (Agile Manifesto):
1. Individuals and interactions > Processes and tools
2. Working software > Comprehensive documentation
3. Customer collaboration > Contract negotiation
4. Responding to change > Following a plan

**Scrum Framework** (Most common Agile implementation):

**Roles**:
- **Product Owner**: Defines what to build (priorities)
- **Scrum Master**: Facilitates process (removes blockers)
- **Development Team**: Builds and tests (cross-functional)

**Ceremonies**:
1. **Sprint Planning** (2-4 hours): Plan sprint work
2. **Daily Standup** (15 min): Yesterday/Today/Blockers
3. **Sprint Review** (1-2 hours): Demo to stakeholders
4. **Sprint Retrospective** (1 hour): Team improvement discussion

**Artifacts**:
- Product Backlog (all requirements)
- Sprint Backlog (sprint work)
- Increment (working software)

**When to Use**:
- ✅ Requirements are evolving
- ✅ Quick time to market needed
- ✅ Customer wants frequent demos
- ✅ Experienced, self-organized team
- **Example**: Startup building new mobile app, web applications

**Testing in Agile**:
- Testing happens in every sprint
- Testers are part of the team
- Automation is critical
- Continuous integration
- Quick feedback loops

**Advantages**:
- Quick delivery of working software
- Flexibility to change requirements
- Customer involvement throughout
- Team collaboration
- Early defect detection

**Disadvantages**:
- Requires experienced team
- Can lose focus without discipline
- Less documentation
- Difficult to predict final timeline/cost

**Real Example**: Spotify, Netflix (continuous feature development)

---

### 4. DevOps

**Description**: Culture and practices combining Development and Operations for continuous delivery.

**Key Principles**:
1. **Continuous Integration (CI)**: Frequent code commits, automated builds
2. **Continuous Delivery (CD)**: Automated deployment to production
3. **Infrastructure as Code**: Manage infrastructure via code
4. **Monitoring and Logging**: Continuous monitoring in production
5. **Collaboration**: Dev and Ops work together

**CI/CD Pipeline**:
```
Code Commit → Build → Unit Test → Integration Test → Deploy to Test →
Automated Tests → Deploy to Staging → Smoke Tests → Deploy to Production →
Monitor
```

**Testing in DevOps**:
- **Shift-left**: Test early and often
- **Test Automation**: Critical for fast feedback
- **Shift-right**: Testing in production (monitoring)
- **Everyone Owns Quality**: Not just testers

**When to Use**:
- ✅ Frequent releases needed
- ✅ Cloud-based applications
- ✅ Microservices architecture
- ✅ Mature testing automation
- **Example**: SaaS products, e-commerce platforms

**Tools**:
- **CI/CD**: Jenkins, GitLab CI, GitHub Actions
- **Containerization**: Docker, Kubernetes
- **Monitoring**: Prometheus, Grafana, New Relic
- **Testing**: Selenium, JUnit, Postman

**Real Example**: Amazon (deploys to production every 11.7 seconds)

---

**Comparison Table**:

| Aspect | Waterfall | V-Model | Agile | DevOps |
|--------|-----------|---------|-------|--------|
| **Flexibility** | Low | Low | High | Very High |
| **Testing** | At end | Parallel to dev | Every sprint | Continuous |
| **Customer Involvement** | Low | Low | High | High |
| **Documentation** | Extensive | Extensive | Minimal | Minimal |
| **Best For** | Simple, fixed requirements | Safety-critical | Evolving requirements | Continuous delivery |
| **Release Cycle** | Months/Years | Months | 2-4 weeks | Days/Hours |

**Interview Tip**: Companies usually don't follow pure models - most use hybrid approaches. Show you understand the trade-offs.

---

### Q7: What is Agile Testing? How is it different from traditional testing?

**Answer**:

**Agile Testing Definition**:
Testing approach that follows Agile principles - continuous, collaborative, and iterative testing throughout the development lifecycle rather than at the end.

**Key Differences**:

| Traditional Testing | Agile Testing |
|---------------------|---------------|
| Testing is a phase after development | Testing happens concurrently with development |
| Separate QA team | Testers are part of development team |
| Extensive documentation | Minimal documentation, focus on working software |
| Requirements fixed upfront | Requirements evolve during development |
| Test plan created before coding | Test strategy adapts each sprint |
| Manual testing preferred | Automation is critical |
| Limited customer involvement | Continuous customer feedback |
| Defects found late | Defects found and fixed quickly |

**Agile Testing Principles**:

1. **Continuous Testing**: Test throughout sprint, not at end
2. **Continuous Feedback**: Quick feedback to developers
3. **Team Collaboration**: Testers work closely with developers
4. **Customer-Centric**: Focus on delivering value
5. **Simplicity**: Test what's important
6. **Respond to Change**: Adapt tests as requirements change
7. **Sustainable Pace**: Avoid burnout
8. **Face-to-Face Communication**: Reduce documentation

**Testing Activities in Agile Sprint**:

**Sprint Planning** (Day 1):
- Understand user stories
- Identify test scenarios
- Estimate testing effort
- Define acceptance criteria

**During Sprint** (Days 2-8):
- Write test cases for user stories
- Develop automation scripts
- Test features as they're developed
- Log and verify bugs immediately
- Regression testing

**Sprint Review** (Day 9):
- Demo tested features to stakeholders
- Get feedback

**Sprint Retrospective** (Day 10):
- Discuss what went well in testing
- Identify improvement areas
- Update testing practices

**Agile Testing Quadrants** (Brian Marick):

```
                Technology-Facing
                       |
        Q1             |              Q2
    Unit Tests        |         Functional Tests
    Component Tests    |         Story Tests
                       |         Prototypes
    -------------------|-------------------
                       |
    Q3                 |              Q4
    Exploratory Tests  |         Performance Tests
    Usability Tests    |         Security Tests
    User Acceptance    |         "ility" Tests
                       |
                Business-Facing
```

**Q1 (Automated, Technology-Facing)**: Support the team (unit, component tests)
**Q2 (Automated, Business-Facing)**: Validate functionality (acceptance tests)
**Q3 (Manual, Business-Facing)**: Critique the product (exploratory, usability)
**Q4 (Tools, Technology-Facing)**: Critique the product (performance, security)

**Challenges in Agile Testing**:
1. ⚠️ Less time for testing (short sprints)
2. ⚠️ Regression testing overhead (need automation)
3. ⚠️ Constantly changing requirements
4. ⚠️ Limited documentation
5. ⚠️ Need for cross-functional skills

**How to Succeed**:
1. ✅ Invest heavily in test automation
2. ✅ Collaborate daily with developers
3. ✅ Prioritize test cases (not everything can be tested)
4. ✅ Use CI/CD pipelines
5. ✅ Learn programming skills
6. ✅ Focus on risk-based testing

**Real-World Example**:

**Traditional Waterfall**:
- 6-month project
- Testing starts in month 5
- 500 bugs found in 1 month
- Major release after 6 months
- High risk of critical bugs

**Agile**:
- 6 months = 12 two-week sprints
- Testing in every sprint
- 40-50 bugs found and fixed per sprint
- Working software delivered every 2 weeks
- Continuous feedback and improvement

**Interview Tip**: Emphasize that Agile testing requires different mindset - from "quality gatekeeper" to "quality advocate" working with the team.

---

## Section 3: Test Levels

### Q8: Explain different levels of testing with examples.

**Answer**:

### 1. Unit Testing

**Definition**: Testing smallest testable parts of application in isolation.

**Who Performs**: Developers

**What is Tested**: Individual functions, methods, classes

**Techniques**: White-box testing

**Example**:
```java
// Function to test
public int add(int a, int b) {
    return a + b;
}

// Unit test
@Test
public void testAdd() {
    Calculator calc = new Calculator();
    assertEquals(5, calc.add(2, 3));
    assertEquals(0, calc.add(-1, 1));
    assertEquals(-5, calc.add(-2, -3));
}
```

**Real Scenario**: Testing calculateDiscount() function
- Input: price = $100, discount = 10%
- Expected: $10
- Tests: Valid values, negative values, zero, boundary values

**Tools**: JUnit, TestNG, NUnit, PyTest, Jest

**Coverage**: 80-90% code coverage expected

---

### 2. Integration Testing

**Definition**: Testing interaction between integrated units/modules.

**Who Performs**: Testers or Developers

**What is Tested**: Data flow between modules, API calls, database connections

**Approaches**:

**a) Big Bang**:
- Integrate all modules at once
- ❌ Difficult to find root cause
- ✅ Quick for small systems

**b) Top-Down**:
- Start with top-level modules
- Use stubs for lower modules
- ✅ Critical modules tested first
- ❌ Need to create stubs

**Example**:
```
Main Module (tested first)
  ↓ calls
Sub-Module 1 (STUB)  Sub-Module 2 (STUB)
```

**c) Bottom-Up**:
- Start with lowest modules
- Use drivers to call them
- ✅ No stubs needed
- ❌ Critical modules tested late

**Example**:
```
DRIVER (simulates main module)
  ↓ calls
Sub-Module 1 (real)  Sub-Module 2 (real)
```

**d) Sandwich/Hybrid**:
- Combination of top-down and bottom-up
- ✅ Best of both approaches

**Real Scenario**: E-commerce Application
- **Unit**: Individual functions tested (calculateTotal, applyDiscount)
- **Integration**: Testing Login → Dashboard → Product Catalog → Add to Cart → Checkout
  - Does logged-in user info pass to dashboard correctly?
  - Does cart data persist when navigating?
  - Does payment module receive correct order total?

**Example Test Cases**:
1. Verify user session maintained across pages
2. Verify cart items saved to database
3. Verify order total passed correctly to payment gateway
4. Verify email service triggered after order placement

**Tools**: JUnit, TestNG, Postman (API integration), SOAP UI

---

### 3. System Testing

**Definition**: Testing complete, integrated system as a whole.

**Who Performs**: Independent QA team

**What is Tested**: End-to-end functionality against requirements

**Techniques**: Black-box testing

**Types of System Testing**:

1. **Functional Testing**: Features work as expected
2. **Performance Testing**: Response time, scalability
3. **Security Testing**: Vulnerabilities, authentication
4. **Usability Testing**: User-friendliness
5. **Compatibility Testing**: Different browsers, OS
6. **Recovery Testing**: System recovers from crashes
7. **Reliability Testing**: System stable over time

**Real Scenario**: Banking Application System Testing

**Functional Tests**:
- User can login, transfer money, pay bills, check balance
- All navigation flows work
- Reports generate correctly

**Performance Tests**:
- 1000 concurrent users can login
- Transaction completes in < 3 seconds
- System handles 10,000 transactions/hour

**Security Tests**:
- SQL injection doesn't work
- Session timeout after inactivity
- Password encrypted in database

**Environment**: Test environment that mimics production

**Entry Criteria**:
- All integration tests passed
- Test environment ready
- Test data prepared

**Exit Criteria**:
- All test cases executed
- No critical/high bugs open
- 95%+ pass rate

**Tools**: Selenium, JMeter, Postman, OWASP ZAP

---

### 4. Acceptance Testing (UAT)

**Definition**: Validation that system meets business needs and is ready for production.

**Who Performs**: End users, business analysts, stakeholders

**What is Tested**: Real-world business scenarios

**Types**:

**a) User Acceptance Testing (UAT)**:
- Real users test in production-like environment
- **Example**: Bank employees test new loan approval system

**b) Business Acceptance Testing (BAT)**:
- Verify business objectives met
- **Example**: System reduces loan processing time from 5 days to 2 days

**c) Contract Acceptance Testing**:
- Verify contractual requirements
- **Example**: Software must handle 10,000 users (as per contract)

**d) Regulation/Compliance Acceptance Testing**:
- Meet legal/regulatory standards
- **Example**: Healthcare app must comply with HIPAA

**e) Alpha Testing**:
- Done by internal employees before release
- In controlled environment

**f) Beta Testing**:
- Done by select external users
- In real production environment
- **Example**: Gmail was in beta for 5 years

**Real Scenario**: Hospital Management System

**UAT Test Cases**:
1. Doctor can view patient history within 5 clicks
2. Nurse can schedule appointments for next 30 days
3. Admin can generate monthly reports
4. System sends appointment reminders 24 hours ahead
5. Prescription can be printed clearly

**Acceptance Criteria Example**:
```
User Story: As a doctor, I want to view patient history quickly

Acceptance Criteria:
✅ Patient history loads within 2 seconds
✅ Shows last 10 visits by default
✅ Can filter by date range
✅ Can search by diagnosis
✅ Allergies highlighted in red
✅ Print functionality available
```

**UAT Process**:
1. Business creates test scenarios based on real usage
2. UAT environment set up with production-like data
3. Users execute tests and provide feedback
4. Issues logged and prioritized
5. Sign-off obtained from stakeholders

**Success Criteria**:
- 100% critical scenarios pass
- Users satisfied with usability
- Performance meets expectations
- Formal sign-off received

---

### 5. Regression Testing

**Definition**: Re-testing to ensure new changes haven't broken existing functionality.

**When Performed**:
- After bug fixes
- After new features added
- After configuration changes
- Before every release

**Scope Decision**:

**Full Regression**: Test everything
- **When**: Major releases, critical changes
- **Time**: Days/Weeks
- ✅ Complete coverage
- ❌ Time-consuming

**Partial Regression**: Test affected areas
- **When**: Minor changes, bug fixes
- **Time**: Hours/Days
- ✅ Faster
- ❌ Might miss issues

**Progressive Regression**: New tests for new features + existing tests
**Corrective Regression**: No spec changes, retest existing

**Regression Test Suite Strategy**:

**Priority 1 (Critical)**: Always run (30% of tests)
- Login, payment, checkout
- Core business flows
- High-risk areas

**Priority 2 (High)**: Run for major releases (40% of tests)
- Important features
- Recently modified areas
- Previously defective modules

**Priority 3 (Medium/Low)**: Run periodically (30% of tests)
- Nice-to-have features
- Rarely used functionality
- Stable modules

**Real Example**:

**Scenario**: E-commerce site adds "Wishlist" feature

**Regression Testing Needed**:
1. Login still works (might affect session handling)
2. Add to cart still works (similar functionality)
3. Checkout still works (database schema might have changed)
4. Search still works (UI layout might have changed)
5. Payment still works (integration points)

**Best Practices**:
1. ✅ Automate regression suites (80%+ automation)
2. ✅ Maintain regression suite (remove obsolete tests)
3. ✅ Run after every build (CI/CD)
4. ✅ Prioritize test cases
5. ✅ Use test management tools

**Tools**: Selenium, Cypress, TestNG, Jenkins (CI/CD)

---

### Q9: What is Smoke Testing vs Sanity Testing?

**Answer**:

### Smoke Testing

**Definition**: Basic tests to verify critical functionalities work and build is stable enough for detailed testing.

**Analogy**: Like checking if car engine starts before driving - if engine doesn't start, no point testing other features.

**Characteristics**:
- ✅ Broad and shallow (covers many features superficially)
- ✅ Done immediately after build deployment
- ✅ Usually automated
- ✅ Documented (formal test cases)
- ✅ Takes 30-60 minutes
- ✅ Performed by developers or testers

**Purpose**: "Is the build stable enough to proceed with detailed testing?"

**Decision**:
- ✅ **Pass**: Proceed with detailed testing
- ❌ **Fail**: Reject build, send back to development

**Example - E-commerce Application**:

| Test | Steps | Time |
|------|-------|------|
| Launch app | Open website | 1 min |
| Login | Enter credentials, click login | 1 min |
| Homepage | Verify homepage loads | 1 min |
| Search | Search for "phone", verify results | 2 min |
| Product page | Click product, verify details page | 1 min |
| Add to cart | Add item, verify cart count increases | 2 min |
| Checkout | Navigate to checkout page | 1 min |
| Logout | Logout successfully | 1 min |

**Total**: ~10 minutes for basic smoke tests

---

### Sanity Testing

**Definition**: Narrow, focused tests to verify specific functionality works correctly after changes.

**Analogy**: Like checking only the repaired brake after fixing - not testing entire car.

**Characteristics**:
- ✅ Narrow and deep (few features but in detail)
- ✅ Done after bug fixes or small changes
- ✅ Usually manual
- ✅ Not documented (unscripted)
- ✅ Takes 15-30 minutes
- ✅ Performed by testers

**Purpose**: "Does the specific changed feature work correctly?"

**Decision**:
- ✅ **Pass**: Proceed with regression testing
- ❌ **Fail**: Reject fix, send back to development

**Example - Bug Fix Verification**:

**Bug**: "Add to Cart" button not working

**After Fix, Sanity Tests**:
1. ✅ Click "Add to Cart" on Product 1 → Added successfully
2. ✅ Click "Add to Cart" on Product 2 → Added successfully
3. ✅ Verify cart shows 2 items
4. ✅ Change quantity in cart → Updates correctly
5. ✅ Remove item from cart → Removes successfully

**Focus**: Only cart-related functionality, not entire application

---

### Comparison Table

| Aspect | Smoke Testing | Sanity Testing |
|--------|---------------|----------------|
| **Objective** | Verify build stability | Verify specific feature after change |
| **Scope** | Broad (many features) | Narrow (one feature/area) |
| **Depth** | Shallow | Deep |
| **When** | After new build/deployment | After bug fix/minor change |
| **Documentation** | Documented | Usually undocumented |
| **Automation** | Automated | Usually manual |
| **Performed By** | Developers or testers | Testers |
| **Time** | 30-60 minutes | 15-30 minutes |
| **Example** | Test all major flows superficially | Test only "Search" feature in detail |
| **Subset Of** | Acceptance testing | Regression testing |

---

**Real-World Scenario**:

**Day 1 Morning**: New build deployed
- **Smoke Test**: Run automated smoke suite (100 tests, 30 min)
- **Result**: 98 pass, 2 fail (Login and Search failed)
- **Action**: Build rejected, sent back to dev

**Day 1 Evening**: Build redeployed with fixes
- **Smoke Test**: Run again
- **Result**: 100 pass
- **Action**: Build accepted, detailed testing begins

**Day 2**: Tester finds bug: "Filter by Price not working"
- Dev fixes and deploys

**Day 2 Evening**: Bug fix deployed
- **Sanity Test**: Test only filter functionality (10 test cases, 15 min)
- **Result**: All filter tests pass
- **Action**: Proceed with regression testing

---

**Interview Tips**:

**Tricky Question**: "Can we perform smoke and sanity testing together?"
**Answer**: Yes! In practice, after a bug fix:
1. First run smoke tests (is build stable?)
2. Then run sanity tests (does the fix work?)
3. Then run regression tests (nothing else broken?)

**Tricky Question**: "Which is more important?"
**Answer**: Both are important at different stages. Smoke testing prevents wasting time on unstable builds. Sanity testing prevents unnecessary regression testing if fix doesn't work.

---

### Practical Example for Interview:

**Interviewer**: "We deployed a new build. Walk me through your testing approach."

**Strong Answer**:
"I would follow this approach:

1. **First - Smoke Testing (30 min)**:
   - Run automated smoke test suite covering critical paths
   - Check application launches, login works, main features accessible
   - Verify database connectivity, API endpoints responding
   - If smoke tests fail → Reject build immediately

2. **Second - Detailed Testing (if smoke passes)**:
   - Execute planned test cases for new features
   - Perform exploratory testing
   - Run sanity tests on recently fixed bugs

3. **Third - Regression Testing**:
   - Run automated regression suite
   - Manually test high-risk areas

4. **Fourth - Sign-off**:
   - If all tests pass, provide go-ahead for release
   - If critical issues found, log bugs and retest after fixes

This approach ensures we catch critical issues early and don't waste time on unstable builds."

---

## Section 4: Test Types & Techniques

### Q10: Explain Equivalence Partitioning with real example.

**Answer**:

**Definition**:
Test design technique where input data is divided into partitions (groups) where all values in a partition are expected to behave similarly. If one value works, all in that partition should work. If one fails, all should fail.

**Purpose**:
- Reduce number of test cases while maintaining coverage
- Test efficiently without testing every possible value
- Based on assumption that system treats values in same partition equally

**Key Principle**:
"Instead of testing 1000 values, identify partitions and test 1 value from each partition."

**Steps to Apply**:
1. Identify input conditions
2. Divide into valid and invalid partitions
3. Select one representative value from each partition
4. Create test cases

---

**Example 1: Age Field Validation**

**Requirement**: Website allows registration for ages 18-65

**Step 1: Identify Partitions**

| Partition | Description | Range | Expected Behavior |
|-----------|-------------|-------|-------------------|
| Invalid - Too Young | Below minimum age | Age < 18 | Reject with error |
| Valid | Within accepted range | 18 ≤ Age ≤ 65 | Accept |
| Invalid - Too Old | Above maximum age | Age > 65 | Reject with error |

**Step 2: Select Test Values**

| Test Case | Input | Partition | Expected Result |
|-----------|-------|-----------|-----------------|
| TC_001 | 10 | Invalid (Too Young) | Error: "Must be 18 or older" |
| TC_002 | 17 | Invalid (Too Young) | Error: "Must be 18 or older" |
| TC_003 | 18 | Valid (Boundary) | Registration successful |
| TC_004 | 35 | Valid (Middle) | Registration successful |
| TC_005 | 65 | Valid (Boundary) | Registration successful |
| TC_006 | 70 | Invalid (Too Old) | Error: "Maximum age is 65" |
| TC_007 | 100 | Invalid (Too Old) | Error: "Maximum age is 65" |

**Why This Works**:
- If age 10 is rejected, ages 5, 15, 17 should also be rejected (same partition)
- If age 35 is accepted, ages 18-65 should also be accepted (same partition)
- If age 70 is rejected, ages 66, 80, 90 should also be rejected (same partition)

**Without Equivalence Partitioning**:
- Would need to test: 1, 2, 3, 4...98, 99, 100 (100 test cases!)

**With Equivalence Partitioning**:
- Only need: 7 test cases (3 partitions, but including boundaries)

---

**Example 2: Discount Code Validation**

**Requirement**:
- Discount code must be exactly 6 alphanumeric characters
- Only uppercase letters (A-Z) and digits (0-9) allowed
- Cannot be all numbers or all letters (must be mixed)
- Must be unique (not already used)

**Partitions**:

| # | Partition | Example Values | Valid? | Expected Result |
|---|-----------|----------------|--------|-----------------|
| 1 | Valid (Mixed alphanumeric, 6 chars) | "ABC123", "X1Y2Z3" | ✅ Valid | Accepted |
| 2 | Invalid (Less than 6 chars) | "ABC12", "A1" | ❌ Invalid | Error: "Must be 6 characters" |
| 3 | Invalid (More than 6 chars) | "ABC1234", "ABCDEFG1" | ❌ Invalid | Error: "Must be 6 characters" |
| 4 | Invalid (All letters) | "ABCDEF" | ❌ Invalid | Error: "Must contain numbers" |
| 5 | Invalid (All numbers) | "123456" | ❌ Invalid | Error: "Must contain letters" |
| 6 | Invalid (Contains lowercase) | "abc123" | ❌ Invalid | Error: "Only uppercase allowed" |
| 7 | Invalid (Contains special chars) | "ABC@12" | ❌ Invalid | Error: "Only letters and numbers" |
| 8 | Invalid (Contains spaces) | "ABC 123" | ❌ Invalid | Error: "No spaces allowed" |
| 9 | Invalid (Already used) | "ABC123" (used) | ❌ Invalid | Error: "Code already used" |
| 10 | Invalid (Empty) | "" | ❌ Invalid | Error: "Code required" |

**Test Cases**:

```
TC_001: Input = "ABC123" (Valid) → Expected: Accepted ✅
TC_002: Input = "X1Y2Z3" (Valid) → Expected: Accepted ✅
TC_003: Input = "ABC12" (< 6 chars) → Expected: Error "Must be 6 characters" ❌
TC_004: Input = "ABC1234" (> 6 chars) → Expected: Error "Must be 6 characters" ❌
TC_005: Input = "ABCDEF" (All letters) → Expected: Error "Must contain numbers" ❌
TC_006: Input = "123456" (All numbers) → Expected: Error "Must contain letters" ❌
TC_007: Input = "abc123" (Lowercase) → Expected: Error "Only uppercase allowed" ❌
TC_008: Input = "ABC@12" (Special char) → Expected: Error "Only letters and numbers" ❌
TC_009: Input = "ABC 12" (Space) → Expected: Error: "No spaces allowed" ❌
TC_010: Input = "" (Empty) → Expected: Error "Code required" ❌
```

---

**Example 3: Order Quantity** (E-commerce)

**Requirement**: User can order 1 to 100 units of a product

**Partitions**:

| Partition | Range | Test Value | Expected |
|-----------|-------|------------|----------|
| Invalid (Zero/Negative) | ≤ 0 | -1, 0 | Error: "Minimum quantity is 1" |
| Valid | 1-100 | 1, 50, 100 | Order successful |
| Invalid (Exceeds limit) | > 100 | 101, 150 | Error: "Maximum quantity is 100" |

**Test Cases**:
```
TC_001: Quantity = -5 → Error ❌
TC_002: Quantity = 0 → Error ❌
TC_003: Quantity = 1 → Success ✅ (Minimum boundary)
TC_004: Quantity = 50 → Success ✅ (Middle value)
TC_005: Quantity = 100 → Success ✅ (Maximum boundary)
TC_006: Quantity = 101 → Error ❌
TC_007: Quantity = 999 → Error ❌
```

---

**Example 4: Password Strength** (Complex Real-World)

**Requirement**:
- Length: 8-20 characters
- Must contain: uppercase, lowercase, digit, special character
- Cannot contain username
- Cannot be same as last 3 passwords

**Valid Partition**:
- Length 8-20, has all required character types, not username, not recent password
- **Example**: "Pass@123word", "MyP@ssw0rd"

**Invalid Partitions**:
1. Too short: "P@ss1" (< 8)
2. Too long: "Pass@123wordverylong123" (> 20)
3. No uppercase: "pass@123word"
4. No lowercase: "PASS@123WORD"
5. No digit: "Pass@password"
6. No special char: "Pass1234word"
7. Contains username (if username = "john"): "John@123pass"
8. Same as previous password: "Pass@123word" (if used before)

**Test Strategy**:
- 1 test from valid partition
- 1 test from each invalid partition (8 tests)
- **Total**: 9 test cases instead of hundreds

---

**Benefits of Equivalence Partitioning**:

1. ✅ **Reduces Test Cases**: Test representative values, not every value
2. ✅ **Increases Coverage**: Ensures all types of inputs tested
3. ✅ **Saves Time**: Fewer tests to execute
4. ✅ **Systematic Approach**: Structured, not random
5. ✅ **Easy to Maintain**: Clear partition logic

**When to Use**:
- Input fields with ranges (age, price, quantity)
- Dropdown selections (country, category)
- Input validation (email, phone, zip code)
- Status fields (New, Active, Inactive, Deleted)

**Interview Tips**:

**Common Question**: "Why not test all values?"
**Answer**: "Testing all values is impractical and inefficient. If age 10 is rejected, age 11, 12, 13 will also be rejected because they're in the same partition. Equivalence partitioning reduces test cases while maintaining coverage by testing representative values from each partition."

**Common Question**: "What if values in same partition behave differently?"
**Answer**: "That would indicate a defect in the system. The assumption is that all values in a partition should be treated identically by the system. If they're not, it's a bug that needs fixing."

---

**Practical Exercise for Interview**:

**Interviewer**: "Design test cases for a flight booking system where passengers can be 0-12 years (child), 13-64 years (adult), 65+ years (senior). Use equivalence partitioning."

**Strong Answer**:

"I'll identify three partitions based on passenger age:

**Partitions**:
1. Invalid (< 0): Age -1 → Error
2. Child (0-12): Ages 0, 6, 12 → Child fare applied
3. Adult (13-64): Ages 13, 30, 64 → Adult fare applied
4. Senior (65+): Ages 65, 75, 85 → Senior fare applied
5. Invalid (unrealistic age): Age 150 → Error

**Test Cases**:
- TC_001: Age = -1 → Error: "Invalid age"
- TC_002: Age = 0 → Child fare (boundary)
- TC_003: Age = 6 → Child fare (middle)
- TC_004: Age = 12 → Child fare (boundary)
- TC_005: Age = 13 → Adult fare (boundary)
- TC_006: Age = 35 → Adult fare (middle)
- TC_007: Age = 64 → Adult fare (boundary)
- TC_008: Age = 65 → Senior fare (boundary)
- TC_009: Age = 80 → Senior fare (middle)
- TC_010: Age = 150 → Error: "Unrealistic age"

I've covered all partitions with 10 test cases instead of testing all possible ages (0-150+)."

---

---

### Q11: Explain Boundary Value Analysis (BVA) with examples.

**Answer**:

**Definition**:
Boundary Value Analysis is a test design technique where test cases are designed using values at boundaries (edges) of equivalence partitions. Errors are most likely to occur at boundaries rather than in the middle of partitions.

**Key Principle**:
"Test at boundaries - if boundary values work correctly, values in between will likely work too."

**BVA Rules**:
For a range [min, max]:
1. **Test minimum boundary**: min - 1, min, min + 1
2. **Test maximum boundary**: max - 1, max, max + 1

**Why Boundaries?**
- Off-by-one errors common in code (using < instead of <=)
- Array index errors (0-based vs 1-based indexing)
- Rounding errors at boundaries
- Database field limits

---

**Example 1: Age Field (18-65)**

**Requirement**: Accept ages 18 to 65

**Boundary Values**:

| Test Case | Age Value | Expected Result | Boundary Type |
|-----------|-----------|-----------------|---------------|
| TC_001 | 17 | Error: "Must be 18 or older" | Min - 1 |
| TC_002 | 18 | Accept | Min (Lower boundary) |
| TC_003 | 19 | Accept | Min + 1 |
| TC_004 | 64 | Accept | Max - 1 |
| TC_005 | 65 | Accept | Max (Upper boundary) |
| TC_006 | 66 | Error: "Maximum age 65" | Max + 1 |

**Code Bug Example**:
```java
// WRONG CODE (common mistake)
if (age > 18 && age < 65) {  // Should be >= and <=
    // Accept
}
// This would reject age 18 and 65!

// CORRECT CODE
if (age >= 18 && age <= 65) {
    // Accept
}
```

**Testing would catch this**: Age 18 and 65 would fail, revealing the bug.

---

**Example 2: Order Quantity (1-100)**

**Requirement**: User can order 1 to 100 items

**Boundary Value Test Cases**:

| TC | Value | Expected | Reason |
|----|-------|----------|--------|
| TC_001 | 0 | Error | Below minimum |
| TC_002 | 1 | Success | Minimum boundary |
| TC_003 | 2 | Success | Just above minimum |
| TC_004 | 99 | Success | Just below maximum |
| TC_005 | 100 | Success | Maximum boundary |
| TC_006 | 101 | Error | Above maximum |

---

**Example 3: Password Length (8-20 characters)**

**Boundary Values**:

| Length | Test Password | Expected | Boundary |
|--------|---------------|----------|----------|
| 7 | Pass@12 | Error: "Min 8 characters" | Min - 1 |
| 8 | Pass@123 | Accept | Min |
| 9 | Pass@1234 | Accept | Min + 1 |
| 19 | Pass@1234567890ABC | Accept | Max - 1 |
| 20 | Pass@1234567890ABCD | Accept | Max |
| 21 | Pass@1234567890ABCDE | Error: "Max 20 characters" | Max + 1 |

---

**Example 4: Discount Percentage (0-100%)**

**Test Cases**:

| Value | Expected | Boundary |
|-------|----------|----------|
| -1 | Error | Below min |
| 0 | Accept (no discount) | Min |
| 1 | Accept | Min + 1 |
| 99 | Accept | Max - 1 |
| 100 | Accept (free) | Max |
| 101 | Error | Above max |

---

**BVA vs Equivalence Partitioning**:

| Aspect | Equivalence Partitioning | Boundary Value Analysis |
|--------|--------------------------|-------------------------|
| **Focus** | Groups of values | Boundary values |
| **Test Values** | One from each partition | Values at edges |
| **Coverage** | Broad | Focused on boundaries |
| **Test Cases** | Fewer | More |
| **When to Use** | General input testing | After EP, focus on boundaries |
| **Example** | Age: 10, 35, 70 | Age: 17, 18, 19, 64, 65, 66 |

**Best Practice**: Use both together!
1. First: Identify partitions (EP)
2. Then: Test boundaries of each partition (BVA)

---

**Real-World Bug Example**:

**Scenario**: E-commerce shipping cost calculator
- Orders $0-$50: $10 shipping
- Orders $50+: Free shipping

**Bug**: Code used `amount > 50` instead of `amount >= 50`
- Order of exactly $50 charged $10 shipping (wrong!)
- BVA test with $50 would catch this bug

**BVA Test Cases**:
- $49: $10 shipping ✅
- $50: Free shipping ✅ (would fail with bug)
- $51: Free shipping ✅

---

**Interview Tips**:

**Tricky Question**: "Why test 3 values (min-1, min, min+1) for each boundary?"
**Strong Answer**:
- min-1: Ensures values below minimum are rejected
- min: Ensures exact boundary value is accepted (catches >= vs > bugs)
- min+1: Ensures values just above boundary are accepted
- This catches off-by-one errors, the most common boundary bugs

**Tricky Question**: "Can you use BVA for non-numeric values?"
**Answer**: Yes! Examples:
- String length: "AB" (min-1), "ABC" (min), "ABCD" (min+1) for 3-10 characters
- Date ranges: Jan 31, Feb 1, Feb 2 for month boundaries
- Array indices: index[-1], [0], [1] and [size-1], [size], [size+1]

---

### Q12: What is Decision Table Testing? When to use it?

**Answer**:

**Definition**:
Decision Table Testing is a systematic approach to test complex business rules with multiple combinations of inputs. It's a table showing all possible combinations of conditions and their corresponding actions/outcomes.

**Structure of Decision Table**:

```
┌─────────────────────────────────────┐
│  Conditions (Inputs)                │
├─────────────────────────────────────┤
│  Rules (Combinations)               │
├─────────────────────────────────────┤
│  Actions/Outcomes (Expected Results)│
└─────────────────────────────────────┘
```

**When to Use**:
- ✅ Multiple conditions affecting outcome
- ✅ Complex business logic
- ✅ Combination testing needed
- ✅ IF-THEN-ELSE logic with multiple conditions
- ✅ Different outputs based on input combinations

---

**Example 1: Login Validation**

**Business Rules**:
- Username must exist
- Password must be correct
- Account must be active

**Decision Table**:

| Rule # | Username Exists? | Password Correct? | Account Active? | Expected Result |
|--------|------------------|-------------------|-----------------|-----------------|
| 1 | Yes | Yes | Yes | Login Success ✅ |
| 2 | Yes | Yes | No | "Account locked" ❌ |
| 3 | Yes | No | Yes | "Invalid password" ❌ |
| 4 | Yes | No | No | "Invalid password" ❌ |
| 5 | No | - | - | "User not found" ❌ |

**Test Cases from Decision Table**:

```
TC_001 (Rule 1):
- Valid user, correct password, active account
- Expected: Login successful

TC_002 (Rule 2):
- Valid user, correct password, inactive account
- Expected: "Account locked" error

TC_003 (Rule 3):
- Valid user, wrong password, active account
- Expected: "Invalid password" error

TC_004 (Rule 5):
- Invalid username
- Expected: "User not found" error
```

---

**Example 2: E-commerce Discount Calculator**

**Business Rules**:
- Member customers get 10% discount
- Orders over $100 get 5% additional discount
- Coupon code adds 15% discount
- Maximum discount is 25%

**Decision Table**:

| Rule | Member? | Order>$100? | Has Coupon? | Base Discount | Additional | Coupon | Total | Applied |
|------|---------|-------------|-------------|---------------|------------|--------|-------|---------|
| 1 | No | No | No | 0% | 0% | 0% | 0% | 0% |
| 2 | Yes | No | No | 10% | 0% | 0% | 10% | 10% |
| 3 | No | Yes | No | 0% | 5% | 0% | 5% | 5% |
| 4 | No | No | Yes | 0% | 0% | 15% | 15% | 15% |
| 5 | Yes | Yes | No | 10% | 5% | 0% | 15% | 15% |
| 6 | Yes | No | Yes | 10% | 0% | 15% | 25% | 25%✓ |
| 7 | No | Yes | Yes | 0% | 5% | 15% | 20% | 20% |
| 8 | Yes | Yes | Yes | 10% | 5% | 15% | 30% | 25%✓ |

✓ = Capped at 25% maximum

**Test Cases**:
- One test case for each rule (8 test cases total)
- Verify calculated discount matches expected

---

**Example 3: Insurance Premium Calculator**

**Factors**:
- Age: <25 (Young), 25-60 (Adult), >60 (Senior)
- Gender: Male, Female
- Smoker: Yes, No

**Decision Table** (Simplified - showing premium multiplier):

| Rule | Age Group | Gender | Smoker | Base Premium | Multiplier | Final Premium |
|------|-----------|--------|--------|--------------|------------|---------------|
| 1 | Young | Male | Yes | $1000 | 2.5x | $2500 |
| 2 | Young | Male | No | $1000 | 1.8x | $1800 |
| 3 | Young | Female | Yes | $1000 | 2.0x | $2000 |
| 4 | Young | Female | No | $1000 | 1.5x | $1500 |
| 5 | Adult | Male | Yes | $1000 | 1.5x | $1500 |
| 6 | Adult | Male | No | $1000 | 1.0x | $1000 |
| 7 | Adult | Female | Yes | $1000 | 1.3x | $1300 |
| 8 | Adult | Female | No | $1000 | 0.9x | $900 |
| 9 | Senior | Male | Yes | $1000 | 2.0x | $2000 |
| 10 | Senior | Male | No | $1000 | 1.5x | $1500 |
| 11 | Senior | Female | Yes | $1000 | 1.7x | $1700 |
| 12 | Senior | Female | No | $1000 | 1.3x | $1300 |

**Total Test Cases**: 12 (one for each rule)

---

**Example 4: ATM Withdrawal**

**Conditions**:
- Sufficient Balance?
- Daily limit not exceeded?
- Card valid?

**Decision Table**:

| Rule | Sufficient Balance | Within Daily Limit | Valid Card | Result |
|------|--------------------|--------------------|------------|--------|
| 1 | Yes | Yes | Yes | Dispense cash ✅ |
| 2 | Yes | Yes | No | Card error ❌ |
| 3 | Yes | No | Yes | Limit exceeded ❌ |
| 4 | Yes | No | No | Card error ❌ |
| 5 | No | Yes | Yes | Insufficient funds ❌ |
| 6 | No | Yes | No | Card error ❌ |
| 7 | No | No | Yes | Insufficient funds ❌ |
| 8 | No | No | No | Card error ❌ |

---

**Advantages of Decision Tables**:
1. ✅ Systematic and complete coverage
2. ✅ Easy to understand complex logic
3. ✅ Identifies missing combinations
4. ✅ Helps find contradictory requirements
5. ✅ Good documentation
6. ✅ Reduces redundant test cases

**Disadvantages**:
1. ❌ Can become very large with many conditions
   - Formula: 2^n rows (n = number of conditions)
   - 5 conditions = 32 rows!
2. ❌ Time-consuming to create
3. ❌ Maintenance overhead when requirements change

**Optimization Techniques**:

**1. Rule Collapsing**: Combine rules with same outcome
```
Original:
Rule 1: Username=Valid, Password=Wrong, Active=Yes → Error
Rule 2: Username=Valid, Password=Wrong, Active=No → Error

Collapsed:
Rule: Username=Valid, Password=Wrong, Active=ANY → Error
```

**2. Eliminate Impossible Combinations**:
```
If username doesn't exist, password and account status don't matter
```

---

**Real-World Application**:

**Banking Transfer Rules**:
- Amount > 0
- Amount <= Balance
- Amount <= Daily Limit ($10,000)
- Recipient account valid
- Not blacklisted

Instead of 2^5 = 32 test cases, use decision table to identify:
- Critical paths (positive scenarios)
- Important negative scenarios
- Optimize to ~10-15 meaningful test cases

---

**Interview Tips**:

**Tricky Question**: "Decision tables create too many test cases. How to reduce?"
**Strong Answer**:
1. Collapse rules with same outcome
2. Eliminate impossible combinations
3. Use pairwise testing for less critical combinations
4. Focus on business-critical paths
5. Risk-based prioritization

**Tricky Question**: "When NOT to use decision tables?"
**Answer**:
- Simple IF-ELSE logic (1-2 conditions)
- When conditions are not independent
- Workflows (use state transition instead)
- Ranges of values (use EP/BVA instead)

---

### Q13: What is State Transition Testing? Explain with example.

**Answer**:

**Definition**:
State Transition Testing is a technique used when system behavior changes based on current state and input (event). It tests all possible states, transitions between states, and verifies correct state changes.

**Key Concepts**:
- **State**: Condition or situation of system at a specific time
- **Transition**: Movement from one state to another
- **Event**: Action or input triggering transition
- **Guard Condition**: Rule that must be true for transition

**When to Use**:
- ✅ System has different states
- ✅ Behavior depends on current state
- ✅ Event-driven systems
- ✅ Workflow applications
- ✅ Finite state machines

---

**Example 1: ATM Card States**

**States**:
1. **Idle**: No card inserted
2. **Card Inserted**: Waiting for PIN
3. **PIN Entered**: Validating
4. **Authenticated**: Ready for transaction
5. **Locked**: Too many wrong attempts

**State Transition Diagram**:

```
          Insert Card
    Idle ───────────→ Card Inserted
                          │
                          │ Enter PIN
                          ↓
                      PIN Entered
                      │         │
          Valid PIN   │         │  Invalid PIN
                      ↓         ↓
                Authenticated  Retry (3 attempts max)
                      │         │
                      │         │ 3rd wrong attempt
                      │         ↓
                      │      Locked
                      │         │
                Remove Card ←───┘
                      │
                      ↓
                    Idle
```

**State Transition Table**:

| Current State | Event | Next State | Action |
|---------------|-------|------------|--------|
| Idle | Insert Card | Card Inserted | Prompt for PIN |
| Card Inserted | Enter PIN | PIN Entered | Validate PIN |
| PIN Entered | Valid PIN | Authenticated | Show menu |
| PIN Entered | Invalid PIN (attempt 1 or 2) | Card Inserted | Show error, retry |
| PIN Entered | Invalid PIN (attempt 3) | Locked | Retain card |
| Authenticated | Transaction Complete | Card Inserted | Return to menu |
| Authenticated | Eject Card | Idle | Return card |
| Locked | - | - | Contact bank |
| Any State | Timeout (30 sec) | Idle | Eject card |

**Test Cases**:

**Valid Transitions** (Positive Tests):
```
TC_001: Idle → Insert Card → Card Inserted
TC_002: Card Inserted → Enter Correct PIN → Authenticated
TC_003: Authenticated → Complete Transaction → Card Inserted
TC_004: Authenticated → Eject Card → Idle
```

**Invalid Transitions** (Negative Tests):
```
TC_005: Try 1 wrong PIN → Retry
TC_006: Try 2 wrong PINs → Retry
TC_007: Try 3 wrong PINs → Locked
TC_008: Timeout from any state → Return to Idle
```

**Invalid State Changes** (Should Not Happen):
```
TC_009: Idle → Authenticated (without card insert) → Should fail
TC_010: Card Inserted → Authenticated (without PIN) → Should fail
TC_011: Locked → Authenticated (without bank reset) → Should fail
```

---

**Example 2: Shopping Cart States**

**States**:
1. **Empty**: No items
2. **Has Items**: Items added
3. **Checkout**: Proceeding to pay
4. **Payment Processing**: Processing payment
5. **Order Confirmed**: Payment successful
6. **Cancelled**: User cancelled

**State Diagram**:

```
Empty ──Add Item──→ Has Items ──Checkout──→ Checkout
  ↑                      │                       │
  │                      │                       │
  │             Remove All Items         Enter Payment
  │                      │                       ↓
  │                      ↓              Payment Processing
  │                    Empty               │          │
  │                                        │          │
  └────────────────────────────────── Cancel ←─────┘
                                          │
                            Payment Success│
                                          ↓
                                 Order Confirmed
```

**Test Cases**:

```
TC_001: Empty → Add Item → Has Items
TC_002: Has Items → Add More → Has Items
TC_003: Has Items → Remove All → Empty
TC_004: Has Items → Checkout → Checkout State
TC_005: Checkout → Enter Payment → Payment Processing
TC_006: Payment Processing → Success → Order Confirmed
TC_007: Payment Processing → Failure → Checkout (retry)
TC_008: Any State → Cancel → Cancelled
TC_009: Cancelled → New Session → Empty
```

---

**Example 3: Bug Life Cycle (State Transition)**

**States**: New → Assigned → Open → Fixed → Retest → Closed

**Transition Table**:

| From | Event | To | Who |
|------|-------|-----|-----|
| New | Assign to Dev | Assigned | Lead |
| Assigned | Developer Starts | Open | Dev |
| Open | Fix Applied | Fixed | Dev |
| Fixed | Send for Retest | Retest | Dev |
| Retest | Fix Verified | Closed | Tester |
| Retest | Still Fails | Reopen | Tester |
| Reopen | Developer Restarts | Open | Dev |
| Any | Not a Bug | Rejected | Anyone |
| Any | Defer to Later | Deferred | Lead |

**Test Scenarios**:

```
1. Happy Path:
   New → Assigned → Open → Fixed → Retest → Closed

2. Reopen Scenario:
   New → Assigned → Open → Fixed → Retest → Reopen → Open → Fixed → Retest → Closed

3. Rejection:
   New → Rejected (Not a bug)

4. Deferment:
   New → Assigned → Deferred (Low priority)
```

---

**Example 4: User Account States**

**States**:
- Not Registered
- Registered (unverified)
- Active
- Suspended
- Locked
- Deleted

**Transitions**:

```
Not Registered ──Register──→ Registered
                                  │
                          Verify Email
                                  ↓
                               Active
                           │    │    │
           Login Attempts  │    │    │  Admin Action
             3+ failed     │    │    │
                           │    │    │
                    ┌──────┘    │    └──────┐
                    ↓            ↓           ↓
                 Locked     Suspended    Deleted
                    │            │
              Unlock by    Reactivate
                Admin         by Admin
                    │            │
                    └────→ Active ←────┘
```

**Coverage Criteria**:

**1. State Coverage**: Visit all states at least once
- Test case for each state (6 test cases minimum)

**2. Transition Coverage** (0-switch): Test all valid transitions
- Test case for each arrow in diagram

**3. All Paths Coverage**: Test all possible paths (expensive!)

**Best Practice**: Focus on:
- All valid transitions
- Common paths
- Critical business flows
- Error handling transitions

---

**State Transition Test Design**:

**Step 1**: Identify all states
**Step 2**: Identify events causing transitions
**Step 3**: Draw state diagram
**Step 4**: Create state table
**Step 5**: Identify invalid transitions
**Step 6**: Design test cases for:
- All valid transitions
- Invalid transitions
- Boundary conditions (first/last state)
- Error paths

---

**Real-World Example: Online Order**

**States**: Placed → Confirmed → Shipped → Delivered → Completed

**Test Scenarios**:
```
Positive:
- Place order → Confirm → Ship → Deliver → Complete

Negative:
- Cancel after placement (Placed → Cancelled)
- Cancel after confirmation (Confirmed → Cancelled)
- Cannot cancel after shipping
- Return after delivery (Delivered → Return Initiated)

Edge Cases:
- Stuck in "Shipped" for > 7 days → Escalate
- Payment failure (Placed → Payment Failed)
```

---

**Interview Tips**:

**Tricky Question**: "How is state transition different from decision table?"
**Answer**:
- **Decision Table**: Tests combinations at one point in time
- **State Transition**: Tests behavior over time and sequence of events
- **Example**: Login rules = Decision Table, Order lifecycle = State Transition

**Tricky Question**: "How many test cases needed for state transition?"
**Answer**: Depends on coverage criteria:
- **Minimum**: All states visited = N test cases (N = number of states)
- **Better**: All transitions = Number of valid transitions
- **Complete**: All paths = Exponential (often infeasible)
- **Practical**: All transitions + critical paths + error paths

---

### Q14: Explain Bug/Defect Life Cycle in detail.

**Answer**:

**Definition**:
Bug Life Cycle (or Defect Life Cycle) is the journey of a defect from discovery through resolution to closure. It defines all states a bug goes through and transitions between those states.

**Complete Bug Life Cycle**:

```
                          ┌──────────┐
                          │   NEW    │
                          └────┬─────┘
                               │ Assigned by Lead
                               ↓
                        ┌──────────────┐
                ┌───────│   ASSIGNED   │
                │       └──────┬───────┘
                │              │ Developer Accepts
                │              ↓
            Rejected      ┌─────────┐
          (Not a Bug)←────│  OPEN   │
                │         └────┬────┘
                │              │ Fix Applied
                │              ↓
                │         ┌─────────┐
                │         │  FIXED  │────→ Duplicate (Already fixed)
                │         └────┬────┘
                │              │ Sent for Retest
                │              ↓
                │         ┌──────────┐
                │    ┌────│  RETEST  │
                │    │    └────┬─────┘
                │    │         │
                │    │     Verified Pass
                │    │         ↓
                │    │    ┌─────────┐
                │    │    │ VERIFIED│────→ Deferred (Fix later)
                │    │    └────┬────┘
                │    │         │ Approved by Lead
                │    │         ↓
                │    │    ┌─────────┐
                │    └───→│ CLOSED  │←────┘
              │          └─────────┘
          Still Fails
                │             │
                └─→ REOPEN ───┘
```

---

**Defect States Explained**:

### 1. NEW
**Description**: Defect just discovered and logged by tester

**Actions**:
- Tester creates bug report
- Adds details (steps, screenshots, environment)
- Sets severity and priority
- Assigns to Test Lead or directly to Dev Lead

**Who Can Set**: Tester

**Example**: "Login button not working - just found and logged"

---

### 2. ASSIGNED
**Description**: Bug assigned to specific developer

**Actions**:
- Test Lead/Dev Lead reviews bug
- Assigns to appropriate developer
- May adjust priority/severity

**Who Can Set**: Test Lead, Dev Lead, Project Manager

**Example**: "Bug assigned to John (Backend Developer)"

---

### 3. OPEN
**Description**: Developer acknowledged and started working on it

**Actions**:
- Developer accepts the bug
- Starts investigating root cause
- Begins coding the fix

**Who Can Set**: Developer

**Example**: "Developer working on fixing login issue"

---

### 4. FIXED
**Description**: Developer completed the fix and code changes

**Actions**:
- Fix implemented
- Code reviewed
- Checked into version control
- Deployed to test environment

**Who Can Set**: Developer

**Example**: "Login bug fixed in build 2.3.5"

---

### 5. RETEST / READY FOR RETEST
**Description**: Build deployed, ready for tester to verify fix

**Actions**:
- Developer marks as fixed
- Notifies tester
- New build available

**Who Can Set**: Developer, Build Engineer

**Example**: "Build 2.3.5 deployed to QA. Please verify BUG-1234"

---

### 6. VERIFIED
**Description**: Tester confirmed fix works correctly

**Actions**:
- Tester retests the bug
- Confirms issue resolved
- Marks as verified

**Who Can Set**: Tester

**Example**: "Verified in build 2.3.5 - login works correctly now"

---

### 7. CLOSED
**Description**: Bug completely resolved and signed off

**Actions**:
- Final approval given
- Bug officially closed
- No further action needed

**Who Can Set**: Test Lead, Project Manager

**Example**: "Bug closed after verification and approval"

---

**Other States**:

### 8. REJECTED / NOT A BUG
**Description**: Not actually a defect

**Reasons**:
- Works as designed
- Duplicate of existing bug
- Not reproducible
- Invalid test data
- Environment issue
- Misunderstood requirement

**Example**:
```
Reported: "System shows error for invalid email"
Status: Rejected (Working as designed - validation is correct)
```

---

### 9. REOPENED
**Description**: Fix didn't work, bug persists

**Reasons**:
- Same issue still occurs
- Fix created new issue
- Works in some scenarios but not others
- Incomplete fix

**Example**:
```
Initial: "Login fails with special characters in password"
Fixed: "Works for some special chars"
Reopened: "Still fails for @ and # symbols"
```

---

### 10. DEFERRED / POSTPONED
**Description**: Fix postponed to future release

**Reasons**:
- Low priority
- No time in current sprint
- Waiting for other dependencies
- Scheduled for next release

**Example**:
```
Bug: "UI misalignment on iPad Mini"
Decision: Low priority, defer to Release 3.0
```

---

### 11. DUPLICATE
**Description**: Same bug already reported

**Actions**:
- Mark as duplicate
- Reference original bug ID
- Close this instance

**Example**:
```
BUG-1234: "Cart total calculation wrong"
BUG-1240: "Cart shows incorrect total" → Duplicate of BUG-1234
```

---

### 12. CANNOT REPRODUCE / NOT REPRODUCIBLE
**Description**: Unable to recreate the issue

**Reasons**:
- Insufficient information
- Environment-specific
- Intermittent issue
- Fixed in meantime

**Actions**:
- Request more details from tester
- If still can't reproduce, close or defer

---

**Example Bug Life Cycle Flows**:

**Scenario 1: Happy Path (Smooth Fix)**
```
1. Tester finds bug → NEW
2. Lead assigns to developer → ASSIGNED
3. Developer starts work → OPEN
4. Developer fixes bug → FIXED
5. Build deployed → RETEST
6. Tester verifies fix works → VERIFIED
7. Lead approves closure → CLOSED

Timeline: 2-3 days
```

**Scenario 2: Bug Reopened**
```
1. Tester finds bug → NEW
2. Lead assigns → ASSIGNED
3. Developer works on it → OPEN
4. Developer claims fixed → FIXED
5. Deployed to QA → RETEST
6. Tester tests → Still fails → REOPENED
7. Goes back to → OPEN
8. Developer fixes properly → FIXED
9. Deployed again → RETEST
10. Tester verifies → VERIFIED
11. Lead approves → CLOSED

Timeline: 5-7 days
```

**Scenario 3: Rejected as Not a Bug**
```
1. Tester reports: "System doesn't allow future dates in Date of Birth"
   → NEW
2. Lead reviews → ASSIGNED to Dev
3. Developer checks: This is validation rule, working correctly
   → REJECTED ("Working as designed")

Timeline: 1 day
```

**Scenario 4: Deferred to Next Release**
```
1. Tester finds: "Export to PDF shows wrong footer on page 50+"
   → NEW
2. Lead reviews: Edge case, low priority → ASSIGNED
3. Team discusses: Not critical for v2.0 release
   → DEFERRED (scheduled for v2.1)

Timeline: Moved to future sprint
```

---

**Defect Metrics Related to Life Cycle**:

**1. Defect Age**:
```
Formula: (Date Closed - Date Opened)
Example: Bug opened Jan 1, closed Jan 5 = 4 days old
Target: < 5 days for high priority, < 10 days for medium
```

**2. Defect Rejection Ratio**:
```
Formula: (Rejected Defects / Total Defects) × 100
Example: 10 rejected out of 100 total = 10%
Target: < 10% (lower is better - means quality bug reporting)
```

**3. Reopen Rate**:
```
Formula: (Reopened Defects / Fixed Defects) × 100
Example: 5 reopened out of 50 fixed = 10%
Target: < 5% (lower is better - means fixes are good)
```

**4. Defect Resolution Time**:
```
Average time from NEW to CLOSED
Target: Depends on priority and severity
```

**5. Defect Density**:
```
Formula: Total Defects / Size (KLOC or Function Points)
Example: 50 defects in 10 KLOC = 5 defects/KLOC
```

---

**Best Practices**:

**1. Clear Workflow**:
- Define who can change each status
- Document approval requirements
- Set SLAs for each state

**2. Communication**:
- Notify stakeholders on status change
- Add comments explaining transitions
- Tag relevant people

**3. Tracking**:
- Monitor bugs stuck in same state
- Track average age per state
- Identify bottlenecks

**4. Regular Reviews**:
- Daily standup: Review open bugs
- Weekly: Review all bugs > 7 days old
- Monthly: Analyze closed bugs and trends

---

**Interview Tips**:

**Tricky Question**: "What if tester and developer disagree if bug is fixed?"
**Strong Answer**:
1. Developer demonstrates fix to tester
2. If still disagreement, involve Test Lead/Tech Lead
3. May need to redefine acceptance criteria
4. If truly not fixed, reopen with detailed evidence
5. Sometimes need Product Owner to clarify expected behavior

**Tricky Question**: "A bug is reopened 3 times. What does this indicate?"
**Answer**: Multiple possibilities:
- Developer not understanding root cause (surface fix)
- Incomplete testing by developer before marking fixed
- Edge cases not considered
- Complex bug needing more investigation
- Communication gap on requirements
- Action: Senior developer should review and pair with original developer

---

### Q15: What is the difference between Severity and Priority?

**Answer**:

**Severity** and **Priority** are two different aspects of a defect that help teams decide how to handle bugs.

**Severity**: **Technical Impact** - How much the bug affects the application functionality

**Priority**: **Business Impact** - How urgently the bug needs to be fixed

---

### Severity Levels

**Critical (S1)**:
- **Definition**: Application crash, data loss, security breach
- **Impact**: System unusable, blocks testing
- **Examples**:
  - Application crashes on startup
  - Payment processing fails
  - Data deleted permanently
  - Security vulnerability exposed
  - Database corruption
- **Who Decides**: Technical team (Developers, Testers)

**High (S2)**:
- **Definition**: Major functionality broken
- **Impact**: Core feature not working, workaround difficult
- **Examples**:
  - Cannot checkout in e-commerce
  - Login fails for all users
  - Search returns no results
  - Email notifications not sent
  - Cannot save changes
- **Who Decides**: Technical team

**Medium (S3)**:
- **Definition**: Moderate functionality impact
- **Impact**: Feature partially working or workaround exists
- **Examples**:
  - Filter not working (manual search possible)
  - Sorting incorrect
  - Minor calculation errors
  - Optional feature broken
  - UI misalignment
- **Who Decides**: Technical team

**Low (S4)**:
- **Definition**: Cosmetic issues, minor inconveniences
- **Impact**: Minimal functionality impact
- **Examples**:
  - Spelling mistakes
  - Wrong color/font
  - Alignment issues
  - Tooltip missing
  - Icons not loading
- **Who Decides**: Technical team

---

### Priority Levels

**P1 - Critical/Immediate**:
- **Definition**: Must fix immediately
- **Timeline**: Same day, ASAP
- **Examples**:
  - Production system down
  - Blocking release
  - Customer-facing critical issue
  - Data breach
- **Who Decides**: Business/Management

**P2 - High**:
- **Definition**: Fix in current sprint/release
- **Timeline**: 1-2 days
- **Examples**:
  - Important feature not working
  - Affecting many users
  - Customer escalation
  - Must fix before release
- **Who Decides**: Product Owner, Project Manager

**P3 - Medium**:
- **Definition**: Fix in next sprint
- **Timeline**: 1-2 weeks
- **Examples**:
  - Minor feature issues
  - Affects some users
  - Workaround available
  - Can wait for next release
- **Who Decides**: Product Owner

**P4 - Low**:
- **Definition**: Fix when time permits
- **Timeline**: Future releases
- **Examples**:
  - Nice-to-have fixes
  - Cosmetic improvements
  - Rarely used features
  - Enhancement requests
- **Who Decides**: Product Owner

---

### Severity vs Priority Comparison

| Aspect | Severity | Priority |
|--------|----------|----------|
| **Meaning** | How bad is the bug? | How soon to fix? |
| **Focus** | Technical impact | Business impact |
| **Perspective** | System/Product | Customer/Business |
| **Decided By** | Technical team | Business/Management |
| **Based On** | Functionality impact | Business urgency |
| **Fixed?** | Usually fixed | Can change |
| **Example** | System crash (Critical) | Must fix today (P1) |

---

### Real-World Examples Combining Both

**Example 1**: **High Severity, Low Priority**

```
Bug: Rare admin report generation fails

Severity: High (Major feature broken)
Priority: P4 (Low)

Reason:
- Technically major functionality broken
- But used by only 2 admins per month
- Workaround: Manual export
- Business impact minimal
- Fix can wait for next release
```

**Example 2**: **Low Severity, High Priority**

```
Bug: Company logo wrong color on homepage

Severity: Low (Cosmetic issue)
Priority: P1 (Critical)

Reason:
- Technically minor UI issue
- But homepage is first impression
- Wrong branding visible to millions
- CEO noticed (management escalation)
- Marketing campaign launching tomorrow
- Must fix before campaign
```

**Example 3**: **High Severity, High Priority**

```
Bug: Payment gateway not processing transactions

Severity: Critical (Core feature broken)
Priority: P1 (Immediate)

Reason:
- Technically critical - no payments
- Business critical - losing revenue
- Customer-facing problem
- Affects all users
- Must fix immediately
```

**Example 4**: **Low Severity, Low Priority**

```
Bug: Typo in FAQ page footer

Severity: Low (Spelling error)
Priority: P4 (Low)

Reason:
- Technically minor
- Business impact minimal
- Rarely noticed
- Can fix anytime
- Not urgent
```

---

### Severity + Priority Matrix

| Severity ↓ Priority → | P1 (Immediate) | P2 (High) | P3 (Medium) | P4 (Low) |
|---|---|---|---|---|
| **Critical (S1)** | **Fix NOW** 🔥<br>Example: Production crash | Fix today<br>Example: Dev crash | Fix this week<br>Example: Test crash | Rare case<br>Example: Edge case crash |
| **High (S2)** | Fix today<br>Example: Login broken before launch | **Fix this sprint**<br>Example: Core feature broken | Fix next sprint<br>Example: Optional feature | Defer<br>Example: Rarely used feature |
| **Medium (S3)** | Fix today<br>Example: Visible UI before launch | Fix this sprint<br>Example: Minor feature issue | **Normal fix cycle**<br>Example: Small bug | Backlog<br>Example: Enhancement |
| **Low (S4)** | Fix today<br>Example: Logo wrong before event | Fix this sprint<br>Example: Branding issue | Fix next sprint<br>Example: Minor UI | **Fix when time**<br>Example: Cosmetic |

---

### How to Decide Severity

**Questions to Ask**:
1. Can users complete their task?
2. Is there a workaround?
3. How many features affected?
4. Does it cause data loss?
5. Is system still usable?

**Decision Tree**:
```
Does it crash/block system?
├── Yes → Critical Severity
└── No ↓

Is core functionality broken?
├── Yes → High Severity
└── No ↓

Does it affect user experience significantly?
├── Yes → Medium Severity
└── No → Low Severity
```

---

### How to Decide Priority

**Questions to Ask**:
1. How many users affected?
2. What's the business impact?
3. Is workaround acceptable?
4. Is it blocking release?
5. Customer escalation?
6. Marketing/Sales impact?

**Decision Tree**:
```
Is production down or blocking release?
├── Yes → P1 (Immediate)
└── No ↓

Is it customer-facing and affecting many users?
├── Yes → P2 (High)
└── No ↓

Can it wait for next sprint?
├── No → P2 (High)
└── Yes ↓

Can it wait for future release?
├── No → P3 (Medium)
└── Yes → P4 (Low)
```

---

### Special Cases

**Case 1**: **Severity and Priority Mismatch**
```
Usually, High Severity = High Priority
But not always!

Examples of Mismatch:
- High Sev, Low Pri: Admin feature broken, rarely used
- Low Sev, High Pri: Logo wrong before product launch
```

**Case 2**: **Priority Changes Over Time**
```
Initially: Bug X = P3 (Medium priority)
Later: Product launch next week
Updated: Bug X = P1 (Visible to customers)

Severity stays same, Priority increased
```

**Case 3**: **Multiple Stakeholders Disagree**
```
Tester: "This is Critical Severity!"
Developer: "It's just Medium"
Resolution:
- Technical Lead decides Severity
- Product Owner decides Priority
- Escalate to Manager if still disputed
```

---

### Interview Tips

**Tricky Question**: "Can priority be higher than severity?"
**Strong Answer**: "Yes, absolutely! Severity is technical impact, Priority is business urgency. A cosmetic bug (low severity) can be high priority if it's visible on homepage during major marketing campaign. Similarly, a rarely-used admin feature crashing (high severity) might be low priority if workaround exists and few users affected."

**Tricky Question**: "Who has final say on severity and priority?"
**Strong Answer**:
- **Severity**: Determined by technical team (testers, developers, tech leads) based on functionality impact
- **Priority**: Determined by business stakeholders (product owners, project managers, executives) based on business needs
- If disagreement, escalate to respective leads

**Tricky Question**: "Give example where you changed priority of a bug."
**Example Answer**: "We had a pagination bug (Medium severity) scheduled for next sprint (P3). But Product Owner informed us that a major client demo was scheduled in 2 days featuring this feature. We escalated priority to P1, fixed it immediately for the demo. After demo, similar bugs remained P3."

---

**Real Interview Scenario**:

**Interviewer**: "Assign severity and priority to these bugs:"

**Bug 1**: "Mobile app crashes when user has 1000+ notifications"
**Answer**:
- **Severity**: High (app crashes = major issue)
- **Priority**: P3 (Medium) - Rare edge case, most users have < 100 notifications, can fix in next sprint

**Bug 2**: "Company name misspelled on login page"
**Answer**:
- **Severity**: Low (cosmetic/spelling error)
- **Priority**: P1 (Immediate) - Brand name visible to all users, affects credibility, very embarrassing

**Bug 3**: "Cannot process refunds in admin panel"
**Answer**:
- **Severity**: Critical (core admin function broken)
- **Priority**: P1 (Immediate) - Customer refunds delayed, legal/compliance issue, business impact

**Bug 4**: "Export to CSV shows date in wrong format"
**Answer**:
- **Severity**: Medium (feature works but output incorrect)
- **Priority**: P2 (High) - Finance team needs it for month-end reports, fix this week

---

This comprehensive understanding of Severity vs Priority is crucial for effective bug management and demonstrates mature testing knowledge in interviews.

---

## Practice Questions for Self-Assessment

1. Your e-commerce site offers free shipping for orders above $50. Identify partitions and create test cases.

2. A login system locks account after 3 failed attempts. Design test cases using equivalence partitioning for password attempts.

3. Grade calculation system: 90-100=A, 80-89=B, 70-79=C, 60-69=D, <60=F. Create partitions and test cases.

---

*Continue practicing with different scenarios to master test design techniques!*

---

[Continuing with Q16-Q30 in next section due to length...]

### Q16: How to write an effective bug report?

**Answer**:

An effective bug report should be clear, complete, and actionable, enabling developers to quickly understand, reproduce, and fix the issue.

**Essential Components**:

**1. Bug ID**: BUG-1234 (auto-generated)

**2. Summary/Title**: One-line clear description
- ❌ Bad: "Button not working"
- ✅ Good: "Login button unresponsive when clicked on Chrome browser"

**3. Description**: Detailed explanation
```
The login button on the homepage does not respond to clicks.
When user clicks the button, nothing happens - no error message,
no loading indicator, no navigation. Button appears enabled but is non-functional.
```

**4. Steps to Reproduce**: Clear, numbered steps
```
1. Navigate to www.example.com
2. Enter valid email: test@example.com
3. Enter valid password: Test@123
4. Click "Login" button
```

**5. Expected Result**: What should happen
```
User should be logged in and redirected to dashboard page
```

**6. Actual Result**: What actually happens
```
Nothing happens. Button doesn't respond, no error, stays on login page
```

**7. Environment Details**:
```
- OS: Windows 11 (Build 22000)
- Browser: Chrome 120.0.6099.129
- Screen Resolution: 1920x1080
- Build Version: v2.3.5
- Test Environment: QA (qa.example.com)
- Date/Time: 2025-01-27 10:30 AM EST
```

**8. Severity & Priority**:
```
Severity: Critical (blocks login)
Priority: P1 (must fix immediately)
```

**9. Attachments**:
- Screenshots with annotations
- Screen recording video
- Console logs
- Network HAR file
- Database state (if relevant)

**10. Additional Information**:
- Works fine in Firefox, Safari
- Only fails in Chrome
- Started after build 2.3.5 deployment
- Related to BUG-1200 (possible regression)

---

**Bug Report Example**:

```markdown
**Bug ID**: BUG-1234

**Summary**: Login button unresponsive on Chrome after entering credentials

**Module**: User Authentication

**Description**:
The login button on the homepage becomes unresponsive after user enters
credentials. The button appears enabled but doesn't respond to clicks.
No error message displayed. Console shows JavaScript error.

**Severity**: Critical
**Priority**: P1
**Reported By**: John Doe (QA Tester)
**Assigned To**: Development Team
**Status**: NEW

**Environment**:
- OS: Windows 11
- Browser: Chrome 120.0
- App Version: v2.3.5
- Environment: QA
- URL: https://qa.example.com/login

**Steps to Reproduce**:
1. Open Chrome browser
2. Navigate to https://qa.example.com/login
3. Enter email: test@example.com
4. Enter password: Test@123
5. Click "Login" button
6. Observe: Button doesn't respond

**Expected Result**:
- Login should be processed
- User redirected to dashboard (/dashboard)
- Session cookie should be set
- Welcome message should display

**Actual Result**:
- Button doesn't respond to click
- No visual feedback
- Remains on login page
- No error message shown

**Console Error**:
```
TypeError: Cannot read property 'submit' of null at HTMLButtonElement.onClick
(login.js:45)
```

**Network Activity**:
- No POST request to /api/login endpoint
- Login API call never triggered

**Workarounds**:
- Works fine in Firefox 121.0
- Works in Chrome when pressing Enter key
- Incognito mode: Same issue

**Additional Notes**:
- Bug introduced in v2.3.5 (worked in v2.3.4)
- Affects all Chrome users
- ~60% of user base uses Chrome
- Blocking production deployment

**Attachments**:
1. screenshot_login_issue.png
2. console_error_video.mp4
3. network_trace.har
4. browser_console.log

**Related Bugs**:
- Might be related to BUG-1200 (button refactoring)
- Similar to BUG-890 (resolved last month)
```

---

**Best Practices**:

**1. Be Specific**:
- ❌ "Search doesn't work"
- ✅ "Search returns 0 results for keyword 'laptop' despite 50 laptop products in database"

**2. One Bug Per Report**:
- Don't combine multiple issues
- Each bug = separate report
- Exception: Related bugs in same flow

**3. Reproducible**:
- Include exact steps
- Provide specific test data
- Mention prerequisites
- ✅ 100% reproducible = best
- ⚠️ Intermittent bugs: mention reproduction rate (e.g., "Fails 3 out of 10 times")

**4. Evidence**:
- Always attach screenshots
- Video for complex flows
- Console logs for JS errors
- Network traces for API issues
- Database queries if relevant

**5. Context**:
- When did it start failing?
- Did it ever work?
- What changed recently?
- Is it environment-specific?

**6. Impact**:
- How many users affected?
- Is workaround available?
- Business impact?
- Customer complaints?

---

**Bug Report Checklist**:

Before submitting:
- [ ] Can I reproduce it consistently?
- [ ] Did I include all steps?
- [ ] Are steps clear enough for others to follow?
- [ ] Did I attach screenshot/video?
- [ ] Did I check console for errors?
- [ ] Did I verify it's not already reported?
- [ ] Did I set correct severity/priority?
- [ ] Did I test in multiple browsers/environments?
- [ ] Did I include all environment details?
- [ ] Is summary clear and specific?

---

**Common Mistakes to Avoid**:

**Mistake 1**: Vague description
```
❌ "App is slow"
✅ "Dashboard loads in 15 seconds (expected < 3 seconds)"
```

**Mistake 2**: Missing steps
```
❌ "Cannot checkout"
✅ "Cannot checkout when applying discount code 'SAVE20'"
    Steps: 1. Add item, 2. Go to cart, 3. Enter 'SAVE20', 4. Click checkout
```

**Mistake 3**: No environment info
```
❌ Missing browser, OS, version
✅ Chrome 120, Windows 11, v2.3.5
```

**Mistake 4**: Assumptions instead of facts
```
❌ "Button probably has wrong CSS class"
✅ "Button doesn't respond. Console shows: TypeError at line 45"
```

**Mistake 5**: Emotional language
```
❌ "Horrible bug! System completely broken! Very bad!"
✅ "Login functionality not working on Chrome. Critical severity."
```

---

**Interview Tips**:

**Tricky Question**: "What makes a bug report high quality?"
**Strong Answer**: "A high-quality bug report has three key characteristics:
1. **Reproducible**: Clear steps that anyone can follow
2. **Complete**: All necessary information provided
3. **Actionable**: Developer can immediately start fixing without asking questions

The goal is to save time - both yours (no back-and-forth clarifications) and developer's (can fix quickly)."

---

### Q17: What is a Test Plan? What does it contain?

**Answer**:

**Definition**:
A Test Plan is a detailed document describing the testing approach, scope, resources, schedule, and activities for a project. It's a strategic blueprint for entire testing effort.

**Purpose**:
- Define WHAT to test
- Define HOW to test
- Define WHO will test
- Define WHEN to test
- Get stakeholder agreement before testing starts

---

**Test Plan Components (IEEE 829 Standard)**:

**1. Test Plan Identifier**:
```
Document ID: TP-ECOM-2025-001
Version: 1.0
Date: January 27, 2025
```

**2. Introduction**:
- Project overview
- Background
- Purpose of document
```
Example:
"This test plan defines the testing approach for the E-commerce
Payment Gateway Integration project scheduled for release on Feb 15, 2025."
```

**3. Test Items / Features to be Tested**:
```
In Scope:
✅ Payment gateway integration
✅ Multiple payment methods (Credit Card, PayPal, Apple Pay)
✅ Payment confirmation emails
✅ Order status updates
✅ Refund processing
✅ Failed payment handling

Out of Scope:
❌ Existing shopping cart (already tested)
❌ Product catalog
❌ User registration
❌ Third-party gateway internal logic
```

**4. Features NOT to be Tested**:
- Explicitly state exclusions
- Reason for exclusion
```
- Legacy admin reports (will be deprecated next month)
- iOS app (separate test plan)
- Manual invoice generation (out of current scope)
```

**5. Testing Approach / Strategy**:

**Test Levels**:
- Unit Testing: Developers (100% code coverage)
- Integration Testing: QA + Developers
- System Testing: QA Team
- UAT: Business users

**Test Types**:
- Functional Testing: All payment flows
- Security Testing: PCI DSS compliance
- Performance Testing: 1000 concurrent transactions
- Compatibility: All major browsers
- Regression Testing: Existing checkout flow

**Automation**:
- API Tests: 100% automated (Postman/RestAssured)
- UI Critical Paths: Automated (Selenium)
- Exploratory Testing: Manual

**6. Pass/Fail Criteria**:
```
Pass Criteria:
✅ All P1 test cases pass
✅ 95% of P2 test cases pass
✅ No critical/high severity bugs open
✅ Performance within SLA (< 3 sec response time)
✅ Security scan passes

Fail Criteria:
❌ Any P1 test case fails
❌ Critical bug found
❌ Payment processing failure rate > 0.1%
❌ Security vulnerability discovered
```

**7. Suspension & Resumption Criteria**:

**Suspend Testing If**:
- Critical bugs block testing
- Test environment unavailable
- Build unstable (smoke tests fail)
- Dependencies not met

**Resume Testing When**:
- Fixes deployed and verified
- Environment restored
- New stable build available
- Dependencies resolved

**8. Test Deliverables**:

**Before Testing**:
- Test Plan (this document)
- Test cases
- Test data
- Test environment setup guide

**During Testing**:
- Test execution status reports (daily)
- Bug reports
- Test logs

**After Testing**:
- Test summary report
- Defect summary
- Test metrics
- Sign-off document

**9. Test Environment**:

**Hardware**:
- Web servers: 2x Linux servers
- Database: MySQL 8.0 cluster
- Load balancer: Nginx

**Software**:
- OS: Ubuntu 22.04
- Application: Node.js 18.x
- Database: MySQL 8.0
- Browser: Chrome 120+, Firefox 121+, Safari 17+

**Test Data**:
- 1000 test user accounts
- 500 test products
- Test credit cards (Stripe test mode)
- Mock payment gateway responses

**Access**:
- QA Environment: https://qa.example.com
- Admin Panel: https://qa.example.com/admin
- Credentials: Provided in separate doc

**10. Schedule & Milestones**:

| Phase | Start Date | End Date | Duration |
|-------|-----------|----------|----------|
| Test Planning | Jan 20 | Jan 24 | 5 days |
| Test Case Design | Jan 25 | Jan 31 | 7 days |
| Test Environment Setup | Feb 1 | Feb 2 | 2 days |
| Test Execution (Sprint 1) | Feb 5 | Feb 9 | 5 days |
| Bug Fixing & Retesting | Feb 10 | Feb 12 | 3 days |
| Regression Testing | Feb 13 | Feb 14 | 2 days |
| UAT | Feb 15 | Feb 16 | 2 days |
| Sign-off | Feb 17 | Feb 17 | 1 day |
| **Total** | **Jan 20** | **Feb 17** | **29 days** |

**11. Staffing & Training**:

**Roles & Responsibilities**:
| Role | Name | Responsibility |
|------|------|----------------|
| Test Manager | Alice | Overall planning, reporting |
| Test Lead | Bob | Test execution, team coordination |
| Senior Tester | Charlie | API testing, automation |
| Tester | Diana | Manual testing |
| Tester | Eve | Regression testing |
| DevOps | Frank | Environment setup |

**Training Needs**:
- Payment gateway API training: 2 days
- Security testing workshop: 1 day
- Automation tool: Already trained

**12. Risks & Mitigation**:

| Risk | Impact | Probability | Mitigation |
|------|--------|-------------|------------|
| Payment gateway sandbox unavailable | High | Medium | Setup mock server |
| Key tester unavailable | Medium | Low | Cross-train team members |
| Environment instability | High | Medium | Daily environment health checks |
| Requirement changes | Medium | High | Agile approach, prioritize |
| Automation delays | Low | Medium | Focus on critical paths |

**13. Tools**:

**Test Management**: Jira + Zephyr
**Bug Tracking**: Jira
**Automation**: Selenium WebDriver + TestNG
**API Testing**: Postman + RestAssured
**Performance**: JMeter
**CI/CD**: Jenkins
**Version Control**: Git
**Documentation**: Confluence

**14. Entry Criteria** (Before testing starts):
- ✅ Requirements finalized and approved
- ✅ Test environment ready and accessible
- ✅ Test data prepared
- ✅ Build deployed to QA
- ✅ Smoke test passed
- ✅ Team trained on new features

**15. Exit Criteria** (Before release):
- ✅ All test cases executed
- ✅ 95%+ pass rate
- ✅ No open critical/high bugs
- ✅ Regression testing complete
- ✅ Performance benchmarks met
- ✅ UAT sign-off received
- ✅ Test summary report approved

**16. Approvals**:

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Test Manager | Alice | _________ | _____ |
| QA Lead | Bob | _________ | _____ |
| Dev Lead | Gary | _________ | _____ |
| Project Manager | Helen | _________ | _____ |

---

**Test Plan Types**:

**1. Master Test Plan**: High-level, covers entire project
**2. Phase Test Plan**: Specific phase (Alpha, Beta, UAT)
**3. Test Type Plan**: Specific type (Performance Test Plan)

---

**Best Practices**:

1. **Keep it concise**: 10-15 pages max
2. **Be realistic**: Don't over-commit
3. **Get buy-in**: Stakeholder approval crucial
4. **Living document**: Update as needed
5. **Visual aids**: Use charts, diagrams
6. **Version control**: Track changes
7. **Regular reviews**: Weekly updates

---

**Interview Tips**:

**Question**: "What if requirements keep changing?"
**Answer**: "Use Agile test planning approach:
- Create high-level master test plan
- Detailed planning per sprint
- Continuous reprioritization
- Focus on risk-based testing
- Maintain flexibility while keeping core structure"

---

### Q18: Difference between Test Scenario and Test Case?

**Answer**:

**Test Scenario** and **Test Case** are both test design artifacts, but differ in detail level and purpose.

---

**Test Scenario**:

**Definition**: High-level description of WHAT to test
- One-liner statement
- Describes functionality to be tested
- No detailed steps
- Derived from requirements

**Example**:
```
Test Scenario: Verify user login functionality
Test Scenario: Validate shopping cart operations
Test Scenario: Check payment processing
```

**Characteristics**:
- High-level
- What to test (not how)
- One scenario → Multiple test cases
- Quick to create
- Used in early test planning

---

**Test Case**:

**Definition**: Detailed description of HOW to test
- Step-by-step instructions
- Includes preconditions, steps, expected results
- Derived from test scenarios
- Executable by anyone

**Example**:
```
Test Case ID: TC_LOGIN_001
Test Scenario: Verify user login functionality
Test Case: Verify login with valid credentials

Preconditions:
- User registered with email: test@example.com
- Password: Test@123

Test Steps:
1. Navigate to www.example.com/login
2. Enter email: test@example.com
3. Enter password: Test@123
4. Click "Login" button

Expected Result:
- User successfully logged in
- Redirected to dashboard page
- Welcome message displays: "Welcome, Test User"
```

---

**Detailed Comparison**:

| Aspect | Test Scenario | Test Case |
|--------|---------------|-----------|
| **Definition** | What to test | How to test |
| **Detail Level** | High-level | Low-level/Detailed |
| **Purpose** | Test planning | Test execution |
| **Derived From** | Requirements | Test scenarios |
| **Created By** | Test Lead/Manager | Test Engineers |
| **When** | Early in cycle | After scenarios approved |
| **Contains** | Functionality description | Step-by-step procedure |
| **Execution** | Cannot be executed | Can be executed |
| **Time to Create** | Quick (minutes) | Time-consuming (hours) |
| **Count** | Few (10-20 scenarios) | Many (100-500 test cases) |
| **Example** | "Verify login" | "Verify login with valid email and password" |

---

**Relationship**:

```
1 Requirement
    ↓
Multiple Test Scenarios
    ↓
Multiple Test Cases per Scenario
```

**Example**:

**Requirement**: User Login Feature

**Test Scenario 1**: Verify user authentication
- TC_001: Login with valid credentials
- TC_002: Login with invalid email
- TC_003: Login with invalid password
- TC_004: Login with empty email
- TC_005: Login with empty password

**Test Scenario 2**: Verify password security
- TC_006: Login with password having <8 characters
- TC_007: Login after 3 failed attempts (account locked)
- TC_008: Password should be masked

**Test Scenario 3**: Verify login session management
- TC_009: Session expires after 30 mins inactivity
- TC_010: Remember me functionality
- TC_011: Logout functionality

```
1 Requirement → 3 Scenarios → 11 Test Cases
```

---

**Real-World Example**: **E-commerce Shopping Cart**

**Test Scenarios** (High-level):
1. Verify adding products to cart
2. Verify updating cart quantities
3. Verify removing items from cart
4. Verify cart persistence
5. Verify cart total calculation
6. Verify empty cart handling

**Test Cases for Scenario 1** (Detailed):

```
TC_CART_001: Add single product to empty cart
- Steps: Search product → Click "Add to Cart" → Verify added
- Expected: Product in cart, count shows "1"

TC_CART_002: Add multiple products to cart
- Steps: Add product A → Add product B → Add product C → Check cart
- Expected: All 3 products visible, count shows "3"

TC_CART_003: Add same product multiple times
- Steps: Add product → Click "Add to Cart" again → Check cart
- Expected: Quantity increases to 2

TC_CART_004: Add product when already 99 items in cart
- Steps: Cart has 99 items → Try adding 100th item
- Expected: Error "Maximum 99 items allowed"

TC_CART_005: Add out-of-stock product
- Steps: Select out-of-stock product → Click "Add to Cart"
- Expected: Error "Product out of stock"
```

So: **1 Scenario → 5+ Detailed Test Cases**

---

**When to Use Each**:

**Use Test Scenarios When**:
- ✅ Initial test planning
- ✅ Estimating test effort
- ✅ Communicating with stakeholders
- ✅ High-level coverage review
- ✅ Time-constrained (Agile sprint planning)

**Use Test Cases When**:
- ✅ Actual test execution
- ✅ Detailed coverage needed
- ✅ Compliance/audit requirements
- ✅ New testers need guidance
- ✅ Automation scripting
- ✅ Regression test suites

---

**Example Document**:

**Test Scenario Document**:
```
Project: E-commerce Application
Module: User Registration

Scenario 1: Verify new user registration
Scenario 2: Verify email validation
Scenario 3: Verify password strength rules
Scenario 4: Verify duplicate email handling
Scenario 5: Verify email verification process
Scenario 6: Verify registration confirmation

Total Scenarios: 6
Estimated Test Cases: 30-35
```

**Test Case Document**:
```
Test Case ID: TC_REG_001
Test Scenario: Verify new user registration
Title: Verify registration with all valid details

Preconditions:
- Email test@example.com not already registered

Priority: P1
Severity: High

Test Steps:
1. Navigate to /register
2. Enter First Name: John
3. Enter Last Name: Doe
4. Enter Email: test@example.com
5. Enter Password: Test@1234
6. Confirm Password: Test@1234
7. Check "Accept Terms" checkbox
8. Click "Register" button

Test Data:
- Email: test@example.com
- Password: Test@1234

Expected Result:
- Registration successful
- Confirmation email sent
- Redirected to email verification page
- Message: "Please check your email"

Actual Result: [To be filled during execution]
Status: [Pass/Fail/Blocked]
Executed By: [Tester name]
Execution Date: [Date]
Comments: [Any observations]
```

---

**Agile Context**:

In Agile:
- **Test Scenarios** often written as **Acceptance Criteria** in User Stories
- **Test Cases** created during sprint, less formal
- Focus on executable tests over documentation

**User Story**:
```
As a user
I want to login to my account
So that I can access my personalized dashboard

Acceptance Criteria (basically test scenarios):
✅ User can login with valid email and password
✅ User sees error with invalid credentials
✅ User account locks after 3 failed attempts
✅ User can reset forgotten password
✅ User remains logged in when "Remember me" checked
```

---

**Interview Tips**:

**Question**: "Can we skip test cases and use only scenarios?"
**Answer**: "Depends on context:
- **Can skip test cases** if: Experienced tester, exploratory testing, time-constrained Agile sprints
- **Need test cases** if: Compliance requirements, new team members, complex flows, automation, formal sign-off needed
- **Best practice**: Start with scenarios, create detailed test cases for critical/complex flows"

**Question**: "How many test cases per scenario?"
**Answer**: "Varies by complexity:
- Simple scenario: 3-5 test cases
- Medium scenario: 5-10 test cases
- Complex scenario: 10-20 test cases
- No fixed rule, depends on combinations and edge cases
- Use test design techniques (EP, BVA) to optimize count"

---

### Q19: What is RTM (Requirements Traceability Matrix)? Why is it important?

**Answer**:

**Definition**:
Requirements Traceability Matrix (RTM) is a document mapping requirements to test cases, ensuring complete test coverage and enabling impact analysis when requirements change.

**Purpose**:
- Ensure ALL requirements are tested
- Track testing progress
- Identify missing test coverage
- Impact analysis for changes
- Demonstrate compliance
- Prevent scope creep

---

**RTM Structure**:

**Basic RTM**:

| Req ID | Requirement | Test Case IDs | Status | Defects | Comments |
|--------|-------------|---------------|--------|---------|----------|
| REQ-001 | User can login | TC_001, TC_002, TC_003 | Passed | - | |
| REQ-002 | User can register | TC_004, TC_005, TC_006 | In Progress | BUG-101 | Email issue |
| REQ-003 | Password reset | TC_007, TC_008 | Failed | BUG-102, BUG-103 | Critical |
| REQ-004 | View profile | TC_009 | Not Tested | - | Pending build |

---

**Detailed RTM (Forward & Backward Traceability)**:

| Business Need | Requirement ID | Requirement Description | Design Spec | Test Case IDs | Test Execution Status | Defect IDs | Coverage % |
|---------------|----------------|-------------------------|-------------|---------------|----------------------|------------|------------|
| User Management | BR-01 | REQ-001 | User authentication system | DS-001 | TC_001, TC_002, TC_003 | Pass, Pass, Pass | - | 100% |
| | | REQ-002 | New user registration | DS-002 | TC_004, TC_005, TC_006 | Pass, Fail, Pass | BUG-101 | 100% |
| Payment | BR-02 | REQ-003 | Process credit card payments | DS-003 | TC_007, TC_008, TC_009 | Pass, Pass, In Progress | - | 66% |

---

**Types of RTM**:

**1. Forward Traceability**:
```
Requirements → Design → Development → Testing

Ensures: Every requirement has corresponding design,
code, and test cases
```

**2. Backward Traceability**:
```
Testing → Development → Design → Requirements

Ensures: Every test case maps back to a requirement
(no unnecessary testing)
```

**3. Bidirectional Traceability**:
```
Forward + Backward traceability
Complete picture of coverage
```

---

**Example: E-commerce RTM**

**Requirements**:

| Req ID | Module | Requirement | Priority | Test Cases | Executed | Passed | Failed | Coverage |
|--------|--------|-------------|----------|------------|----------|--------|--------|----------|
| REQ-001 | Login | User login with email/password | High | TC_001-TC_010 | 10/10 | 9 | 1 | 100% |
| REQ-002 | Login | Remember me functionality | Medium | TC_011-TC_012 | 2/2 | 2 | 0 | 100% |
| REQ-003 | Login | Password reset | High | TC_013-TC_018 | 6/6 | 5 | 1 | 100% |
| REQ-004 | Cart | Add product to cart | Critical | TC_019-TC_025 | 7/7 | 7 | 0 | 100% |
| REQ-005 | Cart | Update cart quantity | High | TC_026-TC_030 | 5/5 | 5 | 0 | 100% |
| REQ-006 | Cart | Remove from cart | High | TC_031-TC_033 | 3/3 | 3 | 0 | 100% |
| REQ-007 | Payment | Credit card payment | Critical | TC_034-TC_045 | 10/12 | 8 | 2 | 83% |
| REQ-008 | Payment | PayPal payment | High | TC_046-TC_052 | 0/7 | - | - | 0% |

**Summary**:
- Total Requirements: 8
- Total Test Cases: 52
- Executed: 43/52 (83%)
- Passed: 39/43 (91%)
- Failed: 4/43 (9%)
- Average Coverage: 85%

**Issues Identified**:
- ⚠️ REQ-007: 2 test cases failed (payment issues)
- ⚠️ REQ-008: Not tested yet (PayPal integration pending)
- ⚠️ REQ-007: Only 83% coverage (missing test cases)

---

**Benefits of RTM**:

**1. Complete Coverage**:
```
Every requirement has test cases
No requirements missed
```

**2. Impact Analysis**:
```
Requirement REQ-003 changed → Which test cases affected?
Look up RTM → TC_013-TC_018 need updating
```

**3. Progress Tracking**:
```
How much testing complete?
RTM shows: 43/52 test cases executed (83%)
```

**4. Gap Identification**:
```
REQ-008 has 0% test coverage → Alert!
Need to create/execute test cases
```

**5. Compliance & Audit**:
```
Auditor asks: "Did you test security requirement SR-005?"
RTM shows: Yes, TC_089, TC_090, TC_091 - All Passed
```

**6. Resource Planning**:
```
Count untested requirements
Estimate effort needed
Allocate testers
```

---

**Real-World Scenario**: **Banking Application**

**Requirement Change Impact**:

```
Original REQ-015: "Transfer limit $5,000 per day"
Updated REQ-015: "Transfer limit $10,000 per day"

RTM shows REQ-015 mapped to:
- TC_150, TC_151, TC_152, TC_153, TC_154

Action Needed:
1. Update test cases with new limit
2. Update test data
3. Re-execute all 5 test cases
4. Update automation scripts

Without RTM: Would need to manually search all test cases!
With RTM: Instantly know which tests affected
```

---

**RTM Maintenance**:

**When to Update**:
- New requirement added → Add row
- Requirement changed → Update affected test cases
- Requirement removed → Mark as obsolete
- Test case added → Update mapping
- Test executed → Update status
- Defect found → Add defect ID

**Who Maintains**:
- Test Lead: Overall ownership
- Testers: Update execution status
- Business Analyst: Requirement changes

**Frequency**:
- Daily: Execution status updates
- Weekly: Coverage review
- Per Sprint/Release: Complete review

---

**RTM Metrics**:

**1. Requirement Coverage**:
```
Formula: (Requirements with test cases / Total requirements) × 100
Example: 48/50 requirements covered = 96%
Target: 100%
```

**2. Test Execution Progress**:
```
Formula: (Test cases executed / Total test cases) × 100
Example: 200/250 executed = 80%
```

**3. Test Pass Rate**:
```
Formula: (Passed test cases / Executed test cases) × 100
Example: 180/200 passed = 90%
```

**4. Defect Density per Requirement**:
```
Formula: Defects found / Requirement
Example: REQ-007 has 3 defects (high-risk area)
```

---

**RTM in Agile**:

In Agile, RTM is lighter:
- User Story ↔ Acceptance Criteria ↔ Test Cases
- Maintained in Jira/Azure DevOps
- Links instead of formal matrix
- Continuous update

**Example in Jira**:
```
User Story: USER-101
Acceptance Criteria:
  ✅ AC-1 (tested by TC-001, TC-002)
  ✅ AC-2 (tested by TC-003)
  ⚠️ AC-3 (no test cases yet)

Status: 66% tested
```

---

**Sample RTM Template** (Excel/Google Sheets):

**Sheet 1: Requirements**
- Columns: Req ID, Module, Description, Priority, Assigned To, Status

**Sheet 2: Test Cases**
- Columns: TC ID, Title, Req ID, Priority, Status

**Sheet 3: Traceability Matrix**
- Requirements (rows) × Test Cases (columns)
- Cell: ✓ if mapped

**Sheet 4: Dashboard**
- Coverage charts
- Execution progress
- Defect distribution

---

**Interview Tips**:

**Question**: "Is RTM necessary in all projects?"
**Answer**: "Depends on:
- **Large/complex projects**: Essential for coverage tracking
- **Regulated industries**: Required for compliance (medical, finance)
- **Small projects**: May use lightweight tracing in test management tool
- **Agile**: Use tool-based linking instead of formal RTM
- **Key point**: Concept of traceability is always important, format can vary"

**Question**: "What if requirement has no test cases?"
**Answer**: "Red flag! Either:
1. Test cases not created yet (action: create them)
2. Requirement not testable (action: clarify with BA)
3. Requirement removed but not updated (action: mark obsolete)
4. Coverage gap (action: immediate attention needed)

During testing, RTM must show 100% requirement coverage before release."

---

### Q20: What are Entry and Exit Criteria in testing?

**Answer**:

**Entry Criteria** and **Exit Criteria** are checkpoints defining when testing can start and when it can be considered complete.

---

**Entry Criteria**:

**Definition**: Conditions that must be met BEFORE testing begins

**Purpose**:
- Ensure readiness to test
- Prevent premature testing
- Set clear starting point
- Avoid wasted effort

**Common Entry Criteria**:

**1. Documentation**:
- ✅ Requirements finalized and approved
- ✅ Test plan reviewed and signed off
- ✅ Test cases created and reviewed
- ✅ Test data prepared

**2. Environment**:
- ✅ Test environment set up and accessible
- ✅ Application deployed to test environment
- ✅ Database restored with test data
- ✅ Network connectivity verified
- ✅ All required tools installed

**3. Build/Application**:
- ✅ Build received from development team
- ✅ Build deployed successfully
- ✅ Smoke tests passed
- ✅ No critical blockers from previous build

**4. Resources**:
- ✅ Testing team available and trained
- ✅ Access permissions granted
- ✅ Test accounts created
- ✅ Required hardware/devices available

**5. Dependencies**:
- ✅ Third-party APIs accessible
- ✅ Integrated systems available
- ✅ Test data seeded in database

---

**Exit Criteria**:

**Definition**: Conditions that must be met BEFORE testing stops and release is approved

**Purpose**:
- Ensure adequate testing done
- Quality gates passed
- Clear completion point
- Prevent premature release

**Common Exit Criteria**:

**1. Test Execution**:
- ✅ All planned test cases executed
- ✅ Minimum 95% test cases passed
- ✅ All critical and high priority test cases passed
- ✅ Regression testing completed

**2. Defect Status**:
- ✅ No open critical severity bugs
- ✅ No open high severity bugs
- ✅ All P1/P2 defects resolved and verified
- ✅ Remaining open defects have approved deferral

**3. Coverage**:
- ✅ 100% requirements covered
- ✅ All business-critical scenarios tested
- ✅ All user acceptance tests passed

**4. Quality Metrics**:
- ✅ Defect density within acceptable limits
- ✅ Test pass rate ≥ 95%
- ✅ No new defects found in last 2 regression cycles
- ✅ Performance benchmarks met

**5. Documentation**:
- ✅ Test summary report prepared
- ✅ Known issues documented
- ✅ Sign-off obtained from stakeholders

---

**Example: E-commerce Application**

**Entry Criteria**:

**Documentation Ready**:
```
✅ Requirements document v2.0 signed off
✅ Test plan approved by QA Manager
✅ 150 test cases created and peer-reviewed
✅ Test data sheet prepared with 50 user accounts
```

**Environment Ready**:
```
✅ QA environment: qa.ecommerce.com accessible
✅ Build 2.5.0 deployed to QA
✅ Database restored with test data
✅ Payment gateway sandbox configured
✅ Email server test mode active
```

**Build Ready**:
```
✅ Build 2.5.0 passed development smoke tests
✅ No P1/P2 defects from build 2.4.0
✅ Unit test pass rate: 98%
✅ Code review completed
```

**Team Ready**:
```
✅ 3 testers available full-time
✅ Team trained on new payment gateway
✅ Test management tool (Jira) access granted
✅ Selenium automation scripts updated
```

**Decision**: All entry criteria met ✅ → Start testing on Jan 27, 2025

---

**Exit Criteria**:

**Test Execution Complete**:
```
✅ 150/150 test cases executed (100%)
✅ 145/150 test cases passed (96.7%)
✅ 5 test cases failed (fixed and retested)
✅ Regression suite passed: 98/100 tests
✅ Exploratory testing: 10 hours completed
```

**Defects Resolved**:
```
✅ Critical bugs: 2 found, 2 fixed, 2 verified
✅ High bugs: 5 found, 5 fixed, 5 verified
✅ Medium bugs: 8 found, 6 fixed, 2 deferred to v2.6
✅ Low bugs: 12 found, 8 fixed, 4 deferred
✅ Total: 27 found, 21 fixed, 6 deferred (all low priority)
```

**Coverage**:
```
✅ Requirement coverage: 100% (all 45 requirements)
✅ Code coverage: 85% (unit tests)
✅ Business-critical paths: 100% tested
✅ Payment flows: Tested with all payment methods
✅ Browser compatibility: Chrome, Firefox, Safari tested
```

**Quality Metrics**:
```
✅ Defect density: 0.6 defects/requirement (acceptable < 1)
✅ Pass rate: 96.7% (target ≥ 95%)
✅ No defects in last regression cycle
✅ Performance: Page load < 2 seconds (target < 3 sec)
✅ API response time: < 500ms (target < 1 sec)
```

**Approvals**:
```
✅ Test summary report submitted
✅ QA Manager sign-off: Approved
✅ Dev Lead sign-off: Approved
✅ Product Owner sign-off: Approved
✅ Known issues documented for production support
```

**Decision**: All exit criteria met ✅ → Approve for release

---

**Scenario: Exit Criteria NOT Met**

**Situation**:
```
❌ Test execution: 140/150 completed (93%)
❌ Open bugs: 1 critical, 3 high severity
❌ Pass rate: 88% (below 95% threshold)
❌ Performance: Some pages loading in 5 seconds
```

**Decision**: Do NOT release
**Actions**:
```
1. Fix critical and high bugs
2. Complete pending 10 test cases
3. Retest failed scenarios
4. Investigate and fix performance issues
5. Re-evaluate exit criteria after fixes
```

---

**Suspension vs Exit Criteria**:

**Suspension Criteria**: When to STOP testing temporarily
```
⚠️ Critical bug blocks further testing
⚠️ Test environment down for > 4 hours
⚠️ Build too unstable (smoke tests fail repeatedly)
⚠️ Major requirement change (need test case updates)

Action: Suspend testing, resume when criteria met
```

**Exit Criteria**: When testing is COMPLETE (success)
```
✅ All tests done
✅ Quality goals met
✅ Ready for release
```

---

**Entry/Exit Criteria by Test Level**:

**Unit Testing**:
```
Entry:
- Code complete for unit
- Unit test framework set up

Exit:
- All unit tests pass
- Code coverage ≥ 80%
- Code review passed
```

**Integration Testing**:
```
Entry:
- Unit testing complete
- Integration environment ready
- Interfaces documented

Exit:
- All integration points tested
- No high severity integration bugs
- Data flow verified
```

**System Testing**:
```
Entry:
- Integration testing complete
- System test environment ready
- Test cases reviewed

Exit:
- All test cases executed
- Critical scenarios passed
- Performance acceptable
```

**UAT (User Acceptance Testing)**:
```
Entry:
- System testing complete
- UAT environment ready
- Business users available
- User training complete

Exit:
- Business scenarios validated
- Users accept the system
- Sign-off document signed
```

---

**Agile Context**:

**Sprint Entry Criteria**:
```
✅ User stories defined and refined
✅ Acceptance criteria clear
✅ Development environment ready
✅ Team capacity confirmed
✅ Definition of Ready met
```

**Sprint Exit Criteria (Definition of Done)**:
```
✅ Code complete and peer-reviewed
✅ Unit tests written and passing
✅ Integration tests passing
✅ User stories tested and accepted
✅ No critical bugs
✅ Documentation updated
✅ Deployed to staging
✅ Product Owner approved
```

---

**Benefits**:

**Entry Criteria**:
- ✅ Prevents premature testing
- ✅ Saves time (no testing on unstable builds)
- ✅ Clear expectations set
- ✅ Reduces wastage

**Exit Criteria**:
- ✅ Clear definition of "done"
- ✅ Quality gates enforced
- ✅ Prevents premature releases
- ✅ Objective decision-making

---

**Common Pitfalls**:

**1. Too Strict Entry Criteria**:
```
Problem: Testing never starts (waiting for 100% perfect conditions)
Solution: Balance - essential criteria only
```

**2. Too Lenient Exit Criteria**:
```
Problem: Poor quality released
Solution: Enforce minimum quality bars
```

**3. Not Documented**:
```
Problem: Subjective decisions, arguments
Solution: Document and get approval
```

**4. Not Reviewed**:
```
Problem: Outdated criteria
Solution: Review and update per project
```

---

**Interview Tips**:

**Question**: "What if entry criteria are not fully met but management wants testing to start?"
**Strong Answer**: "I would:
1. **Document risks**: List what's missing and potential impact
2. **Communicate**: Explain delays/issues likely if we proceed
3. **Negotiate**: Identify absolute must-haves vs nice-to-haves
4. **Get approval**: Written confirmation of decision
5. **Track issues**: Log all problems due to missing entry criteria
6. **Example**: If test environment is unstable but we must start, document 'testing delayed due to environment' as risk"

**Question**: "Can exit criteria change during project?"
**Answer**: "Yes, they can be refined but requires formal approval:
- If requirements added: Exit criteria may expand
- If scope reduced: Exit criteria may relax
- If timelines change: May prioritize different criteria
- Always need stakeholder agreement
- Document changes and reasons
- Update test plan accordingly"

---

This completes Q1-Q20 covering fundamental concepts! Continuing with Q21-Q30 covering advanced topics...


### Q21: What is Test Coverage? How do you measure it?

**Answer**:

**Definition**:
Test Coverage is a metric measuring the extent to which testing has been performed. It indicates what percentage of the application has been tested.

**Types of Test Coverage**:

**1. Requirement Coverage**:
```
Formula: (Requirements tested / Total requirements) × 100
Example: 45 out of 50 requirements tested = 90%
```

**2. Test Case Coverage**:
```
Formula: (Test cases executed / Total test cases) × 100
Example: 180 out of 200 test cases executed = 90%
```

**3. Code Coverage** (for unit testing):
- **Statement Coverage**: % of code statements executed
- **Branch Coverage**: % of decision branches tested
- **Path Coverage**: % of possible paths executed
- **Function Coverage**: % of functions called

**4. Feature Coverage**:
```
Formula: (Features tested / Total features) × 100
Example: 8 out of 10 features tested = 80%
```

**Industry Standards**:
- Requirements Coverage: Target 100%
- Code Coverage: Target 80-90%
- Test Case Execution: Target 95%+

---

### Q22: What is Exploratory Testing? When do you use it?

**Answer**:

**Definition**:
Exploratory Testing is simultaneous test design, execution, and learning where testers actively explore the application without predefined test cases.

**When to Use**:
- Time constraints (quick testing needed)
- New features (learn while testing)
- Supplement scripted testing
- Usability evaluation
- Finding creative bugs

**Approach**: Test-Learn-Design cycle

**Example Session**:
```
Charter: Explore search functionality for unexpected behaviors
Time: 60 minutes
Focus: Edge cases, error handling, user experience
```

---

### Q23: Explain Alpha Testing vs Beta Testing.

**Answer**:

**Alpha Testing**:
- Done by internal employees
- At developer's site
- Controlled environment
- Before product release
- White-box and black-box testing
- Identify bugs before customer exposure

**Beta Testing**:
- Done by select external users
- At customer's site
- Real environment
- After alpha testing
- Black-box testing only
- Gather user feedback
- Example: Gmail was in beta for 5 years

---

### Q24: What is Monkey Testing?

**Answer**:

**Definition**:
Monkey Testing is random testing without test cases or plan, similar to a monkey randomly pressing keys.

**Purpose**:
- Find crashes with random inputs
- Test application stability
- Stress testing with unpredictable user behavior

**Example**:
```
- Random clicking everywhere
- Entering random data in fields
- Quick navigation between pages
- Pressing keys randomly
- Combination of unusual actions
```

**Smart Monkey**: Knows UI, performs random but valid actions
**Dumb Monkey**: Completely random actions, may include invalid operations

---

### Q25: What is Ad-hoc Testing?

**Answer**:

**Definition**:
Ad-hoc Testing is informal, unstructured testing without formal test cases, based on tester's intuition and experience.

**Characteristics**:
- No planning
- No documentation
- Done randomly
- Relies on tester's knowledge
- Quick and informal

**When Used**:
- Time constraints
- After formal testing
- New feature quick check
- Knowledge-based exploration

**Difference from Exploratory**:
- Ad-hoc: No structure, completely random
- Exploratory: Has charter, time-boxed, documented findings

---

### Q26: What is Positive vs Negative Testing?

**Answer**:

**Positive Testing**:
- Test with valid inputs
- Verify system works as expected
- Happy path testing
- Example: Login with correct credentials

**Negative Testing**:
- Test with invalid inputs
- Verify system handles errors gracefully
- Example: Login with wrong password

**Importance**: Both equally important
- Positive: Ensures functionality works
- Negative: Ensures proper error handling

**Real Example - Age Field**:
```
Positive:
- Age = 25 → Accept ✅

Negative:
- Age = -5 → Error: "Age cannot be negative"
- Age = "ABC" → Error: "Age must be number"
- Age = Blank → Error: "Age required"
- Age = 999 → Error: "Invalid age"
```

---

### Q27: What is Risk-Based Testing?

**Answer**:

**Definition**:
Risk-Based Testing prioritizes testing efforts based on risk assessment of features/modules.

**Risk Factors**:
1. **Business Criticality**: Core features vs nice-to-have
2. **Complexity**: Complex code more likely to have bugs
3. **Change Frequency**: Recently modified areas
4. **Defect History**: Previously buggy modules
5. **User Impact**: Number of users affected

**Risk Matrix**:

| Feature | Probability of Failure | Impact if Fails | Risk Level | Test Priority |
|---------|------------------------|-----------------|------------|---------------|
| Payment | High | Critical | HIGH | Test First |
| Search | Medium | High | MEDIUM | Test Second |
| Help Page | Low | Low | LOW | Test Last |

**Approach**:
- Test high-risk areas thoroughly
- Medium-risk areas adequately
- Low-risk areas minimally
- Optimize testing when time-constrained

---

### Q28: What is Traceability Matrix? (Already covered as RTM in Q19, but concise version)

**Answer**:

**Traceability Matrix** links requirements to test cases, ensuring:
- All requirements tested
- Impact analysis possible
- No missing coverage
- Progress tracking

**Key Columns**:
- Requirement ID
- Requirement Description
- Test Case IDs
- Status
- Defects

**Purpose**: Demonstrate complete test coverage and enable change impact analysis.

---

### Q29: What are Showstopper bugs?

**Answer**:

**Definition**:
Showstopper bugs are critical defects that prevent major functionality from working, blocking further testing or release.

**Characteristics**:
- Application crashes
- Core features completely broken
- Data corruption
- Security breaches
- Blocks testing/release

**Examples**:
```
✗ Application crashes on launch
✗ Cannot login (all users blocked)
✗ Payment processing fails completely
✗ Database connection lost
✗ Cannot save any data
```

**Action**:
- Immediate priority
- Stop testing if blocking
- Escalate to management
- Fix and deploy ASAP
- Retest before continuing

**Not Showstoppers**:
- Minor UI issues
- Optional feature bugs
- Performance degradation (unless severe)
- Cosmetic problems

---

### Q30: What is the difference between Retesting and Regression Testing?

**Answer**:

**Retesting**:

**Definition**: Re-executing failed test cases after bug fixes to verify fix works

**Purpose**: Confirm bug is fixed

**When**: After developer marks bug as fixed

**Scope**: Only failed test cases

**Example**:
```
1. TC_005 failed: Login button not working
2. Developer fixes bug
3. Tester re-runs TC_005 only
4. Verification: Bug fixed ✅
```

**Characteristics**:
- Targeted testing
- Same test case, same data
- Verifies specific fix
- Cannot be skipped
- Usually manual

---

**Regression Testing**:

**Definition**: Re-testing existing functionality to ensure new changes haven't broken anything

**Purpose**: Ensure no side effects from changes

**When**: After any code change (fix, feature, refactor)

**Scope**: Entire application or affected modules

**Example**:
```
1. New feature: "Wishlist" added
2. Run all existing test cases
3. Verify: Login, Cart, Checkout still work
4. No regression ✅
```

**Characteristics**:
- Broad testing
- Different test cases
- Ensures stability
- Can be automated
- Selective or full regression

---

**Comparison**:

| Aspect | Retesting | Regression Testing |
|--------|-----------|-------------------|
| **Purpose** | Verify bug fixed | Ensure no new bugs |
| **When** | After bug fix | After any change |
| **Scope** | Failed test cases only | All/selective test cases |
| **Can Skip?** | No (must verify fix) | Sometimes (risk-based) |
| **Automation** | Usually manual | Highly automated |
| **Also Called** | Confirmation Testing | - |
| **Example** | Re-run TC_005 after fix | Run full test suite |

---

**Real-World Workflow**:

**Scenario**: Bug BUG-1234 "Login fails with special characters"

**Step 1 - Bug Found**:
```
TC_008: Login with email "test+user@example.com"
Result: FAIL (login not working)
Action: Log BUG-1234
```

**Step 2 - Bug Fixed**:
```
Developer fixes and deploys build 2.5.1
```

**Step 3 - Retesting**:
```
Re-run TC_008 with same data: "test+user@example.com"
Result: PASS ✅ (bug fixed)
Mark BUG-1234 as VERIFIED
```

**Step 4 - Regression Testing**:
```
Run regression suite:
- All login test cases (TC_001 to TC_020)
- Related authentication flows
- User profile tests
Result: All PASS ✅ (no regression)
```

**Step 5 - Sign-off**:
```
Both retesting and regression passed ✅
Ready for release
```

---

**Key Points**:

**Retesting**:
- "Did my fix work?"
- Specific test case
- Targeted
- Always done

**Regression Testing**:
- "Did my fix break anything else?"
- Multiple test cases
- Comprehensive
- Preventive

**Both Are Different**:
- Retesting confirms the FIX
- Regression confirms NO SIDE EFFECTS
- Both are essential for quality

---

**Interview Tip**:

**Tricky Question**: "Can retesting and regression be done together?"

**Strong Answer**: "Yes, they're usually done in sequence:
1. First: Retesting (verify the fix)
2. Then: Regression testing (verify no side effects)
3. Sometimes automated regression includes the fixed test case
4. But conceptually they're different - retesting is targeted, regression is broad
5. In practice, both happen as part of verification cycle after bug fixes"

---

## Summary: 30 Essential Interview Questions Covered

**Software Testing Fundamentals (Q1-Q5)**:
1. What is Software Testing?
2. QA vs QC
3. Verification vs Validation
4. Cost of Defects
5. Role of Tester

**SDLC & Methodologies (Q6-Q7)**:
6. SDLC Models
7. Agile Testing

**Test Levels (Q8-Q9)**:
8. Test Levels
9. Smoke vs Sanity

**Test Design Techniques (Q10-Q13)**:
10. Equivalence Partitioning
11. Boundary Value Analysis
12. Decision Table Testing
13. State Transition Testing

**Defect Management (Q14-Q16)**:
14. Bug Life Cycle
15. Severity vs Priority
16. Bug Report Writing

**Test Artifacts (Q17-Q20)**:
17. Test Plan
18. Test Scenario vs Test Case
19. RTM (Requirements Traceability Matrix)
20. Entry & Exit Criteria

**Test Metrics & Concepts (Q21-Q30)**:
21. Test Coverage
22. Exploratory Testing
23. Alpha vs Beta Testing
24. Monkey Testing
25. Ad-hoc Testing
26. Positive vs Negative Testing
27. Risk-Based Testing
28. Traceability Matrix
29. Showstopper Bugs
30. Retesting vs Regression Testing

---

## Final Interview Preparation Tips

**1. Practice Answering Aloud**: Don't just read - practice explaining to someone

**2. Use Real Examples**: Always relate to real projects or scenarios

**3. Structure Your Answers**:
- Definition
- Example
- Why it's important
- When to use

**4. Be Honest**: Say "I don't know but I can research" rather than guessing

**5. Ask Clarifying Questions**: Shows you think deeply

**6. Draw Diagrams**: Visual explanations impress interviewers

**7. Connect Concepts**: Show how different concepts relate

**8. Discuss Trade-offs**: Demonstrate mature understanding

**9. Stay Updated**: Know latest trends in testing

**10. Be Confident**: You've prepared well!

---

## Quick Revision Checklist

Before interview, ensure you can:

- [ ] Explain what testing is and why it matters
- [ ] Differentiate QA, QC, Verification, Validation
- [ ] Describe all test levels with examples
- [ ] Apply test design techniques (EP, BVA, Decision Table, State Transition)
- [ ] Explain bug life cycle completely
- [ ] Differentiate severity and priority with examples
- [ ] Write a complete bug report
- [ ] Explain test plan contents
- [ ] Create RTM
- [ ] Define entry and exit criteria
- [ ] Discuss SDLC models and when to use each
- [ ] Explain Agile testing approach
- [ ] Differentiate smoke, sanity, regression, retesting
- [ ] Discuss various testing types and techniques
- [ ] Demonstrate understanding of real-world scenarios

---

**Good luck with your interview! You've got this!** 🎯

---

*Complete study guide for Phase 1 Software Testing Basics - Interview Preparation*
*30 Questions with In-depth Explanations and Real-world Examples*
*Prepared for 10-year Testing Experience Role*
