## Google Bert

```
F1 Scores:
	omittable: 0.9533
	measure: 0.9195
	extension: 0.7711
	atelectasis: 0.5185
	satellite: 0.7407
	lymphadenopathy: 0.9487
	pleural: 0.8732
	distant: 0.7429
	Overall: 0.8775
F2 Scores:
	omittable: 0.9572
	measure: 0.9132
	extension: 0.7339
	atelectasis: 0.4023
	satellite: 0.6944
	lymphadenopathy: 0.9487
	pleural: 0.8115
	distant: 0.6952
	Overall: 0.8518
Classification Report:
                 precision    recall  f1-score   support

      omittable       0.95      0.96      0.95       149
        measure       0.93      0.91      0.92        88
      extension       0.84      0.71      0.77        45
    atelectasis       1.00      0.35      0.52        20
      satellite       0.83      0.67      0.74        30
lymphadenopathy       0.95      0.95      0.95        39
        pleural       1.00      0.78      0.87        40
        distant       0.84      0.67      0.74        39

      micro avg       0.92      0.84      0.88       450
      macro avg       0.92      0.75      0.81       450
   weighted avg       0.92      0.84      0.87       450
    samples avg       0.89      0.86      0.87       450
```
![[Google_BERT_training_history.png|600]]
### Freeze first 6 layers
```
F1 Scores:
	omittable: 0.9360
	measure: 0.9157
	extension: 0.8539
	atelectasis: 0.8000
	satellite: 0.7547
	lymphadenopathy: 0.9487
	pleural: 0.9041
	distant: 0.8116
	Overall: 0.8953

F2 Scores:
	omittable: 0.9341
	measure: 0.8837
	extension: 0.8482
	atelectasis: 0.7368
	satellite: 0.6993
	lymphadenopathy: 0.9487
	pleural: 0.8549
	distant: 0.7527
	Overall: 0.8710

Classification Report:
                 precision    recall  f1-score   support

      omittable       0.94      0.93      0.94       149
        measure       0.97      0.86      0.92        88
      extension       0.86      0.84      0.85        45
    atelectasis       0.93      0.70      0.80        20
      satellite       0.87      0.67      0.75        30
lymphadenopathy       0.95      0.95      0.95        39
        pleural       1.00      0.82      0.90        40
        distant       0.93      0.72      0.81        39

      micro avg       0.94      0.86      0.90       450
      macro avg       0.93      0.81      0.87       450
   weighted avg       0.94      0.86      0.89       450
    samples avg       0.89      0.87      0.88       450
```
![[Google_BERT_freeze_6_training_history.png|600]]

-------
## BioBERT
```
F1 Scores:
	omittable: 0.9502
	measure: 0.9480
	extension: 0.7857
	atelectasis: 0.5143
	satellite: 0.6792
	lymphadenopathy: 0.9048
	pleural: 0.9189
	distant: 0.8462
	Overall: 0.8844
F2 Scores:
	omittable: 0.9559
	measure: 0.9382
	extension: 0.7534
	atelectasis: 0.4737
	satellite: 0.6294
	lymphadenopathy: 0.9453
	pleural: 0.8763
	distant: 0.8462
	Overall: 0.8737
	
Classification Report:
                 precision    recall  f1-score   support

      omittable       0.94      0.96      0.95       149
        measure       0.96      0.93      0.95        88
      extension       0.85      0.73      0.79        45
    atelectasis       0.60      0.45      0.51        20
      satellite       0.78      0.60      0.68        30
lymphadenopathy       0.84      0.97      0.90        39
        pleural       1.00      0.85      0.92        40
        distant       0.85      0.85      0.85        39

      micro avg       0.90      0.87      0.88       450
      macro avg       0.85      0.79      0.82       450
   weighted avg       0.90      0.87      0.88       450
    samples avg       0.90      0.90      0.89       450
```
![[Bio_BERT_training_history.png|600]]
### Freeze first 6 layers
```
F1 Scores:
	omittable: 0.9502
	measure: 0.9480
	extension: 0.7857
	atelectasis: 0.5143
	satellite: 0.6792
	lymphadenopathy: 0.9048
	pleural: 0.9189
	distant: 0.8462
	Overall: 0.8844
F2 Scores:
	omittable: 0.9559
	measure: 0.9382
	extension: 0.7534
	atelectasis: 0.4737
	satellite: 0.6294
	lymphadenopathy: 0.9453
	pleural: 0.8763
	distant: 0.8462
	Overall: 0.8737

Classification Report:
                 precision    recall  f1-score   support

      omittable       0.94      0.96      0.95       149
        measure       0.96      0.93      0.95        88
      extension       0.85      0.73      0.79        45
    atelectasis       0.60      0.45      0.51        20
      satellite       0.78      0.60      0.68        30
lymphadenopathy       0.84      0.97      0.90        39
        pleural       1.00      0.85      0.92        40
        distant       0.85      0.85      0.85        39

      micro avg       0.90      0.87      0.88       450
      macro avg       0.85      0.79      0.82       450
   weighted avg       0.90      0.87      0.88       450
    samples avg       0.90      0.90      0.89       450
```
![[Bio_BERT_freeze_6_training_history.png|600]]

-------
## BioClinical BERT
```
F1 Scores:
	omittable: 0.9265
	measure: 0.9059
	extension: 0.8537
	atelectasis: 0.9231
	satellite: 0.7857
	lymphadenopathy: 0.9487
	pleural: 0.8732
	distant: 0.8219
	Overall: 0.8957
F2 Scores:
	omittable: 0.9539
	measure: 0.8871
	extension: 0.8065
	atelectasis: 0.9091
	satellite: 0.7534
	lymphadenopathy: 0.9487
	pleural: 0.8115
	distant: 0.7895
	Overall: 0.8849

Classification Report:
                 precision    recall  f1-score   support

      omittable       0.88      0.97      0.93       149
        measure       0.94      0.88      0.91        88
      extension       0.95      0.78      0.85        45
    atelectasis       0.95      0.90      0.92        20
      satellite       0.85      0.73      0.79        30
lymphadenopathy       0.95      0.95      0.95        39
        pleural       1.00      0.78      0.87        40
        distant       0.88      0.77      0.82        39

      micro avg       0.91      0.88      0.90       450
      macro avg       0.92      0.84      0.88       450
   weighted avg       0.92      0.88      0.89       450
    samples avg       0.92      0.90      0.90       450
```
![[Bio_Clinical_BERT_training_history.png|600]]
```
F1 Scores:
	omittable: 0.9385
	measure: 0.9070
	extension: 0.8409
	atelectasis: 0.7500
	satellite: 0.8077
	lymphadenopathy: 0.9367
	pleural: 0.8889
	distant: 0.8611
	Overall: 0.8973
F2 Scores:
	omittable: 0.9590
	measure: 0.8945
	extension: 0.8296
	atelectasis: 0.6522
	satellite: 0.7394
	lymphadenopathy: 0.9439
	pleural: 0.8333
	distant: 0.8201
	Overall: 0.8827

Classification Report:
                 precision    recall  f1-score   support

      omittable       0.91      0.97      0.94       149
        measure       0.93      0.89      0.91        88
      extension       0.86      0.82      0.84        45
    atelectasis       1.00      0.60      0.75        20
      satellite       0.95      0.70      0.81        30
lymphadenopathy       0.93      0.95      0.94        39
        pleural       1.00      0.80      0.89        40
        distant       0.94      0.79      0.86        39

      micro avg       0.92      0.87      0.90       450
      macro avg       0.94      0.82      0.87       450
   weighted avg       0.93      0.87      0.89       450
    samples avg       0.91      0.89      0.90       450
```
![[Bio_Clinical_BERT_freeze_6_training_history.png|600]]

-------
## BlueBERT
```
F1 Scores:
	omittable: 0.9527
	measure: 0.8914
	extension: 0.7805
	atelectasis: 0.6809
	satellite: 0.6780
	lymphadenopathy: 0.9487
	pleural: 0.8732
	distant: 0.8286
	Overall: 0.8747
F2 Scores:
	omittable: 0.9489
	measure: 0.8884
	extension: 0.7373
	atelectasis: 0.7477
	satellite: 0.6711
	lymphadenopathy: 0.9487
	pleural: 0.8115
	distant: 0.7754
Overall: 0.8618

Classification Report:
                 precision    recall  f1-score   support

      omittable       0.96      0.95      0.95       149
        measure       0.90      0.89      0.89        88
      extension       0.86      0.71      0.78        45
    atelectasis       0.59      0.80      0.68        20
      satellite       0.69      0.67      0.68        30
lymphadenopathy       0.95      0.95      0.95        39
        pleural       1.00      0.78      0.87        40
        distant       0.94      0.74      0.83        39

      micro avg       0.90      0.85      0.87       450
      macro avg       0.86      0.81      0.83       450
   weighted avg       0.90      0.85      0.87       450
    samples avg       0.90      0.88      0.89       450
```
![[BlueBert_training_history.png|600]]