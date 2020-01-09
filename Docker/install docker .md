![](https://subicura.com/assets/article_images/2017-01-19-docker-guide-for-beginners-1/docker-logo.png)






























##Hello Docker 

2013년 도커(Docker)가 세상에 공개되었다. 그리고 5년이 지난 지금 도커는 어디서나 사용되고 있는 오픈소스 프로젝트로 자리매김 하게 되었다. 인터넷에 공개된 수많은 도커에 대한 자료를 읽어보면서 도커의 중요성이 높아진다는 사실을 알고 있었으나 실제로 학습하기까지는 많은 시간이 걸렸다. <br> 
<p> 필자는 현재 AWS, 오픈스택을 주로 공부하고 있으면서 도커에 도전장을 내밀게 되었고 시중에 판매중인 도커책을 구매하여 읽어보기 시작했는데 버전이 많이 달라진 탓인지 재대로 실행하지 않는 부분이 많았다. (사실 AWS든, 오픈스택이든 따라하기 책들은 시간이 지나면 재대로 실행되지 않는 부분이 많은 것 같다.) 

<p> 이제 도커에 입문을 하는 학생의 입장에서 Docker Docs를 보면서 직접 실습해보고, 책을 같이 읽어보면서 도커에 대해 이해를 겸하고자 한다. 이제 시작하는 입장에서 이렇게 매뉴얼을 적어보고 실습하는 것 자체가 본인에게 도움이 될 것이라 생각하면서, 처음 입문하고자 하는 사람들에게도 도움이 됬으면 하는 작은 바람이다. 

## Introduce Docker 

도커에 대한 설명은 다른 블로그에서도 잘 설명되어 있기 때문에 따로 설명하지는 않을 것이다. 중요한 것은 컨테이너기반에 운용되는 도커는 기존의 반가상화, 전가상화에 비해서 가볍고 빠르고 간편하다는 장점이 있다는 것이다. 
<p> 

* Docker는 Community Edition (CE) 과 Enterprise Edition (EE) 2가지 버전으로 제공된다.


* Docker CE는 Docker를 시작하고 컨테이너 기반 앱을 실험하려는 개발자와 소규모 팀에게 이상적이다. 


* Docker는 stable, edge 버전 2가지 종류가 제공되는데 차이점은 다음과 같다. 
	* stable : 분기마다 안정적인 업데이트 제공 
	* edge : 매일 새로운 기능 제공 

## Install Docker 

* 필자의 Docker 설치환경은 다음과 같다. 
	* 하드웨어 : 라떼판다 
	* OS : Ubuntu 16.04.3 LTS 

사실 하드웨어와 소프트웨어는 크게 중요하지 않다. 도커는 거의 모든 플랫폼에서 제공하고 있으며 설치하고 사용할 수 있다. 단지 본인은 우분투 환경이 익숙하고 사용하고 있는 MAC환경에 실험용 환경을 구축하고 싶지 않아서이다. 
<p> 필자와 동일한 환경에서 테스트 하고 싶다면 우분투 환경을 구축하고 실험해 보자!! 

1. 이전 버전 제거하기 

	```bash 
	$ sudo apt remove docker docker-engine docker.io 
	```
2. 국내는 해외망이 상당히 느린탓에 빠른 진행을 위해서 우분투에 미러서버를 변경한다. 
	* 이미 적용된 분이나 하고 싶지 않은 분들은 진행하지 않아도 된다. 
	
	```bash
	$ sudo sed -i 's/kr.archive.ubuntu.com/ftp.daumkakao.com/g' \
	 /etc/apt/sources.list
	$ sudo apt update && sudo apt upgrade -y 
	```
3. 도커를 설치하기전에 필수 라이브러리를 설치하는 과정을 가진다. 
	
	```bash 
	$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
	```   	
	
4. Docker의 공식 GPG키는 다음과 같이 추가한다. 

	```bash 
	$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
	$ sudo apt-key fingerprint 0EBFCD88

	pub   4096R/0EBFCD88 2017-02-22
      	  Key fingerprint = 9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
	uid                  Docker Release (CE deb) <docker@docker.com>
	sub   4096R/F273FCD8 2017-02-22

	```
5. 설치할 Docker 버전이 stable 버전을 원한다면 다음의 명령어를 입력한다. 
	* stable, edge, test 명령을 추가하면 해당 버전으로 저장소가 설정된다. 
	
	```bash 
	$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
	```   	
	* 참고로 Docker 17.06 버전부터는 stable 저장소가 edge나 test 저장소로 푸시된다고 한다. 

6. 이제 docker를 설치할 준비는 되었다. 설치해보자!! 

	```bash
	$ sudo apt udpate 
	$ sudo apt install docker-ce -y  
	```
	
	* 위와 같이 설치하면 docker는 항상 최신 버전으로 설치된다. 
	* 만약 특정버전이 필요하다면 다음과 같은 명령어를 실행한다. 

		```bash
		$ apt-cache madison docker-ce
		
 		docker-ce | 17.12.1~ce-0~ubuntu | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
 		docker-ce | 17.12.0~ce-0~ubuntu | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
 		docker-ce | 17.09.1~ce-0~ubuntu | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
 		docker-ce | 17.09.0~ce-0~ubuntu | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
 		docker-ce | 17.06.2~ce-0~ubuntu | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
 		docker-ce | 17.06.1~ce-0~ubuntu | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
 		docker-ce | 17.06.0~ce-0~ubuntu | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages 
 		
 		$ sudo apt-get install docker-ce=<VERSION>
		```
		
7. docker가 정상적으로 설치되었다면 데몬으로 자동으로 실행된다. 모든지 시작이 반!! 정상적으로 설치되었는지 확인하기 위해서 hello-world을 실행해보자 

	```bash
	$ sudo docker run hello-world
	
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
 		https://cloud.docker.com/

	For more examples and ideas, visit:
 		https://docs.docker.com/engine/userguide/
	 
	```
	
	
이렇게 도커를 설치하는 환경을 정리해 보았다. 아마 순서대로 따라했다면 별 무리없이 hello-world를 볼 수 있을 것이다.
