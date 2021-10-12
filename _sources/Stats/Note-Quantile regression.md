# Quantile regression

**Quantile regression** is a type of regression analysis aims at estimating either the conditional median or other quantiles of the response variable, whereas the method of least squares results in estimates that approximate the conditional mean of the response variable given certain values of the predictor variables.

## 1. Advantages and applications

- Quantile regression is desired if conditional quantile functions are of interest.
- One advantage of quantile regression, relative to the ordinary least squares regression, is that the quantile regression estimates are more robust against outliers in the response measurements.
- In practice we often prefer using different measures of central tendency and statistical dispersion to obtain a more comprehensive analysis of the relationship between variables.
- The need for and success of quantile regression in ecology has been attributed to the complexity of interactions between different factors leading to data with unequal variation of one variable for different ranges of another variable.
- The method of least squares leads to a consideration of problems in an inner product space, involving projection onto subspaces, and thus the problem of minimizing the squared errors can be reduced to a problem in numerical linear algebra. Quantile regression does not have this structure,  and instead leads to problems in *linear programming* that can be solved by the simplex method.

## 2. Quantiles

Let $Y$ be real valued random variable with cumulative distribution function $F_Y(y) = P(Y\le y)$. The $\tau$th quantile of $Y$ is given by

$$
Q_Y(\tau) = F^{-1}_Y(\tau) = \inf\{y: F_Y(y)\ge \tau\}
$$

Where $\tau \in [0, 1]$.

- **Loss function**

  Define the loss function as $\rho_{\tau}(y) = |y(\tau - \mathbb{I}_{y<0})|$, where $\mathbb{I}$ is an indicator variable. A specific quantile can be found by minimizing the expected loss of $Y − u$ with respect to $u$:

  
  $$
  \min_{u}E(\rho_{\tau}(Y-u)) = \min_{u}(\tau - 1)\int_{-\infty}^u(y-u)dF_Y(y) + \tau\int_u^{\infty}(y-u)dF_Y(y).
  $$
  This can be shown by setting the derivative of the expected loss function to 0 and letting $q_{\tau}$ be the solution of

  
  $$
  0 = (1-\tau)\int_{-\infty}^{q_{\tau}}dF_Y(y) - \tau\int_{q_{\tau}}^{\infty}dF_Y(y).
  $$
  This equation reduces to

  
  $$
  0 = F_Y(q_{\tau}) - \tau,\\
  F_Y(q_{\tau}) = \tau.
  $$

## 3. Intuition

Consider $\tau = 0.5$ and let $q$ be an initial guess for $q_{\tau}$. The expected loss evaluated at $q$ is

$$
-0.5\int_{-\infty}^q(y-q)dF_Y(y) + 0.5\int_q^{\infty}(y-q)dF_Y(y).
$$

In order to minimize the expected loss, we move the value of $q$ a little bit to see whether the expect loss will rise or fall.

Suppose we increase $q$ by 1 unit. Then the change of expected loss would be

$$
\int_{-\infty}^q1dF_Y(y) + 0.5\int_q^{\infty}1dF_Y(y).
$$

The first term of the equation is $F_Y (q)$ and second term of the equation is $1-F_Y(q)$. Therefore the change of expected loss function is negative if and only if $F_Y (q)<0.5$, that is if and only if $q$ is smaller than the median. Similarly, if we reduce $q$ by 1 unit, the change of expected loss function is negative if and only if $q$ is larger than the median.

- In order to minimize the expected loss function, we would increase (decrease) $L(q)$ if q is smaller (larger) than the median, until q reaches the median. The idea behind the minimization is to count the number of points (weighted with the density) that are larger or smaller than $q$ and then move $q$ to a point where $q$ is larger than $\tau$% of the points.

## 4. Conditional quantile and quantile regression

Suppose the $\tau$th conditional quantile function is $Q_{Y|X}(\tau)=X\beta_{\tau}$, given tghe distribution function of $Y$, $\beta_{\tau}$can be obtained by solving

$$
\beta_{\tau} = \arg\min_{\beta\in R^{k}}E(\rho_{\tau}(Y-X\beta)).
$$

Solving the sample analog gives the estimator of $\beta$:

$$
\hat{\beta}_{\tau} = \arg\min_{\beta\in R^k}\sum_{i=1}^n(\rho_{\tau}(Y_i-X_i\beta)).
$$

- **Computation**

  - The minimization problem can be reformulated as a *linear programming* problem

    
    $$
    \min_{\beta^+, \beta^-, u^-\in R^{2k}\times R_+^{2n}}\left\{\tau I'_nu^+ + (1-\tau)I'_n u^-|X(\beta^+-\beta^-)+u^+-u^- = Y \right\},
    $$

    where

    $$
    \beta_j^+ =\max(\beta_j, 0), \beta^-_j = -\min(\beta_j, 0), u_j^+=\max(u_j, 0), u_j^- = -\min(u_j, 0).
    $$

  - *Simplex methods or interior point methods* can be applied to solve the linear programming problem.

    

- **Asymptotic properties**

  - For $\tau\in(0, 1)$, under some regularity conditions, $\hat{\beta}_{\tau}$ is asymptotically normal:
    $$
    \sqrt{n}(\hat{\beta}_{\tau}-\beta_{\tau}) \overset{d}{\rightarrow} N(0, \tau(1-\tau)D^{-1}\Omega_xD^{-1})
    $$

  - Direct estimation of the asymptotic variance-covariance matrix is not always satisfactory. Inference for quantile regression parameters can be made with the *regression rank-score tests* or with the *bootstrap* methods.

## 5. Bayesian quantile regression

- Because the quantile regression does not normally assume a parametric likelihood for the conditional distributions of $Y|X$, the Bayesian methods work with a working likelihood.

- A convenient choice is the ***asymmetric Laplacian distribution***, because the mode of the resulting posterior under a flat prior is the usual quantile regression estimates.{cite:p}`koenker1978regression`

  - **Asymmetric Laplace distribution (ALD)**, density function is given by:

    
    $$
    f_p(u)=p(1-p)\exp\{-\rho_p(u)\},
    $$

    where $0 < p < 1$ and $\rho_p(u)=u(p-I(u<0))$.

     - When $p=1/2$, the density reduces to 
       
       $$
       f(u)=\frac{1}{4}\exp(-|u|/2),
       $$

       which is the density of a **standard symmetric Laplace distribuiton.**

       

     - For all other values of $p$, the density is asymmetic.

  - Including location and scale parameters, $\mu, \sigma$, in the ALD we obtain 
  
    $$
    f_p(u;\mu,\sigma)=\frac{p(1-p)}{\sigma}\exp\left\{-\rho_p\left(\frac{u-\mu}{\sigma}\right)\right\}.
    $$

- ***Bayesian quantile regression inference***

  When we are interested in the conditional quantile, $q_p(y_i|{\bf x_i})$, rather than the conditonal mean, $E[Y_i|x_i]$, we could still cast the problem in the framework of the generalized linear model, no matter what the original distribution of the data is, by assuming that 

  ​	(i) $f(y;\mu_i)$ is ALD and 

  ​	(ii) $g(\mu_i)={\bf x_i}'\boldsymbol{\beta}(p)=q_p(y_i|{\bf x_i})$, for any $0 < p < 1$.

  

  - Given the observations, ${\bf y}=(y_1,\cdots,y_n)$, the posterior distribution of $\boldsymbol{\beta}, \pi(\boldsymbol{\beta}|{\bf y})$ is given by 
  
    $$
    \pi(\boldsymbol{\beta}|{\bf y})\propto L({\bf y}|\boldsymbol{\beta})p(\boldsymbol{\beta})
    $$

    where $p(\boldsymbol{\beta})$ is the prior distribution of $\boldsymbol{\beta}$ and $L({\bf y}|\boldsymbol{\beta})$ is the likelihood function written as 

    $$
    L({\bf y}|\boldsymbol{\beta})=p^n(1-p)^n\exp\left\{-\sum_{i}\rho_p(y_i-{\bf x_i}'\boldsymbol{\beta})\right\},
    $$
    

    with a location parameter $\mu_i={\bf x_i}'\boldsymbol{\beta}.$

  - In theory, we can use any prior for $\boldsymbol{\beta}$, but in the absence of any realistic information one could use improper uniform prior distribution for all the components of $\boldsymbol{\beta}$.

    

- The posterior inference, however, must be interpreted with care. {cite:p}`yang2012bayesian` showed that one can have asymptotically valid posterior inference if the working likelihood is chosen to be the empirical likelihood.

## 6. Censored quantile regression 

- If the response variable is subject to censoring, the conditional mean is not identifiable without additional distributional assumptions, but the conditional quantile is often identifiable.
- Let $Y^c = \max(0, Y)$ and $Q_{Y|X}= X\beta_{\tau}$, then $Q_{Y^c}=\max(0, X\beta_{\tau})$. This is the censored quantile regression model: estimated values can be obtained without making any distributional assumptions, but at the cost of computational difficulty, some of which can be avoided by using a simple three step censored quantile regression procedure as an approximation.

 

A number of papers that are on the early application of quantile regression:

1. {cite:p}`buchinsky1998recent`
2. {cite:p}`yu1998local`
3. {cite:p}`he1998bivariate`
4. {cite:p}`koenker1999goodness`
5. {cite:p}`yu2001bayesian`

```{bibliography}
:filter: docname in docnames
```