# Module 1 Homework

# Docker & SQL 

## Question 1. Knowing docker tags
docker run --help --rm - Automativally remove the container when it exits

## Question 2. Understanding docker first run 
The version of the package wheel in python:3.9 - 0.42.0

# Prepare Postgres

## Question 3. Count records

```
SELECT 
  COUNT(index) 
FROM 
  green_taxi_trips 
WHERE 
  lpep_pickup_datetime :: DATE > '2019-09-17' 
  AND lpep_dropoff_datetime :: DATE < '2019-09-19'
```

**Answer**: 15612

## Question 4. Largest trip for each day

```
SELECT 
  lpep_pickup_datetime :: DATE as pickup_date, 
  MAX(trip_distance) AS largest 
FROM 
  green_taxi_trips 
GROUP BY 
  pickup_date 
ORDER BY 
  largest DESC
```

Answer: 2019-09-26

## Quesion 5. Three biggest pick up Boroughs

```
SELECT 
  borough, 
  SUM(total_amount) AS sum_amount 
FROM 
  green_taxi_trips 
  LEFT JOIN taxi_zone_lookup ON (
    taxi_zone_lookup.location_id = green_taxi_trips.pulocation_id
  ) 
GROUP BY 
  borough 
HAVING 
  SUM(total_amount) > 50000 
ORDER BY 
  sum_amount DESC
```

Answer: "Bronx" "Queens" Staten Island"

## Question 6. Largest tip

```
SELECT 
  borough, 
  zone, 
  lpep_pickup_datetime, 
  MAX(tip_amount) AS max_tip 
FROM 
  green_taxi_trips 
  LEFT JOIN taxi_zone_lookup ON (
    taxi_zone_lookup.location_id = green_taxi_trips.dolocation_id
  ) 
WHERE 
  DATE_PART('year', lpep_pickup_datetime) = '2019' 
  AND DATE_PART('month', lpep_pickup_datetime) = 9 
  AND pulocation_id = (
    SELECT 
      location_id 
    FROM 
      taxi_zone_lookup 
    WHERE 
      zone = 'Astoria'
  ) 
GROUP BY 
  borough, 
  zone, 
  lpep_pickup_datetime 
ORDER BY 
  max_tip DESC
```

Answer: JFK Airport