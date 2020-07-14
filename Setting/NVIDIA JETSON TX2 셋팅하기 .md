# NVIDIA JETSON TX2 Setting

> 본 문서는 2018년 05월 31일 기준으로 작성한 문서입니다. 설치 시점과 상의할 수 있으므로 유의하셔야 합니다. 
>
> 셋팅환경은 Ubuntu 16.04 LTS,  NVIDIA JETSON TX2 기반으로 작성되었습니다. 



![](https://elinux.org/images/9/9c/NVIDIA_Jetson_TX2_Module_Devkit.png)



* NVIDIA JETSON TX2는 고성능, 저전력 기반의 임베디드 플랫폼입니다. 
* NVIDIA의 최신 Pascal 아키텍쳐로 구성되어 256 CUDA 코어를 가지고 있으며 8GB RAM, 32GB 스토리지, ARM HMP Dual Denver 2/2 MB L2 + Quad ARM® A57/2 MB L2 빅 리틀 구조의 ARM 프로세서를 가지고 있습니다.
* 가격은 약 100만원 전후로 해외구매를 통해서 구입할 수 있으며, 본 문서에서는 초기 설정부터 JetPack, TensorFlow 설치하는 방법까지 기술하도록 하겠습니다. 



### JETSON TX2 초기 설정하기

* 박스 개봉 이후 조립을 진행합니다.
* 반드시 HDMI가 지원되는 모니터에 JETSON TX2를 연결하고 화면에 표기되는 명령을 실행하여 초기 설정을 진행합니다. 
  * HDMI to DVI 케이블을 사용할 경우 화면이 표기되지 않을 수 있습니다.  
* JETSON TX2는 Ubuntu 16.04 LTS 버전을 사용합니다. 
* 기본 username과 password는 nvidia 입니다. 
* 지금에서 추가로 설정하더라도 JetPack을 설치하게 되면 커널이 새로 빌드되기 때문에 추가 작업을 진행할 필요 없습니다.



### NVIDIA JetPack 이란?  

* JetPack SDK는 NVIDIA에서 제공하는 AI 응용 프로그램을 구축할 때 사용하는 포괄적인 솔루션입니다. 

* JetPack 인스톨러를 사용하여 최신 OS 이미지로 Jetson Developer Kit을 플래시하고, 호스트 PC와 개발자 키트 용 개발자 도구를 설치하며, 개발 환경을 빠르게 시작하는 데 필요한 라이브러리와 API, 샘플 및 문서를 설치할 수 있습니다. 

* L4T R28.2가 탑재 된 JetPack 3.2는 NVIDIA Jetson TX2, Jetson TX2i 및 Jetson TX1의 최신 프로덕션 소프트웨어 릴리스입니다. 

* TensorRT, cuDNN, CUDA 툴킷, VisionWorks, GStreamer 및 OpenCV를 포함한 모든 Jetson 플랫폼 소프트웨어를 번들로 제공하며 LTS Linux 커널과 함께 L4T 위에 구축되었습니다.

* 주요 특징으로는 TensorFlow 모델 지원, DL 응용 프로그램의 최대 15 % perf / W 향상, Docker에 대한 즉시 사용 가능한 커널 지원 및 호스트 PC에서의 Ubuntu 16.04 지원이 포함됩니다.

  

  ![](http://1000logos.net/wp-content/uploads/2017/05/Logo-NVIDIA.jpg)

* JetPack의 최신버전은 https://developer.nvidia.com/embedded/jetpack 에서 다운 받으실 수 있습니다. 

  * 다운로드를 위해서 회원가입을 진행해야 합니다.

  

### NVIDIA JetPack 설치하기

* Ubuntu 16.04 LTS가 설치된 PC 혹은 노트북을 준비합니다. 

  * 반드시 버전이 동일해야 설치가 가능합니다. 
  * 모든 작업이 완료되기까지 **약 2~3시간**이 소요됩니다. 

* JESTON TX2와 Ubuntu 16.04 LTS PC가 같은 네트워크에 연결합니다. 

* Ubuntu 16.04 LTS PC에서 최신 JetPack을 다운로드 하고 터미널에서 다음과 같이 설정합니다. 

  ```bash
  $ chmod +x JetPack-${VERSION}.run
  ```

* 다운로드한 JetPack을 실행합니다. 

  

  ![](https://docs.nvidia.com/jetpack-l4t/content/developertools/mobile/jetpack/images/jetpack_l4t_install.006_600x441.png)

  

  * NEXT를 눌러서 진행합니다. 

  

  ![](https://docs.nvidia.com/jetpack-l4t/content/developertools/mobile/jetpack/images/jetpack_l4t_directory.006_600x441.png)

  

  * JetPack을 설치할 경로입니다. 기본값으로 설정하고 다음으로 넘어갑니다. 

  ​	

  ![](https://docs.nvidia.com/jetpack-l4t/content/developertools/mobile/jetpack/images/jetpack_l4t_dev_environment-3platforms.001_600x441.png)

  

  * 개발환경을 설치하기 위해 보유하고 있는 기기를 선택하고 다음으로 진행합니다. 

    

    ![](https://docs.nvidia.com/jetpack-l4t/content/developertools/mobile/jetpack/images/jetpack_l4t_sudo_pw.001_450x210.png)

    

    ![](https://docs.nvidia.com/jetpack-l4t/content/developertools/mobile/jetpack/images/jetpack_l4t_component_mgr.009_600x524.png)

    

    ![](https://docs.nvidia.com/jetpack-l4t/content/developertools/mobile/jetpack/images/jetpack_l4t_license.004_500x455.png)

    

  * 위와 같이 모든 선택 옵션을 선택하고 Accept 을 진행합니다. 

  * 다운로드를 진행하는데 네트워크 환경에 다르지만 대략 30분 정도 소요됩니다. 

  * 이후 설치하는데 약 10분 정도 소요됩니다. 

  

  ![](https://docs.nvidia.com/jetpack-l4t/content/developertools/mobile/jetpack/images/jetpack_l4t_begin_target_install.004_600x441.png)

  ![](https://docs.nvidia.com/jetpack-l4t/content/developertools/mobile/jetpack/images/jetpack_l4t_network_layout.008_600x441.png)

  ![](https://docs.nvidia.com/jetpack-l4t/content/developertools/mobile/jetpack/images/jetpack_l4t_network_interface.007_600x441.png)

  

  ![](https://docs.nvidia.com/jetpack-l4t/content/developertools/mobile/jetpack/images/jetpack_l4t_post_install.007_600x441.png)

  ![](https://docs.nvidia.com/jetpack-l4t/content/developertools/mobile/jetpack/images/jetpack_l4t_force_recovery_mode.001_600x364.png)

  

  * 이후 설치를 계속 진행하기 위해서 TX2를 복구모드로 접속해야 합니다. 

    * 실행중인 TX2의 전원을 종료합니다. 
    * 전원을 제거합니다. 
    * 내장된 마이크로 USB을 TX2에 연결하고 Ubuntu 16.04 LTS PC에 USB을 연결합니다. 
    * 전원을 다시 연결합니다. 
    * TX2의 전원을 킵니다. 
    * 이후 리커버리 버튼을 누른 상태에서 리셋버튼을 누르고 2초간 동시에 누른 상태에서 리셋 버튼을 때고 리커버리 버튼을 2초정도 누르고 있습니다. 

  * Ubuntu 16.04 LTS PC에서 lsusb 명령을 실행하고, NVIDIA Corp. 디바이스가 보이는지 확인합니다. 

  * 이후 엔터를 JetPack 설치 화면에서 엔터키를 치면 설치가 진행되며 부팅 파일을 옮기며, 필수 라이브러리와 각종 설치파일을 다운로드 및 빌드 작업을 진행합니다. (시간이 오래걸리니 느긋하게 기다리세요.)

  * 진행하는 동안 전원을 끄거나 다른 동작을 절대로 진행하지 마세요.

    ![](https://docs.nvidia.com/jetpack-l4t/content/developertools/mobile/jetpack/images/jetpack_l4t_post_install.007_600x441.png)![](https://docs.nvidia.com/jetpack-l4t/content/developertools/mobile/jetpack/images/jetpack_l4t_install_complete.006_600x441.png)



* 셋팅이 완료되면 모든 설정이 초기상태로 돌아갑니다. 

  * 비밀번호도 초기상태로 진행됩니다. 

  

* 설치가 완료되면 업데이트를 진행합니다. 

  ````bash
  $ sudo apt update && sudo apt upgrade -y 
  ````

  * 만약  GPG error: file:/var/cuda-repo-9-0-local  Release: The following signatures couldn't.... 이라는 오류가 발생하면 다음과 같이 명령을 입력합니다. 

  ```bash
  $ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys F60F4B3D7FA2AF80
  ```

* 샘플 프로그램을 실행하기 위해 다음과 같은 명령을 실행합니다. 

* 실행이 완료되면 영상과 함께 실시간으로 차량이 분석되는 영상이 출력됩니다. 

```bash
sudo ./jetson_clocks.sh
cd ~/tegra_multimedia_api/samples/backend
```

```bash
./backend 1 ../../data/Video/sample_outdoor_car_1080p_10fps.h264 H264 --trt-deployfile ../../data/Model/GoogleNet_one_class/GoogleNet_modified_oneClass_halfHD.prototxt --trt-modelfile ../../data/Model/GoogleNet_one_class/GoogleNet_modified_oneClass_halfHD.caffemodel --trt-forcefp32 0 --trt-proc-interval 1 -fps 10
```



### NVIDIA JETSON TX2 JetPack3.2 TensorFlow 설치하기 

* JAVA 8 버전을 먼저 설치합니다. 

  ```bash
  $ sudo apt-get update
  $ sudo add-apt-repository ppa:webupd8team/java
  $ sudo apt-get update
  $ sudo apt-get install oracle-java8-installer
  ```

* 다음 명령을 실행하여 의존성 파일을 설치합니다. 

  ```bash
  $ sudo apt-get install python3-numpy swig python3-dev python3-pip python3-wheel -y
  ```

* Bazel을 설치합니다. 

  ```bash
  $ wget --no-check-certificate 
  https://github.com/bazelbuild/bazel/releases/download/0.10.0/bazel-0.10.0-dist.zip
  
  $ unzip bazel-0.10.0-dist.zip -d bazel-0.10.0-dist
  
  $ cd bazel-0.10.0-dist
  ./compile.sh
  
  $ cp output/bazel /usr/local/bin
  ```

* github에서 tensorflow를 clone 합니다. 

  ```bash
  $ git clone https://github.com/tensorflow/tensorflow
  ```

* 다음과 같이 TensorFlow 셋팅을 진행합니다. 

  ```bash
  $ cd tensorflow
  
  $ ./configure
  
  You have bazel 0.10.0- (@non-git) installed.
  Please specify the location of python. [Default is /usr/bin/python]: /usr/bin/python3
  
  
  Found possible Python library paths:
    /usr/local/lib/python3.5/dist-packages
    /usr/lib/python3/dist-packages
  Please input the desired Python library path to use.  Default is [/usr/local/lib/python3.5/dist-packages]
  
  Do you wish to build TensorFlow with jemalloc as malloc support? [Y/n]: 
  jemalloc as malloc support will be enabled for TensorFlow.
  
  Do you wish to build TensorFlow with Google Cloud Platform support? [Y/n]: n
  No Google Cloud Platform support will be enabled for TensorFlow.
  
  Do you wish to build TensorFlow with Hadoop File System support? [Y/n]: n
  No Hadoop File System support will be enabled for TensorFlow.
  
  Do you wish to build TensorFlow with Amazon S3 File System support? [Y/n]: n
  No Amazon S3 File System support will be enabled for TensorFlow.
  
  Do you wish to build TensorFlow with Apache Kafka Platform support? [y/N]: n
  No Apache Kafka Platform support will be enabled for TensorFlow.
  
  Do you wish to build TensorFlow with XLA JIT support? [y/N]: n
  No XLA JIT support will be enabled for TensorFlow.
  
  Do you wish to build TensorFlow with GDR support? [y/N]: 
  No GDR support will be enabled for TensorFlow.
  
  Do you wish to build TensorFlow with VERBS support? [y/N]: 
  No VERBS support will be enabled for TensorFlow.
  
  Do you wish to build TensorFlow with OpenCL SYCL support? [y/N]: 
  No OpenCL SYCL support will be enabled for TensorFlow.
  
  Do you wish to build TensorFlow with CUDA support? [y/N]: y
  CUDA support will be enabled for TensorFlow.
  
  Please specify the CUDA SDK version you want to use, e.g. 7.0. [Leave empty to default to CUDA 9.0]: 
  
  
  Please specify the location where CUDA 9.0 toolkit is installed. Refer to README.md for more details. [Default is /usr/local/cuda]: /usr/local/cuda-9.0
  
  
  Please specify the cuDNN version you want to use. [Leave empty to default to cuDNN 7.0]: 
  
  
  Please specify the location where cuDNN 7 library is installed. Refer to README.md for more details. [Default is /usr/local/cuda-9.0]:
  
  
  Do you wish to build TensorFlow with TensorRT support? [y/N]: 
  No TensorRT support will be enabled for TensorFlow.
  
  Please specify a list of comma-separated Cuda compute capabilities you want to build with.
  You can find the compute capability of your device at: https://developer.nvidia.com/cuda-gpus.
  Please note that each additional compute capability significantly increases your build time and binary size. [Default is: 3.5,5.2]
  
  
  Do you want to use clang as CUDA compiler? [y/N]: 
  nvcc will be used as CUDA compiler.
  
  Please specify which gcc should be used by nvcc as the host compiler. [Default is /usr/bin/gcc]: 
  
  
  Do you wish to build TensorFlow with MPI support? [y/N]: 
  No MPI support will be enabled for TensorFlow.
  
  Please specify optimization flags to use during compilation when bazel option "--config=opt" is specified [Default is -march=native]: 
  
  
  Would you like to interactively configure ./WORKSPACE for Android builds? [y/N]: 
  Not configuring the WORKSPACE for Android builds.
  
  Preconfigured Bazel build configs. You can use any of the below by adding "--config=<>" to your build command. See tools/bazel.rc for more details.
  	--config=mkl         	# Build with MKL support.
  	--config=monolithic  	# Config for mostly static monolithic build.
  	--config=tensorrt    	# Build with TensorRT support.
  Configuration finished
  ```

* TensorFlow 빌드를 진행합니다. 

  ```bash
  $ bazel build --config=opt --config=cuda
  ```

* https://devtalk.nvidia.com/default/topic/1031300/tensorflow-1-7-wheel-with-jetpack-3-2-/

  * 위의 URL에서 TensorFlow 1.8 버전을 다운로드 받습니다. 

* 다음 명령을 실행하여 TensorFlow 설치를 진행합니다. 

  ```bash
  $ pip3 install tensorflow-1.8.0-cp35-cp35m-linux_aarch64.whl
  ```

  

### 설치 마무리 

* 모든 설치가 마무리 되었습니다. 
* 설정하시느라 고생 많으셨습니다. 다시한번 말씀 드리지만 본 설치 시점과 설정하시는 부분에 따라서 차이가 있으므로 유의하시길 바랍니다. 
* 현재 시점에서 Ubuntu 18.04 LTS 지원계획은 아직 없습니다. 혹시나 지원 소식이 나오면 추가적인 포스팅을 진행하도록 하겠습니다. 