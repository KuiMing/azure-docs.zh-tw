---
title: 教學課程：以 Azure Active Directory 設定 Coda 來自動佈建使用者 | Microsoft Docs
description: 了解如何從 Azure AD 對 Coda 自動佈建和取消佈建使用者帳戶。
services: active-directory
documentationcenter: ''
author: Zhchia
writer: Zhchia
manager: beatrizd
ms.assetid: 4d6f06dd-a798-4c22-b84f-8a11f1b8592a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/31/2020
ms.author: Zhchia
ms.openlocfilehash: dd625d082d15ce4a6ec59972483b06a84ea6d080
ms.sourcegitcommit: 0b9fe9e23dfebf60faa9b451498951b970758103
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/07/2020
ms.locfileid: "94355034"
---
# <a name="tutorial-configure-coda-for-automatic-user-provisioning"></a>教學課程：設定 Coda 來自動佈建使用者

此教學課程說明您在 Coda 與 Azure Active Directory (Azure AD) 中設定自動使用者佈建所需的步驟。 設定後，Azure AD 就會使用 Azure AD 佈建服務，自動對 [Coda](https://coda.io/) 佈建及取消佈建使用者。 如需此服務的用途、運作方式和常見問題等重要詳細資訊，請參閱[使用 Azure Active Directory 對 SaaS 應用程式自動佈建和取消佈建使用者](../app-provisioning/user-provisioning.md)。


## <a name="capabilities-supported"></a>支援的功能
> [!div class="checklist"]
> * 在 Coda 中建立使用者
> * 當使用者不再需要存取權時，將其從 Coda 中移除
> * 讓 Azure AD 與 Coda 之間的使用者屬性保持同步
> * 對 Coda 使用[單一登入](./coda-tutorial.md) (建議)

## <a name="prerequisites"></a>必要條件

本教學課程中概述的案例假設您已經具有下列必要條件：

* [Azure AD 租用戶](../develop/quickstart-create-new-tenant.md)
* Azure AD 中具有設定佈建[權限](../users-groups-roles/directory-assign-admin-roles.md)的使用者帳戶 (例如，應用程式管理員、雲端應用程式管理員、應用程式擁有者或全域管理員)。
* [Coda 企業](https://help.coda.io/en/articles/3520174-getting-started-with-sso)系統管理員帳戶。

## <a name="step-1-plan-your-provisioning-deployment"></a>步驟 1： 規劃佈建部署
1. 了解[佈建服務的運作方式](../app-provisioning/user-provisioning.md) \(部分機器翻譯\)。
2. 判斷誰會在[佈建範圍](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)內。
3. 判斷哪些資料要[在 Azure AD 與 Coda 之間對應](../app-provisioning/customize-application-attributes.md)。

## <a name="step-2-configure-coda-to-support-provisioning-with-azure-ad"></a>步驟 2： 設定 Coda 以支援使用 Azure AD 進行佈建

1. 選取工作區 [...] 功能表底下的 [組織設定]，以開啟您的組織管理主控台。

    ![Coda 企業組織 SCIM 設定](media/coda-provisioning-tutorial/coda-scim-enable.png)

2. 請確定已啟用 [使用 SCIM 佈建]。
3. 記下 SCIM 基底 URL 和 SCIM 持有人權杖。 如果沒有持有人權杖，請按一下 [產生新的權杖]。

## <a name="step-3-add-coda-from-the-azure-ad-application-gallery"></a>步驟 3： 從 Azure AD 應用程式庫新增 Coda

從 Azure AD 應用程式庫新增 Coda，以開始管理對 Coda 的佈建。 如果您先前已針對 SSO 設定 Coda，則可使用相同的應用程式。 不過，建議您在一開始測試整合時，建立個別的應用程式。 [在此](../manage-apps/add-application-portal.md)深入了解從資源庫新增應用程式。

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>步驟 4： 定義將在佈建範圍內的人員

Azure AD 佈建服務可讓您根據對應用程式的指派，和/或根據使用者屬性，界定將要佈建的人員。 如果選擇根據指派來界定將佈建至應用程式的人員，則可使用下列[步驟](../manage-apps/assign-user-or-group-access-portal.md)將使用者指派給應用程式。 如果選擇僅根據使用者屬性來界定將要佈建的人員，即可使用如[這裡](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)所述的範圍篩選條件。

* 將使用者指派給 Coda 時，您必須選取 [預設存取] 以外的角色。 具有預設存取角色的使用者會從佈建中排除，而且會在佈建記錄中被標示為沒有效率。 如果應用程式上唯一可用的角色是 [預設存取] 角色，您可以[更新應用程式資訊清單](../develop/howto-add-app-roles-in-azure-ad-apps.md) \(部分機器翻譯\) 以新增其他角色。

* 從小規模開始。 在推出給所有人之前，先使用一小部分的使用者進行測試。 當佈建範圍設為已指派的使用者時，您可將一或兩個使用者指派給應用程式來控制這點。 當範圍設為所有使用者時，您可指定[以屬性為基礎的範圍篩選條件](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)。


## <a name="step-5-configure-automatic-user-provisioning-to-coda"></a>步驟 5。 對 Coda 設定自動使用者佈建

本節將引導您逐步設定 Azure AD 佈建服務，以根據 Azure AD 中的使用者指派，在 TestApp 中建立、更新及停用使用者。

### <a name="to-configure-automatic-user-provisioning-for-coda-in-azure-ad"></a>在 Azure AD 中為 Coda 設定自動使用者佈建：

1. 登入 [Azure 入口網站](https://portal.azure.com)。 選取 [企業應用程式]，然後選取 [所有應用程式]。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

2. 在應用程式清單中，選取 [Coda]。

    ![應用程式清單中的 Coda 連結](common/all-applications.png)

3. 選取 [佈建]  索引標籤。

    ![[管理] 選項的螢幕擷取畫面，並已指出 [佈建] 選項。](common/provisioning.png)

4. 將 [佈建模式]  設定為 [自動]  。

    ![[佈建模式] 下拉式清單的螢幕擷取畫面，並已指出 [自動] 選項。](common/provisioning-automatic.png)

5. 在 [管理員認證] 區段底下，輸入您稍早在步驟 2 中擷取的 Coda 租用戶 URL 和祕密權杖。 按一下 [測試連線]，以確保 Azure AD 可以連線至 Coda。 如果連線失敗，請確定您的 Coda 帳戶具有管理員權限並再試一次。

    ![顯示 [管理員認證] 對話方塊的螢幕擷取畫面，您可以在其中輸入您的租用戶 URL 和祕密權杖。](./media/coda-provisioning-tutorial/provisioning.png)

6. 在 [通知電子郵件] 欄位中，輸入應該收到佈建錯誤通知的個人或群組電子郵件地址，然後選取 [發生失敗時傳送電子郵件通知] 核取方塊。

    ![通知電子郵件](common/provisioning-notification-email.png)

7. 選取 [儲存]。

8. 在 [對應] 區段中，選取 [將 Azure Active Directory 使用者同步至 Coda]。

9. 在 [屬性對應] 區段中，檢閱從 Azure AD 同步至 Coda 的使用者屬性。 選取為 [比對] 屬性 (Property) 的屬性 (Attribute) 會用來比對 Coda 中的使用者帳戶以進行更新作業。 如果您選擇變更[比對目標屬性](../app-provisioning/customize-application-attributes.md)，則必須確保 Coda API 支援根據該屬性來篩選使用者。 選取 [儲存] 按鈕以認可所有變更。

   |屬性|類型|
   |---|---|
   |userName|String|
   |作用中|Boolean|
   |name.givenName|String|
   |name.familyName|String|


10. 若要設定範圍篩選，請參閱[範圍篩選教學課程](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)中提供的下列指示。

11. 若要啟用 Coda 的 Azure AD 佈建服務，請在 [設定] 區段中，將 [佈建狀態] 變更為 [開啟]。

    ![佈建狀態已切換為開啟](common/provisioning-toggle-on.png)

12. 透過在 [設定] 區段的 [範圍] 中選擇需要的值，可定義要佈建到 Coda 的使用者。

    ![佈建範圍](common/provisioning-scope.png)

13. 當您準備好要佈建時，按一下 [儲存]  。

    ![儲存雲端佈建設定](common/provisioning-configuration-save.png)

針對 [設定] 區段的 [範圍] 中定義的所有使用者，此作業會啟動首次同步週期。 初始週期會比後續週期花費更多時間執行，只要 Azure AD 佈建服務正在執行，這大約每 40 分鐘便會發生一次。

## <a name="step-6-monitor-your-deployment"></a>步驟 6. 監視您的部署
設定佈建後，請使用下列資源來監視您的部署：

1. 使用[佈建記錄](../reports-monitoring/concept-provisioning-logs.md) \(部分機器翻譯\) 來判斷哪些使用者已佈建成功或失敗
2. 檢查[進度列](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md) \(部分機器翻譯\) 來查看佈建週期的狀態，以及其接近完成的程度
3. 如果佈建設定似乎處於狀況不良的狀態，應用程式將會進入隔離狀態。 [在此](../app-provisioning/application-provisioning-quarantine-status.md)深入了解隔離狀態。

## <a name="additional-resources"></a>其他資源

* [管理企業應用程式的使用者帳戶佈建](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>後續步驟

* [瞭解如何針對佈建活動檢閱記錄和取得報告](../app-provisioning/check-status-user-account-provisioning.md)