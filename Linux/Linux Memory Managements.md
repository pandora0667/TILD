# Linux Memory Managements  

### 개요

* 리눅스의 경우 메모리의 유휴공간을 캐시영역으로 잡아둔다. 이는 일반적인 환경에서 빠른 응답을 위한 기능이지만 과도한 메모리 사용은 서버 운영에 차질이 생길 수 있기 때문에 확인이 필요하다. 
* 메모리 사용률의 경우 어떠한 프로세스가 동작하고 있는지에 따라 다르기 때문에 서버에서 동작하고 있는 프로세스의 메모리 점유와 상태를 꾸준히 확인하는 것이 중요하다.
* 일반적으로 동작중인 프로세스가 메모리를 과도하게 사용하는 경우, 메모리 누수를 의심해 보아야 하며, 이 경우에는 본 문서로는 해결할 수 없음을 알린다. 



### 메모리 용량 확인 

```bash
$ cat /proc/meminfo | grep MemTotal
MemTotal:      2097152 kB
```



### 메모리 사용량순 프로세스 확인 

**RSS(Resident set size) : 물리 메모리를 실제 점유하고 있는 크기 **

1. 간단한 확인 방법 

   ```bash
   $ ps -ef --sort -rss
   ```

2. 상위 10개의 프로세스 메모리 사용량

   ```bash
   $ ps -ef --sort -rss | head -n 11
   ```

3. 메모리 사용량 표시

   ```bash
   $ ps -eo user,pid,ppid,rss,size,vsize,pmem,pcpu,time,cmd --sort -rss | head -n 11
   ```

4. 프로세스 수행명령에서 인수부분을 표시하지 않기 

   ```bash
   $ ps -eo user,pid,ppid,rss,size,vsize,pmem,pcpu,time,comm --sort -rss | head -n 11
   ```



### 명목 메모리 사용률 / 실질 메모리 사용률

* 명목메모리 사용률 : used / total = ( total - free ) / total
* 실질메모리 사용률 : used2 / total = ( total - free2 ) / total = ( total - free - buffers - cached) / total

#### 스크립트 

* 소수점 버리고 계산

  ```shell
  #!/bin/bash
  TOTAL=`free | grep ^Mem | awk '{print $2}'`
  USED1=`free | grep ^Mem | awk '{print $3}'`
  USED2=`free | grep ^-/+ | awk '{print $3}'`
  NOMINAL=$((100*USED1/TOTAL))
  ACTUAL=$((100*USED2/TOTAL))
  echo NOMINAL=${NOMINAL}% ACTUAL=${ACTUAL}%
  ```

* 소수점까지 계산

  ```shell
  #!/bin/bash
  TOTAL=`free | grep ^Mem | awk '{print $2}'`
  USED1=`free | grep ^Mem | awk '{print $3}'`
  USED2=`free | grep ^-/+ | awk '{print $3}'`
  NOMINAL=`echo "100*$USED1/$TOTAL" | bc -l`
  ACTUAL=`echo "100*$USED2/$TOTAL" | bc -l`
  echo NOMINAL=${NOMINAL:0:5}% ACTUAL=${ACTUAL:0:5}%
  ```

  

### 캐시메모리 해제하기 

* pagecache 해제

  ```bash
  $ echo 1 > /proc/sys/vm/drop_caches
  ```

* dentries, inodes 해제

  ```bash
  $ echo 2 > /proc/sys/vm/drop_caches
  ```

* pagecache, dentries, inodes 모두 해제

  ```bash
  $ echo 3 > /proc/sys/vm/drop_caches
  ```

* 플러싱

  ```bash
  $ sync
  ```











