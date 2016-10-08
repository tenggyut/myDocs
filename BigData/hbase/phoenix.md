##Phoenix调研

###Catalog

Phoenix用System.Catalog这张表存储表结构，包括表名、列名、类型等

###Schema

```
create table myschema.mytable(....)
```
这个建表语句会新建一张表名为**myschema.mytable**的HBase物理表。其中**myschema**会被Phoenix单独存到**system.catalog**里的**table_schem**列中，其主要作用是起到一个namespace的作用，便于对表分类

###View

创建View的语句如下：

```
CREATE VIEW mobile_product_metrics (carrier VARCHAR, dropped_calls BIGINT) AS
SELECT * FROM product_metrics
WHERE metric_type = 'm';
```

好处：

- 避免创建很多窄的物理表，而是用少数宽物理表+很多虚拟表
- 便于对列按业务归类

不足：

- 相比传统SQL数据，没有性能上的优势，因为Phoenix不会讲View的结果缓存下来，而仅仅是存储View的查询语句

###安全

Phoenix通过多租户的概念实现数据访问隔离。

```
CREATE TABLE base.event (tenant_id VARCHAR, event_type CHAR(1), created_date DATE, event_id BIGINT)
MULTI_TENANT=true;
```

其底层实现就是在rowkey上加一个租户id前缀，然后扫描的时候就带上这个租户id进行扫描。这个租户id是在建立连接的时候设置的，如下：

```
Properties props = new Properties();
props.setProperty("TenantId", "Acme");
Connection conn = DriverManager.getConnection("localhost", props);
```

###性能测试

####performance.py

这是Phoenix自带的一个性能测试脚本，其中预定义了5个常见SQL场景，然后由用户输入需要测试的数据量级并给出这5个SQL的响应时间。其优点是简单易用，但局限是只有单表测试。

使用:

```
$PHOENIX_HOME/bin/performance.py zk1,zk2 2000000
```

####[Pherf](https://phoenix.apache.org/pherf.html)

这是Phoenix提供的测试框架，可以自定义测试场景、测试量级。其优点是灵活度很高，可配置性强。缺点是需要自己写测试用例和测试场景

使用:

```
./pherf.sh -drop all -l -q -z localhost -schemaFile .*user_defined_schema.sql -scenarioFile .*user_defined_scenario.xml
```

参数解释:

```
-h Help 
-l Apply schema and load data
-q Executes Multi-threaded query sets and write results
-z [quorum] Zookeeper quorum
-m Enable monitor for statistics
-monitorFrequency [frequency in Ms] _Frequency at which the monitor will snopshot stats to log file. 
-drop [pattern] Regex drop all tables with schema name as PHERF. Example drop Event tables: -drop .(EVENT). Drop all: -drop .* or -drop all
-scenarioFile Regex or file name of a specific scenario file to run. 
-schemaFile Regex or file name of a specific schema file to run. 
-export Exports query results to CSV files in CSV_EXPORT directory 
-diff Compares results with previously exported results 
-hint Executes all queries with specified hint. Example SMALL 
-rowCountOverride
-rowCountOverride [number of rows] Specify number of rows to be upserted rather than using row count specified in schema
```

###一些概念解释

- GLOBAL TEMPORARY: 没找到。。。oracle倒是有
- LOCAL TEMPORARY: 也没找到。。。oracle貌似有这个东西
- ALIAS: 不就是别名么？
- SYNONYM: 也没有找到。。。


##Phoenix And Spark Integration

###依赖

添加如下依赖:

```
libraryDependencies ++= Seq(
  "org.apache.spark" %% "spark-core" % "1.6.0" ,
  "org.apache.spark" %% "spark-sql" % "1.6.0"  ,
  "org.apache.spark" %% "spark-hive" % "1.6.0"  ,
  "org.apache.spark" %% "spark-mllib" % "1.6.0"  ,
  "org.apache.spark" %% "spark-streaming" % "1.6.0"  ,
  "org.apache.phoenix" % "phoenix-spark" % "4.6.0-HBase-0.98" ,
  "org.apache.phoenix" % "phoenix-core" % "4.6.0-HBase-0.98" ,
  "org.apache.hbase" % "hbase-common" % "0.98.12-hadoop2"
)
```

###Demo

使用spark执行如下Phoenix SQL查询

```
select d.country_name as "国家名","2011","2012","2013","2014","2015" from DEVELOPE_INDICATOR d where d.indicator_code = 'CM.MKT.INDX.ZG' and "2011" is not null and  "2012" is not null and  "2013" is not null and  "2014" is not null and  "2015" is not null order by abs("2015") desc limit 10;
```

对应的scala代码:

```
val sc = new SparkContext("local", "phoenix-test")
val sqlContext = new SQLContext(sc)

val di = sqlContext.load("org.apache.phoenix.spark",
      Map("table" -> "TEST.DEVELOPE_INDICATOR", "zkUrl" -> "NEOInciteDataNode-1:2181"))


    di
      .filter(di("indicator_code".toUpperCase()) === "CM.MKT.INDX.ZG"
        && di("2011").isNotNull && di("2012").isNotNull
        && di("2013").isNotNull && di("2014").isNotNull && di("2015").isNotNull)
      .orderBy(abs(di("2015")).desc)
      .select(di("country_name".toUpperCase()).as("国家名")
        , di("2011"), di("2012"), di("2013"), di("2014"), di("2015"))
      .take(10).foreach(r => println(r.toString()))
```

###执行时间 

- spark: 1.987 s
- Phoenix: 5.967 s