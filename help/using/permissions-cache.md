---
title: 보안 컨텐츠 캐싱
seo-title: AEM Dispatcher에서 보안 컨텐츠 캐싱
description: Dispatcher에서 권한 감지 캐싱 작동 방식을 살펴볼 수 있습니다.
seo-description: AEM Dispatcher에서 권한 감지 캐싱이 작동하는 방식을 알아봅니다.
uuid: abfed 68 a -2 efe -45 f 6-bdf 7-2284931629 d 6
contentOwner: 사용자
products: sg_ Experiencemanager/dispatcher
topic-tags: Dispatcher
content-type: 참조
discoiquuid: 4 f 9 b 2 bc 8-a 309-47 bc-b 70 d-a 1 c 0 da 78 d 464
translation-type: tm+mt
source-git-commit: f35c79b487454059062aca6a7c989d5ab2afaf7b

---


# 보안 컨텐츠 캐싱 {#caching-secured-content}

권한 감지 캐싱을 사용하면 보안 페이지를 캐시할 수 있습니다. Dispatcher는 캐시된 페이지를 배달하기 전에 페이지에 대한 사용자의 액세스 권한을 확인합니다.

Dispatcher 에는 권한 감지 캐시를 구현하는 authchecker 모듈이 포함되어 있습니다. 모듈이 활성화되면 렌더링은 AEM 서블릿을 호출하여 요청된 컨텐츠에 대한 사용자 인증 및 권한을 수행합니다. 서블릿 응답은 컨텐츠가 웹 브라우저에 배달되는지 여부를 결정합니다.

인증 및 권한 부여 방법은 AEM Deployment 에만 해당되므로 서블릿을 만들어야 합니다.

>[!NOTE]
>
>필터를 사용하여 `deny` 보안 제한 사항을 적용합니다. 사용자 또는 그룹의 하위 집합에 대한 액세스를 허용하도록 구성된 페이지에 대해 권한 감지 캐시를 사용합니다.

다음 다이어그램은 웹 브라우저가 권한 감지 캐싱이 사용되는 페이지를 요청할 때 발생하는 이벤트 순서를 보여줍니다.

## 페이지가 캐시되고 사용자가 인증됨 {#page-is-cached-and-user-is-authorized}

![](assets/chlimage_1.png)

1. Dispatcher는 요청된 컨텐츠가 캐시되고 유효하다고 판단합니다.
1. Dispatcher는 렌더링에 요청 메시지를 보냅니다. HEAD 섹션에는 브라우저 요청의 모든 헤더 라인이 포함됩니다.
1. 렌더링은 Authorizer를 호출하여 보안 검사를 수행하고 Dispatcher에 응답합니다. 응답 메시지에는 사용자가 인증되었음을 나타내는 HTTP 상태 코드가 200 개 포함됩니다.
1. Dispatcher는 렌더링 응답의 헤더 라인과 본문에 있는 캐시된 컨텐츠로 구성된 브라우저에 응답 메시지를 보냅니다.

## 페이지가 캐시되지 않고 사용자가 인증됨 {#page-is-not-cached-and-user-is-authorized}

![](assets/chlimage_1-1.png)

1. Dispatcher는 컨텐츠가 캐시되지 않았거나 업데이트가 필요했다고 판단합니다.
1. Dispatcher는 원래 요청을 렌더링으로 전달합니다.
1. 렌더링은 Authorizer 서블릿을 호출하여 보안 검사를 수행합니다. 사용자가 승인되면 렌더링에 응답 메시지의 본문에 렌더링된 페이지가 포함됩니다.
1. 디스패처가 브라우저로 응답을 전달합니다. Dispatcher가 렌더링에 응답 메시지의 본문을 추가합니다.

## 사용자가 인증되지 않음 {#user-is-not-authorized}

![](assets/chlimage_1-2.png)

1. Dispatcher에서 캐시를 확인합니다.
1. Dispatcher는 브라우저의 요청으로부터 모든 헤더 라인을 포함하는 렌더링에 요청 메시지를 보냅니다.
1. 렌더링은 Authorizer 서블릿을 호출하여 실패한 보안 검사를 수행하고 렌더링은 원래 요청을 디스패처로 전달합니다.

## 권한 감지 캐싱 구현 {#implementing-permission-sensitive-caching}

권한 감지 캐시를 구현하려면 다음 작업을 수행하십시오.

* 인증 및 인증을 수행하는 서블릿 개발
* Dispatcher 구성

>[!NOTE]
>
>일반적으로 보안 리소스는 비보안 파일이 아닌 별도의 폴더에 저장됩니다. 예: /content/secure/


## 인증 서블릿 만들기 {#create-the-authorization-servlet}

웹 컨텐츠를 요청하는 사용자의 인증 및 인증을 수행하는 서블릿을 만들고 배포합니다. 서블릿은 AEM 사용자 계정, 리포지토리 ACL 또는 LDAP 조회 서비스와 같은 인증 및 인증 방법을 사용할 수 있습니다. 디스패처가 렌더링으로 사용하는 AEM 인스턴스에 서블릿을 배포합니다.

서블릿은 모든 사용자가 액세스할 수 있어야 합니다. 따라서 서블릿은 시스템에 대한 읽기 전용 액세스를 제공하는 `org.apache.sling.api.servlets.SlingSafeMethodsServlet` 클래스를 확장해야 합니다.

서블릿은 렌더링의 헤드 요청만 수신하므로 `doHead` 메서드를 구현하기만 하면 됩니다.

렌더링에는 요청된 리소스의 URI가 HTTP 요청의 매개 변수로 포함됩니다. 예를 들어 인증 서블릿은을 통해 `/bin/permissioncheck`액세스합니다. /content/geometrixx-outdoors/en.html 페이지에서 보안 검사를 수행하기 위해 렌더링에 HTTP 요청에서 다음 URL 이 포함됩니다.

`/bin/permissioncheck?uri=/content/geometrixx-outdoors/en.html`

Servlet 응답 메시지에는 다음 HTTP 상태 코드가 포함되어야 합니다.

* 200: 인증 및 인증이 통과되었습니다.

다음 예제 서블릿은 HTTP 요청에서 요청된 리소스의 URL를 얻습니다. 코드는 felix scr `Property` 주석을 사용하여 `sling.servlet.paths` 속성 값을 /bin/permissioncheck로 설정합니다. `doHead` 메서드에서 서블릿은 session 객체를 획득하고 `checkPermission` 이 메서드를 사용하여 적절한 응답 코드를 결정합니다.

>[!NOTE]
>
>sling. servlet. paths 속성의 값은 sling servlet resolver (org. apache. sling. servlets. resolver. slingservletresolver) 서비스에서 활성화해야 합니다.

### 서블릿 예 {#example-servlet}

```java
package com.adobe.example;

import org.apache.felix.scr.annotations.Component;
import org.apache.felix.scr.annotations.Service;
import org.apache.felix.scr.annotations.Property;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingSafeMethodsServlet;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.jcr.Session;

@Component(metatype=false)
@Service
public class AuthcheckerServlet extends SlingSafeMethodsServlet {
 
    @Property(value="/bin/permissioncheck")
    static final String SERVLET_PATH="sling.servlet.paths";
    
    private Logger logger = LoggerFactory.getLogger(this.getClass());
    
    public void doHead(SlingHttpServletRequest request, SlingHttpServletResponse response) {
     try{ 
      //retrieve the requested URL
      String uri = request.getParameter("uri");
      //obtain the session from the request
      Session session = request.getResourceResolver().adaptTo(javax.jcr.Session.class);     
      //perform the permissions check
      try {
       session.checkPermission(uri, Session.ACTION_READ);
       logger.info("authchecker says OK");
       response.setStatus(SlingHttpServletResponse.SC_OK);
      } catch(Exception e) {
       logger.info("authchecker says READ access DENIED!");
       response.setStatus(SlingHttpServletResponse.SC_FORBIDDEN);
      }
     }catch(Exception e){
      logger.error("authchecker servlet exception: " + e.getMessage());
     }
    }
}
```

## 권한 감지 캐시에 대한 Dispatcher 구성 {#configure-dispatcher-for-permission-sensitive-caching}

Dispatcher의 auth_ checker 섹션. 모든 파일은 권한 감지 캐싱의 동작을 제어합니다. auth_ checker 섹션에는 다음과 같은 하위 섹션이 포함됩니다.

* `url`: 보안 검사를 수행하는 `sling.servlet.paths` 서블릿의 속성 값.

* `filter`: 권한 감지 캐시가 적용되는 폴더를 지정하는 필터입니다. 일반적으로 `deny` 필터는 모든 폴더에 적용되고 `allow` 필터는 보안 폴더에 적용됩니다.

* `headers`: 인증 서블릿에 응답에 포함된 HTTP 헤더를 지정합니다.

디스패처가 시작되면 디스패처 로그 파일에 다음과 같은 디버그 수준 메시지가 포함됩니다.

`AuthChecker: initialized with URL 'configured_url'.`

다음 auth_ checker 섹션에서는 Dispatcher가 Prevoius 항목의 서블릿을 사용하도록 구성합니다. 필터 섹션은 보안 HTML 리소스에서만 권한 검사를 수행하도록 합니다.

### 예제 구성 {#example-configuration}

```xml
/auth_checker
  {
  # request is sent to this URL with '?uri=<page>' appended
  /url "/bin/permissioncheck"
      
  # only the requested pages matching the filter section below are checked,
  # all other pages get delivered unchecked
  /filter
    {
    /0000
      {
      /glob "*"
      /type "deny"
      }
    /0001
      {
      /glob "/content/secure/*.html"
      /type "allow"
      }
    }
  # any header line returned from the auth_checker's HEAD request matching
  # the section below will be returned as well
  /headers
    {
    /0000
      {
      /glob "*"
      /type "deny"
      }
    /0001
      {
      /glob "Set-Cookie:*"
      /type "allow"
      }
    }
  }
```

