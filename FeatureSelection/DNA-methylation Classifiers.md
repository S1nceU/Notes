## Project layout

```bash

.

├─ data/                     # input CSVs

│  ├─ [mval]normal.csv

│  ├─ [mval]tumor.csv

│  ├─ [bval]normal.csv

│  ├─ [bval]tumor.csv

│  └─ [raw]label.csv

├─ weights/                  # model weights (auto-created)

├─ images/                   # figures (auto-created)

├─ cache/                    # imputer/VT/scaler snapshots (auto-created)

├─ split/                    # train/val/test splits (auto-created)

└─ src/

   ├─ cmd/

   │  └─ train.py            # main entrypoint

   ├─ data/

   │  ├─ io.py               # read_beta_matrix/read_labels + save/load_splits

   │  └─ splits.py           # build_or_load_splits

   ├─ preprocess/

   │  └─ pipeline.py         # fit_preprocess/transform_preprocess

   ├─ models/

   │  ├─ linear.py           # LASSO/Ridge/ElasticNet

   │  └─ xgb.py              # XGBoost

   └─ utils/

      ├─ importance.py       # feature importance utilities

      ├─ metrics.py          # evaluate_split(), TorchProbaAdapter

      ├─ paths.py            # weight_path(), fig_path(), preproc_path()

      ├─ plots.py            # plot_roc()

      └─ seed.py             # random seed utilities

```

  

## Installation

1. Create an environment

```bash

conda create -n probeC python=3.10 -y

conda activate probeC

```

2. Install core deps

```bash

pip install numpy pandas scikit-learn matplotlib joblib

pip install torch --index-url https://download.pytorch.org/whl/cu126

pip install xgboost                

```

  

## Data expectations
- data/\[mval]\*.csv

- data/\[bval]\*.csv

  

## Run training

### LASSO

```

python -m src.cmd.train --model lasso --type B --subset all --alpha 1e-3 --seed 42

```

### Ridge

```

python -m src.cmd.train --model ridge --type M --subset all --alpha 1e-3 --seed 42

```

### Elastic Net

```

python -m src.cmd.train --model elasticnet --type M --subset all --alpha 1e-3 --l1_ratio 0.5 --seed 42

```

### XGBoost

```

python -m src.cmd.train --model xgb --type B --subset all --seed 42

```

  

## CLI flags

- ```--model {lasso|ridge|elasticnet|xgb}``` : 模型選擇
	- lasso、ridge、elasticnet 使用 Pytorch 做線性回歸模型
	- xgb 使用 XGBoost 套件訓練

- ```type {M|B}``` : 輸入矩陣

- ```--subset <tag>``` : 檔名標籤

- ```--alpha <float>``` : 正規化強度

- ```--l1_ratio <float>``` :  Elastic Net 中 L1 以及 L2 正規化比例

- ```seed <int>```

## Output

- Models $\rightarrow$ weight/
	- Torch 模型：\*.pt（僅存 state_dict 與一些必要超參數）
	- XGBoost：\*.joblib

- Pre-process $\rightarrow$ preproc/\*.joblib
	- 包含 imputer / VarianceThreshold / scaler / feature_names_all
	- 預測時務必載入相同的前處理，確保特徵對齊與尺度一致

- Image → image/*.png
	- 各分割（Train(tr) / Train(all) / Val / Test）的 ROC 曲線


