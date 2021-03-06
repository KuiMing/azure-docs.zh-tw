---
title: 應用程式閘道上的 Azure Web 應用程式防火牆 (WAF) v2 自訂規則
description: 本文概述 Azure 應用程式閘道上 (WAF) v2 自訂規則的 Web 應用程式防火牆。
services: web-application-firewall
ms.topic: article
author: vhorne
ms.service: web-application-firewall
ms.date: 04/14/2020
ms.author: victorh
ms.openlocfilehash: c0f802f5113e38e811c110ee913099e76fa7be0b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "81383820"
---
# <a name="custom-rules-for-web-application-firewall-v2-on-azure-application-gateway"></a>Azure 應用程式閘道上的 Web 應用程式防火牆 v2 自訂規則

Azure 應用程式閘道 Web 應用程式防火牆 (WAF) v2 隨附預先設定、平臺管理的規則集，可提供各種不同類型攻擊的保護。 這些攻擊包括跨網站腳本、SQL 插入和其他攻擊。 如果您是 WAF 系統管理員，您可能會想要撰寫自己的規則來增強核心規則集 (CRS) 規則。 您的規則可以根據符合條件來封鎖或允許要求的流量。

自訂規則可讓您建立自己的規則，這些規則會針對每個通過 WAF 的要求進行評估。 這些規則的優先順序會高於受控規則集中的其餘規則。 自訂規則包含規則名稱、規則優先順序和相符條件的陣列。 如果符合這些條件，就會採取動作來 (允許或封鎖) 。

例如，您可以封鎖 192.168.5.4/24 範圍內 IP 位址的所有要求。 在此規則中，運算子為 *IPMatch*、MATCHVALUES 是 IP 位址範圍 (192.168.5.4/24) ，而動作是封鎖流量。 您也可以設定規則的名稱和優先權。

自訂規則支援使用複合邏輯來建立更先進的規則，以滿足您的安全性需求。 例如， (條件 1 **和** 條件 2]) **或** [條件 3]) 。 這表示如果符合條件 1 **和** 條件2， **或** 符合條件3，則 WAF 應該採取自訂規則中指定的動作。

相同規則內的不同比對條件，一律會使用 **和**複合。 例如，封鎖來自特定 IP 位址的流量，而且只有在使用特定瀏覽器的情況下。

如果您想要 **或** 兩個不同的條件，則這兩個條件必須在不同的規則中。 例如，封鎖特定 IP 位址的流量，或封鎖使用特定瀏覽器的流量。

> [!NOTE]
> WAF 自訂規則的最大數目是100。 如需應用程式閘道限制的詳細資訊，請參閱 [Azure 訂用帳戶和服務限制、配額和條件約束](../../azure-resource-manager/management/azure-subscription-service-limits.md#application-gateway-limits)。

自訂規則也支援正則運算式，就像在 CRS 規則集中一樣。 如需範例，請參閱 [建立和使用自訂 web 應用程式防火牆規則](create-custom-waf-rules.md)中的範例3和5。

## <a name="allowing-vs-blocking"></a>允許與封鎖

使用自訂規則可讓您輕鬆地使用和封鎖流量。 例如，您可以封鎖來自某個 IP 位址範圍的所有流量。 如果要求來自特定的瀏覽器，您可以建立另一個規則來允許流量。

若要允許某項功能，請確定 `-Action` 參數已設定為 [ **允許**]。 若要封鎖某些問題，請確定 `-Action` 參數已設定為 [ **封鎖**]。

```azurepowershell
$AllowRule = New-AzApplicationGatewayFirewallCustomRule `
   -Name example1 `
   -Priority 2 `
   -RuleType MatchRule `
   -MatchCondition $condition `
   -Action Allow

$BlockRule = New-AzApplicationGatewayFirewallCustomRule `
   -Name example2 `
   -Priority 2 `
   -RuleType MatchRule `
   -MatchCondition $condition `
   -Action Block
```

上一個 `$BlockRule` 對應至 Azure Resource Manager 中的下列自訂規則：

```json
"customRules": [
      {
        "name": "blockEvilBot",
        "priority": 2,
        "ruleType": "MatchRule",
        "action": "Block",
        "matchConditions": [
          {
            "matchVariables": [
              {
                "variableName": "RequestHeaders",
                "selector": "User-Agent"
              }
            ],
            "operator": "Contains",
            "negationConditon": false,
            "matchValues": [
              "evilbot"
            ],
            "transforms": [
              "Lowercase"
            ]
          }
        ]
      }
    ], 
```

此自訂規則包含必須符合才能進行動作的名稱、優先順序、動作和相符條件的陣列。 如需這些欄位的進一步說明，請參閱下欄欄位描述。 如需自訂規則的範例，請參閱 [建立和使用自訂 web 應用程式防火牆規則](create-custom-waf-rules.md)。

## <a name="fields-for-custom-rules"></a>自訂規則的欄位

### <a name="name-optional"></a>名稱 [選用]

規則的名稱。  它會出現在記錄中。

### <a name="priority-required"></a>Priority [必要]

- 決定規則估價順序。 值越低，規則的評估就越早。 允許的範圍是從1-100。 
- 在所有自訂規則中必須是唯一的。 優先順序為40的規則會在優先順序為80的規則之前進行評估。

### <a name="rule-type-required"></a>規則類型 [必要]

目前必須是 **MatchRule**。

### <a name="match-variable-required"></a>Match 變數 [required]

必須是下列其中一個變數：

- RemoteAddr –遠端電腦連接的 IP 位址/主機名稱
- RequestMethod – HTTP 要求方法 (GET、POST、PUT、DELETE 等。 ) 
- QueryString – URI 中的變數
- PostArgs – POST 主體中傳送的引數。 只有在 ' Content-type ' 標頭設定為 ' application/x-www-urlencoded ' 和 ' 多部分/表單資料 ' 時，才會套用使用此比對變數的自訂規則。
- RequestUri –要求的 URI
- RequestHeaders –要求的標頭
- RequestBody –這包含整體的整個要求主體。 只有在 ' Content-type ' 標頭設定為 ' application/x-www-urlencoded ' 時，才會套用使用此比對變數的自訂規則。 
- RequestCookies –要求的 Cookie

### <a name="selector-optional"></a>選取器 [選用]

描述 matchVariable 集合的欄位。 例如，如果 matchVariable 是 RequestHeaders，則選取器可能會在 *使用者代理程式* 標頭上。

### <a name="operator-required"></a>Operator [必要]

必須是下列其中一個運算子：

- IPMatch-僅在符合變數為*RemoteAddr*時使用
- 等於–輸入與 MatchValue 相同
- 包含
- LessThan
- GreaterThan
- LessThanOrEqual
- GreaterThanOrEqual
- BeginsWith
- EndsWith
- Regex
- Geomatch (preview) 

### <a name="negate-condition-optional"></a>否定條件 [選用]

否定目前的條件。

### <a name="transform-optional"></a>轉換 [選用]

在嘗試比對之前，具有要執行之轉換名稱的字串清單。 這些可以是下列轉換：

- 小寫
- Trim
- UrlDecode
- UrlEncode 
- RemoveNulls
- HtmlEntityDecode

### <a name="match-values-required"></a>符合值 [必要]

要比對的值清單，可視為 *或*「ed」。 例如，它可以是 IP 位址或其他字串。 值格式取決於上一個運算子。

### <a name="action-required"></a>動作 [必要]

- Allow –授權交易，略過所有其他規則。 指定的要求會新增到允許清單中，一旦相符，要求就會停止進一步評估並傳送至後端集區。 系統不會針對任何進一步的自訂規則或受控規則評估允許清單上的規則。
- Block –根據 *SecDefaultAction* (偵測/預防模式) 來封鎖交易。 就像允許動作一樣，在評估要求並將其加入至封鎖清單之後，就會停止評估並封鎖要求。 任何符合相同條件的要求都不會進行評估，而且只會遭到封鎖。 
- 記錄：讓規則寫入記錄檔，但讓其餘規則執行以進行評估。 其他自訂規則會依優先順序來評估，後面接著 managed 規則。

## <a name="geomatch-custom-rules-preview"></a>地區比對自訂規則 (預覽)

自訂規則可讓您建立量身打造的規則，以符合您應用程式和安全性原則的確切需求。 您可以依國家/地區限制對您 web 應用程式的存取。 如需詳細資訊，請參閱 [Geomatch 自訂規則 (預覽) ](geomatch-custom-rules.md)。

## <a name="next-steps"></a>後續步驟

瞭解自訂規則之後，[建立您自己的自訂規則](create-custom-waf-rules.md)。
