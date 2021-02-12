# Shell Decision Making

이번 장에서는 Unix Shell에서 특정 조건일 때, 올바른 수행이 가능하도록 하는 조건문에 대해서 알아본다. 



## The if...else statements 

if else 문은 주어진 옵션 집합에서 조건을 선택할 수 있도록 지원한다.

어떠한 조건에 대해서 True가 될 때 지정된 문이 실행되고, False일 경우 실행되지 않는다. 대부분 비교 연산자를 통해 작성한다. 아래 실습을 통해 알아볼 것이지만 각 구문에 대한 공백을 지켜야 오류가 발생하지 않는다. 



###  **if...fi** statement

#### 문법 

```shell
if [ expression ] 
then 
   Statement(s) to be executed if expression is true 
fi
```



#### Example

```shell
#!/bin/bash

a=10
b=20

if [ $a == $b ]
then
   echo "a is equal to b"
fi

if [ $a != $b ]
then
   echo "a is not equal to b"
fi
```

```bash
$ ./if.sh
a is not equal to b
```



### **if...else...fi** statement

#### 문법 

```shell
if [ expression ]
then
   Statement(s) to be executed if expression is true
else
   Statement(s) to be executed if expression is not true
fi
```



#### Example

```shell
#!/bin/bash

a=10
b=20

if [ $a == $b ]
then
   echo "a is equal to b"
else
   echo "a is not equal to b"
fi
```

```bash
$ ./if-else.sh 
a is not equal to b
```



### **if...elif...fi** statement 

#### 문법 

```shell
if [ expression 1 ]
then
   Statement(s) to be executed if expression 1 is true
elif [ expression 2 ]
then
   Statement(s) to be executed if expression 2 is true
elif [ expression 3 ]
then
   Statement(s) to be executed if expression 3 is true
else
   Statement(s) to be executed if no expression is true
fi
```



#### Example

```shell
#!/bin/bash

a=10
b=20

if [ $a == $b ]
then
   echo "a is equal to b"
elif [ $a -gt $b ]
then
   echo "a is greater than b"
elif [ $a -lt $b ]
then
   echo "a is less than b"
else
   echo "None of the condition met"
fi
```

```bash
$ ./if-else.sh 
a is less than b
```



## The case...esac Statement 

if...elif 문을 여러 개 사용하여 다중 경로 분기를 수행할 수 있지만 모든 분기가 단일 변수에 의해서 정해진다면 위의 방법은 적절하지 않다. 이러한 문제를 해결하기 위해서 case...esac 문을 사용하여 if...elif보다 효과적으로 스크립트를 작성할 수 있다. 



#### 문법 

```shell
case word in
   pattern1)
      Statement(s) to be executed if pattern1 matches
      ;;
   pattern2)
      Statement(s) to be executed if pattern2 matches
      ;;
   pattern3)
      Statement(s) to be executed if pattern3 matches
      ;;
   *)
     Default condition to be executed
     ;;
esac
```

문자열 word는 일치하는 항목이 발견될 때까지 모든 패턴과 비교를 수행하며, 일치한 패턴이 있을 경우 해당 작업을 수행한다. 만약 일치하는 항목이 없으면 아무 작업을 수행하지 않고 종료된다. 

최대의 패턴의 수는 정해져 있지 않지만 최소 하나 이상의 패턴이 존재해야 하며, ;;의 경우 C 프로그래밍에서 break와 동일한 기능을 수행한다. 



#### Example

```shell
#!/bin/bash

FRUIT="kiwi"

case "$FRUIT" in
   "apple") echo "Apple pie is quite tasty." 
   ;;
   "banana") echo "I like banana nut bread." 
   ;;
   "kiwi") echo "New Zealand is famous for kiwi." 
   ;;
esac
```

```bash
$ ./case.sh 
New Zealand is famous for kiwi.
```

실습한 예제를 제외하고 커멘트 라인에서 인수를 받아서 처리해야 하는 경우 유용하게 사용될 수 있다. 

#### Example

```shell
#!/bin/bash

option="${1}" 
case ${option} in 
   -f) FILE="${2}" 
      echo "File name is $FILE"
      ;; 
   -d) DIR="${2}" 
      echo "Dir name is $DIR"
      ;; 
   *)  
      echo "`basename ${0}`:usage: [-f file] | [-d directory]" 
      exit 1 # Command to come out of the program with status 1
      ;; 
esac 
```

```bash
$ ./test.sh
test.sh:usage: [-f file] | [-d directory]

$ ./test.sh -f hello-world
File name is hello-world

$ ./test.sh -d dev
Dir name is dev
```





