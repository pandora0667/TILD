# Get Started, Part 4: Swarms 

### Docker Swarm 을 알기전에 알아야 하는 것들 

​	

![](https://subicura.com/assets/article_images/2017-02-25-container-orchestration-with-docker-swarm/swarmnado.gif)



* Docker Swarm을 이해하기 전에 서버 오케스트레이션(server orchestration) 이란 단어가 무엇인지 이해해야 합니다. 

* 만약 당신이 회사에서 인프라를 관리하고 있는데 각각의 서버의 역할을 분할하고 적용했다고 생각합니다. 

  * 연구실을 예를들어 웹서버 1, 가상화 서버 2, GitLab 3, DB 4 등 각각의 역할을 두고 서버를 운영 한다고 가정합니다.
  * 처음에 봤을때는 역할별로 정리가 잘 되어있다고 생각할 수 있지만 특정 서비스에 작업이 몰리거나, 역할이 모호한 어플리케이션과 같은 경우에는 어디에 설치해야 하는지 애매한 경우가 생길 수 있습니다. 
  * 또한 다른 서버로 변경하고 싶어도 기존 서버의 의존성이 강하여 쉽게 옮기지 못하는 경우가 생기는 경우도 있습니다. 
  * Docker를 사용한다면 쉽게 해결할 수 있으나 역시 사람이 관여를 해야하는 부분이기 때문에 완전한 자동화 개발툴이 필요하다고 사람들은 생각하게 되었습니다. 

  ![](https://subicura.com/assets/article_images/2017-02-25-container-orchestration-with-docker-swarm/container-orchestration.png)

* 서버 오케스트레이션(server orchestration)은 **여러 서버와 서비스를 자동으로 관리**해주는 작업이라고 생각하면 됩니다. 

* 각 작업에는 스케줄링, 클러스터링, 서비스 디스커버리, 로깅, 모니터링과 같은 기능이 포함되어 있습니다. 

  * **스케줄링** 
    * 컨테이너를 적당한 서버에 배포해 주는 역할을 담당합니다. 오케스트레이션 툴에 따라서 지원하는 기능이 조금씩 다르다고 할 수 있는데, 유휴상태인 서버에 배포하거나, 차례대로 배포 혹은 랜덤하게 배포할 수 있습니다. 
    * 컨테이너의 수를 여러개로 나눠서 배포하고 컨테이너가 죽으면 즉시 컨테이너를 다른서버에 배포하여 서비스에 문제가 없도록 합니다. 
  * **클러스터링**
    * 여러개의 서버를 하나의 서버처럼 사용할 수 있도록 하는 기술을 말합니다. 클러스터에 새로운 서버를 추가하거나 제거할 수 있고, 네트워크를 통해 멀리 떨어져 있는 서버를 사용할 수 있습니다. 
    * 적개는 몇대 부터 수천대의 서버를 클러스터로 묶어서 관리할 수 있습니다. 
  * **서비스 디스커버리**
    * 서비스를 찾아주는 기능으로서 클러스터 환경에서는 컨테이너가 어느 서버에 실행되는지 알 수 없고 컨테이너가 중지되면 IP나 Port 정보를 업데이트 해야 하기 때문에 이러한 기능을 담당해주는 서비스라고 생각하면 됩니다. 
  * **로깅, 모니터링**
    * 여러 대의 서버를 관리하는 경우 로그서버에 한곳에서 관리하는게 편리하기 때문에 사용합니다. 
    * 별도의 프로그램을 사용하는 경우도 있습니다. 

* 하지만 이러한 서버 오케스트레이션은 도입하기 상당히 어려운 부분이 존재합니다. 설치와 관리가 어렵고 보통은 수백~수천개의 대규모의 서버를 관리하기 위한 목적으로 나왔기 때문에 도입시 많은 비용과 관리비가 추가적으로 들어가게 됩니다. 

* 만약 이런 대규모 서비스를 구축하기 위해서는 많은 공부가 필요함은 물론, 장애가 생기면 전체 서버에 문제가 생길 수 있기 떄문에 도입에 상당히 조심 스러운 부분이 있었습니다. 

* 서버 오케스트레이션은 많은 장점도 있었지만 단점도 많기 때문에 도입이 활발하게 이루워 지지 않았지만 Docker Swarm이 등장하고 나서 많은 변화가 일어나게 되었습니다. 

  * **구축 비용이 거의 들지 않고 다양한 기능을 쉽게 그리고 가볍게 사용** 할 수 있다는 장점이 있습니다. 



### 컨테이너 오케스트레이션 툴

![](https://subicura.com/assets/article_images/2017-02-25-container-orchestration-with-docker-swarm/orchestration-tools.png)



* 호스트 머신이 한대가 있다면 docker-run이나 docker-compose 등 무엇으로 container를 실행해도 상관은 없으나 사용자가 많아져서 부하가 커지게 된다면 한대의 호스트 머신으로 서비스를 감당할 수 없습니다.

* 따라서 이러한 문제를 해결하기 위해서 여러대의 호스트 머신에서 여러개의 container를 실행하기 위해 네트워크와 호스트 리소스 등을 포함한 여러 문제를 해결해 주기 위해서 나온 것이 **컨테이너 오케스트레이션**입니다. 

* 컨테이너 오케스트레이션을 제공하는 툴은 상당히 많이 존재하며, 그중 현재 가장 뜨고 있는 것은 **쿠버네티스** 입니다. 

  ![](https://raw.githubusercontent.com/1ambda/1ambda.github.io/master/assets/images/infra-kubernetes/intro/logo.png)

* 쿠버네티스는 15년 동안 구글의 노하우로 만든 툴입니다. 

  * Production-Grade Container orchestration 기능이 매우 휼륭하여 자동화된 컨테이너 배포 및 확장과 관리가 용이합니다. 
  * 자체적으로 구축하기에는 어려움이 있으나 GCG, Azure와 같은 플랫폼을 사용하면 편리하게 구현이 가능합니다. 

* 쿠버네티스에 대한 자세한 설명은 본 절에서 다루기에는 논외이기 때문에 나중에 자세히 설명하도록 하겠습니다. 



### Docker Swarm 

* Docker Swarm은 Docker와 별도로 개발되었지만 1.12 버전 이후부터 통합되었습니다. 

* Docker에 모든 것이 통합되어 있기 때문에 별도의 설치가 필요가 없으며 도커의 명령어와 compose을 그대로 사용할 수 있기 때문에 다른 툴에 비해서 쉽다는 장점을 가지고 있습니다. 

* 기능이 단순하기 때문에 세부적인 설정이 어렵다는 단점을 가지고 있으나 5000개의 컨테이너에서도 동작하는 안정성이 입증되어 있기 때문에 아직도 많이 사용하는 오케스트레이션이라고 할 수 있습니다. 

* 일단 진도를 나가기 전에 용어에 대한 정리를 하고 진행하도록 하겠습니다. 

  |          용어          |                             내용                             |
  | :--------------------: | :----------------------------------------------------------: |
  |       Container        | 사용자가 실행하는 프로그램의 독립적인 실행을 보장하기 위한 격리된 공간 |
  |          Task          | 도커 컨테이너 또는 격리된 공간 안에서 실행되는 작업단위로, Swarm이 작업계획을 관리하는 최소 단위 |
  |        Service         | 사용자가 Docker Swarm에게 단위 업무를 할당하는 논리적인 단위, Swarm에 의해 Task로 분할되어 처리 |
  | Dockerized Host / Host |     Docker Engine이 탑재된 Virtual Machine이나 Baremetal     |
  |   Swarm Node / Node    |          Docker Engine이 Swarm mode로 동작하는 Host          |
  | Manager Node / Manager |     Swarm Node 중에서 Cluster 관리 역할을 수행하는 Node      |
  |  Worker Node / Worker  | Swarm Node 중에서 Container를 실행하여 실제 일을 처리하는 Node. 일부러 제외하지 않으면 모든 Node는 기본적으로 Worker가 됨 |



### Prerequisites

* 설치된 Docker 버전이 1.13 이거나 그 이상 버전을 사용하고 있어야 합니다. 

* Docker Compose가 있어야 합니다. 자세한 정보는 이전 챕터를 참조하세요. 

*  Docker Machine이 필요합니다. Docker for Mac, Docker for Windows에서는 기본적으로 설치되어 있으나 Linux사용자의 경우 설치가 필요합니다. Windows 10 이전버전을 사용하시는 분에 대해서는 Docker Toolbox 를 설치하세요. 

  * ***Install Docker Machine***

    * Mac OS X

    ```bash
    $ base=https://github.com/docker/machine/releases/download/v0.14.0 &&
      curl -L $base/docker-machine-$(uname -s)-$(uname -m) >/usr/local/bin/docker-machine &&
      chmod +x /usr/local/bin/docker-machine
    ```

    * Linux 

    ```bash
    $ base=https://github.com/docker/machine/releases/download/v0.14.0 &&
      curl -L $base/docker-machine-$(uname -s)-$(uname -m) >/tmp/docker-machine &&
      sudo install /tmp/docker-machine /usr/local/bin/docker-machine
    ```

    * Windows with Git Bash 

    ```bash
    $ base=https://github.com/docker/machine/releases/download/v0.14.0 &&
      mkdir -p "$HOME/bin" &&
      curl -L $base/docker-machine-Windows-x86_64.exe > "$HOME/bin/docker-machine.exe" &&
      chmod +x "$HOME/bin/docker-machine.exe"
    ```

  * 설치 이후 버전을 확인합니다. 

    ```bash
    $ docker-machine version
    docker-machine version 0.14.0, build 89b8332
    ```

* 지난 시간에서 테스트로 작성한 friendlyhello 이미지가 레지스트리에 푸시되어 있어야 합니다. 



### Introduction 

* 지난 시간에서는 응용 프로그램을 확장하고 로드밸런싱을 활성화 하는 방법을 알아보았습니다. 
* 이를 통해 인스턴스를 5개를 생성하여 서비스를 확장였으나 이번 시간에서는 여러개의 머신에서 실행할 수 있도록 클러스터에 배포합니다. 다중 머신 어플리케이션은 여러개의 머신을 사용할 수 있도록 하며 이를 "Dockerized" 라고 말하며,  이러한 클러스터에서 여러 시스템을 결합하는 것을 swarm이라고 합니다. 



### Understanding Swarm Clusters

* swarm은 도커를 실행하고 클러스터링에 join한 그룹입니다. 이러한 그룹은  **swarm manager** 가 관리하게 됩니다. 
* swarm manager에 있는 머신은 물리 혹은 가상 머신일 수 있으며,  swarm에 가입하게 되면 node라고 부르게 됩니다. 
* swarm manager는 시스템 사용률이 가장 낮은 머신에게 컨테이너를 할당하는 “emptiest node"으로 동작하며, 혹은 “global”로 설정하게 되면 지정된 컨테이너의 인스턴스를 하나를 정확하게 가져오도록 할 수 있습니다. 
* swarm manager는 명령을 실행하고 worker가 swarm에 속하도록 관리할 수 있는 유일한 권한이 있으며  worker는  swarm manager의 통제하에 있기 때문에 어떤한 작업을 다른 worker에게 지시할 권한은 없습니다. 
* 지금까지 우리는 단일 머신에서 Docker를 사용했으나 Docker는 swarm모드로 전환하여 사용할 수 있습니다. 
* swarm을 활성화 하면 현재 머신은 swarm에 맵핑되고 swarm manager가 관리를 대신하게 됩니다. 



### Set up your swarm 

* swarm은 여러 노드로 구성되어 있으며, 물리 혹은 가상시스템으로 구성되어 있을 수 있습니다. 
* docker swarm init으로 간단하게 사용할 수 있으며, swarm 모드를 활성화고 현재 시스템은 swarm manager가 되며, 다른 시스템에서 docker swarm join 명령을 통해서 swarm에 join 할 수 있습니다. 
* 본 챕터에서는 VM을 활용하여 2대의 PC를 만들고 swarm을 통해 클러스터링 할 것입니다. 

#### Create a cluster

* Local VMs (Mac, Linux, windows 7 and 8)

  * 먼저 가상머신을 생성하기 위해 virtualbox가 필요합니다. 

  >윈도우10과 같이 Hyper-V가 설치된 환경에서는 virtualbox를 설치할 필요가 없기 때문에 Hyper-V를 사용합니다. 
  >
  >Docker Toolbox를 사용한다면 virtualbox가 포함되어 있기 때문에 다음과 같이 실행할 수 있습니다. 

  

  ```bash
  & docker-machine create --driver virtualbox myvm1
  & docker-machine create --driver virtualbox myvm2
  ```

* Local VMs(Windows 10 / Hyper-V)

  * 먼저 가상 시스템 (VM)이 공유 할 수 있도록 가상 스위치를 신속하게 생성하여 서로 연결할 수 있도록합니다.
    1. Hyper-V 관리자 시작
    2. 오른쪽 메뉴에서 **Virtual Switch Manager** 를 클릭
    3. 유형 **외부의** **가상 스위치 만들기를** 클릭합니다.
    4. 이름을 지정 `myswitch`하고 호스트 컴퓨터의 활성 네트워크 어댑터를 공유하려면 확인란을 선택합니다.

  ```bash
  docker-machine create -d hyperv --hyperv-virtual-switch "myswitch" myvm1
  docker-machine create -d hyperv --hyperv-virtual-switch "myswitch" myvm2
  ```

* 다음 명령을 통해 VM의 주소를 가져옵니다. 

  ```bash
  $ docker-machine ls
  
  NAME    ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER        
  myvm1   -        virtualbox   Running   tcp://192.168.99.100:2376           v18.05.0-ce
  myvm2   -        virtualbox   Running   tcp://192.168.99.101:2376           v18.05.0-ce
  ```

* swarm 초기화을 초기화 하고 노드를 다음과 같이 추가 합니다. 

  ```bash
  $ docker-machine ssh myvm1 "docker swarm init --advertise-addr <myvm1 ip>"
  Swarm initialized: current node <node ID> is now a manager.
  
  To add a worker to this swarm, run the following command:
  
    docker swarm join \
    --token <token> \
    <myvm ip>:<port>
  
  To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
  ```

  * 이후 두번째 머신에서는 myvm1에서 나온 `docker swarm join` 명령어를 아래와 같이 입력합니다. 
  * `docker swarm init` 에서는 `docker swarm join`추가하려는 노드에서 실행할 수 있도록 미리 구성된 명령이 포함되어 있습니다. 

  ```bash
  $ docker-machine ssh myvm2 "docker swarm join \
  --token <token> \
  <ip>:2377"
  
  This node joined a swarm as a worker.
  ```

* 첫번째 swarm을 만들었습니다!! 축하합니다. 

* 다음 명령을 통해 swarm의 매니저 노드를 확인하세요 

  ```bash
  $ docker-machine ssh myvm1 "docker node ls"
  ID      HOSTNAME    STATUS   AVAILABILITY  MANAGER STATUS   ENGINE VERSION
  k6w***    myvm1      Ready       Active         Leader        18.05.0-ce
  j14***    myvm2      Ready       Active                       18.05.0-ce
  ```

  >처음부터 다시 시작하려면 `docker swarm leave` 를 각 노드에서 실행하세요. 
  >
  >`docker-machine ssh myvm2 "docker swarm leave"`
  >
  >`docker-machine ssh myvm1 "docker swarm leave"`



### Deploy app your app on the swarm cluster 

* 이제 이전장에서 실행했던 컨테이너를 실행하면 됩니다. 
* 반드시 명심해야 할 점은 swarm managers은 myvm1이며 다른 노드는 신경쓰지 않아도 됩니다. 



#### Configure a docker-machine shell to the swarm manager

* 지금까지는 `docker-machine ssh` VM에 명령하기 위해서 Docker 명령을 래핑했습니다 . 

* 다른 방법은  `docker-machine env <machine>`현재 셸을 구성하여 VM의 Docker 데몬과 통신하도록 명령을 실행하는 것입니다.

*  이 방법은 로컬 `docker-compose.yml`파일을 사용하여 어디서나 복사 할 필요없이 "원격"으로 응용 프로그램을 배포 할 수 있습니다. 

* `docker-machine env myvm1` 을 입력한 다음 출력의 마지막 줄에 제공된 명령을 복사하여 붙여 넣고 실행하여 셸이 대화 할 `myvm1`  swarm manager를 구성합니다 .

* 쉘을 구성하는 명령은 Mac, Linux 또는 Windows 중 어떤 것이 있는지에 따라 다르기 때문에 아래를 참고하세요.

  * Mac & Linux

  ```bash
  $ docker-machine env myvm1
  export DOCKER_TLS_VERIFY="1"
  export DOCKER_HOST="tcp://192.168.99.100:2376"
  export DOCKER_CERT_PATH="/Users/sam/.docker/machine/machines/myvm1"
  export DOCKER_MACHINE_NAME="myvm1"
  # Run this command to configure your shell:
  # eval $(docker-machine env myvm1)
  
  eval $(docker-machine env myvm1)
  
  $ docker-machine ls
  NAME    ACTIVE   DRIVER       STATE     URL                      SWARM   DOCKER        ERRORS
  myvm1   *        virtualbox   Running   tcp://192.168.99.100:2376         v17.06.2-ce   
  myvm2   -        virtualbox   Running   tcp://192.168.99.101:2376         v17.06.2-ce   
  ```

  * Windows 

  ```shell
  PS C:\Users\sam\sandbox\get-started> docker-machine env myvm1
  $Env:DOCKER_TLS_VERIFY = "1"
  $Env:DOCKER_HOST = "tcp://192.168.203.207:2376"
  $Env:DOCKER_CERT_PATH = "C:\Users\sam\.docker\machine\machines\myvm1"
  $Env:DOCKER_MACHINE_NAME = "myvm1"
  $Env:COMPOSE_CONVERT_WINDOWS_PATHS = "true"
  # Run this command to configure your shell:
  # & "C:\Program Files\Docker\Docker\Resources\bin\docker-machine.exe" env myvm1 | Invoke-Expression
  
  & "C:\Program Files\Docker\Docker\Resources\bin\docker-machine.exe" env myvm1 | Invoke-Expression
  
  PS C:PATH> docker-machine ls
  NAME    ACTIVE   DRIVER   STATE     URL                          SWARM   DOCKER        ERRORS
  myvm1   *        hyperv   Running   tcp://192.168.203.207:2376           v17.06.2-ce
  myvm2   -        hyperv   Running   tcp://192.168.200.181:2376           v17.06.2-ce
  ```



### Deploy the app one the swarm manager 

*  실행한 myvm1에서  swarm manager 사용하여 응용 프로그램을 배포 할 수 있습니다. 

* myvm1에서 `docker stack deploy` 명령을 통해서 지난 시간에 사용했던 `docker-compose.yml` 복사합니다. 

*  이 명령을 완료하는 데 몇 초가 걸릴 수 있으며 배포에 시간이 걸릴 수 있습니다. 

* `docker service ps <service_name>`  명령을 사용하면 서비스가 재배포되었는지 확인할 수 있습니다. 

* 이전에 작성한 `docker-compose.yml`이 있는지 확인하고 myvm1으로 파일을 scp 명령을 통해 전달합니다. 

* 이전과 마찬가지로 다음 명령을 실행하여 앱을 배포합니다. 

  ```bash
  $ docker-machine scp docker-compose.yml myvm1:~
  $ docker-machine ssh myvm1 "docker stack deploy -c docker-compose.yml getstartedlab"
  ```

* 위와 같이 설정하면 swarm cluster가 완료됩니다. 

![](https://docs.docker.com/get-started/images/app-in-browser-swarm.png)



### Accessing your cluster

![](https://docs.docker.com/engine/swarm/images/ingress-routing-mesh.png)

>swarm 모드를 활성화하기 전에 swarm에서 수신 네트워크를 사용하려면 swarm 노드간에 다음 포트를 열어야합니다.
>
>- 컨테이너 네트워크 검색을위한 포트 7946 TCP / UDP
>- 컨테이너 수신 네트워크 용 포트 4789 UDP.



* 위의 그림은  `my-web`포트 `8080`에서 게시 된 서비스의 라우팅 메쉬가 어떻게 보이는지에 대한 다이어그램입니다 .
* 우리는 2개의 VM을 생성했습니다. 확인해 보면 두 IP 주소로 접근할 때 정상적으로 동작하는 것을 확인할 수 있는데 그 이유는 하나의 swarm 노드가  **routing mesh** 로 구성되기 때문입니다. 
* 이렇게하면 실제로 컨테이너를 실행중인 노드가 무엇이든 관계없이 swarm의 특정 포트에 배포 된 서비스가 항상 해당 포트를 소유하도록 할 수 있습니다. 

### Iterating and scaling your app

* 이전에 배웠던 방법과 똑같이 yml 파일을 수정하면 컨테이너의 수를 조정할 수 있습니다. 



### Clenup and reboot 

* Unsetting docker-machine shell variable settings

  * Mac or Linux 

    ```bash
    $ eval $(docker-machine env -u)
    ```

  * windows 

    ```powershell
      & "C:\Program Files\Docker\Docker\Resources\bin\docker-machine.exe" env -u | Invoke-Expression
    
    ```

* 배포한 서비스를 중지하려면 다음 명령을 입력합니다. 

```bash
$ docker-machine ssh myvm1 "docker stack rm getstartedlab"
Removing service getstartdlab_web
Removing network getstartdlab_webnet
```

* swarm을 종료하기 위해서는 다음 명령을 입력합니다. 

```bash
$ docker-machine ssh myvm2 "docker swarm leave"
$ docker-machine ssh myvm1 "docker swarm leave --force"
```

* 버추얼 머신을 종료하거나 삭제하기 위해서는 다음과 명령을 입력합니다. 
  * 아래의 명령은 모든 VM을 정지하고 삭제하기 때문에 일부만 종료하거나 삭제해야 한다면 머신의 이름을 적어줍니다. 

```bash
$ docker-machine stop $(docker-machine ls -q)
$ docker-machine rm $(docker-machine ls -q)
```

