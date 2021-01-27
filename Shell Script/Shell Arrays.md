# 쉘 스크립트 배열

![](https://miro.medium.com/max/3828/1*WHp2-ZURUgPLpu0WbvrFWA.png)

이번 시간에서는 쉘 스크립트에서 배열을 사용하여 변수의 집합을 그룹화하는 방법을 알아본다.

## 배열 정의

우리가 지금까지 쉘 스크립트에서 변수를 선언할 때 다음과 같은 방법을 사용했다.

```bash
#!/bin/bash

NAME01="Lucas"
NAME02="soengwon"
NAME03="wisoft"
NAME04="hanbat"

echo $NAME01
```

위에서 정의한 변수를 배열로 정의하면 다음과 같이 사용할 수 있다.

```bash
#!/bin/bash	

NAME[0]="Lucas"
NAME[1]="soengwon"
NAME[2]="wisoft"
NAME[3]="hanbat"

echo "First Index: ${NAME[0]}"
echo "Second Index: ${NAME[1]}"
```

만약 모든 배열에 접근하고자 한다면 다음 방법으로 수행할 수 있다.

```bash
${array_name[*]}
${array_name[@]}
```

쉘 스크립트에서 배열을 정의하는 방법은 다음과 같다.

```bash
array_name=("element 1" "element 2" "element 3")
```

다음 예제를 수행하여 결과를 확인한다.

```bash
#!/bin/bash

array=(111 "Lucas" "Tistory" 222 333)

echo $array
echo ${array[*]}
echo ${array[2]}
```

```shell
$ ./array.sh
111
111 Lucas Tistory 222 333s
Tistory
```

인덱스를 지정하지 않은 경우 첫 번째 값만 출력되며, 배열에 저장되어 있는 값을 모두 출력하거나 지정하여 출력할 수 있다.

#### ksh 배열 정의

ksh의 경우 bash와 다르게 set 명령을 사용하여 배열을 지정한다. 다음 예제를 확인한다.

```bash
#!/bin/ksh 

set -A array "lucas" 123 "foo" "bar"

echo $array
echo ${array[*]}
echo ${array[2]}
```

```shell
$ ./array.sh
lucas
lucas 123 foo bar
foo
```

## 변수 값 혹은 명령 실행 결과 변수 저장하기

괄호안에 있는 값을 직접 지정할 수 있지만 변수로 설정한 값을 배열에 저장하거나, \`\`를 통해서 명령의 결과를 저장할 수 있다.

```bash
#!/bin/bash

VAR="redhat debian gentoo darwin"
DISTRO=($VAR)
echo ${DISTRO[1]}

TODAY=(`date`)
echo ${TODAY[3]}

INFO=(`uname -a`)
echo ${INFO[1]}
```

```shell
$ ./array.sh
debian
수요일
MacBook-Pro.local
```

## 배열 값 추가

먼저 단일 요소를 추가하는 방법을 알아본다.

```bash
#!/bin/bash 

NUMBER=(1 2 3 4)
echo ${NUMBER[*]}

NUMBER+=(5)
echo ${NUMBER[*]}
```

여러 요소를 추가하고 싶다면 다음과 같이 수행한다.

```bash
#!/bin/bash 

NUMBER=(1 2 3 4)
echo ${NUMBER[*]}

NUMBER+=(5 6 7 8 9 10)
echo ${NUMBER[*]}
```

변수의 값을 요소로 추가하고자 하면 다음과 같이 수행한다.

```bash
#!/bin/bash 

COMMAND=("ls" "pwd" "ps" "clear")
echo ${COMMAND[*]}
ELEMENT="123 456"

COMMAND+=($ELEMENT)

echo ${COMMAND[*]}
```