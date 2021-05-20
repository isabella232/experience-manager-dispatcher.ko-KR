---
title: 캐시 성능을 위한 웹 사이트 최적화
seo-title: 캐시 성능을 위한 웹 사이트 최적화
description: 캐싱의 이점을 극대화하기 위해 웹 사이트를 디자인하는 방법을 알아봅니다.
seo-description: Dispatcher는 성능을 최적화하는 데 사용할 수 있는 많은 내장 메커니즘을 제공합니다. 캐싱의 이점을 극대화하기 위해 웹 사이트를 디자인하는 방법을 알아봅니다.
uuid: 2d4114d1-f464-4e10-b25c-a1b9a9c715d1
contentOwner: User
products: SG_EXPERIENCEMANAGER/DISPATCHER
topic-tags: dispatcher
content-type: reference
discoiquuid: ba323503-1494-4048-941d-c1d14f2e85b2
redirecttarget: https://helpx.adobe.com/experience-manager/6-4/sites/deploying/using/configuring-performance.html
index: y
internal: n
snippet: y
source-git-commit: 2ca816ac0776d72be651b76ff4f45e0c3ed1450f
workflow-type: tm+mt
source-wordcount: '1167'
ht-degree: 3%

---


# 캐시 성능을 위한 웹 사이트 최적화 {#optimizing-a-website-for-cache-performance}

<!-- 

Comment Type: remark
Last Modified By: Silviu Raiman (raiman)
Last Modified Date: 2017-10-25T04:13:34.919-0400

<p>This is a redirect to /experience-manager/6-2/sites/deploying/using/configuring-performance.html</p>

 -->

>[!NOTE]
>
>Dispatcher 버전은 AEM과 독립적입니다. 이전 버전의 AEM에 대한 설명서에 포함된 Dispatcher 설명서에 대한 링크를 따라간 경우 이 페이지로 리디렉션되었을 수 있습니다.

Dispatcher는 성능을 최적화하는 데 사용할 수 있는 많은 내장 메커니즘을 제공합니다. 이 섹션에서는 캐싱의 이점을 극대화하도록 웹 사이트를 디자인하는 방법을 설명합니다.

>[!NOTE]
>
>Dispatcher가 표준 웹 서버에 캐시를 저장한다는 것을 이해하는 데 도움이 될 수 있습니다. 이는 사용자가 다음을 의미함을 의미합니다.
>
>* 는 페이지로 저장할 수 있는 모든 것을 캐시하고 URL을 사용하여 요청할 수 있습니다
>* 는 HTTP 헤더, 쿠키, 세션 데이터 및 양식 데이터와 같은 다른 항목을 저장할 수 없습니다.

>
>
일반적으로 많은 캐싱 전략에는 적절한 URL을 선택하고 이 추가 데이터에 의존하지 않는 것이 포함됩니다.

## 일관된 페이지 인코딩 사용 {#using-consistent-page-encoding}

HTTP 요청 헤더는 캐시되지 않으므로 헤더에 페이지 인코딩 정보를 저장하는 경우 문제가 발생할 수 있습니다. 이 경우 Dispatcher가 캐시에서 페이지를 제공할 때 웹 서버의 기본 인코딩이 페이지에 사용됩니다. 이 문제를 피하는 방법에는 두 가지가 있습니다.

* 인코딩을 하나만 사용하는 경우 웹 서버에서 사용되는 인코딩이 AEM 웹 사이트의 기본 인코딩과 같은지 확인하십시오.
* 다음 예와 같이 HTML `head` 섹션에서 `<META>` 태그를 사용하여 인코딩을 설정합니다.

```xml
        <META http-equiv="Content-Type" content="text/html; charset=EUC-JP">
```

## URL 매개 변수 {#avoid-url-parameters} 방지

가능하면 캐싱하려는 페이지의 URL 매개 변수를 사용하지 마십시오. 예를 들어 사진 갤러리가 있는 경우 다음 URL은 캐시되지 않습니다(Dispatcher가 [에 따라 구성된 경우가 아니면](dispatcher-configuration.md#main-pars_title_24)).

```xml
www.myCompany.com/pictures/gallery.html?event=christmas&amp;page=1
```

그러나 다음과 같이 이러한 매개 변수를 페이지 URL에 넣을 수 있습니다.

```xml
www.myCompany.com/pictures/gallery.christmas.1.html
```

>[!NOTE]
>
>이 URL은 동일한 페이지를 호출하고 gallery.html과 동일한 템플릿을 호출합니다. 템플릿 정의에서 페이지를 렌더링하는 스크립트를 지정하거나 모든 페이지에 대해 동일한 스크립트를 사용할 수 있습니다.

## URL {#customize-by-url}로 사용자 지정

사용자가 글꼴 크기(또는 기타 레이아웃 사용자 지정)를 변경할 수 있도록 허용하는 경우, 다른 사용자 지정 사항이 URL에 반영되었는지 확인하십시오.

예를 들어 쿠키는 캐시되지 않으므로 쿠키(또는 유사한 메커니즘)에 글꼴 크기를 저장하는 경우 글꼴 크기는 캐시된 페이지에 대해 유지되지 않습니다. 따라서 Dispatcher는 임의 글꼴 크기의 문서를 반환합니다.

선택기로 URL에 글꼴 크기를 포함하면 이 문제가 발생합니다.

```xml
www.myCompany.com/news/main.large.html
```

>[!NOTE]
>
>대부분의 레이아웃 측면에서 스타일 시트 및/또는 클라이언트측 스크립트를 사용할 수도 있습니다. 캐싱은 일반적으로 매우 잘 작동합니다.
>
>다음과 같은 URL을 사용할 수 있는 인쇄 버전에도 유용합니다.&quot;
>
>`www.myCompany.com/news/main.print.html`
>
>템플릿 정의의 스크립트 글로브를 사용하여 인쇄 페이지를 렌더링하는 별도의 스크립트를 지정할 수 있습니다.

## 제목 {#invalidating-image-files-used-as-titles}으로 사용된 이미지 파일을 무효화합니다.

페이지 제목 또는 다른 텍스트를 그림으로 렌더링하는 경우 페이지에서 콘텐츠 업데이트 시 삭제되도록 파일을 저장하는 것이 좋습니다.

1. 이미지 파일을 페이지와 동일한 폴더에 넣습니다.
1. 이미지 파일에 대해 다음 이름 지정 형식을 사용합니다.

   `<page file name>.<image file name>`

예를 들어 myPage.title.gif 파일에 페이지 제목을 저장할 수 있습니다. 페이지가 업데이트되면 이 파일은 자동으로 삭제되므로 페이지 제목을 변경하면 자동으로 캐시에 반영됩니다.

>[!NOTE]
>
>이미지 파일이 반드시 AEM 인스턴스에 존재하지 않을 수도 있습니다. 이미지 파일을 동적으로 만드는 스크립트를 사용할 수 있습니다. 그러면 Dispatcher가 웹 서버에 파일을 저장합니다.

## 탐색에 사용된 이미지 파일 무효화 {#invalidating-image-files-used-for-navigation}

탐색 항목에 그림을 사용하는 경우 이 방법은 기본적으로 제목과 같으며, 약간 더 복잡합니다. 대상 페이지가 있는 모든 탐색 이미지를 저장합니다. 일반 및 활성에 두 개의 그림을 사용하는 경우 다음 스크립트를 사용할 수 있습니다.

* 페이지를 정상적으로 표시하는 스크립트입니다.
* &quot;.normal&quot; 요청을 처리하고 일반 그림을 반환하는 스크립트.
* &quot;.active&quot; 요청을 처리하고 활성화된 그림을 반환하는 스크립트입니다.

페이지와 동일한 이름 지정 핸들로 이러한 그림을 만드는 것이 중요합니다. 콘텐츠 업데이트가 페이지와 이 그림을 삭제하도록 하려면

페이지가 일반적으로 자동 무효화되더라도 수정되지 않은 페이지의 경우 사진은 여전히 캐시에 남아 있습니다.

## 개인화 {#personalization}

Dispatcher는 개인화된 데이터를 캐시할 수 없으므로 개인화를 필요한 위치로 제한하는 것이 좋습니다. 이유는 다음과 같습니다.

* 자유롭게 사용자 정의 가능한 시작 페이지를 사용하는 경우 사용자가 요청할 때마다 해당 페이지를 작성해야 합니다.
* 반면, 10개의 다른 시작 페이지를 선택할 수 있는 경우 각 시작 페이지를 캐싱할 수 있으므로 성능이 향상됩니다.

>[!NOTE]
>
>각 페이지를 개인화하는 경우(예: 제목 표시줄에 사용자 이름을 표시하는 방법) 캐싱할 수 없으므로 큰 성능에 영향을 줄 수 있습니다.
>
>그러나 이 작업을 수행해야 하는 경우 다음을 수행할 수 있습니다.
>
>* iFrame을 사용하여 페이지를 모든 사용자에 대해 동일한 한 부분과 사용자의 모든 페이지에 대해 동일한 한 부분으로 분할합니다. 그런 다음 이 두 부품을 캐싱할 수 있습니다.
>* 클라이언트측 JavaScript를 사용하여 개인화된 정보를 표시합니다. 그러나 사용자가 JavaScript를 해제할 경우 페이지가 여전히 올바르게 표시되는지 확인해야 합니다.

>



## 고정 연결 {#sticky-connections}

[고정 ](dispatcher.md#TheBenefitsofLoadBalancing) 연결한 명의 사용자에 대한 문서가 모두 동일한 서버에서 구성되었는지 확인합니다. 사용자가 이 폴더를 벗어나서 나중에 해당 폴더로 돌아오면 연결이 계속 유지됩니다. 웹 사이트에 고정 연결이 필요한 모든 문서를 보관할 하나의 폴더를 정의합니다. 다른 문서가 들어 있지 않도록 하세요. 이는 개인화된 페이지 및 세션 데이터를 사용하는 경우 로드 밸런싱에 영향을 줍니다.

## MIME 유형 {#mime-types}

브라우저가 파일 유형을 결정할 수 있는 방법에는 두 가지가 있습니다.

1. 확장 프로그램(예:.html, .gif, .jpg 등)
1. 서버가 파일과 함께 보내는 MIME 유형에 의해

대부분의 파일의 경우 MIME 유형은 파일 확장명에 암시되어 있습니다. i.e.:

1. 확장 프로그램(예:.html, .gif, .jpg 등)
1. 서버가 파일과 함께 보내는 MIME 유형에 의해

파일 이름에 확장명이 없으면 일반 텍스트로 표시됩니다.

MIME 유형은 HTTP 헤더의 일부이며, 따라서 Dispatcher가 캐시하지 않습니다. AEM 응용 프로그램이 파일 끝은 인식할 수 없지만 대신 MIME 유형을 사용하는 파일을 반환하면 이러한 파일이 잘못 표시될 수 있습니다.

파일이 제대로 캐시되는지 확인하려면 다음 지침을 따르십시오.

* 파일의 확장자가 항상 적절한지 확인합니다.
* download.jsp?file=2214와 같은 URL이 있는 일반 파일 서버 스크립트는 사용하지 마십시오. 파일 사양을 포함하는 URL을 사용하도록 스크립트를 다시 작성합니다.이전 예제의 경우 다운로드.2214.pdf입니다.

