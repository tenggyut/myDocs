##闲谈

###内容概览

1. HBase简单使用手册
2. 函数式编程简介
3. 什么是编程能力

###闲谈开始

####1. HBase简单使用手册

#####常用运维命令

启动/停止HBase集群

```
$HBASE_HOME/bin/start-hbase.sh
$HBASE_HOME/bin/stop-hbase.sh
```

启动/停止HBase的某个节点

```
$HBASE_HOME/bin/hbase-daemon.sh start/stop regionserver
```

清理HBase的znode(清理前，请关闭HBase集群)

```
$ZOOKEEPER_HOME/bin/zkCli.sh

zk> rmr /hbase
```
#####常用HBase Shell命令

获取某张表某个Rowkey的所有列

```
hbase> get '${TABLE_NAME}', '${ROWKEY}'
```

快照

```
hbase> snapshot '${TABLE_NAME}','${SNAPSHOT_NAME}' //打快照
hbase> restore_snapshot '${SNAPSHOT_NAME}' //用快照回滚表数据
hbase> clone_snapshot '${SNAPSHOT_NAME}','${NEW_TABLE_NAME}' //克隆快照

```
#####教训！

不要自己手动安装HBase集群！

不要自己手动安装HBase集群！

不要自己手动安装HBase集群！

然后，请允许我延伸一下。。。

不要自己手动安装集群！

不要自己手动安装集群！

不要自己手动安装集群！

现在市面上比较好流行的集群管理工具：

1. Cloudera的**Cloud Manager**
2. Hortonworks贡献给Apache的**Ambari**

自从有了集群管理工具之后，我的天空 星星都亮了！

![ambari](http://img.blog.csdn.net/20151129142352600?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

####2. 函数式编程简介

#####2.1. 函数是一等公民(First-Class Citizen)

在计算机科学领域，*一等公民*是指没有任何使用限制的编程语言实体。因此，作为一等公民的函数能出现在其他函数的参数列表中(即将函数作为参数进行传递)，也能作为其他函数的返回值。

高阶函数(Higher-Order Function)，在概念上和*一等公民*有想通的部分。其不同之处在于，**高阶函数**指的是数学上已其他函数为参数的函数(例如: g(f(x))中的函数g就是一个高阶函数)，而**一等公民**则是计算机领域中的相关概念。

应用：偏函数(partial application)或叫做柯里化(currying)

#####2.2. 无副作用(No Side Effects)

所谓的副作用，既是变量的状态在经过函数处理之后发生了变化。为了达到**无副作用**的目标，函数式编程中要求数据是不可变的(Immutable Data)。

#####2.3. ReFerential Transparency

*ReFerential Transparency*的含义是指一个函数的输出结果只依赖于输入参数。换句话说，对同一个函数多次传入相同的输入参数所获得的输出是一样一样的。

想想，这对缓存来说是多么大的一个福音啊！

再想想，这对于并行计算来说，简直是天籁啊（参考著名的MapReduce）！

#####2.4. 支持函数式编程的语言

纯函数式编程语言：Haskell

混合式：F#、Scala、Lisp、Java8、JavaScript、R、MATLAB等

####3. 什么是真正的编程能力（https://www.zhihu.com/question/31034164）

这个问题是我在知乎上闲逛时偶然发现的。很有意思的一个话题，大家可以去翻翻看，也许能找到一些共鸣。

####4. 工具

1. MarkDown
2. 科学上网（ss，vpn...）
3. Launchy/FindAndRunRobot

