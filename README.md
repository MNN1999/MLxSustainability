# CO₂ emissions, human development and energy transitions (SEI portfolio project)

This project explores how territorial CO₂ emissions per capita relate to human development and energy system characteristics across countries over 1995–2021. It combines:

- a cleaned country–year panel dataset from UNDP, World Bank and Our World in Data,
- regression models (linear and tree-based) to predict log CO₂ emissions per capita,
- model explainability with SHAP values,
- a small semantic-search tool for sustainability indicators using transformer embeddings, and
- a simple graph representation of indicators and themes.

The aim is to demonstrate skills that are relevant for SEI’s “AI tools for data extraction” role: data engineering, machine learning, model interpretation, semantic search and basic graph-based structures in a sustainability context.

---

## Data sources

The analysis relies on country–year data for 204 countries, roughly 1990–2024, restricted to 1995–2021 for modelling:

- **CO₂ emissions:** Our World in Data (tonnes per person and related CO₂ variables).
- **Human development variables:** UNDP (gross national income per capita, mean years of schooling).
- **Demographic and structural indicators:** World Bank (life expectancy, under-five mortality, population growth, urban population share).
- **Energy and access indicators:** World Bank / OWID (access to electricity, share of renewables in final energy consumption).

All merging, filtering and cleaning of the raw sources is done in a separate notebook and saved as a harmonised dataset `model_data_clean.parquet`.

---

## Methods

### 1. Data preparation and coverage

- Harmonised data for 204 UNDP countries, removing aggregates and non-countries.
- Analysed variable coverage by year and indicator.
- Selected **1995–2021** as a stable modelling window with high coverage across core variables.
- Dropped a weakly covered alternative accounting measure (`consumption_co2`) and kept:
  - IDs: `iso_code`, `country`, `year`
  - Outcome: `co2_per_capita`
  - Predictors: income, education, health, mortality, energy access, renewables, urbanisation, population growth.
- Within each country, missing values were interpolated over time where possible, and the few remaining incomplete rows for the outcome or predictors were dropped.

### 2. Transformations and exploratory analysis

- CO₂ per capita, income per capita and under-five mortality are strongly right-skewed, so log transforms are used:
  - `co2_per_capita_log = log(1 + co2_per_capita)`
  - `gni_pc_log = log(1 + gni_pc)`
  - `under5_mortality_log = log(1 + under5_mortality)`
- Descriptive statistics, correlation matrices and histograms show:
  - a strong positive association between income and emissions per capita,
  - strong co-movement among development indicators (education, health, access, urbanisation),
  - a negative relationship between renewable energy share and CO₂ emissions per capita.
- A composite pairplot confirms these patterns and supports using `co2_per_capita_log` as the main outcome.

### 3. Modelling: linear regression and Random Forest

The main modelling sample uses:

- Outcome: `co2_per_capita_log`
- Predictors:
  - `gni_pc_log`
  - `mys`
  - `life_expectancy`
  - `under5_mortality_log`
  - `electricity_access_pct`
  - `renewable_energy_pct`
  - `urban_population_pct`
  - `population_growth_pct`

A **time-based train/test split** is used:

- Train: 1995–2012
- Test: 2013–2021

Models:

1. **Baseline linear regression**

   - Features standardised (mean 0, unit variance) in a simple sklearn pipeline.
   - Performance:
     - Train R² ≈ 0.85, MSE ≈ 0.13
     - Test R² ≈ 0.79, MSE ≈ 0.15

2. **Random Forest regressor**

   - 300 trees, default depth, trained on the same features and split.
   - Performance on the test set:

   | model            | dataset | R²    | MSE   |
   | ---------------- | ------- | ----- | ----- |
   | LinearRegression | test    | 0.794 | 0.151 |
   | RandomForest     | test    | 0.888 | 0.082 |

   The Random Forest explains close to **89% of the variance** in log CO₂ emissions per capita in the test period, a clear improvement over the linear baseline.

### 4. Model explainability: feature importance and SHAP

Two types of feature importance are computed for the Random Forest:

- **Tree-based importance (mean decrease in impurity)**:

  - `gni_pc_log` is by far the most important feature,
  - followed by `renewable_energy_pct`,
  - with other predictors contributing smaller shares.

- **SHAP values (TreeExplainer)**:
  - Provide local, observation-specific attributions: for each country–year, each feature has a SHAP value indicating how much it pushes the prediction up or down.
  - The SHAP beeswarm plot shows:
    - higher income (`gni_pc_log`) strongly increases predicted log CO₂ per capita,
    - higher renewable energy shares tend to decrease predicted emissions,
    - higher electricity access, urbanisation, education and life expectancy generally increase emissions, capturing a development and infrastructure gradient,
    - under-five mortality and population growth have smaller average contributions.
  - The SHAP bar plot (mean absolute SHAP values) confirms the global ranking:
    - income as the dominant driver,
    - renewables as the second strongest predictor,
    - followed by electricity access and other development indicators.

SHAP therefore complements the standard feature importance by providing both **direction** and **local explanations**, and a robust global ranking based on actual contribution magnitudes.

---

## 3. Semantic search over indicator metadata

To illustrate AI-supported data extraction and retrieval, a simple semantic search tool is constructed for the indicators used in the analysis:

- A metadata table is defined with:
  - indicator IDs (e.g. `gni_pc_log`, `renewable_energy_pct`),
  - human-readable names,
  - short textual descriptions.
- Names and descriptions are embedded using the pre-trained `all-MiniLM-L6-v2` sentence-transformer model (Hugging Face).
- A k-nearest neighbour index with cosine distance is built on the embeddings.
- A helper function `search_indicators(query, top_k)`:
  - encodes a free-text query,
  - retrieves the closest indicator descriptions,
  - and returns a ranked list with similarity scores.

Example queries such as _“education and human capital”_, _“child health and mortality”_ or _“renewable energy and climate mitigation”_ retrieve the expected education, health, energy and emissions indicators. This demonstrates how transformer-based embeddings and vector search can be used to organise and discover indicators for evidence synthesis and sustainability research.

---

## 4. Simple graph representation of indicators and themes

To showcase a basic graph-based structure:

- A small NetworkX graph is built from the indicator metadata.
- Nodes:
  - indicator nodes (with attributes like name),
  - thematic nodes (e.g. “Emissions & energy”, “Human development”, “Infrastructure & access”, “Urbanisation”, “Demography”).
- Edges connect indicators to their themes.

This allows simple graph queries, for example:

- listing all indicators linked to “Emissions & energy”,
- finding which themes are associated with indicators whose names contain “mortality”,
- visualising small subgraphs such as the “Emissions & energy” theme and its indicators.

Although minimal, this graph representation illustrates how sustainability indicators and their relationships can be organised in a way that supports querying and integration with other tools.

---
