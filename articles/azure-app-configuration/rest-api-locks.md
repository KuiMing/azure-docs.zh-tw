---
title: Azure 應用程式組態 REST API-鎖定
description: 使用 Azure 應用程式組態 REST API 處理索引鍵/值鎖定的參考頁面
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 4949db646c54d75f60d29d3c631d0f4ee8d7c26e
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/06/2020
ms.locfileid: "93424089"
---
# <a name="locks"></a>鎖定

api 版本：1。0

此 API 會提供索引鍵/值資源的鎖定/解除鎖定語義。 它支援下列作業：

- 鎖定位置
- 移除鎖定

如果存在，則 `label` 必須是明確的標籤值， ( **不** 是萬用字元) 。 針對所有作業，它是選擇性參數。 如果省略，則表示沒有標籤。

## <a name="prerequisites"></a>必要條件

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-rest-api-prereqs.md)]

## <a name="lock-key-value"></a>鎖定 Key-Value

- **必要：** ``{key}`` 、 ``{api-version}``  
- *選用：*``label``

```http
PUT /locks/{key}?label={label}&api-version={api-version} HTTP/1.1
```

**反應：**

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.microsoft.appconfig.kv+json; charset=utf-8"
```

```json
{
  "etag": "4f6dd610dd5e4deebc7fbaef685fb903",
  "key": "{key}",
  "label": "{label}",
  "content_type": null,
  "value": "example value",
  "created": "2017-12-05T02:41:26.4874615+00:00",
  "locked": true,
  "tags": []
}
```

如果索引鍵/值不存在，則會傳回下列回應：

```http
HTTP/1.1 404 Not Found
```

## <a name="unlock-key-value"></a>解除鎖定 Key-Value

- **必要：** ``{key}`` 、 ``{api-version}``  
- *選用：*``label``

```http
DELETE /locks/{key}?label={label}?api-version={api-version} HTTP/1.1
```

**反應：**

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.microsoft.appconfig.kv+json; charset=utf-8"
```

```json
{
  "etag": "4f6dd610dd5e4deebc7fbaef685fb903",
  "key": "{key}",
  "label": "{label}",
  "content_type": null,
  "value": "example value",
  "created": "2017-12-05T02:41:26.4874615+00:00",
  "locked": true,
  "tags": []
}
```

如果索引鍵/值不存在，則會傳回下列回應：

```http
HTTP/1.1 404 Not Found
```

## <a name="conditional-lockunlock"></a>條件式鎖定/解除鎖定

若要防止競爭條件，請使用 `If-Match` 或 `If-None-Match` 要求標頭。 `etag`引數是索引鍵標記法的一部分。 如果 `If-Match` `If-None-Match` 省略或，則作業將會無條件進行。

只有當目前的索引鍵/值表示符合指定的時，下列要求才會套用作業 `etag` ：

```http
PUT|DELETE /locks/{key}?label={label}&api-version={api-version} HTTP/1.1
If-Match: "4f6dd610dd5e4deebc7fbaef685fb903"
```

只有當目前的索引鍵/值表示存在但不符合指定的時，下列要求才會套用作業 `etag` ：

```http
PUT|DELETE /kv/{key}?label={label}&api-version={api-version} HTTP/1.1
If-None-Match: "4f6dd610dd5e4deebc7fbaef685fb903"
```
