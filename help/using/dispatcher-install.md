---
title: Dispatcher 설치
seo-title: AEM Dispatcher 설치
description: Microsoft Internet Information Server, Apache Web Server 및 Sun Java Web Server-iPlanet에 Dispatcher 모듈을 설치하는 방법을 알아봅니다.
seo-description: Microsoft Internet Information Server, Apache Web Server 및 Sun Java Web Server-iPlanet에 AEM Dispatcher 모듈을 설치하는 방법을 알아봅니다.
uuid: 2384b907-1042-4707-b02f-fba2125618cf
contentOwner: User
converted: true
topic-tags: dispatcher
content-type: reference
discoiquuid: f00ad751-6b95-4365-8500-e1e0108d9536
exl-id: 9375d1c0-8d9e-46cb-9810-fa4162a8c1ba
source-git-commit: 3a0e237278079a3885e527d7f86989f8ac91e09d
workflow-type: tm+mt
source-wordcount: '3684'
ht-degree: 0%

---

# Dispatcher 설치 {#installing-dispatcher}

<!-- 

Comment Type: draft

<h2>Introduction</h2>

 -->

[Dispatcher 릴리스 노트](release-notes.md) 페이지를 사용하여 운영 체제 및 웹 서버에 대한 최신 Dispatcher 설치 파일을 가져옵니다. Dispatcher 릴리스 번호는 Adobe Experience Manager 릴리스 번호와 독립적으로, Adobe Experience Manager 6.x, 5.x 및 Adobe CQ 5.x 릴리스와 호환됩니다.

>[!NOTE]
>
>Adobe Experience Manager 6.5를 사용하려면 Dispatcher 버전 4.3.2 이상이 필요합니다. 즉, Dispatcher 버전은 AEM과 독립적입니다. 예를 들어 Dispatcher 버전 4.3.2는 Adobe Experience Manager 6.4와도 호환됩니다.

다음 파일 이름 지정 규칙이 사용됩니다.

`dispatcher-<web-server>-<operating-system>-<dispatcher-version-number>.<file-format>`

예를 들어, `dispatcher-apache2.4-linux-x86_64-ssl-4.3.1.tar.gz` 파일에는 Linux i686에서 실행되며 **tar** 형식을 사용하여 패키지화된 Apache 2.4 웹 서버용 Dispatcher 버전 4.3.1이 포함되어 있습니다.

다음 표에는 각 웹 서버의 파일 이름에 사용되는 웹 서버 식별자가 나와 있습니다.

| 웹 서버 | 설치 키트 |
|--- |--- |
| Apache 2.4 | dispatcher-apache **2.4**-&lt;기타 매개 변수> |
| Microsoft Internet Information Server 7.5, 8, 8.5 | dispatcher-**iis**-&lt;기타 매개 변수 |
| Sun Java Web Server iPlanet | dispatcher-**ns**-&lt;기타 매개 변수> |

>[!CAUTION]
>
>플랫폼에 사용할 수 있는 최신 버전의 Dispatcher를 설치해야 합니다. 매년 제품 개선 사항을 활용하기 위해 최신 버전을 사용하도록 Dispatcher 인스턴스를 업그레이드해야 합니다.

각 아카이브에는 다음 파일이 포함되어 있습니다.

* 디스패처 모듈
* 예제 구성 파일
* 설치 지침 및 마지막 분 정보가 포함된 README 파일
* 현재 및 이전 릴리스에서 해결된 문제를 나열하는 CHANGES 파일

>[!NOTE]
>
>설치를 시작하기 전에 README 파일에서 마지막 변경 사항/플랫폼별 메모를 확인하십시오.

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

## Microsoft 인터넷 정보 서버 {#microsoft-internet-information-server}

이 웹 서버를 설치하는 방법에 대한 자세한 내용은 다음 리소스를 참조하십시오.

* 인터넷 정보 서버에 대한 Microsoft 자체 문서
* [&quot;공식 Microsoft IIS 사이트&quot;](https://www.iis.net/)

### 필수 IIS 구성 요소 {#required-iis-components}

IIS 버전 8.5 및 10을 사용하려면 다음 IIS 구성 요소가 설치되어 있어야 합니다.

* ISAPI 확장

또한 웹 서버(IIS) 역할을 추가해야 합니다. 서버 관리자를 사용하여 역할 및 구성 요소를 추가합니다.

## Microsoft IIS - 디스패처 모듈 {#microsoft-iis-installing-the-dispatcher-module} 설치

Microsoft 인터넷 정보 시스템에 필요한 아카이브는 다음과 같습니다.

* `dispatcher-iis-<operating-system>-<dispatcher-release-number>.zip`

ZIP 파일에는 다음 파일이 포함되어 있습니다.

| 파일 | 설명 |
|--- |--- |
| `disp_iis.dll` | Dispatcher 동적 링크 라이브러리 파일입니다. |
| `disp_iis.ini` | IIS의 구성 파일입니다. 이 예는 요구 사항에 따라 업데이트할 수 있습니다. **참고**:ini 파일의 이름 루트는 dll과 같아야 합니다. |
| `dispatcher.any` | Dispatcher에 대한 구성 파일 예제 . |
| `author_dispatcher.any` | 작성자 인스턴스에서 작업하는 Dispatcher에 대한 구성 파일 예제 . |
| README | 설치 지침 및 마지막 분 정보가 포함된 Readme 파일입니다. **참고**:설치를 시작하기 전에 이 파일을 확인하십시오. |
| 변경 사항 | 현재 및 이전 릴리스에서 해결된 문제를 나열하는 파일을 변경합니다. |

다음 절차를 사용하여 Dispatcher 파일을 올바른 위치에 복사합니다.

1. Windows 탐색기를 사용하여 `<IIS_INSTALLDIR>/Scripts` 디렉터리를 만듭니다(예: `C:\inetpub\Scripts`).

1. Dispatcher 패키지에서 다음 파일을 이 Scripts 디렉토리에 추출합니다.

   * `disp_iis.dll`
   * `disp_iis.ini`
   * Dispatcher가 AEM 작성자 인스턴스 또는 게시 인스턴스에서 작업 있는지에 따라 다음 파일 중 하나:
      * 작성자 인스턴스:`author_dispatcher.any`
      * 게시 인스턴스:`dispatcher.any`

## Microsoft IIS - Dispatcher INI 파일 구성 {#microsoft-iis-configure-the-dispatcher-ini-file}

`disp_iis.ini` 파일을 편집하여 Dispatcher 설치를 구성합니다. `.ini` 파일의 기본 형식은 다음과 같습니다.

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
| 구성 경로 | 로컬 파일 시스템 내의 `dispatcher.any` 위치(절대 경로)입니다. |
| 로그 파일 | `dispatcher.log` 파일의 위치입니다. 이 설정을 설정하지 않으면 로그 메시지가 windows 이벤트 로그로 이동합니다. |
| loglevel | 메시지를 이벤트 로그로 출력하는 데 사용되는 로그 수준을 정의합니다. 로그 파일의 로그 레벨은 다음 값을 지정할 수 있습니다.<br/>0 - 오류 메시지만 <br/>1 - 오류 및 경고 <br/>2 - 오류, 경고 및 정보 메시지  <br/>3 - 오류, 경고, 정보 및 디버그 메시지. <br/>**참고**:설치 및 테스트 중에 로그 레벨을 3으로 설정한 다음 프로덕션 환경에서 실행할 때 0으로 설정하는 것이 좋습니다. |
| 재승인 | HTTP 요청의 인증 헤더를 처리하는 방법을 지정합니다. 다음 값이 유효합니다.<br/>0 - 인증 헤더가 수정되지 않았습니다. <br/>1 - &quot;Basic&quot; 이외의 &quot;Authorization&quot;이라는 헤더를  `Basic <IIS:LOGON\_USER>` 동등 항목으로 바꿉니다.<br/> |
| 서버 변수 | 서버 변수를 처리하는 방법을 정의합니다.<br/>0 - IIS 서버 변수가 Dispatcher와 AEM 모두에 전송됩니다. <br/>1 - 모든 IIS 서버 변수(예:  `LOGON\_USER, QUERY\_STRING, ...`)가 요청 헤더와 함께 Dispatcher에 전송되고(캐시되지 않은 경우 AEM 인스턴스에도 전송됨).  <br/>서버 변수에는  `AUTH\_USER, LOGON\_USER, HTTPS\_KEYSIZE` 및 기타 여러 변수가 포함됩니다. 자세한 내용은 IIS 설명서를 참조하십시오. |
| enable_chunked_transfer | 클라이언트 응답에 대해 청크 전송을 활성화(1) 또는 비활성화(0)할지 정의합니다. 기본값은 0입니다. |

구성 예:

```xml
[main]
configpath=C:\Inetpub\Scripts\dispatcher.any
loglevel=1
servervariables=1
replaceauthorization=0
```

### Microsoft IIS {#configuring-microsoft-iis} 구성

Dispatcher ISAPI 모듈을 통합하도록 IIS를 구성합니다. IIS에서는 와일드카드 응용 프로그램 매핑을 사용합니다.

### 익명 액세스 구성 - IIS 8.5 및 10 {#configuring-anonymous-access-iis-and}

작성자 인스턴스의 기본 플러시 복제 에이전트가 초기화 요청으로 보안 자격 증명을 전송하지 않도록 구성되었습니다. 따라서 Dispatcher 캐시를 사용하는 웹 사이트에서는 익명 액세스를 허용해야 합니다.

웹 사이트에서 인증 방법을 사용하는 경우 Flush 복제 에이전트를 그에 따라 구성해야 합니다.

1. IIS 관리자를 열고 Dispatcher 캐시로 사용 중인 웹 사이트를 선택합니다.
1. 기능 보기 모드를 사용하여 IIS 섹션에서 인증을 두 번 클릭합니다.
1. 익명 인증을 사용할 수 없으면 익명 인증을 선택하고 작업 영역에서 활성화를 클릭합니다.

### Dispatcher ISAPI 모듈 통합 - IIS 8.5 및 10 {#integrating-the-dispatcher-isapi-module-iis-and}

다음 절차를 사용하여 Dispatcher ISAPI 모듈을 IIS에 추가합니다.

1. IIS 관리자를 엽니다.
1. Dispatcher 캐시로 사용할 웹 사이트를 선택합니다.
1. 기능 보기 모드를 사용하여 IIS 섹션에서 처리기 매핑을 두 번 클릭합니다.
1. [처리기 매핑] 페이지의 [작업] 패널에서 [와일드카드 스크립트 맵 추가]를 클릭하고 다음 속성 값을 추가한 다음 [확인]을 클릭합니다.

   * 요청 경로:*
   * 실행 파일:disp_iis.dll 파일의 절대 경로입니다(예: `C:\inetpub\Scripts\disp_iis.dll`).
   * 이름:처리기 매핑에 대한 수사적 이름(예: `Dispatcher`).

1. 표시되는 대화 상자에서 ISAPI 및 CGI 제한 목록에 disp_iis.dll 라이브러리를 추가하려면 예를 클릭합니다.

   IIS 7.0 및 7.5의 경우 구성이 완료되었습니다. IIS 8.0을 구성하는 경우 나머지 단계를 계속 진행합니다.

1. (IIS 8.0) 처리기 매핑 목록에서 방금 만든 처리기를 선택하고 작업 영역에서 편집을 클릭합니다.
1. (IIS 8.0) 스크립트 맵 편집 대화 상자에서 요청 제한 단추를 클릭합니다.
1. (IIS 8.0) 처리기가 아직 캐시되지 않은 파일 및 폴더에 사용되도록 하려면 [요청이 매핑된 경우에만 처리기 호출]을 선택 취소한 다음 [확인]을 클릭합니다.
1. (IIS 8.0) 스크립트 맵 편집 대화 상자에서 확인을 클릭합니다.

### 캐시에 대한 액세스 구성 - IIS 8.5 및 10 {#configuring-access-to-the-cache-iis-and}

기본 앱 풀 사용자에게 Dispatcher 캐시로 사용 중인 폴더에 대한 쓰기 권한을 제공합니다.

1. Dispatcher 캐시로 사용 중인 웹 사이트의 루트 폴더를 마우스 오른쪽 단추로 클릭하고 `C:\inetpub\wwwroot` 등의 속성 을 클릭합니다.
1. 보안 탭에서 편집을 클릭한 다음 권한 대화 상자에서 추가를 클릭합니다. 사용자 계정을 선택하는 대화 상자가 열립니다. [위치] 단추를 클릭하고 컴퓨터 이름을 선택한 다음 [확인]을 클릭합니다.

   다음 단계를 완료하는 동안 이 대화 상자를 열어 두십시오.

1. iis 관리자에서 Dispatcher 캐시에 사용할 IIS 사이트를 선택하고 창의 오른쪽에서 고급 설정을 클릭합니다.
1. 응용 프로그램 풀 속성 값을 선택하고 클립보드에 복사합니다.
1. 열린 대화 상자로 돌아갑니다. 선택할 개체 이름 입력 상자에 `IIS AppPool\`을 입력한 다음 클립보드의 내용을 붙여 넣습니다. 값은 다음 예제와 비슷합니다.

   `IIS AppPool\DefaultAppPool`

1. 이름 확인 단추를 클릭합니다. Windows에서 사용자 계정을 확인하면 확인을 클릭합니다.
1. Dispatcher 폴더의 권한 대화 상자에서 방금 추가한 계정을 선택하고 전체 제어&#x200B;**를 제외하고 계정**&#x200B;에 대한 모든 권한을 활성화하고 확인을 클릭합니다. 확인 을 클릭하여 폴더 속성 대화 상자를 닫습니다.

### JSON Mime 유형 등록 - IIS 8.5 및 10 {#registering-the-json-mime-type-iis-and}

Dispatcher에서 JSON 호출을 허용하도록 할 때 다음 절차를 사용하여 JSON MIME 유형을 등록하십시오.

1. IIS 관리자에서 웹 사이트를 선택하고 기능 보기를 사용하여 Mime 유형을 두 번 클릭합니다.
1. JSON 확장이 목록에 없는 경우 작업 패널에서 추가 를 클릭하고 다음 속성 값을 입력한 다음 확인 을 클릭합니다.

   * 파일 이름 확장명:`.json`
   * MIME 유형:`application/json`

### 빈 숨겨진 세그먼트 제거 - IIS 8.5 및 10 {#removing-the-bin-hidden-segment-iis-and}

다음 절차를 사용하여 `bin` 숨겨진 세그먼트를 제거합니다. 새 세그먼트가 아닌 웹 사이트에는 이 숨겨진 세그먼트가 포함될 수 있습니다.

1. IIS 관리자에서 웹 사이트를 선택하고 기능 보기를 사용하여 요청 필터링을 두 번 클릭합니다.
1. `bin` 세그먼트를 선택하고 제거를 클릭한 다음 확인 대화 상자에서 예를 클릭합니다.

### 파일에 IIS 메시지 로깅 - IIS 8.5 및 10 {#logging-iis-messages-to-a-file-iis-and}

다음 절차를 사용하여 Dispatcher 로그 메시지를 Windows 이벤트 로그 대신 로그 파일에 작성합니다. 로그 파일을 사용하도록 Dispatcher를 구성하고 IIS에서 파일에 대한 쓰기 권한을 제공해야 합니다.

1. Windows 탐색기를 사용하여 IIS 설치의 로그 폴더 아래에 `dispatcher` 폴더를 만듭니다. 일반적인 설치에 대한 이 폴더의 경로는 `C:\inetpub\logs\dispatcher`입니다.

1. 디스패처 폴더를 마우스 오른쪽 단추로 클릭하고 속성 을 클릭합니다.
1. 보안 탭에서 편집을 클릭한 다음 권한 대화 상자에서 추가를 클릭합니다. 사용자 계정을 선택하는 대화 상자가 열립니다. [위치] 단추를 클릭하고 컴퓨터 이름을 선택한 다음 [확인]을 클릭합니다.

   다음 단계를 완료하는 동안 이 대화 상자를 열어 두십시오.

1. iis 관리자에서 Dispatcher 캐시에 사용할 IIS 사이트를 선택하고 창의 오른쪽에서 고급 설정을 클릭합니다.
1. 응용 프로그램 풀 속성 값을 선택하고 클립보드에 복사합니다.
1. 열린 대화 상자로 돌아갑니다. 선택할 개체 이름 입력 상자에 `IIS AppPool\`을 입력한 다음 클립보드의 내용을 붙여 넣습니다. 값은 다음 예제와 비슷합니다.

   `IIS AppPool\DefaultAppPool`

1. 이름 확인 단추를 클릭합니다. Windows에서 사용자 계정을 확인하면 확인을 클릭합니다.
1. Dispatcher 폴더의 권한 대화 상자에서 방금 추가한 계정을 선택하고 전체 제어,**를 제외한 계정**&#x200B;에 대한 모든 권한을 활성화하고 확인을 클릭합니다. 확인 을 클릭하여 폴더 속성 대화 상자를 닫습니다.
1. 텍스트 편집기를 사용하여 `disp_iis.ini` 파일을 엽니다.
1. 다음 예와 유사한 텍스트 행을 추가하여 로그 파일의 위치를 구성한 다음 파일을 저장합니다.

   ```xml
   logfile=C:\inetpub\logs\dispatcher\dispatcher.log
   ```

### 다음 단계 {#next-steps}

Dispatcher 사용을 시작하려면 먼저 알고 있어야 합니다.

* [](dispatcher-configuration.md) Dispatcher 구성
* [Dispatcher](page-invalidate.md) 에서 작동하도록 AEM을 구성합니다.

## Apache 웹 서버 {#apache-web-server}

>[!CAUTION]
>
>**Windows** 및 **Unix**&#x200B;모두 아래의 설치 지침은 여기에서 다룹니다. 단계를 수행할 때는 주의하십시오.

### Apache 웹 서버 {#installing-apache-web-server} 설치

Apache 웹 서버를 설치하는 방법에 대한 자세한 내용은 설치 설명서( [online](https://httpd.apache.org/) 또는 배포)를 참조하십시오.

>[!CAUTION]
>
>소스 파일을 컴파일하여 Apache 바이너리를 만드는 경우 **동적 모듈 지원**&#x200B;을 사용하도록 설정해야 합니다. 이 작업은 **—enable-shared** 옵션 중 하나를 사용하여 수행할 수 있습니다. 최소한 `mod_so` 모듈이 포함됩니다.
>
>자세한 내용은 Apache Web Server 설치 설명서를 참조하십시오.

Apache HTTP Server [보안 팁](https://httpd.apache.org/docs/2.4/misc/security_tips.html) 및 [보안 보고서](https://httpd.apache.org/security_report.html)도 참조하십시오.

### Apache 웹 서버 - 디스패처 모듈 추가 {#apache-web-server-add-the-dispatcher-module}

Dispatcher는 다음 중 하나로 제공됩니다.

* **Windows**:동적 링크 라이브러리(DLL)
* **Unix**:동적 공유 개체(DSO)

설치 아카이브 파일에는 Windows 또는 Unix를 선택했는지 여부에 따라 다음 파일이 포함됩니다.

| 파일 | 설명 |
|--- |--- |
| disp_apache&lt;x.y>.dll | Windows:Dispatcher 동적 링크 라이브러리 파일입니다. |
| dispatcher-apache&lt;x.y>-&lt;rel-nr>.so | Unix:Dispatcher 공유 개체 라이브러리 파일입니다. |
| mod_dispatcher.so | Unix:예제 링크. |
| http.conf.disp&lt;x> | Apache 서버의 구성 파일 예제. |
| dispatcher.any | Dispatcher에 대한 구성 파일 예제 . |
| README | 설치 지침 및 마지막 분 정보가 포함된 Readme 파일입니다. **참고**:설치를 시작하기 전에 이 파일을 확인하십시오. |
| 변경 사항 | 현재 및 이전 릴리스에서 해결된 문제를 나열하는 파일을 변경합니다. |

다음 단계를 사용하여 Apache 웹 서버에 Dispatcher를 추가하십시오.

1. Dispatcher 파일을 적절한 Apache 모듈 디렉토리에 배치합니다.

   * **Windows**:장소  `disp_apache<x.y>.dll` `<APACHE_ROOT>/modules`
   * **Unix**:설치에  `<APACHE_ROOT>/libexec` 따라 또는  `<APACHE_ROOT>/modules`디렉토리를 찾습니다.\
      `dispatcher-apache<options>.so`을 이 디렉터리에 복사합니다.\
      장기 유지 관리를 단순화하기 위해 Dispatcher에 `mod_dispatcher.so`이라는 심볼릭 링크를 만들 수도 있습니다.\
      `ln -s dispatcher-apache<x>-<os>-<rel-nr>.so mod_dispatcher.so`

1. dispatcher.any 파일을 `<APACHE_ROOT>/conf` 디렉토리에 복사합니다.

   **참고:**  Dispatcher 모듈의 DispatcherLog 속성이 그에 따라 구성되어 있으면 이 파일을 다른 위치에 배치할 수 있습니다. (아래의 디스패처별 구성 항목을 참조하십시오.)

### Apache 웹 서버 - SELinux 속성 구성 {#apache-web-server-configure-selinux-properties}

SELinux가 활성화된 RedHat Linux Kernel 2.6에서 Dispatcher를 실행하는 경우 Dispatcher 로그 파일에서 이와 같은 오류 메시지가 표시될 수 있습니다.

`Mon Jun 30 00:03:59 2013] [E] [16561(139642697451488)] Unable to connect to backend rend01 (10.122.213.248:4502): Permission denied`

이는 SELinux 보안 기능이 활성화되어 있기 때문일 수 있습니다. 그런 다음 다음 다음 작업을 수행해야 합니다.

* 디스패처 모듈 파일의 SELinux 컨텍스트를 구성합니다.
* HTTPD 스크립트 및 모듈을 사용하여 네트워크 연결을 만듭니다.
* 캐시된 파일이 저장되는 docroot의 SELinux 컨텍스트를 구성합니다.

터미널 창에 다음 명령을 입력하고 `[path to the dispatcher.so file]` 을 Apache Web Server에 설치한 Dispatcher 모듈의 경로로 바꾸고 *`path to the docroot`* 을 docroot가 있는 경로로 바꿉니다(예: ).`/opt/cq/cache`):

```shell
semanage fcontext -a -t httpd_modules_t [path to the dispatcher.so file]
setsebool -P httpd_can_network_connect on
chcon -R --type httpd_sys_content_t [path to the docroot]
semanage fcontext -a -t httpd_sys_content_t "[path to the docroot](/.*)?"
```

### Apache 웹 서버 - Dispatcher에 대한 Apache 웹 서버 구성 {#apache-web-server-configure-apache-web-server-for-dispatcher}

`httpd.conf` 을 사용하여 Apache 웹 서버를 구성해야 합니다. Dispatcher 설치 키트에서 `httpd.conf.disp<x>` 구성 파일 예를 찾을 수 있습니다.

이러한 단계는 필수입니다.

1. 다음으로 이동 `<APACHE_ROOT>/conf`.
1. 편집할 `httpd.conf`을 엽니다.
1. 나열된 순서로 다음 구성 항목을 추가해야 합니다.

   * **** LoadModule을 사용하여 시작할 때 모듈을 로드합니다.
   * **DispatcherConfig, DispatcherLog** 및 **DispatcherLogLevel**&#x200B;을 포함하여 디스패처별 구성 항목입니다.
   * **** SetHandller를 사용하여 Dispatcher를 활성화합니다. **LoadModule**.
   * **** ModMimeUsePathInfo에서 mod_mime **의 동작을 구성합니다**.

1. (선택 사항) htdocs 디렉토리의 소유자를 변경하는 것이 좋습니다.

   * 하위 프로세스가 데몬으로 시작되지만(보안을 위해) apache 서버는 루트로 시작됩니다. DocumentRoot(`<APACHE_ROOT>/htdocs`)는 사용자 데몬에 속해야 합니다.

      ```xml
      cd <APACHE_ROOT>  
      chown -R daemon:daemon htdocs
      ```

**LoadModule**

다음 표에는 사용할 수 있는 예가 나와 있습니다.정확한 항목은 특정 Apache 웹 서버에 따라 다릅니다.

|  |  |
|--- |--- |
| Windows | `... LoadModule dispatcher_module modules\disp_apache.dll ...` |
| Unix(심볼 링크 가정) | `... LoadModule dispatcher_module libexec/mod_dispatcher.so ...` |

>[!NOTE]
>
>각 문의 첫 번째 매개 변수는 위의 예와 동일하게 작성해야 합니다.
>
>이 명령에 대한 자세한 내용은 제공된 예제 구성 파일 및 Apache 웹 서버 설명서 를 참조하십시오.

**Dispatcher 특정 구성 항목**

Dispatcher 관련 구성 항목은 LoadModule 항목 뒤에 배치됩니다. 다음 표에는 Unix 및 Windows 모두에 적용할 수 있는 구성 예가 나와 있습니다.

**Windows 및 Unix**

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
| DispatcherConfig | Dispatcher 구성 파일의 위치 및 이름입니다. <br/>이 속성이 주 서버 구성에 있으면 모든 가상 호스트는 속성 값을 상속합니다. 그러나 가상 호스트는 DispatcherConfig 속성을 포함하여 주 서버 구성을 재정의할 수 있습니다. |
| DispatcherLog | 로그 파일의 위치 및 이름입니다. |
| DispatcherLogLevel | 로그 파일의 로그 수준:<br/>0 - 오류 <br/>1 - 경고 <br/>2 - 정보 <br/>3 - 디버그 <br/>**참고**:설치 및 테스트 중에 로그 레벨을 3으로 설정한 다음 프로덕션 환경에서 실행할 때 0으로 설정하는 것이 좋습니다. |
| DispatcherNoServerHeader | *이 매개 변수는 더 이상 사용되지 않으며 더 이상 영향을 주지 않습니다.*<br/><br/> 사용할 서버 헤더를 정의합니다.  <br/><ul><li>정의되지 않음 또는 0 - HTTP 서버 헤더에 AEM 버전이 포함되어 있습니다. </li><li>1 - Apache 서버 헤더가 사용됩니다.</li></ul> |
| DispatcherReceptRoot | 루트 &quot;/&quot;에 대한 요청을 거절할지 여부를 정의합니다.<br/>**0** - / <br/>**1**&#x200B;에 대한 요청을 수락합니다. / 에 대한 요청은 디스패처에서 처리하지 않습니다.올바른 매핑에 mod_alias를 사용합니다. |
| DispatcherUseProcessedURL | Dispatcher가 추가로 처리하는 모든 데 사전 처리된 URL을 사용할지 여부를 정의합니다.<br/>**0** - 웹 서버에 전달된 원래 URL을 사용합니다. <br/>**1**  - 디스패처는 디스패처 앞에 있는 핸들러에서 이미 처리한 URL(즉, `mod_rewrite`) 내의 아무 곳에나 삽입할 수 있습니다.  예를 들어 원래 또는 처리된 URL은 Dispatcher 필터와 일치합니다. URL은 캐시 파일 구조의 기준으로도 사용됩니다.   mod_rewrite에 대한 자세한 내용은 Apache 웹 사이트 설명서 를 참조하십시오.예를 들어 Apache 2.4. mod_rewrite를 사용할 때는 &#39;passthrough&#39; 플래그를 사용하는 것이 좋습니다 | PT&#39;(다음 처리기로 전달)을 사용하여 rewrite 엔진이 내부 request_rec 구조의 uri 필드를 파일 이름 필드의 값으로 설정하도록 합니다. |
| DispatcherPassError | ErrorDocument 처리에 대한 오류 코드를 지원하는 방법을 정의합니다.<br/>**0** - Dispatcher는 클라이언트에 대한 모든 오류 응답을 스푸처리합니다. <br/>**1**  - Dispatcher는 클라이언트에 대한 오류 응답을 스풀링하지 않습니다(상태 코드가 400보다 크거나 같음). 그러나 상태 코드를 Apache에 전달합니다. 예를 들어 ErrorDocument 지시문이 이러한 상태 코드를 처리할 수 있도록 허용합니다. <br/>**코드 범위**  - Apache에 응답이 전달되는 오류 코드 범위를 지정합니다. 다른 오류 코드가 클라이언트에 전달됩니다. 예를 들어 다음 구성은 오류 412에 대한 응답을 클라이언트에 전달하며 다른 모든 오류가 Apache에 전달됩니다.DispatcherPassError 400-411,413-417 |
| DispatcherKeepAliveTimeout | keep-alive 시간 제한을 초 단위로 지정합니다. Dispatcher 버전 4.2.0부터 기본 keep-alive 값은 60입니다. 값이 0이면 keep-alive가 비활성화됩니다. |
| DispatcherNoCanonURL | 이 매개 변수를 On으로 설정하면 정식 URL 대신 원시 URL이 백엔드에 전달되고 DispatcherUseProcessedURL의 설정이 무시됩니다. 기본값은 Off입니다. <br/>**참고**:Dispatcher 구성의 필터 규칙은 항상 원시 URL이 아닌 정리된 URL에 대해 평가됩니다. |




>[!NOTE]
>
>경로 항목은 Apache 웹 서버의 루트 디렉토리에 상대적입니다.

>[!NOTE]
>
>서버 헤더의 기본 설정은 다음과 같습니다.`  
ServerTokens Full` `  
DispatcherNoServerHeader 0`\
(통계적 목적을 위해) AEM 버전을 표시합니다. 헤더에서 사용할 수 있는 이러한 정보를 비활성화하려면 다음을 설정할 수 있습니다.`  
ServerTokens Prod`\
자세한 내용은 [ServerTokens Directive(예: Apache 2.4)에 대한 Apache 설명서](https://httpd.apache.org/docs/2.4/mod/core.html)를 참조하십시오.

**SetHandler**

이러한 항목 후에는 Dispatcher가 들어오는 요청을 처리할 수 있도록 구성 컨텍스트( `<Directory>`, `<Location>`)에 **SetHandler** 문을 추가해야 합니다. 다음 예제는 전체 웹 사이트에 대한 요청을 처리하도록 Dispatcher를 구성합니다.

**Windows 및 Unix**

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

다음 예에서는 가상 도메인에 대한 요청을 처리하도록 Dispatcher를 구성합니다.

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

**Unix**

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
**SetHandler** 문의 매개 변수는 모듈에 정의된 처리기의 이름이므로 위의 예제&#x200B;*와 동일하게*&#x200B;기록되어야 합니다.
이 명령에 대한 자세한 내용은 제공된 예제 구성 파일 및 Apache 웹 서버 설명서 를 참조하십시오.

**ModMimeUsePathInfo**

**SetHandler** 문 다음에 **ModMimeUsePathInfo** 정의도 추가해야 합니다.

>[!NOTE]
`ModMimeUsePathInfo` 매개 변수는 Dispatcher 버전 4.0.9 이상을 사용하는 경우에만 사용하고 구성해야 합니다.
(Dispatcher 버전 4.0.9는 2011년에 릴리스되었습니다. 이전 버전을 사용하는 경우 최신 Dispatcher 버전으로 업그레이드하는 것이 적절합니다.

모든 Apache 구성에 대해 **ModMimeUsePathInfo** 매개 변수를 `On`로 설정해야 합니다.

`ModMimeUsePathInfo On`

mod_mime 모듈(예: [Apache 모듈 mod_mime](https://httpd.apache.org/docs/2.4/mod/mod_mime.html) 참조)은 HTTP 응답용으로 선택한 컨텐츠에 컨텐츠 메타데이터를 할당하는 데 사용됩니다. 기본 설정은 mod_mime이 컨텐츠 유형을 결정할 때 파일 또는 디렉토리에 매핑되는 URL의 부분만 고려함을 의미합니다.

`On`에서 `ModMimeUsePathInfo` 매개 변수는 `mod_mime`가 *complete* URL을 기반으로 컨텐츠 유형을 결정하도록 지정합니다.즉, 가상 리소스에는 해당 확장을 기반으로 메타데이터가 적용됩니다.

다음 예에서는 **ModMimeUsePathInfo**&#x200B;를 활성화합니다.

**Windows 및 Unix**

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

### HTTPS(Unix 및 Linux)에 대한 지원 활성화 {#enable-support-for-https-unix-and-linux}

Dispatcher는 OpenSSL을 사용하여 HTTP를 통해 보안 커뮤니케이션을 구현합니다. Dispatcher 버전 **4.2.0**&#x200B;부터 OpenSSL 1.0.0 및 OpenSSL 1.0.1이 지원됩니다. Dispatcher는 기본적으로 OpenSSL 1.0.0을 사용합니다. OpenSSL 1.0.1을 사용하려면 다음 절차를 사용하여 Dispatcher가 설치된 OpenSSL 라이브러리를 사용하도록 심볼 링크를 만듭니다.

1. 터미널을 열고 현재 디렉토리를 OpenSSL 라이브러리가 설치된 디렉토리로 변경합니다. 예를 들면 다음과 같습니다.

   ```shell
   cd /usr/lib64
   ```

1. 심볼 링크를 만들려면 다음 명령을 입력합니다.

   ```shell
   ln -s libssl.so libssl.so.1.0.1
   ln -s libcrypto.so libcrypto.so.1.0.1
   ```

>[!NOTE]
사용자 지정된 Apache 버전을 사용하는 경우 Apache와 Dispatcher가 동일한 버전의 [OpenSSL](https://www.openssl.org/source/)을 사용하여 컴파일되었는지 확인하십시오.

### 다음 단계 {#next-steps-1}

Dispatcher 사용을 시작하려면 먼저 다음을 수행해야 합니다.

* [](dispatcher-configuration.md) Dispatcher 구성
* [Dispatcher](page-invalidate.md) 에서 작동하도록 AEM을 구성합니다.

## Sun Java System Web Server / iPlanet {#sun-java-system-web-server-iplanet}

>[!NOTE]
Windows 및 Unix 환경에 대한 지침은 여기에서 다룹니다.
실행할 항목을 선택할 때는 주의하십시오.

### Sun Java System Web Server / iPlanet - 웹 서버 {#sun-java-system-web-server-iplanet-installing-your-web-server} 설치

이러한 웹 서버를 설치하는 방법에 대한 자세한 내용은 해당 설명서를 참조하십시오.

* Sun Java System 웹 서버
* iPlanet Web Server

### Sun Java System Web Server / iPlanet - 디스패처 모듈 {#sun-java-system-web-server-iplanet-add-the-dispatcher-module} 추가

Dispatcher는 다음 중 하나로 제공됩니다.

* **Windows**:동적 링크 라이브러리(DLL)
* **Unix**:동적 공유 개체(DSO)

설치 아카이브 파일에는 Windows 또는 Unix를 선택했는지 여부에 따라 다음 파일이 포함됩니다.

| 파일 | 설명 |
|---|---|
| `disp_ns.dll` | Windows:Dispatcher 동적 링크 라이브러리 파일입니다. |
| `dispatcher.so` | Unix:Dispatcher 공유 개체 라이브러리 파일입니다. |
| `dispatcher.so` | Unix:예제 링크. |
| `obj.conf.disp` | iPlanet/Sun Java System 웹 서버의 구성 파일 예제. |
| `dispatcher.any` | Dispatcher에 대한 구성 파일 예제 . |
| README | 설치 지침 및 마지막 분 정보가 포함된 Readme 파일입니다. 참고:설치를 시작하기 전에 이 파일을 확인하십시오. |
| 변경 사항 | 현재 및 이전 릴리스에서 해결된 문제를 나열하는 파일을 변경합니다. |

다음 단계를 사용하여 웹 서버에 Dispatcher를 추가하십시오.

1. 웹 서버의 `plugin` 디렉토리에 Dispatcher 파일을 배치합니다.

### Sun Java System Web Server / iPlanet - Dispatcher {#sun-java-system-web-server-iplanet-configure-for-the-dispatcher} 구성

`obj.conf` 을 사용하여 웹 서버를 구성해야 합니다. Dispatcher 설치 키트에서 `obj.conf.disp` 구성 파일 예를 찾을 수 있습니다.

1. 다음으로 이동 `<WEBSERVER_ROOT>/config`.
1. 편집할 `obj.conf`을 엽니다.
1. 시작하는 줄을 복사합니다.\
   `Service fn="dispService"`\
   `obj.conf.disp`에서 `obj.conf`의 초기화 섹션으로 이동합니다.

1. 변경 사항을 저장합니다.
1. 편집할 `magnus.conf` 을 엽니다.
1. 시작하는 두 줄을 복사합니다.\
   `Init funcs="dispService, dispInit"`\
   및\
   `Init fn="dispInit"`\
   `obj.conf.disp`에서 `magnus.conf`의 초기화 섹션으로 이동합니다.

1. 변경 사항을 저장합니다.

>[!NOTE]
다음 구성은 모두 한 줄에 있어야 하며 `$(SERVER_ROOT)` 및 `$(PRODUCT_SUBDIR)`은(는) 각 값으로 대체해야 합니다.

**초기화**

다음 표에는 사용할 수 있는 예가 나와 있습니다.정확한 항목은 특정 웹 서버에 따라 다릅니다.

**Windows 및 Unix**

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
| config | 구성 파일 `dispatcher.any.`의 위치 및 이름 |
| 로그 파일 | 로그 파일의 위치 및 이름입니다. |
| loglevel | 로그 파일에 메시지를 쓸 때 로그 수준:<br/>**0** 오류 <br/>**1** 경고 <br/>**2** 정보 <br/>**3** 디버그 <br/>**참고:** 설치 및 테스트 중에 로그 수준을 3으로 설정하고 프로덕션 환경에서 실행할 때에는 0으로 설정하는 것이 좋습니다. |
| keepalivetimeout | keep-alive 시간 제한을 초 단위로 지정합니다. Dispatcher 버전 4.2.0부터 기본 keep-alive 값은 60입니다. 값이 0이면 keep-alive가 비활성화됩니다. |

요구 사항에 따라 Dispatcher를 개체에 대한 서비스로 정의할 수 있습니다. 전체 웹 사이트에 대한 Dispatcher를 구성하려면 기본 개체를 수정합니다.


**Windows**

```
...  
NameTrans fn="document-root" root="$(PRODUCT\_SUBDIR)\\dispcache"  
...  
Service fn="dispService" method="(GET|HEAD|POST)" type="\*\\\*"  
...
```

**Unix**

```
...  
NameTrans fn="document-root" root="$(PRODUCT\_SUBDIR)/dispcache"  
...  
Service fn="dispService" method="(GET|HEAD|POST)" type="\*/\*"  
...
```

### 다음 단계 {#next-steps-2}

Dispatcher 사용을 시작하려면 먼저 다음을 수행해야 합니다.

* [](dispatcher-configuration.md) Dispatcher 구성
* [Dispatcher](page-invalidate.md) 에서 작동하도록 AEM을 구성합니다.
