# TesorFlow 설치 
  
>텐서플로는 다양한 작업에대해 데이터 흐름 프로그래밍을 위한 오픈소스 소프트웨어 라이브러리이다. 심볼릭 수학 라이브러리이자, 뉴럴 네트워크같은 기계학습 응용프로그램에도 사용된다. 이것은 구글내 연구와 제품개발을 위한 목적으로 구글 브레인팀이 만들었고 2015년 11월 9일 아파치 2.0 오픈소스 라이센스로 공개되었다. <br> 

![](https://hiseon.me/wp-content/uploads/2018/02/tensorflow-logo.png)

위의 위키백과와 같이 TensorFlow는 구글에서 공계한 기계학습 라이브러리로서 파이썬을 활용하여 연산처리를 작성할 수 있다. <br> 
본인은 4학년 캡스톤과제에 딥러닝을 적용하기 위하여 "골빈해커의 3분 딥러닝 텐서플로우맛" 이라는 책을 가지고 학습을 진행하기로 하였으며 설치과정을 정리하여 공유하고자 글을 쓰게 되었다. 

### TensorFlow의 기능
* 설치과정에 앞서서 TensorFlow의 기능을 살펴보도록 하자 
	* 데이터 플로우 그래프를 통한 표현 
	* 아이디어 테스트에서 서비스 단계까지 이용 
	* 계산 구조와 목표 함수만 정의하면 자동으로 미분계산 처리 가능 
	* 코드 수정없이 CPU/GPU 모드 동작가능
	* Python/C++를 지원하며, SWIG를 통해 다양한 언어 지원 가능  

* 일반적으로 공개된 버전의 경우 일반PC에서도 사용이 가능하고, GPU가속 버전의 경우 GPGPU를 사용해서 더욱 빠르게 처리할 수 있다. 

### TensorFlow 임베디드 보드 
![](http://images.nvidia.com/content/tegra/embedded-systems/images/jx10-jetson-tx2-170203.jpg)

















* 엔비디아에서 개발한 TX2 보드의 경우 파스칼 아키텍쳐로 구성되며, 256개의 CUDA 코어를 가지고 있다. 
* 기존의 공개된 TX1보다 사양은 올라갔으며 (가격도 상승....) 빠른 처리가 가능할 것으로 생각하여 TensorFlow 셋팅을 통해 사용하려고 준비하고 있다.  
* 여유가 된다면 TX2 개발보드를 구매하여 셋팅하고 실행하는 과정을 포스팅하도록 하고 본격적으로 TensorFlow를 설치해보록 하자!! 

### Hello, TensorFlow

* 본인은 Ubuntu 16.04.3 LTS 버전에서 설치하였으며, 사용자 환경에 따라 차이가 있음을 알아두자!! 

```bash
$ sudo apt install python3-pip -y 
$ pip3 install --upgrade pip 
$ pip3 install --upgrade tensorflow 
```
* 만약 엔비디아의 GPU를 사용하고 있다면 다음과 같이 실행한다. 

```bash
$ pip3 install --upgrade tensorflow-gpu 
```

* 필자가 읽고 있는 책에서 사용하는 라이브러리를 다음과 같이 추가로 설치하고, 깃허브에서 예제 소스를 다운받는다. 

```bash
$ pip3 install numpy matplotlib pillow 
$ git clone https://github.com/golbin/TensorFlow-Tutorials.git
```

* 이제 TensorFlow 설치가 끝났다. 이제 예제를 실행하면서 설치가 정상적으로 진행됬는지 확인하면 된다. 

```bash
$ cd TensorFlow-Tutorials/03 - TensorFlow Basic
$ python3 01 - Basic.py 

Tensor("Const:0", shape=(), dtype=string)
Tensor("Add:0", shape=(), dtype=int32)
...
```
* 다음과 같이 잘 실행된다면 TensorFlow설치를 마무리 지은 것이다. 마지막으로 주피터 노트북을 설치하여 편리한 환경을 만들어 보자!! 

### Hello, Jupyter Notebook
* 이곳에서 주피터 노트북에 대한 자세한 설명을 확인할 수 있다. 
	* http://jupyter.org/ 

```bash
$ pip3 install jupyter
$ jupyter notebook
```
* 만약, 본인의 PC에서 주피터 노트북을 설치했다면 다음과 같은 명령어를 치면 웹브라우저가 열리면서 실행될 것이다. 
* 하지만 원격에서 설치하고 사용하는 환경이라면 외부접속이 막혀있기 때문에 접속할 수 없기 때문에 다음과 같이 실행한다. 

```bash 
$ jupyter notebook --ip=0.0.0.0
```
* 이렇게 설정하면 외부 IP접속이 허용되는 것을 확인할 수 있다. 


### 문제해결 
주피터 노트북을 설치하고 웹브라우저까지 접속이 완료되었다면 실제 코드를 작성해볼 차례이다. 근데 본인은 새로운 프로젝트를 실행하면 다음과 같은 오류메시지가 뜨면서 실행이 되지 않았다. 
![](/Users/seongwonlee/Documents/Study/TensorFlow/jupyter error .png)

가만히 살펴보니 주피터 노트북에서 사용하는 설정파일에 접근할 수 없어서 나타나는 에러였다. 이 문제를 해결하기 위해 다음과 같은 코드를 실행한다. 

```bash 
$ sudo chown user:user ~/.local/share/jupyter 
```
* user 항목에 본인이 사용하는 계정이름을 적고 다시 확인해보면 오류가 발생하지 않을 것이다. 

