# Docker Node Example 

* Node Express를 활용한 간단한 Dockerfile를 만들어서 Docker 이미지를 만들어보도록 하겠습니다. 
* 연구실은 Nexus를 통해서 Registry 구성이 완료되어 있으며, 관련 설정내용은 다음 링크에 자세히 설명되어 있으므로 참고하여 주시길 바랍니다. 
  * [Nexus Docker Private Registry](https://git.wisoft.io/wisoft-lab/infrastructure/blob/master/Setting/Nexus%20Registry%20Setting%20.md)



### 주의사항 

* 본 문서는 Mac OS를 기준으로 작성되었습니다. 
  * Linux 사용시 sudo를 반드시 사용해야 하며 윈도우의 경우 사용 방법이 상의할 수 있습니다. 
  * Docker는 가급적 Linux에서 사용하는 것을 추천합니다. 



### 기존 Docker 이미지 정리하기

```bash
$ docker stop $(docker ps -a -q)
$ docker rm $(docker ps -a -q)
$ docker rm $(docker images -a -q)
```



### Create node express application 

* server.js 


* Dockerfile 

  ```dockerfile
  FROM node:11
  
  # Create app directory
  WORKDIR /usr/src/app
  
  # Install app dependencies
  # A wildcard is used to ensure both package.json AND package-lock.json are copied
  # where available (npm@5+)
  COPY package*.json ./
  
  RUN npm install
  # If you are building your code for production
  # RUN npm install --only=production
  
  # Bundle app source
  COPY . .
  
  EXPOSE 8080
  CMD [ "npm", "start" ]
  ```

* .dockerignore

  ````dockerfile
  node_modules
  npm-debug.log
  ````



### Building your image

```bash
$ docker build -t <username>/node-web-app .
```

```bash
$ docker images 

REPOSITORY                      TAG        ID              CREATED
node                            8          1934b0b038d1    5 days ago
<your username>/node-web-app    latest     d64d3505b0d2    1 minute ago
```



### Run the image 

```bash
$ docker run -p 8080:8080 -d  <username>/node-web-app

$ docker ps 
$ docker logs <container id> 

# Example 
Running on the http://localhost:8080

$ curl -i localhost:8080

HTTP/1.1 200 OK
X-Powered-By: Express
Content-Type: text/html; charset=utf-8
Content-Length: 12
ETag: W/"c-M6tWOb/Y57lesdjQuHeB1P/qTV0"
Date: Mon, 13 Nov 2017 20:53:59 GMT
Connection: keep-alive

Hello world
```

















###  