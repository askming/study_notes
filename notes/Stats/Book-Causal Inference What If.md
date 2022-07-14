# [book] Causal Inference: What If

*by Miguel A. Hernan and James M. Robins*

## Chapter 2 Randomized Experiments

### 2.1 Randomization
- Counterfacatual: $Y^a$ the "theoretical" outcome under certain treatment $a$

```{margin}
Note  $Y^a\perp\!\!\!\perp A$ is different from  $Y\perp\!\!\!\perp A$
```
- **Exchangeability** assumption: $Y^a\perp\!\!\!\perp A$, the counterfacatual is independent of actual treatment assigned; i.e. the counterfacatual is "fixed" no matter what treatment the subject receives. It can be observed if the actual treatment received is the same as the counterfactual treatment.

- **Mean exchangeability**: $E[Y^a|A=1] = E[Y^a|A=0] \text{ for all } a$
  - Exchangeability implies mean exchangeability, but not verse vasa (e.g. due to the difference in other distributional parameter, such as variance)
  - For dichotomous outcomes, exchangeability is equivalent to mean exchangeability
- <mark>In ideal randomized experiments association is causation.</mark>

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
    \sum_l E\left[Y|A=a, L=l\right]Pr(L=l)
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
- Key ideas in this chapter
  - Definition of effect modification (EM); why we care about it?
  - How to identify/quantify effect modification? Hint: use stratification
  - Stratification and matching are both adjustment methods; what is the definition of adjustment? Any other adjustment methods?

### 4.1 & 4.3 Definitionof effect modification (what) and why effect modification
- Definition of effect modification
  - We say that $V$ is a modifier of the effect of $A$ on $Y$ when the average causal effect of $A$ on $Y$ varies across levels of $V$
    - The existence of effect modification depends on the effect measure being used (e.g., risk difference, risk ratio), so it is more accurate to use the term *effect-measure modification*, to emphasize the dependence of the concept on the choice of effect measure
  - Since the average causal effect can be measured using different effect measures (e.g., risk difference, risk ratio), the presence of effect modification depends on the effect measure being used.
  - Here only consider variables $V$ that are not affected by treatment $A$

- Types of effect modification
  - *qualitative effect modification*: when the average causal effects in the subsets are in the opposite direction.
    - In the presence of qualitative effect modification, additive effect modification implies multiplicative effect modification, and vice versa. In the absence of qualitative effect modification, however, one can find effect modification on one scale (e.g., multiplicative) but not on the other (e.g., additive).

- Why we are about effect modification
  1. If a factor $V$ modifies the effect of treatment $A$ on the outcome $Y$ then the average causal effect will differ between populations with different prevalence of $V$
     - There is generally no such athingas “*the average* causal effect of treatment $A$ on outcome $Y$ (period)”, but “the average causal effect of treatment $A$ on outcome $Y$ in a population with a particular mix of causal effect modifiers.” 
  2. Evaluating the presence of effect modification is helpful to identify the groups of individuals that would benefit most from an intervention.
     - if there is a nonzero causal effect in at least one stratum of $V$ and the counterfactual risk $Pr[Y^{a=0} =1|V = v]$ varies with $v$,then effect modification is guaranteed on either the additive or the multiplicative scale. 
     - Additive, but not multiplicative, effect modification is the appropriate scale to identify the groups that will benefit most from intervention.
  
  ```{margin}
  The terms “effect modification” and “interaction” are sometimes used as synonymous in the scientific literature. “Interaction” as a causal concept that is related to, but different from, effect modification (Chapter 5).
  ```
  3. The identification of effect modification may help understand the biological, social, or other mechanisms leading to the outcome.

- *Transportability* of causal inference: The extrapolation of causal effects computed in one population to a second population
  - Conditional causal effects in the strata defined by the effect modifiers may be more transportable than the causal effect in the entire population, but there is no guarantee that the conditional effect measures in one population equal the conditional effect measures in another population. Due to different reasons:
    - unmeasured, or unknown, causal effect modifiers whose conditional distributions vary between the two populations
    - when transportability may be justified from one to antoher population?
      - Effection modification: similar distribution of effect modifiers
      - Versions fo treatment: requires similar distr. of versions fo treatment
      - Interference: treating one individual many affect the outcome of others in the populaiton
  - transportability of causal effects is an unverifiable assumption that relies heavily on subject-matter knowledge. It is a more difficult problem than the identification of causal effects in a single population


### 4.2, 4.4, & 4.5 How to quantify effect modification
#### Stratification
```{margin}
if treatment assignment was random and unconditional, exchangeability is expected in every subset of the population.
```
- To identify effect modification by $V$ in an ideal experiment with unconditional randomization, one just needs to conduct a stratified analysis, that is, to compute the association measure in each level of the variable $V$.

```{margin}
Step 2 can be ignored when $V$ is equal to the variables $L$ that are needed for conditional exchangeability
```
- The procedure to compute the conditional risks $Pr[Y^{a=1}=1|V = v]$ and $Pr[Y^{a=0}=1|V = v]$ in each stratum $v$ has two stages: 1) stratification by $V$ , and 2) standardization by $L$ (or, equivalently, IP weighting with weights depending on $L$).

#### Adjustment via stratification
- In practice stratification is often used as an alternative to standardization (and IP weighting) to adjust for $L$. In fact, the use of stratification as a method to adjust for $L$ is so widespread that many investigators consider the terms “stratification” and “adjustment” as synonymous.
  - Stratification provides conditional effect measures within each of the stratum

- Unlike standardization and IP weighting, adjustment via stratification requires computing the effect measures in subsets of the population defined by a combination of all variables $L$ that are required for conditional exchangeability.
  ```{margin}
  In contrast, stratification by $V$ followed by IP weighting or standardization to adjust for $L$ allows one to deal with exchangeability and effect modification separately, as described above.
  ```
  - That is, the use of stratification forces one to evaluate effect modification by all variables $L$ required to achieve conditional exchangeability, regardless of whether one is interested in such effect modification.
- Other problems associated with the use of stratification are *noncollapsibility* of certain effect measures like the odds ratio (Fine Point 4.3) and inappropriate adjustment that leads to bias when, in the case for **time-varying treatments**, it is necessary to adjust for time-varying variables $L$ that are affected by prior treatment (see Part III of the book).

```{margin}
Stratification requires positivity in addition to exchangeability: the causal effectcannotbecomputed in subsets $L=l$ in which there are only treated, or untreated, individuals.
```
- *Ristriction*: compute causal effect in only some of the strata defined by the variables $L$ and no stratum-specific measures for some other strata


#### Adjustment via matching
```{margin}
Pay attention to the difference in matching applied to case-control studies: 

Even if the matching factors suffice for conditional exchangeability, matching in cases and controls does not achieve unconditional exchangeability of the treated and the untreated in the matched population. Adjustment for the matching factors via stratification is required to estimate conditional (stratumspecific) effect measures.
```
- The goal of matching is to construct a subset of the population in which the variables $L$ have the same distribution in both the treated and the untreated.
  - Under the assumption of conditional exchangeability given $L$, the result of this procedure is (unconditional) exchangeability of the treated and the untreated in the matched population.
  - The chosen group defines the subpopulation on which the causal effect is being computed (e.g. effect in the treated/untreated)

- Because the matched population is a subset of the original study population, the distribution of causal effect modifiers in the matched study population will generally differ from that in the original, unmatched study population

#### Fine and Tech Points 4.1 Causal effect in the treated/untreated 
- Consider a particular and special subset of the population, the treated ($A=1$), the average causal effect in the treated is expressed as

$$
Pr(Y^{a=1} =1|A=1) - Pr(Y^{a=0} = 1 | A=0)
$$

- The causal risk ratio in the treated is also called the **standardized morbidity ratio (SMR)**
  ```{margin}
  However, even though one could say that there is effect modification by the pretreatment variable $V$ even if $V$ is only a surrogate (e.g., nationality) for the causal effect modifiers, one would not say that there is modification of the effect $A$ by treatment $A$ because it sounds confusing.
  ```
- The average effect in the treated will differ from the average effect in the population if the distribution of individual causal effects varies between the treated and the untreated.

![kyDpUe_2022_06_20](https://cdn.jsdelivr.net/gh/askming/upic@master/uPic/kyDpUe_2022_06_20.png)

```{margin}
In other words, it is irrelevant whether the risk in the untreated, had they been treated, equals the risk in those who were actually treated.
```
- Computing the average causal effect in the treated only requires partial exchangeability $Y^{A=0} \perp\!\!\!\perp A|L$.
  - similarly, the average causal effect in the untreated is computed under the partial exchangeability condition $Y^{a=1} \perp\!\!\!\perp A|L$

- To compute the counterfactual mean $E(Y^{a}|A=a')$:
  - Standardization
    
    $$
    E(Y^{a}|A=a') = \sum_l E(Y|A=a, L=l)Pr(L=l|A=a')
    $$

  - IP weighting
    
    $$
    E(Y^{a}|A=a') = \frac{E\left[\frac{I(A=a)Y}{f(A|L)Pr(A=a'|L)}\right]}{E\left[\frac{I(A=a)}{f(A|L)Pr(A=a'|L)}\right]}
    $$

### 4.6 Summary
- Stratified analysis is needed to inspect the existence of effect modification from certain variable(s) $L$ on the treatment $A$
- When doing standardization and IP weighting, stratefication is used as a technique to estimate the average/weighted outcome in each of the treatment arms to construct the final estimate of the treatment effect
  
  $$
  \begin{array}{ll}
  Pr(Y^{a=1}=1) - Pr(Y^{a=0}=1)
  &= \sum_lPr(Y^{a=1}=1|L=l) - \sum_lPr(Y^{a=0}=1|L=l) \\
  &= \sum_l Pr(Y=1|A=1, L=l) - \sum_l Pr(Y=1|A=0, L=l)
  \end{array}
  $$

- In contrast, pooling stratum-specific effect (e.g., Woolf, Mantel-Haenszel, maximum likelihood) needs to compute the stratum-specific effect first followed by combining the individual effects using some kind of weighted average with weights chosen to reduce the variability of the pooled estimate.
  - The main goal of pooling is to obtain a narrower confidence interval around the common stratum-specificeffect measure, but **the pooled effect measure is still a conditional effect measure**.

- These four approaches can be divided into two groups according to the type of effect they estimate: standardization and IP weighting can be used to compute either marginal or conditional effects, stratification/restriction and matching can only be used to compute conditional effects in certain subsets of the population.
  - The idea is that, if the effect measure is the same in all strata (i.e., if there is no effect-measure modification), then the pooled effect measure will be a more precise estimate of the common effect measure.
  - In the absence of effect modification, the effect measures (risk ratio or risk difference) computed via standardization, IP weighting, stratification/restriction, and matching will be equal.

- Investigators conducting observational studies need to explicitly define the causal effect of interest and the subset of the population in which the effect is being computed. Otherwise, misunderstandings might easily arise when effect measures obtained via different methods are different.



## Chapter 5 Interaction