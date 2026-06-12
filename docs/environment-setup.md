# Environment Setup

## Objective

This file documents how I set up OWASP crAPI locally using Docker and Docker Compose.

The goal of this phase was to confirm that the local lab was running correctly before starting API reconnaissance and security testing.

## Local Repository Path

```text
~/Cybersecurity-Portfolio/api-security-crapi
Local Lab Path
~/Cybersecurity-Labs/crAPI-main
Tools Used
Ubuntu
Docker
Docker Compose
OWASP crAPI
curl
Nmap
Firefox
Docker Version
Docker version 29.1.3, build 29.1.3-0ubuntu3~24.04.2

Evidence file:

evidence/tool-outputs/docker-version.txt
Docker Compose Version

Evidence file:

evidence/tool-outputs/docker-compose-version.txt
Download crAPI

Command used:

curl -L -o /tmp/crapi.zip https://github.com/OWASP/crAPI/archive/refs/heads/main.zip

Evidence files:

evidence/tool-outputs/crapi-download-output.txt
evidence/tool-outputs/crapi-zip-file-info.txt
Extract crAPI

The crAPI source was extracted under:

~/Cybersecurity-Labs/crAPI-main

Evidence files:

evidence/tool-outputs/crapi-unzip-output.txt
evidence/tool-outputs/crapi-lab-directory-listing.txt
Docker Deployment Directory

The Docker deployment directory used was:

/root/Cybersecurity-Labs/crAPI-main/deploy/docker

Evidence files:

evidence/tool-outputs/crapi-docker-path.txt
evidence/tool-outputs/crapi-docker-directory-listing.txt
Pull Docker Images

Command used:

docker compose pull

Evidence file:

evidence/tool-outputs/docker-compose-pull.txt
Start crAPI

Command used:

docker compose -f docker-compose.yml --compatibility up -d

Evidence file:

evidence/tool-outputs/docker-compose-up.txt
Verify Running Containers

Command used:

docker compose ps

Evidence files:

evidence/tool-outputs/docker-compose-ps.txt
evidence/screenshots/environment/docker-containers-running.png

Observed result:

The crAPI containers started successfully. The main local services included crAPI web on port 8888 and MailHog on port 8025.
Verify crAPI Web Application

Command used:

curl -I http://localhost:8888

Evidence file:

evidence/tool-outputs/curl-localhost-8888.txt

Observed result:

HTTP/1.1 200 OK
Server: openresty/1.27.1.2

Screenshot:

evidence/screenshots/environment/crapi-homepage.png
Verify MailHog

MailHog was accessed in the browser at:

http://localhost:8025

Evidence files:

evidence/tool-outputs/curl-localhost-8025.txt
evidence/screenshots/environment/mailhog-homepage.png

Observed result:

The MailHog web interface loaded successfully in the browser.
Local Service Scan

Command used:

nmap -Pn -sV -p 8888,8025 localhost

Evidence file:

evidence/tool-outputs/nmap-local-services.txt

Observed result:

Port 8025/tcp was open and identified as an HTTP service.
Port 8888/tcp was open and identified as an OpenResty web application server.
Stop the Lab Safely

Command to stop the lab:

cd ~/Cybersecurity-Labs/crAPI-main/deploy/docker
docker compose -f docker-compose.yml --compatibility down
Notes

This lab was executed only on my local machine.

No external targets were tested.

The environment evidence was collected before starting API reconnaissance and security testing.
