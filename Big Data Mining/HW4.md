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
	+ 執行 spark-class org.apache.spark.deply.worker.Worker spark://192.168.18.2:7077 

### 資料夾結構
- homework4.py
- ml-1m
	- all of datasets 
- output
	- all of output csv
### 源代碼
![[Pasted image 20241223174459.png|500]]
![[Pasted image 20241223174521.png|500]]
![[Pasted image 20241223174539.png|500]]
![[Pasted image 20241223174556.png|500]]
![[Pasted image 20241223174621.png|500]]

### 輸出結果
+ 執行 spark-submit --master spark://192.168.18.2:7077 --conf spark.driver.host=192.168.18.2 homework4.py
#### Spark Master Web
![[Pasted image 20241223173543.png|800]]
#### Spark Cluster Web
![[Pasted image 20241223173524.png|800]]
#### 第一題
- List the top-rated movies by all users
- Output：Q1_top_rated_movies.csv
![[Pasted image 20241223175140.png|200]]
#### 第二題
- List the top-rated movies grouped by gender, age group, and occupation, respectively
- Output：
	- Age：Q2_top_movies_by_age.csv
	![[Pasted image 20241223175319.png|200]]
	- Gender：Q2_top_movies_by_gender.csv
	![[Pasted image 20241223175412.png|200]]
	- Occupation：Q2_top_movies_by_occupation.csv
	![[Pasted image 20241223175506.png|200]]
#### 第三題
- Calculate the average rating score of each user for all movies and group by genre, respectively
- Output：
	- User average rating
	![[Pasted image 20241223175743.png|200]]
	- Genre average rating
	![[Pasted image 20241223175832.png|200]]
#### 第四題
- Given any user, list the top ‘similar’ users based on the cosine similarity of the ratings each user has given (sorted in descending order of ‘user’ similarity score).
- UserID：3446
- Output：Q4_similar_users_to_user_3446.csv
![[Pasted image 20241223175949.png|300]]
#### 第五題
- Given any movie, list the top ‘similar’ movies based on the cosine similarity of the ratings each movie received (sorted in descending order of ‘movie’ similarity score).
- MovieID：3446
- Output：Q5_similar_movies_to_movie_3446.csv
![[Pasted image 20241223180058.png|300]]
### 小組分工

|    學號     | 姓名  | 貢獻比例 |     工作內容      |
| :-------: | :-: | :--: | :-----------: |
| 113598007 | 楊明哲 | 100% | 程式編寫、除錯以及文書處理 |

