---
tags: clinical trial, stats method
---

# Notes on FDA Guidances for Industry

## Adjusting for Covariates in Randomized Clinical Trials for Drugs and Biological Products 

*(May 2021, Revision 1)*

### 1. Introduction

- The main focus of the guidance is on the use of prognostic baseline factors[^prognostic] to improve precision for estimating treatment effects rather than the use of predictive biomarkers to identify groups more likely to benefit from treatment. 
- This guidance does not address use of covariates to control for confounding variables in non-randomized trials or the use of covariate adjustment for analyzing longitudinal repeated measures data. 

### 2. Background

- Incorporating prognostic baseline factors in the primary statistical analysis of clinical trial data can result in a more efficient use of data to demonstrate and quantify the effects of treatment with minimal impact on bias or the Type I error rate.
- The ICH guidance for industry *E9 Statistical Principles for Clinical Trials (September 1998)* addresses these issues briefly. 
- In linear models, adjustment for baseline variables often leads to improved precision by reducing residual variance. 
  - When adjusting for covariates based on fitting **nonlinear regression models**, such as logistic regression models in studies with binary outcomes, there are additional considerations that arise because inclusion of baseline covariates in a regression model can change the treatment effect that is being estimated. 

### 3. Recommendations for covariate adjustment in randomized trials

#### A. General consideration

- Sponsors can adjust for baseline covariates in the analyses of efficacy endpoints in randomized clinical trials.
- Unadjusted analyses are acceptable but adjustment for baseline covariates will generally reduce the variability of estimation of treatment effects and thus lead to narrow CI and more powerful hypothesis testing
- Sponsors should prospectively specify the covariates and the mathematical form of the covariate adjusted estimator in the statistical analysis plan before any unblinding of comparative data.  
- Recommend to adjust for covariates that are prognostic for the outcome of interest in the trial.
- Missing baseline covariate and imputation recommendation: can be singly or multiply imputed, or missingness indicators (Groenwold et al. 2012) can be added to the model for covariate adjustment. Should NOT impute separately by treatment arm.
- FDA recommends that sponsors estimate standard errors using the Huber-White robust “sandwich” estimator (Rosenblum and van der Laan 2009) or the nonparametric bootstrap method (Efron and Tibshirani 1993) rather than using nominal standard errors.
- Randomization is often stratified by baseline covariates. In this case, FDA recommends that the standard error computation account for the stratified randomization (Bugni et al. 2018) with or without strata variables in an adjustment model. (How?)
- Covariate adjustment is acceptable even if baseline covariates are strongly associated with each other (e.g., body weight and body mass index). However, adjusting for less redundant variables generally provides greater efficiency gains. 

#### B. Linear models

- **average treatment effect**, which is the difference in expected outcomes between subjects assigned to treatment and control groups. The average treatment effect is an example of an *unconditional treatment* effect, which quantifies the effect at the population level of moving a target population from untreated to treated. 
- Covariate adjustment through a linear model (without treatment by covariate interactions) also estimates a conditional treatment effect, which is a treatment effect assumed to be *approximately constant* across subgroups defined by baseline covariates in the model. 
  - they happen to coincide in linear models 
- The linear model may include treatment by covariate interaction terms. However, when using this approach, the primary analysis should still be based on an estimate from the model of the average treatment effect. (How?)

#### C. Nonlinear models

- with some parameters such as odds ratios, even when all subgroup treatment effects are identical this subgroup-specific conditional treatment effect can differ from the unconditional treatment effect (i.e., the effect at the population level from moving the target population from untreated to treated) (Gail et al. 1984). 
  - This is termed **non-collapsibility** (Agresti 2002) 
  
  - example
  
    |          | Percentage of target population | Success  |  Rate   | Odds ratio |
    | -------- | :-----------------------------: | :------: | :-----: | :--------: |
    |          |                                 | New drug | Placebo |            |
    | Males    |               50%               |  80.0%   |  33.3%  |    8.0     |
    | Females  |               50%               |  25.0%   |  4.0%   |    8.0     |
    | Combined |              100%               |  52.5%   |  18.7%  |    4.8     |
  
    
  
- In trials with time-to-event outcomes, the hazard ratio is also generally non-collapsible. Unlike the odds ratio or hazard ratio, the risk difference and relative risk are collapsible. 

- Cochran-Mantel-Haenszel methods (Mantel and Haenszel 1959) are acceptable for the analysis of clinical trial data and attempt to estimate a conditional treatment effect 

- In nonlinear regression models (without treatment by covariate interactions) the treatment effect is assumed to be approximately constant across subgroups defined by baseline covariates in the model and can provide more personalized information than the unconditional treatment effect.
  
- The FDA recommends Sponsors to perform covariate adjusted estimation and inference for an unconditional treatment effect in the primary analysis of data from a randomized trail. Recommended steps to implement this:
  
  - Fit a non-linear model with maximum likelihood with prespecified baseline adjustment. The model should include an intercept term
  - For each subject compute the model-based prediction of the outcome (this will depend on the type of outcome) under treatment and control, respectviely
  - Estimate the average treatment effect by comparing the average outcome from above step
  - Use nonparametric bootstrap or standard error formulas for confidence interval construction and hypothesis testing.

[^prognostic]: *prognostic baseline factors* used in this guidance refers to baseline covariates that are likely to be associated with the primary endpoint 


### Other related references
1. [Commentary to the gudiance](https://thestatsgeek.com/2021/06/11/comments-on-fda-guidance-adjusting-for-covariates-in-randomized-clinical-trials-for-drugs-and-biological-products/)
2. Benkeser D, Díaz I, Luedtke A, Segal J, Scharfstein D, Rosenblum M. [Improving precision and power in randomized trials for COVID‐19 treatments using covariate adjustment, for binary, ordinal, and time‐to‐event outcomes.](https://onlinelibrary.wiley.com/doi/full/10.1111/biom.13377) *Biometrics*. 2020 Jan 1.
  - [Commentary](https://onlinelibrary.wiley.com/doi/10.1111/biom.13494?af=R) of above paper by Lisa LaVange


<hr>

## Non-Inferiority Clinical Trials to Establish Effectiveness

### 1. Intro and background

### 2. General considerations for Non-inferiority studies


### 3. Choosing the non-inferiority margin and testing the non-inferiority hypothesis


### 4. FAQ & examples