---
ms.openlocfilehash: be983a643b45847d2a44db018b1383aa56ab2302
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "88691034"
---
1. 從這個位置複製存放庫： https://github.com/Azure-Samples/live-video-analytics-iot-edge-python 。
1. 在 Visual Studio Code 中，開啟存放庫下載所在的資料夾。
1. 在 Visual Studio Code 中，移至 *src/cloud-to-device-console-app* 資料夾。 在其中建立一個檔案，並將其命名為 *appsettings.json*。 此檔案將包含執行程式所需的設定。
1. 從您稍早在本快速入門中產生的 *~/clouddrive/lva-sample/appsettings.json* 檔案複製內容。

    文字看起來應該會像下列輸出。

    ```
    {  
        "IoThubConnectionString" : "HostName=xxx.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey=XXX",  
        "deviceId" : "lva-sample-device",  
        "moduleId" : "lvaEdge"  
    }
    ```
1. 移至 *src/edge* 資料夾，並建立名為 *.env* 的檔案。
1. 複製 */clouddrive/lva-sample/edge-deployment/.env* 檔案的內容。 文字看起來應該會像下列程式碼。

    ```
    SUBSCRIPTION_ID="<Subscription ID>"  
    RESOURCE_GROUP="<Resource Group>"  
    AMS_ACCOUNT="<AMS Account ID>"  
    IOTHUB_CONNECTION_STRING="HostName=xxx.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey=xxx"  
    AAD_TENANT_ID="<AAD Tenant ID>"  
    AAD_SERVICE_PRINCIPAL_ID="<AAD SERVICE_PRINCIPAL ID>"  
    AAD_SERVICE_PRINCIPAL_SECRET="<AAD SERVICE_PRINCIPAL ID>"  
    INPUT_VIDEO_FOLDER_ON_DEVICE="/home/lvaadmin/samples/input"  
    OUTPUT_VIDEO_FOLDER_ON_DEVICE="/var/media"
    APPDATA_FOLDER_ON_DEVICE="/var/local/mediaservices"
    CONTAINER_REGISTRY_USERNAME_myacr="<your container registry username>"  
    CONTAINER_REGISTRY_PASSWORD_myacr="<your container registry password>"      
    ```
    