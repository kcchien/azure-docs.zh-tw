---
title: "轉換成 Azure Machine Learning 中的預測性實驗 | Microsoft Docs"
description: "如何將用於預測性分析模型的機器學習服務訓練實驗轉換成可以當做 Web 服務部署的預測實驗。"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: eb943c45-541a-401d-844a-c3337de82da6
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye
translationtype: Human Translation
ms.sourcegitcommit: 6d749e5182fbab04adc32521303095dab199d129
ms.openlocfilehash: db91a464843a7c2dc5460f12f7f306972d3a7da8
ms.lasthandoff: 03/22/2017


---
# <a name="convert-a-machine-learning-training-experiment-to-a-predictive-experiment"></a>將機器學習服務訓練實驗轉換成預測實驗
Azure Machine Learning 可讓您建置、測試以及部署預測性分析解決方案。

一旦您建立並反覆測試「訓練實驗」  以訓練您的預測性分析模型，並做好使用該模型為新資料評分的準備之後，您必須準備並簡化用於評分的實驗。 接著，您可以將此「預測實驗」實際運作為 Azure Web 服務，讓使用者可將資料傳送至您的模型，並接收您模型的預測。

轉換為預測實驗之後，您就準備好定型模型，可以當做 Web 服務部署。 Web 服務的使用者會將輸入資料傳送到您的模型，然後您的模型就會傳送回預測結果。 因此，當您轉換為預測實驗時，您必須記住您預期其他人使用您模型的方式。

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

將訓練實驗轉換為預測實驗包含三個步驟：

1. 儲存您已訓練過的機器學習模型，然後將機器學習演算法和[定型模型][train-model]模組替換為您所儲存的定型模型。
2. 將訓練調整為僅適用於評分所需的模組。 訓練實驗包含訓練所需，但一旦模型受過訓練並準備好用於評分之後，就不再需要的多個模組。
3. 定義實驗中您將接受來自 Web 服務使用者之資料的所在位置，以及將傳回的資料。

## <a name="set-up-web-service-button"></a>設定 Web 服務按鈕
執行您的實驗之後 (實驗畫布底部的 [執行] 按鈕)，[設定 Web 服務] 按鈕 (選取 [預測 Web 服務] 選項) 會為您執行三個步驟將訓練實驗轉換成預測實驗：

1. 它會將定型模型當做模組，儲存在模組調色盤的 [定型模型] 區段 (實驗畫布的左側)，然後將機器學習演算法和[定型模型][train-model]模組替換為所儲存的定型模型。
2. 它會移除顯然不需要的模組。 在我們的範例中，這包括[分割資料][split]、第 2 個<sup></sup>[評分模型][score-model]以及[評估模型][evaluate-model]模組。
3. 它會建立 Web 服務輸入和輸出模組，並將它們加入到實驗的預設位置中。

例如，以下實驗會使用範例人口普查資料，訓練二元推進式決策樹模型：

![訓練實驗][figure1]

這項實驗中的模組基本上執行是四個不同的功能：

![模組功能][figure2]

當您將這個訓練實驗轉換為預測實驗時，就不再需要其中部分模組或它們有不同的用途：

* **資料** - 在評分期間，不會使用此範例資料集中的資料，因為 Web 服務的使用者會提供要評分的資料。 不過，定型模型會使用此資料集中的中繼資料，例如，資料類型。 因此，您必須將資料集保留在預測實驗中，讓它能夠提供這項中繼資料。
* **準備** - 根據要提交用於評分的資料而定，這些模組不一定需要處理傳入的資料。
  
    例如，在此範例中，範例資料集可能會有遺漏的值，而且它包含培訓模型不需要的資料行。 因此，範例中包含[清除遺漏的資料][clean-missing-data]模組來處理遺漏的值，並包含[選取資料集中的資料行][select-columns]模組以將那些額外的資料行從資料流程中排除。 如果您知道透過 Web 服務提交用於評分的資料不會有遺漏的值，則您可以移除[清除遺漏的資料][clean-missing-data]模組。 不過，由於[選取資料集中的資料行][select-columns]模組有助於定義一組要評分的功能，因此必須保留此模組。
* **訓練** - 這些模組可用來訓練模型。 當您按一下 [設定 Web 服務] 時，這些模組都會由單一定型模型模組來加以取代。 這個新模組會儲存在模組調色盤的 [定型模型] 區段。
* **評分** - 在此範例中，[分割資料][split]模組是用來將資料流分割成一組測試資料和訓練資料。 在預測實驗中不需要這個模組，因此可以移除。 同樣地，第 2 個<sup></sup>[評分模型][score-model]模組和[評估模型][evaluate-model]模組會用來比較測試資料的結果，因此在預測實驗中也不需要這些模組。 但其餘的[評分模型][score-model]模組需要透過 Web 服務傳回評分結果。

以下是按一下 [設定 Web 服務] 之後，我們的範例外觀：

![已轉換的預測實驗][figure3]

這可能足以準備您的實驗，以當做 Web 服務部署。 不過，您可能會想要執行一些您的實驗專屬的額外工作。

### <a name="adjust-input-and-output-modules"></a>調整輸入和輸出模組
在訓練實驗中，您使用一組訓練資料進行一些處理，以取得 Machine Learning 演算法所需格式的資料。 如果您預期透過 Web 服務收到的資料不會需要這項處理，可以將 **Web 服務輸入模組** 移到實驗中的另一個節點 (按一下 **Web 服務輸入模組**的輸出，然後將其拖曳到適當模組的輸入連接埠)。

例如，**設定 Web 服務**預設會將 **Web 服務輸入**模組放在資料流程的頂端，如上圖所示。 不過，如果輸入資料不需要這項處理，則您可以手動將 **Web 服務輸入** 放在資料處理模組之後：

![移動 Web 服務輸入][figure4]

透過 Web 服務提供的輸入資料現在會直接傳入 [評分模型] 模組，而不需要任何前置處理。

同樣地， **設定 Web 服務** 會將 Web 服務輸出模組放在資料流程的底部。 在此範例中，Web 服務會將[評分模型][score-model]模組的輸出傳回給使用者，其中包括完整的輸入資料向量以及評分結果。

不過，如果您偏好傳回其他內容，則可以在 **Web 服務輸出**模組之前新增其他模組。 例如，如果您只想傳回評分結果，而不傳回整個輸入資料向量，則您可以新增[選取資料集中的資料行][select-columns]模組，以排除評分結果以外的所有資料行。 接著，您要將 **Web 服務輸出**模組移到[選取資料集中的資料行][select-columns]模組的輸出。 然後，實驗會看起來像這樣︰

![移動 Web 服務輸出][figure5]

### <a name="add-or-remove-additional-data-processing-modules"></a>新增或移除其他資料處理模組
如果在您的實驗中還有您知道在評分期間不需要的模組，則可以移除這些模組。 例如，由於我們已經將 **Web 服務輸入**模組移到資料處理模組之後的某個點，因此我們可以從預測實驗中移除[清除遺漏的資料][clean-missing-data]模組。

我們的預測實驗現在看起來像這樣：

![移除額外的模組][figure6]

> [!TIP]
> 請注意，我們不會移除資料集或[選取資料集中的資料行][select-columns]模組。 Web 服務中的模型不會使用原始資料集中的資料，但它會使用資料集中定義的中繼資料，例如每個資料行的資料類型。 同樣地，[選取資料集中的資料行][select-columns]模組會識別模型將會使用哪幾行的資料。 因此，這兩個模組都需要保留在預測性實驗中。

### <a name="add-optional-web-service-parameters"></a>新增選用的 Web 服務參數
在某些情況下，您可能需要讓 Web 服務的使用者變更存取服務時的模組行為。 *Web 服務參數* 可讓您執行這項操作。

常見的範例是設定[匯入資料][import-data]模組，讓已部署之 Web 服務的使用者可以在存取 Web 服務時，指定不同的資料來源。 或者，設定[匯出資料][export-data]模組，以便能夠指定不同的目的地。

您可以定義 Web 服務參數，並使其與一個或多個模組參數產生關聯，而且您可以指定它們是必要還是選用參數。 接著，Web 服務的使用者可以在服務受到存取時，提供這些參數的值，並據此修改模組動作。

如需關於 Web 服務參數的詳細資訊，請參閱[使用 Azure Machine Learning Web 服務參數][webserviceparameters]。

[webserviceparameters]: machine-learning-web-service-parameters.md


## <a name="deploy-the-predictive-experiment-as-a-web-service"></a>將預測實驗部署為 Web 服務
既然已經為預測實驗做好充分的準備，您可以將它當做 Azure Web 服務部署。 使用者可以使用 Web 服務，將料傳送到您的模型，模型就會傳回其預測。

如需完整部署流程的詳細資訊，請參閱[部署 Azure Machine Learning Web 服務][deploy]

[deploy]: machine-learning-publish-a-machine-learning-web-service.md


<!-- Images -->
[figure1]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure1.png
[figure2]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure2.png
[figure3]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure3.png
[figure4]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure4.png
[figure5]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure5.png
[figure6]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure6.png


<!-- Module References -->
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[export-data]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/

