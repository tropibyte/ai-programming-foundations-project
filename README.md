# AI Programming Foundations — Reproducible Data Workflow

**Repository:** https://github.com/tropibyte/ai-programming-foundations-project

## Project Description

This project implements a complete, reproducible data workflow on the Titanic
passenger dataset using Python, pandas, and Seaborn/Matplotlib. It ingests the raw
CSV, applies documented cleaning functions, runs a reusable exploratory-analysis
routine, and produces four labeled visualizations of the dataset's survival
patterns. It is a foundation project — no models are trained — designed to be the
clean, auditable base that later ML, deep-learning, and agentic projects build on.

**What I built:** a modular Jupyter notebook (`data_workflow.ipynb`) with named,
docstring-documented functions for cleaning and exploratory analysis, plus a written
summary report (`module_summary.pdf`) with academic citations.

**Dataset:** Titanic — Machine Learning from Disaster (891 rows, 12 columns).
Source: https://www.kaggle.com/c/titanic

## How to Run the Project

**1. Install dependencies**

```bash
pip install -r requirements.txt
```

**2. Open and run the notebook**

```bash
jupyter notebook data_workflow.ipynb
```

Then run all cells in order (Cell → Run All, or Kernel → Restart & Run All). The
notebook expects `titanic.csv` in the same folder as `data_workflow.ipynb`. It
executes top-to-bottom with no errors and produces four figures inline.

## Reproducibility

- All dependencies are pinned in `requirements.txt`, generated with
  `pip freeze > requirements.txt`.
- A single random seed is set once at the top of the notebook.
- The raw dataframe is copied before any transformation, so every cleaning step is
  traceable to an untouched source.
- Version control: the repository
  (https://github.com/tropibyte/ai-programming-foundations-project) contains multiple
  commits tracking each task and a development branch (`feature/eda-enhancements`)
  beyond `main`.

## Reflection — Bias & Data Quality

Cleaning choices in this project can introduce bias if applied carelessly. `Age` was
imputed using the median within each `Pclass`×`Sex` group; this preserves real
structure but compresses within-group variance and concentrates imputed values at
the group medians, which could bias any downstream age-based analysis toward those
central values. The `Cabin` column was dropped (~77% missing) rather than imputed —
the right call to avoid fabricating data, but it discards whatever deck-location
signal Cabin carried, which may correlate with survival. More broadly, the dataset
only includes passengers with recorded manifest entries, so any conclusion describes
this historical record rather than a general population. To reduce these risks I kept
imputation transparent and grouped rather than global, dropped rather than guessed
the near-empty column, and documented every choice so a reviewer can audit its
downstream effect.

## Reflection — Future Integration

**How this workflow changes for machine learning:** The cleaning and EDA functions
stay, but I would add a feature-engineering stage (e.g. family size from
`SibSp`+`Parch`, title extracted from `Name`), encode categoricals, scale numeric
features, and split into train/test sets before fitting a classifier — with the same
seed-based reproducibility carried through to the model.

**Preparing for neural networks:** Deep models need more careful numeric preparation
— normalization/standardization of inputs, one-hot or embedding representations for
categoricals, and batching. The modular cleaning functions here become the first
stage of a preprocessing pipeline that feeds tensors, and reproducibility extends to
seeding the framework's RNG as well.

**Agentic automation potential:** Each function in this notebook is a discrete,
documented step, which makes the workflow a natural target for an agentic system: an
agent could profile missingness, choose and apply cleaning strategies, generate the
EDA summary, and draft interpretations, with the docstrings serving as tool
specifications. The human role shifts to reviewing the agent's decisions rather than
writing each step by hand.
