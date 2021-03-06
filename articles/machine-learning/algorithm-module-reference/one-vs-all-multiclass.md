---
title: 一對多多級
titleSuffix: Azure Machine Learning
description: 瞭解如何在 Azure Machine Learning 設計工具中使用「一對多」多元分類模組，以建立二元分類模型的集團。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 11/13/2020
ms.openlocfilehash: 4dfe284a00052cbd1915d62355e1d7772f3712ab
ms.sourcegitcommit: 1cf157f9a57850739adef72219e79d76ed89e264
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/13/2020
ms.locfileid: "94591864"
---
# <a name="one-vs-all-multiclass"></a>一對多多級

本文說明如何在 Azure Machine Learning 設計工具中使用「一對多」多元分類模組。 目標是要使用 *一對多* 方法來建立可以預測多個類別的分類模型。

當結果取決於連續或類別預測量變數時，此模組可用來建立能夠預測三個或更多可能結果的模型。 此方法也可讓您使用二元分類方法解決需要多個輸出級別的問題。

### <a name="more-about-one-versus-all-models"></a>深入瞭解一對一模型

某些分類演算法允許依設計使用兩個以上的類別。 其他的結果會將可能的結果限制為兩個值的其中一個 (二進位或雙類別的模型) 。 但即使是二元分類演算法也可以透過各種策略來調整多類別分類工作。 

這個模組會執行一對多方法，在此方法中，會為多個輸出類別建立二進位模型。 模組會針對個別類別，根據其補數來評估每個二進位模型， (模型中的所有其他類別) ，就像是二元分類問題一樣。 除了其計算效率以外 (只 `n_classes` 需要分類器) ，這種方法的其中一個優點就是它的可解譯性。 由於每個類別只會以一個和一個分類器表示，因此您可以藉由檢查對應的分類器來瞭解類別的相關知識。 這是多元分類最常使用的策略，而且是公平的預設選擇。 然後，模組會執行這些二元分類器並選擇具有最高信賴分數的預測，以執行預測。 

基本上，此模組會建立個別模型的集團，然後合併結果，以建立可預測所有類別的單一模型。 任何二元分類器都可用來做為一對一模型的基礎。  

例如，假設您設定 [兩個類別的支援向量機器](two-class-support-vector-machine.md) 模型，並將它提供給「一對多」多元分類模組的輸入。 模組會針對輸出類別的所有成員建立兩個類別的支援向量機器模型。 然後，它會套用一對多方法來合併所有類別的結果。  

此課程模組使用 sklearn 的 OneVsRestClassifier，您可以在 [此](https://scikit-learn.org/stable/modules/generated/sklearn.multiclass.OneVsRestClassifier.html)深入瞭解詳細資料。

## <a name="how-to-configure-the-one-vs-all-multiclass-classifier"></a>如何設定「一對多」多元分類分類器  

此課程模組會建立二元分類模型的集團來分析多個類別。 若要使用此模組，您必須先設定和定型 *二元分類* 模型。 

您可以將二進位模型連接到「一對多」多元分類模組。 然後，您可以使用 [定型模型](train-model.md) 搭配加上標籤的訓練資料集來定型模型的集團。

當您結合模型時，「一對多」多元分類會建立多個二元分類模型、將每個類別的演算法優化，然後合併模型。 雖然訓練資料集可能會有多個類別值，模組仍會執行這些工作。

1. 在設計工具中，將「一對多」多元分類模組新增至您的管線。 您可以在 [ **分類** ] 類別中的 [ **Machine Learning-初始化** ] 下找到此模組。

   「一對多」多元分類分類器沒有自己的可設定參數。 任何自訂都必須在提供做為輸入的二元分類模型中完成。

2. 將二元分類模型加入至管線，並設定該模型。 例如，您可能會使用 [二級支援向量機器](two-class-support-vector-machine.md) 或 [雙類別促進式決策樹](two-class-boosted-decision-tree.md)。

3. 將「 [定型模型](train-model.md) 」模組新增至您的管線。 連接「一對多」多元分類之輸出的未定型分類器。

4. 在 [ [定型模型](train-model.md)] 的其他輸入上，連接具有多個類別值的加上標籤訓練資料集。

5. 提交管線。

## <a name="results"></a>結果

定型完成之後，您可以使用模型來進行多元預測。

或者，您可以將未定型的分類器傳遞至 [交叉驗證模型](cross-validate-model.md) ，以根據加上標籤的驗證資料集進行交叉驗證。


## <a name="next-steps"></a>後續步驟

請參閱 Azure Machine Learning 的[可用模組集](module-reference.md)。 
