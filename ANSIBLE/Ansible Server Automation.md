# Ansible AWX를 활용한 서버 자동화 1

![](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcR1Ot4uXe4Uqr3l69rYdTzSVQdTIZDnn9oWUzBK4aefC3hxIVq_wA)



### Why Ansible?

* Ansible은 Infrastructure as a code에 대표적인 도구로, 현재 가장많이 사용되는 서버 자동화 도구라고 할 수 있다. 
* Ansible의 경우 기존의 Agent의 설치할 필요 없이 SSH 접속을 통해서 간편하게 운영할 수 있으며, 다양한 모듈을 제공하여 쉽고 빠르게 운영 서버에 즉각적인 배포가 가능하다는 장점이 있다. 
* Ansible은 2012년에 출시되어 2013년 레드햇에 인수되어 현재까지 개발되고 있다. 이에 대한 자세한 소개는 지난 1편을 글을 통해 확인할 수 있으므로 본 편을 읽어보기 전에 기본 개념을 확인하고 오기를 바란다. 



### Installation Ansible

#### MAC OS

```bash
$ brew install ansible
```

#### Fedora

````shell
$ sudo dnf install ansible
````

#### Ubuntu 

```shell
$ sudo apt update 
$ sudo apt install software-properties-common
$ sudo apt-repository --yes --update ppa:ansible/ansible
$ sudo apt install ansible -y
```

* 본 연구실의 모든 서버는 Ubuntu Server 18.04 LTS를 사용하고 있다. 



### 연구실에 Ansible을 도입하기 까지… 

* Ansible를 사용하는 방식에는 CLI를 사용하는 방법과 Ansible Tower와 같은 웹기반으로 사용하는 방법이 있다. 

  * Tower의 경우 레드헷에서 유료기반으로 사용해야 한다는 단점이 있으며 , CLI의 경우 익숙해지기 전까지 사용하는데 어려움이 있다는 단점이 있다. 

* 단, Ansible Tower의 오픈소스 버전인 Ansible AWX를 사용한다면 Tower의 웹 기반 구조를 사용할 수 있다는 장점이 있기 때문에 Ansible AWX에 대해서 본격적으로 알아보도록 하겠다. 

  ![](https://www.ansible.com/hubfs/2017_Images/Blog/5-Things-AWX-Blog-Post-Header.png)

* 현재 본 연구실에는 ESXi 서버 2대에서 약 20여대의 연구용 및 서비스용 VM이 동작하고 있으며, 이외 딥러닝용 서버, 관리용 서버, 모니터링 등 다양한 서버를 한명이 관리하고 있다. 
* 관리하는 서버의 댓수와 서비스가 다양화 되면서 요구사항에 대한 빠르게 대응하기 위함과, 백업, 보안 패치등 신경써야 하는 부분이 많아 짐에 따라서 업무를 효율적으로 분산하기 위해서 본격적으로 도입을 진행하였다. 
  * 현재 약 4개월 정도 연구실에서 사용하는 템플릿을 정의하고, 실행 내용을 매일 자동화으로 실행하여 서버 운영에 많은 도움이 되고 있다. 



### Ansible  AWX 설치하기

**본 연구실은 Ubuntu 18.04 Server LTS 버전을 기준으로 사용하기 때문에 이점 참고 바란다.**



1. Ansible을 설치할 서버를 준비한다. 

   1. 사양은 크게 중요하지 않으나, 본 연구실과 같이 네트워크가 각각 분리되어 있을 경우 NIC 추가를 통해서 각 네트워크에 맵핑될 수 있도록 했다. 
   2. Ansible AWX는 다양한 설치 방법이 존재하나 가장 기본적인 docker-compose를 통한 설치방법을 살펴본다. 

2. Docker, docker-compose, Python3, Ansible과 같은 의존성 파일이 설치해야 한다. 

   ```bash
   $ sudo apt update && sudo apt upgrade -y
   $ sudo apt install \
       apt-transport-https \
       ca-certificates \
       curl \
       gnupg-agent \
       software-properties-common -y
       
   $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
   
   $ sudo apt-key fingerprint 0EBFCD88
   
   $ sudo add-apt-repository \
      "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) \
      stable"
   
   $ sudo apt update
   $ sudo apt install docker-ce docker-ce-cli containerd.io -y 
   $ sudo docker --version
   
   $ sudo apt install python3 python3-pip -y
   $ pip3 install docker-compose
   $ docker-compsose --version
   ```

3. 다음 Ansible AWX 깃허브에서 소스 다운로드를 진행한다. 

   ````bash
   $ git clone https://github.com/ansible/awx.git
   $ vi awx/installer/inventory
   ````

4. 기본 설정파일을 통해 설치를 진행하면 재부팅시 설정파일이 모두 사라지기 때문에 inventory 파일을 수정한다. 

   ```bash
   # Common Docker parameters
   awx_task_hostname=awx
   awx_web_hostname=awxweb
   postgres_data_dir=/pgdocker
   host_port=80
   #ssl_certificate=
   docker_compose_dir=/awxcompose
   
   # This will create or update a default admin (superuser) account in AWX, if not provided
   # then these default values are used
   admin_user=$(user)
   admin_password=$(password) 
   ```

   1. 기본 설정에서 위와 같이 설정을 변경해도 운영하는데 크게 문제는 없으나 Ansible Tower에서 사용되는 데이터베이스나 메시지큐에 대한 비밀번호를 변경해도 된다. 

5. 설정파일 변경 후 아래 디렉토리에서 설치를 시작한다. 

   ```bash
   $ cd awx/installer
   $ sudo ansible-playbook -i inventory install.yml
   ```

6. 잠시후 Docker에 정상적으로 Deploy 되어 있는지 확인한다. 

   ```bash
   $ sudo docker ps -a
   
   IMAGE                        PORTS                       NAMES
   ansible/awx_task:4.0.0       8052/tcp                    awx_task
   ansible/awx_web:4.0.0        0.0.0.0:80->8052/tcp        awx_web
   postgres:9.6                 5432/tcp                    awx_postgres
   ansible/awx_rabbitmq:3.7.4   4369/tcp, 5671-5672/tcp..   awx_rabbitmq
   memcached:alpine             1211/tcp                    awx_memcached
   ```

7. Docker 로그를 통해 상태를 확인할 수 있다. 

   ```bash
   $ sudo docker logs -f awx_task
   ```

8. 해당 설치된 IP를 통해 웹브라우저에 접속한다. 이후 내용은 Tower와 유사함으로 Ansible Tower의 내용을 기반으로 작성하겠다. 

   ​	![](https://docs.ansible.com/ansible-tower/latest/html/quickstart/_images/qs-login-form.png)

9. 로그인을 진행하면 다음과 같은 화면이 보이게 된다. (Tower 로고만 AWX로 변경되어 있다.)

   ![](https://docs.ansible.com/ansible-tower/latest/html/quickstart/_images/home-dashboard.png)

   

10. 본격적으로 사용하기 위해서는 여러가지 셋팅이 필요한데 이에 대한 부분은 다음 시간을 통해서 자세히 살펴보도록 하겠다. 

    1. 본 연구실에서는 GitLab과 연동하여 yml 파일을 정의하고, 각 서버의 인증 정보와, 서버 종류를 등록하여 관리한다. 



### Apache를 이용한 Reverse Proxy 사용하기

**AWX를 IP주소를 통해서 관리하는 것은 매우 불편하기 때문에 Apache Reverse Proxy에 등록하였다.**

**도메인이 발급되어 있고, Certbot을 통해 Let's Encrypt SSL 사용이 가능하다는 전재를 조건으로 한다**



* Ansible AWX의 템플릿을 실행하면 다음과 같이 실행결과가 표기되는데  웹 소켓 기반으로 동작하기 때문에 Reverse Proxy 등록시 웹 소켓 리다이렉션 규칙을 정해주어야 한다. 
  ![](https://docs.ansible.com/ansible-tower/latest/html/quickstart/_images/qs-job-templates-demo-complete.png)



* 크롬 관리자 모두를 통해서 내부망에서 AWX의 통신과정을 확인하여 아래와 같이 Apache 파일을 수정하였다. 

  ```
   36   RewriteEngine On
   37   RewriteCond %{REQUEST_URI} ^/websocket [NC,OR]
   38   RewriteCond %{HTTP:UPGRADE} ^WebSocket$ [NC,OR]
   39   RewriteCond %{HTTP:CONNECTION} ^Upgrade$ [NC]
   40   RewriteRule .* ws://10.0.0.4%{REQUEST_URI} [P,QSA,L]
   41   RewriteCond %{DOCUMENT_ROOT}/%{REQUEST_FILENAME} !-f
   42   RewriteRule .* http://10.0.0.4%{REQUEST_URI} [P,QSA,L]
   43
   44   ProxyPass / http://10.0.0.4/ nocanon
   45   ProxyPassReverse / http://10.0.0.4/
   46
   47   ProxyPass /websocket/ ws://10.0.0.4/websocket/
   48   ProxyPassReverse /websocket/ ws://10.0.0.4/websocket/
   49
   50   RequestHeader set X-Forwarded-Proto "https"
   51   RequestHeader set X-Forwarded-Port "443"
  ```

  * 본 규칙이 100% 정답이라고 할 수 없기 때문에 참고용도로 확인하길 바란다. 

































