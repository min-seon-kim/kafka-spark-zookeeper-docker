# Kafka-Spark 분산 처리 클러스터

## 위치 설명

docker/base : kafka cluster를 Dockerfile로 구성. 

cluster : cluster 를 analysis하는 코드.


## 전체 클러스터 구성
<img width="477" alt="image" src="https://github.com/sperospera1225/kafka-spark-zookeeper-docker/assets/67995592/cdbe66c8-0247-4828-ac03-b19dff53f84e">

* 구성환 환경에서는 별도의 zookeeper를 사용하고 port를 3개 모두 통일시켰다.


## Docker 환경 구성 방법
### 1. docker network setting
```
docker network create --gateway 172.28.5.254 --subnet 172.28.0.0/16 base_2
execute network.sh
```
### 2. build Dockerfile using build.sh
```
execute build.sh
```
### 3. make data1~3 directory
```
select 3 directory in host
and change host mount directory in run script
```
### 4. run script
```
execute run1.sh run2.sh run3.sh
```
### 5. execute zookeeper_start.sh in each container

### 6. execute kafka_start.sh in each container

## Spark 클러스터 구성 설명

1) Kafka에서 ingestion한 topic의 consumer코드로 data를 받아옵니다.
2) hadoop yarn기반의 resource manager를 통해 현재 3개의 노드에서 spark-cluster모드로 설정해놓았습니다.
3) Spark MLlib기반의 코드로 data processing을 하고 Elephas 라이브러리로 연동하여 딥러닝 모델을 학습합니다.
4) 분류된 데이터를 MongoDB에 Spark-Mongo Connector를 이용하여 실시간으로 저장합니다.

## Kafka 기본 명령어 목록

```
1. 토픽 리스트 확인.
bin/kafka-topics.sh --list --zookeeper kafka1:2181,kafka2:2181,kafka3:2181/twitter  

# console producer 실행
bin/kafka-console-producer.sh --broker-list localhost:9092 --topic topicname

# console consumer 실행
bin/kafka-console-consumber.sh –bootsrap-server localhost:9092 –topic topicname

# topic 생성
bin/kafka-topics.sh --create --zookeeper kafka1:2181,kafka2:2181,kafka3:2181/twitter --replication-factor 3 --partitions 1 --topic connect-configs
```

## Spark 기본 명령어 목록 

1. Spark 시작
진행 노드 : master
```
$ ~/spark/sbin/start-all.sh
$ ~/spark/sbin/start-history-server.sh
```
3. Spark 종료
진행 노드 : master
```
$ ~/spark/sbin/stop-all.sh
$ ~/spark/sbin/stop-history-server.sh
$ rm -rf ~/spark/evevntLog/*
```
