# MLxSustainability – CO₂, human development & energy

**Quick navigation**

- [Overview](#overview)
- [Notebooks](#notebooks)
- [Figures](#figures)
- [Environments](#environments)
- [GitHub repo](https://github.com/MNN1999/MLxSustainability)

---

## Overview

This project analyses how territorial CO₂ emissions per capita relate to human development, energy access and renewable energy across 204 countries (1995–2021), using a fully reproducible data and modelling pipeline.

The workflow includes:

- **Data ingestion and harmonisation**  
  Integrates country–year data from UNDP, World Bank and Our World in Data into a single, cleaned panel. Handles missingness and coverage (by year and variable), filters to a stable window (1995–2021), and drops weak or unreliable indicators.

- **Preprocessing and feature engineering**  
  Applies log-transformations to skewed variables (CO₂ per capita, income, child mortality), constructs a consistent set of development and energy features, and standardises inputs where required for model fitting and comparison.

- **Modelling and evaluation**  
  Implements a time-aware train/test split, fits a baseline linear regression and a non-linear Random Forest regressor, and compares performance using R² and MSE. The Random Forest achieves test R² ≈ 0.89 for log CO₂ emissions per capita.

- **Model explainability**  
  Uses tree-based feature importances and SHAP values to interpret how income, renewable energy, access to electricity, urbanisation and other development indicators jointly shape CO₂ emissions per capita, at both global and country–year levels.

- **AI tools for information retrieval and structure**  
  Builds a semantic search tool over indicator metadata using transformer-based sentence embeddings and vector search, and a small NetworkX graph linking indicators to thematic domains (emissions & energy, human development, demography, etc.).

For full technical details, see the [README](README.md) and notebooks in the `notebooks/` folder.


---

## Notebooks

Core analysis and tools:

- **01 – Data preparation & coverage**  
  `rawdata_processing.ipynb`  
  [View on GitHub](rawdata_processing.ipynb)

- **02 – EDA, modelling & SHAP**  
  `Eda_and_modelling.ipynb`  
  [View on GitHub](Eda_and_modelling.ipynb)

- **03 – Semantic search & indicator graph**  
  `Semantics_and_networkgraph.ipynb`  
  [View on GitHub](Semantics_and_networkgraph.ipynb)

---

## Figures

Selected outputs:

- Model performance comparison (linear vs Random Forest):  
  `figures/model_performance_rf_vs_linear.png`
- SHAP summary plots:  
  `figures/shap_summary_beeswarm.png`  
  `figures/shap_summary_bar.png`

You can browse all figures in the [`figures/`](figures/) folder.

---

## Environments

Reproducible environment specs:

- Modelling + SHAP: [`env/ds-shap_environment.yml`](env/ds-shap_environment.yml)
- Semantic search + graph: [`env/ds-text_environment.yml`](env/ds-text_environment.yml)
