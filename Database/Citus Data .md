# Citus Data 

citus는 PostgreSQL 데이터베이스를 실행, 관리, 확장 등 대규모 환경에서 스케일아웃을 위해 사용된다. citus를 사용하여 데이터베이스를 수평적으로 확장이 가능하며, 데이터베이스로 들어오는 SQL 쿼리를 병렬화 하여 빠른 처리가 가능하다는 장점을 가지고 있다. 

PostgreSQL HA 구성을 위해 많이 사용하는 Streaming Replication과 pgpool과 비교했을 때, 설치가 간단하고 구성이 쉽다는 장점이 있으며, 오픈소스로 공개되어 있다. Microsoft Azure에서 손쉽게 사용할 수 있으나, 본 글에서는 VM 환경에서 구축하는 방법에 대해 소개한다. 



### System Requirements

테스트 환경에서 진행하기 위해서 다음과 같은 시스템에서 설정을 진행하였음을 밝힌다. 

> 2020.04월 기준 citus db는 Debian 및 Rad Hat 계열의 OS에서 설치가 가능하며, PostgreSQL12, citus92_12 버전을 사용한다. 

* OS : Centos7 
* vCPU : 2 
* RAM : 4GB
* DISK : 10GB 



