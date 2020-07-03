# Go Tutorial 



[TOC]

## Go 언어란? 

 본 문서는 다양한 인터넷 자료와 Go 공식 문서 및 "가장 빨리 만나는 Go 언어"

![](https://hackernoon.com/drafts/2c35295i.png)

 2019년 구글이 개발한 프로그래밍 언어로써, GC (garbage collection)와 병행성 (concurrent)을 잘 지원하는 컴파일 언어입니다. 로버트 그리즈머, 롭 파이크, 케네스 톰슨이 C++의 복잡성이 싫어서 개발되었습니다. 

 현재도 어떠한 패키지에 무엇을 포함할지는 이 세 사람이 만장일치로 합의해야 이루어진다고 하며, **Golang** 으로 불리기도 합니다. Go 언어 사용자들을 고퍼(Gopher)라고 부르며, 고퍼들을 위한 연례행사인 고퍼콘(Gophercon)이 열리고 있습니다. 



### Go 언어 특징

 Go 언어의 특징은 다음과 같습니다.  

1. 컴파일 언어이지만 컴파일러가 소스 코드를 해석하는 pass 수를 줄여서 인터프리터 언어처럼 빠르게 동작합니다. 
2. 언어의 문법이 간결하여 접근하기 쉽고 높은 성능을 낼 수 있습니다. 
3. 자료형 체계에서 정적 타입 검사가 이루어지기 때문에 Python 등에 익숙해져 있는 경우 생소할 수 있지만 풍부한 라이브러리를 통해서 다양한 기능을 쉽게 구현할 수 있습니다. 
4. 고루틴이라는 비동기 매커니즘을 제공하여 이벤트 처리 및 병렬 프로그래밍 작성이 쉽습니다. 
5. 고루틴은 OS에서 관리하는 경량 스레드보다 더 가볍기 때문에 CPU 코어갯수와 무관하게 수백, 수천만 고루틴을 작성해도 성능에 문제가 발생하지 않습니다. ( 비공기 처리 부분은 Erlang에서 영향을 받았기 때문 )
6. 파일 언어인 덕분에, 속도가 느린 스크립트 언어에서 연산 퍼포먼스가 필요한 부분을 Go로 대체해 넣을 수도 있습니다. 



![](https://w.namu.la/s/9954b15909d4b5acd1772a6a0ec35a7002645f92db28bc44568e8fb1c35a9b406dc23afa2e164dd95b24f783fff197e55897e2691d061cfc3ad60f328fabd43b5387e96539a2541b0a70fb3d3c39f5fd9e39d43af753bf4aaefd367452200b57)

 2016년 중반부터 CMS (Contents Management System)와 마이크로서비스 부분으로 사용 영역 및 점유율이 급격하게 늘어서 현재 스택오버플로우에서 실시한 2017년 미국 개발자 평균 연봉 1위 (11만불)의 언어로 기록되었습니다. 



### 지원 OS

![](https://www.hackint0sh.org/wp-content/uploads/2019/05/Best-os-for-Gaming.jpg)

 현재는 리눅스, 윈도우10, macOS 등 여러 플랫폼을 지원합니다. (윈도우 XP, 윈도우 Vista, 윈도우 7을 지원하지 않습니다.) 바이트코드를 생성하는 언어가 아니기 때문에 바이너리만 배포할 경우 C/C++ 프로그램이 그렇듯 해당 타깃 머신에 맞춰서 각각 컴파일해서 배포하거나 소스코드를 배포해야 합니다. 



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
* godoc : HTTP를 통해 문서 확인.
* gorename : 변수, 함수 등을 형 안전(type-safe) 방식으로 이름 변경
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

 Go 언어는 윈도우 10만 공식적으로 지원하기 때문에 이전 버전을 사용하고 있다면 OS 업그레이드를 진행하여 주시길 바랍니다. 본 설치는 윈도우 10 2004 빌드에서 진행하였습니다. 

1. https://golang.org/ 다음 링크로 접속합니다. 
2. 

#### Linux 



#### Mac OS 



### 개발환경 설정하기



## Hello World 작성하기 



## 변수 설정하기



## 조건문 / 반복문 / 분기문 



## 배열과 슬라이스 



## 클로저 / 재귀 / 지연호출 



## 구조체 / 메서드 / 인터페이스 



## 고루틴 / 채널 / 동기화 / 셀렉트 



## 파일 입출력 / 정규 표현식 / json



## 자료구조 



## 암호화 / 복호화 



## 압축 및 압축 풀기 



## TCP 통신 



## RPC 통신 



## HTTP 서버 







































