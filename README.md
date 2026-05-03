# 🧬 CATALYST-Gene: Bidirectional LSTM with Bahdanau Attention for Cancer Classification

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://python.org)
[![TensorFlow](https://img.shields.io/badge/TensorFlow-2.x-orange.svg)](https://tensorflow.org)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Kaggle](https://img.shields.io/badge/Dataset-Kaggle-20BEFF.svg)](https://kaggle.com)

> A deep learning pipeline for classifying **5 cancer types** from RNA-Seq gene expression data using a novel Bidirectional LSTM architecture enhanced with Bahdanau Attention mechanism.

---

##  Overview

This project proposes **CATALYST-Gene** — a custom neural architecture that combines:
- **Bidirectional LSTM** to capture forward & backward temporal dependencies in gene sequences
- **Bahdanau Attention** to focus on the most discriminative gene expression patterns
- **Feature Engineering** on top-1000 variance-selected genes

The model is benchmarked against a standard Baseline LSTM model.

---

## Dataset

- **Source:** [Gene Expression Cancer RNA-Seq levels – Kaggle](https://www.kaggle.com/dsv/16063726)
- **DOI:** https://doi.org/10.34740/kaggle/dsv/16063726
- **Samples:** 801 patients
- **Features:** 20,531 genes → top 1,000 selected by variance
- **Target Classes:** BRCA, KIRC, COAD, LUAD, PRAD

---

##  Model Architecture


Input (1 × 1005 features)
│
▼
BiLSTM (128 units, return_sequences=True)
│
├──────────────────────┐
▼                      ▼
Bahdanau Attention     LSTM (64) → LSTM (32)
(context vector)       (sequential branch)
│                      │
└──────────┬───────────┘
▼
Concatenate
│
Dense(64) → Dense(32) → Softmax(5)

---

##  Results

| Metric   | Baseline LSTM | CATALYST-Gene |
|----------|:-------------:|:-------------:|
| Accuracy | ~96%          | ~98%+         |
| F1-Score | ~0.96         | ~0.98         |

---

##  How to Run

### Option 1: Run on Kaggle (Recommended)
1. Upload the notebook to [Kaggle Notebooks](https://kaggle.com/code)
2. Add the dataset: `salmanthecodepro/catalyst-gene-bilstm-attention-for-cancer`
3. Run all cells

### Option 2: Run Locally
```bash
git clone https://github.com/YOUR_USERNAME/catalyst-gene-bilstm-attention
cd catalyst-gene-bilstm-attention
pip install -r requirements.txt
jupyter notebook catalyst-gene-bilstm-attention.ipynb
```

---

##  Project Structure

catalyst-gene-bilstm-attention/
│
├── catalyst-gene-bilstm-attention.ipynb
├── requirements.txt
├── README.md
└── results/
    └── catalyst_gene_results.png

---

##  Key Techniques

- **Variance-based Gene Selection** – Top 1000 most variable genes
- **Statistical Feature Engineering** – Mean, Std, Max, Min, Range per sample
- **Bahdanau Attention** – Custom Keras layer with learnable weights
- **Early Stopping + ReduceLROnPlateau** – Prevents overfitting
- **Stratified Train/Val/Test Split** – 64% / 16% / 20%

---

##  Dependencies

See `requirements.txt`

---

## 👤 Author

**Salman Jahan**  
Department of Computer Science and engineering  
Uttara University
