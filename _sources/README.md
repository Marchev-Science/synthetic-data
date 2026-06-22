# Synthetic Data Generation — Methods and Code

Python implementations of the main approaches to synthetic tabular data generation: statistical methods (Monte Carlo, Cholesky decomposition, Gaussian copula), deep learning (GAN), and probabilistic dataset merging (ProbCon).

The accompanying synthesised dataset is published on Hugging Face:  
**DOI: [10.57967/hf/3701](https://doi.org/10.57967/hf/3701)**

Research project НИД НИ 23/2023/В — University of National and World Economy (UNWE), Sofia.

**Online book:** [marchev-science.github.io/synthetic-data](https://marchev-science.github.io/synthetic-data/) — browse the notebooks as a readable website with sidebar navigation and one-click Google Colab launch.

---

## Authors

| Topic | Authors |
|---|---|
| Monte Carlo simulation | Vasil Marchev |
| Cholesky decomposition | Angel Marchev, Vasil Marchev |
| Probabilistic Concatenation (ProbCon) | Vasil Marchev, Angel Marchev |
| GAN anonymisation | Angel Marchev, Vasil Marchev |
| Inverse copula sampling | Angel Marchev, Dimitar Lyubchev, Vasil Marchev |
| Validation methods | Angel Marchev, Vasil Marchev, Aleksandar Efremov |
| Full synthesis case study | Vasil Marchev, Angel Marchev, Milena Piryankova, Daniel Masarliev, Valentin Mitkov |

---

## Method Decision Guide

Choose a synthesis method based on what input data is available:

```
What data is available?
│
├─ No data at all
│    └─ Monte Carlo feature-wise simulation + business logic filtering
│
├─ Marginal distributions + correlation matrix known
│    └─ Cholesky decomposition                    → notebooks/ch08_cholesky/
│
├─ Multivariate joint distribution (copula) available
│    └─ Inverse copula sampling                   → notebooks/ch13_copula/
│
├─ Partial real features + business rules
│    └─ Feature derivation + filtering                        (planned)
│
├─ Multiple partial datasets with overlapping keys
│    └─ Probabilistic Concatenation / ProbCon     → notebooks/ch11_probcon/
│
└─ Full real dataset (confidential / privacy-restricted)
     └─ Generative Adversarial Network (GAN)      → notebooks/ch12_gan/
```

After generation, all methods pass through:
- **Horizontal synchronisation** — cross-variable logical consistency checks
- **Vertical feature validation** — distributional fidelity (KS test, business logic, HECA/LLM-PA/LOFO-AC)

| Available inputs | Recommended method |
|---|---|
| Only marginal distributions | Monte Carlo simulation |
| Distributions + correlation matrix | Cholesky decomposition |
| Multivariate joint distribution | Inverse copula sampling |
| Partial real dataset + business rules | Feature engineering + filtering |
| Multiple partial datasets with overlap | ProbCon (fuzzy matching) |
| Full real dataset (confidential) | GAN |

---

## Quick Start

```bash
pip install -r requirements.txt
jupyter notebook
```

---

## Notebooks

### Overview

| Folder | Notebook | Description |
|---|---|---|
| `notebooks/00_overview/` | `introduction.ipynb` | **Start here.** Decision guide + runnable code examples for every method |

### Supporting Techniques

| Folder | Notebook | Technique |
|---|---|---|
| `notebooks/01_imputation/` | `01_missing_values_by_category.ipynb` | Mean imputation within groups |
| `notebooks/01_imputation/` | `02_missing_values_noisy_input.ipynb` | Distribution-preserving imputation |
| `notebooks/02_oversampling/` | `01_random_naive_oversampling.ipynb` | Random oversampling for class imbalance |
| `notebooks/02_oversampling/` | `02_smote.ipynb` | SMOTE synthetic minority oversampling |
| `notebooks/03_monte_carlo/` | `01_dice_distribution.ipynb` | Monte Carlo probability estimation |
| `notebooks/03_monte_carlo/` | `02_distribution_fitting.ipynb` | Fitting marginal distributions with `fitter` |

### Algorithm Implementations

#### Cholesky Decomposition — `notebooks/ch08_cholesky/`
**`cholesky_decomposition.ipynb`**

End-to-end pipeline: 2-variable intro → N-variable generalisation → user-defined correlation matrix → full synthesis with `fitter`-based distribution fitting and KS-test validation.

*Use when: marginal distributions and a correlation matrix are known or can be estimated.*

#### Probabilistic Concatenation (ProbCon) — `notebooks/ch11_probcon/`
**`01_probabilistic_concatenation.ipynb`**  
**`02_data_link.ipynb`** *(requires external Excel files — see warning cell in notebook)*

Levenshtein-distance + TF-IDF probabilistic merging of partial datasets from different sources. Resolves ambiguous key matches stochastically.

*Use when: multiple partial datasets share overlapping key variables but values differ across sources.*

#### GAN Anonymisation — `notebooks/ch12_gan/`
**`gan_synthetic_data.ipynb`**

Trains a generator/discriminator pair on the Bulgarian financial/demographic survey dataset. Pre-trained models stored in `models/`. Validation uses cosine similarity of mean vectors and k-means cluster analysis.

*Use when: a full real dataset is available but confidential or privacy-restricted.*

#### Inverse Copula Sampling — `notebooks/ch13_copula/`
**`cold_modeling_copula.ipynb`**

Gaussian copula generating 50 000 synthetic records with 40+ categorical and 8 numerical features. KS-test validated.

*Use when: the multivariate joint distribution is available.*

### Not Yet Implemented

| Method | Status |
|---|---|
| Generation with partial information sets | Planned |
| Validation for behavioural modelling (HECA, LLM-PA, LOFO-AC) | Planned |
| Full synthesis case study | Planned |
| Methodological survey | Planned |

### Reference
**`reference/sklearn_synthesis_functions.ipynb`** — Quick-reference for scikit-learn's built-in dataset generators.

---

## Data

The `data/` directory contains the Bulgarian financial and demographic survey dataset (~15 000 records, ~48 features):

| File | Description |
|---|---|
| `sim_data_vasko - rab - sim_data.csv` | Primary input for GAN training |
| `sim_data_vasko - sim_data.csv` | Variant input dataset |
| `generated_data.csv` | GAN-generated synthetic output |
| `marchev-synth-data.csv` | Copula-generated synthetic output |

Variable groups: demographic, socioeconomic, psychological/personality, individual risk propensity, financial and banking.

The full synthesised dataset (50 000 records) is available on Hugging Face: [DOI 10.57967/hf/3701](https://doi.org/10.57967/hf/3701).

---

## Pre-trained Models

The `models/` directory contains TensorFlow SavedModel checkpoints for the GAN:

```python
import tensorflow as tf
generator     = tf.keras.models.load_model('models/generator.tf')
discriminator = tf.keras.models.load_model('models/discriminator.tf')
```

---

## Publications

1. Marchev, A., Marchev, V. (2024). *Modelling Synthetic Information Arrays: Approaches for Synthesis and Validation*. UNWE Publishing House, Sofia. ISBN (forthcoming). — **monograph; this repository is the companion code**
2. Marchev, A., Marchev, V. (2024). *Automated Algorithm for Multi-variate Data Synthesis with Cholesky Decomposition*. ICACS 2023, ACM. DOI: [10.1145/3631908.3631909](https://doi.org/10.1145/3631908.3631909)
3. Marchev, V., Marchev, A., Piryankova, M., Masarliev, D., Mitkov, V. (2023). *Synthesizing an anonymized multidimensional dataset featuring financial, economic, demographic, and personal traits data*. VSIM, vol. 19, no. 1, ISSN 1314-0582
4. Marchev, A., Marchev, V. (2022). *Synthesizing multi-dimensional personal data sets*. AIP Conference Proceedings, 2505(1): 020012. DOI: [10.1063/5.0100615](https://doi.org/10.1063/5.0100615)
5. Marchev, V., Marchev, A. (2021). *Methods for Simulating Multi-dimensional Data for Financial Services Recommendation*. Bulgarian Economic Papers, BEP 02-2021, ISSN: 2367-7082
6. Марчев В., Марчев А. (2020). *Симулация на многокритериална база от данни за банкови услуги*. НИТ и Големи данни
7. Marchev, V., Marchev, A. (2024). *Anonymizing Personal Information Using Distribution-based Data Synthesis* (in press)
8. Lyubchev, D., Marchev, A., Marchev, V. (2024). *Inverse Copula Sampling for Multi-dimensional Data Synthesis*
9. Marchev, V., Marchev, A. (2025). *Methodological Considerations for Anonymizing Tabular Data Using Generative Adversarial Networks*

---

## Related

- Case study companion: [angel-marchev/case-cold-start-modeling](https://github.com/angel-marchev/case-cold-start-modeling)

---

## License

MIT — see [LICENSE](LICENSE).
