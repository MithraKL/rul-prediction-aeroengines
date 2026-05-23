# RUL Prediction of Aircraft Engines
### Machine Learning-Based Remaining Useful Life Prediction Using Adaptive Ensemble Techniques


---

## Overview

Aircraft engines degrade over time under extreme thermal, pressure, and mechanical stress. Predicting their **Remaining Useful Life (RUL)** enables condition-based maintenance — servicing engines exactly when needed, not too early or too late.

This project builds an **adaptive ensemble framework** that predicts RUL on the NASA C-MAPSS benchmark dataset across all four subsets (FD001–FD004), outperforming the single-model XGBoost baseline by **24.2% in RMSE**.

---

## Key Results

| Subset | Conditions | Fault Modes | RMSE | R² | PICP |
|--------|-----------|-------------|------|----|------|
| FD001 | 1 | 1 | 18.04 | 0.803 | 77% |
| FD002 | 6 | 1 | 17.64 | 0.841 | 91% |
| FD003 | 1 | 2 | 20.49 | 0.738 | 79% |
| FD004 | 6 | 2 | 21.33 | 0.769 | 81% |

**Baseline comparison (FD001):**

| Method | RMSE | R² |
|--------|------|----|
| Linear Regression | 29.07 | 0.510 |
| Random Forest | 28.15 | 0.540 |
| XGBoost (base paper) | 23.80 | 0.670 |
| **Adaptive Ensemble (ours)** | **18.04** | **0.803** |

---

## Three Core Innovations

**1. Adaptive Weighted Ensemble**  
Five base models (XGBoost, LightGBM, ExtraTrees, MLP, RandomForest) are combined using softmax temperature weighting from validation RMSE scores, blended with life-stage domain weights. Boosting models dominate near failure; tree ensembles dominate in the healthy early phase.

**2. Operating Condition-Aware Clustering**  
PCA (42 → 10 components) + KMeans (K=3, selected by elbow curve) clusters engine cycles into operating regimes. A separate ensemble is trained per cluster, preventing one model from averaging across incompatible engine states.

**3. Conformal Prediction Intervals**  
Rather than point estimates, the system outputs calibrated P10/P90 intervals using the 85th percentile of validation residuals. Prediction Interval Coverage Probability (PICP) ranges from 73–91% across all subsets.

---

## Pipeline

```
NASA C-MAPSS data
       ↓
Preprocessing (drop low-variance sensors, IQR clipping, StandardScaler)
       ↓
Feature Engineering (14 raw sensors → 42 features via rolling mean + std, window=5)
       ↓
PCA + KMeans Clustering (K=3 operating regimes)
       ↓
Per-cluster Ensemble Training (5 models × 3 clusters × 4 subsets = 60 models)
       ↓
Adaptive Weighted Prediction (softmax weights + life-stage blend, α=0.35)
       ↓
Conformal Calibration → P10/P90 intervals
       ↓
Evaluation: RMSE, R², PICP, MPIW
```

---

## Project Structure

```
rul-prediction-aeroengines/
├── notebooks/
│   └── rul-prediction-of-aeroengines.ipynb   # Main notebook (full pipeline)
├── report/
│   ├── RUL_Complete_Report_Dhanush.pdf        # Full project report
│   └── Final_RUL_Presentation.pptx           # Presentation slides
├── data/
│   └── README_data.md                        # Dataset download instructions
├── requirements.txt
├── .gitignore
└── README.md
```

---

## Dataset

This project uses the **NASA C-MAPSS (Commercial Modular Aero-Propulsion System Simulation)** dataset.

**Download:** [NASA Prognostics Data Repository](https://www.nasa.gov/intelligent-systems-division/discovery-and-systems-health/pcoe/pcoe-data-set-repository/)

After downloading, place the files as follows:
```
data/
├── train_FD001.txt
├── test_FD001.txt
├── RUL_FD001.txt
├── train_FD002.txt
├── test_FD002.txt
├── RUL_FD002.txt
├── train_FD003.txt
├── test_FD003.txt
├── RUL_FD003.txt
├── train_FD004.txt
├── test_FD004.txt
└── RUL_FD004.txt
```

Update the data path at the top of the notebook accordingly.

---

## Installation

```bash
git clone https://github.com/<your-username>/rul-prediction-aeroengines.git
cd rul-prediction-aeroengines
pip install -r requirements.txt
```

Then open the notebook:
```bash
jupyter notebook notebooks/rul-prediction-of-aeroengines.ipynb
```

Or run directly on **Google Colab** or **Kaggle Notebooks** (no GPU required — trains in under 15 minutes on CPU).

---

## Requirements

- Python 3.10+
- scikit-learn 1.2+
- xgboost 1.7+
- lightgbm 3.3+
- pandas 1.5+
- numpy 1.23+
- matplotlib 3.6+
- seaborn 0.12+

Install all with:
```bash
pip install -r requirements.txt
```

---

## Authors

- **L Mithra Kumar**

---

## References

- Saxena et al. (2008) — C-MAPSS dataset paper
- Deepika J et al. (2025) — XGBoost baseline (ICMCSI-2025)
- Full references in `report/RUL_Complete_Report_Dhanush.pdf`

---
