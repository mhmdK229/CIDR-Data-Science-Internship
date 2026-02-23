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

### Stacked modeling flow (visual)

```mermaid
flowchart TD
  A[Input tables (aggregated)<br/>• LAMAS tax aggregates<br/>• Sector codes (3-digit / 4-digit)<br/>• Geography: District / Subdistrict<br/>• CBS socio-economic features<br/>• Sector-name text embeddings + cosine similarity] --> B

  B[Stage 1 Model<br/>Level: 3-digit sector + District<br/>Target: Mean income] --> C[Stage 1 Predictions<br/>(added as features)]

  C --> D[Stage 2a Model<br/>Level: 3-digit sector + District<br/>Features include Stage 1 preds]
  C --> E[Stage 2b Model<br/>Level: 4-digit sector + Subdistrict<br/>Features include Stage 1 preds]

  D --> F[Stage 2a Predictions<br/>(added as features)]
  E --> G[Stage 2b Predictions<br/>(added as features)]

  F --> H[Final Model<br/>Level: 4-digit sector + Subdistrict<br/>Features include Stage 1 + Stage 2 predictions<br/>+ CBS + embedding similarity]
  G --> H

  H --> I[Final Predictions]
