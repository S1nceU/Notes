## Google Bert

```
F1 Scores
INFO:__main__:omittable: 0.9288
INFO:__main__:measure: 0.9048
INFO:__main__:extension: 0.8636
INFO:__main__:atelectasis: 0.6667
INFO:__main__:satellite: 0.6571
INFO:__main__:lymphadenopathy: 0.9610
INFO:__main__:pleural: 0.9250
INFO:__main__:distant: 0.8378
INFO:__main__:Overall: 0.8814
F2 Scores
INFO:__main__:omittable: 0.9232
INFO:__main__:measure: 0.8796
INFO:__main__:extension: 0.8520
INFO:__main__:atelectasis: 0.5914
INFO:__main__:satellite: 0.7188
INFO:__main__:lymphadenopathy: 0.9536
INFO:__main__:pleural: 0.9250
INFO:__main__:distant: 0.8115
INFO:__main__:Overall: 0.8725
	
Classification Report:
                 precision    recall  f1-score   support

      omittable       0.94      0.92      0.93       149
        measure       0.95      0.86      0.90        88
      extension       0.88      0.84      0.86        45
    atelectasis       0.85      0.55      0.67        20
      satellite       0.57      0.77      0.66        30
lymphadenopathy       0.97      0.95      0.96        39
        pleural       0.93      0.93      0.93        40
        distant       0.89      0.79      0.84        39

      micro avg       0.90      0.87      0.88       450
      macro avg       0.87      0.83      0.84       450
   weighted avg       0.90      0.87      0.88       450
    samples avg       0.90      0.88      0.89       450
```

### Freeze first 6 layers
```
F1 Scores:
INFO:__main__:omittable: 0.9295
INFO:__main__:measure: 0.9157
INFO:__main__:extension: 0.8602
INFO:__main__:atelectasis: 0.8000
INFO:__main__:satellite: 0.8077
INFO:__main__:lymphadenopathy: 0.9367
INFO:__main__:pleural: 0.8571
INFO:__main__:distant: 0.6970
INFO:__main__:Overall: 0.8843
F2 Scores:
INFO:__main__:omittable: 0.9552
INFO:__main__:measure: 0.8837
INFO:__main__:extension: 0.8772
INFO:__main__:atelectasis: 0.7368
INFO:__main__:satellite: 0.7394
INFO:__main__:lymphadenopathy: 0.9439
INFO:__main__:pleural: 0.7895
INFO:__main__:distant: 0.6284
INFO:__main__:Overall: 0.8682

Classification Report:
                 precision    recall  f1-score   support

      omittable       0.89      0.97      0.93       149
        measure       0.97      0.86      0.92        88
      extension       0.83      0.89      0.86        45
    atelectasis       0.93      0.70      0.80        20
      satellite       0.95      0.70      0.81        30
lymphadenopathy       0.93      0.95      0.94        39
        pleural       1.00      0.75      0.86        40
        distant       0.85      0.59      0.70        39

      micro avg       0.91      0.86      0.88       450
      macro avg       0.92      0.80      0.85       450
   weighted avg       0.92      0.86      0.88       450
    samples avg       0.89      0.88      0.88       450
```


-------
## BioClinical BERT
```
F1 Scores:
INFO:__main__:omittable: 0.9428
INFO:__main__:measure: 0.8953
INFO:__main__:extension: 0.8889
INFO:__main__:atelectasis: 0.7059
INFO:__main__:satellite: 0.6667
INFO:__main__:lymphadenopathy: 0.9114
INFO:__main__:pleural: 0.9367
INFO:__main__:distant: 0.8611
INFO:__main__:Overall: 0.8909
F2 Scores:
INFO:__main__:omittable: 0.9409
INFO:__main__:measure: 0.8830
INFO:__main__:extension: 0.8889
INFO:__main__:atelectasis: 0.6383
INFO:__main__:satellite: 0.6463
INFO:__main__:lymphadenopathy: 0.9184
INFO:__main__:pleural: 0.9296
INFO:__main__:distant: 0.8201
INFO:__main__:Overall: 0.8789

Classification Report:
                 precision    recall  f1-score   support

      omittable       0.95      0.94      0.94       149
        measure       0.92      0.88      0.90        88
      extension       0.89      0.89      0.89        45
    atelectasis       0.86      0.60      0.71        20
      satellite       0.70      0.63      0.67        30
lymphadenopathy       0.90      0.92      0.91        39
        pleural       0.95      0.93      0.94        40
        distant       0.94      0.79      0.86        39

      micro avg       0.91      0.87      0.89       450
      macro avg       0.89      0.82      0.85       450
   weighted avg       0.91      0.87      0.89       450
    samples avg       0.91      0.89      0.89       450
```

### Freeze first 6 layers
```
F1 Scores:
INFO:__main__:omittable: 0.9351
INFO:__main__:measure: 0.9036
INFO:__main__:extension: 0.8687
INFO:__main__:atelectasis: 0.6875
INFO:__main__:satellite: 0.7925
INFO:__main__:lymphadenopathy: 0.9487
INFO:__main__:pleural: 0.8767
INFO:__main__:distant: 0.8060
INFO:__main__:Overall: 0.8904
F2 Scores:
INFO:__main__:omittable: 0.9536
INFO:__main__:measure: 0.8721
INFO:__main__:extension: 0.9188
INFO:__main__:atelectasis: 0.5978
INFO:__main__:satellite: 0.7343
INFO:__main__:lymphadenopathy: 0.9487
INFO:__main__:pleural: 0.8290
INFO:__main__:distant: 0.7337
INFO:__main__:Overall: 0.8760

Classification Report:
                 precision    recall  f1-score   support

      omittable       0.91      0.97      0.94       149
        measure       0.96      0.85      0.90        88
      extension       0.80      0.96      0.87        45
    atelectasis       0.92      0.55      0.69        20
      satellite       0.91      0.70      0.79        30
lymphadenopathy       0.95      0.95      0.95        39
        pleural       0.97      0.80      0.88        40
        distant       0.96      0.69      0.81        39

      micro avg       0.92      0.87      0.89       450
      macro avg       0.92      0.81      0.85       450
   weighted avg       0.92      0.87      0.89       450
    samples avg       0.90      0.88      0.89       450
```


-------

## BioBERT
```
F1 Scores:
INFO:__main__:omittable: 0.9342
INFO:__main__:measure: 0.8929
INFO:__main__:extension: 0.8193
INFO:__main__:atelectasis: 0.5185
INFO:__main__:satellite: 0.7547
INFO:__main__:lymphadenopathy: 0.9367
INFO:__main__:pleural: 0.9351
INFO:__main__:distant: 0.9474
INFO:__main__:Overall: 0.8927
F2 Scores:
INFO:__main__:omittable: 0.9454
INFO:__main__:measure: 0.8681
INFO:__main__:extension: 0.7798
INFO:__main__:atelectasis: 0.4023
INFO:__main__:satellite: 0.6993
INFO:__main__:lymphadenopathy: 0.9439
INFO:__main__:pleural: 0.9137
INFO:__main__:distant: 0.9326
INFO:__main__:Overall: 0.8728
	
Classification Report:
                 precision    recall  f1-score   support

      omittable       0.92      0.95      0.93       149
        measure       0.94      0.85      0.89        88
      extension       0.89      0.76      0.82        45
    atelectasis       1.00      0.35      0.52        20
      satellite       0.87      0.67      0.75        30
lymphadenopathy       0.93      0.95      0.94        39
        pleural       0.97      0.90      0.94        40
        distant       0.97      0.92      0.95        39

      micro avg       0.93      0.86      0.89       450
      macro avg       0.94      0.79      0.84       450
   weighted avg       0.93      0.86      0.89       450
    samples avg       0.91      0.88      0.89       450
```

### Freeze first 6 layers
```
F1 Scores:
INFO:__main__:omittable: 0.9439
INFO:__main__:measure: 0.9024
INFO:__main__:extension: 0.9111
INFO:__main__:atelectasis: 0.7273
INFO:__main__:satellite: 0.6984
INFO:__main__:lymphadenopathy: 0.9487
INFO:__main__:pleural: 0.9189
INFO:__main__:distant: 0.8611
INFO:__main__:Overall: 0.8985
F2 Scores:
INFO:__main__:omittable: 0.9533
INFO:__main__:measure: 0.8645
INFO:__main__:extension: 0.9111
INFO:__main__:atelectasis: 0.6452
INFO:__main__:satellite: 0.7190
INFO:__main__:lymphadenopathy: 0.9487
INFO:__main__:pleural: 0.8763
INFO:__main__:distant: 0.8201
INFO:__main__:Overall: 0.8846

Classification Report:
                 precision    recall  f1-score   support

      omittable       0.93      0.96      0.94       149
        measure       0.97      0.84      0.90        88
      extension       0.91      0.91      0.91        45
    atelectasis       0.92      0.60      0.73        20
      satellite       0.67      0.73      0.70        30
lymphadenopathy       0.95      0.95      0.95        39
        pleural       1.00      0.85      0.92        40
        distant       0.94      0.79      0.86        39

      micro avg       0.92      0.88      0.90       450
      macro avg       0.91      0.83      0.86       450
   weighted avg       0.93      0.88      0.90       450
    samples avg       0.91      0.90      0.90       450
```


-------
## BiomedBERT
```
F1 Scores:
INFO:__main__:omittable: 0.9346
INFO:__main__:measure: 0.9123
INFO:__main__:extension: 0.8864
INFO:__main__:atelectasis: 0.8235
INFO:__main__:satellite: 0.7000
INFO:__main__:lymphadenopathy: 0.9367
INFO:__main__:pleural: 0.8060
INFO:__main__:distant: 0.7778
INFO:__main__:Overall: 0.8826
F2 Scores:
INFO:__main__:omittable: 0.9495
INFO:__main__:measure: 0.8966
INFO:__main__:extension: 0.8744
INFO:__main__:atelectasis: 0.7447
INFO:__main__:satellite: 0.7000
INFO:__main__:lymphadenopathy: 0.9439
INFO:__main__:pleural: 0.7219
INFO:__main__:distant: 0.7407
INFO:__main__:Overall: 0.8689

Classification Report:
                 precision    recall  f1-score   support

      omittable       0.91      0.96      0.93       149
        measure       0.94      0.89      0.91        88
      extension       0.91      0.87      0.89        45
    atelectasis       1.00      0.70      0.82        20
      satellite       0.70      0.70      0.70        30
lymphadenopathy       0.93      0.95      0.94        39
        pleural       1.00      0.68      0.81        40
        distant       0.85      0.72      0.78        39

      micro avg       0.91      0.86      0.88       450
      macro avg       0.90      0.81      0.85       450
   weighted avg       0.91      0.86      0.88       450
    samples avg       0.90      0.88      0.88       450
```

### Freeze first 6 layers
```
F1 Scores:
INFO:__main__:omittable: 0.9439
INFO:__main__:measure: 0.9302
INFO:__main__:extension: 0.8667
INFO:__main__:atelectasis: 0.7059
INFO:__main__:satellite: 0.7778
INFO:__main__:lymphadenopathy: 0.9231
INFO:__main__:pleural: 0.8947
INFO:__main__:distant: 0.7945
INFO:__main__:Overall: 0.8955
F2 Scores:
INFO:__main__:omittable: 0.9533
INFO:__main__:measure: 0.9174
INFO:__main__:extension: 0.8667
INFO:__main__:atelectasis: 0.6383
INFO:__main__:satellite: 0.7292
INFO:__main__:lymphadenopathy: 0.9231
INFO:__main__:pleural: 0.8673
INFO:__main__:distant: 0.7632
INFO:__main__:Overall: 0.8834

Classification Report:
                 precision    recall  f1-score   support

      omittable       0.93      0.96      0.94       149
        measure       0.95      0.91      0.93        88
      extension       0.87      0.87      0.87        45
    atelectasis       0.86      0.60      0.71        20
      satellite       0.88      0.70      0.78        30
lymphadenopathy       0.92      0.92      0.92        39
        pleural       0.94      0.85      0.89        40
        distant       0.85      0.74      0.79        39

      micro avg       0.92      0.88      0.90       450
      macro avg       0.90      0.82      0.85       450
   weighted avg       0.91      0.88      0.89       450
    samples avg       0.90      0.89      0.89       450
```
---
## RadBERT

```
INFO:__main__:F1 Scores:
INFO:__main__:omittable: 0.9377
INFO:__main__:measure: 0.8743
INFO:__main__:extension: 0.8736
INFO:__main__:atelectasis: 0.8500
INFO:__main__:satellite: 0.7742
INFO:__main__:lymphadenopathy: 0.9367
INFO:__main__:pleural: 0.9067
INFO:__main__:distant: 0.7941
INFO:__main__:Overall: 0.8901
INFO:__main__:F2 Scores:
INFO:__main__:omittable: 0.9508
INFO:__main__:measure: 0.8469
INFO:__main__:extension: 0.8559
INFO:__main__:atelectasis: 0.8500
INFO:__main__:satellite: 0.7895
INFO:__main__:lymphadenopathy: 0.9439
INFO:__main__:pleural: 0.8718
INFO:__main__:distant: 0.7297
INFO:__main__:Overall: 0.8800
INFO:__main__:Classification Report:

                 precision    recall  f1-score   support

      omittable       0.92      0.96      0.94       149
        measure       0.92      0.83      0.87        88
      extension       0.90      0.84      0.87        45
    atelectasis       0.85      0.85      0.85        20
      satellite       0.75      0.80      0.77        30
lymphadenopathy       0.93      0.95      0.94        39
        pleural       0.97      0.85      0.91        40
        distant       0.93      0.69      0.79        39

      micro avg       0.91      0.87      0.89       450
      macro avg       0.90      0.85      0.87       450
   weighted avg       0.91      0.87      0.89       450
    samples avg       0.90      0.89      0.89       450

```
### Freeze first 6 layers
```
F1 Scores:
	omittable: 0.9295
	measure: 0.8966
	extension: 0.8352
	atelectasis: 0.8947
	satellite: 0.6923
	lymphadenopathy: 0.9487
	pleural: 0.8974
	distant: 0.7826
	Overall: 0.8857
F2 Scores:
	omittable: 0.9552
	measure: 0.8904
	extension: 0.8407
	atelectasis: 0.8673
	satellite: 0.6338
	lymphadenopathy: 0.9487
	pleural: 0.8838
	distant: 0.7258
	Overall: 0.8809
	
Classification Report:

                 precision    recall  f1-score   support

      omittable       0.89      0.97      0.93       149
        measure       0.91      0.89      0.90        88
      extension       0.83      0.84      0.84        45
    atelectasis       0.94      0.85      0.89        20
      satellite       0.82      0.60      0.69        30
lymphadenopathy       0.95      0.95      0.95        39
        pleural       0.92      0.88      0.90        40
        distant       0.90      0.69      0.78        39

      micro avg       0.89      0.88      0.89       450
      macro avg       0.89      0.83      0.86       450
   weighted avg       0.89      0.88      0.88       450
    samples avg       0.90      0.89      0.89       450
```


### Framework: Hierarchical Multi-Label Classification for Lung Cancer Staging

---

## 📝 **設計建議**

### 1. **數據輸入層（Data Input Layer）**
   - **Radiology Report 文本：** 作為主要數據來源。
   - **示例文本：** 
     

     A tumor with a diameter of 12 cm is observed... Bilateral hilar lymph nodes are enlarged...
     

   - 使用圖標（如放射影像）和**標註的文本框**表示數據源。

### 2. **特徵提取層（Feature Extraction Layer）**
   - 根據不同的 **TNM 特徵**，將文本分成三個子任務：
     - **T-Feature（腫瘤大小與侵襲）：**  
       - **特徵示例：**  
         - Pleural Infiltration：`Destruction of the left 3rd rib`
         - Measure：`diameter of 12 cm`  
         
     - **N-Feature（淋巴結轉移）：**  
       - **特徵示例：**  
         - Pleural Infiltration：`Destruction of the left 3rd rib`
         - Lymphadenopathy：`Bilateral hilar lymph nodes are enlarged`
          - Satellite：`Satellite lesion`

     - **M-Feature（遠處轉移）：**  
       - **特徵示例：**  
         - Distant：`No distant metastasis`  
         - Extension：`Chest wall invasion`
        
   - 使用**綠色、橙色、紫色**的標籤清晰區分 T、N、M 的特徵提取子任務。

### 3. **特徵合併層（Feature Aggregation Layer）**
   - 將所有子任務的特徵合併，形成一個完整的子文章。
   - **示例：**  
T: 合併
- Pleural Infiltration：`Destruction of the left 3rd rib`
- Measure：`diameter of 12 cm`  

Ｎ: 合併
- Pleural Infiltration：`Destruction of the left 3rd rib`
- Lymphadenopathy：`Bilateral hilar lymph nodes are enlarged`
- Satellite：`Satellite lesion`

M: 合併
- Distant：`No distant metastasis`  
- Extension：`Chest wall invasion`

   - 可以用 **箭頭** 連接子特徵到主文本，表示特徵融合過程。

### 4. **主任務分期層（Main Task Staging Layer）**
   - **模型輸出：**  

     T: T2a  
     N: N3  
     M: M0  
     
   - 將輸出用 **彩色框標註**，並與原始文本進行對齊，清楚展示分期結果。

### 5. **整體視覺化結構**

1. **頂部：** Radiology Report 圖示與文本輸入。  
2. **中部：** 分三條路徑分別進行 **T、N、M 特徵提取**。  
3. **底部：** 三條路徑匯聚到一個 **Main Task 分期模型**，產生最終 TNM 分期預測。  

---

## 🔧 **設計細節調整建議**
1. **顏色編碼：**  
   - T 特徵與輸出：**綠色**  
   - N 特徵與輸出：**橙色**  
   - M 特徵與輸出：**紫色**  

2. **箭頭指向：**  
   - 使用箭頭清楚連接從 **Subtask 層** 到 **Main Task 層** 的數據流向。  

3. **示例標註：**  
   - 在圖中標註具體的文本片段，增強直觀理解。

這樣的調整可以使整個框架更具體且易於理解，展示出從放射報告到 TNM 分期預測的完整流程。