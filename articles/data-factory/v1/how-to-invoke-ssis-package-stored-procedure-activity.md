---
title: 使用 Azure Data Factory 預存程式活動叫用 SSIS 封裝
description: 本文描述如何使用預存程序活動從 Azure Data Factory 管線叫用 SQL Server Integration Services (SSIS) 封裝。
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 01/19/2018
ms.author: jingwang
ms.openlocfilehash: c7a99e7e5f27f8c3503c7fa6124d27cfc4e7f4a4
ms.sourcegitcommit: fb3c846de147cc2e3515cd8219d8c84790e3a442
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/27/2020
ms.locfileid: "92636760"
---
# <a name="invoke-an-ssis-package-using-stored-procedure-activity-in-azure-data-factory"></a>在 Azure Data Factory 使用預存程序活動叫用 SSIS 套件
本文描述如何使用預存程序活動從 Azure Data Factory 管線叫用 SSIS 封裝。 

> [!NOTE]
> 本文適用於 Data Factory 第 1 版。 如果您使用目前版本的 Data Factory 服務，請參閱[使用預存程序活動來叫用 SSIS 套件](../how-to-invoke-ssis-package-stored-procedure-activity.md)。

## <a name="prerequisites"></a>必要條件

### <a name="azure-sql-database"></a>Azure SQL Database 
本文中的逐步解說會使用 Azure SQL Database。 您也可以使用 Azure SQL 受控執行個體。

### <a name="create-an-azure-ssis-integration-runtime"></a>建立 Azure-SSIS 整合執行階段
如果您未依照[教學課程：部署 SSIS 套件](../tutorial-deploy-ssis-packages-azure.md)中的逐步指示進行，請建立 Azure SSIS 整合執行階段。 您無法使用第 1 版的 Data Factory 來建立 Azure-SSIS 整合執行階段。 

## <a name="azure-powershell"></a>Azure PowerShell
在本節中，您可以使用 Azure PowerShell 以叫用 SSIS 封裝的預存程序活動建立資料處理站管線。

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

依照[如何安裝和設定 Azure PowerShell](/powershell/azure/install-az-ps)中的指示，安裝最新的 Azure PowerShell 模組。

### <a name="create-a-data-factory"></a>建立 Data Factory
下列程序提供建立資料處理站的步驟。 您會在此資料處理站中建立具有預存程序活動的管線。 預存程序活動會執行 SSISDB 資料庫中的預存程序來執行 SSIS 套件。

1. 定義資源群組名稱的變數，以便稍後在 PowerShell 命令中使用。 將下列命令文字複製到 PowerShell，以雙引號指定 [Azure 資源群組](../../azure-resource-manager/management/overview.md)的名稱，然後執行命令。 例如： `"adfrg"` 。 
   
     ```powershell
    $resourceGroupName = "ADFTutorialResourceGroup";
    ```

    如果資源群組已經存在，您可能不想覆寫它。 將不同的值指派給 `$ResourceGroupName` 變數，然後執行一次命令
2. 若要建立 Azure 資源群組，請執行下列命令： 

    ```powershell
    $ResGrp = New-AzResourceGroup $resourceGroupName -location 'eastus'
    ``` 
    如果資源群組已經存在，您可能不想覆寫它。 將不同的值指派給 `$ResourceGroupName` 變數，然後執行一次命令。 
3. 定義 Data Factory 名稱的變數。 

    > [!IMPORTANT]
    >  將資料處理站名稱更新為全域唯一的。 

    ```powershell
    $DataFactoryName = "ADFTutorialFactory";
    ```

5. 若要建立資料處理站，請使用 $ResGrp 變數中的 Location 和 ResourceGroupName 屬性，執行下列 **>get-azdatafactory** Cmdlet： 
    
    ```powershell       
    $df = New-AzDataFactory -ResourceGroupName $ResourceGroupName -Name $dataFactoryName -Location "East US"
    ```

請注意下列幾點：

* Azure Data Factory 的名稱在全域必須是唯一的。 如果發生下列錯誤，請變更名稱，並再試一次。

    ```
    The specified Data Factory name 'ADFTutorialFactory' is already in use. Data Factory names must be globally unique.
    ```
* 若要建立 Data Factory 執行個體，您用來登入 Azure 的使用者帳戶必須為 **參與者** 或 **擁有者** 角色，或是 Azure 訂用帳戶的 **管理員** 。

### <a name="create-an-azure-sql-database-linked-service"></a>建立 Azure SQL Database 連結服務
建立連結服務，以將裝載 SSIS 目錄的 Azure SQL Database 中的資料庫連結至您的 data factory。 資料處理站使用此連結服務中的資訊連線到 SSISDB 資料庫，並執行預存程序來執行 SSIS 套件。 

1. 使用下列內容，在 **C:\ADF\RunSSISPackage** 資料夾中建立名為 **AzureSqlDatabaseLinkedService.json** 的 JSON 檔案： 

    > [!IMPORTANT]
    > 儲存檔案之前，以您的 Azure SQL Database 的值取代 &lt;servername&gt;、&lt;username&gt;@&lt;servername&gt; 和 &lt;password&gt;。

    ```json
    {
        "name": "AzureSqlDatabaseLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "typeProperties": {
                "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=SSISDB;User ID=<username>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
        }
        }
    ```
2. 在 **Azure PowerShell** 中，切換至 **C:\ADF\RunSSISPackage** 資料夾。
3. 執行 **>new-azdatafactorylinkedservice** Cmdlet 來建立連結服務： **>azuresqldatabaselinkedservice** 。 

    ```powershell
    New-AzDataFactoryLinkedService $df -File ".\AzureSqlDatabaseLinkedService.json"
    ```

### <a name="create-an-output-dataset"></a>建立輸出資料集
輸出資料集是 Dummy 資料集，可以驅動管線的排程。 請注意，頻率設為 [小時]，間隔設為 [1]。 因此，管線在管線開始與結束時間內會每小時執行一次。 

1. 使用下列內容建立 OutputDataset.json 檔案： 
    
    ```json
    {
        "name": "sprocsampleout",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": { },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }
    ```
2. 執行 **AzDataFactoryDataset** Cmdlet 來建立資料集。 

    ```powershell
    New-AzDataFactoryDataset $df -File ".\OutputDataset.json"
    ```

### <a name="create-a-pipeline-with-stored-procedure-activity"></a>使用預存程序活動建立管線 
在此步驟中，您會建立具有預存程序活動的管線。 活動會叫用 sp_executesql 預存程序以執行 SSIS 套件。 

1. 使用下列內容，在 **C:\ADF\RunSSISPackage** 資料夾中建立名為 **MyPipeline.json** 的 JSON 檔案：

    > [!IMPORTANT]
    > 在儲存檔案之前，以 SSIS 目錄中資料夾、專案和套件的名稱取代 &lt;資料夾名稱&gt;、&lt;專案名稱&gt;、&lt;套件名稱&gt;。

    ```json
    {
        "name": "MyPipeline",
        "properties": {
            "activities": [{
                "name": "SprocActivitySample",
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "sp_executesql",
                    "storedProcedureParameters": {
                        "stmt": "DECLARE @return_value INT, @exe_id BIGINT, @err_msg NVARCHAR(150)    EXEC @return_value=[SSISDB].[catalog].[create_execution] @folder_name=N'<folder name>', @project_name=N'<project name>', @package_name=N'<package name>', @use32bitruntime=0, @runinscaleout=1, @useanyworker=1, @execution_id=@exe_id OUTPUT    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @exe_id, @object_type=50, @parameter_name=N'SYNCHRONIZED', @parameter_value=1    EXEC [SSISDB].[catalog].[start_execution] @execution_id=@exe_id, @retry_count=0    IF(SELECT [status] FROM [SSISDB].[catalog].[executions] WHERE execution_id=@exe_id)<>7 BEGIN SET @err_msg=N'Your package execution did not succeed for execution ID: ' + CAST(@exe_id AS NVARCHAR(20)) RAISERROR(@err_msg,15,1) END"
                    }
                },
                "outputs": [{
                    "name": "sprocsampleout"
                }],
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }],
            "start": "2017-10-01T00:00:00Z",
            "end": "2017-10-01T05:00:00Z",
            "isPaused": false
        }
    }    
    ```

2. 若要建立管線： **RunSSISPackagePipeline** ，請執行 **AzDataFactoryPipeline** Cmdlet。

    ```powershell
    $DFPipeLine = New-AzDataFactoryPipeline -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -Name "RunSSISPackagePipeline" -DefinitionFile ".\RunSSISPackagePipeline.json"
    ```

### <a name="monitor-the-pipeline-run"></a>監視管道執行

1. 執行 **>get-azdatafactoryslice** 以取得輸出資料集之所有配量的詳細資料 * *，這是管線的輸出資料表。

    ```powershell
    Get-AzDataFactorySlice $df -DatasetName sprocsampleout -StartDateTime 2017-10-01T00:00:00Z
    ```
    請注意，您在此處指定的 StartDateTime 與在管線 JSON 中指定的開始時間是相同的。 
1. 執行 **Get-AzDataFactoryRun** ，來取得特定配量的活動回合詳細資料。

    ```powershell
    Get-AzDataFactoryRun $df -DatasetName sprocsampleout -StartDateTime 2017-10-01T00:00:00Z
    ```

    您可以繼續執行此 Cmdlet 直到您看到配量處於 **就緒** 狀態或 **失敗** 狀態。 

    您可以對伺服器中的 SSISDB 資料庫執行下列查詢，以確認封裝是否已執行。 

    ```sql
    select * from catalog.executions
    ```

## <a name="next-steps"></a>後續步驟
如需預存程序活動的詳細資訊，請參閱[預存程序活動](data-factory-stored-proc-activity.md)一文。