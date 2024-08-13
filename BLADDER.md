## 基本統計
- Tumor vs Normal
	- Normal : 100
	- Tumor : 87
- Gender
	- Normal
		- Male : 66
		- Female : 34
	- Tumor
		- Male : 54
		- Female : 33
- Age
	- Normal
		- Mean : 63
		- Min : 21
		- Max : 92
	- Tumor 
		- Mean : 72.7
		- Min : 43
		- Max : 94

## 基因表達與 Tumor 的關係
- COL2A1 (ref gene), dOTX1, dTFAP2B, dZNF154, dGALR1, dZIC4

#### COL2A1
- Normal 
	- Mean : 25.26
	- Min : 17.45
	- Max : 31.86
- Tumor 
	- Mean : 26.21
	- Min : 18.2
	- Max : 21.8

#### dOTX1

z-score 標準化、min max 標準化?

建一個最佳基因組合的模型後，將所有資料放進該模型，出來有生病的人找出共病相似的有那些病種

- 591.（腎水腫）：有显著关系，说明腎水腫可能与腫瘤的发生有关。
- 599.（其他尿道及泌尿道疾患）：有显著关系，说明其他尿道及泌尿道疾患可能与腫瘤的发生有关。


Group 1
- Age, dOTX1, dGALR1, 599.(其他尿道及泌尿道疾患), 591.(腎水腫)
- 年齡、基因、共病
Group 2
- dOTX1, dGALR1, 599.(其他尿道及泌尿道疾患), 591.(腎水腫)
- 基因、共病
Group 3
- 599.(其他尿道及泌尿道疾患), 591.(腎水腫)
- 共病
Group 4
- dOTX1, dGALR1
- 基因
Group 5 
- Age, dOTX1, dGALR1
- 年齡、基因
Group 6
- Age, 599.(其他尿道及泌尿道疾患), 591.(腎水腫)
- 年齡、共病

| Group | Accuracy | Precision | Recall | F1 Score |
|:-----:| -------- | --------- | ------ | -------- |
|   1   | 0.8246   | 0.7586    | 0.8800 | 0.8148   |
|   2   | 0.7544   | 0.6667    | 0.8800 | 0.7586   |
|   3   | 0.5965   | 0.5217    | 0.9600 | 0.6761   |
|   4   | 0.7895   | 0.7600    | 0.7600 | 0.7600   |
|   5   | 0.8596   | 0.7931    | 0.9200 | 0.8519   |
|   6   | 0.7544   | 0.6774    | 0.8400 | 0.7500   |


