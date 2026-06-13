# Shop Endpoints

## Objective

This file documents shop and order-related endpoints observed during the OWASP crAPI assessment.

Shop endpoints are useful for authorization, BOLA, business logic, and rate-limit testing.

## Evidence

```text
evidence/screenshots/reconnaissance/burp-http-history-crapi.png
evidence/requests/authentication/authenticated-request-format.txt
evidence/responses/authentication/authenticated-request-format-response.txt
GET /workshop/api/shop/products?limit=...

Purpose:

Retrieve shop product listings.

Authentication required:

Observed during authenticated browsing.

Observed status code:

200

Notes:

This endpoint returns product data used by the shop page.
POST /workshop/api/shop/orders

Purpose:

Create a shop order.

Authentication required:

Yes

Observed status code:

200

Observed request body:

{"product_id":2,"quantity":1}

Observed response:

{"id":7,"message":"Order sent successfully.","credit":80.0}

Security relevance:

This endpoint is relevant for business logic testing because it creates an order and updates the user's credit value.

Evidence:

Request: evidence/requests/authentication/authenticated-request-format.txt
Response: evidence/responses/authentication/authenticated-request-format-response.txt
Screenshot: evidence/screenshots/authentication/authenticated-request-format-burp.png
GET /workshop/api/shop/orders

Purpose:

Retrieve shop orders for the authenticated user.

Authentication required:

Yes

Observed status code:

200

Security relevance:

This endpoint is relevant for authorization testing because order data should be limited to the authenticated user.
GET /workshop/api/shop/orders/all?limit=...

Purpose:

Retrieve paginated order data.

Authentication required:

Yes

Observed status code:

200

Security relevance:

This endpoint should be reviewed carefully because the path includes "all", which may indicate broader order access.
GET /workshop/api/shop/orders/6

Purpose:

Retrieve a specific shop order by order ID.

Authentication required:

Yes

Observed status code:

200

Security relevance:

This endpoint is a strong candidate for BOLA testing because the order ID is directly included in the URL.
Priority for Next Testing

Primary BOLA candidate:

GET /workshop/api/shop/orders/6

Primary business logic candidate:

POST /workshop/api/shop/orders

Primary authorization review candidate:

GET /workshop/api/shop/orders/all?limit=...
Notes

No shop vulnerability was confirmed in this phase.

This file only documents reconnaissance results and candidate endpoints for later testing.
