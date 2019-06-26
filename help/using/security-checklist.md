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
source-git-commit: 6d3ff696780ce55c077a1d14d01efeaebcb8db28

---


# The Dispatcher Security Checklist{#the-dispatcher-security-checklist}

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
>또한 라이브하기 전에 AEM 버전의 보안 검사 목록을 작성해야 합니다. Please refer to the corresponding [Adobe Experience Manager documentation](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/security-checklist.html).

## Use the Latest Version of Dispatcher {#use-the-latest-version-of-dispatcher}

사용 가능한 최신 버전을 플랫폼에 설치해야 합니다. 최신 버전을 사용하도록 Dispatcher 인스턴스를 업그레이드하면 제품 및 보안 개선 사항을 활용할 수 있습니다. See [Installing Dispatcher](dispatcher-install.md).

>[!NOTE]
>
>Dispatcher 로그 파일을 보면 Dispatcher 설치의 현재 버전을 확인할 수 있습니다.
>
>`[Thu Apr 30 17:30:49 2015] [I] [23171(140735307338496)] Dispatcher initialized (build 4.1.9)`
>
>To find the log file, inspect the dispatcher configuration in your `httpd.conf`.

## Restrict Clients that Can Flush Your Cache {#restrict-clients-that-can-flush-your-cache}

Adobe recommends that you [limit the clients that can flush your cache.](dispatcher-configuration.md#limiting-the-clients-that-can-flush-the-cache)

## Enable HTTPS for transport layer security {#enable-https-for-transport-layer-security}

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

## Restrict Access {#restrict-access}

디스패처를 구성할 때는 외부 액세스를 가능한 한 제한해야 합니다. See [Example /filter Section](dispatcher-configuration.md#main-pars_184_1_title) in the Dispatcher documentation.

## Make Sure Access to Administrative URLs is Denied {#make-sure-access-to-administrative-urls-is-denied}

필터를 사용하여 웹 콘솔과 같은 관리 URL에 대한 외부 액세스를 차단해야 합니다.

See [Testing Dispatcher Security](dispatcher-configuration.md#testing-dispatcher-security) for a list of URLs that need to be blocked.

## Use Whitelists Instead Of Blacklists {#use-whitelists-instead-of-blacklists}

화이트리스트는 본질적으로 이후 액세스 제어를 제공하는 더 좋은 방법이며, 모든 액세스 요청은 허용 목록에 명시적으로 포함되지 않는 한 거부되어야 한다고 가정합니다. 이 모델은 아직 검토되지 않았거나 특정 구성 단계에서 고려되지 않았을 수 있는 새 요청에 대한 보다 제한적인 제어를 제공합니다.

## Run Dispatcher with a Dedicated System User {#run-dispatcher-with-a-dedicated-system-user}

디스패처를 구성할 때 권한이 최소한의 전용 사용자에 의해 웹 서버를 실행했는지 확인해야 합니다. 디스패처 캐시 폴더에 쓰기 권한을 부여하는 것이 좋습니다.

추가 기능: IIS 사용자는 다음과 같이 웹 사이트를 구성해야 합니다.

1. In the physical path setting for your web site, select **Connect as specific user**.
1. 사용자를 설정합니다.

## Prevent Denial of Service (DoS) Attacks {#prevent-denial-of-service-dos-attacks}

서비스 거부 (DoS) 공격은 컴퓨터 리소스를 의도한 사용자가 사용할 수 없도록 하기 위한 것입니다.

At the dispatcher level, there are two methods of configuring to prevent DoS attacks: [](https://docs.adobe.com/content/docs/en/dispatcher.html#/filter (Filters))

* Use the mod_rewrite module (for example, [Apache 2.4](https://httpd.apache.org/docs/2.4/mod/mod_rewrite.html)) to perform URL validations (if the URL pattern rules are not too complex).

* Prevent the dispatcher from caching URLs with spurious extensions by using [filters](dispatcher-configuration.md#configuring-access-to-conten-tfilter).\
   예를 들어 캐싱 규칙을 변경하여 다음과 같이 예상된 MIME 유형으로 캐싱을 제한합니다.

   * `.html`
   * `.jpg`
   * `.gif`
   * `.swf`
   * `.js`
   * `.doc`
   * `.pdf`
   * `.ppt`
   An example configuration file can be seen for [restricting external access](#restrict-access), this includes restrictions for mime types.

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
* `/libs/granite/security/currentuser.json`**(데이터를 캐시해서는** 안 됨)

* `/libs/cq/i18n/*` (Internalization)

<!-- 

Comment Type: remark
Last Modified By: unknown unknown (ims-author-00AF43764F54BE740A490D44@AdobeID)
Last Modified Date: 2015-06-26T04:38:17.016-0400

<p>We need to highlight whether a path applies to all versions or specific ones.<br /> </p>

 -->

## Configure Dispatcher to prevent CSRF Attacks {#configure-dispatcher-to-prevent-csrf-attacks}

AEM provides a [framework](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/security-checklist.html#verification-steps) aimed at preventing Cross-Site Request Forgery attacks. 이 프레임워크를 제대로 사용하려면 Dispatcher에서 CSRF 토큰 지원 기능을 사용해야 합니다. 다음을 수행할 수 있습니다.

1. Creating a filter to allow the `/libs/granite/csrf/token.json` path;
1. Add the `CSRF-Token` header to the `clientheaders` section of the Dispatcher configuration.

## Prevent Clickjacking {#prevent-clickjacking}

To prevent clickjacking we recommend that you configure your webserver to provide the `X-FRAME-OPTIONS` HTTP header set to `SAMEORIGIN`.

For more [information on clickjacking please see the OWASP site](https://www.owasp.org/index.php/Clickjacking).

## Perform a Penetration Test {#perform-a-penetration-test}

Adobe는 프로덕션 진행하기 전에 AEM 인프라에 대한 침투 테스트를 수행하는 것을 적극 권장합니다.

