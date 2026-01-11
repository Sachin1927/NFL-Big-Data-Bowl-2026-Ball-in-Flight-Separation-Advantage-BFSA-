# 🏈 NFL Big Data Bowl 2026: Ball-in-Flight Separation Advantage (BFSA)

[![Python](https://img.shields.io/badge/Python-3.9%2B-blue?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![Kaggle Competition](https://img.shields.io/badge/Kaggle-Big%20Data%20Bowl%202026-20BEFF?style=for-the-badge&logo=kaggle&logoColor=white)](https://www.kaggle.com/competitions/nfl-big-data-bowl-2026)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)]()

> **Analytics Competition Entry:** Quantifying receiver openness during the critical "Ball-in-Flight" phase using Next Gen Stats and physics-based modeling.

---

## 📖 Overview

The downfield pass is the "crown jewel" of American sports. The **NFL Big Data Bowl 2026** challenges participants to analyze player movement during the specific window when the ball is in the air—from the quarterback's release to the catch (or incompletion).

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

## 🧮 Mathematical Foundation

To quantify receiver openness beyond simple distance, we implemented a physics-based model that accounts for velocity vectors and trajectory dynamics.

### 1. Instantaneous Separation ($S_t$)
At any given timestamp $t$, the Euclidean distance between the Receiver ($R$) and the nearest Defender ($D$) is calculated using their Cartesian coordinates $(x, y)$:

$$S_t = \sqrt{(x_R(t) - x_D(t))^2 + (y_R(t) - y_D(t))^2}$$

### 2. Projected Closing Speed ($PCS$)
Mere speed difference is insufficient. We calculate the **Closing Speed** by projecting the relative velocity vector ($\vec{V}_{rel}$) onto the unit displacement vector ($\hat{u}$). This determines if the defender is actually gaining ground on the receiver.

$$\text{PCS}_t = (\vec{V}_D - \vec{V}_R) \cdot \underbrace{\left( \frac{\vec{P}_R - \vec{P}_D}{\| \vec{P}_R - \vec{P}_D \|} \right)}_{\text{Unit Vector } \hat{u}}$$

* **Interpretation:** A positive $PCS$ indicates the defender is closing the gap. A negative value implies the receiver is creating separation.

### 3. The BFSA Metric (Integral)
The final **Ball-in-Flight Separation Advantage** score is the time-weighted integral of separation, penalized by the defender's closing speed, over the duration of the ball's flight ($T_{flight}$).

$$\text{BFSA} = \int_{t=0}^{T_{flight}} \left( S_t - \alpha \cdot \text{PCS}_t \right) \cdot w(t) \, dt$$

* **$S_t$**: Instantaneous Separation
* **$\alpha$**: Weighting factor for closing speed threat
* **$w(t)$**: Time-decay function (higher weight as ball approaches arrival)

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
