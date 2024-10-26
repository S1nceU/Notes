---
Jounal name: "[[Nature communications]]"
DOI: https://doi.org/10.1038/s41467-023-37837-7
tags:
  - DNA
  - Methylation
  - Kidney
  - Diabetic
---
## Background
- 全球正面臨2型糖尿病的流行，導致與糖尿病相關的終末期腎病 (ESKD) 負擔加重。鑒於糖尿病腎病 (DKD) 的可預防性，有必要識別那些有進展為 DKD 和 ESKD 風險的個體，以便及早進行強化干預。這些針對 DKD 的擴展治療選擇，增加了開發新模型以分層高腎功能障礙風險患者的迫切性。

- 遺傳及表觀遺傳標誌物，如甲基化和miRNA，可能為DKD風險分層提供新的線索。特別是甲基化標誌物，被認為可能介導代謝記憶並與糖尿病併發症相關。本研究探討外周血中CpG位點甲基化是否與腎功能相關，並檢驗其在預測2型糖尿病患者腎功能惡化中的潛力。


- 在 1995 年到 2007 年總共有 1271 名 2 型糖尿病患者，14.6 年有 33% [[ESKD|ESKD]] 患者
- 資料：患者的住院紀錄、隨訪數據，腎小管過濾率 ( [[GFR|eGFR]] )，並且資料中有含有性別、年齡、抽菸
- 甲基化晶片是 450K、血液樣本
- CpG 甲基化水平可以預測 eGFR 隨著時間的推移而下降，所以使用 eGFR 的成長斜率建構模型

## Method
- 他們所用的甲基化分析套件為 R 的 RnBeads，得到甲基化資料

 - 使用 PCA 對甲基化中 CpG 位點降維，得到數個主成分PCs，取 PCA 結果前 50 個主成分，接著將患者樣本進 k fold 分成 10 份，使用三個迴歸方式，每次都用9份做訓練1份做測試，觀察CpG位點裡的 PCs 是否與臨床變數有相關
	- 甲基化的主成分分析中與性別、年齡、吸菸狀況會有高度的關聯，所以很容易從 PCs 推測補充表 6的答案![[Pasted image 20241025032740.png]]
	- L2正規化的邏輯回歸、SVM、RF等分類模型![[Pasted image 20241025024901.png]]
	- 在4b的圖中可以觀察到性別、年齡、吸菸與eGFR相關性不高，可說明它們對腎功能的影響較為間接或不穩定
	- 在圖4a中，甲基化與腎臟功能一定的相關性
	- 而最後甲基化加入臨床變數後並沒有改善模型的表現，這可以表達在甲基化已經捕捉了很大一部份性別、年齡、吸菸的關係了，所以沒有變得更好，則可能因為多加了許多冗餘資料則使相關變低![[Pasted image 20241023225318.png]]
	 - 連結：[DKD EWAS AND PREDICTION MODELS](https://hkdbrmlab.shinyapps.io/DKD_EWAS/) ![[Pasted image 20241024224740.png]]

- eGFR 的計算方法是用 Chronic Kidney Disease Epidemiology Collaboration ( [[CKD-EPI]] ) 所計算
	- 而 baseline eGFR 是當下也是初始的 eGFR，以便與後續的 eGFR 做比較，單位 $ml/min/1.73m^2$
	- eGFR slope 是一個變化速率，單位 $eGFR / one year$
	- 公式：
		- 混和效應模型
			- $log(eGFR_{ij}) = \beta_0 + \beta_1t_{ij} + b_{0i} + b_{1i}t_{ij} + \epsilon_{ij}$
				- 第 i 個參與者，第 j 次的測量
				- $\beta_0$：表示所有患者在 t = 0 的平均 eGFR 對數值
				- $\beta_1t_{ij}$：$t_{ij}$ 對所有患者eGFR影響的平均變化率
				- $b_{0i}$：每個患者與總體平均截距之間的差異
				- $b_{1i}t_{ij}$：每個參與者的eGFR隨時間變化的個體特定速率
		- 斜率
			- $(eGFR\ slope)_i = (e^{\beta_{1} + b_{1i}} - 1) * 100$

- 單一位點分析
	- 作者將每個CpG位點建構一個線性模型，為了觀察每個位點對於eGFR的關係性
		- 利用甲基化水平以及其他協變量訓練出一線性回歸模型，主要看甲基化水平在模型中是否顯著
		- 可以透過 P 值驗證，也需要多重假設校正，為了減少像是甲基化
		- 而最後得到有顯著效果的 CpG 則可能在腎臟疾病中有重大關聯
	
- M value
	- $M \ value = log_2\frac{\beta}{1-\beta}$
	- 當甲基化水平非常接近0或1時，β值會變得極不穩定且非線性，這可能會影響回歸分析的效果
	- M value 可以放大 $\beta$ 的範圍，極端的 $\beta$ 有更多的數值能表示他的極端，則可以使得 $\beta$ 會有更好的數學特性，使在放進回歸模型時有更合適
	- 而作者在最後做了相關係數檢測，做了Pearson和Spearman結果為高度相關

- 以往單一位點研究兩個問題
	- 一個是有一些 CpG 點位雖沒有強烈關係，卻能補充其他位點來解釋殘餘的腎臟功能差異。而這些位點無法透過單一個位點去分別出來
	- 基因組有依賴性或是其他原因，單一位點可能會有強烈關係，但卻是因為依賴關係而造成冗餘，則結果在非功能的位點上
- 多位點方法
	- 所以他們開發了方法，多位點模型，可同時考慮所有 CpG 位點，並選擇一個子集建立模型，推斷 eGFR 的情況
	- 使用 LASSO 建構迴歸模型


## Result
- 作者將多個與基線 [[GFR|eGFR]] 和其斜率顯著相關的CpG位點，並在其中做了單一位點分析以及自建的多位點模型預測

	- 以下是文中有提及的基因
	- [[ITGB2]]：在單一位點分析取 FDR = 0.05 時，並未識別出，卻在多位點模型中識別出有一位點 cg24707889 有相關聯，並在其他研究中表示其實該基因與腎臟有相關聯，甚至有可預防猴子記性腎臟損傷的長期纖維化。
	- [[RFTN1]], [[CTSB]]：先前研究未發現與 eGFR 有相關的位點，位點表現卻有所差異。
	- [[TXNIP]]：在單一位點分析取 FDR = 0.05 時，並未識別出，卻在多位點模型中是選出一位點 cg19693031，在該基因的 3 端。基因的編碼與 [[DKD]] 的發病機理有關，該基因的位點在有併發症和沒有併發症的 1 型糖尿病患者之間有不同程度的甲基化。
	- [[ANXA1]]：在單一位點和多位點模型都有是別出的位點 cg13591783，而該基因與 [[DKD]] 在表現上有所差異。

- 驗證的7個位點為單一位點和多位點都有的位點做分析，則有 2 個位點在數據中就與控制組有顯著的差異了，也有其 2 個位點在另測試模型的數據中與腎臟病有所關聯。此凸顯了血液中甲基化與腎臟病之間有潛在關聯。

- 在最後有一獨立實驗，181位2型糖尿病患者，在隨訪期間有80位罹患ESKD，則將此事件放進多位點模型中，效果都相當好。則他將baseline eGFR與模型一起預測則**甲基化預測**效果完全不顯著，則可表明甲基化預測ESKD的能力是由 baseline eGFR 相關的甲基化變化所中介的
	- 在此研究中，基線eGFR能夠很好地評估這種風險，因為它**直接反映了腎臟當前的健康狀態**。而DNA甲基化變化則可能揭示了**腎臟疾病的潛在生物學機制**，特別是那些在基線eGFR不夠低時也能提供預測信息的情況。然而，當基線eGFR加入模型後，甲基化變化所能解釋的風險減少，這就是預測顯著性下降的原因。
	- 因為 baseline eGFR 在預測 ESKD 的情況下，它的情況太好，則稀釋掉的甲基化所帶來的潛在可能性

