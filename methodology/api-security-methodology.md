# API Security Methodology

## Objective

This methodology was used to guide the OWASP crAPI API security assessment.

The goal was to follow a structured, evidence-based API testing process.

## Methodology Sources

This project was based on concepts from:

- OWASP API Security Top 10 2023
- OWASP WSTG
- PTES-inspired assessment structure
- Manual API testing with Burp Suite

## Phase 1 — Environment Setup

The local lab was started with Docker and Docker Compose.

Evidence collected:

```text
evidence/tool-outputs/docker-compose-ps.txt
evidence/screenshots/environment/docker-containers-running.png
evidence/screenshots/environment/crapi-homepage.png
evidence/screenshots/environment/mailhog-homepage.png
Phase 2 — Authentication Mapping

The authentication flow was mapped before authorization testing.

Activities:

Created User A
Created User B
Captured registration requests
Captured login requests
Identified bearer token usage
Saved authenticated request format

Evidence:

api-recon/authentication-flow.md
api-recon/user-roles-and-test-accounts.md
evidence/requests/authentication/
evidence/responses/authentication/
Phase 3 — API Reconnaissance

The application was browsed normally while traffic was captured in Burp Suite.

Activities:

Reviewed Burp HTTP history
Identified API base paths
Mapped observed endpoints
Grouped endpoints by function
Selected high-value testing targets

Evidence:

api-recon/api-routes-observed.md
endpoint-inventory/endpoint-inventory.md
Phase 4 — Authorization Testing

Authorization testing focused on object ownership and cross-user access.

Method:

Capture a valid request as the object owner.
Identify object IDs in the request.
Switch to another user context.
Replay the same request with the second user's token.
Compare the response.

Confirmed result:

Broken Object Level Authorization

Evidence:

findings/01-broken-object-level-authorization.md
evidence/requests/bola/
evidence/responses/bola/
Phase 5 — Response Data Review

JSON responses were reviewed for unnecessary fields.

Confirmed result:

Excessive Data Exposure / Broken Object Property Level Authorization

Evidence:

findings/03-excessive-data-exposure.md
evidence/requests/excessive-data-exposure/
evidence/responses/excessive-data-exposure/
Phase 6 — Authentication Observation

The login endpoint was tested with a small number of invalid login attempts.

This was documented as an observation, not a confirmed vulnerability.

Evidence:

findings/02-broken-authentication.md
evidence/requests/broken-authentication/
evidence/responses/broken-authentication/
Phase 7 — BFLA Review

A sensitive-looking endpoint was reviewed:

GET /workshop/api/shop/orders/all?limit=30&offset=0

Result:

Broken Function Level Authorization was not confirmed.

Evidence:

findings/04-broken-function-level-authorization.md
Phase 8 — Mass Assignment Testing

Unexpected JSON properties were added to the order creation request.

Tested properties:

price
credit

Result:

Mass Assignment was not confirmed.

Evidence:

findings/05-mass-assignment.md
Phase 9 — Business Logic Testing

The shop order flow was tested to verify credit validation.

Result:

Business logic abuse was not confirmed.
The API blocked orders after the user's credit reached 0.0.

Evidence:

findings/06-rate-limit-or-business-logic-observation.md
Phase 10 — Reporting

The final phase focused on documentation.

Documents updated:

README.md
reports/final-report.md
remediation/remediation-summary.md
lessons-learned/lessons-learned.md
lessons-learned/skills-demonstrated.md
CAREER-MATERIAL.md
Finding Status Rules

Confirmed:

Used only when evidence proved the issue.

Observation:

Used when behavior was interesting but not enough to confirm a vulnerability.

Not Confirmed:

Used when the test did not prove a vulnerability.

Final Note

This methodology prioritized careful testing, clean evidence, and honest reporting.
