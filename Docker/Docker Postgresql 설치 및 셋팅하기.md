# Docker Postgresql 설치 및 셋팅하기

* 본 문서는 Docker를 사용하여 Postgresql을 설치하고 셋팅하는 방법을 설명하고 있습니다. 

* Linux, Mac OS에서 사용할 것을 권장합니다. 

* 다음 링크에서 공식 postgres Docker 이미지를 다운로드 받습니다.

  ![](https://d1q6f0aelx0por.cloudfront.net/product-logos/a28dcd12-094d-4248-bfcc-f6fb954c7ab8-postgres.png)

  * https://hub.docker.com/_/postgres



---

1. 아래와 같이 최신 postgres Docker 이미지를 다운로드 받습니다. 

   ```shell
   $ docker pull postgres
   ```

2. admin 비밀번호를 셋팅하는 방법은 다음과 같습니다. 

   ```shell
   $ docker run -d -p 5432:5432 --name pgsql -e POSTGRES_PASSWORD=mysecretpassword postgres
   ```

3. Docker 볼륨을 생성하여 데이터를 계속해서 유지해야 한다면 다음 옵션을 사용합니다. 

   ```bash
   $ docker volume create pgdata
   $ docker run -d -p 5432:5432 --name pgsql -it --rm -v pgdata:/var/lib/postgresql/data postgres
   ```

4. 컨테이너에 접속하여 postgres 설정을 진행합니다. 

   ```bash
   $ docker exec -it pgsql bash
   
   root@cb9222b1f718:/# psql -U postgres
   psql (10.3 (Debian 10.3-1.pgdg90+1))
   Type "help" for help.
   postgres=# CREATE DATABASE mytestdb;
   CREATE DATABASE
   postgres=#\q
   ```

5. 기본 이미지에서 추가 초기화 작업을 진행할 경우 다음과 같이 설정을 진행합니다. 

   * 다음 예제는 사용자와 데이터베이스를 추가하는 작업입니다. 

   ```bash
   root@12345# vi /docker-entrypoint-initdb.d/init-user-db.sh
   
   #!/bin/bash
   set -e
   
   psql -v ON_ERROR_STOP=1 --username "$POSTGRES_USER" --dbname "$POSTGRES_DB" <<-EOSQL
       CREATE USER docker;
       CREATE DATABASE docker;
       GRANT ALL PRIVILEGES ON DATABASE docker TO docker;
   EOSQL
   ```

6. 다음과 같이 설정을 진행했다면 외부에서 데이터베이스를 사용할 수 있습니다. 