## 資工碩一 113598007 楊明哲
### 環境設置
1. **Windows**
	+ 環境版本
		+ Java : 17.0.12
		+ Python : 3.10.10
		+ PySpark : 3.5.3
		+ Pandas : 2.2.3
	- 執行指令 spark-class org.apache.spark.deploy.master.Master，並打開 URL
		- 指令結果![[Pasted image 20241201180849.png|600]]
2. **Ubuntu**
	+ 環境版本
		+ Memory : 8GB
		+ Processors : 16
		+ Java : 17.0.12
		+ Python : 3.10.12
		+ Spark : 3.5.3
	+ 執行 spark-class org.apache.spark.deply.worker.Worker spark://192.168.18.6:7077 

### 資料夾結構
- homework3.py
- data
	- all of datasets 
- output
	- all of output csv

### 源代碼
![[Pasted image 20241201181012.png|500]]
![[Pasted image 20241201181026.png|500]]
![[Pasted image 20241201181041.png|500]]
![[Pasted image 20241201181100.png|500]]
### 輸出結果
+ 執行 spark-submit --master spark://192.168.18.6:7077 --conf spark.driver.host=192.168.18.6 homework3.py
#### Spark Master Web
![[Pasted image 20241201175842.png|800]]
#### Spark Cluster Web
![[Pasted image 20241201175831.png|800]]
#### 第一題
+ set representation
+ Output：shingles_matrix.csv
![[Pasted image 20241201181328.png|200]]
#### 第二題
+ minhash signatures
+ Output：minhash_signatures_matrix.csv
![[Pasted image 20241201181426.png|800]]
#### 第三題
+ candidate pairs
+ Output：candidate_paris.csv
![[Pasted image 20241201181509.png|150]]
### 小組分工

| 學號、姓名 | 貢獻比例 |     工作內容      |
| :---: | :--: | :-----------: |
|  楊明哲  | 100% | 程式編寫、除錯以及文書處理 |
