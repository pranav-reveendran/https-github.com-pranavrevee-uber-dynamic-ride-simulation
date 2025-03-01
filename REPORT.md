# Uber Distributed Ride Simulation - Project Report

## SP25: DATA-236 Sec 12 - Distributed Systems

---

## Table of Contents
- [Client Application Screenshots](#client-application-screenshots)
- [Booking a Ride](#booking-a-ride)
- [Test Output (JMeter)](#test-output-jmeter)
- [Redis Caching](#redis-caching)
- [Dynamic Pricing & ML Model](#dynamic-pricing--ml-model)
- [Geo-Mapping & Real-Time](#geo-mapping--real-time)
- [Object Management Policy](#object-management-policy)
- [Handling Heavyweight Resources](#handling-heavyweight-resources)
- [Database Write Policy](#database-write-policy)
- [Observations & Lessons Learned](#observations--lessons-learned)

---

## Client Application Screenshots
![Client Application Screenshot](docs/screenshots/client_app.png)

## Booking a Ride
![Booking a Ride](docs/screenshots/booking_ride.png)

## Test Output (JMeter)
![JMeter Output](docs/screenshots/jmeter_output.png)

## Redis Caching
![Redis Caching](docs/screenshots/redis_caching.png)

---

## Dynamic Pricing & ML Model
- **Goal:** Predict fair, real-time ride prices based on distance, demand, and historical data.
- **Data:** Trained on the [Kaggle Uber Fares Dataset](https://www.kaggle.com/datasets/yasserh/uber-fares-dataset).
- **Features Used:**
  - Pickup/dropoff coordinates (geo-mapping)
  - Time of day, day of week
  - Distance (Haversine formula)
  - Passenger count
  - Historical demand/supply
- **Model:** Random Forest Regressor (scikit-learn)
- **Workflow:**
  1. Preprocessing: Clean and transform raw ride data.
  2. Feature Engineering: Calculate distance, encode time/location, normalize.
  3. Training: Fit regression model to predict `fare_amount`.
  4. Serving: Expose `/api/pricing/predict` endpoint for real-time fare prediction.
  5. Dynamic Adjustment: Apply surge multiplier if demand > supply.
- **Example Formula:**
  ```
  base_fare = base_rate + (per_mile_rate * distance) + (per_minute_rate * duration)
  surge_multiplier = 1 + (current_demand - available_drivers) / available_drivers
  final_fare = ML_predicted_fare * surge_multiplier
  ```
- **Why it matters:**
  - Ensures fair pricing for riders and drivers.
  - Adapts to real-time city demand.
  - Proven on real Uber data.

---

## Geo-Mapping & Real-Time
- **Frontend:** Interactive maps for ride booking and driver tracking.
- **Backend:**
  - Stores and queries geo-coordinates for drivers/rides.
  - Finds drivers within 10 miles using geospatial queries (MongoDB/Redis).
- **Real-Time:**
  - Uses Socket.io for live ride status and driver location updates.

---

## Object Management Policy
- Modular structure: `models/`, `controllers/`, `services/`, `routes/`, `middleware/`
- Models define MongoDB schemas for users, rides, billing.
- Controllers handle business logic for each route.
- Services encapsulate reusable logic (pricing, ML, uploads).
- Routes map HTTP endpoints to controller functions.
- Middleware for authentication, authorization, validation.
- Ensures maintainability, scalability, and clear separation of concerns.

---

## Handling Heavyweight Resources
- **MongoDB:** Single connection pool, monitored and reused.
- **File Uploads:** Cloudinary + Multer, offloading storage.
- **External APIs:** Wrapped in try-catch with retries.
- **Socket.io:** Cleaned up on disconnect to avoid leaks.
- **Redis:** Used for caching and session management, with fallback logic.

---

## Database Write Policy
- **Validation:** All data validated before DB write.
- **Auth:** Only authenticated/authorized users can write.
- **Event Triggers:** Immediate write for critical events (booking, status change).
- **Batching:** Non-critical logs/events batched for efficiency.

---

## Observations & Lessons Learned
> This project taught us the value of modular design, real-time systems, and robust distributed architecture. Load testing with JMeter (10,000 users) revealed the importance of DB indexing and connection pooling. Clean Git workflows, code reviews, and thorough API testing were key to our success. Next, we aim to automate CI/CD and improve monitoring for real-time system health. 