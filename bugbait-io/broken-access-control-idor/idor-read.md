# IDOR – Unauthorized Read via Product ID Manipulation

## Category: 
A01:2025 – Broken Access Control

## Severity
Medium

## Affected Endpoint
GET /v1/comments/product/{id}

## Description
The application does not enforce proper authorization checks
on the comments API. An authenticated user can modify the
product_id parameter and read comments of other products.

## Proof of Concept
1. Login as a normal user
2. Intercept:
   GET /v1/comments/product/3
3. Change product_id to another value
4. Server returns unauthorized data

## Impact
Unauthorized access to comment data of other products.

## Recommendation
Implement server-side authorization checks.
Do not trust client-supplied identifiers.



![IDOR Read PoC](screenshots/idor-read-poc.png)
