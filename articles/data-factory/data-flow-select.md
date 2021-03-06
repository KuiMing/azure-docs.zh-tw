---
title: 在對應資料流程中選取轉換
description: Azure Data Factory 對應資料流程選取轉換
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 06/02/2020
ms.openlocfilehash: 2d8c4d1915e22ccabf193f1b34c5fc4797ead549
ms.sourcegitcommit: 4f4a2b16ff3a76e5d39e3fcf295bca19cff43540
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93040241"
---
# <a name="select-transformation-in-mapping-data-flow"></a>在對應資料流程中選取轉換

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

使用「選取」轉換來重新命名、卸載或重新排列資料行。 這種轉換並不會改變數據列資料，而會選擇要將哪些資料行傳播至下游。 

在「選取」轉換中，使用者可以指定固定的對應、使用模式來執行以規則為基礎的對應，或啟用自動對應。 固定和以規則為基礎的對應可同時在相同的 select 轉換中使用。 如果資料行不符合其中一個已定義的對應，就會將它卸載。

## <a name="fixed-mapping"></a>固定對應

如果您的投射中定義的資料行少於50，則所有定義的資料行預設都有固定的對應。 固定的對應會接受定義的內送資料行，並將其對應至完整的名稱。

![固定對應](media/data-flow/fixedmapping.png "固定對應")

> [!NOTE]
> 您無法使用固定的對應來對應或重新命名漂移資料行

### <a name="mapping-hierarchical-columns"></a>對應階層式資料行

固定的對應可以用來將階層式資料行的 subcolumn 對應至最上層資料行。 如果您有已定義的階層，請使用資料行下拉式清單來選取 subcolumn。 Select 轉換將會建立新的資料行，其中包含 subcolumn 的值和資料類型。

![階層式對應](media/data-flow/select-hierarchy.png "階層式對應")

## <a name="rule-based-mapping"></a>規則型對應


> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4xiXz]

如果您想要一次對應許多資料行或傳遞漂移資料行，請使用以規則為基礎的對應，利用資料行模式來定義您的對應。 根據 `name` 、、和資料 `type` 行進行比對 `stream` `position` 。 您可以使用固定和以規則為基礎的對應組合。 依預設，具有大於50資料行的所有投影都會預設為以規則為基礎的對應，以符合每個資料行，並輸出輸入的名稱。 

若要加入以規則為基礎的對應，請按一下 [ **新增對應** ]，然後選取 [ **規則型對應** ]。

![螢幕擷取畫面顯示從 [新增對應] 選取的規則型對應。](media/data-flow/rule2.png "規則型對應")

每個以規則為基礎的對應都需要兩個輸入：要比對的條件，以及命名每個對應資料行的名稱。 這兩個值都是透過 [運算式](concepts-data-flow-expression-builder.md)產生器來輸入的。 在 [左運算式] 方塊中，輸入您的布林值比對條件。 在 [右運算式] 方塊中，指定要對應的相符資料行。

![螢幕擷取畫面顯示對應。](media/data-flow/rule-based-mapping.png "規則型對應")

使用 `$$` 語法來參考相符資料行的輸入名稱。 使用上述影像作為範例，假設使用者想要比對名稱少於六個字元的所有字串資料行。 如果有一個傳入的資料行命名為 `test` ，運算式 `$$ + '_short'` 就會將資料行重新命名 `test_short` 。 如果這是唯一存在的對應，就會從輸出資料中卸載不符合條件的所有資料行。

模式符合漂移和定義的資料行。 若要查看規則所對應的定義資料行，請按一下規則旁邊的 [眼鏡] 圖示。 使用資料預覽來驗證您的輸出。

### <a name="regex-mapping"></a>Regex 對應

如果您按一下向下箭號圖示，則可以指定 RegEx 對應條件。 Regex 對應條件符合所有符合指定之 RegEx 條件的資料行名稱。 這可以與標準規則型對應搭配使用。

![螢幕擷取畫面顯示具有階層層級和名稱相符專案的 RegEx 對應條件。](media/data-flow/regex-matching.png "規則型對應")

上述範例符合 RegEx 模式 `(r)` 或包含小寫 r 的任何資料行名稱。 類似于標準規則型對應，所有相符的資料行都是使用語法來改變右邊的條件 `$$` 。

如果您的資料行名稱中有多個 RegEx 相符專案，您可以參考特定的相符專案， `$n` 其中 ' n ' 指的是哪一個相符專案。 例如，' $2 ' 指的是資料行名稱中的第二個相符項。

### <a name="rule-based-hierarchies"></a>以規則為基礎的階層

如果您定義的投射具有階層，您可以使用以規則為基礎的對應來對應階層個子。 指定要對應其個子的比對條件和複雜資料行。 每個相符的 subcolumn 都會使用右側指定的「名稱即」規則進行輸出。

![螢幕擷取畫面顯示針對階層使用以規則為基礎的對應。](media/data-flow/rule-based-hierarchy.png "規則型對應")

上述範例符合複雜資料行的所有個子 `a` 。 `a` 包含兩個個子 `b` 和 `c` 。 輸出架構會包含兩個數據行 `b` ，而「 `c` 名稱」條件為 `$$` 。

### <a name="parameterization"></a>參數化

您可以使用以規則為基礎的對應，將資料行名稱參數化。 使用關鍵字來比對 ```name``` 傳入的資料行名稱與參數。 例如，如果您有資料流程參數 ```mycolumn``` ，您可以建立符合任何等於之資料行名稱的規則 ```mycolumn``` 。 您可以將符合的資料行重新命名為硬式編碼的字串，例如「業務索引鍵」，並明確地加以參考。 在此範例中，比對條件為， ```name == $mycolumn``` 且名稱條件為「商務索引鍵」。 

## <a name="auto-mapping"></a>自動對應

加入 [選取] 轉換時，可以藉由切換自動對應滑杆來啟用 **自動對應** 。 使用「自動對應」時，「選取」轉換會將所有內送資料行（不含重複專案）對應到其輸入的名稱。 這會包含漂移資料行，這表示輸出資料可能包含未在架構中定義的資料行。 如需漂移資料行的詳細資訊，請參閱 [架構漂移](concepts-data-flow-schema-drift.md)。

![自動對應](media/data-flow/automap.png "自動對應")

使用 auto 對應 on 時，select 轉換將會接受略過重複設定，並為現有的資料行提供新的別名。 在相同資料流程和自我聯結案例中進行多個聯結或查閱時，別名會很有用。 

## <a name="duplicate-columns"></a>重複的資料行

依預設，「選取」轉換會在輸入和輸出投射中卸載重複的資料行。 重複的輸入資料行通常來自聯結和查閱轉換，其中資料行名稱會在聯結的每一端複製。 如果您將兩個不同的輸入資料行對應至相同的名稱，則可能會發生重複的輸出資料行。 切換核取方塊，選擇是否要卸載或傳遞重複的資料行。

![略過重複項](media/data-flow/select-skip-dup.png "略過重複項")

## <a name="ordering-of-columns"></a>資料行的排序

對應的順序決定輸出資料行的順序。 如果輸入資料行對應多次，則只會接受第一個對應。 如果卸載任何重複的資料行，則會保留第一個相符項。

## <a name="data-flow-script"></a>資料流程指令碼

### <a name="syntax"></a>語法

```
<incomingStream>
    select(mapColumn(
        each(<hierarchicalColumn>, match(<matchCondition>), <nameCondition> = $$), ## hierarchical rule-based matching
        <fixedColumn>, ## fixed mapping, no rename
        <renamedFixedColumn> = <fixedColumn>, ## fixed mapping, rename
        each(match(<matchCondition>), <nameCondition> = $$), ## rule-based mapping
        each(patternMatch(<regexMatching>), <nameCondition> = $$) ## regex mapping
    ),
    skipDuplicateMapInputs: { true | false },
    skipDuplicateMapOutputs: { true | false }) ~> <selectTransformationName>
```

### <a name="example"></a>範例

以下是 select 對應及其資料流程腳本的範例：

![選取腳本範例](media/data-flow/select-script-example.png "選取腳本範例")

```
DerivedColumn1 select(mapColumn(
        each(a, match(true())),
        movie,
        title1 = title,
        each(match(name == 'Rating')),
        each(patternMatch(`(y)`),
            $1 + 'regex' = $$)
    ),
    skipDuplicateMapInputs: true,
    skipDuplicateMapOutputs: true) ~> Select1
```

## <a name="next-steps"></a>後續步驟
* 使用 Select 重新命名、重新排序和別名資料行之後，請使用 [接收轉換](data-flow-sink.md) 將資料放入資料存放區。
