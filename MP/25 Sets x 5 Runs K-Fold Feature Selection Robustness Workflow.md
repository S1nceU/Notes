# Consensus Feature Selection

## 這份文件在說明什麼

這份文件記錄的是本專案中一條特定的分析流程：

1. 用 `config/kfold_run_all_sets.ps1` 跑完 5 組 `25 sets` 的 k-fold feature selection。
2. 將每一組 25 sets 的輸出結果手動集中整理到 `weights/0304/`。
3. 在每一組結果資料夾內，用 `analysis.ipynb` 把 25 個 set 的 `feature_stability.csv` 再彙整成一份摘要，輸出到 `summary_outputs/`。
4. 最後再把 5 組摘要結果手動整合成 `Analysis_25sets_5seeds_robustness.xlsx`，用來檢查 feature robustness。

這不是整個專案的通用 README，而是描述「2026-03 這次 25 sets x 5 runs robustness 分析」到底做了哪些事。

## 名詞對照

- `1 run`：一組 `25 sets` 資料切分，例如 `data/TCGA_UCEC_25_Sets_1`。
- `1 set`：某一個 run 底下的其中一份資料，例如 `TCGA_UCEC_Set_01`。
- `5 runs`：對應到 `data/TCGA_UCEC_25_Sets_1` 到 `data/TCGA_UCEC_25_Sets_5`，後續整理後對應到 `weights/0304/25sets_1` 到 `weights/0304/25sets_5`。
- `summary_outputs`：某一個 run 內，把 25 個 set 的 feature stability 結果再做一次彙整後得到的摘要資料夾。
- `Analysis_25sets_5seeds_robustness.xlsx`：把 5 個 runs 的摘要結果再往上整合後的總表。

備註：
這裡慣用「5 seeds」來描述最終 robustness 分析，但從目前 repo 內可見流程來看，這 5 組主要體現在 `TCGA_UCEC_25_Sets_1` 到 `_5` 這五組資料切分。`config/kfold.yaml` 內的 k-fold `seed` 本身是固定的 `20250925`。

## 流程總覽

```text
data/TCGA_UCEC_25_Sets_1 ~ 5
  -> config/kfold_run_all_sets.ps1
  -> python -m src.cmd.kfold_fs (每個 run 跑 25 次，共 125 次)
  -> 每個 set 產生一個 kfold_fs 輸出資料夾
  -> 手動集中整理到 weights/0304/25sets_1 ~ 5
  -> 每個 run 用 analysis.ipynb 匯總 25 個 set
  -> 產生 summary_outputs/
  -> 手動整合 5 個 runs 的摘要
  -> weights/0304/Analysis_25sets_5seeds_robustness.xlsx
```

## Step 1. 批次跑 5 組 25-set feature selection

執行腳本：

```powershell
powershell -File config/kfold_run_all_sets.ps1
```

此腳本會依序跑：

- `data/TCGA_UCEC_25_Sets_1`
- `data/TCGA_UCEC_25_Sets_2`
- `data/TCGA_UCEC_25_Sets_3`
- `data/TCGA_UCEC_25_Sets_4`
- `data/TCGA_UCEC_25_Sets_5`

每一組裡面都包含：

- `TCGA_UCEC_Set_01`
- `TCGA_UCEC_Set_02`
- ...
- `TCGA_UCEC_Set_25`

也就是總共會執行 5 x 25 = 125 次 `kfold_fs`。

### 實際呼叫的 Python 指令

腳本內每次實際執行的核心指令是：

```powershell
python -m src.cmd.kfold_fs `
  --config config/kfold.yaml `
  --subset Set_XX `
  --normal_csv <set 的 [mval]normal_DMP.csv> `
  --tumor_csv <set 的 [mval]tumor_DMP.csv> `
  --label_csv <set 的 [raw]label.csv> `
  --beta_normal_csv <set 的 [bval]normal_DMP.csv> `
  --beta_tumor_csv <set 的 [bval]tumor_DMP.csv>
```

### 這一步使用的主要參數

`config/kfold.yaml` 內的主要設定如下：

- `type: M`
- `models: ["svm", "xgb", "relieff"]`
- `n_splits: 5`
- `shuffle: true`
- `seed: 20250925`
- `top_m: 100`
- `freq_threshold: 0.26`
- `model_coverage_threshold: 1`
- `alphas: [1.0, 5.0, 10.0, 20.0]`
- `n_neighbors: 10`
- `delta_beta_threshold: 0.2`

### 每個 set 會產生什麼

每次 `kfold_fs` 執行後，會產生一個資料夾，命名形式類似：

```text
weights/.../kfold_fs_m_Set_01_20260226-204703/
```

裡面主要包含：

- `config.json`
- `feature_stability.csv`
- `selected_features.txt`
- `summary.json`
- `per_fold/`

其中：

- `feature_stability.csv` 是後續 notebook 彙整的核心輸入。
- `selected_features.txt` 是該 set 經過 k-fold aggregation 後留下的 feature 名單。
- `per_fold/` 保留各 fold、各模型的 feature importance 結果，方便追查。

另外，整體批次執行時間會被記錄到：

```text
output/kfold_run_times.csv
```

## Step 2. 手動整理到 `weights/0304/`

跑完之後，這次分析不是直接在原始輸出位置上繼續做，而是把結果手動集中整理到：

```text
weights/0304/
```

這個資料夾下目前整理成：

- `weights/0304/25sets_1`
- `weights/0304/25sets_2`
- `weights/0304/25sets_3`
- `weights/0304/25sets_4`
- `weights/0304/25sets_5`

每個 `25sets_n` 底下都保留：

- 25 個 `kfold_fs_m_Set_*` 結果資料夾
- `analysis.ipynb`
- `summary_outputs/`

這一步是手動整理，不是目前 repo 內的自動化腳本流程。

## Step 3. 用 `analysis.ipynb` 匯總單一 run 的 25 個 sets

在每個 `weights/0304/25sets_n/` 裡，都有一份 `analysis.ipynb`。

這份 notebook 的用途是：

1. 掃描同一個 run 底下的 25 個 `kfold_fs_m_Set_*` 資料夾。
2. 讀入每個 set 的 `feature_stability.csv`。
3. 針對 feature 在 25 個 sets 中的表現做統計整理。
4. 輸出摘要 CSV 與圖表到 `summary_outputs/`。

### Notebook 產出的摘要檔案

每個 run 的 `summary_outputs/` 主要包含：

- `feature_stability_summary.csv`
- `feature_models_union.csv`
- `set_level_summary.csv`
- `core_biomarkers_list_like.csv`
- `top20_select_freq.png`
- `top20_lowest_avg_rank.png`
- `set_select_freq.png`

部分 run 另外還有帶 run 名稱尾碼的輸出，例如：

- `core_biomarkers_list_like_25sets_5.csv`

### 這些摘要檔案的大意

- `feature_stability_summary.csv`
  - 以 feature 為單位，彙整它在 25 個 sets 之間的統計值，例如 mean、std、min、max。
- `feature_models_union.csv`
  - 記錄某個 feature 在這 25 個 sets 裡曾經被哪些模型選到。
- `set_level_summary.csv`
  - 以 set 為單位，整理每個 set 的整體 feature selection 摘要。
- `core_biomarkers_list_like.csv`
  - 用比較容易閱讀的格式，列出每個 feature 出現於多少個 sets、平均 selection frequency、對應模型與出現在哪些 sets。

### 重要提醒

目前 notebook 內的 `base_dir` 曾指向桌面路徑，例如：

```text
C:\Users\Jason\Desktop\6models\DMP_sets_0304_25sets_random_seed\25sets_1
```

如果要在目前 repo 內重新執行 notebook，必須先把 `base_dir` 改成實際的專案路徑，例如：

```text
G:\TrainModel\weights\0304\25sets_1
```

否則 notebook 會讀不到正確資料夾。

## Step 4. 手動整合 5 個 runs 的摘要結果

當 `25sets_1` 到 `25sets_5` 都各自產出 `summary_outputs/` 後，再把這 5 份結果手動整合成：

```text
weights/0304/Analysis_25sets_5seeds_robustness.xlsx
```

這一步同樣是手動整理，不是目前 repo 內的自動化流程。

### 這份 Excel 在做什麼

這份總表的目的是比較 5 個 runs 之間，哪些 features 在不同 run 中都維持穩定。

工作簿目前可見的內容包括：

- `Raw_Overview`
- `Jaccard_Summary`
- `Full_100pct_counts`
- `Full_100pct_all5`
- `A_*` 一系列工作表
- `B_*` 一系列工作表

### A / B 兩套 criterion

依照工作簿內容：

- `A_Criterion`
  - `>=50% coverage & >=2 models`
  - 對應 `Set_Count_Min = 13`
- `B_Criterion`
  - `>=90% coverage & >=2 models`
  - 對應 `Set_Count_Min = 23`

也就是說，最終 Excel 不是只看某個 feature 有沒有在某一個 set 被選到，而是進一步要求：

- 在單一 run 中，該 feature 至少要出現在夠多的 sets 裡。
- 同時還要由至少 2 個 models 支持。
- 然後再比較它是否在 5 個 runs 之間都一致出現。

### Excel 裡常見的彙整表

- `A_consensus_5of5` / `B_consensus_5of5`
  - 滿足條件，且在 5 個 runs 全部都出現的 features。
- `A_consensus_ge4of5` / `B_consensus_ge4of5`
  - 滿足條件，且至少在 4 個 runs 出現。
- `A_consensus_ge3of5` / `B_consensus_ge3of5`
  - 滿足條件，且至少在 3 個 runs 出現。
- `A_kof5_counts` / `B_kof5_counts`
  - 統計符合條件的 features 分別出現在幾個 runs。
- `A_PairwiseOverlap` / `B_PairwiseOverlap`
  - 比較 run 與 run 之間的 feature overlap。
- `A_PairwiseJaccard` / `B_PairwiseJaccard`
  - 比較 run 與 run 之間的 Jaccard similarity。
- `A_Run1_core` 到 `A_Run5_core`
  - 對應各 run 在 criterion A 下的核心 feature。
- `B_Run1_core` 到 `B_Run5_core`
  - 對應各 run 在 criterion B 下的核心 feature。

## 這條流程的重點結論型態

這條分析流程的重點不是只產出單次 feature list，而是分成三層 robustness 檢查：

1. `單一 set 內`
   - 用 k-fold + 多模型做 feature selection。
2. `單一 run 的 25 個 sets 之間`
   - 看 feature 是否能跨 set 重複出現。
3. `5 個 runs 之間`
   - 看 feature 是否在不同資料切分下仍然穩定。

因此，`Analysis_25sets_5seeds_robustness.xlsx` 可以視為這次分析最終的 robustness 總結。

## 目前哪些步驟是手動的

目前流程中，下列步驟仍是手動操作：

- 將各 run 的 kfold 結果集中整理到 `weights/0304/`
- 在各 run 內手動開啟並執行 `analysis.ipynb`
- 修改 notebook 中的 `base_dir`
- 將 5 個 runs 的摘要再手動整合成 Excel

也就是說，只有「批次跑 125 次 kfold feature selection」是由 `config/kfold_run_all_sets.ps1` 自動化；後面的整理、摘要、跨 run 彙整目前都還不是完全自動。

## 建議後續維護方式

如果之後要讓其他人重跑這條流程，建議至少確認以下幾件事：

1. 先確認 `config/kfold.yaml` 的參數是否與本次分析一致。
2. 確認 `data/TCGA_UCEC_25_Sets_1 ~ 5` 的內容沒有被替換。
3. 跑完後，依照相同命名規則整理到 `weights/0304/25sets_1 ~ 5` 或新的版本資料夾。
4. 執行各 run 的 `analysis.ipynb` 前，先修改 `base_dir`。
5. 最後再用一致的 criterion 把 5 個 runs 整合到總表。

## 相關檔案

- `config/kfold_run_all_sets.ps1`
- `config/kfold.yaml`
- `weights/0304/25sets_1/analysis.ipynb`
- `weights/0304/25sets_1/summary_outputs/`
- `weights/0304/25sets_2/summary_outputs/`
- `weights/0304/25sets_3/summary_outputs/`
- `weights/0304/25sets_4/summary_outputs/`
- `weights/0304/25sets_5/summary_outputs/`
- `weights/0304/Analysis_25sets_5seeds_robustness.xlsx`
