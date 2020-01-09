# Docker Private Registry 

* 지금까지 우리가 만든 Docker image는 Docker Hub에 등록하여 사용했습니다. 
* 하지만 Docker hub는 반드시 public으로만 이미지를 push 할 수 있으며, 개인사용자는 하나의 이미지만 private으로 사용할 수 있습니다. 
* 이러한 불편함을 해소하기 위해 개인 pirvate rgistry를 만들어보고 간단하게 이미지를 업로드하는 방법을 알아보도록 하겠습니다. 

### Run a local registry

* 다음 명령어를 통해서 간단하게 private registry를 만들 수 있습니다. 

  ```bash
  $  docker run -d -p 5000:5000 --restart=always --name registry registry:2 
  ```

  * 본 명령어를 통해 개인 PC에 레지스트리를 생성할 수 있습니다. 
  * 로컬환경에서 진행할 때는 문제가 되지 않지만 프로덕션 환경에서는 반드시 TLS로 보호되어야 합니다. 
  * 보호되지 않는 프로덕션 환경에서의 registry는 사용이 제한됩니다. 



### Copy an image from Docker Hub to your registry 

* 이번에는 Docker Hub에서 이미지를 pull 하여 개인 이미지를 만들어보고 private registry로 push 하는 방법을 알아보겠습니다. 

* 다음 명령어를 통해서 Docker Hub에서 이미지를 pull 합니다. 

  ```bash
  $ docker pull hello-world
  $ docker pull ubuntu:16.04
  ```

* 이미지를 만들었다고 가정하고 다음과 같이 tag를 지정합니다. 

  ```bash
  $ docker tag hello-world localhost:5000/hello-world
  $ docker tag ubuntu:16.04 localhost:5000/my-ubuntu
  ```

* localhost로 이미지를 푸시합니다. 

  ```bash
  $ docker push localhost:5000/hello-world
  $ docker push localhost:5000/my-ubuntu
  ```

* localhost에 이미지가 push 되었는지 확인하기 위해 다음 명령어를 입력합니다. 

  ```bash
  $ curl -X GET http://localhost:5000/v2/_catalog
  {"repositories":["hello-world"]}
  
  $ curl -X GET http://localhost:5000/v2/hello-world/tags/list
  {"name":"hello-world","tags":["latest"]}
  ```

* localhost에 업로드한 이미지를 pull 하여 확인합니다.  

  ```bash
  $ docker image remove hello-world
  $ docker image remove ubuntu:16.04
  $ docker image remove localhost:5000/hello-world
  $ docker image remove localhost:5000/my-ubuntu
  
  $ docker pull localhost:5000/hello-world
  $ docker pull localhost:5000/my-ubuntu
  ```

  * 로컬에 저장되어있는 이미지만 삭제될 뿐 registry에 저장되어 있는 이미지는 삭제되지 않습니다. 

### Stop a local registry 

* private registry를 삭제하는 방법을 알아봅니다. 

  ```bash
  $ docker container stop registry && docker container rm -v registry
  ```