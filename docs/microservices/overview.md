---
sidebar_position: 1
---

# Microservices Overview

QuckChat consists of **32 microservices** built with 5 different technology stacks.

## Service Map

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          QUCKCHAT MICROSERVICES                              │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                    SPRING BOOT (5 Services)                          │   │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐  │   │
│  │  │   Auth   │ │   User   │ │Permission│ │  Audit   │ │  Admin   │  │   │
│  │  │  :8081   │ │  :8082   │ │  :8083   │ │  :8084   │ │  :8085   │  │   │
│  │  └──────────┘ └──────────┘ └──────────┘ └──────────┘ └──────────┘  │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                      NESTJS (3 Services)                             │   │
│  │  ┌──────────────┐   ┌──────────────┐   ┌──────────────┐             │   │
│  │  │   Backend    │   │   Realtime   │   │ Notification │             │   │
│  │  │   Gateway    │   │   Gateway    │   │   Service    │             │   │
│  │  │    :3000     │   │    :4000     │   │    :3001     │             │   │
│  │  └──────────────┘   └──────────────┘   └──────────────┘             │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                    ELIXIR/PHOENIX (6 Services)                       │   │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐  │   │
│  │  │ Presence │ │   Call   │ │ Message  │ │Notif Orch│ │  Huddle  │  │   │
│  │  │  :4001   │ │  :4002   │ │  :4003   │ │  :4004   │ │  :4005   │  │   │
│  │  └──────────┘ └──────────┘ └──────────┘ └──────────┘ └──────────┘  │   │
│  │  ┌──────────┐                                                       │   │
│  │  │  Event   │                                                       │   │
│  │  │Broadcast │                                                       │   │
│  │  │  :4006   │                                                       │   │
│  │  └──────────┘                                                       │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                        GO (10 Services)                              │   │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐  │   │
│  │  │Workspace │ │ Channel  │ │  Search  │ │  Thread  │ │ Bookmark │  │   │
│  │  │  :5004   │ │  :5005   │ │  :5006   │ │  :5009   │ │  :5010   │  │   │
│  │  └──────────┘ └──────────┘ └──────────┘ └──────────┘ └──────────┘  │   │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐  │   │
│  │  │ Reminder │ │  Media   │ │   File   │ │Attachment│ │   CDN    │  │   │
│  │  │  :5011   │ │  :5001   │ │  :5002   │ │  :5012   │ │  :5013   │  │   │
│  │  └──────────┘ └──────────┘ └──────────┘ └──────────┘ └──────────┘  │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                              │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                      PYTHON (8 Services)                             │   │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐               │   │
│  │  │Analytics │ │Moderation│ │  Export  │ │Integration│               │   │
│  │  │  :5007   │ │  :5014   │ │  :5015   │ │  :5016   │               │   │
│  │  └──────────┘ └──────────┘ └──────────┘ └──────────┘               │   │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐               │   │
│  │  │    ML    │ │Sentiment │ │ Insights │ │Smart Reply│               │   │
│  │  │  :5008   │ │  :5017   │ │  :5018   │ │  :5019   │               │   │
│  │  └──────────┘ └──────────┘ └──────────┘ └──────────┘               │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

## Service Categories

### Authentication & Security (Spring Boot)

| Service | Port | Database | Description |
|---------|------|----------|-------------|
| [auth-service](./spring-boot/auth-service) | 8081 | MySQL | JWT, OAuth, 2FA, OTP |
| [user-service](./spring-boot/user-service) | 8082 | MySQL | User CRUD, profiles |
| [permission-service](./spring-boot/permission-service) | 8083 | MySQL | RBAC, Casbin |
| [audit-service](./spring-boot/audit-service) | 8084 | MySQL | Audit logging |
| [admin-service](./spring-boot/admin-service) | 8085 | MySQL | Admin operations |

### API & Notifications (NestJS)

| Service | Port | Database | Description |
|---------|------|----------|-------------|
| [backend-gateway](./nestjs/backend-gateway) | 3000 | PostgreSQL | API Gateway |
| [realtime-service](./nestjs/realtime-service) | 4000 | Redis | WebSocket Gateway |
| [notification-service](./nestjs/notification-service) | 3001 | PostgreSQL | Push, Email, SMS |

### Real-time Communication (Elixir)

| Service | Port | Database | Description |
|---------|------|----------|-------------|
| [presence-service](./elixir/presence-service) | 4001 | MongoDB | Online status |
| [call-service](./elixir/call-service) | 4002 | MongoDB | Voice/Video calls |
| [message-service](./elixir/message-service) | 4003 | MongoDB | Real-time messaging |
| [notification-orchestrator](./elixir/notification-orchestrator) | 4004 | MongoDB | Notification delivery |
| [huddle-service](./elixir/huddle-service) | 4005 | MongoDB | Audio rooms |
| [event-broadcast-service](./elixir/event-broadcast-service) | 4006 | MongoDB | Event distribution |

### Organization & Storage (Go)

| Service | Port | Database | Description |
|---------|------|----------|-------------|
| [workspace-service](./go/workspace-service) | 5004 | MySQL | Workspace management |
| [channel-service](./go/channel-service) | 5005 | MySQL | Channel management |
| [search-service](./go/search-service) | 5006 | MySQL + ES | Full-text search |
| [thread-service](./go/thread-service) | 5009 | MySQL | Thread conversations |
| [bookmark-service](./go/bookmark-service) | 5010 | MySQL | Saved items |
| [reminder-service](./go/reminder-service) | 5011 | MySQL | Reminders |
| [media-service](./go/media-service) | 5001 | MongoDB | Media processing |
| [file-service](./go/file-service) | 5002 | MongoDB | File storage |
| [attachment-service](./go/attachment-service) | 5012 | MongoDB | Attachments |
| [cdn-service](./go/cdn-service) | 5013 | MongoDB | CDN management |

### AI & Analytics (Python)

| Service | Port | Database | Description |
|---------|------|----------|-------------|
| [analytics-service](./python/analytics-service) | 5007 | MySQL | Usage analytics |
| [moderation-service](./python/moderation-service) | 5014 | MySQL | Content moderation |
| [export-service](./python/export-service) | 5015 | MySQL | Data export |
| [integration-service](./python/integration-service) | 5016 | MySQL | Third-party integrations |
| [ml-service](./python/ml-service) | 5008 | Databricks | ML predictions |
| [sentiment-service](./python/sentiment-service) | 5017 | Databricks | Sentiment analysis |
| [insights-service](./python/insights-service) | 5018 | Databricks | Business insights |
| [smart-reply-service](./python/smart-reply-service) | 5019 | Databricks | AI suggestions |

## Communication Patterns

### Service-to-Service Communication

```
┌─────────────────────────────────────────────────────────────────┐
│                COMMUNICATION PATTERNS                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Synchronous (REST/gRPC)                                        │
│  ├── Gateway → Auth Service (token validation)                  │
│  ├── Gateway → User Service (profile data)                      │
│  └── Gateway → Permission Service (authorization)               │
│                                                                  │
│  Asynchronous (Kafka)                                           │
│  ├── All Services → Analytics Service (events)                  │
│  ├── Auth Service → Audit Service (login events)                │
│  └── Message Service → Notification Orchestrator                │
│                                                                  │
│  Real-time (WebSocket/Phoenix Channels)                         │
│  ├── Client ↔ Realtime Gateway ↔ Elixir Services               │
│  └── Presence broadcasts, message delivery                      │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

## Technology Rationale

### Why Spring Boot for Auth?

- **Enterprise security patterns** - Battle-tested Spring Security
- **Transaction management** - ACID compliance for user data
- **Extensive OAuth libraries** - Spring Security OAuth2
- **Audit logging** - Spring Data Envers

### Why Elixir for Real-time?

- **Millions of connections** - OTP/BEAM VM designed for concurrency
- **Fault tolerance** - Supervisor trees, let it crash philosophy
- **Phoenix Channels** - Built-in WebSocket abstraction
- **Low latency** - Sub-millisecond message routing

### Why Go for CRUD?

- **Performance** - Compiled, low memory footprint
- **Simplicity** - Easy to read, maintain, deploy
- **Concurrency** - Goroutines for parallel processing
- **Fast cold starts** - Ideal for Kubernetes scaling

### Why Python for ML?

- **ML ecosystem** - TensorFlow, PyTorch, scikit-learn
- **Data processing** - Pandas, NumPy
- **Databricks integration** - Native Python support
- **Rapid prototyping** - Quick iteration on ML models

## Service Dependencies

```
┌─────────────────────────────────────────────────────────────────┐
│                  SERVICE DEPENDENCIES                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Backend Gateway                                                │
│  └── Depends on: Auth, User, Permission (critical)             │
│                                                                  │
│  Message Service                                                │
│  └── Depends on: Presence, User (for validation)               │
│                                                                  │
│  Notification Service                                           │
│  └── Depends on: User (for preferences)                        │
│                                                                  │
│  Search Service                                                 │
│  └── Depends on: Elasticsearch (external)                      │
│                                                                  │
│  ML Services                                                    │
│  └── Depends on: Databricks (external)                         │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

## Health Endpoints

All services expose health endpoints:

```bash
# HTTP health check
GET /health
GET /actuator/health  # Spring Boot

# Response
{
  "status": "healthy",
  "version": "1.0.0",
  "uptime": 3600,
  "dependencies": {
    "database": "healthy",
    "redis": "healthy",
    "kafka": "healthy"
  }
}
```

## Deployment

### Docker Compose

```bash
# Start all services
docker-compose -f docker-compose.yml -f docker-compose.services.yml up -d

# Start specific service group
docker-compose up -d auth-service user-service permission-service
```

### Kubernetes

```bash
# Deploy all services
kubectl apply -f k8s/

# Deploy specific namespace
kubectl apply -f k8s/spring-boot/
kubectl apply -f k8s/elixir/
```
