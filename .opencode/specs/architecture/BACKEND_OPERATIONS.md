# Backend Operations Manual & Service Map

<!-- @META: Operations Documentation -->
<!--
    File: .opencode/specs/architecture/BACKEND_OPERATIONS.md
    Version: 2.0.0
    Type: Operational Standard
    Status: Living Document
-->

## 1. Service Architecture

<!-- @NOTE(arch-001): Microservices Topology -->
The Nexus OS backend follows a **Modular Monolith** architecture evolving into **Microservices**.
Core services are grouped by domain but deployable independently.

### Topology
- **Edge Layer**: Cloudflare / Nginx Ingress
- **API Gateway**: `hub` (Signaling Server) acting as the primary entry point
- **Service Layer**: Node.js services (AI, Billing, Storage)
- **Data Layer**: PostgreSQL (Primary), Redis (Cache), S3 (Blob)

### Communication Patterns
1.  **Synchronous (RPC)**:
    - Internal REST over HTTP/2
    - gRPC for high-throughput inter-service calls (future)
2.  **Asynchronous (Event-Driven)**:
    - **SystemBus** (In-memory for local modules)
    - **Kafka/Redpanda** (Inter-service events)
    - **WebSockets** (Real-time client push)

---

## 2. API Design Standards

<!-- @RULE: API Consistency -->
All services must adhere to the **Nexus API Standard** (RFC-API-001).

### REST Guidelines
- **Resources**: Plural nouns (e.g., `/users`, `/invoices`)
- **Versioning**: URI Versioning (e.g., `/api/v1/...`)
- **Status Codes**:
    - `2xx`: Success (200 OK, 201 Created)
    - `4xx`: Client Error (400 Bad Req, 401 Unauth, 403 Forbidden, 404 Not Found)
    - `5xx`: Server Error (500 Internal, 503 Unavailable)
- **Pagination**: Cursor-based preferred for large sets; Offset-based for simple lists.
    - `?cursor=xyz&limit=20`
- **Filtering**: `?field[eq]=value` or SCIM-like syntax.

### GraphQL
- Used primarily by the Frontend BFF (Backend for Frontend) layer.
- **Federation**: Apollo Federation for composing subgraphs.

---

## 3. Database Strategy

<!-- @NOTE(db-001): Data Persistence -->

### Primary Store: PostgreSQL
- **Usage**: Relational data (Users, Orgs, Billing)
- **Schema Management**: Drizzle ORM + Migrations
- **High Availability**: Primary-Replica setup with auto-failover (Patroni/AWS RDS)

### Migration Policy
1.  **Forward-only**: Migrations must never destroy data.
2.  **Backwards Compatible**: Database changes must support the *previous* code version during deployment.
3.  **Zero-Downtime**: 
    - Phase 1: Add column (nullable)
    - Phase 2: Dual-write application
    - Phase 3: Backfill data
    - Phase 4: Enforce constraints

### Backup
- **Point-in-Time Recovery (PITR)**: Enabled (7 days retention)
- **Daily Snapshots**: Retained for 30 days
- **Off-site Replication**: Encrypted copy in separate region

---

## 4. Caching Strategy

<!-- @NOTE(cache-001): Performance Tier -->

### Redis Cluster
- **L1 Cache**: In-memory (LRU) within Node.js processes (short-lived, <5s)
- **L2 Cache**: Redis Cluster (Shared state)
    - **Session Store**: User sessions (TTL: 7 days)
    - **Rate Limits**: Sliding window counters
    - **Query Cache**: Expensive SQL results (TTL: variable)

### Invalidation
- **Event-Based**: Cache keys invalidated via "EntityUpdated" events
- **TTL**: All cache entries MUST have a TTL. No infinite keys.

---

## 5. Message Queue & Event Bus

<!-- @NOTE(mq-001): Asynchronous Coupling -->

### Kafka / Redpanda
- **Topics**: `domain.entity.event` (e.g., `billing.invoice.created`)
- **Guarantees**: At-least-once delivery
- **Serialization**: Protobuf or Avro (Schema Registry enforced)

### Patterns
- **Outbox Pattern**: Atomically write to DB and Outbox table; Relay reads Outbox -> Kafka.
- **Dead Letter Queues (DLQ)**: Failed consumers retry 3x, then move message to DLQ.

---

## 6. Observability Stack

<!-- @NOTE(obs-001): Visibility -->

### Logging
- **Format**: JSON Structured Logging
- **Standard Fields**: `trace_id`, `span_id`, `service`, `level`, `timestamp`
- **Levels**: ERROR, WARN, INFO, DEBUG (dev only)
- **Aggregation**: ELK Stack / Datadog / Loki

### Metrics (Prometheus)
- **RED Method**: Rate, Errors, Duration
- **Standard Exports**: 
    - `http_request_duration_seconds`
    - `nodejs_eventloop_lag_seconds`
    - `db_connection_pool_usage`

### Tracing (OpenTelemetry)
- **Propagation**: W3C Trace Context
- **Sampling**: 10% of standard traffic, 100% of errors
- **Visualization**: Jaeger / Tempo

---

## 7. Service Level Agreements (SLA)

| Service Tier | Availability | Latency (p95) | RPO (Data Loss) | RTO (Recovery) |
|--------------|--------------|---------------|-----------------|----------------|
| **Tier 0** (Auth, Billing) | 99.99% | < 200ms | 0 min | 15 min |
| **Tier 1** (API, Signaling) | 99.9% | < 500ms | 5 min | 1 hour |
| **Tier 2** (Async Jobs) | 99.5% | N/A | 1 hour | 4 hours |
| **Tier 3** (Internal Tools) | 99.0% | < 1s | 24 hours | 8 hours |

---

## 8. Incident Response

<!-- @NOTE(ops-001): On-Call -->

### Severity Levels
- **SEV-1 (Critical)**: Total outage, Data loss, Security breach. (Page 24/7)
- **SEV-2 (High)**: Major feature broken, High latency. (Page 24/7)
- **SEV-3 (Medium)**: Minor bug, Workaround available. (Business Hours)
- **SEV-4 (Low)**: Cosmetic, Non-urgent. (Ticket)

### Response Process
1.  **Acknowledge**: On-call engineer ACKs within 15m (SEV-1/2).
2.  **Triaging**: Assess impact. Declare incident.
3.  **Mitigation**: Focus on restoring service (rollback, scale up, disable feature). Root cause analysis is LATER.
4.  **Communication**: Update status page every 30m.
5.  **Post-Mortem**: Required for all SEV-1/2 within 48h.

---

## 9. Deployment Strategy

<!-- @NOTE(deploy-001): CI/CD -->

### Strategies
- **Blue/Green**: For stateless services. Instant switch traffic.
- **Canary**: Rollout to 5% -> 20% -> 50% -> 100%. Auto-rollback on error spike.
- **Rolling Update**: K8s default.

### Pipeline
1.  **Build**: Docker build + Unit Tests
2.  **Staging**: Deploy to Staging -> E2E Tests -> Integration Tests
3.  **Production**: Manual approval (or auto for low-risk) -> Canary Deploy

---

## 10. Security Standards

<!-- @SECURITY: Mandatory Controls -->

### Authentication & Authorization
- **AuthN**: JWT (Access Token 1h, Refresh Token 7d).
- **AuthZ**: RBAC (Role-Based) + ABAC (Attribute-Based) for fine-grained resource access.
- **MFA**: Enforced for Admins and Sensitive Ops.

### Data Protection
- **Encryption at Rest**: AES-256 for DB and S3.
- **Encryption in Transit**: TLS 1.3 everywhere.
- **Secrets**: HashiCorp Vault / AWS Secrets Manager. No env vars for secrets in plain text.

---

## 11. Rate Limiting

<!-- @NOTE(rate-001): Traffic Control -->

### Policies
- **Global**: 1000 req/min per IP (DDoS protection)
- **Authenticated**: 
    - User: 300 req/min
    - Org: 5000 req/min
- **Endpoint Specific**:
    - Login: 5 req/min
    - AI Inference: Quota-based (Token bucket)

### Throttling
- Returns `429 Too Many Requests`.
- Headers: `X-RateLimit-Limit`, `X-RateLimit-Remaining`, `X-RateLimit-Reset`.

---

## 12. Disaster Recovery

<!-- @NOTE(dr-001): Continuity -->

### Scenarios
1.  **Region Failure**: Failover to secondary region (Active-Passive).
    - **DNS Failover**: Route53 / Cloudflare LB.
    - **DB Promotion**: Promote Read Replica to Primary.
2.  **Data Corruption**: Restore from PITR backup to new instance.

### Drill Schedule
- **Database Restore**: Monthly
- **Region Failover**: Quarterly

---

## 13. Service Registry

Detailed operational map of all backend components.

### Hub Modules (Monolith)

| Module | Responsibility | Dependencies | Criticality | SLA (Avail) | Owner | Runbook |
|--------|----------------|--------------|-------------|-------------|-------|---------|
| **Access** | RBAC/ABAC logic | DB, Cache | **P0** | 99.99% | Core Auth | [RUN-001](#) |
| **Auth** | Login, JWT, MFA | DB, Redis, Email | **P0** | 99.99% | Core Auth | [RUN-002](#) |
| **Billing** | Credits, Invoices | Stripe, DB | **P0** | 99.99% | FinTech | [RUN-003](#) |
| **Orgs** | Multi-tenancy | DB | **P1** | 99.9% | Core Platform | [RUN-004](#) |
| **Signaling** | WebRTC WebSocket | Redis (PubSub) | **P1** | 99.9% | Realtime | [RUN-005](#) |
| **Users** | Profiles, Settings | DB | **P1** | 99.9% | Core Platform | [RUN-006](#) |
| **Audit** | Compliance Logs | DB (Timescale) | **P2** | 99.5% | Security | [RUN-007](#) |
| **RateLimit** | Traffic Control | Redis | **P0** | 99.99% | SRE | [RUN-008](#) |

### Standalone Applications

| App | Responsibility | Dependencies | Criticality | SLA | Owner | Runbook |
|-----|----------------|--------------|-------------|-----|-------|---------|
| **AI Inference** | LLM Gateway | OpenAI/Anthropic | **P1** | 99.5% | AI Team | [RUN-AI-01](#) |
| **Cloud Storage** | S3 Wrapper/Mgmt | AWS S3 / R2 | **P1** | 99.9% | Infra | [RUN-STO-01](#) |
| **Knowledge Base** | Vector Search / RAG | Vector DB, AI App | **P2** | 99.0% | AI Team | [RUN-KB-01](#) |
| **Billing Gateway** | Payment Webhooks | Stripe/MercadoPago | **P0** | 99.99% | FinTech | [RUN-pay-01](#) |

### Shared Packages

| Package | Purpose | Criticality | Maintainer |
|---------|---------|-------------|------------|
| `circuit-breaker` | Fault tolerance | High | Platform |
| `config` | Env var validation | Critical | Platform |
| `db` | Drizzle ORM schemas | Critical | Data Eng |
| `events` | Kafka/Bus types | High | Platform |
| `logger` | JSON logging | Medium | SRE |
| `worker` | Background jobs | High | Platform |

---

<!-- @TODO: Link actual runbook URLs when created -->
<!-- @TODO: Add monitoring dashboard links -->

---

## 14. Operational Roles & Responsibilities

### Incident Command System (ICS)
During a SEV-1 or SEV-2 incident, the following roles are assigned:

1.  **Incident Commander (IC)**:
    - **Responsibility**: Runs the incident. Makes all final decisions. Does NOT debug.
    - **Authority**: Can pull anyone from any team.
    - **Handoff**: Must formally hand off role if leaving.

2.  **Operations Lead (Ops)**:
    - **Responsibility**: Performs system changes (rollbacks, restarts, scaling).
    - **Focus**: Mitigation and stability.

3.  **Communications Lead (Comms)**:
    - **Responsibility**: Updates status page, informs internal stakeholders.
    - **Frequency**: Internal every 30m, External every 1h.

4.  **Scribe**:
    - **Responsibility**: Documents timeline, commands issued, and key observations in the Slack channel.

### On-Call Rotation
- **Schedule**: Weekly rotation. Handover on Mondays at 11:00 AM.
- **Shadowing**: New engineers must shadow 2 rotations before going solo.
- **Compensation**: Standard override pay applies for off-hours paging.

---

## 15. Compliance & Auditing

### Data Privacy (GDPR/CCPA)
- **Right to be Forgotten**: Supported via `POST /api/user/deletion-request`. Processed within 30 days.
- **Data Export**: Users can download their data archive via `GET /api/user/export`.
- **PII Handling**: All PII (Email, IP) is encrypted at rest or anonymized in logs.

### Security Compliance (SOC2 Type II)
- **Change Management**: All PRs require 1 approval. No direct commits to `main`.
- **Access Control**: Quarterly access review for AWS/Prod DB access.
- **Vulnerability Management**:
    - **SAST**: GitHub CodeQL runs on every PR.
    - **DAST**: Weekly scans on staging environment.
    - **Depedency Check**: Dependabot alerts for high-severity CVEs.

