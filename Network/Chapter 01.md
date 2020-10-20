# Network Chapter 1 

* 브라우저의 동작을 처음부터 끝까지 추적하여 동작과정을 서술합니다. 
* URL의 규칙에 따라 URL의 의미를 조사하고, request message에 대한 전반적인 내용을 살펴봅니다. 
* 전송된 메시지가 네트워크를 통해서 서버측 까지 전달하는 과정을 살펴봅니다. 



## HTTP Request Message 

![](https://www.howtogeek.com/wp-content/uploads/2018/08/xshutterstock_1016605861.jpg.pagespeed.gp+jp+jw+pj+ws+js+rj+rp+rw+ri+cp+md.ic.7bIGbGMxEj.jpg)



* 사용자가 URL을 웹 브라우저에 입력하면 웹 브라우저는 URL을 해독하는 곳부터 동작이 시작됩니다. 

  ![](http://ibiblio.org/team/intro/URL/urlparts.gif)

  * URL의 앞 부분의 Protocol은 액세스 하는 방법을 나타내는 방법을 나타냅니다. 
  * 이러한 Protocol의 종류에 따라 뒤에 사용되는 방법이 정해집니다. 

* URL 해독이 완료되면 어디에 엑세스해야 하는지 판명됩니다. 

  * HTTP/HTTPS 프로토콜을 사용하여 웹 서버에 전달하는 과정을 거치게 됩니다. 

  ![](https://www.cloudflare.com/img/learning/security/glossary/what-is-ssl/http-vs-https.svg)

  * HTTP 프로토콜은 서버와 클라이언트가 주고받는 메시지의 내용이자 순서를 정리하고 있습니다. 

  * Request 메시지를 전송할 때, '무엇을', '어떻게 해서' 하겠다는 내용이 포함되어 있는데 '무엇을'에 해당되는 것을 URI(Uniform Resource Identifier)라고 하며, 페이지에 저장한 파일이나 CGI 프로그램의 파일명을 URI로 사용합니다. 

    * 다양한 엑세스 대상을 통칭하여 사용합니다. 

  * '어떻게 해서'에 대한 부분은 메소드에 해당하는 부분으로 웹서버에게 어떠한 동작을 하고 싶은지 전달하는 역할을 담당합니다. 

    ![](http://www.aliencoders.org/wp-content/uploads/2016/01/http-header-functions.jpg)

  * 이외에도 HTTP 메시지에 보충 정보를 나타내는 헤더파일과, Request 메시지가 서버에 도착하면, 그 결과가 정상적으로 처리되었는지 이상이 발생했는지에 대한 **Status Code**가 Response 메시지 앞에 포함되어 전송됩니다. 

#### Request message format 

![](https://mdn.mozillademos.org/files/13821/HTTP_Request_Headers2.png)

* 첫 번째 행을 Request Line 이라고 하며, 이 한 행으로 Request의 동작에 대한 내용을 대략적으로 알 수 있습니다. 
* 클라이언트는 서버에게 옵션헤더 정보를 보내 자신이 설정한 내용과 받아드릴 문서의 형식을 전달합니다. 
* 경로명은 URL에 포함되어 있으므로, URL에서 경로명을 추출하여 복사합니다.
* 다음 메시지 헤더에 부가적으로 자세한 내용을 작성합니다. 
  * 데이터의 종류, 언어, 압축 형식, 클라이언트나 서버의 소프트웨어의 명칭과 버전, 데이터 유효기간 등
* 마지막으로 메시지 본문에 전달할 실질적인 내용을 전달하게 됩니다. 
  * 단, GET 메소드의 경우 URI만으로 웹 서버가 무엇을 할지 판단할 수 있으므로 메시지 본문에는 내용이 없습니다. 



#### HTTP Response 

* Request 요청이 있고 나서는 서버측에서는 클라이언트로 부터 응답이 되돌아 오게 됩니다. 

  ![](http://www.tcpipguide.com/free/diagrams/httpresponse.png)

* 응답된 결과는 요청한 결과가 정상적으로 처리되었는지에 대한 여부가 포함되어 있습니다. 이를 **Status Code**라고 합니다. 

  ![](https://i1.wp.com/fixerrorcode.com/wp-content/uploads/2018/07/https-status-codes-fix.jpg?fit=638%2C479&ssl=1)

* Request 메시지에 쓰는 URI는 한 개만으로 결정되어 있기 때문에 파일을 한 개씩만 읽을 수 있습니다. 

  * 파일을 따로 따로 읽어드려야 하며, 서버측은 이에 대한 요청에 대한 응답을 반환합니다. 



## DNS Lookup 

* 엑세스 대상의 서버까지 메시지를 전송할 때는 IP 주소에 따라 엑세스 대상이 어디에 있는지 판단합니다. 

* 송신측의 메시지는 같은 서브넷 환경에 있는 스위치(허브)로 전달되고 송신측에 가장 가까운 라우터가 수신측의 경로를 파악하여 전송함으로서 통신이 진행하게 됩니다. 

* 이때, TCP/IP 네트워크는 IP주소를 가지고 통신을 하기 때문에 IP주소를 모르면 통신을 할 수 없습니다. 

* 따라서 사람은 기억하기 쉬운 이름(도메인)을 사용하고 실 통신은 IP주소를 사용하는 방식이 정착되었으며, 이러한 역할을 담당하는 것이 **DNS**입니다. 

  ![](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSQ9x5c8rzIhsKbdv9o4za_Q8UAJyTk7Eq5ZQeZCPLJojngDO-r)

* DNS 서버에 조회한다는 의미는 DNS 서버에 조회 메시지를 보내고, 이에 응답하는 메시지를 받는다는 의미입니다. 

* 여기서 웹 브라우저와 같은 DNS 클라이언트의 요청을 DNS로 전달하고 이로 부터 IP주소를 받아 클라이언트에게 제공하는 것을 DNS Resolver 혹은 Resolver라고 지칭합니다. 

* 단, 이러한 Resolver의 모든 기능을 PC와 같은 클라이언트 호스트로 구현하는 것은 단일 시스템 자원의 한계가 있기 때문에  Resolver의 대부분의 기능은 DNS에서 제공하고, 클라이언트 호스트에는 Resolver의 단순한 기능을 지니도록 구현되어 있습니다. 

  * 이러한 Resolver을 **Stub Resolver**라고 하며, 이를 통해 Resolver가 구현된 네임 서버의 IP주소만 파악하면 됩니다. 
  * 이는 OS 내부의 Socket 라이브러리를 활용하게 되며, 프로토콜 스택을 호출하여 메시지 송신 동작을 진행하게 됩니다. 
  * DNS로의 응답이 들어오면 프로토콜 스택에서 Resolver로 전달되고 최종적으로 애플리케이션에 IP주소가 전달됩니다. 

  ![](https://www.lifewire.com/thmb/q8pqtIQ3GUOm3mf2OyJ7sUPjgak=/768x0/filters:no_upscale():max_bytes(150000):strip_icc()/change-dns-servers-windows-10-598cdd4faf5d3a0011ebb997.png)


#### DNS 동작원리 

![](https://www.netmanias.com/ko/?m=attach&no=1997)

1. 브라우저에서 'www.naver.com'을 입력했을 때, PC는 미리 설정되어 있는 Local DNS에게 IP 주소를 물어봅니다. 
2. 만약 Local DNS에 호스트 네임에 대한 정보가 없을 경우 각 Local DNS에 설정된 Root DNS에 질의를 시작합니다. 
3. Root DNS는 전세계에 13대가 구축되어 있으며, 우리나라에는 3대의 미러 서버가 설정되어 있습니다. 
4. Root DNS에도 호스트에 대한 정보가 없으면, 다른 DNS 서버에게 질의할 수 있도록 요청합니다. (.com DNS)
5. Local DNS는 .com을 관리하는 DNS에게 호스트 네임에 대한 질의를 요청하고 요청한 결과가 없을 경우 다시 질의할 다른 DNS 서버의 주소를 알려줍니다. 
6. Local DNS는 naver.com을 관리하는 DNS에게 호스트 네임에 대한 질의를 요청하고 결과가 있을 경우 IP주소에 대한 결과를 반환합니다. 
7. Local DNS는 'www.naver.com'에 대한 IP 주소를 캐싱하고, 클라이언트에게 전달합니다. 

* 이와 같이 Local DNS 서버가 여러 DNS 서버를 차례대로 물어봐서 답을 찾는 과정을 **Recursive Query**라고 합니다. 



## DNS operation process

* 위에서 간단하게 DNS의 동작원리를 살펴보았으므로 이번에는 상세 동작원리를 살펴보도록 하겠습니다. 

* DNS 조회 메시지에는 다음 세 가지 정보가 포함되어 있습니다.

  * 이름
    * 서버나 메일배송 목적지와 같은 이름입니다. 
  * 클래스 
    * DNS의 구조를 고안했을 때 인터넷 이외에도 네트워크에서의 이용까지 검토하여 식별하기 위헤 클래스 정보가 있었습니다. 단, 지금은 인터넷 이외의 네트워크는 소멸되었기 때문에 클래스는 항상 인터넷을 나타내는 'IN' 입니다. 
  * 타입
    * 이름이 어떤 종류의 정보가 지원되는지를 나타냅니다. 예를 들어 A 타입일 경우 이름에 IP주소가 지원되는 것을 나타내고 MX이면 이름에 메일 배송 목적지가 지원된다는 것을 나타냅니다. 또한 이 타입에 따라 클라이언트의 회답하는 정보의 내용이 달라집니다. 

* 이러한 정보를 가지고 DNS서버에 조회를 하게 되면 DNS 서버는 등록된 정보를 찾아서 이름, 클래스, 타입의 세 가지가 일 치하는 것을 찾습니다. 

* 이들 세 가지 항목이 일치하면 등록된 IP 주소를 클라이언트에게 전송합니다. 

* 메일을 전송할 때는 MX 타입을 통해서 배송 목적지(@xxx.com)의 이름을 조회하게 됩니다. 

  * MX의 경우 메일 서버 IP 주소도 함께 클라이언트에게 전송합니다. 

* A, MX 타입 이외에도 IP 주소에서 이름을 조사할 때 사용하는 PTR 타입, 이름에 닉네임(alias)을 붙이기 위한 CNAME, DNS 서버의 IP 주소를 등록하는 NS, 도메인 자체의 속성 정보를 등록하는 SOA 등이 있습니다. 

  * 이를 **리소스 레코드**라고 합니다. 

  ![](https://krnic.or.kr/images/english/dns/imgDnsSys01_2018.png)

* DNS 서버는 여러군대에 분산되어, **도메인명**이라는 계층적 구조를 가진 이름이 있습니다. 
* 이렇게 계층화된 도메인의 정보를 서버에 등록하는데, 이때 하나의 도메인을 일괄적으로 취급합니다. 즉, 한 개의 도메인 정보를 일괄적으로 DNS서버에 등록하고 도메인 한 대의 정보를 분할하여 복수의 DNS 서버에 등록하는 것은 불가능 합니다. 
* 따라서 DNS 서버에 등록된 IP 주소를 찾기 위해서는 하위 도메인을 담당하는 DNS 서버의 IP 주소를 상위 DNS 서버에 등록합니다. 
* Root DNS 서버는 앞에서 언급했듯이 전세계적으로 13개 밖에 존재하지 않기 때문에 DNS 서버 소프트웨어를 설치하면 자동으로 등록이 완료됩니다. 이와 같이 계층별 DNS 서버가 도메인의 주소를 질의하면서 결과적으로 최종적인 IP 주소를 받아 올 수 있게 되며, 한 번 조사한 이름은 캐시에 저장하여 요청시 바로 응답할 수 있도록 합니다. 
  * 단, 캐시에 저장된 정보가 변경될 수 있기 때문에 캐시 안에 있는 정보는 항상 올바르다고 할 수 없습니다. 
  * 따라서 캐시에 저장한 유효기간이 지나면 캐시에서 삭제하게 되며, DNS 서버에서 응답할 시에는 캐시를 통해 응답한 것인지 등록처 DNS에서 응답했는지에 대한 정보도 같이 전달하게 됩니다. 





