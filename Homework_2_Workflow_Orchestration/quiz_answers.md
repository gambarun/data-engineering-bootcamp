# Homework 2: Workflow Orchestration
**Data Engineering Zoomcamp 2026**

---

## Question 1
**Within the execution for Yellow Taxi data for the year 2020 and month 12: what is the uncompressed file size (i.e. the output file `yellow_tripdata_2020-12.csv` of the extract task)?**

**Answer:**  
**134.5 MiB**

**Resolution / Verification:**  
Check bucket under Cloud Storage → Click on bucket name → Check Object → CSV file:  
`dtc-de-course-data-lake-2026/yellow_tripdata_2020-12.csv`  

[Cloud Storage link](https://console.cloud.google.com/storage/browser/_details/dtc-de-course-data-lake-2026/yellow_tripdata_2020-12.csv;tab=live_object?authuser=1&project=dtc-de-course-485100&supportedpurview=project)

---

## Question 2
**What is the rendered value of the variable `file` when the inputs `taxi` is set to `green`, `year` is set to `2020`, and `month` is set to `04` during execution?**

**Answer:**  
`green_tripdata_2020-04.csv`

---

## Question 3
**How many rows are there for the Yellow Taxi data for all CSV files in the year 2020?**

**Answer:**  
**24,648,499**

**SQL Query:**
```sql
WITH temp AS (
  SELECT count(*) AS cnt FROM `dtc-de-course-485100.zoomcamp.yellow_tripdata_2020_01`
  UNION ALL
  SELECT count(*) AS cnt FROM `dtc-de-course-485100.zoomcamp.yellow_tripdata_2020_02`
  UNION ALL
  SELECT count(*) AS cnt FROM `dtc-de-course-485100.zoomcamp.yellow_tripdata_2020_03`
  UNION ALL
  SELECT count(*) AS cnt FROM `dtc-de-course-485100.zoomcamp.yellow_tripdata_2020_04`
  UNION ALL
  SELECT count(*) AS cnt FROM `dtc-de-course-485100.zoomcamp.yellow_tripdata_2020_05`
  UNION ALL
  SELECT count(*) AS cnt FROM `dtc-de-course-485100.zoomcamp.yellow_tripdata_2020_06`
  UNION ALL
  SELECT count(*) AS cnt FROM `dtc-de-course-485100.zoomcamp.yellow_tripdata_2020_07`
  UNION ALL
  SELECT count(*) AS cnt FROM `dtc-de-course-485100.zoomcamp.yellow_tripdata_2020_08`
  UNION ALL
  SELECT count(*) AS cnt FROM `dtc-de-course-485100.zoomcamp.yellow_tripdata_2020_09`
  UNION ALL
  SELECT count(*) AS cnt FROM `dtc-de-course-485100.zoomcamp.yellow_tripdata_2020_10`
  UNION ALL
  SELECT count(*) AS cnt FROM `dtc-de-course-485100.zoomcamp.yellow_tripdata_2020_11`
  UNION ALL
  SELECT count(*) AS cnt FROM `dtc-de-course-485100.zoomcamp.yellow_tripdata_2020_12`
)
SELECT SUM(cnt) AS total_rows
FROM temp;
````

---

## Question 4
**How many rows are there for the Green Taxi data for all CSV files in the year 2020?**

**Answer:**  
**1,734,051**

**SQL Query:**
```sql
WITH temp AS (
  SELECT count(*) AS cnt FROM `dtc-de-course-485100.zoomcamp.green_tripdata_2020_01`
  UNION ALL
  SELECT count(*) AS cnt FROM `dtc-de-course-485100.zoomcamp.green_tripdata_2020_02`
  UNION ALL
  SELECT count(*) AS cnt FROM `dtc-de-course-485100.zoomcamp.green_tripdata_2020_03`
  UNION ALL
  SELECT count(*) AS cnt FROM `dtc-de-course-485100.zoomcamp.green_tripdata_2020_04`
  UNION ALL
  SELECT count(*) AS cnt FROM `dtc-de-course-485100.zoomcamp.green_tripdata_2020_05`
  UNION ALL
  SELECT count(*) AS cnt FROM `dtc-de-course-485100.zoomcamp.green_tripdata_2020_06`
  UNION ALL
  SELECT count(*) AS cnt FROM `dtc-de-course-485100.zoomcamp.green_tripdata_2020_07`
  UNION ALL
  SELECT count(*) AS cnt FROM `dtc-de-course-485100.zoomcamp.green_tripdata_2020_08`
  UNION ALL
  SELECT count(*) AS cnt FROM `dtc-de-course-485100.zoomcamp.green_tripdata_2020_09`
  UNION ALL
  SELECT count(*) AS cnt FROM `dtc-de-course-485100.zoomcamp.green_tripdata_2020_10`
  UNION ALL
  SELECT count(*) AS cnt FROM `dtc-de-course-485100.zoomcamp.green_tripdata_2020_11`
  UNION ALL
  SELECT count(*) AS cnt FROM `dtc-de-course-485100.zoomcamp.green_tripdata_2020_12`
)
SELECT SUM(cnt) AS total_rows
FROM temp;
```

---

## Question 5
**How many rows are there for the Yellow Taxi data for the March 2021 CSV file?**

**Answer:**  
**1,925,152**

**SQL Query:**
```sql
SELECT count(*) AS cnt 
FROM `dtc-de-course-485100.zoomcamp.yellow_tripdata_2021_03`;
```

--- 

## Question 6
**How would you configure the timezone to New York in a Schedule trigger?**

**Answer:**  
**Add a timezone property set to America/New_York in the Schedule trigger.**

**YAML Example:**
[Kestra official page](https://kestra.io/docs/workflow-components/triggers/schedule-trigger)

```yaml
triggers:
  - id: daily
    type: io.kestra.plugin.core.trigger.Schedule
    cron: "@daily"
    timezone: America/New_York
