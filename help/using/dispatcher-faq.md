---
title: 발송자 주요 문제
seo-title: AEM Dispatcher의 주요 문제
description: AEM Dispatcher의 주요 문제
seo-description: Adobe AEM Dispatcher의 주요 문제
translation-type: tm+mt
source-git-commit: 76cffbfb616cd5601aed36b7076f67a2faf3ed3b

---


# AEM Dispatcher 주요 문제 FAQ

![디스패처 구성](assets/CQDispatcher_workflow_v2.png)

## 소개

### 디스패처란?

Dispatcher는 Adobe Experience Manager의 캐싱 및/또는 로드 밸런싱 툴로 빠르고 동적인 웹 저작 환경을 구현할 수 있습니다. 캐싱의 경우 Dispatcher는 Apache와 같은 HTTP 서버의 일부로 작동하여 가능한 많은 정적 웹 사이트 컨텐츠를 저장(또는 "캐싱")하고 웹 사이트의 레이아웃 엔진에 가끔 액세스하는 것을 목표로 합니다. 부하 균형 조정 역할에서 디스패처는 다양한 AEM 인스턴스(렌더링)에 사용자 요청(로드)을 배포합니다.

캐싱의 경우 Dispatcher 모듈은 웹 서버의 정적 컨텐츠 제공 기능을 사용합니다. 디스패처는 캐시된 문서를 웹 서버의 문서 루트에 배치합니다.

### Dispatcher는 어떻게 캐싱을 수행합니까?

Dispatcher는 웹 서버의 정적 컨텐츠 제공 기능을 사용합니다. Dispatcher는 캐시된 문서를 웹 서버의 문서 루트에 저장합니다. Dispatcher에는 웹 사이트를 변경할 때 캐시 컨텐츠를 업데이트하는 두 가지 기본 방법이 있습니다.

* **컨텐츠** 업데이트는 변경된 페이지와 해당 페이지와 직접 연결된 파일을 제거합니다.
* **자동 무효화는** 업데이트 후 오래된 캐시 부분을 자동으로 무효화합니다. 예를 들어, 관련 페이지를 삭제하지 않고 최신 상태가 아닌 것으로 효과적으로 플래그를 지정합니다.

### 로드 밸런싱의 이점은 무엇입니까?

로드 밸런싱은 여러 AEM 인스턴스에 사용자 요청(로드)을 배포합니다. 다음 목록은 로드 밸런싱을 위한 이점을 설명합니다.

* **향상된 처리 능력**:실제로 디스패처가 여러 AEM 인스턴스 간에 문서 요청을 공유한다는 것을 의미합니다. 각 인스턴스에는 처리할 문서가 적으므로 응답 시간이 빨라집니다. Dispatcher는 각 문서 범주에 대한 내부 통계를 유지하므로 로드를 예측하고 쿼리를 효율적으로 배포할 수 있습니다.
* **장애 조치**&#x200B;증가:Dispatcher가 인스턴스로부터 응답을 받지 않으면 요청을 다른 인스턴스 중 하나로 자동으로 전달합니다. 따라서 인스턴스를 사용할 수 없게 되면 그 유일한 효과는 계산 능력이 손실되는 정도에 비례하여 사이트의 둔화입니다.

>[!NOTE]
>
>자세한 내용은 Dispatcher [개요 페이지를 참조하십시오.](dispatcher.md)

## 설치 및 구성

### Dispatcher 모듈은 어디에서 다운로드합니까?

Dispatcher 릴리스 정보 페이지에서 최신 Dispatcher [모듈을 다운로드할 수](release-notes.md) 있습니다.

### Dispatcher 모듈은 어떻게 설치합니까?

Dispatcher [설치 페이지를](dispatcher-install.md) 참조하십시오

### 디스패처 모듈을 구성하려면 어떻게 합니까?

디스패처 [구성](dispatcher-configuration.md) 페이지를 참조하십시오.

### 작성자 인스턴스에 대해 디스패처를 구성하려면 어떻게 합니까?

자세한 [단계는 작성자 인스턴스와](dispatcher.md#using-a-dispatcher-with-an-author-server) Dispatcher 사용을 참조하십시오.

### 여러 도메인으로 디스패처를 구성하려면 어떻게 합니까?

도메인이 다음 조건을 충족하는 경우 여러 도메인으로 CQ 디스패처를 구성할 수 있습니다.

* 두 도메인의 웹 컨텐츠는 단일 AEM 저장소에 저장됩니다.
* Dispatcher 캐시의 파일은 각 도메인에 대해 별도로 무효화할 수 있습니다.

자세한 [내용은 Dispatcher와 다중 도메인](dispatcher-domains.md) 사용을 참조하십시오.

### 사용자의 모든 요청이 동일한 게시 인스턴스로 라우팅되도록 디스패처를 구성하려면 어떻게 합니까?

사용자의 모든 문서가 동일한 AEM 인스턴스에서 처리되도록 [고정 연결](dispatcher-configuration.md#identifying-a-sticky-connection-folder-stickyconnectionsfor) 기능을 사용할 수 있습니다. 이 기능은 개인화된 페이지와 세션 데이터를 사용하는 경우에 중요합니다. 데이터는 인스턴스에 저장됩니다. 따라서 동일한 사용자의 후속 요청은 해당 인스턴스로 반환해야 하며 그렇지 않으면 데이터가 손실됩니다.

고정 연결은 디스패처의 요청을 최적화하는 기능을 제한하므로 필요한 경우에만 이 방법을 사용해야 합니다. "고정" 문서를 포함하는 폴더를 지정할 수 있으므로 해당 폴더의 모든 문서가 사용자에 대해 동일한 인스턴스에서 처리되도록 할 수 있습니다.

### 고정 연결과 캐싱을 동시에 사용할 수 있습니까?

고정 연결을 사용하는 대부분의 페이지의 경우 캐시를 해제해야 합니다. 그렇지 않으면 세션 컨텐츠에 상관없이 페이지의 동일한 인스턴스가 모든 사용자에게 표시됩니다.

일부 애플리케이션의 경우 고정 연결과 캐싱을 모두 사용할 수 있습니다. 예를 들어 세션에 데이터를 쓰는 양식을 표시하는 경우 일관된 연결 및 캐싱을 사용할 수 있습니다.

### 디스패처와 AEM 게시 인스턴스가 동일한 물리적 시스템에 상주할 수 있습니까?

네, 그 기계가 충분히 강력하다면. 그러나 다른 컴퓨터에 디스패처 및 AEM 게시 인스턴스를 설정하는 것이 좋습니다.

일반적으로 게시 인스턴스는 방화벽 내에 있고 디스패처는 DMZ에 있습니다. 동일한 물리적 컴퓨터에 게시 인스턴스와 디스패처가 모두 있는 경우 방화벽 설정에서 외부 네트워크에서 게시 인스턴스에 직접 액세스할 수 없도록 해야 합니다.

### 특정 확장명으로 파일만 캐싱할 수 있습니까?

예. 예를 들어 GIF 파일만 캐시하려면 dispatcher.any 구성 파일의 캐시 섹션에서 *.gif를 지정합니다.

### 캐시에서 파일을 삭제하려면 어떻게 해야 합니까?

HTTP 요청을 사용하여 캐시에서 파일을 삭제할 수 있습니다. HTTP 요청을 받으면 Dispatcher가 캐시에서 파일을 삭제합니다. 디스패처는 페이지에 대한 클라이언트 요청을 받을 때만 파일을 다시 캐시합니다. 이러한 방식으로 캐시된 파일을 삭제하는 것은 동일한 페이지에 대한 동시 요청을 받을 가능성이 없는 웹 사이트에 적합합니다.

HTTP 요청에는 다음 구문이 있습니다.

```
POST /dispatcher/invalidate.cache HTTP/1.1
CQ-Action: Activate
CQ-Handle: path-pattern
Content-Length: 0
```

Dispatcher는 CQ-Handle 헤더 값과 일치하는 이름을 가진 캐시된 파일 및 폴더를 삭제합니다. 예를 들어 CQ 핸들은 다음 항목과 `/content/geomtrixx-outdoors/en` 일치합니다.

geometrixx-outdoors 디렉토리에 en으로 명명된 모든 파일(파일 확장명 포함)en 디렉토리 `_jcr_content` 아래에 이름이 지정된 모든 디렉토리(있는 경우 페이지의 하위 노드의 캐시된 렌더링 포함) `CQ-Action` 디렉토리는 `Delete` 또는 `Deactivate`인 경우에만 삭제됩니다.

이 항목에 대한 자세한 내용은 Dispatcher [캐시 수동으로 무효화를 참조하십시오](page-invalidate.md).

### 권한 구분 캐싱은 어떻게 구현합니까?

보안 컨텐츠 [캐싱 페이지를](permissions-cache.md) 참조하십시오.

### Dispatcher와 CQ 인스턴스 간 통신은 어떻게 보호합니까?

Dispatcher [보안 검사 목록](security-checklist.md) 및 AEM [보안 검사 목록 페이지를](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security-checklist.html) 참조하십시오.

### 발송자 문제가 `jcr:content``jcr%3acontent`

**질문**:최근 Adobe는 CQ 저장소에서 일부 데이터 양식을 얻던 ajax 호출 중 하나가 `jcr:content` 포함된 디스패처 수준에서 문제가 발생하여 인코딩되어 잘못된 결과 세트가 `jcr%3acontent` 발생하는 문제에 직면했습니다.

**답변**:'친숙한' URL을 가져와 Dispatcher에서 요청을 보내고 Dispatcher에서 캐싱 문제를 해결하려면 이 방법을 사용하십시오. `ResourceResolver.map()` map() 메서드는 `:` 콜론을 밑줄로 인코딩하고 resolve() 메서드는 다시 SLING JCR 읽기 가능한 형식으로 디코딩합니다. map() 메서드를 사용하여 Ajax 호출에 사용되는 URL을 생성해야 합니다.

자세한 내용: [https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html#namespace-mangling](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html#namespace-mangling)

## 발송자 플러시

### 게시 인스턴스에서 Dispatcher 플러시 에이전트를 구성하려면 어떻게 합니까?

복제 [페이지를](https://helpx.adobe.com/content/help/en/experience-manager/6-4/sites/deploying/using/replication.html#ConfiguringyourReplicationAgents) 참조하십시오.

### Dispatcher 플러시 문제를 어떻게 해결합니까?

[다음 질문에 대한 답변을 제공하는 이 문제 해결 문서를](https://helpx.adobe.com/content/help/en/experience-manager/kb/troubleshooting-dispatcher-flushing-issues.html) 참조하십시오.

* Dispatcher 캐시에 저장되지 않는 컨텐츠를 디버깅하려면 어떻게 해야 합니까?
* 캐시 파일이 업데이트되지 않는 상황을 디버깅하려면 어떻게 합니까?
* Dispatcher 플러싱과 관련 없는 상황이 작동하는 경우 어떻게 디버깅합니까?

삭제 작업으로 인해 디스패처가 플러시되는 경우 Sensei Martin의 이 커뮤니티 블로그 게시물에서 해결 방법을 [사용하십시오](https://mkalugin-cq.blogspot.in/2012/04/i-have-been-working-on-following.html).

### Dispatcher 캐시에서 DAM 자산을 플러시하려면 어떻게 합니까?

"체인 복제" 기능을 사용할 수 있습니다.  이 기능이 활성화되면 디스패처 플러시 에이전트는 작성자로부터 복제를 받으면 플러시 요청을 보냅니다.

활성화하려면:

1. [게시 시 플러시 에이전트를 만들려면 다음](page-invalidate.md#invalidating-dispatcher-cache-from-a-publishing-instance) 단계를 따르십시오.
1. 각 에이전트의 구성으로 이동하고 트리거 **탭에서** 수신 **확인란을 선택합니다** .

## 기타

Dispatcher는 문서가 최신 상태인지 여부를 어떻게 결정합니까?
문서가 최신 상태인지 여부를 확인하려면 Dispatcher가 다음 작업을 수행합니다.

문서가 자동 무효화의 대상이 되는지 확인합니다. 그렇지 않으면 문서가 최신 문서로 간주됩니다.
문서가 자동 무효화되도록 구성된 경우 Dispatcher는 사용 가능한 마지막 변경 내용보다 오래되었는지 여부를 확인합니다. 이전 버전인 경우 디스패처는 AEM 인스턴스에서 현재 버전을 요청하고 캐시에 있는 버전을 대체합니다.

### Dispatcher는 문서를 어떻게 반환합니까?

Dispatcher 구성 [파일을 사용하여 Dispatcher가 문서를 캐시하는지 여부를 정의할](dispatcher-configuration.md) 수 `dispatcher.any`있습니다. Dispatcher는 캐시 가능한 문서 목록에 대해 요청을 확인합니다. 문서가 이 목록에 없으면 디스패처가 AEM 인스턴스에서 문서를 요청합니다.

이 `/rules` 속성은 문서 경로에 따라 캐시되는 문서를 제어합니다. Dispatcher는 `/rules` 속성에 관계없이 다음 상황에서 문서를 캐시하지 않습니다.

* 요청 URI에 물음표가 있는 `(?)`경우
* 일반적으로 이것은 캐시할 필요가 없는 검색 결과와 같은 동적 페이지를 나타냅니다.
* 파일 확장자가 없습니다.
* 웹 서버에는 문서 유형(MIME 유형)을 결정하기 위한 확장이 필요합니다.
* 인증 헤더가 설정됨(구성할 수 있음)
* AEM 인스턴스가 다음 헤더로 응답하는 경우:
   * no-cache
   * no-store
   * must-revalidate

Dispatcher는 캐시된 파일을 정적 웹 사이트의 일부인 것처럼 웹 서버에 저장합니다. 사용자가 캐시된 문서를 요청하면 디스패처는 문서가 웹 서버의 파일 시스템에 있는지 여부를 확인합니다. 이 경우 디스패처는 문서를 반환합니다. 그렇지 않으면 디스패처가 AEM 인스턴스에서 문서를 요청합니다.

>[!NOTE]
>
>GET 또는 HEAD(HTTP 헤더의 경우) 메서드는 Dispatcher에서 액세스할 수 있습니다. 응답 헤더 캐싱에 대한 자세한 내용은 HTTP 응답 [헤더 캐싱 섹션을 참조하십시오](dispatcher-configuration.md#caching-http-response-headers) .

### 설정에서 여러 디스패처를 구현할 수 있습니까?

예. 이러한 경우, Dispatcher가 모두 AEM 웹 사이트에 직접 액세스할 수 있는지 확인하십시오. Dispatcher는 다른 Dispatcher에서 오는 요청을 처리할 수 없습니다.

