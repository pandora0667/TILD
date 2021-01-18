# Public Cloud 기본

![](https://miro.medium.com/max/3000/1*YTVgxkvq3YLyHxJi9r1ktw.png)

1980년대 전후하여 원격지와 컴퓨터 근처에 있는 단말기 사이에 있는 수많은 네트워크 장비를 전부 그리지 않고 구름을 통해 추상화한 것이 시초가 되어, 2000년대에 들어 클라우드 컴퓨팅이라는 개념이 완성되었다. 모든 서버 및 네트워크 장비와 같은 인프라스트럭처는 데이터센터에 존재하며, 사용자는 이를 인지하지 않고 자원을 임대하여 사용한 만큼 비용을 지불하고 서비스 규모를 자유자재로 설정할 수 있는 특징이 있다.

## IDC와 클라우드

### IDC

온라인 서비스를 운영하는데 있어서 필요한 서버는 반드시 필요하지만 물리적 공간 및 전력 및 온습도를 제어하기 위한 항온항습 및 방음시설 등을 갖춰야 하는 불편함이 있다. 대기업을 제외하고는 대부분의 기업에서는 이러한 서버를 자체 구축하여 운영하는 일이 쉽지 않기 때문에 IDC(Internet Data Center)를 사용하고 있다.

![](http://www.giganetusa.com/wp-content/uploads/data/sc847.jpg)![](http://www.digitaltoday.co.kr/news/photo/200907/2(14).jpg)

IDC는 서버 및 네트워크 장비를 안정적이고 전문적으로 관리하기 위한 시설로, 온도와 습도를 일정하게 유지하기 위한 항온항습, 전력을 안정적으로 공급받기 위해 2곳 이상의 변전소를 사용하며, 자체 비상발전시스템과 UPS 등의 시설을 유지한다. 사용자는 IDC에 일정 공간을 임대하는 코로케이션(Co-location) 방식으로 계약을 진행한다. 코로케이션은 서버 비용과 월 이용료를 감안하면 최소 수천만 원 이상의 금액이 청구되기 때문에 일부 회사들은 이러한 형태의 코로케이션을 임대받아서 다시 더 작은 형태의 서비스로 나눠서 제공하는데 이를 서버 호스팅이라고 한다.

서버 호스팅을 통해 사용자에게 서버를 빌려주고 이에 대한 관리비와 설치비 등을 청구하게 된다. 하지만 일부 사용자에게 있어서 대규모의 서비스를 운영하는 경우가 아니라면 하나의 서버도 고성능이기 때문에 이를 가상화 방식으로 여러 사용자에게 나눠서 임대하는 방식을 가상서버 호스팅 방식이라고 한다. 이러한 방식은 초기 계약 당시 서버의 성능과 네트워크 비용이 고정된 상태에서 6개월 혹은 1년 단위로 계약을 진행하고 매월 금액을 지불해야 한다.

사용자 입장에서는 서버를 직접 구매하고 설치 및 유지보수까지 IDC에서 대행하는 측면에 있어서 긍정적인 부분이 있으나, 서비스 규모가 일정하지 않은 상태에서 IDC를 사용하는 것은 부담이 될 수 밖에 없다.

### 클라우드

클라우드는 On-Demand Service 즉 주문형 서비스가 핵심으로 사용자가 원하는 시점에 사용하고, 더이상 필요하지 않으면 서비스를 종료하는 방식으로 구성된다. 이러한 서비스를 제공하는 업체로는 AWS, GCP, AZURE가 있다. 초기에는 시간 단위로 가격을 책정했으나 현재는 초단위로 트래픽을 측정하여 비용을 청구한다. 이러한 계산방식은 기존 IDC 계약방식에 비해 유연한 서비스 구성이 가능하며, 실제 사용한 만큼의 서비스 자원에 대한 비용을 청구한다는 부분에서 효율적이라고 할 수 있다.

만약 DAU가 100명 미만인 서비스를 운영하고 있다고 가정할 때, 어떠한 이유로 인해 MCU가 1만명을 도달했다면 IDC나 서버를 구축한 상태에서는 트래픽을 받아들이지 못하고 서버가 죽어버리는 사태가 일어나겠지만, 클라우드를 사용하면 별다른 노력 없이도 즉각적으로 자원을 늘릴 수 있기 때문에 능동적으로 대처할 수 있으며, 추후 트래픽이 정상적으로 돌아올 경우 자원을 줄이는 방법을 사용할 수 있다.

![](https://miro.medium.com/max/609/1*uS9J8btKCQaMOhnUXp62aA.jpeg)

또한 클라우드는 단순한 컴퓨터 자원을 제공하는 것을 넘어서 네트워크, 애플리케이션, 스토리지와 같은 다양한 기능은 SaaS 형태로 제공하고 있다. 이를 통해 개발자는 인프라스트럭처의 개념을 이해하지 않고 개발에 집중할 수 있다.

### 서비스 성장 추이의 지표

위에 설명에서 DAU, MCU와 같은 단위를 작성했는데 서비스 성장에 있어서 IT 업계에 많이 사용하는 용어에 대해 간단하게 정리하도록 한다,

-   MAU (Monthly Active User)
    
    -   한 달에 몇 명이나 서비스를 이용하는가?
-   WAU ( Weekly Active User)
    
    -   집계기간이 짧아서 서비스의 규모를 파악하기 힘들 때, 7일을 기준으로 사용자 수를 파악
-   DAU (Daily Active User)
    
    -   하루에 몇 명이나 서비스를 사용하는지에 대한 지표로, 동시접속자와는 다른 의미를 가진다.
-   MCU (Maximum Current User)
    
    -   순간 동시 접속자를 의미하며, 실시간으로 수치를 계산한다. 이와 비슷한 단어로 ACU (Avarage Current User)를 통해 평균 동시 접속 사용자를 사용한다.
-   Stickiness
    
    -   DAU/MAU = Stickiness 수치로서 월간 사용자 대비 일간 활성 사용자의 비율로 계산한다. 여기서 DAU는 어제, MAU는 최근 30일 간의 사용자 수이다.
    -   이를 통해 월간 순수 사용자가 얼마나 접속했는지 알 수 있으며, 서비스의 재방문 정도를 관리해서 의존도를 알 수 있다. 값이 클수록 재방문율이 높다는 것을 의미한다.

## 퍼블릭, 프라이빗, 하이브리드, 멀티 클라우드

-   **퍼블릭 클라우드**
    
    -   우리가 일반적으로 사용하는 클라우드 컴퓨팅의 일반적인 유형으로 클라우드 리소스(예: 서버 및 스토리지)는 타사 클라우드 서비스 공급자가 소유하고 운영하며 인터넷을 통해 제공한다.
    -   퍼블릭 클라우드를 사용할 경우 모든 하드웨어, 소프트웨어 및 기타 지원 인프라를 클라우드 공급자가 소유하고 관리한다.

-   **프라이빗 클라우드**
    
    -   기업 혹은 조직에서 독점적으로 사용하는 클라우드이다. 프라이빗 클라우드는 실제로 조직의 현장 데이터 센터에 있거나 타사 서비스 공급자가 호스팅 할 수 있다.
    -   프라이빗 클라우드에서는 서비스와 인프라가 항상 프라이빗 네트워크에서 유지 관리되며, 하드웨어와 소프트웨어는 조직에서만 전용으로 사용된다.
    -   특정 조직이 요구하는 기준을 대부분 만족할 수 있으며, 중요한 기업 혹은 조직의 데이터를 관리하는 중견 조직과 대규모 조직 외에도 정부 기관과 금융 기관이 사용한다.

-   **하이브리드 클라우드**
    
    -   온 프레미스 혹은 프라이빗 클라우드와 퍼블릿 클라우드를 결합하는 클라우드 컴퓨팅 유형이다.
        
        ![](https://www.cloudflare.com/resources/images/slt3lc6tev37/6Su5n7q8QC7fAsQDgaLnMz/e0b9106dedc8b779933c2cb56bb91abf/hybrid_cloud-public-cloud-vs-private-cloud.svg)
    -   개발/론칭/배포 등 전방적인 Dev/Ops 관점 및 규모 유연성, 비즈니스 지향적이기 때문에 효율적인 IT 인력 운영이 가능하고 신기술 도입과 적용이 용의 하다.
        
    -   단, 미션 크리티컬 한 워크로드까지 퍼블릭 클라우드에 두기는 부담스러운 경우 프라이빗 클라우드에 적용한다.
        
        -   실시간 실행이 요구되는 경우
        -   보안상 민감한 작업 혹은 데이터를 다루는 경우
        -   기업의 내부 작업, 혹은 정부, 금융과 같은 유간 기관의 규제 적용 대상의 경우
    -   퍼블릭 -> 프라이빗 혹은 프라이빗 -> 클라우드 간의 분산 재배치가 용의 하게 이루어질 수 있다.
        

-   **멀티, 멀티/하이브리드 클라우드**
    
    ![](https://www.cloudflare.com/resources/images/slt3lc6tev37/2FUanuH7qCS1oycfYY4IMn/6b790f0e98674ce50c37cf8909d8a4b2/multicloud-vs-hybrid-cloud.svg)
    -   멀티 클라우드 및 멀티/하이브리드 클라우드는 다수의 클라우드와 결합하여 사용하며, 서비스의 안정성 규모 확장, 의존성을 줄이는 방식이다.
    -   서로 상의한 클라우드끼리 통신하기 위해서 다음과 같은 방법을 사용한다.
        -   API (Application Programming Interfaces)
        -   VPN (Virtual Private Network)
        -   WAN (Wide Area Networks)
    -   각각의 클라우드 끼리 통신이 원활하지 않다면 별도의 클라우드 환경을 병행으로 구성하는 것에 불가하며, 이를 유기적으로 관리 구성하기 위한 전략과 안정적인 네트워크 구성이 필요하다.
    -   퍼블릭, 프라이빗, 하이브리드 클라우드의 모든 장점을 수용한다는 측면에서 유리하다고 할 수 있지만 초기 설계 복잡도가 증가하는 단점이 있다.

## 클라우드가 능사는 아니다.

온 프레미스 환경이나 IDC를 완벽하게 대체하기 위한 수단으로 클라우드를 사용하는 것이 모두 정답이 될 수 없으며, 기업의 환경이나 상황에 따라 인프라스트럭처의 구조는 변경될 수 있다. 위에서 설명했듯이 이러한 문제점들을 보안하고 해결하기 위한 다양한 방식의 클라우드 시스템이 출시되었으나 충분한 이해가 없이 무작정 사용하는 것은 지양해야 한다. 또한 요금 측면에 있어서 100% 저렴하다고 할 수 없다.

클라우드는 튜닝 기술이나 시스템 및 네트워크 구성 등의 역량에 따라서 요금 과금이 천차만별이기 때문에 이러한 부분도 반드시 고려해야 할 것이다.