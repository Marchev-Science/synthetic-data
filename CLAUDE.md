# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Companion code repository for the synthetic data monograph by prof. Angel Marchev Jr., PhD. All demos are Jupyter notebooks organized by technique chapter. The primary working directory is `/home/junior/development/synth_data`.

## Running Notebooks

```bash
pip install -r requirements.txt
jupyter notebook
# or for a specific notebook:
jupyter notebook notebooks/ch08_cholesky/cholesky_decomposition.ipynb
```

To inspect a notebook as a Python script:

```bash
jupyter nbconvert --to script <notebook>.ipynb --stdout
```

## Repository Structure

```
notebooks/
  00_overview/       ← guided tour: runnable examples for every method (start here)
  01_imputation/     ← supporting: missing value techniques (Ch. 2.2)
  02_oversampling/   ← supporting: imbalanced dataset handling (Ch. 3.3.1)
  03_monte_carlo/    ← supporting: Monte Carlo simulation (Ch. 6.3.3)
  ch08_cholesky/     ← Part IV Ch. 8: Cholesky decomposition
  ch11_probcon/      ← Part IV Ch. 11: Probabilistic Concatenation (ProbCon)
  ch12_gan/          ← Part IV Ch. 12: GAN anonymisation
  ch13_copula/       ← Part IV Ch. 13: Inverse copula sampling
data/                ← Bulgarian financial/demographic CSVs (~15k rows, ~48 features)
models/              ← pre-trained TF SavedModel (generator.tf, discriminator.tf)
fuzzymatcher/        ← vendored fuzzy matching library
reference/           ← sklearn synthesis function reference
```

Chapters 9, 10, 14, 15 from the monograph are not yet represented by notebooks.

## Git Remote

Push via SSH: `git@github.com:Marchev-Science/synthetic-data.git`. HTTPS auth is unavailable non-interactively.

## Notebook Paths and Data Files

Notebooks under `notebooks/` reference data with `../../data/` relative paths. The GAN notebook (`ch12_gan/gan_synthetic_data.ipynb`) reads `../../data/sim_data_vasko - rab - sim_data.csv` and saves outputs to `../../data/` and `../../models/`.

`notebooks/ch11_probcon/02_data_link.ipynb` requires two external Excel files (`demogr-filtered.xlsx`, `big5_data.xlsx`) not in this repo — see the warning cell at the top of that notebook.

## Algorithm Taxonomy

From the monograph Ch. 6 methodological tree:
- **Random methods**: Monte Carlo (`03_monte_carlo/`), GAN (`ch12_gan/`)
- **Non-random methods**: Cholesky (`ch08_cholesky/`), Inverse Copula (`ch13_copula/`)
- **Dataset linking**: ProbCon (`ch11_probcon/`)
- **Supporting building blocks**: Imputation (`01_imputation/`), Oversampling (`02_oversampling/`)

## fuzzymatcher Library

`fuzzymatcher/` is a vendored copy of fuzzymatcher 0.0.5. Import it by adding the repo root to `sys.path`. Public API: `link_table(...)` and `fuzzy_left_join(...)` — see `fuzzymatcher/__init__.py`.

## Pre-trained Models

Saved in `models/` as TensorFlow SavedModel format (`generator.tf`, `discriminator.tf`). Load with `tf.keras.models.load_model('models/generator.tf')`.
