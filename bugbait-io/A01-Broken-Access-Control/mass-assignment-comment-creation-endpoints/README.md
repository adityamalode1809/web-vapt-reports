# Mass Assignment via Comment Creation Endpoint

Category: A01:2025 – Broken Access Control  
Severity: High  
Affected Endpoint: /v1/comments  
Authentication Required: Yes (Low-privileged user)

## Description

The application does not enforce strict server-side control over which parameters
are accepted by the comment creation API. As a result, an authenticated
low-privileged user can supply additional, unauthorized fields in the request
body, which are processed by the backend without restriction.

This behavior indicates a Mass Assignment (Over-posting) vulnerability, where
client-supplied input is blindly mapped to backend objects instead of being
restricted to an explicit allowlist.


## Proof of Concept (PoC)

1. Login as a normal user.
2. Intercept the request to:
   POST /v1/comments
3. Modify the request body by adding additional parameters that should be
   controlled by the server.
4. Forward the request to the server.
5. Observe that the server responds with HTTP 201 Created, confirming that the
   request was processed successfully.
   
  ![Mass Assignment PoC](screenshots/mass-assignment-poc.png)


## Impact

- An attacker can inject unauthorized attributes into backend objects.
- Sensitive fields intended to be server-controlled can be manipulated by the client.
- This may lead to privilege escalation, data integrity compromise, or abuse of
  business logic.
- The vulnerability demonstrates weak access control enforcement.

## Root Cause

- Missing server-side parameter allowlisting.
- Blind trust in client-supplied JSON input.
- Direct binding of request payloads to internal models.
- Lack of authorization validation on object attributes.


## Recommendation

- Implement strict server-side allowlists for accepted request parameters.
- Reject or ignore any unexpected or unauthorized fields in incoming requests.
- Derive sensitive attributes such as user identity and privilege level
  exclusively from the authenticated session or JWT.
- Apply schema-based validation or Data Transfer Objects (DTOs) for request bodies.

## Notes

- The issue is fully reproducible.
- Acceptance of unauthorized fields alone is sufficient to confirm the vulnerability.
- No race condition or timing dependency is involved.


## Disclaimer

This testing was performed on an intentionally vulnerable application for
educational purposes only.
