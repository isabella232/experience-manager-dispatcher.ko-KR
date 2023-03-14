---
title: Dispatcher 문제 해결
seo-title: Troubleshooting AEM Dispatcher Problems
description: Dispatcher 문제를 해결하는 방법에 대해 알아봅니다.
seo-description: Learn to troubleshoot AEM Dispatcher issues.
uuid: 9c109a48-d921-4b6e-9626-1158cebc41e7
cmgrlastmodified: 01.11.2007 08 22 29 [aheimoz]
pageversionid: 1193211344162
template: /apps/docs/templates/contentpage
contentOwner: User
products: SG_EXPERIENCEMANAGER/DISPATCHER
topic-tags: dispatcher
content-type: reference
discoiquuid: a612e745-f1e6-43de-b25a-9adcaadab5cf
exl-id: 29f338ab-5d25-48a4-9309-058e0cc94cff
source-git-commit: 26c8edbb142297830c7c8bd068502263c9f0e7eb
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 43%

---

# Dispatcher 문제 해결 {#troubleshooting-dispatcher-problems}

>[!NOTE]
>
>Dispatcher 버전은 AEM과 독립적이지만 Dispatcher 설명서는 AEM 설명서에 임베드되어 있습니다. 항상 최신 버전의 AEM 설명서에 임베드된 Dispatcher 설명서를 사용하십시오.
>
>이전 버전의 AEM에 대한 설명서에 임베드된 Dispatcher 설명서에 대한 링크를 따라간 경우 이 페이지로 리디렉션되었을 수 있습니다.

>[!NOTE]
>
>을(를) 확인합니다. [Dispatcher 기술 자료](https://helpx.adobe.com/experience-manager/kb/index/dispatcher.html), [Dispatcher 플러시 문제 해결](https://experienceleague.adobe.com/search.html?lang=en#q=troubleshooting%20dispatcher%20flushing%20issues&amp;sort=relevancy&amp;f:el_product=[Experience%20Manager]) 그리고 [Dispatcher 주요 문제 FAQ](dispatcher-faq.md) 추가 정보.

## 기본 구성 확인 {#check-the-basic-configuration}

항상 그렇듯이 첫 번째 단계는 기본 사항을 확인하는 것입니다.

* [기본 작동 확인](/help/using/dispatcher-configuration.md#confirming-basic-operation)
* 웹 서버 및 Dispatcher에 대한 모든 로그 파일을 확인합니다. 필요한 경우 `loglevel` Dispatcher에 사용됩니다. [로깅](/help/using/dispatcher-configuration.md#logging).

* [구성 확인](/help/using/dispatcher-configuration.md):

   * 여러 Dispatcher가 있습니까?

      * 조사 중인 웹 사이트/페이지를 어떤 Dispatcher에서 처리하는지 확인했습니까?
   * 필터를 구현했습니까?

      * 이러한 필터가 조사 중인 문제에 영향을 줍니까?


## IIS 진단 도구 {#iis-diagnostic-tools}

IIS는 실제 버전에 따라 다양한 추적 도구를 제공합니다.

* IIS 6 - IIS 진단 도구를 다운로드하고 구성할 수 있음
* IIS 7 - 추적이 완전히 통합됨

이러한 도구는 활동을 모니터링하는 데 도움이 될 수 있습니다.

## IIS 및 404를 찾을 수 없음 {#iis-and-not-found}

IIS를 사용할 때 `404 Not Found` 여러 시나리오에서 반환되는 중입니다. 이 경우 다음 기술 자료 문서를 참조하십시오.

* [IIS 6/7: POST 메서드가 404를 반환합니다](https://helpx.adobe.com/experience-manager/kb/IIS6IsapiFilters.html)
* [IIS 6: 기본 경로가 포함된 URL에 대한 요청 `/bin` 반환 `404 Not Found`](https://helpx.adobe.com/experience-manager/kb/RequestsToBinDirectoryFailInIIS6.html)

또한 Dispatcher 캐시 루트와 IIS 문서 루트가 동일한 디렉토리로 설정되어 있는지 확인합니다.

## 워크플로 모델 삭제 문제 {#problems-deleting-workflow-models}

**증상**

Dispatcher를 통해 AEM 작성자 인스턴스에 액세스할 때 워크플로 모델을 삭제하려고 하면 문제가 발생합니다.

**재현 단계:**

1. 작성자 인스턴스에 로그인합니다(요청이 Dispatcher를 통해 라우팅되는지 확인).
1. 워크플로우 만들기 예를 들어 Title을 workflowToDelete로 설정하면
1. 워크플로가 성공적으로 생성되었는지 확인합니다.
1. 을(를) 선택하고 마우스 오른쪽 단추로 워크플로우를 클릭한 다음 을(를) 클릭합니다 **삭제**.

1. **예**&#x200B;를 클릭하여 확인합니다.
1. 다음을 보여주는 오류 메시지 상자가 나타납니다.\
   &quot; `ERROR 'Could not delete workflow model!!`&quot;.

**해결**

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

## mod_dir 간섭(Apache) {#interference-with-mod-dir-apache}

이 프로세스에서는 Dispatcher가 상호 작용하는 방법을 설명합니다 `mod_dir` Apache 웹 서버 내부에서 다양한 예기치 않은 효과로 이어질 수 있습니다.

### Apache 1.3 {#apache}

Apache 1.3에서, `mod_dir` 는 URL이 파일 시스템의 디렉토리에 매핑되는 모든 요청을 처리합니다.

다음 중 하나를 수행합니다.

* 기존 `index.html` 파일로 요청 리디렉션
* 디렉터리 목록 생성

Dispatcher가 활성화되어 있으면 자신을 컨텐츠 유형의 처리기로 등록하여 이러한 요청을 처리합니다 `httpd/unix-directory`.

### Apache 2.x {#apache-x}

Apache 2.x에서는 상황이 다릅니다. 모듈은 URL 수정과 같은 요청의 여러 단계를 처리할 수 있습니다. 다음 `mod_dir` 가 있는 URL에 요청을 리디렉션하여 이 단계를 처리합니다(URL이 디렉토리에 매핑될 때) `/` 추가됨.

Dispatcher가 를 차단하지 않습니다 `mod_dir` 를 수정하지만 를 사용하여 리디렉션된 URL로 요청을 완전히 처리합니다. `/` 추가됨). 원격 서버(예: AEM)이 요청을 처리하는 경우 이 프로세스에서 문제가 발생할 수 있습니다 `/a_path` 에 대한 요청과 `/a_path/` (다음의 경우) `/a_path` 기존 디렉토리에 매핑).

이러한 상황이 발생하는 경우 다음 중 하나를 수행해야 합니다.

* disable `mod_dir` 대상 `Directory` 또는 `Location` Dispatcher에 의해 처리된 하위 트리

* `DirectorySlash Off`를 사용하여 `/`를 추가하지 않도록 `mod_dir` 구성
