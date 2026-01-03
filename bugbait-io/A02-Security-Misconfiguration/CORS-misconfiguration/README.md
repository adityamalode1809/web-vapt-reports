# CORS Misconfiguration Allowing Arbitrary Origins with Credentials

Category: A02:2025 – Security Misconfiguration  
Severity: High  
Affected Endpoint: POST /v1/auth/login  


## Description

The application implements an insecure Cross-Origin Resource Sharing (CORS)
configuration.

The API reflects arbitrary origins supplied in the `Origin` request header
and allows credentials in cross-origin requests. This allows a malicious
website to read sensitive responses in the context of an authenticated user.



## Proof of Concept (PoC)

1. Login as a normal user.
2. Intercept the request:
POST /v1/auth/login
3. Add the following header:
Origin: https://evil.com
4. Forward the request to the server.
5. Observe that the response headers allow the malicious origin and credentials.

### Observed Response Headers


Access-Control-Allow-Origin: https://evil.com
Access-Control-Allow-Credentials: true


![CORS Origin Reflection PoC](./screenshot/cors-origin-reflection-poc.png)



## Impact

- A malicious website can read sensitive API responses on behalf of an authenticated user.
- This may lead to unauthorized data access, session abuse, or account compromise.
- The vulnerability weakens browser-enforced same-origin protections.



## Recommendation

- Restrict `Access-Control-Allow-Origin` to trusted domains only.
- Do not reflect user-supplied origins in CORS responses.
- Never enable `Access-Control-Allow-Credentials` for arbitrary or wildcard origins.
- Implement a strict CORS policy based on business requirements.


