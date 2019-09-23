---
title: 발송자 개요
seo-title: Adobe AEM Dispatcher 개요
description: 이 문서에서는 Dispatcher에 대한 일반적인 개요를 제공합니다.
seo-description: 이 문서에서는 Adobe Experience Manager Dispatcher에 대한 일반적인 개요를 제공합니다.
uuid: 71766f86-5e91-446b-a078-061b179d090d
pageversionid: '1193211344162'
topic-tags: dispatcher
content-type: 참조
discoiquuid: 1d449ee2-4cdd-4b7a-8b4e-7e6fc0a1d7ee
translation-type: tm+mt
source-git-commit: de6a513baf3e6b1a1463a442fa840e59f2196e8e

---


# 발송자 개요 {#dispatcher-overview}

>[!NOTE]
>
>발송자 버전은 AEM과 독립적입니다. 이전 버전의 AEM에 대한 설명서에 포함된 Dispatcher 설명서에 대한 링크를 따라간 경우 이 페이지로 리디렉션되었을 수 있습니다.

Dispatcher는 Adobe Experience Manager의 캐싱 및/또는 로드 밸런싱 도구입니다. AEM의 Dispatcher를 사용하면 AEM 서버를 공격으로부터 보호할 수 있습니다. 따라서 Dispatcher를 엔터프라이즈급 웹 서버와 함께 사용하여 AEM 인스턴스의 보안을 강화할 수 있습니다.

디스패처를 배포하는 프로세스는 선택한 웹 서버 및 OS 플랫폼과 독립적입니다.

1. Dispatcher에 대해 알아보십시오(이 페이지). 또한 디스패처에 대한 [FAQ를 참조하십시오](https://helpx.adobe.com/experience-manager/using/dispatcher-faq.html).
1. 웹 서버 설명서에 따라 [지원되는 웹 서버를](https://helpx.adobe.com/experience-manager/6-3/sites/deploying/using/technical-requirements.html) 설치합니다.

1. [웹 서버에 Dispatcher 모듈을](dispatcher-install.md) 설치하고 그에 따라 웹 서버를 구성합니다.
1. [디스패처](dispatcher-configuration.md) 구성(dispatcher.any 파일).

1. [컨텐츠 업데이트가](page-invalidate.md) 캐시를 무효화하도록 AEM을 구성합니다.

>[!NOTE]
>
>AEM에서 Dispatcher가 작동하는 방식을 자세히 알아보려면 2017년 7월 AEM [커뮤니티 전문가에게 물어보십시오](https://bit.ly/ATACE0717).

필요에 따라 다음 정보를 사용하십시오.

* [Dispatcher 보안 검사 목록](security-checklist.md)
* [발송자 기술 자료](https://helpx.adobe.com/cq/kb/index/dispatcher.html)
* [캐시 성능을 위한 웹 사이트 최적화](https://helpx.adobe.com/experience-manager/6-4/sites/deploying/using/configuring-performance.html)
* [여러 도메인에 Dispatcher 사용](dispatcher-domains.md)
* [Dispatcher에서 SSL 사용](dispatcher-ssl.md)
* [권한 구분 캐싱 구현](permissions-cache.md)
* [발송자 문제 해결](dispatcher-troubleshooting.md)
* [발송자 주요 문제 FAQ](dispatcher-faq.md)

>[!NOTE]
>
>**Dispatcher의 가장 일반적인** 사용은 AEM **게시 인스턴스의**&#x200B;응답을 캐시하여 외부에서 게시되는 웹 사이트의 응답성과 보안을 향상시키는 것입니다. 대부분의 토론은 이 사건에 초점을 맞추고 있다.
>
>그러나 Dispatcher를 사용하면 **작성자 인스턴스의**&#x200B;응답성을 높일 수 있습니다. 특히 많은 사용자가 웹 사이트를 편집하고 업데이트할 수 있습니다. 이 사례에 대한 자세한 내용은 [아래의 작성자 서버와 함께 Dispatcher](#using-a-dispatcher-with-an-author-server)사용을 참조하십시오.

## Dispatcher를 사용하여 캐시를 구현하는 이유는 무엇입니까? {#why-use-dispatcher-to-implement-caching}

웹 퍼블리싱에는 두 가지 기본적인 방법이 있습니다.

* **정적 웹 서버**:아파치나 IIS와 같이 매우 간단하지만 속도가 빠릅니다.
* **컨텐츠 관리 서버**:동적, 실시간, 지능형 컨텐츠를 제공하지만 보다 많은 계산 시간과 기타 리소스가 필요합니다.

Dispatcher를 사용하면 빠르고 동적인 환경을 구현할 수 있습니다. Apache와 같은 정적 HTML 서버의 일부로서 다음 목적을 위해 작동합니다.

* 정적 웹 사이트의 형태로 사이트 컨텐츠를 최대한 저장(또는 "캐싱")
* 가능한 한 거의 모든 레이아웃 엔진에 액세스합니다.

즉,

* **정적 컨텐츠는** 정적 웹 서버와 동일한 속도와 간편한 방식으로 처리되며&#x200B;*또한 정적 웹 서버에 사용할 수 있는 관리 및 보안 도구를 사용할 수 있습니다*.

* **동적 컨텐츠는** 필요한 만큼 시스템 속도를 저하시키지 않고 필요에 따라 생성됩니다.

디스패처에는 동적 사이트의 컨텐츠를 기반으로 정적 HTML을 생성하고 업데이트하는 메커니즘이 포함되어 있습니다. 정적 파일로 저장되는 문서와 항상 동적으로 생성되는 문서를 자세히 지정할 수 있습니다.

이 조항은 이 이면의 원칙을 보여줍니다.

### 정적 웹 서버 {#static-web-server}

![](assets/chlimage_1-3.png)

Apache 또는 IIS와 같은 정적 웹 서버는 웹 사이트 방문자에게 정적 HTML 파일을 제공합니다. 정적 페이지는 한 번 생성되므로 각 요청에 대해 동일한 컨텐츠가 전달됩니다.

이 과정은 매우 간단해서 매우 효율적이다. 방문자가 파일(예: HTML 페이지)을 요청하는 경우 파일은 대개 메모리에서 직접 가져오며, 최악의 경우 로컬 드라이브에서 읽습니다. 정적인 웹 서버는 오랫동안 제공되어 왔으며, 관리와 보안 관리를 위한 다양한 툴이 있으며, 네트워크 인프라와 매우 잘 통합되었습니다.

### 컨텐츠 관리 서버 {#content-management-servers}

![](assets/chlimage_1-4.png)

AEM과 같은 컨텐츠 관리 서버를 사용하는 경우 고급 레이아웃 엔진은 방문자의 요청을 처리합니다. 엔진은 스타일, 형식 및 액세스 권한과 함께 저장소에서 컨텐츠를 읽어 방문자의 요구 사항과 권한에 맞는 문서로 변환합니다.

이를 통해 보다 풍부하고 다이내믹한 컨텐츠를 제작할 수 있으므로 웹 사이트의 유연성과 기능을 향상시킬 수 있습니다. 그러나 레이아웃 엔진은 정적 서버보다 처리 능력이 더 필요하므로 많은 방문자가 시스템을 사용하는 경우 이 설정을 지연시킬 수 있습니다.

## Dispatcher에서 캐싱을 수행하는 방법 {#how-dispatcher-performs-caching}

![](assets/chlimage_1-5.png)

**캐시 디렉토리** 캐싱을 위해 디스패처 모듈은 정적 컨텐츠를 제공하는 웹 서버의 기능을 사용합니다. 디스패처는 캐시된 문서를 웹 서버의 문서 루트에 배치합니다.

>[!NOTE]
>
>HTTP 헤더 캐싱에 대한 구성이 부족한 경우 Dispatcher는 페이지의 HTML 코드만 저장합니다. 이 코드는 HTTP 헤더를 저장하지 않습니다. 웹 사이트 내에서 다른 인코딩을 사용하는 경우 이러한 인코딩이 손실될 수 있으므로 문제가 될 수 있습니다. HTTP 헤더 캐시를 활성화하려면 발송자 [캐시 구성을 참조하십시오.](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html)

>[!NOTE]
>
>NAS(Network Attached Storage)에서 웹 서버의 문서 루트를 찾아 성능이 저하됩니다. 또한 NAS에 있는 문서 루트가 여러 웹 서버 간에 공유되면 복제 작업이 수행될 때 간헐적으로 잠금이 발생할 수 있습니다.

>[!NOTE]
>
>디스패처는 캐시된 문서를 요청된 URL과 같은 구조로 저장합니다.
>
>파일 이름의 길이에 대한 OS 수준의 제한이 있을 수 있습니다.예를 들어 선택기가 많은 URL이 있는 경우

### 캐싱 방법

Dispatcher에는 웹 사이트를 변경할 때 캐시 컨텐츠를 업데이트하는 두 가지 기본 방법이 있습니다.

* **컨텐츠** 업데이트는 변경된 페이지와 해당 페이지와 직접 연결된 파일을 제거합니다.
* **자동 무효화는** 업데이트 후 오래된 캐시 부분을 자동으로 무효화합니다. 즉, 관련 페이지를 삭제하지 않고 최신 상태가 아닌 것으로 효과적으로 플래그를 지정합니다.

### 컨텐츠 업데이트

컨텐츠 업데이트에서 하나 이상의 AEM 문서가 변경됩니다. AEM 파섹

1. 캐시에서 수정된 파일을 삭제합니다.
1. 캐시에서 동일한 핸들을 사용하여 시작하는 모든 파일을 삭제합니다. 예를 들어 /en/index.html 파일이 업데이트되면 /en/index로 시작하는 모든 파일이 업데이트됩니다. 가 삭제됩니다. 이 메커니즘을 사용하면 특히 사진 탐색과 관련하여 캐시 효율성이 높은 사이트를 디자인할 수 있습니다.
1. 소위 *stfile* 을 **만지는**&#x200B;것입니다.그러면 상태 파일의 타임스탬프가 업데이트되어 마지막 변경 날짜의 날짜가 표시됩니다.

다음 사항을 명시해야 합니다.

* 컨텐츠 업데이트는 일반적으로 대체해야 하는 내용을 "알고 있는" 작성 시스템과 함께 사용됩니다.
* 컨텐츠 업데이트의 영향을 받는 파일은 제거되지만 즉시 교체되지는 않습니다. 다음에 파일이 요청되면 디스패처는 AEM 인스턴스에서 새 파일을 가져와 캐시에 저장하여 이전 컨텐츠를 덮어씁니다.
* 일반적으로 페이지의 텍스트를 통합하는 자동으로 생성된 사진은 동일한 핸들을 사용하여 시작하는 그림 파일에 저장되므로 연결을 삭제하도록 합니다. 예를 들어 페이지 mypage.html의 제목 텍스트를 이미지 mypage.titlePicture.gif로 동일한 폴더에 저장할 수 있습니다. 이렇게 하면 페이지가 업데이트될 때마다 캐시에서 그림이 자동으로 삭제되므로 사진이 항상 페이지의 현재 버전을 반영하도록 할 수 있습니다.
* 언어 폴더당 하나 등 여러 개의 상태 파일이 있을 수 있습니다. 페이지가 업데이트되면 AEM은 상태 파일이 포함된 다음 상위 폴더를 찾아 해당 파일을 *수정합니다* .

### 자동 무효화

자동 무효화는 파일을 실제로 삭제하지 않고 캐시의 일부를 자동으로 무효화합니다. 모든 컨텐츠 업데이트에서는 소위 상태 파일이 터치되므로 해당 타임스탬프가 마지막 컨텐츠 업데이트를 반영합니다.

Dispatcher에는 자동 무효화의 대상이 되는 파일 목록이 있습니다. 해당 목록의 문서가 요청되면 디스패처는 캐시된 문서의 날짜를 상태 파일의 타임스탬프와 비교합니다.

* 캐시된 문서가 최신 문서인 경우 Dispatcher가 반환합니다.
* 이전 버전인 경우 디스패처는 AEM 인스턴스에서 현재 버전을 검색합니다.

다시 한 번, 특정 요점을 명시해야 합니다.

* 자동 무효화는 일반적으로 HTML 페이지의 경우 상호 관계가 복잡할 때 사용됩니다. 이러한 페이지에는 링크 및 탐색 항목이 포함되어 있으므로 일반적으로 컨텐츠 업데이트 후에 업데이트해야 합니다. PDF 또는 사진 파일을 자동으로 생성한 경우 자동으로 무효화하도록 선택할 수 있습니다.
* 자동 무효화는 상태 파일을 수정하는 것 외에는 업데이트 시 디스패처가 수행하는 작업을 포함하지 않습니다. 그러나 상태 파일을 터치하면 캐시 컨텐츠가 캐시에서 물리적으로 제거되지 않고 자동으로 사용되지 않습니다.

## Dispatcher가 문서를 반환하는 방법 {#how-dispatcher-returns-documents}

![](assets/chlimage_1-6.png)

### 문서의 캐시 대상 여부 확인

Dispatcher가 구성 파일에서 [캐시하는 문서를](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html)정의할 수 있습니다. Dispatcher는 캐시 가능한 문서 목록에 대해 요청을 확인합니다. 문서가 이 목록에 없으면 디스패처가 AEM 인스턴스에서 문서를 요청합니다.

Dispatcher는 *항상* 다음 경우에 AEM 인스턴스에서 직접 문서를 요청합니다.

* 요청 URI에 물음표 "?"가 포함된 경우. 일반적으로 이것은 검색 결과와 같은 동적 페이지를 나타내며 캐시할 필요가 없습니다.
* 파일 확장자가 없습니다. 웹 서버에는 문서 유형(MIME 유형)을 결정하기 위한 확장이 필요합니다.
* 인증 헤더가 설정됨(구성할 수 있음)

>[!NOTE]
>
>GET 또는 HEAD(HTTP 헤더의 경우) 메서드는 Dispatcher에서 액세스할 수 있습니다. 응답 헤더 캐싱에 대한 자세한 내용은 HTTP 응답 [헤더 캐싱 섹션을 참조하십시오](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html) .

### 문서가 캐시되는지 확인

디스패처는 캐시된 파일을 정적 웹 사이트의 일부인 것처럼 웹 서버에 저장합니다. 사용자가 액세스 가능한 문서를 요청하면 Dispatcher는 해당 문서가 웹 서버의 파일 시스템에 있는지 여부를 확인합니다.

* 문서가 캐시되면 Dispatcher는 파일을 반환합니다.
* 캐시되지 않으면 디스패처는 AEM 인스턴스에서 문서를 요청합니다.

### 문서가 최신 문서인지 확인

문서가 최신 상태인지 확인하려면 Dispatcher에서 다음 두 단계를 수행합니다.

1. 문서가 자동 무효화의 대상이 되는지 확인합니다. 그렇지 않으면 문서가 최신 문서로 간주됩니다.
1. 문서가 자동 무효화되도록 구성된 경우 Dispatcher는 사용 가능한 마지막 변경 내용보다 오래되었는지 여부를 확인합니다. 이전 버전인 경우 디스패처는 AEM 인스턴스에서 현재 버전을 요청하고 캐시에 있는 버전을 대체합니다.

>[!NOTE]
>
>자동 **무효화가** 없는 문서는 물리적으로 삭제될 때까지 캐시에 남아 있습니다.예: 웹 사이트의 컨텐츠 업데이트

## 부하 균형 조정의 이점 {#the-benefits-of-load-balancing}

로드 밸런싱은 여러 AEM 인스턴스에 웹 사이트의 계산 로드를 배포하는 방법입니다.

![](assets/chlimage_1-7.png)

얻을 수 있는 이점:

* **향상된 처리 능력**&#x200B;실제로 이것은 디스패처가 여러 AEM 인스턴스 간에 문서 요청을 공유함을 의미합니다. 이제 각 인스턴스에 처리할 문서가 적으므로 응답 시간이 빨라집니다. Dispatcher는 각 문서 범주에 대한 내부 통계를 유지하므로 로드를 예측하고 쿼리를 효율적으로 배포할 수 있습니다.

* **실패 시 보장 범위가**&#x200B;증가함Dispatcher가 인스턴스에서 응답을 받지 않으면 요청을 자동으로 다른 인스턴스 중 하나로 전달합니다. 따라서 인스턴스를 사용할 수 없게 되면 그 유일한 효과는 계산 능력이 손실되는 정도에 비례하여 사이트의 둔화입니다. 그러나 모든 서비스는 계속 제공됩니다.

* 동일한 정적 웹 서버에서 서로 다른 웹 사이트를 관리할 수도 있습니다.

>[!NOTE]
>
>로드 밸런싱은 부하를 효율적으로 분산시키는 반면 캐싱은 부하를 줄이는 데 도움이 됩니다. 따라서 로드 밸런싱을 설정하기 전에 캐싱을 최적화하고 전체 로드를 줄이십시오. 캐싱이 좋을수록 로드 밸런서의 성능이 향상되거나 로드 밸런싱이 불필요하게 렌더링될 수 있습니다.

>[!CAUTION]
>
>단일 디스패처는 일반적으로 사용 가능한 게시 인스턴스의 용량을 채울 수 있지만 일부 드문 애플리케이션의 경우 두 Dispatcher 인스턴스 간의 로드 균형을 추가로 조정할 수 있습니다. 추가 디스패처가 사용 가능한 게시 인스턴스의 로드를 늘리고 대부분의 응용 프로그램의 성능을 쉽게 줄일 수 있으므로 여러 디스패처가 있는 구성을 신중하게 고려해야 합니다.

## 디스패처가 로드 밸런싱을 수행하는 방법 {#how-the-dispatcher-performs-load-balancing}

### 성능 통계

디스패처는 AEM의 각 인스턴스가 문서를 처리하는 속도에 대한 내부 통계를 유지합니다. 이 데이터를 기반으로, 디스패처는 요청에 응답할 때 가장 빠른 응답 시간을 제공할 인스턴스를 예측하여 해당 인스턴스에서 필요한 계산 시간을 예약합니다.

요청 유형마다 평균 완료 시간이 다를 수 있으므로 Dispatcher에서 문서 카테고리를 지정할 수 있습니다. 그런 다음 시간 예측을 계산할 때 고려됩니다. 예를 들어 일반적인 응답 시간이 다를 수 있으므로 HTML 페이지와 이미지를 구분할 수 있습니다.

정교한 검색 기능을 사용하는 경우 검색 쿼리에 대한 새 카테고리를 만들 수 있습니다. 이렇게 하면 Dispatcher가 검색 쿼리를 가장 빠르게 응답하는 인스턴스로 보낼 수 있습니다. 이렇게 하면 "값비싼" 검색 쿼리를 여러 개 받을 때 인스턴스가 지연되는 반면 다른 인스턴스는 "더 저렴한" 요청을 받습니다.

### 개인화된 컨텐츠(일관된 연결)

일관된 AEM 인스턴스에서 한 사용자에 대한 문서를 모두 작성할 수 있습니다. 이는 개인화된 페이지와 세션 데이터를 사용하는 경우에 중요합니다. 데이터는 인스턴스에 저장되므로 동일한 사용자의 후속 요청이 해당 인스턴스로 반환되거나 데이터가 유실됩니다.

고정 연결은 디스패처의 요청을 최적화하는 기능을 제한하므로 필요한 경우에만 사용해야 합니다. "고정" 문서를 포함하는 폴더를 지정할 수 있으므로 해당 폴더의 모든 문서가 각 사용자에 대해 동일한 인스턴스에 작성되도록 할 수 있습니다.

>[!NOTE]
>
>고정 연결을 사용하는 대부분의 페이지의 경우 캐싱을 해제해야 합니다. 그렇지 않으면 세션 컨텐츠에 관계없이 모든 사용자와 페이지가 동일하게 표시됩니다.
>
>일부 ** 애플리케이션의 경우 고정 연결과 캐싱을 모두 사용할 수 있습니다.예를 들어 데이터를 세션에 기록하는 양식을 표시하는 경우

## 여러 디스패처 사용 {#using-multiple-dispatchers}

복잡한 설정에서 여러 Dispatcher를 사용할 수 있습니다. 예를 들어 다음을 사용할 수 있습니다.

* 인트라넷에 웹 사이트 게시
* 다른 주소와 보안 설정이 다른 두 번째 디스패처가 동일한 컨텐츠를 인터넷에 게시합니다.

이러한 경우 각 요청이 하나의 Dispatcher만을 통과하는지 확인하십시오. Dispatcher는 다른 Dispatcher의 요청을 처리하지 않습니다. 따라서 Dispatcher가 모두 AEM 웹 사이트에 직접 액세스하도록 하십시오.

## CDN 파섹 {#using-dispatcher-with-a-cdn}

Akamai Edge Delivery 또는 Amazon Cloud Front와 같은 CDN(컨텐츠 전달 네트워크)은 최종 사용자에게 가까운 위치에서 컨텐츠를 제공합니다. By that

* 최종 사용자의 응답 시간 단축
* 서버 로드 해제

HTTP 인프라 구성 요소로서 CDN은 Dispatcher와 유사하게 작동합니다.cdn 노드가 요청을 받으면 가능한 경우 캐시에서 요청을 처리합니다(리소스는 캐시에서 사용할 수 있으며 유효합니다). 그렇지 않으면 리소스를 검색하고 적절한 경우 추가 요청을 위해 캐시하기 위해 다음으로 가장 가까운 서버에 연결됩니다.

"다음 가장 가까운 서버"는 특정 설정에 따라 다릅니다. 예를 들어 Akamai 설정에서 요청은 다음 경로를 취할 수 있습니다.

* Akamai 에지 노드
* Akamai Midgres 레이어
* 방화벽
* 부하 균형 조정기
* 디스패처
* AEM

대부분의 경우 Dispatcher는 캐시에서 문서를 제공하고 CDN 서버로 반환되는 응답 헤더에 영향을 줄 수 있는 다음 서버입니다.

## CDN 캐시 제어 {#controlling-a-cdn-cache}

CDN이 Dispatcher에서 재가져오기 전에 리소스를 캐시하는 시간을 제어하는 방법에는 여러 가지가 있습니다.

1. 명시적 구성\
   MIME 유형, 확장, 요청 유형 등에 따라 CDN의 캐시에서 특정 리소스가 얼마나 오래 보관되는지를 구성합니다.

1. 만료 및 캐시 제어 헤더\
   대부분의 CDN은 업스트림 서버에서 보내는 경우 `Expires:` 및 `Cache-Control:` HTTP 헤더를 처리합니다. 이러한 작업은 [mod_expires Apache 모듈을 사용하여 수행할](https://httpd.apache.org/docs/2.4/mod/mod_expires.html) 수 있습니다.

1. 수동 무효화\
   CDN을 사용하면 웹 인터페이스를 통해 캐시에서 리소스를 제거할 수 있습니다.
1. API 기반 무효화\
   대부분의 CDN은 캐시에서 리소스를 제거할 수 있는 REST 및/또는 SOAP API를 제공합니다.

일반적인 AEM 설정에서 확장 및/또는 경로별 구성은 위의 포인트 1과 2를 통해 수행할 수 있으며, 디자인 이미지 및 클라이언트 라이브러리와 같이 자주 변경되지 않는 자주 사용하는 리소스에 대해 적절한 캐싱 기간을 설정할 수 있습니다. 새 릴리스가 배포되면 일반적으로 수동 무효화가 필요합니다.

이 방법을 사용하여 관리 컨텐츠를 캐시하는 경우 구성된 캐싱 기간이 만료되고 문서를 다시 Dispatcher에서 가져오는 경우에만 컨텐츠 변경 사항이 최종 사용자에게 표시된다는 의미입니다.

보다 세밀하게 조정하기 위해 API 기반 무효화를 사용하면 디스패처 캐시가 무효화되므로 CDN 캐시를 무효화할 수 있습니다. CDNs API를 기반으로, 고유한 [ContentBuilder](https://docs.adobe.com/docs/en/cq/current/javadoc/com/day/cq/replication/ContentBuilder.html) 및 [TransportHandler](https://docs.adobe.com/docs/en/cq/current/javadoc/com/day/cq/replication/TransportHandler.html) (API가 REST 기반이 아닌 경우)를 구현하고CDN의 캐시를 무효화하는 데 사용할 복제 에이전트를 설정할 수 있습니다.

>[!NOTE]
>
>AEM( [CQ) Dispatcher Security 및 CDN+Browser](https://www.slideshare.net/andrewmkhoury/dispatcher-caching-aemgemspart2jan2015) 캐싱 및 Dispatcher Caching에 기록된 프레젠테이션을 [참조하십시오](https://docs.adobe.com/content/ddc/en/gems/dispatcher-caching---new-features-and-optimizations.html).

## 작성자 서버와 함께 디스패처 사용 {#using-a-dispatcher-with-an-author-server}

>[!CAUTION]
>
>터치 UI와 [함께 AEM을 사용하는](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/touch-ui-concepts.html) 경우 작성자 인스턴스 컨텐츠를 캐시하지 **말아야** 합니다. 작성자 인스턴스에 대해 캐싱이 활성화된 경우 이를 비활성화하고 캐시 디렉토리의 컨텐츠를 삭제해야 합니다. 캐싱을 비활성화하려면 `author_dispatcher.any` 파일을 편집하고 `/rule` 섹션의 `/cache` 속성을 다음과 같이 수정해야 합니다.

```xml
/rules
{
/0000
{ /type "deny" /glob "*"}
}
```

작성자 인스턴스 앞에 디스패처를 사용하여 작성 성능을 향상시킬 수 있습니다. 제작 디스패처를 구성하려면 다음을 수행합니다.

1. 웹 서버에 Dispatcher 설치(Apache 또는 IIS 웹 서버일 수 있음, Dispatcher [설치 참조](dispatcher-install.md))
1. 기본 올바른 설치가 수행되었는지 확인하기 위해 작업 중인 AEM 게시 인스턴스에 대해 새로 설치된 Dispatcher를 테스트할 수 있습니다.
1. 이제 디스패처가 TCP/IP를 통해 작성자 인스턴스에 연결할 수 있는지 확인합니다.
1. sample dispatcher.any 파일을 author_dispatcher.any 파일로 대체하여 Dispatcher [다운로드와](release-notes.md#downloads)함께 제공됩니다.
1. 텍스트 `author_dispatcher.any` 편집기에서 을 열고 다음 사항을 변경합니다.

   1. 및 `/hostname` `/port` `/renders` 섹션을 변경하여 작성자 인스턴스를 가리킵니다.
   1. 캐시 `/docroot` 디렉토리를 가리키도록 `/cache` 섹션의 내용을 변경합니다. 터치 UI와 [함께 AEM을 사용하는](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/touch-ui-concepts.html)경우 위의 경고를 참조하십시오.
   1. 변경 사항을 저장합니다.

1. 위에서 구성한 `/cache` &gt; `/docroot` 디렉토리에서 기존 파일을 모두 삭제합니다.
1. 웹 서버를 다시 시작합니다.

>[!NOTE]
>
>제공된 `author_dispatcher.any` 구성을 통해 아래 컨텐츠에 영향을 주는 CQ5 기능 팩, 핫픽스 또는 애플리케이션 코드 패키지를 설치하거나 다음에 `/libs` 요청될 때 새로 업그레이드된 파일이 반입되고 캐시된 이전 파일이 아닌 새로 업그레이드된 파일이 보관되도록 디스패처 캐시의 해당 디렉토리 아래에 캐시된 파일을 삭제해야 `/apps` 합니다.

>[!CAUTION]
>
>이전에 구성한 작성자 디스패처를 사용하고 *디스패처 플러시 에이전트를* 활성화한 경우 다음을 수행하십시오.

1. AEM 작성자 인스턴스에서 **작성자 디스패처의** 플러시 에이전트를 삭제하거나 비활성화합니다.
1. 위의 새로운 지침에 따라 작성자 디스패처 구성을 다시 수행합니다.

<!--
[Author Dispatcher configuration file (Dispatcher 4.1.2 or later)](assets/author_dispatchernew.any)
-->
<!--[!NOTE]
>
>A related knowledge base article can be found here:  
>[How to configure the dispatcher in front of an authoring environment](https://helpx.adobe.com/cq/kb/HowToConfigureDispatcherForAuthoringEnvironment.html)
-->
