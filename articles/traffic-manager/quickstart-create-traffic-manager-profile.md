---
title: 快速入門：建立應用程式 HA 的設定檔 - Azure 入口網站 - Azure 流量管理員
description: 本快速入門文章會說明如何使用 Azure 入口網站建立流量管理員設定檔，以建置高可用性的 Web 應用程式。
services: traffic-manager
author: duongau
manager: twooley
Customer intent: As an IT admin, I want to direct user traffic to ensure high availability of web applications.
ms.service: traffic-manager
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/15/2020
ms.author: duau
ms.openlocfilehash: 45489d3500a4a744f2aeb34dc21122d180797133
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/16/2020
ms.locfileid: "92101293"
---
# <a name="quickstart-create-a-traffic-manager-profile-using-the-azure-portal"></a>快速入門：使用 Azure 入口網站建立流量管理員設定檔

本快速入門會說明如何建立流量管理員設定檔，以便為 Web 應用程式提供高可用性。

在本快速入門中，您會了解 Web 應用程式的兩個執行個體。 每個執行個體會在不同的 Azure 區域中執行。 您會建立以[端點優先順序](traffic-manager-routing-methods.md#priority-traffic-routing-method)為基礎的流量管理員設定檔。 此設定檔會將使用者流量導向執行 Web 應用程式的主要網站。 流量管理員會持續監視 Web 應用程式。 如果主要網站無法使用，它會提供自動容錯移轉至備份網站。

如果您沒有 Azure 訂用帳戶，請立即建立[免費帳戶](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。

## <a name="prerequisites"></a>Prerequisites

在本快速入門中，您必須在不同的 Azure 區域 (美國東部  和西歐  ) 中部署 Web 應用程式的兩個執行個體。 每個執行個體都會作為流量管理員的主要和容錯移轉端點。

1. 登入 [Azure 入口網站](https://portal.azure.com)。

1. 在畫面的左上方，選取 [建立資源]。 搜尋 **Web 應用程式** ，然後選取 [建立]。

1. 在 [建立 Web 應用程式]  中，在 [基本資訊]  索引標籤中輸入或選取下列值：

    | 設定                 | 值 |
    | ---                     | --- |
    | 訂用帳戶            | 選取您的訂用帳戶。 |    
    | 資源群組          | 選取 [新建]，並在文字方塊中輸入 *myResourceGroupTM1* 。|
    | Name                    | 輸入 Web 應用程式的唯一 **名稱** 。 這個範例會使用 *myWebAppEastUS* 。 |
    | 發佈                 | 選取 [程式碼]。 |
    | 執行階段堆疊           | 選取 [ASP.NET V4.7]。 |
    | 作業系統        | 選取 [Windows]。 |
    | 區域                  | 選取 [美國東部]  。 |
    | Windows 方案            | 選取 [新建]，然後在文字方塊中輸入 *myAppServicePlanEastUS* 。 |
    | SKU 和大小            | 選取 [標準 S1 100 總 ACU，1.75 GB 記憶體]。 |
   
1. 選取 [監視] 索引標籤，或選取 [下一步：監視]。  在 [監視] 下，將 [Application Insights] > [啟用 Application Insights] 設定為 [否]。

1. 選取 [檢閱及建立]  。

1. 檢閱設定，然後選取 [建立]。  成功部署 Web 應用程式時，它會建立預設網站。

1. 遵循步驟 1 至步驟 6，以建立名為 *myWebAppWestEurope* 的第二個 Web 應用程式。 [資源群組] 名稱是 [myResourceGroupTM2]，[區域] 是 [西歐]，且 [App Service 方案] 名稱是 [myAppServicePlanWestEurope]。 所有其他設定則與 *myWebAppEastUS* 相同。

## <a name="create-a-traffic-manager-profile"></a>建立流量管理員設定檔

建立可根據端點優先順序導向使用者流量的流量管理員設定檔。

1. 在畫面的左上方，選取 [建立資源]。 然後搜尋 [流量管理員設定檔]，並選取 [建立]。
1. 在 [建立流量管理員設定檔] 中，輸入或選取下列設定：

    | 設定 | 值 |
    | --------| ----- |
    | 名稱 | 為流量管理員設定檔輸入唯一的名稱。|
    | 路由方法 | 選取 [優先順序]。|
    | 訂用帳戶 | 選取您要套用流量管理員設定檔的訂用帳戶。 |
    | 資源群組 | 選取 [myResourceGroupTM1]。|
    | 位置 |此設定會參考資源群組的位置。 其不會影響將全球部署的流量管理員設定檔。|

1. 選取 [建立]。

## <a name="add-traffic-manager-endpoints"></a>新增流量管理員端點

將「美國東部」中的網站新增為所有使用者流量的主要路由目標端點。 將「西歐」中的網站新增為容錯移轉端點。 當主要端點無法使用時，流量就會自動路由傳送到容錯移轉端點。

1. 在入口網站的搜尋列中，輸入您在上一節建立的流量管理員設定檔名稱。
1. 從搜尋結果中選取設定檔。
1. 在 [流量管理員設定檔] 的 [設定] 區段中，選取 [端點]，然後選取 [新增]。

    :::image type="content" source="./media/quickstart-create-traffic-manager-profile/traffic-manager-endpoint-menu.png" alt-text="流量管理員設定檔中的端點設定":::

1. 輸入或選取下列設定：

    | 設定 | 值 |
    | ------- | ------|
    | 類型 | 選取 [Azure 端點]。 |
    | Name | 輸入 myPrimaryEndpoint。 |
    | 目標資源類型 | 選取 [App Service]。 |
    | 目標資源 | 選取 [選擇 App Service] > [美國東部]。 |
    | 優先順序 | 選取 [1]  。 狀況良好時，所有流量都會送至這個端點。 |

    :::image type="content" source="./media/quickstart-create-traffic-manager-profile/add-traffic-manager-endpoint.png" alt-text="您將端點新增至流量管理員設定檔的螢幕擷取畫面":::
    
1. 選取 [確定]。
1. 若要為第二個 Azure 區域建立容錯移轉端點，請使用下列設定重複步驟 3 和 4：

    | 設定 | 值 |
    | ------- | ------|
    | 類型 | 選取 [Azure 端點]。 |
    | Name | 輸入 myFailoverEndpoint。 |
    | 目標資源類型 | 選取 [App Service]。 |
    | 目標資源 | 選取 [選擇 App Service] > [西歐]。 |
    | 優先順序 | 選取 **2** 。 如果主要端點狀況不良，則所有流量都會送到此容錯移轉端點。 |

1. 選取 [確定]。

新增好兩個端點之後，它們會顯示在 [流量管理員設定檔] 中。 請注意，其監視狀態目前為 [線上]。

## <a name="test-traffic-manager-profile"></a>測試流量管理員設定檔

在本節中，您會檢查流量管理員設定檔的網域名稱。 您也會將主要端點設定為無法使用。 最後，您可以看到 Web 應用程式仍可使用。 這是因為流量管理員將流量傳送至容錯移轉端點。

### <a name="check-the-dns-name"></a>檢查 DNS 名稱

1. 在入口網站的搜尋列中，搜尋您在上一節建立的 **流量管理員設定檔** 名稱。
1. 選取流量管理員設定檔。 [概觀] 頁面隨即出現。
1. [流量管理員設定檔]  會顯示新建立之流量管理員設定檔的 DNS 名稱。
  
    :::image type="content" source="./media/quickstart-create-traffic-manager-profile/traffic-manager-dns-name.png" alt-text="流量管理員 DNS 名稱的位置螢幕擷取畫面":::

### <a name="view-traffic-manager-in-action"></a>檢視流量管理員的運作

1. 在網頁瀏覽器中，輸入流量管理員設定檔的 DNS 名稱，以檢視 Web 應用程式的預設網站。

    > [!NOTE]
    > 在此快速入門案例中，所有要求都會路由傳送至主要端點。 它會設定為 [優先順序 1]。

    :::image type="content" source="./media/quickstart-create-traffic-manager-profile/traffic-manager-test.png" alt-text="確認流量管理員設定檔可用性的網頁螢幕擷取畫面":::

1. 若要檢視流量管理員容錯移轉的運作，請停用主要網站：
    1. 在 [流量管理員設定檔] 頁面中，從 [概觀] 區段選取 [myPrimaryEndpoint]。
    1. 在 [myPrimaryEndpoint] 中，選取 [已停用] > [儲存]。
    1. 關閉 myPrimaryEndpoint。 請注意，狀態目前為 [已停用]。
1. 複製上一個步驟中的流量管理員設定檔 DNS 名稱，以在新的網頁瀏覽器工作階段中檢視網站。
1. 確認 Web 應用程式仍可使用。

主要端點無法使用，因此您會路由傳送至容錯移轉端點。

## <a name="clean-up-resources"></a>清除資源

完成時，請刪除資源群組、Web 應用程式和所有相關資源。 若要這麼做，從儀表板中選取每個個別項目，然後選取每個頁面頂端的 [刪除]。

## <a name="next-steps"></a>後續步驟

在本快速入門中，您建立了流量管理員設定檔。 它可讓您引導高可用性 Web 應用程式的使用者流量。 若要深入了解如何路由傳送流量，請繼續進行流量管理員的教學課程。

> [!div class="nextstepaction"]
> [流量管理員教學課程](tutorial-traffic-manager-improve-website-response.md)
