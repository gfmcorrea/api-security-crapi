# User Roles and Test Accounts

## Objective

This file documents the local test accounts used during the OWASP crAPI API security assessment.

The goal was to keep authorization testing organized and separate User A objects from User B objects.

## Test Account Rules

I used only local lab accounts.

No real personal accounts were used.

Passwords are not published.

Tokens and cookies are redacted before publishing.

## User A

Purpose:

```text
Baseline authenticated user.

Details:

Name: Gabriel User A
Email: gabriel.usera@crapi.local
Password: [REDACTED_PASSWORD]
Role: Normal user

Evidence:

Registration request: evidence/requests/authentication/user-a-registration-request.txt
Registration response: evidence/responses/authentication/user-a-registration-response.txt
Login request: evidence/requests/authentication/user-a-login-request.txt
Login response: evidence/responses/authentication/user-a-login-response.txt
User B

Purpose:

Authorization comparison user.

Details:

Name: Gabriel User B
Email: gabriel.userb@crapi.local
Password: [REDACTED_PASSWORD]
Role: Normal user

Evidence:

Registration request: evidence/requests/authentication/user-b-registration-request.txt
Registration response: evidence/responses/authentication/user-b-registration-response.txt
Login request: evidence/requests/authentication/user-b-login-request.txt
Login response: evidence/responses/authentication/user-b-login-response.txt
Privileged or Admin User
No privileged or admin user was confirmed during this phase.
Role Observations
Only normal user accounts were used during this phase.
Authenticated Request Evidence

The authenticated request selected for this phase was:

POST /workshop/api/shop/orders

Evidence:

Request: evidence/requests/authentication/authenticated-request-format.txt
Response: evidence/responses/authentication/authenticated-request-format-response.txt
Screenshot: evidence/screenshots/authentication/authenticated-request-format-burp.png
Authorization Testing Use

User A and User B will be used to test:

Object ownership
Vehicle access
User profile access
Shop or order access
Function-level authorization
Cross-user access control
Notes

Each account was used only inside the local OWASP crAPI lab.

All credentials, tokens, and cookies must be redacted before publishing.
