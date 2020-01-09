# VPN (Virtual Private Network)

![](https://www.my-private-network.co.uk/wp-content/uploads/2016/10/VPN-explainer-1.png) 

* Virtual Private Network (VPN)은 물리적으로 떨어져 있는 지역에 네트워크를 전용선으로 연결하기에는 상당한 비용이 들어가기 때문에 일반적인 인터넷 네트워크와 암호화 기술을 사용하여 통신 시스템을 구축하는 것을 말합니다. 

* 이러한 상황에서 일반적인 인터넷 회선을 암호화하여 개인 전용선으로 사용하는 것이 VPN의 핵심으로, 기업에서 본사와 지사 사이에 전용선 대신에 비용절감을 위해서 사용하는 경우가 많으며, 개인이 사용하는 경우도 많이 있습니다. 
* VPN은 일반적으로 **안전한 네트워크**라고 말합니다. VPN에 접속한 클라이언트가 인터넷에 통신하는 경우 VPN 서버의 IP로 데이터가 송수신되고, 클라이언트와 서버간에는 터널간 패킷을 암호화 하기 때문에 VPN 서버의 로그를 확인하지 않는 이상 사용자를 알아내는 것이 힘들다고 할 수 있습니다. 
  * 거의 힘들다고 표현하는 이유는 VPN의 종류나 암호화 방식에 따라서 해독이 가능한 경우가 있기 때문입니다. 



### VPN의 장점 

1. 전용선을 일일히 설치할 필요가 없기 때문에 장거리 통신망 구축비용이 매우 저렴하다. 
2. **익명성**보장이 가능하다. 
3. 해외 사이트 속도 향상 
   * 이는 국내 ISP가 늘어나는 해외 트래픽 비용을 감당하지 못해 QoS 정책을 통해 국내 유저의 해외 인터넷 속도의 제한을 걸었기 때문입니다. 
   * ISP의 속도제한을 우회하여 접속하기 때문에 본래 나와야하는 속도가 나오는 경우가 생기기 때문입니다. 
   * 가입된 인터넷 회선 종류에 따라서 다르기 때문에 무조건적인 장점이라고 할 수 없기도 하니 참고 하시길 바랍니다. 



### VPN의 특징 

* 응용 애플리케이션 하단에 동작하기 때문에 애플리케이션을 수정할 필요가 없으며, **터널링** 기술과 암호화 및 인증 기술을 사용하여 사용자에게 투명한 통신 서비스를 제공합니다. 

  >터널링은 상용망에서 전용망과 같은 보안효과를 주기 위한 기법으로 VPN 내의 두 호스트 간의 가상 경로를 제공하며, 인터넷과 같이 안전하지 않은 공중망에서 사용자간 마치 터널이 뚫린 것처럼 통로를 마련하여 데이터를 안전하게 전송하는 방식

* 다양한 프로토콜과 방식을 통해 다양한 VPN 구축이 가능합니다. 또한 위치에 상관없이 해당 ISP의 POP (Point of Presence, ISP망의 상호간 접속점으로 가입망에서 인터넷 백본망 접근을 위한 접속점)로 접속하면 VPN 접속이 가능하기 때문에 이동 사용자의 접속 부담이 감소됩니다. 
* VPN의 구성요소는 다음과 같이 2가지로 나뉘어 집니다. 
  * **L2L (LAN to LAN)**
    * 본사, 지사간의 연결 유형을 가지고 있으며, VPN 장비를 설치하여 네트워크에 접속합니다. 
  * **L2C (LAN ro Client)**
    * 재택근무, 이동근무 등과 같은 소규모에 사용되며 각 PC에서 Client를 통해 접속합니다. 
* 단, ISP간의 기술이 다른 문제점으로 다른 ISP 간의 연동문제가 발생할 수 있으며, 성능저하의 문제가 발생할 수 있습니다.  



### 통신규격 

* **PPTP (Point to Point Tunneling Protocol)**

  ![](https://www.yamaha.com/products/en/network/settings/mikrotik_pptp_vpn/images/diagram.jpg)

  * 라우터와 클라이언트에서 일반적으로 많이 사용되는 통신 규격으로 PPP (Point-to-Point Protocol) 기술을 확장하여 개발되었습니다. 

  * 마이크로소프트가 주도하여 개발한 MS-CHAP과 RC4를 합성하여 암호화 합니다. 일반적으로 대부분의 OS가 지원하지만 시간이 지날 수록 많은 보안상의 결함이 발견되어 애플의 경우 PPTP를 사용하지 않고 있습니다. 

  * 단, 가정용이나 높은 수준의 암호화 등급에 의존하지 않은 경우 사용하기 충분하기 때문에 12자리의 복잡한 암호를 사용할 것을 권장하고 있습니다. 

    ![](https://www.oreilly.com/library/view/virtual-private-networks/1565925297/tagoreillycom20070222oreillyimages92272.png)

  * IP, IPX 또는 NetBEUI (Network BIOS Enhanced User Interface, IBM) 페이로드를 암호화하고, IP헤더로 캡슐화 하여 전송하는데, PPTP는 터널의 유지, 보수, 관리를 위해서 TCP를 사용하고 이동통신에서 사용하기에 적합한 구조를 가지고 있습니다. 

  * IP주소를 설정할 때는 주소의 충돌을 피하기 위해서 LAN의 IP주소가 원격지의 VPN의 LAN 주소와 달라야 합니다. 

* **L2TP (Layer 2 Tunneling Protocol**

  ![](https://campus.barracuda.com/resources/attachments/image/41116183/1/Client2SiteL2TP.png)

  * **L2F (Layer 2 Forwarding Protocol)** 와 PPTP를 결합하여 만든 규격이기 때문에 PPP를 기본적으로 지원합니다. L2TP는 터널을 만들어주기만 하며, 암호화는 IPSec을 사용합니다.

  * PPTP는 IP 기반의 네트워크만 지원하는 반면 L2TP는 패킷 중심의 지점 간 연결이기만 하면 통신이 가능합니다. 

  * 또한, 두 지점 사이의 하나의 터널만을 생성하는 PPTP와 다르게 두 지점사이에 여러 개의 터널을 사용할 수 있으며, 터널에 따른 QoS 적용도 가능하다는 장점이 있습니다. 

    ![](https://www.researchgate.net/profile/Adrian_Graur2/publication/287458379/figure/fig4/AS:314843556007939@1452075961245/L2TP-Tunnel-Data-Frame-Format.png)

    > L2F은 시스코에서 제안한 터널링 프로토콜입니다. 자제적인 암호화나 기밀을 제공하지 않으며, PPP 트래픽을 터널링 하도록 설계되었습니다. 
    >
    > 단, 하나의 터널의 여러 개의 연결을 지원하기 때문에 다자간 통신이 가능하며 UDP를 사용합니다. 

  * 방화벽에서 차단하는 것으로 알려진 UDP 500포트를 사용하기 때문에 연결에 대한 문제를 겪을 수 있습니다. 

* **IPSec (IP Security)**

  ![](https://cdn.ttgtmedia.com/rms/onlineimages/security-ipsec_tunnel_mode_mobile.png)

  * IPSec은 보안성이 부족한 기존 IP프로토콜을 보완하기 위한 보안 기술로써, 기존 IPv4에서는 보안이 필요한 경우에만 선택적으로 사용되었지만 IPv6에서는 기본 스팩에 포함된 필수 기술입니다. 

    * AH (Authentication Header)와 ESP (Encapsulation Security Payload)를 통해 IP 데이터그램의 인증과 무결성, 기밀성을 제공합니다. 

  * IP계층의 보안을 위해서 IETF에 의해 제안되었으며 VPN 구현에 널리 사용되고 있는 기술입니다. 

  * IKE(Internet Key Exchange)와 ESP(Encapsulation Security Payload) 암호화 방식이 사용되는데 해당 암호화 패킷을 차단하면 이를 사용하는 L2TP도 사용할 수 없습니다. 

  * IPSec은 터널모드와 전송모드가 존재합니다.

    * **터널모드** : 전체 데이터 패킷을 암호화화며, 터널의 종단점과 첫 번째 라우터 사이는 평문으로 전송하고 라우터와 라우터 사이만 암호화가 되기 때문에 주로 망 간 연결에 사용합니다. 
    * **전송모드** : IP 페이로드를 암호화하여 IP헤더로 캡슐화합니다. 이를통해 데이터 패킷의 메시지가 암호화됩니다. 

    ![](https://community.cisco.com/kxiwq67737/attachments/kxiwq67737/6001-discussions-vpn/41422/1/74410-TunnelVsTransport.jpg)

  *  두 가지 모드 모두 서로 다른 두 네트워크 간의 데이터 전송을 보호합니다. 또한 강력한 보안 시스템을 제공하기 위해 다른 보안 프로토콜을 추가로 사용할 수 있습니다. 

* **OpenVPN**

  ![](https://openvpn.net/wp-content/uploads/2018/06/about_text_logo.png)

  * 오픈소스 VPN이라는 뜻으로 기초규격은 L2TP를 따르고 있으나 L2TP와 다르게 세개의 레이어를 확립할 수 있다는 장점이 있습니다. 
  * 다양한 OS를 지원하고 있지만 오픈소스라는 한계때문에 기본적으로 내장되어 있지 않으나 프로파일 하나만 있으면 어떤 OS라도 쉽게 연결할 수 있습니다. 
  * 3DES, AES, Camellia, Blowfish, CAST-128등과 같이 다양한 암호화 방식을 지원하기 때문에 상당히(매우) 안전하며, 대부분의 방화벽도 우회할 수 있다는 장점을 가지고 있습니다. 
  * 단, 설치 및 운영에 어려움이 있으며, 서드파티 소프트웨어를 통해 사용해야 한다는 불편함이 있습니다. 
    ![](https://duo.com/assets/img/documentation/openvpn_as/open_vpn_network_diagram.png)

* **SSTP (Secure Socket Tunneling Protocol)**

  ![](https://drfone.wondershare.com/images/article/2018/01/15148208737537.jpg)

  * 마이크로소프트에 의해 개발되어 윈도우 비스타 서비스팩1에 추가되었습니다. 비교적 최근에 개발된 통신규격이라고 할 수 있습니다. 
  * SSL v3 규격을 기반으로 제작되었으며, 현재는 리눅스와 RouterOS에서 사용할 수 있으나, 윈도우 전용 플랫폼으로 사용되고 있습니다. 
  * 다만 마이크로소프트가 SSTP를 독점 소유하고 있으며 NSA(미국 국방수 산하 국가안보국)에서 운영한다고 알려져 있어서 신뢰성에서 많은 의구심을 가지고 있는 전송규약이나 일반적으로 안전하다고 알려져 있습니다. 

* **MPLS (Multi-Protocol Label Switching) VPN**

  ![](https://www.juniper.net/documentation/images/g015524.gif)

  * MPLS VPN은 사이트-투-사이트 연결 종류에 가장 적합하다고 할 수 있습니다. MPLS에 대한 내용은 아래 링크를 참조하시길 바랍니다. 

  * 이 부분에 대한 내용은 LTE MPLS L3VPN 네트워크라는 주제로 따로 언급하도록 하겠습니다.

    * [MPLS의 이해](https://judo0179.tistory.com/40) 
