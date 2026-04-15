# PanCancer MultiOmics — Unsupervised Cancer Subtype Identification

A machine learning pipeline for integrating multi-omics data across 32 cancer types using the TCGA Pan-Cancer Atlas dataset. Built as a graduate course project in Big Data Science (University of Colorado Denver, Applied Mathematics M.S. program).

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
- **6,364 total features** in the combined early-fusion matrix
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
- Missing value imputation via `SimpleImputer` (mean strategy)
- Feature scaling with `StandardScaler`
- One-hot encoding of categorical clinical variables (gender, pathologic stage)
- 2σ clipping for outlier handling

### Early Fusion
All three modalities concatenated into a single (8,314 × 6,364) feature matrix prior to dimensionality reduction — preserving cross-modal relationships.

### Dimensionality Reduction & Clustering
- **PCA** to 50 components (retaining dominant variance structure)
- **K-Means clustering** (k=5) on PCA-reduced space
- **Silhouette Score: 0.534** — indicating strong, meaningful cluster separation

### Evaluation
- Silhouette score for internal cluster quality
- Cluster-to-cancer-label mapping (cross-tabulation)
- Mean age at diagnosis per cluster
- PCA 2D scatter visualization colored by cluster and cancer label

---

## Key Result

A silhouette score of **0.534** on the full 8,314-sample, 6,364-feature matrix indicates that the early-fusion representation captures genuine biological structure across cancer types — well above the threshold typically considered meaningful for high-dimensional omics data.

Cluster 1 skewed markedly younger (mean age 44.1) compared to other clusters (ages 54–60), suggesting the clustering captures clinically meaningful patient subgroups beyond simple cancer-type separation.

---

## Tech Stack

```
Python · scikit-learn · pandas · numpy · matplotlib · seaborn
```

---

## Repository Structure

```
PanCancer-MultiOmics/
├── ProjectCode.ipynb     # Full pipeline: EDA, preprocessing, fusion, clustering, evaluation
└── README.md
```

> **Note:** Raw TCGA data files are not included due to size. Data can be downloaded from the [GDC Data Portal](https://portal.gdc.cancer.gov/) or [PanCanAtlas Publications](https://gdc.cancer.gov/about-data/publications/pancanatlas).

---

## Next Steps (Planned)

- [ ] Transformer-based unified tokenization for deep multi-omics embedding (RQ2 final model)
- [ ] Feature importance analysis via SHAP values (RQ4)
- [ ] Survival analysis by cluster using Kaplan-Meier curves (RQ3)
- [ ] Comparison of early, late, and intermediate fusion strategies

---

## Author

**Weston White** | M.S. Applied Mathematics, University of Colorado Denver  
[whitewest19@gmail.com](mailto:whitewest19@gmail.com)
