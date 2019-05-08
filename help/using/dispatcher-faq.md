---
title: Dispatcher 주요 문제
seo-title: AEM Dispatcher의 주요 문제
description: AEM Dispatcher의 주요 문제
seo-description: Adobe AEM Dispatcher의 주요 문제
translation-type: tm+mt
source-git-commit: f35c79b487454059062aca6a7c989d5ab2afaf7b

---


# AEM Dispatcher 주요 문제 FAQ

![Dispatcher 구성](assets/CQDispatcher_workflow_v2.png)

## 소개

### 디스패처 소개

Dispatcher는 빠르고 다이내믹한 웹 저작 환경을 구현하는 데 도움이 되는 Adobe Experience Manager의 캐싱 및/또는 로드 밸런싱 툴입니다. 캐싱의 경우, 디스패처는 가능한 많은 정적 웹 사이트 컨텐츠를 저장 (또는 &quot;캐싱&quot;) 하고 가능한 한 드물게 웹 사이트의 레이아웃 엔진에 액세스하는 목적을 가지고 Apache와 같은 HTTP 서버의 일부로 작동합니다. 로드 밸런싱 역할에서 디스패처는 다른 AEM 인스턴스 (렌더링) 에 사용자 요청 (로드) 를 배포합니다.

캐싱의 경우 디스패처 모듈은 정적 컨텐츠를 제공하는 웹 서버의 기능을 사용합니다. Dispatcher는 캐시된 문서를 웹 서버의 문서 루트에 저장합니다.

### 디스패처는 어떤 방식으로 캐싱을 수행합니까?

디스패처는 웹 서버의 기능을 사용하여 정적 컨텐츠를 제공합니다. 디스패처는 캐시된 문서를 웹 서버의 문서 루트에 저장합니다. 디스패처에는 웹 사이트를 변경할 때 캐시 컨텐츠를 업데이트하는 두 가지 기본 방법이 있습니다.

* **컨텐츠 업데이트는** 변경된 페이지와 직접 연관된 파일을 제거합니다.
* **자동 무효화는** 업데이트 후 오래된 날짜가 될 수 있는 캐시 부분을 자동으로 무효화합니다. 예를 들어 관련 페이지를 삭제하지 않고 연관성 있는 페이지에 효과적으로 플래그를 지정합니다.

### 로드 밸런싱의 이점은 무엇입니까?

부하 균형 조정은 여러 AEM 인스턴스에서 사용자 요청 (로드) 를 배포합니다. 다음 목록은 로드 균형 조정에 대한 이점을 설명합니다.

* **처리 능력 향상**: 실제로는 Dispatcher가 AEM의 여러 인스턴스 사이에 문서 요청을 공유한다는 것을 의미합니다. 각 인스턴스마다 처리할 문서가 적으므로 응답 시간이 단축됩니다. 디스패처는 각 문서 카테고리에 대한 내부 통계를 유지하므로, 로드를 예측하고 쿼리를 효율적으로 배포할 수 있습니다.
* **Increased fail-safe coverage**: If the Dispatcher does not receive responses from an instance, it will automatically relay requests to one of the other instance(s). 따라서 인스턴스를 사용할 수 없을 경우, 유일한 효과는 컴퓨팅 성능에 비례하는 사이트 속도입니다.

>[!NOTE]
>
>자세한 내용은 [Dispatcher 개요 페이지를 참조하십시오.](dispatcher.md)

## 설치 및 구성

### Dispatcher 모듈은 어디에서 다운로드할 수 있습니까?

[Dispatcher 릴리스 정보](release-notes.md) 페이지에서 최신 Dispatcher 모듈을 다운로드할 수 있습니다.

### Dispatcher 모듈은 어떻게 설치합니까?

Dispatcher 페이지 [설치](dispatcher-install.md) 참조

### Dispatcher 모듈은 어떻게 구성합니까?

[Dispatcher](dispatcher-configuration.md) 페이지 구성을 참조하십시오.

### 작성자 인스턴스용 Dispatcher는 어떻게 구성합니까?

자세한 내용은 작성자 인스턴스와 함께 Dispatcher [사용을](dispatcher.md#using-a-dispatcher-with-an-author-server) 참조하십시오.

### 여러 도메인으로 디스패처를 구성하려면 어떻게 해야 합니까?

도메인이 다음 조건을 충족하면 여러 도메인이 있는 CQ 디스패처를 구성할 수 있습니다.

* 두 도메인 모두에 대한 웹 컨텐츠가 단일 AEM 저장소에 저장됩니다.
* Dispatcher 캐시의 파일은 각 도메인에 대해 개별적으로 무효화할 수 있습니다.

자세한 내용은 [여러 도메인의](dispatcher-domains.md) Dispatcher 사용을 참조하십시오.

### 사용자의 모든 요청이 동일한 게시 인스턴스로 라우팅되도록 디스패처를 구성하려면 어떻게 해야 합니까?

[고정 연결](dispatcher-configuration.md#identifying-a-sticky-connection-folder-stickyconnectionsfor) 기능을 사용하면 사용자의 모든 문서가 동일한 AEM 인스턴스에서 처리될 수 있습니다. 이 기능은 개인화된 페이지 및 세션 데이터를 사용하는 경우에 중요합니다. 데이터는 인스턴스에 저장됩니다. 따라서 같은 사용자의 후속 요청이 해당 인스턴스로 돌아가야 하거나 데이터가 유실됩니다.

고정 연결은 Dispatcher의 요청을 최적화하는 기능을 제한하므로 필요한 경우에만 이 방법을 사용해야 합니다. &quot; 고정 &quot;문서가 들어 있는 폴더를 지정하여 해당 폴더의 모든 문서가 사용자에 대해 동일한 인스턴스에서 처리되도록 할 수 있습니다.

### 고정 연결과 캐싱을 동시에 사용할 수 있습니까?

고정 연결을 사용하는 대부분의 페이지에 대해 캐싱을 해제해야 합니다. 그렇지 않으면 세션 콘텐트에 관계없이 모든 사용자에게 동일한 페이지 인스턴스가 표시됩니다.

일부 애플리케이션의 경우 고정 연결과 캐싱을 모두 사용할 수 있습니다. 예를 들어 세션에 데이터를 쓰는 양식을 표시하는 경우 고정 연결과 캐싱을 동시에 사용할 수 있습니다.

### 디스패처 및 AEM 게시 인스턴스가 동일한 물리적 컴퓨터에 상주할 수 있습니까?

예. 시스템 성능이 매우 강력합니다. 그러나 다른 컴퓨터에서 Dispatcher 및 AEM 게시 인스턴스를 설정하는 것이 좋습니다.

일반적으로 게시 인스턴스는 방화벽 내부에 있으며, 디스패처는 DMZ에 상주합니다. 동일한 물리적 컴퓨터에 게시 인스턴스와 디스패처를 모두 사용하려는 경우 방화벽 설정을 통해 외부 네트워크에서 게시 인스턴스에 직접 액세스할 수 없도록 해야 합니다.

### 특정 확장명의 파일만 캐시할 수 있습니까?

예. 예를 들어 GIF 파일만 캐시하려는 경우 Dispatcher의 Cache 섹션에서 *. gif를 지정합니다. 모든 구성 파일의 경우

### 캐시에서 파일을 삭제하려면 어떻게 해야 합니까?

HTTP 요청을 사용하여 캐시에서 파일을 삭제할 수 있습니다. HTTP 요청이 수신되면, 디스패처가 캐시에서 파일을 삭제합니다. Dispatcher는 해당 페이지에 대한 클라이언트 요청을 수신할 때에만 파일을 다시 캐시합니다. 이 방법으로 캐시된 파일을 삭제하는 것은 동일한 페이지에 대한 동시 요청을 받을 가능성이 없는 웹 사이트에 적합합니다.

HTTP 요청에는 다음 구문이 있습니다.

```
POST /dispatcher/invalidate.cache HTTP/1.1
CQ-Action: Activate
CQ-Handle: path-pattern
Content-Length: 0
```

Dispatcher는 CQ 핸들 헤더의 값과 일치하는 이름이 있는 캐시된 파일과 폴더를 삭제합니다. 예를 들어의 `/content/geomtrixx-outdoors/en` cq-handle는 다음 항목을 찾습니다.

Geometrixx-Outdoors 디렉토리
(있는 경우) 에 이름이 지정된 모든 파일 (모든 파일 확장자) 는 en 디렉토리 `_jcr_content` 아래에서 이름이 지정된 디렉토리 (, 있는 경우, 페이지의 하위 노드의 캐시된 렌더링 포함)
를 포함합니다. 디렉토리 en는 is가 `CQ-Action``Delete` 있는 `Deactivate`경우에만 삭제됩니다.

이 주제에 대한 자세한 내용은 수동으로 Dispatcher 캐시 무효화를 참조하십시오 [](page-invalidate.md).

### 권한 감지 캐싱을 구현하려면 어떻게 해야 합니까?

보안 컨텐츠 [캐싱](permissions-cache.md) 페이지를 참조하십시오.

### Dispatcher와 CQ 인스턴스 간의 통신을 안전하게 하려면 어떻게 해야 합니까?

[Dispatcher 보안 검사 목록](security-checklist.md) 및 [AEM 보안 검사 목록](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security-checklist.html) 페이지를 참조하십시오.

### Dispatcher issue `jcr:content` changed to `jcr%3acontent`

**질문**: 최근 Dispatcher 수준에서 일부 데이터 양식 CQ 리포지토리를 가져오고 있는 AJAX 호출에서 인코딩되어 잘못된 `jcr:content` 결과 세트가 `jcr%3acontent` 생성되는 문제가 해결되었습니다.

**답변**: `ResourceResolver.map()` Use method to be get and issue to be used and also get request from and also to resolve the caching issue with dispatcher. map () 메서드는 `:` 콜론을 밑줄로 인코딩하고 resolve () 메서드는 이 콜론을 sling jcr 읽기 가능 포맷으로 다시 디코딩합니다. map () 메서드를 사용하여 ajax 호출에 사용되는 URL를 생성해야 합니다.

자세히 보기: [https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html#namespace-mangling](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html#namespace-mangling)

## 디스패처 초기화

### 게시 인스턴스에서 Dispatcher Flush 에이전트를 구성하려면 어떻게 해야 합니까?

[복제](https://helpx.adobe.com/content/help/en/experience-manager/6-4/sites/deploying/using/replication.html#ConfiguringyourReplicationAgents) 페이지를 참조하십시오.

### Dispatcher 플러시 문제를 해결하려면 어떻게 해야 합니까?

[다음 질문에 답변하는 문제 해결 문서를](https://helpx.adobe.com/content/help/en/experience-manager/kb/troubleshooting-dispatcher-flushing-issues.html) 참조하십시오.

* 컨텐츠가 디스패처 캐시에 저장되지 않는 상황을 디버깅하려면 어떻게 해야 합니까?
* 캐시 파일이 업데이트되지 않는 상황을 디버깅하려면 어떻게 해야 합니까?
* Dispatcher 플러싱과 관련된 아무 것도 작동하지 않는 상황을 디버깅하려면 어떻게 해야 합니까?

삭제 작업으로 인해 디스패처가 플러시되는 경우, Sensei Martin 이 이 커뮤니티 블로그 게시물에서 해결책을 [사용하십시오](https://mkalugin-cq.blogspot.in/2012/04/i-have-been-working-on-following.html).

### 디스패처 캐시에서 DAM 에셋을 플러시하려면 어떻게 해야 합니까?

&quot; 체인 복제 &quot;기능을 사용할 수 있습니다. 이 기능이 활성화되면 작성자로부터 복제를 받으면 Dispatcher Flush 에이전트가 플러시 요청을 보냅니다.

활성화하려면 다음을 수행하십시오.

1. [게시 시 여기에](page-invalidate.md#invalidating-dispatcher-cache-from-a-publishing-instance) 있는 단계에 따라 에이전트를 플러시합니다.
1. 각 에이전트 구성으로 이동하고 **트리거** 탭에서 수신 **상자의 확인란을** 선택합니다.

## 기타

디스패처는 문서가 최신 버전인지 어떻게 알 수 있습니까?
문서가 최신 버전인지 여부를 확인하기 위해 디스패처는 다음 작업을 수행합니다.

문서가 자동 무효화의 대상이 되는지 확인합니다. 그렇지 않으면 문서가 최신 상태로 간주됩니다.
문서가 자동 무효화를 위해 구성된 경우 디스패처는 사용 가능한 마지막 변경 내용보다 이전 버전인지 또는 오래된 버전인지 확인합니다. 이전 버전인 경우, Dispatcher는 AEM 인스턴스에서 현재 버전을 요청하고, 캐시에 있는 버전을 대체합니다.

### Dispatcher는 문서를 어떻게 반환합니까?

Dispatcher [구성](dispatcher-configuration.md) 파일을 사용하여 Dispatcher에서 문서를 캐시하는지 여부를 정의할 `dispatcher.any`수 있습니다. Dispatcher는 캐시 가능한 문서 목록에 대한 요청을 확인합니다. 문서가 이 목록에 없으면 Dispatcher는 AEM 인스턴스에서 문서를 요청합니다.

속성은 `/rules` 문서 경로에 따라 캐시되는 문서를 제어합니다. 이 `/rules` 속성에 상관없이 Dispatcher는 다음 상황에서 문서를 캐시하지 않습니다.

* 요청 URI에 물음표가 들어 `(?)`있는 경우
* 이것은 일반적으로 검색 결과 (캐시할 필요가 없는 검색 결과) 를 나타냅니다.
* 파일 확장자가 없습니다.
* 웹 서버는 확장자가 있어야 문서 유형 (MIME-type) 를 확인할 수 있습니다.
* 인증 헤더가 설정됨 (구성 가능)
* AEM 인스턴스가 다음 헤더로 응답하는 경우:
   * 캐시 없음
   * no-store
   * 다시 유효성 검사 필요

디스패처는 캐시된 파일을 정적 웹 사이트의 일부인 것처럼 웹 서버에 저장합니다. 사용자가 캐시된 문서를 요청하면 디스패처는 문서가 웹 서버의 파일 시스템에 있는지 확인합니다. 그럴 경우, Dispatcher는 문서를 반환합니다. 그렇지 않은 경우, Dispatcher는 AEM 인스턴스에서 문서를 요청합니다.

>[!NOTE]
>
>GET 또는 HEAD (HTTP 헤더 경우) 메서드는 디스패처가 캐시할 수 있습니다. 응답 헤더 캐싱에 대한 자세한 내용은 HTTP 응답 헤더 [캐싱](dispatcher-configuration.md#caching-http-response-headers) 섹션을 참조하십시오.

### 하나의 설정에서 여러 배송을 구현할 수 있습니까?

예. 이러한 경우, 두 디스패처가 AEM 웹 사이트에 직접 액세스할 수 있는지 확인하십시오. 디스패처는 다른 디스패처에서 들어오는 요청을 처리할 수 없습니다.

