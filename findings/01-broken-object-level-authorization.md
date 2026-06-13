# Finding 01 — Broken Object Level Authorization

## Status

Confirmed

## Severity

High

## CVSS Estimate

CVSS v3.1 estimate: 6.5 Medium

Vector:

```text
AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:N/A:N
```

Note:

```text
The business severity was classified as High because the issue exposed another user's order and payment-related metadata.
```

## Affected Endpoint

```text
GET /workshop/api/shop/orders/7
```

## Affected Method

```text
GET
```

## Affected Object

```text
Order ID: 7
```

## OWASP API Top 10 Mapping

```text
API1:2023 Broken Object Level Authorization
```

## Description

The API allowed one authenticated user to access another user's order by directly requesting the order ID in the URL.

User A created order ID 7.

Then, using User B's authenticated context, I sent a request to the same endpoint:

```text
GET /workshop/api/shop/orders/7
```

The API returned `200 OK` and exposed User A's order details to User B.

## Technical Details

Baseline test:

```text
User A accessed User A's own order ID 7.
```

Modified test:

```text
User B accessed User A's order ID 7 by using the same direct object reference.
```

Expected secure behavior:

```text
The API should verify that the authenticated user owns the requested order before returning order details.
```

Observed behavior:

```text
The API returned the same User A order data to User B.
```

## Baseline Request

Evidence file:

```text
evidence/requests/bola/get-order-7-user-a-baseline-request.txt
```

Summary:

```http
GET /workshop/api/shop/orders/7 HTTP/1.1
Host: localhost:8888
Authorization: Bearer [REDACTED_TOKEN]
```

## Baseline Response

Evidence file:

```text
evidence/responses/bola/get-order-7-user-a-baseline-response.txt
```

Relevant response data:

```json
{
  "order": {
    "id": 7,
    "user": {
      "email": "gabriel.usera@crapi.local",
      "number": "11999990001"
    },
    "product": {
      "id": 2,
      "name": "Wheel",
      "price": "10.00"
    },
    "quantity": 1,
    "status": "delivered"
  },
  "payment": {
    "order_id": 7,
    "amount": 10,
    "card_number": "XXXXXXXXXXXX1632",
    "card_owner_name": "Gabriel User A",
    "card_type": "Visa",
    "card_expiry": "11/2030",
    "currency": "USD"
  }
}
```

## Modified Request

Evidence file:

```text
evidence/requests/bola/get-order-7-user-b-modified-request.txt
```

Summary:

```http
GET /workshop/api/shop/orders/7 HTTP/1.1
Host: localhost:8888
Authorization: Bearer [REDACTED_TOKEN]
```

The token used in this modified request belonged to User B.

## Modified Response

Evidence file:

```text
evidence/responses/bola/get-order-7-user-b-modified-response.txt
```

Relevant response data:

```json
{
  "order": {
    "id": 7,
    "user": {
      "email": "gabriel.usera@crapi.local",
      "number": "11999990001"
    },
    "product": {
      "id": 2,
      "name": "Wheel",
      "price": "10.00"
    },
    "quantity": 1,
    "status": "delivered"
  },
  "payment": {
    "order_id": 7,
    "amount": 10,
    "card_number": "XXXXXXXXXXXX1632",
    "card_owner_name": "Gabriel User A",
    "card_type": "Visa",
    "card_expiry": "11/2030",
    "currency": "USD"
  }
}
```

## Steps to Reproduce

1. Start the OWASP crAPI local lab.
2. Log in as User A.
3. Create a shop order.
4. Open the order details page and capture the request in Burp Suite.
5. Confirm that User A can access the order:

```text
GET /workshop/api/shop/orders/7
```

6. Save the baseline request and response.
7. Log out from User A.
8. Log in as User B.
9. Capture an authenticated request from User B to obtain User B's authorization context.
10. Replay the same order details request using User B's token:

```text
GET /workshop/api/shop/orders/7
```

11. Observe that the API returns `200 OK`.
12. Confirm that the response still contains User A's order, phone number, and payment-related metadata.
13. Save the modified request and response.
14. Redact tokens and cookies before publishing.

## Observed Result

The API returned User A's order details when the request was sent using User B's authenticated context.

The response exposed:

* User A email
* User A phone number
* Order ID
* Product details
* Order status
* Payment transaction metadata
* Masked card number
* Card owner name
* Card type
* Card expiry

## Technical Impact

An authenticated user can access another user's order by changing or reusing the order ID in the URL.

This indicates that the API does not properly enforce object ownership before returning order details.

## Business Impact

In a real application, this issue could allow customers to view other customers' order and payment-related data.

This could lead to:

* Customer privacy exposure
* Unauthorized access to order history
* Exposure of payment metadata
* Loss of user trust
* Compliance and regulatory risk

## Recommended Remediation

Implement server-side object-level authorization checks for order access.

Recommended actions:

* Use the authenticated user ID from the token or session.
* Check whether the requested order belongs to the authenticated user.
* Return `403 Forbidden` or `404 Not Found` when the user does not own the order.
* Do not rely on frontend restrictions.
* Add automated tests for cross-user order access.
* Log unauthorized access attempts to order objects.
* Review other endpoints that use direct object IDs.

## Evidence

Baseline request:

```text
evidence/requests/bola/get-order-7-user-a-baseline-request.txt
```

Baseline response:

```text
evidence/responses/bola/get-order-7-user-a-baseline-response.txt
```

Modified request:

```text
evidence/requests/bola/get-order-7-user-b-modified-request.txt
```

Modified response:

```text
evidence/responses/bola/get-order-7-user-b-modified-response.txt
```

Screenshots:

```text
evidence/screenshots/bola/get-order-7-user-a-baseline.png
evidence/screenshots/bola/get-order-7-user-b-modified.png
```

## Limitations

This finding was validated only in the local OWASP crAPI lab.

No external systems were tested.

The test was limited to order ID 7 and the observed shop order endpoint.

## Conclusion

Broken Object Level Authorization was confirmed on the shop order details endpoint.

The API returned User A's order and payment-related metadata when the same order ID was requested using User B's authenticated context.
