---
title: "Azure Data Lake Store 常見問題集 | Microsoft Docs"
description: "針對 Azure Data Lake Store 的問題進行疑難排解或加以減緩的指導方針"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: bf7fd555-3e30-43ce-b28c-c3ad0a241fdb
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/06/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: a3629845014cb401df96d2d8bf7b9801a0664150
ms.openlocfilehash: 2f184f5289b9394572023fe9d1aec2d28a73c4f7


---
# <a name="frequently-asked-questions-for-azure-data-lake-store"></a>Azure Data Lake Store 常見問題集
在本文中，您將了解 Azure Data Lake Store 的相關常見問題集。

## <a name="how-can-i-further-protect-my-data-from-region-wide-disasters-or-accidental-deletions"></a>如何進一步保護我的資料以防止區域全面性災害或意外刪除？
透過自動化複本，Azure Data Lake Store 帳戶中的資料可彈性應對區域內發生的暫時性硬體故障。 這可確保持久性和高可用性，符合 Azure Data Lake Store 的 SLA。 以下提供了一些指引，說明如何進一步保護資料，使其不受罕見的全區停電或意外刪除事件影響。

### <a name="disaster-recovery-guidance"></a>災害復原指引
每位客戶備妥自己的災害復原計畫是相當重要的。 請參閱下面的 Azure 文件，以建置災害復原計畫。 這裡有一些資源可協助您建立自己的計畫。

* [Azure 應用程式的災害復原和高可用性](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)
* [Azure 復原技術指導](../resiliency/resiliency-technical-guidance.md)

#### <a name="best-practices"></a>最佳作法
我們建議您以符合災害復原計畫需求的頻率，將重要資料複製到位於其他區域的另一個 Data Lake Store 帳戶。 您可以使用各種方法來複製資料，包括 [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md)、[Azure PowerShell](data-lake-store-get-started-powershell.md) 或 [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md)。 Azure Data Factory 這項服務很適合用來反覆建立和部署資料移動管線。

發生區域性停電時，您可以立即從資料所複製到的區域存取資料。您可以監視 [Azure 服務健康狀況儀表板](https://azure.microsoft.com/status/)以判斷全球各地 Azure 服務的狀態。

### <a name="data-corruption-or-accidental-deletion-recovery-guidance"></a>資料損毀或意外刪除復原指引
雖然 Azure Data Lake Store 可透過自動化複本提供資料恢復，但這無法防止應用程式 (或開發人員/使用者) 的資料遭到損毀或意外刪除。

#### <a name="best-practices"></a>最佳作法
若要防止意外刪除，建議您先為 Data Lake Store 帳戶設定正確的存取原則。  這包括套用 [Azure 資源鎖定](../azure-resource-manager/resource-group-lock-resources.md)來鎖定重要資源，以及套用採用可用 [Data Lake Store 安全性功能](data-lake-store-security-overview.md)的帳戶和檔案層級存取控制。 另外，也建議您使用 [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md)、[Azure PowerShell](data-lake-store-get-started-powershell.md) 或 [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md)，在另一個 Data Lake Store 帳戶、資料夾或 Azure 訂用帳戶定期建立重要資料的複本。  這可用來從資料損毀或刪除事件中復原。 Azure Data Factory 這項服務很適合用來反覆建立和部署資料移動管線。

組織也可以啟用 Azure Data Lake Store 帳戶的[診斷記錄](data-lake-store-diagnostic-logs.md)，收集資料存取稽核線索來提供刪除或更新檔案之可疑人員的相關資訊。

## <a name="next-steps"></a>後續步驟
* [開始使用 Azure Data Lake Store](data-lake-store-get-started-portal.md)
* [保護 Data Lake Store 中的資料](data-lake-store-secure-data.md)




<!--HONumber=Feb17_HO2-->


