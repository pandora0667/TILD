# PostgreSQL,  MySQL Docker 데이터베이스 초기화 하기

 2020년 애플리케이션을 디플로이할 때 Docker를 활용하는 일은 당연한 일이 되어가고 있다. 본문에서는 Docker가 무엇인지 어떻게 사용하는지에 대한 설명을 다루는 것이 아닌 Docker를 활용해 데이터베이스를  초기화 방식에 대해서 알아보도록 하는 시간을 가진다.

> Docker에 대한 기본 지식은 필자 블로그에 설명했기 때문에 Docker가 무엇인지, 활용하는지에 대한 방법을 잘 모른다면 글을 읽어보고 오는 것을 추천한다.



### 환경

 Docker를 사용하는 사람이라면 윈도우보다는 Linux, MacOS에서 매끄럽게 동작한다는 사실을 알고 있을 것이다. 윈도우도 예전보다 좋아지긴  MacOS 카탈리나에서 실습을 진행했음을 밝힌다.



### Docker 데이터베이스 SQL 

 PostgreSQL,  MySQL이든 Docker 실행할 때 내부 데이터베이스를  방법은 동일하다. docker-compose.yml 혹은 Dockerfile 파일에 미리 해당 데이터베이스를 정의하고, docker-entrypoint-initdb.d에 SQL 스크립트 파일을 추가하면 Docker 이미지 실행 시 자동으로 해당 SQL 스크립트를 읽어서 데이터베이스 세팅이 진행된다.



#### Docker 데이터베이스 docker-compose.yml

 MySQL에서 해당 환경변수는 다양하게 존재하는 데 가장 많이 사용하는 변수는 `MYSQL_ROOT_PASSWORD`, `MYSQL_DATABASE`이다. 

```yml
db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: "password"
      MYSQL_DATABASE: "database_name"
      MYSQL_ALLOW_EMPTY_PASSWORD: "yes"
```

 PostgreSQL의 변수도 크게 다르지 않다. 대표적인 변수로는 `POSTGRES_PASSWORD`, `POSTGRES_USER`, `POSTGRES_DB` 이 있다. 

```yaml
db:
    image: postgres
    environment:
      POSTGRES_PASSWORD: "password"
      POSTGRES_USER: "username"
      POSTGRES_DB: "database_name"
```

 위와같이 yaml 파일을 정의하면 컨테이너 생성 시 데이터베이스 이름과 비밀번호가 생성되며, 설정한 데이터베이스에는 우리가 정의한 SQL 스크립트 내용이 실행된다.



#### docker-entrypoint-initdb.d 설정

 docker-compose.yml 파일에 기본 내용 작성을 완료하면 다음에는 컨테이너 내부에 docker-entrypoint-initdb.d에 SQL 스크립트 파일을 저장해야 한다. 스크립트 이름은 상관은 없으나, 컨테이너 docker-entrypoint-initdb.d 내부에 파일이 반드시 지정되어 있어야 한다. 

```yaml
    volumes:
      - "./init/:/docker-entrypoint-initdb.d/"
```

 docker-compose.yml이 존재하는 디렉터리에서 init 디렉터리 밑에 **init.sql**을 생성했다면 해당 SQL 파일은 볼륭으로 마운트 시켜서 읽어드리겠금 한다. 이렇게 하면 컨테이너 실행 시 최초 한 번에 대해서 스크립트 파일에 내용이 반영된다. 



### Example 

 아래 파일은 MySQL을 활용한 docker-compose.yml 예제 파일이다. 

```yml
version: '3.3'

services:

  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: "password"
      MYSQL_DATABASE: "seongwon"
      MYSQL_ALLOW_EMPTY_PASSWORD: "yes"
    
    volumes:
      - "./init/:/docker-entrypoint-initdb.d/"
    ports:
      - 3306:3306 
    container_name: example_mysql
```

```shell
$ mkdir init
$ vi init.sql
```

```sql
SET NAMES utf8 ;

DROP TABLE IF EXISTS `acp`;
SET character_set_client = utf8mb4 ;

CREATE TABLE `acp` (
  `ri` varchar(200) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL,
  `pv` longtext NOT NULL,
  `pvs` longtext NOT NULL,
  PRIMARY KEY (`ri`),
  CONSTRAINT `acp_ri` FOREIGN KEY (`ri`) REFERENCES `lookup` (`ri`) ON DELETE CASCADE ON UPDATE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```



### Docker로 데이터베이스 메인 운영이 적절한가?

 사실 이 부분은 사설이긴 하나,  서비스를 디플로이하는데 있어서 가장 신경 써야 하는 부분이 과연 데이터베이스를 Docker로 메인 운영이 가능한지에 대한 의문이 들 수 있다. Docker의 여러 장점이 있기 때문에 데이터베이스를 Docker로 운영하는 데 크게 지장은 없다. 하지만 데이터베이스의 핵심은 데이터이기 때문에 데이터가 소실되지 않도록 각별히 신경 써야 한다.

 기본적으로 컨테이너는 시스템 리부팅 등의 일이 일어나면 컨테이너의 내부 내용이 전부 사라지기 때문에 저장된 데이터가 전부 소실될 수 있기 때문이다. 이를 해결하기 위해서 볼륨을 사용하는 경우도 있으나, 데이터 마이그레이션 할 때 저장된 데이터까지 완벽하게 마이그레이션 할 수 있다는 보장이 없다. 따라서 실제 운영 시에는 Docker 내부의 데이터를 외부에 파일 형태로 마운트 시키거나 외부 스토리지에 저장하는 형식으로 저장하는 것이 좋다. 이럴 경우 컨테이너만 tar로 추출해서 다른 서버에서 파일을 마운트 시키면 된다. (컨테이너를 tar 형태로 추출하는 방법에 대해서는 다음에 서술하도록 하겠다)

 중요한 시스템이라면 Docker에 너무 의존하기보다 직접 설치하고 세팅해서 운영하고, 백업을 철저히 진행하는 것이 당연할 것이다. []()



 

 

 