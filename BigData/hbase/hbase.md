###HBASE

####JVM

```
export HBASE_OPTS="-server -Xms20g -Xmx20g -Xmn1g -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=70"
```

####CopyTable
```
$HBASE_HOME/bin/hbase org.apache.hadoop.hbase.mapreduce.CopyTable --peer.adr=dstClusterZK:2181:/hbase tableOrig
```

####OutOfOrderScannerNextException

if this Exception happens and your rpc/client-scanner-period seems ok, check region server's log, may be something is wrong there.

###ImportTable
```
$HBASE_HOME/bin/hbase org.apache.hadoop.hbase.mapreduce.Import table hdfs-path
```

###ycsb pre-split usertable

```
create 'usertable', 'family', {SPLITS => (1..n_splits).map {|i| "user#{1000+i*(9999-1000)/n_splits}"}}
```