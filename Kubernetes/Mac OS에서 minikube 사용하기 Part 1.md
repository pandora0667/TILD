# Mac OS에서 minikube 사용하기 Part 1

![](https://miro.medium.com/max/3454/1*5BUo9SV9idWAKP85w8L3Eg.png) 

필자의 블로그에서 Kubernetes (K8s)가 무엇인지, 기본 명령어와 사용방법에 대해서 간단하게 알아가는 시간을 가졌다. 재대로 사용하기 위해서는 master 노드와 slave 노드를 셋팅하여 사용하는게 일반적이지만 이번 시간에서는 minikube를 설치해서 하나의 머신에서 K8s의 사용법을 익히는 방법을 서술하고자 한다. 

>필자는 Mac OS 기반에서 진행했으며, 리눅스에서는 설치방법이 다소 다를 수 있음을 알려드립니다. 리눅스에서 사용하는 방법은 추후 설명하도록 하겠습니다. 
>
>본 설명을 따라하기 위해서는 기본적으로 Docker가 설치되어 있어야 합니다. Docker 설치방법은 본 블로그 혹은 공식 문서를 확인하시길 바랍니다. 



 이번 Part 1에서는 minikube가 무엇인지, 셋팅하는 방법과 간단하게 애플리케이션을 실행하는 방법을 알아보도록 하자. 

![](https://t1.daumcdn.net/cfile/tistory/99FD41465C9327AB38)

 기본적으로 K8s는 서로 연결되어 단일 유닛처럼 동작하는 고가용성의 컴퓨터 클러스터를 상호조정하는 역할을 담당한다. 이때 minikube의 경우 로컬 머신에 가상환경을 구축하고, 하나의 노드로 구성된 간단한 클러스터를 배포하는 가벼운 구현체라고 할 수 있다. minikube를 사용하기 위해서는 설치 및 작업이 필요한데 다음에서 알아보도록 한다. 



### kubectl과 minikube 설정하기 

1. **Mac에서 kubectl과 minikube를 셋팅하기 위해서 brew를 활용한다.** 

   ```shell
   $ brew install kubectl minikube
   ```

2. **minikube에서 VM을 사용하기 위해 virtualbox를 설치한다.** 

   ```shell
   $ brew cask install virtualbox virtualbox-extension-pack 
   ```

3. **설치가 완료되었으면 kubectl 명령어를 입력하면 다음과 같은 에러메시지가 출력된다.** 

   ```shell
   $ kubectl get services
   The connection to the server localhost:8080 was refused - did you specify the right host or port?
   ```

    이는 K8s에서 각 노드들은 마스터가 제공하는 K8s의 API를 통해서 마스터와 통신해서 애플리케이션 컨테이너를 구동하는 지시자 역할을 담당하는데 아직 API 통신을 진행하기 위한 설정이 진행되지 않았기 때문에 이러한 에러가 발생하는 것이다. 

    본 문제를 해결하기 위해서 minikube를 실행한다. 

4. **minikube 실행** 

   ```shell
   $ minikube start --vm-driver=virtualbox
   
   😄  minikube v1.6.2 on Darwin 10.15.2
   ✨  Selecting 'virtualbox' driver from user configuration (alternates: [hyperkit])
   🔥  Creating virtualbox VM (CPUs=2, Memory=2000MB, Disk=20000MB) ...
   🐳  Preparing Kubernetes v1.17.0 on Docker '19.03.5' ...
   🚜  Pulling images ...
   🚀  Launching Kubernetes ...
   ⌛  Waiting for cluster to come online ...
   🏄  Done! kubectl is now configured to use "minikube"
   ```

   ![](https://github.com/pandora0667/TILD/blob/master/screenshot/minikube/minikube_virtualbox.png?raw=true)

    최초 설정시 관련 VM 이미지를 다운로드 하는 과정을 거치고 나서 virtualbox 화면을 살펴보면 위의 그림과 같이 minikube VM이 생성된 것을 확인할 수 있다. 

5. **kubectl 명령 확인하기**

   ```shell
   $ kubectl get node
   NAME       STATUS   ROLES    AGE   VERSION
   minikube   Ready    master   11d   v1.17.0
   
   $ kubectl get pods
   No resources found in default namespace.
   
   $ kubectl get services
   NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
   kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   31s
   ```

    여기까지 문제없이 설징되었다면 기본설정은 완료된 것이다. 이제 로컬머신에서 K8s 사용할 준비가 되었다. 

### K8s 대시보드 확인하기 

 컨테이너의 경우 CNI (Container Network Interface) 구조의 네트워크로 구성되어 있기 때문에 외부에서 접속하기 위해서는 로컬머신안에서도 프록시 설정을 진행해야 한다. 하지만 minikube의 경우 간단하게 확인할 수 있는 방법으로 애플리케이션에 접근할 수 있다. 

```shell
minikube dashboard
🤔  Verifying dashboard health ...
🚀  Launching proxy ...
🤔  Verifying proxy health ...
🎉  Opening http://127.0.0.1:64081/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/ in your default browser...
```

![](https://github.com/pandora0667/TILD/blob/master/screenshot/minikube/dashboard.png?raw=true)



### minikube 가상머신 중지하기

 minikube사용을 완료하고 중지하고 싶다면 다음 명령을 입력한다. 

```shell
$ minikube stop
✋  Stopping "minikube" in virtualbox ...
🛑  "minikube" stopped.
```



### minikube 삭제하기 

 minikube를 삭제하는 방법도 그리 어렵지 않다. 단, VM을 강제로 삭제하면 안된다. 

```shell
$ minikube delete
🔥  Deleting "minikube" in virtualbox ...
💔  The "minikube" cluster has been deleted.
🔥  Successfully deleted profile "minikube"
```



 지금까지 K8s을 로컬 머신에서 사용하기 위해 많이 사용하는 minikube에 대한 설치와 간단 명령어 중지 및 삭제 방법에 대해 알아보았다. 다음에는 간단한 예제를 통해서 K8s 실습을 진행하도록 하겠다.

















