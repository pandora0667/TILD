# 오픈소스 기반 화상회의 플랫폼 Jitsi Meet 설치 및 셋팅

![Building your own Video Conferencing Platform with Jitsi Meet – Part 2 -  Aspire Systems Poland Blog](https://blog.aspiresys.pl/wp-content/uploads/2020/10/Blog2-1.png)

코로나 상황이 장기화되면서 비대면을 통한 온라인 시스템 사용이 많아지고 있는 가운데, 오픈소스로 공개된 Jitsi Meet를 온프레미스 혹은 클라우드 환경에서 설치하는 방법을 살펴보고자 한다. Jitsi Meet 이외에 많은 사람들이 사용하는 Zoom, google hangouts meet와 같은 플랫폼이 존재하지만 사용자별, 시간별로 별도의 요금이 청구된다.

Jitsi Meet는 Free Video Conferencing Software for Web & Mobile 플랫폼이며, WebRTC 기반으로 동작한다. 이외에도 Windows, Linux, macOS, iOS, Android의 현존하는 대부분의 시스템 환경을 지원한다.

오픈소스임에도 불구하고 대부분의 기능을 전부 지원하지만, 공식 홈페이지에서 설치 방법을 따라 하게 되면 에러가 발생하기 때문에 다음과 같이 정리하고자 한다. 클라우드 환경에서는 Jitsi Meet 환경을 마켓에서 제공하기도 하지만 본 글에서는 전체적인 설치 방법에 대해 기술한다.

## 구축 환경

-   **OS** : Ubuntu 20.04 LTS
-   **Processor**: Intel Core i5 or i7 CPU / 2 GHz Dual Core Processor or better
-   **RAM**: 8GB
-   **Storage**: 10GB of free hard drive space

## Installation

Jitsi Meet는 **Debian 9 (Stretch), Ubuntu 18.04 (Bionic Beaver)** 이상의 OS에서 설치가 가능하다. 또한 **OpenJDK 8 or OpenJDK 11** 버전을 사용해야 한다. 기본 설치를 진행하게 되면 OpenJDK 16 버전이 설치되서 사용자 추가 시 에러가 발생한다.

### 도메인 발급

Jitsi Meet를 사용하기 위해 도메인 발급이 필요하다. Let's Encrypt를 통해 SSL/TLS 적용할 것이며 기반 환경이 구축되어 있다는 전재하에 진행하도록 하겠다.

### Guide

```bash
$ sudo apt update && sudo apt dist-upgrade-y && sudo apt autoremove -y
$ sudo apt install apt-transport-https -y

$ sudo apt-add-repository universe
$ sudo apt update

$ sudo apt install openjdk-11-jdk -y 
$ curl https://download.jitsi.org/jitsi-key.gpg.key | sudo sh -c 'gpg --dearmor > /usr/share/keyrings/jitsi-keyring.gpg'
$ echo 'deb [signed-by=/usr/share/keyrings/jitsi-keyring.gpg] https://download.jitsi.org stable/' | sudo tee /etc/apt/sources.list.d/jitsi-stable.list > /dev/null
$ sudo apt update && sudo apt install jitsi-meet -y 
```

![](https://i.imgur.com/mHJfZOR.png)![0_1542322627544_fad902cf-aec2-4aac-9fed-66a8369fbefa-image.png](https://i.imgur.com/dqAUECs.png)

### Generate a Let's Encrypt certificate (optional, recommended)

```bash
$ sudo /usr/share/jitsi-meet/scripts/install-letsencrypt-cert.sh
```

### 고급 구성

Jitsi Meet는 UDP 10000 포트를 통해 사용자간의 비디오 및 오디오를 전송한다. 서버가 NAT를 통해 구축되어 있다면 (대부분의 환경이 여기에 포함) 다음과 같이 설정을 추가한다.

```bash
$ sudo vi /etc/jitsi/videobridge/sip-communicator.properties
```

```bash
org.ice4j.ice.harvest.NAT_HARVESTER_LOCAL_ADDRESS=<Local.IP.Address>
org.ice4j.ice.harvest.NAT_HARVESTER_PUBLIC_ADDRESS=<Public.IP.Address>
```

Jitsi Meet가 100명 이하의 적은 사용자를 가진다면 본 설정은 하지 않아도 되지만 그 이상의 사용자가 접속할 예정이라면 systemd를 수정해야 한다.

```bash
$ sudo vi /etc/systemd/system.conf 
```

```bash
DefaultLimitNOFILE=65000
DefaultLimitNPROC=65000
DefaultTasksMax=65000
```

설정이 완료된 값을 확인하기 위해서 다음과 같이 수행한다.

```bash
$ sudo systemctl show --property DefaultLimitNPROC
$ sudo systemctl show --property DefaultLimitNOFILE
$ sudo systemctl show --property DefaultTasksMax
```

### 마무리

설치가 마무리 되었다면 프로세스가 정상적으로 실행되고 있는지 확인하고, 재실행한다.

```bash
$ sudo systemctl status jitsi-videobridge2.service
● jitsi-videobridge2.service - Jitsi Videobridge
     Loaded: loaded (/lib/systemd/system/jitsi-videobridge2.service; enabled; vendor preset: enabled)
     Active: active (running) since Thu 2021-05-20 04:54:46 UTC; 5h 35min ago
   Main PID: 7843 (java)
      Tasks: 45 (limit: 65000)
     Memory: 206.2M
     CGroup: /system.slice/jitsi-videobridge2.service
             └─7843 java -Xmx3072m -XX:+UseG1GC -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp -Djdk.tls.ephemeralDHKeySize=2048 -D>

May 20 04:54:46 jitsi-meet systemd[1]: Starting Jitsi Videobridge...
May 20 04:54:46 jitsi-meet systemd[1]: Started Jitsi Videobridge.
```

```bash
$ sudo systemctl restart jitsi-videobridge2.service
```

### Uninstall

삭제 방법은 다음과 같다.

```bash
$ sudo apt purge jigasi jitsi-meet jitsi-meet-web-config jitsi-meet-prosody jitsi-meet-turnserver jitsi-meet-web jicofo jitsi-videobridge2 -y 
```

### Debugging problems

기타 설치후 문제가 발생한다면 다음과 같이 로그기록을 확인할 수 있다.

```bash
$ tail -f /var/log/jitsi/jvb.log
$ tail -f /var/log/jitsi/jicofo.log
$ tail -f /var/log/prosody/prosody.log
```

이외 발생하는 문제의 경우 방화벽 문제일 가능성이 크다. 트래픽 허용을 위해 포트가 열려있는지 확인한다.

-   80 TCP-Let 's Encrypt로 SSL 인증서 확인 / 갱신
-   443 TCP-Jitsi Meet에 대한 일반 액세스
-   10000 UDP-일반 네트워크 비디오 / 오디오 통신
-   22 TCP-SSH를 사용하여 서버에 액세스 하는 경우
-   3478 UDP-스턴 서버 (coturn, 선택 사항, 활성화하려면 config.js 변경 필요)
-   5349 TCP-TCP를 통한 대체 네트워크 비디오 / 오디오 통신 (예 : UDP가 차단된 경우), coturn에서 제공

---

간단하게 Jitsi Meet를 활용하기 위한 설치 방법에 대해서 서술하였다. 본 문서는 Jitsi Meet Handbook Docs를 참고하였으며, 기타 확인사항은 다음 URL을 확인한다.

-   [https://jitsi.github.io/handbook/docs/intro](https://jitsi.github.io/handbook/docs/intro)