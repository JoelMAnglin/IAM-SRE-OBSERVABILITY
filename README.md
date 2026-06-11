# Enterprise IAM SRE Observability Platform

## Overview

The Enterprise IAM SRE Observability Platform is a containerized Identity and Access Management (IAM) environment designed to demonstrate enterprise-grade identity federation, secrets management, monitoring, centralized logging, and operational automation.

This project was built to simulate many of the technologies and workflows commonly used by IAM Engineers, Security Engineers, Identity Architects, PAM Engineers, and Site Reliability Engineers (SREs).

The environment integrates Keycloak, OpenLDAP, HashiCorp Vault, Prometheus, Grafana, Loki, Promtail, Docker, and PowerShell automation into a single repeatable deployment.

---

# Project Objectives

* Deploy a modern IAM platform using Keycloak
* Implement LDAP federation using OpenLDAP
* Secure privileged credentials with Vault
* Monitor platform health using Prometheus
* Centralize authentication logs using Loki and Promtail
* Visualize operational metrics and security events in Grafana
* Automate health validation using PowerShell
* Demonstrate troubleshooting and operational ownership

---

# Architecture

```text
┌─────────────────────────────────────────────────────────────┐
│                    Enterprise Users                         │
│          Administrators • Analysts • Service Accounts      │
└─────────────────────────────┬───────────────────────────────┘
                              │
                              ▼

┌─────────────────────────────────────────────────────────────┐
│                         Keycloak                            │
│                                                           │
│  • Identity Provider (IdP)                               │
│  • Authentication                                         │
│  • SSO                                                    │
│  • User Federation                                        │
└─────────────────────────────┬───────────────────────────────┘
                              │
                              │ LDAP Federation
                              ▼

┌─────────────────────────────────────────────────────────────┐
│                         OpenLDAP                            │
│                                                           │
│  • User Directory                                          │
│  • Identity Store                                          │
│  • User Lifecycle Source                                   │
└─────────────────────────────────────────────────────────────┘

┌──────────────────────┐              ┌──────────────────────┐
│        Vault         │              │   PowerShell Scripts │
│                      │              │                      │
│ • Secrets Mgmt       │              │ • Health Checks      │
│ • Credentials        │              │ • Availability Tests │
│ • Service Accounts   │              │ • Latency Monitoring │
└──────────┬───────────┘              └──────────┬───────────┘
           │                                     │
           └─────────────────┬───────────────────┘
                             │
                             ▼

┌─────────────────────────────────────────────────────────────┐
│                  Authentication Events                      │
└─────────────────────────────┬───────────────────────────────┘
                              │
                              ▼

┌─────────────────────────────────────────────────────────────┐
│                         Promtail                            │
│                     Log Collection                          │
└─────────────────────────────┬───────────────────────────────┘
                              │
                              ▼

┌─────────────────────────────────────────────────────────────┐
│                           Loki                              │
│                    Log Aggregation                          │
└─────────────────────────────┬───────────────────────────────┘
                              │
                              ▼

┌──────────────────────┐              ┌──────────────────────┐
│     Prometheus       │─────────────►│       Grafana        │
│                      │   Metrics    │                      │
│ • Service Health     │              │ • Dashboards         │
│ • Monitoring         │              │ • Alerting           │
└──────────────────────┘              └──────────────────────┘
```

---

# Technology Stack

| Category           | Technology      |
| ------------------ | --------------- |
| Identity Provider  | Keycloak        |
| Directory Services | OpenLDAP        |
| Secrets Management | HashiCorp Vault |
| Monitoring         | Prometheus      |
| Visualization      | Grafana         |
| Logging            | Loki            |
| Log Collection     | Promtail        |
| Automation         | PowerShell      |
| Containerization   | Docker          |
| Orchestration      | Docker Compose  |

---

# Environment Components

## Keycloak

Responsible for:

* Authentication
* Identity Federation
* Administrative Access
* User Management
* SSO Capabilities

### Key Accomplishments

* Created IAM-LAB realm
* Created permanent administrative account
* Removed dependency on bootstrap admin
* Configured LDAP federation

---

## OpenLDAP

Responsible for:

* User storage
* Centralized identity source
* Authentication backend

### Federated Users

* jdoe
* asmith
* ironman
* captainamerica
* thor
* blackwidow
* hulk

---

## HashiCorp Vault

Responsible for:

* Secrets management
* Credential protection
* Service account storage

Example secret paths:

```text
iam/keycloak-admin
iam/ldap-bind-account
database/admin
```

---

## Prometheus

Responsible for:

* Service monitoring
* Availability tracking
* Metrics collection

Monitored Services:

* Keycloak
* Vault
* OpenLDAP
* Loki
* Grafana

---

## Grafana

Responsible for:

* Dashboarding
* Visualization
* Operational visibility

Example Dashboards:

* Authentication Events
* Failed Logins
* Successful Logins
* Service Health
* Platform Availability

---

## Loki

Responsible for:

* Centralized logging
* Authentication event storage
* Log querying

Example Events:

```text
login_success
login_failed
brute_force_detected
```

---

## Promtail

Responsible for:

* Log collection
* Log forwarding
* Authentication event ingestion

---

## PowerShell Automation

Custom scripts created for:

```powershell
check_keycloak.ps1
check_vault.ps1
check_ldap.ps1
check_loki.ps1
check_grafana.ps1
```

Capabilities:

* Health validation
* Availability testing
* Latency measurements
* Operational monitoring

---

# Deployment

## Clone Repository

```bash
git clone https://github.com/YOUR_USERNAME/iam-sre-observability-platform.git

cd iam-sre-observability-platform
```

## Start Environment

```bash
docker compose up -d
```

## Verify Containers

```bash
docker compose ps
```

Expected services:

```text
keycloak
openldap
vault
prometheus
grafana
loki
promtail
```

---

# Access URLs

| Service     | URL                         |
| ----------- | --------------------------- |
| Keycloak    | http://localhost:8080       |
| Grafana     | http://localhost:3000       |
| Prometheus  | http://localhost:9090       |
| Vault       | http://localhost:8200       |
| Loki Health | http://localhost:3100/ready |

---

# Challenges & Troubleshooting

## Docker Image Pull Failures

Issue:

```text
EOF
httpReadSeeker errors
```

Resolution:

* Restart Docker Desktop
* Pull images individually
* Rebuild containers

---

## Port Conflicts

Issue:

```text
Port 8080 already allocated
```

Resolution:

* Identify conflicting service
* Stop conflicting container
* Restart Keycloak

---

## Promtail Configuration Issues

Issue:

```text
field promtail not found
failed to create client manager
```

Resolution:

* Rebuild Promtail configuration
* Validate YAML syntax
* Verify client definitions

---

## LDAP Federation Problems

Issue:

LDAP users not visible in Keycloak.

Resolution:

* Verify Users DN
* Verify Bind DN
* Validate LDAP connectivity
* Perform synchronization

---

## Grafana Log Visibility

Issue:

No logs displayed.

Resolution:

* Verify Promtail ingestion
* Validate Loki datasource
* Confirm authentication log generation

---

# Skills Demonstrated

* Identity and Access Management
* LDAP Administration
* Identity Federation
* Secrets Management
* Docker Administration
* PowerShell Automation
* Site Reliability Engineering
* Security Engineering
* Observability
* Monitoring
* Log Aggregation
* Troubleshooting
* Root Cause Analysis
* Technical Documentation

---

# STAR Summary

## Situation

Enterprise IAM systems require monitoring, observability, logging, and secure identity management.

## Task

Build a fully functioning IAM platform capable of identity federation, monitoring, logging, and secrets management.

## Action

Implemented Keycloak, OpenLDAP, Vault, Prometheus, Grafana, Loki, Promtail, Docker, and PowerShell automation while troubleshooting multiple integration challenges.

## Result

Successfully deployed an enterprise-style IAM observability platform providing centralized identity management, secrets protection, operational visibility, logging, and automated health validation.
