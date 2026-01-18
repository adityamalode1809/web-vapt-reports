# Improper Session Termination Allows Authentication Token Reuse After Logout

Category: A07:2025 – Authentication Failures  
Severity: High  
Affected Endpoint: `GET /v1/comments/product/{id}`  
(Authentication-protected API)

---

## Description

The application fails to properly terminate authenticated sessions on logout.

After a user logs out via the application UI, the previously issued JWT
authentication token remains valid and continues to be accepted by the server.
As a result, protected API endpoints can still be accessed using the old token.

This indicates improper session termination and missing server-side token
invalidation controls.

---

## Proof of Concept (PoC)

1. Login to the application with a valid user account.
2. Capture an authenticated request to:
GET /v1/comments/product/1
3. Send the request to **Burp Repeater** and confirm it returns **200 OK**.
4. Logout from the application using the normal UI logout functionality.
5. Replay the same request from Burp Repeater **without modifying the JWT token**.
6. Observe that the server still responds with **200 OK** and returns comment data.

![JWT Token Reuse After Logout PoC](./screenshot/jwt-token-reuse-after-logout.png)

---

## Impact

- Logout mechanism becomes ineffective from a security perspective.
- Enables session hijacking and token replay attacks if a token is compromised.
- Allows unauthorized access to protected resources after user logout.
- Increases risk of account takeover and sensitive data exposure.
- Violates secure authentication and session management best practices.

---

## Recommendation

- Implement server-side token invalidation on logout.
- Maintain a JWT revocation or blacklist mechanism.
- Use short-lived access tokens with strict expiration enforcement.
- Invalidate tokens on logout, password change, and privilege updates.
- Monitor and log reuse of invalidated or expired tokens.


