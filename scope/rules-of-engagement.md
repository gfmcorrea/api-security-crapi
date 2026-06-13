# Rules of Engagement

## Purpose

This file defines the rules followed during the OWASP crAPI local API security assessment.

## Authorization

Testing was performed only against a local OWASP crAPI lab.

I did not test external systems or systems without permission.

## Allowed Activities

The following activities were allowed:

- Running OWASP crAPI locally
- Capturing local API traffic with Burp Suite
- Creating local test users
- Sending manual requests through Burp Repeater
- Testing object-level authorization
- Testing JSON request and response behavior
- Saving redacted evidence
- Writing findings and remediation guidance

## Restricted Activities

The following activities were not performed:

- Denial-of-service testing
- High-volume brute force
- Credential stuffing
- Phishing
- Malware usage
- Persistence
- Evasion
- Exploitation against external systems
- Testing real companies or public targets
- Destructive actions

## Rate and Volume Limits

Only safe low-volume manual testing was performed.

Authentication testing was limited to a small number of invalid login attempts.

## Evidence Rules

Evidence could include:

- Raw HTTP requests
- Raw HTTP responses
- Screenshots
- Tool outputs
- Notes

Before publishing, sensitive values had to be redacted.

Redaction format:

```text
Authorization: Bearer [REDACTED_TOKEN]
Cookie: [REDACTED_COOKIE]
password: [REDACTED_PASSWORD]
Reporting Rules

Findings were only marked as confirmed when evidence proved the issue.

Results that were not proven were documented as:

Observation
Not Confirmed
Informational
Safety Statement

This project was performed for education and portfolio development only.

No external targets were tested.
