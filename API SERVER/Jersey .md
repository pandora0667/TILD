#RESTful Web Services and Jersey

* RESTful API 웹 서비스는 웹에서 가장 잘 동작하도록 만들어진 서비스입니다. 
* Representational State Transfer (REST)는 일관된 인터페이스와 같은 제약 조건을 지정하는 아키텍쳐 스타일로서 웹 서비스에 적용하는 경우, 성능, 확장성 및 수정 가능성과 같은 바람직한 속성을 유도하여 웹에서 최상의 서비스 동작을 가능하게 합니다. 
* REST 아키텍쳐 스타일에서 데이터 및 기능은 리소스로 간주하며, URI (Uniform Resource Identifier)를 사용하여 접근합니다. 
  * URI는 일반적으로 웹에서 연결합니다. 
  * 리소스는 일련의 단순하고 잘 정의된 작업을 사용하여 수행합니다. 
* REST는 서버-클라이언트 아키텍쳐에 한하여 동작하며, HTTP와 같은 **상태를 저장하지 않는 통신 프로토콜**을 사용하도록 고안되었습니다. 
  * REST 스타일에서 클라이언트와 서버는 표준화된 인터페이스와 프로토콜을 사용하여 리소스 표현을 교환합니다. 
  * 이러한 원칙은 RESTful 어플리케이션이 단순하고 고성능이 되도록 합니다. 



![](https://steemitimages.com/DQmWFf63XnbB1w3CVWaQLT5NEh3oWTVVKhCM9n46gkzX7pM/restful-api-image.png)



* 엄격한 의미로 REST는 네트워크 아키텍쳐 원리의 모음이며, 여기서 ''네트워크 아키텍쳐 원리''란 자원을 정의하고 자원에 대한 주소를 지정하는 방법의 전반을 일컫습니다. 
* 간단한 의미로는, 웹상의 자료를 HTTP위에서 SOAP(Simple Object Access Protocol) - 일반적으로 널리 알려진 HTTP, HTTPS, SMTP 등을 통해 XML 기반의 메시지를 컴퓨터 네트워크 상에서 교환하는 프로토콜 이나 쿠키를 통한 세션 트랙킹 같은 별도의 전송계층 없이 전송하기 위한 아주 간단한 인터페이스입니다. 
* 필딩의 REST 아키텍쳐 형식을 따르면 HTTP나 WWW이 아닌 아주 커다란 소프트웨어 시스템을 설계하는 것도 가능하며, RPC 대신에 간단한 XML과 HTTP 인터페이스를 통해 설계하는 것도 가능합니다. 



## REST 아키텍쳐에 적용되는 6가지 조건 

1. 클라이언트 - 서버 : 일관적인 인터페이스로 분리되어야 한다. 
2. 무상태 - 각 요청은 클라이언트와 콘텍스트가 서버에 저장되어서는 안된다. 
3. 캐시 처리 가능 - WWW에서와 같이 클라이언트는 응답을 캐싱할 수 있어야 한다. 잘 관리되는 캐싱은 클라이언트 - 서버 간 상호작용을 부분적으로 또는 완전하게 제거하여 확장성과 성능을 향상시킨다. 
4. 계층화 - 클라이언트는 보통 대상 서버에 직접 연결되었는지, 또는 중간 서버를 통해 연결되어 있는지 알 수 없다. 중간 서버는 로드밸런싱 기능이나 공유 캐시 기능을 제공하여 시스템 규모 확장성을 향상시키는데 유용하다. 
5. Code on Demand (option) - 자바 애플릿이나 자바크립트의 제공을 통해 서버가 클라이언트가 실행시킬 수 있는 로직을 사용하여 기능을 확장시킬 수 있다. 
6. 인터페이스의 일관성 - 아키텍쳐를 단순화 시키고 작은 단위로 분리함으로서 클라이언트 - 서버의 각 파트가 독립적으로 개선될 수 있도록 해준다. 



## REST 인터페이스의 원칙에 대한 가이드 

#### 자원의 식별 

* 요청 내에 기술된 개별 자원을 식별할 수 있어야 한다. 웹 기반의 REST 시스템에서의 URI의 사용을 예로 들 수 있습니다. 자원 그 자체는 클라이언트가 받는 문서와는 개념적으로 분리되어 있어야 합니다. 예를 들어 서버는 DB 내부의 자료를 직접 전송하는 대신에 DB 레코드를 HTML, XML이나 JSON 등의 형식으로 전송합니다.

#### 메시지를 통한 리소스의 조작 

* 클라이언트가 어떤 자원을 지칭하는 메시지와 특정 메타데이터만 가지고 있다면 이것으로 서버 상의 해당 자원을 변경, 삭제할 수 있는 충분한 정보를 가지고 있는 것 입니다. 

#### 자기서술적 메시지 

* 각 메시지는 자신을 어떻게 처리해야 하는지에 대한 충분한 정보를 포함해야 합니다. 예를 들어 인터넷 미디어 타입을 전달한다면, 그 메시지에는 어떤 파서를 이용해야 하는지에 대한 정보도 포함되어야 합니다. 
* 미디어 타입만 가지고도, 클라이언트는 어떻게 그 내용을 처리해야 할지 알 수 있어야 합니다. 메시지를 이해하기 위해 그 내용까지 살펴봐야 한다면, 그 메시시지는 자기서술적이 아닙니다. 예를 들어 단순히 "application/xml"이라는 미디어 타입은, 실제 내용을 다운로드 받지 않으면 그 메시지만 가지고는 무엇을 해야할지에 대해 충분히 알려주지 못합니다. 

#### 애플리케이션의 상태에 대한 엔진으로서 하이퍼미디어 

* 만약에 클라인언트가 관련된 리소스에 접근하기를 원한다면, 리턴되는 지시자에서 구별될 수 있어야 합니다. 충분한 콘텍스트 속에서 URI를 제공해주는 하이퍼 텍스트 링크를 예로 들 수 있습니다. 

  

Mapping HTTP Methods to Operations Performed

|HTTP Method|Operation Performed|
|:---:|:---:|
|GET|리소스를 가져옵니다.|
|POST|정의된 의미가 없으면 자원을 생성합니다.|
|PUT|자원을 생성하거나 업데이트 합니다.|
|DELETE|자원을 삭제합니다.|

## How Does Jersey Fit In?

* Jersey는 Sun의 JSR 311에서 구현된 JAX-RS: RESTful 웹 서비스용 Java API입니다. 
* Jersey는 JSR-311에서 정의된 내용을 가지고 있기 때문에 개발자는 Java 및 Java JVM으로 RESTful 웹 서비스를 쉽게 작성할 수 있습니다. 
* Jersey는 JSR에서 지정하지 않은 추가 기능도 지원합니다. 



# RESTful Resource

## Creating a RESTful Resource Class

* 루트 리소스 클래스는 @Path로 어노테이션을 달거나 또는 @GET, @PUT, @POST, @DELETE와 같은 요청 메서드 지정자로 어노테이션이 하나 이상의 메서드가 있는 POJO(Plain Old Java Object)입니다. 

>**Plain Old Java Object**, 간단히 **POJO**는 말 그대로 해석을 하면 오래된 방식의 간단한 [자바](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94_(%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D_%EC%96%B8%EC%96%B4)) 오브젝트라는 말로서 [Java EE](https://ko.wikipedia.org/wiki/Java_EE) 등의 중량 [프레임워크](https://ko.wikipedia.org/wiki/%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC)들을 사용하게 되면서 해당 프레임워크에 종속된 "무거운" 객체를 만들게 된 것에 반발해서 사용되게 된 용어이다. 2000년 9월에 [마틴 파울러](https://ko.wikipedia.org/w/index.php?title=%EB%A7%88%ED%8B%B4_%ED%8C%8C%EC%9A%B8%EB%9F%AC&action=edit&redlink=1), 레베카 파슨, 조쉬 맥킨지 등이 사용하기 시작한 용어로서 마틴 파울러는 다음과 같이 그 기원을 밝히고 있다.
>
>"우리는 사람들이 자기네 시스템에 보통의 객체를 사용하는 것을 왜 그렇게 반대하는지 궁금하였는데, 간단한 객체는 폼 나는 명칭이 없기 때문에 그랬던 것이라고 결론지었다. 그래서 적당한 이름을 하나 만들어 붙였더니, 아 글쎄, 다들 좋아하더라고."
>
>​																			- 마틴 파울러
>
>POJO라는 용어는 이후에 주로 특정 [자바](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94_(%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D_%EC%96%B8%EC%96%B4)) 모델이나 기능, [프레임워크](https://ko.wikipedia.org/wiki/%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC) 등을 따르지 않은 자바 오브젝트를 지칭하는 말로 사용되었다. [스프링 프레임워크](https://ko.wikipedia.org/wiki/%EC%8A%A4%ED%94%84%EB%A7%81_%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC)는 POJO 방식의 프레임워크이다.
>
>즉 POJO = Java Beans 라고 생각해도 된다. 순수하게 getter, setter 메서드로 이루어진 Value Object의 Beans을 말한다. 



* 요청 메서드는 지정자로 어노테이션된 클래스의 메서드이며, 이번장에서는 Jersey를 사용하여 Java 객체에 어노테이션을 추가하여 RESTful 웹 서비스를 만드는 방법에 대해 설명합니다. 



## Developing RESTful Web Service with Jersey 

* RESTful  웹 서비스를 개발하기 위한 JAX-RS API는 REST 아키텍쳐를 사용하는 애플리케이션을 쉽게 개발할 수 있도록 설계된 Java 프로그램밍 언어 API입니다. 
* JAX-RS API는 Java 프로그래밍 언어 어노테이션을 사용하여 RESTful 웹 서비스 개발을 단순화 시킬 수 있습니다. 
* 개발자는 HTTP 관련 어노테이션을 사용하여, Java 프로그래밍 언어 클래스 파일을 작성하여 해당 자원에서 수행할 수 있는 작업을 정의합니다. 
* Jersey 어노테이션은 런타임 어노테이션이기 때문에 런타임 반영은 리소스에 대한 클래스와 아티팩트를 생성한 다음에 클래스 및 아티팩트 컬렉션을 WAR(Web Application Archive)에 작성합니다. 
* 자원은 Java EE 또는 웹 서버에 WAR를 배치하여 클라이언트에 노출됩니다. 



## Overview of a Jersey-Annotated Application 

* 다음 샘플은 JAX-RS 어노테이션을 사용하는 루트 자원 클래스의 아주 간단한 예제 입니다.

  ```java
  package com.sun.jersey.samples.helloworld.resources;
  
  import javax.ws.rs.GET;
  import javax.ws.rs.Produces;
  import javax.ws.rs.Path;
  
  // The Java class will be hosted at the URI path "/helloworld"
  @Path("/helloworld")
  public class HelloWorldResource {
      
      // The Java method will process HTTP GET requests
      @GET
      // The Java method will produce content identified by the MIME Media
      // type "text/plain"
      @Produces("text/plain")
      public String getClichedMessage() {
          // Return some cliched textual content
          return "Hello World";
      }
  }
  ```

## What Are Some of the Annotations Defined by JAX-RS?

|Annotations|Description|
| :---: | :---: |
|@Path| Java 클래스가 호스팅될 위치를 나타내는 상대 URI 경로입니다.  |
|@GET|이 요청 메서드는 지정자로 어노테이션된 Java 메서드는 HTTP GET 요청을 처리하며, 리소스의 동작은 응답하는 HTTP 메서드에 의해 결정됩니다.|
|@POST|이 요청 메서드는 지정자로 어노테이션된 Java 메서드는 HTTP POST 요청을 처리하며, 리소스의 동작은 응답하는 HTTP 메서드에 의해 결정됩니다.|
|@PUT|이 요청 메서드는 지정자로 어노테이션된 Java 메서드는 HTTP PUT 요청을 처리하며, 리소스의 동작은 응답하는 HTTP 메서드에 의해 결정됩니다.|
|@DELETE|이 요청 메서드는 지정자로 어노테이션된 Java 메서드는 HTTP DELETE 요청을 처리하며, 리소스의 동작은 응답하는 HTTP 메서드에 의해 결정됩니다.|
|@HEAD|이 요청 메서드는 지정자로 어노테이션된 Java 메서드는 HTTP HEAD 요청을 처리하며, 리소스의 동작은 응답하는 HTTP 메서드에 의해 결정됩니다.|
|@PathParam|리소스 클래스에서 사용할 수 있도록 추출할 수 있는 매개변수 유형입니다. URI 경로 매개변수는 요청 URI에서 추출되며, 매개변수의 이름은 @Path 클래스 어노테이션에서 지정된 URI 경로의 템플릿 변수 이름에 해당합니다.|
|@QueryParam|리소스 클래스에서 사용할 수 있도록 추출할 수 있는 매개변수 유형입니다. 쿼리 매개 변수는 요청 URI 쿼리 매개 변수에서 추출됩니다.|
|@Consumes|클라이언트가 보낸 리소스를 사용할 수 있는 표현의 MIME 미디어 유형을 지정하는 데 사용합니다.|
|@Produces|자원을 생성하여 클라이언트에 다시 보낼 수 있는 표현의 MIME 미디어 유형을 지정하는데 사용됩니다. (text/plain)|
|@Provider|MessageBodyReader 및 MessageBodyWriter와 같이 JAX-RS 런타임에 관심있는 모든 것에 사용됩니다. HTTP 요청의 경우 MessageBodyReader는 HTTP 요청 엔터티 본문을 메서드 매개 변수에 매핑하는 데 사용됩니다. 응답 측에서 반환 값은 MessageBodyWriter를 사용하여 HTTP 응답 엔터티 본문에 매핑됩니다. 응용 프로그램이 HTTP 헤더 나 다른 상태 코드와 같은 추가 메타 데이터를 제공해야하는 경우 메서드는 엔터티를 래핑하고 Response.ResponseBuilder를 사용하여 작성할 수있는 Response를 반환 할 수 있습니다.|

## The @Path Annotation and URI Path Templates

* @Path 어노테이션은 자원이 응답하는 URI 경로 템플릿를 식별하여 자원의 클래스에서 지정됩니다. 
* URI 경로 템플릿은 URI 구분 내에 변수가 포함된 URI이며, 이러한 변수는 자원이 대체된 URI를 기반으로 요청에 응답할 수 있도록 런타임시에 대체됩니다. 

>@Path ( "/ users / {username}")
>이 유형의 예에서는 사용자가 이름을 입력하라는 메시지를 표시 한 다음이 URI 경로 템플릿에 대한 요청에 응답하도록 구성된 Jersey 웹 서비스가 응답합니다. 예를 들어 사용자가 Galileo로 사용자 이름을 입력하면 웹 서비스가 다음 URL에 응답합니다.
>
>http://example.com/users/Galileo
>username 변수의 값을 얻으려면 다음 코드 예제와 같이 @PathParam annotation을 요청 메서드의 메서드 매개 변수에 사용할 수 있습니다.

```java
@Path("/users/{username}")
public class UserResource {

    @GET
    @Produces("text/xml")
    public String getUser(@PathParam("username") String userName) {
        ...
    }
}
```

* 사용자 이름을 대문자와 소문자로만 구성해야하는 경우 기본 정규 표현식을 대체하는 특정 정규 표현식을 선언 할 수 있습니다. 다음 예제는 이것이 @Path 어노테이션과 함께 어떻게 사용될 수 있는지 보여줍니다. 
  * @Path("users/{username: [a-zA-Z][a-zA-Z_0-9]}")
  * 이 유형의 예제에서 username 변수는 대소 문자와 0 개 이상의 영문자 및 밑줄 문자로 시작하는 사용자 이름 만 일치시킵니다. 사용자 이름이 해당 템플리트와 일치하지 않으면 404 (찾을 수 없음) 응답이 발생합니다.
* @Path 값은 슬래시 (/)로 시작하거나 시작하지 않을 수도 있지만 차이는 없습니다. 마찬가지로, @Path 값은 forward slash (/)로 끝날 수도 있고 끝나지 않을 수도 있습니다. 차이가 없기때문에 슬래시로 끝나지 않는 요청 URL이 모두 일치합니다. 그러나 Jersey에는 리디렉션 메커니즘이 있습니다. 리디렉션 메커니즘을 사용하면 요청 URL이 /로 끝나지 않고 일치하는 @Path가 /로 끝나는 경우 /로 끝나는 요청 URL로 리디렉션을 자동으로 수행합니다.



## More on URI Path Template Variables

* URI 경로 템플릿에는 하나 이상의 변수가 있으며 각 변수 이름은 중괄호로 묶여 {변수 이름을 시작하고} 끝나야합니다. 위의 예에서 username은 변수 이름입니다. 런타임에 위 URI 경로 템플릿에 응답하도록 구성된 리소스는 URI의 {username} 위치에 해당하는 URI 데이터를 username의 변수 데이터로 처리하려고 시도합니다.

* http://example.com/myContextRoot/jerseybeans/{name1}/{name2}/ 에 응답하는 리소스를 배포하려는 경우 응답하는 Java EE 서버에 WAR을 배포해야합니다. 

* http://example.com/myContextRoot URI 요청에 응답 한 다음 리소스를 다음 @Path 어노테이션을 작성하세요.

  ```java
  @Path("/{name1}/{name2}/")
  public class SomeResource {
  	...
  }
  ```

  ```xml
  <servlet-mapping>
  	  <servlet-name>My Jersey Bean Resource</servlet-name>
  	  <url-pattern>/jerseybeans/*</url-pattern>
  </servlet-mapping>
  ```

* 변수 이름은 URI 경로 템플리트에서 두 번 이상 사용될 수 있습니다.



Examples of URI path templates

|URI Path Template|URI After Substitution|
| :---: | :---: |
|`http://example.com/{name1}/{name2}/`|`http://example.com/jay/gatsby/`|
|`http://example.com/{question}/{question}/{question}/`|`http://example.com/why/why/why/`|
|`http://example.com/maps/{location}`|`http://example.com/maps/Main%20Street`|
|`http://example.com/{name3}/home/`|`http://example.com//home/`|

 



## The Request Method Designator Annotations

* 요청 메소드 지정자 어노테이션은 JAX-RS에 의해 정의되고 비슷한 이름의 HTTP 메소드에 해당하는 런타임 어노테이션입니다.

* 자원 클래스 파일 내에서 HTTP 메소드는 요청 메소드 지정 어노테이션을 사용하여 Java 프로그래밍 언어 메소드에 맵핑됩니다. 리소스의 동작은 리소스가 응답하는 HTTP 메서드 중 어떤 것으로 결정됩니다. Jersey는 일반적인 HTTP 메소드 인 @GET, @POST, @PUT, @DELETE, @HEAD에 대한 요청 메소드 지정자 세트를 정의하지만 사용자 정의 요청 메소드 지정자를 작성할 수 있습니다.

* 다음 예는 저장 영역 서비스 샘플에서 발췌 한 것으로 PUT 메소드를 사용하여 저장 영역 컨테이너를 작성하거나 갱신합니다.

  ```java
  @PUT
  public Response putContainer() {
      System.out.println("PUT CONTAINER " + container);
  
      URI uri =  uriInfo.getAbsolutePath();
      Container c = new Container(container, uri.toString());
  
      Response r;
      if (!MemoryStore.MS.hasContainer(c)) {
          r = Response.created(uri).build();
      } else {
          r = Response.noContent().build();
      }
  
      MemoryStore.MS.createContainer(c);
      return r;
  }
  ```

* 기본적으로 JAX-RS 런타임은 명시 적으로 구현되지 않은 경우 HEAD 및 OPTIONS 메소드를 자동으로 지원합니다. HEAD의 경우 런타임은 구현 된 GET 메소드 (있는 경우)를 호출하고 응답 엔티티 (설정된 경우)를 무시합니다. OPTIONS의 경우, 허용 응답 헤더는 자원이 지원하는 HTTP 메서드 세트로 설정됩니다. 또한 Jersey는 리소스를 설명하는 WADL 문서를 반환합니다.

>WADL (Web Application Description Language) - HTTP 기반 웹 애플리케이션 (전형적 REST용 웹 서비스)의 기계가 읽을 수 있는  XML의 설명 입니다. 
>
>WADL은 서비스에서 제공하는 리소스와 해당 서비스 간의 관계를 모델링 하며, 웹의 기존 HTTP 아키텍쳐를 기반으로 하는 웹 서비스의 재사용성을 단순화 하기 위한 것입니다. 



* 요청 메소드 지정자로 장식 된 메서드는 void, Java 프로그래밍 언어 유형 또는 javax.ws.rs.core.Response 객체를 리턴해야합니다. 요청 매개 변수 추출에 설명 된대로 PathParam 또는 QueryParam 어노테이션을 사용하여 URI에서 여러 매개 변수를 추출 할 수 있습니다. Java 유형과 엔티티 본문 간의 변환은 MessageBodyReader 또는 MessageBodyWriter와 같은 엔티티 제공자의 책임입니다.
* 추가 메타 데이터에 응답을 제공해야하는 메서드는 Response 인스턴스를 반환해야합니다. ResponseBuilder 클래스는 빌더 패턴을 사용하여 Response 인스턴스를 작성하는 편리한 방법을 제공합니다. HTTP PUT 및 POST 메소드는 HTTP 요청 본문을 필요로하므로 PUT 및 POST 요청에 응답하는 메소드에 MessageBodyReader를 사용해야합니다.



## Using Entity Providers to Map HTTP Response and Request Entity Bodies

* 엔티티 프로 바이더는, 표현과 그 관련의 Java 형간에 매핑 서비스를 제공합니다. 엔티티 공급자에는 MessageBodyReader와 MessageBodyWriter라는 두 가지 유형이 있습니다. 

* HTTP 요청의 경우 MessageBodyReader는 HTTP 요청 엔터티 본문을 메서드 매개 변수에 매핑하는 데 사용됩니다. 응답 측에서 반환 값은 MessageBodyWriter를 사용하여 HTTP 응답 엔터티 본문에 매핑됩니다. 응용 프로그램이 HTTP 헤더 나 다른 상태 코드와 같은 추가 메타 데이터를 제공해야하는 경우 메서드는 엔터티를 래핑하고 Response.ResponseBuilder를 사용하여 작성할 수있는 Response를 반환 할 수 있습니다.

* 다음 목록은 엔티티에 대해 자동으로 지원되는 표준 유형을 포함합니다. 다음 표준 유형 중 하나를 선택하지 않은 경우 엔터티 공급자 만 작성하면됩니다.

  * byte[] — All media types (*/*)
  * java.lang.String — All text media types (text/*)
  * java.io.InputStream — All media types (*/*)
  * java.io.Reader — All media types (*/*)
  * java.io.File — All media types (*/*)
  * javax.activation.DataSource — All media types (*/*)
  * javax.xml.transform.Source — XML types (text/xml, application/xml and application/*+xml)
  * javax.xml.bind.JAXBElement and application-supplied JAXB classes XML media types (text/xml, application/xml and application/*+xml)
  * MultivaluedMap<String, String> — Form content (application/x-www-form-urlencoded)
  * StreamingOutput — All media types (*/*), MessageBodyWriter only

  다음 예제는 MessageBodyReader를 @Consumes 및 @Provider 어노테이션과 함께 사용하는 방법을 보여줍니다.

  ```java
  @Consumes("application/x-www-form-urlencoded")
  @Provider
  public class FormReader implements MessageBodyReader<NameValuePair> {
      
  // 다음은 MessageBodyWriter를 @Produces 및 @Provider 어노테이션과 함께 사용하는 방법을 보여줍니다.
  
  @Produces("text/html")
  @Provider
  public class FormWriter implements MessageBodyWriter<Hashtable<String, String>> {
  // The following example shows how to use ResponseBuilder:
  
      @GET
      public Response getItem() {
          System.out.println("GET ITEM " + container + " " + item);
          
          Item i = MemoryStore.MS.getItem(container, item);
          if (i == null)
              throw new NotFoundException("Item not found");
          Date lastModified = i.getLastModified().getTime();
          EntityTag et = new EntityTag(i.getDigest());
          ResponseBuilder rb = request.evaluatePreconditions(lastModified, et);
          if (rb != null)
              return rb.build();
              
          byte[] b = MemoryStore.MS.getItemData(container, item);
          return Response.ok(b, i.getMimeType()).
                  lastModified(lastModified).tag(et).build();
      }    
     
  ```



## Using @Consumes and @Produces to Customize Requests and Responses

* 리소스로 전송 된 다음 클라이언트로 다시 전달되는 정보는 HTTP 요청 또는 응답의 헤더에 MIME 미디어 유형으로 지정됩니다. javax.ws.rs.Consumes 및 javax.ws.rs.Produces 어노테이션을 사용하여 자원이 응답하거나 생성 할 수있는 표현의 MIME 미디어 유형을 지정할 수 있습니다.
* 기본적으로 자원 클래스는 HTTP 요청 및 응답 헤더에 지정된 표현의 모든 MIME 미디어 유형에 응답하고 생성 할 수 있습니다.



### The @Produces Annotation

* @Produces 어노테이션은 자원이 생성하여 클라이언트로 다시 보낼 수있는 MIME 미디어 유형 또는 표현을 지정하는 데 사용됩니다. @Produces가 클래스 수준에서 적용되면 리소스의 모든 메서드는 기본적으로 지정된 MIME 형식을 생성 할 수 있습니다. 메서드 수준에서 적용되면 클래스 수준에서 적용된 모든 @Produces 어노테이션을 재정의합니다.

* 자원의 메소드가 클라이언트 요청에서 MIME 유형을 생성 할 수없는 경우 Jersey 런타임은 HTTP "406 Not Acceptable"오류를 다시 보냅니다.

  ```java
  @Produces({"image/jpeg,image/png"})
  The following example shows how to apply @Produces at both the class and method levels:
  
  @Path("/myResource")
  @Produces("text/plain")
  public class SomeResource {
  	@GET
  	public String doGetAsPlainText() {
  		...
  	}
  
  	@GET
  	@Produces("text/html")
  	public String doGetAsHtml() {
  		...
  	}
  }
  ```

* doGetAsPlainText 메소드는 기본적으로 클래스 수준에서 @Produces 어노테이션의 MIME 미디어 유형으로 설정됩니다. doGetAsHtml 메서드의 @Produces 어노테이션은 클래스 수준 @Produces 설정을 재정의하고 메서드가 일반 텍스트가 아닌 HTML을 생성 할 수 있도록 지정합니다.

* 리소스 클래스가 하나 이상의 MIME 미디어 유형을 생성 할 수있는 경우 선택한 리소스 방법은 클라이언트가 선언 한 가장 적합한 미디어 유형에 해당합니다. 보다 구체적으로, HTTP 요청의 Accept 헤더는 가장 수용 가능한 것을 선언했습니다. 예를 들어 Accept 헤더가 Accept : text / plain이면 doGetAsPlainText 메소드가 호출됩니다. 또는 Accept 헤더가 Accept : text / plain; q = 0.9, text / html으로 클라이언트가 text / plain 및 text / html의 미디어 유형을 허용 할 수 있지만 후자를 선호한다고 선언하면 doGetAsHtml 메소드가 호출됩니다 .

* 하나 이상의 미디어 유형이 동일한 @Produces 선언에 선언 될 수 있습니다. 다음 코드 예제는 이것이 어떻게 수행되는지 보여줍니다.

  ```java
  @Produces({"application/xml", "application/json"})
  public String doGetAsXmlOrJson() {
  	...
  }
  ```

* doGetAsXmlOrJson 메소드는 application / xml 및 application / json 중 하나의 미디어 유형이 수용 가능한 경우 호출됩니다. 둘 다 똑같이 받아 들일 수 있다면, 먼저 발생하기 때문에 전자를 선택하게됩니다. 위의 예는 명확성을 위해 MIME 미디어 유형을 명시 적으로 나타냅니다. typographical errors를 줄일 수있는 상수 값을 참조하는 것이 가능합니다. 자세한 내용은 MediaType의 상수 필드 값을 참조하십시오.

### The @Consumes Annotation

* @Consumes 어노테이션은 리소스가 클라이언트에서 받아 들일 수 있거나 소비 할 수있는 표현의 MIME 미디어 유형을 지정하는 데 사용됩니다. @Consumes가 클래스 수준에서 적용되면 모든 응답 메서드는 기본적으로 지정된 MIME 형식을 허용합니다. @Consumes가 메소드 레벨에서 적용되면, 클래스 레벨에서 적용된 @Consumes 어노테이션을 대체합니다.

* 자원이 클라이언트 요청의 MIME 유형을 사용할 수 없으면 Jersey 런타임은 HTTP "415 지원되지 않는 매체 유형"오류를 되돌려 보냅니다.

* @Consumes의 값은 허용되는 MIME 유형의 String 배열입니다. -> @Consumes({"text/plain,text/html"})

* 다음 예제에서는 클래스 및 메서드 수준 모두에서 @Consumes를 적용하는 방법을 보여줍니다.

  ```java
  @Path("/myResource")
  @Consumes("multipart/related")
  public class SomeResource {
  	@POST
  	public String doPost(MimeMultipart mimeMultipartData) {
  		...
  	}
  
  	@POST
  	@Consumes("application/x-www-form-urlencoded")
  	public String doPost2(FormURLEncodedProperties formData) {
  		...
  	}
  }
  ```

* doPost 메소드의 기본값은 클래스 수준에서 @Consumes 어노테이션의 MIME 미디어 유형입니다. doPost2 메소드는 클래스 레벨 @Consumes 어노테이션을 겹쳐 쓰며 URL 인코딩 된 양식 데이터를 허용 할 수 있도록 지정합니다.

* 요청 된 MIME 형식에 응답 할 수있는 리소스 메서드가 없으면 HTTP 415 오류 (지원되지 않는 미디어 형식)가 클라이언트에 반환됩니다.

* 이 섹션의 앞 부분에서 설명한 HelloWorld 예제는 다음 코드 예제와 같이 @Consumes를 사용하여 완곡 한 메시지를 설정하도록 수정할 수 있습니다.

  ```java
  @POST
  @Consumes("text/plain")
  public void postClichedMessage(String message) {
      // Store the message
  }
  ```

* 이 예제에서 Java 메소드는 MIME 미디어 유형 text / plain으로 식별되는 표현을 사용합니다. 리소스 메서드가 void를 반환합니다. 즉, 표현이 반환되지 않고 상태 코드가 HTTP 204 (No Content) 인 응답이 반환됩니다.



## Extracting Request Parameters

* 자원 메소드의 매개 변수에는 매개 변수 기반 어노테이션으로 어노테이션을 달아 요청에서 정보를 추출 할 수 있습니다. 이전 예제에서는 @PathParam 매개 변수를 사용하여 @Path에 선언 된 경로와 일치하는 요청 URL의 경로 구성 요소에서 경로 매개 변수를 추출했습니다. 

* 리소스 클래스에서 사용할 수 있도록 추출 할 수있는 매개 변수에는 쿼리 매개 변수, URI 경로 매개 변수, 폼 매개 변수, 쿠키 매개 변수, 헤더 매개 변수 및 행렬 매개 변수의 여섯 가지 유형이 있습니다.

* 쿼리 매개 변수는 요청 URI 쿼리 매개 변수에서 추출되며 메서드 매개 변수 인수에서 javax.ws.rs.QueryParam 어노테이션을 사용하여 지정됩니다. 다음 예제 (sparklines 샘플 응용 프로그램)는 @QueryParam을 사용하여 요청 URL의 쿼리 구성 요소에서 쿼리 매개 변수를 추출하는 방법을 보여줍니다.

  ```java
  @Path("smooth")
  @GET
  public Response smooth(
          @DefaultValue("2") @QueryParam("step") int step,
          @DefaultValue("true") @QueryParam("min-m") boolean hasMin,
          @DefaultValue("true") @QueryParam("max-m") boolean hasMax,
          @DefaultValue("true") @QueryParam("last-m") boolean hasLast,           
          @DefaultValue("blue") @QueryParam("min-color") ColorParam minColor,
          @DefaultValue("green") @QueryParam("max-color") ColorParam maxColor,
          @DefaultValue("red") @QueryParam("last-color") ColorParam lastColor
          ) { ... }
  ```

* 쿼리 매개 변수 "step"이 요청 URI의 쿼리 구성 요소에 있으면 "step"값이 추출되고 32 비트 부호있는 정수로 구문 분석되어 step 메서드 매개 변수에 할당됩니다. "step"가 존재하지 않으면 @DefaultValue 어노테이션에 선언 된대로 2의 기본값이 단계 메소드 매개 변수에 지정됩니다. "step"값을 32 비트 부호있는 정수로 구문 분석 할 수없는 경우 HTTP 400 (클라이언트 오류) 응답이 반환됩니다.

* ColorParam과 같은 사용자 정의 Java 유형을 사용할 수 있습니다. 다음 코드 예제에서는이를 구현하는 방법을 보여줍니다.

  ```java
  public class ColorParam extends Color {
      public ColorParam(String s) {
          super(getRGB(s));
      }
  
      private static int getRGB(String s) {
          if (s.charAt(0) == '#') {
              try {
                  Color c = Color.decode("0x" + s.substring(1));
                  return c.getRGB();
              } catch (NumberFormatException e) {
                  throw new WebApplicationException(400);
              }
          } else {
              try {
                  Field f = Color.class.getField(s);
                  return ((Color)f.get(null)).getRGB();
              } catch (Exception e) {
                  throw new WebApplicationException(400);
              }
          }
      }
  }
  ```

* @QueryParam 및 @PathParam은 다음 자바 유형에서만 사용할 수 있습니다.

  * char를 제외한 모든 기본 유형
  * Character를 제외하는 원시적 형의 모든 래퍼 클래스
  * 단일 String 인수를 허용하는 생성자
  * 단일 String 인수를 허용하는 valueOf (String)라는 정적 메서드가있는 모든 클래스
  * 단일 String을 매개 변수로 사용하는 생성자가있는 모든 클래스

* @DefaultValue가 @QueryParam과 함께 사용되지 않고 쿼리 매개 변수가 요청에 없으면 value는 List, Set 또는 SortedSet에 대한 빈 컬렉션입니다. 다른 오브젝트 형의 경우는 기본 유형에 대한 Java의 정의 값은 null입니다. 

* URI 경로 매개 변수는 요청 URI에서 추출되며 매개 변수 이름은 @Path 클래스 수준 어노테이션에 지정된 URI 경로 템플리트 변수 이름에 해당합니다. URI 매개 변수는 method 매개 변수 인수의 javax.ws.rs.PathParam 어노테이션을 사용하여 지정됩니다. 다음 예제는 메서드에서 @Path 변수와 @PathParam 어노테이션을 사용하는 방법을 보여줍니다.

  ```java
  @Path("/{userName}")
  public class MyResourceBean {
  	...
  	@GET
  	public String printUserName(@PathParam("userName") String userId) {
  		...
  	}
  }
  ```

* 위의 URI 경로 템플리트 변수 이름 userName은 printUserName 메소드에 대한 매개 변수로 지정됩니다. @PathParam 어노테이션은 변수 이름 userName으로 설정됩니다. 런타임시 printUserName이 호출되기 전에 userName 값이 URI에서 추출되고 String으로 캐스팅됩니다. 그런 다음 결과 문자열을 userId 변수로 메서드에서 사용할 수 있습니다.

* URI 경로 템플릿 변수를 지정된 유형으로 변환 할 수없는 경우 Jersey 런타임은 HTTP 400 잘못된 요청 오류를 클라이언트에 반환합니다. @PathParam 어노테이션을 지정된 유형으로 변환 할 수없는 경우 Jersey 런타임은 HTTP 404 Not Found 오류를 클라이언트에 반환합니다.

* @PathParam 매개 변수와 다른 매개 변수 기반 어노테이션 인 @MatrixParam, @HeaderParam, @CookieParam 및 @FormParam은 @QueryParam과 동일한 규칙을 따릅니다.

* 쿠키 매개 변수 (javax.ws.rs.CookieParam)는 쿠키 관련 HTTP 헤더에 선언 된 쿠키에서 정보를 추출합니다. 헤더 매개 변수 (javax.ws.rs.HeaderParam)는 HTTP 헤더에서 정보를 추출합니다. 행렬 매개 변수 (매개 변수를 javax.ws.rs.MatrixParam)는 URL 경로 세그먼트에서 정보를 추출합니다. 이 매개 변수는이 자습서의 범위를 벗어납니다.

* (javax.ws.rs.FormParam) 양식 매개 변수는 MIME 매체 유형 application / x-www-form-urlencoded이고 HTML 양식에 지정된 인코딩을 따르는 요청 표현에서 정보를 추출합니다. 

* 이 매개 변수는 HTML 양식에 게시 된 정보를 추출하는 데 매우 유용합니다. 다음 예제에서는 POST 된 양식 데이터에서 "name"이라는 양식 매개 변수를 추출합니다.

  ```java
  @POST
  @Consumes("application/x-www-form-urlencoded")
  public void post(@FormParam("name") String name) {
      // Store the message
  }
  ```

* 값에 대한 매개 변수 이름의 일반 맵을 가져 오는 것이 필요한 경우 쿼리 및 경로 매개 변수에 다음 예제와 같은 코드를 사용합니다. 

  ```java
  @GET
  public String get(@Context UriInfo ui) {
      MultivaluedMap<String, String> queryParams = ui.getQueryParameters();
      MultivaluedMap<String, String> pathParams = ui.getPathParameters();
  }
  
  // 헤더 및 쿠키 매개 변수에 대한 코드는 다음과 같습니다. 
  
  @GET
  public String get(@Context HttpHeaders hh) {
      MultivaluedMap<String, String> headerParams = hh.getRequestHeaders();
      Map<String, Cookie> pathParams = hh.getCookies();
  }
  
  // 일반적으로, @Context는, 요구 또는 응답에 관계하는 문맥상의 Java 형을 취득하기 위해서 사용할 수 있습니다.
  // 양식 매개 변수의 경우 다음을 수행 할 수 있습니다.
  
  @POST
  @Consumes("application/x-www-form-urlencoded")
  public void post(MultivaluedMap<String, String> formParams) {
      // Store the message
  }
  ```



---



## 부연설명

**세션 트랙킹** 

* 세션 트래킹(Session Tracking)은 좁은 의미로 요청된 세션을 찾아 주는 동작이다. 클러스터링 환경에서 세션 트래킹의 지원 범위(Scope)에 따라 라우팅(Routing)에 의한 세션 트래킹과 세션 서버(Session Server)를 통한 세션 트래킹으로 구분된다. 

> 본 설명은 TMAX 사의 JEUS 프로그램에서 지원하는 세션 트랙킹 동작에 관한 설명입니다. 

![](https://technet.tmaxsoft.com/upload/download/online/jeus/pver-20140827-000001/session/resources/session-tracking/01_architecture_of_web_engine_with_session.png)

* HTTP 세션은 동일한 클라이언트(예를 들면 웹 브라우저)의 HTTP 요청과 연관된 일련의 작업들이다. 여러 클라이언트가 이런 요청을 표준 HTTP를 통하여 보낼 때 기본적으로 HTTP 헤더에 유일한 "Client ID"가 없기 때문에 웹 서버는 이 클라이언트들을 구별할 수 없는 문제가 발생한다.
* 따라서 웹 서버는 관련된 사용자 요청을 하나의 세션으로 트래킹할 수 없다. 이것은 HTTP가 무상태(stateless)와 무연결(connectionless) 프로토콜이기 때문이다.
* HTTP 요청은 다음과 같이 동작합니다. 
  * 클라이언트는 웹 서버에 연결한다. 
  * 클라이언트는 무상태 HTTP 요청을 생성한다. 
  * 클라이언트는 응답을 받는다. 
  * HTTP 연결이 끊어진다. 
* "Client ID"나 지속적인 세션의 개념은 HTTP 프로토콜에 포함되어 있지 않다. 따라서 웹 서버는 HTTP 연결이 끊어지거나 요청에 대한 응답 수신을 종료한 순간, 각 요청에 대한 사항들을 잃어버린다(단일 요청의 경우 발생). 그렇기 때문에 복잡한 웹 애플리케이션에서 사용자가 지속적으로 서로 연관된 요청을 수행하는 경우에는 사용이 불가능하다.
* 이 문제를 극복하기 위해 세션 ID라는 특수 string을 각 HTTP 요청에 추가한다. 이 유일한 ID는 클라이언트가 최초 요청을 할 때 필요에 따라 생성되어 클라이언트에 전달된다. 이후 클라이언트의 요청에 이 세션 ID가 지속적으로 붙여진다. 이러한 과정을 통해서 웹 엔진은 각 요청의 근원을 파악할 수 있기 때문에 사용자가 거래를 하는 동안에 대화성 상태(Conversational state)를 유지할 수 있고, 세션의 기능이 없는 HTTP 프로토콜을 이용하여 세션의 개념을 지원할 수 있다.
* 세션 ID는 쿠키 또는 클라이언트에게 반환되는 HTML 페이지의 각 URL 링크의 파라미터로 자동 추가되고 이것을 URL Rewriting이라고 한다. 또 다른 옵션은 hidden field로 HTML 폼에 세션 ID를 저장하는 방법이 있다. 물론 서블릿 프로그래머의 관점에서 이 논점들은 강력한 Servlet API에 의해 모두 구현되는 것들이다. 

>본 절에서는 JEUS 웹 엔진에서의 세션 트래킹 동작 방식에 대해 설명한다. 
>
>JEUS 웹 엔진은 세션 트래킹을 가능하게 하기 위해 URL Rewriting과 쿠키를 지원하고, 그 중에서 기본적으로 쿠키가 사용된다. 세션 ID를 운반할 때 사용되는 쿠키를 세션 쿠키(Session Cookie)라고 한다.
>
>웹 엔진에서 하나의 세션은 하나의 Servlet API인 HTTP 세션 클래스의 인스턴스로 표현된다. 이 인스턴스는 세션 쿠키(또는 URL Rewriting의 결과로 생성된 URL 파라미터)의 세션 ID와 연관되어 있다. HTTP 세션 객체는 기본적으로 그것을 생성하는 웹 엔진에 존재한다. 또한 사용자 선호 성향이나 사용자가 전자 상거래 사이트에서 구매하고 싶은 물품의 목록 등과 같은 사용자에 대한 데이터를 가지고 있다.
>
>URL Rewriting은 여기서 설명한 기술의 개념과 유사한 것이다. 단, 세션 ID가 HTML 페이지의 URL 링크에 포함된다는 것이 세션 쿠키가 별도의 쿠키 헤더(Cookie Header)에 포함된다는 것과 다른 점이다.



* 세션 쿠키는 하나의 클라이언트가 JEUS 웹 엔진이 관리하는 리소스를 요청하는 과정에서 사용한다. 

* 다음은 웹 엔진이 세션 쿠키를 발급하는 과정에 대한 설명이다. 

  ![](https://technet.tmaxsoft.com/upload/download/online/jeus/pver-20140827-000001/session/resources/session-tracking/02_initializing_a_session_with_a_single_web_engine_using_a_session_id_cookie.png)

  1. 클라이언트는 웹 서버에 초기 요청을 한다.
  2. 웹 서버는 웹 엔진에 요청을 전달한다.
  3. 웹 엔진은 해당 클라이언트를 위한 HTTP 세션 객체와 해당 HTTP 세션의 세션 ID를 지닌 세션 쿠키를 생성한다. 이렇게 생성한 세션 ID는 동일한 클라이언트가 지속적인 요청을 할 때 메모리에서 생성한 HTTP 세션 객체를 가져오기 위해 사용된다.
  4. 응답 데이터와 세션 쿠키가 웹 서버에 전달된다.
  5. 세션 쿠키는 클라이언트의 웹 브라우저에 응답과 함께 전달되고 HTTP 연결은 끊어진다.
  6. 세션 ID를 포함하는 세션 쿠키는 클라이언트의 웹 브라우저에 저장된다.

* 이제 클라이언트는 세션 쿠키를 갖게 되었고, 동일한 웹 엔진에 또 다른 요청할 경우에 쿠키를 포함해서 보낼 수 있다. 웹 엔진은 쿠키의 세션 ID를 통해 클라이언트를 인식할 수 있고, 클라이언트의 HTTP 세션 객체를 가져올 수 있다.

* 다음은 이 과정에 대한 설명이다. 

  ![](https://technet.tmaxsoft.com/upload/download/online/jeus/pver-20140827-000001/session/resources/session-tracking/03_making_a_second_request_to_the_same_web_engine_using_previously_stored_session.png)

  1. 클라이언트는 동일한 웹 서버에게 또 다른 요청을 보낸다. 이번에는 이전에 받은 세션 쿠키를 웹 브라우저에서 받아 요청에 첨부한다.
  2. 웹 서버는 세션 쿠키가 포함된 요청을 받아서 처음의 요청과 같이 동일한 웹 엔진에 전달한다.
  3. 웹 엔진은 요청과 세션 쿠키를 받는다. 세션 쿠키에서 발견된 세션 ID에 해당하는 HTTP 세션 객체를 자신의 메모리 영역에서 찾아온다. 웹 엔진은 찾아온 HTTP 세션 데이터를 사용하여 요청을 처리한다.
  4. 응답 데이터와 세션 쿠키는 웹 서버에 전달된다.
  5. HTTP 응답이 웹 브라우저에게 전달되고 HTTP 연결은 끊어진다. 세션 쿠키는 연결이 처음 생성될 때에만 응답에 포함되어 전달되면 되고, 그 이후의 응답에는 함께 전달될 필요는 없으므로 이 응답에 쿠키가 함께 전달될 필요는 없다.



출처 : https://technet.tmaxsoft.com/upload/download/online/jeus/pver-20140827-000001/session/chapter_session_tracking.html



---



**MINE**

* **MIME 타입**이란 클라이언트에게 전송된 문서의 다양성을 알려주기 위한 메커니즘입니다: 웹에서 파일의 확장자는 별  의미가 없습니다. 그러므로, 각 문서와 함께 올바른 MIME 타입을 전송하도록, 서버가 정확히 설정하는 것이 중요합니다. 브라우저들은 리소스를 내려받았을 때 해야 할 기본 동작이 무엇인지를 결정하기 위해 대게 MIME 타입을 사용합니다.
* 수 많은 종류의 문서가 있으므로 많은 MIME 타입들이 존재합니다. 우리는 이 글에서 웹 개발에 있어 가장 중요한 MIME 타입들 중 몇 가지를 나열해 살펴보긴 하겠지만, 특정 종류의 문서에 딱 들어맞는 것을 보려면 다음의 전용 문서를 참고하시기 바랍니다: [MIME 타입의 전체 목록](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types/Complete_list_of_MIME_types).
* MIME 타입의 구조는 매우 간단합니다; `'/'`로 구분된 두 개의 문자열인 타입과 서브타입으로 구성됩니다. 스페이스는 허용되지 않습니다. *type*은 카테고리를 나타내며 *개별(discrete) 혹은* *멀티파트* 타입이 될 수 있습니다. *subtype* 은 각각의 타입에 한정됩니다.
  * type/subtype
* 개별 타입 
  * text/plain 
  * text/html 
  * image/jpeg 
  * image/png 
  * audio/mpeg
  * audio/ogg 
  * audio/* 
  * video/mp4 
  * application/octet-stream
  *  …

|타입|설명|일반적인 서브타입 예시|
| :---: | :---: | :---: |
|text| 텍스트를 포함하는 모든 문서를 나타내며 이론상으로는 인간이 읽을 수 있어야 합니다 |`text/plain`, `text/html`,`text/css, text/javascript`|
|image| 모든 종류의 이미지를 나타냅니다. (animated gif처럼) 애니메이션되는 이미지가 이미지 타입에 포함되긴 하지만, 비디오는 포함되지 않습니다.|`image/gif`, `image/png`, `image/jpeg`, `image/bmp`, `image/webp`|
|audio|모든 종류의 오디오 파일들을 나타냅니다.|`audio/midi`, `audio/mpeg, audio/webm, audio/ogg, audio/wav`|
|video|모든 종류의 비디오 파일들을 나타냅니다.|`video/webm`, `video/ogg`|
|application|모든 종류의 이진 데이터를 나타냅니다.|`application/octet-stream`, `application/pkcs12`, `application/vnd.mspowerpoint`, `application/xhtml+xml`, `application/xml`,  `application/pdf`|

* 특정 서브타입이 없는 텍스트 문서들에 대해서는 `text/plain`가 사용되어야 합니다. 특정 혹은 알려진 서브타입이 없는 이진 문서에 대해서는 유사하게, `application/octet-stream`이 사용되어야 합니다.

   

출처 : https://developer.mozilla.org/ko/docs/Web/HTTP/Basics_of_HTTP/MIME_types