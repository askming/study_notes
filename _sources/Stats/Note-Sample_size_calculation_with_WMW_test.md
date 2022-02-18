# Sample size calculation for the Wilcoxon-Mann-Whitney test adjusting for ties

Based on {cite:p}`zhao2008sample`

### 1. When to use Wilcoxon-Mann-Whitney (WMW) test [^alias]?

- Non-parametric test for comparing two groups of observations on a **continuous** or **ordered categorical (ordinal)** variable when there is no underlying distributional assumption imposed on the data.

### 2. Detail about WMW

WMW uses the competing probability $\pi = Pr(X > Y) + 0.5Pr(X = Y)$ to quantify the difference between two groups under comparison, where X and Y are random variables with CDF $F_X$ and $F_Y$, respectively. The null hypothesis is

$$H_0: \pi = 0.5$$

An unbiased estimator $\hat{\pi}$ of the competing probability $\pi$ is 

$$\hat{\pi} = (mn)^{-1}\sum_{i=1}^m\sum_{j=1}^n\delta(X_i - Y_j)$$

where $\delta(t) = 1$ if $t> 0$, 0.5 if $t=0$, and 0 if $t<0$.

The variance of $\hat{\pi}$ under the null hypothesis can be calculated as

$$\hat{\sigma}^2_0 = V_0[\hat{\pi}] = \frac{N+1}{12mn} - \frac{1}{12N(N-1)mn}\sum_{c=1}^D(M_c^3 - M_c),$$

where $m$ and $n$ are sample sizes for each group, $N=m+n$ and $M_c$ is the marginal total for a unique group defined by the outcome value. Where there is no ties, it reduces to $(N+1)/(12mn)$.

In WMW test, under null the z-statistic is construct as follows:

$$z_0 = \frac{\hat{\pi} - 0.5}{\hat{\sigma}_0}\sim N(0, 1)$$

In general, e.g. Sigel and Castellan recommended when $m=3$ or 4 and $n>12$; $m>4$ and $n>10$, above normal approximation can be made.

### 3. Sample size formula

Derived from the fact that 

$$\left(\frac{\mu_1 - \mu_0}{\sigma_0}\right)^2 = \left(Z_{\alpha/2} + \frac{\sigma_1}{\sigma_0}Z_{\beta}\right)^2$$ 

where $\mu_0$, $\sigma_0$ and $\mu_1$, $\sigma_1$ are the means and sd of $\hat{\pi}$ under the null and the alternative hypothesis, and $Z_{\alpha/2}$ is the upper $(\alpha/2)$th quantile of the standard normal distribution.

Further assume $\sigma_0 = \sigma_1$ along with the fact that $\mu_0 = 0.5$, the sample size formula simplifies to 

$$\left(\frac{\mu_1 - \mu_0}{\sigma_0}\right)^2 = \left(Z_{\alpha/2} + Z_{\beta}\right)^2$$

The final sample size formula for the WMW test is derived as follows:

$$N = \frac{(Z_{\alpha/2}+Z_{\beta})^2(1-\sum_{c=1}^D((1-t)p_c+tq_c)^3)}{12t(1-t)(\sum_{c=2}^Dp_c\sum_{d=1}^{c-1}q_d + 0.5\sum_{c=1}^Dp_cq_c-0.5)^2}$$

When there is no ties, the sample size formula simplifies to

$$N = \frac{(Z_{\alpha/2}+Z_{\beta})^2}{12t(1-t)(\hat{\mu}_1 -0.5)^2} = \frac{(Z_{\alpha/2}+Z_{\beta})^2}{12t(1-t)(\sum_{c=2}^Dp_c\sum_{d=1}^{c-1}q_d + 0.5\sum_{c=1}^Dp_cq_c-0.5)^2}$$

[^alias]: also called two-sample Wilcoxon rank sum test or Mann-Whitney test

```{bibliography}
:filter: docname in docnames
```