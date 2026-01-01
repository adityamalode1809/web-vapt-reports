# Horizontal Privilege Escalation via Comments API

Category: A01:2025 – Broken Access Control  
Severity: High  
Affected Endpoints:  
- GET /v1/comments/product/{product_id}  
- POST /v1/comments  

Authentication Required: Yes (Low-privileged user)



## Description

The application does not properly enforce **horizontal access control** across
comment-related API endpoints.

An authenticated low-privileged user can:
- Access comment data belonging to other users
- Create comments on behalf of other users

This is possible by manipulating object identifiers such as `product_id`
and user-controlled parameters like `userId` in API requests.

The issue represents a **Horizontal Privilege Escalation**, where users at the
same privilege level can access or modify each other’s data.



## Proof of Concept (PoC)

### Horizontal READ

1. Login as a normal user (e.g., `userId: 5936`).
2. Intercept the request:
3. Forward the request without modification.
4. Observe that the response contains comments associated with **other user IDs**.

![Horizontal Read PoC](horizontal-read-poc.png)



### Horizontal WRITE

1. Intercept the request:
2. Modify the `userId` parameter to another valid user ID.
3. Forward the request to the server.
4. Observe that the server responds with **HTTP 201 Created**.

![Horizontal Write PoC](./screenshots/horizontal-write-poc.png)



## Impact

- Unauthorized access to other users’ data
- User impersonation via parameter manipulation
- Compromise of application data integrity
- Potential abuse leading to loss of user trust



## Root Cause

- Missing horizontal authorization checks
- Blind trust in client-supplied identifiers
- Lack of object ownership validation
- Failure to bind actions to authenticated user context



## Remediation

- Enforce strict server-side authorization checks
- Derive user identity exclusively from session or JWT
- Validate object ownership before read/write operations
- Ignore client-supplied user identifiers
- Return proper authorization errors (HTTP 403)

