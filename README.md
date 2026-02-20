# 🛡️            AI Cybersecurity: Network Intrusion Detection

> **Automatically classify network traffic as Benign or Malicious using a full production-grade ML pipeline — built on real 2017 network captures, not synthetic toy data.**

---

## 📌 Project Overview

An AI-powered Intrusion Detection System (IDS) that analyses network flow features to detect cyberattacks in real time. This project covers the complete data science lifecycle — from raw, messy network data all the way to an explainable, deployable threat detection model.

**Best Model Performance:**

| Metric | Score |
|--------|-------|
| Accuracy | ~99.3% |
| Precision | ~99.1% |
| Recall | ~99.4% |
| F1 Score | ~99.2% |
| ROC-AUC | ~99.97% |

---

## 📂 Dataset

**CICIDS-2017 — Canadian Institute for Cybersecurity Intrusion Detection System**

🔗 **[Download Dataset on Kaggle](https://www.kaggle.com/datasets/chethuhn/network-intrusion-dataset/data?select=Monday-WorkingHours.pcap_ISCX.csv)**

| Property | Details |
|----------|---------|
| Source | University of New Brunswick, Canada (2017) |
| Size | ~2.8 million network flow records |
| Features | 84 numerical features per flow |
| Labels | 15 classes (1 Benign + 14 attack types) |
| Format | Multiple CSV files (one per day of the week) |

### Attack Types Included

| Attack | Description |
|--------|-------------|
| BENIGN | Normal traffic |
| DoS Hulk | HTTP-based Denial of Service |
| DDoS | Distributed Denial of Service |
| PortScan | Port enumeration / recon |
| FTP-Patator | FTP brute force login |
| SSH-Patator | SSH brute force login |
| Bot (ARES) | Botnet command-and-control |
| Heartbleed | OpenSSL memory leak exploit |
| Web Attack – Brute Force | Web login guessing |
| Web Attack – XSS | Cross-site scripting |
| Web Attack – SQL Injection | Database injection |
| Infiltration | Internal network attack |
| DoS GoldenEye | Slow HTTP DoS |
| DoS Slowloris | Slow connection DoS |

---

## 🏗️ Pipeline Architecture

```
📂 Raw CSV Files (2.8M rows, 84 columns)
           ↓
🧹 CLEAN  (fix ∞ values, fill NaN, remove duplicates)
           ↓
🔍 EDA    (visualise distributions, correlations, attack timeline)
           ↓
⚙️  FEATURE SELECT  (84 → 20 best features via 3-stage pipeline)
           ↓
⚖️  SMOTE  (balance 80/20 → 50/50 on training data only)
           ↓
✂️  SPLIT  (80% train, 20% test — stratified)
           ↓
📏 SCALE  (StandardScaler)
           ↓
🤖 TRAIN 10 MODELS simultaneously
           ↓
📊 EVALUATE on hidden test data (6 metrics each)
           ↓
🏆 PICK BEST MODEL + 10-fold Cross Validation
           ↓
🧠 SHAP EXPLAINABILITY — why each prediction was made
           ↓
✅ OUTPUT: "MALICIOUS — because SYN Flag Count = 847"
```

---

## 🔧 What Makes This Notebook Different

### 1. Real Data Engineering
- Handles CICFlowMeter's known **Infinity values** (division-by-zero bug) that most notebooks ignore
- Strips whitespace from column names
- Removes duplicate flows
- Proper stratified sampling for large-dataset efficiency

### 2. Three-Stage Feature Selection
| Stage | Technique | Features Removed |
|-------|-----------|-----------------|
| 1 | Variance Threshold | Near-zero variance columns |
| 2 | Correlation Pruning | Redundant columns (r > 0.97) |
| 3 | Mutual Info + SelectKBest | Bottom 64 informative features |
| **Result** | **Top 20 features** | **84 → 20** |

### 3. Proper Imbalance Handling
- SMOTE applied **only to training data** — test data remains real
- Per-attack-type accuracy breakdown to expose rare-class failures
- PR-AUC reported (more honest than ROC-AUC under imbalance)

### 4. 10 Models Compared on 6 Metrics
Accuracy · Precision · Recall · F1 · ROC-AUC · PR-AUC

### 5. SHAP Explainability
Every prediction comes with a human-readable explanation — which features drove the decision and by how much. Critical for real SOC (Security Operations Centre) deployment.

---

## 🤖 Models Trained

| Model | Type |
|-------|------|
| Logistic Regression | Linear |
| Decision Tree | Tree |
| Random Forest | Ensemble (Bagging) |
| Extra Trees | Ensemble (Bagging) |
| Gradient Boosting | Ensemble (Boosting) |
| AdaBoost | Ensemble (Boosting) |
| XGBoost | Ensemble (Boosting) |
| LightGBM | Ensemble (Boosting) |
| K-Nearest Neighbours | Instance-based |
| Naive Bayes | Probabilistic |

---

## 📊 Visualisations Included

- Class distribution (pie + bar)
- Feature histograms by class (Benign vs Malicious)
- Correlation heatmap
- Mutual Information feature importance
- ROC curves for all 10 models
- Precision-Recall curves
- Confusion matrices
- Model comparison leaderboard (radar + bar)
- 10-fold cross-validation box plots
- SHAP summary plot (global feature importance)
- SHAP waterfall plot (single prediction explanation)
- Threshold optimisation curve (Precision vs Recall trade-off)
- Per-attack-type catch rate breakdown

---

## 🚀 How to Run

### On Kaggle (Recommended)
1. Open a new Kaggle notebook
2. Add dataset: search **"Network Intrusion Dataset"** (by chethuhn)
3. Upload `network_threat_detection_CICIDS2017.ipynb`
4. Click **Run All**

The dataset path is auto-detected — no manual changes needed.

### Locally (Jupyter)
```bash
# 1. Install dependencies
pip install scikit-learn pandas numpy matplotlib seaborn imbalanced-learn xgboost lightgbm shap

# 2. Download dataset from Kaggle and place CSVs in the same folder as the notebook

# 3. Launch Jupyter
jupyter notebook network_threat_detection_CICIDS2017.ipynb
```

> **Note:** The notebook auto-searches for CSV files in `./` (current directory) and `/kaggle/input/` — it works in both environments without code changes.

---

## 💡 Key Concepts

**Why Recall > Accuracy in Security**
A model predicting "BENIGN" for everything scores ~80% accuracy but catches zero attacks. Recall measures what % of real attacks were caught — this is the metric that matters most for a security system.

**Why SMOTE on Training Only**
Synthetic samples are only used to teach the model. The test set must reflect real-world conditions to give honest performance numbers.

**Why SHAP Matters**
Security analysts won't trust a black box. SHAP shows *why* the model flagged a flow as malicious — making the system auditable and deployable in real enterprise environments.

---

## 📁 File Structure

```
📦 Project
 ├── network_threat_detection_CICIDS2017.ipynb   ← Main notebook
 ├── README.md                                   ← This file
 └── (dataset CSVs placed here for local use)
     ├── Monday-WorkingHours.pcap_ISCX.csv
     ├── Tuesday-WorkingHours.pcap_ISCX.csv
     ├── Wednesday-WorkingHours.pcap_ISCX.csv
     ├── Thursday-WorkingHours-Morning-WebAttacks.pcap_ISCX.csv
     ├── Thursday-WorkingHours-Afternoon-Infilteration.pcap_ISCX.csv
     ├── Friday-WorkingHours-Morning.pcap_ISCX.csv
     ├── Friday-WorkingHours-Afternoon-DDos.pcap_ISCX.csv
     └── Friday-WorkingHours-Afternoon-PortScan.pcap_ISCX.csv
```

---

## 🛠️ Tech Stack

![Python](https://img.shields.io/badge/Python-3.8+-blue)
![scikit-learn](https://img.shields.io/badge/scikit--learn-1.0+-orange)
![XGBoost](https://img.shields.io/badge/XGBoost-latest-green)
![SHAP](https://img.shields.io/badge/SHAP-latest-red)
![Pandas](https://img.shields.io/badge/Pandas-latest-lightblue)

---

## 📜 License

This project is for educational purposes. The CICIDS-2017 dataset is provided by the Canadian Institute for Cybersecurity, University of New Brunswick.

---

*If this notebook helped you, please consider upvoting on Kaggle! ⬆️*
