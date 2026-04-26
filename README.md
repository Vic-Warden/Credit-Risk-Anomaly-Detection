# Credit Risk Anomaly Detection

**A REST API (Python/Flask) for real-time credit risk scoring and anomaly detection on loan applications.**

---

## Overview

Flask-based REST API that evaluates loan applications in real time using two ML models:
- **Credit Scoring** — Random Forest classifier predicting the probability of default.
- **Anomaly Detection** — Isolation Forest flagging atypical or potentially fraudulent applications.

A lightweight HTML/CSS/JS frontend consumes the API and visualizes results.

---

## Stack

| Layer | Technology |
|---|---|
| API | Python 3.9, Flask, scikit-learn, joblib |
| Models | Random Forest, Isolation Forest (`.pkl`) |
| Data | MySQL/MariaDB, Pandas, NumPy |
| Frontend | HTML, CSS, Vanilla JS (`fetch`) |
| DevOps | Docker, GitHub Actions |

---

## Project Structure

```
api/
  app.py                  # Flask app — /score endpoint
  Dockerfile
  requirements.txt
  models/
    anomaly_model_scoring_credit.pkl
    anomaly_model_detection_anomalies.pkl
database/
  schema.sql              # Relational schema (clients, comptes, demandes_de_pret)
  generate_data.py        # Synthetic data generation
  simulated_loans.csv
notebooks/
  exploration.ipynb       # EDA
  scoring_credit.ipynb    # Credit scoring model training
  detection_anomalies.ipynb # Anomaly detection model training
frontend/
  index.html
  style.css
  app.js
```

---

## API

### `GET /`
Health check. Returns model load status.

### `POST /score`

**Request body (JSON):**
```json
{
  "credit_score": 680,
  "loan_amount": 15000,
  "interest_rate": 5.5,
  "borrower_income": 50000,
  "term_months": 36,
  "borrower_age": 35
}
```

**Response:**
```json
{
  "probability_of_default": 0.1423,
  "is_anomaly": 0
}
```

`is_anomaly: 1` indicates the application was flagged by the Isolation Forest model.

---

## Run with Docker

```bash
docker build -t credit-risk-api .
docker run -p 5000:5000 credit-risk-api
```

---

## Run locally

```bash
pip install -r api/requirements.txt
python api/app.py
```

API available at `http://localhost:5000`.

---

## Model Limitations

The current models rely solely on borrower-level microeconomic features. Systemic risk is not captured. Potential improvements include integrating macroeconomic indicators (unemployment rate, GDP growth, credit spreads, market volatility) via Bloomberg BQL/BFF APIs.