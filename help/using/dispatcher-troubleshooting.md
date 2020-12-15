---
title: Dispatcher 문제 해결
seo-title: AEM 발송자 문제 해결
description: 발송자 문제를 해결하는 방법을 알아봅니다.
seo-description: AEM Dispatcher 문제를 해결하는 방법을 학습합니다.
uuid: 9c109a48-d921-4b6e-9626-1158cebc41e7
cmgrlastmodified: 01.11.2007 08 22 29 [aheimoz]
pageversionid: 1193211344162
template: /apps/docs/templates/contentpage
contentOwner: User
products: SG_EXPERIENCEMANAGER/DISPATCHER
topic-tags: dispatcher
content-type: reference
discoiquuid: a612e745-f1e6-43de-b25a-9adcaadab5cf
translation-type: tm+mt
source-git-commit: 9af0dc22d32f1176b84c28a70b1a4701414d434e
workflow-type: tm+mt
source-wordcount: '553'
ht-degree: 7%

---


# Dispatcher 문제 해결 {#troubleshooting-dispatcher-problems}

>[!NOTE]
>
>디스패처 버전은 AEM과 독립적이지만, Dispatcher 문서는 AEM 문서에 포함되어 있습니다. 항상 최신 버전의 AEM에 대한 설명서에 포함된 Dispatcher 문서를 사용합니다.
>
>이전 버전의 AEM에 대한 설명서에 포함된 Dispatcher 설명서에 대한 링크를 따라간 경우 이 페이지로 리디렉션되었을 수 있습니다.

>[!NOTE]
>
>자세한 내용은 [디스패처 기술 자료](https://helpx.adobe.com/cq/kb/index/dispatcher.html), [디스패처 플러싱 문제 해결](https://helpx.adobe.com/adobe-cq/kb/troubleshooting-dispatcher-flushing-issues.html) 및 [디스패처 주요 문제 FAQ](dispatcher-faq.md)를 참조하십시오.

## 기본 구성 {#check-the-basic-configuration} 확인

항상 첫 번째 단계는 기본 사항을 확인하는 것입니다.

* [기본 작업 확인](/help/using/dispatcher-configuration.md#confirming-basic-operation)
* 웹 서버 및 디스패처의 모든 로그 파일을 확인합니다. 필요한 경우 디스패처 [로깅](/help/using/dispatcher-configuration.md#logging)에 사용되는 `loglevel`을 늘립니다.

* [구성 확인](/help/using/dispatcher-configuration.md):

   * 여러 디스패처 있습니까?

      * 조사 중인 웹 사이트/페이지를 Dispatcher에서 처리하는지 확인했습니까?
   * 필터를 구현했습니까?

      * 이것이 당신이 조사하고 있는 일에 영향을 줍니까?


## IIS 진단 도구 {#iis-diagnostic-tools}

IIS는 실제 버전에 따라 다양한 추적 도구를 제공합니다.

* IIS 6 - IIS 진단 도구를 다운로드하고 구성할 수 있습니다.
* IIS 7 - 추적이 완전히 통합됨

활동을 모니터링하는 데 도움이 될 수 있습니다.

## IIS 및 404를 찾을 수 없음 {#iis-and-not-found}

IIS를 사용할 때 다양한 시나리오에서 `404 Not Found`이(가) 반환되는 문제가 발생할 수 있습니다. 이러한 경우 다음 기술 자료 문서를 참조하십시오.

* [IIS 6/7:POST 메서드가 404를 반환합니다.](https://helpx.adobe.com/dispatcher/kb/IIS6IsapiFilters.html)
* [IIS 6:기본 경로 반환이 포함된 URL에 대한  `/bin` 요청  `404 Not Found`](https://helpx.adobe.com/dispatcher/kb/RequestsToBinDirectoryFailInIIS6.html)

또한 디스패처 캐시 루트와 IIS 문서 루트가 동일한 디렉토리로 설정되어 있는지 확인해야 합니다.

## 워크플로 모델 삭제 문제 {#problems-deleting-workflow-models}

**증상**

Dispatcher를 통해 AEM 작성자 인스턴스에 액세스할 때 워크플로우 모델을 삭제하는 데 문제가 발생했습니다.

**재현할 단계:**

1. 작성자 인스턴스에 로그인합니다(요청이 디스패처를 통해 라우팅되는지 확인).
1. 새 워크플로우 만들기;예를 들어 Title을 workflowToDelete로 설정하면
1. 워크플로우가 성공적으로 생성되었는지 확인합니다.
1. 선택하고 작업 과정을 마우스 오른쪽 단추로 클릭한 다음 **삭제**&#x200B;를 클릭합니다.

1. **예**&#x200B;를 클릭하여 확인합니다.
1. 다음과 같은 오류 메시지 상자가 표시됩니다.\
   &quot; `ERROR 'Could not delete workflow model!!`&quot;.

**해상도**

`dispatcher.any` 파일의 `/clientheaders` 섹션에 다음 헤더를 추가합니다.

* `x-http-method-override`
* `x-requested-with`

```
{  
{  
/clientheaders  
{  
...  
"x-http-method-override"  
"x-requested-with"  
}
```

## mod_dir(Apache) {#interference-with-mod-dir-apache}에 대한 간섭

Apache 웹 서버 내에서 디스패처가 `mod_dir`과(와) 상호 작용하는 방식을 설명합니다. 이렇게 하면 예상치 못한 다양한 효과가 발생할 수 있습니다.

### Apache 1.3 {#apache}

Apache 1.3 `mod_dir`에서는 URL이 파일 시스템의 디렉토리에 매핑되는 모든 요청을 처리합니다.

다음 중 하나를 수행합니다.

* 요청을 기존 `index.html` 파일로 리디렉션
* 디렉토리 목록 생성

디스패처가 활성화되어 있으면 디스패처는 컨텐트 유형 `httpd/unix-directory`의 핸들러로 자신을 등록하여 이러한 요청을 처리합니다.

### Apache 2.x {#apache-x}

Apache 2.x에서는 사정이 다릅니다. 모듈은 URL 수정과 같이 요청의 다른 단계를 처리할 수 있습니다. `mod_dir` 요청이 추가되면(URL이 디렉터리로 매핑될 때) 요청을 URL로 리디렉션하여 이 단계를  `/` 처리합니다.

디스패처는 `mod_dir` 수정을 가로채지는 않지만 리디렉션된 URL(즉, `/`이 추가된 포함)로 요청을 완전히 처리합니다. 원격 서버(예: AEM)이 `/a_path/`에 대한 요청과 다른 방식으로 `/a_path`에 대한 요청을 처리하는 경우(`/a_path`이(가) 기존 디렉토리로 매핑되는 경우) 문제가 발생할 수 있습니다.

이러한 경우 다음 중 하나를 수행해야 합니다.

* 디스패처가 처리하는 `Directory` 또는 `Location` 하위 트리에 대해 `mod_dir` 비활성화

* `/`을(를) 추가하지 않도록 `mod_dir`를 구성하려면 `DirectorySlash Off`을(를) 사용합니다.
