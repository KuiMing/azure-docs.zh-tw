---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 10/15/2020
ms.author: erhopf
ms.openlocfilehash: ee88a7cc187644c89aca5656df9ab9ae48a5a056
ms.sourcegitcommit: 93329b2fcdb9b4091dbd632ee031801f74beb05b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2020
ms.locfileid: "92096907"
---
1. 啟動 Eclipse。

1. 在 Eclipse Launcher 中，於 [工作區] 欄位中輸入新工作區目錄的名稱。 然後選取 [啟動]。

   ![Eclipse Launcher 的螢幕擷取畫面](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-jre-01-create-new-eclipse-workspace.png)

1. 不久之後，Eclipse IDE 的主要視窗隨即出現。 如果出現 [歡迎使用] 畫面，請加以關閉。

1. 從 Eclipse 功能表列中，選擇 [檔案] > [新增] > [專案] 以建立新專案。

1. [新增專案]  對話方塊隨即出現。 選取 [Java 專案]，然後選取 [下一步]。

   ![已醒目提示 [Java 專案] 的 [新增專案] 對話方塊螢幕擷取畫面](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-jre-02-select-wizard.png)

1. [新增 Java 專案] 精靈隨即啟動。 在 [專案名稱] 欄位中，輸入 **quickstart**，然後選擇 [JavaSE-1.8] 作為執行環境。 選取 [完成]。

   ![[新增 Java 專案] 精靈的螢幕擷取畫面](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-jre-03-create-java-project.png)

1. 如果出現 [是否要開啟相關的透視圖?] 視窗，請選取 [開啟透視圖]。

1. 在 [套件總管] 中，以滑鼠右鍵按一下 **quickstart** 專案。 從操作功能表中選擇 [設定] > [轉換成 Maven 專案]。

   ![套件總管的螢幕擷取畫面](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-jre-04-convert-to-maven-project.png)

1. [建立新的 POM] 視窗隨即出現。 在 [群組識別碼] 欄位中輸入 *com.microsoft.cognitiveservices.speech.samples*，並且在 [成品識別碼] 欄位中輸入 *quickstart*。 然後選取 [完成]。

   ![建立新的 POM 的螢幕擷取畫面](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-jre-05-configure-maven-pom.png)

1. 開啟 *pom.xml* 檔案並加以編輯。

   * 在檔案結尾的 `</project>` 結尾標記之前，建立 `repositories` 元素並讓其參考語音 SDK 的 Maven 存放庫，如下所示：

     [!code-xml[POM Repositories](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-microphone/pom.xml#repositories)]

   * 同時新增 `dependencies` 元素，並以語音 SDK 版本 1.13.0 作為相依性：

     [!code-xml[POM Dependencies](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-microphone/pom.xml#dependencies)]

   * 儲存變更。
