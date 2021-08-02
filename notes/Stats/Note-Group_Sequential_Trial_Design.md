---
tags: clinical trial, trial design
---

# Notes on Group Sequential Trial Design​
<hr>

## Stopping boundaries & alpha spending functions for IA

### Group sequential boundaries 

#### Equally sized stages

- The basic strategy of the GS boundary is to define a critical value at each interim analysis ($Z_c(k), k = 1, 2, \cdots, K$) such that the overall type I error rate will be maintained at a pre-specified level. That is,
  
  $$
  P_{H_0}(|Z_1^*|\ge u_1 \text{ or } \cdots |Z_K^*|\ge u_K) = \alpha
  $$

  is fulfilled.

- Symmetric two-sided boundary is often used but an asymmetric GS procedure can also be implemented 

- For symmetric cases, we reject the null hypothesis if at any of the interim analyses
  
  $$
  |Z(K)|\ge Z_c(K), k = 1, \cdots, K
  $$

  - the test statistic $Z(k)$ uses the cumulative data up to analysis $k$:
- 
    $$
    Z(k) = \{Z^*(1) + \cdots+Z^*(k)\}/\sqrt{k}
    $$

    where $Z^*(k)$ is the test statistic constructed from the $k^{\text{th}}$ group data.

  - If $Z^*(k)\sim N(\Delta, 1)$, then $Z(k)\sim N(\Delta/\sqrt{k}, 1)$. The distribution of $Z(k)/\sqrt{k}$ can be written as a recursive density function, evaluated by numerical integration (*Armitage et al* and *Pocock*). 

    - Using this density function, we can compute the probability of exceeding the critical values at each interim.

      - For Pocock method, $Z_c(k) = Z_p$, a constant, for all $k = 1, 2, \cdots, K$

      - For O’Brien—Fleming method, $Z_c(k) = Z_{OBF}/\sqrt{k}$, where $Z_{OBF}$ is a constant calculated using the recursive density function and iterative interpolation such that the desired type I error rate is achieved under null (same for $Z_p$ above).

      - For Wang and Tsiatis power family, $Z_c(k) = Z_{WT}k^{\Delta-0.5}$, and Pocock and OBF are just special cases of this family of ($\Delta-$class) boundaries.

      - For Peto method, a large critical value such as 3.5 is proposed for each IA and for the last analysis, the usual critical value (e.g. 1.96) is to be used. Since the IA critical values are so conservative, the overall type I error rate will be approximately the same as the last one.

        ![image-20200911141904103](https://raw.githubusercontent.com/askming/picgo/master/image-20200911141904103.png)
    
    - Under null, the sum of these probabilities is the alpha level of the sequential test procedure. Under the alternative, we can calculate the power of the procedure.

#### Unequally sized stages

- Theoretically, the stopping boundaries derived from the equally sized sequential design do not match exactly with those for unequally sized study design although for small number of interim analyses, the difference is minimal and ignorable
- To come up with the "theoretically correct" boudaries for unequally sized stages, the *worst case scenario adjusted critical values* can be used.
- A better, and more complex/sophiscated, procedure to accommodate the irregularity in the timing of IA is to use the alpha-spending function (see below)

##### Sample sizes fixed in advance

- Two modes of calculation of the continuation regions based on Wang and Tsiatis $\Delta-$class for unequally planned stage sizes

    $$
    u_k = c(K, \alpha, \Delta, V)k^{\Delta-0.5}, k = 1, \cdots, K
    $$

   
    $$
    u_k = c(K, \alpha, \Delta, V){\left(t_k\over t_1\right)}^{\Delta-0.5}, k = 1, \cdots, K
    $$

    where $V = (t_1, \cdots, t_K)$ is a vector of preplanned information rates; $c(K, \alpha, \Delta, V)$ is a constant resulting from the chosen design that depends on the max number $K$ of stages, $\alpha$, shape parameter $\Delta$, and the vector $V$.

- An important characteristic of the O’Brien and Fleming design with critical
  values defined by the second mode/version is that they are nearly identical to the respective stage-wise critical values of the five-stage design with equally sized stages.

- The other way round: Inserting an interim analysis has nearly no effect if one uses the critical values according to the second mode/version. This is particularly true for conservative bounds for the first few stages, which is the case for O’Brien and Fleming’s design.

#### Lan-DeMets  $\alpha-$spending function (approximates the group sequential boundaries above, other than the Pocock one)

- Why

  - To be more flexible than the fixed sample size method, i.e. the exact (calendar) time when IAs should be done doesn’t need to be pre-specified. 
  - It provide a test procedure that enables interim analyses at arbitrary time points of analyses. Specifically, this approach can be used if the interim analyses are not scheduled at the observation of a specific number of observations but at fixed calendar times.

- How

  - The $\alpha-$spending function is a way of describing the rate at which the total $\alpha$ is spent as a continuous function of information fraction and thus induces a corresponding boundary

  - Let $t$ be the fraction of information that have been observed at the IA. For comparison of means, $t=n/N$ (number of patients at IA divided by the total target sample size); for survival analysis $t = d/D$ (number of observed events divided by the expected number of events).

    - Let $\alpha(t_k) - \alpha(t_{k-1})$ be the $\alpha$ (or Type I error) that is spent/allocated to the $k$th IA and
  - 
      $$
      P_k = P_{H_0}\left(\bigcap_{\tilde{k}=1}^{k-1}\left\{|Z_{\tilde{k}}|<u_{\tilde{k}}\right\}, |Z_k|\ge u_{k}\right) =\alpha(t_k) - \alpha(t_{k-1}), \text{ for } k = 1, \cdots, K
      $$
      
      and $P_1 + \cdots + P_K = \alpha$
    
      
    
  - Lan DeMets $\alpha-$spending functions that approximate specific group sequential boundaries:
  
    - **O’Brien—Fleming type**: $\alpha(t) = 2 - 2\Phi(Z_{\alpha/2}/\sqrt{t})$
    
    - Pocock type: $\alpha(t) = \alpha\ln(1+(e-1)t)$
  
    - Others: 
    
      - Kim and DeMets: $\alpha(t) = \alpha t^{\theta}$ for $\theta>0$; 
    
      - Hwang et al.: 
       
       $$ \alpha(t) = \left\{
         \begin{array}{ll}  
         \alpha[(1-e^{-\gamma t})/(1-e^{\gamma})] \text{ for } \gamma \ne 0\\ 
         \alpha t_k \text{ for } \gamma =0 
         \end{array}
         \right.
       $$
    
    <img src="https://raw.githubusercontent.com/askming/picgo/master/image-20200911110442997.png" alt="image-20200911110442997" style="zoom:50%;" />
    
    
    
    - Note: *Pocock type of $\alpha-$spending function is not linear in $t$, which means that constant boundaries do not correspond with a linear shape of the $\alpha-$spending function.*
    
      
    
  
- The $\alpha$-spending function approximates the classical OBF group sequential boundaries also for the case of unequally spaced stages. Thus, it is reasonable to use this function to produce boundaries very similar to O’Brien and Fleming’s test. It is also worth mentioning that using the one-sided version of $\alpha$-spending function for the one-sided cases improves this approximation.

- The approximation of the constant boundaries for Pocock’s design when using the $\alpha$-spending function is somewhat worse if unequally spaced information rates are considered. It doesn't always provide constant critical values, especially for $t_k>k/K$, the function even produces slightly increasing boundaries, which are difficult to justify.

  - Indeed, for unequal stage sizes, certain values of $\theta$, for example, $\theta$ = 0.80, for the Kim and DeMets (1987b) $\alpha$-spending function provides a better approximation to Pocock’s constant boundaries. Nevertheless, the approximation for equally sized stages behaves quite well.

## References

- :page_with_curl: DeMets & Lan, *Interim analysis: the alpha spending function approach* 
- :book: [Wassmer, G., & Brannath, W. (2016)](Book-Group%20Sequential%20and%20Confirmatory%20Adaptive%20Designs%20in%20Clinical%20Trials.md). *Group Sequential and Confirmatory Adaptive Designs in Clinical Trials*. Springer.  

