---
title: 캐시 성능을 위한 웹 사이트 최적화
seo-title: 캐시 성능을 위한 웹 사이트 최적화
description: 웹 사이트를 디자인하여 캐싱의 이점을 극대화하는 방법을 살펴봅니다.
seo-description: Dispatcher는 성능을 최적화하는 데 사용할 수 있는 다양한 메커니즘을 제공합니다. 웹 사이트를 디자인하여 캐싱의 이점을 극대화하는 방법을 살펴봅니다.
uuid: 2 d 4114 d 1-f 464-4 e 10-b 25 c-a 1 b 9 a 9 c 715 d 1
contentOwner: 사용자
products: sg_ Experiencemanager/dispatcher
topic-tags: Dispatcher
content-type: 참조
discoiquuid: BA 323503-1494-4048-941 D-C 1 D 14 F 2 E 85 B 2
redirecttarget: https://helpx.adobe.com/experience-manager/6-4/sites/deploying/using/configuring-performance.html
index: y
internal: n
snippet: y
translation-type: tm+mt
source-git-commit: f35c79b487454059062aca6a7c989d5ab2afaf7b

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
>발송자 버전은 AEM와 독립적입니다. 이전 버전의 AEM에 대한 설명서에 포함된 Dispatcher 설명서에 대한 링크를 팔로우한 경우 이 페이지로 리디렉션되었을 수 있습니다.

Dispatcher는 성능을 최적화하는 데 사용할 수 있는 다양한 메커니즘을 제공합니다. 이 섹션에서는 캐싱 이점을 극대화하기 위해 웹 사이트를 디자인하는 방법을 설명합니다.

>[!NOTE]
>
>디스패처가 표준 웹 서버에 캐시를 저장한다는 점을 기억해야 합니다. 이것은 다음을 의미합니다.
>
>* 페이지로 저장하고 URL를 사용하여 요청할 수 있는 모든 내용을 캐시할 수 있음
>* HTTP 헤더, 쿠키, 세션 데이터 및 양식 데이터와 같은 다른 항목을 저장할 수 없습니다.
>
>
일반적으로 많은 캐싱 전략에는 좋은 URL를 선택하는 것이 포함되며 이 추가 데이터에 의존하지 않습니다.

## 일관된 페이지 인코딩 사용 {#using-consistent-page-encoding}

HTTP 요청 헤더는 캐시되지 않으므로 헤더에 페이지 인코딩 정보를 저장하는 경우 문제가 발생할 수 있습니다. 이 경우, 디스패처가 캐시에서 페이지를 제공하면 웹 서버의 기본 인코딩이 페이지에 사용됩니다. 다음 두 가지 방법으로 이 문제를 해결할 수 있습니다.

* 인코딩을 하나만 사용하는 경우 웹 서버에 사용되는 인코딩이 AEM 웹 사이트의 기본 인코딩과 동일한지 확인합니다.
* 다음 `<META>` 예와 같이 HTML `head` 섹션의 태그를 사용하여 인코딩을 설정합니다.

```xml
        <META http-equiv="Content-Type" content="text/html; charset=EUC-JP">
```

## URL 매개 변수 방지 {#avoid-url-parameters}

가능하면 캐시할 페이지의 URL 매개 변수를 피하십시오. 예를 들어 사진 갤러리가 있는 경우, 다음 URL 이 캐시되지 않습니다 (디스패처가 적절하게 [](dispatcher-configuration.md#main-pars_title_24)구성되지 않은 경우).

```xml
www.myCompany.com/pictures/gallery.html?event=christmas&amp;page=1
```

그러나 다음과 같이 이러한 매개 변수를 페이지 URL에 배치할 수 있습니다.

```xml
www.myCompany.com/pictures/gallery.christmas.1.html
```

>[!NOTE]
>
>이 URL는 gallery. html와 동일한 페이지와 동일한 템플릿을 호출합니다. 템플릿 정의에서 페이지를 렌더링하는 스크립트를 지정하거나 모든 페이지에 동일한 스크립트를 사용할 수 있습니다.

## URL로 사용자 정의 {#customize-by-url}

사용자가 글꼴 크기 (또는 기타 레이아웃 사용자 정의) 를 변경할 수 있도록 허용하는 경우 다른 사용자 정의 내용이 URL에 반영되었는지 확인하십시오.

예를 들어 쿠키는 캐시되지 않으므로 쿠키 (또는 유사한 메커니즘) 에 글꼴 크기를 저장하면 캐시된 페이지에 대해 글꼴 크기가 유지되지 않습니다. 그 결과, Dispatcher는 임의 글꼴 크기의 문서를 임의로 반환합니다.

URL에 선택기로 글꼴 크기를 포함하면 이 문제가 발생하지 않습니다.

```xml
www.myCompany.com/news/main.large.html
```

>[!NOTE]
>
>대부분의 레이아웃 측면에서는 스타일 시트 및/또는 클라이언트측 스크립트를 사용할 수도 있습니다. 이러한 기능은 일반적으로 캐싱을 통해 완벽하게 작동합니다.
>
>이 기능은 다음과 같은 URL를 사용할 수 있는 인쇄 버전에서 유용합니다. «
>
>`www.myCompany.com/news/main.print.html`
>
>템플릿 정의의 스크립트 Globbing를 사용하여 인쇄 페이지를 렌더링하는 별도의 스크립트를 지정할 수 있습니다.

## 제목으로 사용된 이미지 파일 무효화 {#invalidating-image-files-used-as-titles}

페이지 제목 또는 다른 텍스트를 그림으로 렌더링하면 해당 파일을 페이지의 컨텐츠 업데이트 시 삭제되도록 저장하는 것이 좋습니다.

1. 이미지 파일을 페이지와 동일한 폴더에 배치합니다.
1. 이미지 파일에 다음 이름 지정 형식을 사용합니다.

   `<page file name>.<image file name>`

예를 들어 페이지 mypage. title. gif 파일에 myPage.html 페이지의 제목을 저장할 수 있습니다. 페이지가 업데이트되면 이 파일은 자동으로 삭제되므로 페이지 제목을 변경하면 자동으로 캐시에 반영됩니다.

>[!NOTE]
>
>이미지 파일이 AEM 인스턴스에 물리적으로 존재하는 것은 아닙니다. 이미지 파일을 동적으로 만드는 스크립트를 사용할 수 있습니다. 그러면 Dispatcher가 웹 서버에 파일을 저장합니다.

## 탐색에 사용된 이미지 파일 무효화 {#invalidating-image-files-used-for-navigation}

탐색 항목에 사진을 사용하는 경우 기본적으로 제목과 동일하며 약간 복잡합니다. 모든 탐색 이미지를 대상 페이지에 저장합니다. 정상 및 활성 사진에 두 개의 사진을 사용하는 경우 다음 스크립트를 사용할 수 있습니다.

* 페이지를 정상적으로 표시하는 스크립트입니다.
* &quot;. normal &quot;을 처리하는 스크립트는 일반 그림을 요청하고 반환합니다.
* &quot;. active &quot;를 처리하는 스크립트는 활성화된 사진을 요청하고 반환합니다.

컨텐츠 업데이트가 페이지뿐만 아니라 이러한 사진도 삭제하도록 하려면 동일한 명명 핸들을 사용하여 이러한 사진을 만드는 것이 중요합니다.

페이지가 자동으로 무효화되더라도 수정되지 않은 페이지의 경우 사진은 여전히 캐시에 남아 있습니다.

## 개인화 {#personalization}

디스패처는 개인화된 데이터를 캐시할 수 없으므로 필요한 위치로 개인화를 제한하는 것이 좋습니다. 이유를 설명합니다.

* 자유롭게 사용자 정의 가능한 시작 페이지를 사용하는 경우 사용자가 요청할 때마다 해당 페이지를 구성해야 합니다.
* 반면, 10 개의 서로 다른 시작 페이지를 제공하는 경우 각 페이지 중 하나를 캐시하여 성능을 향상시킬 수 있습니다.

>[!NOTE]
>
>각 페이지를 개인화하는 경우 (예: 사용자의 이름을 제목 표시줄에 넣는 경우) 캐시할 수 없으므로 성능이 크게 저하될 수 있습니다.
>
>그러나 이렇게 해야 하는 경우 다음을 수행할 수 있습니다.
>
>* iframe를 사용하여 페이지를 모든 사용자와 사용자의 모든 페이지에 대해 동일한 한 부분으로 분할합니다. 그런 다음 두 부분을 모두 캐시할 수 있습니다.
>* 클라이언트측 JavaScript를 사용하여 개인화된 정보를 표시할 수 있습니다. 그러나 사용자가 JavaScript를 끄는 경우 페이지가 올바르게 표시되는지 확인해야 합니다.
>



## 일관된 연결 {#sticky-connections}

[고정 연결은](dispatcher.md#TheBenefitsofLoadBalancing) 한 사용자에 대한 문서가 모두 동일한 서버에 작성되도록 합니다. 사용자가 이 폴더를 떠나고 나중에 다시 돌아갈 경우 연결은 계속 유지됩니다. 하나의 폴더를 정의하여 웹 사이트에 대한 일관된 연결이 필요한 모든 문서를 보관할 수 있습니다. 다른 문서가 들어 있지 않도록 해보십시오. 이렇게 하면 개인화된 페이지 및 세션 데이터를 사용하는 경우 로드 밸런싱이 영향을 받습니다.

## MIME 유형 {#mime-types}

브라우저에서 파일 유형을 결정할 수 있는 방법에는 두 가지가 있습니다.

1. By its extension (e. g.html. gif.jpg 등)
1. 서버가 파일과 함께 보내는 MIME 유형으로,

대부분의 파일의 경우 파일 확장명에 MIME-type 이 내포되어 있습니다. 예:

1. By its extension (e. g.html. gif.jpg 등)
1. 서버가 파일과 함께 보내는 MIME 유형으로,

파일 이름에 확장명이 없으면 일반 텍스트로 표시됩니다.

MIME 형식은 HTTP 헤더의 일부이므로, 디스패처는 이를 캐시하지 않습니다. AEM 애플리케이션에서 인식된 파일이 없지만 MIME 유형에 의존하는 파일을 반환하는 경우 이러한 파일이 잘못 표시될 수 있습니다.

파일이 제대로 캐시되었는지 확인하려면 다음 지침을 따르십시오.

* 파일 확장명이 항상 올바른지 확인합니다.
* 다운로드. jsp와 같은 URL 이 있는 범용 파일 제공 스크립트를 사용하지 마십시오. file = 2214. 파일 사양이 들어 있는 URL를 사용할 스크립트를 다시 작성합니다. 이전 예제의 경우 .2214. pdf를 다운로드하십시오.

