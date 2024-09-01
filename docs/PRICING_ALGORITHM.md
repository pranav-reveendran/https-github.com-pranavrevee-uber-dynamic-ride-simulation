# Dynamic Pricing Algorithm

## Overview
The dynamic pricing algorithm predicts ride fares using a combination of base fare calculation, historical data, and machine learning. It leverages the Kaggle Uber Fares dataset to train and validate the model, ensuring prices reflect real-world supply and demand.

## Data Used
- [Kaggle Uber Fares Dataset](https://www.kaggle.com/datasets/yasserh/uber-fares-dataset)
  - Features: key, fare_amount, pickup_datetime, passenger_count, pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude

## Algorithm Steps
1. **Base Fare Calculation**
   - Base fare = f(distance, time, local rates)
   - Distance calculated using Haversine formula between pickup and dropoff
2. **Historical Data & Demand**
   - Analyze historical demand, driver supply, peak times, and traffic
   - If demand > supply, apply surge multiplier
3. **Machine Learning Model**
   - Train regression model (Random Forest/Gradient Boosting) on Kaggle data
   - Features: distance, time of day, day of week, pickup/dropoff location, passenger count
   - Output: Predicted fare
4. **Dynamic Adjustment**
   - Real-time: If active demand spikes, increase multiplier
   - Example: `final_fare = base_fare * surge_multiplier`

## Example Formula
```
base_fare = base_rate + (per_mile_rate * distance) + (per_minute_rate * duration)
surge_multiplier = 1 + (current_demand - available_drivers) / available_drivers
final_fare = ML_predicted_fare * surge_multiplier
```

## ML Model Details
- **Input Features:**
  - Distance (Haversine)
  - Pickup time (hour, day)
  - Pickup/dropoff lat/long (binned)
  - Passenger count
- **Target:** fare_amount
- **Model:** Random Forest Regressor (scikit-learn)
- **Validation:** 80/20 train/test split, RMSE metric

## Rationale
- Combines interpretable base fare with data-driven ML prediction
- Adapts to real-time supply/demand for fairness and efficiency
- Proven on real Uber data for accuracy

## Usage in System
- Fare prediction endpoint uses trained model
- Admin can view pricing analytics and adjust parameters
- All fare calculations logged for audit
