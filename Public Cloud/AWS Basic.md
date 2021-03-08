# AWS 기본 튜토리얼

![](https://upload.wikimedia.org/wikipedia/commons/thumb/9/93/Amazon_Web_Services_Logo.svg/768px-Amazon_Web_Services_Logo.svg.png)

처음 시작할 때 AWS는 매우 복잡하게 느껴질 수 있다. 다양한 설루션과 기능을 선택하는 데 있어 기존 오프레미스 방식과 근복적으로 다른 체계를 가지고 있으며, 요금 정책 또한 다양하기 때문이다. 이번 시간에서는 AWS를 접근하기 위해 필요한 기본적인 구조와 사용방법, 실습을 다루고 있다.

본 튜토리얼은 AWS 공식 홈페이지를 기반으로 작성되었음을 알린다.

## AWS 핵심 개념

AWS는 기존 사용자의 경험과 상관없이 시작할 수 있도록 설계 및 구성되어 있으며, 이 과정에서 AWS 5대 원칙 (AWS Well-Architected)를 따른다. AWS Well-Architected는 애플리케이션 및 워크로드에 사용할 보안, 성능, 복구 등 효율성이 뛰어난 인프라스트럭처 구조를 만들기 위해 만들어졌다.

### AWS Well-Architected와 5대 원칙

1.  운영 우수성 원칙
    -   시스템을 실행 및 모니터링하기 위해 변경 자동화, 이벤트 및 일상적으로 관리하는 인프라스트럭처 시스템의 표준을 정의한다.
2.  보안 원칙
    -   데이터의 기밀성, 무결성, 권한 관리를 통한 사용자 식별과 관리, 시스템 보호와 보안 이벤트를 탐지한다.
3.  안전성 원칙
    -   사용자가 해당 솔루션 혹은 기능을 사용하는 데 있어 안정적이고 원하는 시점에 올바르고 일관적으로 수행하도록 보장해야 한다.
4.  성능 효율성 원칙
    -   컴퓨팅, 네트워크, 스토리지와 같은 한정적인 자원을 효율적으로 사용할 수 있어야 한다.
5.  비용 최적화 원칙
    -   지출 영역 파악 및 통제, 가장 적절하고 적합한 수의 리소스 유형 선택, 시간대별 지출 분석과 초과 지출 없이 비즈니스 요구 사항을 유지하기 위해서 불필요한 비용을 피하는데 중점을 둔다.

## 가상 머신 생성하기

AWS에서 가상 머신을 사용하기 위해서는 EC2(Elastic Compute Cloud)를 사용하면 된다. 여러 가지 프로세서, 스토리지, 네트워킹, 운영 체제, 구매 모델을 선택할 수 있는 가장 폭넓고 세분화된 컴퓨팅 플랫폼을 제공한다.

### EC2 콘솔에서 시작하기

AWS EC2 대시보드가 업데이트가 되었기 때문에 본 장에서도 업데이트된 대시보드를 기준으로 설명한다. AWS 회원가입이 진행되어 있어야 하며, 간단하게 생성부터 종료까지 실습을 진행한다.

-   대시보드 화면에서 인스턴스 시작을 클릭한다.
    
    ![](https://github.com/pandora0667/TILD/blob/master/screenshot/cloud/AWS-EC2-1.png?raw=true)
-   Amazon Machine Image (AMI) 종류에서 본인이 원하는 이미지와 아키텍처를 선택한다. 최근 AWS에서는 ARM 기반의 인스턴스를 제공하고 있다.
    
    ![](https://github.com/pandora0667/TILD/blob/master/screenshot/cloud/AWS-EC2-2.png?raw=true)
-   인스턴스 유형에서 t2.micro를 선택하고 다음으로 넘어간다.
    
    ![](https://github.com/pandora0667/TILD/blob/master/screenshot/cloud/AWS-EC2-3.png?raw=true)
-   인스턴스 생성 이후 보안 그룹 구성으로 접속하여 SSH 접속을 제한한다. 실행하고자 하는 인스턴스의 역할에 따라 보안 그룹을 만들어서 규칙을 추가할 수 있다.
    
    ![](https://github.com/pandora0667/TILD/blob/master/screenshot/cloud/AWS-EC2-4.png?raw=true)
-   인스턴스에 접속하기 위한 키를 생성한다. 이미 키가 존재하는 경우 기존것을 사용하고 존재하지 않는 경우 새롭게 키를 발급받으며, 절대로 유출되지 않도록 유의한다.
    
    ![](https://github.com/pandora0667/TILD/blob/master/screenshot/cloud/AWS-EC2-5.png?raw=true)
-   인스턴스가 생성되고 몇 분이 지나면 인스턴스 상태가 `실행 중` 상태로 변경되어 사용할 수 상태로 변경된다.
    
    ![](https://github.com/pandora0667/TILD/blob/master/screenshot/cloud/AWS-EC2-6.png?raw=true)
-   다운로드 된 키의 퍼미션을 다음과 같이 변경하고 계정 이름과 퍼블릭 DNS를 통해 접속한다.
    
    ```
    $ chmod 400 key.pem 
    $ ssh ubuntu@ec2-public-dns-compute.amazonaws.com
    ```
    
    ![](https://github.com/pandora0667/TILD/blob/master/screenshot/cloud/AWS-EC2-7.png?raw=true)
-   최종적으로 만들어진 인스턴스는 아무것도 설치되지 않은 기본 상태이며, 본인이 수행하고자 하는 시스템 구성을 직접 구축한다. 이후 최종적으로 불필요한 요금 과금을 방지하기 위해 인스턴스를 종료한다.
    
    ![](https://github.com/pandora0667/TILD/blob/master/screenshot/cloud/AWS-EC2-8.png?raw=true)

### Amazon Lightsail

Lightsail은 애플리케이션 또는 웹 사이트를 구성하는데 필요한 모든 것을 쉽게 구축할 수 있도록 만들어진 가상 머신이다. 사용자는 미리 만들어진 템플릿을 통해 서버를 쉽게 생성하고 삭제할 수 있다.

-   아래 링크로 접속하여 Lightsail에 접속한다.
    
    -   [https://lightsail.aws.amazon.com/ls/webapp/home/instances](https://lightsail.aws.amazon.com/ls/webapp/home/instances)
    
    ![](https://github.com/pandora0667/TILD/blob/master/screenshot/cloud/Lightsail-wordpress-1.png?raw=true)
    
    Lightsail에서는 인스턴스와 컨테이너, 데이터베이스, 스토리지까지 한 번에 구축할 수 있으며, 기타 네트워크 및 스냅샷 설정도 가능하다. 각 기준에 따라 최대 3개월까지 무료로 사용할 수 있으나, 유료 서비스이기 때문에 주의한다. 본 챕터에서는 인스턴스 항목에서 워드프레스를 시작해 본다.
    
-   인스턴스 위치와 플랫폼 및 블루프린트를 선택한다.
    
    ![](https://github.com/pandora0667/TILD/blob/master/screenshot/cloud/Lightsail-wordpress-2.png?raw=true)
-   SSH 키의 경우 EC2 콘솔에서 생성한 키와 호환되지 않기 때문에 새로운 키를 생성하고 다운로드 받아 유출되지 않도록 안전하게 보관한다. 이후 인스턴스 플랜을 선택한다.
    
    ![](https://github.com/pandora0667/TILD/blob/master/screenshot/cloud/Lightsail-wordpress-3.png?raw=true)
-   고유한 리소스 이름을 작성하고 인스턴스를 생성한다.
    
    ![](https://github.com/pandora0667/TILD/blob/master/screenshot/cloud/Lightsail-wordpress-4.png?raw=true)
-   인스턴스 생성이 완료되면 다음과 같이 화면이 표시된다.
    
    ![](https://github.com/pandora0667/TILD/blob/master/screenshot/cloud/Lightsail-wordpress-5.png?raw=true)
-   생성한 인스턴스를 클릭하여 상세 페이지로 접속할 수 있으며, 사용자 계정과 IP 정보가 표시된다. SSH 접속을 위해 다음과 같이 입력한다.
    
    ![](https://github.com/pandora0667/TILD/blob/master/screenshot/cloud/Lightsail-wordpress-6.png?raw=true)
    
    ```
    $ chmod 400 LightsailDefaultKey-ap-northeast-2.pem
    $ ssh -i "LightsailDefaultKey-ap-northeast-2.pem" bitnami@52.79.134.108
    ```
    
    ![](https://github.com/pandora0667/TILD/blob/master/screenshot/cloud/Lightsail-wordpress-7.png?raw=true)
-   SSH로 접속한 이후 서비스가 정상적으로 동작하고 있는지 확인한다.
    
    ```
    bitnami@ip-172-26-6-156:~$ sudo systemctl status bitnami.service
    ● bitnami.service - LSB: bitnami init script
       Loaded: loaded (/etc/init.d/bitnami; enabled; vendor preset: enabled)
       Active: active (running) since Wed 2021-02-24 07:21:05 UTC; 5min ago
        Tasks: 263 (limit: 559)
       Memory: 255.9M
       CGroup: /system.slice/bitnami.service
               ├─ 1561 /opt/bitnami/apache2/bin/httpd.bin -f /opt/bitnami/apache2/conf/httpd.conf
               ├─ 1566 /opt/bitnami/apache2/bin/httpd.bin -f /opt/bitnami/apache2/conf/httpd.conf
               ├─ 1567 /opt/bitnami/apache2/bin/httpd.bin -f /opt/bitnami/apache2/conf/httpd.conf
               ├─ 2205 /bin/sh /opt/bitnami/mysql/bin/mysqld_safe --defaults-file=/opt/bitnami/mysql/my.cnf --mysqld=mysqld.bin --sock
               ├─ 2524 /opt/bitnami/mysql/bin/mysqld.bin --defaults-file=/opt/bitnami/mysql/my.cnf --basedir=/opt/bitnami/mysql --data
               ├─ 2728 php-fpm: master process (/opt/bitnami/php/etc/php-fpm.conf)
               ├─ 2729 php-fpm: pool wordpress
               ├─ 2951 /usr/bin/gonit
               └─15727 /opt/bitnami/apache2/bin/httpd.bin -f /opt/bitnami/apache2/conf/httpd.conf
    ```
    
-   서비스가 정상적으로 동작하고 있다면 웹 브라우저를 통해 퍼블릭 IP를 입력하면 워드프레스 블로그를 확인할 수 있다.
    
    ![](https://github.com/pandora0667/TILD/blob/master/screenshot/cloud/Lightsail-wordpress-8.png?raw=true)
-   마지막으로 생성된 인스턴스를 중지하고 삭제하여 추가 과금이 이루어지지 않도록 조치한다.
    
    ![](https://github.com/pandora0667/TILD/blob/master/screenshot/cloud/Lightsail-wordpress-9.png?raw=true)![](https://github.com/pandora0667/TILD/blob/master/screenshot/cloud/Lightsail-wordpress-10.png?raw=true)

## AWS Lambda Hello World

![](https://d2908q01vomqb2.cloudfront.net/1b6453892473a467d07372d45eb05abc2031647a/2019/02/04/sam-layers-diag.png)

클라우드 서비스 중에서 특히 AWS를 통해 얻을 수 있는 장점중에 하나는 서버리스 구조의 람다를 사용할 수 있다는 것이다. 서버리스는 서버를 고려하지 않고 애플리케이션과 서비스를 구축하고 실행할 수 있는 서비스로서, 서버 또는 클러스터 프로비저닝, 패치 및 OS 관리와 같은 인프라스트럭처 작업을 최소화할 수 있다.

거의 모든 유형의 애플리케이션과 벡엔드 서비스를 백엔드 서비스를 서버리스로 구축할 수 있으며, 애플리케이션을 높은 가용성으로 실행하고 확장하는 데 필요한 모든 사항이 자동으로 처리된다.

이번에는 AWS Lambda를 활용하여 서버리스 기반의 Hello, World 프로그램을 작성한다.

-   AWS Lambda 콘솔을 준비한다.
    
    ![](https://github.com/pandora0667/TILD/blob/master/screenshot/cloud/Lambda-1.png?raw=true)
-   블루프린트는 일부 최소한의 처리를 수행할 수 있는 예제 코드를 제공한다. 대부분 블루프린트는 Amazon S3, DynamoDB 또는 사용자 정의 애플리케이션과 같은 특정 이벤트 소스의 이벤트를 처리할 수 있다. 샘플을 수행하기 위해서 블루프린트 항목에 hello-world-python을 선택하고 구성을 진행한다.
    
    ![](https://github.com/pandora0667/TILD/blob/master/screenshot/cloud/Lambda-2.png?raw=true)
-   Lambda 함수는 사용자가 제공한 코드, 관련 종속성 및 구성으로 이루어져 있다. 사용자가 제공한 구성 정보에는 할당하려는 컴퓨팅 리소스(메모리 등), 실행 제한 시간, 그리고 사용자 대신 Lambda 함수를 실행할 수 있도록 AWS Lambda가 맡는 IAM 역할이 포함된다.
    
-   Lambda 함수를 실행하기 위한 기본 정보를 입력하고 함수를 생성한다.
    
    ![](https://github.com/pandora0667/TILD/blob/master/screenshot/cloud/Lambda-3.png?raw=true)
-   테스트 이벤트를 생성하고 값을 변경한다. 단, 기본 구조는 변경하지 않는다.
    
    ![](https://github.com/pandora0667/TILD/blob/master/screenshot/cloud/Lambda-5.png?raw=true)
-   Test 버튼을 클릭하여 함수와 이벤트 호출이 정상적으로 이루어지는지 확인한다.
    
    ![](https://github.com/pandora0667/TILD/blob/master/screenshot/cloud/Lambda-6.png?raw=true)
-   지금까지 Lambda를 활용하여 Hello World 예제를 실행했다. 추가적인 비용 지출을 막기 위해서 함수를 삭제한다.
    
    ![](https://github.com/pandora0667/TILD/blob/master/screenshot/cloud/Lambda-7.png?raw=true)