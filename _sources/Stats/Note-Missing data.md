# Missing data

## 1. Missing data mechanism 

1. MCAR: missingness is independent of observed & unobserved data
2. MAR: missing only depends on observed information and is independent on unobserved
   -  MAR assumption = ignorability assumption, similar/same assumption as ignorability in the causal framework
3. Missing that depends on unobserved predictors
4. Missing that depends on the missing value itself, e.g. "censoring"

## 2. Handling missing data

1. Discard the data with missing or complete-case analysis
   - Can potentially leading to hiased results if missing cases is different from complete case
   - Many data can potentially discarded => much smaller sample size
2. Available-case analysis
   - use different subsets (complete data) for different analyses
   - => non-consistency b/t subsets systematic bias
   - related: complete-variables analysis
3. Non-response weighting: e.g. IPW
4. Impute missing data
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
5. Random imputation of single variable
   - simple random imputation
   - zero coding & topcoding
     - e.g. impute income that is positive only 
     - topcoding: set up-limit to the value, censor any value beyond the up-limit $\rightarrow$ not to correct the data, but rather perform simple transformation to improve the predictive power of the regression model
   - Using regression predictions to perform deterministric imputation



