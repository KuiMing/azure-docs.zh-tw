---
title: 適用于內容傳遞網路的 Azure 安全性基準
description: 內容傳遞網路安全性基準提供的程式指引和資源，可讓您執行 Azure 安全性基準測試中所指定的安全性建議。
author: msmbaldwin
ms.service: azure-cdn
ms.topic: conceptual
ms.date: 11/16/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 64731ce443ddd806402efd33fe22300ab8362d18
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2020
ms.locfileid: "94663455"
---
# <a name="azure-security-baseline-for-content-delivery-network"></a>適用于內容傳遞網路的 Azure 安全性基準

此安全性基準會將來自 [Azure 安全性基準測試版本 2.0](../security/benchmarks/overview.md) 的指引套用至內容傳遞網路。 Azure 安全性基準提供如何在 Azure 上保護雲端解決方案的建議。 內容會依 Azure 安全性基準測試所定義的 **安全性控制** ，以及適用于內容傳遞網路的相關指引來分組。 尚未排除適用于內容傳遞網路的 **控制項**。

若要瞭解內容傳遞網路如何完全對應至 Azure 安全性基準測試，請參閱 [完整的內容傳遞網路安全性基準對應](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines)檔案。

## <a name="identity-management"></a>身分識別管理

*如需詳細資訊，請參閱 [Azure 安全性基準測試：身分識別管理](/azure/security/benchmarks/security-controls-v2-identity-management)。*

### <a name="im-6-restrict-azure-resource-access-based-on-conditions"></a>IM-6：根據條件限制 Azure 資源存取

**指導** 方針：依國家或地區限制對內容傳遞網路內容的存取。 在 CDN 端點上建立特定路徑的規則，以允許或封鎖所選國家或地區中使用地區篩選功能的內容。

- [依國家或地區限制 Azure CDN 內容](cdn-restrict-access-by-country.md)

**Azure 資訊安全中心監視**：目前無法使用

**責任**：客戶

## <a name="privileged-access"></a>特殊許可權存取

*如需詳細資訊，請參閱 [Azure 安全性基準測試：](/azure/security/benchmarks/security-controls-v2-privileged-access)特殊許可權存取。*

### <a name="pa-2-restrict-administrative-access-to-business-critical-systems"></a>PA-2：限制對商務關鍵性系統的系統管理存取

**指導** 方針：使用 azure 角色型存取控制 (內容傳遞網路中的 azure RBAC) ，藉由限制哪些帳戶被授與存取權給他們所在的訂用帳戶和管理群組，來隔離對商務關鍵性系統的存取。

也可限制存取管理、身分識別和安全性系統，這些系統具有您商務關鍵性存取權的系統管理存取權，例如 Active Directory 網網域控制站 (Dc) 、安全性工具和系統管理工具，以及安裝在商務關鍵性系統上的代理程式。 入侵這些管理和安全性系統的攻擊者，可以立即 weaponize 它們來入侵商務關鍵資產。

所有類型的存取控制都應符合您的企業分割策略，以確保存取控制的一致性。

- [Azure 元件和參考模型](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151) 

- [管理群組存取](../governance/management-groups/overview.md#management-group-access) 

- [Azure 訂用帳戶管理員](../cost-management-billing/manage/add-change-subscription-administrator.md)

**Azure 資訊安全中心監視**：目前無法使用

**責任**：共用

### <a name="pa-6-use-privileged-access-workstations"></a>PA-6：使用特殊許可權的存取工作站

**指導** 方針：安全、隔離的工作站對於機密角色（例如系統管理員、開發人員和重要服務操作員）的安全性至關重要。 使用高度安全的使用者工作站及/或 Azure 防禦來進行系統管理工作。 使用 Azure Active Directory (Azure AD) 、Microsoft Defender 進階威脅防護 (ATP) 和/或 Microsoft Intune 部署安全且受管理的使用者工作站以進行系統管理工作。 受保護的工作站可以集中管理以強制執行安全的設定，包括強式驗證、軟體和硬體基準、受限的邏輯和網路存取。

- [瞭解特殊許可權的存取工作站](../active-directory/devices/concept-azure-managed-workstation.md) 

- [部署特殊許可權存取工作站](../active-directory/devices/howto-azure-managed-workstation.md)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="pa-7-follow-just-enough-administration-least-privilege-principle"></a>PA-7：遵循足夠的系統管理 (最低許可權準則)  

**指導** 方針：內容傳遞網路會與 Azure 角色型存取控制整合 (RBAC) 來管理其資源。 使用 Azure RBAC 透過角色指派來管理 Azure 資源存取權。 將這些角色指派給使用者、群組服務主體和受控識別。 某些資源有預先定義的內建角色，而這些角色可透過 Azure CLI、Azure PowerShell 或 Azure 入口網站等工具進行清查或查詢。 

依據角色的要求，限制您透過 Azure RBAC 指派給資源的許可權。 這可補充 Azure AD Privileged Identity Management (PIM) 的及時 (JIT) 方法，並應定期檢查。 

此外，您也可以使用內建角色來配置許可權，並只在必要時才建立自訂角色。

- [什麼是 Azure 角色型存取控制 (Azure RBAC) ](../role-based-access-control/overview.md) 

- [如何在 Azure 中設定 RBAC](../role-based-access-control/role-assignments-portal.md) 

- [如何使用 Azure AD 身分識別和存取權評論](../active-directory/governance/access-reviews-overview.md)

**Azure 資訊安全中心監視**：目前無法使用

**責任**：共用

## <a name="asset-management"></a>資產管理

*如需詳細資訊，請參閱 [Azure 安全性基準測試：資產管理](/azure/security/benchmarks/security-controls-v2-asset-management)。*

### <a name="am-1-ensure-security-team-has-visibility-into-risks-for-assets"></a>上午-1：確保安全性小組能看到資產的風險

**指導** 方針：確定安全性小組已獲得 Azure 租使用者和訂用帳戶中的安全性讀取者許可權，以便他們使用 Azure 資訊安全中心監視安全性風險。 

根據安全性小組責任的結構，監視安全性風險可能是中央安全性小組或本地小組的責任。 不過，安全性見解和風險一律必須在組織內集中匯總。 

安全性讀取者許可權可以廣泛地套用到整個租使用者， (根管理群組) 或限定于管理群組或特定訂用帳戶。 

注意：您可能需要額外的許可權，才能看到工作負載和服務。 

- [安全性讀取者角色總覽](../role-based-access-control/built-in-roles.md#security-reader)

- [Azure 管理群組總覽](../governance/management-groups/overview.md)

**Azure 資訊安全中心監視**：目前無法使用

**責任**：客戶

### <a name="am-2-ensure-security-team-has-access-to-asset-inventory-and-metadata"></a>上午-2：確保安全性小組可以存取資產清查和中繼資料

**指導** 方針：將標記套用至您的 Azure 資源、資源群組和訂用帳戶，以邏輯方式將它們組織成分類法。 每個標記都是由一個名稱和一個值配對組成。 例如，您可以將「環境」名稱和「生產」值套用至生產環境中的所有資源。

- [如何使用 Azure Resource Graph Explorer 建立查詢](../governance/resource-graph/first-query-portal.md) 

- [Azure 資訊安全中心資產庫存管理](../security-center/asset-inventory.md) 

- [資源命名與標記決策指南](https://docs.microsoft.com/azure/cloud-adoption-framework/decision-guides/resource-tagging/?toc=/azure/azure-resource-manager/management/toc.json)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="am-3-use-only-approved-azure-services"></a>上午-3：僅使用已核准的 Azure 服務

**指導** 方針：使用 Azure 原則來審核和限制使用者可以在您的環境中布建的服務。 使用 Azure Resource Graph 來查詢及探索其訂用帳戶內的資源。 您也可以使用 Azure 監視器來建立規則，以在偵測到未核准的服務時觸發警示。

- [如何設定和管理 Azure 原則](../governance/policy/tutorials/create-and-manage.md) 

- [如何使用 Azure 原則拒絕特定的資源類型](../governance/policy/samples/built-in-policies.md#general) 

- [如何使用 Azure Resource Graph Explorer 建立查詢](../governance/resource-graph/first-query-portal.md)

**Azure 資訊安全中心監視**：目前無法使用

**責任**：客戶

### <a name="am-4-ensure-security-of-asset-lifecycle-management"></a>上午-4：確保資產生命週期管理的安全性

**指導** 方針：內容資料網路無法用於確保生命週期管理程式中資產的安全性。 客戶必須負責維護資產的屬性和網路設定，這些資產會被視為高衝擊。 建議客戶建立程式來捕獲屬性和網路設定變更、測量變更影響，以及建立補救工作（適用的話）。

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

## <a name="logging-and-threat-detection"></a>記錄和威脅偵測

*如需詳細資訊，請參閱 [Azure 安全性基準測試：記錄和威脅偵測](/azure/security/benchmarks/security-controls-v2-logging-threat-protection)。*

### <a name="lt-1-enable-threat-detection-for-azure-resources"></a>LT-1：啟用 Azure 資源的威脅偵測

**指導** 方針：內容傳遞網路不提供原生功能來監視與其資源相關的安全性威脅。

將任何記錄從內容傳遞網路轉送至您的安全性資訊和事件管理 (SIEM) 解決方案，可用來設定自訂威脅偵測。 

監視不同類型的 Azure 資產是否有潛在威脅和異常，並著重于取得高品質的警示，以減少因分析師進行排序所需的誤報。 警示可能源自于記錄資料、代理程式或其他資料。

- [建立自訂分析規則來偵測威脅](../sentinel/tutorial-detect-threats-custom.md) 

- [使用 Azure Sentinel 的網路威脅情報](/azure/architecture/example-scenario/data/sentinel-threat-intelligence)

**Azure 資訊安全中心監視**：目前無法使用

**責任**：客戶

### <a name="lt-3-enable-logging-for-azure-network-activities"></a>LT-3：啟用 Azure 網路活動的記錄功能

**指導** 方針：內容傳遞網路不適合部署到虛擬網路，因為這樣您無法啟用網路安全性群組流量記錄、透過防火牆路由傳送流量或執行封包捕獲。

內容傳遞網路會記錄它為客戶存取所處理的所有網路流量。 在您已部署的供應專案資源內啟用網路流程功能，並設定將這些記錄傳送至儲存體帳戶，以進行長期保留和審核。

- [教學課程-如何設定 Azure 內容傳遞網路中的診斷記錄](cdn-azure-diagnostic-logs.md)

**Azure 資訊安全中心監視**：不適用

**責任**：共用

### <a name="lt-4-enable-logging-for-azure-resources"></a>LT-4：啟用 Azure 資源的記錄

**指導** 方針：自動提供的活動記錄，包含內容傳遞網路資源的所有寫入作業 (PUT、POST、DELETE) ，但讀取作業 (取得) 。 當您進行疑難排解時，可以使用活動記錄來尋找錯誤，或是監視組織中的使用者修改資源的方式。

啟用 CDN 的 Azure 資源記錄檔，您可以使用 Azure 資訊安全中心和 Azure 原則來啟用資源記錄和記錄資料收集。 這些記錄檔可能很重要，可供稍後調查安全性事件和執行法庭訓練。

- [如何使用 Azure 監視器收集平臺記錄和計量](../azure-monitor/platform/diagnostic-settings.md) 

- [瞭解 Azure 中的記錄和不同的記錄類型](../azure-monitor/platform/platform-logs-overview.md) 

- [瞭解 Azure 資訊安全中心資料收集](../security-center/security-center-enable-data-collection.md)

**Azure 資訊安全中心監視**：是

**責任**：共用

## <a name="incident-response"></a>事件回應

*如需詳細資訊，請參閱 [Azure 安全性基準測試：事件回應](/azure/security/benchmarks/security-controls-v2-incident-response)。*

### <a name="ir-1-preparation--update-incident-response-process-for-azure"></a>IR-1：準備–更新 Azure 的事件回應程式

**指導** 方針：確定您的組織有處理常式來回應安全性事件、已更新 Azure 的這些處理常式，並會定期執行這些程式以確保其就緒。

- [跨企業環境執行安全性](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

- [事件回應參考指南](/microsoft-365/downloads/IR-Reference-Guide.pdf)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="ir-2-preparation--setup-incident-notification"></a>IR-2：準備–安裝事件通知

**指導** 方針：在 Azure 資訊安全中心中設定安全性事件連絡人資訊。 如果 Microsoft 安全性回應中心 (MSRC) 發現您的資料已被非法或未經授權的物件存取，Microsoft 會使用此連絡人資訊來與您聯繫。 您也可以選擇根據您的事件回應需求，在不同的 Azure 服務中自訂事件警示和通知。 

- [如何設定 Azure 資訊安全中心安全性連絡人](../security-center/security-center-provide-security-contact-details.md)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="ir-3-detection-and-analysis--create-incidents-based-on-high-quality-alerts"></a>IR-3：偵測和分析–根據高品質警示建立事件

**指導** 方針：確定您有建立高品質警示和測量警示品質的流程。 這可讓您學習過去事件的課程，並設定分析師的警示優先順序，讓他們不會浪費時間誤報。 

高品質的警示可以根據過去事件的體驗、驗證的社區來源，以及設計來藉由融合和關聯各種信號來源來產生和清除警示的工具來建立。 

Azure 資訊安全中心在許多 Azure 資產之間提供高品質的警示。 您可以使用 ASC 資料連線器，將警示串流至 Azure Sentinel。 Azure Sentinel 可讓您建立 advanced 警示規則，以自動產生調查的事件。 

使用「匯出」功能來匯出您的 Azure 資訊安全中心警示和建議，以協助找出 Azure 資源的風險。 以手動方式或持續持續的方式匯出警示和建議。

- [如何設定匯出](../security-center/continuous-export.md)

- [如何將警示串流至 Azure Sentinel](../sentinel/connect-azure-security-center.md)

**Azure 資訊安全中心監視**：目前無法使用

**責任**：客戶

### <a name="ir-4-detection-and-analysis--investigate-an-incident"></a>IR-4：偵測和分析–調查事件

**指導** 方針：確定分析師可以在調查潛在事件時查詢及使用不同的資料來源，以全面瞭解發生的情況。 您應收集不同的記錄檔，以追蹤潛在攻擊者在整個終止鏈上的活動，以避免點。  您也應該確定已針對其他分析師和未來的歷程記錄參考來捕捉見解和學習。  

用於調查的資料來源包括已從範圍內服務和執行中的系統收集的集中式記錄來源，但也可以包括：

- 網路資料–使用網路安全性群組的流量記錄、Azure 網路監看員和 Azure 監視器來捕捉網路流量記錄和其他分析資訊。 

- 執行中系統的快照集： 

    - 使用 Azure 虛擬機器的快照集功能來建立執行中系統磁片的快照集。 

    - 使用作業系統的原生記憶體傾印功能來建立執行中系統記憶體的快照。

    - 您可以使用 Azure 服務的快照集功能，或您軟體本身的功能來建立執行中系統的快照集。

Azure Sentinel 在幾乎任何記錄來源和案例管理入口網站中提供廣泛的資料分析，以管理事件的完整生命週期。 調查期間的智慧資訊可以與事件產生關聯，以供追蹤和報告之用。 

- [建立 Windows 機器磁片的快照集](../virtual-machines/windows/snapshot-copy-managed-disk.md)

- [建立 Linux 機器磁片的快照集](../virtual-machines/linux/snapshot-copy-managed-disk.md)

- [Microsoft Azure 支援診斷資訊和記憶體傾印集合](https://azure.microsoft.com/support/legal/support-diagnostic-information-collection/) 

- [使用 Azure Sentinel 調查事件](../sentinel/tutorial-investigate-cases.md)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="ir-5-detection-and-analysis--prioritize-incidents"></a>IR-5：偵測和分析-設定事件優先順序

**指導** 方針：提供內容給分析人員，根據警示嚴重性和資產敏感度先將焦點放在哪個事件上。 

Azure 資訊安全中心會將嚴重性指派給每個警示，以協助您排列應先調查哪些警示的優先順序。 嚴重性取決於資訊安全中心在尋找或用來發出警示的分析中的可信度，以及導致警示的活動背後有惡意意圖的信賴等級。

此外，請使用標記來標示資源，並建立命名系統來識別和分類 Azure 資源，尤其是處理敏感性資料的資源。  您需負責根據發生事件的 Azure 資源和環境的重要性，設定警示的補救優先順序。

- [Azure 資訊安全中心的安全性警示](../security-center/security-center-alerts-overview.md)

- [使用標記來組織 Azure 資源](/azure/azure-resource-manager/resource-group-using-tags)

**Azure 資訊安全中心監視**：目前無法使用

**責任**：客戶

### <a name="ir-6-containment-eradication-and-recovery--automate-the-incident-handling"></a>IR-6：內含專案、根除和復原–將事件處理自動化

**指導** 方針：自動化手動重複的工作，以加快回應時間並降低分析師的負擔。 手動工作需要較長的時間來執行、讓每個事件變慢，並減少分析師可以處理的事件數目。 手動工作也會增加分析師疲勞，這樣會增加造成延遲的人為錯誤的風險，並降低分析師在複雜工作上有效地專注的能力。 使用 Azure 資訊安全中心和 Azure Sentinel 中的工作流程自動化功能，自動觸發動作或執行腳本，以回應傳入的安全性警示。 腳本會採取動作，例如傳送通知、停用帳戶，以及隔離有問題的網路。 

- [在安全中心設定工作流程自動化](../security-center/workflow-automation.md)

- [在 Azure 資訊安全中心中設定自動化威脅回應](../security-center/tutorial-security-incident.md#triage-security-alerts)

- [在 Azure Sentinel 中設定自動化威脅回應](../sentinel/tutorial-respond-threats-playbook.md)

**Azure 資訊安全中心監視**：目前無法使用

**責任**：客戶

## <a name="posture-and-vulnerability-management"></a>狀況和弱點管理

*如需詳細資訊，請參閱 [Azure 安全性基準測試：狀態和弱點管理](/azure/security/benchmarks/security-controls-v2-vulnerability-management)。*

### <a name="pv-3-establish-secure-configurations-for-compute-resources"></a>PV-3：建立計算資源的安全設定

**指導** 方針：不適用;內容傳遞網路沒有提供任何安全性設定功能。

**Azure 資訊安全中心監視**：不適用

**責任**：共用

### <a name="pv-7-rapidly-and-automatically-remediate-software-vulnerabilities"></a>PV-7：快速且自動補救軟體弱點

**指導** 方針：快速部署軟體更新，以補救作業系統和應用程式中的軟體弱點。 

使用常見的風險評分方案設定補救的優先順序 (例如，常見的弱點評分系統) ，或協力廠商掃描工具所提供的預設風險評等。 使用應用程式的內容呈現高安全性風險，以及哪些應用程式需要高執行時間，以量身打造您的環境。

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="pv-8-conduct-regular-attack-simulation"></a>PV-8：進行定期攻擊模擬

**指導** 方針：如有需要，請在您的 Azure 資源上進行滲透測試或 red team 活動，並確保修復所有重要的安全性結果。
遵循 Microsoft Cloud 滲透測試的參與規則，以確保您的滲透測試不違反 Microsoft 原則。 針對受 Microsoft 管理的雲端基礎結構、服務和應用程式，使用 Microsoft 的策略和執行的 Red 小組和即時網站滲透測試。

- [Azure 中的滲透測試](../security/fundamentals/pen-testing.md)

- [滲透測試運作規則](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 

- [Microsoft Cloud Red 小組](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure 資訊安全中心監視**：不適用

**責任**：共用

## <a name="governance-and-strategy"></a>控管與策略

*如需詳細資訊，請參閱 [Azure 安全性基準測試：治理和策略](/azure/security/benchmarks/security-controls-v2-governance-strategy)。*

### <a name="gs-1-define-asset-management-and-data-protection-strategy"></a>GS-1：定義資產管理與資料保護策略 

**指導** 方針：確保您可以記錄並傳達明確的策略，以持續監視和保護系統和資料。 優先探索、評估、保護及監視商務關鍵資料和系統。 

此策略應該包含已記載的指導方針、原則，以及下列元素的標準： 

-   以商務風險為依據的資料分類標準

-   安全性組織風險和資產清查的可見度 

-   安全性組織核准 Azure 服務以供使用 

-   資產在其生命週期內的安全性

-   根據組織資料分類所需的存取控制策略

-   使用 Azure 原生和協力廠商資料保護功能

-   傳輸中和待用使用案例的資料加密需求

-   適當的密碼編譯標準

您可以在參考的連結取得詳細資訊。

- [Azure 安全性架構建議-儲存體、資料和加密](https://docs.microsoft.com/azure/architecture/framework/security/storage-data-encryption?toc=/security/compass/toc.json&amp;bc=/security/compass/breadcrumb/toc.json)

- [Azure 安全性基礎-Azure 資料安全性、加密和儲存體](../security/fundamentals/encryption-overview.md)

- [雲端採用架構-Azure 資料安全性和加密最佳作法](https://docs.microsoft.com/azure/security/fundamentals/data-encryption-best-practices?toc=/azure/cloud-adoption-framework/toc.json&amp;bc=/azure/cloud-adoption-framework/_bread/toc.json)

- [Azure 安全性基準測試-資產管理](/azure/security/benchmarks/security-benchmark-v2-asset-management)

- [Azure 安全性基準測試-資料保護](/azure/security/benchmarks/security-benchmark-v2-data-protection)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="gs-2-define-enterprise-segmentation-strategy"></a>GS-2：定義企業分割策略 

**指導** 方針：使用身分識別、網路、應用程式、訂用帳戶、管理群組和其他控制項的組合，建立企業級的策略來分割資產的存取權。

請仔細平衡安全性分隔的需求，以及針對需要彼此通訊並存取資料的系統，啟用每日作業的需求。

請確定跨控制項類型（包括網路安全性、身分識別和存取模型、應用程式許可權/存取模型，以及人力流程式控制件）一致地實行分割策略。

- [Azure 中的分割策略指引 (影片) ](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151)

- [Azure (檔中的分割策略指引) ](/security/compass/governance#enterprise-segmentation-strategy)

- [利用企業分割策略來調整網路分割](/security/compass/network-security-containment#align-network-segmentation-with-enterprise-segmentation-strategy)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="gs-3-define-security-posture-management-strategy"></a>GS-3：定義安全性狀態管理原則

**指導** 方針：持續測量並減輕個別資產及其託管環境的風險。 設定高價值資產的優先順序以及高度公開的攻擊面，例如已發佈的應用程式、網路輸入和輸出點、使用者和系統管理員端點等等。

- [Azure 安全性基準測試-狀態與弱點管理](/azure/security/benchmarks/security-benchmark-v2-posture-vulnerability-management)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="gs-4-align-organization-roles-responsibilities-and-accountabilities"></a>GS-4：讓組織角色、職責和責任保持一致

**指導** 方針：確保您可以記錄安全性組織中角色和責任的明確策略，並對其進行溝通。 優先提供安全性決策的明確責任、教育所有人共同責任模型，以及教育技術小組來保護雲端。

- [Azure 安全性最佳作法 1-人員：教育小組雲端安全性旅程](/azure/cloud-adoption-framework/security/security-top-10#1-people-educate-teams-about-the-cloud-security-journey)

- [Azure 安全性最佳作法 2-人員：教育小組雲端安全性技術](/azure/cloud-adoption-framework/security/security-top-10#2-people-educate-teams-on-cloud-security-technology)

- [Azure 安全性最佳作法 3-處理：指派雲端安全性決策的責任](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="gs-5-define-network-security-strategy"></a>GS-5：定義網路安全性策略

**指導** 方針：建立 Azure 網路安全性方法作為組織整體安全性存取控制策略的一部分。  

此策略應該包含已記載的指導方針、原則，以及下列元素的標準： 

-   集中式網路管理和安全性責任

-   虛擬網路分割模型與企業分割策略一致

-   不同威脅和攻擊案例中的補救策略

-   網際網路邊緣與輸入和輸出策略

-   混合式雲端和內部部署互連能力策略

-   最新的網路安全性構件 (例如網狀圖、參考網路架構) 

如需詳細資訊，請參閱下列參考資料：
- [Azure 安全性最佳做法 11-架構。單一整合的安全性策略](/azure/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy)

- [Azure 安全性基準測試-網路安全性](/azure/security/benchmarks/security-benchmark-v2-network-security)

- [Azure 網路安全性概觀](../security/fundamentals/network-overview.md)

- [商業網路架構策略](/azure/cloud-adoption-framework/ready/enterprise-scale/architecture)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="gs-6-define-identity-and-privileged-access-strategy"></a>GS-6：定義身分識別和特殊許可權存取策略

**指導** 方針：建立 Azure 身分識別和特殊許可權的存取方法，作為組織整體安全性存取控制策略的一部分。  

此策略應該包含已記載的指導方針、原則，以及下列元素的標準： 

-   集中式身分識別和驗證系統，以及其與其他內部和外部身分識別系統的互連能力

-   不同使用案例和條件中的強式驗證方法

-   保護高許可權的使用者

-   異常使用者活動監視和處理  

-   使用者身分識別和存取權檢查和協調流程

您可以在參考的連結取得詳細資訊。

- [Azure 安全性基準測試-身分識別管理](/azure/security/benchmarks/security-benchmark-v2-identity-management)

- [Azure 安全性基準測試-特殊許可權存取](/azure/security/benchmarks/security-benchmark-v2-privileged-access)

- [Azure 安全性最佳做法 11-架構。單一整合的安全性策略](/azure/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy)

- [Azure 身分識別管理安全性概觀](../security/fundamentals/identity-management-overview.md)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="gs-7-define-logging-and-threat-response-strategy"></a>GS-7：定義記錄和威脅回應策略

**指導** 方針：建立記錄和威脅回應策略，以快速偵測和補救威脅，同時符合合規性需求。 排定為分析師提供高品質的警示和順暢的體驗，讓他們可以專注于威脅，而不是整合和手動步驟。 

此策略應該包含已記載的指導方針、原則，以及下列元素的標準：

-   安全性作業 (SecOps) 組織的角色和責任 

-   妥善定義的事件回應程式，與 NIST 或其他產業架構一致 

-   記錄檔捕捉和保留，以支援威脅偵測、事件回應和合規性需求

-   使用 SIEM、原生 Azure 功能和其他來源，集中查看威脅的相關資訊以及相關資訊 

-   與您的客戶、供應商和感興趣的公開方相關的通訊和通知計畫

-   使用 Azure 原生和協力廠商平臺進行事件處理，例如記錄和威脅偵測、取證和攻擊補救和根除

-   處理事件和事件後活動的流程，例如經驗教訓和辨識項保留

您可以在參考的連結取得詳細資訊。

- [Azure 安全性基準測試-記錄和威脅偵測](/azure/security/benchmarks/security-benchmark-v2-logging-threat-detection)

- [Azure 安全性基準測試-事件回應](/azure/security/benchmarks/security-benchmark-v2-incident-response)

- [Azure 安全性最佳作法 4-處理。更新雲端的事件回應程式](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

- [Azure 採用架構、記錄和報告決策指南](/azure/cloud-adoption-framework/decision-guides/logging-and-reporting/)

- [Azure 企業規模、管理及監視](/azure/cloud-adoption-framework/ready/enterprise-scale/management-and-monitoring)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

## <a name="next-steps"></a>後續步驟

- 請參閱 [Azure 安全性基準測試 V2 總覽](/azure/security/benchmarks/overview)
- 深入了解 [Azure 資訊安全性基準](/azure/security/benchmarks/security-baselines-overview)
