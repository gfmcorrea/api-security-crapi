# Finding 04 — Broken Function Level Authorization

## Status

Not Confirmed

## Severity

Informational

## CVSS Estimate

CVSS was not assigned because Broken Function Level Authorization was not confirmed.

## Tested Endpoint

```text
GET /workshop/api/shop/orders/all?limit=30&offset=0
```

## Tested Method

```text
GET
```

## Tested Function

```text
Paginated order listing
```

## OWASP API Top 10 Mapping

```text
API5:2023 Broken Function Level Authorization
```

## Description

I tested the order listing endpoint because the path included `orders/all`, which looked like a potentially sensitive function.

The goal was to check whether a normal authenticated user could access a broader order listing function and retrieve orders from other users.

The test was performed using User B's authenticated context.

## Request

Evidence file:

```text
evidence/requests/bfla/get-orders-all-user-b-request.txt
```

Request summary:

```http
GET /workshop/api/shop/orders/all?limit=30&offset=0 HTTP/1.1
Host: localhost:8888
Authorization: Bearer [REDACTED_TOKEN]
```

## Response

Evidence file:

```text
evidence/responses/bfla/get-orders-all-user-b-response.txt
```

Response summary:

```json
{
  "orders": [
    {
      "id": 16,
      "user": {
        "email": "gabriel.userb@crapi.local",
        "number": "11999990002"
      },
      "product": {
        "id": 2,
        "name": "Wheel",
        "price": "10.00",
        "image_url": "images/wheel.svg"
      },
      "quantity": 1,
      "status": "delivered",
      "transaction_id": "examplesdaasfa",
      "created_on": "2026-06-13T15:39:20.929960"
    }
  ],
  "next_offset": null,
  "previous_offset": null,
  "count": 1
}
```

## Observed Result

The endpoint returned HTTP 200 OK.

However, the response only included User B's own order:

```text
Order ID: 16
User email: gabriel.userb@crapi.local
Phone: 11999990002
```

No User A orders were returned in this response.

## Security Interpretation

Broken Function Level Authorization was not confirmed for this endpoint during this test.

Even though the endpoint name included `orders/all`, the API response appeared scoped to the authenticated user.

This suggests that the endpoint may use `all` to mean all orders for the current authenticated user, not all orders across all users.

## Technical Impact

No unauthorized function access was confirmed in this test.

The tested endpoint did not expose another user's order list through this request.

## Business Impact

No direct business impact was confirmed from this test.

The result is still useful because it documents that the endpoint was reviewed due to its sensitive-looking path.

## Recommended Secure Behavior

The API should continue enforcing user-level authorization on order listing endpoints.

Recommended actions:

* Keep order list responses scoped to the authenticated user.
* Avoid ambiguous endpoint names such as `orders/all` if the endpoint only returns the current user's data.
* Add automated tests to confirm normal users cannot list orders from other accounts.
* Keep privileged order listing functions separated from user order listing functions.
* Enforce role-based authorization on any real administrative order listing endpoint.

## Evidence

Request:

```text
evidence/requests/bfla/get-orders-all-user-b-request.txt
```

Response:

```text
evidence/responses/bfla/get-orders-all-user-b-response.txt
```

Screenshot:

```text
evidence/screenshots/bfla/get-orders-all-user-b.png
```

## Limitations

This test was performed only in the local OWASP crAPI lab.

No administrator account was confirmed during this phase.

The test used User B's authenticated context and reviewed whether the endpoint returned data from other users.

## Conclusion

Broken Function Level Authorization was not confirmed for the tested order listing endpoint.

The endpoint returned only User B's own order data, so the result was documented as an authorization review note instead of a confirmed vulnerability.
