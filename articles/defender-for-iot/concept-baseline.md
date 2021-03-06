---
title: 基準和自訂檢查
description: 瞭解適用于 IoT 的 Azure Defender 基準概念。
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/07/2019
ms.author: mlottner
ms.openlocfilehash: d97fa4c3c57f6f0dcc5c55b76d839308156c40fb
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "90934490"
---
# <a name="azure-defender-for-iot-baseline-and-custom-checks"></a>適用于 IoT 的 Azure Defender 基準和自訂檢查

本文說明適用于 IoT 基準的 Defender，並摘要說明基準自訂檢查的所有相關屬性。

## <a name="baseline"></a>基準

基準會為每個裝置建立標準行為，並可讓您更輕鬆地建立不尋常的行為或與預期的規範偏差。

## <a name="baseline-custom-checks"></a>基準自訂檢查

基準自訂檢查會使用裝置的 **模組識別** 對應項，建立每個裝置基準的自訂檢查清單。

## <a name="setting-baseline-properties"></a>設定基準屬性

1. 在 IoT 中樞內，找出並選取您想要變更的裝置。
1. 按一下裝置，然後按一下 [ **azureiotsecurity** ] 模組。
1. 按一下 [ **模組身分識別**對應項]。
1. 將 **基準自訂檢查** 檔案上傳至裝置。
1. 將基準屬性新增至安全性模組，然後按一下 [ **儲存**]。

### <a name="baseline-custom-check-file-example"></a>基準自訂檢查檔案範例

若要設定基準自訂檢查：

   ```json
    "desired": {
      "ms_iotn:urn_azureiot_Security_SecurityAgentConfiguration": {
        "baselineCustomChecksEnabled": {
          "value" : true
        },
        "baselineCustomChecksFilePath": {
          "value" : "/home/user/full_path.xml"
        },
        "baselineCustomChecksFileHash": {
          "value" : "#hashexample!"
        }
      }
    },
   ```

## <a name="baseline-custom-check-properties"></a>基準自訂檢查屬性

| 名稱| 狀態 | 有效值| 預設值| 描述 |
|----------|------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|---------------|
|baselineCustomChecksEnabled|必要： true |有效值：**布林**值 |預設值： **false** |傳送高優先順序訊息之前的最大時間間隔。|
|baselineCustomChecksFilePath |必要： true|有效的值： **字串**、 **null** |預設值： **null** |基準 xml 設定的完整路徑|
|baselineCustomChecksFileHash |必要： true|有效的值： **字串**、 **null** |預設值： **null** |`sha256sum` 的 xml 設定檔。 如需其他資訊，請使用 [sha256sum 參考](https://linux.die.net/man/1/sha256sum) 。 |

若要查看其他基準範例，請參閱 [自訂基準範例-1](https://ascforiot.blob.core.windows.net/public/custom_baseline_example_hyperv_ubuntu1804.xml) 和 [自訂基準範例-2](https://ascforiot.blob.core.windows.net/public/oms_audits.xml)。

## <a name="next-steps"></a>後續步驟

- 存取您的 [原始安全性資料](how-to-security-data-access.md)
- [調查裝置](how-to-investigate-device.md)
- 瞭解及探索 [安全性建議](concept-recommendations.md)
- 瞭解及探索 [安全性警示](concept-security-alerts.md)
