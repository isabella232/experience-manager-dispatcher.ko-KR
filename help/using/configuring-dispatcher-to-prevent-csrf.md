---
title: CSRF 공격을 방지하기 위한 Dispatcher 구성
seo-title: CSRF 공격을 방지하도록 Adobe AEM Dispatcher 구성
description: 교차 사이트 요청 위조 공격을 방지하기 위해 AEM Dispatcher를 구성하는 방법을 알아봅니다.
seo-description: 크로스 사이트 요청 위조 공격을 방지하기 위해 Adobe AEM Dispatcher를 구성하는 방법을 알아봅니다.
uuid: F 290 BDEB -54 E 2-4649-B 0 FC -6257 B 422 AF 2 D
topic-tags: Dispatcher
content-type: 참조
discoiquuid: D 61 D 021 E-B 338-4 A 1 D -91 EE -55427557 E 931
translation-type: tm+mt
source-git-commit: 69edbe7608b46c93d238515e4223606eadad0ac4

---


# CSRF 공격을 방지하기 위한 Dispatcher 구성{#configuring-dispatcher-to-prevent-csrf-attacks}

AEM는 교차 사이트 요청 위조 공격을 방지하기 위한 프레임워크를 제공합니다. 이 프레임워크를 제대로 사용하려면 Dispatcher 구성을 다음과 같이 변경해야 합니다.

>[!NOTE]
>
>기존 구성에 따라 아래의 예에서 규칙 번호를 업데이트해야 합니다. 디스패처는 마지막 일치 규칙을 사용하여 허용 또는 거부를 부여하므로 기존 목록의 맨 아래에 규칙을 배치합니다.

1. author-farm. any 및 publish-farm. any `/clientheaders` 의 섹션에서 목록 하단에 다음 항목을 추가합니다.\
   `CSRF-Token`
1. `author-farm.any` AND `publish-farm.any` 또는 `publish-filters.any` FILE의 /filters 섹션에서 Dispatcher `/libs/granite/csrf/token.json` 를 통해 요청할 수 있도록 다음 줄을 추가합니다.\
   `/0999 { /type "allow" /glob " * /libs/granite/csrf/token.json*" }`
1. 의 `/cache /rules` 섹션에서, 디스패처가 `publish-farm.any``token.json` 파일을 캐시하지 못하게 차단하는 규칙을 추가합니다. 일반적으로 작성자는 캐시를 건너뛰므로 규칙을에 추가할 필요가 `author-farm.any`없습니다.\
   `/0999 { /glob "/libs/granite/csrf/token.json" /type "deny" }`

구성이 작동하는지 확인하려면 debug 모드에서 dispatcher. log를 확인하여 token. json 파일이 캐시되지 않고 필터가 차단되고 있는지 확인합니다. 다음과 유사한 메시지가 표시됩니다.\
`... checking [/libs/granite/csrf/token.json]  `
`... request URL not in cache rules: /libs/granite/csrf/token.json`\
`... cache-action for [/libs/granite/csrf/token.json]: NONE`

Apache `access_log`에서 요청이 성공하는지 확인할 수도 있습니다. &quot;/libs/granite/csrf/token. json &quot;에 대한 요청은 HTTP 200 상태 코드를 반환해야 합니다.
