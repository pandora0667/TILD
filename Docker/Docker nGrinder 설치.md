# Docker nGrinder 설치

![](https://0701.static.prezi.com/preview/v2/svnkzkq3pk7srivoyl5elmoojt6jc3sachvcdoaizecfr3dnitcq_3_0.png)

nGrinder는 네이버에서 서버 성능 측정을 목적으로 개발한 오픈소스 프로젝트이다. 실제 서비스를 시작하기 전에 서비스의 부하 테스트를 위함으로, 실제 서비스 이전에 어느 정도의 부하를 견딜 수 있는지에 대한 목적을 두고 있다. 

nGrinder는 Controller와 Agent로 구분되어 있는데, Controller는 웹UI와 에이전트에게 명령을 전달하고 이에 대한 결과를 수집하는 역할을 담당하고, Agent는 Controller부터 수신된 정보를 해당 서버에 가상 유저를 생성하여 부하를 발생시킨다. 

![](https://t1.daumcdn.net/cfile/tistory/2302A7415850941218)


물리 머신에서 설치하기 위해서는 자바를 비롯한 각종 의존성 파일을 설치해야 하기 때문에 Docker를 사용하여 설치하도록 한다. 



#### nGrinder Controller

```bash
$ docker run -d -v ~/ngrinder-controller:/opt/ngrinder-controller -p 80:80 -p 16001:16001 -p 12000-12009:12000-12009 ngrinder/controller:3.4
```

설치가 완료되고, 웹 브라우저에 접속을 시도하면 nGrinder 로그인 화면이 나오게 되며 기본 사용자는 admin이기 때문에 반드시 비밀번호를 변경한다. 



#### nGrinder Agent 

```bash
$ docker run -v ~/ngrinder-agent:/opt/ngrinder-agent -d ngrinder/agent:3.4 controller_ip:controller_web_port
```

 Agent의 경우 Controller가 설치된 머신이 아닌 독립적 운영을 위해 다른 Docker 머신에서 설치할 것을 권장한다. 

모든 설치가 완료되면 에이전트 관리 항목에서 등록된 리스트가 나타나는데 필자는 2개의 Agent를 등록했다. 

![](/Users/seongwon/Documents/TILD/screenshot/ngrinder/1.png)

#### 테스트 진행하기 

![](/Users/seongwon/Documents/TILD/screenshot/ngrinder/2.png)

![](/Users/seongwon/Documents/TILD/screenshot/ngrinder/3.png)

Quick Start 항목에서 프로토콜을 포함한 주소를 적어주고(HTTPS, 도메인 가능) 실행할 스크립트를 지정하고 Strart Test 버튼을 클릭한다. 

![](/Users/seongwon/Documents/TILD/screenshot/ngrinder/4.png)

위 그림과 같이 테스트에 대한 항목을 적을 수 있는데, Agent 갯수와 가상 유저의 수, 시간 혹은 카운트를 지정한다. nGrinder의 경우 바로 테스트를 시행할 수 없으며, 설정된 스크립트를 저장 후 시작되게 되며, 수집된 정보는 다운로드하거나, 언제든지 웹에서 확인할 수 있다.  

필자는 테스트 웹서버에 Agent 2개로 2000명의 가상유저를 생성하여 5분 동안 테스트를 진행하겠다. 

![](/Users/seongwon/Documents/TILD/screenshot/ngrinder/5.png)

테스트가 진행하는 동안의 TPS 그래프가 실시간으로 표시되어, 서버가 얼마나 요청을 잘 처리하고 있는지 확인할 수 있다. 

만약 실제 서비스를 준비하는 중 서버가 얼마나 많은 사용자에 대응할 수 있는지, 검증이 필요하다면 nGrinder를 잘 활용할 수 있기를 바란다. 