# Enterprise Network Traffic Data Management and Threat Detection System

An offline, end-to-end pipeline that turns raw network flow logs into analyst-ready threat insights: Python ETL → normalized SQLite database → Random Forest attack classifier → Streamlit dashboard.

> **Status: in development (CO3554 Data Management Project).**
> ✅ Data feasibility study and GeoIP lookup (notebook)
> 🔲 ETL scripts · 🔲 Normalized database + index benchmarks · 🔲 Classifier · 🔲 Dashboard

## Why
Existing studies on intrusion detection concentrate on model accuracy. This project focuses on the pipeline around the model: how data is stored, organized and enriched, and how a security analyst uses the result.

## Data
| Source | Use |
|---|---|
| [UNSW-NB15](https://research.unsw.edu.au/projects/unsw-nb15-dataset) (~2.54M flows, 49 features, 9 attack types) | Network flows |
| [IP-to-country](https://www.kaggle.com/datasets/ipinfo/ipinfo-country-asn) | Country of each distinct source IP |


## Design principles
- IP addresses and country are **never** model inputs; country is added after detection as analyst context.
- Train/test split first; class imbalance (class weights/SMOTE) handled on training data only.
- Evaluation uses per-class precision, recall and F1, not accuracy alone.
- Normalized schema; query time measured with and without indexes.

## Pipeline
1. **ETL** (Pandas/NumPy): clean, type-fix, resolve each distinct IP to a country via IP-range binary search.
2. **Database** (SQLite): flow, ip_address, country, protocol, service, state, attack_category, prediction tables.
3. **Modelling** (scikit-learn): Logistic Regression baseline vs Random Forest; predictions written back to the DB.
4. **Dashboard** (Streamlit): threat overview, heatmaps, country view, flow explorer.

## Tech stack
Python · Pandas · NumPy · SQLite/SQL · scikit-learn · Streamlit · Google Colab · VS Code

## Repository structure
```text
├── notebooks/      # EDA and GeoIP feasibility notebook
├── src/            # ETL and database scripts (planned)
├── dashboard/      # Streamlit app (planned)
├── data/           # not tracked – see Data section
└── requirements.txt
```

## References
Moustafa & Slay, "UNSW-NB15: a comprehensive data set for network intrusion detection systems," MilCIS 2015.
