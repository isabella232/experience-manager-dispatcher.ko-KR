---
title: AEM Dispatcher 릴리스 노트
seo-title: AEM Dispatcher 릴리스 노트
description: Adobe Experience Manager Dispatcher에 대한 릴리스 노트
seo-description: Adobe Experience Manager Dispatcher에 대한 릴리스 노트
uuid: ae3ccf62-0514-4c03-a3b9-71799a482cbd
topic-tags: 릴리스 노트
content-type: 참조
products: SG_EXPERIENCEMANAGER/6.4
discoiquuid: ff3d38e0-71c9-4b41-85f9-fa896393aac5
translation-type: tm+mt
source-git-commit: 328bc82673783b4a2df2d68481fa7eec88b74b01

---


# AEM Dispatcher 릴리스 노트{#aem-dispatcher-release-notes}

## 릴리스 정보 {#release-information}

|  |  |
|--- |--- |
| 제품 | AEM(Adobe Experience Manager) 디스패처 |
| 버전 | 4.3.3 |
| 유형 | 마이너 릴리스 |
| 날짜 | 2019년 10월 18일 |
| URL 다운로드 | <ul><li>[Apache 2.4](release-notes.md#apache)</li><li>[Microsoft IIS(인터넷 정보 서비스)](release-notes.md#iis)</li></ul> |
| 호환성 | AEM 6.1 이상 |

## 시스템 요구 사항 및 사전 요구 사항 {#system-requirements-and-prerequisites}

요구 사항 및 [전제 조건에 대한](https://helpx.adobe.com/experience-manager/6-4/sites/deploying/using/technical-requirements.html) 자세한 내용은 지원되는 플랫폼 페이지를 참조하십시오.

최신 버전의 AEM Dispatcher를 사용하여 최신 기능, 최신 버그 수정 및 최상의 성능을 구현할 것을 적극 권장합니다.

## 설치 지침 {#installation-instructions}

자세한 지침은 Dispatcher [설치를 참조하십시오](dispatcher-install.md).

## 릴리스 내역 {#release-history}

### 릴리스 4.3.3(2019-10월-18) {#october}

**버그 수정**:

* DISP-739 - LogLevel dispatcher:수준이 **** 작동하지 않음
* DISP-749 - Alpine Linux 디스패처 충돌(추적 로그 수준)

**향상된 기능**:

* DISP 파섹 - DisPATCHER에서 Open 1.1.X 지원
* DISP 파섹-814 - 캐시 플러시 중 Apache 40X 오류
* DISP-818 - mod_expires는 실행 불가능한 컨텐츠에 대한 캐시 제어 헤더를 추가합니다.
* DISP 파섹-821 - 소켓에 로그 컨텍스트를 저장하지 않음
* DISP-822 - 디스패처는 pselect 대신 ppoll을 사용해야 합니다.
* DISP-824 - 보안 DispatcherUseForwardedHost
* DISP 파섹-825 - 디스크에 더 이상 공간이 없으면 특수 메시지를 기록합니다.
* DISP 파섹-826 - 쿼리 문자열이 있는 참조 URI 지원

**새로운 기능**:

* DISP 파섹 - 팜별 캐시 히트 비율
* DISP 파섹 - 테스트를 위한 로컬 서버
* DISP 파섹-828 - 디스패처용 테스트 도커 이미지 만들기

### 릴리스 4.3.2(2019-1월 31일) {#jan}

**버그 수정**:

* DISP-734 - 처리기로 설정되지 않은 경우 Dispatcher에서 insert_output_filter에 충돌이 발생합니다.
* DISP-735 - RE가 Alpine Linux에서 작동하지 않음
* DISP-740 - macOS Mojave에서 디스패처 로딩이 기본적으로 비활성화되어 있습니다.
* DISP 파섹-742 - 차단된 요청이 인증 검사기로 보호된 리소스로 정보를 누출할 수 있습니다.

**향상된 기능**:

* DISP 파섹-746 - DISPATCHER.ANY에서 레이블이 지정되지 않은 문자열이 경고를 생성합니다.

**새로운 기능**:

* DISP-747 - Apache 환경에서 요청 정보 제공

### 릴리스 4.3.1(2018-10월-16) {#oct}

**버그 수정**:

* DISP-656 - 디스패처가 잘못된 ETag 헤더를 제공합니다.
* DISP 파섹-694 - 활성 상태 연결 유지 시 경고 표시
* DISP 파섹-714 - 쿠키 기반 세션 관리가 IIS에서 작동하지 않음
* DISP-715 - 렌더링 쿠키의 보안 플래그
* DISP 파섹-720 - 임시 파일을 닫지 않으면 소진(열려 있는 파일이 너무 많음)할 수 있습니다.
* DISP-721 - Dispatcher가 Apache가 자식을 정상적으로 다시 시작할 때 투표()를 중단합니다.
* DISP 파섹-722 - 캐시 파일은 8진수 모드 0600으로 만들어집니다.
* DISP 파섹-723 - 렌더링 시간 초과가 0으로 설정된 경우 암시적 10분 제한(및 다시 시도)
* DISP 파섹-725 - 문자열 뒤에 오는 문자가 자동으로 이름 없는 값으로 변환됩니다.
* DISP 파섹-726 - 들어오는 호스트와 실제로 일치하는 팜이 없을 때 경고를 기록합니다.
* DISP-727 - Dispatcher가 빈 캐시 파일에 대한 요청 컨텐츠 길이를 확인합니다.
* 디스패처를 통해 헤더 파일에 액세스하려고 할 때 DISP-730 - 404
* DISP-731 - 디스패처가 로그 삽입에 취약합니다.
* DISP 파섹-732 - 디스패처는 URL에서 연속된 '/'를 제거해야 합니다.
* DISP-733 - 디스패처가 연령 헤더를 설정(계산)해야 함

**향상된 기능**:

* DISP-656 - 디스패처가 잘못된 ETag 헤더를 제공합니다.
* DISP 파섹-694 - 활성 상태 연결 유지 시 경고 표시
* DISP-715 - 렌더링 쿠키의 보안 플래그
* DISP 파섹-722 - 캐시 파일은 8진수 모드 0600으로 만들어집니다.
* DISP 파섹-726 - 들어오는 호스트와 실제로 일치하는 팜이 없을 때 경고를 기록합니다.

### 릴리스 4.3.0(2018-6-13) {#jun}

**버그 수정**:

* DISP 파섹 - 숫자 로그 수준이 잘못 적용됨
* DISP-685 - 32비트 Solaris SPARC 바이너리에 __didi3에 대한 정의되지 않은 참조가 있습니다.
* DISP-688 - Dispatcher가 404 응답에서 "X-Cache-Info" 헤더를 반환하지 않음
* DISP 파섹-690 - 마지막으로 수정한 헤더를 캐시할 수 없습니다.
* DISP-691 - w3wp.exe의 액세스 위반
* DISP 파섹 - 디스패처 다운로드 페이지에서 Solaris 서버에 대한 아키텍처 세부 사항을 업데이트해야 합니다.
* DISP 파섹 - DisPATCHER 모듈 4.2.3의 DisPATCHERLog 수준 문제
* DISP-698 - 디스패처 TTL은 s-maxage 및 private 디렉티브를 지원해야 합니다.
* DISP-700 - Alpine Linux에서 모듈이 제대로 작동하지 않음
* DISP-704 - %2b을(를) 포함하는 브라우저 요청이 인코딩되지 않은 게시자로 전송됩니다.
* DISP 파섹-705 - DisPATCHER가 두 번 무료 또는 손상으로 충돌(빠른 상단)
* DISP 파섹-706 - 무효화 동안 디스패처가 무한 루프를 발생시킬 수 있는 백 참조 기호 링크를 따릅니다.
* DISP 파섹-709 - 일부 별칭 URL 확장 차단
* DISP-710 - Cent OS 6에서 사용할 수 없는 Linux용 빌드

**향상된 기능**:

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
| Windows | x86(32비트) | 1.0 | [dispatcher-iis-windows-x86-ssl1.0-4.3.3.zip](https://download.macromedia.com/dispatcher/download/dispatcher-iis-windows-x86-ssl1.0-4.3.3.zip) |
| Windows | x86(32비트) | 1.1 | [dispatcher-iis-windows-x86-ssl1.1-4.3.3.zip](https://download.macromedia.com/dispatcher/download/dispatcher-iis-windows-x86-ssl1.1-4.3.3.zip) |
| Windows | x64(64비트) | 없음 | [dispatcher-iis-windows-x64-4.3.3.zip](https://download.macromedia.com/dispatcher/download/dispatcher-iis-windows-x64-4.3.3.zip) |
| Windows | x64(64비트) | 1.0 | [dispatcher-iis-windows-x64-ssl1.0-4.3.3.zip](https://download.macromedia.com/dispatcher/download/dispatcher-iis-windows-x64-ssl1.0-4.3.3.zip) |
| Windows | x64(64비트) | 1.1 | [dispatcher-iis-windows-x64-ssl1.1-4.3.3.zip](https://download.macromedia.com/dispatcher/download/dispatcher-iis-windows-x64-ssl1.1-4.3.3.zip) |
