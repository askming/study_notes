# [book] Analysis of Observational Health Care Data Using SAS

<hr>

## Chapter 2-3 Propensity Score Stratification and Regression / Propensity Score Matching for Estimating  Treatment Effects 

### 1. Estimating propensity score (PS)

- Logistic regression
- Probit regression: $P(Y=1|X) = \Phi(X^T\beta)$, where $\Phi(\cdot)$ is the CDF of standard normal distribution
- discriminant analysis
- CART
- Neural nets
- GAM(?)

## 2. Using PS

- Stratification
- Matching: with caliper (0.2 SD of logit of PS [^rot])
- Regression covariate
- IPW (Chapter 5): $\frac{1}{p_i}$ for treatment arm, $\frac{1}{1-p_i}$ for control arm, where $p_i$ is the PS for subject $i$

### 3. Assess the balance after applying PS method

- standardized difference of each covariate
- PS distribution
- distribution of the covariates
- goodness of fit statistics

### 4. Estimating treatment effect

- treatment effect estimate
- S.E. of treatment effect

### 5. Sensitivity analysis for PS

## Chapter 4 Doubly Robust Estimation of Treatment Effects 

### Conceptual overview

- Doubly robust (DR) estimation builds on the propensity score approach of Rosenbaum and Rubin (1983) and the inverse probability of weighting (IPW) approach of Robins and colleagues (Robins, 1998; Robins, 1999a; Robins, 1999b; Robins et al., 2000).
- DR estimation combines inverse probability weighting by a propensity score with regression modeling of the relationship between covariates and outcome for each treatment.
  - It combines it in such a way that, as long as *either* the propensity score model *or* the outcome regression models are correctly specified, the effect of the exposure on the outcome will be correctly estimated, assuming that there are no unmeasured confounders (Robins et al., 1994; Robins, 2000; van der Laan and Robins, 2003; 
    Bang and Robins, 2005).
- Procedure
  - one builds and fits a (binary) regression model for the probability that a particular patient received a given treatment as a function of that individual’s covariates (the propensity score).
  - Maximum likelihood regression is conducted separately within the exposed and unexposed populations to predict the mean response (outcome) as a function of confounders and risk factors.
  - each individual observation is given a weight equal to the inverse of the probability of the treatment he/she received based on baseline covariates (as in IPW analysis) to create two pseudopopulations of subjects that represent the expected response in the entire population under those two treatment conditions.
- one trades a possible loss of precision in using the DR estimator for this additional protection. 

### Statistical model

- The estimator of the causal effect
  
  \begin{align}
  \hat{\Delta}_{DR} \\
   & = n^{-1}\sum_{i=1}^n\left[\frac{Z_iY_i}{e(X_i, \hat{\beta})} - \frac{Z_i-e(X_i, \hat{\beta})}{e(X_i, \hat{\beta})}m_1(X_i, \hat{\alpha}_1)\right] \\
   & - n^{-1}\sum_{i=1}^n\left[\frac{(1-Z_i)Y_i}{1-e(X_i, \hat{\beta})} - \frac{Z_i-e(X_i, \hat{\beta})}{1-e(X_i, \hat{\beta})}m_0(X_i, \hat{\alpha}_0)\right] \\
  & = \hat{\mu_{1, Dr}} - \hat{\mu}_{0, DR}
  \end{align}

- The standard error for the DR estimator is estimated using the delta method

  $$
  SE_{\hat{\Delta}_{DR}} = \sqrt{n^{-2}\sum_{i=1}^n \hat{I}^2_i}, 
  $$
  
  where $\hat{I}_i$ is defined as

  \begin{align}
  \hat{I}_i \\
  & = \left[\frac{Z_iY_i}{e(X_i, \hat{\beta})} - \frac{Z_i-e(X_i, \hat{\beta})}{e(X_i, \hat{\beta})}m_1(X_i, \hat{\alpha}_1)\right]\\
  &  - \left[\frac{(1-Z_i)Y_i}{1-e(X_i, \hat{\beta})} - \frac{Z_i-e(X_i, \hat{\beta})}{1-e(X_i, \hat{\beta})}m_0(X_i, \hat{\alpha}_0)\right] - \hat{\Delta}_{DR}
  \end{align}

  

[^rot]: rule of thumb