## 資工碩一 113598007 楊明哲
### 環境設置
1. **Windows**
	+ 環境版本
		+ Java : 17.0.12
		+ Python : 3.10.10
		+ PySpark : 3.5.3
		+ Pandas : 2.2.3
	- 執行指令 spark-class org.apache.spark.deploy.master.Master，並打開 URL
		- 指令結果![[Pasted image 20241016233327.png|600]]
2. **Ubuntu**
	+ 環境版本
		+ Memory : 8GB
		+ Processors : 16
		+ Java : 17.0.12
		+ Python : 3.10.12
		+ Spark : 3.5.3
	+ 執行 spark-class org.apache.spark.deply.worker.Worker spark://192.168.18.5:7077 


### 資料夾結構
- homework1.py
- archive
	- spacenews.csv
- image
	- all of output snapshots
### 源代碼
![[Pasted image 20241027185615.png|600]]
![[Pasted image 20241027190729.png|600]]

### 輸出結果
+ 執行 spark-submit --master spark://192.168.18.5:7077 --conf spark.driver.host=192.168.18.5 homework1.py

#### 第一題
- Title word frequency
![[1.png|200]]
![[2.png|200]]
#### 第二題
- Content word frequency
![[3.png|200]]
![[4.png|200]]

#### 第三題
- Daily article count
![[5.png|200]]
![[6.png|200]]

#### 第四題
- Space in Title and Postexcerpt
![[7.png|600]]
### 小組分工

| 學號、姓名 | 貢獻比例 |     工作內容      |
| :---: | :--: | :-----------: |
|  楊明哲  | 100% | 程式編寫、除錯以及文書處理 |

