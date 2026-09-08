#  AI Data Detective

**AI Data Detective** is a Streamlit application that automatically investigates an uploaded dataset — profiling it, scoring its quality, surfacing correlations, anomalies and statistical patterns, running clustering and machine learning where the data supports it, and producing a downloadable PDF report. No analysis code required.

Upload a CSV or Excel file and let the app do the detective work.

![Python](https://img.shields.io/badge/python-3.9%2B-blue)
![Streamlit](https://img.shields.io/badge/built%20with-Streamlit-FF4B4B)
![License](https://img.shields.io/badge/license-MIT-green)

---

## Features

- **Universal data loading** — CSV and Excel (`.xlsx`, `.xls`), with automatic encoding and delimiter detection, empty row/column cleanup, and datetime inference.
- **Dataset overview** — row/column counts, memory footprint, column types, and a per-column information table.
- **Data Quality Detective** — missing values, duplicate rows, constant and near-constant columns, high-cardinality/identifier-like columns, data-type mismatches (numbers stored as text), and implausible values (negative ages, out-of-range percentages, future dates, etc.), rolled up into a transparent 0–100 health score with a visible point breakdown.
- **Statistical analysis** — full descriptive statistics for numerical and categorical columns, plus a per-column deep dive with a normality test.
- **Exploratory visualizations** — histograms, box plots, distribution plots, scatter plots, correlation heatmaps, grouped bar charts, and time series, generated automatically from your column types.
- **Correlation Detective** — Pearson or Spearman correlation (auto-suggested based on skewness), a heatmap, and plain-language explanations of every strong or moderate relationship found — always noting that correlation is not causation.
- **Anomaly Detective** — IQR and Z-score outlier detection per column, plus multivariate Isolation Forest detection when multiple numerical columns are available, with CSV export of flagged rows.
- **Pattern Detective** — rule-based detection of group differences (with a t-test when comparing two groups), time trends (via linear regression), weekday/weekend effects, and value concentration — every finding is backed by an actual computed statistic, never invented text.
- **Clustering** — K-Means (with an elbow chart and automatic K selection via silhouette score) and DBSCAN (with automatic epsilon estimation), including a cluster scatter plot and a plain-language cluster profile.
- **Machine Learning** — automatic classification/regression detection based on your chosen target column, comparison across Logistic Regression / Decision Tree / Random Forest / Gradient Boosting / XGBoost, confusion matrices, actual-vs-predicted plots, feature importance (built-in or permutation-based), and a class-imbalance warning.
- **AI Insights** — rule-based natural-language insights generated directly from the computed numbers. If you set `OPENAI_API_KEY`, the same findings are rephrased by an LLM for a more polished narrative — the LLM is never allowed to invent new numbers, only to rewrite what was already calculated.
- **Reports** — one-click PDF investigation report covering every section above, plus CSV exports for missing values, statistics, correlations, anomalies, and model comparisons.

##  Project Structure

```
ai-data-detective/
├── app.py                      # Main Streamlit entry point
├── run.py                      # Convenience launcher (python run.py)
├── config/
│   └── config.py                # Thresholds and app-wide settings
├── modules/                     # Analysis pipeline (pure functions, no UI)
│   ├── data_loader.py
│   ├── data_profiler.py
│   ├── data_quality.py
│   ├── statistics.py
│   ├── visualization.py
│   ├── correlation_analysis.py
│   ├── anomaly_detection.py
│   ├── pattern_detection.py
│   ├── clustering.py
│   ├── prediction.py
│   ├── insight_generator.py
│   └── report_generator.py
├── models/                      # Low-level ML model wrappers
│   ├── clustering.py
│   ├── classification.py
│   └── regression.py
├── app_pages/                   # Streamlit page renderers
│   ├── dashboard.py
│   ├── overview.py
│   ├── quality.py
│   ├── statistics.py
│   ├── exploration.py
│   ├── correlations.py
│   ├── anomalies.py
│   ├── patterns.py
│   ├── clustering.py
│   ├── machine_learning.py
│   ├── insights.py
│   └── reports.py
├── utils/
│   ├── logger.py
│   ├── validators.py
│   └── helpers.py
├── tests/                       # pytest suite for the analysis modules
├── data/
│   └── sample_dataset.csv       # Example dataset to try the app with
├── requirements.txt
├── .env.example
└── README.md
```

##  Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/<your-username>/ai-data-detective.git
cd ai-data-detective
```

### 2. Install dependencies

```bash
python -m venv venv
source venv/bin/activate        # On Windows: venv\Scripts\activate
pip install -r requirements.txt
```

### 3. (Optional) Configure an OpenAI API key

Copy `.env.example` to `.env` and fill in your key if you want LLM-enhanced insights. This step is entirely optional — the app is fully functional without it, using rule-based insight generation instead.

```bash
cp .env.example .env
```

### 4. Run the app

```bash
streamlit run app.py
```

or

```bash
python run.py
```

Then open the URL Streamlit prints (typically `http://localhost:8501`).

### 5. Try it out

Upload your own CSV/Excel file from the sidebar, or use the bundled `data/sample_dataset.csv` — a synthetic customer dataset that intentionally contains missing values, a few duplicate rows, some outliers, and an invalid negative age, so you can see every detective in action.

## 🧪 Running the Tests

```bash
pytest tests/ -v
```

The test suite covers the data loader, data-quality checks, statistics, anomaly detection, correlation analysis, pattern detection, clustering, the ML pipeline, report generation, and insight generation — all against deterministic, seeded synthetic data.

## 🧠 Design Notes

- **Modules vs. pages** — everything under `modules/` and `models/` is plain Python with no Streamlit dependency, so it can be tested and reused independently of the UI. Everything under `app_pages/` only handles rendering and user input, delegating all computation to `modules/`.
- **No invented numbers** — every insight, pattern finding, and report statement is generated from an actual computed value. The optional LLM enhancement step is explicitly instructed to rephrase, never fabricate.
- **Health score is deterministic** — the same dataset always produces the same 0–100 score, with a visible, fixed-weight breakdown of exactly which issues cost how many points.
- **Graceful degradation** — clustering and machine learning pages check dataset suitability first (minimum rows, minimum usable columns) and explain clearly why a feature is unavailable rather than crashing or producing misleading results on tiny datasets.

##  Tech Stack

| Layer | Tools |
|---|---|
| UI | Streamlit |
| Data | pandas, numpy, openpyxl, xlrd |
| Stats & ML | scikit-learn, scipy, xgboost |
| Visualization | plotly, matplotlib, seaborn |
| Reporting | reportlab |
| AI Insights (optional) | OpenAI API |
| Testing | pytest |

##  Contributing

Issues and pull requests are welcome. If you're adding a new detective module, follow the existing pattern: put the pure analysis logic in `modules/`, keep it Streamlit-free, add tests in `tests/`, and wire up a thin renderer in `app_pages/`.

##  License

This project is licensed under the [MIT License](LICENSE).
