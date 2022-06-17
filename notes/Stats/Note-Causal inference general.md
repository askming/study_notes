# Causal Inference General Study Notes

## [1. Be Careful When Interpreting Predictive Models in Search of Causal Insights](https://towardsdatascience.com/be-careful-when-interpreting-predictive-models-in-search-of-causal-insights-e68626e664b6)

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


----


## 2.Causal Inference Tutorial from [Data Elixir Newsletter](https://dataelixir.com/)
### [Part 1](https://dataelixir.com/issues/378.html): Foundations
- [Causal Inference: What If?](https://news.dataelixir.com/t/t-l-qzxdd-l-x/)
  > Part 1 of this book ("Causal inference without models") is not short but if you want to learn about making causal inferences from data, this provides one of the best introductions to the topic you can find on the web. It assumes zero prior knowledge of modeling or public health, and is quite approachable. 

  *Note: Already have this book for a while but haven't read yet.*

- [Causality for Machine Learning](https://ff13.fastforwardlabs.com)
  > Chapters one and two provide a math-less introduction to a number of critical topics in causal inference. You'll learn about causal graphs (a simple way to visualize causal relationships), the concept of the "causal hierarchy", and counterfactuals.

- [Thinking Clearly About Correlations and Causation](https://journals.sagepub.com/doi/full/10.1177/2515245917745629?utm_campaign=Data_Elixir&utm_source=Data_Elixir_378)
 
  > a slightly deeper but still math-less explanation of causal graphs.

  *Note: paper in Zotero*

- [Causal Inference Challenges in Industry: A perspective from experiences at LinkedIn](https://www.youtube.com/watch?v=OoKsLAvyIYA)
  > this video gives you a look at how causal inference is viewed from an industry perspective (specifically at LinkedIn)

### [Part 2](https://dataelixir.com/issues/381.html): Core methods and tools

- MS tutorials
  - [Understanding the fundamentals](https://medium.com/data-science-at-microsoft/causal-inference-part-1-of-3-understanding-the-fundamentals-816f4723e54a) 
  - [Selecting algorithms](https://medium.com/data-science-at-microsoft/causal-inference-part-2-of-3-selecting-algorithms-a966f8228a2d)
  -  [Model validation and applications](https://medium.com/data-science-at-microsoft/causal-inference-part-3-of-3-model-validation-and-applications-c84764156a29)
  
    > In this multi-part series, Jane Huang and colleagues at Microsoft outline the critical effect measures to know in causal inference, the data assumptions necessary to employ causal inference in the first place, a comparison of the many open-source packages to carry out the job, and lay out a nice categorization of the algorithms that one can employ. 

- [Implementation of G-Computation on a Simulated Data Set: Demonstration of a Causal Inference Technique](https://academic.oup.com/aje/article/173/7/731/104142?utm_campaign=Data_Elixir&utm_source=Data_Elixir_381) 

  > This paper describes one of the more straight-forward modeling approaches for causal inference. This approachable paper has a FAQ and comes with code if the reader is interested in seeing how the authors arrived at their numbers. It's an approach that [performs well against others in simulation studies too](https://news.dataelixir.com/t/t-l-qjlcut-ehrkiilji-v/).

  *note: paper in Zotero*

- [Trustworthy Online Controlled Experiments:
A Practical Guide to A/B Testing](https://experimentguide.com/?Chap1T)
  
  > This gem of a book was written by the current industry giants in A/B testing and evaluating treatment effects. Understanding the methods and metrics of A/B testing will give you a fantastic intuition for various causal inference approaches.  


---
### 3. Effect (measure) modifier vs confounding factor vs interaction
- We say that $V$ is a *modifier* of the effect of $A$ on $Y$ when the average causal effect of $A$ on $Y$ varies across levels of $V$.
  - Effect modifier is really an effect measure modifier, since its effect depends on which outcome measure (e.g. odds ratio, risk difference, etc.) is being used.
  - Effect modification can exist when it's not a confounding factor
- Does confouding leads to effect modification??
- When there is effect modificaiton, there is an interaction between the effect modifier and the treatment variable
  
#### Reference
- [Confounding vs. effect modification](https://thestatsgeek.com/2021/01/13/confounding-vs-effect-modification/)