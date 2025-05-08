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
	- 乳癌(BRCA)、10 種 TCGA 癌症類型（如 LUNG、GBMLGG、PRAD...）與 GSE39004
	- DNA：450K 的資料
	- RNA：RNA-seq 基因表現數據
- 主要訓練集：
	- BRCA
	- 788 tumor
	- 85 normal
- 而其他資料則作為測試模型泛化的能力

### Pre processing
- Methylation Data $\rightarrow$ $\beta$ 不適合直接用於線性回歸模型，因其分布不符合常態分佈，所以作者使用 $M \ value$，Beta 值界於 0 到 1，偏態分布，不符合線性假設
	![[Pasted image 20250411041543.png|300]]
- 作者在芯片中得到了 485577 個位點，刪除缺失 90007 個位點，並將那些缺失值使用 K-means 做缺失值的插補，補了 31700 個位點
- 20540 個基因，平均表現低於 1RPKM，移除了 3418 個基因
- 透過 UCSC 基因體瀏覽器找到 TSS 有 16681，且在這 16681 當中的 13982 promoter 至少有一個 probe
### Measuring prediction accuracy
- 10 fold CV，9 train、1 test
- $\hat{y}_g = \sum_{k=1}^{M_g} \hat{w}_{k,g} x_{k,g}$
	- $\hat{y_g}$ : 預測 RNA expression
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
- 因為無法處理高維數據 (樣本數 < 探針數)
- 針對樣本的75%做迴歸模型

- 將每一個基因 g，用所有的探針做一次CpG檢定得到結果
- 做多重檢定校正，Bonferroni correction，只保留校正後 p-value < 0.05 的探針 (為了讓探針能夠減少)
- 用篩選後的 CpG 位點為每個基因做多元線性迴歸模型，與 geneEXPLORE 比較
### Testing on an independent cohort
用不同的乳癌樣本，驗證模型泛化能力

### Predicting clinical phenotypes
1. **癌症狀態（Cancer status）**
    - tumor vs. normal（788 tumor / 85 normal）
2. **雌激素受體狀態（ER status）**
    - ER-positive vs. ER-negative（632 positive / 183 negative，58 缺值）
3. **5 年存活率（5-year survival）**
    - 存活與否
4. **PAM50 乳癌亞型（如 Luminal A, Basal 等）**

#### 預測流程
- 預測基因表現
	- 用geneEXPLORE預測每個樣本的表現量
- 使用 Elastic-Net 邏輯迴歸
	- 將預測值帶入邏輯迴歸
	- $logit(p) \ = \ \sum_{g=1}^{G} \hat{\beta_g}\hat{y_g}$
	- 透過 sigmoid 公式，將函數轉回機率 $p$
	- 判別有沒有癌症等特徵，做出分類決策
- 透過 ROC 曲線評估結果
## Result
### **gene** **EXP**ression prediction by **LO**ng-**R**ange **E**pigenetics (geneEXPLORE)
- 結果目標想要做到，相對於傳統方法只看啟動子區域外，geneEXPLORE 還包括跨區域的效果

- 輸入
	- 某樣本的某一基因附近內的 CpG 甲基化值
- 建模
	- 採用 Elastic Net 建立每個基因的迴歸模型
	- 輸出預測的基因表現量 (RNA)
- 訓練
	- 10 fold CV，透過R square評估
![[Pasted image 20250509030142.png]]

|子圖|功能步驟說明|對應方法章節段落|中文解釋|
|---|---|---|---|
|(a)|發現長距離甲基化與基因表現的關聯|**geneEXPLORE 建模原理**、_DNA methylation and RNA-seq data_|基因的表現可能受到來自很遠距離的 CpG 探針調控，需涵蓋跨區域 methylation。|
|(b)|定義探針選取範圍與預測用輸入變數|_Investigation of various distances from promoter regions of genes_|每個基因 gg 取 ±LgL_g Mb 範圍內的 MgM_g 個探針作為模型輸入。|
|(c)|使用 Elastic Net 對每基因預測表現值|_geneEXPLORE 建模方程式_、_Pre-processing_、_Measuring prediction accuracy_|每個基因用 Elastic Net 篩選重要探針，預測表現值 y^g\hat{y}_g。|
|(d)|用預測表現值預測臨床表型（如腫瘤）|_Predicting clinical phenotypes_|使用預測出來的 gene expression y^g\hat{y}_g 作為輸入，預測 pp。|
### The collective effect of long-range methylation on gene expression is higher than that of promoter and gene region methylation on gene expression
- 比較了啟動子區域、基因區域、長距離區域，誰對預測最有效
- 作者用了三個區域建構 geneEXPLORE 模型
![[Pasted image 20250509030434.png]]

| 區域          | 預測平均準確度（CV R²） |
| ----------- | -------------- |
| Long-range  | **0.486**（最高）  |
| Gene region | 0.218          |
| Promoter    | 0.064（最低）      |
- 作者表達了基因表現往往受遠端調控元件所調控，而非僅靠近端的啟動子區域
	- 單一 CpG 的作用可能微弱，但遠端探針的「**集體效應**」透過 elastic net 統合起來，就能產生更完整的預測力
	- 即使遠端探針單獨看效果不大，但在模型中聯合作用時效果明顯提升
### geneEXPLORE outperforms state-of-art gene expression prediction methods
- 作者透過現有的預測基因表現方法比較，也是略勝一籌

| 方法名稱            | 模型技術               | 探針來源區域    | 平均 CV R²  |
| --------------- | ------------------ | --------- | --------- |
| **BioMethyl**   | 無懲罰項的多元迴歸          | 基因區域      | 0.148     |
| **MethylXcan**  | Lasso 回歸           | 基因區域      | 0.224     |
| **geneEXPLORE** | Elastic Net（L1+L2） | 長距離（全染色體） | **0.491** |
## 🔍 為什麼 geneEXPLORE 表現更好？
1. **使用 Elastic Net（比 Lasso 更穩定）**
    - 對高度相關的 CpG 資料（common in methylation）效果更好
    - 可以同時進行變數選擇與懲罰，防止過擬合又保留訊號
2. **涵蓋更廣的探針區域（全染色體）**
    - 而非僅限於基因內部或啟動子
    - 捕捉更多長距離調控訊號（trans-methylation）
3. **自動挑選有效 CpG，忽略無關探針**
    - Elastic Net 不需像 eQTM 一樣事先做顯著性檢定 → 更彈性與全面

### Prediction comparison between geneEXPLORE and multiple regression using expression quantitative trait methylations (eQTMs) in TCGA breast cancer
**geneEXPLORE 比傳統 eQTM 方法在高維資料中更穩定且強大**，特別是在處理 long-range CpG 時表現壓倒性優勢，因為它：
- 不需做前置顯著性檢定
- 可以同時處理數萬個探針
- 自動調整與選擇關鍵 CpG

### Testing geneEXPLORE on an independent cohort
- 測試跨資料集的泛化能力
- 使用 乳癌樣本 做測試

| 方法          | 平均 R²（Test R²） |
| ----------- | -------------- |
| geneEXPLORE | **0.261**      |
| eQTM        | 0.181          |
- 即使測試資料來自 **不同實驗平台（array vs. RNA-seq）**，geneEXPLORE 預測仍表現穩定
- 顯示 geneEXPLORE 不僅在原始資料內部準確，也具備良好的**跨資料集可移植性**
- 證明 geneEXPLORE 適合用於預測未知樣本的基因表現，即使沒有 RNA-seq

### Applicability of geneEXPLORE to 10 other types of cancer
- geneEXPLORE 是否不只適用於乳癌（BRCA）資料，也能廣泛應用於其他癌症類型？![[Pasted image 20250509032652.png]]
- 每一種癌症應「個別訓練模型」，因為 **enhancer 活性具有癌症特異性**

### geneEXPLORE accurately predicts expression of tumor-associated genes
- 作者也有發現，在已知乳癌發展有關的基因，其表現在 geneEXPLORE 非常準確地預測

### geneEXPLORE accurately predicts clinical features of human cancer based on the predicted gene expressions
- 在臨床上的應用

| 預測目標                          | 分類                     |
| ----------------------------- | ---------------------- |
| Cancer status                 | 腫瘤 vs. 正常              |
| ER (estrogen receptor) status | ER 陽性 vs. 陰性           |
| 5-year survival               | 存活 vs. 未存活             |
| PAM50 subtypes                | Luminal A, Basal 等亞型分類 |
- 將樣本的基因表現量作為輸入到 geneEXPLORE
- 使用 Elastic Net 邏輯迴歸模型，預測各個臨床表型
![[Pasted image 20250509035614.png]]