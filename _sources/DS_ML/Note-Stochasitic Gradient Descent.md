 # Stochastic Gradient Decent (SGD)

**Stochastic gradient descent** is a gradient descent optimization method for minimizing an objective function that is written as a sum of differentiable functions.

Evaluating the sum-gradient may require expensive evaluations of the gradients from all summand functions. When the training set is enormous and no simple formulas exist, evaluating the sums of gradients becomes very expensive, because evaluating the gradient requires evaluating all the summand functions' gradients. To economize on the computational cost at every iteration, stochastic gradient descent samples a *subset* of summand functions at every step. This is very effective in the case of large-scale machine learning problems.

In pseudocode, stochastic gradient descent can be presented as follows:
- Choose an initial vector of parameters w and learning rate alpha .
-  Randomly shuffle examples in the training set.
-  Repeat until an approximate minimum is obtained:

```python
For i = 1, ..., n, do:
	w:= w - alpha \\partial Q_i(w)
```

A compromise between the two forms called "*mini-batches*" computes the gradient against more than one training examples at each step. This can perform significantly better than true stochastic gradient descent because the code can make use of vectorization libraries rather than computing each step separately. It may also result in smoother convergence, as the gradient computed at each step uses more training examples.

