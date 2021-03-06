---
title: 在 Microsoft Azure 上部署安全的應用程式
description: 本文討論在 web 應用程式專案的發行和回應階段期間應考慮的最佳作法。
author: TerryLanfear
manager: barbkess
ms.author: terrylan
ms.date: 06/12/2019
ms.topic: article
ms.service: security
ms.subservice: security-develop
services: azure
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: 4c71ddbf1d2b435697b2707acf0b1262f2c5dc31
ms.sourcegitcommit: 5831eebdecaa68c3e006069b3a00f724bea0875a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/11/2020
ms.locfileid: "94517184"
---
# <a name="deploy-secure-applications-on-azure"></a>在 Azure 上部署安全的應用程式
在本文中，我們會提供在您部署雲端應用程式時所要考慮的安全性活動和控制項。 在 Microsoft [安全性開發生命週期 ](/previous-versions/windows/desktop/cc307891(v=msdn.10)) 的發行和回應階段期間，要考慮的安全性問題和概念 (SDL) 的涵蓋範圍。 目標是協助您定義活動和 Azure 服務，您可以使用這些服務來部署更安全的應用程式。

本文章涵蓋下列 SDL 階段：

- 版本
- 回應

## <a name="release"></a>版本
發行階段的重點是將專案用於公開發行。
這包括有效執行發行後服務工作，以及解決稍後可能發生之安全性弱點的規劃方法。

### <a name="check-your-applications-performance-before-you-launch"></a>開始之前，請先檢查您的應用程式效能

在您啟動應用程式或將更新部署至生產環境之前，請先檢查您的應用程式效能。 使用 Visual Studio 來執行雲端 [負載測試](https://www.visualstudio.com/docs/test/performance-testing/getting-started/getting-started-with-performance-testing) ，以找出應用程式中的效能問題、改善部署品質、確定您的應用程式一律啟動或可用，而且您的應用程式可以處理您的發行的流量。

### <a name="install-a-web-application-firewall"></a>安裝 Web 應用程式防火牆

Web 應用程式已逐漸成為利用常見已知弱點的惡意攻擊目標。 這些惡意探索中常見的是 SQL 插入式攻擊和跨網站腳本攻擊。 在應用程式程式碼中防止這些攻擊是一種挑戰。 它可能需要嚴格的維護、修補和監視應用程式拓撲的許多層級。 集中式 WAF 有助於簡化安全性管理。 WAF 解決方案也可以透過在中央位置修補已知弱點，與保護每個個別的 web 應用程式，來回應安全性威脅。

[Azure 應用程式閘道 WAF](../../web-application-firewall/ag/ag-overview.md)可為您的 web 應用程式提供集中式保護，使其免于遭遇常見的攻擊和弱點。 WAF 是以 [OWASP core 規則集](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) 3.0 或2.2.9 的規則為基礎。

### <a name="create-an-incident-response-plan"></a>建立事件回應計劃

準備事件回應計畫非常重要，可協助您解決一段時間可能會出現的新威脅。 準備事件回應計畫包含識別適當的安全性緊急連絡人，並為繼承自組織中其他群組的程式碼和授權的協力廠商程式碼建立安全性服務方案。

### <a name="conduct-a-final-security-review"></a>進行最終的安全性審查

刻意檢查已執行的所有安全性活動，有助於確保軟體版本或應用程式的就緒。  (FSR) 的最後一項安全性審查，通常包括對需求階段中所定義的品質閘道和 bug 列檢查威脅模型、工具輸出和效能。

### <a name="certify-release-and-archive"></a>認證版本與封存

在發行之前認證軟體，有助於確保符合安全性和隱私權需求。 封存所有相關資料對於執行發行後服務工作而言是不可或缺的。 封存也有助於降低與持續性軟體工程相關的長期成本。

## <a name="response"></a>回應
發行後階段的回應，是由開發小組所組成，而且可以適當地回應任何新興的軟體威脅和弱點報告。

### <a name="execute-the-incident-response-plan"></a>執行事件回應計畫

為了協助保護客戶免于出現的軟體安全性或隱私權弱點，必須能夠實行在發行階段中所起始的事件回應計畫。

### <a name="monitor-application-performance"></a>監視應用程式的效能

持續監視您的應用程式在部署後，可能會協助您偵測效能問題和安全性弱點。
協助應用程式監視的 Azure 服務包括：

  - Azure Application Insights
  - Azure 資訊安全中心

#### <a name="application-insights"></a>Application Insights

[Application Insights](../../azure-monitor/app/app-insights-overview.md) 是適用于多個平臺上的 網頁程式開發人員的可延伸應用程式效能管理 (APM) 服務。 您可以使用它來監視即時 Web 應用程式。 Application Insights 會自動偵測效能異常。 它包含強大的分析工具，可協助您診斷問題，並瞭解使用者實際如何運用您的應用程式。 它是設計來協助您持續改善效能和可用性。

#### <a name="azure-security-center"></a>Azure 資訊安全中心

[Azure 資訊安全中心](../../security-center/security-center-introduction.md) 可協助您防止、偵測和回應威脅，並提高對 Azure 資源（包括 web 應用程式）的 (和控制) 安全性的可見度。 Azure 資訊安全中心有助於偵測可能不會察覺到的威脅。 它適用于各種安全性解決方案。

資訊安全中心的免費層僅為 Azure 資源提供有限的安全性。 資訊 [安全中心標準層](../../security-center/security-center-get-started.md) 會將這些功能延伸到內部部署資源和其他雲端。
安全性中心標準可協助您：

  - 找出並修正安全性弱點。
  - 套用存取和應用程式控制，以封鎖惡意活動。
  - 流量分析和情報來偵測威脅。
  - 在遭受攻擊時迅速回應。

## <a name="next-steps"></a>後續步驟
在下列文章中，我們建議可協助您設計及開發安全應用程式的安全性控制與活動。

- [設計安全的應用程式](secure-design.md)
- [開發安全的應用程式](secure-develop.md)