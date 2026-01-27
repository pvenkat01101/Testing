# Phase 2 - Manual Testing Mastery: Interview Questions and Answers

## Section 1: Test Planning and Strategy

### Q1: What is the difference between a Test Strategy and a Test Plan?

**Answer**:
- **Test Strategy** is a high-level approach for the entire project or organization. It defines testing goals, levels, types, tools, and risk approach.
- **Test Plan** is a project-specific document that details scope, schedule, resources, entry/exit criteria, and deliverables.

**Practical Example**:
For a banking app, the strategy says "risk-based testing and API plus UI testing." The plan lists exact test cases, assigned testers, and 2-week schedule.

**In-depth Explanation**:
Strategy answers the "why and how" in broad terms. Plan answers the "what, who, when" for a release. In interviews, show you can operate at both levels.

---

### Q2: How do you perform Risk-Based Testing (RBT)?

**Answer**:
1. Identify features and possible risks
2. Rate impact and probability
3. Calculate risk score (Impact x Probability)
4. Prioritize test cases accordingly

**Practical Example**:
Payment page: Impact High, Probability Medium -> P1 tests. Help page: Impact Low, Probability Low -> P3 tests.

**In-depth Explanation**:
RBT ensures limited time is spent on areas that can hurt the business the most. It also provides a clear, defensible reason for test priorities.

---

### Q3: Explain three-point estimation with a real example.

**Answer**:
Three-point estimation uses Optimistic (O), Most likely (M), and Pessimistic (P).
Formula: (O + 4M + P) / 6

**Practical Example**:
Test case design for login:
- O = 2 hours, M = 4 hours, P = 8 hours
- Estimate = (2 + 4*4 + 8) / 6 = 4.3 hours

**In-depth Explanation**:
It reduces bias by balancing best-case and worst-case assumptions, giving a more realistic estimate.

---

### Q4: What are entry and exit criteria? Give examples.

**Answer**:
- **Entry criteria** are conditions required to start testing.
- **Exit criteria** are conditions required to stop testing.

**Practical Example**:
Entry: Stable build, test data ready, environment up. Exit: All P1 tests executed, no open critical defects, test summary report done.

**In-depth Explanation**:
Clear criteria protect the team from starting too early or releasing too soon. They also set expectations with stakeholders.

---

### Q5: How do you prioritize test cases in tight timelines?

**Answer**:
Prioritize based on risk, business impact, and usage frequency.

**Practical Example**:
In e-commerce, payment and checkout are P1. Wishlist and FAQ are P3.

**In-depth Explanation**:
Prioritization ensures maximum coverage of critical paths, reducing release risk when time is limited.

---

### Q6: What all goes into a test environment plan?

**Answer**:
It includes hardware, OS, browsers, devices, network setup, test data, and integration points (payment, email, SMS).

**Practical Example**:
For a web app: Windows 11, macOS, Chrome/Firefox/Safari, test DB, SMTP server for emails.

**In-depth Explanation**:
Environment issues are a major cause of false failures. Planning avoids delays and instability.

---

## Section 2: Advanced Test Design

### Q7: What is Pairwise Testing and when do you use it?

**Answer**:
Pairwise testing ensures every pair of input values is tested at least once, reducing combinations.

**Practical Example**:
Browser (3) x OS (2) x Language (2) = 12 combos. Pairwise can reduce to 6-7 while covering all pairs.

**In-depth Explanation**:
Most bugs are caused by interaction between two parameters, so pairwise gives high defect detection with fewer tests.

---

### Q8: Difference between Pairwise Testing and Orthogonal Array Testing?

**Answer**:
- Pairwise focuses on all 2-way combinations.
- Orthogonal arrays use mathematical tables for higher coverage with large parameter sets.

**Practical Example**:
Pairwise works for login settings. Orthogonal arrays are better for complex configurations with 5-6 parameters.

**In-depth Explanation**:
Orthogonal arrays scale better and are preferred for telecom, embedded, or ERP systems with many configurations.

---

### Q9: Explain the Classification Tree Method.

**Answer**:
It breaks inputs into classifications and classes, then combines them into test cases.

**Practical Example**:
Login classifications: user type (admin, user), password (valid, invalid), OTP (valid, expired).

**In-depth Explanation**:
This method ensures you cover logical combinations, making test design structured and repeatable.

---

### Q10: What is combinatorial testing and why is it useful?

**Answer**:
Combinatorial testing covers interactions of multiple parameters without full exhaustive testing.

**Practical Example**:
Testing 4 parameters with 3 values each would mean 81 cases, but combinatorial reduces it to a smaller, meaningful set.

**In-depth Explanation**:
It balances coverage and effort, making it practical for complex systems with many configurations.

---

## Section 3: Domain Knowledge

### Q11: How do you test a fund transfer feature in a banking app?

**Answer**:
Test balance updates, limits, beneficiary validation, OTP, and audit logs.

**Practical Example**:
Transfer 500 from Account A to B. Verify A decreases by 500, B increases by 500, transaction ID is created.

**In-depth Explanation**:
Banking requires strict accuracy, security, and auditability. Every transaction must be traceable.

---

### Q12: What are common defects in e-commerce checkout?

**Answer**:
- Incorrect total calculations
- Discount not applied
- Tax issues
- Payment failure
- Inventory not updated

**Practical Example**:
Apply a 10 percent coupon and verify total, tax, and shipping recalculations.

**In-depth Explanation**:
Checkout defects directly impact revenue and customer trust, so they are high-priority tests.

---

### Q13: What key areas do you focus on in healthcare testing?

**Answer**:
Privacy, access control, data accuracy, and audit trails.

**Practical Example**:
Verify only doctors assigned to a patient can view records.

**In-depth Explanation**:
Healthcare data is sensitive, so confidentiality and integrity are as important as functionality.

---

### Q14: How would you test an ERP system module?

**Answer**:
Test workflows, data integrity across modules, and role-based access.

**Practical Example**:
Create a purchase order in procurement and verify it appears in finance approvals.

**In-depth Explanation**:
ERP systems are integrated. A defect in one module can break another, so end-to-end testing is critical.

---

## Section 4: Specialized Testing Types

### Q15: What are the basic steps in accessibility testing?

**Answer**:
Check keyboard navigation, screen reader labels, focus order, contrast, and error messages.

**Practical Example**:
Navigate a login page using only the keyboard and verify focus moves in logical order.

**In-depth Explanation**:
Accessibility improves usability for everyone and is required in many industries.

---

### Q16: Difference between localization and internationalization?

**Answer**:
- **Internationalization (I18N)**: Build the system to support multiple locales.
- **Localization (L10N)**: Adapt content to a specific locale.

**Practical Example**:
US format shows 01/31/2026 and $100. France shows 31/01/2026 and 100 EUR.

**In-depth Explanation**:
I18N is a development activity, L10N is a testing and content activity.

---

### Q17: What is your cross-browser testing strategy?

**Answer**:
Test critical flows on main browsers first, then expand based on user analytics.

**Practical Example**:
If 70 percent users are on Chrome, test all critical flows on Chrome, then do smoke on Firefox/Safari.

**In-depth Explanation**:
Cross-browser testing should be risk-driven to balance cost and coverage.

---

### Q18: How do you test responsive design?

**Answer**:
Check layout, navigation, and touch targets across breakpoints and orientations.

**Practical Example**:
Verify product filters are usable at 320px width and no content overlaps.

**In-depth Explanation**:
Responsive defects are common because CSS changes with breakpoints. Use a device matrix to reduce risk.

---

### Q19: What is database testing and why is it important?

**Answer**:
Database testing validates data integrity, constraints, and consistency between UI and DB.

**Practical Example**:
Place an order in UI and verify a new record in orders table with correct totals.

**In-depth Explanation**:
UI success does not guarantee backend correctness. DB validation catches hidden issues.

---

### Q20: What is API testing and how is it different from UI testing?

**Answer**:
API testing validates business logic at the service layer, while UI testing validates user workflows.

**Practical Example**:
Call POST /login and verify token, without using the UI.

**In-depth Explanation**:
API tests are faster, more stable, and isolate backend defects early.

---

### Q21: How do you test APIs manually using Postman?

**Answer**:
Create requests, set headers/body, send, and validate status codes and responses.

**Practical Example**:
POST /login with valid credentials, check 200 and token. Invalid credentials should return 401.

**In-depth Explanation**:
Manual API testing ensures correctness before automation and helps debug backend issues quickly.

---

### Q22: How do you validate authentication and authorization in API testing?

**Answer**:
Test valid tokens for authorized access and invalid tokens for blocked access.

**Practical Example**:
Call GET /user/profile with a valid token (200). Use an expired token (401).

**In-depth Explanation**:
Authentication confirms identity, authorization checks permissions. Both are critical for security.

---

## Section 5: Test Management and Defect Handling

### Q23: What is a typical Jira defect workflow?

**Answer**:
New -> Assigned -> Open -> Fixed -> Retest -> Closed (or Reopened).

**Practical Example**:
A defect found in checkout is logged as New, assigned to dev, fixed, retested, then closed.

**In-depth Explanation**:
Clear workflow ensures visibility and accountability for defect resolution.

---

### Q24: What information makes a high-quality bug report?

**Answer**:
- Clear title
- Steps to reproduce
- Expected vs actual result
- Environment details
- Severity and priority
- Screenshots/logs

**Practical Example**:
"Checkout fails when applying coupon SAVE10 on Firefox 122, Windows 11. Total becomes negative."

**In-depth Explanation**:
A good bug report reduces back-and-forth, speeds fixes, and avoids misunderstanding.

---

### Q25: How do you manage test cases in Excel effectively?

**Answer**:
Use a consistent template with IDs, steps, expected result, status, priority, and requirement ID.

**Practical Example**:
TC_Chk_001: Apply valid coupon -> Expected: total reduced by 10 percent.

**In-depth Explanation**:
Excel is common in smaller teams. Discipline in structure ensures traceability and reporting.

---

### Q26: What test metrics do you report in manual testing?

**Answer**:
- Test case execution status
- Defect count by severity
- Pass/fail rate
- Test coverage by requirement

**Practical Example**:
"Week 1: 60 percent executed, 5 critical bugs open, 80 percent coverage."

**In-depth Explanation**:
Metrics communicate quality health to stakeholders and help decide release readiness.

---

## Section 6: Problem Solving and Practical Thinking

### Q27: How do you handle last-minute requirement changes?

**Answer**:
Analyze impact, update test cases, re-estimate effort, and align with stakeholders.

**Practical Example**:
A new password rule is added, so update test cases and rerun regression for login flows.

**In-depth Explanation**:
Change control keeps testing aligned with requirements and avoids missed defects.

---

### Q28: What is exploratory testing and how do you structure it?

**Answer**:
Exploratory testing is learning and testing at the same time.

**Practical Example**:
Session-based testing on "checkout" for 60 minutes with a charter: "Find pricing and coupon issues."

**In-depth Explanation**:
It uncovers issues that scripted tests miss, especially in new or unstable features.

---

### Q29: How do you validate data consistency between UI and DB?

**Answer**:
Perform an action in UI, then query the DB to confirm correct records.

**Practical Example**:
After updating profile phone number, verify the users table has the new value.

**In-depth Explanation**:
Data validation catches integration defects and ensures backend reliability.

---

### Q30: When do you decide testing is complete?

**Answer**:
When exit criteria are met: critical tests executed, critical defects closed, and test summary report completed.

**Practical Example**:
All P1 cases pass, no open critical bugs, and regression suite completed.

**In-depth Explanation**:
Testing never guarantees zero defects, but exit criteria ensure acceptable risk before release.
