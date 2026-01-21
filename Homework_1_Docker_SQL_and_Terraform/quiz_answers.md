#`Homework_1_Docker_SQL_and_Terraform/quiz_answers.md`

---

````markdown
# Homework 1: Docker, SQL, and Terraform  
**Data Engineering Zoomcamp 2026**

---

## Question 1  
**What's the version of pip in the `python:3.13` image?**

**Answer:**  
**25.3**

**Command used:**
```bash
docker run --rm python:3.13 pip --version
````

**Output:**

```text
pip 25.3 from /usr/local/lib/python3.13/site-packages/pip (python 3.13)
```

**Verification inside container:**

```bash
docker run -it --rm python:3.13 bash
pip --version
```

---

## Question 2

**Given the `docker-compose.yaml`, what is the hostname and port that pgAdmin should use to connect to Postgres?**

**Answer:**

* From the host machine: `localhost:5432`
* From another container (e.g., pgAdmin): `postgres:5432`

---

## Question 3

**For the trips in November 2025, how many trips had a `trip_distance` less than or equal to 1 mile?**

**Answer:**
**8,007**

**SQL Query:**

```sql
SELECT COUNT(*)
FROM yellow_taxi_trips
WHERE lpep_pickup_datetime >= '2025-11-01'
  AND lpep_pickup_datetime < '2025-12-01'
  AND trip_distance <= 1;
```

**Notes:**

* Dataset: November 2025 Yellow Taxi Trip Records (Parquet)
* Source: NYC TLC Trip Record Data
* Files used:

  * `green_tripdata_2025-11.parquet`
  * `taxi_zone_lookup.csv`

---

## Question 4

**Which pickup day had the longest trip distance count (only trips with `trip_distance < 100` miles)?**

**Answer:**
**2025-11-20**

**SQL Query:**

```sql
SELECT *
FROM (
    SELECT
        TO_CHAR(lpep_pickup_datetime, 'YYYY-MM-DD') AS pickup_date,
        COUNT(*) AS trip_count
    FROM yellow_taxi_trips
    WHERE lpep_pickup_datetime >= '2025-11-01'
      AND lpep_pickup_datetime < '2025-12-01'
      AND trip_distance <= 100
    GROUP BY TO_CHAR(lpep_pickup_datetime, 'YYYY-MM-DD')
) t
ORDER BY trip_count DESC
limit 1;
```

---

## Question 5

**Which pickup zone had the largest total number of trips on November 18, 2025?**

**Answer:**
**East Harlem North**

**SQL Query:**

```sql
SELECT z."Zone"
FROM yellow_taxi_trips y
JOIN zones z
  ON y."PULocationID" = z."LocationID"
WHERE lpep_pickup_datetime >= '2025-11-18'
  AND lpep_pickup_datetime < '2025-11-19'
GROUP BY z."Zone"
ORDER BY COUNT(*) DESC
limit 1;
```

---

## Question 6

**For passengers picked up in the zone "East Harlem North" in November 2025, which drop-off zone had the largest number of tips?**

**Answer:**
**East Harlem South (1,790 trips)**

**SQL Query:**

```sql
SELECT F."DOLocationID", z."Zone", ROUND(SUM(F."tip_amount")::INT, 2) as total_tip
FROM (
SELECT * --y."DOLocationID", y."tip_amount"
FROM yellow_taxi_trips y
JOIN zones pu
  ON y."PULocationID" = pu."LocationID"
WHERE lpep_pickup_datetime >= '2025-11-01'
  AND lpep_pickup_datetime < '2025-12-01'
  AND y."PULocationID" = 74 --pu."Zone" = 'East Harlem North'
)F LEFT JOIN zones z
  ON F."DOLocationID" = z."LocationID"
GROUP BY  F."DOLocationID", z."Zone" 
ORDER BY ROUND(SUM(F."tip_amount")::BIGINT, 2) DESC
limit 1
```

---

## Question 7

**Which sequence describes the Terraform workflow for:**

1. Downloading plugins and setting up backend
2. Generating and executing changes
3. Removing all resources

**Answer:**

```bash
terraform init
terraform apply -auto-approve
terraform destroy
```

**Explanation:**

* `terraform init` initializes providers and backend
* `terraform apply` creates or updates infrastructure
* `terraform destroy` removes all managed resources

```


