# Causal Inference General Study Notes

## [Be Careful When Interpreting Predictive Models in Search of Causal Insights](https://towardsdatascience.com/be-careful-when-interpreting-predictive-models-in-search-of-causal-insights-e68626e664b6)

> A careful exploration of the pitfalls of trying to extract causal insights from modern predictive machine learning models.

- Predictive model tends to identify the **correlations** between features and the outcome, which is different from causation that indicates the underlying data generation (**causal**) process

- Thus, variable importance identified from the predictive model (e.g using [SHAP](**https://github.com/slundberg/shap**) isn't a good indicator for a strong causal effect on the outcome

- When we have the same information captured by several features, predictive models can use any of those features for prediction, even though they are not all causal. 

- e.g. the regularized model identifies Ad Spend as a useful predictor because it summarizes multiple causal drivers (so leading to a sparser model), but that becomes seriously misleading if we start to interpret it as a causal effect.

### Using predictive model to help answer the causal questions

- By looking at the bar plot (dendrogram) generated from SHAP to identify covariate(s) that is  "independent" of and unconfounded by other (observed & unobserved) factors

- When features merge together at the bottom (left) of the dendrogram it means that that the information those features contain about the outcome (renewal) is very redundant and the model could have used either feature. When features merge together at the top (right) of the dendrogram it means the information they contain about the outcome is independent from each other.

- The important ingredient that allowed XGBoost to get a good causal effect estimate for Economy is the feature’s strong independent component (in this simulation); its predictive power for retention is not strongly redundant with any other measured features, or with any unmeasured confounders. In consequence, it is not subject to bias from either unmeasured confounders or feature redundancy.

### Using inference method (as predictive model can't help)

1. Observed confounding

2. Observational Causal Inference

- One particularly flexible tool for observational causal inference is **double/debiased machine learning**. It uses any machine learning model you want to first deconfound the feature of interest (i.e. Ad Spend) and then estimate the average causal effect of changing that feature (i.e. the average slope of the causal effect).
  - Train a model to predict a feature of interest (i.e. Ad Spend) using a set of possible confounders (i.e. any features not caused by Ad Spend).
  - Train a model to predict the outcome (i.e. Did Renew) using the same set of possible confounders.
  - Train a model to predict the residual variation of the outcome (the variation left after subtracting our prediction) using the residual variation of the causal feature of interest.
- check out causal inference package like `econML` or `CausalML`

- Double ML (or any other observational causal inference method) only works when you can measure and identify all the possible confounders of the feature for which you want to estimate causal effects.

- However, as we usually don't know the true causal graph thus we can't decide which variables are confounded by which variable; we could look at the redundancy in the SHAP bar plot and find out the most redundant features and so are good candidates to control for

3. Non-confounding redundancy

- This occurs when the feature we want causal effects for causally drives, or is driven by, another feature included in the model, but that other feature is not a confounder of our feature of interest.

- Non-confounding redundancy can be fixed in principle by removing the redundant variables from the model

- Controlling for a feature we shouldn’t tends to hide or split up causal effects, while failing to control for a feature we should have controlled for tends to infer causal effects that do not exist. This generally makes controlling for a feature the safer option when you are uncertain.

4. Other causal inference techniques

- principals of instrumental variables, differences-in-differences, or regression discontinuities

### When both (predictive model & causal method) fail

- Barring the ability to measure the previously unmeasured features (or features correlated with them), finding causal effects in the presence of unobserved confounding is difficult. In these situations, the only way to identify causal effects that can inform policy is to create or exploit some ***\*randomization\**** that breaks the correlation between the features of interest and the unmeasured confounders. 

*_SHAP: SHapley Additive exPlanations_*