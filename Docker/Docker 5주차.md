# Get Started, Part 5: Stacks



### Prerequisites

* Docker 1.13 버전 이상이어야 합니다.
* 파트 3과 파트 4의 실습이 선행되어 있어야 합니다. 
* 파트 2에서는 컨테이너의 생성방법에 대해서 알아보았습니다. 
* `friendlyhello`가 레지스트리에 푸시되어 있는지 확인해야 합니다. 
* 파트 3에서 진행했던 `docker-compose.yml` 파일을 준비해야 합니다. 
* 파트 4에서 진행했던 `docker-machine` 을 실행하고 준비하고 swarm을 설정합니다. 



### Introduction

* 파트 4장에서는 Docker를 실행하는 시스템 클러스터인 swarm을 설정하고 여러 가상환경에서 동시에 응용프로그램을      배포하는 방법을 학습했습니다. 
* 이번장에서는 분산 응용 프로그램 계층 구조의 최상위 계층에 속해있는 스택(stack)에 대해서 알아보도록 하겠습니다. 
* 스택이란 종속성을 공유하는 상호 연관된 서비스의 그룹이며, 같이 오케스트레이션 그룹을 조정할 수 있습니다. 
* 단일 스택은 전체 응용 프로그램의 기능을 정의하고 오케스트레이션 할 수 있습니다. (매우 복잡한 응용 프로그램이 여러 스택을 사용할 수도 있습니다.)
* 파트 3장에서 배운 compose 파일을 만들고 사용할 수 있으며 `docker stack deploy` 명령을 통해서 사용할 수 있습니다. 
* 하지만 지난 시간에 배운 것은 단일 호스트에서 실행되는 서비스 스택이었다는 점을 기억해야 합니다. 
* 이번시간에서는 여러 서비스를 서로 연관시켜 다중 시스템에서 실행 할 수 있도록 하는데 목적이 있습니다. 



### Add a new service and redeploy

* `docker-compose.yml`파일 에 서비스를 추가하는 것은 쉽습니다 . 

* 먼저, 우리의 swarm이 컨테이너를 어떻게 스케쥴 하는지를 볼 수있는 시각화 서비스를 추가 합니다. 

  ```yaml
  version: "3"
  services:
    web:
      # replace username/repo:tag with your name and image details
      image: username/repo:tag
      deploy:
        replicas: 5
        restart_policy:
          condition: on-failure
        resources:
          limits:
            cpus: "0.1"
            memory: 50M
      ports:
        - "80:80"
      networks:
        - webnet
    visualizer:
      image: dockersamples/visualizer:stable
      ports:
        - "8080:8080"
      volumes:
        - "/var/run/docker.sock:/var/run/docker.sock"
      deploy:
        placement:
          constraints: [node.role == manager]
      networks:
        - webnet
  networks:
    webnet:
  ```

* 다음과 같이 설정을 완료하고 지난번에 실행했던 명령을 통해서 `docker-mache`에 서비스를 추가합니다. 

  ```bash
  $ docker-machine scp docker-compose.yml myvm1:~
  $ docker-machine ssh myvm1 "docker stack deploy -c docker-compose.yml getstartedlab"
  ```

* 이후 `docker-machine ls` 명령어를 통해 vm의 ip주소를 알아내고 웹브라우저의 8080 포트로 접속을 시도합니다. 

  ```bash
  $ docker-machine ls
  NAME    ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER        
  myvm1   -        virtualbox   Running   tcp://192.168.99.100:2376           v18.05.0-ce
  myvm2   -        virtualbox   Running   tcp://192.168.99.101:2376           v18.05.0-ce
  ```

  ![](https://docs.docker.com/get-started/images/get-started-visualizer1.png)

  

* 화면에서 볼 수 있는 것 처럼 vm 2개의  getstartedlab이라는 컨테이너가 분할되어 실행 중인 모습을 확인할 수 있습니다.

* 다음 명령을 통해서 stack에 실행 중인 프로세스의 목록을 확인할 수 있습니다. 

  ```bash
  $ docker-machine ssh myvm1 "docker stack ps getstartedlab"
  ```

* visualizer는 스택에 포함된 모든 앱에서 실행할 수 있는 독립형 서비스입니다. 

* 다른 어떤 것에서도 의존하지 않는 독립적인 환경입니다. 

* 다음에는 방문자 카운터를 제공하는 Redis 서비스를 생성하도록 하겠습니다.  



### Persist the data

* 앱 데이터를 저장하기위한 Redis 데이터베이스를 추가하기 위해 한 번 더 살펴 보겠습니다.

* 아래와 같은 명령을  `docker-compose.yml`파일을 저장하면 Redis 서비스가 추가됩니다. 

  ```yaml
  version: "3"
  services:
    web:
      # replace username/repo:tag with your name and image details
      image: username/repo:tag
      deploy:
        replicas: 5
        restart_policy:
          condition: on-failure
        resources:
          limits:
            cpus: "0.1"
            memory: 50M
      ports:
        - "80:80"
      networks:
        - webnet
    visualizer:
      image: dockersamples/visualizer:stable
      ports:
        - "8080:8080"
      volumes:
        - "/var/run/docker.sock:/var/run/docker.sock"
      deploy:
        placement:
          constraints: [node.role == manager]
      networks:
        - webnet
    redis:
      image: redis
      ports:
        - "6379:6379"
      volumes:
        - "/home/docker/data:/data"
      deploy:
        placement:
          constraints: [node.role == manager]
      command: redis-server --appendonly yes
      networks:
        - webnet
  networks:
  	webnet:
  ```

* Redis는 도커에 공식 이미지가 배포되어 있기 때문에 image 항목에 redis라고 적으면, username/repo 항목을 자동으로 적용하여 다운로드 받을 수 있습니다. 

* Redis 포트는 6379번으로서, 컨테이너에서 호스트로 노출되도록 Redis에 사전 구성 되어있습니다. 

* Compose 파일에서 호스트는 전세계에 개방되어 있기 때문에 실제 호스트 IP주소를 입력할 수 있습니다. 

* 노드를 Redis Desktop Manager로 가져와서 Redis 인스턴스를 관리할 수 있습니다. 

* 가장 중요한 점은 스택 배포간의 데이터가 지속될 수 있도록 몇가지 사항이 Redis 사양에 포함되어 있다는 점 입니다. 

  * redis는 항상 Manager에서 실행되기 때문에 항상 동일한 파일 시스템을 제공합니다. 
  * redis는 호스트 파일 시스템에 있는 임의 디렉토리 컨테이너 내의 /data로 접근하고 이곳에 데이터를 저장합니다. 

* 본 YAML 파일을 적용하기 전에 Manager에게 디렉토리를 생성합니다. 

  ```bash
  $ docker-machine ssh myvm1 "mkdir ./data"
  ```

* 위의 명령을 실행한 뒤에 다시 변경사항을 실행할 수 있도록 deploy를 실행합니다. 

  ```bash
  $ docker-machine scp docker-compose.yml myvm1:~
  $ docker-machine ssh myvm1 "docker stack deploy -c docker-compose.yml getstartedlab"
  ```

* 명령이 완료되었다면 8080포트를 통해 redis가 추가 되었는지 확인하고 우리가 만든 응용프로그램에서 redis가 정상적으로 동작하는지 확인합니다. 정상적으로 잘 따라했다면 다음 그림과 같이 실행되는 모습을 볼 수 있습니다. 

  ![](https://docs.docker.com/get-started/images/app-in-browser-redis.png) 

  ![](https://docs.docker.com/get-started/images/visualizer-with-redis.png)



# Get Started, Part 6: Deploy your app

* 이번장에서는 실질적으로 클라우드 시스템에 우리가 만든 어플리케이션을 배포하고 관리하는 방법을 알아보도록 하겠습니다. 
* AWS와 Microsoft의 Azure를 통해 시스템을 배포할 수 있지만 우리는 AWS을 통해서 swarm을 통해 앱을 배포하고 사용하도록 하겠습니다. 
* AWS 계정이 없는 분들은 아래 사이트에서 계정 가입을 진행합니다.  (AWS는 1년 프리티어 계정을 지원합니다.)
  * https://aws.amazon.com/ko/

* 우리가 사용하는 Docker CE의 경우  Docker Cloud를 사용하여 Amazon Web Services, DigitalOcean 및 Microsoft Azure와 같은 유명한 서비스 공급자에서 응용 프로그램을 관리 할 수 있습니다.
* 설정 및 배포를 위해서는 다음과 같은 일이 필요합니다. 
  -  Docker Cloud를 사용하여 Amazon Web Services, DigitalOcean 및 Microsoft Azure와 같은 유명한 서비스 공급자에서 응용 프로그램을 관리 합니다. 
  - Docker Cloud를 사용하여 컴퓨팅 리소스를 만들고 swarm을 만들 수 있습니다.
  - 앱을 배포합니다.

### Connect Docker Cloud

* Docker Cloud는 [표준 모드](https://docs.docker.com/docker-cloud/infrastructure/) 나 [Swarm 모드](https://docs.docker.com/docker-cloud/cloud-swarm/) 에서 실행할 수 있습니다 .

* 각 실행 밥법에 대해서는 심화과정에 속해있기 때문에 정리하여 따로 실습하는 시간을 가지도록 하겠습니다. 

  



