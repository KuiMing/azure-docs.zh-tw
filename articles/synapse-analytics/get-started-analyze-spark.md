---
title: 快速入門：開始使用 Spark 進行分析
description: 在本教學課程中，您將了解如何使用 Apache Spark 來分析資料
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: spark
ms.topic: tutorial
ms.date: 07/20/2020
ms.openlocfilehash: 89bc2723a0d7c99160c651fb433db6f8892ee676
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/04/2020
ms.locfileid: "93321085"
---
# <a name="analyze-with-apache-spark"></a>使用 Apache Spark 進行分析

## <a name="analyze-nyc-taxi-data-in-blob-storage-using-spark"></a>使用 Spark 分析 Blob 儲存體中的 NYC 計程車資料

在本教學課程中，您將了解使用適用於 Azure Synapse 的 Apache Spark 來載入和分析資料的基本步驟。

1. 在 [資料] 中樞內，按一下 [新增資源] (**已連結** 上方的加號按鈕) >> [瀏覽範例]。 尋找 **NYC 計程車和禮車委員會 - 黃色計程車路線記錄**，然後在其上方按一下。 在頁面底部按 [繼續]，然後 **新增資料集**。 現在，在 [已連結] 底下的 [資料] 中樞內，以滑鼠右鍵按一下 [Azure Blob 儲存體 >> 範例資料集 >> nyc_tlc_yellow] 並選取 [新增筆記本]
1. 這會使用下列程式碼建立新的 Notebook：
    ```
    from azureml.opendatasets import NycTlcYellow

    data = NycTlcYellow()
    data_df = data.to_spark_dataframe()
    display(data_df.limit(10))
    ```
1. 在筆記本中，選擇 [連結至] 功能表中的無伺服器 Spark 集區
1. 選取資料格上的 [執行]

## <a name="load-the-nyc-taxi-data-into-the-spark-nyctaxi-database"></a>將 NYC 計程車資料載入 Spark nyctaxi 資料庫

**SQLDB1** 的資料表中已有可用資料。 將其載入名為 **nyctaxi** 的 Spark 資料庫。

1. 在 Synapse Studio 中，移至 [開發] 中樞。
1. 選取 [+] > [Notebook]。
1. 在筆記本頂端，將 [附加至] 的值設定為 **Spark1**。
1. 選取 [新增程式碼] 以新增筆記本程式碼儲存格，然後貼上下列文字：

    ```scala
    %%spark
    spark.sql("CREATE DATABASE IF NOT EXISTS nyctaxi")
    val df = spark.read.sqlanalytics("SQLDB1.dbo.Trip") 
    df.write.mode("overwrite").saveAsTable("nyctaxi.trip")
    ```

1. 移至 [資料] 中樞，以滑鼠右鍵按一下 [資料庫]，然後選取 [重新整理]。 您應該會看到這些資料庫：

    - **SQLDB1** (專用 SQL 集區)
    - **nyctaxi** (無伺服器 Apache Spark 集區)

## <a name="analyze-the-nyc-taxi-data-using-spark-and-notebooks"></a>使用 Spark 和筆記本來分析 NYC 計程車資料

1. 返回筆記本。
1. 建立新的程式碼儲存格，並輸入下列文字。 然後執行儲存格，以顯示我們載入到 **nyctaxi** Spark 資料庫中的 NYC 計程車資料。

   ```py
   %%pyspark
   df = spark.sql("SELECT * FROM nyctaxi.trip") 
   display(df)
   ```

1. 執行下列程式碼，以執行我們先前對專用 SQL 集區 **SQLDB1** 執行的相同分析。 此程式碼會將分析的結果儲存到名為 **nyctaxi.passengercountstats** 的資料表中，並將結果視覺化。

   ```py
   %%pyspark
   df = spark.sql("""
      SELECT PassengerCount,
          SUM(TripDistanceMiles) as SumTripDistance,
          AVG(TripDistanceMiles) as AvgTripDistance
      FROM nyctaxi.trip
      WHERE TripDistanceMiles > 0 AND PassengerCount > 0
      GROUP BY PassengerCount
      ORDER BY PassengerCount
   """) 
   display(df)
   df.write.saveAsTable("nyctaxi.passengercountstats")
   ```

1. 在資料格結果中，選取 [圖表] 可查看視覺化的資料。

## <a name="customize-data-visualization-with-spark-and-notebooks"></a>使用 Spark 和筆記本自訂資料視覺效果

您可以使用筆記本來控制圖表的呈現方式。 下列程式碼顯示一個簡單的範例。 其使用熱門的程式庫 **matplotlib** 和 **seaborn**。 該程式碼會轉譯與稍早所執行 SQL 查詢相同類型的折線圖。

```py
%%pyspark
import matplotlib.pyplot
import seaborn

seaborn.set(style = "whitegrid")
df = spark.sql("SELECT * FROM nyctaxi.passengercountstats")
df = df.toPandas()
seaborn.lineplot(x="PassengerCount", y="SumTripDistance" , data = df)
seaborn.lineplot(x="PassengerCount", y="AvgTripDistance" , data = df)
matplotlib.pyplot.show()
```



## <a name="load-data-from-a-spark-table-into-a-dedicated-sql-pool-table"></a>將資料從 Spark 資料表載入專用 SQL 集區資料表

先前我們已將資料從專用 SQL 集區資料表 **SQLDB1.dbo.Trip** 複製到 Spark 資料表 **nyctaxi.trip**。 然後，我們使用 Spark 將資料匯總到 Spark 資料表 **nyctaxi.passengercountstats** 中。 現在，我們會將資料從 **nyctaxi.passengercountstats** 複製到名為 **SQLDB1.dbo.PassengerCountStats** 的專用 SQL 集區資料表。

在您的筆記本中執行下列儲存格。 其會將彙總的 Spark 資料表複製回專用 SQL 集區資料表。

```scala
%%spark
val df = spark.sql("SELECT * FROM nyctaxi.passengercountstats")
df.write.sqlanalytics("SQLDB1.dbo.PassengerCountStats", Constants.INTERNAL )
```

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [使用無伺服器 SQL 集區來分析資料](get-started-analyze-sql-on-demand.md)


