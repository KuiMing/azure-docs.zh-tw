---
title: 外框呈現
description: 說明如何執行選取範圍外框呈現
author: florianborn71
ms.author: flborn
ms.date: 02/11/2020
ms.topic: article
ms.custom: devx-track-csharp
ms.openlocfilehash: 55151ab965b13a944fee5517a2ca8b4f9eb64bb0
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/19/2020
ms.locfileid: "92207235"
---
# <a name="outline-rendering"></a>外框呈現

選取的物件可以透過[階層式狀態覆寫元件](../../overview/features/override-hierarchical-state.md)來新增外框呈現，以視覺方式醒目提示。 本章說明如何透過用戶端 API 變更外框呈現的全域參數。

外框屬性是全域設定。 使用外框呈現的所有物件都會使用相同的設定 - 不可能使用每個物件的外框色彩。

## <a name="parameters-for-outlinesettings"></a>`OutlineSettings` 的參數

`OutlineSettings` 類別保留與全域外框屬性相關的設定。 會公開下列成員：

| 參數      | 類型    | 描述                                             |
|----------------|---------|---------------------------------------------------------|
| `Color`          | Color4Ub | 用於繪製外框的色彩。 忽略 Alpha 部分。         |
| `PulseRateHz`    | FLOAT   | 每秒外框擺動的速率|
| `PulseIntensity` | FLOAT   | 外框脈衝效果的強度。 必須介於 0.0 (無脈衝) 與 1.0 (完整脈衝) 之間。 強度會隱含地將外框的最小不透明度設定為 `MinOpacity = 1.0 - PulseIntensity`。 |

![使用不同大綱參數轉譯三次的物件會造成將 ](./media/outlines.png) `color` 參數從黃色 (左) 變更為洋紅 (中心) ，以及 `pulseIntensity` 從0到 0.8 (右) 的效果。

## <a name="example"></a>範例

下列程式碼示範透過 API 設定外框參數的範例：

```cs
void SetOutlineParameters(AzureSession session)
{
    OutlineSettings outlineSettings = session.Actions.OutlineSettings;
    outlineSettings.Color = new Color4Ub(255, 255, 0, 255);
    outlineSettings.PulseRateHz = 2.0f;
    outlineSettings.PulseIntensity = 0.5f;
}
```

```cpp
void SetOutlineParameters(ApiHandle<AzureSession> session)
{
    ApiHandle<OutlineSettings> outlineSettings = session->Actions()->GetOutlineSettings();
    Color4Ub outlineColor;
    outlineColor.channels = { 255, 255, 0, 255 };
    outlineSettings->SetColor(outlineColor);
    outlineSettings->SetPulseRateHz(2.0f);
    outlineSettings->SetPulseIntensity(0.5f);
}
```

## <a name="performance"></a>效能

外框呈現可能會對呈現效能造成重大影響。 這個影響會因指定框架選取和非選取物件之間的螢幕空間空間關聯而異。

## <a name="api-documentation"></a>API 文件

* [C # RemoteManager. OutlineSettings 屬性](/dotnet/api/microsoft.azure.remoterendering.remotemanager.outlinesettings)
* [C + + RemoteManager：： OutlineSettings ( # B1 ](/cpp/api/remote-rendering/remotemanager#outlinesettings)

## <a name="next-steps"></a>後續步驟

* [階層式狀態覆寫元件](../../overview/features/override-hierarchical-state.md)