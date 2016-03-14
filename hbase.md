###HBASE

####CopyTable
```
$HBASE_HOME/bin/hbase org.apache.hadoop.hbase.mapreduce.CopyTable --peer.adr=dstClusterZK:2181:/hbase tableOrig
```

####OutOfOrderScannerNextException

if this Exception happens and your rpc/client-scanner-period seems ok, check region server's log, may be something is wrong there.