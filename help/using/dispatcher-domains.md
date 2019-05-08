---
title: '여러 도메인이 있는 Dispatcher 사용 '
seo-title: '여러 도메인이 있는 Dispatcher 사용 '
description: Dispatcher를 사용하여 여러 웹 도메인에서 페이지 요청을 처리하는 방법을 알아봅니다.
seo-description: Dispatcher를 사용하여 여러 웹 도메인에서 페이지 요청을 처리하는 방법을 알아봅니다.
uuid: 7342 a 1 c 2-fe 61-49 be-a 240-b 487 d 53 c 7 ec 1
contentOwner: 사용자
cq-exporttemplate: /etc/contentsync/templates/geometrixx/page/rewrite
products: sg_ Experiencemanager/dispatcher
topic-tags: Dispatcher
content-type: 참조
discoiquuid: 40 D 91 D 66-C 99 B -422 D -8 E 61-C 0 CED 23272 EF
translation-type: tm+mt
source-git-commit: f35c79b487454059062aca6a7c989d5ab2afaf7b

---


# 여러 도메인이 있는 Dispatcher 사용 {#using-dispatcher-with-multiple-domains}

>[!NOTE]
>
>발송자 버전은 AEM와 독립적입니다. AEM 또는 CQ 설명서에 포함된 Dispatcher 설명서에 대한 링크를 따라왔다면 이 페이지로 리디렉션되었을 수 있습니다.

Dispatcher를 사용하여 다음과 같은 조건을 지원하면서 여러 웹 도메인에서 페이지 요청을 처리할 수 있습니다.

* 두 도메인 모두에 대한 웹 컨텐츠는 단일 AEM 저장소에 저장됩니다.
* Dispatcher 캐시에 있는 파일은 각 도메인에 대해 별도로 무효화할 수 있습니다.

예를 들어 한 회사는 두 브랜드에 대한 웹 사이트를 게시합니다. 브랜드 A와 브랜드 B. 웹 사이트 페이지에 대한 컨텐츠는 AEM에서 작성되며, 동일한 저장소 작업 공간에 저장됩니다.

```
/
| - content  
   | - sitea  
       | - content nodes  
   | - siteb  
       | - content nodes
```

에 대한 `BrandA.com` 페이지는 `/content/sitea`아래에 저장됩니다. URL `https://BrandA.com/en.html` 에 대한 클라이언트 요청이 `/content/sitea/en` 노드에 대해 렌더링된 페이지를 반환합니다. 마찬가지로, 에 대한 `BrandB.com` 페이지는 `/content/siteb`아래에 저장됩니다.

Dispatcher를 사용하여 컨텐츠를 캐시할 때는 클라이언트 HTTP 요청의 페이지 URL, 캐시에 있는 해당 파일의 경로 및 저장소에서 해당 파일의 경로를 연결해야 합니다.

## 클라이언트 요청

클라이언트가 HTTP 요청을 웹 서버로 전송하면 요청된 페이지의 URL 이 Dispatcher 캐시에 있는 컨텐츠로 해결되고 결국 저장소의 컨텐츠로 해결되어야 합니다.

![](assets/chlimage_1-8.png)

1. 도메인 이름 시스템은 HTTP 요청에서 도메인 이름에 대해 등록된 웹 서버의 IP 주소를 검색합니다.
1. HTTP 요청이 웹 서버로 전송됩니다.
1. HTTP 요청이 Dispatcher로 전달됩니다.
1. Dispatcher는 캐시된 파일이 유효한지 여부를 결정합니다. 유효하면 캐시된 파일이 클라이언트에 제공됩니다.
1. 캐시된 파일이 유효하지 않은 경우, Dispatcher는 AEM 게시 인스턴스에서 새로 렌더링된 페이지를 요청합니다.

## 캐시 무효화

Dispatcher Flush Replication 에이전트를 통해 디스패처가 캐시된 파일을 무효화하면 저장소의 컨텐츠 경로가 캐시에 있는 컨텐츠로 확인되어야 합니다.

![](assets/chlimage_1-9.png)

1. AEM 작성자 인스턴스에서 페이지가 활성화되고 컨텐츠가 게시 인스턴스로 복제됩니다.
1. Dispatcher Flush 에이전트는 Dispatcher를 호출하여 복제된 컨텐츠에 대한 캐시를 무효화합니다.
1. Dispatcher는 하나 이상의. stat 파일을 터치하여 캐시된 파일을 무효화합니다.

여러 도메인이 있는 Dispatcher를 사용하려면 AEM, Dispatcher 및 웹 서버를 구성해야 합니다. 이 페이지에 설명된 솔루션은 일반적이며 대부분의 환경에 적용됩니다. 일부 AEM 토폴로지의 복잡성으로 인해 솔루션에는 특정 문제를 해결하기 위한 추가 사용자 지정 구성이 필요할 수 있습니다. 예를 들어 기존의 IT 인프라 및 관리 정책에 맞게 예제를 조정해야 합니다.

## URL 매핑 {#url-mapping}

도메인 URL 및 컨텐츠 경로를 사용하여 캐시된 파일로 문제를 해결하려면 프로세스 중 파일 경로 또는 페이지 URL를 번역해야 합니다. 프로세스의 다른 지점에서 경로 또는 URL 변환이 발생하는 다음 공통 전략에 대한 설명이 제공됩니다.

* (권장) AEM 게시 인스턴스는 리소스 해상도에 Sling 매핑을 사용하여 내부 URL 재작성 규칙을 구현합니다. 도메인 URL는 컨텐츠 저장소 경로로 변환됩니다. ( [AEM에서 들어오는 URL 다시 작성을 참조하십시오](dispatcher-domains.md#main-pars-title-2).)
* 웹 서버는 도메인 URL를 캐시 패스로 변환하는 내부 URL 재작성 규칙을 사용합니다. 웹 [서버가 들어오는 URL를 다시 쓰는 것을 참조하십시오](dispatcher-domains.md#main-pars-title-1).

일반적으로 웹 페이지에 짧은 URL를 사용하는 것이 좋습니다. 일반적으로 페이지 URL는 웹 컨텐츠가 들어 있는 저장소 폴더의 구조를 미러링합니다. 그러나 URL는 맨 위의 저장소 노드 (예:) 를 표시하지 않습니다 `/content`. 클라이언트는 AEM 저장소의 구조를 반드시 알고 있지 않습니다.

## 일반 요구 사항 {#general-requirements}

환경에서는 여러 도메인으로 작동하는 Dispatcher를 지원하기 위해 다음 구성을 구현해야 합니다.

* 각 도메인에 대한 컨텐츠는 저장소의 별도 분기에 상주합니다 (아래 예제 환경 참조).
* Dispatcher Flush Replication Agent는 AEM 게시 인스턴스에 구성됩니다. (게시 인스턴스에서 Dispatcher Cache [무효화를 참조하십시오](page-invalidate.md).)
* 도메인 이름 시스템은 도메인 이름을 웹 서버의 IP 주소로 해결합니다.
* 디스패처 캐시는 AEM 컨텐츠 저장소의 디렉토리 구조를 미러링합니다. 웹 서버의 문서 루트 아래 있는 파일 경로는 저장소의 파일 경로와 동일합니다.

## 제공된 예제 환경 {#environment-for-the-provided-examples}

제공되는 예제 솔루션은 다음 특성이 있는 환경에 적용됩니다.

* AEM 작성자 및 게시 인스턴스는 Linux 시스템에 배포됩니다.
* Apache Httpd는 Linux 시스템에 배포된 웹 서버입니다.
* AEM 컨텐츠 저장소 및 웹 서버의 문서 루트는 다음 파일 구조를 사용합니다 (Apache 웹 서버의 문서 루트:/`usr/lib/apache/httpd-2.4.3/htdocs)`:

   **보관소**

```
  | - /content  
    | - sitea  
  |    | - content nodes 
    | - siteb  
       | - conent nodes
```

**웹 서버의 문서 루트**

```
  | - /usr  
    | - lib  
      | - apache  
        | - httpd-2.4.3  
          | - htdocs  
            | - content  
              | - sitea  
                 | - content nodes 
              | - siteb  
                 | - content nodes
```

## AEM에서 들어오는 URL 다시 작성 {#aem-rewrites-incoming-urls}

리소스 해결을 위한 Sling 매핑을 사용하여 들어오는 URL를 AEM 컨텐츠 경로와 연결할 수 있습니다. AEM 게시 인스턴스에서 매핑을 만들어 Dispatcher의 렌더링 요청이 저장소의 올바른 컨텐츠로 해결되도록 합니다.

디스패처 요청 페이지 렌더링은 웹 서버에서 전달된 URL를 사용하여 페이지를 식별합니다. URL에 도메인 이름이 포함되어 있으면 Sling 매핑이 컨텐츠에 대한 URL를 해결합니다. 다음 그래픽은 노드에 `branda.com/en.html` 대한 URL 매핑을 `/content/sitea/en` 보여줍니다.

![](assets/chlimage_1-10.png)

디스패처 캐시는 저장소 노드 구조를 미러링합니다. 따라서 캐시된 페이지를 무효화하기 위한 결과 요청에 페이지 활성화가 발생하는 경우 URL 이나 경로 변환이 필요하지 않습니다.

![](assets/chlimage_1-11.png)

## 웹 서버에서 가상 호스트 정의 {#define-virtual-hosts-on-the-web-server}

각 웹 도메인에 다른 문서 루트를 할당할 수 있도록 웹 서버에서 가상 호스트를 정의합니다.

* 웹 서버는 각 웹 도메인에 대한 가상 도메인을 정의해야 합니다.
* 각 도메인에 대해 도메인의 웹 컨텐츠가 포함된 저장소의 폴더와 일치하도록 문서 루트를 구성합니다.
* 각 가상 도메인에는 [Dispatcher](dispatcher-install.md) 설치 페이지에 설명된 것처럼 Dispatcher 관련 구성이 포함되어야 합니다.

다음 예제 `httpd.conf` 파일은 Apache 웹 서버에 대한 두 개의 가상 도메인을 구성합니다.

* 서버 이름 (도메인 이름과 일치하는 이름) 는 branda.com (라인 16) 및 brandb.com (30 행) 입니다.
* 각 가상 도메인의 문서 루트는 사이트 페이지가 포함된 Dispatcher 캐시에 있는 디렉토리입니다. (선 17 및 31)

이 구성을 통해 웹 서버는 다음에 대한 요청을 수신할 때 다음 작업을 수행합니다 `https://branda.com/en/products.html`.

* Associates the URL with the virtual host that has a `ServerName` of `branda.com.`

* URL를 Dispatcher로 전달합니다.

### httpd. conf {#httpd-conf}

```xml
# load the Dispatcher module
LoadModule dispatcher_module modules/mod_dispatcher.so
# configure the Dispatcher module
<IfModule disp_apache2.c>
 DispatcherConfig conf/dispatcher.any
 DispatcherLog    logs/dispatcher.log  
 DispatcherLogLevel 3
 DispatcherNoServerHeader 0 
 DispatcherDeclineRoot 0
 DispatcherUseProcessedURL 0
 DispatcherPassError 0
</IfModule>

# Define virtual host for brandA.com
<VirtualHost *:80>
  ServerName branda.com
  DocumentRoot /usr/lib/apache/httpd-2.4.3/htdocs/content/sitea
   <Directory /usr/lib/apache/httpd-2.4.3/htdocs/content/sitea>
     <IfModule disp_apache2.c>
       SetHandler dispatcher-handler
       ModMimeUsePathInfo On
     </IfModule>
     Options FollowSymLinks
     AllowOverride None
   </Directory>
</VirtualHost>

# define virtual host for brandB.com
<VirtualHost *:80>
  ServerName brandB.com
  DocumentRoot /usr/lib/apache/httpd-2.4.3/htdocs/content/siteb
   <Directory /usr/lib/apache/httpd-2.4.3/htdocs/content/siteb>
     <IfModule disp_apache2.c>
       SetHandler dispatcher-handler
       ModMimeUsePathInfo On
     </IfModule>
     Options FollowSymLinks
     AllowOverride None
   </Directory>
</VirtualHost>

# document root for web server
DocumentRoot "/usr/lib/apache/httpd-2.4.3/htdocs"
```

가상 호스트는 기본 서버 섹션에 구성된 [Dispatcherconfig](dispatcher-install.md#main-pars-67-table-7) 속성 값을 상속합니다. 가상 주최자는 자신의 Dispatcherconfig 속성을 포함하여 기본 서버 구성을 무시할 수 있습니다.

### 여러 도메인을 처리하도록 Dispatcher 구성 {#configure-dispatcher-to-handle-multiple-domains}

도메인 이름과 해당 가상 호스트가 포함된 URL를 지원하려면 다음 디스패처 팜을 정의합니다.

* 각 가상 호스트에 대해 발송자 팜을 구성합니다. 이러한 팜은 각 도메인에 대한 웹 서버의 요청을 처리하고 캐시된 파일을 확인하며 렌더링에서 페이지를 요청합니다.
* 컨텐츠가 속해 있는 도메인에 관계없이 캐시를 무효화하는 데 사용되는 Dispatcher Farm를 구성합니다. 이 팜은 플러시 Dispatcher Replication 에이전트의 파일 무효화 요청을 처리합니다.

### 가상 호스트를 위한 디스패처 팜 만들기

가상 호스트의 팜에 다음 구성이 있어야 클라이언트 HTTP 요청의 URL 이 Dispatcher 캐시에서 올바른 파일로 해결됩니다.

* 속성은 `/virtualhosts` 도메인 이름으로 설정됩니다. 이 속성을 사용하면 Dispatcher가 팜에 도메인과 연결할 수 있습니다.
* `/filter` 이 속성을 사용하면 도메인 이름 다음에 잘린 요청 URL 경로에 액세스할 수 있습니다. 예를 들어 `https://branda.com/en.html` URL의 경우 경로는 AS `/en.html`로 해석되므로 필터에서는 이 경로에 대한 액세스를 허용해야 합니다.

* `/docroot` 이 속성은 Dispatcher 캐시에 있는 도메인의 사이트 컨텐츠 루트 디렉토리 경로로 설정됩니다. 이 경로는 원래 요청의 연결된 URL에 대한 접두사로 사용됩니다. 예를 들어, 의 `/usr/lib/apache/httpd-2.4.3/htdocs/sitea` docroot는 요청이 `https://branda.com/en.html``/usr/lib/apache/httpd-2.4.3/htdocs/sitea/en.html` 파일로 해결되도록 합니다.

또한 AEM 게시 인스턴스는 가상 호스트에 대한 렌더링으로 지정해야 합니다. 필요에 따라 다른 농장 속성을 구성합니다. 다음 코드는 branda.com 도메인에 대한 축약된 팜 구성입니다.

```xml
/farm_sitea  {     
    ...
    /virtualhosts { "branda.com" }
    /renders {
      /rend01  { /hostname "127.0.0.1"  /port "4503" }
    }
    /filter {
      /0001 { /type "deny"  /glob "*" }
      /0023 { /type "allow" /glob "*/en*" }  
      ...
     }
    /cache {
      /docroot "/usr/lib/apache/httpd-2.4.3/htdocs/content/sitea"
      ...
   }
   ...
}
```

### 캐시 무효화를 위한 Dispatcher Farm 만들기

캐시된 파일 무효화에 대한 요청을 처리하려면 Dispatcher 팜이 필요합니다. 이 팜은 각 가상 호스트의 Docroot 디렉토리에서. stat 파일에 액세스할 수 있어야 합니다.

다음 속성 구성을 사용하면 디스패처가 캐시에 있는 파일에서 AEM 컨텐츠 저장소의 파일을 해결할 수 있습니다.

* 이 `/docroot` 속성은 웹 서버의 기본 docroot로 설정됩니다. 일반적으로 이 디렉토리는 `/content` 폴더를 만든 디렉토리입니다. Linux에서 Apache의 예 값은 `/usr/lib/apache/httpd-2.4.3/htdocs`다음과 같습니다.
* `/filter` 이 속성을 사용하면 `/content` 디렉토리 아래 파일에 액세스할 수 있습니다.

`/statfileslevel`각 가상 호스트의 루트 디렉토리에. stat 파일이 만들어지도록 속성이 충분히 높아야 합니다. 이 속성을 사용하면 각 도메인의 캐시가 별도로 무효화됩니다. 예제 설정의 경우 `/statfileslevel` , 의 값은 `2``*docroot*/content/sitea` 디렉토리와 `*docroot*/content/siteb` 디렉토리에서. stat 파일을 만듭니다.

또한 게시 인스턴스는 가상 호스트에 대한 렌더링으로 지정해야 합니다. 필요에 따라 다른 농장 속성을 구성합니다. 다음 코드는 캐시를 무효화하는 데 사용되는 팜에 대한 축약된 구성입니다.

```xml
/farm_flush {  
    ...
    /virtualhosts   { "invalidation_only" }
    /renders  {
      /rend01  { /hostname "127.0.0.1" /port "4503" }
    }
    /filter   {
      /0001 { /type "deny"  /glob "*" }
      /0023 { /type "allow" /glob "*/content*" } 
      ...
      }
    /cache  {
       /docroot "/usr/lib/apache/httpd-2.4.3/htdocs"
       /statfileslevel "2"
       ...
   }
   ...
}
```

웹 서버를 시작할 때 디스패처 로그 (디버그 모드) 는 모든 팜의 초기화를 나타냅니다.

```shell
Dispatcher initializing (build 4.1.2)
[Fri Nov 02 16:27:18 2012] [D] [24974(140006182991616)] farms[farm_sitea].cache.docroot = /usr/lib/apache/httpd-2.4.3/htdocs/content/sitea
[Fri Nov 02 16:27:18 2012] [D] [24974(140006182991616)] farms[farm_siteb].cache.docroot = /usr/lib/apache/httpd-2.4.3/htdocs/content/siteb
[Fri Nov 02 16:27:18 2012] [D] [24974(140006182991616)] farms[farm_flush].cache.docroot = /usr/lib/apache/httpd-2.4.3/htdocs
[Fri Nov 02 16:27:18 2012] [I] [24974(140006182991616)] Dispatcher initialized (build 4.1.2)
```

### 리소스 해결을 위한 Sling 매핑 구성 {#configure-sling-mapping-for-resource-resolution}

리소스 해상도에 Sling 매핑을 사용하여 도메인 기반 URL 이 AEM 게시 인스턴스의 컨텐츠로 해결되도록 합니다. 리소스 매핑은 발송자로부터 들어오는 URL (원래 클라이언트 HTTP 요청의 경우) 를 컨텐츠 노드로 변환합니다.

Sling 리소스 매핑에 대한 자세한 내용은 [Sling 설명서의 리소스 해상도](https://sling.apache.org/site/mappings-for-resource-resolution.html) 매핑을 참조하십시오.

일반적으로 추가 매핑이 필요할 수 있지만 다음 리소스를 사용하려면 매핑이 필요합니다.

* 컨텐츠 페이지의 루트 노드 (아래 `/content`)
* 페이지를 사용하는 디자인 노드 (아래 `/etc/designs`)
* The `/libs` folder

컨텐츠 페이지에 대한 매핑을 만든 후 추가 필수 매핑을 발견하려면 웹 브라우저를 사용하여 웹 서버에서 페이지를 엽니다. Publish 인스턴스의 Error. log 파일에서 찾을 수 없는 리소스에 대한 메시지를 찾습니다. 다음 예제 메시지는 매핑이 `/etc/clientlibs` 필수임을 나타냅니다.

```shell
01.11.2012 15:59:24.601 *INFO* [10.36.34.243 [1351799964599] GET /etc/clientlibs/foundation/jquery.js HTTP/1.1] org.apache.sling.engine.impl.SlingRequestProcessorImpl service: Resource /content/sitea/etc/clientlibs/foundation/jquery.js not found
```

>[!NOTE]
>
>기본 Apache Sling Rewriter의 linkchecker 변환기는 끊어진 링크를 방지하기 위해 페이지에서 하이퍼링크를 자동으로 수정합니다. 그러나 링크 대상이 HTML 또는 HTM 파일인 경우에만 링크 재쓰기가 수행됩니다. 다른 파일 유형에 대한 링크를 업데이트하려면 transformer 구성 요소를 만들고 HTML 재작성기 파이프라인에 추가합니다.

### 리소스 매핑 노드 예

다음 표는 branda.com 도메인에 대한 리소스 매핑을 구현하는 노드를 보여줍니다. 도메인과 같이 `brandb.com` 도메인에 대해 유사한 노드가 만들어집니다 `/etc/map/http/brandb.com`. sling 컨텍스트에서 페이지 HTML의 참조가 올바르게 확인되지 않는 경우 매핑이 필요합니다.

| 노드 경로 | 유형 | 속성 |
|--- |--- |--- |
| `/etc/map/http/branda.com` | Sling: 매핑 | 이름: Sling: Internalredirect 유형: 문자열 값: /content/sitea |
| `/etc/map/http/branda.com/libs` | Sling: 매핑 | 이름: Sling: Internalredirect <br/>유형: 문자열 <br/>값: /libs |
| `/etc/map/http/branda.com/etc` | Sling: 매핑 |
| `/etc/map/http/branda.com/etc/designs` | Sling: 매핑 | 이름: Sling: Internalredirect <br/>vtype: 문자열 <br/>vvalue: /etc/designs |
| `/etc/map/http/branda.com/etc/clientlibs` | Sling: 매핑 | 이름: Sling: Internalredirect <br/>vtype: 문자열 <br/>vvalue: /etc/clientlibs |

## Dispatcher Flush Replication Agent 구성 {#configuring-the-dispatcher-flush-replication-agent}

AEM 게시 인스턴스의 Dispatcher Flush 복제 에이전트는 무효화 요청을 올바른 Dispatcher 팜에 전송해야 합니다. 팜에 타깃팅하려면 전송 탭의 Dispatcher Flush Replication Agent의 URI 속성을 사용합니다. 캐시를 무효화하기 위해 구성된 Dispatcher 팜에 대한 `/virtualhost` 속성 값을 포함합니다.

`https://*webserver_name*:*port*/*virtual_host*/dispatcher/invalidate.cache`

예를 들어 이전 예제의 `farm_flush` 팜을 사용하려면 URI가 있습니다 `https://localhost:80/invalidation_only/dispatcher/invalidate.cache`.

![](assets/chlimage_1-12.png)

## 웹 서버가 들어오는 URL를 다시 씁니다. {#the-web-server-rewrites-incoming-urls}

웹 서버의 내부 URL 재작성 기능을 사용하여 도메인 기반 URL를 디스패처 캐시의 파일 경로로 변환합니다. 예를 들어, `https://brandA.com/en.html` 페이지에 대한 클라이언트 요청은 웹 서버의 문서 루트에 있는 `content/sitea/en.html`파일로 변환됩니다.

![](assets/chlimage_1-13.png)

디스패처 캐시는 저장소 노드 구조를 미러링합니다. 따라서 캐시된 페이지를 무효화하기 위한 결과 요청에 페이지 활성화가 발생하는 경우 URL 이나 경로 변환이 필요하지 않습니다.

![](assets/chlimage_1-14.png)

## 웹 서버에 가상 호스트 정의 및 규칙 다시 작성 {#define-virtual-hosts-and-rewrite-rules-on-the-web-server}

웹 서버에서 다음 측면을 구성합니다.

* 각 웹 도메인의 가상 호스트를 정의합니다.
* 각 도메인에 대해 도메인의 웹 컨텐츠가 포함된 저장소의 폴더와 일치하도록 문서 루트를 구성합니다.
* 각 가상 도메인에 대해 들어오는 URL를 캐시된 파일의 경로로 변환하는 URL 이름 바꾸기 규칙을 만듭니다.
* 각 가상 도메인에는 [Dispatcher](dispatcher-install.md) 설치 페이지에 설명된 것처럼 Dispatcher 관련 구성이 포함되어야 합니다.
* 웹 서버가 다시 작성한 URL를 사용하려면 Dispatcher 모듈을 구성해야 합니다. (Dispatcher 설치에서 `DispatcherUseProcessedURL`[속성을 참조하십시오](dispatcher-install.md).)

다음 httpd. conf 파일은 Apache 웹 서버에 대한 두 개의 가상 호스트를 구성합니다.

* 서버 이름 (도메인 이름과 일치하는 이름) 는 `brandA.com` (줄 16) 및 `brandB.com` (32 행) 입니다.

* 각 가상 도메인의 문서 루트는 사이트 페이지가 포함된 Dispatcher 캐시에 있는 디렉토리입니다. (선 20 및 33)
* 각 가상 도메인에 대한 URL 다시 작성 규칙은 요청된 페이지의 경로를 캐시에 있는 페이지 경로와 접두사가 붙는 정규 표현식입니다. (선 19 및 35)
* 속성이로 `DispatherUseProcessedURL` 설정되어 `1`있습니다. (line 10)

예를 들어 웹 서버는 `https://brandA.com/en/products.html` URL 이 있는 요청을 받으면 다음 작업을 수행합니다.

* Associates the URL with the virtual host that has a `ServerName` of `brandA.com.`
* URL를 다시 작성합니다. `/content/sitea/en/products.html.`
* URL를 Dispatcher로 전달합니다.

### httpd. conf {#httpd-conf-1}

```xml
# load the Dispatcher module
LoadModule dispatcher_module modules/mod_dispatcher.so
# configure the Dispatcher module
<IfModule disp_apache2.c>
 DispatcherConfig conf/dispatcher.any
 DispatcherLog    logs/dispatcher.log  
 DispatcherLogLevel 3
 DispatcherNoServerHeader 0 
 DispatcherDeclineRoot 0
 DispatcherUseProcessedURL 1
 DispatcherPassError 0
</IfModule>

# Define virtual host for brandA.com
<VirtualHost *:80>
  ServerName branda.com
  DocumentRoot /usr/lib/apache/httpd-2.4.3/htdocs/content/sitea
  RewriteEngine  on
  RewriteRule    ^/(.*)\.html$  /content/sitea/$1.html [PT]
   <Directory /usr/lib/apache/httpd-2.4.3/htdocs/content/sitea>
     <IfModule disp_apache2.c>
       SetHandler dispatcher-handler
       ModMimeUsePathInfo On
     </IfModule>
     Options FollowSymLinks
     AllowOverride None
   </Directory>
</VirtualHost>

# define virtual host for brandB.com
<VirtualHost *:80>
  ServerName brandB.com
  DocumentRoot /usr/lib/apache/httpd-2.4.3/htdocs/content/siteb
  RewriteEngine  on
  RewriteRule    ^/(.*)\.html$  /content/siteb/$1.html [PT]
   <Directory /usr/lib/apache/httpd-2.4.3/htdocs/content/siteb>
     <IfModule disp_apache2.c>
       SetHandler dispatcher-handler
       ModMimeUsePathInfo On
     </IfModule>
     Options FollowSymLinks
     AllowOverride None
   </Directory>
</VirtualHost>

# document root for web server
DocumentRoot "/usr/lib/apache/httpd-2.4.3/htdocs"
```

### Dispatcher Farm 구성 {#configure-a-dispatcher-farm}

웹 서버가 URL를 다시 작성할 때 Dispatcher는 Dispatcher [구성에 따라 정의된 단일 팜이 필요합니다](dispatcher-configuration.md). 다음 구성은 웹 서버 가상 호스트 및 URL 이름 바꾸기 규칙을 지원하기 위해 필요합니다.

* `/virtualhosts` 속성은 모든 Virtualhost 정의에 대한 servername 값을 포함해야 합니다.
* `/statfileslevel` 각 도메인의 콘텐트 파일이 들어 있는 디렉토리에. htm 파일을 만들 수 있을 정도로 속성이 높아야 합니다.

다음 예제 구성 파일은 Dispatcher와 함께 설치되는 `dispatcher.any` 예제 파일을 기반으로 합니다. 이전 `httpd.conf` 파일의 웹 서버 구성을 지원하려면 다음 변경 사항이 필요합니다.

* `/virtualhosts` 이 속성은 디스패처가 `brandA.com` 및 `brandB.com` 도메인에 대한 요청을 처리하도록 합니다. (line 12)
* `/statfileslevel` 이 속성은 도메인의 웹 컨텐츠 (41 행) 가 포함된 각 디렉토리에 통계 파일이 작성되도록 2로 설정됩니다. `/statfileslevel "2"`

일반적으로 캐시의 문서 루트는 웹 서버 (40 행) 의 문서 루트와 동일합니다. `/usr/lib/apache/httpd-2.4.3/htdocs`

### `dispatcher.any` {#dispatcher-any}

```xml
/name "testDispatcher"
/farms
  {
  /dispfarm0
    {  
    /clientheaders
      {
      "*"
      }      
    /virtualhosts
      {
      "brandA.com" "brandB.com"
      }
    /renders
      {
      /rend01    {  /hostname "127.0.0.1"   /port "4503"  }
      }
    /filter
      {
      /0001 { /type "deny"  /glob "*" }
      /0023 { /type "allow" /glob "*/content*" }  # disable this rule to allow mapped content only
      /0041 { /type "allow" /glob "* *.css *"   }  # enable css
      /0042 { /type "allow" /glob "* *.gif *"   }  # enable gifs
      /0043 { /type "allow" /glob "* *.ico *"   }  # enable icos
      /0044 { /type "allow" /glob "* *.js *"    }  # enable javascript
      /0045 { /type "allow" /glob "* *.png *"   }  # enable png
      /0046 { /type "allow" /glob "* *.swf *"   }  # enable flash
      /0061 { /type "allow" /glob "POST /content/[.]*.form.html" }  # allow POSTs to form selectors under content
      /0062 { /type "allow" /glob "* /libs/cq/personalization/*"  }  # enable personalization
      /0081 { /type "deny"  /glob "GET *.infinity.json*" }
      /0082 { /type "deny"  /glob "GET *.tidy.json*"     }
      /0083 { /type "deny"  /glob "GET *.sysview.xml*"   }
      /0084 { /type "deny"  /glob "GET *.docview.json*"  }
      /0085 { /type "deny"  /glob "GET *.docview.xml*"  }      
      /0086 { /type "deny"  /glob "GET *.*[0-9].json*" }
      /0090 { /type "deny"  /glob "* *.query.json*" }
      }
    /cache
      {
      /docroot "/usr/lib/apache/httpd-2.4.3/htdocs"
      /statfileslevel "2"
      /allowAuthorized "0"
      /rules
        {
        /0000  { /glob "*"     /type "allow"  }
        }
      /invalidate
        {
        /0000  {   /glob "*" /type "deny"  }
        /0001 {  /glob "*.html" /type "allow"  }
        }
      /allowedClients
        {
        }     
      }
    /statistics
      {
      /categories
        {
        /html  { /glob "*.html" }
        /others  {  /glob "*"  }
        }
      }
    }
  }
```

>[!NOTE]
>
>단일 발송자 팜이 정의되었으므로 AEM 게시 인스턴스에서 복제 에이전트를 플러시하려면 특별한 구성이 필요하지 않습니다.

## 비 HTML 파일에 대한 링크 다시 작성 {#rewriting-links-to-non-html-files}

.html 또는.htm 이외의 확장자가 있는 파일에 대한 참조를 다시 작성하려면 Sling Rewriter Transformer 구성 요소를 만들어 기본 재작성기 파이프라인에 추가합니다.

웹 서버 컨텍스트에서 리소스 경로가 올바르게 확인되지 않을 때 참조를 다시 작성합니다. 예를 들어, 이미지 생성 구성 요소가 /content/sitea/en/products.navimage.png와 같은 링크를 만드는 경우 transformer가 필요합니다. 완벽한 기능을 갖춘 인터넷 [웹 사이트를](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/the-basics.html) 만드는 방법에 대한 Topnav 구성 요소는 이러한 링크를 만듭니다.

[Sling Rewriter](https://sling.apache.org/documentation/bundles/output-rewriting-pipelines-org-apache-sling-rewriter.html) 는 사후 처리 Sling 출력을 처리하는 모듈입니다. Rewriter의 SAX 파이프라인 구현은 Generator, One 이상의 Transformers 및 Serializer로 구성됩니다.

* **Generator:** sling 출력 스트림 (HTML 문서) 를 구문 분석하고 특정 요소 유형이 나타날 때 SAX 이벤트를 생성합니다.
* **변환기:** SAX 이벤트를 수신하고 결과적으로 이벤트 Target (HTML 요소) 를 수정합니다. 재작성기 파이프라인은 0 개 이상의 트랜스포머를 포함합니다. 변환기는 시퀀스로 실행되며 시퀀스의 다음 transformer로 SAX 이벤트를 전달합니다.
* **직렬 번호:** 각 transformer의 수정 사항을 포함하여 출력을 직렬화합니다.

![](assets/chlimage_1-15.png)

### AEM 기본 재작성기 파이프라인 {#the-aem-default-rewriter-pipeline}

AEM 에서는 텍스트/HTML 유형의 문서를 처리하는 기본 파이프라인 재작성기를 사용합니다.

* Generator는 HTML 문서를 구문 분석하여, a, img, area, form, base, link, script 및 body 요소가 발생할 때 SAX 이벤트를 생성합니다. Generator `htmlparser`별칭은 입니다.
* 파이프라인은 다음과 같은 트랜스포머를 포함합니다. `linkchecker``mobile``mobiledebug``contentsync`. `linkchecker` 변환기는 끊어진 링크를 방지하기 위해 참조된 HTML 또는 HTM 파일에 대한 패스를 외부화합니다.
* 직렬 변환기는 HTML 출력을 씁니다. 직렬 변환기 별칭은 Htmlwriter 입니다.

`/libs/cq/config/rewriter/default` 노드는 파이프라인을 정의합니다.

### 변압기 만들기 {#creating-a-transformer}

다음 작업을 수행하여 transformer 구성 요소를 만들고 파이프라인에서 사용합니다.

1. 인터페이스를 `org.apache.sling.rewriter.TransformerFactory` 구현합니다. 이 클래스는 transformer 클래스의 인스턴스를 만듭니다. `transformer.type` 속성 (transformer alias) 에 대한 값을 지정하고 클래스를 osgi 서비스 구성 요소로 구성합니다.
1. 인터페이스를 `org.apache.sling.rewriter.Transformer` 구현합니다. 작업을 최소화하려면 `org.apache.cocoon.xml.sax.AbstractSAXPipe` 클래스를 확장할 수 있습니다. Startelement 메서드를 재정의하여 다시 작성 비헤이비어를 사용자 정의합니다. 이 메서드는 transformer로 전달되는 모든 SAX 이벤트에 대해 호출됩니다.
1. 클래스를 번들로 묶을 수 있습니다.
1. AEM 애플리케이션에 구성 노드를 추가하여 트랜스포머를 파이프라인에 추가합니다.

>[!TIP]
>대신 Transformerfactory를 transformer가 정의된 모든 라이저에 삽입되도록 구성할 수 있습니다. 따라서 파이프라인을 구성할 필요가 없습니다.
>
>* `pipeline.mode` 속성을으로 설정합니다 `global`.
>* `service.ranking` 속성을 양의 정수로 설정합니다.
>* 속성을 포함하지 `pipeline.type` 마십시오.


>[!NOTE]
>
>컨텐츠 패키지 Maven 플러그인의 [다중 모듈](https://helpx.adobe.com/experience-manager/aem-previous-versions.html) archetype를 사용하여 Maven 프로젝트를 만듭니다. POMS는 컨텐츠 패키지를 자동으로 만들고 설치합니다.

다음 예제는 이미지 파일에 대한 참조를 다시 쓰는 변환기를 구현합니다.

* Myrewritertransformerfactory 클래스는 myrewritertransformer 객체를 인스턴스화합니다. pipeline. type 속성은 transformer 별칭을 mytransformer로 설정합니다. 파이프라인에 별칭을 포함시키기 위해 파이프라인 구성 노드에는 Transformers 목록에 이 별칭이 포함됩니다.
* Myrewritertransformer 클래스는 abstractsaxtransformer 클래스의 startelement 메서드를 재정의합니다. Startelement 메서드는 img 요소에 대한 src 특성의 값을 다시 씁니다.

이 예제는 강력한 것이 아니며 프로덕션 환경에서 사용되어서는 안 됩니다.

### Transformerfactory 구현 예 {#example-transformerfactory-implementation}

```java
package com.adobe.example;

import org.apache.felix.scr.annotations.Component;
import org.apache.felix.scr.annotations.Service;
import org.apache.felix.scr.annotations.Property;

import org.apache.sling.rewriter.Transformer;
import org.apache.sling.rewriter.TransformerFactory;

@Component
@Service
public class MyRewriterTransformerFactory implements TransformerFactory {
    /* Define the alias */
    @Property(value="mytransformer")
    static final String PIPELINE_TYPE ="pipeline.type";
 
    public Transformer createTransformer() {
        
        return new MyRewriterTransformer ();
    }
}
```

### 변환기 구현 예 {#example-transformer-implementation}

```java
package com.adobe.example;

import java.io.IOException;

import org.apache.cocoon.xml.sax.AbstractSAXPipe;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.rewriter.ProcessingComponentConfiguration;
import org.apache.sling.rewriter.ProcessingContext;
import org.apache.sling.rewriter.Transformer;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import org.xml.sax.Attributes;
import org.xml.sax.SAXException;
import org.xml.sax.helpers.AttributesImpl;

import javax.servlet.http.HttpServletRequest;

public class MyRewriterTransformer extends AbstractSAXPipe implements Transformer {

 private static final Logger log = LoggerFactory.getLogger(MyRewriterTransformer.class);
 private SlingHttpServletRequest httpRequest; 
 /* The element and attribute to act on  */
 private static final String ATT_NAME = new String("src");
 private static final String EL_NAME = new String("img");

 public MyRewriterTransformer () {
 }
 public void dispose() {
 }
 public void init(ProcessingContext context, ProcessingComponentConfiguration config) throws IOException {
  this.httpRequest = context.getRequest();
  log.debug("Transforming request {}.", httpRequest.getRequestURI());
 }
 @Override
 public void startElement (String nsUri, String localname, String qname, Attributes atts) throws SAXException {
  /* copy the element attributes */
  AttributesImpl linkAtts = new AttributesImpl(atts); 
  /* Only interested in EL_NAME elements */
  if(EL_NAME.equalsIgnoreCase(localname)){

   /* iterate through the attributes of the element and act only on ATT_NAME attributes */
   for (int i=0; i < linkAtts.getLength(); i++) {
    if (ATT_NAME.equalsIgnoreCase(linkAtts.getLocalName(i))) {
     String path_in_link = linkAtts.getValue(i);

     /* use the resource resolver of the http request to reverse-resolve the path  */
     String mappedPath = httpRequest.getResourceResolver().map(httpRequest, path_in_link);

     log.info("Tranformed {} to {}.", path_in_link,mappedPath);

     /* update the attribute value */
     linkAtts.setValue(i,mappedPath);
    }
   }

  }
        /* return updated attributes to super and continue with the transformer chain */
 super.startElement(nsUri, localname, qname, linkAtts);
 }
}
```

### 라이터 파이프라인에 변환기 추가 {#adding-the-transformer-to-a-rewriter-pipeline}

변환기를 사용하는 파이프라인을 정의하는 JCR 노드를 만듭니다. 다음 노드 정의는 텍스트/HTML 파일을 처리하는 파이프라인을 만듭니다. HTML에 대한 기본 AEM 생성기 및 파서가 사용됩니다.

>[!NOTE]
>
>transformer 속성을 `pipeline.mode` 로 `global`설정하면 파이프라인을 구성할 필요가 없습니다. `global` 모드는 모든 파이프라인에 트랜스포머를 삽입합니다.

### 재작성기 구성 노드 - XML 표현 {#rewriter-configuration-node-xml-representation}

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:jcr="https://www.jcp.org/jcr/1.0" xmlns:nt="https://www.jcp.org/jcr/nt/1.0"
    jcr:primaryType="nt:unstructured"
    contentTypes="[text/html]"
    enabled="{Boolean}true"
    generatorType="htmlparser"
    order="5"
    serializerType="htmlwriter"
    transformerTypes="[mytransformer]">
</jcr:root>
```

다음 그래픽은 노드의 CRXDE Lite 표현을 보여줍니다.

![](assets/chlimage_1-16.png)
