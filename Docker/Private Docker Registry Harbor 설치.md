# Private Docker Registry Harbor 설치 

![](https://static.hubtee.com/files/fa2/fa204630bed1deaf4e18a19a180f885934b613c6293584180ef1412fe8ed5283.m.png)

### 왜 Harbor를 도입했는가?

 Docker를 사용하다 보면 Docker Registry가 반드시 필요하다. 특히나 기업 혹은 본인 연구실에서는 관련 서비스를 Docker 전환이 진행하고 있는 과정에서 Private Docker Registry는 반드시 필요하다고 할 수 있다. 기존에서는 Sonatype nexus를 사용했으나 maven, npm 등 사설 레지스트리로 사용하는데 충분하지만 Docker로 사용하기 위해서는 https 사용할 수 없어 추가적인 설정이 필요했기 때문에 계속해서 사용하기 에는 적합하지 않다고 판단하였다.

 사실상 Docker Hub를 사용하면 간단하게 문제를 해결할 수 있겠지만, 무료로 사용하기 위해서는 public으로 공개해야 하는 점, private로 사용할 경우 추가적인 비용을 지불해야 한다는 점에서 연구실에서 사용하기에는 부적합하다고 생각했다.

### Harbor 도입의 결정적인 계기

![](https://miro.medium.com/max/871/1*FEChFwTaaiClBo8qBypm1g.jpeg)

 Harbor는 Private Docker Registry에 적합하게 설계되어 있어 기존 Docker 환경에 추가하여 간단하게 유지 보수가 가능하고 각 projects 별로 구분 관리가 가능하다는 점, 계정별 접근 관리가 가능한 여러 장점을 얻을 수 있기 때문에 Docker를 사용한 서비스 구성 및 운영에 있어서 반드시 필요하다고 생각하게 되었다.

 Harbor의 자세한 사항과 문서는 [공식](https://goharbor.io/) 홈페이지에서 확인할 수 있다. 설치방법이 그리 어렵지 않으니 쉽게 따라 할 수 있을 것이다.

### Harbor installation

먼저 앞서 본인은 다음과 같은 설치환경에서 구성했음을 알린다.

-   XCP-ng
    -   CPU : vCPU 16 core
    -   RAM : 16GB
    -   Storage : 500GB
-   OS : Ubuntu 18.04.4 LTS
-   도메인 발급 가능한 환경

---

다음 기술 스택을 알고 있음을 전재로 설명한다.

-   docker
-   docker-compose
-   SSL - certbot

---

Harbor github [링크](https://github.com/goharbor/harbor)를 참조바라며, 다운로드는 [여기](https://github.com/goharbor/harbor/releases)서 받을 수 있다. 2020년 3월 10일 현재 v1.10.1이 최신 버전이며, 다음과 같이 설치를 진행한다.

1.  설치 파일을 다운로드한다.
    
    ```
    $ wget https://github.com/goharbor/harbor/releases/download/v1.10.1/harbor-offline-installer-v1.10.1.tgz
    ```
    
2.  압축을 풀고 harbor.yml 파일을 다음과 같이 수정한다.
    
    ```
    $ tar -xvf harbor-offline-installer-v1.10.1.tgz
    $ cd harbor
    $ vi harbor.yml
    ```
    
    ```
    # http related config
    http:
      # port for http, default is 80. If https enabled, this port will redirect to https port
      port: 80
    
    # https related config
    https:
      # https port for harbor, default is 443
      port: 443
      # The path of cert and key files for nginx
      certificate: /etc/letsencrypt/live/$(domain)/fullchain.pem
      private_key: /etc/letsencrypt/live/$(domain)/privkey.pem
    
    # Uncomment external_url if you want to enable external proxy
    # And when it enabled the hostname will no longer used
    # external_url: https://reg.mydomain.com:8433
    
    # The initial password of Harbor admin
    # It only works in first time to install harbor
    # Remember Change the admin password from UI after launching Harbor.
    harbor_admin_password: $(password)
    
    # Harbor DB configuration
    database:
      # The password for the root user of Harbor DB. Change this before any production use.
      password: $(password)
      # The maximum number of connections in the idle connection pool. If it <=0, no idle connections are retained.
      max_idle_conns: 50
      # The maximum number of open connections to the database. If it <= 0, then there is no limit on the number of open connections.
      # Note: the default number of connections is 100 for postgres.
      max_open_conns: 100
    
    # The default data volume
    data_volume: /data
    ```
    
    -   harbor는 기본적으로 nginx가 포함되어 있으며, HTTPS 사용을 위해서 인증서 위치를 지정한다.
        
        -   본 설치 과정에서는 certbot를 활용해서 인증서를 받는다.
            
        -   certbot 설치
            
            ```
            $ sudo aptupdate && sudo apt install software-properties-common -y 
            $ sudo add-apt-repository universe
            $ sudo add-apt-repository ppa:certbot/certbot
            $ sudo apt update && apt install certbot -y
            ```
            
        -   인증서 발급
            
            ```
            $ sudo certbot certonly -d $(domain)
            ```
    
3.  harbor는 docker-compose 기반에서 동작하기 때문에 설치를 마무리 지으면 다음과 같이 확인할 수 있다.
    
    ```
    $ sudo ./install.sh
    $ sudo docker-compose ps
    ```
    
    ```
          Name                     Command                  State          Ports
    -----------------------------------------------------------------------------------------
    harbor-core         /harbor/harbor_core              Up (healthy)
    harbor-db           /docker-entrypoint.sh            Up (healthy)   5432/tcp
    harbor-jobservice   /harbor/harbor_jobservice  ...   Up (healthy)
    harbor-log          /bin/sh -c /usr/local/bin/ ...   Up (healthy)   127.0.0.1:1514->10514/tcp
    harbor-portal       nginx -g daemon off;             Up (healthy)   8080/tcp
    nginx               nginx -g daemon off;             Up (healthy)   0.0.0.0:80->8080/tcp, 0.0.0.0:443->8443/tcp
    redis               redis-server /etc/redis.conf     Up (healthy)   6379/tcp
    registry            /home/harbor/entrypoint.sh       Up (healthy)   5000/tcp
    registryctl         /home/harbor/start.sh            Up (healthy)
    ```
    
4.  설치가 모두 완료 대면 설정한 도메인으로 접속하여 정상적으로 접속이 가능한지 확인한다.
    
    -   기본 아이디는 admin이며, 비밀번호의 경우 harbor.yml에 설정한 비밀번호이다. 만약 비밀번호 설정 부분을 수정하지 않았다면 Harbor12345가 기본 비밀번호이다.

#### 로그기록 확인

 로그기록의 경우 다음 경로에서 확인할 수 있다.

```
$ cd /var/log/harbor
$ ls -a 

-rw-r--r-- 1 10000 10000  7309406 Mar 10 16:54 core.log
-rw-r--r-- 1 10000 10000  7510288 Mar 10 02:01 jobservice.log
-rw-r--r-- 1 10000 10000 17407416 Mar 10 16:54 portal.log
-rw-r--r-- 1 10000 10000    10643 Mar  5 02:01 postgresql.log
-rw-r--r-- 1 10000 10000  4230067 Mar 10 16:54 proxy.log
-rw-r--r-- 1 10000 10000  1648040 Mar 10 16:53 redis.log
-rw-r--r-- 1 10000 10000 15948958 Mar 10 16:54 registryctl.log
-rw-r--r-- 1 10000 10000 17549068 Mar 10 16:54 registry.log
```

### 설치 테스트

 harbor의 경우 로그인을 진행하면, Projects 항목을 지정할 수 있다. docker 이미지를 업로드할 때의 위치를 지정하는 것으로, public이라는 Projects를 생성하고 정상적으로 업로드가 진행되는지 확인한다.

```
$ sudo docker login $(domain) -u $(id)
Password:
Login Succeeded
```

```
$ sudo docker pull ubuntu
$ sudo docker tag ubuntu $(domain)/public/ubuntu
$ sudo docker push $(domain)/public/ubuntu

The push refers to repository [$(domain)/public/ubuntu]
1852b2300972: Pushed
03c9b9f537a4: Pushed
8c98131d2d1d: Pushed
cc4590d6a718: Pushed
```

 정상적으로 설치가 완료되어 push를 진행했을 경우, public이라는 Projects에 repositories항목을 살펴보면 방금 전에 push 한 ubuntu docker 이미지 파일이 존재할 것이다.

 LDAP, 외부 저장소 연동 등 다양한 기능이 Harbor에 존재하고 있으니 본인 환경에 맞춰서 구축하는 것이 좋을 것이다. 현재 본 연구실에서는 별다른 이상 없이 잘 사용하고 있으며, Private Docker Registry가 필요한 상황이라면 검토하여 운행할 수 있는 좋은 오픈소스라고 생각한다.