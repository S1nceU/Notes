## Problem 
#### ChAMP
Q : 了解"取得具差異性的甲基化位點"
A : 
Q : 差異性是否去除掉管家基因了？！
A : 
Q : 數據的過濾方法
A : 
Q : .DMP 這個函式
A : 

#### GO ( Gene ontology )
URL : 
- [使用基因功能分類GO(Gene Ontology)分析(一)：簡介 – 我們的基因體時代 Our "Gene"ration (weitinglin.com)](https://weitinglin.com/2016/10/18/%e4%bd%bf%e7%94%a8%e5%9f%ba%e5%9b%a0%e5%8a%9f%e8%83%bd%e5%88%86%e9%a1%9egogene-ontology%e5%88%86%e6%9e%90%e4%b8%80%ef%bc%9a%e7%b0%a1%e4%bb%8b/)
- [使用基因功能分類GO(Gene Ontology)分析(二)：GO term的結構 – 我們的基因體時代 Our "Gene"ration (weitinglin.com)](https://weitinglin.com/2016/10/19/%E4%BD%BF%E7%94%A8%E5%9F%BA%E5%9B%A0%E5%8A%9F%E8%83%BD%E5%88%86%E9%A1%9Egogene-ontology%E5%88%86%E6%9E%90%E4%BA%8C%EF%BC%9Ago-term%E7%9A%84%E7%B5%90%E6%A7%8B/)
- [使用AmiGO和QuickGO來獲取Gene Ontology資料 – 我們的基因體時代 Our "Gene"ration (weitinglin.com)](https://weitinglin.com/2016/11/23/%e4%bd%bf%e7%94%a8amigo%e5%92%8cquickgo%e4%be%86%e7%8d%b2%e5%8f%96gene-ontology%e8%b3%87%e6%96%99/)
- [簡單分析Gene Ontology 的工具網站 – 生命科學圖書館推廣服務誌 (sinica.edu.tw)](https://lsl.sinica.edu.tw/Blog/2016/01/gene-ontology/)

AmiGO 可以直接查詢

1. hypertension methylation 論文查詢  
2. 125筆高血壓所有相似疾病 cosine similarity計算疾病相似度 利用加卡 hierarchical clustering (階層式分群法)去做分群 再去跑分析 
3. 查看beta分布的histogram gamma distribution normal distribution peak distribution M字型 
4. 與普通疾病的housekeeping gene list去match有符合多少 去做幾分之幾的數據

 
#### 專題單字小教室

| English | 中文 | 備註 |
| :---: | :---: | :---: |
| histogram | 直方圖 | |
| gamma | $\gamma$ | |
| distribution | 離散 | |
| peak | 峰值 | |
| significant | 顯著的 | | 
| correlation | 相關性 | |
| specific | 明確的 | |
| variation | 變化的 | |
| candidate | 候選 | |
| validate | 驗證 | |
| aspect | 方面 | |
| cluster | 分群 | |
| intersity | 強度 | |


#### Normal distribution ( Gaussian distribution )
常態分布 ( 高斯分布 )
+ 將一連續變項之觀察值發生機率以圖呈現其分布情形
+ 中線會是平均值
+ 範圍：負無窮大 ~ 無窮大


#### P-value
p值其實就是在分佈常態分佈下≥t值的機率密度值(probability density value，P(x≥t))

##### T-value
越接近 0 代表差異越沒有差異，相反的，越遠離差異就越大
![[Pasted image 20230317174418.png]]
( 平均數1 - 平均數2 ) / 兩組資料的標準差

##### How to calculate P-value
從 T-value 積分到無窮大之面積，就為 P-value
有兩邊無窮大，所以正常來說兩邊會加起來，也可以面積 * 2

##### T分布 ( Student's t-distribution )
抽樣下的常態分佈
自由度越高，會越接近該樣本的常態分佈
v -> 自由度 ( N1 + N2 - 2 )
![[Pasted image 20230317175850.png]]

##### P-value < 0.05
意義就在於要當下 T-value 到無窮大的面積大於 0.05 的自由度
而就是說我們**接受虛無假說**，又可以說兩筆資料是有差異的

相反

意義就在於要當下 T-value 到無窮大的面積小於 0.05 的自由度
而就是說我們**拒絕虛無假說**，有可以說兩筆資料差異不大
