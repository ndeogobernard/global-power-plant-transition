# The Global Energy Transition Through Power Plants

A personal data-mining project (climate tech) by **Bernard Issifu**: a data-mining study of the world's power plants:
predicting a plant's fuel type from its basic attributes, and grouping countries by their electricity
generation mix. Every analysis is written in both **Python** and **R**.

## Dataset

**Global Power Plant Database**, version 1.3.0. Global Energy Observatory, Google, KTH Royal Institute
of Technology in Stockholm, Enipedia, World Resources Institute (2021).
Source: <https://github.com/wri/global-power-plant-database>.
Licence: **CC BY 4.0**. Attribution as above; the file in `data/` is unmodified.

- `data/global_power_plant_database.csv`: 34,936 plants in 167 countries, 36 columns (capacity,
  location, primary and secondary fuels, commissioning year, owner, source, reported generation
  2013–2019, estimated generation 2013–2017)
- The database is a **2021 snapshot**; plants built since then are not included.
- `data/processed/`: prepared data written by Part 1 (not committed; run Part 1 first, then Parts 2–3)

## Questions

1. **Can a plant's primary fuel be predicted from its capacity, location and commissioning year?**
   Multi-class classification (multinomial logistic regression, decision tree, random forest) of seven
   fuel groups, evaluated with macro-F1, with random and country-grouped cross-validation and a leakage
   check.
2. **Which groups of countries share similar generation mixes?** k-means clustering of 120 countries
   (at least 1 GW) by their capacity shares in eight fuel groups, with the elbow and silhouette methods
   and a hierarchical-clustering comparison.

## Contents

| Part | Python | R |
|---|---|---|
| 1. Data preparation and exploratory analysis | [`python/01_data_preparation_eda.ipynb`](python/01_data_preparation_eda.ipynb) | [`r/01_data_preparation_eda.Rmd`](r/01_data_preparation_eda.Rmd) ([HTML](r/01_data_preparation_eda.html)) |
| 2. Question 1: predicting fuel type | [`python/02_q1_classification.ipynb`](python/02_q1_classification.ipynb) | [`r/02_q1_classification.Rmd`](r/02_q1_classification.Rmd) ([HTML](r/02_q1_classification.html)) |
| 3. Question 2: clustering countries | [`python/03_q2_clustering.ipynb`](python/03_q2_clustering.ipynb) | [`r/03_q2_clustering.Rmd`](r/03_q2_clustering.Rmd) ([HTML](r/03_q2_clustering.html)) |

**Report:** [`report/issifu_bernard_power_plants_report.pdf`](report/issifu_bernard_power_plants_report.pdf)
(5 pages; Markdown source and `build_report.sh` alongside). Document-kit (LaTeX) style:
[`report/kit-version/issifu_bernard_power_plants_report.pdf`](report/kit-version/issifu_bernard_power_plants_report.pdf).
Figures and result tables from both languages are in `report/figures/`.

## Key results

- **Data:** solar has the most plants (10,665) but 3.3% of capacity; coal has 2,330 plants and 34.4%.
  Commissioning year is missing for 50.1% of plants. Renewables' share of new capacity rose from 13.6%
  in the 2000s to 25.8% in the 2010s and 75.4% in 2020. Python and R produce the identical prepared
  dataset.
- **Question 1:** on a random split the random forest reaches accuracy 0.782 and macro-F1 0.739
  (decision tree 0.525, logistic regression 0.412; R forest 0.684). Location and capacity matter most.
  With countries held out, macro-F1 falls to 0.332 (R 0.330), so the model learns where fuels are built
  rather than general rules. Including WRI's estimated capacity factor would give 0.965, but that
  feature leaks the fuel type.
- **Question 2:** five clusters (k = 5, silhouette 0.382): hydro-dominated (32 countries), coal-dominated
  (18, incl. China and India, 35% of capacity), gas-dominated (25), oil-dominated (11) and diversified
  (34, incl. the USA, Germany, Japan, France). No cluster is led by wind or solar. R assigns 119 of 120
  countries to the same cluster; adjusted Rand index with hierarchical clustering 0.623 (R 0.646).

## How to run

- **Python:** from `python/`, run the notebooks in order (pandas, numpy, matplotlib, seaborn,
  scikit-learn).
- **R:** from `r/`, render each file in order with `rmarkdown::render("01_data_preparation_eda.Rmd")`
  and so on (dplyr, tidyr, ggplot2, nnet, rpart, randomForest, cluster).
- **Report:** from `report/`, run `./build_report.sh` (needs pandoc and Chromium); for the LaTeX
  version, run the document kit's `make_pdf.py` on `report/kit-version/issifu_bernard_power_plants_report.md`.
