kafka是什么？

分布式流平台

我们认为，一个流处理平台具有三个关键能力：

发布和订阅消息（流），在这方面，它类似于一个消息队列或企业消息系统。
以容错的方式存储消息（流）。
在消息流发生时处理它们。



什么是kakfa的优势？
它应用于2大类应用：

构建实时的流数据管道，可靠地获取系统和应用程序之间的数据。
构建实时流的应用程序，对数据流进行转换或反应。


kafka有四个核心API：
应用程序使用 Producer API 发布消息到1个或多个topic（主题）。
应用程序使用 Consumer API 来订阅一个或多个topic，并处理产生的消息。
应用程序使用 Streams API 充当一个流处理器，从1个或多个topic消费输入流，并生产一个输出流到1个或多个输出topic，有效地将输入流转换到输出流。
Connector API允许构建或运行可重复使用的生产者或消费者，将topic连接到现有的应用程序或数据系统。例如，一个关系数据库的连接器可捕获每一个变化。

图1



首先来了解一下Kafka所使用的基本术语：
Topic
Kafka将消息种子(Feed)分门别类，每一类的消息称之为一个主题(Topic).

Producer
发布消息的对象称之为主题生产者(Kafka topic producer)

Consumer
订阅消息并处理发布的消息的种子的对象称之为主题消费者(consumers)

Broker
已发布的消息保存在一组服务器中，称之为Kafka集群。集群中的每一个服务器都是一个代理(Broker). 消费者可以订阅一个或多个主题（topic），并从Broker拉数据，从而消费这些已发布的消息。

