# Admin or Sensitive Endpoints

## Objective

This file documents admin-like, privileged, or sensitive endpoints observed during the OWASP crAPI assessment.

No confirmed admin endpoint was identified during this phase.

## Evidence

```text
evidence/screenshots/reconnaissance/burp-http-history-crapi.png
Sensitive Endpoint Candidates
GET /workshop/api/shop/orders/all?limit=...

Reason:

The endpoint path includes "all", so it should be reviewed carefully to confirm whether it only returns data allowed for the authenticated user.

Authentication required:

Yes

Observed status code:

200

Testing status:

Candidate for authorization review.
POST /identity/api/v2/vehicle/resend_email

Reason:

This endpoint appears related to a verification or email resend flow.

Authentication required:

Observed during authenticated browsing.

Observed status code:

200

Testing status:

Candidate for business logic or authentication flow review.
POST /workshop/api/shop/orders

Reason:

This endpoint creates a shop order and changes the user's credit value.

Authentication required:

Yes

Observed status code:

200

Testing status:

Candidate for business logic testing.
Admin Endpoint Status
No privileged or admin-only endpoint was confirmed during this phase.
Notes

These endpoints are not confirmed vulnerabilities.

They are only sensitive candidates selected for later testing based on observed behavior and endpoint purpose.
