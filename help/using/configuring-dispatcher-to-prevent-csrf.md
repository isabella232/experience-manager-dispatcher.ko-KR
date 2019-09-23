---
title: CSRF 공격을 방지하기 위해 디스패처 구성
seo-title: CSRF 공격을 방지하기 위해 Adobe AEM Dispatcher 구성
description: 교차 사이트 요청 위조 공격을 방지하기 위해 AEM Dispatcher를 구성하는 방법을 알아봅니다.
seo-description: 교차 사이트 요청 위조 공격을 방지하기 위해 Adobe AEM Dispatcher를 구성하는 방법을 알아봅니다.
uuid: f290bdeb-54e2-4649-b0fc-6257b422af2d
topic-tags: dispatcher
content-type: 참조
discoiquuid: d61d021e-b338-4a1d-91ee-55427557e931
translation-type: tm+mt
source-git-commit: 69edbe7608b46c93d238515e4223606eadad0ac4

---


# CSRF 공격을 방지하기 위해 디스패처 구성{#configuring-dispatcher-to-prevent-csrf-attacks}

AEM 파섹 이 프레임워크를 제대로 사용하려면 디스패처 구성을 다음과 같이 변경해야 합니다.

>[!NOTE]
>
>기존 구성에 따라 아래 예에서 규칙 번호를 업데이트해야 합니다. 디스패처는 마지막 일치 규칙을 사용하여 허용 또는 거부를 허용하므로 규칙을 기존 목록의 맨 아래에 배치합니다.

1. author-farm.any 및 publish-farm.any의 `/clientheaders` 섹션에서 다음 항목을 목록 하단에 추가합니다.\
   `CSRF-Token`
1. 사용자 `author-farm.any` 및 `publish-farm.any` 또는 `publish-filters.any` 파일의 /filters 섹션에서 다음 줄을 추가하여 디스패처를 `/libs/granite/csrf/token.json` 통한 요청을 허용합니다.\
   `/0999 { /type "allow" /glob " * /libs/granite/csrf/token.json*" }`
1. 사용자 `/cache /rules` 섹션 `publish-farm.any`아래에 규칙을 추가하여 디스패처가 `token.json` 파일을 캐싱하지 못하도록 합니다. 작성자는 일반적으로 캐싱을 무시하므로 규칙을 추가할 필요가 없습니다 `author-farm.any`.\
   `/0999 { /glob "/libs/granite/csrf/token.json" /type "deny" }`

구성이 제대로 작동하는지 확인하려면 DEBUG 모드에서 dispatcher.log를 통해 token.json 파일이 캐시되고 있지 않고 필터에 의해 차단되지 않고 있는지 확인합니다. 다음과 유사한 메시지가 표시됩니다.\
`... checking [/libs/granite/csrf/token.json]  `
`... request URL not in cache rules: /libs/granite/csrf/token.json`\
`... cache-action for [/libs/granite/csrf/token.json]: NONE`

Apache에서 요청이 성공하는지 확인할 수도 `access_log`있습니다. "/libs/granite/csrf/token.json에 대한 요청은 HTTP 200 상태 코드를 반환해야 합니다.
