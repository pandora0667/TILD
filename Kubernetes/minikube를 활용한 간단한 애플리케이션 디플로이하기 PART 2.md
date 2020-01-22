# minikube를 활용한 간단한 애플리케이션 서비스하기 PART 2

> 본 문서는 **완벽한 IT 인프라 구축의 자동화를 위한 Kubernetes** 책의 내용을 일부 발췌하여 작성한 글입니다.
>
> 글의 이해도를 높이기 위해서는 본 책을 구매해서 내용을 학습하는 것을 추천합니다. 책의 링크와 소스코드는 아래에 공유해드리도록 하겠습니다.

### 개발환경

-   OS : MAC OS
    
-   필수 프로그램 : Docker, minikube
    
-   IDE : Visual Studio Code
    
-   VCS : GIt
    

### 소스코드

필자의 글에서 자세한 내용은 전부 설명하겠지만 소스코드가 필요한 경우에는 소스코드를 다운로드하여서 실습을 진행하길 바란다.

-   책 소스코드 : [https://github.com/ToruMakabe/Understanding-K8s](https://github.com/ToruMakabe/Understanding-K8s)
-   필자 소스코드 : [https://git.wisoft.io/seongwon/kubernetes-study.git](https://git.wisoft.io/seongwon/kubernetes-study.git)

PART 1의 글을 읽지 않은 경우에는 [여기](https://judo0179.tistory.com/70)를 클릭하여 기본 셋팅을 완료하여 사용하시길 바랍니다.

### minikube 클러스터를 활용한 hello-world

먼저 간단하게 hello-world 애플리케이션을 K8s 클러스터에 배포하자.

```
$ minikube start
😄  minikube v1.6.2 on Darwin 10.15.2
✨  Selecting 'virtualbox' driver from existing profile (alternates: [hyperkit])
💡  Tip: Use 'minikube start -p <name>' to create a new cluster, or 'minikube delete' to delete this one.
🏃  Using the running virtualbox "minikube" VM ...
⌛  Waiting for the host to be provisioned ...
🐳  Preparing Kubernetes v1.17.0 on Docker '19.03.5' ...
🚀  Launching Kubernetes ...
🏄  Done! kubectl is now configured to use "minikube"
```

#### 매니페스트 파일

K8s에서 클러스터에 애플리케이션을 어떻게 디플로이 하고, 클라이언트에게 액세스를 어떻게 처리해야 할지에 대한 파일을 매니페스트 파일(manifest file)이라고 한다. 이 매니페스트 파일 안에는 자원 할당과 네트워크 설정 정보 등을 담고 있으며, K8s를 구동하기 위한 핵심 파일이라고 할 수 있다. 보통은 YAML 파일을 통해 작성하게 되며 매니페스트 파일은 다음 2가지로 나눌 수 있다고 생각하면 된다.

1.  디플로이먼트 (deployments)
    -   K8s에서 pod 관리와 네트워킹 목적으로 함께 묶여 있는 하나 이상의 컨테이너 그룹으로, pod의 헬스체크와 생성 및 스케일링을 관리하는 역할을 담당한다.
2.  서비스 (service)
    -   K8s의 pod는 컨테이너 내부의 네트워크 주소를 할당받기 때문에 외부에 접속하기 위해서는 해당 pod의 서비스를 외부 IP로 노출시켜야 한다. 이러한 정보를 담고 있는 것이 서비스이다.

**K8s에서 간단하게 hello-world 파일을 서비스 하기 위해 다음 명령어로 테스트해보자**

-   K8s 공식으로 제공하는 이미지 파일을 통해 hello-node라는 애플리케이션을 디플로이 한다.
    
    ```
    $ kubectl create deployment hello-node --image=gcr.io/hello-minikube-zero-install/hello-node
    ```
    
    ```
    $ kubectl get deployments
    
    NAME         READY   UP-TO-DATE   AVAILABLE   AGE
    hello-node   1/1     1            1           15m
    ```
    
    ```
    $ kubectl get pods
    
    NAME                          READY   STATUS    RESTARTS   AGE
    hello-node-7676b5fb8d-9x5zk   1/1     Running   0          16m
    ```
    
-   K8s 클러스터 안에 존재하는 pod에 내부 IP주소로 할당되어 있기 때문에 현재로는 외부에서 접속할 수 있는 방법이 없다. 따라서 외부에서 접속하기 위한 서비스를 설정해야 한다.
    
    ```
    $ kubectl expose deployment hello-node --type=LoadBalancer --port=8080
    ```
    
    ```
    $ kubectl get services
    
    NAME         TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
    hello-node   LoadBalancer   10.96.165.108   <pending>     8080:32484/TCP   23m
    kubernetes   ClusterIP      10.96.0.1       <none>        443/TCP          20h
    ```
    
    명령어를 확인해보면 CLUSTER-IP에 해당 서비스 IP 주소가 할당되어 있는 것을 확인할 수 있고 EXTERNAL-IP 항목에 외부 IP가 할당된다. pending 항목으로 뜨는 이유는 현재 외부 IP 주소로 할당받지 못했기 때문에 발생하는 문제인데, 실제 K8s 환경에서는 잠시 후 외부 IP 할당이 완료되지만 minikube환경에서는 추가적인 명령을 통해서 외부 IP로 바인딩한다.
    
    ```
    $ minikube service hello-node
    
    |-----------|------------|-------------|-----------------------------|
    | NAMESPACE |    NAME    | TARGET PORT |             URL             |
    |-----------|------------|-------------|-----------------------------|
    | default   | hello-node |             | http://192.168.99.102:32484 |
    |-----------|------------|-------------|-----------------------------|
    🎉  Opening service default/hello-node in default browser...
    ```
    
    위의 명령어를 입력하면 디폴트로 설정된 웹브라우저가 나타나면서 Hello World! 가 표기된다.
    
    ![](https://github.com/pandora0667/TILD/blob/master/screenshot/minikube/hello-wolrd.png?raw=true)
    
    이렇게 간단한 hello-world 애플리케이션을 디플로 이하고 서비스를 진행했다. 이제 본 애플리케이션을 삭제하고 실제 예제를 동작해보도록 하겠다.
    
    ```
    $ kubectl delete deployments hello-node
    deployment.apps "hello-node" deleted
    
    $ kubectl delete service hello-node
    service "hello-node" deleted
    ```
    

### Flask를 활용한 웹 애플리케이션

이제 Flask를 활용한 애플리케이션을 작성하고 Dockerfile을 작성한 뒤, docker 이미지를 만들고 해당 이미지를 가지고 K8s 환경에서 실행할 것이다. 먼저 다음과 같은 디렉터리 구조로 생성하고 각각 파일을 작성해 보자

```
.
└── photo-view
    ├── Dockerfile
    ├── app.py
    ├── requirements.txt
    ├── static
    │   ├── css
    │   │   └── bootstrap.css
    │   └── images
    │       ├── 1.jpg
    │       ├── 2.jpg
    │       ├── 3.jpg
    │       ├── 4.jpg
    │       ├── 5.jpg
    │       └── 6.jpg
    └── templates
        └── index.html
```

**app.py**

```
#!/usr/bin/env python3

from flask import Flask, render_template, request
import os,random,socket

app = Flask(__name__)

images = [
    "1.jpg",
    "2.jpg",
    "3.jpg",
    "4.jpg",
    "5.jpg",
    "6.jpg"
]

@app.route('/')
def index():
    host_name = "{} to {}".format(socket.gethostname(), request.remote_addr)

    image_path = "/static/images/" + random.choice(images)

    return render_template('index.html', image_path=image_path, host_name=host_name)

# Main
if __name__ == "__main__":
    port = int(os.environ.get("PORT", 80))
    try:
        app.run(host="0.0.0.0", port=port, debug=True)
    except Exception as ex:
        print(ex)
```

---

**requirements.txt**

```
Flask==1.0.2
gunicorn==19.9.0

```

---

**index.html**

```
<html>

<head>
  <meta charset="utf-8">
  <title>Kubernetes Sample</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta http-equiv="X-UA-Compatible" content="IE=edge" />
  <link rel="stylesheet" href="/static/css/bootstrap.css">
</head>

<body>
  <!-- Navbar-->
  <nav class="navbar navbar-dark bg-primary">
    <a class="navbar-brand" href="#">Kubernetes Sample</a>
  </nav>

  <!-- Contents -->
  <div class="container" style="padding:20px 0 0 0">
    <div class="row">
      <div class="alert bg-primary" role="alert">
        {{host_name}}
      </div>
    </div>
    <div class="row">
      <img class="img-fluid" src="{{image_path}}" alt="images" />
    </div>
  </div>
</body>

</html>
```

---

**Dockerfile**

```
# Base Image
FROM python:3.6

# Maintainer
LABEL maintainer "Shiho ASA"

# Upgrade pip
RUN pip install --upgrade pip

# Install Path
ENV APP_PATH /opt/photoview

# Install Python modules needed by the Python app
COPY requirements.txt $APP_PATH/
RUN pip install --no-cache-dir -r $APP_PATH/requirements.txt

# Copy files required for the app to run
COPY app.py $APP_PATH/
COPY templates/ $APP_PATH/templates/
COPY static/ $APP_PATH/static/

# Port number the container should expose
EXPOSE 80

# Run the application
CMD ["python", "/opt/photoview/app.py"]

```

> 이미지 파일과 css 파일은 위에 언급한 git repository를 참조하여 작성합니다.
>
> 필자의 gitlab에서는 kubernetes-study/docker/photo-view/static
>
> 책 소스코드에서는 Understanding-K8s/chap02/v2.0 안에 존재합니다.

#### Docker 이미지 빌드하고 Docker Hub에 업로드하기

소스코드 작성이 완료되었다면 Docker 이미지로 빌드하고, Docker Hub에 업로드를 진행한다. 빌드 및 업로드 과정은 다음과 같다.

```
$ docker build -t photo-view . 
$ docker run -p 80:80 photo-view
 * Serving Flask app "app" (lazy loading)
 * Environment: production
   WARNING: Do not use the development server in a production environment.
   Use a production WSGI server instead.
 * Debug mode: on
 * Running on http://0.0.0.0:80/ (Press CTRL+C to quit)
 ...

```

![](https://github.com/pandora0667/TILD/blob/master/screenshot/minikube/photo-view.png?raw=true)

정상적으로 코드 작성이 완료되었다면, 웹브라우저 접속 시 해당 화면이 나타나야 한다. 이제 해당 docker 이미지를 docker hub에 업로드하고 K8s yaml 파일을 작성하자

```
$ docker tag photo-view $(docker_id)/photo-view
$ docker push $(docker_id)/photo-view
```

### deployments, service yaml 파일 작성하기

-   deployments.yaml
    
    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: phtoview-deployment
    
    spec:
      replicas: 5 
      selector:
        matchLabels:
          app: phtoview-deployment
    
      template:
        metadata:
          labels:
            app: phtoview-deployment
            env: stage
    
        spec:
          containers:
          - name: phtoview-deployment
            image: $(docker_id)/photo-view
            resources:
              limits:
                memory: "128Mi"
                cpu: "500m"
    
            ports:
            - containerPort: 80
    ```
    
-   service.yaml
    
    ```
    apiVersion: v1
    kind: Service
    metadata:
      name: webserver
    
    spec:
      type: LoadBalancer
      selector:
        app: phtoview-deployment
    
      ports:
      - port: 80
        targetPort: 80
        protocol: TCP
    ```
    
    Yaml 파일 작성이 완료되었다면 이후 과정은 동일하다. minikube를 활용해서 K8s 클러스터에 애플리케이션을 배포한다. 이때 반드시 배포하고자 하는 애플리케이션은 docker 이미지 파일이 있어야 한다. 그렇지 않으면 K8s 내부에서 해당 이미지를 내려받지 못해 에러가 발생한다.
    

```
$ kubectl apply -f deployments.yaml
deployment.apps/phtoview-deployment created

$ kubectl apply -f service.yaml
service/webserver created
```

```
$ kubectl get deployments
NAME                  READY   UP-TO-DATE   AVAILABLE   AGE
phtoview-deployment   2/5     5            2           62s

$ kubectl get service
NAME         TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
kubernetes   ClusterIP      10.96.0.1      <none>        443/TCP        21h
webserver    LoadBalancer   10.96.165.78   <pending>     80:30648/TCP   63s
```

```
$ minikube service webserver
|-----------|-----------|-------------|-----------------------------|
| NAMESPACE |   NAME    | TARGET PORT |             URL             |
|-----------|-----------|-------------|-----------------------------|
| default   | webserver |             | http://192.168.99.102:30648 |
|-----------|-----------|-------------|-----------------------------|
🎉  Opening service default/webserver in default browser...
```

여기까지 잘 따라왔으면 어느 정도 K8s와 minikube에 대해 어느정도 감이 왔을 거라 생각한다. 그렇다면 추가적으로 간단하게 부트스랩을 활용하여 정적 웹페이지를 만들고 Dockerfile을 만들어서 응용하면 어떨까?

필자는 좋아하는 연예인의 페이지를 부트스트랩으로 작성하고, 위와 동일한 방법으로 서비스를 진행해 보았다. 작성한 파일은 다음과 같다.

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: dbqudwo333-deployment

spec:
  replicas: 5 
  selector:
    matchLabels:
      app: dbqudwo333-deployment

  template:
    metadata:
      labels:
        app: dbqudwo333-deployment
        env: stage

    spec:
      containers:
      - name: dbqudwo333-deployment
        image: jusk2/seongwon-byungjae
        resources:
          limits:
            memory: "128Mi"
            cpu: "500m"

        ports:
        - containerPort: 80

```

```
apiVersion: v1
kind: Service
metadata:
  name: webserver

spec:
  type: LoadBalancer
  selector:
    app: dbqudwo333-deployment
  ports:
  - port: 80
    targetPort: 80
    protocol: TCP

```

앞서 작성했던 파일에 큰 변화는 보이지 않다. 단지 배포할 Docker 이미지가 변경되었을 뿐, 모든 설정과 진행방식을 동일하다. 이 글을 보고 있는 여러분들도 각자만의 응용을 통해서 기본기를 익히는 것이 어떨까?