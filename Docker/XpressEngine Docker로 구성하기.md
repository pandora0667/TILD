# XpressEngine Docker로 구성하기

![](https://www.xpressengine.com/storage/app/public/seo/56/9f/20191217131508b6b8dc83166a47ae68c3b7ebe20a5d08d7994968.jpg)

 나만의 홈페이지를 만들고 싶을 때, 처음부터 시작하는 방법도  존재하는 다양한 오픈소스 프로그램을 활용해서  방법도 있다. Wordpress라는 좋은 플랫폼도 존재하지만 이번 시간에서는 xpressengine을 이용해서 홈페이지를 만들고자 하는 분들에게 도움을 드리고자 한다. 



### Xpressengine 요구사항 

 XE(xpressengine)는 공식사이트에 의하면 다음과 같은 기본 요구사항이 필요하다. 본 문서는 버전 3.0 이상을 기준으로 한다. 

* 웹서버 : Apache & Nginx 
* PHP 7 이상
  * PDO PHP Extension
  * cURL PHP Extension
  * FileInfo PHP Extension
  * GD PHP Extension
  * Mbstring PHP Extension
  * OpenSSL PHP Extension
  * Zip PHP Extension
* DB : MariaDB or MySQL 5.1 이상
* Disk : 500MB 이상 
* 호스팅이 가능한 네트워크 환경 구성 



### Xpressengine 설치

 기본적으로 윈도우나 Linux 환경에서 설치해서 사용할 수 있으나,  의존성이나 추가 패키지를 설치하는 과정이 귀찮은 작업이기에 Docker를 사용해서 설치하기로 했다. 다만  홈페이지에서는 Docker 설치 방법이 없기 때문에 간단하게 Dockerfile를 만들었다. 필요하신 분이라면 언제든지 아래 코드를 수정해서 사용하면 좋을 것 같다.

 일단 본 문서를 읽는 독자라면 Docker를  사용할 수 있는 분이라고 생각하며, 혹시 Docker에 대해 궁금한 점이 있다면 본 블로그에 Docker 항목을 참고하길 권장한다. 



#### Dockerfile

 먼저 [다운로드](http://start.xpressengine.io/download/latest.zip)링크를 클릭하여 latest.zip 파일을 다운로드 하고 다음과 같이 Dockerfile을 작성한다. 

```dockerfile
FROM ubuntu:18.04
LABEL seongwon "seongwon@edu.hanbat.ac.kr"

ARG DEBIAN_FRONTEND=noninteractive

WORKDIR /var/www/html

RUN apt-get update
RUN apt-get install -yq --no-install-recommends apt-utils build-essential
RUN apt-get install -yq evince

ENV TZ=Asia/Seoul
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get upgrade -yq
RUN apt-get install git wget unzip apache2 php7.2 php7.2-fpm \
    php7.2-mysql libapache2-mod-php7.2 php7.2-curl php7.2-gd php7.2-json php7.2-xml php7.2-mbstring php7.2-zip -yq

COPY ./latest.zip /var/www/html/
RUN unzip latest.zip && chmod -R 707 storage/ bootstrap/ config/ vendor/ plugins/ index.php composer.phar

RUN rm -rf /var/www/html/latest.zip /var/www/html/index.html
RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf
RUN service apache2 restart

EXPOSE 80

CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
```

 이후 XE의 추가버전이 업데이트 될 경우 latest.zip 파일만 다운받아 빌드하고 다음과 같이 실행한다. 

```bash
$ docker build -t xpressengine . 
$ docker run -d -p 8080:80 xpressengine
```

![](https://www.xpressengine.io/plugins/gitbookMarkDownParser/assets/html/dotgitbook/assets/install.gif)



 문제없이 실행이 된다면 웹인스톨러 화면이 나오게 되고, DB 정보를 입력해야 하는데, 이 부분도 Docker로 활용하고자 한다면 다음 docker-compose.yml를 참고하길 바란다. 

```yml
version: '3.4'

services:
  database:
    image: mariadb
    environment:
      MYSQL_ROOT_PASSWORD: ${PASSWORD}
      MYSQL_DATABASE: xe
      MYSQL_USER: ${USER}
      MYSQL_PASSWORD: ${PASSWORD}
    ports:
     - 3306:3306
    restart: always
    volumes:
     - ./mariadb:/var/lib/mysql

```



### Xpressengine 운영

 Docker를 사용하면 편하게 원하는 서비스를 간단하게 운영할 수 있다. XE Docker Dockerfile를 docker hub에 업로드하고 mariadb HA 구성까지 docker-compose·yml에 통합시켜 stack으로 deploy 하면 안정적인 운영이 가능할 것으로 보인다.

 다행이라면 XE 3.0 이상 버전부터는 기존 버전보다 설치 및 운영 커스터마이징 서드파티 사용이 좀 더 편해졌다는 장점이 있겠다. 하지만 아직도 부실한 개발자 문서화 및 사용자의 다양한 수정사항이 반영되고 서버 리소스를 적게 사용할 수 있도록 발전되었으면 하는 바람이다.  다양한 CMS 중에서 XE 사용을 고려하는 분들에게 조금이나마 도움이 되었으면 한다.





















