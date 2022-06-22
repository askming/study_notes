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

```{margin}
Weighted average of the treatment effects from subgroups; weight is defined by the sample size of the subgorup
```
- More formally, in *standardization*, the marginal counterfactual risk $Pr[Y^a=1]$ is the weighted average of the stratum-specific risks $Pr[Y^a=1|L=0]$ and $Pr[Y^a=1|L=1]$ with weights equal to the proportion of individuals in the population with $L=0$ and $L=1$, respectively.

- Derivation of counterfactual risk by standardization
  
  $$
  Pr(Y^a=1) = \sum_l Pr(Y^a=1|L=l)Pr(L=l)
  $$

  under conditoinal exchangeability, we can replace the (unobserved) counterfactual riks with the obsered risk:

  $$
  Pr(Y^a=1|L=l) = Pr(Y=1|A=a, L=l)
  $$

  - When, as here, a counterfactual quantity can be expressed as function of the distribution (i.e., probabilities) of the observed data, we say that the counterfactual quantity is identified or identifiable; otherwise, we say it is unidentified or not identifiable.

### 2.4 Inverse probability weighting (IPW)
```{margin}
The idea is that assuming all the patients in one subgroup receive treatment/control, and the outcome probability follows the obseved one from one of the arms (under conditional exchangeability assumption), how would the outcome look like (pseudo counterfactual).

Since the sample size in either arm is increased by N/ni folds (inverse of the proportion of patients in one of the arms), where N is the total sample size in this subgroup and ni is the sample size in one of the arms, the outcome from each invidividual should also be also upscaled by this ratio (N/n).
```

```{margin}
IP weighted estimators were proposedbyHorvitzandThompson (1952) for surveys in which subjects are sampled with unequal probabilities
```

- Under conditional exchangeability $Y^a \perp\!\!\!\perp A|L$ in the original population, the treated and the untreated are (unconditionally) exchangeable in the pseudo-population because the $L$ is independent of $A$.
  - That is, the associational risk ratio in the pseudo-population is equal to the causal risk ratio in both the pseudo-population and the original population.

  ![sJRh3P_2022_06_22](https://cdn.jsdelivr.net/gh/askming/upic@master/uPic/sJRh3P_2022_06_22.png)

- Informally, the pseudo-population is created by weighting each individual in the population by the inverse of the conditional probability of receiving the treatment level, IP weight:
  
  $$
  W^A = 1/f(A|L)
  $$

- Standardization and IP weighting are mathematically equivalent
  - In fact, both standardization and IP weighting can be viewed as procedures to build a new tree in which all individuals receive treatment $a$. Each method uses a different set of the probabilities to build the counterfactual tree: IP weighting uses the conditional probability of treatment $A$ given the covariate $L$ (as shown in Figure above), standardization uses the probability of the covariate $L$ and the conditional probability of outcome $Y$ given $A$ and $L$.
  
  ```{margin}
  In a slight abuse of language we sometimes say that these methods *control* for $L$, but this “analytic control” is quite different from the “physical control” in a randomized experiment. 
  Standardization and IP weighting can be generalized to conditionally randomized studies with continuous outcomes
  ```
  - Because both standardization and IP weighting simulate what would have been observed if the variable (or variables in the vector) $L$ had not been used to decide the probability of treatment, we often say that these methods ***adjust*** for $L$.
  
- Proof of the equivalence of IP weighting and standardization
  - Standardized mean for treatment level $a$ is defined as

    $$
    \sum_l E\left[Y|A=a, L=l\right]
    $$
  
  - IP weighted mean for the treatment level $a$ is defined as

    $$
    E\left[\frac{I(A=a)Y}{f(A|L)}\right]
    $$
    - proof of equivalence
    $$
    E\left[\frac{I(A=a)Y}{f(A|L)}\right]  \\ 
    = \sum_l\frac{1}{f(a|l)}\{E\left[Y|A=a, L=l\right]f(a|l)Pr(L=l)\} \\
    =\sum_l E\left[Y|A=a, L=l\right]
    $$

- It can be shown that both estimates of mean are euqal to the counterfactual mean $E[Y^a]$ under the conditional exchangeability assumption, that is
  
  $$
  E[Y^a] = \sum_l E\left[Y|A=a, L=l\right] = E\left[\frac{I(A=a)Y}{f(A|L)}\right], \text{ under } Y\perp\!\!\!\perp A|L
  $$

  - When treatment is continuous, which is an unlikely design choice in conditionally randomized experiments, $E[I(A=a)Y/f(A|L)]$ is no longer equal to $\sum_l E\left[Y|A=a, L=l\right]$ and thus is biased for $E[Y^a]$ even under exchangeability

## Chapter 4 Effect Modification
- We say that $V$ is a modifier of the effect of $A$ on $Y$ when the average causal effect of $A$ on $Y$ varies across levels of $V$
  - Since the average causal effect can be measured using different effect measures (e.g., risk difference, risk ratio), the presence of effect modification depends on the effect measure being used.
  - *qualitative effect modification*: when the average causal effects in the subsets are in the opposite direction.




![kyDpUe_2022_06_20](https://cdn.jsdelivr.net/gh/askming/upic@master/uPic/kyDpUe_2022_06_20.png)