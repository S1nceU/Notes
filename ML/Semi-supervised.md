### Semi-supervised learing
+ 通常數據集內以標籤的數量會遠少於未標籤資料
+ Transductive learning: unlabeled data is the testing data
	+ **not cheating**, use testing set with label is cheating
	+ 常用在對特定數據集有目標需求，且對於新數據沒有分析需求的狀況。
+ Inductive learning: unlabeled data is not the testing data
	+ training 時，還不知道 testing set，learning model 完後才有 testing set 可以使用
	+ 常用於數據樣本夠大，標籤數據夠多時，或是需求是一般的而非特化的目標的狀況。

| Inductive learning | Transductive learning |
| ------------------ | --------------------- |
| 給從未見過的數據           |                       |
|                    |                       |
 ### Why semi-supervised learing?
+ 我們不缺 data，則是缺 **labeled** 的 data，**unlabeled** 的 data 則可以有超級多
+ 所以用 unlabeled 的資料能夠做，非常做有價值

+ 半監督式學習重點在於假設的合理性

### Supervised Generative Model
+ $\mu$ : 所有樣本數值平均
+ Init: $\theta = \{P(C_1), P(C_2), \mu^1, \mu^2, \Sigma\}$
+ Step 1: compute the 所有 unlabeled data 的後驗機率
	+ $P_{\theta}(C_1 \ |\  x ^ \mu)$
+ Step 2: update
	+ $P(C_1) \ = \frac{N_1 \ + \ \Sigma_{x^u} \ P(C_1 | x^\mu)}{N}$ , $N$ : 所有樣本, $N_1$ : Class 1 的所有樣本, unlabeled 樣本需要用後驗機率的方式了解他為 Class 1 還是 Class 2
		+ 就是將 label 跟 unlabel 的機率加起來多少
	+ $\mu^1 \ = \ \frac{1}{N_1}\sum_{x^r\in C_1}x^r \ + \ \frac{1}{\sum_{x^u}P(C_1 \ | \ X^u)} \sum_{x^u}P(C_1 \ | \ x^u)x^u$
		+ 就是將 label 跟 unlabel 加起來的總值平均
+ 有了 Step 2 新的 model 之後，$P_{\theta}(C_1 \ |\  x ^ \mu)$ 就會不一樣，就回到 Step 1 做迭代至收斂 (**Soft label**)

### Low-density Separation
+ 非黑即白的世界
#### Self-training

+ 在設定 boundary 時，選擇 density 較低的比較好
+ 有點像 Class 1，那就是被歸類於 Class 1，不會有可能 (**Hard label**)

#### Entropy-based Regulariztion
