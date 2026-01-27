# Phase 1 - Web Fundamentals: Theory

## 1. Computer Basics

### 1.1 Operating Systems

**Definition**: Software that manages computer hardware and software resources and provides services for computer programs.

#### Popular Operating Systems

**1. Windows**
- Desktop/laptop OS
- Versions: Windows 10, Windows 11, Windows Server
- File system: NTFS
- Command line: Command Prompt, PowerShell
- Package manager: Chocolatey (third-party)

**2. Linux**
- Open-source OS
- Popular distributions: Ubuntu, CentOS, RedHat, Debian
- File system: ext4, ext3
- Command line: Bash, Zsh
- Package managers: apt (Ubuntu/Debian), yum (CentOS/RedHat)
- **Why important for testers**: Most servers run Linux, automation runs on Linux

**3. macOS**
- Apple's OS for Mac computers
- Based on Unix
- Command line: Terminal (Bash/Zsh)
- Package manager: Homebrew

### 1.2 File Systems and Directory Structure

#### Windows Directory Structure
```
C:\
├── Windows\          (OS files)
├── Program Files\    (64-bit applications)
├── Program Files (x86)\  (32-bit applications)
├── Users\
│   ├── YourName\
│   │   ├── Documents\
│   │   ├── Downloads\
│   │   ├── Desktop\
│   │   └── AppData\  (Application data)
└── ProgramData\      (Shared application data)
```

**Key Paths**:
- `C:\Windows\System32` - System files
- `C:\Users\Username\AppData\Local` - User-specific app data
- `C:\Program Files` - Installed applications

#### Linux/Mac Directory Structure
```
/
├── bin/      (Essential binaries/commands)
├── etc/      (Configuration files)
├── home/     (User home directories)
│   └── username/
│       ├── Documents/
│       ├── Downloads/
│       └── Desktop/
├── var/      (Variable data: logs, cache)
│   └── log/  (Log files)
├── tmp/      (Temporary files)
├── usr/      (User programs)
│   ├── bin/  (User binaries)
│   └── local/  (Locally installed software)
└── opt/      (Optional software)
```

**Key Paths**:
- `/home/username` - User home directory
- `/var/log` - Application and system logs
- `/etc` - Configuration files
- `/tmp` - Temporary files (cleared on reboot)

**Path Formats**:
- **Windows**: `C:\Users\John\Documents\file.txt` (backslash \)
- **Linux/Mac**: `/home/john/documents/file.txt` (forward slash /)

**Important for Testers**:
- Test data location
- Log file location for debugging
- Application installation paths
- Temporary file storage

### 1.3 Command Line/Terminal Basics

#### Why Learn Command Line?
1. **Automation**: Run scripts and automated tests
2. **Server Access**: Most test servers are Linux-based
3. **Efficiency**: Faster than GUI for many tasks
4. **Git Operations**: Version control commands
5. **CI/CD**: Jenkins, Docker commands run in terminal
6. **Troubleshooting**: Check logs, processes, network

#### Essential Windows Commands

```bash
# Navigation
cd Documents              # Change directory
cd ..                     # Go up one level
cd \                      # Go to root
dir                       # List files in directory

# File Operations
type filename.txt         # Display file contents
copy source dest          # Copy file
move source dest          # Move file
del filename              # Delete file
mkdir foldername          # Create directory
rmdir foldername          # Remove directory

# System Information
systeminfo                # Display system information
tasklist                  # List running processes
taskkill /PID 1234        # Kill process by ID
ipconfig                  # Network configuration
ping google.com           # Test network connectivity

# File Search
where python              # Find executable location
findstr "text" file.txt   # Search text in file
```

#### Essential Linux/Mac Commands

```bash
# Navigation
cd Documents              # Change directory
cd ..                     # Go up one level
cd ~                      # Go to home directory
cd /                      # Go to root
pwd                       # Print current directory
ls                        # List files
ls -la                    # List with details and hidden files

# File Operations
cat filename.txt          # Display file contents
cp source dest            # Copy file
mv source dest            # Move/rename file
rm filename               # Delete file
mkdir foldername          # Create directory
rmdir foldername          # Remove empty directory
rm -rf foldername         # Remove directory and contents

# File Permissions
chmod 755 script.sh       # Change file permissions
chown user:group file     # Change file owner

# System Information
uname -a                  # System information
top                       # Running processes (live)
ps aux                    # List all processes
kill PID                  # Kill process
kill -9 PID               # Force kill process
df -h                     # Disk space usage
free -h                   # Memory usage
ifconfig                  # Network configuration
ping google.com           # Test connectivity

# File Search
find . -name "*.txt"      # Find files by name
grep "text" file.txt      # Search text in file
grep -r "text" .          # Recursive search

# Package Management (Ubuntu/Debian)
sudo apt update           # Update package list
sudo apt install package  # Install package
sudo apt remove package   # Remove package

# Viewing Logs
tail -f /var/log/app.log  # View log file (live updates)
tail -100 file.log        # View last 100 lines
head -50 file.log         # View first 50 lines
less file.log             # View file (paginated)
```

#### Essential Commands for Testers

```bash
# Git Commands (covered in detail later)
git clone <url>           # Clone repository
git status                # Check status
git add .                 # Stage changes
git commit -m "message"   # Commit changes
git push                  # Push to remote

# Running Tests
mvn test                  # Run Maven tests (Java)
npm test                  # Run npm tests (JavaScript)
pytest                    # Run Python tests

# Checking Ports
netstat -ano | findstr 8080      # Windows: Check port 8080
lsof -i :8080                    # Mac/Linux: Check port 8080

# Environment Variables
echo %PATH%               # Windows: Display PATH
echo $PATH                # Linux/Mac: Display PATH
set VAR=value             # Windows: Set variable
export VAR=value          # Linux/Mac: Set variable
```

### 1.4 Process Management Basics

**What is a Process?**
A running instance of a program.

**Process States**:
1. **Running**: Currently executing
2. **Ready**: Waiting for CPU
3. **Waiting**: Waiting for resource (I/O, network)
4. **Terminated**: Finished execution

**Why Important for Testers**:
- Identify stuck/hanging processes
- Kill unresponsive browser processes
- Monitor resource usage during performance testing
- Check if application is running

**Viewing Processes**:

**Windows**:
```bash
tasklist                  # List all processes
tasklist /FI "PID eq 1234"  # Filter by PID
taskkill /PID 1234        # Kill by process ID
taskkill /IM chrome.exe   # Kill by image name
```

**Linux/Mac**:
```bash
ps aux                    # List all processes
ps aux | grep chrome      # Find specific process
top                       # Live process monitor
htop                      # Better process monitor (needs install)
kill 1234                 # Kill process by PID
killall chrome            # Kill all chrome processes
```

**Practical Example for Testers**:
```bash
# Scenario: Browser automation stuck

# 1. Find the process
ps aux | grep chrome
# Output: john  1234  10.0  5.0  12345  chrome

# 2. Kill the process
kill 1234

# 3. Verify it's killed
ps aux | grep chrome
# No output = process killed
```

---

## 2. Web Technologies

### 2.1 How the Web Works - Client-Server Architecture

**Overview**:
The web operates on a client-server model where clients (browsers) request resources and servers respond with those resources.

#### Components

**1. Client (Front-end)**:
- Web browser (Chrome, Firefox, Safari, Edge)
- Mobile app
- Desktop application
- Makes requests to server

**2. Server (Back-end)**:
- Web server (Apache, Nginx, IIS)
- Application server (Tomcat, Node.js, Express)
- Processes requests
- Sends responses

**3. Database**:
- Stores application data
- MySQL, PostgreSQL, MongoDB, Oracle

**4. Network**:
- Internet/Intranet
- Routes requests between client and server

#### Request-Response Flow

```
[User] → [Browser] → [Internet] → [Server] → [Database]
                                     ↓
[User] ← [Browser] ← [Internet] ← [Server] ← [Database]
```

**Detailed Step-by-Step**:

1. **User Action**: User types URL in browser or clicks link
   - Example: User types "www.amazon.com"

2. **DNS Lookup**: Browser finds server's IP address
   - Browser asks DNS: "What's the IP of amazon.com?"
   - DNS responds: "52.85.82.186"

3. **HTTP Request**: Browser sends request to server
   ```
   GET /products/123 HTTP/1.1
   Host: www.amazon.com
   User-Agent: Chrome/120.0
   Accept: text/html
   ```

4. **Server Processing**: Server processes request
   - Web server receives request
   - Application server runs business logic
   - Database queried if needed
   - Response prepared

5. **HTTP Response**: Server sends response to browser
   ```
   HTTP/1.1 200 OK
   Content-Type: text/html
   Content-Length: 1234

   <html>
   <body>Product details...</body>
   </html>
   ```

6. **Rendering**: Browser displays the page
   - Parses HTML
   - Loads CSS for styling
   - Executes JavaScript
   - Renders page to user

**Time Breakdown**:
- DNS Lookup: 20-120ms
- Connection: 50-200ms
- Server Processing: 100-500ms
- Download: 100-1000ms
- Rendering: 200-2000ms
- **Total**: 0.5-4 seconds

### 2.2 HTTP/HTTPS Protocols

**HTTP (HyperText Transfer Protocol)**:
- Application-level protocol for web communication
- Stateless (each request independent)
- Runs on top of TCP/IP
- Default port: 80

**HTTPS (HTTP Secure)**:
- HTTP with encryption (SSL/TLS)
- Secure data transmission
- Default port: 443
- Requires SSL certificate
- Shows padlock 🔒 in browser

**Why HTTPS?**:
- ✅ Data encryption (passwords, credit cards)
- ✅ Data integrity (cannot be modified in transit)
- ✅ Authentication (server is who it claims to be)
- ✅ Trust and SEO benefits

### 2.3 HTTP Request-Response Cycle

#### HTTP Request Structure

```
GET /api/users/123 HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
Accept: application/json
Authorization: Bearer token123
Content-Type: application/json
Cookie: session_id=abc123

{
  "optional": "request body for POST/PUT"
}
```

**Parts of HTTP Request**:

1. **Request Line**:
   - Method: GET, POST, PUT, DELETE, etc.
   - URI: /api/users/123
   - HTTP Version: HTTP/1.1

2. **Headers**:
   - Host: example.com (required)
   - User-Agent: Browser/client information
   - Accept: Expected response format
   - Authorization: Authentication token
   - Content-Type: Format of request body
   - Cookie: Session/state information

3. **Body** (optional):
   - Data sent to server (POST, PUT, PATCH)
   - JSON, XML, form data, etc.

#### HTTP Response Structure

```
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 145
Set-Cookie: session_id=xyz789
Cache-Control: max-age=3600

{
  "id": 123,
  "name": "John Doe",
  "email": "john@example.com"
}
```

**Parts of HTTP Response**:

1. **Status Line**:
   - HTTP Version: HTTP/1.1
   - Status Code: 200
   - Status Text: OK

2. **Headers**:
   - Content-Type: Format of response body
   - Content-Length: Size in bytes
   - Set-Cookie: Set cookies in browser
   - Cache-Control: Caching instructions

3. **Body**:
   - Actual data (HTML, JSON, XML, etc.)
   - Can be empty for some responses

### 2.4 HTTP Methods (Verbs)

| Method | Purpose | Has Body? | Idempotent? | Example |
|--------|---------|-----------|-------------|---------|
| **GET** | Retrieve resource | No | Yes | Get user details |
| **POST** | Create new resource | Yes | No | Create new user |
| **PUT** | Update/Replace resource | Yes | Yes | Update entire user |
| **PATCH** | Partial update | Yes | No | Update user email only |
| **DELETE** | Delete resource | No | Yes | Delete user |
| **HEAD** | Get headers only | No | Yes | Check if resource exists |
| **OPTIONS** | Get supported methods | No | Yes | CORS preflight |

**Detailed Explanation**:

#### GET Request
**Purpose**: Retrieve data from server

**Example**:
```
GET /api/users/123 HTTP/1.1
Host: example.com
```

**Response**:
```
HTTP/1.1 200 OK
{
  "id": 123,
  "name": "John Doe"
}
```

**Characteristics**:
- No request body
- Parameters in URL query string: `/users?page=1&limit=10`
- Cacheable
- Can be bookmarked
- Should not modify data

**Testing Focus**:
- Verify correct data returned
- Test pagination, filtering, sorting
- Test invalid IDs (404 error)
- Performance testing

#### POST Request
**Purpose**: Create new resource

**Example**:
```
POST /api/users HTTP/1.1
Host: example.com
Content-Type: application/json

{
  "name": "Jane Smith",
  "email": "jane@example.com",
  "password": "Pass@123"
}
```

**Response**:
```
HTTP/1.1 201 Created
Location: /api/users/124

{
  "id": 124,
  "name": "Jane Smith",
  "email": "jane@example.com"
}
```

**Characteristics**:
- Has request body
- Not idempotent (multiple calls create multiple resources)
- Returns 201 Created
- Not cacheable

**Testing Focus**:
- Verify resource created
- Test with missing required fields
- Test with invalid data
- Test duplicate creation
- Verify database entry

#### PUT Request
**Purpose**: Update entire resource (replace)

**Example**:
```
PUT /api/users/124 HTTP/1.1
Host: example.com
Content-Type: application/json

{
  "name": "Jane Doe",
  "email": "jane.doe@example.com",
  "phone": "123-456-7890"
}
```

**Response**:
```
HTTP/1.1 200 OK
{
  "id": 124,
  "name": "Jane Doe",
  "email": "jane.doe@example.com",
  "phone": "123-456-7890"
}
```

**Characteristics**:
- Replaces entire resource
- Idempotent (multiple identical calls = same result)
- Must include all fields
- Returns updated resource

**Testing Focus**:
- Verify entire resource updated
- Test with missing fields (should fail or set to null)
- Test idempotency
- Test invalid ID

#### PATCH Request
**Purpose**: Partial update

**Example**:
```
PATCH /api/users/124 HTTP/1.1
Host: example.com
Content-Type: application/json

{
  "email": "newemail@example.com"
}
```

**Response**:
```
HTTP/1.1 200 OK
{
  "id": 124,
  "name": "Jane Doe",  (unchanged)
  "email": "newemail@example.com",  (updated)
  "phone": "123-456-7890"  (unchanged)
}
```

**Characteristics**:
- Updates only specified fields
- Other fields remain unchanged
- More efficient than PUT

**Testing Focus**:
- Verify only specified fields updated
- Other fields unchanged
- Test invalid field names

#### DELETE Request
**Purpose**: Delete resource

**Example**:
```
DELETE /api/users/124 HTTP/1.1
Host: example.com
```

**Response**:
```
HTTP/1.1 204 No Content
```
or
```
HTTP/1.1 200 OK
{
  "message": "User deleted successfully"
}
```

**Characteristics**:
- No request body typically
- Returns 204 (No Content) or 200 (OK)
- Idempotent

**Testing Focus**:
- Verify resource deleted from database
- Test deleting already deleted resource (should return 404)
- Test deleting with invalid ID
- Test cascade deletes (related data)

### 2.5 HTTP Status Codes

**Status Code Categories**:
- **1xx**: Informational (rarely used)
- **2xx**: Success
- **3xx**: Redirection
- **4xx**: Client Error
- **5xx**: Server Error

#### Common Status Codes

**2xx Success**:

| Code | Status | Meaning | Example |
|------|--------|---------|---------|
| **200** | OK | Request successful | GET request returns data |
| **201** | Created | Resource created | POST creates new user |
| **204** | No Content | Success but no data to return | DELETE successful |

**3xx Redirection**:

| Code | Status | Meaning | Example |
|------|--------|---------|---------|
| **301** | Moved Permanently | Resource moved to new URL | Old URL redirects |
| **302** | Found (Temporary Redirect) | Temporary redirect | Maintenance page |
| **304** | Not Modified | Cached version is still valid | Browser cache |

**4xx Client Errors**:

| Code | Status | Meaning | Example |
|------|--------|---------|---------|
| **400** | Bad Request | Invalid request syntax/data | Missing required field |
| **401** | Unauthorized | Authentication required | No auth token provided |
| **403** | Forbidden | Authenticated but not authorized | User lacks permission |
| **404** | Not Found | Resource doesn't exist | Invalid user ID |
| **405** | Method Not Allowed | HTTP method not supported | POST to read-only endpoint |
| **409** | Conflict | Request conflicts with current state | Duplicate email registration |
| **422** | Unprocessable Entity | Validation error | Email format invalid |
| **429** | Too Many Requests | Rate limit exceeded | Too many API calls |

**5xx Server Errors**:

| Code | Status | Meaning | Example |
|------|--------|---------|---------|
| **500** | Internal Server Error | Generic server error | Unhandled exception |
| **502** | Bad Gateway | Invalid response from upstream | Proxy error |
| **503** | Service Unavailable | Server temporarily unavailable | Server maintenance |
| **504** | Gateway Timeout | Upstream server timeout | Slow database query |

**Testing Status Codes**:

**Example Test Cases**:
1. **200 OK**: GET /users/123 with valid ID → 200
2. **201 Created**: POST /users with valid data → 201
3. **400 Bad Request**: POST /users without required email → 400
4. **401 Unauthorized**: GET /users without auth token → 401
5. **403 Forbidden**: DELETE /users/123 (regular user deleting) → 403
6. **404 Not Found**: GET /users/999999 (non-existent) → 404
7. **409 Conflict**: POST /users with existing email → 409
8. **500 Internal Server Error**: Trigger server error → 500

### 2.6 Cookies and Sessions

#### Cookies

**Definition**: Small text files stored in browser, sent with every request to the same domain.

**Purpose**:
- Remember user preferences
- Session management
- Tracking and analytics
- Shopping cart data

**Cookie Attributes**:

```
Set-Cookie: session_id=abc123; Domain=example.com; Path=/; Expires=Wed, 09 Jun 2024 10:18:14 GMT; Secure; HttpOnly; SameSite=Strict
```

**Attributes Explained**:

| Attribute | Purpose | Example |
|-----------|---------|---------|
| **Name=Value** | Cookie data | session_id=abc123 |
| **Domain** | Which domain can access | example.com (and subdomains) |
| **Path** | Which path can access | /admin (only /admin/*) |
| **Expires** | When cookie expires | Wed, 09 Jun 2024... |
| **Max-Age** | Seconds until expiration | 3600 (1 hour) |
| **Secure** | Only sent over HTTPS | Secure |
| **HttpOnly** | Not accessible via JavaScript | HttpOnly (security) |
| **SameSite** | Cross-site request control | Strict/Lax/None |

**Types of Cookies**:

1. **Session Cookies**: Deleted when browser closes
   ```
   Set-Cookie: temp_session=xyz123
   ```

2. **Persistent Cookies**: Stored for specified time
   ```
   Set-Cookie: user_pref=dark_mode; Max-Age=2592000
   ```

3. **First-Party Cookies**: Set by visited domain
   ```
   Set-Cookie: login=true; Domain=example.com
   ```

4. **Third-Party Cookies**: Set by external domain (ads, analytics)
   ```
   Set-Cookie: tracking=abc; Domain=analytics.com
   ```

#### Sessions

**Definition**: Server-side storage of user data for duration of visit.

**How Sessions Work**:

1. **User Logs In**:
   ```
   POST /login
   {
     "email": "user@example.com",
     "password": "pass123"
   }
   ```

2. **Server Creates Session**:
   - Generates unique session ID: "sess_abc123xyz"
   - Stores session data on server
   - Sends session ID to client

3. **Server Sends Cookie**:
   ```
   HTTP/1.1 200 OK
   Set-Cookie: session_id=sess_abc123xyz; HttpOnly; Secure
   ```

4. **Browser Stores Cookie**:
   - Cookie saved in browser

5. **Subsequent Requests Include Cookie**:
   ```
   GET /dashboard
   Cookie: session_id=sess_abc123xyz
   ```

6. **Server Validates Session**:
   - Receives session ID
   - Looks up session data
   - Validates user is authenticated
   - Returns requested data

7. **Session Expiration**:
   - Timeout after inactivity (e.g., 30 minutes)
   - User logs out
   - Server deletes session data

**Session Storage**:
- In-memory (RAM) - Fast but lost on restart
- Database - Persistent, slower
- Redis/Memcached - Fast, distributed
- File system - Simple but slower

**Testing Cookies and Sessions**:

**Test Cases**:
1. ✅ Login → Verify session cookie set
2. ✅ Make authenticated request → Verify cookie sent
3. ✅ Logout → Verify session cookie deleted
4. ✅ Session timeout → Verify redirect to login after inactivity
5. ✅ Cookie expiration → Verify expired cookie not accepted
6. ✅ Invalid session ID → Verify 401 Unauthorized
7. ✅ Concurrent sessions → Verify multiple logins from different devices
8. ✅ HttpOnly → Verify JavaScript cannot access cookie
9. ✅ Secure → Verify cookie only sent over HTTPS

**Browser Developer Tools for Cookies**:

**Chrome DevTools**:
1. Press F12
2. Go to Application tab
3. Click Cookies under Storage
4. View/Edit/Delete cookies

### 2.7 Browser Developer Tools (Chrome DevTools)

**Opening DevTools**:
- **Windows/Linux**: F12 or Ctrl+Shift+I
- **Mac**: Cmd+Option+I
- **Right-click**: Inspect Element

#### Key Tabs for Testers

**1. Elements Tab**:
- View and edit HTML/CSS in real-time
- Inspect DOM structure
- Find element locators for automation
- **Use Case**: Find XPath/CSS selectors for Selenium

**2. Console Tab**:
- View JavaScript errors
- Execute JavaScript commands
- View console.log() outputs
- **Use Case**: Debug JavaScript issues, test JS commands

**3. Network Tab** (MOST IMPORTANT for testers):
- View all HTTP requests and responses
- Check request/response headers
- View request/response payload
- Monitor load times
- Filter by type (XHR, JS, CSS, Images)
- **Use Case**: API testing, performance analysis, debugging

**Network Tab Details**:
- **Name**: Request URL
- **Status**: HTTP status code
- **Type**: Resource type (XHR, document, script, etc.)
- **Initiator**: What triggered the request
- **Size**: Response size
- **Time**: Request duration
- **Waterfall**: Visual timeline

**Analyzing a Request**:
1. Click on request name
2. **Headers**: View request/response headers
3. **Preview**: Formatted response
4. **Response**: Raw response
5. **Timing**: Detailed timing breakdown

**4. Application Tab**:
- View cookies
- Local Storage
- Session Storage
- Cache
- Service Workers
- **Use Case**: Cookie testing, storage debugging

**5. Sources Tab**:
- View and debug JavaScript
- Set breakpoints
- Step through code
- **Use Case**: Debug complex JavaScript issues

**6. Performance Tab**:
- Record page load performance
- Analyze bottlenecks
- View frame rate
- **Use Case**: Performance testing

**7. Security Tab**:
- View certificate information
- Security issues
- **Use Case**: HTTPS verification

**Practical Examples for Testers**:

**Example 1: Find Why Login Fails**
1. Open Network tab
2. Click "Preserve log"
3. Attempt login
4. Find POST /login request
5. Check Status: 400 (Bad Request)
6. View Response: {"error": "Email required"}
7. **Root Cause**: Email field validation issue

**Example 2: Find Element Locator**
1. Open Elements tab
2. Click element selector (top-left icon)
3. Click element on page
4. Right-click element in Elements tab
5. Copy → Copy XPath or CSS Selector
6. Use in Selenium script

**Example 3: Test API Response**
1. Open Network tab
2. Filter: XHR (API calls only)
3. Perform action (e.g., search products)
4. Find GET /api/products?q=phone
5. View Response
6. Verify JSON structure and data
7. **Validation**: Check if all expected fields present

---

## 3. HTML Basics

### 3.1 HTML Structure and Tags

**HTML (HyperText Markup Language)**:
- Standard markup language for web pages
- Uses tags to structure content
- Not a programming language (no logic)

**Basic HTML Structure**:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Page Title</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <h1>Welcome to My Website</h1>
    <p>This is a paragraph.</p>
    <script src="script.js"></script>
</body>
</html>
```

**Structure Breakdown**:

1. **<!DOCTYPE html>**: Declares HTML5
2. **<html>**: Root element
3. **<head>**: Metadata (not visible on page)
   - <meta>: Character encoding, viewport
   - <title>: Browser tab title
   - <link>: External CSS files
   - <script>: JavaScript files (can be in head or body)
4. **<body>**: Visible content

### 3.2 Common HTML Elements

#### Text Elements

```html
<!-- Headings (h1 largest, h6 smallest) -->
<h1>Main Heading</h1>
<h2>Subheading</h2>
<h3>Sub-subheading</h3>

<!-- Paragraph -->
<p>This is a paragraph of text.</p>

<!-- Line Break -->
<p>Line 1<br>Line 2</p>

<!-- Horizontal Rule -->
<hr>

<!-- Text Formatting -->
<strong>Bold text</strong>
<b>Bold text (without semantic meaning)</b>
<em>Italic text</em>
<i>Italic text (without semantic meaning)</i>
<u>Underlined text</u>
<mark>Highlighted text</mark>
<small>Small text</small>
<del>Deleted text</del>
<ins>Inserted text</ins>
<sub>Subscript</sub>
<sup>Superscript</sup>
```

#### Links

```html
<!-- Basic link -->
<a href="https://www.example.com">Click here</a>

<!-- Open in new tab -->
<a href="https://www.example.com" target="_blank">Open in new tab</a>

<!-- Link to email -->
<a href="mailto:test@example.com">Send email</a>

<!-- Link to phone -->
<a href="tel:+1234567890">Call us</a>

<!-- Link to section on same page -->
<a href="#section1">Go to Section 1</a>
<div id="section1">Section 1 content</div>
```

#### Images

```html
<!-- Basic image -->
<img src="image.jpg" alt="Description of image">

<!-- Image with width and height -->
<img src="image.jpg" alt="Description" width="300" height="200">

<!-- Image as link -->
<a href="page.html">
    <img src="image.jpg" alt="Clickable image">
</a>
```

#### Lists

```html
<!-- Unordered list (bullets) -->
<ul>
    <li>Item 1</li>
    <li>Item 2</li>
    <li>Item 3</li>
</ul>

<!-- Ordered list (numbers) -->
<ol>
    <li>First item</li>
    <li>Second item</li>
    <li>Third item</li>
</ol>

<!-- Nested list -->
<ul>
    <li>Item 1
        <ul>
            <li>Subitem 1.1</li>
            <li>Subitem 1.2</li>
        </ul>
    </li>
    <li>Item 2</li>
</ul>
```

### 3.3 Important Elements for Testers

#### Div and Span

```html
<!-- Div: Block-level container -->
<div class="container">
    <p>Content inside div</p>
</div>

<!-- Span: Inline container -->
<p>This is <span class="highlight">highlighted</span> text.</p>
```

**Difference**:
- **div**: Block-level (starts new line, full width)
- **span**: Inline (stays on same line)

#### Input Elements (Forms)

```html
<!-- Text input -->
<input type="text" id="username" name="username" placeholder="Enter username">

<!-- Password input -->
<input type="password" id="password" name="password">

<!-- Email input (with validation) -->
<input type="email" id="email" name="email" required>

<!-- Number input -->
<input type="number" id="age" name="age" min="18" max="100">

<!-- Date input -->
<input type="date" id="birthdate" name="birthdate">

<!-- Checkbox -->
<input type="checkbox" id="agree" name="agree" value="yes">
<label for="agree">I agree to terms</label>

<!-- Radio buttons -->
<input type="radio" id="male" name="gender" value="male">
<label for="male">Male</label>
<input type="radio" id="female" name="gender" value="female">
<label for="female">Female</label>

<!-- File upload -->
<input type="file" id="resume" name="resume" accept=".pdf,.doc">

<!-- Hidden field (not visible) -->
<input type="hidden" id="user_id" name="user_id" value="123">
```

#### Button

```html
<!-- Button with text -->
<button type="button">Click Me</button>

<!-- Submit button (submits form) -->
<button type="submit">Submit Form</button>

<!-- Reset button (clears form) -->
<button type="reset">Reset</button>

<!-- Button with onclick event -->
<button onclick="alert('Clicked!')">Alert</button>
```

#### Forms

```html
<form action="/submit" method="POST" id="loginForm">
    <label for="email">Email:</label>
    <input type="email" id="email" name="email" required>

    <label for="password">Password:</label>
    <input type="password" id="password" name="password" required>

    <button type="submit">Login</button>
</form>
```

**Form Attributes**:
- **action**: URL where form data is sent
- **method**: HTTP method (GET or POST)
- **id**: Unique identifier

#### Dropdown (Select)

```html
<!-- Basic dropdown -->
<select id="country" name="country">
    <option value="">Select Country</option>
    <option value="us">United States</option>
    <option value="uk">United Kingdom</option>
    <option value="in">India</option>
</select>

<!-- Multi-select -->
<select id="skills" name="skills" multiple>
    <option value="java">Java</option>
    <option value="python">Python</option>
    <option value="javascript">JavaScript</option>
</select>

<!-- With default selection -->
<select id="language">
    <option value="en" selected>English</option>
    <option value="es">Spanish</option>
</select>
```

#### Tables

```html
<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Age</th>
            <th>City</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>John</td>
            <td>25</td>
            <td>New York</td>
        </tr>
        <tr>
            <td>Jane</td>
            <td>30</td>
            <td>London</td>
        </tr>
    </tbody>
</table>
```

### 3.4 HTML Attributes

**Common Attributes**:

```html
<!-- id: Unique identifier (only one per page) -->
<div id="header">Header content</div>

<!-- class: Group identifier (multiple elements can have same class) -->
<div class="container">Content</div>
<div class="container">More content</div>

<!-- name: Form element name -->
<input type="text" name="username">

<!-- value: Default value -->
<input type="text" value="John Doe">

<!-- placeholder: Hint text -->
<input type="text" placeholder="Enter your name">

<!-- disabled: Disable element -->
<button disabled>Cannot click</button>

<!-- readonly: Cannot edit -->
<input type="text" value="Read only" readonly>

<!-- required: Must be filled -->
<input type="email" required>

<!-- maxlength: Maximum characters -->
<input type="text" maxlength="10">

<!-- title: Tooltip on hover -->
<button title="Click to submit">Submit</button>

<!-- style: Inline CSS -->
<p style="color: red; font-size: 20px;">Red text</p>

<!-- data-* attributes (custom attributes) -->
<div data-user-id="123" data-role="admin">User</div>
```

### 3.5 DOM (Document Object Model) Structure

**What is DOM?**
- Tree-like representation of HTML document
- Allows JavaScript to access and manipulate HTML elements
- Created by browser when page loads

**HTML**:
```html
<!DOCTYPE html>
<html>
<head>
    <title>My Page</title>
</head>
<body>
    <div id="container">
        <h1>Welcome</h1>
        <p class="text">Hello World</p>
    </div>
</body>
</html>
```

**DOM Tree**:
```
Document
└── html
    ├── head
    │   └── title
    │       └── "My Page"
    └── body
        └── div (id="container")
            ├── h1
            │   └── "Welcome"
            └── p (class="text")
                └── "Hello World"
```

**DOM Terminology**:
- **Document**: Root node
- **Element Node**: HTML tags (<div>, <p>, etc.)
- **Text Node**: Text content
- **Attribute Node**: Attributes (id, class, etc.)
- **Parent**: Node above current node
- **Child**: Node below current node
- **Sibling**: Nodes at same level

**Why Important for Testers?**
- Understanding DOM helps find element locators
- Selenium interacts with DOM
- XPath and CSS selectors navigate DOM tree
- JavaScript automation uses DOM methods

### 3.6 Inspecting Elements in Browser

**How to Inspect**:
1. Right-click on element → "Inspect" or "Inspect Element"
2. Press F12 → Click element selector icon → Click element
3. Press Ctrl+Shift+C (Windows/Linux) or Cmd+Opt+C (Mac)

**What You See**:
- HTML structure
- Element attributes (id, class, name, etc.)
- CSS styles applied
- Computed styles
- Event listeners

**Finding Element Information for Automation**:

**Example**: Login button

**HTML**:
```html
<button id="loginBtn" class="btn btn-primary" type="submit">Login</button>
```

**Locator Strategies**:
1. **By ID**: `id="loginBtn"`
   - Selenium: `driver.findElement(By.id("loginBtn"))`

2. **By Class**: `class="btn btn-primary"`
   - Selenium: `driver.findElement(By.className("btn-primary"))`

3. **By CSS Selector**:
   - `button.btn-primary`
   - `#loginBtn`
   - `button[type='submit']`
   - Selenium: `driver.findElement(By.cssSelector("button.btn-primary"))`

4. **By XPath**:
   - `//button[@id='loginBtn']`
   - `//button[text()='Login']`
   - Selenium: `driver.findElement(By.xpath("//button[@id='loginBtn']"))`

**Tips for Testers**:
- ✅ ID is best (unique, fast)
- ✅ Name is good for form elements
- ✅ CSS selectors are fast and readable
- ✅ XPath is powerful but slower
- ❌ Avoid using full XPath (fragile)

---

## 4. CSS Basics

### 4.1 What is CSS?

**CSS (Cascading Style Sheets)**:
- Language for styling HTML elements
- Controls appearance: colors, fonts, layout, spacing
- Separates content (HTML) from presentation (CSS)

**Ways to Include CSS**:

**1. Inline CSS** (directly in HTML element):
```html
<p style="color: red; font-size: 20px;">Red text</p>
```

**2. Internal CSS** (in <style> tag):
```html
<head>
    <style>
        p {
            color: red;
            font-size: 20px;
        }
    </style>
</head>
```

**3. External CSS** (separate .css file):
```html
<head>
    <link rel="stylesheet" href="styles.css">
</head>
```

```css
/* styles.css */
p {
    color: red;
    font-size: 20px;
}
```

### 4.2 CSS Selectors

**Why Important for Testers?**
CSS selectors are used to:
- Locate elements in Selenium/Cypress
- Identify elements in browser DevTools
- Write locators for automation

**Basic Selectors**:

```css
/* Element Selector: Targets all <p> elements */
p {
    color: blue;
}

/* ID Selector: Targets element with id="header" */
#header {
    background-color: gray;
}

/* Class Selector: Targets all elements with class="btn" */
.btn {
    padding: 10px;
}

/* Universal Selector: Targets all elements */
* {
    margin: 0;
}
```

**Combining Selectors**:

```css
/* Multiple classes: Element has both classes */
.btn.primary {
    background-color: blue;
}

/* Descendant: <p> inside <div> */
div p {
    color: red;
}

/* Direct child: <p> that is direct child of <div> */
div > p {
    color: green;
}

/* Multiple selectors: Apply same style to multiple elements */
h1, h2, h3 {
    font-family: Arial;
}
```

**Attribute Selectors** (Very useful for testing):

```css
/* Has attribute */
input[required] {
    border: 2px solid red;
}

/* Exact attribute value */
input[type="text"] {
    width: 200px;
}

/* Attribute contains value */
a[href*="google"] {
    color: blue;
}

/* Attribute starts with */
a[href^="https"] {
    font-weight: bold;
}

/* Attribute ends with */
img[src$=".png"] {
    border: 1px solid black;
}
```

**Pseudo-classes** (State-based):

```css
/* Link states */
a:link { color: blue; }
a:visited { color: purple; }
a:hover { color: red; }
a:active { color: orange; }

/* Form states */
input:focus { border: 2px solid blue; }
input:disabled { background-color: gray; }
input:checked { /* for checkboxes/radio */ }

/* Nth child */
li:first-child { font-weight: bold; }
li:last-child { margin-bottom: 0; }
li:nth-child(2) { color: red; }
li:nth-child(odd) { background-color: #f0f0f0; }
li:nth-child(even) { background-color: white; }
```

### 4.3 CSS Selectors for Selenium

**Examples**:

**HTML**:
```html
<div id="loginForm" class="form-container">
    <input type="email" name="email" class="input-field" placeholder="Email">
    <input type="password" name="password" class="input-field" placeholder="Password">
    <button type="submit" class="btn btn-primary">Login</button>
</div>
```

**CSS Selectors**:

```java
// By ID
driver.findElement(By.cssSelector("#loginForm"));

// By class
driver.findElement(By.cssSelector(".form-container"));

// By element and class
driver.findElement(By.cssSelector("button.btn-primary"));

// By attribute
driver.findElement(By.cssSelector("input[type='email']"));
driver.findElement(By.cssSelector("input[name='password']"));
driver.findElement(By.cssSelector("input[placeholder='Email']"));

// By multiple attributes
driver.findElement(By.cssSelector("input[type='email'][name='email']"));

// Descendant
driver.findElement(By.cssSelector("#loginForm button"));

// Direct child
driver.findElement(By.cssSelector("div.form-container > button"));

// Nth element
driver.findElement(By.cssSelector("input:nth-of-type(1)")); // First input
driver.findElement(By.cssSelector("input:nth-of-type(2)")); // Second input

// Attribute contains
driver.findElement(By.cssSelector("button[class*='btn']"));

// Attribute starts with
driver.findElement(By.cssSelector("input[placeholder^='Email']"));
```

### 4.4 Common CSS Properties

**Text Properties**:
```css
p {
    color: #333333;           /* Text color */
    font-size: 16px;          /* Font size */
    font-family: Arial, sans-serif;  /* Font */
    font-weight: bold;        /* Weight (bold, normal, 100-900) */
    text-align: center;       /* Alignment (left, center, right) */
    text-decoration: underline;  /* Decoration */
    line-height: 1.5;         /* Line spacing */
}
```

**Box Model Properties**:
```css
div {
    width: 300px;             /* Width */
    height: 200px;            /* Height */
    padding: 10px;            /* Inner spacing */
    margin: 20px;             /* Outer spacing */
    border: 1px solid black;  /* Border */
    background-color: #f0f0f0;  /* Background color */
}
```

**Display Properties**:
```css
div {
    display: block;           /* Block-level element */
    display: inline;          /* Inline element */
    display: none;            /* Hidden */
    visibility: hidden;       /* Hidden but takes space */
}
```

---

## 5. JavaScript Fundamentals (Basic)

### 5.1 Why JavaScript for Testers?

**Reasons**:
1. **Browser Automation**: Selenium can execute JavaScript
2. **Cypress**: Uses JavaScript/TypeScript
3. **API Testing**: Postman uses JavaScript
4. **Web Understanding**: Know how websites work
5. **Debugging**: Understand errors in browser console
6. **Dynamic Elements**: Handle AJAX, dynamic content

### 5.2 JavaScript Basics

#### Variables

```javascript
// Declaring variables
var oldWay = "old style (avoid)";
let modernWay = "can be changed";
const constant = "cannot be changed";

// Examples
let username = "John";
let age = 25;
const apiUrl = "https://api.example.com";

// Re-assignment
username = "Jane";  // OK with let
// apiUrl = "new url";  // ERROR with const
```

#### Data Types

```javascript
// String
let name = "John Doe";
let message = 'Hello World';
let template = `Welcome, ${name}!`;  // Template literal

// Number
let age = 25;
let price = 19.99;

// Boolean
let isLoggedIn = true;
let hasAccess = false;

// Null and Undefined
let emptyValue = null;
let notDefined;  // undefined

// Array
let fruits = ["apple", "banana", "orange"];
let numbers = [1, 2, 3, 4, 5];

// Object
let user = {
    name: "John",
    age: 25,
    email: "john@example.com"
};

// Typeof
console.log(typeof name);      // "string"
console.log(typeof age);       // "number"
console.log(typeof isLoggedIn); // "boolean"
```

#### Operators

```javascript
// Arithmetic
let sum = 10 + 5;      // 15
let diff = 10 - 5;     // 5
let product = 10 * 5;  // 50
let quotient = 10 / 5; // 2
let remainder = 10 % 3; // 1

// Comparison
let isEqual = (10 == "10");   // true (type coercion)
let isStrictEqual = (10 === "10"); // false (no type coercion)
let isNotEqual = (10 != 5);   // true
let isGreater = (10 > 5);     // true
let isLess = (5 < 10);        // true

// Logical
let and = (true && false);    // false
let or = (true || false);     // true
let not = !true;              // false
```

#### Console.log for Debugging

```javascript
// Basic logging
console.log("Hello, World!");

// Log variables
let username = "John";
console.log("Username:", username);

// Log multiple values
let age = 25;
console.log("User:", username, "Age:", age);

// Log objects
let user = { name: "John", age: 25 };
console.log(user);

// Template literals
console.log(`User ${username} is ${age} years old`);
```

**Viewing Console Output**:
1. Press F12 to open DevTools
2. Click "Console" tab
3. See console.log() outputs
4. Type JavaScript directly and press Enter

#### Using Browser Console for Testing

**Examples**:

```javascript
// Get element by ID
let element = document.getElementById('loginBtn');
console.log(element);

// Get element by class
let elements = document.getElementsByClassName('btn');
console.log(elements[0]);

// Get element by CSS selector
let btn = document.querySelector('.btn-primary');
console.log(btn);

// Click element
btn.click();

// Get element value
let emailInput = document.getElementById('email');
console.log(emailInput.value);

// Set element value
emailInput.value = 'test@example.com';

// Get element text
let heading = document.querySelector('h1');
console.log(heading.textContent);

// Change element text
heading.textContent = "New Heading";
```

**Practical Testing Use Cases**:

**1. Test form submission without UI**:
```javascript
document.getElementById('email').value = 'test@example.com';
document.getElementById('password').value = 'Pass@123';
document.getElementById('loginBtn').click();
```

**2. Check if element exists**:
```javascript
let button = document.getElementById('submitBtn');
if (button) {
    console.log("Button exists");
} else {
    console.log("Button not found");
}
```

**3. Check element visibility**:
```javascript
let element = document.getElementById('errorMessage');
let isVisible = element && element.offsetHeight > 0;
console.log("Visible:", isVisible);
```

**4. Get all links on page**:
```javascript
let links = document.querySelectorAll('a');
console.log("Total links:", links.length);
links.forEach(link => console.log(link.href));
```

**5. Execute JavaScript in Selenium**:
```java
JavascriptExecutor js = (JavascriptExecutor) driver;

// Scroll to element
js.executeScript("arguments[0].scrollIntoView(true);", element);

// Click element
js.executeScript("arguments[0].click();", element);

// Get element text
String text = (String) js.executeScript("return arguments[0].textContent;", element);

// Set value
js.executeScript("arguments[0].value='test@example.com';", emailField);
```

---

## Summary

**Phase 1 Web Fundamentals** covered:

1. **Computer Basics**:
   - Operating systems (Windows, Linux, Mac)
   - File systems and directory structure
   - Command line basics
   - Process management

2. **Web Technologies**:
   - Client-server architecture
   - HTTP/HTTPS protocols
   - Request-response cycle
   - HTTP methods (GET, POST, PUT, DELETE)
   - Status codes (200, 404, 500, etc.)
   - Cookies and sessions
   - Browser Developer Tools

3. **HTML Basics**:
   - HTML structure
   - Common elements (div, span, input, button, form)
   - HTML attributes (id, class, name, value)
   - DOM structure
   - Inspecting elements

4. **CSS Basics**:
   - CSS selectors (id, class, attribute)
   - Understanding CSS properties
   - CSS selectors for test automation

5. **JavaScript Fundamentals**:
   - Variables and data types
   - Basic operators
   - Console.log for debugging
   - Browser console usage

**Why This Matters for Testers**:
- ✅ Understand how web applications work
- ✅ Debug issues using browser DevTools
- ✅ Find element locators for automation
- ✅ Test APIs using HTTP knowledge
- ✅ Verify cookies and sessions
- ✅ Execute JavaScript commands in automation
- ✅ Read and understand server logs

**Next Steps**:
- Practice with browser DevTools
- Inspect elements on real websites
- Try command line exercises
- Move to Phase 1 Practicals for hands-on work

---

*Master these fundamentals - they form the foundation for web testing and automation!*
