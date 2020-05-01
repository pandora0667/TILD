![img](https://podman.io/images/podman.svg)

### Podman이 무엇인가?

 container의 대표주자 Docker가 필수적으로 자리 잡게 되면서 이제 서비스를 배포하는 시간이 획기적으로 단축되었으며, 작업의 효율성이 증대되었습니다. Docker는 특정 서버의 의존성이나, 운영시 발생할 수 있는 스노 플레이크 서버의 문제점을 해결할 수 있기 때문에, 배 포환겨에 혁신을 이루어냈습니다.

이를 적극적으로 활용하기 위한 대규모 클러스터링 시스템인 Docker Swarm, Kubernetes가 성공적으로 안착하였고 현재까지도 가장 인기 있는 컨테이너 플랫폼으로서 성장하고 있습니다.

하지만 Docker 데몬이 죽게되면 모든 Docker 위에서 동작하고 있는 서비스들이 중단돼 문제가 발생하게 됩니다.

![img](https://www.fujitsu.com/fts/Images/redhat-580x224_tcm21-32995.png)

 podman은 이런 문제점을 해결하기 위해서 나온 컨테이너 플랫폼으로, Centos8에서 권장하고 있습니다. Docker와 다르게 리눅스 커널에 직접 프로세스 형식으로 컨테이너를 실행함으로써, Docker 데몬과 상관없이 독립적인 서비스 운영이 가능하며, Docker와 거의 동일한 명령어를 제공하면서, 기존 Docker 사용자도 쉽게 접근할 수 있습니다.

![img](https://dzone.com/storage/temp/10561741-vm-container-figure1.jpg)

 위의 그림에서 확인할 수 있듯이 기존 VM과 다르게 Docker는 Container Engine 위에 프로세스 형태로 동작하여, 기존의 VM 환경과 다르게 빠르게 deploy할 수 있으며, 서비스 실행 속도가 빠르다는 장점이 있습니다.

![img](https://user-images.githubusercontent.com/12066892/71462973-5171b300-27f8-11ea-814a-2a8a9733efc6.png)

 Docker의 경우 이미지 방식을 사용하기 때문에 레지스트리에서 해당 이미지를 다운받아 실행할 수 있으며, 직접 수정한 부분에 대해서는 수정한 부분에 대한 이미지만 새로 빌드하여 업로드하기 때문에 용량과 확장성에 이점을 가지고 있었습니다.

 하지만 podman의 동작방식은 Docker 방식과 차별화된 방식을 채택하고 있습니다.

![img](https://developers.redhat.com/blog/wp-content/uploads/2019/02/fig2.png)

 위의 그림과 같이 Docker와 다르게 podman의 경우 커널에 직접 이미지가 올라가고 위에 컨테이너가 동작하는 구조이기 때문에 컨테이너의 독립적 실행이 가능하며, 시스템 데몬 등록을 통해서 컨테이너별 실행 및 중단이 가능합니다. 이를 통해 개별적인 컨테이너 운영이 가능하며, Container Engine에 영향을 받지 않습니다.

 Redhat 사용자는 패키지 관리자를 통해서 바로 설치가 가능하며, ubuntu 사용자도 간단하게 설치하여 사용할 수 있습니다. 또한 Docker hub에서 직접 이미지를 가져와서 실행이 가능하며, Dockerfile 작성을 통해서 직접 실행할 수 있습니다. 아직까지는 초기버전이기 때문에 실사용 측면에서는 다소 고려해야 할 부분이 있으며, Docker-Compose를 사용할 수 없다는 단점을 가지고 있습니다.

 하지만 podman-compose를 현재 개발하고 있으며, kubernetes 활용도 가능하기 때문에 앞으로의 가능성과 Docker가 어떻게 변화할지 지켜봐야 할 것입니다.

 다음 포스팅에서는 podman 설치방법과 Registry 등록방법을 알아보도록 하겠습니다.