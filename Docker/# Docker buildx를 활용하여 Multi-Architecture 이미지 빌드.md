# Docker buildx를 활용하여 Multi-Architecture 이미지 빌드



![](https://github.com/pandora0667/TILD/blob/master/screenshot/docker/cluster.png?raw=true)

​	타워형 12Layer 케이스에 PoE 스위치와 PoE HAT, 라즈베이파이 4 8GB 12개를 이용해서 Docker Cluster 시스템을 구축하게 되었다. 마지막 사진은 테스트하기 위해서 저렇게 둔거고 추후 랙에 들어갈 예정이었다. 하드웨어적으로는 준비가 끝나고 ubuntu 20.04과 Docker 설치를 완료하고 테스트를 하려는데 아래와 같은 에러가 날 맞이했다. 



```bash
$ ubuntu@main:~$ sudo docker run jusk2/topent
Unable to find image 'jusk2/topent:latest' locally
latest: Pulling from jusk2/topent
8559a31e96f4: Pull complete
bd517d441028: Pull complete
f67007e59c3c: Pull complete
83c578481926: Pull complete
f3cbcb88690d: Pull complete
181a23d9fada: Pull complete
22f247a94479: Pull complete
Digest: sha256:6224e39b484cb009cb3710d55b900e4940f6e2bc2c7a3078d51007343ae3145b
Status: Downloaded newer image for jusk2/topent:latest
standard_init_linux.go:211: exec user process caused "exec format error"
```

그저 아주 간단한 정적 홈페이지인데 모든 라즈베리파이에서 에러가 발생하기에 Docker 설치에 문제가 생겼는지 의아해서 hello-world를 실행했는데 역시 잘 동작한다. 

```bash
$ ubuntu@main:~$ sudo docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
256ab8fe8778: Pull complete
Digest: sha256:8c5aeeb6a5f3ba4883347d3747a7249f491766ca1caa47e5da5dfcf6b9b717c0
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (arm64v8)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

당연히 정상적으로 동작할 것이라는 내 생각을 뒤집고 동작을 하지 않으니 당혹감과 시간만 버린건가 이런생각이 들었지만 이런걸로 포기할 수 없기 때문에 원인을 찾아보기로 했다.



### 문제의 원인 

​	일단 정상적으로 실행하는 hello-world 이미지와 본인이 직접 빌드한 이미지의 정보를 확인해 보기로 했다. 

```bash
$ ubuntu@main:~$ sudo docker inspect jusk2/topent
....
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 174046322,
        "VirtualSize": 174046322,
....

$ ubuntu@main:~$ sudo docker inspect hello-world
....
        "Architecture": "arm64",
        "Os": "linux",
        "Size": 9136,
        "VirtualSize": 9136,
.... 
```

차근차근 이미지 정보를 확인하던 도중 아차싶었다. CPU Architecture 정보가 서로 다르게 표기된 것이었다. 본인 환경은 MacBook Pro 16 인치였고 실행은 ARM 64비트 기반의 라즈베리파이 4에서 테스트 하니 실행이 될리가 없었다. 아마 빌드부터 실행까지 라즈베리파이에서 진행했다면 아무 문제없이 실행이 됬을 것이다. 하지만 일일히 각 환경에서 빌드하고 테스트하는 것은 비효율적일 뿐만 아니라 Docker에서 이렇게 만들었지 않았을 것이다. 

​	구글에서 검색해보니 Docker에서 buildx를 통해서 크로스 컴파일을 제공하는 사실을 알 수 있었다. 



### buildx 

​	buildx는 현재 Docker 19.03.13에서는 기본적으로 제공하지 않는다. Experimental feature기능을 실행해야 사용할 수 있는데, Docker version 20.10.0-beta1에서는 바로 사용이 가능하다. 아마 차후 정식버전에서는 공식적으로 제공할 것 같다. brew로 간단하게 설치를 진행한다. 

```bash
$ brew install --cask docker-edge
```

```bash
$ docker buildx ls
NAME/NODE   DRIVER/ENDPOINT             STATUS  PLATFORMS
  default * default                     running linux/amd64, linux/arm64, linux/riscv64, linux/ppc64le, linux/s390x, linux/386, linux/arm/v7, linux/arm/v6
```

플랫폼 항목을 살펴보면 linux/amd64, linux/arm64, linux/riscv64, linux/ppc64le, linux/s390x, linux/386, linux/arm/v7, linux/arm/v6 아키텍처를 지원하는 것을 확인할 수 있다. 여기서 빌드할 타겟 플랫폼을 선택하고 빌드하면 각각의 플랫폼에 맞춰서 빌드를 진행해준다. 잠시 Docker 공식 문서에서 제공하는 buildx 명령어를 살펴본다. 

|         Command          |              Description               |
| :----------------------: | :------------------------------------: |
|    docker buildx bake    |           Build from a file            |
|   docker buildx build    |             Start a build              |
|   docker buildx create   |     Create a new builder instance      |
|     docker buildx du     |               Disk usage               |
| docker buildx imagetools | Commands to work on images in registry |
|  docker buildx inspect   |    Inspect current builder instance    |
|     docker buildx ls     |         List builder instances         |
|   docker buildx prune    |           Remove build cache           |
|     docker buildx rm     |       Remove a builder instance        |
|    docker buildx stop    |         Stop builder instance          |
|    docker buildx use     |    Set the current builder instance    |
|  docker buildx version   |    Show buildx version information     |

buildx를 사용할 때, 기본설정을 사용하면 정상적으로 진행이 되지 않기 때문에 Builder을 먼저 생성한다. 

```bash
$ docker buildx create --name lucas
$ docker buildx use lucas
$ docker buildx inspect --bootstrap
docker buildx inspect --bootstrap
[+] Building 4.6s (1/1) FINISHED
 => [internal] booting buildkit                                                                                                          4.5s
 => => pulling image moby/buildkit:buildx-stable-1                                                                                       4.0s
 => => creating container buildx_buildkit_lucas0                                                                                         0.6s
Name:   lucas
Driver: docker-container

Nodes:
Name:      lucas0
Endpoint:  unix:///var/run/docker.sock
Status:    running
Platforms: linux/amd64, linux/arm64, linux/riscv64, linux/ppc64le, linux/s390x, linux/386, linux/arm/v7, linux/arm/v6
```

이제 buildx를 사용할 기본 준비작업은 완료되었다. builder instance 확인을 통해서 등록된 정보를 확인할 수 있다. 

```bash
docker buildx ls
NAME/NODE   DRIVER/ENDPOINT             STATUS  PLATFORMS
lucas *     docker-container
  lucas0    unix:///var/run/docker.sock running linux/amd64, linux/arm64, linux/riscv64, linux/ppc64le, linux/s390x, linux/386, linux/arm/v7, linux/arm/v6 
default     docker
  default   default                     running linux/amd64, linux/arm64, linux/riscv64, linux/ppc64le, linux/s390x, linux/386, linux/arm/v7, linux/arm/v6
```



### 빌드하고 docker hub에 푸시하기 

​	먼저 아주 간단한 Dockerfile을 만들어서 테스트 한다. 

```bash
$ vi Dockerfile 
```

```dockerfile
FROM alpine
RUN apk add util-linux
CMD ["lscpu"]
```

```bash
$ docker buildx build --platform linux/amd64,linux/arm64,linux/arm/v7,linux/arm/v6 -t jusk2/demo-mutliarch --push .

[+] Building 45.8s (15/15) FINISHED
...
 => => pushing manifest for docker.io/jusk2/demo-mutliarch:latest    11.9s   
```

빌드시 각각 아키텍처에 맞게 전부 빌드를 진행하기 때문에 컴퓨터 성능이 다소 떨어지는 환경이라면 복잡한 애플리케이션일 수록 오래걸릴 수 있다. 또한 파일이름을 dockerfile로 하는 경우 에러가 발생하기 때문에 반드시 Dockerfile 이름으로 생성해야 한다. 이점은 꼭 유의하자. 

​	X86-64 아키텍처 시스템과 ARM64 아키텍처에서 각각 실행하면 다음과 같은 결과를 얻을 수 있다. 

```bash
$ docker run jusk2/demo-mutliarch
Unable to find image 'jusk2/demo-mutliarch:latest' locally
latest: Pulling from jusk2/demo-mutliarch
188c0c94c7c5: Pull complete
b13a60da1ab7: Pull complete
Digest: sha256:fdd732eafe78db01ce7d920d8f5bfdc2aba92034cb54221dc17231ec888a8965
Status: Downloaded newer image for jusk2/demo-mutliarch:latest
Architecture:                    x86_64
CPU op-mode(s):                  32-bit, 64-bit
Byte Order:                      Little Endian
Address sizes:                   39 bits physical, 48 bits virtual
CPU(s):                          8
On-line CPU(s) list:             0-7
Thread(s) per core:              1
Core(s) per socket:              1
Socket(s):                       8
Vendor ID:                       GenuineIntel
CPU family:                      6
Model:                           158
Model name:                      Intel(R) Core(TM) i9-9880H CPU @ 2.30GHz
```

```bash
$ sudo docker run jusk2/demo-mutliarch
Unable to find image 'jusk2/demo-mutliarch:latest' locally
latest: Pulling from jusk2/demo-mutliarch
5f621e34cdf4: Pull complete
3523a5ecea09: Pull complete
Digest: sha256:fdd732eafe78db01ce7d920d8f5bfdc2aba92034cb54221dc17231ec888a8965
Status: Downloaded newer image for jusk2/demo-mutliarch:latest
Architecture:                    aarch64
CPU op-mode(s):                  32-bit, 64-bit
Byte Order:                      Little Endian
CPU(s):                          4
On-line CPU(s) list:             0-3
Thread(s) per core:              1
Core(s) per socket:              4
Socket(s):                       1
Vendor ID:                       ARM
Model:                           3
Model name:                      Cortex-A72
```



### xpressengine Dockerfile buildx

​	docker hub에 접속하면 각 아키텍처별로 빌드되어 있는 이미지를 찾을 수 있다. 각 이미지는 아키텍처에 맞게 자동으로 다운로드 받으므로 이후 작업은 신경쓰지 않아도 된다. 다만 Dockerfile 작성시 베이스 이미지가 아키텍처를 지원하지 않는 경우 buildx로 빌드시 오류가 발생하며 다음으로 진행되지 않는다. 다만 대부분의 베이스 이미지는 ARM 아키텍처를 지원하기에 문제가 발생할 확률을 적을 수도 있지만 모든 이미지가 지원하지 않는다는 점은 숙지해야 한다. 

​	해당 베이스 이미지가 지원하지 않으면 본인이 전부 빌드작업을 수행해야 할 수도 있다. 이번에는 buildx를 통한 xpressengine을 직접 빌드하는 작업을 진행할 것이다. 아마 대부분 이 블로그를 보시는 분들이라면 Docker에 대한 기본 지식이 있을거라 바로 적용하실 수도 있겠지만,  다른 블로그의 내용이 간단하게만 설명되어 있어서 연습삼아 진행해도 좋을 것 같다. 

```bash
$ git clone https://git.wisoft.io/seongwon/modudeveloper.git
$ cd modudeveloper/docker
```

```bash
$ docker buildx build --platform linux/amd64,linux/arm64 -t jusk2/xpressengine . --push
[+] Building 1293.8s (22/22) FINISHED
....
> => pushing manifest for docker.io/jusk2/xpressengine:latest
```

빌드시간이 생각보다 오래 걸리기 때문에 여유를 가지고 기다리면 설치가 완료된다. 다음 링크로 접속해서 정상적으로 동작하는지 확인한다. 

```bash
$ docker run -p 8888:80 jusk2/xpressengine 
```



* http://localhost:8888/index.php/install 



​	참고로 라즈베리파이 3는 CPU 아키텍처가 armv7l 이다. 따라서 빌드 플랫폼을 linux/arm/v7로 해야한다는 점 기억한다. 









