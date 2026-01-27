# Phase 1 - Web Fundamentals: Practical Exercises

## Exercise 1: Command Line Navigation

### Objective
Master basic command line operations for testing work.

### Task 1: Directory Navigation (Windows)

```cmd
# 1. Open Command Prompt (Windows + R, type "cmd", Enter)

# 2. Check current directory
cd

# 3. List files in current directory
dir

# 4. Navigate to Documents folder
cd Documents

# 5. Go back to parent directory
cd ..

# 6. Navigate to root
cd \

# 7. Navigate to specific path
cd C:\Users\YourName\Desktop
```

### Task 2: Directory Navigation (Mac/Linux)

```bash
# 1. Open Terminal

# 2. Check current directory
pwd

# 3. List files
ls
ls -la  # Detailed list with hidden files

# 4. Navigate to Documents
cd Documents

# 5. Go back to parent directory
cd ..

# 6. Go to home directory
cd ~

# 7. Navigate to root
cd /
```

### Task 3: File Operations

**Windows**:
```cmd
# Create directory
mkdir TestFolder

# Navigate into it
cd TestFolder

# Create a file
echo Hello World > test.txt

# View file contents
type test.txt

# Copy file
copy test.txt test_backup.txt

# List files to verify
dir

# Delete file
del test_backup.txt

# Go back and remove directory
cd ..
rmdir TestFolder
```

**Mac/Linux**:
```bash
# Create directory
mkdir TestFolder

# Navigate into it
cd TestFolder

# Create a file
echo "Hello World" > test.txt

# View file contents
cat test.txt

# Copy file
cp test.txt test_backup.txt

# List files
ls -l

# Delete file
rm test_backup.txt

# Go back and remove directory
cd ..
rm -rf TestFolder
```

### Task 4: Search and Find

**Windows**:
```cmd
# Find where Chrome is installed
where chrome

# Search for text in file
findstr "error" logfile.txt

# Find all .txt files in directory
dir *.txt /s
```

**Mac/Linux**:
```bash
# Find where python is installed
which python3

# Search for text in file
grep "error" logfile.txt

# Find all .txt files
find . -name "*.txt"

# Find files modified in last 7 days
find . -mtime -7
```

**Deliverable**: Document showing screenshots of successful command executions

---

## Exercise 2: HTTP Methods Practice with Postman

### Objective
Understand HTTP methods by testing a real API.

### Setup
1. Download and install Postman: https://www.postman.com/downloads/
2. We'll use JSONPlaceholder API: https://jsonplaceholder.typicode.com/

### Task 1: GET Request

**Scenario**: Retrieve list of users

**Steps**:
1. Open Postman
2. Create new request
3. Method: GET
4. URL: `https://jsonplaceholder.typicode.com/users`
5. Click "Send"

**Verify**:
- Status: 200 OK
- Response body contains array of users
- Check one user's structure:
  ```json
  {
    "id": 1,
    "name": "Leanne Graham",
    "username": "Bret",
    "email": "Sincere@april.biz"
  }
  ```

**Task**:
- Get single user by ID: `GET /users/1`
- Get user's posts: `GET /users/1/posts`
- Take screenshots of responses

### Task 2: POST Request

**Scenario**: Create new post

**Steps**:
1. Method: POST
2. URL: `https://jsonplaceholder.typicode.com/posts`
3. Go to "Body" tab
4. Select "raw" and "JSON"
5. Enter:
   ```json
   {
     "title": "My Test Post",
     "body": "This is a test post for learning",
     "userId": 1
   }
   ```
6. Click "Send"

**Verify**:
- Status: 201 Created
- Response contains the created post with an ID
- Response:
  ```json
  {
    "title": "My Test Post",
    "body": "This is a test post for learning",
    "userId": 1,
    "id": 101
  }
  ```

**Additional Tests**:
1. POST without title (should fail)
2. POST without userId (should fail)
3. POST with invalid JSON format

### Task 3: PUT Request

**Scenario**: Update entire post

**Steps**:
1. Method: PUT
2. URL: `https://jsonplaceholder.typicode.com/posts/1`
3. Body:
   ```json
   {
     "id": 1,
     "title": "Updated Title",
     "body": "Updated body content",
     "userId": 1
   }
   ```
4. Send

**Verify**:
- Status: 200 OK
- Response shows updated data

### Task 4: PATCH Request

**Scenario**: Update only title

**Steps**:
1. Method: PATCH
2. URL: `https://jsonplaceholder.typicode.com/posts/1`
3. Body:
   ```json
   {
     "title": "Only Title Changed"
   }
   ```
4. Send

**Verify**:
- Status: 200 OK
- Only title updated, other fields unchanged

### Task 5: DELETE Request

**Scenario**: Delete a post

**Steps**:
1. Method: DELETE
2. URL: `https://jsonplaceholder.typicode.com/posts/1`
3. Send

**Verify**:
- Status: 200 OK
- Empty response body or success message

### Task 6: Test Different Status Codes

**404 Not Found**:
- GET `https://jsonplaceholder.typicode.com/users/9999999`
- Verify: 404 status

**400 Bad Request**:
- POST to `/posts` with malformed JSON
- Verify: 400 status (or server-specific error)

**Deliverable**:
- Postman collection exported as JSON
- Document with screenshots of all requests and responses
- Table showing request method, URL, status code, and response time

---

## Exercise 3: Analyze Network Traffic

### Objective
Use Browser DevTools to analyze HTTP requests.

### Task 1: Analyze Website Loading

**Steps**:
1. Open Chrome browser
2. Press F12 to open DevTools
3. Go to "Network" tab
4. Check "Preserve log"
5. Visit: https://www.amazon.com (or any e-commerce site)
6. Wait for page to fully load

**Analysis Questions**:

1. **How many total requests?**
   - Look at bottom of Network tab
   - Example: "150 requests"

2. **Total data transferred?**
   - Example: "2.5 MB transferred, 5.2 MB resources"

3. **Page load time?**
   - Look at "Load: X.XX s" at bottom
   - Example: "Load: 3.45 s"

4. **Types of resources loaded?**
   - Filter by: All, XHR, JS, CSS, Img, Media, Font, Doc
   - Count each type

5. **Largest resource?**
   - Sort by "Size" column
   - Identify largest file

6. **Slowest resource?**
   - Sort by "Time" column
   - Identify slowest loading resource

### Task 2: Analyze Login Request

**Steps**:
1. Visit: https://the-internet.herokuapp.com/login
2. Open DevTools → Network tab
3. Clear existing requests (🚫 icon)
4. Enter credentials:
   - Username: tomsmith
   - Password: SuperSecretPassword!
5. Click "Login"
6. Find the POST request to `/authenticate`

**Analysis**:

1. **Request Method**: POST
2. **Request URL**: https://the-internet.herokuapp.com/authenticate
3. **Status Code**: 200 or 302 (redirect)

4. **Request Headers**:
   - Click request → "Headers" tab
   - Find: Content-Type, User-Agent, Cookie

5. **Request Payload**:
   - Click "Payload" tab
   - See: username and password (Form Data)

6. **Response Headers**:
   - Content-Type: text/html
   - Set-Cookie: (session cookie)

7. **Response Body**:
   - Click "Response" tab
   - See the HTML of logged-in page

**Create Report**:

| Field | Value |
|-------|-------|
| Request Method | POST |
| Request URL | [URL] |
| Status Code | [Code] |
| Request Size | [Size] |
| Response Size | [Size] |
| Time | [ms] |
| Content-Type | [Type] |
| Cookies Set | [Yes/No] |

### Task 3: Find API Calls

**Steps**:
1. Visit: https://www.google.com
2. Open DevTools → Network tab
3. Filter: XHR (shows only API calls)
4. Type something in search box
5. Observe API calls being made

**Document**:
- Each API endpoint called
- Request method
- Response structure
- Purpose of each call

**Deliverable**:
- Network analysis report with screenshots
- Comparison of 3 different websites (load time, requests, size)

---

## Exercise 4: HTML Element Identification

### Objective
Practice finding elements for test automation.

### Task 1: Identify Element Attributes

**Website**: https://the-internet.herokuapp.com/login

**Find these elements and document**:

**1. Username Input Field**:
```
Element: <input>
ID: [Find it]
Name: [Find it]
Type: [Find it]
CSS Selector: [Write it]
XPath: [Write it]
```

**2. Password Input Field**:
```
Element: <input>
ID: [Find it]
Name: [Find it]
Type: [Find it]
CSS Selector: [Write it]
XPath: [Write it]
```

**3. Login Button**:
```
Element: <button> or <input>
ID: [Find it]
Type: [Find it]
Class: [Find it]
Text: [Find it]
CSS Selector: [Write it]
XPath: [Write it]
```

**Steps to Find**:
1. Right-click element → Inspect
2. Note all attributes in Elements tab
3. Right-click element in DevTools
4. Copy → Copy selector (CSS)
5. Copy → Copy XPath

### Task 2: Create Locator Strategy

**Website**: https://automationexercise.com/

**Document locators for**:

| Element | ID | Name | Class | CSS Selector | XPath |
|---------|----|----|-------|--------------|-------|
| Home link | | | | | |
| Products link | | | | | |
| Cart link | | | | | |
| Signup/Login link | | | | | |
| Search box | | | | | |
| Subscribe email | | | | | |

**Best Locator Selection**:
- Mark which locator you would use for each element
- Justify your choice (unique? stable? readable?)

### Task 3: Complex Element Locators

**Website**: https://demo.opencart.com/

**Find and document**:

1. **First product in list**:
   - CSS: `div.product-layout:nth-child(1)`
   - XPath: `(//div[@class='product-layout'])[1]`

2. **"Add to Cart" button for first product**:
   - CSS: [Write it]
   - XPath: [Write it]

3. **All product prices on page**:
   - CSS: [Write it] (should select all)
   - XPath: [Write it]

4. **Product with specific name**:
   - XPath: `//h4[text()='MacBook']/ancestor::div[@class='product-layout']`
   - CSS: [Try to write it]

5. **Search button**:
   - Multiple ways to locate it

**Deliverable**:
- Excel sheet with all element locators
- Screenshot of each element highlighted in DevTools
- Notes on which locator strategy is best and why

---

## Exercise 5: Cookie and Session Testing

### Objective
Understand cookies and sessions for testing.

### Task 1: View Cookies

**Steps**:
1. Visit: https://www.amazon.com
2. Open DevTools → Application tab
3. Expand "Cookies" under Storage
4. Click on "https://www.amazon.com"

**Document**:

| Cookie Name | Value (partial) | Domain | Path | Expires | HttpOnly | Secure |
|-------------|-----------------|--------|------|---------|----------|--------|
| session-id | [value] | .amazon.com | / | [date] | ☑️ | ☑️ |
| ... | ... | ... | ... | ... | ... | ... |

**Analysis**:
1. How many cookies total?
2. Which cookies are session cookies? (no expiry)
3. Which cookies are persistent? (have expiry)
4. Which cookies are HttpOnly? (secure)
5. Which cookies are Secure? (HTTPS only)

### Task 2: Cookie Testing

**Website**: https://the-internet.herokuapp.com/login

**Test Scenario**: Login and verify session cookie

**Steps**:
1. Clear all cookies (DevTools → Application → Cookies → Right-click → Clear)
2. Go to login page
3. Note: No cookies yet
4. Login with valid credentials
5. Check cookies again
6. Document the session cookie

**Cookie Test Cases**:

**TC_001**: Login with valid credentials
- Before login: No session cookie
- After login: Session cookie present
- Value: [Document it]

**TC_002**: Logout
- Delete session cookie manually
- Reload page
- Verify: Redirected to login (not authenticated)

**TC_003**: Cookie expiration
- Login successfully
- Wait for cookie to expire (or edit expiry to past date)
- Refresh page
- Verify: Redirected to login

**TC_004**: Cookie security
- Check HttpOnly flag
- Try accessing via JavaScript console: `document.cookie`
- If HttpOnly: Cookie should not appear
- If not HttpOnly: Cookie visible (security issue!)

### Task 3: Session Testing

**Scenario**: Test session persistence

1. **Single Browser**:
   - Login to application
   - Open new tab
   - Navigate to same application
   - Verify: Already logged in (session shared)

2. **Multiple Browsers**:
   - Login in Chrome
   - Open same application in Firefox
   - Verify: Not logged in (different session)

3. **Incognito/Private Mode**:
   - Login in normal mode
   - Open incognito window
   - Navigate to application
   - Verify: Not logged in (separate session)

4. **Session Timeout**:
   - Login to application
   - Note last activity time
   - Wait for timeout period (e.g., 30 minutes)
   - Try to perform action
   - Verify: Session expired, redirect to login

**Deliverable**:
- Cookie analysis document for 3 websites
- Session test results with screenshots
- Security findings (HttpOnly, Secure flags)

---

## Exercise 6: Status Code Testing

### Objective
Test different HTTP status codes using real APIs.

### Task: Create Test Cases for Status Codes

**API**: https://httpstat.us/ (Returns any status code you request)

### Test Cases

**TC_001: 200 OK**
```
GET https://httpstat.us/200
Expected: Status 200 OK
Response: "200 OK"
```

**TC_002: 201 Created**
```
GET https://httpstat.us/201
Expected: Status 201 Created
```

**TC_003: 204 No Content**
```
GET https://httpstat.us/204
Expected: Status 204, Empty body
```

**TC_004: 301 Moved Permanently**
```
GET https://httpstat.us/301
Expected: Status 301, Redirect
```

**TC_005: 400 Bad Request**
```
GET https://httpstat.us/400
Expected: Status 400 Bad Request
```

**TC_006: 401 Unauthorized**
```
GET https://httpstat.us/401
Expected: Status 401 Unauthorized
```

**TC_007: 403 Forbidden**
```
GET https://httpstat.us/403
Expected: Status 403 Forbidden
```

**TC_008: 404 Not Found**
```
GET https://httpstat.us/404
Expected: Status 404 Not Found
```

**TC_009: 500 Internal Server Error**
```
GET https://httpstat.us/500
Expected: Status 500 Internal Server Error
```

**TC_010: 503 Service Unavailable**
```
GET https://httpstat.us/503
Expected: Status 503 Service Unavailable
```

### Practical Status Code Testing

**Website**: https://the-internet.herokuapp.com/status_codes

**Test**:
1. Click each status code link
2. Verify correct status returned
3. Document actual vs expected

**Real API Testing**:

**API**: https://jsonplaceholder.typicode.com/

**Test Cases**:
1. Valid resource: `GET /posts/1` → 200
2. Invalid resource: `GET /posts/9999999` → 404
3. Create resource: `POST /posts` → 201
4. Update resource: `PUT /posts/1` → 200
5. Delete resource: `DELETE /posts/1` → 200

**Deliverable**:
- Status code test execution report
- Screenshots from Postman
- Real-world scenarios for each status code

---

## Exercise 7: JavaScript Console Practice

### Objective
Use browser console for testing and debugging.

### Task 1: Element Manipulation

**Website**: https://the-internet.herokuapp.com/login

**Open Console** (F12 → Console tab)

**Practice Commands**:

```javascript
// 1. Get username field
let username = document.getElementById('username');
console.log(username);

// 2. Set value
username.value = 'tomsmith';

// 3. Get password field
let password = document.getElementById('password');
password.value = 'SuperSecretPassword!';

// 4. Click login button
let loginBtn = document.querySelector('button[type="submit"]');
loginBtn.click();

// 5. Verify success message
let successMsg = document.querySelector('.success');
console.log(successMsg.textContent);
```

### Task 2: DOM Traversal

**Website**: Any website with navigation menu

```javascript
// 1. Get all links
let allLinks = document.querySelectorAll('a');
console.log('Total links:', allLinks.length);

// 2. Print all link texts and URLs
allLinks.forEach(link => {
    console.log(link.textContent, '→', link.href);
});

// 3. Find broken links (pointing to #)
let brokenLinks = document.querySelectorAll('a[href="#"]');
console.log('Broken links:', brokenLinks.length);

// 4. Get all images
let images = document.querySelectorAll('img');
console.log('Total images:', images.length);

// 5. Find images without alt text
images.forEach(img => {
    if (!img.alt) {
        console.log('Missing alt:', img.src);
    }
});

// 6. Get all input fields
let inputs = document.querySelectorAll('input');
console.log('Input fields:', inputs.length);

// 7. Check which inputs are required
inputs.forEach(input => {
    if (input.required) {
        console.log('Required:', input.name || input.id);
    }
});
```

### Task 3: Form Validation Testing

**Website**: https://the-internet.herokuapp.com/forgot_password

```javascript
// 1. Test with empty email
let emailField = document.getElementById('email');
let submitBtn = document.querySelector('button[type="submit"]');

emailField.value = '';
submitBtn.click();
// Observe: Should show error

// 2. Test with invalid email
emailField.value = 'invalidemail';
submitBtn.click();
// Observe: Should show error

// 3. Test with valid email
emailField.value = 'test@example.com';
submitBtn.click();
// Observe: Should succeed

// 4. Check if email field has proper validation
console.log('Email type:', emailField.type);
console.log('Required:', emailField.required);
console.log('Pattern:', emailField.pattern);
```

### Task 4: Simulate User Interactions

```javascript
// 1. Scroll to bottom
window.scrollTo(0, document.body.scrollHeight);

// 2. Scroll to top
window.scrollTo(0, 0);

// 3. Scroll to specific element
let element = document.getElementById('someId');
element.scrollIntoView();

// 4. Open new tab
window.open('https://www.google.com', '_blank');

// 5. Get current URL
console.log('Current URL:', window.location.href);

// 6. Reload page
// window.location.reload();

// 7. Navigate to new page
// window.location.href = 'https://www.example.com';
```

### Task 5: Debugging Test Scenarios

**Scenario**: Form not submitting

```javascript
// 1. Check if form exists
let form = document.querySelector('form');
console.log('Form:', form);

// 2. Check submit button
let submitBtn = form.querySelector('button[type="submit"]');
console.log('Submit button:', submitBtn);
console.log('Disabled?', submitBtn.disabled);

// 3. Check required fields
let requiredFields = form.querySelectorAll('[required]');
console.log('Required fields:', requiredFields.length);

// 4. Check if all required fields filled
requiredFields.forEach(field => {
    console.log(field.name, ':', field.value);
    if (!field.value) {
        console.log('❌ Empty:', field.name);
    }
});

// 5. Check for JavaScript errors
// Look for errors in Console tab

// 6. Submit form programmatically
form.submit();
```

**Deliverable**:
- JavaScript console commands document
- Screenshots of console outputs
- List of 10 testing scenarios solved using console

---

## Exercise 8: Build a Simple Test HTML Page

### Objective
Create your own test HTML page to practice testing.

### Task: Create Test Page

**File**: `test-login.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Test Login Page</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 400px;
            margin: 50px auto;
            padding: 20px;
        }
        .form-group {
            margin-bottom: 15px;
        }
        label {
            display: block;
            margin-bottom: 5px;
        }
        input {
            width: 100%;
            padding: 8px;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        button {
            width: 100%;
            padding: 10px;
            background-color: #007bff;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }
        button:hover {
            background-color: #0056b3;
        }
        .error {
            color: red;
            font-size: 14px;
            margin-top: 5px;
        }
        .success {
            color: green;
            padding: 10px;
            background-color: #d4edda;
            border-radius: 4px;
            margin-top: 10px;
        }
    </style>
</head>
<body>
    <h2>Login Page</h2>

    <form id="loginForm">
        <div class="form-group">
            <label for="email">Email:</label>
            <input type="email" id="email" name="email" required>
            <div class="error" id="emailError"></div>
        </div>

        <div class="form-group">
            <label for="password">Password:</label>
            <input type="password" id="password" name="password" required>
            <div class="error" id="passwordError"></div>
        </div>

        <div class="form-group">
            <input type="checkbox" id="remember" name="remember">
            <label for="remember" style="display:inline">Remember Me</label>
        </div>

        <button type="submit" id="loginBtn">Login</button>
    </form>

    <div class="success" id="successMsg" style="display:none;">
        Login successful!
    </div>

    <script>
        document.getElementById('loginForm').addEventListener('submit', function(e) {
            e.preventDefault();

            // Clear previous errors
            document.getElementById('emailError').textContent = '';
            document.getElementById('passwordError').textContent = '';
            document.getElementById('successMsg').style.display = 'none';

            // Get values
            let email = document.getElementById('email').value;
            let password = document.getElementById('password').value;

            // Validation
            let isValid = true;

            if (!email) {
                document.getElementById('emailError').textContent = 'Email is required';
                isValid = false;
            } else if (!email.includes('@')) {
                document.getElementById('emailError').textContent = 'Invalid email format';
                isValid = false;
            }

            if (!password) {
                document.getElementById('passwordError').textContent = 'Password is required';
                isValid = false;
            } else if (password.length < 6) {
                document.getElementById('passwordError').textContent = 'Password must be at least 6 characters';
                isValid = false;
            }

            // Success
            if (isValid) {
                console.log('Login attempt:', { email: email, password: '***' });
                document.getElementById('successMsg').style.display = 'block';
            }
        });
    </script>
</body>
</html>
```

### Your Testing Tasks

**1. Manual Testing**:
- Test all scenarios with this page
- Create test cases
- Execute and document results

**2. Element Identification**:
- Find all element locators
- Document ID, name, CSS selector, XPath

**3. Console Testing**:
- Test using JavaScript console
- Automate form submission

**4. Network Analysis**:
- Analyze in DevTools (though this is client-side only)

**5. Modify and Test**:
- Add new field: "Phone Number"
- Add validation
- Test the new field

**Deliverable**:
- Your HTML file
- Test case document
- Test execution results
- Element locator document

---

## Exercise 9: Real Website Testing Project

### Objective
Apply all learned concepts on a real website.

### Project: Test OpenCart Demo

**Website**: https://demo.opencart.com/

### Week 1: Analysis

**Day 1-2: Manual Exploration**
- Explore all features
- Document all pages and functionalities
- Note any bugs found

**Day 3-4: Element Identification**
- Create element repository for:
  - Home page
  - Product page
  - Cart page
  - Checkout page
  - Login/Register page
  - Search functionality

**Day 5: Network Analysis**
- Analyze all HTTP requests
- Document API endpoints
- Note cookies and sessions

### Week 2: Testing

**Day 1-2: Functional Testing**
- Create test cases (minimum 30)
- Execute test cases
- Log bugs

**Day 3: API Testing**
- Identify all API calls in Network tab
- Test APIs using Postman
- Verify responses

**Day 4: Console Testing**
- Test key scenarios using console
- Document JavaScript errors
- Test form validations

**Day 5: Report**
- Create comprehensive test report
- Include:
  - Executive summary
  - Test cases executed
  - Bugs found
  - Network analysis
  - API documentation
  - Element repository
  - Recommendations

**Deliverables**:
1. Test Plan
2. Element Locator Repository (Excel)
3. Test Case Document (50+ test cases)
4. Test Execution Report
5. Bug Report (with screenshots)
6. Network Analysis Report
7. API Documentation
8. Lessons Learned

---

## Exercise 10: Command Line Automation

### Objective
Automate repetitive testing tasks using command line.

### Task 1: Create Batch Script (Windows)

**File**: `run-tests.bat`

```batch
@echo off
echo Starting Test Execution...
echo.

echo Step 1: Navigating to project directory
cd C:\Projects\TestAutomation

echo Step 2: Checking Git status
git status

echo Step 3: Pulling latest code
git pull

echo Step 4: Running Maven tests
mvn clean test

echo Step 5: Opening test report
start target\surefire-reports\index.html

echo.
echo Tests completed!
pause
```

### Task 2: Create Shell Script (Mac/Linux)

**File**: `run-tests.sh`

```bash
#!/bin/bash

echo "Starting Test Execution..."
echo ""

echo "Step 1: Navigating to project directory"
cd ~/Projects/TestAutomation

echo "Step 2: Checking Git status"
git status

echo "Step 3: Pulling latest code"
git pull

echo "Step 4: Running Maven tests"
mvn clean test

echo "Step 5: Opening test report"
open target/surefire-reports/index.html

echo ""
echo "Tests completed!"
```

**Make executable**:
```bash
chmod +x run-tests.sh
```

**Run**:
```bash
./run-tests.sh
```

### Task 3: Useful Test Automation Scripts

**Check if application is running**:

```bash
# Check if application on port 8080 is running
netstat -ano | findstr 8080  # Windows
lsof -i :8080                # Mac/Linux

# If running, kill it
# Windows
taskkill /PID <PID> /F

# Mac/Linux
kill -9 <PID>
```

**Automated environment setup**:

```batch
@echo off
echo Setting up test environment...

echo Checking Java version
java -version

echo Checking Maven version
mvn -version

echo Starting Selenium Grid
java -jar selenium-server.jar standalone

echo Environment ready!
```

**Deliverable**:
- Custom automation scripts for your testing tasks
- Documentation on what each script does
- Screenshots of script execution

---

## Practice Schedule

**Week 1**: Exercises 1-3 (Command Line, HTTP, Network Analysis)
**Week 2**: Exercises 4-6 (HTML Elements, Cookies, Status Codes)
**Week 3**: Exercises 7-8 (JavaScript Console, Test Page)
**Week 4**: Exercises 9-10 (Real Project, Automation)

---

## Self-Assessment Checklist

After completing exercises, you should be able to:

- [ ] Navigate command line confidently
- [ ] Create and manipulate files via command line
- [ ] Use Postman for API testing
- [ ] Test all HTTP methods (GET, POST, PUT, DELETE)
- [ ] Verify HTTP status codes
- [ ] Use Browser DevTools Network tab
- [ ] Analyze HTTP requests and responses
- [ ] Identify HTML elements and attributes
- [ ] Find element locators (ID, class, CSS, XPath)
- [ ] View and test cookies
- [ ] Understand session management
- [ ] Use JavaScript console for testing
- [ ] Manipulate DOM via console
- [ ] Create simple HTML pages for testing
- [ ] Write basic automation scripts

**Next Steps**:
- Complete all exercises
- Build portfolio of test artifacts
- Move to Phase 2: Manual Testing Mastery

---

*Practice is essential! Don't just read - actually perform these exercises hands-on.*
