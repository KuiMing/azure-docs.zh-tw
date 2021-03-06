---
title: Azure AD B2C 的最佳作法
titleSuffix: Azure AD B2C
description: 使用 Azure Active Directory B2C (Azure AD B2C) 時應考慮的建議和最佳作法。
services: active-directory-b2c
author: vigunase
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 06/06/2020
ms.author: vigunase
ms.subservice: B2C
ms.openlocfilehash: d8c0a5ce6f3befd41c0e1399363fd73726693837
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2020
ms.locfileid: "94949712"
---
# <a name="recommendations-and-best-practices-for-azure-active-directory-b2c"></a>Azure Active Directory B2C 的建議和最佳作法

下列最佳作法和建議涵蓋將 Azure Active Directory (Azure AD) B2C 整合至現有或新的應用程式環境的一些主要層面。

## <a name="fundamentals"></a>基礎

| 最佳做法 | 描述 |
|--|--|
| 選擇大部分案例的使用者流程 | Azure AD B2C 的 Identity Experience Framework 是服務的核心強度。 原則可完整描述身分識別體驗，例如註冊、登入或設定檔編輯。 為了協助您設定最常見的身分識別工作，Azure AD B2C 入口網站包含預先定義且可設定的原則，稱為使用者流程。 透過使用者流程，只要按幾下滑鼠，您就可以在幾分鐘內建立絕佳的使用者體驗。 [瞭解何時使用使用者流程與自訂原則](custom-policy-overview.md#comparing-user-flows-and-custom-policies)。|
| 應用程式註冊 | 每個應用程式 (web、原生) 和受保護的 API 都必須在 Azure AD B2C 中註冊。 如果應用程式同時具有 web 和原生版本的 iOS 和 Android，您可以使用相同的用戶端識別碼，將它們註冊為 Azure AD B2C 中的一個應用程式。 瞭解如何 [註冊 OIDC、SAML、web 和原生應用程式](./tutorial-register-applications.md?tabs=applications)。 深入瞭解 [可在 Azure AD B2C 中使用的應用程式類型](./application-types.md)。 |
| 移至每月活躍使用者計費 | Azure AD B2C 已從每月主動驗證移至每月作用中的使用者 (MAU) 帳單。 大部分的客戶都會發現此模型符合成本效益。 [深入瞭解每月活躍使用者計費](https://azure.microsoft.com/updates/mau-billing/)。 |

## <a name="planning-and-design"></a>規劃與設計

定義您的應用程式和服務架構、清查目前的系統，並規劃您的遷移至 Azure AD B2C。

| 最佳做法 | 描述 |
|--|--|
| 架構端對端解決方案 | 規劃 Azure AD B2C 整合時，請包含您所有應用程式的相依性。 請考慮目前在您環境中或可能需要新增至解決方案的所有服務和產品，例如 Azure Functions、客戶關係管理 (CRM) 系統、Azure API 管理閘道和儲存體服務。 將所有服務的安全性和擴充性納入考慮。 |
| 記錄您的使用者體驗 | 詳細說明您的客戶在應用程式中可體驗的所有使用者旅程。 在與您應用程式的身分識別和設定檔方面進行互動時，包括每個畫面以及它們可能會遇到的任何分支流程。 在您的規劃中包含可用性、協助工具和當地語系化。 |
| 選擇正確的驗證通訊協定 |  如需不同應用程式案例及其建議驗證流程的明細，請參閱 [案例和支援的驗證流程](../active-directory/develop/authentication-flows-app-scenarios.md#scenarios-and-supported-authentication-flows)。 |
| 試驗概念證明 (POC) 端對端使用者體驗 | 從我們的 [Microsoft 程式碼範例](code-samples.md) 和 [社區範例](https://github.com/azure-ad-b2c/samples)開始。 |
| 建立遷移計畫 |事先規劃可讓遷移更順暢。 深入瞭解 [使用者遷移](user-migration.md)。|
| 可用性與安全性 | 您的解決方案必須在應用程式可用性和您組織可接受的風險層級之間取得適當的平衡。 |
| 將內部部署相依性移至雲端 | 若要協助確保復原解決方案，請考慮將現有的應用程式相依性移至雲端。 |
| 將現有應用程式遷移至 b2clogin.com | Login.microsoftonline.com 的取代將于2020年12月的所有 Azure AD B2C 租使用者生效。 [深入了解](b2clogin.md)。 |
| 使用 Identity Protection 和條件式存取 | 使用這些功能可大幅提高對具風險驗證和存取原則的控制。 需要 Azure AD B2C Premium P2。 [深入了解](conditional-access-identity-protection-overview.md)。 |

## <a name="implementation"></a>實作

在執行階段中，請考慮下列建議。

| 最佳做法 | 描述 |
|--|--|
| 使用 Visual Studio Code 的 Azure AD B2C 延伸模組編輯自訂原則 | [從 Visual Studio Code Marketplace](https://marketplace.visualstudio.com/items?itemName=AzureADB2CTools.aadb2c)下載 Visual Studio Code 與此社區建立的擴充功能。 雖然並非官方的 Microsoft 產品，但 Visual Studio Code 的 Azure AD B2C 延伸模組包含數項功能，可協助您更輕鬆地使用自訂原則。 |
| 瞭解如何針對 Azure AD B2C 進行疑難排解 | 瞭解如何在開發期間針對 [自訂原則進行疑難排解](./troubleshoot-custom-policies.md?tabs=applications) 。 瞭解一般驗證流程的外觀，並使用工具來探索異常和錯誤。 例如，使用 [Application Insights](troubleshoot-with-application-insights.md) 來檢查使用者旅程的輸出記錄。 |
| 利用我們的經證實的自訂原則模式程式庫 | 尋找數個增強 Azure AD B2C 客戶身分識別與存取管理 (CIAM) 使用者旅程的 [範例](https://github.com/azure-ad-b2c/samples) 。 |

## <a name="testing"></a>測試

測試並自動化您的 Azure AD B2C 執行。

| 最佳做法 | 描述 |
|--|--|
| 全域流量的帳戶 | 使用來自不同全域位址的流量來源，以測試效能和當地語系化需求。 請確定所有的 HTMLs、CSS 和相依性都符合您的效能需求。 |
| 功能和 UI 測試 | 端對端測試使用者流程。 使用 Selenium、VS Web Test 等，每隔幾分鐘加入綜合測試。 |
| 畫筆測試 | 在使用您的解決方案之前，請執行滲透測試練習來確認所有元件都是安全的，包括任何協力廠商相依性。 確認您已使用存取權杖保護您的 Api，並針對您的應用程式案例使用正確的驗證通訊協定。 深入瞭解 [滲透測試](../security/fundamentals/pen-testing.md) 和 [Microsoft Cloud 整合滲透測試的參與規則](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)。 |
| A/B 測試 | 使用一組小型的隨機使用者來飛行您的新功能，然後再推出整個人口。 在 Azure AD B2C 中啟用 JavaScript 之後，您就可以整合 A/B 測試控管，例如 Optimizely、清楚的和其他工具。 |
| 負載測試 | Azure AD B2C 可以調整，但您的應用程式只能在其所有相依性可以調整時進行調整。 負載測試您的 Api 和 CDN。 |
| 節流 |  如果在短時間內從相同來源傳送太多要求，Azure AD B2C 節流流量。 在負載測試時使用數個流量來源，並 `AADB2C90229` 在您的應用程式中正常處理錯誤碼。 |
| 自動化 | 使用持續整合和傳遞 (CI/CD) 管線來自動化測試和部署，例如 [Azure DevOps](deploy-custom-policies-devops.md)。 |

## <a name="operations"></a>作業

管理您的 Azure AD B2C 環境。

| 最佳做法 | 描述 |
|--|--|
| 建立多個環境 | 為了簡化作業和部署的推出，請建立不同的環境來進行開發、測試、生產階段前和生產環境。 為每個租使用者建立 Azure AD B2C 的租使用者。 |
| 使用自訂原則的版本控制 | 請考慮為您的 Azure AD B2C 自訂原則使用 GitHub、Azure Repos 或其他雲端式版本控制系統。 |
| 使用 Microsoft Graph API 將 B2C 租使用者的管理自動化 | Microsoft Graph Api：<br/>管理 [Identity Experience Framework](/graph/api/resources/trustframeworkpolicy?preserve-view=true&view=graph-rest-beta) (自訂原則) <br/>[[索引鍵]](/graph/api/resources/trustframeworkkeyset?preserve-view=true&view=graph-rest-beta)<br/>[使用者流程](/graph/api/resources/identityuserflow?preserve-view=true&view=graph-rest-beta) |
| 與 Azure DevOps 整合 | [CI/CD 管線](deploy-custom-policies-devops.md)可讓您輕鬆地在不同環境之間移動程式碼，並確保生產環境隨時都能就緒。   |
| 與 Azure 監視器整合 | [Audit 記錄檔事件](view-audit-logs.md) 只會保留七天。 [與 Azure 監視器整合](azure-monitor.md) 以保留記錄以供長期使用，或與協力廠商安全性資訊和事件管理整合 (SIEM) 工具，以深入瞭解您的環境。 |
| 設定主動警示和監視 | 使用 Application Insights 追蹤 Azure AD B2C 中的[使用者行為](./analytics-with-application-insights.md)。 |

## <a name="support-and-status-updates"></a>支援和狀態更新

隨時掌握最新的服務狀態，並尋找支援選項。

| 最佳做法 | 描述 |
|--|--|
| [服務更新](https://azure.microsoft.com/updates/?product=active-directory-b2c) |  隨時掌握 Azure AD B2C 產品更新和公告的最新消息。 |
| [Microsoft 支援服務](support-options.md) | 提出 Azure AD B2C 技術問題的支援要求。 計費及訂用帳戶管理支援均為免費提供。 |
| [Azure 狀態](https://status.azure.com/status) | 查看所有 Azure 服務目前的健全狀況狀態。 |