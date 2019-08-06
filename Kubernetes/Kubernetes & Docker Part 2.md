# Kubernetes & Docker Part 2

**쿠버네티스 기초 다지기 3/e**

**본 문서는 kubernetes 공식 문서를 참고하여 작성되었습니다.**



![](https://subicura.com/assets/article_images/2019-05-19-kubernetes-basic-1/workload.png)

### Kubernetes (K8s) 컴포넌트 

**Master Components** 

- 클러스터의 관한 전반적인 스케줄링과 이벤트를 감지하고 반응하는 역할을 담당한다. 
- 마스터 컴포넌트는 클러스터 내 어떠한 머신에서 동작할 수 있으나, 간결성을 위해서 동일 머신 상의 모든 마스터 컴포넌트를 구동시키며, 사용자 컨테이너에서는 동작하지 않는 것이 일반적이다. 

|              종류              |                             역할                             |
| :----------------------------: | :----------------------------------------------------------: |
|         kube-apiserver         | Kubernetes API를 노출하는 컴포넌트 이며, 수평적 스케일을 위해 설계 |
|              etcd              | 분산 데이터 저장소, 클러스터 상태 저장하며 백업은 필수로 진행하여야 함 |
|         kube-scheduler         |            새로운 pod을 감지하여 구동할 노드 선택            |
|    kube-controller-manager     |           컨트롤러를 구동하는 마스터 상의 컴포넌트           |
| cloud-controller-manager (CCM) | kubernets를 다른 클라우드와 연동하여, 외부 클라우드 매니저와 상호 작용 |

1. kube-apiserver 

   ![](https://z-images.s3.amazonaws.com/f/f6/Post-ccm-arch.png)

   * 모든 K8s 컴포넌트들이 kube-apiserver를 통해 동작한다. 
   * API 객체에 대한 데이터를 검증하며 설정한다. 
   * 벡엔드 DB인 etcd에 접근하는 유일한 컴포넌트 

   

2. etcd 

   * go언어로 작성된 키-벨류 저장소 
   * 모든 클러스터 데이터를 저장한다. 

   

3. kube-scheduler  

   * 노드가 배정되지 않은 새로운 pod을 감지하고 구동될 노드를 선택함

   * 스케줄링 결정을 위해서 고려되는 사항은 다음과 같다. 

     * 리소스에 대한 개별, 총체적 요구 사항 

     * 하드웨어 / 소프트웨어 / 정책 제약 

     * 친밀 및 배격 명세 

       >친밀 규칙 
       >
       >* VM/VM 또는 VM/Host가 같은 곳에 있게 하는 규칙 
       >* VM/VM의 경우 동일한 Host에서 구동
       >
       >배격 규칙 
       >
       >* VM/VM을 서로 다른 호스트에서 구동되게 하는 규칙
       >* 같은 곳에 서비스를 하던 도중 호스트 1대가 사용 불가능에 빠져서 서비스 전체가 영향을 받지 않도록 함

     * 데이터 지역성 

     * 워크로드 간 간섭, 데드라인 포함 

     

4. kube-controller-manager  

   * 논리적으로 각 컨트롤러는 개별 프로세스로 동작하지만, 복잡성을 낮추기 위해서 모두 단일 바이너리로 컴파일 되고 단일 프로세스 상에서 실행 
   * 노드 컨트롤러 : 노드가 다운되었을 때 통지와 대응에 대한 권한 책임을 가진다. 
   * 레플리케이션 컨트롤러 : 시스템의 모든 레플리케이션 오브젝트에 대해 알맞는 수의 pod을 유지하는 책임을 가진다. 
   * 앤드포인트 컨트롤러 : 엔드포인트 오브젝트를 채운다. (서비스와 pod를 연결한다.)
   * 서비스 어카운트 & 토큰 컨트롤러 : 새로운 네임스페이스에 대한 기본 계정과 API 접근 토큰을 생성

   

5. cloud-controller-manager (CCM)  

   **CCM 도입 이전 아키텍처**

   ![](https://z-images.s3.amazonaws.com/b/be/Pre-ccm-arch.png)

   **CCM 도입 이후 아키텍처 (현재)**

   ![](https://z-images.s3.amazonaws.com/f/f6/Post-ccm-arch.png)

   * kubernetes 1.6에서 도입된 알파 기능 
   *  클라우드 관련 컨츠롤를 내장한 데몬으로 이전에는 kube-controller-manager에서 모든 것을 담당 
   * kubernetes 프로젝트에 비해 클라우드 프로바이더는 각기 다른 페이스로 개발 / 배포를 진행함으로 특정 프로바이더에  국한된 코드를 CCM으로 추상화 하여 클라우드 벤더들이 kubernetes 코드와 독립적으로 발전할 수 있도록 함



**Worker(Node) Components** 

- 동작중인 Pod를 유지하고 kubernetes 런타임을 환경을 제공하여 모든 노드 상에서 동작

|       종류        |                  역할                  |
| :---------------: | :------------------------------------: |
|      kubelet      | 클러스터 각 노드에서 실행되는 에이전트 |
|    kube-proxy     |            네트워크 프록시             |
| Container Runtime | 컨테이너를 실행을 담당하는 소프트웨어  |



1. kubelet
   * The Kubernetes Node Agent 라는 뜻을 가지고 있음 
   * 각 노드마다 실행되며 kubernetes master와 통신한다. 
   * 다양한 메커니즘으로 제공된 PodSpec 집합을 가지고 있으며, PodSpec에 컨테이너가 정상적으로 동작하도록 보장하며 kubernetes로 생성되지 않은 컨테이너는 관리하지 않는다. 
   * Pod들과 Node의 상태를 클러스터에 보고한다. 

![](https://upload.wikimedia.org/wikipedia/commons/thumb/b/be/Kubernetes.png/700px-Kubernetes.png)

2. kube-proxy  
   * 호스트의 네트워크 규칙을 관리하고 접속 포워딩을 수행하여 kubernetes 서비스를 추상화 하도록 함. 
   * TCP / UDP 스트림 포워딩을 허용하고나 TCP / UDP 포워딩을 벡엔드기능 집합에 걸쳐 RR(라운드 로빈)을 제공



**애드온**

* 클러스터 기능을 이행하는 pod와 서비스이다. 이 pod는 deployments, replication controller 등에 관리될 수 있으며 kube-system 네임페이스내에서 생성된다. 

|             종류              |                             역할                             |
| :---------------------------: | :----------------------------------------------------------: |
|              DNS              |    헤드리스가 아닌 서비스는 DNS를 가지며, 자동탐색에 유용    |
|      Web UI (Dashboard)       |            kubernetes 클러스터를 위한 웹 기반 UI             |
| Container Resource Monitoring | 중앙 데이터베이스 내의 컨테이너들에 대한 포관적인 시계열 매트릭스 기록 |
|     Cluster-level Logging     | 검색 / 열람 인터페이스와 함꼐 중앙 로그 저장소에 컨테이너 로그 저장 |



1. DNS
   * 다른 애드온은 필수적으로 필요하지 않은 것에 비해서 모든 kubernetes는 클러스터 DNS를 갖추어야 한다. 
   * 클러스터 DNS는 구성환경 내 다른 DNS 서버와 더불어, kubernetes 서비스를 위해 DNS 레코드를 제공해주는 DNS 서비스이다. 



### Kubernetes API 

* RESTful 인터페이스를 통해 kubernetes 기능을 제공하고 클러스터의 상태를 저장한다. 
* kubernetes 리소스와 레코드들은 API 객체로 저장되며, API에 대한 RESTful 호출을 통해 변경 
* 사용자는 쿠버네티스 API와 직접, 또는 kubectl과 같은 도구를 통해, 상호작용할 수 있음



#### API 변경 

* 성공적인 시스템은 새로운 시스템의 등장과 기존 시스템에 변경될 필요가 있기 때문에 API는 지속적으로 변경되어야 한다. 
* 단, 일정기간 동안은 현존하는 클라이언트와의 호환성을 깨지 않기 위해서 새로운 API 리소스와 필드가 주기적으로 추가된다.
* Alpha, Beta, Stable의 API를 지원하며, 이에 따라 안정성과 기술 지원이 다르다. 



#### API 버전 규칙

* **알파(Alpha)**

  * 버전이름에 alpha가 포함되어 있다. 
    * ex) v1alpha1 
  * 기본적으로 비활성화 되어 있으며, 이 기능을 활성화 할 경우 다양한 버그에 노출될 수 있다. 
  * 기능에 대한 기술지원이 공지없이 중단될 수 있으며, 다음 소프트웨어를 릴리즈할 때 API 호환성이 깨질 수 있다. 
  * 버그의 위험이 높고, 장기간 지원하지 않기 때문에 단기간 테스트 용도에서만 사용할 것을 권장한다. 

  

* **베타(Beta)**

  * 버전 이름에 beta가 포함된다. 
    * ex:) v2beta3
  * 코드 테스트를 통해 어느정도 검증이 되어 있으며, 기본적으로 활성화 되어 있다. 
  * 구체적인 내용이 변경될 수 있으나, 전반적인 기능에 대한 기술 지원이 중단되지 않는다. 
  * 다음 릴리즈로 이전할 때 가이드가 제공되지만, 전체적인 서비스의 수정으로 인해서 다운타임이 발생할 수 있다. 
  * 다음 릴리즈에 호환되지 않을 가능성이 있기 떄문에 사업적으로 중요하지 않은 용도로만 사용할 것을 권장하며, 복수의 클러스터를 가지고 있어서 독립적으로 업그레이드를 할 수 있다면 안전하다. 
  * 베타사용시 다양한 피드백을 요구하길을 강력하게 요구한다. 베타버전이 끝나면 실질적으로 변경이 어렵다. 

  

* **안정화(stable)**
  * 버전 이름이 vX이고 X는 정수이다. 
  * 안정화 버전의 기능은 이후 여러 버전에 걸쳐서 소프트웨어 릴리즈에 포함된다. 



### Kubernetes Object 이해하기 

* kubernetes 시스템에서 영속성을 가지는 개체로서, 클러스터의 상태를 나타내기 위해서 사용된다. 
  * 어떤 컨테이너와 노드에 어떤 애플리케이션이 동작중인가? 
  * 애플리케이션이 사용할 수 있는 자원은 무엇인가? 
  * 애플리케이션의 재구동, 업그레이드, Fault Tolerance 등에 대해 어떻게 동작해야 하는가? 
* 오브젝트를 만들게 되면 kubernetes는 오브젝트의 생성을 보장하여, 개발자가 의도한 상태가 유지될 수 있도록 한다. 
* 생성이나 수정이나 삭제를 하는 모든 경우에 kubernetes API가 사용되며, **kubctl** 명령어를 통해 CLI 상에서 API를 호출할 수 있도록 한다. 



#### 오브젝트 Spec과 Status

* 모든 kubernetes 오브젝트는 구성을 결정해주는 2가지의 필드를 포함하는데 이것이 바로 Spec과 Status이다. 

* Spec은 모든 특성과 상태를 정의하는 것을 의미하며, Status는 kubernetes 시스템에 의해 제공되고, 업데이트 되어 의도한 상태가 유지될 수 있도록 한다. 

  

![](https://i.imgur.com/KvnTlWj.png)



- kubernetes에서 오브젝트를 생성할 때 오브젝트에 대한 기본적인 정보와 더불어 상태를 기술한 Spec을 기술해야 한다. 

```yml
apiVersion: apps/v1 # apps/v1beta2를 사용하는 1.9.0보다 더 이전의 버전용
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2 # 템플릿에 매칭되는 파드 2개를 구동하는 디플로이먼트임
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80

```

* 위의 예시는 yml 파일을 이용하여 kubernetes 오브젝트를 생성하는 방법을 나타내고 있다. 

* 이렇게 만들어진 yml 파일은 다음과 같이 실행할 수 있다. 

  ```
  kubectl apply -f https://k8s.io/examples/application/deployment.yaml --record
  ```

* 출력 결과는 다음과 같다. 

  ```
  deployment.apps/nginx-deployment created
  ```

* 위와 같이 yml 파일안에는 다음과 같은 필드가 반드시 설정되어 있어야 한다. 

  * apiVersion : 오브젝트를 생성하기 위해 사용하고 있는 kubernetes API 버전 
  * kind : 어떤 종류의 오브젝트를 생성하고자 하는가? 
  * metadata : 이름,  문자열, UID, 옵션으로 네임스페이스를 포함하여 오브젝트를 유일하게 구분지어줄 데이터 



### kubernetes 아키텍처 

![](https://mapr.com/blog/kubernetes-kafka-event-sourcing-architecture-patterns-and-use-case-examples/assets/clusters.png)



* 노드는 kubernetes의 워커 머신으로 이전에는 미니언이라고 불렸다. 
* 노드는 클러스터에 따라 VM 또는 물리머신으로 구성될 수 있으며, 각 노드는 pod를 동작시키기 위해 필요한 서비스를 포함하여 마스터 컴포넌트에 의해 관리된다. 

#### Node Status 

* 노드의 상태는 다음과 같은 정보를 포함한다. 

  * 주소 : 이 필드의 사용법은 클라우드 제공업자 또는 베어메탈 구성에 따라 다양하다.
    * HostName : 노드의 커널의 알려진 호스트 명으로  `--hostname-override` 파라미터를 통해 치환될 수 있다. 
    * ExternalP : 일반적으로 노드의 IP 주소는 외부로 라우트가 가능하다. 
    * InternalIP : 노드의 IP는 클러스터 안에서만 라우트가 가능하다. 

  

  * 컨디션 

    * conditions 필드는 모든 running 노드의 상태를 기술한다. 

      |     Node Condition     |                             설명                             |
      | :--------------------: | :----------------------------------------------------------: |
      |     **OufOfDisk**      | 노드 상에서 pod를 추가하기 위한 용량이 부족한 경우 True, 반대의 경우 False |
      |       **Ready**        | 노드가 상태 양호하며 파드를 수용할 준비가 되어 있는 경우 True, 노드의 상태가 불량하여 파드를 수용하지 못할 경우 False, 그리고 노드 컨트롤러가 마지막 node-monitor-grace-period (기본값 40 기간 동안 노드로부터 응답을 받지 못한 경우) Unknown |
      |   **MemoryPressure**   |    노드 메모리가 넉넉치 않은 경우 True, 반대의 경우 False    |
      |    **PIDPressure**     | 노드 상에 많은 프로세스들이 존재하는 경우 True, 반대의 경우 False |
      |    **DiskPressure**    |    디스크 용량이 넉넉치 않은 경우 True, 반대의 경우 False    |
      | **NetworkUnavailable** | 노드에 대해 네트워크가 올바르게 구성되지 않은 경우 True, 반대의 경우 False |

      * 노드 컨디션은 JSON 오브젝트로 표기되며 상태가 양호하면 다음과 같은 응답을 보낸다. 

        ```json
        "conditions": [
          {
            "type": "Ready",
            "status": "True",
            "reason": "KubeletReady",
            "message": "kubelet is posting ready status",
            "lastHeartbeatTime": "2019-06-05T18:38:35Z",
            "lastTransitionTime": "2019-06-05T11:41:27Z"
          }
        ]
        ```

        

  * 용량과 할당가능 

    * 노드상에서 사용가능한 CPU, 메모리, 노드 상으로 스케줄링 될 수 있는 최대 pod 수가 있다.

    * 용량 블록의 필드의 경우 리소스의 총량을 나타내며, 할당 가능한 블록은 일반 pod에서 사용할 수 있는 노드의 리소스 양을 나타낸다. 

      

  * 정보 

    * 커널 버전, kubernetes 버전, Docker 버전, OS 이름과 같은 노드의 일반적인 정보가 kublet에 의해 수집된다. 



#### 관리 

* pod와 서비스와 달리 노드는 kubernetes에 의해 생성되지 않기 때문에 클라우드 서비스를 통해 외부로 부터 생성되거나 물리적인 서버 혹은 VM안에서 존재한다. 

* kubernetes가 노드를 생성할 때, 노드를 나타내는 오브젝트를 생성하며, 생성 이후 kubernetes 노드의 유효성 여부를 검사한다. 

  ```json
  {
    "kind": "Node",
    "apiVersion": "v1",
    "metadata": {
      "name": "10.240.79.157",
      "labels": {
        "name": "my-first-k8s-node"
      }
    }
  }
  ```

  * 위와 같은 예제로 노드를 생성한다면, kubernetes 내부적으로 노드 오브젝트를 생성하고 metadata.name 필드를 근거로 상태를 체크하여 노드의 유효성을 검사한다. 
  * 이후 노드가 유효하다고 판단되면, pod을 동작시키게 되며, 유효하지 않다면 해당 노드가 유효할 때 까지 어떠한 클러스터 활동에 대해서 무시한다. 

  

  **아래의 그림은 kubernetes의 마스터 노드와 워커 노드, pod의 관계를 나타내고 있다.**

  ![](http://thenewstack.io/wp-content/uploads/2016/11/Chart_04_Kubernetes-Node.png)

  





















