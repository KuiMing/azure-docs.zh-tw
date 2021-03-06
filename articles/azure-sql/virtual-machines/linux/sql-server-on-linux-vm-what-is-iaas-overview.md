---
title: 適用於 Linux 的 Azure 虛擬機器上的 SQL Server 概觀 | Microsoft Docs
description: 深入了解如何在適用於 Linux 的 Azure 虛擬機器上執行完整的 SQL Server 版本。 取得所有 Linux SQL Server VM 映像和相關內容的直接連結。
services: virtual-machines-linux
documentationcenter: ''
author: MashaMSFT
tags: azure-service-management
ms.service: virtual-machines-sql
ms.topic: overview
ms.workload: iaas-sql-server
ms.date: 04/10/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: be063105db2384f566e7c94d9f2e7a2bd808b15f
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/28/2020
ms.locfileid: "92790129"
---
# <a name="overview-of-sql-server-on-azure-virtual-machines-linux"></a>Azure 虛擬機器上的 SQL Server 概觀 (Linux)
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

> [!div class="op_single_selector"]
> * [Windows](../windows/sql-server-on-azure-vm-iaas-what-is-overview.md)
> * [Linux](sql-server-on-linux-vm-what-is-iaas-overview.md)

Azure 虛擬機器上的 SQL Server 可讓您在雲端使用完整的 SQL Server 版本，而不需要管理任何內部部署硬體。 當您使用隨用隨付方案時，SQL Server VM 也會簡化授權成本。

Azure 虛擬機器會在全球許多不同的[地理區域](https://azure.microsoft.com/regions/)中執行。 此外，也提供各種[機器大小](../../../virtual-machines/sizes.md)。 虛擬機器映像庫可讓您使用正確的版本、版次及作業系統建立 SQL Server VM。 這可讓虛擬機器成為許多不同 SQL Server 工作負載的適合選項。 

如果您不熟悉 Azure SQL，請參閱我們深入的 [Azure SQL 影片系列](https://channel9.msdn.com/Series/Azure-SQL-for-Beginners?WT.mc_id=azuresql4beg_azuresql-ch9-niner)中的「Azure VM 上的 SQL Server 概觀」影片：
> [!VIDEO https://channel9.msdn.com/Series/Azure-SQL-for-Beginners/SQL-Server-on-Azure-VM-Overview-4-of-61/player]

## <a name="get-started-with-sql-server-vms"></a><a id="create"></a>開始使用 SQL Server VM

若要開始使用，請選擇具有您所需版本、版次及作業系統的 SQL Server 虛擬機器映像。 下列各節提供 Azure 入口網站中 SQL Server 虛擬機器資源庫映像的直接連結。

> [!TIP]
> 如需如何了解 SQL 映像定價的詳細資訊，請參閱[執行 SQL Server 的 Linux VM 定價頁面](https://azure.microsoft.com/pricing/details/virtual-machines/linux/)。

| 版本 | 作業系統 | 版本 |
| --- | --- | --- |
| **SQL Server 2019** | Ubuntu 18.04 | [Enterprise](https://ms.portal.azure.com/#create/microsoftsqlserver.sql2019-ubuntu1804enterprise-ARM)、[Standard](https://ms.portal.azure.com/#create/microsoftsqlserver.sql2019-ubuntu1804standard-ARM)、[Web](https://ms.portal.azure.com/#create/microsoftsqlserver.sql2019-ubuntu1804web-ARM)、[Developer](https://ms.portal.azure.com/#create/microsoftsqlserver.sql2019-ubuntu1804sqldev-ARM) | 
| **SQL Server 2019** | Red Hat Enterprise Linux (RHEL) 8 | [Enterprise](https://ms.portal.azure.com/#create/microsoftsqlserver.sql2019-rhel8enterprise-ARM)、[Standard](https://ms.portal.azure.com/#create/microsoftsqlserver.sql2019-rhel8standard-ARM)、[Web](https://ms.portal.azure.com/#create/microsoftsqlserver.sql2019-rhel8web-ARM)、[Developer](https://ms.portal.azure.com/#create/microsoftsqlserver.sql2019-rhel8sqldev-ARM)|
| **SQL Server 2019** | SUSE Linux Enterprise Server (SLES) v12 SP5 | [Enterprise](https://ms.portal.azure.com/#create/microsoftsqlserver.sql2019-sles12sp5enterprise-ARM)、[Standard](https://ms.portal.azure.com/#create/microsoftsqlserver.sql2019-sles12sp5standard-ARM)、[Web](https://ms.portal.azure.com/#create/microsoftsqlserver.sql2019-sles12sp5web-ARM)、[Developer](https://ms.portal.azure.com/#create/microsoftsqlserver.sql2019-sles12sp5sqldev-ARM)|
| **SQL Server 2017** | Red Hat Enterprise Linux (RHEL) 7.4 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonRedHatEnterpriseLinux74)、[Standard](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonRedHatEnterpriseLinux74)、[Web](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonRedHatEnterpriseLinux74)、[Express](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonRedHatEnterpriseLinux74)、[Developer](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonRedHatEnterpriseLinux74) |
| **SQL Server 2017** | SUSE Linux Enterprise Server (SLES) v12 SP2 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonSLES12SP2)、[Standard](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonSLES12SP2)、[Web](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonSLES12SP2)、[Express](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonSLES12SP2)、[Developer](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonSLES12SP2) |
| **SQL Server 2017** | Ubuntu 16.04 LTS |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonUbuntuServer1604LTS)、[Standard](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonUbuntuServer1604LTS)、[Web](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonUbuntuServer1604LTS)、[Express](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonUbuntuServer1604LTS)、[Developer](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonUbuntuServer1604LTS) |

> [!NOTE]
> 若要查看適用於 Windows 的可用 SQL Server 虛擬機器映像，請參閱 [Azure 虛擬機器 (Windows) 上的 SQL Server 概觀](../windows/sql-server-on-azure-vm-iaas-what-is-overview.md)。

## <a name="installed-packages"></a><a id="packages"></a>已安裝的套件

當您設定 Linux 上的 SQL Server 時，可以根據您的需求安裝資料庫引擎套件，以及數個選用的套件。 SQL Server 的 Linux 虛擬機器映像會自動為您安裝大多數的套件。 下表顯示每個發佈會安裝哪些套件。

| 散發 | [資料庫引擎](/sql/linux/sql-server-linux-setup) | [工具](/sql/linux/sql-server-linux-setup-tools) | [SQL Server 代理程式](/sql/linux/sql-server-linux-setup-sql-agent) | [全文檢索搜尋](/sql/linux/sql-server-linux-setup-full-text-search) | [SSIS](/sql/linux/sql-server-linux-setup-ssis) | [HA 附加元件](/sql/linux/sql-server-linux-business-continuity-dr) |
|---|---|---|---|---|---|---|
| RHEL | ![RHEL 和資料庫引擎](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![RHEL 和工具](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![RHEL 和 SQL Server 代理程式](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![RHEL 和全文檢索搜尋](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![RHEL 和 SSIS](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![RHEL 和 HA 附加元件](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) |
| SLES | ![SLES 和資料庫引擎](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![SLES 和工具](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![SLES 和 SQL Server 代理程式](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![SLES 和全文檢索搜尋](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![SLES 和 SSIS](./media/sql-server-on-linux-vm-what-is-iaas-overview/no.png) | ![SLES 和 HA 附加元件](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png)|
| Ubuntu | ![Ubuntu 和資料庫引擎](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![Ubuntu 和工具](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![Ubuntu 和 SQL Server 代理程式](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![Ubuntu 和全文檢索搜尋](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![Ubuntu 和 SSIS](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![Ubuntu 和 HA 附加元件](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) |

## <a name="related-products-and-services"></a>相關產品與服務

### <a name="linux-virtual-machines"></a>Linux 虛擬機器

* [Azure 虛擬機器概觀](../../../virtual-machines/linux/overview.md)

### <a name="storage"></a>儲存體

* [Microsoft Azure 儲存體簡介](../../../storage/common/storage-introduction.md)

### <a name="networking"></a>網路功能

* [虛擬網路概觀](../../../virtual-network/virtual-networks-overview.md)
* [Azure 中的 IP 位址](../../../virtual-network/public-ip-addresses.md)
* [在 Azure 入口網站中建立完整格式的網域名稱](../../../virtual-machines/windows/portal-create-fqdn.md)

### <a name="sql"></a>SQL

* [Linux 上的 SQL Server 文件](/sql/linux) \(機器翻譯\)
* [Azure SQL Database 比較](../../azure-sql-iaas-vs-paas-what-is-overview.md)

## <a name="next-steps"></a>後續步驟

在 Linux 虛擬機器上開始使用 SQL Server：

* [在 Azure 入口網站中建立 SQL Server VM](sql-vm-create-portal-quickstart.md)

獲得有關 Linux 上 SQL Server VM 常見問題的答案：

* [Azure 虛擬機器上的 SQL Server 常見問題集](frequently-asked-questions-faq.md)