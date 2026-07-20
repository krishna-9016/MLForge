# MLForge 🔧
### AI-Powered AutoML Platform for End-to-End Model Development

MLForge is an intelligent AutoML platform that automates the entire machine learning pipeline — from raw data to a production-ready, explainable model — through automated preprocessing, feature engineering, model selection, hyperparameter optimization, and explainable AI, all wrapped in an interactive dashboard.

---

## 🎯 Problem Statement

Building a machine learning model traditionally requires:
- Manually cleaning and preprocessing messy data
- Trying multiple algorithms and comparing results
- Tuning hyperparameters through trial and error
- Explaining *why* a model made a prediction to non-technical stakeholders

MLForge automates this entire workflow — a user uploads a dataset, and the platform handles preprocessing, model training, comparison, and explanation, generating a production-ready ML solution with zero manual coding required.

---

## ✨ Core Features

### 1. Automated Data Preprocessing (20+ techniques)
- Missing value imputation (mean, median, KNN, iterative imputation)
- Outlier detection and handling (IQR, Z-score, Isolation Forest)
- Encoding (one-hot, label, target encoding)
- Scaling/normalization (StandardScaler, MinMaxScaler, RobustScaler)
- Feature type auto-detection (numerical, categorical, datetime, text)

### 2. Automated Feature Engineering
- Feature selection (correlation-based, mutual information, recursive feature elimination)
- Feature generation (polynomial features, interaction terms, binning)
- Dimensionality reduction (PCA) for high-dimensional datasets

### 3. Multi-Algorithm Model Training (10+ algorithms)
- **Classification:** Logistic Regression, Random Forest, XGBoost, LightGBM, SVM, KNN, Naive Bayes
- **Regression:** Linear Regression, Ridge/Lasso, Random Forest Regressor, XGBoost Regressor, Gradient Boosting

### 4. Hyperparameter Optimization
- Grid Search (baseline)
- Random Search (faster exploration)
- **Optuna-based Bayesian optimization** (recommended — smarter search using past trial results)

### 5. Model Comparison & Leaderboard
- Automated cross-validation across all trained models
- Ranked leaderboard by chosen metric (accuracy, F1, RMSE, R², AUC-ROC)
- Training time vs. performance trade-off view

### 6. Explainable AI (XAI) with SHAP
- Global feature importance (which features matter most overall)
- Local explanations (why THIS specific prediction was made)
- SHAP summary plots, force plots, and dependence plots
- Business-friendly explanation text generation

### 7. Interactive Dashboard
- Dataset overview & statistical summary on upload
- Live experiment tracking (see models train in real time)
- Performance benchmarking charts
- AI-generated plain-English insights ("Feature X increases churn risk by Y%")
- Exportable prediction reports (PDF/CSV)

---

## 🏗️ Tech Stack

| Layer | Technology |
|---|---|
| Backend | Python, FastAPI |
| ML Core | Scikit-Learn, XGBoost, LightGBM, Optuna |
| Explainability | SHAP |
| Database | PostgreSQL |
| Frontend | ReactJS, Recharts/Plotly (for visualizations) |
| Model Storage | Joblib/Pickle + PostgreSQL metadata tables |
| Task Queue (optional) | Celery + Redis (for async model training) |

---

## 🧱 System Architecture

```
┌─────────────┐      ┌──────────────┐      ┌─────────────────┐
│   React     │─────▶│   FastAPI     │─────▶│  Preprocessing   │
│  Dashboard  │◀─────│   Backend     │◀─────│     Engine       │
└─────────────┘      └──────────────┘      └─────────────────┘
                             │                        │
                             ▼                        ▼
                     ┌──────────────┐      ┌─────────────────┐
                     │  PostgreSQL   │      │  Model Training  │
                     │  (metadata,   │      │  & Optimization  │
                     │   results)    │      │     Engine       │
                     └──────────────┘      └─────────────────┘
                                                      │
                                                      ▼
                                            ┌─────────────────┐
                                            │  SHAP Explainer  │
                                            │     Module       │
                                            └─────────────────┘
```

**Data Flow:**
1. User uploads CSV/dataset via React frontend
2. FastAPI receives file, sends to Preprocessing Engine
3. Preprocessing Engine auto-detects column types, cleans, encodes, scales
4. Cleaned data passed to Model Training Engine — trains multiple algorithms in parallel/sequence
5. Optuna optimizes hyperparameters for top-performing models
6. Results (metrics, models) stored in PostgreSQL
7. SHAP Explainer generates explanations for the best model
8. Dashboard fetches results, renders leaderboard, charts, and insights

---


## 📁  Folder Structure

```
mlforge/
├── backend/
│   ├── app/
│   │   ├── main.py                 # FastAPI entry point
│   │   ├── routers/
│   │   │   ├── upload.py
│   │   │   ├── preprocessing.py
│   │   │   ├── training.py
│   │   │   └── explain.py
│   │   ├── services/
│   │   │   ├── preprocessing_engine.py
│   │   │   ├── model_trainer.py
│   │   │   ├── optimizer.py
│   │   │   └── shap_explainer.py
│   │   ├── models/                 # Pydantic schemas + DB models
│   │   └── db/
│   │       └── database.py
│   └── requirements.txt
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   │   ├── UploadPanel.jsx
│   │   │   ├── Leaderboard.jsx
│   │   │   ├── ExplainabilityView.jsx
│   │   │   └── ExperimentTracker.jsx
│   │   └── App.jsx
│   └── package.json
└── README.md
```

---

