![](https://github.com/pandora0667/TILD/blob/master/screenshot/kafka/PART%203%20Apache%20Kafka/Untitled.png?raw=true)

카프카에 대해서 다시 한번 용어를 정리하면 다음과 같다.

-   **프로듀서** : 카프카로 메시지를 보내는 역할
-   **카프카** : 프로듀서가 보낸 메시지를 저장하는 역할
-   **컨슈머** : 카프카에 저장되어 있는 메시지를 가져오는 역할
-   **주키퍼** : 카프카가 분산 코디네이터인 주키퍼와 연결하여 메타 데이터 및 클러스터 노드 관리

---

카프카를 활용하기 위해서는 클러스터 구성이 반드시 필요하다. 일반적으로 주키퍼 replication 방식이 홀수를 유지해야 하기 때문에 최소 3대를 구성해야 한다. 왜 홀수로 구성해야 하는지에 대한 내용을 설명하도록 한다.

### 들어가기 전에...

카프카를 사용하기 위해서는 주키퍼(Zookeeper) 사용이 필수적이다. 주키퍼는 분산 시스템에서 필수적인 계층 구조인 Key-Value 저장 구조를 통해서 대규모 시스템에 분산된 설정 서비스, 동기화 서비스, 네이밍 등록에 사용될 수 있다.

다중 서비스를 제공하기 위해서 클라이언트가 응답이 실패한 경우 다른 주키퍼 리더에 요청할 수 있다. 주키퍼 노드는 파일 시스템이나 트리 구조와 유사한 계층적 네임 스페이스에 데이터를 저장한다. 클라이언트 노드에 읽고, 쓸 수 있으며, 이러한 방식으로 공유 서비스를 제공한다.

이러한 고가용성 상태에서 주키퍼간의 데이터 정보는 원자적으로 이루어진다.

![](https://github.com/pandora0667/TILD/blob/master/screenshot/kafka/PART%203%20Apache%20Kafka/Untitled%201.png?raw=true)

주키퍼는 standalone, quorum mode로 구성된다. quorum를 번역하면 정족수라는 뜻으로 합의체가 의사를 진행시키거나 의결을 하는 데 필요한 최소한도의 인원수를 뜻한다. 쉽게 말해서 클러스터에서 장애가 발생했을 때 정상적인 운영이 가능한 최소 노드의 수를 말한다.

클러스터로 구성된 클라이언트 노드의 경우 업데이트 요청이 있을 경우 quorum 크기가 동일한 최소 노드 수에 기록이 될 경우 안정적으로 데이터가 저장되었다고 간주한다. 즉 예기치 못한 경우로 인해서 서버가 다운되었고, 이때 데이터 추가가 일어났을 경우 다운된 서버와 그렇지 않은 서버 간의 데이터 정합성을 맞추려 할 때 과반수 서버에 저장된 데이터의 결과를 보고 데이터의 정합성을 유지한다. 이로 인해서 주키퍼 클러스터는 quorum을 유지하는 동안 서비스를 유지할 수 있으며, 과반수 이상의 서버가 다운됐을 경우 서비스는 중단된다.

### 예제

주키퍼 클러스터를 9개로 설정하고 quorum 크기가 5개로 선택했다고 한다면, 모든 클라이언트에서 데이터 수정이 발생할 경우 5개의 노드에서 데이터가 일치해야 안정적인 상태라고 말할 수 있다. 만약 주키퍼 클러스터 중 4개가 다운됐다고 한다면 나머지 5개의 quorum이 클라이언트의 요청을 처리할 수 있다.

### 가장 적절한 클러스터 노드의 수는?

클러스터를 구성하기 위해 가장 적절한 노드를 계산하는 방법은 (n/2 +1)이다. 여기서 n은 총노드의 수로 5개의 클러스터에서 quorum을 구성하고자 한다면 (5/2 +1)의 계산 방식에 따라 3개의 노드가 필요하다.

이해를 돕기 위해 2곳에 데이터 센터에 클러스터를 구성했다고 가정하면 다음과 같다.

![](https://github.com/pandora0667/TILD/blob/master/screenshot/kafka/PART%203%20Apache%20Kafka/Untitled%202.png?raw=true)

DC1에는 3개의 노드가 존재하며 DC2에는 2개의 노드가 존재하며, 각각의 quorum 크기는 2라고 가정한다. 만약 두 데이터센터 사이에 네트워크 단절로 인해서 데이터 센터 간의 통신이 불가능한 경우 각각의 데이터센터에 있는 노드들은 클라이언트의 읽기, 쓰기 요청을 받아 처리를 진행하게 된다.

장애가 발생하기 전에는 DC1과 DC2간의 업데이트를 통해서 데이터 정합성이 유지되지만 네트워크가 단절된 상태에서는 데이터를 전달할 수 없기 때문에 각각의 데이터센터에 존재하는 노드에서는 다른 데이터를 가지는 현상이 발생한다.

클러스터로 구성된 두 시스템 그룹 간 네트워크 혹은 서버 장애로 인해서 일시적으로 단절 현상이 발생하여 나타나는 문제를 스플릿 브레인(Split Brain) 현상이라고 한다. 이러한 문제가 발생하게 될 경우 클러스터 상의 모든 노드가 각자 자신이 Primary로 인식하게 되어 중복으로 서비스가 실행하게 된다. 이로 인해 데이터 동기화 문제로 인해서 추후 문제가 해결되었을 때 정상적인 운영이 불가능하게 되므로 클러스터를 구성하는 의미 자체가 사라지게 된다.

따라서 스플릿 브레인 문제를 방지하기 위해 다수의 합의 요청이 가능한 quorum 크기는 (n/2 +1) 개가 항상 만족해야 한다. 위의 그림에서 5개의 노드가 정상적으로 실행하기 위해서는 3개의 노드가 필수적이지만 DC2는 quorum 구성이 불가능하기 때문에 데이터 처리가 불가능하다.

### 반드시 클러스터 노드의 수가 짝수가 되면 안 되는 건가?

결론적으로 말하면 짝수개의 노드로 클러스터를 구성한다고 해도 문제가 생기지는 않는다.

쉽게 이해하기 위해서 5개로 구성된 클러스터 A, 6개로 구성된 클러스터 B가 존재한다고 가정한다. quorum을 만족하는 노드의 수는 A 클러스터는 (5/2 +1)=3개이며, B 클러스터는 (6/2 +1)=4개가 된다. 만약 장애가 발생해도 정상적으로 동작하기 위한 장애 허용 노드를 계산하면, A 클러스터는 (5-3)=2, B 클러스터는 (6-4)=2가 된다. A 클러스터와 B 클러스터 모두 2개 노드의 장애를 허용할 수 있으며, B 클러스터는 노드 추가로 인한 오버헤드가 발생하기 때문에 짝수개의 노드로 구성하는 것은 이점이 없다.



참고자료 : https://medium.com/@bikas.katwal10/why-zookeeper-needs-an-odd-number-of-nodes-bb8d6020e9e9