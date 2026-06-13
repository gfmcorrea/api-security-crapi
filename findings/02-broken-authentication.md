# Finding 02 — Broken Authentication / Rate Limit Observation

## Status

Observation

## Severity

Low

## CVSS Estimate

CVSS was not assigned because this item was documented as a low-volume observation, not a confirmed vulnerability.

## Tested Endpoint

```text
POST /identity/api/auth/login
```

## Tested Method

```text
POST
```

## Tested Flow

```text
Invalid login attempt handling
```

## OWASP API Top 10 Mapping

```text
API2:2023 Broken Authentication
```

## Description

I tested the login endpoint with repeated invalid password attempts to observe whether the API changed its response, blocked the account, or applied visible rate limiting during a small number of failed login attempts.

This was a safe low-volume test performed only against the local OWASP crAPI lab.

## Request Used

Evidence file:

```text
evidence/requests/broken-authentication/login-invalid-password-request.txt
```

Request body:

```json
{"email":"gabriel.usera@crapi.local","password":"[REDACTED_PASSWORD]"}
```

## Test Performed

I sent five invalid login attempts using the same local test account and an incorrect password.

I saved evidence for:

```text
Attempt 1
Attempt 5
```

The goal was to compare the first and final observed response.

## Attempt 1 Response

Evidence file:

```text
evidence/responses/broken-authentication/login-invalid-attempt-1-response.txt
```

Response:

```json
{"token":null,"type":"","message":"Invalid Credentials","mfaRequired":false}
```

Status code:

```text
401 Unauthorized
```

## Attempt 5 Response

Evidence file:

```text
evidence/responses/broken-authentication/login-invalid-attempt-5-response.txt
```

Response:

```json
{"token":null,"type":"","message":"Invalid Credentials","mfaRequired":false}
```

Status code:

```text
401 Unauthorized
```

## Observed Result

The API returned the same response for attempt 1 and attempt 5.

Observed behavior:

```text
HTTP 401 response remained the same.
The message remained "Invalid Credentials".
No account lockout was observed.
No visible rate-limit response was observed.
No CAPTCHA or additional verification was observed.
No delay or throttling behavior was documented.
```

## Security Interpretation

This does not confirm a Broken Authentication vulnerability by itself.

The result is documented as an observation because only a small number of safe login attempts were performed.

However, in a real application, login endpoints should usually include protections against automated abuse, such as rate limiting, monitoring, and account protection controls.

## Technical Impact

If a production API does not apply rate limiting or abuse detection to login endpoints, attackers may have more opportunity to perform password guessing attacks.

In this local test, only five invalid attempts were sent, so the result should not be overstated.

## Business Impact

In a real environment, weak login abuse protection could increase the risk of:

* Credential guessing
* Account takeover attempts
* Increased authentication noise
* Higher monitoring burden
* Automated abuse against user accounts

## Recommended Remediation

Recommended actions:

* Apply rate limiting to login endpoints.
* Add monitoring for repeated failed login attempts.
* Add alerting for suspicious authentication patterns.
* Consider progressive delays after repeated failures.
* Consider temporary account protection mechanisms when appropriate.
* Keep error messages generic.
* Log failed authentication attempts with useful security context.
* Avoid exposing whether the email exists.
* Add automated tests for repeated failed login behavior.

## Evidence

Request:

```text
evidence/requests/broken-authentication/login-invalid-password-request.txt
```

Responses:

```text
evidence/responses/broken-authentication/login-invalid-attempt-1-response.txt
evidence/responses/broken-authentication/login-invalid-attempt-5-response.txt
```

Screenshots:

```text
evidence/screenshots/broken-authentication/login-invalid-attempt-1.png
evidence/screenshots/broken-authentication/login-invalid-attempt-5.png
```

## Limitations

This was a safe low-volume test.

Only five invalid login attempts were performed.

No brute force, credential stuffing, or denial-of-service testing was performed.

The result applies only to the local OWASP crAPI lab.

## Conclusion

Broken Authentication was not confirmed.

A login rate-limit observation was documented because the API returned the same `401 Invalid Credentials` response after five invalid login attempts, with no visible lockout or throttling behavior observed during this limited test.
