---
title: "智慧型偵測 - 效能異常 | Microsoft Docs"
description: "Application Insights 會主動分析您的應用程式遙測，並向您提出潛在問題的警告。 這項功能不需要進行任何設定。"
services: application-insights
documentationcenter: windows
author: antonfrMSFT
manager: douge
ms.assetid: 6acd41b9-fbf0-45b8-b83b-117e19062dd2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/31/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 63c901529b81c75f46f1b21219054817c148063a
ms.openlocfilehash: 1c46c40b09ca1923190d3c7109d25bd7525cb577


---
# <a name="smart-detection---performance-anomalies"></a>智慧型偵測 - 效能異常


[Application Insights](app-insights-overview.md) 會深入分析您的應用程式遙測，並且可以向您提出潛在效能問題的警告。 您之所以閱讀本文，可能是因為您收到我們透過電子郵件傳送的其中一個智慧型警示。

此功能不需要設定，而且會在您的應用程式產生足夠的遙測時自動啟動。

## <a name="what-is-smart-detection-of-performance-anomalies"></a>什麼是效能異常的智慧型偵測？
「智慧型偵測」會藉由分析您應用程式傳送給 Application Insights 的遙測，探索您應用程式中的效能異常模式。

特別是，它會尋找只會影響某些使用者，或只在某些情況下影響使用者的效能問題。

例如，若您的應用程式頁面在某一類型瀏覽器上的載入速度遠低於其他瀏覽器，或特定伺服器服務要求的速度較慢，它就會通知您。 它同時可以探索與屬性組合相關的問題，例如在某地區中的某個特定時段，頁面載入速度緩慢的問題。

這類異常狀況很難只藉由調查資料來偵測，但比您想像的更為常見。 通常只在您的客戶抱怨時才會浮出檯面。 但那時就太晚了：受影響的使用者已經轉而選擇您的對手！

目前，我們的演算法會查看頁面載入時間、伺服器上的要求回應時間，和相依性回應時間。  

您不需設定任何臨界值或設定規則。 機器學習服務和資料採礦演算法會用來偵測異常模式。

## <a name="about-the-smart-detection-alert"></a>關於智慧型偵測警示
* *我為什麼會收到這封電子郵件？*
  * 「智慧型偵測」分析了您應用程式傳送給 Application Insights 的遙測，並偵測到您的應用程式中有效能問題。
* *收到通知代表一定是有問題嗎？*
  * 不一定。 這只是一個建議，您可以仔細探究其中的問題。
* *我該怎麼辦？*
  * [查看提供的資料](#responding-to-an-alert)。 使用計量瀏覽器來檢閱一段時間的效能，並向下鑽研其他計量。 使用搜尋來篩選出可協助您識別根本原因的特定事件。
* *所以你們會看到我的資料嗎？*
  * 不會。 服務完全是自動的。 只有您會收到通知。 您的資料是 [不公開的](app-insights-data-retention-privacy.md)。

## <a name="the-detection-process"></a>偵測程序
* *偵測哪些種類的效能異常？*
  * 由您自行檢查會很耗時的模式。 例如，某種位置、時間與平台組合的效能不佳。
* *你們會分析 Application Insights 收集的所有資料嗎？*
  * 目前尚未。 我們目前會分析要求回應時間、相依性回應時間和頁面載入時間。 其他計量的分析功能即將推出。
* *我可以建立自己的異常偵測規則嗎？*

  * 尚未提供。 但是您可以：
  * [設定警示](app-insights-alerts.md)，使其在計量超出臨界值時通知您。
  * [匯出遙測](app-insights-export-telemetry.md)至[資料庫](app-insights-code-sample-export-sql-stream-analytics.md)或[至 PowerBI](app-insights-export-power-bi.md) 或[其他](app-insights-code-sample-export-telemetry-sql-database.md)工具以自行分析。
* *執行分析的頻率為何？*

  * 我們每天都會根據前一天的遙測執行分析。
* *那麼，這可以取代[計量警示](app-insights-alerts.md)嗎？*
  * 編號  我們不保證能偵測您可能認為異常的每項行為。

## <a name="how-to-investigate-the-issues-raised"></a>如何調查引發的問題
您可以從電子郵件或從異常清單開啟診斷報告。

![按一下電子郵件警示中的連結，可在 Azure 中開啟診斷報告](./media/app-insights-proactive-performance-diagnostics/03.png)

* **時間**顯示偵測到問題的時間。
* [對象] 說明：

  * 偵測到的問題；
  * 我們發現的事件集的特性顯示了問題行為。
* 表格會比較效能差的事件集和所有其他事件的平均行為。

按下連結以開啟 [計量瀏覽器]，搜尋相關報告、篩選緩慢執行的事件集的時間和屬性。

修改時間範圍和篩選器可探索遙測。

## <a name="how-can-i-improve-performance"></a>如何改善效能？
您可從自己的經驗得知，對網站使用者而言，回應緩慢和失敗是最大挫折之一。 因此，請務必解決問題。

### <a name="triage"></a>分級
首先，這很重要嗎？ 如果頁面的載入速度一直很慢，但是只有 1% 的網站台使用者必須查看該網頁，您或許有更重要的事項需要考慮。 另一方面，如果只有 1% 的使用者開啟該網頁，但它每次都擲回例外狀況，這可能就是值得調查的問題。

使用電子郵件中的影響敘述作為一般指南，但請留意該敘述並不是全部的詳情。 蒐集其他證據進行確認。

請考慮這個問題的參數。 如果與地理位置有關，請設定包括該地區的 [可用性測試](app-insights-monitor-web-app-availability.md) ：該地區可能只有網路問題。

### <a name="diagnose-slow-page-loads"></a>診斷頁面載入緩慢
問題出在哪裡？ 伺服器是否回應太慢、頁面是否很長，或瀏覽器必須執行很多工作才能顯示頁面？

開啟 [瀏覽器] 計量刀鋒視窗。 分段顯示的瀏覽器頁面載入時間可顯示時間的進度。 

* 如果 [傳送要求時間]  太久，不是伺服器回應速度緩慢，就是要求是含有大量資料的文章。 查看 [效能計量](app-insights-web-monitor-performance.md#metrics) 以調查回應時間。
* 設定 [相依性追蹤](app-insights-asp-net-dependencies.md) 以查看速度慢是否是外部服務或您的資料庫所造成。
* 如果 [接收回應]  是主導因素，您的頁面和其相依組件 (JavaScript、CSS 及影像等，而非以非同步方式載入的資料) 會很長。 設定 [可用性測試](app-insights-monitor-web-app-availability.md)，而且務必設定載入相依組件的選項。 當您取得一些結果時，請開啟結果的詳細資料並將它展開，以查看不同檔案的載入時間。
* [用戶端處理時間]  過長表示指令碼執行速度很慢。 如果原因不明顯，請考慮加入一些時間計時程式碼並在 trackMetric 呼叫中傳送時間。

### <a name="improve-slow-pages"></a>改善慢速網頁
Web 上有改善您的伺服器回應和頁面載入時間的完整建議，因此我們不會嘗試這次重複說明。 以下是您可能已知道的一些祕訣，這只是為提醒您：

* 由大型檔案造成的緩慢載入：以非同步方式載入指令碼和其他組件。 使用指令碼統合。 將主頁面分成可個別載入其資料的 Widget。 不要對長資料表傳送純舊式 HTML：使用指令碼要求 JSON 或其他壓縮格式的資料，然後就地填滿資料表。 有一些絕佳的架構可協助進行這一切。 (當然，也必須承擔大型指令碼)。
* 降低伺服器相依性：考慮您的元件的地理位置。 比方說，如果您使用 Azure，請確定 Web 伺服器和資料庫位於相同的區域中。 查詢是否會擷取超過所需的資訊？ 快取或批次處理是否有所幫助？
* 容量問題：查看回應時間和要求計數的伺服器計量。 如果回應時間尖峰與要求計數尖峰不成比例，有可能是您的伺服器已被過度使用。

## <a name="notification-emails"></a>通知電子郵件
* *我必須訂閱這項服務才能收到通知嗎？*
  * 不一定。 我們的 bot 會定期調查所有 Application Insights 使用者的資料，如果偵測到問題，就會傳送通知。
* *我是否可以取消訂閱或改為傳送通知給我的同事？*

  * 按一下警示或電子郵件中的取消訂閱連結。

    它們目前會傳送給擁有 [Application Insights 資源寫入權限](app-insights-resources-roles-access-control.md)的人員。

    您也可以在 [智慧型偵測] 刀鋒視窗的 [設定] 中編輯收件者清單。
* *我不想要被這些訊息淹沒。*
  * 系統限制為每天一封，內含我們尚未報告的最相關問題。 您不會重複收到任何訊息。
* *如果我沒有做任何動作，會收到提醒嗎？*
  * 不會，每個問題您只會收到一次訊息。
* *我遺失了電子郵件。在入口網站中哪裡可以找到通知？*
  * 在應用程式的 Application Insights 概觀中，按一下 [智慧型偵測] 磚。 您可以在這裡找到最多 7 天前的所有通知。

## <a name="next-steps"></a>後續步驟
這些診斷工具可協助您檢查來自您的應用程式的遙測︰

* [計量瀏覽器](app-insights-metrics-explorer.md)
* [搜尋總管](app-insights-diagnostic-search.md)
* [分析 - 功能強大的查詢語言](app-insights-analytics-tour.md)

智慧型偵測是完全自動的。 但是，或許您會想要再設定一些警示？

* [手動設定的度量警示](app-insights-alerts.md)
* [可用性 Web 測試](app-insights-monitor-web-app-availability.md)



<!--HONumber=Nov16_HO3-->


