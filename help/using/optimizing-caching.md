---
title: 캐시 성능을 위한 웹 사이트 최적화
seo-title: 캐시 성능을 위한 웹 사이트 최적화
description: 캐싱의 이점을 극대화하기 위해 웹 사이트를 디자인하는 방법을 살펴볼 수 있습니다.
seo-description: Dispatcher는 성능을 최적화하는 데 사용할 수 있는 다양한 내장 메커니즘을 제공합니다. 캐싱의 이점을 극대화하기 위해 웹 사이트를 디자인하는 방법을 살펴볼 수 있습니다.
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
translation-type: tm+mt
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

Dispatcher는 성능을 최적화하는 데 사용할 수 있는 다양한 내장 메커니즘을 제공합니다. 이 섹션에서는 캐싱의 이점을 극대화하기 위해 웹 사이트를 디자인하는 방법을 설명합니다.

>[!NOTE]
>
>디스패처가 표준 웹 서버에 캐시를 저장한다는 사실을 기억하는 데 도움이 될 수 있습니다. 즉,
>
>* 페이지로 저장할 수 있는 모든 것을 캐싱하고 URL을 사용하여 요청할 수 있습니다.
>* HTTP 헤더, 쿠키, 세션 데이터 및 양식 데이터와 같은 다른 항목은 저장할 수 없습니다.

>
>
일반적으로 많은 캐싱 전략은 좋은 URL을 선택하는 것과 이 추가 데이터에 의존하지 않는 것입니다.

## 일관된 페이지 인코딩 사용 {#using-consistent-page-encoding}

HTTP 요청 헤더는 캐시되지 않으므로 헤더에 페이지 인코딩 정보를 저장하는 경우 문제가 발생할 수 있습니다. 이 경우 Dispatcher가 캐시에서 페이지를 제공하면 웹 서버의 기본 인코딩이 페이지에 사용됩니다. 다음 두 가지 방법으로 이 문제를 해결할 수 있습니다.

* 인코딩을 하나만 사용하는 경우 웹 서버에서 사용되는 인코딩이 AEM 웹 사이트의 기본 인코딩과 동일한지 확인하십시오.
* 다음 예와 같이 HTML `head` 섹션의 `<META>` 태그를 사용하여 인코딩을 설정합니다.

```xml
        <META http-equiv="Content-Type" content="text/html; charset=EUC-JP">
```

## URL 매개 변수 {#avoid-url-parameters} 방지

가능하면 캐시하려는 페이지의 URL 매개 변수를 피하십시오. 예를 들어 그림 갤러리가 있는 경우 다음 URL은 캐시되지 않습니다(Dispatcher가 [그에 따라 구성된 경우는 제외).](dispatcher-configuration.md#main-pars_title_24)

```xml
www.myCompany.com/pictures/gallery.html?event=christmas&amp;page=1
```

그러나 다음과 같이 페이지 URL에 이러한 매개 변수를 배치할 수 있습니다.

```xml
www.myCompany.com/pictures/gallery.christmas.1.html
```

>[!NOTE]
>
>이 URL은 gallery.html과 동일한 페이지와 동일한 템플릿을 호출합니다. 템플릿 정의에서 페이지를 렌더링하는 스크립트를 지정하거나 모든 페이지에 동일한 스크립트를 사용할 수 있습니다.

## URL {#customize-by-url}로 사용자 지정

사용자가 글꼴 크기(또는 기타 레이아웃 사용자 정의)를 변경할 수 있도록 허용하는 경우 다른 사용자 정의 내용이 URL에 반영되어 있는지 확인하십시오.

예를 들어 쿠키는 캐시되지 않으므로 글꼴 크기를 쿠키(또는 유사한 메커니즘)에 저장하는 경우, 캐시된 페이지에 대해서는 글꼴 크기가 유지되지 않습니다. 결과적으로 Dispatcher는 글꼴 크기의 문서를 임의로 반환합니다.

선택기로 URL에 글꼴 크기를 포함하면 이 문제가 발생하지 않습니다.

```xml
www.myCompany.com/news/main.large.html
```

>[!NOTE]
>
>대부분의 레이아웃 측면에서는 스타일 시트 및/또는 클라이언트측 스크립트를 사용할 수도 있습니다. 이러한 기능은 일반적으로 캐싱 작업에서 잘 작동합니다.
>
>다음과 같은 URL을 사용할 수 있는 인쇄 버전에도 유용합니다.&quot;
>
>`www.myCompany.com/news/main.print.html`
>
>템플릿 정의의 스크립트 글로브를 사용하여 인쇄 페이지를 렌더링하는 별도의 스크립트를 지정할 수 있습니다.

## 제목 {#invalidating-image-files-used-as-titles}으로 사용된 이미지 파일을 무효화합니다.

페이지 제목 또는 기타 텍스트를 이미지로 렌더링하는 경우 페이지의 컨텐츠 업데이트 시 삭제되도록 파일을 저장하는 것이 좋습니다.

1. 페이지와 동일한 폴더에 이미지 파일을 배치합니다.
1. 이미지 파일에 다음 이름 지정 형식을 사용합니다.

   `<page file name>.<image file name>`

예를 들어 페이지 myPage.html의 제목을 myPage.title.gif 파일에 저장할 수 있습니다. 페이지가 업데이트되면 이 파일은 자동으로 삭제되므로 페이지 제목의 모든 변경 사항이 캐시에 자동으로 반영됩니다.

>[!NOTE]
>
>AEM 인스턴스에 이미지 파일이 반드시 실제로 존재하는 것은 아닙니다. 이미지 파일을 동적으로 만드는 스크립트를 사용할 수 있습니다. 그런 다음 Dispatcher가 웹 서버에 파일을 저장합니다.

## 탐색에 사용된 이미지 파일 무효화{#invalidating-image-files-used-for-navigation}

탐색 항목에 사진을 사용하는 경우 방법은 기본적으로 제목과 같으며 약간 더 복잡합니다. 모든 내비게이션 이미지를 대상 페이지에 저장합니다. 일반 및 활성 상태를 위해 두 개의 사진을 사용하는 경우 다음 스크립트를 사용할 수 있습니다.

* 페이지를 보통으로 표시하는 스크립트입니다.
* &quot;.normal&quot; 요청을 처리하고 일반 그림을 반환하는 스크립트입니다.
* &quot;.active&quot; 요청을 처리하고 활성화된 사진을 반환하는 스크립트입니다.

콘텐츠에서 페이지 뿐만 아니라 이 그림들을 삭제하도록 하려면 페이지와 동일한 이름 지정 아이디로 이 그림을 만들어야 합니다.

수정되지 않은 페이지의 경우 페이지 자체가 일반적으로 자동으로 무효화되더라도 이미지는 여전히 캐시에 남아 있습니다.

## 개인화 {#personalization}

Dispatcher는 개인화된 데이터를 캐시할 수 없으므로 개인화를 필요한 위치로 제한하는 것이 좋습니다. 이유를 설명하기 위해:

* 자유롭게 사용자 정의 가능한 시작 페이지를 사용하는 경우 사용자가 페이지를 요청할 때마다 해당 페이지를 구성해야 합니다.
* 반면, 10개의 서로 다른 시작 페이지를 선택할 수 있는 경우 각 시작 페이지를 캐시하여 성능을 향상시킬 수 있습니다.

>[!NOTE]
>
>제목 표시줄에 사용자 이름을 넣는 등 각 페이지를 개인화하면 캐시할 수 없으므로 성능에 큰 영향을 줄 수 있습니다.
>
>그러나 이 작업을 수행해야 하는 경우 다음을 수행할 수 있습니다.
>
>* iFrame을 사용하여 페이지를 모든 사용자에 대해 동일한 한 부분으로 분할하고 사용자의 모든 페이지에 대해 동일한 한 부분으로 분할할 수 있습니다. 그런 다음 두 부품을 캐싱할 수 있습니다.
>* 클라이언트측 JavaScript를 사용하여 개인화된 정보를 표시할 수 있습니다. 그러나 사용자가 JavaScript를 끄는 경우 페이지가 여전히 올바로 표시되는지 확인해야 합니다.

>



## 고정 연결 {#sticky-connections}

[고정 ](dispatcher.md#TheBenefitsofLoadBalancing) 연결한 사용자에 대한 문서가 모두 동일한 서버에서 작성되는지 확인합니다. 사용자가 이 폴더를 나갔다가 나중에 다시 돌아오면 연결이 계속 유지됩니다. 웹 사이트에 대해 고정 연결이 필요한 모든 문서를 보관할 하나의 폴더를 정의합니다. 다른 문서를 포함하지 마십시오. 이는 개인화된 페이지 및 세션 데이터를 사용하는 경우 로드 밸런싱에 영향을 줍니다.

## MIME 형식 {#mime-types}

브라우저에서 파일 유형을 결정하는 방법에는 두 가지가 있습니다.

1. 확장자(예:.html, .gif, .jpg 등)
1. 서버가 파일과 함께 보내는 MIME 형식을 기준으로 합니다.

대부분의 파일의 경우 MIME 형식은 파일 확장명에 함축되어 있습니다. i.e.:

1. 확장자(예:.html, .gif, .jpg 등)
1. 서버가 파일과 함께 보내는 MIME 형식을 기준으로 합니다.

파일 이름에 확장자가 없으면 일반 텍스트로 표시됩니다.

MIME 형식은 HTTP 헤더의 일부이며, 따라서 디스패처는 이를 캐시하지 않습니다. AEM 응용 프로그램에서 파일 종료 시간이 인식되지 않았지만 MIME 형식을 사용하는 파일을 반환하는 경우 이러한 파일이 잘못 표시될 수 있습니다.

파일이 제대로 캐시되는지 확인하려면 다음 지침을 따르십시오.

* 파일의 확장자가 항상 올바른지 확인하십시오.
* download.jsp?file=2214와 같은 URL이 있는 일반 파일 서버 스크립트를 사용하지 마십시오. 파일 사양을 포함하는 URL을 사용하도록 스크립트를 다시 작성하십시오.이전 예제의 경우 download.2214.pdf가 됩니다.

