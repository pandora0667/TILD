# PART 1 Shell Script Basic

![](https://github.com/pandora0667/TILD/blob/master/screenshot/Shell%20Script/Untitled.png?raw=true)

## Shell Script

  쉘 스크립트는 리눅스/유닉스에서 실행하기 위해 고안된 오픈소스 프로램이다. 쉘 스크립트에서는 여러 명령을 작성하여, 반복적이고 단순한 형태의 작업을 프로그래밍하고 실행가능한 파일형태로 저장하여 사용할 수 있도록 한다. 

  쉘이란 사용자와 운영체제 간의 인터페이스를 지칭하는 유닉스 용어이다. 쉘을 통해 사용자에게 인터페이스를 제공하고 사람이 해석할 수 있는 명령을 시스템이 해석할 수 있는 명령으로 변환하여 사용자가 원하는 명령을 실행할 수 있도록 한다.

![](https://github.com/pandora0667/TILD/blob/master/screenshot/Shell%20Script/Untitled%201.png?raw=true)

   위의 그림에서 확인할 수 있듯이 커널은 운영체제가 동작하기 위한 가장 핵심적인 부분으로 하드웨어와 운영체제간의 통신을 위해 사용되며, 쉘은 사용자의 입렵받은 명령은 운영체제에 전달하고 이에대한 출력을 터미널 출력을 통해서 사용자간의 상호 통신이 가능하도록 한다. 

---

## 쉘의 종류

  

  이번에는 Unix/GNU Linux에서 가장 많이 사용하는 5가지의 쉘 종류을 살펴본다. 

![](https://github.com/pandora0667/TILD/blob/master/screenshot/Shell%20Script/Untitled%202.png?raw=true)

1. **Bash Shell**

    ![](https://github.com/pandora0667/TILD/blob/master/screenshot/Shell%20Script/Untitled%203.png?raw=true)

      Bash Shell은 Bourne Again Shell의 약자로 1989년에 출기되어 오늘까지 대부분의 리눅스 배포판에서 사용하는 기본 쉘이다.

    - Command line editing
    - 탭 완성
        - 사용하려는 명령의 일부만 입력하고 Tab키를 입력하면 해당 명령을 완성해준다.
        - 만약 입력된 명령어와 중복되는 명령이 많은 경우에는 Tab키를 2번 입력하면 모든 명령을 표시한다.
    - Unlimited size command history
        - 커서표 키를 이용하여 호출했던 명령을 다시 불러 낼 수 있으며, 이 기능을 통해서 예전에 입력한 명령을 호출하여 재사용할 수 있다.
    - Shell Functions and Aliases
        - bash에 내장된 명령을 alias로 지정하여 사용할 수 있다.
    - Job Control

2. **Tcsh/Csh Shell**

    ![](https://github.com/pandora0667/TILD/blob/master/screenshot/Shell%20Script/Untitled%204.png?raw=true)

      Tcsh은 Berkely Unix csh에서 파생되었어 초기 유닉스 시스템에서 사용되어 가장 긴 역사를 가지고 있다.  Tcsh은 스크립트 언어로 C언어 문법과 매우 유사한 특징을 가지고 있다. 

    - C like syntax
    - Command-line editor
    - Programmable word and filename completion
    - Spelling correction
    - Job control

3. **Ksh Shell**

    ![](https://github.com/pandora0667/TILD/blob/master/screenshot/Shell%20Script/Untitled%205.png?raw=true)

      Ksh은 1983년 AT&T의 벨 연구소에서 근무하던 데이비드 콘이 설계하고 개발하였다. sh(Bourne Shell)을 확장해서 확장해서 개발하였ㅇ며, 벨 연구소 사용자의 요청으로 Csh Shell의 특징을 모두 제공하면서 처리속도가 빠르다는 장점을 가지고 있다. 

4. **Zsh Shell**

    ![](https://github.com/pandora0667/TILD/blob/master/screenshot/Shell%20Script/Untitled%206.png?raw=true)

      Zsh은 Unix / GNU Linux Shell의 많은 기능을 통합하여 설계된 Shell로서 1990년도에 출시된 강력한 Shell이다.  다양한 확장 스크립트 및 문법을 제공하며, 개발자들 사이에서 많이 사용되는 Shell이다. 

    - Filename generation
    - Startup files
    - Login/Logout watching
    - Closing comments
    - Concept index
    - Variable index
    - Functions index
    - Key index and many more that you can find out in man pages

5. **Fish**

    ![](https://github.com/pandora0667/TILD/blob/master/screenshot/Shell%20Script/Untitled%207.png?raw=true)

      Fish는 “friendly interactive shell”을 의미하며 2005년에 개발되었다. Fish을 사용하면 구문강조, 자동제안 및 탭 완성 등과 같은 기능을 제공하며, 사용하기 위해 위에서 설명한 Shell 종류보다 간편하고 복잡한 구성이 필요없다는 특징을 가지고 있다. 

    - Man page completions
    - Web based configuration
    - Auto-suggestions
    - Fully scriptable with clean scripts
    - Support for term256 terminal technology

    ---

## 쉘 확인하기

  본인이 사용하고 있는 PC에서 어떠한 쉘을 사용하고 있는지 확인하기 위해서는 다음과 같은 명령을 입력한다. 

```bash
$ echo $SHELL
/bin/zsh
```

## 명령어 기반의 쉘 스크립트 작성하기

  본격적으로 쉘 스크립트를 학습하기 이전에 쉘에서 사용하는 명령을 나열하고 이를 간단한 스크립트로 작성하도록 한다. 

```bash
$ date 
$ cal 
$ pwd 
$ ls 
```

```bash
$ vi task.sh 
```

```bash
date
cal
pwd
ls
```

```bash
$ chmod +x task.sh
$ ./task.sh

2021년 1월 11일 월요일 15시 20분 00초 KST
      1월 2021
일 월 화 수 목 금 토
                1  2
 3  4  5  6  7  8  9
10 11 12 13 14 15 16
17 18 19 20 21 22 23
24 25 26 27 28 29 30
31
/Users/seongwon
Applications				Pictures
Creative Cloud Files			Postman
Desktop					Public
Developments				Virtual Machines.localized
Documents				VirtualBox VMs
Downloads				django-sample-for-docker-compose
Google Drive File Stream		go
Library					pgdata
Movies					task.sh
Music					zsh-syntax-highlighting
```

## Hello World 쉘 스크립트 작성

```bash
#!bin/bash
echo "Hello World" 
```

```bash
$ chmod +x hello-world.sh
$ ./hello-world.sh
Hello World
```

  쉘 스크립트 작성시 가장 첫 라인에 #!bin/bash를 작성하는 이유는 스크립트 파일이 bash로 실행한다는 의미한다. 이를 작성하지 않은 경우에도 리눅스 배포판은 기본적으로 Bash로 설정되어 있기 때문에 동일한 쉘에서는 정상적으로 동작하지만 이를 지정함으로서 다른 쉘간의 오류를 방지할 수 있다. 

  참고로 #기호는 주석을 의미하지만 #!bin/bash 에서는 주석을 의미하지 않는다. 또한 #!bin/bash와 #!bin/sh는 동일한 의미를 가지고 있으므로 혼용해도 상관이 없다. 

## 주석 설정하기

  주석을 설정하기 위해서는 다음과 같이 진행한다. 

```bash
#comment
```

![](https://github.com/pandora0667/TILD/blob/master/screenshot/Shell%20Script/Untitled%208.png?raw=true)

## 쉘 스크립트 실행

  쉘 스크립트는 위에서 아래로 순서대로 지시할 명령을 포함하는 텍스트 파일이다. 따라서 명령을 해석하기 위한 쉘 정의가 반드시 필요하다. 

```bash
$ echo date > date.sh 
$ cat date.sh
date
$ ./date.sh
zsh: permission denied: ./date.sh
$ bash date.sh
2021년 1월 11일 월요일 15시 41분 18초 KST 
```