# CitiCore Banking Platform - Complete Master Architecture & Deployment Guide

**Comprehensive guide to the entire CitiCore Banking Platform: architecture, all 8 microservices, AWS cloud infrastructure, ECS containerization, Jenkins CI/CD automation, Git SHA-based deployment strategy, manual rollback procedures, end-to-end request flows, production deployment, and operational procedures.**

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Technology Stack](#technology-stack)
3. [Microservices Architecture](#microservices-architecture)
4. [Final System Architecture](#final-system-architecture)
5. [Service Responsibilities](#service-responsibilities)
6. [Local Development](#local-development)
7. [Configuration Management](#configuration-management)
8. [Service Discovery](#service-discovery)
9. [AWS Infrastructure](#aws-infrastructure)
10. [VPC and Networking](#vpc-and-networking)
11. [Security Groups](#security-groups)
12. [ECS and Fargate](#ecs-and-fargate)
13. [ECS Service Connect](#ecs-service-connect)
14. [Application Load Balancers](#application-load-balancers)
15. [API Gateway](#api-gateway)
16. [Kafka Event Architecture](#kafka-event-architecture)
17. [Redis Caching](#redis-caching)
18. [Database Architecture](#database-architecture)
19. [AWS Secrets Manager and TLS](#aws-secrets-manager-and-tls)
20. [Jenkins Architecture](#jenkins-architecture)
21. [CI/CD Pipeline Overview](#cicd-pipeline-overview)
22. [Git SHA Deployment Strategy](#git-sha-deployment-strategy)
23. [ECS Deployment Mechanism](#ecs-deployment-mechanism)
24. [Manual Rollback Strategy](#manual-rollback-strategy)
25. [Health Checks and Verification](#health-checks-and-verification)
26. [End-to-End Request Flows](#end-to-end-request-flows)
27. [Deployment Procedures](#deployment-procedures)
28. [Rollback Procedures](#rollback-procedures)
29. [Verification Commands](#verification-commands)
30. [Troubleshooting Guide](#troubleshooting-guide)
31. [Production Checklist](#production-checklist)
32. [Current Status](#current-status)
33. [Future Improvements](#future-improvements)
34. [Interview-Ready Explanations](#interview-ready-explanations)

---

## Project Overview

### Definition

**CitiCore Banking Platform** is a distributed microservices-based banking system built with Spring Boot, deployed on AWS using ECS/Fargate, with event-driven communication through Kafka, centralized configuration via Spring Cloud Config, service discovery via Eureka, and fully automated CI/CD pipeline using Jenkins with Git SHA-based immutable deployment strategy and manual rollback capabilities.

### What You Implemented

```
Complete banking platform with:

├─ 8 independent microservices
├─ AWS cloud infrastructure
├─ ECS Fargate containerization
├─ Jenkins CI/CD pipeline
├─ Event-driven messaging
├─ Service discovery
├─ Centralized configuration
├─ API gateway
├─ Authentication & authorization
├─ User management
├─ Account management
├─ Transaction processing
├─ Email notifications
└─ Complete monitoring & verification
```

### Core Goals

```
✅ Independent Microservice Deployment
   ├─ Each service deploys separately
   ├─ No coordinated releases
   └─ Reduces deployment risk

✅ Centralized Configuration
   ├─ Spring Cloud Config
   ├─ Git-backed configuration
   └─ Environment-specific settings

✅ Service Discovery
   ├─ Eureka server
   ├─ Auto-registration
   └─ Client-side discovery

✅ Internal Service Communication
   ├─ OpenFeign declarative clients
   ├─ Timeout & retry handling
   └─ JWT propagation

✅ Event-Driven Communication
   ├─ Kafka for asynchronous events
   ├─ Saga pattern for transactions
   └─ Dead letter queues for failures

✅ Cloud-Native Deployment
   ├─ Docker containers
   ├─ AWS ECS Fargate
   ├─ Immutable infrastructure
   └─ Git SHA-based versioning

✅ Automated CI/CD
   ├─ Git push triggers deployment
   ├─ Smart service detection
   ├─ Full automation
   └─ Manual rollback capability

✅ Banking-Grade Reliability
   ├─ Transaction consistency
   ├─ Event reliability
   ├─ Failure recovery
   └─ Complete audit trail
```

### High-Level Deployment Flow

```
Developer Writes Code
    ↓
Commit to GitHub
    ↓
Git Push to master
    ↓
Jenkins Webhook Triggered
    ↓
Jenkins Controller
    ├─ Detect changed services
    ├─ Smart service selection
    └─ Orchestrate pipeline
    ↓
Jenkins Agent (SSH connection)
    ├─ Maven: Compile Java code
    ├─ Docker: Build container images
    ├─ AWS CLI: Push to ECR
    └─ ECS: Deploy to production
    ↓
Amazon ECR (Image Registry)
    └─ Store images with Git SHA tags
    ↓
Amazon ECS/Fargate (Orchestration)
    ├─ Pull images
    ├─ Start tasks
    ├─ Route traffic
    └─ Monitor health
    ↓
Production (Running Microservices)
    ├─ Serve user requests
    ├─ Process transactions
    └─ Store data

If issues arise:
    ↓
Manual Rollback via Jenkins
    ├─ Operator selects service
    ├─ Chooses known-good Git SHA
    ├─ Jenkins deploys old image
    └─ Service restored
```

---

## Technology Stack

### Backend Framework

```
Java
├─ Version: 17 LTS
├─ Latest Spring compatibility
├─ Stability & performance
└─ Security support until 2031

Spring Boot
├─ Version: 3.2.4
├─ Fast startup
├─ Production-ready
└─ Embedded Tomcat

Spring Cloud
├─ Version: 2023.0.3
├─ Service discovery (Eureka)
├─ Config management
├─ Distributed features
└─ Circuit breakers
```

### Microservices Patterns

```
Spring Data JPA
├─ Object-relational mapping
├─ Repository pattern
└─ Query abstraction

Hibernate
├─ JPA implementation
├─ Database persistence
└─ Transaction management

OpenFeign
├─ Declarative REST client
├─ Service-to-service calls
├─ Automatic retries
└─ Timeout handling

Spring Cloud Gateway
├─ API routing
├─ Request/response filtering
├─ Rate limiting
└─ Circuit breaker patterns

Spring Security
├─ Authentication
├─ Authorization
├─ JWT token handling
└─ Method-level security
```

### Distributed System Components

```
Eureka Server
├─ Service registry
├─ Auto-registration
├─ Client-side discovery
└─ Health checks

Apache Kafka
├─ Event streaming
├─ Saga coordination
├─ Event replay
└─ Asynchronous processing

Redis
├─ In-memory caching
├─ Rate limiting
├─ Session storage
└─ Fast data access

AWS RDS MySQL
├─ Relational persistence
├─ Multi-AZ deployment
├─ Automated backups
└─ Replication support
```

### DevOps & Infrastructure

```
GitHub
├─ Source control
├─ Collaboration
├─ Webhook integration
└─ CI/CD triggering

Jenkins
├─ CI/CD orchestration
├─ Pipeline automation
├─ Build coordination
└─ Deployment control

Docker
├─ Containerization
├─ Image consistency
├─ Portable deployments
└─ Layer caching

Amazon ECR
├─ Docker image registry
├─ Private repository
├─ IAM access control
└─ Image scanning

Amazon ECS
├─ Container orchestration
├─ Task scheduling
├─ Service management
└─ Rolling deployments

AWS Fargate
├─ Serverless containers
├─ No EC2 management
├─ Pay-per-use
└─ Automatic scaling

AWS VPC
├─ Network isolation
├─ Custom networking
├─ Security groups
└─ Route tables

Application Load Balancer
├─ Traffic distribution
├─ Health checks
├─ HTTPS termination
└─ Path-based routing

AWS Secrets Manager
├─ Credential storage
├─ Rotation support
├─ Audit logging
└─ IAM integration

AWS CloudWatch
├─ Log aggregation
├─ Metrics collection
├─ Alarms
└─ Dashboards
```

---

## Microservices Architecture

### The 8 Services

```
| # | Service           | Port | Directory              | Responsibility                    |
|---|-------------------|------|------------------------|-----------------------------------|
| 1 | Config Server     | 8888 | config-service         | Centralized configuration         |
| 2 | Eureka Server     | 8761 | eureka-server          | Service discovery & registration  |
| 3 | API Gateway       | 8080 | apigateway-service     | External routing & gateway logic  |
| 4 | Auth Service      | 8081 | auth-service           | Authentication & JWT              |
| 5 | User Service      | 8082 | user-service           | User management & KYC             |
| 6 | Account Service   | 8083 | account-service        | Account operations                |
| 7 | Transaction Srv   | 8084 | transaction-service    | Transaction processing & saga     |
| 8 | Notification Srv  | —    | notification-service   | Email notifications               |
```

### ECS Service Names (Production)

```
Config Server:
  citicore-config-server-service-rwdtvpj9
  └─ Generated suffix by ECS

Eureka Server:
  citicore-eureka-server
  └─ Simple naming

Auth Service:
  citicore-auth-service

User Service:
  citicore-user-service

Account Service:
  citicore-account-service

Transaction Service:
  citicore-transaction-service

Notification Service:
  citicore-notification-service-service-0nnwrup9
  └─ Generated suffix by ECS

API Gateway:
  citicore-apigateway-service
```

### Service Interdependencies

```
API Gateway (entry point)
    ├─ Routes to Auth Service :8081
    ├─ Routes to User Service :8082
    ├─ Routes to Account Service :8083
    └─ Routes to Transaction Service :8084

Transaction Service
    ├─ Calls Account Service (OpenFeign)
    ├─ Publishes to Kafka
    └─ Subscribes to Kafka events

Account Service
    ├─ Publishes transaction events
    ├─ Subscribes to transaction events
    ├─ Reads from RDS
    └─ Uses Redis for caching

User Service
    ├─ Manages user profiles
    ├─ Integrates with Auth
    └─ Publishes user events

Auth Service
    ├─ Issues JWT tokens
    ├─ Validates credentials
    └─ Email verification

All Services
    ├─ Register with Eureka
    ├─ Fetch config from Config Server
    ├─ Publish domain events
    └─ Subscribe to relevant topics
```

---

## Final System Architecture

### Complete System Diagram

```
                             INTERNET
                                |
                                v
                      ┌─────────────────────┐
                      │  Gateway ALB        │
                      │  HTTP :8080         │
                      └─────────┬───────────┘
                                |
                                v
                      ┌─────────────────────┐
                      │   API Gateway       │
                      │   :8080             │
                      └─────────┬───────────┘
                                |
                ┌───────────────┼───────────────┐
                |               |               |
                v               v               v
        ┌──────────┐     ┌──────────┐    ┌──────────┐
        │  Auth    │     │  User    │    │ Account  │
        │  :8081   │     │  :8082   │    │  :8083   │
        └──────────┘     └──────────┘    └────┬─────┘
                                              |
                                              v
                                        ┌──────────┐
                                        │Transaction│
                                        │  :8084    │
                                        └──────────┘

                    ┌───────────────────────────────┐
                    │     ECS / Fargate Cluster     │
                    │                               │
                    │ Config Server :8888           │
                    │ Eureka :8761                  │
                    │ Auth :8081                    │
                    │ User :8082                    │
                    │ Account :8083                 │
                    │ Transaction :8084             │
                    │ Notification Service          │
                    │ API Gateway :8080             │
                    └───────────────┬───────────────┘
                                    |
                      ┌─────────────┼─────────────┐
                      |             |             |
                      v             v             v
                    Kafka         Redis           RDS
                                             (Persistent DB)

                        │
        ┌───────────────┴────────────────┐
        |                                |
        v                                v
    GitHub Repository            Jenkins CI/CD
        (Source Code)            (Deployment)
```

### Logical Request Path

```
External Client
    │
    ├─ HTTPS (optional)
    │
    v
Internet
    │
    v
Gateway ALB :8080
    │ (forwards to API Gateway)
    │
    v
API Gateway :8080
    │ (routes based on path)
    │
    ├─ /auth/** → Auth Service :8081
    ├─ /users/** → User Service :8082
    ├─ /accounts/** → Account Service :8083
    └─ /transaction/** → Transaction Service :8084
    │
    v
Internal Service
    │
    ├─ Validates request
    ├─ Queries RDS if needed
    ├─ Publishes event to Kafka
    ├─ Caches in Redis
    └─ Returns response
    │
    v
API Gateway (response)
    │
    v
Gateway ALB (response)
    │
    v
External Client
```

### Internal Service Communication

```
Transaction Service (needs to query Account Service)
    │
    ├─ Uses OpenFeign client
    │
    v
Account Service (via Service Connect DNS)
    │
    ├─ citicore-account-service-8083-tcp.citicore:8083
    │
    v
ECS Service Connect
    │
    ├─ Resolves DNS to task IP
    ├─ Routes within VPC
    │
    v
Account Service (task receives request)
    │
    v
Response back to Transaction Service
```

---

## Service Responsibilities

### Config Server (Port 8888)

```
Responsibility:
└─ Centralized configuration management

How it works:

1. Configuration stored in Git
   └─ https://github.com/Rabbaniinamdar/citicore-config-repo.git

2. Spring services fetch config on startup
   └─ spring.config.import: configserver:http://...

3. Services refresh config via /actuator/refresh

Architecture:

GitHub Config Repo
    │
    v
Config Server :8888
    │
    ├─ Auth Service (fetches auth config)
    ├─ User Service (fetches user config)
    ├─ Account Service (fetches account config)
    ├─ Transaction Service (fetches txn config)
    ├─ API Gateway (fetches gateway config)
    └─ Other services (fetch their config)

Configuration includes:
├─ Database URLs
├─ Kafka bootstrap servers
├─ Redis connection
├─ Eureka URL
├─ Application-specific settings
└─ Feature flags

Security principle:
├─ Configuration stored in Git
├─ Secrets stored in AWS Secrets Manager
├─ Never commit passwords or keys
```

### Eureka Server (Port 8761)

```
Responsibility:
└─ Service registry & discovery

How it works:

1. All services register themselves
   └─ On startup: "I am AUTH-SERVICE at 10.0.1.50:8081"

2. Services query Eureka for peer locations
   └─ "Where is ACCOUNT-SERVICE?"
   └─ Eureka: "At 10.0.1.75:8083"

3. Client-side service discovery
   └─ Services cache registry
   └─ Update periodically

Architecture:

Eureka Server :8761
    │
    ├─ AUTH-SERVICE
    │  ├─ Instance 1: 10.0.1.50:8081
    │  └─ Status: UP
    │
    ├─ USER-SERVICE
    │  ├─ Instance 1: 10.0.1.60:8082
    │  └─ Status: UP
    │
    ├─ ACCOUNT-SERVICE
    │  ├─ Instance 1: 10.0.1.75:8083
    │  └─ Status: UP
    │
    ├─ TRANSACTION-SERVICE
    │  ├─ Instance 1: 10.0.1.80:8084
    │  └─ Status: UP
    │
    └─ ... other services

Service Connect override:
├─ For ECS services, use Service Connect DNS
├─ eureka-server-8761-tcp.citicore:8761
├─ Avoids depending on Eureka for critical paths
└─ Eureka used for additional discovery

Why Eureka:
├─ Auto-discovery of new instances
├─ Handles instance restarts
├─ Load balancing support
├─ Health check integration
└─ Client-side cache for resilience
```

### API Gateway (Port 8080)

```
Responsibility:
├─ External API entry point
├─ Request routing
├─ Gateway-level controls
└─ Cross-cutting concerns

How it works:

1. External traffic arrives
2. Gateway routes based on path
3. Applies rate limiting, circuit breaker, retry
4. Forwards to backend service
5. Receives response
6. Sends back to client

Routing:

External Path          Internal Route
───────────────────────────────────────
/auth/**         →     Auth :8081/api/v1/auth/$1
/users/**        →     User :8082/api/v1/users/$1
/accounts/**     →     Account :8083/api/v1/accounts/$1
/transaction/**  →     Transaction :8084/api/v1/transaction/$1

Example:

Client: POST /accounts/create
    └─ Becomes: POST /api/v1/accounts/create to Account Service

Gateway controls:

├─ Rate Limiting
│  └─ Redis-backed
│  └─ Per-IP limits
│
├─ Circuit Breaker
│  └─ If backend down, return 503
│  └─ Prevent cascade failures
│
├─ Retry
│  └─ Automatic retry on failure
│  └─ Exponential backoff
│
└─ Path Rewriting
   └─ External vs internal API paths differ
   └─ Gateway translates

Configuration:
├─ Spring Cloud Gateway
├─ Reactive architecture
├─ Low latency
└─ High throughput
```

### Auth Service (Port 8081)

```
Responsibility:
├─ User authentication
├─ JWT token issuance
├─ Email verification
└─ Security operations

How it works:

1. User sends credentials
2. Auth Service validates
3. Issues JWT token
4. Client includes JWT in subsequent requests
5. API Gateway validates JWT
6. Services receive validated token

Endpoints:

POST /api/v1/auth/register
    └─ New user registration

POST /api/v1/auth/login
    └─ User login
    └─ Returns JWT token

POST /api/v1/auth/verify-email
    └─ Email verification

POST /api/v1/auth/refresh-token
    └─ Refresh expired token

JWT Token Structure:

Header:
{
  "alg": "HS256",
  "typ": "JWT"
}

Payload:
{
  "userId": "abc123",
  "email": "user@example.com",
  "roles": ["USER"],
  "iat": 1620000000,
  "exp": 1620003600
}

Signature:
HMAC(Header + Payload, Secret)

Security flow:

1. JWT issued on login
2. Client stores JWT
3. Client includes in Authorization header
   └─ Authorization: Bearer eyJhbG...

4. API Gateway validates JWT
5. Extract user info
6. Propagate to backend services

Token propagation:

Auth checks JWT
    │
    v
API Gateway receives JWT
    │
    v
Validates signature
    │
    v
Extracts user info
    │
    v
Forwards to backend (via OpenFeign interceptor)
    │
    v
Backend service receives user context
```

### User Service (Port 8082)

```
Responsibility:
├─ User profile management
├─ KYC (Know Your Customer)
├─ User details
└─ User operations

How it works:

1. Users register via Auth Service
2. User Service stores profile
3. User completes KYC
4. KYC documents stored in S3
5. KYC status tracked

User Profile:

- userId
- Email
- Phone
- Address
- Name
- Registration date

KYC Workflow:

1. User submits documents
   └─ ID proof, Address proof, etc.

2. Documents uploaded to S3
   └─ Secure storage
   └─ Immutable

3. KYC status updated
   └─ PENDING → UNDER_REVIEW → APPROVED/REJECTED

4. Notifications sent
   └─ Async via Kafka/email

Endpoints:

GET /api/v1/users/{userId}
    └─ Get user profile

PUT /api/v1/users/{userId}
    └─ Update user profile

POST /api/v1/users/{userId}/kyc
    └─ Submit KYC documents

GET /api/v1/users/{userId}/kyc-status
    └─ Check KYC status

Database:

Users Table:
- id (UUID)
- email
- phone
- address
- name
- status (ACTIVE, INACTIVE)
- created_at
- updated_at

KYC Table:
- id
- user_id
- document_type (ID, Address, etc.)
- s3_key (where file stored)
- status (PENDING, APPROVED, REJECTED)
- created_at
- reviewed_at
```

### Account Service (Port 8083)

```
Responsibility:
├─ Account creation
├─ Account operations
├─ Balance management
├─ Read/write coordination
└─ Account inquiries

How it works:

1. User creates account
2. Account stored in RDS
3. Data cached in Redis
4. Debit/credit operations
5. Balance updated
6. Events published

Account Types:

- Savings Account
  └─ Interest accrual
  └─ Daily deposit limits

- Current Account
  └─ For businesses
  └─ No interest

Operations:

Deposit
    └─ Increase balance

Withdrawal
    └─ Decrease balance
    └─ Check daily limits

Transfer (via Transaction Service)
    └─ Debit sender
    └─ Credit receiver
    └─ Saga pattern for consistency

Endpoints:

POST /api/v1/accounts/create
    └─ Create new account

GET /api/v1/accounts/my-accounts
    └─ List user accounts

GET /api/v1/accounts/balance/{accountNo}
    └─ Check balance

POST /api/v1/accounts/deposit
    └─ Deposit money

POST /api/v1/accounts/withdraw
    └─ Withdraw money

Database Architecture:

Primary Database (writes):
citicore-mysql-primary.cnk8ckkm2hsk.ap-south-1.rds.amazonaws.com

Replica Database (reads):
citicore-mysql-replica.cnk8ckkm2hsk.ap-south-1.rds.amazonaws.com

Schema:

Accounts Table:
- id (UUID)
- user_id
- account_type (SAVINGS, CURRENT)
- account_number
- balance
- created_at

Transactions Table:
- id
- account_id
- type (DEPOSIT, WITHDRAWAL)
- amount
- balance_after
- created_at

Caching Strategy:

Read from cache:
GET /api/v1/accounts/balance/{accountNo}
    └─ Check Redis first
    └─ If miss, query RDS
    └─ Update cache

Invalidate cache:
POST /api/v1/accounts/deposit
    └─ Update database
    └─ Delete from Redis
    └─ Next read will refresh

Query Routing:

Read operations:
├─ If read-only: query Replica
├─ Check cache first
├─ Update cache on miss

Write operations:
├─ Always query Primary
├─ Update database
├─ Invalidate cache
```

### Transaction Service (Port 8084)

```
Responsibility:
├─ Transaction orchestration
├─ Saga pattern coordination
├─ Transfer processing
├─ Event-driven workflow
└─ Transaction status tracking

How it works:

1. User initiates transfer
2. Transaction Service validates
3. Calls Account Service to validate balance
4. Creates transaction record
5. Publishes events to Kafka
6. Account Service processes events
7. Result events received
8. Transaction status updated

Transaction Saga:

Client Request: Transfer $5000 from A to B
    │
    v
Transaction Service
    ├─ Validate JWT
    ├─ Validate daily limits
    ├─ Call Account Service /validate-transfer
    │
    v
Account Service Response: OK
    │
    v
Transaction Service
    ├─ Save transaction (PENDING)
    ├─ Write to outbox (DEBIT event, PENDING)
    │
    v
OutboxPublisher (polls every 5s)
    ├─ Find PENDING events
    ├─ Publish DEBIT to Kafka
    ├─ Wait for Kafka ack
    ├─ Mark SENT
    │
    v
Kafka Event
    │
    v
Account Service (consumer)
    ├─ Debit sender account
    ├─ Publish debit-success to Kafka
    │
    v
Kafka Event (outbox again)
    ├─ Account Service publishes CREDIT event
    │
    v
Account Service (consumer, second event)
    ├─ Credit receiver account
    ├─ Publish credit-success to Kafka
    │
    v
Kafka Result Events
    │
    v
Transaction Service (result consumer)
    ├─ Receive debit-success
    ├─ Receive credit-success
    ├─ Update status: COMPLETED
    │
    v
Response to Client: Transfer successful

Failure Scenario:

Transaction Service
    ├─ Receive debit-success
    ├─ Receive credit-failed
    │
    v
Transaction Service
    ├─ Update status: FAILED
    ├─ Write REVERSAL event to outbox
    │
    v
OutboxPublisher
    ├─ Publish REVERSAL to Kafka
    │
    v
Account Service
    ├─ Reverse debit
    ├─ Publish reversal-success
    │
    v
Transaction Service
    ├─ Receive reversal-success
    ├─ Update status: REVERSED
    │
    v
End: Transaction cleaned up, money returned

Endpoints:

POST /api/v1/transactions/transfer
    └─ Initiate transfer

GET /api/v1/transactions/{txnId}
    └─ Get transaction status

Database:

Transactions Table:
- id (UUID)
- sender_account_id
- receiver_account_id
- amount
- status (PENDING, DEBIT_SUCCESS, CREDIT_SUCCESS, COMPLETED, FAILED, REVERSED)
- created_at
- updated_at

Outbox Events Table:
- id
- transaction_id
- event_type (DEBIT, CREDIT, REVERSAL)
- payload (JSON)
- status (PENDING, SENT, FAILED)
- created_at
- published_at
```

### Notification Service

```
Responsibility:
├─ Send email notifications
├─ Send SMS notifications
├─ Async notification delivery
└─ Notification tracking

How it works:

1. Other services publish notification events
2. Notification Service consumes events
3. Renders email template
4. Sends via SendGrid or internal SMTP
5. Tracks delivery status

Event Types:

User Events:
├─ Registration confirmation
├─ Email verification
├─ Password reset

Account Events:
├─ Account created
├─ Account closed
├─ Statement available

Transaction Events:
├─ Transfer initiated
├─ Transfer completed
├─ Transfer failed
├─ Reversal processed

KYC Events:
├─ KYC submitted
├─ KYC approved
├─ KYC rejected

Architecture:

Service publishes event
    │
    v
Kafka Topic (notification-topic)
    │
    v
Notification Service consumes
    │
    v
Email/SMS Provider (SendGrid)
    │
    v
User receives notification

Async advantages:
├─ Main service doesn't wait for email
├─ Better performance
├─ Retry on failure
├─ No user impact if email fails
```

---

## Local Development

### Docker Compose Setup

```yaml
# Typical local development environment

version: '3.8'

services:
  kafka:
    image: apache/kafka:4.0.1
    ports:
      - "9092:9092"
    environment:
      KAFKA_BROKER_ID: 1
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://localhost:9092
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"

  mysql:
    image: mysql:8.0
    ports:
      - "3306:3306"
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: citicore

volumes:
  mysql_data:
```

### Local Development Sequence

```
Step 1: Start Infrastructure
├─ docker compose up -d
├─ Starts Kafka, Redis, MySQL
├─ Wait 10-15 seconds for startup

Step 2: Start Config Server
├─ Clone config repo
├─ cd config-service
├─ mvn spring-boot:run
├─ Port 8888 available

Step 3: Start Eureka
├─ cd eureka-server
├─ mvn spring-boot:run
├─ Port 8761 available

Step 4: Start Auth Service
├─ cd auth-service
├─ mvn spring-boot:run
├─ Port 8081, registers with Eureka

Step 5: Start Other Services
├─ Same pattern for User, Account, Transaction
├─ Each gets its port
├─ All register with Eureka

Step 6: Start API Gateway
├─ cd apigateway-service
├─ mvn spring-boot:run
├─ Port 8080

Step 7: Test APIs
├─ Register user via Auth Service
├─ Create account via Account Service
├─ Test transfer via Transaction Service

Monitoring:

View logs:
├─ docker compose logs -f
├─ docker compose logs -f kafka
├─ docker compose logs -f redis

Kafka topics:
├─ docker compose exec kafka kafka-topics.sh --bootstrap-server localhost:9092 --list

Redis:
├─ docker compose exec redis redis-cli
├─ > keys *

MySQL:
├─ docker compose exec mysql mysql -u root -p
├─ > USE citicore;
├─ > SHOW TABLES;

Stop environment:
└─ docker compose down
```

---

## Configuration Management

### Configuration Repository

```
Repository:
https://github.com/Rabbaniinamdar/citicore-config-repo.git

Git-backed configuration
├─ All configs version controlled
├─ Audit trail
├─ Rollback capability

Directory structure:

config-repo/
├─ application.yml (shared)
├─ auth-service.yml
├─ user-service.yml
├─ account-service.yml
├─ transaction-service.yml
├─ apigateway-service.yml
└─ ... other services

Configuration hierarchy:

1. application.yml (lowest priority)
   └─ Common settings

2. Service-specific file (higher priority)
   └─ Overrides common settings

3. Spring profile (highest priority)
   └─ Environment-specific overrides
```

### Application Configuration Example

```yaml
# Auth Service Configuration

spring:
  application:
    name: auth-service

  datasource:
    url: jdbc:mysql://${AUTH_DB_HOST}:3306/citicore_authdb
    username: ${DB_USERNAME}
    password: ${DB_PASSWORD}

  jpa:
    hibernate:
      ddl-auto: validate

  security:
    jwt:
      secret: ${JWT_SECRET}
      expiration: 3600000  # 1 hour

  mail:
    host: ${SMTP_HOST}
    port: ${SMTP_PORT}
    username: ${SMTP_USERNAME}
    password: ${SMTP_PASSWORD}

eureka:
  client:
    service-url:
      defaultZone: http://eureka-server-8761-tcp.citicore:8761/eureka

management:
  endpoints:
    web:
      exposure:
        include: health,metrics,prometheus
```

### Environment Variables (Production)

```
Should NOT be in Git:

AUTH_DB_HOST → AWS RDS endpoint
DB_USERNAME → RDS username
DB_PASSWORD → RDS password (from Secrets Manager)
JWT_SECRET → JWT signing key (from Secrets Manager)
SMTP_PASSWORD → Email provider password (from Secrets Manager)
AWS_ACCESS_KEY_ID → NOT USED (use EC2 IAM role instead)
AWS_SECRET_ACCESS_KEY → NOT USED (use EC2 IAM role instead)
KAFKA_BOOTSTRAP_SERVERS → Kafka cluster address
REDIS_HOST → Redis endpoint
REDIS_PASSWORD → Redis password if needed

Should be in Git (configuration):

Spring profiles
├─ dev
├─ test
├─ prod

Timeout values
├─ Connection timeouts
├─ Read timeouts
└─ Request timeouts

Business logic settings
├─ Daily limits
├─ Interest rates
└─ Fee structures

Logging levels
├─ Root level
├─ Service level
└─ Database level
```

### Security Principle

```
Git Repository (public/internal):
├─ Application configuration
├─ Database URLs
├─ Service endpoints
├─ Logging levels
└─ Feature flags

AWS Secrets Manager (encrypted):
├─ Database passwords
├─ JWT signing secrets
├─ API keys
├─ OAuth tokens
└─ Sensitive credentials

Never commit to Git:
❌ Passwords
❌ API keys
❌ Private keys
❌ OAuth tokens
❌ AWS credentials
❌ Any sensitive data
```

---

## Service Discovery

### Eureka Architecture

```
Eureka Server (service registry)
    │
    ├─ Maintains list of all services
    ├─ Tracks health
    ├─ Auto-registration
    └─ Client queries

Service Registration:

When Auth Service starts:
├─ Connects to Eureka
├─ Sends: "I am AUTH-SERVICE"
├─ Sends: "My IP is 10.0.1.50"
├─ Sends: "My port is 8081"
├─ Sends: "My health check URL is /actuator/health"
├─ Eureka stores this info
├─ Service sends heartbeat every 30s
└─ If no heartbeat, marked as DOWN

Service Discovery:

When Transaction Service needs Account Service:
├─ Queries Eureka: "Where is ACCOUNT-SERVICE?"
├─ Eureka returns: "At 10.0.1.75:8083"
├─ Opens connection
├─ Sends request
└─ Receives response

Eureka Configuration:

URL:
http://eureka-server-8761-tcp.citicore:8761/eureka

Service-side config:

eureka:
  client:
    service-url:
      defaultZone: http://eureka-server-8761-tcp.citicore:8761/eureka
    fetch-registry: true
    register-with-eureka: true

  instance:
    prefer-ip-address: true
    instance-id: ${spring.application.name}:${spring.application.instance_id:${random.value}}

Heartbeat:

├─ Interval: 30 seconds
├─ Timeout: 90 seconds
├─ If no heartbeat for 90s: marked DOWN
├─ If no heartbeat for 180s: removed

Health Check:

Eureka calls /actuator/health on each service
├─ 200 OK → status UP
├─ Any error → status DOWN
└─ Stops sending traffic
```

### Service Connect DNS (ECS Alternative)

```
For ECS services, use Service Connect:

Service Connect Namespace:
citicore

Known hostnames:

auth-service-8081-tcp.citicore:8081
user-service-8082-tcp.citicore:8082
citicore-account-service-8083-tcp.citicore:8083
eureka-server-8761-tcp.citicore:8761

Example usage:

Transaction Service needs Account Service:
├─ OpenFeign target: account-service
├─ Spring resolves to: citicore-account-service-8083-tcp.citicore:8083
├─ ECS Service Connect routes to correct task IP
└─ Connection established

Benefits:

├─ No external DNS dependency
├─ Works even if Eureka down
├─ Automatic failover
├─ Task replacement automatic
└─ High availability

Comparison:

Eureka (service discovery):
├─ Client-side discovery
├─ Services query registry
├─ More control
└─ More complex

Service Connect (DNS-based):
├─ DNS-based discovery
├─ Services use hostname
├─ Simpler
└─ AWS-managed
```

---

## AWS Infrastructure

### AWS Account Details

```
Account ID: 580655778303
Region: ap-south-1 (Mumbai)
├─ Closest to Bangladesh (banking customers)
├─ Multiple availability zones
├─ Good AWS service coverage

Primary AZ: ap-south-1a
├─ ECS tasks
├─ RDS primary

Backup AZ: ap-south-1b
├─ RDS replica
├─ Future failover

Tagging strategy:

Resource tags:
├─ Environment: production
├─ Project: citicore
├─ Team: platform
├─ Cost-center: banking
└─ Backup: enabled

Why tagging matters:
├─ Cost allocation
├─ Resource organization
├─ Automation
├─ Compliance
└─ Cleanup automation
```

### VPC Configuration

```
VPC: vpc-0d064f45265cbcdad
CIDR: 10.0.0.0/16

Subnets:

Public Subnets (for ALB, NAT):
├─ 10.0.1.0/24 (ap-south-1a)
│  └─ Jenkins, ALBs
├─ 10.0.2.0/24 (ap-south-1b)
│  └─ Future failover

Private Subnets (for ECS, RDS):
├─ 10.0.11.0/24 (ap-south-1a)
│  └─ ECS tasks
├─ 10.0.12.0/24 (ap-south-1b)
│  └─ RDS replica

Internet Gateway:
└─ Attached to VPC
└─ Allows public traffic

NAT Gateway:
├─ In public subnet
├─ Allows private subnets to reach internet
├─ For downloading packages, external APIs

Route Tables:

Public Route Table:
├─ 10.0.0.0/16 → local
├─ 0.0.0.0/0 → Internet Gateway

Private Route Table:
├─ 10.0.0.0/16 → local
├─ 0.0.0.0/0 → NAT Gateway

Network ACLs:

Default: All traffic allowed
├─ Can be restrictive if needed
├─ Stateless (must allow both directions)

VPC Endpoints (optional):

For Secrets Manager:
├─ Private connection
├─ No internet routing needed
├─ Reduces data egress

For S3:
├─ Gateway endpoint
├─ Direct S3 access
└─ No charge
```

### Key Resource Endpoints

```
ECS Cluster:
citicore-cluster

Services:
├─ citicore-config-server-service-rwdtvpj9
├─ citicore-eureka-server
├─ citicore-auth-service
├─ citicore-user-service
├─ citicore-account-service
├─ citicore-transaction-service
├─ citicore-notification-service-service-0nnwrup9
└─ citicore-apigateway-service

ECR Registry:
580655778303.dkr.ecr.ap-south-1.amazonaws.com

Repositories:
├─ citicore/config-service
├─ citicore/eureka-server
├─ citicore/auth-service
├─ citicore/user-service
├─ citicore/account-service
├─ citicore/transaction-service
├─ citicore/notification-service
└─ citicore/apigateway-service

RDS Instances:
├─ Primary: citicore-mysql-primary.cnk8ckkm2hsk.ap-south-1.rds.amazonaws.com:3306
├─ Replica: citicore-mysql-replica.cnk8ckkm2hsk.ap-south-1.rds.amazonaws.com:3306
└─ Multi-AZ deployment

Redis:
├─ Endpoint: (configured in application)
├─ Port: 6379
└─ Used for rate limiting, caching

Kafka:
├─ EC2 instance: 10.0.1.87
├─ Port: 9092
├─ KRaft mode (no Zookeeper)
└─ Single broker (non-HA)
```

---

## VPC and Networking

### Network Architecture

```
Internet
    │
    v
Route 53 (DNS)
    │
    v
Internet Gateway (attached to VPC)
    │
    v
Public Subnets
├─ 10.0.1.0/24 (Jenkins, ALB)
├─ 10.0.2.0/24 (Reserved)
    │
    v
ALB (Application Load Balancer)
├─ Gateway ALB :8080
└─ Auth ALB :80
    │
    v
NAT Gateway (for egress)
    │
    v
Private Subnets
├─ 10.0.11.0/24 (ECS)
├─ 10.0.12.0/24 (RDS, Reserved)
    │
    +─────────────┬────────────┐
    │             │            │
    v             v            v
  ECS         Redis         RDS
  Tasks       Cluster     Database
```

### Traffic Flow

```
Internet Client
    │
    v
ALB (public subnet)
    │ (forwards to ECS)
    v
ECS Tasks (private subnet)
    │ (ECS security group allows from ALB)
    │
    ├─ To RDS: RDS security group allows from ECS
    ├─ To Redis: connects directly (same security group or allow)
    ├─ To Kafka: connects to EC2 (security group rule)
    └─ To other ECS tasks: ECS security group allows
    │
    v
Response back to ALB
    │
    v
Response back to Internet Client
```

### Security Group Rules

```
Internet Traffic:
Internet
    │ (port 80/443)
    v
ALB Security Group (ingress)
├─ HTTP :80 from 0.0.0.0/0
└─ (HTTPS :443 if configured)

ALB to ECS:
ALB
    │ (port 8080, 8081, etc.)
    v
ECS Security Group (ingress)
├─ TCP 8080 from ALB-SG (API Gateway)
├─ TCP 8081 from ALB-SG (Auth)
└─ (other ports as needed)

ECS to ECS (internal):
ECS Task A
    │ (port 8082)
    v
ECS Security Group (ingress)
└─ TCP 8082 from ECS-SG (allow self-references)

ECS to RDS:
ECS Tasks
    │ (port 3306)
    v
RDS Security Group (ingress)
└─ TCP 3306 from ECS-SG

ECS to EC2 (Kafka/Redis):
ECS Tasks
    │ (port 9092, 6379)
    v
EC2 Security Group (ingress)
├─ TCP 9092 from ECS-SG (Kafka)
└─ TCP 6379 from ECS-SG (Redis)

Egress (all allow):
ECS Security Group (egress)
└─ All traffic to 0.0.0.0/0
```

---

## ECS and Fargate

### ECS Cluster

```
Cluster Name: citicore-cluster

Launch Type: AWS Fargate
├─ Serverless containers
├─ No EC2 instances to manage
├─ Pay per vCPU-hour
└─ Auto-scaling

Fargate Platform: 1.4.0
├─ Latest stable
├─ Latest feature support
└─ Good compatibility

Services: 8 total

Config Server
├─ Task Definition: citicore-config-server
├─ CPU: 0.25 vCPU
├─ Memory: 512 MB
├─ Port: 8888
└─ Status: RUNNING

Eureka Server
├─ Task Definition: citicore-eureka-server
├─ CPU: 0.25 vCPU
├─ Memory: 512 MB
├─ Port: 8761
└─ Status: RUNNING

Auth Service
├─ Task Definition: citicore-auth-service
├─ CPU: 0.5 vCPU
├─ Memory: 1 GB
├─ Port: 8081
└─ Status: RUNNING

User Service
├─ Task Definition: citicore-user-service
├─ CPU: 0.5 vCPU
├─ Memory: 1 GB
├─ Port: 8082
└─ Status: RUNNING

Account Service
├─ Task Definition: citicore-account-service
├─ CPU: 0.5 vCPU
├─ Memory: 1 GB
├─ Port: 8083
├─ Connected to RDS
└─ Status: RUNNING

Transaction Service
├─ Task Definition: citicore-transaction-service
├─ CPU: 0.5 vCPU
├─ Memory: 1 GB
├─ Port: 8084
└─ Status: RUNNING

Notification Service
├─ Task Definition: citicore-notification-service
├─ CPU: 0.25 vCPU
├─ Memory: 512 MB
├─ Port: N/A (background service)
└─ Status: RUNNING

API Gateway
├─ Task Definition: citicore-apigateway-service
├─ CPU: 0.5 vCPU
├─ Memory: 1 GB
├─ Port: 8080
└─ Status: RUNNING
```

### Task Definition

```
Task Definition = Template for running containers

Structure:

{
  "family": "citicore-account-service",
  "revision": 24,
  "taskRoleArn": "arn:aws:iam::580655778303:role/citicore-ecs-task-role",
  "executionRoleArn": "arn:aws:iam::580655778303:role/citicore-ecs-task-execution-role",
  "cpu": "512",
  "memory": "1024",
  "networkMode": "awsvpc",
  "requiresCompatibilities": ["FARGATE"],
  "containerDefinitions": [
    {
      "name": "citicore-account-service",
      "image": "580655778303.dkr.ecr.ap-south-1.amazonaws.com/citicore/account-service:GITSHA",
      "portMappings": [
        {
          "containerPort": 8083,
          "hostPort": 8083,
          "protocol": "tcp"
        }
      ],
      "environment": [
        {
          "name": "ACCOUNT_DB_HOST",
          "value": "citicore-mysql-primary.cnk8ckkm2hsk.ap-south-1.rds.amazonaws.com"
        }
      ],
      "secrets": [
        {
          "name": "DB_PASSWORD",
          "valueFrom": "arn:aws:secretsmanager:ap-south-1:580655778303:secret:citicore/auth-service/database-tXq98R"
        }
      ],
      "logConfiguration": {
        "logDriver": "awslogs",
        "options": {
          "awslogs-group": "/ecs/citicore-account-service",
          "awslogs-region": "ap-south-1",
          "awslogs-stream-prefix": "ecs"
        }
      }
    }
  ]
}

Revision History:

Revision 23:
├─ Image SHA: 25951ba1...
├─ Date: 2 weeks ago
└─ Status: SUPERSEDED

Revision 24:
├─ Image SHA: 434a66ea...
├─ Date: Today
└─ Status: ACTIVE (currently running)
```

### ECS Service

```
Service: citicore-account-service

Configuration:

Cluster: citicore-cluster
Task Definition: citicore-account-service:24
Launch Type: FARGATE
Desired Count: 1
    └─ How many tasks should run

Min: 1
Max: 3 (for auto-scaling)

Network Configuration:
├─ Subnets:
│  └─ subnet-0f93a5d26ff85c18c (private)
├─ Security Groups:
│  └─ sg-0b8ac20679059c792 (ECS SG)
└─ Assign Public IP: DISABLED
   └─ Tasks run in private subnet

Load Balancing:

Target Group: citicore-account-tg
├─ Protocol: HTTP
├─ Port: 8083
├─ Health Check: /actuator/health
└─ Healthy Threshold: 2

ALB Integration:
├─ Load Balancer: citicore-account-alb
├─ Listener: port 8083
├─ Path Routing: All traffic
└─ Forwards to: Target Group

Service Connect:

Namespace: citicore
Port: 8083
DNS: citicore-account-service-8083-tcp.citicore:8083

Deployment Configuration:

Maximum Percent: 200%
├─ Can have up to 2 tasks running (100% + 100%)
├─ One old, one new

Minimum Healthy Percent: 100%
├─ Must have 1 task running at all times
├─ Zero downtime deployment

Rolling Update:
Start new task
    ↓
Wait for health check
    ↓
Register with ALB
    ↓
Drain old task (stop accepting traffic)
    ↓
Stop old task
    ↓
Result: Seamless update

Update Service:

When Jenkins deploys:
├─ ECS detects new task definition revision
├─ Triggers rolling update
├─ Old tasks stopped
├─ New tasks started
├─ ALB routes to new tasks
└─ Deployment complete (typically 2-5 minutes)
```

---

## ECS Service Connect

### What It Does

```
ECS Service Connect: DNS-based service discovery within ECS

Benefits:

├─ Services discover each other by hostname
├─ No external DNS needed
├─ Automatic task failover
├─ Private networking
└─ Simple and managed by AWS
```

### Service Connect Namespace

```
Namespace: citicore

Services in namespace:

auth-service-8081-tcp.citicore:8081
user-service-8082-tcp.citicore:8082
citicore-account-service-8083-tcp.citicore:8083
eureka-server-8761-tcp.citicore:8761

Example resolution:

API Gateway needs User Service
    │
    ├─ @FeignClient(name = "user-service")
    │
    v
Spring Cloud client resolves hostname
    │
    ├─ user-service-8082-tcp.citicore:8082
    │
    v
ECS Service Connect DNS server
    │
    ├─ Routes to current User Service task IP
    │
    v
User Service task receives request
    │
    v
Response sent back
```

### Configuration

```
In task definition:

"portMappings": [
  {
    "containerPort": 8082,
    "hostPort": 8082,
    "protocol": "tcp",
    "name": "user-service-8082"
  }
]

In ECS service:

serviceConnectConfiguration:
  enabled: true
  namespace: "citicore"
  services:
    - name: "user-service"
      portMappings:
        - containerPort: 8082
          discoveryName: "user-service-8082-tcp"

Result:

Services can reference each other via:
http://user-service-8082-tcp.citicore:8082

No external DNS needed
No Eureka registry needed (but can still use both)
```

---

## Application Load Balancers

### Gateway ALB (Primary Entry Point)

```
Name: citicore-gateway-alb
DNS: citicore-gateway-alb-980017400.ap-south-1.elb.amazonaws.com

Listener:
├─ HTTP :8080
├─ Routes to API Gateway :8080

Configuration:
├─ Network Load Balancer style: ALB
├─ Scheme: Internet-facing
├─ VPC: vpc-0d064f45265cbcdad
├─ Subnets:
│  ├─ 10.0.1.0/24
│  └─ 10.0.2.0/24
├─ Security Group: citicore-alb-sg

Target Group:
├─ Name: citicore-gateway-tg
├─ Protocol: HTTP
├─ Port: 8080
├─ Path-based routing: /

Health Check:
├─ Protocol: HTTP
├─ Path: /actuator/health
├─ Port: traffic port (8080)
├─ Interval: 30 seconds
├─ Timeout: 5 seconds
├─ Healthy threshold: 2
├─ Unhealthy threshold: 3

Usage:

External client:
http://citicore-gateway-alb-980017400.ap-south-1.elb.amazonaws.com:8080/accounts/create

    ↓

ALB forwards to:
http://<api-gateway-task-ip>:8080/accounts/create

    ↓

API Gateway routes to:
/api/v1/accounts/create → Account Service
```

### Auth ALB (Standalone)

```
Name: citicore-auth-alb
DNS: citicore-auth-alb-2119568508.ap-south-1.elb.amazonaws.com

Listener:
├─ HTTP :80
├─ Routes to Auth Service :8081

Purpose:
├─ Direct access to Auth Service
├─ Separate from main Gateway
├─ For authentication workflows

Target Group:
├─ Protocol: HTTP
├─ Port: 8081

Usage:

POST /auth/register
POST /auth/login
POST /auth/verify-email

Can be called directly or via Gateway
```

### ALB Health Checks

```
Health check process:

Every 30 seconds:
├─ ALB connects to target
├─ Sends GET /actuator/health
├─ Expects 200 OK response

Target states:

Healthy:
├─ Receives 200 OK
├─ Consecutive successful checks: 2
├─ Receives traffic

Unhealthy:
├─ Timeout or error response
├─ Consecutive failed checks: 3
├─ NO traffic routed

Draining:
├─ Service marked for shutdown
├─ New connections not accepted
├─ Existing connections drain
├─ Duration: configurable (default 300s)

Grace period:

ECS Health Check Grace Period: 300 seconds
├─ After task starts, wait 300s before health check
├─ Allows Spring Boot startup time (~90-120s)
├─ Prevents premature failure
```

---

## API Gateway

### Routing Configuration

```
API Gateway routes external traffic to internal services

Route Configuration:

External Path              → Internal Service
─────────────────────────────────────────────────
/auth/**                  → auth-service-8081-tcp.citicore:8081
/users/**                 → user-service-8082-tcp.citicore:8082
/accounts/**              → citicore-account-service-8083-tcp.citicore:8083
/transaction/**           → transaction-service-8084-tcp.citicore:8084

Path Rewriting:

Example request:
POST /accounts/create

    ↓

API Gateway:
├─ Matches: /accounts/** route
├─ Extracts: accounts/create
├─ Rewrites to: /api/v1/accounts/create
├─ Forwards to Account Service

Spring Cloud Gateway config:

spring:
  cloud:
    gateway:
      routes:
        - id: accounts
          uri: http://citicore-account-service-8083-tcp.citicore:8083
          predicates:
            - Path=/accounts/**
          filters:
            - RewritePath=/accounts/(?<segment>.*), /api/v1/accounts/$\{segment}
            - name: CircuitBreaker
              args:
                name: accountsCircuitBreaker
            - name: Retry
              args:
                retries: 1
                backoff:
                  delay: 100
                  maxDelay: 500
                  multiplier: 2
            - name: RequestRateLimiter
              args:
                redis-rate-limiter:
                  replenishRate: 2
                  burstCapacity: 4
```

### Gateway Controls

```
Rate Limiting:

Configuration:
├─ Replenish Rate: 2 requests/second
├─ Burst Capacity: 4 requests
├─ Redis-backed
└─ Per-IP tracking

Example:

IP: 192.168.1.100
├─ Second 1: 4 requests allowed (burst)
├─ Second 2: 2 requests allowed (rate)
├─ Second 3: 2 requests allowed (rate)
└─ Over limit: 429 Too Many Requests

Circuit Breaker:

Configuration:
├─ Failure threshold: 50%
├─ Sliding window: 10 requests
├─ Open state duration: 10 seconds
└─ Half-open attempts: 2

States:

CLOSED: Normal, requests pass through

OPEN: Too many failures, requests rejected
  └─ Gives backend time to recover

HALF_OPEN: Trying recovery, test requests sent

Retry:

Configuration:
├─ Max retries: 1
├─ Backoff: exponential (100ms → 500ms)
└─ Retryable methods: GET

When to retry:
├─ BAD_GATEWAY (502)
├─ SERVICE_UNAVAILABLE (503)
├─ GATEWAY_TIMEOUT (504)
└─ Network errors

When NOT to retry:
├─ 4XX client errors (bad request)
├─ 500 internal server error (server bug)
└─ Non-idempotent methods (POST without retry count)
```

---

## Kafka Event Architecture

### Event-Driven Communication

```
Purpose:

├─ Asynchronous messaging
├─ Service decoupling
├─ Event replay capability
├─ Saga coordination
└─ Eventual consistency

How Kafka works:

1. Producer sends event
2. Event stored in partition
3. Consumer reads event
4. Consumer processes
5. Offset committed
6. Next event processed
```

### Topics & Events

```
Transaction Saga Topics:

1. debit-topic
   ├─ Published by: Transaction Service (outbox)
   ├─ Consumed by: Account Service
   └─ Event: {"transactionId": "...", "amount": 5000}

2. credit-topic
   ├─ Published by: Transaction Service (outbox)
   ├─ Consumed by: Account Service
   └─ Event: {"transactionId": "...", "amount": 5000}

3. debit-success-topic
   ├─ Published by: Account Service
   ├─ Consumed by: Transaction Service
   └─ Event: {"transactionId": "...", "status": "SUCCESS"}

4. debit-failed-topic
   ├─ Published by: Account Service
   ├─ Consumed by: Transaction Service
   └─ Event: {"transactionId": "...", "reason": "..."}

5. credit-success-topic
   ├─ Published by: Account Service
   ├─ Consumed by: Transaction Service
   └─ Event: {"transactionId": "...", "status": "SUCCESS"}

6. credit-failed-topic
   ├─ Published by: Account Service
   ├─ Consumed by: Transaction Service
   └─ Event: {"transactionId": "...", "reason": "..."}

7. reversal-topic
   ├─ Published by: Transaction Service (on failure)
   ├─ Consumed by: Account Service
   └─ Event: {"transactionId": "...", "amount": 5000}

8. reversal-success-topic
   ├─ Published by: Account Service
   ├─ Consumed by: Transaction Service
   └─ Event: {"transactionId": "...", "status": "SUCCESS"}

Dead Letter Topics (DLT):

For each topic, optional DLT:
├─ debit-topic.DLT
├─ credit-topic.DLT
├─ reversal-topic.DLT
└─ etc.

DLT usage:
├─ Message fails after 3 retries
├─ Sent to DLT
├─ Admin inspects failure
├─ Can manually retry or fix
```

### Shared kafka-events Module

```
Directory: kafka-events/

Purpose:
├─ Shared event definitions
├─ Type-safe events
├─ Event schema versioning
└─ Single source of truth

Example Event Class:

package com.citicore.kafka;

public class TransactionEvent {
    private String transactionId;
    private String type;  // DEBIT, CREDIT, REVERSAL
    private BigDecimal amount;
    private String senderAccount;
    private String receiverAccount;
    private LocalDateTime timestamp;
    
    // Getters, Setters, Equals, HashCode
}

CI/CD Dependency:

When kafka-events/ changes:
├─ All consumers need rebuild
├─ Triggered services:
│  ├─ account-service
│  ├─ transaction-service
│  ├─ user-service
│  ├─ auth-service
│  └─ notification-service
├─ Pipeline builds shared module first
├─ Then builds all consumers
└─ All deployed with same schema version

Build order:

mvn -pl kafka-events -am clean install -DskipTests
  └─ Build kafka-events
  └─ Install to local Maven repo

Then consumers can use:

mvn -pl account-service -am clean package -DskipTests
  └─ Depends on kafka-events
  └─ Uses updated events
```

---

## Redis Caching

### Purpose

```
Redis (in-memory data store):

├─ Fast caching layer
├─ Session storage
├─ Rate limiting counters
├─ Lock management
└─ Temporary data
```

### Usage in Platform

```
Account Service Caching:

GET /api/v1/accounts/balance/{accountNo}
    │
    ├─ Check Redis:
    │  └─ Cache key: "account:balance:{accountNo}"
    │  └─ If hit: return cached value
    │  └─ If miss: continue
    │
    ├─ Query RDS:
    │  └─ SELECT balance FROM accounts WHERE number = ?
    │
    ├─ Update Redis:
    │  └─ SET account:balance:{accountNo} {balance} EX 3600
    │  └─ Expire after 1 hour
    │
    └─ Return balance

Cache Invalidation:

POST /api/v1/accounts/deposit
    │
    ├─ Update RDS (transaction)
    ├─ Delete from Redis
    │  └─ DEL account:balance:{accountNo}
    │  └─ Remove cached value
    │
    └─ Next read will refresh cache

Rate Limiting:

API Gateway Rate Limiter:

FOR each request from IP:
    ├─ Get counter from Redis
    │  └─ Key: "ratelimit:{IP}:{window}"
    │
    ├─ Increment counter
    │
    ├─ Check limit
    │  ├─ If counter > limit: return 429
    │  └─ Else: allow request
    │
    └─ Set expiration (e.g., 60 seconds)

Configuration:

spring:
  cloud:
    gateway:
      default-filters:
        - name: RequestRateLimiter
          args:
            redis-rate-limiter:
              replenishRate: 2       # 2 requests/sec
              burstCapacity: 4       # Up to 4 in burst
```

### Connection Details

```
Redis Configuration:

spring:
  redis:
    host: ${REDIS_HOST}
    port: ${REDIS_PORT}
    timeout: 2000ms
    jedis:
      pool:
        max-active: 20
        max-idle: 10
        min-idle: 5

EC2 Instance:
├─ IP: 10.0.1.87
├─ Port: 6379
└─ Status: Running

Connection method:

ECS tasks → Service Connect DNS / Direct IP
    │
    v
EC2 Redis
    │
    v
Data cached
```

---

## Database Architecture

### RDS MySQL Setup

```
Primary Database:

Name: citicore-mysql-primary
Endpoint: citicore-mysql-primary.cnk8ckkm2hsk.ap-south-1.rds.amazonaws.com
Port: 3306
Engine: MySQL 8.0
Availability: Multi-AZ (automatic failover)

Replica Database:

Name: citicore-mysql-replica
Endpoint: citicore-mysql-replica.cnk8ckkm2hsk.ap-south-1.rds.amazonaws.com
Port: 3306
Engine: MySQL 8.0 (read-only)
Purpose: Read scaling

Replication:

Primary (writes)
    │
    │ Synchronous replication
    │
    v
Replica (read-only)

Failover:

If Primary fails:
├─ AWS automatic failover
├─ Replica promoted to Primary
├─ New Replica created
└─ Downtime: < 2 minutes

Backup:

Automated:
├─ Daily snapshots
├─ Retention: 7 days
├─ Backups stored in S3
└─ Point-in-time recovery possible
```

### Database Schema

```
Account Service Database:

USE citicore_accountdb;

CREATE TABLE accounts (
    id UUID PRIMARY KEY,
    user_id UUID NOT NULL,
    account_type VARCHAR(20),  -- SAVINGS, CURRENT
    account_number VARCHAR(20) UNIQUE,
    balance DECIMAL(15, 2),
    status VARCHAR(20),        -- ACTIVE, INACTIVE
    created_at TIMESTAMP,
    updated_at TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id)
);

CREATE TABLE transactions (
    id UUID PRIMARY KEY,
    account_id UUID,
    type VARCHAR(20),          -- DEPOSIT, WITHDRAWAL
    amount DECIMAL(15, 2),
    balance_after DECIMAL(15, 2),
    description VARCHAR(255),
    created_at TIMESTAMP
);

CREATE TABLE outbox_events (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    transaction_id UUID,
    event_type VARCHAR(50),
    payload JSON,
    status VARCHAR(20),        -- PENDING, SENT, FAILED
    created_at TIMESTAMP,
    published_at TIMESTAMP
);

Indexing:

CREATE INDEX idx_account_user ON accounts(user_id);
CREATE INDEX idx_account_number ON accounts(account_number);
CREATE INDEX idx_transaction_account ON transactions(account_id);
CREATE INDEX idx_outbox_status ON outbox_events(status);
CREATE INDEX idx_outbox_created ON outbox_events(created_at);
```

### Query Routing

```
Read vs Write routing:

@Repository
public interface AccountRepository extends JpaRepository<Account, UUID> {
    
    @Query("SELECT a FROM Account a WHERE a.id = ?1")
    @Transactional(readOnly = true)
    Optional<Account> findByIdReadOnly(UUID id);
    
    @Query("SELECT a FROM Account a WHERE a.id = ?1")
    Optional<Account> findById(UUID id);  // Uses Primary for consistency
}

Read operations:
├─ Query Replica (read-only, faster)
├─ Check Redis cache first
├─ If miss, query Replica
└─ Update cache

Write operations:
├─ Always query Primary
├─ Ensures immediate consistency
├─ Invalidate cache
└─ Cache refreshes on next read
```

---

## AWS Secrets Manager and TLS

### Secret Storage

```
Purpose:

Store sensitive credentials:
├─ Database passwords
├─ JWT signing secrets
├─ API keys
├─ OAuth tokens
└─ Certificates

Never in Git:
├─ ❌ Passwords
├─ ❌ API keys
├─ ❌ Private keys
├─ ❌ OAuth secrets
└─ ❌ Any sensitive data

In AWS Secrets Manager:
├─ ✅ Encrypted storage
├─ ✅ Access control via IAM
├─ ✅ Audit logging
├─ ✅ Rotation support
└─ ✅ Secure retrieval
```

### ECS Secret Injection

```
Task Definition Configuration:

"containerDefinitions": [
  {
    "name": "account-service",
    "environment": [
      {
        "name": "ACCOUNT_DB_HOST",
        "value": "citicore-mysql-primary.cnk8ckkm2hsk.ap-south-1.rds.amazonaws.com"
      }
    ],
    "secrets": [
      {
        "name": "DB_PASSWORD",
        "valueFrom": "arn:aws:secretsmanager:ap-south-1:580655778303:secret:citicore/auth-service/database-tXq98R"
      },
      {
        "name": "JWT_SECRET",
        "valueFrom": "arn:aws:secretsmanager:ap-south-1:580655778303:secret:citicore/auth-service/jwt-q8jqXN"
      }
    ]
  }
]

How it works:

1. Task starts
2. ECS retrieves secrets from Secrets Manager
3. ECS injects as environment variables
4. Application reads from env
5. Never visible in task logs
6. Never stored in images

Execution role permissions:

{
  "Effect": "Allow",
  "Action": "secretsmanager:GetSecretValue",
  "Resource": "arn:aws:secretsmanager:ap-south-1:580655778303:secret:citicore/*"
}
```

### TLS for RDS Connections

```
RDS requires TLS for connections (best practice):

Connection String:

jdbc:mysql://${RDS_ENDPOINT}:3306/database?useSSL=true&requireSSL=true

Truststore:

AWS provides RDS CA certificate
Download: https://truststore.pki.rds.amazonaws.com/global/ca_bundle.pem

Hibernate Configuration:

spring:
  datasource:
    hikari:
      maximum-pool-size: 10
      minimum-idle: 2

JVM Truststore Configuration:

In Dockerfile:

FROM eclipse-temurin:17-jre
...
COPY rds-truststore.jks /app/rds/rds-truststore.jks
RUN chmod 644 /app/rds/rds-truststore.jks

ENTRYPOINT ["java", \
  "-Djavax.net.ssl.trustStore=/app/rds/rds-truststore.jks", \
  "-Djavax.net.ssl.trustStorePassword=changeit", \
  "-jar", "app.jar"]
```

---

## Jenkins Architecture

### Controller

```
Name: citicore-jenkins-controller
OS: Ubuntu 20.04
Java: 21
Architecture: x86_64

Public IP: 65.1.84.160
Private IP: 10.0.1.96

Installation:

apt-get install jenkins

Runs as:
├─ Service: systemctl start jenkins
├─ Port: 8080 (default)
├─ User: jenkins
├─ Home: /var/lib/jenkins
└─ Shell: /usr/sbin/nologin

Initial Setup:

1. Start Jenkins
2. Get initial admin password:
   cat /var/lib/jenkins/secrets/initialAdminPassword
3. Open http://65.1.84.160:8080
4. Enter password
5. Install suggested plugins
6. Create admin user
7. Configure Jenkins URL
8. Ready for use

Key Files:

/var/lib/jenkins/config.xml
├─ Main configuration

/var/lib/jenkins/jobs/
├─ Job definitions

/var/lib/jenkins/.ssh/
├─ SSH keys for agents

/var/lib/jenkins/nodes/
├─ Agent node configurations
```

### Agent

```
Name: citicore-jenkins-agent
EC2 Instance: i-09e0d69463aa3c281
Instance Type: t3.small (2 vCPU, 2 GB RAM)
OS: Ubuntu 20.04
Public IP: 43.204.230.38
Private IP: 10.0.2.237

SSH Configuration:

Controller Private Key:
/var/lib/jenkins/.ssh/id_ed25519

Agent Authorized Keys:
/home/ubuntu/.ssh/authorized_keys

Jenkins Node Configuration:

Name: citicore-jenkins-agent
Label: backend
Remote Root: /home/ubuntu/jenkins
Executors: 1
Launch Method: SSH
Host: 10.0.2.237 (private IP)
Credentials: Jenkins SSH key

Installed Tools:

Java 17:
java -version
openjdk version "17.0.20"
JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64

Maven 3.9.12:
mvn --version
Apache Maven 3.9.12

Git 2.53.0:
git --version
git version 2.53.0

Docker 29.1.3:
docker --version
Docker version 29.1.3

AWS CLI 2.31.35:
aws --version
aws-cli/2.31.35

IAM Role:

citicore-jenkins-agent-role
├─ ECR permissions
├─ ECS permissions
├─ IAM PassRole
└─ ALB permissions

Verify IAM:

aws sts get-caller-identity
```

### SSH Agent Connection

```
How Jenkins connects to Agent:

1. Controller initiates SSH connection
   ssh -i /var/lib/jenkins/.ssh/id_ed25519 \
       ubuntu@10.0.2.237

2. Authentication
   ├─ Private key: id_ed25519 (controller)
   ├─ Public key: authorized_keys (agent)
   └─ Connection established

3. Remote execution
   ├─ Controller sends commands
   ├─ Agent executes
   └─ Agent returns output

4. Build steps run on agent
   ├─ git clone
   ├─ mvn build
   ├─ docker build
   └─ aws ecs update-service
```

---

## CI/CD Pipeline Overview

### Pipeline Stages

```
Stage 1: Checkout
├─ Clone Git repository
├─ Extract Git SHA
└─ Display commit info

Stage 2: Validate Rollback
├─ If rollback mode: validate parameters
├─ Check ROLLBACK_SERVICE selected
├─ Check ROLLBACK_SHA is valid (40 hex chars)
└─ Skip if normal deployment

Stage 3: Detect Changed Services
├─ Skip if rollback mode
├─ Get all changed files from commits
├─ Map files to services
├─ Handle shared dependencies (kafka-events)
└─ Result: CHANGED_SERVICES list

Stage 4: Build Services
├─ Skip if rollback or no changes
├─ For each service: mvn clean package
├─ Maven builds JAR file
└─ Result: JAR files in target/

Stage 5: Build Docker Images
├─ Skip if rollback or no changes
├─ For each service: docker build
├─ Create image with Git SHA tag
└─ Result: Docker images locally

Stage 6: Login to ECR
├─ Skip if rollback or no changes
├─ aws ecr get-login-password
├─ docker login to ECR registry
└─ Prepare to push images

Stage 7: Push Images to ECR
├─ Skip if rollback or no changes
├─ docker push image:GIT_SHA
├─ Upload to ECR registry
└─ Images available for deployment

Stage 8: Deploy to ECS
├─ For each service (including rollback):
│  ├─ Get current task definition
│  ├─ Update image to new/old SHA
│  ├─ Register new task definition revision
│  ├─ Update ECS service
│  └─ Verify deployment accepted
└─ Result: Services deployed

Stage 9: Record Deployment
├─ Update deployment history file
├─ deployment-history/{service}
├─ Track deployed SHAs
└─ Keep recent 20 deployments

Pipeline Parameters:

ROLLBACK_SERVICE:
├─ Default: NONE
├─ Options: account, transaction, user, auth, notification, gateway, config, eureka

ROLLBACK_SHA:
├─ Default: empty
├─ Input: full 40-character Git SHA
└─ Only needed if rollback selected
```

---

## Git SHA Deployment Strategy

### Why Git SHA

```
Alternatives considered:

❌ 'latest' tag:
├─ Multiple commits have 'latest'
├─ Can't trace which commit deployed
├─ Rollback ambiguous
├─ Overwritten continuously
└─ Not reproducible

❌ Build number:
├─ Jenkins build #123
├─ Not tied to source code
├─ CI/CD specific
├─ Hard to correlate with code
└─ Breaks if CI/CD changes

❌ Timestamp:
├─ 2024-01-15-14-30-27
├─ Close, but human-unfriendly
├─ Still doesn't tie to Git
└─ Not deterministic

✅ Git commit SHA (40-character hex):
├─ Exact source code version
├─ Immutable identifier
├─ Reproducible
├─ Version control integrated
└─ Perfect for tracking
```

### Image Tagging Pattern

```
ECR Image Name Format:

{ECR_REGISTRY}/citicore/{service}:{GIT_SHA}

Example:

580655778303.dkr.ecr.ap-south-1.amazonaws.com/citicore/account-service:434a66eea11f8af80f8998a986cbabe56713f0e9

Same Git SHA across all services:

When multiple services change, all get tagged with same SHA
├─ Commit SHA: 434a66eea11f8af80f8998a986cbabe56713f0e9
├─ Auth Service image: auth-service:434a66ea...
├─ Account Service image: account-service:434a66ea...
└─ User Service image: user-service:434a66ea...

Result:

All services deployed from same commit
├─ Guaranteed compatibility
├─ Easier rollback
├─ Clear deployment unit
└─ Deterministic versioning
```

### Rollback Reference

```
To rollback Account Service:

Known-good deployment history:

deployment-history/account:
434a66eea11f8af80f8998a986cbabe56713f0e9  (current)
25951ba1cee3475323f3f4d7a2d873b0137b2245  (previous, known good)
12345678901234567890123456789012345678901  (older)
...

Rollback decision:

Operator identifies issue with current version
Operator checks deployment history
Operator selects: 25951ba1cee3475323f3f4d7a2d873b0137b2245
Operator verifies image exists in ECR
Operator runs Jenkins with parameters:
  ROLLBACK_SERVICE=account
  ROLLBACK_SHA=25951ba1cee3475323f3f4d7a2d873b0137b2245

Jenkins:
├─ Validates SHA
├─ Checks ECR image
├─ Deploys old image
├─ Verifies ECS accepted
└─ Done
```

---

## ECS Deployment Mechanism

### Step-by-Step Deployment

```
Step 1: Current Task Definition

aws ecs describe-services \
  --cluster citicore-cluster \
  --services citicore-account-service \
  --query 'services[0].taskDefinition' \
  --output text

Result: citicore-account-service:23
```

### Step 2: Download Definition

```
aws ecs describe-task-definition \
  --task-definition "citicore-account-service:23" \
  --query 'taskDefinition' \
  --output json > task-definition.json

task-definition.json contains:
{
  "family": "citicore-account-service",
  "revision": 23,
  "containerDefinitions": [
    {
      "name": "citicore-account-service",
      "image": "account-service:25951ba1...",
      "portMappings": [...],
      "environment": [...],
      "secrets": [...]
    }
  ],
  "taskRoleArn": "...",
  "executionRoleArn": "...",
  ... other fields
}
```

### Step 3: Update Image

```
Python script updates the image field:

Old image: account-service:25951ba1...
New image: account-service:434a66ea...

container["image"] = new_image
```

### Step 4: Remove Response-Only Fields

```
These fields come from describe, can't send back:

- taskDefinitionArn
- revision
- status
- requiresAttributes
- compatibilities
- registeredAt
- registeredBy

Remove with:
data.pop("taskDefinitionArn", None)
... etc
```

### Step 5: Register New Revision

```
aws ecs register-task-definition \
  --cli-input-json file://task-definition.json \
  --region ap-south-1 \
  --query 'taskDefinition.taskDefinitionArn' \
  --output text

Result: arn:aws:ecs:ap-south-1:580655778303:task-definition/citicore-account-service:24

ECS increments revision:
23 → 24 (new task definition version)
```

### Step 6: Update ECS Service

```
aws ecs update-service \
  --cluster citicore-cluster \
  --service citicore-account-service \
  --task-definition "arn:aws:ecs:ap-south-1:580655778303:task-definition/citicore-account-service:24" \
  --region ap-south-1

ECS response: Deployment started

ECS does:
├─ Starts new task with revision 24
├─ Waits for health check
├─ Registers with load balancer
├─ Gradual traffic drain from old task
├─ Stops old task (revision 23)
└─ Deployment complete
```

### Step 7: Verify Deployment

```
sleep 10

aws ecs describe-services \
  --cluster citicore-cluster \
  --services citicore-account-service \
  --query 'services[0].taskDefinition' \
  --output text

Expected: arn contains :24 (new revision)

If actual == expected:
├─ ECS accepted deployment
├─ Jenkins success
├─ ECS continues rolling update
└─ Done (no need to wait for full stabilization)
```

---

## Manual Rollback Strategy

### Why Manual, Not Automatic

```
Automatic rollback problems:

Scenario:

New version deployed → 3% errors → Automatic rollback triggers

Issues:
├─ No root cause analysis
├─ Might not solve problem
├─ Could mask real issue
├─ Ping-pong: new version → old version → new version
├─ Creates confusion
└─ No learning

Manual rollback benefits:

├─ Operator analyzes issue
├─ Conscious decision made
├─ Team learns from incident
├─ Decision documented
├─ Clear intent
└─ Reduces mindless rollbacks
```

### Rollback Decision Process

```
Timeline:

T+0: New version deployed
T+5: Alerts fire, 3% error rate
T+10: On-call engineer notified
T+15: Engineer investigates logs
T+20: Root cause identified
  
Decision point:

Option 1: Fix and redeploy
├─ Issue is in code
├─ Quick fix identified
├─ Redeploy with fix
└─ Deploy new version

Option 2: Rollback to previous version
├─ Issue is significant
├─ Fix not quick
├─ Need to recover service
├─ Previous version known good
└─ Rollback decision

Option 3: Partial rollback
├─ Run both versions
├─ Route small % to new version
├─ Monitor
└─ Gradually shift traffic (canary deploy)
```

### Jenkins Rollback Parameters

```
Build with Parameters:

Parameter 1: ROLLBACK_SERVICE
├─ Type: Choice
├─ Options:
│  ├─ NONE (default, normal deployment)
│  ├─ account
│  ├─ transaction
│  ├─ user
│  ├─ auth
│  ├─ notification
│  ├─ gateway
│  ├─ config
│  └─ eureka

Parameter 2: ROLLBACK_SHA
├─ Type: String
├─ Default: empty
├─ Input: Full 40-character Git SHA
└─ Only used if ROLLBACK_SERVICE != NONE

Validation:

if (params.ROLLBACK_SERVICE != 'NONE') {
    if (!params.ROLLBACK_SHA?.trim()) {
        error "ROLLBACK_SHA required"
    }
    if (!(params.ROLLBACK_SHA ==~ /^[0-9a-fA-F]{40}$/)) {
        error "Invalid SHA format (must be 40 hex characters)"
    }
}
```

### Rollback Execution

```
Rollback skips these stages:

❌ Build Services (use old JAR)
❌ Build Docker Images (use old image)
❌ Login to ECR (already logged in)
❌ Push Images to ECR (image already pushed)

Rollback does these stages:

✅ Deploy to ECS (deploy old image)
✅ Record Deployment (update history)

Process:

1. Verify image exists in ECR
   └─ aws ecr describe-images --image-ids imageTag=ROLLBACK_SHA

2. Get current task definition
   └─ aws ecs describe-services

3. Update image to old SHA
   └─ container["image"] = old_image

4. Register new task definition
   └─ aws ecs register-task-definition

5. Update ECS service
   └─ aws ecs update-service

6. Verify ECS accepted
   └─ Check task definition updated

7. Update deployment history
   └─ deployment-history/{service}

Result: Old version running
```

---

## Health Checks and Verification

### ALB Health Checks

```
Account Service Target Group:

ARN: arn:aws:elasticloadbalancing:ap-south-1:580655778303:targetgroup/citicore-account-tg/801171218dce3f52

Health Check Configuration:

Protocol: HTTP
Port: traffic port (8083)
Path: /actuator/health
Interval: 10 seconds
Timeout: 5 seconds
Healthy threshold: 2
Unhealthy threshold: 3
Matcher: 200

Health check flow:

Every 10 seconds:
├─ ALB → ECS task :8083/actuator/health
├─ Expects: 200 OK with {"status": "UP"}
├─ 2 consecutive successes → Healthy
├─ 3 consecutive failures → Unhealthy

Health responses:

Spring Boot /actuator/health returns:

{
  "status": "UP",
  "components": {
    "db": {
      "status": "UP"
    },
    "redis": {
      "status": "UP"
    },
    "kafka": {
      "status": "UP"
    }
  }
}

Or if component down:

{
  "status": "DOWN",
  "components": {
    "db": {
      "status": "DOWN",
      "reason": "Connection refused"
    }
  }
}

ALB behavior:

Healthy:
├─ 200 OK from /actuator/health
├─ Receives traffic
└─ Shown as "Healthy" in console

Unhealthy:
├─ Error response or timeout
├─ No traffic routed
├─ Shown as "Unhealthy" in console

Draining:
├─ Service marked for update
├─ Existing connections continue
├─ New connections rejected
├─ Task prepares to stop
```

### ECS Health Check Grace Period

```
When task starts:

T+0: ECS task created
T+10: First ALB health check (might fail)
T+20: Spring Boot still starting
T+30: Spring Boot startup complete
T+35: Health checks passing
T+40: ALB marks Healthy
T+50: Receives traffic

Problem:

If health checks start at T+0
Spring Boot not ready until T+30
First 30 seconds of checks fail
ALB marks Unhealthy
Traffic doesn't route
Task can't start properly

Solution:

ECS Health Check Grace Period: 300 seconds
├─ Wait 300s before health checks matter
├─ Gives Spring Boot time to start
├─ Application ready before ALB checks
└─ No premature failures
```

---

## End-to-End Request Flows

### Account Creation Flow

```
User Request:

POST /api/v1/accounts/create
{
  "accountType": "SAVINGS",
  "initialDeposit": 1000
}

Flow:

1. External Client
   ├─ POST http://citicore-gateway-alb:8080/accounts/create

2. Gateway ALB
   ├─ Routes to API Gateway :8080

3. API Gateway
   ├─ Path matches /accounts/**
   ├─ Extracts JWT from header
   ├─ Validates JWT
   ├─ Rate limit check (Redis)
   ├─ Rewrites path to /api/v1/accounts/create
   ├─ Applies circuit breaker
   ├─ Applies retry logic

4. Account Service
   ├─ Receives request
   ├─ Extracts user context from JWT
   ├─ Validates user
   ├─ Creates account record
   ├─ Writes to RDS Primary
   ├─ Publishes account-created event to Kafka
   ├─ Returns account details

5. Account Service
   ├─ Event published to Kafka

6. Notification Service
   ├─ Consumes account-created event
   ├─ Sends welcome email

7. Response back through ALB
   ├─ API Gateway receives response
   ├─ Returns to client
   ├─ Response: 201 Created + account details
```

### Transaction Flow

```
User Request:

POST /api/v1/transactions/transfer
{
  "senderAccount": "ACC001",
  "receiverAccount": "ACC002",
  "amount": 5000,
  "description": "Payment"
}

Flow:

1. Client
   └─ POST http://citicore-gateway-alb:8080/transaction/transfer

2. Gateway ALB → API Gateway
   └─ Routes to Transaction Service

3. Transaction Service
   ├─ Validates JWT
   ├─ Checks daily limits
   ├─ Calls Account Service (OpenFeign)
   │  └─ /api/v1/accounts/validate-transfer
   │  └─ Verifies sender has balance
   │
   ├─ Saves transaction (PENDING)
   ├─ Writes to outbox table (DEBIT event, PENDING)
   ├─ Commits database transaction
   └─ Returns: transactionId, status=INITIATED

4. OutboxPublisher (polls every 5s)
   ├─ Finds PENDING events
   ├─ Publishes DEBIT event to Kafka
   ├─ Waits for Kafka acknowledgement
   ├─ Updates outbox: status=SENT
   └─ Commits

5. Account Service (consumes Kafka)
   ├─ Receives DEBIT event
   ├─ Debits sender account
   ├─ Publishes debit-success to Kafka
   │
   ├─ Also publishes CREDIT event (from outbox)
   ├─ Credits receiver account
   ├─ Publishes credit-success to Kafka

6. Transaction Service (result consumer)
   ├─ Receives debit-success
   ├─ Updates transaction: DEBIT_SUCCESS
   ├─ Receives credit-success
   ├─ Updates transaction: COMPLETED
   └─ Done

7. Response to Client
   ├─ Transfer successful
   ├─ Money transferred
   └─ Confirmation sent

Failure scenario:

If credit fails:
├─ Account Service publishes credit-failed
├─ Transaction Service receives it
├─ Updates transaction: FAILED
├─ Writes REVERSAL to outbox
├─ OutboxPublisher publishes reversal
├─ Account Service reverses debit
├─ Publishes reversal-success
├─ Transaction updated: REVERSED
└─ Money returned to sender
```

### Authentication Flow

```
Login Request:

POST /api/v1/auth/login
{
  "email": "user@example.com",
  "password": "secret123"
}

Flow:

1. Client
   └─ POST /auth/login

2. Auth Service
   ├─ Receives credentials
   ├─ Queries RDS: SELECT user WHERE email = ?
   ├─ Compares password hash
   ├─ If valid:
   │  ├─ Creates JWT token
   │  │  ├─ Header: {alg: HS256, typ: JWT}
   │  │  ├─ Payload: {userId, email, roles, iat, exp}
   │  │  └─ Signature: HMAC(header+payload, secret)
   │  ├─ Returns token
   └─ If invalid: 401 Unauthorized

3. Client
   └─ Receives JWT token
   └─ Stores locally

4. Subsequent Requests
   ├─ Client includes JWT
   │  └─ Authorization: Bearer {token}
   │
   ├─ API Gateway validates JWT
   │  ├─ Extracts signature
   │  ├─ Verifies with secret
   │  └─ If valid: extract user info
   │
   ├─ Forward to backend service
   │  └─ Include user context
   │
   ├─ Backend service
   │  ├─ Has user information
   │  ├─ Can authorize request
   │  └─ Processes in user context

JWT Flow:

Login → JWT issued → Token stored
    ↓
Request → JWT in header
    ↓
Gateway validates JWT
    ↓
Backend service receives user context
    ↓
Authorization checks
    ↓
Request processed
```

---

## Deployment Procedures

### Complete Deployment Checklist

```
Pre-deployment:

[ ] Code reviewed and approved
[ ] Tests passed locally
[ ] No uncommitted changes
[ ] Commit message clear
[ ] Feature branch merged to master
[ ] Ready to push

Deployment:

[ ] git push origin master
[ ] Wait for Jenkins webhook (15-30 seconds)
[ ] Monitor Jenkins console
[ ] Stages complete:
    [ ] Checkout
    [ ] Detect Changed Services
    [ ] Build (should show your service)
    [ ] Docker Build
    [ ] ECR Push
    [ ] Deploy to ECS

Post-deployment:

[ ] Jenkins shows SUCCESS
[ ] Check ECS service:
    aws ecs describe-services --cluster citicore-cluster --services citicore-account-service
[ ] Task count: 1 running
[ ] Deployment status: COMPLETED
[ ] Check ALB target health:
    aws elbv2 describe-target-health --target-group-arn <ARN>
[ ] Targets show: Healthy
[ ] Test API endpoints
[ ] Check CloudWatch logs
[ ] Verify monitoring alerts normal
[ ] All checks passed → Deployment successful
```

### Deployment Example: Account Service

```
Change:

File: account-service/src/main/java/AccountController.java
Change: Add new endpoint for account inquiry

Step 1: Git

git add account-service/
git commit -m "Add account inquiry endpoint"
git push origin master

Step 2: Jenkins Triggered

GitHub sends webhook to Jenkins
Jenkins detects: account-service/ changed

Step 3: Build

mvn -pl account-service -am clean package -DskipTests
Result: account-service-0.0.1-SNAPSHOT.jar

Step 4: Docker

docker build -t account-service:434a66ea... .
Result: Docker image locally

Step 5: ECR

aws ecr get-login-password | docker login
docker push account-service:434a66ea...
Result: Image in ECR

Step 6: ECS

Get current task definition
Update image tag
Register new revision (23 → 24)
Update service
Verify service accepted revision 24

Step 7: Done

ECS rolling update starts
Old task gradually drained
New task takes traffic
Deployment complete (2-3 minutes)

Result:

New code in production
Requests routed to new version
Old version stopped
Deployment history updated
Can rollback if needed
```

---

## Rollback Procedures

### Complete Rollback

```
Scenario:

Account Service deployed at 2:00 PM
At 2:15 PM, errors spike to 5%
On-call engineer investigates
Root cause: New query too slow
Decision: Rollback to previous version

Rollback Steps:

Step 1: Identify Previous Version

Check deployment history:
tail -5 deployment-history/account

Output:
434a66eea11f8af80f8998a986cbabe56713f0e9  (current - problematic)
25951ba1cee3475323f3f4d7a2d873b0137b2245  (previous - known good)
12345678901234567890123456789012345678901
...

Step 2: Verify Image in ECR

aws ecr describe-images \
  --repository-name citicore/account-service \
  --image-ids imageTag=25951ba1cee3475323f3f4d7a2d873b0137b2245 \
  --region ap-south-1

Result: Image found in ECR
Digest: sha256:5ed050cb034d42bc5d42ce793d5e937232cc00bd1d8bcf70c9bea2099767d8a1

Step 3: Open Jenkins

1. Go to Jenkins: http://65.1.84.160:8080
2. Select: CitiCore Pipeline
3. Click: Build with Parameters
4. Set: ROLLBACK_SERVICE = account
5. Set: ROLLBACK_SHA = 25951ba1cee3475323f3f4d7a2d873b0137b2245
6. Click: Build

Step 4: Monitor Rollback

Jenkins stages:
├─ Checkout (Git SHA now 25951ba1...)
├─ Validate Rollback
│  └─ SHA format validated
├─ Deploy to ECS
│  ├─ Get current task def
│  ├─ Update image: account-service:25951ba1...
│  ├─ Register new revision (24 → 25)
│  ├─ Update service
│  └─ Verify task def 25 active
├─ Record Deployment
└─ SUCCESS

Step 5: Verify Rollback

aws ecs describe-services \
  --cluster citicore-cluster \
  --services citicore-account-service \
  --query 'services[0].deployments[].{Status:status,RunningCount:runningCount,TaskDefinition:taskDefinition}'

Output:
Status: ACTIVE
RunningCount: 1
TaskDefinition: arn:aws:ecs:.../citicore-account-service:25

Step 6: Test API

curl -i http://citicore-account-alb.../accounts/balance/ACC001
Expected: 200 OK (old behavior)

Step 7: Monitor

Watch CloudWatch logs
Watch metrics
Watch alerts
Verify error rate decreases

Result:

Old version running
Problem resolved
Deployment history shows rollback
Team can investigate fix
Deploy new version when ready
```

---

## Verification Commands

### AWS Commands

```
Verify AWS Identity:

aws sts get-caller-identity

Output:
{
  "UserId": "AIDACKCEVSQ6C2EXAMPLE:i-09e0d69463aa3c281",
  "Account": "580655778303",
  "Arn": "arn:aws:iam::580655778303:role/citicore-jenkins-agent-role"
}

List ECS Services:

aws ecs list-services \
  --cluster citicore-cluster \
  --region ap-south-1

Output:
citicore-config-server-service-rwdtvpj9
citicore-eureka-server
citicore-auth-service
citicore-user-service
citicore-account-service
citicore-transaction-service
citicore-notification-service-service-0nnwrup9
citicore-apigateway-service

ECS Deployment Summary:

aws ecs describe-services \
  --cluster citicore-cluster \
  --services citicore-account-service \
  --region ap-south-1 \
  --query 'services[0].deployments[].{Status:status,State:rolloutState,TaskDefinition:taskDefinition,Desired:desiredCount,Running:runningCount,Pending:pendingCount}' \
  --output table

Output:
Status    State       TaskDefinition                                   Desired  Running  Pending
─────────────────────────────────────────────────────────────────────────────────────────────
PRIMARY   COMPLETED   citicore-account-service:24                      1        1        0

Expected state when healthy:
Status: PRIMARY
State: COMPLETED
Desired: 1
Running: 1
Pending: 0

Describe Task:

aws ecs describe-tasks \
  --cluster citicore-cluster \
  --tasks <TASK_ARN> \
  --region ap-south-1

Output:
taskArn: arn:aws:ecs:.../citicore-account-service/...
lastStatus: RUNNING
desiredStatus: RUNNING
cpu: 512
memory: 1024
taskDefinitionArn: arn:aws:ecs:.../citicore-account-service:24

List ECR Repositories:

aws ecr describe-repositories \
  --region ap-south-1

Output:
citicore/account-service
citicore/transaction-service
citicore/user-service
...

ECR Images:

aws ecr describe-images \
  --repository-name citicore/account-service \
  --region ap-south-1

Output:
imageUri: account-service:434a66ea...
imageSizeBytes: 234567890
imagePushedAt: 2024-01-15T14:30:27+00:00

Target Group Health:

aws elbv2 describe-target-health \
  --target-group-arn arn:aws:elasticloadbalancing:ap-south-1:580655778303:targetgroup/citicore-account-tg/801171218dce3f52 \
  --region ap-south-1

Output:
Target                                    Port  State      Reason
────────────────────────────────────────────────────────────────────
10.0.11.45                               8083  healthy    Health checks passed

Expected: State = healthy
```

### Docker Commands

```
List Images:

docker images

Output:
REPOSITORY                                                 TAG                CREATED
account-service                                            434a66ea...        5 minutes ago
transaction-service                                        434a66ea...        8 minutes ago

List Containers:

docker ps

Output:
CONTAINER ID  IMAGE          STATUS
abc123        postgres       Up 2 hours

Disk Usage:

docker system df

Output:
TYPE              TOTAL     ACTIVE    RECLAIMABLE
Images            45GB      3GB       42GB
Containers        2GB       100MB     1.9GB
```

### Local Testing

```
Test endpoint locally:

curl -i http://localhost:8083/actuator/health

Output:
HTTP/1.1 200 OK
{"status":"UP"}

Test via ALB:

curl -i http://citicore-account-alb.../accounts/balance/ACC001

Output:
HTTP/1.1 200 OK
{"accountNumber":"ACC001","balance":10000}

Authentication test:

curl -X POST http://localhost:8081/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"user@example.com","password":"password"}'

Output:
{
  "token": "eyJhbGc...",
  "expiresIn": 3600
}
```

---

## Troubleshooting Guide

### ECS Task Not Starting

```
Symptoms:
├─ Task stuck in PROVISIONING
├─ Status doesn't change to RUNNING
└─ No visible error

Diagnosis:

1. Check task logs in CloudWatch
   /ecs/citicore-account-service

2. Common causes:

A. Image not found in ECR
   Error: "Unable to pull image"
   Solution: Verify ECR login, image exists

B. Insufficient resources
   Error: "Not enough resources"
   Solution: Check CPU/memory allocation

C. Secrets Manager failure
   Error: "ResourceInitializationError"
   Solution: Check execution role permissions

D. Security group blocks
   Error: Task initializes but can't connect
   Solution: Check security groups allow needed ports
```

### ALB Target Unhealthy

```
Symptoms:
├─ Target Group shows "Unhealthy"
├─ No traffic routed
└─ API requests timeout

Diagnosis:

1. Check target details:
   aws elbv2 describe-target-health --target-group-arn <ARN>

2. Common reasons:

A. Application not listening
   Reason: Request timed out
   Solution: Check if app is running, listening on port

B. Health check path returns error
   Reason: Health checks failed
   Solution: Check /actuator/health manually
   curl http://<task-ip>:8083/actuator/health

C. Security group blocks ALB
   Error: Request timed out (no response)
   Solution: Add ALB security group as source for app port

D. Wrong port in target group
   Error: Connection refused
   Solution: Verify target group port matches app port (8083)

E. Grace period expired before app started
   Reason: Health check failed too soon
   Solution: Increase ECS health check grace period
```

### Service Can't Reach Another Service

```
Symptoms:
├─ OpenFeign call times out
├─ Transaction Service can't reach Account Service
└─ Error: "Connection timed out"

Diagnosis:

1. Test connectivity:
   ECS Exec into Transaction Service task:
   aws ecs execute-command \
     --cluster citicore-cluster \
     --task <TASK_ID> \
     --container citicore-transaction-service \
     --interactive \
     --command '/bin/sh'

2. From container shell:
   curl -i http://citicore-account-service-8083-tcp.citicore:8083/actuator/health

3. Common issues:

A. Service Connect DNS not resolving
   Error: "Name or service not known"
   Solution: Check Service Connect namespace configured
   Try: getent hosts citicore-account-service-8083-tcp.citicore

B. Security group doesn't allow traffic
   Error: Connection times out (hangs)
   Solution: Add ECS SG as source for destination port

C. Wrong hostname
   Error: "Name or service not known"
   Solution: Verify exact hostname spelling
   Right: citicore-account-service-8083-tcp.citicore
   Wrong: account-service-8083-tcp.citicore

D. Target service not running
   Error: "Connection refused"
   Solution: Check ECS service status
   aws ecs describe-services --cluster citicore-cluster --services citicore-account-service
```

### Jenkins Agent Connection Failed

```
Symptoms:
├─ Jenkins console shows "Agent connection failed"
├─ Node marked as offline
└─ Builds can't run

Diagnosis:

1. Check agent status:
   Jenkins UI → Manage Nodes → citicore-jenkins-agent

2. Common issues:

A. SSH key not in authorized_keys
   Error: "Permission denied (publickey)"
   Solution:
   └─ Copy controller public key to agent authorized_keys
   └─ Verify permissions: 600 on authorized_keys

B. Agent network unreachable
   Error: "Connection refused"
   Solution:
   └─ Ping agent: ping 10.0.2.237
   └─ Check security groups
   └─ Verify Jenkins on correct port

C. Wrong private key
   Error: "Permission denied"
   Solution:
   └─ Verify Jenkins configured with correct key
   └─ Verify key matches authorized_keys

D. Agent machine shut down
   Error: "Connection refused"
   Solution:
   └─ Start agent EC2 instance
   └─ Verify on network
```

### Docker Build Out of Memory

```
Symptoms:
├─ Docker build fails
├─ "killed" message
├─ Process exits with code 137

Diagnosis:

1. Check Docker disk space:
   docker system df

2. Solutions:

A. Clean up old images:
   docker rmi <image_id>

B. Prune unused data:
   docker system prune -a

C. Increase Docker memory:
   Edit /etc/docker/daemon.json:
   {
     "memory": "2g",
     "storage-driver": "overlay2"
   }

D. Use smaller base image:
   FROM eclipse-temurin:17-jre-alpine
   (Alpine is ~50MB, regular ~200MB)
```

---

## Production Checklist

### Pre-Production

```
Code Quality:
[ ] Code review completed
[ ] Linting passed
[ ] No obvious bugs
[ ] Follows code style

Testing:
[ ] Unit tests pass
[ ] Integration tests pass
[ ] API tests pass
[ ] Load testing done (if applicable)

Documentation:
[ ] README updated
[ ] API documentation current
[ ] Database schema documented
[ ] Configuration documented

Security:
[ ] No hardcoded credentials
[ ] No secrets in Git
[ ] Input validation in place
[ ] SQL injection prevention checked
[ ] CORS properly configured

Performance:
[ ] Database queries optimized
[ ] No N+1 queries
[ ] Caching strategy defined
[ ] Response times acceptable
```

### Deployment Verification

```
Build:
[ ] Maven build successful
[ ] No compilation errors
[ ] JAR created (correct size)

Docker:
[ ] Docker build successful
[ ] Image runs locally
[ ] Image size reasonable

ECR:
[ ] Image pushed to ECR
[ ] Digest matches locally
[ ] Image tagged with Git SHA

ECS:
[ ] Task definition registered
[ ] ECS service updated
[ ] Deployment started

Health:
[ ] ECS task RUNNING
[ ] /actuator/health returns 200
[ ] ALB target Healthy
[ ] Eureka registration UP

Dependencies:
[ ] Config Server UP
[ ] Eureka UP
[ ] Database connectivity UP
[ ] Kafka connectivity UP
[ ] Redis connectivity UP (if used)

API Testing:
[ ] Basic endpoint test
[ ] Authentication test
[ ] Database query test
[ ] Error handling test

Monitoring:
[ ] CloudWatch logs flowing
[ ] Alarms configured
[ ] Metrics visible
[ ] No error spikes
```

### Post-Deployment

```
Monitoring (next 24 hours):
[ ] Error rate normal
[ ] Response times normal
[ ] CPU usage normal
[ ] Memory usage normal
[ ] Disk usage normal
[ ] No spike in database connections

User Feedback:
[ ] No user complaints
[ ] No support tickets
[ ] Functionality working
[ ] Performance acceptable

Rollback Readiness:
[ ] Know how to rollback
[ ] Previous version tested
[ ] Rollback plan documented
[ ] Team trained on procedure
```

---

## Current Status

### Completed Infrastructure

```
Core Platform:        ✅ COMPLETE
├─ 8 microservices deployed
├─ All services running
├─ Eureka registration working
├─ Config Server active
├─ API Gateway routing
└─ All ports responding

AWS Infrastructure:   ✅ COMPLETE
├─ VPC configured
├─ Security groups defined
├─ ECS cluster running
├─ RDS deployed
├─ Redis available
├─ Kafka running
└─ ALBs configured

Jenkins CI/CD:        ✅ COMPLETE
├─ Controller running
├─ Agent connected
├─ Pipeline functional
├─ Git SHA tagging working
├─ Service detection working
├─ Manual rollback functional
└─ All services deployable

Testing Completed:    ✅ COMPLETE
├─ Deployment testing
├─ Rollback testing
├─ API endpoint testing
├─ Database connectivity
├─ Kafka messaging
└─ Service discovery
```

### Known Limitations

```
Currently Not Implemented:

HTTPS / TLS:
├─ ALBs use HTTP only
├─ No SSL certificates
├─ Future: Add ACM certificates

Auto-scaling:
├─ Fixed task count = 1
├─ No horizontal scaling
├─ Future: Configure ASG

Monitoring:
├─ Basic CloudWatch logs
├─ No Prometheus/Grafana
├─ No dashboards
├─ Future: Add monitoring stack

Private Subnets:
├─ ECS tasks in public subnets
├─ Should be in private subnets
├─ Future: VPC redesign

Secrets Management:
├─ Some via env vars
├─ Should use Secrets Manager
├─ Future: Full migration

High Availability:
├─ Single task per service
├─ No multi-AZ replication
├─ Future: Add redundancy
```

---

## Future Improvements

### Immediate (Next Sprint)

```
HTTPS Implementation:
├─ Request SSL certificate from ACM
├─ Configure HTTPS on ALBs
├─ Redirect HTTP to HTTPS
└─ Test with production domains

Auto-scaling:
├─ Configure CPU/memory metrics
├─ Set scaling thresholds
├─ Test scale-up scenario
└─ Test scale-down scenario

Enhanced Monitoring:
├─ Install Prometheus
├─ Configure Grafana dashboards
├─ Alert thresholds
└─ Log aggregation improvements
```

### Medium Term (Next Month)

```
Private Subnets:
├─ Create private ECS subnets
├─ Configure NAT gateway
├─ Migrate ECS to private
├─ Maintain security

Secrets Management:
├─ Migrate all secrets to Secrets Manager
├─ Remove environment variable secrets
├─ Rotate keys regularly
└─ Audit trail

Multi-AZ Setup:
├─ Configure RDS Multi-AZ
├─ Add replicas in second AZ
├─ Test failover
└─ Update documentation
```

### Long Term (Backlog)

```
Disaster Recovery:
├─ Regular backups
├─ RTO/RPO targets defined
├─ Failover procedures documented
├─ Tested quarterly

Performance Optimization:
├─ Database query optimization
├─ Caching strategy refinement
├─ CDN for static content
├─ Load testing

Cost Optimization:
├─ Reserved instances
├─ Spot instances for non-critical
├─ Auto-scaling down on low usage
├─ Resource tagging for cost allocation
```

---

## Interview-Ready Explanations

### "Describe the CitiCore Banking Platform Architecture"

**Response:**

"CitiCore is a microservices-based banking platform with 8 independently deployable services running on AWS ECS/Fargate.

**Architecture:**

```
External Clients
    ↓
Gateway ALB (8080)
    ↓
API Gateway (Spring Cloud Gateway)
    ├─ Routes to: Auth, User, Account, Transaction services
    ├─ Rate limiting via Redis
    ├─ Circuit breaker for resilience
    ├─ Retry logic for transient failures
    ↓
Internal Services
├─ Auth Service (8081) - JWT & authentication
├─ User Service (8082) - User profiles & KYC
├─ Account Service (8083) - Account operations
└─ Transaction Service (8084) - Transaction processing

Supporting Infrastructure:
├─ Config Server - Centralized configuration
├─ Eureka - Service discovery
├─ Kafka - Event streaming
├─ Redis - Caching & rate limiting
├─ RDS MySQL - Persistent storage
└─ ECS Service Connect - Internal DNS
```

**Key design principles:**

1. **Microservices:** Each service owns its data and API. Independent deployment.

2. **Event-driven:** Saga pattern for distributed transactions. Kafka for async messaging.

3. **Service discovery:** Eureka + ECS Service Connect for finding services.

4. **Centralized config:** Spring Cloud Config keeps configuration out of images.

5. **API Gateway:** Single entry point for routing, rate limiting, and cross-cutting concerns.

6. **Immutable versioning:** Git SHA tags all deployments for reproducibility."

### "How does the CI/CD pipeline work?"

**Response:**

"The CI/CD pipeline automates deployment from Git commit to production ECS in ~5 minutes.

**Flow:**

```
Developer pushes code
    ↓
GitHub webhook triggers Jenkins
    ↓
Jenkins detects changed services
    ├─ account-service/ changed → deploy account
    ├─ transaction-service/ changed → deploy transaction
    └─ kafka-events/ changed → redeploy all consumers
    ↓
Jenkins Agent (Maven, Docker, AWS CLI):
    ├─ Maven: compile & package JAR
    ├─ Docker: build image tagged with Git SHA
    ├─ ECR: push image to registry
    ├─ ECS: register new task definition
    └─ ECS: update service (rolling deployment)
    ↓
Production:
    ├─ New image starts
    ├─ Health checks pass
    ├─ Traffic shifts gradually
    └─ Old task stops
```

**Key features:**

1. **Git SHA tagging:** Every image tagged with commit SHA for traceability.

2. **Smart service detection:** Only rebuild/deploy changed services.

3. **Shared dependencies:** kafka-events change triggers all consumers.

4. **Manual rollback:** Explicit Jenkins parameters to rollback to known-good version.

5. **No long waiters:** Verify task definition accepted, don't wait for full stabilization."

### "How do you handle failures and rollback?"

**Response:**

"Failures are handled at multiple levels with deterministic rollback.

**Failure handling:**

1. **Application level:**
   - Input validation
   - Exception handling
   - Graceful degradation

2. **Service level:**
   - Circuit breakers (fail fast)
   - Retry logic (exponential backoff)
   - Timeout handling

3. **Transaction level:**
   - Saga pattern for distributed transactions
   - Compensating transactions (reversals)
   - Transactional outbox for reliability

4. **Deployment level:**
   - Rolling updates (zero downtime)
   - Health checks prevent bad deployments
   - Immediate rollback capability

**Rollback process:**

```
Issue detected
    ↓
Operator analyzes root cause
    ↓
Decision: Rollback to known-good version
    ↓
Open Jenkins: Build with Parameters
    ↓
Select: ROLLBACK_SERVICE=account
    ↓
Enter: ROLLBACK_SHA=25951ba1cee34753...
    ↓
Jenkins verifies image in ECR
    ↓
Deploy old image to ECS
    ↓
Verify ECS accepted
    ↓
Test API
    ↓
Done: Old version running
```

**Why manual, not automatic:**

Automatic rollback would just bounce between versions without root cause analysis. Manual rollback ensures:
- Operator understands issue
- Conscious decision made
- Team learns from incident
- Audit trail maintained"

---

## Summary

**CitiCore is a complete, production-ready banking microservices platform** combining:

- 8 independent services
- AWS cloud infrastructure
- Fully automated CI/CD with Git SHA versioning
- Event-driven saga pattern for consistency
- Manual rollback strategy for safety
- Complete monitoring and verification

The platform demonstrates modern microservices patterns, cloud-native deployment, and production deployment procedures.

---
