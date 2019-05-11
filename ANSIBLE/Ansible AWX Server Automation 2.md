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

  

  * 위에서 볼 수 있듯이 Ansible에서는 APT 관리를 Module의 예제를 살펴볼 수 있으며, YAML 표기법을 사용하기 때문에 크게 어렵지 않게 사용할 수 있을 것이다. 

* 지금은 시스템 업데이트/업그레이드만 진행할 것이기 때문에 다음과 같이 YAML를 작성한다. 

  ```yaml
  
  ```

  























































