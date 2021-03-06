---
title: 包含檔案
description: 包含檔案
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.topic: include
ms.date: 09/10/2020
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 320fa542f2b786f0a256c22f2d2eb299c476dcae
ms.sourcegitcommit: 0b9fe9e23dfebf60faa9b451498951b970758103
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/07/2020
ms.locfileid: "94362745"
---
下表提供 [Azure 事件中樞](https://azure.microsoft.com/services/event-hubs/)的特定配額與限制。 如需事件中樞價格的相關資訊，請參閱[事件中樞價格](https://azure.microsoft.com/pricing/details/event-hubs/)。

## <a name="common-limits-for-all-tiers"></a>所有階層的通用限制
下列限制在所有階層之間是共通的。 

| 限制 |  注意 | 值 |
| --- |  --- | --- |
| 每一訂用帳戶的「事件中樞」命名空間數目 |- |100 |
| 每個命名空間的事件中樞數目 | 建立新的事件中樞的後續要求將遭到拒絕。 |10 |
| 事件中樞內的資料分割數目 |- |32 |
| 事件中樞名稱的大小 |- | 256 個字元 |
| 取用者群組名稱的大小 |- | 256 個字元 |
| 每個取用者群組的非 epoch 接收者數目 |- |5 |
| 每個命名空間的授權規則數目 | 建立授權規則的後續要求將遭到拒絕。|12 |
| 對 GetRuntimeInformation 方法的呼叫次數 |  - | 每秒 50 個 | 
| 虛擬網路 (VNet)和 IP 設定規則的數目 | - | 128 | 


## <a name="basic-vs-standard-tiers"></a>基本層和標準層
下表顯示基本和標準層可能有所不同的限制。 

| 限制 | 注意 | 基本 | 標準 |
|---|---|--|---|
| 事件中樞事件的大小上限| &nbsp; | 256 KB | 1 MB |
| 每一個事件中樞取用者群組數目 | &nbsp; |1 |20 |
| 每個命名空間的 AMQP 連線數目 | 後續對更多連線的要求將會遭到拒絕，而且呼叫端程式碼將會收到例外狀況。 |100 |5,000|
| 事件資料的最大保留期間 | &nbsp; |1 日 |1-7 天 |
| 最大輸送量單位 |超過此限制會導致您的資料受到節流，並產生[伺服器忙碌例外狀況](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception)。 若要為標準層要求大量的輸送量單位，請提出[支援要求](/azure/azure-portal/supportability/how-to-create-azure-support-request)。 [更多輸送量單位](../articles/event-hubs/event-hubs-auto-inflate.md)依承諾購買方式，以 20 個為一組來取得。 |20 | 20 | 

## <a name="dedicated-tier-vs-standard-tier"></a>專用層和標準層
事件中樞專用供應項目以每月固定價格計費，最低為 4 小時的使用量。 專用層提供標準方案的所有功能，但可對工作負載需求高的客戶提供企業規模容量和限制。 

請參閱此[文件](https://docs.microsoft.com/azure/event-hubs/event-hubs-dedicated-cluster-create-portal)，以了解如何使用 Azure 入口網站建立專用的事件中樞叢集。

| 功能 | 標準 | 專用 |
| --- |:---|:---|
| 頻寬 | 20 TU (最多 40 TU) | 20 個 CU |
| 命名空間 |  1 | 每個 CU 50 個 |
| 事件中樞 |  每一命名空間 10 個 | 每一命名空間 1000 個 |
| 輸入事件 | 按百萬個事件付費 | 已包括 |
| 訊息大小 | 100 萬個位元組 | 100 萬個位元組 |
| 資料分割 | 每個事件中樞 32 個 | 每個事件中樞 1024 個<br/>每個 CU 2000 個 |
| 用戶群組 | 每個事件中樞 20 個 | 沒有每個 CU 的限制，每個事件中樞 1000 個 |
| 代理連線 | 含 1000 個，上限為 5000 個 | 含 10 萬個和上限 |
| 訊息保留期 | 7 天，每個 CU 包含 84 GB | 90 天，每個 CU 包含 10 TB |
| 擷取 | 按小時付費 | 已包括 |


## <a name="schema-registry-limitations"></a>結構描述登錄限制

### <a name="limits-that-are-the-same-for-standard-and-dedicated-tiers"></a>**標準** 和 **專用** 層的限制相同 
| 功能 | 限制 | 
|---|---|--|
| 結構描述群組名稱的長度上限 | 50 |  
| 結構描述名稱的長度上限 | 100 |    
| 每個結構描述的大小 (位元組) | 1 MB |   
| 每個結構描述群組的屬性數目 | 1024 |
| 每個群組屬性金鑰的大小 (位元組) | 256 | 
| 每個群組屬性值的大小 (位元組) | 1024 | 


### <a name="limits-that-are-different-for-standard-and-dedicated-tiers"></a>**標準** 和 **專用** 層的限制不同 

| 限制 | 標準 | 專用 | 
|---|---|--|---|
| 結構描述登錄 (命名空間) 的大小 (MB) | 25 |  1024 |
| 結構描述登錄或命名空間中的結構描述群組數目 | 1 - 排除預設群組 | 1000 |
| 跨所有結構描述群組的架構版本數目 | 25 | 10000 |





