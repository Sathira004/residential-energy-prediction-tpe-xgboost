# 🏠 Residential Building Energy Load Prediction Using Machine Learning

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange.svg)](residential-energy-prediction-tpe-xgboost.ipynb)

Predicting **Heating Load (Y1)** and **Cooling Load (Y2)** of residential buildings using TPE-optimized XGBoost and Random Forest regression, based on the [UCI Energy Efficiency dataset](https://archive.ics.uci.edu/dataset/242/energy+efficiency).

---

## 📋 Table of Contents

- [Project Overview](#-project-overview)
- [Dataset](#-dataset)
- [Methodology](#-methodology)
- [Key Results](#-key-results)
- [Project Structure](#-project-structure)
- [Getting Started](#-getting-started)
- [References](#-references)

---

## 🎯 Project Overview

This project implements and compares machine learning regression models to predict the energy loads of residential buildings. It serves as the practical component (**Phase 2 – Option A: Baseline Implementation**) based on the research paper:

> **"Residential Building Energy Usage Prediction Using Bayesian-Based Optimized XGBoost Algorithm"**  
> — Baqer and Rashidi-Khazaee (2025)

### Objectives

- Perform comprehensive **Exploratory Data Analysis (EDA)** on the Energy Efficiency dataset
- Implement a **Random Forest** regression model as the baseline
- Develop a **Chained XGBoost** regression model with **TPE (Tree-structured Parzen Estimator)** hyperparameter optimization via [Optuna](https://optuna.org/)
- Investigate whether predicted Heating Load can improve Cooling Load prediction through model chaining
- Evaluate models using **MAE**, **MSE**, **RMSE**, and **R²**
- Conduct **stress testing** (noise robustness and dimensionality tests)
- Compare results against the TPE-XGBoost results reported in the research paper

---

## 📊 Dataset

The [UCI Energy Efficiency dataset](https://archive.ics.uci.edu/dataset/242/energy+efficiency) contains **768 simulated residential building configurations** with 8 input features and 2 target variables:

| Feature | Description | Units |
|---------|-------------|-------|
| `X1` | Relative Compactness | Dimensionless |
| `X2` | Surface Area | m² |
| `X3` | Wall Area | m² |
| `X4` | Roof Area | m² |
| `X5` | Overall Height | m |
| `X6` | Orientation (N/E/S/W) | Categorical |
| `X7` | Glazing Area | Dimensionless |
| `X8` | Glazing Area Distribution | Categorical |
| **`Y1`** | **Heating Load** | **kW** |
| **`Y2`** | **Cooling Load** | **kW** |

> Original dataset by Tsanas & Xifara (2012). No missing values; 768 observations.

---

## 🔬 Methodology

### 1. Exploratory Data Analysis
- Feature distribution analysis (histograms)
- Outlier inspection (boxplots)
- Correlation heatmap (Pearson)
- Scatter plot analysis (Y1 vs Y2)

### 2. Data Preprocessing
- One-hot encoding of categorical features (`X6`, `X8`)
- 85/15 train-test split

### 3. Model 1: Random Forest (Baseline)
- Joint multi-output Random Forest with TPE-optimized hyperparameters
- 10-fold cross-validation during optimization

### 4. Model 2: Chained XGBoost
A sequential (chained) approach:
1. **Stage 1:** Optimize and train an XGBoost model for Heating Load (Y1)
2. **Stage 2:** Generate out-of-fold Y1 predictions
3. **Stage 3:** Use Y1 predictions as an additional feature to optimize and train a second XGBoost model for Cooling Load (Y2)

Both stages use **Optuna TPE** for hyperparameter optimization with 10-fold cross-validation.

### 5. Stress Testing
- **Noise Robustness Test:** Gaussian noise injection at increasing levels (σ = 0% to 50%)
- **Dimensionality Stress Test:** Adding irrelevant random features (0 to 20)

---

## 📈 Key Results

### Model Performance Comparison

| Model | Target | MAE (kW) | RMSE (kW) | R² |
|-------|--------|----------|-----------|-----|
| Random Forest | Y1 (Heating) | 0.338 | 0.483 | 0.998 |
| Random Forest | Y2 (Cooling) | 1.232 | 2.037 | 0.954 |
| **Chained XGBoost** | **Y1 (Heating)** | **0.266** | **0.365** | **0.999** |
| **Chained XGBoost** | **Y2 (Cooling)** | **0.773** | **1.223** | **0.983** |

### Improvement over Baseline

- **Heating Load (Y1):** 21.3% MAE reduction, 24.4% RMSE reduction
- **Cooling Load (Y2):** 37.3% MAE reduction, 39.9% RMSE reduction

### Comparison with Research Paper

| Model | Y1 MAE (kW) | Y2 MAE (kW) |
|-------|-------------|-------------|
| Random Forest (Implemented) | 0.338 | 1.232 |
| Chained XGBoost (Implemented) | 0.266 | 0.773 |
| **TPE-XGBoost (Research Paper)** | **0.169** | **0.275** |

> **Note:** The research paper used 10-fold CV across five independent runs on the entire dataset, while the implemented models were validated on a held-out test split (15%). The comparison serves as an indicative benchmark.

---

## 📁 Project Structure

```
residential-energy-prediction-tpe-xgboost/
│
├── README.md                                            # This file
├── LICENSE                                              # MIT License
├── requirements.txt                                     # Python dependencies
├── .gitignore                                           # Git ignore rules
│
├── residential-energy-prediction-tpe-xgboost.ipynb      # Main notebook (full analysis)
│
└── data/
    └── energy_efficiency.xlsx                           # UCI Energy Efficiency dataset
```

---

## 🚀 Getting Started

### Prerequisites

- Python 3.10 or higher
- Jupyter Notebook / JupyterLab (or Google Colab)

### Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/<your-username>/residential-energy-prediction-tpe-xgboost.git
   cd residential-energy-prediction-tpe-xgboost
   ```

2. **Create a virtual environment (recommended):**
   ```bash
   python -m venv venv
   source venv/bin/activate   # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

4. **Launch the notebook:**
   ```bash
   jupyter notebook residential-energy-prediction-tpe-xgboost.ipynb
   ```

### Running on Google Colab

You can also open the notebook directly in Google Colab:

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/<your-username>/residential-energy-prediction-tpe-xgboost/blob/main/residential-energy-prediction-tpe-xgboost.ipynb)

> **Note:** When using Colab, upload the `data/energy_efficiency.xlsx` file or adjust the data path accordingly.

---

## 📚 References

1. **Baqer, A. S., & Rashidi-Khazaee, P.** (2025). *Residential Building Energy Usage Prediction Using Bayesian-Based Optimized XGBoost Algorithm.*

2. **Tsanas, A., & Xifara, A.** (2012). Accurate quantitative estimation of energy performance of residential buildings using statistical machine learning tools. *Energy and Buildings*, 49, 560–567.

3. **UCI Machine Learning Repository** — [Energy Efficiency Dataset](https://archive.ics.uci.edu/dataset/242/energy+efficiency)

4. **Akiba, T., et al.** (2019). Optuna: A Next-generation Hyperparameter Optimization Framework. *Proceedings of the 25th ACM SIGKDD International Conference on Knowledge Discovery & Data Mining.*

---