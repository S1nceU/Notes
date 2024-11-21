## 資工碩一 113598007 楊明哲
### 環境設置
1. **Windows**
	+ 環境版本
		+ Java : 17.0.12
		+ Python : 3.10.10
		+ PySpark : 3.5.3
		+ Pandas : 2.2.3
	- 執行指令 spark-class org.apache.spark.deploy.master.Master，並打開 URL
		- 指令結果![[Pasted image 20241119181843.png|600]]
2. **Ubuntu**
	+ 環境版本
		+ Memory : 8GB
		+ Processors : 16
		+ Java : 17.0.12
		+ Python : 3.10.12
		+ Spark : 3.5.3
	+ 執行 spark-class org.apache.spark.deply.worker.Worker spark://192.168.18.11:7077 
### 資料夾結構
- homework2.py
- Data
	- all of datasets 
- Output
	- all of output csv
### 源代碼
![[Pasted image 20241118152955.png|600]]
![[Pasted image 20241118153031.png|600]]
![[Pasted image 20241118153057.png|600]]
![[Pasted image 20241118153127.png|600]]
![[Pasted image 20241118153154.png|600]]
### 輸出結果
+ 執行 spark-submit --master spark://192.168.18.11:7077 --conf spark.driver.host=192.168.18.11 homework2.py
#### Spark Master Web
![[Pasted image 20241119181731.png|800]]
#### 第一題
- Words frequency
##### Total word frequency
![[Pasted image 20241118153402.png|200]]
##### Daily word frequency
![[Pasted image 20241118153453.png|200]]
##### Topic word frequency
![[Pasted image 20241118153536.png|200]]
#### 第二題
- Social Media word frequency
##### economy
- Hours
![[Pasted image 20241118154605.png|600]]
- Daily
	- Facebook、GooglePlus、LinkedIn
	![[Pasted image 20241118154927.png|300]]
##### microsoft
- Hours
![[Pasted image 20241118155141.png|600]]
- Daily
	- Facebook、GooglePlus、LinkedIn
	![[Pasted image 20241118155222.png|300]]
##### obama
- Hours
![[Pasted image 20241118155420.png|600]]
- Daily
	- Facebook、GooglePlus、LinkedIn
	![[Pasted image 20241118155441.png|300]]
##### palestine
- Hours
![[Pasted image 20241118155513.png|600]]
- Daily
	- Facebook、GooglePlus、LinkedIn
	![[Pasted image 20241118155539.png|300]]
#### 第三題
- Social Media word frequency
![[Pasted image 20241118155742.png|300]]
![[Pasted image 20241118035317.png]]
#### 第四題
- Top 100 words in title and content
##### enconomy
- Title
![[Pasted image 20241118160044.png]]
- Headline
![[Pasted image 20241118160106.png]]
##### microsoft
- Title
![[Pasted image 20241118160121.png]]
- Headline
![[Pasted image 20241118160139.png]]
##### obama
- Title
![[Pasted image 20241118160159.png]]
- Headline
![[Pasted image 20241118160214.png]]
##### palestine
- Title
![[Pasted image 20241118160228.png]]
- Headline
![[Pasted image 20241118160241.png]]
### 小組分工

| 學號、姓名 | 貢獻比例 |     工作內容      |
| :---: | :--: | :-----------: |
|  楊明哲  | 100% | 程式編寫、除錯以及文書處理 |
