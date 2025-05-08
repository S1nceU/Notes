---
Jounal name: "[[Scientific Reports]]"
DOI: https://doi.org/10.1038/s41598-020-60845-2
tags:
  - DNA
  - Methylation
---
## Introduction
已有研究證明
- 基因啟動子 (promoter)
	- 抑制基因的轉錄作用
- 基因體 (gene body)
	- 甲基化表現與基因表現正相關
- 增強子 (enhancer)
	- 通常有較低的
	- 局部甲基化區域 (cis)
		- 距離基因 TSS 1Mb 以內
		- 與基因表現有相關性

近期文獻
- enhancer 離 TSS 超過 1Mb 卻與癌症有所關連
- enhancer 上的 methylation 與癌症 gene 失調有關連性，比 promoter 還明顯

單一 CpG 位點可能不準確的原因
- 多個 CpG 位點造成**集體效應** (collective effect)
- 單一基因通常受到多個 enhancer 共同調控
	- 大多分析方法檢測單一 CpG 位點與單一基因的關聯，難量化多 CpG 位點所帶來的集體效應

所以作者開發 geneEXPLORE
- 同時考慮 cis 與 trans 的 CpG 位點
- 量化多個 long range 甲基化位點的集體效應，建構預測模型
- 使用 TCGA 的多種資料評估驗證模型預測表現

## Method
### Dataset
- 使用了 TCGA 中多個癌症類型的數據
	- 乳癌(BRCA)、結腸癌（COAD）、膠質母細胞瘤（GBM）以及肺腺癌（LUAD）
	- DNA：450K 的資料
	- RNA：RNA-seq 基因表現數據
- 主要訓練集：
	- BRCA
	- 788 tumor
	- 85 normal
- 而其他資料則作為測試模型泛化的能力

### Pre processing
- Methylation Data $\rightarrow$ $\beta$ 不適合直接用於線性回歸模型，因其分布不符合常態分佈，所以作者使用 $M \ value$
	![[Pasted image 20250411041543.png|300]]
- 作者在芯片中得到了 485577 個位點，刪除缺失 90007 個位點，並將那些缺失值使用 K-means 做缺失值的插補，補了 31700 個位點
- 20540 個基因，平均表現低於 1RPKM，移除了 3418 個基因
- 透過 UCSC 基因體瀏覽器找到 TSS 有 16681，且在這 16681 當中的 13982 promoter 至少有一個 probe
### Measuring prediction accuracy
- 10 fold CV，9 train、1 test
- $\hat{y}_g = \sum_{k=1}^{M_g} \hat{w}_{k,g} x_{k,g}$
	- $\hat{y_g}$ : 預測表現量
	- $x_{k,g}$ : 第 $k$ 個與基因 $g$ 相關的probe
	- $\hat{w}_{k,g}$ : 對應 probe 的迴歸係數 (權重)
		- 權重透過 Elastic-Net，自動篩選與估計
	- $M_g$ : 選定區間內的 probe 數量
- $R^2$ 作為評估係數
### Comparing the prediction accuracy of different regions in a gene
- 對於每個基因所有的CpG位點分三個區域
	- Promoter
		- TSS 上游 2000 bp 以及 下游 0 bp 定義啟動子區域 [37]
	- Gene
		- 450k 註釋資料所提供的位置，promoter、5'UTR、First exon、Gene body、3'UTR
	- Long-range 
		- 為 TSS +- 一定數量的 $x$ Mb 的所有 CpG 位點，
- 對於每個區域的 CpG 位點們做10 fold CV

### Investigation of various distances from promoter regions of genes
- 對於每個 long-range，作者從 +- 1Mb 到 +- 50 Mb 甚至到整條染色體等，分別作訓練，計算 $R^2$，得到該基因最佳的long-range範圍
- geneEXPLORE 模型背後的核心邏輯：**不同基因受不同距離範圍的調控，且這些遠端調控的效果通常強於近端**
### Evaluating prediction accuracy using multiple regression based on eQTMs
用一傳統方法，與作者做的 geneEXPLORE 進行比較，而所用之方法是
- **expression Quantitative Trait Methylations（eQTMs）**
	- 每一個 CpG 探針與基因表現之間的統計關聯性測試
- 因為無法處理高為數據 (樣本數 < 探針數)
- 
