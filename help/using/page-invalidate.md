---
title: AEM에서 캐시된 페이지 무효화
seo-title: Adobe AEM에서 캐시된 페이지 무효화
description: 효과적인 캐시 관리를 위해 Dispatcher와 AEM 간의 상호 작용을 구성하는 방법을 알아봅니다.
seo-description: 효과적인 캐시 관리를 위해 Adobe AEM Dispatcher와 AEM 간의 상호 작용을 구성하는 방법을 알아봅니다.
uuid: 66533299-55 C 0-4864-9 BEB -77 E 281 AF 9359
cmgrlastmodified: 01.11.2007 08 22 29 [Aheimoz]
pageversionid: '1193211344162'
template: /apps/docs/templates/contentpage
contentOwner: 사용자
products: sg_ Experiencemanager/dispatcher
topic-tags: Dispatcher
content-type: 참조
discoiquuid: 79 cd 94 be-a 6 bc -4 d 34-bfe 9-393 b 4107925 c
translation-type: tm+mt
source-git-commit: 76cffbfb616cd5601aed36b7076f67a2faf3ed3b

---


# AEM에서 캐시된 페이지 무효화 {#invalidating-cached-pages-from-aem}

AEM와 함께 Dispatcher를 사용할 때는 효과적인 캐시 관리를 위해 상호 작용을 구성해야 합니다. 환경에 따라 구성이 성능을 높일 수 있습니다.

## AEM 사용자 계정 설정 {#setting-up-aem-user-accounts}

기본 `admin` 사용자 계정은 기본적으로 설치된 복제 에이전트를 인증하는 데 사용됩니다. 복제 에이전트에 사용할 전용 사용자 계정을 만들어야 합니다. [](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/security-checklist.html#VerificationSteps)

자세한 내용은 AEM 보안 체크리스트의 복제 및 전송 사용자 [구성](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/security-checklist.html#VerificationSteps) 섹션을 참조하십시오.

## 작성 환경에서 Dispatcher Cache 무효화 {#invalidating-dispatcher-cache-from-the-authoring-environment}

AEM 작성자 인스턴스의 복제 에이전트는 페이지가 게시될 때 디스패처 요청으로 캐시 무효화 요청을 보냅니다. 이렇게 요청하면 디스패처가 결국 새 컨텐츠가 게시되면 캐시에서 파일을 새로 고칩니다.

<!-- 

Comment Type: draft

<p>Cache invalidation requests for a page are also generated for any aliases or vanity URLs that are configured in the page properties. </p>

 -->

<!-- 

Comment Type: remark
Last Modified By: unknown unknown (ims-author-0436B4A35714BFF67F000101@AdobeID)
Last Modified Date: 2017-05-25T10:37:23.679-0400

<p>Hiding this information due to <a href="https://jira.corp.adobe.com/browse/CQDOC-5805">CQDOC-5805</a>.</p>

 -->

다음 절차를 사용하여 페이지 활성화 시 Dispatcher 캐시를 무효화하기 위한 AEM 작성자 인스턴스의 복제 에이전트를 구성합니다.

1. AEM 도구 콘솔을 엽니다. (`https://localhost:4502/miscadmin#/etc`)
1. 작성자의 도구/복제/에이전트 아래에서 필요한 복제 에이전트를 엽니다. 기본적으로 설치된 Dispatcher Flush 에이전트를 사용할 수 있습니다.
1. 편집을 클릭하고 설정 탭에서 **활성화가** 선택되어 있는지 확인합니다.

1. (선택 사항) 별칭 또는 별칭 경로 무효화 요청을 활성화하려면 **별칭 업데이트** 옵션을 선택합니다.
1. 전송 탭에서 Dispatcher에 액세스하는 데 필요한 URI를 입력합니다.\
   표준 Dispatcher Flush 에이전트를 사용하는 경우 호스트 이름과 포트를 업데이트해야 할 수 있습니다. 예: https:// &lt;*Dispatcherhost*&gt;: &lt;*portapache*&gt;/dispatcher/invalidate. cache

   **참고:** Dispatcher Flush 에이전트의 경우 URI 속성은 경로 기반 virtualhost 항목을 사용하여 팜을 구별하는 경우에만 사용됩니다. 이 필드를 사용하여 무효화할 팜을 타깃팅합니다. 예를 들어 farm # 1 에는 가상 호스트가 있고 `www.mysite.com/path1/*` farm # 2 에는 가상 호스트가 있습니다 `www.mysite.com/path2/*`. 의 URL를 사용하여 첫 `/path1/invalidate.cache` 번째 팜을 타깃팅하고 `/path2/invalidate.cache` 두 번째 팜을 타깃팅할 수 있습니다. 자세한 내용은 여러 도메인이 있는 Dispatcher [사용을](dispatcher-domains.md)참조하십시오.

1. 필요에 따라 다른 매개 변수를 구성합니다.
1. 확인을 클릭하여 에이전트를 활성화합니다.

또는 [AEM Touch UI에서 Dispatcher Flush 에이전트를 액세스하고 구성할](https://helpx.adobe.com/experience-manager/6-2/sites/deploying/using/replication.html#ConfiguringaDispatcherFlushagent)수도 있습니다.

별칭 URL에 대한 액세스를 활성화하는 방법에 대한 자세한 내용은 별칭 URL에 대한 액세스 [활성화를 참조하십시오](dispatcher-configuration.md#enabling-access-to-vanity-urls-vanity-urls).

>[!NOTE]
>
>디스패처 캐시 플러시를 위한 에이전트에는 사용자 이름과 암호가 없어도 되지만 구성한 경우 기본 인증으로 전송됩니다.

이 방법에는 다음과 같은 두 가지 잠재적 문제가 있습니다.

* 디스패처는 작성 인스턴스에서 접근할 수 있어야 합니다. 네트워크 (예: 방화벽) 가 둘 사이의 액세스가 제한되도록 구성된 경우 이러한 경우가 아닐 수 있습니다.

* 게시 및 캐시 무효화가 동시에 이루어집니다. 시간에 따라, 사용자는 새 페이지가 게시되기 직전에 캐시에서 제거된 직후 페이지를 요청할 수 있습니다. 이제 AEM에서 이전 페이지를 반환하고 디스패처가 다시 캐시합니다. 이는 큰 사이트에서 발생하는 문제입니다.

## 게시 인스턴스에서 Dispatcher Cache 무효화 {#invalidating-dispatcher-cache-from-a-publishing-instance}

특정 상황에서는 캐시 관리에서 게시 인스턴스로 캐시 관리를 전송함으로써 성능을 향상시킬 수 있습니다. 그러면 게시된 페이지가 수신될 때 캐시 무효화 요청을 발송하는 게시 환경 (AEM 작성 환경 아님) 이 됩니다.

이러한 상황은 다음과 같습니다.

<!-- 

Comment Type: draft

<p>Cache invalidation requests for a page are also generated for any aliases or vanity URLs that are configured in the page properties. </p>

 -->

* 디스패처와 게시 인스턴스 간의 시간 충돌을 방지합니다 (제작 환경에서 Dispatcher 캐시 [무효화 참조](#invalidating-dispatcher-cache-from-the-authoring-environment)).
* 시스템에는 고성능 서버에 있는 여러 게시 인스턴스가 포함되며 하나의 작성 인스턴스만 포함됩니다.

>[!NOTE]
>
>이 방법을 사용하는 결정은 숙련된 AEM 관리자가 수행해야 합니다.

디스패처 플러시는 게시 인스턴스에서 작동하는 복제 에이전트에 의해 제어됩니다. 하지만, 작성 환경에서 구성이 수행되고 에이전트를 활성화함으로써 다음 사항이 전송됩니다.

1. AEM 도구 콘솔을 엽니다.
1. 게시할 때 도구/복제/에이전트 아래에서 필요한 복제 에이전트를 엽니다. 기본적으로 설치된 Dispatcher Flush 에이전트를 사용할 수 있습니다.
1. 편집을 클릭하고 설정 탭에서 **활성화가** 선택되어 있는지 확인합니다.
1. (선택 사항) 별칭 또는 별칭 경로 무효화 요청을 활성화하려면 **별칭 업데이트** 옵션을 선택합니다.
1. 전송 탭에서 Dispatcher에 액세스하는 데 필요한 URI를 입력합니다.\
   표준 Dispatcher Flush 에이전트를 사용하는 경우 호스트 이름과 포트를 업데이트해야 할 수 있습니다. 예를 들어, `http://<dispatcherHost>:<portApache>/dispatcher/invalidate.cache`

   **참고:** Dispatcher Flush 에이전트의 경우 URI 속성은 경로 기반 virtualhost 항목을 사용하여 팜을 구별하는 경우에만 사용됩니다. 이 필드를 사용하여 무효화할 팜을 타깃팅합니다. 예를 들어 farm # 1 에는 가상 호스트가 있고 `www.mysite.com/path1/*` farm # 2 에는 가상 호스트가 있습니다 `www.mysite.com/path2/*`. 의 URL를 사용하여 첫 `/path1/invalidate.cache` 번째 팜을 타깃팅하고 `/path2/invalidate.cache` 두 번째 팜을 타깃팅할 수 있습니다. 자세한 내용은 여러 도메인이 있는 Dispatcher [사용을](dispatcher-domains.md)참조하십시오.

1. 필요에 따라 다른 매개 변수를 구성합니다.
1. 영향을 받는 모든 게시 인스턴스에 대해 반복합니다.

구성 후 작성자에서 게시로 페이지를 활성화하면 이 에이전트가 표준 복제를 시작합니다. 이 로그에는 다음 예와 유사한, 게시 서버에서 온 요청을 나타내는 메시지가 포함되어 있습니다.

1. `<publishserver> 13:29:47 127.0.0.1 POST /dispatcher/invalidate.cache 200`

## 수동으로 디스패처 캐시 무효화 {#manually-invalidating-the-dispatcher-cache}

페이지를 활성화하지 않고 디스패처 캐시를 무효화 (또는 플러시) 하려면, 디스패처에 HTTP 요청을 발행할 수 있습니다. 예를 들어 관리자나 다른 애플리케이션이 캐시를 플러시할 수 있는 AEM 애플리케이션을 만들 수 있습니다.

HTTP 요청으로 인해 디스패처가 캐시에서 특정 파일을 삭제하게 됩니다. 필요한 경우, 디스패처는 새 사본으로 캐시를 새로 고칩니다.

### 캐시된 파일 삭제 {#delete-cached-files}

디스패처가 캐시에서 파일을 삭제하도록 하는 HTTP 요청을 발행합니다. Dispatcher는 해당 페이지에 대한 클라이언트 요청을 수신할 때에만 파일을 다시 캐시합니다. 캐시된 파일을 삭제하는 기능은 동일한 페이지에 대한 동시 요청을 받을 가능성이 없는 웹 사이트에 적합합니다.

HTTP 요청에는 다음 양식이 있습니다.

```xml
POST /dispatcher/invalidate.cache HTTP/1.1  
CQ-Action: Activate  
CQ-Handle: path-pattern
Content-Length: 0
```

Dispatcher는 `CQ-Handler` 헤더의 값과 일치하는 이름이 있는 캐시된 파일과 폴더를 플러시 (삭제) 합니다. 예를 들어, `CQ-Handle` 의 `/content/geomtrixx-outdoors/en` A는 다음 항목과 일치합니다.

* 디렉토리에서 이름이 지정된 `en` 모든 파일 `geometrixx-outdoors` (확장자)

* The en directory named &quot; `_jcr_content`&quot; below the en directory (if it exist, if it exist, contains cached rendering of the page)

디스패처 캐시에 있는 다른 모든 파일 (또는 `/statfileslevel` 설정에 따라 특정 수준) 는 `.stat` 파일을 터치하여 무효화됩니다. 이 파일의 마지막 수정 날짜는 캐시된 문서의 마지막 수정 날짜와 비교되며 파일이 최신 버전이면 다시 `.stat` 반입됩니다. 자세한 [내용은 폴더 수준별](dispatcher-configuration.md#main-pars_title_26) 파일 무효화를 참조하십시오.

추가 헤더를 보내 무효화 (예:. stat 파일 수정) 를 방지할 `CQ-Action-Scope: ResourceOnly`수 있습니다. 이 기능을 사용하면 동적으로 만들어지는 JSON 데이터와 같이 캐시의 다른 부분을 무효화하지 않고 특정 리소스를 플러시하는 데 사용할 수 있습니다 (예: 타사 시스템에서 뉴스를 표시하는 경우 뉴스, 주식 티커 등).

### 파일 삭제 및 제거 {#delete-and-recache-files}

디스패처가 캐시된 파일을 삭제하고 파일을 즉시 검색하고 감쇠하는 HTTP 요청을 발행합니다. 웹 사이트가 동일한 페이지에 대한 동시 클라이언트 요청을 받을 가능성이 있을 때 파일을 삭제하고 즉시 다시 캐시합니다. 즉시 재배치를 수행하면 Dispatcher가 각 동시 클라이언트 요청에 대해 한 번이 아니라 한 번만 페이지를 검색하고 캐시합니다.

**참고:** 파일을 삭제하고 재전송하는 작업은 게시 인스턴스에서만 수행해야 합니다. 작성자 인스턴스에서 수행되는 경우, 리소스를 게시하기 전에 리트윗하려고 하면 레이스 조건이 발생합니다.

HTTP 요청에는 다음 양식이 있습니다.

```xml
POST /dispatcher/invalidate.cache HTTP/1.1  
CQ-Action: Activate   
`Content-Type: text/plain  
CQ-Handle: path-pattern  
Content-Length: numchars in bodypage_path0

page_path1 
...  
page_pathn
```

페이지 경로가 메시지 본문에 있는 별도의 줄에 즉시 나열됩니다. 의 `CQ-Handle` 값은 페이지를 무효화할 수 있도록 무효화하는 페이지 경로입니다. `/statfileslevel`[(캐시](dispatcher-configuration.md#main-pars_146_44_0010) 구성 항목의 매개 변수를 참조하십시오.) 다음 HTTP 요청 메시지는 다음을 삭제하고 `/content/geometrixx-outdoors/en.html page`리래핑합니다.

```xml
POST /dispatcher/invalidate.cache HTTP/1.1  
CQ-Action: Activate  
Content-Type: text/plain   
CQ-Handle: /content/geometrixx-outdoors/en/men.html  
Content-Length: 36

/content/geometrixx-outdoors/en.html
```

### 플러시 서블릿 예 {#example-flush-servlet}

다음 코드는 디스패처로 요청을 무효화하는 서블릿을 구현합니다. 서블릿은 `handle` 매개 `page` 변수와 매개 변수를 포함하는 요청 메시지를 받습니다. 이러한 매개 변수는 각각 recache to recache `CQ-Handle` 헤더와 페이지 경로를 제공합니다. 서블릿은 값을 사용하여 Dispatcher에 대한 HTTP 요청을 구성합니다.

서블릿이 게시 인스턴스에 배포된 경우, 다음 URL로 인해 디스패처가 /content/geometrixx-outdoors/en.html 페이지를 삭제한 다음 새 사본을 캐시합니다.

`10.36.79.223:4503/bin/flushcache/html?page=/content/geometrixx-outdoors/en.html&handle=/content/geometrixx-outdoors/en/men.html`

>[!NOTE]
>
>이 예제 서블릿은 안전하지 않으며 HTTP POST 요청 메시지만 사용하는 것을 보여줍니다. 솔루션은 서블릿에 대한 액세스를 보장해야 합니다.


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

import org.apache.commons.httpclient.*;
import org.apache.commons.httpclient.methods.PostMethod;
import org.apache.commons.httpclient.methods.StringRequestEntity;

@Component(metatype=true)
@Service
public class Flushcache extends SlingSafeMethodsServlet {

 @Property(value="/bin/flushcache")
 static final String SERVLET_PATH="sling.servlet.paths";

 private Logger logger = LoggerFactory.getLogger(this.getClass());

 public void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
  try{ 
      //retrieve the request parameters
      String handle = request.getParameter("handle");
      String page = request.getParameter("page");

      //hard-coding connection properties is a bad practice, but is done here to simplify the example
      String server = "localhost"; 
      String uri = "/dispatcher/invalidate.cache";

      HttpClient client = new HttpClient();

      PostMethod post = new PostMethod("https://"+host+uri);
      post.setRequestHeader("CQ-Action", "Activate");
      post.setRequestHeader("CQ-Handle",handle);
   
      StringRequestEntity body = new StringRequestEntity(page,null,null);
      post.setRequestEntity(body);
      post.setRequestHeader("Content-length", String.valueOf(body.getContentLength()));
      client.executeMethod(post);
      post.releaseConnection();
      //log the results
      logger.info("result: " + post.getResponseBodyAsString());
      }
  }catch(Exception e){
      logger.error("Flushcache servlet exception: " + e.getMessage());
  }
 }
}
```

