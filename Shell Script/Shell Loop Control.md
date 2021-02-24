# Shell Loop Control

본 장에서는 반복문을 Shell에서 반복문을 제어하는 방법에 대해서 알아본다. 지난 챕터에서는 반복문의 종류와 이를 사용하는 방법에 대해서 알아보았지만 때때로 반복문 제어를 통해서 적절한 문장을 수행할 수 있다.

## The infinite Loop

대부분의 조건문에서는 조건이 있기 때문에 조건이 충족되면 반복문을 나갈 수 있다. 무한 루프는 이러한 조건이 충족되지 않고 계속해서 반복문이 수행하면서 문장을 수행하는 것을 말한다.

### Example

```bash
#!/bin/bash

A=10

until [ $A -lt 10 ]
do
   echo $A
   a=`expr $A + 1`
done
```

while 문을 통해 0 ~ 9까지 숫자를 표기하는 예제에서 A는 10보다 크거나 같고 절대로 10보다 작아지지 않기 때문에 무한 루프가 생성된다.

## The break Statement

break는 반복문 안에서 어떠한 조건이 발생할 때까지 모든 코드를 실행하고 반복문을 종료한다.

### Example

```bash
#!/bin/bash

A=0

while [ $A -lt 10 ]
do
   echo $A
   if [ $A -eq 5 ]
   then
      break
   fi
   a=`expr $A + 1`
done
```

while 문을 통해서 0 ~ 5까지 숫자를 출력하는 반복문을 작성했을 때, A의 값이 5와 같은지 확인하고 결과가 참이 되면 반복문을 빠져나오게 된다.

다음은 중첩 for문에서 break를 사용하는 예제이다.

```bash
#!/bin/bash

for VAR1 in 1 2 3
do
   for VAR2 in 0 5
   do
      if [ $VAR1 -eq 2 -a $VAR2 -eq 0 ]
      then
         break
      else
         echo "$VAR1 $VAR2"
      fi
   done
done
```

```sh
$ bash loop.sh
1 0
1 5
3 0
3 5
```

중첩된 반복문 안에서 VAR1의 값이 2가 되고, VAR2의 값이 0이 되는 경우, break를 통해 반복문을 빠져나오게 된다.

## The continue statement

continue의 경우 break와 비슷한 형태로 작성하지만 반복문을 종료하지 않고 특정한 조건이 충족됬을 때, 해당 반복문을 계속해서 실행할 수 있도록 한다. 종종 오류가 발생했으나 반복을 계속 헤서 실행하고자 할 때 사용하기도 한다.

### Example

```bash
#!/bin/bash

NUMS="1 2 3 4 5 6 7"

for NUM in $NUMS
do
   Q=`expr $NUM % 2`
   if [ $Q -eq 0 ]
   then
      echo "Number is an even number!!"
      continue
   fi
   echo "Found odd number"
done
```

본 예제에서는 홀수, 짝수를 구분하는 간단한 예제를 보여주고 있다.

```sh
Found odd number
Number is an even number!!
Found odd number
Number is an even number!!
Found odd number
Number is an even number!!
Found odd number
```