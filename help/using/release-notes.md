---
title: AEM Dispatcher 릴리스 노트
seo-title: AEM Dispatcher 릴리스 노트
description: Adobe Experience Manager Dispatcher 전용 릴리스 노트
seo-description: Adobe Experience Manager Dispatcher 전용 릴리스 노트
uuid: AE 3 CCF 62-0514-4 C 03-A 3 B 9-71799 A 482 CBD
topic-tags: 릴리스 노트
content-type: 참조
products: sg_ Experiencemanager/6.4
discoiquuid: ff 3 d 38 e 0-71 c 9-4 b 41-85 f 9-fa 896393 aac 5
translation-type: tm+mt
source-git-commit: 2d72839d459973cba40f6a938ee157198c7cf50a

---


# AEM Dispatcher 릴리스 노트{#aem-dispatcher-release-notes}

## 릴리스 정보 {#release-information}

|  |
|--- |--- |
| 제품 | AEM (Adobe Experience Manager) Dispatcher |
| 버전 | 4.3.2 |
| 유형 | 마이너 릴리스 |
| 날짜 | 2019 년 1 월 31 일 |
| 다운로드 URL | <ul><li>[Apache 2.4](release-notes.md#apache)</li><li>[Microsoft IIS (Internet Information Services)](release-notes.md#iis)</li></ul> |
| 호환성 | AEM 6.1 이상 |

## 시스템 요구 사항 및 전제 조건 {#system-requirements-and-prerequisites}

요구 사항 및 사전 요구 사항에 대한 자세한 내용은 [지원되는 플랫폼](https://helpx.adobe.com/experience-manager/6-4/sites/deploying/using/technical-requirements.html) 페이지를 참조하십시오.

최신 기능, 최신 버그 수정 및 최상의 성능을 제공하는 데 최신 버전의 AEM Dispatcher를 사용하는 것이 좋습니다.

## 설치 지침 {#installation-instructions}

자세한 지침은 Dispatcher [설치를](dispatcher-install.md)참조하십시오.

## 릴리스 내역 {#release-history}

### 릴리스 4.3.2 (2019-JAN -31) {#jan}

**버그 수정**:

* DISP -734 - 핸들러로 설정하지 않은 경우 INSERT_ OUTPUT_ FILTER에서 디스패처가 충돌합니다.
* Disp -735 - RES do not work on alpha linux
* DISP -740 - macos Mojave에서 디스패처가 기본적으로 비활성화됩니다.
* DISP -742 - 차단된 요청이 인증 Checker 보호된 리소스로 정보를 누출할 수 있습니다.

**향상된**기능:

* Disp -746 - Dispatcher의 레이블되지 않은 문자열. Any should generate a warning

**새로운 기능**:

* Disp -747 - Apache 환경에서 요청 정보 제공

### 릴리스 4.3.1 (2018-10 월 -16 일) {#oct}

**버그 수정**:

* Disp -656 - 디스패처가 잘못된 ETAG 헤더를 제공합니다.
* DISP -694 - 라이브 연결이 끊긴 상태에서 경고 무시
* DISP -714 - IIS에서 쿠키 기반 세션 관리가 작동하지 않음
* disp -715 - renderid 쿠키에 대한 보안 플래그
* DISP -720 - 임시 파일이 닫히지 않으면 고갈될 수 있음 (너무 많은 열린 파일)
* DISP -721 - Apache는 Apache가 하위 항목을 정상적으로 다시 시작할 때 투표 인터럽트 () 를 중지합니다.
* DISP -722 - 캐시 파일이 8 진수 모드로 만들어집니다. 0600
* DISP -723 - 렌더링 시간 초과가 0로 설정된 경우 암시적 10 분 제한 (및 재시도)
* DISP -725 - 문자열이 이름 없는 값으로 자동 변환된 후 후행 문자
* DISP -726 - 실제 들어오는 호스트와 일치하는 팜이 없을 때 경고 기록
* DISP -727 - 디스패처가 빈 캐시 파일에 대한 컨텐츠 길이 요청
* Disp -730 - 404 - Dispatcher를 통해 헤더 파일에 액세스하려고 할 때
* DISP -731 - 디스패처가 로그 주입에 취약함
* DISP -732 - 디스패처가 URL에서 연속 &#39;/&#39; 를 제거해야 함
* Disp -733 - 디스패처가 연령 헤더를 설정 (계산) 해야 함

**향상된**기능:

* Disp -656 - 디스패처가 잘못된 ETAG 헤더를 제공합니다.
* DISP -694 - 라이브 연결이 끊긴 상태에서 경고 무시
* disp -715 - renderid 쿠키에 대한 보안 플래그
* DISP -722 - 캐시 파일이 8 진수 모드로 만들어집니다. 0600
* DISP -726 - 실제 들어오는 호스트와 일치하는 팜이 없을 때 경고 기록

### 릴리스 4.3.0 (2018-Jun -13) {#jun}

**버그 수정**:

* DISP -682 - 숫자 로그 수준이 잘못 적용되었습니다.
* Disp -685 - 32 비트 Solaris SPARC 바이너리는__ divdi 3에 대한 정의되지 않은 참조를 가집니다.
* Disp -688 - Dispatcher가 404 응답 시 &quot;x-cache-info&quot; 헤더를 반환하지 않음
* DISP -690 - 마지막으로 수정한 헤더를 캐시할 수 없음
* disp -691 - w 3 wp. exe의 액세스 위반
* disp -693 - Dispatcher 다운로드 페이지에서 Solaris 서버용 아키텍처 세부 사항을 업데이트해야 함
* Disp -695 - Dispatcher Module 4.2.3의 dispatcherlog 수준 문제
* DISP -698 - 발송자 TTL 이 s-maxage 및 개인 지침을 지원해야 함
* Disp -700 - 알파인 Linux에서 모듈이 올바르게 작동하지 않음
* DISP -704 - % 2 b를 포함하는 브라우저 요청이 게시자에게 인코딩되지 않은 상태로 전송됩니다.
* DISP -705 - Double Free 또는 Corruption (fasttop) 로 인한 디스패처 충돌
* DISP -706 - 무효화하는 동안 디스패처가 무한 루프가 발생할 수 있는 참조 Symlinks를 뒤따릅니다.
* DISP -709 - 일부 별칭 URL 확장 차단
* DISP -710 - SE OS 6에서 사용할 수 없는 Linux 용 빌드

**향상된**기능:

* Disp -652 - 디스패처가 잘못된 날짜 헤더를 제공합니다.

## 유용한 리소스 {#helpful-resources}

* [AEM Dispatcher 개요](dispatcher.md)

## 다운로드 {#downloads}

### Apache 2.4 {#apache}

| 플랫폼 | 아키텍처 | SSL 지원 | 다운로드 |
|---|---|---|---|
| AIX | Powerpc (32 비트) | 아니오 | [dispatcher-apache2.4-aix-powerpc-4.3.2.tar.gz](http://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-aix-powerpc-4.3.2.tar.gz) |
| AIX | Powerpc (32 비트) | 예 | [dispatcher-apache2.4-aix-powerpc-ssl-4.3.2.tar.gz](http://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-aix-powerpc-ssl-4.3.2.tar.gz) |
| AIX | Powerpc (64 비트) | 아니오 | [dispatcher-apache2.4-aix-powerpc64-4.3.2.tar.gz](http://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-aix-powerpc64-4.3.2.tar.gz) |
| AIX | Powerpc (64 비트) | 예 | [dispatcher-apache2.4-aix-powerpc64-ssl-4.3.2.tar.gz](http://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-aix-powerpc64-ssl-4.3.2.tar.gz) |
| Linux | i 686 (32 비트) | 아니오 | [dispatcher-apache2.4-linux-i686-4.3.2.tar.gz](http://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-linux-i686-4.3.2.tar.gz) |
| Linux | i 686 (32 비트) | 예 | [dispatcher-apache2.4-linux-i686-ssl-4.3.2.tar.gz](http://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-linux-i686-ssl-4.3.2.tar.gz) |
| Linux | x 86_ 64 (64 비트) | 아니오 | [dispatcher-apache2.4-linux-x86_64-4.3.2.tar.gz](http://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-linux-x86_64-4.3.2.tar.gz) |
| Linux | x 86_ 64 (64 비트) | 예 | [dispatcher-apache2.4-linux-x86_64-ssl-4.3.2.tar.gz](http://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-linux-x86_64-ssl-4.3.2.tar.gz) |
| macOS | x 86_ 64 (64 비트) | 아니오 | [dispatcher-apache2.4-darwin-x86_64-4.3.2.tar.gz](http://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-darwin-x86_64-4.3.2.tar.gz) |
| Solaris | AMD (32 비트) | 아니오 | [dispatcher-apache2.4-solaris-i386-4.3.2.tar.gz](http://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-solaris-i386-4.3.2.tar.gz) |
| Solaris | AMD (32 비트) | 예 | [dispatcher-apache2.4-solaris-i386-ssl-4.3.2.tar.gz](http://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-solaris-i386-ssl-4.3.2.tar.gz) |
| Solaris | AMD (64 비트) | 아니오 | [dispatcher-apache2.4-solaris-amd64-4.3.2.tar.gz](http://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-solaris-amd64-4.3.2.tar.gz) |
| Solaris | AMD (64 비트) | 예 | [dispatcher-apache2.4-solaris-amd64-ssl-4.3.2.tar.gz](http://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-solaris-amd64-ssl-4.3.2.tar.gz) |
| Solaris | SPARC (32 비트) | 아니오 | [dispatcher-apache2.4-solaris-sparc-4.3.2.tar.gz](http://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-solaris-sparc-4.3.2.tar.gz) |
| Solaris | SPARC (32 비트) | 예 | [dispatcher-apache2.4-solaris-sparc-ssl-4.3.2.tar.gz](http://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-solaris-sparc-ssl-4.3.2.tar.gz) |
| Solaris | SPARC (64 비트) | 아니오 | [dispatcher-apache2.4-solaris-sparcv9-4.3.2.tar.gz](http://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-solaris-sparcv9-4.3.2.tar.gz) |
| Solaris | SPARC (64 비트) | 예 | [dispatcher-apache2.4-solaris-sparcv9-ssl-4.3.2.tar.gz](http://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-solaris-sparcv9-ssl-4.3.2.tar.gz) |

### IIS {#iis}

| 플랫폼 | 아키텍처 | SSL 지원 | 다운로드 |
|---|---|---|---|
| Windows | x 86 (32 비트) | 아니오 | [dispatcher-iis-windows-x86-4.3.2.zip](http://download.macromedia.com/dispatcher/download/dispatcher-iis-windows-x86-4.3.2.zip) |
| Windows | x 86 (32 비트) | 예 | [dispatcher-iis-windows-x86-ssl-4.3.2.zip](http://download.macromedia.com/dispatcher/download/dispatcher-iis-windows-x86-ssl-4.3.2.zip) |
| Windows | x 64 (64 비트) | 아니오 | [dispatcher-iis-windows-x64-4.3.2.zip](http://download.macromedia.com/dispatcher/download/dispatcher-iis-windows-x64-4.3.2.zip) |
| Windows | x 64 (64 비트) | 예 | [dispatcher-iis-windows-x64-ssl-4.3.2.zip](http://download.macromedia.com/dispatcher/download/dispatcher-iis-windows-x64-ssl-4.3.2.zip) |
