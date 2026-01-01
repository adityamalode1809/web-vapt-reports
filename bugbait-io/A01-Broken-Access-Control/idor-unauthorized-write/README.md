# Broken Access Control – IDOR (Unauthorized Write)

Category: A01:2025 – Broken Access Control  
Severity: High  
Affected Endpoint: POST /v1/comments  
Authentication Required: Yes (Low-privileged user)


## Description

The application fails to enforce proper server-side authorization checks
on comment-related API endpoints.

An authenticated low-privileged user can manipulate the `userId` parameter
in the request body and create comments on behalf of other users.  
The backend trusts client-supplied identifiers instead of deriving the user
identity from the authenticated session.

This results in an **IDOR (Insecure Direct Object Reference)** vulnerability
leading to unauthorized write actions.



## Proof of Concept (PoC)

1. Login as a normal user.
2. Intercept the request to:
3. Modify the `userId` parameter to another valid user ID.
4. Forward the request to the server.
5. Observe that the server responds with **HTTP 201 Created**, confirming
unauthorized comment creation.

 



## Impact

- An attacker can impersonate other users by creating comments on their behalf.
- Malicious or misleading content can be injected under another user’s identity.
- Application data integrity is compromised due to unauthorized write actions.



## Root Cause

- Missing server-side authorization checks on write operations.
- Blind trust in client-supplied object identifiers (`userId`).
- Failure to bind actions to the authenticated user context.



## Recommendation

- Derive authenticated user identity exclusively from the session or JWT.
- Ignore client-supplied parameters such as `userId` for authorization decisions.
- Implement strict server-side authorization checks for all write operations.
- Validate object ownership before processing requests.

