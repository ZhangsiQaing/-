搭建环境：CentOS6.4
搭建压缩包：spark-1.6.1.tar.gz、scala-2.10.4.tgz、repository-1.6.1.tar.gz、zinc-0.3.5.3.tgz
搭建目录：/opt/cdh5

建立本地maven仓库
```
cd ~
mkdir .m2
cd .m2
tar -zxvf repository-1.6.1.tar.gz
```

解压spark-1.6.1.tar.gz到/opt/cdh5目录下
```
tar -zxvf spark-1.6.1.tar.gz -C /opt/cdh5
cd spark-1.6.1
```

## 首先编译cdh5版本的spark
复制一个目录用于编译cdh5版本的spark
```
cp -R spark-1.6.1 spark-1.6.1-cdh5
```

进入spark-1.6.1-cdh5目录,修改make-distribution.sh内容
```
添加以下内容：
VERSION=1.6.1
SCALA_VERSION=2.10.4                   # 根据scala的版本来配置
SPARK_HADOOP_VERSION=2.5.0-cdh5.3.6    # 根据hadoop的版本来配置
SPARK_HIVE=1   # 1表示需要将hive的打包进去，非1数字表示不打包hive

并且将脚本中的原来配置注释
```

修改pom.xml文件
```
vim pom.xml

#将scala.version修改为本地scala版本的环境
<scala.version>2.10.4</scala.version>
<scala.binary.version>2.10</scala.binary.version>
```

进入build目录，解压zinc-0.3.5.3.tgz、scala-2.10.4.tgz
```
cd build
tar -zxvf /opt/tools/zinc-0.3.5.3.tgz
tar -zxvf /opt/tools/scala-2.10.4.tgz
```

进入/opt/cdh5/spark-1.6.1-cdh5.3.6/build/apache-maven-3.3.3/conf目录，修改settings.xml,
```
<mirror>
<id>aliyun</id>
<mirrorOf>central</mirrorOf>
<name>aliyun repository</name>
<url>http://maven.aliyun.com/nexus/content/groups/public/</url>
</mirror>
```


回到spark-1.6.1-cdh5目录,执行编译操作
```
./make-distribution.sh --tgz \
	-Phadoop-2.4 \
	-Dhadoop.version=2.5.0-cdh5.3.6 \
	-Pyarn \
	-Phive -Phive-thriftserver
```

编译成功后会在/opt/cdh5/spark-1.6.1-cdh5.3.6目录生成一个spark-1.6.1-bin-2.5.0-cdh5.3.6.tgz压缩包


