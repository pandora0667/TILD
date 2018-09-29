# Docker Start

![](https://logz.io/wp-content/uploads/2016/01/docker-facebook.png)

저번 시간에는 Docker가 무엇인지 핵심 기능인 컨테이너 기반의 가상화가 무엇인지 살펴보았다. 

이번시간에는 Docker를 설치하고 공식 Documents에 있는 명령을 실제로 실행하면서 Docker에 대해 이해도를 가지는 시간을 가지도록 한다. 



> 본 문서는 Mac OS X를 기반으로 작성하였다. Linux 사용자나 Windows 사용자는 설치방법이 각각 상의하기 때문에 공식 문서를 읽어보고 설치를 진행하는 것을 권장한다. 



### Mac OS X 에서 Docker 설치하기 

```bash
$ brew update && brew upgrade && brew cask upgrade && brew cleanup && brew cask cleanup
$ brew search docker
$ brew cask install docker
```

* brew를 사용하여 Docker를 설치하고 Terminal를 열어서 Docker가 정상적으로 설치되어있는지 확인한다.

```bash
$ docker --version
Docker version 18.03.1-ce, build 9ee9f40

$ docker info
Containers: 1
 Running: 0
 Paused: 0
 Stopped: 1
Images: 1
Server Version: 18.03.1-ce
...
```

* 명령어가 실행되지 않는다면 Docker를 실행되고 있는지 확인한다.
  * Mac의 경우 상단에 Docker 모양이 있으면 된다. 

### Test Docker Installation 

```bash
$ docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/engine/userguide/
```

```bash
$ docker image ls
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
hello-world         latest              e38bc07ac18e        5 weeks ago         1.85kB
```

```bash
$ docker container ls -all
CONTAINER ID        IMAGE               COMMAND             CREATED              STATUS                          PORTS               NAMES
94b1c78c6373        hello-world         "/hello"            About a minute ago   Exited (0) About a minute ago                       xenodochial_einstein
```



### Docker로 앱 만들어 보기 

* Docker를 활용하여 간단한 웹 애플리케이션을 만들어 사용한다. 
* 파이썬을 활용하여 어플리케이션을 만들며, 본 프로젝트를 실행하기 위해 빈 디렉토리를 만들어서 테스트 한다. 

----

* 빈 디렉토리에 Dockerfile를 만들고 다음 내용을 작성한다. 

  ```
  # Use an official Python runtime as a parent image
  FROM python:2.7-slim
  
  # Set the working directory to /app
  WORKDIR /app
  
  # Copy the current directory contents into the container at /app
  ADD . /app
  
  # Install any needed packages specified in requirements.txt
  RUN pip install --trusted-host pypi.python.org -r requirements.txt
  
  # Make port 80 available to the world outside this container
  EXPOSE 80
  
  # Define environment variable
  ENV NAME World
  
  # Run app.py when the container launches
  CMD ["python", "app.py"]
  ```

* requirements.txt를 만들고 다음과 같이 작성한다. 

  ```bash
  Flask
  Redis
  ```

* app.py를 만들고 다음과 같이 작성한다. 

  ```python
  from flask import Flask
  from redis import Redis, RedisError
  import os
  import socket
  
  # Connect to Redis
  redis = Redis(host="redis", db=0, socket_connect_timeout=2, socket_timeout=2)
  
  app = Flask(__name__)
  
  @app.route("/")
  def hello():
      try:
          visits = redis.incr("counter")
      except RedisError:
          visits = "<i>cannot connect to Redis, counter disabled</i>"
  
      html = "<h3>Hello {name}!</h3>" \
             "<b>Hostname:</b> {hostname}<br/>" \
             "<b>Visits:</b> {visits}"
      return html.format(name=os.getenv("NAME", "world"), hostname=socket.gethostname(), visits=visits)
  
  if __name__ == "__main__":
      app.run(host='0.0.0.0', port=80)
  ```

  ```bash
  $ ls
  app.py dockerfile requirements.txt
  ```

* 파일을 다 만들었으면 위와 같이 3가지 파일이 있는지 확인한다. 

* 이제 빌드명령을 실행하여 Docker 이미지를 생성한다. 

  * -t 옵션은 친숙한 이름으로 지정하는 옵션이다. 

  ```bash
  $ docker build -t friendlyhello .
  ```

* 다음 명령어를 사용하여 앱을 실행한다. 

  * -p 옵션은 기기의 포트 4000번을 컨테이너의 게시된 80번에 매핑하여 실행하도록 한다. 

  ```bash
  $ docker run -p 4000:80 friendlyhello
  ```

  ![](https://docs.docker.com/get-started/images/app-in-browser.png)

* 앱을 종료할때는 CTRL + C로 종료한다. 

  

  >윈도우는 CTRL + C로 종료하지 않는다. 
  >
  >Windows 시스템에서는 다른 터미널을 열고 'docker container ls' 를 통해 실행 중인 컨테이너 목록을 확인한 다음 컨테이너 'docker container stop  <Container NAME or ID>' 를 통해 중지한다. 
  >
  >이렇게 종료하지 않을 경우 컨테이너를 다시 실행하려고 시도할 때 데몬에서 오류 응답을 받는다. 



* 백그라운드에서 어플리케이션을 실행할때는 다음과 같이 실행한다. 

  ```bash
  $ docker run -d -p 4000:80 friendlyhello
  ```

* 앱에 대한 컨테이너 ID를 얻기위해서 다음 명령어를 실행한다. 

  ```bash
  $ docker container ls
  CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                  NAMES
  f6ffb3c312f7        friendlyhello       "python app.py"     11 seconds ago      Up 11 seconds       0.0.0.0:4000->80/tcp   keen_kirch
  ```

* 실행중인 컨테이너를 종료하기 위해 다음 명령어를 실행한다. 

  ```bash
  $ docker container stop f6ffb3c312f7
  f6ffb3c312f7
  ```



## Docker 사용해 보기 

* Docker의 명령어는 docker run, docker push 와 같이 docker <명령> 형식으로 실행한다. 

  * 리눅스 사용자의 경우 반드시 root 권한으로 실행해야 한다. 

* search 명령어를 활용하여 이미지를 검색한다. 

  ```bash
  $ docker search ubuntu
  NAME                                                      DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
  ubuntu                                                    Ubuntu is a Debian-based 
  
  ...
  ```

* 다양한 이미지가 검색이 되는데 공식 명칭을 가진 OS나 프로그램이 공식 이미지이다. 

* 나머지는 개발자들이 직접 만들어 배포한 사설 이미지이다. 

* pull 명령어로 이미지를 받기 위해서는 다음과 같은 명령어를 사용한다. 

  ```bash
  $ docker pull ubuntu:latest
  latest: Pulling from library/ubuntu
  a48c500ed24e: Pull complete
  1e1de00ff7e1: Pull complete
  0330ca45a200: Pull complete
  471db38bcfbf: Pull complete
  0b4aba487617: Pull complete
  Digest: sha256:c8c275751219dadad8fa56b3ac41ca6cb22219ff117ca98fe82b42f24e1ba64e
  ```

* images 명령어로 이미지 목록을 출력한다. 

  ```bash
  $ docker images
  REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
  friendlyhello       latest              af7ce11cd9ba        34 minutes ago      151MB
  python              2.7-slim            46ba956c5967        2 weeks ago         140MB
  ubuntu              latest              452a96d81c30        3 weeks ago         79.6MB
  hello-world         latest              e38bc07ac18e        5 weeks ago         1.85kB
  ```

* run 명령어으로 컨테이너를 생성한다. 

  ```bash
  $ docker run -i -t --name hello ubuntu /bin/bash
  root@f745dc7699ed:/#
  ```

  * Docker run <옵션> <이미지 이름> <실행할 파일> 형식으로 실행한다. 
  * 본 예제에는 ubuntu 이미지를 컨테이너로 생성한 뒤에 ubuntu 이미지 안에 /bin/bash를 실행한다. 
  * 이미지 이름 대신에 이미지 ID를 사용해도 된다. 
  * bash에서 빠져나가기 위해서는 exit를 입력한다. 
  * 우분투 이미지에서 /bin/bash 실행파일을 직접 실행했기 때문에 빠져나오게 되면 컨테이너가 정지된다. 

* start 명령으로 컨테이너 시작 

  ```bash
  $ docker start hello
  ```

* restart 명령으로 컨테이너 재시작 

  ```bash
  $ docker restart hello
  ```

* attach 명령으로 컨테이너에 접속

  ```bash
  $ docker attach hello
  root@f745dc7699ed:/#
  ```

* exec 명령으로 외부에서 컨테이너 안의 명령어 실행 

  ```bash
  $ docker exec hello echo "wisoft hello"
  wisoft hello
  ```

* stop 명령으로 컨테이너 정지 

  ```bash
  $ docker ps
  CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
  f745dc7699ed        ubuntu              "/bin/bash"         8 minutes ago       Up About a minute                       hello
  
  $ docker stop hello
  hello
  
  $ docker ps
  CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
  ```



> 본 명령어는 이미지 ID로 대체하여 작성해도 상관없이 동작한다. 



* rm 명령으로 컨테이너 삭제

  ```bash
  $ docker rm hello
  hello
  ```

* rmi 명령으로 이미지 삭제 

  ```bash
  $ docker rmi ubuntu:latest
  Untagged: ubuntu:latest
  Untagged: ubuntu@sha256:c8c275751219dadad8fa56b3ac41ca6cb22219ff117ca98fe82b42f24e1ba64e
  Deleted: sha256:452a96d81c30a1e426bc250428263ac9ca3f47c9bf086f876d11cb39cf57aeec
  Deleted: sha256:96fccbf869d3c0ee0fb2e976fdf356dc5872f6410030fd094bbc5b34a7559cdb
  Deleted: sha256:38ffa1479cb9fd81d0d4d057c282a155a4a83bff5d2b507ee9563f996d74272d
  Deleted: sha256:cc6967c5525a55626688a773e4fe578321a2e126a3b1df1bc0763cfd1583c50c
  Deleted: sha256:2a2d486f02032f5a6cc56290a244512daa07a8efe0124bccc5701f0a778aa947
  Deleted: sha256:65bdd50ee76a485049a2d3c2e92438ac379348e7b576783669dac6f604f6241b
  
  $ docker images
  REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
  friendlyhello       latest              af7ce11cd9ba        About an hour ago   151MB
  python              2.7-slim            46ba956c5967        2 weeks ago         140MB
  hello-world         latest              e38bc07ac18e        5 weeks ago         1.85kB
  ```