# System Architecture

## Overview
The Uber Distributed Ride Simulation system is designed as a scalable, distributed 3-tier application with microservices, messaging, caching, and robust data storage.

## Architecture Diagram
```mermaid
graph TD
  A[React Client] -- REST --> B[API Gateway/Express]
  B -- REST --> C1[Driver Service]
  B -- REST --> C2[Customer Service]
  B -- REST --> C3[Billing Service]
  B -- REST --> C4[Admin Service]
  B -- REST --> C5[Rides Service]
  C1 -- Kafka --> D1[Kafka Broker]
  C2 -- Kafka --> D1
  C3 -- Kafka --> D1
  C4 -- Kafka --> D1
  C5 -- Kafka --> D1
  D1 -- Kafka --> C1
  D1 -- Kafka --> C2
  D1 -- Kafka --> C3
  D1 -- Kafka --> C4
  D1 -- Kafka --> C5
  C1 -- SQL/NoSQL --> E1[MySQL]
  C2 -- SQL/NoSQL --> E1
  C3 -- SQL/NoSQL --> E1
  C4 -- SQL/NoSQL --> E1
  C5 -- SQL/NoSQL --> E1
  C1 -- Mongo --> E2[MongoDB]
  C2 -- Mongo --> E2
  C3 -- Mongo --> E2
  C4 -- Mongo --> E2
  C5 -- Mongo --> E2
  C1 -- Redis --> F[Redis Cache]
  C2 -- Redis --> F
  C3 -- Redis --> F
  C4 -- Redis --> F
  C5 -- Redis --> F
```

## Service Decomposition
- **API Gateway**: Routes requests, handles auth, aggregates responses
- **Driver/Customer/Billing/Admin/Rides Services**: Each is a stateless Node.js microservice, communicating via REST and Kafka
- **Kafka**: Event-driven communication, decouples services, enables async processing
- **MySQL**: Stores core entity data (drivers, customers, rides, billing, admin)
- **MongoDB**: Stores media (images, videos), reviews, ride images
- **Redis**: Caches frequent queries (e.g., entity lookups, stats)

## Data Flow
1. Client sends REST request to API Gateway
2. Gateway routes to appropriate service
3. Service processes request, interacts with DB/cache, emits Kafka events
4. Other services consume events as needed (e.g., billing after ride creation)
5. Responses are returned to client

## Deployment
- **Docker Compose**: For local development, all services, DBs, Kafka, Redis run as containers
- **AWS/Kubernetes**: For production, each service is a pod, with managed RDS (MySQL), MSK (Kafka), ElastiCache (Redis), and DocumentDB (MongoDB)
- **CI/CD**: GitHub Actions for build/test/deploy

## Scalability & Reliability
- Stateless services, horizontal scaling
- Kafka for decoupling and resilience
- Redis for performance
- DB sharding/replication as needed

## Notes
- See `docker-compose.yml` for local setup
- See `scripts/performance_test.js` for scalability tests
