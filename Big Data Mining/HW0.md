## 資工碩一 113598007 楊明哲

### 環境設置
1. **Windows**
	+ 環境版本
		+ Java : 17.0.12
		+ Python : 3.10.10
		+ PySpark : 3.5.3
		+ Pandas : 2.2.3
	+ 環境配置
		+ 下載指定版本的 Python 和 Java
		+ 透過 pip install pyspark 安裝 pyspark，並且將它初始化
		+ 將下載的 winutils.exe 放進 pyspark 中 bin 的資料夾下
		+ 設置環境變數
			+ PYSPARK_PYTHON![[Pasted image 20241016232913.png]]
			+ PYSPARK_DRIVER_PYTHON![[Pasted image 20241016232557.png]]
			+ Path ![[Pasted image 20241016232947.png]]
			+ HADOOP_HOME![[Pasted image 20241017001753.png]]
			+ SPARK_HOME![[Pasted image 20241017001824.png]]
		+ 執行指令 spark-class org.apache.spark.deploy.master.Master，並打開 URL
			+ 指令結果![[Pasted image 20241016233327.png|600]]
			+ 網頁結果![[Pasted image 20241016233412.png|600]]
2. **Ubuntu**
	+ 環境版本
		+ Memory : 8GB
		+ Processors : 16
		+ Java : 17.0.12
		+ Python : 3.10.12
		+ Spark : 3.5.3
	+ 環境配置
		+ 下載指定的 Java 版本
		+ 下載指定的 Spark，並且解壓縮到 Documents 下
		+ 將各環境變數設置![[Pasted image 20241016234507.png|500]]
		+ 執行 spark-class org.apache.spark.deploy.worker.Worker spark://192.168.18.11:7077 ![[Pasted image 20241016234812.png|500]]
		+ 網頁畫面![[Pasted image 20241016234930.png|600]]
		+ 並且能夠在 Master 上的網頁看到 Worker 的資訊![[Pasted image 20241016235025.png|500]]


### 源代碼
![[Pasted image 20241016235146.png|600]]
![[Pasted image 20241016235159.png|600]]
![[Pasted image 20241016235209.png|600]]

### 輸出結果
+ 執行 spark-submit --master spark://192.168.18.5:7077 --conf spark.driver.host=192.168.18.5 homework0.py
#### 第一題
+ Find min, max, count
![[Pasted image 20241016235602.png|600]]
#### 第二題
+ Find mean, standard deviation
![[Pasted image 20241016235650.png|600]]

#### 第三題
+ Normalization
![[Pasted image 20241016235753.png|600]]

#### 補充
+ 網頁顯示結果
![[Pasted image 20241017000129.png]]