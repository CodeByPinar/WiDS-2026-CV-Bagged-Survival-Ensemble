# 🔥 WiDS 2026 Survival Hybrid

<p align="center">
  <img src="https://img.shields.io/badge/WiDS-2026-8A2BE2?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Public_LB-0.96841-success?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Survival_Modeling-Time_to_Event-blue?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Ensemble-Hybrid-orange?style=for-the-badge" />
</p>

---

## 📌 Project Overview

This repository contains my full survival modeling solution for the **WiDS Global Datathon 2026**.

The challenge focuses on predicting the probability that a wildfire will threaten evacuation zones within multiple time horizons:

* 12 hours
* 24 hours
* 48 hours
* 72 hours

using only the first 5 hours of ignition signals.

This solution achieved:

# 🏆 **Public Leaderboard Score: 0.96841**

---

# 🧠 Technical Architecture

```
                ┌──────────────────────────────┐
                │     Feature Engineering      │
                │  log transforms + ratios     │
                └──────────────┬───────────────┘
                               │
                               ▼
        ┌────────────────────────────────────────────┐
        │ Gradient Boosting Survival Analysis (GBSA)│
        │ Multi-seed CV bagging (7 seeds × 5 folds) │
        └──────────────┬─────────────────────────────┘
                       │
                       ▼
      ┌──────────────────────────────────────┐
      │ IPCW Weighted LightGBM (24h, 48h)  │
      │ Horizon-specific calibration         │
      └──────────────┬──────────────────────┘
                     │
                     ▼
         ┌────────────────────────────────┐
         │   Metric-Aware Hybrid Blend   │
         │   Weight grid search tuning   │
         └──────────────┬─────────────────┘
                        │
                        ▼
               Final Multi-Horizon Output
```

---

# ⚙️ Modeling Strategy

## 1️⃣ Survival First Approach

Instead of treating this as pure classification, the problem was modeled as **time-to-event prediction**.

Core survival model:

* Gradient Boosting Survival Analysis (GBSA)
* Handles right-censoring naturally
* Outputs full survival curves

---

## 2️⃣ IPCW Horizon Calibration

For mid-term horizons (24h & 48h):

* Converted survival to binary tasks
* Removed unknown censored samples
* Applied IPCW (Inverse Probability of Censoring Weighting)
* Trained LightGBM classifiers

Purpose:
Improve weighted Brier performance without harming ranking quality.

---

# 📊 Hybrid Metric Optimization

Competition metric approximated as:

$$
Score = 0.3 \times C\text{-index} + 0.7 \times (1 - Weighted\ Brier)
$$

Where:

* C-index → ranking stability
* Weighted Brier → calibration accuracy

Weights tuned via grid search.

---

# 🔒 Probability Monotonicity

Multi-horizon consistency enforced:

$$
P(12h) \le P(24h) \le P(48h) \le P(72h)
$$

This guarantees logical and operational reliability.

---

# 🧪 Stability Engineering

✔ Multi-seed CV bagging
✔ Narrow weight search space
✔ Clipped domain-safe survival evaluation
✔ Probability smoothing for 72h

---

# 📈 Results Summary

| Metric     | Value       |
| ---------- | ----------- |
| Public LB  | **0.96841** |
| OOF Hybrid | ~0.974      |
| Seeds Used | 7           |
| Folds      | 5           |

---

# 📦 Dependencies

```
numpy
pandas
scikit-learn
lightgbm
scikit-survival
lifelines
```

---

# 🚀 Key Insights

* Survival modeling outperforms naive classification
* Metric-aware blending is critical
* Calibration and ranking must be balanced
* Multi-horizon enforcement increases stability

---

# 🔗 Kaggle Notebook

[https://www.kaggle.com/code/pinuto/wids-2026-cv-bagged-survival-hybrid-0-96841-lb](https://www.kaggle.com/code/pinuto/wids-2026-cv-bagged-survival-hybrid-0-96841-lb)

---

# 🏅 Closing Note

This project focuses on metric understanding, ensemble stability, and real-world interpretability in survival modeling for wildfire risk forecasting.

Built with precision, calibration focus, and competitive engineering mindset.

---

<p align="center">
  <img src="https://img.shields.io/badge/Built_with-Survival_Modeling-blue?style=flat-square" />
  <img src="https://img.shields.io/badge/Focus-Metric_Optimization-green?style=flat-square" />
  <img src="https://img.shields.io/badge/Approach-Hybrid_Ensemble-orange?style=flat-square" />
</p>
