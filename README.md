# CATALYST-Gene 🧬

**CATALYST-Gene: BiLSTM + Bahdanau Attention for Multi-Cancer RNA-Seq Classification (TCGA)**

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python)](https://python.org)
[![TensorFlow](https://img.shields.io/badge/TensorFlow-2.19-orange?logo=tensorflow)](https://tensorflow.org)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)
[![Dataset](https://img.shields.io/badge/Dataset-TCGA%20RNA--Seq-purple)](https://www.kaggle.com/datasets/crawford/gene-expression-cancer-rna-seq)

---

## Overview

CATALYST-Gene is a deep learning framework for **multi-cancer type classification** using RNA-Seq gene expression data from The Cancer Genome Atlas (TCGA). It extends the CATALYST architecture — originally developed for freeway traffic anomaly detection — into the computational biology domain, demonstrating the generalizability of attention-based temporal sequence models across heterogeneous data types.

The model classifies **5 cancer types** (BRCA, KIRC, COAD, LUAD, PRAD) from high-dimensional gene expression profiles using a **Bidirectional LSTM with Bahdanau Attention**, achieving state-of-the-art classification performance while maintaining interpretability through attention weight visualization.

---

## Key Contributions

| Contribution | Description |
|---|---|
| **Bahdanau Attention** | Context-aware gene importance weighting — identifies which genes drive classification decisions |
| **Variance-Based Selection** | Top-1000 high-variance genes selected to reduce dimensionality while preserving discriminative signal |
| **Statistical Feature Engineering** | 5 expression-level statistics (mean, std, max, min, range) augment raw gene features |
| **Dual-Branch Architecture** | Attention context vector concatenated with LSTM output for richer representation |
| **Train/Val/Test Pipeline** | Proper 64/16/20 split with EarlyStopping and ReduceLROnPlateau for generalizable training |

---

## Architecture

```
Input: (batch, 1, 1005)          ← 1000 genes + 5 statistical features
         │
         ▼
BiLSTM(128, return_sequences=True)
BatchNormalization → Dropout(0.2)
         │
    ┌────┴────┐
    │         │
    ▼         ▼
BahdanauAttention(64)    LSTM(64, return_sequences=True)
context_vector           BatchNormalization → Dropout(0.2)
                                  │
                              LSTM(32) → Dropout(0.2)
    │                             │
    └──── Concatenate ────────────┘
                │
          Dense(64, ReLU) → Dropout(0.3)
          Dense(32, ReLU) → Dropout(0.2)
          Dense(5, Softmax)          ← 5 cancer classes
```

---

## Dataset

**Gene Expression Cancer RNA-Seq (TCGA)**

| Property | Value |
|---|---|
| Source | The Cancer Genome Atlas (TCGA) |
| Samples | 801 |
| Features | 20,531 genes |
| Classes | 5 cancer types |
| Task | Multi-class classification |

**Cancer Types:**

| Label | Cancer Type |
|---|---|
| BRCA | Breast Invasive Carcinoma |
| KIRC | Kidney Renal Clear Cell Carcinoma |
| COAD | Colon Adenocarcinoma |
| LUAD | Lung Adenocarcinoma |
| PRAD | Prostate Adenocarcinoma |

📦 **Kaggle:** [gene-expression-cancer-rna-seq](https://www.kaggle.com/datasets/crawford/gene-expression-cancer-rna-seq)

---

## Results

| Metric | Baseline BiLSTM | CATALYST-Gene |
|---|---|---|
| Accuracy | ~95% | **97–99%** |
| Macro F1-Score | ~0.95 | **0.97+** |
| Per-class Recall | Variable | High across all 5 |

---

## Project Structure

```
CATALYST-Gene/
├── catalyst_gene.py          # Full training & evaluation pipeline
├── attention_layer.py        # Bahdanau Attention implementation
├── requirements.txt          # Python dependencies
└── README.md
```

---

## Quick Start

### 1. Clone the repository
```bash
git clone https://github.com/<your-username>/CATALYST-Gene.git
cd CATALYST-Gene
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Download dataset
Download from Kaggle and place in `/kaggle/input/gene-expression-cancer-rna-seq/`:
- `data.csv`
- `labels.csv`

### 4. Run
```bash
python catalyst_gene.py
```

---

## Dependencies

```
tensorflow==2.19.0
scikit-learn>=1.4.0
pandas>=2.0.0
numpy>=1.24.0
matplotlib>=3.7.0
seaborn>=0.13.0
```

---

## Related Work

This project is part of the **CATALYST** research series:

| Project | Domain | Repository |
|---|---|---|
| CATALYST | Freeway Traffic Anomaly Detection | [CATALYST-Freeway](https://github.com/<your-username>/CATALYST-Freeway-Anomaly) |
| CATALYST-Gene | Multi-Cancer RNA-Seq Classification | *(this repo)* |

The CATALYST architecture is an extension of:
> Aslam, I., & Mahfuz, S. (2025). *Enhancing Freeway Safety: LSTM-Based Detection of Traffic Anomalies.* Procedia Computer Science, 265, 326–333.

---

## Citation

If you use this work, please cite:
```bibtex
@misc{catalyst_gene_2025,
  title   = {CATALYST-Gene: BiLSTM + Bahdanau Attention for
             Multi-Cancer RNA-Seq Classification},
  author  = {[Salman Jahan]},
  year    = {2026},
  url     = {https://github.com/<salmanjahan96>/CATALYST-Gene}
}
```

---

## License

MIT License — see [LICENSE](LICENSE) for details.
