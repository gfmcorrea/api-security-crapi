# API Testing Workflow

## Objective

This file documents the API testing workflow used during this OWASP crAPI assessment.

The goal was to test the API in a structured way, collect evidence, and avoid false positives.

## Workflow Summary

The workflow followed in this project was:

1. Start the local OWASP crAPI lab.
2. Configure the browser to proxy traffic through Burp Suite.
3. Create two local test users.
4. Browse the application normally.
5. Capture API traffic in Burp HTTP history.
6. Identify API base paths and endpoints.
7. Document authentication flow and user context.
8. Select endpoints with object IDs or sensitive behavior.
9. Send baseline requests to Burp Repeater.
10. Modify one value at a time.
11. Compare baseline and modified responses.
12. Save requests, responses, screenshots, and tool outputs.
13. Redact tokens, cookies, and passwords.
14. Write confirmed findings, observations, and not confirmed testing notes.

## Test Users

Two local users were used:

```text
User A: gabriel.usera@crapi.local
User B: gabriel.userb@crapi.local

User A was used as the baseline user.

User B was used for cross-user authorization testing.

Main Testing Areas

The project included testing for:

Broken Object Level Authorization
Broken Authentication observation
Broken Object Property Level Authorization
Broken Function Level Authorization
Mass Assignment
Business Logic credit validation
Evidence Standard

For each test, I saved evidence in the following format:

evidence/requests/
evidence/responses/
evidence/screenshots/

Requests and responses were saved as raw HTTP evidence.

Screenshots were used to show the Burp workflow and relevant API behavior.

Redaction Standard

Sensitive values were redacted before publishing.

Authorization: Bearer [REDACTED_TOKEN]
Cookie: [REDACTED_COOKIE]
password: [REDACTED_PASSWORD]
Reporting Standard

Each finding or testing note includes:

Status
Severity
OWASP API Top 10 mapping
Affected endpoint
Description
Technical details
Evidence
Impact
Remediation
Limitations
Conclusion

Final Note

The most important part of this workflow was validating findings with evidence and avoiding false positives.
