# NovaCart Container Architecture

## 1. Overview

This document describes the internal container architecture of the NovaCart platform.

In this context, a container means a deployable application or data store, not only a Docker container.

NovaCart is designed as a cloud-native microservices platform where each service has a clear business responsibility and can be developed, deployed, scaled, and monitored independently.

## 2. Architecture Style

NovaCart follows these architectural patterns:

- Microservices architecture
- API-first design
- Database per service
- Event-driven communication where appropriate
- Independent service deployment
- Centralized observability
- Infrastructure as Code
- Automated CI/CD
- Security by design

## 3. Main Containers

### 3.1 Web Application

**Technology:** Next.js

**Responsibilities:**

- Provide the customer-facing user interface.
- Display products and categories.
- Manage the shopping cart interface.
- Allow customer registration and login.
- Allow customers to create and track orders.
- Send requests to the API Gateway.
- Display payment and notification status.

**Communication:**

- Communicates with the API Gateway using HTTPS.
- Does not communicate directly with backend databases.

---

### 3.2 API Gateway

**Technology Direction:** NestJS Gateway or AWS API Gateway

**Responsibilities:**

- Provide a single entry point for frontend requests.
- Route requests to the appropriate backend service.
- Validate authentication tokens.
- Apply rate limiting.
- Apply request validation.
- Enforce security policies.
- Collect request logs and metrics.
- Handle API versioning.

**Communication:**

- Receives HTTPS requests from the Web Application.
- Sends requests to backend microservices.
- Returns service responses to the Web Application.

---

### 3.3 Authentication Service

**Technology:** NestJS

**Responsibilities:**

- Register users.
- Authenticate users.
- Issue and validate access tokens.
- Manage roles and permissions.
- Manage password reset workflows.
- Manage user sessions.
- Integrate with an external identity provider when required.

**Data Store:**

- PostgreSQL
- Dedicated authentication database

**Communication:**

- Receives requests from the API Gateway.
- Communicates with PostgreSQL.
- May communicate with an external Identity Provider.

---

### 3.4 Product Service

**Technology:** NestJS

**Responsibilities:**

- Manage products.
- Manage product categories.
- Manage prices.
- Manage product descriptions and images.
- Provide product search and filtering.
- Maintain product availability information.

**Data Store:**

- PostgreSQL
- Dedicated product database

**Communication:**

- Receives requests from the API Gateway.
- Communicates with PostgreSQL.
- Publishes product update events when required.

---

### 3.5 Cart Service

**Technology:** NestJS

**Responsibilities:**

- Create customer shopping carts.
- Add and remove cart items.
- Update item quantities.
- Calculate cart totals.
- Store temporary cart state.
- Validate product availability before checkout.

**Data Store:**

- Redis

**Communication:**

- Receives requests from the API Gateway.
- Communicates with Redis.
- Calls the Product Service to validate products and prices.

---

### 3.6 Order Service

**Technology:** NestJS

**Responsibilities:**

- Create customer orders.
- Maintain order status.
- Store order items and totals.
- Coordinate checkout workflows.
- Request payment processing.
- Publish order-created and order-updated events.
- Provide order tracking information.

**Data Store:**

- PostgreSQL
- Dedicated order database

**Communication:**

- Receives requests from the API Gateway.
- Communicates with PostgreSQL.
- Calls the Product Service when product validation is required.
- Calls or publishes events to the Payment Service.
- Publishes events to the Notification Service.

---

### 3.7 Payment Service

**Technology:** NestJS

**Responsibilities:**

- Create payment transactions.
- Integrate with external payment gateways.
- Track payment status.
- Handle payment callbacks and webhooks.
- Store transaction references.
- Prevent duplicate payment processing.
- Publish payment-success and payment-failed events.

**Data Store:**

- Amazon DynamoDB

**Communication:**

- Receives payment requests from the Order Service.
- Communicates with an external Payment Gateway.
- Stores payment transaction data in DynamoDB.
- Publishes payment result events.

---

### 3.8 Notification Service

**Technology:** NestJS

**Responsibilities:**

- Send email notifications.
- Send SMS notifications.
- Process asynchronous notification events.
- Retry failed notifications.
- Maintain notification delivery status.
- Process order and payment events.

**Data Store:**

- Redis for queueing and temporary state
- Optional persistent notification history database later

**Communication:**

- Consumes order and payment events.
- Communicates with email and SMS providers.
- Uses Redis for queues and retry workflows.

## 4. Data Stores

### 4.1 PostgreSQL

PostgreSQL is used for relational business data that requires strong consistency and transactions.

Initial databases:

- Authentication Database
- Product Database
- Order Database

Each service owns its database schema and other services must not access it directly.

### 4.2 DynamoDB

DynamoDB is used by the Payment Service for scalable payment transaction records and idempotency data.

### 4.3 Redis

Redis is used for:

- Shopping cart state
- Caching
- Session data
- Temporary locks
- Notification queues
- Rate limiting

## 5. Communication Patterns

### 5.1 Synchronous Communication

Synchronous communication is used when an immediate response is required.

Examples:

- Web Application to API Gateway
- API Gateway to backend services
- Cart Service to Product Service
- Payment Service to external Payment Gateway

Initial protocol:

- HTTPS
- REST APIs
- JSON payloads

### 5.2 Asynchronous Communication

Asynchronous communication is used to reduce coupling and improve reliability.

Examples:

- Order created
- Payment succeeded
- Payment failed
- Order status changed
- Notification requested

Planned AWS technologies:

- Amazon SQS
- Amazon SNS
- Amazon EventBridge

The final selection will be documented in an Architecture Decision Record.

## 6. Database Ownership

NovaCart follows the Database per Service pattern.

Rules:

- Each service owns its data.
- Services must not query another service database directly.
- Data is shared through APIs or events.
- Database changes are managed by the owning service.
- Each service maintains its own migrations.

## 7. Security Boundaries

The architecture applies the following security rules:

- All external traffic uses HTTPS.
- Authentication tokens are validated at the API Gateway and services.
- Services use least-privilege permissions.
- Secrets are not stored in source code.
- Production secrets will be stored in AWS Secrets Manager.
- Databases are not publicly accessible.
- Internal services will run in private subnets.
- Payment webhooks must be validated.
- Sensitive data must be encrypted in transit and at rest.

## 8. Deployment Model

Each application container will be packaged as a Docker image.

Initial deployment stages:

1. Local development using Docker Compose.
2. Automated build and testing using CI pipelines.
3. Container image storage in Amazon ECR.
4. Production deployment to Amazon EKS.
5. GitOps deployment using Argo CD.

Each service can be deployed and scaled independently.

## 9. Observability

All services must provide:

- Structured application logs
- Health check endpoints
- Metrics
- Distributed tracing
- Error reporting
- Request correlation IDs

Planned tools:

- Prometheus
- Grafana
- OpenTelemetry
- AWS CloudWatch
- OpenSearch

## 10. Resilience

The platform should support:

- Retry policies
- Timeouts
- Circuit breakers
- Dead-letter queues
- Idempotent operations
- Graceful degradation
- Horizontal scaling
- Health checks
- Automated recovery

## 11. Container Summary

| Container | Technology | Responsibility | Data Store |
|---|---|---|---|
| Web Application | Next.js | Customer user interface | None |
| API Gateway | NestJS / AWS API Gateway | Routing and API security | None |
| Authentication Service | NestJS | Identity and access management | PostgreSQL |
| Product Service | NestJS | Product catalog management | PostgreSQL |
| Cart Service | NestJS | Shopping cart management | Redis |
| Order Service | NestJS | Order lifecycle management | PostgreSQL |
| Payment Service | NestJS | Payment processing | DynamoDB |
| Notification Service | NestJS | Email and SMS delivery | Redis |

## 12. Initial Request Flow

1. The customer opens the Web Application.
2. The Web Application sends a request to the API Gateway.
3. The API Gateway validates and routes the request.
4. The target service processes the business logic.
5. The service reads or writes to its owned data store.
6. When required, the service publishes an event.
7. Other services consume the event and continue the workflow.
8. The final result is returned to the customer.

## 13. Current Decisions

The current architecture decisions are:

- Use a monorepo for the initial implementation.
- Use Next.js for the frontend.
- Use NestJS for backend services.
- Use PostgreSQL for relational business data.
- Use DynamoDB for payment transaction data.
- Use Redis for cart state, cache, and queues.
- Use Docker for container packaging.
- Use Kubernetes and Amazon EKS for production orchestration.
- Use Terraform for infrastructure provisioning.

These decisions will be documented separately in the ADR directory.

## 14. Open Architecture Questions

The following items still require formal decisions:

- NestJS API Gateway versus AWS API Gateway
- Amazon SQS versus EventBridge for domain events
- Amazon Cognito versus custom authentication
- Shared PostgreSQL cluster versus separate clusters
- Amazon MSK or Kafka requirement
- Service mesh requirement
- OpenSearch versus CloudWatch Logs
- Exact disaster recovery strategy
