# Shell Script 변수 

본 문서는 [https://www.tutorialspoint.com/](https://www.tutorialspoint.com/) 에서 제공하는 UNIX / LINUX 쉘 튜토리얼을 참고했다.



## Extended Shell Scripts

쉘 스크립트를 수행할 작업과 시기를 알려주는 필수 구성요소가 필요하다. 대부분의 쉘 스크립트는 이보다 복잡하지만, 쉴 스크립트 역시 일종의 프로그래밍 언어이며, 변수, 함수, 제어, 반복 등과 같은 구조로 이루어진다. 스크립트가 복잡한 구조로 되어 있어도 순차적인 실행구조를 가진다.

다음은 간단한 입력 구조를 가지는 쉘 스크립트 예제이다.

```
#!/bin/sh

echo "What is your name?"
read PERSON
echo "Hello, $PERSON"
```

```
$./test.sh
What is your name?
lucas
Hello, lucas
```

## Using Shell Variables

![](https://media.vlpt.us/images/mgm-dev/post/f4a4edd9-0e4d-47c8-91d7-59616eb1e19e/1_Px7h03Ih7B5QZu4KQpSEoQ.png)

쉘에서 변수를 사용하는 방법에 대해 알아본다. 위키피디아의 정의에 따르면 변수는 "아직 알려지지 않거나 어느 정도까지만 알려져 있는 양이나 정보에 대한 상징적인 이름이다. 컴퓨터 [소스 코드](https://ko.wikipedia.org/wiki/%EC%86%8C%EC%8A%A4_%EC%BD%94%EB%93%9C)에서의 변수 이름은 일반적으로 [데이터 저장 위치](https://ko.wikipedia.org/wiki/%EB%A9%94%EB%AA%A8%EB%A6%AC_%EC%A3%BC%EC%86%8C)와 그 안의 내용물과 관련되어 있으며 이러한 것들은 프로그램 실행 도중에 변경될 수 있다."라고 정의되어 있다.

이를 쉽게 정의하면 다음과 같다.

1.  변수는 컴퓨터 메모리에 존재한다.
2.  할당된 메모리 공간은 정보를 저장하기 위해서 사용된다.
3.  정보가 저장된 공간을 찾기위해서, 이름을 붙여서 사용한다.

변수의 할당된 값은 숫자, 텍스트 파일, 파일 이름, 장치 또는 다른 유형의 데이터일 수 있으며, 변수는 할당된 메모리의 주소를 나타내는 포인터이기 때문에 변수를 생성, 할당, 삭제가 가능하다.

### Variable Names

쉘에서 변수이름을 지칭하는 규칙은 다음과 같다.

-   변수 안에 들어갈 수 있는 글자는 a to z, A to Z이다.
-   변수 안에 들어갈 수 있는 숫자는 0 ~ 9까지 이다.
-   서로 다른 변수 이름을 이어서 사용하기 원한다면 underscore character ( \_ )을 사용한다.
-   쉘 변수의 이름은 대문자를 사용한다.

올바른 변수 선언의 예제를 살펴보면 다음과 같다.

```
_ALL
NAME
VAR_1
VAR_2
```

잘못된 변수 선언의 예제는 다음과 같다.

```
2_VAR
-VARIABLE
VAR1-VAR2
VAR_A!
```

쉘에서!, -, \*와 같은 특수문자를 사용할 수 없는 이유는 쉘 자체에서 지칭하는 의미가 존재하기 때문이다.

### Defining Variables

변수를 정의하는 일반적인 방법은 다음과 같다.

```
variable_name=variable_value
```

```
NAME="Lucas"
```

위의 예제에서 확인할 수 있듯이 NAME이라는 변수를 정의하고 "lucas"라는 값을 대입할 수 있다. 이러한 유형의 변수를 스칼라 변수라고 지칭하는데 스칼라 변수는 한 번에 하나의 값을 저장할 수 있다는 특징을 가지고 있다.

```
ORGANIZATIONS="wisoft"
NUMER_OF_PEOPLE=30
```

### Accessing Values

변수에 저장된 값에 접근하기 위해서는 이름 앞에 $기호를 붙여야 한다. 다음 예제는 정의된 변수 NAME 값에 접근하고 STDOUT으로 출력한다.

```
#!/bin/sh

NAME="Lucas"
echo $NAME
```

### Read-only Variables

쉘에서는 읽기 전용으로 변수를 지정할 수 있으며, 읽기 전용으로 지정된 변수는 값을 변경할 수 없다. 다음 예제는 읽기 전용 변수에서 값을 변경하고자 할 때 나타나는 에러를 보여준다.

```
#!/bin/sh

NAME="Lucas"
readonly NAME
NAME="Seongwon LEE"
```

```
$ /bin/sh: NAME: This variable is read only.
```

### Unsetting Variables

변수에 할당된 값을 해제하면 더 이상 변수에 접근할 수 없다.

```
#!/bin/sh

NAME="Lucas"
unset NAME
echo $NAME
```

위의 변수를 실행하게 되면 어떠한 것도 출력되지 않으며, 읽기 전용으로 선언된 변수는 unset 할 수 없다.

## Variable Types

쉘이 실행하기 위해서는 다음과 같은 3가지의 주요 변수가 존재한다.

### 지역변수

-   쉘의 인스턴스에 존재하는 변수로 기본적으로 쉘에서 지정한 모든 변수는 전역 변수로 선언되기 때문에 지역변수를 사용하기 위해서는 local 키워드를 반드시 사용해야 한다.
-   혹은 쉘을 실행할 때 인자 값으로 넘겨줄 수 있다.

### 환경변수

-   쉘 스크립트를 통해 작성된 프로그램 중에서 정상적으로 동작하기 위한 변수이다.
-   선언된 변수는 모든 자식 프로세스에서 접근하여 사용할 수 있으며, 프로그램이 실행하기 위해 참조되는 경우 외에는 사용하면 안 된다.

### 쉘 변수

-   쉘이 동작하기 위해 필요한 특수 변수이다.
    
-   set 명령을 통해 쉘 변수를 확인할 수 있다.
    
    ```
    $ set
    '!'=0
    '#'=0
    '$'=985
    '*'=(  )
    -=569JNRXZghiklms
    0=-zsh
    '?'=0
    @=(  )
    ARGC=0
    BG
    CDPATH=''
    COLORFGBG='7;0'
    COLORTERM=truecolor
    COLUMNS=114
    COMMAND_MODE=unix2003
    COMP_WORDBREAKS=:
    CPUTYPE=x86_64
    CURRENT_BG=NONE
    ```
    
-   우리가 흔히 알고 있는 쉘 변수로는 $PATH, $HOME 등이 있다.