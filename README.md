# Solar-PV-Generation-Forecasting-and-Household-Feasibility-Analysis
# ML-Based Solar Irradiance Prediction & Household Energy Feasibility Analysis

A machine learning framework for predicting **Global Horizontal Irradiance (GHI)** and translating predictions into actionable solar energy feasibility insights for residential users.

> **B.Tech Project** — Electrical & Computer Engineering, Amrita School of Engineering, Coimbatore  
> Amrita Vishwa Vidyapeetham | March 2026

---

## Team

| Roll No | Name |
|---|---|
| CB.EN.U4ELC24011 | Harsha Vardhan Reddy |
| CB.EN.U4ELC24012 | Rishi Chitturi |
| CB.EN.U4ELC24055 | D Y Shashanka Shekar |
| CB.EN.U4ELC23063 | Arun Karthikeyan |

**Faculty In-Charge:** Mr. Nakkeeran M, Assistant Professor (OC), Dept. of Electrical & Electronics Engineering

---

## Overview

Solar irradiance prediction is a core challenge in renewable energy planning. This project benchmarks **12 machine learning regression models** on multi-location NSRDB data and wraps the best-performing model into a **user-centric feasibility analysis pipeline** that helps homeowners decide whether solar panel installation is worth it for them.

---

## Key Features

- **12-Model Benchmark** — Comparative evaluation of regression models under identical conditions
- **Multi-Location Training** — Denver CO, Los Angeles CA, Phoenix AZ, Miami FL (4 climate types)
- **5-Year GHI Forecast** — Hourly predictions for 2025–2029 with best/worst/average scenarios
- **Household Feasibility Module** — Estimates % of energy consumption coverable by solar
- **Tilt Sensitivity Analysis** — Sweeps 0°–60° to find the optimal panel angle for your location
- **Scenario Forecasting** — 10th/mean/90th percentile generation ranges from 200 RF trees
- **Prediction Reliability Report** — Transparent hour-ahead vs. day-ahead accuracy comparison

---

## Best Model: Random Forest Regressor

| Metric | Value |
|---|---|
| R² | 0.9155 |
| RMSE | 87.97 W/m² |
| MAE | 38.72 W/m² |
| Trees | 200 |
| Min samples per leaf | 5 |
| Preprocessing | StandardScaler |
| Random seed | 43 |

Gradient Boosting was a close second (R² = 0.9091, RMSE = 91.22 W/m²).

**Top feature importances:**

| Feature | Importance |
|---|---|
| Clearsky GHI | 0.9286 |
| Relative Humidity | 0.0272 |
| Temperature | 0.0169 |
| Solar Zenith Angle | 0.0084 |
| Wind Speed | 0.0058 |

---

## Repository Structure

```
├── data/
│   ├── raw/                  # Raw NSRDB CSV files (Denver, LA, Phoenix, Miami)
│   └── unseen/               # Unseen location data for forecasting (2020–2024)
│
├── models/
│   ├── train.py              # Training pipeline for all 12 models
│   ├── evaluate.py           # Evaluation metrics and comparison
│   └── global_rf_model.pkl   # Saved best model (Random Forest, ~29.7 MB)
│
├── analysis/
│   ├── feasibility.py        # Household energy feasibility module
│   ├── tilt_sensitivity.py   # Panel tilt angle optimization (0–60°)
│   ├── scenario_forecast.py  # Best/worst/average case generation
│   └── reliability.py        # Hour-ahead vs day-ahead comparison
│
├── outputs/
│   ├── forecast_2025_2029.csv          # Hourly GHI predictions (mean/worst/best)
│   ├── tilt_sensitivity.csv            # Tilt sweep results
│   ├── scenario_forecast.csv           # Per-year scenario table
│   ├── global_model_pred_vs_actual.png
│   └── final_dashboard.png
│
├── notebooks/
│   └── solar_global_model_and_forecast.ipynb   # Main Google Colab notebook
│
├── requirements.txt
└── README.md
```

---

## Getting Started

### Prerequisites

```bash
pip install -r requirements.txt
```

**Main dependencies:** `scikit-learn`, `pandas`, `numpy`, `matplotlib`

### Dataset

Data is sourced from the **NREL National Solar Radiation Database (NSRDB)**:
- **Training:** 4 U.S. cities — Denver CO, Los Angeles CA, Phoenix AZ, Miami FL
- **Forecasting:** Any unseen NSRDB location (place CSVs in `data/unseen/`)
- Hourly data from **2020–2024** (175,200 rows total; 15% sampled for training = 26,280 rows)
- **Input features:** Year, Month, Day, Hour, Minute, Temperature, Clearsky GHI, Relative Humidity, Solar Zenith Angle, Wind Speed
- **Target:** Global Horizontal Irradiance (GHI) in W/m²

Download data from [NSRDB](https://nsrdb.nrel.gov/) and place CSVs under `data/raw/<Location_Name>/`.

### Running the Pipeline

Open the notebook in Google Colab:

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/)

The notebook runs in 3 parts:

**Part 1 — Train global model** on 4-location NSRDB data (2020–2024)  
**Part 2 — Forecast GHI** for an unseen location across 2025–2029  
**Part 3 — Run user analysis** by filling in your panel specs and consumption

```python
# User inputs (Part 3)
USER_STATUS             = 'installed'   # 'plan' or 'installed'
USER_TILT_ANGLE         = 37           # degrees
USER_PANEL_AREA         = 5.0          # m²
USER_SYSTEM_CAPACITY    = 4.0          # kW
USER_DAILY_CONSUMPTION  = 10           # kWh/day
```

---

## Models Evaluated

| # | Model |
|---|---|
| 1 | Linear Regression |
| 2 | Polynomial Regression (degree=2) |
| 3 | Multiple Linear Regression |
| 4 | Lasso Regression |
| 5 | Ridge Regression |
| 6 | Elastic Net Regression |
| 7 | Support Vector Regression (SVR) |
| 8 | K-Nearest Neighbors Regressor |
| 9 | Decision Tree Regressor |
| 10 | **Random Forest Regressor (Best)** |
| 11 | AdaBoost Regressor |
| 12 | Gradient Boosting Regressor |

All models use a `StandardScaler -> Regressor` scikit-learn pipeline with an 80/20 train-test split (seed=42).

---

## Results

**5-Year GHI Forecast (2025–2029):**

| Year | Avg GHI (W/m²) | Worst | Best | Annual Energy (Wh/m²) |
|---|---|---|---|---|
| 2025 | 231.1 | 174.4 | 265.0 | 2,024,277 |
| 2026 | 231.1 | 174.4 | 265.0 | 2,024,277 |
| 2027 | 231.1 | 174.4 | 265.0 | 2,024,277 |
| 2028 | 231.0 | 174.4 | 264.9 | 2,029,226 |
| 2029 | 231.1 | 174.4 | 265.0 | 2,024,277 |

**Scenario Forecasting (5 m² panel, 20% efficiency, 37° tilt, 3,650 kWh/year consumption):**

| Year | Average (kWh) | Worst (kWh) | Best (kWh) | Avg Coverage |
|---|---|---|---|---|
| 2025–2029 | 1,529.6 | 1,156.4 | 1,752.8 | 41.9% |

**Prediction Reliability:**

| Metric | Hour-ahead | Day-ahead | Degradation |
|---|---|---|---|
| RMSE (W/m²) | 87.97 | 97.73 | +11.09% |
| MAE (W/m²) | 38.72 | 43.66 | +12.76% |
| R² | 0.9155 | 0.8957 | — |

---

## Core Formula

**PV Energy Generation:**
```
Energy (kWh) = GHI x Panel Area x Efficiency x Performance Ratio / 1000
```

**Tilt Correction:** Liu-Jordan cosine transposition formula (south-facing panels).

**Solar Zenith Angle:** Computed analytically via the NREL Solar Position Algorithm — no pvlib dependency required.

---

## Outputs

After running the notebook, the following files are saved to `outputs/`:

| File | Description |
|---|---|
| `global_rf_model.pkl` | Trained Random Forest pipeline (~29.7 MB) |
| `forecast_2025_2029.csv` | Hourly GHI predictions for all 5 years |
| `tilt_sensitivity.csv` | Energy output for tilt angles 0°–60° |
| `scenario_forecast.csv` | Per-year best/worst/average generation table |
| `global_model_pred_vs_actual.png` | Predicted vs actual scatter plot |
| `final_dashboard.png` | 8-panel summary dashboard |

---

## References

1. Allal et al. (2024) — ML algorithms for solar irradiance prediction, *Energies*
2. Jebli et al. (2021) — Pearson correlation-guided solar energy prediction, *Energy*
3. Haroun et al. (2023) — Optimal tilt angles for mono/bifacial PV systems, *Solar Energy*
4. Voyant et al. (2017) — ML methods for solar radiation forecasting: A review, *Renewable Energy*
5. Qing & Niu (2018) — Hourly day-ahead solar irradiance prediction using LSTM, *Energy*

Full reference list available in the project report.

---

## License

This project was developed for academic purposes at Amrita Vishwa Vidyapeetham. Please cite appropriately if you use this work.
