

### create external table:

```sql
CREATE OR REPLACE EXTERNAL TABLE
  `sigma-icon-447800-e1.homework_3_data.yellow_taxi_2024_external_table` OPTIONS ( format = 'PARQUET',
    uris = ['gs://homework_3_bucket/*.parquet'] );

```
### Create a native table:

```sql
CREATE TABLE
  `sigma-icon-447800-e1.homework_3_data.yellow_taxi_2024_native_table` AS
SELECT
  *
FROM
  `sigma-icon-447800-e1.homework_3_data.yellow_taxi_2024_external_table`;
```

#### Q1:
```sql

SELECT COUNT(*) FROM `sigma-icon-447800-e1.homework_3_data.yellow_taxi_2024_native_table`;
```

#### Q2:
```sql
SELECT
  DISTINCT count (PULocationID)
FROM
  `sigma-icon-447800-e1.homework_3_data.yellow_taxi_2024_external_table`;

SELECT
  DISTINCT count (PULocationID)
FROM
  `sigma-icon-447800-e1.homework_3_data.yellow_taxi_2024_native_table`;
```

#### Q3:
```sql
select PULocationID
FROM
  `sigma-icon-447800-e1.homework_3_data.yellow_taxi_2024_native_table`;

select PULocationID,DOLocationID
FROM
  `sigma-icon-447800-e1.homework_3_data.yellow_taxi_2024_native_table`;
```
#### Q4:
```sql
SELECT COUNT(*)
FROM
  `sigma-icon-447800-e1.homework_3_data.yellow_taxi_2024_native_table`
WHERE fare_amount=0;
```
#### Q5:
```sql
create OR REPLACE table homework_3_data.yellow_tripdata_2024_clustered_partitioned
partition by DATE(tpep_dropoff_datetime)
cluster by VendorID as (
  select * from `sigma-icon-447800-e1.homework_3_data.yellow_taxi_2024_native_table`
);
```

#### Q6:

```sql

SELECT DISTINCT VendorID
FROM
  `sigma-icon-447800-e1.homework_3_data.yellow_taxi_2024_native_table`
WHERE DATE(tpep_dropoff_datetime) BETWEEN '2024-03-01' AND '2024-03-15';


SELECT DISTINCT VendorID
FROM
  `sigma-icon-447800-e1.homework_3_data.yellow_tripdata_2024_clustered_partitioned`
WHERE DATE(tpep_dropoff_datetime) BETWEEN  '2024-03-01' AND '2024-03-15';
```
#### Q7:GCS bucket

#### Q8: FALSE
When to Cluster:
Tables larger than 1 GB.
Queries frequently filter/sort on specific columns (e.g., user_id, event_date).


#### Bonus question:
0 Because BigQuery stores the total number of rows in the table’s metadata. 
BigQuery can return the result using metadata without actually processing table data.
