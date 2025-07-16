# Credit Score Analysis and Insights

## Overview

This document presents an analytical overview of the credit scores assigned to Aave V2 wallets using our ML-powered DeFi scoring framework.  
Scores span from 0 to 1000, with higher values reflecting responsible, low-risk activity, and lower values indicating risky, inactive, or bot-like wallet behavior.

Our scoring leverages engineered wallet-level features, real on-chain risk signals, and XGBoost-based modeling. Wallets are classified into bands: **Prime**, **Near Prime**, **Subprime**, and **Deep Subprime**.

---

## 1. Score Distribution

### 1.1 Histogram (0–1000 with bins of 100)

<img width="1000" height="600" alt="score_hist_by_100" src="https://github.com/user-attachments/assets/1d00aae2-7ee8-4b86-b1e3-99e3326b2046" />

The histogram (`score_hist_by_100.png`) shows how wallet scores are distributed in intervals of 100. Most addresses lie within the lower ranges, with a long rightward tail. This aligns with common DeFi usage: many users show limited activity, while only a subset exhibits consistent, responsible engagement.

### 1.2 Kernel Density Estimation (KDE) Overlay

<img width="1000" height="600" alt="score_hist_kde" src="https://github.com/user-attachments/assets/0a5cb2e7-a715-424c-95c3-5594b674282c" />

The KDE line highlights the core mass of scores around the baseline, with a gradual dropoff at higher scores. No unusual gaps or double peaks are observed.

---

## 2. Band Classification and Breakdown

### 2.1 Number of Wallets by Credit Tier

<img width="800" height="500" alt="score_band_bar" src="https://github.com/user-attachments/assets/a5cd5be5-06f3-4eed-b149-dced91845d73" />

The `score_band_bar.png` plot shows user counts in each credit group. The majority are in **Deep Subprime** due to minimal activity, while the remaining tiers host more actively engaged users. This categorization supports downstream credit evaluation logic.

### 2.2 Score Distribution Within Bands

<img width="1000" height="600" alt="score_band_boxplot" src="https://github.com/user-attachments/assets/461d13b7-fce9-449d-817d-c5a899576680" />

In `score_band_boxplot.png`, we observe high median scores and low variance for **Prime** wallets, while **Deep Subprime** clusters tightly around a fixed baseline (often ~350). Intermediate bands display wider score spreads.

---

## 3. Score-Based Wallet Behavior Patterns

### 3.1 Activity Span vs Score Range

<img width="1000" height="400" alt="log_activity_duration_days_by_score_bin" src="https://github.com/user-attachments/assets/1df21c1f-3f32-4d9b-807f-6ec76c1f9d78" />

The chart shows that wallets with longer durations tend to have higher scores, as longevity is positively weighted in the model.

### 3.2 Repayment Frequency by Score

<img width="1000" height="400" alt="log_num_repays_by_score_bin" src="https://github.com/user-attachments/assets/7b0f0904-6ea8-48eb-9d0a-cc78a378c39c" />

Higher-score groups show a greater number of repayments, reinforcing repayment behavior as a signal of reliability.

### 3.3 Liquidations by Score Group

<img width="1000" height="400" alt="log_num_liquidations_by_score_bin" src="https://github.com/user-attachments/assets/9d881147-4fbe-452f-baed-c206bbfdd114" />

The count of liquidations declines as wallet scores rise. Low-score users are more frequently liquidated.

### 3.4 Utilization Rate Trends

<img width="1000" height="400" alt="utilization_rate_by_score_bin" src="https://github.com/user-attachments/assets/1f6875bc-5466-4279-8203-0cc40610b8a4" />

Wallets with high utilization ratios generally fall in the lower or mid-score ranges, indicating riskier borrowing behaviors.

### 3.5 Borrow-to-Repay Ratio

<img width="1000" height="400" alt="borrow_repay_ratio_by_score_bin" src="https://github.com/user-attachments/assets/3adbb01a-7c59-4b51-a036-6ba5f6fa15f1" />

High-score users display better-balanced borrow–repay ratios, suggesting more frequent repayment behavior.

---

## 4. Score Band Summaries

- **Deep Subprime (0–399):** Mostly inactive wallets. Few repays or borrows. Liquidations are rare but due to inactivity, not reliability.
- **Subprime (400–599):** Sporadic borrowing activity. Some repayments. Limited engagement over short spans.
- **Near Prime (600–699):** Sustained interaction. Multiple repayments. Broader token exposure. Low liquidation risk.
- **Prime (700+):** Consistently active wallets. Timely repayments, low utilization, and almost no liquidations.

---

## 5. Observations & Takeaways

- The model effectively segments DeFi wallet behavior and aligns with traditional credit scoring frameworks.
- "Prime" and "inactive" users are distinctly separated, helping with both transparency and risk mitigation.
- Visualization confirms model calibration and interpretability across all score levels.

---

## 6. Reproduction Instructions

To rerun this analysis or build upon it, follow the full data pipeline and setup steps detailed in the [README.md](./README.md) file.