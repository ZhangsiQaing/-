## Spark的运行模式（Spark应用在哪儿运行）
* local：本地运行
* standalone:使用Spark自带一个资源管理框架，将spark应用可以提交到该资源管理框架上运行（分布式的）
* yarn:将spark应用提交到yarn上进行运行
* mesos:类似yarn的一种资源管理框架