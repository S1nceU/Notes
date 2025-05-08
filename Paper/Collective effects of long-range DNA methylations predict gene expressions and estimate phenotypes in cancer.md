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
- Methylation Data $\rightarrow$ $\beta$ 不適合直接用於線性回歸模型，因其分布不符合常態分佈
- 