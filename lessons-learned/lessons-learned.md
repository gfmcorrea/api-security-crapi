# Lessons Learned

## Overview

I built this project to practice API security testing in a local lab using OWASP crAPI.

The goal was not only to find vulnerabilities, but also to follow a clear testing workflow, collect evidence, avoid false positives, and document the results in a way that another security professional could review.

This project helped me understand how API security testing works in practice.

## API Reconnaissance

I learned that API testing starts with understanding the application.

Before testing vulnerabilities, I had to:

* Browse the application normally
* Capture traffic in Burp Suite
* Identify API base paths
* Map endpoints
* Understand which endpoints required authentication
* Identify object IDs
* Separate User A and User B context

This made the rest of the project much easier because I was not testing randomly.

## Authentication Mapping

I learned that authentication mapping is important before authorization testing.

I documented:

* Registration flow
* Login flow
* Bearer token usage
* Authenticated request format
* Invalid login attempt behavior
* User A and User B separation

This helped me understand which token belonged to which user before testing cross-user access.

## Authorization Testing

The most important confirmed issue in this project was Broken Object Level Authorization.

I learned how to test BOLA by:

* Capturing a valid baseline request as the object owner
* Identifying the object ID in the URL
* Logging in as another user
* Replaying the same request with the second user's token
* Comparing the response

This confirmed that User B could access User A's order by changing the order ID.

## Object ID Testing

I learned that direct object IDs in API paths are high-value targets.

The endpoint below was especially important:

```text
GET /workshop/api/shop/orders/{id}
```

Changing the order ID allowed cross-user access.

This showed me why APIs must always enforce object ownership on the server side.

## JSON Response Analysis

I learned to review JSON responses carefully.

The order details response exposed:

* User email
* User phone number
* Transaction metadata
* Masked card number
* Card owner name
* Card type
* Card expiry

This became a confirmed Excessive Data Exposure finding.

I also learned that excessive data exposure becomes more serious when combined with authorization issues.

## Burp Repeater Workflow

Burp Repeater was the main tool used for manual validation.

I used it to:

* Replay baseline requests
* Change one value at a time
* Replace object IDs
* Replace user tokens
* Compare status codes
* Compare response bodies
* Save raw request and response evidence

The most important habit was changing only one thing at a time.

## Mass Assignment Testing

I tested Mass Assignment on the shop order creation endpoint by adding unexpected JSON fields:

```json
{"product_id":2,"quantity":1,"price":0}
```

```json
{"product_id":2,"quantity":1,"credit":9999}
```

The API accepted the requests, but the fields did not change protected server-side values.

So I documented the result as Not Confirmed instead of forcing a vulnerability.

This helped me understand the importance of avoiding false positives.

## Business Logic Testing

I tested whether the API allowed purchases after the user's credit reached zero.

The API allowed purchases while the user still had enough credit, but blocked the next purchase after the balance reached `0.0`.

The blocked response was:

```text
Insufficient Balance. Please apply coupons to get more balance!
```

This showed that the API enforced server-side credit validation for that tested flow.

## Broken Authentication Observation

I tested five invalid login attempts against the local login endpoint.

The API returned the same `401 Invalid Credentials` response for attempt 1 and attempt 5.

I documented this as a low-volume observation, not a confirmed vulnerability.

This helped me understand that authentication testing must be careful and should not be overstated without stronger evidence.

## BFLA Review

I tested the endpoint:

```text
GET /workshop/api/shop/orders/all?limit=30&offset=0
```

The endpoint looked sensitive because it included `orders/all`.

However, the response only returned the authenticated user's own orders.

So Broken Function Level Authorization was not confirmed.

This was useful because it showed that endpoint names can look suspicious, but evidence must decide the finding status.

## Evidence Handling

I learned that evidence quality is very important.

For each test, I saved:

* Request files
* Response files
* Screenshots
* Tool outputs when useful
* Notes explaining the result

I also learned to avoid using screenshots that show tokens.

## Token Redaction

Before publishing, I reviewed files for:

* JWT tokens
* Bearer tokens
* Cookies
* Passwords
* Sensitive values

I used this redaction format:

```text
Authorization: Bearer [REDACTED_TOKEN]
Cookie: [REDACTED_COOKIE]
password: [REDACTED_PASSWORD]
```

This is important because a security portfolio should demonstrate safe evidence handling.

## Avoiding False Positives

One of the biggest lessons from this project was that not every test becomes a confirmed finding.

Confirmed findings:

```text
Broken Object Level Authorization
Excessive Data Exposure
```

Observations or not confirmed tests:

```text
Broken Authentication rate-limit observation
Broken Function Level Authorization
Mass Assignment
Business Logic credit validation
```

This helped me practice honest, evidence-based reporting.

## Reporting

I learned how to write findings with:

* Status
* Severity
* OWASP mapping
* Affected endpoint
* Description
* Technical details
* Evidence
* Impact
* Remediation
* Limitations
* Conclusion

This made the project look more like a real AppSec or API pentest report.

## Final Reflection

This project helped me practice skills that are important for Junior AppSec, Junior Pentester, API Security Analyst, and Junior Security Analyst roles.

The strongest part of the project was the confirmed BOLA finding and how it chained with excessive data exposure.

I also learned that documenting not confirmed tests is valuable because it shows methodology, honesty, and professional judgment.
