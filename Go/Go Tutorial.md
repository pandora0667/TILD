# Go Tutorial 



[TOC]

## Go 언어란? 

![](https://hackernoon.com/drafts/2c35295i.png)

 2019년 구글이 개발한 프로그래밍 언어로써, GC (garbage collection)와 병행성 (concurrent)을 잘 지원하는 컴파일 언어입니다. 로버트 그리즈머, 롭 파이크, 케네스 톰슨이 C++의 복잡성이 싫어서 개발되었습니다. 

 현재도 어떠한 패키지에 무엇을 포함할지는 이 세 사람이 만장일치로 합의해야 이루어진다고 하며, **Golang** 으로 불리기도 합니다. Go 언어 사용자들을 고퍼(Gopher)라고 부르며, 고퍼들을 위한 연례행사인 고퍼콘(Gophercon)이 열리고 있습니다. 



### Go 언어 특징

 Go 언어의 특징은 다음과 같습니다.  

1. 컴파일 언어이지만 컴파일러가 소스 코드를 해석하는 pass 수를 줄여서 인터프리터 언어처럼 빠르게 동작합니다. 
2. 언어의 문법이 간결하여 접근하기 쉽고 높은 성능을 낼 수 있습니다. 
3. 자료형 체계에서 정적 타입 검사가 이루어지기 때문에 Python 등에 익숙해져 있는 경우 생소할 수 있지만 풍부한 라이브러리를 통해서 다양한 기능을 쉽게 구현할 수 있습니다. 
4. 고루틴이라는 비동기 매커니즘을 제공하여 이벤트 처리 및 병렬 프로그래밍 작성이 쉽습니다. 
5. 고루틴은 OS에서 관리하는 경량 스레드보다 더 가볍기 때문에 CPU 코어갯수와 무관하게 수백, 수천만 고루틴을 작성해도 성능에 문제가 발생하지 않습니다. ( 비동기 처리 부분은 Erlang에서 영향을 받았기 때문 )
6. 파일 언어인 덕분에, 속도가 느린 스크립트 언어에서 연산 퍼포먼스가 필요한 부분을 Go로 대체해 넣을 수도 있습니다. 



![](https://w.namu.la/s/9954b15909d4b5acd1772a6a0ec35a7002645f92db28bc44568e8fb1c35a9b406dc23afa2e164dd95b24f783fff197e55897e2691d061cfc3ad60f328fabd43b5387e96539a2541b0a70fb3d3c39f5fd9e39d43af753bf4aaefd367452200b57)

 2016년 중반부터 CMS (Contents Management System)와 마이크로서비스 부분으로 사용 영역 및 점유율이 급격하게 늘어서 현재 스택오버플로우에서 실시한 2017년 미국 개발자 평균 연봉 1위 (11만불)의 언어로 기록되었습니다. 



### 지원 OS

![](https://www.hackint0sh.org/wp-content/uploads/2019/05/Best-os-for-Gaming.jpg)

 현재는 리눅스, 윈도우10, macOS 등 여러 플랫폼을 지원합니다. (윈도우 XP, 윈도우 Vista를 지원하지 않습니다.) 바이트코드를 생성하는 언어가 아니기 때문에 바이너리만 배포할 경우 C/C++ 프로그램이 그렇듯 해당 타깃 머신에 맞춰서 각각 컴파일해서 배포하거나 소스코드를 배포해야 합니다. 



### Go 키워드 및 도구

 Go는 단순함과 실용성을 지원하는 언어이기 때문에 키워드가 25개 밖에 되지 않습니다. 

* break
* default
* func
* interface
* select
* case
* defer
* go
* map
* struct
* chan
* else
* goto
* package
* switch
* const
* fallthrough
* if
* range
* type
* continue
* for
* import
* return
* var

 Go는 수많은 언어 배포판들과 동일한 종류의 디버깅, 테스트, 코드 검사도구를 포함하고 있으며, 다음을 포함합니다. 

* go build :  소스 파일 자체의 정보만을 사용하여 Go 바이너리를 빌드한다. 별도의 makefile은 없다.
* go test : 유닛 테스트 및 마이크로벤치마크
* go fmt : 코드 서식 지정
* go get : 원격 패키지의 검색 및 설치
* go vet : 코드 내의 잠재적인 오류를 찾아내는 정적 분석기
* go run : 코드를 빌드하고 실행하는 바로 가기
* godoc : HTTP를 통해 문서 확인
* gorename : 변수, 함수 등을 안전(type-safe) 방식으로 이름 변경
* go generate : 코드 생성기를 호출하는 표준 방식



### Go를 사용하는 프로젝트

 Go로 작성되어 사용되는 프로그램은 다음과 같습니다. 

- [라이트닝 네트워크](https://ko.wikipedia.org/w/index.php?title=라이트닝_네트워크&action=edit&redlink=1): [비트코인](https://ko.wikipedia.org/wiki/비트코인) 네트워크.
- [CockroachDB](https://ko.wikipedia.org/w/index.php?title=CockroachDB&action=edit&redlink=1): SQL 데이터베이스.
- [도커](https://ko.wikipedia.org/wiki/도커_(소프트웨어)): [리눅스](https://ko.wikipedia.org/wiki/리눅스) 컨테이너를 배치시키는 도구들의 집합
- Doozer: 매니지드 호스팅 제공자 [헤로쿠](https://ko.wikipedia.org/wiki/헤로쿠)의 락 서비스
- Geth 소프트웨어: [이더리움](https://ko.wikipedia.org/wiki/이더리움) 프로토콜 [블록체인](https://ko.wikipedia.org/wiki/블록체인) 기술을 이용한 golang 구현체로서, 전 세계 공유 컴퓨팅 플랫폼을 구현한다.
- Gogs: 셀프 호스팅 Git 서비스.
- [InfluxDB](https://ko.wikipedia.org/wiki/InfluxDB): 고가용성과 고성능 요구사항을 필요로 하는 오픈 소스 데이터베이스.
- [Juju](https://ko.wikipedia.org/w/index.php?title=Juju&action=edit&redlink=1): [캐노니컬](https://ko.wikipedia.org/wiki/캐노니컬)이 주관하는 서비스 오케스트레이션 도구. ([우분투](https://ko.wikipedia.org/wiki/우분투_(운영_체제)) 리눅스의 패키저)
- [Kubernetes](https://ko.wikipedia.org/wiki/Kubernetes): 컨테이너 관리 소프트웨어
- [오픈시프트](https://ko.wikipedia.org/wiki/오픈시프트): 클라우드 컴퓨팅 플랫폼 ([레드햇](https://ko.wikipedia.org/wiki/레드햇)이 서비스함)
- 패커(Packer): 여러 플랫폼을 대상으로 하나의 소스 구성을 통해 동일한 머신 이미지를 만드는 도구.
- [스내피](https://ko.wikipedia.org/wiki/스내피)(Snappy): [우분투 터치](https://ko.wikipedia.org/wiki/우분투_터치)용 패키지 관리자 (캐노니컬 제작)
- [Syncthing](https://ko.wikipedia.org/w/index.php?title=Syncthing&action=edit&redlink=1): 오픈 소스 파일 동기화 클라이언트/서버 애플리케이션
- [GitLab-runner](https://ko.wikipedia.org/w/index.php?title=GitLab-runner&action=edit&redlink=1): 오픈 소스 CI/CD 애플리케이션.
- 이더리움 (geth)

---

Go를 사용한 일부 저명한 오픈 소스 소프트웨어 프레임워크는 다음과 같다.

- [Beego](https://ko.wikipedia.org/w/index.php?title=Beego&action=edit&redlink=1): 고성능 웹 프레임워크
- [Martini](https://ko.wikipedia.org/w/index.php?title=Martini&action=edit&redlink=1): 웹 애플리케이션/서비스용 패키지.
- [고릴라](https://ko.wikipedia.org/w/index.php?title=고릴라_(소프트웨어)&action=edit&redlink=1): Go용 웹 툴킷.
- [Enduro/X ASG](https://ko.wikipedia.org/w/index.php?title=Enduro/X&action=edit&redlink=1): 클러스터 미들웨어, 애플리케이션 서버, 분산 트랜잭션, 멀티 프로세싱 프레임워크

----

Go를 사용하는 저명한 기업 및 사이트는 다음과 같다(일반적으로 다른 언어와 함께 사용

- [AeroFS](https://ko.wikipedia.org/w/index.php?title=AeroFS&action=edit&redlink=1): 프라이빗 클라우드 파일싱크 어플라이언트 제공자
- [Chango](https://ko.wikipedia.org/w/index.php?title=Chango&action=edit&redlink=1): Go를 사용하는 프로그래머틱 광고 회사
- [클라우드 파운드리](https://ko.wikipedia.org/wiki/클라우드_파운드리): [PaaS](https://ko.wikipedia.org/wiki/PaaS)
- [클라우드플레어](https://ko.wikipedia.org/wiki/클라우드플레어)
- [CoreOS](https://ko.wikipedia.org/wiki/CoreOS): [도커](https://ko.wikipedia.org/wiki/도커_(소프트웨어)) 컨테이너를 활용하는 리눅스 기반 운영 체제
- [카우치베이스 서버](https://ko.wikipedia.org/wiki/카우치베이스_서버): 쿼리 및 인덱싱 서비스 (Couchbase 서버 내)
- [드롭박스](https://ko.wikipedia.org/wiki/드롭박스): 일부 중요한 구성 요소들을 파이썬에서 Go로 이관함.
- [구글](https://ko.wikipedia.org/wiki/구글)의 수많은 프로젝트: (download server dl.google.com 등)
- [MercadoLibre](https://ko.wikipedia.org/w/index.php?title=MercadoLibre&action=edit&redlink=1): 여러 퍼블릭 API.
- [몽고DB](https://ko.wikipedia.org/wiki/몽고DB): MongoDB 인스턴스 관리 도구
- [넷플릭스](https://ko.wikipedia.org/wiki/넷플릭스): 서버 아키텍처의 두 부분
- [노바티스](https://ko.wikipedia.org/wiki/노바티스): 내부 인벤토리 시스템용
- [Plug.dj](https://ko.wikipedia.org/w/index.php?title=Plug.dj&action=edit&redlink=1): 상호작용 온라인 소셜 뮤직 스트리밍 웹사이트.
- [Replicated](https://ko.wikipedia.org/w/index.php?title=Replicated&action=edit&redlink=1): [도커](https://ko.wikipedia.org/wiki/도커_(소프트웨어)) 기반 [PaaS](https://ko.wikipedia.org/wiki/PaaS) (기업의 설치 가능 소프트웨어 제작용)
- [사운드클라우드](https://ko.wikipedia.org/wiki/사운드클라우드): 수십 개의 시스템용.
- [트위치](https://ko.wikipedia.org/wiki/트위치)
- [우버](https://ko.wikipedia.org/wiki/우버)



### 부록

 겨울왕국의 Let it Go를 패러디한 Write in Go라는 노래가 공개되었습니다. 영상 링크와 가사는 아래에서 확인하세요. 

Write in Go : https://youtu.be/yhC-361QGJw

>The schedule's tight on the cluster tonight
>분산환경에서 해야 할 일로 쉴 틈 없을 오늘밤
>So I parallelized my code
>그래서 나는 병렬화된 코드를 작성했어
>All those threads and continuations
>그 모든 스레드와 컨티뉴에이션들로
>My head's going to explode
>머리는 폭발하기 일보직전이야
>And all that boilerplate
>심지어 그 모든 의례적인 코드들
>That FactoryBuilderAdapterDelegateImpl
>그 모든 "괴상한디자인패턴의복잡다단한구현"
>Seems unjustified
>이건 맞는 방법이 아닌 것 같아
>Give me something simple
>제발 알려줘 좀 더 간단한 방법을
>
>Don't write in Scheme
>스킴([Scheme](https://namu.wiki/w/Scheme))은 쓰지 마
>Don't write in C
>[C](https://namu.wiki/w/C(프로그래밍 언어))로 짜지 마
>No more pointers that I forget to free()
>해제하길 깜빡한 [포인터](https://namu.wiki/w/포인터)여 이제 그만 안녕
>Java's verbose, Python's too slow
>자바([Java](https://namu.wiki/w/Java))는 장황하고, 파이썬([Python](https://namu.wiki/w/Python))은 느려터졌단 걸
>It's time you know
>이제 깨달을 때가 왔어
>
>Write in Go! Write in Go!
>Go로 짜! Go로 짜!
>No inheritance anymore
>클래스 상속이여 이제 그만 안녕
>Write in Go! Write in Go!
>Go로 짜! Go로 짜!
>There's no do or while, just for
>do도 while도 없어, 오직 for뿐
>I don't care what your linters say
>당신의 린터가 뭐라고 말하든 상관없어
>I've got tools for that
>내겐 수정툴이 있다고
>The code never bothered me anyway
>코드는 더 이상 날 괴롭힐 수 없어
>
>It's funny how some features Make every change seem small
>몇 개의 함수가 재수정을 작게 보이게 만드니 재밌어
>And the errors that once slowed me Don't get me down at all
>느리게 하던 에러들도 더 이상 날 새로 짜게 만들지 못해
>
>It's time to see what Go can do
>이제 Go가 뭘 할 수 있는지 알아볼 시간야
>'Cause is seems too good to be true
>왜냐면 믿기 어렵도록 너무 좋아보이니까
>No long compile times for me.
>더 이상 기나긴 컴파일 타임은 없어
>I'm free!
>난 자유야!
>
>Write in Go! Write in Go!
>Go로 짜! Go로 짜!
>Kiss your pointer math goodbye
>포인터 산술과 작별의 키스를 해
>Write in Go! Write in Go!
>Go로 짜! Go로 짜!
>Time to give GC a try
>GC가 알아서 정리하게 할 시간이야
>I don't care if my structures stay On the heap or stack
>난 상관 안 해 내 구조체가 힙이나 스택 영역에 남아 있어도
>
>My program spawns its goroutines without a sound
>내 프로그램이 단 하나의 에러 사운드 없이 Go 루틴을 생성한다
>Control is spiraling through buffered channels all around
>버퍼링 된 채널들을 통해 제어가 나선형처럼 진행된다
>I don't remember why I ever once subclassed
>내가 왜 옛날엔 서브 클래스를 생성했는지 기억도 안 나
>I'm never going back My tests all build and pass!
>다신 돌아가지 않을 거야, 내 모든 테스트 빌드가 잘 돌아가!
>
>Write in Go! Write in Go!
>Go로 짜! Go로 짜!
>You won't use Eclipse anyomore
>넌 이클립스를 다시는 안 쓰게 될 거야
>Write in Go! Write in Go!
>Go로 짜! Go로 짜!
>Who cares what Boost is for?
>[부스트](https://namu.wiki/w/Boost) 라이브러리가 알 게 뭐야?
>
>I don't care what the tech lead say
>난 상관 안 해 기술 책임자가 뭐라 말하든
>I'll rewrite it all!
>내가 전부 다시 짤 거야!
>Writing code never bothered me anyway
>코드 짜기는 더 이상 날 괴롭힐 수 없으니까



## Go 준비하기 

![](https://github.com/1ambda/1ambda.github.io/raw/master/assets/images/golang/golang-tutorial.jpg) 

 위 글에서 Go언어에 대한 기본적인 개념과 컨셉을 이해했습니다. 이번에는 Go를 시작하기 위한 준비과정에 대한 설명을 진행하도록 하겠습니다. 본 문서는 2020년 7월 기준으로 작성되었으며, 업데이트 및 각 개발 환경의 변화에 따라서 일부 내용이 변경될 수 있습니다. 이러한 부분이 발생할 경우에는 seongwon@edu.hanbat.ac.kr로 문의주시면 확인 후 수정할 수 있도록 조치하겠습니다. 



### Go 설치하기 

 Go언어는 다음과 같은 대부분의 OS를 지원합니다. 본인의 환경에 맞춰서 설치를 진행하고 버전확인을 통해 정상적으로 설치사 진행되었는지 확인하도록 하겠습니다. 

> 2020.07.04일 기준 Go 최신버전은 1.14.4 입니다. 



#### 윈도우 

 Go 언어는 windows 7 / windows 10을 공식적으로 지원하기 때문에 이전 버전을 사용하고 있다면 OS 업그레이드를 진행하여 주시길 바랍니다. 본 설치는 윈도우 10 2004 빌드에서 진행하였습니다. 

1. https://golang.org/ 다음 링크로 접속합니다. 

   ![](https://github.com/pandora0667/TILD/blob/master/screenshot/go/windows-install/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202020-07-04%20%EC%98%A4%EC%A0%84%2012.30.53.png?raw=true)

2. 다운로드 링크로 접속하여 Featured downloads 항목에 Microsoft Windows 항목을 클릭하여 설치파일을 다운받습니다. (OS는 64bit 이어야 합니다.)

   ![](https://github.com/pandora0667/TILD/blob/master/screenshot/go/windows-install/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202020-07-04%20%EC%98%A4%EC%A0%84%2012.31.07.png?raw=true)

   3. 다운받은 파일을 실행하여 설치를 진행합니다. 

      ![](https://github.com/pandora0667/TILD/blob/master/screenshot/go/windows-install/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202020-07-04%20%EC%98%A4%EC%A0%84%2012.31.37.png?raw=true)

      ![](https://github.com/pandora0667/TILD/blob/master/screenshot/go/windows-install/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202020-07-04%20%EC%98%A4%EC%A0%84%2012.31.57.png?raw=true)

      ![](https://github.com/pandora0667/TILD/blob/master/screenshot/go/windows-install/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202020-07-04%20%EC%98%A4%EC%A0%84%2012.33.01.png?raw=true)

4. 설치가 완료되면 cmd 창에서 다음 명령어를 입력하여 정상적으로 설치가 진행되었는지 확인합니다. 

   ![](https://github.com/pandora0667/TILD/blob/master/screenshot/go/windows-install/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202020-07-04%20%EC%98%A4%EC%A0%84%2012.33.24.png?raw=true)

   ```powershell
   C:\Users\jusk2>go version
   go version go1.14.4 windows/amd64
   
   C:\Users\jusk2>go env
   set GO111MODULE=
   set GOARCH=amd64
   set GOBIN=
   set GOCACHE=C:\Users\jusk2\AppData\Local\go-build
   set GOENV=C:\Users\jusk2\AppData\Roaming\go\env
   set GOEXE=.exe
   set GOFLAGS=
   ...
   ```

   

#### Linux 

 본 설치 방법에서는 Ubuntu 20.04 LTS 버전을 기준으로 설치를 진행합니다.  APT 혹은 바이너리 설치파일을 통해서 설치가 가능하기 때문에 2가지 방법을 모두 소개하도록 하겠습니다. 



##### APT 활용하기 

```bash
$ sudo apt update && sudo apt upgrade -y
$ sudo add-apt-repository ppa:gophers/archive
$ sudo apt update
$ sudo apt install golang-1.14.1 -y 
```

---

```bash
$ sudo apt update && sudo apt upgrade -y 
$ sudo apt install golang
```

---

##### 바이너리 활용 

```bash
$ wget -c https://golang.org/dl/go1.14.4.src.tar.gz -O - | sudo tar -xz -C /usr/local
$ export PATH=$PATH:/usr/local/go/bin  # ~/.profile 
$ source ~/.profile
```

```bash
$ go env
$ go version
```



#### Mac OS 

 macOS Catalina 10.15.5 버전 기준으로 설치를 진행했습니다. 

```bash
$ brew install go

$ go env
$ go version
```



### 개발환경 설정하기

 Go 설치를 완료했습니다. 지금부터는 Go 언어를 사용하기 위한 IDE 설치를 진행하도록 하겠습니다. 기본적으로 Go가 컴퓨터에 설치되어 있다면 vim이나 메모장 등의 간단한 에디터로도 작성할 수 있지만 본 강좌에서는 Jetbrains GoLand와 Vistual Studio Code (vscode)를 사용하도록 하겠습니다. 

![](https://resources.jetbrains.com/storage/products/goland/img/meta/goland_logo_300x300.png)

  Jetbrains GoLand는Go 개발자를 위해 제작된 IDE입니다. IDE의 특성에 맞게 코드분석, 실행 및 디버그가 가능하다는 장점이 있습니다. 일반 사용자에게는 비용을 청구해야 하지만 학생의 경우 Education 계정을 생성하면 무료로 사용이 가능합니다. Education 계정 생성에 관한 문의는 각 학교에 문의하여 사용하시길 바랍니다. 

![](https://gaussian37.github.io/assets/img/etc/dev/vsc/vsc.png)

 Microsoft에서 제작한 Visual Studio Code (vscode)의 경우 IDE라고 하기보다는 코드 에디터로 보는것이 정확하지만 오픈소스로 공개되어 있기 때문에 무료로 사용할 수 있다는 점과 다양한 플로그인을 통해서 확장이 가능하다는 장점이 있습니다. 본인의 환경에 맞춰서 사용하면 되며, 실제 Go 프로그래밍을 작성하기에는 전혀 문제가 되지 않습니다. 



## Hello World 작성하기 

 모든 언어의 시작은 Hello World 라는 단어를 자신의 컴퓨터에 표시하는 것으로 시작된다고 해도 과언이 아닙니다. 앞서 설명한 Go언어의 장점들과 특징을 익히기 전에 다음 코드를 작성하도록 하겠습니다. 

```go
package main 

import "fmt"
  
func main() {
	fmt.Println("Hello, world!")
}

```

  Go  프로그램은 패키지로 이루어져 있는데, 그 중 main 패키지만이 Go 프로그램이 실행될 수 있는 패키지입니다. 따라서 main() 함수를 포함하는 패키지의 이름이 main이 아닐 경우 에러가 발생합니다. 

 import의 경우 해당 코드내에서 사용하는 패키지를 사용하는 것으로 사용할 경우에는 반드시 선언해야 합니다. **만약 선언을 하지 않거나 사용하지 않는 패키지가 있는 경우에는 에러가 발생합니다.** import 구문은 다음과 같이 작성될 수 있습니다. 

```go
import "fmt" // 일반적인 사용 

import f "fmt" // 패키지 이름이 길거나 축약하여 사용하고 싶을 때

import ( // 여러 패키지 사용이 필요할 때
	 f "fmt"
  "math/rand"
  "strings"
)
```

 위의 import 구문에서 확인 할 수 있듯이 상황에 맞춰서 패키지를 import 하여 사용할 수 있습니다. 만약 임시적으로 사용하는 패키지를 import하는 경우에는 다음과 같이 처리해야 합니다. 

```go
import (
	 f "fmt"
  _ "math/rand"
  _ "strings"
)
```

 사용하지 않는 패키지 변수 등에는 _ 를 사용해야 하는 특성을 잊지 말아야 합니다. 이러한 특성 때문에 디버그를 하는 등의 경우에는 불편할 수 있지만 전체적으로 코드를 간결하고 일관성 있게 유지하며, 불필요한 함수, 모듈, 패키지 남용으로 인해 복잡성이 증가하는 문제를 사전에 차단할 수 있습니다. 



## 변수 설정하기

 Go 언어는 기본적으로 **var**키워드를 사용해서 변수를 선언할 수 있고, 자료형을 생략하거나 추가할 수 있습니다. Go 언어는 기본적으로 변수 타입을 추론하기 때문인데 다음과 같은 예제를 살펴보도록 하겠습니다. 

```go
var name string // 변수 name은 문자열이다. 
var age int 

var title string = "Golang" // 변수 title은 문자열이며, golang 문자열을 초기화(대입) 한다. 
var number int = "30"
```

 일반적인 언어와 다르게 Go 언어의 경우 자료형이 뒤에 오기 때문에 다른 언어를 접하신 분들이 Go 언어를 접하게 되면 어색하게 느껴질 수 있습니다. 하지만 가만히 살펴보면 사람이 글자를 쓰는 방식과 비슷하기 때문에 가독성에서 뛰어나다고 할 수 있습니다. 

 이번에는 뒤에오는 자료형을 생략해 보겠습니다. 

```go
var name // 컴파일 에러

var title  = "Golang" // 변수 title은 문자열이며, golang 문자열을 초기화 한다. 
var number = "30"
```

 위와 다르게 자료형을 생략했으나 우리는 해당 변수의 형태를 통해 어떠한 변수가 대입되는지 확인할 수 있습니다. 이것을 **타입추론**이라고 부릅니다. 하지만, 자료형이 생략된 상태에서 변수가 선언되는 경우 어떠한 자료형이 대입될지 알 수 없기 때문에 개발자의 실수를 방지하기 위해서 초기화 하지 않는 변수의 경우 Go 컴파일러가 에러를 발생합니다. 

 다음은 변수를 선언하고, 선언된 변수를 출력하는 예제 입니다. 

```go
package main

import f "fmt"

func main() {
	name := "seongwon" // 선언과 동시에 변수를 초기화 할 수 있습니다. 이 경우 축약된 문법을 사용할 수 있습니다. 
	age := 27

	var title string = "Golang"
	var number int = 30

	f.Println(name, age, title, number)
}

```

 Go 언어에서는 한번에 여러 변수 값을 초기화 하거나 상수를 설정하고, iota를 사용하여 규칙적으로 값을 증가시킬 수 있습니다. 

```go
var ( // 변수 i, b, s 는 정수, boolean, 문자열이다. 
	i int
	b bool
	s string
)

var name, title, num1, num2 = "seongwon", "golang", 1, 2 // 각 변수에 순서대로 값을 대입한다. 이때 타입은 초기화된 변수의 형태를 통해 결정된다. 
```

```go 
const NICKNAME = "lucas"

const (
	GO = iota // 여러 상수를 열거하고, 0부터 1씩 값을 증가시킨다. 
	JAVA
	PYTHON
	C
)
```

 iota를 사용하면 단순히 값을 1씩 증가시키며 사용하는 것 이외에 다음과 같이 변형하여 사용할 수 있습니다. 

```go
const (
	_ = iota // 0 -> 무시
  KB ByteSize = 1 << (10 * iota) // << 연산자는 비트를 1만큼 쉬프트 합니다. 따라서 본 구문은 2의 10이기 때문에 1024가 됩니다. 
	MB
	GB
  TB
  PT
  EB
)
```

 다음 예제를 통해서 위에서 설명한 코드가 정상적으로 동작하는지 확인하세요. 

```go
package main

import f "fmt"
type ByteSize uint64

var (
	i int
	b bool
	s string
)

var name, title, num1, num2 = "seongwon", "golang", 1, 2

const NICKNAME = "lucas"

const (
	GO = iota // 여러 상수의 값을 0부터 1씩 값을 증가시킨다.
	JAVA
	PYTHON
	C
)

const (
	_ = iota // 초기값이 0이기 때문에 버린다. 
	KB  ByteSize = 1 << (10 * iota) // << 연산자는 비트를 이동시킨다. 본 문법에서는 1을 왼쪽으로 10번 이동하므로 2의 10승은 1024가 됩니다.
	MB
	GB
	TB
	PT
	EB
)

func main() {
	i, b, s = 1, true, "example"

	f.Println(i, b, s)
	f.Println("nickname : ", NICKNAME)
	f.Println(name, title, num1, num2)

	f.Println(GO, JAVA, PYTHON, C)

	f.Println(KB, MB, GB, TB, PT, EB)
}

```



## 조건문 / 분기문 / 반복문 

### if  사용하기

 if 문은 해당 조건이 만족하면 {}안에 코드를 수행합니다. 다만 다른 언어와 다르게 조건식을 ()를 작성하지 않아도 되지만 반드시 Boolean 형식으로 작성해야 하며, 해당 문법을 같은 라인에 두어야 합니다. 이를 어기게 될 경우 에러가 발생합니다. 다음은 기본적인 if 문법을 사용한 예제입니다. 

```go
package main

import f "fmt"

func main() {
	num := 10

	if num <= 10 {
		f.Println("10 보다 작거나 같다")
	}
}

```

 다른 프로그래밍 언어와 같이 if ~ else 문도 같이 사용가능합니다. 어떠한 조건이 충족되지 않을 경우 if ~ else 문으로 넘어가게 되고, 모든 조건이 충족되지 않을경우 else 문으로 넘어갑니다. 다음 예제에서는 어떠한 숫자를 입력하고 그 숫자가 특정 숫자보다 큰지 작은지 확인하는 예제입니다. 

```go
package main

import f "fmt"

func main() {
	var num int
	f.Println("1 ~ 10까지의 정수 하나를 입력")
	f.Scanln(&num)

	if num >= 1 && num <= 5 {
		f.Println("숫자는 1보다 크고 5보다 작다")
	} else if num >= 6 && num <= 10 {
		f.Println("숫자는 6보다 크고 10보다 작다")
	} else {
		f.Println("잘못 입력 했습니다.")
	}
}

```

 참고로 Go 언어에는 삼항 조건문이 없습니다. 따라서 간단한 조건문이라도 완전한 if 문을 사용해야 합니다. 



### switch 사용하기 

 go 언어에서 switch 여러 분기에 걸친 조건문들 사용하기에 적당합니다. 다만 다른 언어와 다르게 switch 문에 사용되는 break를 사용하지 않아도 된다는 특징이 있습니다. 

 아래는 기본 switch 문을 사용하는 예제입니다. 

```go
package main

import f "fmt"

func main() {
	var number int
	f.Println("Input number")
	f.Scanln(&number)

	switch number {
	case 1:
		f.Println("1")
	case 2:
		f.Println("2")
	case 3:
		f.Println("3")
	default:
		f.Println(number)
	}
```

 다음은 fallthrough을 사용하는 예제입니다. 

```go
test := 1
	switch test {
	case 1:
		f.Println("1")
		fallthrough
	case 2:
		f.Println("2")
		fallthrough
	case 3:
		f.Println("3")
		fallthrough
	default:
		f.Println("default")
	}
```

 swith 문을 활용하여 현재가 무슨 요일인지 확인하는 예제를 작성해 보도록 하겠습니다. 먼저 시간을 구하기 위한 Time 패키지를 사용하며, 구해진 시간을 활용하여 일주일에서 어떤 요일인지 확인하는 작업이 필요합니다. Go 언어에서는 구현하고자 하는 패키지가 이미 존재하기 때문에 간단하게 구할 수 있습니다. 다음 예제를 확인하세요.

```go 
package main

import (
	f "fmt"
	"time"
)

func main() {

	currentTime := time.Now()
	f.Println("현재 시간 : ", currentTime)

	weekday := time.Now().Weekday()

	switch weekday {
	case time.Monday:
		f.Println("월요일")
	case time.Tuesday:
		f.Println("화요일")
	case time.Wednesday:
		f.Println("수요일")
	case time.Thursday:
		f.Println("목요일")
	case time.Friday:
		f.Println("금요일")
	default:
		f.Println("주말")
	}
}

```



### for 사용하기

 다른 프로그래밍 언어와 다르게 for는 Go의 유일한 루프 구조입니다. C / C++ / JAVA와 같은 구조를 가지고 있기 때문에 익숙하게 사용할 수 있습니다. for를 사용하기에 앞서서 Go 언어에서는 다음과 같은 구조를 가지고 있습니다. 

1. 하나의 조건을 갖는 가장 기본적인 유형   
2. 기본적인 초기화/조건/후연산 for 루프
3. 조건이 없는 for는 루프 밖으로 break 하거나 함수 내부에서 return을 하기 전까지 계속 반복 (루프의 다음 반복을 위해 continue를 사용할 수 있습니다.)

```go
i := 1
	for i <= 3 {
		fmt.Println(i)
		i = i + 1
	}

```

```go
	for j := 7; j <= 9; j++ {
		fmt.Println(j)
	}
```

```go
	for {
		fmt.Println("loop")
		break
	}
```

```go
	for n := 0; n <= 5; n++ {
		if n%2 == 0 {
			continue
		}
  }
```

 추후 range, 채널 그리고 다른 자료구조들을 살펴볼 때 몇가지 다른 형태의 for문을 활용할 것입니다.  다음 2가지 예제를 통해 for문을 익숙해 지도록 하겠습니다. 

```go
package main

import f "fmt"

func main() {
	i := 1
	for i <= 3 {
		f.Println(i)
		i = i + 1
	}

	for j := 7; j <= 9; j++ {
		f.Println(j)
	}
	
	for {
		f.Println("loop")
		break
	}

	for n := 0; n <= 5; n++ {
		if n%2 == 0 {
			continue
		}
		f.Println(n)
	}
}

```

```go
package main

import f "fmt"

func main() {
	var number int

	f.Println("Multiplication table")
	f.Scanln(&number)

	for i := 0; i < 10; i++ {
		f.Println(number, " * ", i, " = ", number*i)
	}

	f.Println("------------------------------------")

	for i := 0; i < 5; i++ {
		if i == 3 {
			f.Println("3이 되었습니다.")
			break
		}
	}

	f.Println("------------------------------------")

	for i := 0; i < 5; i++ {
		for j := 0; j < 5; j++ {
			if j == 3 {
				continue
			}
			f.Println(i, j)
		}
	}
}

```



## 배열 / 슬라이스 / 맵 

### 배열 사용하기 

 Go 언어에도 다른 프로그래밍 언어와 동일하게 배열이 존재합니다. 배열은 숫자, 문자, 문자열 등을 나열해 놓은 상태로서 다양한 값이 들어갈 수 있습니다. 또한, 배열은 비슷한 데이터들을 한번에 가지고 있을 때 사용되거나, 반복적으로 사용되어야 하는 값들의 집합을 만들 때 사용합니다.

 Go 언어에서 배열은 길이가 고정되어 있고, 다른 언어와 마찬가지로 배열의 인덱스는 0부터 시작합니다. 다음 예제를 통해 배열을 선언하도록 하겠습니다. 

```go
	var empty [5]int // 크기가 5인 배열을 생성
	empty[4] = 5 // empty 배열 4번째 부분에 값을 5로 초기화 한다. 
	f.Println(empty)

	var a [3]int = [3]int{1, 2, 3} // 정수형의 크기가 3인 배열을 선언하고 값을 초기화 한다. 
	var b = [3]string{"name", "age", "weight"} // 문자열의 크기가 3인 배열을 선언하고 값을 초기화 한다. 
	var c = [5]float32{ // 실수형의 크기가 5인 배열을 선언하고 값을 초기화 한다. 
		1.14,
		2.14,
		3.14,
		4.14,
		5.14,
	}
```

 배열의 크기를 미리 선언하지 않더라도 초기화 할 요소의 개수 만큼 배열의 크기가 정해집니다. 

```go
	d := [...]string{"apple", "lg", "samsung", "kt"}
```

 만약 다차원의 배열을 선언하고 싶다면 다음 예제와 같이 작성합니다. 현재는 2차원 배열이지만 3차원 4차원 배열 등도 동일하게 선언할 수 있습니다. 

```go
	var multiArray = [3][3]int{
		{1, 2, 3},
		{4, 5, 6},
		{7, 8, 9},
	}
```

 len 키워드를 사용하면 배열의 크기를 쉽게 산출할 수 있습니다. 다음 예제 코드를 통해 배열을 연습해 보세요. 

```go 
package main

import f "fmt"

func main() {

	var empty [5]int
	empty[4] = 5
	f.Println(empty)

	var a [3]int = [3]int{1, 2, 3}
	var b = [3]string{"name", "age", "weight"}
	var c = [5]float32{
		1.14,
		2.14,
		3.14,
		4.14,
		5.14,
	}

	d := [...]string{"apple", "lg", "samsung", "kt"} // 배열을 선언과 동시에 초기화 하는 경우 []의 배열 크기를 생략할 수 있습니다. 

	f.Println("array size", len(a))

	for i := 0; i < len(b); i++ {
		f.Println("b 내용 : ", b[i])
	}

	f.Println(c, d)

	var multiArray = [3][3]int{
		{1, 2, 3},
		{4, 5, 6},
		{7, 8, 9},
	}
  
  for _, array := range multiArray {
   for _, value := range array {
      f.Println(value)
   }
 }
  
	f.Println("----------------------")
	f.Println(multiArray)
	f.Println("array size", len(multiArray))

}

```

 배열의 값을 for문으로 쉽게 순회하고 싶다면 아래와 같이 간단하게 정의할 수도 있습니다. 

```go
	for  _, value := range a { 
		f.Println("b 내용 : ", value)
	}
```

 배열을 복사하고 싶다면 다음과 같이 실행합니다. 

```go
package main

import f "fmt"

func main() {

	original := [...]string{"ios", "linux", "unix", "android", "windows"}
	clone := original // 배열 전체 복사

	original[3] = "ubuntu"

	f.Println(original)
	f.Println(clone)
}

```

 위의 코드에서 확인할 수 있듯이 배열을 복사하면서로 다른 메모리 영역에 복사하기 때문에 원본에 수정이 일어나도 복사한 배열에는 값이 변하지 않습니다. 이를 수정하고 싶다면 포인터를 통해서 레퍼런스 타입으로 변경하면 됩니다. 

### 슬라이스 사용하기 

 Go 언어의 슬라이스는 기본적으로 배열과 동일하지만 크기가 고정되어 있지 않고 동적으로 길이가 늘어난다는 특징을 가지고 있습니다. 만약 길이가 0인 빈 배열을 만들기 위해선 내장 함수 make를 사용하면 됩니다. 이때 생성되는 슬라이스를 nil 슬라이스라고 하며 null과 동일합니다. 

 기본적으로 Go 언어에서의 슬라이스는 레퍼런스 타입입니다. 다음 예제는 Go 언어에서 슬라이스를 선언하는 방법에 대해서 알아보도록 하겠습니다. 

```go
	var a []int // 슬라이스는 배열과 달리 [] 안에 길이를 지정하지 않습니다. 이렇게 생성된 슬라이스의 길이는 0입니다. 
	var a []int = make([]int, 5) // make 함수로 int형에 길이가 5인 슬라이스에 공간 할당
	var b = make([]string, 5) // 슬라이스를 선언할 때 자료형과 [] 생략
	c := make([]float64, 5, 10) // 슬라이스를 선언할 때 var 키워드, 자료형과 [] 생략
```

 슬라이스의 값을 초기화 하기 위해서는 {}를 반드시 사용해야 합니다. 한줄로 작성할 수도 있지만 여러줄로 초기화 할 수도 있습니다. 여기서 반드시 기억해야 할 점은 여러줄로 선언시 마지막 원소에서 콤마를 붙어야 한다는 점입니다. 

```go
	a := []int{70, 80, 90, 100, 110}

	b := []int{
		10,
		20,
		30,
		40,
		50,  // 여러 줄로 나열할 때는 마지막 요소에 콤마를 붙임
}
```

 슬라이스에서 길이는 익숙하지만 용량은 상대적으로 생소할 수 밖에 없습니다. 배열은 길이의 변경이 필요할 때마다 새로운 길이를 가진 배열을 다시 할당하는 비효율적인 작업을 진행해야 하지만 slice는 길이의 변경에 대비하여 미리 특정 용량을 가진 배열을 할당해두고, 정해진 길이 만큼만 사용할 수 있도록 하여, 길이의 수정 만으로 배열을 재할당할 필요 없이 유동적으로 사용할 수 있습니다. 

![](https://media.vlpt.us/post-images/kimmachinegun/57629370-cbb4-11e8-a4ed-41eb39296329/%EC%8A%AC%EB%9D%BC%EC%9D%B4%EC%8B%B11.png)

 위의 그림을 이해하기 위해서 다음 코드를 확인하시길 바랍니다. 참고로 미리 슬라이스의 용량을 크게 할당하면 요소가 추가될 때마다 메모리를 새로 할당하지 않아도 되므로 성능상 이점이 있으나 메모리 공간을 많이 차지하게 되고. 반대로 슬라이스 용량을 적게 할당하면 처음부터 메모리 공간은 적게 차지하지만, 요소가 추가될 때마다 메모리를 새로 할당하게 되므로 성능이 떨어질 수 있습니다.

슬라이스의 길이는 len 함수로 구할 수 있으며, 용량은 cap 함수로 구할 수 있습니다

```go
	c := make([]float64, 5, 10)
	os := []string{"ios", "linux", "unix", "android", "windows"}

	f.Println(len(os), cap(os))
	f.Println(len(c), cap(c))
```

 슬라이스에 값을 추가로 지정하고 싶다면 append 함수를 사용합니다. 

```go
	var a []int = make([]int, 5)

	for i := 0; i < len(a); i++ {
		a[i] = i
	}	

	add := append(a, 6, 7, 8)
	f.Println("size :", len(add), ", components :", add)
```

 만약에 슬라이스에 다른 슬라이스를 추가해야 한다면 append 함수에 ... 을 추가로 사용합니다. 다음을 통해 예시를 확인할 수 있습니다. 

```go 
	a := []int{1, 2, 3}
	b := []int{4, 5, 6}

	a = append(a, b...) 

	f.Println(a)
```

 만약 슬라이스에 할당된 용량이 길이보다 크더라도 길이를 벗어난 인덱스에는 접근할 경우 Panic 에러를 발생시킵니다.  슬라이스는 레퍼런스타입이라고 위에서 설명했습니다. 따라서 슬라이스를 복사하도라도 레퍼런스로 참조하기 때문에 복사본에 수정된 내용은 원본 내용에도 반영됩니다. 다음 코드를 통해 확인하시길 바랍니다. 

```go 
	originalArray := [...]int{1, 2, 3}
	var cloneArray [3]int

	cloneArray = originalArray
	cloneArray[0] = 4
	f.Println("배열 복사 : ", "original : ", originalArray, "clone : ", cloneArray)

	originalSlice := []int{1, 2, 3}
	var cloneSlice []int

	cloneSlice = originalSlice
	cloneSlice[0] = 4
	f.Println("슬라이스 복사 : ", "original : ", originalSlice, "clone : ", cloneSlice)
```

 슬라이스를 사용한다면 슬라이싱을 뺄 수 없습니다. Go에서는 배열과 slice의 특정 영역을 slice 형태로 추출할 수 있도록 slicing이라는 기능을 제공합니다. **변수명[시작인덱스(포함):끝인덱스(불포함)]** 을 반드시 기억합니다. 다음 예제를 통해 확인해보겠습니다.

```go
	f.Println(os[0:2])
	f.Println(os[1:3])
	f.Println(os[:3])
	f.Println(os[3:])
```

 이제까지 Go 언어에서 슬라이스 기능에 대해 살펴보았습니다. 얼핏 많은 내용이 나왔지만 다음 코드를 통해 지금까지 학습한 내용을 복습하고 값이 어떻게 표현되는지 확인하시길 바랍니다. 

```go
package main

import f "fmt"

func main() {

	var a []int = make([]int, 5)
	var b = make([]string, 5)
	c := make([]float64, 5, 10)

	f.Println(a, b, c)

	for i := 0; i < len(a); i++ {
		a[i] = i
	}
	f.Println(a)

	add := append(a, 6, 7, 8)
	f.Println("size :", len(add), ", components :", add)

	os := []string{"ios", "linux", "unix", "android", "windows"}
	f.Println(len(os), cap(os))
	f.Println(len(c), cap(c))

	f.Println(os[0:2])
	f.Println(os[1:3])
	f.Println(os[:3])
	f.Println(os[3:])

}

```

```go
package main

import f "fmt"

func main() {

	originalArray := [...]int{1, 2, 3}
	var cloneArray [3]int

	cloneArray = originalArray
	cloneArray[0] = 4
	f.Println("배열 복사 : ", "original : ", originalArray, "clone : ", cloneArray)

	originalSlice := []int{1, 2, 3}
	var cloneSlice []int

	cloneSlice = originalSlice
	cloneSlice[0] = 4
	f.Println("슬라이스 복사 : ", "original : ", originalSlice, "clone : ", cloneSlice)

	f.Println("-------------------------")

	a := []string{"windows", "linux", "android", "ios"}
	b := make([]string, 3)

	copy(b, a)
	f.Println(a)
	f.Println(b)

	b[0] = "ubuntu"
	f.Println(a)
	f.Println(b)

}

```



### 맵 사용하기

 맵(Map)은 키(Key)에 대응하는 값(Value)을 신속히 찾는 해시테이블(Hash table)을 구현한 자료구조입니다. 맵에 정의된 키는 중복되지 않아야 하며, Python언어의 딕셔너리와 매우 유사하지만, Nil map이 있다는 차이점을 가지고 있습니다. Nil map에는 어떤 데이터를 쓸 수 없는데, map을 초기화하기 위해 make()함수를 사용합니다.

```go
	var test map[int]string // map 선언, Nil map 생성
	test = make(map[int]string)
```

 맵을 선언하고 값을 저장하고 값을 출력하기 위해서 다음과 같이 사용할 수 있습니다.

```go
	a := make(map[string]int)

	a["age"] = 27
	a["height"] = 175
	f.Println(a)
	
	b := map[string]float64{
		"pi":    3.141592,
		"sqrt2": 1.41421356,
	}

	f.Println(b["pi"], b["sqrt2"])
```

 Go 언어에서 맵을 사용하는 방법은 다른 프로그래밍 언어와 크게 다르지 않습니다. 아래 코드는 map에 키와 값을 선언하고, 출력하며, 맵에 있는 값의 여부를 확인하거나, 순회하여 출력, 값을 삭제하는 예제 입니다.  

```go
package main

import f "fmt"

func main() {

	a := make(map[string]int)

	a["age"] = 27
	a["height"] = 175
	f.Println(a)

	b := map[string]float64{
		"pi":    3.141592,
		"sqrt2": 1.41421356,
	}

	f.Println(b["pi"], b["sqrt2"])

	f.Println("-------------------")

	capacityUnit := make(map[string]string)

	capacityUnit["1byte"] = "8 bit"
	capacityUnit["1MB"] = "1024 Byte"
	capacityUnit["1GB"] = "1024 MB"
	capacityUnit["1TB"] = "1024 GB"
	capacityUnit["1PB"] = "1024 TB"

	value, ok := capacityUnit["1YB"]
	f.Println(value, ok)

	if result, ok := capacityUnit["1GB"]; ok {
		f.Println(result, ok)
	}

	f.Println("-------------------")

	for key, result := range capacityUnit {
		f.Println(key, " : ", result)
	}

	f.Println("-------------------")

	for _, result := range capacityUnit {
		f.Println(result)
	}

	delete(capacityUnit, "1PB")
	f.Println(capacityUnit)
}

```

 추가적으로 map안에 map을 여러번 선언하여 사용할 수 있습니다. 다음 방법을 사용해 보세요. 

```go
package main

import f "fmt"

func main() {
  
	terrestrialPlanet := map[string]map[string]float32{
		"Mercury": map[string]float32{
			"meanRadius":    2439.7,
			"mass":          3.3022e+23,
			"orbitalPeriod": 87.969,
		},
		"Venus": map[string]float32{
			"meanRadius":    6051.8,
			"mass":          4.8676e+24,
			"orbitalPeriod": 224.70069,
		},
		"Earth": map[string]float32{
			"meanRadius":    6371.0,
			"mass":          5.97219e+24,
			"orbitalPeriod": 365.25641,
		},
		"Mars": map[string]float32{
			"meanRadius":    3389.5,
			"mass":          6.4185e+23,
			"orbitalPeriod": 686.9600,
		},
	}

	f.Println(terrestrialPlanet["Mars"]["mass"])
  
}

```



## 함수 

 함수(function)란 어떠한 입력을 통해서 어떠한 출력 매개변수로 맵핑하는 독립적인 코드 영역입니다. 

![](http://www.codingnuri.com/golang-book/assets/function.png)

 Go 언어에서는 main 함수가 기본이지만 필요에 따라서 여러 함수 선언을 통해서 다양한 역할을 담당하는 함수를 선언하여 사용할 수 있습니다. 이렇게 기능별 함수를 선언하면 해당 역할을 담당하는 함수를 재사용 할 수 있으며, 코드를 간결하게 작성할 수 있습니다. 다음 예제는 함수를 사용한 기본 예제입니다. 

```go
package main

import f "fmt"

func hello() {
  f.println("Hello")
}

func main() {
  hello()
  world()
}

func world() {
  f.println("world!")
}

```

 일부 언어의 경우 함수를 선언하기 위해서는 정의한 함수를 main 보다 위에 써주거나 함수가 존재한다는 정의를 해줘야 하지만 Go 언어는 함수를 정의할 때 위치 제약이 없습니다. 하지만 다음과 같이 코드를 작성하는 경우 에러가 발생합니다. 

```go
func hello() // 컴파일 에러가 발생함, 함수 선언의 위치는 상관이 없으나 {}의 위치는 반드시 지켜야함
{
  f.println("Hello")
}
```

 Go 언어에서 함수를 선언하고 매개변수를 넘기거나 결과값을 리턴받을 때는 다음과 같이 작성합니다. 

```go
func sum(a int, b int) int { // int형 변수 2개를 입력받고, int형 변수 하나를 반환할 때
	return a + b
}

func subtract(a int, b int) (r int) { // 리턴할 값을 지정할 때
	r = a - b
	return r
}
```

 Go 언어에서는 리턴값을 여러개 지정할 수 있습니다. 이를 사용하고자 할때는 다음과 같이 사용합니다. 

```go
func division(a int, b int) (int, int) { // 리턴값을 여러개 지정할 수 있음
	value := a / b
	remainder := a % b

	return value, remainder
}
```

 만약에 인자의 갯수의 상관없이 함수를 호출하고 싶다면 가변함수를 사용하면 됩니다. 가변함수를 통해 인자값을 넘기는 경우에는  ... 을 사용합니다. 다음 예제를 통해 살펴보도록 하겠습니다. 

```go
func multiplication(n ...int) int {

	calculation := 2
	for _, value := range n {
		calculation *= value
	}

	return calculation
}
```

 지금까지 Go 언어에서의 함수의 특징을 살펴보았습니다. 다음 예제를 통해 함수 사용법을 정리합니다. 

```go
package main

import f "fmt"

func main() {

	a := 10
	b := 20

	result := sum(a, b)
	f.Println(result)

	f.Println(subtract(30, 20))

	f.Println(division(4, 3))
	_, remainder := division(456, 25)
	f.Println("나머지 : ", remainder)

}

func sum(a int, b int) int {
	return a + b
}

func subtract(a int, b int) (r int) {
	r = a - b
	return r
}

func division(a int, b int) (int, int) {
	value := a / b
	remainder := a % b

	return value, remainder
}

```

```go
package main

import f "fmt"

func main() {

	f.Println("가변 함수")

	result := multiplication(2, 3, 4, 5)
	f.Println(result)

	slice := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
	f.Println(sliceSum(slice...))

	r := func(a int, b int) int {
		return a + b
	}(1, 3)
	f.Println(r)

}

func multiplication(n ...int) int {

	calculation := 2
	for _, value := range n {
		calculation *= value
	}

	return calculation
}

func sliceSum(n ...int) int {

	result := 0
	for _, value := range n {
		result += value
	}

	return result
}

```



## 클로저 / 재귀 / 지연호출 

### 클로저 사용하기 

 Go 언어에서의 클로저란 익명 함수(Anonymous functions)와 비슷한 개념으로 이름 없이 한줄로 함수를 정의하고 싶을때 유용합니다. 이를 통해 함수 안에서 함수를 선언 및 정의할 수 있고, 바깥쪽 함수에 선언된 변수에도 접근할 수도 있습니다. 

 아래 예제를 통해 클로저 사용 예제를 확인하시길 바랍니다. 

```go
package main
import f "fmt"

func main() {

	f.Println("클로저")

	sum := func(a int, b int) int {
		return a + b
	}
	result := sum(1, 2)
	f.Println(result)

	f.Println("-----------------")

	cal := voltage()
	f.Println(cal(2, 4))
	f.Println(cal(5, 2))
	f.Println(cal(10, 5))

}

func voltage() func(i int, r int) int {

	return func(i int, r int) int {
		return i * r
	}

}

```

 위의 예제를 살펴보면 클로저를 통해 함수를 호출하고 이를 통해 값을 반환합니다. 하지만 위의 예제만 가지고는 왜 클로저를 사용하는지 알 수 없습니다. 복잡해 보이는 클로저를 사용하는 이유는 다음과 같습니다. 

 지역변수는 함수 호출이 끝나면 소멸됩니다. 하지만 클로저를 사용하면, 지역변수는 소멸되지 않고 호출할 때마다 지속적으로 사용할 수 있기 때문에 함수를 선언했을때의 환경을 유지할 수 있습니다. 이를 통해 프로그래밍의 흐름을 제어할 수 있으므로, 함수형 프로그래밍 언어의 큰 장점이라고 할 수 있습니다. 

 다음 예제를 통해 확인해보세요. 

```go
package main
import f "fmt"

func main() {
  
	nextInt := intSeq()
	f.Println(nextInt())
	f.Println(nextInt())
	f.Println(nextInt())

	newInts := intSeq()
	f.Println(newInts())
  
}

func intSeq() func() int {
  
	i := 0
	return func() int {
		i += 1
		return i
	}
  
}

```



### 재귀 사용하기 

 함수가 계속해서 자기 자신을 호출해서 사용하는 것을 재귀함수하고 합니다. 재귀함수 자체는 호출이 지속적으로 반복적으로 이루어지기 때문에 성능이 떨어진다는 단점이 있지만 **알고리즘 자체가 재귀적으로 표현하기 자연스러울 때** 혹은 **변수 사용을 줄여줄때** 많이 사용합니다. 따라서 재귀 함수를 사용할 때는 이득과 이로 인해 발생하는 성능하락을 고려하여 사용하는 것이 좋습니다. 아래는 재귀함수를 이용한 간단한 예제입니다. 

```go
package main
import f "fmt"

func main() {

	f.Println("재귀 함수")
	f.Println(fact(5))
  
}

func fact(n int) int {
  
	if n == 0 {
		return 1
	}
	return n * fact(n-1)
  
}

```



### 지연호출이란?

 Go 언어에서 지연호출이란 현재 함수가 끝나기 직전에 실행되는 것을 뜻합니다. 다음 예제를 실행해 보면 쉽게 감을 잡을 수 있습니다. 

```go
package main 
import f "fmt"

func hello(a int){ 
  f.Println("Go " , a) 
} 

func main(){ 
  
  defer hello(4)
  hello(1) 
  hello(2) 
  hello(3) 
  
}

```

```go
package main 
import f "fmt"

func main(){ 
  
	for i:=0; i<5; i++{
  	defer f.Printf("%d ", i) 
  } 
  
}

```

 지연 호출을 사용하면 추후 파일을 읽고 쓰는 작업을 진행할 때, 파일 스트림을 나중에 닫아야 하는 여러 상황에서 유용하게 사용될 수 있기 때문에 흐름에 따른 분기 실행에 익숙해질 필요가 있습니다. 다음 예제는 지연호출을 사용하여 다양한 어떻게 동작하는지 확인해보도록 하겠습니다. 코드를 먼저 실행하기 전에 어떠한 형식으로 값이 출력될지 확인해 보시길 바랍니다. 

```go 
package main
import f "fmt"

func main() {

	f.Println("지연 호출")
	name()

	array := []int{1, 2, 3, 4, 5}

	for _, value := range array {
		defer f.Println(value)
	}
}

func name() {
	defer func() {
		f.Println("lee")
	}()

	func() {
		f.Println("seongwon")
	}()
}

```



## 포인터 

 Go 언어에서도 C / C++과 같이 포인터를 지원합니다. 포인터는 프로그래밍 언어에서 다른 변수, 혹은 그 변수의 메모리 공간주소를 가리키는 변수를 말한다. 포인터가 가리키는 값을 가져오는 것을 역참조(dereference)라고 합니다. Go 언어에서 포인터를 생성하는 방법은 다음과 같습니다. 

![](https://upload.wikimedia.org/wikipedia/commons/thumb/2/2f/Exam_int_point.png/370px-Exam_int_point.png)

 JAVA나 Python 등과 같은 현대의 언어들은 대부분 포인터를 사용하지 않습니다. 정확하게 말하면 내부적인 로직을 통해 참조를 통해서 사용가능하게 하는데, 이는 휴먼에러를 방지하기 위함도 있지만, 다양한 자료형 변수들을 효율적으로 처리하기 어렵다는 단점이 있습니다. 

 Go 언어는 포인터의 장점과 단점을 절충하여 연산이나 캐스팅을 막아 간단하게 접근할 수 있습니다. 또한 Go언어에서는 외부로 레퍼런싱 되는 객체의 경우 힙에 할당해 주며 자동으로 관리합니다. 

```go
	var numPtr *int
```

 우리가 익숙한 방법으로 포인터를 사용하는 것을 확인할 수 있습니다. 위에서 선언한 포인터는 참조값을 가지고 있지 않기 때문에 nil 포인터가 됩니다. 이렇게 선언된 포인터는 사용할 수 없기 때문에 new를 통해 포인터를 초기화해야 합니다. 

```go 
	var numPtr2 *int = new(int)
```

 다음예제를 통해 포인터를 사용했을 때 결과가 어떻게 나오는지 확인해보세요. 

```go
package main

import f "fmt"

func main() {

	f.Println("포인터")

	var numPtr *int // nil 포인터 생성
	f.Println(numPtr)
	var numPtr2 *int = new(int) // 포인터 초기화
	f.Println(numPtr2)

	i := 1
	f.Println("init : ", i)
	number(i)
	f.Println("value", i)
	pointerNumber(&i) // int형 변수 i의 주소값을 pointerNumber 함수로 전달
	f.Println("pointer : ", i) 
	f.Println("address : ", &i) // int형 i가 참조하는 주소값 출력
}

func number(a int) {
	a = 0
}

func pointerNumber(a *int) {
	*a = 0 // 해당 주소를 참조하는 변수의 값을 변경
}

```

 뒤에서 설명하겠지만 Go 언어에서는 클래스 개념이 없고 구조체를 대신 사용합니다. 따라서 구조체를 통한 프로그래밍할 때 포인터를 사용하면 쉽게 인자값을 넘기고 작성할 수 있습니다.  Go  언어에서 포인터를 사용하는 좋은 방법에 대해서는 다음 링크를 확인해보시길 바랍니다. 

> https://stackoverflow.com/questions/23542989/pointers-vs-values-in-parameters-and-return-values



## 구조체 / 메서드 / 인터페이스 

### 구조체 

 Go의 structs는 필드들로 이루어진 타입을 갖는 컬렉션입니다. 레코드를 구성하기 위해 데이터들을 그룹핑 하는데 유용합니다. 다음 예제를 통해 구조체 선언을 확인해 보세요.  

```go
type person struct {
	name string
	age  int
}
```

  위 person 구조체 타입은 name과 age라는 필드를 가지고 있습니다. 같은 자료형을 가지고 있다면 다음과 같이 작성할 수 도 있습니다. 

```go
type person struct {
	phone, age int
}
```

 구조체를 선언하고 필드명을 초기화 할 수 있으며, 생략된 필드는 0을 가지게 됩니다. 기본적으로 . 을 사용해서 구조체 필드에 접근하여 사용하며, &를 사용하면 구조체 포인터를 사용할 수 있습니다. 다음 예제를 통해 구조체 연습을 진행하도록 하겠습니다. 

```go
package main

import f "fmt"

type person struct {
	name string
	age  int
}

func main() {

	f.Println("구조체")

	f.Println(person{"lucas", 27})
	f.Println(person{name: "bob"})
	f.Println(person{age: 19})

	s := person{name: "kan", age: 30}
	f.Println(s.name, s.age)

	f.Println(&s)

}

```

```go 
package main

import f "fmt"

type subject struct {
	operatingSystem float32
	database        float32
	network         float32
	java            float32
}

func main() {

	student := subject{operatingSystem: 50, database: 30, network: 60, java: 50}
	f.Println(average(&student))

}

func average(sub *subject) float32 {

	result := (sub.operatingSystem + sub.database + sub.network + sub.java) / 4
	return result

}

```



### 메서드 

 우리가 알고 있는 메서드는 입력값이 있고, 그 입력값을 받아서 무언가 한 다음 결과를 도출해 내는 수학의 함수와 비슷한 개념입니다. 이때 입력값을 매개변수라고 하고, 결과값을 리턴값이며, 인자( Argument )는 어떤 함수를 호출시에 전달되는 값을 의미합니다. 하지만  Go 언어에서는 클래스가 존재하지 않기 때문에 구조체만 필드값을 가지고 메서드는 별도로 분리되서 관리합니다.

 다음 예제를 통해서 메서드 개념을 확인하세요. 

```go
package main

import f "fmt"

type rect struct {  // 구조체 선언
	width, height int
}

func main() {

	f.Println("메서드")

	r := rect{width: 20, height: 5}
	f.Println("area : ", r.area())
	f.Println("perim : ", r.perim())

	rp := &r
	rp.height = 10
	rp.width = 5

	f.Println("area : ", rp.area()) // 메서드 호출
	f.Println("perim : ", rp.perim())

}

// 메서드 사용
func (r *rect) area() int { // 구조체 지정
	return r.width * r.height
}

func (r rect) perim() int {
	return 2*r.width + 2*r.height
}

```

구조체에 정의된 두 개의 메서드를 호출했습니다. Go는 메서드 호출에 대해 값과 포인터간의 변환을 자동으로 처리합니다. 메서드 호출시 값 복사를 피하기 위해 포인터 리시버 타입을 사용할 수도 있고 전달되는 구조체를 변경할 수 있도록 할 수도 있습니다.



### 인터페이스 

 일반적으로 JAVA에서 사용하는 인터페이스는 클래스들이 구현해야 하는 동작을 지정하는데 사용되는 추상 자료형입니다. Go 언어에서는 인터페이스 타입(type)이 구현해야 하는 메서드 원형(prototype)들을 정의합니다. 즉, 하나의 사용자 정의 타입이 인터페이스를 구현하기 위해서는 단순히 그 인터페이스가 갖는 모든 메서드들을 구현하면 됩니다. 

 아래는 간단한 인터페이스를 생성하는 예제입니다. 

```go
package main

import f "fmt"

type test interface { // 인터페이스 정의
}

func main() {

	var i test // 인터페이스 선언
	f.Println(i)

}

```

 현재는 아무것도 없는 인터페이스를 생성하고 호출했기 때문에 nil 값이 호출됩니다. 인터페이스를 사용할 때는 변수를 선언하는 방식과 동일하며,  다른 자료형과 동일하게 함수의 매개변수, 리턴값, 구조체의 필드로 사용할 수 있습니다.

 다음 예제를 통해 구조체와 메서드, 인터페이스를 확인해 보세요. 

```go
package main

import (
	f "fmt"
	"math"
)

type geometry interface {
	area() float64
	perim() float64
}

type rect struct {
	width, height float64
}

type circle struct {
	radius float64
}

func main() {

	f.Println("인터페이스")

	r := rect{width: 3, height: 4}
	c := circle{radius: 5}
	measure(r)
	measure(c)

}

func (r rect) area() float64 {
	return r.width * r.height
}

func (r rect) perim() float64 {
	return 2*r.width + 2*r.height
}

func (c circle) area() float64 {
	return math.Pi * c.radius * c.radius
}

func (c circle) perim() float64 {
	return 2 * math.Pi * c.radius
}

func measure(g geometry) {
	f.Println(g)
	f.Println(g.area())
	f.Println(g.perim())
}

```



## 고루틴 / 채널 / 동기화 / 셀렉트 

### 고루틴

 고루틴 (goroutine)은 가벼운 스레드와 같은 것으로 수행 흐름과 별개로 병렬처리가 가능하게 합니다. OS에서 스케줄링으로 관리되는 스레드(약 1MB)보다 가볍기 때문에 (약 8kbyte) 자신의 코어갯수보다 많이 실행해도 무리 없이 동작한다는 장점이 있습니다. 고루틴은 고 런타임이 관리하고 고채널을 통해 고루틴간의 통신을 할 수 있습니다. 

 Go 언어에서 고루틴을 실행하는 방법은 아래와 같이 매우 간단합니다. 

````go
	go Hello() // 일반함수를 통한 고루틴 실행
````

 ```go
	for i := 0; i < 3; i++ {
    go func(n int) { // 익명함수(클로저)를 통한 고루틴 실행
			f.Println("goroutine : ", n)
			oneTime.Do(Hello)
		}(i)
	}
 ```

 고루틴을 사용하고자 하는 곳에 go 키워드를 사용함으로서 사용할 수 있으며, 이때 현재 함수의 실행 흐름과는 논리적으로 분리되어 동작합니다. 본격적으로  고루틴의 사용법을 익히기 전에 고루틴이 어떠한 방식에 기초하여 동작하는지에 대해 알고 넘어갈 필요가 있습니다. 

 OS에서 여러 프로그래밍이 동작할 때 동시성 혹은 병렬성으로 동작합니다. 흔히 동시성이라고 하면 병렬성과 비슷한 개념이라고 생각할 수 있지만 이는 명확히 다른 개념입니다. 동시성은 싱글 코어에서 멀티 스레드를  동작시키는 논리적인 개념으로 한번에 여러개가 동시에 실행되는 것 처럼 보이게 됩니다. 하지만 병렬성은 물리적으로 동시에 여러작업을 처리할 수 있기 때문에 멀티 코어에서 멀티 스레드를 동작시키는 방식입니다. 

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FCxC94%2FbtqwVThGa7H%2F96yyfojioFOPKZPjKMNHq0%2Fimg.png)

  Go 1.5 버전 이전에는 CPU 코어 하나만 사용하도록 기본 설정되어 있기 때문에 여러개의 고루틴을 만들더라도 동시성으로 동작하였으나 현재는 물리 CPU 개수만큼 사용하도록 설정되어 각 코어에서 시분할 ( Concurrent ) 처리로 동작합니다. 만약 여러 CPU를 병렬로 실행하고자 하는 코드를 작성하고자 하면 **runtime.GOMAXPROCS(runtime.NumCPU())** 함수를 호출하여 시스템의 모든 코어를 사용하도록 설정할 수 있습니다. 

 다음 예제는 간단하게 고루틴 활용한 예제입니다. 

```go
package main

import f "fmt"

func main() {

	f.Println("goroutine 경량 실행 스레드")

	go Hello()
	f.Scanln()
}

func Hello() {

	f.Println("Hello World")

}

```



### 채널 

![](https://media.geeksforgeeks.org/wp-content/uploads/20190731140438/Untitled-Diagram46.jpg)

 

Go 언어에서 채널이란 고루틴끼리 데이터를 주고받는 통로(파이프)의 역할을 수행합니다. make(chan 자료형)을 통해 함수를 생성해야 하며, 채널 연산자 <-를 사용한다는 특징을 가지고 있습니다. 채널은 어떠한 데이터 타입도 주고 받을 수 있고 실행의 흐름을 동기화 할 수 있습니다. 

 Go 언어에서 채널을 생성하는 방법은 2가지가 존재하는데 하나는 일반 함수를 사용하는 방법이고, 다른 하나는 익명함수를 사용하는 방법입니다. 다음 예제를 통해서 간단한 채널을 만들고 실행해 보겠습니다. 

```go
package main

import f "fmt"

func main() {

	msg := make(chan string)

	go func() { msg <- "ping" }()

	result := <-msg
	f.Println(result)

}

```

 고루틴으로 생성된 string 타입의 채널은 result 라는 변수에 저장되게 되는 구조로, 기본적으로 송신과 수신은 송신자와 수신자가 준비될 때까지 블로킹됩니다. 이 특징은 다른 동기화 작업을 하지 않고도 ping 메시지를 수신받습니다. 

 다음은 일반함수로 생성된 고루틴을 함수로 통해 채널을 전달받아 계산하는 예제입니다. 

```go
package main

import f "fmt"

func main() {

	f.Println("Channels")

	c := make(chan int)
	go sum(10, 20, c)

	result := <-c

	f.Println(result)

}

func sum(a int, b int, c chan int) {

	c <- a + b

}

```

 sum 함수가 고루틴으로 생성된 후, 매개변수로 채널을 넘겼습니다. 이때 살펴봐야 할 점은 sum 함수에서 c라는 변수의 리턴값이 존재하지 않는다는 것인데, 이는 채널이 레퍼런스 타입이기 때문입니다. 

 Go 언어 채널에는 Unbuffered Channel과 Buffered Channel 2가지 채널이 존재합니다. Unbuffered Channel은 위에서 살펴본 것과 같이 하나의 하나의 수신자가 송신자로 부터 데이터를 받을 때까지 블록킹하지만,  Buffered Channel은 수신자가 데이터를 받을 준비가 되어 있더라도 송신자의 지정된 버퍼만큼 데이터를 보내고 다른일을 수행할 수 있도록 합니다. 다음 예제를 통해서 살펴보록 하겠습니다. 

![](https://res.cloudinary.com/practicaldev/image/fetch/s--QsKam5l5--/c_imagga_scale,f_auto,fl_progressive,h_420,q_auto,w_1000/https://thepracticaldev.s3.amazonaws.com/i/fxntn4jz05iyikdfru6d.jpeg)

```go 
package main

import f "fmt"

func main() {

	f.Println("beffered")

	messages := make(chan string, 2) // string 타입의 채널을 만들고 해당 채널의 버퍼는 2이다. 
	messages <- "wisoft"
	messages <- "lab"

	f.Println(<-messages)
	f.Println(<-messages)
  
}

```

 위의 예제에서는 버퍼의 크기가 2이지만 이를 초과하는 데이터를 채널에 넣게 되면 어떠한 일이 발생하는지 확인해 보도록 하겠습니다. 위 코드에서 message 채널에 string 변수를 넣고 이를 출력해 보겠습니다. 

```go
	messages <- "golang"
	f.Println(<-messages)
```

 버퍼의 크기가 2인 messages 변수에 string 변수가 하나 더 들어갔기 때문에 버퍼의 크기는 초과하게 되고 결과적으로 다음과 같은 에러가 발생하게 됩니다. 

>fatal error: all goroutines are asleep - deadlock!

 따라서 버퍼의 크기를 정하게 되면, 해당 채널에 할당된 버퍼의 크기가 넘지 않도록 주의해야 합니다. 

 다음은 고루틴간의 채널을 동기화 하는 방법을 알아보도록 하겠습니다. 이전 시간에서 고루틴을 사용하고 나서, 프로그램의 종료는 막기 위해서 **f.Scanln()**을 사용했는데 이는 main 함수와 고루틴이 동시에 실행되어 바로 종료과 되는 상황을 막기 위함이었습니다. 이러한 문제를 해결하기 위해서 고루틴이 끝날때까지 대기하기 위해 블로킹 리시브를 사용하는 방법을 다음 예제를 통해 알아보겠습니다. 

```go
package main

import (
	f "fmt"
	"time"
)

func main() {

	f.Println("synchronization")

	done := make(chan bool, 1)
	go worker(done)

	<-done
}

func worker(done chan bool) {

	f.Println("working...")
	time.Sleep(time.Second)
	f.Println("done")

	done <- true
}

```

 worker 함수는 done 변수의 bool 채널을 매개변수로 입력받고 해당 함수가 호출시 랜덤한 시간만큼 수행 후에 true 값을 전달하여 함수의 작업이 끝났음을 전달합니다. 따라서 채널은 worker 함수로부터 알림을 받을 때까지 블로킹합니다. 

 채널은 고루틴끼리 통신하기 위한 양방향 통로(파이프)이지만 채널을 함수의 인자로 사용할 때는 채널이 수신용인지 송신용인지를 정해서 타입의 안전성을 향상시킬 수 있습니다. 다음 예제를 실행해 보도록 하겠습니다. 

```go
package main

import f "fmt"

func main() {

	f.Println("Channel direction")

	pings := make(chan string, 1)
	pongs := make(chan string, 1)

	ping(pings, "passed message")
	pong(pings, pongs)

	f.Println(<-pongs)

}

func ping(pings chan<- string, msg string) { // 송신 전용 채널

	pings <- msg

}

func pong(pings <-chan string, pongs chan<- string) { // 수신 전용 채널

	msg := <-pings
	pongs <- msg

}

```

 ping 함수는 송신용 채널만을 받습니다. 이 채널에 수신용 채널을 넣으면 컴파일시 에러가 발생합니다. pong 함수는 수신용 채널인 (pings)를 하나 받고 송신용인 (pongs)를 두 번째 인자로 받습니다. 

### 동기화 

 OS를 기본적으로 공부했던 개발자라면 동기화가 얼마나 중요한 개념인지 다들 알고 계실 것입니다. 다중 처리기, 다중 코어에서 어떠한 작업을 분할에서 작업할 시, 해당 작업이 이전 작업 혹은 다른 작업의 같은 메모리 영역을 건들일 경우 예기치 않은 동작이나 치명적인 에러를 나타낼 수 있습니다. 

![](https://miro.medium.com/max/700/1*jUgonhLolQEA7S0wXvJ9Ww.png)

 OS 내부적으로 원자적 연산을 보장하기 위해 다양한 기법을 사용하지만, 응용 프로그래머 입장에서도 멀티코어를 잘 처리하기 위해서는 코드안에서의 동기화 처리가 매우 중요합니다. 다행인 점이라면 Go 언어에서는 동기화 처리를 위한 문법이 상당히 간결하면서도 강력하며, 쉽게 멀티 코어를 지원하는 프로그래밍을 작성할 수 있다는 점입니다. Go 언어에서는 Mutex를 사용합니다. 

>[컴퓨터 과학](https://ko.wikipedia.org/wiki/컴퓨터_과학)에서 **락**(lock) 또는 **뮤텍스**(mutex, [상호 배제](https://ko.wikipedia.org/wiki/상호_배제)에서)는 여러 [스레드를 실행](https://ko.wikipedia.org/wiki/스레드_(컴퓨팅))하는 환경에서 자원에 대한 접근에 제한을 강제하기 위한 [동기화](https://ko.wikipedia.org/wiki/동기화) 매커니즘이다. 락은 [상호 배제](https://ko.wikipedia.org/wiki/상호_배제) [동시성 제어](https://ko.wikipedia.org/w/index.php?title=동시성_제어&action=edit&redlink=1) 정책을 강제하기 위해 설계된다. 

  Go 언어에서 뮤텍스를 선언하고 해제하는 방법은 다음과 같습니다. 

```go
mutex.Lock() 
mutex.Unlock()
```

  읽기/쓰기가 동시에 일어나는 작업의 경우 다음과 같이 뮤텍스를 선언하고 해제합니다. 

```go
rwMutex.Lock()
rwMutex.RUnlock()
```

 고루틴에서 뮤텍스를 사용하고 다른 고루틴에게 CPU를 양보하기 위해서는 다음을 반드시 사용해야 합니다. 

```go
runtime.Gosched()
```

 다음 예제를 통해 뮤텍스를 사용하지 않았을 때 발생하는 고루틴간의 경쟁사항을 알아보도록 하겠습니다. 

```go
runtime.GOMAXPROCS(runtime.NumCPU()) // 모든 CPU 사용
fmt.Println(runtime.NumCPU()) // 본인 PC가 사용하고 있는 코어 및 HT(intel) or SMT(AMD) 갯수

	var data = []int{} // int형 슬라이스 생성

	go func() {                             
		for i := 0; i < 1000; i++ {     
			data = append(data, 1)  

			runtime.Gosched()       
		}
	}()

	go func() {                             
		for i := 0; i < 1000; i++ {     
			data = append(data, 1)  

			runtime.Gosched()       
		}
	}()

	time.Sleep(2 * time.Second)      
	fmt.Println(len(data))           
```

 위의 예제를 살펴보면 int형 슬라이스에서 2개의 고루틴이 1000번 반복하며 슬라이스에 append 하는 것을 확인할 수 있습니다. 우리가 예상하는 data의 길이는 2000이 되어야 하지만 실질적으로 실행하면 2000이 나오지 않습니다. 이는 두개의 고루틴이 하나의 data에 접근하여 경쟁사항을 만들었기 때문이며 이를 경쟁 조건(Race condition)이라고 말합니다. 위와 같은 문제를 해결하기 위해서 우리는 뮤텍스를 사용하여 경쟁 조건이 발생하지 않도록 조치해야 합니다. 

```go
go func() {                             
		for i := 0; i < 1000; i++ {     
			mutex.Lock()            
			data = append(data, 1)  
			mutex.Unlock()         

			runtime.Gosched()       
		}
	}()

	go func() {                             
		for i := 0; i < 1000; i++ {     
			mutex.Lock()            
			data = append(data, 1)  
			mutex.Unlock()          

			runtime.Gosched()
		}
	}()
```

 이전 코드에서 달라진 점은 뮤텍스의 사용유무라는 것을 알 수 있습니다. 이를 통해 경쟁 조건을 만족하여 데이터의 길이는 정확히 2000이 나오는 것을 확인할 수 있을 것입니다. 하지만 여기서 주의해야 할 점은 뮤텍스에서 Lock을 걸고 나면 반드시 Unlock을 해야 한다는 점입니다. 이를 주의하지 않고 뮤텍스를 사용할 경우 교착상태 (deadlock)이 발생하여 코드가 멈추게 됩니다. 멀티 스레드 사황에서는 교착상태가 얼마든지 나타날 수 있으며, 한번 발생하게 되면 문제점을 파악하는데 많은 시간이 소요될 수 있습니다. 

 멀티스레드를 활용한 프로그래밍 작성이 처음이라면 이러한 문제가 발생하지 않도록 반드시 주의하면서 작성해야 합니다. 다음은 뮤텍스 사용을 좀더 고도화 하여 읽기/쓰기 작업을 진행할 때의 경쟁사항과 이를 해결하는 코드 예제를 살펴보도록 하겠습니다. 

```go
package main

import (
	f "fmt"
	"runtime"
	"sync"
	"time"
)

func main() {

	runtime.GOMAXPROCS(runtime.NumCPU())
	Condition()
	f.Println("-------------------")
	Mutex()

}

func Condition() {

	data := 0

	go func() {
		for i := 0; i < 3; i++ {
			data += 1
			f.Println("write : ", data)
			time.Sleep(10 * time.Millisecond)
		}
	}()

	go func() {
		for i := 0; i < 3; i++ {
			f.Println("read : ", data)
			time.Sleep(1 * time.Second)
		}
	}()

	go func() {
		for i := 0; i < 3; i++ {
			f.Println("read2 : ", data)
			time.Sleep(2 * time.Second)
		}
	}()

	time.Sleep(10 * time.Second)

}

func Mutex() {

	data := 0
	rwMutex := new(sync.RWMutex)

	go func() {
		for i := 0; i < 3; i++ {
			rwMutex.Lock()
			data += 1
			f.Println("write : ", data)
			time.Sleep(10 * time.Millisecond)
			rwMutex.Unlock()
		}
	}()

	go func() {
		for i := 0; i < 3; i++ {
			rwMutex.Lock()
			f.Println("read : ", data)
			time.Sleep(1 * time.Second)
			rwMutex.Unlock()
		}
	}()

	go func() {
		for i := 0; i < 3; i++ {
			rwMutex.Lock()
			f.Println("read2 : ", data)
			time.Sleep(2 * time.Second)
			rwMutex.Unlock()
		}
	}()

	time.Sleep(10 * time.Second)

}

```

 위의 코드를 살펴보면 data라는 int형 공통 변수 안에서 반복문을 통해 읽기와 쓰기 작업이 고루틴으로 동시에 작업하는 것을 확인할 수 있습니다. 경쟁상황에서는 재대로 값이 출력되지 않는 문제가 발생하는데 OS에서는 이를 **Readers-Writers Problem**라고 합니다. 이러한 문제를 해결하기 위해 Go 언어에서 제공하는 rwMutex를 통해서 경쟁조건을 막아주고 쓰기 작업 중 읽기 작업이 동시에 일어나지 않도록 조치해야 합니다. 

 복잡한 반복문과 고루틴이 동작하고 있는 상황에서 초기화를 위해 딱 한번만 실행을 해야 하는 경우 sync.Once를 사용하고 Do를 사용합니다. 다음 예제를 통해 살펴보도록 하겠습니다. 

```go
package main

import (
    "fmt"
    "sync"
)

var doOnce sync.Once

func main() {
    DoSomething()
    DoSomething()
    DoSomething()
}

func DoSomething() {
    doOnce.Do(func() {
        fmt.Println("Run once - first time, loading...")
    })
    fmt.Println("Run this every time")
}

```

 

### 셀렉트 

 채널을 통해 다중 연산을 하는 경우 이를 손쉽게 사용할 수 있는 셀렉트라는 분기문을 제공합니다. 이를 통해 원하는 채널에 값이 들어왔을 때만 해당 분기문을 실행하기 때문에 다중 채널을 통한 연산시 동기화 연산으로 확장시킬 수 있습니다. 

 다음 예제를 통해 기본 동작을 확인해 보도록 하겠습니다. 

```go
package main

import (
	f "fmt"
	"time"
)

func main() {

	f.Println("select")

	c1 := make(chan string)
	c2 := make(chan string)

	go func() { // 익명함수를 통한 고루틴 생성
		time.Sleep(time.Second * 1) // 1초간 대기
		c1 <- "one" // c1 채널에 string 값 one을 전송 
	}()

	go func() {
		time.Sleep(time.Second * 2)
		c2 <- "two"
	}()

	for i := 0; i < 2; i++ {
		select { // 해당 select 분기문 안에 있는 채널에 값이 들어왔을 때 case 내용을 실행한다. 
		case msg1 := <-c1:
			f.Println("received", msg1)
		case msg2 := <-c2:
			f.Println("received", msg2)
		}
	}

}

```

 select 분기문도 default 케이스를 지정할 수 있으며 case에 지정된 채널에 값이 들어오지 않았을 때 즉시 실행됩니다. 단, default에 적절한 처리를 하지 않으면 CPU 코어를 모두 점유하므로 주의해야 합니다. 



## 파일 입출력 / 정규 표현식 / json

### 파일 입출력 

 거의 모든 프로그래밍 언어에서 데이터를 읽기/쓰기 작업 기능이 필요합니다. Go 언어 역시 이러한 기능을 제공하며, 다음 예제를 통해서 확인해 보겠습니다. 

```go
package main

import (
	f "fmt"
	"io/ioutil"
	"os"
)

func main() {

	CurrentDirectory()
	data, err := ioutil.ReadFile("hello.txt")
	check(err)
	f.Print(string(data))
}

func check(e error) {
	if e != nil {
		f.Println("파일을 읽을 수 없습니다.")
		f.Println("Error Code : ", e)
	}
}

func CurrentDirectory() {
	path, _ := os.Getwd()
	f.Println(path)
}

```

 CurrentDirectory 함수에서는 현재 디렉터리에 위치를 알려주고 check 함수를 통해 파일의 존재 여부에 대한 기본적인 에러처리를 담당합니다. 이러한 조건이 충족되면 ioutil.ReadFile 패키지를 통해서 "hello.txt" 파일을 읽어서 string으로 출력합니다. 

 다음 예제를 통해서 Go 언어의 OS 패키지에서 제공하는 파일 작성하는 다른 방법을 살펴보겠습니다. 

```go
package main

import (
	"bufio"
	f "fmt"
	"os"
)

func main() {

	file, err := os.OpenFile(
		"hello.txt",
		os.O_CREATE|os.O_RDWR|os.O_TRUNC, // 파일이 존재하지 않으면 생성하고, 읽기/쓰기 모드로 파일을 만들고 / 파일이 존재하면 해당 내용을 삭제합니다. 
		os.FileMode(0644)) // 파일 권한 설정

	if err != nil {
		f.Println(err)
		return
	}
	defer file.Close() // main 함수가 끝나기 전에 파일을 닫습니다. 

	w := bufio.NewWriter(file)
	w.WriteString("hello, wisoft")
	w.Flush()

	r := bufio.NewReader(file)
	fi, _ := file.Stat()
	b := make([]byte, fi.Size())

	file.Seek(0, os.SEEK_SET)
	r.Read(b)

	f.Println(string(b))
}

```

 이번에는 bufio 패키지를 사용해서 파일 읽기 / 쓰기를 구현해 보도록 하겠습니다. 

```go
package main

import (
	"bufio"
	f "fmt"
	"os"
)

func main() {

	file, err := os.OpenFile(
		"hello2.txt",
		os.O_CREATE|os.O_RDWR|os.O_TRUNC,
		os.FileMode(0644))

	if err != nil {
		f.Println(err)
		return
	}
	defer file.Close()

	r := bufio.NewReader(file)
	w := bufio.NewWriter(file)

	rw := bufio.NewReadWriter(r, w)

	rw.WriteString("Hello, World")
	rw.Flush()

	file.Seek(0, os.SEEK_SET)
	data, _, _ := rw.ReadLine()
	f.Println(string(data))

}

```

 Go 언어에서는 다양한 패키지를 통해서 파일 읽기 / 쓰기를 구현할 수 있습니다. 위에서 작성한 것과 같이 똑같은 일을 처리하지만 다른 패키지를 사용함에 따라서 그 특성이 조금식 달라지게 되는데 어떠한 부분이 달라지는지 확인해 보도록 하겠습니다. 

> 다음 설명은 https://xoit.tistory.com/3의 내용을 차용했음을 알립니다. 

* package "os"
  * os는 low level에서 운영체제 기능에 대한 플랫폼과는 독립적인 인터페이스를 제공하는 패키지이다. Unix와 비슷한 디자인이지만, 에러 처리는 Go와 비슷하다. os.File 타입은 디스크 위에 파일이나 바이트를 스트리밍하는 io.Reader, io.Writer를 구현할 때 사용된다.
* package "io"
  * io는 I/O primitive(가장 기본 단위)의 기본 인터페이스를 제공하는 패키지이다. 바이트 스트림(io.Reader, io.Writer, etc..)을 처리하는 인터페이스 뿐만 아니라 io.Copy(파일 복사), io.ReadFull(n byte 읽기)와 같은 함수를 제공한다.
* package "io/util"
  * io/ioutil는 I/O 유틸리티 함수를 구현한 패키지이다. io.ReadFile은 전체 파일을 메모리로 읽어들여 빠른 읽기를 수행할 수 있다. 그래서 큰 파일을 읽을 때는 사용하지 않는 것이 좋다. 정확한 파일 크기의 byte slice([]byte)를 할당하며, 자동으로 파일을 닫는다.
* package "bufio"
  * bufio는 효율 향상을 위해 입출력을 버퍼링하는 io.Reader, io.Writer를 래핑하는 타입을 제공한다. 행, 단어 단위로 읽기를 수행할때 사용한다. bufio.Scanner는 독립적인 행(line)을 효율적으로 읽는 데 유용한 타입이다. Scanner의 Scan()함수는 미리 정의된 개행문자 혹은 사용자가 정의한 delimeter(Split()사용)를 만날 때까지를 한 줄로(행, line) 판단하여 데이터를 한줄씩 읽는다.



### 정규표현식

 정규표현식은 특정한 규칙을 가진 문자열의 집합을 표현하는데 사용하는 형식 언어입니다. 주로 Programming Language나 Text Editor 등 에서 문자열의 검색과 치환을 위한 용도로 쓰이며, 입력한 문자열에서 특정한 조건을 표현할 경우 일반적인 조건문으로는 다소 복잡할 수도 있지만, 정규표현식을 이용하면 매우 간단하게 표현 할 수 있습니다. 

 하지만 코드가 간단한 만큼 가독성이 떨어져서 표현식을 숙지하지 않으면 이해하기 힘들다는 문제점이 있습니다.

![](http://www.nextree.co.kr/content/images/2016/09/jhkim-140117-RegularExpression-151-1.png)

 이번 Go 언어 시간에서는 정규표현식에 대한 자세한 설명을 하지는 않을 것이기 때문에, 자주 사용하게 될 정규표현식을 코드 예제를 작성하고 실행하여 살펴보도록 하겠습니다. 

```go
package main

import (
	"bytes"
	f "fmt"
	"regexp"
)

func main() {

	match, _ := regexp.MatchString("p([a-z]+)ch", "peach")
	f.Println(match)
	r, _ := regexp.Compile("p([a-z]+)ch")

	f.Println(r.MatchString("peach"))
	f.Println(r.FindString("peach punch"))
	f.Println(r.FindStringIndex("peach punch"))
	f.Println(r.FindStringSubmatch("peach punch"))
	f.Println(r.FindStringSubmatchIndex("peach punch"))
	f.Println(r.FindAllString("peach punch", -1))

	f.Println(r.FindAllStringSubmatchIndex("peach punch pinch", -1))
	f.Println(r.FindAllString("peach punch pinch", 2))
	f.Println(r.Match([]byte("peach")))

	r = regexp.MustCompile("p([a-z]+)ch")
	f.Println(r)

	f.Println(r.ReplaceAllString("a peach", "<fruit>"))

	in := []byte("a peach")
	out := r.ReplaceAllFunc(in, bytes.ToUpper)
	f.Println(string(out))

}

```



### JSON

 JavaScript Object Notation (JSON)은 Javascript 객체 문법으로 구조화된 데이터를 표현하기 위한 문자 기반의 표준 포맷입니다. 웹 어플리케이션에서 데이터를 전송할 때 일반적으로 사용합니다. json 문법은 다른 프로그래밍 언어를 해보았다면 어느정도 익숙해져 있다고 가정하고  자세한 설명은 넘어가도록 하겠습니다. Json 자체에 대해 궁금하다면 아래 링크를 확인하시길 바랍니다. 

> https://ko.wikipedia.org/wiki/JSON 

 다음 예제를 확인하여 간단하게 json을 만들어 보도록 하겠습니다. 

```go
package main

import (
	"encoding/json"
	f "fmt"
)

func main() {

	doc := `{"name": "lucas", "age": "27"}`

	var data map[string]interface{} // JSON 패키지가 디코딩 데이터를 저장할 수 있는 변수를 선언, 임의의 데이터 타입의 문자열 맵을 가짐

	json.Unmarshal([]byte(doc), &data)
	f.Println(data["name"], data["age"])

	f.Println("------------------")
	json2()

}

func json2() {

	data := make(map[string]interface{})

	data["name"] = "K"
	data["age"] = 10

	doc, _ := json.Marshal(data)
	f.Println(string(doc))
}

```

 여기서 Marshal과 Unmarshal이라는 함수를 호출한 것을 확인할 수 있습니다. 소프트웨어는 바이트 단위로 데이터를 인식하는데, 모든 바이트는 해석하는 도구에 따라 달라집니다. 이렇게 변환하고 해석하는 것을 우리는 인코딩 혹은 마샬링이라고 부르게 됩니다. Go 언어에서 encoding 패키지가 이를 담당하게 되고, 실제로는 인터페이스만 정의되어 있고 사용되는 데이터 형태에 따라 서브패키지로 기능이 구성됩니다. 

 **‘마샬링(Marshaling)’** 혹은 **‘인코딩(Encoding)’**은 논리적 구조를 로우 바이트로 변환하는 것으로 Go value를 바이트 슬라이스로 변경하는 것을 말합니다. 

```go
func Marshal(v interface{}) ([]byte, error) 
```

 이와 반대로 바이트 슬라이스나 문자열을 논리적 자료 구조로 변경하는 것을 **언마샬링(Unmashaling)**이라고 합니다. 

```go
func Unmarshal(data []byte, v interface{}) error 
```

 따라서 첫번째 인자로 바이트 슬라이스를 넘겨주고, 두번째로 결과를 담게될 변수 포인터를 넘겨주는 구조로 작성해야 합니다. 

 다음 예제를 통해 구조체를 통한 json을 생성해 보도록 하겠습니다.

```go
package main

import (
	"encoding/json"
	f "fmt"
)

type Author struct {
	Name  string
	Email string
}

type Comments struct {
	Id      uint64
	Author  Author
	Content string
}

type Article struct {
	Id         uint64
	Title      string
	Author     Author
	Content    string
	Recommends []string
	Comments   []Comments
}

func main() {

	data := make([]Article, 1) // 값을 저장할 구조체 슬라이스

	data[0].Id = 1
	data[0].Title = "Hello, World"
	data[0].Author.Name = "lucas"
	data[0].Author.Email = "lucas@example.com"
	data[0].Content = "Good"
	data[0].Recommends = []string{"J", "K"}

	data[0].Comments = make([]Comments, 1)
	data[0].Comments[0].Id = 1
	data[0].Comments[0].Author.Name = "Keven"
	data[0].Comments[0].Author.Email = "keven@example.com"
	data[0].Comments[0].Content = "Nice keven"

	doc, _ := json.Marshal(data)
	f.Println(string(doc))

}

```



## URL 파싱하기 

 Go 언어에서는 간단하게 URL를 파싱할 수 있는 기능을 제공합니다. 다음 예제를 통해서 간단하게 확인해 보세요. 

```go
package main

import (
	f "fmt"
	"net"
	"net/url"
)

func main() {

	s := "postgres://user:pass@host.com:5432/path?k=v#f"

	u, err := url.Parse(s)
	if err != nil {
		panic(err)
	}

	f.Println(u.Scheme)
	f.Println(u.User)
	f.Println(u.User.Username())

	password, _ := u.User.Password()
	f.Println(password)

	f.Println(u.Host)
	host, port, _ := net.SplitHostPort(u.Host)
	f.Println(host)
	f.Println(port)

	f.Println(u.Path)
	f.Println(u.Fragment)

	f.Println(u.RawQuery)
	m, _ := url.ParseQuery(u.RawQuery)
	f.Println(m)
	f.Println(m["k"][0])

}

```



## 압축 및 압축 풀기 

 Go 언어에서는 간단하게 파일을 압축하고 풀수 있는 패키지를 기본적으로 제공합니다. 각각의 예제를 작성하고 실행하겠습니다.



### 파일 압축하기 

```go
package main

import (
	"compress/gzip"
	f "fmt"
	"os"
)

func main() {

	file, err := os.OpenFile(
		"hello.txt.gz",
		os.O_CREATE|os.O_RDWR|os.O_TRUNC,
		os.FileMode(0644))
	if err != nil {
		f.Println(err)
		return
	}
	defer file.Close()

	w := gzip.NewWriter(file)
	defer w.Close()

	s := "Hello World"
	w.Write([]byte(s))
	w.Flush()

}

```



### 파일 압축 풀기 

```go
package main

import (
	"compress/gzip"
	f "fmt"
	"io/ioutil"
	"os"
)

func main() {

	file, err := os.Open("hello.txt.gz")
	if err != err {
		f.Println(err)
		return
	}
	defer file.Close()

	r, err := gzip.NewReader(file)
	if err != nil {
		f.Println(err)
		return
	}
	defer r.Close()

	b, err := ioutil.ReadAll(r)
	if err != nil {
		f.Println(err)
		return
	}

	f.Println(string(b))

}

```



## TCP 통신 

 Go 언어에서는 기본 패키지에 다양한 네트워크 프로토콜을 제공합니다. 이중 TCP는 네트워크 전송계층에서 가장 핵심적인 부분이라고 할 수 있을 정도로 많이 사용되며, HTTP 프로토콜도 TCP기반으로 동작합니다. 

 간단하게 서버 / 클라이언트 통신 서버를 만들어 보고 동작하는지 확인해보도록 하겠습니다. 



### TCP Server

```go
package main

import (
	f "fmt"
	"net"
)

func main() {

	f.Println("server running 8888 port")

	ln, err := net.Listen("tcp", ":8888") // 8888 포트로 리스닝하는 tcp 서버를 생성합니다.
	if err != nil {
		f.Println(err)
		return
	}
	defer ln.Close()

	for {
		conn, err := ln.Accept() // 클라이언트로 부터 커넥션이 들어올 경우 이를 받아드립니다. 
		if err != nil {
			f.Println(err)
			continue
		}
		defer conn.Close()

		go requestHandler(conn)
	}

}

func requestHandler(conn net.Conn) {

  data := make([]byte, 4096) // 클라이언트와 서버간의 데이터 길이를 정의합니다. (바이트 슬라이스)

	for {
		n, err := conn.Read(data)
		if err != nil {
			f.Println(err)
			return
		}

		f.Println(string(data[:n]))

		_, err = conn.Write(data[:n]) // 해당 클라이언트로부터 데이터를 전송합니다.
		if err != nil {
			f.Println(err)
			return
		}
	}
}

```



### TCP Client

```go
package main

import (
	f "fmt"
	"net"
	"strconv"
	"time"
)

func main() {

	client, err := net.Dial("tcp", "127.0.0.1:8888") // 해당 서버로 접속 시도

	if err != nil {
		f.Println(err)
		return
	}
	defer client.Close()

	go func(c net.Conn) {
		data := make([]byte, 4096) // 서버와 클라이언트간의 데이터 길이 동기화

		for {
			n, err := c.Read(data)
			if err != nil {
				f.Println(err)
				return
			}

			f.Println(string(data[:n]))
			time.Sleep(1 * time.Second)

		}
	}(client)

	go func(c net.Conn) {
		i := 0
		for {
			s := "Hello " + strconv.Itoa(i)

			_, err := c.Write([]byte(s)) // 서버로 부터 데이터 전송
			if err != nil {
				f.Println(err)
				return
			}

			i++
			time.Sleep(1 * time.Second)
		}
	}(client)

	f.Scanln()

}

```





## RPC 통신 

 RPC(Remote Procedure call)이란, 별도의 원격 제어를 위한 코딩 없이 다른 주소 공간에서 리모트의 함수나 프로시저를 실행 할 수 있게 해주는 프로세스간 통신입니다. 즉, 위치에 상관없이 RPC를 통해 개발자는 위치에 상관없이 원하는 함수를 사용할 수 있습니다.

 RPC 모델은 분산컴퓨팅 환경에서 많이 사용되어왔으며, 현재에는 MSA(Micro Software Archtecture)에서 마이크로 서비스간에도 많이 사용되는 방식입니다. 서로 다른 환경이지만 서비스간의 프로시저 호출을 가능하게 해줌에 따라 언어에 구애받지 않고 환경에 대한 확장이 가능하며, 좀 더 비지니스 로직에 집중하여 생산성을 증가시킬 수 있습니다.

 아래 간단한 RPC 예제를 작성해 보도록 하겠습니다. 

### RPC Server

```go
package main

import (
	f "fmt"
	"net"
	"net/rpc"
)

type Calc int // RPC 서버에 등록하기 위해 임의의 타입으로 정의

type Args struct { // 매개변수
	A, B int
}

type Reply struct { // 리턴값
	C int
}

func main() {

	f.Println("rpc server running at 6000 port")

	rpc.Register(new(Calc)) // Calc 타입의 인스턴스를 서버의 등록

	ln, err := net.Listen("tcp", ":6000")
	if err != nil {
		f.Println(err)
		return
	}
	defer ln.Close()

	for {
		conn, err := ln.Accept()
		if err != nil {
			continue
		}
		defer conn.Close()

		go rpc.ServeConn(conn)
	}

}

func (c *Calc) Sum(args Args, reply *Reply) error {

	reply.C = args.A + args.B // 두 값을 더하여 리턴값 구조체에 넣어줌
	return nil

}

```



### RPC Client

```go
package main

import (
	f "fmt"
	"net/rpc"
)

type Args struct {
	A, B int
}

type Reply struct {
	C int
}

func main() {

	client, err := rpc.Dial("tcp", "127.0.0.1:6000") // RPC 서버 접ㅈ속
	if err != nil {
		f.Println(err)
		return
	}
	defer client.Close()
  
  // 동기 호출
	args := &Args{1, 2}
	reply := new(Reply)

	err = client.Call("Calc.Sum", args, reply) // 함수 호출
	if err != nil {
		f.Println(err)
		return
	}
	f.Println(reply.C)

  // 비동기 호출
	args.A = 4
	args.B = 9

	sumCall := client.Go("Calc.Sum", args, reply, nil) // 고루틴으로 호출
	<-sumCall.Done
	f.Println(reply.C)

}

```



## HTTP 서버 

 Go 언어를 사용한 기본 컨셉을 확인해 보았습니다. Go 언어는 디바이스 제어부터 애플리케이션 계층까지 다양한 분야에서 사용할 수 있습니다. 마지막으로 간단한 HTTP 서버를 작성하여 마루리 하도록 하겠습니다. 

```go
package main

import (
	f "fmt"
	"net/http"
)

func main() {

	f.Println("running http server at 8080 port")

	http.HandleFunc("/", func(res http.ResponseWriter, req *http.Request) {
		res.Write([]byte("Hello World"))
	})

	http.ListenAndServe(":8080", nil)
}

```

 

## 마지막 인사

 지금까지 본 튜토리얼을 따라와 주셔서 감사합니다. 본 문서에서 수정해야하는 부분이 있다면 seongwon@edu.hanbat.ac.kr로 연락주시면 빠르게 수정할 수 있도록 하겠습니다. 감사합니다. 





































