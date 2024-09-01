# API Design Document

## Overview
This document describes the REST API endpoints for the Uber Distributed Ride Simulation system, including request/response formats, error codes, and Kafka event structures.

---

## Driver Service
### Endpoints
- `POST /api/drivers` — Create a new driver
- `GET /api/drivers` — List all drivers
- `GET /api/drivers/:id` — Get driver by ID
- `PUT /api/drivers/:id` — Update driver info
- `DELETE /api/drivers/:id` — Delete driver
- `GET /api/drivers/search` — Search drivers by attributes
- `POST /api/drivers/:id/intro` — Upload intro video/image

#### Example: Create Driver
```json
POST /api/drivers
{
  "driverId": "123-45-6789",
  "firstName": "John",
  "lastName": "Doe",
  "address": "123 Main St",
  "city": "Boston",
  "state": "MA",
  "zipCode": "02118",
  "phone": "555-1234",
  "email": "john@example.com",
  "carDetails": "Toyota Prius, 2018"
}
```

#### Response
```json
201 Created
{
  "message": "Driver created successfully",
  "driver": { ... }
}
```

---

## Customer Service
### Endpoints
- `POST /api/customers` — Create a new customer
- `GET /api/customers` — List all customers
- `GET /api/customers/:id` — Get customer by ID
- `PUT /api/customers/:id` — Update customer info
- `DELETE /api/customers/:id` — Delete customer
- `POST /api/customers/:id/review` — Add review
- `POST /api/customers/:id/upload` — Upload ride images

---

## Billing Service
### Endpoints
- `POST /api/billing` — Create bill for ride
- `GET /api/billing/:id` — Get bill by ID
- `GET /api/billing/search` — Search bills
- `DELETE /api/billing/:id` — Delete bill

---

## Admin Service
### Endpoints
- `POST /api/admin/drivers` — Add driver
- `POST /api/admin/customers` — Add customer
- `GET /api/admin/review/:type/:id` — Review driver/customer
- `GET /api/admin/stats/revenue` — Revenue/day
- `GET /api/admin/stats/rides` — Total rides (area-wise)
- `GET /api/admin/stats/graphs` — Rides per area/driver/customer
- `GET /api/admin/billing/:id` — Get bill info

---

## Rides Service
### Endpoints
- `POST /api/rides` — Create ride
- `PUT /api/rides/:id` — Edit ride
- `DELETE /api/rides/:id` — Delete ride
- `GET /api/rides/customer/:customerId` — List rides by customer
- `GET /api/rides/driver/:driverId` — List rides by driver
- `GET /api/rides/stats/location` — Ride stats by location
- `GET /api/rides/nearby` — Drivers within 10 miles

---

## Kafka Event Structure
- All create/update/delete actions emit events to Kafka topics (e.g., `driver-events`, `ride-events`).
- Event format:
```json
{
  "eventType": "DRIVER_CREATED",
  "timestamp": "2024-09-01T12:00:00Z",
  "payload": { ... }
}
```

---

## Error Codes
- `400 Bad Request` — Invalid input, malformed data
- `401 Unauthorized` — Auth required
- `404 Not Found` — Entity not found
- `409 Conflict` — Duplicate entity
- `422 Unprocessable Entity` — Validation failed
- `500 Internal Server Error` — Server error

---

## Notes
- All endpoints return JSON.
- Use JWT for authentication where required.
- All state/zip/SSN validated as per project spec.
- See `validators.js` for validation logic.
