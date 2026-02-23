# CIDR Mean Income Prediction (Stacked / Hierarchical AutoGluon)

This repository contains work from my **CIDR Data Science internship**. The goal is to predict **regional mean income** using a stacked (multi-stage) modeling pipeline built with **AutoGluon Tabular**. We train models at different **sector × geography** granularities and **reuse their predictions as features** in later stages to improve the final model.

> **Note on data:** The original data is not included in this repository. The project is based on **aggregated LAMAS tax data**, enriched with external socio-economic information (CBS) and additional engineered features described below.

---

## Pipeline overview

### High-level approach
1. Train an initial model at a coarser level.
2. Generate predictions and add them as **new features**.
3. Train more granular models and again feed predictions forward.
4. Train the final model at the most detailed level and produce predictions.


