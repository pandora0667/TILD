# Dockerfiles 작성 우수 사례 

* 본 문서는 효율적인 이미지를 구현하기 위해 권장되는 모범 사례와 방법을 다루는 문서입니다. 

* 도커의 `dockerfile`는 주어진 이미지를 만드는데 필요한 모든 명령을 순서대로 포함하고 있는 텍스트 파일로서, 각 명령을 읽어서 이미지를 자동으로 빌드합니다. 

* 도커의 이미지는 각 `dockerfile` 명령어를 나타내는 읽기전용 레이어로 구성되며, 레이어는 이전 레이어들이 겹쳐진 스택입니다. 

  ```bash
  FROM ubuntu:15.04
  COPY . /app
  RUN make /app
  CMD python /app/app.py
  ```

  * `FROM ubuntu:15.04` 우분투 15.04 도커 이미지에서 레이어를 생성합니다. 
  * `COPY` 도커 클라이언트의 현재 디렉토리를 생성합니다. 
  * `RUN` 응용프로그램을 빌드합니다. 
  * `CMD` 컨테이너 내에서 실행할 명령을 지정합니다. 

* 이미지를 실행하고 컨테이너를 생성 할 때 기본 레이어 위에 새로운 쓰기 레이어를 추가하는 구조로 되어 있습니다. 

* 새 파일을 작성하고, 기존 파일 수정 및 파일 제작과 같은 컨테이너의 모든 변경 사항이 쓰기 레이어에 추가됩니다. 



### General guideliens and recommendations 

####Create ephemeral containers

* 반드시 기억해야 하는 점은 많은 사용자들이 일반적인 가상 시스템으로 컨테이너를 처리하고 있다는 점입니다. 

* 이러한 사고방식으로 컨테이너를 생성하게 되면 컨테이너의 중요한 이점과 특성을 잊어버리게 됩니다. 

* 컨테이너는 ephemeral(임시/일회용)이라는 사실을 기억하고 있어야 합니다. 

* ephemeral이라는 것은 컨테이너를 정지, 삭제하고 새롭게 빌드할 때 최소한의 설정으로 대체할 수 있다는 것을 의미합니다. 

* 이러한 방식의 컨테이너 운영 지침을 자세하게 이해하고 싶다면 *The 12-factor App* 방법론의 **프로세스**를 참조하세요.

  * *The 12-factor App* 는 소프트웨어를 서비스 형태로 제공하는 것이 일반화 되면서 나오게 된 방법론 입니다. 
  * 우리는 이 방법론을 통해 SaaS 앱을 만들 수 있습니다.  
  * 자동화를 통한 개발자의 시간과 비용을 최소화 하고, OS에 따라 변경되는 부분을 명확히 하여 이식성을 극대화 합니다. 또한 최근 클라우드 시스템의 등장으로 서버와 시스템의 관리가 필요가 없어짐에 따라 개발환경과 운영환경의 차이를 줄이고 지속적인 배포가 가능하게 되었습니다. 
  * 이러한 방법론은 SaaS 형식의 애플리케이션을 개발하고자 하는 모든 개발자가 읽어야 하며,  이를 관리하는 엔지니어가 대상입니다. 여기서는 12가지 방법 중 5번 프로세스에 대해 알아보겠습니다. 

  >### Execute the app as one or more stateless processes
  >
  >앱은 하나 이상의 프로세스로 실행 환경에서 실행된다고 할 수 있습니다. 
  >
  >가장 간단한 경우로 예를 들자면, 코드는 독립 실행 가능한 스크립트 형태이며, 실행 환경은 런타임이 설치된 개발자의 PC가 될 수 있습니다. 이러한 환경에서 개발자는 python my_script.py와 같은 형식으로 실행할 수 있습니다.
  >
  >12-factor 프로세스는 무상태이며 아무것도 공유하지 않습니다. 만약 유지되야 하는 어떠한 데이터가 있다면 우리는 데이터베이스를 활용해야 할 것입니다. 
  >
  >짧은 단일 트랙잭션 내에서 캐시로 프로세스의 메모리 공간이나 파일시스템을 사용해도 됩니다. 예를 들자면 큰 파일을 받고, 해당 파일을 처리하고, 그 결과를 데이터베이스에 저장하는 경우가 있습니다.
  >
  >*The 12-factor App* 방법론에서  앱이란 각 유형의 많은 프로세스가 실행하고 있기 때문에 향우 요청이나 작업이 다른 프로세스에 의해서 처리될 가능성이 크기 때문에 메모리나 디스크에 캐시된 항목을 사용할 수 있다고 가정하지 않습니다.  만약 프로세스를 하나만 실행하는 경우에도 재시작(OS상에서 이루어지는 메모리 및 파일 시스템 재배치 작업)되면 로컬상의 데이터는 일반적으로 모두 지워집니다. 
  >
  >django-asset packager 와 같은 Asset패키지 관리자는 컴파일된 Asset을 저장한 캐시로 파일 시스템을 사용합니다. *The 12-factor App* 에서는 이러한 컴파일을 런타임이 아니라 빌드 단계에서 사용하는 것을 선호합니다. 
  >
  >일부 웹 시스템에서는 "sticky sessions"을 사용하는데 이는 애플리케션이 세션 데이터를 캐싱하여,  향후 똑같은 사용자가 접속했을 경우 같은 프로세스로 라우팅 되도록 할 수 있습니다. 하지만 이러한 방법은 *The 12-factor* 에 위배되는 행위 이기 때문에 사용하면 안되며, Redis처럼 유효기간을 제공하는 데이터 저장소에 저장하는 것이 적합합니다. 

  

#### Understanding build context 

* `docker run` 명령을 실행하면 현재 작업 디렉토리를 build context라고 합니다. 

* 기본적으로 도커파일은 여기에 있다고 가정하지만 `-f` 옵션을 통해서 다른 위치를 지정할 수 있습니다. 

* 도커파일은 실제 위치에 상관없이 현재 디렉토리에 있는 파일과 모든 디렉토리의 내용이 도커데몬으로 전송됩니다.  

* 아래의 build context 예제를 실행해서 확인하세요. 

  ```bash
  $ mkdir myproject && cd myproject
  $ echo "hello" > hello
  $ echo -e "FROM busybox\nCOPY /hello /\nRUN cat /hello" > Dockerfile
  $ ls
  Dockerfile	hello
  $ docker build -t helloapp:v1 .
  ```

  * 도커파일과 hello 파일을 다른 디렉토리로 이동하고 마지막 빌드의 캐시에 의존하지 않고 `-f` 옵션을 사용하여 도커파일을 가리키고 build context 디렉토리를 지정합니다. 

  ```bash
  $ mkdir -p dockerfiles context
  $ mv Dockerfile dockerfiles && mv hello context
  $ docker build --no-cache -t helloapp:v2 -f dockerfiles/Dockerfile context
  ```

* 실수로 이미지를 빌드하는데 불필요한 파일을 포함하면 build context와 이미지가 커지게 됩니다. 

* 또한 이미지를 빌드하는 시간과 런타임 시간이 증가될 수 있습니다. 

* build context 파일이 얼마나 큰지 보려면 빌드할 때 다음과 같은 메시지를 찾으세요. 

  ```bash
  Sending build context to Docker daemon  187.8MB
  ```



---

* 테스트로 실행했던 모든 컨테이너와 이미지를 삭제하고 싶다면 다음 명령을 실행하세요. 

  ```bash
  $ docker stop $(docker ps -a -q)
  $ docker rm $(docker ps -a -q)
  $ docker rmi $(docker images -q)
  ```

---



#### Pipe Dockerfile though  `stdin`

* 도커 17.05 이상 부터는 로컬, 원격 build context를 도커파일 파이프를 통해 이미지를 빌드하는 기능이 추가되었습니다. 

* 이전 버전에서는 from을 사용해여 이미지를 빌드하면 build context가 전송되지 않습니다. 

  * 로컬 build context

  ```bash
  $ docker build -t foo . -f-<<EOF
  FROM busybox
  RUN echo "hello world"
  COPY . /my-copied-files
  EOF
  ```

  * 원격 build context

  ```bash
  $ docker build -t foo https://github.com/thajeztah/pgadmin4-docker.git -f-<<EOF
  FROM busybox
  COPY LICENSE config_local.py /usr/local/lib/python2.7/site-packages/pgadmin4/
  EOF
  ```

#### Exclude with .dockerignore

* 빌드와 관련없는 파일을 제거하려면 `.dockerignore` 을 사용하세요. 

* 예제는 다음과 같습니다. 

  ```
  # comment
  */temp*
  */*/temp*
  temp?
  ```



#### Use muliti-stage bulids 

* muliti-stage bulids 는 도커 17.05 이상부터 사용할 수 있습니다. 

* 이미지 빌드에 대한 가장 어려운 점 중 하나는 이미지 크기를 줄이는 것입니다. 

* 도커파일의 각 명령은 이미지에 레이어를 추가하므로 다음 레이어로 이동하기 전에 필요하지 않은 이슈를 정리해야합니다. 

* 매우 효율적인 도커파일을 작성하기 위해서는 전통적으로 레이어를 가능한 한 작게 유지하고 각 레이어가 이전 레이어에서 필요로하는 아티팩트를 갖도록하고 다른 것은 필요 없도록 쉘 트릭 및 기타 로직을 사용해야했습니다.

* 실제로 애플리케이션을 구축하는 데 필요한 모든 것이 포함된 하나의 도커파일을 개발하고 운영에 사용하기 위한 하나의 Dockerfile을 만드는 것이 매우 일반적이었습니다.

* 이것을 "빌더 패턴"이라고합니다. 하지만 두 개의 Dockerfiles를 유지 관리하는 것은 이상적이지 않습니다.

* 다음 `Dockerfile.build`와 `Dockerfile` 빌더 패턴을 준수합니다. 

  ```bash
  FROM golang:1.7.3
  WORKDIR /go/src/github.com/alexellis/href-counter/
  COPY app.go .
  RUN go get -d -v golang.org/x/net/html \
    && CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o app .
  ```

  * 이 예제는 `RUN` `&&`연산자를 사용하여 두 개의 명령을 인위적으로 압축 하여 이미지에 추가 레이어가 생성되지 않도록합니다. 

  ```bash
  FROM alpine:latest  
  RUN apk --no-cache add ca-certificates
  WORKDIR /root/
  COPY app .
  CMD ["./app"] 
  ```

  ```bash
  #!/bin/sh
  echo Building alexellis2/href-counter:build
  
  docker build --build-arg https_proxy=$https_proxy --build-arg http_proxy=$http_proxy \  
      -t alexellis2/href-counter:build . -f Dockerfile.build
  
  docker container create --name extract alexellis2/href-counter:build  
  docker container cp extract:/go/src/github.com/alexellis/href-counter/app ./app  
  docker container rm -f extract
  
  echo Building alexellis2/href-counter:latest
  
  docker build --no-cache -t alexellis2/href-counter:latest .
  rm ./app
  ```

* `build.sh` 스크립트를 실행하면 첫 번째 이미지를 작성하고 컨테이너를 작성하여 이슈를 복사 한 다음 두 번째 이미지를 빌드해야합니다.

*  두 이미지 모두 시스템의 공간을 차지하고 `app` 로컬 디스크에도 아티팩트가 남아 있습니다 .

* 위의 문제점을 파악하고 아래와 같이 muliti-stage bulid을 사용하면 이러한 문제점을 완벽하게 해결할 수 있습니다. 

  ```bash
  FROM golang:1.7.3
  WORKDIR /go/src/github.com/alexellis/href-counter/
  RUN go get -d -v golang.org/x/net/html  
  COPY app.go .
  RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o app .
  
  FROM alpine:latest  
  RUN apk --no-cache add ca-certificates
  WORKDIR /root/
  COPY --from=0 /go/src/github.com/alexellis/href-counter/app .
  CMD ["./app"]  
  ```

* 다음과 같이 muliti-stage bulid는 도커파일에서 여러 문장을 사용하는 것을 허용하며, 각 `FROM` 명령은 다른 기준을 가지고 새로운 빌드를 시작합니다. 

* 이 파일을 실행하고자 할 때는 다음과 같습니다. 

  ```bash
  $ docker build -t alexellis2/href-counter:latest .
  ```

* 아래는 Go를 도커파일로 작성한 것으로 분석해보고 실행해 보시길 바랍니다.  

  * COPY는 파일을 이미지에 추가하는 명령어 입니다. 

  * scratch는 도커에서 빈 베이스 이미지를 뜻하며, scratch 이미지라고 합니다. 

    * scratch 이미지에는 아무것도 없기 때문에 컨테이너가 생성되지 않으며 보통 기본 이미지로 많이 사용합니다. 

    * 다음을 실행해서 확인해 보세요. 

      ```bash
      FROM scratch
      ADD hello /
      CMD ["/hello"]
      ```

      ```bash
      $ docker build --tag hello .
      ```

  ```bash
  FROM golang:1.9.2-alpine3.6 AS build
  
  # Install tools required for project
  # Run `docker build --no-cache .` to update dependencies
  RUN apk add --no-cache git
  RUN go get github.com/golang/dep/cmd/dep
  
  # List project dependencies with Gopkg.toml and Gopkg.lock
  # These layers are only re-built when Gopkg files are updated
  COPY Gopkg.lock Gopkg.toml /go/src/project/
  WORKDIR /go/src/project/
  # Install library dependencies
  RUN dep ensure -vendor-only
  
  # Copy the entire project and build it
  # This layer is rebuilt when a file changes in the project directory
  COPY . /go/src/project/
  RUN go build -o /bin/project
  
  # This results in a single layer image
  FROM scratch
  COPY --from=build /bin/project /bin/project
  ENTRYPOINT ["/bin/project"]
  CMD ["--help"]
  ```



#### Decouple applications

* 각 컨테이너에는 하나의 작업만 있어야 합니다. 응용프로그램을 여러 컨테이너로 분리하면 재사용성이 올라갑니다. 
* 예를 들어 웹 애플리케이션의 스택은 3개의 컨테이너로 분리되어 웹 어플리케이션, 데이터베이스, 캐시관리로 운영할 수 있습니다. 
* 각 컨테이너를 하나의 프로세스로 제한하는 것은 좋은 선택일 수도 있으나 상황에 따라 선택해야 합니다.
* 예를 들어 Celery(Distributed Task Queue)는 여러 작업 프로세스를 생성할 수 있으며, Apache는 요청당 프로세스를 생성할 수 있습니다. 
* 각 컨테이너가 서로 의존 관계에 있는 경우 도커 컨테이너 네트워크를 사용하여 컨테이너가 통신할 수 있도록 할 수 있습니다. 이렇게 모듈 컨테이너를 잘 관리하기 위해서는 개발자의 선택이 중요하다고 할 수 있습니다. 



#### Sort multi-line arguments

* 가능하면 다음과 같이 정렬하는 것을 추천합니다. 

* 이러한 방식은 패키지의 중복을 피하고 쉽게 업데이트 할 수 있으며 가독성을 올려줍니다. 

  ```
  RUN apt-get update && apt-get install -y \
    bzr \
    cvs \
    git \
    mercurial \
    subversion
  ```




#### Leverage build cache

* 이미지를 만들 때 Docker는 사용자의 지침을 단계별로 `Dockerfile`실행하여 지정된 순서대로 실행합니다.
* 각 명령을 검사 할 때 Docker는 새이미지를 만드는 대신 캐시에서 기존 이미지를 찾아서 재사용 합니다. 
* 캐시를 전혀 사용하지 않으려면 명령에 `--no-cache=true` 옵션을 사용할 수 있습니다. 



# Dockerfile instructions

* 도커파일을 작성할 때 다음 지침을 따를 것을 권장합니다. 
* `FROM` 명령어에서 가능하면 공식 저장소 이미지를 기초로 사용할 것을 강력히 권장합니다. 
  * 공식저장소에서 제공되는 이미지는 크기가 작고 완벽한 리눅스 배포판입니다. 

#### LABLE

* 이미지에 레이블을 추가하여 프로젝트 별 이미지 구성, 라이센스 정보 기록, 자동화 지원 또는 기타 이유로 도움을 받을 수 있습니다. 

* 각 레이블에 `LABEL`하나 이상의 키 - 값 쌍이 있는 행을 추가하십시오 . 

* 다음 예제는 서로 다른 수용 가능한 형식을 보여줍니다. 설명 주석은 인라인에 포함됩니다.

  ```bash
  # Set one or more individual labels
  LABEL com.example.version="0.0.1-beta"
  LABEL vendor1="ACME Incorporated"
  LABEL vendor2=ZENITH\ Incorporated
  LABEL com.example.release-date="2015-02-12"
  LABEL com.example.version.is-production=""
  ```

  ```bash
  # Set multiple labels on one line
  LABEL com.example.version="0.0.1-beta" com.example.release-date="2015-02-12"
  ```

  ```bash
  # Set multiple labels at once, using line-continuation characters to break long lines
  LABEL vendor=ACME\ Incorporated \
        com.example.is-beta= \
        com.example.is-production="" \
        com.example.version="0.0.1-beta" \
        com.example.release-date="2015-02-12"
  ```



#### RUN 

#####APT-GET

* 일반적으로 가장 많이 사용되는 `RUN` 의 응용 프로그램입니다. 

* 패키지를 설치할 때 몇가지 주의할 점이 있으므로 반드시 숙지해야 합니다. 

* 만약 다음과 같은 도커 파일이 있다고 가정하고 문제점을 알아보도록 하겠습니다. 

  ```bash
      FROM ubuntu:14.04
      RUN apt-get update
      RUN apt-get install -y curl
  ```

* 이미지를 만들고나면 모든 레이어는 도커 캐시에 있습니다. 

* 만약 추가할 패키지가 있어서 아래와 같이 작성하면 어떻게 될까요? 

  ```bash
      FROM ubuntu:14.04
      RUN apt-get update
      RUN apt-get install -y curl nginx
  ```

* 도커는 초기버전과 수정된 명령어가 동일하다고 판단하여 이전 캐시를 재사용합니다. 

* 결과적으로 `apt-get update` 가 실행되지 않기 때문에 빌드시 잠재적으로 구형 패키지가 사용될 수 있습니다. 

* `RUN apt-get update && apt-get install -y` 을 사용 하면 코딩이나 수동 개입없이 최신 패키지 버전을 설치할 수 있습니다. 이 기법을 **캐시 무효화**라고합니다.

  ```bash
      RUN apt-get update && apt-get install -y \
          package-bar \
          package-baz \
          package-foo=1.3.*
  ```

* 위와 같이 버전을 지정하면 빌드의 내용과 상관없이 특정 버전을 검색하도록 할 수 있습니다. 

* 이러한 방법을 통해 예기치 못한 패키지 변경으로 인하여 생기는 문제점을 사전에 예방할 수 있습니다. 

* 다음 예제는 권장하는 방법입니다. 

  ```bash
  RUN apt-get update && apt-get install -y \
      aufs-tools \
      automake \
      build-essential \
      curl \
      dpkg-sig \
      libcap-dev \
      libsqlite3-dev \
      mercurial \
      reprepro \
      ruby1.9.1 \
      ruby1.9.1-dev \
      s3cmd=1.1.* \
   && rm -rf /var/lib/apt/lists/*
  ```

* `rm -rf /var/lib/apt/lists/*` 을 사용하면 `/var/lib/apt/lists` 에 저장되어 있는 apt 캐시가 지워지기 때문에 이미지 크기가 줄어듭니다. 



# 실습 

* 지금까지 배운 내용을 가지고 연습하는 시간을 가지도록 하겠습니다. 

* 오픈소스 프로그램 중 rocket.chat이라는 오픈소스 채팅 프로그램이 있습니다. 

  ![](https://cdn-images-1.medium.com/max/1920/1*tXPdgFYMLD-SXLzfyT5w0A.png)

* 이 프로그램을 직접 로컬머신에 업로드 하여 실행해 보세요. 

* 직접 도커파일을 만들지 않고도 지금까지 배운 도커 명령만으로 간단하게 실행할 수 있습니다. 

* 정상적으로 만들어 보았다면 swarm을 사용하여 응용 프로그램을 실행해 보세요.

* 먼저 상용 프로그램을 도커로 운영할 수 있는 실습능력을 갖추어 보고 실제 어플리케이션을 만들고 운영해 보겠습니다. 

