# Vehicle Endpoints

## Objective

This file documents vehicle-related endpoints observed during the OWASP crAPI assessment.

Vehicle endpoints are important because they may contain user-specific objects and identifiers.

## Evidence

```text
evidence/screenshots/reconnaissance/burp-http-history-crapi.png
GET /identity/api/v2/vehicle/vehicles

Purpose:

Retrieve vehicle data linked to the authenticated user.

Authentication required:

Yes

Observed status code:

200

Security relevance:

This endpoint is relevant for BOLA and excessive data exposure testing because it returns vehicle data associated with the authenticated account.

Testing notes:

This endpoint should be reviewed later to identify vehicle IDs, ownership data, and unnecessary JSON fields.
POST /identity/api/v2/vehicle/resend_email

Purpose:

Resend a vehicle-related email during the vehicle verification flow.

Authentication required:

Observed during authenticated browsing.

Observed status code:

200

Security relevance:

This endpoint is relevant for verification flow and business logic review.

Testing notes:

This endpoint should not be tested with high-volume requests. Any testing must remain low-volume and safe.
Priority for Next Testing

Primary endpoint:

GET /identity/api/v2/vehicle/vehicles

Why:

It may expose vehicle object identifiers and user-specific data that can be used for BOLA or excessive data exposure testing.
Notes

No vehicle-specific vulnerability was confirmed in this phase.

This file only documents reconnaissance results.
