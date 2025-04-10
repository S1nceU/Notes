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
