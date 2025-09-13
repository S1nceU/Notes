1. 下載並安裝 Python 環境
	1. 到 Python 官網取得 Python 3.13.7 安裝包 ([Python Release Python 3.13.7 | Python.org](https://www.python.org/downloads/release/python-3137/))![[Pasted image 20250913180002.png]]
	2.  幫我注意下面的**環境變數選項記得勾選**
		![[Pasted image 20250913180108.png|300]]
	3. 打開 CMD，打上```python --version```，測試是否安裝成功
		![[Pasted image 20250913180319.png]]
2. 安裝 Visual Studio Code
	1. 到 Visual Studio Code 官網取得安裝包 ([Visual Studio Code - Code Editing. Redefined](https://code.visualstudio.com/))
	2. 安裝過程幾乎都是下一步，但可以注意在圖片中的選項，**下面四個選項勾選**![[Pasted image 20250913180638.png|300]]
3. 開始實作
	1. 從資料夾按右鍵就可以打開專案，或是透過 Visual Studio Code 的預設頁面開啟資料夾
		![[Pasted image 20250913181012.png|300]]![[Pasted image 20250913181032.png|300]]
	2. 安裝擴充套件
		![[Pasted image 20250913181135.png||30]]
		可以透過搜尋安裝擴充套件，以下是建議的擴充套件		![[Pasted image 20250913181300.png]]![[Pasted image 20250913181344.png]]
	3. 會用到的函式庫
		- numpy **( 因為它會含在 matplotlib 的必要函式庫中，所以不用多次安裝它 )**
		- pandas
		- matplotlib
		- requests
	4. 安裝函式庫
		按下```Ctrl + ~```，打開終端，並複製下面指令到終端上執行
		```bash
		pip install matplotlib pandas requests
		```
		安裝會印出 Successfully installed 等提示 ![[Pasted image 20250913182517.png]]
	5. 開始進行實驗
		1. 他會先請你選擇環境
		![[Pasted image 20250913182832.png]]
		2. 你可以直接選擇剛剛安裝的 **Python 3.13.7**![[Pasted image 20250913182855.png]]
		3. 用法則會跟上次課程的 Jupyter 用法相同
			![[Pasted image 20250913183021.png]]
			