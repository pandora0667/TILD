# Docker Tutorials and Labs

* 지금까지 도커 공식 문서에 있는 내용을 살펴보면서 도커의 사용법과 도커의 동작 원리 등을 살펴보았습니다. 
* 지금부터는 도커에서 공식적으로 제공하고 있는 *Docker Tutorials and Labs* 를 참조하면서 실제 실행하면서 사용하는 방법을 알아보도록 하겠습니다. 
* 다음 깃허브 주소를 클론하세요. 
  * https://github.com/docker/labs.git
* 위의 저장소에서는 도커를 사용하기 위한 다양한 예제와 설명들이 첨부되어 있습니다. 이 문서또한 위의 설명을 기본으로 작성 되었음을 알려드립니다. 



>```bash
>$ docker stop $(docker ps -a -q)
>$ docker rm $(docker ps -a -q)
>$ docker rmi $(docker images -q)
>```
>
>* 먼저 위와 같은 명령을 통해 지금까지 테스트로 실행했던 모든 도커파일을 정리해 주세요. 



### Docker for Beginners

* 먼저 다운로드 받은 깃허브 파일에서 다음 디렉토리로 이동합니다. 

  ```bash
  $ cd labs/beginner
  $ ls -l
  total 8
  drwxr-xr-x  6 seongwonlee  staff   192B  7  7 20:44 chapters
  drwxr-xr-x  6 seongwonlee  staff   192B  7  7 20:44 flask-app
  drwxr-xr-x  8 seongwonlee  staff   256B  7  7 20:44 images
  -rw-r--r--  1 seongwonlee  staff   270B  7  7 20:44 readme.md
  drwxr-xr-x  4 seongwonlee  staff   128B  7  7 20:44 static-site
  $ cd static-site
  ```

* static-site 디렉토리를 살펴보면 Dockerfile과 Hello_docker.html 이 있는 것을 확인할 수 있습니다. 

* Dockerfile의 내용은 다음과 같습니다. 

  ```bash
  FROM nginx
  ENV AUTHOR=Docker
  
  WORKDIR /usr/share/nginx/html
  COPY Hello_docker.html /usr/share/nginx/html
  
  CMD cd /usr/share/nginx/html && sed -e s/Docker/"$AUTHOR"/ Hello_docker.html > index.html ; nginx -g 'daemon off;'
  ```

* 간단하게 nginx를 활용하여 정적 HTML 파일을 서비스하는 모습을 볼 수 있습니다. 

* 다음 명령어를 통해 정적 파일을 실행해 보겠습니다.

  ```bash
  $ docker run -d dockersamples/static-site
  ```

* 명령어를 살펴보면 먼저 로컬의 저장소를 확인하고 만약의 이미지를 찾을 수 없다면 원격 저장소에서 이미지를 다운받는 모습을 확인할 수 있습니다. 

  ```bash
  $ docker ps
  CONTAINER ID        IMAGE                       COMMAND                  CREATED              STATUS              PORTS               NAMES
  2164446d2736        dockersamples/static-site   "/bin/sh -c 'cd /usr…"   About a minute ago   Up About a minute   80/tcp, 443/tcp     nervous_cori
  ```

* 위에서 컨테이너 아이디를 알아내고 컨테이너를 종료해보도록 하겠습니다. 

  ```bash
  $ docker stop 2164446d2736
  $ docker rm 2164446d2736
  ```

* 아래 명령어를 통해서 매개변수의 내용을 확인하고 detached 모드로 다시 컨테이너를 실행해 보겠습니다. 

  ```bash
  $ docker run --name static-site -e AUTHOR="Your Name" -d -P dockersamples/static-site
  b2f7a27670d4f8cda3f8e0b85f8bee6d5d9f9754757e72cb07d28eb0afc09b16
  ```

  * 위에서 사용한 옵션의 설명은 다음과 같습니다. 
    * -d : 백그라운드에서 어플레케이션을 실행합니다. 이를 detached 모드라고 합니다. 
    * -p : 컨테이너 포트를 Docker 호스트의 임의의 포트와 맵핑합니다. 
    * -e : 환경변수를 컨테이너에 전달하는 방법입니다. 
    * --name : 컨테이너의 이름을 지정할 수 있습니다. 
    * AUTHOR : 환경변수 이름이며 위에서 이름을 전달하였습니다. 

* 아래 명령어를 통해 도커가 어떤 포트에서 동작하는지 확인할 수 있습니다. 

  ```bash
  $ docker port static-site
  443/tcp -> 0.0.0.0:32770
  80/tcp -> 0.0.0.0:32771
  ```

* 만약 이 어플리케이션을 swarm 모드에서 실행하고 있다면 아래 명령어를 통해 기본 manager의 IP를 확인할 수 있습니다. 

  ```bash
  $ docker-machine ip default
  192.168.99.100
  ```

* 이제 웹 브라우저에서 실행하고 있는 정적 웹사이트를 확인해 보세요. 

  ![](https://github.com/docker/labs/raw/master/beginner/images/static.png)

* 이제 실행 중인 컨테이너를 정지하고 삭제합니다. 

  ```bash
  $ docker stop static-site
  $ docker rm static-site
  ```

   

### Docker Images

* 도커에서 이미지는 컨테이너를 생성하는데 있어서 가장 기초가 되는 부분입니다. 

* 본인의 PC에서 생성되어 있는 이미지를 확인하고자 한다면 다음 명령어로 확인할 수 있습니다. 

  ```bash
  $ docker images
  ```

* 만약에 특정 버전의 이미지를 가져와야 한다면 버전을 명시해야 합니다. 

  ```bash
  $ docker pull ubuntu:12.04
  ```

* 버전을 명시하지 않는다면 최신 이미지를 가져옵니다. 

  ```bash
  $ docker pull ubuntu
  ```

* **Base images**

  * 상위 이미지가 없는 이미지로, 우분투 알파인 또는 데비안과 같은 OS가 있는 이미지 입니다. 

* **Child images** 

  * 기본 이미지를 기반으로 추가 기능을 추가할 수 있는 이미지 입니다. 

* **Official images**

  * 도커에서 공식 승인된 이미지로써 도커는 모든 공식 저장소를 검토하고 게시하는 전담 팀을 후원합니다. 공식 이미지에는 조직이나 사용자 이름이 앞에 붙지 않습니다. 

* **User images**

  * 사용자가 직접 만들고 배포한 이미지로서 기본 이미지를 기반으로 추가기능을 추가합니다. 일반적으로 사용자 혹은 조직이름이 추가되며 형식은 `user/image-name` 으로 되어있습니다. 

  

### Create your first image 

* 본 파트에서는 이미지를 만드는 방법에 대해서 알아봅니다. 

* 관련 이미지는 다음 디렉토리에 위치합니다. 

  ```bash
  $ cd labs/beginner/flask-app
  ls -l
  total 24
  -rw-r--r--  1 seongwonlee  staff   527B  7  7 20:44 Dockerfile
  -rw-r--r--  1 seongwonlee  staff   1.6K  7  7 20:44 app.py
  -rw-r--r--  1 seongwonlee  staff    14B  7  7 20:44 requirements.txt
  drwxr-xr-x  3 seongwonlee  staff    96B  7  7 20:44 templates
  ```

* 본 디렉토리의 구조는 위와 같이 구성되어 있습니다. 

* 각 파일의 내용은 다음과 같습니다. 

  * **app.py**

    ```python
    from flask import Flask, render_template
    import random
    
    app = Flask(__name__)
    
    # list of cat images
    images = [
        "http://ak-hdl.buzzfed.com/static/2013-10/enhanced/webdr05/15/9/anigif_enhanced-buzz-26388-1381844103-11.gif",
        "http://ak-hdl.buzzfed.com/static/2013-10/enhanced/webdr01/15/9/anigif_enhanced-buzz-31540-1381844535-8.gif",
        "http://ak-hdl.buzzfed.com/static/2013-10/enhanced/webdr05/15/9/anigif_enhanced-buzz-26390-1381844163-18.gif",
        "http://ak-hdl.buzzfed.com/static/2013-10/enhanced/webdr06/15/10/anigif_enhanced-buzz-1376-1381846217-0.gif",
        "http://ak-hdl.buzzfed.com/static/2013-10/enhanced/webdr03/15/9/anigif_enhanced-buzz-3391-1381844336-26.gif",
        "http://ak-hdl.buzzfed.com/static/2013-10/enhanced/webdr06/15/10/anigif_enhanced-buzz-29111-1381845968-0.gif",
        "http://ak-hdl.buzzfed.com/static/2013-10/enhanced/webdr03/15/9/anigif_enhanced-buzz-3409-1381844582-13.gif",
        "http://ak-hdl.buzzfed.com/static/2013-10/enhanced/webdr02/15/9/anigif_enhanced-buzz-19667-1381844937-10.gif",
        "http://ak-hdl.buzzfed.com/static/2013-10/enhanced/webdr05/15/9/anigif_enhanced-buzz-26358-1381845043-13.gif",
        "http://ak-hdl.buzzfed.com/static/2013-10/enhanced/webdr06/15/9/anigif_enhanced-buzz-18774-1381844645-6.gif",
        "http://ak-hdl.buzzfed.com/static/2013-10/enhanced/webdr06/15/9/anigif_enhanced-buzz-25158-1381844793-0.gif",
        "http://ak-hdl.buzzfed.com/static/2013-10/enhanced/webdr03/15/10/anigif_enhanced-buzz-11980-1381846269-1.gif"
    ]
    
    @app.route('/')
    def index():
        url = random.choice(images)
        return render_template('index.html', url=url)
    
    if __name__ == "__main__":
        app.run(host="0.0.0.0")
    ```

  * **requirements.txt**

    ```
    Flask==0.10.1
    ```

  * **templates/index.html**

    ```html
    <html>
      <head>
        <style type="text/css">
          body {
            background: black;
            color: white;
          }
          div.container {
            max-width: 500px;
            margin: 100px auto;
            border: 20px solid white;
            padding: 10px;
            text-align: center;
          }
          h4 {
            text-transform: uppercase;
          }
        </style>
      </head>
      <body>
        <div class="container">
          <h4>Cat Gif of the day</h4>
          <img src="{{url}}" />
          <p><small>Courtesy: <a href="http://www.buzzfeed.com/copyranter/the-best-cat-gif-post-in-the-history-of-cat-gifs">Buzzfeed</a></small></p>
        </div>
      </body>
    </html>
    ```

  * **Dockerfile**

    ```yaml
    # our base image
    FROM alpine:3.5
    
    # Install python and pip
    RUN apk add --update py2-pip
    
    # install Python modules needed by the Python app
    COPY requirements.txt /usr/src/app/
    RUN pip install --no-cache-dir -r /usr/src/app/requirements.txt
    
    # copy files required for the app to run
    COPY app.py /usr/src/app/
    COPY templates/index.html /usr/src/app/templates/
    
    # tell the port number the container should expose
    EXPOSE 5000
    
    # run the application
    CMD ["python", "/usr/src/app/app.py"]
    ```

* 위의 도커파일에서 보면 alpine을 기본 이미지로 하여 python 파일을 작성합니다. 

* 도커파일을 통해 이미지를 생성할 수 있으며 앞서 언급한 것과 같이 가볍게 만들 수 있도록 설계하는 것이 중요합니다. 

* `docker build` 명령어를 통해 이미지를 생성할 수 있으며, 이때 이름을 지정하기를 권장합니다. 이는 도커 클라우드에 이미지를 저장하는데 중요한 역할을 합니다. 

* `-t` 옵션을 통해 이름을 지정할 수 있으며, `.` 은 현재 디렉토리를 의미합니다. 

  ```bash
  $ docker build -t <YOUR_USERNAME>/myfirstapp .
  Sending build context to Docker daemon 9.728 kB
  Step 1 : FROM alpine:latest
   ---> 0d81fc72e790
  Step 2 : RUN apk add --update py-pip
   ---> Running in 8abd4091b5f5
  fetch http://dl-4.alpinelinux.org/alpine/v3.3/main/x86_64/APKINDEX.tar.gz
  fetch http://dl-4.alpinelinux.org/alpine/v3.3/community/x86_64/APKINDEX.tar.gz
  (1/12) Installing libbz2 (1.0.6-r4)
  (2/12) Installing expat (2.1.0-r2)
  (3/12) Installing libffi (3.2.1-r2)
  (4/12) Installing gdbm (1.11-r1)
  (5/12) Installing ncurses-terminfo-base (6.0-r6)
  (6/12) Installing ncurses-terminfo (6.0-r6)
  (7/12) Installing ncurses-libs (6.0-r6)
  (8/12) Installing readline (6.3.008-r4)
  (9/12) Installing sqlite-libs (3.9.2-r0)
  (10/12) Installing python (2.7.11-r3)
  (11/12) Installing py-setuptools (18.8-r0)
  (12/12) Installing py-pip (7.1.2-r0)
  Executing busybox-1.24.1-r7.trigger
  OK: 59 MiB in 23 packages
   ---> 976a232ac4ad
  Removing intermediate container 8abd4091b5f5
  Step 3 : COPY requirements.txt /usr/src/app/
   ---> 65b4be05340c
  Removing intermediate container 29ef53b58e0f
  Step 4 : RUN pip install --no-cache-dir -r /usr/src/app/requirements.txt
   ---> Running in a1f26ded28e7
  Collecting Flask==0.10.1 (from -r /usr/src/app/requirements.txt (line 1))
    Downloading Flask-0.10.1.tar.gz (544kB)
  Collecting Werkzeug>=0.7 (from Flask==0.10.1->-r /usr/src/app/requirements.txt (line 1))
    Downloading Werkzeug-0.11.4-py2.py3-none-any.whl (305kB)
  Collecting Jinja2>=2.4 (from Flask==0.10.1->-r /usr/src/app/requirements.txt (line 1))
    Downloading Jinja2-2.8-py2.py3-none-any.whl (263kB)
  Collecting itsdangerous>=0.21 (from Flask==0.10.1->-r /usr/src/app/requirements.txt (line 1))
    Downloading itsdangerous-0.24.tar.gz (46kB)
  Collecting MarkupSafe (from Jinja2>=2.4->Flask==0.10.1->-r /usr/src/app/requirements.txt (line 1))
    Downloading MarkupSafe-0.23.tar.gz
  Installing collected packages: Werkzeug, MarkupSafe, Jinja2, itsdangerous, Flask
    Running setup.py install for MarkupSafe
    Running setup.py install for itsdangerous
    Running setup.py install for Flask
  Successfully installed Flask-0.10.1 Jinja2-2.8 MarkupSafe-0.23 Werkzeug-0.11.4 itsdangerous-0.24
  You are using pip version 7.1.2, however version 8.1.1 is available.
  You should consider upgrading via the 'pip install --upgrade pip' command.
   ---> 8de73b0730c2
  Removing intermediate container a1f26ded28e7
  Step 5 : COPY app.py /usr/src/app/
   ---> 6a3436fca83e
  Removing intermediate container d51b81a8b698
  Step 6 : COPY templates/index.html /usr/src/app/templates/
   ---> 8098386bee99
  Removing intermediate container b783d7646f83
  Step 7 : EXPOSE 5000
   ---> Running in 31401b7dea40
   ---> 5e9988d87da7
  Removing intermediate container 31401b7dea40
  Step 8 : CMD python /usr/src/app/app.py
   ---> Running in 78e324d26576
   ---> 2f7357a0805d
  Removing intermediate container 78e324d26576
  Successfully built 2f7357a0805d
  ```

### Run your image

* 다음 명령어를 통해서 실제 어플리케이션을 실행 할 수 있습니다. 

  ```bash
  $ docker run -p 8888:5000 --name myfirstapp YOUR_USERNAME/myfirstapp
   * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
  ```

### Push your image 

* 다음은 도커에서 이미지를 도커 클라우드에 업로드 하는 방법을 기술합니다. 

  ```bash
  $ docker login
  $ docker push YOUR_USERNAME/myfirstapp
  ```

* 이제 도커 이미지를 삭제하고 도커 클라우드에서 이미지를 가져와서 실행해보세요. 





### Docker Swarm Tutorial

* 지금까지는 수동으로 swarm을 지정하고 실행하였습니다. 

* 이번에는 스크립트를 작성하는 예를 살펴보고 자동으로 운영할 수 있는 방법을 알아보도록 하겠습니다. 

* 예제는 다음 경로에 있습니다. 

  ```bash
  $ cd labs/swarm-mode/beginner-tutorial
  $ ls -l
  total 72
  -rw-r--r--  1 seongwonlee  staff    19K  7  7 20:44 README.md
  drwxr-xr-x  4 seongwonlee  staff   128B  7  7 20:44 images
  -rw-r--r--  1 seongwonlee  staff   1.8K  7  7 20:44 swarm-node-hyperv-setup.ps1
  -rw-r--r--  1 seongwonlee  staff   154B  7  7 20:44 swarm-node-hyperv-teardown.ps1
  -rwxr-xr-x  1 seongwonlee  staff   1.8K  7  7 20:44 swarm-node-vbox-setup.sh
  -rwxr-xr-x  1 seongwonlee  staff   195B  7  7 20:44 swarm-node-vbox-teardown.sh
  ```

* 지금까지 사용했던 방식인 버추얼박스를 통해서 실행하는 방법을 다음과 같이 스크립트로 작성할 수 있습니다. 

  ```bash
  #!/bin/bash
  
  # Swarm mode using Docker Machine
  
  managers=3
  workers=3
  
  # create manager machines
  echo "======> Creating $managers manager machines ...";
  for node in $(seq 1 $managers);
  do
  	echo "======> Creating manager$node machine ...";
  	docker-machine create -d virtualbox manager$node;
  done
  
  # create worker machines
  echo "======> Creating $workers worker machines ...";
  for node in $(seq 1 $workers);
  do
  	echo "======> Creating worker$node machine ...";
  	docker-machine create -d virtualbox worker$node;
  done
  
  # list all machines
  docker-machine ls
  
  # initialize swarm mode and create a manager
  echo "======> Initializing first swarm manager ..."
  docker-machine ssh manager1 "docker swarm init --listen-addr $(docker-machine ip manager1) --advertise-addr $(docker-machine ip manager1)"
  
  # get manager and worker tokens
  export manager_token=`docker-machine ssh manager1 "docker swarm join-token manager -q"`
  export worker_token=`docker-machine ssh manager1 "docker swarm join-token worker -q"`
  
  echo "manager_token: $manager_token"
  echo "worker_token: $worker_token"
  
  # other masters join swarm
  for node in $(seq 2 $managers);
  do
  	echo "======> manager$node joining swarm as manager ..."
  	docker-machine ssh manager$node \
  		"docker swarm join \
  		--token $manager_token \
  		--listen-addr $(docker-machine ip manager$node) \
  		--advertise-addr $(docker-machine ip manager$node) \
  		$(docker-machine ip manager1)"
  done
  
  # show members of swarm
  docker-machine ssh manager1 "docker node ls"
  
  # workers join swarm
  for node in $(seq 1 $workers);
  do
  	echo "======> worker$node joining swarm as worker ..."
  	docker-machine ssh worker$node \
  	"docker swarm join \
  	--token $worker_token \
  	--listen-addr $(docker-machine ip worker$node) \
  	--advertise-addr $(docker-machine ip worker$node) \
  	$(docker-machine ip manager1)"
  done
  
  # show members of swarm
  docker-machine ssh manager1 "docker node ls"
  ```

* 위의 스크립트를 실행하면 매니저 노드 3개 워커 노드 3개 총 6개의 가상머신이 생성됩니다. 

* 다음은 web 이라고 부르는 어플리케이션을 nginx를 통해 실행하도록 하겠습니다. 

  ```bash
  $ docker-machine ssh manager1 "docker service create -p 80:80 --name web nginx:latest"
  $ docker-machine ssh manager1 "docker service ls"
  ID            NAME  REPLICAS  IMAGE         COMMAND
  2x4jsk6313az  web   1/1       nginx:latest  
  ```

* `docker-machine ls` 명령을 통해 실행중인 머신 리스트를 확인하고 웹 브라우저를 실행하여 접속합니다. 

  ![](https://github.com/docker/labs/raw/master/swarm-mode/beginner-tutorial/images/manager1-nginx.png)

  ![](https://github.com/docker/labs/raw/master/swarm-mode/beginner-tutorial/images/manager2-nginx.png)

  * 예전에 설명 했듯이 swarm의 경우 각 노드가 메쉬구조로 생성되기 때문에 어떠한 노드 IP로 접속하더라도 같은 결과를 얻을 수 있습니다. 

* 다음 명령을 통해서는 실행 중인 서비스를 확인할 수 있습니다. 

  ```bash
  $ docker-machine ssh manager1 "docker service inspect web"
  [
      {
          "ID": "2x4jsk6313azr6g1dwoi47z8u",
          "Version": {
              "Index": 104
          },
          "CreatedAt": "2016-08-23T22:43:23.573253682Z",
          "UpdatedAt": "2016-08-23T22:43:23.576157266Z",
          "Spec": {
              "Name": "web",
              "TaskTemplate": {
                  "ContainerSpec": {
                      "Image": "nginx:latest"
                  },
                  "Resources": {
                      "Limits": {},
                      "Reservations": {}
                  },
                  "RestartPolicy": {
                      "Condition": "any",
                      "MaxAttempts": 0
                  },
                  "Placement": {}
              },
              "Mode": {
                  "Replicated": {
                      "Replicas": 1
                  }
              },
              "UpdateConfig": {
                  "Parallelism": 1,
                  "FailureAction": "pause"
              },
              "EndpointSpec": {
                  "Mode": "vip",
                  "Ports": [
                      {
                          "Protocol": "tcp",
                          "TargetPort": 80,
                          "PublishedPort": 80
                      }
                  ]
              }
          },
          "Endpoint": {
              "Spec": {
                  "Mode": "vip",
                  "Ports": [
                      {
                          "Protocol": "tcp",
                          "TargetPort": 80,
                          "PublishedPort": 80
                      }
                  ]
              },
              "Ports": [
                  {
                      "Protocol": "tcp",
                      "TargetPort": 80,
                      "PublishedPort": 80
                  }
              ],
              "VirtualIPs": [
                  {
                      "NetworkID": "24r1loluvdohuzltspkwbhsc8",
                      "Addr": "10.255.0.9/16"
                  }
              ]
          },
          "UpdateStatus": {
              "StartedAt": "0001-01-01T00:00:00Z",
              "CompletedAt": "0001-01-01T00:00:00Z"
          }
      }
  ]
  ```

* 이제 web 서비스를 확장해 보도록 하겠습니다. 

  ```bash
  $ docker-machine ssh manager1 "docker service scale web=15"
  web scaled to 15
  $ docker-machine ssh manager1 "docker service ls"
  ID            NAME  REPLICAS  IMAGE         COMMAND
  2x4jsk6313az  web   15/15     nginx:latest  
  ```

  * 위의 scale을 통해 서비스를 15개로 확장하였습니다. 

* 도커는 15개의 서비스를 모든 노드에 균등하게 분배합니다. 

  ```bash
  $ docker-machine ssh manager1 "docker service ps web"
  ID                         NAME    IMAGE         NODE      DESIRED STATE  CURRENT STATE 
  61wjx0zaovwtzywwbomnvjo4q  web.1   nginx:latest  worker3   Running        Running 13 
  bkkujhpbtqab8fyhah06apvca  web.2   nginx:latest  manager1  Running        Running 2    
  09zkslrkgrvbscv0vfqn2j5dw  web.3   nginx:latest  manager1  Running        Running 2  
  4dlmy8k72eoza9t4yp9c9pq0w  web.4   nginx:latest  manager2  Running        Running 2   
  6yqabr8kajx5em2auvfzvi8wi  web.5   nginx:latest  manager3  Running        Running 2    
  21x7sn82883e7oymz57j75q4q  web.6   nginx:latest  manager2  Running        Running 2   
  14555mvu3zee6aek4dwonxz3f  web.7   nginx:latest  worker1   Running        Running 2  
  1q8imt07i564bm90at3r2w198  web.8   nginx:latest  manager1  Running        Running 2    
  encwziari9h78ue32v5pjq9jv  web.9   nginx:latest  worker3   Running        Running 2    
  aivwszsjhhpky43t3x7o8ezz9  web.10  nginx:latest  worker2   Running        Running 2   
  457fsqomatl1lgd9qbz2dcqsb  web.11  nginx:latest  worker1   Running        Running 2   
  7chhofuj4shhqdkwu67512h1b  web.12  nginx:latest  worker2   Running        Running 2 
  7dynic159wyouch05fyiskrd0  web.13  nginx:latest  worker1   Running        Running 2    
  7zg9eki4610maigr1xwrx7zqk  web.14  nginx:latest  manager3  Running        Running 2   
  4z2c9j20gwsasosvj7mkzlyhc  web.15  nginx:latest  manager2  Running        Running 2  
  
  ```

* 특정 노드를 제외시킬 수도 있습니다. 이렇게 되면 선택된 특정 노드는 중지되고 다른 노드에서 자동으로 재조정 됩니다. 

  ```bash
   docker-machine ssh manager1 "docker node update --availability drain worker1"
  worker1
  $ docker-machine ssh manager1 "docker service ps web"
  ID                         NAME        IMAGE         NODE      DESIRED STATE  CURRENT 
  61wjx0zaovwtzywwbomnvjo4q  web.1       nginx:latest  worker3   Running        Running  
  bkkujhpbtqab8fyhah06apvca  web.2       nginx:latest  manager1  Running        Running  
  09zkslrkgrvbscv0vfqn2j5dw  web.3       nginx:latest  manager1  Running        Running  
  4dlmy8k72eoza9t4yp9c9pq0w  web.4       nginx:latest  manager2  Running        Running 
  6yqabr8kajx5em2auvfzvi8wi  web.5       nginx:latest  manager3  Running        Running   
  21x7sn82883e7oymz57j75q4q  web.6       nginx:latest  manager2  Running        Running    
  8so0xi55kqimch2jojfdr13qk  web.7       nginx:latest  worker3   Running        Running 
  14555mvu3zee6aek4dwonxz3f   \_ web.7   nginx:latest  worker1   Shutdown       Shutdown 
  1q8imt07i564bm90at3r2w198  web.8       nginx:latest  manager1  Running        Running 
  encwziari9h78ue32v5pjq9jv  web.9       nginx:latest  worker3   Running        Running  
  aivwszsjhhpky43t3x7o8ezz9  web.10      nginx:latest  worker2   Running        Running 
  738jlmoo6tvrkxxar4gbdogzf  web.11      nginx:latest  worker2   Running        Running 
  457fsqomatl1lgd9qbz2dcqsb   \_ web.11  nginx:latest  worker1   Shutdown       Shutdown 
  7chhofuj4shhqdkwu67512h1b  web.12      nginx:latest  worker2   Running        Running   
  4h7zcsktbku7peh4o32mw4948  web.13      nginx:latest  manager3  Running        Running   
  7dynic159wyouch05fyiskrd0   \_ web.13  nginx:latest  worker1   Shutdown       Shutdown 
  7zg9eki4610maigr1xwrx7zqk  web.14      nginx:latest  manager3  Running        Running   
  4z2c9j20gwsasosvj7mkzlyhc  web.15      nginx:latest  manager2  Running        Running 
  ```

  * worker1의 노드가 중지되고 다른 노드가 스케일이 재조정 되어 실행 되는 모습을 볼 수 있습니다. 

  * 이러한 모습은 다음을 통해서 명확하게 확인할 수도 있습니다. 

    ```bash
    $ docker-machine ssh manager1 "docker node ls"
    ID                           HOSTNAME  STATUS  AVAILABILITY  MANAGER STATUS
    3cq6idpysa53n6a21nqe0924h    manager3  Ready   Active        Reachable
    64swze471iu5silg83ls0bdip *  manager1  Ready   Active        Leader
    7eljvvg0icxlw20od5f51oq8t    manager2  Ready   Active        Reachable
    8awcmkj3sd9nv1pi77i6mdb1i    worker1   Ready   Drain         
    avu80ol573rzepx8ov80ygzxz    worker2   Ready   Active        
    bxn1iivy8w7faeugpep76w50j    worker3   Ready   Active
    ```

* 이제는 어플리케이션의 스케일을 줄여보겠습니다. 

  ```
  $ docker-machine ssh manager1 "docker service scale web=10"
  web scaled to 10
  $ docker-machine ssh manager1 "docker service ps web"
  ID                         NAME        IMAGE         NODE      DESIRED STATE  CURRENT 
  61wjx0zaovwtzywwbomnvjo4q  web.1       nginx:latest  worker3   Running        Running   
  bkkujhpbtqab8fyhah06apvca  web.2       nginx:latest  manager1  Shutdown       Shutdown 
  09zkslrkgrvbscv0vfqn2j5dw  web.3       nginx:latest  manager1  Running        Running   
  4dlmy8k72eoza9t4yp9c9pq0w  web.4       nginx:latest  manager2  Running        Running  
  6yqabr8kajx5em2auvfzvi8wi  web.5       nginx:latest  manager3  Running        Running 
  21x7sn82883e7oymz57j75q4q  web.6       nginx:latest  manager2  Running        Running    
  8so0xi55kqimch2jojfdr13qk  web.7       nginx:latest  worker3   Running        Running     
  14555mvu3zee6aek4dwonxz3f   \_ web.7   nginx:latest  worker1   Shutdown       Shutdown  
  1q8imt07i564bm90at3r2w198  web.8       nginx:latest  manager1  Running        Running 
  encwziari9h78ue32v5pjq9jv  web.9       nginx:latest  worker3   Shutdown       Shutdown  
  aivwszsjhhpky43t3x7o8ezz9  web.10      nginx:latest  worker2   Shutdown       Shutdown  
  738jlmoo6tvrkxxar4gbdogzf  web.11      nginx:latest  worker2   Running        Running     
  457fsqomatl1lgd9qbz2dcqsb   \_ web.11  nginx:latest  worker1   Shutdown       Shutdown  
  7chhofuj4shhqdkwu67512h1b  web.12      nginx:latest  worker2   Running        Running  
  4h7zcsktbku7peh4o32mw4948  web.13      nginx:latest  manager3  Running        Running    
  7dynic159wyouch05fyiskrd0   \_ web.13  nginx:latest  worker1   Shutdown       Shutdown    
  7zg9eki4610maigr1xwrx7zqk  web.14      nginx:latest  manager3  Shutdown       Shutdown 
  4z2c9j20gwsasosvj7mkzlyhc  web.15      nginx:latest  manager2  Shutdown       Shutdown  
  ```

* 이번에는 중지된 worker1 노드를 다시 살려보도록 하겠습니다. 

  ```bash
  $ docker-machine ssh manager1 "docker node update --availability active worker1"
  worker1
  $ docker-machine ssh manager1 "docker node inspect worker1 --pretty"
  ID:			8awcmkj3sd9nv1pi77i6mdb1i
  Hostname:		worker1
  Joined at:		2016-08-23 22:30:15.556517377 +0000 utc
  Status:
   State:			Ready
   Availability:		Active
  Platform:
   Operating System:	linux
   Architecture:		x86_64
  Resources:
   CPUs:			1
   Memory:		995.9 MiB
  Plugins:
    Network:		bridge, host, null, overlay
    Volume:		local
  Engine Version:		17.03.0-ce
  Engine Labels:
   - provider = virtualbox
  ```

* manager1 노드를 swarm에서 제거해 보도록 하겠습니다. 

  ```bash
  $ docker-machine ssh manager1 "docker swarm leave --force"
  Node left the swarm.
  ```

* 노드를 swarm 에서 제거한 후 약 30초의 시간이 지나면 새로운 매니저 노드를 자동으로 선출하고 swarm서비스가 계속 진행됩니다. 

  ```bash
  $ docker-machine ssh manager2 "docker node ls"
  ID                           HOSTNAME  STATUS  AVAILABILITY  MANAGER STATUS
  3cq6idpysa53n6a21nqe0924h    manager3  Ready   Active        Reachable
  64swze471iu5silg83ls0bdip    manager1  Down    Active        Unreachable
  7eljvvg0icxlw20od5f51oq8t *  manager2  Ready   Active        Leader
  8awcmkj3sd9nv1pi77i6mdb1i    worker1   Ready   Active        
  avu80ol573rzepx8ov80ygzxz    worker2   Ready   Active        
  bxn1iivy8w7faeugpep76w50j    worker3   Ready   Active
  ```

  * 위에서 볼 수 있듯이 manager1이 Unreachable 되었고 manager2가 새로운 리더로 설정된 모습을 볼 수 있습니다. 

  * 서비스는 다음과 같이 종료할 수도 있습니다. 

    ```bash
    $ docker-machine ssh manager2 "docker service rm web"
    web
    ```

* 다음 스크립트를 실행하여 모든 머신을 종료하고 삭제하여 마무리 하세요. 

  ```bash
  $ ./swarm-node-vbox-teardown.sh
  Stopping "manager3"...
  Stopping "manager2"...
  Stopping "worker1"...
  Stopping "manager1"...
  Stopping "worker3"...
  Stopping "worker2"...
  Machine "manager3" was stopped.
  Machine "manager1" was stopped.
  Machine "manager2" was stopped.
  Machine "worker2" was stopped.
  Machine "worker1" was stopped.
  Machine "worker3" was stopped.
  About to remove worker1, worker2, worker3, manager1, manager2, manager3
  Are you sure? (y/n): y
  Successfully removed worker1
  Successfully removed worker2
  Successfully removed worker3
  Successfully removed manager1
  Successfully removed manager2
  Successfully removed manager3
  ```

  