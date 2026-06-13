# Authentication Endpoints

## Objective

This file documents authentication and identity-related endpoints observed during the assessment.

## Evidence

```text
evidence/screenshots/reconnaissance/burp-http-history-crapi.png
evidence/requests/authentication/authenticated-request-format.txt
evidence/responses/authentication/authenticated-request-format-response.txt
POST /identity/api/auth/verify

Purpose:

Verify authentication-related data during the user flow.

Authentication required:

Observed during authenticated browsing.

Observed status code:

200

Notes:

This endpoint appeared in Burp HTTP history during normal authenticated browsing.
GET /identity/api/v2/user/dashboard

Purpose:

Retrieve dashboard data for the authenticated user.

Authentication required:

Yes

Observed status code:

200

Notes:

This endpoint is user-specific and should only return data for the authenticated user.
GET /identity/api/v2/user/videos/0

Purpose:

Retrieve user-related video or training content.

Authentication required:

Observed during authenticated browsing.

Observed status code:

404

Notes:

The endpoint was observed but returned 404 during this phase.
Authenticated Request Format

The authenticated request selected for evidence was:

POST /workshop/api/shop/orders

It used the following authentication format:

Authorization: Bearer [REDACTED_TOKEN]
Cookie: [REDACTED_COOKIE]

Evidence:

Request: evidence/requests/authentication/authenticated-request-format.txt
Response: evidence/responses/authentication/authenticated-request-format-response.txt
Screenshot: evidence/screenshots/authentication/authenticated-request-format-burp.png
Notes

Authentication and identity endpoints were documented before authorization testing.

This helps confirm which requests are authenticated and which user context is being used.
