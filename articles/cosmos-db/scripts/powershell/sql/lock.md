---
title: 使用 PowerShell 指令碼建立 Azure Cosmos SQL API 資料庫與容器的資源鎖定
description: 為 Azure Cosmos SQL API 資料庫與容器建立資源鎖定
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 06/12/2020
ms.openlocfilehash: 853780b7c2be3bd574f7c308ef55f7f8f9fc2eec
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93074415"
---
# <a name="create-a-resource-lock-for-azure-cosmos-sql-api-database-and-container-using-azure-powershell"></a>使用 Azure PowerShell 建立 Azure Cosmos SQL API 資料庫與容器的資源鎖定
[!INCLUDE[appliesto-sql-api](../../../includes/appliesto-sql-api.md)]

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

> [!IMPORTANT]
> 除非先啟用 `disableKeyBasedMetadataWriteAccess` 屬性鎖定 Cosmos DB 帳戶，否則資源鎖定對使用任何 Cosmos DB SDK、任何透過帳戶金鑰連線的工具，或是 Azure 入口網站進行連線之使用者所做的變更無效。 若要深入了解如何啟用此屬性，請參閱[防止來自 SDK 的變更](../../../role-based-access-control.md#prevent-sdk-changes) (機器翻譯)。

## <a name="sample-script"></a>範例指令碼

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/sql/ps-sql-lock.ps1 "Create, list, and remove resource locks")]

## <a name="clean-up-deployment"></a>清除部署

在執行過指令碼範例之後，您可以使用下列命令來移除資源群組和所有與其相關聯的資源。

```powershell
Remove-AzResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>指令碼說明

此指令碼會使用下列命令。 下表中的每個命令都會連結至命令特定的文件。

| Command | 注意 |
|---|---|
|**Azure 資源**| |
| [New-AzResourceLock](/powershell/module/az.resources/new-azresourcelock) | 建立資源鎖定。 |
| [Get-AzResourceLock](/powershell/module/az.resources/get-azresourcelock) | 取得資源鎖定，或列出資源鎖定。 |
| [Remove-AzResourceLock](/powershell/module/az.resources/remove-azresourcelock) | 移除資源鎖定。 |
|||

## <a name="next-steps"></a>後續步驟

如需有關 Azure PowerShell 的詳細資訊，請參閱 [Azure PowerShell 文件](/powershell/)。