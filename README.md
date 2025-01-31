## Data Warehouse and BigQuery

### OLAP VS OLTP

Online analytical processing (OLAP) and online transaction processing (OLTP) are two different data processing systems that have different purposes and use cases. OLAP is used to analyze data, while OLTP is used to process transactions. 

#### Purpose 
OLAP: Analyzes large amounts of data to generate reports, identify trends, and make business decisions
OLTP: Processes transactions in real-time to manage daily business operations

#### Use cases
OLAP: Used by data scientists, business analysts, and knowledge workers 
OLTP: Used by frontline workers, such as cashiers, bank tellers, and hotel desk clerks 

#### Data 
OLAP: Optimized for heavy read, low write workloads
OLTP: Optimized for processing a large number of transactions
Examples
OLAP: Used to generate sales reports, forecast trends, and evaluate permits 
OLTP: Used to process online purchases, bank transactions, and inventory management 

#### Data integrity 
OLAP: Data integrity is unaffected because the database is not often updated
OLTP: Data integrity constraint must be maintained

### Partitioning and clustering in BigQuery

#### Partitioning:
1. A partitioned table is divided into segments, called partitions, that make it easier to manage and query your data. By dividing a large table into smaller partitions, you can improve query performance and control costs by reducing the number of bytes read by a query. You partition tables by specifying a partition column which is used to segment the table

2. See Partition Information:
```sql
  SELECT table_name, partition_id,total_rows,
  FROM   zoomcamp.INFORMATION_SCHEMA.PARTITIONS
  WHERE table_name="green_tripdata"
  order by total_rows desc
```
3. Documentation: https://cloud.google.com/bigquery/docs/creating-partitioned-tables

#### Clustering

clustering sorts the table based on user-defined columns.

```sql
create OR REPLACE table zoomcamp.green_tripdata_clustered
partition by DATE(lpep_pickup_datetime)
cluster by VendorID as (
  select * from zoomcamp.green_tripdata
);
```
