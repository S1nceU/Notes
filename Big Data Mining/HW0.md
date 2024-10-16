## 資工碩一 113598007 楊明哲

### 環境設置
1. **Windows**
	+ 環境版本
		+ Java: 17.0.12
		+ Python: 3.10.10
		+ PySpark: 3.5.3
		+ Pandas: 2.2.3
	+ 環境配置
		+ 下載指定版本的 Python 和 Java
		+ 透過 pip install pyspark 安裝 pyspark，並且將它初始化
		+ 將下載的 winutils.exe 放進 pyspark 中 bin 的資料夾下
		+ 設置環境變數
			+ PYSPARK_PYTHON![[Pasted image 20241016232913.png]]
			+ PYSPARK_DRIVER_PYTHON![[Pasted image 20241016232557.png]]
			+ Path ![[Pasted image 20241016232947.png]]
		+ 執行指令 spark-class org.apache.spark.deploy.master.Master，並打開 URL
		+ 
