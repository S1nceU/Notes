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
- 也有研究題及 CNN 萃取蛋白質特徵遠端同源性檢測的技術
	- DeepRHD：[DeepRHD: An efficient hybrid feature extraction technique for protein remote homology detection using deep learning strategies - ScienceDirect](https://www.sciencedirect.com/science/article/pii/S1476927122001293)
- 作者了解甲基化與基因表達之間的關係，也...
- 因此作者提出，DeepMethyGene，利用variable convolution kernel 以及 ResNet block，試圖提高甲基化預測基因表達的準確率

## Method
### Datasets
- 使用了 TCGA 中多個癌症類型的數據
	- 乳癌(BRCA)、結腸癌（COAD）、膠質母細胞瘤（GBM）以及肺腺癌（LUAD）
	- DNA：450K 的資料
- 主要訓練集：
	- BRCA
	- 788 tumor
	- 85 normal
- 而其他資料則作為測試模型泛化的能力

### Data proprocessing
- 篩選甲基化資料的方法
	- 過濾缺失值並且將基因表達量 $\beta$ 值轉換為 $M$ 值
	- $M$ 值更適合建模 (為什麼?)
	- 透過表達程度以及啟動子篩選出，高代表性的探針以及基因
	- 細節如何篩選，文中表達透過引用 [34] 
		- Data preprocessing followed the methodology outlined in [34]
- 模型訓練資料
	- 透過 BRCA 控制、實驗組的資料訓練模型外，也將控制組的 85 筆樣本單獨抽出再訓練模型，**減少因為腫瘤與正常樣本差異量所造成異質性干擾模型表現**
- 測試資料
	- 各資料集作為驗證評估不同癌症類型的預測效能

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

### 不同的模型比較
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
- 