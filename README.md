## Overview

This project simulates common high-priority SaaS support incidents and shows how they are investigated and resolved from a backend perspective.

The focus is not just on identifying failures, but on tracing them through logs, request data, and API behavior, then providing clear root cause explanations and practical fixes.

Stack: Node.js, Express.js, Postman, VS Code

### What this project demonstrates
* Handling real API failures (500, 403, latency issues)
* Reading and interpreting server-side behavior
* Using Postman to inspect requests and headers
* Turning vague customer complaints into clear technical findings
* Writing simple, useful root cause summaries

---

### Investigation Scenarios

#### 1. 500 Internal Server Error
![image1](https://raw.githubusercontent.com/Paulinus26/API-Failure-Triage-Investigation-Lab/bf9260dd3764298254c95d829e5b7579a1ce96af/internal-error-response.png) 
*Figure 1: 500-error-server-crash.png.*

**Case: Invalid input crashing the API**

**Issue**
A user reported that submitting certain values in the User ID field caused the application to fail.

**Investigation**
From the server logs in the terminal, the request was reaching the endpoint but failing during processing. The error showed that the system expected a numeric value but received a string, which triggered an unhandled exception.

![Image 1](https://raw.githubusercontent.com/Paulinus26/API-Failure-Triage-Investigation-Lab/8cc2a91df4bd5445d4057d7d53e1809d30bf9a59/Error%20message.png) 
*Figure 2: VS Code Terminal Stack Trace identifying the code failure at server.js:15*

**Root Cause**
No input validation on the User ID field.

**Resolution**
Added validation to check input type before processing. The API now returns a structured response instead of crashing.
* **Before:** 500 Internal Server Error
* **After:** 400 Bad Request with clear message

![image 2](https://raw.githubusercontent.com/Paulinus26/API-Failure-Triage-Investigation-Lab/f619b29e18f62215096215266eeaa89001e4dff4/Bad%20request.png) 
*Figure 3: 400-bad-request-fix.png*

#### 2. 403 Forbidden

**Case: Access blocked despite valid login**

**Issue**
An admin user could not access the /admin route even after logging in.

**Investigation**
Using Postman, I inspected the request headers and found that the API required a User-Role header. This was missing from the client request.

**Root Cause**
Required authorization header not included in the request.

**Resolution**

• Confirmed required header format
• Retested with User-Role: Admin
• Access restored with correct request
• Before: 403 Forbidden
• After: 200 OK

![image3](https://raw.githubusercontent.com/Paulinus26/API-Failure-Triage-Investigation-Lab/de783d360b5b83c6ad78bb6cb336f22039e7db59/Image%20403%20forbidden.png) 
*Figure 4: Successful 200 OK response after applying correct User-Role parameters*

#### 3. High Latency / Timeout Behavior

**Case: Slow API response affecting usability**

**Issue**
Users reported that the monthly report endpoint was taking too long to load.

**Investigation**
Postman showed response times above 10 seconds. The request eventually completed, but the delay made the feature unreliable from a user perspective.

**Root Cause**
Slow processing on the backend (likely query or computation delay).

**Resolution**
No immediate failure to fix, but clearly identified as a performance issue.

**Recommended next steps:**
* Review database query performance
* Consider background processing for report generation
* Introduce async handling if needed

**Outcome**
* **Status:** 200 OK
* **Problem:** Unacceptable response time (10.07s)

![image5](https://raw.githubusercontent.com/Paulinus26/API-Failure-Triage-Investigation-Lab/0e359416793657f950c8e891e65c6887f53f9e04/performance%20latency.png) 
*Figure 5: Postman Metrics showing 10.07s response time (High Latency)*

## Conclusion

This project reflects how I approach support work when things break in production. I start with what the user is seeing, reproduce it, and follow the request through until the failure point is clear. From there, I focus on giving a direct explanation of what went wrong and what needs to change.

Across these scenarios, the issues were different, but the approach stayed consistent. Check the request, read the logs, confirm the behavior, and avoid assumptions. Some fixes were immediate, like input validation. Others required escalation, such as performance bottlenecks. In both cases, the goal is to remove ambiguity and move the issue forward with useful context.

The outcome is not just a resolved ticket, but a clearer system. Fewer repeated issues, better error handling, and more predictable API behavior. That is the standard I aim for in a support role.
