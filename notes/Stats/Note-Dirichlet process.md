# [Dirichlet process](https://en.wikipedia.org/wiki/Dirichlet_process)

## Formal definition
A Dirichlet process over a set $S$ is a *stochastic process* whose sample path (i.e. an infinite-dimensional set of random variates drawn from the process) is a probability distribution on $S$. The finite dimensional distributions are from the [Dirichlet distribution](https://en.wikipedia.org/wiki/Dirichlet_distribution): If $H$ is a finite measure on $S$, $\alpha$ is a positive real number and $X$ is a sample path drawn from a Dirichlet process, written as

$$X \sim \mathrm{DP}\left(\alpha, H\right) $$

then for any measureable [partition](https://en.wikipedia.org/wiki/Partition_of_a_set) of $S$, say $\left\{B_i\right\}_{i=1}^{n}$, we have that
$$\left(X\left(B_1\right),\dots,X\left(B_n\right)\right) \sim \mathrm{Dirichlet}\left(\alpha H\left(B_1\right),\dots, \alpha H\left(B_n\right)\right).$$

## A list of facts
- In probability theory, a Dirichlet process is a random process, that is a probability distribution whose domain is itself a set of probability distributions.

- Given a Dirichlet process $\mathrm{DP}\left(H, \alpha\right)$, where $H$ (the base distribution or base measure) is an arbitrary distribution and $\alpha$ (the concentration parameter) is a positive real number, a draw from $\mathbf{DP}$ will return a random distribution (the output distribution) over some of the values that can be drawn from $H$.

- That is, the support of each draw of the output distribution is always a subset of the support of base distribution. 

- The output distribution will be discrete, meaning that individual values drawn from the output distribution will sometimes repeat themselves even if the base distribution is continuous (i.e., if two different draws from the base distribution will be distinct with probability one).

- The extent to which values will repeat is determined by $\alpha$, with higher values causing less repetition. 

- Note that the Dirichlet process is a stochastic process, meaning that technically speaking it is an infinite sequence of random variables, rather than a single random distribution.

- We can call this the **distribution-centered** view of the Dirichlet process. First, draw a random output distribution from this process, and then consider an infinite sequence of random variables representing values drawn from this distribution. Note that, conditioned on the output distribution, the variables are independent identically distributed. Now, consider instead the distribution of the random variables that results from marginalizing out (integrating over) the random output distribution. (This makes all the variables dependent on each other. However, they are still *exchangeable*, meaning that the marginal distribution of one variable is the same as that of all other variables. That is, they are *"identically distributed" but not "independent"*.) 
- Consider the Dirichlet process as defined above, as a distribution over random distributions, and call this process $\operatorname{DP}_1()$; The resulting infinite sequence of random variables with the given marginal distributions is another view onto the Dirichlet process, denoted here $\operatorname{DP}_2()$.

- The conditional distribution of one variable given all the others, or given all previous variables, is defined by the [Chinese restaurant process] (https://en.wikipedia.org/wiki/Chinese_restaurant_process).

- Another way to think of a Dirichlet process is as an infinite-dimensional generalization of the Dirichlet distribution.
- a Dirichlet distribution can be thought of as a distribution over $K$-dimensional discrete distributions. Imagine generalizing a symmetric Dirichlet distribution, defined by a dimension $K$ and concentration parameter $\alpha/K$, to an infinite set of probabilities; the resulting distribution over infinite-dimensional discrete distributions is called the *stick-breaking process*.
- Imagine then using this set of probabilities to create an infinite-dimensional mixture model, with each separate probability from the set associated with a mixture component, and the value of each component drawn separately from a base distribution $H$; then draw an infinite number of samples from this mixture model. The infinite set of random variables corresponding to the marginal distribution of these samples is a Dirichlet process with parameters $H$ and $\alpha$.

## Application of Dirichlet process
- Dirichlet processes are frequently used in Bayesian nonparametric statistics. "Nonparametric" here does not mean a parameter-less model, rather a model in which representations grow as more data are observed.

- In a Bayesian nonparametric model, the prior and posterior distributions are not parametric distributions, but stochastic processes.[1](https://en.wikipedia.org/wiki/Dirichlet_process#cite_note-2)

- The fact that the Dirichlet distribution is a probability distribution on the simplex of non-negative numbers that sum to one makes it a good candidate to model distributions of distributions or distributions of functions.

- Additionally, the non-parametric nature of this model makes it an ideal candidate for clustering problems where the distinct number of clusters is unknown beforehand.

-As draws from a Dirichlet process are discrete, an important use is as a prior probability in [infinite mixture models](https://en.wikipedia.org/wiki/Infinite_mixture_model).

- In this case, S is the parametric set of component distributions. The generative process is therefore that a sample is drawn from a Dirichlet process, and for each data point in turn a value is drawn from this sample distribution and used as the component distribution for that data point.

- The fact that there is no limit to the number of distinct components which may be generated makes this kind of model appropriate for the case when the number of mixture components is not well-defined in advance. For example, the infinite mixture of Gaussians model.[2](https://en.wikipedia.org/wiki/Dirichlet_process#cite_note-3)

