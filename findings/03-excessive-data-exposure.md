# Finding 03 — Excessive Data Exposure / Broken Object Property Level Authorization

## Status

Confirmed

## Severity

Medium

## CVSS Estimate

CVSS v3.1 estimate: 5.3 Medium

Vector:

```text
AV:N/AC:L/PR:L/UI:N/S:U/C:L/I:N/A:N
Affected Endpoint
GET /workshop/api/shop/orders/7
Affected Method
GET
Affected Object
Order details and payment metadata
OWASP API Top 10 Mapping
API3:2023 Broken Object Property Level Authorization
Description

The order details endpoint returned more information than necessary in the API response.

When requesting order ID 7, the API returned order details, user contact information, and payment-related metadata in the same response.

The response included the user's email address, phone number, payment transaction metadata, masked card number, card owner name, card type, and card expiry date.

Technical Details

Endpoint tested:

GET /workshop/api/shop/orders/7

The response exposed the following fields:

order.user.email
order.user.number
order.transaction_id
payment.transaction_id
payment.order_id
payment.amount
payment.paid_on
payment.card_number
payment.card_owner_name
payment.card_type
payment.card_expiry
payment.currency

Expected secure behavior:

The API should return only the fields required by the frontend for the order details page.

Observed behavior:

The API returned user contact information and payment-related metadata together with the order details.
Request

Evidence file:

evidence/requests/excessive-data-exposure/get-order-7-details-request.txt

Summary:

GET /workshop/api/shop/orders/7 HTTP/1.1
Host: localhost:8888
Authorization: Bearer [REDACTED_TOKEN]
Response

Evidence file:

evidence/responses/excessive-data-exposure/get-order-7-details-response.txt

Relevant response data:

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
      "price": "10.00",
      "image_url": "images/wheel.svg"
    },
    "quantity": 1,
    "status": "delivered",
    "transaction_id": "0example-4308-ae1b-d3ea8cae8c8e",
    "created_on": "2026-06-13T00:04:06.673849"
  },
  "payment": {
    "transaction_id": "047591e4-9f64-4308-example",
    "order_id": 7,
    "amount": 10,
    "paid_on": "2026-06-13T00:04:06.673849",
    "card_number": "XXXXXXXXXXXX1632",
    "card_owner_name": "Gabriel User A",
    "card_type": "Visa",
    "card_expiry": "11/2030",
    "currency": "USD"
  }
}
Steps to Reproduce
Start the OWASP crAPI local lab.
Log in as User A.
Open the shop order details page.
Capture the order details request in Burp Suite.
Send the request:
GET /workshop/api/shop/orders/7
Review the JSON response.
Confirm that the response includes user contact information and payment-related metadata.
Save the request, response, and screenshot.
Redact tokens and cookies before publishing.
Observed Result

The API returned order details together with user and payment-related fields.

The exposed fields included:

User email
User phone number
Order transaction ID
Payment transaction ID
Paid date
Masked card number
Card owner name
Card type
Card expiry
Currency
Technical Impact

The API exposes more object properties than necessary for the tested order details flow.

This increases the amount of sensitive or semi-sensitive data available in API responses and can make other attacks more impactful.

In this project, the issue is especially relevant because it can be chained with the confirmed BOLA finding. If another user can access the order object, the excessive response fields increase the amount of exposed data.

Business Impact

In a real application, this issue could increase privacy and compliance risk.

Even when card numbers are masked, exposing payment metadata, card owner name, card expiry, user phone number, and transaction identifiers can increase the impact of unauthorized access.

Possible business impact:

Increased customer privacy exposure
Higher impact if combined with authorization flaws
Exposure of unnecessary payment metadata
Loss of user trust
Compliance review concerns
Recommended Remediation

Return only the fields required by the frontend.

Recommended actions:

Remove unnecessary user contact fields from order detail responses.
Remove payment metadata that is not required by the client.
Do not expose transaction identifiers unless required.
Avoid returning card expiry date unless there is a clear business need.
Use response DTOs or serializers to control exposed fields.
Apply field-level authorization.
Review all order and payment API responses for unnecessary fields.
Add automated tests to prevent sensitive fields from being returned unintentionally.
Evidence

Request:

evidence/requests/excessive-data-exposure/get-order-7-details-request.txt

Response:

evidence/responses/excessive-data-exposure/get-order-7-details-response.txt

Screenshot:

evidence/screenshots/excessive-data-exposure/get-order-7-excessive-data-response.png
Limitations

This finding was validated only in the local OWASP crAPI lab.

No external systems were tested.

The assessment reviewed the observed order details response for order ID 7.

Conclusion

Excessive Data Exposure was confirmed on the order details endpoint.

The API returned user contact information and payment-related metadata that should be minimized or protected through field-level response control.
