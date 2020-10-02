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

#### 본 설치는 Ubuntu 20.04 버전에서 진행했습니다. 





## Docker 

#### 대부분의 시스템에 Docker로 설치하는 것을 권장합니다. 







