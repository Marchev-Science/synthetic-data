# Synthetic Data Generation — Demo Code

Companion repository for the monograph on synthetic data generation by **prof. Angel Marchev Jr., PhD** (University of National and World Economy, Sofia).

The notebooks demonstrate a full spectrum of techniques for creating structured tabular data — from simple oversampling and imputation to Cholesky decomposition, copula modelling, and deep generative models.

---

## Why Synthetic Data?

| Problem | Synthetic data solution |
|---|---|
| Imbalanced dataset | Oversampling (naive or SMOTE) |
| Missing values | Imputation |
| Privacy / data regulations | Full synthetic dataset |
| No real data available | Simulation from distributions |
| Cost reduction | Replace expensive data collection |
| Feature engineering | Non-linear transforms on simulated data |

---

## Algorithm Taxonomy

The techniques in this repository fall into two fundamental categories, plus supporting methods:

### Random Methods
| Method | When to use |
|---|---|
| **Monte Carlo simulation** | Marginal distributions are known or can be estimated from a small sample |
| **Generative Adversarial Network (GAN)** | A full real dataset is available but confidential or restricted |

### Non-Random (Deterministic) Methods
| Method | When to use |
|---|---|
| **Cholesky decomposition** | Marginal distributions *and* a correlation matrix are available |
| **Inverse copula sampling** | A multivariate joint distribution (copula) is available |

### Supporting Techniques
| Technique | Purpose |
|---|---|
| **Data imputation** | Fill missing values while preserving distributional properties |
| **Oversampling (Naive / SMOTE)** | Address class imbalance in classification datasets |
| **Fuzzy matching** | Stitch together partial datasets from different sources |

---

## Decision Guide

Choose your method based on what information is available:

```
Only marginal distributions known?
  → notebooks/03_monte_carlo/

Marginal distributions + correlation matrix?
  → notebooks/04_cholesky/

Multivariate joint distribution (copula) available?
  → notebooks/05_copula/

Full real dataset (confidential)?
  → notebooks/06_gan/

Multiple partial datasets with overlapping variables?
  → notebooks/07_fuzzy_matching/

Imbalanced classes?
  → notebooks/02_oversampling/

Missing values?
  → notebooks/01_imputation/
```

---

## Quick Start

```bash
pip install -r requirements.txt
jupyter notebook
```

Open any notebook from the `notebooks/` directory. All notebooks are self-contained except:
- `notebooks/06_gan/gan_synthetic_data.ipynb` — reads from `data/sim_data_vasko - rab - sim_data.csv`
- `notebooks/07_fuzzy_matching/02_data_link.ipynb` — requires external Excel files (see note inside notebook)

---

## Notebooks

### Chapter 1 — Data Imputation (`notebooks/01_imputation/`)
| Notebook | Description |
|---|---|
| `01_missing_values_by_category.ipynb` | Impute missing values using the mean within categorical subgroups instead of the global mean |
| `02_missing_values_noisy_input.ipynb` | Impute using distribution-preserving noise sampling to avoid distribution distortion |

### Chapter 2 — Oversampling (`notebooks/02_oversampling/`)
| Notebook | Description |
|---|---|
| `01_random_naive_oversampling.ipynb` | Randomly duplicate minority-class records to balance a dataset |
| `02_smote.ipynb` | SMOTE and its variants — interpolation-based synthetic minority oversampling |

### Chapter 3 — Monte Carlo Simulation (`notebooks/03_monte_carlo/`)
| Notebook | Description |
|---|---|
| `01_dice_distribution.ipynb` | Probability distribution of dice outcomes — introduction to Monte Carlo thinking |
| `02_distribution_fitting.ipynb` | Fit the best statistical distribution to observed data using the `fitter` library |

### Chapter 4 — Cholesky Decomposition (`notebooks/04_cholesky/`)
| Notebook | Description |
|---|---|
| `cholesky_decomposition.ipynb` | End-to-end: from a 2-variable intro through N-variable generalisation to a full pipeline with distribution fitting and KS-test validation |

### Chapter 5 — Copula Modelling (`notebooks/05_copula/`)
| Notebook | Description |
|---|---|
| `cold_modeling_copula.ipynb` | Gaussian copula with 40+ categorical and 8 numerical features; generates 50 000 synthetic records with KS-test validation |

### Chapter 6 — Generative Adversarial Networks (`notebooks/06_gan/`)
| Notebook | Description |
|---|---|
| `gan_synthetic_data.ipynb` | Train a GAN on real tabular financial/demographic data; pre-trained generator and discriminator saved in `models/` |

### Chapter 7 — Fuzzy Matching & Dataset Linking (`notebooks/07_fuzzy_matching/`)
| Notebook | Description |
|---|---|
| `01_probabilistic_concatenation.ipynb` | Levenshtein-distance-based probabilistic merging of two partial datasets |
| `02_data_link.ipynb` | Fuzzy join using the `fuzzymatcher` library (requires external Excel files — see note) |

### Reference
| Notebook | Description |
|---|---|
| `reference/sklearn_synthesis_functions.ipynb` | Quick-reference for scikit-learn's built-in dataset generators (`make_classification`, `make_blobs`, etc.) |

---

## Data

The `data/` directory contains a Bulgarian demographic and financial survey dataset (~15 000 records, ~48 features) used across the GAN and copula notebooks:

| File | Description |
|---|---|
| `sim_data_vasko - rab - sim_data.csv` | Primary input for GAN training (financial services survey data) |
| `sim_data_vasko - sim_data.csv` | Variant input dataset |
| `generated_data.csv` | GAN-generated synthetic output |
| `marchev-synth-data.csv` | Copula-generated synthetic output |

Features include: sex, age, education level, employment status, marital status, household composition, nationality, religion, socioeconomic status, profession, income, expenses, investment portfolio, banking products, and insurance coverage.

---

## Pre-trained Models

The `models/` directory contains TensorFlow SavedModel checkpoints for the GAN trained in Chapter 6:

```python
import tensorflow as tf
generator = tf.keras.models.load_model('models/generator.tf')
discriminator = tf.keras.models.load_model('models/discriminator.tf')
```

---

## Publications

1. Marchev, A., Marchev, V., 2024, *Automated Algorithm for Multi-variate Data Synthesis with Cholesky Decomposition*, ICACS 2023, ACM, DOI: [10.1145/3631908.3631909](https://doi.org/10.1145/3631908.3631909)
2. Marchev, V., Marchev, A., Piryankova, M., Masarliev, D., Mitkov, V., 2023, *Synthesizing an anonymized multidimensional dataset featuring financial, economic, demographic, and personal traits data*, VSIM, vol. 19, no. 1, ISSN 1314-0582
3. Marchev, A., Marchev, V., 2022, *Synthesizing multi-dimensional personal data sets*, AIP Conference Proceedings, 2505(1): 020012, DOI: [10.1063/5.0100615](https://doi.org/10.1063/5.0100615)
4. Marchev, V., Marchev, A., 2021, *Methods for Simulating Multi-dimensional Data for Financial Services Recommendation*, Bulgarian Economic Papers, BEP 02-2021, ISSN: 2367-7082
5. Марчев В., Марчев А., 2020, *Симулация на многокритериална база от данни за банкови услуги*, НИТ и Големи данни
6. Marchev, V., Marchev, A., 2024, *Anonymizing Personal Information Using Distribution-based Data Synthesis* (in press)
7. Lyubchev, D., Marchev, A., Marchev, V., 2024, *Inverse Copula Sampling for Multi-dimensional Data Synthesis*
8. Marchev, V., Marchev, A., 2025, *Methodological Considerations for Anonymizing Tabular Data Using Generative Adversarial Networks*

---

## Related

- Case study companion repo: [angel-marchev/case-cold-start-modeling](https://github.com/angel-marchev/case-cold-start-modeling)

---

## License

MIT — see [LICENSE](LICENSE).
