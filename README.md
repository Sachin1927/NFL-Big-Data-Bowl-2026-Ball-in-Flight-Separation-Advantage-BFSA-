# 🏈 NFL Big Data Bowl 2026: Ball-in-Flight Separation Advantage (BFSA)

[![Python](https://img.shields.io/badge/Python-3.9%2B-blue?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![Kaggle Competition](https://img.shields.io/badge/Kaggle-Big%20Data%20Bowl%202026-20BEFF?style=for-the-badge&logo=kaggle&logoColor=white)](https://www.kaggle.com/competitions/nfl-big-data-bowl-2026)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)]()

> **Analytics Competition Entry:** Quantifying receiver openness during the critical "Ball-in-Flight" phase using Next Gen Stats and physics-based modeling.

---

## 📖 Overview

The downfield pass is the "crown jewel" of American sports. The **NFL Big Data Bowl 2026 (University Track** challenges participants to analyze player movement during the specific window when the ball is in the air—from the quarterback's release to the catch (or incompletion).

**This project introduces "BFSA" (Ball-in-Flight Separation Advantage)**: A novel spatial metric that quantifies a receiver's ability to create separation from defenders *after* the ball is thrown, providing coaches with a granular efficiency score for route running under pressure.

---

## 📊 Key Features & Methodology

### 1. The Challenge
Standard metrics track separation at the moment of the catch. However, they fail to capture the *dynamic* battle for position while the ball is airborne. A receiver might be open at the throw but covered at the catch—or vice versa.

### 2. The Solution: BFSA Metric
We processed high-frequency **Next Gen Stats (NGS)** tracking data (10 frames/second) to build a physics-aware model:
* **Trajectory Modeling:** Modeled the ball's ballistic trajectory to predict the "Landing Zone" (x, y coordinates).
* **Dynamic Separation:** Calculated the Euclidean distance between the target receiver and the nearest defender at every 0.1s interval during flight.
* **Advantage Scoring:** `BFSA` accumulates the separation advantage over the duration of the flight, weighted by the ball's proximity to the arrival point.

⚙️ Methodology1. DatasetWe utilized the Big Data Bowl 2026 dataset, specifically:input_2023_wXX.csv: Tracking from snap until throw.output_2023_wXX.csv: Tracking from throw until catch/incompletion.Focus: Targeted Receiver vs. Nearest Defender (Defensive Coverage role).2. Core Geometry & FormulasThe metric calculates the spatial relationship between players and the Ball Landing Point ($B$) for every frame $t$ the ball is in the air.Definitions:$R_t$: Receiver position at frame $t$.$D_t$: Nearest Defender position at frame $t$.$B$: Ball Landing coordinates $(x_{ball}, y_{ball})$.The BFSA Equation:At the final frame $T$ (arrival), the metric is defined as:$$BFSA = d_{r,T} - d_{d,T}$$Where $d$ represents the Euclidean distance to the ball.If $BFSA > 0$: The Receiver is closer (Offensive Advantage).If $BFSA < 0$: The Defender is closer (Defensive Advantage).3. Secondary MetricsReceiver Route Efficiency ($RE_r$):$$RE_r = \frac{\text{Actual Path Length}}{\text{Straight Line Distance}}$$Values close to 1.0 indicate efficient, direct routes to the ball.Closing Speed ($CS$):Measured for both Receiver ($CS_r$) and Defender ($CS_d$) to track convergence velocity (yards/sec).

### 3. Tech Stack
* **Core:** Python, Pandas, NumPy
* **Data Processing:** Polars (for high-volume tracking data efficiency)
* **Visualization:** Matplotlib, Seaborn, Plotly (Interactive field mapping)
* **Physics Engine:** Custom vector logic for player velocity and acceleration profiles.

---

## 📂 Repository Structure

```text
├── input/                  # (GitIgnored) Raw NFL Next Gen Stats data
├── notebooks/              # Jupyter Notebooks for analysis and modeling
│   └── BFSA_Analysis.ipynb # Main analysis logic
├── output/                 # Generated CSVs and metric reports
├── src/                    # (Optional) Helper scripts for data cleaning
├── .gitignore              # Ignores large data files (*.csv)
├── requirements.txt        # Python dependencies
└── README.md               # Project documentation

🚀 Getting Started
Prerequisites
Python 3.9+

Jupyter Notebook / Lab

Installation
Clone the repo
git clone [https://github.com/sachin1927/nfl-big-data-bowl-2026.git](https://github.com/sachin1927/nfl-big-data-bowl-2026.git)
cd nfl-big-data-bowl-2026

Install dependencies
pip install -r requirements.txt

Get the Data

Download the official dataset from the Kaggle Competition Page.

Place the .csv files inside the input/ folder.

Run the Analysis

jupyter notebook notebooks/BFSA_Analysis.ipynb

📈 Results & Business Impact
For Coaches: BFSA identifies "Contested Catch Specialists" who may not have high top speed but consistently maintain separation leverage during ball flight.

For Scouting: Provides a quantitative metric to evaluate rookie receivers' route-running discipline beyond the 40-yard dash time.

Visualization: The notebook includes 2D animated plays showing the BFSA score evolving in real-time as the play unfolds.

🔗 Links & References
Kaggle Notebook: View the code on Kaggle

Input Data: Kaggle Dataset

Competition: NFL Big Data Bowl 2026
👨‍💻 Author
Sachin
Kaggle
