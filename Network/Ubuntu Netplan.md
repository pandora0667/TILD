# Ubuntu Bionic Netplan

* 연구실 서버를 Ubuntu Server 18.04.1 LTS 버전으로 전환이 완료된 상태에서 리버스 프록시 서버에 추가적으로 가상 NIC를 추가하고 IP를 구성하기 위해서 기존의 네트워크 설정파일을 들어갔더니 다음과 같은 문구를 발견하였다. 

  ```bash
  $ vi /etc/network/interfaces
  
  # ifupdown has been replaced by netplan(5) on this system.  See
  # /etc/netplan for current configuration.
  # To re-enable ifupdown on this system, you can run:
  #    sudo apt install ifupdown
  ```

* 기본적으로 설정하던 설정 방법이 deprecated 되었고 netplan 방식으로 전환된 것이다. 

* 예전방식을 고수하여 사용할 수도 있지만 예전방법을 고수한다면 발전이 있을 수 있을까 하는 생각도 들면서, 변경된 방식이 사용하기에 더 편하겠지 하는 생각에 공식문서를 참조하여 설정을 진행하였고 다음과 같이 작성하게 되었다. 

  * 찾아보니 변경된지가 한참이 지났는데 이제서야 발견을 하다니.. 



### 설치 타입에 따른 설정파일 위치

| Install Type |                File Location                 |
| :----------: | :------------------------------------------: |
|  Server ISO  |       **/etc/netplan/01-netcfg.yaml**        |
| Cloud Image  |     **/etc/netplan/50-cloud-init.yaml**      |
| Desktop ISO  | **/etc/netplan/01-network-manager-all.yaml** |

* 위의 표에서 확인할 수 있듯이 설치 타입에 따라 파일명은 달라질 수 있으나 기본적인 디렉토리 위치는 같다. 
* 또한 특이점으로는 YAML 표기법으로 변경되었다는 것이다. 



### Example

* 먼저 서버에 설치된 NIC의 이름을 확인하기 위해서 다음과 같은 명령어를 실행한다, 

  ```bash
  $ ip a
  
  1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
      link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
      inet 127.0.0.1/8 scope host lo
         valid_lft forever preferred_lft forever
      inet6 ::1/128 scope host
         valid_lft forever preferred_lft forever
  2: ens160: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
      link/ether 00:0c:29:08:81:97 brd ff:ff:ff:ff:ff:ff
      inet 10.0.0.3/24 brd 10.0.0.255 scope global ens160
         valid_lft forever preferred_lft forever
      inet6 fe80::20c:29ff:fe08:8197/64 scope link
         valid_lft forever preferred_lft forever
  ```

* 본 서버에는 NIC가 하나이기 때문에 하나의 NIC만 표기되지만 여러개의 NIC가 정상적으로 설치되었다면 이름을 확인 할 수 있을 것이다. 

* 공식 설명에서 나오는 문서의 예제를 통해서 설정 Example을 살펴보도록 하겠다. 

  * enp3s0 setup with IPv4 DHCP
  * enp4s0 setup with IPv4 static with custom MTU
  * IPv6 static tied to a specific MAC address
  * IPv4 and IPv6 DHCP with jumbo frames tied to a specific MAC address

  ```yaml
  ethernets:
      enp3s0:
          dhcp4: true
      enp4s0:
          addresses:
              - 192.168.0.10/24
          gateway4: 192.168.0.1
          mtu: 1480
          nameservers:
              addresses:
                  - 8.8.8.8
                  - 9.9.9.9
      net1:
          addresses:
              - fe80::a00:10a/120
          gateway6: fe80::a00:101
          match:
              macaddress: 52:54:00:12:34:06
      net2:
          dhcp4: true
          dhcp6: true
          match:
              macaddress: 52:54:00:12:34:07
          mtu: 9000
  
  ```

  * YAML의 장점답게 설정파일이 한눈에 보기 쉽게 되어있어서 크게 문제될 것 같지 않다고 생각한다. 
  * 아직 몇몇 개발자 분들이나 사용자분들은 YAML 표기가 생소할 수도 있으나 필자도 Ansible을 공부하다 보니 점점 익숙해지는 느낌이다. 

* Bonding 설정 

  * Bonding 네트워크 설정에 관한 네트워크 설정에 대해서는 다음 wiki page에서 확인바란다. 
    * [Ubuntu Bonding](https://help.ubuntu.com/community/UbuntuBonding?_ga=2.47296547.5475972.1546175131-1696157538.1544368933#Descriptions_of_bonding_modes) 
  * 본 설정은 mode 1 **active-backup** 으로 셋팅되어 있으며 mode 정보에 따라서 서술하면 된다. 

  ```yaml
  bonds:
      bond0:
          dhcp4: yes
          interfaces:
              - enp3s0
              - enp4s0
          parameters:
              mode: active-backup
              primary: enp3s0
  ```

* Bridges 설정 

  * DHCP를 활용한 간단한 예제는 다음과 같다. 

    ````yaml
    bridges:
        br0:
            dhcp4: yes
            interfaces:
                - enp3s0
    ````

* Vlan 설정

  * 기본적으로 vlan을 설정하기 위해서 key로 이름을 지정하고 vlan에서 사용하는 ID와 링크 정보를 추가한다. 

    ```yaml
    vlans:
        vdev:
            id: 101
            link: net1
            addresses:
                - 10.0.1.10/24
        vprod:
            id: 102
            link: net2
            addresses:
                - 10.0.2.10/24
        vtest:
            id: 103
            link: net3
            addresses:
                - 10.0.3.10/24
        vmgmt:
            id: 104
            link: net4
            addresses:
                - 10.0.4.10/24
    ```












