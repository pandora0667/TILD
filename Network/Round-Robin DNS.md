# 라운드로빈 DNS (Round-Robin DNS)

<img src="https://aidanfinn.com/wp-content/uploads/2017/12/Azure-DNS-1024x1024.png" style="zoom:33%;" />

사용자가 `www.google.com` 도메인을 입력했을 경우 네트워크계층(L3)에서는 도메인을 통해 통신이 불가능하기 때문에 IP주소로 변경하기 위해 DNS(Domain Name System)를 사용한다. 따라서 인터넷에 연결된 각각의 서버는 공인 IP주소를 가지고 있으나, 사용자는 복잡한 구조의 숫자를 기억할 필요가 없다. 

즉, 서버를 운영하기 위해서는 도메인과 이에 대응되는 공인IP가 있어야 하며, 구글 DNS(8.8.8.8), 클라우드플레어 DNS(1.1.1.1) 등의 DNS서버가 PC에 설정되어 있어야 한다. 물론 수동으로 설정하지 않아도 자동으로 ISP 사업자에서 운영하는 DNS 서버로 할당 될 수 있다. 

일반적으로 공인 IP는 하나의 도메인에 맵핑되기 때문에 DNS 조회를 하면 다음과 같은 결과를 볼 수 있다. 

```bash
$ nslookup google.com
Server:		192.168.10.1
Address:	192.168.10.1#53

Non-authoritative answer:
Name:	google.com
Address: 216.58.220.142

$ nslookup facebook.com
Server:		192.168.10.1
Address:	192.168.10.1#53

Non-authoritative answer:
Name:	facebook.com
Address: 31.13.77.35
```

위의 결과에서 볼 수 있듯이 `google.com`, `facebook.com` 은 하나의 공인 IP를 반환하는 것을 확인할 수 있으나, 일부 서비스의 경우 같은 도메인에 여러개의 공인 IP주소가 할당 될 수 있다. 

```bash
$  nslookup naver.com
Server:		192.168.10.1
Address:	192.168.10.1#53

Non-authoritative answer:
Name:	naver.com
Address: 223.130.195.95
Name:	naver.com
Address: 125.209.222.142
Name:	naver.com
Address: 223.130.195.200
Name:	naver.com
Address: 125.209.222.141

$ nslookup instagram.com
Server:		192.168.10.1
Address:	192.168.10.1#53

Non-authoritative answer:
Name:	instagram.com
Address: 54.237.7.164
Name:	instagram.com
Address: 3.213.82.30
Name:	instagram.com
Address: 54.236.65.131
Name:	instagram.com
Address: 3.94.174.10
Name:	instagram.com
Address: 34.234.142.160
Name:	instagram.com
Address: 52.54.235.161
Name:	instagram.com
Address: 34.237.204.39
Name:	instagram.com
Address: 52.73.101.12
```

우리가 일반적으로 알고 있는 방식과 다르게 `naver.com`, `instagram.com` 하나의 DNS에 여러개의 공인 IP주소가 보여준다. 이외에도 다양한 글로벌한 서비스도 이와 동일하게 표기된다. 

![](https://t1.daumcdn.net/cfile/tistory/2413C14E590B4F8A3A)



이러한 방식을 라운드로빈 DNS (Round-Robin DNS)라고 한다. 일반적인 라운드로빈 방식과 다르게 별도의 LB장비나 소프트웨어 방식을 사용하지 않고, DNS에서 레코드 정보를 조회하는 시점에서 트래픽을 분산한다. 웹에서 사용하는 HTTP/HTTPS에서만 사용하는 것이 아니라 FTP, SMTP 등에서도 사용할 수 있다. 서버는 각각의 공인 IP를 가지고 있고 사용자가 도메인을 통해 접속하는 순간 DNS에서 레코드 조회시 반환하는 하나의 공인 IP로 접속하게 된다. 이러한 방식을 통해 자연스럽게 각 서버에 부하가 분배된다. 

라운드로빈 DNS를 통해 반환된 공인 IP 리스트를 처리하는 표준 절차가 정해져 있지 않기 때문에 일반적으로 DNS 쿼리 요청시 첫 번째로 도착한 주소로 연결을 시도하거나, 애플리케이션이 임의로 선택하는 경우도 존재하며, A 레코드간의 순환이 이루어질 수 있다. 즉 사용자의 애플리케이션이나 OS에 따라서 동작하는 방식이 다르다. 하지만 클라이언트들이 어떤 서버를 사용할 것인지 결정하기 때문에 낮은 비용으로 여러 대의 서버로 트래픽을 분산할 수 있다는 장점이 있다. 

LB는 Round Robin, Weighted Round Robin, IP Hash, Least Connection, Least Response Time과 같은 다양한 알고리즘이 존재하는데, 이를 가능하게 위해서는 애플리케이션이 정상적으로 동작하는지 확인하는 health check가 포함된다. 또한 실제 서비스가 외부로 노출되지 않기 때문에 보안적인 측면에서는 어느정도 안전하다고 할 수 있으며, LB가 사용자와 서버간의 중계자 역할을 수행하기 때문에 투명성이 보장된다. 

하지만 라운드로빈 DNS는 LB 처럼 health check 기능이 없기 때문에 목록에있는 주소 중 하나의 서비스가 실패하면 DNS는 해당 주소를 계속 제공하고 클라이언트는 여전히 작동 불가능한 서비스에 도달하려고 시도한다. 이는 서비스 장애로 나타날 수 있으며, 네트워크 응답시간이나 서버 부하상태와 같은 상태를 고려하지 않는다. 따라서 무중단 서비스에서 라운드로빈 DNS를 사용하면 안된다. 

이러한 문제점을 해결하기 위해 GLSB (Global server load balancing)를 사용할 수 있다. GLSB는 등록된 호스트에게 주기적으로 health check를 수행하고, 네트워크의 거리 혹은 지역에 따라 주기적으로 성능을 측정하고 결과를 저장하여 DNS 쿼리시 지리적, 네트워크 거리가 가까운 서버를 반환한다. 물론 지리적으로 가까운 서버는 RTT도 작기 때문에 동일한 결과를 반환하는 경우도 많다. 

![](https://t1.daumcdn.net/cfile/tistory/99B7C23D5AF5267633?download)

>  GLSB의 자세한 개념과 더불어 Edge DNS GSLB는 다음시간에 자세히 설명하도록 한다. 



라운드로빈 DNS를 사용함으로서 간편하게 서버의 부하를 줄일 수 있지만 세션이 필요한 서비스의 경우, DNS 퀴리 주소가 변경되어 기존 세션이 끊어지는 경우가 생길 수 있기 때문에 세션을 공유할 수 있도록 하는 세션 클러스터링 기술을 사용하거나, L7 LB에서 IP가 쿠키값을 사용하여 동일 서버로 접속할 수 있도록 Sticky Session으로 동인한 공인 IP로 접속하도록 설정할 수 있다. 













 