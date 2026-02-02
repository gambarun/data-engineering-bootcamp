# Homework 3: BigQuery Optimization
**Data Engineering Zoomcamp 2026**

---

## Question 1
**What is the count of records for the 2024 Yellow Taxi Data?**

**Answer:**  
**20,332,093**

**SQL Query:**
```sql
SELECT COUNT(*) 
FROM `dtc-de-course-485100.hw3_bucket_2024.yellow_tripdata_2024_h1`;
```

**Loading data to BigQuery:**
```
j-mac$  bq load \
  --source_format=PARQUET \
  dtc-de-course-485100:hw3_bucket_2024.yellow_tripdata_2024_h1 \
  "gs://dtc-de-course-data-lake-2026/yellow_tripdata_2024-0*.parquet"

Waiting on bqjob_r13953b61246dcbb6_0000019c1b82e529_1 ... (15s) Current status: DONE  
```

---
## Question 2

**Write a query to count the distinct number of PULocationIDs for the entire dataset on both the tables.  What is the estimated amount of data that will be read when this query is executed on the External Table and the Table?**

**Answer:**  
**0 MB for the External Table and 155.12 MB for the Materialized Table**

**SQL Query:**
```sql
SELECT COUNT(DISTINCT PULocationID) AS distinct_pu
FROM `dtc-de-course-485100.hw3_bucket_2024.yellow_tripdata_2024_h1`;
--262 --Bytes processed --0 B (results cached)

SELECT COUNT(DISTINCT PULocationID) AS distinct_pu
FROM `dtc-de-course-485100.hw3_bucket_2024.hw3_external`;
--Bytes processed 155.12 MB
```


---
## Question 3

**Write a query to retrieve the PULocationID from the table (not the external table) in BigQuery. Then retrieve PULocationID and DOLocationID. Why are the estimated bytes different?**

**Answer:**  
**BigQuery is a columnar database, and it only scans the specific columns requested in the query. Querying two columns (PULocationID, DOLocationID) requires reading more data than querying one column (PULocationID), leading to a higher estimated number of bytes processed.**

**SQL Query:**
```sql
-- Query one column
SELECT PULocationID
FROM `dtc-de-course-485100.hw3_bucket_2024.yellow_tripdata_2024_h1`;

-- Query two columns
SELECT PULocationID, DOLocationID
FROM `dtc-de-course-485100.hw3_bucket_2024.yellow_tripdata_2024_h1`;
```

---

## Question 4

**How many records have a fare_amount of 0?**

**Answer:**  
**8,333**

**SQL Query:**
```sql
SELECT COUNT(*)
FROM `dtc-de-course-485100.hw3_bucket_2024.yellow_tripdata_2024_h1`
WHERE fare_amount = 0;
```

---

## Question 5

**What is the best strategy to make an optimized table in BigQuery if your query will always filter based on `tpep_dropoff_datetime` and order by `VendorID`? (Create a new table with this strategy)**

**Answer:**  
**Partition by `tpep_dropoff_datetime` and Cluster on `VendorID`**

**SQL Query:**
```sql
CREATE OR REPLACE TABLE
  `dtc-de-course-485100.hw3_bucket_2024.yellow_tripdata_2024_h1_partitioned`
PARTITION BY DATE(tpep_dropoff_datetime)
AS
SELECT *
FROM `dtc-de-course-485100.hw3_bucket_2024.yellow_tripdata_2024_h1`
WHERE tpep_dropoff_datetime >= TIMESTAMP('2024-03-01')
  AND tpep_dropoff_datetime < TIMESTAMP('2024-03-16');
```

---

## Question 6

**Retrieve the distinct VendorIDs between `tpep_dropoff_datetime` 2024-03-01 and 2024-03-15 using the materialized table and then the partitioned table. Note the estimated bytes processed.**

**Answer:**  
**310.24 MB for non-partitioned table and 26.84 MB for the partitioned table**

**SQL Query:**
```sql
-- Non-partitioned table
SELECT DISTINCT(VendorID)
FROM `dtc-de-course-485100.hw3_bucket_2024.yellow_tripdata_2024_h1`
WHERE tpep_dropoff_datetime >= TIMESTAMP('2024-03-01')
  AND tpep_dropoff_datetime < TIMESTAMP('2024-03-16');

-- Partitioned table
SELECT DISTINCT(VendorID)
FROM `dtc-de-course-485100.hw3_bucket_2024.yellow_tripdata_2024_h1_partitioned`
WHERE tpep_dropoff_datetime >= TIMESTAMP('2024-03-01')
  AND tpep_dropoff_datetime < TIMESTAMP('2024-03-16');
```

---

## Question 7

**Where is the data stored in the External Table you created?**

**Answer:**  
**GCP Bucket**

---

## Question 8

**It is best practice in BigQuery to always cluster your data.**

**Answer:**  
**True**

