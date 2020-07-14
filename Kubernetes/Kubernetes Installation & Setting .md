# Kubernetes Installation & Setting 

[![Version](https://img.shields.io/badge/version-2019.08.28-red.svg)![License](https://img.shields.io/github/license/mashape/apistatus.svg)](./LICENSE) ![GitHub followers](https://img.shields.io/github/followers/pandora0667?style=social)



## Description

* **본 프로젝트는 Kubernetes 설치 및 사용을 위한 기본 사용을 위한 프로젝트입니다.**

* **설치파일은 쉘 스크립트로 제공되며, 예제는 "쿠버네티스 기초다지기 3/e"을 참고하였습니다.**

* **버전 및 상황에 따라서 내용은 변경될 수 있습니다.**

   

#### 기본설정 

* 본 프로젝트의 Kubernetes는 로컬 머신 혹은 VM 환경에서 동작하도록 작성되었습니다.

  * OS : Ubuntu 18.04 LTS 

  * Docker : 19.03.1

    * Kubernetes는 Docker 18.09를 권장하나 최신버전에서도 동작하기 때문에 본 설정파일에서는 최신 Docker 버전을 사용하였습니다. 

    * Docker version is not on the list of validated versions: 19.03.1. Latest validated version: 18.09

    * 경고메시지를 보고 싶지 않으시다면 쉴 스크립트에서 Docker 설치환경을 다음과 같이 변경합니다. 

      ```bash
      $ sudo apt install docker.io -y
      ```

      

## 설치

* **Kubernetes 설치를 위해 다음 환경을 구축하세요**
  * master node 
  * worker node 
* **본 환경에서는 worker node를 2개로 설정하였습니다. **



### Master Node Installation 

```bash
$ git clone https://git.wisoft.io/seongwon/kubernetes-Installation.git
$ cd kubernetes-installation
$ chmod +x kubernetes-master.sh
```

* 설치 완료되면 홈 디렉토리에 'join-command.txt '파일이 생성되며, 여기에 Worker Node와 연결하기 위한 token 정보가 포함됩니다. 

  ```bash
  $ sudo kubectl get no
  NAME           STATUS     ROLES    AGE   VERSION
  k8s-master     Ready      master   73m   v1.15.3
  ```

  * Master Node에서 노드 정보가 정상적으로 실행하고 있는지 확인합니다. 



### Worker Node Installation

```shell
$ git clone https://git.wisoft.io/seongwon/kubernetes-Installation.git
$ cd kubernetes-installation
$ chmod +x kubernetes-worker.sh
```

- 설치가 완료되면 다음과 같은 명령문이 표기됩니다. 

  ```shell
  $ Please complete the installation by entering the kubernets join command. Thank you.
  ```



## Kubernetes Cluster Setting 

* Master Node 설치시 화면에 출력된 Token을 각 Worker Node에 입력합니다. 형식은 다음과 같습니다. 

  ```bash
  $ kubeadm join $(HOST-IP):6443 --token mrp99b.di58v221201oslj6 \
      --discovery-token-ca-cert-hash sha256:637acd3afecac2c3f887bf602783e0f4344fb51a38544d07972ccd58ad0f6faf
  ```

* Master Node에서 추가된 노드를 확인합니다. 

  ```bash
  $ sudo kubectl get no
  NAME           STATUS   ROLES    AGE   VERSION
  k8s-master     Ready    master   87m   v1.15.3
  k8s-worker-1   Ready    <none>   14m   v1.15.3
  k8s-worker-2   Ready    <none>   14m   v1.15.3
  ```



## Error 

* 에러가 발생할 경우 다음과 같이 해결합니다. 

  ```bash
  $ sudo kubectl get no
  
  The connection to the server $(HOST-IP):6443 was refused - did you specify the right host or port?
  ```

  ```bash
  $ sudo -i 
  
  $ sudo swapoff -a
  
  $ exit
  
  $ strace -eopenat kubectl version
  openat(AT_FDCWD, "/etc/passwd", O_RDONLY|O_CLOEXEC) = 3
  Client Version: version.Info{Major:"1", Minor:"15", GitVersion:"v1.15.3", GitCommit:"2d3c76f9091b6bec110a5e63777c332469e0cba2", GitTreeState:"clean", BuildDate:"2019-08-19T11:13:54Z", GoVersion:"go1.12.9", Compiler:"gc", Platform:"linux/amd64"}
  
  $ sudo systemctl status kubelet
  Active: active (running) since Wed 2019-08-28 20:06:18 KST; 2min 11s ago
  
  $ kubectl get componentstatus
  NAME                 STATUS    MESSAGE             ERROR
  scheduler            Healthy   ok
  controller-manager   Healthy   ok
  etcd-0               Healthy   {"health":"true"}
  
  $ kubectl get no
  NAME           STATUS   ROLES    AGE    VERSION
  k8s-master     Ready    master   125m   v1.15.3
  k8s-worker-1   Ready    <none>   52m    v1.15.3
  k8s-worker-2   Ready    <none>   52m    v1.15.3
  ```

  

  

  

## Developer Profile

 **seongwon**

- Blog / Homepage 
  -  https://judo0179.tistory.com/
  - https://dbqudwo333.ml
- Github 
  -  https://github.com/pandora0667
- Instagram 
  - https://www.instagram.com/seongwon0667/



## License

This project is licensed under the MIT License - see the [LICENSE.md](https://git.wisoft.io/seongwon/kubernetes-installation/blob/master/LICENSE) file for details

