# Authentication Flow

## Objective

This file documents the authentication flow observed during the OWASP crAPI local API security assessment.

The goal was to understand how registration, login, token usage, and authenticated API requests worked before testing authorization issues.

## Test Accounts

User A:

```text
Email: gabriel.usera@crapi.local
Password: [REDACTED_PASSWORD]
Role: Normal user
Purpose: Baseline account

User B:

Email: gabriel.userb@crapi.local
Password: [REDACTED_PASSWORD]
Role: Normal user
Purpose: Authorization comparison account
Registration Flow

Two local test users were created in the OWASP crAPI lab.

User A registration evidence:

Request: evidence/requests/authentication/user-a-registration-request.txt
Response: evidence/responses/authentication/user-a-registration-response.txt
Screenshot: evidence/screenshots/authentication/user-a-registration-burp.png

User B registration evidence:

Request: evidence/requests/authentication/user-b-registration-request.txt
Response: evidence/responses/authentication/user-b-registration-response.txt
Screenshot: evidence/screenshots/authentication/user-b-registration-burp.png

Observed result:

Both local test users were created successfully in the crAPI lab.
Login Flow

Both users were used to authenticate through the crAPI login flow.

User A login evidence:

Request: evidence/requests/authentication/user-a-login-request.txt
Response: evidence/responses/authentication/user-a-login-response.txt
Screenshot: evidence/screenshots/authentication/user-a-login-burp.png

User B login evidence:

Request: evidence/requests/authentication/user-b-login-request.txt
Response: evidence/responses/authentication/user-b-login-response.txt
Screenshot: evidence/screenshots/authentication/user-b-login-burp.png

Observed result:

Both users were able to authenticate successfully.
Authenticated Request Format

The authenticated request selected as evidence was a shop order request.

Evidence:

Request: evidence/requests/authentication/authenticated-request-format.txt
Response: evidence/responses/authentication/authenticated-request-format-response.txt
Screenshot: evidence/screenshots/authentication/authenticated-request-format-burp.png

Observed authenticated request:

POST /workshop/api/shop/orders HTTP/1.1
Host: localhost:8888
Authorization: Bearer [REDACTED_TOKEN]
Cookie: [REDACTED_COOKIE]
Content-Type: application/json

Request body:

{"product_id":2,"quantity":1}

Observed response:

{"id":7,"message":"Order sent successfully.","credit":80.0}
Token Handling

The application uses a bearer token in authenticated API requests.

The token was sent in the HTTP Authorization header.

Before saving evidence to the repository, the token was replaced with:

Authorization: Bearer [REDACTED_TOKEN]

Cookies were also redacted:

Cookie: [REDACTED_COOKIE]
MailHog and Verification Flow

MailHog was available during the assessment at:

http://localhost:8025

MailHog evidence from the environment setup phase:

evidence/screenshots/environment/mailhog-homepage.png

Authentication and verification-related API traffic was observed in Burp HTTP history during normal application usage.

Email verification was not documented as a separate security finding in this phase.

Logout Behavior

Logout behavior was not fully assessed during this phase.

Password Reset or Recovery Flow

Password reset was not assessed during this phase.

Role and User Context

User A:

Email: gabriel.usera@crapi.local
Role: Normal user
Purpose: Baseline authenticated user

User B:

Email: gabriel.userb@crapi.local
Role: Normal user
Purpose: Authorization comparison user
Test Account Separation

Two separate local accounts were created to support authorization testing.

User A will be used as the baseline account.

User B will be used to compare object ownership and cross-user access behavior.

Redaction Standard

Before publishing, sensitive values were redacted using this format:

Authorization: Bearer [REDACTED_TOKEN]
Cookie: [REDACTED_COOKIE]
Set-Cookie: [REDACTED_COOKIE]
token: [REDACTED_TOKEN]
access_token: [REDACTED_TOKEN]
refresh_token: [REDACTED_TOKEN]
password: [REDACTED_PASSWORD]
Notes

Authentication mapping was completed before authorization testing.

This made it easier to understand which requests were valid and which requests were modified during later testing.
