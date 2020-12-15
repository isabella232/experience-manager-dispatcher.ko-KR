---
title: Dispatcher에 SSL 사용
seo-title: Dispatcher에 SSL 사용
description: SSL 연결을 사용하여 AEM과 통신하도록 Dispatcher를 구성하는 방법을 알아봅니다.
seo-description: SSL 연결을 사용하여 AEM과 통신하도록 Dispatcher를 구성하는 방법을 알아봅니다.
uuid: 1a8f448c-d3d8-4798-a5cb-9579171171ed
contentOwner: User
products: SG_EXPERIENCEMANAGER/DISPATCHER
topic-tags: dispatcher
content-type: reference
discoiquuid: 771cfd85-6c26-4ff2-a3fe-dff8d8f7920b
index: y
internal: n
snippet: y
translation-type: tm+mt
source-git-commit: f9fb0e94dbd1c67bf87463570e8b5eddaca11bf3
workflow-type: tm+mt
source-wordcount: '1375'
ht-degree: 0%

---


# Dispatcher에 SSL 사용 {#using-ssl-with-dispatcher}

디스패처와 렌더링 컴퓨터 간의 SSL 연결 사용:

* [단방향 SSL](#use-ssl-when-dispatcher-connects-to-aem)
* [상호 SSL](#configuring-mutual-ssl-between-dispatcher-and-aem)

>[!NOTE]
>
>SSL 인증서와 관련된 작업은 제3자 제품에 바인딩됩니다. Adobe Platinum 유지 관리 및 지원 계약의 적용을 받지 않습니다.

## Dispatcher가 AEM {#use-ssl-when-dispatcher-connects-to-aem}에 연결할 때 SSL 사용

SSL 연결을 사용하여 AEM 또는 CQ 렌더링 인스턴스와 통신하도록 Dispatcher를 구성합니다.

Dispatcher를 구성하려면 먼저 AEM 또는 CQ가 SSL을 사용하도록 구성합니다.

* AEM 6.2:[SSL을 통해 HTTP 활성화](https://helpx.adobe.com/experience-manager/6-2/sites/deploying/using/config-ssl.html)
* AEM 6.1:[SSL을 통해 HTTP 활성화](https://docs.adobe.com/content/docs/en/aem/6-1/deploy/configuring/config-ssl.html)
* 이전 AEM 버전:[이 페이지](https://helpx.adobe.com/experience-manager/aem-previous-versions.html)를 참조하십시오.

### SSL 관련 요청 헤더 {#ssl-related-request-headers}

Dispatcher가 HTTPS 요청을 받으면 Dispatcher는 AEM 또는 CQ로 보내는 후속 요청에 다음 헤더를 포함합니다.

* `X-Forwarded-SSL`
* `X-Forwarded-SSL-Cipher`
* `X-Forwarded-SSL-Keysize`
* `X-Forwarded-SSL-Session-ID`

`mod_ssl`이(가) 있는 Apache-2.4를 통한 요청에는 다음 예와 유사한 헤더가 포함됩니다.

```shell
X-Forwarded-SSL: on
X-Forwarded-SSL-Cipher: DHE-RSA-AES256-SHA
X-Forwarded-SSL-Session-ID: 814825E8CD055B4C166C2EF6D75E1D0FE786FFB29DEB6DE1E239D5C771CB5B4D
```

### SSL {#configuring-dispatcher-to-use-ssl}을 사용하도록 Dispatcher 구성

SSL을 통해 AEM 또는 CQ와 연결하도록 Dispatcher를 구성하려면 [dispatcher.any](dispatcher-configuration.md) 파일에 다음 속성이 필요합니다.

* HTTPS 요청을 처리하는 가상 호스트입니다.
* 가상 호스트의 `renders` 섹션에는 HTTPS를 사용하는 CQ 또는 AEM 인스턴스의 호스트 이름과 포트를 식별하는 항목이 포함됩니다.
* `renders` 항목에는 `1` 값의 `secure` 속성이 포함됩니다.

참고:필요한 경우 HTTP 요청을 처리할 다른 가상 호스트를 만듭니다.

다음 예제 dispatcher.any 파일은 HTTPS를 사용하여 호스트 `localhost` 및 포트 `8443`에서 실행되는 CQ 인스턴스에 연결하는 속성 값을 보여 줍니다.

```
/farms
{
   /secure
   { 
      /virtualhosts
      {
         # select this farm for all incoming HTTPS requestss
         "https://*"
      }
      /renders
      {
      /0001
         {
            # hostname or IP of the render
            /hostname "localhost"
            # port of the render
            /port "8443"
            # connect via HTTPS
            /secure "1"
         }
      }
     # the rest of the properties are omitted
   }

   /non-secure
   { 
      /virtualhosts
      {
         # select this farm for all incoming HTTP requests
         "https://*"
      }
      /renders
      {
         /0001
      {
         # hostname or IP of the render
         /hostname "localhost"
         # port of the render
         /port "4503"
      }
   }
    # the rest of the properties are omitted
}
```

## Dispatcher와 AEM {#configuring-mutual-ssl-between-dispatcher-and-aem} 간 상호 SSL 구성

Mutual SSL을 사용하도록 Dispatcher와 렌더링 컴퓨터(일반적으로 AEM 또는 CQ 게시 인스턴스) 간의 연결을 구성합니다.

* 디스패처는 SSL을 통해 렌더링 인스턴스에 연결합니다.
* 렌더링 인스턴스는 Dispatcher 인증서의 유효성을 확인합니다.
* Dispatcher는 렌더링 인스턴스의 인증서를 신뢰할 수 있는지 확인합니다.
* (선택 사항) Dispatcher는 렌더링 인스턴스의 인증서가 렌더링 인스턴스의 서버 주소와 일치하는지 확인합니다.

상호 SSL을 구성하려면 신뢰할 수 있는 인증 기관(CA)에서 서명한 인증서가 필요합니다. 자체 서명된 인증서는 적합하지 않습니다. 인증 기관(CA) 또는 제3자 인증 기관(CA)의 서비스를 사용하여 인증서를 서명할 수 있습니다. 상호 SSL을 구성하려면 다음 항목이 필요합니다.

* 렌더링 인스턴스 및 발송자에 대해 서명된 인증서
* CA 인증서(인증 기관 역할을 하는 경우)
* CA, 인증서 및 인증서 요청을 생성하기 위한 OpenSSL 라이브러리

상호 SSL을 구성하려면 다음 단계를 수행하십시오.

1. [](dispatcher-install.md) 플랫폼용 최신 버전의 Dispatcher를 설치합니다. SSL을 지원하는 Dispatcher 바이너리를 사용합니다(SSL은 dispatcher-apache2.4-linux-x86-64-ssl10-4.1.7.tar 등 파일 이름에 있음).
1. [Dispatcher 및 렌더링 인스턴스용 CA 서명 ](dispatcher-ssl.md#main-pars-title-3) 인증서를 만들거나 가져옵니다.
1. [렌더링 인증서가 포함된 키 ](dispatcher-ssl.md#main-pars-title-6) 저장소를 만들고 렌더의 HTTP 서비스를 구성하여 사용합니다.
1. [상호 SSL을 위해 Dispatcher 웹 서버 ](dispatcher-ssl.md#main-pars-title-4) 모듈을 구성합니다.

### CA 서명 인증서 만들기 또는 얻기 {#creating-or-obtaining-ca-signed-certificates}

게시 인스턴스 및 디스패처를 인증하는 CA 서명 인증서를 만들거나 가져옵니다.

#### CA {#creating-your-ca} 만들기

CA 역할을 하는 경우 [OpenSSL](https://www.openssl.org/)을 사용하여 서버 및 클라이언트 인증서를 서명하는 인증 기관을 만듭니다. OpenSSL 라이브러리가 설치되어 있어야 합니다. 타사 CA를 사용하는 경우 이 절차를 수행하지 마십시오.

1. 터미널을 열고 현재 디렉토리를 `/usr/local/ssl/misc` 같은 CA.sh 파일을 포함하는 디렉토리로 변경합니다.
1. CA를 만들려면 다음 명령을 입력한 후 문제가 발생할 때 값을 입력합니다.

   ```shell
   ./CA.sh -newca
   ```

   >[!NOTE]
   >
   >openssl.cnf 파일의 여러 속성은 CA.sh 스크립트의 비헤이비어를 제어합니다. CA를 만들기 전에 이 파일을 필요에 따라 수정해야 합니다.

#### 인증서 {#creating-the-certificates} 만들기

OpenSSL을 사용하여 제3자 CA로 보내거나 CA로 서명할 인증서 요청을 만듭니다.

인증서를 만들 때 OpenSSL은 [일반 이름] 속성을 사용하여 인증서 보유자를 식별합니다. 렌더링 인스턴스의 인증서에 대해, Dispatcher가 게시 인스턴스의 호스트 이름과 일치하는 경우에만 인증서를 수락하도록 구성하는 경우 인스턴스 컴퓨터의 호스트 이름을 일반 이름으로 사용합니다. ([DispatcherCheckPeerCN](dispatcher-ssl.md#main-pars-title-11) 속성을 참조하십시오.)

1. 터미널을 열고 현재 디렉토리를 OpenSSL 라이브러리의 CH.sh 파일이 포함된 디렉토리로 변경합니다.
1. 다음 명령을 입력하고 메시지가 표시되면 값을 입력합니다. 필요한 경우 게시 인스턴스의 호스트 이름을 공통 이름으로 사용합니다. 호스트 이름은 렌더링의 IP 주소에 대한 DNS 확인 가능 이름입니다.

   ```shell
   ./CA.sh -newreq
   ```

   제3자 CA를 사용 중인 경우, newreq.pem 파일을 CA로 보내 서명하십시오. 인증 기관(CA) 역할을 하는 경우 3단계를 계속 진행합니다.

1. CA 인증서를 사용하여 인증서를 서명하려면 다음 명령을 입력합니다.

   ```shell
   ./CA.sh -sign
   ```

   CA 관리 파일이 포함된 디렉토리에 newcert.pem 및 newkey.pem이라는 두 파일이 만들어집니다. 렌더링 컴퓨터의 공용 인증서와 개인 키입니다.

1. newcert.pem의 이름을 rendercert.pem으로 변경하고 newkey.pem의 이름을 renderkey.pem으로 바꿉니다.
1. 2단계와 3단계를 반복하여 Dispatcher 모듈에 대한 새 인증서와 새 공개 키를 만듭니다. Dispatcher 인스턴스와 관련된 공통 이름을 사용해야 합니다.
1. newcert.pem의 이름을 discert.pem으로 변경하고 newkey.pem의 이름을 discover.pem으로 바꿉니다.

### 렌더링 컴퓨터에서 SSL 구성 {#configuring-ssl-on-the-render-computer}

rendercert.pem 및 renderkey.pem 파일을 사용하여 렌더링 인스턴스에서 SSL을 구성합니다.

#### 렌더링 인증서를 JKS 형식 {#converting-the-render-certificate-to-jks-format}으로 변환

다음 명령을 사용하여 PEM 파일인 렌더링 인증서를 PKCS#12 파일로 변환합니다. 렌더링 인증서에 서명한 CA의 인증서도 포함합니다.

1. 터미널 창에서 현재 디렉토리를 렌더링 인증서 및 개인 키 위치로 변경합니다.
1. PEM 파일인 렌더링 인증서를 PKCS#12 파일로 변환하려면 다음 명령을 입력합니다. 렌더링 인증서에 서명한 CA의 인증서도 포함합니다.

   ```shell
   openssl pkcs12 -export -in rendercert.pem -inkey renderkey.pem  -certfile demoCA/cacert.pem -out rendercert.p12
   ```

1. PKCS#12 파일을 JKS(Java KeyStore) 형식으로 변환하려면 다음 명령을 입력합니다.

   ```shell
   keytool -importkeystore -srckeystore servercert.p12 -srcstoretype pkcs12 -destkeystore render.keystore
   ```

1. Java 키 저장소는 기본 별칭을 사용하여 만들어집니다. 원하는 경우 별칭을 변경합니다.

   ```shell
   keytool -changealias -alias 1 -destalias jettyhttp -keystore render.keystore
   ```

#### Render의 Truststore {#adding-the-ca-cert-to-the-render-s-truststore}에 CA Cert 추가

인증 기관(CA) 역할을 하는 경우 CA 인증서를 키 저장소에 가져옵니다. 그런 다음 키 저장소를 신뢰하도록 렌더링 인스턴스를 실행하는 JVM을 구성합니다.

<!-- 

Comment Type: remark
Last Modified By: unknown unknown (sbroders@adobe.com)
Last Modified Date: 2014-08-12T13:11:21.401-0400

<p style="font-family: tahoma, arial, helvetica, sans-serif; font-size: 12px;">The jetty http service has properties to specify trusted CA certificates for mutual SSL for 6.0. Whether they are operable is undetetermined. See https://issues.adobe.com/browse/DOC-4738.</p> 
<p style="font-family: tahoma, arial, helvetica, sans-serif; font-size: 12px;"> </p> 
<p style="font-family: tahoma, arial, helvetica, sans-serif; font-size: 12px;">For 5.6.1, you would specify the system property javax.net.ssl.trustStore, using the path to cacerts as value.</p>

 -->

1. 텍스트 편집기를 사용하여 cacert.pem 파일을 열고 다음 줄 앞에 오는 모든 텍스트를 제거합니다.

   `-----BEGIN CERTIFICATE-----`

1. 다음 명령을 사용하여 인증서를 키 저장소에 가져옵니다.

   ```shell
   keytool -import -keystore cacerts.keystore -alias myca -storepass password -file cacert.pem
   ```

1. 렌더링 인스턴스를 실행하는 JVM을 구성하여 키 저장소를 신뢰하려면 다음 시스템 속성을 사용합니다.

   ```shell
   -Djavax.net.ssl.trustStore=<location of cacerts.keystore>
   ```

   예를 들어 crx-quickstart/bin/quickstart 스크립트를 사용하여 게시 인스턴스를 시작하는 경우 CQ_JVM_OPTS 속성을 수정할 수 있습니다.

   ```shell
   CQ_JVM_OPTS='-server -Xmx2048m -XX:MaxPermSize=512M -Djavax.net.ssl.trustStore=/usr/lib/cq6.0/publish/ssl/cacerts.keystore'
   ```

#### 렌더링 인스턴스 구성 {#configuring-the-render-instance}

렌더링 인증서를 사용하여 렌더링 인스턴스의 HTTP 서비스가 SSL을 사용하도록 구성하려면 게시 인스턴스&#x200B;*섹션의* SSL 활성화 섹션의 지침을 따릅니다.

* AEM 6.2:[SSL을 통해 HTTP 활성화](https://helpx.adobe.com/experience-manager/6-2/sites/deploying/using/config-ssl.html)
* AEM 6.1:[SSL을 통해 HTTP 활성화](https://docs.adobe.com/content/docs/en/aem/6-1/deploy/configuring/config-ssl.html)
* 이전 AEM 버전:[이 페이지를 참조하십시오.](https://helpx.adobe.com/experience-manager/aem-previous-versions.html)

### 발송자 모듈 {#configuring-ssl-for-the-dispatcher-module}에 대한 SSL 구성

Dispatcher가 상호 SSL을 사용하도록 구성하려면 Dispatcher 인증서를 준비한 다음 웹 서버 모듈을 구성합니다.

### 통합 발송자 인증서 만들기 {#creating-a-unified-dispatcher-certificate}

발송자 인증서와 암호화되지 않은 개인 키를 하나의 PEM 파일에 결합합니다. 텍스트 편집기 또는 `cat` 명령을 사용하여 다음 예와 유사한 파일을 만듭니다.

1. 터미널을 열고 현재 디렉토리를 display.pem 파일의 위치로 변경합니다.
1. 개인 키를 해독하려면 다음 명령을 입력합니다.

   ```shell
   openssl rsa -in dispkey.pem -out dispkey_unencrypted.pem
   ```

1. 텍스트 편집기 또는 `cat` 명령을 사용하여 암호화되지 않은 개인 키와 인증서를 다음 예제와 비슷한 단일 파일에 결합합니다.

   ```xml
   -----BEGIN RSA PRIVATE KEY-----
   MIICxjBABgkqhkiG9w0B...
   ...M2HWhDn5ywJsX
   -----END RSA PRIVATE KEY-----
   -----BEGIN CERTIFICATE-----
   MIIC3TCCAk...
   ...roZAs=
   -----END CERTIFICATE-----
   ```

### 발송자에 사용할 인증서 지정 {#specifying-the-certificate-to-use-for-dispatcher}

[디스패처 모듈 구성](dispatcher-install.md#main-pars-55-35-1022)(`httpd.conf`에서)에 다음 속성을 추가합니다.

* `DispatcherCertificateFile`:공개 인증서와 암호화되지 않은 개인 키가 포함된 Dispatcher 통합 인증서 파일의 경로입니다. 이 파일은 SSL 서버가 Dispatcher 클라이언트 인증서를 요청할 때 사용됩니다.
* `DispatcherCACertificateFile`:SSL 서버가 루트 인증 기관에서 신뢰하지 않는 CA를 나타낼 때 사용되는 CA 인증서 파일의 경로입니다.
* `DispatcherCheckPeerCN`:원격 서버 인증서에 대한 호스트 이름 확인을 활성화(  `On`) 또는 비활성화(  `Off`) 할지 여부.

다음 코드는 구성의 예입니다.

```xml
<IfModule disp_apache2.c>
  DispatcherConfig conf/dispatcher.any
  DispatcherLog    logs/dispatcher.log
  DispatcherLogLevel 3
  DispatcherNoServerHeader 0
  DispatcherDeclineRoot 0
  DispatcherUseProcessedURL 0
  DispatcherPassError 0
  DispatcherCertificateFile disp_unified.pem
  DispatcherCACertificateFile cacert.pem
  DispatcherCheckPeerCN On
</IfModule>
```

