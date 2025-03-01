# Backend - Uber Distributed Ride Simulation

## Overview
This is the backend for the Uber Distributed Ride Simulation project. It is built as a set of Node.js microservices, each responsible for a core domain (drivers, customers, billing, admin, rides), communicating via REST and Kafka.

## Microservices
- Driver Service
- Customer Service
- Billing Service
- Admin Service
- Rides Service

## Features
- RESTful APIs for all entities
- Kafka-based event messaging
- Redis caching for performance
- MySQL for core data, MongoDB for media/reviews
- Robust validation and error handling
- Scalable, stateless architecture

## Tech Stack
- Node.js, Express
- MySQL, MongoDB
- Kafka, Redis
- Docker, Kubernetes-ready

## Setup
```bash
npm install
# Set up .env with DB/Kafka/Redis credentials
npm run dev
```

## Usage
- All services run on different ports (see `.env`)
- API docs in `../docs/API_DESIGN.md`
- Test harness: `node backend/tests/test_harness.js`

## Folder Structure
- `services/` — Microservices
- `models/` — Data models
- `database/` — DB connectors
- `kafka/` — Kafka producer/consumer
- `cache/` — Redis caching
- `utils/` — Validators, constants
- `tests/` — Test harness

## License
MIT 