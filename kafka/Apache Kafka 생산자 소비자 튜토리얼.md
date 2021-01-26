# Apache Kafka 생산자 소비자 CLI 튜토리얼

![](https://i1.wp.com/cloud.githubusercontent.com/assets/40263/15908479/f04cc88c-2d8f-11e6-8f5e-208bc663e180.png?ssl=1)

이번 시간에는 Docker를 활용하여 카프카를 실행하고 CLI 환경에서 토픽을 생성하고, 생산자 및 소비자 환경을 구축하여 메시지 전달 방식에 대해 실습한다. 본 실습은 confluent에서 제공하는 기본 튜토리얼을 바탕으로 진행하였으며, Docker 기반으로 실행해야 하므로 본인이 사용하고 있는 환경에 Docker 설치가 완료되어 있어야 한다.

만약 Docker 설치가 어렵거나 네이티브로 실행해야 하는 경우 이전시간에 윈도우, 우분투를 기반으로 설치방법을 기술하였으니 전 챕터를 확인하길 바란다.

## 카프카 설치

-   필자 환경은 다음과 같다.
    -   MacBook Pro 2019 16인치 Intel Core i9, 32GB
    -   macOS Big Sur 11.1
    -   Docker version 20.10.0-rc1, build 5cc2396
    -   docker-compose version 1.27.4, build 40524192

### Docker 카프카 기본 설치

디렉터리를 생성하고 docker-compose.yml을 작성하고 실행한다.

```
$ mkdir console-consumer-producer-basic && cd console-consumer-producer-basic
```

```
---
version: '2'

services:
  zookeeper:
    image: confluentinc/cp-zookeeper:6.0.0
    hostname: zookeeper
    container_name: zookeeper
    ports:
      - "2181:2181"
    environment:
      ZOOKEEPER_CLIENT_PORT: 2181
      ZOOKEEPER_TICK_TIME: 2000

  broker:
    image: confluentinc/cp-kafka:6.0.0
    hostname: broker
    container_name: broker
    depends_on:
      - zookeeper
    ports:
      - "29092:29092"
    environment:
      KAFKA_BROKER_ID: 1
      KAFKA_ZOOKEEPER_CONNECT: 'zookeeper:2181'
      KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: PLAINTEXT:PLAINTEXT,PLAINTEXT_HOST:PLAINTEXT
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://broker:9092,PLAINTEXT_HOST://localhost:29092
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
      KAFKA_GROUP_INITIAL_REBALANCE_DELAY_MS: 0

```

```
$ docker-compose up -d && docker-compose logs -f
```

단일 노드에서 카프카를 실행할 수 있도록 카프카와 주키퍼 설치를 완료했다.

### Topic 생성하기

생산자가 소비자에게 데이터를 전송하기 위해서는 토픽을 생성해야 한다. 다음 과정을 실행한다.

```
$ docker-compose exec broker kafka-topics --create --topic example-topic --bootstrap-server broker:9092 --replication-factor 1 --partitions 1
```

### 소비자 준비하기

터미널을 생성하고 생산자로부터 생성된 데이터를 받기 위한 소비자를 다음과 같이 CLI 기반으로 구성한다.

```
$ docker-compose exec broker bash
$ kafka-console-consumer --topic example-topic --bootstrap-server broker:9092 
```

### 생산자 준비하기

터미널을 생성하고 소비자에게 데이터를 전송하기 위한 생산자를 다음과 같이 CLI 기반으로 구성한다.

```
$ docker-compose exec broker bash
$ kafka-console-producer --topic example-topic --broker-list broker:9092
```

데이터를 보내기 위한 프롬프트가 생성되면 다음과 같이 데이터를 전송한다.

```
lucas
wisoft
hanbat
tistory blog kafka basic tutorial
```

CTRL+C를 통해 데이터 전송을 중단할 수 있다.

### 소비자 확인

생산자에게 데이터를 작성하고 소비자 터미널을 확인하면 생산자에게 전송한 데이터와 동일한 값이 출력되는 것을 확인할 수 있다.

### 생산자에서 전송한 모든 데이터 확인하기

일반적으로 생산한 데이터는 소비자가 즉각적으로 확인해야 하지만, 소비자가 준비되지 않은 상태에서 생산자 데이터가 생성된 경우 기존에 데이터를 읽을 수 없다. 하지만 카프카는 기본 7일간의 데이터를 파일 형태로 저장하기 때문에 소비자가 원할 경우 데이터를 다시 가져올 수 있다는 특징이 있다.

소비자 터미널을 종료한 상태에서 --from-beginning 명령을 추가로 입력하게 되면 지금까지 생산한 모든 데이터를 확인할 수 있다. 또한 다른 소비자에서 같은 토픽으로 데이터를 수집받는 경우에도 동일한 결과를 얻을 수 있다.

```
$ kafka-console-consumer --topic example-topic --bootstrap-server broker:9092  --from-beginning 
```

```
lucas
wisoft
hanbat
tistory blog kafka basic tutorial
```

### key-value 값으로 데이터 생성하기

카프카는 key-value 형식을 통해 데이터를 전송할 수 있다. 단, 키 값이 없는 상태에서 데이터를 전송한 경우 null이 된다. key-value 형식으로 데이터를 보내기 위해서는 CLI 환경에서 추가적인 환경 설정이 필요하다.

parse.keys와 key.separator를 통해서 key-value 데이터가 전송됨을 알리고, 구분자를 지정할 수 있다. 다음과 같은 명령을 수행한다.

```
$ kafka-console-producer --topic example-topic --broker-list broker:9092\
  --property parse.key=true\
  --property key.separator=":"
```

```
key1:what a lovely
key1:bunch of coconuts
foo:bar
fun:hello kafka
```

### 소비자에서 key-value 값 확인하기

생산자에서 key-value 형식으로 데이터를 전송한 경우, 소비자 역시 동일한 형식으로 데이터를 전송받을 수 있다. 해당 토픽으로부터 처음부터 마지막까지 전송된 데이터를 모두 확인하기 위해 다음과 같은 명령을 수행한다.

```
$ kafka-console-consumer --topic example-topic --bootstrap-server broker:9092 \
 --from-beginning \
 --property print.key=true \
 --property key.separator="-"
```

```
null-lucas
null-wisoft
null-hanbat
null-tistory blog kafka basic tutorial
key1-what a lovely
key1-bunch of coconuts
foo-bar
fun-hello kafka
```

지금까지 생성된 데이터를 확인할 때, 확인할 수 있는 것처럼 key 값이 없는 경우 null이 되며, - 구분자를 통해 value 값이 표기되는 것을 확인할 수 있다.

### 실습 마무리하기

지금까지 기본적으로 생산자와 소비자 간의 데이터 전송을 CLI 환경에서 실습을 진행했다. 실행환경을 정리하기 위해서 다음과 같은 명령을 입력한다.

```
$ docker-compose down
```