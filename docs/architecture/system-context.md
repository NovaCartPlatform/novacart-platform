# NovaCart System Context

## 1. Overview

NovaCart is a cloud-native e-commerce platform designed to simulate a real enterprise production environment.

The platform provides customers with the ability to browse products, manage shopping carts, place orders, complete payments, and receive notifications.

The system is designed using microservices architecture and will be deployed on AWS using modern DevOps and cloud-native practices.

## 2. Business Goals

The main goals of NovaCart are:

- Provide a scalable and secure e-commerce platform.
- Support high availability and fault tolerance.
- Allow independent deployment of services.
- Apply modern DevOps and DevSecOps practices.
- Use managed AWS services where appropriate.
- Support monitoring, logging, and observability.
- Provide a realistic enterprise project for learning and portfolio purposes.

## 3. Primary Users

### Customer

The customer can:

- Register and log in.
- Browse products.
- Search and filter products.
- Add products to the shopping cart.
- Place orders.
- Complete payments.
- Track order status.
- Receive email and SMS notifications.

### Administrator

The administrator can:

- Manage products.
- Manage inventory.
- Manage customer orders.
- Review payments.
- View operational reports.
- Monitor platform health.

## 4. Core System Components

NovaCart contains the following main components:

- Web Application
- API Gateway
- Authentication Service
- Product Service
- Cart Service
- Order Service
- Payment Service
- Notification Service
- PostgreSQL
- DynamoDB
- Redis

## 5. External Systems

NovaCart may integrate with the following external systems:

- Payment Gateway
- Email Service
- SMS Provider
- Identity Provider
- Shipping Provider

## 6. High-Level Request Flow

1. The customer accesses the NovaCart web application.
2. The web application sends requests to the API Gateway.
3. The API Gateway routes requests to the appropriate backend service.
4. Each service processes its own business logic.
5. Each service accesses its assigned data store.
6. Payment requests are sent to an external payment gateway.
7. Notifications are sent using email or SMS providers.

## 7. Architecture Principles

The platform follows these architecture principles:

- Microservices architecture
- API-first design
- Database per service
- Infrastructure as Code
- Automated CI/CD
- Security by design
- Observability by default
- Independent service deployment
- Horizontal scalability

## 8. Non-Functional Requirements

### Availability

The platform should remain available during service or infrastructure failures.

### Scalability

Services should scale independently according to workload.

### Security

The platform should use secure authentication, authorization, encryption, and secrets management.

### Performance

Customer-facing requests should respond within acceptable latency targets.

### Observability

The platform should provide centralized logging, metrics, tracing, and alerting.

### Maintainability

The codebase and infrastructure should be modular, documented, and easy to update.

## 9. Technology Direction

The initial technology direction is:

- Frontend: Next.js
- Backend: NestJS
- Relational Database: PostgreSQL / Amazon RDS
- NoSQL Database: Amazon DynamoDB
- Cache: Redis / Amazon ElastiCache
- Containers: Docker
- Orchestration: Kubernetes / Amazon EKS
- Infrastructure as Code: Terraform
- CI/CD: GitHub Actions and Jenkins
- GitOps: Argo CD
- Monitoring: Prometheus and Grafana
- Logging: OpenSearch or CloudWatch
- Cloud Platform: AWS

## 10. Current Status

The current phase focuses on:

- Project planning
- Repository structure
- Architecture documentation
- Local development setup
- Initial service scaffolding
