Spark local on linux(spark shell命令)

## Spark 1.x环境搭建步骤
* 安装JDK（建议JDK 7以上）
* 安装Scala（2.10.4）
* 安装Hadoop 2.x（至少HDFS）
* 安装Spark Local

安装JDK1.7
```
待补充
```

安装Scala2.10.4
```
 解压：
 tar xvf scala-2.10.3.tgz

 添加环境变量：
 vi /etc/prifile
 添加如下参数：
     export SCALA_HOME=/opt/moduels/scala-2.10.4
     export PATH=$PATH:$SCALA_HOME/bin
使环境变量生效：
     source /etc/profile
```

安装Hadoop  2.x
```
待补充
```

## 安装Spark Local
解压编译好的spark压缩包
```
tar -zxvf spark-1.6.1-bin-2.5.0-cdh5.3.6.tgz -C /opt/cdh5/
```

重命名spark目录
```
mv spark-1.6.1-bin-2.5.0-cdh5.3.6/ spark-1.6.1
```

进入spark根目录，修改conf中的配置文件
```
cd spark-1.6.1
cp conf/spark-env.sh.template conf/spark-env.sh
```

修改spark-env.sh
```
vim spark-env.sh

# 添加以下内容
JAVA_HOME=/opt/moduels/jdk1.7.0_67/        #设置Java路径
SCALA_HOME=/opt/moduels/scala-2.10.4       #设置Scala路径
HADOOP_CONF_DIR=/opt/cdh5/hadoop-2.5.0-cdh5.3.6/etc/hadoop    #设置hadoop配置文件路径
SPARK_LOCAL_IP=bigdata-senior01.ibeifeng.com   #设置hadoop的ip
```
* 上面四个配置项中，除了HADOOP_CONF_DIR外，其它的必须给定；HADOOP_CONF_DIR给定的主要功能是：给定连接HDFS的相关参数(实际上本地运行的时候，只需要给定core-site.xml和hdfs-site.xml)

测试spark的本地环境
```
启动HDFS
./bin/run-example SparkPi 10
./bin/spark-shell
```

