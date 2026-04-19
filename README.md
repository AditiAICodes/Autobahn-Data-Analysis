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
* **Data Cleaning (The Janitor):** Utilized `.loc`, `.isna()`, `.fillna()`, and `.replace()` to mathematically patch missing data (using fleet averages) and recalibrate broken sensors without losing rows.
* **Visualization:** Built color-coded Seaborn Barplots and Matplotlib Pie Charts to visually prove correlations between specific driver modes and engine overheating risks.

---

