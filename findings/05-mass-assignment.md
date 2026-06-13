# Finding 05 — Mass Assignment

## Status

Not Confirmed

## Severity

Informational

## CVSS Estimate

CVSS was not assigned because Mass Assignment was not confirmed for this endpoint.

## Tested Endpoint

```text
POST /workshop/api/shop/orders
```

## Tested Method

```text
POST
```

## Tested Object

```text
Shop order creation
```

## OWASP API Top 10 Mapping

```text
API3:2023 Broken Object Property Level Authorization
```

## Description

I tested the shop order creation endpoint for Mass Assignment by adding unexpected JSON properties to the request body.

The baseline request used only the expected fields:

```json
{"product_id":2,"quantity":1}
```

Then I tested two modified requests:

```json
{"product_id":2,"quantity":1,"price":0}
```

```json
{"product_id":2,"quantity":1,"credit":9999}
```

The API still created orders successfully, but the extra fields did not appear to change the order price or the user credit in the response.

## Technical Details

Baseline request:

```text
POST /workshop/api/shop/orders
```

Baseline body:

```json
{"product_id":2,"quantity":1}
```

Baseline response:

```json
{"id":11,"message":"Order sent successfully.","credit":40.0}
```

Modified request with extra `price` property:

```json
{"product_id":2,"quantity":1,"price":0}
```

Response:

```json
{"id":12,"message":"Order sent successfully.","credit":30.0}
```

Modified request with extra `credit` property:

```json
{"product_id":2,"quantity":1,"credit":9999}
```

Response:

```json
{"id":13,"message":"Order sent successfully.","credit":20.0}
```

## Observed Result

The API accepted the requests and created new orders.

However, the unexpected fields did not appear to control the server-side price or credit value.

The credit value decreased by 10 after each order:

```text
Baseline order: credit 40.0
Request with price=0: credit 30.0
Request with credit=9999: credit 20.0
```

This indicates that the backend calculated the order cost server-side and did not trust the submitted `price` or `credit` fields.

## Conclusion

Mass Assignment was not confirmed for the tested shop order endpoint.

The endpoint accepted extra JSON properties, but the tested fields did not appear to modify protected server-side values.

This result was documented as a testing note instead of a confirmed vulnerability.

## Evidence

Baseline request:

```text
evidence/requests/mass-assignment/create-order-baseline-request.txt
```

Baseline response:

```text
evidence/responses/mass-assignment/create-order-baseline-response.txt
```

Extra price request:

```text
evidence/requests/mass-assignment/create-order-extra-price-request.txt
```

Extra price response:

```text
evidence/responses/mass-assignment/create-order-extra-price-response.txt
```

Extra credit request:

```text
evidence/requests/mass-assignment/create-order-extra-credit-request.txt
```

Extra credit response:

```text
evidence/responses/mass-assignment/create-order-extra-credit-response.txt
```

Screenshots:

```text
evidence/screenshots/mass-assignment/create-order-baseline.png
evidence/screenshots/mass-assignment/create-order-extra-price.png
evidence/screenshots/mass-assignment/create-order-extra-credit.png
```

## Recommended Secure Behavior

The API should continue to reject or ignore unexpected fields that users should not control.

Recommended actions:

* Use an allowlist of accepted request fields.
* Reject unexpected JSON properties when possible.
* Do not bind client input directly to internal server-side objects.
* Keep price and credit calculation on the server side.
* Add automated tests for unexpected properties such as `price`, `credit`, `role`, `isAdmin`, and `balance`.

## Notes

This test was performed only in the local OWASP crAPI lab.

No external systems were tested.

No destructive actions were performed.
