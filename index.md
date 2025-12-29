title: "Modelling CO₂ emissions per capita from human development and energy indicators"
description: "Country-level panel data, regression, SHAP, semantic search and graph tools."
theme: jekyll-theme-minimal

**Quick navigation links to:**  

<a href="#notebooks" style="text-decoration:none; padding:3px 8px; border-radius:999px; border:1px solid #ddd;">Notebooks</a> 
<a href="#figures" style="text-decoration:none; padding:3px 8px; border-radius:999px; border:1px solid #ddd;">Figures</a> 
<a href="#environments" style="text-decoration:none; padding:3px 8px; border-radius:999px; border:1px solid #ddd;">Environments</a> 
<a href="https://github.com/MNN1999/MLxSustainability" style="text-decoration:none; padding:3px 8px; border-radius:999px; border:1px solid #ddd;">GitHub repo</a>

---

## Overview

This project analyses how territorial CO₂ emissions per capita relate to human development, energy access and renewable energy across 204 countries (1995–2021), using a fully reproducible data and modelling pipeline.

<table>
  <tr>
    <td>
      <img
        src="https://github.com/MNN1999/MLxSustainability/raw/main/figures/pairplot_modeldata.png"
        alt="Pairplot of key model variables"
        style="width:100%; max-width:420px; height:260px; object-fit:contain;"
      />
      <div><sub><b>Pairplot of key variables</b> – relationships between log CO₂, income, education, health and energy indicators</sub></div>
    </td>
    <td>
      <img
        src="https://github.com/MNN1999/MLxSustainability/raw/main/figures/model_comparison_table.png"
        alt="Model performance: Linear Regression vs Random Forest"
        style="width:100%; max-width:420px; height:260px; object-fit:contain;"
      />
      <div><sub><b>Model performance</b> – linear regression vs Random Forest (R² and MSE on test set)</sub></div>
    </td>
  </tr>
  <tr>
    <td>
      <img
        src="https://github.com/MNN1999/MLxSustainability/raw/main/figures/shap_summary_beeswarm.png"
        alt="SHAP beeswarm summary"
        style="width:100%; max-width:420px; height:260px; object-fit:contain;"
      />
      <div><sub><b>SHAP beeswarm</b> – distribution of feature contributions across all country–year observations</sub></div>
    </td>
    <td>
      <img
        src="https://github.com/MNN1999/MLxSustainability/raw/main/figures/shap_summary_bar.png"
        alt="SHAP bar summary"
        style="width:100%; max-width:420px; height:260px; object-fit:contain;"
      />
      <div><sub><b>SHAP global importance</b> – mean absolute contribution by feature</sub></div>
    </td>
  </tr>
</table>

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

For full technical details, see the [README](README.md) and notebooks in the project repository.


---

## Notebooks

Core analysis and tools:

- **Data Preparation** [View on GitHub](https://github.com/MNN1999/MLxSustainability/blob/main/rawdata_processing.ipynb)

- **EDA, Modelling & SHAP** [View on GitHub](https://github.com/MNN1999/MLxSustainability/blob/main/Eda_and_modelling.ipynb)

- **Semantic search & Indicator Network Graph** [View on GitHub](https://github.com/MNN1999/MLxSustainability/blob/main/Semantics_and_networkgraph.ipynb)

---

## Figures

Browese all images directly in the [figures folder on GitHub](https://github.com/MNN1999/MLxSustainability/tree/main/figures).

---

## Environments

Reproducible environment specs:

- Modelling + SHAP: [`Modelling and SHAP Environment`](https://github.com/MNN1999/MLxSustainability/blob/main/env/ds-shap_environment.yml)
- Semantic search + graph: [`Semantic Environment`](https://github.com/MNN1999/MLxSustainability/blob/main/env/ds-text_environment.yml)
 
