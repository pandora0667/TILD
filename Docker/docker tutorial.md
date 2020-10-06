# 불좀 꺼줄래? 내 Docker좀 보게 

[toc]

## 서버와 네트워크 간단한 역사

​	1957년 인류 최초 인공위성인 소련의 스푸크니호의 발사가 성공하자 핵전쟁 등의 상황에서도 살아남을 수 있는 네트워크를 연구하였고 이 결과 기존의 회선 교환(circuit switching)방식보다는 패킷 교환(packet switching)방식이 매우 견고하고하다는 것을 발견하여, 미국 국방부에서는 1962년 ARPANET (Advanced Research Projects Agency Network) 프로젝트를 시행, 1969년 로스 앤젤레스에 있는 캘리포니아 대학교에 that's us! 메시지를 전송하는데 성공한다. 

* 1969년 ARPANET 구성도 

  ![](http://classes.design.ucla.edu/Spring06/161A/projects/camile/arpanet/1969thumb.jpg)

* 1977년 ARPANET 구성도

  ![](http://classes.design.ucla.edu/Spring06/161A/projects/camile/arpanet/1977thumb.jpg)

  ​	초기 군사용 및 연구용으로 사용되던 ARPANET은 사용요구가 많아지면서 1983년 미국 국방성은 군사용 네트워크를 분리시키고 아파넷을 대중에게 공개되었다. 인터넷이란 단어가 처음 등장한것은 1973년 TCP/IP를 정립한 Robert E. Kahn, Vinton Gray Cerf가 네트워크의 네트워크'를 구현하여 모든 컴퓨터를 하나의 통신망 안에 연결(International Network)하고자 하는 의도에서 이를 줄여 인터넷(Internet)이라고 처음 명명하였던 데 어원을 두고 있다.  

  ​	현재와 같이 TCP/IP 프로토콜이 표준 규격으로 사용된 것은 1983년 1월 1일 ARPANET이 NCP 패킷 송출 중단 이후 지금까지 표준 프로토콜로 계속사용하고 있다. 한국에서는 1982년 5월 전길남 박사가 서울대학교와 한국전자통신연구원(ETRI) 구미 전자기술연구소가 TCP/IP를 이용하여 1200bps 전화선으로 연결한것이 시초가 되었고 이는 전 세계에서 두 번째로 인터넷 연결을 성공한 국가가 되었다. 

  ​	이후  대한민국의 인터넷 이용자 수는 급증하였고 글로벌 서비스가 활성화 되면서 인터넷을 통한 다양한 서비스들이 등장하게 되었고, 이를 구축 운영하기 위한 서버 도입도 활발하게 도입되었다. 

  <img src="https://upload.wikimedia.org/wikipedia/commons/8/8a/Korean_internet_statistics.png" style="zoom: 67%;" />

  2020년 현재 사용자와 서버를 연결하기 위한 네트워크 구성을 상상을 초월할 만큼 촘촘하게 구성되기 시작되었고, IoT, 인공지능, 5G 기술에 힘입어 4차 산업혁명의 길을 겪으며, 인류가 5000년동안 만들어낸 데이터가 오늘날에는 단 하루만에 생성되는 세상에서 살고 있다. 

  * 라우터로 연결된 인터넷을 시각화한 그림

    <img src="https://upload.wikimedia.org/wikipedia/commons/d/d2/Internet_map_1024.jpg" style="zoom:50%;" />

    

  이렇게 연결된 다양한 기기는 네트워크를 통해 연결되고 최종목적지인 서버를 통해 데이터를 처리하게 된다. 서버는 어떠한 서비스나 데이터를 처리하기 위한 컴퓨터이며, 수많은 트래픽을 감당하기 위해서는 고성능의 컴퓨팅 파워를 요구하게 된다. 이러한 데이터를 처리하기 위한 서버와 네트워크 장비가 모여있는 곳을 데이터 센터라고 부르며. 여기서 사용되는 전력은  매년 약 200TWh 세계 전력 사용량의 1%에 해당한다. 

  ![](https://cdn11.bigcommerce.com/s-zky17rj/images/stencil/500x500/uploaded_images/rackservers.jpg?t=1541631413)



​	시간이 지나며 하드웨어의 성능은 급격하게 향상되고, 이런 하드웨어 성능을 효율적으로 처리하고자 하는 다양한 연구가 진행되었으며, 우리는 앞으로 컨테이너를 활용한 가상화 방식에 대해 알아보도록 할 것이다. IT 산업의 기술 발전을 위해 노력했던 연구진과 실무진에게 감사의 마음을 표하며 본격으로 시작하도록 하겠다. 



## 가상화

​	하드웨어의 성능향상이 비약적으로 발전하게 되면서 하나의 서버의 자원이 100% 사용하지 않는다는 사실을 알게 되었다. 기존에는 서버의 특성에 따라 서버를 구입하고 설치 및 셋팅을 진행하면서 서비스의 규모가 늘어남에 따라 막대한 비용과 시간이 들어가는 비효율적인 시스템을 가지게 되었다. 이러한 문제점을 해결하고자 다양한 가상화 기법이 탄생하게 되었고 가상화의 종류는 다음과 같이 정리할 수 있다. 

* Application 
  * 자바의 JVM
* H/W 
  * Hypervisor
* Desktop
  * VNC, X-Windows, Remote Desktop, Virtual Box, VMware  etc.. 
* Network
  * VPN, VLAN, SDN etc .. 
* Server 
  * 전가상화, 반가상화, 운영 체제 수준 가상화
* Memory
  * Page Table Entry, MMU (Memory Management Unit), TLB (Translation Looaside Buffer)
* 스토리지
  * VMFS, VMDK etc ... 

위에서 알 수 있듯이 우리가 익숙한 것들도 있고 아닌것들도 있을것이다. 여기서 중요한 점은 이 모든 가상화 기술은 한정된 자원을 얼마나 효율적으로 사용할 수 있을지에 대한 고민에서 탄생했다는 것이다. 이번시간에서는 서버의 자원을 효율적으로 사용할 수 있는 방법에 대해 집중적으로 알아보고 왜 컨테이너 기법이 탄생하게 되었는지 알아보도록 하겠다. 



### 서버 가상화? 

​	서버 가상화에서 가장 핵심이 되는것이 바로 하이퍼바이저이다. 우리가 사용하고 있는 윈도우, 리눅스, mac os 등은 기본적으로 컴퓨터에 직접 설치되고 하드웨어와 프로그램 및 스케줄링과 같은 대부분의 중요 작업은 운영체제의 커널에서 수행된다. 즉, 우리가 어떤 운영체제를 사용하는지에 따라서 커널의 역할을 비슷하지만 그 동작방식이 다르다고 할 수 있으며, 이에 따라 다른 운영체제와 호환할 수 있는 애플리케이션을 제작하기 위해서는 응용 프로그래머가 이에 맞춰서 별도로 개발해야 하는 수고스러움이 존재한다. 

​	그럼 서버를 가상화 하면서 얻을 수 있는 이점은 무엇이 있을까? 바로 컴퓨팅 자원을 여러개로 분할하여 최대한 사용하여 그 효율성을 극대화 할 수 있다는 점이다. 

![](https://vapour-apps.com/wp-content/uploads/2016/05/figure1.gif)

위 그림에서 볼 수 있듯이 기존에는 3가지의 애플리케이션을 실행하기 위해서는 3개의 서버가 별도로 존재하는것을 알 수 있다. 하지만 가상화를 사용하면 하나의 하드웨어 즉 서버에 3가지의 운영체제와 각각의 애플리케이션이 동작하는 모습을 볼 수 있다. 그렇다면 이런 생각이 들수도 있다. ""우리가 사용하는 애플리케이션이 하나의 컴퓨팅에서 여러개 동작하듯이 서버하나에서 여러개의 애플리케이션을 동작하면 되지 않을까?" 결과론적으로 말하자면 가능하다. 개발용 머신이라면. 우리가 만약 실제 서비스를 운영한다고 가정하고 일반적인 환경인 웹서버와 데이터베이스 , UI 서버를 하나의 서버에서 동시에 돌린다고 생각해보자. 만약 이러한 구조에서 서버의 장애가 발생하거나, 서비스의 자원이 부족하여 서버를 증설헤야하는 상황이 생긴다면 여러분은 어떻게 대처할 것인가? 

​	테스트 및 개발환경에서는 하나의 서버에서 다수의 응용 애플리케이션을 통해 테스트를 진행하는데 있어서 아무런 지장이 없을 수도 있겠지만 실제 서비스 환경에서 장애는 기업에 심각한 비용 손실을 발생할 수 있다. 따라서 각 서버마다 웹서버, 데이터베이스 서버, UI 서버 등 각 기능에 맞춰서 컴퓨팅 자원을 서로 다르게 구입하거나, 어떠한 기능에 특화된 서버를 구입할 수 있다. 하지만 가상화를 사용하면 사용하는 컴퓨팅 자원 즉 하드웨어는 동일하지만 격리된 공간에서 독립적으로 구성되기 때문에 특정 서버에 문제가 발생하더라도 다른 서버에 문제가 생기지 않고, 서버 구입 비용을 획기적으로 줄일 수 있으며 백업과 복구의 관리가 용의해진다는 장점이 있다. 만약 본인이 실제 운영서버 한대에서 모든 애플리케이션을 동작시키는 형태로 나아간다면 최악의 선택이 될 수 있다. 



### 부록 - 서버 가상화는 언제부터? 

​	가상화 기술은 상당히 오래전부터 사용되어왔다. 1967년 1월에 제작된 메인 프레임 IBM CP-40은 다음에서 살펴볼 전가상화 (Full virtualization) 기능을 제공하였다. 이 메인프레임은 가상화 기술을 지원하기 위해 개조된 전용 S/360-40에서 실행되었고 여러 개의 운영 체제가 동시에 실행할 수 있을만큼 가상화 기능을 제공했다. 

> 사실 그 이전부터 컴퓨터 하드웨어는 여러 개의 사용자 응용 프로그램을을 실행하기에 충분한 가상화가 이루어지고 있었다.  

CP-40은 곧 전가상화 기능을 가진 첫 컴퓨터 시스템이었던 IBM System/360-67용의 CP-67로 다시 작성되어 1966년 출하를 시작되었고, 우리가 운영체제에서 배운 내용은 하드웨어 방식의 페이지 변환 테이블도 가지고 있었다. 다음 그림은 실제 메인프레임의 사진이다. 

<img src="https://ssh.guru/content/images/2018/08/ibmcp67.jpg" style="zoom: 67%;" />

![](https://upload.wikimedia.org/wikipedia/commons/6/69/IBM360-67AtUmichWithMikeAlexander.jpg)

![](https://upload.wikimedia.org/wikipedia/commons/d/dc/IBM360-67ConfigConsoleAtUmich.jpg)



### 서버 가상화의 종류? 

![](https://phoenixnap.com/kb/wp-content/uploads/2019/01/hypervisor.png)

​	서버 가상화의 종류는 크게 2가지로 분류된다. 첫번째 Type 1 hypervisor (native / bare-meta / 반가상화), 두번째 Type 2 hypervisor (hosted / Full virtualization / 전가상화)가 있다. 본격적으로 우리가 서버 가상화 종류을 이해하기 전에 반드시 숙지해야 할 부분이 있다. 

​	우리가 일반적으로 운영체제를 설치하면 설치된 운영체제가 커널을 통해 하드웨어를 제어하게 되는데,  하이퍼바이저의 자체에서는 하드웨어를 제어할 수 없기때문에 Dom0 (Domain 0)라는 관리 머신이 함께 동작한다. Dom0는 하드웨어에 접근할 수 있는 특별한 권한을 가지고 있고 디바이스 드라이버 역시 포함되어 있다. 즉 하이퍼바이저에서는 Dom0가 없이는 동작할 수 없다. 따라서 Dom0가 얼마나 많은 역할을 담당하는지에 따라 가상화의 종류가 결정된다. 이와 별개로 DomU (Domain U)는 하드웨어에 직접 접근할 수 있는 권한이 없는 비특권 도메인으로 가상 디스크나 네트워크 접근에 대한 부분을 관리한다. 만약 DomU가 실질적으로 물리 하드웨어에 동작을 원한다면 반드시 Dom0와 통신해야 한다. 

* **아래 그림은 XEN 하이퍼바이저를 기반으로 설명한 그림이며, 동작과정은 다른 하이퍼바이저와 비슷하다.** 

![](https://wiki.xenproject.org/images/0/09/Xen_arch1.png)

![](https://wiki.xenproject.org/images/c/c1/Xen_arch2.png)

#### 그래서 Type 1, Type 2 방식의 차이가 무엇일까?

![](https://upload.wikimedia.org/wikipedia/commons/e/e1/Hyperviseur.png)



1. Type 1 hypervisor (native / bare-meta / 반가상화)

   ![](https://www.flexiant.com/wp-content/uploads/Type-1-Hypervisor.png)

   ​	Type 1 hypervisor를 bare-meta로 부르는 이유는 "어떤 소프트웨어도 담겨있지 않은 하드웨어"라는 의미이기 때문이다. 즉 하드웨어 위에 운영체제가 설치되지 않고 하이퍼바이저가 하드웨어를 직접 컨트롤 하는 것을 말한다. 단일 하이퍼바이저가 하드웨어를 종합적으로 관리하기 때문에 오버헤드가 적고 리소스 관리에 유연하다는 장점이 있다. 하지만 운영체제 즉 커널이 정상적으로 동작하기 위해서는 system call이 아닌 hyper call를 통해 요청을 처리해야 한다. 이는 위에서 설명한 하이퍼바이저의 Dom0와 DomU에 명령을 실행하기 위한 별도의 명령어가 필요하다는 뜻이기 때문에 커널을 수정하지 않는 이상 Type 1 hypervisor 방식을 사용할 수 없다. 리눅스나 유닉스의 경우 오픈소스 형태이기 때문에 커널을 수정하는것이 쉬웠으나 윈도우의 경우 커넗을 공개하지 않기 때문에 원칙적으로라면 실행하지 못하는게 정상이다. 하지만 XEN과 마이크로소프트가 Type 1 hypervisor를 지원하도록 커널을 수정하고 해당 프로그램을 제공하기 때문에 사용하는데 지장은 없다. 

   ​	종류에는 VMware ESX and ESXi, Microsoft Hyper-V, Citrix XenServer, Oracle VM이 있다. 

   

2. Type 2 hypervisor (hosted / Full virtualization / 전가상화)

   ![](https://www.flexiant.com/wp-content/uploads/Type-2-Hypervisor.png)

   ​	Type 2 hypervisor의 경우 하드웨어 위에 일반적인 운영체제가 동작하고 있고 그 위에 하이퍼바이저를 설치하고, 모든 하드웨어 자원을 가상화하여 운영하는 방식이다.  모든 자원을 가상화하고 이를 각각의 하이퍼바지어저가 전달해주는 방식이기 때문에 Type 1 hypervisor 보다 상대적으로 오버헤드가 크게 나타날 수 밖에 없지만 게스트 OS의 수정이 전혀 필요하지 않다. 또한 기존 운영체제를 사용하면서 가상머신을 운영할 수 있다는 장점이 있기 때문에 우리가 가장 흔하게 접근할 수 있는 방식이다. 

   ​	종류에는 VMware Workstation/Fusion/Player, VMware Server, Microsoft Virtual PC, Oracle VM VirtualBox, Red Hat Enterprise Virtualization, KVM이 있다.      

### 부록 - operating-system-level virtualization

​	operating-system-level virtualization 한국어로는 운영체제 수준 가상화이다. 운영체제의 커널이 하나의 사용자 공간을 가지고 있는 것이 아니라 여러 개의 격리된 사용자 공간을 가질 수 있도록 하는 서버 가상화 방식이다. 어렵게 보일 수 있지만 컨테이너, 소프트웨어 컨테이너, 가상화 엔진, jail 등이 여기에 속하며 사용자 관점에서는 독립된 다른 서버로 보이게 해준다. 이의 기술 표준이 chroot다. chroot는 1979년 유닉스 버전 7 개발중에 도입되었고 1982년 빌 조이가 BSD에 추가했다. 일반적인 커널에서는 자식 프로세스는 실행되고 있는 자신의 영역이외의 파일을 접근할 수 없기 때문에 이를 루트 디렉토리로 변경할 수 있는 역할을 담당한다. 쉽게 말하면 일종의 감옥을 만들고 그 감옥안에 루트 디렉토리를 사용할 수 있도록 변경하는 것이다. 

​	이러한 방식을 통해 하나의 커널안에서도 여러개의 독립적인 사용자 영역을 가질 수 있게 되는 구조를 가질 수 있게 되고, 우리가 학습할 Docker의 기본 지식이 된다. 



## 컨테이너

​	우리가 일반적으로 생각하는 컨테이너는 무엇인가? 

![](http://files.itworld.co.kr/archive/image/2016/10/5939008153_270d7f65a6_b.jpg)

선박 운영에 있어서 사용되는 컨테이너는 일종의 표준 규격을 가지고 안에 화물을 넣어서 쉽게 이동할 수 있는 인류의 물품수송에 일대 혁명을 가져온 발명품이다. 컨테이너가 없던 시절 크레인과 인력을 동원하여 화물을 적양하 하는것이 일반적이였다. 여기서 발생하는 인력과 시간 및 사고와 물자 분실은 필수 불가결한 존재였다. 하지만 컨테이너가 등장하면서 규격화된 크레인과 수송체계만 있으면 과정이 매우 단순화되며 비용도 크게 줄었다. 

​	우리가 학습할 컨테이너도 운송용 컨테이너와 다르지 않다. 만약 본인이 Go로 작성된 어떠한 애플리케이션을 만들고 있다고 가정한다. 당연히 본인 컴퓨터에서는 관련 환경설정이 되어 있기 때문에 개발하고 운영하는데 아무런 지장이 없겠지만 다른이가 내가 만든 애플리케이션을 이어서 개발하거나 실행하고자 한다면 본인과 다른 컴퓨팅 한경때문에 정상적으로 실행이 되지 않을 것이고 이를 실행하기 위한 별도의 셋팅과정이 필요할 것이다. 만약 이렇게 만들어진 애플리케이션이 다양한 라이브러리와 의존성을 가지고 있다면, 서로 다른 개발자 컴퓨터 사이의 개발환셩 셋팅에 따라 오류가 발생할 수 있고 이를 해결하기 위한 엄청난 노력이 필요할 것이다. 이를 효과적으로 해결할 수 있는 방법이 바로 컨테이너이다. 

​	컨테이너는 위의 부록에서 반시 언급한  operating-system-level virtualization의 종류이다. 

![](https://www.redhat.com/cms/managed-files/what-is-a-container.png)

운영체제 수준에서 가상화하는 방식을 채택하기 때문에 별도의 하드웨어가 필요하지 않고 본인이 실행하고 있는 운영체제 커널을 공유해서 컨테이너를 실행하게 되며 하이퍼바이저를 사용하는 가상화 기술대비 매우 빠른 속도로 실행되는 특징을 가지고 있다. 물론 프로세스를 격리하기 때문에 약간의 오버헤드를 가질 수 있지만 하이퍼바이저에서 발생하는 오버헤드에 비하면 미미한 수준이며, 하나의 운영체제에서 다수의 프로세스가 실행되고 있는것과 같이 다수의 컨테이너를 실행하는 것도 가능하다. 

​	이렇게 격리된 컨테이너는 호스트의 환경이 아닌 독자적인 실행환경을 가지고 있기 때문에 다른 운영체제 혹은 다른 개발 환경에서도 독립적으로 실행이 가능하고, 상태를 가지지 않는 특성 때문에 다른 컨테이너에게 영향을 주지 않으면서 파일 혹은 이미지 형식으로 쉽게 공유할 수 있다. 이러한 컨테이너의 기능을 잘 이해하기 위해서는 리눅스 컨테이터의 동작을 이해할 필요가 있다. 



#### 리눅스 컨테이너 LXC

![](https://www.geeksforgeeks.org/wp-content/uploads/linux-virtualization.png)

​	리눅스 컨테이너 LXC는 리눅스에서 컨테이너 기능을 제공하기 위한 인터페이스다. 기본적으로 API를 제공하기 때문에 리눅스 사용자가 쉽게 시스템 또는 애플리케이션 컨테이너를 생성하고 관리할 수 있게 도와주는 역할을 수행한다. 리눅스 컨테이너 LXC가 운영체제 수준 가상화이기 때문에 namespce, Apparmor와 SELinux, Seccomp, chroot, CGroups과 같은 커널 기능을 사용한다. 여기서 가장 중요하게 살펴봐야 할 것이 namespace와 CGroups이다.

* **namespace** 

  * 하이퍼바이저에서는 게스트 머신이 독립적인 공간을 제공하고 서로 충돌이 나지 않도록 관리하는 기능을 가지고 있는데, 리눅스 커널에는 namespace가 동일한 역할을 담당한다. 리눅스 커널에는 9가지의 namespace가 제공되고 있는데 살펴보면 다음과 같다. 

    * mnt (파일시스템 마운트): 호스트 파일시스템에 구애받지 않고 독립적으로 파일시스템을 마운트하거나 언마운트 가능
    * pid (프로세스): 독립적인 프로세스 공간을 할당
    * net (네트워크): namespace간에 network 충돌 방지 (중복 포트 바인딩 등)
    * ipc (SystemV IPC): 프로세스간의 독립적인 통신통로 할당
    * uts (hostname): 독립적인 hostname 할당
    * user (UID): 독립적인 사용자 할당
    * UTS / Time Namespace : 독립적인 시스템 시간할당 및 서로 다른 호스트 및 도메일 이름 할당 
    * Control group (cgroup) Namespace : 자신이 속한 cgroup의 그룹의 상대적인 경로 제공

    ![](https://camo.githubusercontent.com/4e774a08fa4f0f63694906362b26b099e848d37d/68747470733a2f2f74686570726163746963616c6465762e73332e616d617a6f6e6177732e636f6d2f692f37353862387379676a6a6976797164703074687a2e706e67)

    ![](https://camo.githubusercontent.com/5a17e1529afffcd02d251093ff5d116ee687bcff/68747470733a2f2f74686570726163746963616c6465762e73332e616d617a6f6e6177732e636f6d2f692f62706e396673326c766d67696779767961376f742e706e67)

* **CGroups**

  * 커널에서 사용되는 자원 (resources)에 대한 제어를 가능하게 하는 기능을 말한다. 이 기능 없이는 각각의 컨테이너에서 별도의 자원을 사용할 수 없기 때문에 실행자체가 불가능하다. CGroups에서는 다음과 같은 자원을 제어한다. 

    ![](https://acg-wordpress-content-production.s3.us-west-2.amazonaws.com/app/uploads/2020/06/cgroups.png)

    ![](https://i.stack.imgur.com/uLPjc.png)

    * CPU
    * 메모리 
    * I/O (입출력) 
    * 네트워크 
    * device 노드 

  이렇게 정의한 컨셉을 그림으로 표현하면 다음과 같다. 

  ![](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e7/Linux_kernel_unified_hierarchy_cgroups_and_systemd.svg/1600px-Linux_kernel_unified_hierarchy_cgroups_and_systemd.svg.png)

  우리가 리눅스 운영체제에 커널에 대한 모든 내용을 전부 숙지할 수는 없갰지만 최소한 Docker 컨테이너를 재대로 이해해야 Docker에서 발생하는 여러 제약사항이나 문제점을 쉽게 이해하고 찾을 수 있는 해결력을 가질 수 있다. 더불어서 LXD라는 것도 존재하는데 이는 LXC를 사용하는 컨테이너 시스템이다. Docker를 사용하던 LXD를 사용하던지 상관없이 우리는 리눅스 커널의 LXC의 기능을 사용한다. 



### 부록 - 컨테이너는 만능일까? 

​	리눅스 컨테이너는 당연히 리눅스 커널에서 관리하기 때문에 리눅스 이외에는 동작하지 않고, 컨테이너 환경에서 다른 OS를 설치할 수 없다. 실습을 진행하면서 다른 운영체제를 다운받아서 설치하는 경우를 보면서 헷갈릴 수가 있지만 엄밀하게 말하면 실행중인 커널은 하나이기 때문에 다른 운영체제 컨테이너를 다운로드 받더라도 호스트 OS의 커널을 사용하면서 동작한다. 따라서 컨테이너별 별도의 커널 구성이 불가하기 때문에 컨테이너마다 다른 커널을 설치하고 운영하는 것 조차 불가능하다. 

​	그렇다면 윈도우는 컨테이너 사용이 불가능 할까? 원칙적으로는 불가능하지만 가상화 기술을 이용해 이를 보안한다. 기존 윈도우에서 컨테이너 즉 Docker를 사용하기 위해서는 윈도에서 제공하는 별도의 하이퍼바이저를 사용해서 리눅스를 설치하고 이 기반위에서 동작하게 구현을 해두었다. 하지만 WSL (Windows Subsystem for Linux)이 포함되면서 윈도우 커널안에도 리눅스 호환 커널 인터페이스를 제공하기 시작했고, WSL2를 통해 윈도우에서도 Docker 사용이 원할해 졌다. 

![](https://i2.wp.com/bbon.kr/wp-content/uploads/2020/06/ms_loves_linux.png?fit=653%2C367&ssl=1)

##  Docker

![](https://www.docker.com/sites/default/files/d8/2019-07/horizontal-logo-monochromatic-white.png)

​	Docker는 솔로몬 하익스 (Solomon Hykes)가 설립한 프랑스 PaaS기업인 dotCloud에서 내부 프로젝트로 진행되었던 컨테이너 시스템을 오픈소스로 공개하고 회사 이름을 Docker로 변경하게 되었다. 기존 리눅스 컨테이너의 기능을 상당부분 사용했으며, 사실상 업계 표준으로 사용되어 Docker로 작성된 패키지 / 이미지가 많아서 접근성과 사용하기 좋다는 장점을 가지고 있다. 



### Docker Installation 

​	본격적으로 Docker를 사용하기 이전에 각 운영체제별로 Docker를 설치하는 방법을 기술하도록 하겠다. 

#### Windows 10 

* **윈도우 2004 WSL2 기반으로 설치하는 방법을 기술하도록 하겠다.** 



#### Linux 

* **리눅스에서는 Ubuntu 20.04 LTS 버전을 기준으로 기술한다.** 

#### Mac OS

* **macOS Catalina에서 brew를 사용하여 설치하는 방법으로 기술한다.** 



----



### How to use Docker ? 

![](https://www.redhat.com/cms/managed-files/traditional-linux-containers-vs-docker_0.png)

#### docker search 

#### docker pull 

#### docker image

#### docker run 

#### docker ps 

#### docker start 

#### docker resrart 

#### docker logs

#### docker exec 

#### docker stop 

#### docker rm 

#### docker rmi 



### 부록 - 새로운 컨테이너의 등장 podmad 



## 실전연습 - Docker hub 사용하기 

### ubuntu 

### postgres 

### Nginx & httpd

###   

## Docker 데이터 저장하기 



## Docker backup & restore



### docker export 

### docker import 

### docker save 

### docker load 



## Dockerfile 작성하기 

### What is Dockerfile ? 

##### FROM 

##### LABEL 

##### ARG 

##### WORKDIR 

##### RUN 

##### ENV 

##### COPY 

##### EXPOSE 

##### CMD 

##### ENTRYPOINT 



### Dockerfile 이미지 만들기

#### docker build 

#### docker tag

#### docker push 



## 실전 연습 - Dockerfile로 이미지 만들고 push 하기 



### Simple Web Application with bootstrap



### Node.js Application



### Java Application 



### Python Application 



### Advanced Application 



## Overview of Docker Compose



### Docker Compose install 



### How to use Docker Compose ? 



### Docker compose command 

#### docker-compose up 

#### docker-compose stop 

#### docker-compose logs 



## 실전연습 - Docker Compose 애플리케이션 배포하기



## Container Cluster concept



### docker swarm 



### kubernetes 



### Container management tools 



