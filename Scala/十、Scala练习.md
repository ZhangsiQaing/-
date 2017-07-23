## Scala统计单词个数
```
val lines= List(
      "hadoop,spark,hbase,hive"
      "hadoop,spark,hbase,hive",
      "",
      "spark,hive,spark,spark',hdfs,wordCount"
    )

lines
      .filter(_.nonEmpty)    
      .flatMap(_.split(","))
      .filter(_.trim.nonEmpty)
      .map(word => (word.trim,1))
      .groupBy(_._1)
      .map{
        case (word,iter) => {
           val sum = iter.map(_._2).sum
          (word,sum)
        }
      }
      .foreach(println)
```