---
title: 使用媒體服務中的標準編碼器為影片編碼 - Azure | Microsoft Docs
description: 本主題說明如何使用媒體服務的標準編碼器，根據輸入解析度和位元速率，以自動產生的位元速率階梯，進行視訊的編碼。
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/31/2020
ms.author: inhenkel
ms.custom: seodec18
ms.openlocfilehash: 05accd69f1868b8b0e0f6dbd4fb5c21ee843ec5e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "89297709"
---
#  <a name="encode-with-an-auto-generated-bitrate-ladder"></a>使用自動產生的位元速率階梯進行編碼

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

## <a name="overview"></a>概觀

本文說明如何使用媒體服務的標準編碼器並根據輸入解析度和位元速率，以自動產生的位元速率階梯，進行視訊的編碼。 這種內建的編碼器設定或預設，永遠不能超過輸入解析度和位元速率。 例如，如果輸入是 720p 3 Mbps，則輸出會維持在最多 720p，且速率啟動低於 3 Mbps。

### <a name="encoding-for-streaming"></a>只針對串流處理進行編碼

當您在 [轉換]**** 中使用預設 [AdaptiveStreaming]**** 時，您所得到的輸出很適合透過 HLS 和 DASH 等串流通訊協定進行傳遞。 當使用這項預設時，服務會自行決定應該產生多少層的視訊，以及應當採用的位元速率和解析度。 輸出內容包含 MP4 檔案，其中的 AAC 編碼音訊和 H.264 編碼視訊為非交錯格式。

如需此預設的應用範例，請參閱[串流處理檔案](stream-files-dotnet-quickstart.md)。

## <a name="output"></a>輸出

本節顯示三個由媒體服務編碼器產生的輸出視訊層範例，它們進行編碼時都是利用 **AdaptiveStreaming** 預設。 在所有情況下，輸出包含純音訊 MP4 檔案，以 128 kbps 編碼的立體聲。

### <a name="example-1"></a>範例 1
高度 "1080" 和畫面播放速率 "29.970" 的來源會產生 6 個視訊層︰

|層|高度|寬度|位元速率 (kbps)|
|---|---|---|---|
|1|1080|1920|6780|
|2|720|1280|3520|
|3|540|960|2210|
|4|360|640|1150|
|5|270|480|720|
|6|180|320|380|

### <a name="example-2"></a>範例 2
高度 "720" 和畫面播放速率 "23.970" 的來源會產生 5 個視訊層︰

|層|高度|寬度|位元速率 (kbps)|
|---|---|---|---|
|1|720|1280|2940|
|2|540|960|1850|
|3|360|640|960|
|4|270|480|600|
|5|180|320|320|

### <a name="example-3"></a>範例 3
高度 "360" 和畫面播放速率 "29.970" 的來源會產生 3 個視訊層︰

|層|高度|寬度|位元速率 (kbps)|
|---|---|---|---|
|1|360|640|700|
|2|270|480|440|
|3|180|320|230|

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [串流處理檔案](stream-files-dotnet-quickstart.md)
