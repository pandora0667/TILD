# Shell 특수 변수 

본 문서는 다음 링크를 참조하여 작성하였음을 알린다.

-   [https://www.tutorialspoint.com/unix/unix-special-variables.htm](https://www.tutorialspoint.com/unix/unix-special-variables.htm)

이번 시간에서는 유닉스/리눅스에서 사용하는 특수 변수에 대해서 알아보도록 한다. 이러한 변수 이름은 예약어로 지정되어 있기 때문에 지역, 전역 변수로 사용할 수 없으며, 스크립트 작성 시 각별하게 유의해야 한다.

## 예약 변수 (Reserved Variable)

일반적인 프로그래밍 언어에서 사용하는 예약 변수와 동일한 기능을 담당한다고 생각하면 된다. 쉘 프로그래밍을 작성할 때, 예약변수를 사용하면 보편적인 실행환경으로 작성할 수 있으므로 편리하게 사용할 수 있다.

| 변수명         | 설명                                                         |
| :------------- | :----------------------------------------------------------- |
| HOME           | 사용자 홈 디렉터리                                           |
| PATH           | 실행 파일을 찾을 경로                                        |
| LANG           | 프로그램 사용시 기본 지원되는 언어                           |
| FUNCNAME       | 현재 함수 이름                                               |
| SECONDS        | 스크립트가 실행된 시간 (초 단위)                             |
| SHLVL          | 중첩된 쉘 레벨 (현재 실행중인 쉘 레벨 수이며, Linux 배포판에 따라 다름) |
| SHELL          | 로그인해서 사용하고 있는 쉘                                  |
| PPID           | 부모 프로세스의 PID                                          |
| BASH           | BASH 실행 파일 경로                                          |
| BASH\_ENV      | 스크립트 실행시 BASH 시작 파일을 읽을 위치 변수              |
| BASH\_VERSION  | 현재 설치된 BASH 버전                                        |
| BASH\_VERSINFO | 배열로 상세 정보 출력                                        |
| MAIL           | 메일 보관 경로                                               |
| MAILCHECK      | 메일 확인 시간                                               |
| OSTYPE         | 운영체제 종류                                                |
| TERM           | 터미널의 종류                                                |
| HOSTNAME       | 호스트 이름                                                  |
| HOSTTYPE       | 시스템 하드웨어 종류                                         |
| MACHTYPE       | 머신 종류                                                    |
| LOGNAME        | 로그인 이름                                                  |
| UID            | 사용자 UID                                                   |
| TMOUNT         | 로그아웃할 시간, 0일 경우 무제한                             |
| USER           | 사용자의 이름                                                |
| USERNAME       | 사용자 이름                                                  |
| GROUPS         | 사용자 그룹 (/etc/passwd)                                    |
| HISTFILE       | 히스토리 파일 경로                                           |
| HISTFILESIZE   | 히스토리 파일 사이즈                                         |
| HISTSIZE       | 히스토리에 저장된 개수                                       |
| HISTCONTROL    | 중복되는 명령에 대한 기록 유무                               |
| DISPLAY        | X 디스플레이 이름                                            |
| IFS            | 입력 필드 구분자                                             |
| VISAUL         | VISUAL 편집기 이름                                           |
| EDITOR         | 기본 편집기 이름                                             |
| COLUMNS        | 터미널의 컬럼 수                                             |
| LINES          | 터미널의 라인 수                                             |
| LS\_COLORS     | ls 명령의 색상 관련 옵션                                     |
| PS1            | 기본 프롬프트 스트링. 기본값은 \`\\s-\\v$ '                  |
| PS2            | 긴 문자 입력을 위해 나타나는 문자열. 기본 값은 `>`           |
| PS3            | 쉘 스크립트에서 `select` 사용시 프롬프트 변수(기본값: `#?`)  |
| PS4            | 쉘 스크립트 디버깅 모드의 프롬프트 변수                      |

## 부록 - 특수 권한

일반적으로 리눅스 유닉스는 사용자의 파일 권한을 부여하여 기초적인 보안체계를 유지한다. 파일이나 디렉터리에서는 user, group, other 권한이 존재하며 각각 읽기, 쓰기, 실행 권한을 부여할 수 있다.

이러한 권한을 수정하기 위해서 chmod 명령을 사용하는데 쉘에서 특수 권한을 주기 위해서도 같은 명령을 사용한다.

### SetUID

-   파일을 실행할 때 일시적으로 소유자의 권한을 얻어 실행할 수 있도록 한다.
    
-   root 권한으로 지정된 프로그램에 SetUID가 지정되어 있으면 root 권한으로 실행된다.
    
    ```
    $ touch setuid
    $ ll 
    -rw-r--r--  1 seongwon  staff     0B  1 25 14:50 setuid
    
    $ chmod 4644 ./setuid
    -rwSr--r--  1 seongwon  staff     0B  1 25 14:50 setuid
    ```
    
-   기존에 실행권한이 없으면 대문자 S, 있으면 소문자 s로 표시된다.
    
-   리눅스에서 설정되어 있는 대표적인 파일은 /usr/bin/passwd이다.
    
    ```
    $ ls -l /usr/bin/passwd
    -rwsr-xr-x 1 root root 68208 May 28  2020 /usr/bin/passwd
    ```
    
    -   해당 파일은 계정의 비밀번호를 변경할 수 있도록 하는 실행파일로 root만 변경 가능하도록 설정되어 있다.
    -   /usr/bin/passwd에 SetUID가 설정되어 있지 않으면 일반 사용자는 root 사용자를 통해 비밀번호를 변경해야 하므로 일반 사용자도 /etc/passwd 파일을 수정 가능하도록 설정되어 있다.
    -   /etc/passwd는 사용자의 이름과 같은 권한에 대한 데이터베이스만 존재하며, 실제 비밀번호는 /usr/bin/passwd 사용자가 패스워드를 변경할 때만 사용한다. 과거 패스워드가 파일 형태로 저장되어 있으나 현재는 저장되지 않는다. 이러한 부분에 대한 자세한 설명은 shadow파일의 대해 검색하길 바란다.

### SetGID

-   파일을 실행할 때 일시적으로 파일 소유 그룹의 권한을 얻어 실행할 수 있도록 한다.
    
    ```
    $ touch setgid
    $ ll 
    -rw-r--r--  1 seongwon  staff     0B  1 25 15:20 setgid
    
    $ chmod 2644 ./setuid
    -rw-r-Sr--  1 seongwon  staff     0B  1 25 15:20 setgid
    ```
    
    -   기존 권한에 실행권한이 없으면 대문자 S, 있으면 소문자 s로 표시된다.

### Sticky Bit

-   Sticky Bit가 설정된 디렉토리에 파일을 생성하면 해당 파일은 생성한 사람의 소유가 되며 소유자와 root만이 해당 파일에 대한 삭제 및 수정에 대한 권한을 가질 수 있다.
    
-   즉 Sticky Bit가 설정된 디렉토리안에 누구나 파일을 생성할 수는 있지만 삭제는 본인과 관리자만 가능하다.
    
    ```
    $ mkdir sticky
    $ chmod 1644 sticky 
    $ ll 
    drw-r--r-T  2 seongwon  staff    64B  1 25 15:22 sticky
    ```
    
-   일반적으로 리눅스, 유닉스는 파일이나 디렉터리의 소유자 혹은 소유 그룹만이 삭제, 수정할 수 있도록 권한을 지정하지만 모든 사용자들이 파일을 생성, 수정, 삭제할 수 있는 다음과 같은 공용 디렉터리도 존재한다.
    
    -   /tmp
    -   /var/tmp
-   파일의 소유자가 아닌 다른 사용자가 퍼미션이 777인 파일을 삭제, 수정하는 과정에서 문제가 발생할 수 있기 때문에 를 방지하기 위해 Sticky Bit를 설정하여 공용 디렉터리라도 해당 파일의 소유자와 관리자만 삭제, 수정 권한을 줄 수 있도록 한다.
    

## 특수 변수

| 변수명 | 설명                                                         |
| ------ | ------------------------------------------------------------ |
| $0     | 현재 스크립트 파일 이름                                      |
| $n     | 스크립트가 호출된 인수, n은 인수의 십진수를 표현 ($1, $2, $3 ...) |
| $#     | 매개 변수의 총 개수                                          |
| $\*    | 전체 인자 값                                                 |
| $@     | 모든 인자가 ""로 묶여 있으며, 스크립트가 두 개의 인수를 받으면 $1, $2와 동일하다. |
| $?     | 마지막으로 실행된 명령어, 함수, 스크립트 자식의 종료 상태    |
| $$     | 현재 스크립트의 PID                                          |
| $!     | 마지막으로 실행된 백그라운드 PID                             |

## Command-Line Arguments

위에서 설명한 특수 변수를 활용하여 쉘 스크립트를 작성하고, 인수를 넘긴다.

```
#!/bin/sh

echo "File Name: $0"
echo "First Parameter : $1"
echo "Second Parameter : $2"
echo "Quoted Values: $@"
echo "Quoted Values: $*"
echo "Total Number of Parameters : $#"
```

```
$ ./arguments.sh lucas wisoft
File Name: ./arguments.sh
First Parameter : lucas
Second Parameter : wisoft
Quoted Values: lucas wisoft
Quoted Values: lucas wisoft
Total Number of Parameters : 2
```

```
#!/bin/sh

for TOKEN in $*
do
   echo $TOKEN
done
```

```
$ ./arguments2.sh lucas wisoft hanbat seongwon
lucas
wisoft
hanbat
seongwon
```