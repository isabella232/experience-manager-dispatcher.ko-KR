---
title: Dispatcher 보안 검사 목록
seo-title: Dispatcher 보안 검사 목록
description: 프로덕션 작업을 진행하기 전에 완료해야 하는 보안 점검 목록.
seo-description: 프로덕션 작업을 진행하기 전에 완료해야 하는 보안 점검 목록.
uuid: 7 BFA 3202-03 F 6-48 E 9-8 D 2 E -2 A 40 E 137 ECBE
contentOwner: 사용자
products: sg_ Experiencemanager/dispatcher
topic-tags: Dispatcher
content-type: 참조
discoiquuid: FBFAFA 55-C 029-4 ED 7-AB 3 E -1 BEBFDE 18248
jcr-lastmodifiedby: remove-legacypath -6-1
index: y
internal: n
snippet: y
translation-type: tm+mt
source-git-commit: f35c79b487454059062aca6a7c989d5ab2afaf7b

---


# Dispatcher 보안 검사 목록{#the-dispatcher-security-checklist}

<!-- 

Comment Type: remark
Last Modified By: unknown unknown (ims-author-00AF43764F54BE740A490D44@AdobeID)
Last Modified Date: 2015-06-05T05:14:35.365-0400

<p>Food for thought listed on <a href="https://jira.corp.adobe.com/browse/DOC-5649">DOC-5649</a>. To be considered while proof-reading.</p> 
<p> </p>

 -->

프런트 엔드 시스템으로 디스패처는 Adobe Experience Manager 인프라에 추가 보안 레이어를 제공합니다. 제작을 진행하기 전에 다음 검사 목록을 작성해 두는 것이 좋습니다.

>[!CAUTION]
>
>또한 라이브하기 전에 AEM 버전의 보안 검사 목록을 작성해야 합니다. 해당 [Adobe Experience Manager 설명서를](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/security-checklist.html)참조하십시오.

## 최신 버전의 Dispatcher 사용 {#use-the-latest-version-of-dispatcher}

사용 가능한 최신 버전을 플랫폼에 설치해야 합니다. 최신 버전을 사용하도록 Dispatcher 인스턴스를 업그레이드하면 제품 및 보안 개선 사항을 활용할 수 있습니다. Dispatcher [설치를 참조하십시오](dispatcher-install.md).

>[!NOTE]
>
>Dispatcher 로그 파일을 보면 Dispatcher 설치의 현재 버전을 확인할 수 있습니다.
>
>`[Thu Apr 30 17:30:49 2015] [I] [23171(140735307338496)] Dispatcher initialized (build 4.1.9)`
>
>로그 파일을 찾으려면에서 Dispatcher 구성을 검사합니다 `httpd.conf`.

## 캐시를 플러시할 수 있는 클라이언트 제한 {#restrict-clients-that-can-flush-your-cache}

캐시를 플러시할 수 있는 클라이언트를 [제한하는 것이 좋습니다.](dispatcher-configuration.md#limiting-the-clients-that-can-flush-the-cache)

## 전송 레이어 보안에 HTTPS 사용 {#enable-https-for-transport-layer-security}

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

디스패처를 구성할 때는 외부 액세스를 가능한 한 제한해야 합니다. Dispatcher [설명서의 /filter 섹션 섹션을](dispatcher-configuration.md#main-pars_184_1_title) 참조하십시오.

## 관리 URL에 대한 액세스가 거부됨 {#make-sure-access-to-administrative-urls-is-denied}

필터를 사용하여 웹 콘솔과 같은 관리 URL에 대한 외부 액세스를 차단해야 합니다.

차단해야 [하는 URL 목록은 Dispatcher Security](dispatcher-configuration.md#testing-dispatcher-security) 테스트를 참조하십시오.

## 블랙리스트 대신 화이트리스트 사용 {#use-whitelists-instead-of-blacklists}

화이트리스트는 본질적으로 이후 액세스 제어를 제공하는 더 좋은 방법이며, 모든 액세스 요청은 허용 목록에 명시적으로 포함되지 않는 한 거부되어야 한다고 가정합니다. 이 모델은 아직 검토되지 않았거나 특정 구성 단계에서 고려되지 않았을 수 있는 새 요청에 대한 보다 제한적인 제어를 제공합니다.

## 전용 시스템 사용자로 Dispatcher 실행 {#run-dispatcher-with-a-dedicated-system-user}

디스패처를 구성할 때 권한이 최소한의 전용 사용자에 의해 웹 서버를 실행했는지 확인해야 합니다. 디스패처 캐시 폴더에 쓰기 권한을 부여하는 것이 좋습니다.

추가 기능: IIS 사용자는 다음과 같이 웹 사이트를 구성해야 합니다.

1. 웹 사이트의 실제 경로 설정에서 **Connect를 특정 사용자로 선택합니다**.
1. 사용자를 설정합니다.

## Dos (서비스 거부) 공격 방지 {#prevent-denial-of-service-dos-attacks}

서비스 거부 (DoS) 공격은 컴퓨터 리소스를 의도한 사용자가 사용할 수 없도록 하기 위한 것입니다.

Dispatcher 수준에는 DoS 공격을 방지하도록 구성하는 두 가지 방법이 있습니다. [](https://docs.adobe.com/content/docs/en/dispatcher.html#/filter (필터))

* URL 유효성 검사를 수행하려면 mod_ rewrite 모듈 (예: [Apache 2.2](https://httpd.apache.org/docs/2.2/mod/mod_rewrite.html)) 를 사용합니다 (URL 패턴 규칙이 너무 복잡한 경우).

* 필터를 사용하여 [트랩이 잘못된 확장명으로 URL 캐시되지 않도록 합니다](dispatcher-configuration.md#configuring-access-to-conten-tfilter).\
   예를 들어 캐싱 규칙을 변경하여 다음과 같이 예상된 MIME 유형으로 캐싱을 제한합니다.

   * `.html`
   * `.jpg`
   * `.gif`
   * `.swf`
   * `.js`
   * `.doc`
   * `.pdf`
   * `.ppt`
   외부 액세스를 제한하기 위해 [예제 구성 파일을 볼 수 있으며](#restrict-access), 여기에는 MIME 유형에 대한 제한 사항이 포함됩니다.

게시 인스턴스에서 전체 기능을 안전하게 활성화하려면 다음 노드에 대한 액세스를 방지하도록 필터를 구성하십시오.

* `/etc/`
* `/libs/`

그런 다음 다음 노드 경로에 액세스할 수 있도록 필터를 구성합니다.

* `/etc/designs/*`
* `/etc/clientlibs/*`
* `/etc/segmentation.segment.js`
* `/libs/cq/personalization/components/clickstreamcloud/content/config.json`
* `/libs/wcm/stats/tracker.js`
* `/libs/cq/personalization/*` (JS, CSS 및 JSON)
* `/libs/cq/security/userinfo.json` (CQ 사용자 정보)
* `/libs/granite/security/currentuser.json`**(데이터를 캐시해서는**안 됨)

* `/libs/cq/i18n/*` (Internalization)

<!-- 

Comment Type: remark
Last Modified By: unknown unknown (ims-author-00AF43764F54BE740A490D44@AdobeID)
Last Modified Date: 2015-06-26T04:38:17.016-0400

<p>We need to highlight whether a path applies to all versions or specific ones.<br /> </p>

 -->

## CSRF 공격을 방지하도록 Dispatcher 구성 {#configure-dispatcher-to-prevent-csrf-attacks}

AEM는 교차 사이트 요청 위조 공격을 방지하기 위한 [프레임워크를](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/security-checklist.html#verification-steps) 제공합니다. 이 프레임워크를 제대로 사용하려면 Dispatcher에서 CSRF 토큰 지원 기능을 사용해야 합니다. 다음을 수행할 수 있습니다.

1. 경로를 허용하는 필터 `/libs/granite/csrf/token.json` 만들기
1. Dispatcher `CSRF-Token` 구성의 `clientheaders` 섹션에 헤더를 추가합니다.

## 클릭재킹 방지 {#prevent-clickjacking}

클릭재킹을 방지하려면 `X-FRAME-OPTIONS` HTTP 헤더를 설정하도록 웹 서버를 구성하는 것이 좋습니다 `SAMEORIGIN`.

클릭재킹에 [대한 자세한 내용은 OWASP 사이트를 참조하십시오](https://www.owasp.org/index.php/Clickjacking).

## 침투 테스트 수행 {#perform-a-penetration-test}

Adobe는 프로덕션 진행하기 전에 AEM 인프라에 대한 침투 테스트를 수행하는 것을 적극 권장합니다.

