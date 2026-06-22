# Modelling Synthetic Information Arrays: Approaches for Synthesis and Validation

**Companion code repository** for the collective monograph of the same name, published under research project НИД НИ 23/2023/В, University of National and World Economy (UNWE), Sofia.

> The monograph is written in Bulgarian. All code in this repository is in English.

The synthesised dataset produced by the methods in this repository is published on Hugging Face:  
**DOI: [10.57967/hf/3701](https://doi.org/10.57967/hf/3701)**

---

## Authors

**Editor:** Assoc. Prof. Vasil Marchev, PhD — Department of Management, UNWE, Sofia

| Chapter | Authors |
|---|---|
| 1, 2 — Big Data & Information Provision | Vasil Marchev |
| 3 — Nature of Synthetic Data | Vasil Marchev (50%), Angel Marchev (50%) |
| 4 — Validation of Synthetic Data | Vasil Marchev (50%), Angel Marchev (50%) |
| 5 — Business Logic | Vasil Marchev (50%), Angel Marchev (50%) |
| 6 — Methodological Aspects | Angel Marchev (50%), Vasil Marchev (50%) |
| 7 — Synthesis of Multidimensional Personal Data | Angel Marchev (50%), Vasil Marchev (50%) |
| 8 — Cholesky Decomposition Algorithm | Angel Marchev (50%), Vasil Marchev (50%) |
| 9 — Generation with Partial Datasets | Angel Marchev (33%), Vasil Marchev (33%), Aleksandar Efremov (33%) |
| 10 — Validation for Behavioural Modelling | Angel Marchev (33%), Vasil Marchev (33%), Aleksandar Efremov (33%) |
| 11 — ProbCon | Vasil Marchev (50%), Angel Marchev (50%) |
| 12 — GAN Anonymisation | Angel Marchev (50%), Vasil Marchev (50%) |
| 13 — Copula | Angel Marchev (33%), Dimitar Lyubchev (33%), Vasil Marchev (33%) |
| 14 — Synthesising Anonymised Multidimensional Dataset | Vasil Marchev (20%), Angel Marchev (20%), Milena Piryankova (20%), Daniel Masarliev (20%), Valentin Mitkov (20%) |
| 15 — Methodological Approaches for Multidimensional Personal Data | Vasil Marchev (10%), Angel Marchev (10%), Kaloyan Haralambiev (10%), Aleksandar Efremov (10%), Boyan Markov (10%), Dimitar Lyubchev (10%), Milena Piryankova (10%), Bogomil Filipov (10%), Daniel Masarliev (10%), Valentin Mitkov (10%) |

---

## Monograph Structure

The monograph is organised in four parts:

| Part | Chapters | Content |
|---|---|---|
| **I. Introduction** | — | Research goals, hypotheses, scope, limitations |
| **II. General Guidelines** | 1–5 | Big data, information provision, nature of synthetic data, validation, business logic |
| **III. Methodology** | 6–7 | Methodological tree; overview of all synthesis approaches |
| **IV. Approaches** | 8–15 | Algorithm demonstrations — the chapters covered by this repository |

---

## The Methodological Tree (Chapter 6)

Chapter 6 establishes the decision framework for choosing a synthesis method:

```
What data is available?
│
├─ No data at all
│    └─ Monte Carlo feature-wise simulation + business logic filtering (Ch. 7, 8)
│
├─ Marginal distributions + correlation matrix
│    └─ Cholesky decomposition (Ch. 8)                    → notebooks/ch08_cholesky/
│
├─ Multivariate joint distribution (copula)
│    └─ Inverse copula sampling (Ch. 13)                  → notebooks/ch13_copula/
│
├─ Partial real features + business rules
│    └─ Feature derivation + business logic filtering (Ch. 9)   [see note]
│
├─ Multiple partial datasets with overlapping variables
│    └─ Probabilistic Concatenation / ProbCon (Ch. 11)    → notebooks/ch11_probcon/
│
└─ Full real dataset (confidential / unlicensed)
     └─ Generative Adversarial Network (Ch. 12)           → notebooks/ch12_gan/
```

After generation, all approaches pass through:
- **Horizontal synchronisation** — cross-variable logical consistency
- **Vertical feature validation** — distributional fidelity (KS test, business logic, HECA/LLM-PA/LOFO-AC)

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

These notebooks cover building-block concepts used across multiple chapters:

| Folder | Notebook | Monograph reference |
|---|---|---|
| `notebooks/01_imputation/` | `01_missing_values_by_category.ipynb` | Ch. 2.2 (data interpolation) |
| `notebooks/01_imputation/` | `02_missing_values_noisy_input.ipynb` | Ch. 2.2 (distribution-preserving imputation) |
| `notebooks/02_oversampling/` | `01_random_naive_oversampling.ipynb` | Ch. 3.3.1 (data enrichment) |
| `notebooks/02_oversampling/` | `02_smote.ipynb` | Ch. 3.3.1 (data enrichment) |
| `notebooks/03_monte_carlo/` | `01_dice_distribution.ipynb` | Ch. 6.3.3 (Monte Carlo simulation) |
| `notebooks/03_monte_carlo/` | `02_distribution_fitting.ipynb` | Ch. 6.3.3 / 8.2.1 (distribution fitting) |

### Part IV — Algorithm Demonstrations

#### Chapter 8 — Automated Algorithm for Multivariate Data Synthesis with Cholesky Decomposition
**`notebooks/ch08_cholesky/cholesky_decomposition.ipynb`**

End-to-end notebook covering: 2-variable intro → N-variable generalisation → user-defined correlation matrix → full pipeline with `fitter`-based distribution fitting and KS-test validation.

*Use when: marginal distributions and a correlation matrix are known or can be estimated.*

#### Chapter 11 — Probabilistic Concatenation (ProbCon)
**`notebooks/ch11_probcon/01_probabilistic_concatenation.ipynb`**  
**`notebooks/ch11_probcon/02_data_link.ipynb`** *(requires external Excel files — see warning in notebook)*

Levenshtein-distance-based probabilistic merging of partial datasets from different sources. The ProbCon algorithm normalises keys via binary transformation, computes match probabilities, and resolves ambiguous matches stochastically.

*Use when: multiple partial datasets share overlapping key variables but values are not identical across sources.*

#### Chapter 12 — GAN Anonymisation
**`notebooks/ch12_gan/gan_synthetic_data.ipynb`**

Trains a generator/discriminator pair on the real Bulgarian financial/demographic survey dataset. Pre-trained models are stored in `models/`. Validation uses cosine similarity of mean vectors and k-means cluster analysis.

*Use when: a full real dataset is available but confidential or privacy-restricted.*

#### Chapter 13 — Joint Distribution Synthesis (Copula)
**`notebooks/ch13_copula/cold_modeling_copula.ipynb`**

Gaussian copula generating 50 000 synthetic records with 40+ categorical and 8 numerical features. Categorical variables sampled from empirical frequency distributions; numerical variables generated via Cholesky-decomposed correlation matrix with feature bounds. KS-test validated.

*Use when: a multivariate joint distribution (copula) is available.*

### Chapters Not Yet in Code

The following Part IV chapters from the monograph are not yet represented by notebooks in this repository:

| Chapter | Title | Status |
|---|---|---|
| 9 | Generation with Partial Information Sets | Planned |
| 10 | Validation for Behavioural Modelling (HECA, LLM-PA, LOFO-AC) | Planned |
| 14 | Synthesising the Anonymised Multidimensional Dataset (case study) | Planned |
| 15 | Methodological Approaches for Multidimensional Personal Data | Planned |

### Reference
**`reference/sklearn_synthesis_functions.ipynb`** — Quick-reference for scikit-learn's built-in dataset generators.

---

## Data

The `data/` directory contains the Bulgarian financial and demographic survey dataset (~15 000 records, ~48 features) used in Chapters 12 and 13:

| File | Description |
|---|---|
| `sim_data_vasko - rab - sim_data.csv` | Primary input for GAN training (Ch. 12) |
| `sim_data_vasko - sim_data.csv` | Variant input dataset |
| `generated_data.csv` | GAN-generated synthetic output (Ch. 12) |
| `marchev-synth-data.csv` | Copula-generated synthetic output (Ch. 13) |

Variable groups (Ch. 2.2.2): demographic, socioeconomic, psychological/personality, individual risk propensity, financial and banking.

The full synthesised dataset (50 000 records) is available on Hugging Face: [DOI 10.57967/hf/3701](https://doi.org/10.57967/hf/3701).

---

## Pre-trained Models

The `models/` directory contains TensorFlow SavedModel checkpoints for the GAN (Chapter 12):

```python
import tensorflow as tf
generator     = tf.keras.models.load_model('models/generator.tf')
discriminator = tf.keras.models.load_model('models/discriminator.tf')
```

---

## Publications

1. Marchev, A., Marchev, V. (2024). *Automated Algorithm for Multi-variate Data Synthesis with Cholesky Decomposition*. ICACS 2023, ACM. DOI: [10.1145/3631908.3631909](https://doi.org/10.1145/3631908.3631909)
2. Marchev, V., Marchev, A., Piryankova, M., Masarliev, D., Mitkov, V. (2023). *Synthesizing an anonymized multidimensional dataset featuring financial, economic, demographic, and personal traits data*. VSIM, vol. 19, no. 1, ISSN 1314-0582
3. Marchev, A., Marchev, V. (2022). *Synthesizing multi-dimensional personal data sets*. AIP Conference Proceedings, 2505(1): 020012. DOI: [10.1063/5.0100615](https://doi.org/10.1063/5.0100615)
4. Marchev, V., Marchev, A. (2021). *Methods for Simulating Multi-dimensional Data for Financial Services Recommendation*. Bulgarian Economic Papers, BEP 02-2021, ISSN: 2367-7082
5. Марчев В., Марчев А. (2020). *Симулация на многокритериална база от данни за банкови услуги*. НИТ и Големи данни
6. Marchev, V., Marchev, A. (2024). *Anonymizing Personal Information Using Distribution-based Data Synthesis* (in press)
7. Lyubchev, D., Marchev, A., Marchev, V. (2024). *Inverse Copula Sampling for Multi-dimensional Data Synthesis*
8. Marchev, V., Marchev, A. (2025). *Methodological Considerations for Anonymizing Tabular Data Using Generative Adversarial Networks*

---

## Related

- Case study companion: [angel-marchev/case-cold-start-modeling](https://github.com/angel-marchev/case-cold-start-modeling)
- Funding: Research project НИД НИ 23/2023/В, UNWE, Sofia

---

## License

MIT — see [LICENSE](LICENSE).
