可重试异常：
REQUEST_TIMED_OUT : The request timed out. 
NETWORK_EXCEPTION ： The server disconnected before a response was received.

不可重试：
BROKER_NOT_AVAILABLE ： The broker is not available.
REPLICA_NOT_AVAILABLE ： The replica is not available for the requested topic-partition
MESSAGE_TOO_LARGE ： The request included a message larger than the max message size the server will accept.
INVALID_TOPIC_EXCEPTION ： The request attempted to perform an operation on an invalid topic.


kafka::http://kafka.apache.org/quickstart
启动：kafka-server-start config/server.properties   kafka-server-start ../../config/server.properties

（kafka_2.10-0.10.0.1\bin\windows目录下执行）启动命令，先启动zk，再启动kafka
zookeeper : zookeeper-server-start.bat ../../config/zookeeper.properties
kafka :     kafka-server-start.bat ../../config/server.properties
jaeger-all-in-one.exe --collector.zipkin.http-port=9411
kafka-server-start.bat ../../config/server1.properties
kafka-server-start.bat ../../config/server2.properties

创建topic:kafka-topics --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test
查看topic:kafka-topics --list --zookeeper localhost:2181
查看topic详情:kafka-topics.bat --describe --zookeeper localhost:2181 --topic my-replicated-topic

发送消息:kafka-console-producer --broker-list localhost:9092 --topic test
消费消息:kafka-console-consumer.sh --bootstrap-server localhost:9092 --zookeeper localhost:2181 --topic  test --from-beginning

bin/kafka-console-consumer.sh --bootstrap-server commsrvkafka:4531 --topic  zenap_zenap_license_update --from-beginning

kafka-topics.bat --list --zookeeper localhost:2181
bin/kafka-console-consumer.sh --bootstrap-server commsrvkafka:4531 --topic zenap_zenap_license_update --from-beginning

查看ProcessId命令 :  wmic process where "caption = 'java.exe' and commandline like '%server-1.properties%'" get processid
kill pid : taskkill /pid 6016 /f



Adding and removing topics:

bin/kafka-topics.sh --zookeeper zk_host:port/chroot --create --topic my_topic_name
      --partitions 20 --replication-factor 3 --config x=y
	  
bin/kafka-topics.sh --zookeeper zk_host:port/chroot --alter --topic my_topic_name
      --partitions 40
	  
bin/kafka-configs.sh --zookeeper zk_host:port/chroot --entity-type topics --entity-name my_topic_name --alter --add-config x=y

bin/kafka-configs.sh --zookeeper zk_host:port/chroot --entity-type topics --entity-name my_topic_name --alter --delete-config x

bin/kafka-topics.sh --zookeeper zk_host:port/chroot --delete --topic my_topic_name


Checking consumer position:

kafka-consumer-groups --bootstrap-server localhost:9092 --describe --group my-group
bin/kafka-consumer-groups.sh --zookeeper localhost:2181 --describe --group my-group

bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --list
bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --describe --group test-consumer-group
bin/kafka-consumer-groups.sh --zookeeper localhost:2181 --list


kafka-run-class.bat kafka.tools.DumpLogSegments

kafka-run-class.bat kafka.tools.ConsumerOffsetChecker  --group=test --zookeeper=127.0.0.1:2181 --topic=kafkaDemo-00
kafka-run-class.bat kafka.tools.DumpLogSegments --files=D:\Home\Logs\kafka\logs-0\kafkaDemo-00-1\00000000000000000000.log

pdm-cli node list  --->  com_srv_share,    |   net_api: 192.168.30.172
docker ps|grep kafka
docker exec -ti bc57d589cf0b /bin/sh
查看消费者消费情况
./kafka-topics.sh --list --zookeeper localhost:4532
./kafka-topics.sh --describe --zookeeper localhost:2181 --topic my-replicated-topic

./kafka-topics.bat --zookeeper localhost:2181  --describe --topic streams-input-0
./kafka-consumer-groups.sh  --bootstrap-server  localhost:4531 --describe --group fm-base-message
./kafka-console-consumer.sh --bootstrap-server  localhost:4531 --topic FM_EMF_UP_TOPIC

kafka-consumer-groups.bat --bootstrap-server localhost:9092 --describe --group group00


kafka-run-class.bat kafka.tools.DumpLogSegments --files D:\Home\Logs\kafka\kafkaSimple-0\00000000000000000000.log  --print-data-log --deep-iteration


windows

kafka-topics.bat --list --zookeeper localhost:2181
kafka-console-producer.bat --broker-list localhost:9092 --topic TOPIC00
kafka-console-consumer.bat --bootstrap-server localhost:9092  --topic TOPIC00 --from-beginning
kafka-consumer-groups.bat --bootstrap-server localhost:9092 --describe --group streams
kafka-topics.bat --describe --zookeeper localhost:2181 --topic my-replicated-topic
kafka-topics.bat --zookeeper localhost:2181 --alter --topic streams-plaintext-input --partitions 10
kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 10 --topic streams-plaintext-input-multi

paas

bin/kafka-topics.sh --list --zookeeper 194.167.0.9:4532
bin/kafka-console-producer.sh --broker-list 194.167.0.9:4531 --topic resource.change
bin/kafka-console-consumer.sh --bootstrap-server 194.167.0.9:4531 --topic resource.change --from-beginning
bin/kafka-consumer-groups.sh --bootstrap-server 194.167.0.9:4531 --describe --group mml_groupId
./kafka-topics.sh --describe --zookeeper localhost:2181 --topic my-replicated-topic
kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 10 --topic streams-plaintext-input-multi
bin/kafka-consumer-groups.sh --new-consumer --bootstrap-server 194.167.0.9:4531 --list

bash-4.4# bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test1 --config retention.ms=6000
bash-4.4# bin/kafka-topics.sh --describe --zookeeper localhost:2181 --topic test1
Topic:test1     PartitionCount:1        ReplicationFactor:1     Configs:retention.ms=6000
        Topic: test1    Partition: 0    Leader: 0       Replicas: 0     Isr: 0

		


 tcpdump  host 192.167.167.5 and port 9092 -w 11kafka1.cap
 
 