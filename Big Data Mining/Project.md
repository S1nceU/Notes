## Google Bert

```
INFO:__main__:F1 Scores:
INFO:__main__:omittable: 0.9533
INFO:__main__:measure: 0.9195
INFO:__main__:extension: 0.7711
INFO:__main__:atelectasis: 0.5185
INFO:__main__:satellite: 0.7407
INFO:__main__:lymphadenopathy: 0.9487
INFO:__main__:pleural: 0.8732
INFO:__main__:distant: 0.7429
INFO:__main__:Overall: 0.8775
INFO:__main__:F2 Scores:
INFO:__main__:omittable: 0.9572
INFO:__main__:measure: 0.9132
INFO:__main__:extension: 0.7339
INFO:__main__:atelectasis: 0.4023
INFO:__main__:satellite: 0.6944
INFO:__main__:lymphadenopathy: 0.9487
INFO:__main__:pleural: 0.8115
INFO:__main__:distant: 0.6952
INFO:__main__:Overall: 0.8518
INFO:__main__:Classification Report:
c:\Users\Zhe\miniconda3\envs\moead\lib\site-packages\sklearn\metrics\_classification.py:1248: UndefinedMetricWarning: Precision and F-score are ill-defined and being set to 0.0 in samples with no predicted labels. Use `zero_division` parameter to control this behavior.
  _warn_prf(average, modifier, msg_start, len(result))
INFO:__main__:
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

INFO:__main__:{'F1': {'omittable': 0.9533333333333331, 'measure': 0.9195402298850575, 'extension': 0.7710843373493975, 'atelectasis': 0.5185185185185185, 'satellite': 0.7407407407407408, 'lymphadenopathy': 0.9487179487179487, 'pleural': 0.8732394366197184, 'distant': 0.7428571428571428, 'Overall': 0.8774795799299883}, 'F2': {'omittable': 0.9571619812583666, 'measure': 0.9132420091324202, 'extension': 0.7339449541284403, 'atelectasis': 0.40229885057471265, 'satellite': 0.6944444444444444, 'lymphadenopathy': 0.9487179487179486, 'pleural': 0.8115183246073298, 'distant': 0.695187165775401, 'Overall': 0.851835070231083}}
```

### Freeze first 6 layers
```
INFO:__main__:F1 Scores:
INFO:__main__:omittable: 0.9360
INFO:__main__:measure: 0.9157
INFO:__main__:extension: 0.8539
INFO:__main__:atelectasis: 0.8000
INFO:__main__:satellite: 0.7547
INFO:__main__:lymphadenopathy: 0.9487
INFO:__main__:pleural: 0.9041
INFO:__main__:distant: 0.8116
INFO:__main__:Overall: 0.8953
INFO:__main__:F2 Scores:
INFO:__main__:omittable: 0.9341
INFO:__main__:measure: 0.8837
INFO:__main__:extension: 0.8482
INFO:__main__:atelectasis: 0.7368
INFO:__main__:satellite: 0.6993
INFO:__main__:lymphadenopathy: 0.9487
INFO:__main__:pleural: 0.8549
INFO:__main__:distant: 0.7527
INFO:__main__:Overall: 0.8710
INFO:__main__:Classification Report:
c:\Users\Zhe\miniconda3\envs\moead\lib\site-packages\sklearn\metrics\_classification.py:1248: UndefinedMetricWarning: Precision and F-score are ill-defined and being set to 0.0 in samples with no predicted labels. Use `zero_division` parameter to control this behavior.
  _warn_prf(average, modifier, msg_start, len(result))
INFO:__main__:
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

INFO:__main__:{'F1': {'omittable': 0.936026936026936, 'measure': 0.9156626506024097, 'extension': 0.853932584269663, 'atelectasis': 0.8, 'satellite': 0.7547169811320754, 'lymphadenopathy': 0.9487179487179487, 'pleural': 0.9041095890410958, 'distant': 0.8115942028985509, 'Overall': 0.8953488372093023}, 'F2': {'omittable': 0.9341397849462364, 'measure': 0.883720930232558, 'extension': 0.8482142857142857, 'atelectasis': 0.7368421052631579, 'satellite': 0.6993006993006993, 'lymphadenopathy': 0.9487179487179486, 'pleural': 0.854922279792746, 'distant': 0.7526881720430108, 'Overall': 0.8710407239819006}}
```
## BioBERT
```
INFO:__main__:F1 Scores:
INFO:__main__:omittable: 0.9502
INFO:__main__:measure: 0.9480
INFO:__main__:extension: 0.7857
INFO:__main__:atelectasis: 0.5143
INFO:__main__:satellite: 0.6792
INFO:__main__:lymphadenopathy: 0.9048
INFO:__main__:pleural: 0.9189
INFO:__main__:distant: 0.8462
INFO:__main__:Overall: 0.8844
INFO:__main__:F2 Scores:
INFO:__main__:omittable: 0.9559
INFO:__main__:measure: 0.9382
INFO:__main__:extension: 0.7534
INFO:__main__:atelectasis: 0.4737
INFO:__main__:satellite: 0.6294
INFO:__main__:lymphadenopathy: 0.9453
INFO:__main__:pleural: 0.8763
INFO:__main__:distant: 0.8462
INFO:__main__:Overall: 0.8737
INFO:__main__:Classification Report:
c:\Users\Zhe\miniconda3\envs\moead\lib\site-packages\sklearn\metrics\_classification.py:1248: UndefinedMetricWarning: Precision and F-score are ill-defined and being set to 0.0 in samples with no predicted labels. Use `zero_division` parameter to control this behavior.
  _warn_prf(average, modifier, msg_start, len(result))
INFO:__main__:
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

INFO:__main__:{'F1': {'omittable': 0.9501661129568106, 'measure': 0.9479768786127167, 'extension': 0.7857142857142856, 'atelectasis': 0.5142857142857143, 'satellite': 0.6792452830188679, 'lymphadenopathy': 0.9047619047619048, 'pleural': 0.9189189189189189, 'distant': 0.8461538461538461, 'Overall': 0.8843537414965987}, 'F2': {'omittable': 0.9558823529411764, 'measure': 0.9382151029748282, 'extension': 0.7534246575342466, 'atelectasis': 0.4736842105263158, 'satellite': 0.6293706293706293, 'lymphadenopathy': 0.945273631840796, 'pleural': 0.8762886597938145, 'distant': 0.8461538461538461, 'Overall': 0.8736559139784947}}
```

### Freeze 6
```
INFO:__main__:F1 Scores:
INFO:__main__:omittable: 0.9502
INFO:__main__:measure: 0.9480
INFO:__main__:extension: 0.7857
INFO:__main__:atelectasis: 0.5143
INFO:__main__:satellite: 0.6792
INFO:__main__:lymphadenopathy: 0.9048
INFO:__main__:pleural: 0.9189
INFO:__main__:distant: 0.8462
INFO:__main__:Overall: 0.8844
INFO:__main__:F2 Scores:
INFO:__main__:omittable: 0.9559
INFO:__main__:measure: 0.9382
INFO:__main__:extension: 0.7534
INFO:__main__:atelectasis: 0.4737
INFO:__main__:satellite: 0.6294
INFO:__main__:lymphadenopathy: 0.9453
INFO:__main__:pleural: 0.8763
INFO:__main__:distant: 0.8462
INFO:__main__:Overall: 0.8737
INFO:__main__:Classification Report:
c:\Users\Zhe\miniconda3\envs\moead\lib\site-packages\sklearn\metrics\_classification.py:1248: UndefinedMetricWarning: Precision and F-score are ill-defined and being set to 0.0 in samples with no predicted labels. Use `zero_division` parameter to control this behavior.
  _warn_prf(average, modifier, msg_start, len(result))
INFO:__main__:
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

INFO:__main__:{'F1': {'omittable': 0.9501661129568106, 'measure': 0.9479768786127167, 'extension': 0.7857142857142856, 'atelectasis': 0.5142857142857143, 'satellite': 0.6792452830188679, 'lymphadenopathy': 0.9047619047619048, 'pleural': 0.9189189189189189, 'distant': 0.8461538461538461, 'Overall': 0.8843537414965987}, 'F2': {'omittable': 0.9558823529411764, 'measure': 0.9382151029748282, 'extension': 0.7534246575342466, 'atelectasis': 0.4736842105263158, 'satellite': 0.6293706293706293, 'lymphadenopathy': 0.945273631840796, 'pleural': 0.8762886597938145, 'distant': 0.8461538461538461, 'Overall': 0.8736559139784947}}
```
## BioClinical BERT
```
INFO:__main__:F1 Scores:
INFO:__main__:omittable: 0.9265
INFO:__main__:measure: 0.9059
INFO:__main__:extension: 0.8537
INFO:__main__:atelectasis: 0.9231
INFO:__main__:satellite: 0.7857
INFO:__main__:lymphadenopathy: 0.9487
INFO:__main__:pleural: 0.8732
INFO:__main__:distant: 0.8219
INFO:__main__:Overall: 0.8957
INFO:__main__:F2 Scores:
INFO:__main__:omittable: 0.9539
INFO:__main__:measure: 0.8871
INFO:__main__:extension: 0.8065
INFO:__main__:atelectasis: 0.9091
INFO:__main__:satellite: 0.7534
INFO:__main__:lymphadenopathy: 0.9487
INFO:__main__:pleural: 0.8115
INFO:__main__:distant: 0.7895
INFO:__main__:Overall: 0.8849
INFO:__main__:Classification Report:
INFO:__main__:
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

INFO:__main__:{'F1': {'omittable': 0.926517571884984, 'measure': 0.9058823529411764, 'extension': 0.8536585365853658, 'atelectasis': 0.9230769230769231, 'satellite': 0.7857142857142856, 'lymphadenopathy': 0.9487179487179487, 'pleural': 0.8732394366197184, 'distant': 0.8219178082191781, 'Overall': 0.8956916099773242}, 'F2': {'omittable': 0.9539473684210527, 'measure': 0.8870967741935485, 'extension': 0.8064516129032258, 'atelectasis': 0.9090909090909091, 'satellite': 0.7534246575342466, 'lymphadenopathy': 0.9487179487179486, 'pleural': 0.8115183246073298, 'distant': 0.7894736842105264, 'Overall': 0.8848566308243729}}

```

```
INFO:__main__:F1 Scores:
INFO:__main__:omittable: 0.9385
INFO:__main__:measure: 0.9070
INFO:__main__:extension: 0.8409
INFO:__main__:atelectasis: 0.7500
INFO:__main__:satellite: 0.8077
INFO:__main__:lymphadenopathy: 0.9367
INFO:__main__:pleural: 0.8889
INFO:__main__:distant: 0.8611
INFO:__main__:Overall: 0.8973
INFO:__main__:F2 Scores:
INFO:__main__:omittable: 0.9590
INFO:__main__:measure: 0.8945
INFO:__main__:extension: 0.8296
INFO:__main__:atelectasis: 0.6522
INFO:__main__:satellite: 0.7394
INFO:__main__:lymphadenopathy: 0.9439
INFO:__main__:pleural: 0.8333
INFO:__main__:distant: 0.8201
INFO:__main__:Overall: 0.8827
INFO:__main__:Classification Report:
c:\Users\Zhe\miniconda3\envs\moead\lib\site-packages\sklearn\metrics\_classification.py:1248: UndefinedMetricWarning: Precision and F-score are ill-defined and being set to 0.0 in samples with no predicted labels. Use `zero_division` parameter to control this behavior.
  _warn_prf(average, modifier, msg_start, len(result))
INFO:__main__:
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

INFO:__main__:{'F1': {'omittable': 0.9385113268608415, 'measure': 0.9069767441860465, 'extension': 0.8409090909090908, 'atelectasis': 0.7499999999999999, 'satellite': 0.8076923076923077, 'lymphadenopathy': 0.9367088607594937, 'pleural': 0.888888888888889, 'distant': 0.8611111111111112, 'Overall': 0.8972602739726027}, 'F2': {'omittable': 0.9589947089947092, 'measure': 0.8944954128440367, 'extension': 0.8295964125560538, 'atelectasis': 0.6521739130434783, 'satellite': 0.7394366197183099, 'lymphadenopathy': 0.943877551020408, 'pleural': 0.8333333333333334, 'distant': 0.8201058201058201, 'Overall': 0.8827493261455526}}

```
## BlueBERT

