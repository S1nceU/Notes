## Google Bert

```
F1 Scores
	measure: 0.8191
	extension: 0.8913
	atelectasis: 0.6452
	satellite: 0.6275
	lymphadenopathy: 0.8315
	pleural: 0.8611
	distant: 0.5862
	Overall: 0.7883
F2 Scores
	measure: 0.8518
	extension: 0.9031
	atelectasis: 0.5495
	satellite: 0.5674
	lymphadenopathy: 0.8981
	pleural: 0.8073
	distant: 0.4857
	Overall: 0.7716
	
Classification Report:
                 precision    recall  f1-score   support

        measure       0.77      0.88      0.82        88
      extension       0.87      0.91      0.89        45
    atelectasis       0.91      0.50      0.65        20
      satellite       0.76      0.53      0.63        30
lymphadenopathy       0.74      0.95      0.83        39
        pleural       0.97      0.78      0.86        40
        distant       0.89      0.44      0.59        39

      micro avg       0.82      0.76      0.79       301
      macro avg       0.85      0.71      0.75       301
   weighted avg       0.83      0.76      0.78       301
    samples avg       0.49      0.50      0.49       301
```
![[Google_BERT_training_history 1.png]]
### Freeze first 6 layers
```
F1 Scores:
	measure: 0.8939
	extension: 0.7907
	atelectasis: 0.0000
	satellite: 0.0000
	lymphadenopathy: 0.9014
	pleural: 0.7302
	distant: 0.5000
	Overall: 0.7265
F2 Scores:
	measure: 0.9029
	extension: 0.7692
	atelectasis: 0.0000
	satellite: 0.0000
	lymphadenopathy: 0.8511
	pleural: 0.6284
	distant: 0.3846
	Overall: 0.6481

Classification Report:
                 precision    recall  f1-score   support

        measure       0.88      0.91      0.89        88
      extension       0.83      0.76      0.79        45
    atelectasis       0.00      0.00      0.00        20
      satellite       0.00      0.00      0.00        30
lymphadenopathy       1.00      0.82      0.90        39
        pleural       1.00      0.57      0.73        40
        distant       1.00      0.33      0.50        39

      micro avg       0.91      0.60      0.73       301
      macro avg       0.67      0.48      0.55       301
   weighted avg       0.77      0.60      0.66       301
    samples avg       0.42      0.40      0.41       301
```
![[Google_BERT_freeze_6_training_history 1.png]]

-------
## BioBERT
```
F1 Scores:
	measure: 0.8970
	extension: 0.8636
	atelectasis: 0.7692
	satellite: 0.7719
	lymphadenopathy: 0.9600
	pleural: 0.8235
	distant: 0.7778
	Overall: 0.8546
F2 Scores:
	measure: 0.8625
	extension: 0.8520
	atelectasis: 0.7576
	satellite: 0.7483
	lymphadenopathy: 0.9375
	pleural: 0.7447
	distant: 0.7407
	Overall: 0.8214
	
Classification Report:
                 precision    recall  f1-score   support

        measure       0.96      0.84      0.90        88
      extension       0.88      0.84      0.86        45
    atelectasis       0.79      0.75      0.77        20
      satellite       0.81      0.73      0.77        30
lymphadenopathy       1.00      0.92      0.96        39
        pleural       1.00      0.70      0.82        40
        distant       0.85      0.72      0.78        39

      micro avg       0.92      0.80      0.85       301
      macro avg       0.90      0.79      0.84       301
   weighted avg       0.92      0.80      0.85       301
    samples avg       0.54      0.52      0.53       301
```
![[BioBERT_training_history.png]]
### Freeze first 6 layers
```
F1 Scores:
	measure: 0.8957
	extension: 0.8736
	atelectasis: 0.6875
	satellite: 0.8276
	lymphadenopathy: 0.9487
	pleural: 0.8889
	distant: 0.7813
	Overall: 0.8664
F2 Scores:
	measure: 0.8548
	extension: 0.8559
	atelectasis: 0.5978
	satellite: 0.8108
	lymphadenopathy: 0.9487
	pleural: 0.8333
	distant: 0.6906
	Overall: 0.8236

Classification Report:
                 precision    recall  f1-score   support

        measure       0.97      0.83      0.90        88
      extension       0.90      0.84      0.87        45
    atelectasis       0.92      0.55      0.69        20
      satellite       0.86      0.80      0.83        30
lymphadenopathy       0.95      0.95      0.95        39
        pleural       1.00      0.80      0.89        40
        distant       1.00      0.64      0.78        39

      micro avg       0.95      0.80      0.87       301
      macro avg       0.94      0.77      0.84       301
   weighted avg       0.95      0.80      0.86       301
    samples avg       0.55      0.53      0.53       301
```
![[BioBERT_freeze_6_training_history.png]]

-------
## BioClinical BERT
```
F1 Scores:
	measure: 0.9059
	extension: 0.9231
	atelectasis: 0.7647
	satellite: 0.7778
	lymphadenopathy: 0.9600
	pleural: 0.8451
	distant: 0.7879
	Overall: 0.8734
F2 Scores:
	measure: 0.8871
	extension: 0.9292
	atelectasis: 0.6915
	satellite: 0.7292
	lymphadenopathy: 0.9375
	pleural: 0.7853
	distant: 0.7104
	Overall: 0.8367

Classification Report:
                 precision    recall  f1-score   support

        measure       0.94      0.88      0.91        88
      extension       0.91      0.93      0.92        45
    atelectasis       0.93      0.65      0.76        20
      satellite       0.88      0.70      0.78        30
lymphadenopathy       1.00      0.92      0.96        39
        pleural       0.97      0.75      0.85        40
        distant       0.96      0.67      0.79        39

      micro avg       0.94      0.81      0.87       301
      macro avg       0.94      0.79      0.85       301
   weighted avg       0.94      0.81      0.87       301
    samples avg       0.54      0.53      0.54       301
```
![[Bio_ClinicalBERT_training_history.png]]
### Freeze first 6 layers
```
F1 Scores:
	measure: 0.8889
	extension: 0.8966
	atelectasis: 0.6667
	satellite: 0.7812
	lymphadenopathy: 0.9367
	pleural: 0.8406
	distant: 0.8611
	Overall: 0.8622
F2 Scores:
	measure: 0.8451
	extension: 0.8784
	atelectasis: 0.5914
	satellite: 0.8117
	lymphadenopathy: 0.9439
	pleural: 0.7672
	distant: 0.8201
	Overall: 0.8305

Classification Report:
                 precision    recall  f1-score   support

        measure       0.97      0.82      0.89        88
      extension       0.93      0.87      0.90        45
    atelectasis       0.85      0.55      0.67        20
      satellite       0.74      0.83      0.78        30
lymphadenopathy       0.93      0.95      0.94        39
        pleural       1.00      0.72      0.84        40
        distant       0.94      0.79      0.86        39

      micro avg       0.92      0.81      0.86       301
      macro avg       0.91      0.79      0.84       301
   weighted avg       0.93      0.81      0.86       301
    samples avg       0.55      0.53      0.54       301
```
![[Bio_ClinicalBERT_freeze_6_training_history.png]]

-------
## BiomedBERT
```
F1 Scores:
	measure: 0.9080
	extension: 0.8315
	atelectasis: 0.7778
	satellite: 0.7333
	lymphadenopathy: 0.9231
	pleural: 0.8235
	distant: 0.7606
	Overall: 0.8438
F2 Scores:
	measure: 0.9018
	extension: 0.8259
	atelectasis: 0.7292
	satellite: 0.7333
	lymphadenopathy: 0.9231
	pleural: 0.7447
	distant: 0.7181
	Overall: 0.8215

Classification Report:
                 precision    recall  f1-score   support

        measure       0.92      0.90      0.91        88
      extension       0.84      0.82      0.83        45
    atelectasis       0.88      0.70      0.78        20
      satellite       0.73      0.73      0.73        30
lymphadenopathy       0.92      0.92      0.92        39
        pleural       1.00      0.70      0.82        40
        distant       0.84      0.69      0.76        39

      micro avg       0.88      0.81      0.84       301
      macro avg       0.88      0.78      0.82       301
   weighted avg       0.89      0.81      0.84       301
    samples avg       0.56      0.53      0.54       301
```
![[BiomedBERT_training_history.png]]
### Freeze first 6 layers
```
F1 Scores:
	measure: 0.9070
	extension: 0.7961
	atelectasis: 0.8108
	satellite: 0.6897
	lymphadenopathy: 0.9487
	pleural: 0.8286
	distant: 0.8116
	Overall: 0.8450
F2 Scores:
	measure: 0.8945
	extension: 0.8613
	atelectasis: 0.7732
	satellite: 0.6757
	lymphadenopathy: 0.9487
	pleural: 0.7632
	distant: 0.7527
	Overall: 0.8322

Classification Report:
                 precision    recall  f1-score   support

        measure       0.93      0.89      0.91        88
      extension       0.71      0.91      0.80        45
    atelectasis       0.88      0.75      0.81        20
      satellite       0.71      0.67      0.69        30
lymphadenopathy       0.95      0.95      0.95        39
        pleural       0.97      0.72      0.83        40
        distant       0.93      0.72      0.81        39

      micro avg       0.87      0.82      0.84       301
      macro avg       0.87      0.80      0.83       301
   weighted avg       0.88      0.82      0.84       301
    samples avg       0.55      0.54      0.54       301
```
![[BiomedBERT_freeze_6_training_history.png]]

```
INFO:__main__:F1 Scores:
INFO:__main__:measure: 0.9222
INFO:__main__:extension: 0.8261
INFO:__main__:atelectasis: 0.7429
INFO:__main__:satellite: 0.7347
INFO:__main__:lymphadenopathy: 0.9487
INFO:__main__:pleural: 0.3333
INFO:__main__:distant: 0.0500
INFO:__main__:Overall: 0.7544
INFO:__main__:F2 Scores:
INFO:__main__:measure: 0.8933
INFO:__main__:extension: 0.8370
INFO:__main__:atelectasis: 0.6842
INFO:__main__:satellite: 0.6475
INFO:__main__:lymphadenopathy: 0.9487
INFO:__main__:pleural: 0.2381
INFO:__main__:distant: 0.0318
INFO:__main__:Overall: 0.6799
INFO:__main__:Classification Report:

                 precision    recall  f1-score   support

        measure       0.97      0.88      0.92        88
      extension       0.81      0.84      0.83        45
    atelectasis       0.87      0.65      0.74        20
      satellite       0.95      0.60      0.73        30
lymphadenopathy       0.95      0.95      0.95        39
        pleural       1.00      0.20      0.33        40
        distant       1.00      0.03      0.05        39

      micro avg       0.92      0.64      0.75       301
      macro avg       0.94      0.59      0.65       301
   weighted avg       0.94      0.64      0.69       301
    samples avg       0.43      0.41      0.42       301

```

```
INFO:__main__:F1 Scores:
INFO:__main__:measure: 0.9091
INFO:__main__:extension: 0.8571
INFO:__main__:atelectasis: 0.7895
INFO:__main__:satellite: 0.7308
INFO:__main__:lymphadenopathy: 0.9367
INFO:__main__:pleural: 0.8649
INFO:__main__:distant: 0.7941
INFO:__main__:Overall: 0.8616
INFO:__main__:F2 Scores:
INFO:__main__:measure: 0.9091
INFO:__main__:extension: 0.8628
INFO:__main__:atelectasis: 0.7653
INFO:__main__:satellite: 0.6690
INFO:__main__:lymphadenopathy: 0.9439
INFO:__main__:pleural: 0.8247
INFO:__main__:distant: 0.7297
INFO:__main__:Overall: 0.8406
INFO:__main__:Classification Report:
c:\Users\Zhe\miniconda3\envs\moead\lib\site-packages\sklearn\metrics\_classification.py:1248: UndefinedMetricWarning: Precision and F-score are ill-defined and being set to 0.0 in samples with no predicted labels. Use `zero_division` parameter to control this behavior.
  _warn_prf(average, modifier, msg_start, len(result))
c:\Users\Zhe\miniconda3\envs\moead\lib\site-packages\sklearn\metrics\_classification.py:1248: UndefinedMetricWarning: Recall and F-score are ill-defined and being set to 0.0 in samples with no true labels. Use `zero_division` parameter to control this behavior.
  _warn_prf(average, modifier, msg_start, len(result))
INFO:__main__:
                 precision    recall  f1-score   support

        measure       0.91      0.91      0.91        88
      extension       0.85      0.87      0.86        45
    atelectasis       0.83      0.75      0.79        20
      satellite       0.86      0.63      0.73        30
lymphadenopathy       0.93      0.95      0.94        39
        pleural       0.94      0.80      0.86        40
        distant       0.93      0.69      0.79        39

      micro avg       0.90      0.83      0.86       301
      macro avg       0.89      0.80      0.84       301
   weighted avg       0.90      0.83      0.86       301
    samples avg       0.56      0.54      0.54       301

INFO:__main__:Predictions saved to Datasets/predict_csv\RadBERT_freeze_6_submission.csv
INFO:__main__:       id  sentence_index  measure  extension  atelectasis  satellite  \
0  147290               0        1          0            0          0   
1  147290               1        0          0            0          0   
2  147290               2        0          0            0          0   
3  147290               3        0          0            0          0   
4  241752               0        1          0            0          0   

   lymphadenopathy  pleural  distant  
0                0        0        0  
1                0        0        0  
2                0        0        0  
3                0        0        0  
4                0        0        0  
INFO:__main__:           id  sentence_index
0      147290               0
1      147290               1
2      147290               2
3      147290               3
4      241752               0
..        ...             ...
410  16572985               1
411  16572985               2
412  16572985               3
413  16572985               4
414  16572985               5

[415 rows x 2 columns]

```