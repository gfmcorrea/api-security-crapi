# Final Report — API Security Assessment Against OWASP crAPI

## Executive Summary

I performed a local API security assessment against OWASP crAPI.

The purpose of this project was to practice API security testing in a safe lab environment and document the full workflow in a professional GitHub portfolio format.

The assessment focused on:

* API reconnaissance
* Endpoint inventory
* Authentication flow mapping
* Authorization testing
* Object ID testing
* JSON response review
* Mass assignment testing
* Business logic validation
* Evidence collection
* Token redaction
* Security reporting

All testing was performed locally against:

```text
http://localhost:8888
```

MailHog was available at:

```text
http://localhost:8025
```

No external targets were tested.

## Scope

In scope:

* OWASP crAPI running locally
* API endpoints observed during normal usage
* Local test accounts
* Authentication and authorization flows
* Vehicle, user, shop, and order-related endpoints
* Safe low-volume testing

Out of scope:

* External systems
* Real third-party APIs
* Denial-of-service testing
* Malware
* Persistence
* Evasion
* Phishing
* Real data exfiltration
* Destructive actions
* Public internet scanning
* Attacks against systems without permission

## Environment

The lab environment used:

* Ubuntu
* Docker
* Docker Compose
* OWASP crAPI
* Burp Suite Community Edition
* Firefox
* curl
* jq
* Nmap
* Git
* GitHub

Environment evidence:

```text
Docker containers screenshot: evidence/screenshots/environment/docker-containers-running.png
crAPI homepage screenshot: evidence/screenshots/environment/crapi-homepage.png
MailHog screenshot: evidence/screenshots/environment/mailhog-homepage.png
Docker Compose output: evidence/tool-outputs/docker-compose-ps.txt
Nmap output: evidence/tool-outputs/nmap-local-services.txt
```

## Methodology

The methodology was based on:

* OWASP API Security Top 10 2023
* OWASP WSTG concepts where useful
* PTES-inspired assessment structure
* Evidence-based validation

Workflow:

1. Start local crAPI lab.
2. Configure Burp Suite and Firefox.
3. Register local test users.
4. Capture authentication requests.
5. Build endpoint inventory.
6. Identify object IDs and user context.
7. Capture baseline requests.
8. Modify one value at a time.
9. Compare responses.
10. Save evidence.
11. Redact sensitive values.
12. Document confirmed findings, observations, and not confirmed tests.

## Test Accounts

Two local users were created for authorization testing.

User A:

```text
Name: Gabriel User A
Email: gabriel.usera@crapi.local
Role: Normal user
```

User B:

```text
Name: Gabriel User B
Email: gabriel.userb@crapi.local
Role: Normal user
```

Purpose:

```text
User A was used as the baseline user.
User B was used to test cross-user access and authorization behavior.
```

## Reconnaissance Summary

During reconnaissance, I browsed the application normally and captured traffic in Burp Suite.

Observed API areas included:

* Authentication and identity endpoints
* User dashboard endpoints
* Vehicle endpoints
* Shop product endpoints
* Shop order endpoints
* Community endpoints

Important observed endpoints:

```text
POST /identity/api/auth/verify
GET /identity/api/v2/user/dashboard
GET /identity/api/v2/vehicle/vehicles
POST /identity/api/v2/vehicle/resend_email
GET /workshop/api/shop/products?limit=...
POST /workshop/api/shop/orders
GET /workshop/api/shop/orders
GET /workshop/api/shop/orders/all?limit=...
GET /workshop/api/shop/orders/{id}
GET /community/api/v2/community/post...
```

Reconnaissance evidence:

```text
Burp HTTP history screenshot: evidence/screenshots/reconnaissance/burp-http-history-crapi.png
Observed routes: api-recon/api-routes-observed.md
Endpoint inventory: endpoint-inventory/endpoint-inventory.md
```

## Authentication Flow Summary

Authentication testing included:

* Registration flow
* Login flow
* Token usage
* Authenticated request format
* Invalid login attempt observation

Authenticated requests used a bearer token:

```text
Authorization: Bearer [REDACTED_TOKEN]
```

Authentication evidence:

```text
api-recon/authentication-flow.md
api-recon/user-roles-and-test-accounts.md
evidence/requests/authentication/
evidence/responses/authentication/
evidence/screenshots/authentication/
```

## Findings Summary

| ID | Finding                                                              |        Status |      Severity | OWASP Mapping |
| -- | -------------------------------------------------------------------- | ------------: | ------------: | ------------- |
| 01 | Broken Object Level Authorization                                    |     Confirmed |          High | API1:2023     |
| 02 | Broken Authentication / Rate Limit Observation                       |   Observation |           Low | API2:2023     |
| 03 | Excessive Data Exposure / Broken Object Property Level Authorization |     Confirmed |        Medium | API3:2023     |
| 04 | Broken Function Level Authorization                                  | Not Confirmed | Informational | API5:2023     |
| 05 | Mass Assignment                                                      | Not Confirmed | Informational | API3:2023     |
| 06 | Business Logic Credit Validation                                     | Not Confirmed | Informational | API6:2023     |

## Finding 01 — Broken Object Level Authorization

Status:

```text
Confirmed
```

Severity:

```text
High
```

OWASP API Top 10 Mapping:

```text
API1:2023 Broken Object Level Authorization
```

Affected endpoint:

```text
GET /workshop/api/shop/orders/{id}
```

Summary:

```text
A normal authenticated user was able to access another user's order by changing the order ID in the URL.
```

Evidence showed that User B could access User A's order data through a direct object reference.

The exposed data included:

* User email
* User phone number
* Order details
* Product details
* Payment metadata
* Masked card number
* Card owner name
* Card type
* Card expiry

Evidence:

```text
findings/01-broken-object-level-authorization.md
evidence/requests/bola/
evidence/responses/bola/
evidence/screenshots/bola/
```

## Finding 02 — Broken Authentication / Rate Limit Observation

Status:

```text
Observation
```

Severity:

```text
Low
```

OWASP API Top 10 Mapping:

```text
API2:2023 Broken Authentication
```

Tested endpoint:

```text
POST /identity/api/auth/login
```

Summary:

```text
The login endpoint returned the same 401 response after five invalid password attempts.
```

The API returned:

```json
{"token":null,"type":"","message":"Invalid Credentials","mfaRequired":false}
```

This was documented as a safe low-volume observation, not a confirmed vulnerability.

Evidence:

```text
findings/02-broken-authentication.md
evidence/requests/broken-authentication/
evidence/responses/broken-authentication/
evidence/screenshots/broken-authentication/
```

## Finding 03 — Excessive Data Exposure / Broken Object Property Level Authorization

Status:

```text
Confirmed
```

Severity:

```text
Medium
```

OWASP API Top 10 Mapping:

```text
API3:2023 Broken Object Property Level Authorization
```

Affected endpoint:

```text
GET /workshop/api/shop/orders/{id}
```

Summary:

```text
The order details endpoint returned more data than necessary, including user contact information and payment-related metadata.
```

The response included:

* User email
* User phone number
* Transaction ID
* Payment transaction ID
* Paid date
* Masked card number
* Card owner name
* Card type
* Card expiry
* Currency

This issue increased the impact of the confirmed BOLA finding because unauthorized access to an order also exposed additional sensitive object properties.

Evidence:

```text
findings/03-excessive-data-exposure.md
evidence/requests/excessive-data-exposure/
evidence/responses/excessive-data-exposure/
evidence/screenshots/excessive-data-exposure/
```

## Finding 04 — Broken Function Level Authorization

Status:

```text
Not Confirmed
```

Severity:

```text
Informational
```

OWASP API Top 10 Mapping:

```text
API5:2023 Broken Function Level Authorization
```

Tested endpoint:

```text
GET /workshop/api/shop/orders/all?limit=30&offset=0
```

Summary:

```text
The endpoint name looked sensitive because it included orders/all, but the response returned only the authenticated user's own order data.
```

The endpoint was reviewed because it looked like a broader order listing function.

Observed result:

```text
User B received only User B's own order.
No User A orders were returned.
```

Evidence:

```text
findings/04-broken-function-level-authorization.md
evidence/requests/bfla/
evidence/responses/bfla/
evidence/screenshots/bfla/
```

## Finding 05 — Mass Assignment

Status:

```text
Not Confirmed
```

Severity:

```text
Informational
```

OWASP API Top 10 Mapping:

```text
API3:2023 Broken Object Property Level Authorization
```

Tested endpoint:

```text
POST /workshop/api/shop/orders
```

Summary:

```text
Extra JSON properties were accepted in the request body, but the tested fields did not modify protected server-side values.
```

Tested baseline body:

```json
{"product_id":2,"quantity":1}
```

Tested modified bodies:

```json
{"product_id":2,"quantity":1,"price":0}
```

```json
{"product_id":2,"quantity":1,"credit":9999}
```

Observed result:

```text
The backend ignored the unexpected fields for price and credit calculation.
```

Evidence:

```text
findings/05-mass-assignment.md
evidence/requests/mass-assignment/
evidence/responses/mass-assignment/
evidence/screenshots/mass-assignment/
```

## Finding 06 — Business Logic Credit Validation

Status:

```text
Not Confirmed
```

Severity:

```text
Informational
```

OWASP API Top 10 Mapping:

```text
API6:2023 Unrestricted Access to Sensitive Business Flows
```

Tested endpoint:

```text
POST /workshop/api/shop/orders
```

Summary:

```text
The API allowed purchases while the user had enough credit, but blocked the next purchase after the credit reached 0.0.
```

Observed sequence:

```text
Credit 20.0 -> 10.0: order accepted.
Credit 10.0 -> 0.0: order accepted.
Credit 0.0 -> new order: blocked with 400 Bad Request.
```

Blocked response:

```json
{"message":"Insufficient Balance. Please apply coupons to get more balance!"}
```

Evidence:

```text
findings/06-rate-limit-or-business-logic-observation.md
evidence/requests/business-logic/
evidence/responses/business-logic/
evidence/screenshots/business-logic/
```

## Impact Analysis

The highest-impact confirmed issue was Broken Object Level Authorization.

The BOLA finding allowed an authenticated user to access another user's order object by changing the order ID.

The Excessive Data Exposure finding increased the impact of BOLA because the order details response included user contact information and payment-related metadata.

The other tested areas were documented as observations or not confirmed results because the evidence did not support marking them as confirmed vulnerabilities.

This distinction is important because professional security reporting should avoid false positives.

## Remediation Summary

Main remediation themes:

* Enforce server-side object-level authorization.
* Validate object ownership before returning order data.
* Return only required fields in API responses.
* Apply field-level authorization for sensitive object properties.
* Keep payment metadata minimized.
* Add automated tests for cross-user object access.
* Maintain login abuse protections.
* Keep order listing endpoints scoped to the authenticated user.
* Continue ignoring or rejecting unexpected JSON fields.
* Keep server-side credit validation enforced.

Full remediation file:

```text
remediation/remediation-summary.md
```

## Evidence Handling

Evidence was saved under:

```text
evidence/requests/
evidence/responses/
evidence/screenshots/
evidence/tool-outputs/
```

Redaction standard:

```text
Authorization: Bearer [REDACTED_TOKEN]
Cookie: [REDACTED_COOKIE]
Set-Cookie: [REDACTED_COOKIE]
token: [REDACTED_TOKEN]
access_token: [REDACTED_TOKEN]
refresh_token: [REDACTED_TOKEN]
password: [REDACTED_PASSWORD]
transaction_id: *EXAMPLE*
```

Before publishing, I reviewed evidence for:

* JWT tokens
* Cookies
* Passwords
* Reset tokens
* Verification tokens
* Private secrets

## Limitations

This assessment was limited to OWASP crAPI running locally.

No external systems were tested.

No high-volume brute force or denial-of-service testing was performed.

Only endpoints observed during the assessment were documented.

Findings were marked confirmed only when evidence supported them.

## Lessons Learned

This project helped me understand that API testing depends heavily on context.

The most important steps were:

* Mapping the authentication flow
* Understanding object ownership
* Capturing clean baseline requests
* Modifying one value at a time
* Comparing responses carefully
* Avoiding false positives
* Redacting evidence before publishing

I also learned that not every test becomes a confirmed vulnerability.

Documenting not confirmed tests and observations is still useful because it shows methodology, honesty, and evidence-based reporting.

## Conclusion

This project shows a complete API security testing workflow against a local vulnerable API lab.

It demonstrates how I performed reconnaissance, mapped endpoints, tested authorization, reviewed JSON responses, collected evidence, and wrote professional findings.

The confirmed findings were:

```text
Broken Object Level Authorization
Excessive Data Exposure / Broken Object Property Level Authorization
```

The observations and not confirmed tests were documented clearly to avoid false positives.

The project was completed safely in a local environment and is intended for portfolio and learning purposes.
