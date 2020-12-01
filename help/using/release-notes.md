---
title: AEM Dispatcher 릴리스 노트
seo-title: AEM Dispatcher 릴리스 노트
description: Adobe Experience Manager 발송자별 릴리스 노트
seo-description: Adobe Experience Manager 발송자별 릴리스 노트
uuid: ae3ccf62-0514-4c03-a3b9-71799a482cbd
topic-tags: release-notes
content-type: reference
products: SG_EXPERIENCEMANAGER/6.4
discoiquuid: ff3d38e0-71c9-4b41-85f9-fa896393aac5
translation-type: tm+mt
source-git-commit: 328bc82673783b4a2df2d68481fa7eec88b74b01
workflow-type: tm+mt
source-wordcount: '805'
ht-degree: 9%

---


# AEM Dispatcher 릴리스 노트{#aem-dispatcher-release-notes}

## 릴리스 정보 {#release-information}

|  |  |
|--- |--- |
| 제품 | Adobe Experience Manager(AEM) 디스패처 |
| 버전 | 4.3.3 |
| 유형 | 마이너 릴리스 |
| 날짜 | 2019년 18월 10일 |
| 다운로드 URL | <ul><li>[Apache 2.4](release-notes.md#apache)</li><li>[Microsoft IIS(인터넷 정보 서비스)](release-notes.md#iis)</li></ul> |
| 호환성 | AEM 6.1 이상 |

## 시스템 요구 사항 및 사전 요구 사항 {#system-requirements-and-prerequisites}

요구 사항 및 전제 조건에 대한 자세한 내용은 [지원되는 플랫폼](https://helpx.adobe.com/experience-manager/6-4/sites/deploying/using/technical-requirements.html) 페이지를 참조하십시오.

Adobe은 최신 기능, 최신 버그 수정 및 최상의 성능을 위해 최신 버전의 AEM 디스패처를 사용하는 것이 좋습니다.

## 설치 지침 {#installation-instructions}

자세한 내용은 [Dispatcher](dispatcher-install.md) 설치를 참조하십시오.

## 릴리스 내역 {#release-history}

### 릴리스 4.3.3(2019-Oct-18) {#october}

**버그 수정**:

* DISP-739 - LogLevel dispatcher:**level**&#x200B;이(가) 작동하지 않습니다.
* DISP-749 - Alpine Linux 디스패처가 추적 로그 수준에서 충돌합니다.

**향상된** 기능:

* DISP-813 - Dispatcher에서 Openssl 1.1.x 지원
* DISP-814 - 캐시 플러시 중 Apache 40x 오류
* DISP-818 - mod_expires는 실행 불가능한 컨텐츠에 대한 캐시-컨트롤 헤더를 추가합니다.
* DISP-821 - 소켓에 로그 컨텍스트를 저장하지 않음
* DISP-822 - 디스패처는 pselect 대신 투표를 사용해야 합니다.
* DISP-824 - Secure DispatcherUseForwardedHost
* DISP-825 - 디스크에 더 이상 공간이 없으면 특수 메시지를 기록합니다.
* DISP-826 - 쿼리 문자열이 있는 참조 URI 지원

**새로운 기능**:

* DISP-703 - 팜별 캐시 히트 비율
* DISP-827 - 테스트용 로컬 서버
* DISP-828 - 발송자용 테스트 문서 이미지 만들기

### 릴리스 4.3.2(2019-Jan-31) {#jan}

**버그 수정**:

* DISP-734 - 처리기로 설정되지 않은 경우 Dispatcher에서 insert_output_filter에 충돌이 발생합니다.
* DISP-735 - RE가 Alpean Linux에서 작동하지 않음
* DISP-740 - macOS Mojave에서 Dispatcher가 기본적으로 비활성화되어 있습니다.
* DISP-742 - 차단된 요청이 인증 검사기로 보호된 리소스로 정보를 누출할 수 있습니다.

**향상된** 기능:

* DISP-746 - dispatcher.any에서 레이블이 지정되지 않은 문자열은 경고를 생성합니다.

**새로운 기능**:

* DISP-747 - Apache 환경에서 요청 정보 제공

### 릴리스 4.3.1(2018-Oct-16) {#oct}

**버그 수정**:

* DISP-656 - 디스패처가 잘못된 ETag 헤더를 제공합니다.
* DISP-694 - 활성 상태 연결 유지 시 경고 표시
* DISP-714 - 쿠키 기반 세션 관리가 IIS에서 작동하지 않음
* DISP-715 - 렌더링 쿠키의 보안 플래그
* DISP-720 - 임시 파일을 닫지 않고 소모(열려 있는 파일이 너무 많음)할 수 있습니다.
* DISP-721 - Apache가 하위 항목을 정상적으로 다시 시작할 때 Dispatcher가 투표()를 중단합니다.
* DISP-722 - 캐시 파일은 8진수 모드 0600을 사용하여 만듭니다.
* DISP-723 - 렌더링 시간 초과가 0으로 설정된 경우 암시적 10분 시간 초과(및 다시 시도)
* DISP-725 - 문자열 뒤에 오는 문자가 자동으로 이름 없는 값으로 변환됩니다
* DISP-726 - 들어오는 호스트와 실제로 일치하는 팜이 없을 때 경고를 기록합니다.
* DISP-727 - 디스패처가 빈 캐시 파일에 대한 요청 컨텐츠 길이를 확인합니다.
* 디스패처를 통해 헤더 파일에 액세스하려고 할 때 DISP-730 - 404
* DISP-731 - 디스패처가 로그 삽입에 취약합니다.
* DISP-732 - 디스패처는 URL에서 연속된 &#39;/&#39;를 제거해야 합니다.
* DISP-733 - 발송자가 연령 헤더를 설정(계산)해야 합니다.

**향상된** 기능:

* DISP-656 - 디스패처가 잘못된 ETag 헤더를 제공합니다.
* DISP-694 - 활성 상태 연결 유지 시 경고 표시
* DISP-715 - 렌더링 쿠키의 보안 플래그
* DISP-722 - 캐시 파일은 8진수 모드 0600을 사용하여 만듭니다.
* DISP-726 - 들어오는 호스트와 실제로 일치하는 팜이 없을 때 경고를 기록합니다.

### 릴리스 4.3.0(2018-Jun-13) {#jun}

**버그 수정**:

* DISP-682 - 숫자 로그 수준이 잘못 적용됨
* DISP-685 - 32비트 Solaris SPARC 바이너리에 __div3에 대한 정의되지 않은 참조가 있습니다.
* DISP-688 - Dispatcher가 404 응답에서 &quot;X-Cache-Info&quot; 헤더를 반환하지 않습니다.
* DISP-690 - 마지막으로 수정한 헤더를 액세스할 수 없습니다.
* DISP-691 - w3wp.exe의 액세스 위반
* DISP-693 - 디스패처 다운로드 페이지에서 solaris 서버의 아키텍처 세부 사항을 업데이트해야 합니다.
* DISP-695 - Dispatcher 모듈 4.2.3의 DispatcherLog 수준 문제
* DISP-698 - Dispatcher TTL은 s-maxage 및 private 디렉티브를 지원해야 합니다.
* DISP-700 - Alpine Linux에서 모듈이 올바르게 작동하지 않음
* DISP-704 - %2b를 포함하는 브라우저 요청이 인코딩되지 않은 게시자로 전송됩니다.
* DISP-705 - Dispatcher가 두 번 없어지거나 손상되어 작동 중단(fasttop)
* DISP-706 - 무효화 시 디스패처가 무한 루프를 발생시킬 수 있는 백 참조 심링크
* DISP-709 - 일부 별칭 URL 확장 차단
* DISP-710 - Cent OS 6에서 사용할 수 없는 Linux용 빌드

**향상된** 기능:

* DISP-652 - 디스패처가 잘못된 날짜 헤더를 제공합니다.

## 유용한 리소스 {#helpful-resources}

* [AEM Dispatcher 개요](dispatcher.md)

## 다운로드 {#downloads}

### Apache 2.4 {#apache}

| 플랫폼 | 아키텍처 | OpenSSL 지원 | 다운로드 |
|---|---|---|---|
| Linux | i686(32비트) | 없음 | [dispatcher-apache2.4-linux-i686-4.3.3.tar.gz](https://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-linux-i686-4.3.3.tar.gz) |
| Linux | i686(32비트) | 1.0 | [dispatcher-apache2.4-linux-i686-ssl1.0-4.3.3.tar.gz](https://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-linux-i686-ssl1.0-4.3.3.tar.gz) |
| Linux | i686(32비트) | 1.1 | [dispatcher-apache2.4-linux-i686-ssl1.1-4.3.3.tar.gz](https://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-linux-i686-ssl1.1-4.3.3.tar.gz) |
| Linux | x86_64(64비트) | 없음 | [dispatcher-apache2.4-linux-x86_64-4.3.3.tar.gz](https://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-linux-x86_64-4.3.3.tar.gz) |
| Linux | x86_64(64비트) | 1.0 | [dispatcher-apache2.4-linux-x86_64-ssl1.0-4.3.3.tar.gz](https://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-linux-x86_64-ssl1.0-4.3.3.tar.gz) |
| Linux | x86_64(64비트) | 1.1 | [dispatcher-apache2.4-linux-x86_64-ssl1.1-4.3.3.tar.gz](https://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-linux-x86_64-ssl1.1-4.3.3.tar.gz) |
| macOS | x86_64(64비트) | 없음 | [dispatcher-apache2.4-darwin-x86_64-4.3.3.tar.gz](https://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-darwin-x86_64-4.3.3.tar.gz) |

### IIS {#iis}

| 플랫폼 | 아키텍처 | OpenSSL 지원 | 다운로드 |
|---|---|---|---|
| Windows | x86(32비트) | 없음 | [dispatcher-iis-windows-x86-4.3.3.zip](https://download.macromedia.com/dispatcher/download/dispatcher-iis-windows-x86-4.3.3.zip) |
| Windows | x86(32비트) | 1.0 | [dispatcher-iis-windows-x86-ssl1.0-4.3.zip](https://download.macromedia.com/dispatcher/download/dispatcher-iis-windows-x86-ssl1.0-4.3.3.zip) |
| Windows | x86(32비트) | 1.1 | [dispatcher-iis-windows-x86-ssl1.1-4.3.zip](https://download.macromedia.com/dispatcher/download/dispatcher-iis-windows-x86-ssl1.1-4.3.3.zip) |
| Windows | x64(64비트) | 없음 | [dispatcher-iis-windows-x64-4.3.3.zip](https://download.macromedia.com/dispatcher/download/dispatcher-iis-windows-x64-4.3.3.zip) |
| Windows | x64(64비트) | 1.0 | [dispatcher-iis-windows-x64-ssl1.0-4.3.zip](https://download.macromedia.com/dispatcher/download/dispatcher-iis-windows-x64-ssl1.0-4.3.3.zip) |
| Windows | x64(64비트) | 1.1 | [dispatcher-iis-windows-x64-ssl1.1-4.3.zip](https://download.macromedia.com/dispatcher/download/dispatcher-iis-windows-x64-ssl1.1-4.3.3.zip) |
