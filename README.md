# Kafka-Spark Cluster

## 위치 설명

#### kafka/cluster : kafka cluster를 Docker로 구성. 
#### cluster : cluster 를 analysis하는 코드. 

## Docker 사용법 

#### 1. docker network setting

docker network create --gateway 172.28.5.254 --subnet 172.28.0.0/16 base_2

execute network.sh

#### 2. build Dockerfile using build.sh
execute build.sh

#### 3. make data1~3 directory
select 3 directory in host
and change host mount directory in run script

#### 4. run script
execute run1.sh run2.sh run3.sh

#### 5. execute zookeeper_start.sh in each container

#### 6. execute kafka_start.sh in each container

## Spark 클러스터 구성 방법

1) 현재 컨테이너 3개를 zookeeper로 연동해서 띄워놓은 상태입니다.
2) configuration(partition number, consumer group. etc.)를 설정해서 topic을 생성합니다.
3) 생성된 topic으로 kafka producer로 data를 ingest합니다
4) consumer코드로 data를 받아와서 처리합니다(spark로 넘겨줄수도 있고, database로 저장할 수도 있고)
5) hadoop yarn기반의 resource manager를 통해 현재 3개의 노드에서 cluster모드로 설정해놓았습니다.
6) Bigcomp는 Spark MLlib기반의 코드로 data processing을 하고 Elephas 라이브러리로 연동하여 딥러닝 모델 학습
