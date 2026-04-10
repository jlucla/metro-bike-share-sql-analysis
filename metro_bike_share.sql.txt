-- =========================
-- LA Bike Share Usage Analysis
-- =========================
--Dataset: LA Metro Bike Share Trip Data 2025 Q4 (October –  December)
--Source: https://bikeshare.metro.net/about/data/
--Author: Joey Lee

--Data Overview
SELECT * 
FROM metro_bike_clean
LIMIT 5;

--What hours tend to be the most and least popular?
SELECT 
start_hour,
COUNT(*) AS total_trips
FROM metro_bike_clean
GROUP BY start_hour
ORDER BY total_trips DESC;

--How does usage vary by time of day?
SELECT 
start_period,
COUNT(*) AS total_trips
FROM metro_bike_clean
GROUP BY start_period
ORDER BY total_trips DESC;

--What are the top 10 most popular stations where trips began?
SELECT 
start_station_na,
COUNT(*) AS total_trips
FROM metro_bike_clean
GROUP BY start_station_na
ORDER BY total_trips DESC
LIMIT 10;

--What are the top 10 least popular stations where trips began?
SELECT 
start_station_na,
COUNT(*) AS total_trips
FROM metro_bike_clean
GROUP BY start_station_na
ORDER BY total_trips 
LIMIT 10;

--How does usage vary by month?
SELECT
start_month,
COUNT(*) AS total_trips
FROM metro_bike_clean
GROUP BY start_month
ORDER BY total_trips DESC;

--How does average trip duration (in minutes) vary by month?
SELECT
start_month,
ROUND(AVG(duration_min),2) AS avg_duration
FROM metro_bike_clean
GROUP BY start_month
ORDER BY avg_duration;

--What percentage of all trips are one-way trips?
SELECT
trip_route_categ AS category,
COUNT(*) as trip_count,
ROUND(
  (COUNT(*) * 1.0 / SUM(COUNT(*)) OVER ()) * 100,
  2
	) AS percent
from metro_bike_clean
GROUP BY trip_route_categ;

--What times of day tend to have longer trips?
SELECT
    start_period,
    ROUND(AVG(duration_min),2) as avg_duration
FROM metro_bike_clean
GROUP BY start_period
ORDER BY avg_duration DESC;

--How does popularity of one-way vs round trip vary by hour?
SELECT
    start_hour,
    trip_route_categ,
    COUNT(*) AS trip_count,
    ROUND(
        COUNT(*) * 100.0 / SUM(COUNT(*)) OVER (PARTITION BY start_hour),
        2
    ) AS percent_within_hour
FROM metro_bike_clean
GROUP BY start_hour, trip_route_categ
ORDER BY start_hour;

--What are the top 10 stations where the longest rides (on average) begin?
SELECT
    start_station_na,
    ROUND(AVG(duration_min), 2) AS avg_duration
FROM metro_bike_clean
GROUP BY start_station_na
ORDER BY avg_duration DESC
LIMIT 10;

--How do total trips vary by hour for weekdays in November?
SELECT
    start_hour,
    COUNT(*) AS total_trips
FROM metro_bike_clean
WHERE start_month = 'November'
  AND start_weekday IN ('Monday','Tuesday','Wednesday','Thursday','Friday')
GROUP BY start_hour
ORDER BY start_hour;
