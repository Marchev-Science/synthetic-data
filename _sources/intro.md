# Synthetic Data Generation

**Methods and Code** — a collection of Python implementations covering the main approaches to synthetic tabular data generation.

```{admonition} Dataset
:class: tip
The synthesised dataset produced by these methods is published on Hugging Face:
**DOI [10.57967/hf/3701](https://doi.org/10.57967/hf/3701)**
```

---

## Methods at a Glance

| Available inputs | Method | Notebook |
|---|---|---|
| No data — distributions known | Monte Carlo simulation | [Ch. 3](notebooks/03_monte_carlo/01_dice_distribution) |
| Distributions + correlation matrix | Cholesky decomposition | [Ch. 8](notebooks/ch08_cholesky/cholesky_decomposition) |
| Multivariate joint distribution | Inverse copula sampling | [Ch. 13](notebooks/ch13_copula/cold_modeling_copula) |
| Multiple partial datasets | Probabilistic Concatenation | [Ch. 11](notebooks/ch11_probcon/01_probabilistic_concatenation) |
| Full real dataset (confidential) | GAN | [Ch. 12](notebooks/ch12_gan/gan_synthetic_data) |

**New here?** Start with the [introduction notebook](notebooks/00_overview/introduction) for a hands-on tour with runnable code examples for every method.

---

## Decision Tree

```
What data is available?
│
├─ No data at all
│    └─ Monte Carlo feature-wise simulation
│
├─ Marginal distributions + correlation matrix
│    └─ Cholesky decomposition
│
├─ Multivariate joint distribution (copula)
│    └─ Inverse copula sampling
│
├─ Multiple partial datasets with overlapping keys
│    └─ Probabilistic Concatenation (ProbCon)
│
└─ Full real dataset (confidential / privacy-restricted)
     └─ Generative Adversarial Network (GAN)
```

---

## Quick Start

```bash
pip install -r requirements.txt
jupyter notebook
```

Or click the rocket icon (🚀) on any page to open it directly in **Google Colab**.

---

## Authors

Angel Marchev · Vasil Marchev · Dimitar Lyubchev · Milena Piryankova · Aleksandar Efremov · Daniel Masarliev · Valentin Mitkov

Research project НИД НИ 23/2023/В — University of National and World Economy (UNWE), Sofia.

---

## Cite

> Marchev, A., Marchev, V. (2024). *Modelling Synthetic Information Arrays: Approaches for Synthesis and Validation*. UNWE Publishing House, Sofia.

> Marchev, A., Marchev, V. (2024). *Automated Algorithm for Multi-variate Data Synthesis with Cholesky Decomposition*. ICACS 2023, ACM. DOI: [10.1145/3631908.3631909](https://doi.org/10.1145/3631908.3631909)
