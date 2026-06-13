# API Security Assessment Against OWASP crAPI

## Overview

I built this project to practice API security testing in a safe local lab.

The target application was OWASP crAPI, also known as Completely Ridiculous API. It is a deliberately vulnerable API application created for API security training.

This project shows how I tested a vulnerable API step by step, collected real evidence, documented confirmed findings, and avoided marking unconfirmed behavior as vulnerabilities.

The main focus was:

* API reconnaissance
* Endpoint mapping
* Authentication flow analysis
* Authorization testing
* Object ID manipulation
* JSON response analysis
* Evidence collection
* Token redaction
* Clear vulnerability reporting

No external targets were tested. Everything was performed locally against OWASP crAPI.

## Target

Target application:

```text
OWASP crAPI
```

Local application:

```text
http://localhost:8888
```

MailHog:

```text
http://localhost:8025
```

## Lab Environment

I used the following environment:

* Ubuntu
* Docker
* Docker Compose
* OWASP crAPI
* Burp Suite Community Edition
* Firefox
* curl
* jq
* Nmap
* Git
* GitHub

## Tools Used

* Burp Suite Community Edition for capturing and modifying API requests
* Firefox configured to proxy traffic through Burp
* curl for quick local service checks
* Nmap for local service verification
* Docker and Docker Compose for running the lab
* Git and GitHub for version control and portfolio publishing

## Scope

The assessment was limited to OWASP crAPI running locally.

In scope:

* `http://localhost:8888`
* `http://localhost:8025`
* API endpoints observed during normal application usage
* Authentication and authorization flows
* User, vehicle, shop, order, and profile-related API behavior
* Local lab testing only

Out of scope:

* External systems
* Real third-party APIs
* Denial-of-service testing
* Malware
* Persistence
* Evasion
* Phishing
* Real data exfiltration
* Destructive actions
* Attacks against systems I do not own or have permission to test

## Methodology

I followed a manual API security testing workflow based on:

* OWASP API Security Top 10 2023
* OWASP WSTG concepts where useful
* PTES-inspired structure for planning, reconnaissance, testing, evidence, and reporting

My workflow was:

1. Start the local crAPI lab
2. Configure Firefox and Burp Suite
3. Register two local test users
4. Capture baseline API requests
5. Identify API endpoints, object IDs, and user context
6. Replay requests in Burp Repeater
7. Modify one value at a time
8. Compare baseline and modified responses
9. Save request and response evidence
10. Redact tokens and cookies
11. Document confirmed findings, observations, and not confirmed tests

## Repository Structure

`api-recon/`

Contains notes from API reconnaissance, including observed routes, authentication flow, technologies, user roles, and test accounts.

`appendices/`

Contains supporting material such as commands used, payload notes, evidence checklist, and Postman notes.

`docs/`

Contains setup guides and workflow documentation for the lab, Burp Suite, Postman, and API testing process.

`endpoint-inventory/`

Contains organized endpoint notes grouped by authentication, vehicle, shop, admin or sensitive functions, and general inventory.

`evidence/`

Contains evidence collected during testing, including requests, responses, screenshots, and tool outputs.

All tokens, cookies, and passwords were redacted before publishing.

`findings/`

Contains individual finding reports and testing notes.

`lessons-learned/`

Contains personal lessons learned and skills demonstrated during the project.

`methodology/`

Contains the API security methodology used in the project.

`remediation/`

Contains remediation guidance for the documented findings and observations.

`reports/`

Contains the final assessment report.

`scope/`

Contains the project scope and rules of engagement.

## Findings Summary

### Finding 01 — Broken Object Level Authorization

Status:

```text
Confirmed
```

Severity:

```text
High
```

OWASP API Top 10 Mapping:

```text
API1:2023 Broken Object Level Authorization
```

Affected endpoint:

```text
GET /workshop/api/shop/orders/{id}
```

Summary:

```text
A normal authenticated user was able to access another user's order by changing the order ID in the URL.
```

Evidence:

```text
findings/01-broken-object-level-authorization.md
evidence/requests/bola/
evidence/responses/bola/
evidence/screenshots/bola/
```

### Finding 02 — Broken Authentication / Rate Limit Observation

Status:

```text
Observation
```

Severity:

```text
Low
```

OWASP API Top 10 Mapping:

```text
API2:2023 Broken Authentication
```

Affected endpoint:

```text
POST /identity/api/auth/login
```

Summary:

```text
The login endpoint returned the same 401 response after five invalid password attempts. This was documented as a low-volume observation, not a confirmed vulnerability.
```

Evidence:

```text
findings/02-broken-authentication.md
evidence/requests/broken-authentication/
evidence/responses/broken-authentication/
evidence/screenshots/broken-authentication/
```

### Finding 03 — Excessive Data Exposure / Broken Object Property Level Authorization

Status:

```text
Confirmed
```

Severity:

```text
Medium
```

OWASP API Top 10 Mapping:

```text
API3:2023 Broken Object Property Level Authorization
```

Affected endpoint:

```text
GET /workshop/api/shop/orders/{id}
```

Summary:

```text
The order details endpoint returned user contact information and payment-related metadata, including email, phone number, transaction data, card owner name, card type, and card expiry.
```

Evidence:

```text
findings/03-excessive-data-exposure.md
evidence/requests/excessive-data-exposure/
evidence/responses/excessive-data-exposure/
evidence/screenshots/excessive-data-exposure/
```

### Finding 04 — Broken Function Level Authorization

Status:

```text
Not Confirmed
```

Severity:

```text
Informational
```

OWASP API Top 10 Mapping:

```text
API5:2023 Broken Function Level Authorization
```

Tested endpoint:

```text
GET /workshop/api/shop/orders/all?limit=30&offset=0
```

Summary:

```text
The endpoint name looked sensitive because it included orders/all, but the response returned only the authenticated user's own order data.
```

Evidence:

```text
findings/04-broken-function-level-authorization.md
evidence/requests/bfla/
evidence/responses/bfla/
evidence/screenshots/bfla/
```

### Finding 05 — Mass Assignment

Status:

```text
Not Confirmed
```

Severity:

```text
Informational
```

OWASP API Top 10 Mapping:

```text
API3:2023 Broken Object Property Level Authorization
```

Tested endpoint:

```text
POST /workshop/api/shop/orders
```

Summary:

```text
Extra JSON properties such as price and credit were accepted in the request body, but they did not change protected server-side values.
```

Evidence:

```text
findings/05-mass-assignment.md
evidence/requests/mass-assignment/
evidence/responses/mass-assignment/
evidence/screenshots/mass-assignment/
```

### Finding 06 — Business Logic Credit Validation

Status:

```text
Not Confirmed
```

Severity:

```text
Informational
```

OWASP API Top 10 Mapping:

```text
API6:2023 Unrestricted Access to Sensitive Business Flows
```

Tested endpoint:

```text
POST /workshop/api/shop/orders
```

Summary:

```text
The API allowed purchases while the user had enough credit, but blocked the next purchase after the credit reached 0.0.
```

Evidence:

```text
findings/06-rate-limit-or-business-logic-observation.md
evidence/requests/business-logic/
evidence/responses/business-logic/
evidence/screenshots/business-logic/
```

## Evidence Handling

I saved evidence using this structure:

```text
evidence/requests/
evidence/responses/
evidence/screenshots/
evidence/tool-outputs/
```

For each tested issue, I saved:

* Baseline request
* Baseline response
* Modified request when applicable
* Modified response when applicable
* Screenshot
* Notes explaining the result

Before publishing, I redacted tokens, cookies, and passwords.

Redaction standard:

```text
Authorization: Bearer [REDACTED_TOKEN]
Cookie: [REDACTED_COOKIE]
Set-Cookie: [REDACTED_COOKIE]
token: [REDACTED_TOKEN]
access_token: [REDACTED_TOKEN]
refresh_token: [REDACTED_TOKEN]
password: [REDACTED_PASSWORD]
transaction_id: *EXAMPLE*
```

## How to Reproduce the Lab Locally

Download crAPI:

```bash
curl -L -o /tmp/crapi.zip https://github.com/OWASP/crAPI/archive/refs/heads/main.zip
```

Extract the project:

```bash
unzip /tmp/crapi.zip
```

Enter the Docker deployment directory:

```bash
cd crAPI-main/deploy/docker
```

Pull images:

```bash
docker compose pull
```

Start the lab:

```bash
docker compose -f docker-compose.yml --compatibility up -d
```

Verify containers:

```bash
docker compose ps
```

Check the web application:

```bash
curl -I http://localhost:8888
```

Check exposed local services:

```bash
nmap -Pn -sV -p 8888,8025 localhost
```

Open the application:

```text
http://localhost:8888
```

Open MailHog:

```text
http://localhost:8025
```

Stop the lab safely:

```bash
docker compose -f docker-compose.yml --compatibility down
```

## Skills Demonstrated

This project demonstrates:

* API reconnaissance
* Endpoint inventory creation
* Authentication flow documentation
* Authorization testing
* BOLA testing
* BFLA testing
* Mass assignment testing
* JSON response review
* Business logic testing
* Burp Suite Repeater workflow
* Evidence collection
* Token redaction
* Security reporting
* Remediation writing
* GitHub portfolio documentation

## Lessons Learned

I learned that good API testing depends on understanding how the application works before trying to report vulnerabilities.

The most important part was capturing clean baseline requests and then changing only one thing at a time.

I also learned that not every test becomes a confirmed finding. Some tests are better documented as observations or not confirmed results.

This is important because professional security reporting should avoid false positives.

## Ethical Disclaimer

This project was performed only in a local lab environment using OWASP crAPI.

I did not test external systems, real companies, or third-party applications.

The purpose of this project is education, portfolio development, and safe security practice.
