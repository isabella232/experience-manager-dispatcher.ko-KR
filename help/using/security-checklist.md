---
title: Dispatcher 보안 검사 목록
seo-title: Dispatcher 보안 검사 목록
description: 프로덕션을 시작하기 전에 완료해야 하는 보안 확인 목록입니다.
seo-description: 프로덕션을 시작하기 전에 완료해야 하는 보안 확인 목록입니다.
uuid: 7bfa3202-03f6-48e9-8d2e-2a40e137ecbe
contentOwner: User
products: SG_EXPERIENCEMANAGER/DISPATCHER
topic-tags: dispatcher
content-type: reference
discoiquuid: fbfafa55-c029-4ed7-ab3e-1bebfde18248
jcr-lastmodifiedby: remove-legacypath-6-1
index: y
internal: n
snippet: y
translation-type: tm+mt
source-git-commit: 7889c025fb8fb29e6f11ea01c5248470556d3160
workflow-type: tm+mt
source-wordcount: '653'
ht-degree: 1%

---


# Dispatcher 보안 검사 목록{#the-dispatcher-security-checklist}

<!-- 

Comment Type: remark
Last Modified By: unknown unknown (ims-author-00AF43764F54BE740A490D44@AdobeID)
Last Modified Date: 2015-06-05T05:14:35.365-0400

<p>Food for thought listed on <a href="https://jira.corp.adobe.com/browse/DOC-5649">DOC-5649</a>. To be considered while proof-reading.</p> 
<p> </p>

 -->

Adobe은 프로덕션을 시작하기 전에 다음 체크리스트를 완료하는 것이 좋습니다.

>[!CAUTION]
>
>라이브하려면 AEM 버전의 보안 체크리스트를 완료해야 합니다. 해당 [Adobe Experience Manager 설명서](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/security-checklist.html)를 참조하십시오.

## 최신 버전의 Dispatcher {#use-the-latest-version-of-dispatcher} 사용

사용 가능한 최신 버전을 플랫폼에 설치해야 합니다. 제품 및 보안 개선 사항을 활용하려면 최신 버전을 사용하려면 Dispatcher 인스턴스를 업그레이드해야 합니다. [Dispatcher](dispatcher-install.md) 설치를 참조하십시오.

>[!NOTE]
>
>디스패처 로그 파일을 보고 현재 버전의 디스패처 설치를 확인할 수 있습니다.
>
>`[Thu Apr 30 17:30:49 2015] [I] [23171(140735307338496)] Dispatcher initialized (build 4.1.9)`
>
>로그 파일을 찾으려면 `httpd.conf`에서 발송자 구성을 검사하십시오.

## 캐시를 플러시할 수 있는 클라이언트 제한 {#restrict-clients-that-can-flush-your-cache}

Adobe에서는 [에서 캐시를 플러시할 수 있는 클라이언트를 제한하는 것이 좋습니다.](dispatcher-configuration.md#limiting-the-clients-that-can-flush-the-cache)

## 전송 레이어 보안에 대한 HTTPS 활성화 {#enable-https-for-transport-layer-security}

작성자 및 게시 인스턴스 모두에서 HTTPS 전송 레이어를 활성화하는 것이 좋습니다.

<!-- 

Comment Type: remark
Last Modified By: unknown unknown (ims-author-00AF43764F54BE740A490D44@AdobeID)
Last Modified Date: 2015-06-26T04:41:28.841-0400

<p>Recommended to have SSL termination, front end SSL.</p> 
<p>Question is do we want to have SSL communication between dispatcher and AEM instances (publish and/or author).</p> 
<p>We might want to have two items:</p> 
<ul> 
 <li>MUST HTTPS clients -&gt; dispatcher / load balancer</li> 
 <li>NICE load balancer -&gt; dispatcher<br /> </li> 
 <li>NICE dispatcher -&gt; instances if sensitive information such as credit cards / or infrastructure requirements such as DMZ</li> 
</ul>

 -->

## 액세스 제한 {#restrict-access}

Dispatcher를 구성할 때는 외부 액세스를 가능한 한 제한해야 합니다. Dispatcher 설명서의 [예제 /filter Section](dispatcher-configuration.md#main-pars_184_1_title)를 참조하십시오.

## 관리 URL에 대한 액세스가 거부되었는지 {#make-sure-access-to-administrative-urls-is-denied} 확인

필터를 사용하여 웹 콘솔과 같은 모든 관리 URL에 대한 외부 액세스를 차단해야 합니다.

차단해야 하는 URL 목록은 [Dispatcher Security 테스트](dispatcher-configuration.md#testing-dispatcher-security)를 참조하십시오.

## 차단 목록 {#use-allowlists-instead-of-blocklists} 대신 허용 목록 사용

허용 목록은 기본적으로 액세스 제어를 제공하는 더 나은 방법이므로, 모든 액세스 요청은 명시적으로에 속하지 않는 한 거부되어야 한다고 허용 목록에 추가하다 가정합니다. 이 모델은 특정 구성 단계 동안 아직 검토되지 않았거나 고려되지 않았을 수 있는 새 요청에 대해 보다 제한적인 제어를 제공합니다.

## 전용 시스템 사용자가 {#run-dispatcher-with-a-dedicated-system-user}인 Dispatcher 실행

Dispatcher를 구성할 때는 웹 서버가 최소한의 권한을 가진 전용 사용자에 의해 실행되어야 합니다. 디스패처 캐시 폴더에만 쓰기 액세스 권한을 부여하는 것이 좋습니다.

또한 IIS 사용자는 다음과 같이 웹 사이트를 구성해야 합니다.

1. 웹 사이트의 실제 경로 설정에서 **특정 사용자로 연결을 선택합니다**.
1. 사용자를 설정합니다.

## 서비스 거부(DoS) 공격 방지 {#prevent-denial-of-service-dos-attacks}

서비스 거부(DoS) 공격은 의도한 사용자가 컴퓨터 리소스를 사용할 수 없도록 하려고 한 것입니다.

발송자 수준에서는 DoS 공격을 방지하기 위해 구성하는 두 가지 방법이 있습니다.[](https://docs.adobe.com/content/docs/en/dispatcher.html#/filter (필터))

* mod_rewrite 모듈(예: [Apache 2.4](https://httpd.apache.org/docs/2.4/mod/mod_rewrite.html))을 사용하여 URL 유효성 검사를 수행합니다(URL 패턴 규칙이 너무 복잡하지 않은 경우).

* 디스패처가 [filters](dispatcher-configuration.md#configuring-access-to-conten-tfilter)를 사용하여 잘못된 확장 기능으로 URL을 캐싱하지 않도록 합니다.\
   예를 들어 캐싱 규칙을 변경하여 다음 같은 필요한 MIME 유형으로 캐시를 제한합니다.

   * `.html`
   * `.jpg`
   * `.gif`
   * `.swf`
   * `.js`
   * `.doc`
   * `.pdf`
   * `.ppt`

   [외부 액세스](#restrict-access)를 제한하는 구성 파일의 예를 볼 수 있으며 여기에는 MIME 유형에 대한 제한이 포함됩니다.

게시 인스턴스에서 전체 기능을 안전하게 활성화하려면 다음 노드에 액세스할 수 없도록 필터를 구성합니다.

* `/etc/`
* `/libs/`

그런 다음 필터를 구성하여 다음 노드 경로에 액세스할 수 있습니다.

* `/etc/designs/*`
* `/etc/clientlibs/*`
* `/etc/segmentation.segment.js`
* `/libs/cq/personalization/components/clickstreamcloud/content/config.json`
* `/libs/wcm/stats/tracker.js`
* `/libs/cq/personalization/*` (JS, CSS 및 JSON)
* `/libs/cq/security/userinfo.json` (CQ 사용자 정보)
* `/libs/granite/security/currentuser.json` (**데이터는 캐시되지 않아야** 함)

* `/libs/cq/i18n/*` (국제화)

<!-- 

Comment Type: remark
Last Modified By: unknown unknown (ims-author-00AF43764F54BE740A490D44@AdobeID)
Last Modified Date: 2015-06-26T04:38:17.016-0400

<p>We need to highlight whether a path applies to all versions or specific ones.<br /> </p>

 -->

## CSRF 공격 {#configure-dispatcher-to-prevent-csrf-attacks}을(를) 방지하도록 Dispatcher 구성

AEM은 크로스 사이트 요청 위조 공격을 방지하기 위한 [framework](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/security-checklist.html#verification-steps)를 제공합니다. 이 프레임워크를 제대로 사용하려면 디스패처에서 CSRF 토큰 지원을허용 목록에 추가하다해야 합니다. 다음을 통해 이 작업을 수행할 수 있습니다.

1. `/libs/granite/csrf/token.json` 경로를 허용하는 필터 만들기;
1. Dispatcher 구성의 `clientheaders` 섹션에 `CSRF-Token` 헤더를 추가합니다.

## 클릭재킹 방지 {#prevent-clickjacking}

클릭재킹을 방지하려면 `X-FRAME-OPTIONS` HTTP 헤더가 `SAMEORIGIN`로 설정되어 있도록 웹 서버를 구성하는 것이 좋습니다.

클릭재킹에 대한 자세한 내용은 OWASP 사이트[를 참조하십시오.](https://www.owasp.org/index.php/Clickjacking)

## 통과 테스트 수행 {#perform-a-penetration-test}

Adobe은 프로덕션을 시작하기 전에 AEM 인프라의 보급률 테스트를 수행하는 것이 좋습니다.

