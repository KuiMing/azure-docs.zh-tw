---
title: Azure Data Factory 中的連結服務
description: 了解 Data Factory 中的連結服務。 已連結的服務會將計算/資料存放區連結至資料處理站。
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: anandsub
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 08/21/2020
ms.openlocfilehash: 3d49422af01e38884b5d8ff871fbe84254938944
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "89434107"
---
# <a name="linked-services-in-azure-data-factory"></a>Azure Data Factory 中的連結服務

> [!div class="op_single_selector" title1="選取您目前使用的 Data Factory 服務版本："]
> * [第 1 版](v1/data-factory-create-datasets.md)
> * [目前的版本](concepts-linked-services.md)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

本文說明什麼是連結的服務、如何以 JSON 格式定義它們，以及如何在 Azure Data Factory 管線中使用它們。

如果您不熟悉 Data Factory，請參閱 [Azure Data Factory 簡介](introduction.md)來概略了解。

## <a name="overview"></a>概觀

資料處理站可以有一或多個管線。 「管線」是一起執行某個工作的「活動」所組成的邏輯群組。 管線中的活動會定義要在資料上執行的動作。 例如，您可以使用複製活動，將資料從 SQL Server 複製到 Azure Blob 儲存體。 接著，您可以使用在 Azure HDInsight 叢集上執行 Hive 指令碼的 Hive 活動，來處理來自 Blob 儲存體的資料以產生輸出資料。 最後，您可能會使用第二個複製活動，將輸出資料複製到先前的 SQL 資料倉儲 Azure Synapse Analytics () ，並在其中建立商務智慧 (BI) 報表方案。 如需有關管線和活動的詳細資訊，請參閱 Azure Data Factory 中的[管線和活動](concepts-pipelines-activities.md)。

現在，「資料集」是一個具名的資料檢視，指向或參考您想要在「活動」中用來作為輸入或輸出的資料。

在您建立資料集之前，您必須建立一個「已連結的服務」，以將資料存放區連結到資料處理站。 已連結的服務非常類似連接字串，可定義 Data Factory 連接到外部資源所需的連線資訊。 這麼說吧：資料集代表已連結之資料存放區內的資料結構，而已連結的服務則定義與資料來源的連線。 例如，「Azure 儲存體」已連結服務會將儲存體帳戶連結到 Data Factory。 Azure Blob 資料集代表該 Azure 儲存體帳戶內包含要處理之輸入 Blob 的 Blob 容器和資料夾。

以下是一個範例案例。 若要將資料從 Blob 儲存體複製到 SQL Database，您要建立兩個連結服務：類型為 Azure 儲存體和 Azure SQL Database。 接著，建立兩個資料集：Azure Blob 資料集 (此資料集參考 Azure 儲存體連結服務) 和 Azure SQL 資料表資料集 (此資料集參考 Azure SQL Database 連結服務)。 「Azure 儲存體」和 Azure SQL Database 已連結服務包含 Data Factory 在執行階段分別用來連接到「Azure 儲存體」和 Azure SQL Database 的連接字串。 Azure Blob 資料集會指定包含 Blob 儲存體中輸入 Blob 的 Blob 容器和 Blob 資料夾。 「Azure SQL 資料表」資料集會指定做為資料複製目的地的 SQL Database 中 SQL 資料表。

下圖顯示 Data Factory 中管線、活動、資料集及已連結服務之間的關聯性：

![管線、活動、資料集、已連結的服務之間的關聯性](media/concepts-datasets-linked-services/relationship-between-data-factory-entities.png)

## <a name="linked-service-json"></a>連結服務 JSON

Data Factory 中的連結服務會以 JSON 格式定義如下：

```json
{
    "name": "<Name of the linked service>",
    "properties": {
        "type": "<Type of the linked service>",
        "typeProperties": {
              "<data store or compute-specific type properties>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

下表描述上述 JSON 的屬性：

屬性 | 描述 | 必要 |
-------- | ----------- | -------- |
NAME | 連結服務的名稱。 請參閱 [Azure Data Factory - 命名規則](naming-rules.md)。 |  是 |
type | 連結服務的類型。 例如： AzureBlobStorage (資料存放區) 或 AzureBatch (計算) 。 請參閱 typeProperties 的描述。 | 是 |
typeProperties | 每個資料存放區和計算的類型屬性都不同。 <br/><br/> 如需支援的資料存放區類型及其類型屬性，請參閱 [連接器總覽](copy-activity-overview.md#supported-data-stores-and-formats) 文章。 請瀏覽資料存放區連接器的文章，以了解資料存放區特有的類型屬性。 <br/><br/> 如需支援的計算類型與其類型屬性，請參閱[計算連結服務](compute-linked-services.md)。 | 是 |
connectVia | 用來連線到資料存放區的 [Integration Runtime](concepts-integration-runtime.md)。 您可以使用 Azure Integration Runtime 或自我裝載整合執行階段 (如果您的資料存放區位於私人網路中)。 如果未指定，就會使用預設的 Azure Integration Runtime。 | 否

## <a name="linked-service-example"></a>已連結的服務範例

下列連結服務是 Azure Blob 儲存體連結服務。 請注意，此類型會設定為 Azure Blob 儲存體。 Azure Blob 儲存體連結服務的類型屬性包含連接字串。 Data Factory 服務會在執行階段使用連接字串來連線至資料存放區。

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="create-linked-services"></a>建立連結的服務

您可以透過 [管理中樞](author-management-hub.md) 以及參考這些服務的任何活動、資料集或資料流程，在 Azure Data Factory UX 中建立連結的服務。

您可以使用下列其中一個工具或 Sdk 來建立連結服務： [.NET API](quickstart-create-data-factory-dot-net.md)、 [PowerShell](quickstart-create-data-factory-powershell.md)、 [REST API](quickstart-create-data-factory-rest-api.md)、Azure Resource Manager 範本和 Azure 入口網站。


## <a name="data-store-linked-services"></a>資料存放區連結服務

您可以從[連接器總覽](copy-activity-overview.md#supported-data-stores-and-formats)文章中，找到 Data Factory 所支援的資料存放區清單。 按一下資料存放區，以了解支援的連線屬性。

## <a name="compute-linked-services"></a>計算連結服務

參考[支援的計算環境](compute-linked-services.md)，以取得您可以從資料處理站與不同設定連線的不同計算環境之相關詳細資料。

## <a name="next-steps"></a>後續步驟

如需使用上述其中一個工具或 SDK 來建立管線和資料集的逐步指示，請參閱下列教學課程。

- [快速入門：使用 .NET 來建立資料處理站](quickstart-create-data-factory-dot-net.md)
- [快速入門：使用 PowerShell 來建立資料處理站](quickstart-create-data-factory-powershell.md)
- [快速入門：使用 REST API 來建立資料處理站](quickstart-create-data-factory-rest-api.md)
- [快速入門：使用 Azure 入口網站來建立資料處理站](quickstart-create-data-factory-portal.md)
