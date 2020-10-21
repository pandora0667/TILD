# kafka Installation



[toc]

## Description 

![](https://blog.kakaocdn.net/dn/bYrnay/btqwg3L8tCG/o6MB4Dsfw2XUYp33IrkvDk/img.png) 

본 문서에서는 Kafka를 사용하기 위해 각 OS별로 설치하는 방법에 대해 기술합니다. 이전 이론에 대한 학습과 더불어 본 설치를 통해서 본격적인 실습에 준비하시길 바랍니다. 



각 OS 버전에 따라 설치방법이 조금씩 다를 수 있으며, 기술한 버전 이외의 약간의 차이점이 있거나 실행이 정상적으로 진행되지 않을 수 있습니다. 따라서 본인 환경에 직접 적용하기 이전에 가상환경 등 테스트를 진행하는 것을 권장하며 혹은 Docker로 설치하는 것을 적극 권장합니다. 



## Windows 

#### 본 설치는 윈도우 2004 버전에서 진행했습니다.

- Apache Kafka를 설치하기 위해서는 먼저 JDK 1.8이 설치되어 있어야 합니다. 
  - 현재 LTS 버전인 JDK 11의 경우 Kafka 설치 및 운영시 오류가 발생할 수 있습니다. 

1. **JDK 1.8 Install** 

   - JDK 1.8을 설치하기 위한 다양한 방법이 있지만 본 문서에서는 Chocolate를 활용하여 진행합니다. 

     - PowerShell 실행을 실행하여 Chocolate 설치를 진행합니다. 

       ```powershell
       Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
       ```

   - 이후 cmd 창에서 다음과 같이 JDK 1.8 설치를 진행합니다. 

     ```powershell
     choco install jdk8
     ```

     ```powershell
     java -version
     ```

     ![](https://www.goavega.com/wp-content/uploads/2019/11/javasdk.png)

2. **Kafka Binary downloads**

   - 다음 링크에서 Kafka 바이너리 설치파일을 다운로드 받습니다. 

     - https://kafka.apache.org/downloads

     ![](https://github.com/pandora0667/TILD/blob/master/screenshot/kafka/windows/1.png?raw=true)

3. **Create Kafka folder**

   - 다운받은 바이너리 파일의 압축을 풀고 C 드라이브 혹은 D 드라이브 상단에 kafka 폴더를 생성하고 압축파일을 이동합니다. 

     ![](https://github.com/pandora0667/TILD/blob/master/screenshot/kafka/windows/2.png?raw=true)

4. **Change the default configuration value**

   - config/zookeeper.Properties 파일의 내용을 다음과 같이 수정합니다. 

     ![](https://github.com/pandora0667/TILD/blob/master/screenshot/kafka/windows/3.png?raw=true)

   - config/server.properties 파일의 내용을 수정합니다. 

     ![](https://github.com/pandora0667/TILD/blob/master/screenshot/kafka/windows/4.png?raw=true)

5. **Start apache Kafka and zookeeper** 

   - Kafka 데이터가 있는 디렉터리로 이동한 다음 터미널에서 다음 명령을 통해 zookeeper를 실행합니다. 

     ![](https://github.com/pandora0667/TILD/blob/master/screenshot/kafka/windows/5.png?raw=true)

   - 새로운 터미넣을 열고 Kafka 서버를 다음 명령을 통해 실행합니다. 

     ![](https://github.com/pandora0667/TILD/blob/master/screenshot/kafka/windows/6.png?raw=true)

6. **Test Topic** 

   - 기본 설치가 마무리 되었습니다. 다음 사진을 통해 정상적으로 동작하는지 여부를 파악하세요. 

     ![](https://github.com/pandora0667/TILD/blob/master/screenshot/kafka/windows/7.png?raw=true)



## Ubuntu 

#### 본 설치는 Ubuntu 20.04 LTS 버전에서 진행했습니다. 

1. 패키지 업데이트를 시행합니다. 

   ```bash
   $ sudo apt update && sudo apt upgrade -y 
   ```

2. JAVA를 설치합니다. JDK 8 혹은 JDK 11를 설치할 수 있습니다. 본 실습에서는 JDK 8를 기준으로 설치를 진행합니다. 

   ```bash
   $ sudo apt install openjdk-8-jdk
   ```

   ```bash
   $ sudo apt install openjdk-11-jdk
   ```

   ```bash
   $ java -version
   openjdk version "1.8.0_265"
   OpenJDK Runtime Environment (build 1.8.0_265-8u265-b01-0ubuntu2~20.04-b01)
   OpenJDK 64-Bit Server VM (build 25.265-b01, mixed mode)
   ```

3. 카프카 바이너리 파일을 다운로드 받고 압축을 해제한 다음 디렉토리를 이동합니다. 

   ```bash
   $ sudo wget https://downloads.apache.org/kafka/2.4.1/kafka_2.13-2.4.1.tgz
   ```

   ```bash
   $ sudo tar xzf kafka_2.13-2.4.1.tgz
   $ sudo mv kafka_2.13-2.4.1 /opt/kafka
   ```

4. 주키퍼와 카프카를 데몬으로 등록합니다. 

   ```bash
   $ sudo vi /etc/systemd/system/zookeeper.service
   ```

   ```bash
   [Unit]
   Description=Apache Zookeeper service
   Documentation=http://zookeeper.apache.org
   Requires=network.target remote-fs.target
   After=network.target remote-fs.target
   
   [Service]
   Type=simple
   ExecStart=/opt/kafka/bin/zookeeper-server-start.sh /opt/kafka/config/zookeeper.properties
   ExecStop=/opt/kafka/bin/zookeeper-server-stop.sh
   Restart=on-abnormal
   
   [Install]
   WantedBy=multi-user.target
   ```

   ```bash
   sudo vi /etc/systemd/system/kafka.service
   ```

   ```bash
   [Unit]
   Description=Apache Kafka Service
   Documentation=http://kafka.apache.org/documentation.html
   Requires=zookeeper.service
   
   [Service]
   Type=simple
   ExecStart=/opt/kafka/bin/kafka-server-start.sh /opt/kafka/config/server.properties
   ExecStop=/opt/kafka/bin/kafka-server-stop.sh
   
   [Install]
   WantedBy=multi-user.target
   ```

   ```bash
   $ sudo systemctl daemon-reload
   ```

5. 주키퍼와 카프카를 실행하고 실행상태를 확인합니다.

   ```bash
   $ sudo systemctl start zookeeper
   $ sudo systemctl start kafka
   ```

   ```bash
   $ sudo systemctl status zookeeper
   ● zookeeper.service - Apache Zookeeper service
        Loaded: loaded (/etc/systemd/system/zookeeper.service; disabled; vendor preset: enabled)
        Active: active (running) since Wed 2020-10-07 01:22:02 UTC; 1h 42min ago
          Docs: http://zookeeper.apache.org
      Main PID: 17647 (java)
         Tasks: 26 (limit: 1074)
        Memory: 72.7M
        CGroup: /system.slice/zookeeper.service
                └─17647 java -Xmx512M -Xms512M -server -XX:+UseG1GC -XX:MaxGCPauseMillis=20 -XX:InitiatingHeapOccupancyPercent=35 -XX:+ExplicitGCInvokes>
   
   ```

   ```bash
   $ sudo systemctl status kafka
   ● kafka.service - Apache Kafka Service
        Loaded: loaded (/etc/systemd/system/kafka.service; disabled; vendor preset: enabled)
        Active: active (running) since Wed 2020-10-07 01:26:25 UTC; 1h 38min ago
          Docs: http://kafka.apache.org/documentation.html
      Main PID: 18716 (java)
         Tasks: 66 (limit: 1074)
        Memory: 338.8M
        CGroup: /system.slice/kafka.service
                └─18716 java -Xmx1G -Xms1G -server -XX:+UseG1GC -XX:MaxGCPauseMillis=20 -XX:InitiatingHeapOccupancyPercent=35 -XX:+ExplicitGCInvokesConc>
   ```

6. 주키퍼와 카프카를 백그라운드로 실행하기 위한 스크립트를 작성합니다. (옵션사항)

   ```bash
   $ sudo vi kafkastart.sh
   ```

   ```bash
   #!/bin/bash
   sudo nohup /opt/kafka/bin/zookeeper-server-start.sh -daemon /opt/kafka/config/zookeeper.properties > /dev/null 2>&1 &
   sleep 5
   sudo nohup /opt/kafka/bin/kafka-server-start.sh -daemon /opt/kafka/config/server.properties > /dev/null 2>&1 &
   ```

   ```bash
   $ sudo chmod +x kafkastart.sh
   ```

7. 간단하게 토픽을 생성하여 정상적으로 실행되는지 확인합니다. 

   ```bash
   $ cd /opt/kafka
   $ sudo bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test
   Created topic "test".
   ```

   ```bash
   $ sudo bin/kafka-topics.sh --list --zookeeper localhost:2181
   test
   ```

8. 해당 토픽에 메시지를 생성합니다. 

   ```bash
   $ sudo bin/kafka-console-producer.sh --broker-list localhost:9092 --topic DevOps
   > Hello
   > Kafka
   ```

9. 컨슈머를 실행하여 해당 토픽으로 부터 메시지가 수신되는지 확인합니다. 

   ```bash
   $ sudo bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning
   > Hello
   > Kafka
   ```

10. 카프카를 원격에서 접속하기 위해서는 다음과 같이 설정파일을 수정합니다. 

    ```bash
    $ cd /opt/kafka/config
    $ sudo vi server.properties
    ```

    ```bash
    # listeners=PLAINTEXT://:9092
    advertised.listeners=PLAINTEXT://<HOST IP>:9092
    ```

    

## Docker 

#### 대부분의 시스템에 Docker로 설치하는 것을 권장합니다. 

1. docker-compose.yml 파일을 작성합니다. (본 파일은 confluentinc에 kafka-single-node을 참고하였다.)

   ```yml
   ---
   version: '2'
   services:
     zookeeper:
       image: confluentinc/cp-zookeeper:latest
       environment:
         ZOOKEEPER_CLIENT_PORT: 2181
         ZOOKEEPER_TICK_TIME: 2000
   
     kafka:
       image: confluentinc/cp-kafka:latest
       depends_on:
         - zookeeper
       ports:
         - 9092:9092
       environment:
         KAFKA_BROKER_ID: 1
         KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
         KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://kafka:29092,PLAINTEXT_HOST://localhost:9092
         KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: PLAINTEXT:PLAINTEXT,PLAINTEXT_HOST:PLAINTEXT
         KAFKA_INTER_BROKER_LISTENER_NAME: PLAINTEXT
         KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
   ```

2. 작성된 docker-compose.yml 파일을 실행하고 컨테이너 상태를 확인합니다. 

   ```bash
   $ docker-compose up -d
   Starting kafka-single-node_zookeeper_1 ... done
   Starting kafka-single-node_kafka_1     ... done
   
   $ docker-compose ps
               Name                         Command            State              Ports
   ------------------------------------------------------------------------------------------------
   kafka-single-node_kafka_1       /etc/confluent/docker/run   Up      0.0.0.0:9092->9092/tcp
   kafka-single-node_zookeeper_1   /etc/confluent/docker/run   Up      2181/tcp, 2888/tcp, 3888/tcp
   ```



---

​	Docker 설치에서 참고했던 링크를 공유한다. 들어가서 확인하시길 바랍니다. 

* https://www.joinc.co.kr/w/man/12/Kafka/docker 
* https://github.com/confluentinc/cp-docker-images
* https://www.skyer9.pe.kr/wordpress/?p=1307
* https://github.com/netman2k/docker-kafka-swarm
* https://github.com/wurstmeister/kafka-docker
