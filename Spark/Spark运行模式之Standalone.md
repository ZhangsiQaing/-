## Spark on Standalone
  将spark应用运行在standalone上
  Standalone是一个Spark自带的资源管理框架，功能类似yarn
  * Yarn的框架:
    * ResourceManager：管理集群的资源，包括：监控、申请...
    * NondManager: 管理当前节点的资源以及启动container中的task任务
    资源：
      CPU&内存
  * Standalone的框架：
    * Master：集群资源管理，包括：监控、申请...
    * Worker: 任务执行节点的资源管理，包括资源管理以及executor启动
    资源：
      CPU&内存
   Standalone的配置：

    -1. 前提要求：Spark的local本地模式可以成功运行<br>
    -2. 修改${SPARK_HOME}/conf中的配置文件
    ```
    vim spark-env.sh
    SPARK_MASTER_IP=hadoop-senior01.ibeifeng.com
    SPARK_MASTER_PORT=7070
    SPARK_MASTER_WEBUI_PORT=8080
    SPARK_WORKER_CORES=2  ## 一个worker服务中可以包含多少核的CPU，逻辑上的
    SPARK_WORKER_MEMORY=2g  ## 一个worker服务中可以包含多少内存，逻辑上的
    SPARK_WORKER_PORT=7071
    SPARK_WORKER_WEBUI_PORT=8081
    SPARK_WORKER_INSTANCES=2 ## 指定一台服务器可以启动多少个work服务

    mv slaves.template slaves
    vim slaves
    hadoop-senior01.ibeifeng.com ## 给定work服务所在机器的IP地址或者主机名，一行一个
    ```
    
    -3. 启动服务
    ```
       ./sbin/start-master.sh
       ./sbin/start-slaves.sh
       ===> ./sbin/start-all.sh
       （关闭服务使用: ./sbin/stop-all.sh）
    ```
    -4. 测试
    ```
       jps： 能够看到master和worker服务
       webui: http://hadoop-senior01:8080/
       spark-shell测试：
         bin/spark-shell --master spark://bigdata-senior01.ibeifeng.com/:7070
        val lines = sc.textFile("/zsq/spark/data/word.txt")
        val words = lines.flatMap(line => line.split(" "))
        val filteredWrods = words.filter(word => word.trim.nonEmpty)
        val wordAndNums = filteredWrods.map(word => (word.trim, 1))
        val result = wordAndNums.reduceByKey((a, b) => a + b)
        result.take(10)
    ```

## Spark运行模式Standalone Master(HA)
  http://spark.apache.org/docs/1.6.1/spark-standalone.html#high-availability
  -1. Single-Node Recovery with Local File System
    基于本地文件系统的Master恢复机制
    修改${SPARK_HOME}/conf/spark-env.sh
SPARK_DAEMON_JAVA_OPTS="-Dspark.deploy.recoveryMode=FILESYSTEM -Dspark.deploy.recoveryDirectory=/tmp"
  -2. Standby Masters with ZooKeeper
    基于zk提供热备(HA)的master机制
    修改${SPARK_HOME}/conf/spark-env.sh
SPARK_DAEMON_JAVA_OPTS="-Dspark.deploy.recoveryMode=ZOOKEEPER -Dspark.deploy.zookeeper.url=hadoop-senior01:2181,hadoop-senior02:2181,hadoop-senior03:2181 -Dspark.deploy.zookeeper.dir=/spark"


## 应用监控
  * 1. 运维人员会使用专门的运维工具进行监控，eg: zabbix等，（网络、磁盘、内存、cpu、进程等）
  * 2. 使用CM(CDH)、Ambari(Apache、HDP)大数据专门的运维管理工具
  * 3. 通过软件自带的web页面进行监控，eg: spark 8080、 hdfs 50070、mapreduce job history 19888....
  * 4. 通过oozie等调度工具进行监控(当job执行失败的时候可以通知开发人员，通过email、短信)
  * 5. Linux上的进程好像是可以自动恢复的（supervise方式启动）


## Spark应用监控：
  http://spark.apache.org/docs/1.6.1/monitoring.html
  * 1. 针对正在运行的应用，可以通过webui来查看，默认端口号是4040，当4040被占用的时候，端口号往上递推.
  * 2. 针对于已经执行完的job/应用，可以通过spark的job history server服务来查看

MapReduce Job History服务：
```
  -1. 开启日志聚集功能
  -2. 给定日志上传的hdfs文件夹
  -3. 启动mr的job history服务(读取hdfs上的文件内容，然后进行展示操作)
```

Spark Job History服务：
```
  -1. 创建HDFS上用于存储spark应用执行日志的文件夹
    hdfs dfs -mkdir -p /spark/history
  -2. 修改配置文件(开启日志聚集功能)
    mv spark-defaults.conf.template spark-defaults.conf
    vim spark-defaults.conf
spark.eventLog.enabled           true
spark.eventLog.dir               hdfs://hadoop-senior01.ibeifeng.com:8020/spark/history
   -3. 测试spark的应用是否会将执行的日志数据上传到HDFS上
     ./bin/spark-shell
   -4. 配置spark的job history相关参数(指定从哪儿读取数据)
     vim spark-env.sh
SPARK_HISTORY_OPTS="-Dspark.history.fs.logDirectory=hdfs://hadoop-senior01.ibeifeng.com:8020/spark/history"
   -5. 启动history 服务
      sbin/start-history-server.sh
      关闭：sbin/stop-history-server.sh
   -6. 测试
      访问webui: http://hadoop-senior01:18080/
```


Spark Job History Rest API:
   http://<server-url>:18080/api/v1 
   eg：
     http://hadoop-senior01:18080/api/v1/applicationshttp://hadoop-senior01:18080/api/v1/applications/local-1498985874725/logs 