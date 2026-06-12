# Career Material — API Security Assessment Against OWASP crAPI

## Short Resume Bullet

Performed a local API security assessment against OWASP crAPI, mapping endpoints, testing authentication and authorization controls, validating OWASP API Top 10 risks, collecting redacted evidence, and documenting findings with remediation guidance.

## Resume Bullets

- Built a local API security lab using OWASP crAPI, Docker, Docker Compose, Burp Suite, Postman, curl, jq, and Nmap.
- Mapped API endpoints and documented authentication flows, user roles, object IDs, and API request/response behavior.
- Tested API security issues aligned with OWASP API Security Top 10, including BOLA, Broken Authentication, Excessive Data Exposure, BFLA, Mass Assignment, and business logic observations.
- Captured and organized evidence using baseline requests, modified requests, responses, screenshots, and tool outputs.
- Redacted tokens, cookies, passwords, and sensitive values before publishing the project on GitHub.
- Wrote professional findings, impact analysis, remediation guidance, lessons learned, and a final report.

## LinkedIn Project Description

I built a hands-on API security project using OWASP crAPI in a local Docker lab.

In this project, I mapped API endpoints, documented the authentication flow, created separate test users, tested authorization issues such as BOLA and BFLA, reviewed JSON responses for excessive data exposure, and practiced evidence-based reporting with Burp Suite and Postman.

The main goal was to simulate a real API security assessment in a safe lab environment and document the full workflow clearly on GitHub.

This project helped me improve my API testing methodology, request/response analysis, object ID testing, evidence handling, token redaction, and technical reporting for AppSec and pentest roles.

## GitHub Repository Description

API security assessment against OWASP crAPI with endpoint mapping, authentication analysis, OWASP API Top 10 testing, redacted evidence, findings, remediation, and lessons learned.

## Interview Explanation

I created this project to practice API security testing in a realistic local lab.

I used OWASP crAPI as the target application and ran it locally with Docker. I captured API traffic with Burp Suite, mapped endpoints, created separate test users, and tested authorization by comparing baseline and modified requests.

For example, before testing BOLA, I captured a valid request as the object owner. Then I changed one object ID at a time and compared the response. I followed the same evidence-based approach for authentication, excessive data exposure, mass assignment, function-level authorization, and business logic observations.

I focused a lot on documentation quality. I saved requests, responses, screenshots, and tool outputs, then redacted tokens and cookies before publishing the project.

The main thing I learned is that good API testing depends on understanding the application flow, user context, object ownership, and response behavior before trying to report a finding.

## Skills List for LinkedIn

- API Security
- Application Security
- OWASP API Security Top 10
- OWASP WSTG
- Burp Suite
- Postman
- Docker
- Docker Compose
- API Reconnaissance
- Endpoint Mapping
- Authentication Testing
- Authorization Testing
- Broken Object Level Authorization
- Broken Function Level Authorization
- Mass Assignment Testing
- JSON Response Analysis
- Evidence Collection
- Vulnerability Reporting
- Remediation Guidance
- GitHub Portfolio
