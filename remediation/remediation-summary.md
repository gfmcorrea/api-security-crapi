# Remediation Summary

## Objective

This file summarizes remediation recommendations for the API security issues and observations documented in this project.

The recommendations are written for a development or AppSec team.

## Finding 01 — Broken Object Level Authorization

Status:

```text
Confirmed
```

Affected endpoint:

```text
GET /workshop/api/shop/orders/{id}
```

Risk:

```text
Authenticated users may access order objects that belong to other users by changing the order ID in the URL.
```

Recommended remediation:

* Enforce object ownership checks on every order details endpoint.
* Use the authenticated user ID from the token or session.
* Verify that the requested order belongs to the authenticated user before returning it.
* Return `403 Forbidden` or `404 Not Found` when the user does not own the requested order.
* Do not rely on frontend route restrictions.
* Avoid exposing predictable object IDs when possible.
* Add automated tests for cross-user object access.
* Log unauthorized object access attempts.
* Review all endpoints that use direct object references.

Example secure logic:

```text
1. Extract authenticated user ID from token.
2. Look up requested order ID.
3. Compare order.owner_id with authenticated_user.id.
4. Return the order only if ownership matches.
5. Otherwise return 403 or 404.
```

## Finding 03 — Excessive Data Exposure / Broken Object Property Level Authorization

Status:

```text
Confirmed
```

Affected endpoint:

```text
GET /workshop/api/shop/orders/{id}
```

Risk:

```text
The API returns more user and payment-related fields than necessary in the order details response.
```

Exposed data included:

* User email
* User phone number
* Order transaction ID
* Payment transaction ID
* Payment date
* Masked card number
* Card owner name
* Card type
* Card expiry
* Currency

Recommended remediation:

* Return only fields required by the frontend.
* Remove unnecessary user contact fields from order detail responses.
* Remove payment metadata that is not needed by the client.
* Avoid returning card expiry date unless there is a clear business need.
* Avoid returning transaction identifiers unless required.
* Use response DTOs or serializers to control exposed fields.
* Separate internal database models from public API response models.
* Apply field-level authorization for sensitive object properties.
* Add automated tests to prevent sensitive fields from being returned unintentionally.
* Review all order, payment, profile, and vehicle API responses for unnecessary fields.

Example safer response:

```json
{
  "order": {
    "id": 7,
    "product": {
      "name": "Wheel",
      "price": "10.00",
      "image_url": "images/wheel.svg"
    },
    "quantity": 1,
    "status": "delivered",
    "created_on": "2026-06-13T00:04:06.673849"
  }
}
```

## Finding 02 — Broken Authentication / Rate Limit Observation

Status:

```text
Observation
```

Tested endpoint:

```text
POST /identity/api/auth/login
```

Risk:

```text
During a safe low-volume test, the login endpoint returned the same 401 response after five invalid password attempts.
```

Recommended remediation:

* Apply rate limiting to login endpoints.
* Add monitoring for repeated failed login attempts.
* Add alerting for suspicious authentication patterns.
* Consider progressive delays after repeated failed attempts.
* Consider temporary account protection mechanisms when appropriate.
* Keep authentication error messages generic.
* Log failed login attempts with useful security context.
* Avoid exposing whether an email exists.
* Add automated tests for repeated failed login behavior.

Recommended production controls:

```text
Rate limiting
Account protection
Authentication monitoring
Suspicious login alerting
Credential stuffing detection
Generic error messages
Security logging
```

## Finding 04 — Broken Function Level Authorization

Status:

```text
Not Confirmed
```

Tested endpoint:

```text
GET /workshop/api/shop/orders/all?limit=30&offset=0
```

Observed result:

```text
The endpoint returned only User B's own order data. No User A orders were returned.
```

Recommended secure behavior:

* Keep order list responses scoped to the authenticated user.
* Avoid ambiguous endpoint names such as `orders/all` if the endpoint only returns the current user's orders.
* Separate user order listing endpoints from administrative order listing endpoints.
* Enforce role-based authorization on any real administrative order listing endpoint.
* Add automated tests confirming normal users cannot list orders from other accounts.
* Log access to sensitive or administrative listing functions.

## Finding 05 — Mass Assignment

Status:

```text
Not Confirmed
```

Tested endpoint:

```text
POST /workshop/api/shop/orders
```

Tested unexpected properties:

```json
{"price":0}
```

```json
{"credit":9999}
```

Observed result:

```text
The API accepted the requests, but the tested fields did not change protected server-side values.
```

Recommended secure behavior:

* Use an allowlist of accepted request fields.
* Reject unexpected JSON properties when possible.
* Do not bind client input directly to internal server-side objects.
* Keep price and credit calculation on the server side.
* Apply field-level authorization for sensitive properties.
* Add automated tests for unexpected properties such as `price`, `credit`, `role`, `isAdmin`, and `balance`.
* Log suspicious submissions containing unexpected sensitive fields.

Example allowlist:

```text
Allowed fields:
- product_id
- quantity

Rejected or ignored fields:
- price
- credit
- role
- isAdmin
- balance
- verified
```

## Finding 06 — Business Logic Credit Validation

Status:

```text
Not Confirmed
```

Tested endpoint:

```text
POST /workshop/api/shop/orders
```

Observed result:

```text
The API allowed purchases while the user had enough credit, but blocked a new purchase after the credit reached 0.0.
```

Blocked response:

```json
{"message":"Insufficient Balance. Please apply coupons to get more balance!"}
```

Recommended secure behavior:

* Keep credit validation on the backend.
* Do not trust client-side balance values.
* Reject purchases when the user does not have enough balance.
* Validate product price and user balance inside the same trusted transaction.
* Prevent race conditions in purchase flows.
* Log rejected purchase attempts.
* Add automated tests for insufficient balance scenarios.
* Review coupon and credit increase flows separately.

## General API Security Recommendations

### Object-Level Authorization

* Enforce object ownership checks on all object-specific endpoints.
* Do not trust object IDs supplied by the client.
* Use the authenticated user context for authorization decisions.
* Deny access by default.
* Add tests for horizontal privilege escalation.

### Field-Level Authorization

* Return only the fields required for each user flow.
* Avoid exposing internal IDs, transaction IDs, debug fields, or sensitive metadata.
* Use response DTOs or serializers.
* Review API responses from a privacy and least-privilege perspective.

### Authentication Protection

* Rate limit login and password reset endpoints.
* Monitor failed login attempts.
* Keep error messages generic.
* Protect tokens from exposure in logs, URLs, and frontend storage.
* Expire and rotate tokens appropriately.

### Function-Level Authorization

* Separate normal user functions from administrative functions.
* Enforce permissions in the API layer.
* Do not rely only on frontend visibility.
* Add tests for normal user access to sensitive functions.

### Input Validation

* Validate JSON schemas.
* Reject unexpected fields when possible.
* Use strict allowlists for request bodies.
* Keep sensitive calculations on the server side.
* Avoid direct model binding from client-controlled input.

### Business Logic Protection

* Validate business rules server-side.
* Prevent replay of sensitive actions when needed.
* Add anti-abuse controls to sensitive flows.
* Validate state transitions.
* Test edge cases such as insufficient balance, duplicate actions, and invalid object ownership.

### Logging and Monitoring

Log security-relevant events such as:

* Failed login attempts
* Authorization failures
* Cross-user object access attempts
* Repeated sensitive actions
* Unexpected JSON properties
* Insufficient balance purchase attempts

Avoid logging:

* Passwords
* Tokens
* Full cookies
* Sensitive payment data

## Final Note

The main remediation priority is server-side authorization.

The most important confirmed issue in this project was Broken Object Level Authorization. Fixing object ownership validation would significantly reduce the risk of unauthorized order access.

The second major priority is response minimization. Even when authorization is fixed, APIs should still avoid returning unnecessary user and payment-related fields.
