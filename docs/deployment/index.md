---
sidebar_position: 1
---

# Deployment Overview

QuckChat supports multiple deployment environments, each serving a specific purpose in the software development lifecycle.

## Environment Hierarchy

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         PRODUCTION TIER                                  │
│  ┌─────────────────────────────┐  ┌─────────────────────────────┐       │
│  │         LIVE               │  │       PRODUCTION            │       │
│  │   (Blue/Green Active)      │◄─│    (Release Candidate)      │       │
│  │   live.quckchat.com        │  │   prod.quckchat.com         │       │
│  └─────────────────────────────┘  └─────────────────────────────┘       │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                         PRE-PRODUCTION TIER                              │
│  ┌─────────────────────────────┐                                        │
│  │         STAGING             │                                        │
│  │   (Production Mirror)       │                                        │
│  │   staging.quckchat.com      │                                        │
│  └─────────────────────────────┘                                        │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                           UAT TIER                                       │
│  ┌───────────────┐  ┌───────────────┐  ┌───────────────┐               │
│  │     UAT1      │  │     UAT2      │  │     UAT3      │               │
│  │  (Feature A)  │  │  (Feature B)  │  │  (Regression) │               │
│  │ uat1.quckchat │  │ uat2.quckchat │  │ uat3.quckchat │               │
│  └───────────────┘  └───────────────┘  └───────────────┘               │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                         TESTING TIER                                     │
│  ┌─────────────────────────────┐                                        │
│  │            QA               │                                        │
│  │   (Quality Assurance)       │                                        │
│  │   qa.quckchat.com           │                                        │
│  └─────────────────────────────┘                                        │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                       DEVELOPMENT TIER                                   │
│  ┌─────────────────────────────┐  ┌─────────────────────────────┐       │
│  │           DEV               │  │          LOCAL              │       │
│  │   (Integration Testing)     │  │     (Developer Machine)     │       │
│  │   dev.quckchat.com          │  │     localhost:3000          │       │
│  └─────────────────────────────┘  └─────────────────────────────┘       │
└─────────────────────────────────────────────────────────────────────────┘
```

## Environment Summary

| Environment | Purpose | Data | Deployment | Access |
|-------------|---------|------|------------|--------|
| **Local** | Developer testing | Mock/Seed | Manual | Developer only |
| **Dev** | Integration testing | Synthetic | On commit | Development team |
| **QA** | Quality assurance | Test data | On PR merge | QA team |
| **UAT1** | Feature testing | Sanitized prod | Manual | Business users |
| **UAT2** | Feature testing | Sanitized prod | Manual | Business users |
| **UAT3** | Regression testing | Sanitized prod | Scheduled | QA + Business |
| **Staging** | Pre-production validation | Prod clone | On release | All teams |
| **Production** | Release candidate | Real data | Controlled | Operations |
| **Live** | Active production | Real data | Blue/Green | End users |

## Infrastructure per Environment

| Environment | Kubernetes | Database | Redis | Kafka | Replicas |
|-------------|------------|----------|-------|-------|----------|
| Local | Docker Compose | SQLite/H2 | Single | Single | 1 |
| Dev | Minikube | Shared RDS | Shared | Shared | 1 |
| QA | EKS (small) | Dedicated RDS | Dedicated | Shared | 2 |
| UAT1-3 | EKS (medium) | Dedicated RDS | Dedicated | Shared | 2 |
| Staging | EKS (prod-like) | Dedicated RDS | Cluster | Cluster | 3 |
| Production | EKS (full) | Multi-AZ RDS | Cluster | Cluster | 3-5 |
| Live | EKS (full) | Multi-AZ RDS | Cluster | Cluster | 5-10 |

## Deployment Flow

```
Developer → Local → Dev → QA → UAT → Staging → Production → Live
    │         │       │      │     │       │          │         │
    │         │       │      │     │       │          │         │
    ▼         ▼       ▼      ▼     ▼       ▼          ▼         ▼
  Code    Docker   Auto   Manual Manual  Release   Canary   Blue/Green
  Test    Compose  Deploy Deploy Deploy  Branch    Deploy    Switch
```

## Quick Links

- [Local Environment](./local) - Developer machine setup
- [Dev Environment](./dev) - Development integration
- [QA Environment](./qa) - Quality assurance testing
- [UAT Environments](./uat) - User acceptance testing
- [Staging Environment](./staging) - Pre-production
- [Production Environment](./production) - Release deployment
- [Live Environment](./live) - Active production
