# Endpoint Inventory

## Objective

This file is the main endpoint inventory for the OWASP crAPI local assessment.

The endpoints were observed while browsing the application normally and reviewing Burp Suite HTTP history.

## Evidence

```text
evidence/screenshots/reconnaissance/burp-http-history-crapi.png
Authentication and Identity Endpoints

Endpoint name:

Authentication verification

Method:

POST

Path:

/identity/api/auth/verify

Authentication required:

Observed during authenticated browsing

Observed status code:

200

Notes:

This endpoint was observed during the authenticated user flow.

Endpoint name:

User dashboard

Method:

GET

Path:

/identity/api/v2/user/dashboard

Authentication required:

Yes

Observed status code:

200

Notes:

This endpoint returns dashboard data for the authenticated user.

Endpoint name:

User videos

Method:

GET

Path:

/identity/api/v2/user/videos/0

Authentication required:

Observed during authenticated browsing

Observed status code:

404

Notes:

The endpoint was observed but returned 404 during this phase.
Vehicle Endpoints

Endpoint name:

User vehicles

Method:

GET

Path:

/identity/api/v2/vehicle/vehicles

Authentication required:

Yes

Observed status code:

200

Security relevance:

Relevant for BOLA and excessive data exposure testing because it returns vehicle-related data for the authenticated user.

Endpoint name:

Vehicle email resend

Method:

POST

Path:

/identity/api/v2/vehicle/resend_email

Authentication required:

Observed during authenticated browsing

Observed status code:

200

Security relevance:

Relevant for authentication, verification flow, and business logic review.
Shop Endpoints

Endpoint name:

Shop products

Method:

GET

Path:

/workshop/api/shop/products?limit=...

Authentication required:

Observed during authenticated browsing

Observed status code:

200

Notes:

This endpoint returns shop product data.

Endpoint name:

Create shop order

Method:

POST

Path:

/workshop/api/shop/orders

Authentication required:

Yes

Observed status code:

200

Observed request body:

{"product_id":2,"quantity":1}

Observed response:

{"id":7,"message":"Order sent successfully.","credit":80.0}

Security relevance:

Relevant for business logic testing, order flow review, and rate-limit observation.

Evidence:

evidence/requests/authentication/authenticated-request-format.txt
evidence/responses/authentication/authenticated-request-format-response.txt

Endpoint name:

User shop orders

Method:

GET

Path:

/workshop/api/shop/orders

Authentication required:

Yes

Observed status code:

200

Security relevance:

Relevant for authorization testing because it returns order data linked to the authenticated user.

Endpoint name:

All shop orders paginated

Method:

GET

Path:

/workshop/api/shop/orders/all?limit=...

Authentication required:

Yes

Observed status code:

200

Security relevance:

Relevant for authorization review because the endpoint name suggests broader order access.

Endpoint name:

Specific shop order

Method:

GET

Path:

/workshop/api/shop/orders/6

Authentication required:

Yes

Observed status code:

200

Security relevance:

Relevant for BOLA testing because it uses a direct object ID in the URL.
Community Endpoints

Endpoint name:

Community post data

Method:

GET

Path:

/community/api/v2/community/post...

Authentication required:

Observed during authenticated browsing

Observed status code:

200

Security relevance:

Relevant for object ID review and JSON response analysis if post metadata is exposed.
Priority Targets for Testing

Priority 1:

GET /workshop/api/shop/orders/6

Reason:

Direct object ID in the URL. Strong candidate for BOLA testing.

Priority 2:

GET /identity/api/v2/vehicle/vehicles

Reason:

Authenticated user vehicle data. Candidate for excessive data exposure and authorization review.

Priority 3:

GET /workshop/api/shop/orders/all?limit=...

Reason:

Order listing endpoint with broad naming. Candidate for authorization review.

Priority 4:

POST /workshop/api/shop/orders

Reason:

Business logic endpoint that creates orders and updates credit value.
Notes

This inventory is based on endpoints observed in Burp Suite during normal use.

More endpoints may be added later if they are observed during testing.
