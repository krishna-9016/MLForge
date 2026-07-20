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

## 📅 Build Roadmap (Phased Approach)

### Phase 1: Foundation (Week 1)
- [ ] Set up FastAPI backend skeleton + PostgreSQL schema
- [ ] Build file upload endpoint (CSV parsing with Pandas)
- [ ] Auto-detect column types (numeric/categorical/datetime)
- [ ] Basic React frontend with upload UI + dataset preview table

### Phase 2: Preprocessing Pipeline (Week 2)
- [ ] Implement missing value imputation strategies
- [ ] Implement encoding strategies (one-hot, label)
- [ ] Implement scaling strategies
- [ ] Build a "preprocessing config" UI so users can toggle techniques
- [ ] Unit test preprocessing functions on 2-3 sample datasets (Titanic, Iris, a regression dataset)

### Phase 3: Model Training Engine (Week 3)
- [ ] Implement training loop for classification algorithms
- [ ] Implement training loop for regression algorithms
- [ ] Build cross-validation + metric calculation logic
- [ ] Store results in PostgreSQL (model name, metrics, training time)
- [ ] Build leaderboard API endpoint

### Phase 4: Hyperparameter Optimization (Week 4)
- [ ] Integrate Optuna for top 2-3 models from leaderboard
- [ ] Define search spaces per algorithm
- [ ] Store best hyperparameters + re-trained model

### Phase 5: Explainability (Week 5)
- [ ] Integrate SHAP for tree-based models (TreeExplainer — fastest)
- [ ] Generate summary plots (global importance)
- [ ] Generate force plots (single prediction explanation)
- [ ] Convert SHAP values into plain-English insight sentences (simple template-based NLG is fine to start)

### Phase 6: Dashboard Polish (Week 6)
- [ ] Build experiment tracking view (live status of training)
- [ ] Build performance comparison charts (Recharts/Plotly)
- [ ] Build exportable report generation (PDF via a Python lib like `reportlab` or `weasyprint`)
- [ ] Add loading states, error handling, empty states

### Phase 7: Deployment & Documentation (Week 7)
- [ ] Deploy backend (Render/Railway free tier)
- [ ] Deploy frontend (Vercel/Netlify)
- [ ] Write final README, add architecture diagram, add demo GIF/video
- [ ] Record a 2-3 min demo video for your portfolio

---

## 📁 Suggested Folder Structure

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

## 🗣️ Interview Talking Points (know these cold)

**"Why did you build this?"**
Manually building ML models is repetitive and time-consuming — I wanted to automate the repetitive 80% (preprocessing, model comparison, tuning) so more time goes into problem framing and interpreting results, which is where the real value is.

**"Why SHAP over LIME?"**
SHAP is grounded in game theory (Shapley values) and gives mathematically consistent, additive explanations — meaning feature contributions sum up to the actual prediction. LIME approximates locally but isn't always consistent across similar instances.

**"Why Optuna over GridSearch?"**
GridSearch is exhaustive and slow — it tries every combination blindly. Optuna uses Bayesian optimization (specifically Tree-structured Parzen Estimators) to intelligently pick the next hyperparameter set based on past trial performance, converging to good results in far fewer trials.

**"How do you handle a dataset where the target column isn't specified?"**
The platform should let the user select the target column explicitly on upload, then infer the task type (classification vs. regression) based on the target's data type and cardinality (e.g., a numeric column with only 2-10 unique values → classification).

**"What happens if two models score similarly on accuracy?"**
This is exactly why the leaderboard shouldn't rank on a single metric alone — I'd show training time, inference time, and interpretability trade-offs too, so the "best" model depends on the business context, not just raw accuracy.

---

## 🚀 Stretch Goals (if you have extra time)
- AutoML-style automated model selection (like an "auto-pilot" mode that picks preprocessing + model combo automatically based on dataset characteristics)
- Support for time-series datasets (auto-detect datetime index, suggest Prophet/ARIMA)
- User authentication + saved experiment history per user
- Docker Compose setup for one-command local deployment

---

## 📝 Notes for Yourself
- Don't try to build all 20+ preprocessing techniques and 10+ algorithms on day one — get a **thin end-to-end slice working first** (1 preprocessing technique, 2 algorithms, basic SHAP) before scaling up breadth. A working thin pipeline beats a half-built wide one, especially if you need to demo this soon.
- Keep a `sample_datasets/` folder with 2-3 clean test datasets (Titanic for classification, Boston Housing/California Housing for regression) so you can demo reliably without depending on user uploads during interviews.