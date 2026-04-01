## Overview

This project simulates common high-priority SaaS support incidents and shows how they are investigated and resolved from a backend perspective.

The focus is not just on identifying failures, but on tracing them through logs, request data, and API behavior, then providing clear root cause explanations and practical fixes.

Stack: Node.js, Express.js, Postman, VS Code

### What this project demonstrates
• Handling real API failures (500, 403, latency issues)
• Reading and interpreting server-side behavior
• Using Postman to inspect requests and headers
• Turning vague customer complaints into clear technical findings
• Writing simple, useful root cause summaries
### Investigation Scenarios
1. 500 Internal Server Error

### Case: Invalid input crashing the API

### Issue
A user reported that submitting certain values in the User ID field caused the application to fail.

## Investigation
From the server logs in the terminal, the request was reaching the endpoint but failing during processing. The error showed that the system expected a numeric value but received a string, which triggered an unhandled exception.

### Root Cause
No input validation on the User ID field.

## Resolution
Added validation to check input type before processing.
The API now returns a structured response instead of crashing.

• Before: 500 Internal Server Error
• After: 400 Bad Request with clear message

Suggested screenshot
Postman response comparison:

• Before → 500 error
• After → clean 400 JSON response
2. 403 Forbidden

### Case: Access blocked despite valid login

### Issue
An admin user could not access the /admin route even after logging in.

## Investigation
Using Postman, I inspected the request headers and found that the API required a User-Role header. This was missing from the client request.

### Root Cause
Required authorization header not included in the request.

## Resolution

• Confirmed required header format
• Retested with User-Role: Admin
• Access restored with correct request
• Before: 403 Forbidden
• After: 200 OK

Suggested screenshot
Postman Headers tab showing User-Role: Admin and successful response

3. High Latency / Timeout Behavior

### Case: Slow API response affecting usability

### Issue
Users reported that the monthly report endpoint was taking too long to load.

### Investigation
Postman showed response times above 10 seconds. The request eventually completed, but the delay made the feature unreliable from a user perspective.

### Root Cause
Slow processing on the backend (likely query or computation delay).

### Resolution
No immediate failure to fix, but clearly identified as a performance issue.

### Recommended next steps:

• Review database query performance
• Consider background processing for report generation
• Introduce async handling if needed

### Outcome

• Status: 200 OK
• Problem: unacceptable response time

Suggested screenshot
Postman response showing 200 OK with response time >10s
