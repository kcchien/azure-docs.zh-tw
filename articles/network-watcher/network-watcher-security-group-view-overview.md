---
title: "Azure 網路監看員中的安全性群組檢視簡介 | Microsoft Docs"
description: "本頁提供網路監看員安全性檢視功能的概觀"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: ad27ab85-9d84-4759-b2b9-e861ef8ea8d8
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 2f03ba60d81e97c7da9a9fe61ecd419096248763
ms.openlocfilehash: de48da2c77c65f3e806474ae37ffd00772622b02
ms.lasthandoff: 03/04/2017

---

# <a name="introduction-to-network-security-group-view-in-azure-network-watcher"></a>Azure 網路監看員中的網路安全性群組檢視簡介

網路安全性群組可以在子網路層級或 NIC 層級產生關聯。 若在子網路層級產生關聯，它會套用至子網路中的所有 VM 執行個體。 網路安全性群組檢視會傳回在 NIC 和子網路層級產生關聯的所有已設定 NSG 和規則。 此外，VM 中的每個 NIC 也會傳回有效的安全性規則。 使用 [網路安全性群組] 檢視，您就可以評估 VM 的網路弱點，例如開啟的連接埠。 您也可以根據所設定的安全性規則和有效規則之間的比較，驗證網路安全性群組是否如預期般運作。

更大範圍的使用案例是在安全性合規性與稽核時。 您可以定義一組規定的安全性規則，以做為控管組織安全性的模型。 藉由比較網路中每個 VM 的規定規則和有效規則，即可以程式設計方式實作定期合規性稽核。

在入口網站中，規則分為「有效」、「子網路」、「網路介面」和「預設」。 這可讓您簡潔地了解套用至虛擬機器的規則。 檢視畫面中提供了 [下載] 按鈕，以方便您輕鬆地下載所有安全性規則 (不論其標籤為何) 到 CSV 檔案中。

![安全性群組檢視][1]

您可以選取規則，隨即會開啟新的刀鋒視窗，顯示網路安全性群組和來源與目的地前置詞。 從這個刀鋒視窗中，您可以直接瀏覽至網路安全性群組資源。

![向下鑽研][2]

### <a name="next-steps"></a>後續步驟

瀏覽[使用 PowerShell 稽核網路安全性群組設定](network-watcher-nsg-auditing-powershell.md)，以了解如何稽核網路安全性群組設定

[1]: ./media/network-watcher-security-group-view-overview/securitygroupview.png
[2]: ./media/network-watcher-security-group-view-overview/figure1.png










