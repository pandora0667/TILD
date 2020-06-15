# ngrinder docker-compose 설정 및 agnet controller 연결하기

 연구실에는 docker-swarm을 이용한 7개의 VM을 사용 중에 있습니다. 여기에 ngrinder를 통해 개발 중인 프로젝트의 성능 측정을 목적으로 사용하고 있습니다. ngrinder의 자세한 설명을 보고 싶으시다면 [여기 링크를](https://judo0179.tistory.com/63) 참조하세요.

 ngrinder를 사용하기 위해서는 agent가 controller에 연결되어 있어야 하는데 이 부분이 정상적으로 연결되지 않는 버그가 심심치 않게 발생하고 있습니다. 필자는 다음과 같은 방법으로 문제를 해결했습니다.



````yaml
version: '3.5'
services:
  controller:
    image: ngrinder/controller
    restart: always
    ports: 
      - "1000:80"
      - "16001:16001"
      - "12000-12009:12000-12009"
    volumes:
      - /mnt/swarm/ngrinder-controller:/opt/ngrinder-controller

  agent:
    image: ngrinder/agent
    restart: always
    links:    
      - controller

````



처음 설정 파일을 확인해보면 agent도 볼륨을 사용하도록 설정되어 있는데, 이때 agnet.conf 파일에 등록된 controller의 주소와 연결 포트가 다르게 설정되어 있는 상황이 발생하게 됩니다.

 일단 기본적으로 agnet의 경우 저장되는 데이터는 서비스 운영에 있어서 문제를 발생하지 않기 때문에 해당 볼륨을 삭제하고 클러스터링에서 재시작하게 되면 controller 연결이 문제없이 진행됩니다.

