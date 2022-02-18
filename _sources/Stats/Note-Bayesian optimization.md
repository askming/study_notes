# [Exploring Bayesian Optimization](https://distill.pub/2020/bayesian-optimization/)

## Mining gold!

## Active learning

- Procedure for Active Learning

  1. Choose and add the point with the highest uncertainty to the training set (by querying/labeling that point)

  2. Train on the new training set
  3. Go to #1 till convergence or budget elapsed

## Bayesian optimization

- The core question in Bayesian Optimization: “***Based on what we know so far, which point should we evaluate next?***”
- In Bayesian Optimization, we need to balance exploring uncertain regions, which might unexpectedly have high gold content, against focusing on regions we already know have higher gold content (a kind of exploitation).
- Acquisition functions are heuristics for how desirable it is to evaluate a point, based on our present model.
- This brings us to how Bayesian Optimization works. At every step, we determine what the best point to evaluate next is according to the acquisition function by optimizing it. We then update our model and repeat this process to determine the next point to evaluate.
- To solve this problem, we will follow the following algorithm:
  1. We first choose a surrogate model for modeling the true function *f* and define its **prior**.
  2. Given the set of **observations** (function evaluations), use Bayes rule to obtain the **posterior**.
  3. Use an acquisition function $\alpha(x)$, which is a function of the posterior, to decide the next sample point $x_t = \text{argmax}_x \alpha(x)$.
  4. Add newly sampled data to the set of **observations** and goto step #2 till convergence or budget elapses.

### Acquisition Functions

- Acquisition functions are crucial to Bayesian Optimization, and there are a wide variety of options:

  - Probability of improvement (PI)

    $$
    x_{t+1}=argmax(α_{PI} (x))=argmax(P(f(x)≥(f(x^+)+ϵ)))
    $$

    Where, 

    $P(\cdot)$ Indicate probability

    $\epsilon$ is a small positive number 

    And, $x^+ = \text{argmax}_{x_i\in x_{1:t}}f(x_i)$ where $x_i$ is the location queried at $i^{th}$time step.

    - PI uses $\epsilon$ to strike a balance between exploration and exploitation. Increasing $\epsilon$ results in querying locations with a larger $\sigma$ as their probability density is spread.
    - with high exploration, the setting becomes similar to active learning.

  - Expected improvement (EI)

    - The idea is fairly simple — choose the next query point as the one which has the highest expected improvement over the current max $f(x^+)$, where $x^+ = \text{argmax}_{x_i \in x_{1:t}}f(x_i)$ and $x_i$ is the location queried at $i^{th}$ time step.

- summaries of the core ideas associated with acquisition functions: 

  - i) they are heuristics for evaluating the utility of a point; 
  - ii) they are a function of the surrogate posterior; 
  - iii) they combine exploration and exploitation; and 
  - iv) they are inexpensive to evaluate.

## Hyperparameter Tuning

- When training a model is not expensive and time-consuming, we can do a grid search to find the optimum hyperparameters. However, grid search is not feasible if function evaluations are costly, as in the case of a large neural network that takes days to train. Further, grid search scales poorly in terms of the number of hyperparameters.
- We turn to Bayesian Optimization to counter the expensive nature of evaluating our black-box function (accuracy).



# Other readings

- [Bayesian Optimization vs Gradient Descent](https://stats.stackexchange.com/questions/161923/bayesian-optimization-or-gradient-descent/161936#161936)