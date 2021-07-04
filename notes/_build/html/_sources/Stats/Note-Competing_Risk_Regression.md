---
tags: survival analysis
---

# Competing risk models

## Why doing competing risk model

- In competing risk analysis, individuals experiencing the competing risk event have zero probability of experiencing the event of interest. 
- In contrast, the naïve Kaplan-Meier approach assumes that these individuals would experience the same probability of event of interest in pure theory (non-informative censoring). 
  - Thus, the latter <u>overestimates</u> the cumulative incidence <u>of the event of interest</u>. 

- **Cumulative incidence** derived from Kaplan-Meier estimator is always **larger** than that obtained by accounting for competing risks. 
  - In Kaplan-Meier estimation, an individual is removed from the risk set when the individual experiences competing event, i.e. treated as non-informative censoring case. 
  - Within competing risk framework, the overall survival probability is calculated, which accoutns for any type of event,  thus is **lower** compared with that from naive KM estimate.
- Treating competing risk (e.g. death) as censoring is not appropriate:
  - it removes the importance of competing event difference between the groups that may mislead the primary comparison by introduing bias
  - the primary outcome is not independent of the competing risk, in which case, treating the competing risk as censoring violates the non-informative censoring assumption

## Basic concepts

- **Cause-specific hazard function**, $h_k(t)$: the instantaneous rate of failure due to cause $k$ conditional on survival until time t or later
  $$
  h_k(t) = \lim_{\Delta t\rightarrow0}\frac{P(t<T<t+\Delta t, \delta=k|T>t)} {\Delta t}, k=1, \cdots, K
  $$

- **Cumulative incidence function** (CIF or **subdistribution function**), is the prob. of failure due to cause $k$ prior to time $t$
  $$
  F_k(t)=P(T\le t, \delta = k), k = 1, \cdots, K
  $$

  - it's not a true probability distribution

  - $$
    F_k(t) = \int_0^tS(u)h_k(u)du = \int_0^t S(u)dH_k(u), k = 1, \cdots, K
    $$

    - where $H_k(t) = \int_0^t h_k(u)du$ is the cause-specific cumulative hazard function and 
    - $S(t) = \exp(-\sum_{k=1}^K H_k(t))$ is the overall survival function, which is the probability of surviving beyond time $t$

  - when $K > 1$ (i.e. in the presence of competing risk), the one-to-one relationship does not exist between the cause-specific hazard function and the corresponding CIF

## Competing risk models

### 1. Cause-specific hazard modeling

- This model assumes competing events other than the primary event of interest as consoring. For cause $k$, a separate proportional hazards model can be assumed:
  $$
  h_k(t)={h}_{k0}(t)\exp(\beta_k'Z)
  $$
  and the partial likelihood function for $k$th event is given as
  $$
  {L}(\beta_k) = \prod_i\left(\exp(\beta'_kZ_i)\over\sum_{j\in\tilde{\mathcal{R}_i}}\exp(\beta'_kZ_j)\right)^{\delta_i \equiv k}
  $$
  where $\mathcal{R}_i$ denotes the risk set at $X_i$ and contains individuals who don't fail from any cause or are not censored before $X_i$.

- Because competing risks are present, it is no longer appropriate to interpret the cause-specific hazard ratios in terms of a relative risk type of measure as regular hazard ratios. As {cite:p}`andersen2012interpretability` point out, this is a direct consequence of the loss of the one-to-one relationship between the cause-specific hazard function and the cumulative incidence function.

### 2. Cumulative incidence (CIF) modeling (or proportional subdistribution hazards model (PSH), or Fine and Gray model)

- This method models the subdistribution hazard, which is defined as 
  
  $$
  \tilde{h}_k(t)=\frac{d}{dt}\log(1-F_k(t))
  $$

  and 

  $$
  \tilde{h}_k(t)=\tilde{h}_{k0}(t)\exp(\beta_k'Z)
  $$

- The pseudo-liklihood function is written as 
  
  $$
  \tilde{L}(\beta_k) = \prod_i\left(\exp(\beta'_kZ_i)\over\sum_{j\in\tilde{\mathcal{R}_i}}\omega_{ij}\exp(\beta'_kZ_j)\right)^{\delta_i \equiv k}
  $$

  where $\omega_{ij}$ is the subject-specific weight for those in the risk set. Subject who experience no event of interest before $X_i$ are assigned weight $\omega_{ij} =1$, and subject who experience competing events before $X_i$ are given weight $\omega_{ij} = \frac{\hat{G}(X_i)}{\hat{G}(\min(X_j, X_i))}$, which decrease with time (refers to event time $X_i$). $\hat{G}(t)$ is the KM estimate of the survival function of the censoring distribution, which is the cumulative prob. that a subject is still be followed at time $t$.

  - The new risk set $\tilde{\mathcal{R}}_i$ is modified from the risk set definition that comes from the regular likelihood in order to accommodate the subdistribution hazard. The difference is that the new risk set includes subjects who have experienced a competing event before $X_i$ and are therefore immortal.
  - The subdistribution hazard function might not have the proper interpretation of an instantaneous risk of failure {cite:p}`andersen2012interpretability`. {cite:p}`austin2017practical` provided practical recommendations for interpreting the regression coefficients and reporting the results properly.

- As with Cox models, **Fine and Gray** is also based on proportional hazards. 

  - The alternative **Gray's test** is a non-parametric test that does not rely on the proportional hazards assumption; however, it does not offer an effect size or the ability to adjust for confounders, analogous to the log-rank test

- In **Subdistribution hazard ratio (SHR)**, the magnitude of which is affected by both the time to event of interest among survivors and the probability of competing event. It assesses the association between an intervention and primary event of interest *accounting for the existence* of the alternative outcome of competing event. 

- Difference between <u>cause-specific hazard</u> vs <u>subdistribution hazard</u> is that the competing risk events are treated differently. The former considers competing risk events as non-informative censoring, whereas the latter takes into account the informative censoring nature of the competing risk events {cite:p}`zhang2017survival`

  - Proportional subdistribution hazards (PSH) model is a commonly used method for regression analysis of time-to-event data in the presence of competing risks.
- Latouche et al. (2013) argue that a proper competing-risks analysis should report results on all the cause-specific hazards and cumulative incidence functions.


## Software for PSH model

- SAS: [PSHREG](https://cemsiis.meduniwien.ac.at/en/kb/science-research/software/statistical-software/pshreg/); PROC LIFETEST can also conduct hypothesis tests account for competing risks

- R: [Package ‘cmprsk’](https://cran.r-project.org/web/packages/cmprsk/cmprsk.pdf)



## References

```{bibliography}
:filter: docname in docnames
```

- [When do we need competing risks methods for survival analysis in nephrology? \| Nephrology Dialysis Transplantation | Oxford Academic](https://academic.oup.com/ndt/article/28/11/2670/1823847)

- [Patient Death as a Censoring Event or Competing Risk Event in Models of Nursing Home Placement](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2811964/)

- [Cause-Specific Analysis of Competing Risks Using the PHREG Procedure](https://www.sas.com/content/dam/SAS/support/en/sas-global-forum-proceedings/2018/2159-2018.pdf)

