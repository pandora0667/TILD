# ANSIBLE 1장

![](https://cdn-images-1.medium.com/max/1800/1*hdwjXl1x4Q3VXmL7UG1XrQ.png)

### Why Asible?

* 시간이 지날수록 다양한 IT 시스템이 늘어나면서 관리자 한명이 관리해야 하는 역할들이 늘어나고 있습니다. 

* 단적으로, 하나의 서버에 배포환경을 구축하고, 소프트웨어를 설치하고, 운영 및 업데이트에 이르기까지 공통적인 일들을 반복적으로 수행해야 한다면 시간적으로 비효율적이며, 현재 인프라 환경에 맞지 않을 것입니다. 

* 불과 10여년 전만해도 IT 인프라는 물리 서버와 물리 네트워크로 구성된 일련된 센터를 통해서 운영되었습니다. 

  ![](https://www.progressivetech.com/wp-content/uploads/2016/06/iStock_84905873_SMALL.jpg)
  * 관리자는 몇 대 ~ 몇 십대 정도를 관리하는 정도로 장애가 발생하면 장애를 조치하는 등의 단순하고 반복적인 역할을 담당해야 했습니다. 
  * 각 서버별로 정해진 시스템 구성에 따라 운영되기 때문에 실제 설치한 서버의 시스템 자원을 효과적으로 사용할 수 없었으며, 이러한 고민은 2007년 경제 위기와 함께 기업들은 최적화라는 명분아래 비용절감을 IT전략의 우선순위로 두고 서버 가상화 기술을 염두하기 시작했습니다. 
  * 이렇게 발전하기 시작한 가상화(Virtualization)은 현재에 가장 기본적이며 필수적인 기술로 인식되어 사용되고 있으며, 현재는 가상화(Virtualization)을 사용하지 않은 곳을 찾기 힘들 정도 입니다. 
  * 또한 가상화(Virtualization)을 기반에 퍼블릭 클라우드, 프라이빗 클라우드를 넘어 하이브리드 클라우드까지 도입진행이 이루어지고 있는 환경에서 더이상 인프라 관리자가 Bash or Power Shell을 통해 관리하는 것이 힘들어지게 되었습니다. 

  ![](https://www.contegix.com/wp-content/uploads/2017/06/devops-process.png)

* 급속도로 변화하는 IT 서비스요구에 맞춰 인프라역시 기민성(Agility)와 유연성(Flexibility)을 충족해야 하는 시기가 도래하였습니다. 

  > IT팀은 단순하게 업무를 지원하는 팀이 아니다. 점차적으로 비즈니스와 매우 밀접한 관계를 가져가고 있으며 하루라도 IT 환경 없이 업무를 지속할 수 없는 조직이 거의 대부분이다. 이러한 형태라면 조직내 IT의 레벨이 전체적인 비즈니스 레벨을 좌우한다고 봐도 무방하다.
  >
  > IT팀에게 반복적으로 발생하는, 그리고 시간이 오래 걸리는 형태의 장애 처리 업무의 부담을 줄여주고, 이들이 조직내에 IT 기술을 통해 더욱더 생산적인 비즈니스를 이어갈 수 있는, 그리고 요새 많이 회자가 되고 있는 보안과 같은 추가적인 가치에 지식 습득 및 기술 구현을 투자할 수 있게 한다면, 조직은 IT 가치와 함께 생산성 및 비용 절감, 나아가 더 나은 비즈니스 결과를 얻을 수 있다.
  >
  > -zdnet-

* 따라서 업무의 연속성을 위해 서로가 자동으로 관리하게 만들어진 코드를 쉽게 이해할 수 있어야 하며, 이러한 생산성 도구가 사용하는데 어려워서도 안됩니다. 

* Ansible은 이러한 급변하는 IT 인프라에서 필수적으로 사용해야 하는 **배포 자동화 도구**가 되었습니다. 



### History of Ansible

* Ansible은 SF 소설에서 등장하는 공간적 거리에 관계없이 동시에 통신이 가능하도록 하는 기구, 즉 초광속 통신 장치가 본래의 의미입니다. 
* provisioning server application의 Cbbler의 저자이자 리눅스 컴퓨터 시스템의 원격 관리를 위한 오픈소스 소프트웨어 Func(Fedora Unified Network Controller) 프레임워크을 제작한 Michael DeHaan(마이클 데한)이 개발하였습니다. 
* 2012년 2월 20일 처음공개되어 2015년 10월 Red Hat에 의해 인수되었습니다. 



### Andible 왜 인기가 있을까? 

* "Simple, agentless IT automation that anyone can use" 이것이 Ansible이 추구하는 목표입니다. 
* Ansible이 유명해지고, 보급된 이유는  아래 3가지 개념을 통해 확인할 수 있습니다. 
  * IaaS(Infrastructure as a Service)
    * 과거 기업은 서비스를 오픈하기 위해서는 자체적으로 서버를 구입하거나 렌트하여 사용하였습니다. 따라서 서버를 구축하고 네트워크를 유지하기 위해서 관리자가 일일히 관리하는 것이 관례였습니다. 
    * 하지만 IaaS가 등장하면서 인프라에 대한 상당한 부분이 소프트웨어적으로 처리하는 것이 가능해졌고, 서버에대해 직접 알지 못하더라도 필요한 만큼의 자원을 할당하고 반납하는 것이 가능해졌습니다. 
  * Infrastructure ad Code 
    * IT 시스템상에서 인프라를 관리하는 과정들을 소프트웨어가 자동으로 실행하고 관리할 수 있는 코드 형태로 기술한다는 의미입니다. 
  * DevOps
    * 개발단계와 운영단계를 밀접하게 연결하여 소프트웨어 라이브 사이클을 단기화 한다는 개념으로 기존에 서비스를 배포 및 서버를 유지하기 위해서 서버 운영자의 개념이 존재하였으나 이러한 과정을 코드로 관리하여 소프트웨어를 개발, 빌드, 배포, 운영까지의 모든 과정에 직접 개입하여 효율을 높이는 것을 의미합니다. 
* 위와 같이 3가지의 개념이 등장함에 따라 인프라 환경을 효율적이고 자동으로 관리하며, 코드상으로 관리할 수 있는 도구가 필요하게 된 것입니다. 

##### Ansible 말고 대체 솔루션은 없을까? 

* 물론 Andible 외에도 인프라를 관리하는 도구들은 다양합니다. 

|                 |       Puppet        |             Chef             |             Salt              |    Ansible     |
| :-------------: | :-----------------: | :--------------------------: | :---------------------------: | :------------: |
|     개발사      |     Puppet Lab      |           Opscode            |           SaltStack           | Ansible Works  |
|    배포시기     |       2005.08       |           2009.01            |            2011.03            |    2012.03     |
|    개발언어     |        Ruby         | Ruby(Cilent), Erlang(Server) |            Python             |     Python     |
|   코드베이스    |    Puppet Forge     |       Chef Supermarket       |         Salt-Formula          | Ansible Galaxy |
|     Web UI      |  Puppet Enterprise  |         Chef Manage          |     SaltStack Enterprise      | Ansible Tower  |
|    정의파일     | 독자 DSL, 내장 Ruby |    독자 DSL(Ruby 베이스)     | YAML, 독자 DSL(Python 베이스) |      YAML      |
| Agent 설치 유무 |        필요         |             필요             |           선택가능            |     불필요     |

* 이렇게 다양한 **배포 자동화 도구**중에서 Ansible 인기있는 이유는 Agent의 설치가 필요 없으며, 쉽게 배우고 사용할 수 있다는 장점이 있기 때문입니다. 
  * 노드 관리를 위해 Agent를 설치되어 있어야 한다면 설치관련 이슈나 신경써야 하는 부분이 증가하게 됩니다. 
  * YAML 형태의 Playbook으로 배포되기 때문에 가독성이 뛰어나다는 장점이 있습니다. 



### Ansible의 특징

* 앞서 말했듯이 Agent 설치가 불필요하기 때문에 통신을 위해서 SSH를 활용합니다. 
  * 이를 통해 필요로 하는 대상에 즉각적으로 사용이 가능하다는 장점이 있습니다. 
  * 거의 모든 운영체제에서 사용이 가능합니다. (윈도우의 경우 설치는 불가능하나 구성 관리는 가능합니다.)
* 현존하는 거의 모든 운영체제를 구성 관리할 수 있으며, 운영체제간의 응용프로그램의 세부 구성을 모두 Ansible에서 통합관리할 수 있습니다. 
* Ansible에서는 높은 보안과 신뢰성을 위한 기능들을 제공합니다. 사용자의 환경에 따라 접속정보들을 변경할 수 있으며, 주요 파일들을 암호화 하여 보호할 수 있습니다. 
* 연산을 여러 번 적용하더라도 결과가 달라지지 않는 멱등성을 보장합니다. 
* 목적에 따라 사용할 수 있도록 다양한 모듈을 제공하기 때문에 이를 통해서 원하는 모든 작업을 진행할 수 있습니다. 또한 플러그인을 통해서 연결 방법 지정, 작업 시간 표시, 에러 디버그 등의 추가적인 기능 또한 함께 제공합니다. 
* 코딩 작성 능력이 거의 필요 없는 특성을 가지고 있기 때무에 다른 구성 도구에 비해서 비약적으로 빠르게 배워서 적용할 수 있는 특징을 가지고 있습니다. 



### Ansible 기본 개념 

* Ansible에서는 크게 3가지 요소가 있습니다. 각각 인벤터리, 플레이북, 모듈로 **(1)어디서**, **(2)무엇을,** **(3) 어떻게 수행할지**를 정의합니다. 

  * 인벤토리(inventory)

    * Ansible에 의해 제어되며 Infrastructure as a Code의 대상이 될 서버의 목록을 정의하는 파일입니다. 

    * 기본적으로 '/etc/ansible/hosts'안에 정의하게 되며 정의는 다음과 같습니다. 

      ```
      mail.example.com
      
      [webservers]
      foo.example.com
      bar.example.com
      
      [dbservers]
      one.example.com
      two.example.com
      three.example.com
      ```

      ```yaml
      all:
        hosts:
          mail.example.com:
        children:
          webservers:
            hosts:
              foo.example.com:
              bar.example.com:
          dbservers:
            hosts:
              one.example.com:
              two.example.com:
              three.example.com:
      ```

    * 기본적으로 호스트의 정보를 정의할 때는 서버의 이름을 가장 앞에 쓰고 그 다음 해당 서버의 정보를 나열합니다. 

    * 인벤터리에서는 여러 서버들의 접속 정보 (SSH 접근, 포트, 리눅스 사용자)등을 정의합니다. 

    * 또한 그룹화 기능을 제공하여 각각 서버의 특징을 묶어서 정의할 수 있습니다. 이를 통해 구분하기 쉽고 추후 Ansible을 통해서 원하는 그룹만 식별하여 명령을 내릴 수 있습니다. 

  * 플레이북(Playbook)

    * 플레이북은 yaml 포맷으로 되어 있는 파일로, 인벤터리 파일에서 정의된 서버들이 무엇을 해야하는지 정의합니다.

    * Ansible을 사용한다는 것은 플레이북을 사용한다는 것을 의미하며, 플레이북 단독으로 사용되지 않고 인벤토리와 결합하여 사용합니다. 

      ````yaml
      ---
      - name: Install nginx
        hosts: host.name.ip
        become: true
      
        tasks:
        - name: Add epel-release repo
          yum:
            name: epel-release
            state: present
      
        - name: Install nginx
          yum:
            name: nginx
            state: present
      
        - name: Insert Index Page
          template:
            src: index.html
            dest: /usr/share/nginx/html/index.html
      
        - name: Start NGiNX
          service:
            name: nginx
            state: started
      ````

    * 위와 같은 플레이북을 살펴보면 이름은 'Install nginx'이며, 'host.name.ip'라는 대상에 수행한다는 것을 명시하고 있습니다. 

    * 'become: true'의 경우 리눅스 사용자가 sudo 권한을 사용하겠다는 것을 의미하며 아래에 정의된 task를 통해서 실제 플레이북에서 실행할 작업을 나열합니다. 

    * 헤당 task를 잠시 살펴보면 epel-release (Extra Packages for Enterprise Linux)를 yum를 통해서 설치해라 라는 의미를 담고 있습니다. 

      * 이때, state의 present는 Installed와 같은 의미로 해석할 수 있으며, latest 혹은 remove와 같은 형태로 지정할 수 있습니다. 

    * 플레이북에서는 Role를 정의하여 여러 여러개의 플레이북을 정의해서 사용합니다. 

  * 모듈(module)

    * 모듈은 플레이북에서 task가 어떻게 수행될지를 나타내는 요소입니다. 

    * 예를 들어 yum 명령을 통해서 패키지를 사용할 때에는 yum 모듈을 사용할 수 있으며, apt 명령을 통해 패키지를 설치할 때는 apt 모듈을 사용하여 사용합니다. 

    * 'debug' 모듈은 어떠한 내용을 출력할 지에 대한 명시가 있어야 하기 때문에 **msg:** 라는 옵션을 반드시 명시해야 합니다. 각 모듈의 사용방법은 각각 상의하나 자주 사용하는 모듈은 정해져 있으므로 문서를 참고하여 사용합니다. 

      ![](https://i.ytimg.com/vi/avwoj01RseA/maxresdefault.jpg) 

