Data-Driven Modelling of Solar PV Inverters

A data-driven monitoring and analytics project for utility-scale solar PV inverter systems. The project uses anonymized SCADA-style time-series data to estimate expected inverter output, detect operational deviations, forecast power generation, and present model outputs through a monitoring dashboard.

Why this project

Solar plant operators need to distinguish normal generation variability from genuine performance deviations. Weather, irradiance, temperature, curtailment, sensor errors, communication gaps, and inverter operating conditions can all affect measured output.

This project builds an analytical loop:

SCADA data
   ↓
Data validation & preprocessing
   ↓
Expected-power modelling / Digital Twin
   ↓
Actual vs. expected comparison
   ├──→ Anomaly detection
   └──→ Short-term forecasting
   ↓
Monitoring dashboard

Key capabilities

1. SCADA data processing

Works with high-frequency five-minute observations.

Handles missing values, invalid readings, and irregular time-series behavior.

Uses inverter electrical measurements and environmental variables.

Separates normal nighttime behavior from potentially abnormal operating periods.

2. Digital Twin

The Digital Twin estimates expected active AC power for an inverter.

The main modelling approach combines:

A physics-based expected-power model.

XGBoost residual learning to capture nonlinear and device-specific behavior.

Conceptually:

Expected Power = Physics Model + ML Residual

The original project evaluation reported a test R² of approximately 0.99998 and test NRMSE of approximately 0.00099 for the hybrid model on the evaluated anonymized inverter data.

3. Anomaly detection

The anomaly framework combines multiple indicators instead of relying on a single AC-power threshold:

Robust Z-score analysis of AC power.

DC/AC consistency checks.

Performance-ratio deviations.

Composite anomaly flags for periods requiring investigation.

The anomaly detector is intended as an operational review signal, not an automatic fault diagnosis.

4. Power forecasting

Several forecasting approaches were investigated:

VAR

LSTM

SARIMA/SARIMAX

Trend/seasonal decomposition using LSTM and DNN

Additional benchmark approaches

The forecasting experiments demonstrated that environmental variables and historical operating behavior contain useful information for short-term power prediction, while missing and invalid observations remain an important limitation.

5. Monitoring dashboard

A React + TypeScript dashboard provides the presentation/integration layer for:

Project/device navigation

Digital Twin outputs

Model comparisons

Metrics

Configurable dashboard widgets

The current dashboard is a demonstration interface rather than a live production inference system.

Technology stack

Python

Pandas

NumPy

Scikit-learn

XGBoost

LSTM / deep learning

SARIMA / SARIMAX

Statsmodels

React

TypeScript

Repository structure

A recommended structure for the project is:

solar-pv-inverter-analytics/
│
├── data/
│   └── README.md
│
├── notebooks/
│   ├── 01_eda.ipynb
│   ├── 02_digital_twin.ipynb
│   ├── 03_anomaly_detection.ipynb
│   └── 04_forecasting.ipynb
│
├── src/
│   ├── preprocessing/
│   ├── digital_twin/
│   ├── anomaly_detection/
│   └── forecasting/
│
├── dashboard/
│   └── ...
│
├── reports/
│   └── Solar_PV_Inverter_Analytics_Report.pdf
│
├── requirements.txt
└── README.md

Adjust the structure to match the actual repository files.

Data confidentiality

The original project uses confidential/anonymized plant data. Do not upload raw SCADA data, plant identifiers, locations, customer information, or other sensitive operational metadata to a public repository.

For a public GitHub version, use:

Synthetic data, or

Fully sanitized/approved sample data.

Reproducing the project

Create a Python environment.

Install the required Python dependencies.

Place only approved/sanitized data in the data directory.

Run the preprocessing/EDA workflow.

Train the Digital Twin.

Run anomaly detection.

Run the forecasting experiments.

Start the dashboard.

Example environment setup:

python -m venv .venv

Windows:

.venv\Scripts\activate

Linux/macOS:

source .venv/bin/activate

Then install dependencies:

pip install -r requirements.txt

The exact execution commands should follow the scripts/notebooks included in the repository.

Key results

Component

Result / observation

Data

Five-minute SCADA observations

Digital Twin

Hybrid physics + XGBoost residual model

Hybrid DT test R²

≈ 0.99998

Hybrid DT test NRMSE

≈ 0.00099

Anomaly detection

Composite multi-signal approach

Forecasting

VAR, LSTM, SARIMA/SARIMAX and decomposition approaches

Dashboard

React + TypeScript

Limitations

Models were evaluated on the available anonymized dataset.

Generalization to unseen plants is not established.

Missing telemetry and invalid readings affect forecasting performance.

Anomaly flags require operational/domain review.

The dashboard is currently a presentation layer and not a live SCADA inference platform.

Future improvements

Connect a sanitized real-time SCADA stream.

Add automated data-quality and telemetry-health checks.

Deploy model inference as an API.

Add automated alerts for sustained performance deviations.

Add recurring Excel/report exports for asset-management reporting.

Track plant-level KPIs such as performance ratio, availability, generation variance, and categorized losses.

Add model monitoring and retraining/drift detection.

Extend validation across multiple plants and inverter types.

Resume description

Data-Driven Modelling of Solar PV Inverters

Technologies: Python, Pandas, Scikit-learn, XGBoost, LSTM, SARIMA/SARIMAX, React, TypeScript

Developed a Digital Twin for utility-scale solar PV inverters to estimate expected AC power from SCADA measurements using a physics-based model and XGBoost residual learning.

Implemented composite anomaly detection using robust outlier analysis, DC/AC consistency checks, and performance-ratio deviations to identify periods requiring operational review.

Evaluated short-term power forecasting methods and integrated Digital Twin, anomaly, and model outputs into a React/TypeScript monitoring dashboard.
