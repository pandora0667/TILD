# Ubuntu Mysql Database install and setting


## install 
1. apt-cache search mysql-server
2. sudo apt install mysql-server -y 
3. root password **** 
4. install complete 


## 설치 확인 
1. cat /etc/init.d/mysql
2. /etc/init.d/mysql status or systemctl status mysql.service

## Database CRUD 
1. CREATE DATABASE `데이터베이스명` CHARACTER SET utf8 COLLATE utf8_general_ci;
2. DROP DATABASE `데이터베이스명`;
3. SHOW DATABASES;
4. USE `데이터베이스명`

## 유저생성 
1. create user 'testUser'@'%' identified by 'pass'; // 원격접속허용
2. create user 'testUser'@'localhost' identified by 'pass; // 로컬접속
3. flush privileges;
4. grant all privileges on mvc.* to jusk2@'%' identified by 'lsw940630';

## mysql 원격접속 
1. /etc/mysql/mysql.conf.d
2. sudo vi mysqld.cnf 
3. bind-address		= 127.0.0.1 -> 0.0.0.0
4. sudo systemctl restart mysql.service