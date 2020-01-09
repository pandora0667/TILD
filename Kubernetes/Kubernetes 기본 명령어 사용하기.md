# Kubernetes 기본 명령어 사용하기

지난 시간에는 Kubernetes를 이해하기 이전에 컨테이너 기술이 무엇인지?, kubernetes의 이해, GCP를 활용해서 간단한 애플리케이션을 운영해 보았고, 온프레미스 환경에서 설치하는 스크립트를 업로드했다.

이번 시간에서는 Kubernetes를 운영하기 위해서 필요한 기본 사용법에 대해서 서술하도록 하겠다.

![](https://developers.redhat.com/blog/wp-content/uploads/2019/03/logo.png)

### Overview

kubernetes에서는 기본적으로 kubectl 명령어를 사용한다. 기본 명령어를 통해서 CLI에서 대부분의 조작이 가능하기 때문에 기본 명령어에 대한 숙지는 필수적이라고 할 수 있다.

```
$ kubectl

kubectl controls the Kubernetes cluster manager.

 Find more information at: https://kubernetes.io/docs/reference/kubectl/overview/

Basic Commands (Beginner):
  create         Create a resource from a file or from stdin.
  expose         Take a replication controller, service, deployment or pod and expose it as a new Kubernetes Service
  run            Run a particular image on the cluster
  set            Set specific features on objects

Basic Commands (Intermediate):
  explain        Documentation of resources
  get            Display one or many resources
  edit           Edit a resource on the server
  delete         Delete resources by filenames, stdin, resources and names, or by resources and label selector

Deploy Commands:
  rollout        Manage the rollout of a resource
  scale          Set a new size for a Deployment, ReplicaSet, Replication Controller, or Job
  autoscale      Auto-scale a Deployment, ReplicaSet, or ReplicationController

Cluster Management Commands:
  certificate    Modify certificate resources.
  cluster-info   Display cluster info
  top            Display Resource (CPU/Memory/Storage) usage.
  cordon         Mark node as unschedulable
  uncordon       Mark node as schedulable
  drain          Drain node in preparation for maintenance
  taint          Update the taints on one or more nodes

Troubleshooting and Debugging Commands:
  describe       Show details of a specific resource or group of resources
  logs           Print the logs for a container in a pod
  attach         Attach to a running container
  exec           Execute a command in a container
  port-forward   Forward one or more local ports to a pod
  proxy          Run a proxy to the Kubernetes API server
  cp             Copy files and directories to and from containers.
  auth           Inspect authorization

Advanced Commands:
  diff           Diff live version against would-be applied version
  apply          Apply a configuration to a resource by filename or stdin
  patch          Update field(s) of a resource using strategic merge patch
  replace        Replace a resource by filename or stdin
  wait           Experimental: Wait for a specific condition on one or many resources.
  convert        Convert config files between different API versions
  kustomize      Build a kustomization target from a directory or a remote url.

Settings Commands:
  label          Update the labels on a resource
  annotate       Update the annotations on a resource
  completion     Output shell completion code for the specified shell (bash or zsh)

Other Commands:
  api-resources  Print the supported API resources on the server
  api-versions   Print the supported API versions on the server, in the form of "group/version"
  config         Modify kubeconfig files
  plugin         Provides utilities for interacting with plugins.
  version        Print the client and server version information

Usage:
  kubectl [flags] [options]

Use "kubectl <command> --help" for more information about a given command.
Use "kubectl options" for a list of global command-line options (applies to all commands).
```

kubectl이 기본적인 명령어이자 많은 기능을 제공하기 때문에 위의 나오는 내용을 숙지하는 것이 중요하다고 할 수 있다. 필자는 kubernetes 1.15 버전을 사용했음을 알린다.

kubectl은 kubernetes 클러스터에 대해 명령을 수행하기 위한 CLI 인터페이스이다. kubectl은 $HOME/. kube에서 config 파일을 찾아, kubeconfig 환경변수 및 플래그를 설정하고, kubeconfig 파일을 지정한다. 다음 명령어를 사용하면 kubeconfig의 자세한 설명을 확인할 수 있다.

```
$ kubectl config view
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://192.168.10.30:6443
  name: kubernetes
contexts:
- context:
    cluster: kubernetes
    user: kubernetes-admin
  name: kubernetes-admin@kubernetes
current-context: kubernetes-admin@kubernetes
kind: Config
preferences: {}
users:
- name: kubernetes-admin
  user:
    client-certificate-data: REDACTED
    client-key-data: REDACTED
```

### Syntax

**본 문서는 kubernetes v1.15 버전에 대한 내용을 담고 있습니다.**

kubectl에 기본 구문은 다음과 같다.

```
$ kubectl [command] [TYPE] [NAME] [flag]
```

-   **command**
  
    -   create, get, describe, delete 등 자원에 대한 동작을 조작한다.
-   **TYPE**
  
    -   자원에 대한 유형을 지정하며, 대소 문자를 구분하지 않으나, 단수, 복수, 약식을 지정할 수 있다.
      
    -   다음을 모두 같은 출력을 실행한다.
      
        ```
        $ kubectl get pod pod1
        $ kubectl get pods pod1
        $ kubectl get po pod1
        ```
    
-   **NAME**
  
    -   리소스에 대한 이름을 지정하며, 대소문자를 구분한다. 이름을 생략하면 기본적으로 모든 자원에 세부 사항이 표시된다.
      
        ```
        $ kubectl get pods
        ```
        
    -   여러 자원에 대해 조작이 필요할 경우 유형 혹은 이름별로 각 자원을 지정하거나 하나 이상의 파일을 지정할 수 있다.
      
        -   자원이 모든 동일하여 그룹화하는 경우 **TYPE1 name1 name2 name<#>**
          
            ```
            $ kubectl get pod example-pod1 example-pod2
            ```
            
        -   자원 유형을 개별적으로 지정하는 경우 **TYPE1/name1 TYPE1/name2 TYPE2/name3 TYPE<#>/name<#>**
          
            ```
            $ kubectl get pod/example-pod1 replicationcontroller/example-rc1
            ```
        
    -   하나 이상의 파일을 자원으로 구성하는 경우 **\-f file1 -f file2 -f file<#>**
      
        ```
        $ kubectl get pod -f ./pod.yaml
        ```
        
    -   JSON보다, 사용자가 작성하고, 해석하기 쉬운 YAML 파일을 사용한다.
    
-   **flag**
  
    -   옵션으로 선택하여 사용할 수 있다. kubernetes API 서버와 포트 주소를 지정하기 위해서 **\-s** 혹은 **\--server** 플래그를 사용할 수 있다.

### Resource types

Resource types에서는 다음 명령을 통해 확인할 수 있다.

```
$ kubectl api-resources

NAME                              SHORTNAMES   APIGROUP                       NAMESPACED   KIND
bindings                                                                      true         Binding
componentstatuses                 cs                                          false        ComponentStatus
configmaps                        cm                                          true         ConfigMap
endpoints                         ep                                          true         Endpoints
events                            ev                                          true         Event
limitranges                       limits                                      true         LimitRange
namespaces                        ns                                          false        Namespace
nodes                             no                                          false        Node
persistentvolumeclaims            pvc                                         true         PersistentVolumeClaim
persistentvolumes                 pv                                          false        PersistentVolume
pods                              po                                          true         Pod
podtemplates                                                                  true         PodTemplate
replicationcontrollers            rc                                          true         ReplicationController
resourcequotas                    quota                                       true         ResourceQuota
secrets                                                                       true         Secret
serviceaccounts                   sa                                          true         ServiceAccount
services                          svc                                         true         Service
mutatingwebhookconfigurations                  admissionregistration.k8s.io   false        MutatingWebhookConfiguration
validatingwebhookconfigurations                admissionregistration.k8s.io   false        ValidatingWebhookConfiguration
customresourcedefinitions         crd,crds     apiextensions.k8s.io           false        CustomResourceDefinition
apiservices                                    apiregistration.k8s.io         false        APIService
controllerrevisions                            apps                           true         ControllerRevision
daemonsets                        ds           apps                           true         DaemonSet
deployments                       deploy       apps                           true         Deployment
replicasets                       rs           apps                           true         ReplicaSet
statefulsets                      sts          apps                           true         StatefulSet
tokenreviews                                   authentication.k8s.io          false        TokenReview
localsubjectaccessreviews                      authorization.k8s.io           true         LocalSubjectAccessReview
selfsubjectaccessreviews                       authorization.k8s.io           false        SelfSubjectAccessReview
selfsubjectrulesreviews                        authorization.k8s.io           false        SelfSubjectRulesReview
subjectaccessreviews                           authorization.k8s.io           false        SubjectAccessReview
horizontalpodautoscalers          hpa          autoscaling                    true         HorizontalPodAutoscaler
cronjobs                          cj           batch                          true         CronJob
jobs                                           batch                          true         Job
certificatesigningrequests        csr          certificates.k8s.io            false        CertificateSigningRequest
leases                                         coordination.k8s.io            true         Lease
events                            ev           events.k8s.io                  true         Event
daemonsets                        ds           extensions                     true         DaemonSet
deployments                       deploy       extensions                     true         Deployment
ingresses                         ing          extensions                     true         Ingress
networkpolicies                   netpol       extensions                     true         NetworkPolicy
podsecuritypolicies               psp          extensions                     false        PodSecurityPolicy
replicasets                       rs           extensions                     true         ReplicaSet
ingresses                         ing          networking.k8s.io              true         Ingress
networkpolicies                   netpol       networking.k8s.io              true         NetworkPolicy
runtimeclasses                                 node.k8s.io                    false        RuntimeClass
poddisruptionbudgets              pdb          policy                         true         PodDisruptionBudget
podsecuritypolicies               psp          policy                         false        PodSecurityPolicy
clusterrolebindings                            rbac.authorization.k8s.io      false        ClusterRoleBinding
clusterroles                                   rbac.authorization.k8s.io      false        ClusterRole
rolebindings                                   rbac.authorization.k8s.io      true         RoleBinding
roles                                          rbac.authorization.k8s.io      true         Role
priorityclasses                   pc           scheduling.k8s.io              false        PriorityClass
csidrivers                                     storage.k8s.io                 false        CSIDriver
csinodes                                       storage.k8s.io                 false        CSINode
storageclasses                    sc           storage.k8s.io                 false        StorageClass
volumeattachments                              storage.k8s.io                 false        VolumeAttachment
```

Resource types에 대해서 간단히 사용을 해보고 각 내용에 대해서 자세히 알아보도록 한다.

1.  node 정보 확인
  
    ```
    $ kubectl get no
    NAME           STATUS     ROLES    AGE   VERSION
    k8s-master     Ready      master   12d   v1.15.3
    k8s-worker-1   NotReady   <none>   12d   v1.15.3
    k8s-worker-2   NotReady   <none>   12d   v1.15.3
    ```
    
2.  pod 확인
  
    ```
    $ kubectl get po
    NAME                  READY   STATUS    RESTARTS   AGE
    ibm-7f4bcb9c7-nrfhk   0/1     Pending   0          83m
    ```
    
3.  서비스 확인
  
    ```
    $ kubectl get svc
    NAME         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)   AGE
    ibm          ClusterIP   10.100.243.246   <none>        80/TCP    87m
    kubernetes   ClusterIP   10.96.0.1        <none>        443/TCP   89m
    ```
    
4.  deployments 확인
  
    -   의도하는 상태를 확인하고, 비율을 조정할 때 사용
    
    ```
    $ kubectl get deployments
    NAME   READY   UP-TO-DATE   AVAILABLE   AGE
    ibm    0/1     1            0           89m
    ```
    
5.  proxy 사용
  
    -   kubernetes의 proxy는 컨테이너의 특성상 내부 IP로 할당된 pod을 외부로 노출시키기 위한 방법으로 kubetnetes API 처리를 접근하기 위해서 사용되며, reverse proxy와 같이 동작한다.
    -   기본적으로 로컬 머신에서 접근이 가능하며, 외부에서 접근하기 위해서는 별도의 작업이 필요하다.
    
    ```
    $ kubectl proxy --port=8080
    ```
    
    ```
    $ curl http://localhost:8080/api/
    
    {
      "kind": "APIVersions",
      "versions": [
        "v1"
      ],
      "serverAddressByClientCIDRs": [
        {
          "clientCIDR": "0.0.0.0/0",
          "serverAddress": "192.168.10.30:6443"
        }
      ]
    ```
    

### Examples: Common operations

kubernetes 기본 문법을 차례대로 확인했으므로 다음 명령어에 대해 익숙해지는 작업을 하도록 하겠다.

-   **kubectl apply**를 사용하여 하나 이상의 서비스를 적용하는 경우
  
    ```
    # Create a service using the definition in example-service.yaml.
    $ kubectl apply -f example-service.yaml
    
    # Create a replication controller using the definition in example-controller.yaml.
    $ kubectl apply -f example-controller.yaml
    
    # Create the objects that are defined in any .yaml, .yml, or .json file within the <directory> directory.
    $ kubectl apply -f <directory>
    ```
    
-   **kubectl get**를 사용하여 하나 이상의 리소스를 나열하는 경우
  
    ```
    # List all pods in plain-text output format.
    $ kubectl get pods
    
    # List all pods in plain-text output format and include additional information (such as node name).
    $ kubectl get pods -o wide
    
    # List the replication controller with the specified name in plain-text output format. Tip: You can shorten and replace the 'replicationcontroller' resource type with the alias 'rc'.
    $ kubectl get replicationcontroller <rc-name>
    
    # List all replication controllers and services together in plain-text output format.
    $ kubectl get rc,services
    
    # List all daemon sets, including uninitialized ones, in plain-text output format.
    $ kubectl get ds --include-uninitialized
    
    # List all pods running on node server01
    $ kubectl get pods --field-selector=spec.nodeName=server01
    ```
    
-   **kubectl describe** 초기화되지 않은 리소스를 포함하여 기본 상태를 확인하는 경우
  
    ```
    # Display the details of the node with name <node-name>.
    $ kubectl describe nodes <node-name>
    
    # Display the details of the pod with name <pod-name>.
    $ kubectl describe pods/<pod-name>
    
    # Display the details of all the pods that are managed by the replication controller named <rc-name>.
    # Remember: Any pods that are created by the replication controller get prefixed with the name of the replication controller.
    $ kubectl describe pods <rc-name>
    
    # Describe all pods, not including uninitialized ones
    $ kubectl describe pods --include-uninitialized=false
    ```
    
-   **kubectl delete** 자원 또는 서비스를 삭제하는 경우
  
    ```
    # Delete a pod using the type and name specified in the pod.yaml file.
    $ kubectl delete -f pod.yaml
    
    # Delete all the pods and services that have the label name=<label-name>.
    $ kubectl delete pods,services -l name=<label-name>
    
    # Delete all the pods and services that have the label name=<label-name>, including uninitialized ones.
    $ kubectl delete pods,services -l name=<label-name> --include-uninitialized
    
    # Delete all pods, including uninitialized ones.
    $ kubectl delete pods --all
    ```
    
-   **kubectl exec** pod 컨테이너에 명령을 실행하는 경우
  
    ```
    # Get output from running 'date' from pod <pod-name>. By default, output is from the first container.
    $ kubectl exec <pod-name> date
    
    # Get output from running 'date' in container <container-name> of pod <pod-name>.
    $ kubectl exec <pod-name> -c <container-name> date
    
    # Get an interactive TTY and run /bin/bash from pod <pod-name>. By default, output is from the first container.
    $ kubectl exec -ti <pod-name> /bin/bash
    ```
    
-   **kubectl logs** pod 컨테이너의 로그를 확인하는 경우
  
    ```
    # Return a snapshot of the logs from pod <pod-name>.
    $ kubectl logs <pod-name>
    
    # Start streaming the logs from pod <pod-name>. This is similar to the 'tail -f' Linux command.
    $ kubectl logs -f <pod-name>
    ```