---
title: Dispatcher 구성
seo-title: Dispatcher 구성
description: Dispatcher를 구성하는 방법을 알아봅니다.
seo-description: Dispatcher를 구성하는 방법을 알아봅니다.
uuid: 253 EF 0 F 7-2491-4 CEC-AB 22-97439 DF 29 FD 6
cmgrlastmodified: 01.11.2007 08 22 29 [Aheimoz]
pageversionid: '1193211344162'
topic-tags: Dispatcher
content-type: 참조
discoiquuid: aeffee 8 e-bb 34-42 a 7-9 a 5 e-b 7 d 0 e 848391 a
translation-type: tm+mt
source-git-commit: 8e8638027c79015a4151539918a17069fb16873a

---


# Configuring Dispatcher{#configuring-dispatcher}

>[!NOTE]
>
>발송자 버전은 AEM와 독립적입니다. 이전 버전의 AEM에 대한 설명서에 포함된 Dispatcher 설명서에 대한 링크를 팔로우한 경우 이 페이지로 리디렉션되었을 수 있습니다.

다음 섹션에서는 Dispatcher의 다양한 측면을 구성하는 방법을 설명합니다.

## Support for IPv4 and IPv6 {#support-for-ipv-and-ipv}

AEM 및 Dispatcher의 모든 요소는 IPv 4 및 IPv 6 네트워크 모두에 설치할 수 있습니다. [IPV 4 및 IPV 6를 참조하십시오](https://helpx.adobe.com/experience-manager/6-3/sites/deploying/using/technical-requirements.html#AdditionalPlatformNotes).

## Dispatcher Configuration Files {#dispatcher-configuration-files}

By default the Dispatcher configuration is stored in the `dispatcher.any` text file, though you can change the name and location of this file during installation.

구성 파일에는 Dispatcher의 동작을 제어하는 단일 값 또는 다중 값 속성이 포함되어 있습니다.

* Property names are prefixed with a forward slash `/`.
* Multi-valued properties enclose child items using braces `{ }`.

예제 구성은 다음과 같이 구성됩니다.

```xml
# name of the dispatcher
/name "internet-server"

# each farm configures a set off (loadbalanced) renders
/farms
 {
  # first farm entry (label is not important, just for your convenience)
   /website 
     {  
     /clientheaders
       {
       # List of headers that are passed on
       }
     /virtualhosts
       {
       # List of URLs for this Web site
       }
     /sessionmanagement 
       {
       # settings for user authentification
       }
     /renders
       {
       # List of AEM instances that render the documents
       }
     /filter
       {
       # List of filters
       }
     /vanity_urls
       {
       # List of vanity URLs
       }
     /cache
       {
       # Cache configuration
       /rules
         {
         # List of cachable documents
         }
       /invalidate
         {
         # List of auto-invalidated documents
         }
       }
     /statistics
       {
       /categories
         {
         # The document categories that are used for load balancing estimates
         }
       }
     /stickyConnectionsFor "/myFolder"
     /health_check
       {
       # Page gets contacted when an instance returns a 500
       }
     /retryDelay "1"
     /numberOfRetries "5"
     /unavailablePenalty "1"
     /failover "1"
     }
 }
```

구성에 기여하는 다른 파일을 포함할 수 있습니다.

* 구성 파일이 큰 경우 더 작은 파일 (관리 용이 되는 파일) 로 분할하여 이러한 파일을 포함할 수 있습니다.
* 자동으로 생성되는 파일을 포함시킵니다.

예를 들어 myfarm 파일을 포함시키려면 /farms 구성에 다음 코드를 사용하십시오.

```xml
/farms
  {
  $include "myFarm.any"
  }
```

포함할 파일 범위를 지정하려면 별표 (&quot; *&quot;) 를 와일드카드로 사용합니다.

For example, if the files `farm_1.any` through to `farm_5.any` contain the configuration of farms one to five, you can include them as follows:

```xml
/farms
  {
  $include "farm_*.any"
  }
```

## Using Environment Variables {#using-environment-variables}

값을 하드 코딩하는 대신 Dispatcher의 문자열 값 속성에서 환경 변수를 사용할 수 있습니다. To include the value of an environment variable, use the format `${variable_name}`.

For example, if the dispatcher.any file is located in the same directory as the cache directory, the following value for the [docroot](dispatcher-configuration.md#main-pars-title-30) property can be used:

```xml
/docroot "${PWD}/cache"
```

As another example, if you create an environment variable named `PUBLISH_IP` that stores the hostname of the AEM publish instance, the following configuration of the [/renders](dispatcher-configuration.md#main-pars-127-25-0008) property can be used:

```xml
/renders {
  /0001 {
    /hostname "${PUBLISH_IP}"
    /port "8443"
  }
}
```

## Naming the Dispatcher Instance {#naming-the-dispatcher-instance-name}

Use the `/name` property to specify a unique name to identify your Dispatcher instance. The `/name` property is a top-level property in the configuration structure.

## Defining Farms {#defining-farms-farms}

`/farms` 속성은 하나 이상의 Dispatcher 비헤이비어를 정의하며, 여기서 각 세트는 다른 웹 사이트 또는 URL와 연결됩니다. `/farms` 속성에는 단일 농장 또는 여러 팜이 포함될 수 있습니다.

* Dispatcher를 사용하여 모든 웹 페이지 또는 웹 사이트를 동일한 방식으로 처리할 수 있습니다.
* 웹 사이트 또는 웹 사이트의 다른 영역에 다른 발송자의 비헤이비어가 필요한 경우 여러 팜을 만듭니다.

The `/farms` property is a top-level property in the configuration structure. To define a farm, add a child property to the `/farms` property. Dispatcher 인스턴스 내에서 팜을 고유하게 식별하는 속성 이름을 사용합니다.

`/*farmname*` 속성은 다중 값이며 Dispatcher 비헤이비어를 정의하는 다른 속성을 포함합니다.

* 팜이 적용되는 페이지의 URL.
* 문서 렌더링에 사용할 하나 이상의 서비스 URL (일반적으로 AEM 게시 인스턴스) 입니다.
* 여러 문서 렌더러를 로드 밸런싱하는 데 사용할 통계.
* 다른 몇 가지 비헤이비어를 참조하십시오.

값에는 영숫자 (a-z, 0-9) 문자를 포함할 수 있습니다. The following example shows the skeleton definition for two farms named `/daycom` and `/docsdaycom`:

```xml
#name of dispatcher
/name "day sites"

#farms section defines a list of farms or sites
/farms
{
   /daycom
   {
       ...
   }
   /docdaycom
   {
      ...
   }
}
```

>[!NOTE]
>
>둘 이상의 렌더링 팜을 사용하는 경우 목록이 상향식으로 평가됩니다. This is particularly relevant when defining [Virtual Hosts](dispatcher-configuration.md#main-pars-117-15-0006) for your websites.

각 farm 속성에는 다음 하위 속성이 포함될 수 있습니다.

| 속성 이름 | 설명 |
|--- |--- |
| [/홈 페이지](#specify-a-default-page-iis-only-homepage) | 기본 홈 페이지 (선택 사항) (IIS 에만 해당) |
| [/clientheaders](#specifying-the-http-headers-to-pass-through-clientheaders) | 클라이언트 HTTP 요청의 헤더를 통과합니다. |
| [/virtualhosts](#identifying-virtual-hosts-virtual-hosts) | 이 팜에 대한 가상 호스트입니다. |
| [/sessionmanagement](#enabling-secure-sessions-session-management) | 세션 관리 및 인증 지원 |
| [/renders](#defining-page-renderers-renders) | 렌더링된 페이지 (일반적으로 AEM 게시 인스턴스) 를 제공하는 서버입니다. |
| [/filter](#configuring-access-to-content-filter) | Dispatcher가 액세스를 활성화하는 URL를 정의합니다. |
| [/vanity_urls](#enabling-access-to-vanity-urls-vanity-urls) | 별칭 URL에 대한 액세스를 구성합니다. |
| [/propagateSyndPost](#forwarding-syndication-requests-propagate-syndpost) | 신디케이션 요청 전달을 지원합니다. |
| [/cache](#configuring-the-dispatcher-cache-cache) | 캐싱 비헤이비어를 구성합니다. |
| [/statistics](#configuring-load-balancing-statistics) | 로드 균형 계산을 위한 통계 카테고리 정의. |
| [/stickyConnectionsFor](#identifying-a-sticky-connection-folder-sticky-connections-for) | 고정 문서가 들어 있는 폴더입니다. |
| [/health_check](#specifying-a-health-check-page) | 서버 가용성을 판별하는 데 사용할 URL 입니다. |
| [/retryDelay](#specifying-the-page-retry-delay) | 실패한 연결을 다시 시도하기 전 지연. |
| [/unavailablePenalty](#reflecting-server-unavailability-in-dispatcher-statistics) | 로드 균형 계산에 대한 통계에 영향을 주는 벌금. |
| [/failover](#using-the-fail-over-mechanism) | 원본 요청이 실패하면 다른 렌더링에 대한 요청을 다시 전송합니다. |

## Specify a Default Page (IIS Only) - /homepage {#specify-a-default-page-iis-only-homepage}

>[!CAUTION]
>
>`/homepage`매개 변수 (iis 전용) 가 더 이상 작동하지 않습니다. Instead, you should use the [IIS URL Rewrite Module](https://docs.microsoft.com/en-us/iis/extensions/url-rewrite-module/using-the-url-rewrite-module).
>
>If you are using Apache, you should use the `mod_rewrite` module. See the Apache web site documentation for information about `mod_rewrite` (for example, [Apache 2.4](https://httpd.apache.org/docs/current/mod/mod_rewrite.html)). When using `mod_rewrite`, it is advisable to use the flag ** [&#39;passthrough|PT&#39; (pass through to next handler)](https://helpx.adobe.com/dispatcher/kb/DispatcherModReWrite.html)** to force the rewrite engine to set the `uri` field of the internal `request_rec` structure to the value of the `filename` field.

<!-- 

Comment Type: draft

<p>The optional /homepage parameter specifies the page that Dispatcher returns when a client requests an undeterminable page or file.</p> 
<p>Typically this situation occurs when a user specifies an URL for which neither IIS or AEM provides an automatic redirection target. For example, if the AEM render instance is shut down after the content is cached, the content redirect URL is unavailable.</p> 
<p>The following example configuration displays the <span class="code">index.html</span> page in such circumstances:</p>

 -->

<!-- 

Comment Type: draft

<codeblock gutter="true" class="syntax xml">
  /homepage&nbsp;"/index.html" 
</codeblock>

 -->

<!-- 

Comment Type: draft

<p>The <span class="code">/homepage</span> section is located inside the <span class="code">/farms</span> section, for example:<br /> </p>

 -->

<!-- 

Comment Type: draft

<codeblock gutter="true" class="syntax xml">
  #name&nbsp;of&nbsp;dispatcher!!discoiqbr!!/name&nbsp;"day&nbsp;sites"!!discoiqbr!!!!discoiqbr!!#farms&nbsp;section&nbsp;defines&nbsp;a&nbsp;list&nbsp;of&nbsp;farms&nbsp;or&nbsp;sites!!discoiqbr!!/farms!!discoiqbr!!{!!discoiqbr!!&nbsp;&nbsp;&nbsp;/daycom!!discoiqbr!!&nbsp;&nbsp;&nbsp;{!!discoiqbr!!&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/homepage&nbsp;"/index.html"!!discoiqbr!!&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;...!!discoiqbr!!&nbsp;&nbsp;&nbsp;}!!discoiqbr!!&nbsp;&nbsp;&nbsp;/docdaycom!!discoiqbr!!&nbsp;&nbsp;&nbsp;{!!discoiqbr!!&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;...!!discoiqbr!!&nbsp;&nbsp;&nbsp;}!!discoiqbr!!} 
</codeblock>

 -->

## Specifying the HTTP Headers to Pass Through {#specifying-the-http-headers-to-pass-through-clientheaders}

`/clientheaders` 속성은 클라이언트 HTTP 요청에서 렌더러로 전달하는 HTTP 헤더 목록을 정의합니다 (AEM 인스턴스).

기본적으로 Dispatcher는 표준 HTTP 헤더를 AEM 인스턴스로 전달합니다. 경우에 따라 추가 헤더를 전달하거나 특정 헤더를 제거할 수 있습니다.

* AEM 인스턴스에서 HTTP 요청에 기대하는 헤더 (예: 사용자 지정 헤더) 를 추가합니다.
* 웹 서버와 관련이 있는 헤더 (예: 인증 헤더) 를 제거합니다.

전달할 헤더 세트를 사용자 지정하는 경우 기본적으로 포함되는 헤더 등 전체 헤더 목록을 지정해야 합니다.

For example, a Dispatcher instance that handles page activation requests for publish instances requires the `PATH` header in the `/clientheaders` section. `PATH` 헤더를 사용하면 복제 에이전트와 디스패처 간의 통신이 가능합니다.

The following code is an example configuration for `/clientheaders`:

```shell
/clientheaders
  {
  "CSRF-Token"
  "X-Forwarded-Proto"
  "referer"
  "user-agent"
  "authorization"
  "from"
  "content-type"
  "content-length"
  "accept-charset"
  "accept-encoding"
  "accept-language"
  "accept"
  "host"
  "if-match"
  "if-none-match"
  "if-range"
  "if-unmodified-since"
  "max-forwards"
  "proxy-authorization"
  "proxy-connection"
  "range"
  "cookie"
  "cq-action"
  "cq-handle"
  "handle"
  "action"
  "cqstats"
  "depth"
  "translate"
  "expires"
  "date"
  "dav"
  "ms-author-via"
  "if"
  "lock-token"
  "x-expected-entity-length"
  "destination"
  "PATH"
  }
```

## Identifying Virtual Hosts {#identifying-virtual-hosts-virtualhosts}

`/virtualhosts` 속성은 디스패처가 이 팜에 대해 허용하는 모든 호스트 이름/URI 조합의 목록을 정의합니다. 별표 (&quot; *&quot;) 문자를 와일드카드로 사용할 수 있습니다. Values for the / `virtualhosts` property use the following format:

```xml
[scheme]host[uri][*]
```

* `scheme`: (선택 사항) either `https://` or `https://.`
* `host`: 호스트 컴퓨터의 이름 또는 IP 주소와 필요한 경우 포트 번호입니다. (See [https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.23](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.23))
* `uri`: (선택 사항) 리소스 경로.

다음 예제 구성은 mycompany의.com 및. ch 도메인과 mysubdivision의 모든 도메인에 대한 요청을 처리합니다.

```xml
   /virtualhosts
    {
    "www.myCompany.com"
    "www.myCompany.ch"
    "www.mySubDivison.*"
    }
```

The following configuration handles *all* requests:

```xml
   /virtualhosts
    {
    "*"
    }
```

### Resolving the Virtual Host {#resolving-the-virtual-host}

When Dispatcher receives an HTTP or HTTPS request, it finds the virtual host value that best-matches the `host,` `uri`, and `scheme` headers of the request. Dispatcher evaluates the values in the `virtualhosts` properties in the following order:

* Dispatcher는 가장 낮은 농장에서 시작하여 Dispatcher. any 파일에서 위쪽으로 진행합니다.
* For each farm, Dispatcher begins with the topmost value in the `virtualhosts` property and progresses down the list of values.

Dispatcher는 다음 방법으로 가장 일치하는 가상 호스트 값을 찾습니다.

* The first-encountered virtual host that matches all three of the `host`, the `scheme`, and the `uri` of the request is used.
* `virtualhosts` 값과 요청이 `scheme` 모두 일치하는 `uri``scheme``uri` 부분이 없는 경우 요청의 일치와 `host` 일치하는 첫 번째 발견된 가상 호스트가 사용됩니다.
* If no `virtualhosts` values have a host part that matches the host of the request, the topmost virtual host of the topmost farm is used.

Therefore, you should place your default virtual host at the top of the `virtualhosts` property in the topmost farm of your dispatcher.any file.

### Example Virtual Host Resolution {#example-virtual-host-resolution}

The following example represents a snippet from a dispatcher.any file that defines two Dispatcher farms, and each farm defines a `virtualhosts` property.

```xml
/farms
  {
  /myProducts 
    { 
    /virtualhosts
      {
      "www.mycompany.com"
      }
    /renders
      {
      /hostname "server1.myCompany.com"
      /port "80"
      }
    }
  /myCompany 
    { 
    /virtualhosts
      {
      "www.mycompany.com/products/*"
      }
    /renders
      {
      /hostname "server2.myCompany.com"
      /port "80"
      }
    }
  }
```

다음 표는 해당 HTTP 요청에 대해 해결된 가상 호스트를 보여 줍니다.

| 요청 URL | 해결된 가상 호스트 |
|---|---|
| `https://www.mycompany.com/products/gloves.html` | `www.mycompany.com/products/*;` |
| `https://www.mycompany.com/about.html` | `www.mycompany.com` |

## Enabling Secure Sessions - /sessionmanagement {#enabling-secure-sessions-sessionmanagement}

>[!CAUTION]
>
>`/allowAuthorized`**이** 기능을 `"0"` 활성화하려면 `/cache` 섹션에서 설정되어야 합니다.

사용자가 팜의 모든 페이지에 액세스하려면 로그인 팜에 액세스할 수 있도록 보안 세션을 만듭니다. 로그인하면 사용자는 팜의 모든 페이지에 액세스할 수 있습니다. See [Creating a Closed User Group](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/cug.html#CreatingTheUserGroupToBeUsed) for information about using this feature with CUGs.

`/sessionmanagement` 속성은의 하위 속성입니다 `/farms`.

>[!CAUTION]
>
>웹 사이트의 섹션이 다른 액세스 요구 사항을 사용하는 경우 여러 팜을 정의해야 합니다.

**/sessionmanagement** 에는 다음과 같은 몇 가지 하위 매개 변수가 있습니다.

**/directory** (필수)

세션 정보를 저장하는 디렉토리입니다. 디렉토리가 없으면 해당 디렉토리가 생성됩니다.

**/encode** (선택 사항)

세션 정보를 인코딩하는 방법입니다. MD 5 알고리즘을 사용하여 암호화를 위해 &quot;MD 5&quot; 를 사용하거나 16 진수 인코딩에 &quot;hex&quot; 를 사용합니다. 세션 데이터를 암호화하는 경우 파일 시스템에 대한 액세스 권한이 있는 사용자는 세션 콘텐트를 읽을 수 없습니다. 기본값은 &quot;MD 5&quot; 입니다.

**/header** (선택 사항)

인증 정보를 저장하는 HTTP 헤더 또는 쿠키의 이름. If you store the information in the http header, use `HTTP:<*header-name*>`. To store the information in a cookie, use `Cookie:<header-name>`. If you do not specify a value `HTTP:authorization` is used.

**/timeout** (선택 사항)

세션이 마지막으로 사용된 후 시간이 초과될 때까지의 시간 (초) 입니다. 지정하지 않은 경우 &quot;800&quot; 이 사용되므로 사용자의 마지막 요청 이후 13 분 이상 세션이 시간 초과됩니다.

예제 구성의 모양은 다음과 같습니다.

```xml
/sessionmanagement 
  { 
  /directory "/usr/local/apache/.sessions" 
  /encode "md5" 
  /header "HTTP:authorization" 
  /timeout "800" 
  }
```

## Defining Page Renderers {#defining-page-renderers-renders}

/renders 속성은 Dispatcher가 문서를 렌더링하기 위해 요청을 보내는 URL를 정의합니다. The following example `/renders` section identifies a single AEM instance for rendering:

```xml
/renders
  {
    /myRenderer
      {
      # hostname or IP of the renderer
      /hostname "aem.myCompany.com"
      # port of the renderer
      /port "4503"
      # connection timeout in milliseconds, "0" (default) waits indefinitely
      /timeout "0"
      }
  }
```

다음 예제 /renders 섹션은 Dispatcher와 동일한 컴퓨터에서 실행되는 AEM 인스턴스를 식별합니다.

```xml
/renders
  {
    /myRenderer
     {
     /hostname "127.0.0.1"
     /port "4503"
     }
  }
```

다음 예제 /renders 섹션에서는 두 개의 AEM 인스턴스 간에 균등하게 렌더링 요청을 배포합니다.

```xml
/renders
  {
    /myFirstRenderer
      {
      /hostname "aem.myCompany.com"
      /port "4503"
      }
    /mySecondRenderer
      {
      /hostname "127.0.0.1"
      /port "4503"
      }
  }
```

### Renders options {#renders-options}

**/timeout**

AEM 인스턴스에 액세스하는 연결 시간 초과를 밀리초 단위로 지정합니다. 기본값은 &quot;0&quot; 이므로 디스패처가 무기한으로 대기합니다.

**/receiveTimeout**

응답이 허용되는 시간 (밀리초) 를 지정합니다. 기본값은 &quot;600000&quot; 이며, 디스패처는 10 분 동안 기다립니다. &quot; 0 &quot;을 설정하면 제한 시간이 완전히 제거됩니다.\
응답 헤더를 구문 분석하는 동안 시간 초과에 도달한 경우 504 (잘못된 게이트웨이) 의 HTTP 상태가 반환됩니다. 응답 본문을 읽는 동안 시간 초과가 감지되면, 디스패처는 클라이언트에 불완전한 응답을 반환하지만 작성되었을 수 있는 캐시 파일을 삭제합니다.

**/ipv4**

Specifies whether Dispatcher uses the `getaddrinfo` function (for IPv6) or the `gethostbyname` function (for IPv4) for obtaining the IP address of the render. A value of 0 causes `getaddrinfo` to be used. A value of 1 causes `gethostbyname` to be used. 기본값은 0 입니다.

Getaddrinfo 함수는 IP 주소 목록을 반환합니다. Dispatcher는 TCP/IP 연결을 설정하기 전까지 주소 목록을 반복합니다. 따라서 render 호스트 이름이 mutliple IP 주소와 연결되어 있고, 호스트가 getaddrinfo 함수에 대한 응답으로 항상 동일한 순서에 있는 IP 주소 목록을 반환하는 경우 ipv 4 속성이 중요합니다. 이 경우, 디스패처가 연결되어 있는 IP 주소가 랜덤화되도록 gethostbyname 함수를 사용해야 합니다.

Amazon Elastic Load Balancing (ELB) 는 잠재적으로 동일한 IP 주소 목록을 사용하여 getaddrinfo에 응답하는 서비스입니다.

**/secure**

`/secure` 속성 값이 &quot;1&quot; 인 경우, HTTPS를 사용하여 AEM 인스턴스와 통신합니다. For additional details, also see [Configuring Dispatcher to Use SSL](dispatcher-ssl.md#configuring-dispatcher-to-use-ssl).

**/always-resolve**

With Dispatcher version **4.1.6**, you can configure the `/always-resolve` property as follows:

* &quot; 1 &quot;으로 설정하면 모든 요청에서 호스트 이름이 해결됩니다 (디스패처는 어떠한 IP 주소도 캐시하지 않음). 각 요청에 대해 호스트 정보를 가져오는 데 필요한 추가 호출로 인해 성능에 약간의 영향이 있을 수 있습니다.
* 속성이 설정되어 있지 않으면 기본적으로 IP 주소가 캐시됩니다.

또한 다음 샘플에서와 같이, 동적 IP 해상도 문제가 발생할 경우 이 속성을 사용할 수 있습니다.

```xml
/rend {
  /0001 {
     /hostname "host-name-here"
     /port "4502"
     /ipv4 "1"
     /always-resolve "1"
     }
  }
```

## Configuring Access to Content {#configuring-access-to-content-filter}

Use the `/filter` section to specify the HTTP requests that Dispatcher accepts. 다른 모든 요청은 404 오류 코드가 있는 웹 서버로 다시 전송됩니다 (페이지를 찾을 수 없음). If no `/filter` section exists, all requests are accepted.

**참고:** [Statfile](dispatcher-configuration.md#main-pars-title-28) 에 대한 요청은 항상 거부됩니다.

>[!CAUTION]
>
>See the [Dispatcher Security Checklist](security-checklist.md) for further considerations when restricting access using Dispatcher. Also, read the [AEM Security Cheklist](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/security-checklist.html) for additional security details regarding your AEM installation.

/filter 섹션은 HTTP 요청의 요청선 부분에 있는 패턴에 따라 컨텐츠에 대한 액세스를 거부하거나 허용하는 일련의 규칙으로 구성됩니다. /filter 섹션에 대한 Whilist 전략을 사용해야 합니다.

* 첫째, 모든 항목에 대한 액세스를 거부하십시오.
* 필요한 경우 컨텐츠에 대한 액세스를 허용합니다.

### Defining a Filter {#defining-a-filter}

`/filter` 섹션의 각 항목에는 요청 라인 또는 전체 요청 라인의 특정 요소와 일치하는 유형과 패턴이 포함됩니다. 각 필터에는 다음 항목이 포함될 수 있습니다.

* **** 유형: 는 `/type` 패턴과 일치하는 요청에 대한 액세스를 허용할지 또는 거부할지 여부를 나타냅니다. The value can be either `allow` or `deny`.

* **element of the request line:** HTTP `/method``/url`요청의 `/query``/protocol` 요청선 부분에 있는 이러한 특정 부분에 따른 요청을 필터링하는 패턴을 포함합니다. 전체 요청 라인이 아닌 요청 라인의 요소에 대한 필터링은 기본 필터 방법입니다.

* **요청 라인의 고급 요소:** Dispatcher 4.2.0 부터는 4 개의 새로운 필터 요소를 사용할 수 있습니다. These new elements are `/path`, `/selectors`, `/extension` and `/suffix` respectively. 하나 이상의 항목을 포함하여 URL 패턴을 추가로 제어할 수 있습니다.

>[!NOTE]
>
>For more information about what part of the request line each of these elements references, see the [Sling URL Decomposition](https://sling.apache.org/documentation/the-sling-engine/url-decomposition.html) wiki page.

* **GLOB 속성**: 속성은 `/glob` HTTP 요청의 전체 요청 라인과 일치하기 위해 사용됩니다.

>[!CAUTION]
>
>Globs로 필터링은 Dispatcher에서 더 이상 사용되지 않습니다. As such, you should avoid using globs in the `/filter` sections since it may lead to security issues. 따라서,

`/glob "* *.css *"`

You should use

`/url "*.css"`

#### The request-line Part of HTTP Requests {#the-request-line-part-of-http-requests}

HTTP/1.1 defines the [request-line](https://www.w3.org/Protocols/rfc2616/rfc2616-sec5.html) as follows:

*method request-uri http-version*&lt; crlf &gt;

&lt; CRLF &gt; 문자는 캐리지 리턴과 라인 피드를 따릅니다. 다음 예제는 클라이언트가 Geometrixx-Outoors 사이트의 en 페이지를 요청할 때 받는 요청줄입니다.

GET /content/geometrixx-outdoors/en.html http .1 .1 &lt; CRLF &gt;

패턴은 요청 라인과 &lt; CRLF &gt; 문자의 공백 문자를 고려해야 합니다.

#### Double-quotes vs Single-quotes {#double-quotes-vs-single-quotes}

When creating your filter rules, use double quotation marks `"pattern"` for simple patterns. If you are using Dispatcher 4.2.0 or later and your pattern includes a regular expression, you must enclose the regex pattern `'(pattern1|pattern2)'` within single quotation marks.

#### Regular Expressions {#regular-expressions}

Dispatcher 4.2.0 부터는 필터 패턴에서 POSIX Extended 정규 표현식을 포함할 수 있습니다.

#### Troubleshooting Filters {#troubleshooting-filters}

If your filters are not triggering in the way you would expect, enable [Trace Logging](#trace-logging) on dispatcher so you can see which filter is intercepting the request.

#### Example Filter: Deny All {#example-filter-deny-all}

다음 필터 섹션은 디스패처가 모든 파일에 대한 요청을 거부하게 합니다. 모든 파일에 대한 액세스를 거부한 다음 특정 영역에 대한 액세스를 허용해야 합니다.

```xml
  /0001  { /glob "*" /type "deny" }
```

명시적으로 거부된 영역을 요청하면 404 오류 코드 (페이지가 발견되지 않음) 가 반환됩니다.

#### Example Filter: Deny Acess to Specific Areas {#example-filter-deny-acess-to-specific-areas}

또한 필터를 사용하면 ASP 페이지와 게시 인스턴스 내의 중요한 영역에 대한 액세스를 거부할 수 있습니다. 다음 필터는 ASP 페이지에 대한 액세스를 거부합니다.

```xml
/0002  { /type "deny" /url "*.asp"  }
```

#### Example Filter: Enable POST Requests {#example-filter-enable-post-requests}

다음 예제 필터를 사용하면 post 메서드로 양식 데이터를 제출할 수 있습니다.

```xml
/filter {
    /0001  { /glob "*" /type "deny" }
    /0002 { /type "allow" /method "POST" /url "/content/[.]*.form.html" }
}
```

#### Example Filter: Allow Access to the Workflow Console {#example-filter-allow-access-to-the-workflow-console}

다음 예는 워크플로우 콘솔에 대한 외부 액세스를 거부하는 데 사용되는 필터를 보여줍니다.

```xml
/filter {
    /0001  { /glob "*" /type "deny" }
    /0002  {  /type "allow"  /url "/libs/cq/workflow/content/console*"  }
}
```

게시 인스턴스에서 웹 애플리케이션 컨텍스트를 사용하는 경우 (예: 게시) 필터 정의에 추가할 수도 있습니다.

```xml
/0003   { /type "deny"  /url "/publish/libs/cq/workflow/content/console/archive*"  }
```

제한된 영역 내의 단일 페이지에 액세스해야 하는 경우에도 액세스 권한을 허용할 수 있습니다. 예를 들어 워크플로우 콘솔 내의 보관 탭에 대한 액세스를 허용하려면 다음 섹션을 추가합니다.

```xml
/0004  { /type "allow"  /url "/libs/cq/workflow/content/console/archive*"   }
```

>[!NOTE]
>
>여러 필터 패턴이 요청에 적용되면 적용되는 마지막 필터 패턴이 효과적입니다.

#### Example filter: Using Regular Expressions {#example-filter-using-regular-expressions}

이 필터는 작은 따옴표 사이에 정의된 정규 표현식을 사용하여 비공개 컨텐츠 디렉토리의 익스텐션을 활성화합니다.

```xml
/005  {  /type "allow" /extension '(css|gif|ico|js|png|swf|jpe?g)' }
```

#### Example filter: Filter Additional Elements of a Request URL {#example-filter-filter-additional-elements-of-a-request-url}

Below is a rule example that blocks content grabbing from the `/content` path and its subtree, using filters for path, selectors and extensions:

```xml
/006 {
        /type "deny"
        /path "/content/*"
        /selectors '(feed|rss|pages|languages|blueprint|infinity|tidy)'
        /extension '(json|xml|html)'
        }
```

### Example /filter section {#example-filter-section}

Dispatcher를 구성할 때는 외부 액세스를 가능한 한 제한해야 합니다. 다음 예에서는 외부 방문자에게 최소한의 액세스를 제공합니다.

* `/content`
* 디자인 및 클라이언트 라이브러리와 같은 기타 컨텐츠 예를 들면 다음과 같습니다.

   * `/etc/designs/default*`
   * `/etc/designs/mydesign*`

After you create filters, [test page access](dispatcher-configuration.md#main-pars-title-19) to ensure your AEM instance is secure.

The following /filter section of the dispatcher.any file can be used as a basis in your [Dispatcher configuration](dispatcher-configuration.md) file.

이 예는 Dispatcher와 함께 제공되는 기본 구성 파일을 기반으로 하며 프로덕션 환경에서 사용하기 위한 예입니다. # is deactivated with # is deactivated (comment out), if you choose to activate any of these (by the # on that line) as this can have a security impact as this can have security impact.

모든 항목에 대한 액세스를 거부한 다음 특정 (제한된) 요소에 대한 액세스를 허용해야 합니다.

<!-- 

Comment Type: remark
Last Modified By: unknown unknown (ims-author-00AF43764F54BE740A490D44@AdobeID)
Last Modified Date: 2015-06-26T04:32:37.986-0400

<p>We should mention the config files that are shipped with the dispatcher distribution and only give a few examples here. This aims to avoid confusion and reduce content maintenance.<br /> </p>

 -->

```xml
  /filter
      {
      # Deny everything first and then allow specific entries
      /0001 { /type "deny" /glob "*" }
      
      # Open consoles
#     /0011 { /type "allow" /url "/admin/*"  }  # allow servlet engine admin
#     /0012 { /type "allow" /url "/crx/*"    }  # allow content repository
#     /0013 { /type "allow" /url "/system/*" }  # allow OSGi console
        
      # Allow non-public content directories
#     /0021 { /type "allow" /url "/apps/*"   }  # allow apps access
#     /0022 { /type "allow" /url "/bin/*"    }
      /0023 { /type "allow" /url "/content*" }  # disable this rule to allow mapped content only
      
#     /0024 { /type "allow" /url "/libs/*"   }
#     /0025 { /type "deny"  /url "/libs/shindig/proxy*" } # if you enable /libs close access to proxy

#     /0026 { /type "allow" /url "/home/*"   }
#     /0027 { /type "allow" /url "/tmp/*"    }
#     /0028 { /type "allow" /url "/var/*"    }

      # Enable extensions in non-public content directories, using a regular expression
      /0041
        {
        /type "allow"
        /extension '(css|gif|ico|js|png|swf|jpe?g)'
        }

      # Enable features 
      /0062 { /type "allow" /url "/libs/cq/personalization/*"  }  # enable personalization

      # Deny content grabbing, on all accessible pages, using regular expressions
      /0081
        {
        /type "deny"
        /selectors '((sys|doc)view|query|[0-9-]+)'
        /extension '(json|xml)'
        }
      # Deny content grabbing for /content and its subtree
      /0082
        {
        /type "deny"
        /path "/content/*"
        /selectors '(feed|rss|pages|languages|blueprint|infinity|tidy)'
        /extension '(json|xml|html)'
        }

#     /0087 { /type "allow" /method "GET" /extension 'json' "*.1.json" }  # allow one-level json requests
}
```

>[!NOTE]
>
>Apache와 함께 사용하는 경우 Dispatcher 모듈의 dispatcheruseprocessedurl 속성에 따라 필터 URL 패턴을 디자인합니다. [(Apache 웹 서버 - Dispatcher 용 Apache 웹 서버 구성을](dispatcher-install.md#main-pars-55-35-1022)참조하십시오.)

>[!NOTE]
>
>Dynamic Media에 대한 필터 0030 및 0031는 AEM 6.0 이상에 적용할 수 있습니다.

액세스를 확장하도록 선택하는 경우 다음 권장 사항을 고려하십시오.

* External access to `/admin` should always be *completely* disabled if you are using CQ version 5.4 or an earlier version.

* Care must be taken when allowing access to files in `/libs`. 액세스 권한은 개별적으로 허용되어야 합니다.
* 복제할 수 없도록 복제 구성에 대한 액세스를 거부합니다.

   * `/etc/replication.xml*`
   * `/etc/replication.infinity.json*`

* Google 가젯 역방향 프록시에 대한 액세스를 거부합니다.

   * `/libs/opensocial/proxy*`

Depending on your installation, there might be additional resources under `/libs`, `/apps` or elsewhere, that must be made available. You can use the `access.log` file as one method of determining resources that are being accessed externally.

>[!CAUTION]
>
>콘솔 및 디렉토리에 대한 액세스를 통해 프로덕션 환경에 대한 보안 위험을 초래할 수 있습니다. 명시적 합성이 없는 경우, 비활성화 (주석 처리) 되어야 합니다.

>[!CAUTION]
>
>If you are [using reports in a publish environment](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/reporting.html#UsingReportsinaPublishEnvironment) you should configure Dispatcher to deny access to `/etc/reports` for external visitors.

### Restricting Query Strings {#restricting-query-strings}

Since Dispatcher version 4.1.5, use the `/filter` section to restrict query strings. It is highly recommended to explicitly allow query strings and exclude generic allowance through `allow` filter elements.

A single entry can have either *glob* or some combination of *method*,*url*,*query* and *version* but not both. The following example allows the `a=*` query string and denies all other query strings for URLs that resolve to the `/etc` node:

```xml
/filter {
 /0001 { /type "deny" /method "POST" /url "/etc/*" }
 /0002 { /type "allow" /method "GET" /url "/etc/*" /query "a=*" }
}
```

>[!NOTE]
>
>If a rule contains a `/query`, it will only match requests that contain a query string and match the provided query pattern.
>
>In above example, if requests to `/etc` that have no query string should be allowed as well, the following rules would be required:


```xml
/filter {  
>/0001 { /type "deny" /method “*" /url "/path/*" }  
>/0002 { /type "allow" /method "GET" /url "/path/*" }  
>/0003 { /type “deny" /method "GET" /url "/path/*" /query "*" }  
>/0004 { /type "allow" /method "GET" /url "/path/*" /query "a=*" }  
}  
```

### Testing Dispatcher Security {#testing-dispatcher-security}

Dispatcher 필터는 AEM 게시 인스턴스에서 다음 페이지 및 스크립트에 대한 액세스를 차단해야 합니다. 웹 브라우저를 사용하여 사이트 방문자의 다음 페이지를 열고 코드 404가 반환되었는지 확인합니다. 다른 결과가 있으면 필터를 조정합니다.

/content/add_valid_page.html에 대한 일반적인 페이지 렌더링을 보셨습니까? debug = layout.


* /admin
* /system/console
* /dav/crx.default
* /crx
* /bin/crxde/logs
* /jcr: system/jcr: versionstorage. json
* /_jcr_system/_jcr_versionStorage.json
* /libs/wcm/core/content/siteadmin.html
* /libs/collab/core/content/admin.html
* /libs/cq/ui/content/dumplibs.html
* /var/linkchecker.html
* /etc/linkchecker.html
* /home/users/a/admin/profile.json
* /home/users/a/admin/profile.xml
* /libs/cq/core/content/login.json
* /content/../libs/foundation/components/text/text.jsp
* /컨텐츠/.{.}/libs/foundation/components/text/text/text. jsp
* /apps/sling/config/org.apache.felix.webconsole.internal.servlet.OsgiManager.config/jcr % 3 Acontent/jcr % 3 adata
* /libs/foundation/components/primary/cq/workflow/components/participants/json.GET.servlet
* /content.pages.json
* /content.languages.json
* /content.blueprint.json
* /컨텐츠.-1. json
* /content.10.json
* /content.infinity.json
* /content.tidy.json
* /content.tidy.-1. blubber. json
* /content/dam.tidy.-100. json
* /content/content/geometrixx.sitemap.txt
* /content/add_valid_page.query.json? statement =//*
* /content/add_valid_page.qu % 65 ry. js % 6 fn? statement =//*
* /content/add_valid_page.query.json?statement=//*[@transportPassword]/(@transportPassword%20|%20@transportUri%20|%20@transportUser)
* /content/add_valid_path_to_a_page/_jcr_content.json
* /content/add_valid_path_to_a_page/jcr: content. json
* /content/add_valid_path_to_a_page/_jcr_content.feed
* /content/add_valid_path_to_a_page/jcr: content. feed
* /content/add_valid_path_to_a_page/pagename._ jcr_ content. feed
* /content/add_valid_path_to_a_page/pagename.jcr: content. feed
* /content/add_valid_path_to_a_page/pagename.docview.xml
* /content/add_valid_path_to_a_page/pagename.docview.json
* /content/add_valid_path_to_a_page/pagename.sysview.xml
* /etc.xml
* /content.feed.xml
* /content.rss.xml
* /content.feed.html
* /content/add_valid_page.html? debug = layout
* /projects
* /tagging
* /etc/replication.html
* /etc/cloudservices.html
* /시작

터미널 또는 명령 프롬프트에서 다음 명령을 실행하여 익명 쓰기 액세스 활성화 여부를 결정합니다. 노드에 데이터를 쓸 수 없습니다.

`curl -X POST "https://anonymous:anonymous@hostname:port/content/usergenerated/mytestnode"`

터미널 또는 명령 프롬프트에서 다음 명령을 실행하여 디스패처 캐시를 무효화하고 코드 404 응답을 수신했는지 확인합니다.

`curl -H "CQ-Handle: /content" -H "CQ-Path: /content" https://yourhostname/dispatcher/invalidate.cache`

## Enabling Access to Vanity URLs {#enabling-access-to-vanity-urls-vanity-urls}

<!-- 

Comment Type: remark
Last Modified By: unknown unknown (sbroders@adobe.com)
Last Modified Date: 2015-03-25T14:23:05.185-0400

<p style="font-family: tahoma, arial, helvetica, sans-serif; font-size: 12px;">For https://jira.corp.adobe.com/browse/DOC-4812</p> 
<p style="font-family: tahoma, arial, helvetica, sans-serif; font-size: 12px;">The "com.adobe.granite.dispatcher.vanityurl.content" package needs to be made public before publishing this contnet.</p>
 -->

CQ 또는 AEM 페이지에 대해 구성된 별칭 URL에 액세스할 수 있도록 Dispatcher를 구성합니다.

별칭 URL에 대한 액세스가 활성화되면, Dispatcher는 Render 인스턴스에서 실행되는 서비스를 정기적으로 호출하여 별칭 URL 목록을 얻습니다. Dispatcher는 이 목록을 로컬 파일에 저장합니다. `/filter` 섹션의 필터로 인해 페이지에 대한 요청이 거부되면, Dispatcher는 별칭 URL 목록을 참조합니다. 거부된 URL 이 목록에 있으면, Dispatcher는 별칭 URL에 대한 액세스를 허용합니다.

To enable access to vanity URLs, add a `/vanity_urls` section to the `/farms` section, similar to the following example:

```xml
 /vanity_urls {
      /url "/libs/granite/dispatcher/content/vanityUrls.html"
      /file "/tmp/vanity_urls"
      /delay 300
 }
```

The `/vanity_urls` section contains the following properties:

* `/url`: Render 인스턴스에서 실행되는 별칭 URL 서비스의 경로입니다. The value of this property must be `"/libs/granite/dispatcher/content/vanityUrls.html"`.

* `/file`: 디스패처가 별칭 URL 목록을 저장하는 로컬 파일의 경로입니다. Dispatcher에서 이 파일에 대한 쓰기 권한을 가지고 있는지 확인합니다.
* `/delay`: (초) 별칭 URL 서비스 호출 사이의 시간입니다.

>[!NOTE]
>
>If your render is an instance of AEM you must install the [VanityURLS-Components](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq600/component/vanityurls-components) package to install the vanity URL service. (See [Signing In to Package Share](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/package-manager.html#SigningIntoPackageShare).)

다음 절차를 사용하여 별칭 URL에 대한 액세스를 활성화합니다.

1. 렌더링 서비스가 AEM 인스턴스인 경우 게시 인스턴스에 com. adobe. granite. dispatcher. vanityurl. content 패키지를 설치합니다 (위 참고 참조).
1. For each vanity URL that you have configured for an AEM or CQ page, ensure that the ` [/filter](dispatcher-configuration.md#main-pars_134_32_0009)` configuration denies the URL. 필요한 경우 URL를 거부하는 필터를 추가합니다.
1. Add the `/vanity_urls` section below `/farms`.
1. Apache 웹 서버를 다시 시작합니다.

## Forwarding Syndication Requests - /propagateSyndPost {#forwarding-syndication-requests-propagatesyndpost}

신디케이션 요청은 일반적으로 디스패처를 위한 것이므로 기본적으로 렌더러로 전송되지 않습니다 (예: AEM 인스턴스).

필요한 경우 /propagateSyndPost 속성을 &quot;1&quot; 로 설정하여 신디케이션 요청을 Dispatcher로 전달합니다. 설정된 경우 필터 섹션에서 게시물 요청이 거부되지 않도록 해야 합니다.

## Configuring the Dispatcher Cache - /cache {#configuring-the-dispatcher-cache-cache}

`/cache` 이 섹션에서는 Dispatcher에서 문서를 캐시하는 방법을 제어합니다. 캐싱 전략을 구현하도록 여러 하위 속성을 구성합니다.


* /docroot
* /statfile
* /serveStaleOnError
* /allowAuthorized
* /규칙
* /statfileslevel
* /invalidate
* /invalidateHandler
* /allowedClients
* /ignoreUrlParams
* /headers
* /mode
* /gracePeriod


캐시 예 섹션은 다음과 같이 표시될 수 있습니다.

```xml
/cache
  {
  /docroot "/opt/dispatcher/cache"
  /statfile  "/tmp/dispatcher-website.stat"          
  /allowAuthorized "0"
      
  /rules
    {
    # List of files that are cached
    }

  /invalidate
    {
    # List of files that are auto-invalidated
    }
  }
  
```

>[!NOTE]
>
>For permission-sensitive caching, read [Caching Secured Content](permissions-cache.md).

### Specifying the Cache Directory {#specifying-the-cache-directory}

`/docroot` 속성은 캐시된 파일이 저장되는 디렉토리를 식별합니다.

>[!NOTE]
>
>디스패처와 웹 서버가 같은 파일을 처리하도록 이 값이 웹 서버의 문서 루트와 정확히 같은 경로여야 합니다.\
>웹 서버는 Dispatcher 캐시 파일을 사용할 때 올바른 상태 코드를 전달해야 합니다. 따라서 이 파일을 찾는 것도 중요합니다.

여러 팜을 사용하는 경우 각 팜은 다른 문서 루트를 사용해야 합니다.

### Naming the Statfile {#naming-the-statfile}

`/statfile` 속성은 statfile로 사용할 파일을 식별합니다. Dispatcher는 이 파일을 사용하여 가장 최근의 컨텐츠 업데이트 시간을 등록합니다. statfile는 웹 서버에 있는 모든 파일일 수 있습니다.

statfile에 컨텐츠가 없습니다. 컨텐츠가 업데이트되면 디스패처가 타임스탬프를 업데이트합니다. 기본 statfile 파일의 이름은. stat 이고 docroot에 저장됩니다. Dispatcher는 Statfile에 대한 액세스를 차단합니다.

>[!NOTE]
>
>If `/statfileslevel` is configured, Dispatcher ignores the `/statfile` property and uses .stat as the name.

### Serving Stale Documents When Errors Occur {#serving-stale-documents-when-errors-occur}

`/serveStaleOnError` 이 속성은 렌더링 서버에서 오류를 반환할 때 디스패처가 무효화된 문서를 반환하는지 여부를 제어합니다. 기본적으로, Statfile를 터치하고 캐시된 컨텐츠를 무효화할 때, 디스패처는 다음에 캐시된 컨텐츠를 요청 시 삭제합니다.

If `/serveStaleOnError` is set to &quot;1&quot;, Dispatcher does not delete invalidated content from the cache unless the render server returns a successful response. AEM 또는 연결 시간 초과의 5 xx 응답이 디스패처를 통해 오래된 컨텐츠를 제공하고 HTTP 상태 111 (재유효성 검사 실패) 로 응답합니다.

### Caching When Authentication is Used {#caching-when-authentication-is-used}

`/allowAuthorized` 속성은 다음 인증 정보를 포함하는 요청이 캐시되는지 여부를 제어합니다.

* `authorization` 헤더입니다.
* `authorization`이름이 지정된 쿠키입니다.
* `login-token`이름이 지정된 쿠키입니다.

캐시된 문서를 클라이언트로 반환할 때 인증이 수행되지 않으므로 이 인증 정보를 포함하는 요청은 캐시되지 않습니다. 이러한 구성은 디스패처가 캐시된 문서를 필요한 권한이 없는 사용자에게 제공하는 것을 방지합니다.

그러나 요구 사항으로 인해 인증된 문서의 캐싱을 허용하는 경우 /allowAuthorized로 설정하십시오.

`/allowAuthorized "1"`

>[!NOTE]
>
>`/sessionmanagement` 속성을 사용하여 세션 관리를 활성화하려면 `/allowAuthorized` 속성을로 설정해야 `"0"`합니다.

### Specifying the Documents to Cache {#specifying-the-documents-to-cache}

The `/rules` property controls which documents are cached according to the document path. /rules 속성에 관계없이 Dispatcher는 다음 상황에서 문서를 캐시하지 않습니다.

* 요청 URI에 물음표 (&quot;? &quot;).\
   이것은 일반적으로 검색 결과 (캐시할 필요가 없는 검색 결과) 를 나타냅니다.
* 파일 확장자가 없습니다.\
   웹 서버는 확장자가 있어야 문서 유형 (MIME-type) 를 확인할 수 있습니다.
* 인증 헤더가 설정됨 (구성 가능)
* AEM 인스턴스가 다음 헤더로 응답하는 경우:

   * `no-cache`
   * `no-store`
   * `must-revalidate`

>[!NOTE]
>
>GET 또는 HEAD (HTTP 헤더 경우) 메서드는 디스패처가 캐시할 수 있습니다. For additional information on response header caching, see the [Caching HTTP Response Headers](dispatcher-configuration.md#caching-http-response-headers) section.

Each item in the /rules property includes a [glob](#designing-patterns-for-glob-properties) pattern and a type:

* GLOB 패턴은 문서의 경로와 일치하는 데 사용됩니다.
* 유형은 GLOB 패턴과 일치하는 문서를 캐싱할지 여부를 나타냅니다. 이 값은 문서를 캐시하기 위해 허용하거나 (문서를 항상 렌더링하려는 경우) 거부할 수 있습니다.

동적 페이지가 없는 경우 (위 규칙에서 이미 제외된 것 이외에) 모든 것이 캐시되도록 Dispatcher를 구성할 수 있습니다. 이에 대한 규칙 섹션은 다음과 같습니다.

```xml
/rules
  { 
    /0000  {  /glob "*"   /type "allow" }
  }
```

For information about glob properties, see [Designing Patterns for glob Properties](#designing-patterns-for-glob-properties).

페이지의 일부 섹션 (예: 뉴스 애플리케이션) 이나 닫힌 사용자 그룹이 있는 경우 예외를 정의할 수 있습니다.

>[!NOTE]
>
>캐시된 페이지에 대해 사용자 권한이 확인되지 않으므로 닫힌 사용자 그룹을 캐시할 수 없습니다.

```xml
/rules
  {
   /0000  { /glob "*" /type "allow" }
   /0001  { /glob "/en/news/*" /type "deny" }
   /0002  { /glob "*/private/*" /type "deny"  }   
  }
```

**압축**

Apache 웹 서버에서는 캐시된 문서를 압축할 수 있습니다. 압축을 통해 Apache는 클라이언트가 요청한 경우 압축된 양식으로 문서를 반환할 수 있습니다. Compression is done automatically by enabling the Apache module `mod_deflate`, for example:

```xml
AddOutputFilterByType DEFLATE text/plain
```

모듈은 Apache 2. x와 함께 기본적으로 설치됩니다.

<!-- 

Comment Type: draft

<note type="note"> 
 <p>Depending on the Apache web server version you can enable <span class="code">gzip</span> compression as follows:</p> 
 <ul> 
  <li>For Apache 1.3 you can enable the <span class="code">mod_gzip </span>module. The module must be downloaded and installed.</li> 
  <li>For Apache 2.x you can enable the <span class="code">mod_deflate</span> module. The module is installed by default with Apache 2.x.<br /> </li> 
 </ul> 
 <p> </p> 
</note>

 -->

<!-- 

Comment Type: draft

<p>The following rule caches all documents in compressed form; Apache can return either the uncompressed or the compressed form to the client:</p>

 -->

<!-- 

Comment Type: draft

<codeblock gutter="true" class="syntax xml">
  /rules!!discoiqbr!!&nbsp;&nbsp;{!!discoiqbr!!&nbsp;&nbsp;&nbsp;/rulelabel&nbsp;&nbsp;{&nbsp;&nbsp;/glob&nbsp;"*"&nbsp;/type&nbsp;"allow"&nbsp;&nbsp;/compress&nbsp;"gzip"&nbsp;}!!discoiqbr!!&nbsp;&nbsp;} 
</codeblock>

 -->

<!-- 

Comment Type: remark
Last Modified By: Silviu Raiman (raiman)
Last Modified Date: 2017-11-13T09:23:24.326-0500

<p>Hidden the <span class="code">mod_gzip</span> content as requested in CQDOC-11124.</p>

 -->

### Invalidating Files by Folder Level {#invalidating-files-by-folder-level}

`/statfileslevel` 이 속성을 사용하여 캐시된 파일을 경로에 따라 무효화할 수 있습니다.

* Dispatcher creates `.stat`files in each folder from the docroot folder to the level that you specify. Docroot 폴더는 Level 0 입니다.
* Files are invalidated by touching the `.stat` file. `.stat` 파일의 마지막 수정 날짜는 캐시된 문서의 마지막 수정 날짜와 비교됩니다. The document is re-fetched if the `.stat` file is newer.

* When a file located at a certain level is invalidated then **all** `.stat` files from the docroot **to** the level of the invalidated file or the configured `statsfilevel` (whichever is smaller) will be touched.

   * For example: if you set the `statfileslevel` property to 6 and a file is invalidated at level 5 then every `.stat` file from docroot to 5 will be touched. 이 예제를 계속 진행하면, 파일이 수준 7에서 무효화됩니다. `stat` Docroot에서 6로 파일을 터치합니다 (이후) `/statfileslevel = "6"`.

무효화되지 않은 파일에 대한 경로** 를 따라가는 리소스만 영향을 받습니다. Consider the following example: a website uses the structure `/content/myWebsite/xx/.` If you set `statfileslevel` as 3, a `.stat`file is created as follows:

* `docroot`
* `/content`
* `/content/myWebsite`
* `/content/myWebsite/*xx*`

When a file in `/content/myWebsite/xx` is invalidated then every `.stat` file from docroot down to `/content/myWebsite/xx`is touched. This would be the case only for `/content/myWebsite/xx` and not for example `/content/myWebsite/yy` or `/content/anotherWebSite`.

>[!NOTE]
>
>Invalidation can be prevented by sending an additional Header `CQ-Action-Scope:ResourceOnly`. 이 기능을 사용하면 캐시의 다른 부분을 무효화하지 않고도 특정 리소스를 플러시할 수 있습니다. See [this page](https://adobe-consulting-services.github.io/acs-aem-commons/features/dispatcher-flush-rules/index.html) and [Manually Invalidating the Dispatcher Cache](https://helpx.adobe.com/experience-manager/dispatcher/using/page-invalidate.html) for additional details.

>[!NOTE]
>
>`/statfileslevel` 속성 값을 지정하면 `/statfile` 속성이 무시됩니다.

### Automatically Invalidating Cached Files {#automatically-invalidating-cached-files}

`/invalidate` 속성은 컨텐츠가 업데이트될 때 자동으로 무효화되는 문서를 정의합니다.

자동 무효화를 통해 디스패처는 컨텐츠 업데이트 후 캐시된 파일을 삭제하지 않지만 다음에 요청할 때 유효성을 확인합니다. 캐시에 자동 무효화되지 않은 문서는 컨텐츠 업데이트가 명시적으로 삭제될 때까지 캐시에 유지됩니다.

자동 무효화는 일반적으로 HTML 페이지에 사용됩니다. HTML 페이지에는 다른 페이지에 대한 링크가 포함되어 있어서 컨텐츠 업데이트가 페이지에 영향을 미치는지 여부를 확인하는 것은 어렵습니다. 컨텐츠가 업데이트될 때 모든 관련 페이지가 무효화되지 않도록 모든 HTML 페이지를 자동으로 무효화합니다. 다음 구성은 모든 HTML 페이지를 무효화합니다.

```xml
  /invalidate
  {
   /0000  { /glob "*" /type "deny" }
   /0001  { /glob "*.html" /type "allow" }
  }
```

For information about glob properties, see [Designing Patterns for glob Properties](#designing-patterns-for-glob-properties).

이 구성은 /content/geometrixx/en 활성화 시 다음 활동을 초래합니다.

* 패턴 en 이 있는 모든 파일.*는 /content/geometrixx/ 폴더에서 제거됩니다.
* /content/geometrixx/en/_jcr_content 폴더가 제거됩니다.
* /invalidate 구성과 일치하는 다른 모든 파일은 즉시 삭제되지 않습니다. 이러한 파일은 다음 요청이 발생하면 삭제됩니다. Example /content/geometrixx.html is not deleted, it will be deleted when /content/geometrixx.html is requested.

자동으로 생성된 PDF 및 ZIP 파일을 다운로드하는 경우 이러한 파일도 자동으로 무효화해야 할 수 있습니다. 구성 예는 다음과 같습니다.

```xml
/invalidate
  {
   /0000 { /glob "*" /type "deny" }
   /0001 { /glob "*.html" /type "allow" }
   /0002 { /glob "*.zip" /type "allow" }
   /0003 { /glob "*.pdf" /type "allow" }
  }
```

Adobe Analytics와 AEM 통합은 웹 사이트의 analytics. sitecatalyst. js 파일에서 구성 데이터를 전달합니다. Dispatcher 예제. Dispatcher와 함께 제공되는 모든 파일에는 이 파일에 대한 다음과 같은 무효화 규칙이 포함됩니다.

```xml
{
   /glob "*/analytics.sitecatalyst.js"  /type "allow"
}
```

### Using custom invalidation scripts {#using-custom-invalidation-scripts}

/invalidateHandler 속성을 사용하면 Dispatcher가 받은 각 무효화 요청에 대해 호출되는 스크립트를 정의할 수 있습니다.

다음 인수를 사용하여 호출됩니다.

* 핸들\
   무효화되는 컨텐츠 경로
* 작업\
   복제 작업 (예: 활성화, 비활성화)
* 작업 범위\
   The replication Action&#39;s Scope (empty, unless a header of `CQ-Action-Scope: ResourceOnly` is sent, see [Invalidating Cached Pages from AEM](page-invalidate.md) for details)

다른 애플리케이션별 캐시를 무효화하거나, 페이지의 외부화된 URL 및 docroot의 해당 위치가 컨텐츠 경로와 일치하지 않는 경우를 처리하기 위해 사용할 수 있습니다.

아래의 스크립트 예제에서는 각 무효화 요청을 파일에 기록합니다.

```xml
/invalidateHandler "/opt/dispatcher/scripts/invalidate.sh"
```

#### sample invalidation handler script {#sample-invalidation-handler-script}

```shell
#!/bin/bash

printf "%-15s: %s %s" $1 $2 $3>> /opt/dispatcher/logs/invalidate.log
```

### Limiting the Clients That Can Flush the Cache {#limiting-the-clients-that-can-flush-the-cache}

/allowedClients 속성은 캐시를 플러시할 수 있는 특정 클라이언트를 정의합니다. Globbing 패턴은 IP와 일치합니다.

다음 예를 참조하십시오.

1. 모든 클라이언트에 대한 액세스를 거부합니다.
1. localhost에 명시적으로 액세스 허용

```xml
/allowedClients
  {
   /0001 { /glob "*.*.*.*"  /type "deny" }
   /0002 { /glob "127.0.0.1" /type "allow" }
  }
```

For information about glob properties, see [Designing Patterns for glob Properties](#designing-patterns-for-glob-properties).

>[!CAUTION]
>
>/allowedClients를 정의하는 것이 좋습니다.
>
>이 작업이 수행되지 않으면 모든 클라이언트에서 캐시를 지우도록 호출을 실행할 수 있습니다. 이 작업이 반복적으로 수행되면 사이트 성능에 심각한 영향을 줄 수 있습니다.

### Ignoring URL Parameters {#ignoring-url-parameters}

`ignoreUrlParams` 이 섹션에서는 캐시에서 페이지를 캐시할지 또는 배달할지 결정할 때 무시되는 URL 매개 변수를 정의합니다.

* 요청 URL에 모두 무시되는 매개 변수가 들어 있으면 페이지가 캐시됩니다.
* 요청 URL에 무시되지 않는 매개 변수가 하나 이상 들어 있으면 페이지가 캐시되지 않습니다.

페이지에 대해 매개 변수가 무시되면 페이지가 처음 요청될 때 페이지가 캐시됩니다. 요청에 있는 매개 변수의 값과 관계없이, 이후 페이지 요청은 캐시된 페이지에 제공됩니다.

To specify which parameters are ignored, add glob rules to the `ignoreUrlParams` property:

* 매개 변수를 무시하려면 매개 변수를 허용하는 glob 속성을 만듭니다.
* 페이지가 캐시되지 않도록 하려면 매개 변수를 거부하는 glob 속성을 만드십시오.

다음 예제에서는 디스패처가 &quot;q&quot; 매개 변수를 무시하고 q 매개 변수를 포함하는 요청 URL 이 캐시되도록 합니다.

```xml
/ignoreUrlParams
{
    /0001 { /glob "*" /type "deny" }
    /0002 { /glob "q" /type "allow" }
}
```

Using the example `ignoreUrlParams` value, the following HTTP request causes the page to be cached because the `q` parameter is ignored:

```xml
GET /mypage.html?q=5
```

Using the example `ignoreUrlParams` value, the following HTTP request causes the page to **not** be cached because the `p` parameter is not ignored:

```xml
GET /mypage.html?q=5&p=4
```

For information about glob properties, see [Designing Patterns for glob Properties](#designing-patterns-for-glob-properties).

### Caching HTTP Response Headers {#caching-http-response-headers}

>[!NOTE]
>
>This feature is avaiable with version **4.1.11** of the Dispatcher.

`/headers` 이 속성을 사용하면 디스패처가 캐시할 HTTP 헤더 유형을 정의할 수 있습니다. 캐시되지 않은 리소스에 대한 첫 번째 요청에서 구성된 값 중 하나와 일치하는 모든 헤더 (아래의 구성 샘플 참조) 는 캐시 파일 옆에 별도의 파일로 저장됩니다. 이후 요청 시 캐시된 리소스가 응답에 추가됩니다.

다음은 기본 구성의 샘플입니다.

```xml
/cache {
  ...
  /headers {
    "Cache-Control"
    "Content-Disposition"
    "Content-Type"
    "Expires"
    "Last-Modified"
    "X-Content-Type-Options"
    "Last-Modified"
  }
}
```

>[!NOTE]
>
>또한 파일 Globbing 문자는 허용되지 않는다는 점을 참고하십시오. For more details, see [Designing Patterns for glob Properties](#designing-patterns-for-glob-properties).

>[!NOTE]
>
>AEM에서 ETAG 응답 헤더를 저장하고 배달하려면 Dispatcher가 필요한 경우 다음을 수행하십시오.
>
>* Add the header name in the `/cache/headers`section.
>* Add the following [Apache directive](https://httpd.apache.org/docs/2.4/mod/core.html#fileetag) in the Dispatcher related section:
>



```xml
FileETag none
```

### Dispatcher Cache File Permissions {#dispatcher-cache-file-permissions}

`mode` 속성은 캐시의 새 디렉토리 및 파일에 적용되는 파일 권한을 지정합니다. This setting is restricted by the `umask` of the calling process. 다음 값 중 하나 이상의 합으로 구성된 8 진수입니다.

* 0400 소유자별로 읽기를 허용합니다.
* 0200 소유자별로 쓰기 허용.
* 0100 소유자는 디렉토리에서 검색할 수 있습니다.
* 0040 그룹 구성원이 읽을 수 있습니다.
* 0020 그룹 구성원이 쓰기를 허용합니다.
* 0010 그룹 구성원이 디렉토리에서 검색할 수 있도록 허용합니다.
* 0004 다른 사람이 읽을 수 있도록 허용합니다.
* 0002 다른 사람의 쓰기를 허용합니다.
* 0001 다른 사람이 디렉토리에서 검색할 수 있도록 허용합니다.

기본값은 0755 로, 소유자가 읽기, 쓰기 또는 검색을 수행할 수 있고 그룹 및 다른 사람이 읽거나 검색할 수 있습니다.

### Throttling .stat file touching {#throttling-stat-file-touching}

With the default `/invalidate` property, every activation effectively invalidates all `.html` files (when their path matches the `/invalidate` section). 트래픽이 많은 웹 사이트에서 여러 번 활성화하면 CPU 로드가 백엔드에 증가합니다. In such a scenario, it would be desirable to &quot;throttle&quot; `.stat` file touching to keep the website responsive. You can do this by using the `/gracePeriod` property.

`/gracePeriod` 속성은 마지막 활성화 이후 캐시에서 자동 무효화되지 않은 리소스가 계속 제공될 수 있는 시간 (초) 를 정의합니다. 이 속성은 활성화 일괄 처리가 전체 캐시를 반복적으로 무효화하는 설정에서 사용할 수 있습니다. 권장 값은 2 초입니다.

For additional details, also read the `/invalidate` and `/statfileslevel`sections above.

## Configuring Time Based Cache Invalidation - /enableTTL {#configuring-time-based-cache-invalidation-enablettl}

If set, the `enableTTL` property will evaluate the response headers from the backend, and if they contain a `Cache-Control` max-age or `Expires` date, an auxiliary, empty file next to the cache file is created, with modification time equal to the expiry date. 캐시된 파일이 수정 시간을 지나 요청된 경우 백엔드에서 자동으로 다시 요청됩니다.

You can enable the feature by adding this line to the `dispatcher.any` file:

```xml
/enableTTL "1"
```

>[!NOTE]
>
>This feature is avaiable with version **4.1.11** of the Dispatcher.

## Configuring Load Balancing - /statistics {#configuring-load-balancing-statistics}

The `/statistics` section defines categories of files for which Dispatcher scores the responsiveness of each render. Dispatcher는 점수를 사용하여 요청을 보낼 렌더링을 결정합니다.

제작한 각 카테고리는 GLOB 패턴을 정의합니다. Dispatcher는 요청된 컨텐츠의 URI를 이러한 패턴에 비교하여 요청된 컨텐츠의 카테고리를 결정합니다.

* 카테고리 순서는 URI와 비교할 순서를 결정합니다.
* URI와 일치하는 첫 번째 카테고리 패턴은 파일의 범주입니다. 더 이상 카테고리 패턴이 평가되지 않습니다.

Dispatcher는 최대 8 개의 통계 카테고리를 지원합니다. 8 개 이상의 카테고리를 정의하는 경우 처음 8 개만 사용됩니다.

**선택 항목 렌더링**

디스패처가 렌더링된 페이지를 필요로 할 때마다 다음 알고리즘을 사용하여 렌더링을 선택합니다.

1. If the request contains the render name in a `renderid` cookie, Dispatcher uses that render.
1. If the request includes no `renderid` cookie, Dispatcher compares the render statistics:

   1. Dispatcher는 요청 URI의 cateogry를 결정합니다.
   1. Dispatcher는 해당 카테고리에 대해 가장 낮은 응답 점수가 있는 렌더링을 결정하고 해당 렌더링을 선택합니다.

1. 아직 렌더링이 선택되지 않은 경우 목록의 첫 번째 렌더링을 사용합니다.

렌더링 카테고리에 대한 점수는 이전 응답 시간을 기반으로 하며, 이전에 실패하고 디스패처가 시도한 성공적인 연결을 기반으로 합니다. 각 시도에 대해 요청된 URI의 카테고리에 대한 점수가 업데이트됩니다.

>[!NOTE]
>
>로드 밸런싱을 사용하지 않으면 이 섹션을 생략할 수 있습니다.

### Defining Statistics Categories {#defining-statistics-categories}

렌더링 선택에 대한 통계를 유지할 각 문서 유형에 대한 범주를 정의합니다. /statistics 섹션에는 /categories 섹션이 포함되어 있습니다. 카테고리를 정의하려면 다음 형식을 갖는 /categories 섹션 아래에 라인을 추가합니다.

`/name { /glob "pattern"}`

The category `name` must be unique to the farm. The `pattern` is described in the [Designing Patterns for glob Properties](#designing-patterns-for-glob-properties) section.

URI의 카테고리를 결정하기 위해 Dispatcher는 일치가 발견될 때까지 URI를 각 카테고리 패턴과 비교합니다. Dispatcher는 목록의 첫 번째 카테고리와 순서대로 cointinues로 시작합니다. 따라서 보다 구체적인 패턴을 먼저 사용하여 카테고리를 배치하십시오.

예를 들어 Dispatcher는 기본 Dispatcher 입니다. 모든 파일은 HTML 카테고리와 다른 카테고리를 정의합니다. HTML 카테고리는 더 구체적이므로 먼저 나타납니다.

```xml
/statistics
  {
  /categories
    {
      /html { /glob "*.html" }
      /others  { /glob "*" }
    }
  }
```

다음 예에는 검색 페이지에 대한 카테고리가 포함되어 있습니다.

```xml
/statistics
  {
  /categories
    {
      /search { /glob "*search.html" }
      /html { /glob "*.html" }
      /others  { /glob "*" }
    }
  }
```

### Reflecting Server Unavailability in Dispatcher Statistics {#reflecting-server-unavailability-in-dispatcher-statistics}

`/unavailablePenalty` 이 속성은 렌더링에 대한 연결이 실패할 때 렌더링 통계에 적용되는 시간 (1/10 초 초) 를 설정합니다. Dispatcher는 요청된 URI와 일치하는 통계 카테고리에 시간을 추가합니다.

예를 들어, AEM 이 실행 (및 의견 수렴 아님) 되거나 네트워크 관련 문제로 인해 지정된 호스트 이름/포트에 대한 TCP/IP 연결이 설정될 수 없을 때 위약금이 적용됩니다.

`/unavailablePenalty` 속성은 `/farm` 섹션의 직접 자식입니다 ( `/statistics` 섹션의 형제).

If no `/unavailablePenalty` property exists, a value of &quot;1&quot; is used.

```xml
/unavailablePenalty "1"
```

## Identifying a Sticky Connection Folder - /stickyConnectionsFor {#identifying-a-sticky-connection-folder-stickyconnectionsfor}

`/stickyConnectionsFor` 이 속성은 스티커 문서가 포함된 하나의 폴더를 정의합니다. URL를 사용하여 액세스합니다. Dispatcher는 이 폴더에 있는 단일 사용자의 모든 요청을 동일한 Render 인스턴스로 전송합니다. 고정 연결은 세션 데이터가 모든 문서에 대해 존재하고 일관되게 유지되도록 합니다. This mechanism uses the `renderid` cookie.

다음 예제에서는 /products 폴더에 대한 고정 연결을 정의합니다.

```xml
/stickyConnectionsFor "/products"
```

When a page is composed of content from several content nodes, include the `/paths` property that lists the paths to the content. For example, a page contains content from `/content/image`, `/content/video`, and `/var/files/pdfs`. 다음 구성은 페이지의 모든 컨텐츠에 대한 일관된 연결을 가능하게 합니다.

```xml
/stickyConnections {
  /paths {
    "/content/image"
    "/content/video"
    "/var/files/pdfs"
  }
}
```

### httpOnly {#httponly}

When sticky connections are enabled, the dispatcher module sets the `renderid` cookie. This cookie doesn&#39;t have the `httponly` flag, which should be added in order to enhance security. You can do this by setting the `httpOnly` property in the `/stickyConnections` node of a `dispatcher.any` configuration file. The property&#39;s value (either 0 or 1) defines whether the `renderid` cookie has the `HttpOnly` attribute appended. 기본값은 0 이므로 속성이 추가되지 않음을 의미합니다.

For additional information about the `httponly` flag, read [this page](https://www.owasp.org/index.php/HttpOnly).

### secure {#secure}

When sticky connections are enabled, the dispatcher module sets the `renderid` cookie. This cookie doesn&#39;t have the **secure** flag, which should be added in order to enhance security. You can do this by setting the `secure` property in the `/stickyConnections` node of a `dispatcher.any` configuration file. The property&#39;s value (either 0 or 1) defines whether the `renderid` cookie has the `secure` attribute appended. 기본값은 0 입니다. 즉, 수신 요청이 안전하다면 속성이 추가됩니다. * * 이 값을 1로 설정하면 수신 요청이 안전한지 여부에 관계없이 보안 플래그가 추가됩니다.

## Handling Render Connection Errors {#handling-render-connection-errors}

렌더링 서버에서 500 오류를 반환하거나 사용할 수 없는 경우 Dispatcher 동작을 구성합니다.

### Specifying a Health Check Page {#specifying-a-health-check-page}

`/health_check` 속성을 사용하여 500 상태 코드가 발생할 때 확인된 URL를 지정합니다. If this page also returns a 500 status code the instance is considered to be unavailable and a configurable time penalty ( `/unavailablePenalty`) is applied to the render before retrying.

```xml
/health_check
  {
  # Page gets contacted when an instance returns a 500
  /url "/health_check.html"
  }
```

### Specifying the Page Retry Delay {#specifying-the-page-retry-delay}

The / `retryDelay` property sets the time (in seconds) that Dispatcher waits between rounds of connection attempts with the farm renders. 각 반올림에 대해 디스패처가 렌더링에 연결을 시도하는 최대 횟수는 팜의 렌더링 수입니다.

Dispatcher uses a value of `"1"` if `/retryDelay` is not explicitly defined. 대부분의 경우 기본값이 적절합니다.

```xml
/retryDelay "1"
```

### Configuring the Number of Retries {#configuring-the-number-of-retries}

`/numberOfRetries` 이 속성은 디스패처가 렌더링으로 수행하는 최대 연결 시도 횟수를 설정합니다. 이 재시도 횟수가 지난 후 Dispatcher에서 렌더링에 성공적으로 연결할 수 없는 경우 디스패처가 실패한 응답을 반환합니다.

각 반올림에 대해 디스패처가 렌더링에 연결을 시도하는 최대 횟수는 팜의 렌더링 수입니다. Therefore, the maximum number of times that Dispatcher attempts a connection is ( `/numberOfRetries`) x (the number of renders).

If the value is not explicitly defined, the default value is **5**.

```xml
/numberOfRetries "5"
```

### Using the Failover Mechanism {#using-the-failover-mechanism}

원본 요청에 실패하면 Dispatcher 팜에서 장애 조치 메커니즘을 활성화하여 요청을 다른 렌더링에 다시 보낼 수 있습니다. 장애 조치가 활성화되면 디스패처가 다음과 같은 비헤이비어를 갖게 됩니다.

* 렌더링에 대한 요청이 HTTP 상태 503 (사용 불가능) 를 반환하면 디스패처가 요청을 다른 렌더링으로 보냅니다.
* When a request to a render returns HTTP status 50x (other than 503), Dispatcher sends a request for the page that is configured for the `health_check` property.

   * Health Check가 500 (internal_ server_ error) 를 반환하는 경우 Dispatcher는 원래 요청을 다른 렌더링으로 보냅니다.
   * Healtch 검사에서 HTTP 상태 200를 반환하면 디스패처가 클라이언트에 초기 HTTP 500 오류를 반환합니다.

장애 조치를 활성화하려면 농장 (또는 웹 사이트) 에 다음 줄을 추가합니다.

```xml
/failover "1" 
```

>[!NOTE]
>
>To retry HTTP requests that contain a body, Dispatcher sends a `Expect: 100-continue` request header to the render before spooling the actual contents. CQSE와 CQ 5.5의 경우 100 (계속) 또는 오류 코드가 포함된 즉시 답변을 얻을 수 있습니다. 다른 서블릿 컨테이너도 이 기능을 지원해야 합니다.

## Ignoring Interruption Errors - /ignoreEINTR {#ignoring-interruption-errors-ignoreeintr}

>[!CAUTION]
>
>이 옵션은 일반적으로 필요하지 않습니다. 다음과 같은 로그 메시지가 표시될 때만 사용해야 합니다.
>
>`Error while reading response: Interrupted system call`

Any file system oriented system call can be interrupted `EINTR` if the object of the system call is located on a remote system accessed via NFS. 이러한 시스템 호출이 시간 초과되거나 중단될 수 있는지 여부는 기본 파일 시스템이 로컬 시스템에 마운트된 방식을 기반으로 합니다.

인스턴스에 이러한 구성이 있고 로그에 다음 메시지가 포함된 경우 /ignoreEINTR 매개 변수를 사용합니다.

`Error while reading response: Interrupted system call`

내부적으로 Dispatcher는 다음과 같이 표현할 수 있는 루프를 사용하여 원격 서버 (예: AEM) 의 응답을 읽습니다.

`while (response not finished) {  
read more data  
}`

Such messages can be generated when the `EINTR` occurs in the &quot; `read more data`&quot; section and are caused by the reception of a signal before any data was received.

To ignore such interrupts you can add the following parameter to `dispatcher.any` (before `/farms`):

`/ignoreEINTR "1"`

Setting `/ignoreEINTR` to `"1"` causes Dispatcher to continue to attempt to read data until the complete response is read. 기본값은 0 이고 옵션을 비활성화합니다.

## Designing Patterns for glob Properties {#designing-patterns-for-glob-properties}

Several sections in the Dispatcher configuration file use `glob` properties as selection criteria for client requests. GLOB 속성 값은 요청된 리소스의 경로 또는 클라이언트의 IP 주소와 같이, 디스패처가 요청의 측면과 비교할 패턴입니다. For example, the items in the `/filter` section use glob patterns to identify the paths of the pages that Dispatcher acts on or rejects.

GLOB 값에는 패턴을 정의하는 와일드카드 문자와 영숫자 문자가 포함될 수 있습니다.

| 와일드카드 문자 | 설명 | examples |
|--- |--- |--- |
| `*` | 문자열에 있는 문자의 연속된 인스턴스 0 개 이상을 일치합니다. The final character of the match is determined by either of the following situations: <br/>A character in the string matches the next character in the pattern, and the pattern character has the following characteristics:<br/><ul><li>Not a *</li><li>A?</li><li>문자 그대로의 문자 (공백 포함) 또는 문자 클래스입니다.</li><li>패턴의 끝에 도달했습니다.</li></ul>문자 클래스 내에서 문자는 글자 그대로 해석됩니다. | `*/geo*``/content/geometrixx` 노드 및 `/content/geometrixx-outdoors` 노드 아래의 모든 페이지에 일치합니다. The following HTTP requests match the glob pattern: <br/><ul><li>`"GET /content/geometrixx/en.html"`</li><li>`"GET /content/geometrixx-outdoors/en.html"` </li></ul><br/> `*outdoors/*`<br/>노드 아래의 모든 페이지에 `/content/geometrixx-outdoors` 일치합니다. For example, the following HTTP request matches the glob pattern: <br/><ul><li>`"GET /content/geometrixx-outdoors/en.html"`</li></ul> |
| `?` | 단일 문자와 일치합니다. 외부 문자 클래스를 사용합니다. 문자 클래스 내에서 이 문자는 글자 그대로 해석됩니다. | `*outdoors/??/*`<br/> Geometrixx Outdoors 사이트의 모든 언어에 대한 페이지와 일치합니다. For example, the following HTTP request matches the glob pattern: <br/><ul><li>`"GET /content/geometrixx-outdoors/en/men.html"`</li></ul><br/>다음 요청이 GLOB 패턴과 일치하지 않습니다. <br/><ul><li>&quot; GET /content/geometrixx-outdoors/en.html &quot;</li></ul> |
| `[ and ]` | 문자 클래스의 시작 부분과 끝 부분을 지정합니다. 문자 클래스에는 하나 이상의 문자 범위와 단일 문자가 포함될 수 있습니다.<br/>대상 문자가 문자 클래스의 문자와 일치하는 경우 또는 정의된 범위 내에서 일치하는 경우 일치합니다.<br/>닫는 대괄호가 포함되지 않으면 패턴은 일치하지 않습니다. | `*[o]men.html*`<br/> 다음 HTTP 요청과 일치합니다.<br/><ul><li>`"GET /content/geometrixx-outdoors/en/women.html"`</li></ul><br/>다음 HTTP 요청과 일치하지 않음:<br/><ul><li>`"GET /content/geometrixx-outdoors/en/men.html"`</li></ul><br/> `*[o/]men.html*`<br/>다음 HTTP 요청과 일치합니다. <br/><ul><li>`"GET /content/geometrixx-outdoors/en/women.html"`</li><li>`"GET /content/geometrixx-outdoors/en/men.html"`</li></ul> |
| `-` | 문자 범위를 나타냅니다. for use in character classes. 문자 클래스의 외부에서 이 문자는 글자 그대로 해석됩니다. | `*[m-p]men.html*` 다음 HTTP 요청과 일치합니다. <br/><ul><li>`"GET /content/geometrixx-outdoors/en/women.html"`</li></ul>Does not match the following HTTP request:<br/><ul><li>`"GET /content/geometrixx-outdoors/en/men.html"`</li></ul> |
| `!` | 다음에 나오는 문자 또는 문자 클래스를 무효화합니다. 문자 클래스 내의 문자 및 문자 범위를 무효화하는 경우에만 사용할 수 있습니다. Equivalent to the `^ wildcard`. <br/>문자 클래스의 외부에서 이 문자는 글자 그대로 해석됩니다. | `*[!o]men.html*`<br/> 다음 HTTP 요청과 일치합니다. <br/><ul><li>`"GET /content/geometrixx-outdoors/en/men.html"`</li></ul><br/>다음 HTTP 요청과 일치하지 않음: <br/><ul><li>`"GET /content/geometrixx-outdoors/en/women.html"`</li></ul><br/>`*[!o!/]men.html*`<br/> 다음 HTTP 요청과 일치하지 않음:<br/><ul><li>`"GET /content/geometrixx-outdoors/en/women.html"` 또는 `"GET /content/geometrixx-outdoors/en/men. html"`</li></ul> |
| `^` | 다음에 나오는 문자 또는 문자 범위를 무효화합니다. 문자 클래스 내의 문자와 문자 범위만 무효화하는 데 사용됩니다. `!` 와일드카드 문자와 같습니다. <br/>문자 클래스의 외부에서 이 문자는 글자 그대로 해석됩니다. | `!` 와일드카드 문자의 예가 적용되고 example 패턴의 `!` 문자를 `^` 문자로 대체합니다. |


<!--- need to troubleshoot table

The following table describes the wildcard characters.

<table border="1" cellpadding="1" cellspacing="0" width="100%"> 
 <tbody> 
  <tr> 
   <th>Wildcard character</th> 
   <th>Description</th> 
   <th>Examples</th> 
  </tr> 
  <tr> 
   <td>*</td> 
   <td><p>Matches zero or more contiguous instances of any character in the string. The final character of the match is determined by either of the following situations:</p> 
    <ul> 
     <li>A character in the string matches the next character in the pattern, and the pattern character has the following characteristics: 
      <ul> 
       <li>Not a *</li> 
       <li>Not a ?</li> 
       <li>A literal character (including a space) or a character class.</li> 
      </ul> </li> 
     <li>The end of the pattern is reached.</li> 
    </ul> <p>Inside a character class, the character is interpreted literally.</p> </td> 
   <td><p>*/geo*</p> <p>Matches any page below the /content/geometrixx node and the /content/geometrixx-outdoors node. The following HTTP requests match the glob pattern:</p> 
    <ul> 
     <li>"GET /content/geometrixx/en.html"</li> 
     <li>"GET /content/geometrixx-outdoors/en.html" </li> 
    </ul> <p>*outdoors/*</p> <p>Matches any page below the /content/geometrixx-outdoors node. For example, the following HTTP request matches the glob pattern:</p> 
    <ul> 
     <li>"GET /content/geometrixx-outdoors/en.html"</li> 
    </ul> </td> 
  </tr> 
  <tr> 
   <td>?</td> 
   <td><p>Matches any single character. Use outside character classes.</p> <p>Inside a character class, this character is interpreted literally.</p> </td> 
   <td><p>*outdoors/??/*</p> <p>Matches the pages for any language in the geometrixx-outdoors site. For example, the following HTTP request matches the glob pattern:</p> 
    <ul> 
     <li>"GET /content/geometrixx-outdoors/en/men.html"</li> 
    </ul> <p>The following request does not match the glob pattern:</p> 
    <ul> 
     <li>"GET /content/geometrixx-outdoors/en.html" </li> 
    </ul> </td> 
  </tr> 
  <tr> 
   <td>[ and ]</td> 
   <td><p>Demarks the beginning and end of a character class.</p> <p>Character classes can include one or more character ranges and single characters.</p> <p>A match occurs if the target character matches any of the characters in the character class, or within a defined range.</p> <p>If the closing bracket is not included, the pattern produces no matches.</p> <p></p> <p></p> <p></p> </td> 
   <td><p>*[o]men.html*</p> <p>Matches the following HTTP request:</p> 
    <ul> 
     <li>"GET /content/geometrixx-outdoors/en/women.html"</li> 
    </ul> <p>Does not match the following HTTP request:</p> 
    <ul> 
     <li>"GET /content/geometrixx-outdoors/en/men.html" </li> 
    </ul> <p>*[o/]men.html*</p> <p>Matches the following HTTP requests: </p> 
    <ul> 
     <li>"GET /content/geometrixx-outdoors/en/women.html" </li> 
     <li> "GET /content/geometrixx-outdoors/en/men.html" </li> 
    </ul> <p></p> </td> 
  </tr> 
  <tr> 
   <td>-</td> 
   <td><p>Denotes a range of characters. For use in character classes.<br /> </p> <p>Outside of a character class, this character is interpreted literally.</p> <p></p> </td> 
   <td><p>*[m-p]men.html*</p> <p>Matches the following HTTP request:</p> 
    <ul> 
     <li>"GET /content/geometrixx-outdoors/en/women.html"</li> 
    </ul> <p>Does not match the following HTTP request:</p> 
    <ul> 
     <li>"GET /content/geometrixx-outdoors/en/men.html"</li> 
    </ul> <p></p> </td> 
  </tr> 
  <tr> 
   <td>!</td> 
   <td><p>Negates the character or character class that follows. Use only for negating characters and character ranges inside character classes. Equivalent to the ^ wildcard.</p> <p>Outside of a character class, this character is interpreted literally.</p> <p></p> </td> 
   <td><p>*[!o]men.html*</p> <p>Matches the following HTTP request: </p> 
    <ul> 
     <li>"GET /content/geometrixx-outdoors/en/men.html"</li> 
    </ul> <p>Does not match the following HTTP request</p> 
    <ul> 
     <li>"GET /content/geometrixx-outdoors/en/women.html" </li> 
    </ul> <p>*[!o!/]men.html*</p> <p>Does not match the following HTTP request:</p> 
    <ul> 
     <li>"GET /content/geometrixx-outdoors/en/women.html" or "GET /content/geometrixx-outdoors/en/men. html" </li> 
    </ul> </td> 
  </tr> 
  <tr> 
   <td>^</td> 
   <td><p>Negates the character or character range that follows. Use for negating only characters and character ranges inside character classes. Equivalent to the ! wildcard character.</p> <p>Outside of a character class, this charcter is interpreted literally.</p> </td> 
   <td>The examples for the ! wildcard character apply, substituting the ! characters in the example patterns with ^ characters.</td> 
  </tr> 
 </tbody> 
</table>
-->

## Logging {#logging}

웹 서버 구성에서 다음을 설정할 수 있습니다.

* Dispatcher 로그 파일의 위치입니다.
* 로그 수준입니다.

자세한 내용은 웹 서버 문서 및 Dispatcher 인스턴스의 readme 파일을 참조하십시오.

**Apache Rotated/Piped 로그**

**Apache** 웹 서버를 사용하는 경우 회전 및/또는 piped 로그에 표준 기능을 사용할 수 있습니다. 예를 들어 Piped 로그를 사용하는 경우:

`DispatcherLog "| /usr/apache/bin/rotatelogs logs/dispatcher.log%Y%m%d 604800"`

자동으로 회전됩니다.

* Dispatcher 로그 파일; 확장자에 타임스탬프를 추가합니다 (로그/dispatcher. log % Y % m % d).
* (60 x 60 x 24 x 7 = 604800 초)

Please see the Apache web server documentation on Log Rotation and Piped Logs; for example [Apache 2.2](https://httpd.apache.org/docs/2.2/logs.html).

>[!NOTE]
>
>설치 시 기본 로그 수준이 높음 (예: Level 3 = debug) 이므로 디스패처에서 모든 오류 및 경고를 기록합니다. 이것은 초기 단계에서 매우 유용합니다.
>
>However, this requires additional resources, so when the Dispatcher is working smoothly *according to your requirements*, you can(should) lower the log level.

### Trace Logging {#trace-logging}

Dispatcher의 다른 향상 기능에서도 버전 4.2.0는 추적 로깅을 도입했습니다.

이것은 디버그 로깅보다 더 높은 수준이며 로그에 추가 정보를 표시합니다. 여기에는 다음에 대한 로깅이 추가됩니다.

* 전달된 헤더의 값;
* 특정 작업에 적용되는 규칙입니다.

You can enable Trace Logging by setting the log level to `4` in your web server.

다음은 추적 기능이 활성화된 로그의 예입니다.

```xml
[Thu Mar 03 16:05:38 2016] [T] [17183] request.headers[Host] = "localhost:8443"
[Thu Mar 03 16:05:38 2016] [T] [17183] request.headers[User-Agent] = "curl/7.43.0"
[Thu Mar 03 16:05:38 2016] [T] [17183] request.headers[Accept] = "*/*"
[Thu Mar 03 16:05:38 2016] [T] [17183] request.headers[X-Forwarded-SSL-Client-Cert] = "(null)"
[Thu Mar 03 16:05:38 2016] [T] [17183] request.headers[Via] = "1.1 localhost:8443 (dispatcher)"
[Thu Mar 03 16:05:38 2016] [T] [17183] request.headers[X-Forwarded-For] = "::1"
[Thu Mar 03 16:05:38 2016] [T] [17183] request.headers[X-Forwarded-SSL] = "on"
[Thu Mar 03 16:05:38 2016] [T] [17183] request.headers[X-Forwarded-SSL-Cipher] = "DHE-RSA-AES256-SHA"
[Thu Mar 03 16:05:38 2016] [T] [17183] request.headers[X-Forwarded-SSL-Session-ID] = "ba931f5e4925c2dde572d766fdd436375e15a0fd24577b91f4a4d51232a934ae"
[Thu Mar 03 16:05:38 2016] [T] [17183] request.headers[X-Forwarded-Port] = "8443"
[Thu Mar 03 16:05:38 2016] [T] [17183] request.headers[Server-Agent] = "Communique-Dispatcher"
```

and an event logged when a file that match a blocking rule is requested:

```xml
[Thu Mar 03 14:42:45 2016] [T] [11831] 'GET /content.infinity.json HTTP/1.1' was blocked because of /0082
```

## Confirming Basic Operation {#confirming-basic-operation}

웹 서버, Dispatcher 및 AEM 인스턴스의 기본 작업 및 상호 작용을 확인하려면 다음 단계를 사용할 수 있습니다.

1. `loglevel` To `3`를 설정합니다.

1. 웹 서버를 시작합니다. 이렇게 하면 디스패처가 시작됩니다.
1. AEM 인스턴스를 시작합니다.
1. 웹 서버 및 디스패처에 대한 로그 및 오류 파일을 확인합니다.\
   웹 서버에 따라 다음과 같은 메시지가 표시됩니다.\
   `[Thu May 30 05:16:36 2002] [notice] Apache/2.0.50 (Unix) configured`\
   및:\
   `[Fri Jan 19 17:22:16 2001] [I] [19096] Dispatcher initialized (build XXXX)`

1. 웹 서버를 통해 웹 사이트를 탐색합니다. 컨텐츠가 필요에 따라 표시되고 있는지 확인합니다.\
   For example, on a local installation where AEM runs on port `4502` and the web server on `80` access the Websites console using both:\
   ` https://localhost:4502/libs/wcm/core/content/siteadmin.html  
https://localhost:80/libs/wcm/core/content/siteadmin.html  
`결과는 동일해야 합니다. 동일한 메커니즘으로 다른 페이지에 대한 액세스를 확인할 수 있습니다.

1. 캐시 디렉토리가 작성되고 있는지 확인합니다.
1. 페이지를 활성화하여 캐시가 올바르게 플러시되는지 확인합니다.
1. If everything is operating correctly you can reduce the `loglevel` to `0`.

## Using Multiple Dispatchers {#using-multiple-dispatchers}

복잡한 설정에서 여러 발송자를 사용할 수 있습니다. 예를 들어, 다음을 사용할 수 있습니다.

* 인트라넷에 웹 사이트를 게시하는 하나의 Dispatcher
* 다른 주소 아래에 있는 두 번째 Dispatcher와 보안 설정을 사용하여 인터넷에 동일한 내용을 게시합니다.

이러한 경우, 각 요청이 하나의 Dispatcher를 통과하는지 확인하십시오. 디스패처는 다른 디스패처의 요청을 처리하지 않습니다. 따라서 두 디스패처 모두 AEM 웹 사이트에 직접 액세스해야 합니다.

## Debugging {#debugging}

When adding the header `X-Dispatcher-Info` to a request, Dispatcher answers whether the target was cached, returned from cached or not cacheable at all. The response header `X-Cache-Info` contains this information in a readable form. 이러한 응답 헤더를 사용하여 디스패처가 캐시된 응답과 관련된 문제를 디버깅할 수 있습니다.

This functionality is not enabled by default, so in order for the response header `X-Cache-Info` to be included, the farm must contain the following entry:

```xml
/info "1"
```

예,

```xml
/farm
{
    /mywebsite
    {
        # Include X-Cache-Info response header if X-Dispatcher-Info is in request header
        /info "1"
    }
}
```

`X-Dispatcher-Info` 또한 헤더에 값이 필요하지 않지만 테스트에 사용할 `curl` 경우 다음과 같이 헤더를 보내려면 값을 제공해야 합니다.

```xml
curl -v -H "X-Dispatcher-Info: true" https://localhost/content/we-retail/us/en.html
```

Below is a list containing the response headers that `X-Dispatcher-Info` will return:

* **캐시됨**\
   대상 파일이 캐시에 포함되어 있고 디스패처가 이를 전달하는 것이 타당하다고 판단했습니다.
* **캐싱**\
   대상 파일이 캐시에 포함되어 있지 않고, 디스패처가 출력을 캐시하여 전달하는 것이 타당하다고 판단했습니다.
* **캐싱: 통계 파일 가장 최근의**
Target 파일은 캐시에 포함되어 있지만 최근 통계 파일에서 무효화됩니다. 디스패처는 대상 파일을 삭제하고 출력에서 다시 생성하여 전달합니다.
* **캐시할 수 없음: 문서 루트**
없음 Farm의 구성에 문서 루트 (구성 요소 `cache.docroot`) 가 포함되어 있지 않습니다.
* **캐시할 수 없음: 캐시 파일 경로가 너무 깁니다.**\
   대상 파일 (문서 루트 및 URL 파일의 연결) 이 시스템에서 가능한 가장 긴 파일 이름을 초과합니다.
* **캐시할 수 없음: 임시 파일 경로가 너무 깁니다.**\
   임시 파일 이름 템플릿이 시스템에서 가능한 가장 긴 파일 이름을 초과합니다. 디스패처는 캐시된 파일을 실제로 만들거나 덮어쓴 후에 임시 파일을 먼저 만듭니다. The temporary file name is the target file name with the characters `_YYYYXXXXXX` appended to it, where the `Y` and `X` will be replaced to create a unique name.
* **캐시할 수 없음: 요청 URL에 확장 기능이 없습니다.**\
   The request URL has no extension, or there is a path following the file extension, for example: `/test.html/a/path`.
* **캐시할 수 없음: Request was a get or head**
the HTTP method is either a get or a head. 디스패처는 출력에 캐시해서는 안 되는 동적 데이터가 포함되어 있다고 가정합니다.
* **캐시할 수 없음: 요청에 쿼리 문자열이 들어 있음**\
   요청에 쿼리 문자열이 들어 있습니다. 디스패처는 출력이 지정된 쿼리 문자열에 따라 달라지고 캐시되지 않는다고 가정합니다.
* **캐시할 수 없음: 세션 관리자가 인증되지 않았습니다.**\
   The farm&#39;s cache is governed by a session manager (the configuration contains a `sessionmanagement` node) and the request didn&#39;t contain the appropriate authentication information.
* **캐시할 수 없음: 요청에 권한 포함**\
   The farm is not allowed to cache output ( `allowAuthorized 0`) and the request contains authentication information.
* **캐시할 수 없음: Target는 디렉토리입니다.**\
   대상 파일은 디렉토리입니다. This might point to some conceptual mistake, where a URL and some sub-URL both contain cacheable output, for example if a request to `/test.html/a/file.ext` comes first and contains cacheable output, the dispatcher will not be able to cache the output of a subsequent request to `/test.html`.
* **캐시할 수 없음: 요청 URL에 후행 슬래시가 있음**\
   요청 URL에 후행 슬래시가 있습니다.
* **캐시할 수 없음: 캐시 규칙이 아닌 요청 URL**\
   팜의 캐시 규칙은 일부 요청 URL의 출력 캐시를 명시적으로 거부합니다.
* **캐시할 수 없음: 인증 검사기가 액세스를 거부했습니다.**\
   팜의 인증 검사기가 캐시된 파일에 대한 액세스를 거부했습니다.
* **캐시할 수 없음: 세션 유효하지 않은**
경우 farm의 캐시가 세션 관리자에 의해 제어되고 (구성에 `sessionmanagement` 노드가 들어 있음) 사용자의 세션이 더 이상 유효하지 않거나 유효하지 않습니다.
* **캐시할 수 없음: 응답으로 원격`no_cache `** 서버가 `Dispatcher: no_cache` 헤더를 반환하여 디스패처가 출력을 캐시하도록 금지했습니다.
* **캐시할 수 없음: 응답 컨텐츠 길이가 0**
이고 응답 컨텐츠 길이가 0 입니다. 디스패처는 길이가 0 인 파일을 만들지 않습니다.
