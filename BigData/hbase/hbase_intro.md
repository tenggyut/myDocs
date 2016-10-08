##HBASE简介

###HBASE架构

![](http://i2.itc.cn/20160507/3084_60e220a0_a581_3731_0413_61f28797ccf3_1.png)

###HBASE存储模型

- 抽象模型
  ![](http://pic002.cnblogs.com/images/2012/79891/2012060400513336.jpg)
	- Row Key：Table主键 行键 Table中记录按照Row Key排序
	- Timestamp：每次对数据操作对应的时间戳，也即数据的version number
	- Column Family：列簇，一个table在水平方向有一个或者多个列簇，列簇可由任意多个Column组成，列簇支持动态扩展，无须预定义数量及类型，二进制存储，用户需自行进行类型转换

- 实际存储结构的简化版:
  
  ![](http://pic002.cnblogs.com/images/2012/79891/2012060400583891.jpg)

###表设计

>"HBase的一张表只对应一个查询"

1. Rowkey根据查询业务进行安排，长度控制在128个字节以内，主要是内存考虑
2. 预切分，防止热点和提高吞吐量
3. 万不得已，可以考虑添加索引表

###0.98.x的API

####连接HBASE

```
Configuration conf = HBaseConfiguration.create();
conf.set(HConstants.ZOOKEEPER_QUORUM, "zk1,zk2,zk3");
HConnection connection = HConnectionManager.createConnection(conf);
```

**Configuration**里还可以设置其他参数，比如scan的timeout等，可以根据需求进行添加。其中Zookeeper的地址是必须配置的

####表管理（建表，删表）

```
Configuration conf = HBaseConfiguration.create();
conf.set(HConstants.ZOOKEEPER_QUORUM, "zk1,zk2,zk3");
HBaseAdmin admin = new HBaseAdmin(conf);

//新建命名空间
admin.createNamespace(NamespaceDescriptor.create("my_ns").build());

//建表
HTableDescriptor tableDesc = new HTableDescriptor(TableName.valueOf("my_ns:mytable"));
tableDesc.setDurability(Durability.SYNC_WAL);
//add a column family "mycf"
HColumnDescriptor hcd = new HColumnDescriptor("mycf");
tableDesc.addFamily(hcd);
admin.createTable(tableDesc);

//删表
String tablename = "my_ns:mytable";
if(admin.tableExists(tablename)) {
  try {
      admin.disableTable(tablename);
      admin.deleteTable(tablename);
  } catch (Exception e) {
      // TODO: handle exception
      e.printStackTrace();
  }
}
admin.close();
```

####获取表操作对象

```
HTableInterface table = connection.getTable("table");
```

关于**HTableInterface**有个非常重要的设置需要了解:

```
/* @param autoFlush
 *          Whether or not to enable 'auto-flush'.
 * @param clearBufferOnFail
 *          Whether to keep Put failures in the writeBuffer. If autoFlush is true, then
 *          the value of this parameter is ignored and clearBufferOnFail is set to true.
 *          Setting clearBufferOnFail to false is deprecated since 0.96.
table.setAutoFlush(autoFlush, clearBufferOnFail);
```

####Get、Delete、Put

- Get操作

  ```
Get get = new Get("rowkey".toBytes());
Result row = table.get(get);
byte[] value = r.getValue("cf".getBytes(), "qualifier/column".getBytes());
  ```

- Put操作

  ```
  //单个Put
  Put p = new Put("rowkey".getBytes());
  p.add("cf".getBytes(), "qualifier/column".getBytes(), "value".getBytes());
  ...
  table.put(p);
  
  //批量Put
  List<Put> puts = new ArrayList();
  puts.add(put);
  ...
  
  table.put(puts);
  ```
- Delete操作

  ```
  Delete delete = new Delete("rowkey".getBytes());
  table.delete(delete);
  ```  
  
####Scan

```
Scan s = new Scan();
s.setCache(3000);//一次从Server端取多少条数据到客户端
s.setStartRow("startrow".getBytes());//从rowkey为"startrow"的记录开始扫描
s.setStopRow("stoprow".getBytes());//扫描在rowkey为"stoprow“的记录处停止，扫描结果不包括该条记录
s.setTimeStamp(min,max);//[min,max),扫描结果数据的插入时间戳在这个范围之内
ResultScanner rs = table.getScanner(s);
for (Result r : rs) {...}
```  

Tip1: 在使用Scan的时候，我们很多时候是需要扫描具有某个固定rowkey前缀的数据，这时我们可以按如下方式设置*startrow*和*stoprow*：

```
byte[] start = "rowkey_prefix".getBytes();
byte[] end = "rowkey_prefix".getBytes();
end[end.length - 1] = end[end.length - 1] + 1;
s.setStartRow(start);
s.setStopRow(end);
```

HBase的Scan也提供了一个过滤器框架（内置了很多过滤器，也可以进行自定义），但HBase不擅长非Rowkey条件的数据过滤，故只这里只介绍如下rowkey相关的过滤器使用：

- RowFilter:

  ```
  //扫描rowkey符合某个正则的数据
  Scan scan = new Scan();
String reg = "^136([0-9]{8})$";//满足136开头的手机号
RowFilter filter = new RowFilter(CompareOp.EQUAL, 
new RegexStringComparator(reg));
scan.setFilter(filter);
  ```