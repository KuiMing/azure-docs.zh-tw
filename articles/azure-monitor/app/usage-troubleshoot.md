---
title: 針對使用者分析工具（Azure 應用程式見解）進行疑難排解
description: 疑難排解指南 - 使用 Application Insights 分析網站和應用程式使用情況。
ms.topic: conceptual
author: NumberByColors
ms.author: daviste
ms.date: 07/11/2018
ms.reviewer: mbullwin
ms.openlocfilehash: 02d1bd9d204d88ba90218b1254c66ac0da80be85
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "87323497"
---
# <a name="troubleshoot-user-behavior-analytics-tools-in-application-insights"></a>針對 Application Insights 中的使用者行為分析工具進行疑難排解
您有關於 [Application Insights 中的使用者行為分析工具](usage-overview.md)：[使用者、工作階段、事件](usage-segmentation.md)、[漏斗圖](usage-funnels.md)、[使用者流程](usage-flows.md)[保留期](usage-retention.md)或同群使用者的問題嗎？ 以下是一些解答。

## <a name="counting-users"></a>計算使用者
**使用者行為分析工具顯示我的應用程式有一個使用者/會話，但我知道我的應用程式有許多使用者/會話。如何修正這些不正確的計數？**

Application Insights 中的所有遙測事件都有[匿名使用者識別碼](./data-model-context.md)和[工作階段識別碼](./data-model-context.md)，這兩者是其標準屬性的一部分。 根據預設，所有使用情況分析工具都是以這些識別碼為基礎來計算使用者和工作階段。 如果這些標準屬性中未填入應用程式之各個使用者和工作階段的唯一識別碼，您就會在使用情況分析工具中看到不正確的使用者及工作階段計數。

如果您要監視 Web 應用程式，最簡單的解決方案就是將 [Application Insights JavaScript SDK](./javascript.md) 新增至您的應用程式，並確定您要監視的每個頁面上，都已載入指令碼的程式碼片段。 JavaScript SDK 會自動產生匿名使用者與工作階段識別碼，然後在應用程式送出遙測事件時，於遙測事件中填入這些識別碼。

如果您要監視 Web 服務 (沒有使用者介面)，請根據服務的唯一使用者和工作階段概念，[建立遙測初始設定式，以填入匿名使用者識別碼和工作階段識別碼屬性](usage-send-user-context.md)。

如果您的應用程式傳送[已驗證的使用者識別碼](./api-custom-events-metrics.md#authenticated-users)，您可以根據使用者工具中的已驗證使用者識別碼來進行計算。 在 [顯示] 下拉式清單中，選擇 [已驗證的使用者]。

使用者行為分析工具目前不支援以匿名使用者識別碼、已驗證的使用者識別碼或工作階段識別碼之外的屬性作為基礎，來計算使用者或工作階段。

## <a name="naming-events"></a>命名事件
**我的應用程式有數千個不同的網頁檢視和自訂事件名稱。很難區分它們，而使用者行為分析工具通常會沒有回應。如何修正這些命名問題？**

整個使用者行為分析工具都會使用網頁檢視和自訂事件名稱。 若要獲得這些工具的價值，就必須適當地為這些事件命名。 目標是有太少的一般名稱 ( 「按鈕已按下」 ) ，以及具有太多、過於明確的名稱 ( 「在 HTTP：/www.contoso.com/index」 ) 上按一下 [編輯 \/ ] 按鈕。

若要對應用程式所傳送的網頁檢視和自訂事件名稱進行任何變更，您必須變更應用程式的原始程式碼，然後重新部署。 **Application Insights 中的所有遙測資料都會儲存 90 天，且無法加以刪除**，因此您對事件名稱所進行的變更需要 90 天才會完整顯示。 進行名稱變更之後的 90 天內，遙測中的事件名稱會新舊並陳，因此請據以調整查詢，並告知您的小組。

如果應用程式傳送太多網頁檢視名稱，請確認是您在程式碼中手動指定了這些網頁檢視名稱，還是 Application Insights JavaScript SDK 自動傳送了這些網頁檢視名稱：

* 如果使用[ `trackPageView` API](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md)以手動方式在程式碼中指定網頁檢視名稱，請將名稱變更為較不特定的名稱。 請避免常見錯誤，例如將 URL 放入網頁檢視名稱中。 相反地，請使用 `trackPageView` API 的 URL 參數。 將網頁檢視名稱中的其他詳細資料移至自訂屬性中。

* 如果是 Application Insights JavaScript SDK 自動傳送了網頁檢視名稱，您可以變更網頁標題，也可以改為手動傳送網頁檢視名稱。 依預設，SDK 會以網頁檢視名稱的形式，傳送每一個網頁的[標題](https://developer.mozilla.org/docs/Web/HTML/Element/title)。 您可以將標題改得較為普通，但請留意 SEO 和這項變更可能會造成的其他影響。 使用 `trackPageView` API 手動指定網頁檢視名稱會覆寫自動收集到的名稱，因此您不必變更網頁標題就能在遙測中傳送較為普通的名稱。   

如果應用程式傳送太多自訂事件名稱，請將程式碼中的名稱改為不會太過特殊的名稱。 同樣地，請避免直接將 URL 和其他個別網頁或動態的資訊放在自訂事件名稱中。 相反地，請使用 `trackEvent` API 將這些詳細資料移至自訂事件的自訂屬性中。 例如，請避免使用 `appInsights.trackEvent("Edit button clicked on http://www.contoso.com/index")`，而是建議使用 `appInsights.trackEvent("Edit button clicked", { "Source URL": "http://www.contoso.com/index" })` 之類的形式。

## <a name="next-steps"></a>接下來的步驟

* [使用者行為分析工具概觀](usage-overview.md)

## <a name="get-help"></a>取得說明
* [Stack Overflow](https://stackoverflow.com/questions/tagged/ms-application-insights)

