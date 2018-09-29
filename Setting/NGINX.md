# NGINX Documents

![](https://cdn-1.wp.nginx.com/wp-content/uploads/2015/04/NGINX_logo_rgb-01.png)





NGINX는 웹서버 소프트웨어로, 가벼움과 높은 성능을 목표로 합니다. 

웹 서버, 리버스 프록시 및 메일 프록시 기능을 가지며 2017년 10월 기준 웹 사이트에서 쓰이는 웹 서버 소프트웨어 중에서 2위를 기록했습니다. 

## High-performance load balancing 
![](https://cdn-1.wp.nginx.com/wp-content/uploads/2017/09/NGINX-Plus-Features-Load-balancer-1024x290.png)

* 로드밸런싱은 여러 서버의 워크로드를 고르게 분산시키는 프로세스 입니다. 
* 웹 응용프로그램의 경우 HTTP 요청은 응용 프로그램 서버 전체로 고르게 분산됩니다. 
* 로드밸런싱은 단일 서버로 확장 할 수 있는 것 보다 많은 요청을 처리할 수 있으며, 하나의 서버에 장애가 발생하면 다른 응용 프로그램이 온라인 상태를 유지할 수 있습니다. 


* 오픈소스 NGINX와 NGINX Plus는 각 HTTP 연결을 종료하고 요청을 개별적으로 처리 할 수 있습니다. 
* SSL 요청을 검사하고 속도제한을 사용하여 요청을 대기시킨 균형 조정 정책을 선택할 수 있습니다. 

### HTTP 로드밸런싱 
```
http {
    upstream my_upstream {
        server server1.example.com;
        server server2.example.com;
    }

    server {
        listen 80;
        location / {
            proxy_set_header Host $host;
            proxy_pass http://my_upstream;
        }
    }
}
```
* 일반적으로 로드밸런싱 규칙은 위와 같이 설정합니다. 
* 기본적으로 라운드 로빈을 지원하며, 로드밸런싱 방법에는 아래와 같은 방법을 지원합니다. 

|방법|설명|
|:---:|:---:|
|라운드로빈(기본값)| 요청을 순서대로 처리합니다.|
|최소 연결| 각 요청은 서버에 할당된 가중치를 고려하여 활성 연결 수가 가장 적은 서버로 전송합니다.|
|해시| 요청은 클라이언트 IP주소 또는 URL과 같은 사용자 정의 키를 기반으로 배포됩니다.<br> NGINX Plus는 업스트림 서버 세트가 변경된 경우의 재분배을 최소화하기 위해 선택적으로 일관된 해시를 적용합니다.|
|IP 해시(HTTP만 해당| 요청은 클라이언트의 IP 주소의 처음 세 옥텟을 기반으로 배포됩니다. |
|최소 시간(NGINX Plus)| 요청시 가장 빠른 응답 시간과 가장 적은 활성 연결로 업스트림 서버로 전송합니다.|

### TCP & UDP 로드밸런싱 
```bash
stream {
    upstream my_upstream {
        server server1.example.com:1234;
        server server2.example.com:2345;
    }

    server {
        listen 1123 [udp];
        proxy_pass my_upstream;
    }
} 
```

### 세션 지속성
* NGINX Plus에서만 사용할 수 있는 세션 지속성을 사용하면 사용자 세션을 식별하고 모든 요청을 동일한 업스트림 서버로 전송합니다. 

```bash
upstream backend {
    server webserver1;
    server webserver2;

    sticky cookie srv_id expires=1h domain=.example.com path=/;
}
```
* 응용 프로그램서버가 로컬로 상태를 저장하고 NGINX가 진행중인 세션을 다른 서버로 보낼 때 발생할 수 있는 치명적인 오류를 피할 수 있습니다. 
* 세션 지속성은 응용 프로그램이 클러스터에서 정보를 공유할 때 성능을 향상시킬 수 있습니다. 
* NGINX Plus는 클라이언트 요청시 쿠키값이 포함되어 있는데 위의 예에서는 전송된 서버를 식별하는 srv_id 라는 쿠키 값을 삽입하고, 다음 요청에 쿠키에 해당값이 포함되어 있으면 동일한 서버로 전달합니다. 

### NGINX 오픈소스 버전에서 IP 4 옥텟까지 hash 설정하기 
* 무료 오픈소스인 NGINX 버전에서는 IP 3 옥텟까지 동일 IP 버전으로 셋팅되어 있습니다. 
* 이 문제를 해결하기 위해서는 다음과 같은 문구를 추가합니다. 

```bash
upstream backend {
    hash $remote_addr consistent;

    server backend1.example.com:12345  weight=5;
    server backend2.example.com:12345;
    server unix:/tmp/backend3;
}

server {
    listen 80;
    location / {
          proxy_pass http://backend;
      }
}
```

