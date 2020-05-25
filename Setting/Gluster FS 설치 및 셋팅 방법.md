# Gluster FS 설치 및 셋팅 방법



![](https://www.gluster.org/wp-content/uploads/2016/03/gluster-ant.png)



 Gluster FS는 분산 스토리지 시스템으로 2011년 레드헷에 인수되어 이더넷 혹은 인피니티밴드를 통해서 수백수천 개의 서버에 스토리지를 단일 네트워크 파일 시스템으로 통합한다. 기존의 분산 파일 시스템 구성에 비해 간단하고 대규모 I/O 처리능력이 뛰어나고 높은 호환성으로 인해서 클라우드 인프라 구성에 많이 이용되고 있다. 

 특히나 메타데이터를 관리할 중앙서버가 필요 없고 Gluster FS 노드들에 데이터의 변경 이력이 실시간으로 반영되기 때문에 하나의 노드가 문제가 발생해도 다른 여러노드에 분산저장이 되어 있다는 점이 장점이다. 

![](https://www.researchgate.net/profile/Kyar_Aye/publication/283644051/figure/fig13/AS:613545414758401@1523292034143/Gluster-File-System-Architecture.png)

 본인은 docker-swarm를 통해 클러스터링 되어있는 컨테이너 외부 볼륨 데이터를 일괄적으로 관리하기 위해 사용하였다. 



### Prerequisites 

* OS : Ubuntu 18.04 LTS

* VM : 7개

* Network : 192.168.10.0/24 

  * 192.168.10.11, 192.168.10.21~26

  

### Configure Hosts File

 작업을 진행하기 있어서 각 Gluster FS를 구성하고자 하는 서버의 호스트 파일을 수정한다. 

```bash
$ vi /etc/hosts
```

```shell
192.168.10.11 master
192.168.10.21 swarm-1
192.168.10.22 swarm-2
192.168.10.23 swarm-3
192.168.10.24 swarm-4
192.168.10.25 swarm-5
192.168.10.26 swarm-6
```

 위와같이 호스트 파일을 변경하고 ping이 정상적으로 나가는지 확인한다. 

```shell
$ ping -c 3 swarm-1
$ ping -c 3 swarm-2
$ ping -c 3 swarm-3
...
```



### Install Gluster FS Server

 Gluster FS를 설치하기 위해서 모든 노드들에 다음과 같이 설치를 진행한다. 

```bash
$ sudo apt install software-properties-common -y
$ wget -O- https://download.gluster.org/pub/gluster/glusterfs/6/rsa.pub | apt-key add -
$ sudo add-apt-repository ppa:gluster/glusterfs-6
$ sudo apt install glusterfs-server -y

$ sudo systemctl start glusterd
$ sudo systemctl enable glusterd
```

```bash
$ systemctl status glusterd
● glusterd.service - GlusterFS, a clustered file-system server
   Loaded: loaded (/lib/systemd/system/glusterd.service; enabled; vendor preset:
   Active: active (running) since Mon 2020-05-25 16:47:19 KST; 1h 46min ago
     Docs: man:glusterd(8)
  Process: 22930 ExecStart=/usr/sbin/glusterd -p /var/run/glusterd.pid --log-lev
 Main PID: 22948 (glusterd)
    Tasks: 9 (limit: 4915)
   CGroup: /system.slice/glusterd.service
           └─22948 /usr/sbin/glusterd -p /var/run/glusterd.pid --log-level INFO

May 25 16:47:18 master systemd[1]: Starting GlusterFS, a clustered file-system
May 25 16:47:19 master systemd[1]: Started GlusterFS, a clustered file-system

$ glusterfsd --version
glusterfs 6.9
Repository revision: git://git.gluster.org/glusterfs.git
Copyright (c) 2006-2016 Red Hat, Inc. <https://www.gluster.org/>
GlusterFS comes with ABSOLUTELY NO WARRANTY.
It is licensed to you under your choice of the GNU Lesser
General Public License, version 3 or any later version (LGPLv3
or later), or the GNU General Public License, version 2 (GPLv2),
in all cases as published by the Free Software Foundation.
```

 

### Configure Gluster FS Servers

 기본적인 작업이 완료되었으며 분산 스토리지 셋팅을 위해 다음 명령어를 입력한다. 

```bash
$ sudo mkdir -p /gfsvolume/swarm
$ sudo gluster volume create swarm_vol transport tcp \
	swarm-1:/gfsvolume/swarm swarm-2:/gfsvolume/swarm swarm-3:/gfsvolume/swarm \
	swarm-4:/gfsvolume/swarm swarm-5:/gfsvolume/swarm swarm-6:/gfsvolume/swarm force
	
$ sudo gluster volume start swarm_vol
```

 분산  Gluster FS 볼륨을 생성하기 위한 디렉터리를 생성하고 볼륨 생성을 진행한다. 이후 볼륨을 시작하게 되면 다음과 같이 볼륨 정보가 나타나면 정상적으로 셋팅이 진행된 것이다. 

```bash
$ sudo gluster volume info

Volume Name: swarm_vol
Type: Distribute
Volume ID: fa802b1d-c88d-4ae1-ab5b-4bf588eaebe0
Status: Started
Snapshot Count: 0
Number of Bricks: 6
Transport-type: tcp
Bricks:
Brick1: swarm-1:/gfsvolume/swarm
Brick2: swarm-2:/gfsvolume/swarm
Brick3: swarm-3:/gfsvolume/swarm
Brick4: swarm-4:/gfsvolume/swarm
Brick5: swarm-5:/gfsvolume/swarm
Brick6: swarm-6:/gfsvolume/swarm
Options Reconfigured:
nfs.disable: on
transport.address-family: inet
```



### Setup Gluster FS Client

 각 노드에게 볼륨을 마운트하기 위한 작업을 진행한다. 

```bash
$ mkdir -p /mnt/swarm
$ sudo mount -t glusterfs master:/swarm_vol /mnt/swarm
```

```bash
$ df -h /mnt/swarm/
Filesystem         Size  Used Avail Use% Mounted on
master:/swarm_vol  1.3T   92G  1.1T   8% /mnt/swarm
```

 모든 셋팅이 완료되었다. 마운트 된 해당 디렉터리에 파일을 생성하게 되면 모든 노드들에 분산 저장된다. 추가적으로 마운트 된 볼륨이 재부팅되어도 유지될 수 있도록 fstab 셋팅을 진행한다.

```shell
$ vi /etc/fstab

master:/swarm_vol /mnt/swarm glusterfs defaults,_netdev 0 0
```































