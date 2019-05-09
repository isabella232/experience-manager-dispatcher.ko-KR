---
title: Dispatcher 문제 해결
seo-title: AEM Dispatcher 문제 해결
description: Dispatcher 문제를 해결하는 방법을 알아보십시오.
seo-description: AEM Dispatcher 문제를 해결하는 방법을 알아보십시오.
uuid: 9 C 109 A 48-D 921-4 B 6 E -9626-1158 CEBC 41 E 7
cmgrlastmodified: 01.11.2007 08 22 29 [Aheimoz]
pageversionid: '1193211344162'
template: /apps/docs/templates/contentpage
contentOwner: 사용자
products: sg_ Experiencemanager/dispatcher
topic-tags: Dispatcher
content-type: 참조
discoiquuid: A 612 E 745-F 1 E 6-43 DE-B 25 A -9 adcaadab 5 CF
translation-type: tm+mt
source-git-commit: 76cffbfb616cd5601aed36b7076f67a2faf3ed3b

---


# Dispatcher 문제 해결 {#troubleshooting-dispatcher-problems}

>[!NOTE]
>
>Dispatcher 버전은 AEM와 독립적이지만 Dispatcher 설명서는 AEM 설명서에 포함되어 있습니다. 항상 최신 버전의 AEM에 대한 설명서에 포함된 Dispatcher 설명서를 사용하십시오.
>
>이전 버전의 AEM에 대한 설명서에 포함된 Dispatcher 설명서에 대한 링크를 팔로우한 경우 이 페이지로 리디렉션되었을 수 있습니다.

>[!NOTE]
>
>자세한 내용은 [Dispatcher 기술 자료](https://helpx.adobe.com/cq/kb/index/dispatcher.html), [Dispatcher의 문제 해결 문제 해결 문제](https://helpx.adobe.com/adobe-cq/kb/troubleshooting-dispatcher-flushing-issues.html) 및 [Dispatcher 주요 문제 FAQ](dispatcher-faq.md) 를 참조하십시오.

## 기본 구성 확인 {#check-the-basic-configuration}

언제나 첫 번째 단계는 기본 사항을 확인하는 것입니다.

* [기본 작업 확인](#ConfirmBasicOperation)
* 웹 서버 및 디스패처에 대한 모든 로그 파일을 확인합니다. 필요한 경우 Dispatcher `loglevel`[로깅에 사용할 수 있습니다](#Logging).

* [구성을 확인하십시오](#ConfiguringtheDispatcher).

   * 여러 개의 발송자가 있습니까?

      * 조사하는 웹 사이트/페이지를 처리하는 디스패처를 결정했습니까?
   * 필터를 구현했습니까?

      * 이것이 조사 중인 문제에 영향을 줍니까?


## IIS 진단 도구 {#iis-diagnostic-tools}

IIS는 실제 버전에 따라 다양한 추적 도구를 제공합니다.

* IIS 6 - IIS 진단 도구를 다운로드하여 구성할 수 있습니다.
* IIS 7 - 추적이 완전히 통합

활동을 모니터링할 수 있습니다.

## IIS 및 404를 찾을 수 없음 {#iis-and-not-found}

IIS를 사용하는 경우 다양한 시나리오에서 반환되는 경험이 `404 Not Found` 있을 수 있습니다. 그렇다면 다음 기술 자료 문서를 참조하십시오.

* [IIS 6/7: POST 메서드가 404를 반환합니다.](https://helpx.adobe.com/dispatcher/kb/IIS6IsapiFilters.html)
* [IIS 6: 기본 경로 `/bin` 리턴이 포함된 URL에 대한 요청 `404 Not Found`](https://helpx.adobe.com/dispatcher/kb/RequestsToBinDirectoryFailInIIS6.html)

또한 Dispatcher Cache 루트 및 IIS 문서 루트가 동일한 디렉토리로 설정되어 있는지 확인해야 합니다.

## 워크플로우 모델 삭제 문제 {#problems-deleting-workflow-models}

**증시**

Dispatcher를 통해 AEM 작성자 인스턴스에 액세스할 때 워크플로우 모델을 삭제하려는 문제가 해결되었습니다.

**재현 단계:**

1. 작성자 인스턴스에 로그인합니다 (요청이 Dispatcher를 통해 라우팅되는지 확인).
1. 새 워크플로우 만들기 예를 들어 제목이 Workflowtodelete로 설정된 경우
1. 워크플로우가 성공적으로 생성되었는지 확인합니다.
1. 워크플로우를 선택하고 마우스 오른쪽 단추로 클릭한 다음 **삭제를 클릭합니다**.

1. **예**를 클릭하여 확인합니다.
1. 표시되는 오류 메시지 상자가 나타납니다.\
   &quot; `ERROR 'Could not delete workflow model!!`&quot;.

**해상도**

다음 헤더를 파일 `/clientheaders` 섹션에 `dispatcher.any` 추가합니다.

* `x-http-method-override`
* `x-requested-with`

`{  
{  
/clientheaders  
{  
...  
"x-http-method-override"  
"x-requested-with"  
}`

## mod_ dir (Apache) 과의 간섭 {#interference-with-mod-dir-apache}

다음은 Apache webserver `mod_dir` 내부에서 디스패처가 상호 작용하는 방식을 설명합니다. 이 경우 예상치 못한 다양한 효과가 발생할 수 있습니다.

### Apache 1.3 {#apache}

Apache 1.3 `mod_dir` 에서는 URL 이 파일 시스템의 디렉토리에 매핑되는 모든 요청을 처리합니다.

다음 중 하나를 수행합니다.

* 요청을 기존 `index.html` 파일로 리디렉션
* 디렉토리 목록 생성

디스패처가 활성화되면 해당 요청을 컨텐츠 유형에 `httpd/unix-directory`대한 핸들러로 등록하여 해당 요청을 처리합니다.

### Apache 2. x {#apache-x}

Apache 2. x 에서는 다릅니다. 모듈은 URL 수정과 같은 다른 요청의 단계를 처리할 수 있습니다. `mod_dir` 요청 (URL 이 디렉토리로 매핑될 때) 를 `/` 덧붙인 URL로 리디렉션하여 이 스테이지를 처리합니다.

Dispatcher는 `mod_dir` 수정을 가로채지 않고 리디렉션된 URL (즉, 추가) `/` 에 대한 요청을 완전히 처리합니다. 원격 서버 (예: AEM) 가 요청을 `/a_path` 다르게 처리하는 경우 `/a_path/` (기존 디렉토리에 `/a_path` 대한 맵이 있는 경우) 문제가 발생할 수 있습니다.

이러한 경우 다음 중 하나를 수행해야 합니다.

* 디스패처가 `mod_dir` 처리하는 `Directory` 또는 `Location` 하위 트리에 대해 비활성화

* 추가되지 않도록 구성하는 `DirectorySlash Off``mod_dir` 데 사용 `/`
