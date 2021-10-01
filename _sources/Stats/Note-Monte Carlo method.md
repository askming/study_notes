---
tags: stats, simulation
---

# Monte Carlo Method

- **Monte Carlo** method is the art of approximating an expectation by the sample mean of a function of simulated random variables. Its essence in very familiar terms: Monte Carlo is about invoking laws of large numbers to approximate expectations.[^1]

  

- ***In more mathematical terms***: Consider a (possibly multidimensional) random variable X having probability mass function or probability density function $f_X(x)$ that is greater than zero on a set of values $\mathcal{X}$. Then the expected value of a function $g$ of $X$ is
  
  $$
  \mathbb{E}(g(X)) = \sum_{x\in \mathcal{X}}g(x)f_X(x)
  $$
  
  if $X$ is discrete, and
  
  $$
  \mathbb{E}(g(X)) = \int_{x\in \mathcal{X}}g(x)f_X(x)
  $$

  if $X$ is continuous.

  

  - Now if we were to take an n-sample of $X$'s, $(x_1, \cdots, x_n)$, and we compute the mean of $g(x)$ over the sample, then we would have the Monte Carlo estimate
    
    $$
    \tilde{g}_n(x) = {{1}\over{n}}\sum_{i=1}^n g(x_i)
    $$
    
    of $\mathbb{E}(g(X))$. We could, alternatively, speak of the random variable
    
    $$
    \tilde{g}_n(X) = {{1}\over{n}}\sum_{i=1}^n g(X_i),
    $$
    
    which we call the Monte Carlo estimator of $\mathbb{E}(g(X))$. 

- If $\mathbb{E}(g(X))$, exists, then [*the weak law of large numbers*](https://en.wikipedia.org/wiki/Law_of_large_numbers) tellsus that for any arbitrarily small $\epsilon$
  
  $$
  \lim_{n\rightarrow\infty}P(|\tilde{g}_n(X) - \mathbb{E}(g(X))|\ge \epsilon) = 0
  $$
  
  This tells us that as $n$ gets large, there is small probability that $\tilde{g}_n(x)$ deviates much from $\mathbb{E}(g(X))$.

- One other thing to note. at this point is that $\tilde{g}_n(x)$ is unbiased for $\mathbb{E}(g(X))$:
  
  $$
  \mathbb{E}(\tilde{g}_n(X)) = \mathbb{E} \left( {1\over n}\sum_{i=1}^ng(X_i)\right) = {1\over n}\sum_{i=1}^n\mathbb{E}(g(X_i)) = \mathbb{E}(g(X)).
  $$

## Markov Chain Monte Carlo

- In statistics, **Markov chain Monte Carlo (MCMC)** methods (which include random walk Monte Carlo methods) are a class of algorithms for sampling from probability distributions based on constructing a Markov chain that has the desired distribution as its equilibrium distribution. The state of the chain after a large number of steps is then used as a sample of the desired distribution. The quality of the sample improves as a function of the number of steps.

























[^1]: This applies when the simulated variables are independent of one another, and might apply when they are correlated with one another (for example if they are states visited by an ergodic Markov chain), all of this extends to samples from Markov chains via the weak law of large numbers for the number of passages through a recurrent state in an ergodic Markov chain (see Feller 1957).

