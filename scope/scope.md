# Scope

## Project

API Security Assessment Against OWASP crAPI

## Objective

The objective of this project was to perform a safe, local API security assessment against OWASP crAPI.

This project was created for learning, portfolio development, and practical AppSec training.

## In Scope

The following items were in scope:

```text
OWASP crAPI running locally
http://localhost:8888
http://localhost:8025

The following testing areas were in scope:

API reconnaissance
Endpoint inventory
Authentication flow mapping
Authorization testing
Object ID testing
JSON response analysis
Mass Assignment testing
Business Logic testing
Safe invalid login observation
Evidence collection
Remediation documentation
Out of Scope

The following items were out of scope:

Public internet targets
Third-party APIs
Real company systems
Denial-of-service testing
Malware
Persistence
Evasion
Phishing
Social engineering
Destructive testing
Attacks against systems without permission
High-volume brute force
Credential stuffing
Real data exfiltration
Target Application

Target:

OWASP crAPI

Local URL:

http://localhost:8888

MailHog:

http://localhost:8025
Test Accounts

Two local accounts were used:

User A: gabriel.usera@crapi.local
User B: gabriel.userb@crapi.local

No real accounts were used.

Data Handling

Only local lab data was used.

Tokens, cookies, and passwords were redacted before publishing.

Final Note

This assessment was performed only in a controlled local lab environment.
