---
title: "Azure 服務匯流排常見問題集 (FAQ) | Microsoft Docs"
description: "回答一些有關 Azure 服務匯流排的常見問題。"
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: cc75786d-3448-4f79-9fec-eef56c0027ba
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/09/2017
ms.author: sethm;jotaub
translationtype: Human Translation
ms.sourcegitcommit: da7f8e3e61705cf07ff65c9dd1d8f292f4fb9f62
ms.openlocfilehash: 9061829e42ed5563d64860774aa7d80f2ab011bd
ms.lasthandoff: 02/10/2017


---
# <a name="service-bus-faq"></a>服務匯流排常見問題集
本文提供 Microsoft Azure 服務匯流排的一些常見問題解答。 您也可以造訪 [Azure 支援常見問題集](http://go.microsoft.com/fwlink/?LinkID=185083) ，以取得一般的 Azure 價格和支援資訊。

## <a name="general-questions-about-azure-service-bus"></a>關於 Azure 服務匯流排的一般問題
### <a name="what-is-azure-service-bus"></a>什麼是 Azure 服務匯流排？
[Azure 服務匯流排](service-bus-messaging-overview.md)是非同步的訊息雲端平台，可讓您在彼此分離的系統之間傳送資料。 Microsoft 以服務的形式提供此功能，這表示您不需要裝載任何自有硬體就能使用。

### <a name="what-is-a-service-bus-namespace"></a>什麼是服務匯流排命名空間？
[命名空間](service-bus-create-namespace-portal.md)提供範圍容器，可在應用程式內定址服務匯流排資源。 必須建立命名空間才能使用服務匯流排，而且這也是開始使用的第一個步驟。

### <a name="what-is-an-azure-service-bus-queue"></a>什麼是 Azure 服務匯流排佇列？
[服務匯流排佇列](service-bus-queues-topics-subscriptions.md)是訊息儲存所在的實體。 如果您有多個應用程式，或多個需要彼此通訊的分散式應用程式部分，佇列會特別有用。 佇列和配送中心的類似之處在於，兩者都會接收多個產品 (訊息)，再從該處送出。

### <a name="what-are-azure-service-bus-topics-and-subscriptions"></a>什麼是 Azure 服務匯流排主題和訂用帳戶？
主題可視覺化為佇列，而且在使用多個訂用帳戶時，主題會變成更豐富的訊息模型；基本上是一對多的通訊工具。 此發佈/訂閱模型 (或「pub/sub」) 可讓應用程式將訊息傳送至具有多個訂用帳戶的主題，以便讓多個應用程式接收該訊息。

### <a name="what-is-a-partitioned-entity"></a>什麼是分割的實體？
傳統的佇列或主題由單一訊息代理程式處理並儲存在一個訊息存放區中。 [分割的佇列或主題](service-bus-partitioning.md)會由多個訊息代理程式處理，並儲存在多個訊息存放區。 這表示分割佇列或主題的整體輸送量不會再受到單一訊息代理程式或訊息存放區的效能所限制。 此外，即使訊息存放區暫時中斷也不會讓分割的佇列或主題無法使用。

請注意，使用分割實體時無法確保順序。 若分割無法使用，您仍可從其他分割傳送及接收訊息。

## <a name="best-practices"></a>最佳作法
### <a name="what-are-some-azure-service-bus-best-practices"></a>Azure 服務匯流排的最佳做法有哪些？
* [使用服務匯流排的效能改進最佳作法][Best practices for performance improvements using Service Bus] - 這篇文章說明如何在交換訊息時將效能最佳化。

### <a name="what-should-i-know-before-creating-entities"></a>建立實體前的須知事項為何？
佇列和主題的下列屬性是不可變的。 在佈建實體時請將這一點納入考量，因為若要修改屬性，就必須建立新的替代實體。

* 大小
* 分割
* 工作階段
* 重複偵測
* 快速實體

## <a name="pricing"></a>價格
本節提供服務匯流排價格結構的一些常見問題解答。 您也可以造訪 [Azure 支援常見問題集](http://go.microsoft.com/fwlink/?LinkID=185083) ，以取得一般 Microsoft Azure 價格資訊。 如需服務匯流排價格的完整資訊，請參閱 [服務匯流排價格詳細資料](https://azure.microsoft.com/pricing/details/service-bus/)。

### <a name="how-do-you-charge-for-service-bus"></a>服務匯流排的收費方式為何？
如需服務匯流排價格的完整資訊，請參閱[服務匯流排價格詳細資料][Pricing overview]。 除了註明的價格，您還需支付您的應用程式佈建所在資料中心外部的輸出相關資料傳輸費用。

### <a name="what-usage-of-service-bus-is-subject-to-data-transfer-what-is-not"></a>何種服務匯流排用法需支付資料傳輸費用？ 何種不需？
指定的 Azure 區域內的任何資料傳輸都是免費提供，以及任何輸入的資料傳輸。 區域外部的資料傳送需要出口流量費用，可以在[這裡](https://azure.microsoft.com/pricing/details/bandwidth/)找到。

### <a name="does-service-bus-charge-for-storage"></a>服務匯流排是否會收取儲存體費用？
不會，服務匯流排不會收取儲存體費用。 不過，有配額會限制每個佇列/主題可保存的資料數量上限。 請參閱下一個常見問題。

## <a name="quotas"></a>配額
如需服務匯流排的限制和配額清單，請參閱[配額概觀][Quotas overview]。

### <a name="does-service-bus-have-any-usage-quotas"></a>服務匯流排是否有任何使用量配額？
根據預設，對於所有雲端服務，Microsoft 會設定針對所有客戶的訂用帳戶計算的彙總每月使用量配額。 因為我們了解您的需求可能超過這些限制，請隨時連絡客戶服務部門，讓我們能夠了解您的需求並適當地調整這些限制。 針對服務匯流排，整體使用量配額會為每個月 5 億個訊息。

雖然我們有權停用在指定的月份內超出其使用量配額的客戶帳戶，但我們將提供電子郵件通知並且在採取任何動作之前多次嘗試連絡客戶。 超出這些配額的客戶仍需負責支付超出配額的費用。

如同 Azure 上的其他服務，服務匯流排會強制執行一組特定的配額，以確保公平的資源使用量。 以下是此服務會強制執行的使用量配額：

#### <a name="queuetopic-size"></a>佇列/主題大小
您會在建立佇列或主題時指定佇列或主題大小上限。 此配額的值可以是 1、2、3、4 或 5 GB。 如果達到大小上限，其他內送訊息將會遭到拒絕，而且呼叫端程式碼將會收到例外狀況。

#### <a name="naming-restrictions"></a>命名限制
服務匯流排命名空間的名稱長度只能介於 6 到 50 個字元之間。 對於每個佇列、主題或訂用帳戶，字元數限制為 1 到 50 個字元之間。

#### <a name="number-of-concurrent-connections"></a>並行連線數目
佇列/主題/訂用帳戶 -佇列/主題/訂用帳戶上並行 TCP 連線數目的限制為 100。 如果達到這個配額，其他連接的後續要求將會遭到拒絕，而且呼叫端程式碼將會收到例外狀況。 對於每個訊息工廠而言，如果該訊息工廠所建立的任何用戶端有作用中的作業擱置，或在少於 60 秒之前完成作業，則服務匯流排會維持一個 TCP 連線。 REST 作業不會計入並行 TCP 連線內。

#### <a name="number-of-topicsqueues-per-service-namespace"></a>每個服務命名空間的主題/佇列數目
服務命名空間上的主題/佇列 (持久型儲存體支援的實體) 數目上限為 10,000。 如果達到此配額，後續要求在服務命名空間上建立新主題/佇列將會遭到拒絕。 在此情況下，視建立嘗試是透過入口網站或在用戶端程式碼中完成而定，Azure 傳統入口網站將會顯示一則錯誤訊息，或呼叫端程式碼將會收到例外狀況。

### <a name="message-size-quotas"></a>訊息大小配額
#### <a name="queuetopicsubscription"></a>佇列/主題/訂用帳戶
**訊息大小** – 每則訊息的大小總計限制為 256KB，包含訊息標頭。

**訊息標頭大小** – 每個訊息標頭的限制為 64 KB。

超出這些大小配額的訊息將會遭到拒絕，而且呼叫端程式碼將會收到例外狀況。

**每個主題的訂用帳戶數目** – 每個主題的訂用帳戶數目上限為 2,000。 如果達到此配額，將會拒絕建立主題的其他訂用帳戶的後續要求。 在此情況下，視建立嘗試是透過入口網站或在用戶端程式碼中完成而定，Azure 傳統入口網站將會顯示一則錯誤訊息，或呼叫端程式碼將會收到例外狀況。

**每個主題的 SQL 篩選器數目** – 每個主題的 SQL 篩選器數目上限為 2,000。 如果達到這個配額，則在主題上建立其他篩選器的後續要求都會遭到拒絕，而且呼叫端程式碼將會收到例外狀況。

**每個主題的相互關聯篩選器數目** – 每個主題的相互關聯篩選器數目上限為 100,000。 如果達到這個配額，則在主題上建立其他篩選器的後續要求都會遭到拒絕，而且呼叫端程式碼將會收到例外狀況。

## <a name="troubleshooting"></a>疑難排解
### <a name="what-are-some-of-the-exceptions-generated-by-azure-service-bus-apis-and-their-suggested-actions"></a>Azure 服務匯流排 API 所產生的例外狀況有哪些，其建議的動作為何？
如需可能的服務匯流排例外狀況，請參閱[例外狀況概觀][Exceptions overview]。

### <a name="what-is-a-shared-access-signature-and-which-languages-support-generating-a-signature"></a>什麼是共用存取簽章，何種語言可支援產生簽章？
共用存取簽章是以 SHA-256 安全雜湊或 URI 為基礎的驗證機制。 如需如何以 Node、PHP、Java 和 C\# 產生自有簽章的相關資訊，請參閱[共用存取簽章][Shared Access Signatures]一文。

## <a name="subscription-and-namespace-management"></a>訂用帳戶和命名空間管理
### <a name="how-do-i-migrate-a-namespace-to-another-azure-subscription"></a>如何將命名空間移轉到另一個 Azure 訂用帳戶？
使用 Azure 入口網站，您可以依下列指示[這裡](../azure-resource-manager/resource-group-move-resources.md#use-portal)，將服務匯流排命名空間移轉到另一個訂用帳戶。 如果您想要使用 PowerShell，請遵循下列指示︰ 

下列的命令順序會從一個 Azure 訂用帳戶的命名空間移到另一個。 若要執行這項作業，命名空間必須已經是作用中，而且執行 PowerShell 命令的使用者必須是來源與目標訂用帳戶的系統管理員。

```powershell
# Create a new resource group in target subscription
Select-AzureRmSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzureRmResourceGroup -Name 'targetRG' -Location 'East US'

# Move namespace from source subscription to target subscription
Select-AzureRmSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzureRmResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzureRmResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```

## <a name="next-steps"></a>後續步驟
若要深入了解服務匯流排，請參閱下列主題。

* [Azure 服務匯流排進階簡介 (部落格文章)](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
* [Azure 服務匯流排進階簡介 (Channel9)](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
* [服務匯流排概觀](service-bus-messaging-overview.md)
* [Azure 服務匯流排架構概觀](service-bus-fundamentals-hybrid-solutions.md)
* [開始使用服務匯流排佇列](service-bus-dotnet-get-started-with-queues.md)

[Best practices for performance improvements using Service Bus]: service-bus-performance-improvements.md
[Best practices for insulating applications against Service Bus outages and disasters]: service-bus-outages-disasters.md
[Pricing overview]: https://azure.microsoft.com/pricing/details/service-bus/
[Quotas overview]: service-bus-quotas.md
[Exceptions overview]: service-bus-messaging-exceptions.md
[Shared Access Signatures]: service-bus-sas-overview.md

