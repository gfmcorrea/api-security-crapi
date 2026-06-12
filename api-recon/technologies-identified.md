# Technologies Identified

## Objective

This file documents the technologies and local services identified during the initial environment setup.

Only evidence collected from the local OWASP crAPI lab was used.

## Local Application

Target application:

```text
OWASP crAPI

Local URL:

http://localhost:8888

MailHog URL:

http://localhost:8025
Technologies Observed

The following technologies were observed during setup and service verification:

OWASP crAPI
Docker
Docker Compose
MailHog
OpenResty
HTTP API
JSON API responses
Local web application on port 8888
Local MailHog service on port 8025
Docker Evidence

Docker version evidence:

evidence/tool-outputs/docker-version.txt

Docker Compose version evidence:

evidence/tool-outputs/docker-compose-version.txt

Docker containers evidence:

evidence/tool-outputs/docker-compose-ps.txt
evidence/screenshots/environment/docker-containers-running.png
Local HTTP Evidence

crAPI HTTP response evidence:

evidence/tool-outputs/curl-localhost-8888.txt

MailHog HTTP check evidence:

evidence/tool-outputs/curl-localhost-8025.txt

crAPI homepage screenshot:

evidence/screenshots/environment/crapi-homepage.png

MailHog screenshot:

evidence/screenshots/environment/mailhog-homepage.png
Local Service Scan

Nmap command used:

nmap -Pn -sV -p 8888,8025 localhost

Nmap evidence:

evidence/tool-outputs/nmap-local-services.txt

Observed local services:

8025/tcp open http
8888/tcp open http OpenResty web app server 1.27.1.2
Notes

This information was collected only from the local crAPI lab.

No external systems were scanned or tested.
