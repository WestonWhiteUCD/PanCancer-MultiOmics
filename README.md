# PanCancer MultiOmics — Unsupervised Cancer Subtype Identification

A machine learning pipeline for integrating multi-omics data across 32 cancer types using the TCGA Pan-Cancer Atlas dataset. Built as a graduate course project in Big Data Science (University of Colorado Denver, Applied Mathematics M.S. program).

> **Note:** This project is currently being rebuilt with corrected preprocessing methodology and an expanded notebook structure. Notebooks are being added incrementally with full documentation of methodological decisions.

---

## Overview

Cancer subtypes share molecular signatures that cut across tissue of origin. This project explores whether integrating **mRNA gene expression**, **DNA methylation**, and **clinical data** via early fusion and unsupervised learning can meaningfully separate cancer types without relying on labeled diagnoses.

---

## Dataset

| Modality | Source | Samples | Features |
|---|---|---|---|
| mRNA expression | TCGA Pan-Cancer Atlas | 8,314 | 3,217 genes |
| DNA methylation | TCGA Pan-Cancer Atlas | 8,314 | 3,139 CpG sites |
| Clinical data | TCGA Pan-Cancer Atlas | 8,314 | 6 variables |

- **32 cancer types** represented (BRCA, PRAD, THCA, LGG, HNSC, and 27 others)
- **~1,006 total features** in the combined early-fusion matrix after feature selection (500 mRNA + 500 methylation + ~6 clinical)
- Clinical variables: survival status, days to event, age at diagnosis, gender, pathologic stage, cancer label

---

## Research Questions

| # | Question |
|---|---|
| RQ1 | Can mRNA, DNA methylation, and clinical data be integrated to distinguish between cancer types in a Pan-Cancer cohort? |
| RQ2 | Does early fusion improve separation of cancer types compared to single-modality PCA? |
| RQ3 | Are identified clusters associated with known cancer types or clinical outcomes (survival, stage)? |
| RQ4 | Which molecular and clinical features drive the separation of cancer types? |

---

## Methods

### Preprocessing
- Sample ID normalization and alignment across all three modalities (8,314 common samples)
- Top-500 highest-variance feature selection per omics modality (mRNA and methylation separately) — data-driven, label-free
- Missing value imputation via `SimpleImputer` (mean strategy) — applied **before** scaling
- Feature scaling with `StandardScaler` — applied **after** imputation
- One-hot encoding of categorical clinical variables (gender, pathologic stage)

> **Preprocessing order rationale:** Imputation must precede scaling because `StandardScaler` computes column statistics from the data. Imputing after scaling distorts those statistics and produces incorrect scaled values.

### Early Fusion
All three modalities concatenated into a single (~8,314 × 1,006) feature matrix prior to dimensionality reduction — preserving cross-modal relationships between mRNA expression, methylation, and clinical variables.

### Dimensionality Reduction & Clustering
- **PCA** to 50 components (retaining dominant variance structure)
- **k-selection** via elbow curve and silhouette sweep across k=2–10
- **K-Means clustering** on PCA-reduced space at optimal k
- **Silhouette score** reported after correct preprocessing (see Notebook 3)

### Evaluation
- Silhouette score for internal cluster quality
- Cluster-to-cancer-label mapping (cross-tabulation)
- Mean age at diagnosis per cluster
- PCA 2D scatter visualization colored by cluster and cancer label

---

## Key Result

Cluster 1 skewed markedly younger (mean age 44.1) compared to other clusters (ages 54–60), suggesting the clustering captures clinically meaningful patient subgroups beyond simple cancer-type separation.

Full quantitative results including silhouette scores and cluster composition are reported in Notebook 5 after correct preprocessing.

---

## Tech Stack

```
Python · scikit-learn · PyTorch · pandas · numpy · matplotlib · seaborn
```

---

## Repository Structure

```
PanCancer-MultiOmics/
├── .gitignore
├── README.md
├── requirements.txt
├── data/
│   └── README.md
├── PanCancer_MultiOmics_Data_Loading_&_EDA.ipynb        ✓ complete
├── PanCancer_MultiOmics_Preprocessing.ipynb              ⟳ in progress
├── PanCancer_MultiOmics_PCA_Baseline.ipynb               planned
├── PanCancer_MultiOmics_Transformer_Autoencoder.ipynb    planned
└── PanCancer_MultiOmics_Evaluation.ipynb                 planned
```

> **Note:** Raw TCGA data files are not included due to file size. Data can be downloaded from the [GDC Data Portal](https://portal.gdc.cancer.gov/) or [PanCanAtlas Publications](https://gdc.cancer.gov/about-data/publications/pancanatlas).

---

## Roadmap

- [x] Data loading, alignment, and exploratory data analysis (Notebook 1)
- [ ] Corrected preprocessing pipeline with feature selection (Notebook 2)
- [ ] PCA baseline with data-driven k-selection via silhouette sweep (Notebook 3)
- [ ] Transformer autoencoder for deep multi-omics embedding in PyTorch (Notebook 4)
- [ ] Full evaluation — cluster interpretation, SHAP values, Kaplan-Meier survival curves (Notebook 5)

---

## Author

**Weston White** | M.S. Applied Mathematics, University of Colorado Denver  
[whitewest19@gmail.com](mailto:whitewest19@gmail.com)
