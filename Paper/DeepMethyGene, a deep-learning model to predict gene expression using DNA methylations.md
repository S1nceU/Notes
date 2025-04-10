---
Jounal name: "[[BMC Bioinformatics]]"
DOI: https://doi.org/10.1186/s12859-025-06115-2
tags:
  - DNA
  - Methylation
  - "#MachineLearning"
  - "#DeepLearning"
---
## Background
- 在基因調控與表關遺傳學中，已有許多研究透過多樣化的架構預測 miRNA 與疾病的關聯，展示了整合網路嵌入與深度學習技術的潛力。
	- DBNMDA：[DBNMDA: 预测潜在 miRNA-疾病相关性的深度信念网络（Briefings in Bioinformatics）_mirna 深度学习-CSDN博客](https://blog.csdn.net/adsdasdasdahj/article/details/130550061)
	- DBN 架構預測 miRNA-disease 關係
- 也有研究題及 CNN 萃取蛋白質特徵遠端同源性檢測的技術
	- DeepRHD：[DeepRHD: An efficient hybrid feature extraction technique for protein remote homology detection using deep learning strategies - ScienceDirect](https://www.sciencedirect.com/science/article/pii/S1476927122001293)
	- 使用 CNN 萃取蛋白質序列特徵
- 這些成功案例說明深度學習在生物序列建模上的潛力
- 因此作者延伸此概念至甲基化與基因表現層級，提出 DeepMethyGene，探索 CpG 甲基化與 RNA 表現之間的預測關聯。
- 提出 DeepMethyGene 模型，利用 CNN 深度學習架構，從基因區域附近的 DNA 甲基化數據（CpG M 值）預測基因表現量（RNA-seq derived gene expression），希望以更便宜穩定的甲基化資料推估高變動成本高的 RNA 資料，具有潛在臨床與生物資訊應用價值。
## Method
### Datasets
- 使用了 TCGA 中多個癌症類型的數據
	- 乳癌(BRCA)、結腸癌（COAD）、膠質母細胞瘤（GBM）以及肺腺癌（LUAD）
	- DNA：450K 的資料
	- RNA：RNA-seq 基因表現數據
- 主要訓練集：
	- BRCA
	- 788 tumor
	- 85 normal
- 而其他資料則作為測試模型泛化的能力

### Data proprocessing
- 篩選甲基化資料的方法
	- 過濾缺失值並且將基因表達量 $\beta$ 值轉換為 $M$ 值
	- $M$ 值更適合建模 (以取得類高斯分布 $\rightarrow$ 提高模型穩定性)
		![[Pasted image 20250411041543.png|300]]
		- M-values具有更好的统计特性，更适合用于进行下游的统计分析（差异分析等）
		- Beta-values更加容易解释，更能说明生物学上的意义
		
- Beta-values更加容易解释，更能说明生物学上的意义
	- 透過表達程度以及啟動子篩選出，高代表性的探針以及基因
	- 細節如何篩選，文中表達透過引用 [34] 
		- Data preprocessing followed the methodology outlined in [34]
- 模型訓練資料
	- 透過 BRCA 控制、實驗組的資料訓練模型外，也將控制組的 85 筆樣本單獨抽出再訓練模型，**減少因為腫瘤與正常樣本差異量所造成異質性干擾模型表現**
- 測試資料
	- 各資料集作為驗證評估不同癌症類型的預測效能
DeepMethyGene 模型是針對「某個病人」的「某個基因」，使用該基因附近 ±1Mb 的 CpG 甲基化資訊，來預測該基因在該病人中 RNA-seq 測得的表現量。
雖然 DNA 甲基化（M 值）與 RNA 表現量是來自不同實驗平台，分別反映不同層級的分子活動，但已有大量研究指出，基因啟動子與鄰近區域的甲基化程度與基因轉錄活性呈顯著負相關。因此，透過 CNN 模型對 CpG 甲基化區段進行特徵學習，有機會建構出由 M 值預測 RNA 表現量的模型。DeepMethyGene 即是針對此一生物學假設所設計的深度學習架構。
### DeepMethyGene model
- 作者提出一模型發想，建立在**自適應性**、**遞迴卷積神經網路**架構上的預測模型，源自於 ResNet
	- 可調節（adaptive）且遞迴（recursive）的卷積神經網路（CNN）
- 該模型專門用來處理 **一維輸入資料**，例如甲基化資料在基因組上的連續分佈，因此也非常適合用在 **時序資料（time series）與訊號分析（signal processing）**。
- 這個網路架構的關鍵特點是：
	- 能夠根據輸入資料的規模，**動態調整模型的複雜度**
	- 以此達到 **計算效能與預測效果的最佳化**
- 殘差模組（residual blocks）
	- 有效解決深度神經網路在訓練過程中常見的問題
		- **梯度消失（vanishing gradient）**
		- **梯度爆炸（exploding gradient）**

- 模型設計架構
	- 輸入為 TSS ±1Mb 區域 CpG 的 M 值向量
	- 為一個**自適應回歸卷積神經網路（AdaptiveRegressionCNN）**
	- 兩層一維卷積層 (Conv1 d)
	- 全連接層 (Fully Connected layers)，用於特徵映射與進行最終的迴歸(regression)預測
	- 流程為
		- 第一層卷積後，加入 **一個殘差區塊（Residual Block）**
		- 接著再加第二層卷積與 **另一個殘差區塊**
		- 經過 **一個含 512 個神經元的全連接層**
		- 然後再通過一個**單一輸出神經元**進行基因表現量的預測（回歸模型）
	![[Pasted image 20250410223424.png]]
	- 最佳化以及評估方法
		- Adam 優化器 ( learning rate = 0.001 )
		- MSELoss 
		- 5-fold cross-validation
		- correlation coefficient
		- $R^2$ value
	- 訓練過程設定
		- 700 epochs
		- Early stopping when patience = 90，以避免 overfitting
- 最後的結果會是單一位點的表現資料

### Design of residual blocks 殘差區塊的設計
- Residual block 包含兩層一維卷積 (Conv1 d)
- 每一層的大小
	- Kernal size : 3
	- padding : 1
- 這樣的設計在不改變資料維度的情況下，增加卷積層
- 在每一層卷積過後，LeakyReLU 激活函數
	- 負斜率設為 0.01 -> 對於負數輸入，輸出不回零，而是保留部分訊號
	- 可避免 ReLU 死區問題 (Dead zone)，即輸出長期為 0 而導致神經元失效
- 殘差區塊透過 **跳接（skip connection）** 將：
	- 原始輸入 → 與卷積層輸出相加
	- 好處：
		- 避免 gradient vanishing/explosion
		- **讓模型更容易學習“變化”而不是“絕對輸出”**，提升訓練效率與穩定性

### Architecture of the adaptive regression convolutional neural network
- 模型架構
	- Conv1d + Residual Block 所組成，形成一個 adaptive regression CNN
- 為不同基因上下流的CpG數量不一致的問題，作者在卷積通道數做了設計
	- 第一層卷積：
		- 輸出通道數 = $min(64, input\_size \ // \ 10)$
	-  第二層卷積：
		- 輸出通道數 = $min(32, input\_size \ // \ 20)$
- 每個卷積模組之間會加入 **殘差區塊（Residual Block）**
- 所有經卷積與殘差強化後的特徵，會被 **攤平成一維向量（reshape）**
- 然後再輸入到 **全連接層（Fully Connected, FC）**
模型尾端的全連接層負責將卷積網路所學到的特徵進行整合與壓縮，前一層使用 512 個神經元作為特徵抽象層，最後輸出至一個單一神經元進行迴歸，輸出即為該 sample 在該基因的預測表現量。這樣的設計能有效保留高維特徵資訊，同時控制模型參數數量，達到準確與計算效率的平衡。
### 不同的模型比較
📊 結論補充比較：

| 模型            | 優勢            | 劣勢                |
| ------------- | ------------- | ----------------- |
| SVM           | 小資料集、線性特徵     | 無法處理高維、非線性、時間依賴資料 |
| Deep Forest   | 處理表格資料與非線性關係佳 | 缺乏捕捉序列連續性與深層特徵的能力 |
| DeepMethyGene | 善於學習序列、特徵層次深  | 訓練成本較高，但效能最佳      |
#### SVM Support Vector Machine
- 但在這個高維度的資料集則會有效能下降的可能性
- 難處理時間序列這類具非線性與依賴性特徵的資料
#### Deep Forest
- 又稱 Cascase forest
- 以集合學習原則為基礎，與 RF 一樣以 Decision Tree 為基礎
- 特點：將多層模型 **像神經網路一樣堆疊成階層式結構（multi-layered）**

- 運作方式：
	- 每一層會輸出一組預測結果或特徵
	- 再作為**下一層的輸入**
	- 最終形成一種 **類似深度學習的階層式決策架構**
- 雖然能捕捉複雜的特徵關係，但無法像 ResNet 等深度學習架構那樣，捕捉到連續性與動態變化的特性

## Result
### DeepMethyGene accurately predicts gene expression using DNA methylation
- 作者用了自己研究的模型與其他模型做了比較，像是 SVM、RF、geneEXPLORE等模型對象
- 本段結果顯示 DeepMethyGene 在預測基因表現量上表現優異。相較於現有模型（如 geneEXPLORE、SVM、Random Forest），DeepMethyGene 在 TCGA 乳癌資料集上達成 R² = 0.640，並在三個其他癌症資料集中均展現高度泛化能力。透過消融實驗進一步證明卷積層對模型效能至關重要。此外，即便模型僅使用正常樣本訓練，也仍維持高準確率，反映其對資料異質性的穩定適應能力。
### The prediction performance is influenced by the number of CpGs near a gene
- CpG 數量是否影響預測準確度？
- 在作者的測試底下，發現抓取的 CpG 數越多，預測越準
- 反映資料豐富性對深度模型特徵學習的重要性。此結果不僅支持模型設計的合理性，也提供後續預測應用中的信心參考。
- 要能精確預測基因表現，需要周圍 CpG 位點提供**足夠豐富的甲基化資訊**
- 若某個基因本身附近 CpG 少，那模型預測可能會比較沒把握
### The prediction performance is associated with the distance between CpGs and genes
- 不同**距離範圍內的 CpG 位點**對預測是否有不同的貢獻
- 他們做了多組模型訓練，每組輸入資料的 CpG 位置是：
	- **從 TSS 起 ±1 kb、±10 kb、±100 kb、±1 Mb**
- 作者發現距離基因越近的 CpG 位點對預測表現貢獻越大
## Conclusions
- 作者使用 TCGA 數個資料集，為了開發一個能夠預測基因表現量的深度學習模型
- 而在選取 CpG 位點的方法，傳統方法很難處理在每個基因要取不同的位點數量
- 所以作者研究了一個解決方案，DeepMethyGene，採用動態通道調整 + 卷積神經網路 + 殘差結構(ResNet block)適應不同數量位點輸入的問題
- 模型架構也會根據輸入的CpG數量動態調整通道計算量，避免浪費資源，同時也能提升效能以及準確率
- 模型透過CNN神經網路抽取 CpG 和表現量中複雜的序列特徵，並且運用 ResNet 穩定模型表現，並且在其中也能夠學會「局部 CpG 密度、位置、距離」與 RNA expression 的非線性關係
- 在與傳統方法比較時，也更加準確也更穩定
	- DeepMethyGene （R² = 0.640）
	- geneEXPLORE（R² = 0.449）
	- SVM（R² = 0.327）
	- Random Forest（R² = 0.374)
- 在數據統計上，將 $\beta$ 轉為 $M$ 值，使得數值更接近常態分佈，能提升統計學的穩定性以及預測表現
- 在未來應用上，也可用於時序性的資料，像是疾病進程預測等，甚至能夠在臨床較塊推估 RNA 的表現
## Summary
本研究


學會的方法：
在神經網路訓練中，每筆樣本會進行一次 forward 預測與 loss 計算，接著透過反向傳播計算每一層參數的梯度（方向與靈敏度），並根據這些梯度與學習率共同決定如何調整模型的參數，使預測更準確。這是模型「學會」資料的核心機制。

在深度學習模型中，對每一筆資料，模型會先進行前向傳播（forward pass）產生預測值，並透過損失函數（例如 MSELoss）計算預測與真實值的誤差。接著透過反向傳播（backpropagation）計算每一層參數對 loss 的偏導數（即梯度），以決定這些參數對預測結果的重要性與修正方向。最終透過學習率（learning rate）決定實際的更新幅度。

而為了避免在深層網路中出現梯度爆炸（exploding gradient）與梯度消失（vanishing gradient）問題，模型中引入 Residual Block，其透過跳接（skip connection）機制，讓梯度可在網路中更穩定地傳遞，進而提升模型可訓練性與收斂速度。