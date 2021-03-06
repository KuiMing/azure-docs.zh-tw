---
title: 連接架構-適用於 PostgreSQL 的 Azure 資料庫-單一伺服器
description: 描述適用於 PostgreSQL 的 Azure 資料庫單一伺服器的連接架構。
author: mksuni
ms.author: sumuth
ms.service: postgresql
ms.topic: conceptual
ms.date: 05/23/2019
ms.openlocfilehash: a6e2bc93a589e0a3f709eb1a8956bf8ca3d8bf6b
ms.sourcegitcommit: 80034a1819072f45c1772940953fef06d92fefc8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/03/2020
ms.locfileid: "93242053"
---
# <a name="connectivity-architecture-in-azure-database-for-postgresql"></a>適用於 PostgreSQL 的 Azure 資料庫中的連線架構
本文說明適用於 PostgreSQL 的 Azure 資料庫連線架構，以及如何將流量導向至 Azure 內部和外部用戶端的適用於 PostgreSQL 的 Azure 資料庫資料庫實例。

## <a name="connectivity-architecture"></a>連線架構
您適用於 PostgreSQL 的 Azure 資料庫的連線是透過負責將連入連線路由至叢集中伺服器實體位置的閘道所建立。 下圖說明流量的流程。

:::image type="content" source="./media/concepts-connectivity-architecture/connectivity-architecture-overview-proxy.png" alt-text="連接架構的總覽":::

當用戶端連接到資料庫時，會取得連接到閘道的連接字串。 此閘道具有可接聽埠5432的公用 IP 位址。 在資料庫叢集中，會將流量轉送到適當的適用於 PostgreSQL 的 Azure 資料庫。 因此，若要連線到您的伺服器（例如從公司網路），則必須開啟用戶端防火牆，以允許連出流量抵達我們的閘道。 您可以在下方找到每個區域的閘道所使用的 IP 位址完整清單。

## <a name="azure-database-for-postgresql-gateway-ip-addresses"></a>適用於 PostgreSQL 的 Azure 資料庫閘道 IP 位址
下表列出所有資料區域適用於 PostgreSQL 的 Azure 資料庫閘道的主要和次要 Ip。 主要 IP 位址是閘道目前的 IP 位址，而第二個 IP 位址是容錯移轉的 IP 位址（如果主要複本失敗）。 如前所述，客戶應該允許輸出到這兩個 IP 位址。 第二個 IP 位址不會在任何服務上接聽，直到適用於 PostgreSQL 的 Azure 資料庫啟用以接受連接為止。

| **區域名稱** | **閘道 IP 位址** |
|:----------------|:-------------|
| 澳大利亞中部| 20.36.105.0     |
| 澳大利亞 Central2     | 20.36.113.0   |
| 澳大利亞東部 | 13.75.149.87, 40.79.161.1     |
| 澳大利亞東南部 |191.239.192.109, 13.73.109.251   |
| 巴西南部 | 104.41.11.5, 191.233.201.8, 191.233.200.16  |
| 加拿大中部 |40.85.224.249  |
| 加拿大東部 | 40.86.226.166    |
| 美國中部 | 23.99.160.139, 13.67.215.62, 52.182.136.37, 52.182.136.38     |
| 中國東部 | 139.219.130.35    |
| 中國東部 2 | 40.73.82.1  |
| 中國北部 | 139.219.15.17    |
| 中國北部 2 | 40.73.50.0     |
| 東亞 | 191.234.2.139, 52.175.33.150, 13.75.33.20, 13.75.33.21     |
| 美國東部 | 40.121.158.30, 191.238.6.43, 40.71.8.203, 40.71.83.113   |
| 美國東部 2 |40.79.84.180, 191.239.224.107, 52.177.185.181, 40.70.144.38, 52.167.105.38  |
| 法國中部 | 40.79.137.0, 40.79.129.1  |
| 法國南部 | 40.79.177.0     |
| 德國中部 | 51.4.144.100     |
| 德國東北部 | 51.5.144.179  |
| 印度中部 | 104.211.96.159     |
| 印度南部 | 104.211.224.146  |
| 印度西部 | 104.211.160.80    |
| 日本東部 | 13.78.61.196, 191.237.240.43  |
| 日本西部 | 104.214.148.156, 191.238.68.11, 40.74.96.6, 40.74.96.7    |
| 南韓中部 | 52.231.32.42   |
| 南韓南部 | 52.231.200.86    |
| 美國中北部 | 23.96.178.199, 23.98.55.75, 52.162.104.35, 52.162.104.36    |
| 歐洲北部 | 40.113.93.91, 191.235.193.75, 52.138.224.6, 52.138.224.7    |
| 南非北部  | 102.133.152.0    |
| 南非西部 | 102.133.24.0   |
| 美國中南部 |13.66.62.124, 23.98.162.75, 104.214.16.39, 20.45.120.0   |
| 東南亞 | 104.43.15.0, 23.100.117.95, 40.78.233.2, 23.98.80.12     |
| 阿拉伯聯合大公國中部 | 20.37.72.64  |
| 阿拉伯聯合大公國北部 | 65.52.248.0    |
| 英國南部 | 51.140.184.11   |
| 英國西部 | 51.141.8.11  |
| 美國中西部 | 13.78.145.25     |
| 西歐 | 40.68.37.158, 191.237.232.75, 13.69.105.208, 104.40.169.187  |
| 美國西部 | 104.42.238.205, 23.99.34.75, 13.86.216.212, 13.86.217.212 |
| 美國西部 2 | 13.66.226.202  |
||||

## <a name="next-steps"></a>下一步

* [使用 Azure 入口網站建立和管理適用於 PostgreSQL 的 Azure 資料庫防火牆規則](./howto-manage-firewall-using-portal.md)
* [使用 Azure CLI 建立和管理適用於 PostgreSQL 的 Azure 資料庫防火牆規則](./howto-manage-firewall-using-cli.md)
