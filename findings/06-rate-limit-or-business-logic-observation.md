# Finding 06 — Business Logic Credit Validation

## Status

Not Confirmed

## Severity

Informational

## CVSS Estimate

CVSS was not assigned because the business logic issue was not confirmed.

## Tested Endpoint

```text
POST /workshop/api/shop/orders
```

## Tested Method

```text
POST
```

## Tested Flow

```text
Shop order creation and user credit validation
```

## OWASP API Top 10 Mapping

```text
API6:2023 Unrestricted Access to Sensitive Business Flows
```

## Description

I tested whether the shop order endpoint allowed purchases after the user's available credit reached zero.

The test was performed by sending repeated valid order creation requests with the same product and quantity.

The goal was to check whether the API would continue processing purchases even when the user no longer had enough credit.

## Request Used

Evidence file:

```text
evidence/requests/business-logic/create-order-credit-validation-request.txt
```

Request body:

```json
{"product_id":2,"quantity":1}
```

## Test Results

First request:

```text
Response file: evidence/responses/business-logic/create-order-credit-20-to-10-response.txt
```

Response:

```json
{"id":14,"message":"Order sent successfully.","credit":10.0}
```

Second request:

```text
Response file: evidence/responses/business-logic/create-order-credit-10-to-0-response.txt
```

Response:

```json
{"id":15,"message":"Order sent successfully.","credit":0.0}
```

Third request after credit reached zero:

```text
Response file: evidence/responses/business-logic/create-order-credit-zero-test-response.txt
```

Response:

```json
{"message":"Insufficient Balance. Please apply coupons to get more balance!"}
```

## Observed Result

The API allowed purchases while the user still had enough credit.

When the user's credit reached `0.0`, the next purchase attempt was blocked with:

```text
HTTP/1.1 400 Bad Request
```

The response message was:

```text
Insufficient Balance. Please apply coupons to get more balance!
```

## Conclusion

Business logic abuse was not confirmed for this tested flow.

The API correctly blocked the order creation request after the user no longer had enough balance.

This behavior indicates that the backend performs a server-side credit validation before processing new orders.

## Evidence

Request:

```text
evidence/requests/business-logic/create-order-credit-validation-request.txt
```

Responses:

```text
evidence/responses/business-logic/create-order-credit-20-to-10-response.txt
evidence/responses/business-logic/create-order-credit-10-to-0-response.txt
evidence/responses/business-logic/create-order-credit-zero-test-response.txt
```

Screenshots:

```text
evidence/screenshots/business-logic/create-order-credit-20-to-10.png
evidence/screenshots/business-logic/create-order-credit-10-to-0.png
evidence/screenshots/business-logic/create-order-credit-zero-test.png
```

## Recommended Secure Behavior

The API should continue enforcing server-side balance validation.

Recommended actions:

* Keep credit validation on the backend.
* Do not trust client-side balance values.
* Reject purchases when the user does not have enough balance.
* Log rejected purchase attempts.
* Add automated tests for insufficient balance scenarios.
* Review coupon and credit increase flows separately.

## Notes

This test was performed only in the local OWASP crAPI lab.

No denial-of-service testing was performed.

The test used a small number of valid requests only.
