# Bayesian Media Mix Modeling (MMM) with PyMC-Marketing
### Owners: Nick Pandolfi, William Wang

This repository contains a final project for ADSP 32014: "Bayesian Machine Learning with Generative AI Applications" at the University of Chicago. 

It demonstrates the application of Bayesian inference to the problem of Media Mix Modeling (MMM), using the pymc-marketing library to estimate the effectiveness and saturation of various advertising channels.

### Project Overview
The goal of this analysis is to quantify the contribution of different marketing spends to total sales while accounting for adstock effects (delayed impact) and saturation (diminishing returns). By using a Bayesian framework, we can incorporate prior knowledge and provide a probabilistic range for our Return on Ad Spend (ROAS) estimates.

### Key Components of the Analysis
* Model Specification: We utilized a Bayesian MMM with a Geometric Adstock transformation to capture the "memory" of advertising and a Hill Function to model non-linear saturation effects.
* Trace Diagnostics: Our model showed strong convergence across all parameters (e.g., adstock_alpha, saturation_beta, gamma_fourier), with stable "fuzzy caterpillar" traces indicating reliable posterior sampling.
* Response Decomposition: The analysis revealed that nonbranded search spend is our primary driver, contributing approximately 24.9% of total attributed sales, followed by branded search and Facebook.
* ROAS Efficiency: We calculated that nonbranded search is our most efficient channel with a ROAS of $20.18, while branded search ($3.17) and radio ($5.34) represent our least efficient investments.
* Sensitivity Analysis: We performed a parameter sweep to visualize how incremental changes in spend across different channels would impact total contribution, identifying nonbranded search as the lever with the highest marginal return.

### Channel Performance Summary

| Channel | Total Contribution (%) | ROAS | Marginal Priority |
| :--- | :--- | :--- | :--- |
| **Nonbranded Search** | 24.9% | $20.18 | **High** |
| **OOH** | 8.0% | $13.85 | Medium |
| **Print** | 14.2% | $12.30 | Medium |
| **Facebook** | 19.7% | $6.34 | Maintenance |
| **TV** | 4.8% | $5.58 | Low |
| **Radio** | 8.6% | $5.34 | Low |
| **Branded Search** | 19.8% | $3.17 | Efficiency Play |

### Priors and Model Assumptions
This project employs a Bayesian framework to estimate marketing effectiveness, enabling the quantification of uncertainty and the use of informative priors to guide the model toward realistic business outcomes.

Model Architecture: We define our target variable as a sum of baseline, seasonal, and marketing components:

$$
y_{t} = \text{intercept} + \sum \text{seasonality}_{t} + \sum \text{channel contribution}_{t} + \epsilon_{t}
$$

* Baseline (Intercept): The model assumes a baseline level of sales independent of marketing activity. Our posterior estimates place this "organic" contribution at roughly 30–35%.
* Seasonality: We utilize Fourier terms (gamma_fourier) to capture cyclical patterns, such as weekly or monthly fluctuations.
* Likelihood: The model assumes a normal likelihood for the noise term $\epsilon_t$, with the standard deviation y_sigma estimated at ~0.13.2.

Marketing Transformations: To account for the non-linear nature of advertising, we apply two primary transformations:
* Geometric Adstock: This models the "carryover" effect of advertising over time. The adstock_alpha parameter represents the decay rate; our model converged on a low alpha (~0.05), indicating that marketing impact in this dataset is felt primarily in the immediate period with minimal long-term memory.
* Hill Saturation Function: This captures diminishing returns as spend increases.
* $\lambda$ (Lambda): Controls the shape of the S-curve. Our estimate of ~4.0 suggests a threshold effect where a certain level of spend is required before seeing significant marginal returns.
* $\beta$ (Beta): Represents the maximum potential contribution of the channel, estimated at ~0.12 for the primary saturation curve.

Prior Selections:
* We utilize weakly informative priors to allow the data to drive the final estimates while ensuring parameters remain within physically plausible bounds (e.g., ensuring marketing contributions are non-negative). The tight overlap and stationarity in our MCMC trace plots confirm that the posterior distributions have successfully converged and are well-informed by the observed data.

### Future Work & Scaling
While this model provides a robust baseline, future iterations could improve accuracy and actionability through:

* **Increased Data Granularity**: Transitioning from weekly to daily data to better capture short-term decay rates (Adstock) and weekend/holiday spikes.
* **Causal Inference Integration**: Using randomized controlled trials (incrementality tests) to inform our Bayesian priors, moving from correlation-based attribution to true causal impact.
* **External Regressors**: Incorporating macroeconomic indicators (e.g., Consumer Price Index), competitor spend data to account for external market shifts, or Brand Health data to account for long-term brand building effects.
* **Hierarchical Modeling**: If expanding to multiple regions, implementing a hierarchical Bayesian structure to share information across geographies while allowing for local variance.

### Libraries
* Python 3.11+
* PyMC & PyMC-Marketing: For Bayesian modeling and MCMC sampling.
* ArviZ: For exploratory analysis of Bayesian models and trace visualizations.
* Pandas/Matplotlib: For data manipulation and custom waterfall/sensitivity plots.
