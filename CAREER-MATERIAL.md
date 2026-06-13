# Career Material — API Security Assessment Against OWASP crAPI

## Short Resume Bullet

Performed a local API security assessment against OWASP crAPI, identifying and documenting confirmed BOLA and Excessive Data Exposure vulnerabilities, testing authentication, authorization, mass assignment, and business logic flows, and publishing redacted evidence with remediation guidance on GitHub.

## Resume Bullets

* Built a local API security lab using OWASP crAPI, Docker, Docker Compose, Burp Suite, Firefox, curl, Nmap, Git, and GitHub.
* Mapped API endpoints, authentication flow, user roles, object IDs, order IDs, and API request/response behavior.
* Confirmed a Broken Object Level Authorization vulnerability by showing that one authenticated user could access another user's order through direct object ID manipulation.
* Confirmed Excessive Data Exposure by identifying user contact information and payment-related metadata exposed in the order details API response.
* Tested Broken Authentication, BFLA, Mass Assignment, and Business Logic flows and documented results as observations or not confirmed when evidence did not support a vulnerability.
* Collected and organized evidence using baseline requests, modified requests, HTTP responses, screenshots, and tool outputs.
* Redacted JWT tokens, cookies, passwords, and sensitive values before publishing the project.
* Wrote professional findings, impact analysis, remediation guidance, lessons learned, and a final report.

## LinkedIn Project Description

I built a hands-on API security project using OWASP crAPI in a local Docker lab.

In this project, I mapped API endpoints, documented the authentication flow, created separate test users, and tested API security risks based on OWASP API Security Top 10.

The strongest confirmed finding was Broken Object Level Authorization. I showed that User B could access User A's order by changing the order ID in the API request. I also confirmed Excessive Data Exposure because the order details response exposed user contact information and payment-related metadata.

I also tested Broken Authentication, Broken Function Level Authorization, Mass Assignment, and Business Logic flows. Some tests were documented as observations or not confirmed because the evidence did not support marking them as vulnerabilities.

This project helped me improve my API testing methodology, Burp Suite workflow, object ID testing, evidence handling, token redaction, and security reporting.

## GitHub Repository Description

API security assessment against OWASP crAPI with endpoint mapping, authentication analysis, confirmed BOLA and Excessive Data Exposure findings, redacted evidence, remediation guidance, and lessons learned.

## Interview Explanation

I created this project to practice API security testing in a realistic local lab.

I used OWASP crAPI as the target application and ran it locally with Docker. I captured API traffic with Burp Suite, mapped endpoints, created separate test users, and tested authorization by comparing baseline and modified requests.

The main confirmed vulnerability was Broken Object Level Authorization. User A created an order, and then I used User B's authenticated context to request the same order ID. The API returned User A's order data to User B, which confirmed the authorization issue.

I also confirmed Excessive Data Exposure because the order details response returned more data than necessary, including user email, phone number, transaction metadata, masked card number, card owner name, card type, and card expiry.

I tested other areas too, including login rate-limit behavior, BFLA, Mass Assignment, and business logic credit validation. I did not mark those as confirmed vulnerabilities when the evidence did not support it. I documented them as observations or not confirmed testing notes.

The main thing I learned is that good API testing depends on understanding the application flow, user context, object ownership, and response behavior before writing a finding.

## 30-Second Interview Version

I built an API security portfolio project using OWASP crAPI in a local Docker lab.

I used Burp Suite to map endpoints, capture authenticated requests, and test authorization controls with two separate users.

I confirmed a BOLA vulnerability where User B could access User A's order by changing the order ID. I also confirmed Excessive Data Exposure because the order response exposed contact and payment-related metadata.

For the other tests, like Mass Assignment, BFLA, and business logic, I documented the results honestly as not confirmed when the evidence did not prove a vulnerability.

This project shows my API testing workflow, evidence handling, token redaction, and technical reporting.

## Skills List for LinkedIn

* API Security
* Application Security
* OWASP API Security Top 10
* OWASP WSTG
* Burp Suite
* Docker
* Docker Compose
* API Reconnaissance
* Endpoint Mapping
* Authentication Testing
* Authorization Testing
* Broken Object Level Authorization
* Broken Function Level Authorization
* Broken Object Property Level Authorization
* Excessive Data Exposure
* Mass Assignment Testing
* Business Logic Testing
* JSON Response Analysis
* Evidence Collection
* Vulnerability Reporting
* Remediation Guidance
* GitHub Portfolio
* Security Documentation

## Project Highlights

Confirmed findings:

```text
Broken Object Level Authorization
Excessive Data Exposure / Broken Object Property Level Authorization
```

Observed or not confirmed tests:

```text
Broken Authentication rate-limit observation
Broken Function Level Authorization review
Mass Assignment testing
Business Logic credit validation
```

Main tools:

```text
OWASP crAPI
Docker
Docker Compose
Burp Suite Community Edition
Firefox
curl
Nmap
Git
GitHub
```

## Natural GitHub About Text

API security assessment against OWASP crAPI focused on OWASP API Security Top 10, BOLA, Excessive Data Exposure, authentication review, authorization testing, evidence collection, and remediation reporting.

## Natural LinkedIn Post

I finished a new API Security portfolio project using OWASP crAPI in a local Docker lab.

The goal was to simulate a real API security assessment and document the full process on GitHub.

In this project, I:

* Mapped API endpoints with Burp Suite
* Created separate test users
* Documented the authentication flow
* Tested object-level authorization
* Confirmed a BOLA vulnerability
* Confirmed Excessive Data Exposure
* Tested Mass Assignment, BFLA, login behavior, and business logic flows
* Collected request/response evidence
* Redacted tokens and cookies before publishing
* Wrote findings, remediation guidance, and a final report

The most important lesson was that not every test becomes a confirmed vulnerability. I documented confirmed findings, observations, and not confirmed tests separately to avoid false positives.

This project helped me improve my API testing methodology, evidence handling, and technical reporting for AppSec and API Security roles.

## Portfolio Explanation

This project was created to show practical API security skills in a safe and authorized lab.

It is not just a list of vulnerabilities. It includes the full workflow:

* environment setup
* scope
* rules of engagement
* methodology
* API reconnaissance
* endpoint inventory
* authentication flow
* findings
* evidence
* remediation
* lessons learned
* final report

The project is useful for Junior AppSec, Junior Pentest, API Security Analyst, and Junior Security Analyst roles.

