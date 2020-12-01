---
title: Dispatcher 구성
description: Dispatcher 구성 방법을 알아봅니다.
translation-type: tm+mt
source-git-commit: 6177dafa64d7c22f72ccb64e343b85f4ee133d73
workflow-type: tm+mt
source-wordcount: '8513'
ht-degree: 2%

---


# Dispatcher 구성 {#configuring-dispatcher}

>[!NOTE]
>
>Dispatcher 버전은 AEM과 독립적입니다. 이전 버전의 AEM에 대한 설명서에 포함된 Dispatcher 설명서에 대한 링크를 따라간 경우 이 페이지로 리디렉션되었을 수 있습니다.

다음 섹션에서는 Dispatcher의 다양한 측면을 구성하는 방법을 설명합니다.

## IPv4 및 IPv6 지원 {#support-for-ipv-and-ipv}

AEM 및 Dispatcher의 모든 요소는 IPv4 및 IPv6 네트워크 모두에 설치할 수 있습니다. [IPV4 및 IPV6](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/introduction/technical-requirements.html?lang=en#ipv-and-ipv)을 참조하십시오.

## 발송자 구성 파일 {#dispatcher-configuration-files}

설치 중에 이 파일의 이름과 위치를 변경할 수는 있지만 기본적으로 Dispatcher 구성은 `dispatcher.any` 텍스트 파일에 저장됩니다.

구성 파일에는 Dispatcher의 동작을 제어하는 일련의 단일 값 또는 다중 값 속성이 포함되어 있습니다.

* 속성 이름 앞에 슬래시(`/`)가 붙습니다.
* 다중값 속성은 중괄호 `{ }`를 사용하여 하위 항목을 묶습니다.

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

* 구성 파일이 큰 경우 여러 개의 작은 파일로 분할할 수 있습니다(관리가 더 쉬워짐). 그런 다음 이러한 파일을 포함할 수 있습니다.
* 자동으로 생성된 파일을 포함하려면

예를 들어 /farms 구성에 myFarm.any 파일을 포함하려면 다음 코드를 사용합니다.

```xml
/farms
  {
  $include "myFarm.any"
  }
```

별표(`*`)를 와일드카드로 사용하여 포함할 파일 범위를 지정합니다.

예를 들어 `farm_1.any` ~ `farm_5.any` 파일에 1~5개의 팜 구성이 포함되어 있는 경우 다음과 같이 포함시킬 수 있습니다.

```xml
/farms
  {
  $include "farm_*.any"
  }
```

## 환경 변수 사용 {#using-environment-variables}

값을 하드 코딩하는 대신 dispatcher.any 파일의 문자열 값 속성에서 환경 변수를 사용할 수 있습니다. 환경 변수의 값을 포함하려면 `${variable_name}` 형식을 사용하십시오.

예를 들어 dispatcher.any 파일이 캐시 디렉토리와 동일한 디렉토리에 있는 경우 [docroot](#specifying-the-cache-directory) 속성에 대해 다음 값을 사용할 수 있습니다.

```xml
/docroot "${PWD}/cache"
```

다른 예로 AEM 게시 인스턴스의 호스트 이름을 저장하는 `PUBLISH_IP`이라는 환경 변수를 만드는 경우 [/renders](#defining-page-renderers-renders) 속성의 다음 구성을 사용할 수 있습니다.

```xml
/renders {
  /0001 {
    /hostname "${PUBLISH_IP}"
    /port "8443"
  }
}
```

## 발송자 인스턴스 이름 지정 {#naming-the-dispatcher-instance-name}

`/name` 속성을 사용하여 Dispatcher 인스턴스를 식별하는 고유한 이름을 지정합니다. `/name` 속성은 구성 구조의 최상위 속성입니다.

## 팜 정의 {#defining-farms-farms}

`/farms` 속성은 각 세트가 다른 웹 사이트 또는 URL과 연결된 하나 이상의 Dispatcher 비헤이비어 세트를 정의합니다. `/farms` 속성은 단일 팜 또는 여러 팜을 포함할 수 있습니다.

* Dispatcher가 모든 웹 페이지 또는 웹 사이트를 동일한 방식으로 처리하도록 하려는 경우 단일 팜을 사용합니다.
* 웹 사이트 또는 다른 웹 사이트에서 다른 디스패처 비헤이비어가 필요한 경우 여러 팜을 만들 수 있습니다.

`/farms` 속성은 구성 구조의 최상위 속성입니다. 팜을 정의하려면 `/farms` 속성에 자식 속성을 추가합니다. Dispatcher 인스턴스 내에서 팜을 고유하게 식별하는 속성 이름을 사용합니다.

`/farmname` 속성은 다중값이며 발송자 동작을 정의하는 다른 속성을 포함합니다.

* 팜이 적용되는 페이지의 URL.
* 문서 렌더링에 사용할 하나 이상의 서비스 URL(일반적으로 AEM 게시 인스턴스)입니다.
* 여러 문서 렌더러를 로드 밸런싱하는 데 사용할 통계입니다.
* 캐시할 파일과 위치 등 여러 가지 다른 비헤이비어

값에는 영숫자(a-z, 0-9)가 포함될 수 있습니다. 다음 예에서는 `/daycom` 및 `/docsdaycom`이라는 이름의 두 팜에 대한 뼈대 정의를 보여 줍니다.

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
>렌더링 팜을 두 개 이상 사용하는 경우 목록이 상향으로 평가됩니다. 이는 웹 사이트에 대해 [가상 호스트](#identifying-virtual-hosts-virtualhosts)를 정의할 때 특히 유용합니다.

각 팜 속성에는 다음 하위 속성이 포함될 수 있습니다.

| 속성 이름 | 설명 |
|--- |--- |
| [/홈 페이지](#specify-a-default-page-iis-only-homepage) | 기본 홈 페이지(선택 사항)(IIS만 해당) |
| [/clientheaders](#specifying-the-http-headers-to-pass-through-clientheaders) | 통과하기 위한 클라이언트 HTTP 요청의 헤더입니다. |
| [/virtualhosts](#identifying-virtual-hosts-virtualhosts) | 이 팜의 가상 호스트입니다. |
| [/sessionmanagement](#enabling-secure-sessions-sessionmanagement) | 세션 관리 및 인증 지원 |
| [/renders](#defining-page-renderers-renders) | 렌더링된 페이지를 제공하는 서버(일반적으로 AEM 게시 인스턴스). |
| [/filter](#configuring-access-to-content-filter) | Dispatcher가 액세스를 허용하는 URL을 정의합니다. |
| [/vanity_url](#enabling-access-to-vanity-urls-vanity-urls) | 별칭 URL에 대한 액세스를 구성합니다. |
| [/propagateSyndPost](#forwarding-syndication-requests-propagatesyndpost) | 신디케이션 요청 전달을 지원합니다. |
| [/cache](#configuring-the-dispatcher-cache-cache) | 캐싱 동작을 구성합니다. |
| [/statistics](#configuring-load-balancing-statistics) | 부하 균형 계산을 위한 통계 범주 정의 |
| [/stickyConnectionsFor](#identifying-a-sticky-connection-folder-stickyconnectionsfor) | 고정 문서가 들어 있는 폴더입니다. |
| [/health_check](#specifying-a-health-check-page) | 서버 가용성을 확인하는 데 사용할 URL입니다. |
| [/retryDelay](#specifying-the-page-retry-delay) | 실패한 연결을 다시 시도하기 전의 지연. |
| [/unavailable벌칙](#reflecting-server-unavailability-in-dispatcher-statistics) | 로드 밸런싱 계산에 대한 통계에 영향을 주는 처벌 |
| [/failover](#using-the-failover-mechanism) | 원래 요청이 실패하면 요청을 다른 렌더링으로 다시 보냅니다. |
| [/auth_checker](permissions-cache.md) | 권한 구분 캐싱의 경우 [보안 콘텐츠 캐싱](permissions-cache.md)을 참조하십시오. |

## 기본 페이지 지정(IIS만 해당) - /homepage {#specify-a-default-page-iis-only-homepage}

>[!CAUTION]
>
>`/homepage`매개 변수(IIS만 해당)가 더 이상 작동하지 않습니다. 대신 [IIS URL 다시 작성 모듈](https://docs.microsoft.com/en-us/iis/extensions/url-rewrite-module/using-the-url-rewrite-module)을 사용해야 합니다.
>
>Apache를 사용하는 경우 `mod_rewrite` 모듈을 사용해야 합니다. `mod_rewrite`(예: [Apache 2.4](https://httpd.apache.org/docs/current/mod/mod_rewrite.html))에 대한 자세한 내용은 Apache 웹 사이트 설명서를 참조하십시오. `mod_rewrite`을(를) 사용할 때는 플래그 **[&#39;passthrough|PT&#39;(다음 처리기로 통과)](https://helpx.adobe.com/dispatcher/kb/DispatcherModReWrite.html)**&#x200B;을(를) 사용하여 다시 작성 엔진을 사용하여 내부 `request_rec` 구조의 `uri` 필드를 `filename` 필드의 값으로 강제 설정하는 것이 좋습니다.

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

## HTTP 헤더를 지정하여 {#specifying-the-http-headers-to-pass-through-clientheaders} 통과

`/clientheaders` 속성은 Dispatcher가 클라이언트 HTTP 요청에서 렌더러(AEM 인스턴스)로 전달하는 HTTP 헤더 목록을 정의합니다.

기본적으로 Dispatcher는 표준 HTTP 헤더를 AEM 인스턴스로 전달합니다. 경우에 따라 추가 헤더를 전달하거나 특정 헤더를 제거할 수 있습니다.

* AEM 인스턴스에서 HTTP 요청에 기대하는 사용자 지정 헤더와 같은 헤더를 추가합니다.
* 웹 서버와 관련된 인증 헤더와 같은 헤더를 제거합니다.

통과하도록 헤더 세트를 사용자 지정하는 경우, 기본적으로 기본적으로 포함되어 있는 헤더 목록을 포함하여 전체 헤더 목록을 지정해야 합니다.

예를 들어 게시 인스턴스의 페이지 활성화 요청을 처리하는 Dispatcher 인스턴스에는 `/clientheaders` 섹션의 `PATH` 헤더가 있어야 합니다. `PATH` 헤더는 복제 에이전트와 디스패처 간의 통신을 활성화합니다.

다음 코드는 `/clientheaders`에 대한 예제 구성입니다.

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

## 가상 호스트 식별 {#identifying-virtual-hosts-virtualhosts}

`/virtualhosts` 속성은 Dispatcher가 이 팜에 대해 허용하는 모든 호스트 이름/URI 조합 목록을 정의합니다. 별표(`*`) 문자를 와일드카드로 사용할 수 있습니다. / `virtualhosts` 속성 값은 다음 형식을 사용합니다.

```xml
[scheme]host[uri][*]
```

* `scheme`:(선택 사항)  `https://` 또는  `https://.`
* `host`:호스트 컴퓨터의 이름 또는 IP 주소와 필요한 경우 포트 번호입니다. ([https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.23](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.23) 참조)
* `uri`:(선택 사항) 리소스의 경로입니다.

다음 예제 구성은 myCompany의 .com 및 .ch 도메인에 대한 요청과 mySubDivision의 모든 도메인을 처리합니다.

```xml
   /virtualhosts
    {
    "www.myCompany.com"
    "www.myCompany.ch"
    "www.mySubDivison.*"
    }
```

다음 구성은 *모든* 요청을 처리합니다.

```xml
   /virtualhosts
    {
    "*"
    }
```

### 가상 호스트 {#resolving-the-virtual-host} 확인

Dispatcher가 HTTP 또는 HTTPS 요청을 받으면 요청의 `host,` `uri` 및 `scheme` 헤더와 가장 잘 일치하는 가상 호스트 값을 찾습니다. Dispatcher는 다음 순서로 `virtualhosts` 속성의 값을 평가합니다.

* 디스패처는 가장 낮은 팜에서 시작하여 dispatcher.any 파일에서 위쪽으로 진행됩니다.
* 각 팜에 대해 Dispatcher는 `virtualhosts` 속성에서 가장 높은 값으로 시작하여 값 목록을 진행합니다.

Dispatcher는 다음과 같은 방식으로 가장 일치하는 가상 호스트 값을 찾습니다.

* 요청의 세 개, `host` 및 `scheme`에 모두 일치하는 첫 번째 접촉 가상 호스트가 사용됩니다.`uri`
* `virtualhosts` 값에 요청의 `scheme` 및 `uri` 부분이 모두 일치하는 부분이 없으면 요청의 `scheme` 및 `uri`와 일치하는 처음 발견된 가상 호스트가 사용됩니다.`host`
* `virtualhosts` 값에 요청의 호스트와 일치하는 호스트 부분이 없는 경우 맨 위에 있는 팜의 맨 위 가상 호스트가 사용됩니다.

따라서 기본 가상 호스트를 `dispatcher.any` 파일의 맨 위 팜에 있는 `virtualhosts` 속성의 맨 위에 배치해야 합니다.

### 가상 호스트 해상도 예 {#example-virtual-host-resolution}

다음 예는 두 개의 Dispatcher 팜을 정의하는 `dispatcher.any` 파일의 조각을 나타내며 각 팜은 `virtualhosts` 속성을 정의합니다.

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

다음 표에서는 해당 HTTP 요청에 대해 해결된 가상 호스트를 보여 줍니다.

| 요청 URL | 해결된 가상 호스트 |
|---|---|
| `https://www.mycompany.com/products/gloves.html` | `www.mycompany.com/products/` |
| `https://www.mycompany.com/about.html` | `www.mycompany.com` |

## 보안 세션 활성화 - /sessionmanagement {#enabling-secure-sessions-sessionmanagement}

>[!CAUTION]
>
>`/allowAuthorized` **이 기능** 을 활성화하려면  `"0"`   `/cache` 섹션에서 로 설정해야 합니다.

사용자가 팜의 페이지에 액세스하기 위해 로그인해야 할 수 있도록 렌더링 팜에 액세스할 수 있는 보안 세션을 만듭니다. 로그인한 사용자는 팜의 페이지에 액세스할 수 있습니다. CUG에서 이 기능 사용에 대한 자세한 내용은 [폐쇄된 사용자 그룹 만들기](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/cug.html?lang=en#creating-the-user-group-to-be-used)를 참조하십시오. 또한 라이브되기 전에 Dispatcher [보안 검사 목록](/help/using/security-checklist.md)을 참조하십시오.

`/sessionmanagement` 속성은 `/farms`의 하위 속성입니다.

>[!CAUTION]
>
>웹 사이트의 섹션에서 서로 다른 액세스 요구 사항을 사용하는 경우 여러 팜을 정의해야 합니다.

**/** sessionmanagementas several sub-parameters:

**/directory** (필수)

세션 정보를 저장하는 디렉토리입니다. 디렉토리가 없는 경우 해당 디렉토리가 생성됩니다.

>[!CAUTION]
>
> 디렉토리 하위 매개 변수 **를 구성할 때 심각한 문제가 발생할 수 있으므로 루트 폴더(`/directory "/"`)를 가리키지 마십시오.** 세션 정보를 저장하는 폴더에 대한 경로를 항상 지정해야 합니다. 예:

```xml
/sessionmanagement
  {
  /directory "/usr/local/apache/.sessions"
  }
```

**/encode** (선택 사항)

세션 정보가 인코딩되는 방법입니다. md5 알고리즘을 사용하는 암호화에 `md5`, 16진수 인코딩에 `hex` 사용 세션 데이터를 암호화할 경우 파일 시스템에 액세스할 수 있는 사용자는 세션 내용을 읽을 수 없습니다. 기본값은 `md5`입니다.

**/header** (선택 사항)

인증 정보를 저장하는 HTTP 헤더 또는 쿠키의 이름입니다. http 헤더에 정보를 저장하는 경우 `HTTP:<header-name>`을 사용하십시오. 정보를 쿠키에 저장하려면 `Cookie:<header-name>`을 사용하십시오. `HTTP:authorization` 값을 지정하지 않으면

**/timeout** (선택 사항)

세션 시간이 지난 후에 초과될 때까지 남은 시간(초)입니다. 지정하지 않은 경우 `"800"`이 사용되므로 사용자의 마지막 요청 이후 세션이 약 13분 이상 시간이 초과됩니다.

구성 예는 다음과 같습니다.

```xml
/sessionmanagement
  {
  /directory "/usr/local/apache/.sessions"
  /encode "md5"
  /header "HTTP:authorization"
  /timeout "800"
  }
```

## 페이지 렌더링 정의 {#defining-page-renderers-renders}

/renders 속성은 Dispatcher가 문서를 렌더링하기 위해 요청을 보내는 URL을 정의합니다. 다음 예제 `/renders` 섹션은 렌더링할 단일 AEM 인스턴스를 식별합니다.

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

다음 예제 /renders 섹션은 발송자와 동일한 컴퓨터에서 실행되는 AEM 인스턴스를 식별합니다.

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

다음 예제 /renders 섹션은 두 AEM 인스턴스 간에 동일한 렌더링 요청을 배포합니다.

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

### 옵션 {#renders-options} 렌더링

**/timeout**

AEM 인스턴스에 액세스하는 연결 시간 초과(밀리초)를 지정합니다. 기본값은 `"0"`이므로 Dispatcher가 무기한 대기합니다.

**/receiveTimeout**

응답이 허용된 시간(밀리초)을 지정합니다. 기본값은 `"600000"`이므로 Dispatcher가 10분 동안 대기합니다. `"0"`을 설정하면 시간 초과가 완전히 제거됩니다.

응답 헤더를 구문 분석하는 동안 시간 초과에 도달하면 HTTP 상태 504(잘못된 게이트웨이)가 반환됩니다. 응답 본문을 읽는 동안 시간 초과에 도달하면 디스패처는 클라이언트에 불완전한 응답을 반환하지만 기록되었을 수 있는 캐시 파일을 삭제합니다.

**/ipv4**

디스패처가 렌더링의 IP 주소를 얻기 위해 `getaddrinfo` 함수(IPv6의 경우) 또는 `gethostbyname` 함수(IPv4의 경우)를 사용하는지 여부를 지정합니다. 값이 0이면 `getaddrinfo`이 사용됩니다. `1` 값이 있으면 `gethostbyname`이(가) 사용됩니다. 기본값은 `0`입니다.

`getaddrinfo` 함수는 IP 주소 목록을 반환합니다. Dispatcher는 TCP/IP 연결을 설정할 때까지 주소 목록을 반복합니다. 따라서 render hostname이 여러 IP 주소와 연결되고 호스트가 `getaddrinfo` 함수에 대한 응답으로 항상 같은 순서로 IP 주소 목록을 반환하는 경우 `ipv4` 속성이 중요합니다. 이 경우 Dispatcher가 연결하는 IP 주소가 임의로 지정되도록 `gethostbyname` 함수를 사용해야 합니다.

Amazon ELB(Elastic Load Balancing)는 잠재적으로 동일한 IP 주소 목록을 가진 getaddrinfo에 응답하는 서비스입니다.

**/secure**

`/secure` 속성에 `"1"` 값이 있는 경우 디스패처는 HTTPS를 사용하여 AEM 인스턴스와 통신합니다. 자세한 내용은 [Dispatcher가 SSL](dispatcher-ssl.md#configuring-dispatcher-to-use-ssl)을 사용하도록 구성을 참조하십시오.

**/always-resolve**

Dispatcher 버전 **4.1.6**&#x200B;에서는 다음과 같이 `/always-resolve` 속성을 구성할 수 있습니다.

* `"1"`으로 설정하면 모든 요청에서 호스트 이름을 확인합니다. 디스패처는 IP 주소를 캐시하지 않습니다. 각 요청에 대한 호스트 정보를 가져오는 데 필요한 추가 호출로 인해 약간의 성능 영향이 있을 수 있습니다.
* 속성이 설정되지 않으면 기본적으로 IP 주소가 캐시됩니다.

또한 다음 샘플에서와 같이 동적 IP 해상도 문제가 발생하는 경우에도 이 속성을 사용할 수 있습니다.

```xml
/renders {
  /0001 {
     /hostname "host-name-here"
     /port "4502"
     /ipv4 "1"
     /always-resolve "1"
     }
  }
```

## 콘텐트 {#configuring-access-to-content-filter}에 대한 액세스 구성

`/filter` 섹션을 사용하여 Dispatcher가 허용하는 HTTP 요청을 지정합니다. 다른 모든 요청은 404 오류 코드(페이지를 찾을 수 없음)와 함께 웹 서버로 다시 전송됩니다. `/filter` 섹션이 없으면 모든 요청이 허용됩니다.

**참고: 상태 파일** 에 대한  [](#naming-the-statfile) 요청은 항상 거부됩니다.

>[!CAUTION]
>
>Dispatcher를 사용하여 액세스를 제한할 때의 추가 고려 사항은 [Dispatcher 보안 검사 목록](security-checklist.md)을 참조하십시오. 또한 AEM 설치에 대한 추가 보안 세부 사항은 [AEM 보안 검사 목록](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-checklist.html?lang=en#security)을 참조하십시오.

`/filter` 섹션은 HTTP 요청의 요청 라인 부분에 있는 패턴에 따라 컨텐츠에 대한 액세스를 거부하거나 허용하는 일련의 규칙으로 구성됩니다. `/filter` 섹션에 허용 목록 전략을 사용해야 합니다.

* 우선 모든 것에 대한 액세스를 거부합니다.
* 필요에 따라 컨텐츠에 대한 액세스를 허용합니다.

### 필터 정의 {#defining-a-filter}

`/filter` 섹션의 각 항목에는 요청 라인 또는 전체 요청 라인의 특정 요소와 일치하는 유형과 패턴이 포함되어 있습니다. 각 필터에는 다음 항목이 포함될 수 있습니다.

* **유형**:패턴과 일치하는 요청에 대한 액세스를 허용할지 또는 거부할지를  `/type` 나타냅니다. 값은 `allow` 또는 `deny`일 수 있습니다.

* **요청 라인의 요소:** HTTP 요청의 요청 라인 부분 `/method`의 이러한 특정 부분 `/url`에 따라 요청을 필터링하는 패턴,  `/query` `/protocol` 또는 포함 전체 요청 라인이 아닌 요청 라인의 요소에 대한 필터링은 기본 필터 방법입니다.

* **요청 라인의 고급 요소:** Dispatcher 4.2.0부터 4개의 새로운 필터 요소를 사용할 수 있습니다. 이러한 새 요소는 각각 `/path`, `/selectors`, `/extension` 및 `/suffix`입니다. URL 패턴을 추가로 제어하려면 이러한 항목 중 하나 이상을 포함합니다.

>[!NOTE]
>
>이러한 각 요소가 참조하는 요청 라인의 일부에 대한 자세한 내용은 [Sling URL Discovertising](https://sling.apache.org/documentation/the-sling-engine/url-decomposition.html) Wiki 페이지를 참조하십시오.

* **glob 속성**:이  `/glob` 속성은 HTTP 요청의 전체 요청 라인과 일치하는 데 사용됩니다.

>[!CAUTION]
>
>전역 필터링 기능은 Dispatcher에서 더 이상 사용되지 않습니다. 따라서 보안 문제가 발생할 수 있으므로 `/filter` 섹션에서 글로브를 사용하지 않아야 합니다. 따라서 다음 대신
>
>`/glob "* *.css *"`
>
>다음에
>
>`/url "*.css"`

#### HTTP 요청의 요청 라인 부분 {#the-request-line-part-of-http-requests}

HTTP/1.1은 [request-line](https://www.w3.org/Protocols/rfc2616/rfc2616-sec5.html)을 다음과 같이 정의합니다.

`Method Request-URI HTTP-Version<CRLF>`

`<CRLF>` 문자는 줄 피드가 뒤에 오는 캐리지 리턴을 나타냅니다. 다음은 클라이언트가 WKND 사이트의 미국-영어 페이지를 요청할 때 수신되는 요청 라인입니다.

`GET /content/wknd/us/en.html HTTP.1.1<CRLF>`

패턴은 요청 라인의 공백 문자와 `<CRLF>` 문자를 고려해야 합니다.

#### 큰따옴표 vs 작은따옴표 {#double-quotes-vs-single-quotes}

필터 규칙을 만들 때는 간단한 패턴에 큰따옴표 `"pattern"`를 사용하십시오. Dispatcher 4.2.0 이상을 사용하고 패턴에 정규 표현식이 포함되어 있는 경우 단일 따옴표 안에 regex 패턴 `'(pattern1|pattern2)'`을(를) 포함해야 합니다.

#### 정규 표현식 {#regular-expressions}

4.2.0 이후 버전의 Dispatcher에서는 필터 패턴에 POSIX 확장 정규 표현식을 포함할 수 있습니다.

#### 필터 문제 해결 {#troubleshooting-filters}

필터가 원하는 방식으로 트리거되지 않는 경우, 요청을 가로채는 필터를 확인할 수 있도록 디스패처에 [추적 로깅](#trace-logging)을 활성화하십시오.

#### 필터 예:모두 거부 {#example-filter-deny-all}

다음 예제 필터 섹션은 Dispatcher가 모든 파일에 대한 요청을 거부하도록 합니다. 모든 파일에 대한 액세스를 거부한 다음 특정 영역에 대한 액세스를 허용해야 합니다.

```xml
  /0001  { /glob "*" /type "deny" }
```

명시적으로 거부된 영역에 대한 요청은 404 오류 코드(페이지를 찾을 수 없음)가 반환되게 합니다.

#### 필터 예:특정 영역에 대한 액세스 거부 {#example-filter-deny-access-to-specific-areas}

또한 필터를 사용하면 ASP 페이지 및 게시 인스턴스 내의 민감한 영역과 같은 다양한 요소에 대한 액세스를 거부할 수 있습니다. 다음 필터는 ASP 페이지에 대한 액세스를 거부합니다.

```xml
/0002  { /type "deny" /url "*.asp"  }
```

#### 필터 예:POST 요청 활성화 {#example-filter-enable-post-requests}

다음 예제 필터를 사용하면 POST 메서드로 양식 데이터를 제출할 수 있습니다.

```xml
/filter {
    /0001  { /glob "*" /type "deny" }
    /0002 { /type "allow" /method "POST" /url "/content/[.]*.form.html" }
}
```

#### 필터 예:워크플로 콘솔 {#example-filter-allow-access-to-the-workflow-console}에 대한 액세스 허용

다음 예는 워크플로우 콘솔에 대한 외부 액세스를 거부하는 데 사용되는 필터를 보여줍니다.

```xml
/filter {
    /0001  { /glob "*" /type "deny" }
    /0002  {  /type "allow"  /url "/libs/cq/workflow/content/console*"  }
}
```

게시 인스턴스가 웹 응용 프로그램 컨텍스트(예: 게시)를 사용하는 경우 필터 정의에도 추가할 수 있습니다.

```xml
/0003   { /type "deny"  /url "/publish/libs/cq/workflow/content/console/archive*"  }
```

여전히 제한된 영역에서 단일 페이지에 액세스해야 하는 경우 해당 페이지에 대한 액세스를 허용할 수 있습니다. 예를 들어 워크플로우 콘솔 내의 보관 탭에 액세스할 수 있도록 하려면 다음 섹션을 추가합니다.

```xml
/0004  { /type "allow"  /url "/libs/cq/workflow/content/console/archive*"   }
```

>[!NOTE]
>
>여러 필터 패턴이 요청에 적용되면 적용되는 마지막 필터 패턴이 적용됩니다.

#### 필터 예:정규 표현식 사용 {#example-filter-using-regular-expressions}

이 필터는 단일 인용 부호 사이에 여기에 정의된 일반 표현식을 사용하여 공중이 아닌 컨텐츠 디렉토리의 익스텐션을 활성화합니다.

```xml
/005  {  /type "allow" /extension '(css|gif|ico|js|png|swf|jpe?g)' }
```

#### 필터 예:요청 URL {#example-filter-filter-additional-elements-of-a-request-url}의 추가 요소 필터링

다음은 경로, 선택기 및 확장명에 대한 필터를 사용하여 `/content` 경로 및 해당 하위 트리에서 표시되는 컨텐츠를 차단하는 규칙 예입니다.

```xml
/006 {
        /type "deny"
        /path "/content/*"
        /selectors '(feed|rss|pages|languages|blueprint|infinity|tidy)'
        /extension '(json|xml|html)'
        }
```

### 예 /filter section {#example-filter-section}

Dispatcher를 구성할 때는 외부 액세스를 가능한 한 제한해야 합니다. 다음 예에서는 외부 방문자에 대한 최소한의 액세스를 제공합니다.

* `/content`
* 디자인 및 클라이언트 라이브러리와 같은 기타 컨텐츠예를 들면 다음과 같습니다.

   * `/etc/designs/default*`
   * `/etc/designs/mydesign*`

필터를 만든 후 [테스트 페이지 액세스](#testing-dispatcher-security)를 통해 AEM 인스턴스가 안전한지 확인합니다.

`dispatcher.any` 파일의 다음 `/filter` 섹션을 [Dispatcher 구성 파일의 기초로 사용할 수 있습니다.](#dispatcher-configuration-files)

이 예는 Dispatcher와 함께 제공되며 프로덕션 환경에서 사용하기 위한 예로서 사용되는 기본 구성 파일을 기반으로 합니다. `#`으로 접두사가 추가된 항목은 비활성화(주석 처리됨)됩니다. 보안 영향이 있을 수 있으므로 해당 행에서 `#`을(를) 제거하여 이러한 항목을 활성화하려는 경우 주의해야 합니다.

모든 항목에 대한 액세스를 거부한 다음 특정(제한된) 요소에 대한 액세스를 허용해야 합니다.

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
>Apache와 함께 사용할 때 Dispatcher 모듈의 DispatcherUseProcessedURL 속성에 따라 필터 URL 패턴을 디자인합니다. ([Apache 웹 서버 - Dispatcher용 Apache 웹 서버 구성](dispatcher-install.md##apache-web-server-configure-apache-web-server-for-dispatcher)을 참조하십시오.)

>[!NOTE]
>
>Dynamic Media 관련 필터 `0030` 및 `0031`은 AEM 6.0 이상에 적용됩니다.

액세스를 확장하도록 선택한 경우 다음 권장 사항을 고려하십시오.

* CQ 버전 5.4 또는 이전 버전을 사용하는 경우 `/admin`에 대한 외부 액세스는 항상 *완전히*&#x200B;가 비활성화되어야 합니다.

* `/libs`의 파일에 액세스할 수 있도록 허용하려면 주의해야 합니다. 액세스 권한은 개별적으로 허용되어야 합니다.
* 복제 구성에 대한 액세스를 거부하여 볼 수 없습니다.

   * `/etc/replication.xml*`
   * `/etc/replication.infinity.json*`

* Google 가젯 역방향 프록시 액세스 거부:

   * `/libs/opensocial/proxy*`

설치 내용에 따라 `/libs`, `/apps` 또는 기타 다른 곳에 추가 리소스가 있을 수 있으며 이러한 리소스를 사용할 수 있어야 합니다. 외부에서 액세스되는 리소스를 확인하는 방법 중 하나로 `access.log` 파일을 사용할 수 있습니다.

>[!CAUTION]
>
>콘솔 및 디렉토리 액세스는 프로덕션 환경에 대한 보안 위험을 초래할 수 있습니다. 명시적 명시가 없는 경우 비활성화된(주석 처리됨) 상태로 두어야 합니다.

>[!CAUTION]
>
>게시 환경](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/reporting.html?lang=en#using-reports-in-a-publish-environment)에서 보고서를 사용하는 [인 경우 Dispatcher가 외부 방문자에 대해 `/etc/reports`에 대한 액세스를 거부하도록 구성해야 합니다.

### 쿼리 문자열 제한 {#restricting-query-strings}

Dispatcher 버전 4.1.5부터 `/filter` 섹션을 사용하여 쿼리 문자열을 제한합니다. 쿼리 문자열을 명시적으로 허용하고 `allow` 필터 요소를 통해 일반 허용을 제외하는 것이 좋습니다.

단일 항목에는 `glob` 또는 `method`, `url`, `query` 및 `version`의 일부 조합을 사용할 수 있지만 둘 다 사용할 수는 없습니다. 다음 예제에서는 `a=*` 쿼리 문자열을 허용하고 `/etc` 노드로 전달되는 URL에 대한 다른 모든 쿼리 문자열을 거부합니다.

```xml
/filter {
 /0001 { /type "deny" /method "POST" /url "/etc/*" }
 /0002 { /type "allow" /method "GET" /url "/etc/*" /query "a=*" }
}
```

>[!NOTE]
>
>규칙에 `/query`이 포함된 경우 쿼리 문자열이 포함된 요청만 일치하고 제공된 쿼리 패턴과 일치합니다.
>
>위의 예에서 쿼리 문자열이 없는 `/etc`에 대한 요청도 함께 허용되어야 하는 경우 다음 규칙이 필요합니다.


```xml
/filter {  
>/0001 { /type "deny" /method “*" /url "/path/*" }  
>/0002 { /type "allow" /method "GET" /url "/path/*" }  
>/0003 { /type “deny" /method "GET" /url "/path/*" /query "*" }  
>/0004 { /type "allow" /method "GET" /url "/path/*" /query "a=*" }  
}  
```

### Dispatcher 보안 테스트 {#testing-dispatcher-security}

디스패처 필터는 AEM 게시 인스턴스의 다음 페이지 및 스크립트에 대한 액세스를 차단해야 합니다. 웹 브라우저를 사용하여 다음 페이지를 사이트 방문자로 열고 코드 404가 반환되는지 확인합니다. 다른 결과가 나오면 필터를 조정합니다.

`/content/add_valid_page.html?debug=layout`에 대한 일반 페이지 렌더링이 표시되어야 합니다.

* `/admin`
* `/system/console`
* `/dav/crx.default`
* `/crx`
* `/bin/crxde/logs`
* `/jcr:system/jcr:versionStorage.json`
* `/_jcr_system/_jcr_versionStorage.json`
* `/libs/wcm/core/content/siteadmin.html`
* `/libs/collab/core/content/admin.html`
* `/libs/cq/ui/content/dumplibs.html`
* `/var/linkchecker.html`
* `/etc/linkchecker.html`
* `/home/users/a/admin/profile.json`
* `/home/users/a/admin/profile.xml`
* `/libs/cq/core/content/login.json`
* `/content/../libs/foundation/components/text/text.jsp`
* `/content/.{.}/libs/foundation/components/text/text.jsp`
* `/apps/sling/config/org.apache.felix.webconsole.internal.servlet.OsgiManager.config/jcr%3acontent/jcr%3adata`
* `/libs/foundation/components/primary/cq/workflow/components/participants/json.GET.servlet`
* `/content.pages.json`
* `/content.languages.json`
* `/content.blueprint.json`
* `/content.-1.json`
* `/content.10.json`
* `/content.infinity.json`
* `/content.tidy.json`
* `/content.tidy.-1.blubber.json`
* `/content/dam.tidy.-100.json`
* `/content/content/geometrixx.sitemap.txt `
* `/content/add_valid_page.query.json?statement=//*`
* `/content/add_valid_page.qu%65ry.js%6Fn?statement=//*`
* `/content/add_valid_page.query.json?statement=//*[@transportPassword]/(@transportPassword%20|%20@transportUri%20|%20@transportUser)`
* `/content/add_valid_path_to_a_page/_jcr_content.json`
* `/content/add_valid_path_to_a_page/jcr:content.json`
* `/content/add_valid_path_to_a_page/_jcr_content.feed`
* `/content/add_valid_path_to_a_page/jcr:content.feed`
* `/content/add_valid_path_to_a_page/pagename._jcr_content.feed`
* `/content/add_valid_path_to_a_page/pagename.jcr:content.feed`
* `/content/add_valid_path_to_a_page/pagename.docview.xml`
* `/content/add_valid_path_to_a_page/pagename.docview.json`
* `/content/add_valid_path_to_a_page/pagename.sysview.xml`
* `/etc.xml`
* `/content.feed.xml`
* `/content.rss.xml`
* `/content.feed.html`
* `/content/add_valid_page.html?debug=layout`
* `/projects`
* `/tagging`
* `/etc/replication.html`
* `/etc/cloudservices.html`
* `/welcome`

익명 쓰기 액세스 허용 여부를 확인하려면 터미널 또는 명령 프롬프트에서 다음 명령을 실행합니다. 노드에 데이터를 쓸 수 없습니다.

`curl -X POST "https://anonymous:anonymous@hostname:port/content/usergenerated/mytestnode"`

Dispatcher 캐시를 무효화하려고 시도하고 코드 404 응답을 수신하는지 확인하려면 터미널 또는 명령 프롬프트에서 다음 명령을 실행합니다.

`curl -H "CQ-Handle: /content" -H "CQ-Path: /content" https://yourhostname/dispatcher/invalidate.cache`

## 별칭 URL에 대한 액세스 활성화 {#enabling-access-to-vanity-urls-vanity-urls}

<!-- 

Comment Type: remark
Last Modified By: unknown unknown (sbroders@adobe.com)
Last Modified Date: 2015-03-25T14:23:05.185-0400

<p style="font-family: tahoma, arial, helvetica, sans-serif; font-size: 12px;">For https://jira.corp.adobe.com/browse/DOC-4812</p> 
<p style="font-family: tahoma, arial, helvetica, sans-serif; font-size: 12px;">The "com.adobe.granite.dispatcher.vanityurl.content" package needs to be made public before publishing this contnet.</p>
 -->

AEM 페이지에 대해 구성된 별칭 URL에 액세스할 수 있도록 Dispatcher를 구성합니다.

별칭 URL에 대한 액세스가 활성화되면 Dispatcher는 렌더링 인스턴스에서 실행되는 서비스를 주기적으로 호출하여 별칭 URL 목록을 가져옵니다. Dispatcher는 이 목록을 로컬 파일에 저장합니다. `/filter` 섹션의 필터로 인해 페이지 요청이 거부되면, Dispatcher는 별칭 URL 목록을 협의합니다. 거부된 URL이 목록에 있으면 Dispatcher에서 별칭 URL에 액세스할 수 있습니다.

별칭 URL에 대한 액세스를 활성화하려면 다음 예와 유사한 `/farms` 섹션에 `/vanity_urls` 섹션을 추가하십시오.

```xml
 /vanity_urls {
      /url "/libs/granite/dispatcher/content/vanityUrls.html"
      /file "/tmp/vanity_urls"
      /delay 300
 }
```

`/vanity_urls` 섹션에는 다음 속성이 포함됩니다.

* `/url`:렌더링 인스턴스에서 실행되는 별칭 URL 서비스의 경로입니다. 이 속성의 값은 `"/libs/granite/dispatcher/content/vanityUrls.html"`이어야 합니다.

* `/file`:별칭 URL 목록을 저장하는 로컬 파일 경로입니다. Dispatcher가 이 파일에 대한 쓰기 액세스 권한을 가지고 있는지 확인하십시오.
* `/delay`:(초) 별칭 URL 서비스에 대한 호출 사이의 시간입니다.

>[!NOTE]
>
>렌더링이 AEM의 인스턴스인 경우 별칭 URL 서비스를 활성화하려면 소프트웨어 배포](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/granite/vanityurls-components)에서 [VanityURLS-Components 패키지를 설치해야 합니다. (자세한 내용은 [소프트웨어 배포](https://experienceleague.adobe.com/docs/experience-manager-65/administering/contentmanagement/package-manager.html?lang=en#software-distribution)를 참조하십시오.)

별칭 URL에 대한 액세스를 활성화하려면 다음 절차를 사용하십시오.

1. 렌더링 서비스가 AEM 인스턴스인 경우 게시 인스턴스에 &#39;com.adobe.granite.dispatcher.vanityurl.content 패키지를 설치합니다(위 참고 참조).
1. AEM 또는 CQ 페이지에 대해 구성한 별칭 URL에 대해 [`/filter`](#configuring-access-to-content-filter) 구성이 URL을 거부하는지 확인하십시오. 필요한 경우 URL을 거부하는 필터를 추가합니다.
1. `/farms` 아래에 `/vanity_urls` 섹션을 추가합니다.
1. Apache 웹 서버를 다시 시작합니다.

## 신디케이션 요청 전달 - /propagateSyncPost {#forwarding-syndication-requests-propagatesyndpost}

신디케이션 요청은 일반적으로 디스패처용이므로 기본적으로 렌더러(예: AEM 인스턴스)로 전송되지 않습니다.

필요한 경우 `/propagateSyndPost` 속성을 `"1"`로 설정하여 신디케이션 요청을 Dispatcher로 전달합니다. 설정된 경우 필터 섹션에서 POST 요청이 거부되지 않도록 해야 합니다.

## 발송자 캐시 구성 - /cache {#configuring-the-dispatcher-cache-cache}

`/cache` 섹션은 Dispatcher가 문서를 캐시하는 방식을 제어합니다. 캐싱 전략을 구현하도록 여러 하위 속성을 구성합니다.

* `/docroot`
* `/statfile`
* `/serveStaleOnError`
* `/allowAuthorized`
* `/rules`
* `/statfileslevel`
* `/invalidate`
* `/invalidateHandler`
* `/allowedClients`
* `/ignoreUrlParams`
* `/headers`
* `/mode`
* `/gracePeriod`
* `/enableTTL`

예제 캐시 섹션은 다음과 같습니다.

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
>권한 구분 캐시의 경우 [보안 컨텐츠 캐싱](permissions-cache.md)을 읽어 보십시오.

### 캐시 디렉터리 지정 {#specifying-the-cache-directory}

`/docroot` 속성은 캐시된 파일이 저장되는 디렉토리를 식별합니다.

>[!NOTE]
>
>이 값은 Dispatcher 및 웹 서버가 동일한 파일을 처리하도록 웹 서버의 문서 루트와 정확히 동일한 경로여야 합니다.\
>웹 서버는 디스패처 캐시 파일을 사용할 때 올바른 상태 코드를 전달해야 하므로 이 파일도 찾을 수 있어야 합니다.

여러 팜을 사용하는 경우 각 팜은 다른 문서 루트를 사용해야 합니다.

### 상태 파일 이름 지정 {#naming-the-statfile}

`/statfile` 속성은 상태 파일로 사용할 파일을 식별합니다. Dispatcher는 이 파일을 사용하여 최신 컨텐츠 업데이트 시간을 등록합니다. 상태 파일은 웹 서버의 모든 파일이 될 수 있습니다.

상태 파일에 내용이 없습니다. 컨텐츠가 업데이트되면 Dispatcher는 타임스탬프를 업데이트합니다. 기본 상태 파일의 이름은 `.stat`이며 doroot에 저장됩니다. 디스패처는 상태 파일에 대한 액세스를 차단합니다.

>[!NOTE]
>
>`/statfileslevel`이(가) 구성된 경우 Dispatcher는 `/statfile` 속성을 무시하고 `.stat`를 이름으로 사용합니다.

### 오류가 발생하면 오래된 문서 제공 {#serving-stale-documents-when-errors-occur}

`/serveStaleOnError` 속성은 렌더링 서버에서 오류를 반환하는 경우 Dispatcher가 무효화된 문서를 반환할지 여부를 제어합니다. 기본적으로 상태 파일을 터치하고 캐시된 컨텐츠를 무효화하면 Dispatcher는 다음에 요청할 때 캐시된 컨텐츠를 삭제합니다.

`/serveStaleOnError`이(가) `"1"`로 설정된 경우, 렌더링 서버에서 성공적인 응답을 반환하지 않는 한 Dispatcher는 무효화된 컨텐츠를 캐시에서 삭제하지 않습니다. AEM의 5xx 응답이나 연결 시간 초과로 인해 Dispatcher가 오래된 컨텐츠를 제공하고 HTTP 상태(재유효성 검사 실패)를 사용하여 응답합니다.

### 인증이 사용될 때 캐싱{#caching-when-authentication-is-used}

`/allowAuthorized` 속성은 다음 인증 정보가 포함된 요청이 캐시되는지 여부를 제어합니다.

* `authorization` 헤더
* `authorization`이라는 쿠키
* `login-token`이라는 쿠키

기본적으로, 캐시된 문서가 클라이언트에 반환될 때 인증이 수행되지 않으므로 이 인증 정보를 포함하는 요청은 캐시되지 않습니다. 이 구성은 Dispatcher가 필요한 권한이 없는 사용자에게 캐시된 문서를 제공하지 못하게 합니다.

그러나 요구 사항에 인증된 문서의 캐싱이 허용되면 `/allowAuthorized`을(를) 하나로 설정합니다.

`/allowAuthorized "1"`

>[!NOTE]
>
>세션 관리를 사용하려면 `/sessionmanagement` 속성을 `/allowAuthorized` 속성으로 설정해야 합니다.`"0"`

### 캐싱할 문서 지정 {#specifying-the-documents-to-cache}

`/rules` 속성은 문서 경로에 따라 캐시되는 문서를 제어합니다. `/rules` 속성에 관계없이 Dispatcher는 다음 상황에서 문서를 캐시하지 않습니다.

* 요청 URI에 물음표(`?`)가 포함된 경우.
   * 일반적으로 이것은 캐시할 필요가 없는 검색 결과와 같은 동적 페이지를 나타냅니다.
* 파일 확장명이 없습니다.
   * 웹 서버에는 문서 유형(MIME 유형)을 결정하기 위한 확장명이 필요합니다.
* 인증 헤더가 설정됨(구성할 수 있음).
* AEM 인스턴스가 다음 헤더로 응답하는 경우:

   * `no-cache`
   * `no-store`
   * `must-revalidate`

>[!NOTE]
>
>GET 또는 HEAD(HTTP 헤더의 경우) 메서드는 Dispatcher에서 캐시할 수 있습니다. 응답 헤더 캐시에 대한 자세한 내용은 [HTTP 응답 헤더 캐싱](#caching-http-response-headers) 섹션을 참조하십시오.

`/rules` 속성의 각 항목에는 [`glob`](#designing-patterns-for-glob-properties) 패턴과 유형이 포함되어 있습니다.

* `glob` 패턴은 문서의 경로와 일치하는 데 사용됩니다.
* 유형은 `glob` 패턴과 일치하는 문서를 캐시할지 여부를 나타냅니다. 이 값은 허용(문서를 캐시할 수 있음) 또는 거부(문서를 항상 렌더링할 수 있음)일 수 있습니다.

동적 페이지가 없는 경우(위의 규칙에 의해 이미 제외된 페이지 제외) Dispatcher가 모든 것을 캐시하도록 구성할 수 있습니다. 이에 대한 규칙 섹션은 다음과 같습니다.

```xml
/rules
  {
    /0000  {  /glob "*"   /type "allow" }
  }
```

전역 속성에 대한 자세한 내용은 전역 속성의 패턴 디자인](#designing-patterns-for-glob-properties)을 참조하십시오.[

동적(예: 뉴스 애플리케이션) 또는 닫힌 사용자 그룹 내에 페이지의 일부 섹션이 있는 경우 예외를 정의할 수 있습니다.

>[!NOTE]
>
>캐시된 페이지에 대한 사용자 권한이 확인되지 않으므로 닫힌 사용자 그룹을 캐시해서는 안 됩니다.

```xml
/rules
  {
   /0000  { /glob "*" /type "allow" }
   /0001  { /glob "/en/news/*" /type "deny" }
   /0002  { /glob "*/private/*" /type "deny"  }
  }
```

**압축**

Apache 웹 서버에서는 캐시된 문서를 압축할 수 있습니다. 압축으로 인해 Apache는 클라이언트가 요청한 경우 문서를 압축 양식으로 반환할 수 있습니다. 압축은 Apache 모듈 `mod_deflate`을(를) 활성화하여 자동으로 수행됩니다. 예:

```xml
AddOutputFilterByType DEFLATE text/plain
```

이 모듈은 Apache 2.x와 함께 기본적으로 설치됩니다.

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

### 폴더 수준 {#invalidating-files-by-folder-level}별로 파일 무효화

`/statfileslevel` 속성을 사용하여 경로에 따라 캐시된 파일을 무효화합니다.

* Dispatcher는 문서 루트 폴더에서 지정한 수준으로 각 폴더에 `.stat`개의 파일을 만듭니다. docroot 폴더는 레벨 0입니다.
* `.stat` 파일을 터치하면 파일이 무효화됩니다. `.stat` 파일의 마지막 수정 날짜는 캐시된 문서의 마지막 수정 날짜와 비교됩니다. `.stat` 파일이 최신 파일이면 문서를 다시 가져옵니다.

* 특정 수준에 있는 파일이 무효화되면 무효화된 파일 또는 구성된 `statsfilevel` 수준(더 작은 파일)의 모든&#x200B;**docroot**&#x200B;에서`.stat` 파일 모두를 터치합니다.****

   * 예:`statfileslevel` 속성을 6으로 설정하고 5단계에서 파일이 무효화되는 경우 doroot에서 5로 모든 `.stat` 파일이 수정됩니다. 이 예에서 파일이 레벨 7에서 무효화되는 경우 마다 이 예제를 계속 진행합니다. `stat` docroot에서 6까지 파일을 수정하게 됩니다(이후 `/statfileslevel = "6"`).

무효화된 파일에 대한 경로&#x200B;**의**&#x200B;만 영향을 받습니다. 다음 예를 생각해 보십시오.웹 사이트에서는 `/content/myWebsite/xx/.` 구조를 사용합니다. `statfileslevel`를 3으로 설정하면 `.stat`파일이 다음과 같이 만들어집니다.

* `docroot`
* `/content`
* `/content/myWebsite`
* `/content/myWebsite/*xx*`

`/content/myWebsite/xx`에 있는 파일이 무효화되면 doroot에서 `/content/myWebsite/xx`까지 모든 `.stat` 파일이 터치됩니다. 이것은 `/content/myWebsite/xx`에 대해서만 해당되며, 예: `/content/myWebsite/yy` 또는 `/content/anotherWebSite`에는 해당되지 않습니다.

>[!NOTE]
>
>추가 헤더 `CQ-Action-Scope:ResourceOnly`을(를) 전송하면 무효화를 방지할 수 있습니다. 캐시의 다른 부분을 무효화하지 않고 특정 리소스를 플러시하는 데 사용할 수 있습니다. 자세한 내용은 [이 페이지](https://adobe-consulting-services.github.io/acs-aem-commons/features/dispatcher-flush-rules/index.html) 및 [수동으로 Dispatcher 캐시 무효화를 참조하십시오](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/page-invalidate.html?lang=en#configuring).

>[!NOTE]
>
>`/statfileslevel` 속성의 값을 지정하면 `/statfile` 속성이 무시됩니다.

### 캐시된 파일 {#automatically-invalidating-cached-files}을(를) 자동으로 무효화합니다.

`/invalidate` 속성은 콘텐츠가 업데이트될 때 자동으로 무효화되는 문서를 정의합니다.

자동 무효화를 통해 디스패처는 컨텐츠 업데이트 후 캐시된 파일을 삭제하지 않고 다음에 요청하는 경우 해당 파일의 유효성을 확인합니다. 자동 무효화되지 않은 캐시에 있는 문서는 컨텐츠 업데이트가 명시적으로 삭제될 때까지 캐시에 유지됩니다.

자동 무효화는 일반적으로 HTML 페이지에 사용됩니다. HTML 페이지에는 종종 다른 페이지에 대한 링크가 포함되어 있으므로 컨텐츠 업데이트가 페이지에 영향을 주는지 판단하기가 어렵습니다. 컨텐츠가 업데이트될 때 관련 페이지가 모두 무효화되도록 하려면 모든 HTML 페이지를 자동으로 무효화합니다. 다음 구성은 모든 HTML 페이지를 무효화합니다.

```xml
  /invalidate
  {
   /0000  { /glob "*" /type "deny" }
   /0001  { /glob "*.html" /type "allow" }
  }
```

전역 속성에 대한 자세한 내용은 전역 속성의 패턴 디자인](#designing-patterns-for-glob-properties)을 참조하십시오.[

이 구성에서는 `/content/wknd/us/en`이(가) 활성화되면 다음 활동이 발생합니다.

* 패턴 엔이 있는 모든 파일* `/content/wknd/us` 폴더에서 제거됩니다.
* `/content/wknd/us/en./_jcr_content` 폴더가 제거되었습니다.
* `/invalidate` 구성과 일치하는 다른 모든 파일은 즉시 삭제되지 않습니다. 이러한 파일은 다음 요청이 발생하면 삭제됩니다. Adobe 예제 `/content/wknd.html`는 삭제되지 않으며 `/content/wknd.html`가 요청되면 삭제됩니다.

다운로드용으로 자동으로 생성된 PDF 및 ZIP 파일을 제공하는 경우 이러한 파일도 자동으로 무효화해야 할 수 있습니다. 구성 예는 다음과 같습니다.

```xml
/invalidate
  {
   /0000 { /glob "*" /type "deny" }
   /0001 { /glob "*.html" /type "allow" }
   /0002 { /glob "*.zip" /type "allow" }
   /0003 { /glob "*.pdf" /type "allow" }
  }
```

Adobe Analytics와의 AEM 통합은 웹 사이트의 `analytics.sitecatalyst.js` 파일에 구성 데이터를 전달합니다. Dispatcher와 함께 제공되는 예제 `dispatcher.any` 파일에는 이 파일에 대해 다음 무효화 규칙이 포함되어 있습니다.

```xml
{
   /glob "*/analytics.sitecatalyst.js"  /type "allow"
}
```

### 사용자 지정 무효화 스크립트 사용 {#using-custom-invalidation-scripts}

`/invalidateHandler` 속성을 사용하면 Dispatcher가 수신한 각 무효화 요청에 대해 호출되는 스크립트를 정의할 수 있습니다.

다음 인수를 사용하여 호출됩니다.

* 핸들 - 무효화된 컨텐츠 경로
* 작업 - 복제 작업(예: 활성화, 비활성화)
* 작업 범위 - 복제 작업의 범위(`CQ-Action-Scope: ResourceOnly`의 헤더가 전송되지 않는 경우 비어 있음)에 대한 자세한 내용은 [AEM](page-invalidate.md)에서 캐시된 페이지 무효화를 참조하십시오.

다른 응용 프로그램 특정 캐시를 무효화하는 등 다양한 사용 사례를 포함하거나, 페이지 및 문서 루트의 해당 위치가 컨텐츠 경로와 일치하지 않는 경우를 처리하는 데 사용할 수 있습니다.

아래 스크립트 예제는 파일에 대한 각 무효화 요청을 기록합니다.

```xml
/invalidateHandler "/opt/dispatcher/scripts/invalidate.sh"
```

#### 샘플 무효화 처리기 스크립트 {#sample-invalidation-handler-script}

```shell
#!/bin/bash

printf "%-15s: %s %s" $1 $2 $3>> /opt/dispatcher/logs/invalidate.log
```

### 캐시를 플러시할 수 있는 클라이언트 제한 {#limiting-the-clients-that-can-flush-the-cache}

`/allowedClients` 속성은 캐시를 플러시할 수 있는 특정 클라이언트를 정의합니다. 글로빙 패턴은 IP와 일치합니다.

다음 예:

1. 클라이언트 액세스 거부
1. 명시적으로 localhost에 대한 액세스 허용

```xml
/allowedClients
  {
   /0001 { /glob "*.*.*.*"  /type "deny" }
   /0002 { /glob "127.0.0.1" /type "allow" }
  }
```

전역 속성에 대한 자세한 내용은 전역 속성의 패턴 디자인](#designing-patterns-for-glob-properties)을 참조하십시오.[

>[!CAUTION]
>
>`/allowedClients`을 정의하는 것이 좋습니다.
>
>이렇게 하지 않으면 모든 클라이언트가 캐시를 지우는 호출을 실행할 수 있습니다.이 작업을 반복적으로 수행하면 사이트 성능에 심각한 영향을 줄 수 있습니다.

### URL 매개 변수 {#ignoring-url-parameters} 무시

`ignoreUrlParams` 섹션에서는 페이지가 캐시되거나 캐시에서 배달되는지 여부를 결정할 때 무시되는 URL 매개 변수를 정의합니다.

* 요청 URL에 모두 무시된 매개 변수가 포함되어 있으면 페이지가 캐시됩니다.
* 요청 URL에 무시되지 않은 하나 이상의 매개 변수가 포함되어 있으면 페이지가 캐시되지 않습니다.

페이지에 대한 매개 변수가 무시되면 페이지가 처음으로 요청될 때 페이지가 캐시됩니다. 페이지의 후속 요청은 요청에 있는 매개 변수의 값에 관계없이 캐시된 페이지에 제공됩니다.

무시되는 매개 변수를 지정하려면 `ignoreUrlParams` 속성에 글로벌 규칙을 추가합니다.

* 매개 변수를 무시하려면 매개 변수를 허용하는 전역 속성을 만듭니다.
* 페이지가 캐시되지 않도록 하려면 매개 변수를 거부하는 글로벌 속성을 만듭니다.

다음 예에서는 Dispatcher가 `q` 매개 변수를 무시하므로 q 매개 변수를 포함하는 요청 URL이 캐시됩니다.

```xml
/ignoreUrlParams
{
    /0001 { /glob "*" /type "deny" }
    /0002 { /glob "q" /type "allow" }
}
```

다음 HTTP 요청은 `q` 매개 변수가 무시되기 때문에 페이지를 캐시합니다.`ignoreUrlParams`

```xml
GET /mypage.html?q=5
```

다음 HTTP 요청은 `p` 매개 변수가 무시되지 않으므로 페이지가 **로 캐시되지 않도록 합니다.`ignoreUrlParams`**

```xml
GET /mypage.html?q=5&p=4
```

전역 속성에 대한 자세한 내용은 전역 속성의 패턴 디자인](#designing-patterns-for-glob-properties)을 참조하십시오.[

### HTTP 응답 헤더 캐싱 {#caching-http-response-headers}

>[!NOTE]
>
>이 기능은 Dispatcher의 버전 **4.1.11**&#x200B;에서 사용할 수 있습니다.

`/headers` 속성을 사용하면 Dispatcher에서 캐시할 HTTP 헤더 유형을 정의할 수 있습니다. 캐시되지 않은 리소스에 대한 첫 번째 요청에서는 구성된 값 중 하나와 일치하는 모든 헤더가 캐시 파일 옆에 있는 별도의 파일에 저장됩니다. 캐시된 리소스에 대한 후속 요청에서 저장된 헤더가 응답에 추가됩니다.

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
>파일 글로벌 문자는 허용되지 않습니다. 자세한 내용은 전역 속성에 대한 패턴 디자인[을 참조하십시오.](#designing-patterns-for-glob-properties)

>[!NOTE]
>
>AEM에서 ETag 응답 헤더를 저장하고 전달하려면 Dispatcher가 필요합니다.
>
>* `/cache/headers`섹션 내에 헤더 이름을 추가합니다.
>* Dispatcher 관련 섹션에 다음 [Apache 디렉티브](https://httpd.apache.org/docs/2.4/mod/core.html#fileetag)을 추가합니다.

>
>
```xml
>FileETag none
>```

### 발송자 캐시 파일 권한 {#dispatcher-cache-file-permissions}

`mode` 속성은 캐시의 새 디렉토리 및 파일에 적용할 파일 권한을 지정합니다. 이 설정은 호출 프로세스의 `umask`에 의해 제한됩니다. 다음 값 중 하나 이상의 합으로 구성된 합수입니다.

* `0400` 소유자별 읽기 허용
* `0200` 소유자별 쓰기 허용
* `0100` 소유자가 디렉토리에서 검색할 수 있도록 허용합니다.
* `0040` 그룹 구성원이 읽을 수 있도록 허용합니다.
* `0020` 그룹 구성원별 쓰기 허용
* `0010` 그룹 구성원이 디렉토리에서 검색할 수 있도록 허용합니다.
* `0004` 다른 사람의 읽기 허용
* `0002` 다른 사람의 쓰기를 허용합니다.
* `0001` 다른 사람이 디렉토리에서 검색할 수 있도록 허용합니다.

기본값은 `0755`입니다. 이 값을 사용하면 소유자가 읽고, 쓰거나 검색하고, 그룹 및 기타 사용자가 읽고 검색할 수 있습니다.

### {#throttling-stat-file-touching} 수정 .stat 파일

기본 `/invalidate` 속성을 사용하는 모든 활성화는 모든 `.html` 파일을 무효화합니다(경로가 `/invalidate` 섹션과 일치하는 경우). 트래픽이 많은 웹 사이트에서 후속 활동을 수행하면 백 엔드의 cpu 로드가 증가합니다. 이러한 경우 웹 사이트가 반응하도록 &quot;스로틀&quot; `.stat` 파일을 만지는 것이 좋습니다. 이 작업은 `/gracePeriod` 속성을 사용하여 수행할 수 있습니다.

`/gracePeriod` 속성은 마지막 활성화 이후 캐시에서 부실, 자동 무효화된 리소스가 여전히 제공될 수 있는 시간(초)을 정의합니다. 이 속성은 활성화 일괄 처리가 전체 캐시를 반복해서 무효화하는 설정에서 사용할 수 있습니다. 권장 값은 2초입니다.

자세한 내용은 위의 `/invalidate` 및 `/statfileslevel`섹션을 참조하십시오.

### 시간 기반 캐시 무효화 구성 - /enableTTL {#configuring-time-based-cache-invalidation-enablettl}

설정된 경우 `/enableTTL` 속성은 백엔드의 응답 헤더를 평가하고, 이 헤더가 `Cache-Control` 최대 연령 또는 `Expires` 날짜를 포함하고 있는 경우, 캐시 파일 옆에 있는 보조 빈 파일이 만들어지고 수정 시간은 만료 날짜와 같습니다. 캐시된 파일이 수정 시간 이후에 요청되면 백 엔드에서 자동으로 다시 요청됩니다.

>[!NOTE]
>
>이 기능은 Dispatcher의 **4.1.11** 이상 버전에서 사용할 수 있습니다.

## 부하 균형 조정 구성 - /statistics {#configuring-load-balancing-statistics}

`/statistics` 섹션은 Dispatcher가 각 렌더링의 응답성을 평가하는 파일 범주를 정의합니다. 디스패처는 점수를 사용하여 요청을 전송할 렌더링을 결정합니다.

만드는 각 카테고리는 글로벌 패턴을 정의합니다. Dispatcher는 요청된 컨텐츠의 URI를 이러한 패턴과 비교하여 요청된 컨텐츠의 범주를 확인합니다.

* 카테고리 순서는 URI와 비교할 순서를 결정합니다.
* URI와 일치하는 첫 번째 카테고리 패턴은 파일의 카테고리입니다. 더 이상 범주 패턴이 평가되지 않습니다.

Dispatcher는 최대 8개의 통계 카테고리를 지원합니다. 8개 이상의 카테고리를 정의하는 경우 처음 8개만 사용됩니다.

**렌더링 선택**

디스패처가 렌더링된 페이지를 필요로 할 때마다 다음 알고리즘을 사용하여 렌더링을 선택합니다.

1. 요청에 `renderid` 쿠키의 렌더링 이름이 포함되어 있으면 Dispatcher는 해당 렌더링을 사용합니다.
1. 요청에 `renderid` 쿠키가 없으면 Dispatcher가 렌더링 통계를 비교합니다.

   1. 디스패처는 요청 URI의 범주를 결정합니다.
   1. Dispatcher는 해당 카테고리에 대해 가장 낮은 응답 점수가 있는 렌더링을 결정하고 해당 렌더링을 선택합니다.

1. 렌더링을 아직 선택하지 않은 경우 목록에서 첫 번째 렌더링을 사용합니다.

렌더링 카테고리의 점수는 이전 응답 시간과 Dispatcher가 시도하는 실패 및 성공한 연결을 기반으로 합니다. 각 시도에 대해 요청된 URI 범주의 점수가 업데이트됩니다.

>[!NOTE]
>
>부하 균형을 사용하지 않는 경우 이 섹션을 생략할 수 있습니다.

### 통계 범주 정의 {#defining-statistics-categories}

렌더링 선택을 위한 통계를 유지할 각 문서 유형에 대한 범주를 정의합니다. `/statistics` 섹션에는 `/categories` 섹션이 포함되어 있습니다. 카테고리를 정의하려면 다음 형식의 `/categories` 섹션 아래에 줄을 추가하십시오.

`/name { /glob "pattern"}`

`name` 카테고리는 팜과 고유해야 합니다. `pattern`은 Glob 속성](#designing-patterns-for-glob-properties) 패턴 디자인 섹션에 설명되어 있습니다.[

URI의 카테고리를 결정하기 위해 Dispatcher는 일치 항목을 찾을 때까지 URI를 각 카테고리 패턴과 비교합니다. Dispatcher는 목록의 첫 번째 카테고리로 시작하여 순서대로 계속됩니다. 따라서 보다 구체적인 패턴이 있는 카테고리를 먼저 배치하십시오.

예를 들어 Dispatcher의 기본 `dispatcher.any` 파일은 HTML 카테고리와 다른 카테고리를 정의합니다. HTML 카테고리는 보다 구체적이며 먼저 나타납니다.

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

다음 예에는 검색 페이지의 카테고리도 포함됩니다.

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

### Dispatcher 통계에서 서버 비가용성 반영 {#reflecting-server-unavailability-in-dispatcher-statistics}

`/unavailablePenalty` 속성은 렌더링에 연결할 수 없을 때 렌더링 통계에 적용되는 시간(10분의 1 초)을 설정합니다. 디스패처는 요청된 URI와 일치하는 통계 카테고리에 시간을 추가합니다.

예를 들어, AEM이 실행 중이 아니거나(수신 불가능) 네트워크 관련 문제로 인해 지정된 호스트 이름/포트에 대한 TCP/IP 연결을 설정할 수 없을 때 위약금이 적용됩니다.

`/unavailablePenalty` 속성은 `/farm` 섹션(`/statistics` 섹션의 동위)의 직접적인 하위입니다.

`/unavailablePenalty` 속성이 없으면 `"1"`의 값이 사용됩니다.

```xml
/unavailablePenalty "1"
```

## 고정 연결 폴더 식별 - /stickyConnectionsFor {#identifying-a-sticky-connection-folder-stickyconnectionsfor}

`/stickyConnectionsFor` 속성은 고정 문서를 포함하는 하나의 폴더를 정의합니다.URL을 사용하여 액세스합니다. Dispatcher는 이 폴더에 있는 단일 사용자의 모든 요청을 동일한 렌더링 인스턴스로 보냅니다. 고정 연결을 통해 모든 문서에서 세션 데이터가 표시되고 일관되게 유지됩니다. 이 메커니즘은 `renderid` 쿠키를 사용합니다.

다음 예제에서는 /products 폴더에 대한 고정 연결을 정의합니다.

```xml
/stickyConnectionsFor "/products"
```

페이지가 여러 컨텐츠 노드의 컨텐츠로 구성된 경우 컨텐츠의 경로를 나열하는 `/paths` 속성을 포함합니다. 예를 들어 페이지에 `/content/image`, `/content/video` 및 `/var/files/pdfs`의 컨텐츠가 포함되어 있습니다. 다음 구성은 페이지의 모든 컨텐츠에 대해 고정 연결을 활성화합니다.

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

고정 연결이 활성화되면 디스패처 모듈은 `renderid` 쿠키를 설정합니다. 이 쿠키에는 보안을 강화하기 위해 추가되어야 하는 `httponly` 플래그가 없습니다. 이 작업은 `dispatcher.any` 구성 파일의 `/stickyConnections` 노드에서 `httpOnly` 속성을 설정하여 수행할 수 있습니다. 속성 값(`0` 또는 `1`)은 `renderid` 쿠키에 `HttpOnly` 특성이 추가되었는지 여부를 정의합니다. 기본값은 `0`입니다. 즉, 속성이 추가되지 않습니다.

`httponly` 플래그에 대한 자세한 내용은 [이 페이지](https://www.owasp.org/index.php/HttpOnly)를 참조하십시오.

### secure {#secure}

고정 연결이 활성화되면 디스패처 모듈은 `renderid` 쿠키를 설정합니다. 이 쿠키에는 보안을 강화하기 위해 추가되어야 하는 `secure` 플래그가 없습니다. 이 작업은 `dispatcher.any` 구성 파일의 `/stickyConnections` 노드에서 `secure` 속성을 설정하여 수행할 수 있습니다. 속성 값(`0` 또는 `1`)은 `renderid` 쿠키에 `secure` 특성이 추가되었는지 여부를 정의합니다. 기본값은 `0`입니다. 즉, 들어오는 요청이 안전하므로 속성이 **if**&#x200B;이면 추가됩니다. 값이 `1`으로 설정된 경우 들어오는 요청이 안전한지 여부에 관계없이 보안 플래그가 추가됩니다.

## 렌더링 연결 오류 처리 {#handling-render-connection-errors}

렌더링 서버에서 500 오류를 반환하거나 사용할 수 없을 때 디스패처 동작을 구성합니다.

### 상태 확인 페이지 지정 {#specifying-a-health-check-page}

`/health_check` 속성을 사용하여 500 상태 코드가 발생할 때 확인되는 URL을 지정합니다. 이 페이지가 500 상태 코드도 반환하는 경우 인스턴스를 사용할 수 없는 것으로 간주되고 구성 가능한 시간 벌점( `/unavailablePenalty`)이 렌더링에 적용된 후 다시 시도합니다.

```xml
/health_check
  {
  # Page gets contacted when an instance returns a 500
  /url "/health_check.html"
  }
```

### 페이지 재시도 지연 지정 {#specifying-the-page-retry-delay}

`/retryDelay` 속성은 Dispatcher가 팜 렌더와의 연결 시도 간 대기하는 시간(초)을 설정합니다. 각 라운드에 대해 Dispatcher가 렌더로의 연결을 시도하는 최대 수는 팜의 렌더링 수입니다.

`/retryDelay`이(가) 명시적으로 정의되지 않은 경우 디스패처는 `"1"` 값을 사용합니다. 기본값은 대부분의 경우 적절합니다.

```xml
/retryDelay "1"
```

### 재시도 횟수 구성 {#configuring-the-number-of-retries}

`/numberOfRetries` 속성은 Dispatcher가 렌더에서 수행하는 최대 연결 시도 횟수를 설정합니다. Dispatcher가 이 재시도 횟수 후 렌더링에 성공적으로 연결할 수 없는 경우 실패한 응답을 반환합니다.

각 라운드에 대해 Dispatcher가 렌더로의 연결을 시도하는 최대 수는 팜의 렌더링 수입니다. 따라서 Dispatcher가 연결을 시도하는 최대 수는 ( `/numberOfRetries`) x(렌더링 수)입니다.

값이 명시적으로 정의되지 않은 경우 기본값은 `5`입니다.

```xml
/numberOfRetries "5"
```

### 페일오버 메커니즘 사용 {#using-the-failover-mechanism}

원래 요청이 실패할 때 Dispatcher 팜의 장애 조치(failover) 메커니즘을 사용하여 요청을 다른 렌더링으로 다시 보냅니다. 장애 조치(failover)가 활성화되어 있으면 Dispatcher의 비헤이비어는 다음과 같습니다.

* 렌더링을 요청하는 경우 HTTP 상태 503(UNAVAILABLE)가 반환되면 디스패처는 요청을 다른 렌더링으로 보냅니다.
* 렌더링에 대한 요청이 HTTP 상태 50x(503이 아님)를 반환하는 경우 Dispatcher는 `health_check` 속성에 대해 구성된 페이지에 대한 요청을 보냅니다.
   * 상태 검사가 500(INTERNAL_SERVER_ERROR)을 반환하는 경우 Dispatcher는 원래 요청을 다른 렌더링으로 보냅니다.
   * 검사 결과 HTTP 상태 200이 반환되면 Dispatcher는 초기 HTTP 500 오류를 클라이언트에 반환합니다.

장애 조치를 활성화하려면 팜(또는 웹 사이트)에 다음 줄을 추가하십시오.

```xml
/failover "1"
```

>[!NOTE]
>
>본문이 포함된 HTTP 요청을 다시 시도하려면 Dispatcher가 실제 컨텐츠를 스풀링하기 전에 `Expect: 100-continue` 요청 헤더를 렌더링으로 보냅니다. CQSE가 있는 CQ 5.5에서 100(계속) 또는 오류 코드로 즉시 응답합니다. 다른 서블릿 컨테이너도 이 옵션을 지원해야 합니다.

## 중단 오류 무시 - /ignoreINTR {#ignoring-interruption-errors-ignoreeintr}

>[!CAUTION]
>
>이 옵션은 일반적으로 필요하지 않습니다. 다음 로그 메시지만 표시될 때만 이 메시지를 사용해야 합니다.
>
>`Error while reading response: Interrupted system call`

시스템 호출의 개체가 NFS를 통해 액세스하는 원격 시스템에 있는 경우 모든 파일 시스템 지향 시스템 호출이 `EINTR`에 중단될 수 있습니다. 이러한 시스템 호출이 시간 초과되거나 중단될 수 있는지 여부는 로컬 시스템에 기본 파일 시스템이 마운트된 방식을 기반으로 합니다.

인스턴스에 이러한 구성이 있고 로그에 다음 메시지가 포함된 경우 `/ignoreEINTR` 매개 변수를 사용하십시오.

`Error while reading response: Interrupted system call`

내부적으로 디스패처는 다음과 같이 표시될 수 있는 루프를 사용하여 원격 서버(즉, AEM)의 응답을 읽습니다.

```text
while (response not finished) {  
read more data  
}
```

이러한 메시지는 `EINTR`이 &quot; `read more data`&quot; 섹션에서 발생할 때 생성되며 데이터를 받기 전에 신호의 수신으로 인해 발생할 수 있습니다.

이러한 인터럽트를 무시하려면 `/farms` 앞에 `dispatcher.any` 매개 변수를 추가할 수 있습니다.

`/ignoreEINTR "1"`

`/ignoreEINTR`을 `"1"`으로 설정하면 Dispatcher가 전체 응답을 읽을 때까지 데이터를 계속 읽으려고 합니다. 기본값은 `0`이며 옵션을 비활성화합니다.

## 전역 속성에 대한 패턴 디자인 {#designing-patterns-for-glob-properties}

Dispatcher 구성 파일의 여러 섹션에서 클라이언트 요청에 대한 선택 기준으로 `glob` 속성을 사용합니다. `glob` 속성의 값은 Dispatcher가 요청한 리소스의 경로 또는 클라이언트의 IP 주소와 같은 요청의 측면과 비교하는 패턴입니다. 예를 들어 `/filter` 섹션의 항목은 `glob` 패턴을 사용하여 Dispatcher가 작업하거나 거부한 페이지의 경로를 식별합니다.

`glob` 값에는 패턴을 정의하는 와일드카드 문자 및 영숫자 문자가 포함될 수 있습니다.

| 와일드카드 문자 | 설명 | 예 |
|--- |--- |--- |
| `*` | 문자열에 있는 모든 문자의 0개 이상의 인접한 인스턴스와 일치합니다. 일치의 최종 문자는 다음 상황 중 하나에 의해 결정됩니다.<br/>문자열의 문자가 패턴의 다음 문자와 일치하며 패턴 문자에는 다음 특성이 있습니다.<br/><ul><li>* 아님</li><li>아니요?</li><li>문자(공백 포함) 또는 문자 클래스입니다.</li><li>패턴의 끝에 도달했습니다.</li></ul>문자 클래스 안에선 문자 그대로 해석됩니다. | `*/geo*` 노드  `/content/geometrixx` 및  `/content/geometrixx-outdoors` 노드 아래의 모든 페이지와 일치합니다. 다음 HTTP 요청은 글로벌 패턴과 일치합니다.<br/><ul><li>`"GET /content/geometrixx/en.html"`</li><li>`"GET /content/geometrixx-outdoors/en.html"` </li></ul><br/> `*outdoors/*` <br/>노드 아래의 모든 페이지와  `/content/geometrixx-outdoors` 일치합니다. 예를 들어 다음 HTTP 요청은 글로벌 패턴과 일치합니다.<br/><ul><li>`"GET /content/geometrixx-outdoors/en.html"`</li></ul> |
| `?` | 모든 단일 문자와 일치합니다. 외부 문자 클래스를 사용하십시오. 문자 클래스 내에서 이 문자는 문자 그대로 해석됩니다. | `*outdoors/??/*`<br/> geometrixx-outdoors 사이트의 모든 언어에 대한 페이지와 일치합니다. 예를 들어 다음 HTTP 요청은 글로벌 패턴과 일치합니다.<br/><ul><li>`"GET /content/geometrixx-outdoors/en/men.html"`</li></ul><br/>다음 요청이 글로벌 패턴과 일치하지 않습니다.  <br/><ul><li>&quot;GET /content/geometrixx-outdoors/en.html&quot;</li></ul> |
| `[ and ]` | 문자 클래스의 시작과 끝을 나타냅니다. 문자 클래스에는 하나 이상의 문자 범위와 단일 문자가 포함될 수 있습니다.<br/>대상 문자가 문자 클래스 또는 정의된 범위 내의 문자와 일치하는 경우 일치합니다.<br/>닫는 괄호가 포함되지 않으면 패턴에서 일치하는 항목이 만들어지지 않습니다. | `*[o]men.html*`<br/> 다음 HTTP 요청과 일치합니다.<br/><ul><li>`"GET /content/geometrixx-outdoors/en/women.html"`</li></ul><br/>다음 HTTP 요청과 일치하지 않습니다.<br/><ul><li>`"GET /content/geometrixx-outdoors/en/men.html"`</li></ul><br/> `*[o/]men.html*` <br/>다음 HTTP 요청과 일치합니다.  <br/><ul><li>`"GET /content/geometrixx-outdoors/en/women.html"`</li><li>`"GET /content/geometrixx-outdoors/en/men.html"`</li></ul> |
| `-` | 문자 범위를 나타냅니다. 문자 클래스에 사용할 경우  문자 클래스 밖에서 이 문자는 문자 그대로 해석됩니다. | `*[m-p]men.html*` 다음 HTTP 요청과 일치합니다.  <br/><ul><li>`"GET /content/geometrixx-outdoors/en/women.html"`</li></ul>다음 HTTP 요청과 일치하지 않습니다.<br/><ul><li>`"GET /content/geometrixx-outdoors/en/men.html"`</li></ul> |
| `!` | 다음에 나오는 문자 또는 문자 클래스를 무효화합니다. 문자 클래스 내의 문자 및 문자 범위를 무효화하는 경우에만 사용하십시오. `^ wildcard`에 해당합니다. <br/>문자 클래스 밖에서 이 문자는 문자 그대로 해석됩니다. | `*[!o]men.html*`<br/> 다음 HTTP 요청과 일치합니다.  <br/><ul><li>`"GET /content/geometrixx-outdoors/en/men.html"`</li></ul><br/>다음 HTTP 요청과 일치하지 않습니다.  <br/><ul><li>`"GET /content/geometrixx-outdoors/en/women.html"`</li></ul><br/>`*[!o!/]men.html*`<br/> 다음 HTTP 요청과 일치하지 않습니다.<br/><ul><li>`"GET /content/geometrixx-outdoors/en/women.html"` 또는 `"GET /content/geometrixx-outdoors/en/men. html"`</li></ul> |
| `^` | 다음에 나오는 문자 또는 문자 범위를 무효화합니다. 문자 클래스 내의 문자 및 문자 범위만 무효화하는 데 사용합니다. `!` 와일드카드 문자와 같습니다. <br/>문자 클래스 밖에서 이 문자는 문자 그대로 해석됩니다. | `!` 와일드카드 문자의 예는 예제 패턴의 `!` 문자를 `^` 문자로 대체하여 적용됩니다. |


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

## 로깅 {#logging}

웹 서버 구성에서 다음을 설정할 수 있습니다.

* Dispatcher 로그 파일의 위치입니다.
* 로그 수준.

자세한 내용은 웹 서버 설명서 및 Dispatcher 인스턴스의 Readme 파일을 참조하십시오.

**Apache Rotated / Pipped Logs**

**Apache** 웹 서버를 사용하는 경우 회전 및/또는 파이프 로그에 표준 기능을 사용할 수 있습니다. 예: 단축된 로그 사용:

`DispatcherLog "| /usr/apache/bin/rotatelogs logs/dispatcher.log%Y%m%d 604800"`

자동으로 회전됩니다.

* 디스패처 로그 파일;확장자(`logs/dispatcher.log%Y%m%d`)에 타임스탬프가 있습니다.
* 매주(60 x 60 x 24 x 7 = 604800초).

로그 순환 및 파이프라인 로그에 대한 Apache 웹 서버 설명서를 참조하십시오.예: [Apache 2.4](https://httpd.apache.org/docs/2.4/logs.html)

>[!NOTE]
>
>설치 시 기본 로그 수준이 높음(예: 레벨 3 = 디버그)이므로 디스패처가 모든 오류 및 경고를 기록합니다. 이것은 초기 단계에서 매우 유용합니다.
>
>그러나 이 경우 추가적인 리소스가 필요하므로 Dispatcher가 사용자 요구 사항&#x200B;*에 따라 원활하게 작동하는 경우 로그 수준을 낮출 수 있습니다(그래야).*

### 추적 로깅 {#trace-logging}

Dispatcher의 기타 향상된 기능 중 버전 4.2.0에서는 추적 로깅도 도입되었습니다.

로그 파일에 추가 정보를 표시하는 디버그 로깅보다 높은 수준입니다. 다음에 대한 로깅을 추가합니다.

* 전달된 헤더의 값;
* 특정 작업에 적용되는 규칙입니다.

웹 서버에서 로그 수준을 `4`으로 설정하여 추적 로깅을 활성화할 수 있습니다.

다음은 추적이 활성화된 로그의 예입니다.

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

또한 차단 규칙과 일치하는 파일이 요청되면 이벤트가 기록됩니다.

```xml
[Thu Mar 03 14:42:45 2016] [T] [11831] 'GET /content.infinity.json HTTP/1.1' was blocked because of /0082
```

## 기본 작업 확인 {#confirming-basic-operation}

웹 서버의 기본 작동 및 상호 작용을 확인하려면 Dispatcher 및 AEM 인스턴스를 다음 단계를 사용할 수 있습니다.

1. `loglevel`을 `3`로 설정합니다.

1. 웹 서버 시작;디스패처가 시작됩니다.
1. AEM 인스턴스를 시작합니다.
1. 웹 서버 및 디스패처에 대한 로그 및 오류 파일을 확인합니다.
   * 웹 서버에 따라 다음과 같은 메시지가 표시됩니다.
      * `[Thu May 30 05:16:36 2002] [notice] Apache/2.0.50 (Unix) configured` 및
      * `[Fri Jan 19 17:22:16 2001] [I] [19096] Dispatcher initialized (build XXXX)`

1. 웹 서버를 통해 웹 사이트를 서핑합니다. 컨텐츠가 필요에 따라 표시되는지 확인합니다.\
   예를 들어 AEM이 포트 `4502`에서 실행되고 `80`의 웹 서버가 두 가지 모두를 사용하여 웹 사이트 콘솔에 액세스하는 로컬 설치에서
   * `https://localhost:4502/libs/wcm/core/content/siteadmin.html`
   * `https://localhost:80/libs/wcm/core/content/siteadmin.html`
   * 결과는 동일해야 한다. 동일한 메커니즘으로 다른 페이지에 대한 액세스를 확인합니다.

1. 캐시 디렉토리가 채워지는지 확인합니다.
1. 캐시를 올바르게 플러시하고 있는지 확인하려면 페이지를 활성화합니다.
1. 모든 것이 올바르게 작동하는 경우 `loglevel`을 `0`로 줄일 수 있습니다.

## 여러 Dispatcher 사용 {#using-multiple-dispatchers}

복잡한 설정에서 여러 Dispatcher를 사용할 수 있습니다. 예를 들어 다음을 사용할 수 있습니다.

* 인트라넷에 웹 사이트를 게시할 하나의 Dispatcher
* 다른 주소와 보안 설정이 다른 두 번째 Dispatcher가 동일한 콘텐츠를 인터넷에 게시합니다.

이러한 경우 각 요청이 하나의 Dispatcher만 통과하는지 확인하십시오. Dispatcher는 다른 Dispatcher의 요청을 처리하지 않습니다. 따라서 Dispatcher가 모두 AEM 웹 사이트에 직접 액세스하도록 하십시오.

## 디버깅 {#debugging}

헤더 `X-Dispatcher-Info`을(를) 요청에 추가하면 Dispatcher는 대상이 캐시되었는지, 캐시되어 반환되었는지, 아니면 전혀 캐시할 수 없는지 확인합니다. 응답 헤더 `X-Cache-Info`에 이 정보가 읽을 수 있는 형식으로 포함되어 있습니다. 이러한 응답 헤더를 사용하여 디스패처가 캐시한 응답과 관련된 문제를 디버깅할 수 있습니다.

이 기능은 기본적으로 활성화되어 있지 않으므로 응답 헤더 `X-Cache-Info`을(를) 포함하려면 팜에 다음 항목이 포함되어야 합니다.

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

또한 `X-Dispatcher-Info` 헤더에는 값이 필요하지 않지만 테스트를 위해 `curl`을(를) 사용하는 경우 헤더를 전송하려면 다음과 같은 값을 제공해야 합니다.

```xml
curl -v -H "X-Dispatcher-Info: true" https://localhost/content/wknd/us/en.html
```

다음은 `X-Dispatcher-Info`이(가) 반환하는 응답 헤더를 포함하는 목록입니다.

* **캐시됨**\
   대상 파일은 캐시에 포함되어 있으며 디스패처는 이를 전달하는 것이 유효하다고 판단했습니다.
* **캐싱**\
   대상 파일이 캐시에 포함되어 있지 않으며 디스패처는 출력을 캐시하여 전달하는 것이 유효하다고 판단했습니다.
* **캐싱:stat 파일이 보다**
최신입니다. 대상 파일이 캐시에 포함되어 있지만, 더 최근 stat 파일에 의해 무효화됩니다. 디스패처가 대상 파일을 삭제하고 출력에서 다시 만들어 전달합니다.
* **취소할 수 없음:문서**
루트 없음팜의 구성에 문서 루트(구성 요소)가 없습니다. 
`cache.docroot`).
* **취소할 수 없음:캐시 파일 경로가 너무 깁니다.**\
   대상 파일(문서 루트 및 URL 파일의 연결)이 시스템에서 사용 가능한 가장 긴 파일 이름을 초과합니다.
* **취소할 수 없음:임시 파일 경로가 너무 깁니다.**\
   임시 파일 이름 템플릿이 시스템에서 가능한 가장 긴 파일 이름을 초과합니다. 디스패처는 캐시된 파일을 실제로 만들거나 덮어쓰기 전에 임시 파일을 먼저 만듭니다. 임시 파일 이름은 대상 파일 이름이며 여기에 `_YYYYXXXXXX`이라는 문자가 추가됩니다. 여기서 `Y` 및 `X`는 고유한 이름을 만들기 위해 대체됩니다.
* **취소할 수 없음:요청 URL에 확장자가 없습니다.**\
   요청 URL에 확장자가 없거나 파일 확장자 다음의 경로가 있습니다. 예:`/test.html/a/path`.
* **취소할 수 없음:요청이 GET 또는**
HEADT가 아님HTTP 메서드는 GET도 HEAD도 아닙니다. 디스패처는 출력에 캐시되지 않아야 하는 동적 데이터가 포함된다고 가정합니다.
* **취소할 수 없음:요청에 쿼리 문자열이 포함됨**\
   요청에 쿼리 문자열이 포함되었습니다. 디스패처는 출력이 지정된 쿼리 문자열에 따라 달라지므로 캐시하지 않는다고 가정합니다.
* **취소할 수 없음:세션 관리자가 인증하지 않았습니다.**\
   팜의 캐시는 세션 관리자(구성에 `sessionmanagement` 노드가 포함되어 있음)에 의해 제어되며 요청에 해당 인증 정보가 포함되어 있지 않습니다.
* **취소할 수 없음:권한 포함**\
   팜은 출력( `allowAuthorized 0`)을 캐시할 수 없으며 요청에 인증 정보가 포함되어 있습니다.
* **취소할 수 없음:target은 디렉토리입니다.**\
   대상 파일은 디렉토리입니다. 이것은 URL과 일부 하위 URL이 모두 캐시 가능한 출력을 포함하는 개념적 실수를 가리키는 것일 수 있습니다. 예를 들어 `/test.html/a/file.ext`에 대한 요청이 먼저 오고 캐시 가능한 출력을 포함하는 경우 디스패처는 이후 요청의 출력을 `/test.html`에 캐시할 수 없습니다.
* **취소할 수 없음:요청 URL에 후행 슬래시가 있습니다.**\
   요청 URL에 후행 슬래시가 있습니다.
* **취소할 수 없음:요청 URL이 캐시 규칙에 없습니다.**\
   팜의 캐시 규칙은 일부 요청 URL의 출력을 명시적으로 캐시하지 않습니다.
* **취소할 수 없음:권한 검사기가 액세스를 거부함**\
   팜의 인증 검사기가 캐시된 파일에 대한 액세스를 거부했습니다.
* **취소할 수 없음:세션이**
잘못되었습니다. 팜의 캐시는 세션 관리자(구성이  `sessionmanagement` 노드를 포함함)에 의해 제어되며 사용자의 세션이 유효하지 않거나 더 이상 유효하지 않습니다.
* **취소할 수 없음:응답에`no_cache`**
원격 서버가 
`Dispatcher: no_cache` 헤더. 디스패처가 출력을 캐시하도록 합니다.
* **취소할 수 없음:응답 컨텐츠 길이가**
0입니다. 응답의 컨텐츠 길이는 0입니다.디스패처는 길이가 0인 파일을 만들지 않습니다.
