---
title: AEM Dispatcher 릴리스 노트
seo-title: AEM Dispatcher 릴리스 노트
description: Adobe Experience Manager Dispatcher에 관한 릴리스 노트
seo-description: Adobe Experience Manager Dispatcher에 관한 릴리스 노트
uuid: ae3ccf62-0514-4c03-a3b9-71799a482cbd
topic-tags: release-notes
content-type: reference
products: SG_EXPERIENCEMANAGER/6.4
discoiquuid: ff3d38e0-71c9-4b41-85f9-fa896393aac5
exl-id: b55c7a34-d57b-4d45-bd83-29890f1524de
source-git-commit: 3a0e237278079a3885e527d7f86989f8ac91e09d
workflow-type: tm+mt
source-wordcount: '805'
ht-degree: 10%

---

# AEM Dispatcher 릴리스 노트{#aem-dispatcher-release-notes}

## 릴리스 정보 {#release-information}

|  |  |
|--- |--- |
| 제품 | Adobe Experience Manager(AEM) Dispatcher |
| 버전 | 4.3.3 |
| 유형 | 마이너 릴리스 |
| 날짜 | 2019년 10월 18일 |
| 다운로드 URL | <ul><li>[Apache 2.4](release-notes.md#apache)</li><li>[Microsoft IIS(인터넷 정보 서비스)](release-notes.md#iis)</li></ul> |
| 호환성 | AEM 6.1 이상 |

## 시스템 요구 사항 및 사전 요구 사항 {#system-requirements-and-prerequisites}

요구 사항 및 사전 요구 사항에 대한 자세한 내용은 [지원되는 플랫폼](https://helpx.adobe.com/experience-manager/6-4/sites/deploying/using/technical-requirements.html) 페이지를 참조하십시오.

Adobe은 최신 기능, 최신 버그 수정 및 최상의 성능을 이용할 수 있도록 최신 버전의 AEM Dispatcher를 사용하는 것이 좋습니다.

## 설치 지침 {#installation-instructions}

자세한 지침은 [Dispatcher 설치](dispatcher-install.md)를 참조하십시오.

## 릴리스 내역 {#release-history}

### 릴리스 4.3.3(2019년 10월 18일) {#october}

**버그 수정**:

* DISP-739 - LogLevel 디스패처:**level**&#x200B;이 작동하지 않습니다.
* DISP-749 - Alpine Linux 디스패처가 추적 로그 레벨을 사용하여 충돌합니다.

**개선 사항**:

* DISP-813 - Dispatcher에서 openssl 1.1.x 지원
* DISP-814 - 캐시 플러시 중 Apache 40x 오류
* DISP-818 - mod_expires는 실행 불가능한 컨텐츠에 대해 캐시 제어 헤더를 추가합니다
* DISP-821 - 로그 컨텍스트를 소켓에 저장하지 않음
* DISP-822 - Dispatcher는 pselect 대신 폴링을 사용해야 합니다.
* DISP-824 - 보안 DispatcherUseForwardedHost
* DISP-825 - 디스크에 더 이상 공간이 없으면 특수 메시지를 기록합니다
* DISP-826 - 쿼리 문자열로 참조 URI 지원

**새로운 기능**:

* DISP-703 - 팜 특정 캐시 적중률
* DISP-827 - 테스트용 로컬 서버
* DISP-828 - Dispatcher용 테스트 Docker 이미지 만들기

### 릴리스 4.3.2(2019년 1월 31일) {#jan}

**버그 수정**:

* DISP-734 - 처리기로 설정되지 않은 경우 Dispatcher가 insert_output_filter에서 충돌을 발생합니다
* DISP-735 - RE가 Alpine Linux에서 작동하지 않음
* DISP-740 - macOS Mojave에서 Dispatcher 로드를 기본적으로 사용할 수 없습니다
* DISP-742 - 차단된 요청이 인증 검사기로 보호된 리소스에 대한 정보를 누출할 수 있음

**개선 사항**:

* DISP-746 - dispatcher.any에서 레이블이 지정되지 않은 문자열이 경고를 생성해야 함

**새로운 기능**:

* DISP-747 - Apache 환경에서 요청 정보 제공

### 릴리스 4.3.1(2018년 10월 16일) {#oct}

**버그 수정**:

* DISP-656 - Dispatcher가 잘못된 ETag 헤더를 제공합니다
* DISP-694 - 연결 상태를 유지할 때 경고 표시 안 함
* DISP-714 - 쿠키 기반 세션 관리가 IIS에서 작동하지 않습니다
* DISP-715 - renderid 쿠키에 대한 보안 플래그
* DISP-720 - 임시 파일이 닫히지 않아 고갈될 수 있습니다(열려 있는 파일이 너무 많음).
* DISP-721 - Apache가 하위 항목을 올바르게 다시 시작할 때 Dispatcher 중단 폴링()이 발생합니다
* DISP-722 - 캐시 파일이 8차 모드 0600으로 만들어집니다.
* DISP-723 - 렌더링 시간 초과가 0으로 설정된 경우 암시적 10분 시간 초과(및 다시 시도)
* DISP-725 - 문자열 뒤에 오는 후행 문자가 자동으로 이름 없는 값으로 변환됩니다
* DISP-726 - 들어오는 호스트와 실제로 일치하는 팜이 없으면 경고를 기록합니다
* DISP-727 - Dispatcher가 빈 캐시 파일에 대한 요청 컨텐츠 길이를 확인합니다
* Dispatcher를 통해 헤더 파일에 액세스하려고 할 때 DISP-730 - 404
* DISP-731 - Dispatcher가 로그 주입에 취약합니다.
* DISP-732 - Dispatcher가 URL에서 연속된 &#39;/&#39;를 제거해야 합니다
* DISP-733 - Dispatcher가 연령 헤더를 설정(계산)해야 합니다

**개선 사항**:

* DISP-656 - Dispatcher가 잘못된 ETag 헤더를 제공합니다
* DISP-694 - 연결 상태를 유지할 때 경고 표시 안 함
* DISP-715 - renderid 쿠키에 대한 보안 플래그
* DISP-722 - 캐시 파일이 8차 모드 0600으로 만들어집니다.
* DISP-726 - 들어오는 호스트와 실제로 일치하는 팜이 없으면 경고를 기록합니다

### 릴리스 4.3.0(2018년 6월 13일) {#jun}

**버그 수정**:

* DISP-682 - 잘못 적용된 숫자 로그 수준
* DISP-685 - 32비트 Solaris SPARC 바이너리에 __didi3에 대한 정의되지 않은 참조가 있습니다.
* DISP-688 - Dispatcher가 404 응답에서 &quot;X-Cache-Info&quot; 헤더를 반환하지 않습니다
* DISP-690 - 마지막 수정 헤더에 액세스할 수 없습니다.
* DISP-691 - w3wp.exe의 액세스 위반
* DISP-693 - dispatcher 다운로드 페이지에서 solaris 서버에 대한 아키텍처 세부 사항을 업데이트해야 합니다.
* DISP-695 - Dispatcher 모듈 4.2.3의 DispatcherLog 수준 문제
* DISP-698 - Dispatcher TTL은 s-maxage 및 private 지시어를 지원해야 합니다.
* DISP-700 - Alpine Linux에서 모듈이 제대로 작동하지 않습니다.
* DISP-704 - %2b가 포함된 브라우저 요청이 인코딩되지 않은 게시자에게 전송됩니다
* DISP-705 - 이중 사용 가능 또는 손상으로 인한 디스패처 충돌(상단)
* DISP-706 - 무효화 동안 디스패처가 무한 루프를 생성할 수 있는 백 참조 symlink를 따릅니다
* DISP-709 - 일부 별칭 URL 확장 차단
* DISP-710 - Cent OS 6에서 사용할 수 없는 Linux용 빌드

**개선 사항**:

* DISP-652 - Dispatcher가 잘못된 날짜 헤더를 제공합니다.

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
| Windows | x86(32비트) | 1.0 | [dispatcher-iis-windows-x86-ssl1.0-4.3.3.zip](https://download.macromedia.com/dispatcher/download/dispatcher-iis-windows-x86-ssl1.0-4.3.3.zip) |
| Windows | x86(32비트) | 1.1 | [dispatcher-iis-windows-x86-ssl1.1-4.3.3.zip](https://download.macromedia.com/dispatcher/download/dispatcher-iis-windows-x86-ssl1.1-4.3.3.zip) |
| Windows | x64(64비트) | 없음 | [dispatcher-iis-windows-x64-4.3.3.zip](https://download.macromedia.com/dispatcher/download/dispatcher-iis-windows-x64-4.3.3.zip) |
| Windows | x64(64비트) | 1.0 | [dispatcher-iis-windows-x64-ssl1.0-4.3.3.zip](https://download.macromedia.com/dispatcher/download/dispatcher-iis-windows-x64-ssl1.0-4.3.3.zip) |
| Windows | x64(64비트) | 1.1 | [dispatcher-iis-windows-x64-ssl1.1-4.3.3.zip](https://download.macromedia.com/dispatcher/download/dispatcher-iis-windows-x64-ssl1.1-4.3.3.zip) |
