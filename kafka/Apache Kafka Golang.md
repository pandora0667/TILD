# Apache Kafka Golang 생산자 소비자 튜토리얼 

**본 문서는 Kafka 공식 문서 및 GitHub을 기준으로 작성되었음을 알린다.**

![](https://axual.com/wp-content/uploads/2020/07/Kafka-Performance-Metrics-To-Monitor-1.jpg)

Apache Kafka producer와 consumer를 구현할 때 사용하는 다양한 언어기반의 클라이언트를 제공한다. 지원하는 언어는 다음과 같다.

-   Java
-   Ruby
-   Python
-   C
-   C++
-   Go

본 글에서는 Go언어를 기반으로 Kafka Example을 작성하는 방법을 알아보도록 한다.

## Kafka docker-compose.yml

애플리케이션을 실행하기 전에 Apache Kafka를 docker-compose를 통해 실행한다.

```bash
$ git clone https://github.com/confluentinc/cp-docker-images.git
$ cd cp-docker-images/examples/kafka-single-node
$ docker-compose up -d 
```

## Consumer

Kafka에서는 consumer 구현을 위한 두 가지의 API를 제공한다. 세부적인 것들이 모두 추상화되어 있어서 몇 번의 간단한 함수 호출로 구현할 수 있는 [High Level Consumer API](http://kafka.apache.org/081/documentation.html#highlevelconsumerapi), offset과 같은 세부적인 부분까지 다룰 수 있으나 구현하기가 까다로운 [Simple Consumer API](http://kafka.apache.org/081/documentation.html#simpleconsumerapi)가 제공된다.

본 예제에서는 간단하게 데이터를 수신받는 기능만 구현할 것이기 때문에 High Level Consumer API를 사용한다.

```go
package Config

import (
    "gopkg.in/confluentinc/confluent-kafka-go.v1/kafka"
)

func Kafka() *kafka.Consumer {


    c, err := kafka.NewConsumer(&kafka.ConfigMap{
        "bootstrap.servers": "localhost",
        "group.id":          "myGroup",
        "auto.offset.reset": "earliest",
    })

    if err != nil {
        panic(err)
    }

    return c
}
```

`kafka.NewConsumer` 부분에서 kafka 접속정보를 입력한다. 접속 에러가 발생할 경우 `panic` 발생하여 시스템을 종료한다.

```go
package Basic

import (

    "fmt"
    "kafka-comsumer/Config"
)

func Consumer()  {

    c := Config.Kafka()

    c.SubscribeTopics([]string{"weather"}, nil)
    defer c.Close()

    for {
        msg, err := c.ReadMessage(-1)
        if err == nil {
            fmt.Printf("Message on %s: %s\n", msg.TopicPartition, string(msg.Value))
        } else {
            // The client will automatically try to recover from all errors.
            fmt.Printf("Consumer error: %v (%v)\n", err, msg)
        }
    }
}
```

`c.SubscribeTopics` 부분에서 수신받을 Topic을 지정한다. 지정된 Topic으로 부터 메시지가 수신되면 파티션으로부터 메시지를 꺼내서 출력한다. 모든 수신이 완료되면 최종적으로 `defer c.Close()` 을 통해서 접속을 종료한다.

```go
package main

import (
    "fmt"
    "kafka-comsumer/Basic"
)

func main() {

    fmt.Println("Kafka Consumer Example")
    Basic.Consumer()
}
```

`main` 패키지를 생성하고 `Consumer()을` 호출한다.

## Producer

go언어를 통해 쉽게 작성할 수 있는 Producer 예제이다.

```go
package Config

import "gopkg.in/confluentinc/confluent-kafka-go.v1/kafka"

func Kafka() *kafka.Producer{

    p, err := kafka.NewProducer(&kafka.ConfigMap{"bootstrap.servers": "localhost"})
    if err != nil {
        panic(err)
    }

    return p
}
```

`kafka.NewProducer(&kafka.ConfigMap{"bootstrap.servers": "localhost"})` 부분은 kafka에서 접속정보를 지정한다.

```go
package Basic

import (
    "fmt"
    "gopkg.in/confluentinc/confluent-kafka-go.v1/kafka"
    "kafka-producer/Config"
    "kafka-producer/Sensor"
)

func Producer()  {

    p := Config.Kafka()
    defer p.Close()

    // Delivery report handler for produced messages
    go func() {
        for e := range p.Events() {
            switch ev := e.(type) {
            case *kafka.Message:
                if ev.TopicPartition.Error != nil {
                    fmt.Printf("Delivery failed: %v\n", ev.TopicPartition)
                } else {
                    fmt.Printf("Delivered message to %v\n", ev.TopicPartition)
                }
            }
        }
    }()

    // Produce messages to topic (asynchronously)
    topic := "myTopic"

    for _, word := range []string{"Welcome", "to", "the", "Confluent", "Kafka", "Golang", "client"} {
        _ = p.Produce(&kafka.Message{
            TopicPartition: kafka.TopicPartition{Topic: &topic, Partition: kafka.PartitionAny},
            Value:          []byte(word),
        }, nil)
    }

    topic2 := "weather"

    var humid [10]string
    humid = Sensor.Humidity()

    for _, word := range humid {
        _ = p.Produce(&kafka.Message{
            TopicPartition: kafka.TopicPartition{Topic: &topic2, Partition: kafka.PartitionAny},
            Value:          []byte(word),
        }, nil)
    }

    // Wait for message deliveries before shutting down
    p.Flush(15 * 1000)

}
```

`topic := "myTopic"`, `topic2 := "weather"` 을 통해서 Topic을 지정하고 `TopicPartition을` 통해서 배열로 저장된 메시지를 byte 형식으로 메시지를 송신한다. `go func()` 부분은 go언어에서 익명 함수를 통해서 고루틴을 실행하고 메시지 전송 여부에 대해 체크한다.

두 개의 Topic이 생성되었기 때문에 Consumer에서는 수신받을 적절한 Topic을 지정해야 한다.

```go
package Sensor

import (
    "math/rand"
    "strconv"
    "time"
)

func Humidity() [10]string {

    var val[10] string
    for i := 0; i < len(val); i++ {
        rand.Seed(time.Now().UnixNano())
        val[i] = "humid : " + strconv.Itoa(rand.Intn(100))
    }

    return val
}
```

위 패키지는 특별한 기능은 없으며 `main`에서 작성된 부분을 따로 분리했다. 지금 예제는 습도라는 가상의 센서에서 배열을 반환하여 전송하는 기능을 담당한다.

```go
package main

import (
    "fmt"
    "kafka-producer/Basic"
)

func main() {

    fmt.Println("Kafka Producer Example")
    Basic.Producer()

}
```

`main` 패키지를 통해서 작성된 `Producer()`를 호출한다.

지금까지 Apache Kafka에 기본적인 개념과 CLI, 코드 실습을 진행하였다. 현재 작성된 기술만으로 모두 이해할 수는 없겠지만 본인 스스로 도움이 되었으면 한다.

