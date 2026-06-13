# API Routes Observed

## Objective

This file documents the API routes observed while browsing OWASP crAPI normally through the browser and capturing traffic in Burp Suite.

The goal of this phase was to build an initial API route map before testing specific vulnerabilities.

## Evidence

Burp HTTP history screenshot:

```text
evidence/screenshots/reconnaissance/burp-http-history-crapi.png

Authenticated request evidence:

evidence/requests/authentication/authenticated-request-format.txt
evidence/responses/authentication/authenticated-request-format-response.txt
Observed API Base Paths

The following API base paths were observed in Burp HTTP history:

/identity/api/
/identity/api/v2/
/workshop/api/
/workshop/api/shop/
/community/api/v2/
Authentication and Identity Routes
POST /identity/api/auth/verify

Purpose:

Verify authentication-related data during the user flow.

Authentication required:

Observed during authenticated browsing.

Observed status code:

200

Evidence:

evidence/screenshots/reconnaissance/burp-http-history-crapi.png
GET /identity/api/v2/user/dashboard

Purpose:

Retrieve dashboard data for the authenticated user.

Authentication required:

Yes

Observed status code:

200

Evidence:

evidence/screenshots/reconnaissance/burp-http-history-crapi.png
GET /identity/api/v2/user/videos/0

Purpose:

Retrieve user-related video or training data.

Authentication required:

Observed during authenticated browsing.

Observed status code:

404

Evidence:

evidence/screenshots/reconnaissance/burp-http-history-crapi.png
Vehicle Routes
GET /identity/api/v2/vehicle/vehicles

Purpose:

Retrieve vehicle data linked to the authenticated user.

Authentication required:

Yes

Observed status code:

200

Security relevance:

This route is relevant for object-level authorization testing because vehicle data may be linked to a specific user.

Evidence:

evidence/screenshots/reconnaissance/burp-http-history-crapi.png
POST /identity/api/v2/vehicle/resend_email

Purpose:

Resend a vehicle-related email during the vehicle verification flow.

Authentication required:

Observed during authenticated browsing.

Observed status code:

200

Security relevance:

This route may be useful for authentication, verification flow, or business logic review.

Evidence:

evidence/screenshots/reconnaissance/burp-http-history-crapi.png
Shop Routes
GET /workshop/api/shop/products?limit=...

Purpose:

Retrieve shop product listings.

Authentication required:

Observed during authenticated browsing.

Observed status code:

200

Evidence:

evidence/screenshots/reconnaissance/burp-http-history-crapi.png
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

This route is relevant for business logic testing, authorization testing, and rate-limit observation.

Evidence:

evidence/requests/authentication/authenticated-request-format.txt
evidence/responses/authentication/authenticated-request-format-response.txt
evidence/screenshots/authentication/authenticated-request-format-burp.png
GET /workshop/api/shop/orders

Purpose:

Retrieve shop order data for the authenticated user.

Authentication required:

Yes

Observed status code:

200

Security relevance:

This route is relevant for authorization and object access testing.

Evidence:

evidence/screenshots/reconnaissance/burp-http-history-crapi.png
GET /workshop/api/shop/orders/all?limit=...

Purpose:

Retrieve paginated order list data.

Authentication required:

Yes

Observed status code:

200

Security relevance:

This route is relevant for authorization review because the word "all" may indicate broader order access that should be validated carefully.

Evidence:

evidence/screenshots/reconnaissance/burp-http-history-crapi.png
GET /workshop/api/shop/orders/6

Purpose:

Retrieve details for a specific order.

Authentication required:

Yes

Observed status code:

200

Security relevance:

This route is relevant for BOLA testing because it uses a direct order object ID.

Evidence:

evidence/screenshots/reconnaissance/burp-http-history-crapi.png
Community Routes
GET /community/api/v2/community/post...

Purpose:

Retrieve community post data.

Authentication required:

Observed during authenticated browsing.

Observed status code:

200

Security relevance:

This route may be useful for object ID review and excessive data exposure testing if responses contain user or post metadata.

Evidence:

evidence/screenshots/reconnaissance/burp-http-history-crapi.png
Initial Security Notes

The most interesting routes for the next phases are:

GET /identity/api/v2/vehicle/vehicles
GET /workshop/api/shop/orders
GET /workshop/api/shop/orders/all?limit=...
GET /workshop/api/shop/orders/6
POST /workshop/api/shop/orders

Reason:

These routes involve authenticated user data, object IDs, vehicle data, order data, or business logic behavior.
Notes

This route map was created from Burp HTTP history during normal application usage.

The next phase will use this inventory to choose targets for BOLA, excessive data exposure, mass assignment, BFLA, and business logic testing.
