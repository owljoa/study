# kafka 클러스터 구성

1. docker-compose 설치

    https://docs.docker.com/compose/install/

2. 컨테이너 집합 실행

    ```shell
    # foreground로 실행
    docker-compose up
    
    # background로 실행
    docker-compose up -d
    ```

3. 구동중인 컨테이너 확인

    ```shell
    $ docker-compose ps
    NAME                  IMAGE                             COMMAND                       SERVICE       CREATED          STATUS          PORTS
    kafka-kafka-1-1       confluentinc/cp-kafka:7.4.4       "/etc/confluent/docker/run"   kafka-1       32 minutes ago   Up 32 minutes   9092/tcp, 0.0.0.0:29092->29092/tcp
    kafka-kafka-2-1       confluentinc/cp-kafka:7.4.4       "/etc/confluent/docker/run"   kafka-2       32 minutes ago   Up 32 minutes   9092/tcp, 0.0.0.0:39092->39092/tcp
    kafka-zookeeper-1-1   confluentinc/cp-zookeeper:7.4.4   "/etc/confluent/docker/run"   zookeeper-1   32 minutes ago   Up 32 minutes   2888/tcp, 3888/tcp, 0.0.0.0:22181->2181/tcp
    kafka-zookeeper-2-1   confluentinc/cp-zookeeper:7.4.4   "/etc/confluent/docker/run"   zookeeper-2   32 minutes ago   Up 32 minutes   2888/tcp, 3888/tcp, 0.0.0.0:32181->2181/tcp
    ```

<br>

# Kafka CLI

1. 카프카 다운로드

    참고: https://kafka.apache.org/downloads

    ```shell
    curl https://archive.apache.org/dist/kafka/3.7.0/kafka_2.13-3.7.0.tgz -o kafka_2.13-3.7.0.tgz
    ```

2. 압축풀기

    ```shell
    tar xvf kafka_2.13-3.7.0.tgz
    ```


## 토픽 관련 커맨드

- 압축 풀어진 kafka_2.13-3.7.0 디렉토리 접근 (bin 디렉토리 내 실행파일 사용 예정)

```shell

# 토픽 생성
$ ./bin/kafka-topics.sh --create --topic my-topic --bootstrap-server localhost:29092
Created topic my-topic.

# 토픽 파티션 개수 늘리기
$ ./bin/kafka-topics.sh --alter --partitions 7 --topic my-topic --bootstrap-server localhost:29092

# 토픽 목록
$ ./bin/kafka-topics.sh --list --bootstrap-server localhost:29092
my-topic

# 토픽 세부사항
$ ./bin/kafka-topics.sh --describe --topic my-topic --bootstrap-server localhost:29092

Topic: my-topic	TopicId: 3Iv7-_WxS56tuhob4pqFMw	PartitionCount: 7	ReplicationFactor: 1	Configs:
	Topic: my-topic	Partition: 0	Leader: 2	Replicas: 2	Isr: 2
	Topic: my-topic	Partition: 1	Leader: 1	Replicas: 1	Isr: 1
	Topic: my-topic	Partition: 2	Leader: 2	Replicas: 2	Isr: 2
	Topic: my-topic	Partition: 3	Leader: 1	Replicas: 1	Isr: 1
	Topic: my-topic	Partition: 4	Leader: 2	Replicas: 2	Isr: 2
	Topic: my-topic	Partition: 5	Leader: 1	Replicas: 1	Isr: 1
	Topic: my-topic	Partition: 6	Leader: 2	Replicas: 2	Isr: 2

## 프로듀서 & 컨슈머 등록 커맨드

# 토픽 프로듀서
$ ./bin/kafka-console-producer.sh --bootstrap-server localhost:29092 --topic my-topic
> (메시지 입력가능)

# 토픽 컨슈머
$ ./bin/kafka-console-consumer.sh --bootstrap-server localhost:29092 --topic my-topic
(프로듀서에서 메시지 입력 시 해당 메시지 출력됨)


## 컨슈머 그룹 관련 커맨드

# 컨슈머 그룹 목록 (컨슈머가 3개 등록된 상태)
$ ./bin/kafka-consumer-groups.sh --bootstrap-server localhost:29092 --list
console-consumer-27941
console-consumer-21128
console-consumer-41174

# 컨슈머 그룹 세부사항
$ ./bin/kafka-consumer-groups.sh --bootstrap-server localhost:29092 --describe --group console-consumer-27941

GROUP                  TOPIC           PARTITION  CURRENT-OFFSET  LOG-END-OFFSET  LAG             CONSUMER-ID                                           HOST            CLIENT-ID
console-consumer-27941 test-events     6          -               0               -               console-consumer-858a3160-1d05-4907-9760-d1eced65d179 /192.168.65.1   console-consumer
console-consumer-27941 test-events     3          -               0               -               console-consumer-858a3160-1d05-4907-9760-d1eced65d179 /192.168.65.1   console-consumer
console-consumer-27941 test-events     5          -               0               -               console-consumer-858a3160-1d05-4907-9760-d1eced65d179 /192.168.65.1   console-consumer
console-consumer-27941 test-events     2          -               0               -               console-consumer-858a3160-1d05-4907-9760-d1eced65d179 /192.168.65.1   console-consumer
console-consumer-27941 test-events     1          -               0               -               console-consumer-858a3160-1d05-4907-9760-d1eced65d179 /192.168.65.1   console-consumer
console-consumer-27941 test-events     0          -               0               -               console-consumer-858a3160-1d05-4907-9760-d1eced65d179 /192.168.65.1   console-consumer
console-consumer-27941 test-events     4          -               8               -               console-consumer-858a3160-1d05-4907-9760-d1eced65d179 /192.168.65.1   console-consumer


```


