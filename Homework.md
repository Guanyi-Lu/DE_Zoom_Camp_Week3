## Set up
### download data using git bash
```bash
for month in $(seq -w 1 12); do
  curl -O "https://d37ci6vzurychx.cloudfront.net/trip-data/green_tripdata_2022-${month}.parquet"
done
```
### upload to storage bucket

```bash
$ gsutil -m cp green_tripdata_2022-*.parquet gs://homework_3_bucket/
```

### create external table:

```sql
CREATE OR REPLACE EXTERNAL TABLE `sigma-icon-447800-e1.homework_3_data.green_taxi_2022_external_table`
OPTIONS (
  format = 'PARQUET',
  uris = ['gs://homework_3_bucket/*.parquet']
);
```
### Create a native table:

```sql
CREATE TABLE `sigma-icon-447800-e1.homework_3_data.green_taxi_2022_native_table`
AS
SELECT * FROM `sigma-icon-447800-e1.homework_3_data.green_taxi_2022_external_table`;
```

#### Q1:
```sql

SELECT COUNT(*) FROM `sigma-icon-447800-e1.homework_3_data.green_taxi_2022_native_table`;
```

#### Q2:
```sql
 SELECT
  DISTINCT count (PULocationID)
FROM
  `sigma-icon-447800-e1.homework_3_data.green_taxi_2022_external_table`;

SELECT
  DISTINCT count (PULocationID)
FROM
  `sigma-icon-447800-e1.homework_3_data.green_taxi_2022_native_table`;
```

#### Q3:
```sql
SELECT COUNT(*)
FROM
  `sigma-icon-447800-e1.homework_3_data.green_taxi_2022_native_table`
WHERE fare_amount=0
```
#### Q4:
```sql
create OR REPLACE table homework_3_data.green_tripdata_2022_clustered_partitioned
partition by DATE(lpep_pickup_datetime)
cluster by PUlocationID as (
  select * from `sigma-icon-447800-e1.homework_3_data.green_taxi_2022_native_table`
);
```
#### Q5:
```sql
SELECT DISTINCT PULocationID
FROM
  `sigma-icon-447800-e1.homework_3_data.green_taxi_2022_native_table`
WHERE DATE(lpep_pickup_datetime) BETWEEN '2022-06-01' AND '2022-06-30';
```
ESTIMATE PROCESS 12.82MB

```sql
SELECT DISTINCT PULocationID
FROM
  `sigma-icon-447800-e1.homework_3_data.green_tripdata_2022_clustered_partitioned`
WHERE DATE(lpep_pickup_datetime) BETWEEN '2022-06-01' AND '2022-06-30'
```
ESTIMATE PROCESS 1.12MB

#### Q6:GCS bucket

#### Q7: FALSE
When to Cluster:
Tables larger than 1 GB.
Queries frequently filter/sort on specific columns (e.g., user_id, event_date).


#### Q8:
0 Because BigQuery stores the total number of rows in the table’s metadata. 
BigQuery can return the result using metadata without actually processing table data.