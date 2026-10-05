# TexCast AI – AI-Powered Textile Demand Forecasting System
### College Machine Learning Project-Based Learning (PBL) Submission

[![Python 3.11](https://img.shields.io/badge/Python-3.11-3776AB.svg?style=flat&logo=python&logoColor=white)](https://python.org)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.142-009688.svg?style=flat&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com)
[![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-1.4+-F7931E.svg?style=flat&logo=scikit-learn&logoColor=white)](https://scikit-learn.org)
[![XGBoost](https://img.shields.io/badge/XGBoost-2.0+-EB5424.svg?style=flat&logo=xgboost&logoColor=white)](https://xgboost.readthedocs.io)
[![SHAP](https://img.shields.io/badge/Explainability-SHAP-brightgreen.svg?style=flat)](https://shap.readthedocs.io)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---

## 🌟 Executive Summary & PBL Abstract
**TexCast AI** is a production-ready, full-stack predictive demand forecasting platform engineered specifically for the Indian textile manufacturing and retail supply chain. 

Traditional apparel forecasting relies either on error-prone manual spreadsheets (Excel) or rigid enterprise ERP modules that neglect cultural seasonality. **TexCast AI** innovates by integrating:
1. **Multi-Variate Indian Cultural & Climate Dynamics:** Deep integration of festival calendars (**Pongal, Diwali, Onam, Eid, Christmas, Wedding Season**) and monsoon rainfall/temperature dynamics across 6 fabric classes (**Cotton, Silk, Denim, Polyester, Linen, Wool**).
2. **Dual-Output Predictive Architecture:** Simultaneously delivers exact unit volume demand (**Regression**) and quantile-based demand tiers (**Classification: Low / Medium / High**) bounded by statistical 90% confidence intervals.
3. **Explainable AI (SHAP):** Resolves the machine learning "black-box" challenge by generating real-time TreeSHAP force attributions explaining *why* demand was forecasted up or down.
4. **Smart Inventory Advisor:** Couples predictions directly to inventory formulas (**Safety Stock Buffer, Reorder Point [ROP], Economic Order Quantity [EOQ]**) with automated risk alerts (Critical Stock-Out, Reorder Soon, Optimal, Overstock).
5. **Interactive What-If Simulation Sandbox:** Real-time sliders for price point, discount %, promotions, and festival campaigns with instant delta recalculations.
6. **10-Model Empirical Benchmark + Weighted Ensemble:** Rigorously trains and evaluates 5 algorithms in both regression and classification modes with TimeSeriesSplit validation.
7. **Bilingual Localization (English / தமிழ்):** Dynamic instant toggle for Tamil and English retail users.

---

## 🔑 Demo Accounts & Credentials
> **Important for Evaluators & PBL Presentation:**
> Demo accounts are seeded automatically upon database initialization.

| Role | Email | Password | Access Privileges |
| :--- | :--- | :--- | :--- |
| **Admin** | `admin@texcast.ai` | `Admin@TexCast2026` | Full platform access, 10-model leaderboard, dataset upload, one-click auto-retraining, user management, and system audit trail. |
| **Retailer (User)** | `retailer@texcast.ai` | `Retailer@123` | Dashboard, dual-output forecaster, What-If simulator, smart inventory advisor, "My Data" CRUD management (isolated by `owner_id`), and PDF reports. |

---

## 🧠 Machine Learning Engine: 10 Models + Ensemble

TexCast AI trains and persists **5 core machine learning algorithms in BOTH Regression and Classification modes**:

```
                               ┌────────────────────────────────────────────────────────┐
                               │                    TexCast AI Engine                   │
                               └───────────────────────────┬────────────────────────────┘
                                                           │
                        ┌──────────────────────────────────┴──────────────────────────────────┐
                        ▼                                                                     ▼
     ┌─────────────────────────────────────┐                               ┌─────────────────────────────────────┐
     │       REGRESSION INFERENCE          │                               │     CLASSIFICATION INFERENCE        │
     │  (Predicts Exact Demand Units)      │                               │     (Predicts Demand Tier Level)    │
     ├─────────────────────────────────────┤                               ├─────────────────────────────────────┤
     │ 1. Linear Regression                │                               │ 1. Logistic Regression              │
     │ 2. Decision Tree Regressor          │                               │ 2. Decision Tree Classifier         │
     │ 3. Random Forest Regressor          │                               │ 3. Random Forest Classifier         │
     │ 4. XGBoost Regressor                │                               │ 4. XGBoost Classifier               │
     │ 5. K-Nearest Neighbors (KNN)        │                               │ 5. K-Nearest Neighbors (KNN)        │
     │ 6. BONUS: Weighted Ensemble         │                               │ 6. BONUS: Weighted Ensemble         │
     ├─────────────────────────────────────┤                               ├─────────────────────────────────────┤
     │ Metrics: MAE, RMSE, R², MAPE (%)    │                               │ Metrics: Accuracy, Precision,       │
     │ Auto-Selection: Maximize R² Score   │                               │          Recall, F1-Score, CM       │
     └─────────────────────────────────────┘                               └─────────────────────────────────────┘
```

### Data Pipeline & Preprocessing
- **Time-Based Split:** 80% Train, 20% Test chronologically ordered to completely eliminate look-ahead data leakage.
- **Feature Scaling:** `StandardScaler` for numeric features (critical for distance-based KNN and linear models).
- **Categorical Encoding:** `OneHotEncoder` with fixed category dictionaries for robust out-of-sample inference.
- **Ensemble Weights:** Dynamically weighted based on validation score ($R^2$ for regression, $F_1$ for classification).

---

## 📊 Architectural Comparison vs. Existing Systems

| Feature / Capability | TexCast AI (This System) | Excel / Manual Spreadsheets | Basic ERP Modules | Generic Single-Model SaaS (Prophet) |
| :--- | :---: | :---: | :---: | :---: |
| **Number of ML Models** | **10 Models + Ensemble** | None (Manual Moving Avg) | 1-2 Basic Statistical Curves | 1 Model (Prophet/ARIMA) |
| **Dual-Output (Reg + Clf)** | **Yes (Units + Tier)** | Manual formula | No (Quantity only) | No (Continuous curve only) |
| **Indian Festival Dynamics** | **Yes (Multi-year, 6 festivals)** | Manual guesswork | Generic public holidays | Manual regressor setup needed |
| **Monsoon & Weather Dynamics**| **Yes (Temp & Rainfall)** | No | No | Rare |
| **Explainable AI (SHAP)** | **Interactive TreeSHAP** | Blackbox formula | Opaque ERP rules | Trend decomposition only |
| **Smart Inventory Advisory** | **ROP, SS, EOQ, Risk Badges** | Separate sheet | Static min/max | None (Forecasting only) |
| **Live What-If Simulator** | **Instant Browser Sliders** | Manual copy-paste | Requires MRP batch rerun | Static charts only |
| **Role-Based Access Control**| **Strict Backend RBAC (403)** | None | Complex Enterprise Modules | Basic viewer role |
| **Cost & Deployment** | **Open-Source / Render Ready** | Labor-intensive | High Enterprise License | Recurring Monthly Subscriptions |

---

## 🛠️ Tech Stack & Directory Structure

- **Backend:** Python 3.11, FastAPI, SQLAlchemy ORM, SQLite, Scikit-Learn, XGBoost, SHAP, Joblib, Passlib (Bcrypt), Python-Jose (JWT), ReportLab (PDF generation).
- **Frontend:** Jinja2 server-rendered templates, HTML5, Tailwind CSS (CDN), Vanilla JavaScript, Chart.js. Single deployable service — **zero Node.js or npm build step required**.
- **Theming:** Deep indigo (`#1e1b4b`) + Teal accents (`#0d9488`), subtle fabric-weave textures, light/dark mode, bilingual English/Tamil dictionary.

```
texcast_ai/
├── app/
│   ├── main.py                     # FastAPI entrypoint, lifecycle & seeds
│   ├── config.py                   # App settings & secrets
│   ├── database.py                 # SQLAlchemy SQLite engine
│   ├── auth/                       # Security, Bcrypt, JWT & RBAC dependencies
│   ├── models/                     # SQLAlchemy ORM database tables
│   ├── schemas/                    # Pydantic data validation schemas
│   ├── ml/
│   │   ├── data_generator.py       # 3-year synthetic Indian textile generator
│   │   ├── feature_engineering.py  # OneHotEncoder, StandardScaler, TimeSplit
│   │   ├── trainer.py              # 10 models + Ensemble training & metrics
│   │   ├── predictor.py            # ModelManager singleton & dual inference
│   │   ├── explainer.py            # TreeSHAP feature impact engine
│   │   └── inventory.py            # Safety stock, ROP, EOQ & risk radar
│   ├── routers/                    # Auth, Predict, Data Entry, Admin, Export
│   ├── utils/                      # ReportLab PDF generator & audit logger
│   ├── templates/                  # Jinja2 responsive HTML templates
│   └── static/                     # CSS textures, Chart.js helpers, i18n
├── data/
│   ├── synthetic_textile_data.csv  # 3-year benchmark dataset
│   └── models/                     # Joblib serialized models & leaderboard.json
├── tests/                          # Pytest suite (Auth, RBAC, Prediction, CRUD)
├── scripts/                        # Standalone utility runners
├── render.yaml                     # Render Infrastructure-as-Code blueprint
├── requirements.txt                # Pinned dependencies
├── runtime.txt                     # python-3.11.9
└── README.md                       # Comprehensive PBL documentation
```

---

## 🚀 Quickstart: Running Locally

### 1. Clone & Set Up Virtual Environment
```bash
git clone https://github.com/YOUR_USERNAME/texcast-ai.git
cd texcast-ai

# Create virtual environment
python -m venv venv

# Activate (Windows)
venv\Scripts\activate
# Activate (Linux/macOS)
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt
```

### 2. Environment Variables (Optional)
Copy `.env.example` to `.env`:
```bash
cp .env.example .env
```
*(Default settings automatically fall back to SQLite and default demo admin credentials).*

### 3. Launch the Application
```bash
uvicorn app.main:app --host 127.0.0.1 --port 8000 --reload
```

Open your browser to:
👉 **`http://localhost:8000`**

On initial boot, the app automatically:
- Creates SQLite tables (`texcast.db`).
- Seeds the `admin@texcast.ai` and `retailer@texcast.ai` demo accounts.
- Generates the 3-year synthetic benchmark dataset and pre-trains all 10 models + Ensemble into `data/models/`.

---

## 🧪 Running Automated Unit Tests
```bash
pytest tests/ -v
```
Verifies:
- User registration, duplicate checks, password hashing, and JWT tokens.
- Strict RBAC enforcement (verifying non-admin users receive `403 Forbidden` on all admin routes).
- Dual prediction outputs, confidence intervals, and What-If calculations.
- Ownership checks on user data entry (preventing unauthorized cross-user modifications).

---

## ☁️ Deployment Instructions (Deploying to Render Free Web Service)

TexCast AI is pre-configured with `render.yaml`, `runtime.txt`, and committed models for instant zero-configuration deployment on [Render](https://render.com).

### Step-by-Step Instructions:

1. **Push to GitHub:**
   ```bash
   git init
   git add .
   git commit -m "feat: complete production-ready TexCast AI platform"
   git branch -M main
   git remote add origin https://github.com/YOUR_USERNAME/texcast-ai.git
   git push -u origin main
   ```

2. **Connect to Render:**
   - Log in to your [Render Dashboard](https://dashboard.render.com).
   - Click **New +** &rarr; Select **Blueprint** (or **Web Service**).
   - Select your `texcast-ai` GitHub repository.

3. **Automatic Configuration:**
   - Render will detect `render.yaml` automatically:
     - **Build Command:** `pip install -r requirements.txt`
     - **Start Command:** `uvicorn app.main:app --host 0.0.0.0 --port $PORT`
     - **Python Version:** `3.11.9`
   - Click **Apply** / **Create Web Service**.

4. **Live Verification:**
   - Once the deploy completes (~2-3 minutes), Render provides your public URL:
     `https://texcast-ai.onrender.com`
   - Visit the health check endpoint to confirm initialization:
     `https://texcast-ai.onrender.com/health`

---

## 📋 Evaluator's "How to Test" Checklist

1. **Landing & UI Aesthetics:**
   - Open `/`. Verify dark/light toggle and English/Tamil language toggle.
2. **Retailer Authentication:**
   - Sign in with `retailer@texcast.ai` / `Retailer@123`.
   - View `/dashboard` with dynamic KPI cards, 30-day forecast trend, and category doughnut charts.
3. **Dual-Output Prediction & SHAP:**
   - Visit `/predict`. Select `Silk`, `Diwali`, `South`, `Price: 4500`, `Discount: 20%`.
   - Click **Generate AI Forecast**.
   - Verify the **Dual Output** (Exact Units + Demand Level badge), 90% confidence range, and the **SHAP Waterfall Bar Chart** highlighting festival & price impacts.
   - Click **Export PDF** and verify the downloaded executive brief.
4. **Interactive What-If Simulation:**
   - Open `/simulator`. Drag the discount and price sliders; observe the live calculated delta and inventory risk status update without page reload.
5. **Smart Inventory Advisor:**
   - Open `/inventory`. Review safety stock, ROP, and EOQ calculations.
   - Click **Adjust Stock** to add incoming inventory; observe status badges update.
6. **User Data Entry (CRUD):**
   - Open `/my-data`. Add a product, log a single sale, or upload a bulk CSV.
   - Click **Refresh Forecast** to update catalog forecasts without model retraining.
7. **RBAC Security Enforcement:**
   - While logged in as retailer, try navigating to `/admin/leaderboard` or `/admin/users` &rarr; verify strict **403 Forbidden**.
8. **Admin Operations:**
   - Sign out and log in as `admin@texcast.ai` / `Admin@TexCast2026`.
   - Visit `/admin/leaderboard` &rarr; inspect all 10 models, metrics (MAE, RMSE, R2, MAPE, F1, Accuracy), and Confusion Matrix.
   - Visit `/admin/retrain` &rarr; click **One-Click Auto-Retrain** and review before/after delta comparison.
   - Visit `/admin/users` and `/admin/audit` to view system audit logs.

---

## 📸 Screenshots Recommended for the PBL Report
1. **System Architecture & Workflow Diagram** (Included in report design).
2. **Main Dashboard:** KPI cards, demand trends with confidence bounds, and category split.
3. **Dual-Output Predictor:** Exact quantity + demand tier badge + SHAP explanation bar chart.
4. **What-If Simulator:** Sliders and live elasticity delta response.
5. **Smart Inventory Radar:** Color-coded stockout and overstock risk badges.
6. **Model Benchmark Leaderboard:** 10-model comparison matrix and confusion matrix.
7. **Bilingual View:** Screenshot of the UI in Tamil (தமிழ்).
8. **Sample Generated PDF Forecast Brief:** Downloaded ReportLab document.
