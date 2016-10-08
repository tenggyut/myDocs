##Alluxio安装及与HBASE的整合

###安装

目前我们用的是Hadoop 2.6.0，所以需要自己从源码编译Alluxio

```
git clone https://github.com/Alluxio/alluxio.git
```

use master branch because of [this issue](https://alluxio.atlassian.net/browse/ALLUXIO-2025)

```
cd alluxio
mvn clean package -DskipTests -Phadoop2.6
```

Alluxio的发行包(distrubition)很不成熟，没有一个标准的发行包结构(bin,conf,lib...)。执行完上述命令后的结果就是它的发行包

###初始化及启动

初始化配置

```
bin/alluxio bootstrap-conf <ALLUXIO_MASTER_HOSTNAME> hdfs
```
**注意：**执行完这个命令后，需要手动修改$ALLUXIO_HOME/conf/alluxio-env.sh。因为它默认hdfs的url是`localhost:9000`，请手动修改为实际url

格式化文件系统

```
$ALLUXIO_HOME/bin/alluxio format
```

在启动master和worker

```
$ALLUXIO_HOME/bin/alluxio-start.sh master //启动master
$ALLUXIO_HOME/bin/alluxio-start.sh worker //启动worker
```

###查看WEB-UI

可以访问master的[WEB-UI](http://localhost:19999)

###问题

- `$ALLUXIO_HOME/libexec/alluxion-config.sh`中有对`JAVA_HOME`的设置，但有问题，目前想到的解决方案是手动修改为实际的`JAVA_HOME`路径
- 用`$ALLUXIO_HOME/bin/alluxio-start.sh workers`启动所有worker节点是无效的，应该是远程执行bash命令的写法有问题。目前的解决方案是到每个worker节点手动执行`$ALLUXIO_HOME/bin/alluxio-start.sh worker`

###与HBASE的整合

在`hbase-site.xml`中添加如下一段配置:

```
<property>
    <name>fs.alluxio.impl</name>
    <value>alluxio.hadoop.FileSystem</value>
</property>
```
将`hbase.rootdir`修改为如下值:

```
<property>
    <name>hbase.rootdir</name>
    <value>alluxio://<ALLUXIO_MASTER_HOSTNAME>:19998/hbase</value>
</property>
```

分发`hbase-site.xml`到各节点，然后重启HBase

###HBASE和Alluxio整合后的性能测试

- YCSB

  - 半读半写：29000+ops(with alluxio) vs 12000+ops(without alluxio)

- Phoenix的performance.py:
  
  1000W量级
  - sql1: 3.631 s vs 11.576 s
  - sql2: 2.221 s vs 10.412 s
  - sql3: 3.023 s vs 11.023 s
  - sql4: 4.441 s vs 12.249 s
  - sql5: 4.307 s vs 12.99 s
  
###问题
但一旦hbase表开始split，则Region Server会报错并直接退出。目前正在论坛上咨询，看有解决方案没。