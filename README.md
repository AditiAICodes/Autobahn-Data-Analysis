# Autobahn Telemetry Analysis

*Simulated high-speed vehicle telemetry inspired by German Autobahn conditions**

## Project Overview
This project evolves from a raw Python simulation script into a highly structured Data Engineering pipeline, paving the way for predictive Machine Learning. Instead of downloading a clean, pre-made dataset, I built a custom engine to generate realistic sensor data, intentionally corrupted it, and built a pipeline to clean and visualize the results. 

**The Core Architecture:** `Simulate → Corrupt → Clean → Analyze → Predict`

### Tech Stack & Skills
* **Languages:** Python 3
* **Libraries:** Pandas, NumPy, Matplotlib, Seaborn, Random, Datetime
* **Core Skills:** Data Imputation (`NaN` handling), Boolean Filtering, Forensic Logging, Data Visualization, System Architecture.



## Phase 1: The Telemetry Sensor Engine
A modular Python loop that mimics an onboard vehicle computer generating live data.
* **Live Sensors:** Generates continuous readouts for Speed (km/h), Engine Temp (°C), and Oil Pressure (atm).
* **The "Chef" Logic:** Uses complex `if/elif` gates to detect critical thresholds (e.g., Police Radar ranges, Critical Engine Overheating).
* **The Black Box:** Automatically logs anomaly data into a persistent `telemetry_logs.txt` file for forensic review.

## 📊 Phase 2: The Data Refinery & Visualization
Transforms 500+ rows of raw generated data into actionable business intelligence using **Pandas**.
* **Intentional Data Corruption:** Simulated real-world messy data by injecting `NaN` voids, typographical errors ("Trakk" instead of "Track"), and impossible sensor glitches (999 km/h).
* **Data Cleaning :** Utilized `.loc`, `.isna()`, `.fillna()`, and `.replace()` to mathematically patch missing data (using fleet averages) and recalibrate broken sensors without losing rows.
* **Visualization:** Built color-coded Seaborn Barplots and Matplotlib Pie Charts to visually prove correlations between specific driver modes and engine overheating risks.

---
## Phase 3: Baseline Machine Learning Model

With a clean and structured dataset in place, the pipeline transitions into predictive modeling to evaluate whether engine health can be inferred from telemetry signals.

###  Dataset Expansion
To move beyond synthetic validation, external datasets were introduced:
* An initial Kaggle dataset (~1000 rows) revealed **extremely weak predictive signals (~23.5% accuracy)**, highlighting poor feature separability.
* A larger dataset (~19,500 rows) was later used to improve statistical reliability and model training.

This iterative approach ensured the model was tested under **increasingly realistic conditions**.

### ⚙️ Feature Selection
The following telemetry signals were used as baseline inputs:
* Engine RPM
* Lubricating Oil Pressure
* Fuel Pressure
* Coolant Pressure
* Lubricating Oil Temperature
* Coolant Temperature

These represent core mechanical indicators of engine health.

### Model Architecture
The baseline model uses a **Random Forest** classifier. 

**Why Random Forest?**
* Handles non-linear relationships
* Robust to noisy data
* Requires minimal preprocessing
* Provides feature importance insights

### 🔄 Pipeline Flow
`Load Data -> Train/Test Split -> Feature Scaling -> Model Training -> Prediction -> Evaluation`

###  Data Leakage Prevention
* Scaling was applied using **training data only** (`fit_transform` on train, `transform` on test).
* Ensures the model does not gain unfair knowledge of unseen data.

### 📊 Baseline Results
* **Accuracy:** ~63.5% - Prioritized Recall over Accuracy to ensure engine failures are not missed (False Negatives).
* **Observation:** Model showed **bias toward the majority class** (healthy engines).

> ** Key Insight:** Despite multiple sensors, the model struggled to clearly separate healthy vs faulty engines. **Raw telemetry signals alone are insufficient for high-confidence failure prediction.**

---

## Phase 4: Imbalance Handling & Model Optimization

To address bias toward the majority class, advanced data balancing techniques were applied.

### 📉 The Imbalance Problem
Dataset distribution: `Healthy Engines > Faulty Engines`

This caused the model to:
* favor "healthy" predictions
* inflate accuracy while missing actual failures

###  Solution: SMOTE
Applied **Synthetic Minority Over-sampling Technique (SMOTE)**.

**How It Works:**
* Generates synthetic minority (faulty) samples
* Interpolates between existing fault cases
* Balances dataset without duplication
  
Note: SMOTE was applied only on training data to prevent evaluation leakage and maintain realistic performance estimation.

### ⚙️ Updated Pipeline
`Train/Test Split -> Apply SMOTE (Train Only) -> Scaling -> Model Training -> Evaluation`

> **⚠️ Critical Rule:** SMOTE is applied **ONLY** on training data to avoid corrupting evaluation integrity.

### 📊 Optimized Results
* **Accuracy:** ~62.8% (slight decrease)
* **Recall (Fault Detection):** significantly improved

###  Trade-off Analysis
| Metric | Effect |
| :--- | :--- |
| **Accuracy** | ↓ Slightly Decreased |
| **Recall** | ↑ Significantly Improved |
| **Safety** | ↑ Maximized |

System reliability improves by prioritizing fault detection over overall accuracy
> **Key Insight:** A lower accuracy model can be better if it detects more real failures. This is critical in safety systems where **missing a fault = catastrophic**, but **false alarms = acceptable**.

###  Model Output & Deployment
The trained model is exported as a serialized file: `Autobahn_ai_model.joblib`. 
This represents the final trained AI system, ready for reuse, deployment, and integration into applications.

---

## 🏁 Final Conclusions

This project demonstrates a full-stack journey from raw simulation to real-world ML limitations:

**✔️ What Worked**
* End-to-end ML pipeline construction
* Handling noisy and imbalanced data
* Understanding evaluation beyond accuracy

**⚠️ What Failed (and why it matters)**
* Sensor data alone lacked strong predictive power
* Even advanced techniques could not exceed ~60-65% accuracy

> **🧭 Core Learning:** The limiting factor in Machine Learning is often the data, not the model.

###  Future Improvements
* Incorporate vibration/acoustic sensors
* Use time-series modeling (trend-based failure detection)
* Apply advanced models (e.g., sequence models, deep learning)
* Transition to high-quality datasets (e.g., aerospace telemetry)

### 📌 Project Summary
This is not just a predictive model. It is a systematic investigation into how data quality, feature design, and imbalance affect real-world Machine Learning systems.

