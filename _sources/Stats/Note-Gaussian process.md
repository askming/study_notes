# Gaussain Process

## Multivariate Gaussian distributions

- For a given set of training points, there are potentially infinitely many functions that fit the data. Gaussian processes offer an elegant solution to this problem by assigning a probability to each of these functions

  [[1]](http://www.gaussianprocess.org/gpml/chapters/RW.pdf)

  - The mean of this probability distribution then represents the most probable characterization of the data. 
  - Furthermore, using a probabilistic approach allows us to incorporate the confidence of the prediction into the regression result.

- Gaussian distributions have the nice algebraic property of being closed under *conditioning* and *marginalization*. 

  - Being closed under conditioning and marginalization means that the resulting distributions from these operations are also Gaussian, which makes many problems in statistics and machine learning tractable.

  - Marginalization

    $$
    p_X(x) = \int_yp_{X,Y}(x, y)dy = \int_yp_{X|Y}(x|y)p_Y(y)dy
    $$
    

  - Conditioning
  
    $$
    X∣Y ∼N(μ_X+Σ_{XY}Σ_{YY}^{−1}(Y−μ_Y),Σ_{XX}−Σ_{XY}Σ_{YY}^{−1}Σ_{YX})\\
    Y∣X ∼N(μ_Y+Σ_{YX}Σ_{XX}^{−1}(X−μ_X),Σ_{YY}−Σ_{YX}Σ_{XX}^{−1}Σ_{XY})
    $$
    

    - In conditioning, the new mean only depends on the conditioned variable, while the covariance matrix is **independent** from this variable (constant and equal to the marginal variance/covariance).

## Gaussian Processes

- Stochastic processes, such as Gaussian processes, are essentially a set of random variables. In addition, each of these random variables has a corresponding index *i*. 

- In Gaussian processes we treat each test point as a random variable. A multivariate Gaussian distribution has the same number of dimensions as the number of random variables. Since we want to predict the function values at $∣X∣=N$ test points, the corresponding multivariate Gaussian distribution is also *N*-dimensional. Making a prediction using a Gaussian process ultimately boils down to drawing samples from this distribution. We then interpret the *i*-th component of the resulting vector as the function value corresponding to the i*i*-th test point.

### Kernels

- The clever step of Gaussian processes is how we set up the covariance matrix $\Sigma$. The covariance matrix will not only describe the shape of our distribution, but ultimately determines the characteristics of the function that we want to predict. 

  - We generate the covariance matrix by evaluating the kernel *k*, which is often also called *covariance function*, pairwise on all the points. 

- The kernel receives two points $t,t’ \in \mathbb{R}^n$ as an input and returns a similarity measure between those points in the form of a scalar:
  $$
  k:\mathbb{R}^n \times \mathbb{R}^n, \Sigma = \text{Cov}(X, X') = k(t, t')
  $$

- Since the kernel describes the similarity between the values of our function, it controls the possible shape that a fitted function can adopt. 

- Kernels allow similarity measures that go far beyond the standard euclidean distance (*L*2-distance). Many of these kernels conceptually embed the input points into a higher dimensional space in which they then measure the similarity

- Example of kernels

  - **RBF**: $\sigma^2\exp\left(-{||t-t'||}\over{2l^2}\right)$
  - **Periodic**: $\sigma^2\exp\left(-{2\sin^2(\pi|t-t'|/p)}\over{l^2}\right)$
  - **Linear**: $\sigma_b^2 + \sigma^2(t-c)(t'-c)$

- Kernels can be separated into *stationary* and *non-stationary* kernels. 

  - *Stationary* kernels, such as the RBF kernel or the periodic kernel, are functions invariant to translations, and the covariance of two points is only dependent on their relative position.
  - *Non-stationary* kernels, such as the linear kernel, do not have this constraint and depend on an absolute location.

- A big benefit that kernels provide is that they can be combined together, resulting in a more specialized kernel.
  - The decision which kernel to use is highly dependent on prior knowledge about the data, e.g. if certain characteristics are expected.
  - The most common kernel combinations would be addition and multiplication. And there are more possibilities such as concatenation or composition with a functions 

Qs: 

- didn't understand what is a GP in an intuitive way and hot wo interpret it.
- what does functions from xx  distributions taht are created using different kernels mean?
- each data sample/point is a variable in GP? the dimension is the number of data points?

## References:

[A Visual Exploration of Gaussian Processes](https://distill.pub/2019/visual-exploration-gaussian-processes/)

