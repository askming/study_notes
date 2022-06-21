# [book] Causal Inference: What If

*by Miguel A. Hernan and James M. Robins*

## Chapter 2 Randomized Experiments

### 2.2 Conditional randomization
- *Conditionally randomized experiments* meaning there are several randomization probabilities that depend (are conditional) on the values of some variable(s) $L$.
- *Marginally randomized experiments* meaning there is only a single unconditional (marginal) randomization probability (doesn't need to be 0.5) that is common to all individuals.
  - a marginally randomized experiment is expected to result in exchangeability of the treated and the untreated: 
  
  $$
  Pr(Y^a=1|A=1) = Pr(Y^a=1|A=0) \text{ or } Y^a \perp\!\!\!\perp A \text{ for all } a
  $$

```{margin}
This imbalance indicates that the risk of bad outcome in treated, had they remained untreated, would have been higher than that in the untreated.
```
- In contrast, a conditionally randomized experiment will not generally result in exchangeability of the treated and the untreated because, by design, each group may have a different proportion of individuals with bad prognosis.
- However, the conditional (or stratified) randomization can be considered as a combination of separate marginal randomiztions from each unique stratum defined by the randomization factors, within which the general rules of marginal randomization and causal inference apply, that is
  
  $$
  Pr[Y^a=1|A =1, L=l]=Pr[Y^a=1|A =0, L=l] \text{ or } Y^a \perp\!\!\!\perp A|L=l \text{ for all } a
  $$

- To calculate the population average causal effect with conditional randomization, do the following two steps
  1. First, compute the average causal effect in each of these subsets or strata of the population
  2. Second, compute the average causal effect by combining the individual effects from step 1
       - Using <mark>standardization</mark> or <mark>inverse probability weighting</mark> (wich are mathmatically equivalent)


### 2.3 Standardization


### 2.4 Inverse probability weighting (IPW)


## Chapter 4 Effect Modification
- We say that $V$ is a modifier of the effect of $A$ on $Y$ when the average causal effect of $A$ on $Y$ varies across levels of $V$
  - Since the average causal effect can be measured using different effect measures (e.g., risk difference, risk ratio), the presence of effect modification depends on the effect measure being used.
  - *qualitative effect modification*: when the average causal effects in the subsets are in the opposite direction.




![kyDpUe_2022_06_20](https://cdn.jsdelivr.net/gh/askming/upic@master/uPic/kyDpUe_2022_06_20.png)