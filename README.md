## Project Description
This repository contains data processing, feature ranking, and machine-learning artifacts for predicting cyclodextrin–guest complex bioavailability. The workflow combines classical descriptors with Iso2vec molecular embeddings, evaluates multiple model families, and compares variants with and without embedding features.

## Project Tree
```text
.
├── README.md
├── pyproject.toml
├── poetry.lock
├── inputs/
│   └── data/
│       ├── NewCDMergedData.csv
│       ├── CDRawData.csv
│       ├── CDEnrichedData.csv
│       ├── NewCDData.csv
│       └── clean/
│           ├── dataset_with_vec.csv
│           ├── dataset_without_vec.csv
│           ├── dataset_with_vec_expanded_scaled.csv
│           └── dataset_without_vec_expanded_scaled.csv
├── outputs/
│   ├── analiza/
│   ├── analiza_hmbfr/
│   │   ├── with_vecs_new/
│   │   │   ├── hmbfr_feature_scores.csv
│   │   │   └── trainready/
│   │   └── without_vecs_new/
│   │       ├── hmbfr_feature_scores.csv
│   │       └── trainready/
│   ├── models/
│   ├── models_hmbfr/
│   │   ├── global_summary.csv
│   │   ├── with_vecs_new/
│   │   └── without_vecs_new/
│   └── reports/
└── predicting-cyclodextrin-bioavailability/
    ├── notebooks/
    └── scripts/
        └── utils/
```

## Supplementary Materials Path

### Supplementary Material 1. Primary Dataset
- **File name:** `NewCDMergedData.csv`
- **Exact path:** `inputs/data/NewCDMergedData.csv`
- **Content summary:** Final source dataset used for all analyses in the manuscript. It contains **980 samples** and **249 columns**. Each row represents a single cyclodextrin–guest complex and includes:
  - experimental conditions and target variable,
  - host and guest identifiers,
  - physicochemical and structural descriptors,
  - Iso2vec embeddings derived from isomeric SMILES:
    - guest vectors: `iso2vec-0` … `iso2vec-9`,
    - host vectors: `iso2vec-host-0` … `iso2vec-host-9`.
- **Role in pipeline:** Primary data source; train-ready matrices (S5–S6 equivalents) are derived from this dataset via feature selection and standardization.

### Supplementary Material 2. HmbFR Feature Ranking
- **File name:** `hmbfr_feature_scores.csv`
- **Exact path:** `outputs/analiza_hmbfr/with_vecs_new/hmbfr_feature_scores.csv`
- **Content summary:** Complete Heat Map–Based Feature Ranker (HmbFR) ranking with **166 features** and associated importance values.
  - ranking values are stored in the `score` column,
  - ranking includes Iso2vec guest components (`iso2vec-0` … `iso2vec-9`) and classical descriptors.
- **Role in pipeline:** Used to build reduced feature subsets such as top-32, top-64, and top-128; the top-64 subset was selected for the final configuration.

### Supplementary Material 3. Model Comparison Summary
- **File name:** `global_summary.csv`
- **Exact path:** `outputs/models_hmbfr/global_summary.csv`
- **Content summary:** Compact summary table of the **top 8** model-selection outcomes across dataset variants and feature-set sizes (e.g., `with_vecs_new` vs `without_vecs_new`, and `full`, `top32`, `top64`, `top128`).
- **Role in pipeline:** Supports final model selection reported in the manuscript (best configuration: LightGBM on `with_vecs_new` / `top64`).

### Supplementary Material 4. Detailed Report for Final Configuration
- **File name:** `report.md`
- **Exact path:** `outputs/models_hmbfr/with_vecs_new/top64/report.md`
- **Content summary:** Detailed performance table for the final selected setup: `with_vecs_new` / `top64` (**980 samples**, **64 selected features**). It reports test-set metrics (**MAE**, **RMSE**, **R²**) for evaluated model families in a directly comparable format sorted by RMSE.
- **Role in pipeline:** Provides transparent per-model performance details for the final selected configuration.

### Supplementary Material 5. Train-Ready Dataset Without Iso2vec
- **File name:** `dataset_without_vec_expanded_scaled.csv`
- **Exact path:** `inputs/data/clean/dataset_without_vec_expanded_scaled.csv`
- **Content summary:** Standardized train-ready matrix for modeling without Iso2vec embeddings. It contains **980 rows** and **157 columns**.
  - Regression target `DeltaG` remains in the original physical scale (not standardized).
  - Remaining numerical feature columns are standardized (approximately zero mean and unit variance).
- **Role in pipeline:** Used for fair benchmarking of models that do not use embedding-based structural features.

### Supplementary Material 6. Train-Ready Dataset With Iso2vec
- **File name:** `dataset_with_vec_expanded_scaled.csv`
- **Exact path:** `inputs/data/clean/dataset_with_vec_expanded_scaled.csv`
- **Content summary:** Analogous standardized train-ready matrix that includes Iso2vec guest embeddings. It contains **980 rows** and **167 columns**.
  - Includes additional standardized guest embedding columns: `iso2vec-0` … `iso2vec-9`.
  - Regression target `DeltaG` remains in the original physical scale.
  - All feature columns (including Iso2vec components) are standardized.
  - Note: only guest Iso2vec components are included in this matrix; host Iso2vec components are not included.
- **Role in pipeline:** Supports benchmark and final-model experiments in the embedding-augmented setting.

