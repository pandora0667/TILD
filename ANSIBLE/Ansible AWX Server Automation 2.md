# Ansible AWX를 활용한 서버 자동화 2 

* 지난 시간 우리는 Ansible에 대해서 알아보았고 Ansible 프로젝트 관리를 위한 오픈소스 커뮤니티 프로젝트인 AWX에 대해서도 살펴보았다.
* 지난 문서에 대해서 문제없이 잘 설치가 되었다면 기본적으로 AWX를 사용할 준비가 되었다고 할 수 있다.  



![](https://github.com/pandora0667/TILD/blob/master/screenshot/Ansible%20Server%20Automation-2/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202019-05-10%20%EC%98%A4%EC%A0%84%2011.00.35.png?raw=true)

* 위의 사진은 본 연구실에 설치된 AWX의 기본 화면이다. 기본 Dashboard 화면에서는 Ansible 프로젝트의 상태와 최근에 사용한 Templates의 화면이 출력되며, 실행된 Templates의 성공/실패 여부를 확인할 수 있다. 



### 기본 화면구성 

* 일단 본격적으로 기본 프로젝트 설정 방법을 알아보기 이전에 AWX의 기본 구조를 살펴보도록 하겠다. 
  * 자세한 문서는 Ansible Tower 문서를 살펴보아도 좋다. 필자도 본 문서를 가지고 학습하였다. 
    * https://docs.ansible.com/ansible-tower/

|       목록        |                             설명                             |
| :---------------: | :----------------------------------------------------------: |
|   **DASHBOARD**   |                 최근 Ansible 실행상태를 확인                 |
|     **JOBS**      |               최근에 실행된 프로젝트의 리스트                |
|   **SCHEDULES**   |        등록된 Ansible 프로젝트에 정기적인 실행 스케줄        |
|   **PROJECTS**    | Ansible Playbook의 모음을 단위로 묶은 것으로 SCM으로 등록 가능 |
|  **CREDENTIALS**  | 관리할 서버의 인증정보 설정 (로컬 머신, 클라우드, 오픈스택, vCenter 등 지원) |
|  **INVENTORIES**  |           Ansible 프로젝트를 실행할 호스트의 그룹            |
|   **TEMPLATES**   | Inventory 정보와 Playbook 정보를 조합하여 실행할 Job를 구성  |
| **ORGANIZATIONS** | Users, Teams, Invemtory, Project, Job Templates, Admins 상위 구조 |
|  **APPLICATION**  |               Token 기반 외부 Application 설정               |



### Ubuntu 시스템 업데이트/업그레이드 자동화 하기

* 가장 기본적으로 해볼 것은 연구실에 설치된 Ubuntu 18.04 LTS 서버의 시스템 업데이트/업그레이드를 진행해 볼 것이다. 
  * 기본적이면서 서버를 운영함에 있어 필수적인 요소이기 때문에 실습을 진행해 보고 감을 익혀보도록 하자. 

* 먼저 Ansible Playbook를 작성하기 위해서 Ansible 공식 문서에서 apt module를 검색한다. 

  ![](https://github.com/pandora0667/TILD/blob/master/screenshot/Ansible%20Server%20Automation-2/ansible_apt_module.png?raw=true)
  ![](https://github.com/pandora0667/TILD/blob/master/screenshot/Ansible%20Server%20Automation-2/ansible_apt_example.png?raw=true)

  * Ansible Documents를 살펴보면 Ansible을 활용하기 위한 다양한 모듈을 기본적으로 제공하고 있는데 검색한 결과와 같이 APT에 대한 모듈에 대한 설명도 나와 있는 것을 확인할 수 있다.

* 다양한 활용에 대한 예시가 나와있으나 본 시간에서는 시스템 업데이트/업그레이드에 대한 예제만 작성할 것이기 떄문에 다음과 같이 YMAL 파일을 작성한다. 

  ```yaml
  ---
  - name: Ubuntu server update and upgrade 
    gather_facts: no 
    hosts: all
  
    tasks:
      - name: Update APT package manager repositories cache
        apt: 
          update_cache: yes
  
      - name: Upgrade installed packages 
        apt:
          upgrade: dist
  
      - name: Ubuntu update and upgrade after autoremvoe
        apt:
           autoremove: yes
  ```

  * 아주 간단하게 YAML 파일을 작성하였다. 적용할 host는 all이라고 지정하였으나 이는 AWX에서 범위를 선택할 수 있기 때문에 크게 상관하지 않아도 된다. 

* 이렇게 설정한 YAML 파일은 GItLab 혹은 Github에 업로드 하여 AWX에 등록한다. 

![](https://github.com/pandora0667/TILD/blob/master/screenshot/Ansible%20Server%20Automation-2/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202019-05-10%20%EC%98%A4%EC%A0%84%2011.01.24.png?raw=true)

![](https://github.com/pandora0667/TILD/blob/master/screenshot/Ansible%20Server%20Automation-2/ansible_project.png?raw=true)

* 위의 그림과 같이 이 프로젝트에 대한 이름과 기관을 지정하고 본인의 Git주소를 지정하여 저장을 클릭한다. 
* 이때 SCM 타입은 반드시 Git으로 지정해야 하며, Git 프로젝트는 Public으로 되어 있어야 AWX에서 접근하여 Playbook 파일을 선택할 수 있다. 
* 기본적으로는 Playbook 파일이 변경될 때마다 Git에 Push를 진행하고 AWX에서 최신 SCM 파일을 읽어드릴 수 있도록 새로고침 버튼을 클릭해야 하나, 스케줄을 지정하면 매분 혹은 매 시간마다 새로운 버전이 있는지 확인아여 갱신하도록 설정하루 수도 있다. 





















































