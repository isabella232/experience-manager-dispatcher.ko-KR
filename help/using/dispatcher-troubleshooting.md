---
title: Dispatcher 문제 해결
seo-title: AEM 발송자 문제 해결
description: 디스패처 문제를 해결하는 방법을 알아봅니다.
seo-description: AEM Dispatcher 문제를 해결하는 방법을 알아봅니다.
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
source-git-commit: 5734e601379fda9a62eda46bded493b8dbd49a4c
workflow-type: tm+mt
source-wordcount: '553'
ht-degree: 7%

---


# Dispatcher 문제 해결 {#troubleshooting-dispatcher-problems}

>[!NOTE]
>
>디스패처 버전은 AEM과 무관하지만 Dispatcher 문서는 AEM 문서에 임베드됩니다. 항상 최신 버전의 AEM용으로 설명서에 포함된 Dispatcher 설명서를 사용하십시오.
>
>이전 버전의 AEM에 대한 설명서에 포함된 Dispatcher 설명서에 대한 링크를 따라간 경우 이 페이지로 리디렉션되었을 수 있습니다.

>[!NOTE]
>
>자세한 내용은 [Dispatcher 기술 자료](https://helpx.adobe.com/cq/kb/index/dispatcher.html), Dispatcher Flashing 문제 [해결](https://helpx.adobe.com/adobe-cq/kb/troubleshooting-dispatcher-flushing-issues.html) 및 [Dispatcher 주요 문제 FAQ를](dispatcher-faq.md) 참조하십시오.

## 기본 구성 확인 {#check-the-basic-configuration}

항상 첫 번째 단계는 기본 사항을 확인하는 것입니다.

* [기본 작업 확인](#ConfirmBasicOperation)
* 웹 서버 및 디스패처의 모든 로그 파일을 확인합니다. 필요한 경우 디스패처 로깅 `loglevel` 에 사용되는 필드를 [늘립니다](#Logging).

* [구성 확인](#ConfiguringtheDispatcher):

   * 여러 디스패처가 있습니까?

      * 조사 중인 웹 사이트/페이지를 Dispatcher에서 처리하고 있는지 확인했습니까?
   * 필터를 구현했습니까?

      * 이것이 당신이 조사하고 있는 문제에 영향을 줍니까?


## IIS 진단 도구 {#iis-diagnostic-tools}

IIS는 실제 버전에 따라 다양한 추적 도구를 제공합니다.

* IIS 6 - IIS 진단 도구를 다운로드하고 구성할 수 있습니다.
* IIS 7 - 추적이 완전히 통합됨

이러한 기능은 활동을 모니터링하는 데 도움이 될 수 있습니다.

## IIS 및 404를 찾을 수 없음 {#iis-and-not-found}

IIS를 사용하는 경우 다양한 시나리오에서 반환되는 경우가 `404 Not Found` 발생할 수 있습니다. 그렇다면 다음 기술 자료 문서를 참조하십시오.

* [IIS 6/7:POST 메서드가 404를 반환합니다.](https://helpx.adobe.com/dispatcher/kb/IIS6IsapiFilters.html)
* [IIS 6:기본 경로 반환이 포함된 URL에 대한 `/bin` 요청 `404 Not Found`](https://helpx.adobe.com/dispatcher/kb/RequestsToBinDirectoryFailInIIS6.html)

또한 디스패처 캐시 루트와 IIS 문서 루트가 동일한 디렉토리로 설정되었는지 확인해야 합니다.

## 워크플로우 모델 삭제 문제 {#problems-deleting-workflow-models}

**증상**

Dispatcher를 통해 AEM 작성자 인스턴스에 액세스할 때 워크플로우 모델을 삭제하는 데 문제가 발생했습니다.

**재현하는 단계:**

1. 작성자 인스턴스에 로그인합니다(요청이 디스패처를 통해 라우팅되는지 확인).
1. 새 워크플로우 만들기;예를 들어, 제목이 workflowToDelete로 설정되어 있으면 됩니다.
1. 워크플로우가 성공적으로 생성되었는지 확인합니다.
1. 워크플로우를 선택하고 마우스 오른쪽 단추로 클릭한 다음 **삭제를 클릭합니다**.

1. **예**&#x200B;를 클릭하여 확인합니다.
1. 다음과 같은 오류 메시지 상자가 표시됩니다.\
   &quot; `ERROR 'Could not delete workflow model!!`&quot;.

**해상도**

파일의 섹션에 다음 헤더 `/clientheaders` 를 `dispatcher.any` 추가합니다.

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

## mod_dir(Apache)에 대한 간섭 {#interference-with-mod-dir-apache}

This describes how the dispatcher interact with `mod_dir` in the Apache webserver, as this can lead to various, immediately unexpected effects:

### Apache 1.3 {#apache}

Apache 1.3에서는 URL이 파일 시스템의 디렉토리로 매핑되는 모든 요청을 처리합니다 `mod_dir` .

다음 중 하나를 수행합니다.

* 요청을 기존 `index.html` 파일로 리디렉션
* 디렉토리 목록 생성

디스패처가 활성화되면 컨텐츠 유형에 대한 처리기로 등록하여 이러한 요청을 처리합니다 `httpd/unix-directory`.

### Apache 2.x {#apache-x}

Apache 2.x에서는 상황이 다릅니다. 모듈은 URL 수정과 같이 요청의 다른 단계를 처리할 수 있습니다. `mod_dir` 요청을 리디렉션하여(URL이 디렉터리로 매핑되는 경우) 이 단계를 `/` 처리합니다.

디스패처는 수정을 `mod_dir` `/` 가로채지는 않지만 리디렉션 URL로 요청을 완전히 처리합니다(즉, 추가됨). 원격 서버(예: AEM)이 요청에 대한 요청을 `/a_path` 다르게 처리하는 경우(기존 디렉토리로 매핑되는 경우) 문제가 `/a_path/` `/a_path` 발생할 수 있습니다.

이러한 경우 다음 중 하나를 수행해야 합니다.

* 디스패처 `mod_dir` 에서 처리하는 `Directory` `Location` 또는 하위 트리에 대해 비활성화

* 추가하지 `DirectorySlash Off` 않도록 `mod_dir` 구성합니다. `/`
