# Missing HTTP Strict Transport Security (HSTS)

Category: A04:2025 – Cryptographic Failures  
Severity: Medium  
Affected Endpoint: `/` (Application root – all HTTPS endpoints)


## Description

The application does not implement the HTTP Strict Transport Security (HSTS)
response header.

Although the application redirects HTTP requests to HTTPS, the absence of HSTS
allows an attacker to attempt HTTPS downgrade attacks (SSL stripping) during the
user’s initial connection.

Without HSTS, the browser is not instructed to enforce HTTPS-only communication,
leaving the first request vulnerable on untrusted networks.



## Proof of Concept (PoC)

1. Open the application in a browser.
2. Open Developer Tools → Network tab.
3. Refresh the page.
4. Inspect the **Document** request response headers.
5. Observe that the `Strict-Transport-Security` header is not present.
6. Access the application using `http://` and observe that it relies only on
   redirection, not browser-level enforcement.

![Missing HSTS Header](./screenshots/missing-hsts-header.png)



## Impact

- An attacker on the same network (e.g., public Wi-Fi) can attempt SSL stripping attacks.
- Users may be forced to interact with the application over HTTP during the first request.
- Sensitive data such as session cookies or authentication details could be exposed.



## When Severity Becomes HIGH

Severity escalates to **High** if:
- The application is directly accessible over HTTP without forced redirection, or
- Session cookies lack the `Secure` flag, or
- Sensitive actions (login, token exchange) are allowed over HTTP.



## Recommendation

- Enable HSTS by adding the following response header:
Strict-Transport-Security: max-age=31536000; includeSubDomains
- Ensure all sensitive cookies are marked as `Secure`.
- Enforce HTTPS at the browser level rather than relying only on redirects.
