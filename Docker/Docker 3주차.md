# Get Started, Part2: Containers

###Share Your image

* 도커의 장점 중 하나는 도커에서 빌드한 이미지를 쉽게 공유할 수 있다는 점입니다. 
* 코드의 공유와 관리를 용의하게 하기 위해 우리가 git를 사용하는 것 처럼 도커에서도 사용자가 만든 이미지를 쉽게 공유할 수 있도록 **공식 저장소**를 제공하고 있습니다. 
  * https://hub.docker.com/
* 공식 저장소에는 이미 많은 사람들이 프론트 엔드 애플리케이션, 백엔드 애플리케이션과 같은 다양한 애플리케이션을 공유하고 있으며, 이러한 저장소를 registries라고 합니다. 
* docker registries는 개인PC에 설치하여 운영할 수 있으며, 공식 저장소 서비스를 활용하여 사용할 수 있습니다. 
* registries는 저장소의 모음이며 저장소는 이미 만들어진 코드를 제외하고는 GitHub 저장소와 같은 종류의 이미지 모음입니다. 
* registries 계정은 많은 repository를 생성할 수 있습니다. 

### Log in with your Docker ID

* 도커 계정이 없는 경우 [cloud.docker.com](https://cloud.docker.com/)에서 회원가입을 진행해야 합니다. 

  * 회원가입 이후 반드시 사용자 이름과 비밀번호를 기억하세요

  ```bash
  $ docker login
  
  Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
  Username: username
  Password:
  Login Succeeded
  ```

* 위와 같이 로컬 시스템에 Docker public registry에 로그인 합니다. 

### Tag the image

* 로컬 이미지를 레지스트리 저장소에 연결하는 표기법은 다음과 같습니다.

  * `username/repository:tag`
  * 태그는 선택사항이지만 Docker 이미지에 버전을 제공하는 데 사용하는 방법이기 때문에 권장합니다. 
  * repository에는 의미있는 이름을 붙여야 합니다.  `get-started:part2`
  * `get-started` 저장소에 저장되고 `part2` 라는 태그가 붙습니다.
  * 이제 이미지를 모두 태그를 추가하세요.
    *  실행 `docker tag image`사용자 이름, 저장소 및 태그 이름과 원하는 목적지로 이미지 업로드 있도록. 명령 구문은 다음과 같습니다. 

  ```bash
  $ docker tag image username/repository:tag
  ```

  * 예는 다음과 같습니다. 

  ```bash
  $ docker tag friendlyhello john/get-started:part2
  ```

* docker image ls 를 실행하면 새로운 태그가 추가된 이미지를 볼 수 있습니다. 

  ```bash
  $ docker image ls
  REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
  jusk2/get-started   part2               34bfc40dd349        2 minutes ago       151MB
  friendlyhello       latest              34bfc40dd349        2 minutes ago       151MB
  python              2.7-slim            46ba956c5967        4 weeks ago         140MB
  hello-world         latest              e38bc07ac18e        7 weeks ago         1.85kB
  ```

### Publish the image 

* 태그가 지정된 이미지를 저장소에 업로드 합니다. 

  ```bash
  $ docker push username/repository:tag
  ```

  ```bash
  $ docker push jusk2/get-started:part2
  The push refers to repository [docker.io/jusk2/get-started]
  6ea7fc018711: Pushed
  dce8439d1ac3: Pushed
  5936c79df190: Pushed
  a40d037570f2: Mounted from library/python
  4e1a46391216: Mounted from library/python
  10dd6271862c: Mounted from library/python
  ba291263b085: Mounted from library/python
  part2: digest: sha256:dc412133bb9f51bc8ff38f09514e680aab4489a13887dcc94ed090956f763f94 size: 1787
  ```

* 이미지가 정상적으로 업로드가 완료되면 위와같이 정상적으로 업로드가 완료되는 모습을 확인할 수 있습니다. 

* 또한 Docker Hub에서도 업로드된 이미지를 확인할 수 있습니다. 

### Pull and run the image from the remote repository

* 지금부터는 어디서는 docker run 명령을 통해서 어디서는 개인이 업로드한 도커 이미지를 사용할 수 있습니다. 

  ```bash
  $ docker run -p 4000:80 username/repository:tag
  ```

  * 개인 docker image 목록를 출력한 다음에 각자 만든 개인 이미지를 삭제하고 위의 명령을 실행하여 정상적으로 실행되는지 확인합니다. 

* 이미지가 로컬상에서 사용할 수 없는 환경이라면 Docker는 저장소에서 이미지를 가져와서 실행하게 됩니다. 

  * 이렇게 받은 이미지도 리스트를 확인할 수 있습니다. 

  ```bash
  $ docker run -p 4000:80 john/get-started:part2
  Unable to find image 'john/get-started:part2' locally
  part2: Pulling from john/get-started
  10a267c67f42: Already exists
  f68a39a6a5e4: Already exists
  9beaffc0cf19: Already exists
  3c1fe835fb6b: Already exists
  4c9f1fa8fcb8: Already exists
  ee7d8f576a14: Already exists
  fbccdcced46e: Already exists
  Digest: sha256:0601c866aab2adcc6498200efd0f754037e909e5fd42069adeff72d1e2439068
  Status: Downloaded newer image for john/get-started:part2
   * Running on http://0.0.0.0:80/ (Press CTRL+C to quit)
  ```



# Get Started, Part 3: Services

### Prerequisites 

* 설치된 Docker 버전이 1.13 이거나 그 이상 버전을 사용하고 있어야 합니다. 

* Docker Compose가 있어야 합니다. 

  * mac, windows10 사용자는 이미 설치되어 있습니다. 
  * 리눅스 사용자의 경우 직접 설치해야 합니다. 
    * 설치경로는 https://github.com/docker/compose/releases 을 참조하세요. 
  * 윈도우10 이전버전을 사용하는 사람은 Hyper-V, Docker Toolbox를 사용합니다. 

* 이번 장에서는 전에 배웠던 내용을 기본으로 하고 사용하기 때문에 이전 장을 학습하지 않은 분들은 먼저 학습하세요. 

* friendlyhello 공유된 이미지를 사용하기 때문에 이미지가 레지스트리로 푸시 되어있어야 합니다. 

* 이미지가 배포된 컨테이너가 사용가능한지 확인해야 합니다. 

  * `username`, `repo`,  `tag`: `docker run -p 80:80 username/repo:tag`,  `http://localhost/`.

  

### Introduction 

* 파트3 에서는 응용프로그램을 확장하고 로드밸런싱을 활성화 합니다. 

### About services

* 분산 응용 프로그램에서 응용 프로그램의 일부분을 "서비스"라고 지칭합니다. 
  * 예를 들어 비디어 공유사이트를 상상할 때, 데이터베이스에 사용자가 업로드하는 데이터를 저장하는 서비스, 비디오를 백그라운드에서 트랜스코딩하는 서비스, 프론트 엔드 서비스 등 
* 서비스는 "containers in production" 이라고 합니다. 즉 서비스는 하나의 이미지만 실행하지만 이미지를 실행하는 방법을 체계화 하는 것을 뜻합니다. 
  * 어떤 포트를 사용해야 하며, 얼마나 많은 컨테이너를 실행하지만 서비스를 안정적으로 유지할 수 있는지 등 입니다. 
* 서비스를 확장한다는 것은 해당 소프트웨어를 실행하는 컨테이너의 인스턴스 수가 변경되기 때문에 많은 컴퓨팅 리소스가 필요하게 됩니다. 



### Your first docker-compose.yml file

* YAML 파일은 도커 컨테이너의 행동과 방법을 정의할 수 있습니다. 

* 먼저 아래와 같이`docker-compose.yml` 파일을 작성합니다. 

  ```yaml
  version: "3"
  services:
    web:
      # replace username/repo:tag with your name and image details
      image: username/repo:tag
      deploy:
        replicas: 5
        resources:
          limits:
            cpus: "0.1"
            memory: 50M
        restart_policy:
          condition: on-failure
      ports:
        - "80:80"
      networks:
        - webnet
  networks:
    webnet:
  ```

* image 에서는 업로드한 이미지를 registry에서 당겨옵니다. 

* 해당 이미지의 인스턴스는 5개를 호출하며, CPU 사용율은 10%으로 제한, 메모리는 50MB를 사용하도록 합니다.

* 컨테이너가 실패하면 즉시 다시 시작하도록 합니다. 

* 호스트의 포트는 80번에 맵핑합니다. 

* 로드밸런싱 네트워크를 통해 포트 80을 공유하도록 webnet에 지시합니다. 

* webnet은 기본 네트워크 셋팅을 정의합니다. (로드밸런싱 네트워크)



### Run your new load-balanced app

* `docker stack deploy` 을 하기전에 다음 명령을 실행합니다. 

```bash
$ docker swarm init
```

> 위 명령의 의미는 PART 4장에서 자세히 살펴보도록 하겠습니다. 위의 명령을 실행하지 않으면 “this node is not a swarm manager.” 이라는 오류가 발생합니다. 

* 이제 실행하기 위해서 다음과 같은 명령을 실행합니다. 이름은 getstartedlab이라고 지정하겠습니다. 

  ```bash
  $ docker stack deploy -c docker-compose.yml getstartedlab
  Creating network getstartedlab_webnet
  Creating service getstartedlab_web
  ```

* 단일 서비스 스택은 배포된 이미지의 인스턴스 5개를 하나의 호스트에서 실행합니다. 

* 다음 명령어를 통해 실행되는 서비스를 확인해 보도록 하겠습니다. 

  ```bash
  $ docker service ls
  ID                 NAME           MODE   REPLICAS    IMAGE                  PORT
  zeipwcy1vteu getstartedlab_web replicated  5/ jusk2/get-started:part2   *:80->80/tcp
  ```

* 이 예제에서는 getstartedlab_web 이름으로 지정했으며, 서비스ID, 이미지 이름, 포트가 함께 나오게 됩니다. 

* 서비스에서 실행되는 단일 컨테이너를 **태스크** 라고합니다. 

* 작업에는 replicaas 정의 된 번호까지 숫자가 증가는 공유 ID가 제공됩니다. docker-compose.yml에서 서비스 작업을 나열하세요. 

  ```bash
  $ docker service ps getstartedlab_web
  ```

*   서비스에 의해 필터링 되지는 않지만 시스템의 모든 컨테이너를 나열하면 작업도 표시됩니다. 

  ```bash
  $ docker container ls -q
  fff615c7a277
  a4fded0d27a2
  c4ab51e827cc
  2ae57d6c1b22
  f7077779504d
  ```

* 웹브라우저에서 http://localhost를 실행하거나 curl을 통해서 Hostname을 확인합니다. 

  ```bash
  $ curl -4 http://localhost
  <h3>Hello World!</h3><b>Hostname:</b> a4fded0d27a2<br/><b>Visits:</b> <i>cannot connect to Redis, counter disabled</i>
  ```

* 어느 방식으로 실행하더라도 컨테이너 ID가 변경되서 로드 밸런싱합니다. 

* 각 요청은 라운드 로빈으로 동작합니다. 

  > **윈도우10를 사용하시나요?**
  >
  > 윈도우 10 PowerShell은 curl 사용이 가능해야 합니다. 하지만 그렇지 않을 경우 리눅스 터미널를 사용하거나 윈도우용 wget를 다운받아서 사용합니다. 

  >**응답시간이 느린가요? **
  >
  >네트워크 구성에 따라 컨테이너가 HTTP 요청에 응답하는데 최대 30초가 걸릴 수 있습니다.  하지만 이는 Docker 또는 Swarm 성능을 나타내지는 않습니다. 앞으로 진행할 튜토리얼을 통해서 Redis를 사용하게 될 것이며, 데이터를 유지하는 서비스를 추가하지 않았기 때문에 방문자 카운터는 동작하지 않을 것입니다. 



### Scale the app

* docker-compose.yml 파일에서 replicas 속성 값을 변경하고 저장하고 docker stack deploy 를 다시 시작하면 변경된 속성 값을 토대로 다시 실행합니다. 

  ```bash
  $ docker stack deploy -c docker-compose.yml getstartedlab
  ```

* Docker는 전체 업데이트를 수행하기 때문에 먼저 스택을 떼어 내거나 컨테이너를 제거할 필요가 없습니다. 

* 컨테이너를 재정렬하여 확인하면 배포된 인스턴스가 재구성되었는지 확인합니다. 

  * 더 많은 태스크가 생기거나 더 적은 태스크가 생기는 모습을 볼 수 있을 것입니다. 



### Take down the app and swarm 

* 실행중인 어플리케이션을 다음 명령어로 종료합니다. 

  ```bash
  $ docker stack rm getstartedlab
  ```

* swarm을 종료합니다. 

  ```bash
  $ docker swarm leave --force
  ```

* Docker를 사용하여 앱을 실행하고 확장할 수 있습니다. 

* 다음장에서는 Docker Swarm을 활용하여 클러스터링 하는 방법을 알아보도록 하겠습니다. 