# Adaptive Clinical Trial Design

## Adaptive group sequential design with sample size re-estimation

### Sample size re-estimation (SSR)

- There are mainly two types of SSR
  - Blinded SSR
  - Unblinded SSR

- SSR is usually used with group sequential design (GSD), in combination it’s called adaptive-GSD (AGSD)
- The goal of sample size re-estimation is to increase the power for the final analysis in the event of *promising* interim data. That is, there is a potential to show clinically meaningful benefit from the treatment, while the study with original sample size is under-powered to demonstrate such benefit. It is usually used when the original sample size estimation is more optimistic than what is needed in the reality due to various reasons, which leads to smaller total sample size needed to achieve the targeted power to show the actual underlying difference.
- One summary papers on this topic:
  - *Pritchet et. al., 2015 Stat in Biopham Research*

#### Unblinded SSR

- The major concern for u-SSR is that inappropriate analysis method followed by adjusted sample size may inflate the overall type 1 error rate. [^type_1_inflation] Different methods have been developed to address this issue:

  - *Cui, Hung, and Wang (1999)* Biometrics paper proposed a method based on modifying the weights used in the traditional repeated significance test (two-sample mean), where the overall nominal $\alpha$ level is preserved. 
  - *Chen, DeMets, and Lan (2004)* Stats in Med paper discussed the possible scenarios when the type 1 error will not be inflated after u-SRR, where statistical adjustment is not necessary (i.e. using conventional hypothesis test). Those scenarios are referred as *promising* interims, whose definition is based on the conditional power given the interim data. They proposed the conditional power should be greater than 50% to be considered as promising.
  - *Gao, Ware, and Mehta (2008)* J Biopharm Stat paper extended above idea to a broader range of promising zones.
  - *Mehta and Pocock (2010)* Stats in Med paper made the work of *Gao, Ware and Mehta* more accessible to practioners by presenting it in the context of two-stage designs and tabulating explicit cut-off values for the promising region under different adaptive rules based on conditional power.


##### <a name="promisingzone"> Summary of **{cite:p}`mehta2011adaptive`** paper (so-called *promising zone* method)</a>

- Notations:

  - $n_1$: sample size at interim; $n_2$: total planned sample size; $\tilde{n}_2 = n_2 - n_1$
  - $\tilde{n}_2^*$: updated $\tilde{n}_2$ after sample size re-estimation; $n_2^* = n_1 + \tilde{n}_2^*$, the updated total sample size after re-estimation 
  - $n_{\max}$: the max acceptable sample size after re-estimation
  -  $\delta$: true underlying treatment difference; $\hat{\delta}$ the empirical estimate of $\delta$ based on observed data
  - $Z_i$: Wald statistics at stage $i, \text{e.g.} i = 1, 2$

- Statistical components used in u-SSR: 

  - conditional power (based on interim results and for the sample size left): $\text{CP}_{\hat{\delta}}(z_1, \tilde{n}_2) = \text{Pr}(Z_2 > z_{\alpha}|z_1)$
  - $\text{CP}_{\min}$: lower limit of the conditional power defining the promising region; upper limit is the targeted power of the study, i.e. $1-\beta$
  - $b(z_1, \tilde{n}_2^*(z_1))$: new critical value threshold under sample size re-estimation

- Practical steps in application

  1. define $\text{CP}_{\min}$ by finding the region of conditional power that corresponds to $b(z_1, \tilde{n}_2^*(z_1))$ below $z_{\alpha}$: $\mathcal{P} = \{\text{CP}_{\hat{\delta}_1}(z_1, \tilde{n}_2): b(z_1, \tilde{n}_2^*(z_1)) \le z_{\alpha}\}$

     - This requires to compute $b(z_1, \tilde{n}_2^*(z_1))$ for each $\text{CP}_{\hat{\delta}_1}(z_1, \tilde{n}_2) \in (0, 1)$: $\text{CP} \rightarrow z_1 \rightarrow \tilde{n}_2’ \text{ and } n_2' \rightarrow n_2^* \rightarrow b(z_1, \tilde{n}_2^*(z_1))$
     - **This is defined ahead of data are unblinded**

  2. calculate the $\text{CP}_{\hat{\delta}_1}(z_1, \tilde{n}_2)$ from observed data and check which region (unfavorable, promising, or favorable) it falls into:

     - if it falls into unfavorable region, sample size won’t be adjusted; instead, need to check it against the futility boundary
     - if it falls into the favorable regions, no sample size adjustment is needed; continue the study without modification
     - if it falls into the promising region: calculate the sample size such that the conditional power meets the targeted power: i.e., find $n_2'$ such that $\text{CP}_{\hat{\delta}_1}(z_1, \tilde{n}'_2) = 1-\beta$

  3. If sample size needs to be adjusted, i.e. the interim result is promising, update the sample size to 

     ​																			$$n_2^* = \min(n_{\max}, n_2')$$

- Sample size re-estimation (adaptive design) v.s. group sequential design (GSD) vs. adaptive GSD (A-GSD)

  - 

##### Comparison of commonly used uSSR re-estimation methods

*Based on Herrmann et al (2020): A new conditional performance score for the evaluation of adaptive group sequential designs with sample size recalculation*

- Observed conditional power approach
- Restricted observed conditional power approach
- <a href="#promisingzone">Promising zone approach</a>
- Optimization function approach





[^type_1_inflation]: If the sample size recalculation is based on estimates of nuisance parameters such as within-group variance for normal response or pooled event rate for a binary outcome, the type I error rate will not be materially inated. However, if the sample size recalculation is based on the observed treatment dierence, the type I error rate could be substantially inated and an appropriate statistical adjustment may be needed to control it.

```{bibliography}
:filter: docname in docnames
```