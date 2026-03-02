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

### Libraries
* Python 3.11+
* PyMC & PyMC-Marketing: For Bayesian modeling and MCMC sampling.
* ArviZ: For exploratory analysis of Bayesian models and trace visualizations.
* Pandas/Matplotlib: For data manipulation and custom waterfall/sensitivity plots.
