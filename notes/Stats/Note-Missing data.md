# Missing data

## Missing data mechanism 

1. MCAR: missingness is independent of observed & unobserved data
2. MAR: (prob. of) missing only depends on observed information and is independent on unobserved info
   -  **Distinctness**: the parameters $\theta$ of the data model and the parameters $\phi$ of the missing data indicators are distinct. That is, knowing the values of $\theta$ does not provide any additional information about $\phi$, and vice versa.
   -  If both *MAR* and *distinctness* assumptions are satisfied, the missing-data mechanism is said to be **ignorable**. {cite:p}`yuan2010multiple}` (this is similar/same assumption as ignorability in the causal framework)
3. Missing that depends on unobserved predictors
4. Missing that depends on the missing value itself, e.g. "censoring"

## 2. Handling missing data

2.1 Discard the data with missing or complete-case analysis
   - Can potentially leading to hiased results if missing cases is different from complete case
   - Many data can potentially discarded => much smaller sample size
  
2.2 Available-case analysis
   - use different subsets (complete data) for different analyses
   - => non-consistency b/t subsets systematic bias
   - related: complete-variables analysis

2.3 Non-response weighting: e.g. IPW

2.4 Single imputation for missing data
   - single imputation: s.e. tend to be too low
     - by choosing a single imputation, we in essence pretend that we know the true value with certainty but in reality we have substaintial uncertainty about the missing value
     - Mean imputaion: may distort the distr. of that var.; underestimate of the standard deviation; distorts relationship b/t variables by "pulling" estimates of the corrleation -> o
     - LOCF: conservative; but can also be anti-conservative
     - Using information from related obs.
     - Indicator var. for missingness of cat. predictors: i.e. adding extra cat. for "missing"
     - Indicator var. for continuous predictor
       - creating an extra indicator variable
       - replacing missing with, e.g. 0 or by the mean
       - adding interaction b/t an indicator for response and these predictors can help alternative this bias
     - Random imputation of single variable
        - simple random imputation
        - zero coding & topcoding
          - e.g. impute income that is positive only 
          - topcoding: set up-limit to the value, censor any value beyond the up-limit $\rightarrow$ not to correct the data, but rather perform simple transformation to improve the predictive power of the regression model
        - Using regression predictions to perform deterministric imputation

### 2.5 Multiple imputation {cite:p}`yuan2010multiple}`

#### Why multiple imputation
- Single imputation does not reflect the uncertainty about the predictions of the unknown missing values, and the resulting estimated variances of the parameter estimates will be biased toward zero.
- Multiple imputation does not attempt to estimate each missing value through simulated values but rather to represent a random sample of the missing values.

#### Imputation mechanisms
```{margin}
A data set is said to have a monotone missing pattern when the event that a variable $Y_j$ is missing for the individual $i$ implies that all subsequent variables $Y_k, k>j$, are all missing for the individual $i$.
```
- For *monotone missing* data patterns, either a parametric regression method that assumes multivariate normality or a nonparametric method that uses propensity scores is appropriate.
  - When you have a monotone missing data pattern, you have greater flexibility in your choice of strategies.
- For an *arbitrary missing* data pattern, a Markov chain Monte Carlo (MCMC) method (Schafer 1997) that assumes multivariate normality can be used.
  - The MCMC method, which creates multiple imputations by using simulations from a Bayesian prediction distribution for normal data.

- Another way to handle a data set with an arbitrary missing data pattern is to use the MCMC approach to impute enough values to make the missing data pattern monotone. Then, you can use a more flexible imputation method.

##### Regression method
- In the regression method, a regression model is fitted for each variable with missing values. Based on the resulting model, a new regression model is then drawn and is used to impute the missing values for the variable

- High-level description of the procedure
  - A regression model is fit for the variable with missing data as the outcome (e.g. $Y_j$ below) with complete data:

   $$
   Y_j = \beta_0 + \beta_1X_1 + \beta_2X_2 + \cdots + \beta_kX_k
   $$

- Once the model is fit, generate regression parameters from the posterior predictive distribution:
  - The variance is drawn as 
  
      $$
      \sigma^2_{*j} = \hat{\sigma}_j^2(n_j-k-1)/g
      $$

      where $g$ is a $\chi^2_{n_j-k-1}$ random variable and $n_j$ is the number of non-missing observations for $Y_j$
   - The regression coefficients are drawn as
  
      $$
      \beta_* = \hat{\beta} + \sigma_{*j}\bf{V}_{hj}'\bf{Z}
      $$

      where $\bf{V}'_{hj}$ is the upper triangular matrix of the Cholesky decomposition, i.e. $\bf{V}_{j} = \bf{V}'_{hj}\bf{V}_{hj}$; and $\bf{Z}$ is a vector of $k+1$ independent random normal varirates(?).

   - The missing values are then replaced by
      
      $$
      \beta_{*0} + \beta_{*1}x_1 + \beta_{*2}x_2 + \cdots + \beta_{*k}x_k + z_i\sigma_{*j}
      $$

      where $x_1, x_2, \cdots, x_k$ are the covariates of the observation with missing value and $z_i$ is a simulated normal deviate (i.e. random error).

- Missing data imputed from predictive mean matching (pmm) method
  - for each missing value, it imputes an observed value which is closest to the predicted value from the simulated regression model
  - The predictive mean matching method ensures that imputed values are plausible and may be more appropriate than the regression method if the normality assumption is violated (Horton and Lipsitz 2001, p. 246).

##### Propensity score method
- In the propensity score method, a propensity score is generated for each variable with missing values to indicate the probability of that observation being missing.
- The observations are then grouped based on these propensity scores, and an approximate Bayesian bootstrap imputation (Rubin 1987, p. 124) is applied to each group (Lavori, Dawson, and Shera 1995).
- High-level description of the procedure
  - Create an indicator variable $R_j$ with the value 0 for observations with missing $Y_j$ and 1 otherwise
  - Fit a logistic regression model with $R_j$ as the outcome
   
   $$
   \text{logit}(p_j) = \beta_0 + \beta_1X_1 + \beta_2X_2 + \cdots + \beta_kX_k
   $$
   
   where $p_j = Pr(R_j=0 | X_1, X_2, \cdots, X_k)$

   - Divide the observations into a fixed number of groups, e.g. 5, based on the propensity scores
   - Apply an approximate Bayesian bootstrap imputation to each group.
     - In each group, randomly draw the same number of samples as with that in the completed data (say $n_1$) with replacement; then draw the number of samples as with that in the missing data (say $n_1$) with replacement from the sample just created

- The method uses only the covariate information that is associated with whether the imputed variable values are missing. It does not use correlations among variables.

- <mark>It is effective for inferences about the distributions of individual imputed variables, such as an univariate analysis, but it is not appropriate for analyses involving relationship among variables, such as a regression analysis (Schafer 1999, p. 11).</mark>

##### MCMC method
- High-level description of the procedure
  - The imputation I-step
    - this step draws values for $Y_{miss}$ from a conditional distribution: $Y_{miss}^{(t+1)} \sim p(Y_{miss}|Y_{obs}, \theta^{(t)})$
  - The posterior P-step
    - Simulates the posterior population mean vector and coariance maxtrix from the complete sample estimates. These estimates are then used in the I-step: $\theta^{(t+1)} \sim p(\theta|Y_{obs}, Y^{(t+1)}_{miss})$
  - This creates a Markov Chain: $(Y_{miss}^{(1)}, \theta^{(1)}), (Y_{miss}^{(2)}, \theta^{(2)}), \cdots$, which comverges in distribution to $p(Y_{miss}, \theta | Y_{obs})$

#### Combining inferences from MI data sets
- Let $\hat{Q}_i$ and $\hat{U}_i$ denote the point and variance estimates from the $i$th imputed data set, $i=1, 2, \cdots, m$. 
  - Then the point estimate from MI is 
      
      $$
      \bar{Q} = {1\over m}\sum_{i=1}^m\hat{Q}_i
      $$

  - The variance estimate associated with $\bar{Q}$ is

      $$ 
      T = \bar{U} + (1 + \frac{1}{m})B
      $$
      
      where 

      $$ 
      \bar{U} = \frac{1}{m}\sum_{i=1}^m \hat{U}_i
      $$

      is the within-impuatio variance; and

      $$
      B = \frac{1}{m-1}\sum_{i=1}^m (\hat{Q}_i - \bar{Q})^2
      $$

      is the between-impuation variance.

   - The statistic $(Q - \hat{Q})T^{-1/2} \sim t_{v_m}$, where

      $$
      v_m = (m-1)\left[ 1 + \frac{\bar{U}}{(1-m^{-1})B}\right]
      $$

      - When the complete-data degrees of freedom $v_0$ is small and there is only a modest proportion of missing data, the computed degrees of freedom, $v_m$, can be much larger than $v_0$, which is inappropriate. Barnard and Rubin (1999) recommend the use of an adjusted degrees of freedom, $v_m^*$. Where

      $$ 
      v_m^* = \left[\frac{1}{v_m} + \frac{1}{\hat{v}_{obs}}\right]^{-1}
      $$

      where

      $$
      \hat{v}_{obs} = \frac{v_0+1}{v_0+3}v_0(1-\gamma),
      $$

      $$
      \gamma = \frac{(1+m^-1)B}{T}.
      $$


#### Multiple imputation efficiency
- The **relative increase in variance ratio** due to nonresponse
   
   $$
   r = \frac{1+m^{-1}B}{\bar{U}}
   $$

   - When there is no missing info about $Q$, both values $r$ and $B$ are zero.
   - With large value of $m$ or a small value of $r$, the df $v_m$ will be large and the distribution will be approx. normal.
 - The **fraction of missing info** about $Q$
   
   $$
   \hat{\lambda} = \frac{r + 2/(v_m+3)}{r+1}
   $$

- The relative efficiency of using the finite $m$ imputation estimator, rather than using an infinite number for the fully efficient imputation, in units of variance, is approx. a funciton of $m$ and $\lambda$.
  
  $$ 
  RE = \left(1+\frac{\lambda}{m}\right)^{-1}
  $$

  - For cases with little missing info, only a small number of imputations are necessary for the MI analysis.


#### Imputation Model
- **Imputer's model**: model used to impute missing values
- **Analyst's model**: model (variables) used to analyze the multiply imputed data
  - In practice, the two models need not be the same, as explained by Schafer (1997, pp. 139–143), who discusses the consequences for various scenarios.

- In general, as many variables should be included in the imputer’s model.
- To produce high-quality impuations for a particular variable, include variables that are potentially related to the imputed variable and variables that are potentially related to the missingness of the imputed variable (Schafer 1997, p. 143).