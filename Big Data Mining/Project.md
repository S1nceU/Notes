## Google Bert

```
INFO:__main__:F1 Scores:
INFO:__main__:measure: 0.8191
INFO:__main__:extension: 0.8913
INFO:__main__:atelectasis: 0.6452
INFO:__main__:satellite: 0.6275
INFO:__main__:lymphadenopathy: 0.8315
INFO:__main__:pleural: 0.8611
INFO:__main__:distant: 0.5862
INFO:__main__:Overall: 0.7883
INFO:__main__:F2 Scores:
INFO:__main__:measure: 0.8518
INFO:__main__:extension: 0.9031
INFO:__main__:atelectasis: 0.5495
INFO:__main__:satellite: 0.5674
INFO:__main__:lymphadenopathy: 0.8981
INFO:__main__:pleural: 0.8073
INFO:__main__:distant: 0.4857
INFO:__main__:Overall: 0.7716
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

### Freeze first 6 layers
```
INFO:__main__:F1 Scores:
INFO:__main__:measure: 0.8939
INFO:__main__:extension: 0.7907
INFO:__main__:atelectasis: 0.0000
INFO:__main__:satellite: 0.0000
INFO:__main__:lymphadenopathy: 0.9014
INFO:__main__:pleural: 0.7302
INFO:__main__:distant: 0.5000
INFO:__main__:Overall: 0.7265
INFO:__main__:F2 Scores:
INFO:__main__:measure: 0.9029
INFO:__main__:extension: 0.7692
INFO:__main__:atelectasis: 0.0000
INFO:__main__:satellite: 0.0000
INFO:__main__:lymphadenopathy: 0.8511
INFO:__main__:pleural: 0.6284
INFO:__main__:distant: 0.3846
INFO:__main__:Overall: 0.6481

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


-------
## BioBERT
```
INFO:__main__:F1 Scores:
INFO:__main__:measure: 0.8970
INFO:__main__:extension: 0.8636
INFO:__main__:atelectasis: 0.7692
INFO:__main__:satellite: 0.7719
INFO:__main__:lymphadenopathy: 0.9600
INFO:__main__:pleural: 0.8235
INFO:__main__:distant: 0.7778
INFO:__main__:Overall: 0.8546
INFO:__main__:F2 Scores:
INFO:__main__:measure: 0.8625
INFO:__main__:extension: 0.8520
INFO:__main__:atelectasis: 0.7576
INFO:__main__:satellite: 0.7483
INFO:__main__:lymphadenopathy: 0.9375
INFO:__main__:pleural: 0.7447
INFO:__main__:distant: 0.7407
INFO:__main__:Overall: 0.8214
	
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

### Freeze first 6 layers
```
INFO:__main__:F1 Scores:
INFO:__main__:measure: 0.8957
INFO:__main__:extension: 0.8736
INFO:__main__:atelectasis: 0.6875
INFO:__main__:satellite: 0.8276
INFO:__main__:lymphadenopathy: 0.9487
INFO:__main__:pleural: 0.8889
INFO:__main__:distant: 0.7813
INFO:__main__:Overall: 0.8664
INFO:__main__:F2 Scores:
INFO:__main__:measure: 0.8548
INFO:__main__:extension: 0.8559
INFO:__main__:atelectasis: 0.5978
INFO:__main__:satellite: 0.8108
INFO:__main__:lymphadenopathy: 0.9487
INFO:__main__:pleural: 0.8333
INFO:__main__:distant: 0.6906
INFO:__main__:Overall: 0.8236

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


-------
## BioClinical BERT
```
INFO:__main__:F1 Scores:
INFO:__main__:measure: 0.9059
INFO:__main__:extension: 0.9231
INFO:__main__:atelectasis: 0.7647
INFO:__main__:satellite: 0.7778
INFO:__main__:lymphadenopathy: 0.9600
INFO:__main__:pleural: 0.8451
INFO:__main__:distant: 0.7879
INFO:__main__:Overall: 0.8734
INFO:__main__:F2 Scores:
INFO:__main__:measure: 0.8871
INFO:__main__:extension: 0.9292
INFO:__main__:atelectasis: 0.6915
INFO:__main__:satellite: 0.7292
INFO:__main__:lymphadenopathy: 0.9375
INFO:__main__:pleural: 0.7853
INFO:__main__:distant: 0.7104
INFO:__main__:Overall: 0.8367

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

### Freeze first 6 layers
```
INFO:__main__:F1 Scores:
INFO:__main__:measure: 0.8889
INFO:__main__:extension: 0.8966
INFO:__main__:atelectasis: 0.6667
INFO:__main__:satellite: 0.7812
INFO:__main__:lymphadenopathy: 0.9367
INFO:__main__:pleural: 0.8406
INFO:__main__:distant: 0.8611
INFO:__main__:Overall: 0.8622
INFO:__main__:F2 Scores:
INFO:__main__:measure: 0.8451
INFO:__main__:extension: 0.8784
INFO:__main__:atelectasis: 0.5914
INFO:__main__:satellite: 0.8117
INFO:__main__:lymphadenopathy: 0.9439
INFO:__main__:pleural: 0.7672
INFO:__main__:distant: 0.8201
INFO:__main__:Overall: 0.8305

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


-------
## BiomedBERT
```
INFO:__main__:F1 Scores:
INFO:__main__:measure: 0.9080
INFO:__main__:extension: 0.8315
INFO:__main__:atelectasis: 0.7778
INFO:__main__:satellite: 0.7333
INFO:__main__:lymphadenopathy: 0.9231
INFO:__main__:pleural: 0.8235
INFO:__main__:distant: 0.7606
INFO:__main__:Overall: 0.8438
INFO:__main__:F2 Scores:
INFO:__main__:measure: 0.9018
INFO:__main__:extension: 0.8259
INFO:__main__:atelectasis: 0.7292
INFO:__main__:satellite: 0.7333
INFO:__main__:lymphadenopathy: 0.9231
INFO:__main__:pleural: 0.7447
INFO:__main__:distant: 0.7181
INFO:__main__:Overall: 0.8215

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

### Freeze first 6 layers
```
INFO:__main__:F1 Scores:
INFO:__main__:measure: 0.9070
INFO:__main__:extension: 0.7961
INFO:__main__:atelectasis: 0.8108
INFO:__main__:satellite: 0.6897
INFO:__main__:lymphadenopathy: 0.9487
INFO:__main__:pleural: 0.8286
INFO:__main__:distant: 0.8116
INFO:__main__:Overall: 0.8450
INFO:__main__:F2 Scores:
INFO:__main__:measure: 0.8945
INFO:__main__:extension: 0.8613
INFO:__main__:atelectasis: 0.7732
INFO:__main__:satellite: 0.6757
INFO:__main__:lymphadenopathy: 0.9487
INFO:__main__:pleural: 0.7632
INFO:__main__:distant: 0.7527
INFO:__main__:Overall: 0.8322

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
