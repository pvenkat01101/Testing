# Phase 2 - Manual Testing Mastery: Theory

## 1. Test Planning and Strategy

### 1.1 Test Plan vs Test Strategy
- **Test Strategy**: High-level approach for the entire project or organization (scope, levels, types, tools, risk approach).
- **Test Plan**: Detailed, project-specific document (scope, schedule, resources, entry/exit criteria).

**Rule of thumb**:
- Strategy is the "why and how" at a high level.
- Plan is the "what, who, when" for a specific release.

### 1.2 Risk-Based Testing (RBT)
**Goal**: Focus testing effort where failure risk is highest.

**Steps**:
1. Identify features and risks
2. Rate **Impact** (business damage) and **Probability** (likelihood)
3. Calculate Risk = Impact x Probability
4. Prioritize test cases by risk score

**Example risk matrix**:

| Feature | Impact | Probability | Risk | Priority |
|---------|--------|-------------|------|----------|
| Payment | High | Medium | High | P1 |
| Search | Medium | Medium | Medium | P2 |
| Help page | Low | Low | Low | P3 |

### 1.3 Test Estimation Techniques
- **Three-Point Estimation**:
  - Optimistic (O), Most likely (M), Pessimistic (P)
  - Estimated effort = (O + 4M + P) / 6
- **Work Breakdown Structure (WBS)**:
  - Break features into smaller tasks, then estimate each
- **Experience-Based**:
  - Use historical data or similar projects

### 1.4 Entry and Exit Criteria
**Entry Criteria**: Conditions to start testing
- Stable build deployed
- Requirements baselined
- Test data ready
- Environment ready

**Exit Criteria**: Conditions to stop testing
- All P1 and P2 test cases executed
- No open critical/high defects
- Regression passed
- Test summary report completed

### 1.5 Test Environment Planning
**Includes**:
- Hardware and software versions
- Browsers/devices
- Network conditions
- Test data setup
- Third-party integrations (payment, email, SMS)

**Common risks**:
- Unstable environment
- Incomplete data
- Misaligned configuration

---

## 2. Advanced Test Design

### 2.1 Pairwise Testing (All-Pairs)
**Concept**: Test all possible pairs of input combinations instead of all combinations.

**Why**: Most defects are caused by interaction of 2 parameters.

**Example**:
Inputs:
- Browser: Chrome, Firefox, Safari
- OS: Windows, macOS
- Language: English, Spanish

Total combos = 3 x 2 x 2 = 12
Pairwise might reduce to 6-7 cases while still covering all pairs.

### 2.2 Orthogonal Array Testing
**Concept**: Use mathematical arrays to cover combinations efficiently.

**Best for**:
- Many parameters
- Multiple values per parameter
- Complex systems (telecom, ERP)

### 2.3 Classification Tree Method
**Concept**: Break inputs into classifications and classes, then derive test cases.

**Steps**:
1. Identify input classifications (e.g., login type, password rules)
2. Define valid/invalid classes
3. Combine into test cases

### 2.4 Combinatorial Testing
**Goal**: Achieve high coverage with fewer test cases.

**Usage**:
- Configuration testing
- UI + backend integration
- Multi-factor workflows

---

## 3. Domain Knowledge (Examples)

### 3.1 Banking Domain
**Key workflows**:
- Login with multi-factor authentication
- Account summary
- Fund transfer (within bank, other bank)
- Beneficiary management
- Statement generation

**Critical validations**:
- Balance accuracy
- Transaction limits
- OTP expiry
- Audit logs
- Security and privacy

### 3.2 E-Commerce Domain
**Key workflows**:
- Search and filtering
- Product details
- Cart and checkout
- Promotions and coupons
- Payment and order confirmation

**Critical validations**:
- Pricing, tax, discount accuracy
- Inventory updates
- Email/SMS confirmation
- Refund and cancellation

### 3.3 Healthcare Domain
**Key workflows**:
- Patient registration
- Appointment scheduling
- Medical records access
- Billing and insurance

**Critical validations**:
- Privacy of patient data (PHI)
- Access control
- Audit trails
- Data accuracy

### 3.4 ERP Systems
**Key modules**:
- Finance, HR, Inventory, Procurement

**Critical validations**:
- Data integrity across modules
- Role-based access
- Workflow approvals
- Reports and analytics

---

## 4. Specialized Testing Types

### 4.1 Accessibility Testing
**Goal**: Ensure app is usable by people with disabilities.

**Key checks**:
- Keyboard-only navigation
- Screen reader labels
- Color contrast
- Focus order and visibility
- Alternative text for images

**Guideline**: WCAG 2.1 (A, AA, AAA)

### 4.2 Localization and Internationalization
- **Internationalization (I18N)**: Build software to support multiple languages and regions.
- **Localization (L10N)**: Adapt UI and content for a specific locale.

**Checks**:
- Date/time formats
- Currency and number formatting
- Text expansion
- Right-to-left languages

### 4.3 Cross-Browser Testing
**Goal**: Ensure consistent behavior across browsers.

**Focus areas**:
- Layout differences
- JavaScript compatibility
- CSS rendering
- Third-party integrations

### 4.4 Mobile Responsive Testing
**Goal**: Verify UI adapts to screen sizes and orientation.

**Checks**:
- Breakpoints
- Touch targets
- Image scaling
- Navigation usability

### 4.5 Database Testing
**Checks**:
- CRUD operations
- Data integrity and constraints
- Null handling
- Referential integrity
- Data consistency between UI and DB

### 4.6 API Testing (Manual using Postman)
**Core concepts**:
- HTTP methods (GET, POST, PUT, DELETE)
- Status codes (200, 201, 400, 401, 403, 404, 500)
- Headers, query params, request body
- Authentication (Basic, Bearer, OAuth)

**Typical checks**:
- Correct status codes
- Response schema
- Business validation messages
- Performance and response time

---

## 5. Test Management Tools

### 5.1 Jira Basics
- Issue types: Story, Task, Bug, Epic
- Workflow: New -> In Progress -> In Review -> Done
- Bug workflow: New -> Assigned -> Open -> Fixed -> Retest -> Closed
- JQL for filtering

### 5.2 Test Case Management in Excel
**Common columns**:
- Test Case ID
- Title
- Preconditions
- Steps
- Expected Result
- Actual Result
- Status
- Priority
- Requirement ID

### 5.3 Azure DevOps Test Plans
- Create test plans and suites
- Assign test cases
- Run and track executions
- Link defects to test cases

### 5.4 Bug Reporting Best Practices
- Clear title
- Steps to reproduce
- Expected vs actual result
- Environment details
- Severity and priority
- Attach screenshots or logs

---

## 6. Key Takeaways
- Manual testing mastery depends on planning, risk focus, and strong domain understanding.
- Advanced test design saves effort while maintaining coverage.
- Specialized testing expands your value in real projects.
- Test management tools help track quality and communicate status.
