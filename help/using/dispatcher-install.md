---
title: Dispatcher 설치
seo-title: AEM Dispatcher 설치
description: Microsoft Internet Information Server, Apache Web Server 및 Sun Java Web Server-iplanet에 Dispatcher 모듈을 설치하는 방법을 살펴봅니다.
seo-description: Microsoft Internet Information Server, Apache Web Server 및 Sun Java Web Server-iplanet에 AEM Dispatcher 모듈을 설치하는 방법을 알아봅니다.
uuid: 2384 B 907-1042-4707-B 02 F-FBA 2125618 CF
contentOwner: 사용자
converted: 'true'
topic-tags: Dispatcher
content-type: 참조
discoiquuid: F 00 AD 751-6 B 95-4365-8500-E 1 E 0108 D 9536
translation-type: tm+mt
source-git-commit: f35c79b487454059062aca6a7c989d5ab2afaf7b

---


# Dispatcher 설치 {#installing-dispatcher}

<!-- 

Comment Type: draft

<h2>Introduction</h2>

 -->

>[!NOTE]
>
>발송자 버전은 AEM와 독립적입니다. 이전 버전의 AEM에 대한 설명서에 포함된 Dispatcher 설명서에 대한 링크를 팔로우한 경우 이 페이지로 리디렉션되었을 수 있습니다.

[Dispatcher 릴리스 노트](release-notes.md) 페이지를 사용하여 운영 체제 및 웹 서버에 대한 최신 Dispatcher 설치 파일을 입수합니다. Dispatcher 릴리스 번호는 Adobe Experience Manager 릴리스 번호와 관련이 없으며 Adobe Experience Manager 6. x, 5. x 및 Adobe CQ 5. x와 호환됩니다.

다음 파일 이름 지정 규칙이 사용됩니다.

`dispatcher-<web-server>-<operating-system>-<dispatcher-version-number>.<file-format>`

예를 들어, 이 `dispatcher-apache2.4-linux-x86_64-ssl-4.3.1.tar.gz` 파일에는 Linux i 686에서 실행되고 **TAR** 형식을 사용하여 패키지화된 Apache 2.4 웹 서버용 Dispatcher 버전 4.3.1 이 포함되어 있습니다.

다음 표에는 각 웹 서버의 파일 이름에 사용되는 웹 서버 식별자가 나와 있습니다.

| 웹 서버 | 설치 키트 |
|--- |--- |
| Apache 2.4 | Dispatcher-Apache **2.4**-&lt; 기타 매개 변수 &gt; |
| Apache 2.2 | Dispatcher-Apache **2.2**-&lt; 기타 매개 변수 &gt; |
| Microsoft Internet Information Server 7.5, 8, 8.5 | dispatcher-**iis**-&lt; other parameters &gt; |
| Sun Java 웹 서버 iplanet | dispatcher-**ns**-&lt; other parameters &gt; |

>[!NOTE]
>
>플랫폼에 사용할 수 있는 최신 버전의 Dispatcher를 설치해야 합니다. 연간 기준으로 Dispatcher 인스턴스를 업그레이드하여 제품 개선 사항을 최대한 활용할 수 있습니다.

각 보관 파일에는 다음 파일이 포함되어 있습니다.

* Dispatcher 모듈
* 예제 구성 파일
* 설치 지침 및 최종 정보가 들어 있는 README 파일
* 현재 및 이전 릴리스에서 해결된 문제를 나열하는 변경 파일

>[!NOTE]
>
>설치를 시작하기 전에 최종 단계에서 변경 사항/플랫폼별 메모에 대한 README 파일을 확인하십시오.

<!-- 

Comment Type: draft

<h3>Supported Web Servers</h3>

 -->

<!-- 

Comment Type: draft

<p>The following web servers are supported for use with Dispatcher version 4.1.12:</p>

 -->

<!-- 

Comment Type: draft

<p>The following sections detail the specific web server installation procedures.</p>

 -->

## Microsoft Internet Information Server {#microsoft-internet-information-server}

이 웹 서버를 설치하는 방법에 대한 자세한 내용은 다음 리소스를 참조하십시오.

* Internet Information Server에 대한 Microsoft의 설명서
* [&quot; 공식 Microsoft IIS 사이트 &quot;](https://www.iis.net/)

### 필수 IIS 구성 요소 {#required-iis-components}

IIS 버전 8.5 및 10를 사용하려면 다음 IIS 구성 요소가 설치되어 있어야 합니다.

* ISAPI 익스텐션

또한 웹 서버 (IIS) 역할을 추가해야 합니다. 서버 관리자를 사용하여 역할 및 구성 요소를 추가합니다.

## Microsoft IIS - Dispatcher 모듈 설치 {#microsoft-iis-installing-the-dispatcher-module}

Microsoft 인터넷 정보 시스템에 필요한 보관 자료는 다음과 같습니다.

* `dispatcher-iis-<operating-system>-<dispatcher-release-number>.zip`

zip 파일에는 다음 파일이 포함되어 있습니다.

| 파일 | 설명 |
|--- |--- |
| `disp_iis.dll` | Dispatcher Dynamic Link 라이브러리 파일입니다. |
| `disp_iis.ini` | IIS 용 구성 파일입니다. 이 예는 사용자의 요구 사항에 맞게 업데이트될 수 있습니다. ****참고: INI 파일의 이름 루트는 DLL와 같아야 합니다. |
| `dispatcher.any` | Dispatcher의 예제 구성 파일입니다. |
| `author_dispatcher.any` | 작성자 인스턴스와 함께 작동하는 Dispatcher의 예제 구성 파일입니다. |
| README | 설치 지침과 최종 정보가 들어 있는 Readme 파일 ****참고: 설치를 시작하기 전에 이 파일을 확인하십시오. |
| changes | 현재 및 이전 릴리스에서 해결된 문제를 나열하는 파일을 변경합니다. |

다음 절차를 사용하여 Dispatcher 파일을 올바른 위치로 복사합니다.

1. 예를 들어, Windows 탐색기를 사용하여 `<IIS_INSTALLDIR>/Scripts` 디렉토리를 만듭니다 `C:\inetpub\Scripts`.

1. Dispatcher 패키지에서 다음 파일을 이 Scripts 디렉토리로 추출합니다.

   * `disp_iis.dll`
   * `disp_iis.ini`
   * AEM 작성자 인스턴스 또는 게시 인스턴스에서 디스패처가 작동하는지 여부에 따라 다음 파일 중 하나를 반환합니다.
      * 작성자 인스턴스: `author_dispatcher.any`
      * 게시 인스턴스: `dispatcher.any`

## Microsoft IIS - Dispatcher INI 파일 구성 {#microsoft-iis-configure-the-dispatcher-ini-file}

파일을 `disp_iis.ini` 편집하여 Dispatcher 설치를 구성합니다. 파일의 기본 형식은 `.ini` 다음과 같습니다.

```xml
[main]
configpath=<path to dispatcher.any>
loglevel=1|2|3
servervariables=0|1
replaceauthorization=0|1
```

다음 표에서는 각 속성에 대해 설명합니다.

| 매개 변수 | 설명 |
|--- |--- |
| Configpath | 로컬 파일 시스템 `dispatcher.any` (절대 경로) 내 위치. |
| logfile | 파일의 `dispatcher.log` 위치입니다. 설정되지 않은 경우 로그 메시지는 Windows 이벤트 로그로 이동합니다. |
| Loglevel | 이벤트 로그에 메시지를 출력하는 데 사용되는 로그 수준을 정의합니다. 다음 값을 지정할 수 있습니다. 로그 파일의 로그 수준: <br/>0 - 오류 메시지만. <br/>1 - 오류 및 경고. <br/>2 - 오류, 경고 및 정보 메시지 <br/>3 - 오류, 경고, 정보 및 디버그 메시지. <br/>****참고: 설치 및 테스트 중에 로그 수준을 3로 설정하고 프로덕션 환경에서 실행 시 0로 설정하는 것이 좋습니다. |
| Replaceauthorization | HTTP 요청에서 인증 헤더가 처리되는 방식을 지정합니다. 다음 값이 유효합니다.<br/>0 - 인증 헤더가 수정되지 않습니다. <br/>1 - &quot; basic &quot;이외의&quot; authorization &quot;이라는 헤더를 `Basic <IIS:LOGON\_USER>` 이와 동등한 이름으로 대체합니다.<br/> |
| Servervariables | 서버 변수가 처리되는 방식을 정의합니다.<br/>0 - IIS 서버 변수는 디스패처 또는 AEM에 모두 전송되지 않습니다. <br/>1 - 모든 IIS 서버 변수 (등 `LOGON\_USER, QUERY\_STRING, ...`) 는 요청 헤더와 함께 디스패처로 전송되며, 캐시되지 않은 경우에는 AEM 인스턴스에도 전송됩니다. <br/>서버 변수에는 `AUTH\_USER, LOGON\_USER, HTTPS\_KEYSIZE` 등이 포함됩니다. 자세한 내용은 전체 변수 목록을 보려면 IIS 설명서를 참조하십시오. |
| enable_ chunked_ transfer | 클라이언트 응답에 대해 청크 전송을 활성화 (1) 하거나 비활성화 (0) 할지 여부를 정의합니다. 기본값은 0 입니다. |

예제 구성:

```xml
[main]
configpath=C:\Inetpub\Scripts\dispatcher.any
loglevel=1
servervariables=1
replaceauthorization=0
```

### Microsoft IIS 구성 {#configuring-microsoft-iis}

Dispatcher ISAPI 모듈을 통합하도록 IIS를 구성합니다. IIS 에서는 와일드카드 응용 프로그램 매핑을 사용합니다.

### 익명 액세스 구성 - IIS 8.5 및 10 {#configuring-anonymous-access-iis-and}

작성자 인스턴스의 기본 플러시 복제 에이전트는 플러시 요청이 있는 보안 자격 증명을 보내지 않도록 구성됩니다. 따라서 디스패처 캐시를 사용하는 웹 사이트는 익명 액세스를 허용해야 합니다.

웹 사이트에서 인증 방법을 사용하는 경우 그에 따라 플러시 복제 에이전트를 구성해야 합니다.

1. IIS 관리자를 열고 disptcher 캐시로 사용 중인 웹 사이트를 선택합니다.
1. 기능 보기 모드를 사용하는 IIS 섹션에서 인증을 두 번 클릭합니다.
1. 익명 인증이 활성화되어 있지 않으면 익명 인증을 선택하고 [작업] 영역에서 [활성화] 를 클릭합니다.

### Dispatcher ISAPI 모듈 통합 - IIS 8.5 및 10 {#integrating-the-dispatcher-isapi-module-iis-and}

다음 절차를 사용하여 Dispatcher ISAPI 모듈을 IIS에 추가합니다.

1. IIS 관리자를 엽니다.
1. 디스패처 캐시로 사용 중인 웹 사이트를 선택합니다.
1. IIS 섹션 두 번 클릭 처리기 매핑을 사용하여 기능 보기 모드를 사용합니다.
1. 처리기 매핑 페이지의 작업 패널에서 와일드카드 스크립트 맵을 클릭하고 다음 속성 값을 추가한 다음 확인을 클릭합니다.

   * 요청 경로: *
   * 실행 파일: disp_ iis. dll 파일의 절대 경로입니다 `C:\inetpub\Scripts\disp_iis.dll`.
   * 이름: 핸들러 매핑의 설명형 이름입니다 `Dispatcher`. 예를 들면 다음과 같습니다.

1. 대화 상자가 나타나면 disp_ iis. dll 라이브러리를 ISAPI 및 CGI 제한 목록에 추가하려면 예를 클릭합니다.

   IIS 7.0 및 7.5의 경우 구성이 완료되었습니다. IIS 8.0를 구성하는 경우 나머지 단계를 계속 진행합니다.

1. (IIS 8.0) 처리기 매핑 목록에서 방금 만든 작업을 선택하고 [작업] 영역에서 [편집] 를 클릭합니다.
1. (IIS 8.0) 스크립트 맵 편집 대화 상자에서 요청 제한 단추를 클릭합니다.
1. (IIS 8.0) 아직 캐시되지 않은 파일과 폴더에 대해 핸들러를 사용하도록 하려면 요청이 매핑된 경우에만 [핸들러 호출] 를 선택 해제한 다음 [확인] 를 클릭합니다.
1. (IIS 8.0) [스크립트 맵 편집] 대화 상자에서 [확인] 를 클릭합니다.

### 캐시에 대한 액세스 구성 - IIS 8.5 및 10 {#configuring-access-to-the-cache-iis-and}

디스패처 캐시로 사용되는 폴더에 대한 쓰기 액세스 권한을 기본 앱 풀 사용자에게 제공합니다.

1. Dispatcher 캐시로 사용 중인 웹 사이트의 루트 폴더를 마우스 오른쪽 단추로 클릭하고 [속성] 를 클릭합니다 `C:\inetpub\wwwroot`.
1. [보안] 탭에서 [편집] 를 클릭한 다음 [권한] 대화 상자에서 [추가] 를 클릭합니다. 사용자 계정을 선택하는 대화 상자가 열립니다. [위치] 단추를 클릭하고 컴퓨터 이름을 선택한 다음 [확인] 를 클릭합니다.

   다음 단계를 진행하는 동안 이 대화 상자를 열어 두십시오.

1. IIS 관리자에서 Dispatcher 캐시에 사용 중인 IIS 사이트를 선택하고 창 오른쪽에서 [고급 설정] 를 클릭합니다.
1. 응용 프로그램 풀 속성 값을 선택하고 클립보드에 복사합니다.
1. 열기 대화 상자로 돌아갑니다. [선택할 개체 이름 입력] 상자에 클립보드의 내용을 입력한 `IIS AppPool\` 다음 붙여 넣습니다. 이 값은 다음 예와 비슷해야 합니다.

   `IIS AppPool\DefaultAppPool`

1. [이름 확인] 단추를 클릭합니다. Windows에서 사용자 계정을 해결하면 [확인] 를 클릭합니다.
1. Dispatcher 폴더에 대한 권한 대화 상자에서 방금 추가한 계정을 선택하고 모든 권한을 제외한 계정에 **대한 모든 권한을 활성화하고 확인을** 클릭합니다. [확인] 를 클릭하여 [폴더 속성] 대화 상자를 닫습니다.

### JSON MIME 유형 등록 - IIS 8.5 및 10 {#registering-the-json-mime-type-iis-and}

디스패처를 통해 JSON 호출을 허용하려면 다음 절차를 사용하여 JSON MIME 유형을 등록합니다.

1. IIS 관리자에서 웹 사이트를 선택하고 기능 보기를 사용하여 MIME 유형을 두 번 클릭합니다.
1. JSON 확장자가 목록에 없으면 [액션] 패널에서 [추가] 를 클릭하고 다음 속성 값을 입력한 다음 [확인] 를 클릭합니다.

   * 파일 이름 확장자: `.json`
   * MIME 유형: `application/json`

### 저장소 숨겨진 세그먼트 제거 - IIS 8.5 및 10 {#removing-the-bin-hidden-segment-iis-and}

숨겨진 세그먼트를 제거하려면 다음 절차를 `bin` 사용하십시오. 새 웹 사이트가 아닌 웹 사이트에는 이 숨겨진 세그먼트가 포함될 수 있습니다.

1. IIS 관리자에서 웹 사이트를 선택하고 기능 보기를 사용하여 요청 필터링 두 번 클릭합니다.
1. `bin` 세그먼트를 선택하고 [제거] 를 클릭한 다음 확인 대화 상자에서 예를 클릭합니다.

### IIS 메시지를 파일에 로깅 (IIS 8.5 및 10) {#logging-iis-messages-to-a-file-iis-and}

다음 절차를 사용하여 Windows 이벤트 로그 대신 로그 파일에 Dispatcher 로그 메시지를 작성하십시오. 로그 파일을 사용하도록 Dispatcher를 구성하고 파일에 대한 쓰기 액세스 권한을 IIS에게 제공해야 합니다.

1. Windows 탐색기를 사용하여 IIS 설치의 로그 폴더 `dispatcher` 아래에 있는 폴더를 만듭니다. 일반적인 설치를 위한 이 폴더의 경로입니다 `C:\inetpub\logs\dispatcher`.

1. Dispatcher 폴더를 마우스 오른쪽 버튼으로 클릭하고 속성을 클릭합니다.
1. [보안] 탭에서 [편집] 를 클릭한 다음 [권한] 대화 상자에서 [추가] 를 클릭합니다. 사용자 계정을 선택하는 대화 상자가 열립니다. [위치] 단추를 클릭하고 컴퓨터 이름을 선택한 다음 [확인] 를 클릭합니다.

   다음 단계를 진행하는 동안 이 대화 상자를 열어 두십시오.

1. IIS 관리자에서 Dispatcher 캐시에 사용 중인 IIS 사이트를 선택하고 창 오른쪽에서 [고급 설정] 를 클릭합니다.
1. 응용 프로그램 풀 속성 값을 선택하고 클립보드에 복사합니다.
1. 열기 대화 상자로 돌아갑니다. [선택할 개체 이름 입력] 상자에 클립보드의 내용을 입력한 `IIS AppPool\` 다음 붙여 넣습니다. 이 값은 다음 예와 비슷해야 합니다.

   `IIS AppPool\DefaultAppPool`

1. [이름 확인] 단추를 클릭합니다. Windows에서 사용자 계정을 해결하면 [확인] 를 클릭합니다.
1. Dispatcher 폴더에 대한 권한 대화 상자에서 방금 추가한 계정을 선택하고 계정의 모든 권한을 활성화** 를 제외한 모든 권한을 활성화한 다음 확인을 클릭합니다. [확인] 를 클릭하여 [폴더 속성] 대화 상자를 닫습니다.
1. 텍스트 편집기를 사용하여 `disp_iis.ini` 파일을 엽니다.
1. 다음 예와 유사한 텍스트 줄을 추가하여 로그 파일의 위치를 구성한 다음 파일을 저장합니다.

   ```xml
   logfile=C:\inetpub\logs\dispatcher\dispatcher.log
   ```

### 다음 단계 {#next-steps}

Dispatcher를 사용하려면 먼저 다음을 알고 있어야 합니다.

* [Configure](dispatcher-configuration.md) Dispatcher
* [AEM에서 Dispatcher](page-invalidate.md) 를 사용하여 작업할 수 있습니다.

## Apache 웹 서버 {#apache-web-server}

>[!CAUTION]
>
>**Windows** 및 **UNIX** 에서 설치 지침이 여기에 설명되어 있습니다. 단계를 수행할 때에는 주의하십시오.

### Apache 웹 서버 설치 {#installing-apache-web-server}

Apache 웹 서버를 설치하는 방법에 대한 자세한 내용은 [온라인](https://httpd.apache.org/) 또는 배포 중 설치 설명서를 참조하십시오.

>[!CAUTION]
>
>소스 파일을 컴파일하여 Apache 바이너리를 만드는 경우 다이내믹 모듈 지원 기능을 활성화해야 ****합니다. 이 작업은 **—공유** 옵션을 활성화합니다. 최소한 `mod_so` 모듈이 포함됩니다.
>
>자세한 내용은 Apache Web Server 설치 매뉴얼을 참조하십시오.

Also see the Apache HTTP Server [Security Tips](https://httpd.apache.org/docs/2.2/misc/security_tips.html) and [Security Reports](https://httpd.apache.org/security_report.html).

### Apache 웹 서버 - Dispatcher 모듈 추가 {#apache-web-server-add-the-dispatcher-module}

Dispatcher는 다음과 같이 제공됩니다.

* **Windows**: DLL (Dynamic Link Library)
* **UNIX**: DSO (Dynamic Shared Object)

설치 아카이브 파일에는 Windows 또는 Unix를 선택했는지에 따라 다음 파일이 포함됩니다.

| 파일 | 설명 |
|--- |--- |
| disp_ apache &lt; x. y &gt;. dll | Windows: Dispatcher Dynamic Link 라이브러리 파일입니다. |
| dispatcher-apache &lt; x. y &gt;-&lt; rel-nr &gt;. so | UNIX: Dispatcher 공유 객체 라이브러리 파일입니다. |
| mod_ dispatcher. so | UNIX: 예제 링크. |
| http. conf. disp &lt; x &gt; | Apache 서버에 대한 예제 구성 파일. |
| Dispatcher. any | Dispatcher의 예제 구성 파일입니다. |
| README | 설치 지침과 최종 정보가 들어 있는 Readme 파일 ****참고: 설치를 시작하기 전에 이 파일을 확인하십시오. |
| changes | 현재 및 이전 릴리스에서 해결된 문제를 나열하는 파일을 변경합니다. |

Apache 웹 서버에 디스패처를 추가하려면 다음 단계를 수행합니다.

1. Dispatcher 파일을 적절한 Apache 모듈 디렉토리에 놓습니다.

   * **Windows**: place `disp_apache<x.y>.dll``<APACHE_ROOT>/modules`
   * **UNIX**: 설치에 따라 `<APACHE_ROOT>/libexec` 또는 `<APACHE_ROOT>/modules`디렉토리를 찾습니다.\
      이 `dispatcher-apache<options>.so` 디렉토리에 복사합니다.\
      장기 유지 관리를 간소화하기 위해 Dispatcher로 이름이 `mod_dispatcher.so` 지정된 상징적 링크를 만들 수도 있습니다.\
      `ln -s dispatcher-apache<x>-<os>-<rel-nr>.so mod_dispatcher.so`

1. Dispatcher. any 파일을 `<APACHE_ROOT>/conf` 디렉토리로 복사합니다.

   **참고:** Dispatcher 모듈의 dispatcherlog 속성이 적절하게 구성되어 있는 한 이 파일을 다른 위치에 배치할 수 있습니다. (아래의 Dispatcher 관련 구성 항목을 참조하십시오.)

### Apache 웹 서버 - Selinux 속성 구성 {#apache-web-server-configure-selinux-properties}

Redhat Linux 커널 2.6에서 selinux가 활성화된 Dispatcher를 실행하는 경우 Dispatcher logfile에서 이와 같은 오류 메시지가 발생할 수 있습니다.

`Mon Jun 30 00:03:59 2013] [E] [16561(139642697451488)] Unable to connect to backend rend01 (10.122.213.248:4502): Permission denied`

이는 활성화된 Selinux 보안 때문일 수 있습니다. 그런 다음 다음 작업을 수행해야 합니다.

* Dispatcher 모듈 파일의 selinux 컨텍스트를 구성합니다.
* HTTPD 스크립트 및 모듈을 활성화하여 네트워크 연결을 만듭니다.
* 캐시된 파일이 저장되는 docroot의 selinux 컨텍스트를 구성합니다.

Terminal 창에서 다음 명령을 입력하고 `[path to the dispatcher.so file]` Apache 웹 서버에 설치한 Dispatcher 모듈 및 Docroot가 *`path to the docroot`* 있는 경로 (예: `/opt/cq/cache`) 로 바꿉니다.

```shell
semanage fcontext -a -t httpd_modules_t [path to the dispatcher.so file]
setsebool -P httpd_can_network_connect on
chcon -R --type httpd_sys_content_t [path to the docroot]
semanage fcontext -a -t httpd_sys_content_t "[path to the docroot](/.*)?"
```

### Apache 웹 서버 - Dispatcher 용 Apache 웹 서버 구성 {#apache-web-server-configure-apache-web-server-for-dispatcher}

Apache 웹 서버를 사용하여 `httpd.conf`구성해야 합니다. Dispatcher 설치 키트에서 이름이 지정된 예제 구성 파일을 찾을 `httpd.conf.disp<x>`수 있습니다.

이러한 단계는 필수입니다.

1. 다음으로 이동 `<APACHE_ROOT>/conf`.
1. 열어서 `httpd.conf`편집할 수 있습니다.
1. 나열된 순서대로 다음 구성 항목을 추가해야 합니다.

   * **Loadmodule** 를 사용하여 시작할 때 모듈을 로드합니다.
   * **Dispatcherconfig, dispatcherlog** 및 **dispatcherloglevel를 포함한 Dispatcher 관련 구성 항목입니다**.
   * **Sethandler** 를 사용하여 디스패처를 활성화합니다. **Loadmodule**.
   * **Modmimeusepathinfo** 를 사용하여 **mod_ mime의 동작을 구성합니다**.

1. (선택 사항) Htdocs 디렉토리의 소유자를 변경하는 것이 좋습니다.

   * 하위 프로세스가 보안 목적인 데몬으로 시작되더라도 Apache 서버가 루트로 시작됩니다. Documentroot (`<APACHE_ROOT>/htdocs`) 는 사용자 데몬에 속해야 합니다.

      ```xml
      cd <APACHE_ROOT>  
      chown -R daemon:daemon htdocs
      ```

**Loadmodule**

다음 표에는 사용할 수 있는 예가 나와 있습니다. 정확한 항목은 특정 Apache 웹 서버에 따라 달라집니다.

|  |
|--- |--- |
| Windows | `... LoadModule dispatcher_module modules\disp_apache.dll ...` |
| UNIX (심볼릭 링크 가정) | `... LoadModule dispatcher_module libexec/mod_dispatcher.so ...` |

>[!NOTE]
>
>각 문의 첫 번째 매개 변수는 위의 예제와 같이 정확하게 작성해야 합니다.
>
>이 명령에 대한 자세한 내용은 제공된 구성 파일 예제 및 Apache Web Server 설명서를 참조하십시오.

**Dispatcher 특정 구성 항목**

Dispatcher 관련 구성 항목은 loadmodule 항목 뒤에 배치됩니다. 다음 표에는 Unix 및 Windows 모두에 적용되는 예제 구성이 나와 있습니다.

**Windows 및 UNIX**

```
...
<fModule disp_apache2.c>
DispatcherConfig conf/dispatcher.any
DispatcherLog logs/dispatcher.log DispatcherLogLevel 3
DispatcherNoServerHeader 0 DispatcherDeclineRoot 0
DispatcherUseProcessedURL 0
DispatcherPassError 0
DispatcherKeepAliveTimeout 60
</IfModule>
...
```

개별 구성 매개 변수:

| 매개 변수 | 설명 |
|--- |--- |
| Dispatcherconfig | Dispatcher 구성 파일의 위치 및 이름입니다. <br/>이 속성이 주 서버 구성에 있으면 모든 가상 호스트가 속성 값을 상속합니다. 그러나 가상 주최자는 기본 서버 구성을 무시할 수 있도록 Dispatcherconfig 속성을 포함할 수 있습니다. |
| Dispatcherlog | 로그 파일의 위치 및 이름입니다. |
| Dispatcherloglevel | 로그 파일의 로그 수준: <br/>0 - 오류 <br/>1 - 경고 <br/>2 - 정보 <br/>3 - 디버그 <br/>**참고**: 설치 및 테스트 중에 로그 수준을 3로 설정하고 프로덕션 환경에서 실행 시 0로 설정하는 것이 좋습니다. |
| Dispatchernoserverheader | *이 매개 변수는 더 이상 사용되지 않으며 더 이상 영향을 주지 않습니다.*<br/><br/> 사용할 서버 헤더를 정의합니다. <br/><ul><li>정의되지 않음 또는 0 - HTTP 서버 헤더에 AEM 버전이 포함됩니다. </li><li>1 - Apache 서버 헤더가 사용됩니다.</li></ul> |
| Dispatcherdeclineroot | 루트 &quot;/&quot; 에 대한 요청을 거부할지 여부를 정의합니다. <br/>**0** - Request to/1 accept to/ <br/>**1** - request to/is not handled by the dispatcher; 올바른 매핑에 mod_ alias를 사용합니다. |
| Dispatcheruseprocessedurl | Dispatcher의 모든 추가 처리에 대해 사전 처리된 URL를 사용할지 여부를 정의합니다. <br/>**0** - 웹 서버에 전달된 원본 URL를 사용합니다. <br/>**1** - 디스패처는 웹 서버로 전달된 원본 URL 대신 Dispatcher 앞에 있는 핸들러 (즉 `mod_rewrite`,) 가 이미 처리한 URL를 사용합니다. 예를 들어 원본 또는 처리된 URL는 발송자 필터와 일치합니다. URL는 캐시 파일 구조의 기초로 사용됩니다. mod_ rewrite에 대한 자세한 내용은 Apache 웹 사이트 설명서를 참조하십시오. 예: Apache 2.2. mod_ rewrite를 사용하는 경우 플래그&#39;passthrough&#39;를 사용하는 것이 좋습니다. | pt &#39; (다음 핸들러로 통과) 를 사용하여 다시 작성 엔진이 내부 Request_ Rec 구조의 URI 필드를 filename 필드의 값으로 설정하도록 합니다. |
| Dispatcherpasserror | Errordocument 처리를 위한 오류 코드 지원 방법을 정의합니다. <br/>**0** - 디스패처가 클라이언트에 대한 모든 오류 응답을 스풀링합니다. <br/>**1** - 디스패처는 클라이언트에 오류 응답을 스풀링하지 않고 (상태 코드가 400 보다 크거나 같은 경우) 상태 코드를 Apache로 전달하지만, 상태 코드를 Apache로 전달하여 이러한 상태 코드를 처리하도록 합니다. <br/>**코드 범위** - 응답이 Apache로 전달되는 오류 코드의 범위를 지정합니다. 다른 오류 코드가 클라이언트에 전달됩니다. 예를 들어, 다음 구성은 클라이언트에 오류 412에 대한 응답을 전달하며, 다른 모든 오류는 Apache로 전달됩니다. Dispatcherpasserror 400-411,413-417 |
| Dispatcherkeepalivetimeout | keep-alive 제한 시간 (초) 를 지정합니다. Dispatcher 버전 4.2.0 부터 기본 유지 유지 값은 60 입니다. 값이 0 이면 keep-alive가 비활성화됩니다. |
| Dispatchernocanonurl | 이 매개 변수를 켜짐으로 설정하면 원시 URL 이 canonicalized one 이 아니라 백엔드로 전달되고, dispatcheruseprocessedurl의 설정이 무시됩니다. 기본값은 off 입니다. <br/>****참고: Dispatcher 구성의 필터 규칙은 항상 원시 URL 이 아닌 기밀 URL에 대해 평가됩니다. |




>[!NOTE]
>
>경로 항목은 Apache 웹 서버의 루트 디렉토리에 상대적입니다.

>[!NOTE]
>
>서버 헤더의 기본 설정은 다음과 같습니다. `  
ServerTokens Full``  
DispatcherNoServerHeader 0`\
AEM 버전을 보여 줍니다 (통계적 목적용). 헤더에서 사용할 수 있는 이러한 정보를 비활성화하려면 다음을 설정합니다. `  
ServerTokens Prod`\
자세한 [내용은 Servertokens 지시문 (예: Apache 2.2의 경우)](https://httpd.apache.org/docs/2.2/mod/core.html) 에 대한 Apache 설명서를 참조하십시오.

**Sethandler**

이러한 시작 후에는 Dispatcher **에서** 들어오는 요청을 처리하도록 구성 ( `<Directory>`, `<Location>`) 컨텍스트 컨텍스트에 sethandler 문을 추가해야 합니다. 다음 예제는 전체 웹 사이트의 요청을 처리하도록 Dispatcher를 구성합니다.

**Windows 및 UNIX**

```
...  
<Directory />  
<IfModule disp\_apache2.c>  
SetHandler dispatcher-handler  
</IfModule>  
  
Options FollowSymLinks  
AllowOverride None  
</Directory>  
...
```

다음 예제에서는 가상 도메인 요청을 처리하도록 Dispatcher를 구성합니다.

**Windows**

```
...  
<VirtualHost 123.45.67.89>  
ServerName www.mycompany.com  
DocumentRoot _\[cache-path\]_\\docs  
<Directory _\[cache-path\]_\\docs>  
<IfModule disp\_apache2.c>  
SetHandler dispatcher-handler  
</IfModule>  
AllowOverride None  
</Directory>  
</VirtualHost>  
...
```

**UNIX**

```
...  
<VirtualHost 123.45.67.89>  
ServerName www.mycompany.com  
DocumentRoot /usr/apachecache/docs  
<Directory /usr/apachecache/docs>  
<IfModule disp\_apache2.c>  
SetHandler dispatcher-handler  
</IfModule>  
AllowOverride None  
</Directory>  
</VirtualHost>  
...
```

>[!NOTE]
The parameter of the **sethandler** statement must be written *exactly as the above examples*, as this is the name defined in the handler defined.
이 명령에 대한 자세한 내용은 제공된 구성 파일 예제 및 Apache Web Server 설명서를 참조하십시오.

**Modmimeusepathinfo**

**Sethandler** 문 뒤에 **modmimeusepathinfo** 정의도 추가해야 합니다.

>[!NOTE]
이 `ModMimeUsePathInfo` 매개 변수는 Dispatcher 버전 4.0.9 이상을 사용하는 경우에만 사용하고 구성해야 합니다.
(Dispatcher 버전 4.0.9는 2011 년 릴리스되었습니다. 이전 버전을 사용하고 있는 경우 최신 버전의 Dispatcher 버전으로 업그레이드할 수 있습니다.

모든 Apache 구성에 **대해 modmimeusepathinfo** 매개 변수를 설정해야 `On` 합니다.

`ModMimeUsePathInfo On`

mod_ mime 모듈 (예: [Apache 모듈 mod_ mime 참조](https://httpd.apache.org/docs/2.2/mod/mod_mime.html)) 는 HTTP 응답에 대해 선택된 컨텐츠에 컨텐츠 메타데이터를 할당하는 데 사용됩니다. 기본 설정은 mod_ mime 이 컨텐츠 유형을 결정하면 파일 또는 디렉토리로 매핑되는 URL 부분만 고려한다는 의미입니다.

when `On`, the `ModMimeUsePathInfo` parameter specified that `mod_mime` is to determine the content type based on the *complete* URL; 즉, 가상 리소스는 확장에 따라 메타정보를 적용하게 됩니다.

다음 예제는 **Modmimeusepathinfo**를 활성화합니다.

**Windows 및 UNIX**

```
...  
<Directory />  
<IfModule disp\_apache2.c>  
SetHandler dispatcher-handler  
ModMimeUsePathInfo On  
</IfModule>  
  
Options FollowSymLinks  
AllowOverride None  
</Directory>  
...
```

### HTTPS (UNIX 및 Linux) 지원 활성화 {#enable-support-for-https-unix-and-linux}

Dispatcher는 openssl를 사용하여 HTTP를 통한 안전한 커뮤니케이션을 구현합니다. Dispatcher 버전 **4.2.0**, openssl 1.0.0 및 openssl 1.0.1에서 시작합니다. Dispatcher는 기본적으로 openssl 1.0.0를 사용합니다. Openssl 1.0.1를 사용하려면 다음 절차를 사용하여 Dispatcher가 설치된 openssl 라이브러리를 사용하도록 심볼릭 링크를 만듭니다.

1. 터미널을 열고 현재 디렉토리를 Openssl 라이브러리가 설치된 디렉토리로 변경합니다. 예를 들면 다음과 같습니다.

   ```shell
   cd /usr/lib64
   ```

1. 심볼릭 링크를 만들려면 다음 명령을 입력합니다.

   ```shell
   ln -s libssl.so libssl.so.1.0.1
   ln -s libcrypto.so libcrypto.so.1.0.1
   ```

>[!NOTE]
Apache의 사용자 정의 버전을 사용하는 경우 Apache 및 Dispatcher가 [openssl의 버전을 사용하여 컴파일되는지](https://www.openssl.org/source/)확인합니다.

### 다음 단계 {#next-steps-1}

Dispatcher 사용을 시작하려면 먼저 다음을 수행해야 합니다.

* [Configure](dispatcher-configuration.md) Dispatcher
* [AEM에서 Dispatcher](page-invalidate.md) 를 사용하여 작업할 수 있습니다.

## Sun Java System Web Server/iplanet {#sun-java-system-web-server-iplanet}

>[!NOTE]
Windows 및 UNIX 환경 모두에 대한 지침은 여기에서 다룹니다.
실행할 선택 시에는 주의하십시오.

### Sun Java System Web Server/iplanet - 웹 서버 설치 {#sun-java-system-web-server-iplanet-installing-your-web-server}

이러한 웹 서버를 설치하는 방법에 대한 자세한 내용은 해당 설명서를 참조하십시오.

* Sun Java System Web Server
* Iplanet 웹 서버

### Sun Java System Web Server/iplanet - Dispatcher 모듈 추가 {#sun-java-system-web-server-iplanet-add-the-dispatcher-module}

Dispatcher는 다음과 같이 제공됩니다.

* **Windows**: DLL (Dynamic Link Library)
* **UNIX**: DSO (Dynamic Shared Object)

설치 아카이브 파일에는 Windows 또는 Unix를 선택했는지에 따라 다음 파일이 포함됩니다.

| 파일 | 설명 |
|---|---|
| `disp_ns.dll` | Windows: Dispatcher Dynamic Link 라이브러리 파일입니다. |
| `dispatcher.so` | UNIX: Dispatcher 공유 객체 라이브러리 파일입니다. |
| `dispatcher.so` | UNIX: 예제 링크. |
| `obj.conf.disp` | Iplanet/Sun Java 시스템 웹 서버에 대한 예제 구성 파일입니다. |
| `dispatcher.any` | Dispatcher의 예제 구성 파일입니다. |
| README | 설치 지침과 최종 정보가 들어 있는 Readme 파일 참고: 설치를 시작하기 전에 이 파일을 확인하십시오. |
| changes | 현재 및 이전 릴리스에서 해결된 문제를 나열하는 파일을 변경합니다. |

다음 단계에 따라 웹 서버에 디스패처를 추가합니다.

1. Dispatcher 파일을 웹 서버 `plugin` 디렉토리에 놓습니다.

### Sun Java System Web Server/iplanet - Dispatcher 구성 {#sun-java-system-web-server-iplanet-configure-for-the-dispatcher}

을 사용하여 웹 서버를 구성해야 `obj.conf`합니다. Dispatcher 설치 키트에서 이름이 지정된 예제 구성 파일을 찾을 `obj.conf.disp`수 있습니다.

1. 다음으로 이동 `<WEBSERVER_ROOT>/config`.
1. 열어서 `obj.conf`편집할 수 있습니다.
1. 시작되는 라인을 복사합니다.\
   `Service fn="dispService"`\
   의 `obj.conf.disp` 초기화 `obj.conf`섹션에서

1. 변경 사항을 저장합니다.
1. 열어서 `magnus.conf` 편집할 수 있습니다.
1. 시작하는 두 줄을 복사합니다.\
   `Init funcs="dispService, dispInit"`\
   및\
   `Init fn="dispInit"`\
   의 `obj.conf.disp` 초기화 `magnus.conf`섹션에서

1. 변경 사항을 저장합니다.

>[!NOTE]
다음 구성은 모두 한 줄 및에 `$(SERVER_ROOT)` 있어야 하며 해당 `$(PRODUCT_SUBDIR)` 값으로 대체되어야 합니다.

**init**

다음 표에는 사용할 수 있는 예가 나와 있습니다. 정확한 항목은 특정 웹 서버에 따라 달라집니다.

**Windows 및 UNIX**

```
...  
Init funcs="dispService,dispInit" fn="load-modules" shlib="$(SERVER\_ROOT)/plugins/dispatcher.so"  
Init fn="dispInit" config="$(PRODUCT\_SUBDIR)/dispatcher.any" loglevel="1" logfile="$(PRODUCT\_SUBDIR)/logs/dispatcher.log"  
keepalivetimeout="60"  
...
```

위치:

| 매개 변수 | 설명 |
|--- |--- |
| config | 구성 파일의 위치 및 이름 `dispatcher.any.` |
| logfile | 로그 파일의 위치 및 이름입니다. |
| Loglevel | 로그 파일에 메시지를 작성할 때의 로그 수준: <br/>**0** 오류 <br/>**1** 경고 <br/>**2** Infos <br/>**3** 디버그 <br/>**참고:** 설치 및 테스트 중에 로그 수준을 3로 설정하는 것이 좋습니다. 프로덕션 환경에서 실행 시에는 0로 설정하는 것이 좋습니다. |
| Keepalivetimeout | keep-alive 제한 시간 (초) 를 지정합니다. Dispatcher 버전 4.2.0 부터 기본 유지 유지 값은 60 입니다. 값이 0 이면 keep-alive가 비활성화됩니다. |

요구 사항에 따라 디스패처를 개체에 대한 서비스로 정의할 수 있습니다. 전체 웹 사이트의 Dispatcher를 구성하려면 기본 개체를 수정합니다.


**Windows**

```
...  
NameTrans fn="document-root" root="$(PRODUCT\_SUBDIR)\\dispcache"  
...  
Service fn="dispService" method="(GET|HEAD|POST)" type="\*\\\*"  
...
```

**UNIX**

```
...  
NameTrans fn="document-root" root="$(PRODUCT\_SUBDIR)/dispcache"  
...  
Service fn="dispService" method="(GET|HEAD|POST)" type="\*/\*"  
...
```

### 다음 단계 {#next-steps-2}

Dispatcher 사용을 시작하려면 먼저 다음을 수행해야 합니다.

* [Configure](dispatcher-configuration.md) Dispatcher
* [AEM에서 Dispatcher](page-invalidate.md) 를 사용하여 작업할 수 있습니다.
