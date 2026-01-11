# 🏈 NFL Big Data Bowl 2026: Ball-in-Flight Separation Advantage (BFSA)

[![Python](https://img.shields.io/badge/Python-3.9%2B-blue?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![Kaggle Competition](https://img.shields.io/badge/Kaggle-Big%20Data%20Bowl%202026-20BEFF?style=for-the-badge&logo=kaggle&logoColor=white)](https://www.kaggle.com/competitions/nfl-big-data-bowl-2026)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)]()

> **Analytics Competition Entry:** Quantifying receiver openness during the critical "Ball-in-Flight" phase using Next Gen Stats and physics-based modeling.

---

## 📖 Overview
### 3. Example Play (Game 2023090700, Play 101)
* **Scenario:** Deep Corner Route.
* **Dynamics:** The receiver's path showed smooth convergence...

<p align="center">
  <img src="images/play_animation.gif" alt="Animation of Play 101" width="800">
  <br>
  <em>Figure 3: Frame-by-frame visualization of the Receiver (Blue) vs Defender (Red) closing speed.</em>
</p>
The downfield pass is the "crown jewel" of American sports. The **NFL Big Data Bowl 2026 (University Track** challenges participants to analyze player movement during the specific window when the ball is in the air—from the quarterback's release to the catch (or incompletion).

**This project introduces "BFSA" (Ball-in-Flight Separation Advantage)**: A novel spatial metric that quantifies a receiver's ability to create separation from defenders *after* the ball is thrown, providing coaches with a granular efficiency score for route running under pressure.

---

## 📊 Key Features

### 1. The Challenge
Standard metrics track separation at the moment of the catch. However, they fail to capture the *dynamic* battle for position while the ball is airborne. A receiver might be open at the throw but covered at the catch—or vice versa.

### 2. The Solution: BFSA Metric
We processed high-frequency **Next Gen Stats (NGS)** tracking data (10 frames/second) to build a physics-aware model:
* **Trajectory Modeling:** Modeled the ball's ballistic trajectory to predict the "Landing Zone" (x, y coordinates).
* **Dynamic Separation:** Calculated the Euclidean distance between the target receiver and the nearest defender at every 0.1s interval during flight.
* **Advantage Scoring:** `BFSA` accumulates the separation advantage over the duration of the flight, weighted by the ball's proximity to the arrival point.

## ⚙️ Methodology
**Definitions:**
* $R_t$: Receiver position $(x, y)$ at frame $t$.
* $D_t$: Nearest Defender position $(x, y)$ at frame $t$.

<p align="center">
  <img src="images/bfsa_diagram.png" alt="BFSA Geometry Diagram" width="600">
  <br>
  <em>Figure 1: Visual representation of the Receiver vs. Defender distance vectors.</em>
</p>

**The BFSA Equation:**
### 1. Dataset
We utilized the **Big Data Bowl 2026** dataset, specifically focusing on the following files:
* `input_2023_wXX.csv`: Tracking data from the snap until the pass is thrown.
* `output_2023_wXX.csv`: Tracking data from the release of the pass until the catch or incompletion.
* **Scope:** We filtered the data to isolate interactions between the **Targeted Receiver** and the **Nearest Defender** (specifically those with a "Defensive Coverage" role).

### 2. Core Geometry & Formulas
The metric calculates the spatial relationship between players and the **Ball Landing Point ($B$)** for every frame ($t$) the ball is in the air.



**Definitions:**
* $R_t$: Receiver position $(x, y)$ at frame $t$.
* $D_t$: Nearest Defender position $(x, y)$ at frame $t$.
* $B$: Ball Landing coordinates $(x_{ball}, y_{ball})$.

**The BFSA Equation:**
To ensure that a **positive** score represents an offensive advantage, the metric is calculated as the difference between the defender's distance and the receiver's distance at the arrival frame ($T$):

$$BFSA = d_{defender,T} - d_{receiver,T}$$

Where $d$ represents the Euclidean distance to the ball's landing spot.

**Interpretation:**
* **If $BFSA > 0$:** The Receiver is closer to the ball (Offensive Advantage).
* **If $BFSA < 0$:** The Defender is closer to the ball (Defensive Advantage).
* **If $BFSA \approx 0$:** The players are equidistant (Contested/50-50 ball).

### 3. Secondary Metrics

#### Receiver Route Efficiency ($RE_r$)
Measures how directly the receiver traveled to the landing spot compared to a straight line.

$$RE_r = \frac{\text{Actual Path Length}}{\text{Straight Line Distance}}$$

* **Interpretation:** Values close to **1.0** indicate a highly efficient, direct route. Values significantly higher than 1.0 indicate wasted movement or zig-zagging while the ball was in the air.

#### Closing Speed ($CS$)
We calculated the **Closing Speed** for both the Receiver ($CS_r$) and Defender ($CS_d$) to track their convergence velocity (yards/second) towards the landing spot $B$.

$$CS = \frac{\Delta \text{Distance to Ball}}{\Delta \text{Time}}$$
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

### 📈 Results & Business Impact
### 1. BFSA & Pass Outcomes
Across the dataset, BFSA provided clear discrimination between outcomes...

<p align="center">
  <img src="images/bfsa_correlation.png" alt="Graph of BFSA vs Pass Outcome" width="700">
  <br>
  <em>Figure 2: Distribution of BFSA scores by pass result (Completed vs. Incomplete).</em>
</p>
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
