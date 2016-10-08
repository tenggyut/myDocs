##Hive

###Create External Table

- create external table whose data is at a specific location

```
create external table hive_test (HDXXSJZLB_ID INT, HDBH STRING,HDJJ STRING, HDBJ STRING,HDLY STRING,HDCH STRING,SQLY STRING,XXJS STRING) STORED AS PARQUET LOCATION '/hive_test/';
```