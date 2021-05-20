---
title: Dispatcher 문제 해결
seo-title: AEM Dispatcher 문제 해결
description: Dispatcher 문제를 해결하는 방법을 알아봅니다.
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
exl-id: 29f338ab-5d25-48a4-9309-058e0cc94cff
source-git-commit: 3a0e237278079a3885e527d7f86989f8ac91e09d
workflow-type: tm+mt
source-wordcount: '553'
ht-degree: 7%

---

# Dispatcher 문제 해결 {#troubleshooting-dispatcher-problems}

>[!NOTE]
>
>Dispatcher 버전은 AEM과 독립적이지만 AEM 설명서에 Dispatcher 설명서를 포함합니다. 최신 버전의 AEM에 대해 설명서에 포함된 Dispatcher 설명서를 항상 사용하십시오.
>
>이전 버전의 AEM에 대한 설명서에 포함된 Dispatcher 설명서에 대한 링크를 따라간 경우 이 페이지로 리디렉션되었을 수 있습니다.

>[!NOTE]
>
>자세한 내용은 [Dispatcher 기술 자료](https://helpx.adobe.com/cq/kb/index/dispatcher.html), [Dispatcher 플러시 문제 해결](https://helpx.adobe.com/adobe-cq/kb/troubleshooting-dispatcher-flushing-issues.html) 및 [Dispatcher 주요 문제 FAQ](dispatcher-faq.md)를 참조하십시오.

## 기본 구성 {#check-the-basic-configuration} 확인

항상 첫 번째 단계는 기본 사항을 확인하는 것입니다.

* [기본 작업 확인](/help/using/dispatcher-configuration.md#confirming-basic-operation)
* 웹 서버 및 Dispatcher에 대한 모든 로그 파일을 확인합니다. 필요한 경우 Dispatcher [logging](/help/using/dispatcher-configuration.md#logging)에 사용되는 `loglevel`을 늘립니다.

* [구성을 확인합니다](/help/using/dispatcher-configuration.md).

   * 여러 Dispatcher가 있습니까?

      * 조사 중인 웹 사이트/페이지를 처리하는 Dispatcher를 결정했습니까?
   * 필터를 구현했습니까?

      * 이것이 당신이 조사하고 있는 문제에 영향을 미칩니까?


## IIS 진단 도구 {#iis-diagnostic-tools}

IIS는 실제 버전에 따라 다양한 추적 도구를 제공합니다.

* IIS 6 - IIS 진단 도구를 다운로드하여 구성할 수 있습니다
* IIS 7 - 추적 기능이 완전히 통합됨

활동을 모니터링하는 데 도움이 될 수 있습니다.

## IIS 및 404를 찾을 수 없음 {#iis-and-not-found}

IIS를 사용하는 경우 여러 시나리오에서 `404 Not Found`이 반환될 수 있습니다. 이 경우 다음 기술 자료 문서를 참조하십시오.

* [IIS 6/7:POST 메서드가 404를 반환합니다.](https://helpx.adobe.com/dispatcher/kb/IIS6IsapiFilters.html)
* [IIS 6:기본 경로 반환이 포함된 URL에 대한  `/bin` 요청  `404 Not Found`](https://helpx.adobe.com/dispatcher/kb/RequestsToBinDirectoryFailInIIS6.html)

또한 디스패처 캐시 루트와 IIS 문서 루트가 동일한 디렉토리로 설정되어 있는지 확인해야 합니다.

## 워크플로우 모델 삭제 문제 {#problems-deleting-workflow-models}

**증상**

Dispatcher를 통해 AEM 작성자 인스턴스에 액세스할 때 워크플로우 모델을 삭제하려고 할 때 문제가 발생합니다.

**재현할 절차:**

1. 작성자 인스턴스에 로그인합니다(요청이 디스패처를 통해 라우팅되는지 확인).
1. 새 워크플로우를 만듭니다.예를 들어 Title을 workflowToDelete로 설정하면
1. 워크플로우가 성공적으로 만들어졌는지 확인합니다.
1. 을(를) 선택하고 워크플로우를 마우스 오른쪽 단추로 클릭한 다음 **삭제**&#x200B;를 클릭합니다.

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

이렇게 하면 예기치 않은 다양한 효과가 발생할 수 있으므로 디스패처가 Apache 웹 서버 내에서 `mod_dir`과 상호 작용하는 방법을 설명합니다.

### Apache 1.3 {#apache}

Apache 1.3 `mod_dir`에서는 URL이 파일 시스템의 디렉토리에 매핑되는 모든 요청을 처리합니다.

다음 중 하나가 됩니다.

* 기존 `index.html` 파일로 요청 리디렉션
* 디렉토리 목록 생성

디스패처가 활성화되면 이 디스패처는 컨텐츠 유형 `httpd/unix-directory`에 대한 핸들러로 등록하여 이러한 요청을 처리합니다.

### Apache 2.x {#apache-x}

Apache 2.x에서는 사정이 다릅니다. 모듈은 URL 수정과 같이 요청의 다른 단계를 처리할 수 있습니다. `mod_dir` 가 추가된 URL에 요청을 리디렉션하여 이 단계를  `/` 처리합니다.

Dispatcher가 `mod_dir` 수정을 가로채지 않지만 리디렉션된 URL로 요청을 완전히 처리합니다(즉, `/`가 추가됨). 원격 서버(예: AEM)이 `/a_path/`에 대한 요청과 다르게 `/a_path`에 대한 요청을 처리하는 경우 문제가 발생할 수 있습니다( `/a_path`가 기존 디렉토리에 매핑될 때).

이러한 경우 다음 중 하나를 수행해야 합니다.

* dispatcher에서 처리하는 `Directory` 또는 `Location` 하위 트리에 대해 `mod_dir` 비활성화

* `/` 를 사용하지 않도록 `mod_dir` 구성`DirectorySlash Off`
