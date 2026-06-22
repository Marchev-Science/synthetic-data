# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Companion code repository for the synthetic data monograph by prof. Angel Marchev Jr., PhD. All demos are Jupyter notebooks organized by technique chapter. The primary working directory is `/home/junior/development/synth_data`.

## Running Notebooks

```bash
pip install -r requirements.txt
jupyter notebook
# or for a specific notebook:
jupyter notebook notebooks/04_cholesky/cholesky_decomposition.ipynb
```

To inspect a notebook as a Python script:

```bash
jupyter nbconvert --to script <notebook>.ipynb --stdout
```

## Repository Structure

```
notebooks/
  01_imputation/     ← missing value techniques
  02_oversampling/   ← imbalanced dataset handling
  03_monte_carlo/    ← random simulation methods
  04_cholesky/       ← non-random: correlation-based synthesis
  05_copula/         ← non-random: joint distribution synthesis
  06_gan/            ← random: deep generative model
  07_fuzzy_matching/ ← dataset linking and concatenation
data/                ← Bulgarian financial/demographic CSVs (~15k rows, ~48 features)
models/              ← pre-trained TF SavedModel (generator.tf, discriminator.tf)
fuzzymatcher/        ← vendored fuzzy matching library
reference/           ← sklearn synthesis function reference
```

## Notebook Paths and Data Files

Notebooks under `notebooks/` reference data with `../../data/` relative paths. The GAN notebook (`06_gan/gan_synthetic_data.ipynb`) reads `../../data/sim_data_vasko - rab - sim_data.csv` and saves outputs to `../../data/` and `../../models/`.

`notebooks/07_fuzzy_matching/02_data_link.ipynb` requires two external Excel files (`demogr-filtered.xlsx`, `big5_data.xlsx`) that are not in this repo — see the warning cell at the top of that notebook.

## Algorithm Taxonomy

From the monograph (PPTX Slide 19):
- **Random methods**: Monte Carlo (`03_monte_carlo/`), GAN (`06_gan/`)
- **Non-random methods**: Cholesky (`04_cholesky/`), Inverse Copula (`05_copula/`)
- **Supporting**: Imputation (`01_imputation/`), Oversampling (`02_oversampling/`), Fuzzy Matching (`07_fuzzy_matching/`)

## fuzzymatcher Library

`fuzzymatcher/` is a vendored copy of fuzzymatcher 0.0.5. Its public API (`__init__.py`) exposes:
- `link_table(df_left, df_right, left_on, right_on, ...)` — scored link table
- `fuzzy_left_join(df_left, df_right, left_on, right_on, ...)` — left join result

Pipeline: `DataPreprocessor` → `DataGetter` (SQLite TF-IDF candidate index) → `Scorer` → `Matcher`.

## Pre-trained Models

```python
import tensorflow as tf
generator     = tf.keras.models.load_model('models/generator.tf')
discriminator = tf.keras.models.load_model('models/discriminator.tf')
```
