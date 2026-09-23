# FRISA Material Waste Reduction
### Interpretable Machine Learning for Forged-Ring Manufacturing

Academic data science project developed at **Tecnológico de Monterrey** to analyze material waste in FRISA's forged-ring manufacturing process and identify opportunities to reduce unnecessary machining allowance while preserving operational safety.

## Overview

In forged-ring manufacturing, additional material is intentionally added to each part as a safety margin for later machining and process variability. However, excessive allowance increases raw-material consumption and production cost.

This project studies historical production data to identify systematic excess material and proposes interpretable machine-learning models that can support more informed allowance decisions.

The project focuses on two main questions:

1. **Which parts show enough excess material to be considered candidates for a conservative reduction?**
2. **Can systematic differences between configured and observed outer-diameter allowance be identified and used to recalibrate the current process?**

Because the recommendations may affect a real manufacturing process, **interpretability and operational risk were prioritized over maximizing predictive performance alone**.

## Methodology

The analysis was divided into two complementary modeling approaches.

### 1. Decision Tree Classification

A binary classification model was developed to identify parts whose observed volumetric excess is sufficiently larger than the configured allowance.

A ratio between **real volumetric excess** and **configured volumetric excess** was created, and a threshold of **1.10** was selected as a conservative boundary for identifying potentially excessive parts.

Feature selection was performed using **Recursive Feature Elimination (RFE)**. The selected predictors were:

- Final inner diameter
- Configured inner-diameter allowance
- Configured forging height
- Configured forging weight

A decision tree was chosen as the primary classifier because its rules can be directly interpreted by engineers and operators.

#### 5-fold cross-validation performance

| Metric | Score |
|---|---:|
| Accuracy | 0.765 |
| Precision | 0.929 |
| Recall | 0.747 |
| F1-score | 0.828 |
| AUC | 0.853 |

Precision was treated as the most important classification metric because a false positive could lead to reducing material on a part that does not actually have sufficient excess.

Alternative models, including a voting classifier and XGBoost, were also evaluated. Although XGBoost obtained slightly stronger predictive metrics, the decision tree was retained because interpretability was a central requirement of the project.

### 2. Multiple Linear Regression

A second model was developed to study the **outer-diameter gap**, defined as:

```text
DE gap = average observed DE allowance - configured DE allowance
```

The objective of this model was not to predict every individual part precisely, but to identify systematic differences across geometric families, rolling machines, and part sizes.

The model included:

- Geometric family
- Rolling machine
- Centered log-transformed configured forging weight
- Configured outer-diameter allowance
- Interaction between rolling machine R3 and part weight

The regression used **HC3 robust standard errors** to account for heteroscedasticity.

#### Main regression metrics

| Metric | Result |
|---|---:|
| Test R² | 0.073 |
| Adjusted R² | 0.087 |
| Test RMSE | 3.58 mm |
| Test MAE | 2.78 mm |
| Global F-statistic | 41.27 |
| 5-fold CV R² | 0.083 ± 0.008 |
| 5-fold CV RMSE | 3.67 ± 0.02 mm |

The relatively low R² indicates that most part-to-part variability is explained by factors not available in the dataset. For this reason, the regression was used mainly as an **interpretable inferential model**, rather than as a high-accuracy individual predictor.

## Key Findings

The exploratory analysis and modeling produced several relevant findings:

- Excess material was systematic across multiple geometric families.
- Outer-diameter allowance showed the clearest opportunity for recalibration.
- The configured outer-diameter allowance was not statistically significant in the linear model, suggesting that the final gap is not being corrected proportionally by the current configuration.
- Several geometric families showed significantly larger average gaps than the reference family.
- Rolling machine R3 showed a positive association with the outer-diameter gap, although this result should not be interpreted as causal because R3 also processes larger parts.
- Data-quality issues were identified in the relationship between quality labels and recorded geometric defects.

## Estimated Material-Saving Opportunity

Two conservative intervention strategies were evaluated.

### Decision-tree scenario

The classification approach considered a **10% reduction in configured volumetric excess** for parts identified by the model as candidates.

Under the assumptions used in the report, the model identified close to **4,400 candidate parts** and an estimated material reduction of more than **16 m³** over the analyzed period.

This estimate depends on assumptions about material density, process stability, and the relationship between configured and real excess. It should therefore be interpreted as an opportunity estimate rather than a guaranteed saving.

### Regression scenario

For the regression-based approach, a conservative rule of up to **1 mm of diametral reduction** was evaluated for candidate geometric families.

The estimated opportunity under this rule was approximately:

**8,010 kg of steel over six months**

A smaller 0.5 mm intervention and a more aggressive 2 mm intervention were also analyzed as sensitivity scenarios.

## Limitations

This project is based on observational production data and therefore does **not establish causal relationships**.

Important limitations include:

- Only approximately six months of production data were available.
- The dataset did not contain sufficiently detailed temporal information.
- Some quality labels were inconsistent with recorded geometric measurements.
- Important operational variables such as operator, forging temperature, tooling condition, and manual adjustments were unavailable.
- Several geometric families were underrepresented.
- Machine effects may be confounded with the type and size of parts processed.
- A reduction in configured allowance does not necessarily translate proportionally into a reduction in observed allowance.

For these reasons, the proposed reductions should **not be deployed directly in production without engineering validation**.

## Recommended Next Step

The main recommendation is to perform a controlled pilot test.

A safe implementation would:

1. Start with a small set of high-confidence geometric families.
2. Compare a control group using the current configuration with a treatment group using a conservative reduction.
3. Monitor rejection and rework rates.
4. Stop the experiment if quality metrics deteriorate.
5. Track results by material, machine, customer, lot, and other relevant operational variables.
6. Retrain and recalibrate the models as new production data becomes available.

## Project Workflow

```text
Production Data
      |
      v
Data Cleaning & Exploratory Analysis
      |
      +-----------------------------+
      |                             |
      v                             v
Volumetric Excess              DE Gap Analysis
Classification                 Multiple Regression
      |                             |
      v                             v
Decision Tree                 OLS + HC3 Errors
      |                             |
      v                             v
Candidate Parts              Candidate Families
      |                             |
      +-------------+---------------+
                    |
                    v
       Conservative Pilot Strategy
                    |
                    v
       Estimated Material Savings
```

## Repository Structure

> Update this section once the final repository files are organized.

```text
.
├── data/               # Data files, if distribution is permitted
├── notebooks/          # EDA and modeling notebooks
├── figures/            # Generated plots and model visualizations
├── src/                # Reusable preprocessing/modeling code
├── requirements.txt    # Python dependencies
└── README.md
```

## Data Availability

The project was developed using industrial production data provided for an academic challenge.

**Do not publish the original dataset unless you have explicit permission to distribute it.**  
If the data is proprietary, this repository should contain only the code, methodology, synthetic/sample data where appropriate, and non-confidential results.

## Academic Context

**Course:** Análisis de Ciencia de Datos  
**Institution:** Tecnológico de Monterrey  
**Project stage:** Interpretable Modeling and Quantitative Waste Analysis  
**Year:** 2026

### Team

- Hugo Edel Gamboa Sesma
- Diego Villalón Aguilar
- Sebastián Hernández Gómez
- Enrique Alexander Luna Sánchez
- Alejandro Israel Manducano Rojo

## Disclaimer

This repository documents an academic analysis and should not be interpreted as a production-ready engineering control system. Any modification to industrial forging parameters should be validated through controlled experiments and reviewed by qualified process engineers.
