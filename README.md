# HelloFlink
Project of HelloFlink for ByteDance

1.整体架构介绍：
Flink的开发我们都按照标准的“Source -> Transformation -> Sink”的有向无环图DAG格式进行设计和开发。其中Source用了多种形式进行实现，比如说预定义的
fromElements，自己撰写了一个可以源源不断产生数据的类，Socket，Kafka等；Transformation使用了flatMap、Map、keyBy、sum、reduce等多种算子，同时设置了滚动窗口，将全量和增量窗口结合进行使用，并设置了Watermarks处理延迟乱序数据；Sink同时开发了输出到控制台以及输出到.txt两种输出方式。
总体来看，整个开发的架构还是很有层次的，而且代码处都有相应的注释。

2.各模块介绍：
pom.xml：添加相应的依赖
log4j.properties：日志管理
TimeUtil：时间格式转换类，包含Date类型转String类型、Long类型转String类型以及String类型转Date类型共三种应用场景
WordOrder：构造一个类存放输入的数据时间、单词以及个数
WordOrderSource：构造一个类产生源源不断随机产生输入数据
WordCount：统计5mins内每个单词的数量，我这边做了一个改进：就是不局限于一条记录一个单词只产生一次，而是让用户自己定义单词的数量。使用增量和全量窗口相结合的方式，在节省资源提高效率的同时还返回了窗口的时间信息
WordCount_Watermark：设置Watermarks机制处理延迟乱序数据，其中设置窗口大小为10s，延迟时间为3s
WordCount_AllowedLateness_SideOutput：在Watermarks的基础上设置allowedLateness二次兜底延迟数据处理和SideOutput兜底延迟数据处理，二次兜底时间为1min
 Exactly_Once：写好了Exactly Once Checkpoint的相关配置，但相应的测试案例还没设计好

备注：
1.Kafka作为Source的代码我也写好了，但是由于其更适合用于Linux上进行测试，因此我在WordCount小程序中使用自己定义的类源源不断地产生数据流代替进行测试，然后Watermarks那里采用Socket和nc方法代替进行测试，效果都是一样的
2."testDemo"是一个实现字符串切割的flink测试demo,与本项目无关

3.程序部署启动
由于时间紧张目前还没有打包成jar包，后续有时间会做这一步。目前是把java的项目文件打包成了压缩包，老师解压后用IDEA打开项目就可以执行app里的各个程序了

4.验证测试
该项目共包含WordCount小程序、Watermarks处理延迟无序数据、AllowedLateness & SideOutput机制三个测试，具体的测试用例详见汇报文档，具体的测试操作详见录制的视频。

谢谢老师们的审阅和指导，欢迎提出改进意见！

